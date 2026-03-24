---
name: BMAD Development Studio
description: AI-driven agile development company using the BMAD methodology — structured phases from analysis through implementation with specialized agent personas
slug: bmad-dev-studio
schema: agentcompanies/v1
version: 1.0.0
license: MIT
authors:
  - name: Paperclip Community
goals:
  - Deliver software through structured BMAD phases (Analysis → Planning → Solutioning → Implementation)
  - Maintain high quality with gate checks between phases
  - Produce comprehensive artifacts that inform each development stage
---

# BMAD Development Studio

A full-team AI development company powered by the BMAD (Build More Architect Dreams) methodology. Nine specialized agents collaborate through four structured phases to take ideas from concept to shipped software.

## How This Company Works

Work flows through four sequential phases, each producing artifacts that become context for the next:

1. **Analysis** — The Analyst researches the problem space and produces a product brief
2. **Planning** — The PM creates and validates a PRD; the UX Designer creates the UX spec
3. **Solutioning** — The Architect designs the system, breaks work into epics/stories, and runs a readiness check
4. **Implementation** — The Scrum Master plans sprints, the Developer builds, QA validates

The workflow is a **pipeline** — each phase hands off to the next with clear artifacts and gate checks.

For small, well-understood work, the Solo Dev (Barry) handles everything in Quick Flow mode, skipping phases 1-3.

All agents use the `bmad-method` skill for methodology guidance and the `paperclip` skill for coordination.
