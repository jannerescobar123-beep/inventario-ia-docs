# ADR-006 — Ampliación del contrato API: umbrales, órdenes de compra, tendencias y proveedores

- **Estado:** Aceptada
- **Fecha:** 2026-11-20
- **Decide:** trabajo propio en la rama `mi-mvp` del repositorio de código (`inventario-ia`)
- **Revisión de Persona B:** pendiente de aviso (ver «Aviso al compañero»)
- **Nota de numeración:** **ADR-005 queda reservado** para el canal de señales y la
  política de anonimización de «Señales del Eje» (RF-51–55, RNF-56/57). Este ADR
  usa el 006 para no ocupar ese número.

## Contexto

La regla 2 del proyecto («todo cambio del contrato de API requiere un ADR y avisar al
compañero el mismo día») y el punto 5 de ADR-004 («toda futura modificación de rutas
o payloads exige un ADR nuevo») obligan a registrar cada ampliación del contrato.

Entre el 2026-11-16 y el 2026-11-20 se cerró una serie de requisitos en la rama
`mi-mvp`, y cada paso amplió `docs/openapi.yaml` **sin** que ese cambio quedara
recogido en un ADR. Este ADR registra, por una vez y con retraso, las decisiones de
esa serie. El contrato resultante tiene **24 rutas** (22 de API).

Commits de la serie, en orden:

| Commit | Fecha | Requisitos |
|---|---|---|
| `224b252` | 2026-11-16 | RF-05 (importación incremental), RF-07 (alta y edición manual de producto) |
| `5a6dc17` | 2026-11-16 | Medición de RNF-47 y corrección del lector de Excel |
| `a009c38` | 2026-11-17 | RF-30 (umbrales), RF-42 (stock máximo) |
| `f350970` | 2026-11-17 | RF-27 (órdenes de compra) |
| `aab5718` | 2026-11-19 | RF-10 (tendencias), RF-16 (comparación de periodos) |
| `3cfd16f` | 2026-11-20 | RF-43 (proveedores con lead time) |

## Decisión

### 1. Rutas añadidas

| Requisito | Rutas |
|---|---|
| RF-05 / RF-07 | `POST /api/productos`, `PUT /api/productos/{producto_id}` |
| RF-27 | `GET /api/ordenes-compra`, `GET /api/ordenes-compra.csv`, `GET /api/ordenes-compra.html` |
| RF-30 | `GET /api/umbrales`, `PUT /api/umbrales/{categoria}` |
| RF-10 / RF-16 | `GET /api/tendencias`, `GET /api/comparacion` |
| RF-43 | `GET /api/proveedores`, `POST /api/proveedores`, `PUT /api/proveedores/{proveedor_id}`, `DELETE /api/proveedores/{proveedor_id}`, `PUT /api/productos/{producto_id}/proveedor` |

### 2. Semánticas que quedan fijadas

1. **El lead time pertenece al proveedor, no al producto** (`productos.proveedor_id`
   apunta a `proveedores.lead_time_dias`). Corregir el plazo de un proveedor no
   obliga a tocar el catálogo.
2. **Sin proveedor —o con el proveedor dado de baja— se aplica el global de 7 días.**
   Dar de baja un proveedor no puede romper el cálculo de las recomendaciones.
3. **La baja de proveedor es lógica** (`activo = 0`) y reversible con `PUT activo:true`.
   No se borra la fila ni se limpia la asignación de sus productos.
4. **`PUT /api/proveedores/{id}` es un PUT estricto**: `contacto` y `telefono`
   omitidos se guardan como `null`. `activo` es la excepción: solo cambia si llega en
   el cuerpo, para que un formulario que no lo envía no reactive sin querer.
5. **En `PUT /api/productos` los campos de umbral y stock solo se sobrescriben si
   llegan en el cuerpo** (`stock_maximo`, `dias_alerta_inminente`, `margen_alerta`,
   `proveedor_id`). Un `null` explícito sí los borra. Criterio: conservar la
   configuración que el cliente no menciona.
6. **Los umbrales se resuelven en cascada producto → categoría → fábrica**, con
   `NULL` = heredar. La comparación de categorías ignora tildes y mayúsculas con
   `normalizar_texto`, no con `LOWER()` de SQLite (que solo afecta al ASCII).
7. **La orden de compra se agrupa por proveedor** con subtotales (`grupos`), y
   `lineas` sigue **plano y numerado** para no romper a quien ya la consumía.
8. **Las semanas son ISO** (`date.isocalendar()`), no `strftime('%W')` de SQLite,
   que inventa una «semana 00» y descoloca el año en los bordes de enero.
9. **Las ventanas de tendencia se miden en periodos**, no en días: 90 días / 26
   semanas / 12 meses completos, con los periodos intermedios vacíos conservados a
   cero y los extremos vacíos recortados.
10. **Una comparación sin base devuelve `disponible:false` y `motivo`**, nunca un 0 %
    que parecería una caída real. El periodo en curso se marca `completo:false`
    **sin ajustar el dato**.
11. **El CSV de la orden se emite con BOM UTF-8 y separador `;`** para que Excel en
    español respete las columnas y las tildes. **El PDF lo genera el navegador**
    desde el HTML imprimible: ninguna dependencia nueva y RNF-45 intacto.
12. **La ruta de la base de datos sale de la variable `INVENTARIO_DB`** (mismo valor
    por defecto que antes). `backend/tests/conftest.py` la fija a un temporal antes
    de importar el módulo de base de datos, porque las migraciones se ejecutan al
    importar y las pruebas no pueden tocar la base del usuario.
13. **El contrato se valida en las dos direcciones** (contrato → app y app →
    contrato). Antes solo se validaba una y las rutas nuevas pasaban sin avisar.

### 3. Migración de esquema

- Tabla nueva `proveedores` (`nombre`, `nombre_norm` UNIQUE, `lead_time_dias` con
  CHECK 1–365, `contacto`, `telefono`, `activo`, `fecha_creacion`).
- Columna nueva `productos.proveedor_id` (REFERENCES `proveedores(id)`), con índice.
- Ambas se crean automáticamente al arrancar el backend sobre una base existente
  (`init_db()`), sin perder datos.

## Consecuencias

- ✅ Los siete requisitos cerrados quedan exigibles desde la API y con contrato.
- ✅ La orden de compra pasa a ser utilizable de verdad: se puede enviar a cada
  proveedor por separado y se puede imprimir.
- ✅ El motor de compra deja de usar un plazo fijo y usa el plazo real del proveedor.
- ⚠️ **Cambio de semántica**: el campo `lead_time_dias` de cada recomendación pasó a
  ser **el lead time aplicado a ese producto** (el de su proveedor) y ya no siempre el
  global. Un cliente que comparase ese campo contra el parámetro global verá
  diferencias en productos con proveedor.
- ⚠️ **El CSV de la orden tiene una columna nueva** (`Proveedor`, en tercera
  posición). Un lector que Position por número de columna se desplaza.
- ⚠️ `GET /api/productos` y las recomendaciones devuelven **campos nuevos**
  (`proveedor_id`, `proveedor`, `lead_time_proveedor`): es aditivo y no rompe a un
  cliente que ignore los campos desconocidos.
- ⚠️ La base de datos del usuario **migra en el primer arranque** del backend nuevo
  (se crean la tabla y la columna; no se borra ni se modifica ninguna fila existente).
- ⚠️ RF-30 y RF-42 están resueltos **en la API pero sin pantalla**: el catálogo solo
  permite asignar proveedor; no existe pantalla de edición de producto.
- ✅ La base de datos del usuario **no se ha modificado durante el desarrollo**: toda
  prueba y toda revisión se hicieron sobre copias (`sha256` de la base real sin
  cambios).

## Aviso al compañero

**Pendiente.** La regla 2 pide avisar el mismo día de cada cambio del contrato; esta
serie se resolvió en una rama propia (`mi-mvp`) y el aviso no llegó a hacerse.
Queda como tarea pendiente de quien coordina el equipo.

## Verificación

- Suite completa: **126/126 en verde** (120 funciones de test).
- `npm run build`: correcto (676 módulos).
- HTTP real sobre **copia** de la base: 25 comprobaciones, **0 fallos** (CRUD de
  proveedores, duplicado 409, asignación, lead time 21 aplicado, productos sin
  proveedor a 7 días, dos grupos con subtotales en la orden, CSV/HTML con proveedor,
  baja y vuelta a 7 días, 409 al asignar un proveedor dado de baja, reactivación y
  404).
- Revisión visual de las tres pantallas nuevas y modificadas: 3 ciclos con
  `ui-ux-design-director`, veredicto final **APTO**.
- Contrato: cada paso lo amplió **solo por adición** (0 líneas eliminadas en el YAML).
