---
name: anima
description: Give your AI agent an identity it owns — real email inbox, US phone/SMS and voice, encrypted vault — via Anima MCP or API. Free tier for email/vault/MCP; no cards/wallets/x402.
homepage: https://useanima.sh
docs: https://docs.useanima.sh
metadata: {"mcp": "https://mcp.useanima.sh", "api_base": "https://api.useanima.sh"}
---

# Anima

Identity infrastructure for AI agents: **email**, **US phone/SMS**, **voice**, **encrypted vault**, **MCP**.

- Product: https://useanima.sh
- Docs: https://docs.useanima.sh · https://docs.useanima.sh/llms.txt
- Console: https://console.useanima.sh (API keys `ak_...`)
- MCP: https://mcp.useanima.sh
- ClawHub: https://clawhub.ai/anima

**Not in scope:** payment cards, wallets, or x402 flows.

## Prefer MCP

When Anima MCP is connected (`Authorization: Bearer $ANIMA_API_KEY`), use MCP tools for inbox, phone/SMS, voice, and vault instead of inventing REST calls. Fall back to HTTPS API `https://api.useanima.sh` only when MCP is unavailable.

## Auth

```
Authorization: Bearer ak_...
```

- Keys come from console.useanima.sh or agent sign-up flows documented at docs.useanima.sh.
- Store only in env / secrets manager / client variable UI (`ANIMA_API_KEY`). Never in chat memory, skill notes, or transcripts.
- NEVER send the key to any host other than Anima (`api.useanima.sh`, `mcp.useanima.sh`).

## Surfaces (same identity)

| Surface | Skill | Notes |
|---|---|---|
| Email inbox | `agent-email-inbox` | Real address the agent owns; replies land with the agent |
| US phone / SMS / voice | `agent-phone-sms` | Geographic US lines; phone/voice often plan-gated |
| Encrypted vault | `agent-vault` | Prefer use-never-see; no card/wallet types |

## Agent-friendly rules

1. Get explicit human OK before submitting their email to sign-up / verification flows.
2. Until verified, restrict outbound email to the human as product rules require.
3. Do not spam, phish, or impersonate humans.
4. Only call/SMS numbers with required consent; voice has server-side TCPA gates — refusals are expected, not bugs to bypass.
5. Prefer message ids and short summaries over pasting full email/SMS/transcripts into long-lived memory.

## Related install

See repo `llms-install.md` and `README.md` for Cursor, Claude, Codex, Muse, and registry install blocks.
