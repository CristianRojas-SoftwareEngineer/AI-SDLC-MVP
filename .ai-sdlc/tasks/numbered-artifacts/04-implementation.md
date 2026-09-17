# Implementación

## Resumen

Numbered task-artifact contract applied repo-wide. Templates renamed to `00-state.md`, `01-request.md`, `02-requirements.md`, `03-design.md`, `04-implementation.md`, `05-verification.md`, `06-delivery.md`. All call sites updated: 5 skills, 5 agents, 4 user docs. Three workspaces migrated (files renamed, operational references updated); intentional mapping evidence in this task's `01-request.md`, `02-requirements.md`, `03-design.md` (mapping table, problem statement, before/after quotes) preserved.

## Áreas cambiadas

- `.ai-sdlc/templates/`: 7 `Move-Item` renames; `00-state.md` artifact list numbered.
- `.claude/skills/sdlc-orchestration/SKILL.md`: workspace list + remediation step numbered.
- Phase skills: read/produce/update file names numbered (`requirements`, `design`, `implementation`, `verification` skills).
- Agents: `ai-sdlc` (`00-state.md`), `requirements-engineer` (`02-`), `software-architect` (`03-`), `implementation-engineer` (`04-`), `verification-engineer` (`05-`).
- Docs: `README.md`, `docs/HOW-TO-USE.md`, `docs/architecture.md` (diagram labels, trees, tables, prose), `.ai-sdlc/README.md` (tree + matrix template ref `templates/00-state.md`).
- Tasks `docs-howto` (6 files), `docs-coherence` (6 files + 5 surgical edits in `03-design.md`), `numbered-artifacts` (`00-state.md` list + `03-design.md` steps 1 and 8).

## Pruebas / chequeos agregados o actualizados

None (docs + naming change; repo has no test infrastructure for this).

## Chequeos ya ejecutados

- `Get-ChildItem` templates and task dirs show `00`..`06` order.
- Per-file reference updates listed above; full-repo unnumbered-reference grep pending in `05-verification.md`.

## Limitaciones conocidas

- Intentional unnumbered mentions remain as change evidence: mapping table and rationale in `numbered-artifacts/03-design.md`, problem/AC wording in `02-requirements.md`, full `01-request.md` mapping spec, before/after quotes in `docs-coherence/03-design.md:26`. Documented as allowlist in `05-verification.md`.
