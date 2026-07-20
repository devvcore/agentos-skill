# Install AgentOS for your coding agent

Two layers make it work:

- **Capability** — the `mcp__agentos__*` tools (context, memory, topics, github, secrets, skills, CRM, tasks, drive, chats), served by the AgentOS MCP server.
- **Awareness** — the `agentos` skill, which teaches the agent WHEN to reach for those tools and how the primitives (grounding, topics, memory) work.

You need both. On **Claude Code** the plugin ships both and wires the MCP connection for you. On **Codex** there's no plugin system, so you add the skill file and the MCP server manually (still two short steps).

First, mint a Personal Access Token in AgentOS: **Settings → Access Tokens**. It scopes the connection to your workspace.

---

## Claude Code (one install wires up everything)

```bash
claude plugin marketplace add devvcore/agentos-skill
claude plugin install agentos@agentos
```

On install you're prompted for your **AgentOS URL** (default `https://tryagentos.net`) and your **Personal Access Token** (stored securely — Keychain / credentials file, never in plaintext). The bundled MCP server then starts automatically: `claude mcp list` shows `agentos` connected, `mcp__agentos__context` and the `topic_*`/`github_*` tools become callable, and the awareness skill loads.

Verify:

```bash
claude plugin list      # agentos (enabled)
claude mcp list         # agentos — connected
```

Update later: `claude plugin marketplace update agentos && claude plugin update agentos@agentos`.
Remove: `claude plugin uninstall agentos`.

### Always-on (optional)
To apply the grounding discipline from message one without invoking `/agentos`, add to `~/.claude/CLAUDE.md`:

```markdown
## Working inside AgentOS
Follow the `agentos` skill: ground via `mcp__agentos__context` before answering
about the business; use `recall_memory`/`journal_add` across runs; work the
topics bus (`topic_search`/`topic_subscribe` at work-start, `topic_post` one
line when you claim, finish, or discover something a co-worker would trip on);
write reusable skills back with `skills_create`.
```

---

## Codex (no plugin system — two manual steps)

Codex has no plugin marketplace, so add the pieces directly.

**1. The skill** — copy it into your Codex skills directory:

```bash
git clone https://github.com/devvcore/agentos-skill /tmp/agentos-skill
mkdir -p ~/.codex/skills/agentos
cp /tmp/agentos-skill/skills/agentos/SKILL.md ~/.codex/skills/agentos/
```

**2. The MCP server** — Codex reads the token from an env var (no `${VAR}` in its config):

```bash
export AGENTOS_TOKEN="<your Personal Access Token>"   # add to your shell profile
codex mcp add agentos --url https://tryagentos.net/api/mcp --bearer-token-env-var AGENTOS_TOKEN
```

Verify: `codex mcp list` shows `agentos`. In Codex, invoke the skill with `$agentos`, or let it trigger implicitly on business-touching work.

---

## Self-hosting
Point `agentos_url` (Claude Code) / `--url` (Codex) at your own AgentOS instance's `/api/mcp`. Everything else is identical.

## Troubleshooting
- **No `mcp__agentos__*` tools.** The MCP server isn't connected. Claude Code: `claude mcp list` — if `agentos` is missing, re-run the plugin install and re-enter the token. Codex: confirm `AGENTOS_TOKEN` is set in the current shell and re-run `codex mcp add`.
- **401 from the server.** The token is wrong or from another workspace — mint a fresh one in Settings → Access Tokens.
- **Agent answers from training instead of grounding.** Start a fresh session so context clears; confirm `mcp__agentos__context` is callable.
