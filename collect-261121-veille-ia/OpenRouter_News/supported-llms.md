---
id: collect-261121-veille-ia/openrouter-news/supported-llms
title: "Supported LLMs"
domain: openrouter-news
role: reference
task: article
actors: []
dates: []
keywords: ["model", "availability", "models", "universal", "agent", "customer", "assistant", "workflow"]
source: https://docs.glean.com/administration/llms
source_anchor: ""
source_lines: [1, 171]
section: "OpenRouter_News"
---
Use the Model Hub to securely configure and manage access to commercial and open-source LLMs for Glean Assistant and Agents.

The Model Hub offers a curated set of leading models from providers like OpenAI, Google, Anthropic, and Amazon. You can bring your own keys for providers or use Glean’s Universal Model Key to access pre‑procured models with guardrails.

To configure which models are available and choose models for workflows, see Configure LLMs in the Model Hub.

For details on each deployment model, see Glean deployment models.

GCP hosted customers cannot use AWS hosted models, for example, Amazon Bedrock. This change expands model choice on AWS while keeping cross‑cloud data-path restrictions in place.

## Supported models

Model availability depends on your hosting environment and provider access. See Availability by hosting environment and provider.

If your organization uses the Glean Universal Model Key, **Auto** routes each Assistant request to an enabled model within your Auto mode policy, and users can also pick a specific model. To see which models Glean recommends for each task, see Review recommended models and coverage.

In the text model tables, the **Assistant availability**, **Autonomous agent availability**, and **Workflow agent availability** columns show whether users can select the model in Glean Assistant, autonomous agents, or workflow agents, with the Glean Universal Model Key or with a Customer Key:

- ✅ means users can select the model
- ❌ means users can't select the model

Autonomous agent availability also covers **Auto** steps in workflow agents when the step's **Reasoning mode** is **Fast** or **Thinking**. Workflow agent availability covers the **LLM** picker in every other workflow agent step, including **Auto** steps in **Basic** mode. With a Customer Key, the Assistant model picker appears only when model choice is enabled for your deployment, and users can select only the models your admins enable.

### Text models

The following tables list available text models in Glean, by provider:

#### OpenAI (via Azure or OpenAI)

| Model | Assistant availability |  | Autonomous agent availability |  | Workflow agent availability |  | 
|---|---|---|---|---|---|---|
|  | Universal Model Key | Customer Key | Universal Model Key | Customer Key | Universal Model Key | Customer Key | 
| GPT-5.1 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-5.2 | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | 
| GPT-5.4 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-5.4 Mini | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | 
| GPT-5.5 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-5.6 Luna | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-5.6 Sol | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-5.6 Terra | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-6 Astra | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-6 Luna | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-6 Sol | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-6.1 Sol | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| GPT-6.1 Terra | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 

#### Google Gemini (via Google Vertex AI)

| Model | Assistant availability |  | Autonomous agent availability |  | Workflow agent availability |  | 
|---|---|---|---|---|---|---|
|  | Universal Model Key | Customer Key | Universal Model Key | Customer Key | Universal Model Key | Customer Key | 
| Gemini 3.1 Flash Lite | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | 
| Gemini 3.5 Flash | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Gemini 3.5 Flash Lite | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | 
| Gemini 3.8 Flash | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Gemini 4 Argon | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Gemini 4 Carbon | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 

#### Anthropic (via Google Vertex AI or Amazon Bedrock)

| Model | Assistant availability |  | Autonomous agent availability |  | Workflow agent availability |  | 
|---|---|---|---|---|---|---|
|  | Universal Model Key | Customer Key | Universal Model Key | Customer Key | Universal Model Key | Customer Key | 
| Claude Fable 5 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Fable 5.1 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Fable 5.2 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Haiku 5.5 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Opus 4.6 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Opus 4.7 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Opus 4.8 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Opus 5 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Opus 5.5 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Sonnet 4.6 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Sonnet 5 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 
| Claude Sonnet 5.5 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 

#### Amazon

| Model | Assistant availability |  | Autonomous agent availability |  | Workflow agent availability |  | 
|---|---|---|---|---|---|---|
|  | Universal Model Key | Customer Key | Universal Model Key | Customer Key | Universal Model Key | Customer Key | 
| Amazon Nova Pro 1.0 | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | 

#### Baseten

| Model | Assistant availability |  | Autonomous agent availability |  | Workflow agent availability |  | 
|---|---|---|---|---|---|---|
|  | Universal Model Key | Customer Key | Universal Model Key | Customer Key | Universal Model Key | Customer Key | 
| DeepSeek V4.1 Flash | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | 
| GLM 5.2 | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | 
| GLM 5.3 | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | 
| GLM 5.3 Flash | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | 
| Kimi K3 | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | 
| Nemotron 3 Ultra | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | 

### Image models

Glean supports the following image models, by provider:

#### OpenAI (via Azure or OpenAI)

- GPT Image 2.5 Flare
- GPT Image 2.5 Sunburst

#### Google Gemini (via Google Vertex AI)

- Nano Banana 2
- Nano Banana Pro

Image models power image generation. Users get them by asking Glean to create an image, not by selecting them in a model picker, so the image model lists don't show availability.

### Voice models

Glean supports the following voice models, by provider:

#### OpenAI (via Azure or OpenAI)

- GPT Live 1
- GPT Realtime 1.5
- GPT Realtime 2
- GPT Realtime 2.1
- GPT Transcribe
- GPT-4o Transcribe

Voice models power real-time voice: realtime models hold the spoken conversation, and transcription models turn speech into text. Users get them by starting a voice conversation, not by selecting them in a model picker, so the voice model lists don't show availability.

### Availability and key-mode notes

To see which models users can select in Glean Assistant and Glean Agents, see the **Assistant availability**, **Autonomous agent availability**, and **Workflow agent availability** columns in the text model tables.

- **Models that run behind the scenes** are used by Glean Assistant without appearing in the model picker, such as a faster model that handles lighter steps. With the Glean Universal Model Key,**Auto** also routes each request to an enabled model on its own.
- **Customer Key admin settings** : With a Customer Key, admins choose which models power Glean features in**Admin console → Models** , including the default model. A model assigned this way can power a feature even if users can't select it. See Configure LLMs in the Model Hub and Change your default model on Customer Key.
- **Gemini 3.1 Pro Custom Tools** : In Glean Assistant, Gemini 3.1 Pro Custom Tools is the Customer Key variant of Gemini 3.1 Pro, which is available through the Glean Universal Model Key.
- **Amazon Nova Pro 1.0** is available in workflow agents with a Customer Key only on AWS-based deployments. See Availability by hosting environment and provider.
- **Open models hosted on Baseten** , such as GLM 5.3, Kimi K3, and Nemotron 3 Ultra, are available through the Glean Universal Model Key, where admins control them by creator region. See Manage open models by region. To use them with a Customer Key, contact your Glean representative to configure a custom provider.

## Availability by hosting environment and provider

Which LLM hosting providers you can configure depends on your deployment mode and key type:

- **Glean Universal Model Key** : Glean manages connectivity to all supported providers regardless of your deployment's cloud environment.
- **Customer Key** (BYOK): Access to cloud-hosted providers is limited to your own cloud because cross-cloud access is not supported. Anthropic is the exception: you connect to it directly with your own API key, so it works from either cloud.

| Hosting provider | Glean Universal Model Key | Customer Key — GCP-based deployment | Customer Key — AWS-based deployment | 
|---|---|---|---|
| OpenAI | ✅ | ✅ | ✅ | 
| Azure OpenAI | ✅ | ✅ | ✅ | 
| Google Vertex AI | ✅ | ✅ | ❌ | 
| Amazon Bedrock | ✅ | ❌ | ✅ | 
| Anthropic | ❌ | ✅ | ✅ | 

- **GCP-based deployment** includes Glean Hosted and Customer Hosted on GCP.
- **AWS-based deployment** refers to Customer Hosted on AWS.
- Anthropic (Claude) models are accessible through Google Vertex AI or Amazon Bedrock. With Customer Key, you can also connect directly to Anthropic using your own Anthropic API key, from either a GCP-based or AWS-based deployment.
- For model-level differences between key modes, see the notes under Availability and key-mode notes.

## Pricing

With Enterprise Flex pricing, each agent run uses an amount of FlexCredits determined by the complexity of an agent. This complexity includes how many connectors the agent searches, how many steps it takes, how much memory it maintains, how many tools it executes, and the model used for each step. Agents that use higher-tier models consume more credits than agents that use lower-tier models.

With Enterprise Flex pricing, everyday Glean Assistant queries that use basic and standard models don't use credits. Queries that use premium models consume credits, whether a user selects the model or **Auto** routes to it.

See the following documentation to learn about pricing dashboards:

## See also

- To configure which models are available and choose models for workflows, see Configure LLMs in the Model Hub.
- To control open-model availability by creator region, see Manage open models by region.
- To learn how Glean handles model deprecation, including notification timelines, migration paths, and actions needed for assistants and agents, see Model deprecation.
- To monitor LLM usage and reliability for Customer Key deployments, see LLM Insights.
- To change your default model on a Customer Key deployment, see Change your default model on Customer Key.
