---
description: Create or update an AgentOS task to track in-flight work
argument-hint: <what to track> | done <task-id> <outcome>
---

Manage AgentOS tasks based on: **$ARGUMENTS**

- If I'm describing new work: first `tasks_list({ status: "in_progress" })` to
  avoid duplicating an existing task, then `tasks_create` with a clear title and
  description. Add subtasks with `tasks_subtask_create` if it has distinct steps.
- If I say `done <task-id> <outcome>`: call `tasks_complete(task_id, outcome)`.
- If I'm adding progress to existing work: `tasks_comment(task_id, …)` or
  `tasks_update`.
- If something is blocking me: `tasks_blocked` to flag it.

Confirm the task id and final state.
