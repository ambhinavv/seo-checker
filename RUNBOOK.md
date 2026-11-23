# Runbook

Use this when generating a fresh set of reports or confirming the site is still fine for SEO and AI search.

## Requirements

- Python 3.10 or newer
- Git
- Claude Code CLI (`claude`)
- Claude SEO installed into Claude Code, not into Cursor

Playwright Chromium is optional. Install it when the site is a JavaScript app and the raw HTML is an empty shell. `/seo doctor` reports whether the runtime and Chromium are ready.

## Install (once)

Inside Claude Code:

```text
/plugin marketplace add AgriciDaniel/claude-seo
/plugin install claude-seo@agricidaniel-claude-seo
/seo setup
/seo doctor
```

Windows manual install, if the marketplace path is not used:

```powershell
git clone --depth 1 https://github.com/AgriciDaniel/claude-seo.git
powershell -ExecutionPolicy Bypass -File claude-seo\install.ps1
```

Review `install.ps1` before running it. Setup creates an isolated virtualenv under Claude’s plugin data. It does not install Python packages globally.

## Generate the report set

Start Claude Code from a terminal (Cursor’s terminal is fine; Cursor’s chat is not the runner):

```text
claude
```

Then, with the live site URL:

```text
/seo audit https://example.com
/seo page https://example.com
/seo technical https://example.com
/seo content https://example.com
/seo schema https://example.com
/seo geo https://example.com
/seo agentic https://example.com
/seo images https://example.com
/seo sitemap https://example.com
```

Save the markdown files the audit writes (`FULL-AUDIT-REPORT.md`, `ACTION-PLAN.md`, and any schema, geo, or local reports) with the date in the folder name. This documentation repository stays free of those raw reports so credentials, prospect data, and one-off findings are not committed by accident.

## Recurring health check

1. Confirm `/seo doctor` is clean.
2. Run `/seo drift compare <url>` against the stored baseline.
3. If there is a delta, or on the scheduled full pass, run `/seo audit <url>`.
4. Read Critical and High items in `ACTION-PLAN.md` before the score.
5. Re-run only the specialist command that matches a fix (`/seo schema`, `/seo technical`, `/seo geo`) to confirm that item moved.
6. When the site is accepted as the new known-good state, refresh the baseline with `/seo drift baseline <url>`.

## Cursor

Cursor can edit the site and can store this runbook. It does not load Claude SEO’s `/seo` commands or its `agents/*.md` workers. Those files activate only after Claude Code’s installer places them in Claude’s plugin or skills directory.

## Credentials

Leave Google, Ahrefs, DataForSEO, and the other extensions unset unless that data is required for the pass. Keys belong in Claude SEO’s local config (`~/.config/claude-seo/` on the upstream layout), not in this repo.
