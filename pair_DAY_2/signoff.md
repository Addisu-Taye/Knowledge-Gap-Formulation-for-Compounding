# Sign-Off

Status: Closed

Before this explainer, I treated prompt-only JSON generation as mostly a formatting reliability problem. I assumed that if the instructions were strong enough and the parser was robust enough, the model would reliably produce machine-readable outputs.

I now understand that the deeper issue is decoding behavior during inference. Prompt instructions such as “return ONLY valid JSON” do not restrict what the model can generate. They only shift token probabilities. Under unconstrained decoding, prose reasoning remains a valid continuation, which explains why the model sometimes produced explanatory text instead of a JSON object.

I also understand the mechanical difference between:
- prompt-only JSON generation,
- native function calling,
- structured outputs,
- and constrained decoding.

All of them still rely on next-token prediction, but they differ in how much the decoder constrains or restricts the available token set during generation.

The most important insight I gained is:

> Prompting changes probabilities. Constrained decoding changes the allowed token set itself.

This directly explains why my P24 failures occurred. The parser failed because the model never committed to the structured-output channel in the first place. The issue was not only malformed JSON — it was unconstrained natural-language generation.

I also now understand why thinking-model runs failed more often than Haiku runs. Thinking-style models are more likely to continue visible reasoning during unconstrained decoding, especially when the prompt contains mixed signals such as contradictory formatting examples.

Going forward, I would not rely on prompt-only JSON generation for production judgment modules. I would instead use:
- native structured outputs,
- function-calling with schemas,
- or constrained decoding where available,
combined with validation and retry layers.

I can now explain the P24 failure as an inference-time design issue rather than a simple parsing bug.