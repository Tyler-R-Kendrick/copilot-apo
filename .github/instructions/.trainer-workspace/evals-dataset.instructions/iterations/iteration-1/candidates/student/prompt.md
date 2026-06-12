---
description: "Use when editing official skill eval manifests under evals/evals.json. Covers eval case shape, file references, assertions, and workspace-result expectations."
applyTo: "**/evals/evals.json"
---
# Skill Eval Guidance

- Keep the manifest root as a JSON object with `skill_name` and an `evals` array.
- Keep each eval case realistic: `prompt` should read like an actual user request, not a label or shorthand.
- Use `expected_output` for a human-readable success description, not brittle exact-match text unless exact wording is the real requirement.
- Keep `files` paths relative and prefer assets under `evals/files/`.
- Add `assertions` only when they are objective and observable; leave subjective qualities for later human review.
- Start with a small, varied set of cases and expand coverage only after the first evaluation loop exposes real gaps.

## Required and Optional Fields

Every eval row must include:
- `prompt` — a realistic user request string (never a label like "Test case 1")
- `expected_output` — a human-readable description of a successful response

Optional fields follow these rules when present:
- `assertions` — a list of string predicates that are objectively checkable (e.g., "Response contains a category label", "Response is valid JSON"); do not add subjective assertions
- `files` — a list of paths relative to the eval manifest directory; always store supporting assets under `evals/files/`
- `scoring` — one of `deterministic`, `custom`, or `llm_judge`:
  - `deterministic` — for objective, rule-based checks (valid syntax, field presence, exact matches)
  - `llm_judge` — for semantic or quality judgment (user intent, tone, accuracy)
  - `custom` — for domain-specific or schema-based normalization
- `criteria` — (Optional, for `llm_judge` only) A rubric that guides the judge's evaluation. Differs from `expected_output` in that it describes *how* to evaluate, not *what* success looks like.

## Example: Correct Eval Row

```json
{
  "prompt": "Categorize this support ticket: My invoice shows an incorrect charge.",
  "expected_output": "The skill should classify the ticket as billing and return a category label.",
  "assertions": ["Response contains a category label", "Category is one of: billing, technical, general"],
  "scoring": "llm_judge",
  "criteria": "The response must identify the correct ticket category with a clear label."
}
```

## Forbidden Patterns

| ❌ Bad Form | ✅ Good Form |
|---|---|
| `"prompt": "Test case 3: billing classification"` | `"prompt": "My credit card was charged twice for the same order. Which category does this fall under?"` |
| `"files": ["/absolute/path/to/file"]` | `"files": ["evals/files/data.json"]` |
| `"expected_output": "billing"` (exact match for open task) | `"expected_output": "The response should identify the ticket category with a clear label (e.g., 'billing', 'technical', 'general')"` |
| `"assertions": ["User seems satisfied"]` | `"assertions": ["Response contains a category label", "Category is one of: billing, technical, general"]` |
