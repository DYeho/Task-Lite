# HU-001-Crear una tarea

## Historia de usuario
como integrante del equipo, quiero registrar una nueva tarea con titulo y descripción, para documentar el trabajo que necesito realizar.

## Criterio de aceptación 
### CA-001-Crear Correctamente
Given que el usuario proporciona el titulo valido 
When solicita crear la tarea
Then la tarea debe agregarse
And debe de iniciar con un estado pendiente

### CA-002-Titulo obligatorio
Given que el ususario desea crear una Tarea 
And no proporciona el titulo
When intenta registrar la tarea
Then la tarea no debe ser creada 
And debe recibir informacion indicando que el titulo es obligatorio 

### CA-003-Longitud minima del titulo
Given que el ususario desea crear una tarea
And proporciona un titulo con menos de 3 caracteres
When intenta registrar la tarea 
Then la tarea no debe de ser creada
And debe informarse que el titulo no cumple con la longitud minima

### CA-004-Longitud maxima del titulo
Given que el ususario desea crear una tarea
And proporciona un titulo con mas de 80 caracteres
When intenta registrar la tarea 
Then la tarea no debe de ser creada
And debe informarse que el titulo no cumple con la longitud maxima

### CA-005-Descripcion Opcional
Given que el ususario proporciona un titulo valido 
And no proporciona descripcion 
When registra la tarea
Then la tarea debe crearse correctamente

### CA-006-Descricpcion demasiado extensa
Given que el ususario proporciona una descripcion mayor a 300 caracteres
When intenta crear la tarea
Then la tarea no debe ser registrada
And debe informar la restriccion correspondiente 

## Reglas del Negocio
RN-001: Todas las tareas deben de tener titulo.
RN-002: El titulo debe de contener entre 3 y 80 caracteres.
RN-003: La descripcion es opcional.
RN-004: La descripcion tendra maximo 300 caracteres.
RN-005: Toda tarea nueva inicia como pendiente.
RN-006: Cada tarea debe de poder identificarse de manera unica.
RN-007: Debe conocerse cuando fue creada la tarea.

## Dependencias 
Esta historia no depende funcionalmente de otra HU.

## Fuera de Alcance
* asignar tareas a personas.
* Fechas limite.
* Prioridades.
* Categorias.
* Archivos adjuntos.
* SubTareas.
* Notificaciones.

