---
name: create-issue
description: "Crea un issue en GitHub a partir de una historia de usuario (HU). Usa /create-issue seguido de la HU completa con criterios de aceptación."
---

# Skill: Crear Issue desde Historia de Usuario

## Uso

```
/create-issue

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

## Flujo

1. El usuario invoca `/create-issue` con la HU completa.
2. El agente **Backlog Manager** lee `.github/backlog-config.yml` para obtener `owner`, `repo` y `project_name`.
3. El agente parsea la HU y genera el issue en el repositorio configurado.
4. El agente confirma con el link al issue creado.

## Ejemplo

**Input:**
```
/create-issue

Como [rol del proyecto],
Quiero iniciar sesión y cerrar sesión,
Para acceder de forma segura al dashboard.

Criterios de aceptación

Scenario: Inicio de sesión exitoso
Given: que existe un usuario registrado en el sistema
When: Intenta ingresar con credenciales válidas
Then: El sistema le otorga el acceso
And: debe redirigir el usuario al Dashboard correspondiente según el rol con todas las funciones pertinentes

Scenario: Intento de inicio de sesión con credenciales erróneas
Given: que existe un usuario registrado en el sistema
When: Intenta ingresar con credenciales inválidas
Then: El sistema debe rechazar el ingreso de sesión
And: Muestra un mensaje de credenciales inválidas
```

**Output esperado:**
- Issue creado: `HU - Iniciar sesión y cerrar sesión`
- Labels: `user-story`
- Link al issue en GitHub

## Formato del Issue generado

```markdown
## Historia de Usuario

**Como** [rol del proyecto],
**Quiero** iniciar sesión y cerrar sesión,
**Para** acceder de forma segura al dashboard.

---

## Criterios de Aceptación

### Scenario: Inicio de sesión exitoso
- **Given**: que existe un usuario registrado en el sistema
- **When**: Intenta ingresar con credenciales válidas
- **Then**: El sistema le otorga el acceso
- **And**: debe redirigir el usuario al Dashboard correspondiente según el rol con todas las funciones pertinentes

### Scenario: Intento de inicio de sesión con credenciales erróneas
- **Given**: que existe un usuario registrado en el sistema
- **When**: Intenta ingresar con credenciales inválidas
- **Then**: El sistema debe rechazar el ingreso de sesión
- **And**: Muestra un mensaje de credenciales inválidas

---

## Checklist
- [ ] Criterios de aceptación revisados
- [ ] Issue priorizado en el backlog
```

## Reglas

- NO inventar ni modificar criterios. Usar EXACTAMENTE lo que el usuario proporciona.
- Si la HU está incompleta → pedir lo que falta antes de crear el issue.
- Si ya existe un issue similar → avisar al usuario antes de crear duplicado.
