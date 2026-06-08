# Aegis Trail

**A safety trail for AI coding agents.**

Aegis Trail is a docs-first prompt kit for AI coding agents. It keeps coding sessions recoverable by adding local git checkpoints, secret-safe handoff discipline, no-auto-push defaults, and rescue instructions for the next chat.

It is installed as project-level guidance beside systems like OMO, opencode, Codex CLI, Cursor, Claude-style agents, VS Code agent workflows, and memory/context engines. It does not patch agent internals or package-managed files.

## Magic Context Credit

Aegis Trail is designed to complement [Magic Context by CortexKit](https://github.com/cortexkit/magic-context).

Magic Context handles self-managing context, project memory, recall, historian/dreamer behavior, and compaction replacement. Aegis Trail handles local checkpoint safety, secret-safe staging and commits, no-auto-push policy, fallback handoffs, and rescue discipline.

Aegis Trail does not vendor, copy, fork, or replace Magic Context by default. If you want context and memory support, install Magic Context from CortexKit upstream and then add Aegis Trail compatibility rules on top.

## Editions

| Edition | Best For | Purpose |
| --- | --- | --- |
| Aegis Trail Lite | OMO, Magic Context, or another strong continuity/context layer | Adds the missing safety layer: git checkpoint discipline, secret protection, no-auto-push defaults, and lightweight handoff/rescue rules. |
| Aegis Trail Standalone | Codex CLI, VS Code agents, Cursor, Claude-style agents, vanilla/non-OMO setups | The full Aegis Trail workflow for environments without durable session lifecycle support. It carries its own context monitoring, handoff format, checkpoint rules, and rescue protocol. |
| Aegis Trail Magic Context Compatibility | Projects with `magic-context.jsonc` or CortexKit Magic Context packages/plugins | A Lite install profile that lets Magic Context own context and memory while Aegis Trail owns git, secrets, handoff fallback, and push safety. |

Standalone is not the "better" edition. It is the self-contained edition for environments that do not already provide OMO-style continuity or Magic Context-style memory. For OMO or Magic Context projects, Lite/compatibility mode is usually the right choice because it avoids duplicate lifecycle and compaction rules.

## Why This Exists

Long agent sessions often fail in a predictable way:

```text
long session -> no handoff -> no commit -> context error -> lost working state
```

Aegis Trail creates recovery anchors before that happens:

| Recovery Anchor | What It Protects |
| --- | --- |
| Local git commit | Code and file changes survive context loss. |
| Handoff file | The next agent knows what happened and where to resume. |
| Secret scan | Private data is not accidentally committed, memorized, or pushed. |
| Rescue protocol | A new session can reconstruct state from git, handoff files, task state, and available memory/context tools. |

## Common Scenarios

Use Aegis Trail Lite when an OMO project already has `/handoff`, `/start-work`, persisted tasks, and compaction support. Lite tells the agent to keep using OMO while adding local git checkpoints and secret discipline.

Use Aegis Trail Magic Context Compatibility when Magic Context is installed. Magic Context should own context, memory, recall, historian/dreamer behavior, and compaction replacement. Aegis Trail should only add safety rails around git, secrets, fallback handoff, and pushing.

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
| Full handoff template | Optional | Fallback only | Built in |
| Manual context heuristics | No | No | Yes |
| Magic Context memory secret rules | Compatible | Built in | Only if Magic Context is added later |
| Rescue workflow after context failure | Yes | Yes | Yes |
| Suitable as always-on prompt | Yes | Yes | Sometimes, but larger |

## Installation Philosophy

Installation is intentionally done by an LLM coding agent, not by a fragile shell script or `npx` package.

Aegis Trail does not install Magic Context automatically by default. For opencode projects that need memory/context support, install Magic Context from CortexKit upstream first, then run the Aegis Trail install prompt so the agent detects Magic Context and installs Lite/compatibility mode.

Paste the install prompt from `prompts/install-with-llm-agent.md` into the agent that manages your target project. The agent should read this public repository first, detect your environment, and install the right edition into the correct project instruction file.

Recommended defaults:

| Environment | Recommended Edition |
| --- | --- |
| Magic Context by CortexKit | Aegis Trail Lite / Magic Context Compatibility |
| OMO / oh-my-openagent / oh-my-opencode | Aegis Trail Lite |
| opencode without OMO or Magic Context | Aegis Trail Standalone or Lite, depending on how much automation you want |
| Codex CLI | Aegis Trail Standalone |
| VS Code agent workflows | Aegis Trail Standalone |
| Cursor / Claude-style agents | Aegis Trail Standalone |

Examples are included in `examples/` if you want to see what an installed Aegis Trail section looks like before asking an agent to install it.

## Step 1: Install Aegis Trail

Agent-guided install.

Paste this into your LLM coding agent session from the project you want to protect. The agent will read the public Aegis Trail install guide first, ask setup questions in chat only if the environment is unclear, then install the right edition into project-level instructions.

```text
Install and configure Aegis Trail by following the instructions here:
https://raw.githubusercontent.com/michaelxer/aegis-trail/refs/heads/main/INSTALL.md

Use this public repository as the source of truth:
https://github.com/michaelxer/aegis-trail

Fetch and read the relevant files from that repository before editing this project, especially README.md, INSTALL.md, versions/aegis-trail-lite.md, versions/aegis-trail-standalone.md, docs/magic-context-compatibility.md, docs/update-safety.md, examples/harness-install-examples.md, and templates/gitignore-additions.txt.

Install it for the current project only.

If this project uses Magic Context by CortexKit, install Aegis Trail Lite / Magic Context compatibility mode. Do not install Aegis Trail Standalone context heuristics. Do not copy, vendor, fork, or replace Magic Context. Keep Magic Context installed and updated from upstream.

If this project uses OMO, oh-my-openagent, or oh-my-opencode, install Aegis Trail Lite.

Otherwise install Aegis Trail Standalone unless you find another existing continuity layer and need to ask me first.

Do not modify OMO internals, Magic Context internals, node_modules, generated plugin files, hidden agent internals, or package-managed files. Prefer project-level agent instruction files such as AGENTS.md, CLAUDE.md, CODEX.md, or the harness-specific project rules file.

Add the safe ignore rules if they are missing. Do not create a git repo unless I explicitly ask. Do not enable auto-push unless I explicitly approve it for this repo.

After installation, tell me which edition was installed, which files changed, and how to trigger checkpoint, handoff, and rescue behavior.
```

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
Auto handoff: yes, before stopping meaningful work.
Auto remote push: no, unless explicitly approved per project.
```

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
