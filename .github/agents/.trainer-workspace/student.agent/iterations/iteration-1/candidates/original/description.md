# Original Candidate: student.agent.md

## Source
Unchanged baseline from `.github/agents/student.agent.md` at training start.

## Known Issues (from engineering review)
1. No explicit evidence reading order — agents start revising without reading STEERING.md first.
2. No out-of-scope revision guard — placeholders and eval shapes can be changed unintentionally.
3. Over-broad teacher handoff trigger — 4 conditions trigger teacher handoff, causing loop flooding.
4. Ambiguous engineer handoff — boundary between "reformatting" and "delegating revision" is unclear.
5. No write-back path — candidate save location is never specified.
6. Unbounded self-check loop — "at most one extra self-check" is too vague.

## Predicted Judge Response
Judge would score ~0.5 on the dataset rows because: the agent would often skip STEERING.md reading, miss the write-back step, and over-trigger teacher handoffs. Evidence reading order and revision scope discipline failures would cause partial failures on 3 of 6 training rows.
