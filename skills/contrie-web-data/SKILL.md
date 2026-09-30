---
name: contrie-web-data
description: Get specific facts or fields from one public web page (prices, dates, rates, table rows, specifications, contact or company details, citation details) as JSON with an acceptance decision and the page excerpt behind each value, read a page as clean markdown, check whether values you already have appear in their source page, or watch a page for a changed answer. Use when the answer lives on a known public URL and the user or agent needs values they can trace back to the page.
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
  ones appear on the page and see the supporting excerpt.
- You have values from elsewhere and want to check them against their source page.
- You want to be told when an answer on a page changes.

## When it does not

- Finding pages on the open web: Contrie is not a web search engine. Find the URL first, then extract.
  `contrie_search` covers only Contrie's curated index of public technical reference pages.
- Many pages or a whole site: one page per call; it never follows links or crawls.
- PDFs, images, audio, video, archives and other files: refused at 0 credits.
- reddit.com (refused as `FETCH_FAILED` at 0 credits), and pages behind a login or paywall.
- Sites that refuse automated requests or answer with an anti-bot challenge: the call ends as
  `SOURCE_REFUSED` at 0 credits. Do not retry it in a loop; when the error carries `Retry-After`,
  wait that long before one retry.

## Tools

| Tool | Use it to | Credits |
|---|---|---|
| `contrie_extract` | Get fields from a page: `url`, plus `extract` (plain language) and/or `schema` (JSON Schema for the exact shape) | 1, 3, 8 or 15 when accepted, +2 if rendered; 0 if not accepted |
| `contrie_read` | Get the page as markdown to read yourself | 1, +2 if rendered; 0 if the page could not be read |
| `contrie_search` | Find dated, citable snippets (`query`, optional `limit` 1-10) in Contrie's curated index of public technical reference pages (HTTP, robots.txt, MCP, bot verification). Not the open web; read a result's `url` in full with `contrie_read` | 1 with results; 0 with none |
| `contrie_verify` | Check `data` you already have against a `url` (or raw `html`/`text`) | 0 |
| `contrie_watch` | Re-run one extraction on a schedule (`every` minutes, min 15) and record changes | each run with an accepted answer is charged like an extraction |
| `contrie_changes`, `contrie_monitors`, `contrie_unwatch` | Read a monitor's history, list monitors, stop one | 0 |
| `contrie_account` | See your credits and any restrictions | 0 |

## Reading a result honestly

- Treat structured data as accepted only when `success` is true, `valid` is true and
  `qualityScore` is at least 70. A rejected result is not returned and is charged 0 credits.
- Acceptance also needs at least half of the returned values found in the page text. Yes/no
  values and strings under three characters cannot be found, so ask for the text the page shows
  ("the stock status as shown") rather than a boolean.
- `groundingFields` says, per value, whether its text was found in the page, with an excerpt.
  It checks presence, not meaning: it does not prove the value belongs to that field. When a
  value matters, read its excerpt before relying on it.
- A `null` means Contrie did not find that field on the page. Report it as unknown; do not fill
  it in.
- When `metadata.evidenceStatus` is `signed`, the response carries a receipt that proves who
  issued it and that it is unchanged, not that the page is true.
- Say where a value came from: cite the URL, and quote the excerpt when the user will act on it.
- `metadata.credits` is what the call cost.

## Good requests

- Pass a JSON Schema when the shape matters; ask only for fields the page actually shows.
- Name the unit or format you need in the description ("price as a number, currency as an ISO
  code"), rather than post-processing a guessed string.
- Use `contrie_read` when you need to understand the page, and `contrie_extract` when you need
  specific values.
- Stop monitors you no longer need with `contrie_unwatch`; each scheduled run with an accepted
  answer is charged.

## Setup

No API key is needed. The first tool call that needs an account asks you to sign in to Contrie
(OAuth); a Free account can be created during sign-in and includes up to 200 credits a month. In
Claude Code, open `/mcp` and choose the `contrie` server to sign in. Headless and CI use can send
an API key instead, created at https://www.contrie.com/dashboard/keys. Full docs:
https://www.contrie.com/docs/mcp.
