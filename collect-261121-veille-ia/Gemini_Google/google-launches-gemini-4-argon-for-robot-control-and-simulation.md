---
id: collect-261121-veille-ia/gemini-google/google-launches-gemini-4-argon-for-robot-control-and-simulation
title: "Google launches Gemini 4 Argon for robot control and simulation"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "argon", "simulation", "robot", "gemini", "model", "control", "single"]
source: https://oossa.com/en/google-launches-gemini-4-argon-for-robot-control-and-simulation-3782066
source_anchor: ""
source_lines: [1, 19]
section: "Gemini_Google"
---
Google has introduced Gemini 4 Argon, a model designed for physical‑reasoning tasks in robotics. The model can direct a Boston Dynamics Spot robot through a foot‑pedal exercise after practicing in a MuJoCo simulation twin. It can also steer a simple robotic arm using visual coordinates and HTTP motion commands, and it works with Gemini Omni’s video world model to close the perception‑action loop.

Argon can rebuild MuJoCo simulation scenes from teleoperation video (a process Google calls Real2Sim) and can label demonstration footage for behavior‑cloning. Google says it can synthesize domain‑randomized MuJoCo episodes from a single RGB photo and use those to fine‑tune a vision‑language‑action policy, raising success on the real robot from 56% to 75%.

## Performance and access

Google reports that Argon achieved 87% accuracy on the Embodied Reasoning Question Answering (ERQA) benchmark using pure reasoning, without any external tools. The company says this score leads other frontier models on the same test. Argon is offered to Google AI Ultra subscribers and can be called through the paid Gemini API under the model ID gemini-4-argon.

The robotics demos rely on long, task‑specific prompts that include coordinate grids, motion primitives, and verification steps. Google has not published detailed failure rates for each demo, only noting the single video‑world‑model case.

## Early reactions

Practitioners have pointed out that simulation fidelity matters. Davin YC Dong, commenting on X, warned that a scene can look realistic while its physical interactions are wrong, which is crucial for manipulation tasks. Another developer, pulpmatrix, highlighted the need for clear source documentation around claims of “Video‑to‑Sim‑to‑Real” pipelines, noting that the current release lacks a paper, code repo, or hit‑rate statistics for the cornhole example.

Specialized systems such as OpenVLA, Physical Intelligence’s π‑family, and NVIDIA’s GR00T focus on action prediction for specific robot embodiments and typically rely on large teleoperation datasets. Google’s approach bundles perception, simulation, and control into a single model with heavy prompting, raising questions about whether it can replace a multi‑service pipeline.

## Why it matters

For robotics developers, Argon offers a single model that can turn video data into simulated training environments and improve robot policies, potentially lowering the cost of building separate perception, simulation, and control pipelines. However, the lack of detailed failure metrics and open source resources means users will need to verify performance for their own hardware before relying on it for critical tasks.
