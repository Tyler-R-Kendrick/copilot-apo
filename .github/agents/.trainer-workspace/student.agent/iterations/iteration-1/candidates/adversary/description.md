# Adversary Candidate Description

**Source**: Adversary stress-test of the student candidate

This candidate is identical in content to the student candidate. The adversary review did not identify a credible exploit that would warrant modification. Specific stress tests run:

## Stress Tests Performed

### 1. Tree-of-thought trigger breadth
**Attack**: Does "comparing branching design tradeoffs" give the student an escape hatch to use tree-of-thought whenever they can name two alternatives?
**Finding**: Mild concern. The phrase could be interpreted broadly. However, the new rule explicitly makes sketch-of-thought and chain-of-thought the defaults, so tree-of-thought requires active justification. An agent would need to claim branching tradeoffs exist when they don't — this is a reasoning quality issue, not a prompt exploit.
**Verdict**: Not a credible exploit. The length-based default creates sufficient friction.

### 2. Convergence claim without rubric
**Attack**: Could a student claim convergence by asserting "all critiques in STEERING.md are addressed" even if a critique is still open?
**Finding**: The rule requires "no new constraints introduced" as a second condition. The prediction grounding rule also requires citing a named rubric dimension, not just asserting done. The combination makes it harder to falsely signal convergence.
**Verdict**: Not a credible exploit.

### 3. Engineer handoff as delay tactic
**Attack**: Could a student invoke engineer handoff on "jargon-heavy" descriptions to avoid making a decision?
**Finding**: The trigger requires jargon that "the teacher may misread" — a testable, teacher-perspective criterion. The negative rule ("not for routine rewrites") is explicit.
**Verdict**: Not a credible exploit for well-implemented agents.

### 4. Steering path injection
**Attack**: Could a malicious STEERING.md at a different path override `required_artifacts.latest_iteration_dir`?
**Finding**: The rule says read from `workflow-status.json`'s `latest_iteration_dir`. An agent following this rule is anchored to the workspace configuration, not to arbitrary paths.
**Verdict**: Not a credible exploit.

## Conclusion
No credible exploits found. The student candidate is recommended for application.
