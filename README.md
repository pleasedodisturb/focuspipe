# focuspipe

ADHD-aware focus tracking and executive function support — built as a [Screenpipe](https://screenpi.pe) pipe with [Goose](https://github.com/block/goose) integration.

**focuspipe** is an open-source, local-first AI agent that monitors your screen activity to detect attention patterns, hyperfocus sessions, task drift, and context-switching frequency. It provides real-time nudges and weekly analytics designed specifically for neurodivergent developers.

## Why

Every productivity tool assumes neurotypical attention. They track time, not attention quality. For developers with ADHD:

- **Hyperfocus** is a superpower — but it needs guardrails (3am "just one more thing" sessions)
- **Task drift** is invisible until you realize 2 hours vanished on the wrong thing
- **Context switching** has a measurable cognitive cost — but no tool quantifies yours
- **Post-session amnesia** — you did 6 hours of deep work and can't remember what

focuspipe doesn't judge. It observes, surfaces patterns, and nudges — built by someone with ADHD, for people with ADHD.

## What it does

### Core pipes

1. **Attention State Detector** — Classifies real-time activity into: deep focus, shallow work, context switching, drift, break, idle. Uses app switching frequency, typing patterns, and content analysis via Screenpipe's OCR + accessibility data.

2. **Hyperfocus Guardian** — Detects sustained single-task focus and provides configurable nudges ("You've been deep in this for 3 hours — hydrate, stretch, check if this is still the right task"). Respects flow state — nudges are gentle, not interruptive.

3. **Task Drift Monitor** — Compares current activity against your stated intention (pulled from Linear/GitHub/todoist). Flags when you've drifted >15 minutes from your planned work without a conscious switch.

4. **Context Switch Tracker** — Measures switching frequency, calculates cognitive cost estimates, surfaces daily/weekly patterns. "You averaged 23 context switches/hour on Mondays vs 8 on deep-work Wednesdays."

5. **Session Reconstructor** — Post-session summaries: what you worked on, for how long, what you accomplished. Fights post-hyperfocus amnesia. Optionally syncs to Obsidian/Linear.

6. **Pattern Analytics** — Weekly reports: best focus hours, worst drift triggers, hyperfocus frequency, task completion correlation. Learns YOUR patterns over time.

### Goose integration

focuspipe exposes an MCP server that lets Goose (or any MCP-compatible AI agent) query your attention state:

- "Am I in focus right now?" → real-time attention classification
- "What did I work on yesterday?" → session reconstruction
- "When are my best deep-work hours?" → pattern analytics
- "Should I start this complex task now?" → attention-state-aware task recommendations

### Architecture

```
Screenpipe (captures screen + audio)
    ↓
focuspipe pipes (analyze attention patterns)
    ↓
Local SQLite (attention state history)
    ↓
MCP server (expose to Goose / Claude / any AI agent)
    ↓
Nudge system (macOS notifications + optional Slack/Discord)
```

Everything runs locally. No cloud. No telemetry. Your attention data never leaves your machine.

## Tech stack

- **Screenpipe** — screen/audio capture + OCR + local storage
- **Screenpipe Pipes** — scheduled AI agents (markdown + TypeScript)
- **Goose MCP** — AI agent integration layer
- **SQLite + FTS5** — local attention state database
- **Bun** — JavaScript runtime for pipes
- **Rust** — performance-critical analysis (optional, for real-time classification)

## Status

**Pre-alpha** — Architecture design and grant proposal stage.

## License

MIT
