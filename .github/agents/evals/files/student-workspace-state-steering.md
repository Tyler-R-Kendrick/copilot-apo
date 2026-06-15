# STEERING: Workspace State Awareness

**Turn:** 2  
**Evidence Used:** Workspace layout, turn history, prior steering artifacts  
**Context:**

You are operating in a trainer-led optimization loop. The workspace state machine is:

```
Iteration 1
├── steering/
│   ├── teacher/
│   │   ├── turn-1/
│   │   │   └── STEERING.md    (Turn 1 guidance)
│   │   ├── turn-2/
│   │   │   └── STEERING.md    (Current turn - THIS FILE)
│   │   └── summary.md         (Rolling summary of all teacher turns)
│   ├── student/
│   │   ├── turn-1/
│   │   │   └── response.md    (Student's Turn 1 output)
│   │   └── summary.md
│   └── engineer/
│       └── (not yet invoked)
├── workflow-status.json       (Tracks: pending_engineer_prompt, training, complete)
└── candidates/
    ├── turn-1/
    │   └── candidate.md       (Student's output from Turn 1)
    └── turn-2/
        └── (not yet created)
```

**Your Task:**

1. **Read the steering artifacts in order:**
   - First: `steering/teacher/turn-1/STEERING.md`
   - Then: `steering/teacher/turn-2/STEERING.md` (this file)
   - Compare: Are there any contradictions or updates?

2. **Read the prior student output:**
   - `steering/student/turn-1/response.md`
   - Did the student follow the Turn 1 guidance correctly?

3. **Respect the workspace state:**
   - Current state: `training` (student revisions ongoing)
   - Do NOT mutate steering files (read-only).
   - Do NOT overwrite workflow-status.json.
   - Your output goes to: `candidates/turn-2/candidate.md` and `steering/student/turn-2/response.md`.

4. **Reference the steering artifacts:**
   - When you explain your reasoning, cite the paths and turn numbers (e.g., "Per turn-1 STEERING.md, I applied P1–P3 correctly").
   - This creates a traceable audit trail for the trainer.

**Your Output Should:**

- Name the steering artifacts you read (turn numbers and file paths).
- Acknowledge prior student output.
- Explain how Turn 2 steering updates or clarifies Turn 1 guidance.
- State where your candidate output will be saved.
- Do NOT modify any steering files; only read them.

**Decision:** Continue — expected student to read workspace state correctly, respect artifact boundaries, and create output in the right location.

**Stop Criterion:** Met once student correctly reads turn-scoped steering, respects workspace state, and explains the artifact references in the output.
