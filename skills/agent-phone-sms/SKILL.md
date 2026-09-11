---
name: agent-phone-sms
description: Give your AI agent its own real US phone number — inbound/outbound SMS and voice with transcripts. Use with Anima MCP or API when the agent needs to text, call, or be reached on its own number.
homepage: https://useanima.sh
docs: https://docs.useanima.sh
metadata: {"mcp": "https://mcp.useanima.sh", "api_base": "https://api.useanima.sh"}
---

# Agent Phone Number & SMS

An Anima identity can hold a **geographic US** number for SMS and voice. Transcripts come back as data.

Prefer **Anima MCP** tools when connected. Phone/SMS/voice are typically **plan-gated**; if refused, ask the human to upgrade at https://console.useanima.sh rather than retrying around the gate.

## Capabilities (product)

- Provision / list / release a US number (optional area-code preference; capabilities such as sms, mms, voice).
- Send SMS; receive inbound SMS (webhooks where configured).
- Place calls; fetch transcripts / summaries / scores when available.
- Outbound calling is gated server-side on TCPA consent, plan caps, and spend — refusal means the product is working.

## Precision (do not overclaim)

These are **geographic US lines**, not mobile lines. Do **not** promise they will clear third-party signup gates that check line type — some services accept them, some do not.

## CLI (optional, pinned)

```bash
npm install @anima-labs/cli@0.9.0 --save-exact
anima phone provision --agent <agent-id> --country US
anima phone send-sms --agent <agent-id> --to +15551234567 --body "..."
anima voice place --agent <agent-id> --to +15551234567
anima voice transcript <call-id>
```

## Security

- Transcripts may contain PII and secrets; treat like a recording archive — minimize retention in agent memory.
- SMS may carry OTPs; prefer ids/summaries over raw bodies in durable memory.
- Only contact numbers with required consent. Anima does not scrub the Reassigned Numbers Database or enforce calling-hour windows — those remain caller responsibility.
- NEVER send API keys off Anima hosts.

## Same identity

Email: `agent-email-inbox`. Vault: `agent-vault`. Overview: `anima`.

Docs: https://docs.useanima.sh
