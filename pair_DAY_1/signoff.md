# Sign-Off

Status: Closed

Before this explainer, I believed that my system prompt was effectively “reused” across turns and therefore not a major contributor to cost. I now understand that while the prompt is logically reused, it is computationally recomputed on every API call because KV cache does not persist across API boundaries.

I also learned that prefix caching does not reliably apply in my system. It requires a byte-identical prefix starting from token position 0, along with stable routing and short cache lifetimes. In a multi-turn agent where conversation history grows and may include dynamic elements, these conditions are rarely satisfied. As a result, my system re-pays prefill cost on every turn.

This directly explains the cost variance I observed: tasks with longer trajectories (e.g., task_4 at $0.0963) accumulate more context and therefore incur repeated prefill computation, while shorter trajectories (e.g., task_73 at $0.0077) remain cheap. The difference is driven by token growth across turns, not task difficulty.

I can now precisely attribute cost in my system to repeated prefill and growing context, and I understand how KV cache scope and prefix caching constraints determine whether computation is reused or recomputed.