# KV Cache, Prefix Caching, and Why Your Agent Keeps Paying for the Same Prompt

In my Week 10 Conversion Engine, I observed a surprising pattern:

- Average agent cost: **$0.0199**
- Some tasks: **$0.0963**
- Same reward outcome

At first, I assumed harder tasks simply cost more. But that explanation didn’t hold—these tasks weren’t meaningfully different in outcome quality.

Then I noticed something else:  
My agent re-sends the exact same system prompt on every turn of a multi-turn loop.

That raised a deeper question:

> *Am I actually reusing that prompt—or paying for it again every time?*

To answer this, I had to understand three things: **KV cache**, **prefix caching**, and what really happens at the **API boundary**.

---

## The Load-Bearing Mechanism: KV Cache

At the core of transformer inference is the **KV cache**.

When a model processes tokens, each token generates:
- a **Key (K)**
- a **Value (V)**

These are stored so the model can efficiently attend to previous tokens during generation.

Inference happens in two phases:

### 1. Prefill (processing the prompt)
- The entire input (system + history + user input) is processed
- KV cache is built
- This is parallel and compute-heavy

### 2. Decode (generating output)
- Tokens are generated one at a time
- KV cache is reused
- This is sequential and latency-sensitive

Recent systems work such as *PagedAttention* (Kwon et al., 2023) show how KV cache is managed efficiently within a single inference call, improving memory locality and throughput—but critically, this cache does not persist across independent API requests.

---

## The API Boundary: Where Reuse Breaks

KV cache only exists **within a single API call**.

In a multi-turn agent, each turn is a **new request**. That means:

- The KV cache is discarded after each call
- The model recomputes the entire prompt from scratch
- You are charged again for all input tokens

Even if your system prompt is identical across turns:

> **It is logically reused—but computationally recomputed every time.**

---

## Why Costs Explode in Multi-Turn Systems

Let’s look at how context grows:


Turn 1: S + U1
Turn 2: S + U1 + A1 + U2
Turn 3: S + U1 + A1 + U2 + A2 + U3


Each turn includes everything that came before.

This leads to:

> **Total tokens processed grows roughly O(n²)**

Where *n* is the number of turns.

This directly explains my observed cost variance:
- Short trajectories → low cost (~$0.0077)
- Long trajectories → high cost (~$0.0963)

The difference is not task difficulty—it is **context accumulation**.

---

## What Is Prefix Caching?

Some providers implement **prefix caching**, where identical prompt prefixes may reuse previously computed KV states.

Anthropic’s prompt caching documentation highlights that this reuse is possible—but only under strict conditions and is not guaranteed across all requests.

---

## When Prefix Caching Applies

Prefix caching only works if all of the following conditions are met:

1. **Byte-identical prefix starting at position 0**  
   The cached prefix must match exactly from the first token.  
   If any dynamic content appears before the system prompt (timestamps, IDs, metadata), caching fails entirely.

2. **Exact token match**  
   Even small formatting or whitespace differences break reuse.

3. **Same infrastructure path**  
   Requests must hit the same cache layer or server.

4. **Cache still valid (TTL)**  
   Cached prefixes expire quickly (seconds to minutes).

5. **Provider support**  
   Not all APIs expose or guarantee prefix caching.

> The most important constraint is that prefix matching must start from token 0—any dynamic prefix before your system prompt prevents caching entirely.

In my Conversion Engine, if any dynamic metadata is prepended before the system prompt, prefix caching would never apply—even if the system prompt itself is unchanged.

---

## Why Prefix Caching Doesn’t Save Multi-Turn Agents

In my system:

- System prompt → constant  
- Conversation history → growing  

So each request:
- shares a prefix
- but has a different suffix

As context grows, the reusable portion becomes smaller relative to total input.

> Prefix caching may reduce some cost—but it does not eliminate the dominant cost from accumulated context.

---

## Verifying KV Cache and Prefix Reuse in Practice

To validate what was happening in my system, I ran simple instrumentation experiments.

### Experiment: Measuring Token Usage and Latency

```python
import time
from openai import OpenAI

client = OpenAI()

prompt = "You are a helpful assistant. Explain KV cache in one sentence."

start_time = time.time()

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": prompt}],
)

end_time = time.time()

print("Prompt tokens:", response.usage.prompt_tokens)
print("Completion tokens:", response.usage.completion_tokens)
print("Total latency:", round(end_time - start_time, 3), "seconds")

Example Output
Prompt tokens: 18
Completion tokens: 22
Total latency: 0.82 seconds

Running the same prompt twice produced nearly identical latency, suggesting no observable prefix caching in this setup.

When increasing prompt size across turns:

prompt_tokens increased every turn
latency increased with prompt length

This confirms that prefill computation is repeated rather than reused.

What Changed in My Understanding

Before:

“The system prompt is reused across turns.”

Now:

The system prompt is recomputed and re-billed on every API call.

The cost spikes I observed are not anomalies—they are the natural result of:

growing context across turns
lack of persistent KV cache
limited and unreliable prefix caching
Closing Insight

LLM cost is not per task—it is per token processed.

Multi-turn agents don’t just generate tokens.
They repeatedly reprocess all prior tokens.

Unless you explicitly design around this, your system will:

get slower
get more expensive
and behave unpredictably at scale

##  References
Kwon et al., Efficient Memory Management for Large Language Model Serving with PagedAttention (SOSP 2023)
https://arxiv.org/abs/2309.06180
Anthropic, Prompt Caching Documentation
https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching

#