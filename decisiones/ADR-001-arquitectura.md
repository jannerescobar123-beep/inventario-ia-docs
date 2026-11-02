# ADR-001 — Arquitectura del sistema

- **Estado:** Aceptada
- **Fecha:** 2026-11-02
- **Decide:** Equipo completo (A + B)

## Contexto

Software para PyMEs (tiendas, ferreterías) que analiza ventas, predice demanda,
recomienda compras y responde preguntas con IA. Restricciones clave:

- Debe correr **offline** en el PC del negocio (privacidad: los datos de ventas no salen).
- Hardware modesto: ~8 GB RAM, GPU no garantizada.
- Equipo de 2 personas, prototipo funcional en 4 semanas.

## Decisión

Arquitectura **"web local"**: FastAPI (Python) + SQLite + React/Vite + Ollama como LLM local.

| Capa | Elección |
|---|---|
| Backend / estadística | Python 3.11 + FastAPI + pandas |
| Base de datos | SQLite (archivo único, cero configuración) |
| Frontend | React + Vite + Recharts |
| IA local | Ollama (llama3.2:3b) vía API local con function calling |
| Distribución futura | pywebview o Tauri envolviendo esta misma app |

## Alternativas consideradas

1. **Electron + Node**: UI potente, pero el ecosistema estadístico (predicción de demanda)
   es muy superior en Python; dos runtimes pesados para un equipo de 2.
2. **PySide6/PyQt (todo Python)**: un solo lenguaje, pero UIs de escritorio son más lentas
   de prototipar y el chat/dashboards lucen peor con más esfuerzo.
3. **Nube (OpenAI API + Postgres gestionado)**: mejor calidad de LLM, pero viola el
   requisito de privacidad/offline y agrega costo mensual para el dueño del negocio.

## Consecuencias

- ✅ Prototipado rápido en paralelo (contrato API desacopla a A y B).
- ✅ Privacidad y costo cero por licencias/APIs.
- ⚠️ El LLM local (3B) es menos capaz que uno en nube → mitigar con function calling
  y respuestas basadas en datos estructurados, no en razonamiento libre.
- ⚠️ SQLite no soporta escritura concurrente intensa → suficiente para 1 tienda por PC;
  migrar a PostgreSQL solo si se ofrecen multi-sucursal (post-prototipo).
