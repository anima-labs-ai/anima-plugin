# Anima plugin (multi-catalog)

**Anima** ([useanima.sh](https://useanima.sh)) — identity infrastructure for AI agents: **email**, **US phone/SMS**, **voice**, **encrypted vault**, and **MCP**.  
**No cards / wallets / x402.**

| | |
|---|---|
| Product | https://useanima.sh |
| Docs | https://docs.useanima.sh · [llms.txt](https://docs.useanima.sh/llms.txt) |
| Console (API keys `ak_`) | https://console.useanima.sh |
| MCP | https://mcp.useanima.sh |
| ClawHub (live) | https://clawhub.ai/anima |
| Privacy | https://useanima.sh/privacy |

Auth today is typically **`Authorization: Bearer ak_...`** (`ANIMA_API_KEY`). Official Claude Directory and some OpenAI listing paths expect **OAuth** — see [SUBMISSION.md](./SUBMISSION.md).

---

## Install

### Cursor / Grok Bot

1. Get an agent API key from [console.useanima.sh](https://console.useanima.sh).
2. **Marketplace** (after publish): install **Anima** from [Cursor Marketplace](https://cursor.com/marketplace) / Grok Bot → Settings → Plugins. Set `ANIMA_API_KEY`.
3. **Local test now:**

```bash
cp -R . ~/.cursor/plugins/local/anima
# reload Cursor / Grok Bot; set ANIMA_API_KEY in plugin variables or env
```

Root [`mcp.json`](./mcp.json) uses `${ANIMA_API_KEY}` — never commit real keys.

### Claude (custom connector)

1. Claude.ai → **Customize → Connectors → Add custom connector**
2. URL: `https://mcp.useanima.sh`
3. Auth: Bearer token / header with your `ak_` key (custom connectors).
4. **Official Connectors Directory** requires Team/Enterprise + **OAuth** — not API-key-only. Tracked in [SUBMISSION.md](./SUBMISSION.md).

### Claude Code / Cowork (community marketplace)

When this repo is public on GitHub:

```bash
claude plugin marketplace add anima-labs-ai/anima-plugin
claude plugin install anima
```

Or submit via https://platform.claude.com/plugins/submit (see SUBMISSION.md). Self-hosted marketplace metadata: [`.claude-plugin/marketplace.json`](./.claude-plugin/marketplace.json).

### Codex / OpenAI (remote MCP)

Point a remote MCP client at:

- URL: `https://mcp.useanima.sh`
- Header: `Authorization: Bearer $ANIMA_API_KEY`

For the shared **ChatGPT + Codex Plugins Directory**, use the OpenAI portal (org verification + review). Details in [SUBMISSION.md](./SUBMISSION.md).

### Muse Code (Meta)

No public catalog — configure `~/.config/muse/settings.json` (shape from Meta docs):

```json
{
  "schema_version": 1,
  "mcp_servers": {
    "anima": {
      "transport": "streamable_http",
      "url": "https://mcp.useanima.sh",
      "headers": {
        "Authorization": "Bearer ak_YOUR_KEY"
      },
      "enabled": true,
      "mode": "optional"
    }
  }
}
```

Skills in this repo can be imported where Muse supports Claude-compatible skills (`muse skills import --from claude` per Meta docs).

### ClawHub / OpenClaw

Already live: **https://clawhub.ai/anima**  
Native OpenClaw code plugin (channel + skill) is maintained separately; this monorepo includes an [`openclaw.plugin.json`](./openclaw.plugin.json) pointer plus portable skills.

### Official MCP Registry

[`server.json`](./server.json) registers remote MCP as **`sh.useanima/anima`** → `https://mcp.useanima.sh`.

**Namespace auth (pick one):**

| Method | Name form | When to use |
|---|---|---|
| **DNS / HTTP** (preferred) | `sh.useanima/anima` | You control `useanima.sh` TXT or `/.well-known/mcp-registry-auth` |
| **GitHub OAuth** | `io.github.anima-labs-ai/anima` | Faster if DNS proof is blocked; change `name` in `server.json` before publish |

```bash
# after mcp-publisher login (dns|http|github):
mcp-publisher publish
```

---

## Repo layout

```text
anima-plugin/
├── README.md
├── privacy.md
├── llms-install.md
├── SUBMISSION.md
├── server.json                 # official MCP registry
├── mcp.json                    # Cursor / Agent Plugins MCP
├── plugin.json                 # Agent Plugins 1.0
├── openclaw.plugin.json        # pointer to ClawHub OpenClaw plugin
├── .cursor-plugin/plugin.json
├── .claude-plugin/plugin.json
├── .claude-plugin/marketplace.json
├── skills/
│   ├── anima/SKILL.md
│   ├── agent-email-inbox/SKILL.md
│   ├── agent-phone-sms/SKILL.md
│   └── agent-vault/SKILL.md
└── assets/logo.svg
```

## Security

- Store `ANIMA_API_KEY` only in env / OS keychain / secrets manager / client variable UI.
- Never paste `ak_` into chat memory, skill notes, or transcripts.
- Never send the key to hosts other than Anima (`api.useanima.sh`, `mcp.useanima.sh`).
- Prefer vault **use-never-see** paths; do not pitch payment cards or wallets.

## License

MIT © Anima Labs
