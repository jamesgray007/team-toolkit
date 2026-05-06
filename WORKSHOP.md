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
- [ ] Have at least one rough idea for an agent or skill they want to build (e.g., a meeting-notes summarizer, a brand-voice checker, a code-review helper)

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

### 1. Build locally in `.claude/` (10 min)

Each student:

```bash
mkdir -p ~/Code/<my-name>-tools/.claude/agents
mkdir -p ~/Code/<my-name>-tools/.claude/skills
cd ~/Code/<my-name>-tools
```

They create:
- One simple agent: `.claude/agents/<name>.md`
- One simple skill: `.claude/skills/<name>/SKILL.md`

**Keep them small.** A 15-line skill is better than a perfect one. Examples:
- `meeting-summarizer` skill — turns notes into action items
- `brand-checker` agent — reviews writing against a voice guideline

They test it in Cowork/Claude Code by triggering the agent or invoking the skill from inside that folder.

### 2. Pose the sharing problem (2 min)

Stop the room and ask: *"Your teammate Sarah loves this. How do you give it to her?"*

Let them suggest copy-paste, Slack, Drive. Then ask: *"What if you change it next week?"* This is where they feel the pain plugins solve.

### 3. Convert to a plugin — conversationally (10 min)

Have them open Claude Code/Cowork in their project folder and say:

> "I have skills and agents in `.claude/`. Convert this folder into a Claude Code plugin called `<my-name>-tools`, set up a marketplace.json, and validate the result."

Claude will:
- Move `.claude/agents/*` and `.claude/skills/*` into a plugin layout
- Create `.claude-plugin/plugin.json`
- Create `.claude-plugin/marketplace.json`
- Run `plugin-validator` to check everything

**Teaching moment:** open the agent/skill markdown before and after. Point out the file content is identical — only the folder structure changed.

> Why conversational instead of `/plugin-dev:create-plugin`?
> The wizard is built for greenfield plugins. Students already have working components — a one-line conversational request is faster and more realistic of how they'll actually work day-to-day. Save the wizard for "next time you start from scratch."

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
