# Testing Engineer Skill

Create comprehensive test suites for quality assurance.

## Builder Methods

### 1. design_test_strategy()
- Plan testing approach (unit, integration, E2E)
- Choose frameworks (Jest, Vitest, Playwright, Pytest)
- Define coverage goals (≥70%)
- Document testing patterns

**Testing Pyramid**:
```
     E2E (10%)      - Critical user flows
    Integration (20%) - API endpoints, components
   Unit Tests (70%)   - Functions, utilities
```

### 2. implement_tests()

**Unit Test Example**:
```typescript
// services/user.test.ts
import { createUser } from './user';

describe('createUser', () => {
  it('should create user with valid data', async () => {
    const user = await createUser({
      email: 'test@example.com',
      name: 'Test User'
    });

    expect(user).toHaveProperty('id');
    expect(user.email).toBe('test@example.com');
  });

  it('should reject invalid email', async () => {
    await expect(createUser({
      email: 'invalid',
      name: 'Test'
    })).rejects.toThrow('Invalid email');
  });
});
```

**Integration Test Example**:
```typescript
// api/users.test.ts
import request from 'supertest';
import { app } from '../app';

describe('POST /api/users', () => {
  it('should create user', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', name: 'Test' })
      .expect(201);

    expect(response.body.success).toBe(true);
  });
});
```

**E2E Test Example**:
```typescript
// e2e/registration.spec.ts
import { test, expect } from '@playwright/test';

test('user can register', async ({ page }) => {
  await page.goto('/register');
  await page.fill('input[name="email"]', 'test@example.com');
  await page.fill('input[name="password"]', 'SecurePass123');
  await page.click('button[type="submit"]');

  await expect(page).toHaveURL('/dashboard');
});
```

### 3. run_test_suites()
```bash
npm test              # Run all tests
npm run test:unit     # Unit tests only
npm run test:e2e      # E2E tests
npm run test:coverage # Generate coverage report
```

### 4. generate_coverage_reports()
- Analyze code coverage
- Identify untested code
- Set coverage thresholds
- Report quality metrics

**Output**: Test files and coverage reports

## Success Criteria
- [ ] Unit tests ≥70% coverage
- [ ] All API endpoints tested
- [ ] Critical E2E flows tested
- [ ] All tests passing
- [ ] Fast execution (<2min for unit tests)
