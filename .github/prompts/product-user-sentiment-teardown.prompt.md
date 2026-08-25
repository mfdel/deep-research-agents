---
name: "Product User Sentiment & Churn Teardown"
description: "Mine authentic user reviews, forum discussions, and community complaints to uncover product strengths, critical weaknesses, and unmet customer needs."
---

# Product User Sentiment & Churn Teardown

Analyze authentic user feedback across public forums and review platforms to understand real-world customer satisfaction, churn triggers, and feature gaps.

## Input Parameters
- **Target Product / Competitor:** Name of product (e.g. `Retool`, `Linear`, `Tableau`, `Shopify POS`)
- **Category:** (e.g. Internal Tool Builders, Issue Tracking, Business Intelligence)
- **Target Persona:** (e.g. Software Engineers, Product Managers, Small Business Owners)

## Orchestrator Brief Template
```
Research Topic: Voice-of-customer and sentiment analysis for [TARGET PRODUCT] in the [CATEGORY] space.

Research Goals:
1. Core Value Drivers: What do users praise the most? What are the "aha moments" that drive retention and advocacy?
2. Top Recurring Pain Points: What are the top 5 most frequent user complaints on Reddit, G2, Capterra, and Trustpilot?
3. Churn & Switching Reasons: Why do teams abandon this product? What alternatives do they switch to, and what triggers the migration?
4. Pricing & Packaging Friction: Are users complaining about unexpected tier jumps, per-seat penalties, or opaque billing?
5. Missing Features & Workarounds: What functionality are users actively asking for or hacking together with third-party tools?
6. Support & Reliability Signals: Outage frequency, customer support responsiveness, and breaking updates.

Prioritized Sources:
- Subreddit communities (e.g. r/SaaS, r/webdev, r/startups, r/sysadmin)
- G2, Capterra, Trustpilot, Product Hunt reviews
- Twitter / X complaints, Hacker News "Show HN" and "Ask HN" discussions
```

## Output Format
The report must be saved as `research/sentiment_[product_slug]_[YYYYMMDD].md` containing:
1. **Sentiment Scorecard:** Net sentiment summary and satisfaction breakdown.
2. **Top 5 Praise Themes:** With direct quoted evidence.
3. **Top 5 Critical Pain Points:** Grouped by severity, with authentic user quotes and thread links.
4. **Churn & Migration Pathways:** Table showing where churned users migrate and why.
5. **Product Opportunity White Space:** Specific unmet needs a competitor could exploit.
