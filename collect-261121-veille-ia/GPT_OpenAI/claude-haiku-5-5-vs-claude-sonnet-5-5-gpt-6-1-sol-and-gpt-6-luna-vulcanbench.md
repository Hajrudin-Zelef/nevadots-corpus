---
id: collect-261121-veille-ia/gpt-openai/claude-haiku-5-5-vs-claude-sonnet-5-5-gpt-6-1-sol-and-gpt-6-luna-vulcanbench
title: "Claude Haiku 5.5 vs Claude Sonnet 5.5, GPT-6.1 Sol and GPT-6 Luna | VulcanBench"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "haiku", "claude", "level", "sonnet", "against", "luna", "runs"]
source: https://vulcanbench.com/benchmarks/swe-v4-haiku55-vs-frontier.html
source_anchor: ""
source_lines: [1, 97]
section: "GPT_OpenAI"
---
## What we found

### Highest score

Haiku 5.5 at Max: 92.16 ± 0.37 (n=21)

The highest point estimate of the four, level with Claude Sonnet 5.5 at Max (92.02 ± 0.35): a 0.14-point gap, far inside the error. Both are clearly ahead of GPT-6.1 Sol’s best (88.78 ± 0.40 at Extra-high) and GPT-6 Luna’s (81.42 at Max over its finished runs). Haiku’s Max score is over 21 judged runs: it leaves out paddockcore, which a safety classifier stopped with an empty patch, and the unpublished codeccore.

### Beside Claude Sonnet 5.5

Tied at Max; ahead below it on Code quality, not on tasks solved

Below Max, Haiku 5.5’s combined score is level with Sonnet 5.5’s at Medium (84.03 against 84.10) and above it at Low, High and Extra-high, but only the Extra-high gap (1.62 points, about 2.3 combined standard errors) is clearly outside the error. That edge comes from Code quality, not correctness: Haiku rates 74.83 to 80.13 from Low to Extra-high against Sonnet’s 61.67 to 75.78, yet passes fewer tasks from Low to High (13, 13 and 17 against 15, 17 and 20), the same at Extra-high (22) and one fewer at Max (22 against 23). Haiku costs 1.9 to 4.0 times less per task at every level, about 3.6 times less at Max ($1.54 against $5.52).

### Beside GPT-6.1 Sol

Level up to High, about 3 points ahead at Extra-high and Max

GPT-6.1 Sol is ahead of Haiku 5.5 on the point estimate at Low and Medium (86.22 and 86.44 against 84.42 and 84.03), within about one combined standard error since Haiku’s errors there are 1.97 and 2.50; the two are level at High (88.23 against 88.01). At Extra-high and Max Haiku is ahead by 3.05 and 3.81 points, and both levels are above Sol’s best. Sol solves more tasks at every level (20, 22, 23, 23 and 23 against 13, 13, 17, 22 and 22) and costs 1.9 to 3.8 times less per task.

### Beside GPT-6 Luna

Far ahead at every level, at a far higher cost

Haiku 5.5 is far ahead of GPT-6 Luna at every level, 84.42 against 40.83 at Low and 92.16 against 81.42 at Max, and solves 13 to 22 tasks against Luna’s 0 to 12. Luna is 5 to 142 times cheaper per task, and 6 of its runs hit the 3-hour bound: counting each timeout as 0, Luna scores 71.94 at Extra-high and 67.26 at Max.

GPT-6.1 Sol is the fastest of the four from High up and GPT-6 Luna at Low and Medium. Haiku 5.5 takes 19.0 to 27.7 minutes a task: slower than Sonnet 5.5 from Low to High, faster at Extra-high and Max. In short, at Max Haiku 5.5 matches Sonnet 5.5’s combined score for about 28% of the price per task, solving one task fewer; GPT-6.1 Sol solves more tasks at every level for less money but scores about 3 points lower at the top; GPT-6 Luna is the cheapest and scores lowest.

Tasks solved out of 23, Low to Max: Haiku 5.5 13, 13, 17, 22, 22; Sonnet 5.5 15, 17, 20, 22, 23; GPT-6.1 Sol 20, 22, 23, 23, 23; GPT-6 Luna 0, 0, 2, 11, 12. The whole sweep of 115 runs cost $138.31 for Haiku 5.5 as Claude Code reports it (floor $31.10, ceiling $155.50), $394.77 for Sonnet 5.5, $44.33 for GPT-6.1 Sol and $7.26 for GPT-6 Luna’s finished runs.

## Every level, side by side

One row per model at each effort level. Combined score is the mean over the judged runs, with ±1 sample standard error across tasks and the number of judged runs (n); every cell has 23 runs. Code quality is out of 100. Passed counts all 23 runs. Cost and minutes are means per task over every run, except GPT-6 Luna’s cost, which covers its finished runs (timeouts are unpriced); their standard errors are in each model’s bundle.

| VulcanBench Frontier v4: Claude Haiku 5.5, Claude Sonnet 5.5, GPT-6.1 Sol and GPT-6 Luna at every effort level |  |  |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|---|---|
| Model / harness | Effort | Combined | ±1 SE | Judged n | Code quality | Passed | $/task | Min/task | 
|---|---|---|---|---|---|---|---|---|
| Claude Haiku 5.5 / Claude Code | Low | 84.42 | ±1.97 | 23 | 74.83 | 13/23 | $1.04 | 22.9 | 
| Claude Sonnet 5.5 / Claude Code | Low | 81.63 | ±1.38 | 23 | 61.67 | 15/23 | $2.88 | 20.0 | 
| GPT-6.1 Sol / Codex | Low | 86.22 | ±0.82 | 23 | 70.79 | 20/23 | $0.31 | 6.9 | 
| GPT-6 Luna / Codex | Low | 40.83 | ±2.04 | 23 | 65.84 | 0/23 | $0.03 | 5.6 | 
| Claude Haiku 5.5 / Claude Code | Medium | 84.03 | ±2.50 | 22 | 76.07 | 13/23 | $1.36 | 24.0 | 
| Claude Sonnet 5.5 / Claude Code | Medium | 84.10 | ±1.20 | 23 | 65.35 | 17/23 | $3.00 | 20.8 | 
| GPT-6.1 Sol / Codex | Medium | 86.44 | ±0.78 | 23 | 70.25 | 22/23 | $0.40 | 10.6 | 
| GPT-6 Luna / Codex | Medium | 45.72 | ±1.32 | 23 | 69.33 | 0/23 | $0.01 | 3.6 | 
| Claude Haiku 5.5 / Claude Code | High | 88.01 | ±1.42 | 23 | 77.60 | 17/23 | $1.25 | 26.8 | 
| Claude Sonnet 5.5 / Claude Code | High | 86.79 | ±1.15 | 23 | 70.84 | 20/23 | $2.39 | 14.8 | 
| GPT-6.1 Sol / Codex | High | 88.23 | ±0.40 | 23 | 73.67 | 23/23 | $0.33 | 10.2 | 
| GPT-6 Luna / Codex | High | 57.35 | ±3.80 | 23 | 66.40 | 2/23 | $0.05 | 15.6 | 
| Claude Haiku 5.5 / Claude Code | Extra-high | 91.83 | ±0.39 | 22 | 80.13 | 22/23 | $0.84 | 19.0 | 
| Claude Sonnet 5.5 / Claude Code | Extra-high | 90.20 | ±0.57 | 23 | 75.78 | 22/23 | $3.38 | 21.6 | 
| GPT-6.1 Sol / Codex | Extra-high | 88.78 | ±0.40 | 23 | 74.99 | 23/23 | $0.43 | 15.4 | 
| GPT-6 Luna / Codex | Extra-high | 78.79 | ±3.02 | 21 | 71.49 | 11/23 | $0.16 | 52.6 | 
| Claude Haiku 5.5 / Claude Code | Max | 92.16 | ±0.37 | 21 | 81.30 | 22/23 | $1.54 | 27.7 | 
| Claude Sonnet 5.5 / Claude Code | Max | 92.02 | ±0.35 | 23 | 81.72 | 23/23 | $5.52 | 38.0 | 
| GPT-6.1 Sol / Codex | Max | 88.35 | ±0.33 | 23 | 73.80 | 23/23 | $0.46 | 14.5 | 
| GPT-6 Luna / Codex | Max | 81.42 | ±2.88 | 19 | 71.51 | 12/23 | $0.10 | 60.5 | 

| GPT-6 Luna’s timeouts: combined score over finished runs and with each timeout counted as 0, n=23 runs per level |  |  |  |  | 
|---|---|---|---|---|
| Effort | Judged n | Timeouts | Combined, finished runs | Combined, timeouts as 0 | 
|---|---|---|---|---|
| Low | 23 | 0 | 40.83 ± 2.04 | 40.83 ± 2.04 | 
| Medium | 23 | 0 | 45.72 ± 1.32 | 45.72 ± 1.32 | 
| High | 23 | 0 | 57.35 ± 3.80 | 57.35 ± 3.80 | 
| Extra-high | 21 | 2 | 78.79 ± 3.02 | 71.94 ± 5.47 | 
| Max | 19 | 4 | 81.42 ± 2.88 | 67.26 ± 6.99 | 

## The three cards

Each card sets Haiku 5.5 beside one model at every level, combined score on the left and cost per task on the right, with a table of tasks passed and cost. Haiku 5.5 never fell back to another model; Sonnet 5.5 never did either, and Codex has no refusal fallback.

## Methods and caveats

- **Same tasks, same rubric, same judges.** All four ran the same 23 Frontier v4 tasks, one attempt per task and level, inside a flat 3-hour bound. Code quality was judged under one rubric by Muse Spark 1.3 (Meta) and Grok 4.6 (xAI), from labs other than Anthropic and OpenAI.
- **Separate judging sessions.** Haiku 5.5 was judged under protocol v3.24, Sonnet 5.5 under v3.23, GPT-6.1 Sol under v3.18 and GPT-6 Luna under v3.16. Each applies the same rubric, controls, gates and weights to its own population, and each judge retook the calibration exam every time. Under v3.23 and v3.24 Grok 4.6 ran through Cursor CLI 2026.10.01, pinned by version for the v3.24 round.
- **Runs without a score.** Haiku 5.5 is judged on 22 runs at Medium and Extra-high and 21 at Max: on paddockcore at each a safety classifier stopped the session with an empty patch (counted as failed), and at Max codeccore has no published Code quality score because Grok 4.6’s intent probe gave no valid answer (counted as passed). Its combined score at those levels therefore leaves out the three failed paddockcore runs. GPT-6 Luna is judged on 21 and 19 runs at Extra-high and Max: 6 runs hit the 3-hour bound, so the Luna table above also shows the score with each timeout counted as 0. GPT-6.1 Sol at Medium includes one run scored from Muse Spark 1.3 alone.
- **Cost bases differ.** The two Claude columns use Claude Code’s own list-price total per run; Haiku 5.5 is priced by prompt length, so its report also gives a floor and a ceiling for its two tiers. GPT-6.1 Sol and GPT-6 Luna are API-equivalent at list prices from their token ledgers (Luna’s timeouts are unpriced), while their runs used a ChatGPT Pro subscription. None of these is a bill.
- **Harnesses differ.** Haiku 5.5 ran on Claude Code 2.1.293 to 2.1.296, Sonnet 5.5 on Claude Code 2.1.291 to 2.1.293, GPT-6.1 Sol on Codex 0.159.0 and GPT-6 Luna on Codex 0.155.0, so small gaps between columns are harness confounded. These runs measure a model-and-harness pair, not a bare model.
- **Refusal fallback.** Both Claude columns ran with Claude Code’s refusal fallback on and neither used it. Codex has no fallback.
- **Haiku 5.5 disclosures.** The Haiku sweep paused twice between tasks, which leaves run timings intact, and Claude Code updated itself during it; its judge settings are the recovered original v3.3 and v3.4 protocols, with Cursor reporting Grok 4.6’s display name as “Grok 4.6 Medium” for the same model id. Details are in the Haiku 5.5 report; Sonnet 5.5’s are in its report.
- **Standard errors** describe task sampling across the judged tasks, not judge uncertainty or repeated runs, and are not a significance test. No human rated anything; Code quality is model judgment for a human reader. Frontier Code quality is never compared with Routine v1 Code quality.

Every column also sits on the Frontier v4 leaderboard beside the other models judged under the same rubric.

## Get the data

Every number above can be recomputed from public files. Each bundle holds every run’s scores, both judges’ sub-scores, tokens and cost, the per-level aggregates, the calibration record and the exact judge protocol text.

- **Claude Haiku 5.5:** evidence bundle (runs.csv, scores by level), harness results folder, redacted agent traces.
- **Claude Sonnet 5.5:** evidence bundle (runs.csv, scores by level), harness results folder, redacted agent traces.
- **GPT-6.1 Sol:** evidence bundle (runs.csv, scores by level), harness results folder.
- **GPT-6 Luna:** evidence bundle (runs.csv, scores by level), harness results folder.
- **All Frontier v4 columns:** the leaderboard as CSV.

Withheld for every model: raw prompts and responses, submitted patches, and the quirk answer keys, which describe hidden-test behaviour.
