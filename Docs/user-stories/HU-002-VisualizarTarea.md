# HU-002-Visualizar lista de tareas y su estado

## Historia de usuario
como integrante del equipo, quiero visualizar mis tareas registradas con su título, descripción y estado, para saber qué trabajo tengo pendiente, en curso o terminado desde una sola vista.

## Criterio de aceptación
### CA-001-Mostrar tareas registradas
Given que el usuario tiene una o más tareas registradas
When accede a la vista de tareas
Then deben mostrarse todas sus tareas en una sola vista

### CA-002-Información visible de cada tarea
Given que el usuario tiene tareas registradas
When visualiza la lista de tareas
Then cada tarea debe mostrar su título
And debe mostrar su estado actual

### CA-003-Descripción visible cuando existe
Given que una tarea tiene descripción
When el usuario visualiza la lista de tareas
Then la descripción de esa tarea debe mostrarse

### CA-004-Tarea sin descripción
Given que una tarea no tiene descripción
When el usuario visualiza la lista de tareas
Then la tarea debe mostrarse correctamente
And no debe mostrarse un espacio de descripción vacío

### CA-005-Estados distinguibles
Given que el usuario tiene una tarea pendiente
And tiene una tarea en proceso
And tiene una tarea finalizada
When visualiza la lista de tareas
Then debe poder distinguir el estado de cada tarea sin abrir otra vista

### CA-006-Orden de visualización
Given que el usuario tiene más de una tarea registrada
And las tareas fueron creadas en momentos distintos
When visualiza la lista de tareas
Then las tareas deben mostrarse de la más reciente a la más antigua según su fecha de creación

### CA-007-Sin tareas registradas
Given que el usuario no tiene tareas registradas
When accede a la vista de tareas
Then debe recibir información indicando que aún no hay tareas registradas

### CA-008-Nueva tarea visible
Given que el usuario registra una nueva tarea correctamente
When regresa a la vista de tareas o esta se actualiza
Then la nueva tarea debe aparecer en la lista con estado pendiente

## Reglas del Negocio
RN-001: Deben mostrarse todas las tareas registradas, sin omitir ninguna.
RN-002: Cada tarea debe mostrar como mínimo su título y su estado.
RN-003: La descripción se muestra solo cuando la tarea la tiene.
RN-004: Los tres estados (pendiente, en proceso y finalizada) deben ser visualmente distinguibles entre sí.
RN-005: Las tareas se muestran ordenadas de la más reciente a la más antigua, según su fecha de creación.
RN-006: Todas las tareas se visualizan en una sola vista, sin requerir filtros ni navegación adicional.
RN-007: Visualizar tareas es una acción de solo lectura y no modifica ninguna tarea.

## Dependencias
Depende de HU-001-Crear una tarea, ya que requiere que existan tareas registradas con título, descripción opcional, estado y fecha de creación.

## Fuera de Alcance
* Filtrar tareas.
* Ordenar tareas por criterios elegidos por el usuario.
* Buscar tareas.
* Agrupar tareas por estado.
* Paginación.
* Fechas límite y su visualización.
* Indicar tareas vencidas.
* Editar, avanzar estado o eliminar tareas desde la vista.
* Persistencia de las tareas entre sesiones.
* Vistas de equipo o tareas de otras personas.