# Orchestration Patterns

The recurring shapes of multi-agent coordination. Every framework in this list implements some subset — knowing the pattern tells you what a framework can and can't do, and where it breaks.

## 1. Supervisor (orchestrator-workers)

One coordinator agent decomposes the task, delegates to specialist workers, and consolidates results. Workers may themselves delegate (supervisor-router).

- **Strengths:** clear accountability; easy to add/remove specialists; natural fit for approval gates (the supervisor *is* the gate).
- **Failure modes:** the supervisor is a single point of failure and a context bottleneck — everything routes through its context window. Delegation depth beyond 2–3 levels degrades fast.
- **Examples:** [CrewAI](https://github.com/crewAIInc/crewAI) hierarchical crews, [Agent Squad](https://github.com/2fastlabs/agent-squad) SupervisorAgent, [Magentic-One](https://arxiv.org/abs/2411.04468) (lead Orchestrator), Amazon Bedrock Agents multi-agent, [IBM watsonx Orchestrate](https://www.ibm.com/think/topics/multi-agent-collaboration), [Salesforce Agentforce](https://www.salesforce.com/agentforce/multi-agent-orchestration/).
- **Anthropic's name for it:** orchestrator-workers ([Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)).

## 2. Hierarchical (nested teams)

Supervisors of supervisors: teams composed into teams, each level abstracting the one below. Scales the supervisor pattern past the context bottleneck.

- **Strengths:** scales to large agent counts; locality of failure (a subtree can fail without killing the whole run).
- **Failure modes:** coordination overhead grows with depth; error attribution gets murky ("which level dropped the ball?"); contracts between levels must be explicit or drift compounds.
- **Examples:** [Google ADK](https://github.com/google/adk-python) hierarchical composition, [SmolAgents](https://github.com/huggingface/smolagents) multi-agent hierarchies, [AgentScope](https://agentscope.io).

## 3. Swarm (peer-to-peer)

Agents as peers with direct communication, often with declared connection topologies. No central coordinator.

- **Strengths:** no single bottleneck; robust to individual agent failure; good for open-ended collaboration.
- **Failure modes:** coordination is emergent, not guaranteed — the MAST study ([Why Do Multi-Agent LLM Systems Fail?](https://arxiv.org/abs/2503.13657)) found inter-agent misalignment is ~a third of multi-agent failures. Needs explicit communication contracts or it degenerates into chat chaos.
- **Examples:** [Agency Swarm](https://github.com/VRSEN/agency-swarm) (declared directed flows, claims-restricted handoffs), [AG2](https://github.com/ag2ai/ag2) Network/Hub model, [CAMEL](https://github.com/camel-ai/camel) agent societies.

## 4. Graph-based (workflow graphs)

The orchestration is a static or dynamic graph: nodes are agents/steps, edges are control/data flow. Deterministic where it matters, agentic inside nodes.

- **Strengths:** inspectable, testable, replayable; natural home for retries, branching, fan-out/fan-in, and HITL gates; time-travel debugging.
- **Failure modes:** rigid graphs can't adapt to genuinely novel situations; dynamic graphs reintroduce supervisor-like routing problems.
- **Examples:** [LangGraph](https://github.com/langchain-ai/langgraph) (branching, subgraphs), [Microsoft Agent Framework](https://github.com/microsoft/agent-framework), [PydanticAI](https://github.com/pydantic/pydantic-ai) Pydantic Graph, [Mastra](https://mastra.ai/docs) workflows, [PraisonAI](https://github.com/MervinPraison/PraisonAI) AgentFlow, [LlamaIndex Workflows](https://developers.llamaindex.ai/python/framework/understanding/workflows/).

## 5. Sequential / parallel routing (pipelines)

Fixed or routed pipelines: prompt chaining, parallel fan-out with aggregation, and routing to the right specialist. The simplest "orchestration" — often all you need.

- **Strengths:** simplest to reason about, cheapest to run, easiest to evaluate step-by-step.
- **Failure modes:** no recovery from mid-pipeline failure without durable execution; routing errors cascade.
- **Examples:** [CrewAI](https://github.com/crewAIInc/crewAI) Flows, [Temporal](https://temporal.io/solutions/ai) workflows, [Inngest](https://inngest.com/) step functions, [n8n](https://docs.n8n.io) AI Agent Tool chains.
- **Anthropic's names:** prompt chaining, routing, parallelization ([Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)).

## 6. Debate / evaluator-optimizer

Multiple agents critique and refine each other's outputs — generator-critic loops, multi-agent debate, reflexion. Less "orchestration" than "deliberation architecture," but it is a coordination pattern.

- **Strengths:** measurable quality gains on reasoning tasks; self-correction without human labels.
- **Failure modes:** cost multiplies with rounds; diminishing returns after 2–3 rounds; agents can converge on shared hallucinations.
- **Examples:** see [awesome-decisions-llms](https://github.com/awesome-llms-labs/awesome-decisions-llms) (Tree/Graph of Thoughts, Reflexion, ReAct, Multiagent Debate) for the deep list.

## 7. Event-driven / actor model

Agents as actors reacting to events: message passing, pub/sub, virtual actors with persistent state. The infrastructure-native pattern.

- **Strengths:** natural durability (actors persist); scales horizontally; fits existing event infrastructure.
- **Failure modes:** debugging distributed actor systems is hard; message ordering and exactly-once semantics need care.
- **Examples:** [Dapr Agents](https://github.com/dapr/dapr-agents) (virtual actors + Dapr Workflow), [Temporal](https://temporal.io/solutions/ai), [Hatchet](https://docs.hatchet.run/home/your-first-task) (durable tasks), [Restack](https://www.restack.io/enterprise).

## Cross-cutting: durability and HITL

Every pattern above needs two things in production:

- **Durable execution** — the run survives crashes, redeploys, and long pauses. Provided by framework checkpointing ([LangGraph](https://github.com/langchain-ai/langgraph), [Microsoft Agent Framework](https://github.com/microsoft/agent-framework)) or by a durable backend ([Temporal](https://temporal.io/solutions/ai), [DBOS](https://dbos.dev/), [Inngest](https://inngest.com/), [Trigger.dev](https://trigger.dev), [Hatchet](https://docs.hatchet.run/home/your-first-task)). See [Glossary](glossary.md).
- **Human-in-the-loop** — approval gates at dangerous steps. Native in [LangGraph](https://github.com/langchain-ai/langgraph) (`interrupt` + checkpointer), AutoGen `UserProxyAgent` (ALWAYS/NEVER/AUTO modes), [n8n](https://docs.n8n.io) tool approvals, [PraisonAI](https://github.com/MervinPraison/PraisonAI) approval gates, [Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/workflow) workflows. The standalone HITL SDK [HumanLayer](https://github.com/humanlayer/humanlayer) is deprecated — don't build on it.

## When patterns break (empirical)

The MAST failure taxonomy ([arXiv:2503.13657](https://arxiv.org/abs/2503.13657), UC Berkeley — ID provisional, see README) annotated 1,642 traces across 7 frameworks: 14 failure modes in 3 buckets — **system design** (~44%), **inter-agent misalignment** (~33%), **task verification** (~23%) — with failure rates of 41–86.7% on open-source multi-agent systems. The takeaway: most multi-agent failures are *organizational-design* problems (unclear roles, missing verification, ambiguous handoffs), not model-capability problems. Pick the pattern that makes roles and verification explicit, then enforce them.
