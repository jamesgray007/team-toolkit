# Plugin Workshop — Instructor Script

A ~45 minute end-to-end exercise. Students leave having built a plugin, pushed it to GitHub, and installed each other's plugins from a marketplace.

**Primary runtime:** Claude Cowork. Also works in Claude Code.

## Outcomes

By the end, every student will have:
- A working `.claude/` folder with at least one agent and one skill they wrote
- A GitHub repo containing that work, restructured as a Claude Code plugin
- A `marketplace.json` so others can install their plugins
- Installed a partner's plugin and run it locally

## Pre-workshop checklist (send to students 24h before)

- [ ] Install GitHub CLI: `brew install gh && gh auth login`
- [ ] Have Claude Cowork (or Claude Code) ready
- [ ] **Bring at least one agent and one skill they've already written** (from `~/.claude/` or anywhere else). If they don't have any yet, they should write a small one before the workshop — a 15-line skill is fine.

## Lesson script

### 0. Show the destination (3 min)

Open `github.com/jamesgray007/team-toolkit` on screen. Walk them through the layout:

- The repo is a **marketplace** — one place that hosts many plugins.
- Inside `plugins/` are the individual plugins (`operations`, `brand-tools`).
- Each plugin has its own `.claude-plugin/plugin.json` manifest.
- At the root, `.claude-plugin/marketplace.json` is the catalog.

Run the install command live in Cowork:

```
/plugin marketplace add jamesgray007/team-toolkit
/plugin install operations@team-toolkit
```

Say: *"By the end of today, you'll build something that looks like this. Your name on the repo, your skills and agents inside."*

### 1. Set up the folder and move skills/agents in (10 min)

Each student:

**a. Create the project folder**
```bash
mkdir ~/Code/<my-name>-tools
cd ~/Code/<my-name>-tools
```

**b. Create the `.claude/` folder with `agents/` and `skills/` inside**
```bash
mkdir -p .claude/agents
mkdir -p .claude/skills
```

**c. Move (or copy) their existing skills and agents into it**

If their files are in `~/.claude/`:
```bash
cp ~/.claude/agents/<their-agent>.md .claude/agents/
cp -r ~/.claude/skills/<their-skill> .claude/skills/
```

Or they can drag-and-drop in Finder/VS Code.

**d. Test that Claude picks them up**

Open Claude Cowork or Claude Code in the project folder. Trigger the agent or invoke the skill — it should activate just like it did in their personal `~/.claude/`. This proves the project-scoped `.claude/` works.

> **Teaching beat:** Pause here and point out that Claude found the agent/skill *only because of the folder structure*. The markdown content is identical to what they had before — naming and location is what made it discoverable.

### 2. Pose the sharing problem (2 min)

Stop the room and ask: *"Your teammate Sarah loves this. How do you give it to her?"*

Let them suggest copy-paste, Slack, Drive. Then ask: *"What if you change it next week?"* This is where they feel the pain plugins solve.

### 3. Ask Claude to package it as a plugin (10 min)

Students open Claude Code/Cowork in their project folder and ask in plain English. Two prompts that work — pick one:

**Option A — single plugin, all components:**

> "Create a plugin called `<my-name>-tools` from my `.claude/` folder. Include the `<agent-name>` agent and the `<skill-name>` skill. Set up a marketplace.json too, and validate the result."

**Option B — multiple plugins, students specify the split:**

> "Create two plugins from my `.claude/` folder under one marketplace.
> - `<plugin-1>` — should include the `<agent>` agent and the `<skill-1>` skill
> - `<plugin-2>` — should include the `<skill-2>` skill
>
> Set up the marketplace.json and validate."

Claude will:
- Create `plugins/<name>/` folder(s) with the correct structure
- Move/copy `.claude/agents/*` and `.claude/skills/*` into the right plugin
- Write `.claude-plugin/plugin.json` for each plugin
- Write `.claude-plugin/marketplace.json` at the repo root
- Run `plugin-validator` to check everything

**Teaching moment:** open one of their agent or skill markdown files before and after. Point out the file *content* is byte-for-byte identical — only the folder location changed. The plugin is just a packaging convention.

> Why this conversational approach instead of `/plugin-dev:create-plugin`?
> The `/plugin-dev:create-plugin` skill is an 8-phase wizard built for *greenfield* plugins ("describe what you want to build"). Your students already have working components — a one-line conversational request is faster, doesn't ask them to re-describe what they already wrote, and is more realistic of how they'll actually use Claude day-to-day. Save the wizard for "next time you start from scratch."

### 4. Push to GitHub (5 min)

```bash
cd ~/Code/<my-name>-tools
git init
git add .
git commit -m "Initial plugin"
gh repo create <my-name>-tools --private --source=. --push
```

They now have a live private repo containing their plugin.

### 5. Partner install (10 min)

Pair students up. Each shares their repo URL with their partner. Partner runs:

```
/plugin marketplace add <partner-github-handle>/<their-repo>
/plugin install <plugin-name>@<marketplace-name>
```

They invoke each other's agents/skills. **This is the moment plugins click** — they see their teammate's work running on their machine in 30 seconds.

### 6. Add a second plugin (8 min) — the marketplace payoff

Have each student add a *second* plugin folder to their repo:

```
<my-name>-tools/
└── plugins/
    ├── <first-plugin>/
    └── <second-plugin>/         ← new
        ├── .claude-plugin/plugin.json
        └── skills/<something-new>/
```

They register the new plugin in `marketplace.json`, push.

Partner runs:

```
/plugin install <second-plugin>@<marketplace-name>
```

**No re-adding the marketplace.** That's the punchline of the whole lesson — the marketplace is added *once*, plugins are added forever.

### 7. Wrap (2 min)

Remind them of the three-layer model:

| Layer | What it is | Where it's named |
|---|---|---|
| Repo | Where the code lives | GitHub URL |
| Marketplace | Catalog of plugins | `.claude-plugin/marketplace.json` |
| Plugin | The thing teammates install | `plugins/<name>/.claude-plugin/plugin.json` |

Distinct names for each make the structure obvious. Same name causes confusion.

## Common issues & answers

**"Cowork won't let me push."** — Correct, Cowork can install but not push. Push from your local machine.

**"Why `.claude-plugin/` and not `.claude/`?"** — `.claude/` is project/user config. `.claude-plugin/` is the manifest that turns a folder into a shareable, installable package. Same idea, different scope.

**"Do I need `gh` CLI?"** — To *install* plugins, no. To *create and push your own*, yes (or use the GitHub web UI to create the repo and `git push` manually).

**"Can one repo have multiple plugins?"** — Yes — that's the whole point of `marketplace.json`. Add a folder under `plugins/<name>/`, register it in the marketplace, push.

**"Should I split my work into one plugin or two?"** Two questions to answer:
1. Would someone install A without B? If yes, separate plugins.
2. Does the agent rely on those skills? If yes, bundle them.

## Optional follow-ups

- Run `plugin-dev:plugin-validator` on their repo and walk through the output.
- Introduce `/plugin-dev:create-plugin` as the wizard for *next-time, from scratch* plugin creation.
- Show how to host a plugin in a separate repo and reference it from the marketplace via `"source": { "source": "github", "repo": "owner/repo" }`.
