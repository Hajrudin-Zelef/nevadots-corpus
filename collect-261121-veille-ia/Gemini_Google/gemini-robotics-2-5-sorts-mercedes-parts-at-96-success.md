---
id: collect-261121-veille-ia/gemini-google/gemini-robotics-2-5-sorts-mercedes-parts-at-96-success
title: "Gemini Robotics 2.5 Sorts Mercedes Parts at 96% Success"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-10"]
keywords: ["gemini", "robotics", "success", "model", "deepmind", "parts", "lines", "task"]
source: https://aiincontext.com/articles/apollo-2-humanoids-sort-mercedes-parts-at-96-in-deepmind-test
source_anchor: ""
source_lines: [1, 66]
section: "Gemini_Google"
---
## Why it matters

- Gemini Robotics 2.5, announced November 10, 2026, achieved a 96% success rate sorting parts on active Mercedes-Benz lines in Kecskemét, Hungary, and Sindelfingen, Germany.
- The model ran on Apptronik's Apollo 2 humanoid with a 22-degree-of-freedom SharpaWave hand, evaluated across 200+ tasks with 30-100% higher success and 20-30% faster completion.
- Safe task completion improved 33% around obstacles and 43% around nearby people in DeepMind's evaluations.
- DeepMind open-sourced the ASIMOV-Jailbreak benchmark; Gemini 4 Argon scored 99.5% jailbreak resistance versus 82% for the prior baseline.
- A separate October 13, 2026 simulation experiment lifted real-robot success from 56% to 75% using 4,500+ MuJoCo training episodes generated from one photo.

Google DeepMind's Gemini Robotics 2.5 model sorted automotive parts with a 96% success rate in a proof-of-concept that ran Apptronik's Apollo 2 humanoid robots on active Mercedes-Benz production lines. The company announced the model on November 10, 2026, and is now rolling it out to robotics partners.

The stakes are straightforward. Automotive kitting — arranging parts in the exact sequence each vehicle requires — is repetitive, labor-intensive work that keeps assembly lines fed. If a general-purpose model can do it reliably on live lines, manufacturers gain a credible alternative to dedicated fixed automation for tasks that change with every build.

## Where did the robots actually work?

The trial took place at Mercedes-Benz plants in Kecskemét, Hungary, and Sindelfingen, Germany. DeepMind, Apptronik and the automaker ran Apollo 2 on active production lines, not a lab mock-up. The humanoid autonomously sorted metal automotive brackets from bulk supply bins into sequencing trays arranged in the precise order each vehicle needed, and corrected gaps in the containers it had filled.

Two models divided the workflow. Gemini Robotics ER tracked task progress, while Gemini Robotics 2.5 — a vision-language-action model, meaning one that connects visual and language understanding directly to robot actions — controlled the humanoid's hands and arms. The tested Apollo 2 configuration used a five-finger SharpaWave hand with 22 degrees of freedom (the number of independent joints controlling its movement).

DeepMind frames the value as offloading: robots handle repetitive preparation so assembly lines keep moving while technicians focus on complex build tasks. The company's model page describes control spanning tabletop arms to complete humanoid bodies, including dynamic balance for stepping, squatting and bending, and it offers an early-access waitlist.

Apptronik co-founder and CEO Jeff Cardenas said: "Pairing Apollo 2 with Gemini Robotics, we saw the models improve dramatically in just a few weeks of post-training and carry over to Mercedes-Benz production with very little drop-off."

That "very little drop-off" is the claim worth watching. Simulated and lab results routinely degrade on real factory floors; DeepMind reports the transfer held here, but the company has not published per-site breakdowns.

## How much better is the dexterity?

Beyond the factory trial, DeepMind evaluated the model on more than 200 distinct tasks, including serving food with tongs, tying a gift-box bow and pipetting liquid. The suite combined tool use, manipulation of deformable objects and handling of delicate items.

The reported gains are material:

- 30% to 100% higher success rates than previous baselines, depending on task type
- 20% to 30% faster task completion
- 33% improvement in safe task completion around obstacles
- 43% improvement in safe task completion around nearby people

For physical safety, the model uses inference-time reasoning guidance — reasoning applied while the robot acts — to steer around hazards in real time. The release also targets long-horizon agentic tasks, where safety must hold across multiple sequential steps rather than a single motion.

In plain terms: the model is measurably better at both fine manipulation and not hurting things around it, but DeepMind's own numbers are the only numbers on the table so far.

## What is ASIMOV-Jailbreak?

Alongside the robot release, DeepMind published ASIMOV-Jailbreak, an open-source benchmark for testing whether robots can be manipulated into unsafe actions. Its prompts disguise unsafe requests across dozens of scenes.

Gemini 4 Argon achieved 99.5% jailbreak resistance on the benchmark, compared with 82% for the previous Gemini Robotics ER 2 baseline. The repository ships evaluation code and data under an MIT license.

The benchmark's multi-turn V3 adversarial evaluation contains 845 samples spanning 15 strategy groups, with attacks permitted up to 10 steps. It pairs deterministic checks of action tool calls with a large language model acting as judge. Crucially, it pairs harmful and benign tabletop-manipulation tasks, so the harness measures attack success alongside benign over-refusal — testing whether a model refuses genuinely unsafe instructions without also rejecting safe work.

That dual measurement matters. A robot that refuses everything is safe but useless; the benchmark attempts to distinguish justified refusals from over-caution.

## Where did the training data come from?

DeepMind credits Gemini 4 Argon, its frontier model, with accelerating Gemini Robotics 2.5 development through simulation. Argon can generate thousands of varied practice runs from a single real-world camera scene and build a realistic digital twin. Combined with real training demonstrations, those runs help robots generalize faster to new scenes and objects. Researchers can also use Argon to propose explanations for task failures from video, then test improvements in simulation and on physical robots.

A developer post dated October 13, 2026 documents that loop in a separate experiment. It began with one color photograph of a real tabletop. Argon reconstructed it as a calibrated MuJoCo physics simulation (an open-source physics engine used in robotics research), then produced more than 4,500 training episodes with randomized conditions. Google reported the resulting real-robot success rate rose from 56% to 75%, while completion time fell from 35 seconds to 24 seconds.

DeepMind is careful to note that this earlier experiment is not a Gemini Robotics 2.5 benchmark. It illustrates the development loop behind the release rather than the shipping model's headline numbers.

## What does this mean for industrial robotics?

The announcement's real significance is integration. It connects simulated data generation, hardware control and adversarial safety testing in a single workflow: one model family generates training data, another controls the robot, and an open benchmark stress-tests whether the system can be pushed into unsafe behavior.

The 96% figure comes from one demonstrated task — kitting brackets — at two plants, under conditions the three companies controlled jointly. The open question, which DeepMind itself flags, is how consistently the workflow transfers beyond that task to other robots and other workplaces. A 96% sorting rate on one part family is not yet a general manufacturing capability, and no third-party evaluation accompanies the release.

What the announcement does establish is that a frontier AI lab, a humanoid hardware maker and a major automaker can run a general-purpose VLA model on live production lines with results they describe as near-reliable. The early-access waitlist will determine how quickly anyone outside that trio can check the claim.

Source: unite.ai
