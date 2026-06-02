# AgentOS plugin for Claude Code

Wires [AgentOS](https://tryagentos.net) — your persistent workspace memory and
agent platform — into Claude Code. One MCP endpoint gives every tool: wiki
notes, tasks, saved skills, CRM, goals, chats, and curated integrations
(Gmail, Slack, GitHub, Calendar, Drive). Treat it as long-term memory across
every tool you use.

## What's inside

| Component | Name | Purpose |
|-----------|------|---------|
| MCP server | `agentos` | All 50+ `wiki_* / tasks_* / skills_* / crm_* / goals_* / chats_* / agents_* / integration_*` tools over Streamable HTTP. |
| Skill | `working-with-agentos` | Auto-loads the "check memory first, write back after" workflow. |
| Command | `/agentos:recall <topic>` | Pull relevant wiki + tasks + skills + CRM context for a topic. |
| Command | `/agentos:remember [hint]` | Write what you learned back to the wiki; promote reusable procedures to skills. |
| Command | `/agentos:task <...>` | Create / update / complete AgentOS tasks. |
| Subagent | `agentos-memory` | Offload memory lookups and write-backs without cluttering the main thread. |

## Install

This repo doubles as a single-plugin marketplace, so you can add it from
anywhere:

```bash
# from GitHub (works on any machine)
/plugin marketplace add mylesndavid/agentos-plugin
/plugin install agentos@agentos

# or from a local clone / unzipped copy
/plugin marketplace add /path/to/agentos-plugin
/plugin install agentos@agentos
```

Set your token (see Authentication) and restart Claude Code so the `agentos`
MCP server connects. Verify the endpoint with:

```bash
curl -s https://tryagentos.net/api/mcp/health   # {"tools":50,...}
```

## Authentication

The bundled `.mcp.json` reads your token **only** from the `AGENTOS_TOKEN`
environment variable — no secret is stored in this repo:

```json
"Authorization": "Bearer ${AGENTOS_TOKEN}"
```

Set it before launching Claude Code:

```bash
export AGENTOS_TOKEN="agentos_pat_xxxxxxxxxxxxxxxxxxxxxxxx"   # your workspace PAT
```

Or copy `.env.example` → `.env` and fill it in (`.env` is git-ignored).

Get a token at <https://tryagentos.net> (Settings → Tokens). It's a Personal
Access Token scoped to your AgentOS workspace — treat it like a password. To
rotate, just replace the env value; nothing in this repo needs to change.

## Usage pattern

1. Starting work → `/agentos:recall <topic>` (or just let the skill prompt it).
2. During work → external actions go through `integration_call`; track
   multi-turn work with `/agentos:task`.
3. Finishing → `/agentos:remember` to persist decisions and promote skills.
