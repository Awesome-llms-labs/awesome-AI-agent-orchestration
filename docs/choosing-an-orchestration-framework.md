# Choosing an Orchestration Framework

How to pick from the frameworks in this list. The honest answer: **start with the simplest thing that survives your failure modes**, then add orchestration only where the pain is.

## The one-paragraph decision tree

1. **One agent, short task (< 2 min), tools are local?** You don't need an orchestration framework — a typed loop ([PydanticAI](https://github.com/pydantic/pydantic-ai), [SmolAgents](https://github.com/huggingface/smolagents)) or even a plain SDK call is enough.
2. **One agent, long-running or interruptible?** You need **durable execution**: [LangGraph](https://github.com/langchain-ai/langgraph) (checkpointing + `interrupt`), or a durable backend ([Temporal](https://temporal.io/solutions/ai), [Inngest](https://inngest.com/), [DBOS](https://dbos.dev/), [Trigger.dev](https://trigger.dev)).
3. **Multiple agents with distinct roles?** Pick your coordination style (see [orchestration patterns](orchestration-patterns.md)):
   - Supervisor/router → [CrewAI](https://github.com/crewAIInc/crewAI) Crews, [Agent Squad](https://github.com/2fastlabs/agent-squad) SupervisorAgent, Bedrock Agents multi-agent
   - Explicit graphs → [LangGraph](https://github.com/langchain-ai/langgraph), [Microsoft Agent Framework](https://github.com/microsoft/agent-framework), [Google ADK](https://github.com/google/adk-python), [Mastra](https://mastra.ai/docs)
   - Conversational teams → [AG2](https://github.com/ag2ai/ag2), [CAMEL](https://github.com/camel-ai/camel)
   - Typed contracts → [PydanticAI](https://github.com/pydantic/pydantic-ai) harness, [OpenAI Agents SDK](https://github.com/openai/openai-agents-python) handoffs
4. **Enterprise governance, audit, and cost control?** [BeeAI Framework](https://github.com/i-am-bee/beeai-framework) (deterministic rules), [UiPath Maestro](https://www.uipath.com/platform/agentic-automation), [Microsoft Foundry Agent Service](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/workflow), [IBM watsonx Orchestrate](https://www.ibm.com/think/topics/multi-agent-collaboration).
5. **.NET shop?** [Microsoft Agent Framework](https://github.com/microsoft/agent-framework) is the strongest first-class .NET option (Python parity too).
6. **TypeScript-first?** [Mastra](https://mastra.ai/docs), [VoltAgent](https://voltagent.dev), [Trigger.dev](https://trigger.dev).
7. **Visual/low-code builders for non-engineers?** [n8n](https://docs.n8n.io), [CrewAI AMP](https://crewai.com/amp) (Crew Studio). Do **not** adopt [Flowise](https://github.com/FlowiseAI/Flowise) (archived) or [AutoGen Studio](https://microsoft.github.io/autogen/dev/user-guide/autogenstudio-user-guide/index.html) for production (research prototype).

## What to avoid

- **Don't adopt [AutoGen](https://github.com/microsoft/autogen) or [Semantic Kernel](https://github.com/microsoft/semantic-kernel) for new builds.** Both are officially maintenance-mode/superseded; Microsoft points to [Microsoft Agent Framework](https://github.com/microsoft/agent-framework) with published migration guides.
- **Don't treat [MCP](https://modelcontextprotocol.io) as agent-to-agent orchestration.** It is the agent↔tool layer; agent↔agent is [A2A](https://a2a-protocol.org/latest/)'s domain (see [protocols guide](protocols-guide.md)).
- **Check licenses before building a business on them:** Inngest is SSPL, n8n is fair-code, Windmill is AGPL-3.0, Restate's runtime is BSL-1.1 — none are OSI open source. Memori's license is ambiguous (GitHub NOASSERTION vs README Apache-2.0 claim).
- **"Meta Agentic Orchestration" is not a product.** Meta's Llama Stack is now the independent [OGX](https://github.com/ogx-ai/ogx) project.

## A practical shortlist by use case

| Use case | Start here | Then consider |
|---|---|---|
| Durable single agent with approvals | LangGraph | Temporal / DBOS for infra-level durability |
| Role-based team (research, content) | CrewAI | Agent Squad, Microsoft Agent Framework |
| Deterministic multi-step pipeline | Microsoft Agent Framework, Google ADK, Mastra | PydanticAI + Temporal |
| Regulated enterprise | BeeAI Framework, UiPath Maestro, Foundry | watsonx Orchestrate, Agentforce |
| Fast prototype | SmolAgents, OpenAI Agents SDK | PraisonAI (YAML graphs) |
| Data/analytics agents | TaskWeaver, Dapr Agents | — |
| Self-hosted memory | Mem0, Graphiti, Cognee | Redis Agent Memory, Zep |

## Verification note

Framework facts above were verified on official repos/docs 2026-09-29 (see ✅/⚠️ tags in the README). Anything that can change — pricing, star counts, release cadences — is omitted; check the linked source.
