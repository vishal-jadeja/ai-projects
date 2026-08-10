# AI Projects — Interview Q&A Bank

Questions interviewers ask about builds like these. Answer from your own project experience once each ships — replace generic answers with what actually happened.

**Q: How do you make an LLM return valid structured output reliably?**
A: Layers: (1) constrain at the API level — native structured output / function calling modes where the provider enforces the schema; (2) validate with Pydantic on receipt; (3) on failure, repair loop — re-prompt with the validation error inline, bounded to 2-3 attempts; (4) fallback chain — simpler schema or safe refusal; (5) log every failure with cause so schema drift shows up in dashboards, not incidents. Never string-parse and hope.

**Q: Your RAG system answers wrong. Walk me through debugging it.**
A: Localize first — is it retrieval or generation? Check retrieved chunks for the failing query: if the answer isn't in them → retrieval problem (chunking too coarse/fine, embedding mismatch, missing hybrid/keyword path, stale index). If it is in them → generation problem (context ordering, prompt, model ignoring context — check grounding/citation). This split is why you keep retrieval evals (recall@k) separate from end-to-end evals.

**Q: How do you stop an agent from looping forever?**
A: Hard budgets, not vibes: max iterations, max tool calls, max tokens/cost per run, wall-clock timeout. Plus semantic termination: detect repeated identical tool calls, no-progress reflection steps. On budget exhaustion, degrade gracefully — return best partial result with an explicit "incomplete" flag instead of erroring.

**Q: How do you evaluate an LLM feature before shipping a change?**
A: Golden set of real cases with expected properties → regression suite in CI. LLM-as-judge for scale, but calibrate: spot-check judge agreement against human labels; judge prompts are code, version them. Track eval scores over time — silent regressions are the failure mode. Adversarial set for injections and edge cases.

**Q: What does observability mean for an LLM app beyond normal APM?**
A: Traces per request with spans for each model/tool call; per-span tokens, latency, cost; prompt+completion capture (PII-safe); drift monitoring on input distributions and eval scores; cost attribution per feature/tenant, not just per model. A latency budget question follows: know prefill vs decode and where your p95 goes.

**Q: When would you NOT use an agent?**
A: When the workflow is known ahead of time — use a fixed pipeline/workflow (cheaper, deterministic, testable). Agents earn their cost only when the path genuinely depends on intermediate results. Most "agent" products in production are workflows with one or two LLM decision points — and that's correct engineering, not a compromise.
