---
name: auto-mark-asciinema
description: Automatically detect and insert navigation markers into asciinema .cast recordings. Use when a terminal recording needs chapter points for `asciinema play` jumps (`]`/`[`), or when you want heuristic scene detection (prompts, input starts, output bursts, submits).
license: MIT
compatibility: opencode
---

## When to Use

- A `.cast` recording is long enough that jumping to chapter points would help review
- You want `asciinema play FILE.cast` to support `]` (next marker) and `[` (prev marker) out of the box
- Manual marker placement is tedious and you want AI heuristics to suggest natural boundaries
- You've discovered a `.cast` file has header-only `markers` that **do not** work with the player; inline `"m"` events arerequired

---

## Background: Header vs Inline Markers

`.cast` v2 supports two marker representations:

1. **Header array** `"markers": [[time, "label"], ...]` — human-readable metadata, ignored by `asciinema play`
2. **Inline events** `[time, "m", ""]` — these are what the player scans when you press `]` or `[`

**Both must be present** for the file to be fully useful: header markers carry the labels, inline events make the player jump. The tools below maintain both.

---

## Scripts

Place these in the project's `bin/` or any `$PATH` directory.

| Script | Purpose |
|--------|---------|
| `asciinema-marker` | Manual add/list/clear markers (both header + inline) |
| `asciinema-marker-analyze` | Heuristic auto-detection of 5 narrative states |

---

### 1. asciinema-marker

```
asciinema-marker FILE.cast add TIME LABEL        # absolute
asciinema-marker FILE.cast add-relative SEC LABEL # relative to start
asciinema-marker FILE.cast list
asciinema-marker FILE.cast clear
```

### 2. asciinema-marker-analyze

```
asciinema-marker-analyze FILE.cast           # dry-run suggestions
asciinema-marker-analyze FILE.cast --apply   # write markers inline + header
asciinema-marker-analyze FILE.cast --json    # machine-readable suggestions
```

---

## Heuristic States

The analyzer tracks a simple FSM in the event stream:

```
INIT -> PROMPT -> TYPING -> SUBMITTED -> OUTPUT -> PROMPT
```

Detected transitions become markers:

| Label | Trigger | Typical meaning |
|-------|---------|-----------------|
| `session_start` | First stable prompt | Recording begins |
| `prompt_ready` | Prompt after idle gap | New command context |
| `input_start` | First keystroke after prompt | User begins typing |
| `command_submit` | Bracketed-paste end / CRLF | Enter pressed |
| `output_start` | Burst of ≥60 chars after submit | Results begin |
| `prompt_return` | Prompt after output | Command finished |

---

## Workflow

1. **Analyze** the `.cast` to see what the heuristics found:
   ```bash
   asciinema-marker-analyze recording.cast
   ```

2. **Apply** markers if they look reasonable:
   ```bash
   asciinema-marker-analyze recording.cast --apply
   ```

3. **Review** with `asciinema play`:
   ```bash
   asciinema play recording.cast
   # Press ] to jump to next marker
   # Press [ to jump to previous marker
   # Press space to pause at any marker (if --pause-on-markers set)
   ```

4. **Tweak** manually if the agent missed a boundary or added a false positive:
   ```bash
   asciinema-marker recording.cast add 12.5 "custom label"
   asciinema-marker recording.cast clear   # start over
   ```

---

## Integration Notes

- The analyzer's prompt-detection regex checks for `❭` (used in this shell theme) plus standard `$`, `#`, and `>` prompts. Customize the regex inside `MarkerAnalyzer._contains_prompt()` for other themes.
- Tunables (`IDLE_TO_PROMOT`, `MAX_TYPING_GAP`, `OUTPUT_BURST_CHARS`) are module-level constants near the top of the analyzer script.
- Clearing markers removes **both** the header array and any inline `"m"` events, restoring the `.cast` to a pristine event stream.

## Checklist

- [ ] Scripts are executable and in `$PATH`
- [ ] A backup `.cast` exists before `--apply`
- [ ] `asciinema play` with `]` / `[` jumps correctly between markers
- [ ] Header `"markers"` array is present for third-party tools
- [ ] Labels are meaningful for the recording's narrative
