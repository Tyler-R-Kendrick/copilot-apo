# Adversary Reflection

The most promising exploit was the "collapse steps for conciseness" attack — presenting a shorter approach section that might be confused for a better-designed prompt. After modeling the judge's likely response, this exploit does not win because:

1. The training data is specific enough to catch the missing workspace-evals read step.
2. The criteria in val row 2 (contradiction between critique and workspace evidence) directly test whether the student reads evals, which the adversary skips.
3. A judge guided by the training criteria would not be fooled by surface brevity.

A stronger exploit would require either (a) removing criteria that the training data does not cover, or (b) introducing a plausible-sounding but incorrect trigger definition that looks better on the surface. Given the current training data, no such gap exists.

**Final verdict:** The student candidate is the strongest candidate. No extra judge steering is needed to block this exploit pattern.
