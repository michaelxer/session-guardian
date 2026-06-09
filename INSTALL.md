# Installing Aegis Trail

Aegis Trail is installed by an LLM coding agent because every harness stores instructions differently. The public Aegis Trail repository is the global/latest source of truth, so the agent should fetch it instead of trusting an old local install. Normal Aegis Trail installation still writes rules into the target project's instruction file; it should not edit global agent or harness config unless the user explicitly asks for that separate setup.

The agent should read the public Aegis Trail repository, detect the active environment, ask the required scenario-specific setup questions, and place the correct Aegis Trail edition in a project-level instruction file only after the user answers.

Source repository:

```text
https://github.com/michaelxer/aegis-trail
```

## Recommended Install Method

Paste this into your coding agent from the target project:

```text
Install Aegis Trail for this project.
https://raw.githubusercontent.com/michaelxer/aegis-trail/refs/heads/main/INSTALL.md
```

That is the whole user-facing prompt. The agent should read this file, ask the required guided questions below, then install immediately after the needed answers are complete.

## Source Files

Use this public repository as the source of truth:

```text
https://github.com/michaelxer/aegis-trail
```

Fetch and read these Aegis Trail files from the public repository before editing the target project:

- README.md
- INSTALL.md
- versions/aegis-trail-lite.md
- versions/aegis-trail-standalone.md
- docs/magic-context-compatibility.md
- docs/update-safety.md
- examples/harness-install-examples.md
- templates/gitignore-additions.txt

## Required Guided Questions

Ask questions in chat like an agent-guided installer. Do not edit files before the required questions are answered. After the answers are complete, start the Aegis Trail install immediately instead of asking another generic confirmation.

Do not ask about git commits, pushes, or repo creation during a normal install. Those are not setup choices. Only discuss them if the user already asked for that separate git action.

### Step 0: Detect And Report

After reading the source files and inspecting the target project, report what was detected:

```text
I checked this project and found:

- Aegis Trail source: public global repo at https://github.com/michaelxer/aegis-trail
- Harness: <OpenCode, Pi, Codex CLI, VS Code agent, Cursor, Claude-style agent, or unknown>
- Instruction target: <existing instruction file or file to create>
- OMO: <detected or not detected>
- Magic Context: <detected or not detected>
- Other continuity layer: <detected, not detected, or unclear>
- Git repo: <yes or no>
- Recommended Aegis Trail edition: <Lite, Magic Context compatibility, or Standalone>
- Expected files to change: <instruction file and .gitignore if needed>

Before I edit anything, I need the setup choices below.
```

### Step 1: Ask About Magic Context Only When It Makes Sense

Magic Context is a separate upstream CortexKit project currently documented for OpenCode and Pi. The Magic Context setup is not an Aegis Trail project-file install. It is a user/harness-level context and memory layer that can serve multiple projects through Magic Context's shared store, with optional per-project `magic-context.jsonc` overrides. Do not offer Magic Context as a normal first-install choice for Codex CLI, VS Code agents, Cursor, Claude-style agents, or other non-OpenCode/non-Pi harnesses unless the user explicitly asks to check CortexKit upstream first.

For OpenCode or Pi when Magic Context is not detected, ask:

```text
Question 1: Do you want to set up Magic Context for your OpenCode/Pi environment before installing Aegis Trail here?

1. Yes, pause Aegis Trail and help me run the separate upstream Magic Context setup for this harness first.
2. No, continue without Magic Context for now.
3. I am not sure, explain what Magic Context adds and what global/user-level setup means before I choose.
```

If the user chooses Magic Context first, pause the Aegis Trail install. Treat Magic Context as a separate upstream setup step that may modify user-level or harness-level configuration, shared local storage, compaction behavior, and plugin registration. Ask for explicit approval before running any Magic Context commands.

### Step 2: Ask The Edition Question For The Detected Scenario

Only include choices that make sense for the detected project.

For OpenCode with OMO and no Magic Context:

```text
Question 2: Which Aegis Trail mode do you want for this OpenCode + OMO project?

1. Use Aegis Trail Lite for OMO. Recommended.
2. Use Aegis Trail Standalone anyway. Not recommended because OMO already handles lifecycle/context.
3. Stop with no changes.
```

For OpenCode with OMO and Magic Context detected:

```text
Question 2: Which Aegis Trail mode do you want for this OpenCode project with OMO and Magic Context?

1. Use Aegis Trail Magic Context Compatibility mode. Recommended.
2. Use Aegis Trail Lite for OMO only. Not recommended because Magic Context is active.
3. Use Aegis Trail Standalone anyway. Not recommended because it may duplicate OMO/Magic Context lifecycle rules.
4. Stop with no changes.
```

For OpenCode or Pi with Magic Context detected and no OMO:

```text
Question 2: Which Aegis Trail mode do you want for this Magic Context project?

1. Use Aegis Trail Magic Context Compatibility mode. Recommended.
2. Use Aegis Trail Standalone anyway. Not recommended because Magic Context owns context and memory.
3. Stop with no changes.
```

For OpenCode or Pi with no OMO and no Magic Context:

```text
Question 2: Which Aegis Trail mode do you want for this project?

1. Use Aegis Trail Standalone. Recommended because no strong continuity/context layer was detected.
2. Use Aegis Trail Lite anyway. Only choose this if another lifecycle/context layer exists but was not detected.
3. Stop with no changes.
```

For Codex CLI, VS Code agents, Cursor, Claude-style agents, or another harness with no detected continuity/context layer:

```text
Question 2: Which Aegis Trail mode do you want for this project?

1. Use Aegis Trail Standalone. Recommended.
2. Use Aegis Trail Lite instead. Only choose this if another continuity/context layer already handles lifecycle, memory, handoff, and compaction.
3. Stop with no changes.
```

For another continuity or memory layer where the safe edition is unclear:

```text
Question 2: I found another continuity or memory layer, but the safest Aegis Trail edition is unclear.

Which mode should I use?

1. Use Aegis Trail Lite. Recommended if the other layer already handles context, memory, continuation, or compaction.
2. Use Aegis Trail Standalone. Recommended only if the other layer does not manage lifecycle/context.
3. Explain the tradeoff before I choose.
4. Stop with no changes.
```

### Step 3: Ask Instruction Target Only If Ambiguous

Do not ask technical config-location questions during a normal install. If only one project instruction target is clear, use it.

If multiple plausible project instruction files exist, ask:

```text
Question 3: Which project instruction file should I update?

1. Update <detected primary instruction file>. Recommended.
2. Create or update AGENTS.md.
3. Stop with no changes.
```

### Step 4: Use Safe Git Defaults Without Asking

Do not ask a routine git behavior question during a normal Aegis Trail install. Installing Aegis Trail means editing the target project's instruction file and missing ignore rules only.

Default behavior:

- If no git repo exists, install Aegis Trail without creating a repo.
- If a git repo exists, install Aegis Trail without committing.
- Never push installation changes.
- Report changed files and verification results after the install.

Ask about git only when the user explicitly requested a git action and the request is ambiguous, such as creating a repo, committing the installation, or pushing. Do not turn commit or push into a normal install choice.

### Step 5: Install Immediately After Answers

When all required answers are known, begin the install using those choices:

```text
I have the needed answers:

- Magic Context choice: <set up upstream harness/user layer first, continue without it, already active, or not applicable>
- Aegis Trail edition: <selected edition>
- Instruction target: <file>
- Git behavior: install only by default; no repo creation, commit, or push unless explicitly requested
- Expected changes: <files>

I will install Aegis Trail now using these choices.
```

Then edit files, verify the install, and check diffs for obvious mistakes. Do not create a repo, commit, or push as part of normal installation unless the user explicitly requested that separate git action.

## Install Rules

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

## What The Agent Should Do

1. Detect whether a project instruction file exists, such as `AGENTS.md`, `CLAUDE.md`, `CODEX.md`, or a harness-specific rules file.
2. Treat the public Aegis Trail repository as the source of truth; do not copy rules forward from an old local Aegis Trail install unless they match the fetched source.
3. Detect whether Magic Context is active by looking for `magic-context.jsonc`, `.opencode/magic-context.jsonc`, `@cortexkit/opencode-magic-context`, `@cortexkit/magic-context`, `@cortexkit/pi-magic-context`, or user/project OpenCode or Pi plugin entries for Magic Context.
4. Detect whether OMO / oh-my-openagent / oh-my-opencode or another strong continuity layer is active.
5. Choose the edition.
6. Use Aegis Trail Lite / Magic Context compatibility mode when Magic Context is active.
7. Use Aegis Trail Lite when OMO or another strong continuity layer is active.
8. Use Aegis Trail Standalone when no continuity/context layer is active or the user wants the full original workflow.
9. Ask the required guided questions and wait for the user's answers before editing.
10. If the user chooses to set up Magic Context first, pause the Aegis Trail install and handle Magic Context only as a separate upstream harness/user-level setup with explicit approval.
11. Add the selected Aegis Trail text to the project instruction file after the required answers are complete.
12. Add missing `.gitignore` entries from `templates/gitignore-additions.txt`.
13. Do not place real credentials in any tracked file or Magic Context memory/note.
14. Do not create a git repo unless the user asks.
15. If the user explicitly requested an installation commit, stage only intentional Aegis Trail installation files and check staged content for secrets before committing.
16. Report which edition was installed, which instruction file changed, and how the user should ask for checkpoint, handoff, and rescue behavior.

## Magic Context Installation Notes

Magic Context is an upstream CortexKit project: https://github.com/cortexkit/magic-context

Aegis Trail does not ship Magic Context. Install and update Magic Context from CortexKit upstream when you need context and memory support.

Magic Context setup is separate from Aegis Trail and is usually user-level or harness-level for OpenCode/Pi, not a one-project-only Aegis Trail install. One Magic Context setup can serve multiple projects through Magic Context's shared local store and project identity scoping. Individual projects may still add `magic-context.jsonc` or `.opencode/magic-context.jsonc` to override defaults.

Aegis Trail's LLM install prompt does not install Magic Context automatically by default. If Magic Context is not already present for an OpenCode or Pi project, the installer should say so in the guided questions, should explain that Magic Context setup may affect the user's wider OpenCode/Pi environment, should not copy or vendor it, and should not run Magic Context setup unless the user explicitly asks for that separate upstream setup.

Recommended opencode workflow:

1. Set up Magic Context from CortexKit upstream if you want memory/context support across your OpenCode/Pi environment.
2. Confirm Magic Context is visible through user-level plugin/config entries or project configuration such as `magic-context.jsonc`, `.opencode/magic-context.jsonc`, or `@cortexkit/opencode-magic-context`.
3. Run the Aegis Trail LLM install prompt from the target project.
4. Let Aegis Trail detect Magic Context and install Lite / Magic Context compatibility mode for that project.
5. Keep Magic Context responsible for context and memory across supported projects, and keep Aegis Trail responsible for per-project git checkpoints, secrets, the numbered `HANDOFF_DOC/handoff-NNN.md` trail, and push safety.

When Magic Context is active, Aegis Trail should not add competing context-management instructions. Do not add Standalone context heuristics, duplicate compaction rules, or instructions that fight Magic Context's historian, dreamer, recall, or compaction replacement behavior.

Aegis Trail should add only the safety layer:

- local git checkpoint discipline
- secret/private-data protection for files, commits, handoffs, `ctx_memory`, and `ctx_note`
- no-auto-push defaults
- numbered `HANDOFF_DOC/handoff-NNN.md` handoff trail and rescue discipline

If Magic Context reports a setup conflict, use Magic Context's upstream setup or doctor guidance. Do not patch Magic Context source files from Aegis Trail.

## OMO Installation Notes

For OMO, install Aegis Trail Lite only.

OMO / oh-my-openagent / oh-my-opencode handles agent orchestration, tasks, handoff, compaction, and continuation in OMO projects. Aegis Trail Lite complements it with git checkpoint discipline, secret safety, and no-auto-push defaults.

OMO already provides:

- compaction and context-window recovery
- task persistence under `.omo/tasks/`
- plan/work continuation with `/start-work`
- handoff support through `/handoff`
- tool-output truncation and recovery hooks

Aegis Trail Lite should add only the missing safety layer:

- local git checkpoint discipline
- secret/private-data protection
- numbered `HANDOFF_DOC/handoff-NNN.md` handoff trail before stopping meaningful work or when agent-readable context pressure/heuristics are high after completed work
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
