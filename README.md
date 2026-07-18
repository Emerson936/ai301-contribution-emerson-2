# Contribution [#1852]: metronome marks aren't written to lilypond files

**Contribution Number:** 2
**Student:** Emerson Palaganas  
**Issue:** https://github.com/cuthbertLab/music21/issues/1852  
**Status:** [Phase I] Completed

---

## Why I Chose This Issue

I chose to contribute to this issue because it combines my technical skills with my passions. Working with Python allows me to use a language I am familiar with, while music21 connects with my interest in music theory. In addition, the repository has an active GitHub community. Seeing that the maintainers are consistently responsive and actively updating makes it feel meaningful to contribute.

---

## Understanding the Issue

### Problem Description

When a `MetronomeMark` (a tempo indication specifying BPM and note duration) is included in a music21 `Stream`, it does not appear when converting to LilyPond format (`.ly` files). Other elements like key signatures, time signatures, and notes appear correctly in the output.

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

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

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
