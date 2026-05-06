# Evening Call Summary

The final explainer clarified that the repeated P24 failures were not mainly caused by weak parsing logic, but by unconstrained decoding during inference. The key distinction that emerged during the discussion was that prompt-only JSON generation does not restrict what tokens the model can emit. It only changes the probability distribution over possible continuations.

One important improvement during the evening review was grounding the explanation directly in the actual system implementation. Reviewing the prompt and parser code made the failure mode much clearer:
- the prompt instructed the model not to use markdown fencing,
- but the example below used fenced JSON,
- creating contradictory output-format signals.

We also clarified why thinking-model runs failed more often than Haiku runs. Thinking-style models are more likely to continue with explanatory reasoning before emitting machine-readable structure, especially when decoding is unconstrained.

The revised explainer more clearly distinguished:
- prompt-only JSON generation,
- native function calling,
- structured outputs,
- and constrained decoding.

The strongest insight added during revision was:
> prompting changes token probabilities, while constrained decoding changes the allowed token set itself.

This helped explain why native structured-output systems reduce malformed JSON failures while still allowing semantic-level failures such as incorrect arguments or schema mismatch.

The final version now ties the decoding mechanics directly to the observed P24 abstention behavior in the evaluation pipeline and explains why structured-output APIs provide a more reliable engineering surface than prompt-only formatting instructions.