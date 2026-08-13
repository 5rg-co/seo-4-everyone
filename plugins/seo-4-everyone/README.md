# SEO 4 Everyone

Suite SEO multicliente para Cowork: cuatro agentes especializados (contenido, backlinks, PageSpeed y Reddit) impulsados por datos reales de DataForSEO, más un orquestador que los corre en paralelo y consolida un informe ejecutivo.

Mercado por defecto: **Colombia / español**. Todas las skills aceptan cualquier dominio, por lo que sirve tanto para el sitio propio como para clientes y prospectos. Regla central: **ninguna métrica se inventa** — todo dato cita su fuente (DataForSEO, Lighthouse, Search Console) y lo que no se pudo medir se declara.

## Componentes

### Skills

| Skill | Qué hace | Cómo invocarla |
|---|---|---|
| `seo-radar` | Orquestador: lanza los 4 agentes en paralelo sobre un dominio y entrega informe ejecutivo con quick wins de 30 días y plan de 90 | "Radar SEO completo de ejemplo.com" |
| `seo-contenido` | Investigación de keywords + SERP real → artículo completo optimizado en borrador, listo para publicar | "Escribe un artículo SEO sobre X para ejemplo.com" |
| `seo-backlinks` | Auditoría del perfil de enlaces, brecha contra competidores y plan de link building priorizado | "Analiza los backlinks de ejemplo.com" |
| `seo-pagespeed` | Lighthouse + on-page → diagnóstico priorizado por Core Web Vitals con arreglos concretos según el CMS | "Arregla el PageSpeed de ejemplo.com" |
| `seo-reddit` | Preguntas activas del nicho en Reddit → borradores de respuesta para publicación manual + propuestas de contenido web | "¿Qué preguntan en Reddit sobre X?" |

### Agentes

`seo-agente-contenido`, `seo-agente-backlinks`, `seo-agente-pagespeed`, `seo-agente-reddit` — usados por `seo-radar` para el análisis en paralelo; también invocables por separado.

## Conectores requeridos

| Conector | Estado | Para qué |
|---|---|---|
| **DataForSEO** (MCP) | Obligatorio | Keywords, backlinks, SERPs, Lighthouse, tecnología del sitio. Sin él, el plugin no opera. |
| **Apify** (vía desktop) | Opcional | Vía preferida para rastrear Reddit en vivo. Si no está, las skills usan búsqueda web como respaldo automáticamente. |
| **Search Console** | Opcional | No existe conector dedicado en el registro de Claude hoy. Vías: (a) Supermetrics con la fuente "Google Search Console" (ds_id `GW`) autenticada — requiere suscripción activa; (b) cualquier MCP de GSC que se conecte a futuro: las skills lo detectan y lo usan. Sin GSC, usan `ranked_keywords` de DataForSEO como proxy y lo declaran. |

## Flujo de trabajo típico

1. `seo-radar` sobre un dominio nuevo → informe integral guardado en el proyecto (`investigacion/`).
2. Profundizar con la skill del área que muestre más oportunidad.
3. Encadenar: `seo-reddit` detecta una pregunta con volumen → `seo-contenido` redacta el artículo que la responde.

## Convenciones

- Informes en español, guardados en la carpeta `investigacion/` del proyecto con el patrón `<area>-<dominio>-<fecha>.md`.
- Antes de investigar, las skills revisan si ya existe un informe reciente (<30 días) del mismo dominio en el proyecto y construyen sobre él.
- Reddit: nunca se publica automáticamente; todas las respuestas son borradores para publicación manual.
- El contenido nunca se publica directo a un CMS: siempre se entrega como borrador para revisión.
