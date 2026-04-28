# My Contributions to OpenChamber

This document highlights my pull requests merged into the official [openchamber/openchamber](https://github.com/openchamber/openchamber) repository.

---

## Merged Pull Requests

### perf: Drastically improve cold-start, bundle size, and streaming performance
**PR #1000** | [View on GitHub](https://github.com/openchamber/openchamber/pull/1000) | Merged

A comprehensive performance overhaul targeting three critical areas:

- **Cold-start**: Split bootstrap into 3 phases (blocking → deferred → lazy), removing blocking I/O from the render path. Cuts 50–200ms from time-to-first-paint.
- **Bundle size**: Code-split heavy views (Settings, Git, Diff, Terminal, Files, Plan, Onboarding) with React.lazy. Reduced chunk size warning threshold from 1200KB → 500KB.
- **Streaming**: Throttled store writes (~60Hz → ~1Hz), added custom React.memo comparators for message rows, and lowered virtualization threshold (40 → 15 messages).

**Impact**: Faster app launch, smaller initial bundle, smoother streaming on long sessions.

---

### fix: Make todo list update dynamically when task status changes
**PR #999** | [View on GitHub](https://github.com/openchamber/openchamber/pull/999) | Merged

Fixed a UI staleness bug where todo lists created by AI would not visually update when tasks were marked complete, in-progress, or pending.

- **Root cause**: `areRenderRelevantPartsEqual` did not compare the `output` field on tool parts, so React.memo skipped re-renders for streaming todo updates.
- **Fix**: Added `output` reference comparison for tool parts.

**Impact**: Real-time todo status updates in chat.

---

### perf: Lazy-load heavy dependencies (MarkdownRenderer + CodeMirror languages)
**PR #997** | [View on GitHub](https://github.com/openchamber/openchamber/pull/997) | Merged

Reduced initial bundle size by lazy-loading heavy dependencies:

- **MarkdownRenderer**: Moved the full markdown stack (~1500 lines including marked, react-markdown, mermaid, syntax highlighter, katex) to a dynamically imported module.
- **CodeMirror languages**: Removed static imports for 10+ less-common language parsers. Only 6 common languages remain in the initial bundle; others load on demand via `@codemirror/language-data`.

**Impact**: ~200KB+ reduction in initial bundle size. Markdown and syntax highlighting still work seamlessly.

---

## Open Pull Requests

### feat: Add per-agent default thinking variant setting
**PR #1056** | [View on GitHub](https://github.com/openchamber/openchamber/pull/1056) | Open

Added a **Default Thinking** dropdown to the agent settings page, allowing users to configure a preferred reasoning variant for each custom agent.

- The dropdown dynamically fetches available thinking levels from the selected model's provider.
- The configured variant automatically applies when the agent is active in chat (falls back after session-specific overrides, before global defaults).
- Covers direct sends, queued auto-sends, and model resolution logic.

**Impact**: Agent-specific workflows (e.g., an "advisor" agent that always needs extended reasoning) no longer require manual switching.

---

## Contribution Stats

| Metric | Count |
|--------|-------|
| Merged PRs | 3 |
| Open PRs | 1 |
| Total Lines Added | ~1,890 |
| Total Lines Removed | ~1,819 |
| Focus Areas | Performance, UX Polish, Agent Configuration |

---

*All contributions were made to the upstream [openchamber/openchamber](https://github.com/openchamber/openchamber) repository. This fork exists to track my personal contributions and experiment with features before upstreaming.*
