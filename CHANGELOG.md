# Changelog

## [5.1.2] - 2026-06-05

### Changed
- Refreshed ClawHub summary metadata so the registry hero matches the current skill positioning

## [5.1.1] - 2026-06-05

### Changed
- Fixed stale ClawHub-facing pricing and onboarding language in `SKILL.md`
- Clarified that direct signup requires explicit operator consent and that `auth.md` is the preferred guarded registration path
- Updated OpenClaw install and ClawHub publish instructions for the v5.1.1 package

## [5.1.0] - 2026-05-27

### Changed
- Reframed the skill around two live workflows: outbound physical mail and forwarded inbound document context
- Updated README and SKILL.md to distinguish live inbound context from managed receiving / CMRA private beta
- Fixed MCP install docs to use current tool names such as `list_inbound_forwarding_addresses`, `list_inbound_mail`, and `get_inbound_mail`
- Updated outbound examples to `X-Mailbox-MD-Version: 3` and canonical OpenAPI links to `https://mailbox.bot/openapi.json`
- Refreshed marketplace metadata descriptions and tags to mention inbound context and postal threads

## [5.0.0] - 2026-05-01

### Changed
- Repositioned to outbound-first: "The physical mail API for AI agents and software workflows"
- Updated README with badges, install-first layout, and sandbox documentation
- Updated SKILL.md frontmatter with new tags and description
- Inbound mailbox status clarified as "controlled private beta" (not "coming soon")

### Added
- `llms-install.md` — install guide for Cline, Cursor, Claude Code
- `server.json` — standardized MCP server metadata for directory submissions
- `smithery.yaml` — Smithery marketplace metadata
- `CHANGELOG.md` — this file
- Sandbox documentation (test keys, dry runs, lifecycle simulation)
- Billing safeguards documentation (X-Max-Cost-Cents, dry_run, spend caps)

## [4.0.0] - 2026-04-15

### Added
- SKILL.md with full API reference, decision framework, and configuration
- Outbound mail: send letters, certified mail, batch mailings via API
- MAILBOX.md standing instructions with human-in-the-loop approval gates
- MCP (29 tools), A2A, OpenClaw, REST protocols
- Webhook notifications with HMAC-SHA256 signing
- Multi-channel notifications: email, SMS, Slack, Discord

### Changed
- README updated with ClawHub publishing instructions

## [3.0.0] - 2026-03-20

### Added
- Initial OpenClaw skill with inbound package management
- Action system: scan, forward, shred, hold, dispose, return
- Agent rules and expected shipment matching
- Package tagging and notes
