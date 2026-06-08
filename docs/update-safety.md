# Update Safety

Session Guardian should survive OMO and host-agent updates by staying independent.

## Main Rule

Do not install Guardian inside package-managed internals.

Avoid modifying:

- OMO source files
- `node_modules/`
- generated OMO prompts
- hidden OMO internals
- package-managed plugin files

## Stable Integration Surfaces

Use stable project-level files:

```text
AGENTS.md
CLAUDE.md
CODEX.md
.cursor/rules/
.windsurf/rules/
other project instruction files
```

For opencode/OMO projects, prefer `AGENTS.md` or native project instruction files over editing OMO internals.

## Core And Adapter Pattern

Keep the core policy separate from harness-specific installation.

| Layer | Purpose |
| --- | --- |
| Guardian core | Checkpoint, secret protection, handoff, rescue rules. |
| Host adapter | Where the rule is installed for opencode, Codex, VS Code, Cursor, etc. |
| OMO adapter | Optional guidance for reading `.omo/` public state. |

If OMO changes, the core policy should not change. Only the adapter may need adjustment.

## OMO Update Checklist

After updating OMO:

1. Confirm opencode starts.
2. Confirm OMO agents appear.
3. Confirm `/handoff` still exists if you rely on it.
4. Confirm `/start-work` still exists if you rely on it.
5. Confirm `.omo/tasks` or active task state still behaves as expected.
6. Confirm Guardian Lite is still present in project instructions.
7. Confirm Guardian checkpoint rules still say not to push by default.
8. Confirm `.credentials/`, `.env`, private data, and handoff folders are ignored where appropriate.
9. Run a dummy checkpoint in a test repo before trusting automation.

## Pinning OMO

If stability is more important than latest features, pin the OMO plugin version instead of using `latest`.

If you prefer `latest`, keep Guardian independent and run the checklist after updates.

## Safe Auto-Push

Auto-push should be project opt-in only.

Allowed only when:

- user approved auto-push for this repo
- remote is known and approved
- branch is known and approved
- secret scan passes
- commit is complete and intentional

Prefer a checkpoint branch:

```text
guardian/checkpoints
```
