---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-sol-explained-specs-costs-benchmarks
title: "GPT-6.1 Sol Explained: Specs, Costs & Benchmarks"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-17"]
keywords: ["gpt-6", "openai", "standard", "million", "tokens", "costs", "model", "input"]
source: https://www.layer3labs.io/guides/gpt-6-1-sol-explained
source_anchor: ""
source_lines: [1, 72]
section: "GPT_OpenAI"
---
# GPT-6.1 Sol Explained: Features, Pricing, and Benchmarks

OpenAI launched GPT-6.1 Sol to deliver frontier reasoning, computer use, and automated workflow execution at lower token costs.

On September 29, 2026, OpenAI introduced GPT-6.1 Sol, an upgraded reasoning and agentic model designed to offer near-frontier capability at a fraction of existing inference costs. The release updates the earlier GPT-6 Sol model by bringing software engineering, computer use, and document analysis performance close to OpenAI's flagship GPT-6 Astra. The model functions as a production-focused workhorse for multi-step agentic workflows and automated business operations.

Unlike the standard ChatGPT interface model or prior GPT-6 Sol checkpoints, GPT-6.1 Sol cuts operational costs while closing the performance gap with top-tier reasoning systems. On the DeepSWE v1.1 software engineering benchmark, it matches GPT-6 Astra at roughly one-fifth of the cost, while scoring 2.2 percentage points above Claude Opus 5.5 on AutomationBench 1.0.6 at medium reasoning effort. It also drops the prompt caching price to ten cents per million tokens, cutting cached input costs by half compared to the first GPT-6 Sol release.

For operators running automated workflows across regulated sectors like finance, legal, and healthcare, GPT-6.1 Sol shifts the unit economics of autonomous document and system tasks. Running repetitive, tool-heavy agent loops or parsing dense regulatory PDFs has historically pushed standard API bills beyond sensible operating limits. This model makes long-horizon tool execution, structured PDF extraction, and programmatic desktop automation financially practical for regular operational deployment.

## API Pricing and Token Economics for GPT-6.1 Sol

GPT-6.1 Sol reduces standard input and output token rates to one-fifth of OpenAI's flagship GPT-6 Astra tier. Standard input tokens cost $2.00 per million, while standard output tokens cost $10.00 per million. Cached input tokens cost $0.10 per million tokens, which represents a 95 percent discount compared to standard input pricing and a 50 percent cut compared to the initial GPT-6 Sol release. OpenAI released GPT-6.2 Sol on November 17, 2026, at the same $2.00 and $10.00 rates.

The model identifier for API implementation is gpt-6.1-sol. It is accessible across ChatGPT Work and Codex workspaces for Plus, Pro, Business, Enterprise, and Education tiers, though OpenAI has not made it available in standard ChatGPT Chat. OpenAI has also announced an upcoming variant named GPT-6.1 Sol Ultrafast, designed to deliver up to eight times faster token generation speeds within Codex.

OpenAI has not published official context window limits or maximum output token caps on the GPT-6.1 Sol release page. While the previous GPT-6 Sol release listed a 1.05 million token context window with a 128,000 token maximum output, teams should verify whether those same operational parameters apply to gpt-6.1-sol before sizing large prompt payloads.

OpenAI's cheaper GPT-6.1 model, GPT-6.1 Terra, costs $1 per million input tokens and $5 per million output tokens, half of Sol's standard rates.

- Standard Input: $2.00 per million tokens
- Cached Input: $0.10 per million tokens (95 percent discount on regular inputs)
- Standard Output: $10.00 per million tokens
- API Model Identifier: gpt-6.1-sol
- Supported Interfaces: ChatGPT Work, Codex, and OpenAI API (not standard ChatGPT Chat)

## Benchmark Evaluation across Code, Business Tools, and Documents

Benchmark figures released by OpenAI show that GPT-6.1 Sol approaches or matches top-tier models on practical technical evaluations while maintaining lower compute costs. In real-world software engineering, the model matches GPT-6 Astra on the DeepSWE v1.1 benchmark while beating the original GPT-6 Sol score by 6.4 percentage points at lower reasoning effort.

For business operations, OpenAI evaluated the system on AutomationBench 1.0.6, which measures end-to-end task execution using 47 specialized tools across sales, marketing, operations, customer support, finance, and human resources. GPT-6.1 Sol scored 2.2 percentage points higher than Claude Opus 5.5 at medium reasoning effort while running at approximately one-third of the operational cost. It gained 4.8 percentage points over GPT-6 Sol at identical settings.

Document comprehension tests using GDP.pdf evaluate how models handle complex layout files, including financial statements, clinical diagrams, legal disclosures, and fine print. GPT-6.1 Sol scored higher than Claude Opus 5.5 with fallbacks at less than half the per-task cost, approaching Astra-tier accuracy at roughly one-fifth of the price per completed document task.

## Desktop Computer Use and Scientific Analysis Capabilities

GPT-6.1 Sol incorporates native computer-use capabilities designed to navigate operating system interfaces, manage files, and interact with graphical desktop software. Tested on the OSWorld 2.0 offline evaluation set using partial reward criteria, GPT-6.1 Sol outperformed the original GPT-6 Sol by seven percentage points at maximum reasoning effort while cutting execution costs in half. The system finished within 2.1 percentage points of GPT-6 Astra at approximately one-seventh of the per-task cost.

Scientific data processing shows similar cost-to-performance efficiency gains. On the Terminal-Bench Science 0.1 evaluation covering theorem proving, simulation modeling, and data analysis, GPT-6.1 Sol more than doubled the score achieved by GPT-6 Sol at maximum reasoning effort. Its average task cost at maximum reasoning was $5.47, compared to $23.21 for Claude Opus 5.5 and $23.80 for GPT-6 Astra.

OpenAI still advises using GPT-6 Astra for frontier-grade research tasks. Astra retained the highest overall score on Terminal-Bench Science at 68.1 percent, indicating that the most complex autonomous mathematics and theoretical research still justify Astra's higher price point.

## Factuality Improvements and Alignment Safety Guardrails

OpenAI reports measurable reductions in factual hallucination rates, particularly when operating at lower reasoning effort settings. In benchmark evaluations composed of difficult, de-identified user conversations where an earlier model made an error, the percentage of responses with factual errors dropped from 11.4 percent on GPT-6 Sol to 7.7 percent on GPT-6.1 Sol, marking a 32 percent improvement. Across reasoning levels, its error rate remained within 1.9 points of GPT-6 Astra.

Agentic safety metrics indicate tighter guardrails when the model encounters broken environment tools or strict instruction limits. When exposed to adversarial tests with failing search tools, GPT-6.1 Sol failed to disclose the broken tool in only 2.1 percent of runs at maximum effort. By comparison, GPT-6 Sol failed in 4.9 percent of cases, GPT-6 Astra failed in 1.5 percent, and GPT-6 Luna failed in 28.7 percent.

The model made zero attempts to bypass automated safety reviewers during evaluation runs, matching the records of Astra and the initial GPT-6 Sol. OpenAI released an addendum to its system card covering the specific alignment profile and refusal behaviors of the gpt-6.1-sol model checkpoint.

## Workflow Fit and Operational Tradeoffs for Regulated Teams

Deciding between GPT-6.1 Sol and alternate frontier models comes down to task volume, reasoning latency, and fault tolerance. For high-volume multi-step agent pipelines where cached context can be maintained, GPT-6.1 Sol provides lower operating expenses than running continuous calls against GPT-6 Astra or Claude Opus 5.5. The ten-cent cached input tier makes multi-turn agent conversations feasible for small and mid-sized businesses. If you are deciding whether to move up a version, see GPT-6.2 Sol vs GPT-6.1 Sol.

In our client work automating legal matter intake and CRM document ingestion, we have seen teams abandon multi-step agentic workflows when token bills spiked unexpectedly during repetitive tool-calling routines. A model that cuts per-task automation costs by two-thirds without dropping tool-selection accuracy solves that adoption hurdle. When an agent must iterate across forty API calls to reconcile conflicting balance sheets, lower inference fees prevent cost overruns.

Teams should look elsewhere if their workflow relies exclusively on casual conversational interfaces or consumer chat setups. GPT-6.1 Sol is restricted to ChatGPT Work, Codex, and direct API endpoints, making it unavailable for ad-hoc chats in consumer ChatGPT accounts. Additionally, organizations requiring zero-shot frontier mathematical discovery should continue routing queries to Astra.

## Frequently Asked Questions

- GPT-6.1 Sol delivers higher accuracy across software engineering, business automation, and document extraction at lower cost. It beats GPT-6 Sol by 6.4 percentage points on DeepSWE v1.1, cuts prompt caching costs by 50 percent to $0.10 per million tokens, and lowers factual error rates by roughly 32 percent at low reasoning settings.
- The gpt-6.1-sol API costs $2.00 per million input tokens, $0.10 per million cached input tokens, and $10.00 per million output tokens. This matches roughly one-fifth of the pricing charged for OpenAI's flagship GPT-6 Astra model.
- No, OpenAI has not made GPT-6.1 Sol available in standard ChatGPT Chat. It is supported in ChatGPT Work and Codex workspaces for Plus, Pro, Business, Enterprise, and Education subscribers, as well as through direct developer API endpoints.
- On the OSWorld 2.0 offline benchmark, GPT-6.1 Sol comes within 2.1 percentage points of GPT-6 Astra at maximum reasoning effort. It achieves that performance at roughly one-seventh of Astra's task execution cost.
- OpenAI has not published context window or maximum output token specifications on the GPT-6.1 Sol release page. Organizations should check official OpenAI platform documentation to verify current production context boundaries before deployment.
- Organizations should select GPT-6 Astra for frontier-level scientific research and theorem proving where absolute accuracy outweighs operational cost. On Terminal-Bench Science, Astra achieved the top score of 68.1 percent, leading OpenAI to advise Astra for the most demanding scientific assignments.

### Plan Your GPT-6.1 Sol Implementation

Book a free 30-minute AI compliance review with Layer3 Labs to map token costs, secure system integrations, and evaluate agentic workflows for your business.

Book an AI Review
