# Example: Aegis Trail Lite In AGENTS.md

Use this as a model for an OMO, oh-my-openagent, oh-my-opencode, Magic Context, or strong continuity-layer project instruction file.

```md
## Aegis Trail Lite

Aegis Trail Lite is active for this project.

Use the active lifecycle layer for planning, task persistence, continuation, handoff, compaction, memory, and recovery. Do not modify OMO internals, Magic Context internals, generated prompts, `node_modules/`, or package-managed plugin files.

If Magic Context by CortexKit is active, let Magic Context own context management, project memory, recall, historian/dreamer behavior, and compaction replacement. Aegis Trail only adds git checkpoint, secret safety, fallback handoff, and no-auto-push rules.

Before stopping after meaningful project work:

1. Use the active lifecycle handoff if available, such as OMO `/handoff`, or update the project's current handoff file.
2. If this project is a git repo, inspect `git status` and relevant diffs.
3. Stage only files related to the completed task.
4. Check staged content for secrets, credentials, private data, local databases, exports, and logs.
5. Commit completed intentional work locally with a concise message.
6. Do not push unless the user explicitly approves pushing for this repo.

Never write real secrets into source files, `.omo/`, Magic Context `ctx_memory`, Magic Context `ctx_note`, handoff files, plans, summaries, prompts, or commits. Store private values only in ignored local files such as `.env`, `.credentials/`, or `private/`.

If context is lost, run Aegis rescue: read the latest handoff, inspect git status, inspect recent commits, inspect uncommitted diffs, inspect public OMO state if present, inspect Magic Context config/tools if active, then resume from the safest pending task.
```
