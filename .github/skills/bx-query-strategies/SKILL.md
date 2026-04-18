---
name: bx-query-strategies
description: USE FOR writing effective bx search queries. Covers boolean logic, domain targeting, content operators, freshness flags, goggles, relevance threshold, command selection, and use-case recipes for technical research, competitive intelligence, community research, link prospecting, and content verification.
---

# bx Query Strategies

Apply these techniques when constructing or refining `bx` search queries.

---

## Boolean & Logic in the Query String

All of these work directly inside the `bx "QUERY"` string:

| Technique | Syntax | Example | When to use |
|-----------|--------|---------|-------------|
| Exact phrase | `"phrase"` | `"supply chain disruption"` | Precise wording, error messages, quotes |
| Exclude term | `-term` | `python -snake` | Disambiguation, remove noise |
| Either term | `OR` or `\|` | `React OR Vue` | Synonyms, alternate names |
| Force both terms | `AND` | `React AND TypeScript` | When implicit AND is ambiguous in grouped queries |
| Grouping | `( )` | `(React OR Vue) AND TypeScript` | Complex boolean logic — essential for multi-angle queries |
| Wildcard | `*` | `"best * for SEO"` | Fill-in-the-blank inside exact phrases |
| Number/year range | `N..M` | `laptop $500..$800`, `AI regulation 2022..2025` | Price ranges, year ranges |

**Rules:**
- `OR` and `AND` must be **uppercase** — lowercase is treated as query text
- No space after operators that prefix a term: `"exact phrase"` ✓, `-term` ✓
- Limit to ~4 stacked operators before behavior becomes unpredictable

---

## Domain & Source Targeting

**Prefer `bx` flags over inline `site:` operators** — they are more reliable and composable:

```bash
# Restrict to one domain (flag = more reliable than site: in query)
bx "kubernetes ingress" --include-site docs.kubernetes.io

# Restrict to multiple domains (multi-flag OR logic)
bx "zero trust security" --include-site nist.gov --include-site cisa.gov

# Exclude a domain
bx "python tutorial" --exclude-site w3schools.com

# TLD-scoped authoritative search — use inline site: for TLD patterns
bx "climate policy filetype:pdf site:.gov"
bx "machine learning site:.edu"
```

When inline `site:` is still useful (in the `q` string):
```bash
# Multiple sites with OR — no multi-flag equivalent
bx "topic site:reddit.com OR site:news.ycombinator.com OR site:lobste.rs"

# Subdirectory scoping
bx "topic site:example.com/blog"

# Exclude site inline (combine with --include-site)
bx "AI tools -site:openai.com -site:google.com --include-site github.com"
```

> `--include-site`, `--exclude-site`, and `--goggles` are mutually exclusive in `bx`. Use goggles when you need all three together.

---

## Content-Specific Operators (in query string)

| Operator | Syntax | Example | Effect |
|----------|--------|---------|--------|
| `intitle:` | `intitle:"release notes"` | `intitle:"release notes" kubernetes` | Keyword must be in the page title |
| `allintitle:` | `allintitle:word1 word2` | `allintitle:python beginner guide` | All words must appear in the page title |
| `inurl:` | `inurl:resources` | `inurl:resources mountain bike` | Keyword must appear in the URL |
| `allinurl:` | `allinurl:api docs` | `allinurl:api reference docs` | All words in the URL |
| `intext:` | `intext:"exact phrase"` | `intext:"cookie policy" GDPR` | Keyword must appear in body text |
| `filetype:` | `filetype:pdf` | `climate policy filetype:pdf` | Restrict to file format (PDF, DOC, PPT, XLS, etc.) |

> Use `intitle:` (not `allintitle:`) when combining with other operators — `allintitle:` is less reliable in complex queries.

**High-value combinations:**
```bash
bx "climate policy filetype:pdf site:.gov"              # Government PDF reports
bx "inurl:docs intitle:API site:stripe.com"             # Technical docs on a specific site
bx "topic intitle:\"write for us\" inurl:write-for-us"  # Guest post opportunities
bx "\"natural language processing\" filetype:pdf site:.edu"  # Academic papers
bx "\"exact error message\" site:stackoverflow.com"     # Exact error on Stack Overflow
```

---

## Date & Freshness Filtering

**Use `--freshness` flags** for recency:

| bx flag | Recency |
|---------|---------|
| `--freshness pd` | Past day |
| `--freshness pw` | Past week |
| `--freshness pm` | Past month |
| `--freshness py` | Past year |

```bash
bx news "AI regulation" --freshness pd      # Latest news
bx web "kubernetes release" --freshness pw  # Recent articles
bx "LLM benchmarks" --freshness py          # Past year
```

**Inline date operators** for specific date boundaries (in the query string):
```bash
bx "AI regulation after:2025-01-01"
bx "\"gig economy\" before:2020-01-01"
bx "AI agent frameworks after:2024-01-01 before:2025-06-01"
```

> Prefer `--freshness` flags for rough recency, and `after:`/`before:` when you need a specific date boundary.

---

## Choosing the Right bx Command

| Situation | Command | Notes |
|-----------|---------|-------|
| General web research (default) | `bx "query"` | Pre-extracted, token-budgeted; best for content you'll read |
| URL discovery / link lists | `bx web "query"` | Returns raw results array; best for scouting |
| Latest news / current events | `bx news "query" --freshness pd` | Optimized for articles |
| Forum/community opinions | `bx web "query" --result-filter discussions` | Surfaces Reddit, Hacker News, StackExchange |
| Synthesized explanation | `bx answers "query" --no-stream` | AI-generated summary, cites sources |
| Deep multi-angle research | `bx answers "query" --enable-research` | Iterative multi-search; slower but comprehensive |
| Images | `bx images "query"` | Up to 200 image results |
| Videos | `bx videos "query"` | Returns duration, views, creator |
| Local places / businesses | `bx places "query" --location "City"` | 200M+ POI database |

---

## Goggles — Domain Ranking Control

When `--include-site` and `--exclude-site` are insufficient (e.g. you need boost + block simultaneously), use goggles:

```bash
# Boost authoritative docs, demote SEO blogs
bx "axum middleware" \
  --goggles '$boost=5,site=docs.rs
$boost=3,site=github.com
/blog/$downrank=4' --max-tokens 4096

# Allowlist mode — only results from specific sites
bx "Python asyncio patterns" \
  --goggles '$discard
$boost,site=docs.python.org
$boost,site=peps.python.org'

# Pipe rules from stdin
echo '$boost=10,site=arxiv.org
$boost=5,site=semanticscholar.org
$discard,site=medium.com' | bx "LLM evaluation benchmarks" --goggles @-
```

**Goggle DSL quick reference:**

| Rule | Effect | Example |
|------|--------|---------|
| `$boost=N,site=DOMAIN` | Promote domain (N=1–10) | `$boost=5,site=docs.rs` |
| `$downrank=N,site=DOMAIN` | Demote domain | `$downrank=4,site=medium.com` |
| `$discard,site=DOMAIN` | Remove domain entirely | `$discard,site=pinterest.com` |
| `/path/$boost=N` | Boost matching URL paths | `/docs/$boost=5` |
| `$discard` (first rule) | Allowlist mode — discard all unmatched | `$discard` |

---

## Relevance Threshold

Control how strictly results must match the query:

```bash
--threshold strict     # Tight match — fewer but highly relevant results
--threshold balanced   # Default — good balance
--threshold lenient    # Broad match — more results, lower average relevance
```

Use `--threshold strict` as the bx equivalent of verbatim mode — when searching for exact technical terms, error codes, version numbers, or brand names where synonym substitution would corrupt the results.

---

## Use-Case Recipes

**Technical & Developer Research:**
```bash
bx "\"TypeError: cannot unpack non-iterable NoneType\"" --include-site stackoverflow.com
bx "intitle:\"release notes\" site:example.com" --freshness py
bx "topic filetype:py site:github.com"
bx "\"deprecated\" intext:\"function_name\" site:example.com"
```

**Research & Fact-Finding:**
```bash
bx "climate policy statistics filetype:pdf site:.gov"
bx "\"natural language processing\" filetype:pdf site:.edu" --freshness py
bx "\"claim or quote\" site:.gov OR site:.edu OR site:.org"
bx news "topic" --freshness pw
```

**Competitive Intelligence:**
```bash
bx "site:competitor.com/blog after:2025-01-01"
bx "intitle:\"best SEO tools\" -yourbrand after:2025-01-01"
bx "allintitle:review (Product1 OR Product2) after:2024-01-01"
bx "intext:\"Competitor Name\" -site:competitor.com -site:yourdomain.com"
bx "\"keyword research\" intitle:guide -site:ahrefs.com -site:semrush.com -site:moz.com"
```

**Audience & Community Research:**
```bash
bx web "topic" --result-filter discussions --freshness pm
bx "topic site:reddit.com" --freshness pw
bx "topic site:reddit.com OR site:news.ycombinator.com OR site:stackexchange.com"
bx "site:quora.com topic after:2024-01-01"
```

**Link Prospecting (SEO):**
```bash
bx "topic intitle:\"write for us\" inurl:write-for-us"
bx "topic \"become a contributor\" inurl:contribute"
bx "topic intitle:resources inurl:resources"
bx "topic intext:\"this is a sponsored post by\""
```

**Content Verification:**
```bash
bx "\"paste a distinctive sentence from the original\""
bx "\"unique sentence\" -site:yourdomain.com"
bx "\"alleged quote\" site:snopes.com OR site:factcheck.org"
```

---

## Adaptive Search Loop Patterns

Apply these patterns when deciding what query to run next:

```bash
# Broaden with OR when first query is too narrow
Q1: bx "kubernetes network policy"
Q2: bx "kubernetes (network policy OR CNI OR ingress)"

# Narrow with site: + freshness when too much noise
Q1: bx "AI agent frameworks"
Q2: bx "AI agent frameworks" --include-site github.com --freshness py

# Add --result-filter discussions for community signal
Q1: bx web "topic"
Q2: bx web "topic" --result-filter discussions

# Use answers for synthesis when results are fragmented
bx answers "compare approach A vs approach B" --no-stream

# Strict threshold for technical queries
bx "\"exact API method name\" error" --threshold strict
```
