# BMAD Artifact Templates

Templates for artifacts produced in each BMAD phase. Agents should use these as starting points and adapt to the specific project.

---

## Product Brief Template

**File:** `_bmad/planning/product-brief.md`
**Phase:** 1 — Analysis
**Produced by:** Analyst

```markdown
# Product Brief: {Product Name}

## Opportunity
{1-2 paragraphs describing the market opportunity or problem}

## Target Users
| Persona | Description | Key Pain Points |
|---------|-------------|-----------------|
| {name}  | {who they are} | {what frustrates them} |

## Value Proposition
{Clear statement of what we offer and why it matters}

## Key Assumptions
1. {Assumption that must be true for this to work}
2. ...

## Risks and Unknowns
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| {risk} | High/Med/Low | High/Med/Low | {strategy} |

## Recommended Track
- [ ] Quick Flow (< 15 stories, well-understood)
- [ ] Standard (10-50+ stories, new product)
- [ ] Enterprise (30+ stories, compliance needs)

## Next Steps
{What should the PM focus on first}
```

---

## PRD Template

**File:** `_bmad/planning/PRD.md`
**Phase:** 2 — Planning
**Produced by:** PM

```markdown
# PRD: {Product Name}

## 1. Vision
{Concise product vision statement — what world does this create?}

## 2. Success Metrics
| Metric | Baseline | Target | Timeframe |
|--------|----------|--------|-----------|
| {KPI}  | {current} | {goal} | {when}   |

## 3. User Journeys

### Journey: {Name}
**Persona:** {who}
**Goal:** {what they want to accomplish}

| Step | Action | System Response | Emotion |
|------|--------|-----------------|---------|
| 1    | {what user does} | {what system does} | {how user feels} |

## 4. Domain Analysis

### Entities
| Entity | Description | Key Attributes |
|--------|-------------|----------------|
| {name} | {what it is} | {important fields} |

### Business Rules
1. {Rule that governs domain behavior}
2. ...

## 5. Scope

### In Scope
- {Feature or capability included}

### Out of Scope
- {Explicitly excluded item and why}

## 6. Functional Requirements

### Epic: {Epic Name}

| ID | Requirement | Priority | Acceptance Criteria |
|----|-------------|----------|-------------------|
| FR-001 | {description} | Must-have | {testable criteria} |
| FR-002 | {description} | Should-have | {testable criteria} |

## 7. Non-Functional Requirements

| ID | Category | Requirement | Target |
|----|----------|-------------|--------|
| NFR-001 | Performance | {requirement} | {measurable target} |
| NFR-002 | Security | {requirement} | {standard/benchmark} |
| NFR-003 | Accessibility | {requirement} | {WCAG level} |

## 8. Constraints
- **Technical:** {constraint}
- **Business:** {constraint}
- **Regulatory:** {constraint}
- **Timeline:** {constraint}

## 9. Assumptions and Dependencies
| Type | Description | Risk if Wrong |
|------|-------------|---------------|
| Assumption | {what we assume} | {what happens if wrong} |
| Dependency | {what we depend on} | {impact if unavailable} |

## 10. Revision History
| Date | Author | Changes |
|------|--------|---------|
| {date} | {who} | {what changed} |
```

---

## Architecture Document Template

**File:** `_bmad/architecture/architecture.md`
**Phase:** 3 — Solutioning
**Produced by:** Architect

```markdown
# Architecture: {Product Name}

## 1. System Overview

{High-level description of the system architecture}

### System Diagram
{Mermaid diagram or textual description of components and their relationships}

## 2. Tech Stack

| Layer | Technology | Justification |
|-------|-----------|---------------|
| Frontend | {tech} | {why} |
| Backend | {tech} | {why} |
| Database | {tech} | {why} |
| Infrastructure | {tech} | {why} |

## 3. Component Architecture

### {Component Name}
- **Purpose:** {what it does}
- **Interfaces:** {what it exposes}
- **Dependencies:** {what it needs}
- **Key decisions:** See ADR-{N}

## 4. Data Model

### Entities
{Entity-relationship description or Mermaid ER diagram}

### Storage Strategy
- **Primary store:** {database type and rationale}
- **Caching:** {strategy if applicable}
- **File storage:** {strategy if applicable}

## 5. API Design

### {Endpoint Group}
| Method | Path | Purpose | Auth |
|--------|------|---------|------|
| GET | /api/{resource} | {description} | {auth type} |
| POST | /api/{resource} | {description} | {auth type} |

## 6. Infrastructure

- **Deployment:** {strategy}
- **Scaling:** {approach}
- **Monitoring:** {tools and metrics}
- **CI/CD:** {pipeline description}

## 7. Security

- **Authentication:** {approach}
- **Authorization:** {model}
- **Data protection:** {encryption, PII handling}
- **Secrets management:** {approach}

## 8. Architecture Decision Records

### ADR-001: {Title}
- **Status:** Accepted
- **Context:** {Why this decision was needed}
- **Decision:** {What was decided}
- **Consequences:** {Trade-offs and implications}

### ADR-002: {Title}
...
```

---

## Project Context Template

**File:** `_bmad/architecture/project-context.md`
**Phase:** 3 — Solutioning
**Produced by:** Architect

```markdown
# Project Context: {Product Name}

This document captures conventions and decisions for implementation agents.
Read this before writing any code.

## Code Conventions
- **Language:** {language and version}
- **Style guide:** {reference}
- **Naming:** {conventions for files, functions, variables}
- **Directory structure:** {key directories and their purpose}

## Git Conventions
- **Branch naming:** {pattern}
- **Commit messages:** {format}
- **PR process:** {workflow}

## Testing Conventions
- **Unit tests:** {framework, location, naming}
- **Integration tests:** {framework, location}
- **E2E tests:** {framework, location}
- **Coverage target:** {percentage}

## Key Decisions Summary
{Quick-reference list of ADRs from architecture.md that affect daily coding}

1. ADR-001: {title} — {one-line impact on coding}
2. ADR-002: {title} — {one-line impact on coding}

## Environment Setup
{Steps to get a development environment running}

## Common Patterns
{Recurring patterns in this codebase that developers should follow}
```

---

## Epic Template

**File:** `_bmad/epics/epic-{slug}.md`
**Phase:** 3 — Solutioning
**Produced by:** Architect or Scrum Master

```markdown
# Epic: {Epic Name}

**Priority:** {Must-have / Should-have / Nice-to-have}
**Estimated stories:** {count}
**Total story points:** {sum}
**Dependencies:** {other epics this depends on}

## Description
{What this epic delivers and why it matters}

## Stories

### Story 1: {Title}
- **Points:** {1/2/3/5/8/13}
- **Priority:** {P0/P1/P2}
- **Dependencies:** {story IDs or "none"}
- **Acceptance Criteria:**
  - Given {context}, When {action}, Then {outcome}
  - Given {context}, When {action}, Then {outcome}
- **Technical Notes:** {references to architecture decisions, APIs, components}

### Story 2: {Title}
...

## Implementation Order
1. Story {N} — {rationale for going first}
2. Story {M} — {depends on Story N}
3. ...
```

---

## Sprint Status Template

**File:** `_bmad/sprints/sprint-{n}-status.yaml`
**Phase:** 4 — Implementation
**Produced by:** Scrum Master

```yaml
sprint:
  number: 1
  goal: "{What this sprint delivers}"
  startDate: "YYYY-MM-DD"
  endDate: "YYYY-MM-DD"
  status: active  # active | completed | cancelled

stories:
  - id: "STORY-001"
    title: "{Story title}"
    assignee: "{agent-name}"
    points: 3
    status: done        # todo | in_progress | in_review | done | blocked | carried_over
    issueId: "{paperclip-issue-id}"

  - id: "STORY-002"
    title: "{Story title}"
    assignee: "{agent-name}"
    points: 5
    status: in_progress
    issueId: "{paperclip-issue-id}"
    blockers:
      - "{description of blocker}"

metrics:
  planned_points: 21
  completed_points: 8
  velocity: null  # calculated after sprint completes
  carried_over: 0
```

---

## Sprint Retrospective Template

**File:** `_bmad/sprints/retrospective-{n}.md`
**Phase:** 4 — Implementation
**Produced by:** Scrum Master

```markdown
# Sprint {N} Retrospective

**Sprint goal:** {goal}
**Status:** {met / partially met / not met}

## Metrics
- **Planned:** {N} points across {M} stories
- **Completed:** {N} points across {M} stories
- **Carried over:** {N} stories ({M} points)
- **Velocity:** {points/sprint}
- **Blockers encountered:** {count}

## What Went Well
1. {positive outcome}
2. {positive outcome}

## What Could Improve
1. {area for improvement}
2. {area for improvement}

## Action Items
| Action | Owner | Due |
|--------|-------|-----|
| {what to do} | {who} | {when} |

## Notes
{Additional observations or decisions}
```

---

## Quick Flow Tech Spec Template

**File:** `_bmad/planning/spec-{slug}.md`
**Phase:** Quick Flow
**Produced by:** Solo Dev

```markdown
# Tech Spec: {Feature/Fix Name}

## Problem
{What's wrong or what's needed — 1-2 sentences}

## Solution
{How to fix/build it — be specific about approach}

## Changes
| File | Change | Reason |
|------|--------|--------|
| {path} | {what changes} | {why} |

## Testing
- {How to verify the change works}

## Risks
- {Anything that could go wrong}
```
