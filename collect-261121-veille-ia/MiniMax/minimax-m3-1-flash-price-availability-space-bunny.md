---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-price-availability-space-bunny
title: "MiniMax M3.1 Flash: price, availability, Space Bunny"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-23"]
keywords: ["minimax", "flash", "tokens", "space", "bunny", "model", "october", "version"]
source: https://dotsbot.co/news/minimax-m3-1-flash-launch
source_anchor: ""
source_lines: [1, 130]
section: "MiniMax"
---
## The short answer

- MiniMax launched M3.1 Flash on October 19, 2026, through the MiniMax API, M Plan and MiniMax Code.
- MiniMax lists M3.1 Flash at $0.15 per 1M input tokens, $0.60 per 1M output tokens and $0.015 per 1M cached input tokens.
- MiniMax said Space Bunny, an anonymous model that processed more than 200 trillion tokens, was an early version of M3.1 Flash.
- MiniMax claims the model's overall usage cost for agentic tasks is 70% lower than its predecessor and that online inference is nearly three times faster.
- MiniMax published the M3.1 Flash weights on Hugging Face on October 23, 2026 (UTC) under its MiniMax Community License, an open-weights license that limits the free grant to non-commercial use; a Pro version is still to come.
- Artificial Analysis gave M3.1 Flash a score of 42 on its Intelligence Index at about $0.22 per task, and measured a 93% hallucination rate on AA-Omniscience under that benchmark's definition.

MiniMax launched M3.1 Flash, an agent and coding model, on October 19, 2026, through the MiniMax API, M Plan and MiniMax Code. It costs $0.15 per 1M input tokens and $0.60 per 1M output tokens. MiniMax also confirmed that Space Bunny, an anonymous model that had been running at scale, was an early version of M3.1 Flash, according to MiniMax's post on X and its launch blog.

**Update, October 23, 2026:** MiniMax has made the M3.1 Flash weights public on Hugging Face. "MiniMax M3.1 Flash weights are now public," the company posted on X at 01:06 UTC on October 23, 2026. MiniMax's model card describes "a native multimodal mixture-of-experts model for coding, agentic workflows, computer use, and cybersecurity," with "Approximately 428B total; 23B activated per token," a context window of up to 1 million tokens, and "Text, image, and video inputs; text output." The parameter figures are MiniMax's. The weights come under the MiniMax Community License, so this is an open-weights release. At launch, VentureBeat had reported that MiniMax planned to release the weights on October 23; no independent outlet's report of the release itself was among the sources we reviewed.

## #What changed?

MiniMax released a new Flash model aimed at agentic tasks, coding, computer use and cybersecurity. "Today, we officially roll out MiniMax M3.1 Flash," the company wrote on its blog. Its X post went up at 11:56 UTC on October 19, 2026 (17:26 IST).

MiniMax's headline claim is that "M3.1 Flash delivers a generational leap in agentic tasks, coding, Computer Use and cybersecurity, while making online inference nearly three times faster and reducing usage costs by 70%." Reuters, in a report carried by The Standard, wrote that "Chinese AI startup MiniMax (0100) launched an updated version of AI model M3.1 Flash, it said in a statement on Monday."

### #What is Space Bunny?

MiniMax says Space Bunny was M3.1 Flash before launch. The blog says Space Bunny "processed more than 200 trillion tokens while operating anonymously, was an early version of MiniMax M3.1 Flash." On X, MiniMax said Space Bunny "has topped global leaderboards with 200T+ tokens."

### #What do MiniMax's own benchmarks show?

All of these figures are MiniMax's own claims from its launch blog. Independent results from Artificial Analysis are in the next section.

| Benchmark (MiniMax's own claims) | M3.1 Flash score, per MiniMax | 
|---|---|
| DeepSWE v1.1 | 73.7% | 
| OSWorld V2 | 61.7 | 
| Agents' Last Exam (ALE) | 29.6 | 
| BankerToolBench | 76.2 | 
| CyberGym | 82% (launch blog) | 

MiniMax says the model "ranks first among Chinese models" on both OSWorld V2 and Agents' Last Exam. It also claims output speeds "can reach 200+ TPS (tokens per second), with median (P50) speeds consistently reaching 170+ TPS," which it attributes to "more aggressive sparsity."

## #What do independent tests show?

Artificial Analysis published its own test results for M3.1 Flash in posts on X, which VentureBeat described as independent testing.

Artificial Analysis said M3.1 Flash "scores 42 on the Artificial Analysis Intelligence Index, 13 points higher than MiniMax-M3 and less than half the cost per task." It put the cost of running its Intelligence Index at about $0.22 per task. VentureBeat reported that this takes the average cost of an Intelligence Index task "from approximately $0.51 down to just $0.22" compared with MiniMax M3. Artificial Analysis said DeepSeek V4.1 Flash 1017 "scores 3 points higher."

On its AA-Omniscience benchmark, Artificial Analysis said M3.1 Flash "answers 56% of questions correctly" but "attempts 98% of questions." VentureBeat reported that Artificial Analysis found "a 93% hallucination rate under the benchmark's definition." VentureBeat added: "Importantly, that does not mean 93% of all its answers were incorrect. The metric measures the proportion of non-correct responses that were outright wrong rather than partial answers or refusals."

## #How much does it cost?

MiniMax lists flat per-token pricing for M3.1 Flash on its blog and its pricing page:

| Usage | Price for MiniMax-M3.1-Flash (USD per 1M tokens) | 
|---|---|
| Input | $0.15 | 
| Output | $0.60 | 
| Cached input (prompt caching read) | $0.015 | 

MiniMax claims that "For agentic tasks, the model’s overall usage cost is 70% lower than its predecessor."

## #Who can use it and when?

M3.1 Flash is available on three MiniMax channels, according to the blog, and as open weights on Hugging Face. A Pro version is still to come.

| Channel | Status, per MiniMax | 
|---|---|
| MiniMax API | Available since October 19, 2026 | 
| M Plan subscription | Available since October 19, 2026 | 
| MiniMax Code | Available since October 19, 2026 | 
| Model weights | Public on Hugging Face since October 23, 2026 (UTC) | 
| Pro version | "will also arrive later" | 

The blog says: "M3.1 Flash is now available through the MiniMax API, M Plan, and MiniMax Code." MiniMax has given no date for the Pro version.

### #What does the license allow?

The LICENSE file is titled "MINIMAX COMMUNITY LICENSE." Its main terms, quoted from that file:

- **Non-commercial use is free.** Permission is "granted, free of charge," to use the software "for non-commercial purposes."
- **Commercial use needs attribution.** Commercial users "shall prominently display “Built with MiniMax M3.1-Flash” on a related website, user interface, blogpost, about page or product documentation."
- **Large products need permission.** Products and services that "generate more than 20 million US dollars (or equivalent in other currencies) in yearly revenue" need "a separate, prior written authorization from MiniMax." Below that level, the license asks for a one-time notice to MiniMax.

The license counts fine-tuned or otherwise modified versions used commercially as commercial use, and it includes a list of prohibited uses.

## #How to get it

MiniMax lists these ways in:

1. **API:** use the MiniMax API at platform.minimax.io with the model listed on the pricing page as MiniMax-M3.1-Flash.
2. **M Plan:** subscribe through MiniMax's M Plan page.
3. **MiniMax Code:** download MiniMax Code for desktop from agent.minimax.io/download.
4. **Weights:** download them from the Hugging Face repository. The model card says "We recommend the following inference frameworks to serve the model:" and lists SGLang, vLLM and TokenSpeed. The safetensors files total about 252 GB, per the Hugging Face file listing.

## #How does it compare?

M3.1 Flash arrives two days after DeepSeek's V4.1 Flash 1017 update with Search and Fetch APIs on October 17, 2026, which kept pricing unchanged. Both companies report only their own benchmarks, so their figures are not directly comparable. For another recent model launch built for agents, see our coverage of Meta's Muse Wonder and Muse Wonder Full. More agent model coverage sits in the AI Agents hub.

## #What is still unclear?

- **Independent results.** The benchmark table and speed figures are MiniMax's own claims. The independent results in this article come from Artificial Analysis and use different benchmarks.
- **Pro.** No date has been given.
- **Hardware for self-hosting.** The model card does not state GPU count or memory needs.
- **The 70% figure.** MiniMax compares against "its predecessor" without naming the model or the method in the sources.

## Frequently asked questions

## How much does MiniMax M3.1 Flash cost?

MiniMax's pricing page lists MiniMax-M3.1-Flash at $0.15 per million input tokens, $0.60 per million output tokens and $0.015 per million tokens for prompt caching reads.

## Was Space Bunny a MiniMax model?

Yes, according to MiniMax. The company said Space Bunny, which processed more than 200 trillion tokens while operating anonymously, was an early version of MiniMax M3.1 Flash.

## Are MiniMax M3.1 Flash weights available?

Yes. MiniMax made the weights public on Hugging Face on October 23, 2026 (UTC) under the MiniMax Community License. The license grants free use for non-commercial purposes and sets conditions for commercial use.

## Are the M3.1 Flash benchmark results independent?

MiniMax's own benchmark figures are not independent. Separately, Artificial Analysis tested the model and gave it a score of 42 on its Intelligence Index, at about $0.22 per task.

## What does the 93% hallucination rate for M3.1 Flash mean?

Artificial Analysis measured a 93% hallucination rate on its AA-Omniscience benchmark. VentureBeat reported that this does not mean 93% of all answers were wrong; the metric measures the proportion of non-correct responses that were outright wrong rather than partial answers or refusals.

## Sources

Every factual claim in this article traces back to one of these.

## One email a week on AI agents and bots.

Launches, pricing and rollout changes and the guides worth reading. At most once a week, and you can leave with one click.
