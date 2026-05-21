# GreenAdmin — Dashboard Administrativo

Dashboard administrativo moderno construido con **Vue 3**, **Composition API** y **Vite**.

## Tecnologías

| Tecnología | Versión | Rol |
|---|---|---|
| Vue 3 | ^3.4 | Framework reactivo con Composition API |
| Vite | ^5.0 | Bundler y servidor de desarrollo |
| CSS Variables | nativo | Theming y diseño consistente |
| CSS Grid + Flexbox | nativo | Layout responsivo sin dependencias |

## Estructura de archivos

```
src/
├── components/
│   ├── AppSidebar.vue     # Sidebar colapsable con navegación
│   ├── AppHeader.vue      # Header con buscador y notificaciones
│   ├── StatsCard.vue      # Tarjeta de estadística reutilizable
│   ├── DataTable.vue      # Tabla de proyectos con filtros y orden
│   └── ProgressChart.vue  # Gráfico de barras en CSS puro
├── views/
│   └── DashboardView.vue  # Vista principal que ensambla componentes
├── App.vue                # Layout con CSS Grid y variables globales
└── main.js                # Punto de entrada de la app
```

## Inicio rápido

```bash
npm install
npm run dev
```

## Decisiones de diseño

### Layout CSS Grid
`App.vue` usa `grid-template-areas` para definir la estructura macro:
- `sidebar` ocupa toda la altura izquierda
- `header` queda fijo en la parte superior derecha
- `main` es el área de contenido principal
- `footer` aparece al fondo del contenido

### Sidebar colapsable
El ancho del sidebar cambia entre `240px` y `64px` mediante una variable CSS (`--sidebar-width` / `--sidebar-collapsed-width`). La transición `grid-template-columns` en `.app-layout` garantiza que todo el layout se reajuste fluidamente.

### Gráfico sin librerías
`ProgressChart.vue` construye un gráfico de barras usando exclusivamente Flexbox + porcentajes de altura calculados con `(valor / máximo) * 100`. La barra más alta se resalta con `--color-primary`.

### Paleta de colores
| Variable | Valor | Uso |
|---|---|---|
| `--color-primary` | `#1a472a` | Verde bosque — botones, acentos, sidebar |
| `--color-primary-light` | `#2d6a4f` | Verde medio — estados activos |
| `--color-bg` | `#f4f6f4` | Gris verdoso suave — fondo de página |
| `--color-surface` | `#ffffff` | Blanco — tarjetas y paneles |

## Accesibilidad

- **Roles ARIA**: `role="navigation"`, `role="main"`, `role="banner"`, `role="contentinfo"`, `role="table"`, `role="list"`
- **`aria-current="page"`**: en el enlace activo del sidebar
- **`aria-expanded`**: en el botón de colapsar sidebar y notificaciones
- **`aria-label`**: en todos los íconos decorativos y controles interactivos
- **`aria-sort`**: en columnas de tabla ordenables
- **Skip link**: "Ir al contenido principal" aparece al enfocar con Tab
- **Contraste**: combinación `#1a472a` sobre blanco supera WCAG AA (ratio > 7:1)
- **Navegación por teclado**: todos los elementos interactivos tienen `:focus-visible` con outline visible

## Responsive

| Breakpoint | Comportamiento |
|---|---|
| > 900px | Sidebar expandido `240px`, layout completo |
| 640–900px | Sidebar colapsado `64px`, solo íconos |
| < 640px | Sidebar oculto, header ocupa todo el ancho |

## Capturas de pantalla

> _Las capturas se agregarán tras el primer despliegue._

```
[Desktop — Sidebar expandido]
[Tablet — Sidebar colapsado]
[Mobile — Menú superior]
```
