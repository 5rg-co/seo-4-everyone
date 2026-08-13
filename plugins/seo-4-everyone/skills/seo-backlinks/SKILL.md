---
name: seo-backlinks
description: >
  This skill should be used when the user asks to "analizar backlinks", "auditar el perfil de
  enlaces", "plan de link building", "qué enlaces tiene [dominio]", "enlaces nuevos y perdidos",
  "spam score", or wants link-building opportunities versus competitors. Works for any domain
  (multiclient).
metadata:
  version: "0.1.0"
---

# Auditoría de backlinks y plan de link building

Auditar el perfil de enlaces de un dominio con datos reales de DataForSEO (herramientas `backlinks_*`) y entregar un plan de link building accionable. Nunca inventar métricas: cada cifra debe citar su fuente.

## Parámetros

- **Dominio objetivo** (obligatorio).
- **Competidores** (opcional): si no se indican, detectarlos con `backlinks_competitors` y `dataforseo_labs_google_competitors_domain`, y confirmarlos con el usuario si hay ambigüedad.

## Flujo

### 1. Fotografía del perfil actual

- `backlinks_summary`: total de backlinks, dominios de referencia, rank del dominio.
- `backlinks_referring_domains`: calidad y distribución de los dominios que enlazan (rank, país, TLD).
- `backlinks_anchors`: distribución de anchor text — detectar sobre-optimización (exceso de anchor exacto) o desaprovechamiento (todo "clic aquí"/marca).
- `backlinks_bulk_spam_score`: enlaces tóxicos o sospechosos.

### 2. Dinámica reciente

- `backlinks_timeseries_new_lost_summary`: tendencia de enlaces nuevos vs. perdidos (últimos 6–12 meses).
- `backlinks_bulk_new_lost_backlinks` / `backlinks_bulk_new_lost_referring_domains`: identificar enlaces valiosos perdidos recientemente — candidatos a recuperación (contactar al sitio).

### 3. Brecha contra competidores

- `backlinks_domain_intersection`: dominios que enlazan a 2+ competidores pero no al objetivo — la lista de prospección más valiosa.
- `backlinks_page_intersection` sobre las URLs que compiten por las keywords principales.
- `dataforseo_labs_google_domain_rank_overview` de cada competidor para dimensionar la brecha de autoridad.

### 4. Entregable

Informe en español con:

- Diagnóstico del perfil (fortalezas, riesgos, comparativa con competidores) — cada métrica con fuente.
- **Plan de link building priorizado**: (a) enlaces perdidos a recuperar (URL, contacto sugerido), (b) dominios de la brecha competitiva ordenados por autoridad y relevancia temática, (c) anchors recomendados para equilibrar el perfil, (d) enlaces tóxicos a desautorizar si el spam score lo justifica.
- Para cada objetivo de prospección: qué tipo de contenido o pitch aumentaría la probabilidad de conseguir el enlace.

Guardar el informe en el proyecto (carpeta `investigacion/`, nombre `backlinks-<dominio>-<fecha>.md`) y enviar el archivo con SendUserFile.

## Reglas

- Si el dominio tiene muy pocos backlinks (sitio nuevo), orientar el plan a estrategias de arranque: directorios de calidad del país, prensa local, guest posts en el nicho, y contenido enlazable (datos originales, herramientas).
- No prometer resultados de posiciones: el plan describe acciones y objetivos, no garantías.
