---
name: ask-rem-sdlc
description: Router for rem-sdlc — which skill or phase to use.
disable-model-invocation: true
---

# Ask rem-sdlc

**One product main flow** — the gated cycle. Planning starts with **`grill-me`**. Multi-CLI **ai-judge**, **rem-ralph** (not upstream `ralph`), and **session-close** (Obsidian) are on-ramps.

## Main flow

```text
discover-agents (once)
    ↓
grill-me → [ai-judge plan] → intake → implement|rem-ralph → validate → review → [ai-judge code] → pr
    ↓ on budget / end
session-close → Work/{project}/{handoff,session,memory,teach}/{ts}.md
```

| Situation | Do this |
| --- | --- |
| First time | `/rem-sdlc:init` + `/rem-sdlc:discover-agents` |
| Refresh CLI inventory for judge | `/rem-sdlc:discover-agents` |
| Planning / design / brainstorm | **`grill-me`** (default) or `grill-with-docs` |
| Multi-model ranking of plan/code | **`ai-judge`** / `/rem-sdlc:judge` |
| Autonomous story loop (our fork) | **`rem-ralph`** / **`rem-ralf`** / `/rem-sdlc:rem-ralph` |
| Upstream snarktank convert only | their skill id `ralph` (if installed separately) |
| Ticket ready | `/rem-sdlc:intake` or `cycle` |
| Token/session limit or end of day | **`session-close`** / `/rem-sdlc:session-close` |
| Commits | **`commit-work`** |
| Unsure | this router |

Never replace cycle implement/review with matt `implement`/`code-review` on the same ticket.

## Budgets & vault

`session.max_tokens`, `session.max_iterations`, `session.vault` in `_/sdlc-config.md`.  
Vault layout: `Work/{project}/handoff|session|memory|teach/{timestamp}.md` → [RemGrace/obsidian-personal](https://github.com/RemGrace/obsidian-personal).

## Install on this agent

If commands/skills are missing: **[docs/install.md](../../docs/install.md)** (Claude · Codex · Grok · Cursor · Amp · …).

## Optional companions

`tdd`, `diagnosing-bugs`, `codebase-design`, `domain-modeling`, `handoff`, `writing-great-skills` — see [docs/mattpocock-skills.md](../../docs/mattpocock-skills.md).
