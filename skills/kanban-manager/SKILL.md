---
name: kanban_manager
description: "Manage filesystem Kanban tasks at 00-kanban/: create tasks, plan specs and subtasks, implement, review, report status, move tasks, initialize, or migrate a board."
metadata:
  version: 4.0.0
---

# Kanban Manager

For substantive project work, begin with `Task check:` and identify the matching Task. Read its `readme.md` and relevant Task Context before working.

## Board and setup

Use one `00-kanban/` Board at the workspace root, with these Status folders:

```text
00-kanban/
├── 01-backlog/
├── 02-planning/
├── 03-progress/
├── 04-blocked/
├── 05-review/
├── 06-done/
└── 07-cancelled/
```

For initialization, project setup, missing agent instructions, or an existing `kanban/`, `.kanban/`, or `docs/` layout, read [Setup and migration](references/setup.md). A configured legacy Board remains active until the user approves migration. Inspect competing Board roots before proposing a resolution; use one active Board.

## Select or create a Task

- When the user names a Task, search every Status folder. Otherwise, a related Task in `03-progress` is current.
- If no Task matches, ask which Task to use or whether to create one. An explicit creation request already supplies that authorization.
- Inspect Task folder prefixes across every Status, use the next number above the highest existing number, and preserve historical unnumbered folders. Task names use `<number>-<slug>`.
- Create new Tasks in `01-backlog` with only `readme.md`, unless the user explicitly names another initial Status. Creation directly in planning permits planning artifacts.

The readme is a concise control file: `Goal`, `Current State`, and dated `History`. Link the spec, Subtasks, and living plan; record resume-worthy decisions and results.

## Document ownership

Keep Task Context inside its Task folder: `spec.md`, `tickets/`, one living `implementation-plan.md`, research, notes, and generated material. A Task's spec stays with the Task even when it draws on shared requirements.

A Subtask is one document in the parent Task's `tickets/`, numbered locally in dependency order. It records an outcome, acceptance criteria, blockers, and readiness; it inherits the parent Task's Status permissions. Subtasks use their own local numbers and do not consume Board Task identifiers. Mark completed Subtasks `done` and record verification in their documents.

Use `00-docs/` for shared project documentation with or without Matt Pocock's skills. Standard entrypoints such as `AGENTS.md`, `CLAUDE.md`, `README.md`, and the domain glossary keep their host-required or project-established locations.

## Status permissions

| Status | Agent may do |
|---|---|
| `01-backlog` | Create the Task with its initial readme; inspect and triage. |
| `02-planning` | Create or update the spec, Subtasks, research, and living Implementation Plan. |
| `03-progress` | Implement after an explicit user command; maintain Task Context and verification. |
| `04-blocked` | Record a newly reported `Blocked by:` reason in the readme; otherwise wait. |
| `05-review` | Record verification and feedback. Fixes require a user move to progress. |
| `06-done` | Answer status and history questions. |
| `07-cancelled` | Answer status and history questions. |

Start planning in `02-planning`; maintain its artifacts during authorized implementation in `03-progress`. Implementation requires both progress Status and an explicit command. Name any missing condition before proceeding.

The user decides every Status move. Move only on an explicit command naming the Task and destination; manual user moves are equally valid. You may identify the required destination, but never initiate a move or ask a yes/no move question. Apply these permissions to Subtasks as well as their parent.

In Plan Mode, propose Task actions and documentation changes without editing files.

## Plan a Task

1. Read the Task Context, relevant shared documentation, project glossary, and applicable decisions. Reuse established requirements; keep synthesized Task-specific context with the Task.
2. For specification work, check `to-spec`; for decomposition, check `to-tickets`. Read and follow each installed skill's instructions independently for the authorized planning work. If the host marks a skill explicit-only and the user has not invoked it, use its installed instructions as a reference; invoke the skill itself only on an explicit user request. For a missing workflow, read the corresponding section of [Planning workflows](references/planning-workflows.md).
3. Set the publication contract before following either workflow: update this Task's `spec.md` and publish approved Subtasks as separate documents in its `tickets/`. Existing Tasks are reused; shared-doc specs, scratch output, and additional Board Tasks are not publication destinations for this planning request.
4. When a workflow is missing, suggest optional installation of Matt Pocock's `to-spec` and `to-tickets` and `/setup-matt-pocock-skills`, configured for this Board and publication contract. Continue using bundled guidance. Offer the suggestion once per session; installation remains the user's choice.
5. Preserve the selected workflow's review steps. Publish Subtasks after approval of their granularity and blocking edges. Use `ready-for-agent` readiness and explicit blockers; only Subtasks whose blockers are done form the frontier.
6. Update the Task readme's artifact pointers and the single living Implementation Plan. Planning is complete when the requested artifacts are present in the Task folder and their references and blocking edges resolve.
