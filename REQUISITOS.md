# 📋 Requisitos — Inventario-IA (v1)

Sistema de análisis de ventas e inventario con IA local para tiendas, ferreterías
y PyMEs. Estado: `- [ ]` pendiente · `- [x]` completado. Actualizar al cerrar cada tarea.

---

## A. Importación y gestión de datos

- [ ] **RF-01** — El sistema debe importar archivos de ventas en formato Excel (.xlsx, .xls) y CSV.
- [ ] **RF-02** — Debe ofrecer un asistente de mapeo de columnas para identificar fecha, producto, cantidad, precio y categoría.
- [ ] **RF-03** — Debe guardar plantillas de mapeo para repetir importaciones del mismo proveedor/POS sin reconfigurar.
- [ ] **RF-04** — Debe validar los datos importados (fechas inválidas, cantidades negativas, productos duplicados) y mostrar un reporte de errores claro.
- [ ] **RF-05** — Debe permitir importaciones incrementales sin duplicar registros ya existentes.
- [ ] **RF-06** — Debe mantener un historial de importaciones con opción de revertir una carga errónea.
- [ ] **RF-07** — Debe permitir registrar y editar productos manualmente: nombre, SKU/código de barras, categoría, unidad, costo y precio de venta.
- [ ] **RF-08** — Debe actualizar automáticamente el stock disponible a partir de las ventas importadas.

## B. Análisis de ventas

- [ ] **RF-09** — Debe mostrar un tablero con KPIs: ventas totales, ticket promedio, unidades vendidas, margen y top de productos.
- [ ] **RF-10** — Debe analizar tendencias de venta por día, semana y mes.
- [ ] **RF-11** — Debe calcular la rotación de inventario por producto.
- [ ] **RF-12** — Debe identificar productos de baja rotación y productos "muertos" (sin ventas en N días configurable).
- [ ] **RF-13** — Debe calcular los días de cobertura de inventario restantes por producto.
- [ ] **RF-14** — Debe detectar estacionalidad por producto.
- [ ] **RF-15** — Debe clasificar los productos mediante análisis ABC.
- [ ] **RF-16** — Debe permitir comparar períodos (semana vs. semana anterior, mes vs. mismo mes del año anterior).
- [ ] **RF-17** — Debe calcular el capital inmovilizado por producto y por categoría.

## C. Predicción de demanda

- [ ] **RF-18** — Debe predecir la demanda por producto a 7, 14 y 30 días con algoritmos estadísticos (media móvil, suavizado exponencial, ARIMA).
- [ ] **RF-19** — Debe ajustar las predicciones según estacionalidad y tendencias recientes.
- [ ] **RF-20** — Debe indicar el nivel de confianza de cada predicción (rango mínimo–esperado–máximo).
- [ ] **RF-21** — Debe permitir corregir manualmente una predicción ante eventos conocidos (promociones, feriados).

## D. Recomendaciones de compra

- [ ] **RF-22** — Debe generar una lista semanal de productos sugeridos para comprar con cantidades específicas.
- [ ] **RF-23** — Debe calcular cantidades considerando stock actual, stock mínimo, venta proyectada y lead time del proveedor.
- [ ] **RF-24** — Debe indicar cuándo realizar cada compra (fecha sugerida según punto de reorden).
- [ ] **RF-25** — Debe calcular el punto de reorden automáticamente por producto.
- [ ] **RF-26** — Debe priorizar las recomendaciones por urgencia (crítico / medio / bajo).
- [ ] **RF-27** — Debe generar órdenes de compra exportables o imprimibles.

## E. Alertas inteligentes

- [ ] **RF-28** — Debe alertar sobre productos con riesgo de agotarse pronto.
- [ ] **RF-29** — Debe detectar comportamientos inusuales en ventas (picos o caídas anómalas) con detección estadística.
- [ ] **RF-30** — Debe permitir configurar umbrales de alerta por producto o categoría.
- [ ] **RF-31** — Debe mostrar las alertas en un panel central y en el tablero principal.
- [ ] **RF-32** — Debe enviar un resumen semanal automático del estado del negocio.

## F. Asistente conversacional con IA local

- [ ] **RF-33** — Debe incluir un asistente de chat que responda preguntas en lenguaje natural sobre el negocio.
- [ ] **RF-34** — El modelo de IA debe ejecutarse localmente, sin depender de servicios en la nube.
- [ ] **RF-35** — El asistente debe basar sus respuestas en los datos reales del negocio (no en respuestas genéricas).
- [ ] **RF-36** — El asistente debe explicar en lenguaje sencillo los resultados estadísticos.
- [ ] **RF-37** — Debe citar las cifras y datos que sustentan cada respuesta.
- [ ] **RF-38** — Debe mantener contexto conversacional para preguntas de seguimiento.
- [ ] **RF-39** — Debe ofrecer botones de preguntas frecuentes.
- [ ] **RF-40** — Debe responder en español (con soporte configurable de otros idiomas).

## G. Optimización de inventario

- [ ] **RF-41** — Debe sugerir acciones para productos de baja rotación: liquidación, descuentos o no volver a comprarlos.
- [ ] **RF-42** — Debe permitir definir stock mínimo y máximo por producto.
- [ ] **RF-43** — Debe gestionar proveedores con su tiempo de entrega (lead time) y usarlo en las recomendaciones.
- [ ] **RF-44** — Debe permitir filtrar todo el análisis por categoría, proveedor y rango de fechas.

## H. Requisitos no funcionales

- [ ] **RNF-45** — La aplicación debe funcionar 100% offline; ningún dato debe salir del equipo (privacidad total).
- [ ] **RNF-46** — Debe correr en hardware modesto (8 GB RAM, sin GPU obligatoria), con aceleración GPU opcional.
- [ ] **RNF-47** — Debe importar archivos de hasta 10,000 filas en menos de 30 segundos.
- [ ] **RNF-48** — El asistente de IA debe responder en menos de 10 segundos en equipos básicos.
- [ ] **RNF-49** — Debe instalar con un solo instalador en Windows (deseablemente Linux), sin configuración técnica.
- [ ] **RNF-50** — Debe incluir respaldo automático de la base de datos local y cifrado de la información almacenada.

---

## Cobertura en el prototipo (semanas 0–4)

| Grupo | Estado en prototipo |
|---|---|
| A (datos) | RF-01, RF-04, RF-07 ✔ · importación de hoja "productos" (stock/costos, ADR-003) ✔ |
| B (análisis) | RF-09 parcial (KPIs) · **RF-11, RF-12, RF-13, RF-17 ✔ motor de rotación** |
| C/D (predicción y compra) | **RF-18, RF-19, RF-20, RF-22, RF-24, RF-25, RF-26 ✔** (modelo simple explicable; ARIMA post-prototipo) |
| E (alertas) | **RF-12, RF-28, RF-29 ✔** regeneración automática en GET /api/alertas |
| F (IA local) | Chat básico con Ollama ✔, function calling semana 3 |
| G/H (optimización y NFR) | Posterior al prototipo |
