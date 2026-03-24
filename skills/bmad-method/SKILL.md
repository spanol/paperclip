---
name: bmad-method
description: >
  Apply the BMAD (Build More Architect Dreams) agile methodology to structure
  AI-driven development through four phases: Analysis, Planning, Solutioning,
  and Implementation. Use when starting a new project, planning features,
  creating architecture documents, managing sprints, or when any agent needs
  structured development methodology. Provides agent personas, workflow
  templates, artifact formats, and quality checklists. Do NOT use for
  Paperclip coordination (use the paperclip skill) or for company creation
  (use company-creator).
---

# BMAD Method Skill

BMAD (Build More Architect Dreams) is a structured agile methodology for AI-driven development. It organizes work into four sequential phases, each producing artifacts that become context for the next phase.

This skill integrates BMAD into Paperclip's agent orchestration, mapping phases to issues, personas to agents, and workflows to heartbeat-driven execution.

## Core Principles

1. **Context engineering** — each phase produces documents that become context for the next, so agents always know what to build and why.
2. **Scale-adaptive** — adjust depth based on project complexity (Quick Flow, Standard, Enterprise).
3. **Artifact-driven** — every phase produces concrete, reviewable deliverables.
4. **Workflow-aware handoffs** — agents know who gives them work, what they produce, and who they hand off to.

## Planning Tracks

Choose the right track based on project scope:

| Track | Best For | Stories | Artifacts |
|-------|----------|---------|-----------|
| **Quick Flow** | Bug fixes, small features | 1-15 | Tech spec only |
| **Standard** | Products, platforms | 10-50+ | PRD + Architecture + UX |
| **Enterprise** | Compliance, multi-tenant | 30+ | PRD + Architecture + Security + DevOps |

## The Four Phases

### Phase 1 — Analysis (Optional, recommended for Standard/Enterprise)

**Purpose:** Understand the problem space before committing to solutions.

**Activities:**
- Brainstorming and ideation
- Market, domain, and technical research
- Product brief creation
- Existing project documentation

**Artifacts produced:**
- `brainstorming-report.md`
- `product-brief.md`
- Research reports

**Paperclip mapping:** Create issues with prefix `[BMAD-Analysis]` assigned to the Analyst agent. Mark as `todo` with priority `medium`.

### Phase 2 — Planning (Required for all tracks)

**Purpose:** Define what to build with clear requirements and user experience.

**Activities:**
- Create PRD (Product Requirements Document)
- Validate PRD against checklists
- Create UX design specification

**Artifacts produced:**
- `PRD.md` — requirements, user journeys, success metrics, scope
- `ux-spec.md` — wireframes, flows, design decisions

**Paperclip mapping:** Create issues with prefix `[BMAD-Planning]` assigned to the PM agent. These are `critical` priority — nothing proceeds without a validated PRD.

### Phase 3 — Solutioning (Required for Standard/Enterprise)

**Purpose:** Design the technical solution informed by requirements.

**Activities:**
- Create architecture document with ADRs
- Break requirements into epics and stories
- Implementation readiness check (gate: PASS/CONCERNS/FAIL)
- Generate project context document

**Artifacts produced:**
- `architecture.md` — system design, ADRs, tech stack decisions
- `epics/` — epic files containing stories with acceptance criteria
- `project-context.md` — conventions and decisions for implementation agents

**Paperclip mapping:** Create issues with prefix `[BMAD-Solutioning]` assigned to the Architect agent. The readiness check is a `critical` blocker before Phase 4.

### Phase 4 — Implementation (All tracks)

**Purpose:** Build, test, and ship working software.

**Activities:**
- Sprint planning and story prioritization
- Story implementation (TDD approach)
- Code review
- Sprint status tracking
- Retrospectives and course correction

**Artifacts produced:**
- `sprint-status.yaml` — current sprint state
- Working code with tests
- Code review reports

**Paperclip mapping:** Stories become individual issues assigned to Developer agents. QA issues assigned to QA agent. Sprint management by Scrum Master.

## BMAD Personas for Paperclip Agents

BMAD defines 9 specialized personas. When creating a BMAD-driven company in Paperclip, map these to agents:

| BMAD Persona | Paperclip Role | Phase | Key Capabilities |
|-------------|----------------|-------|------------------|
| **Analyst (Mary)** | specialist | 1 | Brainstorming, research, product briefs |
| **PM (John)** | manager | 2 | PRDs, requirements, validation, stakeholder alignment |
| **UX Designer (Sally)** | specialist | 2 | UX specs, wireframes, user flows |
| **Architect (Winston)** | specialist | 3 | Architecture, ADRs, tech stack, readiness checks |
| **Scrum Master (Bob)** | manager | 4 | Sprint planning, story creation, retrospectives |
| **Developer (Amelia)** | specialist | 4 | Story implementation, TDD, code |
| **QA Engineer (Quinn)** | specialist | 4 | Test generation, quality validation |
| **Tech Writer (Paige)** | specialist | 1,4 | Documentation, diagrams, concept explanation |
| **Solo Dev (Barry)** | specialist | Quick Flow | Rapid spec + implementation, minimal ceremony |

See `skills/bmad-method/references/personas.md` for full persona definitions with instructions templates.

## Workflow: How to Run BMAD in Paperclip

### Step 1 — Assess Scale

When a new project or feature is proposed, determine the track:

```
Quick Flow:  < 15 stories, well-understood domain, bug fix or small feature
Standard:    10-50+ stories, new product or platform feature
Enterprise:  30+ stories, compliance/security requirements, multi-tenant
```

Create an initial issue `[BMAD] Assess project scale: {project-name}` assigned to the PM or CEO.

### Step 2 — Create Phase Issues

Based on the track, create parent issues for each phase:

**Standard track example:**
```
[BMAD-Analysis] Research and product brief for {project}     → Analyst
[BMAD-Planning] PRD and UX design for {project}              → PM
[BMAD-Solutioning] Architecture and stories for {project}    → Architect
[BMAD-Implementation] Sprint execution for {project}         → Scrum Master
```

Use Paperclip's issue hierarchy (`parentId`) to nest phase issues under a project epic.

### Step 3 — Execute Phases Sequentially

Each phase agent:
1. Checks out their phase issue
2. Creates sub-issues for individual activities
3. Produces artifacts (committed to repo or posted as comments)
4. Runs validation checklists
5. Marks phase issue as `completed`
6. The next phase agent is woken via assignment

### Step 4 — Gate Checks

Between phases, run validation:

- **After Phase 2:** PRD validation checklist (see references/checklists.md)
- **After Phase 3:** Implementation readiness check — PASS/CONCERNS/FAIL
  - PASS → proceed to Phase 4
  - CONCERNS → Architect addresses, re-runs check
  - FAIL → back to Phase 2 for PRD revision

Gate checks should be issues assigned to the responsible agent with `critical` priority.

### Step 5 — Implementation Sprint Loop

During Phase 4, the Scrum Master:
1. Creates sprint-status.yaml
2. Selects stories for the sprint from the epic
3. Creates individual story issues assigned to Developer agents
4. Developer implements with TDD, creates PR
5. QA creates test issues
6. Code review issues created
7. Sprint retrospective at end of sprint

## Artifact Storage

BMAD artifacts should be stored in the project repository:

```
{project-root}/
  _bmad/
    planning/
      PRD.md
      ux-spec.md
      product-brief.md
      brainstorming-report.md
    architecture/
      architecture.md
      project-context.md
    epics/
      epic-{slug}.md
    sprints/
      sprint-{n}-status.yaml
      retrospective-{n}.md
```

When working within a Paperclip heartbeat, agents should:
- Commit artifacts to the repo in the `_bmad/` directory
- Post a summary comment on the issue linking to the committed files
- Reference artifact paths in handoff comments to next-phase agents

## Using This Skill in a Heartbeat

When you receive a BMAD-tagged issue:

1. **Read the issue description** — it specifies which BMAD phase/activity to execute
2. **Load the relevant reference** — read the appropriate file from `skills/bmad-method/references/`
3. **Check prerequisites** — ensure prior phase artifacts exist (e.g., PRD.md must exist before architecture)
4. **Execute the workflow** — follow the step-by-step process for your activity
5. **Produce artifacts** — commit to repo and comment on issue
6. **Run quality checklist** — validate output against the checklist
7. **Hand off** — update issue status and create/assign the next phase issue if applicable

## References

Detailed definitions and templates:

- **Personas:** `skills/bmad-method/references/personas.md` — full persona definitions with agent instruction templates
- **Phases:** `skills/bmad-method/references/phases.md` — detailed workflows for each phase activity
- **Artifacts:** `skills/bmad-method/references/artifact-templates.md` — templates for PRD, architecture, sprint-status, etc.
- **Checklists:** `skills/bmad-method/references/checklists.md` — quality validation for each phase gate
