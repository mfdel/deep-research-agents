---
topic: "Redpine (redpine.ai) — company profile, funding, and open positions"
date: 2026-08-20
queries_run: 5
sources_fetched: 9
---

# Research Report: Redpine (redpine.ai)

## Executive Summary

Redpine is a Stockholm-based AI data-infrastructure startup founded in 2024 by Anders Hammarbäck (CEO, ex-McKinsey, ex-Antler) and David Österdahl (CPTO, ex-Spotify, ex-iZettle). It sells a "knowledge layer for agentic AI": licensed, non-public data served to AI agents and models in real time through an API, MCP server, and CLI, billed per token. The company raised a €1.1M angel pre-seed in September 2025 and a €6.8M / $8M seed in April 2026 led by NordicNinja, with Luminar Ventures and node.vc participating — roughly €9M total. It is small (10+ people stated in a job ad) and hiring aggressively: **7 live openings, all full-time, all on-site at the Stockholm HQ**, six of them technical.

## Key Findings

### Product and technology

- Positioning: "The Knowledge Layer for Agentic AI" — "the grounding tool for non-public data." Access via API, MCP, and CLI; client integrations listed for Cursor, ChatGPT, Antigravity, Claude Desktop, and raw MCP. [Source: redpine.ai]
- Data types: scientific journals, real-time signals, non-public image libraries, video assets, private code repositories — "data that does not exist in any public crawl." Company claim: only 1–2% of the world's data is openly available for AI training. [Source: redpine.ai, eu-startups.com]
- Business model: direct licensing deals with content owners rather than scraping; agents pay per token consumed, and data providers are compensated from that flow. Pricing tiers on site: Simple Search, Enterprise Search, Deeper Research; 5 free queries then pay-as-you-go. [Source: redpine.ai, eu-startups.com]
- Technical core: a "headless API interface" combining "proprietary retrieval and reranking technology with real-time data evaluation." Founding data scientist Dr Leonora Vesterbacka (PhD, CERN, ex-KBLab) leads retrieval and reranking. Job ads name the working stack: Go, Rust, Python, multi-cloud, and techniques spanning "embeddings, vector and lexical indexes, learning-to-rank, hybrid systems & knowledge graphs." [Source: tech.eu, eu-startups.com, builtin.com]
- `methodology.redpine.ai` — the deepest intended technical source — is whitelist-gated and self-labels as a demo, not the production product. No public architecture doc or benchmark numbers exist.

### Traction and customers

- Works with "leading international AI labs" (unnamed), plus named partners AsedaSciences (US biotech) and Chegg. Data partner All Ears supplies "50,000 hours of daily spoken media." [Source: redpine.ai, tech.eu]
- At launch (Sept 2025) claimed access to "more than a hundred billion tokens of premium data." [Source: eu-startups.com]
- Target verticals where hallucination is unacceptable: healthcare, law, finance, robotics. Hammarbäck names clinical guidelines, case law, physics research, financial-market data, and human-created news as application areas. [Source: eu-startups.com, tech.eu]
- No revenue, ARR, customer count, or valuation is public. PitchBook is the only likely source and is paywalled; the Sifted article (the one piece targeted at valuation and revenue) returned HTTP 403.

### Funding and investors

| Round | Date | Amount | Investors |
|---|---|---|---|
| Pre-seed | 26 Sep 2025 | €1.1M | Angel-only: Colin M. Evans (OpenAI), Feng Hong (co-founder, Xiaomi), Anna Nordell Westling (co-founder, Sana), Daniel Langkilde (founder, Kognic), Gustav Lindqvist (Perplexity), Spotify alumni network |
| Seed | 28 Apr 2026 | €6.8M (= $8M, same round) | Lead: NordicNinja. Participants: Luminar Ventures, node.vc. Angels: Peter Sarlin (Silo AI), Patrik Tran (Validio), Anna Nordell Westling (Sana), unnamed leaders from OpenAI, Perplexity, Spotify |

- Cumulative funding reported as €9M; the two named rounds sum to €7.9M, so ~€1.1M is unaccounted for in the reported total. [Source: eu-startups.com]
- Marek Kiisa, General Partner at NordicNinja, joined the board with the seed.
- Seed use of proceeds: product development (proprietary retrieval and reranking), expanding data partnerships, global expansion, and hiring across engineering, data science, and go-to-market.

### Open positions (Ashby board, canonical — 7 live)

All roles: full-time, **on-site at Redpine HQ in central Stockholm**, no remote option, no secondary locations. All require "a valid Swedish work permit or similar eligibility"; relocation support is available. No salary bands are published on any posting (`shouldDisplayCompensationOnJobPostings: false`); equity is described in prose as "meaningful" or "significant equity upside."

| Title | Dept | Published | URL |
|---|---|---|---|
| Head of Engineering | Tech | 2026-07-05 | https://jobs.ashbyhq.com/redpine/68f9376a-c0d2-4c36-b4b4-9b79219b2335 |
| Designer, Brand & Product | Tech | 2026-07-08 | https://jobs.ashbyhq.com/redpine/1ce4da85-256c-4ecc-86c8-005183211117 |
| Senior Knowledge Graph Engineer | Tech | 2026-06-24 | https://jobs.ashbyhq.com/redpine/3b52dc4b-b985-427c-90f4-bb3697716340 |
| AI Engineer, Cloud Infrastructure | Tech | 2026-05-06 | https://jobs.ashbyhq.com/redpine/38cd70c8-c776-40fc-80ea-c4fd48d93194 |
| AI Engineer, Retrieval & Quality | Tech | 2026-05-06 | https://jobs.ashbyhq.com/redpine/be3acdef-df36-441b-bb79-09d06a79c951 |
| Applied AI Engineer | Tech | 2025-12-17 | https://jobs.ashbyhq.com/redpine/3819d4bc-9a5d-4a8a-8f19-08015414be1e |
| Go to Market Internship | Internships | 2026-08-05 | https://jobs.ashbyhq.com/redpine/a84338b0-315c-4942-b397-8de250bf0c6c |

Apply link for each = posting URL + `/application`.

Role detail worth noting:

- **AI Engineer, Cloud Infrastructure** — despite the title, this is senior/staff-level distributed data-infrastructure ownership with no manager scope: vector databases, object storage at scale, streaming/ingestion, Kubernetes, and "architectural decisions that will define how the platform scales globally." Ad states "Small team 10+ people, high ownership" and "You won't be scaling someone else's architecture, instead you'll be defining it."
- **AI Engineer, Retrieval & Quality** — tagged mid-level on Built In. Owns the retrieval pipeline (hybrid search, ranking, re-ranking), builds continuous evaluation infrastructure that gates deployments on quality metrics, and builds dashboards benchmarking performance "across customers and competitors." Requires "production experience with retrieval systems: search, ranking, recommendation systems, or RAG at scale."
- **Senior Knowledge Graph Engineer** — "Strong Python and a track record of building data pipelines from scratch."
- **Applied AI Engineer** — ML for data quality and validation, automated labeling, data-processing pipelines.
- **Head of Engineering** — the only leadership-scope opening on the board.
- Hiring philosophy signal from the Retrieval & Quality ad: "We work primarily in Go, Rust and Python" and "We hire for judgment, not stack."
- Core team is drawn from Spotify, Sana, Zettle, and Lunar, plus McKinsey, CERN, and H&M.

## Diverging Views / Uncertainty

- **Headcount is unresolved.** Seedtable reports ~6 employees (likely stale, pre-seed-round); the Cloud Infrastructure job ad says "10+ people." With 7 open roles, the team is in the middle of roughly doubling.
- **Funding math does not fully reconcile.** €1.1M + €6.8M = €7.9M against a reported €9M total. Either an unannounced bridge exists or the €9M is rounded/approximate.
- **A "Founding Engineer – Full Stack" role at `redpine.ai/careers/founding-engineer-full-stack` returns 404** and does not appear on the live Ashby board. Treat as removed; do not cite it as open.
- **No valuation, revenue, or customer-count data is publicly available.** The Sifted piece was blocked (403) and PitchBook is paywalled.
- **The technical claims are self-reported.** "Proprietary retrieval and reranking" is company language with no public benchmark behind it.

## Sources

| Title | URL | Relevance |
|---|---|---|
| Redpine homepage | https://www.redpine.ai/ | Product positioning, access methods, pricing, partners |
| Redpine Ashby job board (public posting API) | https://jobs.ashbyhq.com/redpine | Canonical list of all 7 open roles |
| AI Engineer, Cloud Infrastructure | https://jobs.ashbyhq.com/redpine/38cd70c8-c776-40fc-80ea-c4fd48d93194 | Full JD, team size, work-permit and relocation terms |
| AI Engineer, Retrieval & Quality (Built In) | https://builtin.com/job/ai-engineer-retrieval-quality/9268075 | Full JD, seniority tag, stack, evaluation mandate |
| Redpine Raises $8M Seed | https://www.redpine.ai/insights/redpine-raises-8m-seed | Official seed narrative, investors, use of proceeds |
| Redpine secures €6.8M (tech.eu) | https://tech.eu/2026/04/28/redpine-secures-eur68m-to-power-ai-with-premium-data/ | Round mechanics, CEO quotes, platform description |
| Stockholm's Redpine raises €6.8M (EU-Startups) | https://www.eu-startups.com/2026/04/stockholms-redpine-raises-e6-8-million-to-unlock-licensed-premium-data-for-ai-agents/ | Founders, full investor list, total raised, hiring roadmap |
| Sweden's Redpine launches with €1.1M (EU-Startups) | https://www.eu-startups.com/2025/09/swedens-redpine-launches-with-e1-1-million-backing-to-reduce-ai-hallucinations-through-licensed-data/ | Founding story, pre-seed angels, founder backgrounds |
| Redpine careers page | https://www.redpine.ai/careers | Confirms Ashby embed; no listings in raw HTML |
| Sifted (blocked) | https://sifted.eu/articles/redpine-raises-e6-8m-to-give-ai-agents-access-to-non-public-data | FETCH_FAILED — HTTP 403 |
| methodology.redpine.ai (gated) | https://methodology.redpine.ai/ | FETCH_FAILED — whitelist sign-in wall |

**Disambiguation note:** not related to Redpine Signals, the semiconductor firm acquired by Silicon Labs.
