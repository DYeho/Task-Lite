# HU-006-Eliminar tarea

## Historia de usuario
como integrante del equipo, quiero eliminar una tarea registrada, para mantener mi lista limpia y sin tareas que ya no necesito.

## Criterio de aceptación
### CA-001-Solicitar confirmación antes de eliminar
Given que el usuario tiene una tarea registrada
When solicita eliminar la tarea
Then debe pedirse confirmación antes de eliminarla
And debe informarse que la eliminación no se puede deshacer
And la tarea debe conservarse mientras no se confirme

### CA-002-Confirmar la eliminación
Given que el usuario solicitó eliminar una tarea
And se le pidió confirmación
When confirma la eliminación
Then la tarea debe eliminarse
And ya no debe estar disponible para el usuario

### CA-003-Cancelar la eliminación
Given que el usuario solicitó eliminar una tarea
And se le pidió confirmación
When cancela la eliminación
Then la tarea no debe eliminarse
And la tarea debe conservar todos sus datos

### CA-004-Eliminar en cualquier estado
Given que el usuario tiene una tarea en estado pendiente, en proceso o finalizada
When solicita eliminarla y confirma
Then la tarea debe eliminarse correctamente
And el estado de la tarea no debe impedir la eliminación

### CA-005-Solo se elimina la tarea seleccionada
Given que el usuario tiene varias tareas registradas
And selecciona una de ellas para eliminar
When confirma la eliminación
Then solo la tarea seleccionada debe eliminarse
And las demás tareas deben conservar todos sus datos

### CA-006-Tarea eliminada deja de visualizarse
Given que el usuario eliminó una tarea
When visualiza la lista de tareas
Then la tarea eliminada no debe aparecer en la lista
And las tareas restantes deben mostrarse en el mismo orden definido en HU-002

### CA-007-Eliminar la última tarea
Given que el usuario tiene una sola tarea registrada
When solicita eliminarla y confirma
Then la tarea debe eliminarse
And debe informarse que aún no hay tareas registradas

### CA-008-Eliminación conservada entre sesiones
Given que el usuario eliminó una tarea
When cierra y vuelve a abrir la aplicación
Then la tarea eliminada no debe volver a aparecer
And las demás tareas deben seguir apareciendo con sus datos

## Reglas del Negocio
RN-001: Toda eliminación requiere confirmación previa del usuario.
RN-002: La eliminación es definitiva y no se puede deshacer.
RN-003: Una tarea puede eliminarse en cualquiera de sus estados.
RN-004: Eliminar una tarea solo afecta a la tarea seleccionada.
RN-005: Si el usuario cancela la confirmación, la tarea no sufre ningún cambio.
RN-006: Una tarea eliminada no debe volver a mostrarse, ni en la sesión actual ni en sesiones posteriores, según HU-004.
RN-007: No tener tareas registradas es un caso válido y no es un error.

## Dependencias
Depende de HU-001-Crear una tarea, porque requiere tareas registradas.
Depende de HU-002-Visualizar tareas, porque el usuario necesita ver la tarea para seleccionarla y comprobar que desapareció.
Depende de HU-004-Persistir tareas, porque la eliminación debe conservarse entre sesiones.
No depende de HU-003-Actualizar estado de una tarea ni de HU-005-Editar tarea.

## Fuera de Alcance
* Deshacer una eliminación.
* Papelera o recuperación de tareas eliminadas.
* Eliminar varias tareas a la vez.
* Eliminar todas las tareas de una sola vez.
* Historial de tareas eliminadas.
* Archivar tareas como alternativa a eliminarlas.
* Notificaciones de eliminación.