# Frontend Verifier Skill

Verify frontend implementation meets quality and accessibility standards.

## Builder Methods

### 1. test_components()
- Verify all components render correctly
- Test props and state management
- Check for console errors
- Validate TypeScript types

### 2. verify_accessibility()
- Run Lighthouse accessibility audit
- Test keyboard navigation
- Check ARIA labels
- Verify color contrast (WCAG AA)
- Test with screen reader

**Accessibility Tests**:
```typescript
import { axe } from 'jest-axe';

it('should have no accessibility violations', async () => {
  const { container } = render(<Component />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### 3. check_responsiveness()
- Test on mobile (320px-767px)
- Test on tablet (768px-1023px)
- Test on desktop (1024px+)
- Verify no horizontal scrolling
- Check touch targets (≥44x44px)

### 4. validate_interactions()
- Test form submissions
- Verify error messages display
- Check loading states
- Validate navigation
- Test modal/dialog interactions

## Verification Report

```markdown
# Frontend Verification Report

## Status: [PASS/FAIL]

### Component Testing
- Components rendered: ✓
- No console errors: ✓
- TypeScript valid: ✓

### Accessibility (Score: 95/100)
- WCAG AA compliant: ✓
- Keyboard navigation: ✓
- Screen reader: ✓

### Responsiveness
- Mobile (375px): ✓
- Tablet (768px): ✓
- Desktop (1440px): ✓

### Issues
1. [Issue if any]

## Recommendation: [APPROVED / NEEDS FIXES]
```

## Success Criteria
- [ ] All components working
- [ ] Accessibility ≥90/100
- [ ] Responsive on all devices
- [ ] All interactions functional
- [ ] No critical issues
