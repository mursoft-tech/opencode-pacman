# AGENTS.md

Pac-Man MVP en Vanilla JS. Repo de aprendizaje para **Spec Driven Development**: el objetivo no es la complejidad del juego sino practicar el flujo de specs.

## Skills del repo (lo más importante)

Hay dos skills locales en `.agents/skills/`, invocables como `/spec` y `/spec-impl`. Están fijadas en `skills-lock.json` (origen `klerith/fernando-skills`); no editarlas a mano.

- `/spec <descripción>` diseña una spec y la guarda en `specs/NN-slug.md`. **No escribe código.** Trabaja en el idioma del prompt (el repo usa español).
- `/spec-impl <NN-slug>` implementa una spec. **Bloquea si el `Status` de la spec no es "Approved"/"Aprobado".** Crea y cambia a la rama `spec-NN-slug` (controlado por `AutoCreateBranch` en `specs/.spec-config.yml`, default `true`).
- `specs/` todavía no existe; el primer spec será `01-`. Lee `.agents/skills/spec/template.md` para el formato exacto.
- Ninguna de las dos skills commitea por su cuenta: el commit es decisión del usuario.

## Ejecutar

No hay build, bundler, tests, lint ni `package.json`. Simplemente abre `src/index.html` en un navegador (o sírvelo con cualquier servidor estático). No inventes scripts de npm.

## Arquitectura (no obvia desde los nombres)

- Scripts globales clásicos cargados **en orden** en `src/index.html`: `maze.js` → `game.js` → `render.js` → `main.js`. **Sin ES modules ni imports**: todo se comunica por `window` (`window.MAZE`, `window.createGame`, `window.update`, `window.draw`, `window.DIRS`). Añadir un archivo nuevo requiere su `<script>` en el orden correcto.
- `maze.js` (`MAZE`) es prístino e **inmutable**: cada partida lo copia con `createGame()` a `game.grid`. Nunca mutar `MAZE`.
- Coordenadas del juego en **celdas (flotantes)**, no píxeles; el render convierte con `TILE=20` (`render.js`). El movimiento se mueve en incrementos de `1/speed` por frame para alinearse a celdas enteras.
- Valores del grid: `1` pared, `2` dot, `3` puerta de pen, `0` vacío.

## Estilo

Sin linter. Imita el código existente: comillas simples, indentación de 2 espacios y **espacios dentro de paréntesis** (`if ( x ) { ... }`). Comentarios y textos de UI en español.
