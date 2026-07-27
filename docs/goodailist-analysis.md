# Top Open-Source AI Repos Analysis (July 2026)

Research based on goodailist.com's curated list (18K tracked repos) and cross-referenced with trending AI repos across GitHub, OSSInsight, ByteBytego, and other sources. Filtered and analyzed specifically for Vitalik's setup: solo AI developer, ADHD, local-first/privacy-conscious, Mac Mini M-series + MacBook, heavy Claude Code user, building focuspipe/hawker/CareerOS.

---

## 1. Tools to Integrate NOW

### 1.1 Screenpipe
- **GitHub:** https://github.com/screenpipe/screenpipe
- **Stars:** 20K+ | **License:** MIT (core/CLI), paid desktop app
- **What it does:** Records your screen 24/7 locally, extracts text via OCR and accessibility APIs, transcribes audio, and stores everything in a local SQLite database. Natural language search across everything you've seen or heard. "Pipes" are scheduled AI agents defined as markdown files. YC S26.
- **Why it matters for Vitalik:** You're already evaluating it. For focuspipe specifically, Screenpipe is the missing data layer. Instead of building screen monitoring from scratch, focuspipe could consume Screenpipe's local SQLite as its attention/distraction signal source. It knows what app you're in, what window is active, what URL you're browsing. The "Pipes" system (markdown-defined AI agents) maps directly to focuspipe's monitoring concept. Connects to OpenClaw, Hermes, and 100+ apps.
- **Integration effort:** Weekend project to read its SQLite; drop-in if you just install and use it standalone.
- **Concerns:** Desktop app requires purchase (CLI is free). CPU/RAM usage on M-series is reasonable but not negligible for 24/7 recording. You'd be building a dependency on their schema.

### 1.2 career-ops
- **GitHub:** https://github.com/santifer/career-ops
- **Stars:** Growing | **License:** MIT
- **What it does:** Open-source AI job search system that runs inside your existing AI coding CLI (Claude Code, Codex, OpenCode). Scans 150+ company career pages zero-token via Playwright, scores listings against your CV using a 5-dimension rubric (1.0-5.0), generates ATS-optimized PDF resumes per role, drafts answers to Greenhouse/Ashby/Lever application questions, and tracks your pipeline in a Go terminal dashboard. Built from a real 740-listing job search.
- **Why it matters for Vitalik:** This is essentially what CareerOS is trying to be. Before building from scratch, study this architecture closely. It already runs inside Claude Code (your primary tool), handles the Playwright scraping you need, has the scoring rubric, and does CV tailoring. You could fork it, extend it with your own features, or use it as the foundation for CareerOS. The interview prep modes (plan, practice, drill) added in v1.16 are exactly the kind of ADHD-friendly structured workflow you'd want.
- **Integration effort:** Drop-in. Install the slash commands into your Claude Code setup. Customize the rubric to your preferences.
- **Concerns:** Solo maintainer project. The "zero-token" scanning claim means it uses Playwright directly without LLM calls for the initial scrape, but evaluation still burns API tokens.

### 1.3 n8n
- **GitHub:** https://github.com/n8n-io/n8n
- **Stars:** 190K+ | **License:** Fair-code (Sustainable Use License)
- **What it does:** Visual workflow automation platform with 1,400+ integrations, 70+ LangChain AI nodes, native MCP support, and 5,800+ community AI workflows. Self-host on a $5 VPS for unlimited executions. Chains LLM calls, vector DB queries, and tool-using agents visually.
- **Why it matters for Vitalik:** The glue layer between all your tools. Connect YNAB webhooks to AI analysis, automate marketplace listing workflows for hawker, set up monitoring pipelines for focuspipe alerts, chain Linear issues with Claude API calls. With ADHD, the visual workflow builder means you can see your automations rather than maintaining scattered scripts. The self-hosted approach fits your privacy-first philosophy. One Docker container replaces dozens of cron jobs and webhook handlers.
- **Integration effort:** Weekend project for basic setup. Significant investment to build complex workflows, but each one pays off immediately.
- **Concerns:** Fair-code license means you can't resell it as a service. Memory-hungry if you run many workflows simultaneously. The visual builder can become spaghetti at scale.

### 1.4 Firecrawl
- **GitHub:** https://github.com/mendableai/firecrawl
- **Stars:** 153K+ | **License:** AGPL-3.0
- **What it does:** Turns any web page into clean markdown, structured JSON, or screenshots optimized for LLM consumption. Ships an MCP server out of the box, so Claude Code can call it directly. 93% fewer tokens than raw HTML. SDKs for Python, Node.js, Go, Rust.
- **Why it matters for Vitalik:** Essential building block for hawker (marketplace scraping) and CareerOS (job portal scraping). The MCP server means your Claude Code sessions can crawl and extract structured data natively. For hawker specifically, Firecrawl's structured extraction + the markdown output means you can feed marketplace listings directly into your pricing/analysis pipeline. Much more reliable than raw Playwright scraping for content extraction.
- **Integration effort:** Drop-in as MCP server for Claude Code. Weekend project to integrate into hawker's scraping pipeline.
- **Concerns:** AGPL license means derivative works must be open-sourced if distributed. Self-hosted version is free but the cloud API has usage costs. For heavy scraping (hawker), you'd want self-hosted.

### 1.5 SearXNG
- **GitHub:** https://github.com/searxng/searxng
- **Stars:** 32K+ | **License:** AGPL-3.0
- **What it does:** Self-hosted metasearch engine that aggregates results from 262+ upstream engines (Google, Bing, Wikipedia, etc.) with zero tracking. One Docker command to deploy. Provides a JSON API for programmatic access.
- **Why it matters for Vitalik:** Privacy-respecting search that feeds into all your AI workflows. Use it as the search backend for n8n automations, CareerOS job discovery, and hawker market research. Replaces Google/Bing API costs with a free self-hosted alternative. The JSON API means your agents can search the web without leaking queries to search providers.
- **Integration effort:** Drop-in. 10-minute Docker setup. Point your tools at its API.
- **Concerns:** Upstream engines occasionally block SearXNG instances. Quality depends on which engines you enable. Needs occasional maintenance when engines change their APIs.

### 1.6 Mem0
- **GitHub:** https://github.com/mem0ai/mem0
- **Stars:** 59K+ | **License:** Apache-2.0
- **What it does:** Universal memory layer for AI agents. Provides multi-level memory (user preferences, session context, agent state) with semantic search, BM25 keyword matching, entity linking, and temporal reasoning. Supports 21 frameworks, 20 vector stores, self-hosted or local MCP.
- **Why it matters for Vitalik:** The missing persistence layer for all your AI interactions. With ADHD, context-switching between projects is expensive. Mem0 lets your agents remember what you were working on, your preferences, past decisions, and ongoing threads across Claude Code sessions, focuspipe, and CareerOS. The local MCP option means data stays on your machine.
- **Integration effort:** Weekend project for basic setup with Claude Code. Significant work to integrate deeply across all projects.
- **Concerns:** Self-hosted requires running a vector store (Qdrant, Chroma, etc.). The managed cloud option contradicts local-first philosophy but the self-hosted path works.

---

## 2. Tools Worth Watching

### 2.1 OpenClaw
- **GitHub:** https://github.com/microsoft/openclaw
- **Stars:** 215K+ | **License:** MIT
- **What it does:** Personal AI assistant that runs as a local gateway connecting AI models to 50+ integrations (WhatsApp, Telegram, Slack, Discord, Signal, iMessage via BlueBubbles). Autonomous skill creation, proactive automation, long-term memory. The breakout star of 2026 — went from 9K to 215K stars.
- **Why it matters for Vitalik:** The "one AI to rule them all" approach. Could become the unified interface for focuspipe alerts, hawker notifications, and CareerOS updates across all your messaging platforms. The autonomous skill creation is interesting for ADHD workflows — it learns what you need rather than requiring you to configure it.
- **Why wait:** Still maturing rapidly. The explosive growth means the API surface is unstable. Microsoft acquisition/involvement adds uncertainty about long-term direction. Resource-intensive for a local gateway. Watch for v2.0 stabilization.
- **Integration effort:** Significant — it wants to be the center of your stack, which means rearchitecting around it.

### 2.2 Stagehand
- **GitHub:** https://github.com/browserbase/stagehand
- **Stars:** 50K+ | **License:** MIT
- **What it does:** AI browser automation SDK built on Playwright. Four primitives (act, extract, observe, agent) that use natural language instead of CSS selectors. Scripts survive page redesigns without maintenance. TypeScript and Python SDKs.
- **Why it matters for Vitalik:** Hawker's marketplace scraping would be far more resilient with Stagehand. Instead of maintaining brittle selectors for eBay/Amazon/Facebook Marketplace, you'd write `act("click the next page button")` and it resolves at runtime. Also useful for CareerOS application automation.
- **Why wait:** v3 just stabilized (March 2026). The "self-healing" claims need real-world validation for high-volume scraping. For production automation, structured Playwright is still more reliable. Evaluate for hawker's prototype phase.
- **Integration effort:** Weekend project to prototype. Swapping out existing Playwright code requires testing.

### 2.3 Letta (formerly MemGPT)
- **GitHub:** https://github.com/letta-ai/letta
- **Stars:** 23K+ | **License:** Apache-2.0
- **What it does:** Stateful agents framework with a tiered memory system (core/archival/recall) that gives LLM agents effectively unlimited memory. Analogous to how an OS manages RAM and disk. Agents maintain context across interactions and self-improve over time.
- **Why it matters for Vitalik:** For focuspipe's long-term pattern recognition. An agent that remembers your ADHD patterns over weeks/months, not just single sessions. Could track which interventions actually helped, what time of day you're most focused, which distractions are most harmful. The tiered memory means it doesn't bloat context windows.
- **Why wait:** More complex to set up than Mem0. The framework is better suited for building custom agents from scratch rather than augmenting existing tools. Evaluate once focuspipe's core monitoring is stable.
- **Integration effort:** Significant — requires building agents within the Letta framework.

### 2.4 Open WebUI
- **GitHub:** https://github.com/open-webui/open-webui
- **Stars:** 140K+ | **License:** MIT
- **What it does:** Self-hosted ChatGPT-like interface for Ollama and OpenAI-compatible APIs. RAG, multimodal, multi-user, web search, voice/video calls, custom agents, code editor — all in one Docker container.
- **Why it matters for Vitalik:** If you start running local models via Ollama for privacy-sensitive tasks (financial data, personal notes, ADHD journaling), Open WebUI gives you a polished interface. The RAG pipeline could index your personal documents without them leaving your machine.
- **Why wait:** You're heavily invested in Claude Code as your primary interface. Adding Open WebUI makes sense only when you have specific local-model use cases that Claude Code doesn't cover (e.g., processing financial documents you don't want to send to Anthropic's API).
- **Integration effort:** Drop-in Docker container. The complexity is in deciding what to route to local models vs. Claude.

### 2.5 Composio
- **GitHub:** https://github.com/ComposioHQ/composio
- **Stars:** 28K+ | **License:** Open-source
- **What it does:** Tool integration platform for AI agents. 250+ pre-built integrations (GitHub, Notion, Linear, Gmail, Slack, YNAB) with managed auth (OAuth, API keys). MCP server support for Claude, Cursor. Claims 40% improvement in tool call accuracy.
- **Why it matters for Vitalik:** Could drastically reduce the integration work for hawker and CareerOS. Instead of building each API connection from scratch, Composio provides pre-built, authenticated connectors. The YNAB integration is especially interesting for your personal finance automation.
- **Why wait:** Evaluate whether the managed auth layer conflicts with your local-first philosophy. The value prop is strongest when you're connecting many services, which you are — but the dependency on their infrastructure is a trade-off.
- **Integration effort:** Weekend project for basic setup. Each new tool integration is ~hours instead of days.

### 2.6 Vibe-Trading
- **GitHub:** Trending July 2026
- **What it does:** Converts natural language prompts into backtests, alpha benchmarks, and optional live trades. 452 pre-built alpha factors and point-in-time data handling.
- **Why it matters for Vitalik:** If you ever move into algorithmic trading or want to analyze marketplace pricing trends for hawker. The natural language interface is ADHD-friendly.
- **Why wait:** Trading is a distraction risk for ADHD. Park this unless you have a specific financial analysis use case.

---

## 3. Building Blocks for Your Projects

### For focuspipe (ADHD monitoring)

| Repo | GitHub | What it provides | Integration path |
|------|--------|-----------------|------------------|
| **Screenpipe** | [screenpipe/screenpipe](https://github.com/screenpipe/screenpipe) | Screen/audio recording, OCR, app tracking — the raw attention data layer | Read its SQLite DB for app usage, window focus, browsing patterns |
| **Mem0** | [mem0ai/mem0](https://github.com/mem0ai/mem0) | Persistent memory for tracking ADHD patterns across sessions | Store focus scores, intervention effectiveness, user preferences |
| **Letta** | [letta-ai/letta](https://github.com/letta-ai/letta) | Stateful agents with tiered memory for long-term pattern recognition | Build a "focus coach" agent that remembers weeks of behavior |
| **Ollama** | [ollama/ollama](https://github.com/ollama/ollama) | Local LLM inference — process sensitive attention data without cloud | Run analysis on screen data locally; 172K+ stars, mature |
| **Chroma** | [chroma-core/chroma](https://github.com/chroma-core/chroma) | Local vector DB for semantic search over focus session data | Index and query past focus sessions by similarity |

### For hawker (marketplace bot)

| Repo | GitHub | What it provides | Integration path |
|------|--------|-----------------|------------------|
| **Firecrawl** | [mendableai/firecrawl](https://github.com/mendableai/firecrawl) | Web scraping to clean markdown/JSON, MCP server | Extract marketplace listings, pricing data, product descriptions |
| **Crawl4AI** | [unclecode/crawl4ai](https://github.com/unclecode/crawl4ai) | Open-source LLM-friendly web crawler, 68K stars | Alternative/complement to Firecrawl for bulk crawling |
| **Stagehand** | [browserbase/stagehand](https://github.com/browserbase/stagehand) | Self-healing browser automation via natural language | Resilient marketplace interaction without brittle selectors |
| **Browser Use** | [browser-use/browser-use](https://github.com/browser-use/browser-use) | Full AI browser agent framework, 97K stars | Complex marketplace workflows (listing, pricing, messaging) |
| **n8n** | [n8n-io/n8n](https://github.com/n8n-io/n8n) | Workflow orchestration with 1,400+ integrations | Chain scraping, analysis, listing, notification workflows |
| **SearXNG** | [searxng/searxng](https://github.com/searxng/searxng) | Private metasearch for market research | Price comparison, competitor analysis without tracking |

### For CareerOS (job search)

| Repo | GitHub | What it provides | Integration path |
|------|--------|-----------------|------------------|
| **career-ops** | [santifer/career-ops](https://github.com/santifer/career-ops) | Complete AI job search system for Claude Code | Fork or use directly — it IS the CareerOS concept, already built |
| **Resume-Matcher** | Available on GitHub (~27K stars) | Resume-to-JD matching, keyword gap analysis | Feed into career-ops or build your own scoring |
| **Firecrawl** | [mendableai/firecrawl](https://github.com/mendableai/firecrawl) | Clean extraction from job portals | Structured job listing data for scoring pipeline |
| **Stagehand** | [browserbase/stagehand](https://github.com/browserbase/stagehand) | Natural language browser automation | Application form filling, portal navigation |
| **Mem0** | [mem0ai/mem0](https://github.com/mem0ai/mem0) | Persistent memory across job search sessions | Remember application history, interview feedback, preferences |

---

## 4. Grant Inspiration

These repos suggest adjacent problem spaces where grants (Mozilla, NLNet, Sovereign Tech Fund, EU NGI) could fund novel work:

### 4.1 ADHD-Aware AI Agent Framework
- **Inspired by:** Screenpipe + Letta + focuspipe
- **Concept:** An open-source framework specifically for building AI agents that adapt to neurodivergent cognitive patterns. Not just "focus timers" but agents that understand executive function, context-switching costs, hyperfocus states, and task initiation difficulty. Use Screenpipe's data layer, Letta's memory architecture, and build intervention strategies on top.
- **Grant angle:** Digital accessibility, mental health technology, EU disability inclusion initiatives.

### 4.2 Privacy-First Personal AI Gateway
- **Inspired by:** OpenClaw + SearXNG + Ollama
- **Concept:** A local-first personal AI gateway that routes between local and cloud models based on data sensitivity. Financial data goes to Ollama, coding questions go to Claude, web searches go through SearXNG. Automatic PII detection and routing. MCP-compatible.
- **Grant angle:** Digital sovereignty, GDPR compliance tooling, Sovereign Tech Fund.

### 4.3 Open-Source Career Intelligence Platform
- **Inspired by:** career-ops + Resume-Matcher + Mem0
- **Concept:** A comprehensive, local-first career management system that goes beyond job search: tracks skills development, suggests learning paths, maintains a "career memory" across years, analyzes market trends for your skillset. The anti-LinkedIn — your career data stays on your machine.
- **Grant angle:** Digital public infrastructure, workforce development, EU digital skills initiatives.

### 4.4 Marketplace Fairness Monitor
- **Inspired by:** hawker + Firecrawl + n8n
- **Concept:** An open-source tool that monitors online marketplaces for pricing manipulation, fake reviews, counterfeits, and unfair seller practices. Combines web scraping with LLM analysis. Could serve consumer protection organizations.
- **Grant angle:** Consumer protection, digital market regulation, competition policy.

### 4.5 Passive Productivity Intelligence
- **Inspired by:** Screenpipe + Mem0 + focuspipe
- **Concept:** An AI system that passively learns your work patterns and provides insights without requiring any manual input. No timers to start, no categories to set up. Just works in the background and tells you "You were 40% more productive on Tuesdays when you started with email" or "Context-switching cost you 2.3 hours this week."
- **Grant angle:** Workplace wellbeing, occupational health, human-computer interaction research.

---

## 5. Skip List

| Repo | Stars | Why Skip |
|------|-------|----------|
| **AutoGPT** | 183K | Evolved into a visual agent builder platform — overlaps with n8n/Dify but with a messy license (Polyform Shield for platform, MIT for other parts). Too enterprise-focused for solo dev. |
| **Dify** | 138K+ | Powerful LLM app platform but overkill for solo dev. You'd be deploying and maintaining a full platform when Claude Code + n8n covers your needs. Better for teams of 5+. |
| **Langflow** | 100K+ | Visual AI workflow builder — now IBM-owned after DataStax acquisition. Overlaps with n8n which is more general-purpose and has a larger integration ecosystem. Pick one; n8n wins for your use case. |
| **LangChain/LangGraph** | 34K+ | Enterprise agent framework. You're building with Claude Code directly — adding LangChain adds a massive dependency layer with minimal benefit for a solo developer. LangGraph's stateful agent abstraction is interesting but Letta does it better for your use case. |
| **MetaGPT** | Large | Multi-agent framework for simulating software teams. You ARE the team. This solves a problem you don't have. |
| **CrewAI** | Growing | Multi-agent orchestration. Same issue as MetaGPT — designed for coordinating multiple agents in complex org structures. Overkill and adds complexity for solo workflows. |
| **ComfyUI** | 106K | Node-based image generation workflow. Unless you're doing AI art or product image generation for hawker, this is a fun distraction — which for ADHD is a risk. |
| **LobeChat** | 70K+ | Beautiful AI chat interface, but you're already in Claude Code's terminal. Adding another chat UI fragments your workflow. Only relevant if you switch to local models full-time. |
| **Flowise** | 51K | No-code LLM agent builder — overlaps with n8n + Dify. n8n covers more ground with its 1,400+ integrations. Flowise is better for teams who want a pure AI-focused builder. |
| **PrivateGPT** | Large | Local document QA. Interesting concept but you can achieve the same with Ollama + Chroma + a simple script, or with Open WebUI's built-in RAG. Dedicated product for a narrow use case. |
| **Ollama** (as primary interface) | 172K | You should have Ollama installed for local inference, but don't try to replace Claude Code with it. The models that run on M-series chips are good for privacy-sensitive processing but not competitive with Claude for coding tasks. Use it as a backend, not a frontend. |

---

## Summary: Priority Stack

**This week:**
1. Install Screenpipe CLI and explore its SQLite schema for focuspipe integration
2. Set up career-ops in your Claude Code — it may replace or accelerate CareerOS
3. Add Firecrawl MCP server to your Claude Code configuration

**This month:**
4. Deploy n8n (Docker) and build your first automation (YNAB webhook -> AI analysis)
5. Set up SearXNG for private search across your tools
6. Evaluate Mem0 for cross-session memory in your Claude Code workflows

**This quarter:**
7. Prototype hawker scraping with Stagehand instead of raw Playwright
8. Evaluate OpenClaw once it stabilizes
9. Write grant proposals based on the ideas in Section 4

---

*Research conducted July 2026. Star counts and features are current as of this date. The open-source AI landscape moves fast — re-evaluate quarterly.*

*Note: goodailist.com/repos returned 403 on automated fetch. This analysis was compiled by cross-referencing goodailist's Twitter feed, OSSInsight trending data, ByteBytego's AI repos analysis, and direct GitHub research across 60+ repositories.*
