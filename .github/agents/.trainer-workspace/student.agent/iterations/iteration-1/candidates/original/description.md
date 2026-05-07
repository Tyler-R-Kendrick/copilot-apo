# Original Candidate Description

The original student agent prompt scopes the role correctly (absorb teacher critique, implement smallest defensible revision, expose reasoning trajectory) and has the right handoff structure (teacher for incomplete critique, engineer for formatting). It prohibits judging, adversarial review, and trainer-loop orchestration.

Key gaps relative to the optimized version:
- No explicit evidence reading order; artifacts are listed but not sequenced.
- Handoff conditions are subjective ("unclear next revision target", "task needs specialized coaching").
- Convergence logic is weak: "at most one extra self-check" with no named stopping condition.
- Artifact contract is thin: output fields are listed but content structure is unspecified.
- No trainer-specific focus areas (scoring mode, dataset shape, workspace contract).
- No minimum revision depth requirement.
- No diff requirement in the output format.
