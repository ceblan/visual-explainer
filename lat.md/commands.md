# Commands

Seven slash commands that template common visual explanation workflows. Each command loads the visual-explainer skill, gathers data, then generates a self-contained HTML page.

## Command overview

Seven commands for generating HTML visual explanations from terminal output.

| Command | Purpose | Key sections |
|---------|---------|-------------|
| `/generate-web-diagram` | General-purpose HTML diagram for any topic | Auto-selected representation |
| `/generate-visual-plan` | Visual implementation plan for a feature | 9 required sections |
| `/generate-slides` | Magazine-quality slide deck | 10 slide types, narrative arc |
| `/diff-review` | Visual diff review with architecture comparison | 7 required sections |
| `/plan-review` | Plan vs codebase with risk assessment | 8 required sections |
| `/project-recap` | Mental model snapshot for context switching | 8 required sections |
| `/fact-check` | Verify accuracy of a generated document | Claim extraction + verification |

## Shared behavior

All commands:

- Load the visual-explainer skill and follow its routing rules
- Write output to `~/.agent/diagrams/` with descriptive filenames
- Open generated HTML in the browser
- Follow the skill's Mermaid, table, overflow, and delivery rules
- In Pi package installs, can use `visual_explainer` with `prepare` for context scouting and `render` for the final write

## Slide mode

Any scrollable-page command supports `--slides` to generate a slide deck instead:

```bash
/diff-review --slides
/project-recap --slides 2w
/generate-web-diagram --slides
```

When `--slides` is detected, the agent reads `slide-patterns.md` and `slide-deck.html` instead of the scrollable-page templates, and uses the 10 slide types with SlideEngine navigation.

## `/generate-web-diagram`

General-purpose. The agent picks the representation based on content:

| Content type | Representation |
|-------------|----------------|
| Flowchart, pipeline, state machine | Mermaid |
| Sequence, ER, class, C4, topology | Mermaid |
| Text-heavy architecture, module internals | CSS Grid cards |
| 15+ element architecture | Hybrid: small Mermaid + CSS cards |
| Comparison/audit matrix | HTML `<table>` |
| Timeline/roadmap | CSS timeline |
| Dashboard/metrics | CSS grid + charts/KPIs |

**Usage:** `/generate-web-diagram <topic>` — topic can be any natural language description.

## `/generate-visual-plan`

Produces an editorial/blueprint-style HTML page for documenting feature specs before implementation.

**Required sections:**

1. Goal and scope
2. Current state (architecture diagram)
3. Proposed design (Mermaid or hybrid)
4. Implementation sequence (ordered phases)
5. File map (create/edit/delete)
6. Interface/contracts (types, APIs, schemas)
7. Risk and decision matrix
8. Test plan (unit/integration/e2e)
9. Acceptance checklist

**Research first:** The agent reads relevant repo files before planning — entry points, existing patterns, affected modules, public APIs, tests, config/schema, similar features.

## `/diff-review`

Visual diff review comparing code changes across branches, commits, or PRs.

**Required sections:**

1. Executive summary
2. File map (color-coded new/modified/deleted)
3. Architecture impact (Mermaid or hybrid)
4. Before/after behavior (side-by-side)
5. Risk review (correctness, tests, API, security, performance, maintainability)
6. Coupling map (dependencies, hidden coupling)
7. Review recommendation (merge/readiness, blockers, follow-ups)

**Scope detection:** Interprets argument as branch, commit, range, PR, or `HEAD`. Without argument, compares working tree against `main`/master.

**Data gathering:** Runs git commands for diff stats, name-status, changed files, line counts, public API changes, tests touched, dependencies. Reads changed files and surrounding code paths.

## `/plan-review`

Compares an implementation plan against the actual codebase.

**Required sections:**

1. Plan summary
2. Accuracy verdict (correct/stale/risky/unsupported/missing)
3. Current architecture diagram
4. Proposed architecture (visual diff against current)
5. Gap/risk matrix
6. File-by-file review
7. Better plan (concrete corrections)
8. Decision (approve/revise/reject)

**Source verification:** For each proposed change, verifies whether referenced files exist, whether current behavior matches, and what ripple effects are missing.

## `/project-recap`

Generates a mental model snapshot for returning to a project after time away.

**Required sections:**

1. Project identity (what, stack, entry points)
2. Architecture snapshot (Mermaid or hybrid)
3. Recent activity (grouped narrative)
4. Current state (uncommitted work, branches, TODOs, blockers)
5. Mental model map (key modules, data flow, paths)
6. Risks and cognitive debt
7. Useful commands and files
8. Likely next steps (evidence-based)

## `/fact-check`

Verifies accuracy of a generated HTML document against actual code and git history.

**Workflow:**

1. Read target document (argument or most recent in `~/.agent/diagrams/`)
2. Extract verifiable claims (file paths, function names, behavior, architecture, APIs, dependencies)
3. Inspect actual source or git history for each claim
4. Classify: verified, corrected, unsupported, unverifiable
5. Correct factual errors in place, add verification summary
