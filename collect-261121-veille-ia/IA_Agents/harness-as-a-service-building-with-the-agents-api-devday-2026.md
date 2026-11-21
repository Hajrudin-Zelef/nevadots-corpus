---
id: collect-261121-veille-ia/ia-agents/harness-as-a-service-building-with-the-agents-api-devday-2026
title: "Harness-as-a-Service: Building with the Agents API | DevDay 2026"
domain: ia-agents
role: reference
task: article
actors: []
dates: ["2026-11-13"]
keywords: ["agents", "harness", "harness-as-a-service", "model", "building", "tools", "section", "devday"]
source: https://www.sean-weldon.com/blog/2026-11-13-harness-as-a-service-building-with-the-agents-api-devday-2026
source_anchor: ""
source_lines: [1, 90]
section: "IA_Agents"
---
# Harness-as-a-Service: Building with the Agents API | DevDay 2026

As AI models like GPT-6 Astra become capable of autonomous, long-running, multi-tool work, the software infrastructure (harness) around them must evolve acco

By Sean Weldon
## Harness-as-a-Service: Building with the Agents API

## Abstract

This synthesis examines the architectural rationale behind OpenAI's `Agents API`, a managed **harness-as-a-service** for long-running autonomous agents. The central claim is that model capability and harness capability must co-evolve: where `GPT-4 Turbo` (2023) sustained tool-using workflows lasting seconds, `GPT-6 Astra` is reported to use dozens of tools effectively over hours or days, a regime in which surrounding software infrastructure - not the model - becomes the binding constraint. Four engineering problems are analyzed: decoupling the harness from arbitrary execution environments, exposing turn-level structure for developer-facing visibility, sustaining stateful sessions across context-window boundaries and server redeployments, and reconciling state after interrupted tool calls. A flight-rebooking case study illustrates skill loading, subagent concurrency limits, and human-in-the-loop approval. Implications are drawn for teams deciding whether to operate agent infrastructure themselves.

## 1. Introduction

An **agent harness** is the software layer mediating between a language model and the world: it dispatches tool calls, manages the execution environment, maintains conversational state, and determines what the model sees on each turn. Historically this layer was thin, because workloads were short-lived. The shift described here is quantitative in its premise and qualitative in its consequence.

Back in 2023, GPT-4 Turbo could use these tools to accomplish workflows that lasted maybe a few seconds. GPT-6 Astra, on the other hand, can use dozens of tools effectively for hours or days on end.


A workflow measured in seconds tolerates a harness that holds state in process memory, assumes a single machine, and treats failure as a user-visible retry. A workflow measured in days tolerates none of these. The central thesis is therefore that the `Agents API` externalizes the harness as a managed cloud service, allowing developers to specify tools, data, instructions, and environment while delegating execution, observability, and durability to the provider.

The analysis proceeds as follows. Section 2 situates the `Agents API` within prior interface generations. Section 3 treats the principal engineering problems and a worked case study. Section 4 consolidates technical findings, Section 5 discusses trade-offs and open questions, and Section 6 concludes.

## 2. Background and Related Work

Three prior interface generations inform the current design. The **Assistants API** (2023) offered hosted workflows with `File Search` and `Code Interpreter`, implicitly assuming brief, self-contained tool use - consistent with the model capabilities of that period.

The **Responses API** (2025) addressed a representational problem: sending and receiving multiple content types - text, images, and reasoning - within a single exchange. This multi-modal content model became the substrate for subsequent agentic products, including `Codex` and agentic work within ChatGPT.

The **Agents SDK** (2025) shipped a *local* harness, reflecting an assumption that developers wished to build and operate their own infrastructure. Observed behavior contradicted this: teams repeatedly requested opinionated guidance on tool selection, sandboxing, and secrets management, signaling a preference for delegation over do-it-yourself scaffolding.

As a business or developer, you may have an idea of what tools you want to use, the data, or the execution environment that the agent should use. But that doesn't automatically mean that you want to run the whole harness yourself.


These layers are characterized through a stacked metaphor: the `Responses API` as foundation, `Codex` above it giving agents "hands," and the `Agents API` as self-actualization - "layers in a cake, or elements in a Maslow's hierarchy of inference."

## 3. Core Analysis

### 3.1 Decoupling the Harness from the Execution Environment

The `Codex` harness originally assumed local execution with direct file-system access. Supporting a cloud-hosted harness required separating it from the environment commands actually run in - whether a developer-operated sandbox, an OpenAI-hosted sandbox, or a third-party provider. This introduces a routing problem: a command such as listing files must reach the correct connected environment and return quickly, while commands and results across many simultaneous sessions must remain strictly isolated. A new capability, **computer use over the API**, extends GUI-based computer use (an `Astra` capability) to OpenAI-hosted environments, generalizing the harness beyond command-line tool calls to graphical interaction.

### 3.2 Exposing Structure for Developer Visibility

Raw tool calls and model responses proved insufficient for building usable interfaces. An internal team needed to visualize tool calls and responses organized by conversational turn across extended back-and-forth sessions, but the original API did not expose turn relationships - forcing teams to reconstruct harness-internal logic themselves. OpenAI's response was to add turn information directly to the API, so that developers building monitoring UIs do not need to re-derive the harness's own bookkeeping.

### 3.3 Sustaining Long-Running, Stateful Sessions

Two distinct failure modes threaten sessions lasting hours or days. First, accumulated history can exceed the model's context window; **compaction** addresses this by carrying forward relevant information so agents continue working across window boundaries. Second, the servers hosting a session may be redeployed or crash while the agent is still running, which requires session state to persist independently of any single server instance and requests to be routed to wherever that state now lives - sometimes while the agent is still waiting on results from other machines.

This durability requirement compounds across the dependency chain: a target of **three nines of reliability** for the `Agents API` requires every dependent system - model inference, storage, tool-calling services - to match or exceed that bar, since the composite reliability of a pipeline cannot exceed its weakest link.

A related reconciliation problem arises on crash or redeploy: resumed threads must not repeat already-completed or risky actions.

Receiving no result doesn't tell us whether the action happened, but replaying the command might result in a bad outcome, too.


The example given is a file-edit command whose connection drops mid-flight: the harness cannot safely assume either success or failure, and must instead reconcile the actual environment state against what was last reported before deciding how to proceed.

### 3.4 Case Study: Airline Flight Rebooking

A demonstration scenario grounds these abstractions. A cancelled flight (AL204) requires rebooking many passengers; the demo isolates two difficult cases. A session is created via the `Agents API` in roughly a dozen lines of JavaScript, specifying an OpenAI-hosted sandbox with network access disabled, and streaming events live to a UI. The agent is equipped with a "flight rebooking skill" loaded from a local file and shared through **capability directories**, a mechanism generalizing beyond scripted skills to plugins and MCP servers.

A flight-inventory search tool returns advisory seat availability only; locking a rebooking requires explicit human approval, illustrating **human-in-the-loop** design as a deliberate constraint rather than an afterthought. The session runs in multi-agent mode with a maximum of two concurrent subagents, delegating seating and arrival checks before a proposal is published - with higher concurrency noted to increase token usage. Outputs include a final proposal, a comparison of evaluated options, and a Python script that deterministically verifies the proposal's logic against stated constraints, adding an auditable check atop the agent's own reasoning.

In the two cases presented, one passenger (Erin) required a specific airport (O'Hare), resolved by subagents checking availability and holding seats pending approval. A second case (Ava, travelling with four passengers including children) required seats together; the agent ruled out one flight with a single available seat and another with four available seats that were not contiguous, before selecting a flight satisfying the joint constraint.

## 4. Technical Insights

Several implementation-relevant findings emerge. Routing logic must treat each session's commands and results as isolated streams even when many environments are connected concurrently, since cross-session leakage would corrupt both execution and auditability. Compaction is positioned as a necessary, not optional, component of any harness intended to run beyond a single context window, implying that developers building on top of a managed harness need not implement their own summarization or truncation strategy. The reliability target of three nines is a composite constraint: a managed harness is only as reliable as its weakest dependent service, which argues against treating reliability as a property of the API surface alone. State reconciliation after interrupted tool calls - rather than blind retry - is presented as a general principle for any agent architecture that performs side-effecting actions in an external environment, since retries risk duplicate or destructive effects when the original outcome is unknown. The capability-directories abstraction (skills, plugins, MCP servers) suggests a convergence point for how tool and context delivery is packaged across otherwise distinct extension mechanisms. Finally, subagent concurrency is an explicit, user-tunable trade-off between latency/thoroughness and token cost, rather than a fixed harness behavior.

## 5. Discussion

The broader implication is that "build vs. buy" for agent infrastructure is being reframed specifically around durability and observability rather than raw tool integration, which most frameworks already provide. Developer feedback following the Agents SDK's release indicates that teams are less interested in exercising control over sandboxing or secrets management than in avoiding the operational burden of running that infrastructure reliably at scale and over long time horizons. This matches a broader industry pattern in which commodity infrastructure migrates toward managed services once correctness and reliability requirements - rather than feature requirements - become the dominant cost.

Open questions remain understated in the source material: the mechanics of compaction (what is retained, what is discarded, and how summarization interacts with tool-call provenance) are not detailed, nor are the specific criteria the state-reconciliation logic uses to decide whether a dropped connection warrants verification, retry, or rollback. The ecosystem integrations mentioned - Notion, Rakuten, Bain, and AWS `Bedrock` - suggest the managed-harness model generalizes across both consumer-facing asynchronous task assignment and enterprise simulation use cases, but the source material does not provide comparative data on reliability or cost outcomes across these deployments.

## 6. Conclusion

The `Agents API` represents a deliberate shift of the agent harness from developer-operated infrastructure to a managed cloud service, motivated by the observation that longer-horizon, more capable models expose harness-level problems - environment routing, visibility, statefulness, and failure reconciliation - that most developers would rather not solve themselves. The practical takeaway is that teams evaluating agent infrastructure should weigh their need for environment-level control against the operational cost of building durability guarantees, such as three-nines reliability and crash-safe state reconciliation, from scratch. For teams building long-running, tool-using agents, the Agents API's model of specifying tools, data, instructions, and environment while delegating execution suggests a viable default starting point before investing in custom harness infrastructure.

## Sources

- Harness-as-a-Service: Building with the Agents API | DevDay 2026 - Original Creator (YouTube)
- Analysis and summary by Sean Weldon using AI-assisted research tools

## About the Author

**Sean Weldon** is an AI engineer and systems architect specializing in autonomous systems, agentic workflows, and applied machine learning. He builds production AI systems that automate complex business operations.

Sean Weldon builds apps and custom software at 22Atlas.
