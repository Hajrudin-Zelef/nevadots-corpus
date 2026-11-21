---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-terra-explained-1-5-pricing-and-benchmarks
title: "GPT-6.1 Terra Explained: $1/$5 Pricing and Benchmarks"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "terra", "openai", "input", "million", "tokens", "output", "gpt-5"]
source: https://www.layer3labs.io/guides/gpt-6-1-terra-explained
source_anchor: ""
source_lines: [1, 87]
section: "GPT_OpenAI"
---
# GPT-6.1 Terra Explained

OpenAI's mid-priced GPT-6.1 model costs half as much as Sol per token and targets computer use and office work.

OpenAI launched GPT-6.1 Terra on October 26, 2026, as a fast, mid-priced model for visual reasoning, computer use and office writing. The release updates OpenAI's mid-tier catalog after Terra was left out of the September GPT-6 release. It sits directly between the budget GPT-6 Luna checkpoint and GPT-6.1 Sol in OpenAI's four-tier catalog.

Standard application programming interface (API) rates are $1.00 per million input tokens and $5.00 per million output tokens. OpenAI says that is 50 percent below GPT-6.1 Sol and GPT-5.6 Terra. Against GPT-5.6 Terra's $12.00 output rate, the output cut is 58 percent. OpenAI reports that GPT-6.1 Terra runs more than three times faster than GPT-6.1 Sol and GPT-6 Astra. For the most complex coding tasks, OpenAI says GPT-6.1 Sol and GPT-6 Astra remain the better choice.

## What GPT-6.1 Terra Is Good At

GPT-6.1 Terra is OpenAI's mid-priced model designed specifically for computer use, visual reasoning, and office writing. It handles routine operational workloads at half the token cost of GPT-6.1 Sol, while OpenAI continues to recommend GPT-6.1 Sol or GPT-6 Astra for the most complex coding assignments.

In its launch announcement, OpenAI calls GPT-6.1 Terra a significant step up from GPT-5.6 Terra. Its example tasks are drafting proposals, interpreting technical drawings, filling out forms, editing videos, and reading incoming invoices to enter their details into accounting software.

On Terminal-Bench 4.0, GPT-6.1 Terra solves 50.3 percent of command-line coding tasks at max effort, against 21.5 percent for GPT-5.6 Terra. Artificial Analysis's Coding Agent Index v2.0 rates GPT-6.1 Terra 48 and GPT-6.1 Sol 54.

## GPT-6.1 Terra Price and Place in the Lineup

OpenAI structures its model family across four capability tiers that rise in price and reasoning depth: Luna, Terra, Sol, and Astra. As reported by 9to5Mac, Terra was omitted during the September GPT-6 release and has now been updated from the GPT-5.6 generation to GPT-6.1.

API pricing listed on the OpenAI pricing documentation sets standard input tokens for gpt-6.1-terra at $1.00 per million, cached input tokens at $0.05 per million, and output tokens at $5.00 per million. OpenAI says these rates are 50 percent below GPT-6.1 Sol and GPT-5.6 Terra. GPT-5.6 Terra charges $2.00 input and $12.00 output after its July 30, 2026 cut. GPT-6.1 Terra's $5.00 output rate is 58 percent lower. When input contexts exceed 272,000 tokens, OpenAI applies a long-context step that raises Terra's input rate to $2.00 per million, cached inputs to $0.10 per million, and outputs to $7.50 per million.

Within ChatGPT Work and Codex subscriptions, an identical allowance covers more tasks on GPT-6.1 Terra than on GPT-5.6 Terra. OpenAI notes it covers substantially more than on GPT-6.1 Sol or GPT-6 Astra.

- GPT-6 Luna: $0.10 input, $0.01 cached input, $0.50 output per million tokens (long context: $0.20 / $0.02 / $0.75)
- GPT-6.1 Terra: $1.00 input, $0.05 cached input, $5.00 output per million tokens (long context: $2.00 / $0.10 / $7.50)
- GPT-6.1 Sol: $2.00 input, $0.10 cached input, $10.00 output per million tokens (long context: $4.00 / $0.20 / $15.00)
- GPT-6 Astra: $10.00 input, $1.00 cached input, $50.00 output per million tokens (long context: $20.00 / $2.00 / $75.00)
- GPT-5.6 Terra (prior generation): $2.00 input, $0.20 cached input, $12.00 output per million tokens

## Published Benchmark Results for GPT-6.1 Terra

OpenAI reports gains over GPT-5.6 Terra on Agents' Last Exam, MMMU-Pro, OSWorld 2.1, Terminal-Bench 4.0 and its factual-error test. OpenAI also reports that GPT-6.1 Terra completes tasks over three times faster than GPT-6.1 Sol and GPT-6 Astra. Agents' Last Exam is OpenAI's computer-use benchmark of professional tasks, such as logging an animated film shot by shot or turning architectural drawings into a three-dimensional model. At medium reasoning effort, GPT-6.1 Terra scored 15 percentage points higher than GPT-5.6 Terra at about one-third of the cost. At medium effort, it matched GPT-6 Astra at one-tenth of Astra's token price.

OpenAI reports that on MMMU-Pro, GPT-6.1 Terra outperformed GPT-5.6 Terra across every reasoning setting. It nearly matched GPT-6.1 Sol and GPT-6 Astra, though OpenAI did not publish exact numeric scores. On the OSWorld 2.1 desktop agent benchmark, OpenAI states that Terra at medium reasoning effort surpassed GPT-5.6 Terra's best score at a fraction of the cost per completed task.

OpenAI evaluated factual accuracy on error-inducing prompts. At low reasoning effort, GPT-6.1 Terra's error rate dropped to 16.7 percent, down from 53.6 percent on GPT-5.6 Terra. At maximum reasoning effort, the factual error rate dropped by 18 percentage points to 4.3 percent, nearly matching GPT-6 Astra and GPT-6.1 Sol. Terminal-Bench 4.0 measures command-line coding agents. On this benchmark, Terra solved 50.3 percent of tasks at maximum effort compared to 21.5 percent for GPT-5.6 Terra. On Terminal-Bench-Science 0.1, Terra surpassed GPT-6.1 Sol at lower cost and finished within five points of Claude Opus 5.5 at less than one-fifth of Claude Opus 5.5's estimated cost.

Artificial Analysis publishes its own independent figures. On their Coding Agent Index v2.0, Artificial Analysis scored GPT-6.1 Terra at 48, compared to 54 for GPT-6.1 Sol and 31 for GPT-6 Luna. On their AA-Omniscience hallucination benchmark, Terra produced a 38 percent hallucination rate compared to 88 percent for GPT-5.6 Terra, while lifting overall accuracy by two points. Artificial Analysis recorded a task cost at maximum reasoning effort of $0.40 for GPT-6.1 Terra versus $1.40 for GPT-5.6 Terra. Artificial Analysis also places GPT-6.1 Terra's low and medium effort settings on its intelligence-versus-cost efficiency frontier.

When comparing these figures with other providers, teams should remember that vendor numbers come from separate evaluation setups rather than a single head-to-head test. Anthropic reports that Claude Sonnet 5.5 achieved 70.6 percent on Terminal-Bench 4.0 and 80.1 percent on OSWorld 2.1 under partial-credit scoring. Anthropic's Claude pricing lists Sonnet 5.5 at $2.00 per million input tokens and $10.00 per million output tokens. On inputs up to 272,000 tokens, GPT-6.1 Terra's standard rates are half of Sonnet 5.5's.

- Agents' Last Exam (Medium Effort): 15 percentage points above GPT-5.6 Terra at one-third cost; matches Astra at one-tenth Astra's token price (OpenAI)
- Agents' Last Exam (Max Effort): Over 3x as token efficient as Claude Opus 5.5 and one-fourth its token rate, yielding about 9x lower task cost (OpenAI)
- Terminal-Bench 4.0 (Max Effort): 50.3 percent for Terra versus 21.5 percent for GPT-5.6 Terra (OpenAI); rivaling Sol and roughly 20 points over 5.6 Terra (Artificial Analysis)
- Factual Error Rate: 16.7 percent at low effort (down from 53.6 percent) and 4.3 percent at max effort (OpenAI internal test)
- Coding Agent Index v2.0: Terra 48, Sol 54, Luna 31 (Artificial Analysis)
- Task Cost (Max Effort): $0.40 for Terra versus $1.40 for GPT-5.6 Terra (Artificial Analysis)
- Hallucination Rate (AA-Omniscience): 38 percent for Terra versus 88 percent for GPT-5.6 Terra (Artificial Analysis)

## Where to Access GPT-6.1 Terra

OpenAI's October 26, 2026 changelog lists GPT-6.1 Terra as available within ChatGPT Work and Codex for Plus, Pro, Business, Enterprise, and Education plans. ChatGPT Free and Go tiers do not include GPT-6.1 Terra, and developer API access is not part of workspace seat subscriptions and is billed separately under API rates.

## Context Windows and API Operating Parameters

Detailed specifications on the OpenAI model documentation define an overall context window of 1,050,000 tokens, with a maximum input size of 922,000 tokens and a maximum output ceiling of 128,000 tokens. The model's knowledge cutoff is June 1, 2026. GPT-6.1 Terra accepts multimodal input consisting of text and images, while output generation is limited to text. The model does not support audio or video inputs.

Developers can adjust computational depth through six reasoning effort settings: none, low, medium, high, xhigh, and max, with medium configured as the default setting. Tool calling requires the OpenAI Responses API, though the model can also be accessed via standard Chat Completions and Batch endpoints. Supported tools include computer use, web search, file search, image generation, code interpreter, hosted shell, apply patch, skills, Model Context Protocol (MCP), and tool search, so GPT-6.1 Terra can operate desktop apps, search, and run code through the Responses API.

The Build tier permits 5,000 Requests Per Minute (RPM) and 1,000,000 Tokens Per Minute (TPM). The Launch tier increases allowances to 10,000 RPM and 4,000,000 TPM. The Grow tier provides 15,000 RPM and 40,000,000 TPM. OpenAI lists cache writes at $1.25 per million tokens for 30-minute retention and $2.50 for 12-hour retention in its API pricing.

## Preparedness Framework Safety Classifications

According to the official OpenAI system card addendum, GPT-6.1 Terra meets the Critical risk threshold for cybersecurity capabilities and the High risk threshold for biological and chemical capabilities under OpenAI's Preparedness Framework. To manage these risk profiles, OpenAI applies the identical safeguards stack used on GPT-6.1 Sol and GPT-6 Astra.

OpenAI rates its prompt-injection robustness as similar to GPT-6.1 Sol and GPT-6 Astra. OpenAI also reports that GPT-6.1 Terra is more likely than GPT-5.6 Terra to stay within assigned task scopes and disclose operational limitations, while maintaining production safety benchmark scores on par with GPT-5.6 Terra.

## Workflow Economics and Cost Analysis Modeling

At Layer3Labs, in our internal cost analysis, we modeled three hypothetical workloads to compare what they cost on GPT-6.1 Terra, GPT-6.1 Sol, GPT-5.6 Terra, GPT-6 Astra and Claude Sonnet 5.5. Each scenario uses OpenAI's and Anthropic's published standard rates for inputs up to 272,000 tokens. We calculated costs with this formula. Cost = tasks × (uncached input tokens × input rate + cached input tokens × cached rate + output tokens × output rate) ÷ 1,000,000. The token counts are assumptions we chose for illustration, and no real usage was measured.

We modeled 1,000 invoices at 3,000 input and 500 output tokens. The modeled run costs $5.50 on GPT-6.1 Terra, compared to $11.00 for GPT-6.1 Sol, $12.00 for GPT-5.6 Terra, $55.00 for GPT-6 Astra, and $11.00 for Claude Sonnet 5.5. In a customer support scenario processing 10,000 replies at 2,000 input tokens (where 60 percent of the prompt is a cached system instruction) and 400 output tokens, Terra totaled $28.60. That support run reached $57.20 on Sol and $66.40 on GPT-5.6 Terra. Astra totaled $292.00, while Claude Sonnet 5.5 cost $57.20. In a long-document review scenario evaluating 100 files at 200,000 input tokens and 4,000 output tokens each, Terra totaled $22.00, compared to $44.00 on Sol, $44.80 on GPT-5.6 Terra, $220.00 on Astra, and $44.00 on Sonnet 5.5.

These token-only estimates assume the stated cache reads hit. They exclude cache writes, cache warmup, tool fees, retries, and taxes. Failed cache reads increase cost.

A per-token discount does not guarantee lower per-task expenses. That advantage disappears if an agent outputs excessive tokens to reach a conclusion. In the invoice entry scenario, GPT-6.1 Terra costs the exact same $11.00 as GPT-6.1 Sol if Terra generates 3.2 times as many output tokens as Sol to complete the identical task. Reasoning tokens grow with the effort level. Inputs beyond 272,000 tokens double the base input rate to $2.00 per million.

Who should not pick GPT-6.1 Terra: teams whose main workload is complex coding should start on GPT-6.1 Sol. OpenAI names GPT-6.1 Sol and GPT-6 Astra as the better choice for the most complex coding tasks. Artificial Analysis scores GPT-6.1 Terra 48 against GPT-6.1 Sol's 54 on its Coding Agent Index v2.0. What would change our recommendation: independent evals showing GPT-6.1 Terra needs 3.2 times GPT-6.1 Sol's output tokens for the same job. In our invoice scenario the two cost the same at that ratio, and Sol is cheaper above it. A change to either rate card would also change that threshold. Before you route work, price your own token counts with the GPT-6.1 Terra pricing breakdown.

## Frequently Asked Questions

- No free access is listed. OpenAI's current changelog names Plus, Pro, Business, Enterprise and Edu and does not mention Free or Go. API use of gpt-6.1-terra is billed per token, at $1.00 per million input tokens and $5.00 per million output tokens.
- GPT-6.1 Terra handles routine scripting and command-line automation, scoring 50.3 percent on Terminal-Bench 4.0 compared to 21.5 percent for GPT-5.6 Terra. However, OpenAI explicitly states that GPT-6.1 Sol and GPT-6 Astra remain the better choice for complex software development.
- OpenAI officially launched GPT-6.1 Terra on October 26, 2026. The release updated the mid-tier Terra class from GPT-5.6 to GPT-6.1 across API endpoints, ChatGPT Work, and Codex.
- Use GPT-6.1 Terra for computer-use agents, visual data extraction, invoice reading, and general office writing at half the price per token. Choose GPT-6.1 Sol when your workflows require the most complex coding tasks, as covered in the GPT-6.1 model tiers guide.
- GPT-6.1 Terra provides an overall context window of 1,050,000 tokens, supporting up to 922,000 input tokens and 128,000 output tokens. Standard pricing covers inputs up to 272,000 tokens, above which long-context rates apply.
