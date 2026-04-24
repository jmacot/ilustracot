# CLAUDE.md

Este archivo orienta a Claude Code (claude.ai/code) para trabajar con este repositorio.

## Proyecto

IlustraCOT es un generador de prompts para ilustraciones médicas en COT. Crea prompts optimizados para generadores de imágenes IA (Gemini, DALL-E, Midjourney) con estilos visuales profesionales.

## Arquitectura

**App single-file HTML** (~870 líneas) — todo el CSS en `<style>`, todo el JS en `<script>`, sin build tools ni frameworks. Solo vanilla JS. Sigue la convención del proyecto padre (ver `../CLAUDE.md`).

### Sistema de diseño propio (ni A ni B)

Usa su propia paleta **"Studio Creativo"**, distinta de los Sistemas A/B:
- Fuentes: Inter + Lora (Google Fonts CDN)
- Light: cream `#f8f6f3`, dark: `#111827`
- Acento: sky blue `#0ea5e9` / gradiente índigo
- Header: gradiente azul oscuro sólido `#0f2240 → #1a3a5c → #234e77` con dot grid + glows
- Dark mode: `[data-theme="dark"]` con sky toggle CSS puro (basado en checkbox, no botón JS)

### Flujo de la app (stepper de 4 pasos)

1. **Categoría** — pills de categoría (cirugía, endo, radio) disparan `selectCat()`
2. **Estilo** — carrusel horizontal renderizado por `renderStyles()`, cards dinámicas
3. **Prompt** — panel de resultado con config strip + textarea de prompt, botón copiar vía `copyMain()`
4. **Refinar** — grid de chips de refinamiento renderizado por `renderRefine()`

### Funciones JS clave

- `selectCat(cat, el)` — selecciona categoría, renderiza estilos, avanza stepper
- `renderStyles(cat)` — construye cards de estilo en carrusel desde el objeto `STYLES`
- `selectStyle(key, el)` — selecciona estilo, muestra panel con prompt generado
- `renderRefine(cat, style)` — construye chips de sugerencias de refinamiento
- `copyMain()` / `copyRefine(text, el)` — copiar al portapapeles con toast de feedback
- `advanceStepper(step)` — actualiza UI del stepper al paso dado
- `applyTheme(dark)` — alterna dark/light mode, actualiza CSS vars y meta theme-color

### Estructura de datos

El objeto `STYLES` es el dato central — indexado por ID de estilo, cada entrada contiene:
- `name`, `cat` (categoría), `thumb` (CSS gradiente para card), `icon` (SVG path)
- `prompt` (texto completo del prompt IA)
- `config` (configuración del modelo: nombre, temperature, topP, topK)

## Desarrollo

Sin build step. Abrir `index.html` en navegador o servir localmente:
```bash
npx serve -l 8089
```

## Deploy

GitHub Pages desde rama `main`, raíz `/`. Push a `main` = deploy.

## Convenciones

- Idioma: `lang="es"` siempre, todo el texto UI en español
- Dark mode vía custom properties CSS `[data-theme="dark"]` — nunca overrides de color inline
- Accesibilidad: skip link, `focus-visible`, roles ARIA en elementos interactivos, `prefers-reduced-motion`
- Responsive: mobile-first breakpoint a 600px, hero glows ocultos en móvil
- Notificaciones toast para feedback de copia (purple `#7c3aed`)
- Tema persiste en `localStorage` con expiración diaria (resetea a auto-detect cada día)

## Estructura de archivos

```
index.html          ← app completa (CSS + HTML + JS)
assets/             ← imágenes de muestra de estilos
  cirugia-abierta/  ← muestras estilo cirugía (4 PNGs)
  radiologia/       ← muestras estilo radiología (1 PNG)
docs/               ← docs de análisis y specs de diseño
```
