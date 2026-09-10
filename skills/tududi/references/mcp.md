# Tududi MCP operations

Use the configured Tududi MCP server for every Tududi read and write. Treat its
discovered tool schemas as authoritative because installed versions can differ.
The workflow requires equivalents of `search`, `list_tasks`, `get_task`,
`list_projects`, `get_project`, `create_task`, and `update_task`.

Before the first write on a connection, make a read-only call and confirm that
`update_task` accepts `description`, `project_id`, and `tags`. If the server is
missing those fields, report the required Tududi upgrade instead of using its
REST API.

Use a bound task UID directly with `get_task`. During initial binding, combine
`search` with task and project listings, then read every plausible task before
choosing. Lists have result limits, so prefer a specific search over assuming a
single broad list is exhaustive.

Send the smallest update payload. Omitted fields remain unchanged. When tags
change, first read the task and send the complete union of existing and new tag
names because `update_task.tags` replaces the collection. Task creation and
updates create missing tags while assigning them, so a separate `create_tag`
call is unnecessary.

One explicit task request authorizes the MCP sequence needed to read, write,
tag, and verify that task. Runtime permission prompts remain outside the
skill's control. Treat a tool result marked as an error as failure even when
the MCP protocol request itself succeeded.

Re-read each changed task before reporting success. If a response is missing
or uncertain, inspect current state before retrying creation or update. Retain
successful independent changes and report the task UID plus any incomplete
fields. Do not create, revoke, or delete API keys as part of a normal sync.
