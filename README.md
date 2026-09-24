# Contrie plugin for Claude

Turn one public web page into the data you ask for: validated JSON with a per-field source check
and signed evidence, or clean, structure-preserving markdown. This plugin connects Claude to
Contrie's hosted MCP server (`https://www.contrie.com/mcp`) and adds a skill that tells Claude
when Contrie fits a task and how to read its results honestly.

## What you get

- **`contrie_extract`**: fields from a page, described in plain language or as a JSON Schema,
  with an acceptance decision, a quality score and the page excerpt behind each value.
- **`contrie_read`**: the page as markdown.
- **`contrie_verify`**: check data you already have against its source page (no credits).
- **`contrie_watch`**, **`contrie_changes`**, **`contrie_monitors`**, **`contrie_unwatch`**:
  watch a page for a changed answer and manage those monitors.
- **`contrie_account`**: remaining credits.

One page per call: Contrie is not a search engine or a crawler, and it reads web pages, not PDFs
or other files. Its source check confirms that a value appears on the page, not that the page
states it about that field.

## Install

In Claude Code, from the Claude plugin directory once listed, or from this repository:

```
/plugin marketplace add DaVinciPicks/contrie-claude-plugin
/plugin install contrie@contrie
```

Then open `/mcp`, choose `contrie` and sign in. A free Contrie account can be created during
sign-in and includes up to 200 credits a month. Pricing: https://www.contrie.com/pricing.
Documentation: https://www.contrie.com/docs/mcp.

## Privacy Policy

The plugin itself stores nothing. Tool calls go to Contrie's hosted service, which receives the
URL and request you send, fetches that page, and returns the result to your agent. How Contrie
collects, uses, stores, shares and retains data, and how to contact us, is described in the
Contrie Privacy Policy: https://www.contrie.com/privacy.

## Support

support@contrie.com · Security reports: security@contrie.com

## License

MIT
