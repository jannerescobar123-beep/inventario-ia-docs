# ADR-003 — Importación de dos hojas: "ventas" + "productos"

- **Estado:** Aceptada
- **Fecha:** 2026-11-02
- **Decide:** Equipo completo (A + B)

## Contexto

El cliente necesita ver su catálogo con **stock real, costos y mínimos** en la página
Productos. Hasta ahora la importación solo leía la hoja de ventas, que crea productos
"pelados" (sin costo, sin stock) — solo los que tuvieron ventas.

El archivo de referencia (`datos/ventas_demo.xlsx`) ya trae dos hojas:
`ventas` y `productos` (sku, producto, categoria, costo, precio_venta,
stock_inicial, stock_minimo).

## Decisión

1. **Convención de archivo:** el Excel puede traer una hoja opcional llamada
   `productos`. Si existe, se importa **antes** que las ventas (para que las ventas
   se emparejen con el catálogo).
2. **Emparejamiento:** productos se buscan primero por **SKU**, luego por **nombre**.
   Si no existen, se crean.
3. **Semántica del stock:** el stock de la hoja es el **conteo físico actual**; NO se
   descuentan las ventas históricas importadas (ya ocurrieron). La actualización de
   stock por ventas nuevas (RF-08) queda para la fase post-prototipo.
4. **Contrato extendido** (`POST /api/importar` → `ResumenImportacion`): nuevos campos
   `productos_creados` y `productos_actualizados`. CSV sigue siendo solo ventas.

## Consecuencias

- ✅ La página Productos muestra margen, stock y badges de estado con datos reales.
- ✅ Aparecen en catálogo los productos sin ventas (baja rotación/muertos), clave para RF-12.
- ⚠️ Un Excel sin hoja "productos" sigue funcionando igual que antes (retrocompatible).
- ⚠️ Re-importar el mismo archivo **actualiza** el stock al valor de la hoja
  (sobrescribe cambios manuales) → documentar al usuario; deduplicación (RF-05)
  sigue pendiente para ventas.
