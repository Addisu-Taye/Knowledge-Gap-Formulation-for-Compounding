# KV Cache, Prefix Caching, and Why Your Agent Keeps Paying for the Same Prompt

In my Week 10 Conversion Engine, I observed a surprising pattern: tasks with the same reward outcome had drastically different costs. My `baseline.md` reports an average agent cost of **$0.0199**, but some tasks spike as high as **$0.0963**.

At the same time, my agent re-sends the same system prompt—Tenacious ICP definition, bench summary, and hiring signal brief—on every turn of a multi-turn loop.

I had been assuming that this repeated prefix was “reused” by the model. But I had never verified whether the API actually caches that prefix—or whether I’m paying the full cost every single time.

This post unpacks the mechanism behind that gap: **KV cache**, **prefix caching**, and **cache invalidation**.

---

## The Load-Bearing Mechanism: KV Cache

Transformer inference relies on the **KV cache**.

Each token produces:
- **Keys (K)**
- **Values (V)**

These are stored so the model can reuse them during generation instead of recomputing attention over all previous tokens.

### Two phases of inference

**1. Prefill phase**
- Processes the entire prompt (system + history + user input)
- Builds the KV cache
- Parallel computation

**2. Decode phase**
- Generates tokens one-by-one
- Reuses KV cache
- Sequential and latency-sensitive

> KV cache makes decoding efficient—but only within a single API call.

---

## The API Boundary: Where the Assumption Breaks

In a multi-turn agent, each turn is a **new API call**.

This means:
- KV cache is **not preserved**
- The model **recomputes the full prompt**
- You are **charged again for all input tokens**

Even if your system prompt is identical across turns:

> It is logically reused—but computationally recomputed every time.

---

## Why Costs Grow Faster Than Expected

Across turns, your prompt evolves like this:
Turn 1: S + U1
Turn 2: S + U1 + A1 + U2
Turn 3: S + U1 + A1 + U2 + A2 + U3

Each turn includes the full prior history.

This leads to:
Total tokens processed ≈ O(n²)

Where:
- `n` = number of turns

This explains:
- Avg cost ≈ $0.0199
- Spike cost ≈ $0.0963

The difference is not task difficulty—it is **context growth**.

---

## What Is Prefix Caching?

Some providers implement **prefix caching**:

If the beginning of a prompt is identical, the system may:
- reuse previous computation
- skip part of prefill

This sounds like it should help—but in practice, it is limited.

---

## When Prefix Caching Applies

Prefix caching only works if:

1. **Exact token match**
   - Even whitespace differences break it

2. **Same model + routing**
   - Requests must hit the same infrastructure

3. **Cache still alive**
   - Typically short-lived (seconds–minutes)

4. **Provider supports it**
   - Not guaranteed or visible

---

## Why It Doesn’t Save Multi-Turn Agents

In your system:

- System prompt → constant ✅  
- Conversation history → growing ❌  

So each request:
- shares a prefix
- but has a different suffix

As context grows, the reusable portion becomes a smaller fraction of total work.

> Prefix caching may reduce some cost—but not the dominant cost from accumulated context.

---

## Cache Invalidation: Why Reuse Fails

Caching breaks when:

- Any token in the prefix changes
- Messages are reordered or truncated
- Requests hit different servers
- Cache expires

Even small changes can invalidate reuse entirely.

---

## How to Verify This in Your System

### 1. Track token usage

Log per request:
- `prompt_tokens`
- `completion_tokens`

Compute:

prefill_cost = prompt_tokens × input_price
decode_cost = completion_tokens × output_price

If `prompt_tokens` increases each turn → you are re-paying prefill.

---

### 2. Measure time-to-first-token

Log:
- request start time
- first token received

Approximation:
prefill_latency ≈ time_to_first_token
decode_latency ≈ total_time - prefill_latency

If prefill latency grows with prompt size → no effective caching.

---

### 3. Run identical prompt test

Send the exact same prompt twice:

- If latency drops → caching likely applied
- If not → no reuse

---

## What This Means for My Agent

My original assumption:

> “The system prompt is reused across turns.”

Actual mechanism:

> Each turn recomputes the full prompt, and KV cache is not preserved across API calls.

The cost variance I observed is driven by:
- growing context
- repeated prefill computation
- limited impact of prefix caching

---

## Closing Insight

> **LLM cost is not per task—it is per token processed.**

Multi-turn agents don’t just generate tokens—they repeatedly **reprocess all prior tokens**.

Unless you explicitly design around this, you will pay for the same context again and again.
