# Morning Call Summary

The discussion started from an observation in the Week 10 Conversion Engine: structured prompts and instruction framing significantly improved formatting consistency and agent behavior even though the underlying model weights never changed.

The main question became:
- what capabilities are actually learned during pretraining,
- what changes during post-training,
- and why prompting alone can activate behaviors that appear newly learned.

The key insight from the discussion was that prompting does not usually create entirely new capabilities. Instead, prompting changes the inference-time context, which shifts the probability distribution over possible continuations and activates different latent behaviors already encoded inside the pretrained model.

We clarified the distinction between:
- pretraining as capability acquisition,
- post-training as behavioral shaping,
- and prompting as behavioral steering.

Another important point was separating:
- “learning” during gradient updates,
from:
- “activation” during inference.

This reframed prompt engineering from “teaching the model” toward “selecting and steering behaviors already present inside the network.”

The direction for the explainer became focused on:
- latent capability activation,
- in-context learning,
- instruction tuning,
- and why prompting can dramatically alter behavior without modifying parameters.