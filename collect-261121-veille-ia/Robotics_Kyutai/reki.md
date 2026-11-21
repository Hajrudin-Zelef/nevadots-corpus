---
id: collect-261121-veille-ia/robotics-kyutai/reki
title: "REKI"
domain: robotics-kyutai
role: reference
task: article
actors: []
dates: []
keywords: ["action", "expert", "memory", "frame", "model", "frames", "reki", "each"]
source: https://kyutai.org/reki/
source_anchor: ""
source_lines: [1, 105]
section: "Robotics_Kyutai"
---
Juliette Marrie<sup>1</sup>, Paul Pacaud<sup>2</sup>, Adrien Ramanana
Rahary<sup>1,3</sup>, Shizhe Chen<sup>2</sup>, and Moritz Böhle<sup>1</sup>

We introduce REKI, a 2.4B vision-language-action model (VLA) pretrained exclusively on public data that sets a new Pareto frontier between performance and model size: on LIBERO-Plus, RoboTwin Clean2Rand and RoboCasa365, it is outperformed only by models about twice its size. REKI consists of a vision-language model (VLM) followed by a flow-matching action expert, and is pretrained on a curated mixture of public robot, egocentric human and vision-language data. Additionally, we endow it with a memory that gives the policy access to its recent past at negligible cost: a few register tokens in the action expert summarize the output of the VLM at each frame and persist in the expert's cache. This memory improves success in simulation and on real robots, and makes oscillations between poses about four times rarer. REKI uses a simple model design and training paradigm, and is optimized to be efficient at training and inference. We release the full code across data curation, training, inference and evaluation, deployment, and all pretrained and finetuned model weights.

## Memory in the action expert

Intuitively, past context should help a robot act, but it is not clear how best to use it in a VLA. Current methods put past frames into the vision-language backbone, unroll the model during training, or add a separate memory module, and each of these adds training or inference cost. REKI puts the memory in the action expert: a small set of register tokens summarizes each encoded frame and is cached across chunks, so each frame is encoded only once, with no separate module and no sequential unroll.

## Model

A Qwen3.5-2B vision-language backbone encodes the camera views and the instruction as a context, which every block of a 200M-parameter flow-matching action expert reads by cross-attention. Robot state and flow time modulate the actions through adaptive normalization. In the expert, the action tokens of each frame are preceded by 32 learned register tokens. The registers cross-attend the encoded prompt and views of that frame, and self-attend the registers of the earlier frames. Since they do not attend the action tokens, they depend only on the observations and can be shared across frames and denoising steps.

We train on windows of *K* = 3 frames from one episode, taken one second apart. At inference, the keys and values of the registers are cached in a rolling window over the *K* − 1 preceding frames: when a new frame enters, the registers of the oldest one are dropped.

## Memory at almost no cost

The memory is cheap at training and at inference.

- Training. A window of *K* frames is packed into one batch and encoded in a single pass, so it costs no more VLM compute than*K* memoryless frames. In the expert, registers and action tokens are attended by one variable-length FlashAttention call, and the loss of each frame is averaged over eight independent noise draws that share one VLM pass.
- Inference. The registers depend on neither the flow time nor the robot state, so they are computed only once per frame, in the first denoising step. Their keys and values are cached and reused in the other denoising steps and in the next *K* − 1 frames. With*K* = 3, predicting an action chunk from two 256×256 images takes 21.8 ms on one H100, and reading the cached registers adds 0.1 ms. The cache adds about 2 MiB of GPU memory per past frame.

Time of one action chunk on one H100 against the memory window *K* (two 256×256 cameras, ten
Euler steps, VLM and action expert as CUDA graphs; median of 400 chunks, whiskers 10th to 90th
percentile). Labels give the change against *K* = 1. Hover over a bar for its split.

## Fast inference

We time one call, from the raw observation to the actions of one chunk, on one H100 with LIBERO inputs, for each model with its released checkpoint and code. Most of these models are released without a compiled path, or with one that can be further optimized, so we also compile them ourselves without changing their computation. With compilation, REKI predicts a chunk in 21.8 ms, the fastest of the compared models: X-VLA takes 25.5 ms, π<sub>0.5</sub> (openpi) 33.8 ms, ABot-M0 46.1 ms and InternVLA-A1.5 51.3 ms. InternVLA-A1.5 uses the same VLM as REKI, but its larger action expert increases the denoising time.

Inference latency of one action chunk on one H100, from the raw observation to the actions, with LIBERO inputs. Dark: GPU time; light: host time. Dashed: compiled by us.

## Benchmark results

REKI is pretrained once, then finetuned on each benchmark separately:

- LIBERO-Plus perturbs the 40 LIBERO tasks along seven axes: camera viewpoint, initial robot state, language, lighting, background, sensor noise and object layout.
- RoboTwin 2.0 Clean2Rand evaluates models finetuned on clean scenes only under randomized background, lighting, clutter and table height, separately and combined.
- RoboCasa365 evaluates models trained on its 300 kitchen tasks on 50 target tasks in three groups: atomic, composite seen and composite unseen.

Among the models pretrained on public data, REKI has the highest out-of-distribution success on LIBERO-Plus and RoboTwin Clean2Rand, although it is the smallest. On RoboCasa365, it reaches 38.4, ahead of the larger RLDX-1 (36.0) and Qwen-RobotManip (35.9), both pretrained on in-house data.

Pretraining on public data helps most out of distribution, and most where REKI without pretraining fails: it adds 13.8 points on Clean2Rand, including 30.3 under clutter and 22.4 under the Hard condition, and 4.3 on LIBERO-Plus.

## Memory and repetitive motion

Memory improves success on all three benchmarks, and most of the gain already comes with one past frame. Removing the past frames at evaluation brings the model back to the level of a memoryless one, so the gain comes from reading the past at inference.

*K*at finetuning →

*K*at evaluation

Memory window, written as frames at finetuning → frames at evaluation: success rate. RoboCasa365: the bar gives the three task groups, the line the average. Hover over a bar for its values.

Without memory, the policy often gets stuck moving an arm back and forth between two poses, switching direction at each new chunk.

With memory, such oscillations are about four times rarer on RoboTwin (4.1% of the episodes against 18.6%), and the difference is largest in two-arm tasks.

Oscillation. Left: share of oscillating episodes on RoboTwin Clean, by memory window. Right: oscillating episodes by task class, without (1 → 1) and with (3 → 3) memory.

We attribute this to perceptual aliasing: if two poses each look like demonstrations that continue towards the other, a memoryless policy alternates between them. A short history resolves the ambiguity.

## Public data only

REKI is pretrained for 200k steps on 32 H100 GPUs, on a mixture of public data only:

- Robot manipulation. Public teleoperation datasets (RoboSet, Galaxea, AgiBot World, RoboCOIN, RoboMIND, RDT-1B, DROID, BridgeData V2, RT-1, BC-Z), complemented by motion-planned simulation from InternData-A1 and GR00T-X-Embodiment-Sim and bimanual YAM demonstrations from ABC.
- Egocentric human manipulation. EgoDex, EgoVerse and Xperience, about 350k episodes and 170M frames.
- Vision-language data. The FineVision and LLaVA-OneVision-1.5 subsets most relevant for spatial reasoning, plus RoboPoint, RefSpatial and PixMo, at 10% of the sampled frames.

We keep each dataset's native control convention rather than remapping to a shared action space, and we curate every source before training. As REKI is trained only on public data, it is fully reproducible by the scientific community.

## Released models

| Model | Stage | Robot | Benchmark | 
|---|---|---|---|
| `kyutai/reki-2b` | Pretrained | pretraining mixture | – | 
| `kyutai/reki-2b-libero` | Finetuned | Franka Panda | LIBERO, LIBERO-Plus | 
| `kyutai/reki-2b-robotwin-clean` | Finetuned | ALOHA (bimanual) | RoboTwin Clean2Rand | 
| `kyutai/reki-2b-robotwin-cleanrand` | Finetuned | ALOHA (bimanual) | RoboTwin 2.0, clean + randomized | 
| `kyutai/reki-2b-robocasa365` | Finetuned | PandaOmron (mobile) | RoboCasa365 | 
| `kyutai/reki-2b-robocasa-gr1` | Finetuned | Fourier GR-1 humanoid | RoboCasa-GR1 tabletop | 

The code covers training, evaluation on all benchmarks, the data processing pipeline and a real-robot policy server.

```
from reki import RekiPolicy
policy = RekiPolicy.from_pretrained("kyutai/reki-2b-libero", device="cuda")
policy.reset()  # at every episode start: clears the register cache
actions = policy.predict(
    images={"camera_top": img0, "camera_wrist_left": img1},
    state=proprio,
    task="put the bowl on the plate",
)  # [n_action_steps, action_dim]
```
## Citation

```
@article{marrie2026reki,
  title   = {{REKI}: Registering Knowledge in {VLAs}},
  author  = {Marrie, Juliette and Pacaud, Paul and Ramanana Rahary, Adrien and Chen, Shizhe and B{\"o}hle, Moritz},
  journal = {arXiv:2610.22178},
  year    = {2026}
}
```
