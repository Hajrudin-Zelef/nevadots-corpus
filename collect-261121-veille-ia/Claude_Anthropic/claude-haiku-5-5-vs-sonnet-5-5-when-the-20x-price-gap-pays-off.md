---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-vs-sonnet-5-5-when-the-20x-price-gap-pays-off
title: "Claude Haiku 5.5 vs Sonnet 5.5: When the 20x Price Gap Pays Off"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["haiku", "sonnet", "claude", "anthropic", "october", "price", "coding", "artificial"]
source: https://blog.laozhang.ai/en/posts/claude-haiku-5-5-vs-sonnet-5-5
source_anchor: ""
source_lines: [1, 202]
section: "Claude_Anthropic"
---
## On this page

**Move narrowly scoped, checkable work to Claude Haiku 5.5 and keep agentic coding and multi-step automation on Claude Sonnet 5.5.** Haiku 5.5, released October 7, 2026, lists at one-twentieth of Sonnet 5.5's price per input and output token. Sonnet still leads every benchmark Anthropic published for the pair, and the gap is widest where Sonnet earns its keep: 70.6% vs 39.2% on Terminal-Bench 4.0, according to Anthropic's Claude Haiku 5.5 launch post.

In practice that splits most workloads three ways:

- **Haiku 5.5:** classification, routing, extraction, summarization, context compaction, reading long documents you supply, and lookup subagents working for a bigger model.
- **Sonnet 5.5:** coding agents, terminal work, business automation that chains several tools, and factual answers the model has to get right from memory.
- **Haiku first, Sonnet on failure:** anything where a test, type check or schema validator can tell you automatically that Haiku got it wrong. On Artificial Analysis's medium-effort task costs, this beats running Sonnet on everything once Haiku passes more than about 12% of tasks.

Prices below are Claude API list prices as of October 8, 2026. Benchmark figures come from Anthropic and from Artificial Analysis, and each one names its source. Artificial Analysis figures are from its Intelligence Index v5.0 as of October 28, 2026; that version, built on nine evaluations, replaced v4.3.2 on October 27–28, so older Haiku and Sonnet scores elsewhere won't match.

## Is Claude Haiku 5.5 better than Sonnet? Not on published benchmarks

Sonnet 5.5 scores higher on every row of Anthropic's own comparison table. Haiku 5.5's case is price and output speed, not quality.

| Benchmark (Anthropic's launch table) | Haiku 5.5 | Sonnet 5.5 | 
|---|---|---|
| Terminal-Bench 4.0 (agentic coding) | 39.2% | 70.6% | 
| OSWorld 2.1, offline subset, partial credit | 72.4% | 83.9% | 
| FrontierCode 1.1 (Main) | 46.4% | 52.1% (xhigh) | 
| Humanity's Last Exam, with tools | 57.4% | 64.5% | 
| GDPval-AA v2.1 (Elo) | 1620 | 1840 | 
| Chartography, no tools | 46.4% | 61.6% | 

Anthropic doesn't state the effort setting for most columns. Its own conclusion in the launch post is blunt: Sonnet 5.5 and Opus 5.5 "remain better choices for complex agentic coding tasks", while Haiku 5.5 "is best suited to more narrowly scoped tasks … like compaction, summarization, or subagent work."

Headline scores don't show where the gap is small and where it is large. Artificial Analysis's comparison of Haiku 5.5 and Sonnet 5.5 ran both models at all five effort levels. At `medium`, the default for both in Claude Code, the sub-scores split like this as of October 28, 2026. AA-LCR and SciCode were part of the previous v4.3.2 index and are no longer in v5.0, so their rows show the October 8 results:

| Artificial Analysis eval, both at medium effort | Haiku 5.5 | Sonnet 5.5 | Gap | 
|---|---|---|---|
| AA-LCR v1.1 (long-context reading; v4.3.2 only) | 77.3% | 76.3% | Haiku slightly ahead | 
| SciCode (scientific coding; v4.3.2 only) | 49.0% | 52.9% | Small | 
| Terminal-Bench 4.0 | 15.2% | 29.8% | Sonnet about 2x | 
| AutomationBench-AA (business automation) | 28.6% | 54.9% | Sonnet about 2x | 
| AA-Omniscience v1.1 (knowledge reliability, penalizes hallucination) | 4 | 19 | Large | 
| Intelligence Index v5.0 (composite of 9 evals) | 24.6 | 30.3 | About 6 points | 

Two things follow. Haiku 5.5 read and answered from long documents about as well as Sonnet on AA-LCR, a test that is no longer part of the index, so document Q&A over text you provide is a fair case to test on your own files. It is much weaker at answering facts from its own memory without inventing some, so give it retrieval or a search tool. With search, Anthropic's Prompting Claude Haiku 5.5 guide also says to put today's date in the system prompt.

## Is Haiku 5.5 cheaper than Sonnet 5.5? 20x per token, 4x over 100K

Yes, but Haiku 5.5 has two price rows and Sonnet 5.5 has one. Haiku pays more once a prompt passes 100,000 tokens. Sonnet charges the same rate across its full 1M window. Prices per million tokens, from the Claude API pricing page as of October 8, 2026:

| Per million tokens | Haiku 5.5, prompt up to 100K | Haiku 5.5, prompt over 100K | Sonnet 5.5 | 
|---|---|---|---|
| Input | $0.10 | $0.50 | $2 | 
| 5-minute cache write | $0.125 | $0.625 | $2.50 | 
| 1-hour cache write | $0.20 | $1 | $4 | 
| Cache read | $0.01 | $0.05 | $0.10 | 
| Output (includes thinking) | $0.50 | $2.50 | $10 | 
| Batch input / output | $0.05 / $0.25 | $0.25 / $1.25 | $1 / $5 | 

Read the ratios carefully:

- **Up to 100K tokens of prompt:** Sonnet costs 20x Haiku on input, output and cache writes, but only 10x on cache reads.
- **Over 100K:** the gap drops to 4x on input, output and cache writes, and 2x on cache reads.
- **Sonnet's cache reads were halved on October 7, 2026** , from $0.20 to $0.10. Anthropic estimates that makes Sonnet 5.5 about 20% cheaper on most agentic tasks.
- **The "90% cheaper" and "75% less" figures** from Anthropic's launch post compare Haiku 5.5 with Haiku 4.5, not with Sonnet. Against Sonnet 5.5, Haiku is 95% cheaper per input and output token below 100K.

Each higher Haiku row is labeled "for prompts over 100,000 tokens" and covers input, cache and output, so a long request pays the higher rate on every token in it. Anthropic's pricing page now spells out what counts toward the 100K: a request's prompt length "counts all of its input tokens, including cache reads and cache writes." So budget by total input: cache reads, cache writes and uncached tokens together, tool definitions included. Each request is priced on its own, so a turn that crosses 100K pays the higher rates even when most of its prompt is a cache hit, while earlier turns keep the rates they were billed at.

Haiku 5.5 "uses the same newer tokenizer as Claude 4.7 and later models," per Anthropic's What's new in Claude Haiku 5.5 page. Sonnet 5.5 is one of those models, so the same prompt should count about the same tokens on both. That makes per-token prices directly comparable. It's an inference from the docs, not a stated equivalence.

What typical requests cost, at list prices with equal token counts on both models (tokens × price per million ÷ 1,000,000):

| Request | Haiku 5.5 | Sonnet 5.5 | Sonnet ÷ Haiku | 
|---|---|---|---|
| 10K input, 1K output | $0.0015 | $0.030 | 20x | 
| 99K input, 2K output | $0.0109 | $0.218 | 20x | 
| 150K input, 2K output | $0.080 | $0.320 | 4x | 
| Agent turn: 90K cache read, 5K new input, 2K output | $0.0024 | $0.039 | 16x | 
| Agent turn: 150K cache read, 5K new input, 2K output | $0.0150 | $0.045 | 3x | 
| 1 million classification calls, 2K input and 300 output each | $350 | $7,000 | 20x | 

The 90K agent turn on Sonnet cost $0.048 before the cache cut. At $0.039 it is 18.75% cheaper, close to Anthropic's 20% estimate. The 150K turn is the one to watch. Coding agents and Claude Code sessions pass 100K of context quickly, and once they do, Haiku is only about 3x cheaper per turn.

## Cost per task: 8.5x–24x at equal effort, about 5x at equal score

Per-token prices assume both models use the same number of tokens on a task. They don't. Effort is calibrated separately for each model, and both get verbose at the top levels. Artificial Analysis measured cost and time per task on its Intelligence Index at every effort level, priced at Sonnet's $0.10 cache read and Haiku's up-to-100K rates. Results on Intelligence Index v5.0 as of October 28, 2026; the ratios use Artificial Analysis's unrounded costs:

| Effort | Haiku 5.5: index / cost per task / time per task | Sonnet 5.5: index / cost per task / time per task | Sonnet ÷ Haiku cost | 
|---|---|---|---|
| low | 19 / $0.044 / 98 s | 25 / $0.51 / 121 s | 11.7x | 
| medium | 25 / $0.092 / 193 s | 30 / $0.78 / 178 s | 8.5x | 
| high | 30 / $0.159 / 298 s | 41 / $1.64 / 332 s | 10.3x | 
| xhigh | 34 / $0.25 / 441 s | 50 / $4.09 / 691 s | 16.5x | 
| max | 36 / $0.411 / 593 s | 54 / $10.02 / 1,326 s | 24.4x | 

Artificial Analysis labels the Sonnet runs "with fallback" and prices Haiku only at its lower band. Long-context tasks would cost Haiku more than this table shows.

*The chart shows Artificial Analysis Intelligence Index v4.3.2 figures; the table above gives the v5.0 figures as of October 28, 2026.*

Three readings matter for a decision:

- **At equal score, Haiku is about 5x cheaper but slower.** Haiku at`high` matches Sonnet at`medium` on the index (29.6 vs 30.3) for $0.16 against $0.78 per task, 4.9x cheaper. It takes 298 seconds against 178, about 1.7x as long. One level down, Haiku`medium` and Sonnet`low` pair the same way (24.6 vs 25.3), 5.6x cheaper and 1.6x as long.
- **Haiku's best still trails Sonnet where Sonnet is strong.** Haiku at`max` scores 36, above Sonnet`medium` , for about half the cost ($0.41 vs $0.78), though it takes 3.3 times as long. Yet on AutomationBench-AA it scores 35.4% against Sonnet medium's 54.9%. An equal index score doesn't mean equal results on your workload.
- **"Fastest" means per output token, not per answer.** Haiku generated 175–241 tokens per second against Sonnet's 109–140. At`medium` , a full task took about the same time on both, 193 against 178 seconds. Haiku also thinks before it answers. In Artificial Analysis's earlier October runs on the v4.3.2 tasks, it took about 12 seconds to its first token even at`low` , where Sonnet took about 1 second.

If time to first token matters, as in live chat or voice, test Haiku 5.5 with thinking turned off. The API accepts that at `high` effort or below.

## When to use Haiku 5.5 vs Sonnet 5.5: by how errors get caught

Anthropic's guide to choosing a model suggests starting with Haiku 5.5, testing, and upgrading where needed. It also notes that "tuning effort is often a better lever than switching models." The practical version: try Haiku at a higher effort before moving a job to Sonnet, but compare the cost and time of Haiku `xhigh` or `max` with Sonnet before settling.

| Work | Start on | Move up when | 
|---|---|---|
| Classification, tagging, routing, sentiment | Haiku 5.5, `low` or thinking off | Accuracy on a labeled sample stays below your bar even at `medium` | 
| Extraction into a JSON schema | Haiku 5.5, `medium` | Spot checks find valid JSON with wrong values | 
| Summaries and context compaction | Haiku 5.5 | The next step keeps asking for facts the summary dropped | 
| Q&A over long documents you supply | Haiku 5.5 | Prompts regularly pass 100K tokens, where the price gap falls to about 4x | 
| Search or lookup subagents under a coding agent | Haiku 5.5 subagents, Sonnet or Opus as lead | The lead model redoes subagent results often | 
| Support chat with a fixed policy | Haiku 5.5 at `high` | Policy breaks persist after adding Anthropic's system-prompt adherence line | 
| Coding agents, terminal work, multi-file changes | Sonnet 5.5 | Only well-specified, test-covered tasks are candidates for Haiku first | 
| Automation across CRM, spreadsheets and ticketing tools | Sonnet 5.5 | AutomationBench-AA gap: 28.6% vs 54.9% at `medium` | 
| Facts answered from model memory | Sonnet 5.5, or Haiku with retrieval | AA-Omniscience gap: 4 vs 19 at `medium` | 

The common thread: Haiku fits jobs where a wrong answer is cheap or caught automatically. Sonnet fits jobs where a wrong answer flows downstream unnoticed, such as a half-finished code change reported as done or an automation step that writes bad data.

## Haiku first, Sonnet on failure: cheaper once Haiku passes about 12%

Escalation sends every task to Haiku 5.5 and re-runs only the failures on Sonnet 5.5. The expected cost per task is:

**cost = Haiku cost per task + (1 − p) × Sonnet cost per task**, where p is the share of tasks Haiku gets right.

Escalation beats Sonnet-only whenever p is higher than Haiku's cost divided by Sonnet's. With Artificial Analysis's medium-effort costs ($0.0918 for Haiku, $0.7807 for Sonnet), that threshold is 0.0918 ÷ 0.7807, about 12%:

| Haiku pass rate (p) | Cost per 1,000 tasks, Haiku first | Sonnet only | 
|---|---|---|
| 50% | $482 | $781 | 
| 70% | $326 | $781 | 
| 90% | $170 | $781 | 

Plug in your own numbers. Measure cost per task on both models at the effort you would actually run, and measure p on a sample of real tasks. At list prices for prompts up to 100K, with equal token counts on both models, the threshold falls to 1 in 20 (5%).

This math holds only under four conditions:

1. **Failures must be caught by a machine.** Tests, type checks, builds, schema validation, or a rule that lets the model abstain. Wrong answers that slip through can cost more than the tokens you saved, and the formula doesn't price them.
2. **Don't trust Haiku's own "done."** At`low` and`medium` , Haiku 5.5 sometimes reports a code change as finished without running a check, according to Anthropic's prompting guide. Your harness should run the check itself. The guide also gives a system-prompt paragraph that makes Haiku verify more often.
3. **Escalated tasks take longer.** They pay for Haiku's attempt and then Sonnet's. With Artificial Analysis's medium times (193 seconds for Haiku, 178 for Sonnet), p = 70% averages about 246 seconds per task against 178 for Sonnet only.
4. **Hand Sonnet the task, not Haiku's thinking.** Sonnet 5.5's documentation lists thinking blocks as tied to the model and conversation that produced them. Re-send the original request, and add Haiku's failed output as plain text if it helps. Also drop any forced`tool_choice` : Haiku 5.5 accepts it, Sonnet 5.5 returns an error.

*The chart uses Artificial Analysis v4.3.2 task costs; the table above gives the v5.0 figures as of October 28, 2026: Sonnet only at $781 per 1,000 tasks and a break-even near 12%.*

## Switching code from Sonnet 5.5 to Haiku 5.5: what changes in the API

The two models share a 1M-token context window, 128K max output on the Messages API, text and image input, and adaptive thinking. Most differences are in defaults and thinking controls:

| Setting | Sonnet 5.5 | Haiku 5.5 | 
|---|---|---|
| Model ID (Claude API, Google Cloud, Foundry, Claude Platform on AWS) | `claude-sonnet-5-5` | `claude-haiku-5-5` | 
| Amazon Bedrock ID | `anthropic.claude-sonnet-5-5` | `anthropic.claude-haiku-5-5` | 
| Default effort on the Claude API | `high` | `medium` | 
| Lowest thinking setting | `thinking: {"type": "between_tools"}` | `thinking: {"type": "disabled"}` | 
| Lowest setting accepted at | `low` ,`medium` ,`high` (400 error at`xhigh` /`max` ) | `low` ,`medium` ,`high` (400 error at`xhigh` /`max` ) | 
| Forced `tool_choice` | Returns an error | Accepted; the response starts with the tool call, no thinking block | 
| Earliest retirement | September 28, 2027 | October 7, 2027 | 

Before switching, run through this checklist:

- **Set effort explicitly.** Code that never set effort on Sonnet ran at`high` . The same code on Haiku runs at`medium` . Anthropic's effort parameter documentation recommends`medium` for most Haiku work,`low` for chat and simple high-volume calls, and`high` for knowledge work and strict instruction following.
- **Replace `between_tools`.** Haiku's documented off switch is`disabled` . Use adaptive thinking (omit the`thinking` field) if you need`xhigh` or`max` .
- **Leave room in `max_tokens`.** Thinking is on by default and counts toward`max_tokens` . A small limit can end the response after the thinking block and before any text.
- **Read content blocks by `type`.** Haiku 5.5 responses can begin with thinking blocks, and thinking text is omitted unless you set`thinking.display` to`"summarized"` .
- **Keep the restrictions you already handle on Sonnet.** Both return a 400 error for non-default`temperature` ,`top_p` or`top_k` . Haiku 5.5 also rejects assistant-message prefill.
- **Handle `stop_reason: "refusal"`.** Haiku 5.5 runs safety classifiers with no server-side fallback. Sending the same request again usually gets another refusal.
- **Add Anthropic's Haiku prompt lines where you see the matching problem.** These cover early stopping in long agent prompts at`low` , skipped tool calls when JSON output is combined with thinking off, and reasoning-like text leaking into user-facing replies.
- **Change effort per turn without losing the cache.** Both models accept per-message effort changes with the`mid-conversation-output-config-2026-07-01` beta header on the Claude API and Google Cloud. That keeps the prompt cache, which changing the top-level effort does not. It doesn't work with Haiku's`disabled` or Sonnet's`between_tools` .

A minimal Haiku 5.5 request with effort set explicitly. This is the request shape from Anthropic's effort documentation with the model ID changed:

## Haiku 5.5 in Claude Code and the Claude app: version, /model, subagents

**In the Claude app,** Anthropic says Free, Pro, Max, Team and Enterprise users can all select Haiku 5.5 on Claude.ai, on web, iOS and Android. Sonnet 5.5 is available to everyone. The Claude Help Center article on usage and length limits says usage depends on "which Claude model you're chatting with, and the effort level you've selected." Anthropic doesn't publish a ratio, so there is no official figure for how much further Haiku stretches a plan. For how the five-hour and weekly limits work, see Claude Usage Limits by Plan: What 5x and 20x Actually Multiply.

**In Claude Code,** according to the Claude Code model configuration docs:

- Haiku 5.5 needs Claude Code v2.1.293 or later. Run `claude update` first.
- Switch with `/model claude-haiku-5-5` , or launch with`claude --model claude-haiku-5-5` . The`haiku` alias resolves to Haiku 5.5 only on the Anthropic API. On Amazon Bedrock, Google Cloud, Microsoft Foundry and Claude Platform on AWS,`haiku` still means Haiku 4.5, so use the provider's full model ID or set`ANTHROPIC_DEFAULT_HAIKU_MODEL` .
- To keep Sonnet 5.5 as the main model and run subagents on Haiku, set `CLAUDE_CODE_SUBAGENT_MODEL=haiku` . A subagent definition with its own`model` field still overrides it.`ANTHROPIC_DEFAULT_HAIKU_MODEL` also controls Claude Code's background features.
- Both models default to `medium` effort in Claude Code. That differs from the API, where Sonnet 5.5 defaults to`high` . Thinking can't be turned off on either model in Claude Code.
- Haiku 5.5 runs with the 1M window on every plan. When Claude Code bills to an API key, any Haiku request with more than 100K tokens of prompt pays the higher price row, which matters in long sessions.

If you're weighing per-token API billing against a subscription for this kind of work, Claude API vs Claude Code: Which One to Use and How You Pay covers both, including the new monthly API credit for Max, Ultra and Team subscribers. Anthropic says it is $100 for Max (formerly Max 5x), $200 for Ultra (formerly Max 20x) and up to $500 pooled for Team, usable on any model.

## FAQ about Claude Haiku 5.5 and Sonnet 5.5

### When should I use Haiku vs Sonnet vs Opus vs Fable?

Use Haiku 5.5 for high-volume, narrowly scoped work and as a subagent. Use Sonnet 5.5 for everyday coding, agents and enterprise workloads. Move above Sonnet only when Sonnet still fails your checks at higher effort. Claude Fable 5.2 vs Opus 5.5: When Fable High Beats Opus Max covers that next step, and Claude Opus 5.5 Pricing and Limit Reset: What Each One Changes has Opus 5.5's API prices.

### Is Claude Haiku 5.5 good enough for coding?

It works for scoped coding work: lookups and code search as a subagent, well-specified changes covered by tests, and summarizing diffs or logs. It is not a replacement for Sonnet 5.5 as the main coding agent. On Terminal-Bench 4.0, Anthropic reports 39.2% for Haiku 5.5 against 70.6% for Sonnet 5.5. If you use it for code changes, have your harness run the tests rather than trusting its report.

### Is Haiku 5.5 90% cheaper than Sonnet 5.5?

No. The 90% figure compares Haiku 5.5 with Haiku 4.5 on prompts up to 100K tokens. Against Sonnet 5.5, Haiku 5.5 is 95% cheaper per input and output token up to 100K and 75% cheaper above it. Per finished task on Artificial Analysis's Intelligence Index v5.0, Sonnet costs 8.5x to 24x as much at the same effort, as of October 28, 2026.

### Does Haiku 5.5 allow more security work than Sonnet 5.5?

Somewhat. Anthropic says Haiku 5.5's cybersecurity safeguards "permit a wider range of defensive tasks than our safeguards for Sonnet 5.5, but they still block penetration testing." Finding vulnerabilities in source code is allowed. Organizations whose legitimate security work gets blocked can apply to Anthropic's Cyber Verification Program.
