# Error Handling Standards

## Principles

1. **Fail Fast**: Detect errors early
2. **Be Specific**: Use specific error types
3. **Provide Context**: Include helpful error messages
4. **Handle Gracefully**: Don't crash, recover when possible
5. **Log Appropriately**: Log errors for debugging

## Error Types

### Custom Error Classes

```typescript
// Base application error
export class AppError extends Error {
  constructor(
    message: string,
    public code: string,
    public statusCode: number = 500,
    public isOperational: boolean = true
  ) {
    super(message);
    this.name = this.constructor.name;
    Error.captureStackTrace(this, this.constructor);
  }
}

// Specific error types
export class ValidationError extends AppError {
  constructor(message: string, public field?: string) {
    super(message, 'VALIDATION_ERROR', 400);
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string, id: string) {
    super(`${resource} with id ${id} not found`, 'NOT_FOUND', 404);
  }
}

export class UnauthorizedError extends AppError {
  constructor(message: string = 'Unauthorized') {
    super(message, 'UNAUTHORIZED', 401);
  }
}

export class ForbiddenError extends AppError {
  constructor(message: string = 'Forbidden') {
    super(message, 'FORBIDDEN', 403);
  }
}
```

## Backend Error Handling

### API Error Handler Middleware

```typescript
// Express error handler
export function errorHandler(
  err: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  // Log error
  console.error('Error:', {
    message: err.message,
    stack: err.stack,
    path: req.path,
    method: req.method
  });

  // Handle known errors
  if (err instanceof AppError) {
    return res.status(err.statusCode).json({
      success: false,
      error: {
        code: err.code,
        message: err.message,
        ...(err instanceof ValidationError && { field: err.field })
      }
    });
  }

  // Handle unknown errors
  res.status(500).json({
    success: false,
    error: {
      code: 'INTERNAL_SERVER_ERROR',
      message: 'An unexpected error occurred'
    }
  });
}
```

### Async Error Wrapper

```typescript
// Wrapper to catch async errors
export function asyncHandler(
  fn: (req: Request, res: Response, next: NextFunction) => Promise<any>
) {
  return (req: Request, res: Response, next: NextFunction) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };
}

// Usage
router.get('/users/:id', asyncHandler(async (req, res) => {
  const user = await getUserById(req.params.id);
  if (!user) {
    throw new NotFoundError('User', req.params.id);
  }
  res.json({ success: true, data: user });
}));
```

### Input Validation

```typescript
import { z } from 'zod';

const CreateUserSchema = z.object({
  email: z.string().email('Invalid email format'),
  name: z.string().min(2, 'Name must be at least 2 characters'),
  age: z.number().int().min(0).max(150).optional()
});

export function validateCreateUser(req: Request, res: Response, next: NextFunction) {
  try {
    req.body = CreateUserSchema.parse(req.body);
    next();
  } catch (error) {
    if (error instanceof z.ZodError) {
      const firstError = error.errors[0];
      throw new ValidationError(
        firstError.message,
        firstError.path.join('.')
      );
    }
    throw error;
  }
}
```

## Frontend Error Handling

### React Error Boundary

```typescript
import { Component, ReactNode } from 'react';

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
}

interface State {
  hasError: boolean;
  error?: Error;
}

export class ErrorBoundary extends Component<Props, State> {
  state: State = { hasError: false };

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
    // Log to error tracking service
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || (
        <div className="error-container">
          <h2>Something went wrong</h2>
          <p>{this.state.error?.message}</p>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}
```

### API Error Handling

```typescript
// API client with error handling
export async function apiRequest<T>(
  url: string,
  options?: RequestInit
): Promise<T> {
  try {
    const response = await fetch(url, options);

    if (!response.ok) {
      const errorData = await response.json().catch(() => ({}));
      throw new ApiError(
        errorData.error?.message || 'Request failed',
        response.status,
        errorData.error?.code
      );
    }

    const data = await response.json();
    return data.data;
  } catch (error) {
    if (error instanceof ApiError) {
      throw error;
    }
    if (error instanceof TypeError) {
      throw new ApiError('Network error', 0, 'NETWORK_ERROR');
    }
    throw new ApiError('Unknown error', 500, 'UNKNOWN_ERROR');
  }
}

// Usage with React Query
function useUser(id: string) {
  return useQuery({
    queryKey: ['user', id],
    queryFn: () => apiRequest<User>(`/api/users/${id}`),
    onError: (error: ApiError) => {
      if (error.status === 404) {
        toast.error('User not found');
      } else if (error.status === 401) {
        router.push('/login');
      } else {
        toast.error('Failed to load user');
      }
    }
  });
}
```

### Form Error Handling

```typescript
import { useForm } from 'react-hook-form';

function UserForm() {
  const {
    register,
    handleSubmit,
    setError,
    formState: { errors }
  } = useForm<FormData>();

  const onSubmit = async (data: FormData) => {
    try {
      await createUser(data);
      toast.success('User created');
    } catch (error) {
      if (error instanceof ValidationError) {
        if (error.field) {
          setError(error.field as any, {
            message: error.message
          });
        } else {
          toast.error(error.message);
        }
      } else {
        toast.error('Failed to create user');
      }
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register('email')} />
      {errors.email && (
        <span className="error">{errors.email.message}</span>
      )}
      {/* ... */}
    </form>
  );
}
```

## Database Error Handling

```typescript
// Prisma error handling
import { Prisma } from '@prisma/client';

export async function createUser(data: CreateUserInput) {
  try {
    return await db.user.create({ data });
  } catch (error) {
    if (error instanceof Prisma.PrismaClientKnownRequestError) {
      // Unique constraint violation
      if (error.code === 'P2002') {
        throw new ValidationError(
          'Email already exists',
          'email'
        );
      }
      // Foreign key constraint violation
      if (error.code === 'P2003') {
        throw new ValidationError('Invalid reference');
      }
    }
    throw error;
  }
}
```

## Logging

```typescript
// Structured logging
export const logger = {
  error: (message: string, context?: Record<string, any>) => {
    console.error(JSON.stringify({
      level: 'error',
      message,
      timestamp: new Date().toISOString(),
      ...context
    }));
  },

  warn: (message: string, context?: Record<string, any>) => {
    console.warn(JSON.stringify({
      level: 'warn',
      message,
      timestamp: new Date().toISOString(),
      ...context
    }));
  },

  info: (message: string, context?: Record<string, any>) => {
    console.info(JSON.stringify({
      level: 'info',
      message,
      timestamp: new Date().toISOString(),
      ...context
    }));
  }
};

// Usage
try {
  await processPayment(orderId);
} catch (error) {
  logger.error('Payment processing failed', {
    orderId,
    error: error.message,
    stack: error.stack
  });
  throw error;
}
```

## Error Response Format

### Standard API Error Response

```typescript
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "field": "email",           // Optional: for field-specific errors
    "details": {                // Optional: additional context
      "expected": "email",
      "received": "invalid"
    }
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "requestId": "req_abc123"
  }
}
```

## Best Practices Checklist

- [ ] Use specific error types
- [ ] Provide helpful error messages
- [ ] Include context (field, resource, etc.)
- [ ] Log errors appropriately
- [ ] Don't expose sensitive information
- [ ] Use error boundaries in React
- [ ] Handle async errors
- [ ] Validate inputs early
- [ ] Return consistent error format
- [ ] Set appropriate HTTP status codes
- [ ] Clean up resources in finally blocks
- [ ] Test error scenarios
