# SPEC-000: Foundation

| Campo | Valor |
|-------|-------|
| Proyecto | TaskFlow Lite |
| Estado | Borrador |
| Tipo | Fundación técnica (sin historias funcionales) |

---

## 1. Propósito

Establecer la base técnica y las reglas de trabajo de TaskFlow Lite antes de implementar cualquier historia funcional. Esta SPEC define cómo se organiza el proyecto, qué restricciones técnicas se respetan, qué convenciones se siguen y cuándo se considera que un incremento técnico está terminado.

Su objetivo es que las siguientes SPECs (funcionales) partan de un terreno común y no tengan que redefinir estas decisiones.

---

## 2. Alcance

Esta SPEC cubre:

- La estructura inicial de carpetas y archivos del proyecto.
- Las restricciones técnicas que aplican a todo el proyecto.
- Las convenciones de nombres para archivos, código, CSS y claves de almacenamiento.
- Las convenciones de uso de Git.
- La Definition of Done técnica inicial.
- La forma de ejecutar el proyecto localmente desde VSCode.

Esta SPEC **no** cubre ninguna funcionalidad de tareas (registrar, visualizar, editar, avanzar estado, eliminar ni persistir). Esas capacidades se tratarán en SPECs posteriores.

---

## 3. Estructura inicial del proyecto

```text
taskflow-lite/
├── .gitignore
├── README.md
├── index.html
├── docs/
│   └── specs/
│       └── SPEC-000-foundation.md
└── src/
    ├── css/
    │   └── styles.css
    └── js/
        └── main.js
```

### Descripción de elementos

| Ruta | Responsabilidad |
|------|-----------------|
| `README.md` | Descripción breve del proyecto y cómo ejecutarlo localmente. |
| `.gitignore` | Excluye archivos que no deben versionarse (sistema operativo, editor, etc.). |
| `index.html` | Documento HTML5 de entrada, ubicado en la raíz junto a `README.md`. Carga `src/css/styles.css` y el módulo `src/js/main.js`. |
| `docs/specs/` | Especificaciones del proyecto, una por archivo. |
| `src/css/styles.css` | Hoja de estilos de la aplicación. |
| `src/js/main.js` | Punto de entrada de JavaScript. Se carga como módulo (`type="module"`). |

### Reglas de crecimiento de la estructura

- Los estilos (CSS) y los scripts (JS) de la aplicación viven dentro de `src/`.
- `index.html` es el único archivo de la aplicación que se ubica en la raíz del proyecto.
- No se crean carpetas adicionales dentro de `src/js/` hasta que una SPEC funcional las requiera.
- Cuando una SPEC funcional requiera separar responsabilidades, se agrupará el código por responsabilidad dentro de `src/js/` (por ejemplo, lógica de dominio, acceso a almacenamiento e interfaz). La estructura concreta se decidirá en esa SPEC, no aquí.

---

## 4. Restricciones técnicas

| # | Restricción |
|---|-------------|
| 1 | Lenguajes permitidos: HTML5, CSS3 y JavaScript con ES Modules. |
| 2 | Sin frameworks ni librerías de interfaz. |
| 3 | Sin backend. Toda la lógica se ejecuta en el navegador. |
| 4 | La persistencia de datos usará `localStorage`. No se implementa en esta SPEC. |
| 5 | Sin herramientas de compilación ni empaquetado (bundlers, transpiladores, preprocesadores CSS). |
| 6 | Sin dependencias externas (no CDN, no gestor de paquetes) salvo que se indique lo contrario en una SPEC posterior. |
| 7 | El proyecto debe poder ejecutarse localmente desde VSCode. |
| 8 | El código CSS y JavaScript se organiza dentro de `src/`. `index.html` se ubica en la raíz, a la misma altura que `README.md`. |

### Ejecución local

Los ES Modules **no se cargan de forma confiable al abrir `index.html` directamente con `file://`**, porque los navegadores bloquean la carga de módulos desde ese protocolo. Por eso el proyecto se ejecuta sirviendo la raíz del proyecto por HTTP local.

- Método previsto: extensión **Live Server** de VSCode, abriendo `index.html` desde la raíz del proyecto.
- Esta extensión es una herramienta de desarrollo y no una dependencia del proyecto: no se agrega al código ni a la estructura.

---

## 5. Convenciones de nombres

### Archivos y carpetas

| Elemento | Convención | Ejemplo |
|----------|------------|---------|
| Archivos y carpetas del código | `kebab-case`, en minúsculas | `main.js`, `task-list.js` |
| Archivos de especificación | `SPEC-NNN-nombre-en-kebab-case.md` | `SPEC-000-foundation.md` |

### JavaScript

| Elemento | Convención | Ejemplo |
|----------|------------|---------|
| Variables y funciones | `camelCase` | `taskList`, `getTasks` |
| Constantes de valor fijo | `UPPER_SNAKE_CASE` | `STORAGE_KEY` |
| Clases (si se usan) | `PascalCase` | `Task` |
| Funciones booleanas | Prefijo `is`, `has` o `can` | `isCompleted` |

### HTML y CSS

| Elemento | Convención | Ejemplo |
|----------|------------|---------|
| Atributos `id` | `kebab-case` | `task-form` |
| Clases CSS | `kebab-case` con estilo bloque-elemento | `task-card`, `task-card-title` |
| Variables CSS | `kebab-case` con prefijo `--` | `--color-primary` |

### Almacenamiento

| Elemento | Convención | Ejemplo |
|----------|------------|---------|
| Claves de `localStorage` | Prefijo `taskflow-lite:` seguido de `kebab-case` | `taskflow-lite:tasks` |

### Idioma

- Nombres en código (variables, funciones, archivos, clases CSS): **inglés**.
- Texto visible para la persona usuaria, documentación y mensajes de commit: **español**.

---

## 6. Convenciones de Git

### Ramas

| Rama | Uso |
|------|-----|
| `main` | Rama estable. Solo contiene trabajo que cumple la Definition of Done. |
| `feature/SPEC-NNN-descripcion-corta` | Una rama por SPEC o incremento. |

Ejemplo: `feature/SPEC-000-foundation`.

### Commits

Formato: `tipo(alcance): descripción breve en español`

| Tipo | Uso |
|------|-----|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de un error |
| `docs` | Cambios en documentación o SPECs |
| `style` | Cambios de formato o estilos que no alteran la lógica |
| `refactor` | Reorganización de código sin cambiar el comportamiento |
| `chore` | Tareas de mantenimiento (por ejemplo, `.gitignore`) |

Ejemplos:

- `docs(spec): agregar SPEC-000 de fundación`
- `chore(repo): agregar .gitignore`

### Reglas

1. Commits pequeños y con un solo propósito.
2. No se hace commit de trabajo que rompa la ejecución local del proyecto.
3. Cada rama se integra a `main` solo cuando cumple la Definition of Done.
4. El nombre de la rama y los mensajes de commit referencian el número de SPEC cuando aplique.

---

## 7. Definition of Done técnica inicial

Un incremento se considera terminado cuando cumple todo lo siguiente:

- [ ] La estructura de archivos coincide con la definida en esta SPEC (o con la SPEC vigente que la extienda).
- [ ] El proyecto se abre y se ejecuta localmente desde VSCode sirviendo la raíz del proyecto por HTTP.
- [ ] `index.html` está en la raíz, a la misma altura que `README.md`, es HTML5 válido y declara el idioma del documento.
- [ ] `main.js` se carga como módulo (`type="module"`) sin errores en la consola del navegador.
- [ ] `styles.css` se carga correctamente.
- [ ] No se usan frameworks, librerías externas, backend ni herramientas de compilación.
- [ ] Los nombres de archivos, código, clases CSS y claves de almacenamiento respetan las convenciones de esta SPEC.
- [ ] Los cambios están en commits que siguen la convención de Git definida.
- [ ] El `README.md` indica cómo ejecutar el proyecto localmente.
- [ ] No se agregó funcionalidad ni requisitos que no estén en la SPEC correspondiente.
- [ ] La verificación se realizó manualmente y se dejó constancia de lo que se revisó.

---

## 8. Fuera de alcance

- Cualquier historia funcional: registrar, visualizar, editar, avanzar estado o eliminar tareas.
- Implementación de la persistencia en `localStorage`.
- Diseño visual detallado, sistema de diseño y componentes de interfaz.
- Pruebas automatizadas y herramientas de calidad de código (linters, formateadores).
- Despliegue o publicación.
- Compartir datos, sincronización, asignación a otros miembros y vistas de equipo.
- Soporte para navegadores antiguos o requisitos de accesibilidad formales (se definirán en SPECs posteriores si se requieren).

