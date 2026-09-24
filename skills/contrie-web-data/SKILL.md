---
name: contrie-web-data
description: Get specific facts or fields from one public web page (prices, dates, rates, table rows, specifications, contact or company details, citation details) as validated JSON with a per-field source check and signed evidence, read a page as clean markdown, check data you already have against its source page, or watch a page for a changed answer. Use when the answer lives on a known public URL and the user or agent needs values they can trust and trace back to the page.
---

# Web data with evidence (Contrie)

Contrie reads one public web page per call and returns either the fields you ask for, as JSON
with an acceptance decision and page excerpts, or the page as structure-preserving markdown.
The `contrie` MCP server in this plugin provides the tools below.

## When Contrie fits

- You know the page (or the user gave a URL) and need particular values from it: a price and
  currency, a rate and its effective date, the rows of a table, a product's specifications, a
  paper's citation details, a company's founders and headquarters.
- The values will be used downstream (stored, compared, acted on), so you need to know which
  ones actually appear on the page and see the supporting excerpt.
- You have values from elsewhere and want to check them against their source page.
- You want to be told when an answer on a page changes.

## When it does not

- Finding pages: Contrie is not a search engine. Find the URL first, then extract.
- Many pages or a whole site: one page per call; it never follows links or crawls.
- PDFs, images, audio, video, archives and other files: refused at 0 credits.
- Pages behind a login or paywall, and reddit.com.
- Sites that refuse automated requests: the call ends as `SOURCE_REFUSED` at 0 credits. Do not
  retry it in a loop.

## Tools

| Tool | Use it to | Credits |
|---|---|---|
| `contrie_extract` | Get fields from a page: `url`, plus `extract` (plain language) and/or `schema` (JSON Schema for the exact shape) | 1, 3, 8 or 15 by request, +2 if rendered; 0 if not accepted |
| `contrie_read` | Get the page as markdown to read yourself | 1, +2 if rendered |
| `contrie_verify` | Check `data` you already have against a `url` (or raw `html`/`text`) | 0 |
| `contrie_watch` | Re-run one extraction on a schedule (`every` minutes, min 15) and record changes | each run is charged like an extraction |
| `contrie_changes`, `contrie_monitors`, `contrie_unwatch` | Read a monitor's history, list monitors, stop one | 0 |
| `contrie_account` | See remaining credits and restrictions | 0 |

## Reading a result honestly

- Treat structured data as accepted only when `success` is true, `valid` is true and
  `qualityScore` is at least 70. A rejected result is charged 0 credits.
- `groundingFields` says, per value, whether its text was found in the page, with an excerpt.
  It checks presence, not meaning: it does not prove the value belongs to that field. When a
  value matters, read its excerpt before relying on it.
- A `null` means the page did not state that field. Report it as unknown; do not fill it in.
- Say where a value came from: cite the URL, and quote the excerpt when the user will act on it.
- `metadata.credits` is what the call cost.

## Good requests

- Pass a JSON Schema when the shape matters; ask only for fields the page actually shows.
- Name the unit or format you need in the description ("price as a number, currency as an ISO
  code"), rather than post-processing a guessed string.
- Use `contrie_read` when you need to understand the page, and `contrie_extract` when you need
  specific values.
- Stop monitors you no longer need with `contrie_unwatch`; each scheduled run is charged.

## Setup

The first tool call that needs an account asks you to sign in to Contrie (OAuth); a free account
can be created during sign-in and includes up to 200 credits a month. In Claude Code, open `/mcp`
and choose the `contrie` server to sign in. Headless and CI use can send an API key instead,
created at https://www.contrie.com/dashboard/keys. Full docs: https://www.contrie.com/docs/mcp.
