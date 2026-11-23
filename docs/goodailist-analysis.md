# Top Open-Source AI Repos: Analysis for Vitalik's Workflow

> Compiled 2026-11-23 from goodailist.com/repos and cross-referenced against multiple curated lists (topairepos.com, bitdoze.com, firecrawl.dev, Enterprise DNA best-of, GitHub trending).
>
> **Note:** goodailist.com was not directly accessible from the research environment. This analysis was compiled from extensive web research across multiple curated AI repo lists, GitHub trending data, and community rankings to reconstruct and expand on the repos typically featured in such directories.

---

## 1. Tools to Integrate NOW

These are ready today and would immediately improve Vitalik's daily workflow.

### 1.1 OpenCode — Terminal AI coding companion
- **GitHub:** https://github.com/opencode-ai/opencode
- **Stars:** ~165K–200K+
- **License:** MIT
- **What it does:** A terminal-native AI coding agent that works with 75+ model providers including Anthropic, OpenAI, and local models via Ollama. Supports MCP integrations, built-in planning and execution agents, and multi-file edits. It's essentially a free, open-source alternative to Cursor that lives in your terminal.
- **Why for Vitalik:** He's a heavy Claude Code user in Ghostty + tmux. OpenCode gives him a second coding agent he can point at different providers or local models for tasks where Claude Code isn't ideal (cost management, offline work, or when he wants a second opinion). The MCP support means his existing MCP servers work with it.
- **Integration effort:** Drop-in. `npm install -g opencode` and configure API keys.
- **Concerns:** With Claude Code as primary, this risks being redundant. Best used as a complement for local-model tasks or when Claude is down.

### 1.2 Crawl4AI — LLM-ready web crawler
- **GitHub:** https://github.com/unclecode/crawl4ai
- **Stars:** ~58K–87K
- **License:** Apache-2.0 (check attribution clause)
- **What it does:** A Python crawler that outputs clean markdown from web pages without requiring external API calls. Designed specifically for feeding content into LLM pipelines and RAG systems. Handles JavaScript-heavy sites, extracts structured data, and works with async workflows.
- **Why for Vitalik:** Direct building block for **hawker** (scraping marketplace listings) and **CareerOS** (scraping job boards). Also useful for focuspipe research pipelines. It outputs LLM-ready markdown, which pairs perfectly with Claude for analysis.
- **Integration effort:** Weekend project. `pip install crawl4ai` and write extraction pipelines.
- **Concerns:** The attribution clause in the license is worth reading carefully for commercial use. Some marketplace sites actively block scrapers.

### 1.3 Browser Use — AI browser agent
- **GitHub:** https://github.com/browser-use/browser-use
- **Stars:** ~78K+
- **License:** MIT
- **What it does:** A Python library that lets AI agents control a real browser — clicking, typing, navigating, extracting data. It wraps Playwright and adds AI-driven decision-making on top. The agent can handle multi-step browser tasks autonomously.
- **Why for Vitalik:** This is the automation backbone for **hawker**. Instead of brittle CSS selectors, the AI agent can navigate marketplace UIs, handle CAPTCHAs, deal with layout changes, and execute multi-step listing workflows. Also useful for CareerOS job application flows.
- **Integration effort:** Weekend project. Python library, requires Playwright and an LLM API key.
- **Concerns:** Marketplace ToS violations are a real risk. The hosted cloud version is separate from the open-source library — stick to self-hosted. Resource-intensive (runs a real browser).

### 1.4 n8n — Workflow automation with AI agents
- **GitHub:** https://github.com/n8n-io/n8n
- **Stars:** ~34K+
- **License:** Sustainable Use License (fair-code, not OSI-approved)
- **What it does:** A visual workflow automation platform with 400+ integrations. The 2026 version includes a built-in AI agent builder with memory, tools, and guardrails. Connect to OpenAI, Anthropic, or local models. Self-hostable with Docker.
- **Why for Vitalik:** The connective tissue between his projects. n8n can orchestrate: Screenpipe → focuspipe analysis, marketplace alerts → hawker actions, job board monitoring → CareerOS pipeline. The visual builder is ADHD-friendly — you can see the entire workflow at a glance. It replaces custom glue code between services.
- **Integration effort:** Weekend project to self-host. `docker-compose up` and start connecting nodes.
- **Concerns:** Not true open source (fair-code license restricts commercial resale). The UI can become complex with many workflows. Resource usage on a Mac Mini is moderate but noticeable.

### 1.5 Ollama — Local LLM server
- **GitHub:** https://github.com/ollama/ollama
- **Stars:** ~100K+
- **License:** MIT
- **What it does:** One-command install to run LLMs locally on macOS, Linux, or Windows. Provides an OpenAI-compatible API on port 11434, supports a large model library, and handles model management (download, run, switch). Apple Silicon acceleration works out of the box.
- **Why for Vitalik:** Privacy-first philosophy demands local inference for sensitive tasks — personal finance analysis, ADHD patterns, private communications. Ollama on his Mac Mini M-series is the foundation for every local-AI tool in this list. Many tools (OpenCode, Open WebUI, n8n, Privemail) connect to Ollama out of the box.
- **Integration effort:** Drop-in. `brew install ollama && ollama pull llama3.2` and you're running.
- **Concerns:** Quality gap vs. Claude for complex tasks. Mac Mini M-series can run 7B–14B models well; 70B+ will be slow. Keep it as a complement to Claude, not a replacement.

### 1.6 Kleinanzeigen Bot — Marketplace automation
- **GitHub:** https://github.com/Second-Hand-Friends/kleinanzeigen-bot
- **Stars:** Community-maintained, active
- **License:** Check repo
- **What it does:** Automates posting and managing ads on kleinanzeigen.de (formerly eBay Kleinanzeigen). Handles ad creation, renewal, and management via Playwright. Ships binaries for macOS, Linux, Windows, plus Docker.
- **Why for Vitalik:** Direct integration for **hawker**. This is a mature, maintained tool for the German marketplace he likely uses (Frankfurt-based). Instead of building Kleinanzeigen automation from scratch, hawker can build on top of or integrate with this bot.
- **Integration effort:** Drop-in for Kleinanzeigen. Weekend project to integrate into hawker's broader workflow.
- **Concerns:** Marketplace ToS risk — accounts can be banned. The bot's predecessor was discontinued, suggesting the platform actively fights automation.

### 1.7 Career-Ops — AI job search agent
- **GitHub:** https://github.com/career-ops-hq/career-ops
- **Stars:** ~67K+ (reported, verify)
- **License:** MIT (reported)
- **What it does:** An open-source AI agent that scans job boards, scores listings 1–5 against your CV, tailors ATS-friendly resumes and cover letters, and provides an application tracker. Runs locally in Claude Code, Codex, or other AI CLIs, or as a Claude Code plugin.
- **Why for Vitalik:** Direct building block or competitor for **CareerOS**. At minimum, study its architecture. At best, integrate its job-scoring engine into CareerOS or use it directly. The fact that it runs as a Claude Code plugin fits perfectly into his workflow.
- **Integration effort:** Drop-in as a user tool. Significant effort to integrate into CareerOS's codebase.
- **Concerns:** Verify the star count and actual maintenance status. The 67K figure came from a third-party blog. Check if the scoring quality is actually good before building on it.

---

## 2. Tools Worth Watching

Promising but need evaluation or aren't quite ready for Vitalik's setup.

### 2.1 OpenClaw — Personal AI agent
- **GitHub:** https://github.com/openclaw/openclaw
- **Stars:** ~280K–390K (massive adoption)
- **License:** MIT
- **What it does:** A self-hosted personal AI assistant that connects to WhatsApp, Telegram, Slack, Discord, Signal, iMessage, and more via a central Gateway. Supports voice, visual Canvas workspace, cron jobs, and extensible skills from a workspace folder.
- **Why watch:** Could become Vitalik's unified AI interface across all messaging platforms. The skill system means focuspipe, hawker, and CareerOS could all surface through one agent. But it runs tools on the host machine with full access — security model needs careful evaluation.
- **Why not now:** Young project (explosive star growth in early 2026 raises questions). Security implications of running an agent with full host access through messaging apps are significant. Needs hardening before trusting with sensitive workflows.

### 2.2 Screenpipe — Screen + audio recording for AI
- **GitHub:** https://github.com/mediar-ai/screenpipe
- **Stars:** ~22K
- **License:** Screenpipe Commercial License (NOT MIT/OSI despite some claims)
- **What it does:** Continuously records screen and microphone, indexes frames and audio in a local database. Only captures on activity change. Supports Ollama for local processing. MCP server available. The foundation that focuspipe is built on.
- **Why watch:** Vitalik is already evaluating this. The MCP server means Claude Code can query screen history directly. The commercial license is the main concern — it's source-available, not open source. Watch for community forks or license changes.
- **Why not now:** License uncertainty for commercial/derivative use (focuspipe builds on it). Privacy model needs careful audit — verify exactly what's captured and stored.

### 2.3 Dify — LLM app development platform
- **GitHub:** https://github.com/langgenius/dify
- **Stars:** ~157K–160K
- **License:** Open-source (check specific license)
- **What it does:** A visual platform for building LLM applications with RAG, agents, and workflow orchestration. Includes a prompt IDE, model management, and observability tools. Self-hostable.
- **Why watch:** Could replace custom agent-building code across all three projects. The RAG pipeline is mature enough for CareerOS document analysis. But it's heavier than n8n for simple automation and may be overkill for a solo developer.
- **Why not now:** Overlaps with n8n for workflow automation. Steeper learning curve. Better suited when Vitalik needs production-grade RAG or complex multi-agent systems.

### 2.4 Glance — Personal dashboard
- **GitHub:** https://github.com/glanceapp/glance
- **Stars:** ~36K
- **License:** Check repo
- **What it does:** A self-hosted personal dashboard that aggregates RSS feeds, service status widgets, and custom data sources on a single page. Clean, fast, mobile-friendly.
- **Why watch:** Could serve as Vitalik's command center — Linear tickets, GitHub notifications, marketplace alerts, job board updates, all in one tab. ADHD-friendly because it's passive (you check one page instead of five apps).
- **Why not now:** Needs custom widgets for his specific data sources. The widget ecosystem may not cover Linear, YNAB, or TickTick natively.

### 2.5 AnythingLLM — Self-hosted RAG platform
- **GitHub:** https://github.com/Mintplex-Labs/anything-llm
- **Stars:** ~30K+
- **License:** MIT
- **What it does:** A full-stack application for private document chat. Upload documents, it chunks and embeds them, and you chat with your data using any LLM (commercial or local via Ollama). Supports multi-user, workspaces, and agent capabilities.
- **Why watch:** Could power focuspipe's context-aware suggestions — feed it your work documents, meeting notes, and project files, then ask "what should I focus on today?" Also useful for CareerOS to chat with job descriptions and company research.
- **Why not now:** Setting up a good RAG pipeline takes tuning. The quality of responses depends heavily on chunking strategy and embedding model choice. Worth a weekend experiment.

### 2.6 Open WebUI — LLM chat interface
- **GitHub:** https://github.com/open-webui/open-webui
- **Stars:** ~60K+
- **License:** MIT
- **What it does:** A web-based chat interface that connects to Ollama, llama.cpp, vLLM, or any OpenAI-compatible API. Supports multi-model conversations, document upload, web search, and function calling.
- **Why watch:** When Vitalik runs local models via Ollama, this gives him a ChatGPT-like UI for them. Useful for tasks where the terminal isn't ideal (long document analysis, visual conversations).
- **Why not now:** Vitalik is a terminal-first user (Ghostty + tmux). This adds value mainly for non-coding tasks or when sharing AI access with others.

### 2.7 Whispering / OpenWhispr — Local speech-to-text
- **GitHub:** https://github.com/braden-w/whispering (Whispering) / search for OpenWhispr
- **Stars:** Varies
- **License:** Open source
- **What it does:** Fully local, offline speech-to-text using Whisper models. Press a shortcut, speak, text appears wherever your cursor is. No cloud dependency.
- **Why watch:** ADHD-friendly input method. When Vitalik struggles to start writing (executive function barrier), voice input bypasses the blank-page problem. Also useful for quick task capture without switching contexts.
- **Why not now:** Latency on Apple Silicon needs testing. Quality depends on model size and background noise. Worth a quick test.

---

## 3. Building Blocks for Vitalik's Projects

### For focuspipe (ADHD monitoring)

| Repo | What it provides | Integration path |
|------|-----------------|-----------------|
| **Screenpipe** (mediar-ai/screenpipe) | Screen + audio capture, activity detection | Already the foundation — focuspipe is a Screenpipe pipe |
| **Ollama** (ollama/ollama) | Local LLM inference for pattern detection | Run focus-classification models locally, no cloud needed |
| **i-have-adhd** (GitHub) | Claude instruction set for ADHD-friendly output | Style layer for focuspipe's notifications and summaries |
| **ADHD Focus Mate** (skainguyen1412/adhd-focus-mate) | macOS menu bar AI focus tracker | Study its classification approach; potential merge target |
| **Whisper** (openai/whisper) | Audio transcription | Detect verbal task-switching, meeting context capture |
| **AnythingLLM** (Mintplex-Labs/anything-llm) | RAG for document context | "What should I focus on?" queries against work documents |

### For hawker (marketplace bot)

| Repo | What it provides | Integration path |
|------|-----------------|-----------------|
| **Browser Use** (browser-use/browser-use) | AI-driven browser automation | Core automation engine for marketplace interactions |
| **Crawl4AI** (unclecode/crawl4ai) | LLM-ready web scraping | Listing data extraction, price monitoring |
| **Kleinanzeigen Bot** (Second-Hand-Friends/kleinanzeigen-bot) | Kleinanzeigen automation | Direct integration or reference implementation |
| **ebay-kleinanzeigen-api-mcp** (Sprayer115) | MCP server for Kleinanzeigen | Claude Code can search/manage listings via MCP |
| **n8n** (n8n-io/n8n) | Workflow orchestration | Connect: alert → scrape → analyze → list → notify |
| **PaddleOCR-VL** / **Tesseract** | Receipt/invoice scanning | Automate profit tracking from marketplace sales |

### For CareerOS (job search)

| Repo | What it provides | Integration path |
|------|-----------------|-----------------|
| **Career-Ops** (career-ops-hq/career-ops) | Job scoring, resume tailoring | Core engine or reference architecture |
| **Crawl4AI** (unclecode/crawl4ai) | Job board scraping | Extract listings from multiple boards |
| **Browser Use** (browser-use/browser-use) | Application form automation | Semi-automated application submission |
| **AnythingLLM** (Mintplex-Labs/anything-llm) | Company research RAG | Chat with company docs, job descriptions |
| **n8n** (n8n-io/n8n) | Pipeline orchestration | Scrape → score → tailor → apply → track |
| **Ollama** (ollama/ollama) | Local resume analysis | Privacy-safe resume processing |

---

## 4. Grant Inspiration

These repos suggest adjacent project ideas worth exploring for grants (NLnet, Prototype Fund, Mozilla, Sovereign Tech Fund).

### 4.1 ADHD-aware AI interfaces
- **Gap:** Most AI tools assume neurotypical attention patterns. focuspipe is one of very few tools that explicitly designs for ADHD.
- **Grant angle:** "Accessible AI: executive function support through passive monitoring" — frames focuspipe as an accessibility project, which opens EU/German funding (Prototype Fund DE specifically funds German-based open-source developers).
- **Inspired by:** Screenpipe's growth (22K stars) shows demand for personal AI monitoring. The ADHD Focus Mate project validates the specific niche.

### 4.2 Privacy-preserving personal AI agents
- **Gap:** OpenClaw and similar agents run everything on-device but lack formal privacy guarantees. No open-source personal agent has differential privacy or formal auditing.
- **Grant angle:** "Sovereign Personal AI: a formally audited, privacy-preserving personal agent framework" — aligns with EU digital sovereignty goals and Sovereign Tech Fund priorities.
- **Inspired by:** OpenClaw (390K stars), Screenpipe, and the EU AI Act's requirements for transparent AI systems.

### 4.3 Open marketplace tooling
- **Gap:** Marketplace automation is fragmented (Kleinanzeigen bot, scattered Vinted scripts). No unified open-source framework for multi-marketplace selling.
- **Grant angle:** "Open Marketplace Protocol: an interoperable standard for peer-to-peer marketplace APIs" — frames hawker's underlying problem as infrastructure, not just a bot. NLnet funds internet commons projects.
- **Inspired by:** The kleinanzeigen-bot ecosystem and the absence of Vinted/eBay open-source equivalents.

### 4.4 Neurodiversity-first developer tools
- **Gap:** Developer tools assume consistent focus. No IDE or terminal tool adapts to executive function state.
- **Grant angle:** "Adaptive Developer Environments: developer tools that respond to cognitive state" — builds on focuspipe's monitoring to make dev tools context-aware. Prototype Fund or Mozilla's responsible AI grants.
- **Inspired by:** The i-have-adhd Claude skill (59K+ stars) proves massive demand for ADHD-adapted AI interactions.

### 4.5 Local-first AI for financial autonomy
- **Gap:** Personal finance AI tools either require cloud (YNAB, Monarch) or lack AI (Actual Budget, Firefly III). No local-first finance tool combines budgeting with AI analysis.
- **Grant angle:** "Financial AI Sovereignty: local-first AI-powered personal finance" — ties into digital sovereignty, financial inclusion, and privacy. The Dough project (connects YNAB to Claude) validates the demand.
- **Inspired by:** Actual Budget (29.7K stars), Maybe (AGPL), and the Dough YNAB+Claude connector.

---

## 5. Skip List

These are popular repos that are NOT relevant to Vitalik's setup.

| Repo | Stars | Why skip |
|------|-------|----------|
| **ComfyUI / Stable Diffusion** | ~135K | Image generation is not part of any of Vitalik's projects. GPU-intensive and Mac Mini M-series isn't ideal for it. |
| **vLLM** | High | Production multi-user LLM serving. Needs NVIDIA GPUs. Overkill for a solo developer — Ollama covers his use case. |
| **LangChain** (as a framework) | ~135K–150K | Overly complex for Vitalik's needs. The abstractions add cognitive overhead that works against ADHD. n8n or direct API calls are simpler. |
| **AutoGen** | ~55K–61K | In maintenance mode as of 2026. Not worth building on. |
| **Flowise** | Varies | Reached end of life August 31, 2026. Dead project. |
| **Cursor** | Popular | Not open source. Vitalik already has Claude Code + Ghostty, which is a better terminal-first setup. |
| **LM Studio** | Popular | Not open source (proprietary, free for personal use). Ollama covers the same need and is actually MIT. |
| **DeepSeek-V3** (model) | High stars | The model itself isn't a tool. If Vitalik wants to run it, Ollama handles it. The repo is just model weights. |
| **Automatic1111 WebUI** | ~140K | In maintenance mode since March 2026. Use ComfyUI if image gen is ever needed, but see above — it's not. |
| **OpenHands** | Varies | A full agent platform that overlaps heavily with Claude Code. Adding it creates tool sprawl without clear benefit. |
| **Mastra** | Growing | TypeScript agent framework. Vitalik already has Claude Code's agent capabilities and n8n for orchestration. Another framework is unnecessary complexity. |
| **RAGFlow** | Growing | Enterprise-grade RAG with complex setup. AnythingLLM is simpler for a solo developer. |
| **Dify** (for now) | ~160K | Powerful but heavy. n8n covers workflow automation with less overhead. Revisit if Vitalik needs production RAG or multi-agent orchestration. |

---

## Summary: Priority Action Plan

### This week
1. **Install Ollama** on the Mac Mini — it's the foundation for everything local-AI
2. **Try Career-Ops** as a Claude Code plugin — directly useful for job search
3. **Evaluate kleinanzeigen-bot** for hawker integration

### This month
4. **Set up n8n** via Docker — start connecting Screenpipe → focuspipe → notifications
5. **Integrate Crawl4AI** into hawker's scraping pipeline
6. **Test Browser Use** for marketplace automation prototyping

### This quarter
7. **Experiment with AnythingLLM** + Ollama for private document chat
8. **Watch OpenClaw** for security improvements and community maturity
9. **Apply to Prototype Fund DE** with focuspipe as an ADHD-accessible AI project
10. **Evaluate Glance** as a unified personal dashboard

---

## Sources

Research compiled from:
- [Bitdoze: 60+ Top AI GitHub Repos in 2026](https://www.bitdoze.com/top-ai-github-repos/)
- [TopAIRepos.com](https://topairepos.com/)
- [Enterprise DNA: Best of Open-Source AI](https://enterprisedna.co/directories/open-source/best-of-open-source-ai)
- [Firecrawl: Best Open-Source Agent Frameworks 2026](https://www.firecrawl.dev/blog/best-open-source-agent-frameworks)
- [DEV Community: 20 Biggest Open-Source AI Agent Projects (Nov 2026)](https://dev.to/ai_sovereign/the-20-biggest-open-source-ai-agent-projects-on-github-november-2026-5dpg)
- [Fungies: Top 20 GitHub Repos for AI Agents 2026](https://fungies.io/top-github-repositories-ai-agent-frameworks-2026/)
- [DataAIHub: Top 20 AI GitHub Repos 2026 Mid-Year](https://www.dataaihub.co/research/top-ai-github-repositories-2026)
- [GitHub Topics: ADHD Tools](https://github.com/topics/adhd-tools)
- [GitHub: Career-Ops](https://github.com/career-ops-hq/career-ops)
- [Open Alternative: YNAB Alternatives](https://openalternative.co/alternatives/ynab)
- [Best-of MCP Servers (GitHub)](https://github.com/tolkonepiu/best-of-mcp-servers)
- [Glama: Best GitHub MCP Servers (Nov 2026)](https://glama.ai/mcp/best/github)
- [Screenpipe Launch HN (YC S26)](https://hn.nuxt.dev/item/49024620)
- [Pinggy: Best CLI Coding Agents 2026](https://pinggy.io/blog/best_open_source_cli_coding_agents/)
- [What's Trending: AI Repos](https://whatstrending.ai/repos)
