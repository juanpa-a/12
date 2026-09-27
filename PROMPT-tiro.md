Prompt: Landing page de tiro
Construye la landing page de tiro, una marca mexicana que patrocina a mexicanos chingones de verdad que las marcas ignoran por su color de piel, su clase social, su código postal o su apellido. tiro se financia vendiendo hoodies (uno por talento) con un modelo de negocio 100% abierto. Es un sitio estático de una sola página cuyo único trabajo es: plantar el manifiesto, explicar el modelo con transparencia total y juntar gente en una waitlist para el primer drop. No hay tienda todavía.
La investigación técnica y los datos ya están hechos (secciones 7, 8 y 10). El ambiente ya está preparado (sección 8). No re-investigues versiones ni cifras y no reinstales herramientas. Tu trabajo es implementar.

1. Concepto (léelo antes de diseñar nada)

* El nombre es solo "tiro": siempre en minúsculas, incluso al inicio de una oración, en el `<title>`, en el wordmark y en la OG image. Nunca "Tiro", "TIRO" ni "Tiro MX".
* El significado: "pegarnos un tiro" es slang mexicano para atrevernos a hacer algo difícil.
* La idea central: tiro es dos cartas al mismo tiempo.
   * Una carta de amor al talento mexicano: a la dedicación, la pasión y la disciplina del mexa. Al que entrena antes de ir a trabajar, al que se echa dos horas de transporte para llegar al gym, al que se hizo solo sin palancas. Esta es la mitad cálida y orgullosa, y tiene el mismo peso que la otra.
   * Una carta de odio a toda la discriminación: por color de piel (la pigmentocracia), por clase social, por de dónde vienes, por cómo hablas, por tu apellido, por lo que sea. En México la mayoría de la población es morena y no es rica, pero las marcas casi solo patrocinan a gente blanca y de clase alta. tiro no va contra una sola forma de discriminación, va contra todas.
* Qué hace tiro: patrocina a mexicanos con talento, disciplina y propósito reales: atletas, creadores, artistas, emprendedores. No por followers ni por "verse bien en la campaña", sino porque son buenísimos en lo suyo y le chingan todos los días.
* Modelo de negocio: tiro solo vende hoodies. Cada talento patrocinado tiene un solo modelo de hoodie diseñado con él o ella (al menos al inicio). Cada venta financia directamente a esa persona.
* Open business: todo el modelo es público. Cuánto cuesta hacer cada hoodie, cuánto se le paga al talento, cuánto se va a operación y cuánto queda de margen. Ventas e ingresos a la vista. La transparencia es parte del manifiesto: si vamos contra un sistema opaco, nosotros no escondemos nada.
* Metáfora visual: "tiro" como trayectoria y lanzamiento (tiro parabólico, un tiro a gol, un tiro libre), nunca como arma. Prohibido cualquier imaginería de pistolas, balas, dianas o sangre.

2. Tono del copy
Punto medio entre irreverente y orgulloso. Mexicano, directo, con humor. El copy tiene dos temperaturas y ambas tienen que sentirse: amor sin cursilería cuando habla del talento y la disciplina, y odio con filo y sarcasmo cuando habla de la discriminación.

* Sí: "chingón", "neta", "nos vamos a pegar un tiro", "a los que sí son". Una grosería fuerte ocasional está bien; que no sea la muleta.
* Sí: sarcasmo dirigido al sistema (las marcas, los castings, el "buscamos perfil aspiracional"), nunca a personas o grupos.
* No: lenguaje corporativo, "empoderamiento", "diversidad e inclusión" como slogan vacío, tono de ONG triste.
* Todo el sitio en español de México. `lang="es-MX"`.

3. Referencias de diseño (qué tomar de cada una)
La red del ambiente bloquea estos sitios: no intentes abrirlos ni hacerles fetch. Trabaja con esta descripción.

* heymumble.com: hero juguetón con tiles/objetos flotando alrededor del titular, marquee infinito con una frase repetida ("Just mumble it" → aquí "Vamos a pegarnos un tiro"), mucho aire, layout limpio.
* microsoft.ai/models: ilustraciones tipo acuarela con bordes suaves que se sienten humanas, grid editorial de cards con imagen + título + una línea. Tono humanista.
* orm.drizzle.team: irreverencia en la UI: texto tachado a mano reemplazado por otro, un muro de "testimonios" sarcásticos, detalles de easter egg. Personalidad por encima de pulcritud corporativa.

4. Sistema visual
Paleta: tonos de tierra y piel morena, no pasteles. Define tokens en el bloque `@theme` de Tailwind:

* `--color-barro` (terracota), `--color-tezontle` (rojo oscuro volcánico), `--color-maiz` (amarillo cálido), `--color-jade` (verde profundo), `--color-cacao` (casi negro café, para texto), `--color-cal` (hueso/blanco cálido, fondo principal).
* Nada de blanco puro (#fff) ni negro puro. Soporta dark mode con `prefers-color-scheme` (el variante `dark:` de Tailwind v4 ya usa la media query por default; no hace falta toggle).

Tipografía (vía Fonts API de Astro, ver sección 7):

* Display: Bricolage Grotesque (pesada, con carácter) para titulares enormes.
* Texto: Inter Tight.
* Acento: Instrument Serif itálica para palabras sueltas con énfasis y para la carta de amor.

Ilustración: formas tipo acuarela en la paleta de arriba, hechas con SVG + filtros (`feTurbulence` + `feDisplacementMap` + opacidades superpuestas). No uses fal.ai ni ningún generador de imágenes. Úsalas para conceptos (el arco del tiro, la trayectoria, manchas detrás de retratos).
* Manchas y acuarelas estáticas: diséñalas como SVG en `src/art/` y pre-renderízalas una vez a WebP con un script de Playwright (`scripts/render-art.mjs`, ver sección 8). Sírvelas con `<Image />`. Así se ven igual y no cuestan en el navegador.
* Filtros SVG en vivo solo donde haya animación (el arco del tiro con DrawSVG) y en pocas piezas.
Fotografía: retratos reales de las personas patrocinadas, recortados sobre una mancha acuarela. Por ahora son placeholders (ver sección 6). Nunca uses fotos de stock de personas.
Motivo: un arco/trayectoria parabólica dibujado a mano que atraviesa secciones y conecta el hero con la waitlist. Anímalo con GSAP DrawSVG + ScrollTrigger.

5. Estructura de la página

1. Nav mínima: wordmark "tiro" en minúsculas, un link a "Manifiesto" y un botón "Súmate" que baja a la waitlist.
2. Hero: titular gigante tipo "Nos vamos a pegar un tiro." Debajo, el truco de Drizzle: "Patrocinar a ~~los mismos güeros de Polanco de siempre~~ a los que sí le chingan." (tachón animado dibujado a mano con DrawSVG). Tiles flotando alrededor (tipo Mumble): retratos-placeholder, una mancha acuarela, etiquetas tachadas tipo "casting: se busca perfil aspiracional" y "requisito: buena presentación". CTA: "Súmate a la lista".
3. Marquee: "Vamos a pegarnos un tiro ·" repetido, en la paleta de acento.
4. El problema: 3 cifras grandes con su fuente abajo en letra chica y link. Usa exactamente las cifras verificadas de la sección 10 (las tres "principales"). No inventes ni redondees números.
5. Manifiesto: las dos cartas. Esta es la sección más importante del sitio. Dos cartas consecutivas con tratamiento visual opuesto:
   * Carta de amor ("Querido talento mexicano:"): fondo cálido (`maiz` / `barro`), serif itálica, sensación de carta escrita a mano. Habla de la dedicación, la pasión y la disciplina: los entrenamientos a las 5am, el camión de dos horas, el "me hice solo". 5–7 líneas.
   * Carta de odio ("Querida discriminación:"): fondo oscuro (`cacao` / `tezontle`), grotesca pesada, frases tachadas y reescritas. Nombra todo lo que odiamos: el color de piel, la clase, el código postal, el apellido, el "buena presentación". 5–7 líneas.
   * Cierra con una sola línea que las une: "Esto es un tiro. Y nos lo vamos a pegar."
6. Cómo elegimos: 3 criterios en cards estilo Microsoft AI, cada una con su ilustración acuarela: Son buenísimos en lo suyo, Le chingan todos los días, Tienen una propuesta, no solo un feed. Remata con: "Followers no es un criterio. Tu color, tu apellido y tu código postal tampoco."
7. Cómo funciona: 3 pasos visuales conectados por el arco del tiro: Elegimos a alguien chingón → Diseñamos su hoodie con él/ella → Cada hoodie que compras le paga directo. Remata: "Solo hoodies. Un modelo por persona. Nada más."
8. Los primeros tiros: grid de 6 perfiles placeholder. Cada card es persona + su hoodie: retrato, disciplina, una línea de propuesta y un mockup del hoodie (silueta de hoodie en SVG, en la paleta, con un detalle gráfico propio de esa persona). Etiqueta visible "Próximamente" en cada una.
9. Cuentas claras (open business): la sección estrella de transparencia.
   * Desglose de un hoodie como barra apilada o gráfica de dona (SVG a mano, sin librería de charts): producción, talento, operación/envío, margen de tiro. Cada segmento con su monto en MXN y porcentaje.
   * Métricas públicas en cards grandes: hoodies vendidos, total pagado a talentos, ingresos, gastos. Antes del lanzamiento muestran `0` con la etiqueta "Arrancamos en cero. Aquí lo vas a ver crecer."
   * Todos los números vienen de un solo archivo (`src/data/open.ts`) con comentarios explicando cada campo, para que yo los actualice. No inventes cifras de costos: usa valores placeholder obvios marcados con `TODO`.
   * Tono: "Si vamos a pelear contra un sistema opaco, empezamos por no esconder nada."
10. Lo que nos van a decir: muro tipo Drizzle de objeciones sarcásticas, firmadas por personajes ficticios y genéricos (ej. "Director de Marca Aspiracional S.A. de C.V.", "Agencia de Casting Polanco", "Tu tío en el grupo de WhatsApp"). Ejemplos: "¿Pero sí vende?", "No es racismo, es el target", "¿Y de qué escuela salió?", "Es que no tiene el perfil", "¿Solo hoodies? ¿Y las gorras?", "¿Publicar sus números? Están locos". No uses nombres de personas o marcas reales.
11. Waitlist para el primer drop: titular "Súmate antes del primer tiro." Formulario: email (requerido) + select "¿Quién eres?" (Quiero un hoodie / Soy talento / Soy marca / Nomás le echo porras). Estado de éxito con personalidad ("Ya estás. Te avisamos cuando salga el primer hoodie.").
12. Footer: wordmark, redes (placeholder), una línea tipo "Hecho en México por gente que se cansó de no verse en los anuncios."

6. Placeholders

* Perfiles: nombres y disciplinas inventados pero plausibles (atleta de box de Iztapalapa, skater de Oaxaca, chef de Hidalgo, etc.), guardados en un solo archivo de datos (`src/data/talentos.ts`) para que yo los reemplace fácil.
* Retratos: siluetas o formas abstractas sobre manchas acuarela, no fotos de stock ni caras generadas.
* Deja todos los textos de marketing en un solo archivo de contenido (`src/content/copy.ts`) para poder editarlos sin tocar componentes.
* `PUBLIC_WAITLIST_ENDPOINT` todavía no existe: crea `.env.example` con la variable vacía y un `TODO`. Sin endpoint, el formulario debe mostrar un error claro en vez de fallar en silencio.

7. Técnico (ya investigado y verificado en este ambiente, septiembre 2026)
Stack y versiones:

* Astro 7.3.x (hoy 7.3.5). Requiere Node ≥ 22.12; el ambiente tiene Node 22.22.2 y npm 10.9.7. Astro 7 usa Vite 8 con Rolldown.
* Tailwind CSS 4.3.x (hoy 4.3.3) vía `@tailwindcss/vite` registrado en `vite.plugins` del `astro.config.mjs`.
* GSAP 3.15.x (`npm install gsap`). Todos los plugins son gratis desde que Webflow compró GSAP, incluidos DrawSVG y SplitText (ya verificado: vienen en el paquete de npm). No crees `.npmrc`, no uses registry privado ni tokens.
* Sin framework de UI. Islas de JS solo donde haga falta (marquee, animaciones, formulario).

Ubicación: el repo ya tiene archivos ajenos a tiro (`12.js`, `README.md`) y la config de Claude (`.claude/`, `skills-lock.json`). Haz el scaffold de Astro en la raíz del repo sin borrar ni modificar nada de eso (si `create astro` se niega por el directorio no vacío, créalo en una carpeta temporal y muévelo). Trabaja y haz push en la rama `claude/check-local-setup-tiro-bxek6e`.

Setup de Tailwind (importante, hay trampas conocidas):

```js
// astro.config.mjs
import { defineConfig, fontProviders } from "astro/config";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  vite: { plugins: [tailwindcss()] },
  // fonts: ver abajo
});
```

```css
/* src/styles/global.css */
@import "tailwindcss";
@theme {
  /* tokens de la sección 4 */
}
```

* No uses `@astrojs/tailwind` (es para Tailwind v3) ni `@tailwindcss/postcss`: con Vite 8, la ruta de PostCSS falla al resolver `@import "tailwindcss"`.
* El bug de resolución de CSS de Rolldown solo aparece en `astro build`, no en `astro dev`. Corre `npm run build` desde el primer commit y después de cada cambio de config.
* Si el build truena con un error de Rolldown/resolver, actualiza `astro`, `vite`, `tailwindcss` y `@tailwindcss/vite` a lo último antes de intentar otro workaround.
* En `<style>` con scope dentro de componentes `.astro` que usen `@apply`, agrega `@reference "tailwindcss";` arriba.

Fuentes: usa la Fonts API integrada de Astro (estable desde Astro 6), no links a Google Fonts. `fonts.googleapis.com` y `fonts.gstatic.com` sí son accesibles desde el ambiente, así que el build puede descargarlas.

```js
fonts: [
  { provider: fontProviders.google(), name: "Bricolage Grotesque", cssVariable: "--font-display" },
  { provider: fontProviders.google(), name: "Inter Tight", cssVariable: "--font-sans" },
  { provider: fontProviders.google(), name: "Instrument Serif", cssVariable: "--font-serif" },
],
```

Y en el `<head>`: `import { Font } from "astro:assets";` → `<Font cssVariable="--font-display" preload />` (repite por familia). `docs.astro.build` está bloqueado por la red: si el tipo de config marca error, revisa los tipos en `node_modules/astro` (busca `fontProviders` y la definición de la config `fonts`) en vez de la documentación web.

GSAP:

```js
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { DrawSVGPlugin } from "gsap/DrawSVGPlugin";
gsap.registerPlugin(ScrollTrigger, DrawSVGPlugin);
```

Registra plugins una sola vez en un módulo compartido. Envuelve todas las animaciones en `gsap.matchMedia()` con `(prefers-reduced-motion: no-preference)`.

Resto:

* Formulario: envía a un endpoint configurable por variable de entorno (`PUBLIC_WAITLIST_ENDPOINT`), con validación del lado del cliente y estados loading / éxito / error. No construyas backend.
* Responsive: mobile-first. El hero y el marquee tienen que verse igual de bien en 375px.
* Accesibilidad: contraste AA con la paleta de tierra (revísalo, los tonos cálidos fallan fácil), `alt` descriptivo, foco visible, texto tachado con `<del>` / `<ins>` para lectores de pantalla.
* SEO/OG: título, descripción y una OG image (1200×630) con el wordmark y el titular del hero. Diséñala como SVG/HTML y conviértela a PNG con Playwright (mismo script de la sección 8).
* Performance: Lighthouse ≥ 95 en todo. SVGs inline solo donde se animan, acuarelas estáticas pre-renderizadas, imágenes con `<Image />` de Astro, cero peso innecesario.

8. Herramientas (ya instaladas; no las reinstales)
Al inicio solo verifica que estén cargadas. Si alguna no aparece, dime cuál y no sigas sin ella.

* Plugin `frontend-design` (claude-plugins-official, habilitado a nivel proyecto en `.claude/settings.json`). Invoca su skill antes de diseñar.
* Skills oficiales de GSAP en `.claude/skills/`: `gsap-core`, `gsap-plugins` (DrawSVG, SplitText), `gsap-scrolltrigger`, `gsap-timeline`, `gsap-performance`, `gsap-utils`. Cárgalas antes de escribir animaciones.
* Chrome DevTools MCP (`chrome-devtools`, instalado global, headless, con el Chromium de `/opt/pw-browsers`). Úsalo para ver lo que construyes: levanta `npm run dev` (o `npm run preview` para medir performance), y después de cada sección abre el sitio con `new_page`, usa `resize_page`/`emulate` para 375px y 1440px, toma screenshots con `take_screenshot` (pasa el `pageId`), revísalos críticamente contra las referencias de la sección 3 y corrige antes de avanzar. Usa `lighthouse_audit` y `performance_start_trace`/`performance_stop_trace` para performance y accesibilidad. No me digas que algo quedó bien sin haberlo visto.
* Playwright 1.56 (global) con Chromium en `/opt/pw-browsers/chromium-1194/chrome-linux/chrome` (lánzalo con `executablePath` y `--no-sandbox`). Nunca corras `playwright install`. Úsalo para `scripts/render-art.mjs`: renderizar las acuarelas SVG a WebP y la OG image a PNG.
* Generación de imágenes: no uses fal.ai ni ningún MCP de imágenes. Todo sale de SVG + filtros (sección 4).
* Red: el ambiente bloquea heymumble.com, microsoft.ai, orm.drizzle.team, docs.astro.build, skills.sh y fal.run. npm, Google Fonts y GitHub (vía git) sí funcionan. No pierdas tiempo reintentando hosts bloqueados.

9. Cómo trabajar

1. Verifica que las herramientas de la sección 8 estén cargadas (no las instales).
2. Scaffold con `npm create astro@latest` (template vacío, TypeScript estricto) en la raíz del repo, configura Tailwind, fuentes y GSAP según la sección 7, y verifica que `npm run build` pase.
3. Arma el sistema de tokens, el script de render de acuarelas y el hero completo. Revísalo con screenshots y muéstramelo antes de seguir.
4. Luego el resto de las secciones en orden. Screenshots y autocorrección después de cada una.
5. Haz commit después de cada sección con `npm run build` limpio, y push a `claude/check-local-setup-tiro-bxek6e`.
6. Al final: revisión completa en mobile y desktop, Lighthouse, `npm run build` limpio, y dime qué quedó como `TODO`.

Si algo del concepto o del tono no te queda claro, pregunta antes de inventar.

10. Datos verificados para "El problema"
Todos tienen fuente primaria o secundaria confiable. Úsalos tal cual, con su fuente abajo.
Las tres principales (van en la sección 4 de la página):

1. 1 de cada 4. 23.7% de la población de 18+ en México dijo haber sido discriminada en los últimos 12 meses (subió desde 20.2% en 2017). Fuente: INEGI, ENADIS 2022. https://www.conapred.org.mx/encuesta-nacional-sobre-discriminacion-enadis-2022/
2. 13.9% vs 27.1%. Entre las personas de piel más oscura, 13.9% llegó a ser director, jefe, profesionista o técnico. Entre las de piel más clara, 27.1%. Fuente: INEGI, Módulo de Movilidad Social Intergeneracional (MMSI) 2016. https://www.diariopresente.mx/mexico/inegi-el-color-de-piel-influye-en-empleo-y-salario/194197
3. 2.7% vs 53.5%. Solo 2.7% de quienes vienen de los orígenes socioeconómicos más desfavorecidos hizo estudios superiores, contra 53.5% de quienes vienen de los más privilegiados. Fuente: Oxfam México / Colmex, "Por mi raza hablará la desigualdad" (2019). https://imco.org.mx/informe-raza-hablara-la-desigualdad-via-oxfam/

De reserva (para copy, tooltips o variaciones):

* Personas con piel más oscura (escala PERLA A–E) vs más clara (H–K): contrato laboral 32.8% vs 44.0%; preparatoria o más 38.2% vs 55.1%; acceso a salud 49.7% vs 60.6%. Fuente: INEGI con datos ENADIS 2022 (comunicado 50/25, marzo 2025). https://inegi.org.mx/contenidos/saladeprensa/aproposito/2025/EAP_DiscRacial.pdf
* 26.5% de quienes se autoidentifican con tonos de piel oscuros reportó discriminación en el último año. Misma fuente.
* Entre quienes fueron discriminados, los motivos incluyen: forma de vestir 30.6%, manera de hablar 21.6%, clase social 16.5%, lugar donde vive 15.7%, tono de piel 13.1%. Fuente: ENADIS 2022 vía Gaceta UNAM. https://www.gaceta.unam.mx/crece-el-mexico-discriminador/
* Las personas con tonos de piel oscuros tienen una probabilidad 28% menor (hombres) y 45% menor (mujeres) de llegar al nivel económico más alto que las de piel clara. Fuente: Oxfam México / Colmex 2019, vía IMCO (link de arriba).

Regla: escribe cada cifra con la precisión de la fuente, di siempre de qué año es y no mezcles cifras de encuestas distintas en una misma comparación.
