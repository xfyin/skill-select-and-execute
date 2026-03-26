# skill-selector

A meta-skill that **selects and runs skills from your installed skills** based on user input. It **breaks the request into requirement points** and may recommend **more than one skill** when the task spans different domains (not limited to a single skill). For a single point, one clear match → run it; multiple close matches → list top 5 with score, you pick one, then run.

If nothing installed fits, it falls back to **discovery** (`find-skill` when available, or **`npx skills find`** + **`npx skills add`**) and re-evaluates. Before execution, it performs a **lightweight security review**; do not run skills that fail that review.

Full procedure: [`skills/skill-selector/SKILL.md`](skills/skill-selector/SKILL.md).

Works with Cursor, Claude Code, Windsurf, Cline, and [other agents](https://skills.sh/docs/cli#supported-agents). Invoke via **`/skill-selector`** or ask "which skill should I use?"

## Install

Use the [skills CLI](https://github.com/vercel-labs/skills) (same as [skills.sh](https://skills.sh)):

```bash
npx skills add <owner>/skill-select-and-execute
```

Example (replace with your GitHub username if you forked):

```bash
npx skills add xfyin/skill-select-and-execute
```

The CLI detects your installed agents and installs to the correct path. Options:

```bash
# Install only this skill from a repo
npx skills add owner/skill-select-and-execute --skill skill-selector

# List skills in the repo (no install)
npx skills add owner/skill-select-and-execute --list

# Global install for specific agents
npx skills add owner/skill-select-and-execute -g -a cursor -a claude-code -y
```

See [CLI reference](https://skills.sh/docs/cli) for all options and supported agents.

### Manual install

Clone the repo and copy the skill into your agent’s skills directory (paths per agent: [skills.sh docs](https://skills.sh/docs/cli#supported-agents)):

```bash
git clone https://github.com/<owner>/skill-select-and-execute.git
cp -r skill-select-and-execute/skills/skill-selector ~/.cursor/skills/   # Cursor example
```

**Cursor:** The CLI installs to `~/.agents/skills/`. Cursor’s `/` menu reads from `~/.cursor/skills/`. If `skill-selector` doesn’t appear when you type `/`, run:  
`mkdir -p ~/.cursor/skills/skill-selector && cp ~/.agents/skills/skill-selector/SKILL.md ~/.cursor/skills/skill-selector/`

## License

MIT
