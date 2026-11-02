# ADR-002 — IA local con Ollama y function calling

- **Estado:** Aceptada
- **Fecha:** 2026-11-02
- **Decide:** Equipo completo (A + B)

## Contexto

El asistente debe explicar resultados y responder preguntas usando **los datos reales
del negocio**, sin conexión a internet y sin enviar información de ventas a terceros
(RF-33 a RF-40, RNF-45). El hardware objetivo es modesto (8 GB RAM, sin GPU).

## Decisión

1. **Motor:** Ollama con el modelo `llama3.2:3b` (cuantizado; configurable por variable
   de entorno `OLLAMA_MODEL`).
2. **Patrón:** *function calling*. El LLM no adivina cifras: invoca herramientas
   (`obtener_kpis`, `predecir_demanda`, `productos_baja_rotacion`, `riesgo_agotamiento`)
   que el backend implementa consultando SQLite, y explica esos resultados en lenguaje
   sencillo.
3. **Las predicciones las hacen algoritmos estadísticos** (media móvil, Holt, ARIMA),
   no el LLM. El LLM solo **explica e interpreta**. Esto garantiza números correctos
   y reproducibles.

## Alternativas consideradas

1. **API de nube (OpenAI/Claude)**: mejor razonamiento, pero viola RNF-45 y tiene costo por uso.
2. **Transformers directos en Python**: más control, pero mucha más ingeniería para
   servir el modelo (Ollama resuelve serving, cuantización y API en un paso).
3. **Text-to-SQL puro**: flexible, pero arriesgado en un prototipo (SQL incorrecto →
   cifras erróneas). Se prefiere herramientas fijas y auditables; text-to-SQL con
   guardias puede evaluarse después.

## Consecuencias

- ✅ Respuestas con cifras correctas (vienen de código determinista, no del modelo).
- ✅ Privacidad total y costo cero.
- ⚠️ Modelo 3B: respuestas más simples que un modelo grande → mitigar con prompts
  estrictos y contexto ya calculado.
- ⚠️ Primera respuesta puede tardar ~5–15 s en equipos básicos (RNF-48): medir en
  semana 3 y, si hace falta, probar modelos más pequeños (qwen2.5:1.5b).
