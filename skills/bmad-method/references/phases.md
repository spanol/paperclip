# BMAD Phases — Detailed Workflows

Step-by-step workflows for each BMAD phase activity. Agents should follow these when executing BMAD-tagged issues.

---

## Phase 1 — Analysis

### 1.1 Brainstorming

**Assigned to:** Analyst
**Input:** Problem statement or project idea
**Output:** `_bmad/planning/brainstorming-report.md`

Steps:
1. Read the problem statement from the issue description
2. Generate ideas using multiple techniques:
   - Mind mapping — branch from core concept
   - SCAMPER — Substitute, Combine, Adapt, Modify, Put to other uses, Eliminate, Reverse
   - Reverse brainstorming — how to make the problem worse, then invert
   - Constraint removal — what would you build with no limits?
3. Categorize ideas by theme
4. Score ideas by feasibility (1-5) and impact (1-5)
5. Select top 5-10 ideas with rationale
6. Write brainstorming-report.md and commit to `_bmad/planning/`

### 1.2 Research (Market / Domain / Technical)

**Assigned to:** Analyst
**Input:** Product brief or brainstorming report
**Output:** Research report committed to `_bmad/planning/`

Steps:
1. Define research questions from the input artifact
2. For market research: analyze competitors, market size, trends, gaps
3. For domain research: map domain concepts, rules, constraints, terminology
4. For technical research: evaluate frameworks, APIs, infrastructure options
5. Synthesize findings with recommendations
6. Commit report and comment on issue with key findings

### 1.3 Product Brief

**Assigned to:** Analyst
**Input:** Brainstorming and/or research reports
**Output:** `_bmad/planning/product-brief.md`

Steps:
1. Summarize the opportunity (1-2 paragraphs)
2. Define target users and their pain points
3. State the value proposition
4. List key assumptions to validate
5. Identify risks and unknowns
6. Recommend planning track (Quick Flow / Standard / Enterprise)
7. Commit product-brief.md

---

## Phase 2 — Planning

### 2.1 Create PRD

**Assigned to:** PM
**Input:** Product brief (Phase 1) or direct user requirements
**Output:** `_bmad/planning/PRD.md`

This is a 12-step workflow. Execute each step, gathering user input via issue comments when needed.

1. **Initialize** — Load product brief and any research artifacts
2. **Discovery** — Identify stakeholders, users, and their needs
3. **Vision** — Write a concise product vision statement
4. **Success metrics** — Define measurable KPIs (SMART format)
5. **User journeys** — Map 3-5 primary user journeys with steps and emotions
6. **Domain analysis** — Identify domain entities, relationships, and business rules
7. **Scope definition** — Define in-scope and explicitly out-of-scope items
8. **Functional requirements** — List features grouped by epic, each with:
   - ID (FR-001 format)
   - Description
   - Priority (must-have / should-have / nice-to-have)
   - Acceptance criteria
9. **Non-functional requirements** — Performance, security, scalability, accessibility
10. **Constraints** — Technical, business, regulatory, timeline
11. **Assumptions and dependencies** — What must be true for this to work
12. **Compile PRD** — Assemble all sections using the template from `references/artifact-templates.md`

### 2.2 Validate PRD

**Assigned to:** PM
**Input:** PRD.md
**Output:** Validation report (comment on issue)

Run the PRD validation checklist from `references/checklists.md`. Address each item:

1. Vision clarity — can a new team member understand what we're building?
2. User journey completeness — all primary paths covered?
3. Requirements traceability — every FR maps to a user need?
4. Acceptance criteria — every FR has testable criteria?
5. Scope boundaries — clear in/out decisions?
6. NFR specificity — quantified targets, not vague aspirations?
7. Dependency identification — all external dependencies listed?
8. Risk assessment — top 5 risks with mitigations?
9. Priority alignment — must-haves are truly must-haves?
10. Consistency — no contradictions between sections?
11. Feasibility — requirements are technically achievable?
12. Measurability — success metrics have baselines and targets?
13. Completeness — no obvious gaps in the solution space?

Result: PASS (proceed) / CONCERNS (list items to address) / FAIL (major revision needed)

### 2.3 UX Design

**Assigned to:** UX Designer
**Input:** Validated PRD
**Output:** `_bmad/planning/ux-spec.md`

Steps:
1. Extract user personas from PRD
2. Map information architecture
3. Define navigation model
4. Create wireframe descriptions for key screens
5. Define interaction patterns
6. Specify responsive breakpoints
7. Document accessibility requirements
8. Define design tokens (colors, typography, spacing)
9. Create component inventory
10. Map user flows for primary journeys
11. Define error states and empty states
12. Document loading and transition patterns
13. Specify form validation rules
14. Compile UX spec

---

## Phase 3 — Solutioning

### 3.1 Create Architecture

**Assigned to:** Architect
**Input:** Validated PRD + UX Spec
**Output:** `_bmad/architecture/architecture.md`

Steps:
1. **System overview** — high-level diagram (describe in markdown/mermaid)
2. **Tech stack selection** — justify each choice with ADR
3. **Component architecture** — define services, modules, layers
4. **Data model** — entities, relationships, storage strategy
5. **API design** — endpoints, contracts, authentication
6. **Infrastructure** — deployment, scaling, monitoring
7. **Security architecture** — auth, authorization, data protection
8. **Architecture Decision Records** — document each significant decision:
   - Title, Status (proposed/accepted/deprecated)
   - Context, Decision, Consequences

### 3.2 Create Epics and Stories

**Assigned to:** Architect or Scrum Master
**Input:** Architecture + PRD
**Output:** `_bmad/epics/epic-{slug}.md` files

Steps:
1. Group functional requirements into epics (3-8 stories each)
2. For each story, define:
   - Title and description
   - Acceptance criteria (Given/When/Then format)
   - Technical notes referencing architecture decisions
   - Story points estimate (1/2/3/5/8/13)
   - Dependencies on other stories
3. Order stories within each epic by dependency and priority
4. Write epic files with stories listed in implementation order
5. Create a story dependency graph if cross-epic dependencies exist

### 3.3 Implementation Readiness Check

**Assigned to:** Architect
**Input:** All Phase 2 + Phase 3 artifacts
**Output:** Readiness verdict (issue comment)

Evaluate against the readiness checklist from `references/checklists.md`:

- Architecture covers all functional requirements?
- Data model supports all domain entities?
- API contracts defined for all integrations?
- No conflicting architecture decisions?
- Stories have clear acceptance criteria?
- Dependencies mapped and feasible?
- Security requirements addressed?
- Performance targets have architectural support?

Result: **PASS** / **CONCERNS** (list items) / **FAIL** (back to Phase 2)

---

## Phase 4 — Implementation

### 4.1 Sprint Planning

**Assigned to:** Scrum Master
**Input:** Epics with stories
**Output:** `_bmad/sprints/sprint-{n}-status.yaml`

Steps:
1. Review available stories from epics
2. Select stories for sprint based on:
   - Priority (must-haves first)
   - Dependencies (prerequisites first)
   - Capacity (team size and availability)
3. Create sprint-status.yaml with selected stories
4. Create individual Paperclip issues for each story, assigned to developers
5. Set sprint goal

### 4.2 Story Implementation

**Assigned to:** Developer
**Input:** Story issue with acceptance criteria + architecture.md + project-context.md
**Output:** Working code with tests

Steps:
1. Read story acceptance criteria
2. Read relevant architecture sections
3. Read project-context.md for conventions
4. Plan implementation approach (comment on issue)
5. Write failing tests for acceptance criteria (TDD)
6. Implement code to pass tests
7. Run full test suite
8. Self-review against code quality checklist
9. Commit with descriptive message referencing story ID
10. Update issue status to `in_review`

### 4.3 Code Review

**Assigned to:** Developer (peer) or QA
**Input:** Implementation branch/commit
**Output:** Review comments on issue

Review against:
- Acceptance criteria met?
- Tests cover edge cases?
- Code follows project conventions (project-context.md)?
- No security vulnerabilities introduced?
- Performance acceptable?
- Documentation updated if needed?

### 4.4 Sprint Retrospective

**Assigned to:** Scrum Master
**Input:** Completed sprint
**Output:** `_bmad/sprints/retrospective-{n}.md`

Steps:
1. Gather data: stories completed, stories carried over, blockers encountered
2. What went well?
3. What could improve?
4. Action items for next sprint
5. Update velocity metrics
6. Commit retrospective document

---

## Quick Flow (Parallel Track)

For Quick Flow projects, skip Phases 1-3. The Solo Dev agent handles everything:

1. Read the issue/request
2. Create a brief tech spec (inline in issue or as `_bmad/planning/spec-{slug}.md`)
3. Implement with tests
4. Self-review
5. Done

Quick Flow is appropriate when:
- The change is well-understood
- No architectural decisions needed
- Scope is < 15 stories
- Single developer can handle it end-to-end
