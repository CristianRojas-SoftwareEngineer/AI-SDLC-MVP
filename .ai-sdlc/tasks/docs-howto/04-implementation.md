# Implementación

## Resumen

Docs-only change. Created `docs/HOW-TO-USE.md` (Spanish step-by-step usage guide) and enriched root `README.md` (Spanish full definition: what is / what is not, components, prerequisites, installation, minimal example, artifact map, doc index). No agent, skill, template, or workflow behavior changed.

## Áreas cambiadas

- `docs/HOW-TO-USE.md` (new): prerequisites, installation, `claude --agent ai-sdlc` start, requirement-writing guidance, phase table, workspace map, PASS/FAIL + max-2 remediation, stop-and-ask gates, end-to-end example, troubleshooting, next steps with links to `../README.md` and `architecture.md`.
- `README.md` (edit): added definition, non-goals, components, prerequisites, installation, minimal example, artifact map, documentation index; preserved topology diagram, naming detail, version note.
- `.ai-sdlc/tasks/docs-howto/`: request/state/requirements/design/implementation (this file); verification/delivery pending.

## Pruebas / chequeos agregados o actualizados

None (docs-only; repo has no doc test infrastructure).

## Chequeos ya ejecutados

- Re-read `README.md`, `docs/architecture.md`, `.ai-sdlc/README.md`, `CLAUDE.md` before editing.
- Grounded commands/paths/agents/statuses in `.claude/agents/ai-sdlc.md` and `.claude/skills/sdlc-orchestration/SKILL.md`.

## Limitaciones conocidas

- Verification of link resolution and contract-value consistency still pending (see `05-verification.md`).
