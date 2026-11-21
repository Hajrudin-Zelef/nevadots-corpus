---
id: collect-261121-veille-ia/gemini-google/how-to-use-gemini-4-neon-access-routes-and-pricing
title: "How to Use Gemini 4 Neon: Access Routes and Pricing"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["gemini", "google", "neon", "model", "paid", "antigravity", "studio", "account"]
source: https://www.layer3labs.io/guides/how-to-use-gemini-4-neon
source_anchor: ""
source_lines: [1, 93]
section: "Gemini_Google"
---
# How to Use Gemini 4 Neon

Access Gemini 4 Neon across the paid Gemini API tier, Google AI Studio, Android Studio, Google Antigravity, and Gemini consumer plans.

You can use Gemini 4 Neon through the paid Gemini Application Programming Interface (API) with the model ID gemini-4-neon, in Google Antigravity, in the Gemini app, or in Gemini for business. Antigravity and the app need a paid Pro or Ultra plan. At Layer3Labs, we build custom AI agents and workflow automations for small and mid-sized businesses, so model price and routing decide which model each workflow runs on. Google released Gemini 4 Neon on November 18, 2026, for complex software engineering, knowledge work and science workloads.

There is no free tier for Gemini 4 Neon on the Gemini API. Google offers one for Gemini 4 Hydrogen, but Neon needs a paid account. Every token, reasoning step and tool declaration is billed. Eligible Fairwind Program members get Neon for cyber through a Cloud account team process.

## Every Way to Use Gemini 4 Neon

Google lists several access surfaces for Gemini 4 Neon: developer environments, consumer subscription apps, enterprise tooling and cybersecurity channels. Pick the route that matches your account and role.

| Route | Who Qualifies | What You Need | Model ID or Selector | Source | 
|---|---|---|---|---|
| Gemini API in Google AI Studio | Developers and software engineers | Paid Gemini API billing account | gemini-4-neon | Google AI Studio | 
| Android Studio | Developers | Paid Gemini API billing account | gemini-4-neon | Google launch announcement | 
| Google AI Studio for Subscribers | Google AI Pro and Ultra subscribers | Active Pro or Ultra subscription | gemini-4-neon | Google launch announcement | 
| Gemini App Drop-Down | Consumer AI users | Gemini subscriptions (Pro or Ultra) | Model menu selector | Google launch announcement | 
| Gemini for Business | Enterprise teams and corporate accounts | Gemini for business account | API or model menu selector | Google launch announcement | 
| Google Antigravity | Antigravity developers | Google Antigravity plans (Pro or Ultra) | Standalone or Orchestrator | Google Antigravity setup | 
| Fairwind Program | Eligible cybersecurity defenders | Eligible Fairwind Program membership; request access through your Cloud account team | Neon for cyber routing | Fairwind Program | 

Neon is not on the Gemini API free tier. Anyone calling the model through code or an interactive tool needs a paid Standard, Batch, Flex or Priority billing account. For the older sibling model, see how to use Gemini 4 Argon.

In Google Antigravity, Google says Neon can run standalone or act as an Orchestrator directing parallel Gemini 4 Hydrogen subagents. For agent setup, see our Google Antigravity setup guide.

## Gemini API and Model ID

To call Gemini 4 Neon, set the model identifier gemini-4-neon in your SDK client or request. Google serves it on the paid Gemini API tier in Google AI Studio and Android Studio, with no free API tier.

According to the official Gemini API pricing documentation, Google bills Gemini 4 Neon Standard paid tier usage at $3.00 per 1 million input tokens and $15.00 per 1 million output tokens. Intermediate thinking tokens generated during reasoning phases count directly as output tokens in this billing calculation.

Cached input costs $0.15 per 1 million tokens on the Standard tier, a 95 percent discount off input pricing. Neon supports implicit context caching and not manual explicit caching. For budget mechanics, see our Gemini 4 Neon pricing guide.

Other tiers exist. Batch costs $1.50 per 1 million input tokens, $7.50 per 1 million output tokens and $0.075 per 1 million cached input tokens. Flex matches those rates. Priority costs $5.40 input, $27.00 output and $0.27 cached.

Grounding has its own terms on Standard and Priority. Grounding with Google Search includes 5,000 free requests a month, shared across Gemini 3 and newer models. After that it costs $14 per 1,000 requests, capped at 1,500 a day. Grounding with Google Maps has a shared 5,000-request free tier, then $14 per 1,000 search queries. Tool system instructions and function declarations are metered as tokens.

The model features a unified context window of 1 million tokens for ingestion, alongside support for long-decode generation reaching up to 1 million tokens. Google states that data processed through paid API tiers is not used to improve Google's products.

## Is Gemini 4 Neon Free

No. Google's pricing page says Neon is not available on the free tier, so every API call needs paid billing.

That sets Neon apart from Hydrogen, which has a free API tier. Neon, Carbon and Argon are paid only. For how Google handles tiers across models, see the Gemini 4 Argon free tier answer.

As for fees, it depends on the surface. The API has no free tier for Neon. Google AI Pro and Ultra subscribers can try Neon in Google AI Studio and select it from the Gemini app drop-down. Organizations can use it through Gemini for business.

For frontier alternatives, read Gemini 4 Neon explained. To compare deep reasoning tiers, see Gemini 4 Neon vs Gemini 4 Carbon.

## Thinking Levels

Gemini 4 Neon incorporates four thinking levels. Google sets high as the default thinking level across standard completions.

Google meters thinking tokens as output tokens at $15.00 per 1 million tokens, so higher thinking settings raise request costs. Match the setting to task difficulty to avoid wasted tokens.

On the Artificial Analysis Intelligence Index, Google reports Neon at number 3 overall with a score of 61 at xhigh thinking.

Google also reports that at its high setting Neon costs roughly 50 percent to 80 percent less per task than top competing frontier models. At xhigh it reports over 36 percent less per task than competing models at their highest setting, and a 32 percent lower cost per task than Gemini 4 Carbon at default high thinking.

## Who Should Use a Different Route

Skip the paid Gemini API route if you need a zero-cost development endpoint. For an unpaid sandbox or free prototyping, use Gemini 4 Hydrogen, which has a free tier on the Gemini API.

Teams that want Vertex AI or Google Cloud deployment should not plan production around Neon yet. Google's launch post does not name either as an access surface. If you depend on Vertex AI contracts, check Google's Gemini API pricing page and the launch post for updates.

Neon for cyber is rolling out to eligible Fairwind Program members. To request access, contact your Cloud account team.

## What Would Change Our Answer

Our routing advice depends on Google's published access surfaces and tier pricing. A free API tier for Neon would change our guidance on prototyping.

An official announcement of Vertex AI or Google Cloud availability would let enterprise teams use Neon inside cloud compliance perimeters. Google's launch documentation does not state that today.

## Our Takeaway

Gemini 4 Neon suits production systems that call the paid Gemini API with gemini-4-neon, or that use Google AI Pro and Ultra plans. There is no free API tier. Budget $3.00 per 1 million input tokens and $15.00 per 1 million output tokens on Standard, with thinking tokens billed as output.

Check your subscription terms and billing status against Google's published plan documentation before migration work. Use Gemini 4 Hydrogen for free testing and everyday execution. Use Gemini 4 Neon when your applications need multi-step analysis and tool use.

To begin, open Google AI Studio, select gemini-4-neon and test baseline API calls at the default high thinking level.

## Frequently Asked Questions

- Gemini 4 Neon is available in the Gemini API through Google AI Studio and Android Studio, in Google Antigravity for paid Pro and Ultra subscribers, in the Gemini app for Pro and Ultra subscribers, and in Gemini for business. In addition, eligible cybersecurity defenders in the Fairwind Program receive access to Neon for cyber through their Cloud account team.
- Yes, using Gemini 4 Neon on the Gemini API requires a paid tier billing account, as Google does not provide a free tier for this model. Standard API rates are $3.00 per 1M input tokens and $15.00 per 1M output tokens, while app users must maintain a paid Google AI Pro or Ultra subscription to access the model.
- The official model identifier for API calls and client SDK configuration is gemini-4-neon. Developers must specify this exact string in Google AI Studio, Android Studio, and programmatic requests.

### Architect Your Gemini 4 Neon Integration

Deploying frontier reasoning models requires dependable caching setups, accurate token budgeting, and resilient agent routing. Book a consultation with Layer3Labs to map your model architecture and manage API expenses.

Book a Consultation
