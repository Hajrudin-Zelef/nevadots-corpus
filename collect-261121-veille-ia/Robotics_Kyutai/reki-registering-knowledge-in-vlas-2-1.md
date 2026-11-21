---
id: collect-261121-veille-ia/robotics-kyutai/reki-registering-knowledge-in-vlas-2-1
title: "REKI: Registering Knowledge in VLAs"
domain: robotics-kyutai
role: reference
task: article
actors: []
dates: []
keywords: ["reki", "model", "data", "vlas", "pretrained", "memory", "action", "past"]
source: https://arxiv.org/html/2610.22178v1
source_anchor: ""
source_lines: [1, 1500]
section: "Robotics_Kyutai"
---
# REKI: Registering Knowledge in VLAs

###### Abstract

We introduce REKI, a 2.4B vision-language-action model (VLA) pretrained exclusively on public data that sets a new Pareto frontier between performance and model size: on LIBERO-Plus, RoboTwin Clean2Rand and RoboCasa365, it is outperformed only by models about twice its size. REKI consists of a vision–language model (VLM) followed by a flow-matching action expert, and is pretrained on a curated mixture of public robot, egocentric human and vision–language data. Additionally, we endow it with a memory that gives the policy access to its recent past at negligible cost: a few register tokens in the action expert summarise the output of the VLM at each frame and persist in the expert’s cache. This memory improves success in simulation and on real robots, and makes oscillations between poses about four times rarer. REKI uses a simple model design and training paradigm, and is optimised to be efficient at training and inference. We release the full code across data curation, training, inference and evaluation, deployment, and all pretrained and finetuned model weights: github.com/kyutai-labs/reki.

## 1 Introduction

Vision-language-action models (VLAs) have become an effective paradigm for language-conditioned robot control. A pretrained vision–language model (VLM) encodes camera views and instruction, based on which a smaller action expert predicts the next actions (Black et al., 2024; Physical Intelligence et al., 2025; Bjorck et al., 2025; Shukor et al., 2025; Yuan et al., 2026; Ma et al., 2026).

The strongest VLAs today are pretrained partly on proprietary data, and neither these data nor the pretraining pipelines are released (Physical Intelligence et al., 2025; Yuan et al., 2026), making it hard to reproduce their results. Since each new model changes architecture, data and training recipe at once, gains cannot be traced back to a single choice. With REKI, we release the full pipeline, from data curation to deployment. REKI has a simple design, is pretrained on public data only and sets a new Pareto frontier between performance and model size. It pairs a VLM with a flow-matching action expert, and a memory mechanism gives the policy access to its recent past.

Preserving past context can improve control (Octo Model Team et al., 2024; Liu et al., 2025; Chi et al., 2025), yet most VLAs predict actions from the current observation alone (Kim et al., 2024; Black et al., 2024; Physical Intelligence et al., 2025; Bjorck et al., 2025; Shukor et al., 2025; Lee et al., 2025; Qu et al., 2025; Liu et al., 2026). Existing ways of incorporating such context, however, often introduce substantial overhead (Fig. 2): feeding past frames to the VLM adds their image tokens to every VLM pass (Jang et al., 2025; Yuan et al., 2026), recurrent methods require sequential training (Cherepanov et al., 2026; Jiang et al., 2026), and others introduce additional, dedicated memory modules to the policy (Shi et al., 2026; Koo et al., 2026).

REKI summarises past context through a small set of registers (Darcet et al., 2024) in the action expert. The registers of each frame are cached across chunks (Fig. 3). Each frame is thus encoded only once and no separate module, sequential unroll during training or test-time update mechanisms are necessary. This yields clear gains at almost no overhead, and our 2.4B model REKI ranks first among VLAs pretrained on public data on LIBERO-Plus, RoboTwin Clean2Rand and RoboCasa365 (Fig. 1), and generalises to real robots.

Our ablations show that memory consistently improves performance, with the most pronounced gains for the first past frame (Fig. 5). Beyond task success, we find that memory stabilises action generation: without memory, the policy often oscillates between poses across action chunks, while with memory, such oscillations are about four times rarer (Fig. 5).

Our contributions are thus three-fold: (i) We introduce REKI, a 2.4B-parameter VLA pretrained on public data, which sets a new Pareto frontier between performance and model size. (ii) We introduce register tokens in the action expert to act as a memory, which can be cached efficiently, and analyse how this reduces the failure modes of the memoryless policy. We hope REKI serves as a strong and accessible baseline for future VLA research. To foster VLA research, we release the model weights and our optimised training and inference code, from data curation to deployment on a real robot.

## 2 Related work

Open pretraining. Many VLAs release their weights, and often their finetuning code, but not the code or the exact data mixture used for pretraining (Physical Intelligence, 2025; Bjorck et al., 2025; Liu et al., 2026; Zheng et al., 2025; Chen et al., 2026). The strongest VLAs are pretrained partly on proprietary data, collected (Physical Intelligence et al., 2025; Wang et al., 2026a; Liu et al., 2026) or synthesised (Yuan et al., 2026), and several have no public weights (Physical Intelligence et al., 2026; Wang et al., 2026a; Yuan et al., 2026). OpenVLA, Octo and RDT-1B released their full pipeline on public data such as Open X-Embodiment (Kim et al., 2024; Octo Model Team et al., 2024; Liu et al., 2025), followed by SmolVLA, MolmoAct and MolmoAct2, which also releases bimanual data collected by its authors (Shukor et al., 2025; Lee et al., 2025; Fang et al., 2026). These releases have helped the research community pretrain models again, change their data or design, and compare them under the same conditions. Models pretrained on larger in-house data, with more recent design choices, have nonetheless kept a lead on most benchmarks. Building on this line of work, REKI is pretrained on a broad mixture of public robot, egocentric human and vision–language data, and released in full, from data curation to deployment on a real robot. It reaches the best results among VLAs pretrained on public data on three benchmarks (Section 6.1), and is optimised for fast inference (Section 6.3).

Memory in VLAs. Most of these models act on the current observation alone, yet past context helps a policy identify the task state and recover from failed attempts (Shi et al., 2026; Torne et al., 2026; Gao et al., 2026), and methods differ in where they pay for it (Fig. 2). The most direct approach stacks or compresses past frames in the VLM (Yuan et al., 2026; Torne et al., 2026; Physical Intelligence et al., 2026; Jang et al., 2025). Its cost then grows with the window, at least in the vision encoder (Koo et al., 2026). A recurrent state (Cherepanov et al., 2026; Xiao et al., 2026) or fast weights (Jiang et al., 2026) keep the input short, but must be unrolled step by step in training. Others store past features or keyframes in an extra module (Shi et al., 2026; Koo et al., 2026; Mark et al., 2026; Sridhar et al., 2026). Like VLA and HAMLET, REKI carries the past in a few learnable tokens, but as registers (Darcet et al., 2024) in the action expert. The VLM encodes each frame once, a training window is one batched pass, and at inference memory adds under 1% to the prediction time of a model without memory (, Section A.1). Without memory, a policy can also switch between modes at each new action chunk (Agarwal et al., 2026; Lian et al., 2026), which is attributed to perceptual aliasing in reinforcement learning (Whitehead & Ballard, 1991; Singh et al., 1994; McCallum, 1996). In REKI, memory makes such switches about four times rarer (Section 6.2), without the extra policy queries of temporal ensembling or bidirectional decoding (Zhao et al., 2023b; Liu et al., 2024b).

## 3 Background

Problem formulation. At each decision step  we predict the chunk  of the next  actions of an embodied system, conditioned on the *current context*  with

|  |  |  | (1) | 

which gathers a text prompt , camera observations and the current robot state . More generally, a policy may condition on every context observed so far, .

The camera observations  gather the  available views, static or embodied.
The robot state  is expressed as *absolute joint angles* or an *absolute end-effector pose*, and the actions as *absolute joint angles* or *delta end-effector poses* and yield the next  states at a given control frequency.
Action and state dimensions (, ) vary across embodiments and control modes.

### 3.1 Diffusion policy

Following prior work (Chi et al., 2025; Black et al., 2024), the action expert generates the chunk by flow matching (Lipman et al., 2023). Write for the chunk generated at step and for the contexts observed up to it; the index is fixed in this subsection and omitted. Given noise and a flow time , the interpolant moves from the clean chunk at to noise at , and the expert is trained to regress its constant velocity ,

|  |  |  | (2) | 

The squared error is averaged over the valid steps and action dimensions of the chunk, so that every embodiment has the same weight whatever its number of action dimensions. The action loss is the expectation of this term over the dataset of (context, chunk) pairs,

|  |  |  | (3) | 

At inference, is integrated from to 0 in ten Euler steps.

### 3.2 Conditioning

The action expert is conditioned on the camera observations, the text instruction and the robot state, through two separate paths.
The observations and the instruction are variable-length token sequences.
An encoder embeds them into an *encoded context*

|  |  |  | (4) | 

whose tokens the expert reads as keys and values in its cross-attention blocks.
The state is a single low-dimensional vector.
It is pooled into one *state embedding* , which modulates the expert through its adaptive normalisation (Peebles & Xie, 2023), alongside the flow-time embedding.
Since  does not depend on , it is computed once per frame for all Euler steps.

Encoded context. Following recent practice (Yuan et al., 2026; Yang et al., 2026), is the final-layer hidden states of a VLM, read by cross-attention. This keeps the encoder and the action expert independent, and is reported to match or outperform alternatives such as processing the encoded context jointly with the noisy chunk by self-attention (Wang et al., 2026a), or attending the backbone at single or all layers (Black et al., 2024; Physical Intelligence et al., 2025; Shukor et al., 2025; Bjorck et al., 2025). We use Qwen3.5-2B (Qwen Team, 2026) and pack the prompt and the camera views into a single stream (Appendix Fig. 9). The prompt carries the language instruction, the embodiment, the control mode, the control frequency and a binned episode length (Appendix A). The embodiment, control-frequency and episode-length fields are dropped with probability 0.15, 0.2 and 0.2 during training. Each view is preceded by a learned camera-role token (side, overhead, wrist left, wrist right) and embedded by the VLM’s visual encoder at native resolution. Views above pixels are first downscaled with their aspect ratio preserved. Views are shuffled and randomly dropped during training.

State embedding. is one linear layer with a GELU per state layout, followed by a shared zero-initialised projection at the expert’s width. Its output modulates the expert through the adaptive normalisation. The state is dropped with probability 0.2 during training.

## 4 REKI model

Building on the policy of Section 3, REKI gives the action expert a memory of prior frames through registers (Section 4.1), which are trained end to end with the flow-matching objective and cached at inference (Section 4.2).

### 4.1 Preserving past context

Standard VLAs condition on the current context alone. We give the expert access to the preceding frames through memory tokens per frame, implemented as registers (Darcet et al., 2024). The registers of frame cross-attend the encoded prompt and views of that frame, self-attend the registers of the earlier frames, and persist as keys and values in the expert’s cache, implemented as a rolling window over the preceding frames with (Section 4.2). Since they do not attend the action tokens and skip the adaptive normalisation, they depend only on the observations and can be shared across frames and denoising steps.

Cost and consistency. The memory is added without changing the encoder, which still processes one frame at a time, so the encoder cost is that of a memoryless policy. During training, the frames are encoded in a single batched pass with no recurrence to unroll, and each frame is supervised on its own chunk (Cherepanov et al., 2026; Jiang et al., 2026), so a window of frames yields training signals rather than one (Fig. 2).

Registers. As in Qwen-RobotManip (Yuan et al., 2026), learnable register tokens are prepended to the noisy actions in the expert’s stream, so that frame enters it as , with the actions linearly projected to the expert’s width ; we write for the register tokens of frame . While Qwen-RobotManip discards the registers after each forward pass, we keep their keys and values across frames, giving the action expert access to past observations.

Attention. Every token of cross-attends . In self-attention, writing for attention with queries over the tokens of as keys and values,

|  |  |  | (5) | 

The history thus enters the velocity field through the attention support alone (Fig. 3b).

### 4.2 Training and inference

Objective. A training sample is a window of frames from one episode, taken at a 1 s stride and indexed from 1, the oldest, to , the most recent. At the start of an episode, a window holds fewer frames. Each frame has its own chunk , and its contexts cover the frames up to it, without past states. The loss averages the flow-matching loss of Eq. 2 over the chunks of all frames, each conditioned on the contexts up to its frame,

|  |  |  | (6) | 

Since every later frame of the window reads the registers of a frame, these registers receive gradient from all of their losses. At pretraining, the LM head of the VLM is also trained to predict the subtask when one is annotated and the answers of the vision–language data, with a cross-entropy loss of weight 0.1 (Appendix Fig. 9).

Multi-draw variance reduction. The loss of Eq. 2 is an expectation over the flow time and the noise, and a single draw per frame gives a noisy estimate of it. An extra draw costs one pass of the expert, whereas the context and the registers it reads cost one pass of the encoder, which dominates the cost of a training step. As in Yuan et al. (2026) and Allshire et al. (2026), we therefore estimate the expectation from eight independent draws per frame. The eight noisy chunks share the encoded context and the registers of their frame, so the encoder runs once for all of them. The registers of frame thus receive gradient from the eight draws of every frame from to .

Implementation. The frames of a window are packed into a single batch and kept independent in the backbone by cu_seqlens boundaries. In the expert, the self-attention of Eq. 5 is computed by one variable-length FlashAttention call. For this, the keys and values that each group of queries reads are gathered into one sequence: the register tokens for the registers of frame , and followed by the action tokens of one noisy chunk for the queries of that chunk (Appendix Fig. 9(b)).

Inference. At inference, each frame needs one encoder pass over the current observation and ten Euler steps on the action tokens. The registers do not read the action tokens and skip adaptive normalisation (Section 4.1), so neither they nor the context depend on the flow time or on the state. The registers of the current frame are thus computed together with the action tokens in the first Euler step, and their keys and values are reused at the nine others. The register keys and values are also kept for the frames that follow, pairs per expert block. The cache is a rolling window, so the registers of the oldest frame are dropped when those of a new frame enter (Appendix Fig. 10). At training, each window of frames is processed in isolation. At inference, the cached registers were themselves computed with the registers of earlier frames, so information can persist beyond the window. The VLM prefill and the expert are each compiled into a single CUDA graph. With two images and a chunk of ten actions, REKI predicts a chunk in 21.8 ms on one H100 at , the lowest latency among the VLAs compared in Section 6.3. In the measurement of Fig. 5, the VLM prefill takes 9.5 ms and the Euler steps 8.1 ms, and reading the cached registers adds 0.1 ms.

## 5 Experimental setup

### 5.1 Pretraining data

Robot manipulation. We train on a broad mixture of public teleoperation datasets: RoboSet (Bharadhwaj et al., 2024), Galaxea (Jiang et al., 2025), AgiBot World 2026 (AgiBot World Team, 2026), RoboCOIN (Wu et al., 2025b), RoboMIND (Wu et al., 2025a), RDT-1B (Liu et al., 2025), DROID (Khazatsky et al., 2024), BridgeData V2 (Walke et al., 2023), and twelve datasets of Open X-Embodiment (Open X-Embodiment Collaboration, 2024) including RT-1 (Brohan et al., 2023) and BC-Z (Jang et al., 2022) (Section B.1), complemented by motion-planned simulation from InternData-A1 (Tian et al., 2026) and GR00T-X-Embodiment-Sim (Bjorck et al., 2025) and bimanual YAM demonstrations from ABC (Allshire et al., 2026). These span single-arm, bimanual, mobile, and humanoid platforms across tabletop, in-the-wild, and loco-manipulation settings (Appendix Table 1). Action and state dimensions, control conventions and units vary across sources, and a robot may appear under several conventions. Following Qwen-VLA (Wang et al., 2026a), we keep each dataset’s native control convention rather than remapping to a shared action space, normalise each action dimension to by per-dataset quantiles, and zero-pad actions to a common width under a loss mask. States are absolute, actions are absolute joint positions or delta end-effector poses, and rotations are harmonised to one Euler parameterisation. The prompt and the per-layout state projections of Section 3.2 tell the policy which convention a sample follows. In the spirit of Qwen-RobotManip’s multi-stage filtering (Yuan et al., 2026), we curate every source before training: we keep only the degrees of freedom the policy commands, discard static and near-zero-variance episodes, and correct per-dataset sign conventions. Section B.2 gives the curation stages and the action representation.

Egocentric human manipulation. We add three egocentric human manipulation datasets, EgoDex (Hoque et al., 2026), EgoVerse (Punamiya et al., 2026) and Xperience (Ropedia, 2026), recorded from head-mounted or hand-held cameras. They contribute k episodes and 170M frames. The action target follows Qwen-VLA’s 32D bimanual layout (Wang et al., 2026a), wrist motion and hand articulation per hand, here with the wrist expressed in the camera frame; no proprioceptive state is used, and each is resampled to the pace of robot teleoperation (Yuan et al., 2026). Section B.4 gives the sources, target definition and resampling.

Vision–language data. We train on the FineVision (Wiedmann et al., 2025) and LLaVA-OneVision-1.5 (An et al., 2025) datasets most relevant for spatial reasoning (Section B.5), as well as targeted spatial-grounding and pointing corpora: RoboPoint (Yuan et al., 2024), RefSpatial (Zhou et al., 2025), and PixMo (Deitke et al., 2025). This auxiliary supervision preserves the backbone’s perception and spatial reasoning, and is mixed in at a small ratio with the robot and egocentric data.

### 5.2 Models

REKI pairs the Qwen3.5-2B vision–language backbone with an action expert of 10 blocks of width 768, 32 registers per frame and a memory window of  frames.
It is pretrained on the mixture above for 200k steps on 32 H100 GPUs with a global batch of 2,880 samples, then finetuned on each benchmark separately.
The backbone is trained at  the learning rate of the expert, and 10% of the training samples are vision–language data.
Finetuning steps, node counts and the number of evaluation rollouts per task are in Appendix Table 3, the remaining hyperparameters in Appendix Table 2.
Rows marked *scratch* skip the pretraining and finetune the initialised model directly on the benchmark.<sup>1</sup><sup>1</sup>
              1
              
              
              
              
              
              
              
            Baseline numbers are taken from published results (Appendix Table 7).
The training budgets published for these baselines are in Appendix Table 9.
Models in italics in the tables are not released at the time of writing.

### 5.3 Visual data augmentation

We apply data augmentation to make the model robust to visual variations: colour jittering, mild random cropping, random camera dropout and shuffled camera order. When finetuning on RoboTwin, RoboCasa365 and RoboCasa-GR1, we add a mirroring augmentation, which extends the horizontal flip common in image classification to robot data. With probability 0.5, a sample is replaced by its left-right mirror image. The images are flipped, the state and the actions are reflected through the same plane, the arms of a two-arm robot are swapped, and the prompt marks the sample as mirrored (Section C.2). The augmentations and other training settings, such as the state dropout rate, have a large effect on robustness: on LIBERO-Plus, disabling camera dropout or the image augmentations lowers the success under camera-viewpoint changes by more than 20 points, and disabling state dropout lowers the success under changes of the initial robot state by 8.1 points (Section D.2).

## 6 Experiments

### 6.1 Results

Benchmarks. LIBERO (Liu et al., 2023a) has four suites of ten single-arm tabletop tasks, and LIBERO-Plus (Fei et al., 2025) perturbs its 40 tasks along seven dimensions, from the camera viewpoint and the initial robot state to lighting and object layout. The models are finetuned on the four suites and evaluated on both. RoboTwin 2.0 (Chen et al., 2025b) has 50 tasks on the dual-arm AgileX ALOHA, in clean scenes and in scenes with a randomised background, lighting, clutter and table height. For the out-of-distribution evaluation (Clean2Rand; Yuan et al., 2026), the models are finetuned on clean scenes only and evaluated under six conditions: on clean scenes (Easy), under each randomisation alone and under all four together (Hard). The Clean2Rand score is the mean over these six conditions. RoboCasa365 (Nasiriany et al., 2026) has kitchen tasks for a Franka arm on a mobile base. The models are trained on the human demonstrations of its 300 pretraining tasks and evaluated on 50 target tasks: 18 atomic tasks, 16 composite tasks seen in training and 16 composite tasks unseen in training. RoboCasa-GR1 (Bjorck et al., 2025) has 24 tabletop tasks for the Fourier GR-1 humanoid, with 1000 demonstrations per task. We compare mainly with VLAs of similar complexity, which pair a VLM with an action expert, such as , GR00T, ABot-M0 and InternVLA-A1.5. As a gauge of the current upper bound on these benchmarks, we also report larger models pretrained on in-house data, Qwen-RobotManip, RLDX-1 and Xiaomi-Robotics-1, and the world-action model ABot-M0.5, built on a video generation model. They are trained under different conditions, with more or proprietary data or a video model, so they are a reference rather than a like-for-like comparison. Figure 4 summarises the results across benchmarks, and Appendix Tables 4, 5 and 6 list them in full.

Out of distribution. REKI stands out most when the test conditions differ from training. On LIBERO-Plus and Clean2Rand, it ranks first among the VLAs pretrained on public data, despite having the fewest parameters (2.4B, Figs. 4 and 4). It leads the next of these models by 5.6 points on LIBERO-Plus (InternVLA-A1.5, 84.8) and by 10.1 points on Clean2Rand (ABot-M0, 55.7). On the composite tasks of RoboCasa365 unseen in training, it is on par with Qwen-RobotManip (14.1 against 14.9) and behind Xiaomi-Robotics-1 (32.1), both larger and pretrained on in-house data.

In distribution. When trained and evaluated on the same tasks, REKI is competitive with larger models. On RoboCasa365, it reaches 38.4, ahead of the larger RLDX-1 (36.0) and Qwen-RobotManip (35.9), both pretrained on in-house data (Fig. 4), and on RoboCasa-GR1 it reaches 58.6, on par with the larger RLDX-1 (58.7) and ABot-M0 (58.3) (Fig. 4). On LIBERO, it reaches 97.5, where most models lie between 97.6 and 99.4. REKI is weakest on RoboTwin, where it reaches 85.9 and 84.6, below the similarly sized InternVLA-A1.5 (93.3 / 93.0), which is supervised by an additional video generation model during training and uses a more complex, multi-stage pretraining (Fig. 4).

Pretraining gains. Trained from scratch on each benchmark, REKI reaches 52.0 on Clean2Rand, on par with Qwen-RobotManip (52.3) and ahead of StarVLA (36.6), which both have larger VLMs (4.5B and 4.4B). REKI is ahead of both under the Hard condition (26.3 against 22.6 and 10.6). On LIBERO-Plus it leads with 86.1, ahead of Qwen-RobotManip (78.3) and StarVLA (75.0). Pretraining on public data benefits REKI most out of distribution, and in particular under the RoboTwin randomisations (Appendix Tables 5 and 4). In distribution, it adds 2.9 and 0.8 points on RoboTwin and 1.0 on LIBERO. Out of distribution, it adds 13.8 points on Clean2Rand and 4.3 on LIBERO-Plus, and the gain grows where REKI without pretraining fails most, with 30.3 points under clutter and 22.4 under the Hard condition. On LIBERO-Plus, the gain is also largest under perturbed robot states (11.7).

Additional data or supervision. REKI is outperformed by several larger and more recent models that add data or supervision to the vanilla recipe, with large in-house data collections (Xiaomi Robotics Team, 2026) or video generation models (Song et al., 2026; Zhang et al., 2026). Similarly, InternVLA-A1.5 uses the same VLM as REKI, with an additional video generation model, a larger action expert (24 layers vs 10 for REKI) and a multi-stage pretraining, and is ahead in distribution on RoboTwin (93.3 vs 85.9). Out of distribution, it is behind on LIBERO-Plus (84.8 vs 90.4). Qwen-RobotManip, pretrained with additional synthetic data, gains more from pretraining than REKI (16.2 vs 13.8 points on Clean2Rand). REKI reaches its lead out of distribution without such additions, and we expect it to benefit similarly from additional data, be it real (Xiaomi Robotics Team, 2026), synthetic (Yuan et al., 2026) or generated by a world model (Kim et al., 2026; GigaBrain Team et al., 2026).

### 6.2 Ablation

Memory window. Figure 5 varies the memory window of REKI at finetuning and at evaluation, on LIBERO-Plus after finetuning on LIBERO, on RoboTwin Clean and on RoboCasa365. Finetuned and evaluated with three frames (), REKI reaches 90.4 on LIBERO-Plus, 73.4 on RoboTwin and 38.4 on RoboCasa365, against 88.9, 67.9 and 33.0 without memory (). Most of the gain already comes with the first past frame (: 90.1, 73.6 and 39.3). As every frame of a training window is supervised (Eq. 6), the model finetuned with memory also learns to act without it. Evaluated without memory (), it performs close to the model finetuned at (88.9, 67.2 and 34.9), so the gain comes mainly from reading the past frames at inference, not from training on windows.

Oscillation. For each arm, we compare how it moves during one chunk with how it moved during the previous one, ignoring small movements. Figure 5 counts the episodes in which an arm moves in roughly the opposite direction at three or more consecutive chunks, that is, it goes back and forth at least twice (Fig. 7). We set this criterion on 16 of the 50 tasks and confirmed the effect of memory on the other 34. Fewer than 5% of these episodes succeed. With the same weights, removing the past frames at evaluation raises their share from 4.1% to 14.8%, and one past frame already brings it down to 4.6%. Trained without memory, REKI oscillates in 18.6% of the episodes, and in half of those that fail. The difference is largest in two-arm tasks (Fig. 5).

Why memory helps. A policy without memory maps the current observation to the next chunk. If two poses A and B each look like demonstration states that continue towards the other, the chunk predicted at A leads to B, and the chunk predicted at B leads back to A. We attribute the oscillation to this form of perceptual aliasing, under which memoryless policies are known to fail (Whitehead & Ballard, 1991; Singh et al., 1994; McCallum, 1996). A short history resolves the ambiguity, since a window that has just come from B does not match demonstrations that move towards B. In two-arm tasks, the most frequent cycle is an arm swap, in which one arm reaches while the other retracts (65 against 1 of 1000 episodes without and with memory, top row of Fig. 7). It typically starts a few seconds after a release. Memory does not rescue all these episodes: where the policy without memory oscillates, the policy with memory succeeds on 45% of the seeds.

### 6.3 Inference latency

Protocol. We measure the latency of one call, from the raw observation to the unnormalised actions of one chunk, including preprocessing and the transfers between host and device. This latency bounds how quickly a policy can react to a new observation and how often it can replan, whatever the number of actions it predicts. In practice, policies replan well before the end of a long chunk, typically in under a second, since executing more actions open loop makes them slower to react (Black et al., 2024; Chi et al., 2025; Liu et al., 2024b). Each model predicts one chunk from two images ( for ABot-M0.5), the robot state and a fixed prompt, at batch size one, with its released checkpoint and inference code. Each value in Fig. 6 is the median over 320 chunks (80 for ABot-M0.5) and three repetitions, each on its own GPU of a node reserved for the timing (Section D.4).

Compilation. Most of the compared models are released without a compiled path, or with one that can be further optimised. Without compilation, all models are slow (Fig. 6, left): REKI, with openpi (Physical Intelligence, 2025) and ABot-M0 (Yang et al., 2026) take 118 to 139 ms per chunk, InternVLA-A1.5 (Ma et al., 2026) 352 ms, and X-VLA (0.9B; Zheng et al., 2025) 51 ms. In most of these runs, the GPU is busy less than half of the time, so compiling the prediction, which removes most of the host time, makes it two to seven times faster (Fig. 6, right). We compile REKI entirely into CUDA graphs, including its memory (Section A.1). For a fair comparison, we also compile X-VLA, ABot-M0, InternVLA-A1.5 and ABot-M0.5 (Chen et al., 2026) without changing their computation, and report both their released path and ours.

Results. With compilation, REKI is the fastest of the compared models (Fig. 6, right): it predicts a chunk in 21.8 ms, against 25.5 ms for X-VLA, 33.8 ms for with openpi, 46.1 ms for ABot-M0 and 51.3 ms for InternVLA-A1.5. Its GPU time per chunk, 16.5 ms, is on par with that of X-VLA (16.1 ms) and below that of the other models, which need at least 25.3 ms. InternVLA-A1.5 uses the same VLM as REKI, and the difference lies in its action expert, which runs at each of the ten Euler steps: with 0.5B parameters in 24 layers against 0.2B in 10, its Euler steps take 34.9 ms against 8.2 ms for REKI (Appendix Table 10). ABot-M0.5 generates future video latents before it predicts actions, and takes 2.7 s per chunk without compilation and 1.4 s with the CUDA graphs that we built.

## 7 Real robot experiments

Experimental setup. We use a 6-DoF UR5 robotic arm with a Weiss WSG50 gripper and UMI-style parallel jaws. The action loop runs at 15 Hz. The robot operates over a 1 m1 m tabletop workspace observed by a fixed Orbbec Femto Mega RGB camera, see Fig. 8, left. Observations include an RGB image, and a 7D absolute joint state. Actions are specified as 7D absolute joint state.

Tasks and protocol. We evaluate performance across five manipulation tasks, and report task success rates and partial subtask success rates. Tasks span diverse action primitives, including pick, place, stack, and sweep. The tasks and partial success scores are defined in Appendix E. We collect 50 training episodes per task with a Meta Quest controller, and evaluate on 10 test episodes per task, with randomised initial object placements and distractors in each episode.

Baselines. We finetune the pretrained checkpoints of REKI and on the five tasks for 3,000 optimiser steps, with a global batch of 360 samples. Both models use a learning rate of , held constant after warmup. The action chunk has 50 steps (3.3 s) for , fixed by its pretraining, and 30 steps (2 s) for REKI. In both models, only the last four vision encoder blocks are trained. The backbone is trained at the learning rate of the action expert. REKI is finetuned with a memory window of and without memory (). has no memory.

Results. Across the five tasks, REKI outperforms by 7 to 37.5 percentage points (Fig. 8, right). Removing the memory leaves performance on the shorter-horizon tasks mainly unchanged (65 against 65 and 57.5 against 57.5), but consistently degrades it on the longer-horizon tasks, where REKI outperforms its memoryless variant by 12 to 17.5 percentage points, supporting that memory matters most when tasks require tracking progress over many steps. For qualitative examples, see Fig. 12.

## 8 Discussion

A vanilla VLA. REKI follows a standard VLA recipe: a VLM’s last hidden states are read by a flow-matching action expert, pretrained in a single stage on public robot, egocentric human and vision–language data, and then finetuned on each benchmark. It uses no proprietary data (Yuan et al., 2026; Kim et al., 2026, cf.), no world model (Chen et al., 2026, cf.) and no additional training stages. Even so, it can compete with, and often outperform, VLAs of similar size pretrained on in-house data, and it leads the compared VLAs pretrained on public data out of distribution (Fig. 4), where pretraining adds 13.8 points on Clean2Rand and 4.3 on LIBERO-Plus (Section 6.1). Many of the strongest entries on current leaderboards take the other route: they are world-action models built on video generation models (Chen et al., 2026; Wang et al., 2026b; Zhang et al., 2026; Song et al., 2026) or VLAs pretrained on large proprietary data (Xiaomi Robotics Team, 2026; GigaBrain Team et al., 2026). These models set the current state of the art, but they can be costly to run (Section 6.3). Their pretraining is also hard to reproduce, which makes it difficult to separate the effects of data scale, model size, modelling choices and additional supervision such as world modelling. We intend REKI to serve as an open reference on which each of these factors can be tested in isolation.

An efficient open baseline. We release REKI with its full pipeline, from data curation to deployment, and the checkpoints of every benchmark, so that new architectures, data mixtures or training objectives can be pretrained and compared against it under fixed conditions. The simplicity of the recipe and the efficiency of the code keep such comparisons affordable: finetuning REKI takes 8 H100 GPU-hours on our real-robot tasks and 40 to 400 on the benchmarks (Tables 11 and 3). The memory adds no cost at training (Section 4.2) and less than 1% of the inference time, so REKI is an efficient starting point for studying memory in embodied AI. In the released code, REKI runs entirely as CUDA graphs, memory included, and predicts an action chunk from the LIBERO inputs in 21.8 ms on one H100. It stays the fastest of the compared models even when their released code is compiled (Section 6.3).

Why inference speed matters. Inference speed matters not only at deployment, where it bounds how quickly a policy can react (Section 6.3), but also for training. VLAs are increasingly post-trained with reinforcement learning, in simulation (Li et al., 2025; Chen et al., 2025a; Zang et al., 2026) or in learned world models (Zhu et al., 2025; Sharma et al., 2026), where the policy is queried for every action chunk of every rollout, so that its inference time is multiplied across the whole training run. At 21.8 ms per chunk, REKI offers a strong open starting point for such post-training and lowers the barrier to it, a step towards making it accessible beyond the few groups with large compute budgets.

Memory. In REKI, a few register tokens in the action expert summarise each frame and stay in the expert’s cache, so the VLM still sees each frame only once. Even this short memory helps: with three frames, success rises by 1.5 to 5.5 points on LIBERO-Plus, RoboTwin and RoboCasa365, and by 12 to 17.5 points on the longer real-robot tasks. Most of the gain comes from a single past frame, which in particular reduces repetitive oscillations between two poses, suggesting that a little temporal context is enough to tell apart observations that look alike (Section 6.2). A memory of a few seconds therefore appears sufficient for most tasks in standard benchmarks. Whether the registers can also carry the much longer context of tasks that depend on events far in the past remains to be tested.

## References

- Acharya et al. (2019)
Manoj Acharya, Kushal Kafle, and Christopher Kanan.
TallyQA: Answering complex counting questions.
In *Proc. AAAI Conf. Artif. Intell.* , 2019.
- Agarwal et al. (2026)
Abhinav Agarwal, Adam Wei, Taylan Kargin, Michael Zeng, Cole Becker, Arif Kerem
Dayı, Pablo Parrilo, Asuman Ozdaglar, and Russ Tedrake.
Training and evaluating diffusion policies with long context lengths.
*arXiv:2606.16447* , 2026.
- AgiBot World Team (2026) AgiBot World Team. AgiBot World 2026. Hugging Face dataset, 2026. URL https://huggingface.co/datasets/agibot-world/AgiBotWorld2026.
- Allshire et al. (2026)
Arthur Allshire, Himanshu Gaurav Singh, Ritvik Singh, Adam Rashid, Hongsuk
Choi, David McAllister, Justin Yu, Yiyuan Chen, Huang Huang, Pieter Abbeel,
et al.
Scalable behavior cloning with open data, training, and evaluation.
*arXiv:2606.27375* , 2026.
- An et al. (2025)
Xiang An, Yin Xie, Kaicheng Yang, Wenkang Zhang, Xiuwei Zhao, Zheng Cheng,
Yirui Wang, Songcen Xu, Changrui Chen, Chunsheng Wu, et al.
LLaVA-OneVision-1.5: Fully open framework for democratized
multimodal training.
*arXiv:2509.23661* , 2025.
- Bharadhwaj et al. (2024)
Homanga Bharadhwaj, Jay Vakil, Mohit Sharma, Abhinav Gupta, Shubham Tulsiani,
and Vikash Kumar.
RoboAgent: Generalization and efficiency in robot manipulation via
semantic augmentations and action chunking.
In *Proc. IEEE Int. Conf. Robot. Autom.* , 2024.
- Biten et al. (2019)
Ali Furkan Biten, Ruben Tito, Andres Mafla, Lluis Gomez, Marçal Rusinol,
Ernest Valveny, C. V. Jawahar, and Dimosthenis Karatzas.
Scene text visual question answering.
In *Proc. IEEE/CVF Int. Conf. Comput. Vis.* , 2019.
- Bjorck et al. (2025)
Johan Bjorck, Fernando Castañeda, Nikita Cherniadev, Xingye Da, Runyu Ding,
Linxi Fan, Yu Fang, Dieter Fox, Fengyuan Hu, et al.
GR00T N1: An open foundation model for generalist humanoid robots.
*arXiv:2503.14734* , 2025.
- Black et al. (2024)
Kevin Black, Noah Brown, Danny Driess, Adnan Esmail, Michael Equi, Chelsea
Finn, Niccolo Fusai, Lachy Groom, Karol Hausman, Brian Ichter, et al.
: A vision-language-action flow model for general robot
control.
*arXiv:2410.24164* , 2024.
- Brohan et al. (2023)
Anthony Brohan, Noah Brown, Justice Carbajal, Yevgen Chebotar, Joseph Dabis,
Chelsea Finn, Keerthana Gopalakrishnan, Karol Hausman, Alex Herzog, Jasmine
Hsu, et al.
RT-1: Robotics transformer for real-world control at scale.
In *Robotics: Science and Systems* , 2023.
- Cadene et al. (2024) Remi Cadene, Simon Alibert, Alexander Soare, Quentin Gallouedec, Adil Zouitine, Steven Palma, Pepijn Kooijmans, Michel Aractingi, Mustafa Shukor, Dana Aubakirova, Martino Russi, Francesco Capuano, Caroline Pascal, Jade Choghari, Khalil Meftah, Maxime Ellerbach, Jess Moss, and Thomas Wolf. LeRobot: State-of-the-art machine learning for real-world robotics in PyTorch. https://github.com/huggingface/lerobot, 2024.
- Carter (2024) Jimmy Carter. TextOCR-GPT4V. Hugging Face dataset, 2024. URL https://huggingface.co/datasets/jimmycarter/textocr-gpt4v.
- Chen et al. (2024a)
Guiming Hardy Chen, Shunian Chen, Ruifei Zhang, Junying Chen, Xiangbo Wu, Zhiyi
Zhang, Zhihong Chen, Jianquan Li, Xiang Wan, and Benyou Wang.
ALLaVA: Harnessing GPT4V-synthesized data for lite
vision-language models.
*arXiv:2402.11684* , 2024a.
- Chen et al. (2025a)
Kang Chen, Zhihao Liu, Tonghe Zhang, Zhen Guo, Si Xu, Hao Lin, Hongzhi Zang,
Xiang Li, Quanlu Zhang, Zhaofei Yu, et al.
: Online RL fine-tuning for flow-based
vision-language-action models.
*arXiv:2510.25889* , 2025a.
- Chen et al. (2024b)
Lin Chen, Jinsong Li, Xiaoyi Dong, Pan Zhang, Conghui He, Jiaqi Wang, Feng
Zhao, and Dahua Lin.
ShareGPT4V: Improving large multi-modal models with better
captions.
In *Proc. Eur. Conf. Comput. Vis.* , 2024b.
- Chen et al. (2026)
Ronghan Chen, Yandan Yang, Zuojin Tang, Dongjie Huo, Tong Lin, Haoning Wu,
Haoyun Liu, Yuzhi Chen, Lulu Zheng, Botai Yuan, et al.
ABot-M0.5: Unified mobility-and-manipulation world action model.
*arXiv:2607.00678* , 2026.
- Chen et al. (2025b)
Tianxing Chen, Zanxin Chen, Baijun Chen, Zijian Cai, Yibin Liu, Zixuan Li,
Qiwei Liang, Xianliang Lin, Yiheng Ge, Zhenyu Gu, et al.
RoboTwin 2.0: A scalable data generator and benchmark with strong
domain randomization for robust bimanual robotic manipulation.
*arXiv:2506.18088* , 2025b.
- Cherepanov et al. (2026)
Egor Cherepanov, Nikita Kachaev, Daniil Zelezetsky, Aydar Bulatov, Artem
Pshenitsyn, Yuri Kuratov, Alexey Skrynnik, Aleksandr I Panov, and Alexey K
Kovalev.
VLA: On recurrent memory for partially observable manipulation
in VLA models.
*arXiv:2606.12497* , 2026.
- Chi et al. (2025)
Cheng Chi, Zhenjia Xu, Siyuan Feng, Eric Cousineau, Yilun Du, Benjamin
Burchfiel, Russ Tedrake, and Shuran Song.
Diffusion policy: Visuomotor policy learning via action diffusion.
*Int. J. Robot. Res.* , 44(10-11):1684–1704,
2025.
- Chng et al. (2019)
Chee Kheng Chng, Yuliang Liu, Yipeng Sun, Chun Chet Ng, Canjie Luo, Zihan Ni,
ChuanMing Fang, Shuaitao Zhang, Junyu Han, and Errui Ding.
ICDAR2019 robust reading challenge on arbitrary-shaped text
(RRC-ArT).
In *Proc. Int. Conf. Doc. Anal. Recognit.* , 2019.
- Darcet et al. (2024)
Timothée Darcet, Maxime Oquab, Julien Mairal, and Piotr Bojanowski.
Vision transformers need registers.
In *Int. Conf. Learn. Represent.* , 2024.
- Deitke et al. (2025)
Matt Deitke, Christopher Clark, Sangho Lee, Rohun Tripathi, Yue Yang, Jae Sung
Park, Mohammadreza Salehi, Niklas Muennighoff, Kyle Lo, Luca Soldaini, et al.
Molmo and PixMo: Open weights and open data for state-of-the-art
vision-language models.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2025.
- Fang et al. (2026)
Haoquan Fang, Jiafei Duan, Donovan Clay, Sam Wang, Shuo Liu, Weikai Huang,
Xiang Fan, Wei-Chuan Tsai, Shirui Chen, Yi Ru Wang, et al.
MolmoAct2: Action reasoning models for real-world deployment.
*arXiv:2605.02881* , 2026.
- Fei et al. (2025)
Senyu Fei, Siyin Wang, Junhao Shi, Zihao Dai, Jikun Cai, Pengfang Qian, Li Ji,
Xinzhe He, Shiduo Zhang, Zhaoye Fei, et al.
LIBERO-Plus: In-depth robustness analysis of vision-language-action
models.
*arXiv:2510.13626* , 2025.
- Gao et al. (2026)
Yihuai Gao, Jeff Jinyun Liu, Shuang Li, and Shuran Song.
Gated memory policy: In-context memorization and adaptation.
*arXiv:2604.18933* , 2026.
- GigaBrain Team et al. (2026)
GigaBrain Team, Angen Ye, Axiang Sun, Can Jin, Chenxi Cheng, Chong Shi,
Dengke Shang, Dingqian Zhang, Guan Huang, Guangqiang Wang, et al.
GigaBrain-0.7: Scaling embodied foundation models to emergent
capabilities with a three-system architecture.
*arXiv:2608.15875* , 2026.
- Goyal et al. (2017)
Yash Goyal, Tejas Khot, Douglas Summers-Stay, Dhruv Batra, and Devi Parikh.
Making the V in VQA matter: Elevating the role of image
understanding in visual question answering.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2017.
- Hessel et al. (2022)
Jack Hessel, Jena D. Hwang, Jae Sung Park, Rowan Zellers, Chandra Bhagavatula,
Anna Rohrbach, Kate Saenko, and Yejin Choi.
The abduction of Sherlock Holmes: A dataset for visual abductive
reasoning.
In *Proc. Eur. Conf. Comput. Vis.* , 2022.
- Hoque et al. (2026)
Ryan Hoque, Peide Huang, David J. Yoon, Mouli Sivapurapu, and Jian Zhang.
EgoDex: Learning dexterous manipulation from large-scale egocentric
video.
In *Int. Conf. Learn. Represent.* , 2026.
- Hudson & Manning (2019)
Drew A. Hudson and Christopher D. Manning.
GQA: A new dataset for real-world visual reasoning and
compositional question answering.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2019.
- Jang et al. (2022)
Eric Jang, Alex Irpan, Mohi Khansari, Daniel Kappler, Frederik Ebert, Corey
Lynch, Sergey Levine, and Chelsea Finn.
BC-Z: Zero-shot task generalization with robotic imitation
learning.
In *Proc. Conf. Robot Learn.* , pp.  991–1002, 2022.
- Jang et al. (2025)
Huiwon Jang, Sihyun Yu, Heeseung Kwon, Hojin Jeon, Younggyo Seo, and Jinwoo
Shin.
ContextVLA: Vision-language-action model with amortized multi-frame
context.
*arXiv:2510.04246* , 2025.
- Jhamtani & Berg-Kirkpatrick (2018)
Harsh Jhamtani and Taylor Berg-Kirkpatrick.
Learning to describe differences between pairs of similar images.
In *Proc. Conf. Empir. Methods Nat. Lang. Process.* , 2018.
- Jiang et al. (2025)
Tao Jiang, Tianyuan Yuan, Yicheng Liu, Chenhao Lu, Jianning Cui, Xiao Liu,
Shuiqi Cheng, Jiyang Gao, Huazhe Xu, and Hang Zhao.
Galaxea open-world dataset and G0 dual-system VLA model.
*arXiv:2509.00576* , 2025.
- Jiang et al. (2026)
Yunfan Jiang, Yevgen Chebotar, Ruijie Zheng, Fengyuan Hu, Yunhao Ge, Jimmy Wu,
Tianyuan Dai, Scott Reed, Li Fei-Fei, Yuke Zhu, et al.
RoboTTT: Context scaling for robot policies.
*arXiv:2607.15275* , 2026.
- Karakus (2024) Hazal Karakus. MSCOCO-ControlNet-Canny-Less-Colors. Hugging Face dataset, 2024. URL https://huggingface.co/datasets/hazal-karakus/mscoco-controlnet-canny-less-colors.
- Khazatsky et al. (2024)
Alexander Khazatsky, Karl Pertsch, Suraj Nair, Ashwin Balakrishna, Sudeep
Dasari, Siddharth Karamcheti, Soroush Nasiriany, Mohan Kumar Srirama,
Lawrence Yunliang Chen, Kirsty Ellis, et al.
DROID: A large-scale in-the-wild robot manipulation dataset.
In *Robotics: Science and Systems* , 2024.
- Kim et al. (2026)
Dongyoung Kim, Huiwon Jang, Myungkyu Koo, Suhyeok Jang, Taeyoung Kim, Beomjun
Kim, Byungjun Yoon, Changsung Jang, Daewon Choi, Dongsu Han, et al.
RLDX-1 technical report.
*arXiv:2605.03269* , 2026.
- Kim et al. (2024)
Moo Jin Kim, Karl Pertsch, Siddharth Karamcheti, Ted Xiao, Ashwin Balakrishna,
Suraj Nair, Rafael Rafailov, Ethan Foster, Grace Lam, Pannag Sanketi, et al.
OpenVLA: An open-source vision-language-action model.
*arXiv:2406.09246* , 2024.
- Koo et al. (2026)
Myungkyu Koo, Daewon Choi, Taeyoung Kim, Kyungmin Lee, Changyeon Kim, Younggyo
Seo, and Jinwoo Shin.
HAMLET: Switch your vision-language-action model into a
history-aware policy.
In *Int. Conf. Learn. Represent.* , 2026.
- LAION (2023) LAION. GPT4V-Dataset. Hugging Face dataset, 2023. URL https://huggingface.co/datasets/laion/gpt4v-dataset.
- Laurençon et al. (2024)
Hugo Laurençon, Léo Tronchon, Matthieu Cord, and Victor Sanh.
What matters when building vision-language models?
In *Adv. Neural Inf. Process. Syst.* , 2024.
- Lee et al. (2025)
Jason Lee, Jiafei Duan, Haoquan Fang, Yuquan Deng, Shuo Liu, Boyang Li, Bohan
Fang, Jieyu Zhang, Yi Ru Wang, Sangho Lee, et al.
MolmoAct: Action reasoning models that can reason in space.
*arXiv:2508.07917* , 2025.
- Lerner et al. (2022)
Paul Lerner, Olivier Ferret, Camille Guinaudeau, Hervé Le Borgne, Romaric
Besançon, José G. Moreno, and Jesús Lovón Melgarejo.
ViQuAE, a dataset for knowledge-based visual question answering
about named entities.
In *Proc. Int. ACM SIGIR Conf. Res. Dev. Inf. Retr.* , 2022.
- Li et al. (2025)
Haozhan Li, Yuxin Zuo, Jiale Yu, Yuhao Zhang, Zhaohui Yang, Kaiyan Zhang,
Xuekai Zhu, Yuchen Zhang, Tianxing Chen, Ganqu Cui, et al.
SimpleVLA-RL: Scaling VLA training via reinforcement learning.
*arXiv:2509.09674* , 2025.
- Li et al. (2024)
Xiaotong Li, Fan Zhang, Haiwen Diao, Yueze Wang, Xinlong Wang, and Lingyu Duan.
DenseFusion-1M: Merging vision experts for comprehensive multimodal
perception.
In *Adv. Neural Inf. Process. Syst.* , 2024.
- Li et al. (2023)
Zhuowan Li, Xingrui Wang, Elias Stengel-Eskin, Adam Kortylewski, Wufei Ma,
Benjamin Van Durme, and Alan L. Yuille.
Super-CLEVR: A virtual benchmark to diagnose domain robustness in
visual reasoning.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2023.
- Lian et al. (2026)
Shijie Lian, Bin Yu, Xiaopeng Lin, Zhaolong Shen, Laurence Tianruo Yang, Yurun
Jin, Haishan Liu, Changti Wu, Hang Yuan, Cong Huang, and Kai Chen.
IntentVLA: Short-horizon intent modeling for aliased robot
manipulation.
*arXiv:2605.14712* , 2026.
- Lindström & Abraham (2022)
Adam Dahlgren Lindström and Savitha Sam Abraham.
CLEVR-Math: A dataset for compositional language, visual and
mathematical reasoning.
*arXiv:2208.05358* , 2022.
- Lipman et al. (2023)
Yaron Lipman, Ricky T. Q. Chen, Heli Ben-Hamu, Maximilian Nickel, and Matt Le.
Flow matching for generative modeling.
In *Int. Conf. Learn. Represent.* , 2023.
- Liu et al. (2023a)
Bo Liu, Yifeng Zhu, Chongkai Gao, Yihao Feng, Qiang Liu, Yuke Zhu, and Peter
Stone.
LIBERO: Benchmarking knowledge transfer for lifelong robot
learning.
In *Adv. Neural Inf. Process. Syst.* , 2023a.
- Liu et al. (2023b)
Fangyu Liu, Guy Emerson, and Nigel Collier.
Visual spatial reasoning.
*Trans. Assoc. Comput. Linguist.* , 11:635–651,
2023b.
- Liu et al. (2024a)
Fuxiao Liu, Kevin Lin, Linjie Li, Jianfeng Wang, Yaser Yacoob, and Lijuan Wang.
Mitigating hallucination in large multi-modal models via robust
instruction tuning.
In *Int. Conf. Learn. Represent.* , 2024a.
- Liu et al. (2023c)
Haotian Liu, Chunyuan Li, Qingyang Wu, and Yong Jae Lee.
Visual instruction tuning.
In *Adv. Neural Inf. Process. Syst.* , 2023c.
- Liu et al. (2025)
Songming Liu, Lingxuan Wu, Bangguo Li, Hengkai Tan, Huayu Chen, Zhengyi Wang,
Ke Xu, Hang Su, and Jun Zhu.
RDT-1B: A diffusion foundation model for bimanual manipulation.
In *Int. Conf. Learn. Represent.* , 2025.
- Liu et al. (2026)
Songming Liu, Bangguo Li, Kai Ma, Lingxuan Wu, Hengkai Tan, Xiao Ouyang, Hang
Su, and Jun Zhu.
RDT2: Exploring the scaling limit of UMI data towards zero-shot
cross-embodiment generalization.
*arXiv:2602.03310* , 2026.
- Liu et al. (2024b)
Yuejiang Liu, Jubayer Ibn Hamid, Annie Xie, Yoonho Lee, Maximilian Du, and
Chelsea Finn.
Bidirectional decoding: Improving action chunking via guided
test-time sampling.
*arXiv:2408.17355* , 2024b.
- Lu et al. (2022)
Pan Lu, Swaroop Mishra, Tanglin Xia, Liang Qiu, Kai-Wei Chang, Song-Chun Zhu,
Oyvind Tafjord, Peter Clark, and Ashwin Kalyan.
Learn to explain: Multimodal reasoning via thought chains for science
question answering.
In *Adv. Neural Inf. Process. Syst.* , 2022.
- Lu et al. (2024)
Yujie Lu, Dongfu Jiang, Wenhu Chen, William Yang Wang, Yejin Choi, and
Bill Yuchen Lin.
WildVision: Evaluating vision-language models in the wild with
human preferences.
In *Adv. Neural Inf. Process. Syst.* , 2024.
- Luo et al. (2026)
Hao Luo, Wanpeng Zhang, Yicheng Feng, Sipeng Zheng, Haiweng Xu, Chaoyi Xu,
Ziheng Xi, Yuhui Fu, and Zongqing Lu.
Being-H0.7: A latent world-action model from egocentric videos.
*arXiv:2605.00078* , 2026.
- Luo et al. (2024)
Run Luo, Haonan Zhang, Longze Chen, Ting-En Lin, Xiong Liu, Yuchuan Wu, Min
Yang, Minzheng Wang, Pengpeng Zeng, Lianli Gao, et al.
MMEvol: Empowering multimodal large language models with
Evol-Instruct.
*arXiv:2409.05840* , 2024.
- Ma et al. (2026)
Haoxiang Ma, Junhao Cai, Xiaoxu Xu, Hao Li, Yuyin Yang, Yang Tian, Jiafei Cao,
Hongrui Zhu, Zherui Qiu, Zhaxizhuoma, et al.
InternVLA-A1.5: Unifying understanding, latent foresight, and
action for compositional generalization.
*arXiv:2607.04988* , 2026.
- Mark et al. (2026)
Max Sobol Mark, Jacky Liang, Maria Attarian, Chuyuan Fu, Debidatta Dwibedi,
Dhruv Shah, and Aviral Kumar.
BPP: Long-context robot imitation learning by focusing on key
history frames.
*arXiv:2602.15010* , 2026.
- McCallum (1996)
Andrew Kachites McCallum.
*Reinforcement Learning with Selective Perception and Hidden
State* .
PhD thesis, University of Rochester, 1996.
- Mishra et al. (2019)
Anand Mishra, Shashank Shekhar, Ajeet Kumar Singh, and Anirban Chakraborty.
OCR-VQA: Visual question answering by reading text in images.
In *Proc. Int. Conf. Doc. Anal. Recognit.* , 2019.
- Nasiriany et al. (2026)
Soroush Nasiriany, Sepehr Nasiriany, Abhiram Maddukuri, and Yuke Zhu.
RoboCasa365: A large-scale simulation framework for training and
benchmarking generalist robots.
In *Int. Conf. Learn. Represent.* , 2026.
arXiv:2603.04356.
- Octo Model Team et al. (2024)
Octo Model Team, Dibya Ghosh, Homer Walke, Karl Pertsch, Kevin Black, Oier
Mees, Sudeep Dasari, Joey Hejna, Tobias Kreiman, Charles Xu, et al.
Octo: An open-source generalist robot policy.
In *Robotics: Science and Systems* , 2024.
- Open X-Embodiment Collaboration (2024)
Open X-Embodiment Collaboration.
Open X-Embodiment: Robotic learning datasets and RT-X models.
In *Proc. IEEE Int. Conf. Robot. Autom.* , 2024.
- OpenGVLab (2024) OpenGVLab. ShareGPT-4o, 2024. URL https://sharegpt4o.github.io/.
- Peebles & Xie (2023)
William Peebles and Saining Xie.
Scalable diffusion models with transformers.
In *Proc. IEEE/CVF Int. Conf. Comput. Vis.* , 2023.
- Physical Intelligence (2025) Physical Intelligence. openpi: Open-source models and packages for robotics. https://github.com/Physical-Intelligence/openpi, 2025.
- Physical Intelligence et al. (2025)
Physical Intelligence, Kevin Black, Noah Brown, James Darpinian, Karan
Dhabalia, Danny Driess, Adnan Esmail, Michael Equi, Chelsea Finn, Niccolo
Fusai, et al.
: A vision-language-action model with open-world
generalization.
*arXiv:2504.16054* , 2025.
- Physical Intelligence et al. (2026)
Physical Intelligence, Bo Ai, Ali Amin, Raichelle Aniceto, Ashwin
Balakrishna, Greg Balke, Kevin Black, George Bokinsky, Shihao Cao, Thomas
Charbonnier, et al.
: A steerable generalist robotic foundation model with
emergent capabilities.
*arXiv:2604.15483* , 2026.
- Pi et al. (2024)
Renjie Pi, Jianshu Zhang, Jipeng Zhang, Rui Pan, Zhekai Chen, and Tong Zhang.
Image textualization: An automatic framework for creating accurate
and detailed image descriptions.
*arXiv:2406.07502* , 2024.
- Pont-Tuset et al. (2020)
Jordi Pont-Tuset, Jasper Uijlings, Soravit Changpinyo, Radu Soricut, and
Vittorio Ferrari.
Connecting vision and language with localized narratives.
In *Proc. Eur. Conf. Comput. Vis.* , 2020.
- Punamiya et al. (2026)
Ryan Punamiya, Simar Kareer, Zeyi Liu, Josh Citron, Ri-Zhao Qiu, Xiongyi Cai,
Alexey Gavryushin, Jiaqi Chen, Davide Liconti, Lawrence Y. Zhu, et al.
EgoVerse: An egocentric human dataset for robot learning from
around the world.
*arXiv:2604.07607* , 2026.
- Qu et al. (2025)
Delin Qu, Haoming Song, Qizhi Chen, Yuanqi Yao, Xinyi Ye, Yan Ding, Zhigang
Wang, Jiayuan Gu, Bin Zhao, Dong Wang, and Xuelong Li.
SpatialVLA: Exploring spatial representations for
visual-language-action model.
In *Robotics: Science and Systems* , 2025.
- Quattoni & Torralba (2009)
Ariadna Quattoni and Antonio Torralba.
Recognizing indoor scenes.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2009.
- Qwen Team (2026) Qwen Team. Qwen3.5: Towards native multimodal agents, February 2026. URL https://qwen.ai/blog?id=qwen3.5.
- Ren et al. (2015)
Mengye Ren, Ryan Kiros, and Richard Zemel.
Exploring models and data for image question answering.
In *Adv. Neural Inf. Process. Syst.* , 2015.
- Ropedia (2026) Ropedia. Xperience-10M: A large-scale egocentric multimodal dataset of human experience. Hugging Face dataset, 2026. URL https://huggingface.co/datasets/ropedia-ai/xperience-10m.
- Schwenk et al. (2022)
Dustin Schwenk, Apoorv Khandelwal, Christopher Clark, Kenneth Marino, and
Roozbeh Mottaghi.
A-OKVQA: A benchmark for visual question answering using world
knowledge.
In *Proc. Eur. Conf. Comput. Vis.* , 2022.
- Shao et al. (2019)
Shuai Shao, Zeming Li, Tianyuan Zhang, Chao Peng, Gang Yu, Xiangyu Zhang, Jing
Li, and Jian Sun.
Objects365: A large-scale, high-quality dataset for object
detection.
In *Proc. IEEE/CVF Int. Conf. Comput. Vis.* , 2019.
- Sharma et al. (2026)
Ansh Kumar Sharma, Yixiang Sun, Ninghao Lu, Yunzhe Zhang, Jiarao Liu, and
Sherry Yang.
World-Gymnast: Training robots with reinforcement learning in a
world model.
*arXiv:2602.02454* , 2026.
- Shi et al. (2026)
Hao Shi, Bin Xie, Yingfei Liu, Lin Sun, Fengrong Liu, Tiancai Wang, Erjin Zhou,
Haoqiang Fan, Xiangyu Zhang, and Gao Huang.
MemoryVLA: Perceptual-cognitive memory in vision-language-action
models for robotic manipulation.
In *Int. Conf. Learn. Represent.* , 2026.
- Shridhar et al. (2021)
Mohit Shridhar, Xingdi Yuan, Marc-Alexandre Côté, Yonatan Bisk, Adam
Trischler, and Matthew Hausknecht.
ALFWorld: Aligning text and embodied environments for interactive
learning.
In *Int. Conf. Learn. Represent.* , 2021.
- Shukor et al. (2025)
Mustafa Shukor, Dana Aubakirova, Francesco Capuano, Pepijn Kooijmans, Steven
Palma, Adil Zouitine, Michel Aractingi, Caroline Pascal, Martino Russi,
Andres Marafioti, et al.
SmolVLA: A vision-language-action model for affordable and
efficient robotics.
*arXiv:2506.01844* , 2025.
- Sidorov et al. (2020)
Oleksii Sidorov, Ronghang Hu, Marcus Rohrbach, and Amanpreet Singh.
TextCaps: A dataset for image captioning with reading
comprehension.
In *Proc. Eur. Conf. Comput. Vis.* , 2020.
- Sima et al. (2024)
Chonghao Sima, Katrin Renz, Kashyap Chitta, Li Chen, Hanxue Zhang, Chengen Xie,
Jens Beißwenger, Ping Luo, Andreas Geiger, and Hongyang Li.
DriveLM: Driving with graph visual question answering.
In *Proc. Eur. Conf. Comput. Vis.* , 2024.
- Siméoni et al. (2025)
Oriane Siméoni, Huy V. Vo, Maximilian Seitzer, Federico Baldassarre, Maxime
Oquab, Cijo Jose, Vasil Khalidov, Marc Szafraniec, Seungeun Yi, Michaël
Ramamonjisoa, et al.
DINOv3.
*arXiv:2508.10104* , 2025.
- Singh et al. (2019)
Amanpreet Singh, Vivek Natarajan, Meet Shah, Yu Jiang, Xinlei Chen, Dhruv
Batra, Devi Parikh, and Marcus Rohrbach.
Towards VQA models that can read.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2019.
- Singh et al. (1994)
Satinder P. Singh, Tommi Jaakkola, and Michael I. Jordan.
Learning without state-estimation in partially observable Markovian
decision processes.
In *Proc. Int. Conf. Mach. Learn.* , pp.  284–292, 1994.
- Song et al. (2026)
Wenxuan Song, Jiayi Chen, Jingbo Wang, Shuai Zhou, Xicheng Gong, Zehua Fan,
Ziyang Zhou, Junwu E, Haodong Yan, Fuhao Li, et al.
UniWAM: Unified world-action model.
*arXiv:2610.02054* , 2026.
- Sridhar et al. (2026)
Ajay Sridhar, Jennifer Pan, Satvik Sharma, and Chelsea Finn.
MemER: Scaling up memory for robot control via experience
retrieval.
In *Int. Conf. Learn. Represent.* , 2026.
- StarVLA Community (2026)
StarVLA Community.
StarVLA: A Lego-like codebase for vision-language-action model
developing.
*arXiv:2604.05014* , 2026.
- Suhr et al. (2017)
Alane Suhr, Mike Lewis, James Yeh, and Yoav Artzi.
A corpus of natural language for visual reasoning.
In *Proc. Annu. Meet. Assoc. Comput. Linguist.* , 2017.
- Tian et al. (2026)
Yang Tian, Yuyin Yang, Yiman Xie, Zetao Cai, Xu Shi, Ning Gao, Hangxu Liu,
Xuekun Jiang, Zherui Qiu, Feng Yuan, et al.
InternData-A1: Pioneering high-fidelity synthetic data for
pre-training generalist policy.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2026.
- Tong et al. (2024)
Shengbang Tong, Ellis Brown, Penghao Wu, Sanghyun Woo, Manoj Middepogu,
Sai Charitha Akula, Jihan Yang, Shusheng Yang, Adithya Iyer, Xichen Pan,
et al.
Cambrian-1: A fully open, vision-centric exploration of multimodal
LLMs.
In *Adv. Neural Inf. Process. Syst.* , 2024.
- Torne et al. (2026)
Marcel Torne, Karl Pertsch, Homer Walke, Kyle Vedder, Suraj Nair, Brian Ichter,
Allen Z Ren, Haohuan Wang, Jiaming Tang, Kyle Stachowicz, et al.
MEM: Multi-scale embodied memory for vision language action models.
*arXiv:2603.03596* , 2026.
- Tu et al. (2023)
Haoqin Tu, Chenhang Cui, Zijun Wang, Yiyang Zhou, Bingchen Zhao, Junlin Han,
Wangchunshu Zhou, Huaxiu Yao, and Cihang Xie.
How many unicorns are in this image? A safety evaluation benchmark
for vision LLMs.
*arXiv:2311.16101* , 2023.
- Veit et al. (2016)
Andreas Veit, Tomas Matera, Lukas Neumann, Jiri Matas, and Serge Belongie.
COCO-Text: Dataset and benchmark for text detection and recognition
in natural images.
*arXiv:1601.07140* , 2016.
- Walke et al. (2023)
Homer Rich Walke, Kevin Black, Tony Z. Zhao, Quan Vuong, Chongyi Zheng,
Philippe Hansen-Estruch, Andre Wang He, Vivek Myers, Moo Jin Kim, Max Du,
et al.
BridgeData V2: A dataset for robot learning at scale.
In *Proc. Conf. Robot Learn.* , pp.  1723–1736, 2023.
- Wang et al. (2023)
Junke Wang, Lingchen Meng, Zejia Weng, Bo He, Zuxuan Wu, and Yu-Gang Jiang.
To see is to believe: Prompting GPT-4V for better visual
instruction tuning.
*arXiv:2311.07574* , 2023.
- Wang et al. (2026a)
Qiuyue Wang, Mingsheng Li, Jian Guan, Jinhui Ye, Sicheng Xie, Yitao Liu, Junhao
Chen, Zhixuan Liang, Jie Zhang, Xintong Hu, et al.
Qwen-VLA: Unifying vision-language-action modeling across tasks,
environments, and robot embodiments.
*arXiv:2605.30280* , 2026a.
- Wang et al. (2026b)
Tianheng Wang, Zhou Xie, Heng Jia, Jianhua Xu, Tong Zhang, and Kaicheng Yu.
An action is worth one patch: Unified world-action modeling with
PatchWAM.
*arXiv:2609.25961* , 2026b.
- Wang et al. (2020)
Xinyu Wang, Yuliang Liu, Chunhua Shen, Chun Chet Ng, Canjie Luo, Lianwen Jin,
Chee Seng Chan, Anton van den Hengel, and Liangwei Wang.
On the general value of evidence, and bilingual scene-text visual
question answering.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2020.
- Weyand et al. (2020)
Tobias Weyand, Andre Araujo, Bingyi Cao, and Jack Sim.
Google Landmarks Dataset v2: A large-scale benchmark for
instance-level recognition and retrieval.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2020.
- Whitehead & Ballard (1991)
Steven D. Whitehead and Dana H. Ballard.
Learning to perceive and act by trial and error.
*Machine Learning* , 7(1):45–83, 1991.
- Wiedmann et al. (2025) Luis Wiedmann, Orr Zohar, Amir Mahla, Xiaohan Wang, Rui Li, Thibaud Frere, Leandro von Werra, Aritra Roy Gosthipaty, and Andrés Marafioti. FineVision: Open data is all you need. arXiv:2510.17269, 2025.
- Wu et al. (2025a)
Kun Wu, Chengkai Hou, Jiaming Liu, Zhengping Che, Xiaozhu Ju, Zhuqin Yang, Meng
Li, Yinuo Zhao, Zhiyuan Xu, Guang Yang, et al.
RoboMIND: Benchmark on multi-embodiment intelligence normative data
for robot manipulation.
In *Robotics: Science and Systems* , 2025a.
- Wu et al. (2025b)
Shihan Wu, Xuecheng Liu, Shaoxuan Xie, Pengwei Wang, Xinghang Li, Bowen Yang,
Zhe Li, Kai Zhu, Hongyu Wu, Yiheng Liu, et al.
RoboCOIN: An open-sourced bimanual robotic data COllection for
INtegrated manipulation.
*arXiv:2511.17441* , 2025b.
- Xiao et al. (2026)
Lei Xiao, Jifeng Li, Juntao Gao, Feiyang Ye, Yan Jin, Jingjing Qian, Jing
Zhang, Yong Wu, and Xiaoyuan Yu.
AVA-VLA: Improving vision-language-action models with active visual
attention.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2026.
- Xiaomi Robotics Team (2026)
Xiaomi Robotics Team.
Xiaomi-Robotics-1: Scaling vision-language-action models with over
100K hours of real-world trajectories.
*arXiv:2607.15330* , 2026.
- Xu et al. (2025)
Guowei Xu, Peng Jin, Ziang Wu, Hao Li, Yibing Song, Lichao Sun, and Li Yuan.
LLaVA-CoT: Let vision language models reason step-by-step.
In *Proc. IEEE/CVF Int. Conf. Comput. Vis.* , 2025.
- Xu et al. (2024)
Zhiyang Xu, Chao Feng, Rulin Shao, Trevor Ashby, Ying Shen, Di Jin, Yu Cheng,
Qifan Wang, and Lifu Huang.
Vision-Flan: Scaling human-labeled tasks in visual instruction
tuning.
*arXiv:2402.11690* , 2024.
- Yang et al. (2019)
Kaiyu Yang, Olga Russakovsky, and Jia Deng.
SpatialSense: An adversarially crowdsourced benchmark for spatial
relation recognition.
In *Proc. IEEE/CVF Int. Conf. Comput. Vis.* , 2019.
- Yang et al. (2026)
Yandan Yang, Shuang Zeng, Tong Lin, Xinyuan Chang, Dekang Qi, Junjin Xiao,
Haoyun Liu, Ronghan Chen, Yuzhi Chen, Dongjie Huo, et al.
ABot-M0: VLA foundation model for robotic manipulation with
action manifold learning.
*arXiv:2602.11236* , 2026.
- Yuan et al. (2026)
Haoqi Yuan, Zhixuan Liang, Anzhe Chen, Ye Wang, Haoyang Li, Pei Lin, Yiyang
Huang, Zixing Lei, Tong Zhang, Jiazhao Zhang, et al.
Qwen-RobotManip technical report: Alignment unlocks scale for
robotic manipulation foundation models.
*arXiv:2606.17846* , 2026.
- Yuan et al. (2024)
Wentao Yuan, Jiafei Duan, Valts Blukis, Wilbert Pumacay, Ranjay Krishna,
Adithyavairavan Murali, Arsalan Mousavian, and Dieter Fox.
RoboPoint: A vision-language model for spatial affordance
prediction in robotics.
In *Proc. Conf. Robot Learn.* , 2024.
- Zang et al. (2026)
Hongzhi Zang, Mingjie Wei, Si Xu, Yongji Wu, Zhen Guo, Yuanqing Wang, Hao Lin,
Peihong Wang, Liangzhi Shi, Yuqing Xie, et al.
RLinf-VLA: A unified and efficient framework for reinforcement
learning of vision-language-action models.
In *Robotics: Science and Systems* , 2026.
- Zhai et al. (2023)
Xiaohua Zhai, Basil Mustafa, Alexander Kolesnikov, and Lucas Beyer.
Sigmoid loss for language image pre-training.
In *Proc. IEEE/CVF Int. Conf. Comput. Vis.* , 2023.
- Zhang et al. (2026)
Fei Zhang, Zhaochong An, Duncan Frost, Yikai Wang, Pengfei Liu, Ya Zhang,
Michal Drozdzal, and Amir Bar.
World action modeling with progressive visual planning.
*arXiv:2610.02508* , 2026.
- Zhao et al. (2023a)
Bo Zhao, Boya Wu, Muyang He, and Tiejun Huang.
SVIT: Scaling up visual instruction tuning.
*arXiv:2307.04087* , 2023a.
- Zhao et al. (2023b)
Tony Z. Zhao, Vikash Kumar, Sergey Levine, and Chelsea Finn.
Learning fine-grained bimanual manipulation with low-cost hardware.
In *Robotics: Science and Systems* , 2023b.
- Zheng et al. (2025)
Jinliang Zheng, Jianxiong Li, Zhihao Wang, Dongxiu Liu, Xirui Kang, Yuchun
Feng, Yinan Zheng, Jiayin Zou, Yilun Chen, Jia Zeng, Ya-Qin Zhang, Jiangmiao
Pang, Jingjing Liu, Tai Wang, and Xianyuan Zhan.
X-VLA: Soft-prompted transformer as scalable cross-embodiment
vision-language-action model.
*arXiv:2510.10274* , 2025.
- Zhong et al. (2026)
Linqing Zhong, Yi Liu, Yifei Wei, Ziyu Xiong, Maoqing Yao, Si Liu, and Guanghui
Ren.
ACoT-VLA: Action chain-of-thought for vision-language-action
models.
*arXiv:2601.11404* , 2026.
- Zhou et al. (2025)
Enshen Zhou, Jingkun An, Cheng Chi, Yi Han, Shanyu Rong, Chi Zhang, Pengwei
Wang, Zhongyuan Wang, Tiejun Huang, Lu Sheng, and Shanghang Zhang.
RoboRefer: Towards spatial referring with reasoning in
vision-language models for robotics.
In *Adv. Neural Inf. Process. Syst.* , 2025.
- Zhu et al. (2025)
Fangqi Zhu, Zhengyang Yan, Zicong Hong, Quanxin Shou, Xiao Ma, and Song Guo.
WMPO: World model-based policy optimization for
vision-language-action models.
*arXiv:2511.09515* , 2025.
- Zhu et al. (2016)
Yuke Zhu, Oliver Groth, Michael Bernstein, and Li Fei-Fei.
Visual7W: Grounded question answering in images.
In *Proc. IEEE/CVF Conf. Comput. Vis. Pattern Recognit.* , 2016.

## Appendix

## Appendix A Model details

Figure 9 shows how the prompt and the camera views are packed into one stream for the VLM of Section 3.

Prompt. The prompt of Section 3.2 is a structured user turn carrying one field: value per line:

embodiment: gr1
instruction: grasp the mug
speed: 2
fps: 20
control mode: absolute joint

The speed field is the length of the episode in frames divided by 100 and rounded, so that a smaller value marks a shorter demonstration. It is left out at evaluation. Episodes of ABC (Allshire et al., 2026) that are labelled with their outcome also carry a success field.

Positions. In the expert’s self-attention, register of a frame sits at slot and action step at slot , so that the registers of each frame carry their own temporal encoding. The frame and slot axes reuse the temporal and spatial axes of the VLM’s multimodal rotary embedding.

### A.1 Inference cost and caching

What is cached. One chunk is an encoder pass, then Euler steps over the action tokens, the first of which also computes the register tokens of the current frame (Fig. 10). Only the noisy actions change from one Euler step to the next, so four quantities are reused across the steps.

1. 1.
2. 2. 
Its projection to keys and values, one pair per expert block.
3. 3. 
The keys and values of the register tokens of the current frame, one pair per expert block.
4. 4. 
The register keys and values of the preceding frames, carried over from earlier chunks.

The first three are built the same way at training and at inference, while the fourth is not: the first registers of a window hold memory from earlier registers at inference, and none at training.

What it costs. Figure 5 gives the measurement. On one H100, a chunk takes 22.1 ms without registers, 22.6 ms at and 22.8 ms at , the window used for training and evaluation. Even at , a chunk takes only 24.6 ms. The cache adds about 2 MiB of GPU memory per past frame, 4 MiB at , on a policy of 4.7 GiB.

Protocol. Every measurement is taken on one H100, with two cameras, a chunk of actions and Euler steps, in bf16, with the expert and the VLM prefill run as CUDA graphs. Each value is the median of two interleaved repetitions of 200 chunks. The split by phase comes from a separate pass with a synchronisation after each phase, scaled to the median total. The model is trained and evaluated at . The longer windows are used only for these measurements.

## Appendix B Data

### B.1 Robot embodiments

Table 1 lists the robots of the pretraining mixture with their action conventions and source datasets. From Open X-Embodiment, the mixture uses twelve datasets: Taco Play, Stanford Hydra, NYU Franka Play, BC-Z, RT-1, Furniture Bench, Austin Sailor, Austin BUDS, UT Austin MUTEX, Berkeley Cable Routing, VIOLA and Berkeley RPT. The joint-delta actions of Berkeley RPT and RoboSet are converted to end-effector deltas by forward kinematics. A robot appears under several conventions when its datasets command it differently, joint angles in one and end-effector deltas in another. The state projection of Section 3.2 is selected by state layout and control convention, and the action statistics of Section 5.1 are per dataset, so no source is remapped to another’s convention. A state layout is the ordered list of state channels a dataset records, for example the joint angles and gripper opening of each arm, and fixes the input width of its projection.

| Robot | Action type | Sources | 
| *Single-arm* |  |  | 
| WidowX | EE + G | (Walke et al., 2023) | 
| Google Robot | EE + G | (Brohan et al., 2023; Jang et al., 2022) | 
| Franka Panda | EE + G; J + G | (Open X-Embodiment Collaboration, 2024; Bharadhwaj et al., 2024; Wu et al., 2025a; Khazatsky et al., 2024; Tian et al., 2026) | 
| UR5e | J + G | (Wu et al., 2025a) | 
| *Dual-arm* |  |  | 
| AgileX Cobot Magic | J + G | (Wu et al., 2025b; Wu et al., 2025a; Liu et al., 2025) | 
| ALOHA | J + G | (Tian et al., 2026) | 
| AgiBot G1 | J + G | (Tian et al., 2026) | 
| AgiBot G2A | J + G + W | (AgiBot World Team, 2026) | 
| Galaxea R1 Lite | J + G + W + B; J + G | (Jiang et al., 2025; Wu et al., 2025b) | 
| Dual Franka FR3 | J + G | (Wu et al., 2025a) | 
| AgiBot A2D | J + G | (Wu et al., 2025b) | 
| Galbot G1 | J + G | (Wu et al., 2025b) | 
| RealMan RMC-AIDA-L | J + G | (Wu et al., 2025b) | 
| AI2 AlphaBot 2 | J + G | (Wu et al., 2025b) | 
| YAM | J + G | (Allshire et al., 2026) | 
| Bimanual Panda | EE + G; EE + DH | (Bjorck et al., 2025) | 
| Fourier GR-1 | J + DH | (Bjorck et al., 2025) | 
| TienKung | J + G; J + DH | (Wu et al., 2025a) | 
| AIRBOT MMK2 | J + DH | (Wu et al., 2025b) | 
| Unitree G1 | J + DH | (Wu et al., 2025b) | 
| Kuavo | J + DH | (Wu et al., 2025b) | 
| *Human* |  |  | 
| Real Human | EE + eigengrasp | (Hoque et al., 2026; Punamiya et al., 2026; Ropedia, 2026) | 

### B.2 Data curation

Aggregating data across embodiments, simulators, and collection pipelines introduces heterogeneous noise: sensor outliers, temporal state-action misalignment, and inconsistent conventions across datasets sharing a robot.

- • 
*Channel selection.* We keep only the degrees of freedom the policy commands, dropping derived or uncontrolled channels: forward-kinematic end-effector poses, joint velocities, efforts, IMU readings, and telepresence-parked head, waist, and leg joints.
- • 
*Degenerate episodes.* We discard static and near-zero-variance recordings, including episodes whose follower arm never moves, a failure mode Qwen-RobotManip reports for  of RoboMIND’s UR subset.
- • 
*Sudden jumps.* We fit a smooth trend to each channel with median and Savitzky–Golay filters, and flag a frame when its residual and its acceleration or jerk exceed a per-channel threshold, the larger of 0.05 in raw units and four times the pooled 95th percentile.
Grippers are exempt.
Whole episodes are removed only from InternData-A1, where simulator collisions cause the jumps (6.3% of its episodes).
- • 
*Action representation.* Rotations are harmonised to one Euler parameterisation; other conventions are kept as the source provides them.
Each action dimension is mapped to  from its per-dataset 1st and 99th percentiles (Wang et al., 2026a), which removes scale differences across embodiments while preserving relative motion within each source.
- • 
*Convention correction.* We fix per-dataset sign conventions that would otherwise break cross-dataset consistency (*e.g.* , a sign-flipped elbow joint in InternData); differing scales and encodings are then absorbed by the per-dataset normalisation.

### B.3 Sampling weights

During pretraining, each dataset is sampled with probability proportional to (Table 2). is its number of frames, and and are its language and vision diversity, each divided by its median over the datasets. Datasets from the same source are grouped before weighting.

Both diversities are the effective rank of a set of L2-normalised embeddings, with , where are the singular values of the embedding matrix. The embeddings are not centred, so that a dataset with two distinct instructions does not collapse to rank one. For the language diversity, the rows are the SigLIP (Zhai et al., 2023) text embeddings of the distinct episode task strings, each scaled by the square root of the fraction of frames it covers. For the vision diversity, the rows are the DINOv3 (Siméoni et al., 2025) class tokens of every 15th frame of the main external camera, at most 50,000 frames per dataset.

The RoboCOIN and InternData-A1 collections are further weighted by 0.5, and the weights are capped so that no dataset is seen more than four times.

### B.4 Egocentric data

Sources. The three egocentric sources differ in capture conditions: EgoDex is seated tabletop recordings on Apple Vision Pro, EgoVerse head-mounted rigs, and Xperience in-the-wild moving-camera footage. Frame rates range from 20 to 30 Hz and are left unaltered; the differences are absorbed by the per-dataset quantile normalisation of Section 5.1.

Action target. We follow the layout of Qwen-VLA’s egocentric target (Wang et al., 2026a): per hand, a 6-dimensional wrist motion as translation and axis-angle rotation, and 10 articulation coefficients, 32 dimensions in total. Unlike Qwen-VLA, which expresses the wrist relative to its own initial pose, we express it in the observation camera frame through the rigid transform . The articulation coefficients are the first 10 principal components (“eigengrasps”) of the finger-joint configuration under a PCA basis shared across our three sources. The target is defined relative to the frame that opens each action chunk, so it is composed at load time from the stored world-frame wrist poses and per-frame camera poses rather than precomputed.

State and pacing. The egocentric data have no proprioceptive state, and the policy receives a learned null state in its place. Human hands move faster than a teleoperated robot, so the human actions are slowed down in time (Yuan et al., 2026). Waypoint of a chunk is the hand pose at time after the observation, where is the frame rate of the recording and a per-source pace (Table 2). The pose is interpolated between the two nearest frames. A one-second chunk at thus holds a quarter of a second of human motion.

### B.5 Vision–language data

REKI is co-trained on vision–language data at 10% of the training samples (Table 2). The pool is every FineVision (Wiedmann et al., 2025) dataset except the text-only ones, plus the LLaVA-OneVision-1.5 (An et al., 2025) datasets that FineVision does not already contain, M samples. We keep the datasets covering the tasks most relevant to manipulation: natural-scene objects and attributes, spatial relations, counting, grounding, captioning, scene text, and knowledge and commonsense VQA. Documents, charts, tables, mathematics, code, transcription, GUI grounding, memes and puzzles are dropped. 60 datasets remain, 7.64M samples or 37.8% of the pool. One question–answer pair is drawn per sample and sampling is uniform over samples, so exposure is proportional to the size of a dataset.

The 51 kept from FineVision are a_okvqa (Schwenk et al., 2022), alfworldgpt (Shridhar et al., 2021), allava_laion (Chen et al., 2024a), allava_vflan (Chen et al., 2024a), aokvqa (Schwenk et al., 2022), art (Chng et al., 2019), cambrian(filtered)_processed (Tong et al., 2024), clevr (Lindström & Abraham, 2022), clevr_math (Lindström & Abraham, 2022), coco_colors (Karakus, 2024), cocoqa (Ren et al., 2015), cocotext (Veit et al., 2016), densefusion_1m (Li et al., 2024), drivelm (Sima et al., 2024), est_vqa (Wang et al., 2020), google_landmarks (Weyand et al., 2020), image_textualization(filtered) (Pi et al., 2024), indoor_qa (Quattoni & Torralba, 2009), laion_gpt4v (LAION, 2023), LLaVA_Instruct_150K (Liu et al., 2023c), lnqa (Pont-Tuset et al., 2020), localized_narratives (Pont-Tuset et al., 2020), lrv_normal(filtered) (Liu et al., 2024a), lvis_instruct4v (Wang et al., 2023), mimic_cgd (Laurençon et al., 2024), mmevol (Luo et al., 2024), nlvr2 (Suhr et al., 2017), objects365_qa (Shao et al., 2019), ocrvqa (Mishra et al., 2019), oodvqa (Tu et al., 2023), scienceqa (Lu et al., 2022), scienceqa(nona_context) (Lu et al., 2022), sharegpt4o (OpenGVLab, 2024), sharegpt4v(coco) (Chen et al., 2024b), sharegpt4v(knowledge) (Chen et al., 2024b), sharegpt4v(llava) (Chen et al., 2024b), sharegpt4v(sam) (Chen et al., 2024b), sketchyvqa (Tu et al., 2023), spatialsense (Yang et al., 2019), spot_the_diff (Jhamtani & Berg-Kirkpatrick, 2018), st_vqa (Biten et al., 2019), super_clevr(mathv360k) (Li et al., 2023), tallyqa (Acharya et al., 2019), textcaps (Sidorov et al., 2020), textocr(gpt4v) (Carter, 2024), textvqa (Singh et al., 2019), vision_flan(filtered) (Xu et al., 2024), visual7w (Zhu et al., 2016), vqav2 (Goyal et al., 2017), vsr (Liu et al., 2023b), and wildvision (Lu et al., 2024). The nine kept from LLaVA-OneVision-1.5 are gqa (Hudson & Manning, 2019), llava_cot_100k (Xu et al., 2025), llava_wild (Liu et al., 2023c), llrv_gpt4v (Liu et al., 2024a), sherlock (Hessel et al., 2022), svit (Zhao et al., 2023a), viquae (Lerner et al., 2022), vision_oritented, and visual_chat.

To these we add the object- and free-space-reference splits of RoboPoint (Yuan et al., 2024), the 2D and 3D splits of RefSpatial (Zhou et al., 2025), and PixMo-Points (Deitke et al., 2025) rewritten as pointing and counting questions, all with coordinates on a grid.

## Appendix C Training configuration

Table 2 lists the training configuration. Table 3 gives the node count, the number of steps and the settings that differ between pretraining and the five finetuning runs. Settings absent from Table 3 are those of Table 2 in every run.

| Setting | Value | 
|---|---|
| Encoder |  | 
| Vision–language backbone | Qwen3.5-2B (18 gated-DeltaNet + 6 full-attention layers) | 
| Image resolution | native, up to | 
| Expert conditioning | cross-attention to the final-layer backbone states in every expert block | 
| Trainable components | backbone at a reduced LR (Table 3), last vision blocks, expert | 
| Vision–language co-training | : FineVision (subset), LLaVA-OneVision-1.5 (subset), RoboPoint, RefSpatial, PixMo | 
| Text loss (subtask, vision–language data) | cross-entropy, weight , pretraining only | 
| Prompt-field dropout | (embodiment), (control frequency, speed) | 
| Action expert |  | 
| Size | blocks, hidden, heads, MLP | 
| Precision | bfloat16, flow heads in fp32 | 
| Proprioception | per-layout linear layer with GELU, shared zero-initialised projection, summed into the adaptive-normalisation conditioning | 
| Proprioception dropout |  | 
| Registers per frame |  | 
| Window | frames at a s stride | 
| Action generation |  | 
| Action chunk | s ( steps) | 
| Objective | flow matching, DoF-balanced | 
| Time-step distribution | Beta | 
| Flow draws per forward pass |  | 
| Data |  | 
| Robot corpora | OXE, DROID, AgiBot, RoboMIND, Galaxea, RoboSet, RDT, RoboCOIN, GR00T, InternData, ABC | 
| Egocentric human video | EgoDex, EgoVerse, Xperience | 
| Sampling weight | (Section B.3) | 
| Normalisation | quantile (1st / 99th percentile) | 
| Action-speed alignment | EgoDex , EgoVerse , Xperience (unchanged) | 
| Vision & augmentation |  | 
| Camera dropout | per camera in pretraining, in finetuning; at least one camera kept | 
| Camera order | shuffled | 
| Colour jitter | brightness, contrast, saturation , drawn for each frame of the window and shared by all cameras of the frame | 
| Random crop | overhead and side cameras, scale , one box per camera for the window; pretraining and LIBERO only | 
| Optimisation |  | 
| Learning rate | expert: , VLM: to (Table 3) | 
| Weight decay |  | 
| Gradient clipping |  | 
| Schedule | constant with warmup ( steps when finetuning the pretrained model, otherwise), then decay over the last steps (finetuning only) | 
| Weight averaging | EMA, decay | 
| Gradient checkpointing | ✓ | 

<sup>∗</sup>Multiplied by the language and vision diversity terms of Section B.3.

Pretraining
LIBERO
RoboTwin
RoboCasa365
RoboCasa-GR1
Clean
Clean+Rand
Nodes (H100)
4
1
1
3
4
3
Steps
200k
15k
20k
25k
25k
25k
Wall-clock time (h)
176
5.2
10.9
14.2
12.5
8.3
GPU-hours
5,632
41
87
341
401
199
Backbone LR ( expert LR)
0.1
0.3
0.2
0.3
0.3
0.2
Sampling weights 
✓<sup>∗</sup>
✗
✗
✓
✗
✗
Overhead-camera dropout
✗
✓
✗
✗
✗
✗
Mirroring (Section 5.3)
✗
✗
✓
✓
✓
✓
Evaluation rollouts per task
–
50
20
20
50
50

### C.1 Camera selection

In pretraining, two rules remove cameras from a sample after the random camera dropout. When two or more side cameras remain, one of them is kept at random and the others are removed. On a two-arm robot with both wrist cameras present, the wrist camera of a static arm is removed. An arm is static when, over the action chunk, every action dimension of the arm varies by less than of its standard deviation in the dataset. Both rules keep at least one camera other than the overhead camera. Finetuning uses neither rule. Random camera dropout never removes the overhead camera, except in LIBERO finetuning (Table 3).

### C.2 Mirroring

A sample is mirrored with probability in the finetuning runs on RoboTwin, RoboCasa365 and RoboCasa-GR1 (Table 3). The images are flipped horizontally, and the state and the actions are reflected through the same plane. On a two-arm robot, the two arms and their wrist cameras are swapped. The prompt of a mirrored sample starts with [MIRROR WORLD], and its state goes through a separate projection with separate normalisation statistics. The policy is evaluated in the unmirrored world only.

## Appendix D Full benchmark results

Tables 4 and 5 give the per-condition results summarised in Fig. 4, together with the in-distribution results on LIBERO and RoboTwin. Each LIBERO-Plus column perturbs one dimension: the camera viewpoint, the initial robot state, the language instruction, the lighting, the background texture, the sensor noise or the object layout. The Overall column is the success rate over all 10,030 perturbed episodes, so each dimension is weighted by its number of episodes (Fei et al., 2025). For the in-distribution RoboTwin results, the models are finetuned on clean and randomised scenes together and evaluated on each (Clean, Rand). For Clean2Rand, the models are finetuned on clean scenes only and evaluated on clean scenes (Easy), under a randomised background, lighting, clutter or table height applied alone, and under all four together (Hard). The baseline numbers are taken from published results, and Table 7 gives the source of each one. The Clean2Rand row of Qwen-RobotManip (Yuan et al., 2026) is its joint-space variant. The LIBERO-Plus results of and are from the zero-shot block of ACoT-VLA (Zhong et al., 2026), which evaluated the released checkpoints. ACoT-VLA is initialised from , so it is grouped with the models pretrained on in-house data. The LIBERO-Plus results of StarVLA are those of its Qwen3-VL-OFT model in the StarVLA repository. For Being-H0.7 and ABot-M0.5, the LIBERO-Plus Overall is pooled from the seven published dimensions, because the published total does not match them. The RoboCasa365 leaderboard is at https://robocasa.ai/leaderboard. On RoboCasa-GR1, the result of GR00T-N1.5 is that of the released checkpoint in the Isaac-GR00T repository (https://github.com/NVIDIA/Isaac-GR00T, branch n1.5-release), and the result of is the one reported by StarVLA, which states no source for it. Parameter counts include the action expert, are rounded from exact counts and are read off the released checkpoints where available, including those of StarVLA and RLDX-1 (Kim et al., 2026). Qwen-RobotManip states no expert size; our expert follows its design, so we count 0.2B for it.

*Italic*: model not released at the time of writing. Bold: best in each column within each block, italic rows excluded.

<sup>†</sup>Pooled from the seven reported dimensions, which the published total does not match.

Method
#Params
LIBERO
LIBERO-Plus
VLM
Expert
Camera
Robot
Language
Light
Backgd
Noise
Layout
Overall
*No pretraining*
StarVLA (StarVLA Community, 2026)
4.4B
0.1B
98.0
47.0
60.1
87.0
96.3
95.3
73.1
79.2
75.0
Qwen-RobotManip-scratch (Yuan et al., 2026)
4.5B
0.2B
98.2
70.4
44.9
88.1
95.8
95.5
84.4
79.1
78.3
REKI (ours)  *scratch*
2.2B
0.2B
96.5
94.4
63.9
80.7
97.5
96.0
96.0
79.2
86.1
*With pretraining – in-house data*
 (Black et al., 2024)
2.9B
0.3B
94.4
61.0
40.8
63.5
89.3
84.1
80.1
76.4
69.4
 (Physical Intelligence et al., 2025)
2.9B
0.4B
97.6
75.8
79.4
83.3
95.5
95.0
89.6
87.0
85.7
ACoT-VLA (Zhong et al., 2026)
2.9B
0.9B
98.5
72.6
82.6
87.5
97.7
96.5
87.8
88.1
86.6
Being-H0.7 (Luo et al., 2026)
3B
–
99.2
82.0
59.0
82.8
97.8
90.0
93.5
88.5
84.0<sup>†</sup>
Qwen-RobotManip (Yuan et al., 2026)
4.5B
0.2B
99.1
87.2
75.5
85.6
96.6
97.7
97.7
87.3
89.0
RLDX-1 (Kim et al., 2026)
5.3B
1.6B
97.8
75.0
91.8
92.2
97.1
95.5
80.3
80.7
86.7
*With pretraining – public data only*
ABot-M0 (Yang et al., 2026)
4.4B
0.2B
98.6
60.4
67.9
86.4
96.2
91.6
86.4
82.6
80.5
ABot-M0.5 (Chen et al., 2026)
5B
–
99.4
70.5
87.4
88.6
94.0
89.7
75.5
85.2
83.7<sup>†</sup>
InternVLA-A1.5 (Ma et al., 2026)
2.2B
0.5B
98.9
83.1
55.1
86.9
96.4
98.2
95.6
85.2
84.8
REKI (ours)
2.2B
0.2B
97.5
93.2
75.6
88.5
97.7
98.4
98.6
84.7
90.4

*Italic*: model not released at the time of writing. Bold: best in each column within each block, italic rows excluded.

Method
#Params
RoboTwin
RoboTwin Clean2Rand
VLM
Expert
Clean
Rand
Easy
Backgd
Light
Clutter
Height
Hard
*No pretraining*
StarVLA (StarVLA Community, 2026)
4.4B
0.1B
88.2
88.3
58.1
27.1
50.9
24.2
48.4
10.6
Qwen-RobotManip-scratch (Yuan et al., 2026)
4.5B
0.2B
88.7
88.4
71.6
60.6
70.7
24.6
63.6
22.6
REKI (ours)  *scratch*
2.2B
0.2B
83.0
83.8
67.1
62.4
68.2
27.5
60.7
26.3
*With pretraining – in-house data*
 (Black et al., 2024)
2.9B
0.3B
65.9
58.4
–
–
–
–
–
–
 (Physical Intelligence et al., 2025)
2.9B
0.4B
82.7
76.8
73.1
67.0
69.2
57.9
67.6
47.9
GR00T-N1.7 (Bjorck et al., 2025)
1.5B
1.6B
–
–
43.6
40.4
41.9
27.1
39.0
20.7
Being-H0.7 (Luo et al., 2026)
3B
–
90.2
89.6
–
–
–
–
–
–
Qwen-RobotManip (Yuan et al., 2026)
4.5B
0.2B
93.4
92.5
73.2
74.6
68.4
61.3
71.0
62.6
*With pretraining – public data only*
ABot-M0 (Yang et al., 2026)
4.4B
0.2B
86.1
85.1
70.7
56.5
68.8
46.0
56.3
36.0
ABot-M0.5 (Chen et al., 2026)
5B
–
94.0
94.2
–
–
–
–
–
–
InternVLA-A1.5 (Ma et al., 2026)
2.2B
0.5B
93.3
93.0
–
–
–
–
–
–
REKI (ours)
2.2B
0.2B
85.9
84.6
73.4
71.3
73.5
57.8
69.9
48.7

*Italic*: model not released at the time of writing.

| Method | #Params | Atomic | Composite |  | Avg | 
|---|---|---|---|---|---|
|  |  |  | Seen | Unseen |  | 
| Xiaomi-Robotics-1 <sup>∗</sup> (Xiaomi Robotics Team, 2026) | 5.1B | 80.2 | 57.1 | 32.1 | 57.4 | 
| ABot-M0.5 (Chen et al., 2026) | 5B | 75.9 | 38.3 | 2.7 | 40.4 | 
| RLDX-1 <sup>∗</sup> (Kim et al., 2026) | 6.9B | 67.6 | 27.9 | 8.5 | 36.0 | 
| Qwen-RobotManip <sup>∗</sup> (Yuan et al., 2026) | 4.7B | 68.6 | 20.1 | 14.9 | 35.9 | 
| GR00T-N1.5 <sup>∗</sup> (Bjorck et al., 2025) | 2.4B | 50.7 | 14.8 | 2.7 | 23.9 | 
| GR00T-N1.6 <sup>∗</sup> (Bjorck et al., 2025) | 3.0B | 51.1 | 9.4 | 1.7 | 21.9 | 
| <sup>∗</sup> (Physical Intelligence et al., 2025) | 3.3B | 39.6 | 7.1 | 1.2 | 16.9 | 
| REKI (ours) | 2.4B | 65.8 | 32.0 | 14.1 | 38.4 | 

Method
LIBERO
LIBERO-Plus
RoboTwin Clean, Rand
RoboTwin Clean2Rand
RoboCasa365
RoboCasa-GR1
*No pretraining*
StarVLA
Qwen-RobotManip
repository
checkpoint card
Qwen-RobotManip
–
paper
Qwen-RobotManip-scratch
paper
paper
paper
paper
–
–
*With pretraining – in-house data*
Qwen-RobotManip
ACoT-VLA
InternVLA-A1.5
–
–
–
Qwen-RobotManip
ACoT-VLA
Qwen-RobotManip
Qwen-RobotManip
leaderboard
StarVLA
ACoT-VLA
paper
paper
–
–
–
–
GR00T-N1.5
–
–
–
–
leaderboard
repository
GR00T-N1.6
–
–
–
–
leaderboard
–
GR00T-N1.7
–
–
–
Qwen-RobotManip
–
–
Being-H0.7
Qwen-RobotManip
Qwen-RobotManip
paper
–
–
paper
Qwen-RobotManip
paper
paper
paper
paper
paper
–
RLDX-1
