# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Qué es

Página estática de una sola vista (`index.html`) para comparar y votar opciones de hotel+spa en Bogotá (escapada de cumpleaños, 22–23 agosto). Todo el contenido está en español.

## Arquitectura

**Un solo archivo, sin build, sin dependencias, sin tests.** `index.html` contiene HTML + `<style>` + `<script>` inline. No hay npm, bundler ni servidor: abrir el archivo en el navegador (o `python3 -m http.server`) es todo el flujo de desarrollo.

Restricciones a mantener al editar:

- **No introducir tooling ni dependencias.** Cualquier cambio va dentro de `index.html`. No agregar frameworks, CSS externo ni paquetes. Tampoco fuentes auto-hospedadas: decisión explícita del autor (14 ago 2026) tras ofrecerle cambiar la cara de display. La voz tipográfica es Georgia + `system-ui`; no declarar caras que la página no envía, porque entonces cada lector ve una distinta.
- **Cada opción es un `<article class="card" data-option="Nombre">`.** El `data-option` es la clave de identidad: se guarda en `localStorage` bajo `spaChoice` y se compara en `paint()`. Si se renombra una tarjeta, los votos guardados de ese nombre dejan de coincidir.
- **Las portadas son screenshots automáticos** vía `https://s.wordpress.com/mshots/v1/<url-encoded>?w=1000`, con `onerror` que oculta la imagen y deja ver el gradiente de fondo (`.cover.rose/.plum/.sage/.gold/.blue/.night`). Al añadir una opción hay que url-encodear la URL oficial en el `src`.
- **Estado del voto:** solo `localStorage`, un dispositivo, sin backend. `shareChoice()` usa `navigator.share` con fallback a `navigator.clipboard`.
- **Paleta y layout** salen de las variables CSS en `:root` (vino/rosa/salvia) y de tres breakpoints (930px y 620px). Reusarlas en vez de valores sueltos.

## Datos

Precios, fechas y disponibilidad son datos verificados a mano contra las páginas oficiales enlazadas (ver fecha en el `<footer>`). Al cambiar cifras, actualizar también esa fecha y el bloque `.notice`/`.status` correspondiente si cambia la vigencia.
