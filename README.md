# Contribution [#1852]: metronome marks aren't written to lilypond files

**Contribution Number:** 2
**Student:** Emerson Palaganas  
**Issue:** https://github.com/cuthbertLab/music21/issues/1852  https://github.com/cuthbertLab/music21/pull/1993
**Status:** [Phase IV] Complete

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
**Understand:** `MetronomeMark` objects added to a `Stream` were silently dropped when writing to LilyPond format (`stream.write('lily', ...)`) — no `\tempo` marking appeared anywhere in the output, with no error raised.

**Match:** `LilypondConverter.appendM21ObjectToContext` already had working dispatch branches for similar contextual markers (`Clef`, `KeySignature`, `TimeSignature`) that each delegate to a dedicated `lyEmbeddedScmFrom*` method; `MetronomeMark` just needed the same pattern, reusing the existing `lyMultipliedDurationFromDuration()` helper for duration-to-LilyPond conversion.

**Plan:**
1. Fix `LyTempoEvent.stringOutput()` in `lilyObjects.py` to correctly emit `\tempo <duration> = <bpm>` when a `stenoDuration` is paired with a `scalar` and no `tempoRange`.
2. Add `lyEmbeddedScmFromMetronomeMark()` to `translate.py`, following the existing `lyEmbeddedScmFromKeySignature`/`lyEmbeddedScmFromTimeSignature` pattern.
3. Wire a `'MetronomeMark' in c` branch into `appendM21ObjectToContext`'s dispatch chain.
4. Update tests (doctests in `lilyObjects.py`, new unit tests in `translate.py`).

**Implement:** [PR #1993](https://github.com/cuthbertLab/music21/pull/1993) — commits `9fd67a441` and `3d839a885` on branch [`fix/lily-metronome-mark-1852`](https://github.com/Emerson936/music21/tree/fix/lily-metronome-mark-1852).

**Review:** Confirmed against `CONTRIBUTING.md`/`CLAUDE.md`: AI-assistance disclosed, `Fixes #1852` linked in the PR, `ruff`/`mypy` clean, camelCase/docstring conventions followed, and a `New in v11` marker added per the versioning guidelines.

**Evaluate:** Verified via new unit tests (`testMetronomeMark`, `testMetronomeMarkWrittenInStream`) and doctests, then confirmed end-to-end by reproducing the original issue and compiling the generated `.ly` file with a real LilyPond installation.

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

### Week 1 Progress

Investigated GitHub issue #1852 (MetronomeMark objects silently dropped from LilyPond output), traced the root cause to a missing dispatch branch in translate.py plus two latent bugs in the unused LyTempoEvent class, then implemented and tested the fix. Main challenge was verifying correctness locally without a LilyPond binary installed — worked around it with a monkeypatch, then confirmed for real after installing LilyPond via Homebrew.

### Code Changes

- **Files modified:** music21/lily/translate.py, music21/lily/lilyObjects.py
- **Key commits:** https://github.com/cuthbertLab/music21/pull/1993/commits
- **Approach decisions:** Reused the existing lyMultipliedDurationFromDuration() duration-conversion helper rather than duplicating that logic, and followed the established lyEmbeddedScmFrom* pattern (Clef/KeySignature/TimeSignature) for consistency instead of introducing a new dispatch style. Fixed LyTempoEvent itself (rather than routing around it) since the issue explicitly reported bugs in that class.

---

## Pull Request

**PR Link:** https://github.com/cuthbertLab/music21/pull/1993

**PR Description:** The Lilypond translator's element dispatch never recognized MetronomeMark, so tempo markings were dropped entirely when writing to lily. Also fixes LyTempoEvent.stringOutput(), which ignored a paired stenoDuration when no tempoRange was given, and its docstring, which showed an invalid steno duration ('quarter' instead of '4').

Fixes #1852

AI-assisted with Claude (>10 lines).

**Maintainer Feedback:** 
- August 5: Owner appreciated the PR and work. There were some small issues with my contribution that were fixed by the owner
- August 7: Owner merged code for lily pad enhancement

**Status:** Merged

---

## Learnings & Reflections

### Technical Skills Gained

- Prompt Engineering: Gained more experience speaking and interacting with AI. Learned how to give it context around a problem better
- Python Skills: Even though AI did a lot of coding, having that Python knowledge still helped

### Challenges Overcome

- Giving the AI guidelines: AI did way more than it should have during a few iterations of this. To prevent this, I had to be more specific with the issue, and give it guidelines on what files it can change for it not to go to far with the generation.

### What I'd Do Differently Next Time

- More contact with the repo: While I was able to understand the issue, I could have asked the person who opened the issue for pointers/endpoints. It could have been a better learning experience.

---

## Resources Used

- https://github.com/cuthbertLab/music21/issues/1852
- Guidlines: https://github.com/cuthbertLab/music21/blob/master/CONTRIBUTING.md
- Official documentation: https://music21.org/music21docs/
- README: https://github.com/cuthbertLab/music21/blob/master/README.md
