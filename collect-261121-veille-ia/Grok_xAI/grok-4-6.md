---
id: collect-261121-veille-ia/grok-xai/grok-4-6
title: "Grok 4.6"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["supported", "model", "service", "grok", "standard", "bedrock-mantle", "endpoint", "tier"]
source: https://docs.aws.amazon.com/bedrock/latest/userguide/model-card-xai-grok-4-6.html
source_anchor: ""
source_lines: [1, 173]
section: "Grok_xAI"
---
# Grok 4.6

## Model Details

Grok 4.6 is xAI's frontier model built for coding, agentic tasks, and knowledge work. It builds on previous generations of Grok with a particular focus on long-running agents and more ambitious interactive work. It offers 500K context window and configurable reasoning efforts (low, medium, high, xhigh).

- **Model launch date:** August 18, 2026
- **EOL no sooner than:** August 18, 2027
- **Legacy period:** at least 6 months
- **Model lifecycle policy:** Model lifecycle (For Models Launched Prior to Sept 7 2026)
- **Model EOL date:** N/A
- **End User License Agreements and Terms of Use:** View
- **Model lifecycle:** Active
- **Context window:** 500K tokens
- **Reasoning:** Supported (configurable: low, medium, high, xhigh)

| **Input Modalities** | **Output Modalities** | **APIs supported** | **Endpoints supported** | 
|---|---|---|---|
| Audio | Embedding | `Responses` | `bedrock-runtime` | 
| Image | Image | `Chat Completions` | `bedrock-mantle` | 
| Speech | Speech | `Converse` |  | 
| Text | Text | `Invoke` |  | 
| Video | Video |  |  | 

## Capabilities and Features

__Bedrock Features__

**Features supported using `bedrock-runtime` endpoint**

| **Supported** | **Not Supported** | 
|---|---|
|  |  | 

**Features supported using `bedrock-mantle` endpoint**

| **Supported** | **Not Supported** | 
|---|---|

## Pricing

| **Inference option** | **Input** | **Output** | **Cache read** | 
|---|---|---|---|
| In-Region | $2.20 | $6.60 | $0.55 | 
| Geo CRIS | $2.20 | $6.60 | $0.55 | 
| Global CRIS | $2.00 | $6.00 | $0.50 | 

*All prices are per 1 million tokens. Pricing shown is for the Standard tier.*

**Priority and Flex tier support:** In addition to Standard, Grok 4.6 supports the Priority and Flex service tiers. Priority is billed at **1.75x** the Standard per-token rate (a 75% premium) and Flex at **0.5x** the Standard rate (a 50% discount); apply these multipliers to the Standard rates shown above. For details on each service tier, see service tiers.

## Programmatic Access

Use the following model IDs and endpoint URLs to access this model programmatically. For more information about the available APIs and endpoints, see APIs supported and Endpoints supported.

| **Endpoint** | **Model ID** | **In-Region endpoint URL** | **Geo inference ID** | **Global inference ID** | 
|---|---|---|---|---|
| `bedrock-mantle` | `xai.grok-4.6` | `https://bedrock-mantle.{region}.api.aws/openai/v1` | Not supported | Not supported | 
| `bedrock-runtime` | `xai.grok-4.6` | Not supported | `us.xai.grok-4.6` | `global.xai.grok-4.6` | 

*For example, if region is us-west-2 (Oregon), then the bedrock-mantle endpoint URL will be "https://bedrock-mantle.us-west-2.api.aws/openai/v1". On `bedrock-runtime`, the base URL is "https://bedrock-runtime.{region}.amazonaws.com/openai/v1" and requests must name the geographic cross-Region inference ID `us.xai.grok-4.6` or `global.xai.grok-4.6` as the model.*

## Service Tiers

Amazon Bedrock offers multiple service tiers to match your workload requirements. **Standard** provides pay-per-token access with no commitment (set `"service_tier": "default"` or omit the field). **Priority** delivers faster, prioritized processing for a price premium (set `"service_tier": "priority"`). **Flex** provides lower-cost access for flexible, non-time-sensitive workloads (set `"service_tier": "flex"`). For more information, see service tiers.

| **Standard** | **Priority** | **Flex** | **Reserved** | 
|---|---|---|---|

## Regional Availability

__Regional availability at a glance__

Amazon Bedrock offers three inference options: **In-Region** keeps requests within a single Region for strict compliance, **Geo Cross-Region** routes across Regions within a geography (such as US, EU, and APAC) while respecting data residency, and **Global Cross-Region** routes anywhere worldwide when there are no residency constraints. Refer to the Regional availability by models page for more details.

Availability differs by endpoint.

**Availability using the `bedrock-mantle` endpoint**

| **Region** | **In-Region** | **Geo** | **Global** | 
|---|---|---|---|
| `us-west-2` (Oregon) |  |  |  | 
| `us-gov-east-1` (GovCloud East) |  |  |  | 

**Availability using the `bedrock-runtime` endpoint**

| **Region** | **In-Region** | **Geo** | **Global** | 
|---|---|---|---|
| `us-east-1` (N. Virginia) |  |  |  | 
| `us-east-2` (Ohio) |  |  |  | 
| `us-west-1` (N. California) |  |  |  | 
| `us-west-2` (Oregon) |  |  |  | 
| `us-gov-west-1` (GovCloud West) |  |  |  | 
| `us-gov-east-1` (GovCloud East) |  |  |  | 
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

__Geo inference details__

The destination Regions available to a geographic inference profile depend on the source Region. To retrieve the current routing configuration, call GetInferenceProfile from the source Region.

**Geo: US**

Geo inference ID: `us.xai.grok-4.6`

| **Source Region** | **Destination Regions** | 
|---|---|
| us-east-1 (N. Virginia) | us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 
| us-east-2 (Ohio) | us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 
| us-west-1 (N. California) | us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-1 (N. California), us-west-2 (Oregon) | 
| us-west-2 (Oregon) | us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 
| ca-central-1 (Canada) | ca-central-1 (Canada), us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 
| ca-west-1 (Calgary) | ca-west-1 (Calgary), us-east-1 (N. Virginia), us-east-2 (Ohio), us-west-2 (Oregon) | 

## Quotas and Limits

Your AWS account has default quotas to maintain the performance of the service and to ensure appropriate usage of Amazon Bedrock. The default quotas assigned to an account might be updated depending on regional factors, payment history, fraudulent usage, and/or approval of a quota increase request. For more information, see Quotas for Amazon Bedrock documentation and see the limits for the model.

## Sample Code

**Step 1 - AWS Account:** If you have an AWS account already, skip this step. If you are new to AWS, sign up for an AWS account

**Step 2 - API key:** Go to the Amazon Bedrock console

Grok 4.6 is an xAI model. Amazon Bedrock serves its Responses and Chat Completions APIs through OpenAI-compatible endpoints, so those examples use the OpenAI Python SDK as the client. Configure the SDK with your Amazon Bedrock API key and an Amazon Bedrock base URL as shown below; the requests invoke the xAI model on Amazon Bedrock and are not sent to OpenAI.

**Step 3 - Get the SDK:** To use this getting started guide, you must have Python already installed. Then install the relevant software depending on the APIs you are using.

**Step 4 - Set environment variables:** Configure your environment to use the API key for authentication.

On `bedrock-runtime`, name a cross-Region inference profile as the model â `us.xai.grok-4.6` or `global.xai.grok-4.6`. This model is not available for in-Region inference on that endpoint. Your IAM identity also needs `bedrock:InvokeModel` on your account's default project (`arn:aws:bedrock:{region}:{account-id}:project/default`) in addition to the inference profile.

**Step 5 - Run your first inference request:** Save the file as `bedrock-first-request.py`

## Usage Considerations and Limitations

Keep the following considerations and limitations in mind when you use Grok 4.6:

- **Reasoning effort** â Reasoning is always active by default. You can configure effort through the`reasoning` parameter:`"low"` (default),`"medium"` ,`"high"` , or`"xhigh"` . Reasoning content is encrypted and can be returned by passing`include: ["reasoning.encrypted_content"]` in the Responses API request. You can send the encrypted content back in subsequent turns to provide reasoning context for multi-turn conversations. The Chat Completions API does not return reasoning tokens.```
response = client.responses.create(
    model="us.xai.grok-4.6",
    reasoning={"effort": "high"},
    include=["reasoning.encrypted_content"],
    input="Explain quantum entanglement simply."
)
print(response.output_text)
```
