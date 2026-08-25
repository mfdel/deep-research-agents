# Deep Research Agents 🕵️‍♂️⚡

> **Autonomous multi-agent deep research framework for systematic web exploration, parallel extraction, and analytical synthesis across complex domains.**

---

## Overview

General LLMs suffer from severe context bloat and hallucination when asked to research broad or technical topics directly. **Deep Research Agents** solves this through a **hierarchical multi-agent architecture** where a master **Web Research Orchestrator** manages 3 distinct execution phases across specialized subagents.

By decoupling broad URL discovery from deep-page extraction, the framework gathers evidence across dozens of disparate sources in seconds while preserving pristine context for rigorous analytical synthesis.

---

## System Architecture

```mermaid
flowchart TD
    User(["User / Research Prompt"]) --> Orchestrator["Web Research Orchestrator
(Plans decomposition · Coordinates execution · Synthesizes final report)"]

    subgraph Phase1 ["Phase 1: Discovery & URL Ranking"]
        direction TB
        Scout["Web Search Scout Subagent"]
        S1["Theme Queries (bx / ddgs)"]
        S2["Adaptive Gap-Fill Searches"]
        URLs["Ranked Candidate URLs + Extraction Directives"]
        Scout --> S1 & S2 --> URLs
    end

    subgraph Phase2 ["Phase 2: Parallel Page Extraction"]
        direction TB
        E1["Page Extractor 1 (Theme A URLs)"]
        E2["Page Extractor 2 (Theme B URLs)"]
        EN["Page Extractor N (Theme N URLs)"]
        Summaries["Topic-Filtered Content Summaries
(Raw HTML dropped · Context protected)"]
        E1 & E2 & EN --> Summaries
    end

    subgraph Phase3 ["Phase 3: Synthesis & Verification"]
        direction TB
        Dedup["Cross-Source Fact Verification & Gap Analysis"]
        Report["Structured Research Report
(Executive Summary · Thematic Deep Dives · Numbered Citations)"]
        Dedup --> Report
    end

    Orchestrator ==>|1. Dispatches search themes| Phase1
    Phase1 ==>|Returns ranked URLs| Orchestrator
    Orchestrator ==>|2. Fans out extraction directives| Phase2
    Phase2 ==>|Returns condensed summaries| Orchestrator
    Orchestrator ==>|3. Synthesizes all gathered evidence| Phase3
    Phase3 --> FinalOutput(["Saved to research/research_[slug]_[date].md"])
```

---

## The 3-Phase Execution Model

### Phase 1 — Discovery & URL Ranking (Web Search Scout)
* The Orchestrator decomposes the research question into search themes and dispatches the **Web Search Scout**.
* The Scout generates adaptive search queries across search backends (Brave Search `bx` or DuckDuckGo `ddgs`).
* Evaluates coverage, fills topical gaps, deduplicates sources, and returns a prioritized candidate URL table with specific extraction hints.

### Phase 2 — Parallel Page Extraction (Web Page Extractors)
* The Orchestrator reviews the candidates, writes precise **extraction directives**, and fans out to $N$ parallel **Web Page Extractor** subagents.
* Each extractor visits 2–3 assigned URLs, strips boilerplate/navigation/HTML clutter, and extracts only the evidence, quotes, and metrics strictly relevant to its assigned directive.
* Returns dense, structured factual snippets to the Orchestrator without bloating the parent context.

### Phase 3 — Synthesis & Verification (Web Research Orchestrator)
* The Orchestrator collects all returned evidence, cross-checks claims, resolves contradictions across sources, and identifies remaining data gaps.
* Compiles the comprehensive, publication-grade markdown report with an Executive Summary, thematic sections, uncertainty/risk analysis, and a complete numbered citation table.

---

## Key Capabilities & Why It Works

| Feature | Why It Matters |
|---|---|
| **Zero Context Bloat** | The orchestrator never reads raw HTML or 100-line search dumps. Subagents condense pages into dense factual snippets before returning. |
| **Parallel Extraction Fan-Out** | Ingests 10–30 web pages simultaneously across subagent workers rather than crawling sequentially. |
| **Strict Citation Graph** | Every claim in the final report is anchored to a specific source URL and publisher. |
| **Dual Search Backends** | Native support for **Brave Search (`bx`)** and **DuckDuckGo (`ddgs`)** with zero mandatory paid API keys. |
| **Cross-Platform** | Optimized for GitHub Copilot Agent Mode and Claude Code CLI. |

---

## Prompt Library (`prompts/`)

The repository includes pre-built research playbooks ready to run:

1. **`company-deep-dive.prompt.md`** — 360° corporate audit (business model, architecture, funding, executive pedigree, hiring velocity, moat risks).
2. **`tool-stack-comparison.prompt.md`** — Multi-tool benchmark evaluating latency, developer ergonomics, pricing tiers, and production failure modes.
3. **`product-user-sentiment-teardown.prompt.md`** — Voice-of-customer scraping across G2, Capterra, Reddit, and Trustpilot to uncover churn triggers.
4. **`regulatory-market-scan.prompt.md`** — Scans legal and regulatory mandates (e.g. EU AI Act, CMS healthcare rules) to pinpoint software market opportunities.
5. **`nordic-ai-company-discovery.prompt.md`** — Systematically maps AI-native startups across Nordic hubs matching specific talent or investment criteria.

---

## Real Sample Research Reports (`examples/`)

Inspect full-length, unedited reports produced by the system:

- 🔬 [`research_electromagnetic_fields_emf_and_health_20260720.md`](examples/research_electromagnetic_fields_emf_and_health_20260720.md) — Comprehensive scientific literature review and consensus teardown on EMF exposure and public health.
- 💼 [`research_nordic_capital_fund_activity_and_ai_20260721.md`](examples/research_nordic_capital_fund_activity_and_ai_20260721.md) — Private equity portfolio analysis and AI leadership capability mapping.
- 🎯 [`research_redpine_ai_20260820.md`](examples/research_redpine_ai_20260820.md) — Company intelligence and competitive positioning deep dive.
- 📈 [`research_etsy_traffic_tips_20260801.md`](examples/research_etsy_traffic_tips_20260801.md) — E-commerce marketplace SEO algorithms and organic search velocity mechanics.

---

## Quickstart

### 1. Installation

```bash
# Clone the repository
git clone https://github.com/mfdel/deep-research-agents.git
cd deep-research-agents

# Set up virtual environment with uv (Python 3.12)
uv venv --python 3.12
source .venv/bin/activate
uv pip install -e .
```

### 2. Running in GitHub Copilot or Claude Code

**In GitHub Copilot Agent Mode:**
1. Open any prompt file from `.github/prompts/` (e.g. `company-deep-dive.prompt.md`).
2. Fill in the target company or question.
3. Trigger `@agent Web Research Orchestrator`.

**In Claude Code:**
```bash
claude -p "Research the production failure modes and memory bottlenecks of pgvector vs Qdrant in multi-tenant RAG systems. Save the report in research/"
```

---

## License

MIT License. Built and maintained by [M. Fuat Deligoz](https://github.com/mfdel).
