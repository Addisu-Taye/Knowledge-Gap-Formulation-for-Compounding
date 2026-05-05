# Question

In my Week 10 Conversion Engine, the agent re-sends the same system prompt — Tenacious ICP definition, bench summary, and hiring signal brief — on every turn of a multi-turn loop. My `baseline.md` shows avg_agent_cost: $0.0199, but some tasks cost up to $0.0963 (task_4).

I claim the system prompt is "reused across turns," but I have never verified whether the LLM API is actually caching that repeated prefix or charging me full prefill cost every time.

How does KV cache work at the API call boundary — what conditions must be met for prefix caching to apply, and how does cache invalidation happen when the conversation context changes between turns?

Understanding this would allow me to correctly attribute cost in my agent, explain the observed variance across tasks, and improve prompt and memory design in my system.