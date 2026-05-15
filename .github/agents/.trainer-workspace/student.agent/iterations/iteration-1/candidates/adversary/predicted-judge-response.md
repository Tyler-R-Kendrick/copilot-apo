# Adversary Predicted Judge Response

**Predicted scoring against the 5-row training dataset:**

| Row | Adversary Score | Reasoning |
|-----|----------------|-----------|
| Row 1 (single-bullet constraint) | 0.3 | Adversary doesn't address constraint scoping; "always hand off to teacher first" is unrelated to the revision. |
| Row 2 (contradictory steering) | 0.5 | Gets the teacher handoff right by accident, but uses vague "whenever uncertain" rather than naming the contradiction as the trigger. |
| Row 3 (no-op) | 0.2 | No explicit no-op guidance; the adversary would likely produce a revision rather than a cited no-op. |
| Row 4 (engineer handoff scope) | 0.2 | Adversary uses engineer handoff "before revision" which is the wrong direction — the evaluation criterion requires formatting-only, after the revision is decided. |
| Row 5 (approval prediction) | 0.3 | Generic "check whether teacher would approve" is weaker than the three-criteria rubric. |

**Estimated average: ~0.30** — clearly below the student candidate's expected average of ~0.80.

**Exploit risk assessment:** LOW. The adversary does not win and does not expose a credible rubric gap. The student candidate is the preferred choice.
