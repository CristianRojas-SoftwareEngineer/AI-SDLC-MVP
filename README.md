# AI-SDLC MVP para Claude Code

Un arnés SDLC mínimo y agéntico para Claude Code. Convierte un requisito de desarrollo vago en un ciclo acotado de Requisitos -> Diseño -> Implementación -> Verificación -> Entrega, con Mantenimiento alimentando el siguiente ciclo.

## Qué es

AI-SDLC es un ciclo de control ligero sobre Claude Code: el orquestador `ai-sdlc` recibe tu requisito, delega cada fase a un especialista aislado y persiste el estado en archivos Markdown bajo `.ai-sdlc/tasks/<task-id>/`. La conversación no es el sistema de registro; los artefactos sí lo son y sobreviven a la compactación de contexto o a una nueva sesión.

Un ciclo típico: descubre el comportamiento existente, lo convierte en criterios de aceptación, diseña el cambio mínimo compatible con el repositorio, lo implementa, lo verifica con los comandos del propio repositorio, remedia como máximo dos veces si falla y termina con `06-delivery.md`.

## Qué no es

- No es un runtime de orquestación independiente ni un framework.
- No requiere servidor MCP, orquestador externo, base de datos, runtime personalizado ni equipo de agentes.
- No usa hooks (sin notificaciones externas, formateo automático ni políticas tras eventos de herramientas).
- No incluye agente de despliegue: el despliegue externo solo ocurre bajo control del orquestador y con tu autorización explícita.

## Componentes

- **Orquestador `ai-sdlc`:** dueño de la orquestación y del estado canónico (`00-state.md`). Ejecuta las fases en orden y aplica el límite de 2 remediaciones.
- **`requirements-engineer`:** descubre y formaliza el requisito (solo lectura, no edita código). Produce `02-requirements.md`.
- **`software-architect`:** convierte los requisitos aprobados en el diseño mínimo coherente. Produce `03-design.md`.
- **`implementation-engineer`:** implementa el diseño, actualiza las llamadas afectadas y elimina código obsoleto. Produce `04-implementation.md`.
- **`verification-engineer`:** valida con evidencia objetiva usando los comandos del repositorio (sin `Write`/`Edit`, no corrige en silencio). Produce `05-verification.md` con `PASS`/`FAIL`.
- **Skills por fase:** los procedimientos repetibles viven en `.claude/skills/` (`sdlc-orchestration`, `requirements`, `design`, `implementation`, `verification`) y se cargan solo cuando se necesitan.
- **Plantillas:** contratos Markdown simples en `.ai-sdlc/templates/`; no se cargan de forma global.
- **Espacios de trabajo:** cada tarea vive en `.ai-sdlc/tasks/<task-id>/` con `00-state.md`, `01-request.md`, `02-requirements.md`, `03-design.md`, `04-implementation.md`, `05-verification.md` y `06-delivery.md`.

## Requisitos previos

- Claude Code instalado y funcional.
- Esta estructura copiada en la raíz del repositorio destino (`CLAUDE.md`, `.claude/`, `.ai-sdlc/`).
- Un repositorio con comandos de construcción/prueba detectables (el agente reutiliza tu infraestructura existente).

## Instalación

1. Copia esta estructura en la raíz del repositorio destino.
2. Inicia Claude Code como agente principal AI-SDLC:

```powershell
claude --agent ai-sdlc
```

3. Proporciónale un requisito como, por ejemplo:

> Añade un endpoint que devuelva el perfil del usuario actual.

El agente crea un espacio de trabajo para la tarea en `.ai-sdlc/tasks/<task-id>/` y orquesta a los especialistas.

## Ejemplo mínimo

```powershell
claude --agent ai-sdlc
```

> Necesito que los usuarios puedan descargar sus datos en CSV desde su página de perfil. Solo sus propios datos.

Resultado esperado: `02-requirements.md` con criterios de aceptación, `03-design.md` con el cambio mínimo, implementación integrada en tu árbol de trabajo, `05-verification.md` con evidencia `PASS` y `06-delivery.md` con resumen, archivos cambiados y riesgos residuales. Sin despliegue salvo autorización explícita.

## Mapa de artefactos

```text
.ai-sdlc/tasks/<task-id>/
├── 00-state.md
├── 01-request.md
├── 02-requirements.md
├── 03-design.md
├── 04-implementation.md
├── 05-verification.md
└── 06-delivery.md
```

`00-state.md` es el estado canónico del flujo. Si la verificación falla, el ciclo vuelve a Implementación (máximo 2 veces) y luego marca la tarea `BLOCKED` para pedirte dirección.

## Documentación

- Guía paso a paso: [`docs/HOW-TO-USE.md`](docs/HOW-TO-USE.md) (requisitos previos, cómo escribir un buen requisito, qué ocurre en cada fase, ejemplo end-to-end y problemas frecuentes).
- Fundamento de la arquitectura: [`docs/architecture.md`](docs/architecture.md) (topología, máquina de estados, límites de permisos y decisiones de diseño).

## Por qué el diseño es intencionalmente pequeño

El agente principal es dueño de la orquestación y del estado. Los subagentes son especialistas aislados, elegidos por eficiencia de contexto y enfoque. Las skills contienen los procedimientos reutilizables de cada fase y pueden cargarse de forma progresiva en lugar de sobrecargar el archivo `CLAUDE.md`.

No se requiere ningún servidor MCP, orquestador externo, base de datos, runtime personalizado ni equipo de agentes.

## Topología de agentes

```text
                        +-------------------+
                        |      AI-SDLC      |
                        | main orchestrator |
                        +---------+---------+
                                   |
              +--------------------+--------------------+
              |                    |                    |
              v                    v                    v
      requirements-engineer  software-architect  implementation-engineer
                                                         |
                                                         v
                                                verification-engineer
                                                         |
                               PASS --------------------+---- FAIL
                                |                             |
                                v                             v
                             Delivery                 Implementation
                                                           |
                                                           +--> Verify
```

El cuarto especialista participa intencionalmente en un ciclo acotado de remediación en lugar de convertirse en un segundo orquestador.

## Detalle importante de Claude Code

Los nombres de los subagentes de Claude Code deben usar minúsculas y guiones. Por tanto, el identificador técnico es `ai-sdlc`, mientras que el rol se denomina `AI-SDLC` en el system prompt y en la documentación.

## Nota de versión

Este MVP está dirigido a las capacidades actuales de Claude Code a nivel de proyecto: `CLAUDE.md`, `.claude/agents/*.md` y `.claude/skills/*/SKILL.md`. Claude Code evoluciona con frecuencia, así que valida la sintaxis contra la versión instalada antes de distribuir el sistema de forma amplia.
