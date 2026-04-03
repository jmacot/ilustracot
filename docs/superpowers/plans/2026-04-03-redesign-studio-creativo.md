# IlustraCOT Rediseño "Studio Creativo" — Plan de Implementación

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rediseñar completamente el CSS de IlustraCOT con estética "Studio Creativo" — fondo cálido, degradado Sky Blue a Indigo, carrusel horizontal de estilos, stepper visual con líneas de progreso, sky toggle y accesibilidad.

**Architecture:** Single-file HTML (index.html). Todas las modificaciones son en el bloque style (CSS), el bloque HTML del body (estructura mínima) y el bloque script (JS del tema y stepper). Los datos de PROMPTS, REFINEMENTS y STYLE_THUMBS no se tocan.

**Tech Stack:** HTML5, CSS3, JavaScript vanilla, Google Fonts (Inter + Lora)

---

### Task 1: Variables CSS y fondo cálido

**Files:**
- Modify: `index.html:13-41` (variables :root y [data-theme="dark"])

- [ ] **Step 1: Reemplazar variables :root light**

En index.html:13-27, reemplazar el bloque :root completo. Nuevos valores:
- --bg: #f8f6f3 (cálido crema)
- --surface2: #f0eeeb
- --accent: #0ea5e9
- --accent-light: rgba(14,165,233,0.08)
- --accent-hover: #0284c7
- Nuevo token: --accent-gradient: linear-gradient(135deg, #0ea5e9, #6366f1)
- Resto de tokens se mantienen

- [ ] **Step 2: Reemplazar variables [data-theme="dark"]**

Nuevos valores dark:
- --bg: #111827
- --accent: #60a5fa
- --accent-light: rgba(96,165,250,0.1)
- --accent-hover: #93c5fd
- --accent-gradient: linear-gradient(135deg, #3b82f6, #818cf8)
- --green-light: #14352a
- --amber-light: #3b2a10
- --red-light: #3b1010

- [ ] **Step 3: Verificar en el navegador**

Abrir index.html en el navegador. El fondo debe ser crema cálido #f8f6f3 en light y #111827 en dark.

- [ ] **Step 4: Commit**

Mensaje: "style: update CSS variables to Studio Creativo palette"

---

### Task 2: Header con logo, dot grid, hero glows y sky toggle

**Files:**
- Modify: `index.html:52-91` (CSS del header y theme-toggle)
- Modify: `index.html:479-484` (HTML del header)
- Modify: `index.html:711-742` (JS del tema)

- [ ] **Step 1: Reemplazar CSS del header**

Reemplazar index.html:52-91 (desde HEADER hasta .theme-toggle:hover) por:
- .app-header: text-align left, mismos degradados
- .hero-dot-grid: position absolute, inset 0, radial-gradient dots 24px spacing
- .hero-glow divs: dos orbs con glowPulse animation (6s ease-in-out infinite)
- .header-inner: flex row, gap 14px, z-index 1
- .header-logo: 42x42px, border-radius 12px, background var(--accent-gradient), centered sparkle
- .sky-toggle: CSS puro sol/luna con --toggle-size 20px, absolute top 12px right 12px
  - Container: 2.6x width, 1.3x height, rounded pill
  - Light state: sky blue gradient background, gold circle (sun)
  - Dark state (checked): dark blue background, grey circle with crater spots (moon)
  - Clouds: white blur, fade out on dark
  - Stars: SVG circles, fade in on dark
  - Transition: .5s cubic-bezier(0, -0.02, 0.4, 1.25)

- [ ] **Step 2: Reemplazar HTML del header**

Reemplazar index.html:479-484 por nuevo header con:
- div.hero-dot-grid
- div.hero-glow.hero-glow-1 y .hero-glow-2
- label.sky-toggle con input#sky-check y estructura sun/moon
- div.header-inner con div.header-logo (sparkle entity 10022) y bloque h1 + p

- [ ] **Step 3: Reemplazar JS del tema**

Reemplazar index.html:711-742 (desde THEME hasta initTheme) por:
- skyCheck = getElementById('sky-check')
- applyTheme(dark): set data-theme, skyCheck.checked, meta-theme content
- getAutoTheme(): hour-based (21-7 dark) + prefers-color-scheme
- initTheme(): localStorage con expiracion diaria (theme + theme-date)
- skyCheck change listener: applyTheme + localStorage save
- matchMedia change listener: auto-theme si no hay preferencia guardada hoy

- [ ] **Step 4: Verificar en el navegador**

Header con logo sparkle + título alineado izquierda, dot grid, glows pulsando, sky toggle funcional.

- [ ] **Step 5: Commit**

Mensaje: "style: redesign header with logo, dot grid, glows and sky toggle"

---

### Task 3: Stepper visual

**Files:**
- Modify: `index.html` — añadir CSS del stepper (después del CSS del header)
- Modify: `index.html:486-505` (HTML de secciones, añadir stepper)
- Modify: `index.html:605-614` (JS selectCat — actualizar stepper)
- Modify: `index.html:643-665` (JS pickStyle — actualizar stepper)

- [ ] **Step 1: Añadir CSS del stepper**

Añadir después del sky toggle CSS, antes de SECTIONS:
- .stepper: flex row, padding 20px 4px 6px, fadeInUp animation
- .stepper-step: flex, align-items center, gap 8px
- .stepper-num: 28x28px circle, default #e0e0e0 background, #999 text
- .stepper-text: 12px, font-weight 600, default #999
- .stepper-line: flex 1, height 2px, #e0e0e0 background, with ::after pseudo for fill animation
- .stepper-line.filled::after: width 100%, accent-gradient, 0.6s cubic-bezier transition
- .stepper-step.active .stepper-num: accent-gradient background, white text, stepPulse 2s infinite
- .stepper-step.done .stepper-num: accent-gradient background, white check SVG
- Dark mode overrides: #374151 backgrounds for inactive states
- Responsive (600px): hide .stepper-text, reduce num to 24px, reduce line margin

- [ ] **Step 2: Añadir HTML del stepper**

Insertar después del cierre de .app-header, antes de PASO 1:
- div.stepper#stepper con 4 stepper-step (data-step 1-4) y 3 stepper-line (data-line 1-3)
- Labels: Categoría, Estilo, Prompt, Refinar
- Step 1 starts with class "active"

- [ ] **Step 3: Eliminar los .section-label con .step-num del HTML**

Eliminar las 4 líneas de section-label que contienen .step-num en los pasos 1-4.

- [ ] **Step 4: Eliminar CSS de .section-label y .step-num**

Eliminar index.html:98-110 (el bloque .section-label y .step-num).

- [ ] **Step 5: Añadir función JS updateStepper**

Añadir antes de function selectCat:
- updateStepper(activeStep): itera stepper-steps, marca done (con check SVG) para n menor que activeStep, active para n igual, pendiente para n mayor. Itera stepper-lines, toggle class filled para n menor que activeStep.

- [ ] **Step 6: Llamar updateStepper en selectCat y pickStyle**

En selectCat, añadir updateStepper(2) después de show('sec-style').
En pickStyle, añadir updateStepper(3) después de los show().

- [ ] **Step 7: Verificar en el navegador**

Al abrir: paso 1 activo pulsando. Al elegir categoría: paso 1 check, línea llena, paso 2 activo. Al elegir estilo: pasos 1-2 check, paso 3 activo.

- [ ] **Step 8: Commit**

Mensaje: "feat: add visual stepper with progress lines and pulse animation"

---

### Task 4: Pills de categoría con degradado

**Files:**
- Modify: `index.html` — CSS de .cat-pill (~120-138)

- [ ] **Step 1: Reemplazar CSS de .cat-pill.active**

Cambiar:
- background: var(--accent-gradient)
- border-color: transparent
- box-shadow: 0 4px 12px rgba(14,165,233,0.25)
- Dark mode: color white (eliminar color #0f1724)

- [ ] **Step 2: Aumentar padding para touch targets**

Cambiar .cat-pill padding de 8px 16px a 10px 20px.

- [ ] **Step 3: Verificar y Commit**

Mensaje: "style: category pills with gradient accent and larger touch targets"

---

### Task 5: Carrusel horizontal de estilos

**Files:**
- Modify: `index.html` — CSS de .styles-grid y .style-card (~140-297)
- Modify: `index.html:514` (HTML del contenedor)
- Modify: `index.html:616-641` (JS renderStyles)

- [ ] **Step 1: Reemplazar CSS completo de estilos**

Reemplazar todo desde STYLE PALETTE hasta el final de radiology styles por:
- .style-palette-title: color var(--accent) en vez de text-light, font-size 10px
- .styles-carousel: flex row, gap 12px, overflow-x auto, scroll-snap-type x mandatory, hide scrollbar
- .style-card: flex-shrink 0, width 155px, border-radius 16px, border 1.5px solid border-light, background surface
- .style-card.active: border-color accent, box-shadow con accent ring
- .style-card-thumb: height 90px, background-size cover
- .style-card-thumb .check-badge: 22x22px circle, accent-gradient bg, absolute top-right
- .style-card-body: padding 12px, .style-name 14px bold, .style-desc 10px muted
- Per-style backgrounds: mismos degradados pero aplicados a .style-card-thumb en vez de ::before

- [ ] **Step 2: Cambiar clase del contenedor HTML**

Cambiar class="styles-grid" a class="styles-carousel" en el div id="styles-grid".

- [ ] **Step 3: Reescribir JS renderStyles**

Nueva estructura de card generada por JS:
- div.style-card con data-style-id
- Hijo: div.style-card-thumb (con background-image inline si hay thumbnail base64)
  - Hijo: div.check-badge con SVG check
- Hijo: div.style-card-body
  - Hijo: div.style-name con s.name
  - Hijo: div.style-desc con s.desc

Nota: El contenido de s.name y s.desc proviene del array PROMPTS que es hardcoded en el propio archivo — no es input de usuario. No requiere sanitización.

- [ ] **Step 4: Verificar en el navegador**

Tarjetas en carrusel horizontal, scroll snap, thumbnails, check badge en activa.

- [ ] **Step 5: Commit**

Mensaje: "feat: convert style grid to horizontal carousel with card thumbnails"

---

### Task 6: Botón copiar con degradado

**Files:**
- Modify: `index.html` — CSS de .btn-primary

- [ ] **Step 1: Actualizar CSS del botón**

Cambios:
- background: var(--accent-gradient) en vez de var(--accent)
- hover: filter brightness(1.1) en vez de background accent-hover
- Eliminar las 2 líneas de dark mode override (color #0f1724) — texto siempre blanco

- [ ] **Step 2: Verificar y Commit**

Mensaje: "style: gradient accent on copy button and cleanup dark overrides"

---

### Task 7: Responsive y accesibilidad

**Files:**
- Modify: `index.html` — CSS responsive
- Modify: `index.html` — HTML (skip link)

- [ ] **Step 1: Actualizar CSS responsive**

Reemplazar el bloque @media (max-width: 600px) con reglas para:
- .app-header: padding reducido, radius reducido
- .hero-glow: display none
- .sky-toggle: --toggle-size 16px, top 8px right 8px
- .header-logo: 36x36, font-size 16px, border-radius 10px
- .stepper-text: display none
- .stepper-num: 24x24, font-size 11px
- .stepper-line: margin 0 8px
- .style-card: width 140px
- .style-card-thumb: height 75px
- .refine-grid: 1 columna
- .config-strip: flex-wrap
- .refine-config: flex-wrap, gap 10px

- [ ] **Step 2: Añadir skip link HTML y CSS**

HTML: Añadir como primer hijo de body: enlace .skip-link apuntando a #main.
Añadir id="main" al div.app-container.
CSS: .skip-link absolute top -100px, on :focus top 16px, background var(--text), color var(--bg).

- [ ] **Step 3: Añadir focus-visible y smooth scroll**

CSS:
- html: scroll-behavior smooth
- :focus-visible: outline 2px solid var(--accent), outline-offset 2px
- input/select/textarea:focus-visible: outline none (tienen sus propios estilos de focus)

- [ ] **Step 4: Verificar en el navegador**

Responsive en 600px, skip link con Tab, focus visible en pills y tarjetas.

- [ ] **Step 5: Commit**

Mensaje: "feat: add accessibility (skip link, focus-visible) and update responsive"

---

### Task 8: Limpieza final y versión

**Files:**
- Modify: `index.html` — footer, eliminar CSS muerto

- [ ] **Step 1: Actualizar versión en footer**

Cambiar "COT — IlustraCOT v1.0" por "COT — IlustraCOT v2.0".

- [ ] **Step 2: Eliminar CSS muerto**

Eliminar:
- .theme-toggle CSS (reemplazado por sky toggle)
- .section-label y .step-num CSS (reemplazados por stepper)
- .watermark CSS (ya no se genera)
- .style-text, .style-name overlay, .style-desc overlay del antiguo formato
- Todos los selectores .has-thumb del antiguo formato

- [ ] **Step 3: Verificar flujo completo**

1. Abrir index.html — fondo crema, header con logo y sky toggle
2. Stepper paso 1 activo pulsando
3. Seleccionar categoría — stepper avanza, carrusel aparece
4. Seleccionar estilo — stepper al paso 3, prompt y refine aparecen
5. Copiar prompt — botón verde, toast
6. Dark mode — todo correcto
7. Responsive 600px — stepper compacto, carrusel scrollable

- [ ] **Step 4: Commit**

Mensaje: "chore: cleanup dead CSS and bump version to 2.0"
