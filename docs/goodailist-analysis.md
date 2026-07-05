# goodailist.com Top AI Repos: Personal Integration Analysis

> **Date**: 2026-07-05
> **Source**: [goodailist.com/repos](https://goodailist.com/repos) (curated by Chip Huyen, updated daily)
> **Context**: Analysis of ~70 top open-source AI repos, filtered for relevance to a solo AI developer on Apple Silicon running Claude Code, building focuspipe (ADHD monitoring), hawker (marketplace bot), and CareerOS (job search).

---

## 1. Integrate NOW

These tools provide immediate, concrete value for the current stack and workflow. Prioritized by impact-to-effort ratio.

---

### Screenpipe — 24/7 Screen + Audio Memory
- **GitHub**: https://github.com/screenpipe/screenpipe (~19.6k stars)
- **What it does**: Continuously captures screen content and audio, extracts text via OS accessibility tree, transcribes speech with Whisper, and stores everything in a local SQLite database. "Pipes" (scheduled AI agents defined as markdown files) run against your captured data to take actions. YC S26.
- **Why for Vitalik**: This IS the data layer for focuspipe. Instead of building screen capture from scratch, Screenpipe provides a production-grade, privacy-first capture pipeline. Its event-driven capture (only records on meaningful changes — app switches, clicks, typing pauses) is exactly what ADHD monitoring needs. The MCP server lets Claude Code query your screen history directly. The pipe system means you can write ADHD monitoring logic as markdown prompts that run on a schedule.
- **Integration effort**: Drop-in. `brew install screenpipe`, add MCP server with `claude mcp add screenpipe -- npx -y screenpipe-mcp@latest`. Start building pipes immediately.
- **Concerns**: License changed from MIT to commercial (free for personal use, $25/mo commercial). ~0.5 GB/day storage. CPU usage is 5-10% thanks to event-driven capture, but test on the Mac Mini to confirm it's unobtrusive enough for 24/7 use.

---

### Mem0 — Persistent Memory for AI Sessions
- **GitHub**: https://github.com/mem0ai/mem0 (~60k stars)
- **What it does**: Universal memory layer for AI agents. Extracts facts from conversations and stores them in a vector database indexed by user/session/agent. On new sessions, relevant memories are retrieved via hybrid search (semantic + BM25 + entity matching) and injected into context. OpenMemory MCP server runs locally.
- **Why for Vitalik**: Solves the biggest pain point of Claude Code: losing context between sessions. With ADHD, re-explaining project context after every restart is a huge friction point. Mem0 remembers your preferences, decisions, and project state across sessions. The MCP server means Claude Code automatically gets relevant memories injected — zero effort after setup. Also a building block: focuspipe agents could use Mem0 to remember patterns in your behavior over weeks.
- **Integration effort**: Weekend project. `pip install mem0ai`, run OpenMemory MCP server locally, add to Claude Code MCP config. Self-hosted via Docker Compose (API server + Postgres/pgvector).
- **Concerns**: Needs a vector DB backend (pgvector recommended — you may already have Postgres). v2.0 released April 2026, API is stabilizing. Cloud option exists but self-hosted keeps data local.

---

### n8n — Self-Hosted Workflow Automation
- **GitHub**: https://github.com/n8n-io/n8n (~195k stars)
- **What it does**: Visual workflow automation platform with 1,500+ integrations and 70+ AI-specific nodes built on LangChain. Native MCP support (both client and server). Self-hosted community edition is free with unlimited workflows. Supports Claude, OpenAI, Ollama, and 12+ LLM providers. Execution-based pricing means a 20-step workflow costs the same as a 2-step one.
- **Why for Vitalik**: The glue layer for everything. Build automated workflows for: (1) hawker — monitor marketplace listings, trigger alerts, auto-respond to buyers via Slack/Telegram, (2) CareerOS — scrape job boards, filter matches, auto-apply or save to Linear, (3) YNAB — categorize transactions, send weekly financial summaries, (4) personal — sync TickTick tasks to Linear, aggregate notifications. The visual builder means you can prototype workflows in minutes, and the MCP integration lets Claude Code trigger or modify workflows programmatically.
- **Integration effort**: Weekend project. Docker Compose deployment, then visual workflow building. `docker run -d --name n8n -p 5678:5678 n8nio/n8n`. MCP server is built-in since April 2026.
- **Concerns**: AI-heavy workflows need 8-16 GB RAM. The sustainable-use license prohibits white-labeling/reselling but is fine for personal and internal business use. Community self-hosted is free forever.

---

### Crawl4AI — LLM-Friendly Web Scraping
- **GitHub**: https://github.com/unclecode/crawl4ai (~50k+ stars)
- **What it does**: Async web crawler that turns pages into clean, LLM-ready Markdown. Handles JavaScript rendering, removes noise via heuristic filtering ("Fit Markdown"), converts page links to numbered citations. Supports structured data extraction with any LLM. Docker API server available with auth-by-default security.
- **Why for Vitalik**: Direct building block for hawker (scrape marketplace listings into structured data) and CareerOS (scrape job postings into clean markdown for LLM processing). The Fit Markdown output feeds directly into Claude for analysis. Combined with n8n, you can build scrape → process → act pipelines. There's a community MCP server for Claude Code integration.
- **Integration effort**: Drop-in for scripts, weekend project for pipeline integration. `pip install crawl4ai`. Docker server for production use.
- **Concerns**: v0.9 is a security-focused release (auth on by default). For marketplace scraping, you'll still need to handle rate limiting and proxy rotation yourself. Python-only (no native TS SDK).

---

### Ollama — Local LLM Foundation
- **GitHub**: https://github.com/ollama/ollama (~176k stars)
- **What it does**: One-command local LLM inference. Wraps llama.cpp in a CLI + REST API. Supports 135k+ models. Native Metal acceleration on Apple Silicon. OpenAI-compatible API. 52 million monthly downloads.
- **Why for Vitalik**: Foundation for privacy-first AI processing across all projects. Run small models locally for: focuspipe analysis (classify screen activities, detect distraction patterns), hawker preprocessing (classify listings without API costs), CareerOS filtering (quick relevance scoring of job posts). The OpenAI-compatible API means any tool in your stack can use local models as a drop-in replacement. Also powers whisper.cpp for local transcription in Screenpipe.
- **Integration effort**: Drop-in. `brew install ollama && ollama pull llama3.2:3b`. Community MCP servers available.
- **Concerns**: Telemetry is ON by default — disable with `OLLAMA_NO_CLOUD=1`. 16 GB RAM handles 7B models; the Mac Mini M4 with 24+ GB RAM is ideal. v0.31.1 (June 30, 2026) has Gemma 4 MTP improvements for ~90% faster generation on Apple Silicon.

---

### Whisper.cpp — Local Speech-to-Text
- **GitHub**: https://github.com/ggml-org/whisper.cpp (~51.3k stars)
- **What it does**: C/C++ implementation of OpenAI's Whisper speech recognition with zero dependencies. First-class Apple Silicon support with Metal GPU acceleration and Core ML (3x+ faster than CPU). Models from tiny (75 MB) to large-v3 (2.9 GB).
- **Why for Vitalik**: Screenpipe already uses Whisper for audio transcription, but whisper.cpp independently is useful for: transcribing voice notes from phone (Syncthing → process), recording meeting notes, voice-driven note capture for focuspipe. Community MCP servers (`local-stt-mcp`, `whisper-mcp`) integrate directly with Claude Code for voice commands.
- **Integration effort**: Drop-in via Screenpipe (already included). Standalone: `brew install whisper-cpp`. MCP server: `npm install local-stt-mcp`.
- **Concerns**: Large-v3-turbo model needs ~3-4 GB RAM. Core ML encoder path recommended for best Apple Silicon performance.

---

## 2. Worth Watching

Promising but needs evaluation, maturity, or a specific trigger to adopt.

---

### Letta (formerly MemGPT) — Self-Managing Agent Memory
- **GitHub**: https://github.com/letta-ai/letta (~23.7k stars)
- **What it does**: Platform for building stateful AI agents with a three-tier memory architecture (core/archival/recall) inspired by OS memory management. Agents actively call memory management functions to decide what to remember. Maintains coherent context across 500+ interactions vs. ~50 for typical RAG.
- **Why for Vitalik**: If Mem0 proves insufficient for long-horizon memory, Letta is the next step. Its self-managing memory is ideal for ADHD — the agent handles the cognitive work of deciding what's important. The Letta Code desktop app (launched April 2026) provides a personal agent that learns your patterns. Could become the core intelligence layer for focuspipe.
- **Why wait**: Higher lock-in than Mem0 (2-6 weeks to migrate away). Still in beta (v0.16.8, May 2026). Evaluate after Mem0 integration proves the memory concept.
- **Integration effort**: Significant (1-2 weeks). Docker + Postgres/pgvector for self-hosted.

---

### Mastra — TypeScript Agent Framework
- **GitHub**: https://github.com/mastra-ai/mastra (~25.8k stars)
- **What it does**: Batteries-included TypeScript framework for AI agents with workflows, memory, tools, RAG, and eval. First-class MCP support in both directions. Built by the Gatsby.js team, YC W25, $35M raised. Mastra Studio local dev GUI at localhost:4111.
- **Why for Vitalik**: If you're building focuspipe/hawker/CareerOS in TypeScript, Mastra could be the agent framework. It provides the orchestration layer (workflows, memory, tool management) so you focus on domain logic. MCP support means your existing Claude Code MCP servers work directly. The dev GUI is great for debugging agent behavior.
- **Why wait**: Evaluate whether Claude Agent SDK (already in your stack via Claude Code) covers enough, or if Mastra's workflow engine and multi-provider support add meaningful value. The framework is at v1.48.0 (weekly releases) but the API is still evolving.
- **Integration effort**: Weekend project for POC, significant for production migration.

---

### Browser Use — AI Browser Automation
- **GitHub**: https://github.com/browser-use/browser-use
- **What it does**: AI-powered browser automation that goes beyond Playwright scripting. An AI agent can navigate complex UIs, fill forms, click buttons, and extract data by understanding the page visually and semantically.
- **Why for Vitalik**: Direct upgrade for hawker (marketplace automation requires interacting with complex UIs — login flows, listing creation, messaging buyers) and CareerOS (auto-fill job applications, navigate career portals). Combined with n8n or Mastra, enables fully autonomous marketplace selling workflows.
- **Why wait**: Evaluate reliability on the specific marketplace sites hawker targets. Browser automation is inherently fragile — test before committing.
- **Integration effort**: Weekend project. Python-based, Playwright under the hood.

---

### OpenClaw — Self-Hosted Personal AI Assistant
- **GitHub**: https://github.com/openclaw/openclaw (~382k stars)
- **What it does**: Self-hosted AI assistant that answers through messaging channels you already use (WhatsApp, Telegram, Signal, iMessage, Slack, Discord, 20+ more). Supports every major LLM. Full MCP support. MIT licensed. The fastest-growing open-source project in GitHub history.
- **Why for Vitalik**: Deploy once, access your AI assistant from anywhere — phone, desktop, messaging apps. Perfect for ADHD: instead of opening a terminal and typing `claude`, just message your assistant on Telegram/Signal. Could serve as the user-facing interface for focuspipe alerts and CareerOS updates.
- **Why wait**: Security concerns are active (65k-180k discoverable instances, attack vectors being found). Not yet hardened enough for always-on deployment with sensitive data. Wait for security to mature.
- **Integration effort**: Weekend project. Docker deployment, MCP support built-in.
- **Concerns**: 4 vCPU, 8 GB RAM recommended. Security posture needs careful evaluation.

---

### RAGFlow — Document Intelligence Engine
- **GitHub**: https://github.com/infiniflow/ragflow (~84.3k stars)
- **What it does**: All-in-one RAG engine with deep document understanding (tables, figures, scanned PDFs, multi-column layouts). Built-in DeepDoc parser, chunking, retrieval, re-ranking, citation tracing, and agent capabilities. Web UI for uploading and querying documents.
- **Why for Vitalik**: Build a personal knowledge base that actually understands document structure. Upload resumes, contracts, financial docs, project notes — get reliable answers with source citations. The "upload and ask" simplicity is ideal for ADHD (no pipeline configuration). Could power CareerOS's resume analysis and job matching.
- **Why wait**: Requires 16 GB+ RAM and ARM64 deployment needs Rosetta emulation on Apple Silicon (works but with friction). Evaluate after core integrations (Screenpipe, Mem0, n8n) are stable.
- **Integration effort**: Significant. Docker Compose with multiple services. 50 GB+ disk.

---

### ActivityWatch — Automated Time Tracking
- **GitHub**: https://github.com/ActivityWatch/activitywatch
- **What it does**: Automated, privacy-first time tracking. Records which applications and websites you use and for how long. All data stays local. Cross-platform. Plugin architecture.
- **Why for Vitalik**: Complements Screenpipe for focuspipe — ActivityWatch provides structured time tracking data (which app, how long) while Screenpipe captures content. Together they give a complete picture of attention patterns for ADHD monitoring.
- **Why wait**: Screenpipe may already provide enough data for focuspipe's needs. Evaluate after Screenpipe integration whether you need ActivityWatch's structured categorization on top.
- **Integration effort**: Drop-in. Desktop app with REST API.

---

### AnythingLLM — All-in-One Local AI Platform
- **GitHub**: https://github.com/Mintplex-Labs/anything-llm (~62.6k stars)
- **What it does**: Full-stack desktop AI app with RAG, agents, MCP support (client + server), 25+ LLM providers, custom agent skills, and "Magic Features" (OS-wide dictation, text actions, autocomplete). v1.15.0 (June 2026) added meeting assistant with speaker identification.
- **Why for Vitalik**: Could replace multiple tools with a single app. Document Q&A over local files, meeting transcription, OS-wide AI autocomplete — all local-first. The Magic Features are particularly ADHD-friendly (trigger AI from anywhere, no context switching to a terminal).
- **Why wait**: Risk of being a "jack of all trades" — test whether it does any single thing better than your existing Claude Code + Screenpipe + Ollama stack. The custom license (non-OSI) may limit forking.
- **Integration effort**: Drop-in. Native macOS app.

---

## 3. Building Blocks for Projects

Repos that focuspipe, hawker, or CareerOS should directly build on or integrate with.

---

### For focuspipe (ADHD Monitoring)

| Repo | Stars | What it provides | How to use it |
|------|-------|-----------------|---------------|
| [Screenpipe](https://github.com/screenpipe/screenpipe) | 19.6k | Screen + audio capture pipeline | Core data layer — replace custom capture code |
| [Mem0](https://github.com/mem0ai/mem0) | 60k | Cross-session memory | Remember user patterns over weeks/months |
| [Whisper.cpp](https://github.com/ggml-org/whisper.cpp) | 51.3k | Local speech recognition | Transcribe audio for distraction detection |
| [Ollama](https://github.com/ollama/ollama) | 176k | Local LLM inference | Classify activities, detect focus patterns locally |
| [ChromaDB](https://github.com/chroma-core/chroma) | 28.7k | Vector database | Store activity embeddings for pattern matching |
| [pgvector](https://github.com/pgvector/pgvector) | 22.1k | Postgres vector extension | If already using Postgres, add vector search without a new service |

**Architecture suggestion**: Screenpipe captures → Ollama classifies activities → ChromaDB/pgvector stores embeddings → Mem0 maintains long-term behavioral patterns → Claude Code pipes generate daily ADHD reports.

---

### For hawker (Marketplace Bot)

| Repo | Stars | What it provides | How to use it |
|------|-------|-----------------|---------------|
| [Crawl4AI](https://github.com/unclecode/crawl4ai) | 50k+ | LLM-friendly web scraping | Scrape marketplace listings into structured data |
| [n8n](https://github.com/n8n-io/n8n) | 195k | Workflow automation | Orchestrate scrape → process → list → respond pipelines |
| [Browser Use](https://github.com/browser-use/browser-use) | — | AI browser automation | Automate complex marketplace UIs (listing, messaging) |
| [Composio](https://github.com/ComposioHQ/composio) | 29.1k | Tool integration for agents | Connect marketplace APIs with OAuth handling |
| [marker](https://github.com/VikParuchuri/marker) | 37.2k | Document conversion | Convert product PDFs/catalogs to structured data |

**Architecture suggestion**: Crawl4AI scrapes listings → n8n orchestrates the pipeline → Browser Use handles complex UI interactions → Composio manages API auth → Claude analyzes and acts.

---

### For CareerOS (Job Search)

| Repo | Stars | What it provides | How to use it |
|------|-------|-----------------|---------------|
| [Crawl4AI](https://github.com/unclecode/crawl4ai) | 50k+ | Web scraping | Scrape job boards (LinkedIn, Indeed, company pages) |
| [n8n](https://github.com/n8n-io/n8n) | 195k | Workflow automation | Daily job scrape → filter → notify pipeline |
| [marker](https://github.com/VikParuchuri/marker) | 37.2k | PDF/DOCX conversion | Parse resumes, cover letters, job descriptions to markdown |
| [Docling](https://github.com/docling-project/docling) | 62.7k | Document processing | Advanced document understanding with MCP server |
| [Mem0](https://github.com/mem0ai/mem0) | 60k | Memory layer | Remember application history, preferences, feedback |
| [Mastra](https://github.com/mastra-ai/mastra) | 25.8k | Agent framework (TS) | Build the CareerOS agent with workflows and memory |

**Architecture suggestion**: Crawl4AI/n8n scrape job postings → marker/Docling parse documents → Mem0 tracks application history → Mastra orchestrates the agent → Claude matches, ranks, and drafts applications.

---

## 4. Grant Inspiration

Repos that suggest grant-worthy project ideas in adjacent spaces.

---

### "Passive ADHD Coach" — Combining Screenpipe + Focuspipe + Letta
**Idea**: An always-on, privacy-first ADHD monitoring and coaching system that observes work patterns via Screenpipe, detects context-switching and distraction patterns via local LLMs, maintains long-term behavioral memory via Letta/Mem0, and proactively nudges the user back on track through gentle notifications. Unlike existing ADHD apps that require manual input (timers, checklists, journaling), this works entirely passively.
**Why it's grantable**: ADHD affects ~5% of adults, existing tools require the very executive function skills ADHD impairs. A passive, AI-powered approach is novel and technically feasible with current open-source components. Potential funders: Mozilla Builders, Sovereign Tech Fund, NLNet, Google.org.

### "Local-First Personal Data OS" — Syncthing + Screenpipe + RAGFlow + Mem0
**Idea**: A self-hosted system that captures, indexes, and makes searchable your entire digital life (screen content, audio, documents, messages) while keeping everything on your own hardware. Think "Rewind.ai but fully open-source and local." The RAG engine lets you ask questions like "what was that article I read last Tuesday about TypeScript agents?"
**Why it's grantable**: Privacy-preserving alternatives to surveillance capitalism. Sovereign Tech Fund, NLNet, and Prototype Fund (Germany) fund exactly this.

### "Open CareerOS" — n8n + Crawl4AI + Claude Agent SDK
**Idea**: Open-source job search automation platform for developers. Scrapes job boards, matches against your resume/preferences, auto-generates tailored applications, tracks application status, and provides interview prep — all self-hosted. No data sent to third-party services.
**Why it's grantable**: Job search is a universal pain point with no good open-source tooling. The "AI for good" angle (helping people find employment) resonates with social-impact funders.

### "MCP Marketplace Agent Toolkit"
**Idea**: A suite of MCP servers specifically for marketplace automation — listing management, inventory sync, price optimization, buyer communication — exposing everything as tools that any MCP-compatible agent (Claude Code, Cursor, n8n) can use. Think "Composio but focused on e-commerce and fully self-hosted."
**Why it's grantable**: Enables small sellers to compete with large retailers using AI automation, without giving data to SaaS platforms. EU Digital Markets Act alignment.

---

## 5. Skip List

Popular repos that are NOT relevant to the current setup.

| Repo | Stars | Why skip |
|------|-------|----------|
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI) | 106k | Image generation workflow — not in your use case |
| [Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | — | Image generation — not relevant |
| [vLLM](https://github.com/vllm-project/vllm) | 85.4k | Production-scale GPU inference server. Overkill for solo dev; Ollama covers local inference |
| [TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM) | 14k | NVIDIA-only, no Apple Silicon support at all |
| [SGLang](https://github.com/sgl-project/sglang) | 29.9k | Production GPU serving framework. Apple Silicon support is experimental |
| [Milvus](https://github.com/milvus-io/milvus) | 45.1k | Enterprise-scale vector DB. pgvector or ChromaDB are right-sized for solo dev |
| [Weaviate](https://github.com/weaviate/weaviate) | 16.5k | Enterprise vector DB. Same — pgvector is simpler for your scale |
| [Dify](https://github.com/langgenius/dify) | 148k | Visual AI workflow builder. Overlaps with n8n + Claude Code. Too heavy for solo dev stack |
| [Langflow](https://github.com/langflow-ai/langflow) | 151k | Visual low-code AI builder. Same overlap — you're a code-first developer |
| [Flowise](https://github.com/FlowiseAI/Flowise) | 54.3k | Drag-and-drop LLM builder. You're past the no-code stage |
| [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | 185k | General autonomous agent platform. Claude Code already fills this role |
| [LangChain (Python)](https://github.com/langchain-ai/langchain) | 141k | Python orchestration framework. You're TypeScript-first; use Mastra or Vercel AI SDK instead |
| [CrewAI](https://github.com/crewAIInc/crewAI) | 54.9k | Python multi-agent framework. Same — Python-first doesn't fit your stack |
| [AutoGen](https://github.com/microsoft/autogen) | 59.5k | Now in maintenance mode. Microsoft recommends their Agent Framework instead |
| [MetaGPT](https://github.com/FoundationAgents/MetaGPT) | 69.2k | SW company simulation. Novel concept but not practical for solo dev tooling |
| [BabyAGI](https://github.com/yoheinakajima/babyagi) | 22.3k | Archived/experimental. Not for production use |
| [AgentGPT](https://github.com/reworkd/AgentGPT) | 36.2k | Archived January 2026. Dead project |
| [Open WebUI](https://github.com/open-webui/open-webui) | 144k | Ollama frontend. You use Claude Code, not chat UIs. License changed from MIT to non-OSI |
| [LM Studio](https://github.com/lmstudio-ai) | ~5k (CLI) | Proprietary GUI for local models. Ollama covers this with better CLI/API integration |
| [GPT4All](https://github.com/nomic-ai/gpt4all) | 77.4k | Desktop LLM app. No MCP, slow releases (last Dec 2025). Ollama is strictly better |
| [ONNX Runtime](https://github.com/microsoft/onnxruntime) | 21k | ML inference engine. Too low-level for your use case; llama.cpp/Ollama abstract this away |
| [Continue.dev](https://github.com/continuedev/continue) | 34.3k | Acquired by Cursor, shut down June 2026. Repo is read-only |
| [Devon](https://github.com/entropy-research/Devon) | 3.4k | AI pair programmer. Stalled since May 2025. Use Claude Code instead |
| [E2B](https://github.com/e2b-dev/E2B) | 12.8k | Cloud code sandboxes. Not self-hostable, requires cloud infra. Overkill for local dev |
| [Modal](https://github.com/modal-labs/modal-client) | ~487 | Serverless GPU cloud. Great product but cloud-only — conflicts with local-first philosophy |
| [Unstructured](https://github.com/Unstructured-IO/unstructured) | 15.1k | Document ETL. marker and Docling are better fits — simpler, faster, MCP support |

---

## Quick Reference: Top Picks by Category

### Must-Have Stack (integrate this month)
1. **Screenpipe** — data layer for focuspipe
2. **Mem0** — cross-session memory for Claude Code
3. **n8n** — workflow automation for everything
4. **Crawl4AI** — web scraping for hawker + CareerOS
5. **Ollama** — local LLM foundation (if not already using)

### Best TypeScript-Native Tools
- **Mastra** (25.8k) — agent framework, MCP, by Gatsby team
- **Vercel AI SDK** (25.4k) — provider-agnostic, 11.5M weekly npm downloads
- **CopilotKit** (31.5k) — embed agents in React UIs

### Best for Privacy/Local-First
- **Ollama** — local inference, MIT, no telemetry (if disabled)
- **Screenpipe** — local capture, local storage
- **Whisper.cpp** — local STT, zero dependencies, MIT
- **ChromaDB** — local vector DB, Apache 2.0
- **pgvector** — vector search in existing Postgres

### Best MCP Ecosystem
- **Screenpipe** — built-in MCP server
- **Mem0** — OpenMemory MCP server
- **n8n** — bidirectional MCP (April 2026)
- **Docling** — MCP server for document processing
- **Composio** — 1,000+ tool integrations via MCP

---

## Architecture Vision

```
┌─────────────────────────────────────────────────────────┐
│                    User Layer                            │
│  Claude Code ←→ Screenpipe MCP ←→ Mem0 MCP              │
│  Telegram/Signal (via OpenClaw, later)                   │
└──────────────┬──────────────────────┬───────────────────┘
               │                      │
┌──────────────▼──────────┐ ┌────────▼────────────────────┐
│     focuspipe            │ │   hawker / CareerOS          │
│  Screenpipe → Ollama     │ │  Crawl4AI → n8n → Claude    │
│  → ChromaDB → Reports   │ │  → Browser Use → Respond     │
│  → Mem0 (patterns)      │ │  → Mem0 (history)            │
└─────────────────────────┘ └──────────────────────────────┘
               │                      │
┌──────────────▼──────────────────────▼───────────────────┐
│                  Infrastructure                          │
│  Ollama (local LLM) | pgvector | Syncthing | Docker     │
└─────────────────────────────────────────────────────────┘
```

---

*Generated 2026-07-05 by Claude Code routine. Data sourced from GitHub, web searches, and project documentation. Star counts are approximate as of the analysis date.*
