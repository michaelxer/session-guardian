# Harness Install Examples

These examples show where an LLM coding agent should usually install Aegis Trail globally for each harness. They are not separate editions.

For every harness below, the agent should read `INSTALL.md`, detect the active harness and global/user instruction target, ask the required scenario-specific setup questions, and wait for the user's answers before editing files. After all required answers are complete, the agent should start the install immediately.

Normal installation edits global/user agent instructions only. Do not edit the current repo, create a git repo, commit installation changes, or push during installation unless the user explicitly requested that separate action. Use project instruction files only as an explicit project-specific override or when the harness has no usable global instruction target.

Do not offer Magic Context as a normal setup choice for every harness. CortexKit Magic Context is currently documented for OpenCode and Pi. For Codex CLI, VS Code agents, Cursor, Claude-style agents, or other non-OpenCode/non-Pi harnesses, recommend Aegis Trail Standalone unless another continuity/context layer is already active or the user explicitly asks to check CortexKit upstream first.

## Magic Context By CortexKit

Recommended edition/profile: Aegis Trail Lite + Magic Context Compatibility.

Preferred install target:

```text
Global/user instruction file for the active Magic Context harness
```

Use this prompt from an agent session that can edit global/user instructions:

```text
Install Aegis Trail Lite + Magic Context Compatibility globally for this agent environment. Read the Aegis Trail repo first. Keep Magic Context by CortexKit responsible for context management, memory, recall, historian/dreamer behavior, and compaction replacement. Do not install Aegis Trail Standalone context heuristics. Do not copy, vendor, fork, replace, or patch Magic Context. Add Aegis Trail Lite safety rules for local git checkpoints, secret-safe staging and commits, no-auto-push defaults, a numbered per-project HANDOFF_DOC/handoff-NNN.md trackback trail, and rescue discipline. Never write real secrets or private customer data into ctx_memory, ctx_note, summaries, prompts, handoffs, or commits. Do not edit the current repo, create a repo, commit, or push installation changes unless I explicitly ask.
```

Expected result:

```text
Aegis Trail Lite + Magic Context Compatibility installed in global/user instructions.
Magic Context remains the context and memory manager.
Aegis Trail adds per-project checkpoint, secret, numbered handoff history, and push-safety discipline when operating in repos.
```

## OMO / Oh-My-Openagent / Oh-My-Opencode

Recommended edition: Aegis Trail Lite.

Preferred install target:

```text
Global/user instruction file for OpenCode/OMO
```

Use this prompt from an agent session that can edit global/user instructions:

```text
Install Aegis Trail Lite globally for this OpenCode/OMO environment. Read the Aegis Trail repo first. Add the Lite rules to the global/user instruction file the active agent reads. Do not modify OMO internals, generated prompts, node_modules, package-managed files, or hidden agent internals. Keep OMO responsible for tasks, /handoff, /start-work, compaction, and continuation. Add the numbered per-project HANDOFF_DOC/handoff-NNN.md trackback trail and safe ignore-rule requirements for HANDOFF_DOC/, .credentials/, private/, .env, local databases, exports, backups, cache, temp, and logs. Do not edit the current repo, create a repo, commit, or push installation changes unless I explicitly ask.
```

Expected result:

```text
Aegis Trail Lite installed in global/user instructions.
OMO remains the lifecycle manager.
Aegis Trail adds per-project checkpoint, secret, numbered handoff history, and rescue discipline when operating in repos.
```

## Opencode Without OMO

Recommended edition: Aegis Trail Standalone for full lifecycle management, or Aegis Trail Lite if the project already has another strong continuation system.

Preferred install target:

```text
Global/user instruction file for OpenCode
```

Use this prompt from an agent session that can edit global/user instructions:

```text
Install Aegis Trail globally for this OpenCode environment. If there is no OMO, Magic Context, or equivalent continuity layer, install Aegis Trail Standalone in the global/user instruction file the active agent reads. If a continuity/context layer already handles handoff, compaction, memory, and task persistence, install Aegis Trail Lite instead. Add the numbered per-project HANDOFF_DOC/handoff-NNN.md trackback trail and safe ignore-rule requirements. Do not modify hidden OpenCode internals or generated/package-managed files. Do not edit the current repo, create a repo, commit, or push installation changes unless I explicitly ask.
```

## Codex CLI

Recommended edition: Aegis Trail Standalone.

Preferred install target:

```text
Global/user instruction file for Codex CLI
```

Use this prompt from an agent session that can edit global/user instructions:

```text
Install Aegis Trail Standalone globally for this Codex CLI environment. Add the Standalone rules to the global/user instruction file the active agent reads. The rules must require local checkpoints after meaningful completed work, one per-project handoff file per session before stopping, secret-safe diffs, no auto-push by default, and Aegis rescue after context loss. Do not edit the current repo, create a repo, commit, or push installation changes unless I explicitly ask.
```

## VS Code Agent Workflows

Recommended edition: Aegis Trail Standalone.

Preferred install target depends on the extension or agent. Use the global/user instruction file the active agent reads.

Common targets:

```text
AGENTS.md
.github/copilot-instructions.md
CLAUDE.md
CODEX.md
```

Use this prompt from an agent session that can edit global/user instructions:

```text
Install Aegis Trail Standalone globally for this VS Code agent workflow. Detect which global/user instruction file the active agent reads, then add the Standalone rules there. If the extension has no usable global instruction target, ask before using a project-specific fallback. Do not edit the current repo, create a repo, commit, or push installation changes unless I explicitly ask.
```

## Cursor

Recommended edition: Aegis Trail Standalone.

Preferred install targets:

```text
Global/user Cursor rules or user instruction file
```

Use this prompt from an agent session that can edit global/user instructions:

```text
Install Aegis Trail Standalone globally for this Cursor environment. Prefer the global/user Cursor rules target if available. If Cursor only exposes project rules, ask before using that project-specific fallback. Add rules for local checkpoint commits, one per-project handoff per session, secret-safe handoff content, rescue after context loss, and no auto-push by default. Do not edit the current repo, create a repo, commit, or push installation changes unless I explicitly ask.
```

## Claude-Style Project Agents

Recommended edition: Aegis Trail Standalone.

Preferred install target:

```text
Global/user Claude instruction file
```

Use this prompt from an agent session that can edit global/user instructions:

```text
Install Aegis Trail Standalone globally for this Claude-style agent environment. Add the Standalone rules to the global/user instruction file this agent reads. If no global/user instruction target exists, ask before using a project-specific fallback. Do not edit the current repo, create a repo, commit, or push installation changes unless I explicitly ask.
```

## Smoke Test Prompt

After installation, use this prompt to verify the rules are reachable:

```text
Confirm Aegis Trail is active. Tell me which edition is installed, which global/user instruction file contains it, what counts as meaningful work, when you will create a checkpoint, when you will create or update a per-project handoff, where secrets must be stored, what Magic Context/OMO boundaries apply if present, and what you will inspect during Aegis rescue. Do not edit files for this confirmation.
```
