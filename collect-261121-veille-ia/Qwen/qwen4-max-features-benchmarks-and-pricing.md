---
id: collect-261121-veille-ia/qwen/qwen4-max-features-benchmarks-and-pricing
title: "Qwen4-Max: Features, Benchmarks, and Pricing"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen4-max", "alibaba", "pricing", "model", "features", "flagship", "context", "input"]
source: https://www.datacamp.com/blog/qwen4-max
source_anchor: ""
source_lines: [1, 871]
section: "Qwen"
---
Qwen4-Max: Features, Benchmarks, Pricing, and Access

Alibaba's new flagship ships a 2.4 trillion parameter backbone, native vision, and a 1M-token context window at $1.50 per million input tokens, undercutting every Western flagship on price.

Alibaba's Qwen team released Qwen4-Max on November 16, 2026, and the pricing line is the first thing worth looking at: $1.50 per 1 million input tokens and $4.50 per 1 million output tokens on QwenCloud.

For comparison, the roundups circulating the same week list GPT-6 Astra at $10 input and $50 output, and Claude Fable 5.2 at $8 and $40.

Qwen4-Max is the first model built on the new Qwen4 architecture, with a 2.4 trillion parameter backbone, 71B parameters activated per token, an extra 328B N-gram embedding parameters, native vision, and a 1 million token context window.

Alibaba positions it as a long-horizon agent model rather than a chat model, with most of the launch material devoted to multi-hour autonomous runs in engineering, finance, law, and drug discovery.

In this article, I'll cover everything new with Qwen4-Max, looking at the architecture, the key features, the benchmark numbers (including where independent results diverge from Alibaba's own), effort modes, pricing, and access.

Qwen4-Max is Alibaba's new flagship, built on a fresh Qwen4 architecture with native vision and a 1M-token context window.

The gains sit in long-horizon agentic work and professional deliverables, where it beats Claude Opus 5.5 on several of Alibaba's reported runs.

Terminal coding is the weak spot: it trails Kimi K3.1, Opus 5.5, and both GPT-6 models by a wide margin. Chart and video understanding also lag the three US models.

Price is the strongest argument. It undercuts every Western flagship in the current pricing roundups on both input and output, though lower per-token rates do not guarantee lower cost per task.

Treat the numbers with care: the launch chart is vendor-compiled. Independent runs now exist and are more mixed, with Artificial Analysis ranking it ninth and Vals 11th, and testers describing it as slow and verbose. Vals' own Terminal-Bench run does match Alibaba's figure.

No open weights for Max. Smaller Qwen4 models are promised as open source later.

What Is Qwen4-Max?

Qwen4-Max is Alibaba’s flagship closed-weight model and the first release in the Qwen4 series, succeeding Qwen3.8-Max.

It is a sparse mixture-of-experts model, meaning only a fraction of its parameters work on any given token. Of its 2.4 trillion backbone parameters, 71B are active per token, and it adds another 328B in N-gram embeddings, which are lookup tables that cost little compute. It accepts text, image, and video input and produces text output.

The architecture is where Qwen4 departs most from the previous generation, and it helps explain the cost story later. Three components changed:

Hybrid attention (GDN + QSA): for every sparse-attention layer, three Gated DeltaNet layers compress earlier context into a fixed-size memory. The sparse layer then picks out only the relevant parts of the context, which keeps a 1M-token window affordable.

Gated Residual: each layer has four parallel paths, with gates controlling what flows between them. Alibaba credits this for stable training.

N-gram Embedding: lookups for common local token patterns, which add capacity at little extra compute.

Alibaba reports that the 2.4T training run had no loss spikes, the sudden jumps that signal unstable training.

Its base model, Qwen4-Max-Base, also posted the lowest bits per byte of the five checkpoints it evaluated across knowledge, code, QA, and STEM. Bits per byte measures how well a model predicts text, and lower is better.

The result was 5 to 13% below Qwen3.8-Max-Base, while activating 25% fewer parameters. These are in-house tests, and coverage notes that two of the compared checkpoints had received additional training, so the comparison is not strictly like-for-like.

The headline capability figure is Work360, Alibaba’s own benchmark covering 721 professional fields, where Qwen4-Max scored 52.69 against 47.74 for Qwen3.8-Max.

Qwen4-Max Key Features

The launch is organized around what the model delivers at the end of a long run rather than what it says in a single turn, so the features below are mostly about duration, tools, and output formats.

A 1 million token context window with native vision

Qwen4-Max accepts text, images, and video in a 1M-token context window, with a maximum input of 991K tokens, or 983K with thinking on.

Thinking is on by default, and up to 262K tokens of reasoning count against the output budget.

The output ceiling is documented inconsistently.

The QwenCloud model page lists 393K maximum output in both modes, while the API docs give 131,072 tokens without thinking and 262,144 for thinking and the final answer combined. Check which figure applies to your endpoint before you size max_tokens.

The vision path is native rather than bolted on, which is what makes the clinical demo possible: Alibaba describes the model reviewing a record containing dozens of lab reports, over 10,000 Chinese characters of progress notes, and 1,433 original medical images in a single conversation, sorting them into six imaging series before dispatching subagents to read hundreds of slices.

Alibaba says the imaging finding was consistent with the attending physician's diagnosis and that the four management recommendations matched that day's orders point by point.

Hand over a long-running job and steer it mid-flight

Live Delivery is the feature I find most interesting, because it addresses the thing that makes long agent runs unusable in practice.

Instead of waiting for a multi-hour task to finish and discovering the agent misread the brief in minute three, the agent ships a usable first version early, then improves it across successive releases while you redirect it.

Alibaba has open-sourced the Qwen-Live-Delivery-Plugin under Apache-2.0 so you can install it rather than rebuild it, though at launch it is version 0.1.0 and supports Codex CLI only, on Linux and macOS with Node.js 22+.

Support for other agent harnesses is on its to-do list.

Granted, continuous delivery of partial work is a harness pattern more than a model capability, which is why the plugin being open matters more than the marketing name.

Deliverables you can audit, not just answers

The finance demo produces Excel workbooks and editable PowerPoint decks with two traceability mechanisms: hyperlinks inside cells that lead back to the original data source, and a one-to-one mapping between PowerPoint charts and the underlying workbook.

Alibaba says Qwen4 invokes 90 MCP tools across its financial investment research setup, pulling market quotes, financials, interest rates, and dividend data across A-shares, Hong Kong and US equities, bonds, funds, and derivatives, and cross-checking against public disclosures.

That tool count describes the research setup in general. Separately, Alibaba demonstrates the workflow on a CSI 300 derivatives case.

The legal example is the more impressive one.

Given 198 inconsistently named working papers delivered in batches, Qwen4-Max renamed and sorted them by content into 14 folders, built a working paper index, cross-checked public business and judicial records against the papers, and produced an 86-page report covering 12 review areas and 14 specific legal issues, with every conclusion cited back to a working paper.

Running professional tools end to end

Qwen4-Max operates Blender, Unity, Unreal Engine, and Godot directly, handling geometry modeling, material setup, lighting, gameplay logic, and HUD design, then running the result and using visual feedback to adjust camera tracking and hit response.

One of the demos is an installable Android racing game built in Unity 6.

The manufacturing case is the one with a number attached, though it is a company showcase rather than an independent measurement.

At SANF and Pacific Union, the model built 3D production-line models from 2D layouts, simulated cycle times, ran PLC virtual commissioning, and planned machining toolpaths, cutting manual programming effort for a single workpiece from roughly 475 minutes to 24 minutes.

Operations research and cloud scheduling

Alibaba deployed Qwen4-Max against its own Alibaba Cloud scheduling problems, letting the model generate candidate algorithms, check feasibility and objective values with MindOpt (Alibaba's solver), then iterate on the code.

The reported results show that data annotation model-call scheduling costs fell 10.42% against the reference baseline, online GPU allocation increased whole-machine idle capacity duration by 4.62%, and offline CPU rescheduling reached its optimal objective more than 14.61 times faster.

Those are self-reported internal figures on internal workloads, so treat them as a demonstration of operations research tool use rather than a transferable benchmark.

Autonomous research loops that reach physical results

During a continuous 48-hour agent run on Deep Principle's Mira platform, Qwen4 constructed a candidate space of 2.7 million molecules, screened them, ran first-principles calculations and retrosynthetic analysis, then directed an automated wet lab through synthesis, purification, and testing.

After four iterations of design, synthesis, and active learning, it produced a physical sample of a new coolant molecule.

On DP Technology's PharmMaster platform, targeting the NLRP3 inflammasome, it made 454 tool calls, ran 3,239 computational metric evaluations, and designed 24 candidate molecules.

Relative binding free-energy calculations put the final candidate's nominal affinity at about 3.6 times the patent-derived reference, which DP Technology's experts judged worth wet-lab validation.

Standard API plumbing: function calling, web search, and structured outputs

On QwenCloud, Qwen4-Max supports function calling, structured JSON outputs, prefix completion via Partial Mode, implicit and explicit context caching, batch calling, web search, and fine-tuning. Rate limits are set at 5,000,000 tokens per minute.

How Does Qwen4-Max Perform on the Benchmarks?

Qwen4-Max is strongest on long-horizon agentic work and weakest on terminal coding and visual understanding, and the gap between those is large enough that it should determine whether you adopt it.

The standout is Agents' Last Exam v1.1, where Qwen4-Max scored 46.7 against 43.4 for Claude Opus 5.5, 42.6 for GPT-6.1 Sol, and 27.9 for its own predecessor Qwen3.8-Max.

A jump of 18.8 points in one generation on a long-horizon task benchmark is the single result I'd point to from this release.

All scores below come from Alibaba's launch chart, which is vendor-compiled, so read them as vendor-reported even though some rows cite third-party leaderboards. The best result in each row is in bold.

Benchmark

What it measures

Qwen4-Max

Qwen3.8-Max-0902

Opus 5.5

GPT-6.1 Sol

GPT-6 Astra

Kimi K3.1

Agents' Last Exam v1.1

Long-horizon valuable tasks

46.7

27.9

43.4

42.6

43.4

n/a

AA-Briefcase v1.1 (Elo)

Agentic knowledge work

1686

1605

1801

1555

1569

1674

APEX-Agents v1.1

Professional services tasks

72.1

63.3

73.5

60.0

64.7

65.5

AutomationBench-AA

Agentic SaaS workflows

72.5

56.2

69.5

64.9

68.5

73.3

HLAB-Vals-1.1

Legal agent work

21.3

15.4

9.2

6.3

7.1

11.7

JobBench

Professional job tasks

68.5

65.3

75.4

66.2

66.1

61.2

AgentArena (net improvement)

Real-world agent tasks

7.5

n/a

18.2

11.8

13.9

3.9

The pattern is uneven. Qwen4-Max takes the legal agent benchmark by a wide margin, nearly double the next model (21.3 against Kimi K3.1's 11.7), finishes second on AA-Briefcase, APEX-Agents, AutomationBench-AA, and JobBench, and lands near the bottom on AgentArena, where Opus 5.5's 18.2 is more than twice its 7.5.

If your agents do document-heavy professional work, this looks good; if they operate live tools in open environments, less so.

Coding and terminal tasks

Coding is where I'd hold back. Qwen4-Max scores 48.5 on Terminal-Bench 4.0, behind Kimi K3.1 at 67.2, Opus 5.5 at 59.6, GPT-6 Astra at 59.1, and GPT-6.1 Sol at 56.1, which makes it fifth of the six models charted, even though it improved almost 10 points over Qwen3.8-Max's 38.9.

Benchmark

What it measures

Qwen4-Max

Qwen3.8-Max-0902

Opus 5.5

GPT-6.1 Sol

GPT-6 Astra

Kimi K3.1

DeepSWE v1.1

Agentic coding

75.7

69.3

74.2

75.2

74.1

75.5

CodeArena (Elo)

Web development

1750

1676

1816

1742

1763

1686

Terminal-Bench 4.0

Agentic terminal coding

48.5

38.9

59.6

56.1

59.1

67.2

ProgramBench

Black-box software replication

31.5

28.0

65.0

52.5

55.0

25.0

ProgramBench is the ugly one: 31.5 against 65.0 for Opus 5.5, less than half. On DeepSWE, five models land within 1.6 points of each other, so I'd treat that row as noise rather than a win, even though Qwen4-Max is nominally first.

One caveat on the Terminal-Bench row.

When we compared GPT-6 Astra and Claude Opus 5.5, the published figures were 66.4% for Opus 5.5 and 57.9% for Astra on Terminal-Bench 4.0, against the 59.6 and 59.1 in Alibaba's chart.

Different harnesses produce different numbers on the same benchmark, which is a good reason not to read 5-point gaps as rankings. Qwen4-Max's own figure looks solid, though: Vals independently measured 48.48%.

Reasoning, science, and multimodal

Qwen4-Max scores 51.7 on Humanity's Last Exam, behind Opus 5.5 at 59.8 and GPT-6.1 Sol at 54.0, and ahead of Kimi K3.1 at 51.1. It leads on BioMysteryBench-1.1 with 85.0 and improves hugely on HealthBench Professional, going from 39.6 to 60.0, though both GPT-6 models still score higher.

Benchmark

What it measures

Qwen4-Max

Qwen3.8-Max-0902

Opus 5.5

GPT-6.1 Sol

GPT-6 Astra

Kimi K3.1

Humanity's Last Exam

Multidisciplinary reasoning

51.7

43.9

59.8

54.0

53.6

51.1

BioMysteryBench-1.1

Bioinformatics research

85.0

84.1

81.9

84.4

78.9

80.6

HealthBench Professional

Clinical reasoning

60.0

39.6

59.9

65.1

66.0

57.2

Chartography

Visual chart understanding

54.2

42.0

66.3

67.8

71.1

50.0

Video-Index

Video understanding

68.1

63.2

74.8

73.8

76.7

63.2

The vision rows are the least flattering part of the chart.

On Chartography, Qwen4-Max scored 54.2, up from 42.0 for Qwen3.8-Max, but it trails Opus 5.5, GPT-6.1 Sol, and GPT-6 Astra by 12 to 17 points and beats only Kimi K3.1. Video-Index tells the same story, with 68.1 against 73.8 to 76.7 for the three US models. Native vision works, but it is not a lead.

Token efficiency and cost per task

This is the result that most affects a production budget.

On Work360, Qwen4-Max used about 24% fewer tokens at Xhigh effort and about 35% fewer at High effort than Qwen3.8-Max, and on Terminal-Bench 4.0 the reductions were about 48% and 49%.

Combined with lower API rates, Alibaba reports the average cost per Work360 task fell from $4.43 to $1.67, a 62% reduction.

Scoring higher while spending about 38% as much per task is a more useful claim than most single-benchmark wins, but two caveats apply. Work360 is Alibaba's own benchmark, and the reduction is larger than the token and price cuts alone would suggest: roughly 35% fewer tokens at a 25% lower price implies about 51%, and Alibaba's yuan figures give 58%. Alibaba hasn't explained the gap.

Independent per-task costs are less clear-cut against one rival. On Artificial Analysis's index, a task averages $3.27 for Qwen4-Max against $10.04 for Opus 5.5, but only $1.27 for GPT-6.1 Sol.

On Vals, a test costs $9.33 for Qwen4-Max against $24.77 for Opus 5.5 and $3.61 for Sol, and Sol scores slightly higher (63.94% against 63.20%).

Kimi K3.1 is the closer comparison for most readers weighing an open-ecosystem alternative. It beats Qwen4-Max on Terminal-Bench 4.0 (67.2 versus 48.5) and AutomationBench-AA (73.3 versus 72.5), while Qwen4-Max leads on APEX-Agents v1.1 (72.1 versus 65.5), HLAB-Vals-1.1 (21.3 versus 11.7), and JobBench (68.5 versus 61.2). Pick Kimi for terminal work, Qwen4-Max for professional document workflows.

What independent testing shows

The launch chart is vendor-compiled.

Some rows cite third-party leaderboards, including LM Arena for CodeArena and AgentArena, Artificial Analysis for AA-Briefcase and AutomationBench-AA, the Mercor leaderboard for APEX-Agents v1.1, and the Vals Index for HLAB-Vals-1.1, with Agents' Last Exam cited from its authors.

Alibaba ran JobBench itself. I haven't found independent reproductions of the agentic results, which is why I'd still treat those wins as provisional.

Independent leaderboards are now filling in the broader picture, and they are less flattering than the launch material.

Artificial Analysis scores Qwen4-Max 43 on its Intelligence Index, ninth overall, behind Claude Fable 5.2, Gemini 4 Carbon, Opus 5.5, GPT-6 Astra, GPT-6.1 Sol, and Kimi K3.1.

Vals ranks it 11th of 38 on its Index at 63.20%, the highest of any Chinese model it has tested, and its independent Terminal-Bench run matches Alibaba's number.

On Code Arena WebDev it sits sixth, though still flagged preliminary on a small vote count.

Finance is its best independent showing, eighth of 41 on both Vals' finance index and its Finance Agent benchmark; legal is weaker, 17th of 39 on the Vals Legal Index, which sits oddly beside the HLAB-Vals lead.

Slowness and verbosity are the consistent complaints. Vals clocked about 1 hour 4 minutes per test, slower than all but Opus 5.5 and Sonnet 5.5 among the top 11, and Artificial Analysis recorded 127 million output tokens on its index against a 78.6 million median.

Anecdotal hands-on reports are mixed and often contradict each other, with testers describing overthinking on simple tasks and long completion times on long prompts; one tester rated it below Qwen3.8-Max outright, while another reported it finishing an agentic task in roughly 30 minutes against roughly 70 minutes for MiniMax M3.1 Flash.

Which Qwen4-Max Tier Should You Use?

Qwen4-Max ships as one model with a thinking toggle and named effort levels, so the choice is about how much reasoning you pay for rather than which checkpoint you call. Thinking is on by default, at high effort.

"Standard mode" therefore means setting enable_thinking to false. Input capacity drops slightly with thinking on, from 991K to 983K, and reasoning (up to 262K tokens) draws on the output budget.

Because thinking tokens count toward that budget, a max_tokens sized for the visible reply alone can truncate the answer; on the preview, max_tokens capped thinking and answer together, so assume the same until you've tested.

The documented output ceilings differ between the API docs (131,072 without thinking, 262,144 with) and the model page (393K in both modes), as noted above.

Effort is the second dial.

Alibaba reports Work360 and Terminal-Bench 4.0 results at both Xhigh and High, and the token savings against Qwen3.8-Max were larger at High on Work360 (35% versus 24%) and roughly equal on Terminal-Bench. Since reasoning tokens bill at the output rate of $4.50 per million, the effort setting is the single biggest lever on your bill.

High is also Qwen4-Max's default, while Qwen3.8-Max defaulted to xhigh, so a default-settings comparison between the two generations mixes the model change with an effort change. Set effort explicitly when you A/B test.

Use case

Setting

Why

Multi-hour agentic runs with tool calls

Thinking, High effort

Best token efficiency gain over the previous generation on Work360

Long document review across 100+ files

Thinking, Xhigh effort

262K reasoning budget plus the 1M context window

Structured extraction into JSON

Thinking off

Structured outputs are supported, and turning thinking off avoids reasoning overhead

High-volume batch classification

Thinking off plus batch calling

Caching drops input to $0.075 per million tokens

Hard terminal coding

Consider another model

48.5 on Terminal-Bench 4.0 trails Kimi K3.1, Opus 5.5, and both GPT-6 models

Qwen4-Max Pricing and Availability

QwenCloud lists Qwen4-Max at $1.50 per 1 million input tokens and $4.50 per 1 million output tokens on its international list, with cached input at $0.075 per million. Explicit cache creation costs $1.875 per million for the 5-minute window and $3.00 per million for the 1-hour window, and explicit cache reads are $0.075 per million.

Those rates undercut every Western flagship in the current pricing roundups: GPT-6 Astra at $10 input and $50 output (for prompts up to 272K tokens), Claude Fable 5.2 at $8 and $40, Gemini 4 Carbon at $2 and $10 through November 30 (rising to $4 and $20 after that), and Grok 4.7 at $2 and $6 (under 200K context).

Those are the top-priced tiers, though, and the models Alibaba benchmarks against are cheaper: Claude Opus 5.5 at $4 and $20, and GPT-6.1 Sol and Kimi K3.1 at $2 and $10. Against Astra, Qwen4-Max's output rate is roughly a tenth; against Opus 5.5 it is about 22%, and against Sol and Kimi about 45%.

There is a free quota of 1 million tokens on QwenCloud, available in the Singapore region for about 90 days, and rate limits are published at 5,000,000 tokens per minute.

Alibaba also sells access through a Token Plan rather than pure pay-as-you-go, with third-party reports listing personal tiers at Lite $6/month for 20,000 credits, Essential $10/month for 45,000, Standard $18/month for 80,000, and Pro $43/month for 200,000, plus discounted new-user prices of $3 and $5 on the two cheapest tiers, reported to run through November 30.

The personal plan is a Singapore offering, and during the preview Alibaba restricted it to interactive coding and agent tools rather than production backends, so check the current terms before building on it.

A note on the timeline, because early coverage conflicted on whether a public per-token price existed.

Alibaba opened qwen4-max-preview on November 11, on Token Plan only, with no per-token price and a retirement date of November 25. Pay-as-you-go pricing arrived with the general release on November 16 under the model ID qwen4-max, and the $1.50 and $4.50 figures above are the international list prices as of that date.

Open weights for Max have not been announced; Alibaba says smaller Qwen4 models will be open-sourced later.

How to Get Access to Qwen4-Max?

The API model ID is qwen4-max, served through QwenCloud under either the Token Plan or the standard API, with OpenAI-compatible and Anthropic-compatible endpoints. Outside the API, the model is available in QwenStudio (the Qwen Chat interface), QwenWork, and the Qoder coding product, which Alibaba co-designed alongside the model.

Here is the minimal call using the OpenAI Python client against the compatible-mode Chat Completions endpoint. Thinking is on by default, so the enable_thinking flag is shown only for explicitness; set it to False to switch thinking off.

import os
from openai import OpenAI
client = OpenAI(
api_key=os.getenv("DASHSCOPE_API_KEY"),
base_url="https://ap-southeast-1.maas.qwencloudapi.com/compatible-mode/v1",)
response = client.chat.completions.create(
model="qwen4-max",
messages=[{"role":"user","content":"Summarize the revenue drivers in this quarterly report."}],
extra_body={"enable_thinking":True},# on by default; False turns thinking off)print(response.choices[0].message.content)Powered by

If you want to try Live Delivery, the plugin is on GitHub as Qwen-Live-Delivery-Plugin. Because it currently targets Codex CLI, using it in another harness means wiring up its MCP delivery tools and hooks yourself. For the general patterns behind function calling and agent harnesses, our Harness² tutorial walks through wiring a model into OpenCode and grading the results.

Final Thoughts

Qwen4-Max is Alibaba making a price argument more than a capability argument.

It wins the rows that involve reading a lot of documents and producing a deliverable, loses the rows that involve a terminal, a chart, or a video, and does all of it at roughly a tenth of GPT-6 Astra's output rate.

Would I switch? For document-heavy agent pipelines where cost per task is the constraint, it is worth a serious test, but not a blind migration.

The drop from $4.43 to $1.67 per Work360 task is Alibaba's own figure, and independent runs are more mixed: GPT-6.1 Sol was cheaper per task and scored slightly higher on Vals, and Qwen4-Max is slow and verbose. For hard coding work, I would stay with Opus 5.5 or Kimi K3.1, and Vals' independent Terminal-Bench run confirms that weakness.

If you want to get properly comfortable building with models like this, start with our AI Fundamentals skill track.

FAQs

How is Qwen4-Max different from Qwen3.8-Max?⌄

Qwen4-Max is the first model on the new Qwen4 architecture, with a 2.4 trillion parameter backbone, 71B parameters activated per token, and native vision. It scored 52.69 on Alibaba's Work360 benchmark against 47.74 for Qwen3.8-Max, and jumped from 27.9 to 46.7 on Agents' Last Exam v1.1. It also uses roughly 24% to 49% fewer tokens on the same tasks, cutting average Work360 cost per task from $4.43 to $1.67.

How much does Qwen4-Max cost per million tokens?⌄

QwenCloud lists Qwen4-Max at $1.50 per 1 million input tokens and $4.50 per 1 million output tokens, with cached input and explicit cache reads at $0.075 per million. Explicit cache creation costs $1.875 per million for a 5-minute window and $3.00 per million for a 1-hour window. There is a 1 million token free quota, and Alibaba also sells Token Plan subscriptions starting around $6 per month.

Where can I access Qwen4-Max?⌄

Qwen4-Max is available on QwenCloud through either the Token Plan or the API, using the model ID qwen4-max against an OpenAI-compatible endpoint. It also runs inside QwenStudio (Qwen Chat), QwenWork, and the Qoder coding product. Some third-party documentation refers to a preview ID, qwen4-max-preview.

Is Qwen4-Max open source?⌄

No. Qwen4-Max is closed-weight, and Alibaba has not announced open weights or a public model card for it. Alibaba has said smaller models in the Qwen4 family will be released as open source later, built on the same lineage and training techniques.

Is Qwen4-Max good for coding?⌄

It is competitive on agentic coding and weak on terminal work. Qwen4-Max scores 75.7 on DeepSWE v1.1, roughly level with Claude Opus 5.5 (74.2) and GPT-6.1 Sol (75.2), but only 48.5 on Terminal-Bench 4.0 against 67.2 for Kimi K3.1, and 31.5 on ProgramBench against 65.0 for Opus 5.5. For hard terminal and software replication tasks, other models remain stronger.

What is Live Delivery in Qwen4-Max?⌄

Live Delivery is Qwen4-Max's approach to long-running agent tasks: instead of returning results only when the job is finished, the agent ships a usable first version early and improves it through successive releases while you redirect it mid-run. Alibaba has open-sourced the Qwen-Live-Delivery-Plugin on GitHub so you can install it rather than build the pattern yourself.

Qwen3.8-Max is Alibaba's 2.4T-parameter flagship, topping OSWorld-Verified and PaperBench. Here's what's new, the benchmarks, and what it means for the industry.

Alibaba's Qwen3.7-Max is a new proprietary flagship model built for agentic workflows, with top-tier scores on coding, reasoning, and long-horizon task benchmarks.

Alibaba's Qwen team launched QwQ-32B, a 32-billion parameter, open-source AI model for complex reasoning, competing with larger models like DeepSeek-R1.

Qwen3.8-Flash-Next is Alibaba's open-weight 125B MoE model previewing the Qwen4 architecture. It beats Claude Opus 4.6 Max on most coding and agent benchmarks.

Alibaba's Qwen team released Qwen4-Max on November 16, 2026, and the pricing line is the first thing worth looking at: $1.50 per 1 million input tokens and $4.50 per 1 million output tokens on QwenCloud.

For comparison, the roundups circulating the same week list GPT-6 Astra at $10 input and $50 output, and Claude Fable 5.2 at $8 and $40.

Qwen4-Max is the first model built on the new Qwen4 architecture, with a 2.4 trillion parameter backbone, 71B parameters activated per token, an extra 328B N-gram embedding parameters, native vision, and a 1 million token context window.

Alibaba positions it as a long-horizon agent model rather than a chat model, with most of the launch material devoted to multi-hour autonomous runs in engineering, finance, law, and drug discovery.

In this article, I'll cover everything new with Qwen4-Max, looking at the architecture, the key features, the benchmark numbers (including where independent results diverge from Alibaba's own), effort modes, pricing, and access.

For context on what it is competing against, see our comparison of GPT-6 Astra and Claude Opus 5.5 and our tutorial on building self-improving agents with Harness².

## TL;DR

## What Is Qwen4-Max?

Qwen4-Max is Alibaba’s flagship closed-weight model and the first release in the Qwen4 series, succeeding Qwen3.8-Max.

It is a sparse mixture-of-experts model, meaning only a fraction of its parameters work on any given token. Of its 2.4 trillion backbone parameters, 71B are active per token, and it adds another 328B in N-gram embeddings, which are lookup tables that cost little compute. It accepts text, image, and video input and produces text output.

The architecture is where Qwen4 departs most from the previous generation, and it helps explain the cost story later. Three components changed:

Hybrid attention (GDN + QSA):for every sparse-attention layer, three Gated DeltaNet layers compress earlier context into a fixed-size memory. The sparse layer then picks out only the relevant parts of the context, which keeps a 1M-token window affordable.Gated Residual:each layer has four parallel paths, with gates controlling what flows between them. Alibaba credits this for stable training.N-gram Embedding:lookups for common local token patterns, which add capacity at little extra compute.
Alibaba reports that the 2.4T training run had no loss spikes, the sudden jumps that signal unstable training.

Its base model, Qwen4-Max-Base, also posted the lowest bits per byte of the five checkpoints it evaluated across knowledge, code, QA, and STEM. Bits per byte measures how well a model predicts text, and lower is better.

The result was 5 to 13% below Qwen3.8-Max-Base, while activating 25% fewer parameters. These are in-house tests, and coverage notes that two of the compared checkpoints had received additional training, so the comparison is not strictly like-for-like.

The headline capability figure is Work360, Alibaba’s own benchmark covering 721 professional fields, where Qwen4-Max scored 52.69 against 47.74 for Qwen3.8-Max.

## Qwen4-Max Key Features

The launch is organized around what the model delivers at the end of a long run rather than what it says in a single turn, so the features below are mostly about duration, tools, and output formats.

## A 1 million token context window with native vision

Qwen4-Max accepts text, images, and video in a 1M-token context window, with a maximum input of 991K tokens, or 983K with thinking on.

Thinking is on by default, and up to 262K tokens of reasoning count against the output budget.

The output ceiling is documented inconsistently.

The QwenCloud model page lists 393K maximum output in both modes, while the API docs give 131,072 tokens without thinking and 262,144 for thinking and the final answer combined. Check which figure applies to your endpoint before you size

`max_tokens`.
The vision path is native rather than bolted on, which is what makes the clinical demo possible: Alibaba describes the model reviewing a record containing dozens of lab reports, over 10,000 Chinese characters of progress notes, and 1,433 original medical images in a single conversation, sorting them into six imaging series before dispatching subagents to read hundreds of slices.

Alibaba says the imaging finding was consistent with the attending physician's diagnosis and that the four management recommendations matched that day's orders point by point.

## Hand over a long-running job and steer it mid-flight

Live Delivery is the feature I find most interesting, because it addresses the thing that makes long agent runs unusable in practice.

Instead of waiting for a multi-hour task to finish and discovering the agent misread the brief in minute three, the agent ships a usable first version early, then improves it across successive releases while you redirect it.

Alibaba has open-sourced the Qwen-Live-Delivery-Plugin under Apache-2.0 so you can install it rather than rebuild it, though at launch it is version 0.1.0 and supports Codex CLI only, on Linux and macOS with Node.js 22+.

Support for other agent harnesses is on its to-do list.

Granted, continuous delivery of partial work is a harness pattern more than a model capability, which is why the plugin being open matters more than the marketing name.

## Deliverables you can audit, not just answers

The finance demo produces Excel workbooks and editable PowerPoint decks with two traceability mechanisms: hyperlinks inside cells that lead back to the original data source, and a one-to-one mapping between PowerPoint charts and the underlying workbook.

Alibaba says Qwen4 invokes 90 MCP tools across its financial investment research setup, pulling market quotes, financials, interest rates, and dividend data across A-shares, Hong Kong and US equities, bonds, funds, and derivatives, and cross-checking against public disclosures.

That tool count describes the research setup in general. Separately, Alibaba demonstrates the workflow on a CSI 300 derivatives case.

The legal example is the more impressive one.

Given 198 inconsistently named working papers delivered in batches, Qwen4-Max renamed and sorted them by content into 14 folders, built a working paper index, cross-checked public business and judicial records against the papers, and produced an 86-page report covering 12 review areas and 14 specific legal issues, with every conclusion cited back to a working paper.

## Running professional tools end to end

Qwen4-Max operates Blender, Unity, Unreal Engine, and Godot directly, handling geometry modeling, material setup, lighting, gameplay logic, and HUD design, then running the result and using visual feedback to adjust camera tracking and hit response.

One of the demos is an installable Android racing game built in Unity 6.

The manufacturing case is the one with a number attached, though it is a company showcase rather than an independent measurement.

At SANF and Pacific Union, the model built 3D production-line models from 2D layouts, simulated cycle times, ran PLC virtual commissioning, and planned machining toolpaths, cutting manual programming effort for a single workpiece from roughly 475 minutes to 24 minutes.

## Operations research and cloud scheduling

Alibaba deployed Qwen4-Max against its own Alibaba Cloud scheduling problems, letting the model generate candidate algorithms, check feasibility and objective values with MindOpt (Alibaba's solver), then iterate on the code.

The reported results show that data annotation model-call scheduling costs fell 10.42% against the reference baseline, online GPU allocation increased whole-machine idle capacity duration by 4.62%, and offline CPU rescheduling reached its optimal objective more than 14.61 times faster.

Those are self-reported internal figures on internal workloads, so treat them as a demonstration of operations research tool use rather than a transferable benchmark.

## Autonomous research loops that reach physical results

During a continuous 48-hour agent run on Deep Principle's Mira platform, Qwen4 constructed a candidate space of 2.7 million molecules, screened them, ran first-principles calculations and retrosynthetic analysis, then directed an automated wet lab through synthesis, purification, and testing.

After four iterations of design, synthesis, and active learning, it produced a physical sample of a new coolant molecule.

On DP Technology's PharmMaster platform, targeting the NLRP3 inflammasome, it made 454 tool calls, ran 3,239 computational metric evaluations, and designed 24 candidate molecules.

Relative binding free-energy calculations put the final candidate's nominal affinity at about 3.6 times the patent-derived reference, which DP Technology's experts judged worth wet-lab validation.

## Standard API plumbing: function calling, web search, and structured outputs

On QwenCloud, Qwen4-Max supports function calling, structured JSON outputs, prefix completion via Partial Mode, implicit and explicit context caching, batch calling, web search, and fine-tuning. Rate limits are set at 5,000,000 tokens per minute.

## How Does Qwen4-Max Perform on the Benchmarks?

Qwen4-Max is strongest on long-horizon agentic work and weakest on terminal coding and visual understanding, and the gap between those is large enough that it should determine whether you adopt it.

Alibaba compares it against Qwen3.8-Max-0902, Claude Opus 5.5, GPT-6.1 Sol, GPT-6 Astra, and Kimi K3.1 (Moonshot's Kimi-K3.1-Pro).

## Long-horizon and agentic work

The standout is Agents' Last Exam v1.1, where Qwen4-Max scored 46.7 against 43.4 for Claude Opus 5.5, 42.6 for GPT-6.1 Sol, and 27.9 for its own predecessor Qwen3.8-Max.

A jump of 18.8 points in one generation on a long-horizon task benchmark is the single result I'd point to from this release.

All scores below come from Alibaba's launch chart, which is vendor-compiled, so read them as vendor-reported even though some rows cite third-party leaderboards. The best result in each row is in bold.

46.7180173.573.321.375.418.2
The pattern is uneven. Qwen4-Max takes the legal agent benchmark by a wide margin, nearly double the next model (21.3 against Kimi K3.1's 11.7), finishes second on AA-Briefcase, APEX-Agents, AutomationBench-AA, and JobBench, and lands near the bottom on AgentArena, where Opus 5.5's 18.2 is more than twice its 7.5.

If your agents do document-heavy professional work, this looks good; if they operate live tools in open environments, less so.

## Coding and terminal tasks

Coding is where I'd hold back. Qwen4-Max scores 48.5 on Terminal-Bench 4.0, behind Kimi K3.1 at 67.2, Opus 5.5 at 59.6, GPT-6 Astra at 59.1, and GPT-6.1 Sol at 56.1, which makes it fifth of the six models charted, even though it improved almost 10 points over Qwen3.8-Max's 38.9.

75.7181667.265.0
ProgramBench is the ugly one: 31.5 against 65.0 for Opus 5.5, less than half. On DeepSWE, five models land within 1.6 points of each other, so I'd treat that row as noise rather than a win, even though Qwen4-Max is nominally first.

One caveat on the Terminal-Bench row.

When we compared GPT-6 Astra and Claude Opus 5.5, the published figures were 66.4% for Opus 5.5 and 57.9% for Astra on Terminal-Bench 4.0, against the 59.6 and 59.1 in Alibaba's chart.

Different harnesses produce different numbers on the same benchmark, which is a good reason not to read 5-point gaps as rankings. Qwen4-Max's own figure looks solid, though: Vals independently measured 48.48%.

## Reasoning, science, and multimodal

Qwen4-Max scores 51.7 on Humanity's Last Exam, behind Opus 5.5 at 59.8 and GPT-6.1 Sol at 54.0, and ahead of Kimi K3.1 at 51.1. It leads on BioMysteryBench-1.1 with 85.0 and improves hugely on HealthBench Professional, going from 39.6 to 60.0, though both GPT-6 models still score higher.

59.885.066.071.176.7
The vision rows are the least flattering part of the chart.

On Chartography, Qwen4-Max scored 54.2, up from 42.0 for Qwen3.8-Max, but it trails Opus 5.5, GPT-6.1 Sol, and GPT-6 Astra by 12 to 17 points and beats only Kimi K3.1. Video-Index tells the same story, with 68.1 against 73.8 to 76.7 for the three US models. Native vision works, but it is not a lead.

## Token efficiency and cost per task

This is the result that most affects a production budget.

On Work360, Qwen4-Max used about 24% fewer tokens at Xhigh effort and about 35% fewer at High effort than Qwen3.8-Max, and on Terminal-Bench 4.0 the reductions were about 48% and 49%.

Combined with lower API rates, Alibaba reports the average cost per Work360 task fell from $4.43 to $1.67, a 62% reduction.

Scoring higher while spending about 38% as much per task is a more useful claim than most single-benchmark wins, but two caveats apply. Work360 is Alibaba's own benchmark, and the reduction is larger than the token and price cuts alone would suggest: roughly 35% fewer tokens at a 25% lower price implies about 51%, and Alibaba's yuan figures give 58%. Alibaba hasn't explained the gap.

Independent per-task costs are less clear-cut against one rival. On Artificial Analysis's index, a task averages $3.27 for Qwen4-Max against $10.04 for Opus 5.5, but only $1.27 for GPT-6.1 Sol.

On Vals, a test costs $9.33 for Qwen4-Max against $24.77 for Opus 5.5 and $3.61 for Sol, and Sol scores slightly higher (63.94% against 63.20%).

Kimi K3.1 is the closer comparison for most readers weighing an open-ecosystem alternative. It beats Qwen4-Max on Terminal-Bench 4.0 (67.2 versus 48.5) and AutomationBench-AA (73.3 versus 72.5), while Qwen4-Max leads on APEX-Agents v1.1 (72.1 versus 65.5), HLAB-Vals-1.1 (21.3 versus 11.7), and JobBench (68.5 versus 61.2). Pick Kimi for terminal work, Qwen4-Max for professional document workflows.

## What independent testing shows

The launch chart is vendor-compiled.

Some rows cite third-party leaderboards, including LM Arena for CodeArena and AgentArena, Artificial Analysis for AA-Briefcase and AutomationBench-AA, the Mercor leaderboard for APEX-Agents v1.1, and the Vals Index for HLAB-Vals-1.1, with Agents' Last Exam cited from its authors.

Alibaba ran JobBench itself. I haven't found independent reproductions of the agentic results, which is why I'd still treat those wins as provisional.

Independent leaderboards are now filling in the broader picture, and they are less flattering than the launch material.

Slowness and verbosity are the consistent complaints. Vals clocked about 1 hour 4 minutes per test, slower than all but Opus 5.5 and Sonnet 5.5 among the top 11, and Artificial Analysis recorded 127 million output tokens on its index against a 78.6 million median.

Anecdotal hands-on reports are mixed and often contradict each other, with testers describing overthinking on simple tasks and long completion times on long prompts; one tester rated it below Qwen3.8-Max outright, while another reported it finishing an agentic task in roughly 30 minutes against roughly 70 minutes for MiniMax M3.1 Flash.

## Which Qwen4-Max Tier Should You Use?

Qwen4-Max ships as one model with a thinking toggle and named effort levels, so the choice is about how much reasoning you pay for rather than which checkpoint you call. Thinking is on by default, at high effort.

"Standard mode" therefore means setting

`enable_thinking`to false. Input capacity drops slightly with thinking on, from 991K to 983K, and reasoning (up to 262K tokens) draws on the output budget.
Because thinking tokens count toward that budget, a

`max_tokens`sized for the visible reply alone can truncate the answer; on the preview,`max_tokens`capped thinking and answer together, so assume the same until you've tested.
The documented output ceilings differ between the API docs (131,072 without thinking, 262,144 with) and the model page (393K in both modes), as noted above.

Effort is the second dial.

Alibaba reports Work360 and Terminal-Bench 4.0 results at both Xhigh and High, and the token savings against Qwen3.8-Max were larger at High on Work360 (35% versus 24%) and roughly equal on Terminal-Bench. Since reasoning tokens bill at the output rate of $4.50 per million, the effort setting is the single biggest lever on your bill.

High is also Qwen4-Max's default, while Qwen3.8-Max defaulted to xhigh, so a default-settings comparison between the two generations mixes the model change with an effort change. Set effort explicitly when you A/B test.

## Qwen4-Max Pricing and Availability

QwenCloud lists Qwen4-Max at $1.50 per 1 million input tokens and $4.50 per 1 million output tokens on its international list, with cached input at $0.075 per million. Explicit cache creation costs $1.875 per million for the 5-minute window and $3.00 per million for the 1-hour window, and explicit cache reads are $0.075 per million.

Those rates undercut every Western flagship in the current pricing roundups: GPT-6 Astra at $10 input and $50 output (for prompts up to 272K tokens), Claude Fable 5.2 at $8 and $40, Gemini 4 Carbon at $2 and $10 through November 30 (rising to $4 and $20 after that), and Grok 4.7 at $2 and $6 (under 200K context).

Those are the top-priced tiers, though, and the models Alibaba benchmarks against are cheaper: Claude Opus 5.5 at $4 and $20, and GPT-6.1 Sol and Kimi K3.1 at $2 and $10. Against Astra, Qwen4-Max's output rate is roughly a tenth; against Opus 5.5 it is about 22%, and against Sol and Kimi about 45%.

There is a free quota of 1 million tokens on QwenCloud, available in the Singapore region for about 90 days, and rate limits are published at 5,000,000 tokens per minute.

Alibaba also sells access through a Token Plan rather than pure pay-as-you-go, with third-party reports listing personal tiers at Lite $6/month for 20,000 credits, Essential $10/month for 45,000, Standard $18/month for 80,000, and Pro $43/month for 200,000, plus discounted new-user prices of $3 and $5 on the two cheapest tiers, reported to run through November 30.

The personal plan is a Singapore offering, and during the preview Alibaba restricted it to interactive coding and agent tools rather than production backends, so check the current terms before building on it.

A note on the timeline, because early coverage conflicted on whether a public per-token price existed.

Alibaba opened

`qwen4-max-preview`on November 11, on Token Plan only, with no per-token price and a retirement date of November 25. Pay-as-you-go pricing arrived with the general release on November 16 under the model ID`qwen4-max`, and the $1.50 and $4.50 figures above are the international list prices as of that date.
Open weights for Max have not been announced; Alibaba says smaller Qwen4 models will be open-sourced later.

## How to Get Access to Qwen4-Max?

The API model ID is

`qwen4-max`, served through QwenCloud under either the Token Plan or the standard API, with OpenAI-compatible and Anthropic-compatible endpoints. Outside the API, the model is available in QwenStudio (the Qwen Chat interface), QwenWork, and the Qoder coding product, which Alibaba co-designed alongside the model.
Here is the minimal call using the OpenAI Python client against the compatible-mode Chat Completions endpoint. Thinking is on by default, so the

`enable_thinking`flag is shown only for explicitness; set it to`False`to switch thinking off.
If you want to try Live Delivery, the plugin is on GitHub as Qwen-Live-Delivery-Plugin. Because it currently targets Codex CLI, using it in another harness means wiring up its MCP delivery tools and hooks yourself. For the general patterns behind function calling and agent harnesses, our Harness² tutorial walks through wiring a model into OpenCode and grading the results.

## Final Thoughts

Qwen4-Max is Alibaba making a price argument more than a capability argument.

It wins the rows that involve reading a lot of documents and producing a deliverable, loses the rows that involve a terminal, a chart, or a video, and does all of it at roughly a tenth of GPT-6 Astra's output rate.

Would I switch? For document-heavy agent pipelines where cost per task is the constraint, it is worth a serious test, but not a blind migration.

The drop from $4.43 to $1.67 per Work360 task is Alibaba's own figure, and independent runs are more mixed: GPT-6.1 Sol was cheaper per task and scored slightly higher on Vals, and Qwen4-Max is slow and verbose. For hard coding work, I would stay with Opus 5.5 or Kimi K3.1, and Vals' independent Terminal-Bench run confirms that weakness.

If you want to get properly comfortable building with models like this, start with our AI Fundamentals skill track.
