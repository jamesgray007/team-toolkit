# Team Toolkit

A shared Claude Code plugin of skills and agents for your team. Anyone on the team can install it with one command and instantly have the same agents, skills, and ways of working.

This repo is also a teaching demo for how to package up agents and skills as a Claude Code plugin.

## What's inside

### Agents

- **moneypenny** — an always-on executive assistant that handles morning briefings, email triage, schedule prioritization, task delegation, and end-of-day reviews.

### Skills

- **applying-brand-guidelines** — applies James Gray / JamesGray.AI brand voice, tone, and messaging guidelines to any content.

## Install

The plugin lives in a private GitHub repo, so install it as a marketplace from the Git URL:

```
/plugin marketplace add jamesgray007/team-toolkit
/plugin install team-toolkit@team-toolkit
```

Or, for local development, clone the repo and point Claude Code at the directory:

```bash
git clone https://github.com/jamesgray007/team-toolkit.git ~/Code/team-toolkit
```

Then in Claude Code:

```
/plugin marketplace add ~/Code/team-toolkit
/plugin install team-toolkit
```

## Repo layout

```
team-toolkit/
├── .claude-plugin/
│   └── plugin.json         # Plugin manifest
├── agents/
│   └── moneypenny.md       # Agent definitions
├── skills/
│   └── applying-brand-guidelines/
│       ├── SKILL.md        # Skill definition
│       └── references/     # Supporting reference files
└── README.md
```

## Adding new agents and skills

1. Drop a new agent markdown file in `agents/`, following the same frontmatter pattern as `moneypenny.md`.
2. Drop a new skill folder in `skills/<skill-name>/` with a `SKILL.md` file.
3. Bump `version` in `.claude-plugin/plugin.json`.
4. Commit and push. Teammates run `/plugin update team-toolkit` to pull the changes.

## License

Private. For internal team use.
