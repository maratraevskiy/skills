---
name: tududi
description: Synchronize a repository's Kanban work and Anarlog commitments with Tududi. Use when binding a repository to its roll-up task, syncing it, or publishing Anarlog-derived work.
---

# Tududi project sync

Tududi is a shared **projection**: one bound task holds a repository's current
state and short audit trail. The filesystem Kanban board remains the execution
record. Anarlog sessions are evidence for independently deliverable Tududi
work items, whether or not they belong to the current repository.

## Establish the binding

Read `.agents/tududi.json` when it exists. Its non-secret `task_uid` identifies
the one roll-up task for this repository; `project_uid` is optional and records
its intended Tududi project.

Read [the setup reference](references/setup.md) when the binding is absent or
incomplete, when its task must be found or created, or when authentication is
not available. Obtain the bearer token from `token_env` (default
`TUDUDI_API_TOKEN`).

**Complete when:** the roll-up task is known, or the ownership decision needed
to bind it has been reported without creating an ambiguous task.

## Gather the sources

If the repository has `kanban/`, load `kanban_manager` and follow its
task-selection and status-permission rules. Do not create or move local tasks.
When no board exists, sync only repository evidence explicitly available to
you.

Read the bound Tududi task before changing it. Explicit human decisions in
Tududi win conflicts; preserve useful Tududi-only context.

For an Anarlog session, read [the Anarlog reference](references/anarlog.md).
It owns session selection, the canonical archive, candidate extraction,
deduplication, and portable work-item specifications.

**Complete when:** the relevant local state, current Tududi records, and any
session evidence have been compared.

## Project repository state

Keep the roll-up title and Tududi status unchanged unless the user explicitly
asks to change them. Prefer the Git remote as repository identity, falling back
to a local path only when no remote exists.

Replace the compact snapshot, then append one concise dated history entry only
when a meaningful state change occurred:

```text
Repository: <remote or local path>
Kanban: <active task/status, or brief aggregate>
Current focus: <outcome>
Next action: <one concrete action>
Updated: <YYYY-MM-DD>

History:
- <YYYY-MM-DD>: <meaningful delta>; <source path or task link>
```

Keep history concise and append-only. Link Kanban records, canonical
transcripts, and related work items instead of copying their contents. Add a
history entry for an Anarlog work item only when it belongs to this repository.

**Complete when:** the roll-up is a minimal, current snapshot with a traceable
history, not a duplicate Kanban board.

## Publish and verify

An explicit `$tududi` sync or an explicit request to update Tududi authorizes
updates to known, matching tasks. A distinct Anarlog work item requires a
proposal and user approval before creation. An unresolved destination remains
unpublished.

Read [the API reference](references/api.md) before the first request to an
installation. Use the installation's Swagger documentation as the request
contract. Verify authentication without displaying the token, write only the
approved delta, then re-read each changed task and confirm its UID, project,
status, snapshot, and evidence links.

If authentication fails, a task is missing, or ownership is ambiguous, report
the exact proposed change and what is needed to continue. Do not create a
replacement roll-up task, alter Kanban status, or create/revoke API keys.

Report the bound task, source records consulted, changed fields, proposed or
created Anarlog work items, and deliberately unpublished candidates.
