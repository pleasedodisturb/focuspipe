# Top Open-Source AI Repos: Analysis for Vitalik

**Date:** 2026-10-26
**Source:** Curated from goodailist.com/repos rankings + cross-referenced with GitHub trending, awesome-ai-agents lists, and 2026 roundups.

> goodailist.com was unreachable at research time (DNS failure). This analysis reconstructs the top-100 list from cached references (Chip Huyen's blog, bobpage0451/ai-trending-repos which mirrors the goodailist taxonomy, and multiple 2026 roundup articles).

---

## 1. Tools to Integrate NOW

These would immediately improve your daily workflow. Prioritized by effort-to-value ratio.

### 1.1 Crawl4AI
- **GitHub:** https://github.com/unclecode/crawl4ai (~85k stars)
- **What:** Async Python web crawler that turns pages into LLM-ready markdown/structured output. Uses Playwright under the hood. Apache-2.0.
- **Why for you:** Direct building block for **hawker** (marketplace scraping). It handles anti-bot detection with proxy escalation (v0.8.x+), outputs structured JSON, and feeds directly into LLM pipelines. Replaces custom scraping logic you'd otherwise write from scratch.
- **Integration effort:** Weekend project. `pip install crawl4ai`, write extraction schemas, plug into hawker's pipeline.
- **Concerns:** Heavy dependency (bundles Playwright). For marketplace-specific anti-detection you may still need Patchright on top (see below).

### 1.2 Browser Use
- **GitHub:** https://github.com/browser-use/browser-use (~118k stars)
- **What:** Python library for AI-driven browser automation. The agent sees the page, reasons about it, and acts. MIT license.
- **Why for you:** Complements Crawl4AI for interactive flows in **hawker** -- listing items, responding to messages, navigating marketplace dashboards. Also useful for **CareerOS** job application automation (filling forms, submitting applications).
- **Integration effort:** Weekend project. Works with Claude/OpenAI APIs. Pairs naturally with your existing Playwright knowledge.
- **Concerns:** Token-heavy (sends screenshots to LLM). Cost adds up with frequent runs. Best for tasks that genuinely need visual reasoning.

### 1.3 Patchright (Stealth Playwright)
- **GitHub:** https://github.com/Kaliiiiiiiiii-Vinyzu/patchright
- **What:** Patched Playwright that removes automation detection flags. Drop-in replacement (`pip install patchright`). Removes `--enable-automation`, patches `Runtime.enable` to avoid CDP detection, handles closed Shadow DOMs.
- **Why for you:** **Hawker** needs to interact with marketplaces that actively detect bots (eBay, Amazon, Facebook Marketplace). Stock Playwright gets flagged; Patchright doesn't change your code, just the browser launch.
- **Integration effort:** Drop-in. Replace `from playwright` with `from patchright` in imports.
- **Concerns:** Breaks console debugging. May lag behind Playwright updates by a few days. Effectiveness against specific anti-bot systems (Cloudflare, DataDome) varies.

### 1.4 FastMCP
- **GitHub:** https://github.com/PrefectHQ/fastmcp (~28k stars)
- **What:** Python framework for building MCP servers with decorated functions. Auto-generates schemas and validation. Apache-2.0.
- **Why for you:** You're already deep in the MCP ecosystem with Claude Code. FastMCP lets you rapidly build custom MCP servers for your own tools -- expose focuspipe data to Claude, create hawker MCP tools, build CareerOS query interfaces. Function decorator pattern means minimal boilerplate.
- **Integration effort:** Drop-in for new MCP servers. `uv pip install fastmcp`, decorate functions, `mcp.run()`.
- **Concerns:** v4.0.0 alpha in progress; stick with v3.x stable. Import paths changed from when it was part of the official MCP SDK.

### 1.5 Mem0
- **GitHub:** https://github.com/mem0ai/mem0 (~67k stars)
- **What:** Memory layer for AI agents. Stores user preferences, context, and conversation history across sessions. Self-hostable via Docker or as a library. Apache-2.0.
- **Why for you:** Cross-session memory for **focuspipe** -- remember your ADHD patterns, preferred work blocks, historical focus data. Also useful as a shared memory layer across your projects so Claude-based agents in hawker/CareerOS remember context.
- **Integration effort:** Weekend project for self-hosted setup. Python SDK or MCP plugin available.
- **Concerns:** Self-hosted requires a vector DB (Qdrant/Chroma). The hosted API is easier but sends data to their servers (conflicts with local-first philosophy).

### 1.6 Goose
- **GitHub:** https://github.com/aaif-goose/goose (~51k stars)
- **What:** Open-source AI agent from Block, now under Linux Foundation's AAIF. MCP-native architecture, desktop app + CLI + API. Rust-based. Apache-2.0.
- **Why for you:** You're already evaluating Goose for focuspipe's MCP extension. It's the reference MCP implementation with 70+ extensions. Useful as a general-purpose agent that can orchestrate your MCP tools across projects. Runs on macOS natively.
- **Integration effort:** Already partially integrated with focuspipe. Deepening integration is a weekend project.
- **Concerns:** Brings its own runtime model; you're primarily a Claude Code user. Useful as a complementary agent, not a replacement.

### 1.7 n8n
- **GitHub:** https://github.com/n8n-io/n8n (~50k+ stars)
- **What:** Self-hosted workflow automation with visual canvas + code. 400+ native integrations, AI agent nodes. Fair-code license (free self-hosted).
- **Why for you:** Orchestration layer for your automation empire. Connect YNAB webhooks to budget alerts, automate hawker listing workflows, chain CareerOS job monitoring with notifications. The AI Agent node can call Claude to make decisions in workflows. Replaces custom cron jobs and glue scripts.
- **Integration effort:** Significant (1-2 weeks to set up properly). Docker self-hosted. Learning curve for the visual editor, but pays back fast for ADHD -- visual workflows are easier to maintain than scattered scripts.
- **Concerns:** Resource-heavy (needs a server/VPS). Self-hosting means you own updates and backups. The fair-code license restricts redistribution but not personal use.

---

## 2. Tools Worth Watching

Not ready for immediate integration, but track these for your use cases.

### 2.1 OpenCode
- **GitHub:** https://github.com/anomalyco/opencode (~213k stars)
- **What:** Terminal-first AI coding agent with LSP integration, MCP support, TUI interface. MIT license. Works with local models via Ollama.
- **Why watch:** If you ever want a Claude Code alternative that works with local models for privacy-sensitive work. Currently you're deep in Claude Code's ecosystem, but OpenCode's MCP support means your MCP servers would work with it too.
- **When:** If Claude Code pricing becomes a concern, or you need fully offline coding.

### 2.2 OpenClaw
- **GitHub:** https://github.com/openclaw/openclaw (~392k stars, most-starred repo on GitHub)
- **What:** Self-hosted AI agent that connects through messaging apps (Telegram, Signal, Slack). Apache-2.0.
- **Why watch:** Could serve as the notification/interaction layer for focuspipe -- imagine getting ADHD nudges via Telegram from your own self-hosted agent that has access to your screen activity data.
- **When:** Once focuspipe's core monitoring is stable and you want a conversational interface.

### 2.3 Maybe (Personal Finance)
- **GitHub:** https://github.com/maybe-finance/maybe (~54k stars)
- **What:** Open-source personal finance tracker. Consolidates accounts, investments, assets into a net worth dashboard. AI-driven recommendations. AGPL-3.0.
- **Why watch:** You use YNAB. Maybe is the strongest open-source alternative with AI features. If YNAB's pricing or privacy posture changes, or if you want to build custom finance automation (AI-powered budget categorization, spending pattern detection for ADHD impulse tracking).
- **When:** If you outgrow YNAB or want to integrate finance data into your AI workflows.

### 2.4 Khoj
- **GitHub:** https://github.com/khoj-ai/khoj
- **What:** Self-hosted "second brain" for research and document Q&A. AGPL-3.0. Indexes your notes, documents, and conversations.
- **Why watch:** Could index your project docs, research notes, and conversations across focuspipe/hawker/CareerOS. Natural complement to Mem0 but more document-focused.
- **When:** When you have enough accumulated knowledge across projects that search becomes a bottleneck.

### 2.5 Hermes Agent (NousResearch)
- **GitHub:** https://github.com/NousResearch/hermes-agent (~254k stars)
- **What:** High-performance open-source AI agent. Exploded in popularity in 2026.
- **Why watch:** If it supports MCP and local deployment, could be a powerful agent backbone for autonomous tasks in hawker and CareerOS.
- **When:** Once the ecosystem matures and MCP integration is confirmed.

### 2.6 Composio
- **GitHub:** https://github.com/ComposioHQ/composio (~30k stars)
- **What:** Platform connecting AI agents to 250+ apps. Managed OAuth, tool execution, unified interface for GitHub/Slack/etc.
- **Why watch:** Could dramatically simplify the integration work for CareerOS (job boards, LinkedIn, email) and hawker (marketplace APIs). But it's a hosted service -- check if the self-hosted option exists and is viable.
- **When:** When your integration count exceeds what's practical to maintain with custom MCP servers.

### 2.7 LangGraph
- **GitHub:** https://github.com/langchain-ai/langgraph (~43k stars)
- **What:** Framework for building stateful, multi-actor agent applications as graphs. From the LangChain team.
- **Why watch:** If your agent workflows become complex enough to need explicit state machines (e.g., hawker's listing-monitor-respond-relist cycle, or CareerOS's application-track-followup pipeline).
- **When:** When you hit the limits of linear agent flows.

---

## 3. Building Blocks for Your Projects

### For focuspipe (ADHD monitoring)

| Repo | What it provides | Integration |
|------|-----------------|-------------|
| **Screenpipe** (screenpi.pe) | Core screen/audio recording engine. Rust, local-first, OCR + Whisper. Plugin system (Pipes). | Already evaluating. focuspipe IS a Screenpipe pipe. |
| **Mem0** (mem0ai/mem0) | Persistent memory of focus patterns, user preferences, historical ADHD data across sessions. | Weekend project. Self-host with Qdrant. |
| **Goose** (aaif-goose/goose) | MCP-native agent for conversational interaction with focus data. | Already partially integrated. |
| **FastMCP** (PrefectHQ/fastmcp) | Build custom MCP tools exposing focuspipe's analysis to any MCP client. | Drop-in. Python decorators. |
| **Evidently** (evidentlyai/evidently) | ML monitoring -- track drift in your ADHD pattern models over time. | Significant. Useful when focuspipe has enough data. |

### For hawker (marketplace bot)

| Repo | What it provides | Integration |
|------|-----------------|-------------|
| **Crawl4AI** (unclecode/crawl4ai) | LLM-ready marketplace scraping with anti-bot handling. | Weekend project. Core scraping engine. |
| **Patchright** (patchright) | Stealth browser automation for marketplaces that detect bots. | Drop-in replacement for Playwright. |
| **Browser Use** (browser-use) | AI-driven interactive automation (listing items, messaging buyers). | Weekend project. For flows needing visual reasoning. |
| **Scrapling** (D4Vinci/Scrapling) | Alternative scraping library (~87k stars). Smart element selection. | Weekend project. Evaluate vs Crawl4AI. |
| **n8n** (n8n-io/n8n) | Orchestrate listing cycles, price monitoring, notification workflows. | Significant. Self-hosted Docker. |

### For CareerOS (job search)

| Repo | What it provides | Integration |
|------|-----------------|-------------|
| **Browser Use** (browser-use) | Automate job application form filling, LinkedIn interactions. | Weekend project. |
| **Crawl4AI** (unclecode/crawl4ai) | Scrape job boards, extract structured job data for matching. | Weekend project. |
| **Mem0** (mem0ai/mem0) | Remember application history, interview prep context, company research. | Weekend project. |
| **stitchcv** | Score resumes against job descriptions, generate tailored PDFs. AGPL-3.0. | Weekend project. Python tool. |
| **promptfoo** (promptfoo/promptfoo) | Test and evaluate your AI matching/scoring prompts systematically. | Weekend project. CLI tool. |

---

## 4. Grant Inspiration

These repos suggest adjacent problem spaces where grants (e.g., Mozilla, Sovereign Tech Fund, EU NGI) would be relevant.

### 4.1 AI-Powered Accessibility for Neurodivergent Users
- **Inspiration from:** focuspipe, ADHD Focus Mate, Screenpipe
- **Grant idea:** A local-first, privacy-preserving ADHD executive function assistant that uses screen activity + calendar + task data to provide real-time attention coaching. Unlike commercial solutions, fully self-hosted and open-source.
- **Target grants:** Mozilla Builders, Prototype Fund (Germany-based, perfect for Frankfurt), EU NGI

### 4.2 Open-Source Personal Data Sovereignty Platform
- **Inspiration from:** Screenpipe, Jan, Mem0, Syncthing
- **Grant idea:** A unified local-first platform that gives individuals control over their digital footprint -- screen history, conversations, finances, health data -- with AI analysis that never leaves the device. Essentially "personal AI that owns nothing about you."
- **Target grants:** Sovereign Tech Fund, NLnet Foundation, EU NGI TALER

### 4.3 Anti-Dark-Pattern Marketplace Agent
- **Inspiration from:** hawker, Browser Use, Crawl4AI
- **Grant idea:** An open-source agent that monitors marketplace listings for dark patterns, price manipulation, and scam indicators. Protects both buyers and sellers. Could integrate with consumer protection APIs.
- **Target grants:** Mozilla Technology Fund, EU Digital Services Act related funding

### 4.4 Open MCP Ecosystem for Personal Productivity
- **Inspiration from:** FastMCP, Goose, Composio
- **Grant idea:** A curated, self-hostable collection of MCP servers for personal productivity -- calendar, tasks, finances, health, notes -- with a unified privacy-preserving data layer. "MCP for Humans, not just Developers."
- **Target grants:** Prototype Fund, GitHub Sponsors Fund, NLnet

### 4.5 Neurodivergent-First Job Search Platform
- **Inspiration from:** CareerOS, Browser Use, stitchcv
- **Grant idea:** An AI-powered job search tool designed specifically for neurodivergent users. Automates the executive-function-heavy parts (tracking applications, following up, tailoring resumes) while providing ADHD-friendly interfaces (minimal decisions, automated reminders, momentum-preserving UX).
- **Target grants:** Mozilla Builders, Ashoka Foundation, EU Social Innovation Competition

---

## 5. Skip List

These are popular on the goodailist rankings but NOT relevant to your setup.

| Repo | Stars | Why Skip |
|------|-------|----------|
| **huggingface/transformers** | 168k | You're using API-based models (Claude), not training or fine-tuning. |
| **unslothai/unsloth** | 78k | Fine-tuning framework. Same reason -- you don't train models. |
| **vllm-project/vllm** | 94k | GPU inference server. You use Claude API, not self-hosted LLMs at scale. |
| **hpcaitech/ColossalAI** | 41k | Distributed training. Not your domain. |
| **deepspeedai/DeepSpeed** | 43k | Same -- distributed training infrastructure. |
| **ggml-org/llama.cpp** | 131k | Low-level inference engine. Ollama wraps this; use Ollama if you need local models. |
| **sgl-project/sglang** | 37k | Serving framework for large model deployment. Overkill for personal use. |
| **PaddlePaddle/PaddleOCR** | 91k | Industrial OCR. Screenpipe already handles your OCR needs. |
| **hiyouga/LlamaFactory** | 76k | Fine-tuning toolkit. Not relevant unless you start training models. |
| **ray-project/ray** | 44k | Distributed computing framework. Way too heavy for solo dev. |
| **shadcn-ui** | 126k | UI component library. Useful if building web UIs, but not a priority for your CLI/agent-first workflow. |
| **streamlit/gradio** | 46k/44k | Demo/prototyping UIs. You're building production tools, not demos. |
| **mlflow** | 28k | ML experiment tracking. You're not doing ML experiments. |
| **NVIDIA/garak** | 10k | LLM security testing. Niche unless you're red-teaming models. |
| **OpenBMB/ChatDev** | 35k | Multi-agent software dev simulation. Novelty project, not practical for solo dev. |
| **lm-sys/FastChat** | 40k | Chat model serving. Use Claude API instead. |
| **google-gemini/gemini-cli** | 107k | Gemini-specific CLI. You're in the Claude ecosystem. |
| **openai/codex** | 129k | OpenAI's coding agent. You have Claude Code. |

---

## Summary: Priority Integration Roadmap

### This week (drop-in)
1. **Patchright** -- swap into hawker's Playwright usage
2. **FastMCP** -- start building custom MCP servers for your projects

### This month (weekend projects)
3. **Crawl4AI** -- hawker's scraping engine
4. **Browser Use** -- interactive automation for hawker + CareerOS
5. **Mem0** (self-hosted) -- cross-project memory layer

### Next quarter (significant effort)
6. **n8n** (self-hosted) -- workflow orchestration hub
7. Deepen **Goose** + **Screenpipe** integration in focuspipe

### Watch list (check quarterly)
- OpenCode, OpenClaw, Maybe, Khoj, Composio, LangGraph, Hermes Agent

---

*Research compiled from: goodailist.com (via cached references), bobpage0451/ai-trending-repos, Chip Huyen's AI OSS analysis, 2026 roundup articles from pinggy.io, openalternative.co, vellum.ai, dev.to, bitdoze.com, and others. Star counts are approximate and vary by source date.*
