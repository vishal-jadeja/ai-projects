# AI Projects — Build Plan

Build in order — each project layers on the previous. Theory prerequisite listed per project (stage numbers from `applied-ai/PLAN.md`). Priorities: **P0** must-ship for interviews · **P1** strong differentiator · **P2** backlog.

Definition of shipped: working code + README (problem, trade-offs, failure modes handled, metrics) + demo script or recording + entry in `learn/interview/project-narratives.md`.

**Stack (decision Sep 2026):** Python + FastAPI + pydantic — prerequisite: `python-ai/` stages 1–3. From project #1 onward: containerized (Docker), CI with evals as a merge gate (`cloud-devops/` stages 2–3); project #2 deploys to AWS (`cloud-devops/` stage 4).

## 1. Structured Output Agent `P0`

*Theory: applied-ai stage 2.*

- [ ] Pydantic schemas enforced on every LLM response
- [ ] Validation failures → repair loop (re-prompt with error), bounded retries
- [ ] Fallback chain when repair exhausts (simpler schema → refusal)
- [ ] Log every validation failure with cause
- **Proves:** you can make LLMs reliable, not random.

## 2. RAG Agent with Citation Grounding `P0`

*Theory: applied-ai stages 3–4.*

- [ ] Ingestion: chunking strategy (compare 2), embeddings, vector store
- [ ] Hybrid search (BM25 + vector) + reranking
- [ ] Answers carry citations to source chunks; low-confidence answers flagged, not faked
- [ ] Retrieval eval set: recall@k, grounding/attribution checks — run on every change
- **Proves:** you can prevent hallucinations at scale and measure it.

## 3. ReAct Planning Agent `P0`

*Theory: applied-ai stage 5.*

- [ ] Observe → think → act → reflect loop over 2–3 real tools
- [ ] Iteration limits, tool budgets, termination conditions
- [ ] Self-critique step; graceful degradation on repeated failure
- [ ] Failure-mode handling: hallucinated tool calls, malformed arguments
- **Proves:** you can build agents that don't infinite-loop.

## 4. Multi-Tool Orchestrator `P1`

*Theory: applied-ai stage 7.*

- [ ] Dynamic tool registry with capability descriptions
- [ ] Capability-based routing; permission scoping per tool
- [ ] Parallel execution where independent; conflict resolution
- **Proves:** you can coordinate complex workflows.

## 5. Eval + Observability Harness `P1`

*Theory: applied-ai stages 4 & 9. Applied across projects 1–4.*

- [ ] Golden sets + regression tests runnable in CI
- [ ] LLM-as-judge with spot-checked agreement vs human labels
- [ ] Tracing (LangSmith or OTel): spans, tokens, latency, cost per run
- [ ] Cost dashboard per project/feature
- **Proves:** you treat evals and observability as engineering disciplines — the #1 differentiator in 2026 loops.

## Backlog `P2`

From the 12-project list: memory-enabled conversational agent, human-in-the-loop approval agent, cost-aware router, event-triggered automation agent, multi-agent debate, self-reflective auto-eval agent, OSS framework contribution. Pick up only after 1–5 shipped and interviews demand more.
