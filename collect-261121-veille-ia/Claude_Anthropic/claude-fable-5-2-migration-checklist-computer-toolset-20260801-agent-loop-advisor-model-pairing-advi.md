---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-migration-checklist-computer-toolset-20260801-agent-loop-advisor-model-pairing-advi
title: "Claude Fable 5.2 migration checklist: computer_toolset_20260801 agent loop, advisor model pairing, advisor_redacted_result, model_binding_mismatch and rollback"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-03-01"]
keywords: ["advisor", "fable", "model", "toolset", "only", "claude", "computer", "opus"]
source: https://max.nardit.com/articles/claude-fable-5-2-migration-computer-use-advisor-thinking
source_anchor: ""
source_lines: [1, 65]
section: "Claude_Anthropic"
---
## Contents

Most of a Claude Fable 5.2 migration is about the order, and none of the five documentation pages says what to do first. Fable 5.2 shipped on November 9 at $8 input and $40 output per million tokens. Three of its changes need code: the computer use toolset, the advisor pairing, and thinking blocks that no other model reads. I covered the prefix check separately; it shows up here only because it decides the order.

If Claude Managed Agents runs your agent, you can stop here: the docs say the only change is the model name, plus the advisor’s `model` if the agent has one, and removing `speed: "fast"` if it sets it.

Four things at the account level, settled before any code changes. Under zero data retention, every Fable 5.2 request on the Claude API returns 400 `invalid_request_error` until a workspace has 30-day retention or Anthropic has authorized an exception. In an organization with customer-managed encryption keys, Fable 5.2 has no structured outputs, so `strict: true` and `output_config.format` have to go and your code validates the output instead. Fable 5.2 has its own rate limit, and coming from Opus 5.5 the docs say its token limits are lower at each tier. And it isn’t on Priority Tier.

With those settled, the order is:

1. Change `tools` while you still run the model you have: the advisor to`claude-fable-5-2` , the computer use tool to the toolset. Start new conversations with the new tools.
2. Switch the executor to `claude-fable-5-2` , with`effort` set explicitly, in staging.
3. Turn on the thinking-binding beta header there and count what gets dropped before you move production.

The reason for step 1 coming first is one line in the preserved-thinking docs: the checked prefix covers only `system`, `tools` and `messages`. The `model` field isn’t in it. Changing the advisor or the computer tool is an edit to `tools`, which invalidates the thinking already in a conversation, while changing the model is not an edit. Do the `tools` changes on the old model and the model switch is the only thing left to change, in either direction.

Point the advisor at `claude-fable-5-2` on the executor you run today. A Fable 5.2 executor accepts only `claude-fable-5-2` or `claude-mythos-5-2` as its advisor, and anything else, Fable 5.1 included, returns 400 `invalid_request_error`. The other direction is open: every executor in the compatibility list accepts Fable 5.2 as advisor, Opus 5.5 and Fable 5.1 among them. The advisor tool is in beta (`advisor-tool-2026-03-01`) on the Claude API, Claude Platform on AWS and Google Cloud (Anthropic managed).

The advice comes back encrypted, but that’s only new if your current advisor returns plaintext. Fable and Mythos advisors from 5 onward, Opus 5 and 5.5, Sonnet 5.5 and Haiku 5.5 already return `advisor_redacted_result` with an opaque `encrypted_content`; Opus 4.8 and the other older advisors return `advisor_result` with readable `text`. Code that read the advice has to stop, round-trip both shapes verbatim, and judge the advisor by what it can see:

`stop_reason` on a successful result only appears when the tool definition sets `max_tokens`, which is one reason the definition above sets it. The docs suggest 2048 as a starting cap.

Advisor tokens appear only in `usage.iterations`. The top-level `usage` counts the executor alone, so a cost dashboard that reads top-level usage misses the advisor entirely. That matters more after this change, because those tokens are now billed at Fable 5.2’s $8 and $40, not at Opus 5.5’s $4 and $20.

Rewrite it for `computer_toolset_20260801` before the model switch, on the model you have, if it accepts the toolset. A `computer_20251124` entry returns 400 on Fable 5.2 on every platform except Amazon Bedrock. Code on Opus 5.5, Sonnet 5.5 or Haiku 5.5 already uses the toolset. Fable 5.1 accepts both the toolset and the old tool, so code on it can port the loop in place. Code on Fable 5, Opus 5, Sonnet 5 or Opus 4.8 is listed under the old tool only; port it and test it on one of the 5.5 models first.

Drop the `computer-use-2025-11-24` beta header and send the entry with no name or display size:

`enable_zoom` defaulted to `false` on the old tool; on the toolset `zoom` is on by default, so if your environment can’t produce zoom images, turn it off in `configs` rather than return errors. The toolset rejects `name`, `display_width_px`, `display_height_px`, `display_number` and `enable_zoom`.

In the loop, the action moves from `input.action` to the block’s `name`, every block carries `toolset_name: "computer"`, and Claude can return several actions in one turn. The docs call this a batch: run the blocks in order, stop at the first failure, and answer every later block with the exact text the docs prescribe. An unanswered `tool_use` block is rejected, so a loop that reads only the first block fails on its next request. If your executor can’t run batches yet, `disable_parallel_tool_use: true` in `tool_choice` limits Claude to one action per turn.

This handler takes the turn’s content as plain dicts (`response.` with the Python SDK):

For a three-block turn (a `key` with `repeat: 3`, a click outside a 1024-pixel screen, a `screenshot`) and a stub executor, it returns:

The screenshot never runs, and it still gets an answer. A handler that stops at the failure and returns two results gets a 400 on the next request.

Three more details from the upgrade steps:

- **`repeat` on `key`.** It takes 1 to 100, and a handler that ignores unknown fields presses the key once, with no error anywhere.
- **Oversized images.** The toolset rejects a screenshot or zoom image over the model’s image limits instead of downscaling it, so resize before you return it and keep scaling coordinates back.
- **Result content.** Results may contain only`text` and`image` .

The toolset with all 17 members also adds about 4,500 input tokens per request; turning `zoom` off removes about 410 of them.

The model string, the effort level, and very little else if step 1 is done. Set `effort` explicitly: Fable 5.2 defaults to `high`, Opus 5.5 to `medium`, and the prefix check post covers what that does to cost and to small `max_tokens`. Fast mode works on the Claude API with `speed: "fast"` and no beta header, at $16 and $80 per million tokens. On Microsoft Foundry it’s for deployments hosted on Anthropic: leave out `speed` and send requests to a `claude-fable-5-2-fast` deployment.

In-flight conversations can move too. On the Claude API and Google Cloud (Google managed), Fable 5.2 reads the thinking of Opus 5.5, Fable 5.1 and the other current models, so a conversation that moves onto it keeps its earlier reasoning.

Send the `thinking-binding-controls-2026-08-01` beta header and count `input_transformations` by reason. Moving onto Fable 5.2 keeps the reasoning; moving off it doesn’t. Outside the Fable 5.2 and Mythos 5.2 pair, no model reads their thinking blocks, so a rollback, a router or a refusal fallback that sends a conversation to Opus 5.5 or Fable 5.1 gets a 200, and the model works without that reasoning. That includes server-side fallback (beta on the Claude API and Google Cloud, Anthropic managed): Fable 5.2’s permitted targets are Opus 4.8 and Opus 5, and neither reads its blocks. I wrote about how that drop works when Sonnet 5.5 introduced it.

What the counts should look like: `model_binding_mismatch` should track how often your router or fallback moves a conversation off the pair, and nothing else. `organization_binding_mismatch` should be zero; Fable 5.2 thinking works only in the account that produced it or one linked to it, so a nonzero count means stored conversations are being replayed through another account (reported on the Claude API and Google Cloud, Google managed). `prefix_binding_mismatch` appears only if you opted into `drop_block`, and any entry means something edited the history.

The blocks aren’t destroyed. They stay in your `messages`, aren’t billed while dropped, and are read again when the conversation returns to Fable 5.2, unless your client strips thinking on a model change.

My read is that it’s cheap in one direction, and the docs mostly describe that direction. Moving a conversation onto Fable 5.2 keeps its reasoning and, with the tools already changed, costs one field. Moving it back is the same field and returns 200, and the turns that run on the other model lose what Fable 5.2 worked out. Putting the advisor and the toolset in place first is what makes the rollback a config change instead of a redeploy: coming from Opus 5.5 or Fable 5.1, the executor you came from accepts both a Fable 5.2 advisor and the toolset, so `tools` stays the same both ways. What you can’t keep on the way back is the thinking, and the beta header is the only place that loss is counted.

## **01** / Discussion

## **02** / More articles

1. ### Claude Fable 5.2's prefix check: a 400 that depends on what your code did last turnThe check that rejects replayed reasoning after an edited history now applies to every account. Code that keeps its history append-only never meets it, and code that doesn't won't find out from a single test request. The fix is in the error message; the hard part is producing the error before production does.
2. ### Claude Opus 4.8 fast mode ends November 30: Opus 5.5 is cheaper per token, not always per requestThe replacement costs 20% less per token and 60% less on cache reads. It also thinks on every request and bills that thinking as output, and its speed gain is in throughput, not in time to first token. Whether the move saves money depends on how much each request writes, and on what your error handler does when the fast rate limit runs out.
3. ### GPT-6.2 Astra and Sol in the API: the tool calling rule is old, the billing traps are newThe Responses API requirement reads like the headline change, and it predates this release. Two changes reach an existing agent without a documented error: one field stops caching, and edited history loses its reasoning. The new background windows still cost more than Batch.
