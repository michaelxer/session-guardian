# Installing Session Guardian

Session Guardian is installed by an LLM coding agent because every harness stores instructions differently. The agent should read the public Session Guardian repository, detect the active environment, and place the correct Guardian edition in a project-level instruction file.

Source repository:

```text
https://github.com/michaelxer/session-guardian
```

## Recommended Install Method

Paste this into your coding agent from the target project:

```text
Install and configure Session Guardian by following the instructions here:
https://raw.githubusercontent.com/michaelxer/session-guardian/refs/heads/master/INSTALL.md

Use this public repository as the source of truth:
https://github.com/michaelxer/session-guardian

Fetch and read these Session Guardian files from the public repository before editing the target project:
- README.md
- INSTALL.md
- versions/session-guardian-lite.md
- versions/session-guardian-standalone.md
- docs/update-safety.md
- examples/harness-install-examples.md
- templates/gitignore-additions.txt

Detect the active environment:
- If the project uses OMO / oh-my-openagent / oh-my-opencode, install Guardian Lite.
- If the project uses Codex CLI, VS Code agent workflows, Cursor, Claude-style agents, or no OMO continuity layer, install Guardian Standalone.

Install rules for the current project only, preferably in `AGENTS.md` or the equivalent project instruction file for the current agent harness.

Do not modify OMO internals, node_modules, generated plugin files, hidden agent internals, or package-managed files.

Add the safe ignore rules from `templates/gitignore-additions.txt` if they are missing.

Do not create a git repo unless I explicitly ask. Do not enable auto-push unless I explicitly approve it for this repo.

After installation, tell me which edition was installed, which files changed, and how to trigger checkpoint, handoff, and rescue behavior.
```

## What The Agent Should Do

1. Detect whether a project instruction file exists, such as `AGENTS.md`, `CLAUDE.md`, `CODEX.md`, or a harness-specific rules file.
2. Choose the edition:
   - Use Guardian Lite when OMO is active.
   - Use Guardian Standalone when OMO is not active or the user wants the full original workflow.
3. Add the selected Guardian text to the project instruction file.
4. Add missing `.gitignore` entries from `templates/gitignore-additions.txt`.
5. Do not place real credentials in any tracked file.
6. Do not create a git repo unless the user asks.
7. If a git repo exists, commit only intentional Guardian installation files after checking for secrets.
8. Report which edition was installed, which instruction file changed, and how the user should ask for checkpoint, handoff, and rescue behavior.

## OMO Installation Notes

For OMO, install Guardian Lite only.

OMO already provides:

- compaction and context-window recovery
- task persistence under `.omo/tasks/`
- plan/work continuation with `/start-work`
- handoff support through `/handoff`
- tool-output truncation and recovery hooks

Guardian Lite should add only the missing safety layer:

- local git checkpoint discipline
- secret/private-data protection
- handoff before stopping meaningful work
- rescue instructions after context failure

Do not paste Guardian Standalone into OMO unless you intentionally want a larger, more manual policy.

## Standalone Installation Notes

Use Guardian Standalone for agents that do not have OMO-style lifecycle management.

Good targets:

- Codex CLI
- VS Code agent workflows
- Cursor
- Claude-style project agents
- vanilla opencode without OMO
- other coding agents with project instruction files

Standalone includes its own handoff and context heuristics, so it is intentionally larger than Lite.

## Harness Examples

Use `examples/harness-install-examples.md` for practical install targets, including OMO/opencode, Codex CLI, VS Code agent workflows, Cursor, and Claude-style agents.

The examples are not separate editions. They show where the same Lite or Standalone policy usually belongs in each harness.

## Updating Session Guardian

Guardian should be update-safe because it is not installed inside OMO or inside package-managed folders.

To update:

1. Pull or download the latest Session Guardian repo.
2. Ask your coding agent to compare the installed project rules with the new version.
3. Keep project-specific local policies intact.
4. Re-run the smoke test from `docs/update-safety.md`.
