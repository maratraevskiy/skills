# Kanban Manager Skill

A Codex skill for managing project work in a filesystem Kanban board at `kanban/`.

```text
kanban/
├── 01-backlog/     ← create numbered task folders with only readme.md
├── 02-planning/    ← task documents and one implementation-plan.md
├── 03-progress/    ← implementation after an explicit command
├── 04-blocked/     ← waiting work
├── 05-review/      ← verification and feedback
├── 06-done/        ← completed history
└── 07-cancelled/   ← cancelled history
```

Each task is a numbered folder such as `01-add-oauth`. The agent checks every status folder before creation and uses the next unused number. Task-specific material stays in the task folder; a shared source spec may live in `docs/specs/` or `docs/` and is linked from the task.

Only you move tasks, either manually or by an explicit move command. Planning is allowed only in `02-planning`; it uses one evolving `implementation-plan.md`. Implementation requires both `03-progress` and an explicit command such as `implement this`.

## Matt Pocock skills

When [Matt Pocock's skills](https://github.com/mattpocock/skills) are installed and configured to use `kanban/` as their local tracker:

- Run `$to-spec` to create a canonical source spec at `docs/specs/<task-id>-<slug>.md` and a linked, readme-only backlog task.
- Run `$to-tickets` to split that spec into numbered, linked backlog tasks with dependency records.

The Kanban skill remains usable without those optional skills.

## Install

```bash
npx skills add maratraevskiy/kanban-manager
```

Or copy `skills/kanban-manager` into your project's `.agents/skills/` directory.

## Migrate an existing board

The agent first inspects the old board and shows the exact rename map. It performs no migration until you explicitly approve it. Migration renames `.kanban/` to `kanban/`, maps legacy status names to the seven current folders, and preserves all task folders.

## Usage

```text
Initialize the kanban board.
Create a task for adding OAuth login.
Move 01-add-oauth to planning.
Create an implementation plan from docs/specs/01-add-oauth.md.
Move 01-add-oauth to progress.
Implement 01-add-oauth.
Move 01-add-oauth to review.
```

## License

MIT
