---
name: seo-contenido
description: >
  This skill should be used when the user asks to "crear un artículo SEO", "escribir contenido
  optimizado", "redactar un artículo para posicionar", "contenido para la keyword X", "artículo
  para el blog de [dominio]", or wants a ready-to-publish article backed by real keyword research.
  Works for any domain (multiclient); defaults to Colombia / Spanish.
metadata:
  version: "0.1.0"
---

# Creación de contenido SEO

Producir un artículo completo, optimizado y listo para publicar (borrador), respaldado por datos reales de DataForSEO. Nunca inventar métricas SEO: toda cifra debe venir de una herramienta; si no hay dato, decirlo explícitamente.

## Parámetros

- **Dominio objetivo** (obligatorio): sitio donde se publicará. Si no se indica, preguntar.
- **Tema o keyword semilla** (obligatorio): si el usuario no lo trae, proponer temas desde las oportunidades detectadas en el paso 1.
- **Mercado**: por defecto `location_name: "Colombia"`, `language_code: "es"`. Ajustar si el usuario indica otro mercado.
- **Formato de entrega**: Markdown por defecto; ofrecer también HTML o .docx (usar la skill docx si se pide).

## Flujo

### 1. Detectar oportunidades del dominio

- `dataforseo_labs_google_ranked_keywords` sobre el dominio: keywords donde ya posiciona en posiciones 5–20 (oportunidades de subir con contenido nuevo o mejorado).
- Si hay una fuente de Search Console disponible en la sesión (conector GSC dedicado, o Supermetrics con la fuente "Google Search Console" ds_id `GW` autenticada): consultar queries con muchas impresiones y CTR bajo — son las oportunidades más valiosas. Si no hay GSC disponible, decirlo y usar solo el proxy de DataForSEO.
- `dataforseo_labs_google_relevant_pages`: qué páginas ya capturan tráfico, para no canibalizar.

### 2. Investigación de la keyword

- `dataforseo_labs_google_keyword_overview` para la keyword semilla: volumen, dificultad, CPC, intención.
- `dataforseo_labs_google_keyword_ideas` y `dataforseo_labs_google_related_keywords`: cluster de keywords secundarias y long-tail.
- `dataforseo_labs_search_intent` para confirmar la intención dominante (informacional, comercial, transaccional).
- `serp_organic_live_advanced` con la keyword principal: analizar los 10 primeros resultados reales (títulos, formatos, People Also Ask, featured snippets, AI Overview si aparece).

### 3. Estructura basada en la SERP

- Definir el ángulo que permita competir: qué cubren los top 10, qué les falta, qué formato gana (guía, listado, comparativa).
- Armar outline H1–H3 que cubra la intención completa, las People Also Ask y el cluster de keywords secundarias.

### 4. Redacción

- Escribir el artículo completo en español (o el idioma del mercado indicado): mínimo la extensión mediana del top 10, tono experto pero natural, sin relleno.
- Incluir: título SEO (≤60 caracteres), meta description (≤155), slug sugerido, keywords secundarias integradas de forma natural, propuesta de enlaces internos (usando las páginas del paso 1), y sugerencia de schema markup (Article/FAQ según corresponda).
- Si la skill `humanizer` está disponible, aplicarla al borrador final para eliminar marcas de texto generado por IA.

### 5. Entrega

- Guardar el artículo como archivo y enviarlo con SendUserFile.
- Incluir al inicio del archivo un bloque resumen: keyword principal (volumen y dificultad con fuente DataForSEO), cluster secundario, intención, y competidores del top 3.
- Guardar en el proyecto (carpeta `investigacion/`) un registro breve: tema, keyword, datos clave y fecha, para no repetir investigación en futuras sesiones.

## Reglas

- Verificar antes de escribir que el tema no esté ya cubierto por una página existente del dominio (paso 1); si lo está, proponer actualización en lugar de artículo nuevo.
- Citar la fuente de cada métrica (ej. "volumen 1.300/mes — DataForSEO, Colombia").
- No publicar directamente en ningún CMS: la entrega es siempre un borrador para revisión del usuario.
