# portfolio_update.md

# Portfolio Update — Week 12 Grounding Commit Synthesis

## Candidate: Addisu Taye

This Week 12 work significantly strengthened the technical depth and defensibility of my Weeks 10 and 11 portfolio projects.

The core improvement was not simply producing additional artifacts. It was upgrading my reasoning from:
- surface-level system operation,

to:
- mechanism-level understanding of model behavior, inference systems, evaluation pipelines, and adaptation methods.

Across five grounding commits, I corrected major misconceptions about:
- inference cost,
- structured generation,
- prompting,
- statistical uncertainty,
- and parameter-efficient fine-tuning.

These corrections materially improved how I would design, evaluate, and defend AI systems in production-facing environments.

---

# 1. Inference Cost and KV Cache Mechanics

## Previous understanding

I assumed repeated prompts in multi-turn systems were effectively “reused” across turns.

## Updated understanding

I now understand that:
- KV cache persistence is generally scoped to a single inference call,
- repeated context often incurs repeated prefill cost,
- and prefix caching only applies under strict conditions.

## Portfolio impact

This improved my ability to:
- diagnose latency spikes,
- reason about inference cost accumulation,
- and optimize multi-turn agent systems.

---

# 2. Structured Outputs and Constrained Decoding

## Previous understanding

I treated malformed JSON primarily as a parsing problem.

## Updated understanding

I now understand that:
- prompt-only JSON generation is unconstrained decoding,
- structured outputs change decoder behavior,
- and constrained decoding restricts allowed token trajectories.

## Portfolio impact

This changed how I design:
- tool-calling systems,
- structured-output pipelines,
- and production reliability safeguards.

---

# 3. Prompting and Latent Capability Activation

## Previous understanding

Prompting felt like temporary teaching.

## Updated understanding

I now understand that:
- prompting activates latent behaviors already encoded during pretraining,
- post-training shapes behavioral preference,
- and prompting steers inference-time trajectories.

## Portfolio impact

This improved my ability to:
- design effective prompts,
- distinguish capability limitations from conditioning failures,
- and reason about agent behavior systematically.

---

# 4. Statistical Defensibility and Bootstrap Evaluation

## Previous understanding

I treated bootstrap outputs as automatically trustworthy statistical evidence.

## Updated understanding

I now understand that:
- bootstrap validity depends entirely on the resampling procedure,
- paired evaluations require paired resampling,
- and statistical artifacts can become misleading if the uncertainty structure is wrong.

## Portfolio impact

This improved my ability to:
- audit benchmark pipelines,
- interpret uncertainty carefully,
- and write defensible model-card recommendations.

---

# 5. LoRA and Representation Steering

## Previous understanding

I did not fully understand why low-rank adapters could strongly affect behavior.

## Updated understanding

I now understand that:
- pretrained models already contain rich latent structure,
- LoRA steers representation geometry,
- and higher rank increases adaptation dimensionality while weakening compression constraints.

## Portfolio impact

This improved my ability to:
- reason about fine-tuning tradeoffs,
- choose adapter configurations more intentionally,
- and interpret adaptation behavior mechanistically.

---

# Overall Portfolio Improvement

The combined effect of these grounding commits is a substantial increase in:
- technical rigor,
- inference-level understanding,
- evaluation defensibility,
- and systems reasoning maturity.

The projects from Weeks 10 and 11 are now supported not only by implementation artifacts, but by:
- deeper statistical understanding,
- clearer model-behavior reasoning,
- and stronger deployment-awareness.

Most importantly, Week 12 changed how I approach AI engineering problems.

I now consistently ask:

> What mechanism actually produced this behavior?

That shift has made my debugging, evaluation, and system-design process significantly more rigorous.

---

# Forward-Deployed Engineering Relevance

The strongest outcome of Week 12 was developing the ability to:
- audit AI system behavior mechanistically,
- distinguish benchmark evidence from deployment evidence,
- and reason about model behavior across training, prompting, inference, and evaluation layers.

Those skills directly generalize to Forward-Deployed Engineering work where:
- systems are probabilistic,
- evaluation slices are small,
- infrastructure constraints matter,
- and production claims require careful uncertainty reasoning.

Week 12 transformed my portfolio from:
- “I built AI systems,”

to:
- “I can explain, audit, and defend how those systems behave.”