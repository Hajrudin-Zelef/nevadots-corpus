---
id: collect-261121-veille-ia/minimax/china-s-minimax-releases-m3-1-flash-revealing-it-as-anonymously-tested-space-bunny-at-half-the-api-p
title: "China's MiniMax releases 'M3.1 Flash,' revealing it as anonymously tested 'Space Bunny,' at half the API price of its predecessor"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax", "model", "flash", "tested", "space", "bunny", "evaluation", "deepseek"]
source: https://ai-oriens.com/en/news/minimax-m3-1-flash/
source_anchor: ""
source_lines: [1, 36]
section: "MiniMax"
---
## Anonymously tested model officially released

Chinese AI company MiniMax released "MiniMax-M3.1-Flash," a new model for AI agents, through its developer API on Oct. 19. It revealed that "Space Bunny," which it had been offering without disclosing its name on U.S. AI model routing service OpenRouter since Sept. 23, was an early version of this model. On Oct. 23, it also released the weights (trained parameters) on Hugging Face.

A few hours before the release, @aitrackerbot, an X account that tracks the appearance of models, had spotted the name "minimax-m3.1-flash" in the model list of MiniMax's official API. Space Bunny was offered on OpenRouter as an anonymous large model with very fast inference, and analysis of how it splits text into tokens led to speculation on its first day that it was made by MiniMax.

## 428 billion total parameters, handling 1 million tokens

The model is a "mixture of experts (MoE)" model that uses only some of its many expert components, with about 428 billion total parameters, of which 23 billion are used to process each token. Its context window is about 1.05 million tokens, and it reads text, images and video and answers in text. It uses a mechanism that narrows attention to part of the context in every layer, and MiniMax says it is about three times as fast as the previous model, M3.

API pricing is $0.15 for input and $0.60 for output per million tokens, half of the previous model M3's $0.30 for input and $1.20 for output. MiniMax claims the cost of agent work is 70% lower than with M3. However, according to U.S.-based atomicagent, which tested it, when used at the default setting with the deepest level of reasoning, the cost per task exceeded that of M3.

## Leads on automation evaluation, trails DeepSeek on development

In MiniMax's evaluations, it scored 62.8 on Automation-Bench, which measures business automation, above the 60.5 of "V4.1-Flash-1017," which DeepSeek released on Oct. 17, and the 50.3 of Anthropic's "Claude Opus 5." On the other hand, it scored 73.7 on DeepSWE, which solves software development tasks, below DeepSeek's 77.0 and Claude Opus 5's 74.0. It also scored only 38.1 on Terminal Bench, which tests command-line operations.

| Evaluation results announced by MiniMax. As reproduced by singularitybyte |  |  |  | 
|---|---|---|---|
| Evaluation | MiniMax M3.1 Flash | DeepSeek V4.1 Flash 1017 | Claude Opus 5 | 
|---|---|---|---|
| Automation-Bench | 62.8 | 60.5 | 50.3 | 
| LOCA-Bench 256K | 60.3 | Not disclosed | 53.7 | 
| BankerToolBench | 76.2 | Not disclosed | 76.2 | 
| Terminal Bench 4.0 | 38.1 | 42.3 | 51.8 | 
| DeepSWE v1.1 | 73.7 | 77.0 | 74.0 | 
| NL2Repo-Bench | 61.2 | 68.2 | 75.3 | 

## Third-party evaluation flags frequent fabrication

On U.S.-based Artificial Analysis' overall index, it scored 42, short of the 45 of DeepSeek's 1017 version, but up 13 points from the previous model M3's 29. On the firm's knowledge evaluation, its accuracy of 56% was the highest in its price range. However, on 93% of the questions it could not answer correctly, it confidently gave plausible fabricated answers. For M3, the figure was 18.4%.

Anyone can obtain the weights, but the terms of use differ from an open-source license. Companies with annual revenue above $20 million need MiniMax's prior written permission for commercial use. Military use is prohibited.

## Official release a month after anonymous testing

MiniMax began offering the preview "M3.1-Flash-Preview" in its own development app on Sept. 27, and ended the free anonymous offering on OpenRouter on Oct. 5. Right after the official release, it also offered a limited-time free version on OpenRouter.
