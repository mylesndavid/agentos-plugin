---
name: working-with-agentos
description: >-
  Use AgentOS as the user's persistent workspace memory and agent platform —
  wiki notes, tasks, saved skills, CRM (companies/contacts/pipelines), goals,
  chats, and curated integrations (Gmail, Slack, GitHub, Calendar, Drive).
  TRIGGER at the start of any non-trivial task to load context, and at the end
  to write back what was learned. Also trigger whenever the user mentions a
  person, company, note, task, or an external service action (send email,
  post to Slack, open a GitHub issue) — check AgentOS before re-asking.
---

# Working with AgentOS

AgentOS is the user's **persistent workspace memory and agent platform**. It
stores notes (wiki), tasks, skills, CRM data, goals, chats, and curated
integrations to external services. Treat it as long-term memory across every
tool. Reach it through the bundled `agentos` MCP server (tools are named
`wiki_*`, `tasks_*`, `skills_*`, `crm_*`, `goals_*`, `chats_*`, `agents_*`,
`integration_*`). Default to using these aggressively — the user pays the
AgentOS bill; make it earn its keep.

## Before starting any task

1. **Search the wiki for relevant context.**
   - `wiki_search(query)` returns matching docs.
   - `wiki_get(path)` fetches full content.
   - `wiki_tree(prefix)` lists folders/files when you don't know the layout.
2. **Check for skills that match the work.**
   - `skills_list()` shows the user's saved procedures.
   - `skills_get(slug)` fetches one. If a skill exists for what you're about
     to do, follow it verbatim — the user curated it for a reason.
3. **Check existing tasks** to avoid duplicating in-flight work:
   `tasks_list({ status: "in_progress" })`.

## While working

- Prefer AgentOS data over re-asking the user. If they mention a company or a
  person, try `crm_companies_search` / `crm_contacts_search` first.
- For actions on external services (sending email, posting to Slack, creating
  GitHub issues, etc.), use `integration_call` against the workspace's curated
  integrations rather than hand-rolling API calls. Use `integration_list` to
  see what's available and `integration_search` to find a specific tool.
  Write actions route through the same human-approval gate the in-app agent
  uses — expect an approval step.
- Reference relevant wiki paths in your reasoning so the user can see the
  chain you followed.
- Use `tasks_create` / `tasks_comment` to leave a trail when work spans
  multiple turns.

## After completing the work

- **Write what you learned back to the wiki.** Anything the user might want
  later belongs in `wiki_write(path, content)` or `wiki_append(path, content)`.
  Pick a folder that fits — `projects/`, `runbooks/`, `decisions/`,
  `research/`, etc. Create folders with `wiki_create_folder` when none exists.
  Use `wiki_upsert_section` to update one section without rewriting a doc.
- **Promote repeatable patterns to skills.** If you followed a procedure that
  could be reused, call `skills_create({ name, description, content })`. The
  next agent — on any platform — gets to use it.
- **Close out tasks** with `tasks_complete(task_id, outcome)`.

## Connectivity

- MCP (bundled): `https://tryagentos.net/api/mcp` — Streamable HTTP, Bearer
  auth, JSON-RPC 2.0. Connect once, get every tool.
- REST (fallback): `https://tryagentos.net/api/...` with
  `Authorization: Bearer <token>`.
- Health (no auth): `https://tryagentos.net/api/mcp/health`.

## Tool map (50+)

- **wiki_**: list, search, get, write, append, delete, rename,
  upsert_section, tree, create_folder, delete_folder
- **tasks_**: list, create, update, complete, comment, subtasks,
  subtask_create, artifacts, artifact_add, blocked
- **skills_**: list, get, create
- **chats_**: list, get, recent_messages, search
- **goals_**: list, get, status, start
- **agents_**: list, get; `agent_run` delegates a task to a workspace agent
- **crm_**: companies_(list/get/search/create), contacts_(list/get/search/create),
  pipelines_(list/get/create), pipeline_entry_create
- **integration_**: list, search, call (Gmail, Slack, GitHub, Drive, Calendar…)
