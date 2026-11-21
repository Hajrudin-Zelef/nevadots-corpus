---
id: collect-261121-veille-ia/gpt-openai/amazon-bedrock-managed-agents-for-openai-enters-preview
title: "Amazon Bedrock Managed Agents for OpenAI enters preview"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-09-29"]
keywords: ["openai", "agents", "preview", "bedrock", "service", "amazon", "managed", "models"]
source: https://dev.to/techaiwire/amazon-bedrock-managed-agents-for-openai-enters-preview-4c9k
source_anchor: ""
source_lines: [1, 76]
section: "GPT_OpenAI"
---
Amazon Web Services opened a public preview of Amazon Bedrock Managed Agents, powered by OpenAI, on September 29, 2026. The service runs AI agents on OpenAI models entirely inside a customer's own AWS account. Agents sign in with AWS identities instead of an OpenAI API key, so companies can use their existing AWS permission and audit tools on agent work.

An AI agent is a program that uses a language model to plan and carry out multi-step tasks, such as reading a codebase or processing documents. Amazon Bedrock is AWS's service for running AI models from several vendors.

According to the AWS Weekly Roundup of October 5, the service is built on "a customized version of OpenAI's Agents API," engineered to be AWS-native. AWS says the agents use "the identities, permissions, and governance controls you already use." OpenAI's own Agents API added computer use at DevDay on the same day the preview opened.

The service splits the work in two. Bedrock manages the conversation, calls the model and picks which tool to use. The tools themselves run on compute you provide. That can be your own machine or container, or Amazon Bedrock AgentCore Runtime, AWS's managed runtime for agent sessions.

A technical write-up by Polylane says the service combines OpenAI models, the Codex harness and AgentCore. It targets multi-step work such as investigating a codebase, processing documents and generating files.

Polylane and HighCircl describe these building blocks:

| Part | How it works in the preview | 
|---|---|
| Sign-in | Requests are signed with AWS SigV4; no OpenAI API key is needed | 
| Permissions | Each session runs under an IAM role passed as `role_arn` ; the caller needs`iam:PassRole` | 
| Endpoint | A regional `bedrock-mantle` endpoint, not`bedrock-runtime` | 
| Sessions | Durable and resumable, holding the conversation and model calls | 
| Skills | Reusable skills defined in `SKILL.md` files | 
| Tool servers | MCP servers that run over STDIO inside the execution environment | 
| Audit log | AWS CloudTrail records supported API activity | 

IAM is AWS's system for deciding who can do what. MCP, the Model Context Protocol, is a standard way to connect an agent to outside tools. Polylane's example code uses `openai.gpt-5.6-luna` as the default model and lets users override it.

The preview runs in three US regions: us-east-1 (N. Virginia), us-west-2 (Oregon) and us-east-2 (Ohio). HighCircl adds that AWS has not published a list of supported models, and that availability varies by region and account.

AWS charges nothing extra for Managed Agents itself during the preview. Customers still pay for model inference and for the AWS resources their agents use. AWS says pricing may change at general availability. None of the sources reviewed lists per-token prices for the OpenAI models in this service.

InsideAI News reports that general availability is expected in the coming months, with broader availability expected at AWS re:Invent later this year. AWS first announced a limited preview of the service on April 28, 2026, according to HighCircl and Polylane.

Both write-ups list gaps in the preview:

- No AWS console; it works through APIs and example stacks only.
- No subagents, and no programmatic tool calling, sometimes called code mode.
- No built-in long-term memory. Memory across sessions needs your own datastore.
- No cross-region inference profiles.
- Text is the only documented input.
- No customer-managed encryption keys for stored session data.
- Only local STDIO MCP servers are described, with no remote MCP.

The sources disagree on one safety feature. Polylane says the service supports human approval before consequential actions. HighCircl says approval must be enforced in your own application or tool code.

The same AWS roundup added more OpenAI models to Bedrock. They include GPT-6.1 Sol and an UltraFast mode for GPT-6 Astra, which OpenAI says gives up to 6x faster inference.

The main draw is control. A team that already runs on AWS can give an OpenAI-powered agent an IAM role, log its calls in CloudTrail and keep its tools on its own compute. InsideAI News calls that kind of control important for regulated industries. For those teams, it may be simpler than managing a separate OpenAI account and API keys.

Treat it as a preview, though. AWS says features and APIs can change, so keep production work on stable services for now. Before you build, check five points:

- 
**Region.** Your data and workloads must fit in one of the three US regions.
- 
**Approvals.** Build human sign-off into your own tool code, since the two write-ups disagree on whether the service handles it.
- 
**Memory.** Plan a datastore if agents need to remember anything between sessions.
- 
**Tools.** Remote MCP servers are not described, so tools must run as local STDIO servers in the execution environment.
- 
**Cost.** The service fee is zero for now, but model inference is billed. Track token use from the first test, because pricing may change at general availability.

The next signal to watch is re:Invent. A supported-model list, a console and published prices there would show whether the service is ready for production work.

*This article was first published on Tech AI Wire.*

Deutsch · 日本語 · Français · Español · Português

- OpenAI Agents API adds computer use in a hosted browser
- GPT-6.1 Sol nearly matches Astra at one-fifth the cost

- AWS Weekly Roundup: Amazon Bedrock Managed Agents powered by OpenAI, Q3 service availability updates, Kiro workflows, and more (October 5, 2026) - AWS News Blog
- Bedrock Managed Agents OpenAI preview: setup and limits - Polylane
- Bedrock Managed Agents with OpenAI: what AWS teams get in preview - HighCircl
- AWS Launches Bedrock Managed Agents Powered by OpenAI in Public Preview - InsideAI News

## Top comments (1)

Running the agent inside your own AWS account moves the bill. It does not lower the tokens, and per-run spend is still the number to watch.
