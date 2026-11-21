---
id: collect-261121-veille-ia/kimi/moonshot-releases-kimi-k3-1-pro-weights-as-huawei-and-alibaba-add-deployment-support
title: "Moonshot Releases Kimi K3.1 Pro Weights as Huawei and Alibaba Add Deployment Support"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["model", "kimi", "moonshot", "weights", "through", "release", "training", "november"]
source: https://asiadaily.org/news/17790/
source_anchor: ""
source_lines: [1, 108]
section: "Kimi"
---
## A 2.8 trillion parameter model moves beyond the API

Moonshot AI has released the complete weights of Kimi K3.1 Pro on Hugging Face, giving developers and businesses a way to run its flagship multimodal model on their own computing infrastructure. The announcement came at dawn in Beijing on November 5, following an October 24, 2026 introduction that made the model available through Kimi products and its application programming interface, or API.

K3.1 Pro contains 2.8 trillion parameters, the numerical settings that store what a model has learned. Its mixture of experts design activates approximately 104 billion parameters for each inference step rather than using the entire model at once. It supports roughly one million tokens of context and can understand visual information alongside text.

Huawei Ascend and Alibaba Cloud both announced support on the day of the weights release. Their deployments use the vLLM and SGLang inference frameworks, software that manages how trained models process requests. For businesses considering local deployment, the release brings both downloadable weights and initial routes to running them on Chinese accelerator systems.

Moonshot's model launch announcement, updated October 24, said complete weights would be released by November 4; the public release announcement followed on November 5. The company had already opened access through kimi.com, Kimi App, Kimi Work, Kimi Code and Kimi API.

The practical change is control over where the model runs. An API customer sends requests to a provider's service. An organisation deploying the weights can operate the model within infrastructure it manages, subject to Moonshot's licence and the considerable hardware needed to serve it.

Moonshot also introduced Kimi Hosted Agents, a managed service for keeping AI agents running through lengthy tasks. Together, the announcements offer businesses two different choices: manage the model themselves or hand more of the operating work to Kimi.

## What is new, and what carries over from K3?

Kimi K3.1 Pro builds on the K3 base through additional training rather than starting with a newly trained foundation. Moonshot says it substantially increased computing resources for reinforcement learning, a process that rewards successful actions and helps a model improve through repeated attempts.

Moonshot applied that training across hundreds of task areas, with particular attention to coding, 3D creation, financial research and agents that work through many steps. Its engineering changes covered training software, long context processing, scheduling and sandbox environments where generated code can run.

The approximately one million token context window carries over from K3. Tokens are the pieces into which a model divides text and other inputs. A larger context window allows more documents, instructions and earlier conversation to remain available during a request.

The release sequence separates access to the service from access to the model itself:

- July 16, 2026: Kimi K3 was introduced, according to the published model comparison.
- October 24, 2026: Moonshot introduced K3.1 Pro through its products and API, with weights scheduled for release by November 4.
- November 5: Moonshot announced the weights release and Kimi Hosted Agents; Huawei and Alibaba announced deployment support.

## Open weights come with operating and licence conditions

The weights allow organisations with enough computing capacity to download and run K3.1 Pro independently. Businesses can configure their own serving systems, connect internal tools and pursue specialised adaptations under the applicable licence.

Moonshot uses a custom licence governing commercial use. The release covers model weights, while the training code and original training datasets remain unpublished. Developers therefore receive the trained model, with permissions defined by Moonshot's terms, rather than a complete training package under an Open Source Initiative definition.

The model's size remains a major operating consideration. Although only part of the mixture of experts network is active at each step, the deployment still needs access to the full collection of weights. Long requests also create large amounts of temporary attention data that must be stored while the model works.

## Huawei spreads the memory load across Ascend systems

Huawei says its Ascend team completed adaptation on the day K3.1 Pro weights became available, allowing immediate deployment validation. Support covers Atlas 800 A3, Atlas 900 A3 SuperPod and Ascend 950 supernode systems through vLLM Ascend and SGLang.

The adaptation addresses the model's hybrid attention architecture, including sparse attention and multimodal processing. Attention is the mechanism that helps a model decide which parts of its input matter for its next step. Sparse attention limits some of those connections to reduce the work required for very long inputs.

Ascend uses distributed caching to spread temporary model data across several neural processing units, or NPUs, reducing memory pressure on each accelerator. Huawei also describes changes to core mathematical operations that reduce unnecessary copying and computation.

Deployers can load the original weights or use lower bit precision for different model components, alongside attention cache quantisation. Quantisation stores numbers using fewer bits, lowering memory needs. Huawei says the implementation retains prefix caching, which reuses repeated input; chunked prefill, which processes long prompts in portions; and speculative inference, which accelerates generation by checking proposed tokens in groups.

## Alibaba adds another route through its SuperPoD platform

Alibaba Cloud says its Lingjun Zhenwu M890 SuperPoD completed adaptation and validation across the software stack on vLLM and SGLang. Support also covers Zhenwu 810E configurations, giving the model another deployment route on Chinese accelerators.

Alibaba Cloud, T-Head and the Kimi team worked on the T-Head SAIL software stack, including FlashMLA, DeepGEMM mixture of experts operations, MegaMoE and MoonEP. These components handle attention calculations and the movement of work among experts and accelerators. Customers can deploy through dedicated bare metal SuperPoD instances or Alibaba's REIN service, while API access is also offered through its Qwen AI platform.

Alibaba opened invitation testing for a SuperPoD region in Zhongwei, Ningxia, alongside launch day instances in Zhongwei and Ulanqab. The Zhongwei configuration adds T-Head's Panmai 920 network card, which Alibaba says supports PCIe 5.0, 112G PAM4 Ethernet, throughput of up to 400 gigabits per second and 400 million packets per second.

Those networking details matter because a model this large must move information quickly between processors. Alibaba says the card's integrated PCIe switch can reduce system cost by about 30%. It also says M890 SuperPoD serves more than 100 large customers and forecasts roughly three times current generation performance for its planned V900 successor; that figure is a company expectation.

## Coding results show gains in sustained execution

Moonshot reports a score of 65.7% on Terminal-Bench 4.0, which tests agents on terminal tasks in real computing environments under resource limits. Its published comparison puts GPT-6 Astra at 58.2% and Claude Opus 5.5 at 66.4%, leaving K3.1 Pro 0.7 percentage points behind the latter in that setup.

The company's internal Kimi Code Bench 3.0 Long-horizon score rose from 55.95% for K3 to 61.2% for K3.1 Pro, an improvement of 5.25 percentage points. That test focuses on planning, tool use and completing engineering problems through multiple steps.

Independent evaluations provide another view. Artificial Analysis recorded an Intelligence Index increase from 44 to 55 and Terminal-Bench results of approximately 67% for K3.1 Pro against 13% for K3. Its terminal evaluation used a different agent framework from Moonshot's, so the scores describe different operating setups.

Moonshot's demonstrations put the focus on persistence as well as code generation. In one internal exercise, the model worked on a walking simulation built from 296 skeletal components and 402 muscle components. Across 167 tasks and 420 independent reviews, it improved the simulated body's continuous walking time to 11.2 seconds and corrected 36 errors in shoulder muscle calculations, according to the company.

A separate Plotly evaluation reported 42 correct answers out of 43 for K3.1 Pro, compared with 35 for K3, alongside higher cost and a longer median completion time. Faster token generation and stronger answers can therefore coexist with a longer wait for a completed task.

## 3D creation and finance receive targeted improvements

K3.1 Pro scored 92.7% with tools on BenchCAD, according to Moonshot. The benchmark asks a model to reconstruct 3D objects from views of them by generating computer aided design code. The company's comparison lists 96.2% for Claude Opus 5.5 and 95.9% for GPT-6 Astra.

Moonshot says the model can write and run scripts for professional modelling tools, including Blender and CAD software. Its examples include a FreeCAD turbofan engine containing 2,100 solid objects and a lamp designed as five parts for 3D printing. Kimi's website and app also offer 3D Make for previewing, editing and exporting models.

Financial work is another central target. Moonshot reports a score of 64.4 on OfficeQA Pro v2, which tests analysis of real financial documents through retrieval, checks across documents and numerical reasoning. Its internal evaluations show financial modelling improving by 14%, financial research by 20% and simulated business operations reaching about 4.2 times K3's score.

The published comparison supplies the underlying figures for two of those tests: Financial Research rose from 58.8 to 70.4, while Business Operation increased from 8.9 to 37.0. For a business user, the intended benefit is a model that can carry a task from finding information through calculations to a finished workbook or research output.

## Moonshot publishes two different factual error results

Moonshot's launch announcement says the single factual claim error rate on Kimi Hallu Bench Hard fell from 23.4% for K3 to 8.3% for K3.1 Pro; its API guide gives a different stress test result, falling from 23.35% to 6.85% on anonymised conversations that users had flagged for factual mistakes.

The two pages also give different answer level figures. The launch announcement says the share of responses containing at least one factual error was about 42.4% of K3's rate, while the API guide gives about 46.4%. Moonshot has not explained the relationship between those evaluations.

Both descriptions concern difficult examples selected because models are prone to mistakes. The practical task for financial and business deployments remains checking calculations, citations and factual statements against the underlying records before acting on the output.

The independent comparison also recorded a small decline in one long context test: AA-LCR v1.1 fell from 88.7% for K3 to 86.3% for K3.1 Pro. Organisations processing lengthy documents will need to consider retrieval accuracy alongside the model's unchanged context capacity.

## Hosted agents and API access offer different tradeoffs

Kimi Hosted Agents supplies the operating framework, often called a harness, around an AI model. That framework manages tools, code execution, context, permissions and recovery when a task is interrupted. Moonshot says the service supports operation around the clock, lengthy tasks and collaboration among multiple agents.

Moonshot identifies internal monitoring and operational response as potential uses, alongside customer facing research assistants and support services. Businesses using the managed service define tasks and connect tools while Kimi maintains the underlying runtime. Businesses running the weights take responsibility for their own serving and operating arrangements.

For API customers, Moonshot's API quickstart guide requires a top up of at least 10 yuan to unlock K3.1 Pro. The 15 yuan voucher for newly registered users cannot be used for this model. The guide provides low, high and max reasoning settings, with max as the default, and cache retention choices of five minutes or one hour.

Moonshot also offers K3.1 Pro Highspeed, which it says delivers the same intelligence at roughly two to three times the output speed. The published comparison lists standard input and output rates of $2 and $10 per million tokens, down from $3 and $15 for K3, a reduction of one third before cache charges and other applicable fees. Total spending still depends on how many tokens and tool interactions a task consumes.

Developers face some specific integration requirements: complete assistant messages must be preserved across tool calls, visual inputs require base64 data or uploaded file identifiers rather than public image URLs, and the guide advises against production use of its web search tool while that tool is being updated.

## Key Points

- Moonshot announced Kimi K3.1 Pro weights on November 5 after earlier product and API access.
- The model has 2.8 trillion total parameters, approximately 104 billion active parameters per inference step and roughly one million tokens of context.
- Huawei Ascend and Alibaba Cloud announced deployment support through vLLM and SGLang.
- Moonshot reports gains in coding, 3D modelling and finance, with results varying by task and evaluation setup.
- Independent deployment requires substantial computing resources and compliance with Moonshot's custom licence.
- Kimi Hosted Agents offers a managed alternative for running lengthy tasks and coordinating multiple agents.
