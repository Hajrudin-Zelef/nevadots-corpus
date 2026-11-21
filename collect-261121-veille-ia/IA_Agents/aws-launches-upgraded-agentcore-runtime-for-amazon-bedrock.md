---
id: collect-261121-veille-ia/ia-agents/aws-launches-upgraded-agentcore-runtime-for-amazon-bedrock
title: "AWS launches upgraded AgentCore runtime for Amazon Bedrock"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["amazon", "runtime", "update", "agentcore", "agents", "bedrock", "session", "memory"]
source: https://insideweb3.news/article/aws-launches-upgraded-agentcore-runtime-for-amazon-bedrock
source_anchor: ""
source_lines: [1, 15]
section: "IA_Agents"
---
On 12 November 2026, Amazon Web Services announced an update to its AgentCore runtime that powers AI agents on Amazon Bedrock. The update is available in AWS regions across North America, South America, Europe and Asia Pacific.

The new runtime reduces agent session cold starts from nearly a minute to under five seconds, and in practice starts sessions in under two seconds regardless of image size or concurrency, according to senior engineer Siwei Shen.

## Performance gains

The update increases the maximum compute capacity for a session to 8 vCPUs and 32 GB of memory, a four‑fold increase over the previous limit. Memory consumption for idle agents can be reduced by up to 68 %, and idle agents scale to zero compute, minimising resource use during pauses.

## Data isolation and storage

Each session runs in a hardware‑isolated Firecracker MicroVM with its own identity and audit trail. Data isolation now extends to the filesystem, allowing agents to mount and save to Amazon S3 using FUSE. Per‑session scoping binds each session to its own storage folder via session‑labelled credentials.

The runtime supports container images up to 10 GB in size and provides usage‑based billing that aligns costs with actual agent work rather than idle time. Baseline memory pricing with up to four times burst capacity supports steady workloads.

The update was first reported by the AWS machine learning blog.
