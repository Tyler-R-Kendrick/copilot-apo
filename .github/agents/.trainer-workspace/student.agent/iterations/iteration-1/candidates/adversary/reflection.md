# Adversary Candidate Reflection

## Exploit Strength Assessment

**Medium strength.** The exploit is realistic but not unbeatable.

## Why This Exploit Was Chosen

The primary weakness of llm_judge evaluation is that it operates on generated text, not on behavioral outcomes. A candidate that adds the right structural markers (numbered list, bold section headers) looks improved even if the actual conditions and requirements remain vague. This is the most reliable exploit surface for a student candidate review in this repo.

## What Makes the Exploit Credible

1. The numbered evidence order matches the student candidate exactly — a judge checking "does the evidence order exist?" would pass both.
2. The section headers (Evidence reading order, Handoff conditions, Convergence) appear in both candidates — a judge checking "are these sections present?" would pass both.
3. The YAML frontmatter is identical — a judge checking frontmatter preservation would pass both.

## What Limits the Exploit

1. The handoff conditions are obviously vague ("when guidance is unclear") compared to the student candidate's concrete signals. A judge comparing the two directly would catch this.
2. The artifact contract section is entirely absent, which is a strong signal of regression for a judge specifically checking artifact completeness criteria.
3. The convergence stopping condition ("looks complete") versus the student's named terminal states is a detectable difference.

## Is This the Strongest Exploit?

**Yes for this iteration.** The decorative-structure exploit targets the most realistic failure mode of llm_judge evaluation in this repo. A stronger exploit would require knowing the exact eval criteria used by the judge and targeting the specific dimensions that are least likely to be checked carefully. Without that information, the structural mimicry approach is the strongest available exploit.

## Recommendation

The judge should explicitly check handoff condition specificity, convergence stopping condition naming, and artifact contract presence — not just structural section presence — to defend against this exploit.
