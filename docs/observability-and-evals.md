# Observability & Evals for Multi-Agent Systems

What to trace, what to score, and how to evaluate multi-agent runs without going broke.

## What to trace

A single-agent trace is a linear story. A multi-agent trace is a **tree** (supervisor → workers) or a **graph** (peers messaging). Your observability tool must render the tree, attribute cost per branch, and let you replay a single span.

**The shortlist (all verified 2026-09-29):**

| Tool | Shape | Best for |
|---|---|---|
| [LangSmith](https://docs.langchain.com/langsmith/observability-quickstart) | Hosted | LangGraph/LangChain shops; Trajectory + run-tree views |
| [Langfuse](https://langfuse.com/docs) | OSS + hosted | Collaborative debugging; agents-as-graphs; datasets + experiments |
| [Arize Phoenix](https://github.com/arize-ai/phoenix) | OSS | OpenTelemetry-native; evaluator scoring on traces |
| [Braintrust](https://www.braintrust.dev/docs) | Hosted | Active observability loop: annotate → evaluate → deploy |
| [Opik](https://github.com/comet-ml/opik) | OSS (Apache-2.0) | Full trace trees; PyTest-in-CI evals; fully self-hostable |
| [Pydantic Logfire](https://pydantic.dev/logfire/llm-observability) | Hosted | One trace across request+model+tool+DB; SQL-queryable; per-model cost |
| [AgentOps](https://github.com/AgentOps-AI/agentops) | OSS (MIT) | Two-line session replays; cost tracking |
| [Helicone](https://github.com/Helicone/helicone) | OSS gateway | 100+ models behind one key; routing + fallbacks + traces |
| [Portkey](https://portkey.ai) | Hosted | 1,600+ LLMs; guardrails + governance; MCP gateway |

**Cost attribution is the killer feature.** Multi-agent runs multiply token spend by the agent count *and* the round count. [Pydantic Logfire](https://pydantic.dev/logfire/llm-observability) (token/cost per model and provider), [AgentOps](https://github.com/AgentOps-AI/agentops), and [Helicone](https://github.com/Helicone/helicone) all do cost analytics — pick one before your first 5-agent run, not after the invoice.

## What to evaluate

Evaluate at three levels — component, trajectory, end-to-end:

1. **Component:** did the router route correctly? did the tool call validate? ([DeepEval](https://github.com/confident-ai/deepeval) component-level evals; [Ragas](https://github.com/vibrantlabsai/ragas) metrics)
2. **Trajectory:** was the *path* sane, not just the answer? ([DeepEval](https://github.com/confident-ai/deepeval) trajectory-based evals; [τ-bench](https://arxiv.org/abs/2406.12045)'s `pass^k` reliability metric)
3. **End-to-end:** did the task complete? ([GAIA](https://arxiv.org/abs/2311.12983), [WebArena](https://arxiv.org/abs/2307.13854), [AssistantBench](https://arxiv.org/abs/2407.15711))

**The eval frameworks (verified 2026-09-29):**

- **[DeepEval](https://github.com/confident-ai/deepeval)** — Pytest-style assertions; 50+ metrics (agent, tool-use, conversational, safety, RAG, voice); synthetic dataset generation for edge cases; local-first.
- **[Ragas](https://github.com/vibrantlabsai/ragas)** — LLM-based + traditional metrics; production-aligned test-data generation; LangChain/observability integrations.
- **[Inspect AI](https://github.com/UKGovernmentBEIS/inspect_ai)** — UK AI Security Institute; tool use, multi-turn dialog, model-graded evals; 200+ pre-built evaluations.

## A minimal eval setup that actually works

1. **Golden set:** 20–50 real tasks from your domain, with annotated end states (τ-bench style — grade the *database state*, not the chat log).
2. **CI gate:** [Opik](https://github.com/comet-ml/opik)'s PyTest integration or [DeepEval](https://github.com/confident-ai/deepeval) assertions on every framework/prompt change.
3. **Production sampling:** score a sample of live traces ([Langfuse](https://langfuse.com/docs) production scoring, [Braintrust](https://www.braintrust.dev/docs) annotation) — distribution shift will find your golden set's blind spots.
4. **Failure taxonomy:** when runs fail, classify them (MAST's buckets: system design / inter-agent misalignment / task verification) instead of just retrying.

## Benchmarks worth running

For **tool-use reliability**: [τ-bench](https://arxiv.org/abs/2406.12045). For **general assistant capability**: [GAIA](https://arxiv.org/abs/2311.12983). For **web agents**: [WebArena](https://arxiv.org/abs/2307.13854), [AssistantBench](https://arxiv.org/abs/2407.15711), [Mind2Web](https://arxiv.org/abs/2306.06070). For **agent safety**: [ToolEmu](https://arxiv.org/abs/2309.15817). For **multi-agent collaboration specifically**: [Collab-Overcooked](https://arxiv.org/abs/2502.20073), [MultiAgentBench](https://arxiv.org/abs/2503.01935). See the [README benchmarks section](../README.md#benchmarks) for headline results.
