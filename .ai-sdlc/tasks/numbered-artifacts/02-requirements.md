# Requisitos

## Problema

Task workspace files sort alphabetically (`delivery.md`, `design.md`, `implementation.md`, ...) which does not match lifecycle order, so viewing them together is confusing.

## Resultado deseado

Alphabetical listing of any task workspace matches execution order: canonical state first, then request through delivery.

## Requisitos funcionales

1. Rename the 7 templates in `.ai-sdlc/templates/` to `00-state.md`, `01-request.md`, `02-requirements.md`, `03-design.md`, `04-implementation.md`, `05-verification.md`, `06-delivery.md`; update the artifact list inside `00-state.md`.
2. Update every referencing call site: `sdlc-orchestration` skill workspace list + remediation step, the 4 phase skills (read/produce/update file names), the 5 agents (artifact file names), `.ai-sdlc/README.md` (tree + matrix template reference), `README.md`, `docs/HOW-TO-USE.md`, `docs/architecture.md` (trees, tables, prose mentions).
3. Migrate existing workspaces (`docs-howto`, `docs-coherence`, `numbered-artifacts`): rename files and update in-content references so no unnumbered artifact name remains.

## Restricciones

- Exact file mapping from the normalized intent; zero-padded two-digit prefixes.
- No semantic change: same 7 artifacts, same gates, same Status/Phase/Verdict/Handoff vocabulary.
- `CLAUDE.md` needs no change (references only the task directory).
- User-facing docs stay Spanish; file names stay English with numeric prefixes.

## Alcance

### En alcance

- `.ai-sdlc/templates/` (renames + `00-state.md` content).
- `.claude/skills/*/SKILL.md` (5 files), `.claude/agents/*.md` (5 files).
- `README.md`, `docs/HOW-TO-USE.md`, `docs/architecture.md`, `.ai-sdlc/README.md`.
- Existing task workspaces including this one.

### Fuera de alcance

- State machine, permissions, remediation bound, gates, new artifacts, tooling/scripts for auto-numbering (prefixes are static by convention).

## Suposiciones

- File explorers and `Get-ChildItem` sort `00`-`06` prefixes before/alongside any future files; future artifacts continue the `NN-name.md` convention.
- `00` for state (canonical, always relevant first), `01`-`06` follow lifecycle order.

## Criterios de aceptación

- [ ] `.ai-sdlc/templates/` contains exactly the 7 numbered files; no unnumbered names remain.
- [ ] No unnumbered artifact reference (``request.md``, ``state.md``, ``requirements.md``, ``design.md``, ``implementation.md``, ``verification.md``, ``delivery.md`` without numeric prefix) remains in skills, agents, docs, or task contents.
- [ ] All three existing workspaces list files in lifecycle order and their contents reference only numbered names.
- [ ] `CLAUDE.md`, state machine semantics, gates, and permissions unchanged.

## Preguntas abiertas

### Bloqueantes

Ninguno.

### No bloqueados

Ninguno.
