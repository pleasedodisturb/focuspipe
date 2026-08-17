# Top Open-Source AI Repos — Analysis for Vitalik

> Curated from goodailist.com/repos and cross-referenced with GitHub trending, OSSInsight, ByteByteGo, Firecrawl, and Fungies rankings as of August 2026.
>
> Context: Solo AI developer, Frankfurt. Mac Mini M-series + MacBook. Heavy Claude Code user with MCP servers. ADHD — needs passive tools. Privacy-conscious, local-first. Building: focuspipe (ADHD monitoring), hawker (marketplace bot), CareerOS (job search). Uses: Syncthing, Bitwarden, Ghostty, tmux, Playwright, Linear, TickTick.

---

## 1. Tools to Integrate NOW

These will immediately improve your daily workflow with minimal setup.

### OpenClaw — Personal AI Assistant
- **GitHub:** https://github.com/openclaw/openclaw
- **Stars:** ~214,000+
- **What it does:** Local-first personal AI agent that connects to 50+ messaging platforms (WhatsApp, Telegram, Slack, Signal, iMessage, Discord). Runs on your machine, remembers context across conversations, browses the web, fills forms, runs shell commands, and autonomously writes new skills to extend itself. Now a non-profit backed by a full-time team.
- **Why it's relevant:** This is the "always-on AI layer" you need. It bridges Claude Code's power to your daily messaging — ask questions from your phone via Telegram, have it monitor things, extract data from sites. The self-extending skill system means it gets smarter as you use it. No subscription — bring your own API key.
- **Integration effort:** Weekend project. Docker install, configure messaging integrations, point at your Anthropic API key.
- **Concerns:** 20GB+ storage for full features. CPU usage ~5-10%. API costs for Claude calls. Community is massive but fast-moving — breaking changes possible.

### career-ops — AI Job Search Pipeline
- **GitHub:** https://github.com/santifer/career-ops
- **Stars:** ~43,000
- **What it does:** An AI-powered job search system built natively on Claude Code with 14 slash-command skill modes. Scans 45+ configurable company portals for new postings, scores offers A-F with structured rubrics into 1.0-5.0 scores, generates ATS-optimized CV PDFs via Playwright, tracks applications, and runs batch parallel evaluations. Built by someone who used it to land a Head of Applied AI role from 740 listings → 68 applications → 12 interviews → 1 offer.
- **Why it's relevant:** This IS your CareerOS — or at least the engine for it. It already runs in Claude Code, uses Playwright (which you already have), and is CLI-agnostic. The 14 slash-command modes (scan, pdf, batch, tracker) map directly to what CareerOS needs. Fork it, customize the portal YAMLs for your target companies, and you have a working career pipeline today.
- **Integration effort:** Drop-in. Install the Claude Code skills, configure your profile YAML and portal list, run `/career-ops scan`.
- **Concerns:** MIT licensed, free forever. Only cost is your existing Claude Code subscription. Active maintenance as of May 2026.

### Screenpipe — AI Screen Memory
- **GitHub:** https://github.com/screenpipe/screenpipe
- **Stars:** ~20,000+
- **What it does:** YC S26 company. Continuous 24/7 local screen and audio recording with AI-powered searchable memory. Captures via accessibility APIs (OCR fallback), transcribes audio via Whisper, stores everything in local SQLite. Runs AI agents ("Pipes") triggered by your activity. MCP server included. On supported Macs, uses Apple Intelligence for on-device summaries.
- **Why it's relevant:** This is a direct building block for focuspipe. The screen activity capture, attention monitoring, and local storage architecture is exactly what ADHD monitoring needs. The MCP integration means Claude Code can query your screen history. The Pipe system lets you build custom ADHD-focused agents that react to your behavior patterns (context switching, doom-scrolling detection, focus session tracking).
- **Integration effort:** Weekend project. One-time $400 desktop app purchase or build from source (MIT). ~5-10% CPU, ~20GB/month storage.
- **Concerns:** Storage grows fast at 20GB/month. The $400 price is for the polished app — you can build from source for free. Privacy is excellent: everything stays local.

### Crawl4AI — Async Web Crawler for AI
- **GitHub:** https://github.com/unclecode/crawl4ai
- **Stars:** ~40,000+
- **What it does:** Fully async Python web crawler purpose-built for feeding LLMs. Handles JavaScript rendering, extracts structured data, outputs clean markdown. Adaptive — when a site changes its DOM, it adjusts without human intervention. ~$4.85 per 1k pages.
- **Why it's relevant:** Direct building block for hawker (marketplace monitoring) and CareerOS (job portal scraping). Cheaper than Firecrawl, gives you full control, and the adaptive DOM handling means your marketplace scrapers won't break every time eBay/Amazon tweaks their layout. Pairs perfectly with Claude Code for processing extracted data.
- **Integration effort:** Drop-in Python library. `pip install crawl4ai`, write your scraping pipeline.
- **Concerns:** You handle your own infrastructure. Rate limiting and anti-bot measures are your responsibility.

### Mem0 — Universal Memory Layer for AI
- **GitHub:** https://github.com/mem0ai/mem0
- **Stars:** ~48,000
- **What it does:** Managed memory layer combining vector search, knowledge graph, and key-value caching in a single API. Accumulates memories without overwriting, extracts and links entities, supports multi-signal retrieval (semantic + keyword + entity), and handles temporal reasoning. YC-backed, $24M Series A.
- **Why it's relevant:** Persistent memory across all your AI tools. Right now, each Claude Code session starts fresh. Mem0 lets focuspipe remember your patterns across days/weeks, hawker remembers market trends and pricing history, CareerOS remembers which companies you've contacted and what feedback you got. The temporal reasoning is key for ADHD — "what was I working on last Thursday before I got distracted?"
- **Integration effort:** Weekend project. Python SDK, self-hostable or cloud API. Plug into your existing Claude Code workflows.
- **Concerns:** Self-hosted version needs a vector DB (Qdrant or similar). Cloud version sends data to their servers — evaluate privacy tradeoff.

### n8n — Workflow Automation
- **GitHub:** https://github.com/n8n-io/n8n
- **Stars:** ~70,000+
- **What it does:** Visual workflow automation platform with 400+ integrations, AI agent nodes, and self-hosting support. Connect APIs, databases, and AI models with a drag-and-drop interface. Fair-code licensed.
- **Why it's relevant:** The glue between all your tools. Set up automated workflows: "when a new marketplace listing matches my criteria in hawker → evaluate with Claude → post to Telegram." Or: "when Screenpipe detects 30 minutes of unfocused browsing → send a TickTick reminder." The AI agent nodes let you build complex multi-step automations without code.
- **Integration effort:** Weekend project. Docker self-hosted instance. The visual builder means you can prototype workflows fast.
- **Concerns:** Fair-code license is more restrictive than MIT for commercial hosting. Self-hosted version is fully functional. Memory usage can grow with many active workflows.

---

## 2. Tools Worth Watching

Not ready for immediate integration, but track these.

### Perplexica (now Vane) — Self-Hosted AI Search
- **GitHub:** https://github.com/ItzCrazyKns/Perplexica
- **Stars:** ~33,000
- **What it does:** Open-source Perplexity alternative. Searches the web via SearXNG, reranks with embeddings, generates cited responses using local or cloud LLMs. Ships as a single Docker image. Rebranded to Vane in March 2026.
- **Why it matters:** Self-hosted research engine. Currently you'd use Claude Code for research, but a dedicated search engine with citation tracking would complement it — especially for CareerOS company research and hawker market analysis.
- **Watch because:** The SearXNG dependency adds complexity. Wait for the Vane rebrand to stabilize.

### Leantime — ADHD Project Management
- **GitHub:** https://github.com/Leantime/leantime
- **Stars:** ~5,000+
- **What it does:** Open-source project management built specifically for ADHD and neurodivergent brains. Visual clarity, predictable flows, concise interfaces, emoji-based motivation tracking, dopamine-loop progress donuts, and AI-powered task prioritization. Self-hostable PHP app. Techstars '23.
- **Why it matters:** Designed ground-up for ADHD instead of bolting ADHD features onto a neurotypical tool. The emoji motivation system and dopamine-loop design are research-backed approaches to ADHD productivity.
- **Watch because:** You already use Linear + TickTick. Adding a third PM tool increases cognitive load. But if Linear's structure isn't working for your ADHD, Leantime's approach is worth evaluating. Could also inspire focuspipe's UI/UX.

### Hermes Agent — Self-Improving Agent Framework
- **GitHub:** https://github.com/NousResearch/hermes-agent
- **Stars:** Growing rapidly
- **What it does:** Open-source, self-improving agentic framework from Nous Research. Runs capable multi-step agents on your own infrastructure. Designed for developers who want autonomous agents that get better over time.
- **Why it matters:** The self-improvement loop is interesting for focuspipe — an agent that learns your ADHD patterns and adapts its interventions. Integrates with Screenpipe.
- **Watch because:** Still early. Nous Research is credible but the framework is evolving fast.

### Browser Use — AI Browser Automation
- **GitHub:** https://github.com/browser-use/browser-use
- **Stars:** ~60,000+
- **What it does:** Makes websites accessible for AI agents. Gives LLMs the ability to interact with web pages — clicking, typing, navigating, extracting data — using natural language instructions.
- **Why it matters:** Natural upgrade path for hawker. Instead of scraping HTML, you tell an agent "go to eBay, search for X, extract all listings under $50." More resilient than DOM-based scraping.
- **Watch because:** High CPU/memory usage for browser sessions. Playwright (which you already use) covers most of your needs. But the natural language control layer is compelling for complex marketplace interactions.

### Docling — Document Intelligence
- **GitHub:** https://github.com/docling-project/docling
- **Stars:** ~20,000+
- **What it does:** IBM's document parsing library. Converts PDFs, DOCX, PPTX, images, and more to structured markdown or JSON. Advanced table recognition, OCR, and layout analysis.
- **Why it matters:** CareerOS needs to parse job descriptions from PDFs, company reports, etc. hawker might need to process product specs. Docling handles the messy document-to-structured-data pipeline.
- **Watch because:** Works well now but the model downloads are large. Evaluate when you actually need document parsing at scale.

### Marker — PDF to Markdown
- **GitHub:** https://github.com/VikParuchuri/marker
- **Stars:** ~20,000+
- **What it does:** Converts PDFs to clean markdown with high accuracy. Handles complex layouts, tables, and figures. Local processing, no cloud dependency.
- **Why it matters:** Cleaner PDF processing for CareerOS resume/job description handling. Pairs with Claude Code for analysis.
- **Watch because:** Overlaps with Docling. Pick one when you need it.

### i-have-adhd — ADHD-Friendly AI Output Skill
- **GitHub:** https://github.com/topics/adhd (search "i-have-adhd")
- **What it does:** A skill/prompt for AI programming assistants that prevents information overload. Produces concise, ADHD-friendly outputs — no walls of text, no excessive options, no decision fatigue.
- **Why it matters:** Could be integrated as a Claude Code skill for focuspipe or your personal setup. When you're in a low-focus state, your AI interactions should adapt.
- **Watch because:** New project, needs to prove its approach. But the concept directly maps to focuspipe's mission.

---

## 3. Building Blocks for Your Projects

### For focuspipe (ADHD Monitoring)

| Repo | How to Use It | GitHub |
|------|---------------|--------|
| **Screenpipe** | Core screen/audio capture engine. Fork or integrate via MCP. Use its activity detection for focus scoring. | github.com/screenpipe/screenpipe |
| **Mem0** | Persistent memory for tracking ADHD patterns across sessions. "You tend to lose focus around 2pm on Tuesdays." | github.com/mem0ai/mem0 |
| **Leantime** | Study its ADHD-specific UX patterns (dopamine loops, motivation tracking, reduced decision points) for focuspipe's interface design. | github.com/Leantime/leantime |
| **OpenClaw** | Integration target — focuspipe alerts delivered to your messaging apps via OpenClaw's channel system. | github.com/openclaw/openclaw |
| **n8n** | Orchestrate focus interventions: Screenpipe detects distraction → n8n triggers focuspipe alert → OpenClaw sends to Telegram. | github.com/n8n-io/n8n |

### For hawker (Marketplace Bot)

| Repo | How to Use It | GitHub |
|------|---------------|--------|
| **Crawl4AI** | Primary scraping engine for marketplace monitoring. Adaptive DOM handling means less maintenance when sites change. | github.com/unclecode/crawl4ai |
| **Browser Use** | Complex marketplace interactions (bidding, listing, messaging) via natural language. Upgrade path from raw scraping. | github.com/browser-use/browser-use |
| **Firecrawl** | Alternative to Crawl4AI when you need managed infrastructure. Better for quick prototyping, costlier at scale. | github.com/mendableai/firecrawl |
| **Mem0** | Track pricing trends, buyer patterns, seasonal demand. Persistent marketplace intelligence. | github.com/mem0ai/mem0 |
| **n8n** | Automate the full pipeline: scrape → evaluate → list → track → notify. | github.com/n8n-io/n8n |
| **LiteLLM** | Unified proxy for switching between LLMs based on task — cheap models for listing descriptions, Claude for pricing strategy. | github.com/BerriAI/litellm |

### For CareerOS (Job Search)

| Repo | How to Use It | GitHub |
|------|---------------|--------|
| **career-ops** | Fork as the foundation. It already has portal scanning, scoring, CV generation, and Claude Code integration. | github.com/santifer/career-ops |
| **ai-job-search** | Study the workflow — 69 tailored applications → 20 interviews → 1 signed contract. Proven approach. | github.com/MadsLorentzen/ai-job-search |
| **Resume-Matcher** | Resume-to-JD matching with keyword gap analysis. Complement career-ops scoring. | github.com/srbhr/Resume-Matcher |
| **JobSync** | Self-hosted application tracker with AI resume review. Private, local-first. | github.com/Gsync/jobsync |
| **Crawl4AI** | Scrape job portals beyond career-ops' 45 built-in ones. Custom portal adapters. | github.com/unclecode/crawl4ai |
| **Docling/Marker** | Parse job descriptions and company reports from PDFs for deeper analysis. | github.com/docling-project/docling |

---

## 4. Grant Inspiration

These repos suggest adjacent project ideas worth pursuing for grant funding.

### ADHD + AI Productivity (NSF, NIH, EU Horizon)
- **Concept:** An open-source, AI-powered ADHD support system that uses passive monitoring (Screenpipe), pattern recognition (Mem0), and adaptive interventions (focuspipe) to help neurodivergent adults maintain productivity without requiring discipline.
- **Inspiration from:** Screenpipe's passive capture, Leantime's ADHD-specific design, i-have-adhd's adaptive output concept.
- **Grant angle:** Mental health technology, assistive technology, workplace accessibility. EU has strong funding for digital health tools. NIH funds neurodivergent productivity research.
- **Adjacent repos:** Oculus Vigilis (webcam attention monitoring), Leantime, awesome-adhd curated list.

### Local-First AI Personal Data Sovereignty
- **Concept:** A framework for running personal AI agents that process sensitive data (health, finances, career) entirely on local hardware, with zero cloud dependency.
- **Inspiration from:** OpenClaw's local-first architecture, Jan/Ollama local LLM running, Screenpipe's local SQLite storage.
- **Grant angle:** Digital sovereignty, privacy-preserving AI, GDPR compliance tools. Strong EU funding potential given Frankfurt base.
- **Funding sources:** EU Horizon Europe, German BMWK digital sovereignty programs, Mozilla Foundation, NLnet Foundation.

### AI-Powered Marketplace Equity Tools
- **Concept:** Open-source tools that level the playing field for individual marketplace sellers against corporate sellers using expensive analytics platforms.
- **Inspiration from:** hawker's approach, OctoBot's trading automation, Typebot's conversational commerce.
- **Grant angle:** Economic equity, small business technology, digital marketplace access.
- **Funding sources:** EU SME support programs, Shuttleworth Foundation.

### Neurodivergent Developer Tooling
- **Concept:** A Claude Code skill ecosystem specifically designed for neurodivergent developers — adaptive verbosity, executive function support, context persistence, gentle deadline tracking.
- **Inspiration from:** i-have-adhd skill, career-ops' slash-command approach, focuspipe's monitoring.
- **Grant angle:** Developer productivity, accessible technology, neurodiversity in tech.
- **Funding sources:** AI Grant (aigrant.org), GitHub Fund, Google.org.

---

## 5. Skip List

Popular repos that are NOT relevant to your setup.

| Repo | Stars | Why Skip |
|------|-------|----------|
| **DeepSeek-V3/R1** | 100k+ | Open-weight frontier model. You use Claude via API — no need to run your own LLM. Massive hardware requirements. |
| **ComfyUI** | 106k | Node-based image/video generation. Unless you need AI art for marketplace listings, this is a rabbit hole. |
| **Stable Diffusion WebUI** | 150k+ | Same as ComfyUI — image generation isn't in your workflow. |
| **vLLM** | 50k+ | High-performance LLM serving. You don't self-host models. Claude API is your inference layer. |
| **Transformers (HuggingFace)** | 158k+ | Foundational ML library. Unless you're training models, this is infrastructure you don't need to touch directly. |
| **LangChain** | 110k+ | Foundational agent framework but over-abstracted for your use case. Claude Code + direct API calls is simpler and more maintainable. |
| **LangFlow** | 146k | Visual agent builder. You write code in Claude Code — a visual builder adds a layer you don't need. |
| **Dify** | 136k | Same as LangFlow — impressive platform but you're not building visual AI apps. |
| **AutoGen (Microsoft)** | 50k+ | Multi-agent orchestration framework. Useful at enterprise scale, overkill for solo developer. Claude Code's workflow system covers this. |
| **MetaGPT** | 50k+ | Multi-agent software company simulation. Research project energy, not production tool. |
| **MLflow** | 20k+ | ML lifecycle management for training pipelines. You use pre-trained models via API. |
| **Flowise** | 51k | Another visual builder. Same skip reason as LangFlow/Dify. |
| **SillyTavern** | 15k+ | LLM roleplay frontend. Not your use case. |
| **CrewAI** | 30k+ | Multi-agent framework. You already have Claude Code's agent orchestration. Adding another framework increases complexity without clear benefit. |
| **Leon AI** | 15k+ | Personal assistant. OpenClaw is the better choice — larger community, more integrations, more active development. |
| **Ollama** | 120k+ | Local LLM runner. You use Claude API exclusively. Unless you want to run cheap local models for hawker classification tasks, skip for now. |
| **LM Studio** | N/A | Same as Ollama — GUI for local models. Not needed with Claude API. |

---

## Quick Reference: Priority Actions

### This Week
1. **Install career-ops** as Claude Code skills → immediate CareerOS foundation
2. **Set up Crawl4AI** → hawker marketplace scraping prototype
3. **Evaluate Screenpipe** build-from-source → focuspipe data capture layer

### This Month
4. **Deploy n8n** self-hosted → automation backbone for all three projects
5. **Integrate Mem0** → persistent memory across Claude Code sessions
6. **Try OpenClaw** → personal AI assistant on messaging platforms

### This Quarter
7. **Build focuspipe prototype** combining Screenpipe + Mem0 + n8n
8. **Draft ADHD+AI grant proposal** using EU Horizon Europe template
9. **Evaluate Browser Use** for hawker complex marketplace interactions

---

## Sources

- [ByteByteGo — Top AI GitHub Repos 2026](https://blog.bytebytego.com/p/top-ai-github-repositories-in-2026)
- [Fungies — Top 20 AI Agent Repos](https://fungies.io/top-github-repositories-ai-agent-frameworks-2026/)
- [Firecrawl — Best Trending Repos for AI Devs](https://www.firecrawl.dev/blog/best-github-repos)
- [OSSInsight — Trending AI Repos](https://ossinsight.io/trending/ai)
- [DigitalOcean — What is OpenClaw](https://www.digitalocean.com/resources/articles/what-is-openclaw)
- [career-ops official site](https://career-ops.org/)
- [Screenpipe official site](https://screenpi.pe/about)
- [Awesome MCP Tools — Top MCP Servers 2026](https://awesome-mcp.tools/blog/top-mcp-servers-2026)
- [Apidog — Top 10 MCP Servers for Claude Code](https://apidog.com/blog/top-10-mcp-servers-for-claude-code/)
- [Vellum — Best Local AI Assistants 2026](https://www.vellum.ai/blog/best-local-ai-assistants)
- [Leantime — ADHD Project Management](https://leantime.io/open-source-project-management-for-adhd-why-we-built-leantime-for-neurodivergent-productivity/)
- [OSSAlt — Self-Host Perplexica](https://ossalt.com/guides/self-host-perplexica-open-source-perplexity-2026)
- [Automation Atlas — n8n vs Activepieces](https://automationatlas.io/guides/n8n-vs-activepieces-2026-comparison/)
- [AI Grant](https://aigrant.org/)
- [ScrapeOps — Best AI Web Scraping Tools](https://scrapeops.io/web-scraping-playbook/best-ai-web-scraping-tools/)
- [GitHub Topics — ADHD Tools](https://github.com/topics/adhd-tools)
- [GitHub Topics — Job Search](https://github.com/topics/job-search)
- [Awesome ADHD](https://github.com/XargsUK/awesome-adhd)
- [Mem0 Guide 2026](https://baeseokjae.github.io/posts/mem0-agent-memory-guide-2026/)
