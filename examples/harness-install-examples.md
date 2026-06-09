# Harness Install Examples

These examples show where an LLM coding agent should usually install Aegis Trail. They are not separate editions.

For every harness below, the agent should read `INSTALL.md`, detect the project, ask the required scenario-specific setup questions, and wait for the user's answers before editing files. After all required answers are complete, the agent should start the install immediately.

Normal installation edits project instruction and ignore files only. Do not create a git repo, commit installation changes, or push during installation unless the user explicitly requested that separate git action.

Do not offer Magic Context as a normal setup choice for every harness. CortexKit Magic Context is currently documented for OpenCode and Pi. For Codex CLI, VS Code agents, Cursor, Claude-style agents, or other non-OpenCode/non-Pi harnesses, recommend Aegis Trail Standalone unless another continuity/context layer is already active or the user explicitly asks to check CortexKit upstream first.

## Magic Context By CortexKit

Recommended edition: Aegis Trail Lite / Magic Context compatibility mode.

Preferred install target:

```text
AGENTS.md
```

Use this prompt from the target project:

```text
Install Aegis Trail Lite / Magic Context compatibility mode for this project. Read the Aegis Trail repo first. Keep Magic Context by CortexKit responsible for context management, memory, recall, historian/dreamer behavior, and compaction replacement. Do not install Aegis Trail Standalone context heuristics. Do not copy, vendor, fork, replace, or patch Magic Context. Add Aegis Trail rules for local git checkpoints, secret-safe staging and commits, no-auto-push defaults, a numbered HANDOFF_DOC/handoff-NNN.md trackback trail, and rescue discipline. Never write real secrets or private customer data into ctx_memory, ctx_note, summaries, prompts, handoffs, or commits. Add missing ignore rules. Do not create a repo, commit, or push installation changes unless I explicitly ask.
```

Expected result:

```text
Aegis Trail Lite / Magic Context compatibility mode installed in project instructions.
Magic Context remains the context and memory manager.
Aegis Trail adds checkpoint, secret, numbered handoff history, and push-safety discipline.
```

## OMO / Oh-My-Openagent / Oh-My-Opencode

Recommended edition: Aegis Trail Lite.

Preferred install target:

```text
AGENTS.md
```

Use this prompt from the target project:

```text
Install Aegis Trail Lite for this OMO project. Read the Aegis Trail repo first. Add the Lite rules to AGENTS.md or the existing project instruction file. Do not modify OMO internals, generated prompts, node_modules, package-managed files, or hidden agent internals. Keep OMO responsible for tasks, /handoff, /start-work, compaction, and continuation. Add the numbered HANDOFF_DOC/handoff-NNN.md trackback trail and missing ignore rules for HANDOFF_DOC/, .credentials/, private/, .env, local databases, exports, backups, cache, temp, and logs. Do not create a repo, commit, or push installation changes unless I explicitly ask.
```

Expected result:

```text
Aegis Trail Lite installed in project instructions.
OMO remains the lifecycle manager.
Aegis Trail adds checkpoint, secret, numbered handoff history, and rescue discipline.
```

## Opencode Without OMO

Recommended edition: Aegis Trail Standalone for full lifecycle management, or Aegis Trail Lite if the project already has another strong continuation system.

Preferred install target:

```text
AGENTS.md
```

Use this prompt from the target project:

```text
Install Aegis Trail for this opencode project. If there is no OMO, Magic Context, or equivalent continuity layer, install Aegis Trail Standalone in AGENTS.md. If a continuity/context layer already handles handoff, compaction, memory, and task persistence, install Aegis Trail Lite instead. Add the numbered HANDOFF_DOC/handoff-NNN.md trackback trail and missing ignore rules. Do not modify hidden opencode internals or generated/package-managed files. Do not create a repo, commit, or push installation changes unless I explicitly ask.
```

## Codex CLI

Recommended edition: Aegis Trail Standalone.

Preferred install target:

```text
AGENTS.md
```

Use this prompt from the target project:

```text
Install Aegis Trail Standalone for this Codex CLI project. Add the Standalone rules to AGENTS.md or the active project instruction file. The rules must require local checkpoints after meaningful completed work, one handoff file per session before stopping, secret-safe diffs, no auto-push by default, and Aegis rescue after context loss. Add missing ignore rules. Do not create a repo, commit, or push installation changes unless I explicitly ask.
```

## VS Code Agent Workflows

Recommended edition: Aegis Trail Standalone.

Preferred install target depends on the extension or agent. Use the project-level instruction file the active agent reads.

Common targets:

```text
AGENTS.md
.github/copilot-instructions.md
CLAUDE.md
CODEX.md
```

Use this prompt from the target project:

```text
Install Aegis Trail Standalone for this VS Code agent workflow. Detect which project instruction file the active agent reads, then add the Standalone rules there. If multiple instruction files exist, update only the one relevant to the active agent unless I ask for all of them. Add missing ignore rules. Do not create a repo, commit, or push installation changes unless I explicitly ask.
```

## Cursor

Recommended edition: Aegis Trail Standalone.

Preferred install targets:

```text
.cursor/rules/aegis-trail.mdc
AGENTS.md
```

Use this prompt from the target project:

```text
Install Aegis Trail Standalone for this Cursor project. Prefer .cursor/rules/aegis-trail.mdc if Cursor project rules are used; otherwise use AGENTS.md or the existing project instruction file. Add rules for local checkpoint commits, one handoff per session, secret-safe handoff content, rescue after context loss, and no auto-push by default. Add missing ignore rules. Do not create a repo, commit, or push installation changes unless I explicitly ask.
```

## Claude-Style Project Agents

Recommended edition: Aegis Trail Standalone.

Preferred install target:

```text
CLAUDE.md
```

Use this prompt from the target project:

```text
Install Aegis Trail Standalone for this Claude-style project. Add the Standalone rules to CLAUDE.md or the project instruction file this agent reads. Keep the install project-scoped. Add missing ignore rules. Do not create a repo, commit, or push installation changes unless I explicitly ask.
```

## Smoke Test Prompt

After installation, use this prompt to verify the rules are reachable:

```text
Confirm Aegis Trail is active. Tell me which edition is installed, which instruction file contains it, what counts as meaningful work, when you will create a checkpoint, when you will create or update a handoff, where secrets must be stored, what Magic Context/OMO boundaries apply if present, and what you will inspect during Aegis rescue. Do not edit files for this confirmation.
```
