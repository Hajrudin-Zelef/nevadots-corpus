---
id: collect-261121-veille-ia/claude-anthropic/migrating-from-haiku-4-5-to-claude-haiku-5-5-6-breaking-changes-and-when-the-price-goes-up-5x
title: "Migrating from Haiku 4.5 to Claude Haiku 5.5: 6 breaking changes and when the price goes up 5x"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["thinking", "haiku", "effort", "tokens", "output", "type", "disabled", "claude"]
source: https://dev.to/persimmoq/migrating-from-haiku-45-to-claude-haiku-55-6-breaking-changes-and-when-the-price-goes-up-5x-1m65
source_anchor: ""
source_lines: [1, 89]
section: "Claude_Anthropic"
---
Claude Haiku 5.5 (`claude-haiku-5-5`) was released on October 7, 2026. It is the first Haiku with adaptive thinking and effort, has a 1M-token context window and 128K output, and its API shape now matches Opus 5.5 / Sonnet 5.5. Pricing is $0.10 / $0.50, one tenth of Haiku 4.5, but it is also the first Haiku where the price goes up 5x once the prompt exceeds 100K tokens.

Haiku 4.5 was the last generation that accepted `budget_tokens`, `temperature`, and prefill, so changing only `model` breaks more things than it does on the other 5.5 models. Based on the official migration guide (as of October 12, 2026), here are the 6 changes that return 400 and the things that change silently.

On October 12, 2026, I checked items 1 and 2, plus the effort restriction on `disabled`, against the live API. All requests were short, with `max_tokens: 64`, and the three 400s are not billed.


```
1) thinking: {type: "enabled", budget_tokens: 2000}
   HTTP 400  "thinking.type.enabled" is not supported for this model. Use "thinking.type.adaptive" and "output_config.effort" to control thinking behavior.
2) temperature: 0.2
   HTTP 400  `temperature` is deprecated for this model.
3) thinking: {type: "disabled"} + effort xhigh
   HTTP 400  output_config.effort 'xhigh' is not supported when thinking is disabled on this model. Use effort 'high' or below, or enable thinking.
4) thinking: {type: "disabled"} + effort low
   HTTP 200  stop_reason=end_turn  content=['text']  input_tokens=15  output_tokens=3  thinking_tokens=0
```
Case 4 also shows that `disabled` is accepted with `low`, and that `usage.output_tokens_details.thinking_tokens` comes back as 0. For the other items (prefill, `computer_20250124`, history edits, the organization boundary), I am relying on the official migration guide and did not test them against the live API.

In my own code, the only place that used Haiku 4.5 was one worker in a crawling agent. It passed neither `thinking` nor `temperature`, so changing the model ID was all it needed.

```
# Haiku 4.5
thinking={"type": "enabled", "budget_tokens": 8000}
# Haiku 5.5
thinking={"type": "adaptive"}, output_config={"effort": "medium"}
```
Thinking amount is controlled by `effort` (`low` to `max`). The default is `medium`. `thinking: {"type": "disabled"}` turns thinking off, but it works only with `high` or below; combining it with `xhigh` / `max` returns 400. While thinking is `disabled`, changing effort per message also returns 400.

Any non-default value returns 400. Remove them and steer with the prompt instead.

End `messages` with a user turn. If you used prefill to force JSON, switch to structured outputs (`output_config.format`). Note that Haiku 5.5 on Amazon Bedrock has neither structured outputs nor `strict`. On Bedrock, specify the format in the prompt and validate the output yourself.

On the Claude API and Google Cloud, switch to `computer_toolset_20260801` (no beta header needed). On Amazon Bedrock, switch to `computer_20251124` (`computer-use-2025-11-24`). Remove the `computer-use-2025-01-24` header. The browser tool (`browser_toolset_20260801`) is newly available on the Claude API and Google Cloud.

This check did not exist on Haiku 4.5. If you change `system`, `tools`, or earlier turns and then send thinking blocks back, you get 400. The check is on by default for accounts created on or after August 31, 2026; for older accounts it applies only when you send `prefix_mismatch_behavior`. Keep history append-only, and make instruction changes with a mid-conversation `role: "system"` message. I covered the details in a separate article (Invalid signature in thinking block, in Japanese).

If you send them from a different organization, they are silently dropped without an error, and the model answers without that reasoning. A service that carries conversations for multiple customers in one store should replay each conversation in the organization that created it.

Thinking is on by default. Code that never set `thinking` will start thinking on Haiku 5.5. Because `max_tokens` is a cap on thinking plus the answer combined, a small value can be used up by the `thinking` block alone and leave the text empty. Select the block by `type`, not `content[0].text`. The default for `display` is `omitted`.

Tokens increase by about 30%. This is the same new tokenizer as 4.7 and later. Images also use the high-resolution tier (about 2.5x for 2000x1500). Re-measure with `count_tokens`.

Thinking blocks from every turn stay in the input. Haiku 4.5 kept only the previous turn's. Multi-turn input grows by more than 30%. To remove them, use thinking block clearing in context editing (deleting them yourself triggers item 5).

The minimum cacheable prompt is 512 tokens (4,096 on 4.5). Short prompts you had given up on caching now work.

Refusals were added. `cyber` / `frontier_llm` / `bio` / `general_harms`. You get HTTP 200 with `stop_reason: "refusal"`. There is no server-side fallback, so handle it yourself. Resending usually gets refused again.

Priority Tier is not supported. Context-awareness tags are also not injected (use task budgets for budgeting).

Forced `tool_choice` still works. Unlike Opus 5.5 / Sonnet 5.5, it does not return 400.

| Prompt length | Input | Output | Cache read | Batch | 
|---|---|---|---|---|
| 100K tokens or less | $0.10 | $0.50 | $0.01 | $0.05 / $0.25 | 
| Over 100K tokens | $0.50 | $2.50 | $0.05 | $0.25 / $1.25 | 

"Prompt length" is the total input tokens of a single request, including cache reads and cache writes. Even if caching makes the request cheaper, once the total crosses the threshold that request is billed at the higher rate. The decision is made per request, and earlier requests are not re-billed.

So an agent that runs a long conversation on Haiku 5.5 needs to either shrink it with compaction as input approaches 100K tokens, or compare against Sonnet 5.5 ($2 / $10), whose price does not depend on length. Haiku 5.5 above 100K tokens ($0.50 / $2.50) is still 1/4 of Sonnet 5.5, so it is often still cheaper, but an estimate built on "Haiku is 1/20" will be off.

The official starting points: `medium` for most work (including agentic coding); `low` for chat, short tool tasks, and simple high-volume processing; `high` for knowledge work, long agentic work, and strict instruction following. At `low`, long agentic tasks more often skip searches, stop early, or skip checks. Asking in the prompt to "answer directly" does not reduce thinking; lower the effort instead.

If you give the model a search tool, put today's date in the system prompt so it answers based on recent search results. Render it once at the start of the conversation and keep sending the same `system`. If the day rolls over, put the date in the latest turn.

Before changing `model`, I updated a tool that lists the affected places as file:line. The check for Haiku 5.5 is `--target haiku-5-5`.


```
git clone https://github.com/volvox-ltd/claude5-migration-kit
python claude5-migration-kit/scan.py path/to/your/app --target haiku-5-5
```
| # | Symptom | Fix | 
|---|---|---|
| 1 | 400 on `budget_tokens` | `adaptive` +`effort` | 
| 2 | 400 on `temperature` | Remove it | 
| 3 | 400 on prefill | Structured outputs (validate yourself on Bedrock) | 
| 4 | 400 on `computer_20250124` | New toolset; `computer_20251124` on Bedrock | 
| 5 | 400 on history edits | Append-only | 
| 6 | Thinking vanishes across organizations | Replay in the creating organization | 

The Opus 5.5 / Sonnet 5.5 changes (no `disabled`, no forced tools, and so on) are covered in the Japanese article, which includes a section on those two models.

Official: https://platform.claude.com/docs/en/models/haiku-5-5/migration-guide / https://platform.claude.com/docs/en/about-claude/pricing

*I wrote this with help from Claude, Anthropic's AI model. It drafted the post from the official Haiku 5.5 migration guide and pricing page, and I checked it before publishing.*

## Top comments (0)
