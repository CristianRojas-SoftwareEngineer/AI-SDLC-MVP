# Requisitos

## Problema

Audit found the system globally correct but not fully coherent: Spanish docs contain English prose (`architecture.md` lines 109-110 plus descriptive diagram labels), the Status x Phase x Verdict x Handoff model is undefined (task `Status` vs `Phase` vs `PASS`/`FAIL` vs handoff `COMPLETE`/`FAILED`), read-only wording is imprecise given `Bash` access, architect write scope is unnuanced, and `architecture.md` does not link to `HOW-TO-USE.md`.

## Resultado deseado

A reader finds zero English prose in user docs (machine identifiers stay English with an explicit note), one matrix explaining Status x Phase x Verdict x Handoff, precise permission wording, and bidirectional docs navigation.

## Requisitos funcionales

1. `docs/architecture.md`: translate prose lines 109-110 to Spanish; translate descriptive diagram labels (§2 topology roles, §6 helper lines) keeping machine values (`PENDING`, phase names, `PASS`/`FAIL`, `BLOCKED`, `DONE`, file names, agent ids) in English; add one note that machine identifiers stay English; add link to `HOW-TO-USE.md`.
2. `.ai-sdlc/README.md`: add Status x Phase x Verdict x Handoff matrix (task `Status` values vs `Phase` vs verification verdict vs specialist handoff), clarifying `COMPLETE`/`FAILED` are handoff states, `PASS`/`FAIL` are verification verdicts, `DONE`/`BLOCKED` are terminal task statuses.
3. `docs/HOW-TO-USE.md` + `README.md` + `docs/architecture.md`: replace imprecise "solo lectura" for verification with "sin `Write`/`Edit`: ejecuta comprobaciones (incluido `Bash`), no edita código"; nuance architect write scope (writes `03-design.md` artifact; repo edits belong to implementation unless design requires it — per agent tools, architect keeps Write/Edit for artifact creation).
4. Navigation: `architecture.md` links to `HOW-TO-USE.md` (completing bidirectional triangle README <-> HOW-TO-USE <-> architecture).

## Restricciones

- Spanish prose; English for commands, paths, agent ids, status/verdict values, file names.
- No semantic change to state machine, permissions, remediation bound, gates, agents, skills, templates.
- Smallest diffs preserving existing section structure and diagrams.

## Alcance

### En alcance

- `docs/architecture.md`, `.ai-sdlc/README.md`, `docs/HOW-TO-USE.md`, `README.md`.
- Task workspace `.ai-sdlc/tasks/docs-coherence/`.

### Fuera de alcance

- `.claude/`, `.ai-sdlc/templates/`, `CLAUDE.md`, behavior changes, new docs, translations to other languages.

## Suposiciones

- Machine vocabulary frozen: `PENDING`, `IN_PROGRESS`, `COMPLETE`, `BLOCKED`, `FAILED`, `DONE`, `PASS`/`FAIL`, phase names, `remediation_attempt`, agent ids.
- `00-state.md` template separation of `Status` and `Phase` is the normative model; docs only clarify it.

## Criterios de aceptación

- [ ] No English prose sentences remain in `docs/`, `README.md`, `.ai-sdlc/README.md` (verified by grep for the flagged sentences); machine identifiers remain English with an explicit note.
- [ ] Status x Phase x Verdict x Handoff matrix present and consistent with `00-state.md` template, `ai-sdlc.md` handoff format, and `sdlc-orchestration` gates.
- [ ] Permission wording precise in all three docs (verification = no Write/Edit, Bash allowed for checks; architect = artifact writes).
- [ ] `architecture.md` links to `HOW-TO-USE.md`; all doc links resolve.
- [ ] No diffs to agents, skills, templates, `CLAUDE.md`.

## Preguntas abiertas

### Bloqueantes

Ninguno.

### No bloqueados

Ninguno.
