# Project Skills

A collection of Claude Code skills and slash commands for research and ML engineering.

## Overview

| Type | What it does | Installs to |
|------|-------------|-------------|
| **Skills** | Extend Claude Code with new capabilities | `~/.claude/skills/` |
| **Commands** | Slash commands (`/cmd`) for tasks and workflows | `~/.claude/commands/` |

---

## Skills

Skills are loaded on-demand by Claude Code. Install them once; Claude activates them automatically when relevant.

### `init-latex-project`

Initialize a LaTeX academic paper project with standard structure, macros, and writing guide.

**Supports:** ICLR, CVPR, ICML, ACM SIGCONF, ACL/*ACL

**Usage:**
```
/init-latex-project my-paper --venue iclr --git
/init-latex-project my-cvpr-paper . --venue cvpr
```

**What you get:**
- Venue-specific `main.tex` with correct document class and bibliography style
- Full math macro library (`macros.tex`): calligraphic, bold, blackboard bold, operators, theorem environments
- Standard section files: abstract, intro, related, method, exp, conclusion, appendix
- Empty `bib/refs.bib`

---

### `init-python-project`

Initialize a production-ready Python/ML project, or enhance an existing fork.

**Usage:**
```
/init-python-project
```

**What you get:**
- `src/` layout with editable install via `uv`
- Isolated `tests/` structure mirroring `src/`
- `docs/` with three layers: `outlines/` (planning), `dev/` (features), `src/` (modules)
- Dev tooling: pytest, black, ruff, mypy
- `.env` / `.env.example`, VSCode/Cursor/Claude settings
- Project `CLAUDE.md` with development principles

**Supports both:**
- **New project**: full scaffold from scratch
- **Fork/Enhancement**: analyze existing repo and add missing components

---

### `prompt-manager`

Set up and manage prompts for **LLM agent projects** using a production-grade prompt management system.

If you are building an LLM agent (trading agent, research assistant, RAG pipeline, etc.), hardcoding prompts as Python strings quickly becomes unmanageable. This skill introduces a structured approach: prompts live in versioned YAML + Jinja2 files, separate from your code, with caching and parameter validation built in.

**Trigger:** When working on an LLM agent project and needing to create, organize, version, or manage prompts.

**What you get:**
- YAML config + Jinja2 template separation
- Version management (v1, v2, v3 coexist)
- Multi-level caching (50–90% speedup)
- Pydantic parameter validation
- Hot-reload dev mode

**Directory layout:**
```
prompts/
├── configs/          # YAML: metadata, parameters, LLM config
└── templates/
    ├── common/       # Reusable Jinja2 components
    ├── system/       # System prompts (v1.jinja2, v2.jinja2, ...)
    └── user/         # User prompts
```

---

## Commands

All commands are available as `/command-name` in Claude Code.

### Project Setup

#### `/init-python-project`
Alias that triggers the `init-python-project` skill interactively.

#### `/new-workspace`
Create a new Git branch or worktree for experiments or features.

- Supports `feature/<name>` and `exp/<name>` branch types
- For worktrees: auto-copies `.vscode`, `.cursor`, `.claude` configs; creates symlinks via `.worktree-links`; runs `uv sync`

#### `/project-init`
Initialize a new research project with aligned paper and code repositories under a shared parent folder.

**Output:**
```
~/Projects/<ProjectName>/
├── paper/     ← LaTeX repo (uses init-latex-project)
├── code/      ← Python ML repo (uses init-python-project)
└── PROJECT.md ← Overview linking both repos
```

`PROJECT.md` documents the research overview (method, datasets, metrics) and defines the workflow between paper and code. The paper repo gets `sections/daily_experiments.tex` — a running log for experiment results.

---

### During Development

#### `/update-docs`
Analyse code changes since the last docs update and refresh documentation files.

- Detects project root and docs location automatically
- Finds baseline commit (last docs commit) and diffs code changes
- Maps changes to affected doc files and makes surgical edits
- Preserves existing writing style and language
- Optionally commits the updated docs

#### `/project-sync`
Sync new experiment results from the code repo into the paper's experiment log.

- Auto-detects `code/` and `paper/` sibling directories
- Asks for experiment details, or auto-reads from `outputs/logs/`
- Previews the LaTeX entry before inserting
- Inserts at top of `daily_experiments.tex` (reverse chronological)
- Optionally commits to the paper repo

---

### At Milestones

#### `/add-git-tag`
Mark a project milestone with an annotated Git tag.

**Example tag format:**
```
Tag: v0.2.0
Date: 2026-03-06

## ✅ This Phase — What Was Achieved
- Implemented baseline model
- Added CIFAR-10 data pipeline

## 🚀 Next Phase — What's Planned
- Ablation studies
- Add attention module
```

---

### Device Setup

#### `/setup-git-github`
Configure Git global settings and set up GitHub SSH authentication on a new device.

- Generates ed25519 SSH key
- Configures `~/.ssh/config`
- Guides through adding key to GitHub and tests the connection

> Asks for your name and email interactively — no hardcoded personal info.

#### `/sync-config`
Sync your `~/.claude/` configuration to remote SSH servers via rsync.

- Smart sync: compares timestamps before pushing/pulling
- Safe excludes: skips session data, credentials, history

> Edit the file to pre-fill your server aliases, or leave it interactive.

---

## Installation

```bash
git clone git@github.com:a-green-hand-jack/projects-skills.git ~/Projects/project-skills
```

### Install skills

```bash
cp -r ~/Projects/project-skills/skills/* ~/.claude/skills/
```

### Install commands

```bash
cp ~/Projects/project-skills/commands/* ~/.claude/commands/
```

### Install everything

```bash
cp -r ~/Projects/project-skills/skills/* ~/.claude/skills/
cp ~/Projects/project-skills/commands/* ~/.claude/commands/
```

---

## How Everything Fits Together

```
/project-init
  ├── init-latex-project  (skill)
  └── init-python-project (skill)
        └── /new-workspace

During development:
  ├── prompt-manager   (skill)
  ├── /update-docs
  └── /project-sync

At milestones:
  ├── /add-git-tag
  └── /sync-config
```

---

## Contributing

Each skill is a directory with `SKILL.md` + optional `scripts/` and `templates/`. Each command is a single `.md` file. To contribute:

1. Edit or add the relevant file
2. Test by invoking it in Claude Code
3. Submit a PR describing what changed and why

See `AGENTS.md` for detailed guidelines.
