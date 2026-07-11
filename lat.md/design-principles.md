# Design Principles

The philosophy behind visual-explainer: what makes good visual output, what to avoid, and the quality checks that catch problems before delivery.

## Core principle

Every coding agent defaults to ASCII art. This skill replaces that with real typography, dark/light themes, interactive diagrams, and curated aesthetics. The goal is output that is genuinely easier to read than the terminal alternative.

## Anti-slop guardrails

The skill explicitly forbids patterns that produce generic AI-looking output:

**Forbidden fonts** (as `--font-body`): Inter, Roboto, Arial, Helvetica, system-ui alone. These signal zero design intent.

**Forbidden colors:** Indigo/violet Tailwind defaults (`#8b5cf6`, `#7c3aed`, `#a78bfa`, `#d946ef`). The cyan+magenta+purple neon dashboard combination. Gradient-mesh blobs.

**Forbidden effects:** Gradient text on headings, animated glowing box-shadows, emoji section headers, continuous glow/pulse/breathing effects on static content.

**Forbidden in Mermaid themeVariables:** `#8b5cf6`, `#7c3aed`, `#a78bfa` (indigo/violet), `#d946ef` (fuchsia). Use teal, slate, amber, emerald, or palette-matched colors.

## Aesthetic categories

**Constrained** (safer, less room for generic output): Terminal, blueprint, paper/ink, IDE-inspired, data-dense.

**Flexible** (use with caution): Editorial, sketch, monochrome. These have more freedom and require stronger design choices to avoid slop.

## Typography guidance

Rotate across 13 curated font pairings. Never use the same pairing twice in a row. Minimum: body + mono. Optionally add a display font.

| Pairing | Feel | Best for |
|---------|------|----------|
| DM Sans + Fira Code | Friendly, developer | Blueprint, technical docs |
| Instrument Serif + JetBrains Mono | Editorial, refined | Plan reviews, decision logs |
| IBM Plex Sans + IBM Plex Mono | Reliable, readable | Architecture diagrams |
| Bricolage Grotesque + Fragment Mono | Bold, characterful | Data tables, dashboards |
| Plus Jakarta Sans + Azeret Mono | Rounded, approachable | Status reports, audits |
| Outfit + Space Mono | Clean geometric | Flowcharts, pipelines |

Plus 7 more in [[references]].

## Color guidance

Good accent directions: terracotta+sage, teal+slate, rose+cranberry, amber+emerald, deep blue+gold.

Every page must define both light and dark palettes via CSS custom properties. Start with whichever fits the aesthetic; ensure both work.

## The Slop Test

Seven-point checklist before delivery:

1. Swap dark ↔ light mode — does it still look intentional?
2. Check font choice — would it pass as a designed page, not an AI default?
3. Check color palette — no violet/indigo defaults sneaking in?
4. Check effects — no gradient text, no glowing box-shadows?
5. Squint test — does the visual hierarchy survive blurry vision?
6. Overflow check — any horizontal scroll at normal desktop width?
7. Compare against a generic dark/violet template — would you tell them apart?

## Mermaid invariants

Every Mermaid diagram must have:

- `theme: 'base'` with custom `themeVariables` matching the page palette
- The canonical `diagram-shell` pattern (not bare `<pre class="mermaid">`)
- Zoom in/out/reset/expand controls
- Ctrl/Cmd+scroll zoom, drag panning, click-to-expand
- `flowchart TD` for complex diagrams (5+ nodes). `LR` only for simple 3–4 node linear flows
- `<br/>` in quoted labels (not escaped `\n`)
- Max 10–12 nodes per diagram. Beyond that, use hybrid overview + CSS cards

## Layout invariants

Structural rules for HTML output that prevent overflow and ensure accessibility.

- Semantic HTML where it helps: `<table>`, headings, lists, `<details>`, captions
- CSS custom properties for palette
- Clear aesthetic direction before writing
- Prevent overflow: `min-width: 0` on grid/flex children, `overflow-wrap: break-word`
- No `display: flex` on `<li>` when list markers matter
- Depth used sparingly: hero/elevated only for primary sections
- Entrance/hover animation only when it clarifies hierarchy

## Presentation readability (slides)

Slide-specific readability rules for projection and screen sharing.

- Minimum body text: 16px
- One focal point per slide
- Higher contrast than pages
- Nav chrome visible on any background
- Simpler Mermaid diagrams (max 8–10 nodes, 18px+ labels)

## Auto-detection

The agent automatically kicks in for complex tables — 4+ rows or 3+ columns triggers HTML rendering instead of terminal output. The agent gives a short chat summary and renders the full table as HTML.

## Quality checklist (from SKILL.md)

Before delivery, verify:

- Complete HTML document
- Output written to requested path
- No console errors when opened
- No horizontal overflow at normal desktop width
- Fonts load with fallbacks
- Tables preserve rows/columns and wrap long text
- Mermaid diagrams use `diagram-shell` with zoom/pan/expand
- Slides fit one viewport, include carousel dots, preserve source coverage
- Visual hierarchy makes the main idea obvious in the first viewport
- Styling would still be recognizable against a generic dark/violet template
