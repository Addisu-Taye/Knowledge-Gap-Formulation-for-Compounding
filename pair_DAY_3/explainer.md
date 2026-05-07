# What Pretraining Actually Learns vs What Prompting Activates

In my Week 10 Conversion Engine, I noticed something surprising: changing prompt constraints and structured instructions significantly improved output formatting, reasoning style, and agent behavior — even though the underlying model weights never changed.

At first this felt almost magical.

How can prompting alone create behaviors that look newly learned without modifying parameters? If the model was not retrained, where did those capabilities come from?

Answering that requires separating three different stages of modern LLM behavior:

1. pretraining,
2. post-training / instruction tuning,
3. inference-time prompting.

The key insight is:

> Prompting usually does not create new capabilities.  
> It activates, steers, or selects capabilities already latent inside the pretrained model.

---

# The Three Stages of Modern LLMs

Modern language models are usually built in layers:

| Stage | Purpose |
|---|---|
| Pretraining | Learn general representations and predictive structure |
| Post-training / instruction tuning | Shape behavior and alignment |
| Prompting | Select and activate behaviors at inference time |

Understanding the distinction between these stages explains why prompting can appear surprisingly powerful.

---

# What Pretraining Actually Learns

Pretraining is the largest and most expensive phase.

The model is trained on massive corpora:
- books,
- code,
- websites,
- conversations,
- documentation,
- scientific papers,
- and structured data.

The objective is simple:

:contentReference[oaicite:0]{index=0}

The model learns to predict the next token.

But this simple objective forces the network to internally learn:
- syntax,
- semantics,
- world knowledge,
- reasoning patterns,
- latent concepts,
- style distributions,
- and statistical relationships between ideas.

Importantly:
> pretraining creates the model’s raw capability reservoir.

This includes:
- mathematical reasoning,
- code generation,
- translation,
- classification,
- planning,
- latent tool-use behaviors,
- and even instruction-following tendencies.

The pretrained model already contains an enormous amount of compressed structure.

---

# Why Pretraining Produces Hidden Capabilities

This is the most important insight.

The model is not explicitly trained:
- “to become a classifier”
- “to become an assistant”
- “to follow JSON formatting rules.”

Instead, it learns broad predictive structure from huge amounts of text.

Because the internet already contains:
- explanations,
- instructions,
- question-answer pairs,
- structured formats,
- code,
- reasoning traces,

the model internalizes many latent behaviors during prediction training.

This means capabilities often exist:
> before they are explicitly activated.

The pretrained model may already know:
- how JSON usually looks,
- how assistants respond,
- how chain-of-thought appears,
- how API calls are formatted,
- how structured reasoning is written.

But these capabilities are not yet reliably controllable.

---

# What Post-Training Actually Changes

Post-training does NOT usually create intelligence from scratch.

Instead, it reshapes:
- behavior,
- response style,
- compliance,
- helpfulness,
- and controllability.

This phase often includes:
- supervised fine-tuning (SFT),
- reinforcement learning from human feedback (RLHF),
- preference optimization,
- constitutional alignment,
- tool-use fine-tuning.

The key difference is:

### Pretraining teaches:
> “What continuations are statistically plausible?”

### Post-training teaches:
> “Which continuations should be preferred?”

This is a behavioral layer.

For example:
- refusing unsafe outputs,
- following instructions,
- staying concise,
- returning JSON,
- calling tools correctly,
- answering in assistant style.

These are often post-training behaviors.

---

# Why Prompting Works Without Changing Weights

This is the central mystery.

If weights do not change during prompting:
> why does behavior change so dramatically?

Because prompting changes:
- the context,
- which changes token probabilities,
- which activates different latent circuits already stored in the network.

The model is always computing:

:contentReference[oaicite:1]{index=1}

The prompt becomes part of the context conditioning the prediction.

Different prompts activate different regions of behavior space.

---

# Prompting as Behavioral Steering

A useful mental model is:

> Prompting is not writing new knowledge into the model.  
> It is steering which internal behaviors become active.

For example:

```text
You are a strict JSON validator.
```

does not teach JSON.

Instead, it:
- biases the model toward patterns associated with:
  - strictness,
  - validation,
  - machine-readable formatting,
  - deterministic structure.

The latent capability already existed from pretraining.

The prompt activates it.

---

# Why Structured Instructions Matter So Much

Your Week 10 observation now makes sense.

When you changed:
- prompt constraints,
- formatting instructions,
- system-role framing,
- output examples,

you changed:
> the probability landscape of possible continuations.

Small prompt changes can strongly affect:
- reasoning style,
- verbosity,
- formatting,
- tool use,
- confidence,
- output structure.

Because they alter:
- which latent behaviors become most probable.

---

# Why Prompting Sometimes Looks Like “New Learning”

This is one of the most misleading aspects of LLMs.

A prompt can suddenly unlock:
- better reasoning,
- cleaner formatting,
- stronger planning,
- safer outputs,
- or tool use.

It feels like:
> the model learned something new.

But usually:
- the capability was already latent,
- the prompt merely activated it more effectively.

This is sometimes called:
- elicitation,
- capability activation,
- or behavioral steering.

---

# The Hidden Role of In-Context Learning

Prompting can also create temporary task adaptation without weight updates.

This is called:
> in-context learning.

For example:

```text
Input: cat → animal
Input: apple → fruit
Input: carrot →
```

The model infers:
> “I should continue the pattern.”

No weights change.

Instead:
- the transformer dynamically computes temporary behavior from context.

This is one reason prompting can feel surprisingly powerful.

The model behaves as if it “learned” the task during inference.

---

# Why Prompting Has Limits

Prompting can only activate capabilities already represented inside the model.

It cannot reliably create:
- knowledge absent from pretraining,
- reasoning circuits the model never learned,
- strong competence in fundamentally missing domains.

For example:
- a weak model cannot be prompted into becoming GPT-4-level,
- missing world knowledge cannot be invented through prompting alone.

Prompting is:
> capability selection,
not:
> capability creation.

---

# Connecting Back to Your Week 10 System

Your observation now becomes much clearer.

When structured prompts improved:
- formatting,
- agent behavior,
- reasoning quality,
- output consistency,

you were not adding new knowledge to the model.

You were:
- changing the inference-time conditioning,
- activating different latent behaviors,
- steering the model toward more structured continuations.

The model weights remained unchanged.

But the probability distribution over outputs changed dramatically.

That is why prompting alone can appear so powerful.

---

# Adjacent Concepts

## 1. Chain-of-Thought Prompting

Chain-of-thought prompts do not necessarily create reasoning ability.

They often:
- activate latent reasoning trajectories already present in the pretrained model.

---

## 2. Instruction Tuning

Instruction tuning improves:
- controllability,
- compliance,
- assistant-style behavior,
- task following.

It makes prompting more effective because the model learns to interpret instructions as behavioral steering signals.

---

## 3. RLHF

RLHF shapes:
- which responses are preferred,
- politeness,
- safety,
- helpfulness,
- formatting tendencies.

It strongly affects inference-time behavior even though the base reasoning capability may already exist from pretraining.

---

# Papers and Sources

## Primary Papers

Brown et al. (2020) — *Language Models are Few-Shot Learners*  
https://arxiv.org/abs/2005.14165

→ Introduced strong evidence for in-context learning and prompting-based capability activation.

---

Ouyang et al. (2022) — *Training language models to follow instructions with human feedback*  
https://arxiv.org/abs/2203.02155

→ Describes instruction tuning and RLHF.

---

Wei et al. (2022) — *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*  
https://arxiv.org/abs/2201.11903

→ Shows how prompting can activate latent reasoning behavior.

---

# Key Insight

The most important mental-model shift is this:

> Pretraining builds the capability space.  
> Post-training shapes preferred behavior.  
> Prompting selects which behaviors become active during inference.

Prompting feels powerful not because it changes the model’s weights —
but because modern pretrained models already contain far more latent capability than is immediately visible.