---
id: collect-261121-veille-ia/gemini-google/gemini-4-enters-post-training-as-google-eyes-early-launch
title: "Gemini 4 Enters Post-Training as Google Eyes Early Launch"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "google", "model", "post-training", "early", "launch", "announced", "said"]
source: https://diyai.io/news/gemini-4-post-training-early-launch/
source_anchor: ""
source_lines: [1, 73]
section: "Gemini_Google"
---
Google DeepMind has moved Gemini 4 into the early post-training phase, sending the clearest signal yet that Google’s next flagship AI model is moving toward release. Koray Kavukcuoglu, who leads Google DeepMind as senior vice president and chief AI architect, said at The Information’s AI Agenda Live Summit on 23 September that he hopes Gemini 4 will ship “much earlier” than the end of 2026.

This is a meaningful development milestone, not a launch announcement. DIY AI checked Google’s current Gemini API listings on 24 September and found no Gemini 4 model identifier, public pricing, model card or migration guidance. Developers comparing what is actually deployable today should therefore keep using released models and treat our AI model comparison as the current baseline until Google publishes a Gemini 4 endpoint.

| Gemini 4 detail | Status on 24 September 2026 | 
|---|---|
| Model exists | Confirmed | 
| Development stage | Early post-training | 
| Release timing | Google DeepMind hopes for “much earlier” than the end of 2026, but no date is announced | 
| Gemini API access | Not announced | 
| Google AI Studio access | Not announced | 
| Pricing | Not announced | 
| Context window and limits | Not announced | 
| Public benchmark results | Not announced | 

## The new information is the move from pre-training to post-training

Google had already confirmed it was training Gemini 4. In July, the company described it as its most ambitious pre-training run to date, and Kavukcuoglu discussed that run again in a Google interview published in early September. The 23 September comments move the public timeline forward: the base-model training phase has progressed far enough for Google to be working on post-training.

Post-training is where a capable base model becomes a model people can use reliably. The work can include reinforcement learning, preference optimisation, instruction following, tool behaviour, refusal behaviour, safety testing and targeted improvements to weak tasks. Kavukcuoglu also said Google is developing guardrails and conducting safety testing around Gemini 4.

The important limitation is the word “early”. Post-training can produce rapid improvements, but it can also expose regressions that force another round of tuning and evaluation. Reading “post-training” as “launching in days” would go beyond what Google DeepMind has said.

## Google appears willing to launch an early Gemini 4 build and keep iterating

Kavukcuoglu’s description of the release strategy is arguably more useful than the loose timing. He said the intention is to get an early post-training output in front of users as soon as possible because the team is encouraged by the results, then continue iterating quickly.

That suggests Gemini 4 may arrive more like a moving model programme than a single finished artefact. Google already uses preview, stable and experimental model identifiers across the Gemini API. If Gemini 4 follows a similar path, developers will need to record the exact model ID and evaluation date rather than assuming every “Gemini 4” test refers to the same behaviour.

This matters most for production agent workflows. A model can improve overall while changing tool selection, response length, retry behaviour or safety refusals enough to break an existing workflow. Teams evaluating an early Gemini 4 release should version their prompts and acceptance tests alongside the model rather than treating a successful preview test as permanent.

## Antigravity gives the strongest clue about what Google is testing first

Kavukcuoglu said Gemini 4 is already being used internally by engineers running Google’s Antigravity AI coding tool. That doesn’t confirm Gemini 4 will launch first as a coding model, but it gives us a more useful signal than speculative benchmark screenshots.

Agentic software engineering is a demanding internal test because the model has to do more than answer coding questions. It must maintain context, choose tools, edit files, recover from failed actions and keep working across longer task sequences. Those are exactly the areas where small behavioural changes during post-training can materially alter whether an agent completes a task or gets stuck in a loop.

If Google exposes Gemini 4 through Antigravity or the Gemini API early, the first useful comparison will therefore be task completion rather than a general intelligence leaderboard. For teams weighing current alternatives, our Claude Code pricing guide also shows why coding-agent costs need to include subscription limits, API usage and repeated attempts rather than just a headline token rate.

## No Gemini 4 API, pricing or migration decision exists yet

Google’s public Gemini API model list still centres on the Gemini 3 family. Gemini 3.8 Flash is the current stable Flash model, while Gemini 3.1 Pro remains a preview option. DIY AI found no public Gemini 4 endpoint in the model list or API release notes on 24 September.

That creates a simple rule for developers: do not redesign a production stack around Gemini 4 yet. There is nothing to migrate to, no published price to budget against and no documented compatibility contract. Even if an early endpoint appears soon, teams should expect preview-style uncertainty until Google publishes stable identifiers and deprecation guidance.

Pricing will be especially important. Recent model releases from Google, OpenAI and Anthropic have made cost per completed task a more useful measure than cost per million tokens alone. A more capable model can be cheaper in practice if it needs fewer retries and tool calls, while an inexpensive model can become costly if an agent repeatedly fails before reaching an acceptable result.

## The release timing deserves more caution than the headline suggests

The temptation is to turn “much earlier than the end of the year” into an unofficial launch month. There is not enough evidence for that. Kavukcuoglu expressed a goal, not a committed date, and Google has changed model release plans before as testing progressed.

That caution is also showing up repeatedly in early user discussion. The recurring concern is not whether Gemini 4 exists, which Google has already confirmed, but whether rumours about a specific week or benchmark score deserve trust before a model ID appears. Another common point is that strong benchmark numbers will not settle the question if long-running coding and agent workflows still feel less reliable in practice.

Those are sensible filters. A useful launch signal isn’t another screenshot with an anonymous score. It is a Google model page, a named endpoint, documented access, pricing and reproducible behaviour on real tasks.

## What DIY AI will test when Gemini 4 becomes available

A fair Gemini 4 evaluation needs to separate capability from launch hype. We would start with the released endpoint and run controlled tasks against the current production alternatives using the same prompts, tools and acceptance criteria.

- **Long-running coding:** multi-file changes, test execution, recovery from failed edits and whether the model completes the task without unnecessary loops.
- **Tool use:** whether it selects the right tool, preserves parameters and recovers when a tool returns incomplete or conflicting information.
- **Research and long context:** whether claims stay attached to the right source and whether important evidence is lost as context grows.
- **Cost per accepted task:** token spend, retries, tool calls and human correction rather than token price in isolation.
- **Version stability:** whether an early Gemini 4 build changes materially across preview iterations and whether developers can pin a model version.
- **Safety side effects:** whether new guardrails block legitimate technical work or alter normal workflows as Google tightens the model before broader release.

The acceptance test should stay fixed while the model changes. Otherwise, it is too easy to move the goalposts after seeing a new benchmark or an impressive demo.

## Gemini 4 is now closer, but it is not publicly available

The meaningful update is that Gemini 4 has progressed beyond the pre-training stage Google discussed earlier this year and is now in early post-training, with internal testing and safety work under way. Google DeepMind also wants to release an early version well before the end of 2026 if development continues to go well.

What has not changed is just as important. There is no public release date, API identifier, price, context limit, model card or independent performance evidence yet. Until those appear, Gemini 4 is a confirmed near-term model project, not a deployable product.
