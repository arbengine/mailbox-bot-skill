# Send Certified Mail, Letters, and Postcards from AI Agents

[![Website](https://img.shields.io/badge/Website-mailbox.bot-1D4ED8?style=flat)](https://mailbox.bot)
[![API Docs](https://img.shields.io/badge/API_Docs-api--docs-1D4ED8?style=flat)](https://mailbox.bot/api-docs)
[![MCP](https://img.shields.io/badge/MCP-22_tools-1D4ED8?style=flat)](https://mailbox.bot/mcp-install)
[![OpenAPI](https://img.shields.io/badge/OpenAPI-3.1-1D4ED8?style=flat)](https://mailbox.bot/api/openapi.json)
[![Sandbox](https://img.shields.io/badge/Sandbox-test_keys-1D4ED8?style=flat)](https://mailbox.bot/api-docs#sandbox)
[![License](https://img.shields.io/badge/License-Proprietary-gray?style=flat)]()
[![Status](https://img.shields.io/badge/Status-Outbound_Live-34d399?style=flat)]()

**Outbound print-and-mail is live now. Inbound CMRA-backed agent mailboxes are opening in controlled private beta.**

The physical mail API for AI agents and software workflows. Submit a PDF and an address — mailbox.bot prints, stuffs, stamps, and mails it via USPS, FedEx, or UPS. Certified mail, batch mailings up to 10,000 pieces, tracking, delivery proof, and webhook events.

```
POST a PDF → we print, stuff, stamp, mail → tracking + proof + webhooks
```

## Install

### MCP (Claude Desktop, Cursor, Cline)

Add to your MCP config:

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

Full MCP install guide: [mailbox.bot/mcp-install](https://mailbox.bot/mcp-install)

### OpenClaw

```bash
clawhub install mailbox-bot
```

### REST API

```bash
curl -X POST https://mailbox.bot/api/v1/mail \
  -H "Authorization: Bearer sk_agent_..." \
  -H "X-Mailbox-MD-Version: 1" \
  -F 'document=@letter.pdf' \
  -F 'recipient_name=Acme Corp' \
  -F 'recipient_line1=123 Main St' \
  -F 'recipient_city=Los Angeles' \
  -F 'recipient_state=CA' \
  -F 'recipient_zip=90001' \
  -F 'mail_class=certified'
```

## What's live now

- **Outbound mail** — submit a PDF, facility prints, stuffs, stamps, and mails it with photo proof
- **Certified mail** — USPS Certified, Certified + Return Receipt, Priority, First Class
- **FedEx and UPS** — zone-based rates for overnight, 2-day, ground
- **Batch mail** — send up to 10,000 pieces from a CSV, volume discounts at 500/1000/5000 pieces
- **Sandbox** — test keys (`sk_agent_test_`), dry runs, lifecycle simulation, zero charges
- **Webhook notifications** — HMAC-signed JSON payloads fire on every status transition
- **MAILBOX.md standing instructions** — configure rules for mail handling
- **Human-in-the-loop** — `requires_approval=true` pauses for human approval
- **Billing safeguards** — `X-Max-Cost-Cents` header, `dry_run=true`, daily spend caps

## Coming soon — inbound receiving address

Inbound CMRA-backed agent mailboxes are opening in controlled private beta. Activation requires identity verification, Form 1583 notarization, and facility approval. Southern California first, then Utah and Nevada.

- Real CMRA-licensed street address
- Every piece photographed, scanned, and classified on arrival
- Actions via API: scan, forward, hold, shred, dispose, return to sender
- Agent memory: tag and annotate mail with persistent notes and metadata

## Protocols

| Protocol | Endpoint | Details |
|----------|----------|---------|
| REST API | `https://mailbox.bot/api/v1` | Full CRUD, OpenAPI 3.1 spec |
| MCP | `https://mailbox.bot/api/mcp` | 22 tools, JSON-RPC 2.0 |
| A2A | `https://mailbox.bot/api/a2a` | Agent-to-agent task execution |
| OpenClaw | `https://mailbox.bot/.well-known/agent.json` | Multi-protocol agent card |

## Plans

| Plan | Price | Status | What you get |
|------|-------|--------|-------------|
| **Outbound Only** | $0/mo | **Live now** | Send letters via API. $0.30/page printing + actual carrier postage. |
| **Virtual Mailbox** | $5/mo | **Private beta** | Real CMRA address (SoCal first), inbound + outbound mail, scan on arrival. |

Outbound postage: USPS First Class from $0.78, Priority $11.95, Certified $6.08. Color printing +$0.25/page.

Full pricing: [mailbox.bot/pricing](https://mailbox.bot/pricing)

## Framework integrations

No SDK required — all integrations use the REST API via standard HTTP libraries.

- [LangChain](https://mailbox.bot/docs/langchain) (Python)
- [CrewAI](https://mailbox.bot/docs/crewai) (Python)
- [LlamaIndex](https://mailbox.bot/docs/llamaindex) (Python)
- [Vercel AI SDK](https://mailbox.bot/docs/vercel-ai-sdk) (TypeScript)
- [OpenAI Agents SDK](https://mailbox.bot/docs/openai-agents-sdk) (Python)

## Links

- [Website](https://mailbox.bot)
- [API Docs](https://mailbox.bot/api-docs)
- [Full API Reference (LLM-friendly)](https://mailbox.bot/llms-full.txt)
- [MCP Install Guide](https://mailbox.bot/mcp-install)
- [Sandbox & Test Keys](https://mailbox.bot/api-docs#sandbox)
- [OpenAPI Spec](https://mailbox.bot/api/openapi.json)
- [Agent Discovery](https://mailbox.bot/.well-known/agent.json)
- [Pricing](https://mailbox.bot/pricing)
- [Contact](https://mailbox.bot/contact)

## Publishing to ClawHub (for maintainers)

```bash
npm install -g clawhub
clawhub login
clawhub publish . \
  --slug mailbox-bot \
  --name "mailbox.bot" \
  --version 5.0.0 \
  --changelog "v5.0 — Outbound print-and-mail live. Certified mail, batch mail, sandbox test keys, 22 MCP tools. Inbound agent mailboxes in private beta."
clawhub info mailbox-bot
```

---

Questions? [mailbox.bot/contact](https://mailbox.bot/contact)
