# Changelog

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
- MCP (22 tools), A2A (9 skills), OpenClaw, REST protocols
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
