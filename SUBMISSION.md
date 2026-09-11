# Anima plugin — submission next steps

Local pack is ready under `/workspace/anima-plugin/`. Network publish needs Diyan logins / DNS / OAuth.

## 0) Shared prep (once)

- [ ] Push this monorepo to public GitHub, e.g. `https://github.com/anima-labs-ai/anima-plugin` (update `repository` fields if the org/name differs).
- [ ] Confirm production MCP: `https://mcp.useanima.sh` (Streamable HTTP, tool annotations).
- [ ] Seed a **reviewer** identity (inbox + phone/vault fixtures as needed); no MFA gate for reviewers.
- [ ] Privacy already live: https://useanima.sh/privacy · Support: diyan@useanima.sh / privacy@useanima.sh

---

## 1) Cursor Marketplace (= Grok Bot Plugins) — P0

| | |
|---|---|
| Submit | https://cursor.com/marketplace/publish |
| Docs | https://cursor.com/docs/plugins · https://cursor.com/docs/reference/plugins |
| Local test | `cp -R /workspace/anima-plugin ~/.cursor/plugins/local/anima` |

**Steps**

1. Public GitHub repo with `.cursor-plugin/plugin.json` + root `mcp.json` (`${ANIMA_API_KEY}`).
2. Open https://cursor.com/marketplace/publish and paste the repo URL.
3. Wait for Cursor manual review (often ~3–14 days).

API-key-via-variable is OK for Cursor even if Claude Directory wants OAuth.

---

## 2) Claude Connectors Directory — P0 (blocked on OAuth + Team)

| | |
|---|---|
| Flow | Claude.ai → Organization settings → Directory → submissions |
| Docs | https://claude.com/docs/connectors/building/submission |
| Auth | https://claude.com/docs/connectors/building/authentication |
| Escalate | mcp-review@anthropic.com |

**Blockers for Anima today**

1. **Claude Team or Enterprise** org (Owners submit).
2. **OAuth 2.1 (PKCE)** on MCP — `static_headers` / pure `ak_` is a poor fit for consumer Directory UX.
3. Tool `title` + `readOnlyHint` / `destructiveHint` / `openWorldHint`.

**Until OAuth ships:** users can still add a **custom connector** to `https://mcp.useanima.sh` with Bearer `ak_`.

**Claude Code community plugin (P1, can proceed without OAuth):**

- Validate: `claude plugin validate` (when CLI available)
- Submit: https://platform.claude.com/plugins/submit  
  Team path: https://claude.ai/admin-settings/directory/submissions/plugins/new  
- Docs: https://claude.com/docs/plugins/submit · https://code.claude.com/docs/en/plugins
- Immediate self-serve: `/plugin marketplace add anima-labs-ai/anima-plugin`

---

## 3) Official MCP Registry — P0

| | |
|---|---|
| Registry | https://registry.modelcontextprotocol.io |
| Auth docs | https://modelcontextprotocol.io/registry/authentication |
| Remote docs | https://modelcontextprotocol.io/registry/remote-servers |
| File | [`server.json`](./server.json) → name **`sh.useanima/anima`** |

**Commands (Diyan machine with DNS or GitHub auth):**

```bash
cd /path/to/anima-plugin

# Preferred: DNS TXT on useanima.sh for namespace sh.useanima/*
mcp-publisher login dns --domain useanima.sh --private-key "$PRIVATE_KEY"
# OR HTTP: host /.well-known/mcp-registry-auth then:
# mcp-publisher login http --domain useanima.sh --private-key "$PRIVATE_KEY"

# Fallback: GitHub — change server.json "name" to io.github.anima-labs-ai/anima first
# mcp-publisher login github

mcp-publisher publish
```

Cascades toward PulseMCP / Glama / aggregators after publish.

---

## 4) OpenAI Plugins Directory (ChatGPT + Codex) — P1

| | |
|---|---|
| Portal | https://platform.openai.com/apps-manage |
| Submit docs | https://developers.openai.com/plugins/deploy/submission |
| Review | https://developers.openai.com/plugins/deploy/app-review |

Needs: verified org identity, domain challenge `/.well-known/openai-apps-challenge`, annotated tools, privacy/terms/support URLs, **5 positive + 3 negative** test cases, demo recording, auth path (OAuth expected for polished listing).

---

## 5) Smithery / Glama / mcp.so — P1

| Surface | URL |
|---|---|
| Smithery new | https://smithery.ai/new |
| Smithery docs | https://www.smithery.ai/docs/build/publish |
| Example CLI | `smithery mcp publish "https://mcp.useanima.sh" -n @anima/anima` |
| Glama | https://glama.ai/mcp → Add MCP Server / Connector · FAQ https://glama.ai/mcp/faq |
| mcp.so | https://mcp.so/submit |

---

## 6) ClawHub / OpenClaw — upgrade existing

- Live profile: https://clawhub.ai/anima  
- Code plugin source: `/workspace/anima-clawhub/plugins/anima`  
- Publish: `clawhub package publish` (see OpenClaw docs)  
- Official badge: staff-only — coordinate with ClawHub admins (AgentMail pattern)

---

## 7) Muse / Windsurf / Continue — docs only

Snippets live in [README.md](./README.md). No public Muse gallery.

---

## Suggested order this week

1. GitHub push of this monorepo  
2. `mcp-publisher publish` (`sh.useanima/anima` or GitHub namespace)  
3. Cursor marketplace/publish  
4. Smithery + Glama + mcp.so  
5. Claude custom connector test; start OAuth + Team for Directory  
6. OpenAI org verify + draft plugin  
7. ClawHub package publish / Official ask  
