# Tests

Test specifications for the visual-explainer skill and Pi extension. No test files exist in the repository currently. These specs define what should be tested.

## Extension tests

Tests for the Pi native `visual_explainer` tool defined in extension.ts.

### `visual_explainer` tool registration

The Pi extension registers a tool named `visual_explainer` with `executionMode: "sequential"`.

### prepare action

Tests for the `action: "prepare"` workflow.

#### Returns recommended flow for main agent

When `action: "prepare"` is called with a topic string, returns a `recommendedFlow` array with 5 steps and `subagentAvailable` reflecting the current environment.

#### Returns subagent prompt when subagent is available

When the subagent tool is active and `preferSubagent` is not false, includes a `subagentPrompt` string with the topic and goal embedded.

#### Skips subagent recommendation when preferSubagent is false

When `preferSubagent: false` is passed, `recommendedFlow` uses the direct-gathering variant and `subagentPrompt` is undefined.

#### Rejects empty topic

When `topic` is an empty string after trimming, throws `"topic is required"`.

#### Rejects non-string topic

When `topic` is not a string, throws `"topic must be a string"`.

### render action

Tests for the `action: "render"` workflow.

#### Writes HTML to ~/.agent/diagrams/

When given valid `filename` and `html`, creates the output directory if needed and writes the file.

#### Appends .html extension when missing

When `filename` is `"my-diagram"` (no extension), writes `my-diagram.html`.

#### Preserves .html extension when present

When `filename` is `"my-diagram.html"`, writes `my-diagram.html` (not `my-diagram.html.html`).

#### Rejects filenames with path separators

When `filename` contains `/` or `\`, throws `"filename must be a basename, not a path"`.

#### Rejects filenames with path traversal

When `filename` contains `..`, throws `"filename must not contain '..'"`.

#### Rejects filenames with control characters

When `filename` contains control characters (`\0`–`\x1f`, `\x7f`), throws `"filename must not contain control characters"`.

#### Strips leading @ from filename

When `filename` is `"@my-diagram"`, writes `my-diagram.html`.

#### Validates HTML is a complete document

When `html` does not start with `<!doctype html>` or `<html>` and end with `</html>`, throws validation error.

#### Accepts HTML with leading comments

When `html` starts with `<!-- comment --><!doctype html>`, accepts it (comments stripped before validation).

#### Rejects symlink output directory

When `~/.agent/diagrams/` is a symlink, throws `"must not be a symlink"`.

#### Rejects symlink output file

When the output file already exists as a symlink, throws `"must not be a symlink"`.

#### Returns path in response

When render succeeds, `details.path` contains the full path to the written file.

#### Respects open: false

When `open: false` is passed, `openAttempted` is false and `openStatus` is `"disabled"`.

#### Handles browser open failure

When the platform browser command fails, `openStatus` is `"failed"` and `openError` contains the error message.

#### Handles unsupported platform

When `process.platform` is not darwin/linux/win32, `openStatus` is `"unsupported"`.

#### Respects AbortSignal

When the signal is already aborted, throws immediately.

## Skill routing tests

Tests that the agent reads the correct references for each content type.

### Reference routing

When generating a Mermaid flowchart, the agent reads `mermaid-flowchart.html` and Mermaid sections in `libraries.md` — not `architecture.html` or `data-table.html`.

### Slide mode detection

When a command includes `--slides`, the agent reads `slide-deck.html` and `slide-patterns.md` instead of scrollable-page templates.

### Auto-detection for complex tables

When the agent is about to output a table with 4+ rows or 3+ columns in the terminal, it renders HTML instead and gives a short chat summary.

## Output tests

Tests for output file properties and browser integration.

### File location

Generated HTML is always written to `~/.agent/diagrams/` unless the user specifies another path.

### Self-contained output

Generated HTML is a complete document with embedded CSS and inline JS. Opening it in a browser with no internet connection still produces a usable page (Mermaid and Chart.js may require CDN — this is documented).

### Dark/light mode

Every generated page supports both light and dark themes via `prefers-color-scheme` media query or `data-theme` attribute.

### No horizontal overflow

Generated pages have no horizontal scroll at normal desktop width (1200px+).

### Mermaid zoom controls

Every Mermaid diagram includes zoom in/out/reset/expand controls, Ctrl/Cmd+scroll zoom, drag panning, and click-to-expand.

## Slide tests

Tests for slide deck generation and SlideEngine behavior.

### SlideEngine initialization

After DOM ready and library render, `new SlideEngine()` creates progress bar, dots, counter, and keyboard hints.

### Slide navigation

Arrow keys, space, PageDown advance slides. ArrowUp, ArrowLeft, PageUp go back. Home goes to first, End to last.

### Content density

Each slide fits in 100dvh. Content exceeding density limits (5–6 bullets, 8 table rows, 10 code lines) is split across slides.

### Compositional variety

Three consecutive slides with the same layout pattern (e.g., all centered) violates the variety rule.

### autoFit safety

After render, `autoFit()` scales down Mermaid SVGs, KPI values, and blockquotes that overflow their containers.
