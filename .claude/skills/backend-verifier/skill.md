# Backend Verifier Skill

Verify backend implementation meets security, performance, and quality standards.

## Builder Methods

### 1. test_endpoints()
- Test all API endpoints
- Verify request/response formats
- Check status codes
- Validate error handling

**Endpoint Test Example**:
```typescript
describe('GET /api/users/:id', () => {
  it('should return user', async () => {
    const response = await request(app)
      .get('/api/users/123')
      .set('Authorization', `Bearer ${token}`)
      .expect(200);

    expect(response.body.data).toHaveProperty('id');
  });

  it('should require authentication', async () => {
    await request(app)
      .get('/api/users/123')
      .expect(401);
  });
});
```

### 2. verify_security()
**OWASP Top 10 Checks**:
- [ ] SQL injection prevented
- [ ] XSS protection enabled
- [ ] Authentication required
- [ ] Authorization enforced
- [ ] Input validation working
- [ ] Secrets not exposed
- [ ] HTTPS enforced
- [ ] Rate limiting configured

**Security Tests**:
```typescript
it('should prevent SQL injection', async () => {
  const malicious = "'; DROP TABLE users; --";
  await request(app)
    .get(`/api/users?name=${malicious}`)
    .expect(200); // Should not execute SQL
});

it('should sanitize XSS', async () => {
  const xss = '<script>alert("XSS")</script>';
  const response = await request(app)
    .post('/api/users')
    .send({ name: xss });

  expect(response.body.data.name).not.toContain('<script>');
});
```

### 3. check_performance()
- Average response time <200ms
- P95 response time <500ms
- Database queries optimized
- No N+1 query issues
- Caching implemented

**Load Test**:
```bash
# Using Artillery
artillery quick --count 100 --num 10 http://localhost:3000/api/users
```

### 4. validate_data_integrity()
- Test database constraints
- Verify foreign keys work
- Check unique constraints
- Validate data types
- Test transactions

## Verification Report

```markdown
# Backend Verification Report

## Status: [PASS/FAIL]

### API Testing
- Endpoints tested: 15/15 ✓
- Status codes correct: ✓
- Error handling: ✓

### Security (OWASP Top 10)
- All checks passed: ✓
- No critical vulnerabilities: ✓

### Performance
- Avg response time: 120ms ✓
- P95 response time: 280ms ✓
- Throughput: 500 req/sec ✓

### Data Integrity
- Constraints working: ✓
- Foreign keys valid: ✓

## Recommendation: [APPROVED / NEEDS FIXES]
```

## Success Criteria
- [ ] All endpoints working
- [ ] No security vulnerabilities
- [ ] Performance targets met
- [ ] Data integrity enforced
- [ ] Tests passing ≥70% coverage
