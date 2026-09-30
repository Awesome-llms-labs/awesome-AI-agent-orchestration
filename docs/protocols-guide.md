# Protocols Guide

Which protocol does what, and how they compose. The short version: **MCP connects agents to tools, A2A connects agents to agents, AP2 lets agents pay, and everything else is discovery, identity, or serving.**

## The stack, bottom to top

```
┌─────────────────────────────────────────────────────────┐
│  Commerce / trust:  AP2 (payments), AITP (transactions) │
├─────────────────────────────────────────────────────────┤
│  Agent ↔ agent:     A2A, ACP(→A2A), AGNTCY, ANP, Agora  │
├─────────────────────────────────────────────────────────┤
│  Agent ↔ tool:      MCP                                  │
├─────────────────────────────────────────────────────────┤
│  Serving:           LangChain Agent Protocol (client↔agent)│
└─────────────────────────────────────────────────────────┘
```

## A2A vs MCP — the one distinction that matters

This is the most confused pairing in the ecosystem, so here is the official line ([a2a-protocol.org](https://a2a-protocol.org/latest/)): **"MCP is for agent-to-tool communication; A2A is for agent-to-agent communication. They are complementary, not replacements."**

- Use **MCP** when an agent needs to call a tool, read a resource, or use a prompt template. One agent, many tools.
- Use **A2A** when agents need to discover each other (Agent Cards), delegate tasks with a lifecycle (SUBMITTED → WORKING → COMPLETED/FAILED/CANCELED), stream partial results (Artifacts), and hold multi-turn context (`contextId`). Many agents, one task.
- A typical production topology: **A2A between your agents, MCP between each agent and its tools.** (MCP also serves memory — see [OpenMemory](https://github.com/mem0ai/mem0).)

## The agent-to-agent contenders

| Protocol | Steward | Status | Use when |
|---|---|---|---|
| [A2A](https://a2a-protocol.org/latest/) | Linux Foundation | ✅ Active, v1.0 (2026-03) | Interop between agents from different vendors — the default choice |
| [ACP](https://github.com/i-am-bee/acp) | IBM/BeeAI | ⚠️ Archived, merged into A2A | Don't — use A2A (listed for lineage) |
| [AGNTCY](https://outshift.cisco.com/blog/building-the-internet-of-agents-introducing-the-agntcy) | Cisco/Linux Foundation | ⚠️ Active (snippet-level) | You need the full stack: schema (OASF) + directory + secure messaging (SLIM) |
| [ANP](https://agent-network-protocol.com/) | Community | ✅ Active, spec 1.2 | Open-internet agents where **identity** is the hard problem (did:wba DIDs, E2E encryption) |
| [AITP](https://aitp.dev) | NEAR AI | ✅ Active | Cross-trust-boundary interaction on OpenAI-style Chat Threads |
| [Agora](https://arxiv.org/abs/2410.11905) | Oxford/Eigent (research) | ⚠️ Research only | Academic interest in negotiated meta-protocols |

**Consolidation signal (2025–2026):** IBM's ACP merged into A2A. The agent-to-agent space is converging on A2A as the interop standard, with ANP/AGNTCY covering identity and infrastructure niches. Bet on A2A for interop; treat the rest as specialized.

## Serving and commerce

- **[Agent Protocol (LangChain)](https://github.com/langchain-ai/agent-protocol)** — the REST/OpenAPI contract for *serving* an agent: Runs, Threads, Store. This is what your frontend or orchestrator calls to run an agent — not how agents talk to each other.
- **[AP2](https://ap2-protocol.org/)** — when agents spend money: cryptographically provable user authorization via Verifiable Digital Credentials. Under FIDO Alliance standardization (since April 2026). Authorizes; doesn't settle — bring your own rails.

## Naming collisions (read carefully)

- **Two ACPs:** IBM/BeeAI's *Agent Communication Protocol* (archived, merged into A2A) vs Zed's *Agent Client Protocol* ([agentclientprotocol.com](https://agentclientprotocol.com)) — a JSON-RPC protocol for editors ↔ coding agents, adopted by VS Code, JetBrains, and Cline. Different projects, same acronym.
- **Many "OpenMemory"s:** this list covers only mem0's canonical one (in `mem0ai/mem0`); several unrelated forks reuse the name.

## Further reading

- [A Survey of AI Agent Protocols](https://arxiv.org/abs/2504.16736) (Yang et al., 2025) — the comprehensive two-dimensional classification.
- [LLM-Based Multi-Agent Systems](https://arxiv.org/abs/2411.14033) (Yang et al., 2024) — proposes a preliminary LaMAS protocol covering technical, privacy, and business requirements.
