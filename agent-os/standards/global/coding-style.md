# Global Coding Style Standards

## General Principles

1. **Readability First**: Code is read more than written
2. **Consistency**: Follow established patterns
3. **Simplicity**: Prefer simple solutions over clever ones
4. **Explicit > Implicit**: Make intentions clear

## Naming Conventions

### Variables and Functions
```typescript
// Use camelCase for variables and functions
const userName = 'John';
function getUserById(id: string) { }

// Use descriptive names
const isEnabled = true;  // Good
const flag = true;       // Bad

// Boolean variables start with is/has/should
const isActive = true;
const hasPermission = false;
const shouldRefresh = true;
```

### Classes and Types
```typescript
// Use PascalCase for classes and types
class UserService { }
interface UserData { }
type UserId = string;
```

### Constants
```typescript
// Use UPPER_SNAKE_CASE for constants
const MAX_RETRY_ATTEMPTS = 3;
const API_BASE_URL = 'https://api.example.com';
```

### File Names
- React components: `PascalCase.tsx` (e.g., `UserProfile.tsx`)
- Utilities: `camelCase.ts` (e.g., `formatDate.ts`)
- Types: `camelCase.types.ts` (e.g., `user.types.ts`)
- Tests: `*.test.ts` or `*.spec.ts`

## Code Organization

### File Structure
```typescript
// 1. Imports (grouped and sorted)
import { useState, useEffect } from 'react';      // External
import { Button } from '@/components/ui';        // Internal
import { formatDate } from '@/utils';            // Utils
import type { User } from '@/types';             // Types

// 2. Types and Interfaces
interface ComponentProps {
  user: User;
  onUpdate: (user: User) => void;
}

// 3. Constants
const DEFAULT_TIMEOUT = 5000;

// 4. Component/Function
export function Component({ user, onUpdate }: ComponentProps) {
  // Implementation
}

// 5. Helper functions (if any)
function helperFunction() { }
```

### Function Length
- Keep functions under 50 lines
- Extract complex logic into smaller functions
- One responsibility per function

## Formatting

### Indentation
- Use 2 spaces (not tabs)
- Consistent indentation for all files

### Line Length
- Max 100 characters per line
- Break long lines logically

### Spacing
```typescript
// Space after keywords
if (condition) { }
for (const item of items) { }

// Space around operators
const sum = a + b;
const isValid = x > 0 && y < 10;

// No space for function calls
getValue();

// Space for function definitions
function getValue() { }
```

### Braces
```typescript
// Always use braces, even for single-line blocks
if (condition) {
  doSomething();
}

// Not this
if (condition) doSomething();
```

## Comments

### When to Comment
```typescript
// Good: Explain WHY, not WHAT
// Using exponential backoff to avoid overwhelming the API
const delay = Math.pow(2, attempt) * 1000;

// Bad: Stating the obvious
// Increment counter
counter++;
```

### Types of Comments
```typescript
// Single-line comment for brief explanations

/**
 * Multi-line comment for functions and classes
 *
 * @param userId - The unique identifier for the user
 * @param options - Configuration options
 * @returns The user object or null if not found
 */
function getUser(userId: string, options?: GetUserOptions): User | null {
  // Implementation
}

// TODO: Add pagination support
// FIXME: Handle edge case when user is deleted
// NOTE: This temporary fix should be removed after API update
```

## TypeScript Specific

### Type Annotations
```typescript
// Prefer type inference when obvious
const count = 0;              // TypeScript infers number
const items = [];             // Infer from usage

// Explicit types for function parameters and returns
function calculateTotal(items: Item[]): number {
  return items.reduce((sum, item) => sum + item.price, 0);
}

// Explicit types for complex objects
const config: DatabaseConfig = {
  host: 'localhost',
  port: 5432
};
```

### Avoid `any`
```typescript
// Bad
function process(data: any) { }

// Good
function process(data: unknown) {
  if (typeof data === 'string') {
    // data is string here
  }
}

// Better: Use specific types
function process(data: ProcessInput) { }
```

## Error Handling

```typescript
// Use try-catch for expected errors
try {
  const data = await fetchData();
  return data;
} catch (error) {
  if (error instanceof NetworkError) {
    return handleNetworkError(error);
  }
  throw error; // Re-throw unexpected errors
}

// Validate inputs early
function divide(a: number, b: number): number {
  if (b === 0) {
    throw new Error('Division by zero');
  }
  return a / b;
}
```

## Best Practices

### DRY (Don't Repeat Yourself)
```typescript
// Bad
const user1 = { name: 'John', age: 30 };
const user2 = { name: 'Jane', age: 25 };

// Good
function createUser(name: string, age: number) {
  return { name, age };
}
```

### Early Returns
```typescript
// Good: Early return for edge cases
function processUser(user: User | null) {
  if (!user) return null;
  if (!user.isActive) return null;

  // Main logic here
  return transformUser(user);
}

// Avoid deep nesting
```

### Destructuring
```typescript
// Use destructuring for cleaner code
const { name, email } = user;
const [first, second] = array;
```

### Optional Chaining
```typescript
// Use optional chaining for nested properties
const city = user?.address?.city;

// With nullish coalescing
const displayName = user?.name ?? 'Anonymous';
```

## Code Quality Tools

- **ESLint**: Enforce coding standards
- **Prettier**: Automatic formatting
- **TypeScript**: Type checking
- **Husky**: Pre-commit hooks

## Summary Checklist

- [ ] Descriptive variable names
- [ ] Functions under 50 lines
- [ ] Consistent formatting (2 spaces)
- [ ] TypeScript types defined
- [ ] Error handling present
- [ ] Comments explain WHY
- [ ] No code duplication
- [ ] Early returns used
- [ ] Tests included
