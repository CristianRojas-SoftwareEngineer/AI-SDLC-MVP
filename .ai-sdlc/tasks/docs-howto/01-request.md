# Solicitud

## Solicitud original

No veo ninguna documentación tipo "HOW-TO-USE.md", y el README.md principal es bastante pobre en definición.

## Intento normalizado

Create user-facing usage documentation at `docs/HOW-TO-USE.md` and enrich the root `README.md` with a complete definition (what it is / what it is not, components, prerequisites, minimal end-to-end example). User decisions: location = `docs/HOW-TO-USE.md`; README scope = full definition.

## Contexto del repositorio

AI-SDLC MVP for Claude Code. Root `README.md` (Spanish) covers quick start, rationale, topology, naming detail, version note. `docs/architecture.md` (Spanish) covers intent, topology, agents, skills, artifacts, state machine, permissions. `.ai-sdlc/README.md` (Spanish) covers runtime layout. No usage guide exists. Orchestrator: `.claude/agents/ai-sdlc.md`; workflow: `.claude/skills/sdlc-orchestration/SKILL.md`.

## Restricciones

- Language policy in `CLAUDE.md`: user-facing docs in Spanish; Claude Code artifacts and task workspaces in English.
- Smallest coherent change: only `README.md` + new `docs/HOW-TO-USE.md`; no changes to agents, skills, templates, or state machine.
- Preserve existing conventions: `docs/architecture.md` link pattern, `claude --agent ai-sdlc` entrypoint, `.ai-sdlc/tasks/<task-id>/` workspace layout.

## Notas

Task id: `docs-howto`.
