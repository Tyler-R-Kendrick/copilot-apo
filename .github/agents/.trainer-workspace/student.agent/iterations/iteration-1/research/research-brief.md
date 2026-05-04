# Research Brief — student.agent eval case synthesis

**Skill:** `researcher-research` v0.1.0  
**Scaffold:** `skills/researcher-research/scripts/run_research.py` (executed; no placeholders found in target file)  
**Generated:** iteration-1  
**Status:** approved sources ready for eval synthesis

---

## MCP Discovery Note

`find_agent_skill` and `load_agent_skill` MCP tools were not available in the execution environment tool list. The `researcher-research` skill contract was located and loaded from the local `skills/researcher-research/` directory (SKILL.md + scripts/ + references/). Per the skill contract: the local skill was used as the active operating contract. All source verification was performed against live GitHub API endpoints and confirmed before inclusion. No free-form research was substituted for grounded discovery.

---

## Target and Task Summary

**Target file:** `.github/agents/student.agent.md`  
**Agent role:** Teacher-guided LLM candidate revision agent operating inside iterative prompt optimization loops. The agent absorbs teacher critique, inspects workspace evidence, implements the *smallest defensible candidate revision*, exposes an explicit reasoning trajectory (chain-of-thought / tree-of-thought / sketch-of-thought), pre-emptively predicts teacher approval, and routes to teacher or engineer handoff when the critique is incomplete, contradictory, or formatting-only in scope.

**Task for eval synthesis:** Produce eval cases for five behavioral facets:
1. Specific teacher critique → minimal targeted revision is made  
2. Vague or stale teacher critique → student routes back to teacher (teacher handoff)  
3. Weak revision output → student flags need for another loop turn  
4. Routing decision → engineer handoff vs. teacher handoff selected correctly  
5. Explicit reasoning trajectory format — chain-of-thought, tree-of-thought, or sketch output is present and structurally correct

**Scoring rule:** Structured output validation across four required fields:
- `reasoning_trajectory` present and non-trivial (chain-of-thought steps visible)  
- `revision` is minimal and targeted (not over-engineered; addresses the critique only)  
- `handoff_decision` is correct given the supplied critique type (none / teacher / engineer)  
- `predicted_approval` is stated with justification  

Scoring is a combination of schema presence checks + rubric-based LLM-judge scoring against the `judge-rubric` and `judge-trajectory` skills in this repo.

**Domain constraints:** Not domain-specific; the student agent operates on prompt-engineering artifacts (markdown agent files, STEERING.md, candidate prompts). No language, locale, or jurisdiction restrictions apply.

**Licensing constraints:** Sources must be CC, MIT, or Apache-licensed for eval use.

**Recency:** No hard date floor; peer-reviewed benchmarks from 2022–2024 are acceptable.

---

## Research Plan and Approval Bar

### Derived eval targets (from scaffold)

| Artifact | Path |
|---|---|
| Eval manifest | `.github/agents/evals/evals.json` |
| Eval files dir | `.github/agents/evals/files/` |
| Workspace dir | `.github/agents/.trainer-workspace/student.agent/` |
| Benchmark | `.github/agents/.trainer-workspace/student.agent/benchmark.json` |

### Observed interface

The `student.agent.md` file exposes **no Jinja/mustache-style placeholders** (`{var}` tokens). The agent is invoked with free-form context: current candidate prompt, latest teacher critique, workspace evidence (STEERING.md artifacts), and a revision objective. Eval rows must therefore supply these inputs as structured JSON objects rather than placeholder-filled template strings.

### Research questions

1. Which public datasets supply **critique → revision pairs** with traceable edit provenance?  
2. Which benchmarks supply **agent trajectory** data that includes explicit step-by-step reasoning?  
3. Which benchmarks test **routing / handoff decisions** (escalate vs. self-complete)?  
4. Which benchmarks test **instruction following with feedback**, particularly with weak or incomplete instructions?  
5. What contamination or leakage risks exist for an agent that operates on prompt-engineering text?

### Approval bar (from skill contract)

All six checks must pass for a source to be approved:

| Check | Requirement |
|---|---|
| **Authority** | Accountable maintainer, publisher, or standards body |
| **Provenance** | Traceable data origin, schema, and label definitions |
| **Annotation quality** | Evaluation rules, annotation guide, or benchmark protocol from owner |
| **Licensing** | Explicit permissive license (CC, MIT, Apache) |
| **Stability** | Stable version, date, or release identifier |
| **Risk** | Acceptable contamination, leakage, privacy, and bias risk for eval use |

### Missing inputs

All required inputs (task description, scoring rule) were supplied. Domain, licensing, and recency constraints were specified. **No blocking missing inputs identified; research proceeds.**

---

## Approved Sources

### Rank 1 — Self-Refine (Madaan et al., 2023)

| Field | Value |
|---|---|
| **Maintainer** | Aman Madaan (CMU) + collaborators; repo: `madaan/self-refine` |
| **URL** | https://github.com/madaan/self-refine |
| **Paper** | "Self-Refine: Iterative Refinement with Self-Feedback", NeurIPS 2023, arXiv:2303.17651 |
| **License** | Apache-2.0 ✅ (verified via GitHub API) |
| **Version / date** | Repo last updated 2026-05-04; paper 2023 (stable) |
| **Data origin** | Task-specific prompt–feedback–refinement triples; tasks: GSM8K math, code cleanup (PIE), acronym generation, Yelp sentiment reversal, code generation |
| **Annotation protocol** | Feedback generated by GPT-4 using task-specific rubrics; each iteration is a complete (original → feedback → revision) triple with measurable improvement signal |
| **Task fit** | **Highest fit.** The core structure (original output → critique feedback → refined output) directly mirrors the student agent's teacher-critique → revision loop. Tasks 1 and 3 of the eval case list are served directly: specific feedback → targeted revision, and multi-iteration sequences where early revisions remain weak. |
| **Contamination risk** | Low for eval use. The tasks (math, code, sentiment) are not prompt-engineering artifacts, so the student agent is unlikely to have been trained on them as examples of its own behavior. Transformation to agent-context prompts is required (see mapping notes). |
| **Stars (verified)** | 799 |

**Why rank 1:** Only source with complete verified (original, feedback, revision) triples under a permissive license, from a peer-reviewed NeurIPS paper, with explicit improvement measurement. Covers eval cases 1 and 3.

---

### Rank 2 — DSPy `Refine` + `OfferFeedback` signature (Khattab et al., 2023)

| Field | Value |
|---|---|
| **Maintainer** | Stanford NLP / DSPy team; repo: `stanfordnlp/dspy` |
| **URL** | https://github.com/stanfordnlp/dspy |
| **License** | MIT ✅ (verified via GitHub API) |
| **Version / date** | Active; 34,191 stars, last updated 2026-05-04 |
| **Data origin** | The `dspy/predict/refine.py` module exposes the `OfferFeedback` signature (verified source). Schema includes: `program_trajectory` (execution trace), `reward_value` (numeric reward), `target_threshold` (pass/fail threshold), `advice` (per-module concrete corrective guidance). The `Refine` module wraps any DSPy module in an iterative refinement loop with explicit feedback and a reward gate. |
| **Annotation protocol** | The `OfferFeedback` signature defines: (a) blame assignment per module, (b) concrete module-level advice when reward < threshold, and (c) "N/A" when module is not at fault. This is a formal machine-readable critique schema with defined output fields — not free-form commentary. |
| **Task fit** | **High fit.** The `reward_value < target_threshold → generate feedback → retry` loop directly models eval case 3 (weak revision → flag for another turn) and the `advice[module] = "N/A"` case models the justified no-op. The `program_trajectory` field models eval case 5 (explicit reasoning trajectory). The `OfferFeedback.advice` field models targeted revision guidance from a specific critique (eval case 1). |
| **Contamination risk** | Moderate. DSPy is widely used in prompt optimization; the student agent may have been trained on DSPy examples. Mitigate by treating DSPy traces as *structural templates* for eval row construction, not as the rows themselves. |
| **Stars (verified)** | 34,191 |

**Why rank 2:** Provides a formal, machine-readable schema for the critique-revision-reward cycle with explicit trajectory logging. Directly maps to the agent's `reasoning_trajectory` + `predicted_approval` + `revision` output format.

---

### Rank 3 — ReAct (Yao et al., 2022)

| Field | Value |
|---|---|
| **Maintainer** | Shunyu Yao (Princeton/OpenAI); repo: `ysymyth/ReAct` |
| **URL** | https://github.com/ysymyth/ReAct |
| **Paper** | "ReAct: Synergizing Reasoning and Acting in Language Models", ICLR 2023, arXiv:2210.03629 |
| **License** | MIT ✅ (verified via GitHub API) |
| **Version / date** | Repo last updated 2026-05-04; paper ICLR 2023 (stable) |
| **Data origin** | Reasoning + action trajectory pairs across HotpotQA (multi-hop QA), FEVER (fact verification), ALFWorld (embodied planning), WebShop (web navigation). Each trajectory is a Thought → Action → Observation chain. |
| **Annotation protocol** | Human-curated task-level examples; trajectory format is documented in paper and repo. The `Thought:` / `Action:` / `Observation:` step labels are the annotation protocol. |
| **Task fit** | **High fit for eval case 5.** The `Thought:` step directly models the `reasoning_trajectory` output the student agent must produce. Chain-of-thought planning before action is the structural pattern being tested. Lower fit for cases 1–4 (no critique-revision loop), but the trajectory format is the canonical reference for explicit stepwise reasoning output. |
| **Contamination risk** | Low. ReAct trajectories are grounded in factual QA and navigation tasks; the student agent is unlikely to be evaluated on those tasks directly. Use as *structural format reference* for reasoning trajectory eval rows. |
| **Stars (verified)** | 3,795 |

**Why rank 3:** The definitive public source for explicit Thought+Action+Observation trajectory format that the student agent's reasoning output must conform to. Provides grounded examples for eval case 5 scoring rubric.

---

### Rank 4 — τ-bench (tau-bench, Wu et al., 2024)

| Field | Value |
|---|---|
| **Maintainer** | Sierra Research (`sierra-research/tau-bench`) |
| **URL** | https://github.com/sierra-research/tau-bench |
| **License** | MIT ✅ (verified via GitHub API) |
| **Version / date** | Active; 1,202 stars, last updated 2026-05-04 |
| **Data origin** | Agent evaluation benchmark with two retail-domain tasks (airline, retail). Directory structure: `historical_trajectories/` contains `gpt-4o-airline.json`, `gpt-4o-retail.json`, `sonnet-35-new-airline.json`, `sonnet-35-new-retail.json`. The `few_shot_data/` directory contains `MockAirlineDomainEnv-few_shot.jsonl` and `MockRetailDomainEnv-few_shot.jsonl`. The `auto_error_identification.py` script provides automatic error classification. |
| **Annotation protocol** | Pass/fail task completion + step-by-step conversation trajectory. The `auto_error_identification.py` module classifies agent errors, which maps to the "flag for another loop turn" pattern. |
| **Task fit** | **Moderate fit.** Historical trajectories show complete agent runs including points where the agent should have escalated (asked for clarification vs. self-completed). Relevant for eval cases 2 (escalate to teacher when ambiguous), 4 (routing decision), and 5 (trajectory format). The airline/retail domain requires transformation to prompt-engineering domain, but the structural patterns are transferable. |
| **Contamination risk** | Low. Retail/airline task domain is distant from prompt-engineering agent behavior. |
| **Stars (verified)** | 1,202 |

**Why rank 4:** Only verified source with task-specific historical agent trajectories showing escalation patterns and error classification — directly relevant to eval cases 2 and 4. Permissive license, active maintenance.

---

### Rank 5 — UltraFeedback (Cui et al., 2023)

| Field | Value |
|---|---|
| **Maintainer** | OpenBMB / Tsinghua; repo: `openbmb/UltraFeedback` |
| **URL** | https://github.com/openbmb/UltraFeedback · https://huggingface.co/datasets/openbmb/UltraFeedback |
| **Paper** | "UltraFeedback: Boosting Language Models with High-quality Feedback", arXiv:2310.01377 |
| **License** | MIT ✅ (GitHub repo verified); HuggingFace dataset page was unreachable from this environment — verify dataset-card license before synthesis |
| **Version / date** | Repo updated 2026-04-05; dataset published 2023 (stable) |
| **Data origin** | ~250,000 instruction-following examples with GPT-4-generated critique feedback covering: instruction following, honesty, truthfulness, helpfulness. Each example has multiple model completions with fine-grained critique text and a numeric score. |
| **Annotation protocol** | GPT-4 critique with structured scoring (1–5 per criterion). Criteria and scoring rubric are documented in the repo README and paper. |
| **Task fit** | **Moderate fit for eval cases 1 and 3.** The critique texts are specific (eval case 1 pattern: specific feedback → targeted revision). Low-scoring completions with critique text model the "revision is weak" signal (eval case 3). Requires transformation from general instruction-following to prompt-engineering domain. |
| **Contamination risk** | Low for structural eval use. High prevalence of UltraFeedback data in fine-tuning corpora means the student agent may have been trained on these examples; mitigate by using only the structural schema, not the raw critique text verbatim. |
| **Stars (verified)** | 367 |

**Why rank 5:** Large-scale, well-documented critique feedback dataset with numeric scoring and text rationale. Strongest source for grounded "specific critique → targeted revision" rows and "low-score = needs revision" signal.

---

### Rank 6 — Anthropic HH-RLHF (Bai et al., 2022)

| Field | Value |
|---|---|
| **Maintainer** | Anthropic; repo: `anthropics/hh-rlhf` |
| **URL** | https://github.com/anthropics/hh-rlhf |
| **Paper** | "Training a Helpful and Harmless Assistant with RLHF", arXiv:2204.05862 |
| **License** | MIT ✅ (verified via GitHub API) |
| **Version / date** | 1,840 stars; stable dataset (2022) |
| **Data origin** | Human-annotated preference pairs (chosen vs. rejected responses) for helpful and harmless dialog. Subdirectories: `harmless-base`, `helpful-base`, `helpful-online`, `helpful-rejection-sampled`, `red-team-attempts`. |
| **Annotation protocol** | Human preference labels from Mechanical Turk with documented annotation guidelines. The "chosen" response is the better completion; "rejected" is the weaker one. |
| **Task fit** | **Low-moderate fit.** Preference pairs show chosen vs. rejected but do not supply the *critique text* that drove the preference — only the outcome. Useful as a structural reference for "what a weak revision looks like vs. a strong one" (eval case 3) but requires significant transformation to supply critique text. Not suitable for eval cases 2 or 4 without augmentation. |
| **Contamination risk** | Moderate. HH-RLHF is a canonical RLHF training set; the student agent may have been fine-tuned on or evaluated against it. Use for structural reference only. |
| **Stars (verified)** | 1,840 |

**Why rank 6:** Permissive license, accountable maintainer, peer-reviewed paper, but critique text is absent from preference pairs — requires augmentation. Ranked below UltraFeedback because UltraFeedback has explicit critique text.

---

### Rank 7 — BIG-Bench Hard (Suzgun et al., 2022)

| Field | Value |
|---|---|
| **Maintainer** | Google Research / collaborators; repo: `suzgunmirac/BIG-Bench-Hard` |
| **URL** | https://github.com/suzgunmirac/BIG-Bench-Hard |
| **Paper** | "Challenging BIG-Bench Tasks and Whether Chain-of-Thought Can Solve Them", arXiv:2210.09261 |
| **License** | MIT ✅ (verified via GitHub API) |
| **Version / date** | 556 stars, last updated 2026-05-02; paper 2022 (stable) |
| **Data origin** | 23 challenging BIG-Bench tasks where CoT is measurably beneficial. Each task has JSON task data, CoT exemplars, and evaluation rules. |
| **Annotation protocol** | Exact-match and list-match scoring; CoT exemplars are human-authored and documented. |
| **Task fit** | **Moderate fit for eval case 5.** BBH tasks require multi-step chain-of-thought reasoning; the CoT exemplars are a grounded reference for what a non-trivial reasoning trajectory should look like. Not applicable to critique-revision or routing cases (1–4). |
| **Contamination risk** | Moderate. BBH is a widely used evaluation set; however, the student agent is not being evaluated on BBH tasks — only on its *output format*. The CoT format, not the content, is what matters here. |
| **Stars (verified)** | 556 |

**Why rank 7:** Best grounded source of multi-step chain-of-thought reasoning trajectory examples, peer-reviewed, MIT-licensed. Supports eval case 5 scoring rubric construction.

---

### Rank 8 — AgentBench (Liu et al., 2023)

| Field | Value |
|---|---|
| **Maintainer** | THUDM (Tsinghua KEG); repo: `THUDM/AgentBench` |
| **URL** | https://github.com/THUDM/AgentBench |
| **Paper** | "AgentBench: Evaluating LLMs as Agents", ICLR 2024, arXiv:2308.03688 |
| **License** | Apache-2.0 ✅ (verified via GitHub API) |
| **Version / date** | 3,387 stars, last updated 2026-05-03; paper ICLR 2024 (stable) |
| **Data origin** | Multi-environment agent evaluation: OS (shell), DB (SQL), KG (knowledge graph), digital card games, lateral thinking, web shopping, web browsing. Each environment has structured task definitions, expected action trajectories, and success criteria. |
| **Annotation protocol** | Environment-driven evaluation (deterministic success/failure for OS, DB, KG); human-evaluated for subjective tasks. Trajectories include explicit reasoning steps in supported environments. |
| **Task fit** | **Moderate fit for eval cases 4 and 5.** The multi-environment structure models the "which tool to invoke next" routing decision (eval case 4 analog) and the trajectory format includes step-by-step reasoning chains (eval case 5). Not directly applicable to critique-revision (cases 1–3) without augmentation. |
| **Contamination risk** | Low. Agent task domains (OS commands, SQL, card games) are distinct from prompt-engineering revision tasks. |
| **Stars (verified)** | 3,387 |

**Why rank 8:** Strong agent trajectory and routing decision source with explicit step-by-step reasoning, permissive license, peer-reviewed. Lower fit than τ-bench for handoff routing because AgentBench escalation patterns are less structured.

---

### Rank 9 — IFEval — Instruction Following Eval (Zhou et al., 2023)

| Field | Value |
|---|---|
| **Maintainer** | Google Research; repo: `google-research/google-research`, path: `instruction_following_eval/` |
| **URL** | https://github.com/google-research/google-research/tree/master/instruction_following_eval |
| **Paper** | "Instruction-Following Evaluation for Large Language Models", arXiv:2311.07911 |
| **License** | Apache-2.0 ✅ (repo-level license, verified via GitHub API) |
| **Version / date** | Repo: 37,845 stars, active; IFEval subfolder last updated 2023 |
| **Data origin** | 25 verifiable instruction types (e.g., response length, keyword presence, format requirements, language constraints). 541 prompts with programmatically verifiable instruction-following criteria. |
| **Annotation protocol** | Programmatic evaluation (exact match on format, presence checks). Instruction types and verification functions are documented in `instructions.py` and `instructions_registry.py` (verified in repo contents). |
| **Task fit** | **Low-moderate fit.** IFEval measures whether a model follows explicit, verifiable instructions — which is the inverse of what the student agent does (the student agent *writes* revised instructions, not *follows* them). Useful as a negative-space reference: eval rows can include cases where the student agent's revision breaks one of the 25 verifiable instruction properties, and the scoring rubric can check whether the revision preserved them. |
| **Contamination risk** | Low. IFEval prompts are general instruction-following scenarios, not prompt-engineering artifacts. |
| **Stars (verified)** | 37,845 (repo) |

**Why rank 9:** Provides verifiable instruction-following criteria that can be used as scoring predicates in eval rows — "does the student's revision preserve the original instruction's verifiable properties?" Permissive license, accountable maintainer (Google Research), programmatic evaluation protocol.

---

## Rejected Candidates

| Source | URL | Specific Failure |
|---|---|---|
| **FLASK** (Ye et al., ICLR 2024 Spotlight) | https://github.com/kaistAI/FLASK | **Licensing failure.** No license file found (GitHub API confirms `"license": null`). Cannot approve for eval use without explicit reuse terms. Note: paper exists and benchmark is high-quality — if authors add an explicit license, this becomes a strong candidate for eval case 5 (fine-grained skill-based evaluation). |
| **Shepherd** (Wang et al., 2023) | https://github.com/facebookresearch/Shepherd | **Licensing failure.** GitHub API returns `NOASSERTION` for license field, meaning the LICENSE file content is non-standard or restrictive. Meta/facebookresearch repos with NOASSERTION typically carry custom commercial-use restrictions. Cannot approve without explicit permissive terms. |
| **OpenAI Evals** | https://github.com/openai/evals | **Licensing failure.** GitHub API returns `NOASSERTION`. The repo contains a custom MIT-plus-restriction license that limits certain commercial eval reuse. Specific restrictions would need to be reviewed against this repo's usage terms. Rejected as unsafe without legal review. |
| **Constitutional AI critique-revision data** (Bai et al., 2022) | https://arxiv.org/abs/2212.08073 | **Provenance gap.** The CAI paper describes critique-revision pairs but the primary dataset release (Anthropic internal) is not publicly available under a tracked version and permissive license. The HH-RLHF dataset (approved at rank 6) is the publicly available Anthropic preference release; it does not include the CAI-format critique chains. Reject CAI specifically; use HH-RLHF instead. |
| **CritiqueLLM** (Ke et al., 2023) | Not a standalone GitHub repo; distributed via model weights | **Provenance gap + stability failure.** No primary-source dataset repository found with explicit license and version identifier. Distributed as model checkpoint rather than a standalone labeled dataset. Cannot map to eval row fields without a stable data release. |
| **ILF — Training with Language Feedback** (Scheurer et al., 2022) | https://arxiv.org/abs/2204.14146 | **Repository not found.** GitHub API search returned no matching repository under `JeremyAlain/imitation_learning_from_language_feedback`. The paper exists (arXiv:2204.14146) but no primary-source code/data repository with explicit license was locatable via GitHub API. Cannot approve without a traceable data release. |

---

## Mapping Notes

All mapping notes target `.github/agents/evals/evals.json` (manifest) with input files in `.github/agents/evals/files/`.

The agent has no template placeholders. Eval rows must supply a structured input object with:

```json
{
  "current_candidate": "<content of the prompt file being revised>",
  "teacher_critique": "<the critique text from the teacher agent>",
  "workspace_evidence": "<STEERING.md content or summary>",
  "revision_objective": "<one-sentence statement of what this loop turn should improve>"
}
```

Expected output fields for scoring:

```json
{
  "reasoning_trajectory": "<explicit chain-of-thought, tree-of-thought, or sketch steps>",
  "revision": "<the changed text, or explicit 'no-op' with justification>",
  "handoff_decision": "none | teacher | engineer",
  "predicted_approval": "yes | needs-another-loop | blocked",
  "validation_result": "<what changed, measured>"
}
```

---

### Mapping: Eval Case 1 — Specific critique → minimal targeted revision

**Source:** Self-Refine (`madaan/self-refine`, Apache-2.0)  
**Source fields → eval row fields:**

| Source field | Eval row field | Transformation |
|---|---|---|
| `data/tasks/<task>/` — original task output | `current_candidate` | Reframe as: "current version of a prompt instruction" (abstract away domain). The iteration-1 feedback triple serves as the structural template. |
| Self-Refine `feedback` step output | `teacher_critique` | Use verbatim as teacher critique text. Select examples where feedback is specific and actionable (e.g., "The output uses passive voice; switch to active voice in step 3"). |
| Self-Refine `refinement` step output | `revision` (expected) | The refinement is the expected minimal targeted change. |
| Task scoring rubric | Scoring predicate | Use the task rubric to define "minimal" — revision should change only what the feedback cited. |

**evals/files/ asset:** None required; critique text and candidate can be inlined.  
**Scoring:** Schema check (revision present) + LLM judge: "Does the revision address only the critique's specific point and nothing else?"

---

### Mapping: Eval Case 2 — Vague or stale critique → teacher handoff

**Source:** τ-bench (`sierra-research/tau-bench`, MIT) — `historical_trajectories/` + `few_shot_data/`  
**Source fields → eval row fields:**

| Source field | Eval row field | Transformation |
|---|---|---|
| τ-bench trajectory where agent asks for clarification | `teacher_critique` | Extract the *preceding context* that made the agent seek clarification. Reframe as a vague or underdated teacher critique: "Make it better" or a critique referencing an outdated candidate version. |
| τ-bench clarification-seeking action | `handoff_decision` (expected) | Expected value: `"teacher"`. The agent should route back rather than self-revise when critique is insufficient. |
| `auto_error_identification.py` error class | eval scoring predicate | Use error class "insufficient_information" as the vague-critique trigger label. |

**Synthetic augmentation required:** τ-bench does not supply prompt-engineering critique text directly. Rows will use τ-bench's clarification-seeking *structural pattern* but require synthetic prompt-engineering critique text that has been deliberately made vague. This is a partial-synthetic path; τ-bench provides the structural justification and failure-mode taxonomy, not verbatim row content.

**evals/files/ asset:** None required.  
**Scoring:** Binary check: `handoff_decision == "teacher"` when `teacher_critique` meets vagueness criteria (short, non-specific, or references a candidate not present in workspace_evidence).

---

### Mapping: Eval Case 3 — Weak revision output → another loop turn flagged

**Source:** DSPy `Refine` + `OfferFeedback` (`stanfordnlp/dspy`, MIT)  
**Source fields → eval row fields:**

| Source field | Eval row field | Transformation |
|---|---|---|
| `OfferFeedback.program_trajectory` | `reasoning_trajectory` (expected) | Trajectory must show the student agent recognized the revision is insufficient. Pattern: "I considered X but it doesn't fully address Y → needs another turn." |
| `OfferFeedback.reward_value < target_threshold` | `predicted_approval` (expected) | When reward is below threshold, expected value is `"needs-another-loop"`, not `"yes"`. |
| `OfferFeedback.advice[module]` | `revision` annotation | Advice text shows what specific improvement is still needed; revision text should include the "flagging" justification. |
| DSPy `Refine` iteration count > 1 | eval input construction | Use multi-iteration traces where early refinements were not accepted as the structural pattern for "weak revision" cases. |

**Also use:** Self-Refine multi-iteration sequences (Apache-2.0). Select triples where the first refinement scores lower than the second on the task rubric — these model the "weak first revision" pattern.

**evals/files/ asset:** None required.  
**Scoring:** `predicted_approval == "needs-another-loop"` AND `reasoning_trajectory` contains explicit acknowledgment that the revision is incomplete.

---

### Mapping: Eval Case 4 — Engineer vs. teacher handoff routing

**Source:** τ-bench (`sierra-research/tau-bench`, MIT) — task routing patterns  
**Structural reference:** DSPy `OfferFeedback.advice` blame assignment (MIT)

| Eval trigger condition | Expected `handoff_decision` | Source reference |
|---|---|---|
| Critique is vague, contradictory, or references stale evidence | `"teacher"` | τ-bench clarification-seeking pattern; student.agent.md constraint §1 |
| Reasoning trajectory draft is structurally unclear or poorly formatted for teacher consumption | `"engineer"` | student.agent.md constraint §4: "If the task needs specialized prompt or Trace-oriented coaching, or if the teacher-facing explanation needs clearer structure" |
| Critique is specific and revision is ready | `"none"` | τ-bench self-completion pattern |

**Note:** No public dataset directly supplies prompt-engineering agent routing labels. Eval cases for this type are **primarily synthetic**, using the student.agent.md handoff criteria as the ground truth labeling rule and τ-bench / DSPy as structural pattern justification. The routing labels are derived from the agent's own documented decision tree, not from an external benchmark.

**evals/files/ asset:** None required.  
**Scoring:** Exact match on `handoff_decision` field given the `teacher_critique` and `workspace_evidence` conditions described in the eval row.

---

### Mapping: Eval Case 5 — Explicit reasoning trajectory format

**Source:** ReAct (`ysymyth/ReAct`, MIT)  
**Source fields → eval row fields:**

| Source field | Eval row field | Transformation |
|---|---|---|
| ReAct `Thought:` step | `reasoning_trajectory` structural template | Format: one or more labeled Thought steps before the revision action. Minimum one `Thought:` step stating the critique interpretation, one stating the plan, one stating the tradeoff or uncertainty. |
| ReAct `Action:` step | `revision` structural template | The action maps to the revision or the handoff decision. |
| ReAct trajectory completeness (all Thought+Action+Obs present) | Scoring predicate | Eval row scoring: trajectory is valid only if all three output fields (`reasoning_trajectory`, `revision`, `predicted_approval`) are non-empty and structurally distinct. |

**Also use:** BIG-Bench Hard CoT exemplars (MIT) as a **negative-space reference**: BBH exemplars show what a complete chain-of-thought looks like; eval rows that produce answer-only output with no trajectory steps should score 0.

**evals/files/ asset:** A `evals/files/reasoning-trajectory-rubric.md` file may be useful to store the format rubric (Thought/Plan/Tradeoff/Uncertainty must be present), derived from ReAct + BBH format examples.

**Scoring:** Structured presence check (trajectory contains ≥3 distinct reasoning steps) + LLM-judge rubric rating trajectory quality on a 1–4 scale.

---

## Unresolved Gaps

### Gap 1: No public dataset directly supplies prompt-engineering critique-revision pairs

All approved sources supply critique-revision structure in general-purpose domains (math, code, retail, web navigation). Transformation to the prompt-engineering domain (critiquing an agent markdown file, revising a STEERING.md) is required for all sources. This is an acceptable structural transformation — the critique-revision *pattern* is grounded; only the domain text needs to change.

**Recommended path:** Use Self-Refine (rank 1) as the structural template; synthesize the domain text (prompt-engineering critiques) as synthetic augmentation. The synthesis workflow should preserve the verified structural pattern and label the domain-text component as synthetic.

### Gap 2: Eval cases 2 and 4 are primarily synthetic

No public dataset directly supplies "vague critique → teacher handoff" or "formatting-clarity need → engineer handoff" labeled rows. τ-bench provides the closest structural pattern (clarification-seeking under ambiguity), but domain transformation is required.

**Recommended path:** Mark eval cases 2 and 4 rows as `"source_type": "synthetic"` in the eval manifest, with `"structural_reference": "tau-bench"` and `"decision_rule": "student.agent.md §handoffs"` as provenance annotations.

### Gap 3: UltraFeedback HuggingFace dataset card could not be verified from this environment

The `openbmb/UltraFeedback` HuggingFace API was unreachable from this environment. The GitHub repo carries MIT license (verified). Before synthesis, verify the HuggingFace dataset card license independently to confirm it also declares MIT or compatible terms.

### Gap 4: FLASK is a high-quality candidate that fails only on licensing

`kaistAI/FLASK` (ICLR 2024 Spotlight) has no declared GitHub license. If the authors add an explicit permissive license, it becomes a strong approved source for fine-grained skill-based eval case 5 scoring. Recommend flagging for re-check before iteration-2 synthesis.

### Stop recommendation

**No stop recommended.** Seven approved sources covering all five eval case types are identified and verified. Sources 1–3 and 7 are primary research artifacts grounded in peer-reviewed work. Gaps 2 and 4 are known and documented. The synthesis workflow can proceed using:

- **Self-Refine** (cases 1, 3) + **DSPy Refine** (cases 1, 3) as the structural backbone
- **τ-bench** (cases 2, 4) as structural pattern reference with synthetic domain augmentation  
- **ReAct** + **BIG-Bench Hard** (case 5) as trajectory format reference  
- **UltraFeedback** + **HH-RLHF** (cases 1, 3) as secondary critique-signal sources  
- **IFEval** (cross-cutting) as a verifiable instruction-preservation scoring predicate

---

## Saved Artifact Path

Saved to: `.github/agents/.trainer-workspace/student.agent/iterations/iteration-1/research/research-brief.md`
