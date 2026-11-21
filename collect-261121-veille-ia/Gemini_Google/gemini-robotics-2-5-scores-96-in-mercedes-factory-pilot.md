---
id: collect-261121-veille-ia/gemini-google/gemini-robotics-2-5-scores-96-in-mercedes-factory-pilot
title: "Gemini Robotics 2.5 Scores 96% in Mercedes Factory Pilot"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "robotics", "percent", "google", "model", "deepmind", "robot", "pilot"]
source: https://aiincontext.com/articles/google-deepmind-s-gemini-robotics-2-5-hits-96-in-mercedes-factory-pilot
source_anchor: ""
source_lines: [1, 51]
section: "Gemini_Google"
---
## Why it matters

- Apollo 2, running Gemini Robotics 2.5, sorted metal parts on active Mercedes-Benz lines in Hungary and Germany at a reported 96 percent success rate.
- Google DeepMind says Gemini Robotics 2.5 beats its previous baselines by 30 to 100 percent and completes tasks 20 to 30 percent faster across more than 200 tasks.
- Safe task completion improved 33 percent near obstacles and 43 percent near people in the company's own evaluations.
- Google DeepMind released the open-source ASIMOV-Jailbreak benchmark; Gemini 4 Argon scores 99.5 percent jailbreak resistance versus 82 percent for Gemini Robotics ER 2.
- The model is rolling out to partners Agile, Apptronik, and Mercedes-Benz.

Google DeepMind reports a 96 percent success rate for a humanoid robot sorting metal parts on active Mercedes-Benz production lines, the headline result for its newly introduced robot-control model, Gemini Robotics 2.5. The pilot ran in Kecskemét, Hungary, and Sindelfingen, Germany, with Apptronik's Apollo 2 robot performing kitting work directly alongside vehicle assembly.

The announcement signals a shift in robotics AI from laboratory demos to deployed industrial trials, and it bundles three distinct releases: the Gemini Robotics 2.5 model, an open-source safety benchmark called ASIMOV-Jailbreak, and a simulation pipeline built on Google's Gemini 4 Argon language model.

## What is Gemini Robotics 2.5?

Gemini Robotics 2.5 is a vision-language-action model, or VLA. It processes visual information, understands language instructions, and translates both into robot movements. The model builds on its predecessor, introduced earlier this year, which Google DeepMind designed to give robots full-body control for the first time.

According to Google DeepMind, the company tested the model on more than 200 tasks. The list includes serving food with tongs and pipetting liquids. Measured against Google DeepMind's own previous baselines, the model achieves a 30 to 100 percent higher success rate depending on the task type. It also completes tasks 20 to 30 percent faster.

The model is designed to work across different robot platforms. Google DeepMind names two: Apptronik's humanoid robot Apollo 2, which carries a five-fingered hand with 22 degrees of freedom, and the Franka Duo platform, which uses two-finger parallel grippers. That hardware range matters for adoption: a single control model that transfers between a dexterous humanoid hand and an industrial parallel gripper reduces the integration cost of putting AI-driven robots on factory floors.

## What happened on the Mercedes-Benz production lines?

Together with Apptronik and Mercedes-Benz, Google DeepMind tested the model on active production lines at two plants, in Kecskemét, Hungary, and Sindelfingen, Germany. Apollo 2 handled kitting: the robot sorted metal parts from bulk bins and placed them into trays in the order needed for each vehicle moving down the assembly line.

Two AI models worked in tandem on the task. Gemini Robotics ER monitored the workflow, while Gemini Robotics 2.5 controlled the robot's hands and arms. Google DeepMind reports the 96 percent success rate for this pilot.

Apptronik CEO Jeff Cardenas said the models improved significantly within a few weeks of post-training and successfully transitioned to Mercedes production with minimal performance loss. That transition claim addresses a core problem in robotics: models trained in one environment often lose accuracy when moved to another. Cardenas's statement, as reported, says the loss here was small.

The stakes for automakers are concrete. Kitting is repetitive, precision-dependent work that currently depends on human labor arranged around takt time. A 96 percent success rate on live lines, if sustained, puts humanoid robots inside the operating envelope that logistics and assembly managers evaluate for automation — though Google DeepMind's figures are company-reported and drawn from a pilot, not an independent audit.

## How does the model handle safety around people and obstacles?

Google DeepMind says Gemini Robotics 2.5 uses real-time control that enables a robot to avoid physical hazards while performing a task. In the company's own evaluations, safe task completion improved by 33 percent near obstacles and by 43 percent near people. Those two numbers frame the deployment question for factories where robots and human workers share space, which is the stated setting of the Mercedes-Benz pilot.

Alongside the model, Google DeepMind published the open-source ASIMOV-Jailbreak benchmark on Hugging Face. The benchmark contains hundreds of prompts that disguise unsafe requests in dozens of scenes, and it is designed to test whether robotics models can be tricked into performing dangerous actions.

The company disclosed results for its own models. Gemini 4 Argon achieves a jailbreak resistance of 99.5 percent on the benchmark, compared to 82 percent for its predecessor, Gemini Robotics ER 2. Publishing an adversarial benchmark invites external researchers to attack robotics models systematically — a step toward standardized safety measurement for embodied AI, an area that currently lacks equivalents to the leaderboards common in language-model evaluation.

## Where do the training data come from?

During development of Gemini Robotics 2.5, Google DeepMind used its own language model, Gemini 4 Argon, as a simulation engine. Argon can generate thousands of simulated training runs from a single real camera scene and create a digital twin of it. Combined with real-world training demonstrations, this pipeline is intended to help the robot generalize more quickly to new scenes and objects.

Argon also performs diagnostic work. According to Google DeepMind, the language model can derive hypotheses about the causes of errors from video recordings, then test proposed improvements both in simulation and on the physical robot. The approach mirrors a broader industry move: generating synthetic training data from world models rather than collecting every demonstration by hand, which is slow and expensive.

## Who gets access now?

Google DeepMind says Gemini Robotics 2.5 is being rolled out to partners Agile, Apptronik, and Mercedes-Benz. The named partner list indicates the model remains in a controlled deployment phase rather than general availability, with automotive assembly and humanoid-robot developers as the first users.

For Google DeepMind, the pilot converts a research lineage — from the original full-body control model earlier this year to this release — into measured factory-floor results: 96 percent sorting success, 30 to 100 percent task improvement over prior baselines, and 99.5 percent jailbreak resistance on its own safety benchmark. The next indicator to watch is whether those numbers hold as Apollo 2 moves beyond pilot lines into continuous production, and whether outside researchers use ASIMOV-Jailbreak to confirm or contest the safety claims.

Original: deepmind.google
