# Morning Call Summary

The initial discussion focused on repeated P24 failures in the AI maturity judgment pipeline, where the model returned prose or malformed JSON instead of the required structured object. The key clarification during the call was that this is not primarily a parsing problem but an inference and decoding problem.

We narrowed the question from general “JSON reliability” to the more specific mechanism-level distinction between:
- prompt-only JSON generation,
- native function calling,
- structured outputs,
- and constrained decoding.

The most important insight from the discussion was that prompt instructions like “return valid JSON” do not restrict what the model can generate. They only influence token probabilities. This means prose remains a legal continuation during decoding, which explains why the parser sometimes received no JSON object at all.

We also identified an important contradiction in the current prompt design:
- the instruction says “no markdown fencing”
- but the example demonstrates fenced JSON

This likely weakens the model’s confidence about the intended output format, especially in thinking-model runs where explanatory prose is more likely.

The final direction for the explainer was to focus on what mechanically changes during inference when moving from unconstrained text generation to schema-constrained or tool-based decoding.