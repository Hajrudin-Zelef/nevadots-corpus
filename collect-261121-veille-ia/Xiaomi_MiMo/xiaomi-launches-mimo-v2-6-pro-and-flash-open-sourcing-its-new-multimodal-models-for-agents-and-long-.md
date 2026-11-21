---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomi-launches-mimo-v2-6-pro-and-flash-open-sourcing-its-new-multimodal-models-for-agents-and-long-
title: "Xiaomi launches MiMo-V2.6 Pro and Flash, open-sourcing its new multimodal models for agents and long-horizon work"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["xiaomi", "mimo-v2", "tokens", "model", "flash", "multimodal", "models", "agents"]
source: https://www.datastudios.org/post/xiaomi-launches-mimo-v2-6-pro-flash-open-source-multimodal-agents
source_anchor: ""
source_lines: [1, 241]
section: "Xiaomi_MiMo"
---
Xiaomi launches MiMo-V2.6 Pro and Flash, open-sourcing its new multimodal models for agents and long-horizon work

Sep 22

6 min read

Updated: Sep 23

Xiaomi has released MiMo-V2.6 Pro and MiMo-V2.6 Flash, two open-weight multimodal models designed around long-horizon agents, coding, professional work, computer use, and workloads that combine text, images, video, and audio.

The September 22 release expands Xiaomi's MiMo family in two directions at once. MiMo-V2.6-Pro becomes the flagship model, built on a sparse Mixture-of-Experts architecture with 1.02 trillion total parameters and 42 billion activated parameters, while MiMo-V2.6-Flash targets substantially lower inference cost for high-volume workloads. Both models support a 1-million-token context window and up to 128,000 output tokens.

Xiaomi is also releasing more than model weights. The company has published its technical report, reinforcement-learning resources, more than 7,000 agent task environments, training infrastructure, lightweight harness components, and a distilled 9B model intended for further RL experimentation.

MIMO-V2.6

PRO

FLASH

Positioning

Flagship reasoning and agent model

High-volume professional model

Context window

1M tokens

1M tokens

Maximum output

128K tokens

128K tokens

Modalities

Text, image, video, audio

Text, image, video, audio

Tool calling

Yes

Yes

Web search

Yes

Yes

Structured output

Yes

Yes

Input, cache miss

$0.435 / 1M tokens

$0.14 / 1M tokens

Cached input

$0.0036 / 1M tokens

$0.0028 / 1M tokens

Output

$0.87 / 1M tokens

$0.28 / 1M tokens

Open weights

Yes

Yes

··········


A 1.02-TRILLION-PARAMETER FLAGSHIP WITH 42 BILLION ACTIVE PARAMETERS

MiMo-V2.6-Pro uses a sparse Mixture-of-Experts architecture with 1.02 trillion total parameters but only 42 billion activated for each token, allowing Xiaomi to scale total model capacity without requiring all parameters to participate in every inference step.

The language backbone contains 70 Transformer layers, divided between 60 sliding-window-attention layers and 10 global-attention layers. Its MoE blocks contain 384 routed experts, of which eight are activated per token.

The multimodal stack is integrated into the same model. Xiaomi specifies a 681-million-parameter MiMo vision encoder alongside separate audio encoders, while a five-layer speculative decoder is used for multi-token prediction. The model can therefore process text, images, video, and audio within a common agent workflow instead of routing each medium through a completely separate model family.

For workloads such as repository-scale software development, multimodal research, computer interaction, or multi-agent coordination, the architecture combines this native multimodality with a 1-million-token working context.

··········


XIAOMI IS SCALING REINFORCEMENT LEARNING, NOT ONLY MODEL SIZE

The more distinctive part of MiMo-V2.6 is its training strategy. Xiaomi describes the release as an attempt to scale reinforcement learning toward recursive self-improvement, increasing RL compute, task diversity, and the amount of computation used to evaluate model trajectories.

Instead of performing separate reinforcement-learning runs for coding, visual tasks, cybersecurity, and general agents, Xiaomi says MiMo-V2.6 uses a single mixed RL process in which different task families and agent harnesses are trained together.

The company reports batches containing 1,568 prompts with 16 rollouts per prompt, producing billions of tokens during individual optimization updates.

Its six-day public RL training run completed 30 training steps for both Flash and Pro and generated approximately 750,000 trajectories in total. Xiaomi reports training costs of approximately $850,000 for Flash and $2.62 million for Pro.

Data Studios calculation: combined, those two reported RL runs represent approximately $3.47 million of training expenditure, before considering the six months of research and engineering work Xiaomi says preceded the live training phase.

··········

[[ADPLUS_300x250_1]]

LONG-HORIZON AGENTS ARE THE CENTRAL WORKLOAD

MiMo-V2.6 is designed around tasks in which the model must continue operating after the first reasoning step. That includes inspecting software repositories, executing tools, navigating graphical interfaces, reviewing intermediate results, correcting previous actions, and coordinating several specialized agents over a longer workflow.

Xiaomi's training system reinforces this behavior through multiple agent harnesses rather than optimizing the model around one fixed execution environment. The company argues that exposing the same model to different harness structures improves its ability to transfer agent strategies to frameworks that were not present during training.

The model also uses an agentic grading system designed to distinguish between successful trajectories rather than treating every technically successful completion as equivalent. Better solutions can receive more weight based on factors such as quality and efficiency, creating an incentive to reach correct results with shorter or cleaner execution paths.

For deployed agents, task quality is only one variable. Tokens consumed, number of tool calls, retries, execution time, and human corrections determine the cost of actually completing the workload.

··········

RL RESULTS SHOW LARGE GAINS ON SOFTWARE ENGINEERING

Xiaomi reports substantial improvements during the MiMo-V2.6 reinforcement-learning process. On DeepSWE v1.1, MiMo-V2.6-Flash improved from 48.8 before the full RL run to 65.7 afterward, while MiMo-V2.6-Pro increased from 58.4 to 72.6.

DEEPSWE V1.1

BEFORE RL

AFTER RL

CHANGE

MiMo-V2.6 Flash

48.8

65.7

+16.9 pp

MiMo-V2.6 Pro

58.4

72.6

+14.2 pp

Data Studios calculation: Flash improved by approximately 34.6% relative to its starting score, while Pro improved by approximately 24.3%.

These figures are particularly useful because they compare checkpoints within Xiaomi's own training process rather than mixing models evaluated under unrelated harnesses or inference settings. The final model card reports MiMo-V2.6-Pro at 71.9 on DeepSWE v1.1 and Flash at 67.9 under its published evaluation configuration, reinforcing software engineering as one of the main capability areas of the release.

··········

COMPUTER USE EXTENDS THE MULTIMODAL MODEL INTO ACTION

MiMo-V2.6 combines multimodal perception with action-oriented training. Xiaomi demonstrates the model operating common graphical interfaces for tasks involving information retrieval, editing, data processing, checking its own work, and adjusting subsequent actions based on what appears on screen.

The same architecture is also being tested in embodied environments. Xiaomi shows MiMo-V2.6 receiving multi-camera visual input and controlling a Franka Panda robotic arm through a visual feedback loop to perform grasping, color matching, and object-placement tasks.

These examples demonstrate a broader design objective: the same model should be able to perceive an environment, reason about its state, execute an action, inspect the result, and continue the loop.

That operating pattern is shared by computer-use agents, coding agents, browser agents, and embodied systems even though the external tools are different.

··········

PRO AND FLASH CREATE TWO DIFFERENT COST PROFILES

The API pricing separates MiMo-V2.6 into a performance-oriented Pro tier and a much cheaper Flash tier. MiMo-V2.6-Pro costs $0.435 per million uncached input tokens and $0.87 per million output tokens. MiMo-V2.6-Flash costs $0.14 and $0.28 respectively. Both also support substantially cheaper cached input.

10M INPUT + 2M OUTPUT

PRO

FLASH

Input cost

$4.35

$1.40

Output cost

$1.74

$0.56

Total

$6.09

$1.96

Data Studios calculation: for this simplified uncached workload, Pro costs approximately 3.1 times as much as Flash.

That ratio is almost identical across both input and output pricing, making model selection relatively straightforward when token volume is known. Flash is designed for high-frequency workloads where cost scales rapidly, while Pro allocates more compute to tasks where additional reasoning capability has greater economic value.

Xiaomi has also introduced a Batch API for Pro and Flash at 50% of real-time API pricing, further separating latency-sensitive workloads from asynchronous high-volume processing.

··········

PRO ULTRASPEED ADDS A LATENCY-OPTIMIZED OPTION

Alongside Pro and Flash, Xiaomi is offering MiMo-V2.6-Pro-UltraSpeed, a faster serving mode based on the Pro model.

The company says UltraSpeed can deliver output at up to 20 times the standard inference speed, targeting interactive applications and workflows where model latency blocks subsequent operations. It is available through Xiaomi's MiMo platform and MiMo Desktop.

This creates three operational choices around the same generation: Flash for throughput and low cost, Pro for maximum capability, and Pro UltraSpeed when completion latency is important enough to justify faster infrastructure.

··········

THE OPEN-SOURCE RELEASE GOES BEYOND MODEL WEIGHTS

Xiaomi has open-sourced both MiMo-V2.6 Pro and Flash, but the research package extends considerably further. The release includes more than 7,000 reinforcement-learning task environments covering software engineering, vulnerability reproduction, knowledge-intensive work, and web design and development. Xiaomi has also released the MiMo-V2.6-Distill-Qwen-9B model for smaller-scale RL research.

The company is publishing an end-to-end RL framework covering environment interaction, trajectory generation, reward evaluation, and policy optimization, together with lightweight agent harnesses that separate tools, prompts, and context management.

Researchers can inspect not only the resulting model but also parts of the infrastructure Xiaomi used to produce its agent behavior, including task environments and training configurations.

For open-model development, this makes MiMo-V2.6 a release of models plus an agent-training stack.

··········

WHAT MIMO-V2.6 CHANGES FOR XIAOMI'S AI STRATEGY

MiMo-V2.6 places Xiaomi more directly in the frontier-model competition, but its positioning is unusually specific. The Pro model combines a trillion-parameter sparse architecture, native multimodality, 1M context, long-horizon agent training, computer use, and open weights. Flash provides the same 1M context and multimodal foundation at substantially lower API prices, while UltraSpeed addresses latency-sensitive execution.

The larger strategic contribution may be the training package surrounding the models. By publishing model weights together with thousands of RL environments, a distilled research model, agent harnesses, and parts of its reinforcement-learning framework, Xiaomi is exposing more of the process required to build long-horizon agents rather than limiting the open release to the final checkpoint.

For developers, MiMo-V2.6 can therefore be evaluated on two levels: as a model to deploy and as an open platform for experimenting with agentic reinforcement learning. The second may ultimately be the more distinctive part of the release.
