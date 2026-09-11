---
name: agent-email-inbox
description: Give your AI agent its own real email inbox — an address it owns, that sends and receives, with replies threading back to the agent. Use with Anima MCP or API when the agent needs mail as itself.
homepage: https://useanima.sh
docs: https://docs.useanima.sh
metadata: {"mcp": "https://mcp.useanima.sh", "api_base": "https://api.useanima.sh"}
---

# Agent Email Inbox

Most agents send from a human account or shared `noreply@`. Replies then land behind a human filter. An Anima identity owns a **real address**; mail addressed to the agent arrives at the agent.

Prefer **Anima MCP** tools when connected. Otherwise use API/CLI against `https://api.useanima.sh` with `Authorization: Bearer $ANIMA_API_KEY`.

## Capabilities (product)

- Provision / use an agent inbox (e.g. `{username}@agents.useanima.sh` or custom domain on plan).
- Send and receive email; thread with proper `In-Reply-To` / `References` when the API supports it.
- List, get, and search messages; react to inbound via webhooks where configured.
- Full send to arbitrary recipients unlocks after **human verification** (OTP / console claim) — that gate is deliberate.

## CLI (optional, pinned)

```bash
npm install @anima-labs/cli@0.9.0 --save-exact
npx anima init
anima verify <code>
anima email send --agent <agent-id> --to someone@example.com --subject "..." --body "..."
anima email list --agent <agent-id>
anima email get <message-id>
```

Pin CLI **0.9.0**; prefer local install + lockfile. Do not run unpinned global installs.

## Security

- Store API keys in env / secrets manager / SecretRef — never chat memory.
- Prefer message ids and short summaries over full bodies in durable memory.
- Verify webhook signatures before trusting payloads.
- NEVER send your API key off Anima hosts (`api.useanima.sh` / `mcp.useanima.sh`).

## Enforced product behaviors (do not work around)

- Suppression / opt-out and bounce lists.
- Loop protection (`Auto-Submitted` / velocity breaker where documented).
- Own-address refusal.

## Same identity

Phone/SMS/voice: `agent-phone-sms`. Vault: `agent-vault`. Overview: `anima`.

Docs: https://docs.useanima.sh
