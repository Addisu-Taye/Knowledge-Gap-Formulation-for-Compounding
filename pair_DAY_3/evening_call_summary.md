# Evening Call Summary

The final discussion clarified that prompting, instruction tuning, and pretraining operate at fundamentally different layers of the model lifecycle.

The biggest conceptual shift during the review was understanding that prompting does not inject new knowledge into the model. Instead, prompts change the inference-time conditioning context, which shifts the probability distribution over possible continuations and activates different latent behaviors already present inside the pretrained network.

One especially important clarification was separating:
- capability acquisition,
from:
- capability activation.

We concluded that:
- pretraining builds the underlying capability space,
- post-training reshapes behavioral preferences and controllability,
- and prompting selects which behaviors become active during inference.

This helped explain why structured prompts in the Week 10 Conversion Engine could dramatically improve:
- formatting consistency,
- reasoning style,
- and agent behavior,

without changing model weights at all.

The review also clarified why prompting can feel like “new learning.” The model may already contain latent representations for:
- reasoning traces,
- JSON formatting,
- assistant-style behavior,
- and tool-use patterns,

but prompting activates those behaviors more effectively under the current context.

Another important improvement in the final explanation was distinguishing:
- true parameter updates during training,
from:
- temporary behavioral steering during inference.

This reframed prompt engineering from:
> “teaching the model”

toward:
> “steering and activating latent capability patterns.”

The final explainer now ties the observed Week 10 system behavior directly to:
- next-token prediction,
- context conditioning,
- in-context learning,
- and latent capability activation inside pretrained transformers.