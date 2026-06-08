# Magic Context Compatibility

Aegis Trail is designed to complement [Magic Context by CortexKit](https://github.com/cortexkit/magic-context), not replace it.

Magic Context describes itself as self-managing context and memory for coding agents. It handles context management, project memory, recall, historian/dreamer behavior, and compaction replacement.

Aegis Trail handles a different lane: local git checkpoints, secret-safe staging and commits, no-auto-push policy, fallback handoffs, and rescue discipline.

## Upstream-First Policy

Do not copy, vendor, fork, or replace Magic Context as part of a normal Aegis Trail install.

Use CortexKit upstream for Magic Context:

```text
https://github.com/cortexkit/magic-context
```

Reasons:

- Magic Context is active infrastructure with its own packages, database, installer, plugin setup, and compatibility checks.
- Vendoring it would create a maintenance burden and turn Aegis Trail into a derivative fork by practice.
- Aegis Trail only needs stable compatibility rules, not a private copy of Magic Context source.

If Magic Context has a serious issue later, prefer pinning a known-good upstream version first. Consider a temporary fork only for urgent blocked work. Consider a long-term fork only if upstream becomes abandoned, refuses a critical fix, or requires an unavoidable deep behavior change.

## Detection

Treat Magic Context as active when any of these are present:

- `magic-context.jsonc`
- `.opencode/magic-context.jsonc`
- `@cortexkit/opencode-magic-context`
- `@cortexkit/magic-context`
- `@cortexkit/pi-magic-context`
- an `opencode.json` plugin entry for `@cortexkit/opencode-magic-context`
- Magic Context commands or tools such as `/ctx-status`, `ctx_memory`, `ctx_note`, `ctx_search`, or `ctx_expand`

When Magic Context is detected, install Aegis Trail Lite / Magic Context compatibility mode. Do not install Aegis Trail Standalone context heuristics.

## Division Of Responsibility

| Area | Owner |
| --- | --- |
| Context management | Magic Context |
| Project memory and recall | Magic Context |
| Historian and dreamer behavior | Magic Context |
| Compaction replacement | Magic Context |
| Local git checkpoints | Aegis Trail |
| Secret-safe staging and commits | Aegis Trail |
| No-auto-push default | Aegis Trail |
| Fallback handoff | Aegis Trail |
| Rescue discipline | Aegis Trail |

## Install Rules

When installing Aegis Trail into a Magic Context project:

1. Keep Magic Context installed from CortexKit upstream.
2. Install `versions/aegis-trail-lite.md` or the example in `examples/aegis-trail-with-magic-context.md` into the active project instruction file.
3. Do not install `versions/aegis-trail-standalone.md` unless the user explicitly wants duplicate manual lifecycle rules.
4. Do not edit Magic Context package files, generated files, `node_modules/`, or hidden agent internals.
5. Add missing safe ignore rules from `templates/gitignore-additions.txt`.
6. Keep auto-push disabled unless the user explicitly approves it for the current repo.

## Secret Rules For Memory

Never store real secrets or private customer data in:

- `ctx_memory`
- `ctx_note`
- Magic Context summaries
- prompts
- handoff files
- plans
- commits

Safe memory wording:

```text
Credentials are loaded from ignored `.env` files. Do not commit or memorize credential values.
```

Unsafe memory wording:

```text
OPENAI_API_KEY=<redacted>
DATABASE_URL=<redacted>
```

## Rescue With Magic Context

If a session loses context in a Magic Context project, run Aegis rescue with Magic Context awareness:

1. Read the latest handoff file if present.
2. Inspect `git status`.
3. Inspect `git log --oneline -10`.
4. Inspect uncommitted diffs.
5. Inspect Magic Context config such as `magic-context.jsonc` or `.opencode/magic-context.jsonc`.
6. Use Magic Context recall/search tools if available.
7. Reconstruct the last completed task and next pending task.
8. Continue from the safest resume point.

## Credit

Magic Context is by CortexKit. Official repository: https://github.com/cortexkit/magic-context

Aegis Trail documentation references Magic Context for compatibility and credit only. Magic Context remains an upstream dependency/tool installed separately by users who want memory and context support.
