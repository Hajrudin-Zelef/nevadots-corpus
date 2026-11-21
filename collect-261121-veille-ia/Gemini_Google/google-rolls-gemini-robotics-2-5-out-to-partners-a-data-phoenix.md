---
id: collect-261121-veille-ia/gemini-google/google-rolls-gemini-robotics-2-5-out-to-partners-a-data-phoenix
title: "Google rolls Gemini Robotics 2.5 out to partners â Data Phoenix"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["robotics", "deepmind", "gemini", "partners", "google", "model", "task", "benchmark"]
source: https://dataphoenix.info/news/google-deepmind-gemini-robotics-2-5-partner-rollout
source_anchor: ""
source_lines: [1, 15]
section: "Gemini_Google"
---
Google DeepMind introduced Gemini Robotics 2.5 on November 10 and began rolling it out to robotics partners. DeepMind says the new model can handle a wider range of dexterous tasks, while a companion benchmark tests attacks on robots.

DeepMind evaluated Robotics 2.5 across 200 tasks, including tool use, deformable-object manipulation and delicate handling. Depending on the task, the company reported success rates 30% to 100% higher than previous baselines and completion times 20% to 30% faster. The opened evidence does not independently replicate those results.

Robotics 2.5 is a vision-language-action model that turns camera input and language instructions into commands controlling a robotâs movement. DeepMind pairs it with Gemini Robotics ER, a separate system that reasons about a workspace and plans a sequence of actions.

DeepMindâs robotics model page names Agile Robots, Apptronik and Boston Dynamics as research partners. It also says the program works with more than 100 trusted testers. The company has not disclosed general-availability timing or commercial terms for Robotics 2.5.

For one partner proof of concept, DeepMind said Apptronik Apollo 2 robots used Robotics ER and Robotics 2.5 for automotive kitting on active Mercedes-Benz production lines in KecskemÃ©t, Hungary, and Sindelfingen, Germany. The company reported a 96% success rate in that trial.

The update also adds controls intended to make robot behavior easier to steer around hazards. In DeepMindâs evaluations, reasoning guidance applied while the model was running increased safe task completion by 33% around obstacles and 43% around nearby people. Those figures are company-reported evaluation results, not independently replicated findings.

DeepMind also released ASIMOV-Jailbreak, an open benchmark for testing whether adversarial prompts can push embodied agents toward unsafe physical actions. Googleâs dataset card describes paired harmful and benign tabletop task suites, plus multi-turn attacks that escalate over several exchanges. It lists 845 adversarial jailbreak episodes across 15 strategy groups, with as many as 10 steps in each episode.

The release follows Gemini Robotics 2, announced in July with an emphasis on whole-body control, dexterity and coordination across different robot types. The 2.5 update adds the partner rollout, the 200-task evaluation, the Mercedes-Benz production-line trial and the ASIMOV-Jailbreak benchmark.
