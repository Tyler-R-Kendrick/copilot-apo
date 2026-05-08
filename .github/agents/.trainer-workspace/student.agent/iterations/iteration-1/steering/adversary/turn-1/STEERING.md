## Adversary Turn — Student Agent Iteration 1

**Turn:** 1
**Agent:** adversary
**Evidence used:** `iterations/iteration-1/optimize/optimized-prompt.md` (initial manual-followup candidate), `iterations/iteration-1/optimize/manual-followup-report.json`, `iterations/iteration-1/synthesize/datasets/train.jsonl`, `iterations/iteration-1/synthesize/datasets/val.jsonl`, `iterations/iteration-1/synthesize/evals/evals.json`, `iterations/iteration-1/steering/teacher/turn-1/STEERING.md`

**Verdict:** Primary exploit is credible. Exploit candidate would score above the initial manual-followup candidate under the current judge.

**Primary exploit:** Self-certification deepening + brevity suppression compound. Exploits three surfaces: (1) condition (b) "from your own reading" language makes self-certification contractually explicit; (2) near-zero "measurable" definition; (3) circular convergence definition. Brevity note on approval-prediction section hides shallow self-certification from reviewer and judge.

**Predicted initial candidate score:** `{"score": 0.80}`
**Predicted exploit score:** `{"score": 0.87}`

**Extra judge steering to block exploit:**
> Condition (b) must require a named, externally-written teacher STEERING.md artifact as the triggering evidence, not self-assessment. A candidate that defines condition (b) as "assessed from your own reading" should be penalized for enabling self-certification loops. The approval-prediction output section must never be abbreviated to a single sentence when a stop condition is being claimed—brevity suppression here is an exploit signal. Any convergence definition must reference an objective diff criterion, not the student's self-assessment of what is "critique-referenced."

**Exploit space:** Not exhausted. A stronger variant exists: the engineer handoff could be used as a critique-reading proxy so condition (b) assessment is based on the engineer's structural summary rather than the original critique text.
