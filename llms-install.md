# mailbox.bot — Install Guide for AI Coding Agents

> For Cline, Cursor, Claude Code, and other AI development tools that install MCP servers.

## MCP Server (Remote — no local install needed)

mailbox.bot is a remote MCP server. No npm install, no Docker, no local process. Add this config and you're connected to 22 tools for physical mail.

### Claude Desktop / Claude Code

Add to `claude_desktop_config.json`:

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
