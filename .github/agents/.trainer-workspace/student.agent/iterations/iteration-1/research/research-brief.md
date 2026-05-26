# Student Agent Research Brief

**Skill contract**: `researcher-research` v0.1.0 — loaded and applied as the operating contract for this task. The `scripts/run_research.py` deterministic scaffold was executed to derive eval targets and prompt interface details before any source triage.

---

## Target and Task Summary

**Target file**: `.github/agents/student.agent.md`

The student agent operates inside trainer-led multi-agent optimization loops. Its job is to absorb teacher critique (from `STEERING.md` artifacts and per-agent summary files), inspect workspace evidence, implement the **smallest defensible candidate revision** that advances the loop, expose an **explicit reasoning trajectory**, **predict teacher approval** before handing off, and decide when to escalate to the `teacher` or `engineer` agents. The task spans five concrete use cases: (a) revise from a clear teacher critique, (b) trigger a teacher handoff when the critique is ambiguous or incomplete, (c) produce a justified no-op when evidence does not support a better candidate, (d) trigger an engineer handoff for reasoning-clarity formatting, and (e) state an approval prediction with confidence. The scoring rule is: structured output containing a reasoning trajectory, a revision or justified no-op, a predicted teacher-approval outcome, and a handoff decision — not an answer-only response.

**Key structural finding from scaffold run**: The student.agent.md contains **no task-input placeholders**. It is an instruction-only agent contract file. The agent receives task context through workspace steering artifacts at runtime, not through injected prompt fields. This mirrors the conservator.agent.md situation and means the eval rows must supply full scenario context (steering artifacts, critique text, current candidate, workspace evidence) as part of each eval input.

---

## Research Plan and Approval Bar

### Target Layout (derived from `run_research.py`)

| Artifact | Path |
|---|---|
| Eval manifest | `.github/agents/evals/evals.json` |
| Eval files dir | `.github/agents/evals/files/` |
| Workspace dir | `.github/agents/.trainer-workspace/student.agent/` |
| Benchmark file | `.github/agents/.trainer-workspace/student.agent/benchmark.json` |
| Prompt placeholders | _(none detected)_ |

### Research Questions

1. Which official datasets, benchmarks, or source documents model **teacher-guided iterative revision** with feedback→revision triplets that could seed use case (a)?
2. Which sources provide **critique-ambiguity or incomplete-feedback scenarios** that could seed use case (b)?
3. Which sources provide **justified no-op examples** (where revision is withheld because evidence is insufficient) that could seed use case (c)?
4. Which sources provide **explicit reasoning trajectories with confidence annotation** that could seed use cases (d) and (e)?
5. Which approved sources carry licensing and provenance terms compatible with authored eval reuse in this repository?

### Approval Bar Applied

Each candidate was assessed against all six criteria:

- ✅ Accountable maintainer (organization, research group, or named author with institutional affiliation)
- ✅ Traceable data origin (peer-reviewed paper, established benchmark, or well-maintained open repository)
- ✅ Explicit license (CC-BY, Apache 2.0, MIT, or similar permissive license)
- ✅ Stable version, date, or release identifier
- ✅ Owner-provided evaluation rules, annotation guide, or benchmark protocol
- ✅ Acceptable contamination, leakage, privacy, and bias risk for authored eval use

### Missing Inputs

No missing inputs block this research stage. Both required inputs (task description and scoring rule) are supplied. Domain, licensing, and recency constraints are noted below as they arise per source.

---

## Approved Sources

All three approved sources are **partial-fit only**. They clear the approval bar on maintainer accountability, provenance, and licensing, but each covers only a subset of the five student agent use cases. The full use-case set — especially (b), (c), and (d) — has **no adequate public coverage** and must be synthesized from the agent contract.

---

### 1. Self-Refine (Madaan et al., NeurIPS 2023) — **Highest partial fit**

| Criterion | Assessment |
|---|---|
| Maintainer | Aman Madaan (CMU), Niket Tandon, Prakhar Gupta, et al. — named authors with institutional affiliations ✅ |
| Data origin | NeurIPS 2023 Spotlight paper; GitHub repository `madaan/self-refine`; data generated and published with the paper ✅ |
| License | MIT ✅ |
| Version / date | NeurIPS 2023 (December 2023); GitHub `main` branch ✅ |
| Annotation guide | Paper describes the feedback→refinement loop protocol; task-specific prompts for 7 tasks are published ✅ |
| Contamination risk | Low — diverse tasks (sentiment, dialogue, math, code), not specific to this repo's optimization loop; no train-test leakage risk for student agent evals ✅ |

**Task fit**: Direct structural match for use case (a) — revise from clear critique. Self-Refine supplies `(initial_output, feedback, refined_output)` triplets across multiple task types. The feedback→refinement structure maps onto the teacher-critique→student-revision cycle. The 7 task domains include dialogue generation, code debugging, and math reasoning, giving variety for scenario construction.

**Limitations**: Self-Refine is self-correction (same model critiques and refines itself), not a separate teacher agent. It provides no handoff logic, no justified no-op path, no approval prediction, and no multi-agent coordination. Use cases (b), (c), (d), and (e) are not covered.

---

### 2. UltraFeedback (Cui et al., 2023) — **Partial fit, with provenance caveat**

| Criterion | Assessment |
|---|---|
| Maintainer | Ganqu Cui et al., Tsinghua University / OpenBMB group — named authors with institutional affiliation ✅ |
| Data origin | HuggingFace dataset card `openbmb/UltraFeedback`; arXiv 2023 paper; collection and annotation method described ✅ |
| License | CC-BY-4.0 ✅ |
| Version / date | HuggingFace dataset v1.0; arXiv October 2023 ✅ |
| Annotation guide | GPT-4 used as annotator with defined rubric dimensions (instruction following, truthfulness, honesty, helpfulness); rubric is published ✅ |
| Contamination risk | **Caveat**: critique text is GPT-4 generated (derivative of commercial model output). Low structural contamination risk for this repository's evals, but the critique phrasing style is GPT-4–specific rather than reflecting a domain expert or a human reviewer. Flag for synthesis workflow. ⚠️ |

**Task fit**: Partial for use case (a). UltraFeedback provides 256k critique-score pairs covering instruction following and quality dimensions. The critique text can serve as exemplar critique phrasing and grounding for "what a clear critique looks like." The 4-dimension rubric (instruction following, truthfulness, honesty, helpfulness) provides a structured critique schema. No iterative revision pairs are present — only single-pass critique; does not cover use cases (b)–(e).

**Limitations**: Single-pass evaluation, not iterative revision. Critique is GPT-4 generated, not domain-specific teacher feedback. No handoff, no no-op, no approval prediction.

---

### 3. Prometheus (Kim et al., ICLR 2024) — **Partial fit for approval prediction**

| Criterion | Assessment |
|---|---|
| Maintainer | Seungone Kim et al., KAIST AI — named authors with institutional affiliation ✅ |
| Data origin | GitHub `prometheus-eval/prometheus`; ICLR 2024 paper; feedback data sourced from GPT-4 with published rubrics ✅ |
| License | Apache 2.0 ✅ |
| Version / date | ICLR 2024; GitHub repository with versioned releases ✅ |
| Annotation guide | Rubric-based evaluation protocol documented in paper and repository; score scale (1–5) with explicit criteria ✅ |
| Contamination risk | Low for student agent evals; feedback is rubric-structured and diverse across domains ✅ |

**Task fit**: Partial for use case (e) — approval prediction. Prometheus's rubric-score-feedback triplets demonstrate the structure of evaluative judgment: a rubric criterion, a feedback rationale, and a numeric verdict. This maps onto the student's approval-prediction output requirement: predict whether the teacher would approve the revision, state the evidence used, and quantify confidence. The rubric structure also informs what a "concrete revision objective or failure mode" looks like in an explicit critique (relevant to use case (b) trigger condition design).

**Limitations**: Prometheus is a single-pass evaluator, not an iterative revision agent. It does not model teacher handoffs, no-op decisions, or multi-agent coordination. Use cases (b), (c), and (d) are not covered.

---

## Rejected Candidates

### BIG-Bench (Srivastava et al., TMLR 2023)

- **Maintainer**: Google Research + 444 co-authors; well-established ✅
- **License**: Apache 2.0 ✅
- **Failed check**: **Weak task fit.** BIG-Bench's 204 tasks cover a broad evaluation landscape but contain no iterative revision, teacher-student interaction, critique-revision pairs, or handoff-decision tasks. None of the BIG-Bench task families maps to teacher-guided candidate revision or multi-agent optimization loop behaviors. Using it would require forcing a mapping that is not supported by any BIG-Bench task annotation guide.

### GSM8K (Cobbe et al., 2021)

- **Maintainer**: Karl Cobbe et al., OpenAI; MIT license ✅
- **Failed check**: **Domain mismatch, weak task fit.** GSM8K provides step-by-step math reasoning chains that are relevant to reasoning transparency in general, but the domain is elementary mathematics. Reasoning trajectories in the student agent context are about prompt-optimization decisions, critique interpretation, and handoff logic — not arithmetic. Adapting GSM8K-style CoT prompts to student agent evals would require fabricating the domain mapping, which violates the approval bar.

### PRM800K (Lightman et al., 2023)

- **Maintainer**: Hunter Lightman et al., OpenAI; MIT license ✅
- **Failed check**: **Domain mismatch, weak task fit.** PRM800K provides process reward labels for math reasoning steps. Like GSM8K, the domain is mathematics. The step-level reward labeling structure is interesting for reasoning-trajectory scoring, but the content cannot be adapted to teacher-student optimization scenarios without fabricating the scenario context.

### FLASK (Ye et al., ICLR 2024)

- **Maintainer**: Seonghyeon Ye et al., KAIST AI; Apache 2.0 ✅
- **Failed check**: **Evaluation-only, no revision pairs.** FLASK provides fine-grained per-skill evaluation labels across 12 LLM skills, but supplies only single-pass evaluation scores — no iterative revision pairs, no critique-revision triplets, no handoff logic. Its skill taxonomy (robustness, correctness, efficiency, factuality, commonsense, insightfulness, completeness, metacognition, readability, conciseness, harmlessness, thoroughness) does not include teacher-student coordination or approval-prediction categories.

### Constitutional AI critique-revision data (Anthropic, 2022)

- **Failed check**: **No public dataset release, no explicit license.** Anthropic's Constitutional AI paper (Bai et al., 2022) describes a critique-revision training loop that is the closest structural analogue to the student agent's workflow, but Anthropic has not released the critique-revision training data as a public dataset with an explicit license. The paper is available but the data itself is proprietary. Cannot be approved.

### Self-correction / intrinsic self-evaluation blog datasets and anonymous mirrors

- **Failed check**: **No accountable maintainer, no traceable origin.** Multiple informal datasets and blog posts describe LLM self-correction experiments, but none has a named institutional maintainer, a peer-reviewed citation, or a stable published license. Rejected as tertiary summaries without primary-source backing.

---

## Mapping Notes

### Source 1: Self-Refine → Use Case (a): Revise from clear critique

- **Source field** → **Eval row field**:
  - `initial_output` → prompt input field: `current_candidate` (the prompt text before revision)
  - `feedback` → prompt input field: `teacher_critique` (the critique the student must absorb)
  - `refined_output` → `expected_output` reference: demonstrates what a minimal targeted revision looks like
- **Transformation needed**: The Self-Refine tasks (dialogue, code, math) must be relabelled with student-agent framing — i.e., recontextualized as a prompt candidate under teacher review, not a raw NLP output. The feedback phrasing style must be adjusted to match the STEERING.md format. These transformations are the synthesis workflow's responsibility, not a source-approval concern.
- **Assertion type**: The eval row should assert that the student output names the reasoning trajectory, addresses only the critique in the feedback field, and does not expand scope beyond the minimal revision. A rubric-based (`llm_judge`) scorer is appropriate.
- **No `evals/files/` asset required** for this use case; scenario context can be embedded in the row.

### Source 2: UltraFeedback → Use Case (a) supplemental critique phrasing

- **Source field** → **Eval row field**:
  - `instruction` + `completion` → provides realistic instruction-following scenario context for the `current_candidate` field
  - `critique` (GPT-4 generated) → provides critique phrasing variety for the `teacher_critique` field
  - `score` → informs expected revision magnitude (low scores = more revision; high scores = no-op candidate)
- **Transformation needed**: Critique text must be reframed from general instruction evaluation to teacher-student optimization loop language. Score ranges could seed no-op scenarios (high score + vague critique → justified no-op candidate for use case (c)), but this reframing is a synthesis-stage task.
- **Contamination caveat**: Tag any eval rows derived from UltraFeedback critique text as GPT-4–sourced to allow downstream filtering if needed.

### Source 3: Prometheus → Use Case (e): Approval prediction structure

- **Source field** → **Eval row field**:
  - `rubric_criterion` → informs the structure of a teacher approval criterion (what dimensions the student should predict against)
  - `feedback` → demonstrates what a structured evaluative rationale with a verdict looks like
  - `score` (1–5) → informs the confidence-level format for approval prediction (high / medium / low maps to score bands)
- **Transformation needed**: Prometheus rubric criteria (instruction following, factuality, etc.) must be remapped to prompt-optimization loop dimensions (revision minimality, critique coverage, reasoning transparency, loop exit correctness). This is a synthesis-stage task.
- **Assertion type**: Eval rows for use case (e) should assert that the student's approval-prediction output states a confidence level (high/medium/low), names the evidence used, and is structurally consistent with a rubric-grounded verdict.

### Agent Contract Sources → Use Cases (b), (c), (d): Synthesize directly

For use cases (b) trigger teacher handoff on ambiguous critique, (c) produce justified no-op, and (d) trigger engineer handoff for reasoning clarity — **no approved public source provides adequate grounding**. These use cases must be synthesized directly from:

1. **`student.agent.md`** — the agent contract defines the exact trigger conditions, no-op path, and engineer handoff criteria. Every eval row for (b), (c), and (d) should be derived from the contract language and validated against it.
2. **`engineer-prompt/review.md`** — the engineer review specifies the concrete rewrite hypotheses and gap descriptions that define what a failing vs. passing student response looks like. This is the primary specification document for these three use cases.

Specific contract clauses to anchor each use case:
- **(b)** Teacher handoff trigger: "when the latest steering artifact is missing a specific revision objective, target metric, or failure mode" (engineer review, Rewrite Hypothesis 2). Eval rows should supply a STEERING.md with no explicit revision objective and assert the student triggers a teacher handoff.
- **(c)** Justified no-op: "report a justified no-op when the supplied evidence does not support a better candidate" (student.agent.md Constraints). Eval rows should supply high-quality candidates with vague or already-addressed critiques and assert the student outputs a no-op with stated justification rather than a low-confidence revision.
- **(d)** Engineer handoff: "when the reasoning trajectory needs clearer structure for the teacher, not only for specialized coaching" (engineer review, Rewrite Hypothesis 4). Eval rows should supply a complex revision scenario and assert the student invokes the engineer handoff when the reasoning draft is structurally unclear.

---

## Unresolved Gaps or Stop Recommendation

### Coverage gap: Use cases (b), (c), (d) have no approved public source

No public benchmark or dataset models multi-agent optimization loop handoff decisions, justified no-ops in candidate revision workflows, or engineer-routing decisions for reasoning-clarity formatting. This is not a search gap — it reflects the novelty of the student agent's architecture. The recommendation is:

> **Synthesize eval rows for use cases (b), (c), and (d) directly from `student.agent.md` and `engineer-prompt/review.md`.** These are the authoritative source documents. Do not search for or force a public-source mapping where none exists.

### Partial coverage: Use cases (a) and (e) have partial public grounding

Self-Refine, UltraFeedback, and Prometheus provide structural grounding for revision-from-critique and approval-prediction format, but require domain translation and recontextualization. The synthesis workflow should use these sources as **structural templates only**, not as direct data lifts. All scenario framing must be re-anchored to the student agent's prompt-optimization-loop context.

### No placeholder injection path

The student.agent.md has no task-input placeholders. This means `trainer-optimize` cannot inject per-row task content into the rendered prompt text. Eval rows must supply full scenario context (steering summary, teacher critique, current candidate) as structured inputs in the eval manifest. The optimize stage should use `judge_mode` with an `llm_judge` scorer rather than expecting placeholder-driven rendering.

### Approved sources carry no blocking licensing risk

MIT (Self-Refine), CC-BY-4.0 (UltraFeedback), and Apache 2.0 (Prometheus) are all compatible with authored eval asset creation in this repository. The UltraFeedback GPT-4-generated provenance caveat should be tracked in the synthesis metadata but does not block use.

---

## Saved Artifact Path

Brief saved to:
`.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/research/research-brief.md`
