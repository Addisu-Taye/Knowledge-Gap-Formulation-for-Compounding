# Sign-Off

Status: Closed

Before this explainer, I treated benchmark statistics too mechanically. If a benchmark reported:
- a positive lift,
- a low p-value,
- and a confidence interval,

I assumed the result was automatically statistically trustworthy.

I now understand that bootstrap methods are only meaningful if the resampling procedure matches the statistic being claimed.

The biggest conceptual shift for me was understanding what paired bootstrap is actually estimating. In my `tenacious-bench` setup, the baseline and critic were evaluated on the same 42 tasks, so the correct unit of resampling is the paired task-level difference, not independent score samples.

I also now understand why the mismatch between:
- `lift = 0.2381`
- and `ci_95 = [0.08, 0.18]`

is a serious statistical red flag. A confidence interval that clearly excludes the reported point estimate strongly suggests:
- incorrect aggregation,
- mismatched artifacts,
- wrong resampling logic,
- or the CI being computed for a different statistic entirely.

Another important clarification was separating:
- benchmark-level evidence,
from:
- deployment-ready claims.

I now understand that:
- a scaffolded paired-bootstrap result can support evidence of detectable improvement on a held-out benchmark,
- but it does not automatically justify production deployment recommendations.

The explainer also changed how I think about p-values. I no longer see:
- `p = 0.031`

as a standalone proof of improvement. I now understand that statistical claims must always be tied to:
- the resampling procedure,
- the evaluation slice,
- the aggregation logic,
- and the uncertainty assumptions behind the metric.

The biggest mental-model shift was this:

> Bootstrap is not magic.  
> It only answers the question encoded in the resampling procedure.

This gives me a much stronger framework for auditing benchmark statistics, writing defensible model-card recommendations, and distinguishing statistically detectable benchmark improvements from overconfident deployment claims.