---
name: "Nordic AI Company Discovery"
description: "Discover AI-native companies in the Nordics that align with an individual's professional background and domain expertise. Dispatches the Web Research Orchestrator with a grounded research brief."
---

You are helping discover AI-native companies in the Nordic region that align with a specific individual's professional profile.

Before dispatching the research pipeline, collect the following from the user (or ask them to load the relevant files):

1. **Professional background** — roles, companies, domain expertise, years of experience
2. **Target geography** — cities or countries (e.g. Stockholm, Malmö, Gothenburg, Copenhagen)
3. **Company stage** — e.g. Series A, B, C, growth
4. **Domain focus** — e.g. fintech AI, B2B SaaS, credit risk, pricing, LLMs
5. **Role type** — e.g. Head of ML, Staff Engineer, VP AI
6. **Compensation floor** — monthly salary in SEK or DKK (optional, used to filter stage/scale)
7. **Hard constraints** — excluded companies, location limits, any other filters

Once you have the above, dispatch the **Web Research Orchestrator** with this research brief structure:

---

```
Research topic: AI-native and AI-first companies in [TARGET GEOGRAPHY] that are [STAGE] startups
or scale-ups, with a focus on [DOMAIN FOCUS], that would be strong job targets for a senior AI/ML leader.

Candidate context:
- [YEARS] years of [DOMAIN] experience, currently [CURRENT ROLE] at [CURRENT COMPANY]
- Background in [KEY DIFFERENTIATOR] — e.g. rare domain+ML hybrid
- Expert in [TECH STACK]
- Looking for: [ROLE TYPE] roles
- Must-haves: AI is the CORE product, high talent density, [STAGE], autonomy to build teams
- Location: [LOCATION CONSTRAINT]
- Salary floor: [FLOOR]
- Excluded companies: [EXCLUSIONS]

Research goals:
1. AI-native startups/scale-ups in [GEOGRAPHY] with [HEADCOUNT RANGE] employees
2. [DOMAIN] specifically — [SUB-DOMAINS]
3. Companies that have raised [STAGE] in [YEAR RANGE]
4. US-HQ AI companies expanding to Nordic offices
5. [SECONDARY GEOGRAPHY] companies if applicable

Sources to prioritise: Sifted, EU-Startups, TechCrunch, Crunchbase, FundedIQ, AliceLabs Sweden AI Report,
Dealroom Nordic, VC portfolio pages (EQT Ventures, Northzone, Creandum).

Constraints: Prefer fresh sources (last 12–18 months).
```

---

After research completes, ask the orchestrator to classify each company into:

- **Tier 1** — Apply now (strong match on product domain, stage, culture signal, and salary floor)
- **Tier 2** — Apply selectively (fits most criteria, one gap)
- **Tier 3** — Monitor (timing wrong, geography non-ideal, or waiting for a trigger event)
- **Ruled out** — Clear disqualifier found

Output a table with: Company, City, Stage, Domain, Tier, and a one-line fit rationale.
