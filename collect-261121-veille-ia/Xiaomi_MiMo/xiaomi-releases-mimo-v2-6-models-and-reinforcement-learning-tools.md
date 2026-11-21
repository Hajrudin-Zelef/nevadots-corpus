---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomi-releases-mimo-v2-6-models-and-reinforcement-learning-tools
title: "Xiaomi releases MiMo-V2.6 models and reinforcement-learning tools"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["xiaomi", "mimo-v2", "model", "reinforcement-learning", "more", "training", "models", "environments"]
source: https://www.aiaffairs.com/technology/xiaomi-releases-mimo-v2-6-models-reinforcement-learning-tools/
source_anchor: ""
source_lines: [1, 37]
section: "Xiaomi_MiMo"
---
Xiaomi released its MiMo-V2.6 model family on 21 September, alongside reinforcement-learning environments and training tools described in its technical report. The release makes the weights of two multimodal models and a smaller research checkpoint available under the MIT licence, eWeek reported.

## Key points

- Artificial Analysis scored MiMo-V2.6-Pro at 46 on its Intelligence Index.
- Xiaomi says Pro and Flash each underwent 30 reinforcement-learning steps in less than six days.
- The release includes more than 7,000 task environments and an end-to-end training framework.

## Three MiMo-V2.6 checkpoints serve different purposes

The downloadable models are MiMo-V2.6-Pro-RL, MiMo-V2.6-Flash-RL and MiMo-V2.6-Distill-Qwen-9B. Their public repositories each list an MIT licence, according to eWeek. Releasing weights allows developers to download and run the models themselves, while the accompanying training resources give researchers material for examining how Xiaomi trained agents to carry out tasks.

Pro accepts text, images, video and audio, and its context window reaches up to 1 million tokens, according to its repository as described by eWeek. A context window is the material a model can hold within one interaction: more like the size of a workbench than a measure of how well the work on it will turn out. Flash is the more efficiency-focused model in the family and also supports those input types and a 1-million-token context window, VentureBeat reported.

Reviewing a long document alongside pictures and a recording could therefore involve a single model that accepts all three kinds of material. The larger context window would allow more of that material to be submitted together, while the result would still depend on how well the model handles the particular contents.

The 9B checkpoint has a different role. Xiaomi describes it as a supervised fine-tuned version of Alibaba’s Qwen3.5-9B, made using MiMo-generated data and intended as a starting point for research into reinforcement learning for agents, eWeek reported. Its purpose is further experimentation rather than serving as a smaller version of Pro or Flash.

## Xiaomi scales practice across 7,000 environments

Reinforcement learning trains a model through repeated attempts at tasks and feedback on the results. Xiaomi’s technical report describes preparation on a broad collection of multimodal material before that stage, followed by reinforcement-learning work across coding, general, visual and cybersecurity tasks. Instead of learning only from completed examples, a model can attempt a task, receive a reward signal and adjust towards responses that earn better feedback.

Xiaomi says it increased the scale of that practice in three ways: processing larger batches of attempts, using a wider range of task environments and devoting more computation to judging the results. Its report describes groupwise grading for long tasks, which evaluates attempts together to give the training process a more useful signal and encourage shorter solutions. Keeping that feedback reliable matters when a task involves several actions before there is an outcome to assess.

In Xiaomi’s account, each training step processed 1,568 samples and between 2.7 billion and 3.7 billion tokens, with context lengths reaching 1 million tokens. VentureBeat reported that the runs generated 16 candidate trajectories per prompt. Xiaomi says Pro and Flash each completed 30 reinforcement-learning steps, involving roughly 750,000 trajectories, in under six days. A trajectory is one attempted route through a task, including the actions taken along the way.

Xiaomi puts the reinforcement-learning cost at about $2.62 million for Pro and $850,000 for Flash, excluding pretraining and other development work, eWeek reported. Those are the company’s figures for this stage of training, rather than prices for producing either model from the beginning.

The release also includes more than 7,000 environments spanning software engineering, vulnerability reproduction, knowledge-intensive tasks and web development, Open Source For You reported. Its end-to-end framework covers interaction with those environments, collection of trajectories, evaluation of rewards and updates to the model. That gives researchers both tasks on which an agent can practise and machinery for feeding the outcomes back into training.

## Artificial Analysis scores MiMo-V2.6-Pro at 46

Artificial Analysis independently tested the released Pro model and scored it at 46 on its Intelligence Index, according to eWeek. Xiaomi described that result as the highest score for an open-weight model on the index and said leading closed-source models scored 53, TechNode reported. The score measures performance on the index; the separate account of the training run, including its costs and the contribution of its components, comes from Xiaomi.

Xiaomi also reports results on SWE-bench Verified and DeepSWE v1.1, benchmarks used to assess coding capabilities. Epoch AI classifies both as flawed because of problems involving scoring, contamination or task quality, eWeek reported. For cybersecurity, Xiaomi reports improvements on its MiMo Cyber benchmark, while eWeek described independent security testing specific to MiMo-V2.6 as limited.

Xiaomi released MiMo-V2.5-Pro in April and MiMo Code, a terminal coding agent, in June, and separately introduced HarnessX, a framework for optimising the systems around an agent, VentureBeat reported. In the MiMo-V2.6 technical report, Xiaomi says it froze the model’s mixture-of-experts router during reinforcement learning and built safeguards against reward hacking, in which a training system finds ways to earn feedback without carrying out the intended task.
