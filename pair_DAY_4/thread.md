1/  
One of the most important lessons I learned this week:

A benchmark number is NOT the same thing as statistical evidence.

My Week 11 evaluation reported:

• baseline = 0.619  
• critic = 0.8571  
• lift = +0.2381  
• paired-bootstrap p = 0.031  

Looks convincing, right?

Then I noticed the saved 95% CI was:

[0.08, 0.18]

…and it didn’t even contain the reported lift 👇

---

2/  
That immediately raised a deeper question:

What exactly was the bootstrap estimating?

Because bootstrap results are only meaningful if:
- the resampling procedure
- matches the statistic being claimed.

---

3/  
In my benchmark, baseline and critic were evaluated on the SAME 42 tasks.

That means the data is paired:

task_i:
- baseline_score_i
- critic_score_i

The resampling unit should therefore be the TASK.

Not individual scores independently.

---

4/  
This is why paired bootstrap matters.

The real question is NOT:

“Are these two unrelated score distributions different?”

The real question is:

“Across the same tasks, how stable is the within-task improvement?”

That’s a completely different uncertainty estimate.

---

5/  
A proper paired bootstrap should:

1. Resample tasks WITH replacement  
2. Keep baseline/critic pairs together  
3. Compute mean(task_deltas)  
4. Repeat thousands of times  
5. Build the CI from the bootstrapped lift distribution

---

6/  
So why was my CI suspicious?

If:
lift = +0.2381

then the bootstrap CI for THAT SAME statistic should usually be centered somewhere around it.

A CI completely missing the point estimate often means:
- wrong statistic
- wrong aggregation
- wrong resampling unit
- mismatched artifacts
- or a pipeline bug

---

7/  
This changed how I think about benchmark reporting.

A p-value alone is not enough.

You must be able to explain:
- what was resampled
- what statistic was estimated
- what uncertainty actually means
- and what claims the benchmark legitimately supports

---

8/  
The biggest insight for me:

Bootstrap is not magic.

It only answers the question encoded in the resampling procedure.

If the resampling logic is wrong, the confidence interval becomes misleading — even if the code runs perfectly.

---

9/  
This also changed how I think about model cards.

A scaffolded benchmark result is NOT automatically a deployment recommendation.

A statistically detectable lift on 42 held-out tasks is evidence…

…but not the same thing as proving production reliability.

#AI #LLM #Evaluation #MachineLearning #Statistics #AIEngineering