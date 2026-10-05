# SPEC-000: Foundation — TaskFlow Lite

| Campo | Valor |
| --- | --- |
| Estado | Borrador |
| Tipo | Técnica / fundacional |
| Historias funcionales | Ninguna (se definen en specs posteriores) |

## 1. Propósito

Definir la base técnica y de trabajo sobre la que se construirá TaskFlow Lite, de modo que las specs funcionales posteriores partan de un proyecto ejecutable, ordenado y con reglas compartidas.

Esta spec no entrega funcionalidad de producto. Entrega un esqueleto que se pueda abrir localmente, con estructura y convenciones acordadas.

## 2. Alcance

Incluye:

- Estructura inicial de carpetas y archivos.
- Restricciones técnicas del proyecto.
- Convenciones de nombres.
- Convenciones de Git.
- Una página base que cargue sus estilos y su módulo JavaScript sin errores.
- Instrucciones para ejecutar el proyecto localmente desde VSCode.
- Definición de Done técnica inicial.

## 3. Estructura inicial del proyecto

```text
taskflow-lite/
├── index.html
├── README.md
├── .gitignore
├── docs/
│   └── specs/
│       └── SPEC-000-foundation.md
└── src/
    ├── css/
    │   └── styles.css
    └── js/
        └── main.js
```

| Elemento | Responsabilidad |
| --- | --- |
| `index.html` | Punto de entrada. Carga `src/css/styles.css` y `src/js/main.js` (como `type="module"`). |
| `README.md` | Descripción breve del proyecto y pasos para ejecutarlo localmente. |
| `.gitignore` | Excluye archivos de sistema y de editor que no deben versionarse. |
| `docs/specs/` | Specs del proyecto, una por archivo. |
| `src/css/styles.css` | Estilos base. |
| `src/js/main.js` | Punto de entrada de JavaScript. |

Reglas de la estructura:

- Todo el código de la aplicación (CSS y JS) vive dentro de `src/`.
- Las carpetas o módulos adicionales se crean solo cuando una spec posterior los requiera.
- No se crean carpetas vacías por anticipado.

## 4. Restricciones técnicas

1. **Lenguajes:** HTML5, CSS3 y JavaScript con ES Modules (`import` / `export`).
2. **Sin frameworks ni librerías** de UI, estado o utilidades.
3. **Sin backend:** no hay servidor propio ni llamadas a APIs externas.
4. **Persistencia futura:** `localStorage`. No se implementa en esta spec.
5. **Sin proceso de build:** sin bundlers, transpiladores ni gestores de paquetes en el producto. Lo que se escribe es lo que ejecuta el navegador.
6. **Ejecución local desde VSCode:** los ES Modules no cargan de forma fiable al abrir `index.html` con doble clic (protocolo `file://`), porque el navegador bloquea la carga de módulos desde ese origen. El proyecto debe ejecutarse mediante un servidor estático local. Opción acordada: la extensión **Live Server** de VSCode. Esta extensión es una herramienta de desarrollo, no una dependencia del proyecto.
7. **Navegadores objetivo:** versiones recientes de un navegador moderno con soporte de ES Modules y `localStorage`.

## 5. Convenciones de nombres

| Elemento | Convención | Ejemplo |
| --- | --- | --- |
| Carpetas | `kebab-case`, minúsculas | `src/js/`, `docs/specs/` |
| Archivos HTML, CSS y JS | `kebab-case` | `main.js`, `styles.css` |
| Variables y funciones JS | `camelCase` | `taskList` |
| Clases JS | `PascalCase` | `TaskStore` |
| Constantes globales JS | `UPPER_SNAKE_CASE` | `STORAGE_KEY` |
| Clases CSS | `kebab-case` | `task-list` |
| IDs HTML | `kebab-case`, solo cuando sean necesarios | `app-root` |
| Claves de `localStorage` | Prefijo `taskflow-lite:` | `taskflow-lite:tasks` |
| Specs | `SPEC-NNN-nombre-corto.md` | `SPEC-000-foundation.md` |

Reglas generales:

- Nombres en inglés en el código; documentación en español.
- Nombres descriptivos; evitar abreviaturas poco claras.

## 6. Convenciones de Git

- **Rama principal:** `main`, siempre en estado funcional.
- **Ramas de trabajo:** una por spec o cambio, con el formato `tipo/spec-NNN-descripcion-corta`, por ejemplo `feature/spec-001-registrar-tarea`.
- **Mensajes de commit:** formato Conventional Commits, `tipo: descripción en imperativo`.
  - Tipos permitidos: `feat`, `fix`, `docs`, `style`, `refactor`, `chore`.
  - Ejemplo: `docs: add SPEC-000 foundation`.
- **Commits pequeños:** un propósito por commit.
- **Referencia a la spec:** el cuerpo del commit o la descripción del cambio menciona la spec que lo origina.
- **Integración a `main`:** solo cuando se cumple la Definition of Done de la spec correspondiente.
- **Archivos a ignorar (`.gitignore`):** archivos de sistema operativo (por ejemplo `.DS_Store`, `Thumbs.db`) y configuración personal de editor que no sea compartida.

## 7. Definition of Done técnica inicial

Esta DoD aplica a SPEC-000 y es el punto de partida para las specs posteriores.

- [ ] La estructura de carpetas coincide con la sección 3.
- [ ] `index.html` es HTML5 válido y declara `lang`, `charset` y `viewport`.
- [ ] `index.html` carga `src/css/styles.css` y `src/js/main.js` con `type="module"`.
- [ ] El proyecto se abre desde VSCode con Live Server y la página carga sin errores en la consola del navegador.
- [ ] No se usan frameworks, librerías externas, backend ni herramientas de build.
- [ ] No existe lógica funcional de producto (tareas, estados, persistencia).
- [ ] El `README.md` explica cómo ejecutar el proyecto localmente.
- [ ] Los nombres de archivos y carpetas cumplen la sección 5.
- [ ] Los cambios están en commits que cumplen la sección 6.
- [ ] La verificación se realizó manualmente y su resultado quedó registrado por quien la ejecutó.

## 8. Fuera de alcance

- Historias funcionales: registrar, visualizar o actualizar tareas.
- Implementación de `localStorage`.
- Diseño visual detallado, sistema de diseño o identidad gráfica.
- Pruebas automatizadas y herramientas de linting o formateo.
- Compartir datos entre personas o dispositivos.
- Autenticación, usuarios y roles.
- Despliegue o publicación del sitio.
- Accesibilidad avanzada y soporte para navegadores antiguos.

## 9. Decisiones abiertas

| Tema | Pregunta | Efecto |
| --- | --- | --- |
| Servidor local | ¿Se confirma Live Server como forma oficial de ejecución? | Define el contenido del `README.md` y el paso de verificación de la DoD. |
| Datos compartidos | ¿El equipo usará un solo navegador o necesita compartir datos? | No afecta a esta spec, pero sí a las specs funcionales posteriores. |