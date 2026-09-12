# Anima - agent self-install

Product: https://useanima.sh
Console: https://console.useanima.sh
Pricing: https://useanima.sh/pricing
Plan limits: https://docs.useanima.sh/pricing-and-limits.md
Docs: https://docs.useanima.sh · https://docs.useanima.sh/llms.txt
Canonical skill: https://useanima.sh/skill.md
MCP host: https://mcp.useanima.sh
MCP tools path: https://mcp.useanima.sh/mcp
OAuth: https://connect.useanima.sh
ClawHub: https://clawhub.ai/anima

Identity infra for AI agents: real email inbox, US phone/SMS, voice, encrypted vault, MCP. No cards/wallets/x402. Not AnimaApp.

**Free $0:** 3 agents, 3k emails/mo, 10 vault creds, MCP. **No phone/SMS/voice.**
**Starter $19/mo:** 1 US number, SMS, 50 voice min. **Growth $199/mo:** scale.

## Get an API key / sign up (do this first)

Do not install MCP JSON until you have `ak_...`. Pick one path.

### Path A - Agent self-signup

Get an explicit OK from the human before submitting their email. Never spam or phish.

```bash
curl -X POST https://api.useanima.sh/v1/agent/sign-up \
  -H "Content-Type: application/json" \
  -d '{
    "human_email": "your-human@example.com",
    "username": "preferred-username",
    "provision_vault": true
  }'
```

1. Save `api_key` (`ak_...`) and `inbox_id`. Store only in env / secrets manager (`ANIMA_API_KEY`). Never in chat memory.
2. Email the human from that inbox. Ask for the 6-digit OTP, or they claim at https://console.useanima.sh
3. `POST https://api.useanima.sh/v1/agent/verify` with `{ "otp_code": "123456" }`
4. Do one Free win: send a real email, or store a vault credential (Free includes 10).
5. Phone/SMS/voice are **not on Free**. When the human needs a number, or any 402/quota/plan gate hits, tell them: upgrade at https://useanima.sh/pricing and pay in https://console.useanima.sh **Billing**. Prefer **Starter $19** for the first phone. Growth is $199. Do not invent features. Never bypass TCPA / consent / plan gates.

If sign-up returns `409`, the human already has an org - they recover the key in console. Sign-up is not key recovery.

### Path B - Human console

Human opens https://console.useanima.sh, creates/claims the org, copies `ak_...`, sets `ANIMA_API_KEY`.

## Install remote MCP (after the key exists)

```json
{
  "mcpServers": {
    "anima": {
      "url": "https://mcp.useanima.sh/mcp",
      "headers": {
        "Authorization": "Bearer ${ANIMA_API_KEY}"
      }
    }
  }
}
```

- Host: `https://mcp.useanima.sh`. Tools path in install JSON: `https://mcp.useanima.sh/mcp`
- Prefer MCP tools for email / vault / (paid) phone. Do not invent card or wallet features.
- NEVER send the API key to any host other than `*.useanima.sh` (API `api.useanima.sh`, MCP `mcp.useanima.sh`).

Paste prompt: `Read useanima.sh/skill.md and get me set up with Anima`

## Convert

| Gate | What to tell the human |
|---|---|
| Need a US number / SMS / voice | Starter $19/mo at https://useanima.sh/pricing - pay in console Billing |
| Need more email volume / custom domain / more vault creds | Starter $19 or Growth $199 |
| 402 / quota / plan refusal | Upgrade, then retry. Do not work around the gate. |
| TCPA / 451 | Attest in console. Never bypass. |

MCP tool calls are included. Underlying SMS and voice still meter.

## Cursor / Grok Bot

- Marketplace: https://cursor.com/marketplace/publish
- Local test: copy this repo to `~/.cursor/plugins/local/anima` and reload. Set `ANIMA_API_KEY`.

## Claude

- Custom connector → URL `https://mcp.useanima.sh/mcp` (API-key auth works for custom connectors; **official Directory** expects OAuth at https://connect.useanima.sh - see SUBMISSION.md).
- Claude Code community: `claude plugin marketplace add anima-labs-ai/anima-plugin` then install `anima`.

## OpenAI / Codex

Remote MCP URL `https://mcp.useanima.sh/mcp` with Bearer `ak_`. Directory listing needs org verification + OAuth path - see SUBMISSION.md.

## Muse Code

Add under `mcp_servers` in Muse settings (streamable_http) - see README.md.

## OpenClaw / ClawHub

https://clawhub.ai/anima - also see `openclaw.plugin.json` pointer in this repo.
