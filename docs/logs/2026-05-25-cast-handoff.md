# Session: Cast Handoff
**Date:** 2026-05-25

## Overview

Bootstrapped `/home/dedmonds/base/cast` as an enso harness/design workspace for a new `cast` command-line playback project. The project started from the idea of forking or patching asciinema to add better terminal playback controls, then expanded to a second seam: using `.cast` as a playback engine for directed Pi session replays.

## Key Decisions

- `cast` starts as a harness/design workspace, not an upstream fork checkout.
- Git authority remains with Duane; agents should not clone/fork/commit without explicit handoff.
- The first active story is controlled playback CLI design, not implementation.
- The web asciinema player is the UX reference for richer control semantics.
- Terminal rewind is state reconstruction; first viable strategy is terminal reset plus fast replay to target.
- Pi session playback should keep Pi session JSONL as canonical tree data and compile selected views into `.cast` plus sidecar metadata.

## Artifacts Modified

- `AGENTS.md` — enso operational norms, project seams, orchestration lanes, git boundary
- `SOUL.md` — project stance and product instincts
- `README.md` — landing page for the project
- `docs/core/PRD.md` — problem, goals, scope, constraints
- `docs/core/architecture/ARCHITECTURE.md` — architecture, decisions, Pi session playback seam
- `docs/core/architecture/playback-control-design.md` — placeholder for STORY-001 design output
- `docs/reference/LESSONS.md` — pending lessons for rewind, web-player UX, git boundary, Pi session projection
- `docs/stories/STORY-001-controlled-playback-cli-design.md` — active first story
- `.opencode/skills` — symlink to `docs/skills`

## Current State

The harness is ready to resume from `/home/dedmonds/base/cast`. No implementation work has started. No upstream asciinema fork/clone has been created by an agent.

## Next Steps

1. Start next session in `/home/dedmonds/base/cast`.
2. Load `SOUL.md`, `AGENTS.md`, `docs/core/PRD.md`, `docs/core/architecture/ARCHITECTURE.md`, and `docs/stories/STORY-001-controlled-playback-cli-design.md`.
3. Execute STORY-001: research upstream asciinema CLI/web-player controls and fill `docs/core/architecture/playback-control-design.md`.
4. Decide whether first implementation should patch asciinema CLI or build a separate `cast` binary.
