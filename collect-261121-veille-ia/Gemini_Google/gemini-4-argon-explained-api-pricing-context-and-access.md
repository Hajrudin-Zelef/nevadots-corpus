---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-explained-api-pricing-context-and-access
title: "Gemini 4 Argon Explained: API Pricing, Context and Access"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-08"]
keywords: ["argon", "gemini", "google", "model", "output", "pricing", "input", "googles"]
source: https://www.lycore.com/technology/gemini-4-argon-explained/
source_anchor: ""
source_lines: [1, 136]
section: "Gemini_Google"
---
**Gemini 4 Argon** is Google’s newest frontier reasoning model, and for developers the questions are practical: can you call it today, how much of a codebase or document set fits in one request, and what will it cost once the launch pricing ends. This guide answers those questions using only Google’s own model page, pricing page, developer guide, blog posts and model card. Where Google has not published something, we say so rather than guess.

*As of November 8, 2026. All prices are USD per million tokens from Google’s Gemini API pricing page, checked November 8, 2026.*

**Short answer:** Gemini 4 Argon (model ID gemini-4-argon) is available on the paid tier of the Gemini API, in Google AI Studio, on Google Cloud and in the Gemini app for Google AI Pro and Ultra subscribers. It has a 1,048,576-token window shared by input, thinking and output, and can generate up to roughly a million output tokens with long decode. It costs USD 2 input and USD 10 output through November 30, 2026, then USD 4 and USD 20 from December 1. It is not on the free tier.

## What Is Gemini 4 Argon?

Google describes Argon as its flagship reasoning model, built for complex software engineering, deep mathematical and scientific problem solving, autonomous multi-step agents and long-form generation. It sits at the top of the Gemini lineup, above the Flash models that most production apps use for high-volume work.

### How Google positions it

Google announced Argon on September 30, 2026, framing it around three areas: real-world software engineering, enterprise knowledge work such as legal and finance, and cybersecurity defense. The model card describes a large sparse mixture-of-experts transformer with native support for text, vision and audio input, and a knowledge cutoff of March 2026. Output is text only, so Argon does not generate images or speech, and it does not support the Live API for real-time voice sessions. Google says it trained Argon to respect system boundaries, report negative findings honestly and pause for confirmation before irreversible steps, which matters if you plan to give it tools that change data.

### What Google claims it can do

Every number in this paragraph is Google’s own. The model card reports 77.9% on DeepSWE v1.1, a long-horizon software engineering benchmark, 51.3% on Zapier’s AutomationBench and 91.7% on LVBench for long video understanding. The same table is candid that Argon trails elsewhere: on Terminal-bench 4.0 it shows Argon at 57.4% against 66.4% for Claude Opus 5.5, and on FrontierSWE v2 it shows 55.0% against 65.5% for GPT-6 Astra. Google also reports that teams using Argon in its Antigravity coding platform increased agentic code submissions by more than 35% while cutting rollbacks by nearly a third. Treat these as vendor claims and test on your own tasks; we compare the three frontier models in more depth in our Grok vs Gemini guide and in the MCP-focused comparison linked later.

## How Much Does Gemini 4 Argon Cost?

Argon launched with introductory pricing that ends on a fixed date. That makes it unusually cheap for a frontier model this month, and it means any budget you build today needs a second column for December.

### Introductory and standard rates

| Tier (per 1M tokens) | Through Nov 30, 2026 | From Dec 1, 2026 | 
|---|---|---|
| Standard input | USD 2.00 | USD 4.00 | 
| Standard output (incl. thinking) | USD 10.00 | USD 20.00 | 
| Cached input (implicit only) | USD 0.10 | USD 0.20 | 
| Batch or Flex input / output | USD 1.00 / USD 5.00 | USD 2.00 / USD 10.00 | 
| Priority input / output | USD 3.60 / USD 18.00 | USD 7.20 / USD 36.00 | 
| Free tier | Not available | Not available | 

Google lists no separate price for prompts above a length threshold, so a 900K-token request is billed at the same per-token rate as a 9K one. Thinking tokens are billed as output, and Google notes that every token the model processes counts, including internal tool system instructions and function declarations when you use grounding. Grounding with Google Search gives 5,000 free requests a month, shared across Gemini 3 and newer models, then USD 14 per 1,000 requests, with a cap of 1,500 requests a day.

### What a request actually costs

Take an agent step that sends 200,000 input tokens and gets 20,000 tokens of thinking plus answer back. That costs USD 0.60 until November 30 and USD 1.20 from December 1. A long-decode job that reads 50,000 tokens and writes 300,000 tokens costs about USD 3.10 now and USD 6.20 in December, or half that on Batch. Because thinking is billed as output, the thinking level you pick moves the bill more than the input size on many reasoning tasks, so measure tokens per task at low, medium and high before you set a default.

## Who Can Access Gemini 4 Argon Today?

Access rolled out in two phases, and the second phase is the one that matters for most teams building products.

### The rollout timeline

At announcement on September 30, Google rolled Argon out only to a set of trusted cyber defenders through its Fairwind Program, said it was taking part in the US government’s voluntary process for pre-release model access, and promised wider availability after more testing. On October 13 Google made it broadly available: through the API in Google AI Studio and the Gemini platform for developers and enterprises, in Android Studio, in Google Antigravity for Google AI Ultra subscribers, and in the Gemini app and AI Mode in Search for Google AI Pro and Ultra subscribers. On Google Cloud it is available in Gemini for business on web, desktop and mobile, and through the API under the same model ID.

### What you need to call it

- **A paid Gemini API project.** Argon is not on the free tier, so a key from an unbilled project will not reach it.
- **The model ID gemini-4-argon.** Google lists it as a stable model, last updated in October 2026.
- **A current SDK.** Google’s examples use the Interactions API in the google-genai Python and JavaScript SDKs, including background execution for long jobs.

On data use, the paid-tier pricing table answers “Used to improve our products” with “No”. Google’s Cloud blog adds that the general Argon model blocks offensive security tasks such as pentesting and exploitation, while a version without those guardrails goes only to defenders vetted through Fairwind. If your product does legitimate security testing, expect refusals and design your workflow around Google’s defensive tooling or a different model for that step.

## How Does the 1M Token Context and Long Decode Work?

Argon’s context and output limits are its most distinctive features, and also the easiest to misread.

### One shared window, not separate limits

The window is 1,048,576 tokens, and input, thinking and output all come out of the same budget. Send a 900,000-token prompt and only about 148,000 tokens remain for thinking and answer combined. That differs from models that publish a separate output cap on top of the input window, so size your retrieval to leave real headroom. Inputs can be text, images, video, audio and PDFs, and Google says agentic video understanding is now generally available on Argon.

### Default output versus long decode

A normal synchronous call defaults to 65,536 output tokens, and Google recommends keeping that default for standard tasks. If your app sets a lower limit, Google suggests raising it to at least 32,768, because Argon can spend more of the limit on thinking than earlier Gemini models, and a low cap can cut an answer short. For longer work you switch on long decode: set background to true and max_output_tokens above 65,536, up to 1,048,576 minus your input. The model then writes in hops of up to 64K tokens, each continuing where the last stopped, and Google’s servers run the continuation loop. When the job finishes you join the text from every model_output step. If you set background without a max_output_tokens value, the server stops after 64K tokens. For complex reasoning and coding, Google’s suggested starting points are 131,072 or 262,144 tokens.

Long decode suits work that genuinely needs one coherent output: a full migration plan across hundreds of files, a long technical report, or a rewrite of a large module. It is a poor fit for chat, where users will not wait minutes for an answer.

## What Changes When You Migrate an App to Argon?

Swapping the model string is the easy part. Google’s migration checklist lists several changes that will break or silently degrade an existing Gemini integration if you skip them.

### Parameters and caching

- **Sampling parameters are ignored.** Remove temperature, top_p and top_k. If you relied on low temperature for deterministic output, enforce structure with structured outputs instead.
- **Explicit context caching is not supported.** Remove cachedContents and caches.create calls. Implicit caching is on by default for prompts of at least 4,096 tokens and discounts cached input by 95% on hits, so keep stable content such as system prompts and tool definitions at the start of the prompt.
- **Thinking level replaces budgets.** Choose low, medium (the default) or high. Google recommends running your evaluations at all three and picking the lowest level that meets your bar.

### Timeouts, state and tools

Hard prompts can take several minutes, so Google asks you to raise HTTP client timeouts to at least 15 minutes or use background execution. A default 60-second gateway timeout in your own infrastructure will fail long before Argon finishes. For multi-turn and function-calling conversations, Google recommends keeping state on the server with previous_interaction_id rather than resending history. On tools, Argon supports function calling, code execution, file search, computer use, Search and Maps grounding, URL context and structured outputs, plus the Batch, Flex and Priority tiers. The Antigravity agent in Gemini Managed Agents can also run on Argon if you prefer a hosted agent loop over writing your own. For how Gemini’s tool model compares with Claude’s MCP and OpenAI’s actions, see Claude MCP vs GPT Actions vs Gemini Extensions.

## Should You Build on Argon? What It Means for Your Software

Argon is a strong option for specific workloads, not an automatic default for everything in your product. The decision usually comes down to output length, input types and how you handle the December price change.

### Where it fits well

Argon makes the most sense when a single request has to read a lot and write a lot: large refactors and code migrations, long structured reports built from many documents, and analysis that mixes PDFs, charts, audio or video. Google’s examples lean the same way, from long document synthesis in finance and legal work to multi-file code changes. If your app already runs on Google Cloud, procurement and data handling are simpler because Argon is available through the same platform. Developers who work in Antigravity can use it directly; we compare that tool with Cursor in Google Antigravity vs Cursor.

### Where a cheaper model is the better call

Classification, extraction, short summaries and chat replies rarely need a frontier reasoning model. Route those to a smaller, faster model and keep Argon for planning and hard steps. Build that routing behind one internal interface so each feature can switch models with a config change, and log input, cached, thinking and output tokens per feature so finance can see what the December 1 change does to real spend. Our guide to integrating AI into an existing web application covers that abstraction layer. At Lycore we take the same approach in our own products: LevelUp, our AI study app, builds summaries, flashcards and quizzes grounded only in a student’s own notes and checks short answers against an answer key, where grounding and validation matter more than the size of the model.

## Pros and Cons of Building on Argon

### Pros

- Long decode can produce close to a million output tokens in one background request.
- Native input for text, images, video, audio and PDFs in one shared 1M-token window.
- Low introductory pricing through November 30, and no separate surcharge for long prompts.
- Implicit caching with a 95% discount on hits and no code to manage cache objects.
- Batch and Flex at half price, plus Priority for latency-sensitive traffic.

### Cons

- Standard prices double on December 1, 2026.
- No free tier, and no explicit caching for teams that relied on it.
- The shared window means very large prompts leave little room for thinking and output.
- Hard requests can run for minutes, which forces background jobs or long timeouts.
- Sampling parameters are ignored, and the knowledge cutoff is March 2026.

## Frequently Asked Questions

### Is Gemini 4 Argon free to use?

Not through the API. Google’s pricing page marks Argon as not available on the Gemini API free tier, so you need a billed project. Through November 30, 2026, the paid rate is USD 2 per million input tokens and USD 10 per million output tokens, rising to USD 4 and USD 20 on December 1. Consumers can use Argon in the Gemini app with a Google AI Pro or Ultra subscription, and Ultra subscribers can also use it in Google Antigravity (AI Studio access is through a paid API project). Search grounding includes 5,000 free requests a month shared across Gemini 3 and newer models, but the model’s tokens are always billed, including thinking tokens.

### How big is the Gemini 4 Argon context window?

It is 1,048,576 tokens, shared by input, thinking and output. That is the key detail: there is no separate output allowance on top, so a 900,000-token prompt leaves only about 148,000 tokens for the model to think and answer. A normal call defaults to 65,536 output tokens. With long decode, using background execution and max_output_tokens above 65,536, a single response can grow to the full window minus your input, generated in hops of up to 64K tokens that Google’s servers chain together. For most apps, retrieval that sends only the relevant files or passages will beat filling the window, both on cost and on answer quality.

### What happens to my bill on December 1, 2026?

Every Argon rate doubles. Standard input moves from USD 2 to USD 4 per million tokens, output from USD 10 to USD 20, and cached input from USD 0.10 to USD 0.20. Batch and Flex move from USD 1 and USD 5 to USD 2 and USD 10, and Priority from USD 3.60 and USD 18 to USD 7.20 and USD 36. Nothing in your code has to change, but the same traffic will cost twice as much. Before the switch, export a week of usage, multiply by the new rates, and decide which features should stay on Argon, move to Batch, or move to a smaller model. Set a billing alert at the new expected level so a surprise shows up in days rather than at month end.

### Can I keep using temperature and explicit caching with Argon?

No. Google’s migration guide says temperature, top_p and top_k are ignored if you send them, so remove them to avoid confusion when you debug. Explicit context caching through cachedContents or caches.create is not supported on Argon, and Google asks you to remove those calls. Implicit caching replaces it: it is on by default for prompts of at least 4,096 tokens and discounts cached input by 95% on cache hits. To get the most from it, put stable content such as system instructions, tool declarations and shared documents at the start of the prompt and keep it identical across requests. Control output variety with clear instructions and structured outputs rather than sampling settings.

## The Bottom Line

Gemini 4 Argon is now broadly available on the paid Gemini API, Google Cloud and Google’s consumer apps, with a shared 1M-token window and a long decode mode that few models match. Its introductory pricing makes it cheap to evaluate until November 30, but plan your budget on the December rates of USD 4 input and USD 20 output. Before you move production traffic, remove sampling parameters and explicit caching, raise your timeouts, test all three thinking levels, and keep Argon for the requests that need its reach while smaller models handle the rest.

Adding long-context AI or agents to your product? Talk to Lycore about model selection, cost planning and production integration, or see our AI development services.
