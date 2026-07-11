# References

Four reference docs that the agent reads before generating HTML. They contain reusable CSS patterns, library guidance, navigation components, and slide-specific patterns. The agent reads only the references needed for the current output.

## Reference overview

Four docs the agent reads before generating HTML. The agent reads only what the current output needs.

| File | Size | Purpose |
|------|------|---------|
| `css-patterns.md` | ~1200 lines | Layout, theming, components, animations, overflow protection |
| `libraries.md` | ~800 lines | Mermaid theming, Chart.js, anime.js, Google Fonts |
| `responsive-nav.md` | ~150 lines | Sticky sidebar TOC, mobile horizontal bar, scroll spy |
| `slide-patterns.md` | ~700 lines | Slide engine, 10 types, transitions, nav chrome, presets |

## `css-patterns.md`

The largest reference. Contains reusable CSS patterns organized into sections:

**Theme setup** — CSS custom properties for light/dark palettes: `--bg`, `--surface`, `--border`, `--text`, `--text-dim`, `--accent`, plus 3–5 semantic accent variables.

**Background atmosphere** — Radial glow, dot grid, diagonal lines, gradient mesh patterns for page backgrounds.

**Section/cards** — `.ve-card` with depth tiers (elevated, recessed, hero, glass). Never use `.node` as CSS class — Mermaid uses it internally.

**Code blocks** — `white-space: pre-wrap` (critical), max-height constraints, file header pattern, collapsible for full files.

**Directory tree** — `<pre>` with monospace, tree connectors, labeled cards, side-by-side comparisons.

**Overflow protection** — `min-width: 0` on grid/flex children, `overflow-wrap: break-word`, no `display: flex` on `<li>` for markers, list markers in bordered containers.

**Mermaid containers** — Full zoom/pan/expand pattern with `diagram-shell` structure, closure-based init, vector-based zoom engine, touch support, adaptive height, smart fit.

**Grid layouts** — Architecture grid (2-column with sidebar), pipeline (horizontal steps), card grid (dashboard), data tables with sticky headers and status badges.

**Connectors** — CSS arrows (vertical, horizontal), SVG curved connectors with arrowheads.

**Animations** — Staggered fade-in via `--i` variable, hover lift, scale-fade, SVG draw-in, CSS counter for hero numbers, choreography guidelines, `prefers-reduced-motion`.

**Sparklines** — Pure SVG inline visualizations, progress bars.

**Prose page elements** — Body text, lead paragraphs, pull quotes, section dividers, article heroes, author bylines, callout boxes, theme toggle, prose anti-patterns.

**Generated images** — Hero banners, inline illustrations, side accents with surf-cli integration.

## `libraries.md`

Guidance for external CDN libraries and typography:

**Mermaid.js** — CDN imports (ESM), ELK layout registration, deep theming with `theme: 'base'`, CSS overrides for node/edge text, `classDef` and `style` gotchas (never set `color:` in classDef), node label special characters, `stateDiagram-v2` label limitations, valid Mermaid writing rules, TD vs LR layout direction, diagram type selection table, dark mode handling.

**Chart.js** — CDN import, dark mode color adaptation, canvas wrapping patterns.

**anime.js** — Orchestrated animations for 10+ element diagrams, staggered reveals, path drawing, count-up numbers, `prefers-reduced-motion` respect.

**Google Fonts** — 13 curated font pairings (body + mono), forbidden fonts (Inter, Roboto, Arial, Helvetica, system-ui), typography by content voice (literary, technical, bold, minimal).

## `responsive-nav.md`

Self-contained navigation pattern for multi-section pages:

- **Desktop:** Sticky sidebar TOC with scroll spy (IntersectionObserver)
- **Mobile:** Sticky horizontal scrollable bar at top
- **Layout:** CSS Grid with `grid-template-columns: 170px 1fr`
- **JavaScript:** Scroll spy with `rootMargin: '-10% 0px -80% 0px'`, smooth scroll, mobile auto-scroll of active tab
- **Usage:** Pages with 4+ major sections. Fewer sections → skip the TOC.

## `slide-patterns.md`

Comprehensive slide deck reference containing:

**Planning process** — 5-step workflow for converting source documents to slides: inventory, map, choose layouts, plan images, verify coverage.

**Slide engine** — Scroll-snap container, 100dvh slides, CSS for `--display`, `--heading`, `--body`, `--label`, `--subtitle`.

**Transitions** — Cinematic entrance (fade + lift + scale), staggered child reveals via IntersectionObserver, `prefers-reduced-motion`.

**Nav chrome** — Progress bar, nav dots, slide counter, keyboard hints with auto-fade, backdrop blur for mixed backgrounds.

**SlideEngine class** — Keyboard/touch/wheel navigation, event delegation for interactive content.

**autoFit()** — Post-render safety net for Mermaid SVGs, KPI values, blockquotes.

**10 slide types:** Title, Section Divider, Content, Split, Diagram, CSS Pipeline, Dashboard, Table, Code, Quote, Full-Bleed.

**Decorative SVGs** — Corner accents, section divider marks, geometric background patterns.

**Proactive imagery** — surf-cli integration for generating title/full-bleed images, prompt craft guidance.

**Content density limits** — Max content per slide type (5–6 bullets, 8 table rows, 10 code lines, ~150 char quotes).

**Responsive breakpoints** — Height-based scaling (700px, 600px, 500px) for projection and small viewports.

**4 curated presets:** Midnight Editorial, Warm Signal, Terminal Mono, Swiss Clean — each with full light/dark palette CSS.
