---
id: collect-261121-veille-ia/qwen/alibabas-qwen4-max-cuts-ai-task-costs-but-benchmarks-show-an-uneven-challenge-to-frontier-rivals
title: "Alibaba’s Qwen4-Max cuts AI task costs, but benchmarks show an uneven challenge to frontier rivals"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen4-max", "model", "alibabas", "tokens", "task", "alibaba", "million", "work360"]
source: https://venturebeat.com/technology/alibabas-qwen4-max-cuts-ai-task-costs-but-benchmarks-show-an-uneven-challenge-to-frontier-rivals
source_anchor: ""
source_lines: [1, 120]
section: "Qwen"
---
Alibaba is making a practical pitch for its latest flagship AI model: businesses should be able to get more work done for less money, even if the model does not lead the overall intelligence rankings.

The company’s Qwen4-Max announcement introduces a model that accepts text, images, and video, works with external software, and accommodates up to one million tokens of context. Alibaba says it improves on Qwen3.8-Max while reducing the average cost of a task in its internal Work360 evaluation from $4.43 to $1.67, approximately 62%.

Qwen4-Max launches as a proprietary model, not an open-source or open-weights release. Artificial Analysis classifies it as proprietary, and Alibaba has not provided downloadable weights that businesses could run on their own infrastructure. Customers can access it through the QwenCloud API, or use it in Qwen Studio, QwenWork, and Qoder.

That is a consequential distinction for a model family known for releasing models developers can download and adapt. Alibaba’s launch announcement promises open releases of smaller Qwen4 models, but gives no dates or licenses and makes no equivalent commitment for Max. As of publication, we found no public commitment to release Qwen4-Max’s weights—or a definitive statement that they will remain closed permanently. The flagship is proprietary today; the announced open-model roadmap concerns smaller versions.

The API charges $1.50 per million input tokens and $4.50 per million output tokens. Those rates undercut several prominent alternatives from Anthropic, OpenAI, and Google, although token prices alone do not determine what a completed assignment costs.

The performance picture is more complicated. Qwen’s launch chart shows competitive results on several professional and extended-work benchmarks, alongside substantial deficits on other coding and visual tasks. An independent aggregate evaluation also places the model behind leading American competitors and Moonshot AI’s Kimi K3.1 Pro.

For enterprise buyers, Qwen4-Max presents a workload-specific proposition: potentially attractive economics and useful capabilities, with consequential differences in accuracy, speed, and deployment options.

## **Lower prices and fewer tokens drive the efficiency pitch**

Alibaba’s Work360 framework organizes professional work across 26 broad domains, 182 subdomains, and 721 specialized fields. Its examples include accounting reviews, manufacturing analysis, legal assignments, and document production.

Qwen4-Max scores 52.69 on the company’s benchmark, compared with 47.74 for Qwen3.8-Max. That is an increase of 4.95 points, or approximately 10.4% relative to the earlier score.

Alibaba reports that token consumption falls approximately 24% at the Xhigh reasoning setting and 35% at High on Work360. On Terminal-Bench 4.0, the corresponding reductions approach 48% and 49%.

These are distinct findings. The roughly 50% token reduction comes from the terminal test; the 62% cost reduction comes from Work360 and reflects both token usage and pricing. Neither establishes a universal savings rate for customer workloads.

There is independent evidence of improved economics, too. Artificial Analysis’s Qwen4-Max evaluation reports $3.27 per Intelligence Index task. Its Kimi K3.1 Pro evaluation reports $3.68, making Qwen approximately 11% cheaper on that measure. These figures come from a different test suite than Alibaba’s $1.67 result and should not be treated as interchangeable.

## **How API prices compare**

The following prices cover standard API usage in U.S. dollars per million tokens. Qwen’s figures come from its Singapore-region listing; Google’s are global rates. Batch discounts, tool charges, regional premiums, and enterprise agreements can change the comparison.

| **Model and pricing source** | **Input** | **Output** | **Cached input** | **Important qualification** | 
|---|---|---|---|---|
| Qwen4-Max | $1.50 | $4.50 | $0.075 | Cached rate applies to qualifying cache hits | 
| Claude Opus 5.5 | $4.00 | $20.00 | $0.20 | Standard Claude API rates | 
| Claude Sonnet 5.5 | $2.00 | $10.00 | $0.10 | Standard global rates | 
| GPT-6.1 Sol | $2.00 | $10.00 | $0.10 | Above 272K input tokens, input/cache rates double and output rises to $15 | 
| Gemini 4 Carbon | $2.00 | $10.00 | $0.10 | Introductory pricing through November 30; then $4/$20/$0.20 | 
| Gemini 4 Hydrogen | $1.50 | $7.50 | $0.075 | Standard global rates | 
| Kimi K3.1 Pro | $2.00 | $10.00 | $0.20 | Rates reported by Artificial Analysis | 

Qwen’s output rate is 55% below the $10 charged for standard Sol, Sonnet, and introductory Carbon usage, and 77.5% below Opus 5.5. Its input rate matches Hydrogen’s, while output costs 40% less.

But Qwen4-Max is not the least expensive option for every application. Anthropic’s rate card lists Haiku 5.5 at $0.10 input and $0.50 output for prompts up to 100,000 tokens, rising to $0.50 and $2.50 above that threshold. Buyers still need to determine which model can complete their work reliably.

Qwen also charges separately for explicit cache creation: $1.875 per million tokens for a five-minute cache and $3 for a one-hour cache, with reads at $0.075. The low cached-input figure should not be applied to every token in a request.

Its separate Token Plan subscription advertises promotional entry pricing from $3 for new customers. That subscription offer, and a separately listed discount for qwen4-max-preview, should not be confused with standard qwen4-max API pricing.

## **Agent benchmarks show strengths—and sharp limits**

An agent must do more than produce a plausible response. It needs to operate tools, interpret results, revise its approach, and deliver something usable. Qwen’s launch comparison tests several parts of that process, with divergent results.

| **Benchmark** | **Qwen4-Max** | **Qwen3.8-Max** | **Opus 5.5** | **GPT-6.1 Sol** | **Kimi K3.1** | 
|---|---|---|---|---|---|
| DeepSWE v1.1: coding agents | 75.7 | 69.3 | 74.2 | 75.2 | 75.5 | 
| Terminal-Bench 4.0: terminal work | 48.5 | 38.9 | 59.6 | 56.1 | 67.2 | 
| ProgramBench: software replication | 31.5 | 28.0 | 65.0 | 52.5 | 25.0 | 
| APEX-Agents v1.1: professional services | 72.1 | 63.3 | 73.5 | 60.0 | 65.5 | 
| AutomationBench-AA: software workflows | 72.5 | 56.2 | 69.5 | 64.9 | 73.3 | 
| ChartoGraphy: chart interpretation | 54.2 | 42.0 | 66.3 | 67.8 | 50.0 | 

These figures reproduce Qwen’s chart, including its Kimi label. They are not independently rerun results. Alibaba’s footnotes attribute some scores to outside evaluators, including Mercor and Artificial Analysis, and describe specific settings or dataset versions for others.

Qwen narrowly leads the displayed DeepSWE results and approaches Opus on APEX-Agents. It also scores 46.7 on Agents’ Last Exam v1.1, versus 27.9 for its predecessor and 43.4 for Opus.

Yet its Terminal-Bench result trails Kimi substantially, and its ProgramBench score is less than half Opus’s. A buyer evaluating an autonomous software-maintenance system should not substitute one favorable coding score for evidence across the operations that system must perform.

The chart-reading gap similarly matters for finance and document-analysis workflows.

## **Overall intelligence remains behind the leaders**

The Artificial Analysis launch chart places Qwen4-Max at 43 on its Intelligence Index, up from 34 for Qwen3.8-Max. Kimi K3.1 Pro scores 46; GPT-6.1 Sol and Claude Sonnet 5.5 score 55; Opus 5.5 reaches 59; Gemini 4 Carbon scores 60; and Claude Fable 5.2 leads this displayed group at 65.

The figures represent the reasoning configurations shown in the chart, rather than necessarily each model’s default setting.

That composite picture qualifies the strongest interpretation of Alibaba’s selected task results. Qwen can compete closely on particular assignments while remaining well behind on an aggregate measure.

It's also a bit slow: At the time of review, Artificial Analysis’s model page reports roughly 32 minutes per Intelligence Index task and approximately 159 seconds to the first answer token, on the more time-consuming end of major foundation models.

These measurements concern its demanding evaluation workload and tested configuration and don't indicate ho wit will perform on every response. They nevertheless caution against equating lower cost with faster delivery.

## **Multimodal input, but not native media generation**

The QwenCloud specification lists text, image, and video inputs and text output, not audio input or native image, audio, or video output.

That distinction clarifies the launch demonstrations. Qwen uses Blender for 3D work, game engines for interactive software, and WonderClip in a video-production workflow. Producing a film through software tools is different from a model directly generating video.

Competitors have different boundaries. Opus 5.5 accepts text and images and returns text. GPT-6.1 Sol also lists text and image input, with audio and video unsupported as native modalities, although it can invoke tools such as image generation. Google’s Carbon and Hydrogen listings include text, image, video, and audio input, with text output. Artificial Analysis lists Kimi K3.1 Pro as accepting text and images.

For applications centered on recorded speech, Qwen’s published specifications therefore require a different integration approach than Google’s audio-capable models. For visual inspection or video analysis, Qwen provides a direct input option, but the relevant accuracy and processing limits still need testing.

## **A larger architecture aims to reduce active computation**

Alibaba describes a 2.4-trillion-parameter backbone, with 71 billion parameters activated for each token and another 328 billion parameters devoted to local token-pattern representations. The 2.4 trillion figure is therefore not a complete count including that additional component.

Its mixture-of-experts design activates only part of the network at a time. A combination of compressed history and selective attention aims to make long inputs more economical, while revised information pathways support training stability.

Alibaba says the model activates approximately 25% fewer parameters than Qwen3.8-Max. Its pretraining chart also reports improved prediction scores across knowledge, code, question answering, and STEM. Those measurements describe prediction performance, not completed-task success, and Alibaba notes that the comparison mixes base and post-trained checkpoints.

The practical question is whether those architectural changes produce reliable work at an acceptable cost. Parameter counts alone cannot answer it.

## **Reactions expose different definitions of a successful release**

The launch has prompted criticism of the gap between selected benchmark results and broader rankings. In a post shown in a screenshot reviewed for this article, the X account Chubby, @kimmonismus, writes: “Qwen 4 is not a good release.”

The account argues that the Artificial Analysis result undermines apparent parity with Opus and speculates that restrictions on distillation explain the shortfall. The supplied post offers no evidence establishing that causal explanation; benchmark scores alone cannot establish how a model’s training access affected its performance.

The independent cost measurements support a more qualified assessment. Qwen trails Kimi on aggregate intelligence while costing less per evaluated task. That can matter to a business processing large workloads, even if it doesn't establish frontier leadership.

## Availability, open weights, and the enterprise decision

Qwen4-Max is available through QwenCloud, Qwen Studio, QwenWork, and Qoder. Its API listing includes tool calling, structured responses, web search, batch processing, and compatibility with OpenAI and Anthropic API formats.

Alibaba also describes a Live Delivery workflow that exposes usable intermediate results so users can redirect an agent before it finishes. The company says it is releasing the associated plugin as open source.

But unlike many of its predecessors, Qwen4-Max isn't an open-weights model. Alibaba promises smaller Qwen4 releases with downloadable weights, but the announcement supplies neither their licenses nor a release schedule. Organizations seeking to operate the flagship on their own infrastructure cannot treat that future commitment as present availability.

The manufacturing and scientific demonstrations are ambitious. Alibaba reports reducing manual programming effort for one machining workpiece from roughly 475 minutes to 24, and describes a 48-hour materials-research run that produced a physical coolant sample. These are company-reported demonstrations, not evidence that comparable savings or discoveries will recur across customer deployments.

Qwen4-Max’s clearest enterprise opportunity is measurable: test whether its lower rates and improved execution reduce the cost of producing an acceptable result. That calculation should include retries, review time, external tools, and latency. Its strongest benchmark results justify evaluation; its weaker results make workload-specific validation essential.
