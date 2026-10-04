# Awesome AI Agent Orchestration [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated directory of **AI agent orchestration**: the frameworks that coordinate agents (multi-agent graphs, swarms, supervisor hierarchies), the protocols agents use to talk to each other, the memory and state layers that make agents durable, the platforms and schedulers that run them in production, the observability and eval tooling that keeps them honest, the benchmarks that measure them, and the papers that defined the field — as of **September 2026**.

A single agent is a loop: prompt, think, act, observe. Orchestration is everything that makes *many* agents — or one agent over *long* horizons — reliable: how work is decomposed and routed, where state lives when the process dies, how agents discover and pay each other, how humans approve dangerous steps, and how you debug the whole thing at 3 a.m.

> **Scope:** This list is about the *coordination mechanisms* — how agents (one or many) are composed, routed, and made reliable over long horizons: orchestration frameworks (multi-agent graphs, swarms, supervisor hierarchies), agent-to-agent protocols, memory and durable state, platforms and schedulers that run agents in production, observability/eval tooling, benchmarks, and the research papers that defined the field. It goes deeper on mechanisms than the ecosystem-wide [awesome-ai-agents](https://github.com/awesome-llms-labs/awesome-ai-agents), and it is broader than [awesome-multi-agents-workflow](https://github.com/awesome-llms-labs/awesome-multi-agents-workflow), which covers *collaboration only* and explicitly excludes single-agent frameworks.


**Verification confidence:** every entry is stamped ✅ **verified** (the claim was confirmed on the vendor's official page, the project repo, or the arXiv abstract page; verification dates 2026-09-29/30) or ⚠️ **unverified** (official-page fetch was rate-limited, so facts rest on search snippets only — never invented). Machine-readable records live in [`data/orchestration.json`](data/orchestration.json) with an `orchestration_verified` boolean per entry. **Scores, specs, and dates are never guessed.**

**Scale:** 99 entries across 7 categories — 29 frameworks, 9 protocols, 10 memory systems, 18 platforms, 14 observability/eval tools, 10 benchmarks, 9 papers. **88 verified** on official sources; **10 explicitly marked unverified** (listed in the README with a ⚠️ and a reason, never dropped).

## Contents
https://crewai.com/amp
- [Orchestration frameworks](#orchestration-frameworks)
  - [General-purpose multi-agent frameworks](#general-purpose-multi-agent-frameworks)
  - [Typed, durable-native, and governance-first](#typed-durable-native-and-governance-first)
  - [Research & specialized frameworks](#research--specialized-frameworks)
  - [Low-code & visual builders](#low-code--visual-builders)
- [Agent communication protocols](#agent-communication-protocols)
- [Memory & state for agents](#memory--state-for-agents)
- [Managed orchestration platforms](#managed-orchestration-platforms)
  - [Cloud agent platforms](#cloud-agent-platforms)
  - [Durable execution & deployment backends](#durable-execution--deployment-backends)
- [Observability & evals for multi-agent systems](#observability--evals-for-multi-agent-systems)
  - [Tracing & observability](#tracing--observability)
  - [Evaluation frameworks](#evaluation-frameworks)
- [Benchmarks](#benchmarks)
- [Research papers](#research-papers)
- [Discontinued & superseded](#discontinued--superseded)
- [Guides](#guides)
- [Related repositories](#related-repositories)
- [Contributing](#contributing)
- [License](#license)

---

## Orchestration frameworks

Frameworks that define how agents are composed, how work flows between them, and how state survives.

### General-purpose multi-agent frameworks

- [LangGraph](https://github.com/langchain-ai/langgraph) — ✅ LangChain: low-level framework for long-running, stateful agents as graphs — durable execution with auto-resume, human-in-the-loop state inspection, short/long-term memory, branching and subgraphs. MIT.
- [CrewAI](https://github.com/crewAIInc/crewAI) — ✅ CrewAI Inc.: role-based **Crews** (autonomous delegation, sequential/hierarchical processes) and event-driven **Flows** (fine-grained control, conditional branching). JSON-first crew projects via CLI; FAQ covers memory, guardrails, HITL. MIT; commercial [CrewAI AMP](#managed-orchestration-platforms).
- [Microsoft Agent Framework](https://github.com/microsoft/agent-framework) — ✅ Microsoft: the unified successor to AutoGen and Semantic Kernel (v1.0, Apr 2026) — agents plus graph-based multi-agent workflows (sequential, concurrent, handoff, group-collaboration) with checkpointing and HITL; Python and .NET SDKs; declarative YAML agents; MCP/A2A support; LTS commitment. MIT.
- [AG2](https://github.com/ag2ai/ag2) — ✅ ag2ai (community): the community AutoGen spin-off ("The Open-Source AgentOS") — protocol-driven, async throughout (`Agent.ask()` → `AgentReply`); a **Network** model with a Hub (registry, write-ahead log, audit trail) and typed channels replaces group-chat patterns; `@tool` decorators; HITL via `context.input()`/`hitl_hook`. Apache-2.0. The original AutoGen-derived line continues as **AG2 Classic** (`ag2ai/ag2-classic`).
- [OpenAI Agents SDK](https://github.com/openai/openai-agents-python) — ✅ OpenAI: lightweight, provider-agnostic multi-agent SDK — agents as tools for other agents, structured handoffs, input/output guardrails, built-in HITL, sessions, and tracing; sandbox, realtime, and voice-agent support. MIT.
- [Google ADK](https://github.com/google/adk-python) — ✅ Google: Agent Development Kit with a graph-based **Workflow Runtime** (routing, fan-out/fan-in, loops, retries, state, HITL, nested workflows), a Task API for structured delegation, and hierarchical multi-agent composition. Model- and deployment-agnostic (Gemini-optimized). Apache-2.0.
- [Agno](https://docs.agno.com) — ✅ Agno: SDK for building agent platforms around agents, teams, and workflows — memory, knowledge, guardrails, 100+ integrations; **AgentOS** runs the platform as a secure, durable API + MCP server with a Control Plane for management.
- [BeeAI Framework](https://github.com/i-am-bee/beeai-framework) — ✅ Linux Foundation (donated by IBM): production-grade multi-agent systems with **rule-based governance** (deterministic rules instead of suggested behavior), dynamic workflows (parallelism, retries, replanning), declarative YAML orchestration, OpenTelemetry, and native MCP/A2A. Python + TypeScript feature parity.
- [Dapr Agents](https://github.com/dapr/dapr-agents) — ✅ Dapr (CNCF): resilient agent framework on Dapr — durable workflow engine (retry, recover from last persisted state), virtual actors (idle agents reclaimable while retaining state), pub/sub coordination, key-value state, contextual memory, MCP auto-discovery. Python, Apache-2.0.
- [OGX (formerly Meta's Llama Stack)](https://github.com/ogx-ai/ogx) — ✅ OGX (independent): the open-source agentic API server formerly known as Meta's Llama Stack — now independent of Meta; its **Responses API** does server-side agentic orchestration (tool calling, MCP integration, built-in RAG) in a single call. (There is no product called "Meta Agentic Orchestration.")
- [Agent Squad](https://github.com/2fastlabs/agent-squad) — ✅ 2FastLabs: formerly AWS Labs' `multi-agent-orchestrator` (moved off AWS Labs) — intent-classified routing across specialist agents; **SupervisorAgent** (team coordination, parallel sub-agents, agent-as-tools), **GroundedAgent** (gatherer/presenter anti-hallucination), **JevClassifier** (typed decision routing via TypeSafe's Jev model); Python/TypeScript/Swift runtimes. Apache-2.0.

### Typed, durable-native, and governance-first

- [PydanticAI](https://github.com/pydantic/pydantic-ai) — ✅ Pydantic: typed agent loop (typed inputs/outputs/tools) plus **Pydantic Graph** typed graph control flow; harness adds sub-agents, planning, memory, persistence; durable execution via Temporal, DBOS, and Prefect; OpenTelemetry/Logfire observability. MIT.
- [Mastra](https://mastra.ai/docs) — ✅ Mastra: TypeScript framework with explicit step-based workflows (`createStep`/`createWorkflow`, typed Zod/Valibot/ArkType schemas), suspension/resumption, streaming, and **time-travel step replay**; managed workflow runners (e.g. Inngest); Studio UI; Mastra Cloud alongside OSS.
- [VoltAgent](https://voltagent.dev) — ✅ VoltAgent: TypeScript "AI Agent Engineering Platform" — OSS Core (Memory, RAG, Guardrails, Tools, MCP, Voice, Workflow) plus VoltOps Console (observability, automation, deployment, evals) cloud or self-hosted.
- [SmolAgents](https://github.com/huggingface/smolagents) — ✅ Hugging Face: minimal code-first framework — **CodeAgent** writes Python as its actions; multi-agent hierarchies; model/tool/modality-agnostic. Security caveat (official): the local code executor is *not* a security boundary — sandbox it (E2B, Blaxel, Modal, Docker). Apache-2.0.
- [Haystack](https://docs.haystack.deepset.ai) — ✅ deepset: open-source "AI orchestration framework" — modular components + pipelines for production agents, RAG, and multimodal search; loop-based Agent component with tool selection and schema-validated runtime state; model-agnostic. Apache 2.0; enterprise platform alongside OSS.
- [TaskWeaver](https://microsoft.github.io/TaskWeaver/docs/overview) — ✅ Microsoft: code-first framework for data-analytics tasks — converts requests into **generated, self-repairing code**; stateful conversations preserve in-memory Python data across rounds; sandbox execution, session isolation; OpenTelemetry. OSS.

### Research & specialized frameworks

- [MetaGPT](https://github.com/FoundationAgents/MetaGPT) — ✅ FoundationAgents: models a software company (PM, architect, project manager, engineer) — "Code = SOP(Team)"; one-line requirement → user stories, APIs, docs, full repo. MIT.
- [CAMEL](https://github.com/camel-ai/camel) — ✅ CAMEL-AI community: research-oriented multi-agent/society framework — role-playing, workforce orchestration, dynamic communication, stateful memory; modules for societies, memory, storage, interpreters, retrieval, runtime, HITL. Apache-2.0.
- [LlamaIndex Workflows](https://developers.llamaindex.ai/python/framework/understanding/workflows/) — ⚠️ LlamaIndex: event-driven orchestration via typed `@step` functions — async branching, loops, fan-out/fan-in; `AgentWorkflow` for agent handoffs; serializable `Context` state with step checkpointing; documented HITL. (Official-docs snippets only — fetch was rate-limited.)
- [Qwen-Agent](https://qwenlm.github.io/Qwen-Agent/en/) — ✅ Qwen team / Alibaba: open-source framework around Qwen instruction following, tool use, planning, and memory.
- [PraisonAI](https://github.com/MervinPraison/PraisonAI) — ✅ Mervin Praison: five-layer stack (Prompt → Context → Harness → Loop → Graph) — persistent memory with handoff `ContextPolicy`, guardrails and approval gates, doom-loop detection, and `AgentFlow` graph topologies (`route()`/`parallel()`/`loop()`/`repeat()`) expressible in YAML with no Python. MIT.
- [SuperAGI](https://github.com/TransformerOptimus/SuperAGI) — ✅ TransformerOptimus: dev-first autonomous agent framework — provision/spawn/deploy agents, toolkits marketplace, GUI, action console, agent memory, ReAct predefined workflows. MIT; README carries an "Under Development!" note.
- [AgentScope](https://agentscope.io) — ✅ AgentScope project: open-source stack for development, evaluation, hosting, memory, and fine-tuning — multi-model calls, retries/fallbacks, tool boundaries, Workspace, Agent Service; JVM framework with multi-agent collaboration; **ReMe** persistent memory across sessions.
- [Agency Swarm](https://github.com/VRSEN/agency-swarm) — ⚠️ VRSEN: extends the OpenAI Agents SDK for collaborative swarms — declared directed communication flows (no open broadcast), claims-restricted handoffs, organizational role structures. (README snippets only — fetch was rate-limited.)

### Low-code & visual builders

- [AutoGen Studio](https://microsoft.github.io/autogen/dev/user-guide/autogenstudio-user-guide/index.html) — ✅ Microsoft: low-code interface for prototyping AutoGen agent teams — visual Team Builder (JSON spec or drag-and-drop), interactive Playground, community Gallery, and export to Python/endpoints/Docker. Official caution: **"not meant to be a production-ready app"** — a research prototype.

---

## Agent communication protocols

The contracts agents use to discover, message, pay, and coordinate with each other.

- [A2A (Agent2Agent Protocol)](https://a2a-protocol.org/latest/) — ✅ Linux Foundation (ex-Google): the agent-to-agent standard — **Agent Cards** (`/.well-known/agent-card.json`) for discovery, a task lifecycle state machine (SUBMITTED → WORKING → COMPLETED/FAILED/CANCELED/REJECTED, plus INPUT_REQUIRED/AUTH_REQUIRED), Artifacts for outputs, `contextId` for multi-turn; JSON-RPC-over-HTTP (+SSE), gRPC, and REST transports. v1.0 era (2026-03); adopted by Azure AI Foundry, Copilot Studio, Bedrock AgentCore, Google Cloud. Official line: *"MCP is for agent-to-tool communication; A2A is for agent-to-agent — they are complementary."*
- [MCP (Model Context Protocol)](https://modelcontextprotocol.io) — ✅ Agentic AI Foundation (ex-Anthropic): the **agent↔tool** layer — Hosts, Clients, Servers exposing Tools, Resources, Prompts over JSON-RPC (stdio or Streamable HTTP); 2026-07-28 spec is a stateless redesign. Listed here because every orchestrated agent eventually calls tools through it.
- [ACP (Agent Communication Protocol)](https://github.com/i-am-bee/acp) — ⚠️ IBM / BeeAI: open REST/HTTP (OpenAPI 3.1) protocol — agents as services, Runs (sync/async/streaming), Await pause/resume, manifest discovery, TrajectoryMetadata audit trails. **Archived Aug 2025 and merged into A2A** — listed for lineage, not for new builds. (Snippet-level; not to be confused with Zed's unrelated *Agent Client Protocol*.)
- [AGNTCY](https://outshift.cisco.com/blog/building-the-internet-of-agents-introducing-the-agntcy) — ⚠️ Cisco Outshift / Linux Foundation: the "Internet of Agents" collective — OASF schema framework, Agent Directory, SLIM secure messaging (MLS), identity spec; SDKs in Python/JS/Go/Rust. Designed to work *alongside* A2A and MCP. (Snippet-level.)
- [ANP (Agent Network Protocol)](https://agent-network-protocol.com/) — ✅ community: open protocol suite for agent identity, naming, discovery, negotiation, and collaboration — signature **did:wba** identity (W3C-DID-compatible over plain HTTPS), end-to-end encrypted messaging, layered application protocols (description, discovery, payment/commerce).
- [Agent Protocol (LangChain)](https://github.com/langchain-ai/agent-protocol) — ✅ LangChain: framework-agnostic REST/OpenAPI spec for **serving** agents in production — Runs, Threads (multi-turn state), and a Store (long-term memory API); streaming with replay and HITL. Client→agent API, not a peer-to-peer protocol. MIT.
- [AP2 (Agent Payments Protocol)](https://ap2-protocol.org/) — ✅ FIDO Alliance (ex-Google): the agent-economy trust layer — cryptographically proves a user authorized an agent's purchase via **Verifiable Digital Credentials** (Checkout + Payment Mandates, SD-JWT); covers human-present and human-not-present flows; donated to FIDO April 2026. Authorizes rather than settles — rail-agnostic.
- [AITP (Agent Interaction & Transaction Protocol)](https://aitp.dev) — ✅ NEAR AI: open protocol for cross-trust-boundary interaction — agents communicate over Chat Threads (largely OpenAI Assistants/Threads-compatible) with extensible Capabilities.
- [Agora Protocol](https://arxiv.org/abs/2410.11905) — ⚠️ Oxford + Eigent AI (research): a **meta-protocol** addressing the "Agent Communication Trilemma" (versatility vs efficiency vs portability) — standardized routines for frequent exchanges, natural language for rare ones, the protocol itself negotiable. Academic proposal, no industry adoption. (Snippet-level.)

---

## Memory & state for agents

Orchestration fails without durable state. These are the memory layers agents read and write across runs, restarts, and handoffs.

- [Mem0](https://github.com/mem0ai/mem0) — ✅ Mem0: open-source memory layer between app and LLM — two-phase `add()` (LLM extraction → ADD/UPDATE/DELETE/NOOP reconciliation), CRUD+search scoped by `user_id`/`agent_id`/`run_id`, 20–30 vector backends (Qdrant default), optional graph memory. Apache-2.0.
- [Graphiti](https://github.com/getzep/graphiti) — ✅ Zep: open-source **temporal knowledge-graph** framework — bi-temporal fact edges (`t_valid`/`t_invalid`), contradictions auto-invalidate old edges (history preserved), hybrid semantic + BM25 + graph retrieval. Apache-2.0.
- [Zep](https://www.getzep.com) — ✅ Zep: managed "unified context layer" — governed, policy-filtered retrieval over a Context Graph, built on Graphiti. (Community Edition deprecated April 2025; OSS effort is on Graphiti.)
- [Memori](https://github.com/MemoriLabs/Memori) — ✅ Memori Labs: agent-native memory infrastructure — Python/TS SDKs; **LLM client-wrapping** persists and recalls conversations automatically in the background; entity → process → session memory levels; MCP server integration; BYODB. License ambiguous (GitHub NOASSERTION vs README Apache-2.0 claim — check before depending on it).
- [Supermemory](https://supermemory.ai/) — ✅ Supermemory: memory + continual learning — a learner model distills context into a vector-graph DB injected in real time; any model, any harness; connectors (Drive, Gmail, Notion, OneDrive, GitHub). Managed; self-hostable repo (MIT).
- [LangMem](https://github.com/langchain-ai/langmem) — ✅ LangChain: official long-term memory SDK for LangGraph agents — semantic, episodic, and procedural memories, including self-improving system-prompt optimization, backed by LangGraph checkpointing.
- [Cognee](https://github.com/topoteretes/cognee) — ✅ Topoteretes: memory control plane — an Extract-Cognify-Load pipeline builds a self-hosted typed knowledge graph (Kuzu/LanceDB/SQLite) so agents share persistent knowledge.
- [Redis Agent Memory](https://github.com/redis/agent-memory-server) — ✅ Redis: managed memory layer (part of Redis Iris) — session memory with TTL plus background promotion of extracted facts to long-term store.
- [Letta](https://letta.com) — ⚠️ Letta (ex-MemGPT): stateful agents from the MemGPT creators (UC Berkeley Sky Computing Lab lineage) — hierarchical memory (self-editing in-context "memory blocks" + archival storage), agents persist over a REST server. (Lineage verified; technical details snippet-level.)
- [OpenMemory](https://github.com/mem0ai/mem0) — ⚠️ mem0: local-first **MCP application** exposing one shared memory layer as an MCP server — any MCP client (Claude Desktop/Code, Cursor, Windsurf, ChatGPT) reads/writes cross-app memory. (Snippet-level; note several unrelated projects share the "OpenMemory" name.)

---

## Managed orchestration platforms

Someone else's computers, running your agents — with deployment, scaling, and governance handled.

### Cloud agent platforms

- [Amazon Bedrock Agents](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-multi-agent-collaboration.html) — ✅ AWS: managed **supervisor pattern** — one Bedrock Agent supervises collaborator sub-agents that plan, split tasks, work in parallel, and hand off via SUPERVISOR / SUPERVISOR_ROUTER modes.
- [Amazon Bedrock AgentCore](https://aws.amazon.com/about-aws/whats-new/2026/04/agentcore-new-features-to-build-agents-faster/) — ✅ AWS: managed harness — define an agent (model + system prompt + tools) and run immediately; each session gets its own **microVM** with filesystem/shell; filesystem persistence lets agents suspend mid-task and resume; AgentCore CLI with IaC governance (CDK now, Terraform coming).
- [Gemini Enterprise Agent Platform](https://google.github.io/adk-docs/) — ✅ Google Cloud: managed runtime for ADK multi-agent orchestrations (sub_agents, AgentTool, graph workflows) as a service — memory bank, sessions, sandbox, observability. (Vertex AI Agent Engine rebranded here.)
- [Microsoft Foundry Agent Service](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/workflow) — ✅ Microsoft: managed multi-agent orchestration (formerly Azure AI Foundry) — visual/YAML workflows (Sequential, Group chat, **Human-in-the-loop**) invoking multiple Foundry agents with variables, versioning, and tracing.
- [Salesforce Agentforce](https://www.salesforce.com/agentforce/multi-agent-orchestration/) — ✅ Salesforce: primary agent routes to specialist secondary agents on the Atlas Reasoning Engine, with A2A support for third-party agents and built-in observability; grounded in CRM/Data Cloud.
- [IBM watsonx Orchestrate](https://www.ibm.com/think/topics/multi-agent-collaboration) — ✅ IBM: enterprise multi-agent collaboration — supervisor/orchestrator routes work across skills (independent agents) via Intent Parser and Flow Orchestrator, with shared context/memory, cross-agent visibility, and cost monitoring.
- [UiPath Maestro](https://www.uipath.com/platform/agentic-automation) — ✅ UiPath: agentic automation control plane coordinating **humans, agents, and systems** — Orchestration (BPMN/Flow/Case), Execution, Governance layers; human judgment as a first-class workflow step; agent registry governing third-party agents (Foundry, Bedrock, LangChain, CrewAI); durable pause/resume/recover.
- [CrewAI AMP](https://crewai.com/amp) — ✅ CrewAI Inc.: managed enterprise suite — build crews in Crew Studio (visual/YAML), deploy to managed infrastructure, consume via API endpoints; executions, live crews, and seats metered.
- [Cloudflare Agents](https://developers.cloudflare.com/agents/) — ✅ Cloudflare: stateful agents on Workers (Durable Objects) that coordinate other agents via `getAgentByName`; official multi-agent worker/pipeline examples.
- [Gemini Enterprise (formerly Google Agentspace)](https://cloud.google.com/geminienterprise) — ⚠️ Google Cloud: the employee-facing governed AI platform (enterprise connectors, agents, search/chat) — distinct from the developer-facing Agent Platform above. (Snippet-level.)

### Durable execution & deployment backends

The runtimes that make agent runs survive crashes, redeploys, and 3 a.m. pager duty.

- [Temporal](https://temporal.io/solutions/ai) — ✅ Temporal Technologies: durable orchestrator — workflows-as-code retaining state for **years**; automatic retries/self-healing; HITL approvals; official integrations with OpenAI Agents SDK, Pydantic AI, Google ADK, Mastra, Strands.
- [LangSmith Deployment](https://docs.langchain.com/langsmith/deployment-quickstart) — ✅ LangChain: managed deployment for LangGraph multi-agent applications (renamed from **LangGraph Platform**, Oct 2025); the framework stays MIT.
- [Inngest](https://inngest.com/) — ✅ Inngest: durable functions for agents and event-driven workflows — multi-step checkpoints, waits, fan-out, throttling, retries, recovery; deploys alongside your existing app. **SSPL** — not OSI open source.
- [Trigger.dev](https://trigger.dev) — ✅ Trigger.dev: open-source durable AI agents/workflows in TypeScript — agents survive refreshes, redeploys, crashes; tool calling, HITL, frontend streaming; atomic deployment versions. Apache-2.0, self-hostable, managed cloud.
- [DBOS](https://dbos.dev/) — ✅ DBOS, Inc.: durable workflows + queues on **Postgres** — normal-code workflows (branches, loops, subtasks, retries), durable HITL waits and signals across restarts; first-party integrations with OpenAI Agents, LlamaIndex, Pydantic AI; DBOS Conductor for recovery/monitoring.
- [Hatchet](https://docs.hatchet.run/home/your-first-task) — ✅ Hatchet: durable task engine — tasks compose into DAG workflows or spawn child tasks at runtime; every task persisted with state and results; retries with backoff, timeouts, concurrency, rate limits, worker affinity.
- [Restack](https://www.restack.io/enterprise) — ✅ Restack: open-source durable workflows **built with Temporal** for orchestration and Kubernetes for scale — state persists for months/years; dev UI with time-travel and replay; Cloud, on-prem, or customer K8s.
- [n8n](https://docs.n8n.io) — ✅ n8n GmbH: fair-code workflow automation with a LangChain-based **AI Agent node**, HITL tool approval, MCP servers as tools, and an AI Agent Tool sub-node for multi-agent delegation; self-host or Cloud. Fair-code, not OSI open source.

---

## Observability & evals for multi-agent systems

When five agents disagree at scale, you need traces, not vibes.

### Tracing & observability

- [LangSmith](https://docs.langchain.com/langsmith/observability-quickstart) — ✅ LangChain: end-to-end tracing — every step of a request captured as nested spans; Trajectory and Details views of the full run tree; native integrations (LangChain, LangGraph, Anthropic).
- [Langfuse](https://langfuse.com/docs) — ✅ Langfuse: open-source AI engineering platform — traces cover LLM *and* non-LLM calls; multi-turn sessions with user tracking; agents as graphs; SDKs, 100+ integrations, OpenTelemetry; production scoring, LLM-as-a-judge, datasets, experiments. Self-hostable.
- [Arize Phoenix](https://github.com/arize-ai/phoenix) — ✅ Arize AI: open-source observability + evaluation on OpenTelemetry/OpenInference — step-by-step tracing, evaluator scoring (LLM/code/human), prompt management with span replay, datasets & experiments; evaluator integrations with Ragas, Deepeval, Cleanlab.
- [Braintrust](https://www.braintrust.dev/docs) — ✅ Braintrust: "active observability" — instrument → observe → annotate → evaluate → deploy; human-feedback annotation and dataset building; experiments and playgrounds; `bt` CLI for coding-agent setup.
- [Opik](https://github.com/comet-ml/opik) — ✅ Comet: Apache-2.0 open-source observability + eval — full trace trees for multi-step agents; datasets, experiments, LLM-as-a-judge metrics (hallucination, moderation, RAG); prompt playground; PyTest integration for CI/CD; integrations with Google ADK, AutoGen, AG2. Fully self-hostable.
- [Pydantic Logfire](https://pydantic.dev/logfire/llm-observability) — ✅ Pydantic: OpenTelemetry-native — request, model, tool, API, and database work as **one trace**; detects agent runs from framework conventions (Pydantic AI, LangGraph, OpenAI Agents SDK, CrewAI, ADK, Strands, Semantic Kernel); PostgreSQL-compatible SQL querying; token/cost per model and provider.
- [AgentOps](https://github.com/AgentOps-AI/agentops) — ✅ AgentOps: open-source (MIT) DevTool platform — session replays in two lines of code; step-by-step execution graphs; LLM cost tracking; native integrations (OpenAI Agents SDK, CrewAI, AG2, CAMEL, LangChain); self-hostable.
- [Helicone](https://github.com/Helicone/helicone) — ✅ Helicone: Apache-2.0 open-source **AI gateway + observability** — 100+ models behind one API key with routing/fallbacks; traces and sessions for agents; cost/latency/quality analytics; self-hostable via Docker/Helm.
- [Portkey](https://portkey.ai) — ✅ Portkey: AI gateway + observability + guardrails + governance — unified API to 1,600+ LLMs, real-time observability and cost monitoring, **MCP gateway** centralizing MCP-server auth/access/observability, PII redaction. (Homepage now also brands as "PRISMA AIRS AI Gateway".)
- [OpenClaw Monitor](https://github.com/flik2002/openclaw-monitor) — ✅ flik2002: MIT-licensed open-source web dashboard for a running OpenClaw gateway — live session list, cron task view, token-usage and 7-day message-trend charts, system metrics; Vue 3 + ECharts frontend, Node.js backend over the gateway's WebSocket JSON-RPC API.

### Evaluation frameworks

- [DeepEval](https://github.com/confident-ai/deepeval) — ✅ Confident AI: open-source eval framework — "unit test LLM outputs with Pytest-style assertions"; 50+ metrics (agent, tool-use, conversational, voice, safety, RAG, multimodal); end-to-end, trajectory-based, and component-level evals; synthetic dataset generation; local-first.
- [Ragas](https://github.com/vibrantlabsai/ragas) — ✅ Vibrant Labs (renamed from Exploding Gradients): toolkit for evaluating and optimizing LLM applications — LLM-based and traditional metrics; production-aligned test-data generation; LangChain and observability integrations. Apache-2.0.
- [Inspect AI](https://github.com/UKGovernmentBEIS/inspect_ai) — ✅ UK AI Security Institute: evaluation framework — prompt engineering, tool use, multi-turn dialog, model-graded evals; 200+ pre-built evaluations runnable on any model; extensible scoring. MIT.

> **Human-in-the-loop note:** first-class HITL lives inside the frameworks (LangGraph `interrupt` + checkpointer; AutoGen `UserProxyAgent` with ALWAYS/NEVER/AUTO modes; n8n tool-approval gates; PraisonAI approval gates). The cross-framework HITL SDK [HumanLayer](https://github.com/humanlayer/humanlayer) is ✅ **deprecated** per its own README — its code now points to a coding-agent rebuild; don't adopt the SDK for new builds.

---

## Benchmarks

Benchmarks that stress agents and multi-agent systems — tool use, web navigation, collaboration, and failure modes.

- [τ-bench](https://arxiv.org/abs/2406.12045) — ✅ Sierra Research: tool-agent-user interaction in airline + retail domains — LM-simulated users, end-of-conversation database state graded against goal states, `pass^k` reliability metric. Headline (authors): GPT-4o succeeds on <50% of tasks; pass^8 < 25% in retail. ([repo](https://github.com/sierra-research/tau-bench))
- [GAIA](https://arxiv.org/abs/2311.12983) — ✅ Meta AI / Hugging Face / AutoGPT: 466 human-curated real-world assistant questions (reasoning, multimodality, browsing, tool use). Humans 92% vs 15% for GPT-4 with plugins (authors). The general-assistant benchmark multi-agent systems report against (e.g. Magentic-One).
- [AgentBench](https://arxiv.org/abs/2308.03688) — ✅ THUDM: 8 environments (OS, database, knowledge graph, card game, lateral thinking, household, shopping, browsing). Headline (authors): big commercial-vs-OSS gap; poor long-term reasoning, decision-making, and instruction-following are the main obstacles.
- [AssistantBench](https://arxiv.org/abs/2407.15711) — ✅ Tel Aviv University et al.: 214 realistic, time-consuming web tasks. Headline (authors): no model exceeds 26 points; SOTA web agents score near zero. Introduces the SeePlanAct agent.
- [WebArena](https://arxiv.org/abs/2307.13854) — ✅ CMU: 812 long-horizon tasks in a self-hosted realistic web (e-commerce, forum, GitLab, CMS), grading functional correctness. Headline (authors): best GPT-4 agent 14.41% vs 78.24% human.
- [Mind2Web](https://arxiv.org/abs/2306.06070) — ✅ Ohio State: 2,000+ open-ended tasks from 137 real websites, 31 domains — the generalist-web-agent dataset, testing cross-task/cross-website/cross-domain generalization. NeurIPS 2023 Spotlight.
- [ToolEmu](https://arxiv.org/abs/2309.15817) — ✅ U. Toronto / Vector / Stanford: LM-emulated sandbox (standard + adversarial) with LM-based safety evaluation — 36 toolkits, 144 cases, 9 risk types. Headline (authors): 68.8% of identified failures are real-world-valid; the safest agent still fails 23.9%. ICLR 2024 Spotlight.
- [Collab-Overcooked](https://arxiv.org/abs/2502.20073) — ✅ academia: LLM **multi-agent collaboration** benchmark on Overcooked-AI — 30 tasks, 6 complexity levels, two LLM agents cooperating via natural language; process-oriented metrics (not just success). Collaboration collapses at levels 5–6 (0–6% success, authors). EMNLP 2025.
- [MultiAgentBench](https://arxiv.org/abs/2503.01935) — ✅ UIUC: collaboration **and** competition across Werewolf, research co-authoring, Minecraft building, database analysis, coding — built on the MARBLE framework with milestone-based KPIs; compares coordination topologies (star, chain, tree, graph — graph best in research, authors). ACL 2025.
- [MAST — Why Do Multi-Agent LLM Systems Fail?](https://arxiv.org/abs/2503.13657) — ⚠️ UC Berkeley: empirically grounded failure taxonomy — 1,642 traces across 7 frameworks, 14 failure modes (system design, inter-agent misalignment, task verification); failure rates 41–86.7%. **arXiv ID not yet confirmed on the abs page** — treat the ID as provisional. (Corroborated by multiple secondary sources.)

---

## Research papers

- [AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation](https://arxiv.org/abs/2308.08155) — ✅ Microsoft Research: the conversable-agents framework paper — conversation patterns programmable in natural language and code; effective across math, coding, QA, OR, decision-making, entertainment.
- [MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework](https://arxiv.org/abs/2308.00352) — ✅ DeepWisdom: SOPs encoded as prompt sequences combat cascading hallucinations; assembly-line roles with intermediate verification.
- [ChatDev: Communicative Agents for Software Development](https://arxiv.org/abs/2307.07924) — ✅ Tsinghua: chat chain + communicative dehallucination; natural language for design, programming language for debugging. (Note: this ID is the *ChatDev* paper — the CAMEL framework paper by Li et al. is a different work.)
- [Magentic-One: A Generalist Multi-Agent System for Solving Complex Tasks](https://arxiv.org/abs/2411.04468) — ✅ Microsoft Research: lead **Orchestrator** plans, tracks progress, and replans to recover from errors, directing specialist agents (browser, files, code); competitive with SOTA on GAIA, AssistantBench, WebArena; ships with the AutoGenBench eval harness.
- [A Survey of AI Agent Protocols](https://arxiv.org/abs/2504.16736) — ✅ Yang et al.: the first comprehensive protocol analysis — two-dimensional classification (context-oriented vs inter-agent × general vs domain-specific); security/scalability/latency comparison.
- [LLM-Based Multi-Agent Systems: Techniques and Business Perspectives](https://arxiv.org/abs/2411.14033) — ✅ Yang et al.: tools-becoming-agents ⇒ LLM-based Multi-Agent Systems (LaMAS); proposes a preliminary LaMAS protocol covering technical, privacy, and business requirements.
- [The Landscape of Emerging AI Agent Architectures for Reasoning, Planning, and Tool Calling](https://arxiv.org/abs/2404.11584) — ✅ Masterman et al.: surveys single- and multi-agent architectures; design themes — leadership impact, communication styles, planning–execution–reflection. (Note: arXiv 2502.06744 is an unrelated plasma-physics paper — use this ID.)
- [Large Language Model based Multi-Agents: A Survey of Progress and Challenges](https://arxiv.org/abs/2402.01680) — ✅ Guo et al. (IJCAI 2024): domains, environments, agent profiling, communication, capacity growth; companion repo tracks the literature.
- [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) — ⚠️ Anthropic: the production field guide — workflows vs agents; "simplest solution possible"; canonical patterns (prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer). (Official page fetch was rate-limited; details from corroborating secondary sources.)

---

## Discontinued & superseded

What died, merged, or got renamed — so you don't build on it by accident. Details in [docs/status-changes.md](docs/status-changes.md).

- [Flowise](https://github.com/FlowiseAI/Flowise) — ✅ **archived**: the drag-and-drop low-code agent builder is a public archive ("Refer to Future of Flowise").
- [AutoGen](https://github.com/microsoft/autogen) — ✅ **maintenance mode**: community-managed, no new features; Microsoft directs new users to [Microsoft Agent Framework](https://github.com/microsoft/agent-framework).
- [Semantic Kernel](https://github.com/microsoft/semantic-kernel) — ✅ **superseded** by Microsoft Agent Framework 1.0 per its own README.
- [ACP (Agent Communication Protocol)](https://github.com/i-am-bee/acp) — ⚠️ **archived, merged into A2A** (Linux Foundation, ~Sept 2025). Build on A2A instead.
- [HumanLayer](https://github.com/humanlayer/humanlayer) — ✅ **deprecated SDK**: the cross-framework HITL SDK is deprecated per its own README; development moved to a coding-agent rebuild.
- **Renames (2025–2026):** LangGraph Platform → **LangSmith Deployment** (Oct 2025) · Vertex AI Agent Engine → **Gemini Enterprise Agent Platform** (Agent Runtime) · Google Agentspace → **Gemini Enterprise** · Azure AI Foundry / Azure AI Agent Service → **Microsoft Foundry** · AWS `multi-agent-orchestrator` → **Agent Squad** (2FastLabs, no longer AWS) · Meta's Llama Stack → **OGX** (independent) · `explodinggradients/ragas` → `vibrantlabsai/ragas` · Portkey homepage now also brands **PRISMA AIRS AI Gateway** · Inngest is **SSPL**, n8n **fair-code**, Windmill **AGPL-3.0**, Restate runtime **BSL-1.1** — none are OSI open source.

---

## Guides

- [Choosing an orchestration framework](docs/choosing-an-orchestration-framework.md) — which framework fits single-agent durability vs multi-agent teams vs enterprise governance.
- [Orchestration patterns](docs/orchestration-patterns.md) — supervisor, hierarchical, swarm, graph-based, sequential/parallel, and when each breaks.
- [Protocols guide](docs/protocols-guide.md) — A2A vs MCP vs ACP vs AGNTCY vs ANP: which layer each lives on and how they compose.
- [Observability & evals](docs/observability-and-evals.md) — what to trace, what to score, and how to eval multi-agent runs without going broke.
- [Glossary](docs/glossary.md) — orchestration vocabulary: durable execution, checkpointing, handoff, Agent Card, HITL, and more.
- [Status changes](docs/status-changes.md) — retirements, merges, and renames, newest first.
- [Machine-readable catalog](data/orchestration.json) — every entry with its verification status.

## Related repositories

- [awesome-multi-agents-workflow](https://github.com/dakotac1994/awesome-multi-agents-workflow) — sibling list: multi-agent workflows (OSS frameworks, SaaS platforms, cloud services, protocols, workflow infra, benchmarks) — the workflow-centric companion to this orchestration-layer list.
- [awesome-ai-agents](https://github.com/awesome-llms-labs/awesome-ai-agents) — sibling list: AI agent frameworks and ecosystems.
- [awesome-decisions-llms](https://github.com/awesome-llms-labs/awesome-decisions-llms) — sibling list: LLMs for decision-making — decision-tuned models, benchmarks, and deliberation pipelines (debate, Tree/Graph of Thoughts).
- [awesome-flagship-llms](https://github.com/awesome-llms-labs/awesome-flagship-llms) — sibling list: the single most capable model from every major lab — the brains behind the agents.
- [awesome-flash-llms](https://github.com/awesome-llms-labs/awesome-flash-llms) — sibling list: cost-performance Flash-class LLMs — price the orchestration loop, not the token.
- [awesome-fast-llms](https://github.com/awesome-llms-labs/awesome-fast-llms) — sibling list: inference-speed LLMs — latency budgets for multi-agent loops.
- [awesome-free-llms](https://github.com/awesome-llms-labs/awesome-free-llms) — sibling list: free LLM tiers and models for zero-cost orchestration experiments.
- [awesome-ai-sandboxes](https://github.com/awesome-llms-labs/awesome-ai-sandboxes) — sibling list: sandboxes for safely executing agent loops.
- [awesome-microVM](https://github.com/awesome-llms-labs/awesome-microVM) — sibling list: microVMs — the isolation primitive under session-per-microVM runtimes like Bedrock AgentCore.
- [awesome-startup-credits](https://github.com/awesome-llms-labs/awesome-startup-credits) — sibling list: startup credit programs — fund the cloud bill for your agent fleet.

## Contributing

Entries and corrections are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md). Every PR is checked by CI: lychee link check over all markdown files, and JSON validation of `data/orchestration.json` (including the `orchestration_verified` boolean, allowed `category`/`status` sets, and required `source_url` for verified entries).

## License

[MIT](LICENSE) © 2026 dakotac1994

