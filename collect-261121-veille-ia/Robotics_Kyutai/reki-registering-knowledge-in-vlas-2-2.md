---
id: collect-261121-veille-ia/robotics-kyutai/reki-registering-knowledge-in-vlas-2-2
title: "REKI: Registering Knowledge in VLAs"
domain: robotics-kyutai
role: reference
task: article
actors: []
dates: []
keywords: ["paper", "camera", "points", "steps", "without", "mirroring", "pretraining", "reki"]
source: https://arxiv.org/html/2610.22178v1
source_anchor: ""
source_lines: [1501, 1716]
section: "Robotics_Kyutai"
---
paper
paper
–
–
leaderboard
paper
Xiaomi-Robotics-1
–
–
–
–
leaderboard
–
*With pretraining – public data only*
ABot-M0
paper
paper
paper
Qwen-RobotManip
–
paper
ABot-M0.5
paper
paper
paper
–
paper
–
InternVLA-A1.5
paper
paper
paper
–
–
–

### D.1 Pretraining steps

Figure 11 gives the Clean2Rand success of REKI after 50k to 200k pretraining steps, and without pretraining. All models are finetuned on clean RoboTwin scenes as in Table 5, but with a backbone learning rate of 0.1 instead of 0.2 times the expert learning rate, and without learning-rate decay. The mean success rises from 53.7 without pretraining to 61.9 after 50k steps, and then by 3.4 points more up to 200k steps. The first 50k steps add most under clutter (19.5 points) and under the Hard condition (11.7 points).

### D.2 Finetuning ablations

Table 8 compares the LIBERO finetuning configuration of REKI with variants that add mirroring, remove one augmentation or lower the backbone learning rate, all evaluated on LIBERO-Plus. It also compares finetuning on RoboCasa365 with and without mirroring. All variants use a memory window of and the same number of finetuning steps.

Mirroring. On RoboCasa365, mirroring (Section C.2) adds 4.9 points on average, mostly on the atomic tasks () and the seen composite tasks (). On LIBERO-Plus, mirroring improves four of the seven perturbation dimensions, by to points. However, it lowers the camera dimension by points, so the overall score drops by . A possible explanation is that the policy infers whether the world is mirrored from the images as well as from the prompt. A large change of the camera viewpoint may then confuse the policy about whether the world is mirrored.

Visual augmentation. Disabling camera dropout or the image augmentations (colour jittering and random cropping) causes a large drop on the camera dimension of LIBERO-Plus, by and points. This suggests that these augmentations make the policy robust to changes of the camera viewpoint. They also give a small gain on the sensor noise and object layout dimensions, of to points. For camera dropout, a likely reason is the wrist camera: the policy is trained to act from the wrist image alone, and the camera and sensor-noise perturbations of LIBERO-Plus change only the image of the external camera (Fei et al., 2025).

State dropout. Disabling state dropout lowers the robot dimension by points: state dropout makes the policy robust to changes of the initial state of the robot.

Backbone learning rate. Lowering the backbone learning rate from to times the expert learning rate lowers the overall score by points. The largest drops are on the camera and robot dimensions, by and points.

LIBERO-Plus Camera Robot Language Light Backgd Noise Layout Overall REKI 93.2 75.6 88.5 97.7 98.4 98.6 84.7 90.4 With mirroring 86.7 77.3 88.7 98.6 99.6 98.3 84.3 89.8 Without colour jitter and crop 69.6 74.8 90.4 97.9 97.8 96.6 80.1 85.7 Without camera dropout 71.7 76.5 90.8 99.0 98.4 94.7 83.1 86.8 Without state dropout 95.4 67.5 87.1 98.4 97.8 98.3 82.8 89.0 Backbone LR expert LR 81.3 71.0 86.7 98.5 98.0 97.8 82.8 87.1

| RoboCasa365 | Atomic | Composite |  | Avg | 
|---|---|---|---|---|
|  |  | Seen | Unseen |  | 
| Without mirroring | 59.6 | 24.1 | 13.5 | 33.5 | 
| With mirroring | 65.8 | 32.0 | 14.1 | 38.4 | 

### D.3 Training budgets of the compared methods

Table 9 collects the training budgets published for the methods compared in Tables 4 and 5. Qwen-RobotManip, its scratch variant, ABot-M0.5 and GR00T-N1.7 state no budget for the compared runs and are left out, as is ACoT-VLA, which is built on . No compared method reports wall-clock time or accelerator-hours, so the success rates of those tables are not matched for training compute.

Method
Pretraining
LIBERO
RoboTwin
Steps
Batch
Samples
Steps
Batch
Samples
Steps
Batch
Samples
*No pretraining*
StarVLA (StarVLA Community, 2026)
–
–
–
–
–
–
140k
192
26.9M
REKI (ours), scratch
–
–
–
30k
720
21.6M
40k / 45k
720 / 2160
28.8M / 97.2M
*With pretraining – in-house data*
 (Black et al., 2024)
700k
–
–
30k
32
0.96M
–
–
–
 (Physical Intelligence et al., 2025)
280k + 80k
–
–
30k
256
7.7M
–
–
–
Being-H0.7 (Luo et al., 2026)
–
–
–
–
128
–
–
128
–
RLDX-1 (Kim et al., 2026)
100k
8192
819.2M
60k
256
15.4M
–
–
–
*With pretraining – public data only*
ABot-M0 (Yang et al., 2026)
100k
1024
102.4M
40k
64
2.6M
125k
192
24.0M
InternVLA-A1.5 (Ma et al., 2026)
300k + 600k
1024
921.6M
100k
–
–
100k
384
38.4M
REKI (ours) (Table 3)
200k
2880
576.0M
15k
720
10.8M
20k / 25k
720 / 2160
14.4M / 54.0M

### D.4 Inference latency of the compared methods

In Section 6.3, each model is timed with its released checkpoint and inference code on one H100 (driver 570, CUDA 12.8), one model per process, at batch size one. The observations are synthetic and seeded, with new images and state at every chunk. In the LIBERO setting, each model receives two images, an 8-D state and a fixed prompt, and applies its own resizing and tokenisation. Each repetition is one warm-up episode followed by 320 timed chunks, and runs as a separate job on its own GPU of a node reserved for the timing. ABot-M0.5 takes several seconds per chunk, so each of its repetitions times 80 chunks, in four episodes of 20. The end-to-end time runs from the numpy observation to the unnormalised numpy actions. The model time starts from preprocessed tensors on the device, and the GPU time sums the CUDA kernel durations of a chunk, from a profiler pass taken last in the process. Eager runs use inference mode where the released code does, and fp32 runs enable TF32, as the LeRobot evaluation (Cadene et al., 2024) does. Every run has to load the released weights strictly, return finite actions of the right shape that change with the input, and keep at least 95% of them within the action range of the checkpoint. The official compiled path of InternVLA-A1.5 is slower than eager, and its optimized path returns non-finite actions after the first chunk, which we avoid by keeping one autocast context open for the whole process. For X-VLA, ABot-M0, InternVLA-A1.5 and ABot-M0.5, we also build a compiled path over the released code, without changing the computation. These runs show what the models can reach, not what their users get. The phase times of REKI and InternVLA-A1.5 in Section 6.3 come from a separate run on one exclusive node (Table 10).

|  | REKI | InternVLA-A1.5 |  | 
|---|---|---|---|
| Phase | compiled | optimized | compiled by us | 
| Vision encoder | 9.8 | 11.5 | 2.1 | 
| VLM prefill |  | 28.7 | 4.7 | 
| Euler steps (10) | 8.2 | 46.0 | 34.9 | 
| Other | 2.0 | 2.9 | 2.7 | 
| Model time | 19.7 | 88.2 | 44.1 | 

## Appendix E Real robot experiments

### E.1 Tasks and scoring

We evaluate on five tasks. Partial credit is awarded for each completed subtask, such as a successful grasp or placement. For tasks with an imposed ordering, a penalty is applied when objects are handled in the wrong order. An episode counts as a full success only when every subtask is completed in the correct order. The first two tasks involve a single object and are shorter-horizon. The remaining three require manipulating two or three objects in a prescribed order, so the policy must keep track of which steps have already been completed.

- • 
place_banana_in_box: pick the yellow banana and place it in the box. Partial credit: 0.5 for grasp; 0.5 for placement (only 0.25 if partially inside the box).
- • 
sweep_paper_into_dustpan: grasp the brush and sweep the paper crumples into the dustpan. Partial credit: 0.5 for grasp; 0.25 once one crumple is inside the dustpan; 0.25 once all crumples are inside.
- • 
arrange_3_fruits_in_plates: place the purple grapes in the yellow plate, then the yellow banana in the pink plate, then the red strawberry in the blue plate. Partial credit: per grasp and per placement for each of the three fruits; for wrong order.
- • 
stack_3_cubes: stack the pink cube on the yellow cube, then the green cube on the pink cube. Partial credit: 0.25 per grasp and 0.25 per placement for each of the two cubes; for wrong order.
- • 
stack_3_cups: stack the pink cup into the yellow cup, then stack the blue cup into the pink cup. Partial credit: 0.25 per grasp and 0.25 per placement for each of the two cups; for wrong order.

### E.2 Finetuning details

|  | REKI |  | 
|---|---|---|
| Optimiser steps | 3,000 | 3,000 | 
| Global batch size | 360 | 360 | 
| Learning rate (action expert) |  |  | 
| Learning rate (backbone) |  |  | 
| LR schedule | warmup, then constant | warmup, then constant | 
| Trainable vision blocks | last 4 | last 4 | 
| Action chunk size | 30 (2 s) | 50 (3.3 s) | 
| Memory window | frames, 2 s apart ( without memory) | – | 
| H100 GPU-hours | 8 | – | 

Both REKI and are initialised from their pretrained checkpoints and finetuned jointly on the five tasks, using the hyperparameters in Table 11. The action chunk has steps ( s) for REKI and ( s) for , which is fixed by its pretraining.
