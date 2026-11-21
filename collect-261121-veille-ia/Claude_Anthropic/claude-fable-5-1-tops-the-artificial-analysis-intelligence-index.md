---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-tops-the-artificial-analysis-intelligence-index
title: "Claude Fable 5.1 tops the Artificial Analysis Intelligence Index"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "intelligence", "tokens", "index", "task", "cache", "effort"]
source: https://artificialanalysis.ai/articles/claude-fable-5-1/
source_anchor: ""
source_lines: [1, 60]
section: "Claude_Anthropic"
---
# Claude Fable 5.1 tops the Artificial Analysis Intelligence Index but costs 20% more per task than Fable 5 despite a 75% cache read price cut

See model page
We supported Anthropic with pre-release evaluation of Claude Fable 5.1. At max effort it scores 66 on the Artificial Analysis Intelligence Index, the highest score we have measured, ahead of Claude Opus 5 (max, 63), Claude Fable 5 (max, 62), GPT-5.6 Sol (max, 61) and Grok 4.6 (high, 61). We evaluated the model with Anthropic's 'default' server-side fallback, which routes safety-flagged requests to Claude Opus 4.8 or Claude Opus 5; fallback served ~4% of output tokens across the Intelligence Index.

**Key takeaways:**

➤ **Frontier intelligence with improvements across benchmarks:** Fable 5.1 gains +4 points on the Intelligence Index over Fable 5. On HLE, Fable 5.1 scores 59.1%, ahead of the previous best of 55.5% from Claude Fable 5. It posts the narrowly highest scores we've seen on Terminal-Bench v2.1 (91.4%) and SciCode (62.0%), and on 𝜏³-Banking it gains 9 points over Fable 5

➤ **75% cache read price cut, but Fable 5.1 still costs more per task:** Anthropic has cut the cache read price from $1 to $0.25 per 1M cached input tokens, with standard pricing unchanged at $10/$50 per 1M input/output tokens. Fable 5.1 (max) costs $3.76 per Intelligence Index task, 20% more than Fable 5 (max), because it uses ~1.7x the output tokens. The cache cut saves ~$1.40 per task, concentrated in the agentic evaluations where the majority of input tokens are cache reads. At xhigh effort Fable 5.1 scores 65 at $2.72 per task, $1.04 less than max, but still above Claude Opus 5 (max, 63) at $2.34

➤ **Claude Fable 5.1 holds the upper end of the Intelligence vs. Output Tokens per Task Pareto frontier:** every model variant scoring higher than GPT-5.6 Sol (medium) on the Intelligence Index is matched or beaten by a Fable 5.1 effort level on both intelligence and token usage

➤ **Highest scores on agentic work tasks, but effectively tied with Opus 5:** Fable 5.1 sets the highest scores we have measured on GDPval-AA v2 (1,853 Elo, +130 over Fable 5) and AA-Briefcase (1,694 Elo, +122 over Fable 5), our agentic knowledge work evaluations. Against Claude Opus 5 the GDPval-AA v2 lead is within the confidence interval and AA-Briefcase (1,685) is effectively tied, with Fable 5.1 ahead on analytical quality and rubric correctness, but behind on presentation

**Other model details:**

➤ **Context window:** 1 million tokens, supporting image and text inputs as with Anthropic's other recent launches

➤ **Pricing:** Fable 5.1 retains the $10/$50/$12.5 input, output, and cache write prices per million tokens from Fable 5, but cache hits have been reduced to $0.25 per million tokens, a 75% relative reduction that will materially reduce agentic workload costs


Claude Fable 5.1 (max) costs $3.76 per Intelligence Index task, 20% more than Claude Fable 5 (max) at $3.14 and 1.6x Claude Opus 5 (max) at $2.34, driven by ~1.7x the output tokens of Fable 5. Anthropic's cut to cache read pricing from $1 to $0.25 per 1M tokens saves ~$1.40 per task; without it Fable 5.1 would cost ~$5.16. At xhigh effort Fable 5.1 scores 65 at $2.72 per task, $1.04 less than max effort.


Claude Fable 5.1's five effort settings span 11x in output token usage, from 13.1M at low effort to 143.7M at max, and score from 58 to 66 on the Artificial Analysis Intelligence Index. Across effort levels Fable 5.1 sits on the Intelligence vs. Output Tokens frontier, but its floor is higher than the GPT-5.6 family's: GPT-5.6 Sol (medium) uses marginally fewer tokens than Fable 5.1 (low), 12M against 13.1M.


Claude Fable 5.1 (max) leads GDPval-AA v2 at 1,853 Elo, ahead of Claude Opus 5 (max) at 1,824, although their confidence intervals overlap. On AA-Briefcase the two are effectively tied at 1,694 against 1,685, and the sub-scores diverge: Fable 5.1 is ahead on analytical quality (2,025 against 1,980) and behind on presentation (1,495 against 1,572). Both benchmarks test whether models can produce accurate and well-presented professional outputs using our open source reference agent harness, Stirrup.


Claude Fable 5.1 (max) attempts 93.4% of AA-Omniscience questions against Claude Opus 5's 87.8%, and records the highest accuracy we have measured at 67.2%, ahead of Claude Fable 5 at 65.4%.

It hallucinates more with this higher attempt rate: of questions it did not get correct, it attempted to respond in 72.6% against Claude Fable 5's 63.6%. The two effects cancel out, and its AA-Omniscience Index score is level with Claude Fable 5.


Full breakdown of the individual evaluations in the Artificial Analysis Intelligence Index for Claude Fable 5.1 across all reasoning efforts:


See Artificial Analysis for further details and benchmarks of Claude Fable 5.1: https://artificialanalysis.ai/models/claude-fable-5-1

#### Read the latest

### Google continues to accelerate its release cadence with Gemini 4 Neon

Google continues to accelerate its release cadence with Gemini 4 Neon, delivering a small Intelligence gain over Gemini 4 Carbon at a ~17% lower Cost per Task

November 18, 2026

### OpenAI has the #2 model in the Intelligence Index with the release of GPT-6.2 Astra and GPT-6.2 Sol

OpenAI’s release of GPT-6.2 Astra and GPT-6.2 Sol delivers the #2 model in the Artificial Analysis Intelligence Index and most of the Cost per Task Pareto frontier

November 17, 2026

### Multimodal Knowledge Agent (MKA) benchmark: testing knowledge work capabilities with complex visual files

We’ve partnered with Google DeepMind to launch their Multimodal Knowledge Agent (MKA) evaluation, a new benchmark for agentic knowledge work over complex visual files

November 10, 2026
