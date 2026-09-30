# Glossary

Orchestration vocabulary, as used in this list.

- **Agent Card** — A2A's discovery document (`/.well-known/agent-card.json`): declares an agent's identity, capabilities, and endpoints so other agents can find and use it.
- **Checkpointing** — Persisting an agent's state (conversation, memory, position in a workflow) so the run can resume after interruption. LangGraph's checkpointers and Microsoft Agent Framework's checkpointing are framework-level; Temporal/DBOS provide it at the infrastructure level.
- **Durable execution** — The guarantee that a workflow survives crashes, redeploys, and long pauses, resuming from the last persisted state. The single most important production property for long-running agents. Provided by Temporal, DBOS, Inngest, Trigger.dev, Hatchet, Restack — or framework checkpointing.
- **Handoff** — Transferring a task (with its context) from one agent to another. Typed handoffs (OpenAI Agents SDK) carry structured data; conversational handoffs (AutoGen/AG2) carry chat history. Agency Swarm restricts handoffs with claims.
- **HITL (human-in-the-loop)** — Pausing an agent run for human approval before a sensitive action: tool calls, payments, destructive operations. Native in LangGraph (`interrupt`), AutoGen (`UserProxyAgent`), n8n, PraisonAI, Foundry workflows. The standalone HumanLayer SDK is deprecated.
- **Interrupt** — LangGraph's mechanism for pausing a run at a defined point (approval, editing, debugging) and resuming later, optionally with modified state.
- **Long-term memory** — State that persists across sessions: facts about the user, learned preferences, accumulated knowledge. See the [memory section](../README.md#memory--state-for-agents): Mem0, Graphiti, Zep, LangMem, Cognee, Supermemory.
- **MCP (Model Context Protocol)** — The agent↔tool standard (Agentic AI Foundation): Hosts/Clients/Servers exposing Tools, Resources, Prompts over JSON-RPC. For agent↔agent, see A2A.
- **Orchestrator** — The coordinator agent (or component) in supervisor/orchestrator-worker patterns that plans, delegates, and consolidates. See [orchestration patterns](orchestration-patterns.md).
- **Replay / time-travel** — Re-executing a workflow from a recorded state for debugging. Mastra offers step replay; Restack offers time-travel with replay in its dev UI; LangSmith shows the full run tree.
- **Run / Thread / Session** — Framework-specific names for one execution of an agent: Runs (LangChain Agent Protocol, Temporal), Threads (Agent Protocol, A2A's task contexts), Sessions (OpenAI Agents SDK, Bedrock AgentCore).
- **Sandbox** — An isolated execution environment for agent actions (code execution, browsing, shell). Ranges from containers (TaskWeaver's sandbox sessions) to session-per-microVM (Bedrock AgentCore) to full VMs — see [awesome-ai-sandboxes](https://github.com/awesome-llms-labs/awesome-ai-sandboxes) and [awesome-microVM](https://github.com/awesome-llms-labs/awesome-microVM).
- **Supervisor** — See Orchestrator. In CrewAI, a crew can run with a manager (hierarchical process); in Agent Squad and Bedrock Agents, the SupervisorAgent/SUPERVISOR mode explicitly routes across specialists.
- **Swarm** — Peer-to-peer multi-agent topology with no central coordinator (Agency Swarm, AG2 Network model).
- **Trajectory** — The full sequence of an agent's steps (thoughts, tool calls, observations). The unit of analysis for trajectory-based evals (DeepEval) and debugging (LangSmith Trajectory view).
- **Virtual actor** — Dapr's model: an addressable, stateful unit of computation (an agent) that can be reclaimed when idle while retaining its state — the actor-model answer to agent lifecycle management.
- **Workflow (durable)** — A long-lived, stateful computation with retries, waits, and human approvals — the infrastructure primitive under agent orchestration (Temporal workflows, Inngest functions, DBOS workflows).
