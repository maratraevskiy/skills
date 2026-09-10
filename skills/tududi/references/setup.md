# Binding a repository to Tududi

Create `.agents/tududi.json` after an unambiguous existing Tududi task is found
or the user has approved creating one. The file contains no credentials and may
be committed when repository conventions allow it.

```json
{
  "version": 2,
  "mcp_server": "tududi",
  "task_uid": "tsk_example",
  "project_uid": "prj_example"
}
```

`project_uid` is optional. `mcp_server` is the connection name exposed by the
agent host. Use UIDs rather than titles: titles can change and are not reliable
bindings. Resolve the project's numeric ID from MCP when `create_task` or
`update_task` requires `project_id`.

For a version 1 binding containing `base_url` and `token_env`, use those fields
only to identify the intended existing MCP connection. After one unambiguous
connection is selected, rewrite the binding as version 2 without credentials.

## Finding or creating the task

1. Read the repository name, configured remote when available, and existing
   Kanban task context.
2. List candidate Tududi tasks and projects from the configured instance.
   Search by an existing repository path, remote, task UID, or unambiguous
   repository name; read every plausible candidate.
3. Bind one unambiguous existing match without another confirmation. Present
   candidates when more than one plausible match remains; do not bind by title
   alone.
4. When no task exists, propose an outcome-oriented Project Roll-up and its
   destination for one confirmation. After approval, create it and record the
   returned UID in the binding.

The task is a repository roll-up, not a clone of every local Kanban card. Keep
the Tududi project unchanged unless the binding explicitly names a different
project and the user has requested the correction.

## MCP connection

Tududi must be version 1.0.0 or later with `FF_ENABLE_MCP=true`, an API token,
and an MCP connection configured in the agent host. Use stdio when the client
can launch the Tududi server on the same machine; use streamable HTTP at
`/api/mcp` for Docker, hosted, or remote installations.

Connection configuration owns the endpoint and credential. Never copy either
into the repository binding. Reuse the bound `mcp_server` without per-session
confirmation. Ask when the connection is missing or multiple connections could
own the binding. When the server is unavailable, unauthenticated, or lacks the
required tool fields, report the setup or version incompatibility instead of
switching transports.

## Optional macOS shell setup

For a local Tududi installation, one token in the macOS Keychain can be shared
by MCP client configurations that connect to that instance. The repository
binding contains no credential. Hosted instances and other machines use their
own secure client configuration.

For a Keychain item named `tududi-api-token-codex-claude`, add this single line
to `~/.zshrc`:

```zsh
export TUDUDI_API_TOKEN="$(security find-generic-password -a "$USER" -s 'tududi-api-token-codex-claude' -w)"
```

Run `source ~/.zshrc` once, or open a new Terminal window. Terminal-launched
`codex` and `claude` sessions then inherit the token automatically. A desktop
app launched outside that shell needs its own secure environment setup.

The agent verifies access by calling a read-only Tududi MCP tool before its
first write. When the connection is absent or unauthorized, report the missing
client setup rather than creating, resetting, or exposing a credential.
