---
id: collect-261121-veille-ia/modeles-open/haiku-5-5-is-10x-cheaper-until-its-tokenizer-catches-up
title: "Haiku 5.5 Is 10x Cheaper Until Its Tokenizer Catches Up"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2025-10-15"]
keywords: ["haiku", "tokens", "input", "prompt", "million", "model", "tokenizer", "line"]
source: https://gloss.run/post/its-tokenizer-gets-there-on-77000-of-the-old-ones
source_anchor: ""
source_lines: [1, 55]
section: "Modeles_Open"
---
Anthropic released Claude Haiku 5.5 on 7 October at $0.10 per million input tokens and $0.50 per million output, down from $1 and $5 on Haiku 4.5. Past 100,000 prompt tokens every rate on the card is five times higher, and the higher rate covers the whole request, cached tokens included.

Haiku 5.5 counts text with the newer tokenizer, which produces roughly 30% more tokens than Haiku 4.5 did for the same input. A prompt that measured 77,000 tokens on the old model lands just over the line on the new one without anyone changing a byte of it.

It is the only current Claude model priced by prompt length. Sonnet 5.5 and Opus 5.5 charge one rate across the full million-token window, OpenAI draws its line at 272,000 tokens and doubles the input price there, and Gemini 3.8 Flash has no line at all.

## The card has two rows

Up to 100,000 tokens, Haiku 5.5 costs $0.10 per million input, $0.50 output, $0.01 for a cache read and $0.125 for a five-minute cache write. Over 100,000 tokens the same four numbers are $0.50, $2.50, $0.05 and $0.625. Batch pricing halves both rows. The 1.1x surcharge for US-only inference applies to both rows too.

Anthropic's announcement puts the comparison in a footnote: Haiku 5.5 is "priced 90% lower than Claude Haiku 4.5 for requests up to 100,000 tokens" and 50% lower above that. The headline is that it "now costs around 75% less to run" on average, and that around 90% of requests to Haiku 4.5 were in the cheaper category.

The step is on the whole request. A call with 100,000 prompt tokens and 1,000 tokens of output costs $0.0105. Add one token to the prompt and it costs $0.0525.

Caching does not keep you under the line. The pricing page says a request's prompt length "counts all of its input tokens, including cache reads and cache writes," and that "a request over the threshold pays the higher prices even when part of its prompt is a cache hit." A cached prefix you were reading at a cent per million costs five cents per million on every request after the one that crosses, with "earlier requests keep the prices they were billed at."

## Where the line really is

Haiku 5.5 uses the tokenizer Anthropic introduced with Opus 4.7. The migration guide says "the same input text produces approximately 30% more tokens on Claude Haiku 5.5 than on Claude Haiku 4.5." The announcement says only that the model "uses slightly more tokens per task."

Thirty percent moves the threshold. 77,000 Haiku 4.5 tokens become about 100,100 Haiku 5.5 tokens, so a prompt-length histogram from your Haiku 4.5 logs has its expensive row starting at 77,000, not 100,000. Count your prompts again with `model` set to `claude-haiku-5-5` before trusting any estimate.

It also moves the discount. Per token, the lower row is 90% cheaper than Haiku 4.5. Per unit of text, it is 87%, because there are 1.3 times as many tokens to pay for. The upper row's 50% becomes 35%: a million Haiku 4.5 tokens cost $1 as input, and the same text costs 1.3 million Haiku 5.5 tokens at $0.50, which is $0.65.

Two other changes make a Haiku 5.5 conversation grow faster than a Haiku 4.5 one did. Adaptive thinking is on by default, so turns that had no thinking blocks before have them now. And the model keeps thinking blocks from every earlier assistant turn in context, where Haiku 4.5 kept only the latest turn's, so the docs warn that multi-turn conversations "carry more input tokens than the tokenizer change alone explains." A subagent loop that starts at 40,000 tokens and adds a few thousand a turn crosses the line sooner than the old model's count suggests, and then pays the upper row on every remaining turn.

## The gateways priced it wrong

On 9 October a user of Bifrost, an open-source LLM gateway, opened an issue because its model catalog could not represent the split. The gateway's tier boundaries are hard-coded at 128,000, 200,000 and 272,000 tokens, which are other vendors' lines. A Haiku 5.5 prompt between 100,000 and 128,000 tokens was billed entirely at the lower row, about five times under. Between 128,000 and 200,000, input and output were right but cache reads and writes were still five times under, because the 128,000 tier has no cache fields.

The reporter's point about agent traffic is the one to keep. Traffic from tools like Claude Code is dominated by cache operations, so a budget that sums the gateway's cost field undercounts exactly the workloads most likely to cross the line. A maintainer closed the issue the same day as already merged to the development branch, pending a release. If your spend dashboards come from a gateway, find out which version computed last week's numbers.

Claude Code itself made Haiku 5.5 its default Haiku model in version 2.1.293 on 7 October. The changelog line carries both rows in parentheses.

## The other vendors draw the line higher

Anthropic's pricing page says every Claude 4.6 and later model "except Claude Haiku 5.5" includes the full 1M window at standard pricing, and spells it out: "A 900k-token request is billed at the same per-token rate as a 9k-token request." Haiku 5.5 is the exception on its own vendor's page.

OpenAI's gpt-6-luna lists at the same $0.10 and $0.50 as Haiku 5.5's lower row. Its long-context prices start above 272,000 input tokens, at double the input price and one and a half times the output price. Gemini 3.8 Flash is $0.75 and $3.75 through the end of the year with no length tier; Google's 200,000-token line is on its Pro preview. Anthropic's threshold is the lowest of the three and its multiplier the steepest. Over 100,000 tokens, Haiku 5.5's list price is five times Luna's for the same token count, before any difference in how the two tokenizers count.

The Hacker News thread on the launch had 1,045 points and 488 comments by the weekend. The first objection in it was that the cutoff "is only applicable to Haiku and not Sonnet or Opus." Others compared it to OpenAI's rule, which is a 2x and 1.5x step at a line most requests never reach, where this one is a 5x step at a line a tenth of them already cross.

## The old model's floor ends on Thursday

Haiku 4.5 shipped on 15 October 2025. Anthropic's deprecation table lists its retirement as "Not sooner than October 15, 2026," which is four days from now. After that date a deprecation notice can come at any time, with 60 days' warning, so the earliest Haiku 4.5 could stop answering is mid-December. Priority Tier capacity does not carry over, because the docs say it "is not supported on Claude Haiku 5.5."

The move itself breaks the requests Haiku is most often used for. `temperature`, `top_p` and `top_k` return a 400 error at any value but the default. Assistant prefill returns a 400 "even with thinking turned off." A manual `budget_tokens` thinking budget returns a 400. The classifier that sends `temperature: 0` and prefills an opening brace to force JSON is the exact request that fails on the new model, and the Python SDK from version 1.0 removes the sampling parameters entirely, so that code raises a `TypeError` before it reaches the network.

## What to do with this

Pull the prompt-length distribution from your Haiku 4.5 logs and divide the threshold by 1.3. Everything above 77,000 old tokens is in the expensive row. Recount a sample with the token counting endpoint on `claude-haiku-5-5` to see how far off 1.3 is for your content.

For subagent loops, put a compaction or thinking-block-clearing point under 100,000 tokens, or route the long ones elsewhere. Over the line, Haiku 5.5's input is a quarter of Sonnet 5.5's $2 and its cache read is half of Sonnet's $0.10, so the gap you chose Haiku for shrinks from 20x to 4x on input and from 10x to 2x on cache reads.

Check which gateway version priced last week. Then fix the 400s, because the deprecation clock on the model you are leaving starts on Thursday.
