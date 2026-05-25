# Cast Architecture

## Overview

Cast is currently a design harness for a better `.cast` playback CLI. The likely implementation path is either an asciinema CLI fork/patch or a separate Rust command that reuses the asciicast model.

The key architectural shift from upstream `asciinema play` is moving from mostly linear event streaming to an indexed playback model: the player needs to know where it is, where markers are, and how to reconstruct terminal state at a target time.

A second product seam is emerging: use `.cast` as the playback engine for directed Pi session replays. Pi's session JSONL remains the semantic source of truth; Cast can compile selected conversation-tree paths into terminal event streams with markers, labels, and sidecar metadata.

## Components

| Component | Responsibility |
|-----------|----------------|
| Enso harness | Persistent planning, research, stories, architecture, and lessons |
| Agent orchestration surface | Route research/design/implementation/verification work while preserving a single synthesis thread |
| Upstream asciinema CLI | Reference implementation for recording, parsing, and current terminal playback behavior |
| Asciinema web player | Reference implementation for richer playback controls and seek semantics |
| Cast CLI | Future command-line interface for controlled playback |
| Pi session projector | Future compiler from Pi session JSONL/tree entries into directed `.cast` playback plus metadata |
| Playback controller | Handles key events and maps them to play/pause/seek/step/marker actions |
| Timeline index | Converts asciicast events into a navigable event/time/marker structure |
| Terminal state restoration | Rebuilds terminal state after backward/absolute seek; initial strategy: reset terminal then fast replay to target |
| Verification corpus | Sample `.cast` files covering markers, resize events, long recordings, alternate screen, colors, and edge cases |

## Key Decisions

| Decision | Rationale |
|----------|-----------|
| Start as design harness, not immediate fork checkout | Git authority belongs to Duane; design can proceed without mutating repo history or cloning upstream. |
| Model the CLI after the web player's control semantics | The web player already solved many UX questions: percent jumps, marker prev/next, frame stepping, seek tokens. |
| Treat rewind as terminal-state reconstruction | Terminal output is historical state mutation, not frames. Backward seek needs replay, snapshots, or terminal emulation. |
| First rewind strategy: terminal reset + fast replay | Simple, compatible, likely good enough for demos. Optimize with checkpoints later only if needed. |
| Keep `.cast` compatibility | The format is the ecosystem seam. Do not fork the format for playback controls. |
| Keep Pi session JSONL canonical | `.cast` is a projection/playback engine for sessions, not the source of truth for Pi's semantic tree. |

## Open Questions

- Should the first implementation patch upstream `src/player.rs`, or build a separate `cast` binary that imports/adapts asciinema parsing code?
- How much flicker is acceptable during reset-and-replay seeks?
- Do we need an on-screen status/help overlay, or is stdout/stderr messaging enough?
- How should resize events be handled when replaying into a terminal of different dimensions?
- Should marker labels be edited into `.cast` files manually, via a helper, or via future recorder improvements?
- For Pi session playback, should entry IDs and branch metadata live in marker labels, custom ignored event codes, or a sidecar index file?
