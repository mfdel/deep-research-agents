# Nordic AI Scout

A multi-agent web research pipeline built on GitHub Copilot Agent Mode for systematically mapping AI-native companies in the Nordic region.

---

## Web Research Agent Structure

A standalone multi-agent system that can be pointed at any research topic. Use it for competitive intelligence, market mapping, due diligence, ecosystem analysis, or any task that requires synthesising information from many web sources simultaneously.

Three agents, each with a single responsibility:

```
Web Research Orchestrator
    │
    ├──► Web Search Scout       — runs bx (Brave Search) queries, ranks candidate URLs
    │
    └──► Web Page Extractor ×N  — fetches pages in parallel, extracts relevant content
```

The orchestrator delegates all searching and fetching — it only coordinates and synthesises. Extractors run in parallel across dozens of pages simultaneously, making it significantly faster than any sequential approach.

---

## Prompt

**`nordic-ai-company-discovery`** — Discovers AI-native companies in the Nordics that align with an individual's professional background and domain expertise. Load your CV and career summary, describe your target criteria, and the pipeline returns a tiered company list with a fit rationale per company.

---

## Setup

- GitHub Copilot with Agent Mode enabled
- `bx` CLI ([Brave Search API](https://api.search.brave.com))

Load your CV and career summary before invoking the prompt.
