# Project setup and migration

Read this reference for initialization, installation/setup, missing project instructions, or legacy directories. Installing a skill package copies its files; project setup is performed by an agent using the installed skill.

## Set up the project

1. Inspect the workspace root, existing Boards and shared-doc directories, and project `AGENTS.md` and `CLAUDE.md`. Retain the configured active Board while any migration awaits approval.
2. For a fresh project, create the canonical Board with all seven Status folders and `00-docs/agents/`. If legacy directories exist, use the migration procedure below before creating a competing layout.
3. Create or update the project's issue-tracker document in the active shared-doc directory. Use the template below, adjusting the Board and docs paths only when the project is still awaiting migration. Keep existing project-specific rules that remain compatible.
4. Add the pointer block below to the applicable instruction file: update an existing `AGENTS.md` for Codex or `CLAUDE.md` for Claude; create the host's standard file if absent. For a project supporting both hosts, update both entrypoints. Preserve unrelated text and replace an existing Kanban block rather than appending a duplicate.
5. Check that the pointer resolves, the tracker names the actual active Board, and repeat setup would leave one coherent instruction block. Setup is complete when the Board and tracker exist and every applicable agent entrypoint reaches the tracker.

### Agent pointer block

```markdown
<!-- kanban-manager:start -->
## Task workflow

For Task creation, planning, implementation, review, status, moves, or publication, use the installed `kanban_manager` skill and read `00-docs/agents/issue-tracker.md` before acting. Keep specs and Subtasks inside their parent Task folder; use `00-docs/` for shared project documentation.
<!-- kanban-manager:end -->
```

### Issue-tracker template

```markdown
# Issue tracker: Filesystem Kanban

Active Board: `00-kanban/` at the workspace root.
Shared project documentation: `00-docs/`.

Use the installed `kanban_manager` skill for Status permissions and setup/migration.

- Status folders: `01-backlog`, `02-planning`, `03-progress`, `04-blocked`, `05-review`, `06-done`, `07-cancelled`.
- A Task is a numbered folder inside a Status; choose the next identifier across the whole Board and preserve historical unnumbered Tasks.
- The Task readme records goal, current state, history, and artifact pointers. Backlog intake creates only this readme; an explicitly requested initial planning Status permits planning artifacts.
- Start planning in planning Status. Implementation requires progress Status and an explicit user command. Only the user decides Status moves.
- Keep the spec, Subtasks, living Implementation Plan, research, and notes in the Task folder. Subtasks inherit the parent's Status permissions.
- When `to-spec` publishes, write the selected Task's spec and update its triage to `ready-for-agent`; reuse the existing Task.
- When `to-tickets` publishes, write separate approved Subtask documents inside the parent Task, with local numbers, acceptance criteria, readiness, and blockers. A completed Subtask records verification and `done` readiness.
- Use each installed planning skill's instructions independently, respecting explicit invocation settings. Use Kanban Manager's bundled description for each missing workflow and suggest optional Matt Pocock skill installation and setup.
- Fetch a referenced Task or Subtask by reading its document and relevant parent Task Context. Subtasks are ready to work only when their blockers are done and the parent permits the work.
```

## Migrate an existing layout

Inspect before proposing changes. List every source and destination, content collision, and affected project reference. Present the exact rename map and wait for explicit approval before execution. An implementation request for the skill alone does not approve directory migration.

| Existing directory | Canonical destination |
|---|---|
| `kanban/` or `.kanban/` | `00-kanban/` |
| `docs/` | `00-docs/` |
| `01_backlog` | `01-backlog` |
| `02_planning` | `02-planning` |
| `02_progress` or `03_progress` | `03-progress` |
| `03_review`, `04_review`, or `05_review` | `05-review` |
| `04_done`, `05_done`, or `06_done` | `06-done` |
| `04_blocked` | `04-blocked` |
| `07_cancelled` | `07-cancelled` |

Inspect any other historical Status name and map it by semantic Status. Already canonical Status names stay unchanged. Add missing canonical Status folders empty; preserve every Task's semantic Status, identifier, and contents.

If old and new roots coexist, inspect both and propose a content-level merge or other concrete resolution that preserves all Tasks and docs; include duplicate Task identifiers and naming collisions in the proposal. Execute only the approved operations, then update active tracker configuration, agent pointers, domain-doc pointers, and project-local links affected by the rename. Preserve historical records as history; refresh their links where needed to remain navigable.

Verify the resulting directory inventory and contents against the inspected originals, resolve updated pointers, and record the migration result. Completion means one configured active Board, preserved content and semantic Status, and working project references. Until then, report the actual active layout.
