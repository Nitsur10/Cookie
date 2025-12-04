# Spec Verifier Skill

Verify specifications are complete, feasible, and ready for implementation.

## Builder Methods

### 1. validate_completeness()
**Checklist**:
- [ ] Mission clearly stated
- [ ] All requirements documented
- [ ] Architecture fully described
- [ ] Tech stack justified
- [ ] Data models defined
- [ ] API endpoints specified
- [ ] Testing strategy included
- [ ] Deployment plan exists

**Output**: `verification/completeness-report.md`

### 2. check_feasibility()
Assess:
- Technical feasibility
- Resource requirements realistic
- Timeline estimates reasonable
- Technology choices proven
- Dependencies available

**Output**: `verification/feasibility-assessment.md`

### 3. verify_consistency()
Check for:
- Internal consistency
- API contracts match data models
- Architecture aligns with requirements
- No contradictions
- Naming conventions uniform

**Output**: `verification/consistency-report.md`

### 4. generate_report()
**Final Report Template**:

```markdown
# Specification Verification Report

## Status: [APPROVED / NEEDS REVISION / REJECTED]

## Executive Summary
The specification is [complete/incomplete] and [feasible/not feasible].

## Completeness: 95/100
✓ All major sections present
⚠️ Missing: Monitoring strategy

## Feasibility: 90/100
✓ Technologies are proven
✓ Timeline is realistic
⚠️ Consider scalability from day 1

## Consistency: 98/100
✓ No contradictions found
✓ Naming consistent

## Issues Found

### Critical (Must Fix)
None

### High Priority
1. Add monitoring strategy

### Medium Priority
1. Consider load balancing plan

## Recommendation
**APPROVED** with minor additions suggested.

Ready to proceed to planning phase.
```

**Output**: `verification/spec-verification.md`

## Success Criteria
- [ ] Completeness verified
- [ ] Feasibility confirmed
- [ ] Consistency checked
- [ ] Report generated
- [ ] Decision made (approve/revise/reject)
