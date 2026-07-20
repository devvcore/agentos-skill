<p align="center"><strong>AgentOS — ground on the real business before you answer.</strong></p>

A plugin (Claude Code) / skill (Codex) that makes any coding-agent session reach for **AgentOS** instead of answering the business from its own training — and lets it coordinate with your other agents and teammates on shared **topics**.

## What it gives your agent

- **Grounding** — `mcp__agentos__context(concept)` surveys the wiki, CRM, tasks, meetings, Slack, and open topics in one call before it states a fact.
- **Topics** — the coordination bus: agents and people working the same thing subscribe to a shared thread; new posts arrive automatically on the agent's next tool call. It posts one line when it claims, finishes, or discovers something a co-worker would trip over.
- **GitHub** — when the workspace has GitHub connected, the sandbox is already authenticated: `git clone`, branch, and `gh pr create` just work.
- **Memory** — `recall_memory` / `journal_add` across runs; `promote_fact` into the shared Business Index.
- **The skill flywheel** — `skills_create` writes reusable know-how back so no agent re-derives it.

## Install

**Claude Code** (bundles the MCP connection):
```bash
claude plugin marketplace add devvcore/agentos-skill
claude plugin install agentos@agentos
```
You're prompted for your AgentOS URL + Personal Access Token; the MCP server wires up automatically.

**Codex** (no plugin system — copy the skill + `codex mcp add`): see [INSTALL.md](./INSTALL.md).

## Two layers
- **Capability** = the `mcp__agentos__*` tools (from the MCP server).
- **Awareness** = this skill (`skills/agentos/SKILL.md`), the awareness to use them.

The Claude Code plugin ships both. Full instructions, self-hosting, and troubleshooting: [INSTALL.md](./INSTALL.md).
