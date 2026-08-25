---
name: web-search
description: USE FOR choosing and using a web search tool for AI agents. Compares and decides between bx (Brave Search CLI), ddgs (DuckDuckGo metasearch, free/no-quota). Covers quotas, filters, site: scoping, full-page content extraction, and the optimal decision strategy for which engine to use. Triggered by "web search", "which search tool", "search quota exhausted", "agent web search", "search with filters/site:", "read a page for an LLM".
---

# Web Search Tools — Router & Decision Guide

This skill helps you **pick the right web-search tool for the job** and then points to a
tool-specific reference file for usage. There are three engines available, each with a
different sweet spot. Read this page first, choose, then open the linked how-to.

| Tool | Reference | One-line role |
|------|-----------|---------------|
| **bx** (Brave Search CLI) | [bx.md](bx.md) + [bx-query-strategies.md](bx-query-strategies.md) | Polished, token-budgeted RAG-ready results. **Metered: ~1000 searches/month, resets monthly.** |
| **ddgs** (DuckDuckGo metasearch) | [ddgs.md](ddgs.md) | **Free, no quota, no key.** Best default for *discovery* (finding URLs). Snippets only. |

> A self-hosted **SearXNG** instance is a "v2" upgrade for unlimited, aggregated, JSON-API
> search — see the note at the bottom.

---

## The core insight: search is two different jobs

Most agent workflows conflate two jobs that need different tools:

1. **Discovery** — find the relevant URLs / threads for a query (with `site:`, freshness, region filters).
2. **Content extraction** — pull the *full* page/thread text (post + comments) for analysis/RAG.

`ddgs` and `bx` do discovery. Only `bx` (via its cached index) 
deliver rich content. Many sites (e.g. **Reddit**) block naked scrapers, so free `.json`
endpoints and anonymous readers fail 

---

## Quick decision

```
Need to FIND pages (URLs, discovery)?
├─ Have bx quota left AND want token-budgeted, ready-to-use snippets? ......... bx
└─ Out of quota / want zero-cost, unlimited discovery? ....................... ddgs (backend="auto")

Need the FULL content of a specific page for an LLM?
├─ Public, scraper-friendly site? ........................................... bx  (or ddgs + fetch)
└─ Anti-bot site (Reddit, LinkedIn, etc.) OR need clean markdown for LLM? .... WebFetch or similar fetching tool

Building a repeatable, high-volume pipeline and willing to run infra? ......... self-host SearXNG (see below)
```

---

## Comparison

| Dimension | **bx** (Brave) | **ddgs** (DuckDuckGo) |
|---|---|---|
| Cost model | ~**1000 searches/mo**, resets monthly | **Free, unlimited** (rate-limited) |
| API key | Yes (Brave key) | **No key** |
| Filters | `site:`, freshness, goggles, boolean | `site:`, region, `timelimit`, safesearch |
| Result depth | **Rich, token-budgeted snippets** (cached index) | **Short snippets only** (~160 chars) |
| Anti-bot sites | Works (reads cached index) | Discovery only |
| Multiple engines | Brave index | brave/bing/google/startpage/yandex/mojeek… |
| Reliability | High | `backend="auto"` robust; single backends flaky |
| Commercial use | OK (paid API) | Gray area (scrapes DDG) |

### Advantages / disadvantages at a glance

**bx** — ✅ cleanest output, token budgeting, goggles, strong query operators. ❌ hard monthly cap; needs paid key for volume.

**ddgs** — ✅ free, no quota, no key, multi-engine, good filters, zero infra. ❌ snippets only (no full content), per-engine rate-limit flakiness, results vary by backend.

---

## Optimal strategy (recommended default stack)

Use a **two-layer** approach — it fully replaces `bx` with no monthly wall while keeping rich content:

| Layer | Tool | Notes |
|---|---|---|
| **1. Discovery** | **`ddgs`** with `backend="auto"` | Free, unlimited. Complementary coverage to `bx` (~25% overlap in testing). |
| **2. Deep content** | **WebFetch** | Fetch the full page content for the *shortlisted* items only. |

Keep leftover **`bx`** as an optional *second discovery index* — its results barely overlap
with `ddgs`, so a few `bx` calls add unique coverage when a scan really matters.

### Reddit / anti-bot scan pattern
```
Phase 1 (discovery):  ddgs, query = '"r/<Subreddit>" <topic> site:reddit.com'   # subreddit as KEYWORD, not site: subpath
Phase 2 (content):    WebFetch <thread_url>
```
> `site:reddit.com/r/<Sub>` subpath scoping is unreliable across **all** engines (index limitation).
> Put the subreddit name in the keywords and restrict to `site:reddit.com`.

---

## When to graduate to SearXNG (self-hosted)

Move here only when you want **maximum aggregated coverage + a stable JSON API** for a
production pipeline, and you're willing to run a container:

```bash
docker run -d --name searxng -p 8080:8080 -v ./searxng:/etc/searxng searxng/searxng
# enable JSON in settings.yml:  search.formats: [html, json]
curl 'http://localhost:8080/search?q=<query>+site:reddit.com&format=json&time_range=year&safesearch=0'
```
Aggregates 70+ engines in one call with server-side merge/dedup. **Public instances disable
the JSON API** (they return 403/429), so you must self-host. Same content-depth limitation as
`ddgs` — pair with WebFetch for full content.

---

## See also
- [ddgs.md](ddgs.md) — how to use ddgs (install, backends, filters, `site:` scoping)
- [bx.md](bx.md) — bx command reference
- [bx-query-strategies.md](bx-query-strategies.md) — bx query construction
