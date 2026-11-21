---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-moonshot-ai-s-3t-class-open-weight-model-benchmarks-pricing-and-cyber-safeguards
title: "Kimi K3.1 Pro: Moonshot AI's 3T-Class Open-Weight Model, Benchmarks, Pricing and Cyber Safeguards"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-10-23"]
keywords: ["kimi", "model", "moonshot", "t-class", "open-weight", "tokens", "cyber", "attention"]
source: https://www.datastudios.org/post/moonshot-ai-kimi-k3-1-pro-open-weight-1m-context-cyber-safeguards
source_anchor: ""
source_lines: [1, 415]
section: "Kimi"
---
Moonshot AI launches Kimi K3.1 Pro, a 3T-class open-weight model with 1M-token context and deliberately reduced cyber capabilities

Oct 25

11 min read

Updated: Oct 28

Moonshot AI has released Kimi K3.1 Pro, the first model in its new K3.1 series, describing it as its most capable 3T-class open-weight model to date. Announced on October 23, 2026, the model is a post-trained version of Kimi K3 that keeps the 1-million-token context window of its predecessor while applying substantially larger post-training compute, with the company reporting gains in long-horizon coding, financial research, office document work and 3D creation.

The release is accompanied by Kimi K3.1 Pro Highspeed, a variant that Moonshot says runs at two to three times the speed of the standard model. Both are available immediately through Kimi on the web, the Kimi mobile apps, the Kimi Work desktop application, Kimi Code and the Kimi API, under the model identifiers kimi-k3.1-pro and kimi-k3.1-pro-highspeed.

API pricing for the standard model is $2.00 per million input tokens on a cache miss, $0.20 per million cached input tokens and $10.00 per million output tokens, while the Highspeed variant doubles the input and output rates. Moonshot has not yet published the weights, but states that the full model weights will be released by November 4, 2026.

The most unusual element of the announcement concerns security. Moonshot says it removed cyber-related content from both the mid-training corpus and the reinforcement learning environments used for the open-weight release, and it reports that this reduced the model's success rate on an internal exploit-development benchmark from 90.5% to 81.0%. The company presents this as a deliberate decision not to release open weights that extend the frontier of offensive cyber capability.

··········


KIMI K3.1 PRO IS A POST-TRAINED VERSION OF K3 RATHER THAN A NEW ARCHITECTURE.

Moonshot describes K3.1 Pro as a post-trained Kimi K3, which means the underlying network is inherited from the model the company introduced in July 2026 rather than rebuilt from scratch. The improvements come from what happens after pretraining: additional mid-training, a larger reinforcement learning budget and a broader set of training environments.

The K3.1 Pro announcement itself does not repeat the architectural specifications, referring only to a 3T-class model. Moonshot's earlier technical description of Kimi K3 provides the relevant baseline: a 2.8-trillion-parameter Mixture-of-Experts model that activates 16 out of 896 experts per token within what the company calls a Stable LatentMoE framework, built on Kimi Delta Attention and Attention Residuals, with Gated MLA shown in its architecture diagram.

Moonshot has not disclosed the number of active parameters, the layer count or the size of the training corpus for either K3 or K3.1 Pro. The 1-million-token context window is confirmed in the K3.1 Pro announcement and in the API documentation, which lists a window of 1,048,576 tokens for K3-series models.

........

Specification

Kimi K3.1 Pro

Developer

Moonshot AI

Announcement date

October 23, 2026

Base model

Kimi K3, post-trained with larger RL compute

Model class

3T-class open-weight model

Base architecture (K3)

2.8T-parameter MoE, 16 of 896 experts active

Attention (K3)

Kimi Delta Attention, Attention Residuals, Gated MLA

Active parameters

Not disclosed

Context window

1,048,576 tokens

Maximum output

131,072 tokens by default, configurable up to 1,048,576

Inputs

Text, images and video files

Reasoning

Always on, with low, high and max effort levels

Open weights

Promised by November 4, 2026

........

··········


LARGE-SCALE JOINT REINFORCEMENT LEARNING ACROSS HUNDREDS OF DOMAINS DRIVES THE CAPABILITY GAINS.

Moonshot attributes most of the improvement to large-scale joint reinforcement learning across hundreds of downstream domains, rather than to separate fine-tuning runs for individual capabilities. Training a single policy across many domains at once is computationally demanding, because each domain requires its own environments, reward signals and rollout infrastructure, but it reduces the risk that gains in one area are achieved at the expense of another.

To make this practical, the company says it reworked several layers of its training stack, including the RL framework itself, long-context inference, overall training efficiency, the sandbox infrastructure in which agents execute code, rollout scheduling and a shared platform for checking the quality of training environments. The stated objective is that each additional increment of RL compute should yield stable and predictable returns, which is a claim about the scalability of the training process rather than a measurable property of the released model.

The practical demonstrations in the announcement focus on long-horizon agentic work. In one internal test, K3.1 Pro built a human walking simulator on top of an anatomical model containing 296 skeletal and 402 muscle components, working through 167 missions and 420 review cycles until the simulated figure progressed from no stable gait to 11.2 seconds of continuous walking. Moonshot also reports that the model can write and execute scripts for Blender and CAD software without dedicated MCP connectors, and can work with Unity, Unreal Engine and Godot in combination with web search and 3D generation.

These examples illustrate the kind of multi-step execution, tool use and error diagnosis the training targeted, although they are single internal demonstrations rather than controlled evaluations, and they do not establish how reliably similar results can be reproduced.

··········

[[ADPLUS_300x250_1]]

CODING AND TERMINAL BENCHMARKS PLACE K3.1 PRO CLOSE TO CLAUDE OPUS 5.5.

On Terminal-Bench 4.0, which measures an agent's ability to complete tasks inside a command-line environment, Moonshot reports a score of 65.7% for K3.1 Pro, compared with 66.4% for Claude Opus 5.5 and 58.2% for GPT-6 Astra. If the figures were obtained under comparable conditions, the open-weight model sits less than one percentage point behind Anthropic's model on this test and more than seven points ahead of OpenAI's.

On Moonshot's own Kimi Code Bench 3.0 long-horizon evaluation, K3.1 Pro scores 61.2%, up from 55.95% for Kimi K3. The company also reports results on BenchCAD, which evaluates the generation of 3D CAD geometry and is scored by voxel intersection-over-union, using 1,000-case subsets drawn from a 17,900-case dataset.

The BenchCAD results are more mixed. With tools, K3.1 Pro reaches 92.7, essentially level with Claude Fable 5.1 at 92.6 but behind Claude Opus 5.5 at 96.2 and GPT-6 Astra at 95.9. Without tools, it scores 72.8 against 73.0 for Opus 5.5 and 60.6 for Fable 5.1. In both settings it is well ahead of DeepSeek-V4.1-Flash-1017, which scores 81.6 and 41.2 respectively.

Unless otherwise noted, Moonshot ran its evaluations at maximum reasoning effort, with a temperature of 1.0 and top-p of 0.95. Most coding comparisons used Claude Code as the harness for all models, while some Kimi results were obtained with Kimi Code, which means the agent scaffolding is not identical across every reported figure.

Opus 5.5: 73.0; Fable 5.1: 60.6; DeepSeek-V4.1-Flash-1017: 41.2

........

··········

FINANCIAL AND BUSINESS WORKFLOWS SHOW THE LARGEST RELATIVE IMPROVEMENTS OVER K3.

The largest gains reported by Moonshot are in financial and business tasks. On internal evaluations, Financial Modeling rises from 50.4 to 57.6 and Financial Research from 58.8 to 70.4, while Business Operation increases from 8.9 to 37, a gain of roughly 4.2 times that suggests K3 performed poorly on this category rather than that K3.1 Pro has reached a high absolute level.

On external evaluations, K3.1 Pro scores 58.6 on Frontier Finance and 64.4 on OfficeQA Pro v2, a benchmark in which PDF documents are rendered as images, which Moonshot describes as a state-of-the-art result. The announcement compares the Frontier Finance score with Claude, GPT, GLM and DeepSeek models in a chart, but the competing values are not provided in the text of the publication.

Because the three largest improvements come from Moonshot's internal benchmarks, they indicate the direction of the training effort more reliably than they indicate the model's position relative to competitors. Organizations considering the model for financial analysis would need to evaluate it on their own document types, data sources and accuracy requirements.

........

Evaluation

Kimi K3

Kimi K3.1 Pro

Change

Internal Financial Modeling

50.4

57.6

+7.2

Internal Financial Research

58.8

70.4

+11.6

Internal Business Operation

8.9

37

~4.2x

Frontier Finance

—

58.6

Not reported for K3

OfficeQA Pro v2

—

64.4

Claimed state of the art

........

··········

MOONSHOT REPORTS A SHARP REDUCTION IN ADVERSARIAL HALLUCINATION RATES.

Moonshot also reports substantial progress on factual reliability, measured with its own Kimi Hallu Bench. In the hard setting with tools enabled, the claim-level hallucination rate falls from 23.4% for Kimi K3 to 8.3% for K3.1 Pro, a relative reduction of approximately 65%. The company adds that the share of responses containing at least one factual error is 42.4% of K3's rate.

In the same tool-enabled setting, GPT-6 Astra records a lower rate of 5.5%, while Claude Opus 5.5 is reported at 15.0%, GLM-5.3 at 22.6% and both DeepSeek and Claude Fable 5.1 at 23.3%. Without tools, K3.1 Pro reaches 5.4%, behind GPT-6 Astra at 4.2% but ahead of Opus 5.5 at 6.8%, Fable 5.1 at 11.4%, DeepSeek at 13.1% and GLM-5.3 at 14.8%.

Moonshot explicitly describes the benchmark as a deliberately adversarial stress test and states that the results do not represent overall error rates in everyday use. The figures are therefore most useful as a relative comparison between models on difficult factual prompts, not as an estimate of how often the model will be wrong in ordinary conversations.

........

Model

Hallu Bench Hard, with tools

Hallu Bench, without tools

Kimi K3.1 Pro

8.3%

5.4%

Kimi K3

23.4%

Not reported

GPT-6 Astra

5.5%

4.2%

Claude Opus 5.5

15.0%

6.8%

Claude Fable 5.1

23.3%

11.4%

GLM-5.3

22.6%

14.8%

DeepSeek

23.3%

13.1%

........

··········

CYBER-RELATED TRAINING DATA WAS REMOVED BEFORE THE OPEN-WEIGHT RELEASE.

The security decision is the most consequential part of the release for the open-weight ecosystem. Moonshot says it removed cyber-related content from the mid-training corpus and from the RL environment mix used for the open-weight model, while retaining general-purpose coding tasks so that ordinary software engineering ability would not be lost.

To measure the effect, the company used xdev-bench-hard, an expert-curated benchmark that reproduces real vulnerabilities in the Linux kernel, Node.js, JavaScriptCore, Exim and VirGL with standard mitigations enabled. To succeed, an agent must achieve arbitrary code execution inside a separate sandbox, and the evaluation imposes no hard timeouts or output token limits. Results are reported as pass@3.

A checkpoint trained without the mitigations solved 90.5% of the tasks, while the released K3.1 Pro solved 81.0%, a reduction of 9.5 percentage points, or about 10.5% in relative terms. The released model nevertheless remains far more capable on this benchmark than Kimi K3, which solved 33.3%, and GLM-5.3, which solved 57.1%, while DeepSeek-V4.1-Flash, another open-weight model, is reported at 85.7%.

The numbers show that the intervention reduced but did not remove offensive capability. A model that achieves arbitrary code execution on four out of five expert-curated exploit tasks remains highly capable in this domain, and Moonshot's own figures place it below an existing open-weight competitor rather than below the general open-weight level.

Moonshot frames its policy in terms of the proprietary frontier: it says the released capability remains behind that of frontier proprietary models that have been publicly available for at least three months, that it aims not to release weights that extend the absolute cyber frontier, and that it intends to progressively raise cyber capability in open weights over time. Once the weights are published, these safeguards depend entirely on what was removed during training, because hosted usage policies cannot be enforced on locally deployed copies.

........

Model

xdev-bench-hard (pass@3)

K3.1 Pro checkpoint without cyber mitigations

90.5%

DeepSeek-V4.1-Flash

85.7%

Kimi K3.1 Pro (released)

81.0%

GLM-5.3

57.1%

Kimi K3

33.3%

........

··········

THE HIGHSPEED VARIANT DOUBLES API PRICES IN EXCHANGE FOR TWO TO THREE TIMES THE SPEED.

Moonshot prices both models on a flat pay-as-you-go basis with no tiering by context length, which matters for a model with a 1-million-token window, since very long prompts are not charged at a premium rate. Context caching is enabled automatically with a default time-to-live of five minutes, and a one-hour option is available at a higher cache-write price.

The standard model costs $2.00 per million input tokens on a cache miss and $10.00 per million output tokens. K3.1 Pro Highspeed costs $4.00 and $20.00 respectively, while cached input remains at $0.20 per million tokens for both models. The rates exclude applicable taxes and were listed by Moonshot as checked on October 24, 2026.

........

Price per 1M tokens

kimi-k3.1-pro

kimi-k3.1-pro-highspeed

Input, cache miss

$2.00

$4.00

Input, cache hit

$0.20

$0.20

Cache write, 5-minute TTL

$2.00

$4.00

Cache write, 1-hour TTL

$4.00

$8.00

Output

$10.00

$20.00

........

··········

THE API EXPOSES ALWAYS-ON REASONING THROUGH AN OPENAI-COMPATIBLE INTERFACE.

According to Moonshot's API documentation, reasoning is always enabled for K3.1 Pro and is controlled through a reasoning_effort parameter that accepts low, high or max, with max as the default. Several sampling parameters are fixed and should be omitted from requests, including a temperature of 1.0, top-p of 0.95 and n of 1.

The API supports custom tool calling with tool_choice and dynamic tool loading, structured output through json_schema with strict mode, streaming with separate reasoning and answer content, partial mode, and image and video inputs supplied as base64 data or uploaded file references rather than public URLs. Access requires a minimum top-up of $1, and rate limits are determined by cumulative top-up tier.

The documentation shows the model being called through the OpenAI Python SDK pointed at Moonshot's endpoint:

import os
from openai import OpenAI
client = OpenAI(
api_key=os.environ["MOONSHOT_API_KEY"],
base_url="https://api.moonshot.ai/v1",
)
completion = client.chat.completions.create(
model="kimi-k3.1-pro",
messages=[{"role": "user", "content": "Introduce Kimi K3.1 Pro in one sentence."}],
)
print(completion.choices[0].message.content)

Because the interface follows the OpenAI chat completions format, existing applications can in principle switch to K3.1 Pro by changing the base URL, API key and model identifier, although the fixed sampling parameters and always-on reasoning mean that request configurations designed for other models may need adjustment.

··········

THE MODEL IS AVAILABLE ACROSS KIMI'S CONSUMER, DEVELOPER AND ENTERPRISE PRODUCTS.

K3.1 Pro is available on the free Kimi plan with limited usage on the web and in Kimi Work, while Kimi Code and the Highspeed variant require a paid membership. Moonshot lists paid tiers at an effective monthly price of $15 for Plus, $31 for Pro, $79 for Max and $159 for Ultra on annual billing, with Plus adding Kimi Code and the Pro, Max and Ultra tiers adding K3.1 Pro Highspeed across the web, Kimi Work and Kimi Code.

For organizations, Kimi Business administrators must enable the model manually, since it is turned off by default at launch. API customers can request Zero Data Retention. Membership does not replace API billing, which remains separate and usage-based.

........

Channel

Availability

Kimi web and mobile apps (iOS, Android, HarmonyOS)

Available, limited usage on the free plan

Kimi Work desktop (3.1.0+, Windows and Apple silicon)

Available

Kimi Code

Available on paid plans, selectable with the /model command

Kimi API

kimi-k3.1-pro and kimi-k3.1-pro-highspeed, Zero Data Retention available

Kimi Business

Off by default, must be enabled by administrators

Open weights

Promised by November 4, 2026

........

··········

OPEN WEIGHTS ARE PROMISED BY NOVEMBER 4, BUT LICENSING AND DEPLOYMENT DETAILS ARE NOT YET PUBLISHED.

Moonshot has committed to releasing the full model weights by November 4, 2026, consistent with its approach for Kimi K3, whose weights were promised within roughly ten days of that model's announcement. The K3.1 Pro announcement does not specify the license that will apply, which will determine whether commercial use, modification and redistribution are permitted and under what conditions.

The company has also not published hardware requirements, quantization formats or recommended serving configurations. For a model in the 3-trillion-parameter class, self-hosting will require multi-GPU or multi-node infrastructure even with aggressive quantization, so for most organizations the open weights are likely to matter primarily through third-party hosting providers, fine-tuning and research access rather than direct local deployment.

··········

THE BENCHMARK COMPARISONS REQUIRE CAREFUL INTERPRETATION.

All of the reported results are vendor-published figures. Several headline comparisons, including Terminal-Bench 4.0, set Moonshot's own results against figures for proprietary models that may have been obtained with different harnesses, and the company notes that comparisons with proprietary models may involve some fallback behavior.

Moonshot does apply some controls that strengthen its methodology: runs in which answers were retrieved from online sources were scored as zero on several agentic benchmarks, and the evaluation settings, judge models and task subsets are disclosed for many tests. The publication also contains minor inconsistencies, such as different OpenAI model names used in the cyber chart and in its footnotes, which illustrates why independent reproduction will be important once the weights are available.

··········

KIMI K3.1 PRO NARROWS THE GAP WITH PROPRIETARY MODELS WHILE TESTING A NEW APPROACH TO OPEN-WEIGHT CYBER RISK.

Kimi K3.1 Pro shows how much capability can be added to an existing frontier-scale base model through post-training alone. Without changing the K3 architecture, Moonshot reports results within a point of Claude Opus 5.5 on Terminal-Bench 4.0, a hallucination rate on its adversarial benchmark that is roughly a third of K3's, and large gains on financial and business tasks, all at API prices well below those typical of proprietary frontier models.

The cyber mitigation is the more significant precedent. By removing offensive security data before release and publishing the measured effect, Moonshot has made its risk trade-off quantifiable, although the result shows a moderate reduction rather than a neutralized capability. The principal open questions are the license that will govern the weights, the hardware needed to serve them, and whether independent evaluations confirm the coding, hallucination and cyber figures once the model can be tested outside Moonshot's own infrastructure.
