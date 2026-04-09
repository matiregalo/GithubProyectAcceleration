# Backlog Manager

Agente de GitHub Copilot que convierte **historias de usuario (HU)** en **issues de GitHub** y los agrega al backlog de un proyecto.

## Requisitos

| Requisito | Detalle |
|---|---|
| VS Code | Cualquier versión reciente |
| GitHub Copilot Chat | Extensión instalada y activa |
| MCP GitHub | Servidor MCP de GitHub configurado |
| Setting habilitado | `github.copilot.chat.codeGeneration.useInstructionFiles: true` |

## Configuración

Antes de usar el agente, editá `.github/backlog-config.yml` con los datos de tu organización, repositorio y proyecto:

```yaml
owner: "TU_ORGANIZACION_O_USUARIO"
repo: "TU_REPOSITORIO"
project_number: 0
project_name: "MI PROYECTO"
```

## Cómo Usar

### Opción 1 — Invocar al agente directamente

```
@Backlog Manager

Como [rol],
Quiero [acción],
Para [beneficio].

Criterios de aceptación

Scenario: [nombre]
Given: [precondición]
When: [acción]
Then: [resultado esperado]
And: [expectativa adicional]
```

### Opción 2 — Usar el slash command

```
/create-issue

Como [rol],
Quiero [acción],
Para [beneficio].
...
```

## Formato de Historia de Usuario

```
Como [rol],
Quiero [acción],
Para [beneficio].

Criterios de aceptación

Scenario: [nombre del escenario]
Given: [precondición]
When: [acción del usuario]
Then: [resultado esperado]
And: [expectativa adicional]
```

## Estructura del Repositorio

```
.github/
├── AGENTS.md                          # Reglas y configuración general
├── README.md                          # Esta guía
├── backlog-config.yml                 # Configuración de org/repo/proyecto
├── copilot-instructions.md            # Instrucciones para Copilot
├── agents/
│   └── backlog-manager.agent.md       # Agente Backlog Manager
└── skills/
    ├── create-issue/
    │   └── SKILL.md                   # Skill /create-issue
    └── create-test-cases/
        └── SKILL.md                   # Skill /create-test-cases
```

## Proyecto GitHub

> Los valores de conexión se configuran en `.github/backlog-config.yml`.

---

## Reglas de Oro

1. **No inventar criterios de aceptación** — usar exactamente lo que proporciona el usuario.
2. **No modificar la redacción** de la HU del usuario.
3. **No código no autorizado** — los agentes no generan ni modifican código sin instrucción explícita.
4. **No suposiciones** — si el requerimiento es ambiguo, el agente pregunta antes de actuar.
5. **Transparencia** — el agente explica qué va a hacer antes de hacerlo.
