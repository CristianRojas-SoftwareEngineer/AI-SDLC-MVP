# Artefactos de ejecución de AI-SDLC

Este directorio almacena el estado duradero de las ejecuciones de AI-SDLC.

## Estructura

```text
.ai-sdlc/
└── tasks/
    └── <task-id>/
        ├── 00-state.md
        ├── 01-request.md
        ├── 02-requirements.md
        ├── 03-design.md
        ├── 04-implementation.md
        ├── 05-verification.md
        └── 06-delivery.md
```

Los artefactos son Markdown de forma deliberada. Claude Code puede leerlos y editarlos sin herramientas adicionales, siguen siendo inspeccionables por personas y sobreviven a la compactación de contexto o a una nueva sesión.

`00-state.md` es el estado canónico del flujo de trabajo. Los demás archivos son salidas/contratos de fase.

## Valores de estado

El campo `Status` de `00-state.md` usa uno de estos valores:

- `PENDING`
- `IN_PROGRESS`
- `COMPLETE`
- `BLOCKED`
- `FAILED`
- `DONE`

La verificación usa `PASS`/`FAIL`; el estado del flujo de trabajo usa los valores de estado anteriores.

## Modelo Estado × Fase × Veredicto × Handoff

`Status` y `Phase` son campos separados en `00-state.md` (ver `.ai-sdlc/templates/00-state.md`). No los confundas con el veredicto de verificación ni con el handoff del especialista:

- `Phase`: `Requirements`, `Design`, `Implementation`, `Verification`, `Delivery`.
- Veredicto (solo en `05-verification.md`): `PASS` / `FAIL`.
- Handoff (especialista -> orquestador, según `.claude/agents/ai-sdlc.md`): `COMPLETE`, `BLOCKED`, `FAILED`.

| Situación | `Status` | `Phase` | Veredicto / Handoff |
|---|---|---|---|
| Tarea creada, sin fase activa | `PENDING` | `Requirements` | — |
| Fase en curso | `IN_PROGRESS` | fase activa | — |
| Fase terminada con éxito | `COMPLETE` | fase terminada | Handoff `COMPLETE` |
| Verificación superada, entrega registrada | `DONE` | `Delivery` | Veredicto `PASS` |
| Pregunta bloqueante o 2 remediaciones fallidas | `BLOCKED` | fase donde se detuvo | Handoff `BLOCKED` |
| Fase que no pudo completarse | `FAILED` | fase donde falló | Handoff `FAILED` |

Reglas: `FAIL` (veredicto) devuelve a Implementación (máximo 2 remediaciones); `FAILED` (handoff) informa de una fase que no pudo completarse; `COMPLETE` describe un handoff de fase, no la tarea; la tarea solo es `DONE` en `Delivery` con veredicto `PASS` y entrega registrada.
