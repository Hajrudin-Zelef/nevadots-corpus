---
id: collect-261121-veille-ia/claude-anthropic/claude-discovers-a-novel-enzyme-system-with-crispr-like-repeats
title: "Claude discovers a novel enzyme system with CRISPR-like repeats"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["system", "claude", "enzyme", "crispr-like", "repeats", "anthropic", "laboratory", "agents"]
source: https://www.techbytes.app/posts/claude-discovers-a-novel-enzyme-system-with-crispr-like-repeats/
source_anchor: ""
source_lines: [1, 85]
section: "Claude_Anthropic"
---
# Claude discovers a novel enzyme system with CRISPR-like repeats

Anthropic deployed 950 Claude agents across 210 million tokens to discover ART, an uncharacterized enzyme system featuring CRISPR-like repeats in phages.

By Dillip Chowdary â¢ Nov 15, 2026 â¢ Source: Anthropic Research

In this post

Anthropic introduced a dedicated life sciences research group and wet laboratory alongside early findings in which its AI model identified an uncharacterized biological mechanism in bacteriophage DNA, according to Anthropic Research's report. The discovery centers on an enzyme system that combines a reverse transcriptase with tandem DNA repeats reminiscent of CRISPR architecture, identified through large-scale sequence analysis guided only by high-level prompts.

This report examines how computational agent fleets surveyed genomic databases, isolated candidate proteins, and transitioned computational predictions to laboratory benches for verification. The breakdown details the operational workflow, screening metrics, and biochemical implications for molecular biologists, biotechnology researchers, and machine learning teams tracking autonomous scientific discovery.

## 01Bay Area laboratory and wet lab research

Anthropic formed the life sciences research team in the spring of 2026 to evaluate whether general AI models can systematize molecular biology discoveries. The organization established a physical laboratory in the Bay Area operating at biosafety levels BSL-1 and BSL-2 to test machine-generated hypotheses. The wet facility avoids handling human pathogens and assigns all experimental executions directly to human laboratory scientists rather than physical robotics.

The scientific team previously studied unusual proteins, CRISPR system evolution, and genomic anomaly detection before joining the laboratory alongside Anthropic drug discovery and model training units. Early trials with automated physical execution frameworks like the Model Hardware Standard proved less practical for ad hoc molecular biology experiments. As a result, the initiative relies on human specialists to synthesize proteins and execute biochemical characterization.

Human researchers provide the starting direction and run the wet experiments, while fleets of AI agents read raw sequence data and evaluate protein families. The goal is to replicate the historical process that turned bacterial defense enzymes into molecular tools like restriction endonucleases and Taq polymerase. The team is studying the hypothesis generation process itself to teach models how to prioritize biologically viable candidates.

## 02Agent search across two hundred thousand enzymes

The discovery campaign focused on reverse transcriptases, enzymes that copy RNA into DNA, which frequently function within bacterial immune defenses. Researchers initiated the project by supplying an agent harness with a high-level prompt directing Claude to search massive DNA databases for uncharacterized variants. A cluster of roughly 950 agents spent 21 hours combing through public genomic records while consuming 210 million tokens.

The autonomous agents surveyed more than 200,000 reverse transcriptase sequences across diverse microbial datasets. Operating without step-by-step human intervention, the models grouped related proteins into distinct families and assessed whether specific sequences possessed unusual characteristics. The agents identified 3,500 new candidate systems that lacked descriptions in the existing scientific literature.

During the genomic sequence scan, one agent examined the raw DNA surrounding an unusual reverse transcriptase gene in a jumbo phage. Combing through neighboring non-coding regions, the agent flagged an adjacent array of evenly spaced sequences and recorded: "[The DNA next to the RT] is spectacular: I can see by eye a tandem repeat array â¦ that's a CRISPR-like â¦ repeat array?!". The observation marked the first recorded recognition of the architecture.

Advertisement

Tech Pulse Daily

### Get tomorrow's pulse first

Join engineers who read Tech Pulse before stand-up. Free, weekday mornings.

## 03Candidate screening through Claude Science and Code

The research group uses standard computational environments including Claude Science and Claude Code to coordinate analysis workflows. The software workflow directs agents to read relevant academic papers and reproduce established findings from public datasets to verify analytical methods before exploring unmapped territory. Agents then inspect the genomic neighborhood of candidate proteins to identify flanking genes or sequence anomalies.

Each agent compiles a structured, human-readable report for every prospective discovery, detailing the proposed biochemical function alongside supporting genomic evidence. In follow-up screening passes, the models critically review their own hypotheses, filtering out the vast majority of false leads and dubious predictions. This pruning process narrowed the initial pool of 3,500 candidates down to 20 prioritized systems.

The surviving reports undergo human evaluation, where scientists assess whether the proposed biochemical mechanism warrants experimental validation. With thousands of proposals generated per campaign, the researchers analyze why certain candidates succeed while others fail. These evaluations are reintroduced into prompt instructions to help the models mirror the scientific discernment of experienced molecular biologists.

## 04Reverse transcriptase with tandem repeat arrays

The system identified by the model has been designated array-associated reverse transcriptase, or ART, and occurs predominantly within bacteriophages. While earlier studies had documented the underlying reverse transcriptase protein itself in a jumbo phage, prior analyses had overlooked the associated genomic architecture. The system incorporates three distinct components: the reverse transcriptase, an adjacent accessory protein of unknown function, and a tandem repeat array.

The repeat region consists of evenly spaced, non-coding DNA sequences arranged in a pattern resembling CRISPR arrays. In classical CRISPR systems, repeat arrays store short guide sequences that direct Cas endonuclease machinery to cut matching nucleic acid targets. The presence of a repeating array beside an RNA-copying enzyme suggests that the non-coding sequences could guide or regulate the enzyme complex.

To confirm the finding, the agent counted individual repeat units, measured spacing distances between repeats, and cross-referenced the layout against all documented reverse transcriptase families. After searching sequence repositories to confirm that the configuration had never been described, the system prepared a summary for wet lab review. Anthropic subsequently released a pre-print detailing the genomic structure and sequence coordinates.

## 05Experimental validation and biological tool potential

Following the computational identification of the ART locus, Anthropic scientists synthesized the components and expressed the proteins in standard laboratory host strains. The physical experiments confirmed the presence of the expressed proteins and verified the structural arrangement predicted by the computational survey. While functional characterization of the system remains ongoing, the configuration mirrors an elite group of programmable molecular machines.

Feng Zhang, an MIT and Broad Institute professor who pioneered CRISPR genome editing, reviewed the pre-print and described the finding as an exciting demonstration of computational biology. Zhang stated: "This is an exciting example of how AI agents can contribute to biological discovery.

The identification of RNA-repeat arrays associated with reverse transcriptases is genuinely intriguing and merits further investigation. I hope this work encourages more scientists to explore how AI can support their research."

Molecular tools such as restriction enzymes, Taq polymerase, and CRISPR all originated from uncharacterized microbial defense systems that copied or modified nucleic acids. Because ART combines an RNA-to-DNA copying enzyme with a programmable-style repeat array, researchers are studying whether it can perform novel DNA manipulations. Anthropic continues to operate human-guided assays at its Bay Area facility to determine the enzyme system's natural biological role.

## Claude discovers a novel enzyme system FAQ

### What is an array-associated reverse transcriptase?

An array-associated reverse transcriptase, or ART, is an enzyme system discovered in bacteriophages that pairs an RNA-copying reverse transcriptase with an uncharacterized accessory protein and an array of evenly spaced, CRISPR-like DNA repeats.

### How did Claude identify the ART system?

Anthropic deployed roughly 950 Claude agents that spent 21 hours and 210 million tokens screening over 200,000 reverse transcriptases, narrowing 3,500 candidates to 20 reports before flagging a repeat array in a jumbo phage.

### Did artificial intelligence conduct the wet lab experiments?

No, human scientists conducted all physical experiments in Anthropic's Bay Area biosafety level 1 and 2 laboratory, while the AI models handled genomic database mining, hypothesis drafting, and data interpretation.

### Why are CRISPR-like repeat arrays significant in biology?

Repeat arrays in systems like CRISPR serve as guide sequences for programmable nucleic acid targeting, indicating that ART could potentially perform targeted cutting, copying, or pasting of genetic material.

## Sources

## Related on Tech Bytes

Advertisement
