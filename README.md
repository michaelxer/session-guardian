# Aegis Trail

**A safety trail for AI coding agents.**

Aegis Trail is a docs-first prompt kit for AI coding agents. It keeps coding sessions recoverable by adding local git checkpoints, secret-safe handoff discipline, no-auto-push defaults, and rescue instructions for the next chat.

It is installed as project-level guidance beside systems like OMO, opencode, Codex CLI, Cursor, Claude-style agents, VS Code agent workflows, and memory/context engines. It does not patch agent internals or package-managed files.

## Compatibility Credits

Aegis Trail is designed to complement [Magic Context by CortexKit](https://github.com/cortexkit/magic-context).

Magic Context handles self-managing context, project memory, recall, historian/dreamer behavior, and compaction replacement. Aegis Trail handles local checkpoint safety, secret-safe staging and commits, no-auto-push policy, numbered portable handoff history, and rescue discipline.

Aegis Trail also works beside OMO / oh-my-openagent / oh-my-opencode. OMO handles agent orchestration, tasks, handoff, compaction, and continuation in OMO projects. Aegis Trail Lite complements it with git checkpoint discipline, secret safety, and no-auto-push defaults.

Aegis Trail does not vendor, copy, fork, or replace Magic Context or OMO by default. If you want context, memory, or orchestration support, install those tools from their upstream projects and then add Aegis Trail compatibility rules on top.

## Editions

| Edition | Best For | Purpose |
| --- | --- | --- |
| Aegis Trail Lite | OMO, Magic Context, or another strong continuity/context layer | Adds the missing safety layer: git checkpoint discipline, secret protection, no-auto-push defaults, and a numbered portable handoff trail. |
| Aegis Trail Standalone | Codex CLI, VS Code agents, Cursor, Claude-style agents, vanilla/non-OMO setups | The full Aegis Trail workflow for environments without durable session lifecycle support. It carries its own context monitoring, handoff format, checkpoint rules, and rescue protocol. |
| Aegis Trail Magic Context Compatibility | Projects with `magic-context.jsonc` or CortexKit Magic Context packages/plugins | A Lite install profile that lets Magic Context own context and memory while Aegis Trail owns git, secrets, numbered handoff history, and push safety. |

Standalone is not the "better" edition. It is the self-contained edition for environments that do not already provide OMO-style continuity or Magic Context-style memory. For OMO or Magic Context projects, Lite/compatibility mode is usually the right choice because it avoids duplicate lifecycle and compaction rules.

## Why This Exists

Imagine a careful builder repairing your house. They open the wall, move wires, find a leak, leave halfway through, and another builder arrives tomorrow. If there is no job note, the new builder has to guess what was changed, what is safe, what is unfinished, and why the first builder made those choices.

AI coding sessions fail in the same ordinary way. The agent may be doing good work, then the chat gets long, the context window fills up, a tool fails, the browser refreshes, or a different agent takes over. Without a written handoff, the next session has to reconstruct the story from scattered chat messages and file diffs. That is when work gets repeated, good changes get reversed, secrets get copied into the wrong place, or the agent confidently continues from the wrong assumption.

Aegis Trail treats handoff documentation like the job notebook for the project. At meaningful stopping points, it asks the agent to leave a numbered local handoff that says what changed, why it changed, what was checked, what is still pending, and how the next session should resume. Older handoffs stay in order, so they become a trackback history when you need to understand when a decision happened or where a mistake entered the work.

Long agent sessions often fail in a predictable way:

```text
long session -> no handoff -> no commit -> context error -> lost working state
```

Aegis Trail creates recovery anchors before that happens:

| Recovery Anchor | Plain Meaning | Technical Meaning |
| --- | --- | --- |
| Local git commit | The finished work is saved in a recoverable checkpoint. | Code and file changes survive context loss and can be inspected with normal git tools. |
| Numbered handoff file | The next agent gets the job notebook instead of guessing. | `HANDOFF_DOC/handoff-NNN.md` records completed work, pending work, decisions, git state, verification, and resume prompts. |
| Secret scan | Private information is kept out of the public trail. | Staged content is checked before commits, handoffs, memory notes, or pushes can capture credentials or private data. |
| Rescue protocol | A new session has a safe way to restart. | The agent reconstructs state from git, handoff files, task state, and available memory/context tools. |

This is why Aegis Trail complements Magic Context instead of replacing it. Magic Context is the memory and context layer: it helps the agent remember, recall, summarize, and survive compaction. Aegis Trail is the safety and evidence layer: it keeps concrete local checkpoints, numbered handoff history, secret rules, and no-auto-push discipline outside the chat window. Memory helps the agent think; the handoff trail helps the next agent verify what really happened.

## Common Scenarios

Use Aegis Trail Lite when an OMO project already has `/handoff`, `/start-work`, persisted tasks, and compaction support. Lite tells the agent to keep using OMO while adding local git checkpoints, secret discipline, and a numbered `HANDOFF_DOC/handoff-NNN.md` trackback trail.

Use Aegis Trail Magic Context Compatibility when Magic Context is installed. Magic Context should own context, memory, recall, historian/dreamer behavior, and compaction replacement. Aegis Trail should only add safety rails around git, secrets, numbered handoff history, and pushing.

Use Aegis Trail Standalone when the host agent does not have a durable session lifecycle. Standalone tells the agent when to checkpoint, when to hand off, what the handoff must contain, and how a new session should recover.

Use the rescue prompt when a session already lost context. The next agent should inspect git state, recent commits, uncommitted diffs, handoff files, public OMO state if OMO is present, and Magic Context tools/config if Magic Context is active.

## Feature Overview

| Feature | Lite | Magic Context Compatibility | Standalone |
| --- | --- | --- | --- |
| Local git checkpoint after completed work | Yes | Yes | Yes |
| Secret/private-data protection | Yes | Yes | Yes |
| Never push unless explicitly allowed | Yes | Yes | Yes |
| Optional private checkpoint push policy | Yes | Yes | Yes |
| Host lifecycle/context manager stays in charge | Yes | Yes, Magic Context | No dependency |
| Numbered portable handoff trail | Yes | Yes | Yes |
| Manual context heuristics | No | No | Yes |
| Magic Context memory secret rules | Compatible | Built in | Only if Magic Context is added later |
| Rescue workflow after context failure | Yes | Yes | Yes |
| Suitable as always-on prompt | Yes | Yes | Sometimes, but larger |

## Installation Philosophy

Installation is intentionally done by an LLM coding agent, not by a fragile shell script or `npx` package.

The short prompt points the agent at `INSTALL.md`. The agent should read the install guide, detect the project, then ask OMC-style setup questions before editing files. The questions should cover the detected harness, instruction target, Magic Context status when relevant, selected edition, and expected file changes. Normal installation edits project instruction and ignore files only; it should not ask a routine git question, create a repo, commit, or push unless the user explicitly requested that separate git action.

Aegis Trail's public installer is global, but the installed Aegis Trail rules are intentionally project-level so each repo can keep its own safety and handoff policy. Aegis Trail does not install Magic Context automatically by default. Magic Context is a separate CortexKit upstream project currently documented for OpenCode and Pi; its setup is user/harness-level and can serve multiple projects through Magic Context's shared store, with optional per-project config overrides. For OpenCode or Pi projects that need memory/context support, set up Magic Context from CortexKit upstream first, then run the Aegis Trail install prompt so the agent detects Magic Context and installs Lite/compatibility mode in the target project. For Codex CLI, VS Code agents, Cursor, and Claude-style agents, Aegis Trail should not offer Magic Context as a normal setup choice unless the user explicitly asks to check CortexKit upstream first.

Paste the short prompt from `prompts/install-with-llm-agent.md` into the agent that manages your target project. The agent should read `INSTALL.md`, detect your environment, ask the scenario-specific setup questions, and install the right edition into the correct project instruction file only after the required answers are complete.

Recommended defaults:

| Environment | Recommended Edition |
| --- | --- |
| Magic Context by CortexKit | Aegis Trail Lite / Magic Context Compatibility |
| OMO / oh-my-openagent / oh-my-opencode | Aegis Trail Lite |
| opencode without OMO, Magic Context, or another continuity layer | Aegis Trail Standalone |
| Codex CLI | Aegis Trail Standalone |
| VS Code agent workflows | Aegis Trail Standalone |
| Cursor / Claude-style agents | Aegis Trail Standalone |

Examples are included in `examples/` if you want to see what an installed Aegis Trail section looks like before asking an agent to install it.

## Step 1: Install Aegis Trail

Agent-guided install.

Paste this into your LLM coding agent session from the project you want to protect:

```text
Install Aegis Trail for this project.
https://raw.githubusercontent.com/michaelxer/aegis-trail/refs/heads/main/INSTALL.md
```

The agent should respond with a short install preview and wait for your answer before changing files.

## Files

| Path | Purpose |
| --- | --- |
| `versions/aegis-trail-lite.md` | Lightweight rules for OMO, Magic Context, and other continuity layers. |
| `versions/aegis-trail-standalone.md` | Full lifecycle rules for projects without a continuity/context manager. |
| `docs/magic-context-compatibility.md` | Compatibility policy, CortexKit credit, detection, and boundaries. |
| `examples/aegis-trail-lite-in-agents.md` | Example project-level Lite installation snippet. |
| `examples/aegis-trail-standalone-in-agents.md` | Example project-level Standalone installation snippet. |
| `examples/aegis-trail-with-magic-context.md` | Example project-level Magic Context compatibility snippet. |
| `examples/harness-install-examples.md` | Harness-specific install examples. |
| `examples/safe-handoff-example.md` | Example handoff that avoids secrets. |
| `commands/aegis-checkpoint.md` | Checkpoint workflow template. |
| `commands/aegis-handoff.md` | Handoff workflow template. |
| `commands/aegis-rescue.md` | Recovery workflow template for new sessions. |
| `docs/update-safety.md` | How to avoid OMO, Magic Context, or host-agent updates breaking Aegis Trail. |
| `templates/gitignore-additions.txt` | Ignore rules for consuming projects. |
| `prompts/install-with-llm-agent.md` | Copy-paste installer prompt for agents. |
| `prompts/resume-from-handoff.md` | Copy-paste continuation prompt template. |

## Auto-Push Policy

Aegis Trail does not enable auto-push by default.

Default behavior:

```text
Auto local commit: yes, after completed meaningful work.
Auto handoff: yes, before stopping meaningful work or when agent-readable context signals are high/critical after completed work.
Auto remote push: no, unless explicitly approved per project.
```

Aegis Trail does not require the user to expose a context percentage. If the host exposes exact usage to the agent, use it. Otherwise, automate from heuristic labels based on observable signals such as completed tasks, files touched, tool calls, long conversation length, lifecycle warnings, and model degradation. Do not invent fake percentages.

After saving a handoff, the agent should tell the user to move to a new session and include the continuation prompt.

If remote backup is needed, prefer a private checkpoint branch such as:

```text
aegis/checkpoints
```

Do not auto-push to `main` unless the user explicitly approves that behavior for the current repo.

## Safety Rules

- Never commit real API keys, tokens, passwords, private lead data, customer data, local databases, exports, or credential files.
- Never write secrets into source code, `.omo/`, handoff files, plans, task files, summaries, prompts, commits, Magic Context `ctx_memory`, or Magic Context `ctx_note`.
- Store private values only in ignored local files such as `.credentials/`, `private/`, or `.env`.
- Stage only files related to the completed task.
- Never push unless explicitly requested or the repo has explicit auto-push approval.

## License

MIT. See `LICENSE`.
