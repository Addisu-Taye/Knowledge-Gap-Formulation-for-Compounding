## How to Audit a Paired Bootstrap Result Before Making a Model-Card Claim

Gersum’s tenacious-bench result looks strong on the surface:

*   **Baseline average:** 0.619
*   **Critic average:** 0.8571
*   **Reported lift:** 0.2381
*   **Tasks:** 42
*   **Paired bootstrap p_value:** 0.031
*   **Saved ci_95:** [0.08, 0.18]

But there is an immediate **red flag**: The reported 95% CI [0.08, 0.18] does not contain the point estimate 0.2381. 

That does not automatically mean the result is false. But it does mean the statistical artifact cannot be trusted until the bootstrap construction is audited. The core question is: **Was uncertainty estimated over the same statistic that produced the reported lift?**

---

### 1. What Paired Bootstrap Should Resample
In this benchmark, the baseline and critic are evaluated on the same 42 tasks. The observations are paired: `task_i: baseline_score_i, critic_score_i`.

The unit of resampling should be the **task**, not individual model scores. For each bootstrap iteration:
1.  **Sample** 42 task indices with replacement.
2.  **Pull** both baseline and critic scores for each sampled task.
3.  **Compute:** `mean(critic_scores_sampled) - mean(baseline_scores_sampled)`.
4.  **Store** that lift and repeat 10,000 times.
5.  **Take** the 2.5th and 97.5th percentiles of the bootstrapped lifts.

This is the right structure because it asks: *On the same set of tasks, how much better is the critic than the baseline?*

---

### 2. Why Paired Bootstrap Matters Here
A non-paired bootstrap might separately resample baseline scores and critic scores. That would be wrong because it breaks the task-level relationship. Some tasks are easy or hard for both systems; the paired bootstrap preserves that covariance. Instead of asking “How variable are two unrelated samples?”, it asks: 

> **“Across these 42 task-level deltas, how stable is the average improvement?”**

---

### 3. Why the CI Mismatch is a Serious Warning
If the reported lift is $0.8571 - 0.619 = 0.2381$, then a percentile bootstrap CI for that statistic should be centered roughly around 0.2381. It should not miss the point estimate unless:

*   **Cause 1: Wrong Statistic.** The CI was computed for something else (e.g., error reduction).
*   **Cause 2: Dataset Mismatch.** Lift and CI came from different data slices.
*   **Cause 3: Unpaired Resampling.** The bootstrap broke the task-level pairing.
*   **Cause 4: Storage Error.** The CI represents margins or deltas incorrectly.
*   **Cause 5: Aggregation Bug.** Macro-averaging was handled differently in the bootstrap vs. the summary.

---

### 4. A Concrete Audit Script
The audit should recompute the statistic from raw paired task scores.

```python
import json
import numpy as np

rng = np.random.default_rng(0)

with open("ablations/ablation_results.json") as f:
    data = json.load(f)

# Expected: [{"task_id": ..., "baseline_score": ..., "critic_score": ...}, ...]
rows = data["held_out_scaffold_scores"]

baseline = np.array([r["baseline_score"] for r in rows], dtype=float)
critic = np.array([r["critic_score"] for r in rows], dtype=float)

assert len(baseline) == len(critic) == 42

diffs = critic - baseline
point_lift = diffs.mean()

B = 10_000
boot_lifts = []
n = len(diffs)

for _ in range(B):
    idx = rng.integers(0, n, size=n)
    boot_lifts.append(diffs[idx].mean())

lo, hi = np.percentile(boot_lifts, [2.5, 97.5])

# Two-sided bootstrap p-value for lift <= 0
p_value = 2 * min(
    np.mean(np.array(boot_lifts) <= 0),
    np.mean(np.array(boot_lifts) >= 0),
)

print("point_lift:", round(point_lift, 4))
print("ci_95:", [round(lo, 4), round(hi, 4)])
print("p_value:", round(p_value, 4))
5. How to Interpret p = 0.031A $p = 0.031$ result should be messaged carefully:“On the 42-task held-out scaffold evaluation, the critic variant showed a higher average score (+0.2381). A paired bootstrap test produced p = 0.031, suggesting the lift is unlikely under a no-improvement resampling baseline. However, the CI artifact requires audit before being used as a deployment claim.”6. What Claims This Result Can SupportSupported ClaimNot Yet Supported"In a 42-task scaffold, the critic outperformed baseline by +0.2381.""The critic is deployment-ready.""The lift is statistically detectable on this scaffolded slice (p = 0.031).""The critic will improve live agent behavior by 23.8 points."7. What Should Change in the Model CardThe model card should be defensible and honest:On the 42-task held-out scaffold slice, the critic variant improved average score from 0.619 to 0.8571 (lift = +0.2381). A paired bootstrap produced p = 0.031. However, the current saved CI artifact (ci_95 = [0.08, 0.18]) does not contain the point estimate and must be audited. This is scaffold evidence, not a live deployment guarantee.Closing Insight: The paired bootstrap only answers the question encoded in the resampling. If the CI doesn't contain the point estimate, the question was likely encoded incorrectly. Recompute, verify, and downgrade the claim until the audit passes.