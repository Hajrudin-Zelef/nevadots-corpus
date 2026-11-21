---
id: collect-261121-veille-ia/gemini-google/gemini-robotics-2-5-put-humanoids-on-a-real-mercedes-line-96-success-and-one-big-catch
title: "Gemini Robotics 2.5 Put Humanoids on a Real Mercedes Line — 96% Success, and One Big Catch"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-10"]
keywords: ["gemini", "robotics", "deepmind", "model", "real", "robot", "success", "kitting"]
source: https://dev.to/paullovelace/gemini-robotics-25-put-humanoids-on-a-real-mercedes-line-96-success-and-one-big-catch-5gje
source_anchor: ""
source_lines: [1, 48]
section: "Gemini_Google"
---
Yesterday, November 10, 2026, Google DeepMind announced Gemini Robotics 2.5 — its most capable vision-language-action (VLA) model to date. The headline number is a good one: Apptronik's Apollo 2 humanoid, running this model, picked and packed automotive parts on active Mercedes-Benz production lines with a 96% success rate.

That is not a lab demo. The robots worked on real lines in Kecskemét, Hungary and Sindelfingen, Germany, doing automotive kitting — sorting individual parts from bulk bins into organized trays in the exact order each vehicle on the line needs. It is repetitive, physically taxing work, and it is exactly the kind of job humanoid robots have been promised for years and rarely actually done.

I have not tested Gemini Robotics 2.5 hands-on — nobody outside DeepMind's partner group has access yet — and this article is built from DeepMind's announcement of November 10, 2026, checked the same day. Where a claim is DeepMind's, I will say so, because a launch-day benchmark is a vendor claim, not an independent result.

A VLA model is the robot's brain in one package: it takes in what the robot sees (vision), understands a spoken or written instruction (language), and outputs the physical movements to do the job (action). Earlier robotics stacks split those jobs across separate systems that had to be stitched together. Gemini Robotics 2.5 is one model doing all three, trained to generalize across robot bodies — it runs on Apptronik's Apollo 2 humanoid with its five-finger, 22-degree-of-freedom SharpaWave hand, and on the Franka Duo platform with two-fingered grippers.

DeepMind tested it on more than 200 distinct tasks — serving food with tongs, tying a bow on a gift box, pipetting liquid, handling delicate and flexible objects. Compared with previous baselines, it achieved 30% to 100% higher success rates depending on the task type, while finishing tasks 20% to 30% faster. The range matters: the biggest gains are on the hard tasks, the ones where older models fumbled.

The system is really two parts working together. Gemini Robotics ER is the embodied reasoning model — the planner that keeps track of the workflow, figures out the multi-step sequence, and adapts when something changes. Gemini Robotics 2.5 is the actions model that controls the hands and arms. In the Mercedes pilot, ER tracked the kitting sequence and 2.5 did the reaching, picking, and placing, correcting for gaps as it went.

And there is a third model behind the curtain: Gemini 4 Argon, Google's frontier model, which DeepMind credits with accelerating the research. Argon acted as a simulation engine, generating thousands of varied practice runs from a real camera scene and building a digital twin of the workspace. Combined with real training demonstrations, that let the robot generalize to new scenes and objects faster — and let researchers diagnose failures from video and test fixes in simulation before trying them on a real robot.

The most important word in this announcement is "active." These were not staged lines built for a camera crew. Apollo 2 worked on production lines that were running, doing kitting that technicians would otherwise do by hand.

Here is what 96% means in practice. Kitting is sorting parts from bulk bins into trays in the right order for each vehicle. A 96% pick-and-pack success rate means the robot got it right 96 times out of 100 — and, just as importantly, that the system could correct for gaps and keep the line moving. DeepMind's framing is that this frees technicians to focus on complex build tasks instead of monotonous prep work. That is the honest pitch for humanoids in factories: not replacing the skilled worker, but taking the repetitive, physically taxing job off their plate.

But keep the number in proportion. This is a proof-of-concept on one task — kitting — at two plants, with a partner's robot, under DeepMind's supervision. It is a strong signal that VLA models are ready for real factory work, but it is not a deployment announcement. Mercedes has not said it is buying fleets of Apollo robots, and DeepMind has not said when this moves from pilot to production.

As robots move into messy, everyday spaces, the safety question stops being theoretical. DeepMind advanced two things here that matter more than the success rates.

First, inference-time reasoning guidance that steers the robot around physical hazards in real time — the same idea as safety classifiers around a language model, applied to movement. In DeepMind's evaluations, this boosted safe task completion around obstacles by 33% and around nearby people by 43%.

Second, a new open-source benchmark called ASIMOV-Jailbreak, now on Hugging Face. It contains 1,500 prompts (DeepMind's blog says "hundreds of prompts that disguise unsafe requests across dozens of scenes") designed to test whether a robot can be talked into doing something unsafe — the physical-world version of a prompt injection. Gemini 4 Argon scores 99.5% jailbreak resistance on it, up from 82% for the previous baseline, Gemini Robotics ER 2.

If you build anything with robots, this is the part to take seriously. A language model that gets jailbroken writes bad text. A robot that gets jailbroken moves a heavy object near a person. The fact that DeepMind is releasing the benchmark open-source means the whole field can now be measured on it — and that is a genuine contribution, whatever you think of the 96% headline.

Here is the part the headlines skip. Gemini Robotics 2.5 is rolling out to DeepMind's robotics partners only — Agile Robots, Apptronik, Boston Dynamics, and 100+ trusted testers. There is no public API. There is no price. There is no general availability date. If you are a developer or a robotics builder reading this, you cannot call this model today, and nobody has said when you will be able to.

That is the honest shape of this launch: a capability announcement and a pilot result, not a product you can build on yet. What you *can* do today is narrower but real:

- 
**Study the ASIMOV-Jailbreak benchmark.** It is open-source on Hugging Face, and if you work on embodied AI, testing your own system against it is the most useful thing to come out of this launch.
- 
**Watch the VLA pattern.** The two-model split — a reasoning planner plus an actions model — is becoming the standard architecture for capable robots. If you are designing a robotics stack, that split is worth understanding now, before the models reach you.
- 
**Follow the partner program.** If your company builds robots or runs factories, the 100+ trusted tester group is the door in. DeepMind has not published an application process, but the partner list is the place to start.

The practical takeaway is short. Yesterday, humanoid robots on a factory floor were a demo-reel promise. Today, there is a documented result: 96% success on a real Mercedes line, on a task that matters, with a safety benchmark the whole field can now use.

But the gap between "proven in a pilot" and "available to build with" is still wide, and DeepMind has not said how fast it closes. If you are a robotics builder, the useful work today is the safety benchmark and the architecture pattern — not waiting for an API that does not exist yet.

Would you trust a humanoid on your factory floor at 96%, or is the remaining 4% the whole story?

*Sources: Google DeepMind blog, "Gemini Robotics 2.5: Accelerating Physical Intelligence", November 10, 2026; Unite.AI coverage of the Apollo 2 / Mercedes-Benz pilot, November 10, 2026; Crypto Briefing coverage, November 10, 2026. All capability, benchmark and pilot figures are DeepMind's, stated as such. No hands-on testing was performed for this article. This breaking article is not the daily buyer-intent post, and it contains no affiliate links.*

## Top comments (0)
