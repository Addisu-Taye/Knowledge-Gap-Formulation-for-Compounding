# Evening Call Summary

The initial explainer was strong on prefill vs decode mechanics and provided solid trace-based analysis, but it did not fully close my original gap around KV cache behavior across API calls and prefix caching. In particular, it did not explicitly state that KV cache does not persist across API calls, which is the core reason my system prompt is recomputed every turn.

During the call, I pushed on this gap. We clarified that while the system prompt is logically reused, it is computationally recomputed on every API call, leading to repeated prefill cost. This distinction was added explicitly in the revised version.

Feedback also focused on prefix caching. The initial version mentioned it but did not define the conditions under which it applies. The revised version now includes the key constraint that prefix matching must be byte-identical from token position 0, along with concrete failure cases such as dynamic prefixes, routing differences, and cache expiration.

The final revision more clearly connects these mechanisms to the observed cost spikes in my Conversion Engine and provides concrete instrumentation strategies (token tracking, TTFT measurement, repeated prompt tests) to verify caching behavior in practice.