---
name: tududi
description: Create and update high-level tasks in a Tududi installation. Use for project roll-ups, independently deliverable outcomes, task synchronization, and relevant tag management; use anarlog-updates for meeting transcript imports.
---

# Tududi high-level tasks

Tududi holds high-level work: a bound **Project Roll-up** summarizes a project's
state, while an **Outcome Task** represents an independently deliverable result.
Local implementation steps remain in the project's own task system.

## Establish the binding

Read `.agents/tududi.json` when it exists. Its non-secret `task_uid` identifies
the Project Roll-up; `project_uid` optionally records its Tududi project.

Read [the setup reference](references/setup.md) when the binding is absent or
incomplete or authentication is unavailable. Obtain the bearer token from
`token_env` (default `TUDUDI_API_TOKEN`).

**Complete when:** an existing binding or one unambiguous match is selected, or
the single ownership choice required to create or disambiguate it is reported.

## Choose the high-level task

For a bare `$tududi` invocation, synchronize the bound Project Roll-up. An
explicit request may create or update an Outcome Task without a repository
binding. Search by binding, source links, project, and outcome terms; read every
plausible match before deciding to update or create. Preserve human-authored
context and established assignment.

Read the target task and available project evidence before changing it. Ask
only when multiple plausible matches or destinations remain. A request to
create or update a task authorizes the necessary task reads and writes within
the established instance; it does not need a separate confirmation per API
call.

**Complete when:** one target task is matched or the one unresolved ownership
choice is reported.

## Write the task

Keep title, project assignment, and status unchanged unless the user requests
their change. A bare invocation may update the Project Roll-up's summary, next
action, and tags even when local work appears complete. Prefer the Git remote
as repository identity, falling back to the opened workspace path.

Use a concise Outcome Task description or, for a Project Roll-up, replace its
compact snapshot and append one dated history entry only for meaningful change:

```text
Repository: <remote or workspace path>
Kanban: <active task/status, or brief aggregate>
Current focus: <outcome>
Next action: <one concrete action>
Updated: <YYYY-MM-DD>

History:
- <YYYY-MM-DD>: <meaningful delta>; <source path or task link>
```

Keep history concise and append-only. Link source records instead of copying
their contents. Do not impose a full implementation specification on a
high-level task.

Read [the tag reference](references/tags.md), choose up to three relevant tag
additions, and include their creation and assignment in the authorized task
operation. Preserve every existing tag.

**Complete when:** the task expresses the high-level outcome or current project
snapshot, with useful source links and relevant tags.

## Publish and verify

An explicit `$tududi` invocation authorizes synchronizing its known bound task.
An explicit Outcome Task request authorizes its unambiguous creation or update,
including creation and assignment of relevant tags. If no Project Roll-up
match exists during first-time binding, propose the roll-up and destination for
one confirmation. Never create among ambiguous matches.

Read [the API reference](references/api.md) before the first request to an
installation. Verify authentication without displaying the token, write the
authorized delta, then re-read each changed task and confirm its UID, project,
status, description, source links, and tags.

If one operation fails, retain successful independent changes and report the
exact partial result. After an uncertain response, read current state before
retrying so tasks and tags are not duplicated. Do not silently replace a
missing bound task or create/revoke API keys.

Report the target task, source records consulted, changed fields and tags, and
anything left incomplete. Distinguish questions required by this workflow from
permission prompts enforced by the runtime.
