---
name: sync-research-skills
description: "Sync research skills between local ~/.claude/skills/ and the remote research repo (Auto-claude-code-research-in-sleep). Use when user says '同步skill', 'sync skills', 'push skills', 'pull skills', 'update research repo', '更新研究仓库'."
argument-hint: "[push|pull|status] [skill-name...]"
allowed-tools: Bash(*), Read, Write, Edit, Glob, Grep
---

# Sync Research Skills

Synchronize research-related skills between local `~/.claude/skills/` and the remote GitHub repo.

## Paths

- **Local skills**: `~/.claude/skills/`
- **Research repo**: `/Users/bytedance/research/Auto-claude-code-research-in-sleep/`
- **Repo skills dir**: `/Users/bytedance/research/Auto-claude-code-research-in-sleep/skills/`
- **Git remotes**:
  - `origin` → `https://github.com/LeonCheung033/Auto-claude-code-research-in-sleep.git` (user's fork)
  - `upstream` → `https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep.git` (original author)

## Research Skills List

The following skills belong to the research pipeline and should be synced:

```
analyze-results, arxiv, auto-paper-improvement-loop, auto-review-loop,
auto-review-loop-llm, auto-review-loop-minimax, comm-lit-review, dse-loop,
experiment-bridge, experiment-plan, feishu-notify, grant-proposal,
idea-creator, idea-discovery, idea-discovery-robot, monitor-experiment,
novelty-check, paper-compile, paper-figure, paper-illustration,
paper-notes-lite, paper-plan, paper-self-review, paper-slides,
paper-write, paper-writing, pixel-art, proof-writer, research-lit,
research-pipeline, research-refine, research-refine-pipeline,
research-review, results-analysis, review-response, run-experiment,
writing-anti-ai
```

Non-research skills (development, plugins, design) live only in `~/.claude/skills/` and are NOT synced.

## Commands

### `push` — Push local skill changes to research repo + GitHub

```bash
# 1. Copy modified skills from local to repo
cp -r ~/.claude/skills/<skill-name>/SKILL.md \
  /Users/bytedance/research/Auto-claude-code-research-in-sleep/skills/<skill-name>/SKILL.md

# 2. Commit in research repo
cd /Users/bytedance/research/Auto-claude-code-research-in-sleep
git add skills/
git commit -m "feat(skills): update <skill-name> — <brief description>"

# 3. Push to fork
git push origin main
```

### `pull` — Pull latest from upstream (original author) and update local

```bash
# 1. Fetch upstream
cd /Users/bytedance/research/Auto-claude-code-research-in-sleep
git fetch upstream

# 2. Merge upstream changes
git merge upstream/main --no-edit

# 3. Push merged result to fork
git push origin main

# 4. Copy updated skills to local
# For each research skill:
cp -r skills/<skill-name>/SKILL.md ~/.claude/skills/<skill-name>/SKILL.md
```

### `status` — Show diff between local and repo

```bash
# For each research skill, diff local vs repo version
diff ~/.claude/skills/<skill-name>/SKILL.md \
  /Users/bytedance/research/Auto-claude-code-research-in-sleep/skills/<skill-name>/SKILL.md
```

## Workflow

### Adding a new skill to the research repo

1. Create the skill locally: `~/.claude/skills/<new-skill>/SKILL.md`
2. Test it works
3. Run `/sync-research-skills push <new-skill>`
4. Verify on GitHub

### Updating an existing skill

1. Edit `~/.claude/skills/<skill>/SKILL.md`
2. Test changes
3. Run `/sync-research-skills push <skill>`

### Getting upstream updates

1. Run `/sync-research-skills pull`
2. Resolve any conflicts if local modifications diverge from upstream
3. Test affected skills

## Rules

- **Always test locally before pushing** — a broken skill in the repo affects all machines
- **Commit messages follow Conventional Commits**: `feat(skills):`, `fix(skills):`, `docs(skills):`
- **Never force push to origin/main** — use merge workflow
- **Back up local skill before overwriting with upstream version** if you have local-only changes
- **Check `git status` before any operation** to avoid committing unrelated changes
