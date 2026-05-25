# Cast PRD

## Problem

`asciinema play` is good at replaying a recording, but weak as a human-controlled playback surface. During terminal playback, the user has only minimal controls: pause/resume, step forward while paused, next marker, and quit. That is not enough for live demos, review, teaching, or exploratory playback.

The `.cast` format already has enough structure to support better control. The missing piece is a command-line player designed around indexed playback, marker navigation, seek/rewind semantics, and a small, learnable control language.

## Goals

1. **Demo-grade control.** Support pause/resume, step forward/back, seek forward/back, percent jumps, and marker navigation from the terminal.
2. **Marker-first playback.** Treat markers as chapters/breakpoints with previous/next navigation and optional auto-pause.
3. **Terminal-correct rewind.** Define and validate a rewind/seek strategy that restores terminal state predictably, likely via reset plus fast replay to target.
4. **Asciicast compatibility.** Preserve compatibility with asciicast v2/v3 recordings; do not invent a new recording format unless forced.
5. **Upstream-aware path.** Keep the design clean enough that it could become an asciinema fork patch, an upstream proposal, or a separate `cast` tool.

## Scope

**In scope:**
- CLI UX design for a `cast play` or standalone `cast` command
- Playback controls and keybinding model
- Timeline/event indexing strategy
- Marker discovery and navigation semantics
- Seek/rewind implementation design
- Harness and story structure for research, implementation, and verification
- Source research into asciinema CLI, asciinema web player, and alternative `.cast` players

**Out of scope:**
- Replacing asciinema's recorder
- Replacing asciinema's web player
- Upload/server functionality
- GIF/SVG/video export
- A full terminal emulator UI unless terminal-state restoration requires it
- Creating a GitHub fork or cloning upstream without Duane explicitly crossing that git boundary

## Constraints

- Start with design and source research before implementation.
- Favor the smallest viable playback engine change.
- Keep compatibility with ordinary terminal playback.
- Treat rewind/seek as the central technical risk.
- Respect git authority: no agent-owned fork/clone/branch mutation.
