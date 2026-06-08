# /guardian-rescue

Use this workflow in a new session after context was lost, the model hit a context error, or the previous agent stopped without a clear handoff.

## Goal

Reconstruct the previous working state from durable anchors.

## Steps

1. Read the latest handoff file if present.
2. Inspect `git status`.
3. Inspect `git log --oneline -10`.
4. Inspect uncommitted diffs.
5. If OMO is active, inspect public OMO state:
   - `.omo/tasks/`
   - `.omo/boulder.json`
   - `.omo/plans/`
   - `.omo/notepads/`
6. Identify the last completed task.
7. Identify the next pending task.
8. Confirm whether all completed work is committed.
9. Continue from the safest resume point.

## If No Handoff Exists

Reconstruct from git:

- latest commits
- uncommitted diffs
- modified files
- project task files
- test/build output if available

Then create a new handoff before continuing substantial work.
