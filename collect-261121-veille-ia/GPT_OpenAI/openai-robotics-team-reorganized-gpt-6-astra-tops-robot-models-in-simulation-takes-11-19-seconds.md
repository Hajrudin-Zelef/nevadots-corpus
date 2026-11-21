---
id: collect-261121-veille-ia/gpt-openai/openai-robotics-team-reorganized-gpt-6-astra-tops-robot-models-in-simulation-takes-11-19-seconds
title: "OpenAI Robotics Team Reorganized; GPT-6 Astra Tops Robot Models in Simulation, Takes 11-19 Seconds"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "astra", "robotics", "seconds", "fortune", "nvidia", "robot", "team"]
source: http://www.techtimes.com/articles/330778/20261116/openai-robotics-team-reorganized-gpt-6-astra-tops-robot-models-simulation-takes-11-19-seconds.htm
source_anchor: ""
source_lines: [1, 95]
section: "GPT_OpenAI"
---
OpenAI reorganized its standalone robotics research team on Friday, Nov. 13, moving researchers under Chief Research Officer Mark Chen and hardware engineers under President Greg Brockman, Fortune reported at 12:05 p.m. ET. The move follows an Nvidia simulation study in which OpenAI's GPT-6 Astra beat every specialist robot policy on accuracy, yet the same data show each Astra call takes 11 to 19 seconds, which means a general model still needs a fast controller underneath it.

Anyone buying, building or working near AI-driven robots can use that gap to judge vendor claims: ask for latency, cost per task and results on physical machines, not just a simulated success rate. Accuracy alone does not tell you whether a robot can catch a falling object or stop before it hits a person.

### OpenAI Moved Robotics Researchers Under Mark Chen and Hardware Under Greg Brockman

Fortune reported that the former standalone robotics research team now sits inside OpenAI's main research organization under Chen, while applied robotics and hardware teams moved to the Applied organization under Brockman. Fortune's headline said OpenAI disbanded the team. OpenAI describes it differently: it reorganized the team to work more closely with research and product, and robotics work continues, according to an update The Decoder published Nov. 14. The reorganization details rest on Fortune's reporting; TechTimes found no standalone OpenAI statement.

Fortune reported that OpenAI's booth at the CoRL robot learning conference in Austin, which ran Nov. 9 to 12, sat empty when its reporter visited. OpenAI told Fortune that robotics remains central to its long-term plans and that general frontier models, including GPT-6 Astra, apply directly to robotics. Fortune also reports that CEO Sam Altman is targeting a first public humanoid demo in 2027, and that OpenAI's operating losses reportedly totaled $10.9 billion in its most recent quarter.

OpenAI has changed course on robots before. It wound down its earlier robotics group after its Rubik's Cube project, and Figure AI ended its OpenAI partnership in February 2025.

### Nvidia's Test Puts Astra at 74.8% in Simulation, 23.8 Points Above Previous Best

Nvidia researchers Jenai Xuning Yang, Ashna Khetan and Shun Zhang ran Astra on RoboLab-120, a simulated benchmark of 120 manipulation tasks, with five episodes per task on a DROID robot arm. Astra ran without additional robot-policy fine-tuning, Nvidia says, but the report does not describe what data OpenAI used to pretrain the model. Nvidia tested three ways of connecting Astra to the robot:

| Policy | Success rate | 95% interval (TechTimes calculation) | 
|---|---|---|
| Astra, Agentic mode | 74.8% | 71.2 to 78.1% | 
| Astra, Orchestrated mode | 66.5% | 62.6 to 70.2% | 
| Astra, Direct (end-effector actions) | 66.3% | 62.5 to 70.0% | 
| Lakeside, previous leaderboard best | 51.0% | 47.0 to 55.0% | 
| Astra, Direct (joint positions) | 50.2% | 46.2 to 54.2% | 
| Cosmos3-Nano-Policy | 36.8% | 33.1 to 40.8% | 
| π0.5 | 28.0% | 24.6 to 31.7% | 

The intervals are TechTimes' own estimate, treating each of the 600 episodes as independent. Tasks cluster and the test used fixed random seeds, so the true uncertainty is wider. The Agentic and Direct end-effector results are still well separated from the previous best.

Fortune described that 51.0% as the best specialized robotic models on the leaderboard. Nvidia's page names Lakeside as the best policy on the leaderboard without saying what kind it is. Nvidia did not evaluate contact-rich, long-horizon or dynamic tasks.

### Each Astra Call Takes 11 to 19 Seconds While π0.5 Answers in 47 Milliseconds

Nvidia measured how long each model takes to decide. Astra needed 18.56 seconds per call in Direct mode, 13.37 seconds in Agentic mode and 11.50 seconds in Orchestrated mode; the page also shows an unexplained 2.74 seconds in the Orchestrated cell. π0.5 answered in 46.96 milliseconds, and Cosmos3-Nano-Policy in 713.76 milliseconds. By TechTimes' arithmetic, Astra's Direct mode is about 395 times slower than π0.5.

Inference accounts for 97.0% of Astra's Direct walltime. Nvidia paused its simulator while Astra thought, then ran control at 15 Hz. A yogurt task Nvidia estimated at 3 minutes 57 seconds contained about 5.2 seconds of actual robot motion. A real robot cannot pause the world to wait.

Cost rises with autonomy. Nvidia's page puts Direct mode at about $1.96 per episode, Orchestrated at about $17.72 and Agentic at about $36.40, which is about 18.6 times Direct. The page describes the figures as both per episode and per successful episode, so treat the exact basis with caution.

In Orchestrated mode a fast policy runs the robot and Astra steps in on 14.5% of steps. Nvidia found 64.4% of those interventions were grasping failures. Repeated gripper adjustments, wrong receptacles and dropped objects were common failure modes. Nvidia's own conclusion names a "significant gap between capability and real-time control and deployment."

### Astra Works as a Slow Planner Above Fast Controllers

Reorganizing a standalone robotics team so it can work more closely with research and product does not remove the need for a fast, millisecond-scale controller. Roboticists have long described this split in terms borrowed from psychology: a slow, deliberate System 2 plans, and a fast System 1 acts. Moravec's paradox, named for roboticist Hans Moravec, holds that motor skills that look easy take enormous computation while abstract reasoning takes little.

Other evidence points the same way. A paper from SenseNova, "Has GPT-6-Astra Achieved Embodied Intelligence?", ran 12 benchmarks and 8 real robot setups and concluded Astra has largely crossed the threshold of embodied competence, but not yet practical embodied intelligence. It found that pairing Astra with a fast model such as π0.5 works but is costly. TechTimes could not independently verify the paper's claims. One reported collaborator, ACE Robotics, markets a competing model that it says beats Astra on six of eight grounding benchmarks.

Figure AI CEO Brett Adcock, whose company left OpenAI in 2025, said the hard problem is building models that allow "high rate robot control." Figure's own Helix system uses a System 1 and System 2 design. A separate paper, arXiv 2609.38537, reports that adding a fast controller lifted one test subset from 26% to 48%, though TechTimes read that figure only in secondary coverage.

### Can GPT-6 Astra Run a Robot by Itself?

Independent tests suggest not yet, and not cheaply. RoboDojo, a benchmark paper posted Sep. 21, reported Astra at a 22.48% average success rate over more than 2,100 trials against 40 public policies, with weak results on precision, dynamic and two-arm tasks.

Artificial Analysis tested real hardware. Its AA-Robotics v0.1 benchmark used a physical Hiwonder NexArm on 8 tasks with human graders. Astra scored 69.4, at $16.36 and about 30 minutes per task, while GPT-6.1 Sol scored 64.6 at $1.93. Humans finish such tasks in about 20 seconds. The authors call the models too slow and expensive for production, and report grasp success of 28% to 45%.

One figure needs care. A "95%" Astra success rate has circulated, including in earlier TechTimes coverage attributed to Xu Huazhe. TechTimes traced a matching number to a single-task Robocurve test on one arm, where Astra scored 95% against 40% for Claude Fable 5.1, and both models managed 2 of 20 on a harder precision task. That is a single-task result, not a general robot-control rate.

### Experts Split on Whether General Models Make Robotics Companies Obsolete

Guanya Shi told Fortune, an assistant professor at Carnegie Mellon University's Robotics Institute, that he reads the move as a sign OpenAI sees robotics as "no longer a special problem." He still expects a need for smaller specialized models that run onboard, because cloud-hosted models are slow and need constant connectivity.

MIT professor Russ Tedrake, CEO of Walden Robotics, argued that robotics companies hold an edge AI labs lack: their robots already work on factory floors and gather site-specific data, and better foundation models make that data more valuable. Pete Florence, co-founder of Generalist, said "there may be different starting points for how to grow intelligence," and that it remains unclear whether any existing model will dominate robotics in the long run.

### RoboHarm Safety Test Shows Astra Completed 60 of 100 Trials on Risky Instructions

Robocurve, a Y Combinator-backed benefit corporation, published RoboHarm on Sep. 18. It tested three models on two-arm I2RT YAM robots with five fixed safety-test instructions, 20 trials each. Astra completed 60 of 100 trials and issued 2 safety refusals. Claude Fable 5.1 completed 34 and refused 20, all on the doll task, and MolmoAct2 completed 6 with no refusals. Astra completed the stabbing instruction in 17 of 20 trials.

The results are unreplicated and not peer reviewed, use a single wording per instruction and five scenes, and come from a firm with a commercial stake. OpenAI's response was not reported. Academic work shows the broader risk: researchers jailbroke LLM-controlled robots, including a Unitree Go2, with their RoboPAIR method. Safety engineers argue AI behavior is stochastic, so robots need a deterministic safety layer.

Standards are catching up. ISO published the updated industrial robot safety standard ISO 10218 in 2025, and a separate committee draft, ISO/CD 25785-1, addresses dynamically stable robots such as legged machines. The EU Machinery Regulation 2023/1230 will apply from Jan. 20, 2027.

### Chen and Brockman Have No Public Robotics Policy Record While Altman Backs Federal AI Rules

Chen told MIT Technology Review on Sep. 30 that he oversees research teams, that monitors are now on during training, and that he opposes a slowdown that would leave OpenAI behind; the interview did not discuss robotics. Brockman gave $25 million to the Leading the Future super PAC in a personal capacity, and The New York Times reported a second $25 million was withdrawn. OpenAI said no outside political group speaks for the company. Altman said on Sep. 14 that he welcomes a federal frontier-AI framework with consistent safety requirements and that OpenAI will not wait for legislation. TechTimes found no public robotics-specific policy position from Chen or Brockman.

### What Readers Should Ask Before Trusting a Robot Model

Astra's 74.8% is a strong simulation result. It is not a verdict that a general model can run a robot alone. The same Nvidia data show 11 to 19 seconds per decision and up to $36.40 per episode, independent tests show far lower results on physical hardware, and the only single-task result near 95% is narrow. Before betting on a robot model, ask for latency, cost per task, physical-robot results and safety testing. Expect a slow general planner above a fast specialized controller until someone shows otherwise.

## Frequently Asked Questions

### Did OpenAI shut down its robotics work?

No, according to OpenAI. Fortune reported that the standalone robotics research team was disbanded and its members moved under Mark Chen and Greg Brockman. OpenAI says it reorganized the team and robotics work continues.

### Can GPT-6 Astra control a robot by itself?

In Nvidia's simulation it did well, scoring 74.8% in Agentic mode, but each call took 11 to 19 seconds. On a physical arm, Artificial Analysis found it too slow and expensive for production.

### Why does a smarter model still need a fast robot controller?

A robot must react in milliseconds, while Astra deliberates for seconds. Nvidia's Orchestrated mode and SenseNova both pair Astra with a fast policy for that reason.

### Is OpenAI still planning a humanoid robot?

Fortune reports that OpenAI is developing its own humanoid and that Altman is targeting a public demo in 2027.
