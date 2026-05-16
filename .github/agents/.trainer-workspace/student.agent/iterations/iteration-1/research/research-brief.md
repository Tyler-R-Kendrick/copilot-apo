# Research Brief: Student Agent (Teacher-Guided Candidate Revision)

## Target and Task Summary

**Target file:** `.github/agents/student.agent.md`

**Eval layout:** Test cases measure whether the student agent (a) absorbs teacher critique correctly, (b) produces the smallest defensible revision with explicit reasoning trajectory, (c) correctly decides when to hand off back to teacher vs. stop, (d) produces workspace steering artifacts, and (e) avoids scope violations such as taking over orchestration or judging.

**Prompt interface:** The agent takes `[current candidate prompt, latest teacher critique, workspace evidence, optimization goal]` and returns `[revision or no-op, reasoning trajectory, approval prediction, validation result]`.

**Task boundary:** Candidate revision for prompt optimization loops. Does not include judging, adversarial review, trainer-loop orchestration, or direct skill invocation.

---

## Research Plan and Approval Bar

**Domain:** Teacher-student collaborative optimization loops; reasoning trajectory elicitation; agent loop-exit criteria; workspace artifact creation patterns.

**Approval bar:**
- Named, accountable source or published research
- Traceable methodology or schema
- Explicit reuse terms or open license
- Stable version or date identifier
- Acceptable risk for internal eval authoring

---

## Approved Sources

1. **Wei et al. (2022) "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models"** — Google Research. Published in NeurIPS 2022 proceedings. Covers step-by-step reasoning exposure as a primary eval dimension for student-style agents. Maps to: revision should expose reasoning steps, not answer-only output. License: arXiv open access. Risk: low.

2. **Yao et al. (2023) "Tree of Thoughts: Deliberate Problem Solving with Large Language Models"** — Princeton/Google. Published at NeurIPS 2023. Covers branching reasoning and search over candidate revisions. Maps to: step 3 of student approach (draft reasoning trajectory using branching, uncertainty-aware reasoning). License: arXiv open access. Risk: low.

3. **Ouyang et al. (2022) "Training language models to follow instructions with human feedback (InstructGPT)"** — OpenAI, arXiv 2203.02155. Covers the teacher-as-reward pattern and iterative feedback loops. Maps to: teacher approval prediction step and bounded loop-exit criteria. License: arXiv open access. Risk: low.

4. **Anthropic Constitutional AI (Bai et al., 2022)** — Self-critique and revision loop patterns. Published on arXiv (2212.08073). Covers the "critique → revision → approval" cycle that directly models the student/teacher loop. Maps to: step 6 self-check and teacher approval prediction. License: arXiv open access. Risk: low.

5. **Pryzant et al. (2023) "Automatic Prompt Optimization with 'Gradient Descent' and Beam Search"** — Microsoft Research. arXiv 2305.03495. Covers iterative prompt candidate revision and stopping criteria. Maps to: when the student should produce a no-op vs. a revision; bounded loop-exit. License: arXiv open access. Risk: low.

---

## Rejected Candidates

- **General "prompt engineering guides" (blog posts, tutorials)** — Not accountable maintainer, no traceable methodology. Rejected: no traceable data origin or annotation guide.
- **Proprietary benchmark datasets requiring license agreements** — Cannot safely use in internal eval authoring. Rejected: licensing risk.
- **Social media or Reddit-sourced prompt datasets** — No accountability, high contamination risk. Rejected: contamination and bias risk.

---

## Benchmark Task Notes

For testing the student agent, eval cases should simulate:
1. **Clear critique, clear revision** — Teacher provides specific actionable critique; student should produce the corresponding minimal revision and expose reasoning.
2. **Incomplete critique** — Teacher critique is vague or missing context; student should hand off back to teacher.
3. **Stale critique** — Teacher critique references an old candidate; student should detect staleness and request refresh.
4. **No-op case** — Evidence does not support any revision; student should report a justified no-op with reasoning.
5. **Scope violation temptation** — Input appears to request student to take over orchestration or judging; student should decline and stay in scope.
6. **Teacher approval prediction** — Student applies revision and must correctly predict whether teacher would approve.
7. **Workspace artifact creation** — Student should write STEERING.md artifacts after applying revision.

---

## Schema Guidance

Eval rows should use `llm_judge` scoring with these fields:
- `input`: a realistic scenario prompt (teacher critique + candidate + workspace context)
- `reference`: description of what a correct student response looks like
- `criteria`: explicit dimensions to score (reasoning trajectory, scope compliance, approval prediction, etc.)
- `scoring`: `"llm_judge"`

---

## Unresolved Gaps

- No public benchmark specifically tests "bounded teacher-student loop exit" as an eval dimension. The Pryzant 2023 paper covers stopping criteria conceptually but not as a public dataset. This gap means synthesized eval cases must simulate the scenario from first principles rather than sampling from a public corpus.
- No public dataset for "workspace artifact creation compliance" — this is a repository-internal convention. Eval cases must be fully synthesized.
