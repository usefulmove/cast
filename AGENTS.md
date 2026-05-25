---
protocol: enso
version: 0.6.6
audience: agent
operations: [Write, Select, Probe, Compress, Isolate, Assign]
directories:
  core: docs/core/
  stories: docs/stories/
  reference: docs/reference/
  skills: docs/skills/
  logs: docs/logs/
---

> Identity, voice, and stance live in [`SOUL.md`](./SOUL.md). Read it first. SOUL.md wins on stance; AGENTS.md wins on operational norms.

# Cast

Cast is an enso harness instance for designing and, if we choose, building a better terminal playback CLI for asciinema `.cast` recordings.

The near-term product idea is a `cast` command: a demo-grade playback interface with real user control during terminal replay.

## Operating stance

- Technical work gets technical language: precise, compact, low-ceremony.
- Retrieval beats memory. For upstream asciinema behavior, inspect source/docs before asserting.
- Push back before agreeing if a simpler or safer path exists.
- Make low-risk harness edits directly; report what changed.

## Project seams

| Seam | Interface | Enabling Point |
|------|-----------|----------------|
| Planning -> Execution | Story docs with Goal, AC, Context Scope, Approach, Verification | `docs/stories/*.md` |
| Ephemeral -> Persistent | Write / Select / Probe / Compress / Isolate / Assign | Agent explicitly persists docs, logs, lessons, or skills |
| Agent -> Codebase | Context Scope: Write / Read / Exclude | Active story scope |
| Agent -> Capability | `SKILL.md` frontmatter | `docs/skills/<name>/` via `.opencode/skills` symlink |
| Stance -> Protocol | `SOUL.md` + `AGENTS.md` | Harness injection |

## Agent orchestration surface

This repo is the surface where orchestration work happens. The primary agent keeps the thread, assigns specialist work only when useful, and returns synthesis in one voice.

Expected specialist lanes:

- **Upstream research** — asciinema CLI/player source, issues, docs, existing alternatives.
- **Playback engine design** — timeline indexing, seek/rewind semantics, terminal reset/replay strategy.
- **CLI UX design** — keybindings, help overlay/status line, demo-mode flow.
- **Implementation** — Rust changes if/when we cross from design into code.
- **Verification** — sample casts, marker navigation, seek accuracy, terminal behavior.

## Directory map

| Path | Purpose |
|------|---------|
| `docs/core/PRD.md` | Product problem, goals, and scope |
| `docs/core/architecture/` | Architecture maps and design decisions |
| `docs/stories/` | Active work stories |
| `docs/reference/` | Durable reference, lessons, completed stories |
| `docs/skills/` | Local reusable skills |
| `docs/logs/` | Session summaries |

## Working rules

1. Read `SOUL.md` before work.
2. Load `docs/core/PRD.md` and the active story before execution.
3. No code modifications until the active story has Approach and Verification filled in.
4. Stay inside the story Context Scope.
5. Update architecture docs when a design decision lands.
6. Add lessons to `docs/reference/LESSONS.md` when we learn something reusable.
7. Write a session log at meaningful checkpoints.

## Version control boundary

Git authority belongs to Duane.

Agents may use read-only git inspection commands. Agents must not stage, commit, amend, push, pull, fetch, merge, rebase, reset, clean, switch branches, delete refs, or otherwise mutate git state.

Forking/cloning upstream asciinema is an explicit handoff boundary. Until Duane crosses it, this repo is a harness/design workspace, not a fork checkout.

## Templates

### Story

```markdown
# STORY-XXX Title
**Branch:** `{ticket-id}-{slug}` *(omit for harness-only stories)*
**Worktree:** `not yet created`

## Goal

## Acceptance Criteria
- [ ]

## Context Scope
**Write:**
**Read:**
**Exclude:**

## Approach & Verification Plan

### Steps
1.

### Risks & Unknowns
-

### Verification
- [ ]

### Reflection
- [ ] New lesson? Update `docs/reference/LESSONS.md`.
- [ ] New subsystem/decision? Update `docs/core/architecture/`.
```

### Session Summary

```markdown
# Session: [Topic]
**Date:** YYYY-MM-DD

## Overview

## Key Decisions
-

## Artifacts Modified
-

## Next Steps
-
```
