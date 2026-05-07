# Grounding Commit

I updated my understanding of prompting and instruction-following behavior in the Week 10 Conversion Engine by correcting a major misconception about what prompting actually does inside a language model.

Previously, I implicitly treated prompting as a lightweight form of “temporary teaching.” When structured prompts significantly improved:
- formatting consistency,
- reasoning style,
- and agent behavior,

I assumed the model was somehow learning new behavior during inference even though no weights were changing.

After investigating the relationship between:
- pretraining,
- post-training,
- and prompting,

I now understand that prompting does not usually create new capabilities. Instead, prompting changes the inference-time conditioning context, which shifts the probability distribution over continuations and activates latent behaviors already encoded inside the pretrained network.

This corrected a major misunderstanding in how I interpreted prompt engineering. The observed behavioral improvements in my system were not evidence of new learning. They were evidence that:
- the pretrained model already contained the relevant latent capabilities,
- and the structured prompts were steering the model toward activating those behaviors more reliably.

I also clarified the distinction between:
- capability acquisition during training,
and:
- capability activation during inference.

This grounding change directly affects future engineering decisions in my systems:
- treating prompts as behavioral steering mechanisms rather than “teaching tools,”
- designing prompts to activate latent structures more intentionally,
- separating true model limitations from poor inference-time conditioning,
- and understanding that prompt quality strongly affects which behaviors become dominant during decoding.

This update transforms my understanding of prompt engineering from:
> “adding behavior”

to:
> “selecting and activating behaviors already latent inside pretrained transformers.”