# Top Open-Source AI Repos: Analysis for Vitalik's Workflow

> Research date: November 16, 2026
> Sources: Multiple curated lists (bitdoze.com, fungies.io, glukhov.org, firecrawl.dev, pinggy.io, findarepo.com, and others), cross-referenced against GitHub project pages.
> Note: goodailist.com was unreachable from the research environment; this analysis is compiled from 10+ overlapping 2026 roundups that cover the same repo universe.

---

## Table of Contents

1. [Integrate NOW](#1-tools-to-integrate-now)
2. [Worth Watching](#2-tools-worth-watching)
3. [Building Blocks for Projects](#3-building-blocks-for-focuspipe-hawker-and-careeros)
4. [Grant Inspiration](#4-grant-inspiration)
5. [Skip List](#5-skip-list)

---

## 1. Tools to Integrate NOW

These would immediately improve your daily workflow with minimal setup.

### 1.1 OpenCode — Open-source terminal coding agent

- **GitHub:** https://github.com/opencode-ai/opencode
- **Stars:** ~170K+ (Nov 2026)
- **License:** MIT

**What it does:** A provider-agnostic terminal coding agent built by the SST/terminal.shop team. Supports 75+ LLM providers including Ollama for local inference. Features a TUI that acts as a client to a local agent server — meaning the same session can be driven from multiple terminals, a desktop app, or even a mobile client. Has "plan" (read-only) and "build" (full access) agent modes, plus custom agent support and an LSP.

**Why it's relevant:** You're already deep in Claude Code, but OpenCode is the fallback you need. When Claude is down, rate-limited, or you want to use a local model for sensitive code, OpenCode fills the gap. It's the most active open-source coding agent in 2026. The client-server architecture means you could start a task on your Mac Mini and pick it up on the MacBook.

**Integration effort:** Drop-in (brew install, configure API keys)
**Concerns:** Large community but young project — API may shift between releases. You'd maintain two agent configs.

---

### 1.2 browser-use — AI browser automation agent

- **GitHub:** https://github.com/browser-use/browser-use
- **Stars:** ~118K+ (Nov 2026)
- **License:** MIT

**What it does:** A Python library that lets AI agents navigate websites, fill forms, extract data, and complete multi-step web workflows. Works with any LLM (including local models via Ollama). The open-source version handles the full agent loop — navigation, extraction, structured output. A separate cloud offering adds stealth features, CAPTCHA handling, and proxy rotation.

**Why it's relevant:** This is the engine hawker needs. Instead of writing brittle Playwright selectors for each marketplace, you describe what to do in natural language and browser-use handles the navigation. For marketplace monitoring, listing creation, and price checking across platforms, this dramatically reduces maintenance when sites change their UI.

**Integration effort:** Weekend project — pip install, write agent scripts for your marketplace flows
**Concerns:** 89% success rate on benchmarks means ~1 in 10 actions may fail. No built-in stealth in OSS version — marketplaces may detect automated access. Python 3.11+ required.

---

### 1.3 Crawl4AI — LLM-ready web crawler

- **GitHub:** https://github.com/unclecode/crawl4ai
- **Stars:** ~85K+ (Nov 2026)
- **License:** Apache 2.0

**What it does:** A Python crawler that outputs clean Markdown or structured JSON from any website. Features LLM-driven extraction with typed JSON schemas, chunking strategies (topic, regex, sentence), and works with any LLM provider including local models.

**Why it's relevant:** Two immediate uses: (1) For hawker — crawl marketplace listings into structured data for price comparison and deal detection. (2) For CareerOS — crawl job boards and company career pages into structured job postings. Unlike Firecrawl, the full feature set is available in the self-hosted version with no AGPL restrictions.

**Integration effort:** Drop-in — `pip install crawl4ai`, point at URLs, get structured JSON
**Concerns:** Rate limiting and anti-bot measures are your problem. Heavy crawling needs a proxy setup.

---

### 1.4 n8n — Workflow automation platform

- **GitHub:** https://github.com/n8n-io/n8n
- **Stars:** ~100K+ (Nov 2026)
- **License:** Sustainable Use License (source-available, NOT OSI open-source)

**What it does:** A self-hosted workflow automation platform with 2,000+ app integrations. Visual workflow builder with code blocks for custom logic. Connects everything — email, APIs, databases, messaging, file systems.

**Why it's relevant:** This is the glue layer for your automation stack. Instead of writing separate cron jobs and scripts: wire YNAB webhooks to spending alerts, automate TickTick task creation from Linear updates, pipe marketplace notifications through filters, schedule job board scraping for CareerOS. For someone with ADHD, n8n's visual builder means you can see and modify workflows without remembering what that bash script in crontab does.

**Integration effort:** Weekend project — Docker self-host, then build workflows incrementally
**Concerns:** Not true open source (Sustainable Use License). Resource-hungry for a personal server — needs 2GB+ RAM. Maintenance burden: upgrades can break workflows.

---

### 1.5 SearXNG — Privacy-respecting metasearch engine

- **GitHub:** https://github.com/searxng/searxng
- **Stars:** ~16K+ (Nov 2026)
- **License:** AGPL-3.0

**What it does:** A self-hosted metasearch engine that aggregates results from 250+ search services without tracking or profiling users. Supports Tor, custom search categories, and API access for programmatic queries.

**Why it's relevant:** You're privacy-conscious and local-first. Running your own SearXNG instance means your search history stays on your hardware. More importantly, the API endpoint lets your AI agents search the web privately — feed it into n8n workflows, Crawl4AI pipelines, or CareerOS job discovery without Google tracking your patterns.

**Integration effort:** Drop-in — Docker compose, works in 15 minutes
**Concerns:** Some upstream engines throttle or block self-hosted instances. Needs occasional updates to keep engine list working. If you're the only user, your searches may be identifiable to upstream engines.

---

### 1.6 Mem0 — Memory layer for AI agents

- **GitHub:** https://github.com/mem0ai/mem0
- **Stars:** ~68K+ (Nov 2026)
- **License:** Apache 2.0

**What it does:** A universal memory layer that extracts, stores, and retrieves facts from conversations. Supports memory scoped to users, agents, and sessions. Uses hybrid search (BM25 + semantic embeddings + entity linking) with vector and optional graph storage backends. Available as a self-hosted SDK or managed cloud.

**Why it's relevant:** For focuspipe specifically — Mem0 could persist context about your work patterns, focus states, and productivity data across sessions. For Claude Code workflows, it could remember project-specific decisions and preferences. For CareerOS, it could track what you've already applied to, interviewer names, and company-specific context without you maintaining a spreadsheet.

**Integration effort:** Weekend project — pip/npm install, needs a vector store (Qdrant recommended)
**Concerns:** Self-hosted performance may differ from managed platform benchmarks. Needs a running vector DB.

---

## 2. Tools Worth Watching

Not ready for immediate integration or needs evaluation, but promising.

### 2.1 Khoj — AI second brain

- **GitHub:** https://github.com/khoj-ai/khoj
- **Stars:** ~25K+ (Nov 2026)
- **License:** AGPL-3.0

**What it does:** A self-hostable AI "second brain" that indexes your documents, web pages, and notes into a searchable knowledge base. Features custom agents, scheduled automations, and deep research across your files and the live web. Access from Obsidian, desktop app, Emacs, web, or WhatsApp.

**Why it's relevant:** For ADHD, the "search everything you've ever read" capability is powerful. Index your project docs, Linear tickets, job search notes, and marketplace research — then ask questions across all of it. The scheduled automation feature could replace manual check-ins: "every morning, summarize what changed in my Linear projects."

**Integration effort:** Significant — self-hosted setup requires Docker, model configuration, and indexing pipeline
**Concerns:** AGPL license. Resource-intensive when self-hosting with local models. Setup is not trivial.

---

### 2.2 Career-Ops — AI job search agent

- **GitHub:** https://github.com/Gsync/career-ops (verify — may have moved)
- **Stars:** ~69K+ (reported, unverified)
- **License:** MIT

**What it does:** An open-source AI job search agent that runs inside your existing coding CLI (Claude Code, Codex, OpenCode). Evaluates job postings into structured reports with scoring, generates tailored resumes and cover letters, and keeps humans in the loop for applications.

**Why it's relevant:** Directly relevant to CareerOS. If it works as described, you could integrate its evaluation and resume-tailoring logic into your own system rather than building from scratch. The Claude Code integration means it fits your existing workflow.

**Integration effort:** Weekend project — but verify the repo is actively maintained first
**Concerns:** High star count is unverified and may be inflated. The space is full of abandoned projects. Test thoroughly before building on it.

---

### 2.3 Stagehand — AI browser SDK

- **GitHub:** https://github.com/browserbase/stagehand
- **Stars:** ~22K+ (Nov 2026)
- **License:** MIT

**What it does:** A TypeScript/Python/Go SDK from Browserbase that adds natural-language browser control (act, extract, observe) on top of Playwright. Runs locally without needing a Browserbase account. The Playwright API remains available alongside AI methods.

**Why it's relevant:** If browser-use's Python-first approach doesn't fit your TypeScript stack, Stagehand is the alternative. Since you already use Playwright heavily, Stagehand is a natural extension — same browser engine, added AI layer. The `extract()` method could simplify hawker's data extraction.

**Integration effort:** Weekend project — npm install, augments existing Playwright scripts
**Concerns:** Each AI call uses 1-3K tokens, so high-volume scraping gets expensive. Still early — API may change.

---

### 2.4 Dayflow — AI-powered time tracking

- **GitHub:** Search for "dayflow" on GitHub (exact repo URL uncertain from research)
- **License:** Reported as open-source, verify

**What it does:** Records one screen frame every 10 seconds and uses AI to write the day into a timeline. Described as open-source and local-first, with AI-powered daily summaries.

**Why it's relevant:** If Screenpipe doesn't work out, this is the lighter alternative. For focuspipe, Dayflow's data (what you were doing, when, for how long) is exactly the input signal you need. The 10-second interval sampling is privacy-friendlier than continuous recording.

**Integration effort:** Evaluate first — verify it actually runs on macOS and is actively maintained
**Concerns:** Very new, limited community. Verify the "open source" claim — many tools in this space are source-available.

---

### 2.5 OpenHands — Autonomous coding agent

- **GitHub:** https://github.com/All-Hands-AI/OpenHands
- **Stars:** ~79K+ (Nov 2026)
- **License:** MIT

**What it does:** An autonomous coding agent that runs in a Docker sandbox. You assign it a GitHub issue and it plans, writes, tests, and opens a pull request. Supports Claude, GPT-4, Gemini, DeepSeek, or any OpenAI-compatible endpoint. Self-hostable with your own LLM keys.

**Why it's relevant:** For your open-source projects (focuspipe, hawker, CareerOS), OpenHands could handle routine issues — bug fixes, dependency updates, documentation. Assign an issue, review the PR. The sandboxed execution means it can't mess up your local environment.

**Integration effort:** Significant — Docker setup, 30-60 minutes for self-hosting, needs model API keys
**Concerns:** Autonomous agents need careful review. The sandbox adds safety but also complexity. Resource-heavy.

---

### 2.6 Jan — Local LLM desktop app

- **GitHub:** https://github.com/janhq/jan
- **Stars:** ~44K+ (Nov 2026)
- **License:** Verify on repo (sources disagree — AGPL-3.0 most likely)

**What it does:** A desktop app for running LLMs locally with a ChatGPT-style interface. Built on llama.cpp, supports GPU acceleration via Metal on Apple Silicon. Runs entirely offline, no telemetry. Can also connect to cloud providers when wanted.

**Why it's relevant:** When you want to chat with a local model without touching the terminal — quick questions, brainstorming, drafting text. Metal support means it runs well on your M-series Mac Mini. Complements Ollama (which runs headless) with a proper UI.

**Integration effort:** Drop-in — download and run
**Concerns:** UI can lag with larger models. The project has had organizational changes. Check recent commit activity.

---

## 3. Building Blocks for focuspipe, hawker, and CareerOS

### For focuspipe (ADHD monitoring)

| Repo | Use Case | Integration |
|------|----------|-------------|
| **ActivityWatch** (github.com/ActivityWatch/activitywatch) | Passive app/window tracking data — the foundational input signal for focus detection. MPL-2.0 licensed, cross-platform, extensible API. ~16K stars. | Use its API to pull window/app activity data into focuspipe's analysis pipeline. Replaces building your own activity monitor. |
| **Mem0** (github.com/mem0ai/mem0) | Persist focus patterns and productivity context across sessions. Remember what tasks the user was working on, their typical focus/break rhythms. | Feed ActivityWatch data through Mem0 to build a persistent model of work patterns. |
| **Screenpipe** (github.com/mediar-ai/screenpipe) | Full screen + audio capture with OCR and transcription. More data than ActivityWatch but heavier. Source-available (not true OSS). ~20K+ stars. | If ActivityWatch's app-level data isn't granular enough, Screenpipe provides screen content. Privacy trade-off is significant. |
| **Ollama** (github.com/ollama/ollama) | Run the local models that analyze focus data. No data leaves the machine. ~177K stars. | Essential infrastructure — focuspipe's AI analysis should run locally for privacy. |

### For hawker (marketplace bot)

| Repo | Use Case | Integration |
|------|----------|-------------|
| **browser-use** (github.com/browser-use/browser-use) | AI-driven marketplace navigation — listing creation, price monitoring, competitor analysis. Handles UI changes without selector updates. | Core automation engine. Write agent scripts per marketplace. |
| **Crawl4AI** (github.com/unclecode/crawl4ai) | Structured data extraction from marketplace listings — prices, availability, seller info, product details into typed JSON. | Feed marketplace URLs, get structured listing data. Pair with n8n for scheduling. |
| **Firecrawl** (github.com/mendableai/firecrawl) | Alternative to Crawl4AI with richer JS rendering. AGPL-3.0 means you'd need to open-source any linked service, or use the cloud tier. ~147K stars. | Consider if Crawl4AI can't handle heavily JS-rendered marketplace pages. |
| **n8n** (github.com/n8n-io/n8n) | Orchestrate the full pipeline: schedule crawls, filter deals, send notifications, update listings. | Glue layer — connect crawlers to notification channels (Pushover, email, Telegram). |
| **SearXNG** (github.com/searxng/searxng) | Privacy-respecting product search across multiple engines without creating a trackable pattern. | Feed product queries through SearXNG API for price comparison across the web. |

### For CareerOS (job search)

| Repo | Use Case | Integration |
|------|----------|-------------|
| **Career-Ops** (verify repo) | Pre-built job evaluation, resume tailoring, and application tracking. MIT licensed. | Evaluate as a starting point or component rather than building from scratch. |
| **Crawl4AI** (github.com/unclecode/crawl4ai) | Crawl job boards and company career pages into structured job postings with salary, requirements, location. | Feed URLs of job boards, get structured job data. |
| **Mem0** (github.com/mem0ai/mem0) | Track application history, interviewer names, company context, and what you've already discussed in interviews. | Persistent memory layer for the entire job search lifecycle. |
| **RAGFlow** (github.com/infiniflow/ragflow) | RAG over your own documents — resume versions, cover letters, interview notes, company research. ~78K stars, Apache 2.0. | Index your job search documents for AI-powered retrieval during prep. |
| **LangChain** (github.com/langchain-ai/langchain) | Orchestrate multi-step job search pipelines — scrape → evaluate → tailor resume → draft cover letter. ~148K stars, MIT. | Framework for building the CareerOS agent pipeline. |

---

## 4. Grant Inspiration

These repos suggest grant-worthy project ideas in adjacent spaces:

### 4.1 ADHD-Aware AI Workspace Agent
**Inspired by:** ActivityWatch + Mem0 + focuspipe concept
**Idea:** An open-source agent that passively monitors app usage, detects focus/distraction patterns, and proactively intervenes — not with willpower-dependent blocking, but by surfacing the right context when focus breaks. "You were working on the hawker pricing module 4 minutes ago. Here's where you left off." Unlike existing tools that require discipline (timers, blockers), this works *with* ADHD by reducing the cost of context-switching back.
**Grant angle:** Digital health, neurodiversity, open-source productivity tools.

### 4.2 Privacy-First Personal AI Memory
**Inspired by:** Mem0 + Khoj + local models
**Idea:** A fully local "memory OS" that indexes everything you do (with consent) — conversations, documents, web browsing, code changes — and makes it searchable through natural language. Unlike cloud services, all data stays on-device. Unlike Rewind/Screenpipe, it's truly open-source.
**Grant angle:** Digital sovereignty, privacy technology, personal knowledge management.

### 4.3 Open-Source Marketplace Intelligence Platform
**Inspired by:** browser-use + Crawl4AI + n8n
**Idea:** A self-hosted platform for small sellers to monitor prices, detect deals, automate listings, and analyze marketplace trends — the intelligence tools that large sellers pay thousands for, available to individuals. Built on open-source components with privacy-first design.
**Grant angle:** Small business technology, marketplace fairness, economic accessibility.

### 4.4 AI-Native Job Search OS
**Inspired by:** Career-Ops + CareerOS concept
**Idea:** An open-source, local-first system that treats job searching like a proper pipeline: discovery → evaluation → application → tracking → interview prep → negotiation. Each stage has AI assistance, but the human stays in the loop. Integrates with LinkedIn, job boards, and company sites.
**Grant angle:** Workforce development, economic mobility, AI for employment.

---

## 5. Skip List

Popular repos that are NOT relevant to your setup:

| Repo | Stars | Why Skip |
|------|-------|----------|
| **OpenClaw** | ~392K | Personal assistant for chat apps (WhatsApp, Telegram, Discord). You don't need a chat-based assistant — you work in the terminal. |
| **Dify** | ~157K | Visual LLM app builder. Overkill for a solo developer who can write code. The visual builder adds complexity without value for your workflow. |
| **Langflow** | ~155K | Same as Dify — visual agent builder. You'd use LangChain directly if you need the abstractions. |
| **Open WebUI** | ~156K | ChatGPT-style web interface for local models. You already have Claude Code in the terminal and Jan for desktop chat. A web UI adds nothing. |
| **LibreChat** | ~25K+ | Another ChatGPT-clone web interface. Same reasoning as Open WebUI. |
| **Gemini CLI** | ~100K | Google's CLI agent. You're invested in Claude Code and it's working. Switching provides no benefit for your stack. |
| **Codex CLI** | ~50K+ | OpenAI's CLI agent. Tuned for OpenAI models. You're on Claude. |
| **vLLM** | ~50K+ | High-throughput LLM serving engine. Built for multi-user serving at scale. You're a single user — Ollama is simpler. |
| **LocalAI** | ~45K | OpenAI API-compatible local server. Ollama already fills this role and has better ecosystem support. |
| **Flowise** | ~51K | Visual LLM builder — **reaches end-of-life August 31, 2026.** Dead project walking. |
| **AutoGen** | ~40K+ | Microsoft's multi-agent framework. Now in maintenance mode. Use CrewAI or LangChain instead if you need multi-agent. |
| **DeepSeek-V3** | ~100K+ | The model weights repo. Useful for researchers, not for a developer who uses models via API/Ollama. |
| **llama.cpp** | ~98K | Low-level inference engine. Ollama wraps it with a better UX. No reason to use directly unless you're optimizing inference performance. |
| **Leon** | MIT | Self-hosted personal assistant. Cool concept but small community and limited skill ecosystem. Not mature enough to rely on. |

---

## Quick Reference: Priority Actions

1. **This week:** Install OpenCode as a Claude Code backup. Set up SearXNG (Docker, 15 min). Install Crawl4AI (`pip install crawl4ai`).
2. **This month:** Set up n8n and build your first automation (YNAB spending alert or job board scraper). Evaluate browser-use for hawker's marketplace flows.
3. **This quarter:** Integrate Mem0 into focuspipe for persistent context. Evaluate Khoj for personal knowledge management. Test Career-Ops for CareerOS integration.
4. **Ongoing:** Watch ActivityWatch + Dayflow for focuspipe input signals. Monitor OpenHands for autonomous issue resolution on your repos.

---

*Analysis compiled from: bitdoze.com, fungies.io, glukhov.org, firecrawl.dev, pinggy.io, findarepo.com, vellum.ai, dev.to, scored.tools, sim.ai, amux.io, stackpicks.dev, letsdatascience.com, and direct project documentation. Star counts are approximate snapshots as of November 2026 — verify on GitHub for current figures.*
