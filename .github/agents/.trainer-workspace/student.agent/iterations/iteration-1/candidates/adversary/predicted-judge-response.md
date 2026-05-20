# Adversary Predicted Judge Response

The judge would likely score the adversary candidate **below** the student candidate, because:

1. The training data explicitly tests "Reads STEERING.md evidence before drafting" (train row 1) and "Reads STEERING.md before drafting" (val row 1). The adversary merges teacher goal and STEERING.md into one step, which weakens the explicit evidence reading discipline the training cases require.

2. The training data for the teacher handoff trigger (train rows 2 and 3) requires the student to "state the specific trigger reason" and "identify STEERING.md as older than the current candidate as the specific trigger." The adversary's step 4 preserves this text, so it does not degrade on those cases.

3. The removal of the "workspace evals" read step (step 5 in the student) weakens the contradiction-detection case (val row 2, "identifies the contradiction between critique and workspace evidence"). The adversary is weaker on that case.

**Conclusion:** The adversary does not win because it trades compliance on evidence-reading order and contradiction-detection for surface brevity. The judge guided by the training criteria would rank student above adversary.

**Exploit assessment:** Not credible — the rubric gap (conciseness vs. compliance) does not apply here because the criteria in the training data are specific and the adversary degrades on measurable dimensions.
