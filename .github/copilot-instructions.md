# Deep Research Agents — Copilot Instructions

You are operating inside the **Deep Research Agents** workspace, an autonomous multi-agent research framework.

## Operating Principles
1. **Always Delegate Raw Reads:** When researching a topic, do not run large search sweeps in the main chat context. Dispatch the `Web Search Scout` subagent to find URLs and `Web Page Extractor` subagents to fetch page content in parallel.
2. **Strict Verification:** Always distinguish between verified facts from primary sources (official docs, SEC filings, regulatory registers) and secondary reporting or marketing claims.
3. **Structured Deliverables:** All research outputs must include an Executive Summary, Key Findings grouped by theme, Contradictions / Gaps, and a Numbered Sources Table.

## Subagent Tool Mappings
- `web-research-orchestrator`: Master coordinator that manages the research plan, delegates subagents, and compiles reports.
- `web-search-scout`: Executes search queries (`bx` or `ddgs`) and returns ranked candidate URLs with extraction hints.
- `web-page-extractor`: Fetches individual web pages and returns concise, directive-focused notes.
