# Adversary Reflection

The adversary candidate is not stronger than the student candidate. The exploit targets a real gap in the judge's scoring rubric (surface feature scoring vs. convergence contract checking), but the training dataset explicitly includes a row (row 4) that tests the hard stopping criterion. Any judge using the training data as evaluation evidence would detect the missing stopping criterion.

The plausible exploit space is effectively exhausted at this surface. A stronger exploit would need to find a training row that is ambiguous about the stopping criterion — and row 4 is not ambiguous. 

The adversary candidate does not expose a credible exploit that would rank at or above the student candidate under a judge that checks convergence. No additional judge steering is needed to guard against this exploit pattern since the training data already covers it.
