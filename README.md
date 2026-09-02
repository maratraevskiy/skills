# RaeVskiy Skills

Reusable agent skills for Codex and other supported coding agents.

## Included skills

| Skill | Purpose |
| --- | --- |
| [`kanban-manager`](skills/kanban-manager/SKILL.md) | Manages project work in a filesystem Kanban board at `kanban/`. |
| [`tududi`](skills/tududi/SKILL.md) | Synchronizes repository Kanban work and Anarlog commitments with Tududi. |

## Install

Install interactively and select one or both skills:

```bash
npx skills add maratraevskiy/skills
```

Install one skill explicitly:

```bash
npx skills add maratraevskiy/skills --skill kanban-manager
npx skills add maratraevskiy/skills --skill tududi
```

Alternatively, copy either `skills/kanban-manager` or `skills/tududi` into your
project's `.agents/skills/` directory.

## Kanban Manager overview

`kanban-manager` uses a filesystem Kanban board at `kanban/`:

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

Each task is a numbered folder such as `01-add-oauth`. Planning requires
`02-planning`; implementation requires `03-progress` and an explicit command.

### Matt Pocock skills

When [Matt Pocock's skills](https://github.com/mattpocock/skills) are installed
and configured to use `kanban/` as their local tracker, use `$to-spec` for the
canonical source spec and `$to-tickets` to split it into linked backlog tasks.

## License

MIT
