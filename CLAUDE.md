# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What is Paperclip

Paperclip is an open-source orchestration platform for AI agents to run autonomous businesses. It's a Node.js server + React UI that manages org charts, budgets, governance, goal alignment, and agent coordination. Agents (Claude Code, Codex, Cursor, OpenClaw, Gemini, etc.) connect via adapters and receive heartbeats to perform work.

## Running the Project

### Prerequisites
- Node.js 20+
- pnpm 9+

### Setup and Development
```sh
pnpm install
pnpm dev          # API server (localhost:3100) + UI in dev middleware mode, with file watching
pnpm dev:once     # Same but without file watching
pnpm dev:server   # Server only
```

No database setup required — embedded PostgreSQL auto-starts when `DATABASE_URL` is unset, storing data at `~/.paperclip/instances/default/db/`.

To reset local dev database: `rm -rf ~/.paperclip/instances/default/db && pnpm dev`

### Build
```sh
pnpm build        # Build all packages (TypeScript + Vite)
pnpm typecheck    # Type check across entire workspace
```

### Testing
```sh
pnpm test:run                    # Vitest unit tests
pnpm test:run -- --testPathPattern=packages/db   # Run tests for a specific package
pnpm test:e2e                    # Playwright E2E tests
pnpm test:e2e:headed             # E2E with visible browser
```

### Database
```sh
pnpm db:generate   # Generate new Drizzle migration from schema changes
pnpm db:migrate    # Apply pending migrations
```

## Architecture

### Monorepo Structure (pnpm workspaces)

- **`server/`** — Express 5 API server. Routes in `server/src/routes/`, services in `server/src/services/`. Serves the UI in production. WebSocket for real-time updates (`server/src/realtime/`).
- **`ui/`** — React 19 + Vite + Tailwind CSS 4. Pages in `ui/src/pages/`, components in `ui/src/components/`. Uses React Context for global state and TanStack Query for server data.
- **`cli/`** — Commander-based CLI, bundled with esbuild. Published as `paperclipai` on npm.
- **`packages/db/`** — Drizzle ORM database layer. Schema files in `packages/db/src/schema/`, migrations in `packages/db/src/migrations/`. Supports embedded PostgreSQL (dev) and standard PostgreSQL (prod).
- **`packages/shared/`** — Shared TypeScript types and constants used by server, UI, and CLI.
- **`packages/adapter-utils/`** — Base classes and utilities for building agent adapters.
- **`packages/adapters/`** — Agent adapter implementations (claude-local, codex-local, cursor-local, gemini-local, openclaw-gateway, opencode-local, pi-local). Each adapter has `src/index.ts` (core), `src/server/` (backend), `src/ui/` (frontend), `src/cli/` (CLI).
- **`packages/plugins/`** — Plugin SDK and plugin examples.
- **`doc/`** — Internal documentation (SPEC, DATABASE, DEPLOYMENT-MODES, DEVELOPING, etc.).
- **`tests/`** — E2E (Playwright) and smoke tests.
- **`scripts/`** — Build, release, dev-runner, and utility scripts.

### Key Patterns

- **Adapter pattern**: Each agent type (Claude, Codex, etc.) has an adapter package under `packages/adapters/`. Adapters export server-side handlers, UI components, and CLI commands. New adapters extend base classes from `adapter-utils`.
- **Plugin system**: SDK at `packages/plugins/`, runtime managed by `server/src/services/plugin-*.ts`. Worker-side hooks with UI bridge.
- **Database**: Drizzle ORM with PostgreSQL. Schema is split per-feature across `packages/db/src/schema/*.ts`. Migrations auto-apply on startup.
- **Auth**: better-auth library. Three deployment modes: `local_trusted` (no auth), `authenticated/private`, `authenticated/public`.
- **Real-time**: WebSocket events for live UI updates.

### TypeScript Configuration
- Base config in `tsconfig.base.json`: ES2023 target, NodeNext modules, strict mode enabled.
- Each package extends the base config.

## Development Policies

- **Lockfile**: Do NOT commit `pnpm-lock.yaml` in pull requests. GitHub Actions owns it and regenerates it on master.
- **Pre-commit**: `scripts/check-forbidden-tokens.mjs` runs to prevent accidental token leaks.
