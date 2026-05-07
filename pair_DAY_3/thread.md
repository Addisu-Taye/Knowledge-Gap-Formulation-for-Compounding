1/  
One of the strangest things I noticed in my Week 10 system:

Changing only the prompt dramatically improved:
- formatting
- reasoning quality
- agent behavior

…but the model weights never changed.

So what was actually happening? 👇

---

2/  
At first this feels almost magical.

How can prompting alone unlock behaviors that look newly learned?

The answer starts with separating 3 different stages:

• pretraining  
• post-training  
• prompting

Most people blur them together.

---

3/  
Pretraining is where the model learns its raw capability space.

The objective is simple:

Predict the next token.

But to do that well, the model internalizes:
- syntax
- semantics
- reasoning patterns
- world knowledge
- latent concepts
- formatting structure

---

4/  
This means the pretrained model already contains many hidden capabilities.

It may already know:
- how JSON usually looks
- how assistants respond
- how chain-of-thought reasoning appears
- how APIs are formatted

…but those behaviors are not yet reliably controllable.

---

5/  
Post-training changes something different.

It usually does NOT create intelligence from scratch.

Instead it reshapes:
- helpfulness
- instruction following
- formatting behavior
- alignment
- tool-use tendencies

Pretraining teaches:
“What continuations are plausible?”

Post-training teaches:
“Which continuations should be preferred?”

---

6/  
Then comes prompting.

This was the biggest insight for me:

Prompting usually does NOT create new capabilities.

It activates or steers capabilities already latent inside the model.

---

7/  
The model is always computing:

P(next token | context)

So when you change the prompt, you change:
- the context
- the probability landscape
- which internal behaviors become most likely

That’s why tiny prompt edits can massively change outputs.

---

8/  
This explains why structured prompts improved my Week 10 agent behavior without changing weights.

I wasn’t “teaching” the model new knowledge.

I was:
- steering it
- activating different latent behaviors
- biasing the decoder toward more structured continuations

---

9/  
One of the most misleading things about LLMs:

Prompting can feel like new learning.

But often the capability was already there.

The prompt simply activated it more effectively.

That’s why prompting feels so powerful.

---

10/  
The key mental-model shift:

Pretraining builds the capability space.

Post-training shapes preferred behavior.

Prompting selects which behaviors become active during inference.