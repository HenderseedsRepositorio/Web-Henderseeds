# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

HenderSeeds SRL website — distribuidor exclusivo RED.IN de Nidera Semillas en Henderson, Bolívar y Daireaux (Buenos Aires, Argentina).

## Stack

Single-file static site: everything lives in `index.html` (~1600 lines: HTML + inline CSS + inline JS). No framework, bundler, or build step.

## Commands

```bash
npx serve .          # Dev server local (puerto 3000)
```

## Deploy

- **Repo:** github.com/HenderseedsRepositorio/Web-Henderseeds
- **Hosting:** Vercel — auto-deploy on push to `main`
- **URL:** https://henderseeds.com (Vercel, dominio custom via GoDaddy DNS)

## Architecture (index.html layout)

The file is organized in three blocks:

1. **`<style>` (lines ~19–1274)** — All CSS, using custom properties defined in `:root`. Sections are marked with `/* ═══ SECTION ═══ */` comments. Responsive breakpoints use `@media (max-width: 768px)`.

2. **`<body>` HTML (lines ~1275–1275)** — Semantic sections in order:
   Nav → Loader → Hero slider (2 slides + particle canvas) → Ticker híbridos → Nosotros → Portfolio (Maíz/Girasol tabs) → Servicios → Blog → Contacto (with form) → Footer → Bot Semillero floating widget

3. **`<script>` (lines ~1276–end)** — Vanilla JS, no dependencies. Key modules:
   - **Loader** — hides splash on `window.load`
   - **Particle canvas** — animated background on hero (desktop only, `<canvas>`)
   - **Hero slider** — auto-rotating with touch/keyboard support (`goToSlide`, `nextSlide`, `prevSlide`)
   - **Scroll behavior** — navbar style change on scroll
   - **Portfolio tabs** — `showTab(type, btn)` switches Maíz/Girasol cards
   - **Contact form** — `handleSubmit(e)` posts to Formspree
   - **Theme toggle** — `toggleTheme()` light/dark mode
   - **Bot Semillero** — floating chatbot widget with keyword-based responses (`sendBotMsg`, `addMsg`)

## Design Tokens (CSS custom properties in :root)

- Navy: `--navy: #0D2D5E`, `--navy-dark: #091E3E`, `--navy-mid: #1A4A8A`, `--navy-light: #14396E`
- Accent: `--orange: #F5A623`
- Fonts: `--arch` (Archivo Black → títulos), `--sans` (DM Sans → cuerpo), `--mono` (DM Mono)
- Radii: `--radius-sm` through `--radius-xl`
- Shadows: `--shadow-sm`, `--shadow-md`, `--shadow-lg`, `--shadow-glow`

## Key Files

```
index.html                  ← The entire site (HTML + CSS + JS)
DATOS-TECNICOS.txt          ← Seed hybrid specs & prices (source of truth)
assets/logos/henderseeds.png
assets/logos/nidera.png
assets/ensayo-campo.jpeg
assets/equipo-ensayo.jpeg
```

## Content Rules

- **NO** usar "empresa familiar". Posicionar como empresa profesional de productos y servicios al productor.
- Mensajes simples y directos, no complejos.
- Precios y datos técnicos de híbridos: SIEMPRE tomar de `DATOS-TECNICOS.txt`, nunca inventar.
- "Potencial de rinde" es orientativo — usar "en ensayos" o "potencial", nunca prometer.
