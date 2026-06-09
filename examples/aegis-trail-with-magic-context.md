# Example: Aegis Trail With Magic Context

Use this as a model for global/user agent instructions when [Magic Context by CortexKit](https://github.com/cortexkit/magic-context) is installed. It can also be used as a project-level override when explicitly requested or when the harness has no usable global instruction target.

```md
## Aegis Trail Lite + Magic Context Compatibility

Aegis Trail Lite is active with Magic Context compatibility boundaries.

Magic Context by CortexKit owns context management, project memory, recall, historian/dreamer behavior, and compaction replacement. Keep Magic Context installed and updated from CortexKit upstream. Do not copy, vendor, fork, replace, or patch Magic Context from Aegis Trail.

Aegis Trail owns local checkpoint safety, secret-safe staging and commits, no-auto-push policy, numbered portable handoff history, and rescue discipline.

Before stopping after meaningful project work:

1. If this project is a git repo, inspect `git status` and relevant diffs.
2. Stage only files related to the completed task.
3. Check staged content for secrets, credentials, private data, local databases, exports, and logs.
4. Commit completed intentional work locally with a concise message.
5. Create or update the current portable handoff at `HANDOFF_DOC/handoff-NNN.md` if stopping after important work, if the user asks, or if portable history state is useful.
6. Do not push unless the user explicitly approves pushing for this repo.

Handoff file rule: the first handoff created in a session becomes the current session's handoff file. If a handoff already exists for this session, update it in place. If not, create `HANDOFF_DOC/` if missing, list existing `handoff-NNN.md` files, and create the next number. Do not create more than one handoff file per session.

Handoffs are a numbered project history trail, not disposable summaries. Keep enough detail to track back what changed, why it changed, and which session made the decision if something goes wrong later.

Create or update the handoff automatically when meaningful work is ending, the user asks, Magic Context or the active lifecycle layer reports context pressure or compaction risk, agent-readable exact context usage is high/critical after a completed task, observable session signals show the session is clearly long or complex, or the session is about to stop after important project work. Do not ask the user for a context percentage and do not invent one. After saving the handoff, tell the user to move to a new session and include the continuation prompt. Do not start another task after handing off.

Never write real secrets or private customer data into source files, handoff files, plans, summaries, prompts, commits, Magic Context `ctx_memory`, or Magic Context `ctx_note`. Store private values only in ignored local files such as `.env`, `.credentials/`, or `private/`.

If context is lost, run Aegis rescue: read the latest handoff if present, inspect git status, inspect recent commits, inspect uncommitted diffs, inspect Magic Context config/tools if active, then resume from the safest pending task.
```
