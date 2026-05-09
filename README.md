
# Knowledge-Gap-Formulation-for-Compounding

This repository contains my Week 12 work focused on researching, explaining, and grounding mechanism-level knowledge gaps in AI systems and agent engineering.

The goal of this week is not only to answer technical questions, but to:
- identify real system-level gaps,
- investigate them deeply,
- connect them to actual implementation behavior,
- and publish public technical artifacts grounded in engineering evidence.

---

# Repository Structure

Each daily folder contains:
- `question.md`
- `morning_call_summary.md`
- `explainer.md`
- `thread.md`
- `evening_call_summary.md`
- `signoff.md`
- `grounding_commit.md`
- `sources.md`

---

# Day 1 Focus

## Topic
KV cache mechanics, prefix caching, and inference cost accumulation in multi-turn agents.

## Key Insight
LLM cost is not per task — it is per token processed. Multi-turn systems repeatedly reprocess context unless explicitly optimized.

### Public Artifact
- Blog: https://open.substack.com/pub/addistaye/p/kv-cache-prefix-caching-and-why-your?r=6am66u&utm_campaign=post&utm_medium=web&showWelcomeOnShare=true

LinkedIn Thread: https://www.linkedin.com/posts/addisu-taye_kv-cache-prefix-caching-and-why-your-agent-share-7457173499602927616-_yu1?utm_source=share&utm_medium=member_desktop&rcm=ACoAAA6NaS4BMXIbB7-C664MF80rOndMGtMofc8

---

# Day 2 Focus

## Topic
Prompt-only JSON generation vs native structured outputs, tool use, and constrained decoding.

## Key Insight
Prompting changes token probabilities. Constrained decoding changes the allowed token set itself.

### Public Artifact
- Blog:https://open.substack.com/pub/addistaye/p/why-please-return-json-fails-function?r=6am66u&utm_campaign=post&utm_medium=web&showWelcomeOnShare=true

- LinkedIn Thread: https://www.linkedin.com/posts/addisu-taye_why-please-return-json-fails-function-share-7457792180317822977-NOBy?utm_source=share&utm_medium=member_desktop&rcm=ACoAAA6NaS4BMXIbB7-C664MF80rOndMGtMofc8

---

# Themes Explored

- KV cache mechanics
- Prefix caching
- Multi-turn inference cost
- Function calling
- Structured outputs
- Constrained decoding
- Tool-use reliability
- Agent failure surfaces
- Decoding behavior during inference

---

# Core Learning Goal

Move from:
> “The model failed unexpectedly.”

to:
> “I can explain the exact inference-time mechanism that produced the failure.”

---

# Public Writing

This repository also serves as a public technical-writing portfolio documenting:
- system failures,
- debugging process,
- mechanism-level explanations,
- and engineering tradeoffs in modern AI systems.



# Day 3 Focus

## Topic
Training and post-training mechanics.

## Subtopic
What pretraining actually learns vs what prompting activates.

## Key Insight
Prompting usually does not create new capabilities. It activates and steers latent capabilities already learned during pretraining by changing inference-time context conditioning.

## Public Artifacts

- Blog: https://open.substack.com/pub/addistaye/p/what-pretraining-actually-learns?r=6am66u&utm_campaign=post&utm_medium=web&showWelcomeOnShare=true

- Thread: https://www.linkedin.com/posts/addisu-taye_ai-llm-machinelearning-share-7458155080102371329-5imN?utm_source=share&utm_medium=member_desktop&rcm=ACoAAA6NaS4BMXIbB7-C664MF80rOndMGtMofc8

---

## Concepts Explored

- Pretraining
- Instruction tuning
- RLHF
- In-context learning
- Latent capability activation
- Behavioral steering
- Prompt conditioning
- Next-token prediction

# Day 4 Focus

## Topic
Evaluation and statistics.

## Subtopic
Bootstrap confidence intervals for agent benchmarks and statistical defensibility.

## Key Insight
Bootstrap methods are only meaningful if the resampling procedure matches the statistic being estimated. A confidence interval is not automatically trustworthy simply because it was produced by a bootstrap pipeline.

## Public Artifacts

- Blog: https://open.substack.com/pub/addistaye/p/why-my-benchmark-confidence-interval?r=6am66u&utm_campaign=post&utm_medium=web&showWelcomeOnShare=true


- Thread: https://www.linkedin.com/posts/addisu-taye_ai-llm-machinelearning-share-7458474229689180160-pT1T?utm_source=share&utm_medium=member_desktop&rcm=ACoAAA6NaS4BMXIbB7-C664MF80rOndMGtMofc8

---

## Concepts Explored

- paired bootstrap
- confidence intervals
- benchmark uncertainty
- statistical defensibility
- task-level resampling
- p-value interpretation
- scaffolded evaluation
- model-card claims

---

# Day 5 Focus

## Topic
Training and post-training mechanics.

## Subtopic
What LoRA actually adapts; why low rank works; what changes at higher rank.

## Key Insight
LoRA works because pretrained transformers already contain rich latent capability structure. Low-rank adapters steer existing representation geometry rather than creating entirely new reasoning systems.

## Public Artifacts

- Blog: (add Substack link)
- Thread: (add LinkedIn thread link)

---

## Concepts Explored

- LoRA
- QLoRA
- Low-rank adaptation
- Representation geometry
- Fine-tuning efficiency
- Attention projection updates
- Adaptation subspaces
- Rank vs expressive capacity

---

# Final Submission Artifacts

## Root-Level Documents

- `synthesis.md`
- `canonical_list.md`
- `portfolio_update.md`

---

# Evidence-Graph Integrity

Every:
- blog post,
- LinkedIn thread,
- grounding commit,
- explainer,
- and synthesis claim

is grounded in at least one of:
- canonical papers,
- authoritative documentation,
- benchmark artifacts,
- implementation evidence,
- runnable code,
- or partner sign-off.

This repository is designed to make every major technical claim traceable and reproducible.

---

# Canonical Sources Referenced Across the Week

## Papers

- Vaswani et al. — *Attention Is All You Need*
- Brown et al. — *Language Models are Few-Shot Learners*
- Ouyang et al. — *Training Language Models to Follow Instructions with Human Feedback*
- Wei et al. — *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*
- Hu et al. — *LoRA: Low-Rank Adaptation of Large Language Models*
- Dettmers et al. — *QLoRA*
- Efron & Tibshirani — *An Introduction to the Bootstrap*

---

## Documentation and Tools

- Anthropic Tool Use Documentation
- Anthropic Prefix Caching Documentation
- OpenAI Structured Outputs Documentation
- SciPy Bootstrap Documentation
- OpenRouter experimentation workflows

---

# Final Reflection

The cumulative trajectory across Weeks 10, 11, and 12 moved from:
- building AI systems,

to:
- building,
- evaluating,
- auditing,
- explaining,
- and defending

AI systems at mechanism level.

The biggest outcome of Week 12 was developing:
> mechanism-level reasoning discipline.

That shift fundamentally changed how I approach:
- prompting,
- inference,
- evaluation,
- adaptation,
- and production reliability in modern AI systems.