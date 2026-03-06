---
name: mailbox-bot
description: Provision a physical shipping address for your AI agent. Receive packages from FedEx, UPS, DHL, Amazon. Scan, webhook, store, forward — a fulfillment node your agent can call like any API.
tags: [logistics, packages, shipping, fulfillment, warehouse, api, webhook, agents]
version: 3.0.0
author: mailbox.bot
repository: https://github.com/arbengine/mailbox-bot-skill
---

# mailbox.bot

**Provision a physical shipping address for your AI agent.**

Receive packages from FedEx, UPS, DHL, Amazon, and other private carriers. Scan, photograph, forward, store — your agent controls a real warehouse facility via API.

## What your agent gets

- **Physical shipping address** — unique reference code at a warehouse facility, usable for shipping within seconds of provisioning
- **Package documentation** — photos, weight, dimensions, carrier info, OCR label extraction on every arrival
- **Webhook notifications** — HMAC-signed JSON payloads fire the moment a package arrives
- **Actions via API** — forward, scan, photograph, hold, shred, dispose, return to sender
- **Standing instructions** — auto-trigger actions based on carrier, weight, sender, tracking patterns
- **Agent memory** — tag and annotate packages with persistent notes and metadata

## How to get started

### If you have an API key

Set your environment variables and use the API directly:

```bash
export MAILBOX_BOT_API_KEY="sk_live_xxxxxxxxxxxxx"
export MAILBOX_BOT_URL="https://mailbox.bot"
```

Skip to the **API Reference** section below.

### If you do NOT have an API key — sign up your operator

You can create an account on behalf of your human operator. This is the same as filling out the signup form at https://mailbox.bot/staging — but via API so you don't need a browser.

The signup form has three fields:

| Field | Label | Placeholder | Rules |
|-------|-------|-------------|-------|
| `full_name` | Full Name | Jane Smith | Required, 2-100 characters, operator's real legal name |
| `email` | Email | you@company.com | Required, valid email — the verification link is sent here |
| `password` | Password | At least 8 characters | Required, minimum 8 characters |

There is no CAPTCHA, no TOS checkbox, and no other fields at this step. Just these three.

**POST to `https://mailbox.bot/api/v1/signup`:**

```bash
curl -X POST https://mailbox.bot/api/v1/signup \
  -H "Content-Type: application/json" \
  -d '{
    "full_name": "Jane Smith",
    "email": "operator@example.com",
    "password": "securepassword123",
    "needs": "receive hardware shipments, ~20 packages/month"
  }'
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `full_name` | string | Yes | Operator's legal name (2-100 chars) |
| `email` | string | Yes | Operator's email — verification link sent here |
| `password` | string | Yes | Min 8 characters |
| `needs` | string | No | Free text — what the agent/operator needs |

No authentication header required. No CAPTCHA. Rate limit: 5 requests per minute per IP.

**What happens when you POST:**

1. Account is created immediately
2. A verification email is sent to the operator's email address automatically
3. The operator's account is set to `onboarding_step: "signup"` (waiting for email verification)

**Success response (201):**

```json
{
  "success": true,
  "message": "Account created. A verification email has been sent. The operator must verify their email and complete KYC to activate the account.",
  "next_steps": {
    "verify_email": "Click the verification link sent to the operator's email",
    "complete_kyc": "https://mailbox.bot/staging",
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

> "I created your mailbox.bot account. Check your email at **operator@example.com** for a verification link. Click it, then go to https://mailbox.bot/staging to finish setup (~5 minutes). Once you're done, I'll have API access to manage your packages."

The human must complete these steps in a browser (the agent cannot do these):
1. Click the email verification link
2. Verify phone number (carrier check — no VoIP/burner phones)
3. Select service type (virtual mailbox or warehouse)
4. Add a payment card (no prepaid/gift cards)
5. Complete identity verification (Stripe Identity or notary session)
6. Accept Terms of Service
7. Select a plan and pay

After the human finishes, an agent and mailbox are auto-provisioned. The operator's dashboard at https://mailbox.bot/dashboard shows the API keys.

**Once you have the API key, set it and start managing packages:**

```bash
curl -s https://mailbox.bot/api/v1/mailboxes \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" | jq .
```

This returns the physical shipping address your agent can use immediately.

---

## API Reference

Base URL: `https://mailbox.bot`

All authenticated endpoints require:
```
Authorization: Bearer $MAILBOX_BOT_API_KEY
```

### List packages

```bash
curl -s "$MAILBOX_BOT_URL/api/v1/packages?status=received" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

Filters: `status`, `carrier`, `since`, `before`, `limit`, `offset`

### Get package details

```bash
curl -s "$MAILBOX_BOT_URL/api/v1/packages/{package_id}" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

### Request an action

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{package_id}/actions" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"action": "scan", "priority": "normal"}'
```

Action types: `scan`, `open_and_scan`, `photograph`, `record_video`, `forward`, `shred`, `dispose`, `return_to_sender`, `hold`

### Forward a package

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{package_id}/forward" \
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
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{package_id}/scan" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"scan_type": "document"}'
```

Scan types: `label`, `envelope`, `document`, `content`

### Get mailbox address

```bash
curl -s "$MAILBOX_BOT_URL/api/v1/mailboxes" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

### Webhook settings

```bash
curl -X PUT "$MAILBOX_BOT_URL/api/v1/webhooks/settings" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "agent_id": "YOUR_AGENT_ID",
    "webhook_url": "https://your-agent.com/webhooks",
    "event_types": ["package.received", "action.completed"]
  }'
```

### Add notes and tags

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{package_id}/notes" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"note": "Contains Mouser order #12345"}'
```

```bash
curl -X POST "$MAILBOX_BOT_URL/api/v1/packages/{package_id}/tags" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"tag": "hardware-order"}'
```

### Check usage

```bash
curl -s "$MAILBOX_BOT_URL/api/v1/usage" \
  -H "Authorization: Bearer $MAILBOX_BOT_API_KEY"
```

---

## When to use this skill

Trigger when user says:
- "I need a shipping address for my agent"
- "Can my agent receive packages?"
- "How do I receive packages via API?"
- "My agent needs to order hardware"
- "Can my agent handle returns?"
- "I need a fulfillment node"
- "How do I get webhook notifications for package arrivals?"
- "Can my agent forward packages automatically?"

**Action flow:**
1. Check for API key (`MAILBOX_BOT_API_KEY`). If present, use the API directly.
2. If no API key, **create an account for the operator** by POSTing to `https://mailbox.bot/api/v1/signup` with their email, a password, and their full name.
3. Tell the operator to check their email, click the verification link, and complete onboarding at https://mailbox.bot/staging (~5 minutes).
4. Once onboarding is done, set the API key and start managing packages.

---

## Decision framework

When checking the mailbox:

1. **List received packages** — `GET /api/v1/packages?status=received`
2. **Triage by urgency** — FedEx Express / marked URGENT → scan label immediately. Heavy boxes → likely needs forwarding. Letters → scan contents.
3. **Check rules** — expected shipments and standing instructions may already cover this package.
4. **Take action** — forward, scan, hold, shred, dispose, or return. Always add a note explaining why.
5. **Report back** — summarize what arrived and what you did.

---

## Configuration

```bash
export MAILBOX_BOT_API_KEY="sk_live_xxxxxxxxxxxxx"
export MAILBOX_BOT_URL="https://mailbox.bot"
```

Full API reference: https://mailbox.bot/llms-full.txt
Agent discovery: https://mailbox.bot/.well-known/agent.json
Pricing: https://mailbox.bot/pricing
Contact: founders@mailbox.bot
