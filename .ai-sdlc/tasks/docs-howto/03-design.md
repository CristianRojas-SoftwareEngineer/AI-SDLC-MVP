# Diseño

## Objetivos

- Give new users a complete definition in `README.md` and a step-by-step guide in `docs/HOW-TO-USE.md`.
- Ground every statement in existing files (`CLAUDE.md`, `.claude/agents/ai-sdlc.md`, `.claude/skills/sdlc-orchestration/SKILL.md`, `.ai-sdlc/templates/`, `docs/architecture.md`).
- Smallest coherent docs-only change: 1 new file + 1 enriched file, zero behavior changes.

## Componentes afectados

- `README.md` (edit): add definition, non-goals, components, prerequisites, installation, minimal example, artifact map, doc index. Keep topology diagram, naming detail, version note.
- `docs/HOW-TO-USE.md` (new): usage guide. Follows existing `docs/` convention alongside `docs/architecture.md`.
- `.ai-sdlc/tasks/docs-howto/` (task workspace): request/state/requirements/design/implementation/verification/delivery.
- Untouched: `.claude/agents/`, `.claude/skills/`, `.ai-sdlc/templates/`, `docs/architecture.md`, `CLAUDE.md`, state machine, permissions.

## Diseño propuesto

`README.md` structure (Spanish):

1. Title + definition paragraph (what AI-SDLC is: thin agentic control loop on Claude Code turning vague requirements into Requirements -> Design -> Implementation -> Verification -> Delivery).
2. `Qué es / Qué no es` (explicit non-goals: no MCP server, no orchestrator externo, no base de datos, no runtime, no agent teams, no hooks, no agente de despliegue).
3. `Componentes` (orquestador `ai-sdlc`, 4 especialistas con rol de una línea, skills por fase, plantillas `.ai-sdlc/templates/`, workspaces `.ai-sdlc/tasks/<task-id>/`).
4. `Requisitos previos` (Claude Code instalado, estructura copiada en la raíz del repo destino).
5. `Instalación` (3 pasos: copiar, iniciar `claude --agent ai-sdlc`, dar requisito).
6. `Ejemplo mínimo` (requirement quote + workspace created + PASS path).
7. `Mapa de artefactos` (7 files + `00-state.md` canonical).
8. `Documentación` (links to `docs/HOW-TO-USE.md`, `docs/architecture.md`).
9. Keep: topology diagram, naming detail, version note (moved after new sections, wording preserved/translated consistently).

`docs/HOW-TO-USE.md` structure (Spanish):

1. `Objetivo y audiencia`, `Requisitos previos`, `Instalación en el repo destino`.
2. `Iniciar una sesión` (`claude --agent ai-sdlc`, with requirement inline or in chat).
3. `Cómo escribir un buen requisito` (observable behavior + context, 2 examples good/bad).
4. `Qué ocurre en cada fase` (table phase -> executor -> artifact -> gate, grounded in skill).
5. `Dónde mirar los resultados` (workspace tree + `00-state.md` canonical note).
6. `Verificación y remediación` (`PASS` -> Delivery -> `DONE`; `FAIL` -> Implementation, max 2, then `BLOCKED`).
7. `Cuándo se detiene a preguntar` (3 human gates from skill).
8. `Ejemplo mínimo end-to-end` (concrete requirement, expected artifacts, delivery note, no deployment unless authorized).
9. `Problemas frecuentes` (blocked requirements, design conflict, still failing after 2 cycles -> new task/iteration).
10. `Siguientes pasos` (links to `../README.md`, `architecture.md`).

## Flujo

No runtime flow changes. Doc reading flow: `README.md` (definition, 5 min) -> `docs/HOW-TO-USE.md` (run first task) -> `docs/architecture.md` (rationale) -> task workspace files (evidence).

## Interfaces y contratos

- File contract: new path `docs/HOW-TO-USE.md`; edited path `README.md`. Relative links: `README.md` -> `docs/HOW-TO-USE.md`, `docs/architecture.md`; `docs/HOW-TO-USE.md` -> `../README.md`, `architecture.md`.
- Content contract: commands `claude --agent ai-sdlc`; agent ids `ai-sdlc`, `requirements-engineer`, `software-architect`, `implementation-engineer`, `verification-engineer`; workspace `.ai-sdlc/tasks/<task-id>/` with `01-request.md`, `00-state.md`, `02-requirements.md`, `03-design.md`, `04-implementation.md`, `05-verification.md`, `06-delivery.md`; remediation bound 2; `PASS`/`FAIL`; `DONE`/`BLOCKED`.
- Language contract: prose Spanish; code identifiers/paths/statuses English.

## Pasos de implementación

1. Update `00-state.md`: Phase `Design`, then `Implementation`.
2. Write `docs/HOW-TO-USE.md` per structure above, grounded in skill/agent facts.
3. Rewrite `README.md` per structure above, preserving diagram + naming + version note.
4. Write `04-implementation.md` (summary, files touched, checks run).
5. Verify: existence of files, link resolution, contract values grep, Spanish prose with English identifiers, no agent/skill/template diffs.
6. Write `05-verification.md` + `06-delivery.md`; update `00-state.md` to `DONE`.

## Estrategia de verificación

Mapped to acceptance criteria:

- AC1 (guide coverage): read `docs/HOW-TO-USE.md`, checklist each required section present.
- AC2 (README definition): read `README.md`, checklist definition/non-goals/components/prereqs/install/example/artifact map/links.
- AC3 (factual accuracy): grep commands/paths/agent names/status values against `.claude/agents/ai-sdlc.md` and `sdlc-orchestration/SKILL.md`; manual diff of values.
- AC4 (no behavior change): `git status` equivalent (file listing) shows only `README.md`, `docs/HOW-TO-USE.md`, `.ai-sdlc/tasks/docs-howto/` changed.
- AC5 (links resolve): check relative link targets exist on disk.

## Consideraciones de despliegue / migración

Not applicable. Docs-only change; no deployment. No migration: existing links to `docs/architecture.md` keep working; new link to `docs/HOW-TO-USE.md` is additive.

## Decisiones clave

- Location `docs/HOW-TO-USE.md` over root: user decision + keeps root clean and matches `docs/architecture.md` convention.
- Full README definition over brief: user decision; non-goals section prevents the most likely misuse (expecting deployment, MCP, hooks, teams).
- No changes to `docs/architecture.md`: avoids duplication; guide links to it for rationale instead of copying it.
- Rejected: `HOW-TO-USE.md` in root (more visible but breaks `docs/` convention and clutters root); rejected: README-as-index-only (loses the 5-minute definition the user asked for).
