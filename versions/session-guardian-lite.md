# Session Guardian Lite

Guardian Lite is the OMO-friendly edition. It assumes OMO / oh-my-openagent already handles agent orchestration, task persistence, context compaction, continuation, and handoff mechanics.

Use this version when the project uses OMO, oh-my-openagent, oh-my-opencode, or an opencode setup with strong built-in continuation.

## Always-On Rules

Use OMO for planning, tasks, continuation, handoff, and compaction.

Before ending meaningful project work:

1. Save recovery state through OMO `/handoff` or the project handoff file.
2. If a git repo exists, checkpoint completed intentional work locally.
3. Stage only files related to the completed task.
4. Check for secrets before committing.
5. Never push unless explicitly requested or this project has explicit auto-push approval.
6. Never write secrets into source files, `.omo/`, handoff docs, plans, task files, summaries, or commits.
7. Store secrets only in ignored local files such as `.credentials/`, `private/`, or `.env`.

## Meaningful Project Work

Meaningful project work includes:

- creating, editing, deleting, or moving project files
- changing dependencies or runtime setup
- changing agent/project configuration
- completing a todo item, checklist item, implementation task, or user-requested milestone
- discovering environment state that future sessions must know

Short explanations, brainstorming, and simple answers do not require a checkpoint or handoff.

## Checkpoint Protocol

After completing meaningful work in a git repo:

1. Run `git status`.
2. Review changed files.
3. Review relevant diffs.
4. Stage only intentional files related to the completed work.
5. Check staged content for obvious secrets and private data.
6. Commit locally with a concise conventional message.
7. Do not push unless explicitly approved.

Do not use blanket `git add -A` unless all changes have been verified as intentional and safe.

## Secret Protection

Never commit or summarize real secrets.

Private data belongs in ignored local files:

```text
.credentials/
private/
.env
.env.*
data/
```

Handoff files may mention where credentials are expected, but must never contain credential values.

Safe handoff wording:

```md
Credentials required:
- Set provider keys in `.env`.
- Local private notes are in `.credentials/`.
- Do not commit real credentials.
```

Unsafe handoff wording:

```md
OPENAI_API_KEY=<redacted>
DATABASE_URL=<redacted>
```

## Handoff Policy

Use OMO `/handoff` when available.

If the project uses a portable handoff folder, create or update the current session's handoff file. Do not create duplicate handoff files in the same session.

At minimum, the handoff should capture:

- user request
- goal
- work completed
- pending work
- git state
- key files
- decisions
- blockers
- continuation prompt

## Rescue Prompt

If context is lost, start a new session and say:

```text
Run Guardian rescue. Inspect git status, recent commits, uncommitted diffs, OMO `.omo/tasks`, `.omo/boulder.json`, `.omo/plans`, `.omo/notepads`, and the latest handoff. Reconstruct the resume point and continue safely.
```

## Auto-Push

Auto-push is disabled by default.

Only push when:

- the user explicitly asks, or
- this repo has explicit auto-push approval, and
- the remote and branch are approved, and
- the secret scan passes.

Prefer a private checkpoint branch such as `guardian/checkpoints` for remote backup.
