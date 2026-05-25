# Lessons

Living queue of actionable insights. Check when integrated into the harness. Remove checked items on cleanup. Unchecked = pending review.

## Pending

- [ ] **Terminal rewind is state reconstruction:** A terminal recording is an event stream, not independent frames. Backward seek requires reset/replay, snapshots, or terminal emulation.
- [ ] **Use the web player as UX reference:** asciinema's web player already has richer control semantics than terminal `asciinema play`; borrow its keyboard model before inventing a new one.
- [ ] **Git fork is a boundary:** Keep design work in this harness until Duane explicitly creates or authorizes the upstream fork/clone path.
- [ ] **`.cast` can be a playback engine for semantic sessions:** Keep Pi session JSONL as canonical tree data, then compile directed session narratives into `.cast` plus metadata for controlled replay.

## Integrated

- [x] Bootstrapped Cast as an enso harness instance.
