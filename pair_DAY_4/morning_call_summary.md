# Morning Call Summary

The discussion started from a statistical inconsistency in the Week 11 `tenacious-bench` evaluation results. The benchmark reported:
- baseline average = `0.619`
- critic average = `0.8571`
- lift = `0.2381`
- paired-bootstrap `p = 0.031`

but the saved confidence interval:
- `ci_95 = [0.08, 0.18]`

did not appear to contain the reported lift.

This immediately raised concerns about whether the bootstrap procedure was estimating the same statistic that produced the point estimate.

The key focus of the discussion became:
- what paired bootstrap is actually resampling,
- why paired resampling matters for benchmark tasks evaluated on the same examples,
- and how to audit whether a confidence interval is statistically defensible.

A major clarification was distinguishing:
- resampling independent scores,
from:
- resampling paired task-level differences.

Because the baseline and critic were evaluated on the same 42 tasks, the correct resampling unit should be the task pair itself. Breaking the pairing would estimate a different uncertainty structure.

Another important point was that bootstrap methods are not inherently trustworthy just because they are widely used. Their validity depends entirely on:
- the statistic being estimated,
- the resampling procedure,
- and the aggregation logic matching correctly.

The conversation also shifted toward model-card implications:
- what kinds of claims a scaffolded benchmark can legitimately support,
- and why statistically detectable benchmark improvements are not automatically deployment guarantees.

The explainer direction became focused on:
- paired bootstrap mechanics,
- CI auditing,
- benchmark defensibility,
- and uncertainty-aware model-card reporting.