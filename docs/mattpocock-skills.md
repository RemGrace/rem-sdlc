# Matt Pocock skills with rem-sdlc

How the [mattpocock/skills](https://github.com/mattpocock/skills) kit maps onto this plugin after the prune.

## Default planning: `grill-me`

For **any** planning or design work in this project, start with:

| Goal | Skill |
| --- | --- |
| Plan / design / "brainstorm" | **`grill-me`** (default) |
| Same + write `CONTEXT.md` / ADRs | **`grill-with-docs`** (+ `domain-modeling`) |
| Stress-test only (model can open it) | `grilling` |

Then: optional **`ai-judge`** (`plan` mode) → `/rem-sdlc:intake` → cycle.

Do **not** use freeform brainstorming as the planning path.

## Bundled companions (safe with the cycle)

Already under `skills/` — they **do not** replace cycle phases:

| Skill | Use with rem-sdlc |
| --- | --- |
| `tdd` | During `/rem-sdlc:implement` at agreed seams |
| `diagnosing-bugs` | When validate/pipeline fails mysteriously |
| `codebase-design` | When reshaping modules / seams mid-design |
| `domain-modeling` | Via grill-with-docs or when glossary drifts |
| `handoff` | Switch agent/session mid-ticket |
| `writing-great-skills` | Editing this plugin's skills |
| `ai-judge` | Multi-CLI second opinions on plan/code |

## Strong host installs (recommended)

Install on the **application** repo (not required inside this plugin):

```bash
npx skills@latest add mattpocock/skills \
  -s prototype \
  -s research \
  -s wayfinder \
  -s improve-codebase-architecture \
  -s codebase-design
```

| Skill | When it helps rem-sdlc |
| --- | --- |
| **`prototype`** | After grill: settle a UI/state question with throwaway code before intake |
| **`research`** | Background cited research before or during grill |
| **`wayfinder`** | Huge multi-session initiatives (foggy roadmap) → then collapse into tickets + rem-sdlc intake |
| **`improve-codebase-architecture`** | Periodic deepening; output can feed grill-me |

## Full matt main flow (optional, host-only)

A **second** idea→ship pipeline. Fine for work that is *not* going through tracks/gates. **Do not** mix with `/rem-sdlc:implement` on the same ticket.

```bash
npx skills@latest add mattpocock/skills \
  -s setup-matt-pocock-skills \
  -s ask-matt \
  -s to-spec \
  -s to-tickets \
  -s implement \
  -s code-review \
  -s triage
```

```text
grill-with-docs → to-spec → to-tickets → implement(+tdd) → code-review
```

vs rem-sdlc:

```text
grill-me → [ai-judge] → intake → implement → validate → review → pr
```

Pick **one** pipeline per unit of work.

## Suggested combined rhythm

```text
0. discover-agents                # inventory CLIs → _/coding-agents.md
1. grill-me / grill-with-docs     # planning (default)
2. prototype / research           # only if a decision needs evidence
3. ai-judge (plan)                # multi-CLI ranking (peers from inventory)
4. rem-sdlc:intake → cycle   # gated delivery
   — or rem-ralph: convert track/PRD → prd.json, then loop (our fork of snarktank/ralph; not skill id `ralph`)
5. ai-judge (code)                # optional peer critique
6. rem-sdlc:review            # still the CRITICAL gate
7. session-close                  # handoff/session/memory/teach → Obsidian
```

## Obsidian vault

Session artifacts go to [RemGrace/obsidian-personal](https://github.com/RemGrace/obsidian-personal):

`Work/{project-name}/{handoff,session,memory,teach}/{timestamp}.md`

Configure `session.vault` in `_/sdlc-config.md`.
