# 🗓️ Roadmap del prototipo — 4 semanas

## Semana 0 — Setup (1–2 días)

- [ ] Ambos: repo `inventario-ia` clonado, entorno corriendo (`start.sh` o `start.bat`)
- [ ] Ambos: leer `docs/ARQUITECTURA.md` y `docs/openapi.yaml` (contrato)
- [ ] Ambos: instalar Ollama + `ollama pull llama3.2:3b`
- [ ] A: ejecutar `python scripts/generar_datos_prueba.py` y verificar `datos/ventas_demo.xlsx`
- [ ] B: `npm install` en frontend y verificar las 3 pantallas

## Semana 1 — Los datos entran, los datos se ven

- [ ] A: mejoras al importador (RF-04, RF-05): deduplicación, más validaciones
- [ ] A: carga del Excel demo a SQLite y verificación de KPIs
- [ ] B: wizard de importación con vista previa de columnas (RF-02)
- [ ] B: dashboard con gráfico de ventas por día (Recharts)

## Semana 2 — Inteligencia

- [ ] A: rotación + días de cobertura + ABC (RF-11, RF-12, RF-13, RF-15, RF-17)
- [ ] A: predicción de demanda 7/14/30 días (RF-18, RF-19, RF-20)
- [ ] A: recomendaciones de compra + generación de alertas (RF-22, RF-25, RF-26, RF-28, RF-29)
- [ ] B: panel "¿Qué compro esta semana?" + panel de alertas
- [ ] B: filtros por categoría y rango de fechas (RF-44)

## Semana 3 — IA local

- [ ] A: herramientas para el LLM (kpis, predicción, rotación, recomendaciones) vía API
- [ ] B: function calling en el chat + contexto con cifras del negocio (RF-35, RF-36, RF-37)
- [ ] B: preguntas de seguimiento (RF-38) y ajuste de prompts

## Semana 4 — Prototipo demostrable

- [ ] Flujo completo: importar → dashboard → alertas → chat con respuestas con cifras
- [ ] B: empaquetado básico con pywebview o Tauri (RNF-49, si el tiempo alcanza)
- [ ] Ambos: ensayar el guion de demo (abajo)

## ✅ Definición de "listo" (para cualquier tarea)

1. Funciona en el PC del otro (no solo en el propio).
2. Cubre el requisito o tiene un issue que lo explica.
3. Revisada por el compañero (PR).

## 🎬 Guion de demo (5 minutos)

1. Importar `datos/ventas_demo.xlsx` desde la pantalla de Importar.
2. Mostrar dashboard: KPIs del negocio.
3. Mostrar panel de alertas: "estos 5 productos se agotan esta semana".
4. Preguntar al chat: "¿qué debo comprar esta semana?" → respuesta con cantidades.
5. Preguntar: "¿qué productos tienen baja rotación?" → respuesta con capital inmovilizado.
6. Cerrar: "todo corrió offline, en el propio PC del negocio".

---

## Estado real a 2026-11-20 (complemento del plan anterior)

> El plan de las 4 semanas se conserva tal cual. Este apartado registra en qué punto
> está cada línea según lo **verificado** en la rama `mi-mvp` del repositorio de
> código (`inventario-ia`), sin reescribir las listas originales.

| Semana | Línea del plan | Estado real |
|---|---|---|
| 1 | A: mejoras al importador (RF-04, RF-05): deduplicación, más validaciones | **RF-04 ✔** (validación y reporte de errores) · **RF-05 ✔** (importación incremental sin duplicados, commit `224b252`) |
| 1 | A: carga del Excel demo a SQLite y verificación de KPIs | **Hecho** (11 productos, 600 ventas del demo; KPIs verificados) |
| 1 | B: wizard de importación con vista previa de columnas (RF-02) | **Pendiente** (RF-02 sigue `[ ]`: el mapeo manual actual no persiste) |
| 1 | B: dashboard con gráfico de ventas por día (Recharts) | **Hecho** (top de productos con Recharts; el gráfico por día llega con la pantalla «Tendencias», RF-10) |
| 2 | A: rotación + días de cobertura + ABC (RF-11, RF-12, RF-13, RF-15, RF-17) | **Hecho** (motor de rotación; `GET /api/abc` y `GET /api/capital` expuestos con ADR-004) |
| 2 | A: predicción de demanda 7/14/30 días (RF-18, RF-19, RF-20) | **Hecho** (media móvil 7/14/30; ARIMA queda post-prototipo) |
| 2 | A: recomendaciones de compra + generación de alertas (RF-22, RF-25, RF-26, RF-28, RF-29) | **Hecho**, y ampliado: **RF-27 ✔** (órdenes exportables), **RF-30 ✔** y **RF-42 ✔** (umbrales y stock máximo, solo API), **RF-43 ✔** (lead time real por proveedor) |
| 2 | B: panel "¿Qué compro esta semana?" + panel de alertas | **Hecho** (pantallas «Qué comprar» y «Alertas») |
| 2 | B: filtros por categoría y rango de fechas (RF-44) | **Pendiente** — siguiente paso previsto; no iniciado |
| 3 | A: herramientas para el LLM vía API | **Hecho** (5 herramientas deterministas; el modelo no calcula) |
| 3 | B: function calling en el chat + contexto con cifras (RF-35, RF-36, RF-37) | **Hecho** |
| 3 | B: preguntas de seguimiento (RF-38) y ajuste de prompts | **Hecho** |
| 4 | Flujo completo: importar → dashboard → alertas → chat con respuestas con cifras | **Hecho y verificado** (ruta completa probada sobre copia de la BD) |
| 4 | B: empaquetado básico con pywebview o Tauri (RNF-49) | **Pendiente** (RNF-49 sigue `[ ]`) |
| 4 | Ambos: ensayar el guion de demo | **Pendiente** |

### Requisitos que quedan abiertos

| Requisito | Motivo |
|---|---|
| RF-02, RF-03 | Asistente de mapeo de columnas y plantillas guardadas |
| RF-06 | Historial de importaciones con reversión |
| RF-08 | Choca con ADR-003 (el stock físico viene de la hoja «productos»): requiere decisión |
| RF-21, RF-32, RF-41 | Corrección manual de predicciones, resumen semanal automático, acciones para baja rotación |
| RF-44 | Filtros por categoría, proveedor y rango de fechas |
| RF-51 a RF-55, RNF-56/57 | Bloqueados hasta que se redacte **ADR-005** (canal de señales y política de anonimización) |
| RNF-49, RNF-50 | Instalador único, respaldo y cifrado de la base local |

> Detalle de cada cierre con su evidencia: `REQUISITOS.md` → «Registro de avance —
> Cierre de RF en la rama `mi-mvp`». Decisiones de contrato: `decisiones/ADR-006-ampliacion-contrato-api.md`.

