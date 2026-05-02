---
name: mailbox-bot
description: The physical mail API for AI agents and software workflows. Send certified mail, letters, notices, and postcards from code. Outbound print-and-mail is live now. Inbound CMRA-backed agent mailboxes are opening in controlled private beta.
tags: [postal-mail, certified-mail, mail-api, ai-agent, mcp, outbound-mail, print-and-mail, webhooks, openclaw, a2a, agent-tools, openapi]
version: 5.0.0
author: mailbox.bot
repository: https://github.com/arbengine/mailbox-bot-skill
---

# mailbox.bot

**The physical mail API for AI agents and software workflows.**

**Outbound print-and-mail is live now.** Submit a PDF and an address — mailbox.bot prints it, stuffs the envelope, applies postage, and mails it via USPS, FedEx, or UPS. Certified mail, batch mailings up to 10,000 pieces, photo proof, tracking, and webhook events. REST, MCP, A2A, and OpenClaw all work today.

**Inbound CMRA-backed agent mailboxes are opening in controlled private beta.** Activation requires identity verification, Form 1583 notarization, and facility approval. Southern California first, then Utah and Nevada.

## Why this matters

Your agent can draft contracts, negotiate terms, and manage workflows — but the moment something requires physical mail, everything stalls. Courts require certified letters. Governments send time-sensitive notices to real addresses. LLCs need a registered address. Tax responses demand paper.

mailbox.bot closes that loop. Your agent now plays in the real world, not just the digital one.

### What it unlocks

- **LLC formation at scale** — receive stamped articles of organization, extract entity numbers, forward originals
- **Legal correspondence** — deadlines in letters recognized and tracked, responses drafted and sent certified
- **IRS and tax response** — IRS notices handled automatically, scanned, classified, routed to the right person
- **Real estate management** — each property gets a mailbox with tailored rules, code violations forwarded, lease renewals sent
- **Vendor and supplier communication** — invoices, purchase orders, and contracts scanned, filed, and responded to
- **Compliance and audit mail** — regulatory notices scanned, classified, and routed with full audit trail
- **Business registration** — registered agent address for LLCs, corps, DAOs — a real street address, not a P.O. Box
- **Agent-to-agent mail bridge** — one agent sends a certified letter, another agent receives and processes it

## What your agent gets

### Live now
- **Outbound mail** — submit a PDF, facility prints, stuffs, stamps, and mails it with photo proof
- **Certified mail** — USPS Certified, Certified + Return Receipt, Priority, First Class, FedEx, UPS
- **Batch mail** — send up to 10,000 pieces from a CSV, volume discounts at 500/1000/5000 pieces
- **Sandbox** — test keys (`sk_agent_test_`), dry runs, lifecycle simulation, zero charges
- **Webhook notifications** — HMAC-signed JSON payloads fire on every status transition
- **MAILBOX.md standing instructions** — configure rules for outbound mail handling
- **Human-in-the-loop** — `requires_approval=true` pauses for human approval on the dashboard
- **Multi-channel notifications** — webhooks, email, SMS, Slack, Discord
- **Billing safeguards** — `X-Max-Cost-Cents` header, `dry_run=true`, per-transaction ceiling, daily spend cap

### Private beta — inbound receiving address
Inbound CMRA-backed agent mailboxes are opening in controlled private beta. Southern California first, then Utah and Nevada.
- **Real mailing address** — CMRA-licensed street address
- **Inbound mail processing** — every piece photographed, scanned, and classified on arrival
- **Actions via API** — scan, forward, photograph, hold, shred, dispose, return to sender
- **Agent memory** — tag and annotate mail with persistent notes and metadata

## Plans

| Plan | Price | Status | What you get |
|------|-------|--------|-------------|
| **Outbound Only** | $0/mo | **Live now** | Send letters via API. $0.30/page printing + actual carrier postage. |
| **Virtual Mailbox** | $5/mo | **Private beta** | Real CMRA address (SoCal first), inbound + outbound mail, scan on arrival. |

Outbound postage (both plans): USPS First Class from $0.78, Priority $11.95 flat rate, Certified $6.08, FedEx/UPS zone-based. Color printing +$0.25/page.

Actions billed per use: scan $3, open & scan $5, photograph $1.50, forward $5 + $2/lb, shred $2, dispose $1, return $5, hold free.

All plans month-to-month, cancel anytime. Full pricing: https://mailbox.bot/pricing

## How to get started

### If you have an API key

Set your environment variables and use the API directly:

```bash
export MAILBOX_BOT_API_KEY="sk_live_xxxxxxxxxxxxx"
export MAILBOX_BOT_URL="https://mailbox.bot"
```

Skip to the **API Reference** section below.

### If you do NOT have an API key — sign up your operator

Create an account on behalf of your human operator. No CAPTCHA, no auth header.

**POST to `https://mailbox.bot/api/v1/signup`:**

```bash
curl -X POST https://mailbox.bot/api/v1/signup \
  -H "Content-Type: application/json" \
  -d '{
    "full_name": "Jane Smith",
    "email": "operator@example.com",
    "password": "securepassword123",
    "needs": "inbound mail scanning + outbound certified letters for legal compliance"
  }'
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `full_name` | string | Yes | Operator's legal name (2-100 chars) |
| `email` | string | Yes | Operator's email — verification link sent here |
| `password` | string | Yes | Min 8 characters |
| `needs` | string | No | Free text — what the agent/operator needs |

Rate limit: 5 requests per minute per IP.

**Success response (201):**

```json
{
  "success": true,
  "message": "Account created. A verification email has been sent. The operator must verify their email and complete KYC to activate the account.",
  "next_steps": {
    "verify_email": "Click the verification link sent to the operator's email",
    "complete_kyc": "https://mailbox.bot/signup",
    "after_kyc": "Select a plan, add payment, and create your first agent to get API keys"
  }
}
```

**Error responses:**

| Status | Error | Cause |
|--------|-------|-------|
| 400 | `"email, password, and full_name are required"` | Missing a required field |
| 400 | `"Password must be at least 8 characters"` | Password too short |
| 400 | `"Invalid email address"` | Bad email format |
| 409 | `"An account with this email already exists"` | Duplicate email |
| 429 | Rate limit exceeded | Too many requests from this IP |

**After signup — tell your operator:**

> "I created your mailbox.bot account. Check your email at **operator@example.com** for a verification link. Click it, then go to https://mailbox.bot/signup to finish setup (~5 minutes). Once you're done, I'll have API access to manage your mail."

The human must complete these steps in a browser:
1. Click the email verification link
2. Verify phone number (carrier check — no VoIP/burner phones)
3. Add a payment card (no prepaid/gift cards)
4. Complete Form 1583 notarization (virtual mailbox only — ~5 min video call, $25)
5. Accept Terms of Service and select a plan

After the human finishes, an agent and mailbox are auto-provisioned. API keys appear on the dashboard at https://mailbox.bot/dashboard.

## Protocols

Your agent can connect via any of these:

| Protocol | Endpoint | Details |
|----------|----------|---------|
| REST API (v1) | `https://mailbox.bot/api/v1` | Full CRUD for agents, mailboxes, mail, actions, rules, webhooks |
| MCP | `https://mailbox.bot/api/mcp` | 22 tools for LLM integration (JSON-RPC 2.0, spec 2025-11-25) |
| A2A | `https://mailbox.bot/api/a2a` | 9 skills for agent-to-agent task execution (v0.3) |
| OpenClaw | `https://mailbox.bot/.well-known/agent.json` | Multi-protocol agent card, WebSocket gateway + webhooks |

### MCP setup (Claude Desktop)

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "mailbox-bot": {
      "url": "https://mailbox.bot/api/mcp",
      "headers": { "Authorization": "Bearer sk_live_xxxxxxxxxxxxx" }
    }
  }
}
```

Full MCP install guide: https://mailbox.bot/mcp-install

## MAILBOX.md — standing instructions

Drop a `MAILBOX.md` file into your agent's context. Your agent reads it, calls the API, and configures your standing instructions as rules. Mail arrives, rules evaluate, actions fire — automatically.

Write "needs approval" next to any rule and the action pauses until you approve on the dashboard.

```markdown
# Mail Instructions

You manage my business mail at mailbox.bot
Ref **MB-6E1A**

## Inbound mail

- Legal notices, contracts → scan + email me
- IRS / state agencies → **Requires approval** before any action
- Junk mail → discard

## Outbound mail

- Signed contracts → send USPS Certified
- Legal notices → send USPS First Class
- Anything over $50 postage → **Needs approval**

## Notifications

- Email for everything
- **Needs approval** → email + SMS with dashboard link
```

Integration guide: https://mailbox.bot/implementation

---

## API Reference

Base URL: `https://mailbox.bot`

All authenticated endpoints require:
```
Authorization: Bearer $MAILBOX_BOT_API_KEY
```

Two key types:
- **Member keys** (`sk_live_`) — full account access, all scopes
- **Agent keys** (`sk_agent_`) — scoped to a single agent

### Get mailbox address

```bash
curl -s "$MAILBOX_BOT_URL/api/v1/mailboxes" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

Returns the physical mailing address your agent can use immediately.

### List mail and packages

```bash
curl -s "$MAILBOX_BOT_URL/api/v1/packages?status=received" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

Filters: `status`, `carrier`, `since`, `before`, `limit`, `offset`

### Get item details

```bash
curl -s "$MAILBOX_BOT_URL/api/v1/packages/{id}" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

### Request an action

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{id}/actions" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"action": "scan", "priority": "normal"}'
```

Action types: `scan`, `open_and_scan`, `photograph`, `forward`, `shred`, `dispose`, `return_to_sender`, `hold`

### Forward mail

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{id}/forward" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "to_name": "John Doe",
    "to_line1": "123 Main Street",
    "to_city": "New York",
    "to_state": "NY",
    "to_zip": "10001"
  }'
```

### Request a scan

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{id}/scan" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"scan_type": "document"}'
```

Scan types: `label`, `envelope`, `document`, `content`

### Send outbound mail

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/mail" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "X-Mailbox-MD-Version: 1" \
  -F 'document=@letter.pdf' \
  -F 'recipient_name=Acme Corp' \
  -F 'recipient_line1=123 Main St' \
  -F 'recipient_city=Los Angeles' \
  -F 'recipient_state=CA' \
  -F 'recipient_zip=90001' \
  -F 'return_name=My Company LLC' \
  -F 'return_line1=100 Main St' \
  -F 'return_city=Austin' \
  -F 'return_state=TX' \
  -F 'return_zip=78701' \
  -F 'mail_class=first_class'
```

Return address fields (`return_name`, `return_line1`, `return_line2`, `return_city`, `return_state`, `return_zip`) are optional — if omitted, the member's saved return address from their profile is used.

### Webhook settings

```bash
curl -X PUT "$MAILBOX_BOT_URL/api/v1/webhooks/settings" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "agent_id": "YOUR_AGENT_ID",
    "webhook_url": "https://your-agent.com/webhooks",
    "event_types": ["mail.received", "action.completed", "mail.forwarded"]
  }'
```

### Add notes and tags

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{id}/notes" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"note": "IRS notice — response deadline April 15"}'
```

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{id}/tags" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"tag": "tax-notice"}'
```

### Check usage

```bash
curl -s "$MAILBOX_BOT_URL/api/v1/usage" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

Full API reference: https://mailbox.bot/llms-full.txt

---

## When to use this skill

Trigger when user says:
- "My agent needs to send a letter"
- "Can my agent send certified mail?"
- "I need to mail a PDF via API"
- "I need my agent to handle legal correspondence"
- "How do I send physical mail programmatically?"
- "Can my agent send batch mailings?"
- "I need a mailing address for my agent" — note: inbound addresses in private beta; outbound-only is live now
- "Can my agent receive mail?" — note: inbound receiving in private beta (SoCal first, then UT/NV)

**Action flow:**
1. Check for API key (`MAILBOX_BOT_API_KEY`). If present, use the API directly.
2. If no API key, **create an account for the operator** by POSTing to `https://mailbox.bot/api/v1/signup`.
3. Tell the operator to check their email, click the verification link, and complete onboarding at https://mailbox.bot/signup (~5 minutes).
4. Once onboarding is done, set the API key and start managing mail.

---

## Decision framework

When checking the mailbox:

1. **List received mail** — `GET /api/v1/packages?status=received`
2. **Triage by urgency** — government agency / legal notice → scan immediately. Known sender → check standing instructions. Junk mail → discard.
3. **Check MAILBOX.md rules** — standing instructions may already cover this piece of mail. If a rule requires approval, notify the human and wait.
4. **Take action** — scan, forward, hold, shred, dispose, or return. Always add a note explaining why.
5. **Report back** — summarize what arrived and what you did.

When sending outbound mail:

1. **Prepare the PDF** — render the document your agent needs to send.
2. **Choose mail class** — First Class for routine, Certified for legal, Priority for urgent.
3. **Submit via API** — the facility prints, stuffs, stamps, and mails it. Photo proof included.
4. **Track delivery** — webhook events fire at each status transition.

---

## Configuration

```bash
export MAILBOX_BOT_API_KEY="sk_live_xxxxxxxxxxxxx"
export MAILBOX_BOT_URL="https://mailbox.bot"
```

## Links

- Website: https://mailbox.bot
- Full API reference: https://mailbox.bot/llms-full.txt
- API docs: https://mailbox.bot/api-docs
- Pricing: https://mailbox.bot/pricing
- MCP install: https://mailbox.bot/mcp-install
- Integration guide: https://mailbox.bot/implementation
- Agent discovery: https://mailbox.bot/.well-known/agent.json
- Blog: https://mailbox.bot/blog
- Contact: https://mailbox.bot/contact
