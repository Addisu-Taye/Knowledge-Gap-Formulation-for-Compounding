# canonical_list.md

# Canonical List — Papers, Tools, and Engineering Patterns

## Purpose

This document summarizes the most valuable papers, tools, and engineering patterns I encountered during Week 12. These are the resources I believe are most worth other Forward-Deployed Engineers studying deeply.

---

# Papers

## 1. Attention Is All You Need

**Authors:** Vaswani et al. (2017)

### Why it matters

This paper provides the foundational transformer architecture underlying all modern LLM systems.

### Most useful insight

Everything ultimately reduces to:
- representation transformation,
- attention routing,
- and next-token prediction.

### Why FDEs should read it

Understanding attention changes how you reason about:
- context windows,
- KV caches,
- prompting,
- and inference cost.

---

## 2. Language Models are Few-Shot Learners

**Authors:** Brown et al. (2020)

### Why it matters

Introduced large-scale evidence for in-context learning and latent capability activation.

### Most useful insight

Prompting can activate capabilities without changing parameters.

### Why FDEs should read it

This paper reframes prompting as:
- behavioral steering,
not:
- lightweight retraining.

---

## 3. Training Language Models to Follow Instructions with Human Feedback

**Authors:** Ouyang et al. (2022)

### Why it matters

Explains RLHF and instruction tuning.

### Most useful insight

Pretraining and post-training optimize fundamentally different objectives.

### Why FDEs should read it

Essential for understanding:
- alignment,
- instruction following,
- and assistant-style behavior.

---

## 4. LoRA: Low-Rank Adaptation of Large Language Models

**Authors:** Hu et al. (2021)

### Why it matters

Introduced parameter-efficient fine-tuning.

### Most useful insight

Low-rank updates work because pretrained models already contain rich latent capability structure.

### Why FDEs should read it

Crucial for understanding:
- adaptation efficiency,
- representation steering,
- and fine-tuning tradeoffs.

---

## 5. QLoRA

**Authors:** Dettmers et al. (2023)

### Why it matters

Makes large-model fine-tuning practical under constrained hardware.

### Most useful insight

Quantization and low-rank adaptation can preserve quality surprisingly well.

### Why FDEs should read it

Highly practical for deployment-constrained environments.

---

## 6. Chain-of-Thought Prompting Elicits Reasoning

**Authors:** Wei et al. (2022)

### Why it matters

Demonstrates latent reasoning activation through prompting.

### Most useful insight

Prompting can reveal capabilities that appear absent under default decoding.

---

## 7. An Introduction to the Bootstrap

**Authors:** Efron & Tibshirani

### Why it matters

Foundational work for uncertainty estimation.

### Most useful insight

Bootstrap validity depends entirely on the resampling procedure.

### Why FDEs should read it

Critical for:
- benchmark evaluation,
- model-card defensibility,
- and uncertainty-aware reporting.

---

# Tools

## OpenAI Structured Outputs

### Why it matters

Provides schema-constrained generation.

### Key lesson

Structured generation reliability often requires decoder-level constraints rather than prompt-only formatting.

---

## Anthropic Tool Use

### Why it matters

Provides practical tool-calling infrastructure.

### Key lesson

Tool-use reliability depends heavily on schema design and behavioral conditioning.

---

## SciPy Bootstrap Utilities

### Why it matters

Practical statistical tooling for paired-bootstrap evaluation.

### Key lesson

Bootstrap pipelines must match the target statistic exactly.

---

## OpenRouter

### Why it matters

Useful experimentation layer for testing provider/model behavior differences.

### Key lesson

Inference infrastructure and runtime support significantly affect structured-output reliability.

---

# Engineering Patterns

## 1. Mechanism-Level Debugging

Move from:
- symptom observation,

Toward:
- mechanism explanation.

---

## 2. Structured Decoding Over Prompt-Only Formatting

Critical systems should not rely solely on unconstrained generation.

---

## 3. Paired Benchmark Evaluation

For task-level evaluations, paired resampling is often the correct uncertainty structure.

---

## 4. Behavioral Steering

Prompting,
LoRA,
and instruction tuning

often act by steering latent capability structure.

---

## 5. Honest Model-Card Reporting

Benchmark improvements are not deployment guarantees.

Statistical claims should always include:
- uncertainty assumptions,
- benchmark limitations,
- and evaluation conditions.

---

# Final Recommendation to the Cohort

The most important habit I would recommend to other FDEs is:

> Always ask what mechanism actually produced the observed behavior.

That single question repeatedly transformed shallow understanding into robust engineering insight during Week 12.