# focuspipe — Goose Grant Proposal

## Project title

**focuspipe: ADHD-Aware Attention Monitoring for AI Agents**

## One-liner

An open-source Screenpipe pipe + Goose MCP extension that gives AI agents real-time awareness of human attention state — built for neurodivergent developers, useful for everyone.

## The problem

340 million adults worldwide have ADHD. In tech, the prevalence is even higher — the field self-selects for hyperfocus-capable, novelty-seeking minds. Yet every productivity tool assumes neurotypical attention:

- **Time trackers** measure hours, not attention quality
- **Focus apps** block distractions but can't detect when you've drifted to the wrong task
- **AI assistants** have zero awareness of your cognitive state — they'll happily suggest a complex refactor when you're scattered across 15 tabs

The result: developers with ADHD build incredible things during hyperfocus sessions, then lose hours to invisible task drift, unmanaged context switching, and post-session amnesia ("I worked all day but what did I actually do?").

**AI agents need to understand human attention the way they understand code.** An agent that knows you're in deep focus should protect that state. An agent that detects you've drifted should gently redirect. An agent that understands your weekly attention patterns should schedule complex tasks during your peak hours.

## The solution

**focuspipe** is a set of Screenpipe pipes + a Goose MCP server that creates an attention-aware layer for AI agents.

### How it works

1. **Screenpipe** continuously captures screen activity (OCR, accessibility tree, audio) — all stored locally in SQLite
2. **focuspipe pipes** run on a schedule, analyzing the captured data to classify attention states: deep focus, shallow work, context switching, drift, break, idle
3. **The MCP server** exposes attention state to any MCP-compatible AI agent (Goose, Claude, Cursor)
4. **The nudge system** provides real-time notifications calibrated for ADHD brains — gentle, non-judgmental, configurable

### What makes this different

- **Attention classification, not time tracking** — "2 hours deep focus on auth refactor" vs "2 hours at computer"
- **Built for ADHD** — understands hyperfocus as a feature (not a bug), detects task drift (not just distraction), respects flow state (nudges are gentle, never jarring)
- **Agent-native** — the primary consumer is AI agents, not dashboards. Goose can query "is now a good time for a complex task?" and get a real answer
- **Local-first** — attention data is deeply personal. It never leaves your machine. No cloud, no telemetry, no employer surveillance
- **Open source** — MIT licensed, built on MIT-licensed foundations (Screenpipe, Goose)

## Grant category alignment

focuspipe directly addresses multiple Goose grant categories:

### Self-flying agents
focuspipe is a background agent that runs continuously via Screenpipe's pipe scheduler. It monitors attention patterns, detects state changes, and takes autonomous action (nudges, summaries, task recommendations) without explicit user commands. This is exactly the "long-running background mode" and "intermediate states" the grant program describes.

### New interaction paradigms
Instead of the user telling the AI what they're doing, the AI observes and infers cognitive state. This inverts the interaction model — the agent responds to attention, not commands. It's emotion/cognition-aware AI in the most practical sense: "I can see you're scattered right now, let me help you pick one thing."

### Self-improving
focuspipe learns individual attention patterns over time. The classification model adapts to your specific focus signatures — your hyperfocus looks different from mine. The weekly analytics feed back into better nudge timing and task recommendations.

## Architecture

```
┌─────────────────────────────────────────────┐
│                  User's Machine              │
│                                              │
│  ┌──────────┐    ┌──────────────────────┐   │
│  │Screenpipe│───>│   focuspipe pipes     │   │
│  │ (capture)│    │                       │   │
│  └──────────┘    │ ┌──────────────────┐  │   │
│                  │ │ Attention State   │  │   │
│                  │ │ Detector          │  │   │
│                  │ ├──────────────────┤  │   │
│                  │ │ Hyperfocus       │  │   │
│                  │ │ Guardian         │  │   │
│                  │ ├──────────────────┤  │   │
│                  │ │ Task Drift       │  │   │
│                  │ │ Monitor          │  │   │
│                  │ ├──────────────────┤  │   │
│                  │ │ Context Switch   │  │   │
│                  │ │ Tracker          │  │   │
│                  │ ├──────────────────┤  │   │
│                  │ │ Session          │  │   │
│                  │ │ Reconstructor    │  │   │
│                  │ ├──────────────────┤  │   │
│                  │ │ Pattern          │  │   │
│                  │ │ Analytics        │  │   │
│                  │ └──────────────────┘  │   │
│                  └──────────┬───────────┘   │
│                             │               │
│                  ┌──────────▼───────────┐   │
│                  │  focuspipe SQLite     │   │
│                  │  (attention history)  │   │
│                  └──────────┬───────────┘   │
│                             │               │
│                  ┌──────────▼───────────┐   │
│                  │  MCP Server           │   │
│                  │  (attention API)      │   │
│                  └──────────┬───────────┘   │
│                             │               │
│              ┌──────────────┼──────────┐    │
│              │              │          │    │
│         ┌────▼───┐    ┌────▼───┐ ┌───▼──┐ │
│         │ Goose  │    │ Claude │ │Cursor│ │
│         └────────┘    └────────┘ └──────┘ │
│                                           │
│         ┌────────────────────────┐        │
│         │ Nudge System           │        │
│         │ (macOS notifications)  │        │
│         └────────────────────────┘        │
└─────────────────────────────────────────────┘
```

## Deliverables (12-month plan)

### Q1: Foundation (Months 1-3)
- [ ] Attention state classification engine (deep focus / shallow / switching / drift / idle)
- [ ] Core Screenpipe pipe: real-time attention state detection
- [ ] Local SQLite schema for attention state history
- [ ] Basic macOS notification nudge system
- [ ] Open source repo with documentation

**Milestone:** Working prototype that classifies attention states from Screenpipe data and sends nudges.

### Q2: Intelligence (Months 4-6)
- [ ] Hyperfocus detection and guardian pipe
- [ ] Task drift monitor (Linear/GitHub integration for intent comparison)
- [ ] Context switch frequency analysis
- [ ] Session reconstructor pipe
- [ ] MCP server exposing attention state to Goose

**Milestone:** Goose can query "what's my attention state?" and get a real-time answer. Hyperfocus sessions are detected and protected.

### Q3: Patterns (Months 7-9)
- [ ] Weekly/monthly pattern analytics
- [ ] Personal attention model (adapts to individual patterns)
- [ ] Obsidian sync pipe for daily attention logs
- [ ] Configurable nudge profiles (intensity, frequency, channels)
- [ ] Dashboard (local web UI)

**Milestone:** Users see their attention patterns over time. The system learns their specific focus signatures.

### Q4: Ecosystem (Months 10-12)
- [ ] Goose extension: attention-aware task scheduling
- [ ] Community pipe templates for common ADHD strategies
- [ ] Research paper: "Attention-Aware AI Agents" (if data supports findings)
- [ ] Plugin system for custom attention classifiers
- [ ] Performance optimization (< 2% CPU overhead on top of Screenpipe)

**Milestone:** Complete ecosystem — capture → classify → nudge → analyze → improve. Goose integration fully functional.

## About the applicant

**Vitalik Garan** — Solo AI developer based in Frankfurt, Germany. Former Amazon engineer. Currently building AI-powered tools full-time, including:

- **hawker** — AI-powered second-hand marketplace automation
- **terminal-craft** — Developer workspace optimization system
- **Claude Code power user** — Deep experience with MCP servers, AI agent workflows, and tool integration

**Why I'm building this:** I have ADHD. I've spent years trying every productivity tool, and they all fail the same way — they track time, not attention. After discovering Screenpipe and realizing its pipe system could support real attention monitoring, I saw the missing piece: nobody has built the intelligence layer that understands ADHD attention patterns. focuspipe is the tool I need, built by someone who understands the problem from the inside.

**Open source track record:** Active GitHub contributor, multiple public repos, Claude Code community member.

## Budget

| Category | Amount | Notes |
|----------|--------|-------|
| Developer time (12 months) | $80,000 | Full-time development |
| Screenpipe Pro license | $600 | Cloud features for testing |
| Infrastructure (CI/CD, testing) | $2,400 | GitHub Actions, test devices |
| User research & testing | $5,000 | ADHD community outreach, beta testing |
| Documentation & community | $5,000 | Docs, tutorials, Discord community |
| Conference/presentation | $5,000 | Present at AI/accessibility conferences |
| Contingency | $2,000 | Unexpected costs |
| **Total** | **$100,000** | |

## Why this matters

ADHD affects 5-7% of adults. In the developer community, it's likely higher. These are some of the most creative, innovative minds in tech — and they're underserved by every productivity tool on the market.

AI agents are becoming central to developer workflows. If these agents can't understand human attention state, they'll continue to interrupt deep focus, miss signs of task drift, and fail to protect the hyperfocus sessions that produce developers' best work.

focuspipe makes AI agents attention-aware. It's useful for everyone, but transformative for the 1 in 20 developers whose brains work differently.

**Open, local, private, and built with empathy.** That's focuspipe.
