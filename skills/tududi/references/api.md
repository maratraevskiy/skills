# Tududi API use

Treat the target instance's Swagger UI at `<base_url>/api` as the API contract:
Tududi versions can differ. Confirm authentication plus project, task, and
tag/label routes and request schemas there before the first write to an
installation. The installed contract is authoritative.

The current public integration examples use bearer authentication and expose
task listing and creation at:

```text
GET  <base_url>/api/v1/tasks
POST <base_url>/api/v1/task
Authorization: Bearer <token>
Content-Type: application/json
```

The Docker installation inspected when this skill was authored also exposes
the versioned equivalents below. Its task create and patch bodies accept
`tags` as an array of objects containing `name`:

```text
GET   <base_url>/api/v1/tags
POST  <base_url>/api/v1/tag
GET   <base_url>/api/v1/task/<id-or-uid>
PATCH <base_url>/api/v1/task/<id-or-uid>
```

Confirm these paths in Swagger because another installed version may differ.

Use the returned task UID as the binding identifier. Send the smallest payload
that changes the intended fields and retain unknown fields. Before assigning
tags, determine whether the installed operation appends or replaces the tag
collection; include existing identifiers when replacement semantics apply.

One explicit task request authorizes the API sequence needed to read, write,
tag, and verify that task. Group operations when the installed API supports it;
do not introduce conversational approval between routine calls. Runtime or
sandbox approval prompts remain outside the skill's control.

Re-read changed records before reporting success. If a response is missing or
uncertain, inspect current state before retrying creation. Preserve successful
task writes when tagging fails and report the task UID and incomplete tag work.
Do not create, revoke, or delete API keys as part of a normal sync. Respect
rate-limit responses and retry only when the response provides a safe retry
time.
