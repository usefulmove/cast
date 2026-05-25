# STORY-001 Controlled Playback CLI Design
**Branch:** *(harness-only story)*
**Worktree:** `not yet created`

## Goal

Design the first viable `cast` playback command-line interface for human-controlled asciicast playback, using upstream asciinema CLI/player behavior as source material.

## Acceptance Criteria

- [ ] Document the current upstream `asciinema play` control model and source locations.
- [ ] Document relevant asciinema web player controls and seek semantics.
- [ ] Define the proposed `cast` CLI command shape and keybindings.
- [ ] Define playback state model: playing, paused, ended, seeking, marker pause.
- [ ] Define initial rewind/seek algorithm and known limitations.
- [ ] Identify sample `.cast` recordings needed for verification.
- [ ] Decide whether the first implementation should patch asciinema CLI or build a separate `cast` binary.

## Context Scope

**Write:**
- `docs/stories/STORY-001-controlled-playback-cli-design.md`
- `docs/core/architecture/ARCHITECTURE.md`
- `docs/core/architecture/playback-control-design.md`
- `docs/reference/LESSONS.md`
- `docs/logs/*.md`

**Read:**
- `docs/core/PRD.md`
- `docs/core/architecture/ARCHITECTURE.md`
- `AGENTS.md`
- `SOUL.md`
- Upstream asciinema docs/source via web or future local checkout
- Existing local terminal recordings in `/home/dedmonds/base/terminal-recordings/`

**Exclude:**
- Mutating any git repository
- Creating/cloning/fetching an upstream fork without Duane's explicit handoff
- Modifying files outside `/home/dedmonds/base/cast/`

## Approach & Verification Plan

### Steps

1. Research upstream CLI playback source and docs.
2. Research web player controls and seek implementation.
3. Compare existing alternative `.cast` players for useful UX/engine ideas.
4. Draft `docs/core/architecture/playback-control-design.md` with command UX, keybindings, state model, and seek algorithm.
5. Update this story with decisions and remaining risks.

### Risks & Unknowns

- Backward seek may flicker or be slow for long recordings if implemented by reset-and-replay.
- Alternate screen, resize events, and terminal modes may make simple replay less predictable.
- Upstream asciinema's current architecture may resist clean patching.
- A separate `cast` binary may duplicate parser/playback code unless carefully scoped.

### Verification

- [ ] Design cites concrete upstream source/docs rather than memory.
- [ ] Proposed controls cover live demo needs: pause, resume, step, previous/next marker, jump, rewind, fast-forward, quit, help.
- [ ] Seek algorithm explains how terminal state is restored.
- [ ] Verification corpus includes at least one recording with markers and one with terminal resize/output complexity.

### Reflection

- [ ] New lesson? Update `docs/reference/LESSONS.md`.
- [ ] New subsystem/decision? Update `docs/core/architecture/`.
