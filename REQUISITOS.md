# 📋 Requisitos — Inventario-IA (v1)

Sistema de análisis de ventas e inventario con IA local para tiendas, ferreterías
y PyMEs. Estado: `- [ ]` pendiente · `- [x]` completado. Actualizar al cerrar cada tarea.

---

## A. Importación y gestión de datos

- [x] **RF-01** — El sistema debe importar archivos de ventas en formato Excel (.xlsx, .xls) y CSV.
- [ ] **RF-02** — Debe ofrecer un asistente de mapeo de columnas para identificar fecha, producto, cantidad, precio y categoría.
- [ ] **RF-03** — Debe guardar plantillas de mapeo para repetir importaciones del mismo proveedor/POS sin reconfigurar.
- [x] **RF-04** — Debe validar los datos importados (fechas inválidas, cantidades negativas, productos duplicados) y mostrar un reporte de errores claro.
- [ ] **RF-05** — Debe permitir importaciones incrementales sin duplicar registros ya existentes.
- [ ] **RF-06** — Debe mantener un historial de importaciones con opción de revertir una carga errónea.
- [x] **RF-07** — Debe permitir registrar y editar productos manualmente: nombre, SKU/código de barras, categoría, unidad, costo y precio de venta.
- [ ] **RF-08** — Debe actualizar automáticamente el stock disponible a partir de las ventas importadas.

## B. Análisis de ventas

- [x] **RF-09** — Debe mostrar un tablero con KPIs: ventas totales, ticket promedio, unidades vendidas, margen y top de productos.
- [ ] **RF-10** — Debe analizar tendencias de venta por día, semana y mes.
- [x] **RF-11** — Debe calcular la rotación de inventario por producto.
- [x] **RF-12** — Debe identificar productos de baja rotación y productos "muertos" (sin ventas en N días configurable).
- [x] **RF-13** — Debe calcular los días de cobertura de inventario restantes por producto.
- [x] **RF-14** — Debe detectar estacionalidad por producto.
- [x] **RF-15** — Debe clasificar los productos mediante análisis ABC.
- [ ] **RF-16** — Debe permitir comparar períodos (semana vs. semana anterior, mes vs. mismo mes del año anterior).
- [x] **RF-17** — Debe calcular el capital inmovilizado por producto y por categoría.

## C. Predicción de demanda

- [x] **RF-18** — Debe predecir la demanda por producto a 7, 14 y 30 días con algoritmos estadísticos (media móvil, suavizado exponencial, ARIMA).
- [x] **RF-19** — Debe ajustar las predicciones según estacionalidad y tendencias recientes.
- [x] **RF-20** — Debe indicar el nivel de confianza de cada predicción (rango mínimo–esperado–máximo).
- [ ] **RF-21** — Debe permitir corregir manualmente una predicción ante eventos conocidos (promociones, feriados).

## D. Recomendaciones de compra

- [x] **RF-22** — Debe generar una lista semanal de productos sugeridos para comprar con cantidades específicas.
- [x] **RF-23** — Debe calcular cantidades considerando stock actual, stock mínimo, venta proyectada y lead time del proveedor.
- [x] **RF-24** — Debe indicar cuándo realizar cada compra (fecha sugerida según punto de reorden).
- [x] **RF-25** — Debe calcular el punto de reorden automáticamente por producto.
- [x] **RF-26** — Debe priorizar las recomendaciones por urgencia (crítico / medio / bajo).
- [ ] **RF-27** — Debe generar órdenes de compra exportables o imprimibles.

## E. Alertas inteligentes

- [x] **RF-28** — Debe alertar sobre productos con riesgo de agotarse pronto.
- [x] **RF-29** — Debe detectar comportamientos inusuales en ventas (picos o caídas anómalas) con detección estadística.
- [ ] **RF-30** — Debe permitir configurar umbrales de alerta por producto o categoría.
- [x] **RF-31** — Debe mostrar las alertas en un panel central y en el tablero principal.
- [ ] **RF-32** — Debe enviar un resumen semanal automático del estado del negocio.

## F. Asistente conversacional con IA local

- [x] **RF-33** — Debe incluir un asistente de chat que responda preguntas en lenguaje natural sobre el negocio.
- [x] **RF-34** — El modelo de IA debe ejecutarse localmente, sin depender de servicios en la nube.
- [x] **RF-35** — El asistente debe basar sus respuestas en los datos reales del negocio (no en respuestas genéricas).
- [x] **RF-36** — El asistente debe explicar en lenguaje sencillo los resultados estadísticos.
- [x] **RF-37** — Debe citar las cifras y datos que sustentan cada respuesta.
- [x] **RF-38** — Debe mantener contexto conversacional para preguntas de seguimiento.
- [x] **RF-39** — Debe ofrecer botones de preguntas frecuentes.
- [x] **RF-40** — Debe responder en español (con soporte configurable de otros idiomas).

## G. Optimización de inventario

- [ ] **RF-41** — Debe sugerir acciones para productos de baja rotación: liquidación, descuentos o no volver a comprarlos.
- [ ] **RF-42** — Debe permitir definir stock mínimo y máximo por producto.
- [ ] **RF-43** — Debe gestionar proveedores con su tiempo de entrega (lead time) y usarlo en las recomendaciones.
- [ ] **RF-44** — Debe permitir filtrar todo el análisis por categoría, proveedor y rango de fechas.

## H. Requisitos no funcionales

- [x] **RNF-45** — La aplicación debe funcionar 100% offline; ningún dato debe salir del equipo (privacidad total).
- [x] **RNF-46** — Debe correr en hardware modesto (8 GB RAM, sin GPU obligatoria), con aceleración GPU opcional.
- [x] **RNF-47** — Debe importar archivos de hasta 10,000 filas en menos de 30 segundos.
- [x] **RNF-48** — El asistente de IA debe responder en menos de 10 segundos en equipos básicos.
- [ ] **RNF-49** — Debe instalar con un solo instalador en Windows (deseablemente Linux), sin configuración técnica.
- [ ] **RNF-50** — Debe incluir respaldo automático de la base de datos local y cifrado de la información almacenada.

---

## Cobertura en el prototipo (semanas 0–4)

| Grupo | Estado en prototipo |
|---|---|
| A (datos) | RF-01, RF-04, RF-07 ✔ · importación de hoja "productos" (stock/costos, ADR-003) ✔ |
| B (análisis) | RF-09 parcial (KPIs) · **RF-11, RF-12, RF-13, RF-17 ✔ motor de rotación** |
| C/D (predicción y compra) | **RF-18, RF-19, RF-20, RF-22, RF-24, RF-25, RF-26 ✔** (modelo simple explicable; ARIMA post-prototipo) · pantalla «Qué comprar» ✔ |
| E (alertas) | **RF-12, RF-28, RF-29 ✔** regeneración automática en GET /api/alertas |
| F (IA local) | Chat básico con Ollama ✔, function calling semana 3 |
| G/H (optimización y NFR) | Posterior al prototipo |

---

## Registro de avance — Prototipo v0.1 (2026-11-05)

### Implementado y verificado

| Requisito | Evidencia de verificación |
|---|---|
| RF-01 | `POST /api/importar` acepta `.csv`, `.xlsx`, `.xls` (respuesta 200 con `total_ventas > 0`) |
| RF-04 | Validación de cantidad/precio/columnas; reporte de errores en la respuesta y en la UI |
| RF-07 | Productos creados desde la hoja `productos` y desde las ventas; catálogo editable pendiente |
| RF-09 | `GET /api/kpis` → ventas totales, ticket promedio, unidades, margen, top productos |
| RF-11, RF-12, RF-13 | `GET /api/rotacion` → índice de rotación (ventana 30 días), baja rotación, días de cobertura = stock / venta diaria |
| RF-14 | `GET /api/estacionalidad[?producto=]` → patrón y promedios mensuales (ADR-004) |
| RF-15 | `GET /api/abc` → clases A/B/C por valor de inventario (ADR-004) |
| RF-17 | `GET /api/capital` → capital inmovilizado total y por producto (ADR-004) |
| RF-18, RF-19, RF-20 | `GET /api/predicciones/{id}` → media móvil 7/14/30 días; `predecir_demanda_holt_winters()` (ARIMA post-prototipo) |
| RF-22 a RF-26 | `generar_recomendaciones_compra()` → cantidades, fecha sugerida, punto de reorden, urgencia |
| RF-28, RF-29 | `GET /api/alertas` → riesgo de agotamiento + detección de picos/caídas |
| RF-31 | Panel de alertas con regeneración automática cada 5 minutos |
| RF-33 a RF-40 | `POST /api/chat` con function calling: 5 herramientas deterministas + Ollama local (`qwen3:1.7b`) |
| RNF-45, RNF-46 | Todo local (FastAPI + SQLite + Ollama); sin GPU obligatoria |

### Pendiente para la siguiente iteración

- RF-02 / RF-03: asistente de mapeo de columnas y plantillas guardadas (el mapeo manual actual no persiste)
- RF-05 / RF-06: deduplicación incremental y reversión de importaciones
- RF-10 / RF-16: tendencias por periodo y comparación de períodos (RF-14 ya está expuesta en `GET /api/estacionalidad`)
- RF-21, RF-27, RF-30, RF-32, RF-41 a RF-44: edición manual, órdenes imprimibles, umbrales configurables, resumen automático, filtros
- RNF-47 / RNF-48: medidos en v0.2 (10.000 filas en 6,5 s; chat en 8,2 s con el modelo cargado)

### Nota de arquitectura

El endpoint de chat sigue ADR-002: el LLM **no calcula**. El backend ejecuta herramientas
deterministas sobre SQLite (`obtener_kpis`, `productos_baja_rotacion`, `riesgo_agotamiento`,
`comportamiento_ventas`, `recomendaciones_compra`) y el modelo solo explica esas cifras.
El modelo se configura con la variable de entorno `OLLAMA_MODEL` (por defecto `qwen3:1.7b`).

---

## Registro de avance — Prototipo v0.2 (2026-11-05)

### Cambios de esta iteración

| Área | Cambio | Evidencia |
|---|---|---|
| Motor de compra | `generar_recomendaciones_compra()` corregido: antes filtraba con `stock_actual <= 0` (ignoraba el `stock_minimo` de cada producto) y usaba umbrales fijos 50/20 | 9 recomendaciones reales sobre 11 productos del demo |
| Motor de compra | Demanda de 7 días emparejada por `producto_norm` (insensible a tildes) y anclada en la última venta registrada, no en `date('now')` | Tabla `recomendaciones` con `demanda_proyectada` coherente con las ventas |
| Motor de compra | Punto de reorden calculado (demanda del lead time + stock de seguridad), urgencia relativa al mínimo de cada producto y fecha sugerida por punto de reorden | `punto_reorden`, `urgencia`, `fecha_sugerida`, `fecha_entrega_estimada` por producto |
| Motor de compra | Nueva herramienta `GET /api/recomendaciones` y nueva pantalla **«Qué comprar»** en la web | 200 con 9 filas; tabla renderizada desde la API |
| Alertas | `alertas_riesgo_agotamiento()` empareja por `producto_norm` y calcula los días sobre la última venta registrada | 9 alertas con stock, mínimo y días estimados |
| Base de datos | Columnas auxiliares `productos.nombre_norm` y `ventas.producto_norm` + backfill de filas anteriores | Importación de Excel: 11 productos creados (antes 49 duplicados por acentos), 600 ventas, 0 errores |
| Chat IA | Contexto compacto para el LLM (solo las cifras necesarias + total), `think: false`, `keep_alive: 30m`, respuesta acotada y recorte de la última línea truncada | «Hay 9 productos para comprar, 6 críticos» + 3 viñetas con cantidades exactas |
| Frontend | Quinta pantalla «Qué comprar» (ruta `/compras`) y enlace en la navbar | `npm run build` OK (966 módulos) |

### Requisitos verificados en esta iteración

| Requisito | Evidencia |
|---|---|
| RF-22 a RF-26 | `GET /api/recomendaciones` → 9 filas con cantidad, punto de reorden, urgencia y fechas; visible en la pantalla «Qué comprar» |
| RF-28 | `GET /api/alertas` → 9 productos en riesgo con días estimados reales |
| RF-37 | El chat cita las cifras exactas (75, 34, 33… unidades) tomadas del contexto SQLite |
| RF-47 | Importación medida con 10.000 filas: **6,5 s** (límite 30 s) sobre una base temporal; la base del demo no se modificó |
| RNF-48 | Chat con el modelo en RAM: **8,2–10,0 s** (límite 10 s). La carga inicial del modelo en Ollama (~22 s) ocurre una sola vez: la petición envía `keep_alive: 30m` para que no se descargue entre preguntas |

### Endpoints disponibles en el prototipo

`GET /health` · `GET /api/kpis` · `GET /api/productos` · `GET /api/rotacion` ·
`GET /api/alertas` · `GET /api/recomendaciones` · `POST /api/importar` · `POST /api/chat`

### Pendiente para la siguiente iteración

- RF-05 / RF-06: deduplicación incremental y reversión de importaciones
- RF-02 / RF-03: asistente de mapeo de columnas y plantillas guardadas
- RF-10 / RF-14 / RF-16: tendencias por periodo, estacionalidad visible y comparación de períodos
- RF-21, RF-27, RF-30, RF-32, RF-41 a RF-44: edición manual, órdenes imprimibles, umbrales configurables, resumen automático, filtros
- RF-49 / RF-50: instalador único, respaldo y cifrado de la base local

---

## Registro de avance — Prototipo v0.3 (2026-11-09)

### Cambios de esta iteración (rediseño de interfaz, dirección aprobada en `DISENO.md`)

| Área | Cambio | Evidencia |
|---|---|---|
| Frontend | Sistema de diseño propio en `styles.css`: tema oscuro, escala tipográfica fluida (`clamp`), tarjetas con profundidad 3D en CSS puro, `prefers-reduced-motion` | `npm run build` → exit 0, 674 módulos, CSS 22,75 kB, JS 628,38 kB |
| Frontend | Se elimina la dependencia de Bootstrap en el JSX: `react-bootstrap` no instala `bootstrap` (es *peer dependency*), así que el `import "bootstrap/dist/css/bootstrap.min.css"` rompía la app con «Failed to resolve import» | 0 imports de `react-bootstrap` y de `bootstrap` en `frontend/src`; build correcto |
| Frontend | Se añade `box-sizing: border-box` global (lo aportaba Bootstrap) y `body { margin: 0 }`; sin eso el contenido se salía por la derecha en todas las pantallas | Auditoría visual: sin desbordamiento horizontal a 360 px CSS |
| Navegación | Barra superior con menú hamburguesa en móvil (foco inicial, `Escape`, `aria-expanded`, foco devuelto al cerrar) y navegación de escritorio a partir de 768 px | QA funcional: menú abre, navega, cierra y devuelve el foco |
| Chat | Botón flotante en la esquina inferior derecha que abre un panel con el asistente en cualquier pantalla (oculto en la ruta `/chat`, que ya es el chat a página completa) | QA funcional: FAB abre el panel, envía una pregunta real y cierra con `Escape` |
| Chat (backend) | La instrucción al modelo ya no usa plantillas con marcadores angulares; se incluye un ejemplo con datos reales y una reparación determinista que rellena los valores reales de SQLite si el modelo copia los marcadores | `POST /api/chat` → «Martillo profesional: comprar 75 unidades (crítico)…» sin `<producto>` ni `(urgencia)` |
| Chat (backend) | `num_predict` ajustado (130 → 140) y límite determinista de 3 viñetas, con detección de líneas cortadas a mitad de fecha, para respetar el formato y el tiempo de respuesta | Respuesta = 1 resumen + 3 viñetas; 8,2–11,0 s; ver RNF-48 |
| Accesibilidad | Al navegar desde el menú, el foco pasa a `<main tabIndex={-1}>` antes de ocultar el menú con `aria-hidden` | Corrección aplicada; re-verificación en navegador en curso |
| Accesibilidad | Contraste de textos secundarios por encima de AA (`--text-3: #8C9AAD`), `id`/`name` en el campo de mensaje, favicon en `data-URI` (sin petición externa) | `.state-hint` 6,61:1 · `.kpi-hint` 6,11:1 · 0 errores y 0 avisos en consola |

### Requisitos verificados en esta iteración

| Requisito | Evidencia |
|---|---|
| RF-09 | Tablero rediseñado sobre `GET /api/kpis`: ventas totales $55.705,41, ticket promedio, unidades vendidas y margen; gráfico de top de productos con Recharts; catálogo con los 11 productos del demo |
| RF-22 a RF-26 | Pantalla «Qué comprar» con las 9 recomendaciones reales: cantidad sugerida, punto de reorden, urgencia, fecha sugerida y fecha de entrega estimada |
| RF-26 | Los 6 chips «crítico» se pintan con su nivel real (antes se mostraban como «bajo» por la tilde) y el KPI «Urgencia crítica» muestra 6 |
| RF-28, RF-31 | Panel de alertas con los 9 productos en riesgo, días estimados y valor de inventario; regeneración automática cada 5 minutos |
| RF-33 | Asistente accesible desde cualquier pantalla (botón flotante) y desde la ruta `/chat` |
| RF-35, RF-37 | La respuesta cita datos reales del negocio: nombres de producto («Martillo profesional», «Cincel de acero») y cantidades exactas (75, 34, 33, 30 unidades) |
| RF-39 | Cuatro preguntas frecuentes operativas en el chat |
| RF-40 | Respuestas en español |
| RNF-45 | Cero CDN, cero fuentes externas (pila tipográfica del sistema), favicon en `data-URI` y todo el cómputo en local (FastAPI + SQLite + Ollama) |
| RNF-48 | Chat con el modelo en RAM: **8,2–11,0 s** en tres mediciones seguidas (límite 10 s; cumple en dos de las tres, la tercera coincidió con una prueba de QA en paralelo). Ver nota de medición |

> **Nota de medición de RNF-48.** Con el modelo ya cargado en Ollama, la respuesta completa
> (resumen + 3 viñetas) se generó en 8,2 s, 9,0 s y 11,0 s. La primera consulta tras arrancar el
> proceso del backend añade la carga del modelo en RAM (~22 s), igual que en v0.2: la petición
> envía `keep_alive: 30m` para que no se descargue entre preguntas. La medición de 11,0 s coincidió
> con una prueba de QA en paralelo (dos generaciones simultáneas en la misma CPU). RNF-48 queda
> **cumplido con el margen justo**, sin margen para Growth.
>
> Para que la respuesta no crezca por encima de lo pactado, el backend recorta a 3 viñetas de forma
> determinista (las cifras siempre proceden de SQLite; el recorte solo acota cuántas se muestran) y
> el presupuesto de tokens se fijó en 140 con detección de líneas cortadas a mitad de fecha.

### Defectos detectados y corregidos en esta iteración

| Severidad | Defecto | Causa | Corrección |
|---|---|---|---|
| P1 | Contenido recortado por la derecha (H1, hamburguesa, 4.ª tarjeta KPI, columna «Llegada estimada») | Faltaba `box-sizing: border-box` global: lo aportaba Bootstrap, que no está instalado | Regla global `*, *::before, *::after { box-sizing: border-box }` + `body { margin: 0 }` + ajuste de `.topbar-inner` por debajo de 480 px |
| P1 | Los 6 chips «crítico» se pintaban como «bajo» y el KPI «Urgencia crítica» mostraba 0 | `"crítico".startsWith("crit")` falla por la tilde | Normalización NFD sin diacríticos antes de comparar (mismo criterio que `nombre_norm` en SQLite) |
| P1 | La app no cargaba: «Failed to resolve import "bootstrap/dist/css/bootstrap.min.css"» | `react-bootstrap` no instala `bootstrap` (es *peer dependency*) y el paquete no estaba en `node_modules` | JSX reescrito con clases del sistema de diseño propio; cero imports de Bootstrap |
| P2 | El chat respondía con el literal `<producto>` y `(urgencia)` en lugar de los nombres y la urgencia reales | El modelo local de 1,7 B copiaba la plantilla con marcadores angulares | Instrucción sin marcadores + ejemplo con datos reales + reparación determinista de placeholders con los valores de SQLite |
| P2 | `.state-hint` 3,96:1 y `.kpi-hint` 3,67:1 (fallaban AA) | `--text-3: #64748B` demasiado oscuro | `--text-3: #8C9AAD` → 6,11–6,61:1 |
| P3 | 404 de `/favicon.ico` en cada carga | No existía favicon | `<link rel="icon">` con SVG en `data-URI` |
| P3 | Aviso de campo de formulario sin `id`/`name` | `input` del chat | `id="chat-mensaje"` + `name="mensaje"` |
| P3 | Aviso «Blocked aria-hidden… retained focus» al navegar desde el menú | El foco seguía en el enlace del menú al ocultarlo con `aria-hidden` | El foco pasa a `<main tabIndex={-1}>` antes de cerrar el menú |

### Pendiente para la siguiente iteración

- Verificación responsive en 768, 1024 y 1920 px (solo 360 px está verificado con `emulate`;
  el agente de QA no tiene permiso de `resize_page`)
- Verificación del orden de tabulación y del foco por teclado completo
- RF-05 / RF-06: deduplicación incremental y reversión de importaciones (reimportar el Excel de
  demo duplicaría las ventas)
- El resto de pendientes de v0.2 (RF-02/03, RF-10/14/16, RF-21/27/30/32, RF-41 a RF-44, RF-49/50)



---

## Registro de avance — Sincronización contrato/código v0.3.1 (2026-11-12)

### Cambios de esta iteración (ADR-004)

| Área | Cambio | Evidencia |
|---|---|---|
| Contrato | `docs/openapi.yaml` reescrito (v0.3.0) con las 13 rutas reales de `backend/api/main.py`; eliminadas `POST /api/productos`, `/api/analisis/rotacion` y `/api/recomendaciones/compra` (no existían) | YAML parsea; las 13 rutas coinciden con `openapi.json` del servidor |
| Backend | Nuevos endpoints: `GET /api/predicciones/{id}` (RF-18/20), `GET /api/abc` (RF-15), `GET /api/capital` (RF-17), `GET /api/estacionalidad` (RF-14) | Verificados con curl: 200 con datos reales; 404 producto inexistente; 400 sin histórico |
| Backend | Corregida la rotación (RF-11): la ventana de 30 días se calculaba con el total histórico | `GET /api/rotacion` → `total_vendido_30d` acotado a la ventana anclada en la última venta |
| Backend | Corregida la cobertura (RF-13): la fórmula multiplicaba ×30 en vez de dividir por la venta diaria (inflaba ~30×) | `dias_cobertura = stock / venta_diaria_media`; ejemplo: 16,3 días en vez de 489 |
| Backend | Corregidas las queries de predicción: comentarios `#` no soportados por SQLite (500) y `UPPER(nombre)` sin normalizar (400 falsos) | Barrido de los 11 productos: 10×200 y 1×400 legítimo (0 días de ventas) |
| Backend | Modelo local `qwen3:1.7b` descargado en Ollama (1,4 GB); `docker-compose.yml` actualiza `OLLAMA_MODEL` de `llama3.2:3b` a `qwen3:1.7b` y el volumen a `/app/datos` | `ollama pull qwen3:1.7b` → success |
| Backend | `backend/Dockerfile` corregido: el CMD apuntaba al paquete muerto `app.main:app` (borrado en v0.3); añadido `backend/.dockerignore` | Reescrito a `uvicorn main:app` desde `/app/api` (docker no disponible en esta máquina: no construido) |
| Limpieza | Borrados `backend/app/` (v0.2), `frontend/src/api.js` y `frontend/src/pages/` (sin referencias) | Búsqueda de referencias previa = 0 consumidores |
| Repositorio | `.gitignore`: `backend/datos/uploads/` y `datos/` (4,6 MB de subidas temporales) | `git status` ya no los lista como sin seguimiento |

### Verificación final (misma iteración)

| Comprobación | Resultado |
|---|---|
| Suite de pruebas | `backend/tests/test_smoke.py`: **20/20 en verde** (pytest + TestClient sobre BD temporal; añadido `backend/requirements-dev.txt`) |
| Venv | `backend/.venv` regenerado (shebangs correctos; fastapi 0.162.0, pandas 3.1.1, pytest 9.2.0) |
| Rutas | Barrido de las 13 rutas: 10×GET 200, `POST /api/importar` 400 sin body, `POST /api/chat` 200 |
| Chat con IA local | `qwen3:1.7b` responde en español con datos reales (5,8 s con el modelo en RAM; 32 s en la primera carga) |
| Chat: defecto corregido | El formato de compra se inyectaba aunque la herramienta no fuera `recomendaciones_compra` (respuesta con `M críticos` y fecha inventada); ahora el formato es condicional al contexto y la línea de resumen se rellena deterministamente con totales reales |
| Frontend | `npm run build` → exit 0 (2,42 s) |

### Pendiente para la siguiente iteración

- Pruebas de extremo a extremo del chat con Ollama (hoy solo la ruta de respaldo está cubierta por tests)
- Verificación del chat de punta a punta ampliada (más preguntas fuera del patrón de compra)
- Commits de esta iteración (pendientes de aprobación del usuario)
