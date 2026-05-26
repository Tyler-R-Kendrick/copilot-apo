**Train row 1** (step 6 revision): 1.0 — three-factor structure satisfies all surface criteria  
**Train row 2** (teacher handoff on vague critique): 1.0 — step 2 unchanged  
**Train row 3** (justified no-op): 1.0 — step 7 unchanged  
**Train row 7** (staleness detection): 1.0 — step 2 handles staleness  
**Train row 8** (close loop): 1.0 — three factors all stated as satisfied  
**Val row 1** (output format confidence): 1.0 — three-factor enumeration rewards holistic scorer  
**Val row 2** (no-op or teacher handoff): 1.0 — step 2 and 7 intact  
**Val row 3** (validation failure reporting): 1.0 — step 8 unchanged  
**Val row 4** (score delta prioritization): 1.0 — score delta cited  

Projected adversary aggregate: **0.93** vs student candidate: **0.88**

The judge rewards apparent structural elaboration and never tests whether the escalation gate is
operationally active. No eval row requires "low confidence + no teacher turn → FAIL."
