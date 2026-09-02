# Anarlog sessions

Use this reference when a Tududi sync includes a meeting, transcript, decision,
or commitment. Anarlog is read-only evidence; Tududi is the execution record.

## Archive the source session

Use a user-supplied canonical transcript when present. Otherwise, when no
session UUID was supplied, list selectable completed meetings and let the user
choose; never select the newest meeting by default.

```sh
anarlog --json meetings list --limit 200
anarlog --json meetings get <source-session-uuid>
anarlog --json meetings transcript <source-session-uuid> --limit 500 --offset <offset>
```

Continue transcript requests until `next_offset` is null. Create or refresh
`docs/anarlog/<source-session-uuid>.md`, unless the repository already names a
different canonical location. It contains frontmatter with the source UUID,
meeting date, and Project Assignment; then the session summary and complete
transcript body.

**Complete when:** the canonical transcript contains all session evidence and
its source UUID and assignment are known.

## Extract and route candidates

Make one candidate for every independently deliverable, evidence-backed
outcome. Record its intended result, decision or commitment, constraints,
owner or dependency, Project Assignment, source UUID, and transcript path.
Background discussion, status reports, and unowned questions are evidence, not
candidates.

For each candidate, search Tududi by source UUID, Project Assignment, outcome
terms, and existing task links. Read every plausible match before deciding its
destination:

- Update the existing task when it already owns the outcome.
- Propose a normal Tududi task when the outcome is distinct.
- Leave an ambiguous candidate unpublished and state the ownership decision
  required.

Use an explicit Project Assignment for a new task. Leave it unassigned when
none is established. A task need not belong to the current repository; normal
tasks are never subtasks of its roll-up.

**Complete when:** every candidate has exactly one destination or an explicit
ownership decision.

## Specify and publish work

Every new task, and each incomplete task receiving material new evidence,
contains these sections:

```markdown
## Context
## Problem Statement
## Solution
## User Stories
## Implementation Decisions
## Testing Decisions
## Definition of Done
## Action Plan
## Out of Scope
## Further Notes
```

`Context` identifies the Project Assignment, source UUID, session date, and
canonical transcript path. Each user story uses `As an <actor>, I want a
<feature>, so that <benefit>`. Definition of Done is observable; Action Plan is
an ordered checklist that reaches it. Preserve human decisions and add dated
evidence notes only where new evidence changes the task.

Show proposed creates, updates, and unpublished candidates. An explicit sync
may update a known matching task; obtain user approval before creating any new
task. Re-read every changed task and confirm its specification, assignment,
and transcript link.
