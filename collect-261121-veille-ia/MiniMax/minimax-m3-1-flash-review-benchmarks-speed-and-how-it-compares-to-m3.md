---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-review-benchmarks-speed-and-how-it-compares-to-m3
title: "MiniMax M3.1 Flash Review: Benchmarks, Speed and How It Compares to M3"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax", "flash", "october", "tokens", "checked", "against", "model", "output"]
source: https://omniakey.com/blog/minimax-m3-1-flash-review
source_anchor: ""
source_lines: [1, 231]
section: "MiniMax"
---
MiniMax released M3.1 Flash on October 19, 2026 at $0.15 per million input tokens and $0.60 per million output, half of M3's rates, and Artificial Analysis scores it 29 on its Intelligence Index against M3's 19 (checked October 30, 2026). In five side-by-side calls it was the more careful model: it wrote a correct `merge_intervals` where M3's own test failed. It also always thinks: it wrote about 2.4 times as many output tokens and took longer on 4 of the 5 tasks.

Like M3, it is a 428B-parameter mixture-of-experts model with 23B active per token, a 1M-token context window and public weights (Hugging Face model cards, checked October 30, 2026).

## Short answer

- **Half the price per token.** $0.15 in and $0.60 out per million tokens, against M3's $0.30 and $1.20, with cached input at $0.015 against $0.06 (MiniMax pricing page, checked October 30, 2026).
- **A higher index and stronger agent scores.** Artificial Analysis scores it 29 against M3's 19, and on MiniMax's model-card table it leads Automation-Bench at 62.8 and ties Opus 5 at 76.2 on BankerToolBench.
- **Every call thinks first.** Thinking cannot be switched off, effort defaults to`max` , and in our runs it wrote 3,420 output tokens against M3's 1,408.
- **Cost per task depends on length.** On our five short calls it cost $0.00220 against M3's $0.00195; on Artificial Analysis's long index tasks it costs $0.49 against $1.29.
- **Our take:** Flash is the better default for coding and agent work. M3 still wins on short, latency-sensitive calls, where fewer tokens matter more than the lower price.

MiniMax M3.1 Flash

Higher index score (29 vs 19), half the price per token, and it got our coding task right. Every call thinks first.

MiniMax M3

Faster on 4 of our 5 calls with less than half the output tokens, and its thinking can be switched off.

## What is MiniMax M3.1 Flash?

It is M3 with three changes that MiniMax says improve inference efficiency. MiniMax M3.1 Flash arrived on October 19, 2026 (MiniMax's release notes, checked October 30, 2026), with 428B total parameters and 23B active per token, the same shape as MiniMax M3. The changes: sparse attention in place of the first three full-attention layers, quantization-aware training and DSpark speculative decoding (Hugging Face model card, checked October 30, 2026).

|  | M3.1 Flash | M3 | 
|---|---|---|
| Released | Oct 19, 2026 | Jun 1, 2026 | 
| Parameters | 428B total, 23B active | 428B total, 23B active | 
| Context | 1M tokens | 1M tokens | 
| Max output | 512K tokens | 512K tokens | 
| Thinking | Always on | On, adaptive, off | 
| Price per 1M | $0.15 in, $0.60 out | $0.30 in, $1.20 out | 

Dates come from MiniMax's release notes, sizes from the Hugging Face model card, prices from MiniMax's pricing page, all checked October 30, 2026. Both models accept 1M tokens of context and recommend 131,072 output tokens, with 524,288 as the maximum (MiniMax's chat API reference, checked October 30, 2026).

Before launch, MiniMax ran it anonymously as Space Bunny, where it processed more than 200 trillion tokens (MiniMax's launch post, October 19, 2026). From October 19, requests to M2.5 and M2.7 under M Plan and Token Plan go to M3.1 Flash automatically, and those two models retire on November 18, 2026 (MiniMax's model FAQ, checked October 30, 2026).

The same launch post says M3.1 Flash makes online inference "nearly three times faster" and reduces usage costs by 70%.

The weights are public at the Hugging Face repo MiniMaxAI/MiniMax-M3.1-Flash under the MiniMax Community License (checked October 30, 2026).

## What benchmarks did MiniMax publish?

MiniMax published two separate sets of numbers. The richer one is on the M3.1 Flash model card, which compares it with Opus 5, GPT-5.6 Sol and four other models, but not with M3 (Hugging Face, checked October 30, 2026).

| Benchmark | M3.1 Flash | Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|
| Automation-Bench v1.0.6 | 62.8 | 50.3 | 45.8 | 
| BankerToolBench | 76.2 | 76.2 | 74.6 | 
| Terminal Bench 4.0 | 38.1 | 51.8 | 39.9 | 
| DeepSWE v1.1 | 73.7 | 74.0 | 73.0 | 
| NL2Repo-Bench | 61.2 | 75.3 | 56.8 | 
| SWE Refactor Bench | 23.0 | 31.0 | 28.5 | 
| OSWorld 2.0 | 61.7 | 70.6 | 65.7 | 
| HLE (without tools) | 43.4 | 55.6 | 49.5 | 

It leads Automation-Bench v1.0.6 at 62.8, where Opus 5 scores 50.3 and GPT-5.6 Sol 45.8. It ties Opus 5 on BankerToolBench at 76.2. One model in the same table, DeepSeek V4.1 Flash-1017, beats it on three coding rows: 42.3 on Terminal Bench 4.0 against 38.1, 77.0 on DeepSWE against 73.7, and 68.2 on NL2Repo-Bench against 61.2.

MiniMax's launch post says it "ranks first among Chinese models" on OSWorld V2 and Agents' Last Exam. In the model card's table, Opus 5 scores 70.6 and GPT-5.6 Sol 65.7 on OSWorld 2.0 against Flash's 61.7, and Opus 5 (32.2) and GPT-5.6 Sol (30.6) both sit above its 29.6 on Agents' Last Exam.

For M3, the numbers come from its own launch on June 1, 2026:

- SWE-Bench Pro: 59.0%
- Terminal-Bench 2.1: 66.0%
- MCP Atlas: 74.2%

Those are different tests from the rows above, so the two sets cannot be compared row by row.

One Hacker News commenter, DCKing, wrote that the model "seems to be pretty benchmaxed" and that "in actual use, it doesn't survive the comparison to models in the benchmark" (October 19, 2026).

## How fast is it?

Per token it is quick; per answer it is often slower. MiniMax says output reaches 200+ tokens per second, with a median of 170+, and credits more aggressive sparsity. Artificial Analysis, measuring MiniMax's own API, recorded 118.9 tokens per second against M3's 101.0, and a first token at 1.36 s against 1.11 s (checked October 30, 2026).

Our runs show where the time goes. Each cell is total time, then time to first token, in seconds; the explain prompt ran twice per model, and the tool call was not streamed.

| Task | M3.1 Flash | M3 | 
|---|---|---|
| Count | 12.4 / 8.3 | 7.2 / 6.5 | 
| Code | 25.2 / 16.1 | 6.2 / 3.3 | 
| Explain, run 1 | 14.3 / 6.8 | 10.1 / 2.9 | 
| Explain, run 2 | 7.4 / 2.5 | 13.1 / 6.0 | 
| Tool call | 9.6 / – | 2.6 / – | 

We measured these on October 30, 2026. After the first token, Flash streamed at roughly 76 to 239 tokens per second against M3's 67 to 107, so raw token rate was not the problem. The thinking was: every streamed Flash call returned `reasoning_content`, and those tokens count toward the total. M3 returned no thinking text on any call.

Hacker News commenter chriddyp, who ran the model on their own data-analytics benchmark, wrote that it "seems to use A LOT of tokens and it's the slowest model that we've tested for the exam; it's slow because of the number of tokens it uses, not the inference speed" (October 20, 2026).

## How did it compare with M3 on the same prompts?

Flash got one more task right, and it was still slower. Both models answered the arithmetic question correctly with 401, both covered all ten HTTP-caching items asked, and both made the two tool calls for Paris and Tokyo. The difference was the coding task.

Output tokens, thinking included:

| Task | M3.1 Flash | M3 | 
|---|---|---|
| Count | 312 | 64 | 
| Code | 838 | 311 | 
| Explain, run 1 | 1,034 | 479 | 
| Explain, run 2 | 1,173 | 493 | 
| Tool call | 63 | 61 | 

Flash wrote 3,420 output tokens across the five tasks against M3's 1,408. The visible answers were close: 377 and 425 words for Flash's two explanations, 364 and 381 for M3's, while Flash's thinking text before them was another 394 and 496 words.

The coding task is the one that decided it. Flash returned a correct `merge_intervals` with nine asserts, including a check that the input list is unchanged, and the file ran clean. M3's merging logic was also right, but it sorts intervals into tuples and only converts back to a list when it merges one, so `merge_intervals([[5, 10]])` returns `[(5, 10)]`. Its own second assert, `assert merge_intervals([[5, 10]]) == [[5, 10]]`, fails.

Lowering effort helps the wait. With `reasoning_effort: "low"`, one extra Flash run took 5.5 s instead of 25.2 s, reached its first token at 2.4 s instead of 16.1 s and wrote 429 tokens instead of 838. The function was correct, but one of its own asserts expected two intervals that do not overlap to merge, so that file stopped with an `AssertionError`.

Flash's thinking reads like clipped, telegraphic English. Its count-task thinking began, verbatim: "We need answer. Need parse subtle: divisible by 3 or by 5, but not by 15." Hacker News commenter simonw reported the same style, calling it "Reasoning traces use truncated English" (October 19, 2026).

For this MiniMax M3.1 Flash review we tested text only: image and video input, long-context prompts, longer agent runs and effort levels other than default and low are untested, and we called both through a single OpenAI-compatible endpoint rather than MiniMax's own API.

## What does it cost per task?

Half the price per token did not make it cheaper on short prompts. On our five calls Flash cost $0.00220 against M3's $0.00195, because it wrote about 2.4 times as many output tokens. M3 cost less on four of the five tasks; Flash cost less on the tool call.

| Task | M3.1 Flash | M3 | 
|---|---|---|
| Count | $0.000199 | $0.000116 | 
| Code | $0.000526 | $0.000434 | 
| Explain, run 1 | $0.000662 | $0.000620 | 
| Explain, run 2 | $0.000730 | $0.000607 | 
| Tool call | $0.000080 | $0.000173 | 
| Total | $0.00220 | $0.00195 | 

Prices use MiniMax's list rates, with cached input at the cache rate.

On long tasks the picture flips. Artificial Analysis's blended price is $0.10 per million tokens against $0.22 for M3, on a 7:2:1 mix of cache hits, input and output, and its average cost to complete one Intelligence Index task is $0.49 against $1.29 (checked October 30, 2026). It also calls Flash very verbose: 160M output tokens across the index against a median of 100M, with M3 at 100M.

One more difference: Flash is flat at $0.15 input, $0.60 output and $0.015 for cached input per million tokens. M3 costs $0.30, $1.20 and $0.06 after MiniMax's permanent 50% discount, for prompts up to 512K input tokens; above that it is $0.60, $2.40 and $0.12. M3 also sells a Priority tier at 1.5 times those rates, turned on with `service_tier: "priority"`, and Flash has no Priority tier (MiniMax pricing page, checked October 30, 2026).

## Should you use M3.1 Flash or M3?

Our MiniMax M3.1 Flash review verdict: Flash is the better default for coding and agent prompts.

| If you... | Pick | Why | 
|---|---|---|
| Code and agent tasks | M3.1 Flash | Passed where M3 failed | 
| Want short, fast calls | M3 | Fewer tokens, less waiting | 
| Need thinking off | M3 | Flash returns an error | 
| Send prompts over 512K | M3.1 Flash | Its price stays flat | 
| Want faster response times | M3 | Priority tier at 1.5x | 

On our short prompts M3 was faster on four of the five calls and cheaper on four of the five, and the explanations were equally complete. The reason to pick Flash is accuracy on work you would otherwise have to redo: its code file passed all nine asserts, and M3's did not.

In a companion test in the same batch, M3.1 Flash ran as a custom model in Factory Droid and fixed a median bug in 30.7 seconds.

If you pick models for Claude Code, best model for Claude Code compares the Claude options. GLM-5.3, also on MiniMax's table, has its own GLM-5.3 vs GLM-5.2 comparison, and Kimi K3's successor is in our Kimi K3.1 Pro review.

For another coding and tool-use comparison, including the Flash sibling, see our MiMo V2.6 Pro review.

## How do you call M3.1 Flash?

OmniaKey serves both ids from one OpenAI-compatible endpoint, so one key and one request shape covers either model. Current rates are on the M3.1 Flash model page and the M3 model page.

```
curl https://api.omniakey.com/v1/chat/completions \
  -H "Authorization: Bearer $OMNIAKEY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "minimax-m3.1-flash",
    "max_tokens": 8000,
    "reasoning_effort": "low",
    "messages": [{"role": "user", "content": "Explain HTTP caching in two paragraphs."}]
  }'
```
The answer arrives in `choices[0].message.content` and the thinking in `reasoning_content`. Change `model` to `minimax-m3` to call M3.

1. `reasoning_effort` accepts`low` ,`medium` ,`high` ,`xhigh` and`max` , and defaults to`max` when you leave it out, which is the setting that thinks most (MiniMax's text-generation guide, checked October 30, 2026).
2. Thinking cannot be turned off on Flash: sending disabled thinking returns a 400 with `model "MiniMax-M3.1-Flash" requires adaptive thinking` . M3 can run with thinking enabled, adaptive or off.
3. `temperature` and`top_p` are ignored; both models always use 1 and 0.95.

For the exact OpenCode v2.2.3 custom-provider config and a small repository tool test, see our MiniMax M3.1 Flash OpenCode setup guide.

## FAQ

### Is MiniMax M3.1 Flash open source?

The weights are public at the Hugging Face repo MiniMaxAI/MiniMax-M3.1-Flash, and the model card declares the MiniMax Community License (checked October 30, 2026). That is open weights with conditions.

### Can you turn off thinking on M3.1 Flash?

No. MiniMax's guide says thinking is on by default and cannot be turned off, and a request that disables it returns a 400 error. Set `reasoning_effort` to `low` to shorten the thinking instead.

### Does M3.1 Flash take images?

MiniMax's guide and model card say text, image and video in, text out (checked October 30, 2026); Artificial Analysis lists text and image only for Flash. On OmniaKey, the model page lists no image input for `minimax-m3.1-flash`, so send text only. Our runs were text-only.

### How big is M3.1 Flash?

428B total parameters and 23B active per token, with a 1M-token context window, the same shape as M3. It is not a smaller model: MiniMax credits sparse attention, quantization-aware training and speculative decoding for its inference efficiency.

### Is M3.1 Flash good for coding?

In our one coding task it beat M3: its `merge_intervals` passed all nine of its asserts, and M3's file failed its own test. On MiniMax's model card it scores 73.7 on DeepSWE v1.1, below DeepSeek V4.1 Flash-1017 at 77.0 in the same table.

## Sources checked

- MiniMax: MiniMax M3.1 Flash — checked October 30, 2026
- MiniMax: MiniMax M3 — checked October 30, 2026
- MiniMax platform: pricing overview — checked October 30, 2026
- MiniMax platform: text generation guide — checked October 30, 2026
- MiniMax platform: model release notes — checked October 30, 2026
- MiniMax platform: models FAQ — checked October 30, 2026
- MiniMax platform: chat API reference — checked October 30, 2026
- Hugging Face: MiniMax-M3.1-Flash — checked October 30, 2026
- Hugging Face: MiniMax-M3 — checked October 30, 2026
- Artificial Analysis: MiniMax-M3.1-Flash — checked October 30, 2026
- Artificial Analysis: MiniMax-M3 — checked October 30, 2026
- Hacker News: MiniMax M3.1 Flash launch thread — checked October 30, 2026
- Hacker News: DCKing on benchmarks, chriddyp on speed and simonw on reasoning traces — checked October 30, 2026

## Update log

- October 30, 2026: first published.

## Models in this article

## Related articles

### Kimi K3.1 Pro Review: Benchmarks, Pricing and What's New

Moonshot's numbers, an independent check, and what it costs per task.

### GLM-5.3 vs GLM-5.2 for Coding

GLM-5.3 keeps the GLM-5.2 base model and list price, but changes the coding decision through stronger post-training, new effort controls, and a breaking thinking-mode migration.

### Best model for Claude Code

Start daily coding with Sonnet 5.5 at $2 per million input tokens, move complex multi-file work to Opus 5.5 at $4, keep Fable 5.2 at $8 for the hardest long sessions, and hand quick edits and subagents to Haiku 5.5.
