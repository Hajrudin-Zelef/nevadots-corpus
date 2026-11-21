---
id: collect-261121-veille-ia/robotics-kyutai/kyutai-reki-model-adds-robot-memory-without-the-usual-compute-tax
title: "Kyutai REKI Model Adds Robot Memory Without the Usual Compute Tax"
domain: robotics-kyutai
role: reference
task: article
actors: []
dates: []
keywords: ["action", "model", "reki", "robot", "kyutai", "memory", "tokens", "expert"]
source: https://www.remio.ai/post/kyutai-reki-model-adds-robot-memory-without-the-usual-compute-tax
source_anchor: ""
source_lines: [1, 181]
section: "Robotics_Kyutai"
---
Kyutai released a 2.4-billion-parameter robot model that remembers recent observations while adding only 0.1 milliseconds to each measured action prediction. The Kyutai REKI model puts that memory inside its action expert, avoiding repeated processing of earlier camera frames. That design targets a stubborn tension in robot learning: memory helps manipulation, but historical context often increases training and inference costs.

REKI, short for Registering Knowledge in Vision-Language-Action Models, combines a vision-language backbone with a smaller module that predicts robot actions. Kyutai says 32 persistent register tokens let the model retain information from recent frames. The registers remain available while the robot predicts its next action chunk.

That approach pressures systems that obtain temporal context by feeding several images through a larger vision-language model. It also challenges the assumption that stronger manipulation policies must rely on larger models or private training collections. REKI uses public data, publishes its training pipeline, and reports competitive results against models with substantially more parameters.

The result is not proof that a compact, open robot model can replace every larger alternative. Its strongest evidence comes from simulation benchmarks and controlled real-robot demonstrations. However, its architecture presents a specific, testable argument: short-term memory belongs near action generation, where it can remain cheap and directly useful.

## The Kyutai REKI Model Makes Memory Part of Action Generation

**REKI’s central change is architectural: recent visual context becomes a cached part of the action expert instead of another expensive vision input.**

A vision-language-action model, or VLA, converts images and language instructions into physical actions. In REKI, a Qwen3.5-2B vision-language model first encodes camera views and the instruction. A 200-million-parameter action expert then reads that representation and generates an action sequence through flow matching.

Flow matching is a generative method that turns an initial noisy action sequence into a usable trajectory over several refinement steps. REKI uses ten Euler steps during the latency tests reported by Kyutai. Robot state and the current flow time enter the action expert through adaptive normalization.

The complete model contains 2.4 billion parameters. According to the official REKI architecture, about 2.2 billion belong to the vision-language backbone. The remaining 200 million form the action expert.

The important addition is a set of 32 learned register tokens for each observed frame. These tokens summarize the encoded instruction and camera views. They then remain in a key-value cache, which stores reusable attention information for later action predictions.

REKI trains on windows containing three frames from one episode. Those frames are sampled one second apart. During inference, the model retains registers from the preceding two frames and drops the oldest set when a new frame arrives.

That window gives the policy a brief record of what it recently saw. A robot can therefore distinguish a continuing action from a newly encountered state. It does not need to infer everything from the latest image alone.

The register tokens never attend to generated action tokens. They depend on observations rather than a particular robot state, flow time, or denoising step. Kyutai can consequently compute them once per frame and reuse them throughout action generation.

This separation matters because flow-based action generation repeats several calculations. Recomputing historical visual context at every refinement step would multiply the expense. REKI keeps the observation summary stable while its action tokens continue changing.

Kyutai reports that predicting one chunk from two 256-by-256 camera images takes 21.8 milliseconds on one Nvidia H100. Reading the cached registers adds 0.1 milliseconds. Each retained frame adds approximately 2 MiB of GPU memory.

Those measurements apply to Kyutai’s compiled implementation and specified hardware. They do not establish latency on smaller accelerators or complete robot systems. Camera capture, networking, safety logic, and actuator control can all increase end-to-end response time.

Still, the architectural result is concrete. REKI adds a limited temporal window without rerunning a larger model across all past frames. It also avoids a separate memory network that would require another training and inference path.

Kyutai released the pretrained model and several benchmark-specific checkpoints through the REKI model card. The weights use a noncommercial share-alike license, while the accompanying code uses the MIT License. That distinction matters for organizations evaluating commercial deployment.

## Efficient Robot Memory Changes the Cost Equation

**The Kyutai REKI model argues that useful robot memory does not require an ever-growing video context or a second reasoning system.**

Many manipulation policies act mainly from the current observation. That design keeps inference manageable, but it removes evidence about how the scene reached its present state. A single image may not reveal whether a grasp just failed, an object moved, or the robot started retracting.

One response is to send several historical frames through the vision-language backbone. That provides direct temporal evidence, but every additional image contributes visual tokens. Compute and memory demands then rise with the context window.

Another approach uses recurrence. A recurrent policy passes a hidden state from one step to the next. That state can summarize earlier observations, although training often becomes sequential and harder to parallelize.

A third route adds a dedicated memory module. This can support richer histories, retrieval, or multiple temporal scales. It also introduces another component whose data flow, optimization, and failure behavior require validation.

REKI chooses a narrower target. Its registers preserve enough recent context to improve local action consistency. The mechanism does not attempt to provide a semantic history of an entire task or workplace.

Training also avoids a sequential rollout through the memory window. Kyutai packs the three frames into one batch and processes them together. The action expert uses variable-length attention, while every frame contributes a training loss.

This arrangement preserves parallel training. The vision-language model performs no more work than it would for the same number of independent, memoryless frames. The action expert pays for the extra register tokens, but that component is much smaller.

During inference, past register keys and values stay in the cache. Current action tokens attend to those registers alongside the current observation. The model therefore receives temporal information through attention operations it already performs.

This is the mechanism behind Kyutai’s “negligible cost” description. The claim is not that memory has literally no cost. The registers consume GPU memory, attention capacity, and 0.1 milliseconds in the reported configuration.

The stronger point is proportional. That measured increment is less than one percent of the 21.8-millisecond action-chunk latency. It is also much smaller than the expense of repeatedly encoding several camera images through a 2.2-billion-parameter backbone.

The memory window remains intentionally short. Three frames sampled one second apart do not capture a long household task or an extended recovery sequence. The design remembers recent visual state, not every relevant event.

That boundary distinguishes REKI from systems targeting minutes of episodic context. It also limits the risks associated with stale information. A rolling cache discards old registers rather than allowing an unbounded history to accumulate.

For practical manipulation, a short memory can still address a common failure. A memoryless policy may alternate between two plausible poses because each current image triggers a different local decision. Recent observations give the action expert evidence about its prior direction.

Kyutai reports that such pose oscillations became roughly four times rarer with memory. That result connects the architecture to observable robot behavior. It is more informative than a benchmark average alone because it identifies one failure mode the registers address.

The design also creates a useful engineering question. Teams no longer need to decide only between stateless control and a large memory system. They can test whether compact, cached observation summaries solve the temporal failures that matter for their robots.

## Public Data Puts Larger VLA Models Under Pressure

**REKI’s strongest competitive argument combines efficiency with reproducibility, rather than claiming an absolute lead over every robot model.**

Kyutai says it pretrained REKI entirely on public sources. The mixture contains one billion frames spanning robot teleoperation, simulation, and egocentric human video. It also contains 12.2 million vision-language samples.

The robot data includes subsets from Open X-Embodiment, BridgeData V2, DROID, AgiBot World, RoboMIND, RoboCOIN, and other released collections. Simulation sources include InternData-A1 and GR00T X-Embodiment Sim.

Human-centered data contributes another perspective on manipulation. Kyutai used EgoDex, EgoVerse, and Xperience material showing hand interactions from an egocentric viewpoint. Vision-language datasets add spatial grounding and pointing examples.

The REKI paper says pretraining ran for 200,000 steps on 32 H100 GPUs. Kyutai retained each dataset’s native control convention instead of converting every robot into one universal action representation.

Public data does not mean effortless reproduction. The reported training run still requires considerable hardware, data processing, and engineering time. Dataset licenses and storage requirements also remain relevant.

However, the release changes what outside researchers can inspect. Kyutai published data-curation code, training logic, inference code, evaluation tools, deployment components, and model weights. The public REKI codebase provides a starting point for replication and modification.

That openness matters because many strong VLA systems publish checkpoints without disclosing their complete pretraining mixtures. Others rely on private robot trajectories or synthetic data pipelines that outsiders cannot recreate exactly.

REKI’s main opponent is therefore not one company. It is the larger, data-heavy VLA route that combines more parameters with less reproducible training inputs. Kyutai argues that architecture and data curation can recover much of that route’s performance.

Its reported benchmark results support part of that argument. REKI reaches 90.4 percent success on LIBERO-Plus and 65.8 percent on RoboTwin Clean2Rand. Kyutai reports that these are the highest scores among compared models pretrained on public data.

The comparison becomes more complicated when systems use proprietary or synthetic data. Qwen-RobotManip reaches 68.5 percent on Clean2Rand, ahead of REKI’s 65.8 percent. Its technical report describes a larger 4.7-billion-parameter model and a human-to-robot synthesis pipeline.

On RoboCasa365, REKI reports a 38.4 percent average success rate. Xiaomi-Robotics-1 reaches 57.4 percent, while ABot-M0.5 reaches 40.4 percent. Both contain about twice as many parameters as REKI.

These numbers support a performance-per-size claim more clearly than an overall performance crown. REKI leads some comparisons and trails larger systems in others. Its advantage depends on which benchmark, data category, and deployment constraint matters.

The RoboCasa365 benchmark also illustrates why one average requires caution. Its tasks include atomic behaviors and composite tasks, with separate seen and unseen conditions. A model can perform unevenly across those groups.

Kyutai’s paper reports that REKI scores 80.2 percent on RoboCasa365 atomic tasks and 57.1 percent on seen composite tasks. Its success rate falls to 32.1 percent on unseen composite tasks.

That final figure exposes the central limit of current VLA evaluations. Recognizing instructions and producing competent local actions does not guarantee reliable composition in unfamiliar situations. Compact memory alone does not close that gap.

REKI nevertheless raises the bar for reproducible baselines. Researchers can alter its registers, data mixture, or action expert without rebuilding an undocumented system. That can make follow-up experiments easier to interpret.

For teams conducting those experiments, maintaining a searchable engineering knowledge base can help connect configuration changes with observed failures. Reproducibility depends on recorded decisions as well as released source code.

## The Benchmark Gains Need a Careful Reading

**REKI’s results are promising, but they remain author-reported measurements shaped by benchmark protocols, compilation choices, and limited real-world tests.**

Memory improves REKI’s reported success rate by 1.5 percentage points on LIBERO-Plus. The gain reaches 5.5 points on RoboTwin clean scenes and 5.4 points on RoboCasa365.

Those improvements appear across three distinct benchmarks, which strengthens the case that memory contributes more than a dataset-specific trick. An evaluation ablation also removes historical frames while keeping the trained model unchanged.

When those past frames disappear, performance returns toward the memoryless baseline. That result suggests the model uses cached history during inference rather than receiving only a regularization benefit during training.

The oscillation analysis provides another useful check. Kyutai defines oscillation as movement between two poses across action chunks. Memory reportedly reduces the affected episodes from a substantially higher memoryless rate to roughly one quarter as many.

However, the measurements still come from REKI’s authors. Independent laboratories need to reproduce the ablations with different seeds, hardware, cameras, and control frequencies. Small implementation differences can affect robot benchmarks.

The latency comparison also needs context. Kyutai measured released models with LIBERO inputs on one H100. The team compiled several systems itself because their public implementations lacked equivalent optimized execution paths.

That effort makes the comparison more practical, but it does not create perfect equality. Models have different architectures, image processing requirements, action horizons, and compiler compatibility. A latency ranking can change under another accelerator or serving stack.

REKI reaches 21.8 milliseconds in its compiled configuration. Kyutai reports 25.5 milliseconds for X-VLA, 33.8 milliseconds for π0.5, 46.1 milliseconds for ABot-M0, and 51.3 milliseconds for InternVLA-A1.5.

Those figures measure action-chunk prediction, not complete robot reaction time. A production system also handles sensors, communication, collision checks, motion planning, and actuator commands. The slowest element can sit outside the neural model.

The real-robot evidence is narrower than the simulation evaluation. Kyutai shows autonomous rollouts using a UR5 arm and reports fine-tuning on 50 demonstrations per task. That indicates deployability, but it does not establish broad reliability across homes or factories.

The short memory window creates additional operational questions. A controller must reset cached registers when an episode ends, a camera changes, or a human interrupts the task. Otherwise, old observations could influence a new situation.

Robot state does not enter the registers themselves. That makes them reusable across denoising steps, which improves efficiency. It may also limit what they can represent about contacts, forces, or internal control transitions.

Visual history can show that an object moved, but it may not capture why a grasp failed. A tactile sensor or joint-torque signal could reveal information absent from the camera. Future variants will need to test richer inputs without losing the efficiency advantage.

Licensing is another constraint. REKI’s code uses the MIT License, but its weights use CC BY-NC-SA 4.0. Companies cannot assume that public availability permits unrestricted commercial integration.

Dataset provenance also deserves review. A mixture can be public while combining sources with different licenses, collection procedures, camera setups, and action semantics. Kyutai says it curated each source, but downstream teams remain responsible for compliance.

Finally, benchmark leadership ages quickly. Competitors can add similar cached registers, distill larger models, or improve compilers. REKI’s lasting contribution will depend less on one leaderboard position than on independent adoption of its memory design.

## What Comes Next for Kyutai REKI

**Three signals will determine whether REKI becomes a durable VLA baseline or remains an efficient research result.**

The first signal is independent reproduction. Outside groups need to rerun the memory ablations and benchmark evaluations using the released checkpoints. Matching the reported gains would strengthen the claim that register memory transfers across implementations.

Reproduction should include both success rates and failure categories. Researchers should count oscillations, repeated grasps, stalled transitions, and unsafe motions. An average score can conceal whether memory fixes one failure while worsening another.

Teams should also test several window lengths. REKI uses three frames sampled one second apart, with two historical frames retained. Longer windows would show how register memory scales before latency or accuracy begins to deteriorate.

If independent tests preserve the reported gains and low overhead, REKI’s architectural case becomes stronger. Large discrepancies would weaken conclusions drawn from the current benchmarks.

The second signal is deployment beyond H100-class infrastructure. The published 21.8-millisecond measurement uses a high-end data-center accelerator and a compiled execution path. Many practical robots operate with smaller edge devices or shared compute.

Quantized checkpoints, reduced image resolutions, and alternative accelerators will reveal whether the model’s efficiency survives realistic constraints. End-to-end tests should include camera acquisition, preprocessing, safety checks, and control communication.

A successful edge deployment would turn REKI’s compact size into an operational advantage. If memory remains cheap but the backbone dominates latency, attention will shift toward distillation and vision-encoder optimization.

The third signal is performance on longer, unfamiliar tasks. RoboCasa365’s unseen composite results show that difficult temporal composition remains unresolved. The model must retain relevant state across more than a few seconds without accumulating stale context.

Researchers can test hybrid systems that combine REKI’s local registers with a slower episodic memory. The fast cache could stabilize immediate movements, while a separate planner tracks completed subtasks and long-term goals.

That combination would preserve the distinction between two memory problems. A robot needs local continuity to avoid oscillation, and it needs task history to complete extended procedures. One mechanism does not necessarily solve both.

Competitors will provide another test. Qwen-RobotManip emphasizes scaling, alignment, and synthetic trajectories. Physical Intelligence’s π0.5 emphasizes broad generalization across real-world environments and heterogeneous training signals.

REKI takes a more constrained position. It proposes that efficient action-side memory and public-data pretraining can produce a small, inspectable policy with competitive results. That position is valuable even when larger models win individual benchmarks.

Developers should watch whether other VLA projects adopt register-based memory or publish comparable ablations. Broad architectural reuse would indicate that the technique solves a common problem rather than fitting one model.

Enterprise buyers should ask a different set of questions. Can the system clear memory safely, meet complete control-loop deadlines, and recover from interruptions? Can its weights be used under the intended commercial terms?

Researchers should examine whether the published data pipeline reproduces the reported checkpoints. They should also document preprocessing decisions, evaluation seeds, and compiler settings. Those details often determine whether an open release becomes a usable baseline.

The Kyutai REKI model does not settle the race for general-purpose robot intelligence. It narrows one important engineering dispute. Recent context can improve manipulation without forcing every historical frame through the most expensive part of the model.

The next step is verification outside Kyutai’s environment. If REKI’s memory gains survive independent robots, smaller hardware, and longer tasks, action-side caching will become difficult for larger VLA developers to ignore.
