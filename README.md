# Contrie Plugin for Claude and Codex

Contrie turns one public web page into the data you ask for. You get JSON with an acceptance
decision and the page excerpt behind each value, or the page as clean markdown that keeps its
structure. This repo connects Claude and Codex to Contrie's hosted MCP server
(`https://www.contrie.com/mcp`). It works three ways: as a Claude Code plugin, as a Claude
connector, and as a Codex connector. The Claude Code plugin also adds a skill that tells Claude
when Contrie fits a task and how to read its results honestly.

## What you get

- **`contrie_extract`**: fields from a page, described in plain language or as a JSON Schema,
  with an acceptance decision, a quality score and the page excerpt behind each value. An answer
  that is not accepted is not returned and costs 0 credits.
- **`contrie_read`**: the page as markdown.
- **`contrie_search`**: snippets from Contrie's curated index of public technical reference pages,
  with URL, fetch time and content hash (1 credit with results, 0 without). It does not search
  the open web.
- **`contrie_verify`**: check whether values you already have appear in their source page (0 credits).
- **`contrie_watch`**, **`contrie_changes`**, **`contrie_monitors`**, **`contrie_unwatch`**:
  watch a page for a changed answer and manage those monitors.
- **`contrie_account`**: your credits and any current restrictions.

One page per call: Contrie is not a web search engine or a crawler, and it reads web pages, not PDFs
or other files. Its source check confirms that a value's text appears on the page, not that the
page states it about that field.

## Install

### Claude Code (plugin)

```
/plugin marketplace add DaVinciPicks/Contrie-Plugin
/plugin install contrie@contrie
```

Then open `/mcp`, choose `contrie` and sign in with your Contrie account.

### Claude (connector)

In claude.ai, go to Customize, Connectors, Add, Add custom connector, and enter
`https://www.contrie.com/mcp`. Sign in when Claude asks. This adds the tools without the skill.

### Codex (connector)

```bash
codex mcp add contrie --url https://www.contrie.com/mcp
codex mcp login contrie --scopes contrie:account,offline_access
```

These commands were checked against Codex CLI 0.157.1. If you open this folder in Codex, the
included `.codex/config.toml` already points at the server, so only the login step is needed.

### Any client

No API key is needed for any of these. Sign-in is OAuth, and a Free account can be created during
sign-in with up to 200 credits a month. Clients with no browser, such as CI, send an API key from
the [dashboard](https://www.contrie.com/dashboard/keys) as an `Authorization: Bearer` header.

To check the connection, ask for `contrie_account` (0 credits), then try `contrie_read` on
`https://example.com/` (1 credit on success).

Pricing: https://www.contrie.com/pricing. Documentation: https://www.contrie.com/docs/mcp.

## Privacy Policy

The plugin itself stores nothing. Tool calls go to Contrie's hosted service, which receives the
URL and request you send, fetches that page, and returns the result to your agent. How Contrie
collects, uses, stores, shares and retains data, and how to contact us, is described in the
Contrie Privacy Policy: https://www.contrie.com/privacy.

## Support

support@contrie.com · Security reports: security@contrie.com

## License

MIT
