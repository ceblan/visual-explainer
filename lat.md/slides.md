# Slides

Slide deck mode: magazine-quality HTML presentations as self-contained files. Activated via `/generate-slides` or `--slides` flag on any scrollable-page command.

## When to use slides

Only when explicitly requested — `/generate-slides`, `--slides` flag, or natural language like "as a slide deck." Never auto-select slide format. Slides are a different medium, not a paginated article.

## Slide engine

Each slide is exactly one viewport (`100dvh`) with scroll-snap. No page-level scrolling within a slide.

```html
<div class="deck">
  <section class="slide slide--title"> ... </section>
  <section class="slide slide--content"> ... </section>
</div>
```

**SlideEngine class** handles:
- Keyboard navigation (arrows, space, PageUp/Down, Home/End)
- Touch swipe navigation
- Scroll-snap alignment
- Progress bar, nav dots, slide counter
- Keyboard hints with auto-fade (4 seconds)
- IntersectionObserver for cinematic entrance animations
- Event delegation: Mermaid zoom, scrollable code, table scroll don't trigger slide navigation

## 10 slide types

Each type has a defined HTML structure and content density limit.

| Type | Max content | Layout |
|------|------------|--------|
| Title | 1 heading + 1 subtitle | Centered hero, 80–120px display type |
| Section Divider | 1 number + heading + optional subhead | Oversized decorative number (200px+) |
| Content | 1 heading + 5–6 bullets | Asymmetric grid (text + aside) |
| Split | 1 heading + 2 panels | 60/40 or 70/30 two-panel |
| Diagram | 1 heading + 1 Mermaid (max 8–10 nodes) | Full-viewport with zoom controls |
| CSS Pipeline | 1 heading + 5–6 step cards | Flex pipeline with arrows (for linear flows) |
| Dashboard | 1 heading + 6 KPI cards | Grid of hero numbers |
| Table | 1 heading + 8 rows | Scrollable table (overflow paginates to next slide) |
| Code | 1 heading + 10 lines | Centered code block with filename label |
| Quote | 1 quote (~150 chars) + attribution | Centered, dramatic typography |
| Full-Bleed | 1 heading + subtitle over background | Background image/gradient with scrim |

## Typography scale

2–3× larger than scrollable pages:

| Element | Size | Weight |
|---------|------|--------|
| Display (title) | 48–120px | 800 (or 400 for serif presets) |
| Section numbers | 100–260px | 200 (decorative) |
| Headings | 28–48px | 700 |
| Body/bullets | 16–24px | 400 |
| Code | 14–18px | 400 (mono) |
| Quotes | 24–48px | 400 (serif italic) |
| Labels | 10–14px | 600 (mono, uppercase) |

## Planning process

Before writing HTML, the agent must:

1. **Inventory source** — enumerate every section, card, table row, decision, collapsible detail
2. **Map to slides** — every inventory item gets slide real estate (don't drop content to fit)
3. **Choose layouts** — assign slide type and spatial composition per slide
4. **Plan images** — check `which surf`, generate 2–4 images for title and full-bleed slides
5. **Verify** — reader should reconstruct every major point from slides alone

**Rule:** A source document with 7 sections typically produces 18–25 slides, not 10–13.

## autoFit

Post-render safety net called after Mermaid/Chart.js render but before SlideEngine init:

- **Mermaid SVGs** — force `width: 100%`, `height: auto` (Mermaid renders with fixed dimensions inside flex)
- **KPI values** — `transform: scale()` for text overflows
- **Blockquotes** — proportional font reduction for long quotes (0.5 floor)

## Four curated presets

Starting points with distinct palettes and font pairings. Agents riff on these — different decks with the same preset should feel distinct.

### Midnight Editorial
Deep navy, serif display (Instrument Serif), warm gold accents. Dark-first. Radial gold glow, corner marks.

### Warm Signal
Cream paper, bold sans (Plus Jakarta Sans), terracotta/coral accents. Light-first. Warm radial glow.

### Terminal Mono
Dark, monospace everything (Geist Mono), green/cyan accents, faint dot grid. Developer-native. Dark-first.

### Swiss Clean
White, geometric sans (DM Sans), single bold accent (blue), visible grid. Minimal and precise. Light-first.

## Compositional variety

Consecutive slides must vary spatial approach. Three centered slides in a row is a smell. Patterns to alternate: centered, left-heavy, right-heavy, edge-aligned, split, full-bleed.

## Responsive breakpoints

Height-based scaling for projection and small viewports:

| Breakpoint | Changes |
|-----------|---------|
| max-height: 700px | Reduced padding, smaller display text |
| max-height: 600px | Hide decorative SVGs, reduce quote padding |
| max-height: 500px | Aggressive padding reduction, hide nav dots |
| max-width: 768px | Single-column grids, hide asides |

## Content density limits

Each slide must fit in 100dvh. If content exceeds limits, split across slides — never scroll within a slide.
