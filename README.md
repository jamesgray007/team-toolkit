# Team Toolkit

A **plugin marketplace** of shared agents and skills for your team, designed to run primarily in **Claude Cowork** (and also works in Claude Code).

This single repo hosts multiple plugins. Teammates add the marketplace once and then install only the plugins they need.

This repo is also a teaching demo for how to package agents and skills as Claude Code / Cowork plugins and distribute them via a marketplace.

## What's inside

This marketplace ships two plugins:

### `operations`

Daily operations agents and skills.

- **moneypenny** (agent) — always-on executive assistant for morning briefings, email triage, schedule prioritization, task delegation, and end-of-day reviews.

### `brand-tools`

Brand and content skills.

- **applying-brand-guidelines** (skill) — applies James Gray / JamesGray.AI brand voice, tone, and messaging guidelines to any content.

## Install

### In Claude Cowork (primary)

Inside a Cowork session:

```
/plugin marketplace add jamesgray007/team-toolkit
/plugin install operations@team-toolkit
/plugin install brand-tools@team-toolkit
```

The first command adds the marketplace once (you'll authenticate with GitHub the first time, since the repo is private). After that, every plugin in this marketplace is one `/plugin install` away.

> **Note for students:** Cowork can install plugins from GitHub but cannot push changes back. To add new agents/skills, edit locally (or via the GitHub web UI) and push from your own machine — then run `/plugin update <plugin-name>` in Cowork to pull the new version.

### In Claude Code

Same commands work:

```
/plugin marketplace add jamesgray007/team-toolkit
/plugin install operations@team-toolkit
/plugin install brand-tools@team-toolkit
```

For local development, clone and add the directory as a marketplace:

```bash
git clone https://github.com/jamesgray007/team-toolkit.git ~/Code/team-toolkit
```

```
/plugin marketplace add ~/Code/team-toolkit
/plugin install operations@team-toolkit
```

## Repo layout

```
team-toolkit/
├── .claude-plugin/
│   └── marketplace.json              # Marketplace catalog — lists every plugin
├── plugins/
│   ├── operations/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json           # Manifest for the operations plugin
│   │   └── agents/
│   │       └── moneypenny.md
│   └── brand-tools/
│       ├── .claude-plugin/
│       │   └── plugin.json           # Manifest for the brand-tools plugin
│       └── skills/
│           └── applying-brand-guidelines/
│               ├── SKILL.md
│               └── references/
└── README.md
```

**Three layers, three names:**

| Layer | Name here | Purpose |
|---|---|---|
| GitHub repo | `team-toolkit` | Where the code lives |
| Marketplace | `team-toolkit` | The catalog teammates `add` once |
| Plugins | `operations`, `brand-tools` | The individual things teammates `install` |

The repo and marketplace happen to share a name, but the plugins are distinct.

## Adding a new plugin to this marketplace

1. Create a folder under `plugins/<plugin-name>/`.
2. Add `plugins/<plugin-name>/.claude-plugin/plugin.json` with `name`, `version`, `description`, `author`.
3. Add the plugin's `agents/`, `skills/`, `commands/`, `hooks/`, etc. inside that folder.
4. Add a new entry to `.claude-plugin/marketplace.json` pointing at `./plugins/<plugin-name>`.
5. Commit and push. Teammates run `/plugin marketplace update team-toolkit` then `/plugin install <plugin-name>@team-toolkit`.

## Adding agents/skills to an existing plugin

1. Drop the new agent in `plugins/<plugin-name>/agents/<name>.md` or skill in `plugins/<plugin-name>/skills/<name>/SKILL.md`.
2. Bump `version` in `plugins/<plugin-name>/.claude-plugin/plugin.json` (and the matching entry in `marketplace.json`).
3. Commit and push. Teammates run `/plugin update <plugin-name>` to pull the changes.

## Hosting plugins in separate repos

If a plugin gets big enough to deserve its own repo, swap its marketplace entry to a remote source:

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

Teammates still install with `/plugin install content-toolkit@team-toolkit` — the marketplace handles the redirect.

## License

Private. For internal team use.
