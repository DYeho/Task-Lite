# HU-007-Tareas vencidas

## Historia de usuario
como integrante del equipo, quiero identificar visualmente las tareas cuya fecha límite ya pasó y que aún no están finalizadas, para saber cuáles requieren atención primero.

## Criterio de aceptación
### CA-001-Marcar tarea vencida
Given que el usuario tiene una tarea con fecha límite anterior a la fecha actual
And la tarea está en estado pendiente o en proceso
When visualiza la lista de tareas
Then la tarea debe mostrarse como vencida
And debe conservar visible su estado actual

### CA-002-Fecha límite de hoy no está vencida
Given que el usuario tiene una tarea con fecha límite igual a la fecha actual
And la tarea está en estado pendiente o en proceso
When visualiza la lista de tareas
Then la tarea no debe mostrarse como vencida

### CA-003-Fecha límite futura no está vencida
Given que el usuario tiene una tarea con fecha límite posterior a la fecha actual
When visualiza la lista de tareas
Then la tarea no debe mostrarse como vencida

### CA-004-Tarea sin fecha límite no está vencida
Given que el usuario tiene una tarea sin fecha límite
When visualiza la lista de tareas
Then la tarea no debe mostrarse como vencida

### CA-005-Tarea finalizada no se marca como vencida
Given que el usuario tiene una tarea en estado finalizada
And su fecha límite es anterior a la fecha actual
When visualiza la lista de tareas
Then la tarea no debe mostrarse como vencida

### CA-006-Finalizar una tarea vencida
Given que el usuario tiene una tarea mostrada como vencida
When cambia su estado a finalizada
Then la tarea debe dejar de mostrarse como vencida

### CA-007-Reabrir una tarea finalizada con fecha pasada
Given que el usuario tiene una tarea finalizada con fecha límite anterior a la fecha actual
When cambia su estado a pendiente o en proceso
Then la tarea debe mostrarse como vencida

### CA-008-Editar la fecha límite actualiza el indicador
Given que el usuario tiene una tarea pendiente o en proceso
When edita su fecha límite y guarda
Then la tarea debe mostrarse como vencida si la nueva fecha es anterior a la fecha actual
And no debe mostrarse como vencida si la nueva fecha es igual o posterior a la fecha actual
And no debe mostrarse como vencida si se elimina la fecha límite

### CA-009-Vencimiento con el paso del tiempo
Given que el usuario tiene una tarea pendiente o en proceso cuya fecha límite es la fecha actual
And cierra la aplicación
When vuelve a abrirla en un día posterior
Then la tarea debe mostrarse como vencida

### CA-010-Indicador distinguible
Given que el usuario tiene tareas vencidas y no vencidas
When visualiza la lista de tareas
Then debe poder distinguir las tareas vencidas de las demás sin abrir otra vista
And el indicador de vencida no debe reemplazar ni ocultar el estado de la tarea

### CA-011-Solo se marcan las tareas vencidas
Given que el usuario tiene varias tareas registradas
And solo algunas cumplen la condición de vencida
When visualiza la lista de tareas
Then solo esas tareas deben mostrarse como vencidas
And las demás tareas deben mostrarse sin indicador de vencida

### CA-012-El indicador no modifica la tarea
Given que una tarea se muestra como vencida
When el usuario visualiza la lista de tareas
Then el título, la descripción, el estado y la fecha límite de la tarea no deben modificarse

## Reglas del Negocio
RN-001: Una tarea está vencida cuando tiene fecha límite, esta es anterior a la fecha actual y su estado es pendiente o en proceso.
RN-002: Una tarea con fecha límite igual a la fecha actual no está vencida.
RN-003: Una tarea sin fecha límite nunca está vencida.
RN-004: Una tarea finalizada nunca está vencida.
RN-005: La condición de vencida se evalúa con la fecha actual del usuario y solo considera la fecha, no la hora.
RN-006: La condición de vencida no es un dato guardado de la tarea; se determina cada vez que se visualiza la tarea.
RN-007: El indicador de vencida complementa al estado y nunca lo reemplaza.
RN-008: Mostrar una tarea como vencida es una acción de solo lectura y no modifica la tarea.

## Dependencias
Depende de HU-001-Crear una tarea, porque requiere tareas registradas.
Depende de HU-002-Visualizar tareas, porque el indicador se muestra en la lista de tareas.
Depende de HU-003-Actualizar estado de una tarea, porque el estado determina si una tarea puede estar vencida.
Depende de HU-004-Persistir tareas, porque la tarea y su fecha límite deben conservarse entre sesiones.
Depende de HU-005-Editar tarea, porque define la fecha límite y permite modificarla.

## Fuera de Alcance
* Cambiar automáticamente el estado de una tarea vencida.
* Filtrar, agrupar u ordenar tareas por vencidas.
* Contador o resumen de tareas vencidas.
* Indicar tareas próximas a vencer.
* Indicar cuántos días lleva vencida una tarea.
* Hora en la fecha límite.
* Actualización en tiempo real del indicador mientras la aplicación permanece abierta durante el cambio de día.
* Notificaciones o recordatorios de vencimiento.
* Manejo de zonas horarias distintas a la del usuario.
* Prioridades, categorías y asignación a personas.