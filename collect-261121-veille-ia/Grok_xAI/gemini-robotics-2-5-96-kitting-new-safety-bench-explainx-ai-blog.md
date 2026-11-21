---
id: collect-261121-veille-ia/grok-xai/gemini-robotics-2-5-96-kitting-new-safety-bench-explainx-ai-blog
title: "Gemini Robotics 2.5: 96% Kitting, New Safety Bench | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["robotics", "gemini", "deepmind", "kitting", "same", "success", "apollo", "safety"]
source: https://explainx.ai/blog/gemini-robotics-2-5-mercedes-kitting-asimov-jailbreak-2026
source_anchor: ""
source_lines: [1, 156]
section: "Grok_xAI"
---
**Google DeepMind released Gemini Robotics 2.5 on November 10, 2026**, calling it the lab's most capable vision-language-action model to date. The post ties together three claims builders care about: **wider everyday dexterity**, a **96% success Mercedes-Benz kitting pilot** on Apptronik's Apollo 2 humanoid, and **safety tooling** that includes the new open **ASIMOV-Jailbreak** benchmark for adversarial robotics prompts described on the DeepMind evals page.

The release is a generational bump on July's Gemini Robotics 2 stack, not a rename. DeepMind still positions an **actions model** (here Robotics 2.5) alongside **embodied reasoning** (ER) for multi-step workflow tracking — the Mercedes demo explicitly pairs **Gemini Robotics ER** for workflow state with **Robotics 2.5** for hand and arm control.

## TL;DR

| Question | Answer | Source | 
|---|---|---|
| What shipped? | Gemini Robotics 2.5 VLA; partner rollout | DeepMind blog | 
| Headline factory result | 96% success automotive kitting pilot | Same; Apptronik + Mercedes-Benz | 
| Skill breadth | 200+ distinct evaluated tasks | Same | 
| Vs prior model | 30–100% higher success; 20–30% faster | Same | 
| Safety steerability | +33% safe completion around obstacles; +43% near people | Same | 
| Jailbreak resistance | 99.5% on ASIMOV-Jailbreak (Argon) vs ~82% ER 2 baseline | Same | 
| Training accelerant | Gemini 4 Argon simulation and failure analysis | Same | 

## Dexterity: 200 tasks and the benchmark shift

DeepMind's central pitch is that **unstructured rooms** need robots that combine tool use, deformable-object manipulation, and delicate forces — not only pick-place on rigid boxes.

The November post evaluates **more than 200 distinct tasks**, with public examples including **serving food with tongs**, **tying a bow on a gift box**, and **pipetting liquid**. Those are intentionally multi-skill compositions: contact-rich physics plus language-conditioned goals.

Compared with earlier Gemini Robotics baselines, DeepMind reports **30% to 100% higher success** depending on category, and **20% to 30% faster** execution on the same suite. The lab published comparative bar charts for tabletop pick-and-place and for dexterous sets (tongs, ribbons, garments).

Gemini Robotics 2.5 benchmark chart comparing pick-and-place success rates against Gemini Robotics 2
*Image: Google DeepMind*

Hardware coverage stays **embodiment-agnostic** in messaging: **Apptronik Apollo 2** with SharpaWave's **22-DoF hand**, plus **Franka Duo** parallel grippers, match the multi-robot story from the Gemini 4 Argon robotics angle and prior Apollo demos.

### What this is not

The bar charts are **lab-reported** on a defined task battery. They do not replace **shift-long MTBF**, **changeover time when SKUs rotate**, or **union and safety sign-off** on a line. Treat the 200-task suite as **capability telemetry**, not a warehouse SLA.

## Mercedes-Benz kitting: what 96% actually means

Apollo 2 humanoid robots on a Mercedes-Benz kitting line
*Image: Google DeepMind*

The factory story is the credibility anchor. DeepMind, **Apptronik**, and **Mercedes-Benz** ran a **proof-of-concept** on **active automotive production lines** in **Kecskemét, Hungary** and **Sindelfingen, Germany**.

The task is **kitting**: repetitive sorting of metal brackets from bulk bins into **sequencing trays** in vehicle order. Apollo 2 reaches into supply bins, grasps brackets, and places them while correcting gaps — work DeepMind describes as monotonous prep that frees technicians for complex assembly.

Control stack in the post:

- **Gemini Robotics ER** tracks workflow and multi-step state.
- **Gemini Robotics 2.5** outputs hand and arm actions.

DeepMind quotes Apptronik CEO **Jeff Cardenas** saying Apollo paired with Gemini Robotics improved dramatically after **weeks of post-training** with **little drop-off** when moving to Mercedes lines.

The cited **96% success rate** applies to that pilot configuration. It is stronger evidence than a teleop reel, but still short of a multi-year **OEE** contract metric. Readers evaluating humanoids for plants should compare against embodied-agent correction research and their own **cycle-time distributions**, not a single press number.

## Safety: steerability and ASIMOV-Jailbreak

Physical AI safety is more than "stop if human nearby." DeepMind highlights two layers in the 2.5 post.

**Inference-time reasoning guidance** steers around hazards during execution. The blog quantifies **33% higher safe completion around obstacles** and **43% around people** versus prior behavior on their evaluations.

For **long-horizon agentic** commands, DeepMind leans on **Gemini 4 Argon**, which the post says leads internal agentic robotics evals on **unsafe task refusal**, **uncertainty resolution**, and **human proximity**.

The newly released **ASIMOV-Jailbreak** benchmark packages **hundreds of prompts** that disguise unsafe requests across **dozens of scenes**. DeepMind reports **99.5% jailbreak resistance** for Argon versus about **82%** for a **Gemini Robotics ER 2** baseline.

Open-sourcing the jailbreak set matters for builders: you can regression-test orchestration layers before tying them to torque. Pair that mindset with agentic robotics safety sessions and your own **estop and workspace interlocks** — benchmarks do not replace hardware limits.

Safety guardrail metaphor for Gemini Robotics steerability and human-aware motion planning

## Gemini 4 Argon as a simulation factory

Training data bottlenecks show up in every robotics foundation-model launch. DeepMind's answer here is **Gemini 4 Argon** acting as a **simulation engine**:

- Generate **thousands of varied practice runs** from a real camera scene.
- Build a **digital twin** for synthetic diversity atop human demonstrations.
- **Hypothesize failure causes** from video and test mitigations in sim and on hardware.

That loop is how DeepMind explains **post-training in weeks** for Apollo without claiming full zero-shot transfer to every Mercedes configuration.

If you run models locally for sim, the same pattern shows up in Cosmos-style world models and in Google's own ERQA-style eval framing: **synthetic diversity plus a small real anchor set**.

## Access and what partners should ask

The November 10 post says **Gemini Robotics 2.5 is rolling out to robotics partners**. There is no Hugging Face card, no Vertex SKU table, and no public API doc in the announcement itself.

Questions to bring to a partner application or enterprise thread:

| Topic | Why it matters | 
|---|---|
| **Latency budget** per arm command | Kitting success rates collapse if inference spans whole seconds per grasp | 
| **ER vs VLA versioning** | Mercedes demo uses both; pin versions in safety files | 
| **Failure taxonomy** | Ask for confusion matrices beyond success percentage | 
| **Jailbreak eval on your verbs** | Run ASIMOV-Jailbreak plus in-house verbs tied to your tools | 
| **Maintenance window** | Who owns post-training when line layouts change | 

For software-only teams, the near-term takeaway is architectural: **factory-ready humanoids need a reasoning layer and an action layer**, plus **sim-generated data**. That matches how Physical Intelligence's API beta and NVIDIA GR00T narratives frame the same split.

## Task examples DeepMind highlighted

The November blog is unusually concrete about **which skills moved**. Grouping them helps procurement teams map demos to their own cells:

| Category | Example tasks named | Why they are hard | 
|---|---|---|
| **Tool + deformable** | Serving food with **tongs** | Grasp compliance + slip | 
| **Fine manipulation** | **Tying a bow** on a gift box | Sequential contact with ribbon | 
| **Lab technique** | **Pipetting liquid** | Volume precision + breakage risk | 
| **Garment handling** | Hanging cardigans, folding pants | Flexible materials sag unpredictably | 
| **Tabletop rigid** | Pick-and-place with higher success bars | Baselines already strong; gains are incremental | 

DeepMind's published charts show some tasks jumping from **0% on Robotics 2** to **double-digit or 100% success on 2.5**. Treat zero-to-nonzero jumps as **small-sample sensitivity** unless trial counts are published.

### Embodiments named in the release

- **Apptronik Apollo 2** with**SharpaWave** five-finger, 22-DoF hand.
- **Franka Duo** parallel grippers for dual-arm setups.

That list matters because **dexterity claims attach to specific end effectors**. A five-finger success curve does not automatically transfer to a two-finger cell without post-training.

### Training timeline language

Mercedes/Apptronik quotes emphasize **weeks of post-training** before line transfer—not zero-shot magic. Budget engineering time when you replicate the stack:

1. Capture line-camera scenes.
2. Run Argon-augmented sim batches.
3. Validate on pilot SKUs before mixing with human-paced assembly.

## How Gemini Robotics 2.5 fits the 2026 robot-model map

Gemini Robotics 2.5 does not exist in isolation. November's frontier stack also includes open tactile models such as LINGbot-Touch, generalist humanoid claims from Figure Helix lines, and cloud-heavy orchestration in AWS Bedrock AgentCore.

DeepMind's differentiation is **Google-scale sim plus Argon** feeding a **partner-gated VLA** with **automotive OEM proof**. It is less "download weights tonight" and more "proof that language-conditioned kitting can survive real bins."

### Practical next steps for readers

1. **Read the July 2.0 post first** if you are new to the ER / VLA / On-Device trio — Gemini Robotics 2 guide.
2. **Clone ASIMOV-Jailbreak** and wire it into any agent that emits robot goals from an LLM.
3. **Track SharpaWave and Apollo 2** hardware notes if you evaluate five-finger humanoids versus gripper cells.
4. **Separate sim success from line success** — demand time-stamped logs, not aggregated percentages.

## What developers are asking

Although this launch did not ship with a major Hacker News front-page thread at publish time, robotics builders repeating questions from the July discussion still apply:

- **"Is ER 2.5 a separate SKU?"** — Treat ER and VLA as paired products until DeepMind publishes a single price list.
- **"Does 96% kitting generalize to my SKU?"** — Unlikely without post-training; the post describes weeks of adaptation.
- **"Can we fine-tune on our own sim?"** — Argon-assisted sim is described, not open-sourced in this note.

## Related on explainx.ai

## FAQ for robotics ML engineers

**Should I fine-tune Robotics 2.5 or ER?** The Mercedes workflow uses **both**: ER for process memory, 2.5 for low-level control. If you only fine-tune one, match the failure mode you see (sequence errors vs grasp slips).

**Is ASIMOV-Jailbreak only for Gemini?** The benchmark is positioned as **open** for the ecosystem; you can run it against any orchestrator that emits natural-language robot goals—even if actions come from a non-Google VLA.

**Where do sim assets live?** This announcement describes Argon-generated twins but does not open-source the full sim pipeline. Plan internal sim investment unless Google releases tooling later.

**Does 96% kitting imply humanoid capex wins?** Not alone. Lines still need **downtime tolerance**, **spare hands**, and **regulatory sign-off**. Use the pilot as evidence that **language-conditioned kitting is no longer purely speculative**, not as justification for fleet-wide humanoid swaps.

*Specifications and partner access reflect Google's November 10, 2026 announcement.*
