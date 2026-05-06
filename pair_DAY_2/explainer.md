# Why Prompt-Instructed JSON Fails: Function Calling, Structured Outputs, and What Actually Changes During Inference

In your Week 10 Conversion Engine, the `ai_maturity.py` judgment module expects the model to return a machine-readable JSON object. But in 43.3% of tau2 evaluation runs, the model instead returned prose reasoning or malformed JSON, triggering the P24 failure and forcing the system to abstain.

At first glance, this looks like a formatting issue:

> “The model just failed to follow instructions.”

But the deeper issue is not formatting. It is about **how decoding works during inference** and what changes mechanically when you move from:

- prompt-only JSON generation,
- to native `tool_use`,
- to structured outputs,
- to constrained decoding.

Understanding that distinction explains why your parser failed, why thinking-model runs behaved differently from Haiku runs, and why native structured outputs reduce some failures while introducing different ones.

---

# The Question, Grounded in the System

Your prompt says:

> “Respond with ONLY a JSON object, no markdown fencing, no explanation.”

But the example immediately below uses a markdown code fence.

That creates a contradiction:
- the instruction says “do not use fencing”
- the demonstration shows fenced JSON

The parser then tries to compensate:

```python
def _extract_json(text: str) -> dict:
    fenced = re.search(r"```(?:json)?\s*\n?(.*?)\n?\s*```", text, re.DOTALL)
    raw = fenced.group(1) if fenced else text.strip()
    try:
        return json.loads(raw)
    except json.JSONDecodeError as e:
        raise AiMaturityParseError(...)
```

This works for:
- valid raw JSON
- fenced JSON

But it cannot recover from the actual P24 failure mode:

```text
Based on the claims provided, this company shows signs of early AI exploration...
```

There is no JSON object to parse at all.

That is the critical distinction:

> The failure happened before parsing.

The model never committed to the JSON output channel in the first place.

---

# The Load-Bearing Mechanism: The Model Only Predicts Tokens

The most important thing to understand is this:

> An LLM never “returns JSON.”

It only predicts the next token.

When the model outputs:

```json
{
  "score": 7
}
```

it is not internally executing a JSON serializer.

It is generating tokens one at a time because those tokens are statistically likely given:
- the prompt,
- the training distribution,
- prior tokens,
- and the decoding constraints.

This means that writing:

```text
Return ONLY valid JSON.
```

does not change the decoding algorithm.

It only changes token probabilities.

The model can still legally generate:

```text
Based on the claims provided...
```

because prose tokens remain available during decoding.

That is why prompt-only JSON generation is fragile:
- syntax correctness is probabilistic,
- not enforced.

---

# Why the Contradictory Example Matters

In LLM prompting, examples often carry more weight than abstract instructions because examples demonstrate the continuation pattern directly.

Your prompt effectively says:

1. “Do not use markdown fencing.”
2. “Here is an example using markdown fencing.”

So the model receives competing signals.

This matters even more for thinking-style models because they are often trained to:
- explain reasoning,
- deliberate before answering,
- produce verbose continuations.

Once the model begins generating explanatory prose, the probability of transitioning cleanly into strict JSON decreases rapidly.

Your parser can repair:
- fenced JSON,
- whitespace issues,
- formatting drift.

But it cannot recover from:
- unconstrained prose generation.

---

# Prompted JSON vs Native Structured Outputs

The mechanical differences become clearer when comparing the three approaches.

---

# 1. Prompt-Only JSON

This is your current setup.

You ask:

```text
Return ONLY valid JSON.
```

But decoding remains fully unconstrained.

At every step:
- all normal language tokens remain available,
- prose is still legal,
- explanations are still possible.

The model is relying entirely on learned instruction-following behavior.

This works surprisingly often because models have seen massive amounts of JSON during training.

But it is still:

> probabilistic formatting.

That means:
- malformed JSON,
- extra prose,
- partial objects,
- missing braces,
remain possible.

---

# 2. Native Tool Use / Function Calling

Native `tool_use` changes the interface around generation.

Instead of asking the model to manually emit JSON text, the API provides:
- tool schemas,
- argument definitions,
- structured action channels.

The model is still doing next-token prediction, but now:
- the probability distribution is shaped around valid tool-call structure,
- and many APIs validate arguments against schemas.

For example:

```json
{
  "tool": "score_company",
  "arguments": {
    "score": 7
  }
}
```

The model is no longer freely improvising output format in the same way.

This dramatically reduces syntax-level failures.

---

# 3. Constrained Decoding

Constrained decoding is stronger.

Instead of merely encouraging JSON, the decoder actively restricts which tokens are allowed at each step.

If the schema requires:

```json
{
  "score":
```

then prose tokens like:

```text
Based
```

may literally become impossible to generate.

This is mechanically different from prompt-only JSON generation.

Prompting changes probabilities.

Constrained decoding changes:

> the allowed token set itself.

That is why constrained decoding can guarantee:
- valid JSON syntax,
- schema conformity,
- structural correctness.

---

# Show It: Why Your P24 Failure Happened

Your system relied on unconstrained natural-language decoding to produce machine-readable structure.

During normal decoding, the model saw both of these continuations as plausible:

### Path A — desired

```json
{
  "score": 7,
  "confidence": 0.82
}
```

### Path B — actual failure

```text
Based on the claims provided, this company appears to...
```

In prompt-only JSON generation:
- both paths are legal,
- both are reachable,
- and thinking-style models are especially prone to Path B.

That explains why:
- Qwen3/thinking-model runs failed more often,
- Haiku runs remained more stable.

The issue was not that the parser was weak.

The issue was that:

> the model never committed to structured decoding in the first place.

---

# Would Native Structured Outputs Have Prevented P24?

For this specific failure mode:

> probably yes.

Native structured outputs or constrained decoding would likely prevent:
- prose instead of JSON,
- malformed syntax,
- empty-object formatting failures.

But they would not eliminate all failures.

Instead, the failure surface changes.

You move from:

```text
Invalid JSON text
```

to:

```text
Valid structure but incorrect semantics
```

For example:
- wrong tool selected,
- missing arguments,
- incorrect field values,
- semantically weak reasoning.

This is still preferable because:
- structured failures are easier to validate,
- parsers become simpler,
- downstream recovery is more reliable.

---

# What the System Should Change

The first fix is immediate:

> Remove the contradiction in the prompt.

If the instruction says:

```text
No markdown fencing
```

the example must not use markdown fencing.

The larger fix is architectural:
- stop relying on prompt-only JSON generation for production judgment modules.

A stronger design would use:
1. native structured outputs,
2. function calling with required schemas,
3. constrained decoding where available,
4. parser + validator as a fallback layer.

The parser should become:

> the last defense,
not the primary structure enforcement mechanism.

---

# Connecting the Dots

This question is not really about JSON formatting.

It is about:
- decoding constraints,
- output-channel control,
- and how much freedom the model has during generation.

Prompt-only JSON says:

> “Please choose the right tokens.”

Structured outputs say:

> “Only structurally valid tokens are allowed.”

That mechanical difference is why native structured-output systems reduce P24-style failures.

They do not make the model smarter.

They make the decoding process stricter.

---

# Pointers

## Papers / Sources

1. OpenAI — Structured Outputs Documentation  
https://platform.openai.com/docs/guides/structured-outputs

2. Anthropic — Tool Use Documentation  
https://docs.anthropic.com/en/docs/build-with-claude/tool-use

3. Geng et al., *Grammar-Constrained Decoding for Structured NLP Tasks*  
https://arxiv.org/abs/2305.13971

---

## Tool / Pattern Used

- Direct inspection of:
  - `agent/judgment/ai_maturity.py`
  - `agent/prompts/ai_maturity_rubric.md`

- Analysis of:
  - parser behavior,
  - fenced vs raw JSON handling,
  - P24 failure traces from tau2 evaluation runs.

---

# Closing Insight

The P24 failure was not just a parsing bug.

It exposed a deeper mismatch:

> the system expected deterministic machine-readable structure from an unconstrained probabilistic text generator.

Once you understand that, the behavior stops looking random.

It becomes an inference-time design decision.