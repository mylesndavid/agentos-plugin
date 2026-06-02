---
description: Recall context from AgentOS memory (wiki + tasks + skills) for a topic
argument-hint: <topic or question>
---

Load relevant context from AgentOS for: **$ARGUMENTS**

Do this efficiently, in parallel where possible:

1. `wiki_search` for the topic. For the top 1–3 hits, `wiki_get` the full doc.
2. `skills_list` and surface any saved skill that matches the topic; if one
   clearly applies, `skills_get` it and note we should follow it.
3. `tasks_list({ status: "in_progress" })` and flag any task related to the topic.
4. If the topic names a person or company, `crm_contacts_search` /
   `crm_companies_search`.

Then give me a tight briefing: what AgentOS already knows, the wiki paths you
read (so I can follow the chain), any matching skill, and open/related tasks.
If nothing relevant exists, say so plainly — don't pad.
