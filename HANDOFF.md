# Handoff — Website Fotografía Daniel Vidal

Última actualización: 2026-08-14

## Qué es esto

Sitio web personal de fotografía (bodas, eventos, branding, deportiva) de Daniel Vidal, en Sydney. Es un **sitio estático de una sola página (SPA sin build)**: todo vive en dos archivos HTML gigantes con CSS y JS inline, sin frameworks ni pasos de compilación.

- **Dominio en vivo:** https://danielvidalt.com/
- **Repo:** https://github.com/danielvidalt/Website-Fotografo-Daniel-Vidal
- **Hosting:** GitHub Pages, servido desde la rama `gh-pages`
- **Deploy:** automático — cualquier `git push` a `main` dispara `.github/workflows/deploy.yml`, que copia todo a `gh-pages`. No hay que hacer nada manual para publicar.
- ⚠️ El CDN de GitHub Pages cachea ~10 minutos. Después de un push, el cambio puede tardar unos minutos en verse en el dominio aunque el deploy ya haya terminado.

## Archivos clave

- `index.html` — sitio en **inglés** (idioma por defecto de la raíz del dominio)
- `es/index.html` — sitio en **español** (casi un duplicado completo de `index.html`, mismo HTML/CSS/JS)
- `Photos webp/` — todas las fotos, organizadas por sesión/carpeta
- `.github/workflows/deploy.yml` — el workflow de deploy (no debería necesitar cambios)

**Importante:** casi cualquier cambio de contenido/diseño hay que hacerlo **en los dos archivos** (`index.html` y `es/index.html`) porque no comparten un template — son dos copias independientes. Si solo se edita uno, ese idioma queda desincronizado del otro.

## Cómo está armado el sitio

Es una SPA hecha a mano con JS vanilla (sin librerías):

- Todas las "páginas" (Inicio, Bodas, Eventos, Branding, **Deportiva**, Sobre mí) son `<div class="page" id="page-XXX">` dentro del mismo HTML, y JS muestra/oculta con `display` + clases `active`/`visible`. No hay rutas de verdad (todo es una sola URL).
- **Deportiva es la página principal** (la que carga por defecto al entrar al sitio). El resto de las secciones están agrupadas en un botón desplegable "Sections"/"Secciones" en el nav.
- El sistema de traducción es un objeto `T = {es:{...}, en:{...}}` en el JS; los elementos con `data-i18n`/`data-i18n-html` se traducen al vuelo con `applyTranslations()`.
- No hay build ni bundler. Para probar cambios localmente: `python3 -m http.server 8000` desde la carpeta del proyecto y abrir `localhost:8000`.

## Trabajo reciente (esta sesión)

Todo esto ya está en `main` y desplegado:

1. Deportiva convertida en página principal; resto de secciones movidas a un menú desplegable "Sections"
2. Galería de Deportiva reordenada: una sola foto por partido (la portada real de cada colección de Pixieset), cada una linkeando a su colección específica
3. Cada foto muestra una ficha con toda la info del partido: equipos, ronda, categoría, fecha/hora, cancha — con fondo semitransparente con blur para legibilidad garantizada
4. Encabezado de la sección rediseñado: título "ESFA Winter 2026" grande y visible, botón del hero pequeño ("Book Match Coverage"), eliminados los links redundantes ("Book a session", "View full gallery"), eliminado el título "Sport in action"
5. **Filtros de búsqueda** en Deportiva: Round, Fecha, Categoría, Club, Cancha (combinables, con reset y estado vacío)
6. Instagram del footer diferenciado por sección: Deportiva → `@danividal.sport`, Bodas → `@danividal.wedding`, resto → `@danielvidal.photography`
7. Varios fixes de bugs móviles: menú "Sections" estaba oculto en pantallas chicas (bug real, ya corregido), cursor personalizado desactivado en táctil (consumía CPU sin sentido), blur del nav desactivado en móvil, flicker del nav al cruzar el scroll de 60px corregido

## Pendiente / a tener en cuenta

- La carpeta `Photos webp/ESFA WINTER 26/` todavía tiene las **18 fotos viejas** que se usaban en la galería anterior (antes de pasar a "una foto por partido"). No se borraron, solo dejaron de mostrarse. Se pueden eliminar si ya no se necesitan.
- El link `spt.pixieset` (traducción) quedó sin usar en el HTML tras el rediseño del encabezado — no rompe nada, es solo texto muerto en el diccionario de traducciones.
- Cuando lleguen más partidos de ESFA para agregar a la galería, el patrón a seguir es: pedir el link de Pixieset → sacar la portada real de esa colección (Pixieset bloquea scraping directo, hay que usar un navegador headless con pausas entre requests para esquivar el challenge de Cloudflare) → agregar como nuevo `<a class="brand-item">` con sus `data-round`, `data-date`, `data-division`, `data-clubs`, `data-pitch` para que los filtros lo detecten automáticamente.

## Cómo seguir trabajando desde otra computadora

1. **Clonar el repo** (si Claude Code no lo tiene ya):
   ```
   git clone https://github.com/danielvidalt/Website-Fotografo-Daniel-Vidal.git
   ```
2. Abrir Claude Code en esa carpeta y pedirle que lea este archivo (`HANDOFF.md`) para tener contexto — no hace falta que repitas toda la historia.
3. Para pedir un cambio, sé específico sobre **qué sección** (Deportiva, Bodas, etc.) — el sitio no tiene nombres de archivo por sección, todo vive en los mismos dos HTML.
4. Recuerda: **todo cambio de contenido debe hacerse en `index.html` Y `es/index.html`** salvo que pidas explícitamente solo un idioma.
5. Cualquier `git push` a `main` publica automáticamente — no hace falta pedir "deploy" aparte, pero sí puede pedirse confirmar que ya se ve en vivo (ten en cuenta el retraso de caché de ~10 min).
6. Si algo "no se ve" después de un cambio, antes de asumir que hay un bug: probar en incógnito / otro dispositivo, porque casi siempre es caché del navegador o del CDN, no el código.
