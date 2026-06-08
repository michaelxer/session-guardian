# Example: Aegis Trail With Magic Context

Use this as a model for a project instruction file when [Magic Context by CortexKit](https://github.com/cortexkit/magic-context) is installed.

```md
## Aegis Trail Magic Context Compatibility

Aegis Trail is active in Magic Context compatibility mode.

Magic Context by CortexKit owns context management, project memory, recall, historian/dreamer behavior, and compaction replacement. Keep Magic Context installed and updated from CortexKit upstream. Do not copy, vendor, fork, replace, or patch Magic Context from Aegis Trail.

Aegis Trail owns local checkpoint safety, secret-safe staging and commits, no-auto-push policy, fallback handoff, and rescue discipline.

Before stopping after meaningful project work:

1. If this project is a git repo, inspect `git status` and relevant diffs.
2. Stage only files related to the completed task.
3. Check staged content for secrets, credentials, private data, local databases, exports, and logs.
4. Commit completed intentional work locally with a concise message.
5. Create or update a lightweight handoff if stopping after important work, if the user asks, or if portable fallback state is useful.
6. Do not push unless the user explicitly approves pushing for this repo.

Never write real secrets or private customer data into source files, handoff files, plans, summaries, prompts, commits, Magic Context `ctx_memory`, or Magic Context `ctx_note`. Store private values only in ignored local files such as `.env`, `.credentials/`, or `private/`.

If context is lost, run Aegis rescue: read the latest handoff if present, inspect git status, inspect recent commits, inspect uncommitted diffs, inspect Magic Context config/tools if active, then resume from the safest pending task.
```
