# Aegis Trail Standalone

Aegis Trail Standalone is the full lifecycle version. Use it for Codex CLI, VS Code agents, Cursor, vanilla agents, or any setup that does not already provide OMO-style task persistence, Magic Context-style memory/context management, context recovery, and handoff workflows.

Standalone is larger than Lite because it carries its own session lifecycle rules.

Do not use Standalone when Magic Context by CortexKit is active. Use Aegis Trail Lite / Magic Context compatibility mode instead, so Magic Context can own context, memory, recall, historian/dreamer behavior, and compaction replacement.

## 0. Session Definition

A session is one continuous conversation thread.

The first handoff created in a session becomes the current session's handoff file.

- If a handoff file was already created earlier in the same session, update that same file in place.
- If no handoff file has been created in the current session, create the next numbered handoff file.
- Do not create more than one handoff file per session.

## 1. Recovery Anchor Rule

Never let meaningful work end without at least one recovery anchor.

Preferred anchors:

- local git commit
- handoff file
- persisted task state
- clear final summary of uncommitted changes

Best outcome:

```text
commit + handoff + task state
```

## 2. Git Checkpoint Protocol

After completing each meaningful task, todo item, checklist item, or milestone:

1. Check whether the project has a git repo by looking for `.git/`.
2. If no git repo exists, skip git operations and do not create one unless the user explicitly asks.
3. If a git repo exists, inspect `git status` and relevant diffs.
4. Stage only files related to the completed task.
5. Search staged content for secrets and private data.
6. Commit locally with a conventional commit message.
7. Do not push unless explicitly requested or this project has explicit auto-push approval.

Recommended commit types:

```text
feat, fix, refactor, docs, chore, test
```

Do not commit incomplete work unless the user explicitly requests a checkpoint/WIP commit.

Do not use blanket `git add -A` unless all changes have been verified as intentional and safe.

## 3. Credentials Protection

When sensitive information is required, use ignored local storage.

Allowed private locations:

```text
.credentials/
private/
.env
.env.*
data/
```

Never commit:

- API keys
- access tokens
- passwords
- login credentials
- database URLs
- SMTP or IMAP credentials
- private business data
- customer or lead data
- private profiles
- local databases
- exports, imports, backups, or logs with private content

If Magic Context is active later, also never store these values in `ctx_memory`, `ctx_note`, summaries, prompts, or handoffs.

Use public-safe templates instead:

```text
.env.example
config.example.json
sample_credentials.example.json
```

If credentials are found in tracked files:

1. Stop.
2. Move the private values to ignored local storage.
3. Replace committed values with environment variables or safe examples.
4. Re-check the diff.
5. Commit the cleanup only after it is clean.

## 4. Required Ignore Rules

Ensure consuming projects ignore private and local state. Use `templates/gitignore-additions.txt` as the source of truth.

Important entries:

```gitignore
HANDOFF_DOC/
.credentials/
private/
.env
.env.*
!.env.example
data/
*.db
*.sqlite
*.sqlite3
exports/
backups/
cache/
temp/
logs/
```

Only commit sample files if they contain fake/demo data.

## 5. Context Monitoring

Skip this section when Magic Context or another context manager is active. The active context manager should own context usage, compaction replacement, recall, and memory.

If the harness provides exact context usage, use the exact percentage.

If exact usage is not available, estimate after completed work using these signals:

| Signal | Low | Medium | High | Critical |
| --- | --- | --- | --- | --- |
| Messages | <15 | 15-30 | 30-50 | 50+ |
| Todos completed | 1-3 | 4-6 | 7-10 | 10+ |
| Files touched | <10 | 10-20 | 20-35 | 35+ |
| Tool calls | <30 | 30-60 | 60-100 | 100+ |

If two or more signals are high, finish the current task, checkpoint, create/update handoff, and stop.

Never interrupt a task mid-flight only to create a handoff. Finish the current task first.

## 6. Handoff Decision Rules

Create or update a handoff when:

- context is high or critical after completing a task
- meaningful work is ending for the turn
- the user asks for a handoff
- the agent notices context degradation
- the session is about to stop after important project work

Do not start a new task after deciding to hand off.

## 7. Handoff Procedure

Use `HANDOFF_DOC/handoff-NNN.md` for portable handoffs.

If this session already has a handoff file, update that same file in place.

If this session does not have a handoff file:

1. Create `HANDOFF_DOC/` if missing.
2. List existing `handoff-NNN.md` files.
3. Pick the next number.
4. Create the new file.

Use this format:

```md
HANDOFF CONTEXT
===============

SESSION INFO
------------
- Handoff number: NNN
- Timestamp: YYYY-MM-DD HH:MM local timezone
- Context level at handoff: estimated percentage or heuristic level
- Tasks completed this session: N of M total

USER REQUESTS AS-IS
-------------------
- [Exact verbatim user requests]

GOAL
----
[One sentence objective]

WORK COMPLETED THIS SESSION
---------------------------
- [x] Completed task

WORK COMPLETED PREVIOUS SESSIONS
--------------------------------
- [x] Prior completed work, if known

PENDING TASKS
-------------
- [ ] Next task <-- RESUME HERE

GIT STATE
---------
- Branch: branch-name
- Last commit: hash "message"
- All changes committed: yes/no
- Remote push status: pushed/not pushed/not configured

KEY FILES
---------
- path - role

IMPORTANT DECISIONS
-------------------
- Decision and reason

PATTERNS AND CONVENTIONS
------------------------
- Project conventions

EXPLICIT CONSTRAINTS
--------------------
- User constraints and technical constraints

BLOCKERS AND WARNINGS
---------------------
- Known issues or None

CONTEXT FOR CONTINUATION
------------------------
- What the next session needs to know
```

After saving the handoff, output a continuation prompt:

```text
Handoff saved to HANDOFF_DOC/handoff-NNN.md.

NEXT SESSION - copy and paste this into a new chat:
Continue working on [PROJECT NAME]. Read HANDOFF_DOC/handoff-NNN.md for full context. Resume from [NEXT TASK DESCRIPTION].
```

Then stop.

## 8. Rescue Procedure

When resuming after context loss:

1. Read the latest handoff file first.
2. Inspect `git status`.
3. Inspect recent commits with `git log --oneline -10`.
4. Inspect uncommitted diff if any.
5. Reconstruct pending tasks from the handoff and project files.
6. Verify the repo state matches the handoff.
7. Continue from the resume marker.

If the handoff is missing, reconstruct from git history and uncommitted diffs.

## 9. Auto-Push Policy

Auto-push is disabled by default.

It may run only when all conditions are true:

- user explicitly enabled auto-push for this project
- remote is known and approved
- branch is known and approved
- secret scan passes
- commit is complete and intentional

Prefer pushing to a private checkpoint branch:

```text
aegis/checkpoints
```

Never auto-push to `main` unless explicitly approved.

## 10. Rules Summary

1. Commit completed meaningful work locally when a git repo exists.
2. Never create a git repo unless the user asks.
3. Never push unless explicitly approved.
4. Never commit credentials or private data.
5. Never put secrets in handoff files.
6. Keep one handoff file per session.
7. Handoff before stopping after meaningful work.
8. Stop after handoff; do not start another task.
9. Rescue from handoff, git, and task state after context loss.
10. The handoff is the source of truth for continuation.
