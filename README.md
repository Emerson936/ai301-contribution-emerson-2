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

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

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

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

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
