---
name: init-latex-project
description: Initialize LaTeX Academic Project with standard structure, macros, and writing guide. Use when user wants to create a new LaTeX paper project for any conference or journal.
argument-hint: <project-name> [target-dir] [--venue <iclr|cvpr|icml|acm|acl|neurips>] [--git]
allowed-tools: Read, Write, Bash, Glob
---

# Initialize LaTeX Academic Project

Set up a complete LaTeX academic paper project from the standard template.

## Skill Directory Layout

```
~/.claude/skills/init-latex-project/
├── SKILL.md
├── templates/
│   ├── macros.tex                  # Full math macro library
│   ├── main.tex                    # Generic arXiv/whitepaper template
│   ├── CLAUDE.md                   # Writing rules for AI agents
│   └── venues/
│       ├── iclr/main.tex           # ICLR single-column template
│       ├── cvpr/main.tex           # CVPR two-column template
│       ├── icml/main.tex           # ICML two-column template
│       ├── acm/main.tex            # ACM SIGCONF template
│       ├── acl/main.tex            # ACL/*ACL single-column template
│       └── neurips/main.tex        # NeurIPS single-column template
└── scripts/
    └── init.sh                     # Shell script that builds the project
```

---

## Steps to Follow

### 1. Parse `$ARGUMENTS`

| Position/Flag | Meaning | Required |
|---|---|---|
| First word | Project name | Yes |
| Second word | Target directory (default: current dir) | No |
| `--venue <name>` | Conference venue | No |
| `--git` | Initialize git repo | No |

Supported venues: `iclr`, `cvpr`, `icml`, `acm`, `acl`, `neurips`

### 2. Run the init script

```bash
bash ~/.claude/skills/init-latex-project/scripts/init.sh <project-name> [target-dir] [--venue <venue>] [--git]
```

**Important**: Use the absolute path to `init.sh`. Do NOT use `~` — expand it to the actual home directory (e.g., `/Users/username/.claude/skills/...`). Find the correct home directory from the environment or from previous messages.

### 3. Offer to immediately customize

After the script runs, ask the user if they want to:
- Update `sections/title.tex` with their actual title
- Fill in authors and affiliations in `main.tex`
- Any other immediate adjustments

### 4. Print the script's output to the user

The script prints a file tree and venue-specific setup notes. Make sure the user sees these.

---

## Venue Details

### ICLR
- **Layout**: Single-column
- **Document class**: `\documentclass{article}` + `\usepackage[submitted]{iclr2026_conference}`
- **Bibliography**: `iclr2026.bst`
- **Extra sections**: `sections/impact.tex` (optional Ethics Statement)
- **Style files**: https://github.com/ICLR/Master-Template

### CVPR
- **Layout**: Two-column (10pt, letterpaper)
- **Document class**: `\documentclass[times,10pt,twocolumn,letterpaper]{article}` + `\usepackage[review]{cvpr}`
- **Bibliography**: `ieeenat_fullname.bst`
- **Important**: Use `figure*` / `table*` for full-width floats in two-column layout
- **Style files**: https://github.com/cvpr-org/author-kit

### ICML
- **Layout**: Two-column (via icml2026.sty)
- **Document class**: `\documentclass{article}` + `\usepackage{icml2026}`
- **Bibliography**: `icml2026.bst` (APA format)
- **MANDATORY**: `sections/impact.tex` — Broader Impact Statement, does NOT count toward page limit
- **Extra**: Uses ICML-specific author macros (`\icmltitle`, `\icmlauthor`, etc.)
- **Style files**: https://icml.cc/Downloads/2026

### ACM (SIGCONF)
- **Layout**: Varies (sigconf is typically single-column for submission)
- **Document class**: `\documentclass[sigconf,review,anonymous]{acmart}`
- **Bibliography**: `ACM-Reference-Format.bst`
- **Author format**: Each author has separate `\author{}`, `\affiliation{}`, `\email{}` commands
- **CCS Concepts**: Required for camera-ready (the `\ccsdesc` commands)
- **Warning**: `acmart` conflicts with some packages — see comments in main.tex
- **Style files**: Usually pre-installed in TeX Live / MiKTeX

### NeurIPS
- **Layout**: Single-column
- **Document class**: `\documentclass{article}` + `\usepackage{neurips_2025}`
- **Bibliography**: `abbrvnat` (natbib, loaded by neurips_2025.sty)
- **MANDATORY**: `sections/impact.tex` — Broader Impact Statement, does NOT count toward page limit
- **MANDATORY**: `sections/checklist.tex` — Full 15-item Author Checklist (pre-filled where possible), does NOT count toward page limit
- **Submission modes**: plain (anonymous) / `[preprint]` (arXiv) / `[final]` (camera-ready)
- **Algorithm package**: Uses `algpseudocode` (provides `\State`, `\Require`, `\Ensure`, `\For`, `\EndFor`, `\Comment`). Do NOT use the older `algorithmic` package which requires ALL-CAPS commands (`\STATE`, `\REQUIRE`, etc.).
- **Style files**: https://neurips.cc/Conferences/2025/PaperInformation/StyleFiles

### ACL / *ACL Venues (EMNLP, NAACL, EACL, COLING)
- **Layout**: Single-column
- **Document class**: `\documentclass[11pt]{article}` + `\usepackage[review]{acl}`
- **Bibliography**: `acl_natbib.bst`
- **Citation style**: `\citet{}` for in-text, `\cite{}` (= `\citep{}`) for parenthetical
- **Extra sections**: `sections/limitations.tex` + `sections/ethics.tex`
- **Style files**: https://github.com/acl-org/acl-style-files

---

## What Is Always Included (Every Venue)

**`macros.tex`** — complete math macro library:
- Calligraphic `\calA`…`\calZ`, bold `\bA`…`\bZ`/`\ba`…`\bz`
- Blackboard bold `\R` `\E` `\P` `\B`
- Greek shortcuts `\sG` `\lG` `\tG` `\pa` `\half`
- Operators `\argmin` `\argmax` `\Softmax` `\Sigmoid` `\Var`
- Sum shortcuts `\sumN` `\sumK` `\sumM` `\sumT`
- Theorem environments (grey tcolorbox): `theorem` `lemma` `definition` `remark` `assumption`
- Author comment macros `\jerry{}` `\wwm{}` `\jieke{}` etc.

**Standard section files** (all placeholder):
`title.tex`, `abstract.tex`, `intro.tex`, `related.tex`, `method.tex`, `exp.tex`, `conclusion.tex`, `appendix.tex`, `acknowledgement.tex`

**`bib/refs.bib`** — empty with usage example

---

## Example Invocations

```
/init-latex-project my-new-paper
/init-latex-project my-iclr-paper ~/Papers --venue iclr --git
/init-latex-project my-cvpr-paper . --venue cvpr
/init-latex-project my-icml-paper . --venue icml --git
/init-latex-project my-acm-paper . --venue acm
/init-latex-project my-acl-paper ~/Papers --venue acl
/init-latex-project my-neurips-paper . --venue neurips --git
```

---

## Package Conflict Notes

The `macros.tex` library is loaded **last** in all venue templates (after venue packages), so our definitions take precedence. Known conflicts:

| Venue | Potential Conflict | Resolution |
|---|---|---|
| ACM | `acmart` redefines `\P` | Our `\P = \mathbb{P}` overrides it — intentional |
| ACM | `acmart` has its own `\cite` | `\let\cite\citep` in macros.tex may interfere — comment it out if needed |
| CVPR | Two-column layout | `tcolorbox` theorem envs may span columns — switch to plain `amsthm` if needed |
| ICML | Uses its own `hyperref` setup | Load macros.tex after `icml2026` to avoid double-load errors |
| NeurIPS | `algorithmic` vs `algpseudocode` | Always use `algpseudocode` (lowercase `\State`, `\Require`, etc.). The older `algorithmic` package uses ALL-CAPS commands and is incompatible with modern algorithm code. |
| **ALL** | **`\def\(`, `\def\)`, `\def\[`, `\def\]` in macros.tex** | **PERMANENTLY DISABLED. These four lines redefined LaTeX's built-in math-mode delimiters (`\( \)` = inline math; `\[ \]` = display math), which broke ALL math environments (`$...$`, `\begin{equation}`, `algorithmic` states, etc.) with cascading "Missing $" and "Extra )" errors. Never re-enable. Use `\left( ... \right)` explicitly instead.** |
| **ALL** | **`\usepackage{algorithm}` + `\usepackage{algorithmic}` in macros.tex** | **PERMANENTLY DISABLED. Loading both `algorithm`/`algorithmic` alongside `algpseudocode` (required by NeurIPS, ICML, ICLR) causes "Command \algorithmicindent already defined" and similar errors. The `algorithm` float wrapper should be loaded once in `main.tex` together with exactly one of `algpseudocode` (modern, recommended) or `algorithmic` (legacy).** |
