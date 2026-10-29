---
description: rem-ralph (fork of snarktank/ralph) — convert PRD/track to prd.json and/or run autonomous story loop under _/rem-ralph/. Named to avoid conflict with upstream /ralph skill.
argument-hint: "[convert|run] [prd-path|ticket-id] [--tool claude|codex|grok|amp|host] [--max N]"
allowed-tools: ["Bash", "Read", "Write", "Edit", "Glob", "Grep", "Agent"]
---

# /rem-sdlc:rem-ralph

Canonical skill: [`skills/rem-ralph/SKILL.md`](../skills/rem-ralph/SKILL.md)  
Upstream (separate install): [snarktank/ralph](https://github.com/snarktank/ralph) skill id `ralph`

## Arguments

- `$1`: `convert` | `run` | omit (convert if no prd.json, else run)
- `$2`: path to PRD markdown, or ticket id
- `--tool <id>`: loop executor (default `rem_ralph.tool` or `claude`)
- `--max N`: max iterations

## Workflow

### convert

1. Load **rem-ralph** skill Job A.
2. Input: `$2` file, track, or conversation plan.
3. Archive previous `_/rem-ralph/prd.json` if different `branchName`.
4. Write `_/rem-ralph/prd.json` + init `progress.txt`.
5. Seed `prompt.md` from `templates/rem-ralph-prompt.md` if missing.
6. Next: `/rem-sdlc:rem-ralph run --max 10`.

### run

1. Ensure prd.json exists under `_/rem-ralph/`.
2. Enforce session budgets.
3. Prefer `scripts/rem-ralph.sh --tool … N`; else one in-process iteration (Job B).
4. On COMPLETE or limit → **session-close**.
5. Remind: validate + review before PR.

## Gate

- convert: valid prd.json, Typecheck on every story, dependency order  
- run: COMPLETE, max iterations, or budget + session-close  
