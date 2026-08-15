# visual-explainer

Generate self-contained HTML pages that turn complex terminal output into styled visual explanations — diagrams, diff reviews, plan reviews, slide decks, data tables. Works across six coding agent harnesses.

## Core concept

Replaces ASCII art and pipe tables with real typography, dark/light themes, and interactive Mermaid diagrams.

- [[architecture]] — file structure, how pieces fit together
- [[harnesses]] — multi-harness support matrix and installation per harness
- [[commands]] — each slash command with purpose, usage, and options
- [[templates]] — HTML template system with four reference palettes
- [[references]] — agent-read reference docs for CSS, libraries, navigation, slides
- [[extension]] — Pi native tool (`visual_explainer`) with prepare and render actions
- [[slides]] — slide deck mode, the SlideEngine, and four curated presets
- [[installation]] — all installation methods across harnesses
- [[design-principles]] — anti-slop guardrails, typography, color, and quality checks
- [[tests]] — test specs for the skill and extension
- [[last-commit]] — most recent tracked commit hash for documentation sync

## Quick start

Fastest path to visual output for each supported harness.

```bash
# Pi (preferred)
pi install git:github.com/nicobailon/visual-explainer

# Claude Code (marketplace)
/plugin marketplace add nicobailon/visual-explainer
/plugin install visual-explainer@visual-explainer-marketplace

# Legacy Pi installer (skill + prompts only, no native tool)
curl -fsSL https://raw.githubusercontent.com/nicobailon/visual-explainer/main/install-pi.sh | bash
```

## Output

All harnesses write generated HTML to `~/.agent/diagrams/` and open it in the browser. The pages are fully self-contained — embedded CSS, inline JS, no external assets except optional CDN imports for Mermaid and Chart.js.
