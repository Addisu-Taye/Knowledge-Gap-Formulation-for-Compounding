# Evening Call Summary

The explainer successfully clarified that KV cache is only preserved within a single API call and is not carried across turns in a multi-turn agent. The distinction between logical reuse of prompts and computational reuse was especially helpful.

Feedback focused on making prefix caching conditions more explicit and emphasizing that it is not guaranteed in most API setups. The section on cache invalidation was strengthened by adding concrete failure cases (token mismatch, routing differences, TTL expiry).

The revised version more clearly connects the mechanism to the observed cost spikes in the system and provides actionable ways to verify caching behavior through instrumentation.