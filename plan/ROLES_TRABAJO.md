# 👥 Roles y rutina del equipo

## Persona A — Núcleo de datos e inteligencia (backend)

**Posee:** `backend/` completo.

- Importador de Excel/CSV con validación y reporte de errores (RF-01 a RF-06)
- Modelo de datos SQLite y datos de prueba realistas
- Motor estadístico: rotación, ABC, cobertura, predicciones (RF-09 a RF-21)
- Recomendaciones de compra, punto de reorden y alertas (RF-22 a RF-32)
- Endpoints FastAPI + herramientas para el LLM (semana 3)
- Cuidar el rendimiento de importación (RNF-47)

## Persona B — Producto: UI y Asistente IA (frontend)

**Posee:** `frontend/` completo + prompts del asistente.

- Dashboard con KPIs, gráficos y filtros (RF-09, RF-16, RF-44)
- Wizard de importación con mapeo de columnas (RF-02, RF-03)
- Panel de alertas y vista "¿qué compro esta semana?" (RF-26, RF-28, RF-31)
- Chat: integración Ollama, prompts, function calling, contexto con cifras (RF-33 a RF-40)
- Experiencia general: estados vacíos, cargas, errores en español sencillo
- Empaquetado final del prototipo (RNF-49)

## Regla de oro

**El contrato de la API (`docs/openapi.yaml`) es la ley.** Ambos trabajan en paralelo
contra ese contrato; nadie bloquea al otro. Cualquier cambio → ADR + aviso el mismo día.

## Rutina

| Momento | Qué |
|---|---|
| Diario (15 min) | Qué hice, qué haré, qué me bloquea |
| Semanal (45 min) | Demo rápida + actualizar checklist de `REQUISITOS.md` + planear semana |
| Al terminar tarea | PR pequeño → revisa el compañero → merge |

## Convenciones

- Ramas: `feat/nombre-corto`, `fix/nombre-corto`, `docs/nombre-corto`
- Commits: `feat: ...`, `fix: ...`, `docs: ...`
- Los `TODO(Persona A)` / `TODO(Persona B)` del código marcan el trabajo pendiente
