# Diseño

## Objetivos

- Eliminate English prose from user docs without touching machine vocabulary.
- Define the Status x Phase x Verdict x Handoff model once, normatively, in `.ai-sdlc/README.md`, and reference it from `architecture.md` §6.
- Make permission wording tool-accurate (Write/Edit vs Bash).
- Close the docs navigation triangle with minimal added links.

## Componentes afectados

- `docs/architecture.md` (edit): §2 diagram role labels + §6 helper lines + §6 closing prose + identifier note + §7 permission nuance + §3 verification row nuance + HOW-TO-USE link.
- `.ai-sdlc/README.md` (edit): extend `Valores de estado` with Status/Phase/Verdict/Handoff matrix.
- `docs/HOW-TO-USE.md` (edit): §8 read-only bullet + §6 verification row (one-line each).
- `README.md` (edit): verification bullet + link already present (no link change needed).
- Untouched: `.claude/`, `.ai-sdlc/templates/`, `CLAUDE.md`, semantics.

## Diseño propuesto

### architecture.md edits

1. §2 diagram: `User requirement` -> `Requisito del usuario`; `main-session orchestrator` -> `orquestador de sesión principal`; role labels `Requirements Engineer` -> `Ingeniero de requisitos`, `Software Architect` -> `Arquitecto de software`, `Implementation Engineer` -> `Ingeniero de implementación`, `Verification Engineer` -> `Ingeniero de verificación`; `Maintenance / feedback` -> `Mantenimiento / comentarios`; `Requirements` -> `Requisitos`; `Delivery` -> `Entrega`; `Verification` (rerun node) -> `Verificación`. Keep: `AI-SDLC`, `PASS`/`FAIL`, all `*.md` file names.
2. §5/§6: after workspace tree add note: "Los identificadores de máquina (`PENDING`, fases, `PASS`/`FAIL`, `BLOCKED`, `DONE`, nombres de fichero y de agentes) se mantienen en inglés en diagramas y artefactos."
3. §6 helper lines: `max 2 remediation cycles` -> `máx. 2 ciclos de remediación`; `still failing` -> `sigue fallando`; `Blocking ambiguity can terminate the flow at Requirements or Design.` -> `La ambigüedad bloqueante puede detener el flujo en Requisitos o Diseño.`; `Maintenance/feedback creates new input to Requirements.` -> `Mantenimiento/comentarios genera una nueva entrada a Requisitos.` Phase names and verdicts stay English inside the code fence (they are machine values); surrounding prose Spanish.
4. §3 verification row: `La recolección de evidencia de solo lectura evita la autorreparación silenciosa` -> `La recolección de evidencia sin `Write`/`Edit` evita la autorreparación silenciosa`.
5. §7 permissions: `Los trabajadores de requisitos y verificación son de solo lectura.` -> `Los trabajadores de requisitos y verificación no tienen `Write`/`Edit` (verificación puede usar `Bash` para ejecutar comprobaciones, sin editar código).`; `Los trabajadores de diseño e implementación pueden escribir` -> `Los trabajadores de diseño e implementación tienen `Write`/`Edit`; el arquitecto escribe el artefacto `design.md` y la edición del repositorio corresponde a implementación`.
6. Add to §5 or §11: `Guía paso a paso: [HOW-TO-USE.md](HOW-TO-USE.md).` (one line; completes triangle).

### .ai-sdlc/README.md edits

Extend `Valores de estado` keeping the existing list, then add:

- `Phase` (separate field in `00-state.md`): `Requirements`, `Design`, `Implementation`, `Verification`, `Delivery`.
- Verdicts (only in `05-verification.md`): `PASS` / `FAIL`.
- Handoff (specialist -> orchestrator, per `ai-sdlc.md`): `COMPLETE`, `BLOCKED`, `FAILED`.
- Matrix: task `Status=DONE` iff `Phase=Delivery` + verdict `PASS` + delivery recorded; `Status=BLOCKED` on blocking question or 2 failed remediations; `COMPLETE`/`FAILED` describe a phase handoff, not the task; `FAIL` (verdict) triggers remediation, `FAILED` (handoff) reports a phase that could not complete.

### HOW-TO-USE.md edits

- §6 verification row: append `(sin `Write`/`Edit`)`.
- §8 bullet: `Los trabajadores de requisitos y verificación son de solo lectura: ...` -> `Los trabajadores de requisitos y verificación no tienen `Write`/`Edit` (verificación usa `Bash` para ejecutar comprobaciones): la verificación informa fallos, no corrige código en silencio.`

### README.md edits

- Verification bullet: `valida con evidencia objetiva usando los comandos del repositorio (solo lectura, no corrige en silencio)` -> `valida con evidencia objetiva usando los comandos del repositorio (sin `Write`/`Edit`, no corrige en silencio)`.

## Flujo

Docs reading flow unchanged: README -> HOW-TO-USE -> architecture -> task files.

## Interfaces y contratos

- Machine vocabulary frozen and reused verbatim from skill/agent/template sources.
- Links: `architecture.md` -> `HOW-TO-USE.md` (new), existing links untouched.

## Pasos de implementación

1. `00-state.md` -> Phase `Implementation`.
2. Edit `docs/architecture.md` (diagram labels, §6 prose, note, permissions, verification row, HOW-TO-USE link).
3. Edit `.ai-sdlc/README.md` (matrix).
4. Edit `docs/HOW-TO-USE.md` (§6 row, §8 bullet).
5. Edit `README.md` (verification bullet).
6. Write `04-implementation.md`; verify; write `05-verification.md` + `06-delivery.md`; `00-state.md` -> `DONE`.

## Estrategia de verificación

- AC1: grep flagged English sentences (`Blocking ambiguity`, `Maintenance/feedback creates`, `max 2 remediation cycles`, `still failing`, `User requirement`, `main-session orchestrator`) returns zero in `docs/` + root/README + `.ai-sdlc/README`; note about English identifiers present.
- AC2: matrix present; values cross-checked against `templates/00-state.md`, `agents/ai-sdlc.md:47-54`, skill gates.
- AC3: grep `solo lectura` for verification contexts returns zero (except intentional historical quote if any); `sin `Write`/`Edit`` present in 4 files.
- AC4: `rg` link check + `Test-Path` for all link targets.
- AC5: no writes to `.claude/`, templates, `CLAUDE.md` (session write log).

## Consideraciones de despliegue / migración

Not applicable. Docs-only.

## Decisiones clave

- Keep phase/verdict/status tokens English inside code fences (machine contract) while translating surrounding labels — preserves grepability and tool parsing.
- Put the normative matrix in `.ai-sdlc/README.md` (owner of state vocabulary) rather than duplicating it in `architecture.md`; architecture §6 references it in one line.
- One-line link to close the triangle instead of restructuring docs.
