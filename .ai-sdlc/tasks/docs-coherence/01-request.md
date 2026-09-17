# Solicitud

## Solicitud original

Sí, aplicalo para verlo en funcionamiento (referring to the coherence audit recommendations).

## Intento normalizado

Apply the bounded coherence fixes from the audit as task `docs-coherence`: Spanish prose in `docs/architecture.md` (lines 109-110 + descriptive diagram labels, machine identifiers stay English), Status x Phase x Verdict x Handoff matrix, precise read-only wording (no Write/Edit), architect write-scope nuance, and `architecture.md` -> `HOW-TO-USE.md` link. Docs-only, no behavior change.

## Contexto del repositorio

Audit covered `README.md`, `docs/HOW-TO-USE.md`, `docs/architecture.md`, `.ai-sdlc/README.md`, `CLAUDE.md`, `.claude/agents/*.md` (5), `.claude/skills/*/SKILL.md` (5), `.ai-sdlc/templates/*.md` (7). Contracts (entrypoint, 7-file workspace, max-2 remediations, PASS/FAIL, 3 human gates, permissions, links) verified coherent except the reported gaps.

## Restricciones

- Language policy: user docs Spanish; machine identifiers/paths/statuses/agent ids English.
- Smallest coherent change: `docs/architecture.md`, `.ai-sdlc/README.md`, `docs/HOW-TO-USE.md`, `README.md` only.
- No changes to `.claude/`, `.ai-sdlc/templates/`, `CLAUDE.md`, state machine semantics, or permissions.

## Notas

Task id: `docs-coherence`. Follows `sdlc-orchestration` skill gates.
