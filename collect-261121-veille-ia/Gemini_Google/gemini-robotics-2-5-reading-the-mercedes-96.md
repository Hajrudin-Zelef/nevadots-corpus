---
id: collect-261121-veille-ia/gemini-google/gemini-robotics-2-5-reading-the-mercedes-96
title: "Gemini Robotics 2.5: Reading the Mercedes 96%"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "robotics", "model", "success", "mercedes", "deepmind", "apollo", "them"]
source: https://robottesters.com/article-gemini-robotics-2-5-apptronik-mercedes
source_anchor: ""
source_lines: [1, 86]
section: "Gemini_Google"
---
On **10 November 2026**, Google DeepMind introduced **Gemini Robotics 2.5**, the newest version of its vision-language-action model.
      A VLA model takes camera images and an instruction in ordinary language and turns them into motor commands for a robot’s arms and hands, so the same model can, in principle, learn many jobs instead of one.
      The headline that travelled was a single number from a factory: Apptronik’s Apollo 2 humanoid, running the new model, kitted car parts on live Mercedes-Benz lines with a **96% success rate**.
    

That is a real step for humanoids, and it’s also the kind of figure a tester should slow down on. This piece explains what Apollo actually did in Kecskemét and Sindelfingen, then reads the 96% the way we’d read any robot claim before believing it: what was counted, what wasn’t, and what it would need to mean for a production line.

## Key takeaways

- Gemini Robotics 2.5 was announced on **10 November 2026** . DeepMind reports a**30–100%** higher success rate than its previous model depending on the task, and**20–30%** faster completion, across more than 200 tasks.
- In a proof-of-concept with Apptronik and Mercedes-Benz, Apollo 2 humanoids picked metal brackets from bulk bins into sequencing containers in Kecskemét (Hungary) and Sindelfingen (Germany), with a reported **96%** success rate.
- DeepMind did not publish the number of trials, the definition of a success, how often a person stepped in, or a cycle time. Mercedes has not published its own figures.
- The model runs on cloud servers, the factory video was sped up 2× according to Ars Technica, and Gemini Robotics 2.5 is only rolling out to partners. There’s no price and no date for anyone else.

## What Apollo 2 did on the Mercedes lines

The task is called kitting. Parts arrive in big bulk bins, and someone has to pick them out one by one and drop them into trays in the exact order a specific car on the line will need them. Carmakers call that just-in-sequence supply, and it matters because a wrong part in the wrong slot can stop the line further down. It’s repetitive and physically tiring, which is why it keeps coming up as a first job for humanoids, as it did with Figure at BMW’s Spartanburg plant (see our Figure 03 BotQ piece).

      DeepMind describes two models working together. **Gemini Robotics ER**, the embodied-reasoning model, keeps track of the workflow: which tray, which part, what’s missing.
      **Gemini Robotics 2.5** drives the arms and hands, reaching into the bin, picking a bracket and placing it, “all while correcting for any gaps”, in DeepMind’s words.
      Apollo 2 does this with the five-finger **SharpaWave** hand, which has 22 degrees of freedom. A degree of freedom is one independent joint motion, so 22 in a single hand is a lot of control to coordinate for a part that a simple gripper could also grab.
    

Jeff Cardenas, Apptronik’s co-founder and CEO, said the models “improve[d] dramatically in just a few weeks of post-training” and carried over to Mercedes production “with very little drop-off”. Post-training means taking the general model and tuning it on data from the specific job. If that holds up, it’s the most interesting claim in the release, because weeks of tuning per task is what would make a general robot cheaper to deploy than a custom cell. It’s also a claim from the company that builds the robot, and I haven’t seen an independent check of it.

## The 96% has no denominator yet

A success rate is a fraction, and DeepMind published only the result. The blog post doesn’t say how many kitting attempts the 96% covers, what counts as one success (a single pick, a full tray, a full shift), or whether a human reset the robot when it got stuck. TechTimes made the same point on 11 November and showed why it matters: 24 out of 25 and 960 out of 1,000 both round to 96%, but the first could plausibly sit anywhere from about 80% to 99%, while the second is pinned close to 96%.

Here is the checklist I’d want answered before calling this production-ready, in the order a plant manager would probably ask:

- **Trials.** How many kitting attempts, over how many shifts, at each plant.
- **Success definition.** Is a success one correct pick, or a complete, correctly ordered tray?
- **Interventions.** How often did a person step in, reset a part or restart the robot, and are those attempts counted as failures?
- **Throughput.** Picks per hour against a human doing the same station. DeepMind gave no cycle time for the pilot.
- **Variants.** How many part types and sequences were tested. Variety is exactly what makes car plants hard to automate.
- **Latency.** What happens on the line when the connection to the cloud model is slow or drops.

None of that makes the result fake. It means the 96% is a demonstration number for now, and the follow-up data is what turns it into a deployment number.

## What 96% means at factory scale

To see why the last few points matter, run the arithmetic. The table below is our own calculation on the one public figure, not something DeepMind or Mercedes published. It assumes each attempt succeeds or fails independently, which real failures usually don’t, so treat it as a sense of scale.

| Success per attempt | Misses per 1,000 attempts | Chance a 10-step kit has no miss | 
|---|---|---|
| **96%** (reported) | about 40 | about 66% | 
| 99% | about 10 | about 90% | 
| 99.9% | about 1 | about 99% | 

Forty misses in a thousand is a lot of manual fixing on a line that runs all day, and if the 96% is per pick rather than per tray, two in three complete trays come out clean. If it’s per tray, the picture is much better. That’s why the definition matters more than the number.

DeepMind is open about the gap. Kanishka Rao, a DeepMind director, told Ars Technica that closing the last 4% will be a “steep road”, and robotics head Carolina Parada framed the trade-off directly: you can be very good at one application at the cost of generalization, or push on generalization “and maybe not get to 99.9 in every application.” I think that’s the honest summary of the whole release. Gemini Robotics is a bet on one model that does many jobs reasonably well, and factories traditionally pay for machines that do one job almost perfectly.

## What the release does show

The jump from lab to line is real. Most humanoid manipulation results still come from research benches, and this one comes from two working plants in two countries. The lab numbers behind it are big too: DeepMind tested more than 200 tasks, from tying a bow on a gift box to pipetting liquid and serving food with tongs, and TechTimes listed task-level gains such as a toolbox lid going from 0% to 93% and ribbon bows from 0% to 70%. Deformable objects like ribbon and clothing are some of the hardest things to manipulate, so those gains count, even where the final numbers are still well below anything you’d sell.

      Safety got more attention than usual. DeepMind says inference-time reasoning, where the model checks the scene for hazards before and during each action, raised safe task completion by **33%** around obstacles and **43%** around nearby people.
      It also released **ASIMOV-Jailbreak**, an open benchmark of prompts that try to trick a robot into unsafe actions, and reports 99.5% resistance for Gemini 4 Argon against 82% for Gemini Robotics ER 2.
      Those are developer-run tests on a developer-built benchmark, but publishing the benchmark lets other labs check them, which is more than most humanoid safety claims offer.
      It fits the conversation at this week’s CoRL and Humanoid Safety Summit.
    

The training story is worth a line as well. DeepMind says Gemini 4 Argon generated thousands of simulated practice runs from real camera scenes, building digital twins of the workcell, and mixed them with real demonstrations. That is the sim-to-real problem in a new form: whatever the model learns in a virtual copy still has to survive real lighting, friction and part variation.

## What it doesn’t show yet

This is a proof-of-concept, not a commercial deployment. Neither DeepMind nor Mercedes has said how many Apollo units will work at either plant, or when. Ars Technica reports that the factory video is sped up 2× to look more human-like, and that the model runs on cloud servers rather than on the robot, so every gain in accuracy still carries network latency. Mercedes isn’t a neutral venue either: it has a commercial agreement with Apptronik and has invested in the company, alongside Google, which owns DeepMind.

And there’s nothing to buy. Gemini Robotics 2.5 is “rolling out to our robotics partners”, which Ars lists as Agile Robots, Apptronik and Boston Dynamics. Apptronik hasn’t published a price for Apollo, which is why it stays unscored on our Apollo page under our methodology.

## How we’d test a kitting humanoid

If we had an Apollo 2 at a kitting station for a week, the test would be dull on purpose, which is how factory tests should be. We’d run full shifts rather than clips, log every attempt with a written pass rule, count every human touch as an intervention, and time picks per hour next to a person at the same station. We’d rotate part variants in the middle of a shift, and pull the network cable once to see what the robot does when the cloud model goes quiet. That’s the same approach as our guide to testing a humanoid robot, applied to one station.

For the bigger picture of which carmakers are putting humanoids on their lines, see the carmakers’ humanoid race, and for how VLA models fit with the rest of a robot’s software, our humanoid AI systems explainer. Boston Dynamics is also on DeepMind’s partner list, so the next interesting result could come from Atlas (more on its CEO in our Robert Playter profile).

My read: Gemini Robotics 2.5 is the most convincing factory result a general-purpose robot model has published so far, and the 96% is a starting point, not a finish line. I’ll believe it’s ready for production when someone publishes the trial count and the interventions per shift next to it.

## Sources

1. Google DeepMind, Gemini Robotics 2.5 accelerates real-world general dexterity, 10 November 2026. 200+ tasks, 30–100% higher success and 20–30% faster than previous baselines, Apollo 2 with the 22-DOF SharpaWave hand, Mercedes-Benz kitting pilot in Kecskemét and Sindelfingen at 96%, Jeff Cardenas quote, 33%/43% safety gains, ASIMOV-Jailbreak, rollout to partners.
2. Ars Technica, DeepMind reveals Gemini Robotics 2.5 and open safety benchmark “ASIMOV-jailbreak”, 10 November 2026. Kanishka Rao and Carolina Parada quotes, cloud-based inference, video sped up 2×, partners Agile Robots, Apptronik and Boston Dynamics.
3. TechTimes, Gemini Robotics 2.5 Reports 96% Success Sorting Mercedes Car Parts but Publishes No Trial Count, 11 November 2026. Missing trial count and success definition, interval illustration, task-level gains, Mercedes investment in Apptronik.
4. The Decoder, Google DeepMind’s Gemini Robotics 2.5 scores 96 percent sorting metal parts in a Mercedes-Benz assembly pilot, 10 November 2026. Secondary report of the pilot.
