# HU-004-Persistir tareas

## Historia de usuario
como integrante del equipo, quiero que mis tareas se conserven al cerrar y volver a abrir el navegador, para no perder mi información entre sesiones.

## Criterio de aceptación
### CA-001-Conservar tareas al reabrir
Given que el usuario tiene una o más tareas registradas
And cierra el navegador
When vuelve a abrir la aplicación en el mismo navegador
Then debe visualizar las mismas tareas que tenía antes de cerrar
And no debe faltar ninguna tarea ni aparecer tareas duplicadas

### CA-002-Conservar los datos de cada tarea
Given que el usuario tiene tareas con título, descripción y estado
When vuelve a abrir la aplicación
Then cada tarea debe conservar su título, su descripción y su estado
And debe conservar su identificador y su fecha de creación

### CA-003-Conservar una tarea nueva
Given que el usuario registra una nueva tarea correctamente
When cierra y vuelve a abrir la aplicación
Then la nueva tarea debe seguir apareciendo en la lista
And debe conservar el estado pendiente

### CA-004-Conservar un cambio de estado
Given que el usuario cambió el estado de una tarea
When cierra y vuelve a abrir la aplicación
Then la tarea debe mostrarse con el estado que tenía antes de cerrar
And no debe volver a su estado anterior

### CA-005-Conservar el orden de visualización
Given que el usuario tiene más de una tarea registrada
When vuelve a abrir la aplicación
Then las tareas deben mostrarse de la más reciente a la más antigua según su fecha de creación

### CA-006-Primer uso sin datos guardados
Given que el usuario abre la aplicación por primera vez
And no existen tareas guardadas en el navegador
When accede a la vista de tareas
Then la aplicación debe funcionar correctamente
And debe informarse que aún no hay tareas registradas

### CA-007-Datos guardados no disponibles o ilegibles
Given que el navegador no permite acceder a las tareas guardadas
Or que las tareas guardadas no pueden leerse correctamente
When el usuario abre la aplicación
Then la aplicación no debe dejar de funcionar
And debe informarse que no fue posible recuperar las tareas guardadas
And no deben mostrarse tareas incompletas o con datos inválidos

## Reglas del Negocio
RN-001: Las tareas se conservan en el navegador del usuario entre sesiones.
RN-002: Toda tarea registrada debe conservarse con todos sus datos: título, descripción, estado, identificador y fecha de creación.
RN-003: Todo cambio en una tarea debe conservarse sin que el usuario realice una acción adicional de guardado.
RN-004: Al abrir la aplicación se muestran las tareas conservadas, respetando el orden definido en HU-002.
RN-005: Cada usuario solo accede a las tareas guardadas en su propio navegador.
RN-006: La ausencia de tareas guardadas es un caso válido y no es un error.
RN-007: Si las tareas guardadas no pueden recuperarse, el usuario debe ser informado.

## Dependencias
Depende de HU-001-Crear una tarea, porque requiere tareas registradas con sus datos.
Depende de HU-002-Visualizar tareas, porque las tareas recuperadas se muestran en esa vista.
Depende de HU-003-Actualizar estado de una tarea, porque los cambios de estado deben conservarse.

## Fuera de Alcance
* Compartir tareas entre personas, navegadores o dispositivos.
* Sincronización de datos.
* Exportar o importar tareas.
* Respaldo o recuperación de datos perdidos.
* Límite de almacenamiento y manejo de almacenamiento lleno.
* Migración de datos entre versiones de la aplicación.
* Cifrado o protección de los datos guardados.
* Eliminar todas las tareas de una sola vez.
* Editar o eliminar tareas.