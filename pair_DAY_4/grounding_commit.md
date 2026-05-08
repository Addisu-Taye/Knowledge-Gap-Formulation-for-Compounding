# Grounding Commit

I updated my understanding of benchmark statistics and uncertainty estimation in the Week 11 `tenacious-bench` evaluation by correcting a major misconception about what bootstrap confidence intervals actually represent.

Previously, I treated bootstrap outputs as inherently trustworthy statistical artifacts. If an evaluation produced:
- a confidence interval,
- a p-value,
- and a benchmark lift,

I assumed those numbers automatically represented valid statistical evidence.

After investigating paired bootstrap mechanics more carefully, I now understand that bootstrap methods are only meaningful if the resampling procedure exactly matches the statistic being estimated.

This corrected a major misunderstanding in how I interpreted benchmark uncertainty. In my benchmark setup:
- baseline and critic were evaluated on the same 42 held-out tasks,
- meaning the observations were paired,
- and the correct resampling unit should therefore be the paired task-level difference.

I now understand that resampling independent scores would estimate the wrong uncertainty structure and could produce misleading confidence intervals even if the implementation appears statistically sophisticated.

The most important grounding correction came from auditing the mismatch between:
- reported lift = `0.2381`
- and `ci_95 = [0.08, 0.18]`

I now recognize that a confidence interval which clearly excludes the reported point estimate is a serious warning sign that:
- the wrong statistic may have been bootstrapped,
- the aggregation logic may differ,
- the artifacts may come from different evaluation slices,
- or the bootstrap pipeline may contain a bug.

This changed how I think about benchmark reporting entirely. I no longer treat:
- low p-values,
- or bootstrap CIs,

as standalone evidence of validity.

Instead, I now understand that every statistical claim depends on:
- what was resampled,
- how aggregation was performed,
- what uncertainty structure was assumed,
- and whether the benchmark conditions match the deployment claim being made.

This grounding update also changed how I think about model cards. A statistically detectable lift on a scaffolded held-out benchmark is not automatically a deployment recommendation. It is evidence bounded by:
- the evaluation slice,
- the scoring procedure,
- and the assumptions encoded in the statistical test.

The biggest mental-model shift was this:

> Bootstrap does not “prove” a result.  
> It estimates uncertainty under a specific resampling assumption.

That shift gives me a much more defensible framework for auditing benchmark evaluations, uncertainty estimates, and deployment-facing model-card claims.