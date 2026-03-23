# API Quick Reference

Base URL: `http://localhost:3100/api`

## Companies

| Action | Method | Endpoint |
|--------|--------|----------|
| Create company | POST | `/api/companies` |
| Get company | GET | `/api/companies/{id}` |
| List companies | GET | `/api/companies` |

### Create Company Body

```json
{
  "name": "string (required, min 1)",
  "description": "string | null",
  "budgetMonthlyCents": "number (default: 0)"
}
```

Response includes auto-generated `issuePrefix` (e.g. "DAI" for "Daichi").

## Projects

| Action | Method | Endpoint |
|--------|--------|----------|
| Create project | POST | `/api/companies/{companyId}/projects` |
| Get project | GET | `/api/projects/{id}` |
| List projects | GET | `/api/companies/{companyId}/projects` |

### Create Project Body

```json
{
  "name": "string (required)",
  "description": "string | null",
  "status": "backlog | planned | in_progress | completed | cancelled",
  "leadAgentId": "uuid | null",
  "targetDate": "string | null",
  "color": "string | null"
}
```

## Project Workspaces

| Action | Method | Endpoint |
|--------|--------|----------|
| Create workspace | POST | `/api/projects/{projectId}/workspaces` |
| List workspaces | GET | `/api/projects/{projectId}/workspaces` |

### Create Project Workspace Body

```json
{
  "name": "string (optional)",
  "sourceType": "local_path | git_repo | remote_managed | non_git_path (default: local_path)",
  "cwd": "string (absolute path, forward slashes on Windows)",
  "isPrimary": "boolean (default: false — set to true to make this the effective working directory)"
}
```

**CRITICAL:** Without a primary workspace, agents run in a Paperclip-managed empty directory instead of the actual project. Always create a workspace with `isPrimary: true` right after creating the project.

## Agents

| Action | Method | Endpoint |
|--------|--------|----------|
| Create agent | POST | `/api/companies/{companyId}/agents` |
| Update agent | PATCH | `/api/agents/{id}` |
| Update permissions | PATCH | `/api/agents/{id}/permissions` |
| Trigger heartbeat | POST | `/api/agents/{id}/heartbeat/invoke` |
| Wakeup with context | POST | `/api/agents/{id}/wakeup` |
| Get agent | GET | `/api/agents/{id}` |
| List agents | GET | `/api/companies/{companyId}/agents` |

### Create Agent Body

```json
{
  "name": "string (required)",
  "role": "ceo | cto | cmo | cfo | engineer | designer | pm | qa | devops | researcher | general",
  "title": "string | null",
  "capabilities": "string | null",
  "adapterType": "claude_local | codex_local | opencode_local | pi_local | cursor | gemini_local | openclaw_gateway",
  "adapterConfig": {
    "cwd": "string (absolute path, forward slashes on Windows)",
    "model": "claude-sonnet-4-6 | claude-opus-4-6 | claude-haiku-4-5",
    "maxTurnsPerRun": "number (default: 200)",
    "timeoutSec": "number (default: 3600)",
    "dangerouslySkipPermissions": "boolean (default: false)",
    "graceSec": "number (default: 20)",
    "extraArgs": "string[]"
  },
  "runtimeConfig": {
    "heartbeat": {
      "enabled": "boolean (default: true)",
      "intervalSec": "number (0 = disabled, recommended: 300)",
      "wakeOnDemand": "boolean (default: true)",
      "maxConcurrentRuns": "number (default: 1, max: 10)"
    }
  },
  "permissions": {
    "canCreateAgents": "boolean (default: false)"
  },
  "reportsTo": "uuid | null (parent agent)",
  "budgetMonthlyCents": "number (default: 0)",
  "metadata": "object | null"
}
```

### Update Permissions Body

```json
{
  "canCreateAgents": "boolean",
  "canAssignTasks": "boolean"
}
```

### Wakeup Body

```json
{
  "source": "on_demand | timer | assignment | automation",
  "triggerDetail": "manual | ping | callback | system",
  "reason": "string (context for the agent)"
}
```

## Issues

| Action | Method | Endpoint |
|--------|--------|----------|
| Create issue | POST | `/api/companies/{companyId}/issues` |
| Update issue | PATCH | `/api/issues/{id}` |
| Get issue | GET | `/api/issues/{id}` |
| List issues | GET | `/api/companies/{companyId}/issues` |
| Search issues | GET | `/api/companies/{companyId}/issues?q=term` |

### Create Issue Body

```json
{
  "title": "string (required)",
  "description": "string | null",
  "projectId": "uuid | null",
  "parentId": "uuid | null (for subtasks)",
  "status": "backlog | todo | in_progress | in_review | done | blocked | cancelled",
  "priority": "critical | high | medium | low",
  "assigneeAgentId": "uuid | null"
}
```

### Update Issue Body

All fields optional:

```json
{
  "title": "string",
  "description": "string",
  "status": "string",
  "priority": "string",
  "assigneeAgentId": "uuid | null",
  "comment": "string (auto-creates a comment)"
}
```

## Heartbeat Runs

| Action | Method | Endpoint |
|--------|--------|----------|
| List runs | GET | `/api/companies/{companyId}/heartbeat-runs?agentId={id}` |
| Get run | GET | `/api/heartbeat-runs/{runId}` |
| Get run events | GET | `/api/heartbeat-runs/{runId}/events` |
| Get run log | GET | `/api/heartbeat-runs/{runId}/log` |
