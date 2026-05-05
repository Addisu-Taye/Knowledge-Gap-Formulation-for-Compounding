# Sources

## Canonical Papers

1. Kwon et al., *Efficient Memory Management for Large Language Model Serving with PagedAttention* (SOSP 2023)  
   https://arxiv.org/abs/2309.06180  
   → Explains KV cache structure, lifecycle, and how it is created during prefill and reused during decode within a single inference call.

2. Pope et al., *Efficiently Scaling Transformer Inference* (MLSys 2023)  
   https://arxiv.org/abs/2211.05102  
   → Provides the hardware-level analysis showing that prefill is compute-bound while decode is memory-bandwidth-bound, explaining why KV cache reuse matters for performance.

---

## Documentation / Primary Sources

3. Anthropic, *Prompt Caching Documentation*  
   https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching  
   → Defines prefix caching behavior, including the requirement for byte-identical prefixes and conditions under which caching applies or fails.

---

## Tool / Experiment

- Direct analysis of `trace_log.jsonl` from Week 10 Conversion Engine:
  - grouped runs by `task_id` to measure cost variance
  - compared cost vs duration to identify inference vs waiting time
  - instrumented token usage and latency to distinguish prefill vs decode behavior

- Minimal API experiment:
  - logged `prompt_tokens` and `completion_tokens`
  - measured time-to-first-token (TTFT)
  - repeated identical prompts to test prefix caching behavior

These experiments were used to validate that:
- KV cache does not persist across API calls  
- prefill computation is repeated across turns  
- prefix caching is not reliably applied in the multi-turn loop