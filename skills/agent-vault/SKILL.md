---
name: agent-vault
description: Encrypted credential vault for AI agents on Anima — store, search, TOTP, and server-side use so the model never sees raw secrets. No cards/wallets/x402.
homepage: https://useanima.sh
docs: https://docs.useanima.sh
metadata: {"mcp": "https://mcp.useanima.sh", "api_base": "https://api.useanima.sh"}
---

# Agent Vault

Encrypted credential vault for AI agents on Anima. Store logins, API keys, OAuth tokens, certificates, secure notes, and identity data — then **use** them so the model never sees raw secrets.

Prefer **Anima MCP** tools when connected. Docs: https://docs.useanima.sh/vault.md (and related vault quickstart / server-side use / reveal policy pages on docs.useanima.sh).

## Use, never see

Prefer:

- Server-side use / brokered reveal
- Local injection (`vault exec` / `vault proxy`) with redaction
- Generate-on-store so plaintext never appears in API responses

Masked `get` for agents; humans reveal in console under audit when policy allows. There is no agent “unmask everything” path.

## Credential types (document these only)

| Type | Use case |
| --- | --- |
| `login` | Website / SSH logins (username, password, URIs, TOTP) |
| `api_key` | Provider and service API keys |
| `oauth_token` | OAuth access / refresh tokens |
| `certificate` | TLS / mTLS private keys and certs |
| `secure_note` | Free-form secret text |
| `identity` | Personal / business identity data |

**Do not** pitch payment card types, wallets, or x402 flows.

## CLI (optional, pinned)

```bash
npm install @anima-labs/cli@0.9.0 --save-exact
anima vault provision --agent <agent-id>
anima vault status --agent <agent-id>
anima vault list --agent <agent-id>
anima vault get <credential-id> --agent <agent-id>   # masked
anima vault use <credential-id> --agent <agent-id> ...
```

Never `npm install -g @anima-labs/cli` unpinned.

## Security

- Never store `ak_` / `ANIMA_API_KEY` in agent chat memory.
- Secrets persist until deleted; scope shares/tokens tightly (permission + TTL).
- Prefer brokered / use-only reveal for high-value secrets.
- NEVER send API keys off Anima hosts.

## Same identity

Email: `agent-email-inbox`. Phone: `agent-phone-sms`. Overview: `anima`.
