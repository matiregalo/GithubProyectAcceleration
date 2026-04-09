---
name: move-to-done
description: "Cierra todos los issues abiertos de un repositorio marcándolos como completados, lo que los mueve a Done en el Project Board. Usa /move-to-done opcionalmente con filtros de labels."
argument-hint: "Opcionalmente especifica labels para filtrar (ej: user-story, test-cases)"
---

# Skill: Mover todos los issues a Done

## Uso

```
/move-to-done
```

O con filtro por labels:
```
/move-to-done labels: user-story, test-cases
```

## Flujo

1. El agente lee `.github/backlog-config.yml` para obtener `owner` y `repo`.
2. El agente lista TODOS los issues abiertos del repositorio usando `mcp_io_github_git_list_issues`.
3. Si el usuario especificó labels, filtra solo los issues que tengan esos labels.
4. Para cada issue abierto, el agente lo cierra con `state: closed` y `state_reason: completed` usando `mcp_io_github_git_issue_write` con método `update`.
5. El agente confirma al usuario cuántos issues se cerraron y lista sus títulos.

> **Nota**: Si el Project Board tiene activo el workflow "Item closed → Move to Done" (habilitado por defecto en GitHub Projects), los issues cerrados se moverán automáticamente a la columna Done.

## Proceso detallado

### Paso 1: Leer configuración

Leer `.github/backlog-config.yml` y extraer `owner` y `repo`.

### Paso 2: Obtener todos los issues abiertos

Usar `mcp_io_github_git_list_issues` con:
- `owner`: del config
- `repo`: nombre del repositorio (solo el nombre, sin el owner)
- `state`: `OPEN`
- `perPage`: `100`

Si hay más de 100 issues, paginar usando el `endCursor` del response en el parámetro `after` hasta obtener todos.

Si el usuario proporcionó labels, usar el parámetro `labels` para filtrar.

### Paso 3: Cerrar cada issue

Para cada issue obtenido, usar `mcp_io_github_git_issue_write` con:
- `method`: `update`
- `owner`: del config
- `repo`: nombre del repositorio (solo el nombre, sin el owner)
- `issue_number`: número del issue
- `state`: `closed`
- `state_reason`: `completed`

### Paso 4: Confirmar

Mostrar al usuario:
- Cantidad total de issues cerrados
- Lista con número y título de cada issue cerrado

## Ejemplo

**Input:**
```
/move-to-done
```

**Output esperado:**
```
Se cerraron 5 issues como completados:

- #1 HU-01 - Inicio y cierre de sesión (Auth Management)
- #2 HU-02 - Gestión de Usuarios por Ecommerce
- #3 HU-03 - Administra su Ecommerce
- #4 HU-04 - Usuario Estándar
- #5 HU-05 - ...

Estos issues se moverán automáticamente a Done en el Project Board si el workflow "Item closed" está activo.
```

## Ejemplo con filtro

**Input:**
```
/move-to-done labels: user-story
```

**Output esperado:**
```
Se cerraron 3 issues con label "user-story" como completados:

- #1 HU-01 - Inicio y cierre de sesión (Auth Management)
- #2 HU-02 - Gestión de Usuarios por Ecommerce
- #3 HU-03 - Administra su Ecommerce
```

## Reglas

- **Confirmar antes de ejecutar**: Antes de cerrar los issues, mostrar la lista de issues que se van a cerrar y pedir confirmación al usuario.
- **Solo issues abiertos**: No intentar cerrar issues que ya están cerrados.
- **Extraer nombre del repo correctamente**: Si `repo` en el config tiene formato `owner/repo`, usar solo la parte después del `/`.
- **Paginar si es necesario**: Si hay más de 100 issues abiertos, recorrer todas las páginas.
- **No modificar el body ni labels**: Solo cambiar el estado del issue a closed/completed.
