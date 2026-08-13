---
name: seo-radar
description: >
  This skill should be used when the user asks for a "radar SEO completo", "análisis SEO integral
  de [dominio]", "auditoría completa de SEO", "corre todos los agentes SEO", or wants the full
  picture (contenido + backlinks + PageSpeed + Reddit) of any domain in one report.
metadata:
  version: "0.1.0"
---

# Radar SEO integral (orquestador)

Ejecutar las cuatro áreas del plugin en paralelo sobre un dominio y consolidar un informe ejecutivo único. Este es el punto de entrada cuando el usuario quiere "todo".

## Parámetros

- **Dominio objetivo** (obligatorio).
- **Mercado**: por defecto Colombia / español.
- **Competidores** (opcional): si no se indican, detectarlos con DataForSEO y listarlos en el informe.
- **Nicho para Reddit**: inferirlo del dominio si no se indica.

## Flujo

### 1. Contexto previo

- Revisar el proyecto (`project_search`) por informes previos del mismo dominio y construir sobre ellos en lugar de repetir investigación.
- `dataforseo_labs_google_domain_rank_overview` para una línea base rápida del dominio.

### 2. Lanzar los cuatro agentes en paralelo

Usar el tool Agent para lanzar los cuatro subagentes del plugin **en un solo mensaje** (concurrentes):

- `seo-agente-contenido`: oportunidades de keywords y plan de contenido (no redacta artículos completos en modo radar; solo detecta y prioriza).
- `seo-agente-backlinks`: auditoría de enlaces y brecha competitiva.
- `seo-agente-pagespeed`: medición Lighthouse de URLs clave y arreglos priorizados.
- `seo-agente-reddit`: preguntas activas del nicho y oportunidades de respuesta.

Pasar a cada agente: dominio, mercado, competidores conocidos y cualquier hallazgo previo del proyecto que le sea relevante.

### 3. Consolidación

Con los resultados de los cuatro agentes, redactar el **informe ejecutivo** en español:

1. **Resumen ejecutivo** (media página): estado general y las 3 conclusiones más importantes.
2. **Quick wins — próximos 30 días**: las 5–10 acciones de mayor impacto/esfuerzo cruzando las cuatro áreas, en orden de ejecución.
3. Una sección por área con los hallazgos clave (cada métrica con su fuente).
4. **Plan 90 días**: contenido a producir, enlaces a conseguir, arreglos técnicos, presencia en Reddit.

### 4. Entrega

- Guardar el informe en el proyecto: `investigacion/radar-seo-<dominio>-<fecha>.md`.
- Enviar el archivo con SendUserFile.
- Ofrecer ejecutar en profundidad cualquiera de las áreas con su skill dedicada (ej. redactar ya el primer artículo con `seo-contenido`).

## Reglas

- Si un agente falla (ej. Apify no disponible para Reddit), incluir las otras áreas igual y reportar qué faltó y por qué.
- No duplicar investigación que ya exista en el proyecto con menos de 30 días; citarla y actualizar solo lo que cambió.
- Toda métrica con fuente; lo que no venga de una herramienta se marca como estimación cualitativa.
