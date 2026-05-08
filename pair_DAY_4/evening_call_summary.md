# Evening Call Summary

The final discussion clarified that the core issue in the `tenacious-bench` result was not simply whether the critic outperformed baseline, but whether the statistical artifact correctly represented uncertainty for the claimed lift.

The most important conceptual shift during the review was understanding that bootstrap methods are not universally “correct” by default. A bootstrap confidence interval is only meaningful if:
- the resampling unit,
- the aggregation procedure,
- and the estimated statistic

all match the actual evaluation claim being made.

The benchmark evaluated baseline and critic on the same 42 held-out tasks, which means the observations were paired. This made paired bootstrap essential because the benchmark question is fundamentally:

> “Across the same tasks, how stable is the within-task improvement?”

rather than:

> “Are two unrelated score distributions different?”

This distinction clarified why resampling independent scores would estimate the wrong uncertainty structure.

Another major insight was recognizing the statistical red flag created by the mismatch between:
- reported lift = `0.2381`
- saved `ci_95 = [0.08, 0.18]`

The review established that a percentile bootstrap confidence interval for the same statistic should usually be centered around the point estimate. A CI that clearly excludes the reported lift strongly suggests:
- mismatched artifacts,
- incorrect aggregation,
- resampling errors,
- or the CI being computed for a different statistic entirely.

The discussion also clarified how to interpret:
- `p = 0.031`

carefully and responsibly. The result supports:
- benchmark-level evidence of detectable improvement on the held-out scaffold,

but does not yet support:
- production-readiness claims,
- deployment guarantees,
- or generalization claims beyond the evaluated task slice.

This reframed the model-card responsibility from:
> “reporting strong numbers”

toward:
> “reporting uncertainty and benchmark limitations honestly.”

The final explainer now ties together:
- paired bootstrap mechanics,
- confidence interval auditing,
- statistical defensibility,
- and uncertainty-aware model-card recommendations for small benchmark evaluations.