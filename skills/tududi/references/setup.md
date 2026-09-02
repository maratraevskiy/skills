# Binding a repository to Tududi

Create `.agents/tududi.json` only after the user has selected the corresponding
Tududi task, or approved creating one. The file contains no credentials and
may be committed when repository conventions allow it.

```json
{
  "version": 1,
  "base_url": "http://127.0.0.1:3002",
  "token_env": "TUDUDI_API_TOKEN",
  "task_uid": "tsk_example",
  "project_uid": "prj_example"
}
```

`project_uid` is optional. Use a task UID rather than a title: titles can
change and are not a reliable binding.

## Finding or creating the task

1. Read the repository name, configured remote when available, and existing
   Kanban task context.
2. List candidate Tududi tasks and projects from the configured instance.
   Search by an existing repository path, remote, task UID, or unambiguous
   repository name; read every plausible candidate.
3. Present the candidate and its project to the user when more than one match
   remains. Do not bind by title alone.
4. When no task exists, propose an outcome-oriented repository task and its
   intended project. Create it only after the user approves, then record the
   returned UID in the binding.

The task is a repository roll-up, not a clone of every local Kanban card. Keep
the Tududi project unchanged unless the binding explicitly names a different
project and the user has requested the correction.

## Credentials and endpoint

`token_env` defaults to Tududi's documented `TUDUDI_API_TOKEN`. Read that
variable; never print its value or write it to the repository. `base_url`
identifies the target local or hosted Tududi instance. Confirm that it is the
intended instance before the first write in a session.

## Optional macOS shell setup

For a local Tududi installation, one token in the macOS Keychain is shared by
every repository that connects to that instance; the repository binding
contains only the task UID. Hosted instances and other machines use their own
secure environment setup.

For a Keychain item named `tududi-api-token-codex-claude`, add this single line
to `~/.zshrc`:

```zsh
export TUDUDI_API_TOKEN="$(security find-generic-password -a "$USER" -s 'tududi-api-token-codex-claude' -w)"
```

Run `source ~/.zshrc` once, or open a new Terminal window. Terminal-launched
`codex` and `claude` sessions then inherit the token automatically. A desktop
app launched outside that shell needs its own secure environment setup.

The agent must verify authentication without displaying the token before its
first write. When the variable is absent or the check returns unauthorized,
report the missing local setup rather than creating, resetting, or exposing a
credential.
