---
description: Initialize a new research project with aligned paper and code repositories under a shared parent folder.
---

# Project Init Workflow

Use this workflow when starting a new research project that requires both a LaTeX paper repo and a Python code repo, managed in parallel under a shared project folder.

## Expected Output Structure

```
~/Projects/<ProjectName>/
├── paper/    ← LaTeX repo (init-latex-project)
├── code/     ← Python ML repo (init-python-project)
└── PROJECT.md ← Project overview linking both repos
```

---

## Step 1 — Gather Project Information

Ask the user the following in a **single message**:

1. **Project name**: What is the project called? (used as the parent folder name, e.g. `DemoProject`)
2. **Parent directory**: Where should this project live? (default: `~/Projects/`)
3. **Paper venue**: Which conference/journal? (`iclr`, `cvpr`, `icml`, `acm`, `acl`, or `none` for generic arXiv)
4. **Research summary** (brief): What is the method? What datasets/benchmarks? What metrics?
   - This will be used to pre-fill the `paper/` structure and `PROJECT.md`
5. **GitHub**: Do you have GitHub repos ready for paper and code? If yes, provide SSH URLs (or skip to add later).

Wait for the user's answers before proceeding.

---

## Step 2 — Create the Parent Folder

```bash
mkdir -p <parent-dir>/<ProjectName>
```

---

## Step 3 — Initialize the Paper Repo

Run the `init-latex-project` skill:

```bash
bash ~/.claude/skills/init-latex-project/scripts/init.sh <ProjectName>-paper <parent-dir>/<ProjectName>/paper [--venue <venue>] --git
```

> **Note**: Expand `~` to the actual home directory (e.g. `/Users/jieke`). Use `--venue` only if a venue was specified; omit for generic arXiv.

After the script runs:

1. Add a `sections/daily_experiments.tex` file to the paper:

```latex
% Daily Experiments Log
% Add experiment entries in reverse chronological order (newest first).
%
% Template for each entry:
%   \subsection*{YYYY-MM-DD — <short title>}
%   \textbf{Setup:} <method variant, dataset, config>\\
%   \textbf{Result:} <key numbers, metric values>\\
%   \textbf{Observation:} <what worked, what didn't>\\
%   \textbf{Next:} <follow-up experiment planned>
```

2. Add `\input{sections/daily_experiments}` to `paper/main.tex` in the appendix area (before `\end{document}`), with a section header:

```latex
\section*{Daily Experiments (Internal)}
\input{sections/daily_experiments}
```

3. If GitHub SSH URL was provided for paper:
```bash
git -C <parent-dir>/<ProjectName>/paper remote add origin <paper-github-url>
git -C <parent-dir>/<ProjectName>/paper push -u origin main
```

---

## Step 4 — Initialize the Code Repo

Invoke the `init-python-project` command. Tell the user:

> "Now let's set up the code repo. I'll run `/init-python-project` for the `code/` directory."

Use the `/init-python-project` command with the following context already established:
- **Target directory**: `<parent-dir>/<ProjectName>/code/`
- **Project type**: `new`, ML project
- **GitHub URL**: from Step 1 (if provided)

Follow all steps in `init-python-project` as normal. The code repo will be initialized at `<parent-dir>/<ProjectName>/code/`.

---

## Step 5 — Create PROJECT.md in the Parent Folder

Write `<parent-dir>/<ProjectName>/PROJECT.md` with the following content (fill in from the user's research summary in Step 1):

```markdown
# <ProjectName>

> <One-line description of the research project>

## Research Overview

**Method**: <method description>
**Datasets**: <datasets used>
**Benchmarks**: <benchmarks / baselines compared against>
**Metrics**: <evaluation metrics>

## Repository Structure

| Repo | Path | Purpose |
|------|------|---------|
| paper | `./paper/` | LaTeX paper (<venue or arXiv>) |
| code  | `./code/`  | Python implementation (uv)     |

## GitHub Remotes

| Repo | URL |
|------|-----|
| paper | <paper-github-url or TBD> |
| code  | <code-github-url or TBD> |

## Workflow

- **Design changes** (method, datasets, metrics): update `paper/sections/method.tex` first, then implement in `code/`
- **Experiment results**: record in `code/experiments/`, then sync to `paper/sections/daily_experiments.tex`
- **Milestones**: use `add-git-tag` workflow in each repo separately
- **Code docs**: use `update-docs` workflow in `code/`

## Key Files

- `paper/sections/method.tex` — canonical method description
- `paper/sections/exp.tex` — main experiments section
- `paper/sections/daily_experiments.tex` — running experiment log
- `code/docs/outlines/project_plan.md` — implementation roadmap
- `code/experiments/` — raw experiment outputs and logs
```

---

## Step 6 — Final Summary

Report to the user:

```
Project initialized: <ProjectName>

paper/  → <paper-github-url or "local only">
code/   → <code-github-url or "local only">

Key workflows for this project:
  - /project-sync   → sync experiment results from code to paper
  - add-git-tag     → mark milestones in either repo
  - update-docs     → refresh code documentation

Next steps:
  1. Fill in paper/sections/method.tex with your method details
  2. Fill in paper/sections/exp.tex with planned experiments
  3. Start implementing in code/src/
  4. When you have results, run /project-sync to log them in paper
```
