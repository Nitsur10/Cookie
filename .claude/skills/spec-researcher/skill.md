# Spec Researcher Skill

This skill helps you research technical solutions and patterns for specifications.

## When to Use

Use this skill when:
- Initialized spec structure exists
- Need to research technical approaches
- Evaluating technology options
- Understanding best practices

## Builder Methods

### 1. analyze_requirements()
**Input**: Initialized specification
**Process**:
- Review requirements deeply
- Identify technical challenges
- List unknowns that need research
- Categorize research needs:
  - Architecture patterns
  - Technology choices
  - Integration approaches
  - Data modeling
  - Security considerations

**Output**: `planning/research-scope.md`

### 2. research_solutions()
**Input**: Research scope
**Process**:
- **Use WebSearch** for latest technologies and patterns
- Research framework options and comparisons
- Study similar implementations
- Investigate best practices
- Explore integration possibilities
- Review documentation and tutorials

**Research Areas**:
- **Frontend**: React, Vue, Svelte, Next.js, etc.
- **Backend**: Node.js, Python, Go, etc.
- **Database**: PostgreSQL, MongoDB, Supabase, etc.
- **APIs**: REST, GraphQL, tRPC
- **Authentication**: JWT, OAuth, Auth0, Clerk
- **Deployment**: Vercel, AWS, Docker
- **Tools**: Testing, CI/CD, monitoring

**Output**: `planning/research-findings.md`

### 3. evaluate_options()
**Input**: Research findings
**Process**:
- Compare different approaches
- List pros and cons for each option
- Consider:
  - Learning curve
  - Community support
  - Performance
  - Scalability
  - Cost
  - Developer experience
  - Long-term maintenance

**Evaluation Matrix**:
```markdown
## Option: {Technology/Pattern}

### Pros
- Advantage 1
- Advantage 2

### Cons
- Disadvantage 1
- Disadvantage 2

### Best For
- Use case 1
- Use case 2

### Recommendation
[Recommend / Not Recommended] because...
```

**Output**: `planning/options-analysis.md`

### 4. document_findings()
**Input**: Evaluated options
**Process**:
- Compile comprehensive documentation
- Make clear recommendations
- Provide rationale for choices
- Include reference links
- Add code examples where helpful
- Document trade-offs

**Output**:
- `reference/tech-stack.md` - Recommended technologies
- `reference/architecture.md` - Architectural approach

## Usage Example

```
Input: Task management app specification

1. analyze_requirements()
   Research needs:
   - Real-time updates
   - User authentication
   - Task assignment
   - Mobile responsive

2. research_solutions()
   Findings:
   - Next.js + React for frontend
   - Supabase for backend/database
   - WebSockets for real-time
   - Clerk for authentication

3. evaluate_options()
   Compared:
   - Next.js vs Remix
   - Supabase vs Firebase
   - Clerk vs Auth0

4. document_findings()
   Recommended stack documented with rationale
```

## Research Tools

### Use WebSearch for:
```
- "Next.js vs Remix 2025 comparison"
- "Best database for task management app"
- "Real-time updates React patterns"
- "Modern authentication solutions"
- "Supabase review and use cases"
```

### Use WebFetch for:
- Official documentation
- GitHub repositories
- Technical blog posts
- Tutorial sites

## Documentation Templates

### Tech Stack Template
```markdown
# Technology Stack

## Frontend
**Choice**: Next.js 14 + React 18
**Rationale**:
- Server components for performance
- Built-in routing
- Great developer experience
- Strong community

**Alternatives Considered**:
- Remix: Good, but less mature
- Vite + React: More setup required

## Backend
[Similar structure]

## Database
[Similar structure]
```

### Architecture Template
```markdown
# System Architecture

## Overview
[High-level description]

## Architecture Pattern
**Pattern**: {Monolith/Microservices/Serverless}
**Rationale**: [Why this pattern]

## Components
1. Frontend Layer
2. API Layer
3. Database Layer
4. Authentication

## Data Flow
[Describe how data flows through system]

## Diagrams
[Include mermaid diagrams if helpful]
```

## Success Criteria

- [ ] All technical areas researched
- [ ] Options evaluated objectively
- [ ] Clear recommendations made
- [ ] Rationale provided for choices
- [ ] Reference links included
- [ ] Architecture documented
- [ ] Ready for spec writing

## Next Agent

Hand off to **spec-writer** with complete research package including:
- Research findings
- Options analysis
- Tech stack recommendations
- Architecture approach
