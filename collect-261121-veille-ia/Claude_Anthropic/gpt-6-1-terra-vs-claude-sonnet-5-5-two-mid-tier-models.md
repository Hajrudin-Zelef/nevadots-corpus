---
id: collect-261121-veille-ia/claude-anthropic/gpt-6-1-terra-vs-claude-sonnet-5-5-two-mid-tier-models
title: "GPT-6.1 Terra vs. Claude Sonnet 5.5: Two Mid-Tier Models"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-26"]
keywords: ["terra", "sonnet", "claude", "gpt-6", "tokens", "effort", "mid-tier", "models"]
source: https://www.datacamp.com/blog/gpt-6-1-terra-vs-claude-sonnet-5-5
source_anchor: ""
source_lines: [1, 648]
section: "Claude_Anthropic"
---
GPT-6.1 Terra vs. Claude Sonnet 5.5: Compare Two Mid-Tier Models

GPT-6.1 Terra costs half as much as Claude Sonnet 5.5, but Sonnet leads on coding and computer use. We compare benchmarks, pricing, speed, and use cases.

As of the last month, OpenAI and Anthropic now both have a strong new mid-tier model.

Both GPT-6.1 Terra and. Claude Sonnet 5.5 sit one step below their companies' flagships, and both are pitched as the everyday workhorse: fast enough for interactive use but cheap enough to run at scale.

That said, they do have trade-offs. Terra is about half the price per token and uses far fewer tokens per task. Sonnet 5.5 is the stronger model, especially at higher effort settings on coding and computer-use tasks.

In this article, I'll compare GPT-6.1 Terra and Claude Sonnet 5.5 on specs, pricing, benchmarks, speed, and cost per task, and help you decide which one fits your workload.

What Is GPT-6.1 Terra?

OpenAI's lineup now has four tiers, from smallest to largest: Luna, Terra, Sol, and Astra. Terra replaces GPT-5.6 Terra, which was left out of September's GPT-6 update, and it costs 50% less than its predecessor.

6.1 Terra has three focus areas:

Computer use: Understanding what's on screen and taking actions across apps

Visual reasoning: Interpreting images, charts, and technical drawings

Writing: Clearer, more natural communication

OpenAI also reports a large drop in factual errors. On its internal set of difficult prompts, Terra's error rate at max effort is 4.3%, down from 22.3% for GPT-5.6 Terra and roughly level with GPT-6.1 Sol.

Terra is available in ChatGPT (for Plus, Pro, Enterprise, and Edu users), in Codex, and through the API as gpt-6.1-terra. It's also rolling out in GitHub Copilot and on Amazon Bedrock.

What Is Claude Sonnet 5.5?

Claude Sonnet 5.5 is the second model in Anthropic's Claude 5.5 family, positioned as a faster, cheaper complement to Claude Opus 5.5. It's a big step up from Sonnet 5: Anthropic says it runs more than 30% faster and costs up to 30% less per task, partly because it batches tool calls.

On several benchmarks, Sonnet 5.5 comes close to Opus 5.5. For example, it scores 1844 on GDPval-AA (knowledge work), just two points below Opus, although we know Opus 5.5 is overall clearly stronger at complex, open-ended work.

Anthropic highlights Sonnet 5.5's strengths in coding and bug fixing, agentic terminal work, design, slides, and spreadsheets. I

Sonnet 5.5 is available in the Claude apps, Claude Code, and the Claude API as claude-sonnet-5-5, as well as on Amazon Bedrock, Google Cloud, and Microsoft Azure.

GPT-6.1 Terra vs. Claude Sonnet 5.5 Specs and Pricing

GPT-6.1 Terra

Claude Sonnet 5.5

Release date

October 26, 2026

September 28, 2026

Input price (per 1M tokens)

$1.00 ($2.00 above 272K)

$2.00

Output price (per 1M tokens)

$5.00 ($7.50 above 272K)

$10.00

Cached input (per 1M tokens)

$0.05

$0.10

Context window

1.05M tokens

1M tokens

Max output

128K tokens

128K tokens

Effort levels

none, low, medium, high, xhigh, max

low, medium, high, xhigh, max

Input

Text, images

Text, images

At list price, Terra costs half as much per token for normal prompts. For very long prompts (above 272K tokens), Terra's rates rise, and the gap disappears.

Per-token price is only half the story. As we'll see below, the two models use very different numbers of tokens to complete the same task.

Benchmarks: GPT-6.1 Terra vs. Claude Sonnet 5.5

Vendor-reported results

OpenAI's launch charts compare Terra directly with Sonnet 5.5 on two agentic benchmarks at each effort level. That was part of the inspiration for this article.

Benchmark

Effort

GPT-6.1 Terra

Claude Sonnet 5.5

OSWorld 2.1 (computer use)

Medium

66.7%

65.9%

High

68.7%

73.3%

Max

74.2%

83.9%

Terminal-Bench 4.0 (agentic coding)

Medium

35.8%

28.8%

High

41.8%

43.0%

Max

50.3%

70.6%

We can see a pattern. At medium effort, Terra matches or beats Sonnet 5.5. As effort increases, Sonnet pulls well ahead, and at max effort it leads by about 10 or 20 points.

Independent results

Artificial Analysis runs both models through the same test suite. At max effort:

Benchmark

GPT-6.1 Terra

Claude Sonnet 5.5

Artificial Analysis Intelligence Index

48

56

GDPval-AA (knowledge work, Elo)

1476

1838

Terminal-Bench 4.0

56%

64%

Humanity's Last Exam

46%

55%

SciCode

55%

61%

AA-LCR (long-context reasoning)

83%

83%

Sonnet 5.5 wins most of these, with its biggest lead on GDPval-AA, which measures real-world professional tasks. The two tie on long-context reasoning.

Effort level matters here too. On the Intelligence Index, both models score 36 at low effort and 41 at medium effort. Sonnet only pulls ahead at high effort and above.

GPT-6.1 Terra vs. Claude Sonnet 5.5 in Cost Per Task and Token Efficiency

This is where Terra looks good. Sonnet 5.5 at max effort thinks a lot: Artificial Analysis measured about 197,000 tokens per task for Sonnet vs. about 35,000 for Terra.

Combine that with Terra's lower per-token price, and the gap in cost per task is actually very large:

Effort

Terra index score (cost per task)

Sonnet 5.5 index score (cost per task)

Low

36 ($0.05)

36 ($0.35)

Medium

41 ($0.10)

41 ($0.48)

High

44 ($0.13)

47 ($0.88)

Max

48 ($0.40)

56 ($5.46)

At medium effort, Terra gets the same index score for roughly a fifth of the cost. At max effort, Sonnet scores 8 points higher, but each task costs more than 13 times as much.

OpenAI's own charts show the same thing. At max effort, an OSWorld task cost $1.19 with Terra vs. $5.74 with Sonnet, and a Terminal-Bench task cost $1.84 vs. $10.44.

GPT-6.1 Terra vs. Claude Sonnet 5.5 in Speed

Speed is interesting story because it depends on what you measure:

Output speed: Sonnet 5.5 generates tokens faster, at about 141 tokens per second vs. 91 for Terra.

Time per task: Terra finishes tasks faster because it writes far fewer tokens. The average task took 384 seconds with Terra vs. 878 seconds with Sonnet, according to the Artificial Analysis Index.

So if you're streaming long answers to users, Sonnet feels quicker. If you're running agentic tasks end to end, Terra is expected to finish first.

Testing GPT-6.1 Terra and Claude Sonnet 5.5

I asked both to write the same four-week workout plan, using the same prompt, high reasoning effort, and one attempt each.

The plan was for an experienced lifter with a shoulder injury, and it had to meet these constraints:

Banned exercises: No overhead pressing, dips, behind-the-neck movements, or barbell bench press.

A conflicting request: He still wants to train chest pressing, because chest is his weakest area.

Two locations: A full gym on Monday and Thursday. At home on Tuesday and Saturday, with only dumbbells up to 30 kg, a flat bench, a pull-up bar, and resistance bands.

Balanced training: Six movement patterns, each at least twice a week: hip hinge, squat or lunge, horizontal push, vertical push, horizontal pull, and vertical pull.

Format: Four weeks, about 60 minutes per session, under 1,000 words.

Progressive load: The excercises have to increase in volume, like sets, reps, weight, or complexity.

Part of the catch is that vertical push usually means pressing overhead, which is banned.

Here's how the two plans compare:

GPT-6.1 Terra

Claude Sonnet 5.5

Banned exercises

None used

None used

Chest pressing

Dumbbell, machine, and floor presses

Same, with about 20 chest sets a week

Vertical push

Dropped it and said why; added a triceps exercise instead

Kept it with angled presses that stop below head height

Home-day equipment

Home equipment only

Home equipment only

Weekly progression

One rep range, plus a rule for when to add weight

Sets and reps listed for each week

Session length

5–6 exercises, fits 60 minutes

7 exercises, tight for 60 minutes

Cost of this run

$0.024

$0.16

Overall score (out of 5)

4.8

4.8

6.1 Terra's home sessions show a difference: A triceps exercise is standing in for the vertical push in bold:

Exercise

Pattern

Sets × reps (weeks 1–3)

Dumbbell Romanian deadlift

Hip hinge

3 × 8–12

Dumbbell split squat

Knee-dominant

2 × 8–12 per leg

Neutral-grip dumbbell floor press

Horizontal push

3 × 8–12

Pull-up, band-assisted if needed

Vertical pull

3 × 6–10

One-arm dumbbell row, bench-supported

Horizontal pull

2 × 10–15 per side

Band triceps pressdown, anchored high on pull-up bar

Downward push; vertical-push substitute

2 × 10–15

Sonnet 5.5's wrote in a below-head-height vertical push in bold:

Exercise

Pattern

W1

W2

W3

W4

Single-leg dumbbell RDL

Hip hinge

3×10

3×12

4×12

2×10

Rear-foot-elevated split squat

Knee-dominant

3×10

3×12

4×12

2×10

Neutral-grip dumbbell floor press

Horizontal push

3×12

3×14

4×14

2×12

One-arm dumbbell row (bench-supported)

Horizontal pull

3×12

3×14

3×14

2×12

Half-kneeling band press, anchored low, 45°

Vertical push

3×12

3×14

3×14

2×12

Chin-up

Vertical pull

3×6

3×8

4×8

2×6

Band chest fly, low-to-high

Accessory (chest)

2×15

2×15

3×15

1×15

Both plans are safe and usable. Sonnet 5.5 showed better judgment by finding a safe way to keep vertical push, where Terra dropped it.

Terra's plan is leaner and cost about a sixth as much. However, Terra overall ignored the final requirement, which was to increase volume / load by increasing sets, reps, weight, or complexity. Sonnet 5.5's plan shows more of a clear progression: 3x10 becomes 3x12, which becomes 4x12, etc.

GPT-6.1 Terra vs. Claude Sonnet 5.5: Which Should You Use?

Choose GPT-6.1 Terra if:

You run high-volume workloads where cost per task matters most

Your tasks are everyday knowledge work: email, summaries, documents, and short Q&A

You mostly use low or medium effort, where Terra matches Sonnet for much less

You need visual reasoning or fast, cheap computer use at moderate difficulty

You're already in the OpenAI ecosystem (ChatGPT, Codex, Copilot)

Choose Claude Sonnet 5.5 if:

You're doing long agentic coding sessions or hard debugging

You need the best results on complex, multi-step computer use

Your work involves professional deliverables like analyses, slides, and spreadsheets, where Sonnet leads on GDPval-AA

You're willing to pay more for higher effort settings to get better results

You're already in the Anthropic ecosystem (Claude Code, Claude apps)

Final Thoughts

Sonnet 5.5 is the more capable model, with a large lead on hard coding and computer-use tasks at high effort. Terra is the more efficient one, matching Sonnet at medium effort for a fraction of the cost.

In my own hands-on test at high effort, both models wrote a safe training plan around a tricky shoulder injury, but Sonnet 5.5 showed better judgment and created the better program in my opinion. But both plans were good and Terra's cost a fraction as much. The best way to choose is to run both on a sample of your own tasks, at the effort level you'd actually use, and compare quality against cost per task, like I did here.

Author

Josef Waples

I'm a data science editor with contributions to research articles in scientific journals. I'm especially interested in linear algebra, statistics, R, and the like.

FAQs

Is GPT-6.1 Terra cheaper than Claude Sonnet 5.5?⌄

Yes. Terra costs $1 per million input tokens and $5 per million output tokens, half of Sonnet 5.5's $2 and $10. Because Terra also uses fewer tokens per task, the difference in cost per task is usually even larger.

Which is better for coding, GPT-6.1 Terra or Claude Sonnet 5.5?⌄

Sonnet 5.5, especially at high effort. It scores 70.6% on Terminal-Bench 4.0 at max effort vs. 50.3% for Terra. At medium effort, though, Terra scores slightly higher (35.8% vs. 28.8%).

Which is better for computer use?⌄

Sonnet 5.5 leads at high effort, scoring 83.9% on OSWorld 2.1 at max effort vs. 74.2% for Terra in OpenAI's charts. At medium effort, the two are roughly tied, and Terra is much cheaper.

What context window do GPT-6.1 Terra and Claude Sonnet 5.5 have?⌄

Both support about 1 million tokens of context (1.05M for Terra, 1M for Sonnet 5.5) and up to 128K output tokens.

Which model is faster?⌄

Sonnet 5.5 generates tokens faster, but Terra usually finishes tasks sooner because it produces far fewer tokens. On Artificial Analysis's tests at max effort, Terra's average task took less than half as long.

Where can I use GPT-6.1 Terra and Claude Sonnet 5.5?⌄

Terra is available in ChatGPT, Codex, the OpenAI API, GitHub Copilot, and Amazon Bedrock. Sonnet 5.5 is available in the Claude apps, Claude Code, the Claude API, Amazon Bedrock, Google Cloud, and Microsoft Azure.

GPT-5.6 Terra and Claude Sonnet 5 are the mid-tier defaults from OpenAI and Anthropic, priced within a dollar of each other. I compare them across coding scores, long-context pricing, and more.

Same list price, different fine print. We compare GPT-6.1 Sol and Claude Sonnet 5.5 on benchmarks, cache and long-context costs, cloud access, and a hands-on coding test.

Sonnet 5 is available now at a discount. GPT-5.6 posts stronger numbers in Terminal-Bench-2.1 but isn't generally available yet. Here's the full breakdown.

Two same-day efficiency releases, priced two to one. We compare GPT-6 Sol and Claude Opus 5.5 on benchmarks, real workload costs, access, and a hands-on Tetris build.

As of the last month, OpenAI and Anthropic now both have a strong new mid-tier model.

Both GPT-6.1 Terra and. Claude Sonnet 5.5 sit one step below their companies' flagships, and both are pitched as the everyday workhorse: fast enough for interactive use but cheap enough to run at scale.

That said, they do have trade-offs. Terra is about half the price per token and uses far fewer tokens per task. Sonnet 5.5 is the stronger model, especially at higher effort settings on coding and computer-use tasks.

In this article, I'll compare GPT-6.1 Terra and Claude Sonnet 5.5 on specs, pricing, benchmarks, speed, and cost per task, and help you decide which one fits your workload.

## What Is GPT-6.1 Terra?

OpenAI's lineup now has four tiers, from smallest to largest: Luna, Terra, Sol, and Astra. Terra replaces GPT-5.6 Terra, which was left out of September's GPT-6 update, and it costs 50% less than its predecessor.

6.1 Terra has three focus areas:

Computer use:Understanding what's on screen and taking actions across appsVisual reasoning:Interpreting images, charts, and technical drawingsWriting:Clearer, more natural communication
OpenAI also reports a large drop in factual errors. On its internal set of difficult prompts, Terra's error rate at max effort is 4.3%, down from 22.3% for GPT-5.6 Terra and roughly level with GPT-6.1 Sol.

Terra is available in ChatGPT (for Plus, Pro, Enterprise, and Edu users), in Codex, and through the API as

`gpt-6.1-terra`. It's also rolling out in GitHub Copilot and on Amazon Bedrock.
## What Is Claude Sonnet 5.5?

Claude Sonnet 5.5 is the second model in Anthropic's Claude 5.5 family, positioned as a faster, cheaper complement to Claude Opus 5.5. It's a big step up from Sonnet 5: Anthropic says it runs more than 30% faster and costs up to 30% less per task, partly because it batches tool calls.

On several benchmarks, Sonnet 5.5 comes close to Opus 5.5. For example, it scores 1844 on GDPval-AA (knowledge work), just two points below Opus, although we know Opus 5.5 is overall clearly stronger at complex, open-ended work.

Anthropic highlights Sonnet 5.5's strengths in coding and bug fixing, agentic terminal work, design, slides, and spreadsheets. I

Sonnet 5.5 is available in the Claude apps, Claude Code, and the Claude API as

`claude-sonnet-5-5`, as well as on Amazon Bedrock, Google Cloud, and Microsoft Azure.
## GPT-6.1 Terra vs. Claude Sonnet 5.5 Specs and Pricing

At list price, Terra costs half as much per token for normal prompts. For very long prompts (above 272K tokens), Terra's rates rise, and the gap disappears.

Per-token price is only half the story. As we'll see below, the two models use very different numbers of tokens to complete the same task.

## Benchmarks: GPT-6.1 Terra vs. Claude Sonnet 5.5

## Vendor-reported results

OpenAI's launch charts compare Terra directly with Sonnet 5.5 on two agentic benchmarks at each effort level. That was part of the inspiration for this article.

66.7%73.3%83.9%35.8%43.0%70.6%
We can see a pattern. At medium effort, Terra matches or beats Sonnet 5.5. As effort increases, Sonnet pulls well ahead, and at max effort it leads by about 10 or 20 points.

## Independent results

Artificial Analysis runs both models through the same test suite. At max effort:

Sonnet 5.5 wins most of these, with its biggest lead on GDPval-AA, which measures real-world professional tasks. The two tie on long-context reasoning.

Effort level matters here too. On the Intelligence Index, both models score 36 at low effort and 41 at medium effort. Sonnet only pulls ahead at high effort and above.

## GPT-6.1 Terra vs. Claude Sonnet 5.5 in Cost Per Task and Token Efficiency

This is where Terra looks good. Sonnet 5.5 at max effort thinks a lot: Artificial Analysis measured about 197,000 tokens per task for Sonnet vs. about 35,000 for Terra.

Combine that with Terra's lower per-token price, and the gap in cost per task is actually very large:

At medium effort, Terra gets the same index score for roughly a fifth of the cost. At max effort, Sonnet scores 8 points higher, but each task costs more than 13 times as much.

OpenAI's own charts show the same thing. At max effort, an OSWorld task cost $1.19 with Terra vs. $5.74 with Sonnet, and a Terminal-Bench task cost $1.84 vs. $10.44.

## GPT-6.1 Terra vs. Claude Sonnet 5.5 in Speed

Speed is interesting story because it depends on what you measure:

Output speed:Sonnet 5.5 generates tokens faster, at about 141 tokens per second vs. 91 for Terra.Time per task:Terra finishes tasks faster because it writes far fewer tokens. The average task took 384 seconds with Terra vs. 878 seconds with Sonnet, according to the Artificial Analysis Index.
So if you're streaming long answers to users, Sonnet feels quicker. If you're running agentic tasks end to end, Terra is expected to finish first.

## Testing GPT-6.1 Terra and Claude Sonnet 5.5

I asked both to write the same four-week workout plan, using the same prompt, high reasoning effort, and one attempt each.

The plan was for an experienced lifter with a shoulder injury, and it had to meet these constraints:

Banned exercises:No overhead pressing, dips, behind-the-neck movements, or barbell bench press.A conflicting request:He still wants to train chest pressing, because chest is his weakest area.Two locations:A full gym on Monday and Thursday. At home on Tuesday and Saturday, with only dumbbells up to 30 kg, a flat bench, a pull-up bar, and resistance bands.Balanced training:Six movement patterns, each at least twice a week: hip hinge, squat or lunge, horizontal push, vertical push, horizontal pull, and vertical pull.Format:Four weeks, about 60 minutes per session, under 1,000 words.Progressive load:The excercises have to increase in volume, like sets, reps, weight, or complexity.
Part of the catch is that vertical push usually means pressing overhead, which is banned.

Here's how the two plans compare:

6.1 Terra's home sessions show a difference: A triceps exercise is standing in for the vertical push in bold:

Band triceps pressdown, anchored high on pull-up barDownward push; vertical-push substitute
Sonnet 5.5's wrote in a below-head-height vertical push in bold:

Both plans are safe and usable. Sonnet 5.5 showed better judgment by finding a safe way to keep vertical push, where Terra dropped it.

Terra's plan is leaner and cost about a sixth as much. However, Terra overall ignored the final requirement, which was to increase volume / load by increasing sets, reps, weight, or complexity. Sonnet 5.5's plan shows more of a clear progression: 3x10 becomes 3x12, which becomes 4x12, etc.

## GPT-6.1 Terra vs. Claude Sonnet 5.5: Which Should You Use?

## Choose GPT-6.1 Terra if:

high-volume workloadswhere cost per task matters mosteveryday knowledge work: email, summaries, documents, and short Q&Alow or medium effort, where Terra matches Sonnet for much lessvisual reasoningor fast, cheapcomputer useat moderate difficultyOpenAI ecosystem(ChatGPT, Codex, Copilot)
## Choose Claude Sonnet 5.5 if:

long agentic coding sessionsor hard debuggingcomplex, multi-step computer useprofessional deliverableslike analyses, slides, and spreadsheets, where Sonnet leads on GDPval-AAhigher effort settingsto get better resultsAnthropic ecosystem(Claude Code, Claude apps)
## Final Thoughts

Sonnet 5.5 is the more capable model, with a large lead on hard coding and computer-use tasks at high effort. Terra is the more efficient one, matching Sonnet at medium effort for a fraction of the cost.

In my own hands-on test at high effort, both models wrote a safe training plan around a tricky shoulder injury, but Sonnet 5.5 showed better judgment and created the better program in my opinion. But both plans were good and Terra's cost a fraction as much. The best way to choose is to run both on a sample of your own tasks, at the effort level you'd actually use, and compare quality against cost per task, like I did here.
