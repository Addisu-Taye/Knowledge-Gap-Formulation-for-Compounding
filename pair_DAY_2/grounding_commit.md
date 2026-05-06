# Grounding Commit

I updated my understanding of the AI maturity judgment pipeline in the Week 10 Conversion Engine by correcting a key assumption about structured outputs and JSON reliability.

Previously, I treated the repeated P24 failures as mainly a parsing and formatting problem. I assumed the model was “trying to output JSON” and occasionally making syntax mistakes that could be recovered through parser logic and validation.

After investigating the actual decoding behavior, I now understand that the deeper issue is that prompt-only JSON generation is fundamentally unconstrained natural-language decoding. The instruction:

> “Respond with ONLY a JSON object”

does not mechanically restrict what the model can generate. It only shifts the probability distribution toward JSON-like continuations.

This means the model can still legally continue with prose reasoning such as:

```text
Based on the claims provided...
```

instead of beginning a structured JSON object.

I also identified a prompt-design issue in the current implementation:
- the instruction prohibited markdown fencing,
- but the example below demonstrated fenced JSON.

This introduced conflicting formatting signals that likely weakened structured-output reliability, especially in thinking-model runs.

The revised system understanding now distinguishes between:
- prompt-only JSON generation,
- native function calling,
- structured outputs,
- and constrained decoding.

The most important change is recognizing that constrained decoding and structured-output APIs do not merely “encourage” JSON formatting. They actively restrict or shape the allowed token set during inference.

This grounding change directly impacts future system design decisions:
- reducing reliance on prompt-only JSON generation,
- using schema-constrained outputs where possible,
- treating parsers as fallback validation layers rather than primary enforcement mechanisms,
- and analyzing failures at the decoding layer rather than only at the parsing layer.

This update transforms the P24 failure from a superficial “JSON formatting bug” into a deeper inference-time decoding and output-channel reliability problem.