# Deep Research Agents — Operating Guide

This repository contains a multi-agent framework for conducting autonomous, high-depth web research on complex technical, business, and regulatory topics.

---

## System Architecture

The research pipeline uses a **3-tier hierarchical agent structure** designed to maximize source coverage while strictly protecting the orchestrator's context window:

```
Web Research Orchestrator
    │
    ├──► Web Search Scout       — generates adaptive search queries (via Brave Search `bx` or DuckDuckGo `ddgs`)
    │
    └──► Web Page Extractor ×N  — parallel workers fetching and extracting only relevant facts per URL
```

### Core Design Rules
1. **Context Isolation:** The Orchestrator *never* reads raw search dumps or full HTML pages. Subagents ingest raw data and return dense, structured summaries.
2. **Parallel Extraction Fan-Out:** Page extractors run concurrently across 5–30 candidate URLs simultaneously.
3. **Evidence-Based Citations:** Every claim in synthesized reports must carry a direct source citation. No uncited assertions or hallucinations.
4. **Moat / ChatGPT Test:** For competitive and market analyses, always evaluate whether the insight requires a proprietary dataset/workflow or is general LLM knowledge.

---

## Directory Structure

```
├── CLAUDE.md                   # This operating guide
├── README.md                   # Public repository documentation
├── pyproject.toml              # Modern Python 3.12 config via uv
├── .github/
│   ├── copilot-instructions.md # GitHub Copilot Agent Mode guidelines
│   ├── agents/                 # Specialized subagent persona definitions
│   │   ├── web-research-orchestrator.agent.md
│   │   ├── web-search-scout.agent.md
│   │   └── web-page-extractor.agent.md
│   └── prompts/                # Ready-to-run research briefs
├── prompts/                    # Mirrored prompt library for CLI / IDE access
│   ├── company-deep-dive.prompt.md
│   ├── tool-stack-comparison.prompt.md
│   ├── product-user-sentiment-teardown.prompt.md
│   ├── regulatory-market-scan.prompt.md
│   └── nordic-ai-company-discovery.prompt.md
├── examples/                   # Full-length, publication-grade research outputs
│   ├── research_electromagnetic_fields_emf_and_health_20260720.md
│   ├── research_nordic_capital_fund_activity_and_ai_20260721.md
│   ├── research_redpine_ai_20260820.md
│   └── research_etsy_traffic_tips_20260801.md
└── research/                   # Local output folder for new research runs
```

---

## Tooling & Execution

- **Environment:** Python 3.12 managed via `uv`
- **CLI Commands:**
  - Setup: `uv venv && source .venv/bin/activate && uv pip install -e .`
  - Search Discovery: `bx web "<query>"` or `ddgs text -q "<query>"`
- **Research Artifacts:** Always save final reports to `research/research_[topic_slug]_[YYYYMMDD].md`.
