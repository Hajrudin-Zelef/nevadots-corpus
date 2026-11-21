---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-benchmarks-and-pricing-computingforgeeks
title: "Claude Fable 5.1: Benchmarks and Pricing | ComputingForGeeks"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-01"]
keywords: ["fable", "claude", "anthropic", "cache", "output", "input", "thinking", "tokens"]
source: https://computingforgeeks.com/claude-fable-5-1-released-features-benchmarks/
source_anchor: ""
source_lines: [1, 168]
section: "Claude_Anthropic"
---
The most interesting number on **Claude Fable 5.1** is not a benchmark. It is the bill. Anthropic released it on September 1, 2026 at the same $10 and $50 per million token price as Fable 5, then cut the price of cache reads by 75%, loosened the safeguards that were refusing benign work, and pushed the score on the hardest agentic tasks up sharply. It is still a Mythos-class model, one tier above Opus 5, and it ships as the same two-model split as before: Fable 5.1 for everyone, Mythos 5.1 behind a trusted-access program.

This guide covers what changed, the benchmark numbers worth trusting, the three API changes that will break Fable 5 code, and how to point Claude Code at the new model. The interesting part is that we did not take the launch post at its word. We ran the same DevOps prompts through `claude-fable-5-1`, `claude-fable-5`, and `claude-opus-5` on the raw Messages API, graded the output with real linters, measured the cost per task, and caught a genuine difference in how the two Fable models handle a security-adjacent question. Everything below was checked against Anthropic’s September 2026 documentation and run live on September 3, 2026.

## Claude Fable 5.1 specs and pricing

The spec sheet is deliberately boring. Fable 5.1 inherits everything structural from Fable 5 and changes exactly one price. The one number that moved is cache reads, and on long agentic sessions that reuse a big context, it is the number that decides your bill.

| Spec | Fable 5.1 | Fable 5 | Opus 5 | Sonnet 5 | 
|---|---|---|---|---|
| Claude API ID | `claude-fable-5-1` | `claude-fable-5` | `claude-opus-5` | `claude-sonnet-5` | 
| Input / output per MTok | $10 / $50 | $10 / $50 | $5 / $25 | $2 / $10 | 
| Cache read per MTok | **$0.25** | $1.00 | $0.50 | $0.20 | 
| Context / max output | 1M / 128k | 1M / 128k | 1M / 128k | 1M / 128k | 
| Knowledge cutoff | June 2026 | Jan 2026 | May 2026 | Jan 2026 | 
| Adaptive thinking | Always on | Always on | On by default | On by default | 
| Priority Tier | No | Yes | No | No | 

Cache reads now cost a quarter of the Fable 5 rate, which is 0.025 times the input price instead of the usual 0.1. Anthropic frames the saving as roughly 25% off a typical workload and up to 45% off context-heavy agentic work, measured over four weeks of real August traffic at default effort. Base input, output, cache writes, and the 512-token minimum cacheable prefix are all unchanged. A useful caveat before you treat that as free money: the saving only lands if your cache actually hits, and an agent that reasons at high effort can spend enough new output tokens to swallow it. We measured exactly that later on.

You can confirm the limits straight from the Models API. Put your key in an environment variable first so it never lands in shell history:

`export ANTHROPIC_API_KEY="your-anthropic-api-key"`
Then query the model object directly:

```
curl -s "https://api.anthropic.com/v1/models/claude-fable-5-1" \
  -H "x-api-key: ${ANTHROPIC_API_KEY}" \
  -H "anthropic-version: 2023-06-01" \
  | jq '{id, max_input_tokens, max_tokens, thinking: .capabilities.thinking.types}'
```
The response confirms the million-token window, the 128k output ceiling, and the detail that trips up ported code: the old `enabled` thinking type is gone, and only `adaptive` survives.

```
{
  "id": "claude-fable-5-1",
  "max_input_tokens": 1000000,
  "max_tokens": 128000,
  "thinking": {
    "enabled": { "supported": false },
    "adaptive": { "supported": true }
  }
}
```
Two access details matter before you route production traffic here. Fable 5.1 requires 30-day data retention and is not available under zero data retention unless Anthropic authorizes it, and it has no Priority Tier, which Fable 5 did have. If you were on Priority Tier for Fable 5, you lose it on the move.

## What Anthropic claims on benchmarks

The launch leads with agentic and scientific work, not saturated exam benchmarks, and the single cleanest signal is Terminal-Bench-Science. Fable 5.1 scores 52.6% against Fable 5’s 24.7%, more than double, with Opus 5 at 29.0% and GPT-5.6 Sol at 22.4% on the same harness.

The rest of the numbers move in the same direction, with the gains widest on agentic and knowledge work and narrowest on already-saturated tests. One honest caveat from Anthropic frames the table. Both Fable models were scored with production safeguards switched on, and on the tasks where a classifier intervened they took zeros. On the computer-use benchmark both Fable models lost points that way, and on AutomationBench it was Fable 5 that took the hit, so part of that row’s wide gap is a safeguard penalty on Fable 5 rather than a jump by 5.1. The Fable scores are conservative, not inflated.

| Benchmark | Fable 5.1 | Fable 5 | Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|---|
| Terminal-Bench-Science 0.1 | **52.6%** | 24.7% | 29.0% | 22.4% | 
| Terminal-Bench 4.0 (agentic coding) | 55.8% | 42.0% | 52.3% | 37.3% | 
| CursorBench 3.2.0 (agentic coding) | **73.4%** | 70.5% | 70.0% | 67.2% | 
| SWE-bench Pro | 81.2 | 80.0 | 79.2 | 64.6 | 
| Humanity’s Last Exam (with tools) | 65.0% | 63.8% | 63.6% | n/a | 
| GDPval-AA v2 (knowledge work, Elo) | **1853** | 1723 | 1824 | 1711 | 
| AutomationBench (business workflows) | 31.4% | 17.1% | 26.9% | 19.6% | 

One correction is worth making because the launch-week blogs got it wrong in bulk. The figures floating around as “Fable 5.1 scores 95% on SWE-bench Verified” come from third-party leaderboards, not from Anthropic, which published no SWE-bench Verified score for Fable 5.1 or Fable 5. The software-engineering number Anthropic does report, in the system card rather than the launch post, is SWE-bench Pro at 81.2, a slim lead over both Fable 5 and Opus 5. The story here is not a coding blowout. It is a model that pulls clearly ahead on long-horizon scientific and agentic tasks and edges everything else.

## Fable 5.1 vs Opus 5 vs Fable 5 on real DevOps work

Vendor benchmarks are marketing until someone reproduces them, so we ran our own. Three tasks went to all three models through the raw Messages API at each model’s default effort, which is what a team swapping a model ID actually experiences. The tasks: a hardened Kubernetes manifest set for an orders API, a defensive log-rotation Bash script, and an OpenTofu VPC module with per-AZ NAT gateways. Real tools did the grading, not us. Every prompt ended with an explicit instruction to return raw code with no markdown fences.

All three models cleared every gate. The Kubernetes manifests passed `kubeconform -strict`, the Bash scripts passed `shellcheck`, and the OpenTofu modules passed `tofu validate` after a real provider init. All three also obeyed the no-fences instruction, which is not a given. When we ran the same style of test at the Opus 5 launch, Opus 5 wrapped its YAML in a markdown fence despite being told not to, and the file failed kubeconform until we stripped it. This time every output started with the shebang or the first resource, clean.

The manifests separated the models on defense-in-depth rather than correctness. Fable 5.1 hit every hard requirement in the prompt: `runAsNonRoot` with a fixed UID at both pod and container level, `readOnlyRootFilesystem`, all capabilities dropped, a `seccompProfile` of `RuntimeDefault`, a PodDisruptionBudget holding two pods, and a NetworkPolicy that allowed egress only to Postgres on 5432 and DNS on 53. Opus 5 matched all of that and went one better by also emitting the `Namespace` object the prompt referenced, which Fable 5.1 assumed already existed. Fable 5 was a touch lighter, applying the security context once at pod level rather than repeating it on the container.

The cost meter is where the trade-off shows. At the same sticker rate, Fable 5.1 is double Opus 5 per token, and on tasks where it decides to think hard, the gap widens fast.

| Task | Fable 5.1 time / cost | Opus 5 time / cost | Fable 5.1 thinking tokens | 
|---|---|---|---|
| Kubernetes manifests | 13.5s / $0.067 | 20.2s / $0.057 | 0 | 
| Bash log rotation | 73.5s / $0.304 | 40.1s / $0.097 | 3,519 | 
| OpenTofu VPC module | 20.3s / $0.106 | 26.0s / $0.069 | 0 | 

Notice the Bash row. Adaptive thinking decided the defensive script with its path guards and edge cases deserved deliberation, spent 3,519 thinking tokens on it, and cost three times what Opus 5 did for the same clean result. On the Kubernetes and OpenTofu tasks it thought nothing at all and stayed close to Opus. That variance is the model working as designed, and it is also the practical argument for tuning effort rather than leaving it at the default. If you point Fable 5.1 at routine work at high effort, you pay for reasoning the task did not need.

## The cheaper cache reads, measured

The cache-read cut is the release’s real selling point, so we put a number on it. We cached a system reference of roughly five and a half thousand tokens with a cache breakpoint, then asked two questions against it back to back. The first call writes the cache, the second reads it.

```
# Second call, identical cached prefix
usage.cache_read_input_tokens = 5560
usage.input_tokens            = 39
usage.output_tokens           = 80
cost                          = $0.00578
```
Those 5,560 cached tokens billed at $0.25 per million, which is $0.00139. On Fable 5, the same read would have cost four times as much at the old $1.00 rate. On a single question that is a rounding error. On a coding agent that re-reads a large repository context on every one of hundreds of turns, it is the difference between a cache hit being nearly free and being a real line item. This is why the saving is largest on context-heavy agentic work and close to nothing on short one-shot calls.

Keep the caveat from the DevOps test in view, though. Independent testing by Artificial Analysis found Fable 5.1 emitting well over one and a half times the output tokens of Fable 5 at max effort, enough that cost per task actually rose on their runs even as the cache-read price fell. Both things are true. The cache discount is real and it helps most when your input dwarfs your output; the moment the model starts thinking hard and writing long, output tokens at $50 per million set the bill, not cache reads at a quarter. Our own Bash task is the small-scale version of the same effect.

## Fewer cybersecurity false positives, tested

Anthropic says Claude Code users should see around 60% fewer interventions per session from the cybersecurity safeguards on Fable 5.1, in part because the model is now allowed to identify software vulnerabilities in source code, which the older safeguards blocked. Fewer interventions means fewer benign requests wrongly stopped. This is the change most likely to matter day to day, and it is the one we could demonstrate cleanly.

We handed both Fable models an ordinary sysadmin question with a real `sshd_config` pasted into the system reference: which directive controls root login, and what is its current value. Fable 5 refused it.

The refusal arrives as a normal HTTP 200 with `stop_reason` set to `refusal` and a `stop_details.category` of `cyber`, and zero output tokens. Fable 5.1 answered the identical request in full, correctly naming `PermitRootLogin` and reading its default off the commented line. Strip the config out of the context and Fable 5 answers the bare question fine, so this was the old, less precise classifier reacting to the presence of a config file next to a security question. On Fable 5.1 it does not fire. If you have ever had a hardening walkthrough or a config review stall on a Claude model for no obvious reason, this is the fix.

Plan for the refusal path regardless, because it still exists for genuinely dual-use work. Detect it by branching on `stop_reason` on every response rather than wrapping the call in a try and except, since a refusal is a success at the HTTP layer. The safeguards still route penetration testing, exploit generation, and binary vulnerability scanning to the Opus models, and Fable 5.1’s permitted fallback targets are Opus 4.8 and Opus 5.

## The API changes that break Fable 5 code

Swapping the model string is not the whole migration. Three changes break code that ran fine against Fable 5. The first is a hard 400 we reproduced against the live API. The other two are rules about the shape of the conversation you send, and they do not surface until a harness edits its own history or switches models mid-run.

First, forced tool use is gone. A `tool_choice` of `any` or a named `tool` is rejected outright, on the Messages API, the token-counting endpoint, and Batches.

```
{
  "type": "invalid_request_error",
  "message": "tool_choice: type \"tool\" and \"any\" are not supported for this model."
}
```
The reasoning is that thinking is always on, and forcing a tool call would skip it and push the model’s working into the arguments. Keep `tool_choice` on `auto`, name the tool in the prompt, and set `strict: true` on the tool if you need schema-valid arguments back, or use structured outputs if the forced call only ever existed to extract JSON.

Second, a Fable 5.1 thinking block is bound to the model that produced it. Opus 5, Fable 5, and every earlier model cannot read one, so when a router or a refusal fallback switches models mid-conversation, the API drops those blocks before the next model sees them. The drop is unbilled and, unless you set the `thinking-binding-controls-2026-08-01` beta header, silent. The practical rule is to keep passing thinking blocks back unchanged and let the API decide which the target model can use, rather than stripping them yourself.

Third, and the one most likely to bite a custom harness, is preserved thinking. A Fable 5.1 thinking block is also bound to the exact conversation prefix that produced it, so editing, reordering, or trimming an earlier turn and then replaying a later thinking block returns a 400 saying the block is bound to a different conversation. Anthropic added this deliberately as an anti-distillation measure, and it is enforced for API accounts created on or after August 31, 2026. Claude Code, claude.ai, and the Agent SDK keep the history append-only for you. If your own code builds the messages array, the rule is to stop editing history: freeze the system prompt, add mid-conversation instructions as system messages rather than by rewriting earlier turns, and let server-side compaction do the trimming.

One more change bites only if you come to Fable 5.1 from Opus 5 rather than from Fable 5: thinking can no longer be disabled at all. Where Opus 5 accepted a disabled thinking block at high effort or below, Fable 5.1 rejects it everywhere, which we confirmed against the live API.

```
{
  "type": "invalid_request_error",
  "message": "\"thinking.type.disabled\" is not supported for this model."
}
```
Control spend with the `effort` parameter instead, which runs from low to max and defaults to high. Because `max_tokens` caps thinking plus visible text together, a workload tuned to a tight ceiling on Fable 5 can now hit it mid-answer, so revisit that value when you migrate.

The additions are friendlier. Per-message effort lets one conversation mix effort levels without resetting the prompt cache, turn-scoped system messages carry a one-turn reminder that costs nothing once it clears on the next turn, and a new `display: "updates"` setting surfaces the model’s between-tool progress notes as text so a long agentic turn does not look frozen. Every text output also now carries Anthropic’s statistical watermark, which adds no tokens or hidden characters.

## Use Claude Fable 5.1 in Claude Code

Claude Code added Fable 5.1 in version 2.1.257, where the `fable` alias now resolves to it. Update first so the model appears in the picker:

```
claude update
claude --version
```
The version prints on its own line:

`2.1.258 (Claude Code)`
Open the picker with `/model` and Fable 5.1 sits at option three, described as the most capable model for your hardest and longest-running tasks. Opus 5 remains the recommended default, which matches Anthropic’s own guidance to start there and reach for Fable only when a job genuinely needs it.

Pick it for a single session or set it as your default, or start pinned to it from the shell:

`claude --model fable`
One caveat on that alias. In Claude apps that reach the model through a gateway, `fable` can still resolve to Fable 5 until the gateway is configured for the new model, so select Fable 5.1 explicitly in the picker there, or pin it with the `ANTHROPIC_DEFAULT_FABLE_MODEL` variable.

A few behaviors are worth knowing. The default effort is high, with no default-effort hold the way Fable 5 had, and thinking cannot be turned off. When a request trips a safeguard, Claude Code can re-run it automatically on an Opus model, sending biology-flagged prompts to Opus 5 and cyber-flagged ones to Opus 4.8, which you can turn off with the `switchModelsOnFlag` setting. On the Anthropic API the model runs with the full million-token window by default. Because a long Fable 5.1 session can burn output tokens at the doubled rate, the habits in our guide on cutting Claude Code token usage matter more here than anywhere, and the rest of the model and session commands live in the Claude Code cheat sheet.

Mind the plan side too. Fable models are not included free on Pro or on standard Team seats, so those accounts spend usage credits from the first token, and there is no one-time credit this time the way there briefly was for Fable 5. If you are on Max, a premium Team seat, or a premium Enterprise seat, up to half of your weekly usage allowance covers Fable before credits kick in.

## The Mythos 5.1 split

Fable 5.1 and Mythos 5.1 are the same weights with different safeguards, exactly as the pair split last time. Mythos 5.1 lifts the cybersecurity and life-sciences restrictions and is limited to vetted US organizations through two programs, a Cyber Verification Program for defensive security work and a Life Sciences Verification Program run in partnership with the US government. It shares the same $10 and $50 pricing and the same cheaper cache reads, and it powers Anthropic’s Claude Security product. One quiet asymmetry sits in the API: Mythos 5.1 does not run the preserved-thinking history check that Fable 5.1 enforces. For everyone not in those programs, Fable 5.1 is the model you call, and the fallback behavior above is how its safeguards stay out of your way.

## Should you switch to Fable 5.1?

If you are already running Fable 5, this is a clean upgrade: the same intelligence tier, cheaper cache reads, and fewer nuisance refusals, for one model-string change plus the three API fixes above. Do the migration, tune effort down from the default on routine routes, and keep your refusal handling.

If you are choosing fresh, take Anthropic’s own advice and start on Opus 5. It is half the price, carries a fresher knowledge cutoff, and in our DevOps runs it matched Fable 5.1 on correctness while costing less per task. Move up to Fable 5.1 for the jobs where its lead is real and worth paying for: multi-hour autonomous runs, long-horizon scientific and research work, repo-wide migrations, and the hard problems where one correct long run beats a stack of cheaper attempts. The benchmark gaps and our own numbers point the same way. You pay the premium for the top of the difficulty curve, and for routine work Opus 5, or Sonnet 5 below it, is the smarter place to spend. The release itself is the tell: the frontier has moved from chasing benchmark records to trimming the cost and friction of running the model you already have.
