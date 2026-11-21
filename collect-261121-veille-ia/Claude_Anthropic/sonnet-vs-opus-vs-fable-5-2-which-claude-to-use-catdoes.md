---
id: collect-261121-veille-ia/claude-anthropic/sonnet-vs-opus-vs-fable-5-2-which-claude-to-use-catdoes
title: "Sonnet vs Opus vs Fable 5.2: Which Claude to Use - CatDoes"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "opus", "tokens", "sonnet", "claude", "haiku", "effort", "model"]
source: https://catdoes.com/blog/sonnet-vs-opus-vs-fable
source_anchor: ""
source_lines: [1, 237]
section: "Claude_Anthropic"
---
# Sonnet vs Opus vs Fable 5.2: Which Claude to Use

Sonnet 5.5 vs Opus 5.5 vs Fable 5.2 (and Haiku 5.5) compared: API prices, speed, effort levels and three real task results, so you know which Claude to use.

Nafis Amiri

Co-Founder of CatDoes

Picking a Claude model used to take five seconds. Opus for hard problems, Sonnet for everything else, Haiku when the bill mattered. Then Anthropic replaced all four models in seven weeks, ending with Claude Fable 5.2 on November 9, and the Sonnet vs Opus vs Fable question got harder: on some real tasks, the cheaper model now wins.

This guide compares Claude Sonnet 5.5, Opus 5.5, Fable 5.2 and Haiku 5.5 on price per million tokens, context window, speed, and what each one does best. You will see how they did on the same three tasks, followed by a short "pick this if" list. The short answer: try Sonnet 5.5 first, keep Opus 5.5 for long agent runs, and pay for Fable 5.2 only when Opus falls short.

## TL;DR

- **Sonnet 5.5** ($2 / $10 per million input / output tokens) is the best value. It finished within half a point of Fable 5.2 on the Vals Index and second at building web apps, for less than Fable or Opus on both.
- **Opus 5.5** ($4 / $20) is Anthropic's recommended starting point and the default model in Claude Code. Pick it for long, open-ended agent work.
- **Fable 5.2** ($8 / $40) is Anthropic's newest and most capable model open to all customers, and 20% cheaper than Fable 5.1. It ranks first on the Vals Index and Vibe Code Bench. Use it when Opus at high effort still falls short.
- **Haiku 5.5** (from $0.10 / $0.50) is for high-volume work, and it is far better than Haiku 4.5: 90.44% at building a web app, up from 11.39%, for $6.07 per app.
- **Mythos 5.2** is the same model as Fable 5.2 with looser safeguards, open only to verified security and life-sciences organizations.

## Table of Contents

- Sonnet vs Opus vs Fable vs Haiku: Comparison Table
- Claude Sonnet 5.5: Best Value for Everyday Work
- Claude Opus 5.5: Anthropic's Default Pick
- Claude Fable 5.2: Most Capable, Now Cheaper
- Claude Haiku 5.5: Cheapest, and Much Better
- Which Claude Model Should You Use? Pick This If
- Frequently Asked Questions
- The Bottom Line

*How we put this together: we did not run these models ourselves for this guide. Prices, limits and plan details come from Anthropic's documentation, and speed and cost-per-task figures come from Artificial Analysis.*

*The three task results come from Vals AI and Simon Willison, who ran the same prompts on these models and published the outputs. Fable 5.2 is new enough that two of those tests haven't been run on it yet, so we show Fable 5.1's result there, labeled. Effort settings change these numbers a lot, so we name the setting every time.*

## Sonnet vs Opus vs Fable vs Haiku: Comparison Table

Opus 5.5 costs twice as much per token as Sonnet 5.5, and Fable 5.2 costs twice as much as Opus. Haiku 5.5 costs a twentieth of Sonnet's rate for prompts up to 100,000 tokens. All four now share a 1M-token context window, 128K tokens of output and a June 2026 knowledge cutoff.

### Price, Context Window and Speed

|  | Sonnet 5.5 | Opus 5.5 | Fable 5.2 | Haiku 5.5 | 
|---|---|---|---|---|
| Price per 1M tokens (input / output) | $2 / $10 | $4 / $20 | $8 / $40 | $0.10 / $0.50 (prompts over 100K: $0.50 / $2.50) | 
| Cached input per 1M tokens | $0.10 | $0.20 | $0.20 | $0.01 | 
| Context window | 1M tokens | 1M tokens | 1M tokens | 1M tokens | 
| Max output | 128K tokens | 128K tokens | 128K tokens | 128K tokens | 
| Anthropic's latency rating | Fast | Moderate | Moderate | Fastest | 
| Default effort on the API | high | medium | high | medium | 
| Output speed at default effort | 137 tokens/s | 103 tokens/s | 92 tokens/s | 218 tokens/s | 
| Intelligence Index at default effort | 42 | 48 | 62 | 25 | 
| Cost per index task at default effort | $1.64 | $2.45 | $9.08 | $0.09 | 
| On Claude plans | Free and up | Pro and up | Max and Ultra (Pro via usage credits) | Free and up | 
| Released | Sep 28, 2026 | Sep 22, 2026 | Nov 9, 2026 | Oct 7, 2026 | 

Prices, limits and latency ratings are from Anthropic's models overview and pricing page, and plan access is from claude.com/pricing. Output speed, the Intelligence Index (v5.0.1) and cost per task come from Artificial Analysis, measured at each model's API default effort.

Two rows deserve a second look. At default settings, Fable 5.2 scores 62 on the Intelligence Index against 48 for Opus 5.5, but costs $9.08 per task against $2.45. Push Opus to max effort and it reaches 59 for $10.04 per task: more than Fable at its default, for a lower score.

### Three Tasks, Four Models

Specs only go so far, so here are three tasks that independent testers ran on these models. The first is app building: Vals AI's Vibe Code Bench asks each model to build a complete web app from a spec, then an agent clicks through it to check that the features work.

The second is Simon Willison's "Generate an SVG of a pelican riding a bicycle" prompt, a quick test of one-shot code output. The third is Vals' MedScribe, which grades clinical notes against 100 rubrics, a stand-in for high-volume writing.

| Task | Sonnet 5.5 | Opus 5.5 | Fable 5.2 | Haiku 5.5 | 
|---|---|---|---|---|
| Build a web app (Vals Vibe Code Bench) | 92.39%, $19.73, 61 min (max effort) | 90.29%, $35.77, 96 min (max effort) | 93.77%, $22.13, 41 min (high effort) | 90.44%, $6.07, 35 min (max effort) | 
| Pelican SVG (Willison, xhigh effort) | $0.057, 41 s | $0.24, 1 min 54 s | Not tested yet (Fable 5.1: $1.83, 7 min 51 s) | 10,524 output tokens, 49 s | 
| Clinical notes (Vals MedScribe, max effort) | 91.10%, $0.51 per note | 91.43%, $1.15 per note | Not tested yet (Fable 5.1: 91.29%, $0.96 per note) | 89.54%, $0.04 per note | 

The most expensive model was not the best app builder. Fable 5.2 scored highest, at 62% of Opus's cost per app and in 42% of the time, even though Vals ran it at high effort and the others at max. Sonnet 5.5 came second for less than either, and Haiku 5.5 scored 90.44%, where Haiku 4.5 managed 11.39%.

On the clinical notes, the three models tested finished within two points of each other, so price decides it. Willison has not posted a Fable 5.2 pelican yet, and Vals has not run Fable 5.2 on MedScribe.

## Claude Sonnet 5.5: Best Value for Everyday Work

Sonnet 5.5 is the model most people should try first. It costs half as much as Opus 5.5 and a quarter as much as Fable 5.2, and on every scored test in this guide it either beat Opus or finished within a point and a half of the leader.

Anthropic released Sonnet 5.5 on September 28, saying it "runs 30%+ faster, and costs up to 30% less for most work" than Sonnet 5. The per-token price stayed at $2 / $10, with a 1M-token context window and 128K tokens of output. On October 7, Anthropic halved its cache-read price to $0.10 per million tokens. It is also on the Claude Free plan, and Willison notes it is now the model behind the claude.ai free tier.

The Vals Index puts Sonnet 5.5 at 70.85%, between Fable 5.2 (71.31%) and Opus 5.5 (70.14%). All three sit within about one standard error of each other, so read it as a three-way tie. Sonnet ran the index for $16.18 per test, against $17.94 for Fable and $24.77 for Opus. Vals ran Fable at high effort and the other two at max.

Anthropic says Sonnet 5.5 is strongest at well-scoped everyday tasks, fixing bugs, and polished documents, slides and spreadsheets.

### How Sonnet 5.5 Did on the Three Tasks

- **App building:** 92.39%, second of the four, at $19.73 and 61 minutes per app.
- **Pelican SVG:** 5.74 cents and 41 seconds at xhigh effort. Willison called it "good," with a "correct bicycle frame, legs either side of the frame, feet touching the pedals, chain in the right place," and "a misshapen blue bicycle helmet." At max effort it spent all 128,000 output tokens thinking ($1.28) and returned no SVG.
- **Clinical notes:** 91.10% at $0.51 per note, a third of a point behind Opus for 44% of the cost. On Vals' MedCode billing-code test, it beat Opus outright, 52.92% to 49.80%.

There are two catches. Anthropic itself says "Opus 5.5 remains clearly stronger at complex, open-ended work requiring sustained judgment." And max effort is expensive on Sonnet: Artificial Analysis measured $10.02 per task at max, level with Opus 5.5 at max ($10.04) and about six times Sonnet's own cost at its default ($1.64).

Choosing a coding tool rather than a model? Our Claude Code vs Cursor comparison covers that.

## Claude Opus 5.5: Anthropic's Default Pick

Opus 5.5 is the model Anthropic tells developers to start with: "If you're unsure which model to use, start with Claude Opus 5.5 for most workloads." Even the Fable 5.2 launch post calls it Anthropic's "strongest daily driver." It is also the default model in Claude Code on Pro, Max, Ultra, Team, Enterprise and the API.

It costs $4 / $20 per million tokens, cheaper than Opus 5 at $5 / $25. Anthropic's launch post says it "generates output more than 30% faster than Opus 5." Fast mode doubles the price to $8 / $40 in exchange for quicker output.

Opus 5.5 defaults to medium effort on the API, and Anthropic's own testing shows it still holds up against Fable 5.2 on cost. In its cost guide, on a SWE-bench Pro subset, Opus 5.5 at its default solved 93.1% of tasks for $0.22 per solved task. At xhigh it solved 96.5% for $0.75, "level with Fable 5.2 at high for a little over half the cost" ($1.39).

Opus also still wins two of Anthropic's own knowledge-work ratings. The Fable 5.2 system card says the new model "scored highest on 14 of 16 results," trailing Opus 5.5 only on GDPval-AA and the AA-Briefcase Elo rating. For how Claude Code on Opus compares with OpenAI's agent, see our Claude Code vs Codex breakdown.

### How Opus 5.5 Did on the Three Tasks

- **App building:** 90.29% at $35.77 and 96 minutes per app, the most expensive and slowest run of the four.
- **Pelican SVG:** 15 seconds at low effort, 25 at medium, and 1 minute 54 seconds at xhigh for 24 cents. At max it thought through all 128,000 output tokens and returned nothing. Willison tried twice, at $2.56 a try and nearly 20 minutes each.
- **Clinical notes:** 91.43%, the top score by a hair, at $1.15 per note.

The lesson from the pelican runs: max effort is not a free upgrade. On both Opus 5.5 and Sonnet 5.5 it burned the whole output budget on a simple prompt.

## Claude Fable 5.2: Most Capable, Now Cheaper

Fable 5.2 is Anthropic's most capable model open to all customers. It costs twice as much as Opus 5.5 per token but 20% less than Fable 5.1, and it ranks first on both Vals tests that have scored it so far.

Anthropic launched Fable 5.2 on November 9 alongside Mythos 5.2. The price is $8 / $40 per million tokens, and cached input costs $0.20, just 2.5% of the input rate. Anthropic says the new models "generate outputs up to 30% faster than Claude Fable 5.1." Artificial Analysis measured 92 tokens per second at its default high effort, still the slowest of the four.

Anthropic's advice hasn't changed. Its models overview reserves Fable 5.2 "for demanding reasoning and long-horizon agentic work, or when your evals on Claude Opus 5.5 at higher effort still fall short." On claude.ai, Fable is not on the Free plan, Pro pays for it with usage credits, and Max and Ultra include it up to half the weekly limit. In Claude Code it needs version 2.1.321 or later and is never the default, so you pick it with /model. On the API it requires 30-day data retention and isn't available under zero data retention unless Anthropic authorizes it.

### What Changed From Fable 5.1

Three things changed besides the price: speed, safety blocks and scores. Anthropic reports "about 55% fewer blocks with Fable 5.2 compared with Fable 5.1," and its launch table shows Fable 5.2 ahead of Fable 5.1 on every result. These are Anthropic's own numbers, not independent tests.

| Anthropic's benchmark | Fable 5.2 | Fable 5.1 | Opus 5.5 | 
|---|---|---|---|
| FrontierCode 2.0 (agentic coding) | 49.1% (xhigh) | 32.4% (xhigh) | 45.5% (max) | 
| FrontierSWE v2 (agentic coding) | 69.9% | 56.3% | 62.3% | 
| OSWorld 2.1 (computer use) | 89.3% | 83.5% | 87.2% | 
| Humanity's Last Exam (with tools) | 74.2% | 65.6% | 67.7% | 
| AA-Briefcase (knowledge work, Elo) | 1785 | 1669 | 1800 | 

The most useful claim is about cost. On FrontierCode 2.0, Fable 5.2 at high effort scored 45.3%, about level with Opus 5.5's best of 45.5%, "for roughly half the cost per task." Its system card puts that at $6.72 per task against $12.82 for Opus. Higher effort isn't always better, either: Fable scored 49.1% at xhigh but 45.7% at max, where each task cost $44.70.

### Fable 5.2 vs Mythos 5.2

Anthropic's launch post is direct about it: "Claude Fable and Claude Mythos are the same underlying model. Fable has strong cybersecurity and biology safeguards to guard against misuse, which allows us to make it available to all users." Mythos 5.2 has more permissive safeguards in those areas and costs the same $8 / $40.

Access is the difference that matters. Mythos 5.2 is available only to organizations verified through Anthropic's Cyber Verification Program or Life Sciences Verification Program, and Claude Enterprise customers can use it through Claude Security. When Fable's safeguards block a request, Anthropic answers it with an older model instead: Opus 4.8 for cybersecurity and Opus 5 for biology. No independent tester has published Mythos 5.2 results yet.

### How Fable 5.2 Did on the Three Tasks

- **App building:** 93.77% at $22.13 and 41 minutes per app, first of 127 entries on Vals' board, run at high effort while the other three ran at max.
- **Pelican SVG:** not tested yet. For reference, Fable 5.1 at max effort used 65,927 output tokens over 13 minutes 54 seconds for $3.30, and Willison called the result "the best pelican I've seen from any of Anthropic's models."
- **Clinical notes:** not tested yet. Fable 5.1 scored 91.29% at $0.96 per note.

Fable 5.2 leads both Vals tests that have scored it, but its lead over Sonnet 5.5 is within the margin of error on each, and Sonnet costs less on both.

## Claude Haiku 5.5: Cheapest, and Much Better

Haiku 5.5 is the cheapest and fastest model in the lineup, and the biggest upgrade of the four. It replaced Haiku 4.5 on October 7, and on Vals' app-building test it went from 11.39% to 90.44%.

The price is $0.10 / $0.50 per million tokens for prompts up to 100,000 tokens, and $0.50 / $2.50 above that. Anthropic's launch post says that is "90% lower than Claude Haiku 4.5 for requests up to 100,000 tokens," and that on average "it now costs around 75% less to run." The context window grew to 1M tokens, output to 128K, and it is Anthropic's "first Haiku-class model to come with an adjustable effort setting," defaulting to medium.

There are two catches. A request over 100,000 tokens pays the higher rate for the whole prompt, "even when part of its prompt is a cache hit." And Willison found the new tokenizer uses "around 1.25x as many tokens" as Haiku 4.5 for the same prompt, "so there's a hidden price increase there."

Anthropic's models overview points it at "high-volume, latency-sensitive tasks such as classification, extraction, and routing." Artificial Analysis measured 218 tokens per second at its default medium effort, the fastest of the four.

### How Haiku 5.5 Did on the Three Tasks

- **App building:** 90.44% at $6.07 and 35 minutes per app, within 2 points of Sonnet 5.5 for under a third of the cost.
- **Pelican SVG:** 10,524 output tokens in 49 seconds at xhigh. Willison wrote, "I got a good bicycle frame for everything beyond low." At max it took 5 minutes 9 seconds and cost 3.3826 cents, and still returned a pelican, unlike Sonnet 5.5 and Opus 5.5 at max.
- **Clinical notes:** 89.54% at about 4 cents per note, under 2 points behind Sonnet 5.5 for about a twelfth of the cost.

Haiku 4.5 is now a legacy model. It is still available and Anthropic hasn't announced a retirement date, but at 10 times Haiku 5.5's price for short prompts, there is little reason to stay on it.

## Which Claude Model Should You Use? Pick This If

Anthropic's docs say most workloads should start with Opus 5.5. The test data above makes a case for trying Sonnet 5.5 first on well-scoped work, then moving up only where it misses.

### Pick Sonnet 5.5 If

- You write, review or fix code in well-defined chunks.
- You are building an app or website from a clear spec.
- You produce documents, slides or spreadsheets.
- Cost matters and you need near-Opus results.

### Pick Opus 5.5 If

- An agent will run for hours on a task without a clear spec.
- You are refactoring a large codebase or designing a complex system.
- The work needs judgment calls along the way, not only correct output.

### Pick Fable 5.2 If

- Opus 5.5 at xhigh or max effort still falls short on your evals.
- An agent will run for hours, or coordinate many subagents, on hard technical work.
- You need multistep research carried through to a finished report or deck.
- The quality of a single output matters more than its cost.

### Pick Haiku 5.5 If

- You classify, extract or route thousands of items and can check the results.
- You need a cheap sub-agent working under a bigger model.
- You are building something simple from a clear spec on a tight budget.
- Your prompts stay under 100,000 tokens, where it is cheapest.

Whichever model you pick, adjust effort before you switch models. Anthropic's model guide puts it plainly: "Tuning effort is often a better lever than switching models." CatDoes works the same way when it builds apps. You switch between Low, Medium, High and Max agent tiers as you work, and the docs suggest letting a higher tier plan a feature, then switching lower to build it.

## Frequently Asked Questions

### Is Claude Opus better than Sonnet?

On long, open-ended work, yes. Anthropic says Opus 5.5 "remains clearly stronger" there. On scored tests the gap is gone: the Vals Index puts Sonnet 5.5 at 70.85% and Opus 5.5 at 70.14%, within one standard error, and Sonnet scored higher at building web apps for a little over half the cost.

### Which Claude model is best for coding?

Opus 5.5 for multi-hour agentic coding and large refactors, which is why it is the Claude Code default. Sonnet 5.5 for everyday code generation and bug fixes, where it matches Opus for less. Fable 5.2 tops Vals' app-building test and Anthropic's FrontierCode 2.0, so move to it when Opus at xhigh or max effort still falls short.

### Is Claude Fable 5.2 better than Opus 5.5?

On most tests, yes. Anthropic's system card says Fable 5.2 scored highest on 14 of 16 results, trailing Opus 5.5 only on two knowledge-work ratings, and it ranks first on the Vals Index. But it costs twice as much per token, and Anthropic's cost guide found Opus 5.5 at xhigh effort level with Fable 5.2 at high for a little over half the cost.

### What is the difference between Claude Fable 5.2 and Mythos 5.2?

They are the same underlying model with different safeguards. Fable 5.2 has strong cybersecurity and biology safeguards and is open to all API customers at $8 / $40 per million tokens. Mythos 5.2 has more permissive safeguards in those areas and is offered only to organizations verified through Anthropic's Cyber or Life Sciences Verification Programs.

### Which Claude models can I use for free?

The Claude Free plan includes Sonnet and Haiku. Opus needs a paid plan such as Pro, which costs $20 a month billed monthly. Fable is not on Free, uses usage credits on Pro, and is included up to half the weekly limit on Max ($100 a month) and Ultra ($200 a month).

### Should I still use Fable 5.1 or Haiku 4.5?

Probably not. Both are legacy models that are still available. Fable 5.2 costs 20% less than Fable 5.1 and beat it on every result in Anthropic's launch table. Haiku 5.5 costs 90% less than Haiku 4.5 for prompts up to 100,000 tokens and scored 90.44% at building a web app, against 11.39% for Haiku 4.5.

## The Bottom Line

Price no longer tracks quality in a straight line across the Claude lineup. Sonnet 5.5 beat Opus 5.5 or finished within a point and a half of the leader on every scored task in this guide, for half Opus's price per token. Opus 5.5 is still Anthropic's default and the safer pick for long, unscripted agent work.

Fable 5.2 now costs less than the model it replaced and leads both Vals rankings it has entered, but at twice Opus's per-token price it is for the jobs Opus can't finish. Haiku 5.5 is the quiet winner: near-Sonnet results on app building and clinical notes for a fraction of the cost. Whatever you choose, test effort levels before paying for a bigger model.

If what you want is a finished app rather than a model to manage, that is what we built CatDoes for. Describe the app, and the agent builds it and ships it to the App Store, Google Play or the web, with the backend included.

Nafis Amiri

Co-Founder of CatDoes
