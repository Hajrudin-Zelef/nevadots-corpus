---
id: collect-261121-veille-ia/gemini-google/google-deepmind-s-gemini-robotics-2-5-scores-96-percent-sorting-metal-parts-in-a-mercedes-benz-assem
title: "Google DeepMind's Gemini Robotics 2.5 scores 96 percent sorting metal parts in a Mercedes-Benz assembly pilot"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "deepmind", "gemini", "robotics", "percent", "model", "robot", "mercedes-benz"]
source: https://the-decoder.com/google-deepminds-gemini-robotics-2-5-scores-96-percent-sorting-metal-parts-in-a-mercedes-benz-assembly-pilot/
source_anchor: ""
source_lines: [1, 45]
section: "Gemini_Google"
---
# Google DeepMind's Gemini Robotics 2.5 scores 96 percent sorting metal parts in a Mercedes-Benz assembly pilot

## Key Points

- Google Deepmind introduced Gemini Robotics 2.5, a new VLA model it says performs 30 to 100 percent better than its own previous baselines depending on the task type.
- On active Mercedes-Benz production lines in Hungary and Germany, Apptronik's humanoid robot Apollo 2 sorted metal parts with a reported success rate of 96 percent.
- Google Deepmind uses its language model Gemini 4 Argon as a simulation engine for training and released the open-source benchmark ASIMOV-Jailbreak to test the safety of robotics models.

**Google Deepmind has unveiled a new AI model for robot control. In a pilot test on active Mercedes-Benz production lines, a humanoid robot reportedly achieved a 96 percent success rate.**

Google Deepmind has introduced Gemini Robotics 2.5, a vision-language-action model (VLA) that processes visual information, understands language instructions, and translates them into robot movements. The model builds on its predecessor, introduced earlier this year, which was designed to give robots full-body control for the first time.

Google Deepmind says it tested the model on more than 200 tasks, including serving food with tongs and pipetting liquids. Compared to the company's own previous baselines, it reportedly achieves a 30 to 100 percent higher success rate depending on the task type and completes tasks 20 to 30 percent faster.

The model is designed to work with different robot types, including Apptronik's humanoid robot Apollo 2 with a five-fingered hand that has 22 degrees of freedom, and the Franka Duo platform with two-finger parallel grippers.

## Google Deepmind tests the model on active Mercedes-Benz production lines

Together with Apptronik and Mercedes-Benz, Google Deepmind tested the model on active production lines in Kecskemét, Hungary, and Sindelfingen, Germany. The Apollo 2 robot handled kitting: it sorted metal parts from bulk bins and placed them into trays in the order needed for each vehicle on the assembly line. Two AI models worked in tandem. Gemini Robotics ER monitored the workflow, while Gemini Robotics 2.5 controlled hands and arms.

*Video: Google*

Google Deepmind reports a 96 percent success rate for this pilot test. Apptronik CEO Jeff Cardenas said the models improved significantly within a few weeks of post-training and successfully transitioned to Mercedes production with minimal performance loss.

## New Safety Benchmark Against Jailbreaks

According to Google Deepmind, Gemini Robotics 2.5 uses real-time control that enables the robot to avoid physical hazards while performing a task. In its own evaluations, safe task completion improved by 33 percent near obstacles and by 43 percent near people.

In addition, Google Deepmind has published the open-source ASIMOV-Jailbreak benchmark on Hugging Face. It contains hundreds of prompts that disguise unsafe requests in dozens of scenes and is designed to test whether robotics models can be tricked into performing dangerous actions. According to the company, Google’s own model, Gemini 4 Argon, achieves a jailbreak resistance of 99.5 percent, compared to 82 percent for its predecessor, Gemini Robotics ER 2.

## Argon Language Model Generates Training Data via Simulation

During the development of Gemini Robotics 2.5, Google Deepmind used its own language model, Gemini 4 Argon, as a simulation engine. Argon can generate thousands of simulated training runs from a real camera scene and create a digital twin. In combination with real-world training demonstrations, this is intended to help the robot generalize more quickly to new scenes and objects. Argon can also derive hypotheses about the causes of errors from video recordings and test improvements in simulation and on the actual robot.

According to Google Deepmind, Gemini Robotics 2.5 is being rolled out to partners Agile, Apptronik, and Mercedes-Benz.

THE DECODER+

## AI News Without the Hype – Curated by Humans

- Every Plus article and the full archive
- No ad banners
- Frontier Radar: 6 deep dives a year

Already a subscriber? Sign in
