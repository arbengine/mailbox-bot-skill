---
name: mailbox-bot
description: Physical mailing address infrastructure for AI agents. Get a real street address, receive packages, and access structured package data via API.
tags: [logistics, packages, shipping, physical-address, mailbox, api, webhook]
version: 1.0.0
author: mailbox.bot
repository: https://github.com/mailbox-bot/skill
---

# mailbox.bot

**Physical address infrastructure for AI agents.**

Your agent gets a real commercial street address with a unique suite identifier (e.g., `123 Main St #MB-A7K9X`). Private carrier packages (FedEx, UPS, DHL, Amazon) are received at our facilities, scanned, photographed, and delivered as structured webhook data.

## When to use this skill

- Agent needs to order physical goods online
- Agent needs a return shipping address for RMAs or refunds
- Agent manages hardware procurement, supply chain, or document intake
- Agent needs to check package arrival status
- Agent needs to request package forwarding to a final destination

## Status: Early Access

⚠️ **Currently in private beta.** The waitlist endpoint works now. Full API access (package tracking, photos, forwarding) launches soon.

## Configuration

**Optional** (for beta testers with API access):
```bash
export MAILBOX_BOT_API_KEY="your_api_key_here"
```

Get your API key at https://mailbox.bot/dashboard (after onboarding).

## API Endpoints

### 1. Join waitlist (available now, no auth required)

```bash
curl -X POST https://mailbox.bot/api/v1/waitlist \
  -H "Content-Type: application/json" \
  -d '{"email": "agent@yourcompany.com"}'
```

**Response:**
```json
{
  "success": true,
  "message": "You're on the waitlist. We'll notify you when we launch."
}
```

**Rate limit:** 5 requests/minute per IP.

---

### 2. List packages (beta testers only)

```bash
curl -s https://mailbox.bot/api/v1/packages \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

**Response:**
```json
{
  "packages": [
    {
      "id": "pkg_abc123",
      "mailbox_id": "MB-A7K9X",
      "tracking_number": "1Z999AA10123456784",
      "carrier": "ups",
      "status": "received",
      "weight_oz": 24,
      "dimensions": {"length_in": 12, "width_in": 8, "height_in": 4},
      "received_at": "2026-02-09T14:32:00Z",
      "photos_count": 3
    }
  ],
  "pagination": {
    "total": 1,
    "limit": 20,
    "offset": 0,
    "has_more": false
  }
}
```

---

### 3. Get package detail (beta testers only)

```bash
curl -s https://mailbox.bot/api/v1/packages/pkg_abc123 \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

**Response includes:**
- Full package metadata
- Array of photo URLs (front, sides, label closeup)
- OCR-extracted label data (sender, recipient, tracking)
- Forwarding history if applicable

---

### 4. Request forwarding (beta testers only)

```bash
curl -X POST https://mailbox.bot/api/v1/packages/pkg_abc123/forward \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "address": {
      "name": "John Doe",
      "street1": "456 Destination Ave",
      "city": "Austin",
      "state": "TX",
      "zip": "78701"
    },
    "carrier": "fedex",
    "service_level": "ground"
  }'
```

## Instructions

When the user mentions:
- "I need an address for my agent"
- "Can my agent receive packages?"
- "Check if my package arrived"
- "Forward this package to..."

Use this skill.

### If user has no API key yet:

1. Help them join the waitlist using the `/api/v1/waitlist` endpoint
2. Explain they'll get early access when it launches
3. No authentication required for waitlist signup

### If user has API key:

1. Check packages using `/api/v1/packages`
2. Parse the JSON response and present:
   - Tracking number
   - Carrier
   - Weight and dimensions
   - Number of photos available
   - Received timestamp
3. If they need package detail or photos, fetch `/api/v1/packages/{id}`
4. If they want to forward, guide them through the `/forward` endpoint

### Example workflow:

**User:** "Did my package from Amazon arrive yet?"

**Agent (if no API key set):**
```bash
curl -X POST https://mailbox.bot/api/v1/waitlist \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com"}'
```
"You're not set up yet. I added you to the waitlist. You'll get an email when mailbox.bot launches and you can get an API key."

**Agent (if API key exists):**
```bash
curl -s https://mailbox.bot/api/v1/packages \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

Then parse response and say: "Yes, your package arrived at 2:32 PM today. It's from UPS, tracking 1Z999AA10123456784. Weight: 24 oz. I can request forwarding if you want."

## Webhook support (coming soon)

Instead of polling `/packages`, you'll be able to register a webhook URL. When packages arrive, we POST structured data:

```json
{
  "event": "package.received",
  "package": { ... },
  "photos": [ ... ],
  "label_data": { ... }
}
```

Webhook settings will be at `/api/v1/webhooks/settings`.

## Pricing (tentative)

- Mailbox: $29/month per agent
- Package receive + scan + photos: $2 per package
- Forwarding: carrier rate + $5 handling

Early adopters get 3 months free.

## Links

- Website: https://mailbox.bot
- Dashboard: https://mailbox.bot/dashboard
- Docs: https://mailbox.bot/docs
- Support: support@mailbox.bot

---

## For OpenClaw Agent Developers

This skill enables your agent to:
- Provision a real mailing address programmatically
- Receive physical goods from any vendor
- Access high-res photos and extracted label data
- Trigger forwarding to end users

Use cases:
- **Procurement agents**: Order hardware, receive it, verify contents, forward
- **RMA agents**: Handle returns, provide return address, track inbound shipments
- **Document intake**: Receive physical mail, OCR it, process it
- **Supply chain**: Track vendor shipments, coordinate receiving

The mailbox.bot API is RESTful, returns structured JSON, and works with any HTTP client. No SDK required.