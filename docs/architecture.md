# Arquitectura del MVP de AI-SDLC

## 1. Intención arquitectónica

El sistema es un ciclo de control agéntico y ligero sobre Claude Code. No implementa un runtime de orquestación independiente. Claude Code proporciona el entorno de ejecución, el ciclo de herramientas, el aislamiento de contexto y el mecanismo de subagentes; los archivos del repositorio proporcionan el estado SDLC duradero.

La decisión central de diseño es hacer de `ai-sdlc` un **subagente personalizado de sesión principal**. Claude Code permite iniciar una sesión con `--agent <name>`, donde el subagente seleccionado aporta el system prompt de la sesión principal mientras `CLAUDE.md` sigue participando en el flujo normal de contexto del proyecto.

## 2. Topología

```text
Requisito del usuario
        |
        v
+-------------------------+
|       AI-SDLC           |
| orquestador de sesión principal|
+-----------+-------------+
             |
             +----> Ingeniero de requisitos
             |          |
             |          v 02-requirements.md
             |
             +----> Arquitecto de software
             |          |
             |          v 03-design.md
             |
             +----> Ingeniero de implementación
             |          |
             |          v 04-implementation.md
             |
             +----> Ingeniero de verificación
             |          |
             |      PASS|FAIL
             |          |
             |          +--------FAIL--------+
             |                             |
             |                             v
             |                    Ingeniero de implementación
             |                             |
             |                             +----> Verificación
             |
             +--------PASS----------------> Entrega

Mantenimiento / comentarios --------------------------------> Requisitos
```

## 3. Por qué cuatro agentes de trabajo

El SDLC tiene más fases conceptuales que agentes esta implementación. Esto es intencional. Los límites entre agentes deben justificarse por aislamiento de contexto, distintos permisos de herramientas y tareas de razonamiento materialmente diferentes, no por forzar un agente por cada fase de libro de texto.

| Fase | Ejecutor | Motivo |
|---|---|---|
| Requisitos | `requirements-engineer` | Descubrimiento y formalización con alta carga de lectura; sin ediciones de código |
| Diseño | `software-architect` | Análisis del repositorio más creación del artefacto de diseño |
| Implementación | `implementation-engineer` | Mayor superficie de mutación/herramientas; enfocado en la ejecución |
| Verificación | `verification-engineer` | La recolección de evidencia sin `Write`/`Edit` evita la autorreparación silenciosa |
| Entrega | `ai-sdlc` | Paso pequeño de síntesis; sin beneficio de aislamiento de contexto |
| Mantenimiento | siguiente tarea / siguiente iteración | Reingresa a Requisitos en lugar de añadir un agente de mantenimiento |

La propiedad importante de eficiencia es que la exploración verbosa, el razonamiento de diseño, los detalles de implementación y la salida de las pruebas no se acumulen en un único contexto. Claude Code plantea los subagentes específicamente como contextos aislados para trabajo secundario y gestión de contexto, y los recomienda para operaciones de alto volumen y flujos secuenciales de varios pasos.

## 4. Por qué Skills más agentes

`CLAUDE.md` contiene solo invariantes generales del repositorio porque se carga al inicio de la sesión y consume contexto en cada petición. Los procedimientos repetibles viven en las Skills, cuyos cuerpos se cargan cuando se necesitan. Cada especialista puede precargar únicamente la skill de la fase que necesita.

Esto es divulgación progresiva en dos niveles:

1. Inicio: las descripciones cortas de los subagentes están visibles para que Claude pueda elegir la delegación.
2. Invocación: el prompt completo del especialista y la skill de fase precargada solo se cargan para ese trabajador.

Las plantillas de apoyo se guardan como Markdown simple bajo `.ai-sdlc/templates/`; no se cargan de forma global.

## 5. Protocolo de artefactos duraderos

La conversación no es el sistema de registro. Cada tarea obtiene un espacio de trabajo:

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

`00-state.md` es el estado canónico del flujo de trabajo. Los demás archivos son contratos de fase. Esto hace que el ciclo sea resiliente a la compactación de contexto y fácil de inspeccionar por una persona.

Nota: los identificadores de máquina (`PENDING`, nombres de fase, `PASS`/`FAIL`, `BLOCKED`, `DONE`, nombres de fichero y de agentes) se mantienen en inglés en diagramas y artefactos.

Guía paso a paso: [`HOW-TO-USE.md`](HOW-TO-USE.md).

## 6. Máquina de estados

```text
PENDING
  -> REQUIREMENTS
  -> DESIGN
  -> IMPLEMENTATION
  -> VERIFICATION
       |
       +-- PASS --> DELIVERY --> DONE
       |
       +-- FAIL --> IMPLEMENTATION
                         ^
                         |
                  máx. 2 ciclos de remediación
                          |
                          +-- sigue fallando --> BLOCKED

La ambigüedad bloqueante puede detener el flujo en Requisitos o Diseño.
Mantenimiento/comentarios genera una nueva entrada a Requisitos.
```

El límite de dos remediaciones es una válvula de seguridad deliberada del MVP. Sin él, un ciclo agéntico de verificar/corregir puede consumir turnos ilimitados intentando satisfacer una condición fallida.

## 7. Permisos y superficie de riesgo

El orquestador solo puede generar los cuatro tipos conocidos de trabajadores. Los trabajadores de requisitos y verificación no tienen `Write`/`Edit` (verificación puede usar `Bash` para ejecutar comprobaciones, sin editar código). Los trabajadores de diseño e implementación tienen `Write`/`Edit`; el arquitecto escribe el artefacto `03-design.md` y la edición del repositorio corresponde a implementación, con Bash disponible donde los comandos del repositorio lo requieran.

No hay ningún servidor MCP configurado. No se requiere ningún plugin. No se requiere memoria persistente de agentes para el MVP; los artefactos de tarea proporcionan memoria explícita y revisable.

## 8. Por qué no equipos de agentes

El flujo de trabajo tiene un único coordinador, dependencias estrictas entre fases y poca oportunidad de concurrencia. Los equipos de agentes introducirían comunicación entre pares y una lista de tareas compartida sin resolver ninguna necesidad real aquí. Una cadena secuencial de subagentes es suficiente.

## 9. Por qué no hooks

Los hooks se omiten intencionalmente. El MVP no requiere notificaciones externas, formateo automático ni aplicación de políticas tras eventos de herramientas. Introducir hooks antes de una necesidad concreta haría el comportamiento en tiempo de ejecución menos transparente.

## 10. Por qué no hay agente de despliegue

La entrega consiste mayormente en sintetizar evidencia y preparación para la publicación. El despliegue externo real es un efecto secundario con implicaciones de autorización, por lo que permanece bajo el control del orquestador y solo se realiza cuando se solicita y autoriza explícitamente. Si un proyecto futuro tiene un proceso de publicación complejo, se puede introducir un `delivery-engineer` dedicado sin cambiar la máquina de estados central.

## 11. Comportamiento de ejecución esperado

Dada una entrada vaga como:

> “Necesito que los usuarios puedan descargar sus datos.”

AI-SDLC primero debe descubrir el comportamiento existente de usuarios/datos/exportación, convertirlo en criterios de aceptación, diseñar la solución más pequeña compatible con el repositorio, implementarla, ejecutar la verificación, remediar como máximo dos veces si es necesario y terminar con `06-delivery.md`.

El agente solo debe detenerse para una decisión del usuario cuando la información faltante cambie la aceptación de negocio, genere una arquitectura materialmente distinta o requiera autorización explícita de despliegue.
