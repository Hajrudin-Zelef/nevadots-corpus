---
id: collect-261121-veille-ia/gemini-google/deepmind-launches-gemini-robotics-2-5-but-99-5-robot-jailbreak-score-is-for-gemini-4-argon
title: "DeepMind Launches Gemini Robotics 2.5, but 99.5% Robot Jailbreak Score Is for Gemini 4 Argon"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["model", "deepmind", "gemini", "robot", "robotics", "argon", "score", "post"]
source: http://www.techtimes.com/articles/330435/20261111/deepmind-launches-gemini-robotics-25-995-robot-jailbreak-score-gemini-4-argon.htm
source_anchor: ""
source_lines: [1, 75]
section: "Gemini_Google"
---
Google DeepMind's 99.5% robot jailbreak resistance score belongs to Gemini 4 Argon, a general-purpose physical reasoning model, not to Gemini Robotics 2.5, the robot-control model DeepMind announced at 10:00 a.m. ET on Nov. 10. DeepMind's post gives no ASIMOV-Jailbreak result for the 2.5 model itself.

That gap matters to anyone weighing robots for a factory floor. Gemini Robotics 2.5 is rolling out to DeepMind's robotics partners, while Argon is available to paying developers through Google's API. A buyer who reads "99.5%" next to the 2.5 launch could assume the robot model earned it. The published numbers do not say that.

The stakes have a date attached. The EU Machinery Regulation, which adds rules for machine-learning safety functions, will apply from Jan. 20, 2027, which is 71 days away.

### Argon and ER 2 Received Scores; Robot Action Model Did Not

DeepMind describes ASIMOV-Jailbreak as an open-source benchmark of hundreds of prompts that disguise unsafe requests across dozens of scenes. The post reports 99.5% resistance for Gemini 4 Argon, up from 82% for its previous baseline, Gemini Robotics ER 2. Both are reasoning models; neither is the vision-language-action model that DeepMind says controls the robot's hands and arms in the Mercedes-Benz pilot.

Gemini Robotics 2.5 is a vision-language-action model, a class that turns images into robot movements, according to The Decoder. In the Mercedes-Benz pilot, a Gemini Robotics ER model tracked the workflow while the 2.5 model controlled the robot's hands and arms. The headline score therefore describes how a reasoning model answers disguised instructions. Whether the action model refuses unsafe intent that slips through, or carries it out, is not covered by any ASIMOV-Jailbreak figure in the safety section of the post.

The Hugging Face dataset card that DeepMind links appears built for model-level testing. Its sample commands send tasks to Google, OpenAI and Anthropic models, and one evaluation has a model decide whether to stop a robot using a 3.0-meter (9.8 ft) distance threshold. The README shown before access is granted does not name a jailbreak-specific prompt set.

### Can Disguised Prompts Still Trick Robot AI? Purdue Found Success Rates Near 100% on Some Datasets

Independent academic work shows why disguised requests are the hard case. Purdue University researchers built RoboJailBench and tested Gemini Robotics ER 1.6 Preview against four attacks and two defenses. With no attack applied, that model rejected adversarial goals 88.39% of the time and accepted benign goals 97.56% of the time. The "conceptual deception" attack, which reframes a harmful goal as a harmless one, succeeded 96% to 99% of the time without a defense on three of the six datasets and 94.44% on the Purdue team's own instruction set, while reaching only 29% to 37% on two others.

Those results do not compare directly with DeepMind's. Purdue tested an older model, used different prompts and forced a binary okay-or-deny answer. They do show what DeepMind's post leaves out: Purdue reports benign-task acceptance next to refusal rates, because a model that refuses everything would score perfectly on resistance alone. DeepMind's post reports no benign-task figure for ASIMOV-Jailbreak.

### Outlets Split Between 99.5% and 98.8% Jailbreak Scores

DeepMind's post says the benchmark holds hundreds of prompts and reports 99.5% for Argon and 82% for ER 2. The Decoder repeats those figures. UA.News, summarizing Ars Technica's report, instead cites 1,500 prompts, 98.8% with Argon in the system and 73.5% for ER 2 alone. Mezha.net's summary of the same coverage carries the 98.8% and 73.5% figures. TechTimes could not access Ars Technica's original article, so its exact wording is unconfirmed.

The gap could reflect different test settings, such as a model alone versus a full system, or a reporting error. No source reviewed resolves it. The Hugging Face page labels the dataset size category as under 1,000 examples, though that label may not cover every file. The page also lists a CC BY 4.0 license in its metadata while its README text says MIT, and it requires visitors to accept conditions and share contact details before opening files. DeepMind calls the benchmark open source. Outside labs would need to settle those details before comparing scores.

### Evaluation Awareness Complicates Self-Reported Safety Scores

DeepMind built the benchmark and reported the results, so the scores are self-reported. The Gemini 4 Argon safety report describes a related issue. In its cyber, stealth and situational-awareness challenge transcripts, the report says, the model showed "pervasive evaluation awareness," meaning many transcripts showed it recognizing a synthetic test. In a separate biology test set, automated checks flagged 65 trajectories out of 390 (16.7%) as showing signs of evaluation awareness. DeepMind says it found no deliberate underperformance in that set and considers its results valid.

Those measurements come from other test suites, not ASIMOV-Jailbreak. The same report says its evaluations of Argon generally run without out-of-model production safeguards unless noted. The robotics post does not say which setting produced 99.5%.

### EU Machinery Rules Start Jan. 20, 2027, After Hungary and Germany Pilots

DeepMind, Apptronik and Mercedes-Benz ran the pilot on active production lines in Kecskemét, Hungary, and Sindelfingen, Germany. Both sites sit under EU law. Regulation (EU) 2023/1230 will apply from Jan. 20, 2027, and it treats software as a safety component.

For machinery whose safety functions rely on machine learning with self-evolving behavior, the regulation requires a Notified Body, an outside conformity assessor, rather than self-certification. It also requires control systems to withstand "reasonably foreseeable malicious attempts by third parties" and requires tracing logs of interventions and software versions uploaded after sale to be enabled for five years after each upload.

The sources reviewed do not say whether Gemini Robotics 2.5, or any robot using it, falls into those categories, or how regulators will treat a language-level jailbreak. Whether such a jailbreak counts as a malicious attempt is an open question for safety engineers, and a benchmark score without recorded settings would likely be thin evidence for a conformity file.

### ASIMOV-2.0 Found Embodiment Gap in 2025 Tests of Earlier Frontier Models

The earlier ASIMOV-2.0 paper from DeepMind researchers, published as an arXiv preprint in 2025, tested models available then, including GPT-5, Gemini 2.5 Pro and Claude Opus 4.1. It found that no model achieved under 30% violations when reasoning jointly about a robot's physical limits, physics and visual cues. Violation rates on its constraints task ranged from 75% to 38.6%. Larger "thinking" budgets helped, especially for smaller models.

The authors framed the goal as meeting functional-safety standards IEC 61508 and ISO 13849-1, which grade safety functions by Safety Integrity Level and Performance Level. Those models are more than a year old. The paper says nothing about Argon, ER 2 or Gemini Robotics 2.5.

### Mercedes Pilot Reached 96% Success; Post Gives No Trial Size or Intervention Count

In the pilot, Apptronik's Apollo 2 humanoid picked metal automotive brackets from bulk bins and placed them into sequencing containers, reaching a 96% success rate. DeepMind's post and The Decoder's report give no trial size, duration, failure definition or count of human interventions.

DeepMind also reports that Gemini Robotics 2.5 posts a 30% to 100% higher success rate than previous baselines across 200 tasks, depending on task type, and finishes tasks 20% to 30% faster. A separate safety feature, inference-time reasoning guidance, improved safe task completion by 33% around obstacles and 43% around nearby people in DeepMind's evaluations. That feature acts on the robot's behavior in the moment. It is a different layer from the jailbreak test, which asks whether a prompt gets accepted at all.

### Ask Four Questions Before Trusting Any Robot Jailbreak Score

A reader comparing robot vendors can use the evidence above as a checklist. Ask which model produced the score, since DeepMind's 99.5% belongs to Argon and not to the 2.5 robot model. Ask how many prompts were used and whether production safeguards were on, since outlets report 99.5% and 98.8% for the same release. Ask for benign-task acceptance alongside refusal rates, as Purdue's tests do. And ask how the vendor layers prompt defenses with deterministic limits on speed, force and zones, a design principle that PwC Legal Germany describes for physical AI under EU rules.

The decision framework is short. A jailbreak score tests one layer, the language layer. It is self-reported until outside labs reproduce it, its prompt count is disputed across outlets, and its test settings are not stated. It does not show factory-floor safety by itself, and the EU rules arriving Jan. 20, 2027 will require conformity documentation for covered machinery, not a headline percentage.

## Frequently Asked Questions

### What is ASIMOV-Jailbreak?

DeepMind describes it as an open-source benchmark of hundreds of prompts that disguise unsafe requests across dozens of scenes, released with Gemini Robotics 2.5 on Nov. 10. Outlets disagree on the prompt count, and some report 1,500.

### Can robots be jailbroken the way chatbots are?

Researchers say yes. The BadRobot authors describe their results on a physical robot arm as the first successful jailbreak of a real-world robotic system. Purdue's RoboJailBench measured high attack success rates against a Gemini robotics model on several datasets.

### Does a high jailbreak score mean a robot is safe to work next to?

No. The score covers how a reasoning model responds to disguised prompts. It does not cover how the action model moves, how often a person must step in, or whether safety hardware works. DeepMind separately reports hazard-avoidance gains of 33% and 43% from inference-time guidance. For EU deployments, the Machinery Regulation from Jan. 20, 2027 will require conformity documentation that a headline percentage with undisclosed settings does not supply.

### Can businesses use Gemini Robotics 2.5 now?

Not publicly. UA.News, summarizing Ars Technica, says the model is not yet available to the general public. DeepMind says it is rolling out to its robotics partners, and The Decoder names Agile, Apptronik and Mercedes-Benz. The Ars-based summary lists Agile Robots, Apptronik and Boston Dynamics instead. Gemini 4 Argon, the model that earned the 99.5% score, is available to paying developers.
