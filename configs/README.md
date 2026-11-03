# MCP client configs

Replace `YOUR_API_KEY` with a key from the [WA Validator dashboard](https://wavalidator.com/dashboard/).

| File | Client | Where it goes |
|---|---|---|
| [`claude_desktop_config.json`](claude_desktop_config.json) | Claude Desktop | Settings → Developer → Edit Config |
| [`cursor-mcp.json`](cursor-mcp.json) | Cursor | `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (global) |
| [`vscode-mcp.json`](vscode-mcp.json) | VS Code (GitHub Copilot agent mode) | `.vscode/mcp.json` |

For Claude Code, run:

```bash
claude mcp add --transport http wavalidator https://wavalidator.com/mcp/ \
  --header "Authorization: Bearer YOUR_API_KEY"
```
