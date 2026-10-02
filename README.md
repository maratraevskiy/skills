# RaeVskiy Skills

Reusable agent skills for Codex and other supported coding agents.

## Included skills

| Skill | Purpose |
| --- | --- |
| [`kanban-manager`](skills/kanban-manager/SKILL.md) | Manages project work in a filesystem Kanban board at `00-kanban/`. |
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

`kanban-manager` uses one workspace-root Board at `00-kanban/`:

```text
00-kanban/
├── 01-backlog/     ← initial readme only
├── 02-planning/    ← specs, subtasks, and one living plan
├── 03-progress/    ← implementation after an explicit command
├── 04-blocked/     ← waiting work
├── 05-review/      ← verification and feedback
├── 06-done/        ← completed history
└── 07-cancelled/   ← cancelled history
```

Each Task is a numbered folder such as `01-add-oauth`. New Tasks begin in
backlog unless you explicitly request another initial Status. Start planning in
planning Status; implementation requires progress Status and an explicit command.
You decide every Status move.

Keep a Task's spec, individual subtasks, research, notes, and one living
`implementation-plan.md` inside its folder. Subtasks live in `tickets/`, use local
numbers and explicit blockers, and inherit their parent's Status permissions.
Use `00-docs/` for shared project documentation in every project.

### Complete project setup

Package installation copies the skill files. Then ask your agent:

> Use Kanban Manager to initialize or set up this project and establish its agent instructions.

The agent creates the Board and tracker instructions and adds a pointer to the
applicable `AGENTS.md` or `CLAUDE.md`, preserving existing rules. Repeat setup
updates the same guidance. See [setup and migration](skills/kanban-manager/references/setup.md).

For an existing `kanban/`, `.kanban/`, or `docs/` layout, the agent presents an
exact migration map for your approval. It preserves Task contents and Status and
updates affected references. Your configured legacy Board remains active until
migration is approved; initializing a second Board is unnecessary.

### Matt Pocock skills

Planning uses the installed instructions for [Matt Pocock's skills](https://github.com/mattpocock/skills):
`to-spec` for specifications and `to-tickets` for approved subtask breakdowns.
Explicit-only skills require an explicit invocation; their installed instructions
can otherwise serve as references for authorized planning. Both workflows publish
inside the selected Task folder, reusing the parent Task.

Each workflow works independently. When one is missing, Kanban Manager uses its
[bundled workflow description](skills/kanban-manager/references/planning-workflows.md)
and suggests optional installation and `/setup-matt-pocock-skills`. Configure
that setup for the active Board and Task-owned publication. Missing skills never
prevent planning.

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
Outcome Tasks through a configured Tududi MCP connection. Tududi 1.0.0 or later
must have `FF_ENABLE_MCP=true`; use HTTP for Docker, hosted, or remote instances
and stdio for a local same-machine installation. An explicit task request
authorizes its routine read, write, tagging, and verification tool calls. It
reuses relevant tags or adds up to three missing ones while preserving existing
assignments. It asks when task ownership is ambiguous or a new Project Roll-up
needs its one-time creation decision.

Runtime or sandbox permission prompts are controlled by the agent host and may
still appear even though the skill avoids repeated conversational confirmation.

## License

MIT
