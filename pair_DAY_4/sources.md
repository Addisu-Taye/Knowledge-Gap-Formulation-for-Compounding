# Sources

## Primary Statistical References

1. Efron & Tibshirani — *An Introduction to the Bootstrap*  
https://www.taylorfrancis.com/books/mono/10.1201/9780429246593/introduction-bootstrap-bradley-efron-tibshirani

→ Foundational reference for bootstrap resampling methods and paired resampling logic.

---

2. SciPy Bootstrap Documentation  
https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.bootstrap.html

→ Reference for bootstrap confidence intervals and bootstrap-distribution construction.

---

3. Mitchell et al. (2019) — *Model Cards for Model Reporting*  
https://arxiv.org/abs/1810.03993

→ Framework for communicating evaluation limitations, uncertainty, and deployment caveats in model-card recommendations.

---

## Benchmark Artifacts Referenced

### `ablations/ablation_results.json`

Reported:
- baseline average: `0.619`
- critic average: `0.8571`
- lift: `0.2381`
- `ci_95 = [0.08, 0.18]`

Observed issue:
- CI does not contain reported lift.

---

### `ablations/statistical_test_output.json`

Reported:
- paired bootstrap
- iterations = `10000`
- `p_value = 0.031`
- scaffolded evaluation status

---

### `tenacious_bench_v0_1/held_out/tasks.jsonl`

Held-out benchmark task slice used for evaluation claims.

---

### `eval/tenacious_bench/scoring_evaluator.py`

Deterministic evaluator responsible for:
- pass/fail scoring
- dimension-level evaluation
- benchmark aggregation

---

## Key Statistical Concepts

- paired bootstrap
- confidence intervals
- task-level paired resampling
- benchmark uncertainty
- scaffolded evaluation
- statistical defensibility
- deployment claims
- benchmark generalization limits

---

## Core Insight

For paired benchmark evaluations, the bootstrap must resample:
- paired task-level differences,

not:
- independent model outputs.

The validity of the CI depends entirely on whether the resampling procedure matches the statistic being claimed.