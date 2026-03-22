# Configuration Patterns

Proven patterns and gotchas learned from real Paperclip project setups.

## Windows Path Convention

**Always use forward slashes** in all API payloads, even on Windows:

```
CORRECT:  "cwd": "C:/code/my-project"
WRONG:    "cwd": "C:\\code\\my-project"
```

Backslashes cause `Internal server error` on PATCH/POST calls. The Paperclip server and claude_local adapter handle forward slashes correctly on all platforms.

## Recommended adapterConfig

Minimum viable config for productive agents:

```json
{
  "cwd": "C:/code/project",
  "model": "claude-sonnet-4-6",
  "maxTurnsPerRun": 200,
  "timeoutSec": 3600,
  "dangerouslySkipPermissions": true
}
```

**Why each field matters:**

- `cwd` — Without this, agents work in a default Paperclip-managed directory instead of the project
- `model` — Without this, the adapter uses its default which may not be what the user expects
- `maxTurnsPerRun` — 200 gives enough room for complex analysis/implementation. Default may be too low
- `timeoutSec` — 3600 (1 hour) prevents premature timeouts on deep work
- `dangerouslySkipPermissions` — Without this, Claude Code prompts for permission on every shell command and the heartbeat stalls with "I need approval for curl" messages

## Recommended runtimeConfig

For autonomous operation:

```json
{
  "heartbeat": {
    "enabled": true,
    "intervalSec": 300,
    "wakeOnDemand": true,
    "maxConcurrentRuns": 1
  }
}
```

**Interval guidance:**

| Interval | Use case |
|----------|----------|
| 60s | Urgent debugging, rapid iteration |
| 300s | Standard autonomous work (recommended default) |
| 600s | Background/low-priority work |
| 0 | Manual-only (no auto heartbeat) |

## Issue Creation Strategy

### Priority and Status Matrix

| Issue type | Priority | Initial status | Why |
|------------|----------|---------------|-----|
| Analysis/audit | critical | todo | Must run first to inform all other work |
| Blocking bugs | critical | todo | Prevents other features from working |
| Incomplete features | high | todo | Core functionality gaps |
| Missing tests (unit) | high | todo | Quality baseline |
| New features | medium | todo | Enhancements |
| E2E tests | medium | todo | Integration validation |
| Polish/refactor | low | todo | Nice-to-haves |

### Issue Description Quality

Bad issue (agent won't know what to do):

```
"Fix the login"
```

Good issue (agent can work autonomously):

```
"Fix login form validation - Backend (Controllers/AuthController.cs)

The registration endpoint POST /api/auth/register returns 500 when email
is already taken instead of 409 Conflict.

1. In AuthController.cs, wrap UserService.RegisterAsync() in try-catch
2. Catch DuplicateEmailException and return Conflict()
3. Add email uniqueness check in UserService before calling Firebase
4. Test with curl: POST /api/auth/register with existing email

Files: Controllers/AuthController.cs, Services/UserService.cs"
```

### Distribution Strategy

Distribute issues by agent specialty:

| Agent role | Issue types |
|------------|-------------|
| Backend Engineer | API endpoints, database, services, migrations, performance |
| Frontend Engineer | UI components, pages, routing, forms, styling, UX |
| QA Engineer | Unit tests, E2E tests, test coverage, validation, security audit |
| DevOps | Docker, CI/CD, deployment, monitoring |
| Full-Stack | Any of the above |

## QA Agent Delegation Pattern

Give the QA agent `canAssignTasks: true` so it can:

1. Run tests and discover bugs
2. Create new issues with detailed reproduction steps
3. Assign those issues to the appropriate specialist agent
4. Track fixes and re-validate

This creates a feedback loop: QA finds → specialist fixes → QA verifies.

## Post-Setup Monitoring

### Check all agents status

```bash
curl -s http://localhost:3100/api/companies/{companyId}/agents \
  | python -m json.tool | grep -E '"name"|"status"|lastHeartbeat'
```

### Check issue progress

```bash
curl -s http://localhost:3100/api/companies/{companyId}/issues \
  | python -m json.tool | grep -E '"identifier"|"status"|"title"'
```

### Get logs from a specific agent's last run

```bash
# Get latest run ID
RUN_ID=$(curl -s "http://localhost:3100/api/companies/{companyId}/heartbeat-runs?agentId={agentId}&limit=1" \
  | python -m json.tool | grep '"id"' | head -1 | awk -F'"' '{print $4}')

# Get events
curl -s "http://localhost:3100/api/heartbeat-runs/$RUN_ID/events?limit=50" | python -m json.tool
```

### Re-trigger a specific agent

```bash
curl -s -X POST http://localhost:3100/api/agents/{agentId}/heartbeat/invoke

# With context (e.g., after fixing a blocker)
curl -s -X POST http://localhost:3100/api/agents/{agentId}/wakeup \
  -H "Content-Type: application/json" \
  -d '{"source": "on_demand", "reason": "Docker Engine is now running, proceed with validation"}'
```

## Recovery After Restart

If the Paperclip server or machine restarts:

1. Start the server: `cd <paperclip-dir> && pnpm dev`
2. Wait for "Server listening" message
3. The heartbeat scheduler auto-resumes — agents with `intervalSec > 0` will auto-trigger
4. For immediate action, manually invoke heartbeats:

```bash
# Trigger all agents in a company
for ID in $(curl -s http://localhost:3100/api/companies/{companyId}/agents | python -m json.tool | grep '"id"' | awk -F'"' '{print $4}'); do
  curl -s -X POST "http://localhost:3100/api/agents/$ID/heartbeat/invoke" &
done
wait
```
