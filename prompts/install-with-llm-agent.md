# Install With An LLM Agent

Copy and paste this into the coding agent from the target project.

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

Detect this project's agent environment.

If the project uses OMO, oh-my-openagent, or oh-my-opencode:
- Install Guardian Lite.
- Rely on OMO for tasks, handoff, compaction, continuation, and recovery.
- Do not modify OMO internals, node_modules, generated plugin files, or hidden OMO prompts.

If the project uses Codex CLI, VS Code agents, Cursor, Claude-style agents, vanilla opencode, or no OMO lifecycle layer:
- Install Guardian Standalone.

Prefer project-level instruction files such as AGENTS.md, CLAUDE.md, CODEX.md, or the harness-specific project rules file. Use examples/harness-install-examples.md as guidance for where the selected edition should go.

Add missing ignore rules from templates/gitignore-additions.txt.

Do not create a git repo unless I explicitly ask.

If a git repo already exists:
- Inspect status and diffs.
- Stage only intentional installation files.
- Check for secrets.
- Commit locally.
- Do not push unless I explicitly approve pushing for this repo.

After installation, tell me which edition was installed, which files changed, and how to trigger checkpoint, handoff, and rescue behavior.
```
