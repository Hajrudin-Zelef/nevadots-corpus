---
id: collect-261121-veille-ia/robotics-kyutai/kyutai-releases-reki-a-2-4b-robotics-model-that-uses-cached-memory-to-reduce-oscillatory-actions-sur
title: "Kyutai releases REKI, a 2.4B robotics model that uses cached memory to reduce oscillatory actions · SURL AI News"
domain: robotics-kyutai
role: reference
task: article
actors: []
dates: []
keywords: ["memory", "model", "reki", "kyutai", "uses", "actions", "action", "releases"]
source: https://ainews.surl.tw/article/kyutai-公開-reki-2-4b-機器人模型-以快取記憶降低動作反覆振盪-a1257414?lang=ja
source_anchor: ""
source_lines: [1, 15]
section: "Robotics_Kyutai"
---
最新模型

# Kyutai releases REKI, a 2.4B robotics model that uses cached memory to reduce oscillatory actions

On October 16, Kyutai announced REKI and released its pretrained and fine-tuned weights, along with data processing, training, and deployment code. The model adds short-term memory to its action expert; the team’s tests show fewer repeated actions, but the weights are limited to non-commercial use and performance still awaits independent reproduction.

Kyutai, Inria, and École nationale des ponts et chaussées have introduced REKI, a vision-language-action model with about 2.4 billion parameters. The project was announced on October 16, and the paper was submitted to arXiv the day before. The release includes pretrained and task-fine-tuned weights, as well as code for data preparation, training, evaluation, and deployment on physical robots. Official release log, paper

REKI uses Qwen3.5-2B to encode camera images and text instructions, then an action expert with about 200 million parameters generates action sequences using flow matching. Its key design adds 32 learnable cache tokens per frame to summarize the current observation and retain key-value pairs in a cache for later actions to read. Because these tokens do not attend to action tokens, they can be reused across denoising steps, and historical images do not need to be encoded again. Training uses a three-frame window with one-second intervals between frames; at inference, memory from the two preceding frames is retained. Architecture

On a single H100, with two 256×256 image streams, ten Euler steps, and CUDA graphs, the team measured 21.8 milliseconds per action chunk. Reading historical memory added about 0.1 milliseconds, while each past frame used about 2 MiB of display memory. These figures offer a basis for assessing the cost of short-term memory, but they should not be treated as the latency of a complete robot control loop. Inference test conditions

The public repository reports success rates of 90.4% on LIBERO-Plus, 65.8% on RoboTwin Clean2Rand, and 38.4% on RoboCasa365, each using its corresponding fine-tuned model. The team’s ablation study also found that, on RoboTwin Clean, the share of episodes oscillating repeatedly between two poses fell from 18.6% without memory to 4.1%. This supports the idea that short-term observations can help resolve action ambiguity, but the results are still from the team’s own tests and do not establish that all robot tasks will benefit. Evaluation code and results, memory ablation

For researchers, the released data recipe and evaluation workflow can help with reproduction. Deployers should note that the weights are licensed under CC BY-NC-SA 4.0. The model was trained primarily on tabletop manipulation and parallel-jaw gripper demonstrations; adapting it to a new robot’s state configuration requires retraining the input layer, and the replanning interval must match the training setup. Independent reproductions, latency on other hardware, and the fine-tuning cost for new grippers and tasks are key areas to watch. Model card and limitations
