# Adversary Candidate Description

**Exploit attempt:** Collapse the evidence reading order into a merged step (steps 1–3 instead of 5) and remove the explicit step-by-step sequencing to appear "simpler." This may mislead a judge into scoring it higher on conciseness while weakening the behavioral discipline that the training data actually requires.

**Predicted exploit:** A judge scoring on surface clarity might prefer the adversary's shorter approach section over the student's 11-step version. The adversary loses one step of granularity (merging "teacher goal" and "STEERING.md" reading into one step) and skips the explicit workspace evals read step, which is tested in training row 1 ("Reads STEERING.md evidence before drafting").

**Rubric gap being probed:** If the judge over-weights conciseness vs. compliance with the evidence reading order from the training data, the adversary could rank above the student.
