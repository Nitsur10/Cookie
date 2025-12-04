# Spec Initializer Skill

This skill helps you initialize new product specifications using builder methods.

## When to Use

Use this skill when:
- Starting a new project from scratch
- User provides requirements or feature ideas
- Need to create initial spec structure
- Beginning the specification phase

## Builder Methods

### 1. gather_requirements()
**Input**: User descriptions, feature requests, problem statements
**Process**:
- Ask clarifying questions about:
  - Target users and their needs
  - Core features and functionality
  - Success criteria
  - Constraints (time, budget, technical)
  - Integration requirements
- Document all requirements clearly
- Identify assumptions explicitly

**Output**: `planning/requirements.md`

**Questions to Ask**:
- What problem does this solve?
- Who are the primary users?
- What are the must-have features?
- What are the nice-to-have features?
- Are there any technical constraints?
- What does success look like?
- What's the timeline?

### 2. validate_inputs()
**Input**: Gathered requirements
**Process**:
- Check for completeness (all key areas covered)
- Identify conflicts or contradictions
- Verify technical feasibility at high level
- Flag any unclear or ambiguous points
- Ensure requirements are measurable

**Output**: `planning/validation-report.md`

**Validation Checklist**:
- [ ] Clear problem statement
- [ ] Defined target users
- [ ] Specific features listed
- [ ] Success criteria measurable
- [ ] No contradictions
- [ ] Technical approach feasible
- [ ] Constraints documented

### 3. create_spec_structure()
**Input**: Validated requirements
**Process**:
```bash
# Create directory structure
agent-os/specs/{date}-{project-name}/
├── README.md
├── planning/
│   ├── initialization.md
│   ├── requirements.md
│   └── validation-report.md
├── reference/
│   ├── architecture.md (placeholder)
│   ├── tech-stack.md (placeholder)
│   └── roadmap.md (placeholder)
├── implementation/
│   └── (for later use)
├── verification/
│   └── (for later use)
└── spec.md (placeholder)
```

**Output**: Complete directory structure

### 4. initialize_documentation()
**Input**: Created structure
**Process**:
- Create README with project overview
- Write initialization.md with setup details
- Document requirements clearly
- Add placeholders for future sections
- Link to next steps

**Output**: Initial documentation set

**README Template**:
```markdown
# {Project Name}

## Overview
[Brief description]

## Status
- Phase: Specification
- Stage: Initialized
- Date: {date}

## Next Steps
1. Technical research (spec-researcher)
2. Write complete specification (spec-writer)
3. Verification (spec-verifier)
```

## Usage Example

```
User: "I want to build a task management app"

Agent executes:
1. gather_requirements()
   - Asks about users, features, integrations
   - Documents requirements

2. validate_inputs()
   - Checks completeness
   - Identifies any gaps

3. create_spec_structure()
   - Creates directory: agent-os/specs/2025-12-05-task-manager/
   - Sets up all folders

4. initialize_documentation()
   - Creates README, initialization.md
   - Documents requirements
```

## Tools Available

- **Read**: Read existing files for context
- **Write**: Create new documentation files
- **Bash**: Create directory structures
- **Glob/Grep**: Search for similar projects (if any)

## Output Location

All outputs go to: `agent-os/specs/{date}-{project-name}/`

## Success Criteria

- [ ] Clear requirements documented
- [ ] Validation complete
- [ ] Directory structure created
- [ ] Initial documentation written
- [ ] Ready for research phase

## Next Agent

Hand off to **spec-researcher** with the initialized specification structure.
