# Resume From Handoff

Copy and paste this into a fresh agent session when continuing work.

```text
Continue working on this project with Session Guardian active.

First, run Guardian rescue:
- Read the latest handoff file if present.
- Inspect git status and recent commits.
- Inspect uncommitted diffs.
- If OMO is active, inspect public OMO state: `.omo/tasks`, `.omo/boulder.json`, `.omo/plans`, and `.omo/notepads`.
- Reconstruct the last completed task and next pending task.
- Do not modify files until you understand the current state.

Then resume from the next pending task.

Before stopping after meaningful work, checkpoint locally and create or update the handoff. Never commit secrets. Never push unless explicitly approved.
```
