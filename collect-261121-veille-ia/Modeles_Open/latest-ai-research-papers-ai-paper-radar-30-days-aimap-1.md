---
id: collect-261121-veille-ia/unifi-ubiquiti/latest-ai-research-papers-ai-paper-radar-30-days-aimap-1
title: "Latest AI research papers: AI Paper Radar (30 days) | AIMap"
domain: unifi-ubiquiti
role: reference
task: article
actors: []
dates: ["2026-10-22"]
keywords: ["across", "denoising", "model", "zhang", "tencent", "prediction", "benchmarks", "deng"]
source: https://aimap.page/en/learn/papers/
source_anchor: ""
source_lines: [1, 1500]
section: "Modeles_Open"
---
**#01** ▲258 ★428 ❝1 2026-10-22 

  Tencent WeChat's WeVisOne-VL unifies object- and pixel-level perception via structured next-token prediction — detection, referring, counting, OCR, segmentation and matting — using an <OBJ> token that drives a light pixel decoder; trained with 3-stage curriculum SFT plus mixed-task RL, it leads 30+ benchmarks, rejects absent-object queries, and is open-sourced.

 Tencent · Tianming Liang, Junyi Pan, Yuxi Li +5

    **#02** ▲111 ★482 ❝3 2026-10-27 

  Hy-Nomas is a diffusion-based video world model supporting free-form text and camera control, preserving causal history across interactions and outperforming existing methods in human evaluation.

 Tencent Hunyuan · Qiaoling Chen, Jiaxiang Cheng, Xueqing Deng +35

    **#03** ▲145 ★1.2k ❝0 2026-11-13 

  Gamer achieves stable generalization and efficient real-time decision-making in 2D and 3D interactive environments via large-scale interaction data pre-training and long-horizon reinforcement learning.

 Affiliation unverified · Run Luo, Kuan Zhang, Qi Deng +24

    **#04** ▲74 ★171 ❝7 2026-10-29 

  JEPAs prove that good views enable efficient, scalable alignment across modalities, outperforming token-by-token prediction in representation quality and sample efficiency.

 galilai-group · Randall Balestriero, Yann LeCun

    **#05** ▲17 ★42k ❝2 2026-11-06 

  e-means outperforms k-means with 10-45x speedup by converging to lower loss in 1/10th the epochs using exponential moving average updates.

 Meta · Swann Bessa, Pierre-Emmanuel Mazaré, Gergely Szilvasy +4

    **#06** ▲105 ★642 ❝0 2026-11-09 

  LingBot-Touch unifies heterogeneous tactile sensor data into a plug-and-play representation, enhancing dexterous manipulation across models and platforms.

 Robbyant · Bing Han, Yichuan Peng, Haoyu Dong +16

    **#07** ▲93 ★894 ❝0 2026-11-04 

  Confucius4-R2T2 is a stable-prefix streaming ASR model based on Longest Stable Prefix (LSP), achieving high accuracy and stability without architectural changes.

 Netease Youdao · Daniel Huang, Qi He, Shaoxiong Duan +3

    **#08** ▲101 ★54 ❝3 2026-10-26 

  The Trinity framework improves multimodal agent performance via capability-driven recursive self-improvement, achieving significant gains on both trained and unseen benchmarks.

 National University of Singapore · Weidong Tang, Ruiheng Wang, Jingxuan Zhang +18

    **#09** ▲127 ★133 ❝0 2026-10-31 

  Wave Forcing (WF) targets real-time long-video generation. The authors note that chunk-wise autoregressive methods need two forward passes for serialized denoising and history-cache refresh even with one-step denoising, and that long rollouts degrade quality and cause camera drift. WF co-designs generation, training and execution for continuous output with multi-step denoising: chunk-wise causal attention reads earlier, cleaner chunks before their denoising finishes, and staggered denoising and cache-refresh stages are spread across full-model replicas, giving one output chunk per concurrent forward round (1F1C) after pipeline fill. A three-stage training procedure adapts the model to this process.

 DENG Lab @ SJTU · Yijie Jin, Yuxuan Liu, Jiajun Xu +7

    **#10** ▲148 ★82 ❝0 2026-11-18 

  Jeveration enables high-quality image generation in a single forward pass, unifying text-to-image, image-to-image, and dense prediction with up to 122x faster speed than existing unified models.

 National University of Singapore · Yuhe Liu, Qi Li, Shihua Zhang +1

    **#11** ▲56 ★3.5k ❝0 2026-11-09 

  MIRIX-2.0 achieves superior accuracy and efficiency in long-term memory across benchmarks, outperforms baselines without fine-tuning, and supports both closed and open models.

 Affiliation unverified · Wei-Chieh Huang, Weizhi Zhang, Jiayan Nan +2

    **#12** ▲33 ★5.2k ❝0 2026-10-26 

  S3AC enables precise token-level advantages and prompt-specific baselines, improving long-chain reasoning performance across model scales.

 Shanghai AI Laboratory · Shuo Shen, Guangran Cheng, Chengqi Lyu +3

    **#13** ▲187 ★26 ❝0 2026-10-19 

  OpenDS-World evaluates deep-search agents on the live open Web: 200 human-verified tasks with unique answers spanning modalities, file formats and interactive access. Among 12 frontier models in one tool environment the best reaches 39.0% accuracy, versus 21.1% for humans over 1,000 attempts; tool coordination and long-horizon search remain hard.

 Affiliation unverified · Xiaomin Yu, Hongliang Chen, Zhe Huang +22

    **#14** ▲49 ★309 ❝1 2026-10-23 

  RSIHub enables composable, comparable agent evolution methods with up to 13.5pp improvement, though gains vary across tasks.

 ByteDance · Zimu Wang, Xiaobo Wang, Siyu Ye +11

    **#15** ▲100 ★148 ❝0 2026-11-02 

  CoworkOS is a general-purpose harness for long-horizon work, addressing the challenge that cowork agents must both deliver high-quality productivity artifacts and keep monitoring and deciding over long horizons. It separates task strategy from execution management: the model decides actions, delegation and acceptance, while the runtime maintains state and feedback. A three-layer architecture supports direct execution or synchronous/asynchronous delegation to subagents with stable responsibilities. Through a shared application layer, humans and agents inspect and revise the same evolving documents, presentations, CAD models and Blender scenes, enabling human-in-the-loop work.

 Renmin University of China · Jiajie Jin, Guanting Dong, Chengjun Pan +37

    **#16** ▲69 ★220 ❝1 2026-11-05 

  SplatAnything identifies a limitation of feed-forward 3D Gaussian splatting (3DGS): Gaussian attributes are usually predicted in a radial-distance-normalized space, yet the spacing between neighboring Gaussian centers still varies substantially across scene scales and camera configurations, shifting scale targets and making supervision ambiguous. The authors propose Gaussian Spacing Normalization (GSNorm), which canonicalizes a robust scene-level spacing statistic to give Gaussian prediction a more consistent spatial reference, and build a systematic benchmark spanning diverse datasets, input sparsity levels and extrapolated viewpoints. The abstract is truncated in our store; concrete results follow the original paper.

 Zhejiang University · Hao Yu, Zhenwei Wang, Yongzhen Hu +7

    **#17** ▲116 ★72 ❝0 2026-10-26 

  A work-centric framework defines two dimensions—persistence and openness—and identifies five core capability families for reliable task execution in agents, with optimization and evaluation analysis.

 Shanghai Jiao Tong University · Zishan Xu, Enjun Du, Mingqi Gao +13

    **#18** ▲58 ★368 ❝1 2026-11-07 

  WorldFoundry provides unified, reproducible infrastructure for persistent, controllable world modeling across video, 3D, and 4D environments with 270 models, 74 benchmarks, and 62 metrics.

 OpenEnvision · Juanxi Tian, Jiadong Hou, Baiang Li +13

    **#19** ▲64 ★79 ❝3 2026-10-21 

  NUS position paper placing recursive self-improvement — improving not just behavior but the improvement process itself — in robot learning, tracing pathways from better robot capabilities to better processes that produce them, and the challenges of partial observability, costly trials and irreversible outcomes in the physical world.

 National University of Singapore · Meng Luo, Zechen Bai, Shanqing Xu +9

    **#20** ▲48 ★305 ❝3 2026-11-01 

  The paper introduces uni-robot-agent, a unified harness for competitive open-weight robot use that supports further training, aiming to reduce robot-use agent (RUA) research's reliance on proprietary frontier models. Without privileged inputs, an out-of-the-box Qwen3.8-Flash-Next scores 31.39 on RoboDojo, above the published GPT-6 Astra-RoboProbe result of 28.97 and many VLA/WAM baselines, and is competitive on 19 real-world tasks. Analysis across model generations, families and sizes links effective robot use to four general capabilities that should be developed jointly.

 Affiliation unverified · Jiarui Yuan, Yuxin Zuo, Xiaoyu Huang +25

    **#21** ▲132 ★6 ❝0 2026-10-22 

  Tencent's FAPL makes video DiTs actually use MLLM latents for cinematic generation: ground the DiT on frozen-MLLM latents, train factor-aware edits (character, scene, action, camera) with paired videos, then freeze the DiT and optimize the MLLM and connector for temporal inference from sparse inputs; it beats baselines on sparse-to-video, factor editing and multi-shot generation.

 Tencent · Songlin Yang, Zhe Wang, Xiaotong Zhao +8

    **#22** ▲56 ★195 ❝0 2026-10-25 

  InteractiveWorld enables verifiable human-AI interaction, revealing current MLLMs' limited reliability in real-world physical tasks despite advances in egocentric AI assistants.

 Meta · Qinsi Wang, Wei Wen, Chong Zhou +7

    **#23** ▲104 ★73 ❝0 2026-11-16 

  InclusionMed is an open medical AI evaluation initiative for inclusive healthcare. The InclusionMed Atlas organizes benchmarks across three healthcare roles and three AI capabilities. The first release selected four public benchmarks from 388 candidates through structured audits and independent reproduction; with the private diagnostic-reasoning benchmark InclusionMed-DxFiles, the Atlas contains five benchmarks covering three of nine role–capability cells. Frontier large language models evaluated on the Atlas are aggregated into the InclusionMed Index, with overall scores from 17.5 to 49.1 out of 100, plus scores by role and capability; a model's rank varied with the role or capability considered. A within-benchmark analysis found that the ten highest-ranked models all scored higher on patient communication than on clinical decision-making, and models with similar Index scores differed substantially in estimated inference cost. The authors call for evaluation covering more diverse healthcare needs and for context-specific model selection, and seek through open collaboration to let diverse healthcare needs worldwide shape how medical AI progress is defined and measured.

 Affiliation unverified ·  The InclusionMed Collaboration

    **#24** ▲77 ★58 ❝1 2026-10-29 

  This report studies the intersection of video generation and the agent harness, an executable runtime around video models that builds context, exposes tools, maintains state, routes feedback and manages budgets. It covers three directions: agentic video generation, where harnesses coordinate scripting, shot planning, asset management, production and editing, plus the evaluation challenges involved; agentic world modeling, where harnesses mediate observation, control, execution and state, including hybrid designs with game engines or agent-written programs; and recursive self-improvement, as named in the title.

 Video Rebirth · Liaoyuan Fan, Yuhao Zheng, Dongxing Mao +18

    **#25** ▲37 ★521 ❝1 2026-10-30 

  Flide is an exact inference engine for decision models such as Jev, which give lightweight, reliable categorical reasoning in LLM agent systems. Generation-oriented engines suffer from heavy prefix recomputation or distorted output probabilities. Flide decouples a request's logical tree from its physical execution within one flattened token stream, and ForkAttention uses GPU-resident indices to navigate dependencies across standard attention and recurrent Gated DeltaNet layers, guaranteeing in exact arithmetic that each branch matches an isolated forward pass. The abstract is truncated in our store; speedup figures follow the original paper.

 National University of Singapore · Bobo Li, Jiajia Song, Hao Fei +2

    **#26** ▲58 ★301 ❝0 2026-11-09 

  WICL achieves 83.4% success rate on 50 manipulation tasks without gradient updates, outperforming trained policies by over 20 points.

 ArcheOne-AI · Heng Fang, Zehui Guo, Yuehan Zhang +3

    **#27** ▲98 ★35 ❝0 2026-11-16 

  The Sage S2.1 Flash technical report introduces Sage S1.5 Flash, a 320-billion-parameter general-purpose mixture-of-experts model for reasoning over scientific evidence and acting in interactive environments. Sage Sci-Perception uses dedicated encoders for molecular graphs and biological sequences and represents spectra as ordered patches of measurements; modality-specific projectors map these into a shared backbone that also processes text and images. Agent capabilities are trained in verifiable environments for tool use, search and coding, plus workflows for machine-learning experiments and scientific software, with completion checked against execution results, environment-state changes or submitted artifacts. On 33 benchmarks across seven domains, versus five baselines, it ranks first on OfficeQA Pro (72.2), MLE-bench Lite (90.3 HumanRank), Harvey LAB and SpreadsheetBench 2, and second on OSWorld-Verified (85.2), WebArena-Verified (67.6) and DeepResearch Bench II (54.2), with particular strength in using documentary evidence, producing verifiable workbooks and carrying out machine-learning tasks, and competitive performance on desktop and browser workflows that require acting on visual feedback.

 Intern Large Models · Kun Cai, Haodong Chen, Guangran Cheng +203

    **#28** ▲70 ★18 ❝0 2026-11-02 

  P²-MOPD achieves faster and more balanced policy recovery than MOPD with half the training steps, reaching 94.7% average recovery at the end.

 StepFun · Sinuo Wang, Jiacheng Lu, Zeyu Zhang +8

    **#29** ▲31 ★829 ❝0 2026-10-29 

  HairDeck generates animatable stylized hair cards from a single image, outperforming existing methods in accuracy and speed using a set diffusion transformer.

 Tsinghua University · Yuze He, Yixiang Wei, Wang Zhao +3

    **#30** ▲115 ★19 ❝0 2026-10-27 

  LOWN is an efficient decoding method for diffusion vision-language models (DVLMs). Visual tokens make up 89-97% of the input, yet removing the image from LLaDA-V changes the unmasked token at only 22.8% of decoding steps on average (as few as 7.9%). LOWN decides at every step whether to process the image and how many tokens to unmask, first predicting masked tokens without the image.

 KAIST AI · Donghyeon Cho, Joowon Kim, Seungho Shin +1

    **#31** ▲47 ★202 ❝1 2026-10-25 

  DynaFlow enables dynamic, free-form multi-agent orchestration, outperforming baselines by 10.90 percentage points across six benchmarks with the same model backbone.

 National University of Singapore · Ruhao Liu, Qi Li, Shizun Wang +1

    **#32** ▲71 ★27 ❝1 2026-11-11 

  SO-Arena is a benchmark characterizing the capability spectrum and decision reliability of System One models (fast decision models without lengthy token generation) across 11 domains and five capability families. It offers evidence-grounded evaluation with manually verified labels, fine-grained domain-specific difficulty criteria, and interactive sandbox evaluation. The abstract is truncated in our library; see the original paper.

 National University of Singapore · Lichen Li, Ziping Dong, Sicheng Feng +2

    **#33** ▲93 ★33 ❝0 2026-11-13 

  AscendGYM provides a cost-aware kernel optimization benchmark with real-world workload configurations and introduces STS to evaluate performance gains relative to token cost.

 SLAI-AITP ·  AI Training Platform Team, Bingwei Zhang, Jing Li +24

    **#34** ▲64 ★12 ❝0 2026-11-03 

  LoCo is a local preference learning method for reasoning compression. Large reasoning models often keep deliberating after they have enough information, inflating inference cost, and existing compression methods mostly act on whole trajectories rather than the local decision of whether more reasoning is needed. LoCo detects candidate overthinking states in the model's own traces, freezes those prefixes, regenerates shorter verified continuations from the same states, and trains on the resulting state-conditioned preference pairs with DPO, teaching the model when to continue and when to conclude rather than simply rewarding shorter outputs.

 Tencent · Muyao Yuan, Liyan Xu, Weizhan Zhang +4

    **#35** ▲38 ★402 ❝1 2026-10-22 

  HiP3R uses a hierarchical pose graph to resolve scale drift and trajectory consistency in long-horizon 3D reconstruction.

 Nanyang Technological University Singapore · Zunnan Xu, Shangchen Zhou, Yihang Luo +1

    **#36** ▲63 ★10 ❝1 2026-10-22 

  UserArena is a real-world e-commerce interactive benchmark with 64,729 session trajectories from 11,010 users, 566,102 items and 13 action types; it pairs them with an interactive environment and offline validation against a controlled production A/B test, enabling five-level evaluation from output validity to sequential behavior.

 alibaba · Jujia Zhao, Jiakai Tang, Jing Yu +6

    **#37** ▲70 ★69 ❝0 2026-11-03 

  Harness-S dynamically transfers repair capabilities from sibling harnesses to improve policy performance under the original base harness.

 Shanghai JiaoTong University · Kangning Zhang, Wenxiang Jiao, Bowen Fang +8

    **#38** ▲114  ❝1 2026-11-05 

  VFlash jointly decodes primitive coordinates for fast, accurate visual planning, reducing prediction errors and achieving ~25x speedup in 3D trajectory forecasting.

 NVIDIA · Yu-Cheng Chou, Shihao Wang, Jin Huang +16

    **#39** ▲60 ★18 ❝0 2026-11-09 

  Sage-Taiyi is a metacognition-inspired architecture for scientific discovery that uses accumulated scientific understanding to decide what to investigate next and how. Through explicit monitoring and control, it revises its understanding of evidence, assumptions and open questions, then adapts research targets and selects reusable reasoning methods. On Taiyi-Bench, an ideation benchmark of 265 tasks from 50 papers across ten disciplines, it ranks first on all eight metrics of historical rediscovery and open-ended ideation, with an Overall score of 54.0 versus 47.9 for the strongest baseline, AI Scientist-v2. It also performs well on two external benchmarks, and five expert-guided collaborations link its proposals to experimental and computational validation, including in chemistry. The abstract is truncated in our store; see the paper for the rest.

 Shanghai AI Laboratory · Weida Wang, Benteng Chen, Yigeng Jiang +87

    **#40** ▲53 ★23 ❝2 2026-10-23 

  TraceTrial-Bench evaluates agent harness reliability across seven dimensions using execution traces and failure attribution, covering 4 harnesses, 9 models, and 95 tasks.

 TraceLite AI · Zhiyong Cao, Hengjie Zhu, Yongxin Wang +20

    **#41** ▲50 ★48 ❝0 2026-11-16 

  WeVisForensics achieves 95.2% detection accuracy across benchmarks using a self-evolving agent framework with dual-timescale policy optimization.

 Tencent · Yipeng Du, Yuxi Li, Ying Zhang +4

    **#42** ▲22 ★286 ❝6 2026-09-29 

  RoboHarness enables Qwen3.8-Flash-Next to outperform state-of-the-art VLA models on BEHAVIOR Challenge 2025 tasks.

 Zhejiang University · Bince Qu, Wei Chen, Bo Zhang

    **#43** ▲71 ★32 ❝0 2026-11-10 

  WikiTrace links insights to original trajectories, enabling agents to verify memory accuracy and reduce memory-induced degradation.

 KAIST AI · Yukyeong Lee, Yumin Choi, Jinheon Baek +1

    **#44** ▲87 ★22 ❝0 2026-11-11 

  VibePrevisBench evaluates agentic 4D scene reconstruction in Blender, revealing multimodal understanding as a key differentiator and deficiencies in handling complex requests and self-correction.

 Kling-Keye · Yuzhe Chen, Muxi Diao, Lele Yang +18

    **#45** ▲74 ★36 ❝0 2026-11-15 

  AllocFlow generates joint portfolio allocations directly from raw market data, achieving superior risk-adjusted returns through a two-stage training scheme without retraining.

 Nanyang Technological University · Annan Wang, Yuchen Xie, Yu Cen +5

    **#46** ▲25 ★367 ❝3 2026-10-22 

  VibeGame enables end-to-end 2D game development from natural language prompts using an AI-native engine and self-evolving agent team, distilling successful projects into reusable modules.

 Nanjing University · Wenbo Hu, Ken Li, Jiazhe Wei +13

    **#47** ▲68 ★52 ❝0 2026-10-24 

  Trajectory2Gym improves tool-use agent performance by constructing decision-grounded, compositionally diverse interactions.

 ant-international · Yihao Hu, Zhihao Wen, Yitong Lin +5

    **#48** ▲42 ★95 ❝1 2026-11-03 

  ToolSpace is a tool-centric environment synthesis framework for long-horizon agents that compose tools across MCP services. Existing MCP environment-scaling methods are data-centric, synthesizing a database and scenario first and deriving tools and tasks from them, so interfaces may not match real ones, compositions aren't grounded in execution and tasks rarely span environments. ToolSpace reverses this: it mines data, state and retry dependencies between adjacent calls in execution traces of real MCP services and open agent corpora using deterministic rules rather than LLM judgment, then clusters the weighted tool-composition graph into environments. Databases are synthesized only for state-mutating environments; query and stateless tools replay recorded returns.

 Renmin University of China · Guanting Dong, Zhichao Sun, Xinyu Yang +22

    **#49** ▲53 ★25 ❝0 2026-10-31 

  BestRand reduces high-error risk and estimation error in benchmark compression while maintaining low cost and sampling efficiency.

 Carnegie Mellon University · George Ling, Shanshan Zhong, Lijin Yang +3

    **#50** ▲81 ★20 ❝0 2026-11-03 

  Route Anything studies model and policy routing in general-purpose AI systems. Rather than one model mastering every capability, the authors argue for orchestrating a heterogeneous ecosystem of specialized models, tools and policies (candidates), with the key question being how to dynamically pick the best candidate per task while balancing performance and compute cost. The paper presents a unified framework and theoretical foundation that characterizes optimal routing under quality-cost trade-offs and shows how the information available to a router determines its attainable performance.

 Affiliation unverified · Pengfei Zhou, Yixing Ma, Hexin Wang +4

    **#51** ▲48 ★32 ❝0 2026-11-02 

  IdeaTaste benchmarks LLM research taste by scoring how closely the proposals a model writes from a literature snapshot frozen at 2025 match the work the same communities published in 2026. Experiments cover six language models and four research areas from established to emerging, with over 90,000 proposals. LLM agents fall well short of human communities, reaching about 26% of human level on citation matching and only 18% on idea matching; model populations can cite as diversely as a community and adopt its tasks and methods, yet rarely reach the same ideas.

 Carnegie Mellon University School of Computer Science · Shanshan Zhong, Chen Xu, Jingjie Ning +3

    **#52** ▲34 ★218 ❝1 2026-10-24 

  AutoTavern builds a faithful Hearthstone simulator and develops TavernAgent that outperforms top human players in gameplay and decision speed.

 Multimedia Computing Group-Nanjing University · Changlian Ma, Limin Wang

    **#53** ▲47 ★149 ❝0 2026-11-11 

  Duo v1 introduces a reciprocal world model enabling real-time audiovisual perception and response in full-duplex interaction.

 inclusionAI · Hengyuan Cao, Biao Gong, Guoshan Lu +17

    **#54** ▲40 ★55 ❝0 2026-11-19 

  WeMM-Video achieves state-of-the-art performance in omni-modal video understanding, with its 35B-A3B variant setting a new open-source benchmark.

 Tencent · Changli Tang, Jie Yang, Tianyi Wang +9

    **#55** ▲166  ❝0 2026-11-17 

  Xing 4 achieves efficient pretraining of a 285B-parameter model on Ascend with optimized architecture and systems, and introduces TeleGym for agent reasoning and policy optimization.

 XingChen-AGI · Xingchen AGI Lab,  :, Baotian Hu +137

    **#56** ▲31 ★98 ❝0 2026-11-17 

  FastFrames is a family of fast autoregressive video diffusion models that achieves interactive latency without compromising quality. Autoregressive video diffusion models generate frames based on new observations and inputs and are a promising medium for interactive visual experiences, but existing models are orders of magnitude too slow for fluid interaction. Profiling the inference pipeline of state-of-the-art models shows that operations often treated as fixed implementation details, such as VAE decoding and KV cache updates, account for over half of end-to-end inference latency; the authors challenge the assumption that these components are indispensable and replace them with lightweight alternatives. The 1.3B variant runs at 36 ms per frame, or 28 responses per second (RPS), on a single B200 GPU while matching the generation quality of a state-of-the-art model that is nearly 8× slower. The 14B variant runs at 45 ms per frame, or 22 RPS, on an 8×B200 GPU node and yields significantly better visual quality and realism. FastFrames is shown to enable real-time interaction in applications such as camera-conditioned and physics-conditioned video generation.

 Apple · Gene Chou, Guandao Yang, Fujun Luan +4

    **#57** ▲56 ★39 ❝0 2026-11-09 

  Infrathon evaluates language agents in long-horizon infrastructure optimization, showing performance shifts over time and agent outperformance of human-designed baselines.

 University of Washington · Zhengyu Hu, Zheyuan Xiao, Yike Wang +18

    **#58** ▲47 ★34 ❝0 2026-10-22 

  This survey unifies video understanding and generation through world modeling, focusing on four operations: reading, writing, sharing, and interaction, shifting from clip-level recognition to persistent, causal state modeling.

 University of Oxford · Shengqiong Wu, Meng Luo, Bobo Li +5

    **#59** ▲30 ★241 ❝1 2026-10-29 

  Intrinsic Distribution Matching improves detail fidelity in text-to-image generation by explicitly constraining feature moments, achieving top scores on LongText-Bench.

 inclusionAI · Deyuan Liu, Yansen Han, Kaiwen Zheng +7

    **#60** ▲59 ★49 ❝0 2026-11-17 

  Weight-space programming enables LLMs to design efficient, accurate models without Transformers, with an associative-memory model up to 17x faster on GPUs.

 Pohang University of Science and Technology · Hyunjong Ok, Jaeho Lee

    **#61** ▲50 ★124 ❝0 2026-11-02 

  CUA-Master is a nested double-loop recursive improvement framework for computer-use agents (CUAs). The authors note that CUAs are limited by the diversity, controllability and reliability of interaction data, that standard pipelines lack iterative feedback, and that recursive methods relying on self-generated experience see diminishing returns. At the system level, an outer world-data-model flywheel keeps incorporating open-world corpora and uses evolving model weaknesses to steer data construction and optimization; at the data level, an inner flywheel couples open-world seeding, which mines authentic workflows from instructional videos as seed tasks, with autocatalytic propagation.

 Affiliation unverified · Xinlei Yu, Gen Li, Ji Zhao +15

    **#62** ▲59 ★2 ❝1 2026-10-22 

  HiSTEVE 1.0 achieves state-of-the-art atomic task performance and long-horizon gameplay in Minecraft via a hierarchical VLA agent with structured planning and native interface control.

 Tencent · Zecong Tang, Zian Guan, Yuncheng Guo +3

    **#63** ▲56 ★72 ❝0 2026-10-23 

  RynnUnify achieves state-of-the-art embodied understanding and planning, with agent and robot control extensions demonstrating strong real-world performance.

 DAMO Academy · Minghao Zhu, Bohan Hou, Kun Xiang +20

    **#64** ▲28 ★205 ❝0 2026-11-19 

  Depth Pro 2 achieves high-metric accuracy, sharpness, and temporal consistency in monocular depth estimation across images and video via a unified scalable network.

 Apple · Peiyun Hu, Marko Mihajlovic, Lyndon Duong +5

    **#65** ▲103  ❝2 2026-10-22 

  IQuest-Q1 is a sparse MoE agentic foundation model for CLI agents (320B total, 15B active), trained in three stages: stable pre-training (Stable SplitNorm, Muon, muP) with a code-heavier second curriculum stage, mid-training with broader data and longer context, and multi-scaffold post-training combining SFT, domain RL and MOPD.

 IQuest · A. Jian Yang, An Huang, Bo Dan +174

    **#66** ▲75 ★19 ❝0 2026-11-02 

  MoviEdit is an end-to-end framework for long-form multi-shot video editing that addresses data, modeling and evaluation together. Such editing must keep an edit coherent across abrupt changes in scene, viewpoint and subject appearance while handling far longer context than single-shot editors. Automatic pipelines spanning synchronized multi-view footage, real multi-shot videos and generated content yield 87K paired editing videos without manual annotation. The model uses a cascaded bidirectional-autoregressive design in which a lightweight bidirectional stage first produces a globally coherent editing draft over a spatiotemporally compressed representation before later refinement.

 Adobe · Suhyeon Lee, Zhengang Li, Soo Ye Kim +6

    **#67** ▲147  ❝0 2026-11-01 

  This survey covers agentic computer graphics: graphics is built on code, and LLM agents can now generate graphics code and refine it iteratively through feedback, opening a possible paradigm where graphics algorithms are built around agentic systems. The authors note that the area is growing quickly and that similar ideas appear under different terminology across tasks, making manual reviews hard to keep current. Its first contribution is a unified taxonomy for characterizing agentic systems across diverse graphics tasks.

 University of Texas at Austin · Hengyu Liu, Jihyeon Je, Ruiqi Wang +11

    **#68** ▲105  ❝0 2026-11-03 

  The paper proposes 'cognitive symbionts' to augment human intelligence through continuous, embedded AI systems that support perception, memory, and thought in real time.

 University of Oxford · Hao Fei

    **#69** ▲40 ★30 ❝0 2026-11-16 

  ALIVE is an interactive streaming video model that supports proactive monitoring, streaming task coordination, and feedback-seeking interaction. Streaming video LLMs can continuously observe the world, yet often handle user requests in isolation. The authors construct ALIVE-SFT-288K and ALIVE-RL-3K for two-stage training: contrastive supervised fine-tuning teaches when and how to respond, while reinforcement learning with Behavior-Aligned Credit Assignment (BACA) combines trajectory-level and local advantages to refine decisions, responses, and silence across multi-turn interactions. They also introduce FSI-Bench to evaluate feedback seeking and subsequent task completion. ALIVE achieves the highest overall scores among the compared models on four public benchmarks and FSI-Bench, demonstrating strong streaming understanding and interactive capabilities.

 Tsinghua University · Jinzhao Li, Yuan Xu, Jinxuan Wu +13

    **#70** ▲38 ★49 ❝1 2026-11-12 

  The paper studies decision models (typed outputs with probabilities) as actor, judge and teacher in long-horizon agents. It compares three decision models and three general LLMs on ALFWorld and uses JEV as a teacher on ALFWorld and WebShop. As actors, models are pulled from a required subgoal by an easier one; as judges, asking whether an action helped rather than whether the task will succeed better separates useful from useless actions. The abstract is truncated in our library; see the original paper.

 Zhejiang University · Haochen Liu, Qianhao Zhu, Zixuan Wang +7

    **#71** ▲26 ★71 ❝1 2026-11-02 

  WebHarbor provides 153 executable, Dockerized real-world web environments evolved via human-agent collaboration for stable GUI agent training and evaluation.

 Microsoft · Zhaoyang Wang, Qianhui Wu, Boyu Gou +54

    **#72** ▲152  ❝0 2026-10-27 

  PDRVLA uses a lightweight physical dynamics module to enable efficient action generation without video simulation, achieving high success rates in simulation benchmarks.

 Affiliation unverified · Hanqing Wang, Zhaotian Cai, Ruichuan An +13

    **#73** ▲46 ★73 ❝0 2026-10-29 

  L3G-World enables real-time, spatially consistent world generation with reduced memory overhead and faster rendering via latent 3D Gaussian memory.

 KAIST AI · Jinhyeok Choi, JoungBin Lee, Jongmin Lee +5

    **#74** ▲136  ❝0 2026-11-13 

  A new agentic robotics paradigm is proposed, featuring a foundation model with embodied harness and a closed-loop evaluation framework for sustained autonomy.

 Grid Robotics · Grid Robotics,  :, Qingfeng Lan +11

    **#75** ▲49 ★68 ❝0 2026-11-02 

  Axion 1.0 is a fully open agent foundation model with a simple recipe: two lightweight supervised fine-tuning stages followed by reinforcement learning, with no dedicated mid-training, DPO or on-policy distillation. The first SFT stage builds self-contained reasoning via model-aware data construction, synthesizing hard problems and adversarially strengthening them based on the base model's own successful strategies; the second extends this to tool-dependent reasoning with a scalable pipeline of diverse, automatically verifiable tasks grounded in real data sources and deterministic computation; RL then strengthens it further.

 Affiliation unverified · Yuqi Jia, Yunjie Tian, Jiayu Zheng +4

    **#76** ▲43 ★69 ❝0 2026-11-05 

  PCBRouteAgent integrates visual and structured data with global planning and local execution, achieving high success rates in complex PCB routing, outperforming baselines on real-world boards.

 Affiliation unverified · Yue Zhang, Jilei Sun, Liqiang Jing +2

    **#77** ▲36 ★38 ❝1 2026-10-22 

  A naive RAG baseline (dense retrieval plus reranking over raw history) ranks first in six of seven settings across LoCoMo, LongMemEval, MEME and MemoryAgentBench, beating the strongest agentic memory system by 0.13–5.53 points thanks to higher evidence coverage.

 Xi'an Jiaotong University · Zhengye Bao, Shengtao Zhang, Haokai Zhang +3

    **#78** ▲95  ❝0 2026-10-26 

  StreamVibe-VL infers implicit user intent from continuous video streams and adapts responses dynamically, improving timing and quality. StreamVibe-Harness enhances consistency and adaptation via hierarchical memory for persistent intent tracking.

 Google · Chengzhi Liu, Yuxiang Lai, Yuzhe Yang +13

    **#79** ▲32 ★352 ❝0 2026-11-07 

  MACaQ enables fast, model-agnostic post-training quantization with low end-to-end overhead and superior perplexity compared to state-of-the-art methods.

 HUAWEI Computing Systems Lab · Lorenz K. Muller, Philippe Bich, Chiara Boretti +3

    **#80** ▲50 ★56 ❝0 2026-10-23 

  WorldCast generates character videos under large-angle camera motions with geometric consistency via 3D point cloud reconstruction and geometry-aligned feature injection.

 Affiliation unverified · Lei Cao, Yu Liu, Wenhua Liao +7

    **#81** ▲45 ★16 ❝1 2026-11-10 

  Tree-Ising targets weight-space programming, where a frontier LLM agent reads a network's weights and writes a repair (a recent MIT work corrected the reasoning loops of Qwen3.5-0.8B in 158 agent turns using 89.7 million tokens). It casts the repair as a sparse discrete optimization over the model's own BF16 weights: forward statistics define an objective that weakens a loop direction, strengthens a concluding direction and preserves the layer output on productive tokens. Admissible edits are sign flips and exponent shifts that keep shared group exponents and mantissas, and the objective decomposes exactly into independent QUBO (Ising) problems, one per output row. The abstract is truncated in our library; see the original paper for details.

 Affiliation unverified · Hadi Salloum, Ammar Ali

    **#82** ▲40 ★110 ❝0 2026-11-11 

  Open Vision Banana provides an open recipe to convert pretrained generators into multi-task perception systems, outperforming prior work in zero-shot settings and improving segmentation via test-time ensembling.

 Fundamental AI Lab at UTN · Arpit Jadon, Valentinos Pariza, Jona Ruthardt +4

    **#83** ▲88  ❝0 2026-11-10 

  APEX achieves better instruction following and visual quality in few-step streaming video generation with reduced GPU memory and iteration time compared to DMD.

 Tsinghua University · Deyuan Liu, Jungang Li, Kaiwen Zheng +8

    **#84** ▲30 ★293 ❝0 2026-11-09 

  Embodied First Exam introduces a unified evaluation framework for coding agents on robots, achieving 34% success rate on core tasks in standard settings with the strongest agent.

 MLL Lab · Kangrui Wang, Qineng Wang, Changhe Chen +27

    **#85** ▲45 ★12 ❝2 2026-10-25 

  CLE benchmarks experience-driven improvement in LLM agents across 600 levels; feedback and revision improve performance by 5.3 pp, yet most models fail to complete all five levels consistently.

 University College London · Haoyu Zhao, Sitian Tu, Yuxuan Huang +16

    **#86** ▲48 ★48 ❝0 2026-10-15 

  R²Orch proposes recursive self-play orchestration for training agent orchestrators. Existing orchestration learning suffers from hand-designed curricula, advantage collapse when all rollouts of a task reach the same outcome, and unreliable evaluation of skills by one noisy score. The paradigm has an inner self-play loop that improves the orchestrator on self-chosen tasks and an outer loop that improves how the inner loop improves. A Recursive Learnability objective rewards a Proposer for choosing tasks whose rollouts keep reaching different outcomes as skills are withdrawn, and a Skill Withdrawal mechanism has the outer loop withdraw each skill in turn.

 Chinese University of Hong Kong, Shenzhen · Mingda Zhang, Yun Huang, Qiang Huang +7

    **#87** ▲27 ★68 ❝0 2026-11-12 

  TTB (Test-Time Baking) targets decoder-only Large View Synthesis Models: KV-cache models give high quality while TTT layers save compute but lose quality. TTB bakes global scene information from cross-view attention into a lightweight MLP, repurposing TTT's fast-weight MLP to learn the attention mapping explicitly, matching TTT efficiency with higher quality, and disentangles camera-only context to fix modality misalignment. The abstract is truncated in our library; see the original paper.

 NVIDIA · Chin-Yang Lin, Ryo Hachiuma, Jaesung Choe +6

    **#88** ▲53 ★23 ❝0 2026-11-06 

  A2-MEAS introduces a code-based embodied multi-agent framework achieving superior performance in robot tasks with robust failure recovery and efficient learning.

 AutoLab · Chao Liang, Ke Chen, Fengxiaoxiao Li +7

    **#89** ▲21 ★78 ❝1 2026-10-28 

  WeVisCut achieves high-quality long-form video generation with 33x fewer tokens per minute than baseline, via shared state and editing inversion data pipeline.

 Tencent · Chenyu Mu, Wenming Weng, Qinrui Wang +8

    **#90** ▲43 ★53 ❝0 2026-11-04 

  TailSteer reduces visual drift in multi-turn image editing by applying corrections at the trajectory tail, preserving base model capabilities.

 MMLab-CUHK · Ruikang Li, Zixuan Chen, Jiarui Wu +6

    **#91** ▲45 ★48 ❝0 2026-11-16 

  ForceHarness enables zero-shot robotic manipulation by frozen multimodal models using force sensing and physics translation.

 Show Lab · Ke Sun, Kevin Yuchen Ma, Yike Zhang +2

    **#92** ▲18 ★391 ❝0 2026-11-17 

  π⁴ recovers 4D scene geometry and motion from uncalibrated RGB inputs with high accuracy and efficiency, using 0.5B parameters and outperforming baselines in geometry and tracking.

 Shanghai AI Laboratory · Yifan Wang, Weiguang Zhao, Ni Yang +4

    **#93** ▲88  ❝1 2026-11-05 

  SLA scales spatial-language-action alignment via continual pre-training using heterogeneous robot data and adaptive feature gating.

 HKUSTGZ · Fuhao Li, Wenxuan Song, Qizhou Huang +7

    **#94** ▲34 ★89 ❝0 2026-11-08 

  InfraRSI achieves up to 2.75x speedup and 30.0% MFU via recursive self-improvement, outperforming existing tools across diverse LLM workloads with cross-hardware experience transfer.

 TokenRhythm ·  InfraRSI Team, Yuchuan Tian, Haocheng Mei +8

    **#95** ▲66 ★8 ❝0 2026-11-10 

  Tideway is a system for efficient inference of large Mixture-of-Experts (MoE) models on memory-constrained consumer devices. There is growing demand to deploy large-scale MoE models locally on consumer PCs to balance data privacy, API costs, and inference efficiency, but unlike datacenter servers with abundant DRAM, consumer devices are tightly constrained in both GPU VRAM and host RAM, forcing massive MoE weights to spill across VRAM, host RAM, and SSD. Existing systems treat these tiers as independent caches and thrash during inference. Tideway's core insight is to manage host RAM and GPU VRAM as a unified memory pool; within this pool, it couples phase-aware memory layouts that adapt to the distinct access patterns of prefill and decode with an execution engine that optimizes CUDA Graph execution under expert paging. On representative consumer hardware with frontier MoE models, Tideway significantly outperforms state-of-the-art systems, achieving average speedups of 1.97× in end-to-end latency and 2.40× in Time-to-First-Token (TTFT), along with 1.61× higher decode throughput against the fastest baseline. Source code is publicly available.

 Duke University · Yifei Wang, Tsun-yu Yang, Yechen Xu +7

    **#96** ▲77  ❝2 2026-11-03 

  This report summarizes observations and insights on multimodal understanding from successive generations of Kling. The authors argue that video generation has moved from isolated clips to coherent short audiovisual sequences, with the bottleneck shifting from rendering to semantic control, and that generation quality is a projection of multimodal understanding across the data-conditioning-alignment-evaluation chain, which they examine module by module. Kling-MLLM, a shared multimodal foundation model unifying image, video, audio, synchronized audio-video and interleaved inputs, is specialized for each module. Because a DiT sees far fewer training samples than a language model, captioning is treated as a semantic contract that maximizes description density, extended to reference-grounded captioning that makes reference bindings explicit.

 KlingAI Research ·  Kling Team, Dong Chen, Jialu Chen +50

    **#97** ▲42 ★8 ❝2 2026-10-20 

  Fudan's Code2Video-Compass is a general benchmark for code-based video generation across T2V, I2V and V2V, testing whether MLLMs can turn visual and temporal requirements into programs that render complete videos.

 Fudan University · Zhenghan Chen, Zekai Shao, Xin Lin +2

    **#98** ▲42 ★42 ❝0 2026-11-12 

  PhysLoop integrates LLMs' physical knowledge to enable self-improvement in video generation through guided generation and localized repair.

 DAGroup-PKU · Yuxuan Chen, Xiaojie Li, Hao Chen +11

    **#99** ▲31 ★60 ❝1 2026-10-25 

  ChronoReplay improves multi-benchmark spatial reasoning by replaying heterogeneous supervision in chronological order, reducing interface lock-in.

 LINs lab · Yihao Hu, Yitong Lin, Pan Wang +8

    **#100** ▲28 ★53 ❝0 2026-10-23 

  Falcon achieves state-of-the-art AVTSE performance with 91.9% fewer parameters and 20.2x lower computation than baselines, on realistic benchmarks.

 Tsinghua Machine Learning Group · Wendi Sang, Kai Li, Yifan Li +2

    **#101** ▲42 ★45 ❝0 2026-11-13 

  MMFlash achieves up to 3.58x lossless speedup in multimodal vision-language models via visual dependence weighting and sparse attention.

 National University of Singapore · Sicheng Feng, Zigeng Chen, Song Wang +2

    **#102** ▲19 ★88 ❝4 2026-10-29 

  CaPtain trains a small open VLM into a capable robot agent via SFT and multi-turn RL, without relying on pre-trained VLAs or frontier models.

 Chinese University of Hong Kong · Yijia Fan, Xilin He, Yilei Jiang +3

    **#103** ▲51 ★4 ❝0 2026-11-02 

  RoboColosseum is a distributed in-the-wild robot evaluation framework that combines unified policy serving and standardized protocols across a network of locally operated robots in diverse environments. An initial study evaluates five vision-language-action (VLA) policies and three frontier AI agents on four embodiments (SO-101, Franka, Bimanual-YAM, G1 Humanoid). The Open Track runs blinded pairwise trials under matched local conditions without task-specific adaptation, and the Fine-tune Track measures how VLA policies adapt to 20 tasks given the same 100 demonstrations per task. The Open Track comprises 5,699 pairwise comparisons at 124 sites.

 Hugging Face · Tan Sang Nguyen, Yi Ru Wang, Shailesh Shailesh +24

    **#104** ▲46 ★30 ❝0 2026-11-08 

  G-STAR addresses the trade-off in discrete autoregressive image generation between long token sequences and weak aggressive compression. Based on product quantization, group size should scale quadratically with per-axis compression under a fixed information budget; the spatial-group co-scaling framework reaches 32x and effective 64x compression while preserving reconstruction, understanding and generation, with a semi-autoregressive decoder for intra-group dependencies. The abstract is truncated in our library; see the original paper.

 LongCat · Xu Huang, Ye Huang, Xiaotong Li +8

    **#105** ▲20 ★185 ❝0 2026-11-02 

  SpecPilot autonomously discovers and optimizes speculative decoding algorithms, achieving up to 10.87× speedup per request and 3.93× with batching across diverse deployments.

 University of California, Berkeley · Kunjun Li, Wenhao Chai, Yihao Liang +7

    **#106** ▲114  ❝0 2026-10-17 

  JD's LayerEditor unifies instruction-guided image editing with controllable generation of reusable RGBA layers: one token sequence jointly represents the edited canvas and an ordered stack of RGB-Alpha layers, so edits stay layer-separable and reusable instead of flattened.

 jingdong · Wen Xue, Yong Liu, Xueheng Li +5

    **#107** ▲52 ★15 ❝0 2026-11-15 

  SWE-CoWork benchmarks 11 coding agents in simulated org settings, revealing significant gaps in requirement tracking and full task completion across the development lifecycle.

 Tokenwave.AI · Xinping Lei, Minghao Liu, Ruijie Zhu +23

    **#108** ▲58 ★13 ❝0 2026-10-26 

  OmniShowcase targets subject-consistent video generation from multi-view reference images: existing multi-reference methods do not explicitly specify the 3D structure linking the observations, and training data does not guarantee the same 3D subject across viewpoint changes. It includes a fully automated data synthesis pipeline that produces training videos along diverse camera trajectories, with viewpoint variation driven by the underlying subject.

 KAIST AI · Junyeong Ahn, Geonwoo Kim, Kinam Kim +4

    **#109** ▲37 ★13 ❝2 2026-10-27 

  GameVerse provides minute-scale, event-aligned gameplay videos that extend the temporal horizon of interactive world models.

 Alaya Lab · Kang He, Wenshuo Peng, Chuanhao Li +7

    **#110** ▲46 ★28 ❝0 2026-11-19 

  Mind2Act World diagnoses robotic manipulation by separating cognition, action, and their composition, revealing that single-capability strength does not translate to closed-loop performance.

 Mind2Act-World · Yuzhi Huang, Jie Wu, Fanding Huang +40

    **#111** ▲29 ★82 ❝0 2026-10-30 

  ReLift reformulates dense 3D tracking as geometric trajectory repair, achieving state-of-the-art accuracy on WorldTrack with improved robustness to occlusion and lifting errors.

 Technion Israel institute of technology · Assaf Singer, Maroun Khriesh, Noam Rotstein +2

    **#112** ▲22 ★91 ❝0 2026-10-26 

  Co-Design Bench is a reference-free benchmark testing whether coding agents can turn real users' multimodal requests into working application revisions, with 273 tasks curated from 37,463 deduplicated queries across 26,314 real coding sessions. It has Multimodal Editing and Repository Editing tracks with 2,331 hidden expert-authored acceptance criteria, graded by a judge-then-refute protocol in which a separate LLM verifier challenges positive verdicts.

 Qwen · Xilin Wei, Shengyuan Ding, Xinyu Fang +13

    **#113** ▲67  ❝0 2026-10-28 

  A-RSI addresses when fresh experience should be introduced in zero-data multimodal self-improvement: it monitors reward variation and accuracy in training logs and refreshes the task pool when the learning signal declines, without consulting validation or test scores. A static library of model-authored programs generates multimodal questions and derives their reference answers, so supervision stays independently checkable rather than relying on self-consistency or majority voting.

 Xiaomi Research · Yu Liu, Youxing Li, Yuedong Tan +17

    **#114** ▲50 ★20 ❝0 2026-10-25 

  AMBIT enables native FOA generation and editing via executable plans and self-distillation, outperforming baselines in content and spatial fidelity.

 NoizAI · Kam Man Wu, Yu Zhang, Pengjun Fang +4

    **#115** ▲41 ★37 ❝0 2026-11-09 

  Reframe improves observation reliance and grounding in visual agents via evidence-aware training and action repair, outperforming baseline methods.

 Nanjing University · Haozhi Yuan, Handong Li, Yifan Zhang +1

    **#116** ▲44 ★30 ❝0 2026-10-14 

  This paper systematically analyzes what generation-side representations in unified multimodal models offer for fine-grained visual understanding, finding them more sensitive to local attributes and spatial relations, and uses native generation-side features directly to strengthen understanding rather than only as auxiliary supervision.

 Affiliation unverified · Tengju Ru, Weitong Lian, Zecong Tang +6

    **#117** ▲59 ★7 ❝0 2026-10-20 

  NAVER AI Lab's LaLa-World is a JEPA-style latent world model with a language-conditioned interface: describing both numerical actions and visual changes in language lets one future predictor train jointly on simulation, robot trajectories and broader image/video data, for planning in imagination and policy fine-tuning.

 NAVER AI Lab · Junha Song, Geonmo Gu, Jeongeun Park +5

    **#118** ▲31 ★113 ❝0 2026-10-19 

  Omni-Woodpecker corrects hallucinations via evidence-prior rebalancing, improving performance across benchmarks with strong cross-architecture generalization.

 Nanjing University · Zijian Liu, Chaoyou Fu, Yan Yang +2

    **#119** ▲30 ★25 ❝0 2026-11-05 

  FewStepRL systematically adapts reinforcement learning objectives to few-step diffusion models via transformation, decomposition, and adaptation, enabling efficient and effective training of multiple methods in a unified framework.

 Tencent Hunyuan · Haitao Lin, Minsi Ren, Yang Li +4

    **#120** ▲35 ★38 ❝0 2026-11-12 

  Ropedia Xperience 2 provides 3D environments and human behavioral recordings, enabling joint scaling of physical AI environments and behaviors.

 Ropedia · Hao Li, Fangzhou Hong, Zhaoxi Chen +19

    **#121** ▲35 ★9 ❝0 2026-11-19 

  Posterior Matching Distillation reconstructs the teacher's full posterior distribution by matching tilted means, enabling diverse and high-quality generation without auxiliary networks.

 NVIDIA · Hidir Yesiltepe, Julius Berner, Arash Vahdat +1

    **#122** ▲65  ❝0 2026-10-29 

  Co-designing System-1 and System-2 reduces execution compute by a factor approaching 1/ρ_u, where ρ_u is the density of utility-relevant interactions.

 Tsinghua University · Qi Wang, Yun Qu, Lizhou Cai +10

    **#123** ▲40 ★22 ❝0 2026-10-27 

  WorkFactory is a framework for scalably constructing coherent, executable professional tasks from professional intents: information-level planning builds an information support graph capturing the dependencies and locations of required information, and probe executions detect support failures to guide repairs. The authors build 112,686 tasks across 612 occupations and fine-tune Qwen models at 4B, 27B and 35B-A3B scales on teacher trajectories, yielding the WorkPilot family, which improves at all three scales.

 IQuest · Ziyang Zeng, Tingyang Chen, Jia Deng +10

    **#124** ▲32 ★109 ❝0 2026-11-01 

  NorthCUA is an integrated system that strengthens computer-use agents through experience at scale. Its Experience Flywheel supplies diverse executable tasks to the NorthCUA Harness, which sustains long interactions, records decisions and checks outcomes, and the Flywheel uses these records to curate verified trajectories. For unsolved tasks, recursive self-improvement evolves reusable skills and keeps verified milestones so later attempts build on earlier progress. Staged Learning combines this experience with grounding data via mid-training and long-horizon SFT, after which the policy learns from fresh attempts in the same Harness through joint desktop-web reinforcement learning.

 beipo-ai · Chaoyun Zhang, Bingxuan Li, Pinxue Guo +27

    **#125** ▲17 ★270 ❝1 2026-11-02 

  Solv3R introduces a looped transformer for iterative multi-view geometry solving, supporting offline, incremental, and streaming reconstruction without task-specific training.

 Affiliation unverified · Jeong-gi Kwak, Andrea Tagliasacchi, Sho Kagami +2

    **#126** ▲38 ★55 ❝0 2026-11-02 

  PhotoGuider tackles composition-aware cropping and 360° view framing. Traditional cropping assumes the photo already contains a good composition and only asks where to crop, whereas a 360° scene first requires finding a viewing direction. PhotoGuider uses a measure-then-frame approach with compact learned scorers over one shared set of composition-aware features, extracted by foundation and task-specific vision models and organized into four photographic tokens: Subject, Composition, Aesthetics and Integrity. A fine-tuned MLLM explainer generates composition-related explanations from images and the measured token assessments. The authors describe it as the first method combining composition-based direction selection in panoramas with refined framing.

 Affiliation unverified · Yuan-Kang Lee, Kuan-Lin Chen, Chia-Che Chang +1

    **#127** ▲47 ★18 ❝0 2026-11-03 

  Factorized MLLM Embeddings (FactEmbed) address composed image retrieval, finding a target image from a reference image plus modification text. The authors argue that MLLM-based methods underuse compositional understanding by mapping composed queries straight into a retrieval embedding with contrastive supervision alone, without resolving the intended visual changes. FactEmbed decouples compositional query comprehension from discriminative embedding construction, using factorized latents that simulate the intended modifications via an intermediate target-imagination objective instead of projecting directly to a single embedding.

 KAIST AI · WonJun Moon, Seongchan Kim, Dahun Kim +1

    **#128** ▲36 ★32 ❝0 2026-11-02 

  EvoMemState introduces relational and reasoning states to model memory evolution and applicability, improving retrieval completeness and response accuracy in long-term dialogues.

 Affiliation unverified · Zhishang Xiang, Yunbo Tang, Zhongqiao Liu +7

    **#129** ▲22 ★91 ❝1 2026-10-28 

  A KL gradient recovery trick improves multi-token prediction by removing noisy gradients, enhancing downstream performance without extra cost.

 Massachusetts Institute of Technology · Jayden Teoh

    **#130** ▲47 ★13 ❝0 2026-11-15 

  Multi-branch Transformers outperform standard architectures in modeling quality and inference efficiency under comparable training compute.

 Affiliation unverified · Gaotang Li, Lingjie Chen, Chunyuan Deng +6

    **#131** ▲26 ★138 ❝0 2026-11-05 

  Drex presents a family of open-source decision models that achieve state-of-the-art accuracy and speed in non-autoregressive option selection tasks.

 Nace.AI · Nischay Dhankhar, Alisher Amantay, Zhanibek Datbayev +6

    **#132** ▲51 ★10 ❝0 2026-10-21 

  KAIST's FRED targets World Action Models, robot policies that decode actions from future video representations taken from a single denoising step of a video diffusion model; it improves those representations via self-distillation with temporal context, since action quality depends directly on them.

 KAIST AI · Huiwon Jang, Suhyeok Jang, Hyeonmo Kang +3

    **#133** ▲47 ★2 ❝0 2026-10-24 

  The FORK reward improves response precision and factual correctness while maintaining rubric coverage, reducing verbosity and reward hacking in rubric-based RL.

 Carnegie Mellon University · Hyun Ryu, Jong-Heon Kim, Yiqing Xie +3

    **#134** ▲28 ★34 ❝1 2026-10-27 

  Using multimodal understanding as a proxy for semantic fidelity, the paper revisits the assumption that quantization error makes discrete visual tokens inherently lossy: discrete representations with much larger errors can understand better, and the effective bit budget of discrete indices is the key determinant of downstream performance, though existing quantizers saturate as added codebooks contribute less.

 LongCat · Jialv Zou, Xiaotong Li, Zhengrong Yue +6

    **#135** ▲23 ★73 ❝1 2026-10-01 

  Closed-weight VLMs outperform open-weight ones across all domains; open-weight models show limited planning and manipulation capabilities.

 Affiliation unverified · Haojian Huang, Pukun Zhao, Zexi Li +7

    **#136** ▲77 ★1 ❝0 2026-10-31 

  The paper proposes Pandora, a psychotherapy client simulation framework. Clients may feel distress before they can describe it, and their expression shifts with emotion, perceived safety and trust, whereas LLM-based simulators built on clinically organized case descriptions tend to explain difficulties more clearly than the client currently understands them. Pandora updates psychological and relational states in response to therapist interventions, builds subjective appraisals and attributions, and plans response content and strategy; a separate expression planner combines these with language patterns distilled from the client's own utterances. It is evaluated against five baselines.

 CLAIN-WHU · Mengxi Xiao, Jingjing Chen, Ziyan Kuang +12

    **#137** ▲23 ★91 ❝0 2026-08-23 

  SparseRead reduces token and latency costs by up to 92.9% and 89.0% respectively, preserving or improving task quality across scenarios.

 Institute of Computing Technology · Zedong Liu, Jiaan Wu, Xinyang Ma +5

    **#138** ▲38 ★18 ❝0 2026-11-04 

  Multi-turn RL improves error recovery and long-horizon resilience in LLM agents, with performance strongly dependent on interaction budgets; BudgetMix RL achieves robustness via dynamic budget variation.

 Chinese Academic of Science Institute of Automation · Kejian Zhu, Zhuoran Jin, Shangqing Tu +5

    **#139** ▲31 ★13 ❝0 2026-10-22 

  Qwen introduces video-centered deep research report generation and OmniDR-Bench, where agents retrieve multimodal evidence and produce interactive reports linked to video moments, scored with Omni Atomic Rubric Trees.

 Qwen · Jiahao Meng, Qize Yang, Yuxuan Wang +4

    **#140** ▲14 ★282 ❝0 2026-10-21 

  Sparse autoencoders extract interpretable, high-performance features from neural detectors' residual streams for auditable text detection.

 GoogleDeepMind · Liam Dugan, Callum McDougall, Matthew Denton +3

    **#141** ▲23 ★40 ❝0 2026-11-04 

  EmoReward dynamically calibrates reward weights in emotional video generation by evolving from evidence reliability during training, improving emotional fidelity and robustness.

 Tencent Hunyuan · Mingzhe Zheng, Yue Wu, Jing Tan +19

    **#142** ▲89  ❝0 2026-11-16 

  K2-Horizon releases a fleet of six fully open language models, from 0.9B to 375B parameters, achieving competitive performance in reasoning and coding via MoVA and Uno, with efficient scaling and synthetic data pre-training.

 Institute of Foundation Models ·  K2 Team, Zhengzhong Liu, Haonan Li +92

    **#143** ▲25 ★30 ❝1 2026-10-25 

  Many dedicated memory systems underperform naive RAG; memory tax reveals hidden losses from memory mechanisms that obscure or remove evidence.

 University of Illinois at Urbana-Champaign · Xuan He, Sining Zhoubian, Ruozhen Yang +8

    **#144** ▲30 ★67 ❝0 2026-11-04 

  TurnWise introduces a turn-level credit GRPO framework that improves streaming video understanding by decoupling learning signals per decision.

 Affiliation unverified · Shuai Dong, Kanghui Tian, Linghan Chen +20

    **#145** ▲56  ❝0 2026-11-10 

  BirdDreams is an autoregressive driving world model that jointly generates perspective video and a schematic bird's-eye-view (BEV) video, making scene layout part of the predicted future. The BEV, rendered from annotations, shows roads and vehicles as simple shapes at fixed metric scale, avoiding the need for future trajectories as inputs. The abstract is truncated in our library; see the original paper.

 NVIDIA · Junyoung Seo, Jaewoo Seo, Seungryong Kim +3

    **#146** ▲16 ★109 ❝0 2026-10-25 

  Increasing reasoning effort in LLM agents does not guarantee performance gains and can be cost-inefficient; a dynamic escalation strategy improves efficiency.

 Together · Kaiser Sun, Yongchan Kwon, Zain Hasan +3

    **#147** ▲57  ❝0 2026-10-27 

  ThinkWorld models actions as Chain-of-Dynamics and uses world imagination with reinforcement learning to achieve effective generalization from human videos in simulation and on real robots.

 NVIDIA · Chaehyun Kim, Chi-Pin Huang, Jisoo Kim +4

    **#148** ▲45 ★11 ❝0 2026-10-30 

  LARC performs latent reasoning in continuous space, improving generation quality and compositional fidelity across text-to-image tasks.

 KAIST AI · Byungwoo Jeon, Yoonwoo Jeong, Hyunseok Lee +2

    **#149** ▲22 ★112 ❝0 2026-11-03 

  VisSearch-Bench evaluates long-horizon visual search in multimodal agents, revealing persistent challenges in linking visual evidence to retrieval results across complex tasks.

 Ant Group · ZhengXian Wu, Qianhao Nie, Zhuohong Chen +10

    **#150** ▲20 ★40 ❝2 2026-10-22 

  AnyGameRenderer is a video-to-video generative renderer that turns gray-blockout games built by coding agents into polished visuals while keeping layout, camera and every entity's position and timing, using plain video as input.

 Alaya Lab · Ruicong Liu, Liangyang Ouyang, Fengbo Lan +2

    **#151** ▲19 ★48 ❝0 2026-10-30 

  Hybrid Computer Use (HCU) enables flexible channel selection and combination, improving efficiency and task success in computer-use agents on OSWorld 2.0.

 Microsoft Research · Haoqing Wang, Xiaoyi Zhang, Sicheng Fan +5

    **#152** ▲33 ★33 ❝0 2026-10-23 

  World-in-Context unifies view, action, and camera controls via in-context conditioning, achieving parity with larger models and high perspective-switching success rate.

 TongyiLab · Harold Haodong Chen, Rui Peng, Rongjin Guo +4

    **#153** ▲26 ★49 ❝0 2026-10-29 

  SpecSlot decouples spectral detail from global attention by localizing high-frequency content, reducing computational cost while maintaining image quality.

 Affiliation unverified · Jiongfan Yang, Weichen Fan, Haiwen Diao +2

    **#154** ▲26 ★26 ❝1 2026-11-04 

  EvoWorld enables causal world evolution beyond visual rendering by integrating world-state reasoning and event-conditioned consequence prediction.

 Affiliation unverified · Yequan Bie, Dengyang Jiang, Zhixuan Chen +5

    **#155** ▲33  ❝2 2026-10-27 

  EmbeddingGemma 2 achieves state-of-the-art multimodal embedding performance with a unified space, outperforming specialists and larger models, while offering flexible resource adaptation.

 Google · Henrique Schechter Vera, Sahil Dua, Jovan Andonov +74

    **#156** ▲19 ★8 ❝6 2026-11-06 

  Weight-space programming achieves significant performance gains via localized parameter edits, showing promise in cross-task transfer but facing reliability and consistency challenges.

 Affiliation unverified · Junru Hu, Cheng Tang, Wei Li +5

    **#157** ▲36 ★14 ❝0 2026-11-10 

  SteerDistill improves task success in quantized LLM agents via closed-loop teacher steering, outperforming baselines on multiple benchmarks with reduced inference cost.

 Affiliation unverified · Genming Cui, Xiangyu Li, Zixu Hao +3

    **#158** ▲15 ★114 ❝1 2026-11-07 

  Deleting fewer than 0.05% of weights achieves full fine-tuning performance without other changes, with much smaller masks and less forgetting than standard methods.

 agentic learning ai lab · Jack Lu, Zhenbang Yang, Mike Lasby +3

    **#159** ▲46 ★5 ❝0 2026-11-04 

  SPILLWAY defends tool-using agents that act on email, files and payments, where an instruction hidden in a web page or shared document can make an agent send data or money to an attacker. Many runtime defenses put a language model on the admit/deny decision, so the check may read the attacker's text and adds a model call at every step; provenance defenses keep the model off the final check but still need a model to plan or route each task first. SPILLWAY instead reads where an action goes from tools' typed returns (such as an email sender or a payment account), while injected instructions arrive as free text, and turns this into a deterministic floor with no per-task plan: a call is admitted when each target value that appears in tool output but not in the user's task was returned by a read-only tool in a typed field or is covered by a fixed delegation rule. A deterministic sanitizer removes instruction-like regions its fixed rules match before the model reads them and retains the removed spans as evidence; a local judge may restore a refused call, but never one whose target lies in the removed text. Across three backbones, and seven task suites on one of them, no standard templated injection redirects an action to an attacker's target under SPILLWAY, at the highest observed mean utility and composite scores (NRP, ERS) among the defenses run under that attack. A forged chat template also produces a raw zero, and mean in-scope attack success under a released reinforcement-learning attacker is 0.007. The floor is reached with no defense-triggered backbone re-run and almost no added model call.

 SAINT Machines · Woosang Lim, Yeseul Chang, Jaewoo Lee

    **#160** ▲34 ★16 ❝0 2026-11-19 

  Coda-DaVinci introduces a task-adaptive agentic framework that dynamically selects proxy-guided or direct generation, improving structure, consistency, and physical plausibility over baselines.

 KlingAI Research · Yiyan Ji, Qixun Wang, Yang Shi +20

    **#161** ▲27 ★14 ❝1 2026-10-25 

  RynnVLA-Latent uses 3D-grounded latent actions to enable scalable robot policy pretraining from heterogeneous videos, achieving high success rates across benchmarks and real-world tasks.

 DAMO Academy · Haoyu Zhao, Xingyue Zhao, Zhian Su +11

    **#162** ▲56  ❝1 2026-10-18 

  Deep Fusion Memory is a chunk-level parametric memory for LLMs: a causal memory predictor generates memory from preceding context and injects it into intermediate hidden states rather than interpolating outputs, avoiding continued-pretraining forgetting and inference-time retrieval cost while cutting offline data-construction work.

 Affiliation unverified · Rubin Wei, Siyuan Sun, Hao Sun +5

    **#163** ▲23 ★78 ❝0 2026-11-16 

  LynnReal-Hand enables precise hand-controlled interaction in video world models via causal flow and region-aware distillation, achieving high fidelity and real-time performance.

 Affiliation unverified · Shaohao Rui, Chaoyang Zhang, Xiaofeng Mao +1

    **#164** ▲22 ★109 ❝0 2026-10-22 

  F2G-Pose is a real-time, single-pass framework for category-level RGB-D object pose estimation that predicts pose, metric size and completed camera-frame shape from one RGB-D observation, lifting dense visual foundation features onto partial point clouds and encoding local geometry with DGCNN tokenization.

 Shanghai Innovation Institute · Jinyu Zhang, Haitao Lin, Jiashu Hou +2

    **#165** ▲29 ★26 ❝0 2026-11-18 

  Game2Player co-develops games and players to scale verified gameplay experience, improving visual agent progress from 14.3% to 31.2% on PlayerBench.

 University of Illinois at Urbana-Champaign · Xirui Jiang, Yuxi Chen, Rui Yang +11

    **#166** ▲22 ★105 ❝0 2026-10-27 

  Venus-DefakerOne2 achieves state-of-the-art performance across diverse forgery types and media, outperforming prior models in both scope and accuracy.

 inclusionAI · Chenfan Qu, Yachao Liang, Mingqi Fang +28

    **#167** ▲20 ★62 ❝1 2026-11-02 

  History Forcing improves long-horizon video modeling by incorporating historical context via residual supervision, enhancing consistency and plausibility in scene evolution.

 Zhejiang University · Hangyi Kuang, Chengzhi Wang, Yefei He +8

    **#168** ▲23 ★41 ❝0 2026-11-09 

  SWAM maintains language-video alignment while predicting actions, outperforming baselines in real-world task generalization.

 Shanghai Jiao Tong University · Yu Zhou, Xinyuan Xia, Xuankun Yang +8

    **#169** ▲30 ★21 ❝0 2026-11-04 

  SimpleHC removes residual mixing and shared memory access reduces training overhead while maintaining performance comparable to mHC.

 dots studio · Xiangdong Zhang, Xiaohan Qin, Sunan Zou +8

    **#170** ▲22 ★71 ❝0 2026-11-06 

  Video-Coder is the first code-driven agentic agent for video generation, significantly improving structural and motion coherence in generated videos.

 University of Central Florida · Chaoyang Wang, Sicheng Gao, Junan Hu +7

    **#171** ▲31 ★18 ❝0 2026-11-16 

  VODA groups per-pixel contributors by local depth structure and directly supervises all Gaussians in depth clusters, improving volumetric segmentation completeness.

 Yonsei University · Jeonghwan Cho, Minsu Kim, Junyoung Hong +1

    **#172** ▲21 ★96 ❝0 2026-10-21 

  The paper introduces a difficulty-based evaluation of benchmarks, identifies two failure modes, and proposes BenchFill to reduce coverage gaps.

 University of Illinois at Urbana-Champaign · Yifan Sun, Naicheng Yu, Jiawen Gong +2

    **#173** ▲45  ❝0 2026-10-20 

  Tsinghua C3I studies cross-layer expert sharing in MoE: since experts in different layers are topologically independent yet highly redundant, it empirically compares per-layer Local Experts with inter-layer shared Meta Experts across multiple dimensions for storage and retrieval efficiency.

 TsinghuaC3I · Ermo Hua, Xiangyu Hong, Baiting Wu +11

    **#174** ▲47  ❝0 2026-10-23 

  Self-training via self-generated questions and reasoning enables domain-wide improvement without external supervision or curated data.

 Google · Leon Sick, Lukas Hoyer, Yannick Strümpler +3

    **#175** ▲12 ★134 ❝0 2026-11-09 

  Evolving Infra adapts compute and KV cache management to agent workloads, reducing end-to-end latency and TTFT by up to 4.1x and 14x over Dynamo.

 MIT HAN Lab · Hao Kang, Weili Xu, Ziyang Li +8

    **#176** ▲19 ★159 ❝0 2026-11-17 

  Invoker-Render enables fine-grained, controllable video generation via structural proxies and appearance references, achieving stability in long-horizon streaming with superior performance over baselines.

 inclusionAI · Ruoyu Wang, Hao Zhong, Yanming Yang +8

    **#177** ▲16 ★25 ❝1 2026-11-02 

  A collaborative intelligence benchmark evaluates AI agents' teamwork, revealing that even with similar individual capabilities, performance varies significantly.

 Stanford University · Sean Wu, Aneesh Pappu, Jiaru Zou +6

    **#178** ▲51  ❝1 2026-10-27 

  Code captions outperform natural language in reasoning and reconstruction, but current MLLMs fail to faithfully capture complex temporal dynamics.

 Affiliation unverified · Jiafu Tang, Yifan Xu, Eric Tang +11

    **#179** ▲18 ★167 ❝0 2026-10-26 

  MatAnyone-E achieves 7.4x speedup and 7.6% memory reduction while preserving matting quality through sparse memory propagation and a lightweight decoder.

 MMLab@NTU · Shuyun Wang, Shangchen Zhou, Peiqing Yang +3

    **#180** ▲40 ★3 ❝0 2026-11-08 

  A unified benchmark reveals utility-fidelity trade-offs in tabular models, with generalists outperforming specialists in utility while fidelity favors specialization.

 Stable AI · Yuanrui Wang, Kaifang Li, Xuanyue Li +4

    **#181** ▲25 ★8 ❝0 2026-10-22 

  AgentVQA is an offline benchmark of 1,000 hand-picked real agent examples from 23 datasets across web agents, robotics, egocentric video, games and spatial planning, for evaluating VLMs in agentic settings.

 zlab-princeton · Advait Gupta, Sachin Konan, Tom Yin +2

    **#182** ▲17 ★191 ❝0 2026-11-15 

  VoxMel is a tokenizer-free framework for autoregressive text-to-speech that makes stable mel modeling tractable through a fixed, exactly invertible two-axis Haar transform: each mel spectrogram is first decomposed along frequency into local averages and a frequency residual, then the averaged component is split along time into Coarse-Mel and a temporal residual. A language-model backbone with a flow-matching head autoregressively generates Coarse-Mel. Non-autoregressive residual tiers then predict the temporal residual from Coarse-Mel, followed by the frequency residual conditioned on the reconstructed frequency-averaged mel; neither residual is fed back into the autoregressive history. Trained on 100K hours of Emilia, the supervised 0.6B model obtains 1.51% WER and 0.738 speaker similarity (SIM) on test-en, and 1.29% CER and 0.755 SIM on test-zh. GRPO improves these results to 1.15% WER / 0.753 SIM and 1.17% CER / 0.776 SIM, respectively. The authors also distill the guided flow head into two- and four-step students requiring 2 or 4 head evaluations per coarse frame; the four-step student retains competitive SIM and nearly matches the supervised model in WER/CER.

 VoxDance-Lab · Wenhao Guan, Jiaxin Ye, Haitao Li +6

    **#183** ▲61  ❝0 2026-11-04 

  FrontierPhysics evaluates end-to-end physics research by agents across nine domains, revealing deficiencies in rigorous reasoning and expert intuition rather than physics knowledge or execution.

 BenchFlow · Bingran You, Wenjun Ke, Ria Rosenauer +69

    **#184** ▲47  ❝1 2026-10-29 

  TOME-Math converts 2,118 math textbooks into 380K structured proof records, significantly improving model performance in mathematical reasoning via preserved curriculum structure.

 Intern Large Models · Youqing Fang, Zhouqi Hua, Yinhao Tang +7

    **#185** ▲32 ★15 ❝0 2026-10-23 

  DiscoDemo generates diverse successful robot demonstrations via RL, improving downstream imitation learning success from 40% to 62% in simulation and 39% to 78% in real world.

 DAVIAN Robotics · Minho Park, Kinam Kim, Donghu Kim +6

    **#186** ▲32 ★3 ❝1 2026-11-04 

  LongBanana is an expert-designed, expert-verified benchmark for long-context multi-reference image synthesis: a model must retrieve dispersed visual evidence, reject distractors, bind entities and relations, and compose one coherent image from many references. It has 90 tasks organized by a 3-by-3 taxonomy, each with about 32 reference images, 20 constraints and 50 evaluation items, for 4,670 atomic items in total. Evaluation is graph-grounded: node, edge and whole-graph obligations become checklist items scored on Content Fidelity, Relational Binding and Global Constraint Satisfaction. The abstract is truncated in our store; remaining results follow the original paper.

 Kolors Team, Kuaishou Technology · Haoxiang Cao, Yuxuan Zhang, Penghui Du +16

    **#187** ▲42  ❝0 2026-11-03 

  OneTrans-GSR unifies reranking and mix-ranking into a single autoregressive model, improving slate quality and reducing latency with scalable compute.

 ByteDance · Feng Cheng, Yu-Hsuan Huang, Fengyi Wang +13

    **#188** ▲32 ★13 ❝0 2026-10-20 

  RLSplat is a budget-controllable feed-forward 3D Gaussian Splatting framework whose representation size, inference latency and peak memory all shrink linearly with the Gaussian budget, instead of predicting dense Gaussians and pruning them.

 Affiliation unverified · Injae Kim, Minseok Joo, Jihwan Park +3

    **#189** ▲34 ★8 ❝0 2026-10-27 

  FrameForge leverages video models' temporal priors with adaptive virtual time relations for superior unified image generation across tasks.

 FrameX-AI · Yijing Lin, Zhendong Mao, Mengqi Huang +2

    **#190** ▲36  ❝0 2026-11-13 

  Self-Improvement Memory enables sustained optimization by recording and reusing rationale and outcomes of prior edits, outperforming baseline methods across domains and models.

 Google · Xin Yu, Fei Xia, Jiaqi Pan +7

    **#191** ▲26  ❝2 2026-10-29 

  Wizard enables real-time, precise character control across novel scenes and characters via structured proxies and adaptive masking.

 Tencent · Yuzhe Wu, Zijun Lin, Cheston Tan +2

    **#192** ▲25 ★21 ❝0 2026-11-17 

  TCP4D uses a unified 4D representation to track and generate TCP trajectories from monocular videos, enabling cross-embodiment transfer and future prediction.

 Affiliation unverified · Haosong Peng, Xiangyang Liu, Zhengshen Zhang +5

    **#193** ▲59  ❝0 2026-11-04 

  NEO-VLA eliminates visual encoders, enabling action policies to access evolving visual representations across layers, achieving superior and more robust control in manipulation tasks.

 MMLab@NTU · Weichen Fan, Haiwen Diao, Penghao Wu +14

    **#194** ▲15 ★142 ❝0 2026-10-28 

  ILE evaluates AI agents' full equity research process, revealing that top models fail to translate research quality into forecast accuracy.

 Affiliation unverified · Huangfei Jiang, Anning Hu, Kai Sheng +7

    **#195** ▲26 ★19 ❝0 2026-10-26 

  CEPO separates coordination and execution in multi-agent systems, consistently improving performance across multiple benchmarks and optimizers.

 Affiliation unverified · Jongha Kim, Seohyun Lee, Jonathan Minho Chung +2

    **#196** ▲31 ★4 ❝0 2026-10-17 

  HABIT-Bench evaluates whether agent memory systems can infer user habits from weak evidence spread across conversations and apply them appropriately. It builds synthetic user histories from task-oriented dialogue data and controlled habit graphs, spanning hundreds of sessions across food, finance, software and travel, with four-choice tasks that include temporary exceptions and habit changes.

 Shanghai Jiao Tong University · Junming Zhang, Rubin Wei, Xu Cheng +7

    **#197** ▲22 ★6 ❝2 2026-10-22 

  Verus-Insight frames explainable artifact detection in AI-generated videos as inspection planning, evidence verification and evidence aggregation, so source decisions rest on faithfully identified artifacts.

 Affiliation unverified · Zhuoran Zhang, Yang Shi, Qixun Wang +11

    **#198** ▲12 ★130 ❝1 2026-11-16 

  MedJev directly maps images and answers to decisions, achieving 6.04x speedup with improved accuracy in medical image understanding.

 The Chinese University of Hong Kong · Fan Zhang, Hao Wu, Jinyang Wu +5

    **#199** ▲36 ★4 ❝0 2026-10-20 

  The paper presents ReTrace, a scalable evaluation framework for systematically studying recovery behavior in computer-use agents. These agents are fragile over long horizons as errors accumulate and actions leave side effects, yet environment resets are infeasible in proprietary settings, action replay is too costly, and API rollback works only on the web, leaving model-based recovery largely unexplored. The authors formalize Transition-Consistent Recovery: identical downstream actions from the recovered and target states must yield identical outcomes.

 KAIST AI · Byeonghun Bang, Sangwoo Park, Jinheon Baek +3

    **#200** ▲26 ★16 ❝0 2026-10-27 

  EvidenceConnect synthesizes evidence-connection data with RLVR training, improving fine-grained spatial perception and multimodal reasoning.

 inclusionAI · Lai Wei, Yatai Li, Liangbo He +10

    **#201** ▲29 ★10 ❝0 2026-11-10 

  MMDM evaluates 28 decision models across 16 real-world scenarios, revealing strong performance on knowledge tasks but poor handling of interdependent options and multi-select reasoning.

 The Chinese University of Hong Kong · Junhao Shen, Qing Zong, Yue Guo +3

    **#202** ▲56  ❝0 2026-11-03 

  DreamX repurposes a pretrained video diffusion model as a universal latent action learner. Existing latent action models infer actions from visual transitions, but their representations often entangle the action with irrelevant visual variation, limiting cross-embodiment transfer. DreamX extracts compact latent actions from the video model's spatiotemporal features, compressing each latent frame by about 300x (1,200 to 4 tokens), and uses the model's generative prior so that the latent actions capture action-relevant dynamics disentangled from appearance, forming a shared action space.

 KAIST AI · Jisoo Kim, Chaehyun Kim, Junsu Kim +6

    **#203** ▲25 ★22 ❝0 2026-10-22 

  TWIG represents every network weight as a path of shared small discrete decisions (sign, power-of-two shift, zero gate, shared-table index), with related weights sharing upper path segments; any trained network can be written this way, turning pruning, quantization, low-rank factorization, codebooks and weight sharing into edits of one object that a single optimizer can choose among.

 ITMO University · Ammar Ali, Stamatis Lefkimmiatis

    **#204** ▲26 ★18 ❝0 2026-11-03 

  VidUEU-Agent is an automated pipeline that synthesizes instruction data from raw videos for multimodal understanding, editing and unified tasks. High-quality instruction data is scarce and costly, while raw videos are abundant but hard to turn into precise instructions. The pipeline has four stages: global perception to filter low-quality shots, mining keyframes and multimodal signals, building metadata, and routing metadata to synthesize instruction samples. Notably, the agent can autonomously iterate the data distribution based on downstream training results to improve quality, balance and training effectiveness.

 Affiliation unverified · Tengju Ru, Weitong Lian, Zecong Tang +12

    **#205** ▲13 ★43 ❝0 2026-10-29 

  A single likelihood-based algorithm unifies pretraining, SFT, and RL, improving accuracy-compute tradeoff and task coverage.

 Carnegie Mellon University · Yuda Song, Alvin Zhang, Fahim Tajwar +7

    **#206** ▲27 ★15 ❝0 2026-11-04 

  Loopback Transformer achieves lower validation loss and higher downstream accuracy with fewer parameters via dynamic update weighting in residual loops.

 University of Wisconsin - Madison · Xuan Ouyang, Zefan Cai, Chenlong Zhang +3

    **#207** ▲18 ★61 ❝0 2026-11-09 

  WaveGen achieves end-to-end text-to-waveform generation without intermediate representations or external modules, outperforming state-of-the-art on Emilia with WER 1.47 and SIM 0.75.

 Ajou university · Sang-Hoon Lee, Ha-Yeong Choi

    **#208** ▲33 ★5 ❝0 2026-11-11 

  Breath-Dx introduces a comprehensive benchmark for breath-based disease screening, achieving up to 0.76 ROC AUC under realistic sensor drift conditions with state-of-the-art time series methods.

 Affiliation unverified · Ivan Poddiakov, Ivan Kruzhilov, Galina Zubkova +3

    **#209** ▲26 ★14 ❝0 2026-10-22 

  TTR-Compressor treats sequence compression as test-time regression, using closed-form fitted coefficients as the compressed representation, and serves as the compressor for block-wise sparse attention and the visual token merger in multimodal LLMs.

 CUHK SphereLab · Sihan Yang, Jie Huang, Hanxuan Li +4

    **#210** ▲24 ★23 ❝0 2026-10-30 

  The paper surveys multimodal embedding fragmentation, proposes a framework of coverage, comparability, and compositionality, advocating joint embeddings for deep integration.

 CodeFuse AI · Ziyin Zhang, Yuyu Guo, Ying Zhang +15

    **#211** ▲34 ★4 ❝0 2026-10-29 

  VideoMemBench evaluates in-context video memory in MLLMs, revealing significant challenges despite Claude Opus 5 achieving the highest score among evaluated models.

 Affiliation unverified · Zhengdong Hu, Chao Wang, Kaituo Feng +4

    **#212** ▲34  ❝1 2026-10-29 

  Reinforcement learning incentivizes LLMs to self-learn from long-form texts, improving proof accuracy and cross-domain problem-solving performance.

 Intern Large Models · Lingkai Kong, Yuzhe Gu, Haiteng Zhao +6

    **#213** ▲53  ❝0 2026-10-26 

  Rollback uses world models to explore counterfactual futures by rolling back to decision points, enabling fine-grained action-level policy improvement with stable and significant gains.

 AgiBot World · Jiayi Luo, Yang Zhang, Liliang Chen +13

    **#214** ▲23 ★4 ❝0 2026-11-19 

  TuringSpace is a synthetic web environment of 125 interactive CAPTCHAs spanning five spatial abilities, solved through clicks and drags on the screen, to test whether multimodal large language models (MLLMs) can, like humans, infer the 3D layout behind a 2D view, predict how an action changes it, and correct themselves after seeing the result. Existing CAPTCHA benchmarks are largely solved and rarely spatial, while spatial benchmarks rarely require action. Every task is a program that generates new instances with a checker and an oracle, so the tasks also form a scalable training gym. Humans solve 94.1% of the tasks, whereas the best model, GPT-6 Astra, still fails 25.9% of them, and the best open-weight model, Kimi K3, fails 68.0%. Further analysis shows that most failures come from misreading the screenshot, and that model reasoning rarely corrects a wrong belief but hallucinates evidence to defend it. Fine-tuning Qwen3.5-9B on this gym raises its success rate from 1.6% to 33.1%, above Kimi K3, and improves results on all seven out-of-distribution benchmarks, including a 9.9-point gain on the real-world indoor videos of VSI-Bench. The authors will release TuringSpace to support further study.

 Meta · Xuwei Ding, Skylar Zhai, Linxin Song +9

    **#215** ▲22  ❝1 2026-11-03 

  Frontier LLMs maintain task accuracy with short contexts via frequent compaction, challenging the necessity of long-context models.

 NVIDIA · Peter Belcak, Saurav Muralidharan, Jindong Jiang +9

    **#216** ▲33  ❝0 2026-11-05 

  ReproBridge improves ML experiment reproducibility, raising success rates to 92.6% and reducing API costs by 19.7% on average.

 Google · Yubo Wang, Rui Meng, Jaehyun Nam +2

    **#217** ▲33  ❝0 2026-11-13 

  PaperCritic outperforms existing models in assessing paper soundness and demonstrates transferability to diverse domains like code and legal review.

 Carnegie Mellon University · Hyun Ryu, Ji Yong Cho, Eunho Yang +4

    **#218** ▲34  ❝0 2026-10-27 

  LociArt enables free-style local video stylization with precise spatial control and preserved non-target content via a unified framework.

 Xiaomi Research · Yuxuan Chen, Feng Xie, Jiagao Hu +4

    **#219** ▲41  ❝1 2026-11-01 

  SWE-Trail studies how to collect training trajectories for coding agents. Sampling many trajectories independently per issue and then curating them yields increasingly redundant later samples, even though earlier ones could guide where effort goes. Simply showing history to the agent as advice makes the trained model rely on advice that is unavailable at test time, creating a train-test mismatch. The authors argue that history should guide where collection effort is spent while the agent decides what to generate, and introduce SWE-Trail on that basis.

 Ant Group · Ziming Li, Lu Yu, Jiatan Huang +3

    **#220** ▲14 ★27 ❝0 2026-11-02 

  SpecOPD improves acceptance length and throughput via branch-parallel on-policy distillation, achieving up to 41.45% gain in mean acceptance length and 16.09× supervised-token throughput.

 Qwen · Tianyu Liu, Jun Zhang, Wenjie Huang +12

    **#221** ▲22 ★24 ❝0 2026-11-17 

  Heave-Ho enables decentralized, long-horizon collaboration among humanoids using local observations and speech-based coordination, outperforming baselines.

 University of Southern California · Ting-Hsuan Chen, Siheng Zhao, Han Jiang +7

    **#222** ▲19 ★20 ❝0 2026-11-04 

  D3D decouples diffusion drafting from target model state updates, enabling asynchronous speculative decoding with significant speedups and layer pruning for cost efficiency.

 Fudan University · Baoyou Chen, Ruiqiao Mei, Guancong Lin +4

    **#223** ▲13 ★176 ❝0 2026-11-16 

  MovieRoll enables high-quality long-form video generation with reduced cache cost via a base-to-distilled framework combining structured training and efficient inference.

 Affiliation unverified · Yunyang Ge, Weijie Kong, Ryan Xu +9

    **#224** ▲33  ❝0 2026-10-24 

  Qwen-Image-PE improves text-to-image prompt design via two-stage training, outperforming baselines in visual quality and generalizing across T2I backbones.

 Qwen · Zekai Zhang, Yanran Zhang, Shengming Yin +26

    **#225** ▲33  ❝0 2026-10-23 

  Retrieval heads remain sparse and cross-modal across modalities, and optimizing them improves document retrieval performance in multimodal models.

 Tencent · Jia Wang, Yuqing Li, Jiangnan Li +2

    **#226** ▲29 ★7 ❝0 2026-10-10 

  Skill adaptation improves final performance across models in self-evolution by tailoring skills to executor capabilities.

 University of Chinese Academy of Sciences  · Zhihao Yang, Haojin Wang, Juncheng Wu +7

    **#227** ▲23 ★17 ❝0 2026-10-13 

  ProAS enables fine-grained credit assignment in multi-turn agentic RL via a dynamically evolving reference graph, improving success rates and accelerating training convergence.

 LINs lab · Bowen Ding, Mingda Wan, Yuhan Chen +7

    **#228** ▲13 ★25 ❝0 2026-10-22 

  BrickWright ensures 100% physical validity in LEGO builds via assembly API and validation, improving prompt adherence across agents.

 Generative Intelligence Lab (CMU) · Ava Pun, Kevin Dai, Sally Chen +7

    **#229** ▲19 ★8 ❝1 2026-10-26 

  Visual memory improves historical recall but reduces current accuracy and forbidden entity avoidance, showing fidelity does not imply correctness.

 Affiliation unverified · Yuzhuo Chen, Huafeng Shi, Xinyu Wang +6

    **#230** ▲21 ★21 ❝0 2026-11-19 

  ZeroVLN is a slow-fast vision-language navigation (VLN) framework that achieves effective navigation with zero navigation-domain supervised fine-tuning of the vision-language model (VLM) brain. Representative VLN methods typically couple a VLM brain with a learnable action head and fine-tune the brain on navigation-domain data to predict geometric goals, but such adaptation can substantially alter base VLM weights and degrade general visual capabilities. Rather than teaching the brain to predict geometric goals, ZeroVLN equips it with a semantic tool-calling interface that conveys movement decisions to an action expert in natural language. The action expert is first adapted to carry out these movement requests, after which closed-loop reinforcement learning jointly improves the brain's movement decisions and the action expert's continuous execution. Trained on the same navigation episodes, ZeroVLN substantially outperforms representative supervised-fine-tuning-based hierarchical VLN methods on standard VLN-CE (continuous environments) benchmarks, while inducing only minor changes to the brain's weights and largely preserving its general capabilities. To the authors' knowledge, ZeroVLN is the first slow-fast VLN framework with zero navigation-domain SFT of the VLM brain, and the first to jointly optimize its slow and fast policies through closed-loop agentic reinforcement learning in VLN-CE.

 ZYT AI Team · Zixuan Huang, Yang Zhou, Guli Zhang +8

    **#231** ▲23 ★14 ❝0 2026-11-19 

  HiUni shares a single vision-language backbone for both understanding and generation, using pixel diffusion and HiGate to preserve spatial details and improve generation.

 ernie-research · Jiahao Hu, Jingdong Du, Yichen Zhang +10

    **#232** ▲39  ❝0 2026-11-17 

  Agent evolution stalls not due to poor proposals, but because improvements are hard to reliably detect due to noisy evaluations.

 ShanghaiTech University · Yuanhao Zeng, Shaocheng Lan, Ao Lu +8

    **#233** ▲20 ★4 ❝0 2026-11-16 

  Accelerate Bench evaluates coding agents on application-level program optimization with 32 CPU optimization tasks adapted from scientific applications and supercomputing competitions. Each task provides a working program and specifies the workload, correctness requirements, timing boundaries and permitted edits, leaving agents to choose where and how to optimize. Public testing supports iterative development, and a separate verifier scores the final submission. On benchmark version 1.1, 16 coding agents, each pairing a model with a harness, obtain Score@1 from 25.5% to 67.4%. Most of their programs are correct, and the size of the speedup separates them: the two leading agents are at least 2× faster on 22 or more of the 32 tasks, while the other fourteen reach 2× on 8 to 17 and leave 125 of their 412 correct outcomes below 1.25×. Controlled replays show why coordination across an application matters: identical source code runs between 0.29× and 2.12× depending only on build and runtime settings, and one code change is worth 1.01× or 6.4× depending on another. The benchmark pairs executable workloads with agents' programs and trajectories to evaluate both their gains and the process behind them.

 Tencent Hunyuan · Bowen Liu, Yuhan Li, Nuo Chen +7

    **#234** ▲30  ❝0 2026-10-24 

  AV-FABLE reveals limited detection capability of AV-LLM language outputs, while frozen feature readouts enable strong manipulation localization in long-form videos.

 Tsinghua University · Yuesheng Huang, Hoi Fan Au, Jiaqi Hou +8

    **#235** ▲34  ❝1 2026-10-22 

  FilmFlow is an agentic system for 3D virtual cinematography that turns natural-language instructions or reference videos into executable camera trajectories in dynamic 3D scenes; a Director agent composes concurrent atomic operations for translation, rotation and framing, supported by specialized agents such as an Interpreter.

 Affiliation unverified · Yiming Zhang, Jiarui Guo, Haojia Wei +5

    **#236** ▲27 ★6 ❝0 2026-11-13 

  V-JEPA-LM examines why video-language models fail to read dynamics: encoders such as V-JEPA learn direction, speed, order and state changes from video, but captions and QA only name a few attributes, so the VLM retrieves them instead of reading the clip. The abstract reports a linear probe reads speed from frozen features at 87% while the VLM answers at chance, locates dynamics in a shared subspace of predictive encoders, and builds counterfactual supervision via pixel transformations producing answer-balanced video variants. The abstract is truncated in our library; see the original paper.

 Affiliation unverified · Keuntae Kim, Dongjun Kim, Yong Suk Choi

    **#237** ▲47  ❝0 2026-11-05 

  WeLM-Image is a unified image foundation model for single-GPU deployment in WeChat's large-scale consumer apps, folding multimodal understanding into text-to-image generation, instruction-guided editing, and multi-reference synthesis via an interleaved sequence of text, reference images, and target latents. Its Mixture-of-Transformers backbone shares attention while keeping understanding and generation parameters separate, with MoE on each path; the abstract cites about 31.7B stored parameters and roughly 5.4B generation parameters activated per target latent token, plus a generation-oriented data engine with a knowledge-grounded visual vocabulary and large-scale visual clustering. The abstract is truncated in our store; remaining training details follow the original paper.

 Affiliation unverified · Canmiao Fu, Chen Li, Guohui Zhang +13

    **#238** ▲10 ★90 ❝1 2026-11-06 

  Energy-Based Reasoners achieve or exceed prior models in reasoning tasks with 345× lower inference compute and superior solution coverage on multi-solution benchmarks.

 Affiliation unverified · William Xingxu Chen, Alexi Gladstone, Zeyu Michael Li +2

    **#239** ▲17 ★3 ❝3 2026-10-01 

  MMVistaReason presents an open-data post-training recipe that achieves superior multimodal reasoning performance with fewer samples through structured supervision and capacity-aware training.

 Zhejiang University · Juekai Lin, Honglin Lin, Yuqian Yuan +7

    **#240** ▲28  ❝0 2026-10-22 

  Cola2 is a hierarchical language-modeling framework that compresses dynamically segmented semantic spans into continuous latents and realizes text with a block-causal masked diffusion decoder, with the encoder and decoder co-adapting.

 ByteDance Seed · Hongcan Guo, Yian Zhao, Feng Wang +4

    **#241** ▲16 ★6 ❝0 2026-10-22 

  RIDAR detects and intervenes in research integrity violations by autonomous research agents, using a 14-type taxonomy; its post-hoc detector reaches 85.2% F1 and finds 3,964 violations in 99.7% of 300 real agent trajectories.

 Knowledge Engineer Group @ Tsinghua University · Jiening Siow, Amy Xin, Junjie Wang +7

    **#242** ▲42  ❝0 2026-10-23 

  Principia constructs and refines scientific principles at test time, outperforming baselines and producing plausible, literature-consistent principles.

 KAIST AI · Dongki Kim, Sangwoo Park, Jinheon Baek +2

    **#243** ▲22 ★3 ❝1 2026-10-03 

  EvalResearchBench tests whether agents can design their own evaluators under fixed time and API budgets by selecting or synthesizing tasks and writing graders; compared against 14 target benchmarks, the best evaluators order about 75% of candidate pairs correctly.

 AG2AI · Yaolun Zhang, Tianyi Xu, Yujie Zhao +3

    **#244** ▲18 ★18 ❝0 2026-11-12 

  WorldDraft evaluates video rendering fidelity against code-defined 4D worlds, revealing tension between world preservation and visual consistency, with WorldDraft-Primer improving both objectives.

 Affiliation unverified · Zhiyu Hou, Wen-Jie Shu, Harold Haodong Chen +7

    **#245** ▲21 ★13 ❝0 2026-11-18 

  WorldRuler enables persistent rule state storage and accurate visual realization of user-defined rules in interactive worlds via an executable 3D rule graph.

 Alaya Lab · Mingliang Zhai, Chuanhao Li, Jiaming Tan +6

    **#246** ▲29 ★3 ❝0 2026-10-14 

  RouteWeaver unifies single-round, multi-round and agentic routing in one LLM policy that learns both which mode to use and which models to call; its COMET-GRPO splits the trajectory advantage into separate signals for mode selection and within-mode execution.

 Nanyang Technological University · Xiaohan Wang, Haozhen Zhang, Qingyuan Liu +2

    **#247** ▲17 ★29 ❝0 2026-11-05 

  EDGAR uses synthetic failure data and diffusion modeling to refine Gaussian avatars, removing artifacts while preserving identity and improving consistency under extreme conditions.

 KAIST · Kwan Yun, Minhyeok Bang, Colette Suhjung Koo +1

    **#248** ▲41  ❝0 2026-11-19 

  MC-Lens reveals concept accessibility and selective access in VLMs, showing concepts participate in downstream reasoning and shared patterns across models.

 University College London · Jiahao Huang, Jun Wang

    **#249** ▲19 ★13 ❝0 2026-10-31 

  StreamDuo improves entity recall and reduces game leakage in unseen games by combining a trainable commentator with a frozen knowledge agent.

 Alaya Lab · Zixian Gao, Penghao Luo, Xiangcheng Cao +5

    **#250** ▲12 ★19 ❝0 2026-10-17 

  DuoLM achieves comparable recommendation quality to expert models with 60x higher throughput via compact encoding and efficient LLM serving.

 GoogleDeepMind · Jiang Wu, Yutong Song, Hongbo Wang +2
