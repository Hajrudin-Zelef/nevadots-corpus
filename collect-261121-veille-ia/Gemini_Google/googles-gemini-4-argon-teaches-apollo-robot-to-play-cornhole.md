---
id: collect-261121-veille-ia/gemini-google/googles-gemini-4-argon-teaches-apollo-robot-to-play-cornhole
title: "Google’s Gemini 4 Argon Teaches Apollo Robot to Play Cornhole"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["argon", "google", "robot", "apollo", "simulation", "demo", "cornhole", "gemini"]
source: https://datalign.kr/en/gemini-4-argon-apollo-cornhole-demo/
source_anchor: ""
source_lines: [1, 61]
section: "Gemini_Google"
---
## What Was Announced

Google has released results from an internal demo in which Apptronik’s humanoid robot Apollo made seven successful cornhole throws in a row (cornhole is an American lawn game where you toss beanbags at a hole in a board), powered by its newly unveiled multimodal model, Gemini 4 Argon. The demo was presented as an example of the robotics potential of the Argon model, which was made available to developers and businesses worldwide through the API on October 13.

According to Google, Argon took video of a person playing cornhole as input, worked out the spatial coordinates of the environment, calculated the joint movements for the robot’s whole body, and then automatically generated simulation code based on that. Google explains that the resulting output was passed to the physical hardware, Apollo, which carried out the demo in the real world.

**📑 Table of Contents**

## Technical Background: Video-to-Sim-to-Real

The core of this demo is a workflow Google calls “Video-to-Sim-to-Real.” Traditionally, teaching a robot a specific movement required people to model the simulation environment by hand, which is generally said to take several weeks of work. Google claims Argon compressed that process to just a few hours.

Here is how the workflow breaks down.

### Video Analysis Stage

Argon analyzes source video of a person performing the physical motion and extracts spatial coordinates and environmental information.

### Simulation Code Generation Stage

Based on the extracted information, it calculates the robot’s whole-body joint movements and automatically converts them into code that can run in a simulation.

### Real Hardware Transfer Stage

The motion logic validated in simulation is passed to the actual Apollo robot and reproduced as a physical throwing motion.

That said, it’s worth pointing out that what Google has shown is the result of an internal demo for a single throwing task, not a general metric of the robot’s overall capabilities.

## Benchmark Performance and Industry Use Cases

Google says Argon scored 87% on ERQA, an Embodied Reasoning benchmark that measures the ability to understand physical and spatial problems in 3D environments, and that it leads other top-tier models in pure reasoning ability. This benchmark measures reasoning about physical situations, so it is a separate evaluation from the real-world demo with Apollo.

For industry use, an integration with Roboflow was mentioned. According to a customer testimonial released by Google, Argon has been integrated into Roboflow for object detection and text extraction, and is being used for product inspection, inventory tracking, and machine guidance. The testimonial reports that Argon ranked first in the object detection evaluation and also led in text benchmarks using manufacturing, logistics, and aviation imagery.

## Practical Uses: What Developers and Companies Can Do

Where this announcement actually matters isn’t in the experience of individuals operating a humanoid themselves, but in its impact on the robot development workflow itself.

### Considering Shorter Simulation Prep Time

If you’re on a robotics development team, you could look into whether Argon’s Video-to-Sim-to-Real workflow can be applied to your own simulation modeling process. Whether Google’s claimed “weeks to hours” reduction holds up on real tasks needs to be verified on your own.

### Adopting Industrial Vision Inspection

If your company does work based on manufacturing, logistics, or aviation imagery, you could evaluate Argon’s object detection and text extraction features, integrated into Roboflow, for product inspection, inventory tracking, and machine guidance.

### Understanding API Access and Pricing

Argon is accessible through AI Studio and the paid Gemini API on the Gemini platform. It can handle up to 1 million tokens combined across input and output, and the introductory pricing is set at $2 per 1 million input tokens and $10 per 1 million output tokens. These prices are scheduled to rise to $4 and $20, respectively, after November 30, 2026, so developers planning long-term projects should factor in this cost change ahead of time.

## Open Questions and Limitations

This demo has several clear limitations. First, what has been shown is only an internal demo of a single task, cornhole, and the source material offers no evidence that it generalizes to a variety of physical tasks. Also, the ERQA score of 87% is a benchmark result measuring reasoning ability, not a direct indicator of the success rate or stability of the physical demo.

In addition, the announcement materials don’t reveal specific technical details on how much human intervention or additional calibration was needed in the simulation generation and hardware transfer process, so this needs to be confirmed further in the official documentation.

## Wrap-Up

Google has shown an internal demo in which Gemini 4 Argon analyzed video of a person, generated simulation code, and transferred it to the Apptronik Apollo robot, which landed seven cornhole throws in a row. The key point isn’t one robot’s throwing trick, but the claim that a workflow called Video-to-Sim-to-Real can sharply cut simulation prep time. However, this is an internal result for a single task, and its generalizability and implementation details still need further verification and confirmation from official sources.

## Leave a Reply
