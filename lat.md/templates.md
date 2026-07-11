# Templates

Four reference HTML templates that demonstrate different visual approaches. Each uses a distinct palette, font pairing, and layout strategy — agents read the relevant template before generating to absorb patterns, not copy verbatim.

## Template overview

Four reference HTML templates, each with a distinct palette to teach agents visual variety.

| Template | Purpose | Palette | Layout |
|----------|---------|---------|--------|
| `architecture.html` | CSS Grid card architecture diagrams | Terracotta + sage | Vertical sections, pipeline, inner grids |
| `mermaid-flowchart.html` | Mermaid diagrams with ELK layout | Teal + cyan | Single diagram + zoom controls |
| `data-table.html` | HTML tables with KPI summary cards | Rose + cranberry | Sticky header table + collapsible details |
| `slide-deck.html` | Full slide deck with 10 slide types | Midnight Editorial (navy + gold) | 100dvh scroll-snap slides |

## `architecture.html`

Demonstrates CSS Grid architecture layouts for text-heavy system overviews.

**Patterns shown:**

- Warm non-default palette (terracotta + sage, not violet/indigo)
- Depth tiers: hero (input sources), default (mid sections), recessed (callout)
- Asymmetric background atmosphere (off-center gradient mesh)
- Section cards with colored accent borders and dot labels
- Vertical flow arrows between sections (inline SVG)
- Horizontal pipeline with step boxes and arrow separators
- Parallel branch within a pipeline
- Color-coded legend, three-column output row
- Staggered fade-in via `--i` CSS variable
- Reduced motion respect, responsive single-column fallback

**When to read this:** Text-heavy architecture, module internals, implementation plans with card-based layouts.

## `mermaid-flowchart.html`

Demonstrates Mermaid diagram integration with the canonical zoom/pan/expand engine.

**Patterns shown:**

- Teal/cyan palette (distinct from other templates)
- Dot-grid background atmosphere
- ESM import of Mermaid + `@mermaid-js/layout-elk`
- Mermaid theme: `'base'` + full `themeVariables`, `fontSize: 16px`
- CSS overrides: `.nodeLabel` 16px, `.edgeLabel` 13px
- `look: 'classic'` for clean lines
- `layout: 'elk'` for better node positioning
- Vector-based zoom/pan engine (not CSS zoom)
- `diagram-shell` > `.mermaid-wrap` > `.mermaid-viewport` > `.mermaid-canvas` structure
- Closure-based `initDiagram(shell)` pattern — per-diagram state in closures
- Source Mermaid code in `<script type="text/plain" class="diagram-source">`
- Click-to-expand (opens full-size in new tab)
- Touch pinch-to-zoom support
- Adaptive viewport height based on diagram aspect ratio
- Smart fit algorithm with readability floor

**When to read this:** Any Mermaid diagram — flowcharts, sequence diagrams, ER diagrams, state machines, class diagrams, C4 architecture.

## `data-table.html`

Demonstrates HTML `<table>` elements with KPI summary cards and status indicators.

**Patterns shown:**

- Rose/cranberry palette (distinct from other templates)
- KPI summary cards above the table (visual hook before data)
- Real `<table>` with sticky header, alternating rows
- Status indicator badges (match, gap, partial) — never emoji
- Text wrapping in wide columns, code references in cells
- Summary footer row with aggregate status
- Collapsible `<details>` section for gap analysis
- Staggered row animation via `--i` variable
- Responsive horizontal scroll wrapper

**When to read this:** Data tables, comparison matrices, audit results, requirements reviews.

## `slide-deck.html`

Demonstrates all 10 slide types in a cohesive narrative using the Midnight Editorial preset.

**Patterns shown:**

- All 10 slide types: Title, Section Divider, Content, Split, Diagram, Dashboard, Table, Code, Quote, Full-Bleed
- SlideEngine JS: keyboard/touch/wheel navigation, progress bar, dots, counter, hints
- Cinematic transitions: fade + translateY + scale, staggered child reveals via IntersectionObserver
- Per-slide background variation (gradient direction, accent glow position)
- Decorative SVG accents (corner marks, quote mark, divider)
- Typography scale: 120px display → 48px heading → 22px body → 14px label
- Compositional variety: centered, left-heavy, split, full-bleed
- Mermaid at presentation scale (18px labels, 2px edges)
- Nav chrome with backdrop blur for mixed-background visibility
- Event delegation: Mermaid zoom and table scroll don't trigger slide navigation
- Responsive height breakpoints for projection and small viewports
- `autoFit()` post-render function for Mermaid SVGs, KPI values, and blockquotes
- `prefers-reduced-motion` respected

**When to read this:** Slide decks, presentations, any content using `--slides` flag.

## Agent routing

The SKILL.md contains a routing table that tells the agent which template and references to read based on the content type:

| Content need | Read |
|-------------|------|
| Text-heavy architecture/cards | `architecture.html` |
| Mermaid flowcharts, sequence, ER, state, class, C4 | `mermaid-flowchart.html` + Mermaid sections in `libraries.md` |
| Data tables, comparisons, audits | `data-table.html` |
| Slide decks | `slide-deck.html` + `slide-patterns.md` |
| CSS layout, overflow, depth, collapsibles | `css-patterns.md` |
| Pages with 4+ sections | `responsive-nav.md` |
| Prose-heavy pages | "Prose Page Elements" in `css-patterns.md` |
