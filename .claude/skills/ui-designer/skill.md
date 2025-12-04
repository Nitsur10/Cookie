# UI Designer Skill

Design and implement user interfaces with accessibility and responsiveness.

## Builder Methods

### 1. create_mockups()
- Design UI layouts and component hierarchy
- Plan responsive breakpoints (mobile, tablet, desktop)
- Define color scheme and typography
- Sketch component structure

**Output**: Design documentation with component tree

### 2. design_components()
- Build reusable React/Vue components
- Implement design system (buttons, forms, cards)
- Use TypeScript for prop types
- Follow atomic design (atoms → molecules → organisms)

**Example Component**:
```typescript
// components/Button.tsx
interface ButtonProps {
  children: React.ReactNode;
  variant?: 'primary' | 'secondary';
  onClick?: () => void;
  disabled?: boolean;
}

export function Button({ children, variant = 'primary', onClick, disabled }: ButtonProps) {
  return (
    <button
      className={`btn btn-${variant}`}
      onClick={onClick}
      disabled={disabled}
      aria-label={typeof children === 'string' ? children : undefined}
    >
      {children}
    </button>
  );
}
```

### 3. ensure_accessibility()
- Add ARIA labels and roles
- Ensure keyboard navigation
- Check color contrast (WCAG AA: 4.5:1)
- Test with screen readers
- Add focus indicators

**Accessibility Checklist**:
- [ ] Semantic HTML (header, nav, main, section)
- [ ] Alt text for images
- [ ] Form labels linked to inputs
- [ ] Keyboard accessible
- [ ] Focus visible
- [ ] Color contrast compliant

### 4. prototype_interactions()
- Implement user interactions (clicks, hovers, inputs)
- Add loading states
- Handle error states
- Create smooth transitions
- Test user flows

**Output**: Complete, accessible, responsive UI components

## Tools Available
- **Read**: Read design specs
- **Write**: Create component files
- **WebSearch**: Research UI patterns and component libraries

## Success Criteria
- [ ] All components built
- [ ] Responsive on all devices
- [ ] Accessible (WCAG AA)
- [ ] Interactive and smooth
- [ ] Design system consistent
