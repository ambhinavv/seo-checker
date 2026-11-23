# Reports

All commands run inside Claude Code, after Claude SEO is installed. Replace the example URL with the page or site under check. Command behavior below follows the upstream command reference for [claude-seo](https://github.com/AgriciDaniel/claude-seo/blob/main/docs/COMMANDS.md).

The full audit is the recurring health check. The other commands are the separate reports when one area needs a closer look.

## Full site

`/seo audit <url>`

1. Crawls up to 500 pages.
2. Detects the business type and shifts emphasis (SaaS, local service, e-commerce, publisher, or agency).
3. Delegates to up to 17 specialists in parallel. Nine run on every full audit. Others run only when the site or the available credentials call for them.
4. Writes a 0–100 health score and a prioritized action plan.

Files:

- `FULL-AUDIT-REPORT.md`
- `ACTION-PLAN.md`
- `screenshots/` when Playwright Chromium is installed

Always-on specialists cover technical SEO, content, schema, sitemaps, performance, visual capture, AI search (geo), search experience (sxo), and the score synthesis. Conditional specialists include Google API data, local SEO, maps, backlinks, topic clusters, drift (only if a baseline already exists), e-commerce, and agentic readiness when that pass is included.

## Single page

`/seo page <url>`

One URL, read in depth:

- On-page SEO: title, meta description, headings, URL
- Content: length, readability, E-E-A-T
- Technical: canonical, robots, Open Graph
- Schema markup
- Images: alt text, size, format
- Core Web Vitals issues visible from the page

## Technical

`/seo technical <url>`

Nine categories:

1. Crawlability
2. Indexability
3. Security
4. URL structure
5. Mobile
6. Core Web Vitals (LCP, INP, CLS)
7. Structured data
8. JavaScript rendering
9. IndexNow

## Content

`/seo content <url>`

Experience, expertise, authoritativeness, and trust, plus AI citation readiness and freshness. Thin or scaled low-value pages are called out here.

`/seo content-brief "<topic>"` is the planning report, not a live-site grade. It returns target queries, intent, a heading outline, internal links, and the competitor angle to beat.

## Schema

`/seo schema <url>`

Detects JSON-LD, Microdata, and RDFa, checks them against Google’s current rich-result rules, and drafts JSON-LD for gaps. Retired types (HowTo rich results, and FAQ rich results after Google’s May 2026 change) are not recommended as ranking fixes.

A schema pass writes a schema report in the same style as `SCHEMA-REPORT.md`: types found, property-level status, and critical mismatches between the page and the markup.

## AI search

`/seo geo <url>`

Generative-engine check for Google AI Overviews and related answer engines:

- Passage citability (self-contained facts)
- Headings, lists, and tables
- Entity clarity
- Authority and source signals
- Structured data that supports those passages

`/seo agentic <url>`

Whether an agent can read and use the site:

- Lighthouse Agentic Browsing, counted the way Lighthouse counts it
- Accessibility tree
- `robots.txt` and Content-Signal for AI crawlers
- `llms.txt`, Markdown delivery, `ai-catalog.json`, and `/.well-known`
- WebMCP (draft). `fix` only drafts files for review. It does not deploy them.

Upstream also writes `GEO-ANALYSIS.md` for the AI-search pass.

## Other reports we can run on the same site

| Command | What the report covers |
|---|---|
| `/seo sitemap <url>` | Existing XML sitemap, or a generated one |
| `/seo images <url>` | Format, weight, alt text, and preview images |
| `/seo hreflang <url>` | International alternates |
| `/seo local <url>` | Google Business Profile, NAP, citations, reviews. Output shape follows `LOCAL-SEO-ANALYSIS.md` |
| `/seo backlinks <url>` | Link profile from free sources (Common Crawl, Moz, Bing) unless a paid extension is configured |
| `/seo google <command> <url>` | Search Console, PageSpeed, CrUX, GA4, Indexing API. Requires that tier’s credentials |
| `/seo drift baseline <url>` | Saves an on-page snapshot |
| `/seo drift compare <url>` | Diff against that snapshot so the next check shows what moved |
| `/seo plan <type>` | Strategy notes for saas, local, ecommerce, publisher, or agency |
| `/seo ecommerce <url>` | Product schema and shopping markup |
| `/seo programmatic` | Scaled page sets and quality gates |
| `/seo competitor-pages` | Comparison and alternatives page briefs |

## Drift is the “still fine” check

A one-off audit is a snapshot. The repeating check is:

1. `/seo drift baseline <url>` once, when the site is in a known-good state.
2. Later, `/seo drift compare <url>` to see on-page regressions.
3. `/seo audit <url>` when the compare shows a real change, or on the regular full pass.

Drift stores baselines locally (SQLite). Those database files are operational data. They are not committed to this documentation repo.

## PDF

With WeasyPrint available, `/seo google` can also render a long-form PDF (cover, summary, findings, recommendations). The markdown files above are the primary record.
