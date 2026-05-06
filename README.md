# Team Toolkit

A shared plugin of skills and agents for your team, designed to run primarily in **Claude Cowork** (and also works in Claude Code). Anyone on the team can install it with one command and instantly have the same agents, skills, and ways of working.

This repo is also a teaching demo for how to package up agents and skills as a Claude Code / Cowork plugin and distribute it via a marketplace.

## What's inside

### Agents

- **moneypenny** — an always-on executive assistant that handles morning briefings, email triage, schedule prioritization, task delegation, and end-of-day reviews.

### Skills

- **applying-brand-guidelines** — applies James Gray / JamesGray.AI brand voice, tone, and messaging guidelines to any content.

## Install

This repo is also a **plugin marketplace**, so adding it once gives teammates access to every plugin we publish here.

### In Claude Cowork (primary)

This is the path students will use. Inside a Cowork session:

```
/plugin marketplace add jamesgray007/team-toolkit
/plugin install team-toolkit@team-toolkit
```

Because the repo is private, Cowork will prompt to authenticate with GitHub the first time. After that, every plugin published in this marketplace is one `/plugin install` away.

> **Note for students:** Cowork can install plugins from GitHub but cannot push changes back. To add new agents/skills, edit locally (or via the GitHub web UI) and push from your own machine — then run `/plugin update team-toolkit` in Cowork to pull the new version.

### In Claude Code

Same commands work:

```
/plugin marketplace add jamesgray007/team-toolkit
/plugin install team-toolkit@team-toolkit
```

For local development, clone and add the directory as a marketplace:

```bash
git clone https://github.com/jamesgray007/team-toolkit.git ~/Code/team-toolkit
```

```
/plugin marketplace add ~/Code/team-toolkit
/plugin install team-toolkit@team-toolkit
```

## Repo layout

```
team-toolkit/
├── .claude-plugin/
│   ├── plugin.json         # Manifest for the team-toolkit plugin
│   └── marketplace.json    # Marketplace listing — add more plugins here
├── agents/
│   └── moneypenny.md       # Agent definitions
├── skills/
│   └── applying-brand-guidelines/
│       ├── SKILL.md        # Skill definition
│       └── references/     # Supporting reference files
└── README.md
```

## Adding new agents and skills (to this plugin)

1. Drop a new agent markdown file in `agents/`, following the same frontmatter pattern as `moneypenny.md`.
2. Drop a new skill folder in `skills/<skill-name>/` with a `SKILL.md` file.
3. Bump `version` in `.claude-plugin/plugin.json` (and the matching entry in `.claude-plugin/marketplace.json`).
4. Commit and push. Teammates run `/plugin update team-toolkit` to pull the changes.

## Adding more plugins to the marketplace

Two patterns, both supported by `.claude-plugin/marketplace.json`:

**Pattern A — subfolder in this repo** (good for tightly related plugins):

```
team-toolkit/
├── .claude-plugin/marketplace.json
├── plugins/
│   ├── content-toolkit/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/
│   │   └── agents/
│   └── coaching-toolkit/
│       └── .claude-plugin/plugin.json
└── (existing team-toolkit files)
```

Then add to `marketplace.json`:

```json
{
  "name": "content-toolkit",
  "source": "./plugins/content-toolkit",
  "description": "..."
}
```

**Pattern B — separate GitHub repo** (good for plugins that have their own lifecycle):

```json
{
  "name": "content-toolkit",
  "source": {
    "source": "github",
    "repo": "jamesgray007/content-toolkit"
  },
  "description": "..."
}
```

Either way, teammates run `/plugin install <plugin-name>@team-toolkit` and Claude Code pulls it from the right place.

## License

Private. For internal team use.
