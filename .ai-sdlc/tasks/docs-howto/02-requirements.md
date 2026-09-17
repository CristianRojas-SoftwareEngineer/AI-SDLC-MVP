# Requisitos

## Problema

The repository has no `HOW-TO-USE.md`-style usage documentation, and the root `README.md` defines the system poorly: it states the loop at a high level and shows quick start, but does not define what AI-SDLC is / is not, its components, prerequisites, or a minimal end-to-end example.

## Resultado deseado

A new user can answer in under 5 minutes: what AI-SDLC is, what it is not, what parts it has, what is needed to run it, how to run one task end-to-end, where to look at results, and where to go next.

## Requisitos funcionales

1. New file `docs/HOW-TO-USE.md` in Spanish with: prerequisites, installation into a target repo, how to start a session (`claude --agent ai-sdlc`), how to write a good requirement, what happens in each phase, where artifacts land (`.ai-sdlc/tasks/<task-id>/` with 7 files), how to interpret verification `PASS`/`FAIL` and the max-2 remediation loop, when the agent stops to ask, and a minimal end-to-end example.
2. Enriched root `README.md` in Spanish with: definition of the system, what it is not, components (orchestrator + 4 specialists + skills + templates + task workspaces), prerequisites, installation, minimal usage example, artifact map, link to `docs/HOW-TO-USE.md` and `docs/architecture.md`.
3. Bidirectional linking: `README.md` links to `docs/HOW-TO-USE.md`; `docs/HOW-TO-USE.md` links back to `README.md` and `docs/architecture.md`.

## Restricciones

- User-facing docs in Spanish; identifiers, commands, file paths, status values (`PENDING`, `PASS`/`FAIL`, `BLOCKED`, `DONE`), and agent names stay in English as in the codebase.
- No changes to `.claude/agents/`, `.claude/skills/`, `.ai-sdlc/templates/`, state machine, or permissions.
- No new runtime, dependency, MCP server, hook, or deployment step.

## Alcance

### En alcance

- `docs/HOW-TO-USE.md` (new).
- Root `README.md` (enrich, keep existing sections where still valid).
- Task artifacts under `.ai-sdlc/tasks/docs-howto/`.

### Fuera de alcance

- Rewriting `docs/architecture.md` beyond adding no content (link-only touch if needed; prefer zero changes).
- Video, screenshots, translations to other languages, automated doc tests.
- Any behavior change in the SDLC loop.

## Suposiciones

- The documented entrypoint remains `claude --agent ai-sdlc` (per `CLAUDE.md` and `.claude/agents/ai-sdlc.md`).
- The workspace layout remains the 7-file contract from `sdlc-orchestration` skill.
- Target audience: developer using Claude Code on a repository where this structure is installed at root.

## Criterios de aceptación

- [ ] `docs/HOW-TO-USE.md` exists, in Spanish, and covers: prerequisites, installation, start command, requirement writing, phase walkthrough, artifact map, PASS/FAIL + remediation bound, stop-and-ask gates, minimal example, troubleshooting, next steps.
- [ ] Root `README.md` in Spanish defines: what AI-SDLC is, what it is not, components, prerequisites, installation, minimal example, artifact map, and links to `docs/HOW-TO-USE.md` and `docs/architecture.md`.
- [ ] All commands, paths, agent names, and status values match the implementation (`ai-sdlc`, `claude --agent ai-sdlc`, `.ai-sdlc/tasks/<task-id>/`, 7 artifact names, max 2 remediations, `PASS`/`FAIL`).
- [ ] No modifications to agents, skills, templates, or state machine.
- [ ] Markdown links between `README.md`, `docs/HOW-TO-USE.md`, and `docs/architecture.md` resolve to existing files.

## Preguntas abiertas

### Bloqueantes

Ninguno.

### No bloqueados

- Whether a future `delivery-engineer` or hooks section will be needed in the guide: recorded as out of scope; guide states current behavior (no deployment agent, no hooks).
