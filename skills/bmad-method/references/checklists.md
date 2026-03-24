# BMAD Quality Checklists

Validation checklists for phase gates and quality assurance. Agents must run the relevant checklist before marking a phase as complete.

---

## PRD Validation Checklist (Phase 2 Gate)

Run after creating PRD.md. All items must pass for a PASS verdict.

| # | Check | Pass Criteria | Status |
|---|-------|---------------|--------|
| 1 | **Vision clarity** | A new team member can read the vision and explain what we're building in their own words | |
| 2 | **User journey completeness** | All primary user paths are mapped with steps, system responses, and emotional states | |
| 3 | **Requirements traceability** | Every functional requirement maps to at least one user need or journey step | |
| 4 | **Acceptance criteria** | Every FR has at least one testable acceptance criterion (Given/When/Then or equivalent) | |
| 5 | **Scope boundaries** | In-scope and out-of-scope are explicitly defined with rationale for exclusions | |
| 6 | **NFR specificity** | Non-functional requirements have quantified targets, not vague aspirations (e.g., "< 200ms p95" not "fast") | |
| 7 | **Dependency identification** | All external dependencies (APIs, services, teams) are listed with risk assessment | |
| 8 | **Risk assessment** | Top 5+ risks identified with likelihood, impact, and mitigation strategy | |
| 9 | **Priority alignment** | Must-haves are truly essential for launch; should-haves won't block release | |
| 10 | **Internal consistency** | No contradictions between sections (e.g., scope says X is out but requirements include X) | |
| 11 | **Technical feasibility** | Requirements are achievable with proposed tech stack and timeline | |
| 12 | **Success measurability** | Every success metric has a baseline, target, and measurement method | |
| 13 | **Completeness** | No obvious gaps — someone could build this product from this document alone | |

**Verdict:**
- **PASS** — All 13 checks pass. Proceed to Phase 3.
- **CONCERNS** — 1-3 checks have issues. List them, address, and re-validate.
- **FAIL** — 4+ checks fail or critical gaps found. Major PRD revision needed.

---

## Implementation Readiness Checklist (Phase 3 Gate)

Run after architecture.md, epics, and project-context.md are complete.

| # | Check | Pass Criteria | Status |
|---|-------|---------------|--------|
| 1 | **Architecture coverage** | Every functional requirement has a clear architectural home (component, service, or module) | |
| 2 | **Data model completeness** | All domain entities from the PRD are represented in the data model | |
| 3 | **API contracts** | All integration points have defined contracts (endpoints, payloads, auth) | |
| 4 | **ADR consistency** | No conflicting architecture decisions; each ADR references the context that motivated it | |
| 5 | **Story quality** | Every story has acceptance criteria in Given/When/Then format | |
| 6 | **Story independence** | Stories can be implemented independently (within dependency order) | |
| 7 | **Dependency feasibility** | All cross-story and cross-epic dependencies are mapped and achievable | |
| 8 | **Security addressed** | Authentication, authorization, and data protection are architecturally defined | |
| 9 | **Performance support** | NFR performance targets have architectural backing (caching, indexing, etc.) | |
| 10 | **Testing strategy** | Test approach defined for unit, integration, and E2E levels | |
| 11 | **Project context complete** | project-context.md covers code conventions, git workflow, and environment setup | |
| 12 | **Estimation confidence** | Story points reflect actual complexity; no "placeholder" estimates | |

**Verdict:**
- **PASS** — All 12 checks pass. Proceed to Phase 4 sprint planning.
- **CONCERNS** — 1-3 checks have issues. Architect addresses and re-validates.
- **FAIL** — 4+ checks fail or a critical architectural gap. Return to Phase 2 for PRD revision if requirements are the issue, or revise architecture if solutioning is the problem.

---

## Code Quality Checklist (Per Story)

Developers run this before marking a story as `in_review`.

| # | Check | Pass Criteria |
|---|-------|---------------|
| 1 | **Acceptance criteria met** | Every AC from the story has passing tests |
| 2 | **Tests pass** | Full test suite passes, no new failures introduced |
| 3 | **No regressions** | Existing functionality still works |
| 4 | **Code conventions** | Follows project-context.md conventions (naming, structure, style) |
| 5 | **No hardcoded values** | Magic numbers, URLs, and keys are in config/constants |
| 6 | **Error handling** | Error cases from AC are handled; no swallowed exceptions |
| 7 | **Security** | No injection vulnerabilities, credentials not in code, inputs validated |
| 8 | **Performance** | No obvious N+1 queries, unnecessary loops, or memory leaks |
| 9 | **Documentation** | Complex logic has inline comments; public APIs have docstrings |
| 10 | **Commit quality** | Descriptive commit message references story ID |

---

## Code Review Checklist

Reviewers (peer Developer or QA) use this to evaluate implementations.

| # | Check | What to Look For |
|---|-------|------------------|
| 1 | **Correctness** | Does the code actually do what the AC says? Test by reading, not by trusting test names |
| 2 | **Edge cases** | Boundary values, empty inputs, null/undefined, concurrent access |
| 3 | **Test quality** | Tests verify behavior, not implementation details; tests would catch regressions |
| 4 | **Readability** | Code is clear without explanation; naming reveals intent |
| 5 | **Architecture alignment** | Implementation follows architecture.md patterns and ADRs |
| 6 | **Security review** | No XSS, SQL injection, SSRF, or auth bypasses |
| 7 | **Performance** | Appropriate use of indexing, caching, pagination |
| 8 | **Error handling** | Errors surface useful information; no silent failures |
| 9 | **Backwards compatibility** | Changes don't break existing consumers |
| 10 | **Dependencies** | New dependencies are justified; no unnecessary additions |

---

## Sprint Retrospective Checklist

Scrum Master uses this to ensure retrospectives are productive.

| # | Check | Purpose |
|---|-------|---------|
| 1 | **Metrics gathered** | Points planned vs completed, velocity trend, blocker count |
| 2 | **Goal assessment** | Was the sprint goal met? If not, why? |
| 3 | **Carried-over analysis** | Why were stories carried over? Pattern or incident? |
| 4 | **Blocker patterns** | Are the same types of blockers recurring? |
| 5 | **Process improvements** | At least 2 concrete, actionable improvements identified |
| 6 | **Action items assigned** | Every action item has an owner and due date |
| 7 | **Previous actions reviewed** | Were last sprint's action items completed? |

---

## Quick Flow Checklist

Solo Dev runs this for Quick Flow work before marking as done.

| # | Check | Pass Criteria |
|---|-------|---------------|
| 1 | **Scope appropriate** | Change is < 15 stories and well-understood |
| 2 | **Tech spec written** | Brief spec exists (even inline in issue) |
| 3 | **Tests written** | Key paths have tests |
| 4 | **Tests pass** | Full test suite passes |
| 5 | **Self-reviewed** | Read own diff as a fresh reviewer |
| 6 | **No security issues** | No credentials, injection points, or auth bypasses |
| 7 | **Commit quality** | Clear commit message |
