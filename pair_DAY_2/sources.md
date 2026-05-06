# Sources

## Canonical Papers / Documentation

1. OpenAI — Structured Outputs Documentation  
https://platform.openai.com/docs/guides/structured-outputs

→ Explains how structured-output APIs and schema-constrained generation work, including how decoding can be restricted to valid output structures.

---

2. Anthropic — Tool Use Documentation  
https://docs.anthropic.com/en/docs/build-with-claude/tool-use

→ Describes native tool use, function-calling behavior, and how models interact with structured tool schemas during inference.

---

3. Geng et al., *Grammar-Constrained Decoding for Structured NLP Tasks*  
https://arxiv.org/abs/2305.13971

→ Explains constrained decoding and how restricting the valid token set during generation improves structured-output reliability.

---

## System Artifacts Analyzed

### Prompt

`agent/prompts/ai_maturity_rubric.md`

Key instruction:
> “Respond with ONLY a JSON object, no markdown fencing, no explanation.”

Observation:
- The instruction prohibited markdown fencing
- The example below used fenced JSON
- This created conflicting output-format signals

---

### Parser / Validator

`agent/judgment/ai_maturity.py`

Analyzed:
- fenced JSON extraction
- fallback raw parsing
- JSONDecodeError handling
- abstention behavior after parsing failure

Core mechanism:

```python
def _extract_json(text: str) -> dict:
    fenced = re.search(r"```(?:json)?\s*\n?(.*?)\n?\s*```", text, re.DOTALL)
    raw = fenced.group(1) if fenced else text.strip()
    return json.loads(raw)
```

---

## Failure Traces Examined

Observed P24 failure mode:

```text
Based on the claims provided, this company shows signs of early AI exploration...
```

instead of:

```json
{
  "score": 7
}
```

Key observation:
- Failure occurred before parsing
- The model never committed to structured output generation

---

## Experiments / Reasoning Used

- Compared:
  - prompt-only JSON generation
  - native function calling
  - structured outputs
  - constrained decoding

- Analyzed:
  - token-level next-token prediction behavior
  - schema-constrained decoding
  - why prose remained a valid continuation under unconstrained decoding

- Compared behavior across:
  - Qwen3 thinking-model runs
  - Haiku runs

Observation:
- Thinking-style models were more likely to leak reasoning into visible output instead of emitting strict machine-readable JSON.