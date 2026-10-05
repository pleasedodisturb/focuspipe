# Top Open-Source AI Repos Analysis

> Personal integration analysis for Vitalik's stack  
> Based on goodailist.com curated repos + extended research  
> Generated: 2026-10-05

---

## 1. Tools to Integrate NOW

These would immediately improve your daily workflow with minimal friction.

### Ollama
- **GitHub:** https://github.com/ollama/ollama
- **Stars:** ~170K | **License:** MIT
- **What it does:** One-command local LLM runner with an OpenAI-compatible API. Since v0.19 (March 2026), the Apple Silicon backend uses Apple's MLX framework instead of llama.cpp, delivering a significant speedup on M-series chips. Supports model pulling, running, and serving with a single binary.
- **Why for Vitalik:** You're already deep in Claude Code, but having a local LLM fallback is essential for privacy-sensitive tasks, offline work, and cost savings on repetitive operations. Your Mac Mini M-series with unified memory can run 70B parameter models. Use it for: local classification in focuspipe (is user distracted?), quick text processing in hawker (listing descriptions), and resume tailoring in CareerOS without sending data to APIs.
- **Integration effort:** Drop-in. `brew install ollama && ollama pull llama3.1` — running in 2 minutes.
- **Concerns:** Models consume disk (5-40GB each). RAM usage scales with model size. Not a Claude replacement for complex reasoning, but excellent for structured tasks.

---

### Crawl4AI
- **GitHub:** https://github.com/unclecode/crawl4ai
- **Stars:** ~66K | **License:** Apache 2.0
- **What it does:** The most-starred open-source LLM-friendly web crawler. Returns clean Markdown by default (fewer tokens than raw HTML), supports any LLM via LiteLLM including local Ollama models, and ships adaptive crawling with information-foraging algorithms. Fully self-hosted, no API keys required for the crawler itself.
- **Why for Vitalik:** Direct building block for **hawker** (marketplace scraping) and **CareerOS** (job board scraping). You're already using Playwright — Crawl4AI wraps Playwright with LLM-optimized output, anti-detection, and structured extraction. Instead of writing custom parsers per marketplace, point Crawl4AI at a listing page and get structured JSON back.
- **Integration effort:** Weekend project. pip install, configure extraction schemas for your target marketplaces. Replaces significant custom scraping code.
- **Concerns:** You handle anti-bot yourself (no proxy service included). For marketplace sites with aggressive bot detection, you'll need to add proxy rotation.

---

### browser-use
- **GitHub:** https://github.com/browser-use/browser-use
- **Stars:** ~50K+ | **License:** MIT
- **What it does:** Python framework that gives AI agents vision and control over a real browser. Model-agnostic (works with Claude, GPT-4o, Gemini, local models). Handles screenshot capture, DOM parsing, navigation, and multi-step task execution. The de facto standard for browser-control AI agents in 2026.
- **Why for Vitalik:** This is the automation backbone **hawker** needs for marketplace interactions that go beyond scraping — posting listings, responding to messages, managing inventory. Also useful for CareerOS to automate application flows on job boards that don't have APIs. Combined with Claude, it can handle complex multi-step workflows like "find similar listings, price competitively, post with optimized descriptions."
- **Integration effort:** Weekend project. Complements your existing Playwright setup rather than replacing it.
- **Concerns:** LLM API costs for vision-based automation add up. Use selectively for tasks that need visual understanding, not for everything.

---

### Mem0
- **GitHub:** https://github.com/mem0ai/mem0
- **Stars:** ~64K | **License:** Apache 2.0
- **What it does:** A memory layer for AI applications. Simple API: `add()` to store, `search()` to retrieve. Gives any AI agent persistent, searchable memory across sessions — remembers user preferences, past interactions, and context. Architecture-agnostic: works with LangChain, CrewAI, custom loops, or directly with Claude.
- **Why for Vitalik:** Your ADHD workflow needs tools that remember context without you having to re-explain. Mem0 could be the persistence layer for focuspipe (remember what apps/sites correlate with focus/distraction patterns), CareerOS (remember which companies you've applied to, interview notes, preferences), and hawker (remember pricing strategies, buyer preferences). It's the "second brain" that actually works passively.
- **Integration effort:** Weekend project. `pip install mem0ai`, configure with Ollama for fully local operation.
- **Concerns:** Requires a vector store (ships with Qdrant). Adds infrastructure complexity. Start with the in-memory store, graduate to persistent when you validate the value.

---

### n8n
- **GitHub:** https://github.com/n8n-io/n8n
- **Stars:** ~65K+ | **License:** Sustainable Use License (fair-code)
- **What it does:** Visual workflow automation platform with 420+ pre-built integrations. Self-hostable, with a node-based canvas for building automations. Connects to practically everything: email, APIs, databases, messaging, file systems. Has AI nodes for LLM integration built in.
- **Why for Vitalik:** The glue layer you're missing. Instead of writing custom scripts to connect your tools, n8n visually wires them together: "When a new eBay listing matches criteria → scrape with Crawl4AI → generate description with Ollama → post via hawker → log to Linear." Also: "When YNAB transaction appears → categorize with AI → update budget dashboard." For ADHD, visual workflows are easier to understand and maintain than scattered scripts.
- **Integration effort:** Weekend project for setup, then incremental. `docker run n8nio/n8n` and start building flows. The learning curve is the integrations, not the platform.
- **Concerns:** Fair-code license restricts SaaS use (irrelevant for personal use). Docker-based — needs a machine running 24/7 (your Mac Mini is perfect). Memory usage grows with workflow count.

---

### Actual Budget
- **GitHub:** https://github.com/actualbudget/actual
- **Stars:** ~17K | **License:** MIT
- **What it does:** Fast, privacy-focused, local-first envelope-budgeting app with optional multi-device sync. Self-hosted alternative to YNAB with bank import rules, goal tracking, and a clean UI. Runs entirely on your infrastructure.
- **Why for Vitalik:** You use YNAB now. Actual gives you the same envelope budgeting with full data ownership, no subscription, and the ability to automate via its API. Connect it to n8n for automated transaction categorization, spending alerts, or AI-powered budget insights. Local-first aligns with your privacy philosophy.
- **Integration effort:** Significant (migrating from YNAB). But the app itself is drop-in — `docker run actualbudget/actual-server`. Migration is the real work.
- **Concerns:** Smaller ecosystem than YNAB. Bank sync requires manual import or third-party bridges. The trade-off is privacy for convenience.

---

## 2. Tools Worth Watching

Not ready for immediate integration, but promising for your use case.

### Screenpipe
- **GitHub:** https://github.com/mediar-ai/screenpipe
- **Stars:** ~20K | **License:** MIT
- **What it does:** Records screen and audio to local storage with AI. Search everything you've seen or heard. 100% local, no cloud, no account. Open-source Rewind.ai alternative. Y Combinator backed. Has an MCP server for Claude Code integration.
- **Why for Vitalik:** You're already evaluating this. For focuspipe specifically, Screenpipe could be the data source — it captures what you're doing on screen, and focuspipe could analyze those patterns for focus/distraction detection. The MCP server means Claude Code can query your screen history directly.
- **Watch because:** Still maturing. Resource usage (continuous screen recording) on Mac Mini needs testing. The plugin/pipe ecosystem is young. Evaluate for 1-2 weeks before committing to building on it.
- **Integration effort:** Drop-in to try, significant to build focuspipe on top of.

---

### Letta (formerly MemGPT)
- **GitHub:** https://github.com/letta-ai/letta
- **Stars:** ~22K | **License:** Apache 2.0
- **What it does:** An OS-inspired AI agent framework with three-tier memory: core (always in context, like RAM), archival (vector store, like disk), recall (conversation history). Agents don't just use Letta for memory — they run inside it. Born from Berkeley research, $10M seed from Felicis.
- **Why for Vitalik:** More sophisticated than Mem0 for agents that need to manage their own context. If focuspipe evolves into a persistent agent that monitors your work patterns over weeks/months, Letta's memory management would handle the growing context better than simple vector search.
- **Watch because:** More complex to set up than Mem0. The framework is opinionated — your agents must run inside Letta, not alongside it. Wait until your agent needs exceed what Mem0 provides.

---

### Khoj
- **GitHub:** https://github.com/khoj-ai/khoj
- **Stars:** ~25K+ | **License:** AGPL-3.0
- **What it does:** Self-hostable AI "second brain." Turns documents, web pages, and notes into a searchable knowledge base with RAG. Supports custom agents, automations, multiple LLM backends (including local via Ollama). Integrates with Obsidian and Emacs.
- **Why for Vitalik:** A unified search across all your knowledge — notes, documents, bookmarked pages, past research. For ADHD, the ability to ask "what was that thing I read about marketplace pricing last week?" and get an answer is invaluable.
- **Watch because:** AGPL license is restrictive if you ever want to commercialize something built on it. Resource usage for continuous indexing needs evaluation.

---

### OpenHands
- **GitHub:** https://github.com/All-Hands-AI/OpenHands
- **Stars:** ~80K+ | **License:** MIT
- **What it does:** AI agent that autonomously writes code, runs tests, debugs, and browses the web inside a Docker sandbox. 72% SWE-bench score. Full autonomous loop: plan → code → execute → iterate. REST/WebSocket API for programmatic control. 9M+ downloads, 500+ contributors.
- **Why for Vitalik:** You're heavy on Claude Code already, but OpenHands could handle delegated tasks — "fix this bug in hawker," "add this feature to CareerOS" — running autonomously while you focus on other work. It's model-agnostic, so you can point it at Claude's API.
- **Watch because:** Claude Code is your primary tool and keeps improving. OpenHands adds value only if you need parallel autonomous coding sessions. Evaluate when Claude Code's concurrent session limits become a bottleneck.

---

### Activepieces
- **GitHub:** https://github.com/activepieces/activepieces
- **Stars:** ~14K | **License:** MIT
- **What it does:** Self-hosted Zapier alternative with MIT license (unlike n8n's fair-code). Visual workflow builder, 200+ integrations, AI-powered automations. Newer and lighter than n8n.
- **Why for Vitalik:** If n8n feels heavy or the licensing concerns you, Activepieces is the MIT-licensed alternative. Fewer integrations but cleaner architecture.
- **Watch because:** Smaller community and fewer integrations than n8n. The MIT license is the main advantage — if n8n's fair-code doesn't bother you for personal use, n8n is more mature.

---

### Second Brain (Desktop App)
- **GitHub:** https://github.com/henrydaum/second-brain
- **What it does:** Desktop personal knowledge base using RAG and multimodal AI. Hybrid lexical/semantic search for local files and images. Entity graph via GraphRAG. Runs entirely locally on Ollama.
- **Why for Vitalik:** Local-first knowledge management that indexes your filesystem and lets you ask questions about your own data. Could integrate with focuspipe's data collection.
- **Watch because:** Early-stage project. Evaluate alongside Khoj to see which fits better.

---

## 3. Building Blocks for Your Projects

### For focuspipe (ADHD monitoring)

| Repo | GitHub | Use Case | Integration |
|------|--------|----------|-------------|
| **Screenpipe** | github.com/mediar-ai/screenpipe | Screen/audio data source for activity tracking | Core data pipeline |
| **Mem0** | github.com/mem0ai/mem0 | Persistent memory for focus patterns over time | Memory layer |
| **Ollama** | github.com/ollama/ollama | Local classification (focused vs distracted) | Inference engine |
| **Whisper.cpp** | github.com/ggerganov/whisper.cpp | Local audio transcription for meeting detection | Audio processing |

**Architecture suggestion:** Screenpipe captures raw screen/audio data → Whisper.cpp transcribes audio locally → Ollama classifies activity patterns → Mem0 stores and retrieves historical patterns → focuspipe UI shows insights and nudges.

### For hawker (marketplace bot)

| Repo | GitHub | Use Case | Integration |
|------|--------|----------|-------------|
| **Crawl4AI** | github.com/unclecode/crawl4ai | Marketplace listing scraping | Data ingestion |
| **browser-use** | github.com/browser-use/browser-use | Automated listing posting, buyer interaction | Action layer |
| **Firecrawl** | github.com/mendableai/firecrawl | Alternative scraper with anti-bot handling | Data ingestion (alt) |
| **n8n** | github.com/n8n-io/n8n | Orchestrate scrape→process→post workflows | Orchestration |

**Architecture suggestion:** Crawl4AI monitors competitor listings on schedule via n8n → Claude analyzes pricing and generates optimized descriptions → browser-use posts/updates listings → n8n handles notifications and logging.

### For CareerOS (job search)

| Repo | GitHub | Use Case | Integration |
|------|--------|----------|-------------|
| **Crawl4AI** | github.com/unclecode/crawl4ai | Job board scraping | Data ingestion |
| **Mem0** | github.com/mem0ai/mem0 | Track applications, preferences, interview notes | Memory layer |
| **browser-use** | github.com/browser-use/browser-use | Automated Easy Apply flows | Action layer |
| **Ollama** | github.com/ollama/ollama | Local resume tailoring, cover letter generation | Inference engine |

**Architecture suggestion:** Crawl4AI scrapes job boards → Ollama/Claude scores relevance against your profile stored in Mem0 → browser-use handles applications on supported platforms → Mem0 tracks everything → n8n sends daily digest of new matches.

---

## 4. Grant Inspiration

These repos suggest adjacent spaces where grant-worthy projects could be built.

### ADHD-Specific AI Agent
- **Inspiration from:** Screenpipe + Mem0 + focuspipe
- **Idea:** An open-source, privacy-first AI agent specifically designed for neurodivergent users. Combines passive activity monitoring (Screenpipe), pattern recognition (local LLM), and gentle intervention (context-aware nudges). Unlike generic productivity tools, it adapts to ADHD patterns — hyperfocus detection, task-switching alerts, energy-level estimation.
- **Grant angle:** Digital health + accessibility + open source. EU Digital Europe Programme, Mozilla Foundation, or Sovereign Tech Fund.

### Privacy-Preserving Personal Finance AI
- **Inspiration from:** Actual Budget + Ollama + Mem0
- **Idea:** A fully local AI financial advisor that runs on your own hardware. Ingests bank transactions (via Actual Budget or direct import), categorizes spending with local LLM, identifies patterns, and provides actionable advice — all without data leaving your device. Could integrate with N26's API.
- **Grant angle:** Financial literacy + privacy + open source. NLnet Foundation, Prototype Fund (German government).

### Open-Source Marketplace Intelligence Platform
- **Inspiration from:** Crawl4AI + browser-use + hawker
- **Idea:** A self-hosted platform for marketplace sellers that aggregates pricing data, competitor analysis, and demand signals across platforms (eBay, Amazon, local marketplaces). Uses AI to suggest optimal pricing, timing, and descriptions. All data stays local.
- **Grant angle:** Small business empowerment + open source + AI democratization. EU Horizon Europe SME Instrument.

### Neurodivergent-Friendly Job Search System
- **Inspiration from:** CareerOS + Mem0 + browser-use
- **Idea:** An AI-powered job search assistant designed for neurodivergent job seekers. Handles the executive-function-heavy parts (tracking applications, following up, tailoring resumes) while the user focuses on the human parts (interviews, networking). Addresses the specific challenges ADHD professionals face in job searching.
- **Grant angle:** Employment accessibility + AI + open source. EU Social Fund, national disability employment programs.

---

## 5. Skip List

Popular repos that are NOT relevant to your setup.

| Repo | Why Skip |
|------|----------|
| **AutoGPT** (~183K stars) | Autonomous agent hype from 2023. Unreliable for production use. You have Claude Code which is vastly more capable for actual development work. |
| **LangChain** (~132K stars) | Over-abstracted framework. You're building with Claude directly via Claude Code and MCP. LangChain adds complexity without value for your stack. If you need orchestration, use CrewAI or n8n instead. |
| **Stable Diffusion / ComfyUI** | Image generation isn't part of your workflow. Your projects are text/data-centric. |
| **vLLM** | Production multi-user LLM serving. Overkill for personal use — Ollama covers your needs. vLLM is for teams serving 100+ concurrent users. |
| **Dify** (~144K stars) | Visual AI app builder. You're a developer who writes code — Dify's low-code approach adds overhead without benefit. Its modified Apache license is also restrictive. |
| **Open WebUI** | Chat interface for local LLMs. You interact with AI through Claude Code in the terminal, not through a web chat UI. |
| **GPT-Engineer / Devin clones** | You already have Claude Code. These are inferior alternatives solving the same problem. |
| **TensorFlow / PyTorch** | You're building AI-powered applications, not training models. These are for ML researchers, not application developers. |
| **MindsDB / MLflow** | ML ops tools for teams managing model training pipelines. Not applicable to your application-layer work. |
| **Supabase / Appwrite** | BaaS platforms. Your stack is local-first and self-hosted. These add cloud dependency you don't want. |
| **Jan AI / LM Studio** | Desktop LLM chat apps. Ollama + terminal is more aligned with your Ghostty/tmux workflow. |
| **PrivateGPT** | Document Q&A tool. Khoj or Second Brain do this better with more features. PrivateGPT is narrower in scope. |
| **DeepSeek models** | Impressive open models, but you access frontier models via Claude API. Running DeepSeek locally via Ollama is the right path if you need it, not the model repo itself. |

---

## Summary: Priority Action List

| Priority | Tool | Action | Time |
|----------|------|--------|------|
| 1 | **Ollama** | Install, pull llama3.1 and a small model for classification | 30 min |
| 2 | **Crawl4AI** | pip install, prototype marketplace scraping for hawker | 1 weekend |
| 3 | **Mem0** | pip install, prototype memory layer for focuspipe | 1 weekend |
| 4 | **browser-use** | pip install, prototype automated listing posting for hawker | 1 weekend |
| 5 | **n8n** | Docker deploy on Mac Mini, wire up first automation | 1 weekend |
| 6 | **Screenpipe** | Install, run for 1 week, evaluate data quality for focuspipe | 1 week eval |
| 7 | **Actual Budget** | Evaluate as YNAB replacement, test N26 import | 2 weeks eval |

---

*Sources: Research compiled from goodailist.com curated listings, GitHub trending, and cross-referenced with multiple open-source AI tool directories and comparison sites.*
