# Good AI List — Top Repos Analysis for Vitalik

> **Source:** [goodailist.com](https://goodailist.com/repos) — Chip Huyen's curated leaderboard tracking 18K+ AI open-source repos, ranked by stars, growth, and activity. Cross-referenced with multiple 2026 roundups.
>
> **Note:** goodailist.com was not directly accessible during this research session. This analysis is compiled from the site's X account (@goodailist), cross-referenced with 8+ independent 2026 AI repo rankings, and direct research on each tool. Star counts are approximate and sourced from the most recent available data (Q3-Q4 2026).
>
> **Date:** November 2026

---

## 1. Tools to Integrate NOW

These tools would immediately improve your daily workflow. Ordered by impact.

### 1.1 Career-Ops — AI Job Search Pipeline
- **GitHub:** [santifer/career-ops](https://github.com/santifer/career-ops)
- **Stars:** ~24K+ (fast-growing since April 2026 launch)
- **License:** MIT

**What it does:** An open-source, CLI-based AI job search system built on Claude Code. It scores job offers A-F, generates ATS-optimized CVs per application, scans job portals, and provides a Go-based TUI dashboard. One user reportedly processed 740+ applications and landed a Head of AI role.

**Why it's relevant:** This is essentially what your CareerOS project aims to be. Either fork it as a foundation or study its architecture — it already solved the Claude Code integration, job scoring, and resume tailoring problems. It's MIT-licensed and built specifically around Claude Code skills.

**Integration effort:** Weekend project to evaluate; significant if forking/adapting for CareerOS.

**Concerns:** Relies on Claude API costs for scoring/generation. Check if its job board scrapers still work — these break frequently.

---

### 1.2 n8n — Self-Hosted Workflow Automation
- **GitHub:** [n8n-io/n8n](https://github.com/n8n-io/n8n)
- **Stars:** ~190K+
- **License:** Sustainable Use License (free self-hosted, source-available)

**What it does:** Visual workflow automation with 400+ integrations, a first-class AI Agent node, and native MCP server support (Public Preview since early 2026). Self-hosted via Docker. Think Zapier but local, free, and with AI agent capabilities built in.

**Why it's relevant:** This is the glue layer for everything you build. Connect your marketplace monitoring (hawker) to Telegram alerts, automate job search pipelines, trigger focus reminders, sync data between tools — all without writing custom integration code. The MCP server means Claude Code/Desktop can directly build and trigger your workflows.

**Integration effort:** Weekend project for basic setup; ongoing for building workflows.

**Concerns:** The Sustainable Use License restricts competing with n8n's cloud offering. Resource usage on a Mac Mini is manageable but not negligible (Node.js + PostgreSQL). The MCP integration is still in Public Preview.

---

### 1.3 Vane (formerly Perplexica) — Self-Hosted AI Search
- **GitHub:** [ItzCraworkerzyProxy/Vane](https://github.com/ItzCrazyKns/Perplexica) (check for rename)
- **Stars:** ~29K+
- **License:** MIT

**What it does:** A self-hosted AI-powered search engine that combines SearXNG (privacy-respecting metasearch) with local LLMs via Ollama. Ask questions, get sourced answers — like Perplexity but running entirely on your hardware.

**Why it's relevant:** Privacy-first search that fits your local-first philosophy. Use it for research without feeding queries to Google. Pairs with Ollama (which you should also run). Useful for CareerOS research, hawker market research, and general development questions.

**Integration effort:** Weekend project (Docker Compose + Ollama).

**Concerns:** Answer quality depends on your local model. A Mac Mini M-series with 16GB+ RAM handles small models well. SearXNG instances occasionally get throttled by upstream engines.

---

### 1.4 Screenpipe — Local Screen & Audio Recording with AI
- **GitHub:** [mediar-ai/screenpipe](https://github.com/mediar-ai/screenpipe)
- **Stars:** ~16.7K+
- **License:** MIT (core/CLI); desktop app is a one-time purchase (~$300-400)

**What it does:** Continuously records your screen and audio, stores everything locally in SQLite, runs OCR and transcription, and makes your entire computer history searchable. Has an MCP server for Claude Desktop, a REST API, and privacy filters that scrub sensitive data (credit cards, API keys) before storage.

**Why it's relevant:** You're already evaluating this — and you should commit. For ADHD monitoring, this is the passive data collection layer focuspipe needs. Instead of building screen capture from scratch, integrate Screenpipe's API. It records what you're actually doing (not what you planned to do), which is the ground truth focuspipe needs for focus analysis.

**Integration effort:** Drop-in for personal use; significant integration work to build focuspipe analytics on top of its API/database.

**Concerns:** CPU/storage usage for continuous recording. The commercial license for the desktop app adds cost. Privacy implications of recording everything — good that it's local, but be careful with shared screens. April 2026 security whitepaper exists — review it.

---

### 1.5 browser-use — AI Browser Automation
- **GitHub:** [browser-use/browser-use](https://github.com/browser-use/browser-use)
- **Stars:** ~118K+
- **License:** MIT

**What it does:** A Python framework for building AI agents that control web browsers via Playwright. Navigate sites, fill forms, extract data, complete multi-step workflows — all driven by LLM reasoning. Ships with both local and cloud MCP servers.

**Why it's relevant:** This is the engine hawker needs. Instead of writing brittle Playwright scripts per marketplace, let an LLM agent handle navigation, adapt to UI changes, and extract listings intelligently. Also useful for CareerOS — automated job application filling, portal navigation, etc.

**Integration effort:** Weekend project for proof-of-concept; significant for production hawker integration.

**Concerns:** Marketplace ToS compliance — automated browsing may violate terms. Anti-bot detection is aggressive on Amazon, eBay, etc. in 2026. The "stealth" features raise ethical questions. Test carefully.

---

### 1.6 Composio — Tool Integration Platform for AI Agents
- **GitHub:** [ComposioHQ/composio](https://github.com/ComposioHQ/composio)
- **Stars:** ~25-29K
- **License:** MIT

**What it does:** A single MCP endpoint that connects AI agents to 1,000+ SaaS apps with managed auth. Instead of building OAuth flows for each service, Composio handles authentication, tool schemas, and execution. Works with Claude Code, Claude Desktop, and other MCP clients.

**Why it's relevant:** Saves enormous integration time. Need Claude Code to read your Gmail, check your calendar, update Linear, post to Slack, or interact with YNAB? Composio handles the auth plumbing. Free tier includes 20K tool calls/month.

**Integration effort:** Drop-in for basic use via MCP.

**Concerns:** **MAJOR: May 2026 security breach exposed 5,001 GitHub tokens and 5,241 API keys.** Review remediation thoroughly before trusting it with any credentials. You're trusting a third party with your OAuth tokens, which conflicts with your local-first philosophy. Consider it only for non-sensitive integrations, and only after verifying the breach has been fully addressed.

---

### 1.7 Mem0 — Persistent Memory for AI Agents
- **GitHub:** [mem0ai/mem0](https://github.com/mem0ai/mem0)
- **Stars:** ~62K+
- **License:** Apache 2.0

**What it does:** A memory layer that gives AI agents persistent, personalized context across sessions. Memories are scoped to users, sessions, or agents. Self-hostable via Docker Compose with Ollama + pgvector for fully local operation.

**Why it's relevant:** Your AI agents (focuspipe, hawker, CareerOS) all need to remember context across sessions. Mem0 provides this without building it from scratch. Self-hosted mode with Ollama keeps everything local. Could power focuspipe's understanding of your work patterns over time.

**Integration effort:** Weekend project for basic setup; moderate for deep integration.

**Concerns:** Self-hosted feature parity with cloud version is disputed — evaluate carefully. Needs PostgreSQL + pgvector infrastructure.

---

### 1.8 Codebase Memory MCP — Persistent Codebase Knowledge Graph
- **GitHub:** [DeusData/codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp)
- **Stars:** ~28-44K (conflicting sources, fast-growing)
- **License:** MIT

**What it does:** An MCP server that indexes your codebase into a persistent knowledge graph that survives session restarts and context compaction. Supports 158 languages, ships as a single static binary (Go/C), zero runtime dependencies. Auto-detects Claude Code, Codex, Cursor, and Windsurf. Has a UI mode for browsing the graph.

**Why it's relevant:** This directly upgrades your Claude Code workflow. When you're jumping between focuspipe, hawker, and CareerOS across sessions, the codebase context gets lost. This tool maintains a persistent understanding of your codebases, so Claude Code starts each session already knowing the architecture, patterns, and conventions.

**Integration effort:** Drop-in. Install and it auto-detects Claude Code.

**Concerns:** Relatively new project — verify stability. Not supported on Windows (use WSL2), but you're on macOS so no issue.

---

### 1.9 Playwright MCP — Official Browser Automation MCP
- **GitHub:** [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp)
- **Stars:** Growing (Microsoft-backed)
- **License:** Open source (Microsoft)

**What it does:** Microsoft's official MCP server for Playwright browser automation. Uses structured accessibility snapshots instead of pixels for LLM-friendly browser interaction. One-line install: `claude mcp add playwright -- npx @playwright/mcp@latest`.

**Why it's relevant:** You already use Playwright heavily. This bridges it directly into your Claude Code sessions — tell Claude Code to test a page, fill a form, or verify a deployment, and it drives a real browser. Drop-in enhancement for your existing workflow.

**Integration effort:** Drop-in. One command to add to Claude Code.

**Concerns:** Requires Node.js 18+. Accessibility snapshots may miss visually-rendered content that's not in the DOM accessibility tree.

---

### 1.10 Crawl4AI — LLM-Optimized Web Crawler
- **GitHub:** [unclecode/crawl4ai](https://github.com/unclecode/crawl4ai)
- **Stars:** ~81K+
- **License:** Apache 2.0

**What it does:** A Python web crawler optimized for AI/LLM pipelines. Built on Playwright + asyncio. Produces structured, LLM-ready output with BFS/DFS/best-first crawling strategies, JavaScript execution, and anti-bot detection handling.

**Why it's relevant:** For hawker and CareerOS, you need reliable web data extraction. Crawl4AI is the better self-hosted choice over Firecrawl — Apache 2.0 license (vs Firecrawl's AGPL), more features in the open-source version, and no Docker container overhead for basic use.

**Integration effort:** Weekend project (pip install).

**Concerns:** Anti-bot arms race means scrapers need ongoing maintenance. Check marketplace ToS compliance.

---

### 1.11 Khoj — Local-First AI Second Brain
- **GitHub:** [khoj-ai/khoj](https://github.com/khoj-ai/khoj)
- **Stars:** ~37K+
- **License:** AGPL-3.0

**What it does:** A self-hosted AI personal assistant and "second brain." Indexes your notes, documents, images (Markdown, PDF, Obsidian, Notion). Provides conversational search over your private data. Supports local models (llama3, qwen, gemma, mistral) and cloud APIs.

**Why it's relevant:** ADHD makes it hard to remember where you put things or what you decided last week. Khoj acts as a searchable extension of your memory — ask it "what did I decide about the hawker pricing strategy?" and it finds the answer in your notes. Complements focuspipe: Khoj indexes knowledge, focuspipe monitors activity.

**Integration effort:** Weekend project (Docker, self-hosted). Note: hosted service (app.khoj.dev) shut down April 2026 — self-hosting is the only path.

**Concerns:** AGPL-3.0 license restricts commercial use of modifications. Community support may decline after hosted service shutdown. Latest beta: March 2026.

---

### 1.12 LiteLLM — Unified LLM API Gateway
- **GitHub:** [BerriAI/litellm](https://github.com/BerriAI/litellm)
- **Stars:** ~55K+
- **License:** MIT

**What it does:** A self-hosted proxy that provides one OpenAI-compatible API endpoint across 100+ LLM providers. Includes virtual keys, spend tracking, budgets, routing, fallbacks, and guardrails. Routes to both cloud APIs (Claude, GPT, Gemini) and local models (Ollama, vLLM).

**Why it's relevant:** As a heavy Claude user building multiple AI projects, you need cost visibility and the ability to swap models. LiteLLM lets you track spend across all your projects, set budgets, and route cheaper tasks to local models while keeping Claude for complex reasoning. One API for everything.

**Integration effort:** Weekend project for basic proxy; moderate for full budget/routing setup.

**Concerns:** Adds a proxy hop (minimal latency if co-located). Another service to maintain. Overkill if you're only using Claude, but valuable as your model usage diversifies.

---

## 2. Tools Worth Watching

Not ready to integrate today, but keep on your radar.

### 2.1 Goose (by Block) — MCP-Native AI Agent
- **GitHub:** [aaif-goose/goose](https://github.com/block/goose) (moved to Linux Foundation AAIF)
- **Stars:** ~50K+
- **License:** Apache 2.0

**What it does:** Block's open-source AI agent where every extension is an MCP server. Supports 30+ LLM providers, 70+ extensions, runs as CLI/desktop/API. Block scaled it to 60% of their company.

**Why it matters:** If you ever want an alternative to Claude Code as your primary agent, Goose is the strongest MCP-native contender. Its extension model aligns with your MCP-heavy workflow. Watch for maturity under AAIF governance.

**Why wait:** You're already deep in Claude Code. Switching agents mid-stream adds friction. Evaluate when you hit a Claude Code limitation.

---

### 2.2 OpenClaw — Self-Hosted Personal AI Assistant
- **GitHub:** [openclaw/openclaw](https://github.com/openclaw/openclaw)
- **Stars:** ~250K+ (unverified — treat with skepticism)
- **License:** MIT

**What it does:** A personal AI assistant platform you run locally. Formerly ClawdBot/Moltbot. Connects to MCP servers, supports local config, and integrates with messaging platforms (WhatsApp, Telegram, Slack, Discord, Signal).

**Why it matters:** Could serve as a unified interface for all your AI agents — one assistant that routes to focuspipe, hawker, or CareerOS as needed, accessible via Telegram or Signal.

**Why wait:** The star count claims are suspiciously high and inconsistent across sources. The project went through two renames in two months (late 2025 to Jan 2026), suggesting instability. Evaluate after it settles.

---

### 2.3 Mastra — TypeScript AI Agent Framework
- **GitHub:** [mastra-ai/mastra](https://github.com/mastra-ai/mastra)
- **Stars:** ~30K+ (estimate)
- **License:** MIT (check repo)

**What it does:** A TypeScript-first framework for building AI agents with first-class MCP support, tool definitions, memory, and workflows. Think LangChain but TypeScript-native and less bloated.

**Why it matters:** If you're building agent features in TypeScript (which focuspipe likely uses), Mastra gives you a cleaner foundation than rolling your own agent loops.

**Why wait:** Your Claude Code workflow already handles most agent tasks. Evaluate when you need a framework for multi-step autonomous agents in production.

---

### 2.4 Activepieces — Open-Source Automation (Zapier Alternative)
- **GitHub:** [activepieces/activepieces](https://github.com/activepieces/activepieces)
- **Stars:** ~15K+ (estimate)
- **License:** MIT (core)

**What it does:** A self-hosted, MIT-licensed workflow automation platform. Like n8n but with a friendlier UI and true MIT license (no source-available restrictions).

**Why it matters:** If n8n's license concerns you, Activepieces is the MIT alternative. Better for embedding automation in your own products.

**Why wait:** n8n has a larger ecosystem and more integrations. Switch only if the license becomes a blocker.

---

### 2.5 Super Productivity — ADHD-Friendly Task Manager
- **GitHub:** [johannesjo/super-productivity](https://github.com/johannesjo/super-productivity)
- **Stars:** ~21K+
- **License:** MIT

**What it does:** Free, open-source task manager with time tracking, Pomodoro timer, break reminders, visual scheduling, and integrations with Jira, GitHub Issues, GitLab, CalDAV. Fully offline, no telemetry. Positions itself as ADHD-friendly with visual timers and low-friction capture.

**Why it matters:** Could replace or complement TickTick for task management with better privacy and ADHD-specific features. The time tracking data could feed into focuspipe.

**Why wait:** You already use TickTick. Evaluate during a frustration moment with TickTick, or when focuspipe needs task-level time data.

---

### 2.6 Semantic Anchors — Better LLM Prompting
- **GitHub:** [LLM-Coding/Semantic-Anchors](https://github.com/LLM-Coding/Semantic-Anchors)
- **Stars:** ~288 (small but has Claude Code plugin)
- **License:** Check repo

**What it does:** A curated catalog of terms and concepts for prompting LLMs — shorthand references that let developers express complex coding concepts concisely. Has plugins for Claude Code, Codex, Gemini CLI, Cursor, Copilot, Windsurf.

**Why it matters:** Small effort, immediate payoff. Install the Claude Code plugin and your prompts become more precise. Listed on Good AI List since October 2026.

**Why wait:** It's small enough to install now, but the concept is still evolving. Try it when you have a slow afternoon.

---

### 2.7 Strix — AI Security Testing
- **GitHub:** [usestrix/strix](https://github.com/usestrix/strix)
- **Stars:** ~24-42K
- **License:** Apache 2.0

**What it does:** Open-source AI penetration testing tool. Bring-your-own-LLM, CI/CD integration via GitHub Actions, partnership with Caido for HTTP interception.

**Why it matters:** As you build user-facing tools (hawker, CareerOS), security testing becomes important. Strix can run automated security scans in your CI pipeline.

**Why wait:** Not needed until you have production-facing services. Add to CI when shipping.

---

### 2.8 promptfoo — LLM Evaluation & Testing
- **GitHub:** [promptfoo/promptfoo](https://github.com/promptfoo/promptfoo)
- **Stars:** ~10K+ (estimate)
- **License:** MIT

**What it does:** CLI tool for evaluating LLM prompts and models. Run test suites against different models, compare outputs, catch regressions. Supports Claude, GPT, local models.

**Why it matters:** As you build more AI features into focuspipe/hawker/CareerOS, you need automated testing for your prompts. promptfoo prevents prompt regressions.

**Why wait:** Only valuable once you have stable prompts to protect. Add it when your AI features reach production quality.

---

## 3. Building Blocks for Your Projects

### For focuspipe (ADHD Monitoring)

| Tool | Use Case | Integration |
|------|----------|-------------|
| **Screenpipe** | Passive screen/audio capture — the data collection layer | Use its SQLite DB and REST API as focuspipe's input source |
| **Khoj** | Searchable "second brain" for notes, decisions, context | Reduces cognitive load — ask it what you decided last week |
| **Mem0** | Long-term memory of work patterns, focus sessions, context | Store and retrieve user behavioral patterns across sessions |
| **Super Productivity** | Task-level time tracking data | Import/sync task completion and time data |
| **Ollama + small models** | Local inference for focus classification | Run focus/distraction classification locally without API costs |
| **n8n** | Workflow triggers (break reminders, focus alerts) | Trigger notifications based on focuspipe analysis |
| **Codebase Memory MCP** | Persistent project context across Claude Code sessions | Reduces context-switching cost when jumping between projects |

### For hawker (Marketplace Bot)

| Tool | Use Case | Integration |
|------|----------|-------------|
| **browser-use** | AI-driven marketplace browsing and listing extraction | Core automation engine — replaces brittle Playwright scripts |
| **Crawl4AI** | Structured web data extraction for LLMs | Primary scraping engine (Apache 2.0, better than Firecrawl for self-hosted) |
| **Playwright MCP** | Browser automation from Claude Code sessions | Quick testing and prototyping of marketplace interactions |
| **SearXNG** | Privacy-respecting market research searches | Search for comparable listings without tracking |
| **n8n** | Orchestrate scraping → analysis → listing → notification | Workflow backbone connecting all hawker components |
| **LiteLLM** | Route cheap analysis to local models, complex to Claude | Manage API costs across many listing analyses |

### For CareerOS (Job Search)

| Tool | Use Case | Integration |
|------|----------|-------------|
| **Career-Ops** | Reference architecture or direct fork | Study/adapt its Claude Code skills, scoring system, ATS CV generation |
| **browser-use** | Automated job portal navigation and application filling | Handle the browser automation layer |
| **Docling** | Parse job descriptions, company reports, resumes | IBM's MIT-licensed doc parser — PDF, DOCX, PPTX, HTML, images → structured data |
| **n8n** | Job alert → score → apply → track pipeline | Orchestrate the full job search workflow |
| **Mem0** | Remember company research, application history, preferences | Persist context across job search sessions |
| **Composio** | Connect to Gmail, LinkedIn, calendars for scheduling | Handle OAuth for email/calendar integrations (review May 2026 security breach first) |

---

## 4. Grant Inspiration

These repos suggest grant-worthy project ideas in adjacent spaces.

### 4.1 AI-Powered ADHD Digital Twin
**Inspired by:** Screenpipe + Mem0 + focuspipe concept

Build a local-first "digital twin" of your work self that passively learns your patterns, predicts focus drops before they happen, and proactively restructures your environment (closes distracting tabs, starts focus music, blocks notifications). No existing tool does this end-to-end. Screenpipe provides the perception layer, Mem0 the memory, and the intelligence layer is the grant-worthy part.

**Grant angle:** Mental health tech + privacy-first AI + ADHD accessibility.

### 4.2 Privacy-Preserving AI Agent Marketplace
**Inspired by:** Composio + MCP ecosystem growth

Build an open marketplace for MCP servers/tools where agents can discover and use capabilities, but all data stays local. Think "app store for AI agents" but privacy-first. Each tool runs locally, auth stays on-device, and the marketplace only indexes capabilities.

**Grant angle:** Open-source AI infrastructure + privacy.

### 4.3 Local-First AI Career Coach
**Inspired by:** Career-Ops + Mem0 + your CareerOS vision

Go beyond Career-Ops' batch-processing model. Build a persistent AI career coach that understands your full professional history, tracks the job market in real-time, identifies skill gaps, suggests learning paths, and proactively surfaces opportunities — all running locally. Career-Ops handles the application pipeline; this handles the strategy.

**Grant angle:** Economic mobility + AI accessibility + privacy.

### 4.4 Federated Marketplace Intelligence
**Inspired by:** hawker concept + SearXNG's federated model

Build an open protocol for marketplace intelligence that respects platform ToS. Instead of scraping, build partnerships with marketplace APIs and provide AI-powered analysis as a privacy-preserving service. Multiple sellers contribute anonymized trend data to improve everyone's analysis without exposing individual strategies.

**Grant angle:** Small business empowerment + ethical AI + federated data.

---

## 5. Skip List

Popular repos that are NOT relevant to your setup.

| Repo | Stars | Why Skip |
|------|-------|----------|
| **ComfyUI** | ~80K+ | Image generation workflow tool. Not relevant unless you're doing AI art/design. |
| **Stable Diffusion WebUI** | ~150K+ | Same — image generation. Not in your workflow. |
| **llama.cpp** | ~98K+ | Low-level C++ LLM inference. Ollama wraps it for you — use Ollama instead of touching this directly. |
| **vLLM** | ~50K+ | High-throughput LLM serving for production. Overkill for solo dev; Ollama handles your local model needs. |
| **LangChain** | ~110K+ | Python-heavy, overly complex for your TypeScript stack. If you need a framework, use Mastra instead. |
| **AutoGPT** | ~183K+ | Ambitious autonomous agent project that never delivered on its promise. Star count is legacy hype, not current utility. |
| **CrewAI** | ~30K+ | Multi-agent orchestration — interesting concept but premature for your projects. You don't need agents managing agents yet. |
| **Dify** | ~80K+ | Visual AI app builder. You're a developer who codes — visual builders add a layer of indirection you don't need. |
| **Langflow** | ~60K+ | Same as Dify — visual builder, not for code-first developers. |
| **Flowise** | ~40K+ | Another visual AI builder. Same reasoning. |
| **Open WebUI** | ~132K+ | Web UI for local LLMs. Useful if you chat with local models regularly, but you primarily use Claude Code. Consider only if you go heavy on local models via Ollama. |
| **OpenHands** | ~55K+ | AI coding agent with web GUI. You already have Claude Code, which does this better for your workflow. |
| **Aider** | ~35K+ | Terminal AI coding assistant. Redundant with Claude Code. |
| **Cline** | ~40K+ | VS Code AI agent. You use Ghostty + tmux, not VS Code. |
| **Continue** | ~25K+ | Open-source IDE AI extension. Same — wrong IDE. |
| **Haystack** | ~25K+ | RAG framework by deepset. Python-heavy, enterprise-focused. Overkill for your projects. |
| **LlamaIndex** | ~45K+ | Data framework for LLM apps. Python-heavy, and your RAG needs are simple enough for direct implementation. |
| **RAGFlow** | ~35K+ | Enterprise RAG engine. Way too heavy for solo dev. |
| **Docling** | ~40K+ | IBM's document parser (MIT). Moved to building blocks — useful for CareerOS resume/job desc parsing. Has MCP server. |
| **marker** | ~15K+ | PDF to markdown (GPL-3.0). Docling is the better choice — MIT license, more formats, MCP support. |
| **yt-dlp** | ~100K+ | Video downloader. Useful utility but not AI-relevant for your workflow. |
| **OpenBB** | ~40K+ | Financial data terminal. Not relevant unless you're doing quantitative analysis or trading. |
| **Taipy** | ~20K+ | Python AI app builder. TypeScript stack mismatch. |

---

## Summary: Priority Action Items

### This Week (drop-in, < 1 hour each)
1. **Install Codebase Memory MCP** — persistent project context across Claude Code sessions (one command)
2. **Install Playwright MCP** — browser automation from Claude Code (`claude mcp add playwright -- npx @playwright/mcp@latest`)
3. **Commit to Screenpipe** — install and start collecting data for focuspipe

### This Month (weekend projects)
4. **Set up n8n** (Docker on Mac Mini) — becomes the automation backbone for all projects
5. **Set up Ollama + Vane** — local-first AI search stack
6. **Study Career-Ops** — evaluate as foundation for CareerOS
7. **Prototype browser-use** — test for hawker marketplace automation

### This Quarter (moderate effort)
8. **Set up LiteLLM** — start tracking AI spend across projects
9. **Integrate Mem0** — add persistent memory to your AI tools
10. **Set up Khoj** — self-hosted second brain for notes and decisions
11. **Add promptfoo** — when your AI features stabilize
12. **Evaluate Goose** — as Claude Code complement/alternative

### Key Insight
**No good open-source ADHD focus monitoring tool exists.** The research across all major repo lists and ADHD tool roundups confirms this gap. focuspipe has real market potential — the closest things are Screenpipe (passive recording, no focus analysis), Super Productivity (task management, not monitoring), and commercial apps like Focus Bear (distraction blocking, not AI-powered analysis). An open-source, local-first, AI-powered ADHD focus monitor is a genuinely novel project.

---

## Methodology

This analysis was compiled on November 9, 2026 from:
- [Good AI List](https://goodailist.com/repos) — Chip Huyen's 18K+ repo tracker (accessed via secondary sources due to network restrictions)
- [Good AI List Bot on X](https://x.com/goodailist) — daily trending repo updates
- [bitdoze.com — 60+ Top AI GitHub Repos](https://www.bitdoze.com/top-ai-github-repos/)
- [firecrawl.dev — Best Trending GitHub Repos](https://www.firecrawl.dev/blog/best-github-repos)
- [fungies.io — Top 20 AI Agent Repos](https://fungies.io/top-github-repositories-ai-agent-frameworks-2026/)
- [analyticsvidhya.com — Trending July 2026](https://www.analyticsvidhya.com/blog/2026/07/trending-ai-github-repositories/)
- [nocobase.com — Top 20 AI Projects](https://www.nocobase.com/en/blog/best-open-source-ai-projects-github-2026)
- [findarepo.com — 30 Best AI Agent Repos](https://findarepo.com/categories/ai-agents/)
- Direct WebSearch research on 40+ individual tools

Star counts are approximate and reflect the most recent available data from Q3-Q4 2026. Verify on GitHub before making decisions.
