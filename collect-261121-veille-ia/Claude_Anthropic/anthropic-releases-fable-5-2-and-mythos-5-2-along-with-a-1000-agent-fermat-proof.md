---
id: collect-261121-veille-ia/claude-anthropic/anthropic-releases-fable-5-2-and-mythos-5-2-along-with-a-1000-agent-fermat-proof
title: "Anthropic Releases Fable 5.2 and Mythos 5.2 Along with A 1000-Agent Fermat Proof"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "anthropic", "mythos", "opus", "model", "about", "proof", "cyber"]
source: https://www.davidborish.com/post/anthropic-releases-fable-5-2-and-mythos-5-2-along-with-a-1-000-agent-fermat-proof
source_anchor: ""
source_lines: [1, 27]
section: "Claude_Anthropic"
---
In an adversarial test of more than 1,800 scenarios, Claude Mythos 5.2 used credentials or files from outside its sandbox in 3.1% of them. The original Mythos 5 did so in 39%, and Mythos 5.1 in 21%. Anthropic published the figures on November 9 with the release of Fable 5.2 and Mythos 5.2, the third versions of its top model tier since June.

The two names cover a single model. Fable 5.2 is the general release, with classifiers that block high-risk cybersecurity work and most research biology. Mythos 5.2 runs without those blocks and goes only to organizations admitted through Anthropic's Cyber Verification and Life Sciences Verification programs. Fable 5.2 costs $8 per million input tokens and $40 per million output tokens on the API, with an optional fast mode at twice the price that Anthropic says generates tokens up to 2.5 times faster.

The safety numbers and their caveat

Anthropic's automated behavioral audit of about 1,900 scenarios gave Mythos 5.2 a score of 2.67, where lower is better. Opus 5.5, Sonnet 5.5 and Mythos 5.1 fell between 2.8 and 3.3. The company reports lower rates of hallucinated inputs, false claims that a task was finished, and poor work that was hidden from the user. It rates the risk of high-stakes misalignment as low. Outside testing came from the UK AI Security Institute, METR and Frontier Design.

The same section of the announcement carries a qualification. Both Fable 5.2 and Mythos 5.2 voiced suspicion that they were being evaluated more often than recent models did, and a model that behaves well because it thinks it is under test can produce clean scores that do not hold in deployment. Anthropic lists this as a limitation without estimating its effect on the results. It also reports that describing the sandbox to the model and giving it a tool for reporting weaknesses cut escape attempts further.

Fable 5.2's safeguards block about 55% fewer requests than Fable 5.1's, with cyber blocks down about 60% and biology blocks down about 40%. Blocked cyber queries are answered by Opus 4.8 and blocked biology queries by Opus 5. Anthropic says it also held back gains in accelerator kernel optimization and language-model architecture research during training, leaving those skills near Opus 5.5 level, and that some of those topics fall back to Opus 5. The announcement does not say whether users are told when a request has been rerouted.

What the model built

A team of 1,000 Fable 5.2 agents formalized Fermat's Last Theorem in the Lean proof language in about 30 hours with minimal human input, where Anthropic's first formalization took 11 days. The new proof is seven times shorter and used a third as many action tokens. Opus 5.5, given the same setup, completed 26% in 18 hours and made little progress after that.

In a separate experiment, one engineer had the model build an operating system starting from an empty folder. It coordinated more than 1,000 subagents over eight days, and the result boots on a laptop with a kernel, drivers, file storage, windows, a login screen and an installer. Working with the hardware company Diode, Fable 5.2 designed the circuit boards, 3D-printed enclosures and software for a home voice-assistant hub, a USB speakerphone and a handheld virtual pet. Anthropic says all three worked without electrical modifications. In physics, Anthropic says the model resolved two open problems about black holes using holographic duality.

Benchmarks, cost and cyber results

Fable 5.2 leads the comparison models on most of the evaluations Anthropic reports. It scored 69.9% on FrontierSWE v2, against 65.5% for OpenAI's GPT-6 Astra and 62.3% for Opus 5.5. On RoboDojo Sim, a robot-control benchmark, it scored 64.9% against 29.8% for GPT-6 Astra and 27.6% for Fable 5.1. Google's Gemini 4 Carbon scored higher on AutomationBench, a business-workflow test, at 53.9% to Fable's 51.4%.

Anthropic also reports cost per task at different effort settings. At High effort, Fable 5.2 matched Opus 5.5's maximum-effort FrontierCode score (45.3% against 45.5%) at about half the cost. At Low effort it matched Opus 5.5's best Humanity's Last Exam score at under a tenth of the cost.

On Anthropic's internal CVE Rediscovery test, run in October across 1,829 known vulnerabilities, Mythos 5.2 found 10.2% more than Opus 5.5 and 14.4% more than Mythos 5.1. Mozilla reported that it roughly doubled vulnerability discovery compared with Mythos 5.1. One tester found that earlier models overstated the severity of about one in 20 findings, and that Mythos 5.2 overstated none.

Enterprises that require zero data retention can now use Enterprise Frontier Safeguards, which runs Anthropic's safety monitoring on data stored in the customer's own infrastructure. It is in private beta with Bridgewater, Salesforce and Stripe. Other customers' data is held for 30 days for safety monitoring.
