---
id: collect-261121-veille-ia/hardware-semi/reflection-ai-releases-beam-becomes-second-best-us-open-model-on-artificial-analysis-intelligence-in
title: "Reflection AI Releases Beam, Becomes Second-Best US Open Model On Artificial Analysis Intelligence Index"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "index", "model", "artificial", "intelligence", "open", "models", "horizon"]
source: https://officechai.com/ai/reflection-ai-releases-beam-becomes-second-best-us-open-model-on-artificial-analysis-intelligence-index/
source_anchor: ""
source_lines: [1, 31]
section: "Hardware_Semi"
---
The US appears to be making some inroads into the open model space, which was thus far largely dominated by China.

Reflection AI, the New York-based lab founded by two former Google DeepMind researchers, has released its first model, and it’s already landed near the top of the pile among open-weight models built in the US.

The model, called Beam, has been benchmarked by Artificial Analysis and scores 30 on the firm’s Intelligence Index, a composite of ten evaluations spanning agentic work, coding, science and general knowledge. That places Beam second among open-weight models from US labs, behind only Meta’s recently released Muse Spark Open, which scored 39, and ahead of NVIDIA’s Nemotron 3.5 Super VL, which came out just a day earlier and scored 26. Globally, only Muse Spark Open and the UAE’s K2 Horizon, which scored 31, sit above Beam among models not built in China.

Beam is a mixture-of-experts model with 501 billion total parameters and 23 billion active per token, released with a 1 million token context window and text-only modalities. It’s available on Hugging Face, and it’s being run through Reflection’s own open-source CLI harness, called Mirror.

**Efficient, but not without trade-offs**

One of the more notable things about Beam is how little compute it needs to get there. Artificial Analysis found it uses roughly 26,000 output tokens per Intelligence Index task, well under half of what K2 Horizon uses to score just one point higher (~52,000 tokens). Proprietary frontier models are still well ahead on the same budget, though — OpenAI’s GPT-6 Astra scores 53 using a similar ~27,000 tokens, which is almost the same efficiency as Beam but nearly double the intelligence score.

Of the roughly 26,000 tokens Beam spends per task, about 19,000 go toward reasoning and only 7,000 toward the final answer. That 26% answer share is the 28th-lowest of the 219 models Artificial Analysis has tested on Intelligence Index v4.3, meaning Beam “thinks” unusually long relative to what it eventually outputs.

The model’s strengths and weaknesses are fairly specific. It does well on agentic benchmarks, scoring 48.5% on AutomationBench-AA, 11 percentage points ahead of K2 Horizon and roughly level with Alibaba’s Qwen3.8 27B. It also stands out on CritPt, a physics-reasoning benchmark, where its 13.1% score is more than double what K2 Horizon and Qwen3.8 27B manage. But it struggles in the terminal: Beam scores just 3.0% on Terminal-Bench 4.0, behind both Qwen3.8 27B (5.6%) and Muse Spark Open (8.1%), and its weakest result across the ten Intelligence Index evaluations.

Beam’s performance on AA-Omniscience, which tests both correctness and overconfidence, tells a similar story of a model that knows more but isn’t always sure when to stop guessing. It answers 33% of questions correctly, comfortably ahead of K2 Horizon (18%) and Qwen3.8 27B (16%), but its hallucination rate also lands at 47%, versus 26% for K2 Horizon and 30% for Qwen3.8 27B. Artificial Analysis’s net index, which weighs accuracy against hallucination, still comes out in Beam’s favour at 1.5, against -3.0 for K2 Horizon and -10.0 for Qwen3.8 27B.

Beam also leads on Artificial Analysis’s newer Cyber Index, a security-focused benchmark built from three evaluations, where it scores 34% — ahead of every other US open-weight model tested so far. On a chart plotting Cyber Index score against output tokens per task, Beam sits on the efficiency frontier alongside OpenAI’s GPT-6 Sol (Daybreak Blue), using far fewer tokens than most other models to get a comparable or better score.

On coding specifically, running inside Mirror, Beam scores 28 on Artificial Analysis’s Coding Agent Index — 15 points behind the next-best setup (Claude Code paired with Qwen3.8 Max, at 43) and 40 points off the leaders. It’s frugal here too, with only two other agent-harness combinations using fewer tokens per task.

**Who’s behind Reflection AI**

Reflection AI was founded in March 2024 by Misha Laskin and Ioannis Antonoglou, two researchers who left Google DeepMind to start the company. Laskin, a theoretical physicist by training who studied at Yale and the University of Chicago, had led reward modelling for DeepMind’s Gemini project before co-founding Reflection, where he’s CEO. Antonoglou, the company’s CTO, was a co-creator of AlphaGo, the system that beat the world Go champion in 2016, and worked on post-training systems for Gemini.

The company came out of stealth in March last year with $130 million in funding across a seed and Series A round, backed by Sequoia Capital, Lightspeed Venture Partners, CRV and NVIDIA’s venture arm, at a valuation of about $555 million. Its original pitch was narrower than what Beam represents today — autonomous coding agents that could read, write, test and deploy code with minimal human involvement.

That changed dramatically in October 2025, when Reflection closed a $2 billion funding round led by NVIDIA, with participation from Eric Schmidt, Citi, and Donald Trump Jr.-backed 1789 Capital, among others. The round pushed the company’s valuation to $8 billion, a roughly 15x jump in seven months, and came with a repositioned mission: building open-weight frontier models as an American, transparent alternative to closed labs like OpenAI and Anthropic, and to Chinese open-weight efforts such as DeepSeek and Qwen. Laskin has described the moment as a “Sputnik moment” for open AI, arguing that if the US doesn’t lead on open models, that ground will be ceded to Chinese labs by default.

Beam is the first language model to come out of that repositioned strategy, and on the numbers so far, it’s a credible opening move — not the best open model out of the US, but closer to the top of that list than most new entrants manage on a first release.
