---
name: kanban_manager
description: "Manage project work in a filesystem Kanban board at `kanban/`: create numbered task folders, respect status permissions, and keep task context together. Use for task creation, planning, implementation, reviews, board status, moves, migration, or when the user mentions Kanban."
metadata:
  version: 3.0.0
---

# Kanban Manager

For every substantive project request, begin with `Task check:` and identify the matching task or state that none matches. Read the selected task's `readme.md` and relevant task files before working.

## Board

Use one `kanban/` directory at the level-1 workspace root. Do not create or use nested boards. The status folders are:

```text
kanban/
├── 01-backlog/
├── 02-planning/
├── 03-progress/
├── 04-blocked/
├── 05-review/
├── 06-done/
└── 07-cancelled/
```

Every task is a folder named `<number>-<slug>`, for example `01-add-oauth`. The number is unique across the board. Before creating a task, inspect every status folder, parse existing numeric prefixes, and use the next unused number; never reuse a number. Preserve historical task folders that have no numeric prefix.

Each task owns its documents. Its `readme.md` stays concise:

```markdown
# Task Name

## Goal
A concise description of the perfect result.

## Current State
Context needed to resume.

## History
- YYYY-MM-DD: Resume-worthy decision, status change, or result.
```

Task-specific plans, research, notes, and generated material belong beside that readme. A source specification may live in `docs/specs/` or elsewhere in `docs/`; link it from the task instead of treating it as task-owned documentation.

## Status permissions

| Status | Agent may do |
|---|---|
| `01-backlog` | Create the task folder and its initial `readme.md`; inspect and triage it. |
| `02-planning` | Read source specs and create or update task documents, including the single living `implementation-plan.md`. |
| `03-progress` | Implement after an explicit user command. Keep task context current. |
| `04-blocked` | Read the task and record a newly reported `Blocked by:` reason in `readme.md`; otherwise wait. |
| `05-review` | Record verification and feedback. Fixes require a user move to `03-progress`. |
| `06-done` | Answer status and history questions. |
| `07-cancelled` | Answer status and history questions. |

Creating a backlog task creates only `readme.md`. Planning belongs only in `02-planning`; use `implementation-plan.md` as the one plan and update it throughout the task.

Implementation requires both a task in `03-progress` and an explicit command such as `implement this`. If either is missing, state the missing condition.

## User-owned status moves

The user decides every status move. They may move a task manually or explicitly command a move that names the task and destination. Only then run the requested move. You may identify a suitable next status, but do not initiate a move or ask for a yes/no move decision.

If a task is blocked, cancelled, done, or in review and the user requests work outside that status's permissions, name the required destination and wait for their explicit move command.

## Task selection and creation

- If the user names a task, path, or status, search all status folders for it.
- If one task is in `03-progress` and the request is related, treat it as current.
- If no task matches project work, ask whether to create a task or which existing task to update.
- After the user confirms creation, create the numbered folder in `01-backlog` with only its initial `readme.md`.

For conversational or administrative input, answer normally. Update a task's `readme.md` only when the input changes resume-worthy context.

## Planning and source specs

Planning is permitted only for a task in `02-planning`. When a user identifies a source specification in `docs/`, read it and link it from the task. Write the task's evolving `implementation-plan.md` in its folder.

In Plan Mode, propose the relevant task action and documentation update, but do not create or edit task files. If the task is not in `02-planning`, state that the user must move it there before task planning can begin.

## Matt Pocock skills

When Matt Pocock's skills are installed and their issue-tracker configuration selects `kanban/` as the local tracker:

- Route source-spec creation to `$to-spec`. The canonical source spec is `docs/specs/<task-id>-<slug>.md`; create the corresponding backlog task with only a readme that links to it.
- Route specification decomposition to `$to-tickets`. It creates numbered, readme-only backlog tasks that link to the source spec and declare dependencies in `readme.md`.

These integrations are optional. Do not assume the skills are installed or invoke them silently; direct the user to invoke them.

## Initialize and migrate

Initialize a new board at the workspace root:

```bash
mkdir -p kanban/01-backlog kanban/02-planning kanban/03-progress \
  kanban/04-blocked kanban/05-review kanban/06-done kanban/07-cancelled
```

For an existing `.kanban/` board or an older underscore layout, inspect its folders and present the exact rename map first. Execute the migration only after the user explicitly approves it. Preserve every task folder and its current semantic status:

| Existing status | Canonical destination |
|---|---|
| `01_backlog` | `01-backlog` |
| `02_planning` | `02-planning` |
| `02_progress` or `03_progress` | `03-progress` |
| `03_review` or `04_review` | `05-review` |
| `04_done` or `05_done` | `06-done` |

Create `04-blocked` and `07-cancelled` empty. A board-root and status-folder rename preserves task status; it never reclassifies individual tasks.
