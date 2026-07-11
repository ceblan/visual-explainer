# Extension

The Pi native tool (`visual_explainer`) registered by `extension.ts`. Provides permission-aware visual explanation planning and browser-integrated HTML rendering.

## Tool definition

**Name:** `visual_explainer`
**Label:** Visual Explainer
**Execution mode:** sequential

## Actions

Two actions: `prepare` for planning, `render` for writing and opening HTML.

### `prepare`

Plans a visual explanation. Detects subagent availability and returns a recommended workflow.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | `"prepare"` | yes | Must be `"prepare"` |
| `topic` | string | yes | What the visual explanation covers |
| `goal` | string | no | What the user wants to understand/decide/communicate |
| `files` | string[] | no | Relevant files to inspect |
| `audience` | string | no | Target audience (developer, PM, team, reviewer, executive) |
| `preferSubagent` | boolean | no | Recommend scout subagent first (defaults to true) |

**Returns:**

- `topic`, `goal`, `audience`, `files` echoed back
- `subagentAvailable` — whether the subagent tool is active
- `recommendedFlow` — 5-step workflow (scout → outline → read references → generate → render)
- `subagentPrompt` — suggested task text if subagent is available

**Subagent detection:** Calls `pi.getActiveTools()` to check for `"subagent"`. Falls back to `pi.getAllTools()` if that throws. The `preferSubagent: false` parameter skips subagent recommendation.

**Design intent:** The prepare action is a planning checkpoint. Because visual explanations consume many tokens, the tool advises the agent to ask before proceeding unless the user explicitly requested a diagram. The recommended flow ensures the agent reads references before generating.

### `render`

Writes complete HTML to `~/.agent/diagrams/` and opens it in the browser.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `action` | `"render"` | yes | Must be `"render"` |
| `filename` | string | yes | Basename or slug (`.html` appended if missing) |
| `html` | string | yes | Complete self-contained HTML document |
| `open` | boolean | no | Open in browser (defaults to true) |

**Validation:**

- `filename`: Rejects paths with `/` or `\`, `..`, control characters, leading `@`
- `html`: Must start with `<!doctype html>` or `<html>` and end with `</html>`. Parsed through lenient HTML parser, not strict XML.
- Output directory: Creates `~/.agent/diagrams/` if needed. Rejects symlinks on both directory and file.

**Browser open:**

| Platform | Command |
|----------|---------|
| macOS | `open <path>` |
| Linux | `xdg-open <path>` |
| Windows | `cmd /c start "" <path>` |

The open call has a 250ms timeout — if the browser doesn't respond, it reports `"dispatched"` and moves on. The child process is `unref()`'d so it doesn't block the event loop.

**Returns:**

- `path` — full path to written file
- `openAttempted`, `openStatus` (disabled/unsupported/dispatched/failed), `openError`

## Prompt guidelines

The tool includes `promptGuidelines` that shape agent behavior:

1. After generating or reviewing a plan/architecture/diff, consider offering a visual explanation
2. Ask before calling `prepare` unless the user explicitly requested visual output
3. If `prepare` recommends subagent scouting, gather context first, then generate HTML, then call `render`
4. Use `render` only after generating complete HTML — pass a basename filename

## Integration with slash commands

The extension complements the slash commands rather than replacing them:

- `/generate-web-diagram` remains the prompt template for general diagrams
- The extension adds a two-step `prepare` → `render` flow for Pi-native installs
- The `prepare` action can recommend subagent scouting (not available in prompt-template-only flows)
- The `render` action handles browser opening and file validation (harder to do from a prompt template alone)
