# Health score

`/seo audit` rolls the specialist findings into one 0–100 score. Use the score to order work. Read the findings before treating the number as a verdict.

Published weights on [claude-seo.md](https://claude-seo.md/):

| Area | Weight |
|---|---|
| Content quality | 23% |
| Technical SEO | 22% |
| On-page SEO | 20% |
| Schema | 10% |
| Performance | 10% |
| AI readiness | 10% |
| Images | 5% |

AI readiness is the answer-engine slice: citability, crawler access, and the agentic checks. It shares the score with classic SEO. It does not replace it.

## Severity

| Label | Meaning |
|---|---|
| Critical | Can block crawling, indexing, or a core user flow. Review immediately. |
| High | Clear user or search risk. Review within a week. |
| Medium | Optimization. Target within a month. |
| Low | Backlog. |

Each recommendation is supposed to carry the observation it rests on, what it depends on, how you would know the fix failed, and a leading indicator to watch. If a report item has no failure check, treat it as a note, not a task.

## What the number is not

- Not a Google quality score.
- Not a prediction of rankings, clicks, or AI citations.
- Not field Core Web Vitals, unless CrUX credentials are configured. Without them, vitals are lab estimates from the page and from PageSpeed when that key exists.

## Business type

The orchestrator classifies the homepage and changes emphasis. It does not drop categories. A SaaS site still gets the technical and schema passes. A local site additionally gets the local and maps passes when those signals are present.
