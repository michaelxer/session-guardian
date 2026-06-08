# /aegis-handoff

Use this workflow before stopping after meaningful project work or when context is getting risky.

## Goal

Save enough state for a fresh agent session to resume safely.

## Lifecycle-Aware Mode

If OMO is active, prefer OMO `/handoff` and include relevant OMO state:

```text
.omo/tasks/
.omo/boulder.json
.omo/plans/
.omo/notepads/
```

Do not modify OMO internals.

If Magic Context is active, let Magic Context own durable memory and recall. Use this handoff as a portable fallback and do not write secrets or private customer data into `ctx_memory`, `ctx_note`, summaries, prompts, or handoff files.

## Portable Mode

If using portable Aegis Trail handoffs, write to:

```text
HANDOFF_DOC/handoff-NNN.md
```

Within one session, update the same handoff file instead of creating a new number.

## Required Content

Include:

- exact user request
- project goal
- completed work
- pending work with resume marker
- git branch and last commit
- uncommitted changes, if any
- key files
- decisions
- patterns and conventions
- blockers and warnings
- continuation prompt

## Secret Rule

Handoff files may reference secret locations but must not contain secret values.

Safe:

```md
Set `OPENAI_API_KEY` in `.env`.
```

Unsafe:

```md
OPENAI_API_KEY=<redacted>
```
