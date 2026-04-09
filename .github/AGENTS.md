# AGENTS.md — Backlog Manager

> Fuente de verdad para agentes de este repositorio.

Este repositorio contiene un agente de GitHub Copilot para gestionar el backlog de un proyecto, creando issues en GitHub a partir de historias de usuario (HU).

## Configuración

> Los valores de organización, repositorio y proyecto se configuran en `.github/backlog-config.yml`.

El agente lee `owner`, `repo`, `project_number` y `project_name` de ese archivo antes de interactuar con la API de GitHub.

## Flujo de trabajo

### Crear issue desde HU
```
[Usuario proporciona HU] → [Backlog Manager parsea] → [Crea issue en GitHub] → [Issue aparece en el backlog]
```

### Crear casos de prueba desde matriz
```
[Usuario proporciona matriz de TC] → [Backlog Manager parsea] → [Crea sub-issue "Casos de prueba" en la HU] → [Crea sub-issues TC-XXX dentro de "Casos de prueba"]
```

Jerarquía resultante:
```
HU - [nombre] #N
  └── Casos de prueba #X
        ├── TC-001 #A
        ├── TC-002 #B
        └── TC-XXX #C
```

## Agentes disponibles

| Agente | Descripción |
|--------|-------------|
| **Backlog Manager** | Crea issues en GitHub a partir de historias de usuario |

## Skills (slash commands)

| Skill | Slash Command | Descripción |
|-------|---------------|-------------|
| create-issue | `/create-issue` | Crea un issue en GitHub desde una HU con criterios de aceptación |
| create-test-cases | `/create-test-cases` | Crea sub-issues de casos de prueba dentro de una HU a partir de una matriz markdown |
| move-to-done | `/move-to-done` | Cierra todos los issues abiertos del repositorio, moviéndolos a Done en el Project Board |

## Formato de Historia de Usuario

```
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

## Formato de Matriz de Casos de Prueba

```
/create-test-cases

HU: #[número del issue]

| HU | ID | Escenario Gherkin | Precondiciones | Datos de prueba | Resultado esperado | Resultado obtenido y Estado | Prioridad |
|----|----|-------------------|----------------|-----------------|--------------------|-----------------------------|-----------|
| HU-XX | TC-XXX | Given... When... Then... | ... | ... | ... | Resultado obtenido: Sin ejecutar; Estado: Sin ejecutar | Crítico |
```

---

## Reglas

1. **NO inventar criterios de aceptación** — usar exactamente lo que proporciona el usuario.
2. **NO modificar la redacción** de la HU del usuario.
3. **Verificar duplicados** antes de crear un issue nuevo.
4. **Si la HU es incompleta** → preguntar al usuario antes de crear el issue.
5. **Label obligatorio**: `user-story` en cada issue creado.
