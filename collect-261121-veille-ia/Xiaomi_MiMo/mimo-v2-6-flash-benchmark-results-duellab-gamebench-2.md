---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-flash-benchmark-results-duellab-gamebench-2
title: "MiMo-V2.6-Flash benchmark results — DuelLab GameBench 2"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-09-22"]
keywords: ["working", "attempt", "balanced", "reasoning", "baseline", "game", "programs", "settings"]
source: https://benchmarks.duellab.org/model-xiaomi-mimo-v2-6-flash
source_anchor: ""
source_lines: [1, 145]
section: "Xiaomi_MiMo"
---
- Score · relative 0–100
- 49.4
- Rank
- —
- Status
- Provisional
- Players generated
- 2026-09-22
- Match evidence updated
- 2026-11-19

## Reasoning settings compared

1. IntensiveNot supported by this model.
2. Reasoning enabledBalanced**58.7** · rank 38 on the Balanced leaderboard
  - Cost per program
  - $0.0178 · provider-reported
 
  - Working programs
  - 8 of 8
  - API time
  - 15.4 min (observed mean, descriptive)
 18.7 points above Reasoning disabled, at 6.1× the cost 
  - **Higher score for $0.0178 or less:** Particle-1 High · Balanced: 62.8 for $0.0130 (provider-reported)
  - **Cheapest scoring at least as high:** GLM-5.3 Flash High · Balanced: 60.5 for $0.00579 (provider-reported) (one of 3 such settings)
3. Reasoning disabledBaseline**40.0** · rank 57 on the Baseline leaderboard
  - Cost per program
  - $0.00293 · provider-reported
 
  - Working programs
  - 7 of 8
  - API time
  - 58 s (observed mean, descriptive)
 
  - **Higher score for $0.00293 or less:** Muse Ember Minimal · Baseline: 46.9 for $0.00281 (provider-reported)
  - It is also the only offered setting scoring at least as high for less.

## Show the numbers

The table needs JavaScript.

## Available settings and grouping

### Available settings

Checked on 2026-09-22 · Provider documentation

### How settings are grouped

- **Baseline** : Reasoning disabled — tested and included
- **Balanced** : Reasoning enabled — tested and included
- **Intensive** : Not available for this model

## Scores by game

## What this result shows

Strongest included setting: Reasoning enabled (58.7).

Across included settings, the highest game score is Game of the Amazons (59.7); the lowest is Xiangqi (29.0). Each game counts every generation attempt, a failed one as 0. These are relative scores within each game, not direct comparisons of difficulty.

Match evidence measures these retained programs. It does not establish how a fresh generation will perform. Independent-generation variation is not estimated in this release.

## Test protocol and evidence limits

- Model identifier
- xiaomi/mimo-v2.6-flash
- Provider route (catalog)
- openrouter
- Prompt identity / profile
- Not retained in this result set / Not retained in this result set
- Prompt version
- Not retained in this result set
- Generation policy
- Generation-quality admission policy (see methodology)
- Requested output-token limits
- 131072 (23 of 23 indexed attempts)

### Generation evidence

- Reasoning disabled: 7 contributing player programs across 7 games
- Reasoning enabled: 8 contributing player programs across 8 games

Counts describe contributing programs, not independent replications: repairs and resends are not new independent samples. Independent-generation lineage is not retained.

| Game | Reasoning group | Programs | Working-program strength | Raw rating | Combined record (W / D / L) | 
|---|---|---|---|---|---|
| Go | Balanced | 1 working of 1 attempt | 50.5 | 1452.2 | 293 / 25 / 330 | 
| Go | Baseline | 1 working of 1 attempt | 66.4 | 1610.8 | 313 / 192 / 145 | 
| Chess | Balanced | 1 working of 1 attempt | 64.4 | 1554.9 | 233 / 260 / 119 | 
| Chess | Baseline | 1 working of 1 attempt | 41.0 | 1366.5 | 18 / 416 / 178 | 
| Shogi | Balanced | 1 working of 1 attempt | 60.4 | 1471.2 | 315 / 46 / 273 | 
| Shogi | Baseline | 1 working of 1 attempt | 39.5 | 1280.6 | 126 / 176 / 334 | 
| Xiangqi | Balanced | 1 working of 1 attempt | 58.0 | 1509.2 | 301 / 72 / 255 | 
| Hex | Balanced | 1 working of 1 attempt | 56.2 | 1426.2 | 341 / 0 / 323 | 
| Hex | Baseline | 1 working of 1 attempt | 49.5 | 1338.3 | 300 / 0 / 368 | 
| International Draughts | Balanced | 1 working of 1 attempt | 62.5 | 1605.5 | 352 / 24 / 256 | 
| International Draughts | Baseline | 1 working of 1 attempt | 47.8 | 1360.9 | 263 / 11 / 360 | 
| Game of the Amazons | Balanced | 1 working of 1 attempt | 80.6 | 1844.7 | 501 / 0 / 161 | 
| Game of the Amazons | Baseline | 1 working of 1 attempt | 38.8 | 1217.4 | 218 / 0 / 444 | 
| Tumbleweed | Balanced | 1 working of 1 attempt | 37.3 | 1139.9 | 223 / 0 / 441 | 
| Tumbleweed | Baseline | 1 working of 1 attempt | 37.0 | 1135.1 | 224 / 0 / 442 | 

Each row covers all the model's programs in one game and setting. Strength and raw rating are attempt-weighted means; under them, each program's score and the lowest and highest rating describe the programs (not a confidence interval). The combined record adds up their matches: evidence volume, not strength.

### Failures and budgets

1 model-code generation failures; 0 provider/infrastructure failures excluded from the generation denominator. Runtime player faults are separate: 0 recorded one-sided faults across contributing program rows. A crash, an illegal move, or a timeout repeated by a later quiet replay, pinned on one program, also counts as that program's loss in its match record; other faults are left out of rating evidence.

Repair/resend outcomes and available costs are included. Exact retry ceilings and execution budgets are not retained in this result set; see the methodology for the public policy. Do not infer a measured setting from a current provider catalog.

## Player program results

Reasoning enabled: 63% / 37% / 0% · n 8Reasoning disabled: 50% / 38% / 12% · n 8 No Overall rankIntensive not supported
## Generation cost, all settings combined

$0.01

- Median
- $0.0072
- Range
- $0.0019–$0.02
- Provider-reported total
- $0.17
- Cost data
- 16 combinations · 16 runs
- Price source
- Provider-reported
- Price list
- boardgame-list-prices-2026-09-08
- Average output
- 31k tokens
- Output range
- 3.1k–79k tokens
- Runs with output-token data
- 16

## How cost is measured

Comparison cost gives equal weight to each model, game, and reasoning-setting combination. Recorded generation attempts include repairs and resends when cost evidence is available; runtime compute and service operation are excluded. Cost and output-token counts use separate telemetry denominators. A list-price estimate is not actual expenditure; mixed evidence is not a uniform standard-price comparison.

## Test protocol

- Combines this model's tested reasoning settings
- Player-program generation; retained protocol details are under “Test protocol and evidence limits” above
