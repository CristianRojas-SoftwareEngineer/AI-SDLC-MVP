# Diseño

## Objetivos

- Alphabetical order == lifecycle order in every task workspace.
- Single static naming convention, no tooling or scripts.
- Zero stale references: templates, skills, agents, docs, and existing task contents all use numbered names.

## Componentes afectados

- `.ai-sdlc/templates/`: 7 renames via `Move-Item`; content edit only in `00-state.md` (artifact list).
- `.claude/skills/sdlc-orchestration/SKILL.md`: workspace list + remediation step.
- `.claude/skills/requirements|design|implementation|verification/SKILL.md`: read/produce/update mentions.
- `.claude/agents/ai-sdlc.md` (`state.md` x2), `requirements-engineer.md`, `software-architect.md`, `implementation-engineer.md`, `verification-engineer.md` (artifact sentences).
- `README.md`, `docs/HOW-TO-USE.md`, `docs/architecture.md`, `.ai-sdlc/README.md` (trees, tables, prose, matrix template ref).
- `.ai-sdlc/tasks/docs-howto/`, `docs-coherence/`, `numbered-artifacts/`: renames + content updates.
- Untouched: `CLAUDE.md`, `.gitignore`, semantics.

## Diseño propuesto

Mapping (old -> new):

| Old | New | Rationale |
|---|---|---|
| `state.md` | `00-state.md` | Canonical state, always relevant first |
| `request.md` | `01-request.md` | Lifecycle input |
| `requirements.md` | `02-requirements.md` | Phase 1 output |
| `design.md` | `03-design.md` | Phase 2 output |
| `implementation.md` | `04-implementation.md` | Phase 3 output |
| `verification.md` | `05-verification.md` | Phase 4 output |
| `delivery.md` | `06-delivery.md` | Phase 5 output |

Reference rewrite rule: bare `` `<name>.md` `` (no `NN-` prefix) becomes `` `NN-<name>.md` `` with the table above. Bare directory mentions (`.ai-sdlc/tasks/<task-id>/`) unchanged. Non-artifact `.md` mentions (`HOW-TO-USE.md`, `architecture.md`, `README.md`, `CLAUDE.md`, skill/agent paths) unchanged. Words without `.md` suffix unchanged.

## Flujo

No runtime flow change. Authoring flow unchanged; only file names differ.

## Interfaces y contratos

- The numbered file name IS the contract; the skill workspace list is normative.
- `00-state.md` artifact list enumerates the 6 numbered phase files.
- Alphabetical tools (`Get-ChildItem`, explorers, glob) now yield lifecycle order.

## Pasos de implementación

1. `00-state.md` (this task) -> Phase `Implementation`.
2. Rename `.ai-sdlc/templates/` 7 files with `Move-Item` (verify parent first).
3. Edit `00-state.md` template artifact list.
4. Edit 5 skills (orchestration list + remediation step; 4 phase skills).
5. Edit 5 agents.
6. Edit 4 docs (README, HOW-TO-USE, architecture, .ai-sdlc/README incl. matrix template ref).
7. Migrate `docs-howto`, `docs-coherence`, `numbered-artifacts`: `Move-Item` renames, then per-file reference updates.
8. Write `04-implementation.md`; verify with unnumbered-reference grep + listing order + link checks; write `05-verification.md` + `06-delivery.md`; `00-state.md` -> `DONE`.

## Estrategia de verificación

- AC1: `Get-ChildItem .ai-sdlc/templates` lists exactly the 7 numbered names.
- AC2: regex `(?<!\d-)(request|state|requirements|design|implementation|verification|delivery)\.md` over repo (excluding `.git/`) returns zero matches; manual review of any hit for false positives (e.g. historical quotes — none expected after migration).
- AC3: `Get-ChildItem` of each task workspace shows `00`..`06` order; same regex scoped to tasks returns zero.
- AC4: `CLAUDE.md` diff empty (no writes performed); skills/agents behavior sentences unchanged apart from file names.

## Consideraciones de despliegue / migración

Migration is the feature: old workspaces renamed in place. No backup infra in repo; renames are git-agnostic (not a git repo). If a consumer bookmarked old paths, they break — accepted per user scope (migrate everything, no parallel paths, per canonical-state rule).

## Decisiones clave

- `00-state.md` over unnumbered `state.md`: user decision; keeps the canonical file pinned first in every listing.
- Static prefixes over auto-increment tooling: no runtime/scripts exist in this MVP; numbering is a naming convention, hence "autoincremental" is satisfied by the fixed lifecycle sequence.
- Full migration over grandfathering: user decision + canonical-state rule (no parallel old/new paths).
- Rejected: `01-state.md` first with request unnumbered (breaks uniform `NN-` globbing); rejected: numbering only templates for future tasks (leaves existing workspaces incoherent).
