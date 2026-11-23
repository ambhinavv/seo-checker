# SEO Checker

This repository documents the SEO checker we use to generate reports for the website and to keep checking that the site stays healthy for search and for AI answer engines.

It is its own project. It is not part of [cypherock-growth-os](https://github.com/ambhinavv/cypherock-growth-os) or [Community-Management](https://github.com/ambhinavv/Community-Management).

The checker is [Claude SEO](https://github.com/AgriciDaniel/claude-seo) by Agrici Daniel (MIT). Product page: [claude-seo.md](https://claude-seo.md/). This repo does not copy that code. It records what the tool is, which reports it writes, and how a run is used as a health check.

## What it does

Claude SEO is a plugin for the Claude Code CLI. You point it at a URL. It fetches the page (and, on a full audit, crawls the site), splits the work across specialist agents, and writes markdown reports plus a 0–100 health score and a prioritized action plan.

A full audit can run up to 17 specialists at once. The usual set covers technical SEO, content quality, schema, sitemaps, performance, screenshots, and AI-search readiness. Extra specialists run only when the site type or credentials call for them (local business, e-commerce, Google APIs, an existing drift baseline, and so on).

The score is a triage heuristic. It is not a Google ranking, a traffic forecast, or a Core Web Vitals field status.

## SEO and AI search

Two checks matter on every pass:

- **SEO.** Crawlability, indexability, security, URLs, mobile, Core Web Vitals (LCP, INP, CLS), on-page tags, content quality (E-E-A-T), schema, images, and sitemaps.
- **AI / answer-engine visibility.** Whether Google AI Overviews, ChatGPT, Perplexity, and Bing Copilot can reach and cite the page, plus agent readiness (Lighthouse Agentic Browsing, `llms.txt`, Markdown delivery, `ai-catalog.json`, WebMCP). Google’s own guidance treats this as SEO for the search experience, not a separate trick.

## Reports

Each command writes its own report. The full list, inputs, and output files are in [REPORTS.md](REPORTS.md). The score weights and severity labels are in [SCORE.md](SCORE.md). The repeatable health-check steps are in [RUNBOOK.md](RUNBOOK.md).

| Command | Report |
|---|---|
| `/seo audit <url>` | `FULL-AUDIT-REPORT.md`, `ACTION-PLAN.md`, optional `screenshots/` |
| `/seo page <url>` | Single-page on-page, content, technical, schema, image, and vitals notes |
| `/seo technical <url>` | Nine technical categories |
| `/seo content <url>` | E-E-A-T and content quality |
| `/seo schema <url>` | Schema detection, validation, and JSON-LD to add |
| `/seo geo <url>` | AI Overviews / generative-engine readiness |
| `/seo agentic <url>` | Agent readiness |
| `/seo sitemap <url>` | XML sitemap analysis or a generated sitemap |
| `/seo images <url>` | Image optimization |
| `/seo drift compare <url>` | What changed since the last baseline |

## How a run stays honest

The plugin follows a fixed architecture, documented upstream in [docs/ARCHITECTURE.md](https://github.com/AgriciDaniel/claude-seo/blob/main/docs/ARCHITECTURE.md):

1. The orchestrator skill (`skills/seo/SKILL.md`) detects the business type.
2. It delegates to agent files under `agents/` (technical, content, schema, performance, visual, geo, and others).
3. Agents fetch pages through the bundled launcher, `scripts/claude-seo run <script.py>`, inside an isolated Python environment. They do not install packages globally.
4. Results are merged into the health score and the action plan.

Page fetch supports a raw HTTP read and a Playwright render for JavaScript-heavy pages (`--render auto`).

## What this is not

- It does not install into Cursor. `/seo` is a Claude Code command. Cursor can hold this documentation; the audit itself runs in the Claude Code CLI. See [RUNBOOK.md](RUNBOOK.md).
- Copying the upstream repository into this folder does not turn the checker on. The installer places skills and agents where Claude Code loads them (`~/.claude/plugins/` or `~/.claude/skills/`).
- Optional data sources (Google Search Console, PageSpeed, CrUX, GA4, Ahrefs, DataForSEO, and others) are off until their own credentials are configured. A run with no keys still fetches the URLs you name and still writes the reports.

## Upstream

- Code: https://github.com/AgriciDaniel/claude-seo
- Commands: https://github.com/AgriciDaniel/claude-seo/blob/main/docs/COMMANDS.md
- License of the checker: MIT
