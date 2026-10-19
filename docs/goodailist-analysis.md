# Top Open-Source AI Repos: Analysis for Vitalik

**Date:** 2026-10-19
**Source:** Curated from goodailist.com/repos cross-referenced with multiple 2026 ranking sources (DataAIHub, Bitdoze, Fungies, NocoBase, Analytics Vidhya, Firecrawl)
**Context:** Solo AI developer, ADHD, privacy-conscious, local-first, Mac M-series, building focuspipe/hawker/CareerOS

> Note: goodailist.com was unreachable at analysis time. This report synthesizes data from 15+ ranking sources covering the same repos, verified against GitHub where possible.

---

## 1. Tools to Integrate NOW

These tools would immediately improve your daily workflow with minimal setup.

### 1.1 ActivityWatch
- **GitHub:** https://github.com/ActivityWatch/activitywatch
- **Stars:** ~18.2k | **License:** MPL-2.0
- **What it does:** Fully local, open-source screen time and activity tracker. Logs active apps, window titles, and browser activity. Data never leaves your device. Stores everything in a local SQLite database with a REST API on `localhost:5600`. Runs on macOS, Linux, Windows, and Android.
- **Why for Vitalik:** This is the missing data layer for focuspipe. ActivityWatch passively collects exactly the behavioral data focuspipe needs to detect focus patterns, context-switching, and doom-scrolling. The REST API means focuspipe can pull data without any user action (critical for ADHD). It's the privacy-respecting foundation Screenpipe promises but with a simpler, proven architecture.
- **Integration effort:** Drop-in. Install, configure watchers, query the API.
- **Concerns:** Cross-device sync is still WIP. No built-in AI analysis layer (but that's what focuspipe would provide). macOS permissions can be finicky with accessibility APIs.

### 1.2 Ollama (with MLX backend)
- **GitHub:** https://github.com/ollama/ollama
- **Stars:** ~100k+ | **License:** MIT
- **What it does:** Run LLMs locally with a single command. Version 0.19 introduces an MLX backend targeting M1-M5 chips for significantly faster inference on Apple Silicon. Supports hundreds of models. Exposes an OpenAI-compatible API.
- **Why for Vitalik:** Foundation for running local AI in all three projects. focuspipe can use local models for on-device behavioral analysis without sending screen data anywhere. hawker can use it for offline product description generation. CareerOS can do resume scoring locally. The MLX backend on your Mac Mini M-series is a meaningful speed boost.
- **Integration effort:** Drop-in. `brew install ollama`, pull a model, point your apps at `localhost:11434`.
- **Concerns:** RAM is the hard limit. 16GB handles 7B-13B models well. 64GB+ needed for 70B+. Quality gap vs Claude/GPT-4 is real for complex tasks, so use local for privacy-sensitive operations and cloud APIs for quality-critical ones.

### 1.3 career-ops
- **GitHub:** https://github.com/santifer/career-ops
- **Stars:** ~9k-69k (varies by source) | **License:** MIT
- **What it does:** An AI job search system built as Claude Code skills. 12 modes including job evaluation (A-F scoring across 10 dimensions), ATS-optimized CV generation per listing, batch URL processing (122+ in parallel), and application tracking. Scans 45+ company career portals (Ashby, Lever, Wellfound). Never auto-sends anything.
- **Why for Vitalik:** This is essentially what CareerOS is trying to build. Rather than building from scratch, fork it or integrate its skill architecture. The creator used it to evaluate 740+ offers, apply to 66, get 12 interviews, and land a Head of Applied AI role. It already runs on Claude Code (your primary tool) and follows the agent-skill standard.
- **Integration effort:** Weekend project to install and customize. Longer to deeply integrate with CareerOS's unique features.
- **Concerns:** Heavy dependence on Claude Code means API costs scale with usage. The skill architecture means it's tightly coupled to the Claude Code workflow. Verify the current maintenance status.

### 1.4 browser-use
- **GitHub:** https://github.com/browser-use/browser-use
- **Stars:** ~99k+ | **License:** MIT
- **What it does:** Python library that gives AI agents real browser control via Playwright. Handles form filling, navigation, data extraction, and multi-step web workflows. Works with Claude, GPT, Gemini, and local models through Ollama. Supports both local and cloud browser backends.
- **Why for Vitalik:** Direct building block for hawker's marketplace automation. Instead of writing brittle Playwright selectors for each marketplace, browser-use lets the AI agent navigate pages using natural language. Combined with Ollama for privacy, you can automate listing creation, price monitoring, and competitor analysis without sending marketplace data to the cloud.
- **Integration effort:** Weekend project. Python library, pip install, works with your existing Playwright setup.
- **Concerns:** AI-driven automation is ~85-95% reliable vs 99%+ for hard-coded selectors. Token costs per action when using cloud models. Marketplace TOS compliance is your responsibility.

### 1.5 n8n (Self-Hosted)
- **GitHub:** https://github.com/n8n-io/n8n
- **Stars:** ~199k | **License:** Sustainable Use (fair-code, free for non-competing use)
- **What it does:** Visual workflow automation platform. Self-hostable via Docker. 400+ integrations including AI Agent nodes that support Ollama, Anthropic, OpenAI. Connects to practically everything: GitHub, Linear, email, Telegram, webhooks, databases, APIs.
- **Why for Vitalik:** The glue layer between all your tools. Set up flows like: ActivityWatch detects 2hrs of unfocused browsing -> n8n triggers focuspipe intervention -> sends Telegram notification. Or: new TickTick task tagged "hawker" -> n8n triggers marketplace listing workflow. Or: Linear issue closed -> n8n updates CareerOS tracking. Self-hosted means your automation data stays on your Mac Mini.
- **Integration effort:** Weekend project for Docker setup + first workflows. Ongoing as you build more automations.
- **Concerns:** Fair-code license prohibits building a competing automation platform (not relevant to you). Self-hosting means you manage updates. Can become a time-sink if you over-automate (ADHD trap: building workflows instead of doing work).

---

## 2. Tools Worth Watching

Not ready to integrate today, but promising for your use case.

### 2.1 Screenpipe
- **GitHub:** https://github.com/mediar-ai/screenpipe
- **Stars:** ~16.7k+ | **License:** MIT
- **What it does:** Records screen and audio continuously, extracts text via accessibility APIs + OCR, transcribes audio with Whisper, stores everything in local SQLite. Has an MCP server for Claude Code integration. Per-app AI access controlled through YAML permissions.
- **Why to watch:** More ambitious than ActivityWatch - captures actual screen content, not just window titles. The MCP server means Claude Code could query your screen history directly. But it's heavier, newer, and less proven.
- **Current status:** You're already evaluating it. The $400 lifetime license vs free self-hosting split suggests the project is commercializing. Watch for: stability on macOS, resource usage (continuous screen capture eats CPU/battery), and whether the open-source version stays full-featured.
- **Wait because:** Still maturing. ActivityWatch covers 80% of the use case with less resource overhead. Revisit in 3-6 months.

### 2.2 Hermes Agent (Nous Research)
- **GitHub:** https://github.com/nousresearch/hermes
- **Stars:** ~180k-225k | **License:** MIT (claimed, verify)
- **What it does:** Self-improving autonomous AI agent that runs as a persistent daemon. Creates reusable skills from experience, maintains persistent memory across sessions. Connects to 15+ messaging platforms (Telegram, Discord, Slack, WhatsApp, Signal). Works with any LLM provider including Ollama.
- **Why to watch:** The "self-improving" angle is interesting for ADHD tools - an agent that learns your patterns over time and gets better at interventions. The messaging platform integration could be the notification layer for focuspipe. Nous Research raised $90M Series B, so the project has runway.
- **Wait because:** At 225k stars in <8 months, it's hyped. Let the dust settle. The "autonomous daemon" model needs careful trust boundaries. Evaluate after 2-3 more releases when the community has stress-tested it.

### 2.3 OpenCode
- **GitHub:** https://github.com/opencode-ai/opencode (verify current home)
- **Stars:** Varies | **License:** MIT
- **What it does:** Open-source terminal AI coding agent. Supports 75+ LLM providers, auto-loads LSPs for better code intelligence, has client/server architecture for remote operation, multi-agent parallel support.
- **Why to watch:** A fully open-source Claude Code alternative means you could self-host your coding agent and use local models for sensitive codebases. The LSP integration for reducing AI mistakes is a meaningful differentiator.
- **Wait because:** You're deeply invested in Claude Code's ecosystem. Only worth switching if Claude Code's pricing becomes unsustainable or you need fully local operation. Keep it as a backup plan.

### 2.4 Stagehand
- **GitHub:** https://github.com/browserbase/stagehand
- **Stars:** ~8k-17k | **License:** MIT
- **What it does:** AI browser automation framework built on Playwright. Adds natural-language methods (`act`, `extract`, `observe`) with auto-caching and self-healing selectors. TypeScript, Python, and Go SDKs.
- **Why to watch:** More structured than browser-use for production workflows. The `extract` and `observe` methods are ideal for marketplace monitoring (hawker). Self-healing selectors mean less maintenance when marketplace UIs change.
- **Wait because:** browser-use is more mature and Python-native. Stagehand is better for TypeScript-heavy projects. Watch for the Python SDK to mature, then evaluate for hawker.

### 2.5 Maybe (Personal Finance)
- **GitHub:** https://github.com/maybe-finance/maybe
- **Stars:** Varies | **License:** Check repo
- **What it does:** Open-source, AI-powered personal finance platform covering budgeting, investing, and goal planning. Self-hostable.
- **Why to watch:** If YNAB ever becomes too limiting or you want more AI-driven financial insights, Maybe is the open-source alternative with the most momentum. Could integrate with your N26 data via CSV exports.
- **Wait because:** YNAB works for you now. Don't fix what isn't broken, especially with ADHD - tool-switching is a productivity trap.

---

## 3. Building Blocks for Your Projects

### For focuspipe (ADHD Monitoring)

| Repo | What It Provides | Integration Path |
|------|-------------------|------------------|
| **ActivityWatch** | Passive activity data collection (apps, windows, browser) | Primary data source. Query REST API for focus metrics |
| **Ollama** | Local LLM inference for behavioral analysis | Run pattern detection on activity data without cloud |
| **Screenpipe** | Rich screen content capture + audio transcription | Future upgrade path for deeper context (what you're reading, not just which app) |
| **n8n** | Workflow orchestration for interventions | Trigger notifications, block distracting apps, log interventions |
| **LangChain/LangGraph** | Agent framework for multi-step reasoning | Build the "focus coach" agent that interprets data and suggests actions |

### For hawker (Marketplace Bot)

| Repo | What It Provides | Integration Path |
|------|-------------------|------------------|
| **browser-use** | AI-driven browser automation | Navigate marketplace UIs, fill listing forms, extract competitor data |
| **Stagehand** | Structured browser automation with self-healing | Production-grade marketplace monitoring with less maintenance |
| **Firecrawl** | Web scraping to clean markdown/JSON | Extract product data, pricing, and descriptions from competitor listings |
| **Playwright** | Low-level browser control | Base layer you already use - browser-use and Stagehand build on it |
| **Ollama** | Local product description generation | Generate listing descriptions privately without sending product data to APIs |

### For CareerOS (Job Search)

| Repo | What It Provides | Integration Path |
|------|-------------------|------------------|
| **career-ops** | Complete job search skill system | Fork or integrate. Covers evaluation, CV generation, tracking |
| **Firecrawl** | Job posting scraping and extraction | Scrape career pages into structured data for analysis |
| **LangChain/LangGraph** | Agent orchestration | Multi-step job evaluation pipeline: scrape -> analyze -> score -> generate CV |
| **Resume Matcher** | ATS-style resume analysis | Validate generated CVs against job descriptions |
| **Ollama** | Local resume/cover letter generation | Draft sensitive career documents without cloud exposure |

---

## 4. Grant Inspiration

These repos and adjacent spaces suggest grant-worthy project ideas:

### 4.1 ADHD-Aware AI Productivity Agent
- **Inspired by:** ActivityWatch + Hermes Agent + focuspipe
- **Idea:** An open-source, privacy-first AI agent that passively monitors work patterns, detects ADHD-related focus loss in real-time, and provides evidence-based interventions. Unlike generic productivity tools, it would understand ADHD-specific patterns (hyperfocus vs. distraction spirals, time blindness, task paralysis).
- **Grant angle:** Digital mental health, accessibility, neurodiversity. NSF, NIMH, or EU Horizon funding for assistive technology.
- **Differentiator:** Existing tools are either cloud-based (privacy issue) or require discipline to use (anti-ADHD). This would be fully local and fully passive.

### 4.2 Open-Source Career Intelligence Platform
- **Inspired by:** career-ops + CareerOS
- **Idea:** A community-maintained, open-source platform that aggregates and scores job listings using local AI, with privacy-preserving salary benchmarking across users (federated learning or differential privacy).
- **Grant angle:** Workforce development, economic mobility, open data. Department of Labor, EU Digital Skills programs.
- **Differentiator:** Current job search tools are either manual (LinkedIn) or cloud-dependent (career-ops needs Claude API). A federated approach protects individual salary data while building collective intelligence.

### 4.3 Privacy-First Personal AI Stack
- **Inspired by:** Ollama + ActivityWatch + Screenpipe + n8n
- **Idea:** A reference architecture and installer for a fully local AI-powered personal productivity stack. One-command setup on Mac/Linux that gives you: local LLM, activity tracking, workflow automation, and an AI assistant - all running on your hardware with zero cloud dependencies.
- **Grant angle:** Digital sovereignty, privacy, open-source infrastructure. NLnet, Mozilla Foundation, Sovereign Tech Fund.
- **Differentiator:** Individual tools exist but integration is hard. A turnkey local stack lowers the barrier for privacy-conscious users.

### 4.4 Neurodivergent-Friendly Marketplace Automation
- **Inspired by:** hawker + browser-use
- **Idea:** Tools that handle the executive-function-heavy parts of marketplace selling (listing creation, pricing, inventory, customer responses) through AI automation, designed specifically for neurodivergent sellers who struggle with the administrative overhead.
- **Grant angle:** Entrepreneurship accessibility, disability inclusion, microenterprise support.
- **Differentiator:** Marketplace tools assume neurotypical executive function. Purpose-built ADHD-friendly automation removes the barriers that cause neurodivergent sellers to abandon platforms.

---

## 5. Skip List

Popular repos that are NOT relevant to your setup:

| Repo | Stars | Why Skip |
|------|-------|----------|
| **OpenClaw** (~385k) | Personal AI agent with messaging integrations | Overlaps heavily with Claude Code, which you're already deep in. OpenClaw is broader but shallower. Switching cost is high for unclear benefit. |
| **PyTorch** (~102k) | Deep learning framework | You're building applications, not training models. Ollama and cloud APIs abstract this away. |
| **Hugging Face Transformers** (~163k) | Model library and inference | Same as PyTorch - too low-level for your use cases. Use Ollama or API providers instead. |
| **vLLM** (~88k) | High-throughput LLM serving | Server-side inference engine for high-concurrency deployments. You're running single-user local inference - Ollama with MLX is the right tool. |
| **Dify** (~133-151k) | LLM app builder with visual workflows | Impressive but heavyweight. You'd need to self-host a full platform when Claude Code + n8n covers the same ground. Dify shines for teams, not solo developers. |
| **LangFlow** (~146k) | Visual LLM workflow builder | Same concern as Dify - visual builders add overhead for a solo dev who's comfortable in the terminal. n8n is a better fit if you want visual flows. |
| **superpowers** (~150k+) | Agentic skills framework for coding agents | Enforces structured workflow (brainstorm, plan, worktree, subagents) on coding agents. Useful for teams, but for a solo dev it adds process overhead. Your CLAUDE.md + skills already serve this purpose. |
| **AutoGPT** (~183k) | Autonomous AI agent | The original "let GPT do everything" project. Impressive demo, unreliable in practice. Hermes Agent is the more mature version of this vision. |
| **llama.cpp** (~122k) | LLM inference engine | Important but low-level. Ollama wraps llama.cpp (and now MLX) with a better UX. Use Ollama unless you need bare-metal control for benchmarking. |
| **DeepSeek-V4** (trending) | Open LLM | A model, not a tool. Access it through Ollama or API providers. No need to interact with the repo directly. |

---

## Summary: Priority Action Plan

### This Week
1. **Install ActivityWatch** on your Mac Mini and MacBook. Let it collect baseline data for 1-2 weeks.
2. **Install Ollama** with the MLX backend. Pull a few models (Llama 3.3 8B for fast tasks, 70B for quality tasks if RAM allows).
3. **Clone career-ops** and run it against a few job listings. Evaluate whether to fork for CareerOS or build on top of it.

### This Month
4. **Set up n8n** on your Mac Mini via Docker. Create your first automation: ActivityWatch data -> focus analysis -> Telegram notification.
5. **Integrate browser-use** into hawker's prototype. Start with a simple marketplace monitoring task.

### This Quarter
6. **Connect ActivityWatch + Ollama** as focuspipe's data pipeline. Local behavioral analysis on local activity data.
7. **Evaluate Screenpipe** against ActivityWatch for focuspipe v2. Does the richer data justify the resource overhead?
8. **Build CareerOS** using career-ops' architecture as a foundation, adding your unique features on top.

---

*Analysis compiled from multiple 2026 ranking sources including DataAIHub Mid-Year Rankings, Bitdoze, Fungies, NocoBase, Analytics Vidhya, Firecrawl Blog, and Zimaspace. Star counts are approximate and vary by snapshot date. All tools should be verified against their GitHub repos for current status, licensing, and features before adoption.*
