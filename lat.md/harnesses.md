# Harnesses

Multi-harness support matrix: which coding agent environments are supported, how each is installed, and what capabilities each provides.

## Support matrix

Which harnesses are supported and what each provides.

| Harness | Support level | Skill | Commands | Native tool | Install method |
|---------|--------------|-------|----------|-------------|----------------|
| Claude Code | Full (marketplace) | Yes | Yes (namespaced) | No | `/plugin marketplace add` + `/plugin install` |
| Pi | Full (package) | Yes | Yes | Yes (`visual_explainer`) | `pi install git:...` |
| Codex CLI | Manual | Yes | Optional | No | Copy to `~/.codex/skills/` |
| OpenCode | Manual | Yes | Optional | No | Copy to `~/.config/opencode/skill/` |
| Cursor | Rules guidance | Via rules | No direct | No | Add `.mdc` rule file |
| OpenClaw | Rules guidance | Via rules | No direct | No | Copy `AGENTS.md` |

## Claude Code

Claude Code has the deepest integration via the marketplace plugin system.

**Install:**
```bash
/plugin marketplace add nicobailon/visual-explainer
/plugin install visual-explainer@visual-explainer-marketplace
```

**Commands are namespaced:** `/visual-explainer:generate-web-diagram`, `/visual-explainer:diff-review`, etc.

**Plugin structure:** Two `plugin.json` files — one at `.claude-plugin/` for marketplace discovery, one at `plugins/visual-explainer/.claude-plugin/` for the actual installable plugin. The marketplace `marketplace.json` controls catalog listing.

## Pi

Pi has the richest integration: skill + commands + native extension tool.

**Install (preferred):**
```bash
pi install git:github.com/nicobailon/visual-explainer
```

**From local checkout:**
```bash
pi install ./visual-explainer
```

**Package metadata** in `package.json` declares:
- `pi.skills` → `./plugins/visual-explainer` (loads SKILL.md)
- `pi.prompts` → `./plugins/visual-explainer/commands` (seven command templates)
- `pi.extensions` → `./plugins/visual-explainer/extension.ts` (native tool)

**Native tool:** `visual_explainer` with two actions:
- `action: "prepare"` — plans a visual explanation, detects subagent availability, returns recommended flow
- `action: "render"` — writes complete HTML to `~/.agent/diagrams/`, opens in browser

See [[extension]] for full details.

**Legacy installer:** `install-pi.sh` copies skill + prompts to `~/.pi/agent/` but does NOT install the native tool. Remove old copies before switching to `pi install` to avoid shadow conflicts:

```bash
rm -rf ~/.pi/agent/skills/visual-explainer
rm -f ~/.pi/agent/prompts/{diff-review,fact-check,generate-slides,...}.md
```

## Codex CLI

Manual copy installation. No marketplace, no native tool.

**Install:**
```bash
git clone --depth 1 https://github.com/nicobailon/visual-explainer.git /tmp/visual-explainer
mkdir -p ~/.codex/skills ~/.codex/prompts
cp -R /tmp/visual-explainer/plugins/visual-explainer ~/.codex/skills/visual-explainer
cp /tmp/visual-explainer/plugins/visual-explainer/commands/*.md ~/.codex/prompts/
rm -rf /tmp/visual-explainer
```

**Invoke:** `$visual-explainer` or ask Codex to use the `visual-explainer` skill. Commands may work as `/prompts:diff-review` depending on build version. Prompt templates are optional and deprecated — the skill works without them.

## OpenCode

Manual copy to observed native skill path.

**Install:**
```bash
git clone --depth 1 https://github.com/nicobailon/visual-explainer.git /tmp/visual-explainer
mkdir -p ~/.config/opencode/skill ~/.config/opencode/command
cp -R /tmp/visual-explainer/plugins/visual-explainer ~/.config/opencode/skill/visual-explainer
cp /tmp/visual-explainer/plugins/visual-explainer/commands/*.md ~/.config/opencode/command/
rm -rf /tmp/visual-explainer
```

**Invoke:** Ask OpenCode to use the `visual-explainer` skill. Command template behavior depends on the installed build.

## Cursor

Rules-based guidance only — not native Agent Skills support.

**Install:** Add `configs/cursor/visual-explainer.mdc` to Cursor project rules. The `.mdc` file points Cursor at the canonical skill directory and tells it to follow `SKILL.md` workflow.

**Limitations:** No slash commands, no native tool. Cursor must read `SKILL.md` and `commands/*.md` manually when asked for diagrams or reviews.

## OpenClaw

Lightweight rules guidance — no plugin adapter.

**Install:** Use `configs/openclaw/AGENTS.md` as project guidance. Copy or reference `plugins/visual-explainer/` as the canonical skill source.

**Limitations:** No command templates, no native tool. The agent reads SKILL.md and follows its workflow when producing diagrams, reviews, or slide decks.
