# API Engineer Skill

This skill helps you design and implement RESTful APIs and backend services.

## When to Use

Use this skill when:
- Building API endpoints
- Implementing business logic
- Creating backend services
- Need request/response handling

## Builder Methods

### 1. design_endpoints()
**Input**: API specification from spec
**Process**:
- Define all REST routes
- Plan request/response formats
- Design error responses
- Document authentication needs
- Plan rate limiting
- Consider versioning

**Endpoint Design Template**:
```markdown
## POST /api/users

### Description
Create a new user

### Authentication
Required (JWT)

### Request Body
```json
{
  "email": "user@example.com",
  "name": "John Doe",
  "password": "SecurePass123"
}
```

### Response (201 Created)
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

### Errors
- 400: Validation error
- 409: Email already exists
- 500: Server error
```

**Output**: `api/api-design.md`

### 2. implement_routes()
**Input**: Endpoint designs
**Process**:
- Create route handlers
- Implement business logic
- Add database operations
- Handle errors properly
- Return consistent responses

**Implementation Patterns**:

**Express.js**:
```typescript
// routes/users.ts
import express from 'express';
import { createUser, getUser } from '../controllers/users';
import { authenticate } from '../middleware/auth';
import { validateCreateUser } from '../middleware/validation';

const router = express.Router();

router.post('/users',
  authenticate,
  validateCreateUser,
  createUser
);

router.get('/users/:id',
  authenticate,
  getUser
);

export default router;
```

**Next.js App Router**:
```typescript
// app/api/users/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { z } from 'zod';

const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(2)
});

export async function POST(request: NextRequest) {
  try {
    const body = await request.json();
    const validated = CreateUserSchema.parse(body);

    const user = await db.user.create({
      data: validated
    });

    return NextResponse.json({
      success: true,
      data: user
    }, { status: 201 });
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json({
        success: false,
        error: {
          code: 'VALIDATION_ERROR',
          message: error.errors[0].message
        }
      }, { status: 400 });
    }

    return NextResponse.json({
      success: false,
      error: {
        code: 'INTERNAL_ERROR',
        message: 'Failed to create user'
      }
    }, { status: 500 });
  }
}
```

**FastAPI (Python)**:
```python
# routes/users.py
from fastapi import APIRouter, Depends, HTTPException
from pydantic import BaseModel, EmailStr

router = APIRouter()

class CreateUserRequest(BaseModel):
    email: EmailStr
    name: str

@router.post("/users", status_code=201)
async def create_user(
    user: CreateUserRequest,
    current_user = Depends(get_current_user)
):
    try:
        new_user = await db.user.create(user.dict())
        return {"success": True, "data": new_user}
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
```

**Output**: API implementation files

### 3. add_validation()
**Input**: Route implementations
**Process**:
- Create validation schemas (Zod, Joi, Pydantic)
- Validate all inputs
- Sanitize data
- Check business rules
- Return clear error messages

**Validation Examples**:

**Zod (TypeScript)**:
```typescript
import { z } from 'zod';

export const CreateUserSchema = z.object({
  email: z.string().email('Invalid email format'),
  name: z.string().min(2, 'Name must be at least 2 characters'),
  age: z.number().int().min(0).max(150).optional(),
  role: z.enum(['user', 'admin']).default('user')
});

// Middleware
export function validateCreateUser(req, res, next) {
  try {
    req.body = CreateUserSchema.parse(req.body);
    next();
  } catch (error) {
    return res.status(400).json({
      success: false,
      error: {
        code: 'VALIDATION_ERROR',
        details: error.errors
      }
    });
  }
}
```

**Pydantic (Python)**:
```python
from pydantic import BaseModel, EmailStr, validator

class CreateUserRequest(BaseModel):
    email: EmailStr
    name: str
    age: int | None = None

    @validator('name')
    def name_must_be_valid(cls, v):
        if len(v) < 2:
            raise ValueError('Name must be at least 2 characters')
        return v

    @validator('age')
    def age_must_be_valid(cls, v):
        if v is not None and (v < 0 or v > 150):
            raise ValueError('Age must be between 0 and 150')
        return v
```

**Output**: Validation schemas and middleware

### 4. create_tests()
**Input**: Implemented routes
**Process**:
- Write unit tests for business logic
- Create integration tests for endpoints
- Test error scenarios
- Test authentication/authorization
- Document test coverage

**Test Examples**:

**Jest + Supertest**:
```typescript
import request from 'supertest';
import { app } from '../app';

describe('POST /api/users', () => {
  it('should create user with valid data', async () => {
    const response = await request(app)
      .post('/api/users')
      .set('Authorization', `Bearer ${token}`)
      .send({
        email: 'test@example.com',
        name: 'Test User'
      })
      .expect(201);

    expect(response.body.success).toBe(true);
    expect(response.body.data).toHaveProperty('id');
  });

  it('should reject invalid email', async () => {
    const response = await request(app)
      .post('/api/users')
      .set('Authorization', `Bearer ${token}`)
      .send({
        email: 'invalid',
        name: 'Test'
      })
      .expect(400);

    expect(response.body.success).toBe(false);
  });

  it('should require authentication', async () => {
    await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', name: 'Test' })
      .expect(401);
  });
});
```

**Output**: `tests/api/*.test.ts`

## Common Patterns

### Authentication Middleware
```typescript
import jwt from 'jsonwebtoken';

export async function authenticate(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({
      success: false,
      error: { code: 'NO_TOKEN', message: 'Authentication required' }
    });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(401).json({
      success: false,
      error: { code: 'INVALID_TOKEN', message: 'Invalid token' }
    });
  }
}
```

### Error Handler
```typescript
export function errorHandler(err, req, res, next) {
  console.error('Error:', err);

  if (err instanceof ValidationError) {
    return res.status(400).json({
      success: false,
      error: {
        code: 'VALIDATION_ERROR',
        message: err.message
      }
    });
  }

  if (err instanceof NotFoundError) {
    return res.status(404).json({
      success: false,
      error: {
        code: 'NOT_FOUND',
        message: err.message
      }
    });
  }

  res.status(500).json({
    success: false,
    error: {
      code: 'INTERNAL_ERROR',
      message: 'An unexpected error occurred'
    }
  });
}
```

### Async Handler Wrapper
```typescript
export const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Usage
router.get('/users/:id', asyncHandler(async (req, res) => {
  const user = await getUser(req.params.id);
  res.json({ success: true, data: user });
}));
```

## API Response Standards

### Success Response
```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "timestamp": "2025-12-05T00:00:00Z",
    "page": 1,
    "total": 100
  }
}
```

### Error Response
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "field": "email",
    "details": { ... }
  }
}
```

## Tools Available

- **Read**: Read spec and requirements
- **Write**: Create API files
- **Bash**: Run tests, start dev server
- **Grep**: Search for patterns

## Success Criteria

- [ ] All endpoints implemented
- [ ] Validation working
- [ ] Error handling complete
- [ ] Authentication enforced
- [ ] Tests passing
- [ ] Code follows standards
- [ ] Documentation updated

## Next Step

Hand off to **backend-verifier** for API quality verification.
