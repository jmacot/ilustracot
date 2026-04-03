# IlustraCOT — Rediseño "Studio Creativo"

**Fecha:** 2026-04-03
**Scope:** Rediseño completo del CSS de index.html. Sin cambios en la lógica JS ni en los datos de prompts.

---

## 1. Dirección visual

Estética "Studio Creativo": luminosa, moderna, con personalidad visual tipo app de diseño (Figma/Canva). Se aleja del look clínico genérico actual manteniendo la estructura funcional.

### Fondo y superficie

| Token | Valor light | Valor dark |
|-------|-------------|------------|
| `--bg` | `#f8f6f3` (cálido crema) | `#111827` |
| `--surface` | `#ffffff` | `#1f2937` |
| `--surface2` | `#f0eeeb` | `#283244` |

### Acento principal

Degradado **Sky Blue → Indigo**: `linear-gradient(135deg, #0ea5e9, #6366f1)`.

Se usa en: pills activas, stepper completado, botón copiar, bordes de selección, check badges.

Para contextos donde se necesita un solo color sólido (texto, bordes): usar `#0ea5e9` como `--accent` en light y `#60a5fa` en dark.

| Token | Light | Dark |
|-------|-------|------|
| `--accent` | `#0ea5e9` | `#60a5fa` |
| `--accent-gradient` | `linear-gradient(135deg, #0ea5e9, #6366f1)` | `linear-gradient(135deg, #3b82f6, #818cf8)` |
| `--accent-light` | `rgba(14,165,233,0.08)` | `rgba(96,165,250,0.1)` |

Resto de tokens semánticos (green, amber, red, border, shadow, radius) se mantienen del Sistema B del CLAUDE.md.

---

## 2. Header

Mantiene el degradado oscuro del Sistema B con mejoras:

- **Dot grid** con div `.hero-dot-grid` (no `::before/::after`)
- **Hero glows** con divs `.hero-glow-1`, `.hero-glow-2` + animación `glowPulse`
- **Sky toggle** CSS puro (reemplaza el botón emoji `theme-toggle` actual)
- **Logo nuevo**: icono `✦` dentro de un div 42×42px con `border-radius: 12px` y fondo `var(--accent-gradient)`, alineado horizontalmente con el título
- **Layout**: flex row con logo + bloque de texto (título + subtítulo)
- **Enlace ← Inicio**: mantener `back-home` del Sistema B

### Dark mode header

```css
background: linear-gradient(135deg, #152238 0%, #1c2f4a 50%, #213656 100%);
```

---

## 3. Stepper visual

Reemplaza los cuadrados `.step-num` actuales por un stepper horizontal con líneas de progreso.

### Estructura

```
[●1 Categoría] ———— [●2 Estilo] ———— [○3 Prompt] ———— [○4 Refinar]
```

### Estados

| Estado | Círculo | Línea anterior | Texto |
|--------|---------|----------------|-------|
| Completado | `var(--accent-gradient)`, blanco, ✓ | `var(--accent-gradient)` | `color: var(--text)` |
| Activo | `var(--accent-gradient)`, blanco, número | — | `color: var(--text); font-weight: 700` |
| Pendiente | `#e0e0e0`, `#999`, número | `#e0e0e0` | `color: #999` |

### Dimensiones

- Círculos: 28×28px, `border-radius: 50%`
- Líneas: `height: 2px`, `flex: 1`
- Transición de línea: `width` animado con `cubic-bezier(0.16, 1, 0.3, 1)` al completar paso
- Paso activo: animación `stepPulse` sutil

### Responsive (≤600px)

- Ocultar labels de texto, mostrar solo círculos + líneas
- Círculos: 24×24px

---

## 4. Pills de categoría

Mantienen la estructura actual `.cat-pills` con cambios visuales:

| Propiedad | Inactiva | Activa |
|-----------|----------|--------|
| Background | `var(--surface)` | `var(--accent-gradient)` |
| Border | `1.5px solid var(--border)` | `transparent` |
| Color texto | `var(--text-muted)` | `white` |
| Shadow | ninguna | `0 4px 12px rgba(14,165,233,0.25)` |

- Iconos SVG inline se mantienen
- Badge `.count` se mantiene
- Tamaño touch: `padding: 10px 20px` (≥44px de alto)

---

## 5. Tarjetas de estilo (carrusel)

Reemplaza la lista vertical `.styles-grid` por un carrusel horizontal.

### Contenedor

```css
.styles-carousel {
  display: flex;
  gap: 12px;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
  padding: 4px 0 8px;
}
.styles-carousel::-webkit-scrollbar { display: none; }
```

### Tarjeta individual

- Ancho fijo: `155px` (desktop), `140px` (móvil)
- `flex-shrink: 0`
- `scroll-snap-align: start`
- `border-radius: 16px`
- Overflow hidden
- Border: `1.5px solid var(--border-light)`

### Estructura interna

```
┌─────────────────┐
│  Degradado/thumb │  ← 90px height, gradiente CSS o thumbnail base64
│         [✓]     │  ← check badge top-right si activa
├─────────────────┤
│  Nombre estilo   │  ← 14px, font-weight 700
│  Descripción     │  ← 10px, color muted
└─────────────────┘
```

### Estado activo

```css
border-color: var(--accent);
box-shadow: 0 0 0 3px rgba(14,165,233,0.15), 0 8px 24px rgba(0,0,0,0.08);
```

Check badge: 22×22px, `border-radius: 50%`, fondo `var(--accent-gradient)`, ✓ blanco.

### Fondos por estilo

Se mantienen los degradados CSS existentes (`data-style-id`). Cuando hay thumbnail base64 (`.has-thumb`), se usa como `background-image: cover`.

---

## 6. Panel de resultado (prompt)

Card con bordes suaves y glassmorphism sutil en el header.

### Estructura

```
┌─ Result header ─────────────────────────┐
│  📄 Nombre estilo · Categoría    ~tokens│
├─ Config strip ──────────────────────────┤
│  Modelo │ Temp │ Top P │ Top K          │
├─ Prompt scroll ─────────────────────────┤
│  (monospace, scrollable, max-height)    │
├─ Copy bar ──────────────────────────────┤
│  [ 📋 Copiar prompt ]  ← degradado     │
└─────────────────────────────────────────┘
```

### Cambios respecto al actual

- `border-radius: 16px` (mantiene)
- `border: 1px solid var(--border-light)` (más sutil)
- Config strip: valores en `color: var(--accent)` en vez de navy
- Botón copiar: `background: var(--accent-gradient)` con hover que intensifica
- Estado copiado: `background: var(--green)` (mantiene)
- Fondo prompt scroll: `var(--surface2)` con custom scrollbar

---

## 7. Sección de refinamiento

Se mantiene la grid 2×2 de `.refine-chip` con ajustes:

- Border-radius: mantiene `var(--radius-sm)` (8px)
- Estado copiado: `border-color: var(--green); background: var(--green-light)`
- `.refine-config` tag: `color: var(--accent)` en vez de navy

---

## 8. Tips card

Se mantiene con ajuste mínimo de border-radius a 16px.

---

## 9. Animaciones

### Existentes (mantener)

- `fadeInUp`: header, secciones
- `slideReveal`: secciones que aparecen
- Toast: slide-up cubic-bezier

### Nuevas

- **Stepper fill**: línea de progreso se llena con transición 0.6s al completar paso
- **stepPulse**: pulso sutil en el paso activo (ya definido en CLAUDE.md)
- **Card hover**: `transform: translateY(-3px)` + sombra elevada en tarjetas de estilo
- **Carousel snap**: `scroll-snap-type: x mandatory` para snap suave

### Reduced motion

Todas las animaciones anuladas con `@media (prefers-reduced-motion: reduce)`.

---

## 10. Accesibilidad

- **Skip link**: añadir `<a href="#main" class="skip-link">Saltar al contenido</a>`
- **Focus visible**: `outline: 2px solid var(--accent)` en todos los interactivos
- **Touch targets**: mínimo 44px en pills, tarjetas de estilo, botones
- **aria-label**: en sky toggle, botones de copia, pills
- **Smooth scroll**: `html { scroll-behavior: smooth; }`
- **Carrusel**: `role="listbox"` en contenedor, `role="option"` en tarjetas

---

## 11. Responsive

### ≤600px

- Stepper: solo círculos, sin labels de texto
- Carrusel: tarjetas 140px de ancho
- Pills: scroll horizontal natural (ya existe)
- Header: padding reducido, ocultar glows
- Sky toggle: `--toggle-size: 16px`
- Config strip: font-size reducido

### ≤480px

- Refine grid: 1 columna en vez de 2

---

## 12. Fuera de scope

- **No se cambia** la lógica JavaScript de generación de prompts
- **No se cambian** los datos de PROMPTS, REFINEMENTS, STYLE_THUMBS
- **No se cambia** la estructura HTML de las secciones (solo clases CSS y atributos)
- **No se añaden** nuevas funcionalidades
- El stepper requiere cambios mínimos en JS para actualizar estados al navegar pasos
