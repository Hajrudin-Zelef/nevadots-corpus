---
id: collect-261121-veille-ia/gemini-google/ai-updates-for-developers-nov-916-2026-fable-5-2-gemini-4-carbon-decision-models-and-what-breaks
title: "AI updates for developers, Nov 9–16 2026: Fable 5.2, Gemini 4 Carbon, decision models, and what breaks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["fable", "models", "thinking", "anthropic", "gemini", "every", "decision", "only"]
source: https://dev.to/cuelark/ai-updates-for-developers-nov-9-16-2026-fable-52-gemini-4-carbon-decision-models-and-what-14j1
source_anchor: ""
source_lines: [1, 195]
section: "Gemini_Google"
---
*By the Cuelark Team. Every release, price and date below comes from the vendor's own blog, docs or changelog, checked on 16 November 2026. The Reddit section links every thread we used.*

This week was busy for anyone shipping on top of model APIs. Anthropic and Google both put out new frontier models. Two vendors now sell "decision" models that answer closed questions instead of writing text. DeepSeek turned its API into a full application stack. And GitHub put a date on retiring six Copilot models. Here is what changed, what it costs, and which parts can break existing code.

We covered Gemini 4 Hydrogen separately in our React chat migration post, so it only comes up here where it touches other changes.

| Date (2026) | What | Why a developer should care | 
|---|---|---|
| Nov 9 | Claude Fable 5.2 and Mythos 5.2 | $8/$40 per 1M tokens. **Preserved thinking now applies to all accounts** , which can 400 your requests if you edit history | 
| Nov 9 | Meta Muse Ember on the Meta Model API | $0.10 / $0.50 per 1M tokens, also on Google Cloud, Oracle Cloud and Vercel AI Gateway | 
| Nov 10 | Gemini 4 Carbon generally available to developers | Intro price $2/$10 until Nov 30, then $4/$20. Sampling params ignored, explicit caching unsupported | 
| Nov 11 | Gemini Decisions API ( `gemini-decision-1` ) | Up to 1,000 yes/no, choice or rubric questions per call. $0.10 per 1M input, output free | 
| Nov 11 | Qwen4-Max-Preview | Two-week preview on Token Plan only. `max_tokens` now covers thinking plus answer | 
| Nov 12 | Inkling 2 Small weights | 594B total / 17B active MoE, Apache 2.0, runs in vLLM and SGLang | 
| Nov 13 | DeepSeek application API suite | Built-in web search, URL reading, file input, OCR, ASR, TTS. A safety parameter required for public apps | 
| Nov 13 | GitHub Copilot deprecations | Six models removed from Copilot on **Dec 14** | 

Anthropic released Claude Fable 5.2 and Mythos 5.2 on November 9.

- 
**Price:** $8 per million input tokens and $40 per million output, lower than 5.1. An optional fast mode generates up to 2.5× faster for $16/$80.
- 
**Speed:** output is up to 30% faster than 5.1.
- 
**Availability:** Fable 5.2 is on every platform, including AWS, Google Cloud and Azure, as`claude-fable-5-2` . Mythos 5.2's cyber capabilities go through Anthropic's Cyber Verification Program.
- 
**Which model to start with:** Anthropic still tells most teams to start on Opus 5.5 and move up to Fable only for the hardest work. Its own numbers are the main reason to look anyway: on FrontierCode 2.0, Fable 5.2 at High effort scored about level with Opus 5.5 at Max effort (45.3% vs 45.5%) for roughly half the cost per task. That's Anthropic's benchmark, not ours.
- 
**Advisor and orchestrator roles:** the post describes using Fable 5.2 as an advisor that cheaper Haiku, Sonnet or Opus models consult, or as an orchestrator that hands simpler work down to them.
- 
**Deliberate limits:** during training, Anthropic took steps to hold two research skills, accelerator kernel optimisation and improving LLM architectures, at about Opus 5.5's level. It says agentic coding and general ML work weren't affected.

One line in the launch post matters more for integrations than any benchmark: "preserved thinking now applies to all accounts". Anthropic frames it as an anti-distillation measure. The preserved thinking docs explain the practical side:

- A thinking block is bound to everything before it: `system` ,`tools` and every earlier message. If any of those change between requests, the block fails a prefix check and the request**can get a 400 error** .
- 
`prefix_mismatch_behavior` lets you choose between that 400 and having the API drop the failing blocks. The response's`input_transformations` field tells you which blocks were dropped.
- 
`thinking: {"type": "enabled"}` is now a 400 on these models. Send`"adaptive"` or leave`thinking` out.
- Thinking blocks from Fable 5.2, Mythos 5.2, Sonnet 5.5 and Haiku 5.5 only work in the account (or linked account) that produced them. Anywhere else they're silently dropped.
- Switching models is fine. The docs say to keep sending the whole history: blocks a model can't read are dropped for that request without an error, and read again when you switch back.

What tends to trip this in real apps: a timestamp or user-specific value injected into the system prompt on every turn, a tool list rebuilt in a different order, or "context management" code that trims or summarises old messages. If your chat backend does any of these, audit it before your traffic moves to the new models. Anthropic ships a helper for Claude Code users: run `claude update`, then `/claude-api preserved-thinking-migration` in the repo that builds your requests.

Google announced Gemini 4 Carbon on October 28 and made it available to developers on November 10. The What's new in Gemini 4 Carbon page is the one to read before switching:

- 
**Model and thinking:**`gemini-4-carbon` , with thinking levels low, medium and high. Medium is the default.
- 
**Price:** $2 input / $10 output per 1M tokens (cached input $0.10) through November 30, rising to $4/$20 from December 1. Carbon isn't on the free tier.
- 
**Context:** one unified 1,048,576-token window. Outputs over 65,536 tokens need`background=True` .
- 
**Migration notes from Google:**  - Raise `max_output_tokens` to at least 32,768.
  - Explicit context caching isn't supported.
  - 
`temperature` ,`top_p` and`top_k` are ignored.
  - Allow request timeouts of at least 15 minutes.
  - Use `previous_interaction_id` to chain turns.
- Raise 

The sampling-parameter change is the same one we hit with Hydrogen. If a shared config sets `temperature` for every model, Carbon will ignore it, so don't rely on it for determinism in tests.

Google's Gemini Decisions API (Nov 11) is a new kind of endpoint. `gemini-decision-1` doesn't generate prose. It scores options for up to **1,000 closed questions in one request**, over inputs of up to 256K tokens that can include text, images, audio, video and PDFs.

- 
**Question types:**`noul` (yes/no),`choice` (2 to 255 options) and`score` (rubric-based).
- 
**Call shape:**`client.decisions.create(model=..., input=[...], questions={...})` .
- 
**Price:** $0.10 per million input tokens, nothing for output, plus a no-cost tier. Google quotes 150–250 ms end to end.
- 
**Integrations:** OpenRouter, LangChain, Vercel and LiteLLM are among those listed at launch.

This is the same idea as OpenAI's Decisions API, which went GA on October 22 alongside `gpt-6-luna` according to OpenAI's changelog. For app developers the use cases are moderation flags, routing ("which tool should handle this?"), extraction checks and eval rubrics. Those are the jobs people currently do by prompting a chat model to "answer only yes or no" and parsing the reply.

DeepSeek's Nov 13 changelog adds a lot at once.

- 
**Model:**`deepseek-flash` , which now points at DeepSeek-V4.1-Flash-1017. It works across the Messages, Responses and Chat Completions formats.
- 
**Built-in tools:**  - 
`deepseek_web_search` , which switches to a deeper thinking search when thinking mode is on.
  - 
`deepseek_read_url` .
  - File input.
  - A safety-check parameter that DeepSeek says is **required for public-facing services** .
- 
- 
**Standalone APIs:** Search/Fetch, OCR, ASR and TTS, plus an open-source demo chat client.

Prices from the pricing page:

| Item | Price | 
|---|---|
| `deepseek-flash` input (cache miss) | $0.30 per 1M tokens peak, $0.15 off-peak | 
| `deepseek-flash` output | $1.20 per 1M tokens peak, $0.60 off-peak | 
| Search or fetch | $0.75 per 1,000 calls each | 
| OCR / parsing | $0.225 per 1,000 pages | 
| TTS | $0.45 per 10K characters ($0.225 during the promotion) | 
| ASR | $0.116 per hour ($0.058 during the promotion) | 

Peak hours are 01:00–04:00 and 06:00–10:00 UTC on weekdays (6:30–9:30 am and 11:30 am–3:30 pm IST). Everything else, including weekends, bills at half price. For batch jobs, scheduling around those windows is an easy 50% saving. The legacy `deepseek-v4-flash` names still work but are now served by the new model at the Flash price.

- 
**Inkling 2 Small** (Thinking Machines, Nov 12)
  - A 594B-parameter MoE with 17B active and image input, released under Apache 2.0 on Hugging Face.
  - Day-one support in vLLM, SGLang, Unsloth and TokenSpeed. Hosted APIs on NVIDIA, Fireworks, Together, Baseten, Nebius, Modal and Cloudflare.
  - Thinking Machines lists the limits itself: it isn't succinct, chat presentation is weaker, and there's no audio.
- 
**Meta Muse Ember** (Meta, Nov 9)
  - A closed-weights model on the Meta Model API at $0.10 input / $0.50 output per 1M tokens for prompts up to 200K tokens. Longer prompts cost $0.30 input, $0.012 cached and $1.50 output.
  - Also on Google Cloud, Oracle Cloud and the Vercel AI Gateway.
- 
**Qwen4-Max-Preview** (Alibaba Cloud Model Studio, Nov 11)
  - Available on the Token Plan only, with no pay-as-you-go. The preview runs until November 25 (24:00 UTC+8), then the model is retired.
  - 1M context and up to 393,216 output tokens.
  - Two API details catch people out. In Chat Completions, `max_tokens` now equals`max_completion_tokens` , so thinking and answer share one budget and a low limit can cut the answer off.`preserve_thinking` is on by default, so you're expected to send`reasoning_content` back.
  - 
`reasoning_effort` accepts low, medium and xhigh, with xhigh as the default.
- 
**OpenAI:** quiet week for the API. The API changelog has no entries between November 6 and 16. ChatGPT Wallet (Nov 13) is a consumer feature.

From the GitHub changelog:

- Fable 5.2 (Nov 9), Gemini 4 Carbon (Nov 10) and Gemini 4 Hydrogen (Nov 13) arrived in Copilot.
- 
**December 14:** six models leave every Copilot experience. Each has a suggested replacement:

| Removed | Suggested replacement | 
|---|---|
| Claude Fable 5 | Fable 5.2 | 
| Claude Haiku 4.5 | Haiku 5.5 (annual individual plans keep 4.5) | 
| Gemini 3.8 Flash | Gemini 4 Hydrogen | 
| Gemini 4 Argon | Gemini 4 Carbon | 
| Grok 4.6 | Grok 4.7 | 
| Kimi K3 | Replacement to be announced | 

- 
**December 8:** hosted Actions runners stop running ARM32 jobs on Linux and Windows. GitHub ties this to the move to Node 24, which doesn't support ARM32.
- 
**Also December 8:** Dependabot alerts switch off on most repositories with no activity in the past year. Alerts are archived, not deleted, and any push turns them back on.
- 
**Preview:** secret scanning now checks Actions workflow logs, for GitHub Enterprise Cloud customers with Secret Protection or Advanced Security.

If your team pins a model name in a Copilot setting, a custom agent or an extension, search for those six names now.

We read the main threads on each launch. These are the recurring arguments in our own words, with one short quote at most per thread.

**Fable 5.2's training limits** (r/LocalLLaMA). The original poster thought the deliberate cap on AI-research skills went too far, calling it "yet another reason" for open-weight models. In the comments, some doubted that Anthropic would hold back its internal models in the same way. Others argued the limit reads like a justification written after the fact.

**Fable 5.2 and usage limits** (r/ClaudeCode). The thread's title says it: "Fable 5.2 uses so much usage it's basically pointless to use". A large part of the discussion was about whether the $20 plan was ever meant for the top model. The most practical replies matched Anthropic's own suggestion: let Fable plan or review and let cheaper models write most of the code. One commenter claimed their weighted-token quota had effectively halved. That's a single unverified report, not something Anthropic has said.

**Preserved thinking** (r/ClaudeAI). This thread is from September 23, when preserved thinking applied only to new API accounts (Claude Help Center), but it's the clearest statement of the developer complaint: preserved thinking gets in the way of trimming or rewriting context. With the feature now on every account, that complaint applies to everyone.

**Inkling 2 Small** (r/LocalLLaMA). People joked that 594B parameters is "small" only by lab standards and won't fit most home rigs. Several made a more useful point: an Apache-licensed US model matters for companies in the US and EU that aren't allowed to use Chinese open-weight models. Early impressions put it a little behind recent DeepSeek and Haiku models.

**Gemini Decision 1** (r/GeminiAI). The first replies were "magic 8-ball" jokes. Other commenters then explained that it picks among the options you give it, which makes it a classifier, not a chatbot.

1. 
**Anthropic users:** grep your request builder for anything that mutates`system` ,`tools` or earlier messages between turns. Decide what`prefix_mismatch_behavior` should do and log`input_transformations` .
2. 
**Gemini users moving to Carbon:** raise`max_output_tokens` , set timeouts to 15 minutes or more, and drop explicit caching and sampling params. If cost matters, test before the December 1 price change.
3. 
**Classification prompts:** try a decision endpoint for any prompt that ends with "answer only yes or no". It's cheaper and you get a structured answer back.
4. 
**Copilot admins:** remove the six model names before December 14, and check for ARM32 jobs and long-idle repos before December 8.

If you're showing these models' reasoning and tool calls in a React chat UI, our agent UI components render reasoning, tool and message parts from a streaming response. They pair well with keeping history append-only, which is what preserved thinking now needs.

Vendor sources (all checked 16 November 2026):

1. Anthropic, Introducing Claude Fable 5.2 and Claude Mythos 5.2 (Nov 9)
2. Anthropic docs, Preserved thinking, and Claude Help Center, Preserved thinking: changing how the Messages API handles thinking blocks
3. Google, Gemini 4 Carbon is now available for developers (Nov 10)
4. Google AI Studio docs, What's new in Gemini 4 Carbon
5. Google, Gemini Decisions API (Nov 11)
6. OpenAI, API changelog
7. DeepSeek, API change log (Nov 13) and Models & pricing
8. Meta, Muse Ember (Nov 9)
9. Thinking Machines, Inkling 2 Small (Nov 12) and the Hugging Face model card
10. Alibaba Cloud Model Studio, Qwen4-Max-Preview
11. GitHub changelog: Claude Fable 5.2 in Copilot, Gemini 4 Carbon in Copilot, Gemini 4 Hydrogen in Copilot, Copilot deprecations in December, ARM32 runner retirement, Dependabot alerts on inactive repositories, Secret scanning in Actions logs

Reddit threads:

1. r/LocalLLaMA, Fable 5.2 not only has guardrails against AI...
2. r/ClaudeCode, Fable 5.2 uses so much usage it's basically pointless to use
3. r/ClaudeAI, The 'preserve thinking' feature breaks context management and degrades Claude's performance
4. r/LocalLLaMA, thinkingmachines/Inkling-2-Small is released
5. r/GeminiAI, Google introduces their first decision model

Prices are list prices at the time of writing and can change. Benchmark figures are the vendors' own.

## Top comments (0)
