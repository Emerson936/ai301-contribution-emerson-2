# Contribution [#1852]: metronome marks aren't written to lilypond files

**Contribution Number:** 2
**Student:** Emerson Palaganas  
**Issue:** https://github.com/cuthbertLab/music21/issues/1852  
**Status:** [Phase I] Completed

---

## Why I Chose This Issue

I chose to contribute to this issue because it combines my technical skills with my passions. Working with Python allows me to use a language I am familiar with, while music21 connects with my int[...]

---

## Understanding the Issue

### Problem Description

When a `MetronomeMark` (a tempo indication specifying BPM and note duration) is included in a music21 `Stream`, it does not appear when converting to LilyPond format (`.ly` files). Other elements [...]

### Expected Behavior

The converted LilyPond file has `\tempo` which gives the tempo of the score.

### Current Behavior

The converted LilyPond file does not have `\tempo` even though everything else gets converted properly

### Affected Components

`music21/lily/translate.py` seems to be where the conversion happens. Fixing the issue probably requires to edit this file.

---

## Reproduction Process

### Environment Setup

Use `git clone https://github.com/Emerson936/music21.git` to clone the repositiory. 

### Steps to Reproduce

1. Open `file.ly`
2. You'll find key signature, time signature, and notes, but nothing for tempo

### Reproduction Evidence

- **Commit showing reproduction:** No need for a commit to reproduce this
- **My findings:** The reported found two other bugs that may fix this:
  1. Using scalar notation alone produces invalid LilyPond syntax.
  2. The docs say to pass a steno duration like "quarter", but in practice "4" is required instead.

---

## Solution Approach

### Analysis

MetronomeMark objects are not written to LilyPond because the LilyPond translator lacks logic to convert MetronomeMark attributes into a `\tempo` command, causing tempo data to be dropped during export.

### Proposed Solution

Add serialization in `music21/lily/translate.py` to detect `MetronomeMark` instances and emit a correctly formatted `\tempo` line using the note unit and BPM.

### Implementation Plan

Implement conversion logic and a helper to compute the LilyPond unit string in `music21/lily/translate.py`, add unit tests that convert Streams with `MetronomeMark` and assert the resulting `.ly` contains the expected `\tempo`, then run and adjust until tests pass.

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: `LyTempoEvent` doctests confirm `stenoDuration` + `scalar` (no range) now emits valid `\tempo 4 = 87` instead of the old bare/invalid `\tempo 87`.
- [ ] Test case 2: `testMetronomeMark` — asserts `lyEmbeddedScmFromMetronomeMark()` converts a numbered `MetronomeMark` correctly and returns None for a numberless one.
- [ ] Test case 3: Fixed LyStenoDuration docstring example (was passing invalid 'quarter' instead of '4'), now verified via doctest.

### Integration Tests

- [ ] Integration scenario 1: `testMetronomeMarkWrittenInStream` builds a full Stream (key, tempo, time sig, note) and confirms `\tempo 4 = 87` appears in the assembled LilyPond output.
- [ ] Integration scenario 2: Ran the issue's exact repro through `stream.write('lily', ...)` and confirmed the compiled `.ly` file passes real `lilypond` compilation without syntax errors.

### Manual Testing

I first verified the logic by monkeypatching around the missing local LilyPond binary and confirming correct `\tempo` output for several durations (quarter, dotted eighth) and the no-number edge case. After installing LilyPond (`brew install lilypond`), I reran the full test suite for real and confirmed all tests pass, with `ruff/mypy` clean across the package.

---

## Implementation Notes

### Week [X] Progress

Investigated GitHub issue #1852 (MetronomeMark objects silently dropped from LilyPond output), traced the root cause to a missing dispatch branch in translate.py plus two latent bugs in the unused LyTempoEvent class, then implemented and tested the fix. Main challenge was verifying correctness locally without a LilyPond binary installed — worked around it with a monkeypatch, then confirmed for real after installing LilyPond via Homebrew.

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** music21/lily/translate.py, music21/lily/lilyObjects.py
- **Key commits:** [Links to important commits]
- **Approach decisions:** Reused the existing lyMultipliedDurationFromDuration() duration-conversion helper rather than duplicating that logic, and followed the established lyEmbeddedScmFrom* pattern (Clef/KeySignature/TimeSignature) for consistency instead of introducing a new dispatch style. Fixed LyTempoEvent itself (rather than routing around it) since the issue explicitly reported bugs in that class.

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** The Lilypond translator's element dispatch never recognized MetronomeMark, so tempo markings were dropped entirely when writing to lily. Also fixes LyTempoEvent.stringOutput(), which ignored a paired stenoDuration when no tempoRange was given, and its docstring, which showed an invalid steno duration ('quarter' instead of '4').

Fixes #1852

AI-assisted with Claude (>10 lines).

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
