---
name: seo-reddit
description: >
  This skill should be used when the user asks "qué están preguntando en Reddit", "preguntas de
  Reddit sobre X", "oportunidades en Reddit", "responder en Reddit", or wants to find live
  questions in the niche to answer via their website and manually on Reddit itself.
metadata:
  version: "0.1.0"
---

# Radar de preguntas en Reddit

Detectar preguntas que la gente está haciendo **ahora mismo** en Reddit sobre el nicho, y convertirlas en dos activos: respuestas listas para publicar manualmente en Reddit, y propuestas de contenido web que capturen esa demanda. Nunca publicar nada en Reddit automáticamente.

## Parámetros

- **Nicho o tema** (obligatorio): ej. "marketing digital", "SEO local", el sector de un cliente.
- **Dominio objetivo** (opcional): el sitio que respondería con contenido.
- **Subreddits**: si el usuario no los indica, proponer 3–6 relevantes (mezclar en inglés y español si existen; los subreddits en español suelen tener menos volumen pero menos competencia).
- **Ventana temporal**: por defecto, última semana.

## Flujo

### 1. Recolección de preguntas — dos vías

**Vía A (preferida): Apify, si el desktop del usuario está conectado.**

- Verificar que las herramientas `mcp__remote-devices__Apify__*` respondan.
- Buscar un actor de scraping de Reddit con `search-actors` (keywords: "reddit scraper"); revisar sus parámetros con `fetch-actor-details` antes de llamarlo.
- Ejecutar con `call-actor` sobre los subreddits elegidos, filtrando posts recientes; recoger resultados con `get-dataset-items`.

**Vía B (respaldo): sin Apify.**

- WebSearch con operadores `site:reddit.com/r/<subreddit>` + el tema, filtrando por recencia.
- `serp_organic_live_advanced` (DataForSEO) con consultas tipo "<tema> reddit" para ver qué hilos posiciona Google — esos hilos reciben tráfico de búsqueda sostenido.
- WebFetch de los hilos prometedores (usar la versión `old.reddit.com` si la moderna no carga) para leer la pregunta y las respuestas existentes.

Si la Vía A falla o el desktop no está conectado, pasar a la Vía B sin preguntar y avisar al final qué vía se usó.

### 2. Filtrado y priorización

Priorizar preguntas que cumplan: (a) recientes o con actividad reciente, (b) sin una respuesta completa y bien votada aún, (c) alineadas con lo que el dominio objetivo puede responder con autoridad, (d) idealmente en hilos que posicionan en Google (doble valor: la respuesta en Reddit también será encontrada vía búsqueda).

Para cada pregunta priorizada, validar demanda de búsqueda con `dataforseo_labs_google_keyword_overview` sobre la keyword equivalente (¿la gente también lo busca en Google?). Citar el volumen con fuente; si no hay volumen medible, decirlo (puede seguir valiendo por la conversación en Reddit).

### 3. Entregables

**Tabla de oportunidades** (una fila por pregunta): subreddit, pregunta, link al hilo, antigüedad, upvotes/comentarios, volumen de búsqueda equivalente (DataForSEO o "sin dato"), y prioridad.

**Por cada oportunidad priorizada (top 5–10):**

1. **Borrador de respuesta para Reddit**, listo para copiar y pegar manualmente: tono de redditor útil (directo, con experiencia real, sin lenguaje de marketing), en el idioma del hilo, que aporte valor completo por sí misma. Mencionar el sitio solo si aporta de verdad y de forma transparente; muchas respuestas no deben incluir enlace — en Reddit la autopromoción evidente quema la cuenta.
2. **Propuesta de contenido web**: título sugerido, keyword objetivo con volumen, y ángulo que responda la pregunta mejor que el hilo. Si el usuario quiere, encadenar con la skill `seo-contenido` para redactar el artículo.

Guardar la tabla y las propuestas en el proyecto (`investigacion/reddit-<tema>-<fecha>.md`) y enviar con SendUserFile.

## Reglas

- Nunca publicar, votar ni comentar en Reddit de forma automática — todo es material para publicación manual del usuario.
- No inventar datos de hilos (upvotes, fechas): reportar solo lo observado; si un dato no se pudo leer, marcarlo como no disponible.
- Respetar el idioma de cada hilo: respuesta en inglés para hilos en inglés.
