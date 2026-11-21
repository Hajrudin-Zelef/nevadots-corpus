---
id: collect-261121-veille-ia/gemini-google/gemini-robotics-2-5-reports-96-success-sorting-mercedes-car-parts-but-publishes-no-trial-count
title: "Gemini Robotics 2.5 Reports 96% Success Sorting Mercedes Car Parts but Publishes No Trial Count"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-10"]
keywords: ["gemini", "robotics", "deepmind", "mercedes", "success", "parts", "does", "robots"]
source: https://www.techtimes.com/articles/330432/20261111/gemini-robotics-25-reports-96-success-sorting-mercedes-car-parts-publishes-no-trial-count.htm
source_anchor: ""
source_lines: [1, 71]
section: "Gemini_Google"
---
Google DeepMind said on Tuesday, Nov. 10, 2026, that Apptronik's Apollo 2 humanoid robots running Gemini Robotics 2.5 reached a 96% success rate kitting automotive parts at Mercedes-Benz plants in Sindelfingen, Germany, and Kecskemét, Hungary. The announcement does not say how many trials that figure covers, how DeepMind defines a success, or how often a human stepped in, which decides whether 96% is a production-ready number or a demonstration statistic.

The missing denominator matters to anyone weighing humanoid robots for a factory line. A score of 24 successes in 25 attempts and 960 in 1,000 both round to 96%, yet a standard 95% Wilson score interval spans roughly 80.5% to 99.3% for the first and 94.6% to 97.0% for the second (a TechTimes calculation, offered as an illustration rather than reported data).

### DeepMind Reports 96% Kitting Success Without Naming Trials or Failure Criteria

DeepMind's post says Gemini Robotics 2.5 was evaluated across more than 200 tasks, with success rates 30% to 100% higher than "previous baselines" and tasks completed 20% to 30% faster. The text does not name the baseline, although the post's charts label the comparison model Gemini Robotics 2. It also does not say whether a task means one attempt or many, so no single percentage can be tied to a number of trials.

The largest individual gains DeepMind lists come from tasks the earlier model could barely do. A toolbox lid went from 0% to 93%, a rice cooker from 0% to 40%, pastries picked up with tongs from 5% to 100%, ribbon bows from 0% to 70%, a cardigan from 0% to 62%, and pants from 0% to 40%. Gains from a starting point of zero are real progress, but they also show that several tasks remain well below any production threshold.

Apptronik chief executive Jeff Cardenas said the models improved dramatically within a few weeks of post-training and carried over to the Mercedes plants with little drop-off. That is a claim about speed of adaptation, and it comes from a company that sells the robot. TechTimes found no independent assessment of the 96% figure as of this writing.

**Read more:**
**Gemini Robotics 2 Controls Full Humanoids: Legs, Torso, Arms, and Fingers Under One Policy**

### How Does Gemini Robotics 2.5 Kit Parts for Mercedes Vehicles?

DeepMind says the robots sort individual parts from bulk bins into trays or sequencing containers in the order each vehicle needs. That is a step in just-in-sequence manufacturing, which means delivering components in exact order to the line. A mistake here is costly because sequence errors cause line stops, so the task arguably rewards consistency more than speed.

Gemini Robotics 2.5 belongs to the vision-language-action (VLA) family, models that take camera images and an instruction and output robot motions, a lineage that runs from Google's RT-2 in July 2023. The Gemini Robotics 1.5 paper described a two-part design in which an embodied-reasoning model plans steps and an action model that thinks before acting executes them. In the Mercedes pilot, DeepMind says the reasoning model tracked the workflow while Gemini Robotics 2.5 controlled the arms and hands, but the post does not publish an architecture for this release.

The engineering economics DeepMind emphasizes are data and simulation. The post says Gemini 4 Argon can generate thousands of varied simulated practice runs from a real-world camera scene and build a digital twin, a virtual copy of a workcell used to train and test behavior before touching hardware. DeepMind does not explain the mechanism, and the tradeoff is the familiar sim-to-real gap: every behavior learned virtually has to survive lighting, friction and part variation on a real line.

### Mercedes Sindelfingen Cut Robots in 2016 Over Variant Complexity: Kitting Differs

The Sindelfingen plant is an unusual place to claim a humanoid milestone. In 2016, Markus Schaefer said robots couldn't cope with the individualization and number of variants in Mercedes vehicles, and the company moved some work back to human workers. The plant produces about 400,000 cars annually, according to The Register's 2016 report.

That history is context, not a contradiction. The 2016 robots were assembly-line machines, while the 2026 pilot is kitting, a different task with different failure modes. What the contrast does show is that variant complexity, the factor Mercedes cited, is exactly what a 96% score cannot describe without knowing how many part types and sequences were tested. DeepMind's post does not give that count.

Mercedes is also not a neutral test site. It signed a commercial agreement in March 2024, announced an investment in March 2025, and joined Google and B Capital as a repeat investor in Apptronik's $520 million Series A-X extension in February 2026. DeepMind, Apptronik and Mercedes all benefit from a strong result. TechTimes found no Mercedes-side statement confirming the 96% figure.

### What 96% Means on a Production Line: Errors Compound Across Steps

Compounding shows why the gap matters. If a kit requires 10 successful picks and each succeeds 96% of the time independently, a full kit succeeds about 66.5% of the time, and at 20 picks about 44.2% (a TechTimes calculation; real failures are rarely independent). DeepMind's 96% may describe tasks rather than individual picks, which is another definition the post leaves open.

TechTimes reported in June that a 95% per-step success rate leaves a 10-step task succeeding only about 60% of the time. Venture firm Bessemer said in April 2026 that production needs about 99.9% reliability. Neither figure is a verdict on the Mercedes pilot, but both show why readers should treat a headline percentage as the start of the analysis rather than the end.

**Read more:**
**Robotics VC Breaks Annual Records at Midyear: Why Physical AI Commands Software Multiples**

### Safety Gains Come From DeepMind's Own Benchmarks, With No US Humanoid Rules

DeepMind says inference-time reasoning guidance raised safe task completion by 33% around obstacles and 43% around nearby people. It also says Gemini 4 Argon scored 99.5% on the open-source ASIMOV-Jailbreak benchmark, against 82% for Gemini Robotics ER 2. These are developer-reported results, and the post does not name an outside evaluator.

The regulatory backdrop is thin. A Robotics 24/7 report says OSHA has no robotics-specific standards, which leaves humanoids without a dedicated federal rule, and ISO 25785-1 is still under development as a safety standard for dynamically stable walking robots.

### Gemini Robotics 2.5 Has No Public Availability Date or Pricing

DeepMind says Gemini Robotics 2.5 is rolling out to robotics partners and thanks Agile, Apptronik and Mercedes-Benz in its acknowledgements, but it gives no general availability date or pricing. The post describes testing on Apollo 2 with its five-finger SharpaWave hand and on Franka Duo dual-arm systems with two-finger parallel grippers.

Comparisons with earlier results require caution. When DeepMind released Gemini Robotics 2 on July 30, 2026, TechTimes reported on July 30 whole-body Apollo 2 figures ranging from 45.7% for floor pickups to 76.3% for shelf pickups. Those were different tasks measured with a different hand, so they do not show progress toward the 96% kitting score. Cardenas said Apptronik built Apollo for this kind of industrial work, and its Robot Park facility in Austin, announced on June 30, 2026, collects the data that feeds that effort.

The buyer's checklist is short. Ask for the number of trials, the written definition of a successful kit, the rate of human interventions per shift, and the count of part variants, and treat any figure that arrives without them as a demonstration result.

## Frequently Asked Questions

### What does a 96% success rate mean if no trial count is published?

It cannot be interpreted precisely. The same 96% could reflect 25 attempts or 1,000, and the plausible range differs sharply between them (roughly 80.5% to 99.3% versus 94.6% to 97.0%, a TechTimes illustration using a Wilson score interval). DeepMind's post does not define success or report human interventions.

### Is Gemini Robotics 2.5 available to buy or test?

DeepMind says it is rolling out to robotics partners and has not given a general availability date or pricing.

### Why did Mercedes remove robots from Sindelfingen in 2016, and does this change that?

Mercedes said robots struggled with the variety of vehicle configurations, and Engadget reported that robots would keep working alongside human workers. The 2026 pilot is a kitting task rather than assembly, so it does not directly test that problem, and no variant count has been disclosed.

### Are there US safety rules for humanoid robots on factory floors?

A Robotics 24/7 report says OSHA has no robotics-specific standards, and the ISO 25785-1 standard remains under development.
