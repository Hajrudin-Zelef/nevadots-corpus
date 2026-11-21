---
id: collect-261121-veille-ia/gemini-google/google-s-gemini-4-argon-scores-87-on-erqa-controls-spot
title: "Google's Gemini 4 Argon Scores 87% on ERQA, Controls Spot"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["argon", "google", "gemini", "robot", "policy", "video", "mujoco", "contact"]
source: https://www.aimastery.page/news/gemini-4-argon-robot-control
source_anchor: ""
source_lines: [1, 66]
section: "Gemini_Google"
---
Google released Gemini 4 Argon on October 13, 2026, positioning it as a general-purpose physical-reasoning model for robot control, Real2Sim scene construction, multimodal video labeling, and iterative policy improvement. For teams assembling separate vision-language-action models, simulators, and labeling pipelines, the immediate question is whether one frontier model with structured prompt harnesses can replace three or four services.

## Capabilities across the robotics lifecycle

In Google's developer blog, principal software engineer Kanishka Rao details four functional areas. In robot control, Argon directs a Boston Dynamics Spot quadruped—configured as 12-DoF legs plus a 6-DoF arm—through a foot-pedal trash-bin task. It first constructs an articulated MuJoCo digital twin of the bin for practice, then runs on hardware with a two-agent verification swarm: a foot-verifier and hand-verifier confirm the foot-pedal step and 3D hand-in-bin alignment before releasing the cans.

A second demo uses a visual coordinate harness with a 2 cm grid projected onto a wrist camera feed, issuing `curl` commands against a local HTTP endpoint and selecting primitives such as `robot.grasp`, `robot.place_until_contact`, and `robot.move_until_contact`. A third closes a visual loop around the Gemini Omni video world model, producing a 33-step rollout that rearranges five Scrabble tiles into A-R-G-O-N.

In Real2Sim, Argon reconstructs MuJoCo scenes from robot teleoperation video. Offline trajectory changes are limited to within 0.55 rad of logged arm-joint positions and 0.65 rad of logged hand-joint positions. After t=0, external forces, welds, and equality constraints are not allowed; objects move only through MuJoCo contact. A separate demo retargets egocentric human hand video onto a 76-DoF bimanual humanoid with 22-DoF five-fingered hands, running at 500 Hz (dt = 0.002 s) without kinematic weld constraints.

For policy improvement, Google reports synthesizing domain-randomized MuJoCo episodes from a single RGB photo to fine-tune a vision-language-action policy, citing a success-rate rise from 56% to 75% on the corresponding real setup.

## Benchmark performance and availability

Google reports Argon scores 87% on the Embodied Reasoning (ERQA) benchmark using pure reasoning without auxiliary tools, and says that result leads all other frontier models on the test. The score and the claim that it leads are Google-reported and have not been independently verified. Argon is available to Google AI Ultra subscribers and through the paid Gemini API as `gemini-4-argon`. Google publishes no per-demonstration failure rates beyond the VLA fine-tuning case.

## Competitive positioning

| System | Primary design point | Simulation tooling | Control loop style | 
|---|---|---|---|
| Gemini 4 Argon | General-purpose physical reasoning across the robotics lifecycle | MuJoCo twin construction and calibration from video or sketches | Prompt harness with HTTP motion primitives and visual verifiers | 
| OpenVLA-class policies | Action prediction under specific robot embodiments | Separate simulator tooling | End-to-end VLA inference from observations | 
| Physical Intelligence π-family | Generalist robot policy from large teleoperation corpora | Not a primary focus | Learned policy, not prompt-driven | 
| NVIDIA GR00T + Isaac Sim | Humanoid embodiment with Isaac-oriented tooling | Isaac Sim digital twins | Separate policy and simulation layers | 
| Gemini Robotics-ER | Spatial perception and planning | Not specified | Spatial reasoning focus | 

As InfoQ reported, early practitioner reaction focused on simulation fidelity. Developer Davin YC Dong commented on X that a scene can look convincing while its objects behave incorrectly, and that for manipulation matching contact events matters as much as matching pixels. That matters because Argon's Real2Sim prompt requires simulated fingertip contact force and torque to match logged tactile readings in timing, magnitude, and direction—enforced through iterative reasoning, not a physics solver tuned by domain experts.

## AI Mastery analysis

The deepest question is whether prompt-harness scaffolding can be a production architecture. The demos depend on long task-specific prompts, coordinate grids, multi-agent verifiers, and timestamped action sequences. That is closer to pipeline architecture than a monolithic learned policy, and it carries the same risks: prompt brittleness under distribution shift, latency from closed-loop VLM calls, and missing failure rates outside the single VLA case.

The Real2Sim constraint set—joint deviation limits, no post-initialization model changes, contact forces sourced only from MuJoCo—is the most replicable specification in the release. Teams evaluating Argon against dedicated VLA pipelines should treat the 500 Hz retargeting demo as a benchmark for their own scene complexity, since the egocentric-video workflow assumes triangulatable camera intrinsics and convex-decomposable object geometry that may not hold in cluttered production environments. The architectural specificity behind these gains lies in the constraint system around the simulator rather than the model weights alone.

Argon suggests Google is treating the robotics stack as a prompt-engineering and scaffolding problem as much as a model-weight problem. If the ERQA score and the VLA fine-tuning result hold under third-party replication, teams maintaining separate labeling, simulation, and improvement services will face real pressure—but the complexity shifts to prompt engineering, a tradeoff production teams will need to price.

## Sources

- 
Primary: [Gemini 4 Argon for Robotics
- 
Google Developers Blog](https://developers.googleblog.com/gemini-4-argon-for-robotics/)
- 
Reported by: InfoQ AI: Google Releases Gemini 4 Argon for Robotics Control, Simulation, and Policy Improvement

## Frequently asked questions

## What is Gemini 4 Argon?

Gemini 4 Argon is Google's general-purpose physical-reasoning model, released October 13, 2026. It targets robot control, Real2Sim scene reconstruction, multimodal video labeling, and iterative policy improvement rather than a single end-effector policy.

## What score does Gemini 4 Argon achieve on ERQA?

Google reports Argon scores 87% on the Embodied Reasoning (ERQA) benchmark using pure reasoning without auxiliary tools. Google says that result outperforms every other frontier model on the test, but the claim has not been independently verified.

## How does Gemini 4 Argon control real robots?

In Google's demos, Argon controls a Boston Dynamics Spot with 12-DoF legs and a 6-DoF arm using a MuJoCo digital twin for practice and a two-agent foot-and-hand verification swarm on hardware. It also uses a calibrated wrist camera with a projected 2 cm world-coordinate grid and local HTTP motion primitives.

## What Real2Sim limits does Gemini 4 Argon enforce?

Offline trajectory changes must stay within 0.55 rad of logged arm joints and 0.65 rad of logged hand joints. After t=0, no external forces, welds, or equality constraints are allowed, and objects move only through MuJoCo contact.

## What success gain did Google report for policy improvement?

Google reports a rise from 56% to 75% success on the corresponding real-world setup after fine-tuning a vision-language-action policy on domain-randomized MuJoCo episodes synthesized from a single RGB photo. This is the only quantitative per-task control result Google publishes.
