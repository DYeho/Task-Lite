# HU-003-Actualizar estado de una tarea

## Historia de usuario
como integrante del equipo, quiero cambiar el estado de una tarea entre pendiente, en proceso y finalizada, para reflejar en todo momento el avance real de mi trabajo.

## Criterio de aceptación
### CA-001-Cambiar a otro estado
Given que el usuario tiene una tarea registrada en cualquier estado
And elige un estado distinto del actual
When solicita cambiar el estado de la tarea
Then la tarea debe pasar al estado elegido
And debe mostrarse con ese nuevo estado

### CA-002-Cambio sin restricción de orden
Given que el usuario tiene una tarea en estado pendiente
And elige el estado finalizada
When solicita cambiar el estado de la tarea
Then la tarea debe pasar a finalizada
And no debe ser necesario pasar antes por en proceso

### CA-003-Retroceder estado
Given que el usuario tiene una tarea en estado finalizada
And elige el estado en proceso o pendiente
When solicita cambiar el estado de la tarea
Then la tarea debe pasar al estado elegido

### CA-004-Elegir el estado actual
Given que el usuario tiene una tarea registrada
And elige el mismo estado que la tarea ya tiene
When solicita cambiar el estado de la tarea
Then el estado de la tarea no debe cambiar
And no debe producirse ningún otro cambio en la tarea

### CA-005-Datos de la tarea sin cambios
Given que el usuario tiene una tarea con título y descripción
When cambia el estado de la tarea
Then el título y la descripción no deben modificarse
And el identificador y la fecha de creación no deben modificarse

### CA-006-Solo cambia la tarea seleccionada
Given que el usuario tiene varias tareas registradas
And selecciona una de ellas para cambiar su estado
When solicita cambiar el estado
Then solo la tarea seleccionada debe cambiar de estado
And las demás tareas deben conservar su estado actual

### CA-007-Nuevo estado visible
Given que el usuario cambió el estado de una tarea
When visualiza la lista de tareas
Then la tarea debe mostrarse con su nuevo estado
And no debe mostrarse con su estado anterior

## Reglas del Negocio
RN-001: Los estados válidos son pendiente, en proceso y finalizada.
RN-002: Una tarea puede cambiar de cualquier estado a cualquier otro estado válido.
RN-003: No existe un orden obligatorio entre estados ni estados terminales.
RN-004: Cambiar a un estado igual al actual no produce ningún cambio.
RN-005: Cambiar el estado no modifica el título, la descripción, el identificador ni la fecha de creación de la tarea.
RN-006: Cambiar el estado solo afecta a la tarea seleccionada.
RN-007: Toda tarea debe tener siempre uno de los tres estados válidos.

## Dependencias
Depende de HU-001-Crear una tarea, porque requiere tareas registradas con estado.
Depende de HU-002-Visualizar tareas, porque el usuario necesita ver la tarea para seleccionarla y comprobar su nuevo estado.

## Fuera de Alcance
* Editar título, descripción o fecha límite.
* Eliminar tareas.
* Confirmación previa al cambio de estado.
* Historial de cambios de estado.
* Registro de la fecha en que cambió el estado.
* Persistencia de los cambios entre sesiones.
* Notificaciones de cambio de estado.
* Asignar tareas a personas.