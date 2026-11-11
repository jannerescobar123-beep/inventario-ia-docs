# Dirección de diseño — Inventario-IA (web MVP)

> Registro de la dirección visual aprobada e implementada.
> Estados: `propuesta` → `revisada por Jarvis` → `aprobada por el usuario` → `implementada` → `verificada`.

## 1. Contexto

- **Producto:** MVP web de análisis de ventas e inventario con IA local (tiendas, ferreterías y PyMEs).
- **Usuario final:** dueño de negocio sin formación técnica; consulta cifras desde móvil o escritorio.
- **Stack visual:** React 18 + Vite 5 + react-bootstrap 2.10 + Recharts 2.15. Sin dependencias nuevas.
- **Restricción dura:** la app funciona **100 % offline** (RNF-45) → sin CDN, sin Google Fonts, sin peticiones externas. Tipografía: solo pila del sistema.

## 2. Dirección aprobada

| Decisión | Valor | Estado |
|---|---|---|
| Estilo | Minimalista profesional, producto de escritorio (no plantilla) | Aprobada por el usuario (2026-11-08) |
| Tema | **Modo oscuro** en toda la aplicación | Aprobada por el usuario (2026-11-08) |
| Tipografía | Grande y fluida (base 17–18 px, títulos con `clamp()`) | Aprobada por el usuario (2026-11-08) |
| Movimiento | Animaciones 3D discretas con propósito | Aprobada por el usuario (2026-11-08) |
| Responsive | Mobile-first, sin desbordamiento horizontal a 360 px | Aprobada por el usuario (2026-11-08) |
| Navegación | Menú de hamburguesa en móvil | Aprobada por el usuario (2026-11-08) |
| Chat | Botón flotante de chatbot que abre el chat en un panel | Aprobada por el usuario (2026-11-08) |

## 3. Sistema visual

### 3.1 Paleta (tokens)

| Rol | Valor | Uso |
|---|---|---|
| Fondo base | `#0B0F14` / `#0E1117` | Lienzo de la aplicación |
| Superficie elevada | `#151A21` | Tarjetas, paneles, tablas, drawer |
| Superficie hover | `#1C222B` | Estados interactivos |
| Borde | `rgba(255,255,255,.08)` | Separadores discretos, sin ruido visual |
| Texto principal | `#F1F5F9` | Títulos y cifras |
| Texto secundario | `#94A3B8` | Etiquetas y ayudas |
| Acento | `#6366F1` (índigo) | Acción primaria, estado activo, foco |
| Éxito | `#34D399` | Stock sano, KPI positivo |
| Alerta | `#FBBF24` | Riesgo medio, urgencia |
| Peligro | `#F43F5E` | Agotamiento inminente, errores |

### 3.2 Tipografía

- Pila: `ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif`.
- Base: **17–18 px**, `line-height: 1.6`.
- Títulos: escala fluida con `clamp()` (≈28 px en móvil → 40 px en escritorio), `letter-spacing` ceñido (-0.02em).
- Cifras (KPIs, montos, cantidades): `font-variant-numeric: tabular-nums` para que las columnas de números queden alineadas.
- Etiquetas secundarias: 13–14 px, mayúsculas con `letter-spacing: .06em` solo en títulos de sección.

### 3.3 Forma y profundidad

- Radio de borde coherente: **12–16 px** (tarjetas), 10 px (controles), pill para el botón flotante.
- Sombras profundas y suaves (elevación por capa, nunca contornos duros).
- Espaciado en múltiplos de 8 px; aire generoso (minimalismo).

## 4. Movimiento 3D

- **3D con CSS puro** (`perspective`, `transform-style: preserve-3d`, `translateZ`, `rotateX/rotateY`). Sin Three.js ni librerías nuevas.
- Aplicaciones: tarjetas KPI con inclinación sutil al hover, fondo con capas flotantes muy lentas, botón de chat con relieve y pulso suave, drawer del chat con entrada en `rotateY`.
- Curva estándar: `cubic-bezier(.22,.61,.36,1)`.
- **`prefers-reduced-motion: reduce`** anula 3D, pulsos y transiciones (respeto a accesibilidad).
- El 3D nunca debe dificultar la lectura de cifras: nunca texto sobre superficies animadas en reposo.

## 5. Navegación y estructura

- **Navbar superior** con marca, enlaces y badge de estado de la API ("API conectada"). En móvil se convierte en **menú de hamburguesa** con panel deslizante (`translate3d`), foco gestionado, cierre al navegar y con `Escape`.
- **Rutas actuales (sin cambios):** `/` (Panel de control), `/importar`, `/alertas`, `/compras` (Qué comprar), `/chat`.
- **Botón flotante de chatbot**: fijo abajo a la derecha, siempre visible. Al abrirlo:
  - escritorio → drawer lateral ancho;
  - móvil → hoja inferior (bottom sheet).
  - Comparte el mismo componente de chat que la ruta `/chat`, contra `POST /api/chat`.

## 6. Tablas y datos

- Cabecera fija visual, filas con hover suave, cifras alineadas a la derecha.
- En pantallas estrechas: scroll horizontal con la primera columna fija o ficha por tarjeta; **sin** desbordar el ancho del viewport.
- Estados vacíos y de error con texto accionable, no spinners eternos.

## 7. Accesibilidad

- Contraste AA sobre fondo oscuro en texto, iconos y bordes de control.
- Foco visible en todos los controles (anillo de acento).
- `aria-label` en botones de solo icono; el drawer y el menú gestionan foco y devuelven el foco al cerrarse.
- Navegación completa por teclado.

## 8. Reglas de implementación

1. Todo el CSS en un único archivo `frontend/src/styles.css` con tokens al principio; nada de estilos inline salvo el mínimo puntual.
2. Bootstrap CSS se importa (dependencia ya existente); el tema oscuro se aplica por encima, no se pelea con sus variables.
3. Cero datos de ejemplo hardcodeados: todo lo visible viene de la API.
4. Cero dependencias nuevas, cero CDN, cero peticiones externas.

## 9. Verificación

| Validación | Estado | Evidencia |
|---|---|---|
| Técnica (build) | **VERIFICADO** | `npm run build` → exit 0, 674 módulos, CSS 22,73 kB, JS 628,20 kB (aviso de tamaño >500 kB, no error) |
| Visual | **VERIFICADO_VISUALMENTE** (360 y 1440) | `ui-ux-design-director` sobre Chrome DevTools MCP: fondo `#0B0F14`, `.page-title` 49,5 px vs `.kpi-value` hero 49,5 px (protagonista 1,47× frente al resto), 0 franjas claras |
| Funcional | En ejecución | Pruebas de `qa-tester` sobre Chrome DevTools MCP (navegación, hamburguesa, chat real) |
| Responsive | **VERIFICADO a 360** (con `emulate 360x780x2`) | Sin contenido fuera del viewport tras corregir `box-sizing`; hamburguesa y FAB completos. 768 / 1024 / 1920 pendientes |
| Consola | **VERIFICADO** | 0 errores, 0 issues; favicon resuelto con data-URI (antes 404) |
| Contraste | **VERIFICADO (computado)** | `--text-3: #8C9AAD` → `.state-hint` 6,61:1 y `.kpi-hint` 6,11:1 (AA cumplido). Medición en pantalla con `evaluate_script` denegada |

### Defectos encontrados y corregidos en la auditoría

| # | Defecto | Causa | Corrección |
|---|---|---|---|
| P1 | Contenido recortado a la derecha (H1, hamburguesa, 4.ª tarjeta KPI, columna «Llegada estimada») | Faltaba `box-sizing: border-box` global: lo aportaba Bootstrap, que no está instalado. `.page` sumaba `width:100%` + padding | Regla global `*, *::before, *::after { box-sizing: border-box }` + `body { margin: 0 }` + ajuste de `.topbar-inner` por debajo de 480 px |
| P1 | Los 6 chips «crítico» se pintaban como `urg-bajo` y el KPI «Urgencia crítica» mostraba 0 | `"crítico".startsWith("crit")` falla por la tilde | Normalización NFD sin diacríticos antes de comparar (mismo criterio que `nombre_norm` en SQLite) |
| P2 | `.state-hint` 3,96:1 y `.kpi-hint` 3,67:1 (fallaban AA) | `--text-3: #64748B` demasiado oscuro | `--text-3: #8C9AAD` → 6,11–6,61:1 |
| P3 | 404 de `/favicon.ico` en cada carga | No existía favicon | `<link rel="icon">` con SVG en data-URI (sin petición externa, cumple RNF-45) |
| P3 | Aviso de campo de formulario sin `id`/`name` | `input` del chat | `id="chat-mensaje"` + `name="mensaje"` |
| P1 | La app no cargaba en el navegador: «Failed to resolve import "bootstrap/dist/css/bootstrap.min.css"» | `react-bootstrap` no instala `bootstrap` (es *peer dependency*): el paquete no está en `node_modules` | JSX reescrito con las clases del sistema de diseño propio; cero imports de Bootstrap |
| P2 (QA) | El chat respondía con el literal `<producto>` y `(urgencia)` en lugar de los nombres y la urgencia reales | El modelo local de 1,7 B copiaba la plantilla con marcadores angulares | Instrucción sin marcadores + ejemplo con datos reales + reparación determinista (`_reparar_placeholders`) con los valores de SQLite |
| P3 (QA) | Aviso «Blocked aria-hidden on an element because its descendant retained focus» al navegar desde el menú | El foco seguía en el enlace del menú cuando se ocultaba con `aria-hidden` | Al pulsar un enlace del menú, el foco pasa a `<main tabIndex={-1}>` antes de cerrarlo. Corrección aplicada; re-verificación en navegador en curso |

> Los estados se actualizan al cierre de la iteración con la evidencia real; no se marca nada como verificado sin comprobarlo.
