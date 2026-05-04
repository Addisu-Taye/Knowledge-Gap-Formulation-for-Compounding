1/  
Why did one task in my LLM agent cost **$0.096** while another cost **$0.007**…  
even though both achieved the same result?

I thought it was “task difficulty.”

It wasn’t.

It was KV cache and how inference actually works 👇

---

2/  
Every LLM call has 2 phases:

• Prefill → process ALL input tokens  
• Decode → generate tokens one-by-one  

Cost:
- Prefill ∝ input tokens  
- Decode ∝ output tokens  

But here’s the catch most people miss…

---

3/  
In a multi-turn agent, you resend the FULL context every turn:

Turn 1: S + U1  
Turn 2: S + U1 + A1 + U2  
Turn 3: S + U1 + A1 + U2 + A2 + U3  

👉 Total tokens grow ~ O(n²)

You’re not just generating—you’re reprocessing history.

---

4/  
“But KV cache should reuse that, right?”

Yes—but only **inside a single API call**.

Across calls:
❌ KV cache is lost  
❌ Prompt is recomputed  
❌ You pay again  

Your system prompt is NOT free.

---

5/  
What about prefix caching?

It *can* help—but only if:

• prefix is **byte-identical from token 0**  
• same server handles request  
• cache hasn’t expired  

If anything dynamic appears before your system prompt → caching breaks completely.

---

6/  
I tested this:

• Logged prompt_tokens → grew every turn  
• Measured latency → increased with prompt size  
• Repeated identical prompts → no latency drop  

👉 No meaningful prefix reuse in practice

---

7/  
This explains everything:

Task_4 ($0.096) → long trajectory → large accumulated context  
Task_73 ($0.007) → short trajectory → small context  

Same outcome.  
Completely different token footprint.

---

8/  
The key insight:

**LLM cost is not per task—it is per token processed.**

Multi-turn agents don’t just generate tokens.

They repeatedly **reprocess all prior tokens**.

---

9/  
Once you see this, system design changes:

• compress prompts  
• limit history  
• rethink multi-turn loops  
• measure token growth explicitly  

Because you’re not paying for intelligence…

You’re paying for repetition.