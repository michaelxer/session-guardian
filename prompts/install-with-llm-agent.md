# Install With An LLM Agent

Copy and paste this into the coding agent from the target project.

```text
Install Aegis Trail for this project.
https://raw.githubusercontent.com/michaelxer/aegis-trail/refs/heads/main/INSTALL.md
```

Expected agent behavior:

- Read `INSTALL.md` and the referenced source files.
- Use the public Aegis Trail repository as the latest source of truth, not an old local install.
- Detect the project instruction file and continuity/context layer.
- Ask the scenario-specific setup questions before editing files.
- Start the install immediately after the required answers are complete.
- Do not install Magic Context automatically.
- Do not create a repo, commit, or push installation changes unless the user explicitly asked for that separate git action.
