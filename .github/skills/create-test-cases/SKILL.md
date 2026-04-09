---
name: create-test-cases
description: "Crea sub-issues de casos de prueba dentro de una historia de usuario (HU). Usa /create-test-cases seguido del identificador de la HU y la matriz de casos de prueba en formato markdown."
---

# Skill: Crear Casos de Prueba desde Matriz

## Uso

```
/create-test-cases

HU: [número del issue de la HU, ej: #5]

| HU | ID | Escenario Gherkin | Precondiciones | Datos de prueba | Resultado esperado | Resultado obtenido y Estado | Prioridad |
|----|----|-------------------|----------------|-----------------|--------------------|-----------------------------|-----------|
| HU-XX | TC-XXX | Given... When... Then... | ... | ... | ... | Resultado obtenido: Sin ejecutar; Estado: Sin ejecutar | Crítico |
```

## Flujo

1. El usuario invoca `/create-test-cases` con el número de issue de la HU y la matriz de casos de prueba completa en markdown.
2. El agente parsea la matriz y extrae cada caso de prueba (fila).
3. El agente busca el issue de la HU padre en el repositorio.
4. El agente crea una sub-issue **"Casos de prueba"** dentro de la HU padre.
5. Dentro de la sub-issue "Casos de prueba", el agente crea una sub-issue **por cada caso de prueba** (cada fila de la matriz).
6. El agente confirma con los links a todos los issues creados.

## Estructura jerárquica resultante

```
HU - [nombre] #N (issue padre existente)
  └── Casos de prueba #X (sub-issue creada)
        ├── TC-001 #A (sub-issue creada)
        ├── TC-002 #B (sub-issue creada)
        └── TC-XXX #C (sub-issue creada)
```

## Formato de entrada: Matriz de Casos de Prueba

La entrada es una tabla markdown con las siguientes columnas obligatorias:

| Columna | Descripción |
|---------|-------------|
| **HU** | Identificador de la historia de usuario (ej: HU-01) |
| **ID** | Identificador del caso de prueba (ej: TC-001) |
| **Escenario Gherkin** | Escenario en formato Given/When/Then |
| **Precondiciones** | Condiciones previas necesarias para ejecutar el caso |
| **Datos de prueba** | Datos de entrada necesarios |
| **Resultado esperado** | Resultado que se espera obtener |
| **Resultado obtenido y Estado** | Inicialmente: "Resultado obtenido: Sin ejecutar; Estado: Sin ejecutar" |
| **Prioridad** | Nivel de prioridad: Crítico, Alto, Medio, Bajo |

## Formato del Issue "Casos de prueba" (sub-issue agrupadora)

**Título**: `Casos de prueba`

**Body**:
```markdown
Esta subtarea agrupa los casos de prueba de la historia de usuario
```

El body es siempre ese texto fijo. NO incluir la matriz completa aquí.

**Label**: `test-cases`

## Formato de cada Issue de Caso de Prueba (sub-issue individual)

**Título**: `[ID del caso de prueba]` (ej: `TC-001`)

**Body**:
El body de cada caso de prueba es una **tabla markdown de una sola fila** con las mismas columnas de la matriz original. Se debe copiar EXACTAMENTE la fila correspondiente del usuario.

```markdown
| HU | ID | Escenario Gherkin | Precondiciones | Datos de prueba | Resultado esperado | Resultado obtenido y Estado | Prioridad |
|----|----|-------------------|----------------|-----------------|--------------------|-----------------------------|-----------|
| HU-XX | TC-XXX | **Given** ... **When** ... **Then** ... | ... | ... | ... | Resultado obtenido: Sin ejecutar; Estado: Sin ejecutar | Crítico |
```

NO usar formato estructurado con secciones. NO agregar checklist ni secciones extra. Solo la tabla con header y la fila del caso.

**Label**: `test-case`

## Ejemplo completo

**Input:**
```
/create-test-cases

HU: #5

| HU | ID | Escenario Gherkin | Precondiciones | Datos de prueba | Resultado esperado | Resultado obtenido y Estado | Prioridad |
|----|----|-------------------|----------------|-----------------|--------------------|-----------------------------|-----------|
| HU-01 | TC-001 | **Given** un administrador con sesión activa **When** cierra su sesión desde el Dashboard **Then** el sistema finaliza la sesión y cualquier intento de acceder nuevamente con esa sesión es rechazado | Usuario con sesión autenticada activa | Sesión válida previamente iniciada | Sesión cerrada; acceso posterior con la misma sesión denegado | Resultado obtenido: Sin ejecutar; Estado: Sin ejecutar | Crítico |
| HU-01 | TC-002 | **Given** un usuario no autenticado **When** intenta acceder al Dashboard **Then** el sistema redirige al login | Ninguna | URL del Dashboard | Redirección a página de login | Resultado obtenido: Sin ejecutar; Estado: Sin ejecutar | Alto |
```

**Output esperado:**
1. Sub-issue "Casos de prueba" creada bajo HU #5
2. Sub-issue "TC-001" creada bajo "Casos de prueba"
3. Sub-issue "TC-002" creada bajo "Casos de prueba"
4. Links a todos los issues creados

## Reglas

- **NO inventar ni modificar datos de los casos de prueba**. Usar EXACTAMENTE lo que el usuario proporciona en la matriz.
- **NO modificar la redacción** de los escenarios Gherkin, precondiciones, datos de prueba ni resultados esperados.
- Si la matriz está incompleta o le faltan columnas → pedir lo que falta antes de crear los issues.
- Si ya existe una sub-issue "Casos de prueba" en la HU → preguntar al usuario si desea agregar los nuevos casos a la existente o crear una nueva.
- **Verificar que el issue de la HU padre existe** antes de crear cualquier sub-issue.
- Cada caso de prueba (cada fila de la matriz) se convierte en una sub-issue individual dentro de "Casos de prueba".
- El label `test-cases` se aplica a la sub-issue agrupadora y `test-case` a cada caso individual.
