# SPEC-001: Gestión de tareas (Sprint 1)

| Campo | Valor |
|-------|-------|
| Proyecto | TaskFlow Lite |
| Estado | Borrador para revisión |
| Tipo | Funcional |
| Sprint | 1 |
| Historias incluidas | HU-001, HU-002, HU-004, HU-005 |
| Depende de | SPEC-000-foundation (debe estar aprobada) |
| Enmienda | SPEC-000, restricciones 2 y 6 (ver §3) |

---

## 1. Propósito

Definir cómo se implementa en el Sprint 1 el ciclo básico de tareas de TaskFlow Lite: registrar, visualizar, conservar entre sesiones y editar tareas. Es la fuente única de decisiones técnicas y de diseño para esas cuatro historias. Los requisitos de negocio (criterios de aceptación y reglas de negocio) viven en las HU y **esta SPEC no los modifica**.

---

## 2. Alcance

### Incluido

- HU-001: Crear una tarea.
- HU-002: Visualizar tareas (versión vigente, ver D-02).
- HU-004: Persistir tareas.
- HU-005: Editar tarea.

### Excluido (Sprint 2 u otros)

- HU-003: Actualizar estado de una tarea. No se construye ningún control para cambiar el estado.
- Eliminar tareas.
- Todo lo declarado "Fuera de Alcance" en cada HU.

---

## 3. Relación con SPEC-000

SPEC-001 hereda de SPEC-000 la estructura, las convenciones de nombres, las convenciones de Git y la Definition of Done, y las extiende. Lo no mencionado aquí se rige por SPEC-000.

### 3.1 Excepción aprobada: Tailwind CSS por CDN (D-01, opción B)

| Restricción de SPEC-000 | Tratamiento en SPEC-001 |
|---|---|
| R2: sin frameworks ni librerías de interfaz | Excepción única: se permite **Tailwind CSS** como librería de estilos. No se permite ninguna otra. |
| R6: sin dependencias externas ni CDN | Excepción única: se permite cargar Tailwind desde su CDN. |
| R5: sin herramientas de compilación | Se cumple. Tailwind Play CDN compila en el navegador, sin paso de build, sin Node y sin `tailwind.config` externo. |

Condiciones de la excepción:

1. La versión queda fijada en `index.html`: `https://cdn.tailwindcss.com/3.4.17`. No se usa una URL sin versión.
2. `src/css/styles.css` se mantiene para ajustes mínimos que no convenga expresar con clases de Tailwind.
3. Consecuencia aceptada: la aplicación necesita conexión a internet para cargar Tailwind.
4. Esta excepción **no** autoriza otras librerías, ni JavaScript externo, ni fuentes externas.
5. Al aprobarse SPEC-001, SPEC-000 se marca como "Enmendada por SPEC-001" en su cabecera (tarea T-01).

---

## 4. Decisiones adoptadas

### 4.1 Decisiones aprobadas

| ID | Decisión |
|----|----------|
| D-01 | Tailwind por CDN con excepción registrada (§3.1). |
| D-02 | De HU-002 solo es válida la segunda versión ("Visualizar tareas", 8 CA). El borrador inicial (con "responsable" y "Sin Asignar") se elimina del archivo. |
| D-03 | Estados oficiales: **pendiente, en proceso, finalizada** (nombres de las HU). Ver §5.2. |
| D-04 | El modelo y la lista soportan los 3 estados. Los CA que requieren estados distintos de pendiente se verifican con **datos semilla** (§12.2). No se construye interfaz de cambio de estado. |
| D-05 | Fecha límite: campo opcional persistido, no se captura al crear, no se muestra en la lista, solo se edita en el formulario de edición. Se aceptan fechas pasadas. |
| D-06 | El formulario de edición puede cerrarse sin guardar, descartando lo escrito. Lo excluido en HU-005 se interpreta como "deshacer una edición ya guardada". |
| D-07 | Tratamiento de datos guardados ilegibles o inválidos (§9.5). |

### 4.2 Supuestos aprobados

| ID | Supuesto |
|----|----------|
| A-01 | Título y descripción se recortan (`trim`) antes de validar. Un título de solo espacios cuenta como vacío. |
| A-02 | Una descripción vacía tras el `trim` equivale a "sin descripción". |
| A-03 | Límites inclusivos: título de 3 a 80 caracteres, descripción de 0 a 300. |
| A-04 | `id` con `crypto.randomUUID()` (disponible en `localhost`). `createdAt` en ISO 8601. |
| A-05 | Si dos tareas tienen el mismo `createdAt`, la insertada después se muestra primero. |
| A-06 | Todo texto escrito por la persona usuaria se inserta con `textContent`, nunca con `innerHTML`. |
| A-07 | Formato de almacenamiento: arreglo JSON bajo la clave `taskflow-lite:tasks`. |
| A-08 | Un único SPEC-001 para todo el sprint. |
| A-09 | Rama `feature/SPEC-001-gestion-de-tareas`; commits en español según SPEC-000. |
| A-10 | Las HU se guardan en `docs/stories/`, lo que amplía la estructura de SPEC-000 (§11). |

### 4.3 Detalles de diseño nuevos (a revisar al aprobar esta SPEC)

Estos puntos no estaban en las HU ni en D-01 a D-07. Los incluyo para que la especificación sea completa y puedes cambiarlos.

| ID | Detalle | Fundamento |
|----|---------|------------|
| N-01 | La edición se hace **en línea**: el botón "Editar" de una tarea reemplaza esa tarjeta por el formulario de edición. | HU-005 pide seleccionar la tarea desde la lista. No exige modal ni otra página. |
| N-02 | Solo puede haber **un formulario de edición abierto** a la vez. Abrir otro cierra el actual descartando lo escrito. | Evita estados ambiguos y mantiene CA-013 simple. |
| N-03 | Guardar una edición **sin cambios** es válido y no genera error. | Ninguna HU lo prohíbe. |
| N-04 | Un registro guardado con `id` repetido se trata como inválido: se conserva el primero y se descartan los repetidos. | HU-004 CA-001 exige "sin duplicados". |
| N-05 | Si falla la escritura en `localStorage`, el error se captura para que la app no se rompa y se muestra un aviso genérico. No se define manejo de almacenamiento lleno. | Fuera de alcance en HU-004, pero la app no debe fallar. |
| N-06 | Los campos de texto **no** usan el atributo `maxlength`, para que las validaciones de longitud máxima puedan mostrarse (CA-004, CA-006, CA-007, CA-008). | Un `maxlength` impediría escribir de más y los CA no serían verificables. |

---

## 5. Modelo de datos

### 5.1 Entidad `Task`

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `id` | string | Sí | Identificador único. Se genera una sola vez y no cambia (HU-001 RN-006, HU-005 RN-007). |
| `title` | string | Sí | 3 a 80 caracteres tras `trim`. |
| `description` | string | Sí (puede ser `""`) | 0 a 300 caracteres. `""` significa "sin descripción". |
| `status` | string | Sí | Uno de los valores de §5.2. |
| `createdAt` | string | Sí | Fecha y hora de creación en ISO 8601. No cambia nunca (HU-001 RN-007). |
| `dueDate` | string o `null` | Sí | Fecha límite `YYYY-MM-DD`, o `null` si no tiene. Ver §6.3. |

### 5.2 Estados (D-03)

| Valor interno | Etiqueta visible | Origen en Sprint 1 |
|---------------|------------------|--------------------|
| `pending` | Pendiente | Estado inicial de toda tarea nueva (HU-001 RN-005). |
| `in-progress` | En proceso | Solo mediante datos semilla (D-04). |
| `done` | Finalizada | Solo mediante datos semilla (D-04). |

### 5.3 Constantes

Definidas en un único módulo y reutilizadas en toda la aplicación.

| Constante | Valor |
|-----------|-------|
| `STORAGE_KEY` | `taskflow-lite:tasks` |
| `TITLE_MIN_LENGTH` | 3 |
| `TITLE_MAX_LENGTH` | 80 |
| `DESCRIPTION_MAX_LENGTH` | 300 |
| `TASK_STATUS` | `pending`, `in-progress`, `done` |

---

## 6. Reglas de validación

El validador es **único y reutilizado** por creación y edición (HU-005 reutiliza las reglas de HU-001). Valida y devuelve una lista de errores, sin modificar nada.

### 6.1 Título (HU-001 CA-002 a CA-004, HU-005 CA-005 a CA-007)

Se aplica `trim` y se evalúa en este orden:

| Condición | Mensaje |
|-----------|---------|
| Vacío | El título es obligatorio. |
| Menos de 3 caracteres | El título debe tener al menos 3 caracteres. |
| Más de 80 caracteres | El título no puede superar los 80 caracteres. |

### 6.2 Descripción (HU-001 CA-005 y CA-006, HU-005 CA-008)

Se aplica `trim`. Vacía es válida.

| Condición | Mensaje |
|-----------|---------|
| Más de 300 caracteres | La descripción no puede superar los 300 caracteres. |

### 6.3 Fecha límite (HU-005 CA-003, CA-004 y CA-009; D-05)

- Vacía equivale a "sin fecha límite" y se guarda como `null`.
- Si se proporciona, debe tener formato `YYYY-MM-DD` y corresponder a una **fecha real del calendario** (por ejemplo, `2026-02-30` es inválida).
- Se aceptan fechas pasadas (D-05).
- En el formulario se usa `<input type="date">` y además se valida en código, porque el navegador no es la única barrera.

| Condición | Mensaje |
|-----------|---------|
| Valor no vacío que no es una fecha válida | La fecha límite no es válida. |

### 6.4 Catálogo de otros mensajes visibles

Todos los textos viven en un único módulo (`messages.js`).

| Situación | Mensaje |
|-----------|---------|
| Lista vacía (HU-002 CA-007, HU-004 CA-006) | Aún no hay tareas registradas. |
| No se pudo leer lo guardado (HU-004 CA-007) | No fue posible recuperar las tareas guardadas. |
| Algunos registros descartados (§9.5) | Algunas tareas guardadas no pudieron recuperarse y no se muestran. |
| Falló la escritura (N-05) | No fue posible guardar los cambios en el navegador. |

---

## 7. Registro de tareas (HU-001)

1. La vista incluye un formulario con **título** (obligatorio) y **descripción** (opcional). No incluye fecha límite (D-05).
2. Al enviar, se valida con §6.1 y §6.2.
3. **Si hay errores:** no se crea la tarea, no se guarda nada y se muestran los mensajes junto a los campos correspondientes. Lo escrito se conserva en el formulario.
4. **Si es válido**, se crea la tarea con:
   - `id` generado (A-04).
   - `title` y `description` recortados.
   - `status` = `pending` (HU-001 RN-005).
   - `createdAt` = momento actual en ISO 8601.
   - `dueDate` = `null`.
5. La tarea se agrega, se guarda automáticamente (§9.3), se actualiza la lista (§8.7) y se limpia el formulario.

---

## 8. Visualización (HU-002)

Vista única, de solo lectura (HU-002 RN-006 y RN-007). Las acciones de registro y edición viven en sus propios controles.

| § | Comportamiento | HU-002 |
|---|----------------|--------|
| 8.1 | Se muestran **todas** las tareas válidas, sin paginación, filtros ni agrupación. | CA-001, RN-001 |
| 8.2 | Cada tarea muestra **título** y **estado** con su etiqueta visible (§5.2). | CA-002, RN-002 |
| 8.3 | La descripción se muestra solo si no está vacía. Sin descripción, no se renderiza ningún bloque vacío. | CA-003, CA-004, RN-003 |
| 8.4 | Los tres estados son distinguibles entre sí por **color y por texto de la etiqueta**, nunca solo por color. | CA-005, RN-004 |
| 8.5 | Orden por `createdAt` descendente. Desempate: la insertada después va primero (A-05). El orden se calcula al mostrar, no se guarda. | CA-006, RN-005 |
| 8.6 | Sin tareas: se muestra el mensaje de §6.4. | CA-007 |
| 8.7 | Tras registrar o editar, la lista se actualiza sin recargar la página. | CA-008 |

Notas:

- La fecha límite **no se muestra** en la lista (D-05, HU-002 deja fuera su visualización).
- El control "Editar" de cada tarjeta pertenece a HU-005 y no altera el carácter de solo lectura de la lista.

---

## 9. Persistencia (HU-004)

### 9.1 Esquema

- Clave: `taskflow-lite:tasks`.
- Valor: arreglo JSON de objetos `Task` (§5.1), en orden de inserción.
- Se guardan todos los datos de cada tarea: título, descripción, estado, identificador, fecha de creación y fecha límite (HU-004 RN-002).

### 9.2 Carga

Al iniciar la aplicación, antes del primer render:

1. Leer la clave.
2. Interpretar el JSON.
3. Validar cada registro (§9.5).
4. Cargar en memoria y mostrar con el orden de §8.5 (HU-004 RN-004).

### 9.3 Guardado automático

Después de **cada** cambio (crear o editar), el arreglo completo se escribe en `localStorage` sin que la persona realice ninguna acción de guardado (HU-004 RN-003). La memoria y el almacenamiento no deben quedar desincronizados: si la escritura falla (N-05), se avisa y la app sigue funcionando.

### 9.4 Primer uso

Si la clave no existe, es un caso **válido**, no un error (HU-004 RN-006): lista vacía con el mensaje de §6.4 y sin aviso de error.

### 9.5 Datos inaccesibles, ilegibles o inválidos (D-07)

| Situación | Resultado |
|-----------|-----------|
| `localStorage` inaccesible (lanza excepción) | Lista vacía y aviso "No fue posible recuperar las tareas guardadas". La app sigue funcionando. |
| Valor que no es JSON legible, o JSON que no es un arreglo | Igual que la fila anterior. |
| Registro individual inválido dentro de un arreglo válido | Se descarta **solo ese registro**. Se muestran los válidos y el aviso "Algunas tareas guardadas no pudieron recuperarse y no se muestran". |
| Registro con `id` repetido (N-04) | Se conserva el primero, se descartan los repetidos y se muestra el mismo aviso. |
| Se descartó algo o hubo error de lectura | **No se sobrescribe** lo guardado durante la carga. Solo se escribe cuando la persona crea o edita una tarea. |

Un registro es **válido** si cumple todo lo siguiente:

- `id`: texto no vacío.
- `title`: cumple §6.1 sin necesidad de recorte adicional.
- `description`: texto de 0 a 300 caracteres.
- `status`: uno de los 3 valores de §5.2.
- `createdAt`: texto que representa una fecha y hora válida.
- `dueDate`: `null` o una fecha válida según §6.3.

Nunca se muestran tareas incompletas ni con datos inválidos (HU-004 CA-007).

---

## 10. Edición (HU-005)

| § | Comportamiento | HU-005 |
|---|----------------|--------|
| 10.1 | Cada tarea tiene un control "Editar". Al activarlo, esa tarjeta pasa a modo edición en línea (N-01) y el formulario se precarga con los valores actuales. Solo hay un formulario abierto a la vez (N-02). | RN-009 |
| 10.2 | Campos editables: **título**, **descripción** y **fecha límite**. | CA-001 a CA-003 |
| 10.3 | Se puede vaciar la descripción y la fecha límite. La tarea queda sin ese dato y sigue siendo válida (`""` y `null`). | CA-004 |
| 10.4 | **Atomicidad:** se validan todos los campos juntos con §6. Si alguno es inválido, no se aplica ningún cambio, la tarea conserva todos sus datos anteriores y se muestran los errores. | CA-005 a CA-010, RN-008 |
| 10.5 | Se puede editar en cualquier estado, incluida finalizada. `id`, `createdAt` y `status` **nunca** se modifican al editar. | CA-011, CA-012, RN-006, RN-007 |
| 10.6 | Solo se modifica la tarea seleccionada. Las demás conservan sus datos. | CA-013, RN-009 |
| 10.7 | Al guardar con éxito: se persiste (§9.3), se cierra el formulario y la lista muestra los datos actualizados **en la misma posición**, porque el orden depende solo de `createdAt`. | CA-014, CA-015, RN-010 |
| 10.8 | Cerrar el formulario sin guardar descarta lo escrito y no modifica la tarea (D-06). No hay historial ni "deshacer" tras guardar. | D-06 |
| 10.9 | Guardar sin cambios es válido (N-03). | N-03 |

La edición no puede aplicar un estado distinto al actual (HU-005 fuera de alcance: cambiar el estado).

---

## 11. Estructura de archivos

Extiende §3 de SPEC-000 (que delega esta decisión a las SPECs funcionales) y agrega `docs/stories/` (A-10).

```text
taskflow-lite/
├── .gitignore
├── README.md
├── index.html
├── docs/
│   ├── specs/
│   │   ├── SPEC-000-foundation.md
│   │   └── SPEC-001-gestion-de-tareas.md
│   └── stories/
│       ├── HU-001-RegistrarTarea.md
│       ├── HU-002-VisualizarTarea.md
│       ├── HU-004-PersistirTareas.md
│       └── HU-005-EditarTarea.md
└── src/
    ├── css/
    │   └── styles.css
    └── js/
        ├── main.js
        ├── domain/
        │   ├── task-constants.js
        │   ├── task-validator.js
        │   └── task-service.js
        ├── storage/
        │   └── task-storage.js
        └── ui/
            ├── messages.js
            ├── task-form.js
            ├── task-list.js
            └── task-edit-form.js
```

| Módulo | Responsabilidad | No debe |
|--------|-----------------|---------|
| `main.js` | Orquestar: cargar, mantener el estado en memoria, persistir tras cada cambio y volver a pintar. | Contener reglas de validación ni acceder directamente a `localStorage`. |
| `domain/task-constants.js` | Constantes de §5.3. | Tener lógica. |
| `domain/task-validator.js` | Validar título, descripción, fecha límite y registros guardados. | Acceder al DOM o al almacenamiento. |
| `domain/task-service.js` | Crear tarea, aplicar edición atómica y ordenar. Funciones que no mutan la entrada. | Acceder al DOM o al almacenamiento. |
| `storage/task-storage.js` | Leer y guardar en `localStorage`; manejar errores y registros inválidos (§9). | Conocer el DOM. |
| `ui/messages.js` | Todos los textos visibles en español. | Tener lógica. |
| `ui/task-form.js` | Formulario de registro y sus errores. | Crear la tarea por sí mismo ni guardarla. |
| `ui/task-list.js` | Render de la lista, estados, descripción condicional y vacío. | Modificar tareas. |
| `ui/task-edit-form.js` | Formulario de edición en línea y sus errores. | Guardar por sí mismo. |

Reglas de HTML y CSS (SPEC-000 §5):

- `id` en `kebab-case` (`task-form`, `task-list`).
- Clases propias en `kebab-case`; el resto de estilos usa utilidades de Tailwind.
- Todo `<input>` y `<textarea>` lleva `<label>` asociado.

---

## 12. Verificación manual

SPEC-000 excluye pruebas automatizadas. La verificación es manual y su constancia se deja en la descripción del PR.

### 12.1 Entorno

- Chrome o Edge actual, servidor Live Server sobre la raíz del proyecto.
- Consola de DevTools abierta: no debe haber errores en ningún escenario.
- Para cada caso se parte de `localStorage` limpio, salvo que se indique otra preparación.

### 12.2 Datos semilla (D-04)

Para casos que requieren estados `in-progress` o `done`, ejecutar en la consola y recargar:

```js
localStorage.setItem('taskflow-lite:tasks', JSON.stringify([
  { id: 'seed-0001', title: 'Tarea pendiente de prueba',   description: 'Con descripción', status: 'pending',     createdAt: '2026-10-20T10:00:00.000Z', dueDate: null },
  { id: 'seed-0002', title: 'Tarea en proceso de prueba',  description: '',                status: 'in-progress', createdAt: '2026-10-21T10:00:00.000Z', dueDate: '2026-11-01' },
  { id: 'seed-0003', title: 'Tarea finalizada de prueba',  description: 'Otra descripción', status: 'done',       createdAt: '2026-10-22T10:00:00.000Z', dueDate: null }
]));
```

Estos datos son solo para verificación y no forman parte del producto.

### 12.3 Checklist por criterio de aceptación

**HU-001: Crear una tarea**

| ID | Preparación y pasos | Resultado esperado |
|----|---------------------|--------------------|
| V-001-01 | Escribir título "Revisar informe" y registrar. | La tarea aparece en la lista como Pendiente. |
| V-001-02 | Dejar el título vacío y registrar. Repetir con solo espacios. | No se crea la tarea. Aparece "El título es obligatorio." |
| V-001-03 | Título de 2 caracteres. Probar también 3 (debe aceptarse). | Con 2: no se crea y aparece el mensaje de mínimo. Con 3: se crea. |
| V-001-04 | Título de 81 caracteres. Probar también 80 (debe aceptarse). | Con 81: no se crea y aparece el mensaje de máximo. Con 80: se crea. |
| V-001-05 | Título válido, descripción vacía. | Se crea correctamente sin descripción. |
| V-001-06 | Título válido, descripción de 301 caracteres. Probar también 300. | Con 301: no se crea y aparece el mensaje. Con 300: se crea. |

**HU-002: Visualizar tareas**

| ID | Preparación y pasos | Resultado esperado |
|----|---------------------|--------------------|
| V-002-01 | Crear 3 tareas y abrir la vista. | Las 3 aparecen en una sola vista. |
| V-002-02 | Observar cualquier tarea. | Muestra título y estado. |
| V-002-03 | Tarea con descripción. | La descripción se muestra. |
| V-002-04 | Tarea sin descripción. | Se muestra bien, sin bloque de descripción vacío (inspeccionar el DOM). |
| V-002-05 | Cargar datos semilla (§12.2). | Pendiente, En proceso y Finalizada se distinguen por color y por texto. |
| V-002-06 | Crear 3 tareas con unos segundos de diferencia. | Orden de más reciente a más antigua. |
| V-002-07 | Limpiar `localStorage` y abrir la vista. | Aparece "Aún no hay tareas registradas." |
| V-002-08 | Registrar una tarea con la lista abierta. | Aparece de inmediato, Pendiente, sin recargar. |

**HU-004: Persistir tareas**

| ID | Preparación y pasos | Resultado esperado |
|----|---------------------|--------------------|
| V-004-01 | Crear 3 tareas, cerrar el navegador y reabrir. | Las mismas 3, sin faltantes ni duplicados. |
| V-004-02 | Crear tarea con título, descripción; cargar semilla con fecha límite. Reabrir. | Se conservan título, descripción, estado, `id`, `createdAt` (revisar en Application > Local Storage). |
| V-004-03 | Crear una tarea, cerrar y reabrir. | Sigue en la lista como Pendiente. |
| V-004-04 | Cargar semilla (estados distintos), reabrir. | Cada tarea conserva su estado. **Verificación a nivel de persistencia; el cambio por interfaz se cierra con HU-003 (D-04).** |
| V-004-05 | Crear varias tareas, reabrir. | Mismo orden de más reciente a más antigua. |
| V-004-06 | Primer uso sin clave guardada. | La app funciona, aparece el mensaje de lista vacía y **no** aparece aviso de error. |
| V-004-07 | (a) Guardar un texto no JSON en la clave. (b) Guardar un arreglo con un registro inválido y uno válido. (c) Simular `localStorage` inaccesible desde DevTools o con ajustes de cookies y datos del sitio. | (a) y (c): lista vacía, aviso de no recuperación, app funcional. (b): solo se ve el registro válido y aparece el aviso parcial. En ninguno hay tareas incompletas y lo guardado no se sobrescribe hasta crear o editar. |

**HU-005: Editar tarea**

| ID | Preparación y pasos | Resultado esperado |
|----|---------------------|--------------------|
| V-005-01 | Editar el título a uno válido y distinto. | La tarea muestra el nuevo título, no el anterior. |
| V-005-02 | Editar la descripción a una válida y distinta. | Muestra la nueva descripción. |
| V-005-03 | Asignar una fecha límite válida, guardar y volver a abrir la edición. | El formulario muestra la fecha guardada. |
| V-005-04 | Tarea con descripción y fecha. Vaciar una, guardar. Repetir con la otra. | Queda sin el dato eliminado y sigue válida. |
| V-005-05 | Vaciar el título y guardar. | La tarea no cambia. "El título es obligatorio." |
| V-005-06 | Título de 2 caracteres y guardar. | La tarea no cambia. Mensaje de mínimo. |
| V-005-07 | Título de 81 caracteres y guardar. | La tarea no cambia. Mensaje de máximo. |
| V-005-08 | Descripción de 301 caracteres y guardar. | La tarea no cambia. Mensaje de descripción. |
| V-005-09 | Fecha inválida. Como el control `date` no deja escribir fechas inexistentes, ejecutar el validador desde la consola con `2026-02-30` y `abc`. | El validador devuelve "La fecha límite no es válida." y, si se fuerza el envío, la tarea no cambia. |
| V-005-10 | Cambiar título y descripción, dejando el título inválido. Guardar. | No se guarda ningún cambio; la tarea conserva todos sus datos anteriores. |
| V-005-11 | Con semilla, editar una tarea de cada estado. | La edición se aplica y el estado no cambia. |
| V-005-12 | Editar y guardar; revisar en Local Storage. | `id`, `createdAt` y `status` idénticos a los previos. |
| V-005-13 | Con 3 tareas, editar una. | Solo cambia esa. |
| V-005-14 | Editar una tarea que no es la más reciente. | Se muestra actualizada y en la misma posición. |
| V-005-15 | Editar, cerrar el navegador y reabrir. | Se muestran los datos editados. |

### 12.4 Casos adicionales

| ID | Caso | Resultado esperado |
|----|------|--------------------|
| V-EXT-01 | Escribir `<b>x</b>` o `<img src=x onerror=alert(1)>` como título y descripción. | Se muestra como texto literal; no se ejecuta nada (A-06). |
| V-EXT-02 | Cerrar el formulario de edición sin guardar. | La tarea no cambia (D-06). |
| V-EXT-03 | Abrir la edición de una tarea y luego la de otra. | Solo queda un formulario abierto; lo escrito en el anterior se descarta (N-02). |
| V-EXT-04 | Dos registros guardados con el mismo `id`. | Se muestra uno y aparece el aviso parcial (N-04). |
| V-EXT-05 | Crear y editar con título de 3 y de 80 caracteres exactos. | Se aceptan (A-03). |

---

## 13. Matriz de trazabilidad

Cobertura: **36 de 36 CA** con destino en esta SPEC, tarea asignada y verificación.

### HU-001

| CA | SPEC-001 | Tareas | Verificación |
|----|----------|--------|--------------|
| CA-001 | §5, §7 | T-02, T-04, T-07 | V-001-01 |
| CA-002 | §6.1, §7 | T-03, T-07 | V-001-02 |
| CA-003 | §6.1 | T-03, T-07 | V-001-03 |
| CA-004 | §6.1 | T-03, T-07 | V-001-04 |
| CA-005 | §6.2, §7 | T-03, T-04, T-07 | V-001-05 |
| CA-006 | §6.2 | T-03, T-07 | V-001-06 |

### HU-002

| CA | SPEC-001 | Tareas | Verificación |
|----|----------|--------|--------------|
| CA-001 | §8.1 | T-06 | V-002-01 |
| CA-002 | §8.2 | T-06 | V-002-02 |
| CA-003 | §8.3 | T-06 | V-002-03 |
| CA-004 | §8.3 | T-06 | V-002-04 |
| CA-005 | §8.4, D-04 | T-06, T-10 | V-002-05 |
| CA-006 | §8.5 | T-04, T-06 | V-002-06 |
| CA-007 | §8.6 | T-06 | V-002-07 |
| CA-008 | §8.7, §7 | T-07, T-09 | V-002-08 |

### HU-004

| CA | SPEC-001 | Tareas | Verificación |
|----|----------|--------|--------------|
| CA-001 | §9.2, N-04 | T-05, T-09 | V-004-01, V-EXT-04 |
| CA-002 | §5, §9.1 | T-05 | V-004-02 |
| CA-003 | §9.3 | T-05, T-09 | V-004-03 |
| CA-004 | §9.1, §9.3, D-04 | T-05, T-10 | V-004-04 (parcial hasta HU-003) |
| CA-005 | §8.5, §9.2 | T-05, T-06 | V-004-05 |
| CA-006 | §9.4 | T-05, T-06 | V-004-06 |
| CA-007 | §9.5, D-07 | T-05, T-09 | V-004-07 |

### HU-005

| CA | SPEC-001 | Tareas | Verificación |
|----|----------|--------|--------------|
| CA-001 | §10.2 | T-08 | V-005-01 |
| CA-002 | §10.2 | T-08 | V-005-02 |
| CA-003 | §6.3, §10.2, D-05 | T-03, T-08 | V-005-03 |
| CA-004 | §10.3 | T-08 | V-005-04 |
| CA-005 | §6.1, §10.4 | T-03, T-08 | V-005-05 |
| CA-006 | §6.1, §10.4 | T-03, T-08 | V-005-06 |
| CA-007 | §6.1, §10.4 | T-03, T-08 | V-005-07 |
| CA-008 | §6.2, §10.4 | T-03, T-08 | V-005-08 |
| CA-009 | §6.3, §10.4 | T-03, T-08 | V-005-09 |
| CA-010 | §10.4 | T-08 | V-005-10 |
| CA-011 | §10.5, D-04 | T-08, T-10 | V-005-11 |
| CA-012 | §10.5 | T-08 | V-005-12 |
| CA-013 | §10.6 | T-08 | V-005-13 |
| CA-014 | §8.5, §10.7 | T-06, T-08 | V-005-14 |
| CA-015 | §9.3, §10.7 | T-05, T-09 | V-005-15 |

### Reglas de negocio con tratamiento transversal

| Regla | SPEC-001 |
|-------|----------|
| HU-001 RN-005, RN-006, RN-007 | §5, §7 |
| HU-002 RN-006, RN-007 | §8 (vista única, solo lectura) |
| HU-004 RN-001, RN-005 | §9.1 (datos en el navegador de cada persona) |
| HU-005 RN-006, RN-007, RN-009 | §10.1, §10.5, §10.6 |

---

## 14. Plan de tareas técnicas

| ID | Tarea | Entregable | Depende de | Cubre |
|----|-------|------------|------------|-------|
| T-01 | Aprobar SPEC-000. Crear rama `feature/SPEC-001-gestion-de-tareas`. Mover las HU a `docs/stories/` dejando HU-002 solo con su versión vigente (D-02). Marcar SPEC-000 como "Enmendada por SPEC-001". Preparar `index.html` con Tailwind 3.4.17, `lang="es"`, contenedores `task-form` y `task-list`, y carga de `main.js` como módulo. Actualizar README. | Estructura base y documentación | Aprobación de SPEC-001 | §3, §11 |
| T-02 | Constantes y definición del modelo `Task`. | `task-constants.js` | T-01 | §5 |
| T-03 | Validador de título, descripción, fecha límite y registros guardados; catálogo de mensajes. | `task-validator.js`, `messages.js` | T-02 | §6, §9.5 |
| T-04 | Servicio: crear tarea (id, fecha, estado inicial), ordenar y aplicar edición atómica. | `task-service.js` | T-02, T-03 | §7, §8.5, §10.4 a §10.6 |
| T-05 | Capa de storage: cargar, guardar, manejar errores, registros inválidos y duplicados. | `task-storage.js` | T-02, T-03 | §9 |
| T-06 | Render de la lista: vacío, descripción condicional, estados distinguibles, botón "Editar". | `task-list.js` | T-02 | §8 |
| T-07 | Formulario de registro con errores visibles. | `task-form.js` | T-03 | §7 |
| T-08 | Formulario de edición en línea con cierre sin guardar. | `task-edit-form.js` | T-03, T-06 | §10 |
| T-09 | Integración en `main.js`: carga inicial, avisos de storage, guardado tras cada cambio, re-render. | `main.js` | T-04 a T-08 | §9.2, §9.3, §10.7 |
| T-10 | Verificación manual completa (§12) con datos semilla. Evidencia en el PR. | Checklist completado | T-09 | §12 |
| T-11 | Cierre: README, revisión de DoD, commits conforme a SPEC-000 y merge a `main`. | Rama lista | T-10 | §15 |

### Orden de commits sugerido

1. `docs(spec): agregar SPEC-001 de gestión de tareas`
2. `docs(stories): agregar historias de usuario del Sprint 1`
3. `chore(repo): preparar index.html con Tailwind`
4. `feat(domain): agregar modelo, constantes y validación de tareas`
5. `feat(storage): agregar persistencia en localStorage`
6. `feat(ui): agregar lista de tareas`
7. `feat(ui): agregar formulario de registro`
8. `feat(ui): agregar edición de tareas`
9. `feat(app): integrar carga, guardado y render`
10. `docs(readme): actualizar instrucciones de ejecución`

---

## 15. Definition of Done

Cumple la DoD de SPEC-000 §7 y además:

- [ ] Los 36 CA tienen su verificación de §12 realizada y registrada, con los 4 casos condicionados por D-04 marcados como verificados con datos semilla.
- [ ] La consola del navegador no muestra errores en ningún escenario.
- [ ] La única dependencia externa es Tailwind 3.4.17 por CDN, conforme a §3.1.
- [ ] Todo texto de la persona usuaria se inserta con `textContent` (A-06).
- [ ] No hay ningún control para cambiar el estado, eliminar tareas ni capturar fecha límite al crear.
- [ ] La lista no muestra la fecha límite.
- [ ] Todos los textos visibles están en español y provienen de `messages.js`.
- [ ] Ningún módulo de `domain/` accede al DOM ni a `localStorage`.
- [ ] La clave de almacenamiento es `taskflow-lite:tasks`.
- [ ] Rama y commits siguen SPEC-000 §6.
- [ ] El README explica cómo ejecutar con Live Server y menciona que se requiere internet para Tailwind.

---

## 16. Fuera de alcance

- HU-003 y cualquier control de cambio de estado.
- Eliminar tareas, individual o masivamente.
- Captura de fecha límite al crear y su visualización en la lista.
- Tareas vencidas, hora de la fecha límite, recordatorios.
- Filtros, búsqueda, paginación, agrupación y orden elegido por la persona.
- Asignación a personas, prioridades, categorías, adjuntos y subtareas.
- Historial de ediciones, fecha de última edición, deshacer una edición guardada.
- Exportar, importar, sincronizar, compartir, cifrar o migrar datos.
- Manejo de almacenamiento lleno.
- Pruebas automatizadas, linters y despliegue.
- Cualquier funcionalidad planificada para el Sprint 2.