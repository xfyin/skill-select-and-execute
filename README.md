# skill-selector

A meta-skill for AI agents (Cursor, Claude Code, etc.) that **selects the most suitable skill from your installed skills** based on user input, then runs it.

- **One clear match** → Recommends it and runs it.
- **Multiple matches** → Lists top 5 with score and one-line reason; you pick one, then it runs.

Invoke via **`/skill-selector`** in Agent chat, or when you ask "which skill should I use?" or "pick the best skill for this task."

## Install (skills.sh ecosystem)

```bash
npx skills add <your-github-username>/<this-repo-name>
```

Example (after you publish this repo under your GitHub account):

```bash
npx skills add your-username/skill-select-and-execute
```

To install only this skill from a repo that has multiple skills:

```bash
npx skills add https://github.com/your-username/skill-select-and-execute --skill skill-selector
```

## Manual install (Cursor)

Copy the skill into Cursor's user skills directory:

```bash
mkdir -p ~/.cursor/skills/skill-selector
cp skills/skill-selector/SKILL.md ~/.cursor/skills/skill-selector/
```

## License

MIT
