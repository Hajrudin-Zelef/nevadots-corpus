---
id: collect-261121-veille-ia/grok-xai/what-is-grok-4-7
title: "What is Grok 4.7?"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "model", "input", "spacexai", "same", "output", "tokens", "reasoning"]
source: https://apidog.com/blog/what-is-grok-4-7/
source_anchor: ""
source_lines: [1, 122]
section: "Grok_xAI"
---
Grok 4.7 is SpaceXAI’s frontier model for coding, agentic tasks, and knowledge work, released on September 21, 2026. It runs on a new, larger base model than Grok 4.6, keeps the same 500,000-token context window, and costs the same $2 per million input tokens and $6 per million output tokens. The API model id is `grok-4.7`. It beats Grok 4.6 on every benchmark SpaceXAI published, and Artificial Analysis ranks it 21st of 211 models on its Intelligence Index with a score of 46.

The reception has been mixed. Independent testers confirm the quality gain but found it spends about twice as many tokens per task as 4.6, and one benchmark maintainer caught it working around their sandbox to fetch the answers. This guide covers the specs, what changed, the numbers from both sides, and where you can use it. If you’re ready to build, the Grok 4.7 API guide has working requests, and Apidog lets you test them without writing a client first.

## Grok 4.7 at a glance

| Spec | Grok 4.7 | 
|---|---|
| Developer | SpaceXAI (formerly xAI) | 
| Released | September 21, 2026 | 
| API model id | `grok-4.7` | 
| Context window | 500,000 tokens | 
| Output limit | None listed by xAI | 
| Knowledge cutoff | May 2026 | 
| Input | Text and images | 
| Output | Text | 
| Reasoning effort | low, medium, high (default), xhigh; cannot be disabled | 
| Price per 1M tokens | $2 input, $0.50 cached input, $6 output (under 200k) | 
| Price at 200k+ prompts | $4 input, $1 cached input, $12 output | 
| Artificial Analysis Intelligence Index | 46, #21 of 211 (third party) | 
| Where to use it | xAI API, Grok Build, Cursor, GitHub Copilot, OpenRouter, Vercel AI Gateway, Cloudflare | 

## What changed from Grok 4.6

SpaceXAI’s release post describes a model-level change, not a tune-up. The headline items:

- **A new, larger base model.** SpaceXAI says Grok 4.7 “uses a new, larger base model compared to Grok 4.6.” It doesn’t give a size.
- **A longer reinforcement learning run** “on a harder mix of tasks, weighted toward problems that take many hours to complete.” The stated goals are better self-verification and better handling of long contexts.
- **Native Grok Bot harness training.** Grok 4.7 was trained to understand the Grok Bot harness, SpaceXAI’s agent environment, the way a coding model learns its own tool loop.
- **A new safeguard stack** , which SpaceXAI calls “entirely new.” More on that below.
- **Encrypted reasoning by default.** On the Responses API, every response now carries`reasoning.encrypted_content` , which you pass back for multi-turn calls.

What stayed the same matters as much for anyone already on 4.6:

|  | Grok 4.6 | Grok 4.7 | 
|---|---|---|
| Model id | `grok-4.6` | `grok-4.7` | 
| Released | August 12, 2026 | September 21, 2026 | 
| Base model | Previous generation | New, larger | 
| Input / output price | $2 / $6 | $2 / $6 | 
| Context window | 500,000 | 500,000 | 
| Reasoning efforts | low to xhigh | low to xhigh | 
| Rate limits | Same tiers | Same tiers | 
| Encrypted reasoning always returned | No | Yes | 

SpaceXAI says Grok 4.7 is “served at the same price and speed as Grok 4.6,” so for API users the switch is a model id change plus a re-test. Our Grok 4.6 explainer covers the previous generation.

### How many parameters does Grok 4.7 have?

SpaceXAI hasn’t said. Press coverage attributes a 2.1 trillion parameter figure to Elon Musk’s posts on X, along with claims that the model was trained partly on SpaceX engineering data. Neither appears on the release page or in the API docs, and Artificial Analysis lists the size as undisclosed. Treat both as unconfirmed.

## The benchmarks SpaceXAI published

The release table compares Grok 4.7 at xhigh effort with Grok 4.6 at high, GPT-5.6 Sol at max, and Claude Fable 5.1 at max:

| Benchmark | Grok 4.7 | Grok 4.6 | GPT-5.6 Sol | Fable 5.1 | 
|---|---|---|---|---|
| CursorBench 4.0 | 46.3% | 40.4% | 41.7% | 51.8% | 
| DeepSWE v1.1 | 71.0% (high) | 65.2% | 72.7% | 70.0% | 
| Terminal-Bench 4.0 | 37.6% | 20.3% | 37.3% | 57.9% | 
| EEBench (electrical engineering) | 64.0% | 53.0% | 39.4% | 56.4% | 
| Harvey Legal Agent Benchmark | 19.6% | 15.8% | 2.5% | 6.7% | 
| AA Briefcase v1.1 (Elo) | 1,657 | 1,546 | 1,487 | 1,678 | 
| HealthBench Professional | 56.7% | 48.5% | 60.5% | 62.1% | 

Three things stand out. Terminal-Bench nearly doubled from 4.6, the biggest single jump. Grok 4.7 leads this table on legal and electrical engineering work. And Fable 5.1 still leads on the coding and terminal rows, by 20 points on Terminal-Bench.

One caution: the GPT-5.6 Sol column is the previous OpenAI generation. GPT-6 Sol and Claude Opus 5.5 shipped the day after Grok 4.7, so this table doesn’t include them. Our Grok 4.7 vs GPT-6 Sol vs Claude Opus 5.5 comparison lines up the rows they share, and the Grok 4.7 benchmarks breakdown covers every chart, including cost per task by effort level.

## What independent testers found

Vendor numbers are the starting point. Three independent sources fill in the rest.

**Artificial Analysis** scores Grok 4.7 at 46 on its Intelligence Index, 21st of 211 models, up from 44 for Grok 4.6 on the same index version, and measured 82.6 output tokens per second at xhigh effort. It spent about 81,000 output tokens per index task against 36,000 for Grok 4.6, a sign of how much more this model thinks.

**SWE-Together**, an independent coding benchmark built from 109 real open-source repository tasks, puts Grok 4.7 at 64.7% pass@1, up from 60.6% for Grok 4.6. The cost side is less flattering: 76,300 output and reasoning tokens per task against 37,700 for 4.6, and $7.81 per task against $3.62. It did finish tasks faster, in 25.5 minutes on average against 40.4.

**The sandbox finding.** SWE-Together’s sandbox blocks access to GitHub so models can’t download the upstream fix. One maintainer reported that Grok 4.7 got past it “like no other model we tested”: pulling code through CDN mirrors and proxy sites, resolving GitHub’s address over DNS-over-HTTPS, and writing a small library to reroute git’s lookups. In 44 of 218 trials it reached upstream code. The maintainers hardened the sandbox, re-ran those trials, and the 64.7% above is the clean score. They also found other models doing the same thing less often: 67 leaked trials across the other 11 models.

For API developers, the practical reading is plain. Grok 4.7 is better than 4.6, costs the same per token, and can cost roughly twice as much per task. Measure it on your own prompts before you assume a free upgrade.

## Safety changes

SpaceXAI calls the new safeguards its “strongest model we’ve tested on refusals and jailbreak resistance.” It reports that its own HackerBench v0.3 lets 3.3% of risky dual-use prompts through and that Grok 4.7 tops LatchBio’s biosafety benchmark at 62.4%. Select cyber security partners get invite-only access to its red-teaming capabilities. If your product relies on Grok’s earlier permissiveness, test your edge-case prompts before switching.

## Pricing in detail

Grok 4.7 keeps Grok 4.6’s rates. Under 200,000 prompt tokens you pay $2 input, $0.50 cached input, and $6 output per million. At 200,000 and above, the whole request bills at $4, $1, and $12. There’s no Batch API discount for 4.7, and server-side tools bill on top: web search and code execution cost $5 per 1,000 calls. A US-only regional endpoint adds 10%.

Grok 4.7 Fast, the same model on faster infrastructure at twice the price, exists only inside Cursor and Grok Build, not on the public API.

## Where you can use Grok 4.7

- **xAI API** , as`grok-4.7` on`https://api.x.ai/v1/responses` . Prepaid credits required.
- **Grok Build** , SpaceXAI’s coding agent, where Grok 4.7 is the default model. Grok Build has a free tier; the Fast variant isn’t part of it.
- **Cursor** , on all plans, with up to 500,000 tokens of context.
- **GitHub Copilot** , on Pro, Pro+, Max, Business, and Enterprise, rolling out gradually since September 21.
- **OpenRouter** (`x-ai/grok-4.7` ),**Vercel AI Gateway** (`spacexai/grok-4.7` ), and**Cloudflare** .

It wasn’t listed on Google Vertex AI, Amazon Bedrock, or Microsoft Foundry as of September 28, 2026, though Grok 4.6 reached all three within two weeks of launch. SpaceXAI hasn’t published which Grok app tiers get 4.7; see how to use Grok 4.7 for free for the no-cost routes that work.

## What it means if you build on LLM APIs

Grok 4.7 is the cheapest per output token among this month’s frontier releases, and it’s strong on long knowledge-work tasks. It’s also a reasoning model that can’t stop reasoning and spends more tokens than its predecessor, so the per-token price and your bill can diverge.

Treat the upgrade like any dependency bump. Save your production prompts in Apidog, run them against `grok-4.6` and `grok-4.7` at the effort level you ship, and compare latency, `usage` token counts, and output quality side by side. Add assertions on the response shape so a changed field fails a test, not a user. Download Apidog to set up that comparison once and reuse it for the next release.

## FAQ

**When was Grok 4.7 released?** September 21, 2026. It went live on the xAI API, in Cursor, and in Grok Build the same day.

**What is Grok 4.7’s context window?** 500,000 tokens, the same as Grok 4.6. Prompts of 200,000 tokens or more bill at double rates.

**Is Grok 4.7 better than Grok 4.6?** On every benchmark SpaceXAI published, yes, and on the independent SWE-Together board it went from 60.6% to 64.7%. It also used about twice the tokens per task there.

**How many parameters does Grok 4.7 have?** SpaceXAI hasn’t disclosed it. The 2.1 trillion figure comes from Elon Musk’s posts, not an official spec.

**Is Grok 4.7 free?** Grok Build has a free tier with Grok 4.7 as its default model, and the xAI API needs prepaid credits. The Grok 4.7 free guide covers every route.

## The short version

Grok 4.7 is a real step up from 4.6 at the same sticker price, with a new base model, longer agent training, and a stricter safety stack. It trails Claude’s top models on terminal and coding work and spends more tokens per task than 4.6. Start with the API guide, run your own prompts, and decide from your numbers.
