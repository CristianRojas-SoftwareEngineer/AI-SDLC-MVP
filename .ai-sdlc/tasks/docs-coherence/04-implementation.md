# Implementación

## Resumen

Docs-only coherence fixes. Translated English prose in `docs/architecture.md` (§2 diagram roles, §6 helper lines + closing sentences), added machine-identifier note and `HOW-TO-USE.md` link, nuanced permission wording (no Write/Edit), added Status x Phase x Verdict x Handoff matrix to `.ai-sdlc/README.md`, and made verification wording tool-accurate in guide + README.

## Áreas cambiadas

- `docs/architecture.md`: topology diagram labels Spanish (roles, requisito, orquestador, entrega, verificación, mantenimiento); §5 identifier note + HOW-TO-USE link; §6 `máx. 2 ciclos de remediación` / `sigue fallando` / two prose sentences Spanish; §3 verification row and §7 permissions precise (Write/Edit/Bash scopes, architect writes `03-design.md`).
- `.ai-sdlc/README.md`: kept status list, added Phase/Verdict/Handoff definitions + matrix + rules.
- `docs/HOW-TO-USE.md`: §6 verification row + §8 bullet precise.
- `README.md`: verification bullet precise.
- `.ai-sdlc/tasks/docs-coherence/`: request/state/requirements/design/implementation.

## Pruebas / chequeos agregados o actualizados

None (docs-only; repo has no doc test infrastructure).

## Chequeos ya ejecutados

- Re-read all four edited files before editing (exact-match edits).
- Design cross-checked against `templates/00-state.md`, `agents/ai-sdlc.md` handoff, skill gates.

## Limitaciones conocidas

- Formal verification (grep/link checks) pending in `05-verification.md`.
