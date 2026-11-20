# 📚 inventario-ia-docs

Repositorio de gestión documental del proyecto **Inventario-IA** — software de análisis
de ventas e inventario con IA local para tiendas, ferreterías y PyMEs.

> El **código** vive en el repositorio `inventario-ia`.
> Aquí vive la fuente de verdad del **qué** y del **porqué**.

## Contenido

| Ruta | Descripción |
|---|---|
| `REQUISITOS.md` | Los 50 requisitos del sistema con checklist de avance |
| `plan/ROADMAP.md` | Plan de 4 semanas, definición de "listo" y guion de demo |
| `plan/ROLES_TRABAJO.md` | Reparto de trabajo Persona A / Persona B y rutina del equipo |
| `decisiones/` | ADRs: decisiones de arquitectura con su contexto y consecuencias |
| `plantillas/ACTA_REUNION.md` | Plantilla para documentar reuniones |

## Reglas del proyecto

1. **Todo cambio de alcance** (nuevo requisito, requisito descartado) se registra aquí **antes** de codificar.
2. **Todo cambio del contrato de API** (`inventario-ia/docs/openapi.yaml`) requiere un ADR y avisar al compañero el mismo día.
3. **Toda reunión** se documenta con la plantilla: 5 minutos al final de la llamada.
4. Las decisiones se toman por consenso; si hay bloqueo, decide quien tenga más contexto del tema y se anota en el acta.

---

## Estado actual (2026-11-20)

| Dato | Valor |
|---|---|
| Código | `inventario-ia`, rama `mi-mvp` (6 commits de trabajo propio publicados; `main` intacta) |
| Contrato | `docs/openapi.yaml` del repositorio de código: **24 rutas** declaradas (22 de API) |
| Pruebas | **126/126** en verde |
| Requisitos cerrados en la última serie | RF-05, RF-10, RF-16, RF-27, RF-30, RF-42, RF-43 |

### Lo que hay funcionando ahora

- **Panel de control** (`/`): KPIs, ranking de productos y catálogo con **selector de proveedor por fila**.
- **Tendencias** (`/tendencias`): serie por día/semana/mes y comparación de periodos (semana, mes y mismo mes del año anterior).
- **Qué comprar** (`/compras`): recomendaciones con punto de reorden, urgencia y fecha; orden de compra en JSON, CSV y HTML imprimible.
- **Proveedores** (`/proveedores`): alta, edición, **baja lógica reversible** y lead time (1–365 días) que alimenta las recomendaciones y agrupa la orden de compra.
- **Alertas** (`/alertas`), **Importar** (`/importar`) y **Chat IA** (`/chat`, local con Ollama).

El detalle de cada paso, con su evidencia, está en `REQUISITOS.md` →
«Registro de avance — Cierre de RF en la rama `mi-mvp`». Las decisiones de contrato
de esa serie están en `decisiones/ADR-006-ampliacion-contrato-api.md`.

> **Nota de vigencia (2026-11-20).** El cuadro de contenido dice «Los 50
> requisitos»; el documento de requisitos recoge hoy **57** (49 funcionales + 8 no
> funcionales) tras la sección «Señales del Eje» (RF-51–55, RNF-56/57). La cifra 50
> corresponde a la redacción inicial y se conserva sin tocar.

