# Planning workflows

Bundled descriptions of Matt Pocock's `to-spec` and `to-tickets` workflows, adapted to Task-owned publication. Read only the section for a missing workflow. The main skill owns Board location, Status permissions, and the publication contract.

## to-spec: synthesize a specification

**Input:** the selected Task, conversation decisions, existing requirements, project vocabulary, and relevant architectural decisions.

1. Read the available context and inspect the project where needed. Synthesize what is established; avoid restarting the discussion as an interview. Flag a material unresolved requirement in the spec instead of inventing a settled decision.
2. Identify the highest practical verification seam. Prefer an existing project-level seam and tests of observable behavior. Describe the proposed checks so the user can review them; discuss a new seam when its choice needs a decision.
3. Write the Task's spec with these sections:
   - **Problem Statement:** the user's problem and current limitations.
   - **Solution:** the behavior the user will get.
   - **User Stories:** an extensive numbered list in the form “As an actor, I want a feature, so that a benefit.” Cover the feature's distinct branches and boundaries.
   - **Implementation Decisions:** settled interfaces, responsibilities, interactions, and architectural choices. Keep incidental file paths and code snippets out of the decisions.
   - **Testing Decisions:** observable verification, the modules or workflows covered, the seam, and relevant prior art.
   - **Out of Scope:** the feature's agreed limits.
   - **Further Notes:** relevant provenance and unresolved decisions.
4. Apply `ready-for-agent` triage in the Task readme and link its spec. Completion means all sections cover the agreed scope and the spec is stored with the selected Task.

## to-tickets: decompose into Subtasks

**Input:** a Task's full spec, plan, or established conversation context, including relevant feedback and dependencies.

1. Read the input and project vocabulary. Inspect relevant project material when needed to understand the work.
2. Draft tracer bullets: narrow, complete slices delivering independently verifiable behavior. Each slice should fit in a fresh context window. Include its documentation and verification rather than splitting them into horizontal layers. Put necessary prefactoring first.
3. Give each slice only its genuine blocking edges. For a wide mechanical refactor that cannot land in independent slices, use expand–contract: add the new form, migrate in bounded batches, then remove the old form. If batches need a shared integration branch, state where verification becomes possible.
4. Present a numbered proposal with each Subtask's title, blockers, and delivered behavior. Ask whether granularity and blocking edges are right and whether any slices should be merged or split. Publish after the user approves the breakdown; an approval already given in the session is sufficient.
5. Write one document per approved Subtask, numbered from `01` in dependency order. Use this shape:

```markdown
# 01: Subtask title

**What to build:** The observable behavior this slice delivers.

**Blocked by:** Local Subtask numbers and titles, or None (can start immediately).

**Status:** ready-for-agent

- [ ] Observable acceptance criterion
- [ ] Verification criterion
```

Keep incidental implementation paths and code snippets out of the Subtasks. Use the parent Task's spec and context instead of duplicating its full requirements. Completion means each approved slice has a separate document, acceptance criteria, and resolvable blockers, with no dependency cycle. The frontier consists of open Subtasks whose blockers are done; readiness never overrides the parent's Status permissions.
