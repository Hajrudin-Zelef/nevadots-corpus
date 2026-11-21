---
id: collect-261121-veille-ia/claude-anthropic/claude-mythos-5-1
title: "Claude Mythos 5.1"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["model", "supported", "endpoint", "claude", "mythos", "information", "more", "bedrock-runtime"]
source: https://docs.aws.amazon.com/bedrock/latest/userguide/model-card-anthropic-claude-mythos-5-1.html
source_anchor: ""
source_lines: [1, 189]
section: "Claude_Anthropic"
---
# Claude Mythos 5.1

## Model Details

Claude Mythos 5.1 is Anthropic's most capable model for cybersecurity defense and life sciences research, including threat intelligence, vulnerability discovery, red teaming, drug discovery, and biodefense screening.

Access is currently gated to a vetted set of organizations through Anthropic's trusted access programs, given the dual-use nature of these domains.

This model is a Preview and is made available to you as a "Beta Service" as defined in the AWS Service Terms. It is subject to your Agreement with AWS, the AWS Service Terms, and the applicable model EULA.

- **Model launch date:** September 1, 2026
- **EOL no sooner than:** September 1, 2027
- **Legacy period:** at least 6 months
- **Model lifecycle policy:** Model lifecycle (For Models Launched Prior to Sept 7 2026)
- **Model EOL date:** N/A
- **End User License Agreements and Terms of Use:** View
- **Model lifecycle:** Active
- **Context window:** 1M tokens
- **Max output tokens:** 128K
- **Sampling parameters:** temperature must be 1.0 or unset; top_p must be 0.99 or unset; temperature and top_p cannot be specified together; top_k is not supported
- **Reasoning:** Supported (adaptive thinking is always on and cannot be disabled; effort level configurable â low, medium, high, xhigh, max; default: high) See Adaptive thinking.
- **Preserved thinking:** Thinking blocks are bound to the model that produced them; Claude Fable 5.1 and Claude Mythos 5.1 read each other's blocks. The conversation prefix isn't checked on Claude Mythos 5.1. For more information, see Preserved thinking.
- **Knowledge cutoff:** June 2026
- **Marketplace product ID:** prod-k24dbcuykobku

| **Input Modalities** | **Output Modalities** | 
|---|---|
| Audio | Embedding | 
| Image | Image | 
| Speech | Speech | 
| Text | Text | 
| Video | Video | 

## Endpoints and APIs supported

The following tables show which endpoints and APIs are supported for Claude Mythos 5.1. For more information, see APIs supported by Amazon Bedrock and Inference endpoints supported by Amazon Bedrock.

**Endpoint support**

| **Endpoint** | **Supported** | 
|---|---|
| `bedrock-runtime` |  | 

**APIs supported on `bedrock-runtime` endpoint**

| **Messages** | **Responses** | **Chat Completions** | **Converse** | **Invoke** | 
|---|---|---|---|---|

## Capabilities and Features

__Bedrock Features__

**Features supported using `bedrock-runtime` endpoint**

__Prompt caching using `bedrock-runtime` endpoint__

For more information, see Prompt caching for faster model inference.

| **Prompt caching supported** | **Min tokens per cache checkpoint** | **Max cache checkpoints per request** | **Supported TTL** | **Fields that accept prompt cache checkpoint** | 
|---|---|---|---|---|
| Yes | 512 | 4 | 5 minutes, 1 hour | `system` ,`messages` , and`tools` | 

**Computer use using `bedrock-runtime` endpoint**

For more information, see Computer use.

| **Tool type** | **Beta header** | 
|---|---|
| `computer_20251124` | `computer-use-2025-11-24` | 

**Client tools and toolsets using `bedrock-runtime` endpoint**

For more information, see Tool use.

| **Tool type** | **Beta header** | 
|---|---|
| `computer_toolset_20260801` | None required | 
| `browser_toolset_20260801` | None required | 
| `bash_20250124` | None required | 
| `text_editor_20250728` | None required | 
| `memory_20250818` | None required | 

**Compaction using `bedrock-runtime` endpoint**

For more information, see Compaction.

| **Compaction supported** | **Beta value** | 
|---|---|
| Yes | `compact-2026-09-04` ,`compact-2026-01-12` | 

## Pricing

For pricing information, see the Amazon Bedrock Pricing

## Programmatic Access

Use the following model IDs and endpoint URLs to access this model programmatically. For more information about the available APIs and endpoints, see APIs supported and Endpoints supported.

| **Endpoint** | **Model ID** | **In-Region endpoint URL** | **Geo inference ID** | **Global inference ID** | 
|---|---|---|---|---|
| `bedrock-runtime` | `anthropic.claude-mythos-5-1` | N/A | `us.anthropic.claude-mythos-5-1` | `global.anthropic.claude-mythos-5-1` | 

*For example, if region is us-east-1 (N. Virginia), then the bedrock-runtime endpoint URL will be "https://bedrock-runtime.us-east-1.amazonaws.com".*

## Service Tiers

Amazon Bedrock offers multiple service tiers to match your workload requirements. **Standard** provides pay-per-token access with no commitment (set `"service_tier": "default"` or omit the field). **Priority** delivers the fastest response times for a price premium (set `"service_tier": "priority"`). **Flex** provides lower-cost access for flexible, non-time-sensitive workloads (set `"service_tier": "flex"`). **Reserved** provides dedicated throughput with a term commitment for predictable workloads; it is set at the account level rather than per request (contact your AWS account team to enable). For more information, see service tiers.

| **Standard** | **Priority** | **Flex** | **Reserved** | 
|---|---|---|---|

## Regional Availability

__Regional availability at a glance__

Amazon Bedrock offers three inference options: **In-Region** keeps requests within a single Region for strict compliance, **Geo Cross-Region** routes across Regions within a geography (such as US, EU, and APAC) while respecting data residency, and **Global Cross-Region** routes anywhere worldwide when there are no residency constraints. Refer to the Regional availability by models page for more details.

| **Region** | **In-Region** | **Geo** | **Global** | 
|---|---|---|---|
| `us-east-1` (N. Virginia) |  |  |  | 
| `us-east-2` (Ohio) |  |  |  | 
| `us-west-1` (N. California) |  |  |  | 
| `us-west-2` (Oregon) |  |  |  | 
| `ca-central-1` (Canada) |  |  |  | 
| `ca-west-1` (Calgary) |  |  |  | 
| `eu-central-1` (Frankfurt) |  |  |  | 
| `eu-central-2` (Zurich) |  |  |  | 
| `eu-north-1` (Stockholm) |  |  |  | 
| `eu-south-1` (Milan) |  |  |  | 
| `eu-south-2` (Spain) |  |  |  | 
| `eu-west-1` (Ireland) |  |  |  | 
| `eu-west-2` (London) |  |  |  | 
| `eu-west-3` (Paris) |  |  |  | 
| `ap-east-2` (Taipei) |  |  |  | 
| `ap-northeast-1` (Tokyo) |  |  |  | 
| `ap-northeast-2` (Seoul) |  |  |  | 
| `ap-northeast-3` (Osaka) |  |  |  | 
| `ap-south-1` (Mumbai) |  |  |  | 
| `ap-south-2` (Hyderabad) |  |  |  | 
| `ap-southeast-1` (Singapore) |  |  |  | 
| `ap-southeast-2` (Sydney) |  |  |  | 
| `ap-southeast-3` (Jakarta) |  |  |  | 
| `ap-southeast-4` (Melbourne) |  |  |  | 
| `ap-southeast-5` (Malaysia) |  |  |  | 
| `ap-southeast-6` (New Zealand) |  |  |  | 
| `ap-southeast-7` (Thailand) |  |  |  | 
| `il-central-1` (Tel Aviv) |  |  |  | 
| `me-central-1` (UAE) |  |  |  | 
| `me-south-1` (Bahrain) |  |  |  | 
| `af-south-1` (Cape Town) |  |  |  | 
| `sa-east-1` (SÃ£o Paulo) |  |  |  | 
| `mx-central-1` (Mexico) |  |  |  | 

__Geo inference details__

The destination Regions available to a geographic inference profile depend on the source Region. To retrieve the current routing configuration, call GetInferenceProfile from the source Region.

**Geo: US**

Geo inference ID: `us.anthropic.claude-mythos-5-1`

| **Source Region** | **Destination Regions** | 
|---|---|
| us-east-1 (N. Virginia) | us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 
| us-east-2 (Ohio) | us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 
| us-west-1 (N. California) | us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-1 (N. California), us-west-2 (Oregon) | 
| us-west-2 (Oregon) | us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 
| ca-central-1 (Canada) | ca-central-1 (Canada), us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 
| ca-west-1 (Calgary) | ca-west-1 (Calgary), us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 

## Data Retention

To use this model, you must set your data retention mode to `aws_review` or `provider_data_share` via the Data Retention API. For more information, see Amazon Bedrock abuse detection.

## Quotas and Limits

Your AWS account has default quotas to maintain the performance of the service and to ensure appropriate usage of Amazon Bedrock. The default quotas assigned to an account might be updated depending on regional factors, payment history, fraudulent usage, and/or approval of a quota increase request. For more information, see Quotas for Amazon Bedrock documentation and see the limits for the model.

## Sample Code

**Step 1 - AWS Account:** If you have an AWS account already, skip this step. If you are new to AWS, sign up for an AWS account

**Step 2 - API key:** Go to the Amazon Bedrock console

**Step 3 - Get the SDK:** To use this getting started guide, you must have Python already installed. Then install the relevant software depending on the APIs you are using.

**Step 4 - Set environment variables:** Configure your environment to use the API key for authentication.

**Step 5 - Run your first inference request:** Save the file as `bedrock-first-request.py`
