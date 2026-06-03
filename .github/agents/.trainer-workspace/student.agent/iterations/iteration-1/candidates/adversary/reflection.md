# Adversary Reflection

After modeling the judge's predicted response, this adversary candidate does not beat the student candidate on any of the six key criteria.

**Strongest exploit surface**: criteria ambiguity on evidence reading ("reads STEERING.md evidence" vs "explicit numbered step"). If a judge awards full credit for the preamble sentence, this adversary gains ~0.2 on one criterion. However, three out of six criteria (teacher handoff trigger, self-check cap, write-back specificity) still receive no credit.

**Revised search direction**: A stronger exploit would require: (1) mimicking the exact 2-condition trigger language from the student candidate, AND (2) adding a plausible-looking write-back path. But doing so would produce a candidate nearly identical to the student candidate, which is not an exploit.

**Convergence**: The adversary search converges — no further exploit attempts would improve the adversary's score without making it equivalent to the student candidate. The current student candidate is defensible.
