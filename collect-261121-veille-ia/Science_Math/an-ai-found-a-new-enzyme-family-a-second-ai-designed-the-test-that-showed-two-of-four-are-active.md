---
id: collect-261121-veille-ia/science-math/an-ai-found-a-new-enzyme-family-a-second-ai-designed-the-test-that-showed-two-of-four-are-active
title: "An AI found a new enzyme family. A second AI designed the test that showed two of four are active"
domain: science-math
role: reference
task: article
actors: []
dates: ["2026-09-23"]
keywords: ["enzymes", "active", "enzyme", "test", "anthropic", "second", "designed", "them"]
source: https://maccus.fr/en/ai-enzyme-lab-validation/
source_anchor: ""
source_lines: [1, 77]
section: "Science_Math"
---
In September 2026, a swarm of Claude agents, Anthropic’s AI models, combed through microbial genomes and flagged a family of enzymes that no one had described. Anthropic said plainly that it did not yet know what the enzymes did.

On November 17, 2026, the biotech company Ginkgo Cloud Labs reported an answer to part of that question. A second system, Claude Opus 5, designed the laboratory experiments. A robotic lab ran them, and two of the four candidates turned out to be active. The work took 13 days from design to data and about $14,000 of cloud-lab services.

The enzymes are reverse transcriptases, proteins that copy RNA back into DNA. Bacteria use several of them to defend themselves against the viruses that infect them, called phages. The new ones sit next to a stretch of repeating genetic text, so Anthropic named them array-associated reverse transcriptases, or ART.

The two reports are worth reading together for the loop they close. Software found the enzymes, Anthropic said it did not know what they were for, and then software designed the experiment that began to answer that question.

## A discovery that came without a function

The first half happened on September 23, 2026, when Anthropic published an account of what it called a scientific discovery by Claude. Roughly 950 agents ran for 21 hours and used 210 million tokens, the units of text an AI reads and writes. They sorted through more than 200,000 reverse transcriptases, picked 3,500 candidate systems, and narrowed those to the 20 most compelling. In that set they flagged the repeat-array pattern that became ART.

The underlying science went into a preprint, a paper shared before peer review, by Peter Yoon, Januka Athukoralage, Emmanuel Ameisen and colleagues. They posted it on bioRxiv, an online repository for biology papers, on October 11, 2026.

One sentence in that announcement is the kind most companies would have buried. “Although we don’t yet know its function,” Anthropic wrote, before listing what the system looked like it might do. The preprint was just as careful.

It reported that one array produced short RNA pieces, but said the authors had “not shown that the RT is active or that the unit RNAs are its substrates.” In plain terms, the authors had not proven the enzyme does any chemistry, or that those RNA pieces are what it acts on. A pattern had been found. Whether it did anything was still open.

## The question handed to a second model

That open question is what Ginkgo Cloud Labs took on. Ginkgo runs a cloud lab, a remote facility where robots carry out experiments ordered online. The company connected Claude Opus 5 to a software harness that lets the model act on its own. It gave the model the Yoon preprint and a catalog of tested lab procedures the robots can run, then asked it to write work orders the lab could execute.

The model broke the preprint into numbered claims, listed the gaps and open questions, and built the ones still worth testing into an ordered set of experiments matched to the lab’s catalog. It specified the DNA for the enzymes and for deliberately broken versions of them, set the conditions for each test, and sent off the orders.

The hypothesis was narrow and concrete: that the ART enzymes copy the RNA their own repeat arrays produce. To test it, the design built each enzyme two ways, one active and one deliberately disabled, and measured each with a DNA assay, a lab test that detects newly made DNA.

Marc Winer’s earlier reporting covered a related pattern in physics, where AI agents wrote quantum programs that were then proved correct in a formal proof checker. There the checker was software. Here it is a physical lab.

## What the robot measured

The readout came from quantitative PCR, or qPCR, a standard method that measures how much DNA a sample contains. The reactions included no added primer, the short starter strand that normally begins DNA copying. Without it, any DNA that appeared had to come from the enzyme itself, working on its own RNA.

Two of the four enzymes, labeled SA1 and LY01, produced far more DNA than their inactivated copies did, and far more than a control enzyme run on sequences that were not its own. The other two, MarsHill and PB50, barely cleared those controls, so Ginkgo calls them inconclusive rather than negative.

The company also found what the active enzymes need in order to work. Their activity depended on two specific amino acids in the enzyme’s core and on an unusually long section at the front of the protein. A separate test published on November 16 by two researchers found that the newest Claude models can predict the next amino acid of little-studied proteins.

Changing those amino acids, or removing the front section, dropped every version to the inactive baseline. SA1 and LY01 lost the activity they had shown. That is the kind of control that separates a real signal from background noise: the enzymes that worked stopped working once the parts that should matter were removed.

## The limits of the result

The vendors’ framing deserves a second read, because both have something to sell. Ginkgo sells the cloud-lab services that produced the $14,000 figure, and Anthropic is a commercial AI company in the middle of a fundraising year. The measurable claims are modest and worth stating exactly.

Activity was shown for two enzymes. The biological job of the system, whether it actually protects bacteria from phages inside a living cell, was not tested at all.

The robot did not work alone. Human researchers reviewed every design, checked the DNA sequences, and chose to make the inactivated controls. The first attempt to confirm the proteins were even present returned almost nothing. The researchers then added a second, more sensitive measurement, which showed the proteins were there and that the earlier assay had been the problem.

Ginkgo is direct that running the experiments “still relied on tight human-in-the-loop oversight,” with people checking the work at each step. Several of the original questions remain open: whether SA1 and LY01 are specific to their own sequences, whether MarsHill and PB50 need different conditions, and what the partner proteins beside each enzyme do, since these tests left them out.

## Two more results the same week

The ART work arrived in a week full of similar results. On the same day, Epoch AI, a group that evaluates AI systems, reported that OpenAI’s GPT-6.2 Astra solved 7 of 68 mathematics problems still open as of August 2026, up from a previous best of three. Each solution was checked by Lean, software that verifies a mathematical proof step by step, and cost about $240 per problem. One of the seven had no known solution before.

At SC26, a supercomputing conference, NVIDIA said AI science agents are now running on researchers’ own computers. A repair agent built at KTH, a university in Sweden, fixed 81% of broken molecular simulations in a single overnight run on one desktop machine. An agent at Argonne National Laboratory, a US research lab, found two new binding sites, the spots on a molecule where another molecule can attach, from a single prompt. Those NVIDIA results come from a company keynote, so they are claims rather than checked results, but the direction is consistent.

Marc Winer has tracked this broader shift, including a case where AI agents did real engineering work that was checked against a real system and a mathematics proof reviewed by a human expert panel.

## What it means

The attention will go to the discovery, but the number that matters is the validation cost. The slow and expensive half of biology is testing a hypothesis at the lab bench, long after the idea itself. That is the half this run made faster and cheaper.

Thirteen days and about $14,000 took a question from a preprint to a first biochemical answer. Because Ginkgo sells those steps as standard services, the $14,000 is a listed price rather than a loose estimate. For a small lab or a founder, that means a specific, testable experiment can now be scoped and bought, where it once needed a funded group and a year.

The software shortened the path from an open question to a checkable experiment. It did not decide which questions are worth asking.

Humans picked the target, reviewed the designs, and rescued a failed measurement mid-run. The result arrived in two weeks. The judgment of which results matter stayed with people.

The questions worth watching are concrete: whether the two active enzymes are specific to their own arrays, whether the partner proteins do anything, whether the system defends a living cell against phages, and whether reading the DNA the enzymes made confirms the signal. If the same found-it-then-tested-it loop repeats on a harder target, the pattern stops being a one-off and starts being a method.

## Sources

- ART proteins are active on their cognate arrays, Ginkgo Cloud Labs, November 17, 2026
- Claude discovers a new enzyme system, Anthropic, September 23, 2026
- Autonomous AI agents discover reverse transcriptases with tandem repeat arrays, Yoon et al., bioRxiv, October 11, 2026
- GPT-6.2 Astra sets a record on FrontierMath Erdos, Epoch AI, November 17, 2026
- Ian Buck kicks off NVIDIA’s week at SC26 as science enters the age of agents, NVIDIA, November 17, 2026

Header image: chart by MACCUS, data from Ginkgo Cloud Labs’ ART assay data (ART_three_assays_long.csv), read November 18, 2026
