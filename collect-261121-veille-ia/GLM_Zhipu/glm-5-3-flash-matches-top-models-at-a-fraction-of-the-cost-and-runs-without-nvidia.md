---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-flash-matches-top-models-at-a-fraction-of-the-cost-and-runs-without-nvidia
title: "GLM-5.3-Flash matches top models at a fraction of the cost, and runs without Nvidia"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["glm-5", "model", "flash", "nvidia", "cost", "tokens", "matches", "models"]
source: https://the-decoder.com/the-chinese-ai-model-glm-5-3-flash-runs-without-nvidia-and-costs-a-fraction-of-what-the-competition-does/
source_anchor: ""
source_lines: [1, 37]
section: "GLM_Zhipu"
---
# GLM-5.3-Flash matches top models at a fraction of the cost, and runs without Nvidia

## Key Points

- Z.ai released GLM-5.3-Flash, a model with 320 billion parameters and a context window of one million tokens.
- On the Intelligence Index it nearly matches the larger GLM-5.3, but at 0.09 dollars per task it's about 7.5 times cheaper.
- The model ran entirely on Chinese AI chips, and Z.ai's own software delivered efficiency on par with Nvidia GPUs.

**Z.ai's new GLM-5.3-Flash model offers strong value for the money, comes with clear weaknesses, and brings a noteworthy infrastructure angle.**

GLM-5.3-Flash is the first natively multimodal model in the GLM-5 series, according to Z.ai. It has 320 billion total parameters, of which only 18 billion are active, ships under an MIT license, and offers a context window of one million tokens. The weights are available on Hugging Face.

Measurements from Artificial Analysis put the model at 57 points on the Intelligence Index at maximum reasoning effort. That's just three points behind the larger GLM-5.3, which scores 60, and level with GPT-5.6 Terra and Muse Spark 1.2.

The price is what stands out. Cost per task on the index runs 0.09 dollars, against 0.68 dollars for GLM-5.3, roughly 7.5 times cheaper. That puts the model on the Pareto frontier of intelligence and cost, according to Artificial Analysis, and adds it to a growing list of Chinese models that have recently put heavy price pressure on Western providers.

On Z.ai's API, GLM-5.3-Flash costs 0.15 dollars per million input tokens and 0.50 dollars per million output tokens, a little over ten percent of the price of GLM-5.3. On agentic tasks the model keeps pace with its bigger sibling. On GDPval-AA v2 it hits an Elo score of about 1770, matching GLM-5.3 and Grok 4.6, and trails only Claude Opus 5. It's still less token-efficient, though. Artificial Analysis found that roughly 90 percent of the output tokens it burned went to reasoning.

## Chinese chips instead of Nvidia

Before launch, Z.ai tested the model anonymously as "ox-alpha" on OpenCode and OpenRouter, where it became the most popular model of the week. Interestingly, all of that traffic ran on Chinese AI chips, according to Z.ai.

SemiAnalysis reports it served 100 trillion tokens a day, a level of capacity that until now was thought possible only for frontier labs. Z.ai puts its hardware efficiency and cost per token on par with common Nvidia GPUs. SemiAnalysis reads this as another test of the "CUDA moat," coming after the recent results from OpenAI's new chip.

CUDA is Nvidia's programming layer between AI software and the graphics card. It has grown for nearly 20 years, and just about every AI framework is tuned for it. Switching to other chips means redoing that work, reprogramming compute operations, adjusting memory access, and hunting down bottlenecks.

So Z.ai built its own serving software on top of SGLang and broke processing into stages that scale independently. The team says this tripled throughput over its first attempt on the same hardware. An agent based on GLM-5.3 helped with the optimization.

THE DECODER+

## AI News Without the Hype – Curated by Humans

- Every Plus article and the full archive
- No ad banners
- Frontier Radar: 6 deep dives a year

Already a subscriber? Sign in
