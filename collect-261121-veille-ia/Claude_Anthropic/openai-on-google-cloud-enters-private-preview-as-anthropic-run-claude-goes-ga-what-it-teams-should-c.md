---
id: collect-261121-veille-ia/claude-anthropic/openai-on-google-cloud-enters-private-preview-as-anthropic-run-claude-goes-ga-what-it-teams-should-c
title: "OpenAI on Google Cloud Enters Private Preview as Anthropic-Run Claude Goes GA: What IT Teams Should Check"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["google", "cloud", "openai", "claude", "anthropic", "gpt-6", "models", "says"]
source: https://digitalmatters.me/it-infrastructure/openai-claude-google-cloud/
source_anchor: ""
source_lines: [1, 149]
section: "Claude_Anthropic"
---
**OpenAI on Google Cloud** arrived on October 21, 2026, when Google put GPT-6 Astra, GPT-6.1 Sol and GPT-6 Luna in its Model Garden as a private preview, according to Google Cloud’s announcement. The next day, Anthropic made a second Claude option on Google Cloud generally available, per Anthropic’s launch article. This one is run by Anthropic itself and billed through Google.

Both moves put models from three makers on one Google Cloud invoice. This piece covers what shipped, how the two Claude versions differ, and why the billing matters.

The short version: the OpenAI models are a private preview for select enterprise customers, with no published prices and with zero data retention still "coming soon," according to Google. OpenAI says general availability is planned "in the coming weeks." Anthropic-run Claude is generally available with the full Claude API feature set, and Anthropic processes your data. If your organization has a Google Cloud commitment, both count toward it. None of the three companies says anything about promotional or nonprofit credits. Facts here are as of October 22, 2026.

## What Google and OpenAI announced for OpenAI on Google Cloud

Google’s October 21 announcement lists three OpenAI models:

- **GPT-6 Astra** for demanding coding, computer use, professional and research work.
- **GPT-6.1 Sol** , which Google describes as near-Astra intelligence at lower cost.
- **GPT-6 Luna** , a fast, cost-efficient model for everyday work.

We covered GPT-6 Sol and Luna at launch, and why GPT-6.1 Astra was pulled, which is why Astra is still a 6.0 model.

Google says the models are "rolling out now in private preview to select enterprise customers." Customers can use Google’s APIs and connect the Codex CLI and desktop app to Google for inference.

OpenAI’s own announcement, also dated October 21, confirms OpenAI on Google Cloud, including Codex support. It says "General availability is planned in the coming weeks" and links a request-access form. It also says "Microsoft remains OpenAI’s primary cloud partner."

Next, Google says OpenAI models are coming to Gemini for business and Google Antigravity. OpenAI names Google Antigravity and the Gemini Agent. Neither gives a date.

Google also claims to be the only hyperscale cloud offering Gemini, Claude and GPT.

## How OpenAI on Google Cloud is billed and secured

According to Google’s announcement, OpenAI usage is sold through Google Cloud Marketplace. It appears on your Google Cloud invoice, draws down existing cloud commitments and counts toward Flexible Savings Plans. OpenAI’s post confirms that enterprises with Google Cloud spend commitments can draw them down.

On data, Google says prompts and outputs stay inside your data perimeter and are never used to train foundation models. The models run on dedicated Google Cloud infrastructure, behind isolated gateways, with VPC Service Controls and unified audit logging.

Google lists zero data retention options and Confidential Compute as "coming soon." Neither Google nor OpenAI publishes prices or regions, or says who operates the serving stack day to day.

## What Anthropic made generally available

Anthropic’s October 22 article calls the new service Claude Models on Google Cloud (Anthropic managed). Its documentation calls it "the Claude API, run by Anthropic and reached through your Google Cloud project."

Per those docs, you subscribe through Google Cloud Marketplace and use Google Cloud credentials, not API keys. Anthropic meters usage in Claude Consumption Units, and the charges land on your Google Cloud bill. Anthropic sends no separate invoice.

The article lists Claude Fable 5.1, Opus 5.5, Sonnet 5.5 and Haiku 5.5, with Claude Managed Agents (beta), the Files API, code execution, web fetch, Agent Skills, the MCP connector (beta), batch processing and the Claude Console. Fast mode on Opus 5.5 is listed at $8 input and $40 output per million tokens.

Setup creates a new Anthropic organization. You cannot convert an existing one, and files, skills, batches and agents stay behind.

## Model maker runs it, your cloud bills it

Until now, Claude on Google Cloud meant one thing. Google ran Claude inside the platform it used to call Vertex AI. Google’s name-changes page says some Vertex AI products were renamed with the introduction of Gemini Enterprise Agent Platform. Anthropic’s overview calls that version Claude Models on Google Cloud (Google managed).

The new option splits two jobs. Google handles access control and the bill. Anthropic runs the service and is the data processor. Anthropic’s docs add that some supporting services, such as its trust and safety systems, process data outside Google Cloud.

The main reason to pick the Anthropic-run version is features. The main reason to skip it is data processing. Anthropic’s article says Google managed fits customers who need Google as the data processor, have strict regional residency rules, or want Google tools such as Model Armor.

## Anthropic-managed vs Google-managed Claude

This table draws on Anthropic’s docs for the Google-managed version and the Anthropic-managed version, as of October 22, 2026.

| Question | Anthropic managed (new, GA) | Google managed (existing) | 
|---|---|---|
| Who runs it and processes data | Anthropic |  | 
| Features | Full Claude API, including Managed Agents, Files API, code execution, web fetch, Skills, MCP connector, batches | Messages, caching, thinking, tool use, citations, structured outputs; no Files API, code execution, web fetch, Skills, MCP connector, Managed Agents or Message Batches API | 
| Locations | Global, with US-only inference through a request setting | Global, US or EU multi-region, and regional for Sonnet 4.6 and older | 
| Zero data retention | On request from Anthropic; does not carry over from another Anthropic organization | Governed by Google Cloud | 
| Rate limits | Set by Anthropic | Set by Google | 
| Billing | Google Cloud Marketplace | Google Cloud, as Agent Platform usage | 

Anthropic’s Google-managed docs also say multi-region and regional endpoints cost 10% more than global ones. Anthropic’s article lists only US and global endpoints for its own version, so an EU residency requirement points to the Google-run version.

On pricing, Anthropic’s pricing page sends Google-managed buyers to Google’s price list. Neither company’s pages promise that either version matches Claude API list prices. Get a quote before you assume parity.

## Why one invoice and committed spend matter

Before these launches, using OpenAI’s models or Anthropic’s full API meant a separate contract and invoice with that company. Now Google and OpenAI say OpenAI usage draws down existing commitments. Anthropic’s docs say its usage counts toward a Google Cloud commitment "the way other Marketplace purchases do." For an IT director, that means one vendor review and one bill.

Google also says OpenAI usage counts toward Flexible Savings Plans. In an August 26 post about Gemini Enterprise usage, Google described these as spend-based commitments with 10% off for one year or 20% off for three.

Credits are different. A commitment is money you agreed to spend. A credit is money Google gives you. No announcement mentions promotional, startup or nonprofit credits. Ask your Google account team in writing whether your credits apply to these Marketplace purchases.

Anthropic’s credits have a similar limit. Its API credits page says subscriber credits, including those on discounted Nonprofit Team plans, can’t be used on Google Cloud. We covered those credits in our Claude Haiku 5.5 post.

**Before you move a client:**Anthropic-managed Claude starts a new Anthropic organization. API keys, files, skills, agents and zero data retention agreements stay behind. Raise existing discounts with your account representative before setup, because Anthropic says discounts can’t be applied to past usage.

## Private preview vs generally available

Google’s launch stage definitions say Preview offerings are "not necessarily feature-complete," carry no SLAs or support commitments, and are meant for test environments unless Google says otherwise. In a private preview, Google also picks who gets in. Treat OpenAI on Google Cloud as something to test, not something to put under a live donation form or member portal.

Anthropic’s generally available version can go to production, subject to your own review of its data terms. For OpenAI models in production now, use OpenAI’s own API or another cloud that already sells them.

## How Azure, AWS and Oracle compare

OpenAI on Google Cloud is not the first cloud deal for these models. This table uses vendor pages as of October 22, 2026.

| Cloud | OpenAI models | Claude | 
|---|---|---|
| Google Cloud | GPT-6 Astra, 6.1 Sol and 6 Luna in private preview | Google-run, plus Anthropic-run (GA October 22) | 
| Microsoft Azure | Hosted and operated by Azure in Microsoft Foundry, including GPT-6 models | Anthropic-run in Microsoft Foundry, billed through Azure Marketplace | 
| AWS | Bedrock Managed Agents powered by OpenAI, in preview | AWS-run in Bedrock, plus Anthropic-run Claude Platform on AWS (GA May 11) | 

Microsoft’s Foundry docs say Azure hosts and operates the models it sells, and list `gpt-6-astra`, `gpt-6-sol`, `gpt-6-luna` and `gpt-6.1-sol`. For Claude, Anthropic’s Foundry docs describe an Anthropic-operated service that keeps prompts within Azure for Azure-hosted deployments.

On AWS, AWS announced the Anthropic-run Claude Platform on AWS on May 11. Anthropic’s docs say AWS operates the Bedrock version. AWS still labels Bedrock Managed Agents, powered by OpenAI as a preview, which we noted in our OpenAI DevDay 2026 recap.

On Oracle, OpenAI’s June 10 post, updated August 7, says Oracle customers can buy OpenAI models, Codex and ChatGPT Work through Oracle Marketplace with eligible Oracle Universal Credits, in the US for now.

## What IT teams and agencies should do with OpenAI on Google Cloud

1. **Find your commitment.** Ask finance how much Google Cloud commitment is left and when it ends.
2. **Ask about credits in writing.** Confirm whether promotional or nonprofit credits apply to Marketplace AI purchases.
3. **Pick the Claude version by data rules.** Need Google as sole processor or EU residency? Stay Google-run. Need Managed Agents, Files or Skills? Test Anthropic-run.
4. **Check your code.** Anthropic’s docs say to use the `AnthropicGoogleCloud` client in place of `AnthropicVertex` for the Anthropic-run version.
5. **Request OpenAI preview access only for testing.** Wait for prices and zero data retention before client production work.
6. **Set budgets.** One invoice helps only if someone watches it.

For the agent side, see our guide to where AI agents run. For Google platform basics, see moving from AI Studio to Vertex AI.

## Frequently Asked Questions

## Is OpenAI on Google Cloud available to everyone?

No. As of October 22, 2026, Google says GPT-6 Astra, GPT-6.1 Sol and GPT-6 Luna are rolling out in private preview to select enterprise customers. OpenAI says general availability is planned in the coming weeks. Both companies link a form to request access.

## How much does OpenAI on Google Cloud cost?

Neither Google nor OpenAI has published prices. Google says usage is billed on your Google Cloud invoice and draws down existing commitments.

## Is zero data retention available for OpenAI on Google Cloud?

Not yet. Google lists zero data retention and Confidential Compute as coming soon. It says prompts and outputs never train foundation models.

## What is Claude Models on Google Cloud (Anthropic managed)?

Per Anthropic’s docs, it is the Claude API, run by Anthropic and reached through your Google Cloud project. Google handles access and billing. It became generally available on October 22, 2026.

## How is it different from Claude on Vertex AI?

The Vertex AI version, now on Gemini Enterprise Agent Platform, is run by Google, with more location choices. The Anthropic-run version adds features such as Managed Agents, the Files API and Skills.

## Does Anthropic-managed Claude count toward my Google Cloud commitment?

Yes. Anthropic says usage counts toward a Google Cloud spend commitment the way other Marketplace purchases do. Talk to your account representative about existing discounts before setup.

## Can nonprofits use Google Cloud credits for these models?

No vendor says, for Claude or for OpenAI on Google Cloud. Ask your Google account team in writing. Anthropic’s own subscriber API credits, including on Nonprofit Team plans, can’t be used on Google Cloud.

## Can I keep Claude data in the EU on Google Cloud?

With the Google-run version, yes, through the EU multi-region endpoint at a 10% premium, per Anthropic’s docs. Anthropic lists only US and global endpoints for its own version as of October 2026.

## Where else are OpenAI and Claude sold through a cloud bill?

Azure sells OpenAI models and Anthropic-run Claude in Microsoft Foundry. AWS offers Claude in Bedrock and Claude Platform on AWS, with OpenAI-powered agents in preview. Oracle sells OpenAI models in the US.
