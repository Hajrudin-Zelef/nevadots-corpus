---
id: collect-261121-veille-ia/gemini-google/deepmind-unveils-gemini-robotics-2-5-and-an-open-safety-test-for-robot-ai
title: "DeepMind unveils Gemini Robotics 2.5 and an open safety test for robot AI"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["percent", "deepmind", "gemini", "robotics", "model", "safety", "robots", "open"]
source: https://iaminer.com/news/deepmind-unveils-gemini-robotics-2-5-and-an-open-safety-test-for-robot-ai
source_anchor: ""
source_lines: [1, 21]
section: "Gemini_Google"
---
## KEY POINTS

- Gemini Robotics 2.5 shows a 30 to 100 percent higher success rate and is 20 to 30 percent faster than the previous version in lab tests.
- Apptronik Apollo 2 robots running the model kitted automotive parts in European Mercedes-Benz factories with a 96 percent success rate.
- DeepMind released ASIMOV-Jailbreak, an open source set of 1,500 prompts meant to test the safety of physical AI.

Google DeepMind has introduced version 2.5 of its Gemini Robotics platform, which it describes as moving closer to matching human performance on real-world tasks. Ars Technica reported that the release arrives together with an open source safety benchmark called ASIMOV-Jailbreak.

The platform has two main layers. Gemini Embodied Reasoning is a vision-language model that takes instructions and uses live video to plan how a robot should complete a job. Gemini Robotics, the vision-language-action model now at version 2.5, turns that plan into physical actions such as moving an actuator.

DeepMind says the new model is considerably better at tasks that demand fine dexterity. It was evaluated on more than 200 activities, including serving food with tongs and using a laboratory pipette. In a demonstration, Apptronik's Apollo 2 humanoid, running the model, tied a bow with its five-fingered hands. That is notable because deformable items such as ribbon are hard for robots to handle.

Compared with the previous version, the company reports a success rate that is 30 to 100 percent higher and speeds that are 20 to 30 percent faster, though those figures come from laboratory testing. To see how the system copes outside the lab, Google worked with Apptronik and Mercedes-Benz. Gemini-powered Apollo 2 robots kitted automotive parts in several European factories and reached a 96 percent success rate.

Ars Technica noted that this may still fall short of what a manufacturer needs, since a human doing such repetitive work is right almost every time. DeepMind director Kanishka Rao called closing the remaining 4 percent gap a "steep road." DeepMind Robotics head Carolina Parada described a trade-off between specializing in one application and pushing for broad generalization, which may not reach 99.9 percent in every case.

Training data was another focus. Real robot data, best gathered through teleoperation, is scarce compared with the text used for chatbots. According to the report, the team used Gemini 4 Argon to build a simulation engine that generated thousands of variations of practice runs from real camera data. DeepMind combined this synthetic material with traditional demonstrations, and Argon also helped analyze failures. These tools do not work in real time, and the 2.5 model still runs on cloud servers, so latency remains.

On safety, the model uses inference-time reasoning to avoid hazards, stopping or waiting when something blocks its way. DeepMind says this let robots finish tasks safely 33 percent more often when obstacles were present, and 43 percent more often when people were in the way.

ASIMOV-Jailbreak contains 1,500 prompts designed to push a robotic AI into unsafe behavior, such as asking it to close a bag with a fastener it cannot operate. With Argon in the loop, DeepMind reports a 98.8 percent jailbreak safety level, against 73.5 percent for Gemini Robotics ER 2 alone. The dataset is on Hugging Face. Gemini Robotics 2.5 is not generally available but is with partners including Agile Robots, Apptronik and Boston Dynamics.
