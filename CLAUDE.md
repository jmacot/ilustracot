# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

IlustraCOT is a prompt generator for medical illustrations in Orthopedic Surgery (COT). It creates optimized prompts for AI image generators (Gemini, DALL-E, Midjourney) with professional visual styles. Live at: https://jmacot.github.io/ilustracot/

## Architecture

**Single-file HTML app** (~870 lines) — all CSS in `<style>`, all JS in `<script>`, no build tools, no frameworks. Vanilla JS only. This follows the parent project convention (see `../CLAUDE.md`).

### Custom Design System (neither A nor B)

This app uses its **own "Studio Creativo" palette**, distinct from the parent project's System A/B:
- Fonts: Inter + Lora (via Google Fonts CDN)
- Light: cream `#f8f6f3`, dark: `#111827`
- Accent: sky blue `#0ea5e9` / indigo gradient
- Header: solid dark blue gradient `#0f2240 → #1a3a5c → #234e77` with dot grid + glows
- Dark mode: `[data-theme="dark"]` with CSS-only sky toggle (checkbox-based, not JS button)

### App Flow (4-step stepper)

1. **Categoria** — category pills (surgery, endo, radio) trigger `selectCat()`
2. **Estilo** — horizontal carousel rendered by `renderStyles()`, cards built dynamically
3. **Prompt** — result panel with config strip + prompt textarea, copy button via `copyMain()`
4. **Refinar** — refinement chips grid rendered by `renderRefine()`

### Key JS Functions

- `selectCat(cat, el)` — selects category, renders styles, advances stepper
- `renderStyles(cat)` — builds style cards in carousel from `STYLES` data object
- `selectStyle(key, el)` — selects style, shows result panel with generated prompt
- `renderRefine(cat, style)` — builds refinement suggestion chips
- `copyMain()` / `copyRefine(text, el)` — clipboard copy with toast feedback
- `advanceStepper(step)` — updates stepper UI to given step number
- `applyTheme(dark)` — toggles dark/light mode, updates CSS vars and meta theme-color

### Data Structure

The `STYLES` object is the core data — keyed by style ID, each entry contains:
- `name`, `cat` (category), `thumb` (gradient CSS for card), `icon` (SVG path)
- `prompt` (the full AI prompt text)
- `config` (model settings: model name, temperature, topP, topK)

## Development

No build step. Open `index.html` in a browser or serve locally:
```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deployment

GitHub Pages from `main` branch, root `/`. Push to `main` = deploy.

## Conventions

- Language: `lang="es"` always, all UI text in Spanish
- Dark mode via `[data-theme="dark"]` CSS custom properties — never inline color overrides
- Accessibility: skip link, `focus-visible`, ARIA roles on interactive elements, `prefers-reduced-motion`
- Responsive: mobile-first breakpoint at 600px, hero glows hidden on mobile
- Toast notifications for copy feedback (purple `#7c3aed`)
- Theme persists in `localStorage` with daily expiry (resets to auto-detect each day)

## File Structure

```
index.html          ← entire app (CSS + HTML + JS)
assets/             ← style thumbnail images
  cirugia-abierta/  ← surgery style samples (4 PNGs)
  radiologia/       ← radiology style samples (1 PNG)
docs/               ← analysis docs and design specs
```
