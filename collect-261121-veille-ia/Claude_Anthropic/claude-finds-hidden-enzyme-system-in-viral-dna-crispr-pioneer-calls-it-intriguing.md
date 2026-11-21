---
id: collect-261121-veille-ia/claude-anthropic/claude-finds-hidden-enzyme-system-in-viral-dna-crispr-pioneer-calls-it-intriguing
title: "Claude Finds Hidden Enzyme System in Viral DNA: CRISPR Pioneer Calls It Intriguing"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["system", "enzyme", "crispr", "claude", "array", "intriguing", "anthropic", "reverse"]
source: https://www.techtimes.com/articles/327986/20260924/claude-finds-hidden-enzyme-system-viral-dna-crispr-pioneer-calls-it-intriguing.htm
source_anchor: ""
source_lines: [1, 71]
section: "Claude_Anthropic"
---
A swarm of Claude Mythos 5 agents working without human guidance spent 21.5 hours searching a database of 1.9 billion protein clusters and surfaced something biology's own experts had not seen before: a previously uncharacterized enzyme system tucked inside bacteriophage DNA, flanked by a long, evenly spaced run of repeating genetic sequences that looks architecturally like a CRISPR array. Anthropic's novel enzyme system announcement describes how wet-lab scientists in the Bay Area confirmed the system is real. The company named it array-associated reverse transcriptases, or ART, published a preprint on September 23, 2026, and the same day Anthropic CEO Dario Amodei publicly called it "preliminary" in a social media post, suggesting it "may constitute a new gene editing mechanism that could have applications in gene therapy."

Feng Zhang — whose laboratory at the Broad Institute of MIT and Harvard was among the first to demonstrate CRISPR-based genome editing in mammalian cells — reviewed the preprint and offered a response worth reading carefully. Feng Zhang's full endorsement reads: "This is an exciting example of how AI agents can contribute to biological discovery. The identification of RNA-repeat arrays associated with reverse transcriptases is genuinely intriguing and merits further investigation. I hope this work encourages more scientists to explore how AI can support their research." Zhang called the architecture intriguing and said it warrants investigation. He did not say ART edits genes, cuts DNA, or will become the next CRISPR. That distinction matters, and this article will hold it.

What makes the announcement significant — and what separates it from every prior TechTimes story about Claude in biology — is not what ART does (still unknown) but what the campaign demonstrated: an AI agent, given only a high-level objective, noticed something unusual in raw sequence data, recognized it as previously undescribed, investigated its structure autonomously, and escalated it for human review. Prior coverage in this publication has documented Claude designing protein binders at twice the industry hit rate and improving a mathematical bound in number theory. Those were instances of AI excelling at known tasks. ART is different: nobody had defined the task of finding ART, because nobody knew ART existed.

### What ART Is and Why the Architecture Matters

To understand what made ART worth filing a report over, it helps to understand what made CRISPR worth pursuing — because the analogy is the entire scientific argument.

CRISPR is not primarily an enzyme. It is a system: a modular arrangement of genetic components that store information in a repeating array, transcribe that array into short RNA guides, and use those guides to direct enzymatic machinery to a specific target. That programmability — the ability to re-target the system by swapping one guide for another — is what allowed CRISPR-Cas9 to become a biotechnology tool after decades as a bacterial curiosity. The Nobel Prize in Chemistry 2020 was awarded to Jennifer Doudna and Emmanuelle Charpentier for recognizing and engineering that programmability.

ART has three parts: a reverse transcriptase enzyme, a neighboring partner gene whose function is unknown, and a long array of evenly spaced DNA repeat sequences. That three-part arrangement — enzyme, partner, array — is structurally similar to the architecture of CRISPR and a handful of other systems. According to the ART preprint, Anthropic's initial wet-lab experiments showed that the ART array transcribes into a set of distinct short RNAs — up to 8 percent of total phage RNA at 15 minutes post-infection in publicly available data from a Staphylococcus phage. That is not proof of programmability, but it is the fingerprint that systems with programmable architectures tend to leave. According to Anthropic, every other known programmable system sharing this same set of characteristics performs operations such as cutting, copying, or pasting DNA.

The jumbo phage reverse transcriptase underlying ART had been spotted before in one of the unusually large-genome viruses that have become an active area of phage biology research. What Claude appears to be the first to notice was the pair of defining features that turn a lone enzyme into a system: the repeat array alongside it, and the accessory protein of unknown function.

Reverse transcriptases are enzymes that copy RNA back into DNA — running genetic information in the direction opposite to the standard flow described by the central dogma of molecular biology. Howard Temin and David Baltimore discovered them in RNA tumor viruses in 1970, work that earned them (along with Renato Dulbecco) the 1975 Nobel Prize for Medicine. Since then, prokaryotic reverse transcriptases have been found in retrons, diversity-generating retroelements, and systems associated with CRISPR-Cas — and researchers have increasingly recognized them as components of microbial immune defense. ART appears to be a new member of that growing family, found in the viruses rather than the bacteria — an unusual location that adds a dimension to the phage-bacterium arms race that biologists are still mapping.

### How Agents Find What Genome Miners Miss

Traditional genome mining — the systematic search of DNA databases for uncharacterized genes — has been a standard tool in molecular biology for two decades. Its limiting factor has never been compute; it has been expert time. According to Anthropic, expert weeks-to-months comparison represents the realistic human timeline for the scale of analysis the campaign ran.

What 949 Claude Mythos 5 agents did in 21.5 hours was every layer of that process at once, without human hands between steps. The campaign was structured around a paired-agent architecture: one agent planned and executed each analytical task while a second reviewed it, and agents opened new sub-tasks as the investigation proceeded. The Next Web on ART figures confirmed: agents gathered more than 200,000 RT sequences from the 1.9 billion protein clusters in the database, scored 3,564 candidate partner families, and filed 19 human-readable reports, each proposing a function and the evidence for it. The 215.6 million tokens consumed across the campaign is a measure of the scale of reading, not a proxy for the quality of judgment.

ART came not from the main investigative thread but from a side path. According to Anthropic, the agent's raw sequence exclamation documented the discovery moment: "[The DNA next to the RT] is spectacular: I can see by eye a tandem repeat array … that's a CRISPR-like … repeat array?!" It then proceeded as a human biologist would: it counted the repeats, measured their spacing, compared the layout against known RT systems in the literature, and searched for any prior description of the pattern. Finding none, it filed a report. Anthropic's scientists reviewed the report, expressed the proteins in standard laboratory strains, and confirmed through biochemical and structural characterization that ART was a previously undescribed biological system.

The entire computational phase — from broad database search to a prioritized shortlist — used Claude Science and Claude Code, the same tools available to any researcher with a paid Anthropic account. The scale, not the tool access, is what changed.

### What Claude Cannot Guarantee: The Reproducibility Problem

The finding carries a material constraint that Anthropic has been transparent about in the preprint, and that belongs in any honest account of what this campaign demonstrated.

Ten separate reruns of the same campaign all missed the array. Cryptopolitan confirmed the ten-rerun failure, reporting that no rerun read the DNA upstream of the enzyme. The ART discovery depended on a specific chain of agent decisions made during a single run — decisions that did not recur when the same high-level prompt was issued again. The discovery was not a reliable output of the methodology; it was an outcome that the methodology made possible on one occasion.

This is not a minor footnote. Reproducibility is the standard by which scientific methodology is evaluated. A human expert who ran a genome mining campaign that found a genuinely novel system but could not find it again on repeated attempts would face the same question: was this a signal, or was it noise that happened to be confirmed by subsequent wet-lab work? In ART's case, the wet-lab confirmation is the answer — the system is real, the array transcribes into short RNAs, the structural characterization holds. But the route to that confirmation was not systematic; it was contingent on a single agent taking an unusual side path that the other 948 agents, across ten reruns, did not take. The preprint's reproducibility section notes that when given the DNA directly, the best models described the array in a minimum of 90 percent of attempts — but with the addition of tools and files, the rate sank to as low as 32 percent.

For researchers evaluating agentic AI genome mining as a tool for their own labs, this is the constraint question that matters most. The pipeline does not guarantee discovery — it makes discovery possible at a scale and speed that human teams cannot match. What distinguishes the proposals worth testing from those that should be set aside remains, by Anthropic's own account, an active question the team is investigating.

### What Anthropic's Scientists Bring to the Pipeline

Anthropic formed the life sciences research group in spring 2026 with a team of scientists who, before joining, had contributed to the understanding of CRISPR system evolution and regulation, the discovery of enzymes for next-generation gene therapies, and the development of computational tools for identifying anomalies in DNA sequences. The team's full background is described in the announcement. The lab, located in the Bay Area, operates at biosafety levels BSL-1 and BSL-2 — the lowest tiers of biological risk — and handles no pathogens that can infect humans.

The workflow Anthropic describes is structured to keep human judgment where it is most valuable: in designing the initial research objective, in reviewing the candidates that survive AI filtering, and in running the laboratory work that confirms or refutes the AI's proposals. Claude agents handle the layers in between — searching the database, generating hypotheses, cross-referencing the literature, writing reports. Anthropic's scientists then take the surviving candidate into the lab.

The group sits within Anthropic's wider life sciences organization, which includes drug discovery teams and the team training Claude in biology and chemistry. A companion development announced simultaneously: the Life Sciences Verification Program, which gives life sciences professionals access to Claude Mythos, Opus, and Sonnet models with biology-specific safeguards. The program is the successor to the government-partnership access architecture this publication reported in August.

### What Comes Next, and What CRISPR's History Suggests

CRISPR was noticed in 1987 as unusual repeat sequences in bacterial DNA. It took until 2002 for the field to establish that these repeats represented a defense system against viruses. Another decade passed before Doudna, Charpentier, Zhang, and others turned the defense mechanism into a programmable editing tool, and another ten years before CRISPR-based therapeutics reached patients. The path from "unusual sequence" to "useful medicine" in that case ran more than three decades.

ART is at step one. The array is real. The short RNAs are confirmed. The enzyme's activity and target have not been demonstrated. Whether the system can be re-targeted — whether it is, in fact, programmable — is the question that will determine its eventual significance. Anthropic has released the preprint specifically to accelerate external input on those questions. Anthropic's collaboration invitation is open to scientists with relevant proposals.

Dario Amodei, in his September 23 social media remarks, described the finding as "preliminary" while suggesting it may constitute a new gene editing mechanism — a framing that accurately holds both the early-stage reality and the possibility that makes the discovery worth reporting. What the 21.5 hours of agent work produced is not a new gene therapy. It produced a question that human biology did not know to ask — a question about what is happening inside bacteriophage DNA in a corner of the sequence space that genome miners had passed through without stopping. That is what the campaign's 215.6 million tokens of autonomous reading actually achieved: not the answer, but the question itself.

## Frequently Asked Questions

### What is ART, and what does it actually do?

ART stands for array-associated reverse transcriptases. It is a three-part biological system found mainly in bacteriophages — the viruses that infect bacteria — consisting of a reverse transcriptase enzyme, a neighboring partner gene of unknown function, and a long array of evenly spaced DNA repeat sequences. Anthropic's initial experiments confirmed that the array is transcribed into distinct short RNA molecules. What those RNAs do, whether the reverse transcriptase is active, and what the overall system's function is in the phage's biology remain entirely unknown. The function question is what current experiments are designed to answer. Full technical details are in the Anthropic ART preprint.

### Is ART the same as CRISPR, or a replacement for it?

No. ART's repeat array resembles the repeat array in CRISPR systems architecturally — both store a library of short sequences that are transcribed into RNA — but CRISPR's function, mechanism, programmability, and use as a gene-editing tool were all established over decades of subsequent research. ART has none of that established picture yet. Feng Zhang's Anthropic blog comment, from one of the scientists who helped make CRISPR a medical tool, called the RNA-repeat array finding "genuinely intriguing" and said it "merits further investigation" — not that it is a gene-editing system. The structural similarity is the reason researchers think ART is worth investigating, not evidence that it can do what CRISPR does.

### How did AI agents find something human genome miners missed?

Genome mining — searching DNA databases for uncharacterized genes — is standard in molecular biology, but it is limited by expert time. A human team searching 200,000 reverse transcriptases, cross-referencing 3,564 candidate partner families, and writing 19 detailed reports would spend weeks to months. Claude Mythos 5 agents did it in 21.5 hours across 949 sessions. ART came from a side path: one agent, reading raw DNA adjacent to an unusual enzyme, noticed a repeat pattern by inspection, counted the repeats, compared them against known systems, searched the literature for prior descriptions, and filed a report for human review — the same sequence of steps a trained biologist would follow, compressed into a fraction of the time. The Anthropic blog's agent discovery account describes the process in detail.

### If the same campaign was run again, would it find ART again?

Not reliably. The preprint's reproducibility finding notes that ten reruns of the same campaign all missed the array. The discovery depended on a specific chain of agent decisions in a single run. The wet-lab confirmation is what makes ART real; the single-run nature of the computational discovery is the methodology's documented limitation. Cryptopolitan's ten-rerun report notes that when given the DNA directly, the best models described the array in at least 90 percent of attempts — but with tools and files added, the rate fell to as low as 32 percent. Researchers considering AI genome mining for their own work should treat the campaign architecture as a tool that makes unexpected discoveries possible, not one that makes them certain.
