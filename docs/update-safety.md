# Update Safety

Aegis Trail should survive OMO, Magic Context, and host-agent updates by staying independent.

## Main Rule

Do not install Aegis Trail inside package-managed internals.

Avoid modifying:

- OMO source files
- `node_modules/`
- generated OMO prompts
- hidden OMO internals
- Magic Context source/package files
- Magic Context generated files
- package-managed plugin files

## Stable Integration Surfaces

Use stable global/user-level instruction files when the harness supports them:

```text
OpenCode user/global instructions
Codex user/global instructions
Claude user/global instructions
Cursor user/global rules
VS Code extension user/global instructions
```

For OpenCode, OMO, or Magic Context environments, prefer user/global instruction files over editing OMO or Magic Context internals. Use project instruction files only as an explicit project-specific override or when the harness has no usable global instruction target.

## Core And Adapter Pattern

Keep the core policy separate from harness-specific installation.

| Layer | Purpose |
| --- | --- |
| Aegis Trail core | Checkpoint, secret protection, handoff, rescue rules. |
| Host adapter | Where the rule is installed for opencode, Codex, VS Code, Cursor, etc. |
| OMO adapter | Optional guidance for reading `.omo/` public state. |
| Magic Context adapter | Compatibility guidance that lets Magic Context own context and memory. |

If OMO or Magic Context changes, the core policy should not change. Only the adapter may need adjustment.

## OMO Update Checklist

After updating OMO:

1. Confirm opencode starts.
2. Confirm OMO agents appear.
3. Confirm `/handoff` still exists if you rely on it.
4. Confirm `/start-work` still exists if you rely on it.
5. Confirm `.omo/tasks` or active task state still behaves as expected.
6. Confirm Aegis Trail Lite is still present in global/user instructions.
7. Confirm Aegis Trail checkpoint rules still say not to push by default.
8. Confirm `.credentials/`, `.env`, private data, and handoff folders are ignored where appropriate.
9. Run a dummy checkpoint in a test repo before trusting automation.

## Magic Context Update Checklist

After updating Magic Context:

1. Run Magic Context's upstream setup or doctor workflow if needed.
2. Confirm Magic Context still owns context management, memory, recall, historian/dreamer behavior, and compaction replacement.
3. Confirm Aegis Trail Lite + Magic Context Compatibility is still present in global/user instructions.
4. Confirm no Aegis Trail Standalone context heuristics were added on top of Magic Context.
5. Confirm Aegis Trail still says not to push by default.
6. Confirm the project rules forbid secrets in `ctx_memory`, `ctx_note`, summaries, prompts, handoffs, and commits.
7. Confirm `.credentials/`, `.env`, private data, and handoff folders are ignored where appropriate.
8. Run a dummy checkpoint in a test repo before trusting automation.

## Pinning OMO

If stability is more important than latest features, pin the OMO plugin version instead of using `latest`.

If you prefer `latest`, keep Aegis Trail independent and run the checklist after updates.

## Pinning Magic Context

If Magic Context stability is more important than latest features, pin a known-good upstream version.

Do not vendor Magic Context into Aegis Trail by default. A temporary fork should be reserved for urgent blocked work, and a long-term fork should be reserved for abandoned upstream or refused critical fixes.

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
aegis/checkpoints
```
