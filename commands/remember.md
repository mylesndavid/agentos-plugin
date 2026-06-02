---
description: Write what we just learned/did back into AgentOS wiki (and promote a skill if reusable)
argument-hint: [optional note or wiki path hint]
---

Persist the useful output of this session into AgentOS long-term memory.

Context / hint from me: **$ARGUMENTS**

1. Decide what's worth keeping: decisions made, facts discovered, runbook
   steps, gotchas. Skip transient chatter.
2. Pick a sensible wiki location (`projects/`, `runbooks/`, `decisions/`,
   `research/`, …). Use `wiki_tree` to match existing structure;
   `wiki_create_folder` if no fitting folder exists.
3. Write it with `wiki_write` (new doc) or `wiki_append` / `wiki_upsert_section`
   (existing doc). Use clear headings and a short summary line up top.
4. If we followed a repeatable procedure, promote it with
   `skills_create({ name, description, content })`.
5. If this closes a tracked task, `tasks_complete(task_id, outcome)`.

Report exactly what you wrote and where (paths + skill slugs) so I can find it.
