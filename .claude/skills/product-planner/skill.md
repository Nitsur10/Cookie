# Product Planner Skill

Plan product features, create roadmap, and coordinate implementation phases.

## Builder Methods

### 1. analyze_requirements()
- Review verified specification
- Understand business priorities
- Identify feature dependencies
- Assess risks and constraints

### 2. prioritize_features()
**MoSCoW Method**:
- **Must Have**: Core MVP features
- **Should Have**: Important but not critical
- **Could Have**: Nice-to-have features
- **Won't Have**: Out of scope

**Example**:
```markdown
## MVP (Must Have)
- User authentication
- Task CRUD operations
- Basic dashboard

## Phase 2 (Should Have)
- Task assignment
- Real-time updates
- Email notifications

## Phase 3 (Could Have)
- Advanced analytics
- Mobile app
- Integrations

## Out of Scope (Won't Have)
- AI recommendations
- Video chat
```

### 3. create_roadmap()
```markdown
# Product Roadmap

## Phase 0: Infrastructure (Week 1)
- Database schema
- API foundation
- Authentication setup

## Phase 1: MVP (Weeks 2-4)
- Core features
- Basic UI
- Testing

## Phase 2: Enhancement (Weeks 5-6)
- Additional features
- Polish
- Performance optimization

## Phase 3: Scale (Future)
- Advanced features
- Integrations
```

### 4. allocate_resources()
**Output**: `planning/task-assignments.yml`

```yaml
phases:
  phase_0_infrastructure:
    tasks:
      - task: "Design database schema"
        agent: "database-engineer"
        dependencies: []
        priority: "high"

      - task: "Set up API structure"
        agent: "api-engineer"
        dependencies: ["database-schema"]
        priority: "high"

  phase_1_mvp:
    tasks:
      - task: "Build authentication"
        agent: "api-engineer"
        dependencies: ["api-structure"]

      - task: "Create UI components"
        agent: "ui-designer"
        dependencies: ["api-structure"]
```

## Success Criteria
- [ ] Features prioritized
- [ ] Roadmap created
- [ ] Dependencies mapped
- [ ] Resources allocated
- [ ] Timeline realistic
