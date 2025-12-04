# Implementation Verifier Skill

Perform final certification that implementation is complete and production-ready.

## Builder Methods

### 1. verify_against_spec()
- Compare implementation to specification
- Check all requirements met
- Verify feature completeness
- Validate contracts honored

**Verification Matrix**:
```markdown
| Requirement | Status | Notes |
|------------|--------|-------|
| User authentication | ✓ | JWT implemented |
| Task creation | ✓ | API + UI complete |
| Real-time updates | ⚠️ | Polling, not WebSocket |
| Mobile responsive | ✓ | All breakpoints tested |
```

### 2. check_integration()
- Test full system end-to-end
- Verify frontend-backend integration
- Check database connectivity
- Test third-party integrations

**E2E Integration Test**:
```typescript
test('Complete user journey', async ({ page }) => {
  // Register
  await page.goto('/register');
  await page.fill('input[name="email"]', 'user@test.com');
  await page.click('button[type="submit"]');

  // Create resource
  await page.goto('/dashboard');
  await page.click('button:has-text("New")');
  await page.fill('input[name="title"]', 'Test Item');
  await page.click('button:has-text("Save")');

  // Verify in UI and DB
  await expect(page.locator('.item')).toContainText('Test Item');

  const dbItem = await db.item.findFirst({ where: { title: 'Test Item' }});
  expect(dbItem).toBeDefined();
});
```

### 3. validate_deployment()
- Check production build succeeds
- Verify environment variables configured
- Test database migrations
- Validate deployment scripts
- Ensure rollback plan exists

**Deployment Checklist**:
- [ ] Production build successful
- [ ] Environment variables documented
- [ ] Database migrations tested
- [ ] CI/CD pipeline configured
- [ ] Monitoring set up
- [ ] Logging configured
- [ ] Backup strategy defined
- [ ] Rollback procedure documented

### 4. certify_completion()
- Review all verification reports
- Compile final assessment
- List known issues/limitations
- Provide recommendations
- Assign final score

## Final Certification Report

```markdown
# Implementation Certification Report

## Overall Status: [CERTIFIED / CONDITIONAL / NOT CERTIFIED]

## Scores
- Specification Compliance: 95/100
- Frontend Quality: 92/100
- Backend Quality: 94/100
- Security: 98/100
- Performance: 90/100
- Documentation: 88/100

**Overall Score: 93/100**

## Summary
The implementation meets all core requirements and is ready for production deployment.

## Verification Results

### ✓ Passed
- All features implemented
- Security verified
- Performance acceptable
- Tests passing
- Documentation complete

### ⚠️ Known Limitations
1. Real-time updates use polling (not WebSocket)
2. Basic analytics (can be enhanced)

### Recommendations
1. Consider WebSocket upgrade for Phase 2
2. Add advanced analytics dashboard
3. Implement caching layer for better performance

## Certification

**Status**: CERTIFIED ✓

**Decision**: Ready for production deployment

**Conditions**: None (fully approved)

## Next Steps
1. Deploy to production
2. Monitor metrics closely
3. Gather user feedback
4. Plan Phase 2 enhancements
```

## Certification Criteria

### CERTIFIED (Score ≥85)
- No critical issues
- All core features working
- Security verified
- Performance acceptable
- Documentation complete

### CONDITIONAL (Score 70-84)
- Minor issues present
- Core features working
- Must address specific conditions
- Production viable with monitoring

### NOT CERTIFIED (Score <70)
- Critical issues present
- Core features missing/broken
- Security vulnerabilities
- Not ready for production

## Success Criteria
- [ ] Spec compliance ≥85%
- [ ] All verifications passed
- [ ] Integration working
- [ ] Deployment ready
- [ ] Documentation complete
- [ ] No critical issues

## Final Output

Certification report with:
- Overall score and status
- Detailed findings
- Known issues
- Recommendations
- Go/no-go decision
