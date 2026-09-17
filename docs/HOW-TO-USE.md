# Cómo usar AI-SDLC

Guía práctica para ejecutar tu primera tarea con AI-SDLC en Claude Code. Si buscas el fundamento (topología, máquina de estados, decisiones de diseño), consulta [`architecture.md`](architecture.md).

## 1. Objetivo y audiencia

AI-SDLC convierte un requisito vago en un cambio validado e integrado en tu repositorio, siguiendo un ciclo acotado:

Requisitos -> Diseño -> Implementación -> Verificación -> Entrega.

Está dirigido a desarrolladores que ya usan Claude Code y tienen esta estructura instalada en la raíz de su repositorio.

## 2. Requisitos previos

- Claude Code instalado y funcional.
- Esta estructura copiada en la raíz del repositorio destino (`CLAUDE.md`, `.claude/`, `.ai-sdlc/`).
- Un repositorio con comandos de construcción/prueba detectables (el agente los reutiliza; no necesitas configurarlos para el MVP).

## 3. Instalación en el repositorio destino

1. Copia `CLAUDE.md`, `.claude/` y `.ai-sdlc/` en la raíz del repositorio destino.
2. Verifica que existen estas rutas:
   - `.claude/agents/ai-sdlc.md` (orquestador).
   - `.claude/agents/requirements-engineer.md`, `software-architect.md`, `implementation-engineer.md`, `verification-engineer.md` (especialistas).
   - `.claude/skills/sdlc-orchestration/SKILL.md` (flujo canónico).
   - `.ai-sdlc/templates/` (contratos de fase).
3. No necesitas instalar ningún servidor MCP, plugin, base de datos ni runtime adicional.

## 4. Iniciar una sesión

Inicia Claude Code como agente principal AI-SDLC:

```powershell
claude --agent ai-sdlc
```

Después proporciónale un requisito en lenguaje natural. Puedes pasarlo directamente tras el comando o escribirlo en el chat. Ejemplo:

> Añade un endpoint que devuelva el perfil del usuario actual.

El agente crea un espacio de trabajo en `.ai-sdlc/tasks/<task-id>/` y orquesta a los especialistas en orden.

## 5. Cómo escribir un buen requisito

Describe comportamiento observable y contexto, no implementación. Incluye qué debe ocurrir, para quién y qué queda fuera si es ambiguo.

Buen ejemplo:

> Necesito que los usuarios puedan descargar sus datos en CSV desde su página de perfil. Solo sus propios datos. No incluye programación de exportaciones.

Ejemplo pobre (evítalo):

> Mejora los datos.

Si falta información que cambie la aceptación de negocio, el agente se detiene y te pregunta antes de implementar. No inventa requisitos.

## 6. Qué ocurre en cada fase

| Fase | Ejecutor | Artefacto | Qué obtienes |
|---|---|---|---|
| Requisitos | `requirements-engineer` | `02-requirements.md` | Problema, alcance, supuestos y criterios de aceptación verificables |
| Diseño | `software-architect` | `03-design.md` | Componentes afectados, cambio propuesto y plan de implementación |
| Implementación | `implementation-engineer` | `04-implementation.md` | Código cambiado, llamadas actualizadas y pruebas relevantes |
| Verificación | `verification-engineer` | `05-verification.md` | Evidencia objetiva (`PASS`/`FAIL`) mapeada a los criterios (no corrige código en silencio) |
| Entrega | `ai-sdlc` | `06-delivery.md` | Resumen del cambio, evidencia, riesgos residuales y estado de despliegue |

El ciclo completo y las reglas de compuerta están definidos en la skill `sdlc-orchestration`. Los especialistas no se coordinan entre ellos; el orquestador les pasa los artefactos. Los de solo lectura (requisitos, diseño, verificación) devuelven su resultado como handoff y el orquestador persiste el artefacto; solo `implementation-engineer` muta el repositorio y escribe su propio `04-implementation.md`.

## 7. Dónde mirar los resultados

Cada tarea tiene su espacio de trabajo:

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

- `01-request.md` conserva tu petición original y la intención normalizada.
- `00-state.md` es el estado canónico del flujo (fase activa, artefactos, intentos de remediación).
- Los demás archivos son los contratos de cada fase.
- La conversación no es el sistema de registro: si la sesión se compacta o se reinicia, estos archivos conservan el estado.

## 8. Verificación y remediación

- `PASS` avanza a Entrega y la tarea termina como `DONE`.
- `FAIL` devuelve a Implementación con la evidencia del fallo.
- El ciclo admite como máximo **2 remediaciones** por tarea. Si sigue fallando, la tarea se marca `BLOCKED` y el agente te pide dirección.
- Los trabajadores de requisitos, diseño y verificación son de solo lectura sobre el repositorio (sin `Write`/`Edit`; la verificación usa `Bash` solo para ejecutar comprobaciones): devuelven su resultado como handoff y el orquestador persiste el artefacto. La verificación informa fallos, no corrige código en silencio.

## 9. Cuándo se detiene a preguntar

El agente no te interrumpe por decisiones rutinarias de implementación. Solo se detiene cuando:

- Un requisito de negocio es genuinamente ambiguo y afecta la aceptación.
- Una decisión arquitectónica de alto impacto tiene varias soluciones válidas y materialmente distintas.
- Un despliegue a un destino externo requiere tu autorización explícita.

## 10. Ejemplo mínimo end-to-end

1. Inicias la sesión:

   ```powershell
   claude --agent ai-sdlc
   ```

2. Pides:

   > Añade un endpoint que devuelva el perfil del usuario actual.

3. El agente crea `.ai-sdlc/tasks/<task-id>/`, descubre el comportamiento existente de usuarios/perfil, escribe `02-requirements.md` con criterios de aceptación, diseña el cambio mínimo en `03-design.md`, lo implementa, ejecuta las pruebas del repositorio y registra la evidencia en `05-verification.md`.
4. Si la verificación pasa, recibes `06-delivery.md` con el resumen, los archivos cambiados y los riesgos residuales. El despliegue externo solo ocurre si lo autorizas explícitamente.

## 11. Problemas frecuentes

- **La tarea queda `BLOCKED` en Requisitos o Diseño:** falta una decisión de negocio o hay un conflicto arquitectónico. Responde la pregunta del agente con la información concreta que pide.
- **La verificación falla dos veces:** la tarea queda `BLOCKED`. Revisa `05-verification.md`, decide si ajustar el requisito o el diseño, y el nuevo intento entra como iteración o tarea nueva (Mantenimiento -> Requisitos).
- **No encuentro el estado:** abre `.ai-sdlc/tasks/<task-id>/00-state.md`; es la fuente canónica, no el historial del chat.
- **El cambio toca más archivos de lo esperado:** el diseño debe actualizar todas las llamadas afectadas y eliminar código obsoleto en lugar de mantener rutas duplicadas.

## 12. Siguientes pasos

- Vuelve al [README principal](../README.md) para la definición completa del sistema.
- Lee [`architecture.md`](architecture.md) para la topología, la máquina de estados, los límites de permisos y por qué el diseño es intencionalmente pequeño.
- El feedback, los defectos o los cambios nuevos entran como otra fase de Requisitos: Mantenimiento alimenta el siguiente ciclo.
