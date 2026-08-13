---
name: seo-pagespeed
description: >
  This skill should be used when the user asks to "arreglar el PageSpeed", "mejorar la velocidad
  del sitio", "auditar Core Web Vitals", "correr Lighthouse", "por qué carga lento [dominio]",
  or wants concrete performance fixes for any site (multiclient).
metadata:
  version: "0.1.0"
---

# Diagnóstico y arreglos de PageSpeed

Auditar el rendimiento de un sitio con Lighthouse y análisis on-page de DataForSEO, y entregar los arreglos concretos — con código o instrucciones según la plataforma del sitio. Nunca inventar puntajes: todo score debe venir de una medición real.

## Parámetros

- **Dominio o URLs** (obligatorio): si solo dan el dominio, auditar como mínimo la home y 2–3 URLs representativas (una de servicio/producto, una de blog).
- **Plataforma del sitio** (detectar): usar `domain_analytics_technologies_domain_technologies` para identificar CMS/stack (WordPress, Shopify, Webflow, Next.js, etc.) y adaptar los arreglos a esa plataforma.

## Flujo

### 1. Medición

- `on_page_lighthouse` por cada URL, en **mobile primero** (es lo que pesa para Google) y luego desktop: Performance, LCP, CLS, TBT/INP, Accessibility, SEO.
- `on_page_instant_pages` por URL: peso de página, recursos, tiempos de servidor, cadena de redirects, compresión, caché.

### 2. Diagnóstico priorizado

Ordenar los hallazgos por **impacto en Core Web Vitals**, no por orden de aparición:

1. Problemas de LCP (imagen hero sin optimizar, render-blocking, servidor lento).
2. Problemas de INP/TBT (JavaScript pesado, third-parties).
3. Problemas de CLS (imágenes sin dimensiones, fuentes, banners inyectados).
4. Resto (caché, compresión, imágenes en general).

### 3. Arreglos concretos

Para cada hallazgo entregar el arreglo listo para aplicar, adaptado a la plataforma detectada:

- **Código directo** cuando aplique: atributos `width/height` y `loading="lazy"`, `<link rel="preload">` para la imagen LCP y fuentes, `font-display: swap`, defer/async de scripts, formato de imagen (AVIF/WebP) con el comando de conversión.
- **WordPress**: plugin o ajuste específico (caché, optimización de imágenes, hosting) y dónde configurarlo.
- **Shopify**: qué tocar en el theme (secciones, apps que inyectan JS) y qué apps eliminar o diferir.
- **Netlify/estáticos**: headers de caché, redirects, optimización en build.
- Third-party scripts: cuáles pesan más (con los ms medidos) y cuáles diferir, condicionar o eliminar.

### 4. Entregable

Informe en español con: scores medidos por URL (mobile y desktop, con fecha de medición), tabla de arreglos priorizada (hallazgo → impacto estimado → arreglo → dónde aplicarlo), y una sección "quick wins de esta semana".

Guardar en el proyecto (`investigacion/pagespeed-<dominio>-<fecha>.md`) y enviar con SendUserFile.

### 5. Verificación (si el usuario aplica arreglos)

Cuando el usuario avise que aplicó los cambios, volver a correr `on_page_lighthouse` sobre las mismas URLs y comparar contra la medición anterior guardada en el proyecto.

## Reglas

- Mobile es la medición de referencia; reportar desktop como secundario.
- No estimar puntajes futuros con precisión falsa ("subirá a 95"); describir el impacto cualitativamente o con rangos.
- Si una URL no se puede medir (bloqueo, timeout), reportarlo en lugar de omitirla.
