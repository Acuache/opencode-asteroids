# AGENTS.md

## Resumen
Juego arcade HTML5 canvas de un solo archivo (UI en español: "NIVEL", "GAME OVER"). Sin frameworks, sin bundler, sin dependencias, sin package.json.

## Ejecutar / verificar
- Abre `index.html` directamente en el navegador, o `npx serve .` y visita `http://localhost:3000`.
- No existe tooling de test/lint/build. La única verificación es manual en el navegador.

## Estructura
- `game.js` — toda la lógica, clases y bucle del juego; se importa con `<script src>` en `index.html`. El estado global (`ship`, `bullets`, `asteroids`, `particles`, `state`, etc.) vive en el nivel superior del módulo de `game.js`.
- `index.html` — el canvas es fijo de `800x600`; las constantes JS `W`/`H` en `game.js` deben coincidir o el render se rompe.

## Convenciones / gotchas
- `'use strict'` al inicio de `game.js`. El código usa clases ES6+ con helpers de fábrica (`wrap`, `rand`, `dist`) — reutilízalos, no los reinventes.
- El canvas 800x600 es toroidal: `wrap(v, max)` en `game.js:27`; las posiciones de las entidades se envuelven con `W`/`H`. Mantén esto en cualquier entidad nueva (balas, asteroides, partículas) o volarán fuera de pantalla.
- Los pools de objetos son arreglos simples filtrados en su lugar cada frame (`bullets = bullets.filter(...)`); las clases nuevas deben exponer `update(dt)`/`draw()` y una bandera `dead`.
- La entrada usa `e.code` (`Space`, `ArrowUp`, ...) con `justPressed` para acciones de flanco — usa `pressed('Space')` para eventos de un solo disparo, `keys[...]` para estado sostenido.
- `dt` está limitado a `0.05` en `loop()`; todo movimiento debe escalarse por `dt`.
- Estados del juego: `'playing' | 'dead' | 'gameover'` (variable `state`). La lógica nueva debe ramificar en `update()`.
- El texto/HUD del juego es en español; mantén los strings nuevos de UI en español.
- Sin conflicto de estilo de comentarios: el código existente usa banners `//` de sección; los comentarios en línea menores son aceptables.