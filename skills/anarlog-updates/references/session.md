# Anarlog session retrieval

This workflow is self-contained. Use the official Anarlog skill when it is
available, but do not require it.

## Choose the source

Use a user-supplied canonical transcript first. Otherwise:

1. Use connected Anarlog Cloud MCP tools first: `list_meetings`, `get_meeting`,
   `get_meeting_transcript`, and, when relevant,
   `get_recurring_meeting_history`.
2. If Cloud MCP is unavailable and CLI login is available, use the CLI with
   `--source cloud` for hosted snapshots.
3. If Cloud has no match, snapshots are disabled, or the meeting exists only on
   this machine, use the local CLI commands without `--source cloud`.
4. A local `anarlog mcp` stdio server may fill the same local gaps. Treat Cloud
   and local results as alternative sources for one meeting, not two sources of
   truth.
5. If neither source is available, direct the user to Anarlog's Cloud API and
   connector or installation documentation. Install software only when asked.

Use only Anarlog's MCP and CLI interfaces. They own application-schema
compatibility; never query or modify Anarlog's SQLite database directly.

## Select and retrieve the meeting

Search or list meetings, use an ID returned by that operation, and never guess
an ID. When no ID was supplied and results are ambiguous, ask the user to
choose. Retrieve meeting details before its transcript: notes, summary,
participants, and action items help classify the evidence. Retrieve recurring
history when earlier meetings in the series can change the Project Update.

Representative CLI commands are:

```sh
anarlog --json meetings list --limit 200
anarlog --json meetings get <meeting-id>
anarlog --json meetings transcript <meeting-id> --limit 200 --offset <offset>
anarlog --json meetings history <meeting-id>
```

Add `--source cloud` after `meetings` when reading hosted snapshots through the
CLI. JSON transcript responses include `pagination.next_offset`. Request pages
of at most 500 words and follow `next_offset` until it is null: a Project Update
requires the complete transcript even when meeting details answer part of the
question.

If Cloud returns no meeting, search locally before reporting it missing. Ground
meeting titles, dates, IDs, participants, and content only in source output that
was actually retrieved. Report whether evidence came from Cloud, the local
database through an approved interface, or a supplied transcript.

## Archive safely

Treat meeting content as private user data. Project writes authorized by
`anarlog-updates` may contain relevant evidence; sending content to another
service or person requires explicit authorization.

Create or refresh the complete Session Archive at the project's established
location. When none exists, use `docs/anarlog/<meeting-id>.md`. Include the
source meeting ID, date, and source as frontmatter, followed by available
meeting context and the complete transcript. Preserve earlier annotations that
remain useful when refreshing the source.

Prefer assembling paged JSON results into the archive. If CLI export is used,
never pass `--force` unless the user explicitly approved replacing that exact
path. Cloud MCP is read-only; Anarlog edits, when requested separately, must be
staged through supported local proposal tools for human acceptance.

Before relying on different commands or fields, consult current Anarlog CLI and
MCP documentation.
