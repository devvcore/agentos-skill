<p align="center">
  <strong>AgentOS — ground on the real business before you answer.</strong>
</p>

A Claude Code / Codex plugin. One skill inside. It makes any coding-agent session naturally reach for AgentOS instead of answering the business from its own training.

## Two layers

- **Capability layer** — the `mcp__agentos__*` tools (context, recall, journal, secrets, skills, CRM, tasks, drive, chats). These come from the AgentOS MCP server.
- **Awareness layer** — this skill. The tools exist, but nothing tells a coding CLI they exist, what they're for, or that grounding on the business is the expected first move. This skill is that missing awareness.

You need both. The tools without the awareness get ignored; the awareness without the tools is inert.

## Install

### Claude Code

```bash
git clone https://github.com/devvcore/agentos-skill ./agentos-skill
claude plugin marketplace add ./agentos-skill
claude plugin install agentos@agentos
```

In Claude Code: `/agentos`.

### Codex

```bash
codex plugin marketplace add devvcore/agentos-skill --ref main
codex plugin add agentos@agentos
```

In Codex: `$agentos`.

Then connect the AgentOS MCP server so the `mcp__agentos__*` tools exist. Full steps in [INSTALL.md](./INSTALL.md).

## What it does

Teaches the agent WHEN to reach for AgentOS and HOW:

- **Ground first.** The moment you reason about a person, client, project, or topic, call `mcp__agentos__context()` before answering from training.
- **Grounding is non-negotiable.** State a specific fact only if a tool result this session showed it. A gap is an answer; an invention poisons the shared record.
- **Use your memory.** `recall_memory` / `recall_shared_truth` at the start; `journal_add` / `promote_fact` as you learn — you're a continuous person across runs, not a fresh chatbot.
- **Credentials are authorization.** A pasted key means store it (`secrets_set`) and use it — no lecture.
- **Skill flywheel.** When you figure out something reusable, write it back with `skills_create` so no agent solves it twice.
- **Staff, not chatbot.** Trusted with sensitive material, judged on whether the thing got done.

The skill body is the same tool-awareness contract native AgentOS agents run under — full text in [skills/agentos/SKILL.md](./skills/agentos/SKILL.md).

## License

MIT.
