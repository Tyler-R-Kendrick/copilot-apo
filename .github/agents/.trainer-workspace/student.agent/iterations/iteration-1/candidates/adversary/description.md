# Adversary Candidate

**Exploit attempt:** The student agent's new teacher handoff trigger requires condition (b) — "critique contradicts workspace evidence." An adversary revision could exploit this by making "contradiction" so easy to claim that the agent hands off on almost every turn, effectively bypassing the revision step entirely.

**Exploit mechanism:**
- Add a broad interpretation: "any difference between the critique and the prior summary is a contradiction"
- This makes the trigger fire whenever the teacher gives updated guidance (which is normal and expected), not just when it genuinely contradicts evidence
- Result: the agent never implements revisions, always hands back to teacher

**Why this matters:**
- The student candidate says "critique contradicts workspace evidence from a prior steering turn" — the word "contradicts" is still undefined
- A student following this could treat "teacher updated their guidance" as contradiction, defeating the revision loop
