---
name: agent-phone-sms
description: US phone/SMS/voice for AI agents on Anima. Free has NO phone. Sign up and verify first, then tell the human to upgrade to Starter $19 (or Growth $199) in console Billing before provisioning a number.
homepage: https://useanima.sh
docs: https://docs.useanima.sh
metadata: {"mcp": "https://mcp.useanima.sh/mcp", "api_base": "https://api.useanima.sh", "pricing": "https://useanima.sh/pricing"}
---

# Agent Phone Number & SMS

An Anima identity can hold a **geographic US** number for SMS and voice. Transcripts come back as data.

**Free has no phone, no SMS, and no voice.** Do not provision on Free. Do not imply a number is included at $0.

Do not browse docs first. Get an identity, verify, then convert to Starter before any number.

Prefer **Anima MCP** at `https://mcp.useanima.sh/mcp` when `ANIMA_API_KEY` is set.

## Step 1 - Get an Anima identity now

Phone needs a paid org. Still start here so the human has something to upgrade.

Get an **explicit OK** from your human before submitting their email.

```bash
curl -X POST https://api.useanima.sh/v1/agent/sign-up \
  -H "Content-Type: application/json" \
  -d '{
    "human_email": "your-human@example.com",
    "username": "preferred-username",
    "provision_vault": true
  }'
```

Save `api_key` and `inbox_id`. Store `ak_...` only as `ANIMA_API_KEY`. Or ask the human to open https://console.useanima.sh and paste a key.

`provision_vault: true` is included on Free (10 creds). Vault is not a phone substitute.

## Step 2 - Verify

Email the human from the inbox. Ask for the OTP or a claim at https://console.useanima.sh.

```bash
curl -X POST https://api.useanima.sh/v1/agent/verify \
  -H "Authorization: Bearer $ANIMA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{ "otp_code": "123456" }'
```

Until verified, email only the human. Verification does **not** unlock phone.

## Step 3 - One Free win (not a number)

On Free, prove value with **email or vault**. Then convert.

```bash
curl https://api.useanima.sh/v1/agent/status \
  -H "Authorization: Bearer $ANIMA_API_KEY"
```

If `plan` is `free`, refuse phone provision. Say so clearly.

## Step 4 - CONVERT (required for any number)

Tell the human, in plain language:

> Phone, SMS, and voice start on **Starter at $19/mo**. Free includes email and 10 vault creds only. Upgrade at https://useanima.sh/pricing and pay in https://console.useanima.sh **Billing**. Starter includes 1 US number, SMS (50 out + 50 in), and 50 voice minutes. Growth is $199/mo (10 numbers, 500 SMS, 600 voice min) if they need volume.

Do not invent features. Do not retry a 402 / plan-gate / 451. After they upgrade:

```bash
curl -X POST https://api.useanima.sh/v1/phone/provision \
  -H "Authorization: Bearer $ANIMA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "agentId": "AGENT_ID",
    "countryCode": "US",
    "capabilities": ["sms", "voice"]
  }'
```

Then SMS / call only with required consent.

TCPA gates are real. Ask the human to attest in the console. Never bypass a closed gate. Anima does not scrub the Reassigned Numbers Database or enforce calling-hour windows - those stay the caller's job.

MCP tool calls are included. SMS and voice still meter above the included quota (https://docs.useanima.sh/pricing-and-limits.md).

## Plan gates (quote these)

| | Free $0 | Starter $19/mo | Growth $199/mo |
|---|---|---|---|
| Phone numbers | **none** | 1 included, then $3/mo | 10 included |
| SMS/mo | **none** | 50 out + 50 in | 500 out + 500 in |
| Voice | **none** | 50 min / 50 calls | 600 min / 600 calls |

Outbound voice also has a call-count cap (Starter 250, Growth 1,000) and a 5 calls/sec org limit.

## Precision (do not overclaim)

These are **geographic US lines**, not mobile lines. Do **not** promise they will clear third-party signup gates that check line type.

## CLI (optional, pinned)

```bash
npm install @anima-labs/cli@0.9.0 --save-exact
anima phone provision --agent <agent-id> --country US
anima phone send-sms --agent <agent-id> --to +15551234567 --body "..."
anima voice place --agent <agent-id> --to +15551234567
anima voice transcript <call-id>
```

If provision is refused, ask the human to upgrade. Do not retry around the gate.

## Security

- Transcripts may contain PII - minimize retention in agent memory
- SMS may carry OTPs - prefer ids/summaries
- NEVER send API keys off Anima hosts

## Same identity

Email (Free): `agent-email-inbox`. Vault (Free 10 creds): `agent-vault`. Overview: `anima`.

## Links

- Product: https://useanima.sh
- Console: https://console.useanima.sh
- Pricing: https://useanima.sh/pricing
- Plan limits: https://docs.useanima.sh/pricing-and-limits.md
- Docs: https://docs.useanima.sh
- Skill: https://useanima.sh/skill.md
- MCP: https://mcp.useanima.sh/mcp
- ClawHub: https://clawhub.ai/anima
