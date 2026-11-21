---
id: collect-261121-veille-ia/grok-xai/grok-4-6-powers-grok-bot-and-ties-gpt-5-6-sol-at-a-fraction-of-the-cost
title: "Grok 4.6 Powers Grok Bot and Ties GPT-5.6 Sol at a Fraction of the Cost"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-02-01"]
keywords: ["grok", "model", "gpt-5", "work", "across", "knowledge", "agentic", "intelligence"]
source: https://www.davidborish.com/post/grok-4-6-powers-grok-bot-and-ties-gpt-5-6-sol-at-a-fraction-of-the-cost
source_anchor: ""
source_lines: [1, 37]
section: "Grok_xAI"
---
xAI released Grok 4.6 on August 12, a little over a month after Grok 4.5, holding its pricing flat at $2 per million input tokens and $6 per million output tokens while posting a five-point gain on the Artificial Analysis Intelligence Index. The model scores 61 on that index, putting it level with OpenAI's GPT-5.6 Sol and one point behind Claude Fable 5. That combination, intelligence gains at an unchanged price, is the core of xAI's pitch for the release, and it lands the model as a serious cost-efficiency option against pricier frontier rivals.

What Changed From Grok 4.5

Grok 4.6 keeps the same 1.5-trillion-parameter foundation as its predecessor. xAI says the gains come entirely from post-training rather than added scale: a longer supplemental training run built on curated, model-generated data covering reasoning, advanced technical concepts, and engineering work. Grok 4.5 was used to regenerate supervised fine-tuning data across reasoning efforts, agent configurations, and domains spanning STEM, software engineering, and general knowledge work, with weaker training traces filtered out. On top of that, xAI ran agentic reinforcement learning across knowledge work, general coding, and a set of specialized environments including kernel optimization, web development, and computer-aided design.

The company says the resulting model checks its own work more often across long task sequences, running additional self-testing and verification before moving to the next step, and produces stronger first attempts on interactive and visual projects. Technical specifications carried over largely unchanged: a 500,000-token context window, a knowledge cutoff of February 1, 2026, text and image input with text-only output, and four configurable reasoning effort levels running from low to an "xhigh" setting. The model is available through function calling, web search, X search, and code execution tools, and xAI recommends setting a prompt cache key for long-running agent loops that benefit from context compaction.

Where the Benchmarks Actually Place It

Artificial Analysis's independent testing gives a more layered picture than the single index score suggests. On GDPval-AA v2, the firm's measure of real-world agentic knowledge work, Grok 4.6 reaches an Elo of 1753, landing within overlapping confidence intervals of Fable 5 and Qwen3.8 Max, meaning the three are statistically difficult to separate on that particular test. On Terminal-Bench v2.1, a measure of terminal-based software tasks, Grok 4.6 scores 88.4%, level with the leading models. On τ³-Banking, a multi-turn customer service and tool-use evaluation, it posts 50.7%, second only to Qwen3.8 Max.

On the composite Intelligence Index itself, which combines nine separate evaluations including SciCode, GPQA Diamond, and Humanity's Last Exam, Grok 4.6 sits at 61 alongside GPT-5.6 Sol, just ahead of Kimi K3 and just behind Fable 5 at 62. That places it firmly among the current frontier tier of models rather than as an outright leader on any single composite score, with its clearest separation from the field showing up in the agentic and knowledge-work evaluations rather than the raw reasoning tests.

The five-point jump over Grok 4.5's score of 56 is also notable set against the pace of the last two releases. Artificial Analysis measured a 23-point gain across Grok 4.6, Grok 4.5, and Grok 4.3 combined, a climb that has moved xAI from a clear also-ran on agentic benchmarks roughly a year ago to a model now competitive turn for turn with GPT-5.6 Sol on the same evaluations. The improvement is concentrated almost entirely in agentic and tool-use categories rather than raw knowledge recall, consistent with xAI's stated focus on long-running tasks over static question answering.

The Cost Argument

Where Grok 4.6 separates itself most clearly is price relative to intelligence. Holding headline pricing flat across a model generation is unusual at the frontier, where capability gains have typically come with cost increases. At $2 per million input tokens and $6 per million output tokens, Grok 4.6 runs more than 60% cheaper than GPT-5.6 Sol ($5 input, $30 output), the model it ties on the Intelligence Index. Artificial Analysis measured Grok 4.6's average cost per task at $0.84, matching Kimi K3 and placing it on what the firm calls the Pareto frontier of intelligence versus cost, meaning no cheaper model currently scores as high and no higher-scoring model currently costs as little.

The efficiency picture holds up on longer agentic work too. On Artificial Analysis's AA-Briefcase benchmark, a private test of long-horizon knowledge-work tasks, Grok 4.6 posts an Elo of 1577, placing it roughly at Fable 5's tier. It reaches that result in about 53 turns and roughly 0.5 billion input tokens on average, a notably efficient path for a long-horizon agentic task according to Artificial Analysis. For workloads that accumulate context over many steps, that turn efficiency compounds into a cost advantage well beyond what the per-token pricing alone suggests. One tradeoff worth noting on the cost side: cache-hit pricing rose to $0.50 per million tokens from Grok 4.5's $0.30, a smaller but real increase buried underneath the flat headline rate.

Availability

Grok 4.6 is live today inside Cursor across all plans, as the default model in Grok Build, through the xAI API, and inside Grok Bot, the always-on agent product xAI released one day earlier. It is also available through OpenRouter, Vercel, and Cloudflare. xAI is running a first-week promotion doubling included usage inside Cursor and Grok Build, and the model is rolling out to SuperGrok subscribers and the standard Grok chat interface via the model picker. Musk described the release on social media in three words, calling it a "banger," a framing that lines up with xAI's broader strategy of shipping models on a roughly monthly cadence rather than saving gains for less frequent, larger jumps.

A Familiar Pattern in the Training Approach

One detail in xAI's own account of the training process is worth flagging on its own terms, separate from the benchmark comparisons. The agentic reinforcement learning stage ran across a mix of general categories, knowledge work and coding, and narrower, more controlled environments: kernel optimization, web development, computer-aided design.

That structure, capability built and measured inside bounded, well-specified environments before being asked to generalize across the far less controlled task distribution real users bring to a chat interface, is the same pattern this publication has tracked across other frontier releases under what I've called the Tony Hawk Paradox. This is an editorial observation about the shape of the training pipeline as xAI has described it rather than a claim the company is making itself, and the real test is the one Artificial Analysis and other independent evaluators will keep running as Grok 4.6 sees production use beyond the benchmark suite.

What's Next

Musk indicated during SpaceX's Q2 earnings call earlier this month that Grok 4.7 is expected roughly three to four weeks after Grok 4.6, with Grok 5 targeted before the end of 2026. Some early reporting on the roadmap has put Grok 4.7 at a larger 2.1-trillion-parameter scale, though that figure has not been confirmed by xAI directly and should be treated as provisional until the company publishes specifications. For now, Grok 4.6 lands as a genuine generational improvement over its predecessor and a legitimate cost-efficiency play against pricier frontier models, with the real test being how it holds up in production use beyond the benchmark suite.

David Borish is a journalist and analyst covering frontier AI and enterprise technology, and the author of the forthcoming book The Tony Hawk Paradox. More of his work is available at davidborish.com.
