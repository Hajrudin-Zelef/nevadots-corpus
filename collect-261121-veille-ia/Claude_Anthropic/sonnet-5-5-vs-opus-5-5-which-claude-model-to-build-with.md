---
id: collect-261121-veille-ia/claude-anthropic/sonnet-5-5-vs-opus-5-5-which-claude-model-to-build-with
title: "Sonnet 5.5 vs Opus 5.5: Which Claude Model to Build With"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "sonnet", "points", "anthropic", "leads", "comparison", "tokens", "work"]
source: https://emergent.sh/learn/sonnet-5-5-vs-opus-5-5
source_anchor: ""
source_lines: [1, 158]
section: "Claude_Anthropic"
---
#### TL;DR

- Sonnet 5.5 vs Opus 5.5 is a close contest: Opus 5.5 leads on seven of the eight benchmarks in Anthropic's Sonnet 5.5 launch comparison, by 3.2 percentage points or less on the percentage-scored ones, and Sonnet 5.5 leads on Terminal-Bench 4.0.
- Artificial Analysis scores them 56 and 58 on its Intelligence Index, a 2-point gap. The widest independent gap is factual accuracy, where Opus 5.5 scores 66% against 54%.
- Sonnet 5.5 costs half as much per token ($2 input and $10 output per million tokens against $4 and $20) and runs faster.
- At the top of the quality range, the price gap reverses: Opus 5.5 reaches Sonnet 5.5's best index score for $3.46 per task, against $7.60 for Sonnet 5.5.
- Use Sonnet 5.5 at Medium or High for well-scoped, high-volume work. Use Opus 5.5 when you need top-quality output, strong factual accuracy, or sustained judgment.


The two models are closer than their prices suggest. Anthropic's launch comparison puts them a few points apart, and on terminal-based agent work, the cheaper model leads.

That makes "which one is better" the wrong question. The real choice depends on how hard the task is and how much effort you set the model to spend.

For everyday work, Sonnet 5.5 delivers most of Opus 5.5's quality at half the token price. For the hardest work, Opus 5.5 can get there for less, according to independent test data. For a deeper look at each score, see the full Sonnet 5.5 benchmarks.

## Sonnet 5.5 vs Opus 5.5 at a glance: same context window, half the price, a few points apart

The two models share the same context window, output limit, and knowledge cutoff. They differ on price, speed, default effort, and how they handle thinking.

| **Spec** | **Claude Sonnet 5.5** | **Claude Opus 5.5** | 
|---|---|---|
| Anthropic's description | The best combination of speed and intelligence | For long-running agentic coding and knowledge work | 
| Input / output price per 1M tokens | $2 / $10 | $4 / $20 | 
| Comparative latency | Fast | Moderate | 
| Thinking | Adaptive | Adaptive (always on) | 
| Default effort on the API | High | Medium | 
| Context window | 1M tokens | 1M tokens | 
| Max output | 128K tokens | 128K tokens | 
| Reliable knowledge cutoff | June 2026 | June 2026 | 
| Retirement | Not sooner than September 28, 2027 | Not sooner than September 22, 2027 | 

*Table 1 - Claude Sonnet 5.5 vs Opus 5.5 specs from Anthropic's models overview, as of September 2026.*

Every row comes from Anthropic's models overview. The default effort row matters more than it looks, because it changes what a default-versus-default comparison measures.

## Opus 5.5 leads on 7 of 8 benchmarks in Anthropic's launch comparison, by 3.2 percentage points or less

Opus 5.5 wins most of the benchmarks in Anthropic's Sonnet 5.5 launch comparison, but the margins are small. The cheaper model leads on the one agentic terminal test.

| **Benchmark** | **Sonnet 5.5** | **Opus 5.5** | **Leader and gap** | 
|---|---|---|---|
| Terminal-Bench 4.0 | 70.6% | 66.4% (Xhigh) | Sonnet 5.5 by 4.2 points | 
| FrontierCode 1.1 Main | 52.1% (Xhigh) | 54.4% | Opus 5.5 by 2.3 points | 
| CursorBench 4.0 | 55.5% | 57.8% | Opus 5.5 by 2.3 points | 
| GDPval-AA v2.1 (Elo) | 1844 | 1846 | Opus 5.5 by 2 Elo points | 
| AA-Briefcase v1.1 (Elo) | 1811 | 1822 | Opus 5.5 by 11 Elo points | 
| Humanity's Last Exam (with tools) | 64.5% | 67.7% | Opus 5.5 by 3.2 points | 
| OSWorld 2.1 (partial credit) | 80.1% | 81.8% | Opus 5.5 by 1.7 points | 
| Chartography (no tools) | 61.6% | 64.4% | Opus 5.5 by 2.8 points | 

*Table 2 - Benchmarks in Anthropic's Sonnet 5.5 launch comparison, as of September 2026. Artificial Analysis ran GDPval-AA and AA-Briefcase.*

The pattern holds across categories. Coding, business deliverables, computer use, and chart reading all sit within a few points. Sonnet 5.5's Terminal-Bench lead, which beat Opus 5.5's best run, favors it for multi-step work in a command-line setting. Opus 5.5's leads on FrontierCode and CursorBench mean it doesn't settle coding as a whole.

One quirk is worth knowing. Anthropic's launch post notes that Sonnet 5.5 scored lower on FrontierCode at Max effort (46.2%) than at Xhigh (52.1%). Anthropic says Max more often triggered a code-review workflow that, in the cases it examined, timed out or added edits beyond the task, which FrontierCode penalizes. For the full Opus 5.5 picture, see the Opus 5.5 benchmarks.

## Independent testing puts them 2 points apart, with one wide gap in factual accuracy

Independent results confirm how close the two models are, and they expose one real weakness in Sonnet 5.5. Artificial Analysis runs every model through the same standardized evaluations. Its setup differs from Anthropic's, but it gives a consistent independent comparison.

On its Intelligence Index, Opus 5.5 at Max effort scores 58 and Sonnet 5.5 at Max scores 56. Those are the top two scores the firm had recorded at Sonnet 5.5's launch.

The independent Terminal-Bench 4.0 run agrees with Anthropic on the winner. In that run, Sonnet 5.5 scored 64% against 60% for Opus 5.5, lower numbers than the launch post on a different test setup, but the same order.

Factual accuracy is where they split. On AA-Omniscience, Opus 5.5 scored 66% factual accuracy against 54% for Sonnet 5.5. That 12-point gap is far wider than anything in Anthropic's launch table. On the same test, Sonnet 5.5 did show a lower hallucination rate, at 47% against 59%. The two are separate metrics, so a lower hallucination rate doesn't offset lower accuracy.

For an app that answers questions from general knowledge, that gap matters more than any coding score.

*Also read our**Opus 5.5 vs Sonnet 5**comparison if Sonnet 5 is also on the table.*

## Sonnet 5.5 costs half as much per token, but not always half per task

Sonnet 5.5's token price is half of Opus 5.5's. Its cost per task is lower only up to a point, because Sonnet 5.5 uses far more tokens when you push it to its highest settings.

### 1. Token prices side by side

| **Price per 1M tokens** | **Claude Sonnet 5.5** | **Claude Opus 5.5** | 
|---|---|---|
| Input | $2 | $4 | 
| Output | $10 | $20 | 
| Cache writes (5 minutes) | $2.50 | $5 | 
| Cache hits | $0.20 | $0.20 | 

*Table 3 - Sonnet 5.5 vs Opus 5.5 API pricing from Anthropic, pricing as of September 2026.*

Opus 5.5 costs exactly double on input, output, and cache writes, while cache hits cost the same on both. The Sonnet 5.5 pricing breakdown covers batching, caching, and plans in detail.

### 2. The default effort settings are flipped

On the Claude API, Sonnet 5.5 defaults to High effort and Opus 5.5 defaults to Medium. Running both models with no settings changed puts the cheaper model at a higher effort level than the pricier one.

That skews most casual comparisons. A default-versus-default test pits Sonnet 5.5 working harder against Opus 5.5 working lighter, so the quality and cost gaps both look different than they are at matched settings.

Anthropic's own guidance for Sonnet 5.5 is to start at High for most work. For well-specified agentic tasks and latency-sensitive chat, it suggests starting at Medium.

### 3. At top quality, Opus 5.5 costs less per task

Matching the two models by score instead of by setting reverses the price story. The table below pairs each effort level with the cost per task that Artificial Analysis measured.

| **Effort** | **Sonnet 5.5 index score** | **Sonnet 5.5 cost per task** | **Opus 5.5 index score** | **Opus 5.5 cost per task** | 
|---|---|---|---|---|
| Low | 36 | $0.41 | 42 | $0.55 | 
| Medium | 41 | $0.59 | 51 | $1.34 | 
| High | 47 | $1.08 | 54 | $1.82 | 
| Xhigh | 52 | $2.74 | 56 | $3.46 | 
| Max | 56 | $7.60 | 58 | $5.98 | 

*Table 4 - Sonnet 5.5 vs Opus 5.5 Intelligence Index score and estimated cost per weighted index task by effort level, independently measured by Artificial Analysis with adaptive reasoning and default fallback, as of September 2026. Figures reflect Artificial Analysis's benchmark workload, not a production bill.*

At the low end, the two are close. At Medium, Sonnet 5.5 scores 41 for $0.59, and Opus 5.5 at Low scores 42 for $0.55.

Higher up, Opus 5.5 pulls ahead on value. It scores 51 at Medium for $1.34, while Sonnet 5.5 needs Xhigh to reach 52, at $2.74. Opus 5.5 at Xhigh matches Sonnet 5.5's best score of 56 for $3.46, less than half the $7.60 Sonnet 5.5 spends at Max.

Anthropic's launch post points the same way. It says Sonnet 5.5 at its higher settings can perform comparably to Opus 5.5 at a similar cost. These are benchmark workload figures, so treat them as a guide to direction rather than a forecast of your own bill.

## Sonnet 5.5 is faster, while Opus 5.5 always thinks before it answers

Anthropic rates Sonnet 5.5's latency as Fast and Opus 5.5's as Moderate. Artificial Analysis measured Sonnet 5.5's output speed at 85 to 139 tokens per second across effort levels, against 74 to 93 for Opus 5.5.

Speed per token is not the same as time per task. At high effort, Sonnet 5.5 writes many more tokens, so a faster stream does not guarantee a faster finish.

The thinking behavior differs too. Opus 5.5's adaptive thinking is always on. By contrast, Sonnet 5.5 decides how much to think, and it can run with up-front thinking turned off at Low, Medium, or High effort. For chat and support tools where the first word needs to appear quickly, that flexibility is Sonnet 5.5's edge.

## Switching models mid-conversation drops Sonnet 5.5's reasoning

Moving a conversation from Sonnet 5.5 to Opus 5.5 partway through costs you the reasoning Sonnet 5.5 built up. According to Anthropic's "What's new" docs, Opus 5.5 cannot read Sonnet 5.5's thinking blocks, and neither can any other model.

Thinking blocks are the model's working notes from earlier turns. When a conversation switches to a model that can't read them, the request still succeeds, but the new model continues without those notes. Anthropic doesn't bill the dropped blocks.

The reverse also applies. In the other direction, Sonnet 5.5 reads thinking blocks from Sonnet 5 and older models, but not from Opus 5.5.

The practical rule is simple: pick the model at the start of a project or workflow, and keep it. A long agent task that starts on Sonnet 5.5 and escalates to Opus 5.5 halfway through loses context it has already paid for.

## Sonnet 5.5 wins on well-scoped work, Opus 5.5 on judgment calls

Sonnet 5.5 is the right default for clear, repeatable tasks at volume. Opus 5.5 earns its price on work that is open-ended, fact-heavy, or needs top-quality output. Anthropic says Opus 5.5 remains clearly stronger at complex work that requires sustained judgment.

| **Use case** | **Better pick** | **Why** | 
|---|---|---|
| Support replies and ticket triage | Sonnet 5.5 at Medium | Faster, and half the token price for high-volume, clear tasks | 
| Bug fixes and scoped feature changes | Sonnet 5.5 at High | Within 2.3 points of Opus 5.5 on CursorBench | 
| Long multi-step agent tasks | Sonnet 5.5 at Xhigh | Leads Opus 5.5 on Terminal-Bench 4.0 in both vendor and independent runs | 
| Reports, documents, and spreadsheets | Sonnet 5.5 | Within 2 Elo points on GDPval-AA | 
| Answers from general knowledge | Opus 5.5 | 12-point lead on AA-Omniscience factual accuracy | 
| Open-ended planning and strategy | Opus 5.5 | Anthropic says Opus 5.5 is clearly stronger at sustained judgment | 
| Top-quality output where cost matters | Opus 5.5 at Medium or Xhigh | Reaches Sonnet 5.5's higher index scores for less per task | 

*Table 5 - When to choose Sonnet 5.5 or Opus 5.5, based on Anthropic's published data and Artificial Analysis results, as of September 2026.*

Anthropic's models overview recommends starting with Opus 5.5 for most workloads when you're unsure. For high-volume, well-specified work, testing Sonnet 5.5 at High first is the cost-conscious alternative. If Sonnet 5.5 falls short at that setting, move to Opus 5.5 rather than pushing Sonnet 5.5 to Max.

## Pick your Claude model once, when you set up the build

The Sonnet 5.5 vs Opus 5.5 choice is a question of fit. Sonnet 5.5 handles well-scoped work at half the token price and higher speed. Opus 5.5 wins on factual accuracy and judgment, and it reaches top-quality output for less per task than Sonnet 5.5 at Max.

Sonnet 5.5 is available on Emergent and you can also use it through the Universal LLM Key, so the apps you build can call it without a separate Anthropic account or API key. Usage is billed through Emergent Credits. When you create a custom agent, you choose the language model it reasons with at setup, which fits the rule above: pick the right model for the job once, then keep it.

It suits the work Emergent builders ship most, like a client portal that drafts support replies at volume or an internal tool that turns spreadsheets into reports.
