# Architecture

How the visual-explainer skill is structured: a core skill definition, seven command templates, four reference docs, four HTML templates, a Pi extension, and a Claude Code marketplace plugin.

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
  extension.ts               ← Pi native tool (prepare + render)
  commands/                  ← slash command templates (7 commands)
  references/                ← agent-read docs before generating
    css-patterns.md          ← layouts, animations, theming
    libraries.md             ← Mermaid, Chart.js, fonts
    responsive-nav.md        ← sticky TOC for multi-section pages
    slide-patterns.md        ← slide engine, transitions, presets
  templates/                 ← reference HTML templates with distinct palettes
    architecture.html        ← CSS Grid cards, terracotta/sage
    mermaid-flowchart.html   ← Mermaid + ELK, teal/cyan
    data-table.html          ← HTML table + KPIs, rose/cranberry
    slide-deck.html          ← 10 slide types, Midnight Editorial
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

- **extensions** — the `visual_explainer` tool (prepare + render)
- **skills** — the `SKILL.md` with routing rules and design principles
- **prompts** — the seven command template markdown files

## Template palette strategy

Each template uses a distinct color palette to teach agents visual variety:

| Template | Palette | Fonts |
|----------|---------|-------|
| architecture.html | Terracotta + sage | IBM Plex Sans + Mono |
| mermaid-flowchart.html | Teal + cyan | Bricolage Grotesque + Fragment Mono |
| data-table.html | Rose + cranberry | Instrument Serif + JetBrains Mono |
| slide-deck.html | Midnight Editorial (navy + gold) | Instrument Serif + JetBrains Mono |

This is intentional — agents that see only one palette learn to repeat it. Four distinct palettes break that pattern.
