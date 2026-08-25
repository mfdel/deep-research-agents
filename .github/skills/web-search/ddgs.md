# ddgs — DuckDuckGo Metasearch (free, no quota, no key)

`ddgs` is a lightweight Python metasearch client. It queries real search engines
(Brave, Bing, Google, Startpage, Yandex, Mojeek, DuckDuckGo…) and returns ranked
results. **No API key, no monthly quota** — only per-minute rate limits.

Best for **discovery** (finding URLs/threads). Returns short snippets only — for full
page content, use a dedicated fetch tool (e.g. WebFetch).

---

## Install

```bash
source .venv/bin/activate
python -m pip install ddgs        # use `python -m pip`, NOT bare `pip`
```

> ⚠️ **venv/pyenv gotcha:** in this workspace, bare `pip` can resolve to a pyenv 3.10
> while the venv `python` is 3.12, so `pip install ddgs` installs into the wrong
> interpreter and the import fails. Always use `python -m pip install ddgs`.

Verify:
```bash
python -c "from ddgs import DDGS; import ddgs; print(ddgs.__version__)"
```

---

## Basic usage

```python
from ddgs import DDGS

with DDGS() as d:
    rows = d.text("python asyncio tutorial", max_results=25)

for r in rows:
    print(r["title"], r["href"])
    print(r["body"])          # short snippet (~150 chars)
```

**Result shape:** each row is `{"title": ..., "href": ..., "body": ...}`.
(Note: the URL field is `href`, not `url`.)

---

## Filters

| Filter | Param | Values | Example |
|---|---|---|---|
| Region | `region` | `us-en`, `uk-en`, `se-sv`, … | `region="us-en"` |
| Freshness | `timelimit` | `d`, `w`, `m`, `y` | `timelimit="w"` (past week) |
| Safe search | `safesearch` | `on`, `moderate`, `off` | `safesearch="off"` |
| Result count | `max_results` | int | `max_results=25` |
| Engine | `backend` | see below | `backend="auto"` |

```python
d.text("water heater leak", region="us-en", timelimit="w",
       safesearch="off", max_results=25)
```

---

## Site scoping (`site:`)

There is **no dedicated site parameter** — use the Google-style `site:` operator
inside the query string:

```python
d.text("plumbing leak site:reddit.com", max_results=25)                 # single site
d.text("hvac repair site:reddit.com OR site:quora.com", max_results=25) # multiple sites
```

### Subreddit pattern (important)
`site:reddit.com/r/<Sub>` subpath scoping returns near-zero results across **all** search
engines (a search-index limitation, not a ddgs bug). Instead, restrict to `site:reddit.com`
and put the subreddit name **in the keywords**:

```python
d.text('"r/MultipleSclerosis" insurance denied appeal site:reddit.com', max_results=25)
```
Then filter results to the subreddit (case-insensitive):
```python
needle = "/r/multiplesclerosis/comments/"
kept = [r for r in rows if needle in (r.get("href","") or "").lower()]
```

---

## Backends — the source matters

`ddgs` can route to different engines. List (text search):
`bing, brave, duckduckgo, google, mojeek, startpage, yahoo, yandex, wikipedia`.

```python
d.text(query, backend="brave")            # single engine
d.text(query, backend="bing, brave, startpage")   # combine several
d.text(query)                              # default = "auto" (rotates + falls back)
```

Findings from testing the **same** query across backends:

- **Different index → different results.** Any single backend capped at ~20 hits; the
  *union* across brave+bing+startpage+yandex was ~60% larger. `startpage` and `brave`
  each contributed unique threads no other engine found.
- **Single backends are flaky** — several throw `DDGSException: No results found` when
  rate-limited on rapid successive calls.
- **Use `backend="auto"` (the default)** for robustness. Pass an explicit combo like
  `"bing, brave, startpage"` when you want maximum coverage for an important scan.

---

## Pitfalls

- **Snippets only** (~150 chars). No full page/comment content — use WebFetch for that.
- **Rate limiting:** add `time.sleep(2)` between queries in a loop; prefer `backend="auto"`.
- **`No results found` exception:** wrap calls in try/except; it often means a backend was
  temporarily blocked, not that nothing exists.
- **Case-sensitive URL filtering:** lowercase both sides when filtering by subreddit/path.
- **URL key is `href`**, not `url`.

---

## Minimal multi-query harvester

```python
import time
from ddgs import DDGS

def harvest(sub, topics, max_results=25):
    needle = f"/r/{sub}/comments/".lower()
    seen, out = set(), []
    for topic in topics:
        q = f'"r/{sub}" {topic} site:reddit.com'
        try:
            with DDGS() as d:
                rows = d.text(q, region="us-en", safesearch="off", max_results=max_results)
        except Exception:
            rows = []
        for r in rows:
            u = (r.get("href") or "").split("?")[0]
            if needle in u.lower() and u not in seen:
                seen.add(u)
                out.append({"url": u, "title": r.get("title",""), "snippet": r.get("body","")})
        time.sleep(2)
    return out
```
