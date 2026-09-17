# Solicitud

## Solicitud original

Algo que vi y me gustaría mejorar, es que los archivos que se generan dentro de los directorios en tasks, sería genial que tuvieran un identificador numérico autoincremental, porque si los veo todos juntos sin orden lógico, es confuso.

## Intento normalizado

Prefix the 7 task-workspace artifacts with zero-padded lifecycle-order numbers so alphabetical listing matches execution order. User decisions: scheme `00-state.md`, `01-request.md`, `02-requirements.md`, `03-design.md`, `04-implementation.md`, `05-verification.md`, `06-delivery.md`; migrate existing tasks (`docs-howto`, `docs-coherence`) plus this task.

## Contexto del repositorio

Contract defined in `.claude/skills/sdlc-orchestration/SKILL.md` (workspace list), referenced by phase skills, specialist agents, `.ai-sdlc/templates/` (7 files), `.ai-sdlc/README.md` (tree + matrix), `README.md`, `docs/HOW-TO-USE.md`, `docs/architecture.md`. Current alphabetical order (`delivery.md` first) does not match lifecycle order. `CLAUDE.md` references only the task directory, not file names.

## Restricciones

- Exact mapping: `state.md`->`00-state.md`, `request.md`->`01-request.md`, `requirements.md`->`02-requirements.md`, `design.md`->`03-design.md`, `implementation.md`->`04-implementation.md`, `verification.md`->`05-verification.md`, `delivery.md`->`06-delivery.md`.
- Machine vocabulary otherwise frozen; no semantic change to state machine, gates, permissions, remediation bound.
- Canonical state: rename templates, update every referencing call site (skills, agents, docs, matrix, existing task contents), remove old names.

## Notas

Task id: `numbered-artifacts`. Workspace created under the current (unnumbered) contract, then migrated with everything else.
