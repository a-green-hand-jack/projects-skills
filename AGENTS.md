# AGENTS.md

Guidance for AI coding agents working in this repository.

## Repository Overview

This repo contains Claude Code skills and slash commands for research and ML engineering. Each file is an instruction document for an AI agent — not executable code.

## Directory Structure

```
project-skills/
├── skills/    # → ~/.claude/skills/   (one subdirectory per skill)
└── commands/  # → ~/.claude/commands/ (one .md file per command)
```

## Adding a New Skill

Skills live in `skills/<skill-name>/` and require a `SKILL.md` with YAML frontmatter:

```markdown
---
name: skill-name
description: One sentence describing when to use this skill. Include trigger phrases.
allowed-tools: Read, Write, Edit, Bash, Glob
---

# Skill Title

Brief description.

## When to Use This Skill
...

## Steps
...
```

**Guidelines:**
- Keep `SKILL.md` under 500 lines; move reference material to separate linked files
- Use `scripts/` for shell automation (scripts don't consume context, only their output does)
- Use `templates/` for file templates the skill writes out
- Write a specific `description` field — this determines when the skill auto-activates

## Adding a New Command

Commands live in `commands/<command-name>.md`. Plain Markdown with a `#` title and structured steps.

Commands should:
- Be self-contained (complete one task end-to-end)
- Ask the user for any required personal configuration rather than hardcoding it
- Show a preview before taking destructive or irreversible actions
- Use `// turbo` comments before steps that should run without user confirmation

## Personal Configuration

Two commands require personal info — keep them generic (interactive) in this repo:

- `commands/setup-git-github.md` — asks user for name/email at runtime
- `commands/sync-config.md` — asks user for server aliases at runtime

Users who want pre-filled versions can maintain a local override in `~/.claude/commands/`.

## Testing

1. Copy the file to `~/.claude/skills/` or `~/.claude/commands/`
2. Invoke in Claude Code (`/command-name` or describe the task)
3. Verify output and iterate

No automated tests — correctness is validated by running the skill.
