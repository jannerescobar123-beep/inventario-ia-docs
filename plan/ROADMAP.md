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
