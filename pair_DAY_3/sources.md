# Sources

## Primary Papers

1. Brown et al. (2020) — *Language Models are Few-Shot Learners*  
https://arxiv.org/abs/2005.14165

→ Introduced strong evidence that large language models can perform in-context learning and activate latent capabilities through prompting without updating weights.

---

2. Ouyang et al. (2022) — *Training language models to follow instructions with human feedback*  
https://arxiv.org/abs/2203.02155

→ Explains instruction tuning and RLHF, including how post-training changes model behavior, compliance, and controllability.

---

3. Wei et al. (2022) — *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*  
https://arxiv.org/abs/2201.11903

→ Demonstrates that prompting can activate latent reasoning behaviors already present inside pretrained models.

---

## Supporting Concepts

### Pretraining

Core mechanism:
- next-token prediction

Objective:

```math
P(x_t \mid x_{<t})
```

Pretraining teaches:
- syntax
- semantics
- latent reasoning structure
- world knowledge
- token relationships
- representation geometry

---

### Post-Training / Instruction Tuning

Post-training reshapes:
- helpfulness
- alignment
- formatting behavior
- instruction following
- tool use
- conversational style

Key distinction:
- pretraining builds capability space
- post-training shapes preferred behaviors

---

### Prompting / Inference-Time Conditioning

Prompting changes:
- context conditioning
- token probabilities
- behavioral activation patterns

Key idea:
- prompting usually activates latent capabilities rather than creating entirely new ones.

---

## Week 10 System Observations

Observed in the Conversion Engine:
- structured prompts significantly improved formatting
- system-role framing changed agent behavior
- output consistency improved without weight updates
- reasoning style changed through prompt constraints alone

This motivated the question:
- what is actually learned during pretraining,
- versus what is activated during inference-time prompting?

---

## Adjacent Concepts Referenced

- in-context learning
- chain-of-thought prompting
- RLHF
- behavioral steering
- latent capability activation
- context conditioning

---

## Key Insight

The explainer is grounded in the distinction between:

1. pretraining:
   - learning broad latent capability space

2. post-training:
   - shaping preferred behaviors

3. prompting:
   - selecting and activating behaviors during inference