---
id: collect-261121-veille-ia/modeles-open/beam-1-0-501b-a23b-a-specs-hardware-fit-pricing-llmap
title: "Beam 1.0 501B A23B â specs, hardware fit & pricing | LLMap"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["arena", "agent", "model", "tool", "changes", "beam", "a23b", "text"]
source: https://www.llmap.ai/models/reflection-beam-1-0-501b-a23b
source_anchor: ""
source_lines: [1, 160]
section: "Modeles_Open"
---
## Our take

Written Nov 19, 2026
Beam 1.0 501B A23B is a downloadable text model with a licence that allows commercial use, changes and redistribution, and it is priced to be the budget option rather than the quality one. Its own placings put it mid-table on chat and near the bottom on agent work.

Use it for everyday chat and drafting where the bill matters more than peak quality, or when you want to run a large model yourself under terms that allow commercial use, changes and redistribution. It handles long documents in a single request, though reliable recall across all of it is unverified in our data. Skip it if you need an agent that reliably calls the right tool, or if you have no machine that can hold a model this size.

The case for it

- The cheapest listed input rate sits well under what the pricier hosts charge, so the host you pick changes the bill more than it changes the model.
- Apache License 2.0 allows commercial use, changes and redistribution.
- Long documents go into one request without being split up first, though room to hold them is not a guarantee of accurate recall.

The case against it

- 74th of 189 on Arena Text (overall) as of 18 Nov 2026, which records which answer people preferred rather than whether it was correct.
- 76th of 79 on Arena Agent Â· Tool use as of 18 Nov 2026, so it is near the bottom of the field at calling the right tool.
- 501.3 billion parameters in total, of which about 23 billion are used per token, so running it yourself is a serious hardware commitment.

## How good is it?

An open text model for chat and everyday questions, though it struggles with multi-step tasks, tool calls and following new instructions.

- carrying out multi-step tasks for youArena Agent Â· 62nd of 79
- calling tools to carry out requestsArena Agent Â· Tool use Â· 76th of 79
- changing course when you give new instructionsArena Agent Â· Steerability Â· 67th of 79

EverydayGeneral questions and everyday reasoning

Arena Text (overall)79th of 193 Â· 1434

**73rd of 193**Arena Maths

**43rd of 187**

CodingWriting and fixing code on its own

Arena Coding54th of 193 Â· 1505

**71st of 119**

AgenticPlanning, calling tools, staying on task

Arena Agent62nd of 79 Â· â0.083

Arena Agent is the only board that has scored it for this.

WritingDrafting and rewriting prose

Arena Creative Writing97th of 193 Â· 1387

Arena Creative Writing is the only board that has scored it for this.

Placings on Arena's agent boards, from live sessions people ran themselves. A model can lead on one of these and sit mid-field on the others.

**72nd of 193**Arena Agent Â· Task outcome

**63rd of 79**Arena Agent Â· Steerability

**67th of 79**Arena Agent Â· Tool use

**76th of 79**

Boards this model appears on that none of the ratings above are built on.

## Every published score for this model11 scoresEvery figure we hold, from 11 boards, with who ran it and a link to the source â including the boards no rating above is built on.

## Can you run it yourself?

Too large for this card. The weights do not fit even with part of them offloaded to system memory.

Comfortable fit

Room to spare. 56.8 GB spare means a 10% error in the size would not change the answer.

## This model on every device we track71 devicesThe Q4 build most people download, on each device: what the weights come to, how much context the memory leaves, and whether it runs. Smallest device that runs it first.

Check against your own machine â Â· Where to rent it hosted â

## Or rent it from someone else

Prices checked 1 hour ago

Scroll sideways to see every columnÂ â

| Current provider offers with price, context and prompt-privacy answers |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
| Provider | In / out per 1M tokens | Context | Throughput | Trains on prompts | Logs prompts | Zero retention | 
|---|---|---|---|---|---|---|
| OpenRouterOpenRouter's own listing | $0.25 / $1.00 | 1M | not measured | Unknown | Unknown | Unknown | 
| CoreWeaveThrough OpenRouter | $0.30 / $1.00 | 1M944K max reply | 494 tok/s | No | No | Confirmed | 
| Nebius AI StudioThrough OpenRouter | $0.25 / $1.00 | 1M131K max reply | 188 tok/s | No | No | Confirmed | 
| Together AIThrough OpenRouter | $0.40 / $1.20 | 1M131K max reply | 243 tok/s | No | No | Confirmed | 
| DigitalOcean GradientThrough OpenRouter | $0.40 / $1.20 | 1M944K max reply | 217 tok/s | No | No | Confirmed | 
| BasetenThrough OpenRouter | $0.40 / $1.20 | 1M131K max reply | 231 tok/s | No | No | Confirmed | 

Across the 6 listings we hold: 5 say they do not train on prompts, 0 say they do and 1 does not say. 5 appear in the zero-retention registry we check; the rest are unknown to us.

What each host's API supports

From the parameter list each endpoint publishes. Streaming is omitted: nothing we hold reports it, for any model.

| API features per host |  |  |  | 
|---|---|---|---|
| Provider | Tool calling | JSON output | Strict schema | 
|---|---|---|---|
| OpenRouterOpenRouter's own listing | â | â | â | 
| CoreWeavenvfp4Through OpenRouter | â | â | â | 
| Nebius AI Studiofp4Through OpenRouter | â | â | â | 
| Together AIThrough OpenRouter | â | â | â | 
| DigitalOcean GradientThrough OpenRouter | â | â | â | 
| Basetenfp4Through OpenRouter | â | â | â | 

Tool calling: 5 of 6 listings say yes, 1 says no. JSON output: 6 of 6 listings say yes. Strict schema: 6 of 6 listings say yes.

## When we formed this view

Recent changes

## Placings

Arena Coding: 54th of 193 Â· 1505 Â· Arena Creative Writing: 97th of 193 Â· 1387 Â· Arena Hard Prompts: 73rd of 193 Â· 1461 Â· Arena Instruction Following: 72nd of 193 Â· 1433 Â· Arena Maths: 43rd of 187 Â· 1473 Â· Arena Text (overall): 79th of 193 Â· 1434 Â· Arena Code (WebDev): 71st of 119 Â· 1437
## Placings

Arena Agent: 62nd of 79 Â· Arena Agent Â· Steerability: 67th of 79 Â· Arena Agent Â· Task outcome: 63rd of 79 Â· Arena Agent Â· Tool use: 76th of 79
## What moved

input â69% ($1.30 â $0.40 per 1M tokens), output â72% ($4.30 â $1.20 per 1M tokens), cache read â69% ($0.130 â $0.040 per 1M tokens)
## What moved

input â70% ($1.00 â $0.30 per 1M tokens), output â67% ($3.00 â $1.00 per 1M tokens), cache read â60% ($0.100 â $0.040 per 1M tokens)
## What moved

New model detected on OpenRouter: reflection/beam-1.0-501b-a23b
Each date is the day we first saw the change, or the day the maker announced it.

What we do not know about this model yet

- We hold no measured file for it, so all 3 sizes on this page are calculated from the parameter count.
- Nothing we hold says whether an endpoint streams, so we do not show it either way.
- 1 of 6 listings does not say whether it trains on prompts.
- We hold no batch or off-peak rate for any of its listings.
- We hold a decode speed for it, but no prompt-processing (prefill) figure, so how long the input side of a job takes is unknown to us.

## Licence and identifiers

## What the licence allowsApache License 2.0, what it allows commercially, and the identifiers you need to pull this model â its Hugging Face repo, our slug and a machine-readable card.

Licence

Fully permissive: commercial use, redistribution, and derivatives allowed. Requires attribution and a copy of the license. Includes an express patent grant.

Identifiers

- Architecture
- Mixture of experts
- Takes in, gives back
- Text in, text out
- Catalogue slug
- reflection-beam-1-0-501b-a23b
