---
name: paperclip-project-setup
description: >
  Orchestrate the full setup of a Paperclip company from an existing codebase:
  discover the project, interview the user, create company + project + agents +
  issues via the Paperclip API, configure heartbeats, and activate agents. Use
  when a user says "set up Paperclip for this project", "create agents for this
  codebase", "orchestrate this repo", or provides a project path and wants
  autonomous agent work. Do NOT use for modifying an already-running company,
  importing company packages, or creating companies from scratch without a
  codebase (use company-creator instead).
---

# Paperclip Project Setup

End-to-end orchestration: analyze a codebase, create a Paperclip company with agents, generate issues from the analysis, and activate autonomous work.

## Prerequisites

- Paperclip server running (default: `http://localhost:3100`)
- The target project accessible on the local filesystem
- `curl` available for API calls

## The Setup Procedure

Follow these steps in order. Do not skip the interview.

### Phase 1 — Discovery

Explore the target codebase thoroughly before asking the user anything. You need to understand:

1. **Tech stack** — languages, frameworks, databases, package managers
2. **Architecture** — monorepo vs monolith, frontend/backend split, microservices
3. **Current state** — what's implemented, what's partial, what's missing
4. **Code quality** — tests (coverage, stubs vs real), TODOs/FIXMEs, linting
5. **Git history** — recent commits, active areas, contributors
6. **Documentation** — README, API docs, inline comments
7. **Infrastructure** — Docker, CI/CD, deployment configs
8. **Pain points** — circular dependencies, missing tests, placeholder pages, known bugs

Use the Agent tool with `subagent_type: Explore` for deep codebase analysis. This phase should produce a mental model of the project sufficient to propose agents and issues.

### Phase 2 — Interview

Use AskUserQuestion for each round. Ask 3-5 questions per round, propose concrete defaults based on your discovery. Do NOT ask open-ended questions — always propose a specific answer the user can confirm or adjust.

**Round 1 — Company & Project:**

- Company name and description? (propose based on project name/README)
- Project name? (propose based on repo name)
- What is the `cwd` path for the project? (propose the path you explored)

**Round 2 — Agents:**

- Which agent roles do you need? Propose based on tech stack discovery:
  - Backend-heavy project → Backend Engineer + QA
  - Full-stack → Backend + Frontend + QA
  - Monolith → Full-Stack Engineer + QA
  - With infra concerns → + DevOps
  - Always include QA unless user opts out
- Names for the agents? (propose human names matching the project's language/culture)
- Adapter type? Default: `claude_local`. Options: `claude_local`, `codex_local`, `opencode_local`, `cursor`, `gemini_local`
- Model? Default: `claude-sonnet-4-6`. Options: `claude-sonnet-4-6`, `claude-opus-4-6`, `claude-haiku-4-5`

**Round 3 — Behavior:**

- Enable automatic heartbeats? Default: yes, 300s (5 min)
- Skip permission prompts (`dangerouslySkipPermissions`)? Default: yes for local dev
- Timeout per run? Default: 3600s (1 hour)
- Max turns per run? Default: 200
- Should QA have permission to delegate bugs to other agents? Default: yes

**Round 4 — Issues:**

- What types of issues to generate? Options (propose all that apply based on discovery):
  - `analysis` — Audit/analysis issues per agent (recommended first)
  - `incomplete` — Finish partially implemented features (TODOs, placeholders)
  - `bugs` — Known bugs or error-prone patterns found
  - `tests` — Missing test coverage (unit, integration, E2E)
  - `features` — New feature suggestions based on project domain
  - `refactor` — Code quality improvements
  - `infra` — Docker, CI/CD, deployment fixes
- Should analysis issues run first (critical/todo) with others in backlog? Default: yes

### Phase 3 — Setup via API

Execute all API calls against the Paperclip server. Use `curl` with JSON payloads.

**Critical conventions (lessons learned):**

- Always use **forward slashes** in Windows paths (`C:/code/project`, not `C:\code\project`)
- API base: `http://localhost:3100/api`
- All requests use `Content-Type: application/json`
- In `local_trusted` mode, no auth headers needed

#### Step 1 — Create Company

```bash
POST /api/companies
{
  "name": "<company-name>",
  "description": "<description>"
}
```

Save the returned `id` as `COMPANY_ID`.

#### Step 2 — Create Project

```bash
POST /api/companies/{companyId}/projects
{
  "name": "<project-name>",
  "description": "<tech-stack-summary>",
  "status": "in_progress"
}
```

Save the returned `id` as `PROJECT_ID`.

#### Step 3 — Create Agents

For each agent, call:

```bash
POST /api/companies/{companyId}/agents
{
  "name": "<agent-name>",
  "role": "<role>",
  "title": "<title>",
  "capabilities": "<detailed-capabilities-based-on-tech-stack>",
  "adapterType": "<adapter>",
  "adapterConfig": {
    "cwd": "<project-path-forward-slashes>",
    "model": "<model>",
    "maxTurnsPerRun": 200,
    "timeoutSec": 3600,
    "dangerouslySkipPermissions": true
  },
  "runtimeConfig": {
    "heartbeat": {
      "enabled": true,
      "intervalSec": 300,
      "wakeOnDemand": true,
      "maxConcurrentRuns": 1
    }
  },
  "permissions": { "canCreateAgents": false }
}
```

Save each returned `id` as `AGENT_<ROLE>_ID`.

**Valid roles:** `ceo`, `cto`, `cmo`, `cfo`, `engineer`, `designer`, `pm`, `qa`, `devops`, `researcher`, `general`

**Agent capabilities** should be specific to the discovered tech stack. Example for a .NET + Angular project:

- Backend: "Specialist in .NET 6, ASP.NET Core, Entity Framework Core, PostgreSQL, REST APIs..."
- Frontend: "Specialist in Angular 17, TypeScript, Bootstrap 5, RxJS, Reactive Forms..."
- QA: "Specialist in automated testing, Playwright E2E, unit tests, API validation..."

#### Step 4 — Configure QA Permissions

If user opted for QA delegation:

```bash
PATCH /api/agents/{qaAgentId}/permissions
{ "canCreateAgents": false, "canAssignTasks": true }
```

#### Step 5 — Create Issues

Create issues based on user selections. Follow this priority order:

**Analysis issues (create first, status: `todo`, priority: `critical`):**
- One per agent: "[Analysis] Full audit of <area>"
- These run first and inform all subsequent work

**Feature/bug/test issues (status: `todo`, priority based on impact):**
- `critical` — Blocking bugs, security issues
- `high` — Incomplete features, missing core tests
- `medium` — New features, improvements, E2E tests
- `low` — Nice-to-haves, polish, dark mode

```bash
POST /api/companies/{companyId}/issues
{
  "title": "<title>",
  "description": "<detailed-description-with-specific-file-paths-and-steps>",
  "projectId": "<projectId>",
  "status": "todo",
  "priority": "<priority>",
  "assigneeAgentId": "<agentId>"
}
```

**Issue description quality matters.** Each issue must include:
- Specific file paths to modify
- Step-by-step implementation instructions
- Expected behavior and acceptance criteria
- Dependencies on other issues if any

Distribute issues evenly across agents by their specialty area.

#### Step 6 — Activate

Trigger initial heartbeats for all agents:

```bash
POST /api/agents/{agentId}/heartbeat/invoke
```

Run all triggers in parallel (use `&` and `wait`).

### Phase 4 — Verification

After activation, verify:

1. All agents show `queued` or `running` status
2. Report the full setup summary to the user

## Setup Summary Template

After completing all phases, present this summary:

```
### Company: <name> (<issue-prefix>)
- ID: <company-id>
- Dashboard: http://127.0.0.1:3100

### Project: <project-name> (in_progress)

### Agents
| Agent | Role | Model | Heartbeat |
|-------|------|-------|-----------|
| <name> | <title> | <model> | <interval>s |

### Issues Created (<count>)
| Issue | Title | Agent | Priority | Status |
|-------|-------|-------|----------|--------|

### Activation
- All agents triggered and working
- Heartbeats auto-run every <interval>s
- QA can delegate bugs to other agents
```

## Monitoring Commands

After setup, the user can check status anytime:

```bash
# Check agent status
curl -s http://localhost:3100/api/companies/{companyId}/agents | python -m json.tool | grep -E "name|status"

# Check issue progress
curl -s http://localhost:3100/api/companies/{companyId}/issues | python -m json.tool | grep -E "identifier|status|title"

# Trigger a specific agent manually
curl -s -X POST http://localhost:3100/api/agents/{agentId}/heartbeat/invoke
```

## Reference

For detailed API schemas, agent configuration options, and governance rules, see:
[Paperclip API Reference](references/api-quick-reference.md)
[Configuration Patterns](references/config-patterns.md)
