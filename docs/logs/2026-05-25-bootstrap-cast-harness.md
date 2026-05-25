# Session: Bootstrap Cast Harness
**Date:** 2026-05-25

## Overview

Created `/home/dedmonds/base/cast` as an enso harness instance for a new `cast` playback command-line interface design project. The project is scoped around better human control during asciinema `.cast` playback, with rewind/seek/marker navigation as the central design problem.

## Key Decisions

- Start as a harness/design workspace rather than immediately cloning/forking upstream asciinema.
- Respect the git boundary: Duane owns fork/clone/branch history decisions.
- Use asciinema's web player as the primary UX reference for richer controls.
- Treat terminal rewind as state reconstruction; first candidate strategy is terminal reset plus fast replay to target.

## Artifacts Modified

- `AGENTS.md` — enso operational norms and project seams
- `SOUL.md` — project stance and product instincts
- `docs/core/PRD.md` — problem, goals, scope, constraints
- `docs/core/architecture/ARCHITECTURE.md` — initial architecture and decisions
- `docs/reference/LESSONS.md` — initial lessons
- `docs/stories/STORY-001-controlled-playback-cli-design.md` — active first story
- `.opencode/skills` — symlink to `docs/skills`

## Next Steps

- Execute STORY-001 by researching upstream asciinema CLI/player source and drafting `docs/core/architecture/playback-control-design.md`.
- Decide after design whether to patch/fork asciinema or build a separate `cast` binary.
