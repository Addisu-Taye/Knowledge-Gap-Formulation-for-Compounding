1/  
In 43.3% of my evaluation runs, my LLM ignored the instruction:

“Return ONLY valid JSON.”

Instead, it generated normal prose.

The parser failed.
The system abstained.
The pitch was never sent.

At first I thought this was a formatting bug.

It wasn’t 👇

---

2/  
The real issue was understanding what changes between:

• prompt-only JSON generation  
• native function calling  
• structured outputs  
• constrained decoding  

These are NOT the same thing internally.

---

3/  
An LLM does not “return JSON” like a normal program.

It only predicts the next token.

So when we prompt:

“Return valid JSON”

we are NOT restricting output.

We are only changing token probabilities.

---

4/  
That means these are BOTH legal continuations during decoding:

Desired:

```json
{
  "score": 7
}
```

Failure:

“Based on the claims provided...”

Under normal prompting, prose is still allowed.

---

5/  
This explains my P24 failures.

The model never committed to the structured output channel in the first place.

The parser was not weak.

The decoder was unconstrained.

---

6/  
Native function calling changes the interface.

The model is still predicting tokens —
BUT:
• tool schemas shape probabilities  
• APIs validate arguments  
• structure becomes enforced

The failure surface shifts from:
❌ malformed JSON

to:
⚠️ semantically wrong structured outputs

---

7/  
Constrained decoding goes even further.

Instead of:
“please output JSON”

the decoder literally blocks invalid tokens.

If the schema expects:

```json
{
  "score":
```

then prose tokens like:
“Based on...”

may become impossible to generate.

---

8/  
Big insight:

Prompting changes probabilities.

Constrained decoding changes the allowed token set itself.

That mechanical difference is why structured-output APIs are much more reliable than prompt-only JSON generation.

---

9/  
The P24 failure was not just a parser bug.

It exposed a deeper mismatch:

We asked a probabilistic text generator to behave like a typed function.

Once I understood that, the system behavior stopped looking random.