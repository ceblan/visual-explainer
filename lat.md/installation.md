# Installation

All installation methods for visual-explainer across six harnesses. Each harness has a different install path and capability set.

## Pi (preferred)

**Package install** — installs skill, commands, and native `visual_explainer` tool:

```bash
pi install git:github.com/nicobailon/visual-explainer
```

From local checkout:

```bash
pi install ./visual-explainer
```

**Legacy installer** — copies skill + prompts only, no native tool:

```bash
curl -fsSL https://raw.githubusercontent.com/nicobailon/visual-explainer/main/install-pi.sh | bash
```

**Migration from legacy to package:** Remove old copied files before `pi install` to avoid shadow conflicts:

```bash
rm -rf ~/.pi/agent/skills/visual-explainer
rm -f ~/.pi/agent/prompts/{diff-review,fact-check,generate-slides,generate-visual-plan,generate-web-diagram,plan-review,project-recap}.md
rm -f ~/.pi/agent/prompts/s[h]are*.md
```

**What gets installed:**

| Resource | Path |
|----------|------|
| Skill (SKILL.md) | `./plugins/visual-explainer/` (via package) |
| Commands | `./plugins/visual-explainer/commands/` (via package) |
| Extension (visual_explainer tool) | `./plugins/visual-explainer/extension.ts` (via package) |

## Claude Code (marketplace)

```bash
/plugin marketplace add nicobailon/visual-explainer
/plugin install visual-explainer@visual-explainer-marketplace
```

**Commands are namespaced:** `/visual-explainer:generate-web-diagram`, `/visual-explainer:diff-review`, etc.

**Plugin identity:** Two `plugin.json` files — `.claude-plugin/marketplace.json` for discovery, `plugins/visual-explainer/.claude-plugin/plugin.json` for the installable plugin.

## Codex CLI

Manual copy. No marketplace, no native tool.

```bash
git clone --depth 1 https://github.com/nicobailon/visual-explainer.git /tmp/visual-explainer
mkdir -p ~/.codex/skills ~/.codex/prompts
cp -R /tmp/visual-explainer/plugins/visual-explainer ~/.codex/skills/visual-explainer
cp /tmp/visual-explainer/plugins/visual-explainer/commands/*.md ~/.codex/prompts/
rm -rf /tmp/visual-explainer
```

**Invoke:** `$visual-explainer` or ask Codex to use the skill. Commands may work as `/prompts:diff-review` depending on build. Prompt templates are optional — the skill works without them.

## OpenCode

Manual copy to observed native skill path.

```bash
git clone --depth 1 https://github.com/nicobailon/visual-explainer.git /tmp/visual-explainer
mkdir -p ~/.config/opencode/skill ~/.config/opencode/command
cp -R /tmp/visual-explainer/plugins/visual-explainer ~/.config/opencode/skill/visual-explainer
cp /tmp/visual-explainer/plugins/visual-explainer/commands/*.md ~/.config/opencode/command/
rm -rf /tmp/visual-explainer
```

**Invoke:** Ask OpenCode to use the `visual-explainer` skill. Command behavior depends on build.

## Cursor

Rules-based guidance only — add to project rules:

- Copy `configs/cursor/visual-explainer.mdc` to Cursor rules
- Or paste contents into the project rules UI

No slash commands, no native tool. Cursor reads SKILL.md manually when asked.

## OpenClaw

Lightweight rules guidance:

- Use `configs/openclaw/AGENTS.md` as project guidance
- Reference `plugins/visual-explainer/` as canonical skill source

No command templates, no native tool.

## Output location

All harnesses write generated HTML to `~/.agent/diagrams/` by default. Browser auto-open depends on the harness, browser access, and sandbox rules.
