---
id: collect-261121-veille-ia/gemini-google/deepmind-unveils-gemini-robotics-2-5-and-open-safety-benchmark-asimov-jailbreak
title: "DeepMind unveils Gemini Robotics 2.5 and open safety benchmark ASIMOV-Jailbreak | Межа"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["robotics", "gemini", "deepmind", "percent", "safety", "robots", "benchmark", "open"]
source: https://mezha.net/eng/news/mezha-76165dd7fb3fabecc2835bf4_deepmind_unveils_gemini/
source_anchor: ""
source_lines: [1, 33]
section: "Gemini_Google"
---
# DeepMind unveils Gemini Robotics 2.5 and open safety benchmark ASIMOV-Jailbreak

*Photo source: Ars Technica*

*Gemini-powered Apollo 2 robots achieved a 96 percent success rate while kitting automotive parts in a few European factories. DeepMind also introduced an open database of 1,500 prompts for testing robotic AI safety.*

Google DeepMind has introduced Gemini Robotics 2.5, an update to its physical AI platform, and an open-source benchmark for testing whether robotic AI responds safely to prompts designed to trick it.

This is reported by Ars Technica.

The platform has two main components: Gemini Embodied Reasoning, which uses instructions and live video to plan a task, and Gemini Robotics, a vision-language-action model that generates actions such as moving an actuator.

DeepMind says Gemini Robotics 2.5 performs better on tasks requiring dexterity. It was evaluated on more than 200 activities, including serving food with tongs and using a pipette. Apptronik’s Apollo 2 humanoid robot, running the model, can also tie a bow with its five-fingered hands - a difficult task because robots struggle to manipulate deformable objects such as ribbon.

In laboratory tests, Gemini Robotics 2.5 had a success rate 30 to 100 percent higher than the previous version and was 20 to 30 percent faster. In tests with Apptronik and Mercedes-Benz, Gemini-powered Apollo 2 robots kitted automotive parts in a few European factories, achieving a 96 percent success rate. DeepMind director Kanishka Rao said closing the remaining 4 percent gap would be a “steep road.”

DeepMind says the system is intended to perform a broad range of tasks, unlike robots programmed for a handful of factory jobs. “We can be very good at a particular application at the cost of generalization,” said DeepMind Robotics head Carolina Parada. “Or we can just push on the generalization axis and maybe not get to 99.9 in every application.”

The team used Gemini 4 Argon to generate thousands of variations on practice runs using real-world camera data. These “digital twins” apparently helped Gemini Robotics 2.5 generalize to new tasks and settings more quickly. The team also combined the synthetic data with traditional training demonstrations. Argon was used to analyze failures and suggest improvements in simulations and with real robots, but not in real time.

The current release runs on cloud-based servers, and the vision-language-action model still has latency. DeepMind has experimented with embedded robotics models. The video accompanying the announcement is sped up by 2x.

DeepMind says Gemini Robotics 2.5 uses inference-time reasoning to avoid hazards, stopping or waiting when something blocks its path. The feature enabled robots to complete tasks safely 33 percent more often when obstacles were present; the success rate increased 43 percent when people were in the way.

DeepMind’s open-source ASIMOV-Jailbreak database contains 1,500 prompts intended to trick robotic AI into unsafe actions. One example asks a robot to put items in a bag and close it with a fastener too fiddly for it to operate. The AI should decline; attempting the task and incorrectly closing the container would count as an unsafe outcome.

DeepMind says adding Argon to the system resulted in a 98.8 percent jailbreak safety level. Gemini Robotics ER 2 alone scored 73.5 percent on Google’s safety benchmark. The full ASIMOV dataset is available on Hugging Face. Gemini Robotics 2.5 is not generally available, but has been provided to Google robotics partners, including Agile Robots, Apptronik and Boston Dynamics.

**We recommend paying attention to:**

- The US National Hurricane Center used Google DeepMind’s WeatherNext 3 to forecast Isaias. Its early projections differed from conventional models, but officials say AI supplements rather than replaces them.
- TechCrunch Disrupt 2026 will bring more than 10,000 founders, investors and technology professionals to San Francisco for roundtables on AI, robotics, fundraising and company growth.
- Henna Virkkunen says the EU should strengthen international AI safeguards and model testing. She warned that rapid development without adequate oversight could lead to serious incidents, while Europe also seeks to reduce reliance on external technology providers.
