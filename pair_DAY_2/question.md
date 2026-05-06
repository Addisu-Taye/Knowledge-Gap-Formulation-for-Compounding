# Question

In my Week 10 Conversion Engine, the `ai_maturity.py` judgment module asks the LLM to return a structured JSON object inside normal natural-language generation. However, in 43.3% of tau2 evaluation runs, the model returned prose reasoning, malformed JSON, or empty objects instead, triggering the P24 failure and forcing the system to abstain.

The current system uses a parser and validator layer to recover fenced JSON and validate outputs, but it does not use native function-calling, structured outputs, or constrained decoding.

My gap is that I do not understand what actually changes inside inference when moving from prompt-only JSON generation to native structured-output systems.

At the token level, what is the mechanical difference between:
- prompting a model to “return valid JSON,”
- native `tool_use` / function-calling,
- and constrained decoding or structured-output APIs?

Is the model fundamentally doing the same next-token prediction in all cases, or do native structured-output systems actively restrict which tokens are allowed during decoding?

Would switching to native structured outputs likely have prevented the P24 failures, or would it mainly move the failure mode from invalid JSON generation to other failures such as incorrect tool selection, missing arguments, or schema mismatch?

Understanding this would help me explain the failure behavior in my evaluation pipeline and make more principled decisions about when prompt-only formatting is sufficient versus when constrained decoding is necessary.