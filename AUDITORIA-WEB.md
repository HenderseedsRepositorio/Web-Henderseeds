# Auditoría Web henderseeds.com — 12 abril 2026

**Nota global: 7.2 / 10**

## Estado de mejoras

| # | Problema | Prioridad | Estado |
|---|---------|-----------|--------|
| 1 | Hero usa 3 fotos stock Pexels en vez de fotos propias | P0 | PENDIENTE |
| 2 | Blog usa imágenes Unsplash y linka a sitios externos, sin contenido propio | P2 | HECHO (3 notas propias con data real, sin Unsplash, sin links externos) |
| 3 | Imágenes locales sin optimizar (ensayo-campo 2.7MB, equipo-ensayo 2.3MB) | P0 | PENDIENTE |
| 4 | Cero testimonios ni prueba social de productores | P1 | PENDIENTE |
| 5 | Google Analytics desactivado (GA4 comentado con placeholder) | P0 | HECHO (G-GGC7FRWF6G) |
| 6 | SeedBot con UX débil (sin onboarding, sin fuzzy match, botón chico en mobile) | P1 | HECHO (quick replies, fuzzy, categorías, botón grande) |
| 7 | Accesibilidad débil (sin ARIA, font 9px en mobile, sin prefers-reduced-motion) | P1 | PENDIENTE |
| 8 | Formulario sin validación visual ni feedback claro | P2 | PENDIENTE |
| 9 | SEO con base sólida pero sin contenido indexable ni schema Product | P2 | PENDIENTE |
| 10 | Footer minimalista, falta badge RED.IN, datos de confianza, mapa zona | P3 | HECHO (badge RED.IN, zona cobertura, "Desde 2023") |

## Detalle de cada punto

### 1. Hero con fotos stock (P0)
Las 3 fotos del hero slider son de Pexels (stock gratuito). Reemplazar con fotos propias de lotes en la zona, jornadas a campo, o el equipo trabajando.

### 2. Blog sin contenido propio (P2) — HECHO
Mejoras implementadas el 12/04/2026:
- 3 notas propias con contenido original de HenderSeeds (sin links externos, sin Unsplash)
- Nota 1: "NS 7765: 9.960 kg/ha y 7% arriba del testigo" — datos de ensayo Zona Núcleo, perfil técnico, argumento comercial
- Nota 2: "Girasol alto oleico: hasta USD 100/tn más" — análisis de mercado, comparativa NS 1227 vs NS 1113, cuenta rápida de rentabilidad
- Nota 3: "Nidera en Expoagro 2026: 5 lanzamientos" — NS 7852, NS 7925, NS 1117 CL con posicionamiento y disponibilidad
- Thumbnails con gradientes SVG branded (sin fotos stock)
- Modal de lectura completa inline (sin navegar fuera del sitio)
- Evento gtag `blog_read` para tracking en Analytics

### 3. Imágenes sin optimizar (P0)
- ensayo-campo.jpeg = 2.7 MB
- equipo-ensayo.jpeg = 2.3 MB
Comprimir a WebP, máximo 200 KB cada una. Usar srcset para mobile.

### 4. Sin testimonios ni prueba social (P1)
Agregar sección de testimonios con 3-4 quotes de productores reales (nombre, localidad, cultivo). Números tipo "Más de 15.000 ha asesoradas".

### 5. Google Analytics desactivado (P0) — HECHO
Activado con G-GGC7FRWF6G el 12/04/2026. 5 eventos de conversión: whatsapp_click, form_submit, portfolio_view, bot_open, hybrid_view.

### 6. SeedBot UX débil (P1) — HECHO
Mejoras implementadas el 12/04/2026:
- Quick-reply buttons al abrir (NS 7765, NS 1113, NS 7852, NS 1227 HO, Maíz, Girasol)
- Búsqueda por categoría (maíz, girasol, clearfield, oleico, temprana, silaje)
- Fuzzy matching para typos (1 dígito de tolerancia)
- Respuestas a saludos, precios, ubicación
- Botón mobile más grande (44-48px, opacity .9)

### 7. Accesibilidad débil (P1)
- Agregar aria-label a botones y links interactivos
- Subir font-size mínimo mono a 12px en mobile
- Agregar @media (prefers-reduced-motion: reduce) para desactivar animaciones
- Corregir jerarquía headings (h2 → h4 sin h3)

### 8. Formulario sin validación (P2)
- Agregar validación inline con mensajes de error visibles
- Extender estado de éxito a 5-7 segundos
- Feedback visual en campos requeridos

### 9. SEO sin contenido ni schema Product (P2)
- Agregar schema Product (JSON-LD) para cada híbrido
- Crear contenido indexable propio
- Optimizar keywords long-tail locales ("semillas maíz Henderson", "girasol CL zona sur Buenos Aires")

### 10. Footer minimalista (P3)
- Agregar badge "Distribuidor exclusivo RED.IN"
- Año de fundación o dato de confianza
- Mini-mapa o lista de localidades cubiertas
- Newsletter signup (opcional)
