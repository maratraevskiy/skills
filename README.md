# Kanban Manager Skill

A Codex skill for managing project work with a filesystem-based Kanban board.

It helps the agent create and maintain one `.kanban/` directory at the level-1 workspace root, with task folders organized by status:

```text
.kanban/01_backlog/
.kanban/02_planning/
.kanban/03_progress/
.kanban/04_review/
.kanban/05_done/
```

The board should not be nested inside a repository, app, package, or other subfolder.

## The folder is the permission

A task's status folder defines what the agent may do with it:

| Status | Agent may |
|---|---|
| `01_backlog` | Produce all pre-implementation artifacts, same ceiling as `02_planning`; tasks here await triage. |
| `02_planning` | Produce all pre-implementation artifacts: plans, research, drafts, brainstorming, implementation specs. |
| `03_progress` | Implement — only after an explicit implement command. |
| `04_review` | Verify and polish: log feedback, and comment, fix, or refine the existing implementation after an explicit command. New scope goes back to `03_progress`. |
| `05_done` | Answer questions from task context. |

Two rules make the statuses trustworthy:

1. **Only the user moves tasks.** The agent runs `mv` between status folders only on an explicit user command. It may observe that a task looks ready for the next status, but it never moves tasks on its own and never asks a yes/no move question.
2. **Implementation is double-gated.** Writing code requires both a task in `03_progress` (or polish work in `04_review`) *and* an explicit command like `implement this`. Status alone is not consent, and neither are words like `continue` or `proceed`.

New tasks are always created in `01_backlog` with a readme and an `initial-implementation-plan.md`. Even `create a task for X and implement it` only creates the task — implementation waits until the user moves it forward and gives the command.

## Install

### Option 1: CLI Install

Use `npx skills` to install the skill directly:

```bash
npx skills add maratraevskiy/kanban-manager
```

This installs the skill into your `.agents/skills/` directory.

### Option 2: Clone and Copy

Clone the repo and copy the skill folder manually:

```bash
git clone https://github.com/maratraevskiy/kanban-manager.git
mkdir -p .agents/skills
cp -r kanban-manager/skills/kanban-manager .agents/skills/
```

## Migrating from v1 boards

Version 2.0.0 adds the `02_planning` status and renumbers the downstream folders. When the agent detects the old 4-folder layout (`01_backlog`, `02_progress`, `03_review`, `04_done`), it offers a one-shot migration and runs it only after your confirmation:

```bash
mv .kanban/04_done .kanban/05_done
mv .kanban/03_review .kanban/04_review
mv .kanban/02_progress .kanban/03_progress
mkdir -p .kanban/02_planning
```

## Usage

Try prompts like:

```text
Initialize the kanban board.
Create a task for adding OAuth login.
Yes, help me brainstorm the implementation details.
Move the OAuth task to planning.
Draft the implementation plan for the OAuth task.
Move the OAuth task to progress.
Implement the OAuth task.
Move it to review.
Polish the error messages on the OAuth task.
What is the status of my kanban tasks?
Migrate my board to the new layout.
```

After creating a task, the skill can offer a one-question-at-a-time brainstorming flow and compile the answers into `implementation-spec.md` (available while the task is in backlog or planning).

If user intent is ambiguous, such as whether `continue`, `proceed`, or `work on this` means planning or implementation, the skill asks instead of guessing.

When asked, the skill keeps implementation details in separate task files such as `initial-implementation-plan.md`, `updates-plan.md`, and `final-implementation.md` while leaving `readme.md` as the concise task control file.

In Plan Mode, planning prompts include the Kanban task action: create a new task with `initial-implementation-plan.md` when no task exists, or update the matching task with `updates-plan.md`.

## License

MIT
