# Contrie plugin for Claude

Turn one public web page into the data you ask for: JSON with an acceptance decision and the page
excerpt behind each value, or clean, structure-preserving markdown. This plugin connects Claude to
Contrie's hosted MCP server (`https://www.contrie.com/mcp`) and adds a skill that tells Claude
when Contrie fits a task and how to read its results honestly.

## What you get

- **`contrie_extract`**: fields from a page, described in plain language or as a JSON Schema,
  with an acceptance decision, a quality score and the page excerpt behind each value. An answer
  that is not accepted is not returned and costs 0 credits.
- **`contrie_read`**: the page as markdown.
- **`contrie_search`**: snippets from Contrie's curated index of public technical reference pages,
  with URL, fetch time and content hash (1 credit with results, 0 without). Not the open web.
- **`contrie_verify`**: check whether values you already have appear in their source page (0 credits).
- **`contrie_watch`**, **`contrie_changes`**, **`contrie_monitors`**, **`contrie_unwatch`**:
  watch a page for a changed answer and manage those monitors.
- **`contrie_account`**: your credits and any current restrictions.

One page per call: Contrie is not a web search engine or a crawler, and it reads web pages, not PDFs
or other files. Its source check confirms that a value's text appears on the page, not that the
page states it about that field.

## Install

This plugin is not published yet. Install it in Claude Code from a local copy of this folder:

```
/plugin marketplace add /path/to/contrie-claude-plugin
/plugin install contrie@contrie
```

Then open `/mcp`, choose `contrie` and sign in with your Contrie account. No API key is needed; a
Free account can be created during sign-in and includes up to 200 credits a month. Pricing:
https://www.contrie.com/pricing. Documentation: https://www.contrie.com/docs/mcp.

## Privacy Policy

The plugin itself stores nothing. Tool calls go to Contrie's hosted service, which receives the
URL and request you send, fetches that page, and returns the result to your agent. How Contrie
collects, uses, stores, shares and retains data, and how to contact us, is described in the
Contrie Privacy Policy: https://www.contrie.com/privacy.

## Support

support@contrie.com · Security reports: security@contrie.com

## License

MIT
