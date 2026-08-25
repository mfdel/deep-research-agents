---
name: "Company 360° Deep Dive"
description: "In-depth multi-source audit of a single company covering business model, architecture, tech stack, funding, executive background, hiring velocity, and key risks."
---

# Company 360° Deep Dive

Conduct an exhaustive, evidence-backed teardown of a target company by dispatching the **Web Research Orchestrator**.

## Input Parameters
Collect or define the following before running:
- **Target Company:** Name and primary website (e.g. `Gilion`, `gilion.com`)
- **Key Questions / Focus Areas:** (e.g. Tech architecture, business model & unit economics, leadership pedigree, live open roles, or competitive moat)
- **Time Horizon:** Recent updates (past 6–12 months) vs founding history

## Orchestrator Brief Template
```
Research Topic: Comprehensive 360° audit of [COMPANY NAME] ([WEBSITE]).

Research Goals:
1. Business Model & Offering: What is the core product? How do they make money (pricing tiers, transaction cuts, SaaS)? Who is their ICP (Ideal Customer Profile)?
2. Technology & Architecture: What is their core tech stack, backend services, AI/ML models, and API integrations? Do they build proprietary models or orchestrate third-party LLMs?
3. Funding & Financial Health: Total raised, latest round valuation, lead investors (VC/PE), estimated revenue, and burn/growth trajectory.
4. Leadership & Talent Density: Founders and C-suite background (ex-FAANG, serial founders, domain veterans), engineering headcount, key recent hires.
5. Hiring Velocity & Open Roles: What roles are currently open on their career page/LinkedIn? What do the job descriptions reveal about upcoming product bets or tech transitions?
6. Real Customer & Industry Sentiment: What are customers and industry analysts saying on forums, news, Reddit, and review sites?
7. Moat & Strategic Risks: What is their defensibility (data network effects, workflow lock-in, regulatory licenses)? What are their top 3 failure modes?

Prioritized Sources:
- Official website, docs, engineering blog, and press releases
- Crunchbase, Dealroom, PitchBook, Sifted, TechCrunch
- LinkedIn company profile, job postings, employee alumni flows
- GitHub public organization / repositories (if any)
- Independent reviews (G2, Capterra, Reddit, Glassdoor)
```

## Output Format
The final report must be saved as `research/research_[company_slug]_[YYYYMMDD].md` containing:
1. **Executive Summary:** 5-bullet summary + 1-sentence verdict.
2. **Company Factsheet:** Founded date, HQ, founders, headcount, funding, valuation, investors.
3. **Product & Business Model Teardown:** How value is created and monetized.
4. **Technical Architecture & Data Flows:** Ingestion, models, orchestration, infrastructure.
5. **Leadership & Organization:** Founder histories and talent signals.
6. **Hiring Signals & Roadmap Insights:** What live job descriptions reveal about strategic priorities.
7. **Competitive Matrix & Moat Evaluation:** Competitors, defensibility rating (1–5), and key vulnerabilities.
8. **Sources & Evidence Table:** Numbered list of citations.
