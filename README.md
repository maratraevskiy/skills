# RaeVskiy Skills

Reusable agent skills for Codex and other supported coding agents.

## Included skills

| Skill | Purpose |
| --- | --- |
| [`kanban-manager`](skills/kanban-manager/SKILL.md) | Manages project work in a filesystem Kanban board at `kanban/`. |
| [`anarlog-updates`](skills/anarlog-updates/SKILL.md) | Applies Anarlog session decisions and commitments to projects. |
| [`tududi`](skills/tududi/SKILL.md) | Creates and updates high-level Tududi tasks with relevant tags. |

## Install

Install interactively and select the skills you need:

```bash
npx skills add maratraevskiy/skills
```

Install one skill explicitly:

```bash
npx skills add maratraevskiy/skills --skill kanban-manager
npx skills add maratraevskiy/skills --skill anarlog-updates
npx skills add maratraevskiy/skills --skill tududi
```

Alternatively, copy the desired directory from `skills/` into your project's
`.agents/skills/` directory.

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

## Anarlog Updates overview

`anarlog-updates` retrieves or accepts a complete meeting transcript and applies its
decisions and commitments to the project where it is invoked. It archives the
full source, updates documentation and filesystem tasks, and asks how to route
information about other projects. Supplying another project's path authorizes
the relevant update there under that project's own instructions. It is
self-contained: it prefers Anarlog Cloud MCP, falls back through cloud and local
CLI sources, and does not require the official `anarlog` skill to be installed.

## Tududi overview

`tududi` maintains high-level [Tududi](https://tududi.com/) Project Roll-ups and
Outcome Tasks against the configured Docker or hosted instance. An explicit
task request authorizes its routine read, write, tagging, and verification API
calls. It reuses relevant tags or creates up to three missing ones while
preserving existing assignments. It asks when task ownership is ambiguous or a
new Project Roll-up needs its one-time creation decision.

Runtime or sandbox permission prompts are controlled by the agent host and may
still appear even though the skill avoids repeated conversational confirmation.

## License

MIT
