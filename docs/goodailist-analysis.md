# goodailist.com Top AI Repos Analysis

**Date:** 2026-07-20
**Source:** [goodailist.com/repos](https://goodailist.com/repos) (Chip Huyen's curated list of 15K+ open-source AI repos), cross-referenced with OSSInsight, ByteBytego, Firecrawl, and GitHub trending data.
**Purpose:** Identify repos relevant to Vitalik's solo AI dev workflow, projects (focuspipe, hawker, CareerOS), and ADHD-optimized tooling philosophy.

---

## 1. Tools to Integrate NOW

These provide immediate, measurable improvements to your daily workflow with minimal setup friction.

### 1.1 Screenpipe
- **GitHub:** https://github.com/screenpipe/screenpipe (~20K stars)
- **What:** YC S26 company. Records your screen 24/7, runs OCR + audio transcription locally, stores everything in SQLite with FTS5 search. Runs as an MCP server so Claude can query your screen history directly.
- **Why for Vitalik:** This is the missing backbone for focuspipe. Instead of building screen capture from scratch, screenpipe gives you a production-grade local recording pipeline. You can query "what was I doing at 2pm?" or "how long did I spend in the terminal today?" directly from Claude Code. The MCP server integration means your existing Claude workflow gains screen-awareness for free. For ADHD monitoring, this is the data layer you need — passive capture, zero discipline required.
- **Integration effort:** Drop-in (brew install, configure MCP, done in 30 minutes)
- **Concerns:** CPU/disk usage on continuous recording — test on your Mac Mini first and tune frame rate. MIT licensed, fully auditable. You're already evaluating it — pull the trigger.

### 1.2 n8n
- **GitHub:** https://github.com/n8n-io/n8n (~197K stars)
- **What:** Self-hosted workflow automation platform with 400+ integrations, native AI agent nodes (70+ LangChain nodes), MCP support, and visual workflow builder. Fair-code license, free community edition with unlimited executions.
- **Why for Vitalik:** This replaces the glue code between your projects. hawker's marketplace monitoring, CareerOS's job scraping pipelines, focuspipe's alert system — all of these are n8n workflows. Set up a cron to scan eBay Kleinanzeigen, pipe through an LLM node for classification, push notifications via Pushover. The visual builder means you prototype in minutes, not hours. Self-hosted on your Mac Mini = zero recurring cost, full privacy.
- **Integration effort:** Weekend project (Docker on Mac Mini, then iterative workflow building)
- **Concerns:** Node.js runtime, can be memory-hungry with many workflows. Fair-code license means you can't resell it, but that's irrelevant for personal use.

### 1.3 Ollama
- **GitHub:** https://github.com/ollama/ollama (~165K+ stars)
- **What:** Run LLMs locally with a single command. Pull models like Qwen 3.5, Mistral Small, Gemma 4, DeepSeek. OpenAI-compatible API at localhost:11434.
- **Why for Vitalik:** Your Mac Mini M4 can run 8B-22B models at 20-30 tok/s via Ollama drawing just 30-40W. This gives you a free, private LLM backend for n8n workflows, hawker's listing classification, and focuspipe's activity categorization. No API costs, no data leaving your network. Pairs with MLX for even faster Apple Silicon inference (30-50% faster than llama.cpp).
- **Integration effort:** Drop-in (one command install, `ollama pull qwen3.5:9b`, done)
- **Concerns:** Model quality won't match Claude for complex tasks — use Ollama for high-volume, low-stakes classification and Claude API for the hard stuff. ~$14/year electricity for 24/7 operation.

### 1.4 Crawl4AI
- **GitHub:** https://github.com/unclecode/crawl4ai (~68K stars)
- **What:** Open-source Python web crawler that outputs clean LLM-ready markdown. Handles JavaScript rendering, extracts structured data, supports async crawling. No external API needed.
- **Why for Vitalik:** Direct building block for hawker (marketplace scraping) and CareerOS (job board crawling). Self-hosted, no per-page API costs like Firecrawl. Outputs clean markdown that feeds directly into your LLM pipelines for classification. Supports Playwright under the hood — you already know Playwright.
- **Integration effort:** Drop-in for Python projects (pip install crawl4ai)
- **Concerns:** You own scaling, proxies, and rate limiting. For tough anti-bot sites, you may still need Firecrawl's managed infra. Use Crawl4AI for the bulk, Firecrawl for the edge cases.

### 1.5 Open WebUI
- **GitHub:** https://github.com/open-webui/open-webui (~50K+ stars)
- **What:** ChatGPT-like web interface for Ollama and any OpenAI-compatible API. Built-in RAG with 9 vector database backends, web search integration, native desktop app with spotlight bar and push-to-talk voice.
- **Why for Vitalik:** Gives you a proper UI for your local Ollama models. The RAG integration means you can chat with your documents (project specs, notes, financial records) privately. The macOS desktop app with spotlight bar = quick AI queries without context-switching. Pairs naturally with Ollama on your Mac Mini.
- **Integration effort:** Drop-in (Docker alongside Ollama, or pip install)
- **Concerns:** Another service to maintain. If you're Claude-primary, this is supplementary — useful for quick local queries and document Q&A where you don't want API costs.

### 1.6 SearXNG
- **GitHub:** https://github.com/searxng/searxng (~32K stars)
- **What:** Self-hosted metasearch engine aggregating 251+ search services. No tracking, no profiling. Can be used as a search backend for Open WebUI and n8n.
- **Why for Vitalik:** Privacy-respecting search that feeds into your AI stack. Open WebUI can use SearXNG for RAG-enhanced web search. n8n can query SearXNG for automated research pipelines. Aligns perfectly with your local-first philosophy.
- **Integration effort:** Weekend project (Docker, then wire into Open WebUI/n8n)
- **Concerns:** Search quality varies by upstream engine availability. Public instances exist if self-hosting feels heavy.

### 1.7 Career-ops
- **GitHub:** https://github.com/santifer/career-ops
- **What:** Open-source AI job search system that runs inside any AI coding CLI (Claude Code, Codex, etc.). Scans portals (Greenhouse, Ashby, Lever), processes 10+ offers in parallel with sub-agents, scores listings A-F, and tailors CVs.
- **Why for Vitalik:** This could either complement or accelerate CareerOS. It already runs inside Claude Code — your primary tool. The parallel sub-agent architecture for evaluating job postings is exactly what CareerOS needs. Study the architecture even if you don't use it directly.
- **Integration effort:** Drop-in for evaluation, significant to integrate into CareerOS
- **Concerns:** Young project — vet the quality of its scoring and CV tailoring before relying on it.

---

## 2. Tools Worth Watching

Not ready for immediate integration or need evaluation, but promising for your use case.

### 2.1 OpenClaw
- **GitHub:** https://github.com/openclaw/openclaw (~346K+ stars, #1 on GitHub)
- **What:** Self-hosted personal AI assistant connecting to 25+ messaging channels (WhatsApp, Telegram, Signal, iMessage, Discord, etc.). Runs locally, connects to Claude/GPT/DeepSeek for inference. Modular "Skills" system for browser automation, API calls, file ops. Now a non-profit foundation.
- **Why watching:** The multi-channel routing is interesting — imagine focuspipe alerts going to Signal, or hawker notifications to Telegram. But at 346K stars, the project is moving fast and the surface area is huge. Wait for the foundation to stabilize governance before building on it.
- **Integration effort:** Weekend project to evaluate, significant to build custom Skills
- **Concerns:** Founder joined OpenAI, project handed to foundation — governance risk. Massive codebase, maintenance burden unclear.

### 2.2 Browser Use
- **GitHub:** https://github.com/browser-use/browser-use (~93K stars)
- **What:** Python library that turns any LLM into a full browser automation agent. Highest score on WebVoyager benchmark (89.1%). Migrated from Playwright to direct CDP in 2026 for performance.
- **Why watching:** Could supercharge hawker's marketplace interactions — not just scraping but actually placing listings, responding to messages, handling negotiations via AI. But LLM-driven browser automation is still expensive (token costs per action) and unpredictable for production.
- **Integration effort:** Weekend project to prototype
- **Concerns:** Token costs add up fast for production use. Reliability for financial transactions (marketplace selling) is risky. Better for research/scraping than transactional flows.

### 2.3 Stagehand
- **GitHub:** https://github.com/browserbase/stagehand
- **What:** AI browser automation SDK with four primitives (act, extract, observe, agent). Hybrid approach — natural language for dynamic parts, cached Playwright commands for stable parts. MIT licensed, TypeScript and Python.
- **Why watching:** More production-safe than Browser Use because of the hybrid AI+deterministic approach. The auto-caching of AI actions into Playwright commands means costs decrease over time as patterns stabilize. Potentially better fit for hawker than Browser Use.
- **Integration effort:** Weekend project
- **Concerns:** Backed by Browserbase (cloud browser company) — evaluate whether self-hosted use stays first-class or gets deprioritized.

### 2.4 Dify
- **GitHub:** https://github.com/langgenius/dify (~136K stars)
- **What:** Production-ready platform for building AI agents with visual workflow builder, built-in RAG pipeline, and support for multiple model providers. TypeScript-based.
- **Why watching:** More feature-complete than Flowise, but also more complex. If n8n doesn't cover your AI workflow needs, Dify is the next step up. However, n8n's broader integration ecosystem (400+ non-AI integrations) makes it more versatile for your use case.
- **Integration effort:** Significant (Docker deployment, learning curve)
- **Concerns:** Overlap with n8n. Pick one orchestrator and stick with it — for your needs, n8n wins on integration breadth.

### 2.5 LangGraph
- **GitHub:** https://github.com/langchain-ai/langgraph (~34K stars, 34.5M monthly downloads)
- **What:** Multi-agent orchestration framework. LangGraph 1.0 GA reached in early 2026. Best-in-class for building complex agent workflows with cycles, persistence, and human-in-the-loop.
- **Why watching:** If focuspipe or CareerOS needs multi-agent architectures (e.g., parallel job evaluators, multi-step ADHD intervention flows), LangGraph is the production-ready option. But for solo dev work, Claude Code's built-in agent capabilities may be sufficient.
- **Integration effort:** Significant (Python/JS, learning LangChain ecosystem)
- **Concerns:** LangChain ecosystem complexity. Evaluate whether your agent needs truly require this vs. simpler patterns.

### 2.6 Jan.ai
- **GitHub:** https://github.com/janhq/jan (~41K stars, 5.3M downloads)
- **What:** Desktop app for running LLMs offline. OpenAI-compatible API, MCP integration, cross-platform. AGPLv3.
- **Why watching:** Alternative to Open WebUI if you want a native desktop experience. The MCP integration is interesting for Claude Code users. But Open WebUI is more feature-rich for RAG workflows.
- **Integration effort:** Drop-in
- **Concerns:** AGPLv3 license is more restrictive than Open WebUI's BSD. Feature overlap with Ollama + Open WebUI combo.

### 2.7 TypeWhisper / OpenWhisper
- **GitHub:** https://github.com/TypeWhisper/typewhisper-mac / https://github.com/Rajvardhman05/openwhisper-app
- **What:** Local speech-to-text for macOS using WhisperKit (optimized for Apple Silicon). Hold a key, speak, text appears at cursor. Optional cleanup via local LLM.
- **Why watching:** Voice input could be an ADHD-friendly interaction mode — speak instead of type when executive function is low. The Apple Silicon optimization means fast, private transcription on your Mac hardware.
- **Integration effort:** Drop-in (macOS app)
- **Concerns:** Voice input quality varies. Test whether it actually reduces friction vs. adding cognitive load.

---

## 3. Building Blocks for Your Projects

### For focuspipe (ADHD monitoring)

| Repo | How to Use | Link |
|------|-----------|------|
| **Screenpipe** | Core data layer — screen capture, OCR, audio transcription, activity timeline. Don't rebuild this. | [GitHub](https://github.com/screenpipe/screenpipe) |
| **Ollama** | Local inference for activity classification ("productive" vs "distraction" vs "break"). Run Qwen 3.5 9B for near-zero cost. | [GitHub](https://github.com/ollama/ollama) |
| **n8n** | Workflow orchestration for alerts, daily summaries, intervention triggers. "If distraction > 20min, send Pushover notification." | [GitHub](https://github.com/n8n-io/n8n) |
| **LlamaIndex** | If you build a "what did I accomplish this week?" feature, LlamaIndex handles indexing and querying your activity history. | [GitHub](https://github.com/run-llama/llama_index) |
| **GAIA** | Proactive notification architecture — outputs land in a notification bell for approval. Study its passive-first UX pattern. | [GitHub](https://github.com/theexperiencecompany/gaia) |

### For hawker (marketplace bot)

| Repo | How to Use | Link |
|------|-----------|------|
| **Crawl4AI** | Scrape marketplace listings (eBay Kleinanzeigen, Facebook Marketplace) into LLM-ready format. | [GitHub](https://github.com/unclecode/crawl4ai) |
| **n8n** | Orchestrate the full pipeline: scrape -> classify -> price -> list -> monitor -> alert. | [GitHub](https://github.com/n8n-io/n8n) |
| **Ollama** | Classify listings, generate descriptions, estimate pricing — all locally. | [GitHub](https://github.com/ollama/ollama) |
| **Stagehand/Browser Use** | For marketplaces that need actual browser interaction (login, post listing, respond to messages). | [GitHub](https://github.com/browserbase/stagehand) |
| **ScraperAI** | Generates reusable scraping "recipes" with AI — useful for standardizing across multiple marketplace formats. | [GitHub](https://github.com/scraperai/scraperai) |

### For CareerOS (job search automation)

| Repo | How to Use | Link |
|------|-----------|------|
| **Career-ops** | Reference architecture — already runs in Claude Code with parallel sub-agent job evaluation. Study or fork. | [GitHub](https://github.com/santifer/career-ops) |
| **AI Job Search** | Battle-tested Claude Code workflow — 69 tailored applications, 20 first interviews. Study the prompts and CV tailoring approach. | [GitHub](https://github.com/MadsLorentzen/ai-job-search) |
| **ApplyPilot** | 6-stage autonomous pipeline: discover -> score -> tailor resume -> write cover letter -> submit. More aggressive automation. | [GitHub](https://github.com/Pickle-Pixel/ApplyPilot) |
| **JobSync** | Self-hosted Next.js application tracker with AI resume review and analytics. Could be CareerOS's tracking UI. | [GitHub](https://github.com/Gsync/jobsync) |
| **Crawl4AI** | Scrape job boards that don't have APIs. | [GitHub](https://github.com/unclecode/crawl4ai) |

---

## 4. Grant Inspiration

These repos and adjacent spaces suggest grant-worthy project ideas:

### 4.1 ADHD-Aware AI Workspace (focuspipe evolution)
- **Inspiration:** Screenpipe (passive recording) + neurodivergent MCP memory system (GitHub topic: neurodivergent)
- **Idea:** An open-source, privacy-first ADHD workstation monitor that uses local LLMs to detect focus state, intervene during distraction spirals, and generate end-of-day accountability reports — all without requiring any user discipline.
- **Potential funders:** [AI Grant](https://aigrant.org/), [OpenAI Mental Health Research Grants](https://openai.com/index/ai-mental-health-research-grants/) (up to $2M pool), [Microsoft AI for Accessibility](https://www.microsoft.com/en-us/accessibility/innovation)
- **Why fundable:** Intersection of AI + accessibility + mental health. Quantifiable impact (focus time improvement). Open-source ethos aligns with grant programs.

### 4.2 Local-First Personal AI Agent for Neurodivergent Professionals
- **Inspiration:** OpenClaw (multi-channel AI assistant) + GAIA (proactive notifications) + Screenpipe (context awareness)
- **Idea:** A personal AI agent that proactively manages executive function tasks — reminds you to eat, suggests task switches during hyperfocus, catches context-switch distractions, and surfaces relevant information before meetings — running entirely on local hardware.
- **Potential funders:** AI Grant, Mozilla Builders, NLnet Foundation
- **Why fundable:** Novel intersection. Most AI assistants require active engagement — this one works passively, which is the key design insight for ADHD users.

### 4.3 Open-Source Career Automation for Underrepresented Job Seekers
- **Inspiration:** Career-ops + AI Job Search + ApplyPilot
- **Idea:** A free, self-hosted job search automation system specifically designed for career changers and neurodivergent professionals, with AI-driven job matching that accounts for transferable skills (not just keyword matching), interview prep with anxiety-reducing techniques, and application tracking with gentle accountability.
- **Potential funders:** Google.org, Schmidt Futures, national employment agencies
- **Why fundable:** Addresses employment equity. Open-source = scalable impact.

### 4.4 Privacy-First Marketplace Intelligence for Small Sellers
- **Inspiration:** Crawl4AI + n8n + Ollama
- **Idea:** Open-source toolkit that gives individual sellers the same market intelligence (pricing trends, demand signals, competitor analysis) that large retailers have — running entirely on their own hardware.
- **Potential funders:** EU Digital Markets Act adjacent grants, small business innovation programs
- **Why fundable:** Levels the playing field for small sellers vs. platforms with proprietary data advantages.

---

## 5. Skip List

Popular repos that are NOT relevant to your setup:

| Repo | Stars | Why Skip |
|------|-------|----------|
| **AutoGPT** | ~182K | Overhyped autonomous agent. You already have Claude Code for agentic workflows. AutoGPT's "let it run forever" model is the opposite of ADHD-friendly — it's anxiety-inducing. |
| **Stable Diffusion / AUTOMATIC1111** | ~157K | Image generation isn't part of your workflow. If you need images, use Claude's image understanding or a hosted API. |
| **ComfyUI** | ~106K | Same — node-based image/video generation. Powerful but irrelevant to your text/automation focus. GPU-hungry too. |
| **vLLM** | High | Production LLM serving at scale. You're a solo dev, not running a model-serving cluster. Ollama is the right abstraction for your needs. |
| **Unsloth** | High | LLM fine-tuning with reduced VRAM. You don't need to fine-tune models — you need to use them effectively. Unless you decide to train a custom ADHD-classification model, skip. |
| **LangFlow** | ~146K | Visual LLM app builder. Overlap with n8n and Dify. n8n is better for your needs because it handles non-AI automation too (cron jobs, webhooks, API calls). |
| **CrewAI** | ~53K | Role-based multi-agent framework. Interesting concept but Claude Code's built-in multi-agent (workflows, subagents) already covers this. Adding another framework is complexity you don't need. |
| **AutoGen** | ~59K | Microsoft's multi-agent conversation framework. Same concern as CrewAI — you don't need a separate agent framework when Claude Code is your primary tool. |
| **Flowise** | ~51K | Another visual LLM builder. Pick n8n or Dify, not three visual builders. |
| **FinRobot** | Low | AI for financial analysis/trading. Overkill — you need YNAB integration and expense tracking, not algorithmic trading. |
| **Haystack** | ~24K | Enterprise NLP framework. Over-engineered for solo dev use. LlamaIndex is simpler for your RAG needs. |

---

## Recommended Stack Architecture

Based on this analysis, here's the integrated stack for your Mac Mini M4:

```
Layer 1: Foundation (install first)
  Ollama          -> local LLM inference (Qwen 3.5 9B)
  Open WebUI      -> chat interface + RAG
  SearXNG         -> private search backend

Layer 2: Data Capture (install second)
  Screenpipe      -> screen/audio recording + MCP server
  Crawl4AI        -> web scraping for hawker/CareerOS

Layer 3: Orchestration (build on top)
  n8n             -> workflow automation, connects everything
  Claude Code     -> primary dev tool, MCP-connected to screenpipe

Layer 4: Project-Specific
  focuspipe       -> built on screenpipe data + ollama classification + n8n alerts
  hawker          -> crawl4ai scraping + ollama classification + n8n orchestration
  CareerOS        -> career-ops patterns + crawl4ai + n8n pipelines
```

All local. All private. All free (except Claude API for the hard stuff).

---

## Next Steps

1. **Today:** Install Ollama and Screenpipe on Mac Mini
2. **This week:** Deploy n8n via Docker, build first hawker scraping workflow
3. **This weekend:** Set up Open WebUI + SearXNG for local AI chat
4. **Next week:** Evaluate career-ops for CareerOS architecture decisions
5. **Ongoing:** Watch OpenClaw foundation governance, Browser Use reliability improvements

---

*Analysis compiled from goodailist.com ecosystem, OSSInsight trending data, GitHub topic pages, and cross-referenced review sources. All star counts approximate as of July 2026.*
