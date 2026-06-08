# Installing Aegis Trail

Aegis Trail is installed by an LLM coding agent because every harness stores instructions differently. The agent should read the public Aegis Trail repository, detect the active environment, and place the correct Aegis Trail edition in a project-level instruction file.

Source repository:

```text
https://github.com/michaelxer/session-guardian
```

## Recommended Install Method

Paste this into your coding agent from the target project:

```text
Install and configure Aegis Trail by following the instructions here:
https://raw.githubusercontent.com/michaelxer/session-guardian/refs/heads/master/INSTALL.md

Use this public repository as the source of truth:
https://github.com/michaelxer/session-guardian

Fetch and read these Aegis Trail files from the public repository before editing the target project:
- README.md
- INSTALL.md
- versions/aegis-trail-lite.md
- versions/aegis-trail-standalone.md
- docs/magic-context-compatibility.md
- docs/update-safety.md
- examples/harness-install-examples.md
- templates/gitignore-additions.txt

Detect the active environment:
- If the project uses Magic Context by CortexKit, install Aegis Trail Lite / Magic Context compatibility mode. Do not install Standalone context heuristics.
- If the project uses OMO / oh-my-openagent / oh-my-opencode, install Aegis Trail Lite.
- If the project uses Codex CLI, VS Code agent workflows, Cursor, Claude-style agents, or no continuity/context layer, install Aegis Trail Standalone.
- If another continuity or memory layer is active and the right choice is unclear, ask me before editing.

Install rules for the current project only, preferably in `AGENTS.md` or the equivalent project instruction file for the current agent harness.

Do not modify OMO internals, Magic Context internals, node_modules, generated plugin files, hidden agent internals, or package-managed files.

Do not copy, vendor, fork, or replace Magic Context. If Magic Context is needed, use the upstream CortexKit project separately.

Add the safe ignore rules from `templates/gitignore-additions.txt` if they are missing.

Do not create a git repo unless I explicitly ask. Do not enable auto-push unless I explicitly approve it for this repo.

After installation, tell me which edition was installed, which files changed, and how to trigger checkpoint, handoff, and rescue behavior.
```

## What The Agent Should Do

1. Detect whether a project instruction file exists, such as `AGENTS.md`, `CLAUDE.md`, `CODEX.md`, or a harness-specific rules file.
2. Detect whether Magic Context is active by looking for `magic-context.jsonc`, `.opencode/magic-context.jsonc`, `@cortexkit/opencode-magic-context`, `@cortexkit/magic-context`, `@cortexkit/pi-magic-context`, or an `opencode.json` plugin entry for Magic Context.
3. Choose the edition.
4. Use Aegis Trail Lite / Magic Context compatibility mode when Magic Context is active.
5. Use Aegis Trail Lite when OMO or another strong continuity layer is active.
6. Use Aegis Trail Standalone when no continuity/context layer is active or the user wants the full original workflow.
7. Add the selected Aegis Trail text to the project instruction file.
8. Add missing `.gitignore` entries from `templates/gitignore-additions.txt`.
9. Do not place real credentials in any tracked file or Magic Context memory/note.
10. Do not create a git repo unless the user asks.
11. If a git repo exists, commit only intentional Aegis Trail installation files after checking for secrets.
12. Report which edition was installed, which instruction file changed, and how the user should ask for checkpoint, handoff, and rescue behavior.

## Magic Context Installation Notes

Magic Context is an upstream CortexKit project: https://github.com/cortexkit/magic-context

Aegis Trail does not ship Magic Context. Install and update Magic Context from CortexKit upstream when you need context and memory support.

When Magic Context is active, Aegis Trail should not add competing context-management instructions. Do not add Standalone context heuristics, duplicate compaction rules, or instructions that fight Magic Context's historian, dreamer, recall, or compaction replacement behavior.

Aegis Trail should add only the safety layer:

- local git checkpoint discipline
- secret/private-data protection for files, commits, handoffs, `ctx_memory`, and `ctx_note`
- no-auto-push defaults
- fallback handoff/rescue discipline

If Magic Context reports a setup conflict, use Magic Context's upstream setup or doctor guidance. Do not patch Magic Context source files from Aegis Trail.

## OMO Installation Notes

For OMO, install Aegis Trail Lite only.

OMO already provides:

- compaction and context-window recovery
- task persistence under `.omo/tasks/`
- plan/work continuation with `/start-work`
- handoff support through `/handoff`
- tool-output truncation and recovery hooks

Aegis Trail Lite should add only the missing safety layer:

- local git checkpoint discipline
- secret/private-data protection
- handoff before stopping meaningful work
- rescue instructions after context failure

Do not paste Aegis Trail Standalone into OMO unless you intentionally want a larger, more manual policy.

## Standalone Installation Notes

Use Aegis Trail Standalone for agents that do not have OMO-style lifecycle management or Magic Context-style memory/context management.

Good targets:

- Codex CLI
- VS Code agent workflows
- Cursor
- Claude-style project agents
- vanilla opencode without OMO or Magic Context
- other coding agents with project instruction files

Standalone includes its own handoff and context heuristics, so it is intentionally larger than Lite.

## Harness Examples

Use `examples/harness-install-examples.md` for practical install targets, including Magic Context, OMO/opencode, Codex CLI, VS Code agent workflows, Cursor, and Claude-style agents.

The examples are not separate editions. They show where the same Lite, compatibility, or Standalone policy usually belongs in each harness.

## Updating Aegis Trail

Aegis Trail should be update-safe because it is not installed inside OMO, Magic Context, or package-managed folders.

To update:

1. Pull or download the latest Aegis Trail repo.
2. Ask your coding agent to compare the installed project rules with the new version.
3. Keep project-specific local policies intact.
4. Re-run the smoke test from `docs/update-safety.md`.
