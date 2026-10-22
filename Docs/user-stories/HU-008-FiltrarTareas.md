# HU-008-Filtrar tareas

## Historia de usuario
como integrante del equipo, quiero filtrar mis tareas por estado, para concentrarme en las tareas pendientes, en proceso o finalizadas cuando mi lista crece.

## Criterio de aceptación
### CA-001-Ver todas las tareas por defecto
Given que el usuario tiene tareas en distintos estados
When accede a la vista de tareas sin haber aplicado un filtro
Then deben mostrarse todas sus tareas
And debe quedar claro que no hay ningún filtro aplicado

### CA-002-Filtrar por un estado
Given que el usuario tiene tareas pendientes, en proceso y finalizadas
And elige filtrar por un estado
When aplica el filtro
Then solo deben mostrarse las tareas que tienen ese estado
And las tareas de los otros estados no deben mostrarse

### CA-003-Filtro activo visible
Given que el usuario aplicó un filtro por estado
When visualiza la lista de tareas
Then debe poder identificar qué filtro está aplicado
And cada tarea mostrada debe conservar su estado visible

### CA-004-Cambiar de filtro
Given que el usuario tiene aplicado un filtro por estado
And elige filtrar por otro estado
When aplica el nuevo filtro
Then deben mostrarse solo las tareas del nuevo estado
And no deben seguir mostrándose las tareas del filtro anterior

### CA-005-Quitar el filtro
Given que el usuario tiene aplicado un filtro por estado
When elige volver a ver todas las tareas
Then deben mostrarse todas sus tareas
And no debe quedar ningún filtro aplicado

### CA-006-Filtro sin resultados
Given que el usuario tiene tareas registradas
And ninguna tiene el estado elegido en el filtro
When aplica el filtro
Then debe informarse que no hay tareas con ese estado
And debe poder quitar el filtro para ver todas sus tareas

### CA-007-Sin tareas registradas
Given que el usuario no tiene tareas registradas
When accede a la vista de tareas
Then debe informarse que aún no hay tareas registradas
And este mensaje debe diferenciarse del mensaje de filtro sin resultados

### CA-008-Orden conservado al filtrar
Given que el usuario tiene varias tareas del estado elegido
When aplica el filtro
Then las tareas mostradas deben conservar el orden definido en HU-002
And el orden no debe alterarse al quitar el filtro

### CA-009-Tarea que cambia de estado con filtro aplicado
Given que el usuario tiene aplicado un filtro por estado
And cambia el estado de una tarea mostrada a un estado distinto del filtrado
When se completa el cambio
Then la tarea debe dejar de mostrarse en la lista filtrada
And la tarea debe conservar su nuevo estado y aparecer al quitar el filtro

### CA-010-Nueva tarea con filtro aplicado
Given que el usuario tiene aplicado un filtro por estado
And registra una nueva tarea correctamente
When se completa el registro
Then la tarea debe crearse en estado pendiente
And solo debe mostrarse en la lista si el filtro aplicado es pendiente
And debe aparecer al quitar el filtro

### CA-011-Editar o eliminar con filtro aplicado
Given que el usuario tiene aplicado un filtro por estado
When edita o elimina una tarea mostrada
Then la edición o eliminación debe funcionar igual que sin filtro
And el filtro debe seguir aplicado

### CA-012-El filtro no modifica las tareas
Given que el usuario aplica, cambia o quita un filtro
When visualiza la lista de tareas
Then ninguna tarea debe modificarse ni eliminarse
And el estado, el título y la descripción de todas las tareas deben conservarse

### CA-013-Filtro no se conserva entre sesiones
Given que el usuario tiene aplicado un filtro por estado
When cierra y vuelve a abrir la aplicación
Then deben mostrarse todas sus tareas
And no debe haber ningún filtro aplicado

## Reglas del Negocio
RN-001: Por defecto se muestran todas las tareas, sin ningún filtro.
RN-002: El filtro se aplica por estado: pendiente, en proceso o finalizada.
RN-003: Solo puede aplicarse un filtro de estado a la vez.
RN-004: Una tarea se muestra con un filtro aplicado únicamente si su estado coincide con el filtro.
RN-005: Las tareas filtradas conservan el orden de HU-002.
RN-006: Filtrar es una acción de solo lectura y no modifica ni elimina tareas.
RN-007: El filtro aplicado debe ser visible para el usuario.
RN-008: Un filtro sin resultados no es un error y se informa de forma distinta a no tener tareas registradas.
RN-009: El filtro es temporal y no se conserva entre sesiones.

## Dependencias
Depende de HU-001-Crear una tarea, porque requiere tareas registradas.
Depende de HU-002-Visualizar tareas, porque el filtro actúa sobre la lista de tareas y conserva su orden.
Depende de HU-003-Actualizar estado de una tarea, porque el estado es el criterio del filtro y puede cambiar con el filtro aplicado.
No depende de HU-004-Persistir tareas, ya que el filtro no se conserva.
HU-005-Editar tarea y HU-006-Eliminar tarea no son requisito, pero deben funcionar con un filtro aplicado.

## Fuera de Alcance
* Filtrar por fecha límite.
* Filtrar por tareas vencidas.
* Filtrar por título o descripción (búsqueda).
* Combinar varios filtros a la vez.
* Ordenar tareas por criterios elegidos por el usuario.
* Agrupar tareas por estado.
* Contador de tareas por estado.
* Conservar el filtro entre sesiones.
* Guardar filtros favoritos.
* Filtrar por personas, prioridades o categorías.