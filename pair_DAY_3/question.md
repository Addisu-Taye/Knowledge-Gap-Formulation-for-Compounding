# Question

In my Week 10 Conversion Engine, changing prompt constraints and structured instructions significantly improved output formatting and agent behavior without changing the underlying model weights. I realized I cannot explain the relationship between pretraining, instruction tuning, and inference-time prompting.

Specifically, what capabilities are actually learned during pretraining versus post-training, and why can prompting alone activate behaviors that appear newly learned without modifying model parameters?

I want to understand whether prompting is creating new capabilities, selecting latent capabilities already learned during pretraining, or temporarily steering the model into different behavioral modes through context conditioning.

Understanding this would help me reason more clearly about why structured prompts can dramatically improve behavior in my system without any fine-tuning, and where the actual boundary exists between learned capability and inference-time activation.