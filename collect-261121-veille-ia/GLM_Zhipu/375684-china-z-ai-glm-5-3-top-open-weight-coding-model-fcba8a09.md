---
id: collect-261121-veille-ia/glm-zhipu/375684-china-z-ai-glm-5-3-top-open-weight-coding-model-fcba8a09
title: "375684 china z ai glm 5 3 top open weight coding model"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["glm-5", "coding", "model", "more", "tokens", "open", "models", "plan"]
source: ""
source_anchor: ""
source_lines: [1, 21]
section: "GLM_Zhipu"
---
In brief
- Zhipu AI (now Z.ai) released GLM-5.3 on Thursday through its GLM Coding Plan and ZCode.
- The lab calls it the "most capable open-weights model for coding,".
- GLM-5.3 is a 743-billion-parameter model built by scaling post-training on the GLM-5.2 base.
Chinese AI lab Z.ai released GLM-5.3 on Thursday, a sizable coding model it's pitching as the strongest open-weights coder on the market. The model is live now through the GLM Coding Plan subscription and ZCode, with API access and downloadable weights following after a safety review.
"Scaling post-training is all we did for GLM-5.3," the company wrote in its launch post. "With GLM-5.2 we built the stack... Over the past month we kept scaling on this stack: more environments, more diverse tasks, and more compute spent training on them."
The team focused more on token efficiency, not raw dominance. GLM-5.3 stands at 743 billion parameters and consumes a lot less tokens per task than its predecessor. Parameters are the amount of dials a model handle while processing information while tokens are the basic unit of information a model can consume or generate.
Z.ai says GLM-5.3 clears 34.5% on its in-house Z.ai Code Bench at Max effort while burning roughly 75,000 output tokens per task, against GLM-5.2's 23.4% at 96,000. Against closed models, the blog notes it beats Claude Opus 4.8 on token economy but "remains behind Claude Fable 5, which reaches 39.5% at Max effort."
In terms of coding, GLM5.3 is a very good performer, beating fellow Chinese model Kimi K3 on the most relevant benchmarks.
On Terminal Bench 3.0—a test of autonomous shell/tool use in real Linux environments—GLM-5.3 scores 28.3, slightly behind closed models Fable 5 (33.7) and GPT-5.6 Sol (34.6). On DeepSWE v1.1, a benchmark for fixing real GitHub issues end-to-end, open rival Kimi K3 (67.5) and Fable 5 (69.7) both beat GLM-5.3's 66.9.
The pattern can be more or less summed up like this: GLM-5.3 clears its own predecessor and some open peers, but closed U.S. models still lead the headline coding boards.
The cybersecurity results show another important leap. GLM-5.3 leads CyberGym at 84.5% and more than doubles GLM-5.2 on exploitation benchmarks. Z.ai says the model flagged 2,436 vulnerabilities across 269 open-source projects, 1,097 of them medium-to-high severity.
"GLM-5.3 takes agentic coding to the next level, delivering a dramatic improvement over GLM-5.2 while achieving better results with fewer output tokens," Z.ai posted on X. "GLM-5.3 is available now through GLM Coding Plan and ZCode. API access and open weights will be released in stages following rigorous safety evaluations."
GLM-5.3 takes agentic coding to the next level, delivering a dramatic improvement over GLM-5.2 while achieving better results with fewer output tokens. pic.twitter.com/KGc6ZR7GHv
— Z.ai (@Zai_org) August 14, 2026
On price, the gap with U.S. frontier models is the open-weights draw. Z.ai's GLM Coding Plan runs on a points quota (off-peak calls cost half), with Zhipu's API priced at roughly a tenth of U.S. frontier per-token rates—GLM-5.2's official rate was $1.40 in / $4.40 out per million tokens. That stacks against GPT-5.3-Codex at $1.75 / $14 and Claude Opus 4.8 near the top of Anthropic's tiers.
How many days will Claude go down in August?
Z.ai is a Beijing lab included on the U.S. Entity List, which means American firms cannot export controlled tech to it. Despite this, GLM is an extremely popular model and Chinese open-weight models already beat American ones on OpenRouter token usage.
GLM-5.3 weights are set for public release in about two weeks, per the launch post—the open-weights label applies to what's coming, not what's downloadable today.
Daily Debrief Newsletter
Start every day with the top news stories right now, plus original features, a podcast, videos and more.
