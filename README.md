
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

- Blog: (add Substack link after publishing)
- Thread: (add LinkedIn thread link after posting)

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
