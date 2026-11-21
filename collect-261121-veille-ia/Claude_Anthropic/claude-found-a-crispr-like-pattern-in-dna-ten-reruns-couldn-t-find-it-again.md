---
id: collect-261121-veille-ia/claude-anthropic/claude-found-a-crispr-like-pattern-in-dna-ten-reruns-couldn-t-find-it-again
title: "Claude Found a CRISPR-Like Pattern in DNA. Ten Reruns Couldn't Find It Again."
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-23"]
keywords: ["reverse", "system", "claude", "anthropic", "found", "repeat", "array", "genetic"]
source: https://dev.to/bargan/claude-found-a-crispr-like-pattern-in-dna-ten-reruns-couldnt-find-it-again-1pkj
source_anchor: ""
source_lines: [1, 269]
section: "Claude_Anthropic"
---
*Anthropic's AI agents found an unusual genetic system in bacteriophages. Initial experiments supported its RNA expression. The autonomous investigation that found it proved much harder to repeat.*

Anthropic ran an AI research campaign that searched vast amounts of genetic data for unfamiliar enzymes.

After roughly 21 hours, one Claude agent noticed something peculiar: a long array of repeating DNA sequences beside a reverse transcriptase gene.

The arrangement resembled part of CRISPR, the biological system behind modern gene-editing technology.

Anthropic's researchers investigated the finding, named the system ART, and confirmed that its repeated DNA produces distinct RNA molecules.

Then they ran the AI investigation ten more times.

**None of the ten reruns rediscovered the repeat array.**

The agents often encountered the relevant enzyme family. Some even investigated it further. But they never examined the stretch of DNA that had revealed the original pattern.

The discovery was worth investigating in the laboratory. Reproducing the path that led to it was another matter.

For anyone building autonomous AI agents, that second result offers a useful engineering lesson.

On September 23, 2026, Anthropic announced that its new life sciences research group had identified a previously uncharacterized biological system with features reminiscent of CRISPR.

The researchers had given Claude a broad assignment: investigate reverse transcriptases and search for previously unknown associations with other genes.

A reverse transcriptase is an enzyme that copies RNA into DNA. Reverse transcriptases appear in many biological systems, including some involved in bacterial defense.

According to Anthropic's announcement, approximately 950 agent sessions processed 210 million tokens while exploring more than 200,000 reverse transcriptases.

The agents initially identified around 3,500 candidate systems and narrowed their investigation to roughly 20 promising leads. The preprint gives more precise campaign totals: 949 sessions, 215.6 million tokens excluding cache reads, and 21.5 hours.

One candidate stood out because of its unusual reverse transcriptase.

While examining the DNA surrounding that enzyme, a Claude Mythos 5 agent recognized a series of repeated sequences.

The researchers published part of its reaction:

I can see by eye a tandem repeat array


The agent suspected a resemblance to CRISPR.

It then counted the repeats, examined their spacing, compared the genetic arrangement with known systems, and searched the scientific literature.

The investigation ended with a report for human review.

Anthropic's scientists took over the experimental work.

The researchers named the system *array-associated reverse transcriptases*, or ART.

ART is found mainly in bacteriophages, viruses that infect bacteria.

Three components appear together:

1. 
**A reverse transcriptase** , belonging to a family of enzymes that typically copy RNA into DNA.
2. 
**A DNA repeat array** , containing regularly arranged repeat-and-spacer units of approximately 200 nucleotides. The conserved repeats themselves are shorter: the preprint reports 15–49 nucleotides, separated by spacers of 120–220 nucleotides.
3. 
**An associated protein-coding gene** , whose function in the system remains unknown.

The repeat array is the feature that caught Claude's attention.

It resembles the organization of CRISPR arrays, although ART has different molecular components and no established CRISPR-like function.

In a bacterial CRISPR-Cas immune system, short pieces of genetic material from previous viral encounters can be stored between repeated DNA sequences.

The cell transcribes these regions into RNA molecules. Together with CRISPR-associated proteins, those RNAs can help recognize matching genetic targets.

Researchers eventually learned to program parts of that machinery to edit DNA.

ART also appears to produce multiple distinct RNA molecules from its repeat array.

The biological role of those RNAs remains unknown.

*Explanatory schematic, not to scale. RNA expression has experimental support; the complete system's function remains unknown.*

In the research preprint, the authors report that ART repeat arrays are expressed during *Staphylococcus* bacteriophage infection and appear as discrete RNA units.

That infection evidence came from reanalysis of an existing public RNA-sequencing dataset. The team also expressed the SA1 ART system in *E. coli* and observed similar short RNAs through small-RNA sequencing.

Together, these results support expression of the repeated region as distinct RNA species.

They leave major questions unanswered.

The researchers have not established the molecular function of the complete ART system. The reverse transcriptase's proposed activity within ART still needs experimental demonstration, as does the role of its associated protein.

No programmable genome-editing capability has been demonstrated.

The result is a newly characterized genetic arrangement with interesting experimental properties. Whether it becomes useful for biotechnology depends on experiments that haven't been completed.

In 2021, Abby Korn and colleagues published a study of three unusually large bacteriophages infecting *Staphylococcus aureus*.

Their paper in the Journal of Virology described a reverse transcriptase associated with a phage called MarsHill.

The researchers also noted a potentially relevant upstream noncoding region.

They did not characterize the repeating array or the broader combination of features Anthropic now calls ART.

Claude therefore wasn't discovering an entirely unknown enzyme.

It helped identify a previously undescribed arrangement around an enzyme family that scientists had already encountered.

Genomic databases contain enormous amounts of material that researchers have collected without fully understanding.

Finding an unusual relationship between existing sequences can be valuable even when the underlying genes have been known for years.

Anthropic ran ten additional research campaigns using the same harness and research brief.

None identified the ART repeat array in the researchers' analysis of the reruns.

Nearly every campaign that completed the initial census sampled ART loci. Two campaigns investigated the lineage more closely.

Yet the agents did not read the upstream DNA that contained the unusual repeats.

This is a peculiar failure because the model had already demonstrated that it could recognize the pattern.

Anthropic also studied the difference between recognition and autonomous investigation.

In evaluations where the relevant sequence was placed directly in context, the strongest models accurately described the array in at least 90% of attempts.

Performance was weaker when agents had to navigate files and tools to locate the evidence themselves. In one file-based condition, Opus 5 at input level 4, recognition fell to 32%.

Sometimes the agents never read a sufficiently long continuous DNA sequence to expose the structure.

The limiting factor was therefore partly the investigation trajectory.

The model could recognize a useful pattern when presented with the evidence. Its autonomous search did not reliably bring that evidence into view.

*Explanatory summary of the reported campaigns, not a calibrated success-rate estimate. The preprint's rerun search used identifiers from the original campaign; ART loci outside that identifier set would not be detected by that search.*

Ten unsuccessful reruns do not provide a precise estimate of how likely the original discovery was. Nor do they disprove the biological finding.

They reveal that the same broad research assignment can produce substantially different investigative behavior.

The outcome depends on which hypotheses the agents pursue, what data they inspect, and when they decide they've seen enough.

Consider a coding agent investigating a failure in a large repository.

The actual problem is a configuration change in a shared package.

The agent has access to the entire codebase. It can search files, inspect commits, execute tests, and read logs.

But it begins with the failing service.

It follows imports, investigates an unrelated dependency, runs several tests, and produces a plausible explanation.

It never opens the configuration file containing the cause.

Giving the agent better reasoning capabilities may improve its investigation, but recognizing a bug and locating the evidence for that bug are separate capabilities.

A benchmark that puts the broken configuration directly into the prompt measures the former.

A benchmark that asks the agent to investigate an unfamiliar repository measures both.

Anthropic's ART experiment offers a concrete reason to evaluate those capabilities separately.

For coding agents, I would want at least three kinds of evidence from an autonomous investigation.

**First, record the search trajectory.**

Which files did the agent open? Which commands did it execute? Which search results did it ignore? Did it inspect the relevant code at all?

A correct final answer tells us little about the reliability of the investigation that produced it.

**Second, repeat the same task.**

One successful run can hide a fragile search strategy. Multiple runs can expose variation in what the agent chooses to inspect.

A useful evaluation should record both successful and unsuccessful trajectories.

**Third, test whether the agent can find known evidence.**

Place a known issue in a realistic repository and check whether the agent locates the relevant files before attempting a fix.

This helps distinguish recognition failures from retrieval and exploration failures.

These are engineering recommendations, not procedures validated by the ART study itself. They follow from the difference between the paper's fixed-context results and its autonomous investigations.

The distinction matters well beyond biology.

Security investigations, incident response, repository-wide migrations, and compliance reviews all involve searching large collections of potentially relevant evidence.

A model may be capable of understanding a clue and still fail to encounter it.

The biological discovery has drawn both interest and skepticism.

Feng Zhang, a pioneer of CRISPR gene editing, described the identification of repeat arrays associated with reverse transcriptases as intriguing and worthy of investigation in a comment published by Anthropic.

Other researchers have been more cautious.

In an IBM Think report, Stanford biochemist Alex Gao said the association merits further investigation to establish its biochemical activity and function.

MIT researcher Kevin Esvelt offered a more skeptical assessment, questioning how unusual the bioinformatics identification really was.

These responses reflect different judgments about the novelty and potential significance of the finding.

The discovery also involved an experienced human research team.

Scientists chose the initial problem, designed the experimental program, reviewed the computational leads, and performed the laboratory work.

The AI agents carried out extensive computational investigation between those human decisions.

The associated preprint was posted to bioRxiv on October 11, 2026 and has not yet undergone peer review. All six authors are Anthropic employees, and the paper discloses a provisional patent application related to the work.

Independent replication and further experiments will help establish how much biological significance ART deserves.

In 1987, Japanese researchers investigating *E. coli* reported unusual repeated sequences in its genome.

They couldn't establish their biological function.

Decades of research eventually connected such sequences to bacterial immune systems and, later, programmable genome editing.

In 2020, Emmanuelle Charpentier and Jennifer Doudna received the Nobel Prize in Chemistry for developing the CRISPR-Cas9 genome-editing method.

The resemblance to ART makes an appealing historical parallel.

But repeated DNA sequences alone don't establish the same mechanism.

CRISPR's usefulness came from understanding how its RNA guides and associated proteins recognize and modify genetic targets.

For ART, the equivalent functional explanation is still missing.

Researchers need to establish what the reverse transcriptase does, how the repeat-derived RNAs participate, whether the associated protein contributes to the mechanism, and what advantage the system provides to the bacteriophage.

Those experiments will determine whether ART is a specialized piece of phage biology or a system that can eventually be adapted for practical use.

Either outcome could add to our understanding of reverse transcriptases.

Anthropic's research group is investigating how to improve the selection of promising hypotheses from large collections of agent-generated reports.

That creates another evaluation problem.

An autonomous scientist can propose thousands of explanations. Most won't be worth testing in a laboratory.

The practical value of the system depends on how many useful candidates it produces, how much computing and human review it consumes, and how often laboratory experiments support its predictions.

The ART campaign offers a detailed example to study.

It also exposes a failure mode that researchers and software engineers can test today.

The original agent examined the DNA upstream of an unusual enzyme and noticed a repeat array. Ten reruns, working from the same assignment, missed it.

A more reliable research agent would need to find promising leads repeatedly, or at least make its exploration sufficiently systematic that a useful candidate is less likely to disappear between runs.

Every autonomous discovery deserves two evaluations: whether the finding survives independent verification, and whether the investigation can produce comparable results when repeated.

Anthropic has provided initial experimental support for RNA expression. Independent replication, the function of ART, and repeatable autonomous discovery remain open questions.

- Anthropic — Claude discovers a novel enzyme system with CRISPR-like repeats, September 23, 2026.
- Yoon et al. — Autonomous AI agents discover reverse transcriptases with tandem repeat arrays, bioRxiv preprint, October 11, 2026. Full text.
- 
Korn et al. — Comparative Genomics of Three Novel Jumbo Bacteriophages Infecting Staphylococcus aureus, *Journal of Virology* , 2021.
- 
Ishino et al. — History of CRISPR-Cas, *Journal of Bacteriology* , 2018.
- IBM Think — What Anthropic's AI biology experiment actually showed, October 5, 2026.

*AI assistance was used in editing and source checking. The cover is an AI-generated conceptual illustration; the diagrams are explanatory graphics, not experimental images.*

## Top comments (4)

The reruns finding the same enzyme family but never looking at that stretch of DNA is the part I keep thinking about. The discovery still held up in the lab, but I'd want every agent's trail saved so a person can see why it looked where it did, and what the others skipped.

That's the part I keep coming back to as well. The lab result confirms the finding, but it doesn't explain why one run found it and the others missed it.

I'd love to see what each run actually explored, including the dead ends. That could be just as interesting as the discovery itself.

Thanks for reading and sharing your thoughts, Sal!

Some comments may only be visible to logged-in visitors. Sign in to view all comments.
