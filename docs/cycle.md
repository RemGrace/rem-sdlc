# The cycle

`rem-sdlc` turns a ticket into a merged PR through gated phases. The track file `_/tracks/<TICKET>.md` is the single source of truth — every command reads YAML frontmatter and writes a delta back.

```text
*  /grill-me                      DEFAULT planning (not brainstorming)
*  /rem-sdlc:discover-agents    inventory coding CLIs for ai-judge
*  /rem-sdlc:judge              optional multi-CLI plan/code court
0  /rem-sdlc:init               project profile → _/sdlc-config.md
1  /rem-sdlc:intake             ticket + spec → track + size s|m|l
2  /rem-sdlc:implement          code + tests; pipeline per requirement
*  /rem-sdlc:rem-ralph        optional autonomous prd.json loop
3  /rem-sdlc:validate           Playwright assertions → demo .webm
4  /rem-sdlc:review             code + security + standards
5  /rem-sdlc:pr                 push + open PR
5b /rem-sdlc:pr-comments        reply to review threads
6  /rem-sdlc:revalidate         post-review re-check + drift
*  /rem-sdlc:cycle              orchestrator 1→6
*  /rem-sdlc:session-close      handoff/session/memory/teach → vault
```

**Planning:** always prefer **`grill-me`** when scope is fuzzy. Optional **`judge plan`** for multi-model ranking. See [mattpocock-skills.md](mattpocock-skills.md).

## Phase gates

| Phase | Gate |
| --- | --- |
| intake | Track validates against `schemas/track.schema.json`; every AC has ≥1 requirement |
| implement | All in-scope requirements `status: done`; pipeline green |
| validate | 0 assertion failures; no new console errors / network 4xx–5xx (size `s`: pipeline only) |
| review | No unaddressed CRITICAL findings |
| pr | Pipeline green; branch pushed; PR opened |
| revalidate | Requirements still pass; spec hash unchanged or drift ack’d |

`/rem-sdlc:cycle` enforces gates between phases. Manual commands enforce their own gates.

## Why gates?

Agentic tools often “produce code and claim success.” Gates flip that: every transition has a verifiable condition. The track remembers where you stopped so resume stays reliable.

## Size-adaptive rigor

| Size | Sequence | Notes |
| --- | --- | --- |
| **s** | intake → implement → validate → pr | No e2e/demo; review skipped by default; pipeline is the gate. **Requires explicit user confirm.** |
| **m** | full sequence | Default; missing `size` ⇒ `m` |
| **l** | mother + sub-tracks per AC group | Fan-out then aggregate PR |

## When to skip a phase

- **Backend-only / non-UI:** validate may write `na` and skip Playwright; implement tests remain the gate.
- **No formal spec:** `spec.convention: freeform` at init.
- **No ticket tracker:** `ticketing.system: none`; freeform slug tracks.
- **Hot-fix without intake:** not recommended; still create a thin track so the journal exists.

## Artifacts (`_/` — gitignored)

```text
_/
├── sdlc-config.md
├── coding-agents.md / .json     # discover-agents
├── tracks/<TICKET>.md
├── rem-ralph/                 # prd.json, progress.txt, prompt.md
├── judge/<run-id>/              # case + peer verdicts
├── demo/
│   ├── credentials.json
│   └── <TICKET>.spec.mjs
└── recordings/
    ├── <TICKET>.validation.md
    ├── <TICKET>.review.md
    ├── <TICKET>.judge.md
    └── <TICKET>.latest.webm
```

Durable artifacts for teammates: the **PR body** (and optional Obsidian notes from session-close).

## Manual vs orchestrated

| Mode | When |
| --- | --- |
| **Manual** | Learning the cycle; unusual projects; step-by-step control |
| **Orchestrated** (`cycle`) | Happy path; optional worktrees between phases |

### Worktrees

Only the **cycle orchestrator** may use git worktrees for phase isolation. Manual commands and skills do **not** require worktrees. If worktrees fail or are unavailable, run phases in-process using `agents/*.md`.

## Resuming

- `/rem-sdlc:cycle <TICKET> --resume` continues from the last non-passing phase in `phase_log`.
- Individual commands are idempotent (e.g. implement skips `done` requirements).

Journal (§7 of the track) is append-only.

## Multi-agent note

Slash commands work natively in Claude Code. On Codex, Grok, Cursor, Amp, etc., open the matching `commands/<phase>.md` (or use the `rem-sdlc` skill adapter) and execute with that harness’s tools. See [install.md](install.md).
