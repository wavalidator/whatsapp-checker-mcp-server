# WhatsApp Checker MCP Server — check WhatsApp numbers from Claude, ChatGPT and Cursor

[![MCP docs](https://img.shields.io/badge/docs-wavalidator.com%2Fmcp--docs-25D366)](https://wavalidator.com/mcp-docs/)
[![Transport](https://img.shields.io/badge/transport-Streamable%20HTTP-blueviolet)](https://modelcontextprotocol.io/)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

The **[WA Validator](https://wavalidator.com) MCP server** lets any [Model Context Protocol](https://modelcontextprotocol.io/) client check whether phone numbers are registered on WhatsApp — straight from the chat. Paste a list of leads into Claude and ask *"which of these have WhatsApp?"*: the assistant calls WA Validator and answers.

- **Server URL:** `https://wavalidator.com/mcp/`
- **Transport:** Streamable HTTP (JSON-RPC 2.0 over one POST endpoint) — hosted, nothing to install
- **Auth:** `Authorization: Bearer YOUR_API_KEY` — the same key as the [REST API](https://wavalidator.com/api-docs/)
- **Tools:** `check_whatsapp_number`, `bulk_check_whatsapp_numbers`

## Get an API key

[Create a free account](https://wavalidator.com/signup/), then generate a key in the [dashboard](https://wavalidator.com/dashboard/). Paste it only into your own client's config file — never into a public chat.

## Connect your AI client

### Claude Code

```bash
claude mcp add --transport http wavalidator https://wavalidator.com/mcp/ \
  --header "Authorization: Bearer YOUR_API_KEY"
```

### Claude Desktop

Settings → Developer → Edit Config, then add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "wavalidator": {
      "url": "https://wavalidator.com/mcp/",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

### Cursor

Add to `.cursor/mcp.json` in your project, or to the global `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "wavalidator": {
      "url": "https://wavalidator.com/mcp/",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

### VS Code, Windsurf and other MCP clients

Any client that supports Streamable HTTP with custom headers works with this shape:

```json
{
  "name": "wavalidator",
  "transport": "http",
  "url": "https://wavalidator.com/mcp/",
  "headers": { "Authorization": "Bearer YOUR_API_KEY" }
}
```

Ready-to-copy files are in [`configs/`](configs/).

## Tools

### `check_whatsapp_number`

Checks one phone number. **1 credit.**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `phone_number` | string | yes | International format with country code, e.g. `14155551234` |

### `bulk_check_whatsapp_numbers`

Checks up to 100 phone numbers in one call. **1 credit per number.**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `numbers` | array of strings | yes | Up to 100 numbers with country code |

Both tools return the same JSON as the [single](https://github.com/wavalidator/whatsapp-number-checker-api) and [bulk](https://github.com/wavalidator/whatsapp-bulk-checker-api) REST endpoints and share their credits and rate limits.

## Example prompts

- *"Check if +1 415 555 1234 has WhatsApp."*
- *"Here are 40 leads from my spreadsheet — tell me which ones have WhatsApp and give me the list as a table."*
- *"Remove every number without WhatsApp from this CSV and give me the cleaned file."*
- *"How many credits do I have left after that check?"*

## Test the server with cURL

```bash
curl -X POST https://wavalidator.com/mcp/ \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

```bash
curl -X POST https://wavalidator.com/mcp/ \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{"name":"check_whatsapp_number","arguments":{"phone_number":"14155551234"}}}'
```

## Errors

Tool errors — invalid key, no credits, rate limit — come back inside the tool result with `isError: true`, so the assistant can read the message and explain it instead of the connection failing silently.

| Error | Fix |
|---|---|
| Missing or invalid API key | Check the `Authorization` header in your client config. |
| Insufficient credits | [Top up](https://wavalidator.com/pricing/). |
| Rate limit reached | Wait a moment and ask the assistant to retry. Bulk calls also have hourly and daily limits. |

## FAQ

**Do I need to install anything?**
No. The server is hosted at `https://wavalidator.com/mcp/`; you only add the URL and your key to your client.

**Does it work with ChatGPT?**
Yes, in any ChatGPT surface that lets you add a remote MCP server with a custom `Authorization` header.

**Is it the same data as the API?**
Yes — the MCP tools call the same WhatsApp validation engine, credits and limits as the REST API.

## Related

- [WhatsApp Number Checker API](https://github.com/wavalidator/whatsapp-number-checker-api)
- [WhatsApp Bulk Checker API](https://github.com/wavalidator/whatsapp-bulk-checker-api)
- [MCP documentation on wavalidator.com](https://wavalidator.com/mcp-docs/)

---

WhatsApp is a trademark of Meta Platforms, Inc.; WA Validator is not affiliated with Meta. Claude is a trademark of Anthropic; ChatGPT of OpenAI; Cursor of Anysphere.
