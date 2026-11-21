---
id: collect-261121-veille-ia/divers-tech/aws-step-functions-become-tools-for-ai-agents
title: "AWS Step Functions Become Tools for AI Agents"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["agents", "step", "terra", "functions", "gpt-6", "tools", "bedrock", "amazon"]
source: https://aiincontext.com/articles/aws-turns-step-functions-into-agent-tools-as-gpt-6-1-terra-hits-bedrock
source_anchor: ""
source_lines: [1, 61]
section: "Divers_Tech"
---
## Why it matters

- AWS Step Functions workflows are now available as tools for AI agents via Amazon Bedrock AgentCore Gateway, with no rebuild of existing workflows required.
- GPT-6.1 Terra is generally available on Amazon Bedrock with input and output tokens priced at least 50% lower than GPT-6.1 Sol and GPT-5.6 Terra.
- AWS cited OpenAI figures showing GPT-6.1 Terra scored 15 percentage points higher than GPT-5.6 Terra on the Agents' Last Exam evaluation.
- AWS Lambda's Seekable OCI lazy loading can cut container startup time by up to 70%; internal benchmarks showed 3–11 second startups versus a 3:42 median for a 13.2 GB image.
- AWS KMS now supports ML-KEM post-quantum key encapsulation with all three NIST FIPS 203 parameter sets on FIPS 140-3 Level 3-validated hardware.

Amazon Web Services has made AWS Step Functions workflows callable as tools by AI agents through Amazon Bedrock AgentCore Gateway, letting organisations expose established operational processes to agents without rebuilding them. The announcement, paired with the general availability of OpenAI's GPT-6.1 Terra on Amazon Bedrock, marks one of AWS's most concrete moves yet to wire AI agents directly into the software and internal systems that companies already run.

The stakes are straightforward. Enterprises have spent years encoding business logic into Step Functions — the AWS orchestration service that sequences tasks across other AWS services — and the market pressure now is to give AI agents access to that logic without large redevelopment projects. This update does exactly that, and it arrives with a lower-cost OpenAI model to run the agents doing the calling.

## What does the Step Functions integration actually change?

Under the new setup, an agent decides when to run a workflow and what input to pass, and Step Functions then executes the sequence in its original order. The underlying workflow logic does not change. AWS keeps each step executing exactly as designed, which means companies can hand agents the keys to production processes while preserving the guarantees those processes were built around.

Both existing Standard and Express workflows can be used as connector targets, and agents can also look up workflow runs, giving them visibility into executions they or other systems have triggered. The integration is delivered through Bedrock AgentCore Gateway, which connects agents to tools over the Model Context Protocol (an open standard for letting models call external tools and data sources).

The feature is available in all regions where AgentCore Gateway operates and is billed at standard rates — no separate pricing tier for the agent-to-workflow path.

AWS framed the launch as part of a broader push to connect AI agents to internal systems without major re-engineering. The company highlighted a separate example in which an engagement platform exposed roughly 400 internal API operations to agents using the same approach. That number is the clearest signal of the scale AWS expects customers to operate at: not a handful of pilot tools, but hundreds of operations opened to agent callers at once.

## How cheap is GPT-6.1 Terra on Bedrock?

Alongside the workflow update, AWS expanded Bedrock's model options with GPT-6.1 Terra, now generally available on the managed AI service. OpenAI positions the model as the everyday-workload line in the GPT-6 family, and AWS led with its pricing against neighbouring models on the platform.

The numbers matter here. Input and output tokens for GPT-6.1 Terra are priced at least 50% lower than GPT-6.1 Sol and GPT-5.6 Terra, according to AWS. That pricing gap positions Terra as the default choice for high-volume agent workloads — precisely the use case the Step Functions integration targets, where an agent may make many tool calls per task.

The model supports up to 1 million input tokens, accepts text and image input, and can be accessed through Bedrock APIs as well as through ChatGPT Work and Codex. The 1-million-token context window puts it in line with the long-context tier of current frontier models.

On performance, AWS cited OpenAI figures showing GPT-6.1 Terra scored 15 percentage points higher than GPT-5.6 Terra in the Agents' Last Exam evaluation, performed on par with GPT-6 Astra, and completed tasks more than three times faster than both. AWS explicitly noted the figures came from OpenAI rather than an independent test — a caveat worth holding onto when comparing vendor-published benchmarks.

## What is new in post-quantum security?

AWS also announced two post-quantum security developments that extend beyond the AI news.

AWS Key Management Service now supports ML-KEM (Module-Lattice Key Encapsulation Mechanism, the NIST-standardised post-quantum algorithm for establishing shared secrets) for post-quantum key encapsulation. Customers can establish a shared secret using a post-quantum algorithm with KMS keys whose private keys remain in FIPS 140-3 Level 3-validated hardware security modules. The service supports all three NIST FIPS 203 parameter sets through Encapsulate and Decapsulate functions.

AWS framed the addition around the risk often described as "harvest now, decrypt later" — the scenario in which encrypted traffic collected today could be exposed once quantum computing becomes strong enough to break current cryptographic methods.

Amazon Aurora PostgreSQL has also added post-quantum TLS key exchange on version 18 and higher. The two launches together reflect growing efforts by cloud providers to give customers tools to prepare systems for future cryptographic changes, even though no fixed deadline has been set for widespread quantum threats.

## Which platform efficiency changes shipped?

The announcement bundle included several infrastructure-level updates that will affect daily operations for AWS customers.

- **IAM Identity Centre** now supports cross-region trusted identity propagation, allowing a user's identity to travel with a request to AWS services in another region. This removes the earlier restriction that services using trusted identity propagation had to remain in the same region.
- **Amazon ECS** has added GPU sharing with NVIDIA Multi-Process Service, enabling multiple ECS tasks to run concurrently on a single NVIDIA GPU. The feature targets workloads such as small-model inference and multi-model serving, where one task may not fully use a GPU.
- **AWS Lambda** now supports Seekable OCI lazy loading for Lambda Managed Instances, allowing functions to fetch only the files needed at each stage of startup rather than pulling an entire container image first. AWS said this can cut container startup time by as much as 70%.
- **Amazon SageMaker AI** training and processing jobs get the same lazy-loading method. AWS cited internal benchmark figures showing lazy loading stayed between three and 11 seconds regardless of image size, compared with a median of three minutes and 42 seconds for a 13.2 GB image.
- **Amazon EKS Auto Mode** has added support for the Kubernetes Gateway API, allowing customers to define traffic routing for applications without installing a separate controller. Existing Ingress workloads continue to operate alongside the new support, making the change additive rather than a migration.

The lazy-loading numbers deserve attention. A startup time held steady between three and 11 seconds regardless of image size, against a median of three minutes and 42 seconds for a 13.2 GB image, represents a reduction of roughly 95% or more at the large end. For teams running cold-start-sensitive inference or short training jobs, that gap translates directly into cost and latency.

## Why this bundle matters

Taken together, the announcements show AWS concentrating on practical changes across four fronts: AI integration, model cost, security and infrastructure efficiency, with Step Functions and Bedrock at the centre of the latest updates.

The Step Functions move is the strategic core. Every workflow a company has already built becomes, in effect, a ready-made agent tool — no rebuild, no re-verification of step logic, no migration. Combined with a GPT-6 family model priced at least 50% below its siblings and benchmarked — by OpenAI's own testing — as substantially stronger on agent evaluations than its predecessor, AWS has assembled the full stack for agent-driven automation on its platform. The open question, which AWS's own caveat about vendor-published benchmarks underscores, is whether independent evaluations confirm the performance claims as these deployments scale.

Source: itbrief.com.au
