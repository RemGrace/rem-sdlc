---
name: rem-ralph
description: "rem-sdlc fork of snarktank/ralph вЂ” convert PRDs/tracks/grill plans to prd.json and run the autonomous story loop under _/rem-ralph/. Use when user says rem-ralph, rem-ralf, convert to ralph format for rem-sdlc. Does NOT replace upstream snarktank /ralph skill."
disable-model-invocation: true
---

# rem-ralph (fork of snarktank/ralph)

**Skill id: `rem-ralph`** вЂ” deliberately *not* named `ralph`, so it does not collide with the upstream [snarktank/ralph](https://github.com/snarktank/ralph) skill when both are installed.

Upstream converter: https://github.com/snarktank/ralph/blob/main/skills/ralph/SKILL.md  
Alias: **`rem-ralf`**

Ralph runs a **fresh agent instance per iteration** until every user story in `prd.json` has `passes: true`. Memory between iterations = git history + `progress.txt` + `prd.json`.

## Jobs

1. **Convert** PRD / grill recap / track в†’ `prd.json` (snarktank format)
2. **Run** the loop (rem-sdlc budgets, multi-CLI inventory, session-close)

---

## Working directory

```text
_/rem-ralph/
  prd.json
  progress.txt
  prompt.md             # from templates/rem-ralph-prompt.md
  archive/
  .last-branch
```

Config override: `rem_ralph.dir` in `_/sdlc-config.md` (default `_/rem-ralph`).

Legacy: if only `_/ralph/` exists from older docs, migrate/rename to `_/rem-ralph/` before writing.

---

## Job A вЂ” Convert to prd.json

### Inputs

- Markdown PRD / plan
- Grill-me shared-understanding recap
- `_/tracks/<TICKET>.md` ACs + requirements

### Output format

```json
{
  "project": "[Project Name]",
  "branchName": "ralph/[feature-name-kebab-case]",
  "description": "[Feature description]",
  "userStories": [
    {
      "id": "US-001",
      "title": "[Story title]",
      "description": "As a [user], I want [feature] so that [benefit]",
      "acceptanceCriteria": [
        "Criterion 1",
        "Criterion 2",
        "Typecheck passes"
      ],
      "priority": 1,
      "passes": false,
      "notes": ""
    }
  ]
}
```

(`branchName` still uses the `ralph/` git prefix вЂ” that is snarktankвЂ™s branch convention, not this skillвЂ™s id.)

See `templates/rem-ralph-prd.example.json`.

### Story size вЂ” number one rule

**Each story must fit ONE iteration (one context window).**

Right-sized: column + migration; one UI component; one server action.  
Too big: вЂњbuild the dashboardвЂќ, вЂњadd authвЂќ вЂ” split them.

### Ordering

1. Schema / migrations  
2. Backend  
3. UI  
4. Aggregations  

### Acceptance criteria

Verifiable only. Always include `"Typecheck passes"`. Logic: `"Tests pass"`. UI: `"Verify in browser"`.

### From track в†’ prd.json

- Map each open requirement в†’ one story  
- `branchName` в†ђ `ralph/<ticket-slug>` or project branch pattern  

### Archive previous runs

If existing `prd.json` has a different `branchName` and `progress.txt` has content в†’ copy to `archive/YYYY-MM-DD-feature-name/`, reset progress.

### Checklist

- [ ] Stories one-iteration sized, dependency-ordered  
- [ ] Every story has Typecheck passes  
- [ ] UI stories have browser verify  
- [ ] Previous run archived if needed  

---

## Job B вЂ” Run the loop

### Prerequisites

1. `_/rem-ralph/prd.json`  
2. `progress.txt` + `prompt.md` (seed from template)  
3. Optional: `_/coding-agents.json` via **discover-agents**  

### Outer loop

```text
for i in 1..max_iterations:
  spawn FRESH agent with prompt.md
  implement ONE story where passes: false
  commit, set passes: true, append progress.txt
  if all passes: true в†’ <promise>COMPLETE</promise>
```

Script: `scripts/rem-ralph.sh --tool claude|codex|grok|amp|host [max_iterations]`

### Each iteration

1. Read prd.json + progress.txt  
2. Branch from `branchName`  
3. One story only  
4. Quality checks (project pipeline)  
5. **commit-work** вЂ” never commit `_/`  
6. `passes: true` + progress append  
7. All done в†’ `<promise>COMPLETE</promise>`  

### Budgets

`session.max_iterations` / `session.max_tokens` / `session.on_limit` в†’ **session-close** on limit or COMPLETE.

### After loop

Still run `/rem-sdlc:validate` + `/rem-sdlc:review` + `/rem-sdlc:pr` вЂ” this skill does not replace cycle gates.

---

## Name conflict policy

| Name | Who |
| --- | --- |
| `ralph` (skill) | **Upstream only** вЂ” snarktank/ralph marketplace |
| **`rem-ralph`** | This plugin |
| **`rem-ralf`** | Alias в†’ this skill |
| `/rem-sdlc:rem-ralph` | Plugin command |

If the user says plain вЂњralphвЂќ and both skills exist, prefer clarifying: upstream convert vs **rem-ralph** (track-aware, `_/rem-ralph/`, vault close).

## Commands

| User | Action |
| --- | --- |
| convert for rem-sdlc | Job A в†’ `_/rem-ralph/prd.json` |
| `/rem-sdlc:rem-ralph` | convert and/or run |
| rem-ralph / rem-ralf | this skill |

## References

- https://github.com/snarktank/ralph  
- https://ghuntley.com/ralph/  
- `templates/rem-ralph-prompt.md`, `templates/rem-ralph-prd.example.json`  
- `scripts/rem-ralph.sh`  
