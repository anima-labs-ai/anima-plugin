# Anima — agent self-install

Product: https://useanima.sh  
MCP: https://mcp.useanima.sh  
Console (API keys `ak_`): https://console.useanima.sh  
Docs: https://docs.useanima.sh · https://docs.useanima.sh/llms.txt  
ClawHub: https://clawhub.ai/anima

## What Anima is

Identity infra for AI agents: real email inbox, US phone/SMS, voice, encrypted vault, MCP. No cards/wallets/x402.

## Install remote MCP (Bearer key)

1. Human creates/claims org and copies an agent API key (`ak_...`) from https://console.useanima.sh
2. Set env: `export ANIMA_API_KEY=ak_...`
3. Connect Streamable HTTP MCP:

```json
{
  "mcpServers": {
    "anima": {
      "url": "https://mcp.useanima.sh",
      "headers": {
        "Authorization": "Bearer ${ANIMA_API_KEY}"
      }
    }
  }
}
```

4. Prefer MCP tools for email / phone / vault. Do not invent card or wallet features.
5. Never put `ak_` keys in chat memory, skill notes, or transcripts — env / secrets manager only.
6. NEVER send the API key to any host other than `*.useanima.sh` (API `api.useanima.sh`, MCP `mcp.useanima.sh`).

## Cursor / Grok Bot

- Marketplace submit: https://cursor.com/marketplace/publish
- Local test: copy this repo to `~/.cursor/plugins/local/anima` and reload.

## Claude

- Custom connector → URL `https://mcp.useanima.sh` (API-key auth works for custom connectors; **official Directory** expects OAuth — see SUBMISSION.md).
- Claude Code community: `claude plugin marketplace add <public-github>/anima-plugin` then install `anima`.

## OpenAI / Codex

Remote MCP URL `https://mcp.useanima.sh` with Bearer `ak_`. Directory listing needs org verification + OAuth path — see SUBMISSION.md.

## Muse Code

Add under `mcp_servers` in Muse settings (streamable_http) — see README.md.

## OpenClaw / ClawHub

https://clawhub.ai/anima — also see `openclaw.plugin.json` pointer in this repo.
