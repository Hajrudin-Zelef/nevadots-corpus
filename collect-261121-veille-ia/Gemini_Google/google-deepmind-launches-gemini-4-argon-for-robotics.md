---
id: collect-261121-veille-ia/gemini-google/google-deepmind-launches-gemini-4-argon-for-robotics
title: "Google DeepMind Launches Gemini 4 Argon for Robotics"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["argon", "google", "deepmind", "robot", "gemini", "simulation", "data", "reasoning"]
source: https://www.brocker.org/google-deepmind-gemini-4-argon-robotics
source_anchor: ""
source_lines: [1, 29]
section: "Gemini_Google"
---
On 13 October 2026, Google DeepMind introduced Gemini 4 Argon, a model built for physical reasoning and robot control, in a post on its developer blog. The company says Argon scores 87% on the Embodied Reasoning Question Answering (ERQA) benchmark using pure reasoning, which it says outperforms every other frontier model it tested.

Argon is positioned as a general-purpose physical reasoning model rather than a narrow robot-control script. Google DeepMind frames it as applicable across what it calls the entire robotics lifecycle: controlling robots directly, building simulated twins of real environments (Real2Sim), labeling and evaluating robot behavior after the fact, and generating new training data to improve other models.

The blog post, written by principal software engineer Kanishka Rao, walks through examples rather than a single flagship demo. In one, Argon directs a Boston Dynamics Spot quadruped to pick up soda cans and step on a foot-pedal trash bin, first rehearsing the motion in a MuJoCo physics simulation before attempting it on the physical robot. In another, Argon controls a simple robotic arm through a visual coordinate system to right a fallen mug and tuck a napkin inside it, issuing commands turn by turn and checking a wrist camera to confirm progress.

Several examples center on Real2Sim, the process of turning real-world footage into a working simulation. Google DeepMind describes Argon building a MuJoCo digital twin from a recorded robot teleoperation episode, matching both the simulated camera views and the contact forces on the robot's fingertips to the logged real-world data. In a separate example, Argon retargets an egocentric video of a human performing a tabletop task onto a simulated 76-degree-of-freedom bimanual humanoid with 22-degree-of-freedom hands. The company also shows Argon converting a hand-drawn pencil sketch of a humanoid throwing a ball at a stack of boxes into a physics-verified simulation with a synthesized throwing motion.

On the data and evaluation side, Google DeepMind says Argon can review footage of robot behavior and produce multi-level annotations: an overall episode goal, semantic subtasks, fine-grained contact-level actions, and automated quality-control flags that catch stalled or fumbled attempts. In one demonstration drawn from a classic 12-ball balance-puzzle setup, Argon both builds the MuJoCo test environment and writes a vision-only solver that identifies an odd-weighted ball using a simulated balance scale.

The company also describes using Argon to generate training data for other robot-control models. Starting from a single overhead photo of a tabletop, Argon built a calibrated simulation, defined a curriculum of 77 pick-and-place and manipulation tasks, and generated more than 4,500 randomized episodes. Google DeepMind says that data raised a real-world robot's success rate on an evaluation benchmark from 56% to 75%, while cutting completion time from 35 seconds to 24 seconds.

## Confirmed

- Gemini 4 Argon scores 87% on the ERQA benchmark (N=400, tool off) using pure reasoning, according to Google DeepMind; the company's chart lists Claude Opus 5.5 at 82.3%, GPT-6 Astra at 81.0%, and Claude Fable 5.1 at 74.0%.
- Google DeepMind demonstrates Argon controlling a Boston Dynamics Spot quadruped, a simple 6-DoF robotic arm, and bimanual Franka FR3 arms, including simulation rehearsal via MuJoCo before physical execution.
- Argon can reconstruct MuJoCo simulation twins from robot camera feeds, egocentric human video, and hand-drawn sketches, according to the company.
- Google DeepMind says using Argon-generated simulation data raised a robot's real-world task success rate from 56% to 75% and cut completion time from 35 seconds to 24 seconds on its evaluation benchmark.
- Gemini 4 Argon (model ID gemini-4-argon) is available to Google AI Ultra subscribers in Google AI Studio and Google Antigravity, and through the paid tier of the Gemini API via the Interactions API with day-one support in the google-genai SDKs.

## Unknown

- The ERQA benchmark comparison is Google DeepMind's own result; independent replication of the 87% score against Claude Opus 5.5, GPT-6 Astra, and Claude Fable 5.1 is not available.
- Pricing for Argon access through the paid Gemini API tier is not stated in the announcement.
- The source does not specify which physical robot hardware, beyond the examples shown (Spot, Franka FR3, SO-101, a 6-DoF arm), is officially supported for production deployment outside demonstration settings.

## Our take

Google DeepMind is pitching Argon less as a single robot-control model and more as a general reasoning layer that spans simulation building, data labeling, and training-data generation. That is a broader claim than a typical benchmark launch. The 56%-to-75% success-rate jump is the number to watch, but it is a vendor-reported result from one evaluation benchmark and one setup, and the post does not compare the cost of simulated data with real-world data collection.
