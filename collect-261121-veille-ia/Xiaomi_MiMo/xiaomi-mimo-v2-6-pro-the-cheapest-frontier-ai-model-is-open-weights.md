---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomi-mimo-v2-6-pro-the-cheapest-frontier-ai-model-is-open-weights
title: "Xiaomi MiMo V2.6 Pro: The Cheapest Frontier AI Model Is Open Weights"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-09-21"]
keywords: ["mimo", "index", "model", "xiaomi", "artificial", "xiaomis", "family", "cheapest"]
source: https://www.intelligentliving.co/xiaomi-mimo-v2-6-pro-cheapest-frontier/
source_anchor: ""
source_lines: [1, 150]
section: "Xiaomi_MiMo"
---
The open-weights crown changed hands today. On September 21, 2026, Xiaomi released the MiMo V2.6 family, and the flagship MiMo V2.6 Pro now sits at the top of Artificial Analysis’s open-model rankings with a score of 46 on the Artificial Analysis Intelligence Index v4.3.2. That matches Grok 4.7, one of the strongest closed American models, while costing a small fraction as much to run.

The score is impressive. The price tag is the real headline. Xiaomi kept API pricing unchanged from the previous generation, which makes MiMo V2.6 Pro the cheapest frontier-class AI model available and the top-rated Chinese model on the index. Here is what the independent benchmarks show, how it compares with DeepSeek, Kimi, GLM, and Qwen, and what the release means for the cost of capable AI.

The MiMo V2.6 lineup has three entries, all trained with large-scale reinforcement learning and all accepting text, images, audio, and video input with a 1-million-token context window. Weights are published under the MIT license on Hugging Face and ModelScope.

- **MiMo V2.6 Pro** : the flagship, a sparse mixture-of-experts model with roughly 1 trillion total parameters and 42 billion activated per token. This is the model setting records on the leaderboards.
- **MiMo V2.6 Flash** : the efficiency-focused sibling at 309 billion total parameters and 15 billion active, priced at $0.14 per million input tokens and $0.28 per million output tokens.
- **MiMo V2.6 Pro UltraSpeed** : a hosted fast-inference edition of Pro that multiplies output speed roughly 10 to 20 times at comparable quality, priced at $4.35 input and $8.70 output per million tokens.

Xiaomi also published the technical report, the reinforcement-learning training code, and the training environments themselves, a level of disclosure that remains rare among frontier labs.

One thing missing from that lineup is already visible in Xiaomi’s API catalog: mimo-x-flash-preview and mimo-x-pro-preview, preview entries of an unreleased family that many observers read as an early look at a future MiMo V3 generation. This is not a teaser timed to the V2.6 launch. Invitations to apply for preview access went out on September 9, applications were accepted shortly after, and select testers began working with the X family before the V2.6 reinforcement-learning training even started, according to the real-time training dashboard Xiaomi published during the run. MiMo Desktop applicants were also invited into the testing program. The capabilities of the X family remain under wraps, but the preview has now been running long enough to suggest Xiaomi’s next generation was already well underway while V2.6 was being trained.

Xiaomi’s official MiMo V2.6 benchmark page lays out the full comparison set, including head-to-head tables against Claude Opus 5, GPT-5.6 Sol, and Claude Fable 5 on coding, agentic, cybersecurity, and visual tasks.

Short answer: it is the strongest openly available model on Artificial Analysis’s independent index right now. MiMo V2.6 Pro scores 46 on the Artificial Analysis Intelligence Index v4.3.2, a 20-point jump over MiMo V2.5 Pro, which sat at 26 on Artificial Analysis’s independent testing. Only five closed models score higher, led by Claude Fable 5.1 and GPT-6 Astra at 53.

The index blends ten evaluations, and Xiaomi’s model shows a broad spread of professional strengths. The table below lines up Xiaomi’s capability indexes and individual evaluation results against three of its closest rivals. Cells are left empty where we do not yet have published results to compare.

| Index or evaluation | MiMo V2.6 Pro | Grok 4.7 | Kimi K3 | Step 5 Preview | 
|---|---|---|---|---|
| **Overall** |  |  |  |  | 
| AA Intelligence Index v4.3.2 | 46 | 46 | 44 | 44 | 
| **Artificial Analysis Capability indexes** |  |  |  |  | 
| Economics Index | 53 | 56 | 53 | 53 | 
| Strategy & Operations Index | 48 | 54 | 50 | 47 | 
| Engineering Index | 47 | 47 | 43 | 45 | 
| Finance & Accounting Index | 46 | 52 | 47 | 45 | 
| Legal Index | 45 | 54 | 49 | 46 | 
| **Benchmark evaluations** |  |  |  |  | 
| AA-Briefcase v1.1 | 54.7% | 55.4% | 51.0% | 47.8% | 
| GDPval-AA v2.1 | 59% | 60% | 51% | 53% | 
| AutomationBench-AA | 58.6% | 65.6% | 58.3% | 51.0% | 
| Terminal-Bench 4.0 | 34.8% | 25.8% | 12.6% | 33.3% | 
| SciCode | 60.9% | 57.4% | 59.5% | 58.9% | 
| Humanity’s Last Exam | 49.4% | 43.1% | 46.9% | 46.5% | 
| GDP.pdf | 19.2% | 20.0% | 22.0% | 14.8% | 
| CritPt | 26.6% | 17.7% | 23.4% | 20.9% | 
| AA-Omniscience Accuracy | 35% | 47% | 48% | 42% | 
| AA-Omniscience Non-Hallucination Rate | 59% | 71% | 47% | 57% | 
| AA-LCR v1.1 | 86.3% | 76.7% | 88.7% | 88.3% | 

The pattern is easy to read. Long-context reasoning and knowledge reliability are standout strengths, coding and agentic work are competitive with the closed frontier, and the clear weak spots are long-horizon terminal tasks and professional document reasoning. Those last two rows are where the top closed models still hold their ground.

Xiaomi’s own evaluation runs, which are not yet independently reproduced, paint an even more aggressive picture for both new models. The table below lines up MiMo V2.6 Pro and MiMo V2.6 Flash against the rivals Xiaomi tested them against. Cells are left empty where we do not have a published result.

| Benchmark | MiMo V2.6 Pro | MiMo V2.6 Flash | MiMo V2.5 Pro | DeepSeek V4.1 Flash | Kimi K3 | Claude Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|---|---|---|---|
| **Code Agent** |  |  |  |  |  |  |  | 
| DeepSWE v1.1 | **71.9** | 67.9 | 19.0 | 74.2 | 69.0 | 74.0 | 73.0* | 
| ProgramBench | **26.5** | 26.0 | 12.5 | 20.3 | 24.5 | 37.0 | 25.0 | 
| MiMo Code Bench | **63.2** | 61.2 | 40.4 | 60.2 | 60.1 | 68.6 | 59.3 | 
| **General Agent** |  |  |  |  |  |  |  | 
| AutomationBench v1.0.6 | **53.1** | 52.3 | 16.0 | 54.8 | 46.7 | 50.3 | 45.8 | 
| Toolathlon-Verified | 76.9 | 73.6 | 49.1 | – | 76.5 | 80.6 | 74.9 | 
| GDPval-AA 2.1 (Elo) | 1673 | – | 1107 | 1600 | 1524 | 1708 | 1588 | 
| Agents’ Last Exam | 31.6 | 27.6 | 13.2 | 31.8 | 28.3 | 31.6 | 30.8 | 
| Terminal Bench 4.0 | 34.9 | 28.8 | 1.5 | 26.8 | 12.6 | 49.0 | 39.9 | 
| Terminal Bench 2.1 | 89.9 | 87.6 | 65.2 | 90.6 | 88.3 | 89.1 | 88.8 | 
| OSWorld-Verified | 82.0 | 80.8 | – | – | 84.8 | 83.4 | 83.0 | 
| JobBench | 62.0 | 61.2 | 25.0 | 45.8 | 54.3 | 65.7 | 45.4 | 
| **Visual Agent** |  |  |  |  |  |  |  | 
| MiMo VisualCoding | 72.3 | 71.5 | – | 70.6 | 70.3 | 70.0 | 73.4 | 
| **Cybersecurity** |  |  |  |  |  |  |  | 
| CyberGym | 94.0 | 95.1 | 40.0 | 88.1 | 80.0 | – | – | 
| ExploitGym | 17.8 | 6.0 | 0.1 | 15.3 | 8.1 | 22.1 | 30.3 | 
| ExploitBench | 47.9 | 25.3 | 16.6 | – | 32.2 | 70.0 | 78.5 | 
| SEC Bench Pro | 66.3 | 47.5 | 17.7 | 62.8 | – | – | 79.1 | 
| MiMo Cyber Bench | 81.7 | 77.2 | 0.0 | 62.7 | 56.3 | – | – | 

** Xiaomi’s own table leaves this cell blank. The figure is taken from the DeepSWE public leaderboard, which reports an average pass score of 73% for GPT-5.6 Sol on DeepSWE v1.1.*

Given how fast Chinese labs have been trading places on this index over the past month, treat the vendor-reported figures as provisional until independent tests are run.

Two things stand out in the current Artificial Analysis standings. First, MiMo V2.6 Pro matches Grok 4.7 at 46, released within the same day. Second, it clears every other open-weights rival, a group that had been trading the crown back and forth for months.



| Model | Lab | Intelligence Index v4.3.2 | Weights | 
|---|---|---|---|
| GPT-6 Astra | OpenAI | 53 | Closed | 
| Claude Fable 5.1 | Anthropic | 53 | Closed | 
| Claude Opus 5 | Anthropic | 51 | Closed | 
| Muse Spark 1.3 | Meta | 48 | Closed | 
| GPT-5.6 Sol | OpenAI | 47 | Closed | 
| Grok 4.7 | xAI | 46 | Closed | 
| **MiMo V2.6 Pro** | **Xiaomi** | **46** | **MIT** | 
| Qwen3.8 Max | Alibaba | 45 | Closed preview | 
| Step 5 Preview | StepFun | 44 | Preview | 
| GLM-5.3 | Z.AI | 44 | Open | 
| Kimi K3 | Moonshot AI | 44 | Open | 
| DeepSeek V4.1 Flash | DeepSeek | 39 | Open | 
| DeepSeek V4 Pro | DeepSeek | 36 | Open | 

In practical terms, the model moves ahead of Kimi K3 and the Step 5 Preview at 44 each, GLM-5.3 at 44, and Qwen3.8 Max at 45. It is also clearly ahead of DeepSeek’s current lineup: DeepSeek V4.1 Flash scores 39 and DeepSeek V4 Pro scores 36, though the anticipated DeepSeek V4.1 Pro could reorder things when it arrives. Hy4 is not yet ranked on Artificial Analysis, so that matchup remains a question mark. Our Hy4 Preview vs MiMo V2.6 Pro comparison lines the two models up on every benchmark both labs publish, alongside the full API pricing breakdown.

Xiaomi held pricing flat from MiMo V2.5, and those prices were already the cheapest in the frontier class. MiMo V2.6 Pro costs $0.435 per million input tokens and $0.87 per million output tokens, with cached input at $0.0036 per million, a discount of about 99%. Xiaomi’s official API pricing, including the half-price Batch API, is laid out below.

| API | Model | Cached input / 1M | Input / 1M | Output / 1M | 
|---|---|---|---|---|
| Real-time | MiMo V2.6 Pro | $0.0036 | $0.435 | $0.87 | 
| Real-time | MiMo V2.6 Flash | $0.0028 | $0.14 | $0.28 | 
| Real-time | MiMo V2.6 Pro UltraSpeed | $0.036 | $4.35 | $8.70 | 
| Batch | MiMo V2.6 Pro | $0.0018 | $0.2175 | $0.435 | 
| Batch | MiMo V2.6 Flash | $0.0014 | $0.07 | $0.14 | 

The gap with American flagships is stark: GPT-5.6 Sol charges around $30 per million output tokens in our LLM API pricing comparison, more than 30 times MiMo V2.6 Pro’s output rate. Artificial Analysis calculates the total cost of running its full Intelligence Index on MiMo V2.6 Pro at just $206.66, about 13 cents per index task. On the Intelligence-versus-cost chart, that combination of a 46 score and near-free execution sits alone in the most attractive corner.

For readers still on the older models: MiMo V2.5 Pro and MiMo V2.5 share those same rates, but Xiaomi lists both as to be deprecated, so new projects should start on V2.6.

Those specifications put sustained agentic workloads within reach of small teams. A full working session that would cost tens of dollars on a closed frontier model typically lands in the low dollars on MiMo V2.6 Pro, and cached-context workloads such as repository-scale code review or long document analysis cost less still. The roughly 99% cache discount is what makes long-running agents and repeated-context chat workloads almost free in practice.

For organizations evaluating a switch, the practical question is no longer whether an open-weights model can handle production workloads, but which provider offers the best mix of price, throughput, and data control.

MiMo is billed pay-as-you-go per token rather than through a flat monthly subscription, though Xiaomi bundles it into consumer apps and offers a MiMo coding plan for developers. MiMo’s roughly 99% cache discounts match DeepSeek’s reputation for making repeated-context workloads like chat history and code review extremely cheap.

Cheap and smart is a familiar pitch. What separates MiMo V2.6 Pro is that it is also fast. Artificial Analysis measured output speed at roughly 130 tokens per second, up from just 46 for MiMo V2.5 Pro. Among models in its intelligence class, only Meta’s Muse Spark 1.3 is both faster and smarter; every other speedster gives up significant benchmark performance.



Latency and time per task are similarly competitive. Time to first token measures about 2.2 seconds, and while GPT-6 Astra and Muse Spark 1.3 complete tasks slightly faster, MiMo V2.6 Pro beats most of the frontier field and comfortably beats its own predecessor. It does use more total tokens per task than V2.5 Pro, but at these prices the difference is close to free in absolute terms.

One honest caveat: measured speeds were recorded on a quiet launch day. Output speed commonly drops as adoption fills capacity, which is exactly why Xiaomi sells the Pro UltraSpeed tier for latency-sensitive workloads.

Launch-day stability is the other caveat, and this comes from our own early testing rather than any leaderboard. In our hands, the API intermittently returned errors, occasionally fired off hundreds of identical tool calls in parallel instead of working through a task step by step, and delivered noticeably slow responses at times. None of that shows up in benchmark scores, all of which run under controlled conditions. It points to bugs or capacity teething problems that are common in a model’s first days and usually get fixed quickly. The timeline explains a lot: the release landed the day after the reinforcement-learning training run finished, leaving essentially no window for extended stability or stress testing before launch. Teams planning to put MiMo V2.6 Pro into production this week should build in retries and keep a fallback model on hand.

The architecture deserves a note, because early parameter claims varied. Xiaomi’s model cards list MiMo V2.6 Pro at 1.02 trillion total parameters with 42 billion active, the same recipe as V2.5 Pro, so the headline gain came from training rather than raw model size.

MiMo V2.6 Pro is compact by frontier standards. DeepSeek V4 Pro carries 1.6 trillion parameters, Qwen3.8 Max runs to 2.4 trillion, and Kimi K3 activates 104 billion parameters per token against MiMo’s 42 billion. Getting to a 46 on the intelligence index with that footprint is an efficiency result as much as a capability result.

The practical consequence is deployment flexibility. Organizations can self-host MiMo V2.6 Pro on their own multi-node infrastructure without per-token royalties, or route to Xiaomi’s API and OpenRouter for convenience. For teams with strict data-residency or confidentiality constraints, running a frontier-scoring model entirely on private hardware is the part of this release that no closed lab currently offers at any price.

The training itself was unusually public. As we covered during the run, Xiaomi livestreamed the reinforcement-learning training with a live dashboard of costs and benchmark progress. The finished models are the product of about 30 reinforcement-learning steps across roughly 750,000 trajectories in under six days.

It is open weights under the MIT license. Model weights are freely downloadable for self-hosting, and Xiaomi has also published the technical report, reinforcement-learning code, and training environments. Strictly speaking, the industry term is “open weights” rather than “open source,” since training data is not fully released.

Pro is the flagship at roughly 1 trillion total parameters and 42 billion active, built for the hardest reasoning and agentic tasks. Flash runs 309 billion total parameters with 15 billion active, costs about a third as much, and comes remarkably close on many agent benchmarks. There is also Pro UltraSpeed, a hosted version of Pro that trades a higher token price for roughly 10 times the output speed.

API access is pay-as-you-go per token rather than a monthly subscription, starting at $0.14 per million input tokens for Flash. Self-hosting is free under the MIT license apart from hardware costs. Xiaomi also bundles MiMo access into its consumer apps and offers a MiMo coding plan, so the practical monthly cost depends entirely on usage.

On the Artificial Analysis Intelligence Index it does, 46 to 36, and it also beats DeepSeek V4.1 Flash at 39. DeepSeek’s anticipated V4.1 Pro release could narrow or reverse the gap, so the open-weights leadership board may shift again soon.

For most coding, analysis, and agentic work, the benchmarks suggest it trades blows with models like GPT-5.6 Sol while costing over 30 times less on output tokens. The main gaps are long-horizon terminal tasks and multi-file programming challenges, where the top closed models still lead. Early adopters should also expect launch-period instability, including API errors and occasional erratic agent behavior.

Price per quality of output is arguably the most important deciding factor in choosing a large language model today, and on that axis MiMo V2.6 Pro is difficult to argue against.

MiMo V2.6 Pro makes three claims that no other model currently holds at once: the cheapest frontier-class model, the top-rated Chinese model, and the top-rated open-weights model on the Artificial Analysis Intelligence Index. A 46 score that matches Grok 4.7 at 13 cents per benchmark task suggests the gap between open and closed AI is now measured in weeks rather than quarters. That timeline held: within a month, Moonshot’s Kimi K3.1 Pro scored 55 on the same index and took over as the top-rated Chinese model, with the open-weights crown set to follow once its weights publish on November 4. Independent verification of Xiaomi’s self-reported evaluations is one remaining formality. Smoothing out the launch-day stability problems we hit in early testing is the other.

Three follow-ups will test how durable this lead is: DeepSeek’s anticipated V4.1 Pro, Artificial Analysis’s pending rankings for Hy4 and the MiMo V2.6 Flash model, and whatever the mimo-x-flash-preview and mimo-x-pro-preview entries in Xiaomi’s catalog eventually become. Those two have been in limited preview testing since before V2.6’s training run began, so the next move from Xiaomi may arrive sooner than a typical release cycle would suggest. Until then, Xiaomi’s release sets the value benchmark every other frontier model has to answer. If both land in the same week as expected, the open-weights leaderboard could look very different again before the month is out, and we will update our coverage as Artificial Analysis publishes new measurements.
