# SEO 4 Everyone

Suite SEO para [Claude Code](https://code.claude.com) y Cowork: cuatro agentes especializados — contenido, backlinks, PageSpeed y Reddit — impulsados por datos reales de [DataForSEO](https://dataforseo.com), más un orquestador que los ejecuta en paralelo y consolida un informe ejecutivo.

Multicliente: todas las skills aceptan cualquier dominio, así que sirve tanto para tu propio sitio como para clientes y prospectos. Mercado por defecto: Colombia / español (configurable).

**Regla central del plugin: ninguna métrica se inventa.** Todo dato cita su fuente (DataForSEO, Lighthouse, Search Console) y lo que no se pudo medir se declara explícitamente.

## Instalación

En Claude Code:

```
/plugin marketplace add saraga-marketing/seo-4-everyone
/plugin install seo-4-everyone@saraga-seo
```

Reemplaza `saraga-marketing/seo-4-everyone` por `usuario/repositorio` si has hecho un fork.

## Skills

| Skill | Qué hace | Ejemplo de uso |
|---|---|---|
| `seo-radar` | Orquestador: lanza los 4 agentes en paralelo y entrega un informe ejecutivo con quick wins de 30 días y plan de 90 | "Radar SEO completo de ejemplo.com" |
| `seo-contenido` | Investigación de keywords + análisis de SERP real → artículo completo optimizado en borrador | "Escribe un artículo SEO sobre X para ejemplo.com" |
| `seo-backlinks` | Auditoría del perfil de enlaces, brecha contra competidores y plan de link building priorizado | "Analiza los backlinks de ejemplo.com" |
| `seo-pagespeed` | Lighthouse + on-page → diagnóstico priorizado por Core Web Vitals con arreglos concretos según el CMS | "Arregla el PageSpeed de ejemplo.com" |
| `seo-reddit` | Preguntas activas del nicho en Reddit → borradores de respuesta para publicación manual + propuestas de contenido web | "¿Qué preguntan en Reddit sobre X?" |

## Agentes

`seo-agente-contenido`, `seo-agente-backlinks`, `seo-agente-pagespeed` y `seo-agente-reddit`. El orquestador `seo-radar` los lanza en paralelo; también pueden invocarse por separado.

## Requisitos

| Conector | Estado | Para qué |
|---|---|---|
| **DataForSEO** (MCP) | Obligatorio | Keywords, backlinks, SERPs, Lighthouse, detección de tecnología. Sin él el plugin no opera. |
| **Apify** | Opcional | Vía preferida para rastrear Reddit en vivo. Si no está disponible, las skills usan búsqueda web como respaldo automáticamente. |
| **Google Search Console** | Opcional | Mejora la priorización de contenido con datos de clics e impresiones reales. Sin GSC, el plugin usa las keywords posicionadas de DataForSEO como proxy y lo declara en el informe. |

## Filosofía de diseño

- **Degradación explícita**: si falta una fuente de datos, el plugin sigue funcionando con lo que tiene y declara qué le faltó y por qué. Nunca rellena huecos con estimaciones disfrazadas de datos.
- **Nada se publica automáticamente**: los artículos se entregan como borradores para revisión, y las respuestas de Reddit son borradores para publicación manual. La autopromoción automatizada en comunidades quema cuentas y reputación.
- **Priorización por impacto**: los hallazgos se ordenan por efecto real (Core Web Vitals, oportunidad de tráfico), no por orden de aparición en la herramienta.

## Licencia

MIT — ver [LICENSE](LICENSE).
