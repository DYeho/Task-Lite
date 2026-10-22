# HU-005-Editar tarea

## Historia de usuario
como integrante del equipo, quiero editar el título, la descripción y la fecha límite de una tarea registrada, para corregir o actualizar su información sin tener que crearla de nuevo.

## Criterio de aceptación
### CA-001-Editar título
Given que el usuario tiene una tarea registrada
And proporciona un título válido distinto del actual
When solicita guardar la edición
Then la tarea debe mostrarse con el nuevo título
And no debe conservar el título anterior

### CA-002-Editar descripción
Given que el usuario tiene una tarea registrada
And proporciona una descripción válida distinta de la actual
When solicita guardar la edición
Then la tarea debe mostrarse con la nueva descripción
And no debe conservar la descripción anterior

### CA-003-Editar fecha límite
Given que el usuario tiene una tarea registrada
And proporciona una fecha límite válida
When solicita guardar la edición
Then la tarea debe quedar con la nueva fecha límite
And la fecha límite debe conservarse al volver a editar la tarea

### CA-004-Quitar descripción o fecha límite
Given que el usuario tiene una tarea con descripción y fecha límite
And elimina la descripción o la fecha límite al editar
When solicita guardar la edición
Then la tarea debe quedar sin el dato eliminado
And la tarea debe seguir siendo válida

### CA-005-Título obligatorio
Given que el usuario edita una tarea
And deja el título vacío
When intenta guardar la edición
Then la tarea no debe modificarse
And debe recibir información indicando que el título es obligatorio

### CA-006-Longitud mínima del título
Given que el usuario edita una tarea
And proporciona un título con menos de 3 caracteres
When intenta guardar la edición
Then la tarea no debe modificarse
And debe informarse que el título no cumple con la longitud mínima

### CA-007-Longitud máxima del título
Given que el usuario edita una tarea
And proporciona un título con más de 80 caracteres
When intenta guardar la edición
Then la tarea no debe modificarse
And debe informarse que el título no cumple con la longitud máxima

### CA-008-Descripción demasiado extensa
Given que el usuario edita una tarea
And proporciona una descripción mayor a 300 caracteres
When intenta guardar la edición
Then la tarea no debe modificarse
And debe informarse la restricción correspondiente

### CA-009-Fecha límite inválida
Given que el usuario edita una tarea
And proporciona una fecha límite que no es una fecha válida
When intenta guardar la edición
Then la tarea no debe modificarse
And debe informarse que la fecha límite no es válida

### CA-010-Edición con datos inválidos conserva todo
Given que el usuario modifica varios datos de una tarea
And al menos uno de ellos no es válido
When intenta guardar la edición
Then ninguno de los cambios debe guardarse
And la tarea debe conservar todos sus datos anteriores

### CA-011-Editar en cualquier estado
Given que el usuario tiene una tarea en estado pendiente, en proceso o finalizada
When edita y guarda datos válidos
Then la edición debe aplicarse correctamente
And la tarea debe conservar el mismo estado que tenía

### CA-012-Datos que no se modifican al editar
Given que el usuario edita y guarda una tarea
When la edición se completa
Then el identificador de la tarea no debe cambiar
And la fecha de creación no debe cambiar
And el estado no debe cambiar

### CA-013-Solo cambia la tarea seleccionada
Given que el usuario tiene varias tareas registradas
And selecciona una de ellas para editar
When guarda la edición
Then solo la tarea seleccionada debe modificarse
And las demás tareas deben conservar sus datos

### CA-014-Edición visible en la lista
Given que el usuario guardó la edición de una tarea
When visualiza la lista de tareas
Then la tarea debe mostrarse con sus datos actualizados
And debe ocupar la misma posición que le corresponde según su fecha de creación

### CA-015-Edición conservada entre sesiones
Given que el usuario guardó la edición de una tarea
When cierra y vuelve a abrir la aplicación
Then la tarea debe mostrarse con los datos editados

## Reglas del Negocio
RN-001: El título es obligatorio.
RN-002: El título debe contener entre 3 y 80 caracteres.
RN-003: La descripción es opcional.
RN-004: La descripción tendrá un máximo de 300 caracteres.
RN-005: La fecha límite es opcional y, si se proporciona, debe ser una fecha válida.
RN-006: Una tarea puede editarse en cualquiera de sus estados, incluida finalizada.
RN-007: Editar una tarea no modifica su estado, su identificador ni su fecha de creación.
RN-008: Si algún dato editado no es válido, no se guarda ningún cambio.
RN-009: Editar una tarea solo afecta a la tarea seleccionada.
RN-010: Toda edición guardada debe conservarse entre sesiones, según HU-004.

## Dependencias
Depende de HU-001-Crear una tarea, porque requiere tareas registradas y reutiliza sus reglas de validación del título y la descripción.
Depende de HU-002-Visualizar tareas, porque el usuario necesita ver la tarea para seleccionarla y comprobar los datos editados.
Depende de HU-004-Persistir tareas, porque las ediciones deben conservarse entre sesiones.
No depende de HU-003-Actualizar estado de una tarea, ya que editar y cambiar el estado son acciones independientes.

## Fuera de Alcance
* Cambiar el estado de la tarea.
* Eliminar tareas.
* Cancelar o deshacer una edición.
* Historial de ediciones.
* Registrar la fecha de la última edición.
* Editar varias tareas a la vez.
* Hora de la fecha límite.
* Indicar tareas con fecha límite vencida.
* Ordenar, filtrar o buscar tareas por fecha límite.
* Recordatorios o notificaciones.
* Asignar tareas a personas, prioridades, categorías y archivos adjuntos.