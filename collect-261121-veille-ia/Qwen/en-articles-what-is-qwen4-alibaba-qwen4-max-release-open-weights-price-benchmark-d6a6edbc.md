---
id: collect-261121-veille-ia/qwen/en-articles-what-is-qwen4-alibaba-qwen4-max-release-open-weights-price-benchmark-d6a6edbc
title: "En articles what is qwen4 alibaba qwen4 max release open weights price benchmark"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["alibaba", "qwen4", "model", "models", "qwen4-max", "next", "weights", "open"]
source: ""
source_anchor: ""
source_lines: [1, 23]
section: "Qwen"
---
Qwen4 is the new generation of Alibaba’s Qwen artificial intelligence models, and its first full release, Qwen4-Max, came out on Monday, November 16, according to The Next Web and MLQ.ai, both citing Alibaba’s launch post. Unlike its predecessor Qwen3.8-Max, whose weights Alibaba said in August it would publish for download (The Decoder called it the first Max-class model to get public weights), and many of Alibaba’s smaller Qwen models, Qwen4-Max cannot be downloaded; it is offered only through Alibaba’s own cloud service and apps.
What Qwen4-Max is
Qwen4-Max is a large language model of the “mixture-of-experts” type, which means only part of the model works on any given request. Alibaba says it has a 2.4-trillion-parameter backbone with 71 billion parameters active for each token, plus 328 billion extra embedding parameters, MLQ.ai reported. Parameters are the internal values a model learns in training; a token is a small chunk of text, often part of a word.
The model reads text, images and video and accepts prompts of up to 1 million tokens, according to The Next Web. Alibaba pitches it as an “AI teammate for work, creativity and discovery”, aimed at coding and long-running tasks.
Morning email
Can you use it, and what does it cost?
Qwen4-Max is available through Alibaba’s QwenCloud API, a subscription called Token Plan, and the company’s QwenWork, Qoder and Qwen Studio products, MLQ.ai reported. A limited preview had opened through Token Plan on November 11.
- Input: $1.50 per million tokens, down from $2.00 for the previous model, Qwen3.8-Max, according to Alibaba Cloud’s international list prices cited by MLQ.ai.
- Output: $4.50 per million tokens, down from $6.00.
- Cached input: $0.075 per million tokens.
Is Qwen4 open source?
Not the flagship. Alibaba did not release Qwen4-Max’s weights, so developers cannot download, run or modify it themselves, The Next Web reported. The company says smaller Qwen4 models will be released with open weights but has not given a date. That matters because Qwen’s open models have been widely used by developers; The Next Web, citing Bloomberg, said Qwen is already the most downloaded open-weight model family in the world.
What Alibaba claims about performance
Alibaba says Qwen4-Max cut the average cost of a task on its own Work360 benchmark from $4.43 to $1.67, about 62% lower than Qwen3.8-Max, mainly because it uses fewer tokens to finish the same work, according to MLQ.ai. Its score on that test rose to 52.69 from 47.74.
On rival models, the picture is mixed. Alibaba published results on 16 benchmarks against Anthropic’s Claude Opus 5.5, OpenAI’s GPT-6.1 Sol and GPT-6 Astra, and Moonshot AI’s Kimi K3.1, and Qwen4-Max leads on some and trails on others, MLQ.ai reported. Charts analyzed by FourWeekMBA and cited by The Next Web show it narrowly ahead of Claude Opus 5.5 on one coding test, DeepSWE (75.7 to 74.2), but behind both Opus 5.5 and Kimi K3.1 on Terminal-Bench 4.0. None of these figures has been independently verified.
The safety guardrails
Much of the launch coverage focused on safety. Alibaba says it kept training data for defensive cybersecurity work, such as finding and fixing software flaws, and removed data that would improve the model’s ability to attack systems. It also added training meant to stop the model from helping with chemical, biological, radiological or nuclear weapons, according to The Next Web.
Alibaba says an internal expert review found no capabilities that pose large-scale cybersecurity risks, MLQ.ai reported. That is the company’s own assessment, not an independent audit.
The emphasis comes after concerns about Chinese models’ behavior. In October, Cohere said an open Qwen model, Qwen3.8-Flash-Next, tried to upload a file to a server in Beijing during a sandboxed test, The Next Web reported, citing TechCrunch. Chinese AI developers have also had to work around US limits on advanced chips; we explained how Chinese AI firms rent Nvidia chips overseas.
What we don’t know
- When the smaller, open-weight Qwen4 models will be released, and how large they will be.
- How Qwen4-Max performs in independent tests: all published scores so far come from Alibaba.
- Whether Qwen4-Max will be offered free in a consumer chat app; the reports we read list only the paid API, Token Plan and Alibaba’s work tools.
