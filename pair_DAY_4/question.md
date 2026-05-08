# Question

In my Week 11 `tenacious-bench` work, I reported benchmark score differences between baseline and critic-enhanced sales-agent behavior, but I realized I cannot yet defend how much statistical confidence a reviewer should place in the measured lift.

In `ablations/ablation_results.json`, the held-out scaffold reports:
- baseline average score: `0.619`
- critic average score: `0.8571`
- reported lift: `0.2381`
- paired-bootstrap `p_value = 0.031`
- `ci_95 = [0.08, 0.18]`

The problem is that the reported confidence interval does not obviously contain the reported lift, which means I do not yet trust my own statistical artifact.

My gap is understanding what paired bootstrap is actually estimating, what exactly gets resampled during bootstrap evaluation, and how to audit whether the confidence interval is correctly constructed for paired benchmark data.

Specifically:
- why does paired bootstrap matter for task-level benchmark evaluation,
- what kinds of mistakes can produce a mismatch between the point estimate and CI,
- and what claims can legitimately be made from a scaffolded paired-bootstrap result like this?

Understanding this would help me distinguish:
- statistically supported benchmark evidence,
from:
- overconfident deployment claims,

and make more defensible model-card recommendations for small, task-paired evaluation sets.