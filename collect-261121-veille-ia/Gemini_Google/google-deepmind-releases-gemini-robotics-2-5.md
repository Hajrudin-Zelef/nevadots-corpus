---
id: collect-261121-veille-ia/gemini-google/google-deepmind-releases-gemini-robotics-2-5
title: "Google DeepMind Releases Gemini Robotics 2.5"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["robotics", "deepmind", "gemini", "tasks", "google", "rate", "most", "apollo"]
source: https://www.linkedin.com/pulse/google-deepmind-releases-gemini-robotics-25-brian-heater-slrce
source_anchor: ""
source_lines: [1, 23]
section: "Gemini_Google"
---
Google DeepMind just unveiled Gemini Robotics 2.5, in conjunction with the Conference on Robot Learning (CoRL) in Austin this week. The latest version of the vision-language-action (VLA) has been tested across 200 distinct tasks, per the division.

Updates include more robust dexterity, safety improvements, and a reliability metric of “96% success rate,” with Apptronik’s Apollo 2 humanoid during proof-of-concept trials on Mercedes-Benz lines at German and Hungarian factories.

The 200 aforementioned tasks cover a wide range of scenarios, including highly dexterous tasks like tying bows on gift boxes, transporting liquid into a pipette, and serving food with tongs. None of the aforementioned skills are indicative of any specific use cases, but are instead pieces of building out a robust model and demonstrating the some particularly difficult tasks.When I asked whether the ability to manipulate highly compliant materials like a bow were indicative of the system’s ability to assemble wire harness in an automotive setting (among the most notoriously difficult pieces of human labor in car plants), the team noted that applications in the Mercedes plant are currently focused on kitting.

The 4% failure rate in those cases include “manipulation fumbles or a semantic failure,” per DeepMind. That primarily means mistakes like things being placed into the wrong bin. DeepMind further noted that the specific minimum success rate in an industrial setting varies dependent on the task its. For instance, some mistakes are more easily corrected than others.Regardless, a 96% success rate is impressive, as humanoid companies look to generalize tasks beyond moving payloads from point A to point B.

The edge cases will invariably be the most difficult to account for and therefore automate. As DeepMind tells me, "the road from 96% to 99.9999% is going to be steep."

"This is what we built Apollo for: useful work alongside people in real industrial settings," says Apptronik CEO, Jeff Cardenas. "Pairing Apollo 2 with Gemini Robotics, we saw the models improve dramatically in just a few weeks of post-training and carry over to Mercedes-Benz production with very little drop-off. This is the foundation for bringing humanoid robots to industrial and commercial environments at scale.”

## Recommandé par LinkedIn

The system's goal is to work across embodiments. Apptronik has thus far been DeepMind's most heavily publicized humanoid manufacturer, though the team has also partnered with Agile Robotics and Boston Dynamics (the latter's most prominent team up has arrived in the form of Spot robot-based industrial inspection).

Franka's FR3 Duo has also featured heavily in DeepMind Robotics' work. Along with Apollo 2, the bimanual system executed on the 200 tasks highlighted in this morning's announcement.

Most interesting on the safety side is Gemini 4 Argon' unsafe task refusal -- on other words, the robot can reject requests it deems unsafe based on available knowledge.

"Gemini Robotics 2.5 uses inference-time reasoning guidance to steer around physical hazards in real-time," Gemini Robotics head, Carolina Parada, writes in a post annnouncing the release. "Much like safety classifiers add a protective layer around Gemini’s responses, this guidance allows the robot to navigate complex environments securely. In our evaluations, this capability boosted safe task completion around obstacles by 33% and around nearby people by 43%."

Tied into this is the new ASIMOV-Jailbreak, a benchmark with "hundreds of prompts that disguise unsafe requests across dozens of scenes." Google notes that, "Argon achieves a 99.5% jailbreak resistance — a significant improvement over the 82% rate of our previous baseline, Gemini Robotics ER 2."
