# ADR-004 — Sincronización del contrato API con la implementación (v0.3)

- **Estado:** Aceptada
- **Fecha:** 2026-11-12
- **Decide:** Persona A (backend) + revisión de Persona B

## Contexto

Tras la reescritura v0.3 (`backend/api/main.py`), el contrato
`docs/openapi.yaml` quedó desfasado respecto al código real:

- Rutas declaradas que **no existen**: `POST /api/productos`,
  `GET /api/analisis/rotacion` (el real es `GET /api/rotacion`),
  `GET /api/recomendaciones/compra` (el real es `GET /api/recomendaciones`).
- Rutas **reales sin declarar**: `GET /api/abc`, `GET /api/capital`,
  `GET /api/estacionalidad`, `GET /`.
- `POST /api/importar` declara el campo `archivo`; el backend espera `file`.
- `POST /api/chat` declara `pregunta`/`conectado`; el backend espera
  `mensaje` (+ `historial` opcional) y devuelve `respuesta`, `herramientas`,
  `contexto`, `modelo`.
- `GET /api/alertas` está declarado como array; el backend devuelve un objeto
  `{ riesgo_agotamiento, comportamiento }`.
- Funciones del motor estadístico sin endpoint (`clasificar_abc`,
  `capital_inmovilizado_por_producto`, `predecir_demanda_*`,
  `detectar_estacionalidad`), inalcanzables para RF-14, RF-15, RF-17, RF-18.

## Decisión

1. **El código manda sobre el contrato desfasado**: se reescribe
   `docs/openapi.yaml` (v0.3.0) para describir exactamente las 13 rutas
   implementadas y los payloads reales de `backend/api/main.py`.
2. **Se exponen los endpoints pendientes** en `main.py`:
   - `GET /api/predicciones/{producto_id}` → media móvil simple, 7/14/30
     días (RF-18 a RF-20). 404 si el producto no existe; 400 si el histórico
     de ventas es insuficiente (< 3 días con ventas).
   - `GET /api/abc` → clasificación ABC por valor de inventario (RF-15).
   - `GET /api/capital` → capital inmovilizado por producto (RF-17).
   - `GET /api/estacionalidad?producto=` → patrón estacional (RF-14).
3. **Se eliminan del contrato** `POST /api/productos` (no implementado; la
   creación de productos ocurre solo vía importación, ADR-003) y las rutas
   inexistentes de la v0.2.
4. **Correcciones de fórmulas** en `estadisticas.py` asociadas a este
   sincronismo (el contrato ya no puede prometer salidas imposibles):
   - Rotación: la ventana de 30 días estaba calculándose con el total
     histórico; ahora se acota a los últimos 30 días anclados en la última
     venta.
   - Días de cobertura: la fórmula multiplicaba ×30 en vez de dividir por
     la venta diaria (inflaba la cobertura un orden de magnitud); ahora
     `dias_cobertura = stock / venta_diaria_media` (null si no hay ventas).
   - Las queries de predicción usaban comentarios `#` (no soportados por
     SQLite) y comparaban `UPPER(nombre)` con el nombre sin normalizar;
     ambas hacían fallar el endpoint con 500/400.
5. Toda futura modificación de rutas o payloads exige un ADR nuevo
   (ADR-001) y actualización del mismo día del contrato.

## Consecuencias

- ✅ Persona B puede consumir el contrato tal cual: todas las rutas
  declaradas responden 200 con datos reales.
- ✅ RF-14, RF-15, RF-17 y RF-18/20 quedan expuestos por API (antes solo
  existían como funciones internas).
- ✅ `GET /api/rotacion` y `/api/recomendaciones` dejan de ser "pendientes":
  ya no existe el estado `RespuestaAnalisis` en el contrato.
- ⚠️ Cambios de semántica en `indice_rotacion` (ahora unidades/mes en
  ventana de 30 días) y `dias_cobertura`: si el frontend mostraba valores
  antiguos, los números cambiarán (a valores correctos).
- ⚠️ `POST /api/productos` sigue pendiente de decidir (¿manual o solo
  importación?); queda fuera del contrato hasta que haya requerimiento.
