# Architecture

How the visual-explainer skill is structured: a core skill definition, seven command templates, five reference docs, four HTML templates, a Pi extension, and a Claude Code marketplace plugin.

## File tree

Complete project file structure showing all source and config files.

```
.claude-plugin/
  marketplace.json          ← marketplace catalog (discovery)
  plugin.json               ← top-level plugin identity
configs/
  codex/AGENTS.md           ← Codex CLI guidance
  cursor/visual-explainer.mdc ← Cursor rules-based guidance
  openclaw/AGENTS.md        ← OpenClaw guidance
  opencode/AGENTS.md        ← OpenCode guidance
  pi/AGENTS.md              ← Pi guidance
plugins/visual-explainer/
  .claude-plugin/plugin.json ← plugin manifest (marketplace)
  SKILL.md                   ← core skill definition + design principles
  extension.ts               ← Pi native tool (prepare, render, render_quick)
  commands/                  ← slash command templates (7 commands)
  references/                ← agent-read docs before generating
    css-patterns.md          ← layouts, animations, theming
    libraries.md             ← Mermaid, Chart.js, fonts
    responsive-nav.md        ← sticky TOC for multi-section pages
    slide-patterns.md        ← slide engine, transitions, presets
    themes.md                ← 11 prebuilt palettes + runtime picker
  templates/                 ← reference HTML templates with distinct palettes
    architecture.html        ← CSS Grid cards, terracotta/sage
    mermaid-flowchart.html   ← Mermaid + ELK, teal/cyan
    data-table.html          ← HTML table + KPIs, rose/cranberry
    slide-deck.html          ← 10 slide types, Midnight Editorial
  mcp/
    README.md                ← MCP server docs
    server.mjs               ← stdio MCP server implementation
  quick/
    README.md                ← quick renderer docs
    base.css                 ← shared styles for quick-mode output
    render.mjs               ← validates JSON spec → HTML
    schema.json              ← authoritative quick-mode JSON Schema
install-pi.sh                ← legacy Pi installer (copies skill + prompts)
package.json                 ← npm metadata, pi config
README.md                    ← overview + install matrix
CHANGELOG.md                 ← version history
```

## Data flow

```
User prompt or slash command
  → Agent reads SKILL.md (routing rules, design principles)
  → Agent reads relevant references (css-patterns, libraries, etc.)
  → Agent reads relevant template (architecture, mermaid, data-table, slides)
  → Agent generates self-contained HTML
  → Writes to ~/.agent/diagrams/
  → Opens in browser
```

For Pi native installs, the extension adds a two-step flow:

```
visual_explainer(action="prepare")   → planning + optional subagent scouting
  → Agent gathers context, reads references, generates HTML
visual_explainer(action="render")    → writes HTML + opens browser
```

## Plugin system

Two layers of Claude Code integration exist:

1. **Top-level** (`.claude-plugin/`) — marketplace discovery. `marketplace.json` lists the plugin for `/plugin marketplace add`. `plugin.json` is the top-level identity.

2. **Inner plugin** (`plugins/visual-explainer/.claude-plugin/plugin.json`) — the actual installable plugin. Claude Code loads skill definitions, commands, and the reference/template files from here.

The marketplace namespaces commands as `/visual-explainer:command-name`.

## Pi package structure

`package.json` contains a `pi` field that advertises three resource types:

```json
{
  "pi": {
    "extensions": ["./plugins/visual-explainer/extension.ts"],
    "skills": ["./plugins/visual-explainer"],
    "prompts": ["./plugins/visual-explainer/commands"]
  }
}
```

- **extensions** — the `visual_explainer` tool (prepare, render, render_quick)
- **skills** — the `SKILL.md` with routing rules and design principles
- **prompts** — the seven command template markdown files

## MCP server

The `mcp/server.mjs` is a stdio-only MCP (Model Context Protocol) server. It is meant for MCP hosts that launch a local child process. It does not start an HTTP listener, handle credentials, call an LLM, or store output outside the local machine.

**Exposed tools:**
- `visual_explainer_prepare` — returns a recommended visual explanation flow
- `visual_explainer_render_html` — validates a complete HTML document and writes it to `~/.agent/diagrams/`
- `visual_explainer_render_quick` — validates a quick-mode JSON spec and writes rendered HTML

**Exposed prompts:** the seven command templates as MCP prompts (`generate-web-diagram`, `generate-visual-plan`, `generate-slides`, `diff-review`, `plan-review`, `project-recap`, `fact-check`).

**Exposed resources:** read-only access to `SKILL.md`, command templates, quick README, and quick schema JSON.

Rendered files are written only to `~/.agent/diagrams/`. Filenames must be basenames. Paths, traversal, control characters, and symlink targets are rejected.

## Quick render system

Compact JSON-to-HTML renderer for explicit `--quick` prompts. The agent emits a JSON spec; `render.mjs` validates it and produces complete self-contained HTML.

**Intended use:** `/generate-web-diagram --quick`, `/diff-review --quick`, `/plan-review --quick`, or `/project-recap --quick`. If the spec does not fit or rendering fails, fall back to the full HTML workflow.

**Components:**
- `quick/schema.json` — authoritative JSON Schema for the spec
- `quick/render.mjs` — validates the spec and renders HTML
- `quick/base.css` — shared styles for quick-mode output
- `quick/README.md` — usage guide for Pi and other harnesses

**Pi integration:** the extension's `render_quick` action accepts a `spec` object and calls `renderQuickSpec(spec)` to produce HTML, then writes it through the same path as `render`.

## Template palette strategy

Each template uses a distinct color palette to teach agents visual variety:

| Template | Palette | Fonts |
|----------|---------|-------|
| architecture.html | Terracotta + sage | IBM Plex Sans + Mono |
| mermaid-flowchart.html | Teal + cyan | Bricolage Grotesque + Fragment Mono |
| data-table.html | Rose + cranberry | Instrument Serif + JetBrains Mono |
| slide-deck.html | Midnight Editorial (navy + gold) | Instrument Serif + JetBrains Mono |

This is intentional — agents that see only one palette learn to repeat it. Four distinct palettes break that pattern.
