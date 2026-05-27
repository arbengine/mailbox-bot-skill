# mailbox.bot — Install Guide for AI Coding Agents

> For MCP-capable AI clients such as Cline, Cursor, Claude Code, Claude Desktop, and other development tools.

## MCP Server (Remote — no local install needed)

mailbox.bot is a remote MCP server. For clients that support remote HTTP MCP servers, no npm install, Docker, or local process is required. Add this config and you're connected to 29 tools for outbound mail plus inbound document context.

### Generic remote HTTP config

Add to your MCP client config:

```json
{
  "mcpServers": {
    "mailbox-bot": {
      "url": "https://mailbox.bot/api/mcp",
      "headers": { "Authorization": "Bearer sk_agent_..." }
    }
  }
}
```

### Command bridge config

For clients that expect a local command, bridge the same remote server with `mcp-remote`:

```json
{
  "mcpServers": {
    "mailbox-bot": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mailbox.bot/api/mcp",
        "--header",
        "Authorization: Bearer sk_agent_..."
      ]
    }
  }
}
```

### Cursor

Add to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "mailbox-bot": {
      "url": "https://mailbox.bot/api/mcp",
      "headers": { "Authorization": "Bearer sk_agent_..." }
    }
  }
}
```

### Cline

Add to Cline MCP settings:

```json
{
  "mcpServers": {
    "mailbox-bot": {
      "url": "https://mailbox.bot/api/mcp",
      "headers": { "Authorization": "Bearer sk_agent_..." }
    }
  }
}
```

## Get an API Key

1. Sign up at https://mailbox.bot/#signup
2. Create an agent in the dashboard
3. Generate an agent-scoped API key (`sk_agent_...`)
4. Use a test key (`sk_agent_test_...`) for sandbox — no charges, same endpoints

## What You Get

22 MCP tools for physical mail:

- **send_outbound_mail** — print and mail a PDF, DOCX, image, TXT, or CSV
- **check_mailbox** — list received mail
- **request_action** — scan, forward, shred, hold, dispose, return
- **get_scan_results** — retrieve OCR text from scanned documents
- **get_mailbox_md** — fetch standing instructions
- And 17 more (tags, notes, rules, expected shipments, usage, webhooks, batch mail)

Full tool catalog: https://mailbox.bot/api/mcp/tools-public

## Links

- Install guide: https://mailbox.bot/mcp-install
- API docs: https://mailbox.bot/api-docs
- Full API reference: https://mailbox.bot/llms-full.txt
- Sandbox: https://mailbox.bot/api-docs#sandbox
