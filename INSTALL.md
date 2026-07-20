# Install agentos

A Claude Code / Codex plugin. One skill inside — the awareness layer for the AgentOS MCP tools.

Two layers, don't confuse them:
- **Capability layer** — the `mcp__agentos__*` tools. These come from connecting the AgentOS MCP server to your session. Without it there is nothing to ground against.
- **Awareness layer** — this skill. It teaches the agent WHEN to reach for those tools and HOW. Install both.

## TL;DR

### Claude Code

```bash
git clone https://github.com/devvcore/agentos-skill ./agentos-skill
claude plugin marketplace add ./agentos-skill
claude plugin install agentos@agentos
```

Open Claude Code, type `/agentos`.

To disable: `claude plugin disable agentos` (or `/plugin disable agentos` from within Claude Code). Re-enable later with `enable` instead of `disable`.

### Codex

```bash
codex plugin marketplace add devvcore/agentos-skill --ref main
codex plugin add agentos@agentos
```

In Codex, type `$agentos` to invoke it explicitly. Codex can also invoke it implicitly when it sees a task that touches the business.

## Connect the AgentOS MCP server (required)

The skill is inert without the `mcp__agentos__*` tools. Add the AgentOS MCP server to your session so those tools exist:

### Claude Code

```bash
claude mcp add agentos --transport http https://tryagentos.net/api/mcp \
  --header "Authorization: Bearer <your AgentOS Personal Access Token>"
```

Mint the token in AgentOS under Settings → Access Tokens. Then `claude mcp list` should show `agentos` connected, and `mcp__agentos__context` and friends become callable — including the topics bus (`topic_*`), whose updates arrive automatically inside your tool results once you subscribe.

### Codex

Add the AgentOS MCP server to your Codex MCP config (`~/.codex/config.toml`), pointing at your workspace's `/mcp` endpoint, then restart Codex.

## Verify

### Claude Code

```bash
claude plugin list
```

Look for `agentos  (enabled)`. Then confirm the tools are live:

```bash
claude mcp list
```

Look for `agentos` connected.

### Codex

```bash
codex plugin list
```

Look for `agentos` in the configured `agentos` marketplace.

## Update

### Claude Code

```bash
cd ./agentos-skill && git pull
```

The marketplace re-reads the local checkout. Next Claude Code session picks up changes.

### Codex

```bash
codex plugin marketplace upgrade agentos
codex plugin remove agentos
codex plugin add agentos@agentos
```

## Uninstall

### Claude Code

```bash
claude plugin uninstall agentos
claude plugin marketplace remove agentos
```

### Codex

```bash
codex plugin remove agentos
codex plugin marketplace remove agentos
```

## Always-on (optional)

To skip `/agentos` and apply the grounding discipline from message one, add to `~/.claude/CLAUDE.md`:

```markdown
## Working inside AgentOS

Always follow the `agentos` skill: whenever reasoning about the business, its
people, clients, projects, docs, or data, ground via `mcp__agentos__context`
first before answering from training; use `recall_memory` / `journal_add`
across runs; write reusable skills back with `skills_create`; and work the
topics bus — `topic_search`/`topic_subscribe` at work-start, `topic_post` one
line when you claim, finish, or discover something a co-worker would trip on.
```

## Troubleshooting

**`/agentos` not in autocomplete.** Restart Claude Code. The plugin index is read at startup.

**`claude plugin marketplace add` fails.** Point at the repo root, not at `.claude-plugin/`. The path must contain `.claude-plugin/marketplace.json`.

**Skill loads but there are no `mcp__agentos__*` tools.** The capability layer is missing. Connect the AgentOS MCP server (see above). The skill is awareness only; it does not ship the tools.

**Agent still answers from training instead of grounding.** Open a new session so stale context clears. If it still drifts, confirm `mcp__agentos__context` is callable (`claude mcp list`), then tighten the wording in `skills/agentos/SKILL.md` and re-invoke.
