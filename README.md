# Cookie Demo - Agent OS Builder Pattern

This project uses **Agent OS** with builder method patterns for systematic software development.

## What is Agent OS?

Agent OS is an AI agent orchestration system that uses specialized agents with builder methods to systematically develop software from specification to deployment.

### Builder Method Pattern

Each agent uses builder methods to break down complex tasks into sequential, manageable steps:

```
requirements = gather_requirements()
validation = validate_inputs(requirements)
structure = create_spec_structure(validation)
docs = initialize_documentation(structure)
return initialized_spec
```

## Quick Start

### 1. Create a New Specification

```bash
# Start the specification workflow
/new-spec
```

This initializes a new product spec with the `spec-initializer` agent.

### 2. Complete Specification

```bash
# Run the full specification workflow
/create-spec
```

This orchestrates:
- spec-initializer → spec-researcher → spec-writer → spec-verifier

### 3. Plan Implementation

```bash
# Create implementation plan
/plan-product
```

This runs:
- product-planner → tasks-list-creator

### 4. Implement

```bash
# Execute implementation workflow
/implement-spec
```

This coordinates:
- database-engineer → api-engineer → ui-designer → testing-engineer → verifiers

## Agent Architecture

### Specification Agents

- **spec-initializer**: Creates initial spec structure
- **spec-researcher**: Researches technical solutions
- **spec-writer**: Writes complete specifications
- **spec-verifier**: Verifies spec quality

### Planning Agents

- **product-planner**: Creates roadmap and prioritizes features
- **tasks-list-creator**: Breaks specs into actionable tasks

### Implementation Agents

- **database-engineer**: Designs schemas and migrations
- **api-engineer**: Builds REST APIs and endpoints
- **ui-designer**: Creates UI components
- **testing-engineer**: Writes automated tests

### Verification Agents

- **frontend-verifier**: Verifies UI quality
- **backend-verifier**: Verifies API quality
- **implementation-verifier**: Final certification

## Builder Methods by Agent

### spec-initializer
1. `gather_requirements()` - Collect user requirements
2. `validate_inputs()` - Verify requirements are clear
3. `create_spec_structure()` - Set up directory structure
4. `initialize_documentation()` - Create initial docs

### database-engineer
1. `design_schema()` - Create data models
2. `create_migrations()` - Write migration scripts
3. `optimize_queries()` - Add indexes and optimize
4. `validate_integrity()` - Test constraints

### api-engineer
1. `design_endpoints()` - Plan API routes
2. `implement_routes()` - Build endpoint handlers
3. `add_validation()` - Add input validation
4. `create_tests()` - Write API tests

### ui-designer
1. `create_mockups()` - Design UI layouts
2. `design_components()` - Build components
3. `ensure_accessibility()` - Add ARIA and a11y
4. `prototype_interactions()` - Implement interactions

## Directory Structure

```
.
├── .claude/
│   ├── agents/agent-os/       # Agent definitions
│   └── commands/agent-os/     # Slash commands
├── agent-os/
│   ├── config.yml            # Agent OS configuration
│   ├── specs/                # Product specifications
│   ├── standards/            # Coding standards
│   │   ├── global/          # Global standards
│   │   ├── backend/         # Backend standards
│   │   ├── frontend/        # Frontend standards
│   │   └── testing/         # Testing standards
│   └── implementations/      # Implementation files
└── README.md
```

## Standards

All agents follow these standards:

### Global Standards
- `coding-style.md` - Code formatting and naming conventions
- `error-handling.md` - Error handling patterns
- `conventions.md` - General conventions
- `validation.md` - Input validation patterns

### Backend Standards
- `api.md` - REST API design
- `models.md` - Data model conventions
- `migrations.md` - Database migration patterns
- `queries.md` - Query optimization

### Frontend Standards
- `components.md` - Component patterns
- `accessibility.md` - WCAG 2.1 guidelines
- `css.md` - Styling conventions
- `responsive.md` - Responsive design

### Testing Standards
- `test-writing.md` - Test patterns and coverage

## Workflow Example

```bash
# 1. Create specification
/new-spec
# Agent asks questions, creates spec structure

# 2. Research and write spec
/create-spec
# Agents research, write, and verify specification

# 3. Plan implementation
/plan-product
# Agents create roadmap and task breakdown

# 4. Implement
/implement-spec
# Agents build database, API, UI, tests

# 5. Verify and deploy
# implementation-verifier certifies completion
```

## Configuration

Edit `agent-os/config.yml` to customize:

- Agent definitions and builder methods
- Workflow stages and sequences
- Output directories
- Standards to follow

## Benefits of Agent OS

1. **Systematic**: Follows proven development workflows
2. **Quality**: Built-in verification at each stage
3. **Consistent**: Standards enforced across all code
4. **Traceable**: Clear documentation of decisions
5. **Maintainable**: Well-structured, tested code

## Next Steps

1. Run `/new-spec` to create your first specification
2. Review the generated spec structure
3. Proceed through the workflow stages
4. Customize `config.yml` for your needs
5. Add project-specific standards

## Support

- View agent definitions in `.claude/agents/agent-os/`
- Check standards in `agent-os/standards/`
- Review config in `agent-os/config.yml`

---

Built with Agent OS using builder method patterns for systematic software development.
