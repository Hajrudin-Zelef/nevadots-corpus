---
id: collect-261121-veille-ia/claude-anthropic/claude-found-a-crispr-like-enzyme-system-950-agents-21-hrs-explainx-ai-blog
title: "Claude Found a CRISPR-Like Enzyme System: 950 Agents, 21 Hrs | explainx.ai Blog"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-23"]
keywords: ["anthropic", "claude", "agents", "enzyme", "system", "function", "found", "search"]
source: https://www.explainx.ai/blog/claude-discovers-novel-enzyme-system-art-crispr-like-repeats-2026
source_anchor: ""
source_lines: [1, 156]
section: "Claude_Anthropic"
---
Anthropic just gave the clearest public example yet of what "AI does science" looks like when you strip out the marketing: **a long parallel search, a very aggressive filter, and a small number of survivors that humans test in a lab.**

On September 23, 2026, Anthropic announced a new life sciences research group and molecular biology lab, and shared its first result. In the announcement, Claude agents flagged what the company calls **array-associated reverse transcriptases (ART)**: a phage enzyme system that sits next to a long array of repeating DNA, a layout that looks similar to CRISPR. The post on X drew 15.8 million views.

The claim is careful, and the function is unknown. This guide separates what is established, what is a hypothesis, and what a 486-point Hacker News thread pushed back on. It follows our earlier coverage of Anthropic's physical wet lab and the Life Sciences Verification Program.

## TL;DR: questions people ask first

| Question | Answer | 
|---|---|
| **What was found?** | ART: a phage reverse transcriptase (RT) + partner gene + long repeat array resembling CRISPR | 
| **Does it edit DNA?** | Unknown. Function is still being studied | 
| **How big was the search?** | ~950 agents, ~21 hours, ~210M tokens | 
| **What did agents narrow down?** | 200,000+ RTs → 3,500 candidate systems → 20 detailed reports → 1 standout | 
| **Who did the lab work?** | Human scientists, all of it | 
| **Peer reviewed?** | No. Pre-print plus blog post | 
| **Outside expert reaction?** | Feng Zhang (MIT/Broad): "genuinely intriguing and merits further investigation" | 
| **Biosafety level?** | Anthropic says BSL-1 and BSL-2 only, no human-infecting pathogens | 

## What Anthropic actually claims

Anthropic frames the discovery inside a familiar pattern in biology: restriction enzymes, Taq polymerase and CRISPR all began with someone noticing something odd in natural DNA. The company formed its research group in spring 2026 to test whether general AI models can systematize that noticing.

The specific finding:

- The underlying **reverse transcriptase** (an enzyme that copies RNA into DNA), found in a jumbo phage, had been identified in previous studies.
- What Claude appears to be the first to notice is the **system's defining features** : an associated array of non-coding DNA sequences plus an**accessory protein of unknown function** .
- First experiments show the array is expressed as a **set of distinct short RNAs** , which suggests something analogous to CRISPR's guide-RNA bank may be at play.
- Only a handful of known systems share this feature set, and all of them are programmable and can cut, copy and paste DNA. Several are in development as promising tools.

Anthropic is explicit that this is early: "Our work to understand the primary function of ARTs is ongoing," and it shared the result now "to demonstrate Claude's capabilities and to give the broader community insight into what we're working on."

## How the search worked: the part builders should study

If you build agents, the process matters more than the biology. Anthropic describes a workflow with a distinctive shape.

1. **Prompt.** Humans gave Claude a prompt to search a massive DNA database for interesting new examples of RTs.
2. **Fan-out.** About 950 agents worked for 21 hours, using 210 million tokens. They gathered more than 200,000 RTs and picked out 3,500 new candidate systems.
3. **Report.** Each survivor gets a short, human-readable report that proposes a function and cites evidence.
4. **Critique.** In follow-up analyses "Claude critically evaluates the evidence — typically most candidates are eliminated at this stage." A survey may end with a single candidate or none.
5. **Narrow.** The 3,500 candidates were narrowed to the 20 most compelling and written up. For an expert scientist, Anthropic says that analysis "can take weeks to months of work."
6. **Notice.** One agent inspecting raw sequence near an unusual RT wrote: "[The DNA next to the RT] is spectacular: I can see by eye a tandem repeat array … that's a CRISPR-like … repeat array?!"
7. **Verify like a scientist.** It counted repeats, measured spacing, compared the layout to known RT systems, searched the literature for prior reports, then filed a report for human review.
8. **Lab.** Humans expressed and characterized the candidate in standard lab strains, with Claude helping interpret data.

Two design choices stand out. First, the pipeline starts by having Claude **reproduce known results from public data** to check its methods. Second, Anthropic treats the *hypotheses themselves* as data: with hundreds to thousands of candidate reports per campaign, the team studies what separates proposals worth testing from those it sets aside, then feeds that back into instructions "to mimic our own scientific taste." That is the same loop teams use to improve coding agents through harness engineering.

The tooling is not exotic. Anthropic says the team works in Claude Science and Claude Code, "the same tools available to any scientist," sometimes with a harness that coordinates many sessions in parallel. For readers building their own multi-agent systems, see what an agent harness is and how coordinators work in Claude Code Projects.

A graphic from Anthropic showing Claude identifying a repeating pattern in the raw DNA sequence next to a reverse transcriptase gene

*Figure: Claude identifying the repeat pattern in raw DNA next to the reverse transcriptase. Source: Anthropic's announcement, reproduced with attribution.*

## What Dario Amodei added

Later on September 24, Anthropic CEO Dario Amodei posted a long statement on X (3.6 million views at the time of writing) that sharpens both the claim and the ambition. The points worth knowing:

**A cautious framing of the result.** He called it a "molecular machine that we suspect could represent a new gene editing mechanism," and added that "its precise function, biotechnological utility (if any), or level of significance is not yet clear." His personal bar: "at minimum it is work I would have been proud to do as a PhD student."

**Who did what.** "The work was done mostly, though not entirely, by Claude." In his account, the life sciences team suggested a broad area of research, Claude read the literature and genome data and found the pattern, **Claude proposed experiments to verify it**, and the human team carried them out. That is slightly broader than the announcement's "initial prompt and the lab work" wording: the agents also proposed the validating experiments.

**Independent, parallel work.** Amodei says a **Stanford team working independently** recently described a novel reverse transcriptase system with an associated non-coding array that is in some ways similar, though the two are "distinct systems that evolved independently from each other." That matters for the novelty debate below: it suggests reverse-transcriptase-plus-array systems are a genuinely active area, and that Claude's find is not the only sign of one. He also places it in a line that runs from CRISPR to the bridge recombinase and VIPR, and says interest in RT-based systems has recently increased.

**Why biology is not "limited by experiments."** He rebuts the argument that AI will help less in biology than in math because biology needs wet-lab work. His answer: humans can collaborate with AI to run experiments and validate key results "in a few weeks," iterating as needed. Eventually Claude might control lab equipment autonomously, "with appropriate safeguards in place, but we aren't [there] today," and he restates that the lab is a BSL-1/BSL-2 facility that does not handle materials dangerous to humans.

**Throughput, not latency.** He is explicit that this "will not in itself speed up clinical trial times." The claim is that faster fundamental discovery could greatly **increase the number of promising candidates entering the pipeline**, raising throughput while latency stays the same, and could help with drug targets, therapeutic modalities and measurement.

**The trend argument.** Amodei lays out a capability curve for math: struggling with high-school math in 2023, doing well on top high-school competitions in 2024, solving minor open problems in 2025, more significant ones in early 2026, and "beginning to solve the top few open problems in all of mathematics" by late 2026. He argues biology is "on a similar exponential trend." He connects this to his essay *Machines of Loving Grace*, which described AI potentially helping to "cure most diseases in 5-10 years," and says this result is a first step toward showing AI can help with, and then drive, discovery.

How to read it: the statement is deliberately hedged on the science and ambitious on the trajectory. The first half (function unknown, significance unclear) matches the Hacker News skeptics. The second half (exponential trend, curing diseases) is a forecast, not a result from this study, and readers should weigh it as such.

## What Hacker News pushed back on

The thread reached 486 points and over 500 comments in seven hours. Below is a themed summary, with paraphrases and short attributed quotes from the thread.

### "This is a new arrangement around a known enzyme"

The top comment argued that the RT is a known retron-like enzyme, so "a sober framing would be something like: Claude identified a previously undescribed genomic arrangement around a known reverse transcriptase." Another read the pre-print hoping for a functional wrinkle and found a conserved, highly transcribed array next to reverse transcriptases with possible partner genes. This matches Anthropic's own wording: the RT was known, the array and accessory protein are new.

### "A pre-print and a blog post are not a paper"

Several commenters called the release marketing and asked why it was not submitted to a peer-reviewed journal first. Defenders replied that pre-prints are the lifeblood of fast fields, that top-journal review can take years, and that the pre-print link was on the page hours before the thread (archive snapshots were cited). Both sides have a point. Preprints trade rigor for speed; readers should hold the claim at "candidate system," not "new tool."

### "Anthropic did test it, and the authors are experts"

One commenter disputed the claim that nothing was tested: the authors are specialists in the field and "they did test this in a lab." Another user complained that this comment chain showed people hallucinating what they wanted to be true. Anthropic's post does say the array is expressed as distinct short RNAs, which is an experimental result, though the function is not.

### "How autonomous was it, really?"

A recurring debate. One reading of Anthropic's text ("Our involvement was limited to the initial prompt and the lab work") is that the agents chose their own candidates. A critic rewrote the passage in more mechanical language and argued that the scientists deserve credit for verification. A researcher who uses Claude Science described it as very good at pattern-spotting around binding sites but harder to keep traceable and reproducible. They treat it "like an IDE" and enforce version control so work can be reproduced. That practical note is worth copying.

### "LLM search is non-reproducible"

Commenters pointed out that stochastic agent searches will not return the same result twice. Anthropic's answer is implicit in its process: the *output* is a candidate with evidence anyone can re-check in the raw DNA. A discovery need not be reproducible as a search, only as a finding.

### "Marketing, IPO, and the everything-company question"

A large branch of the thread asked why an AI lab runs a bio lab, with theories ranging from IPO narrative to vertical integration because the core model business has thin moats. Others noted the timing beside reports of Anthropic's IPO preparations. One commenter's counter-view: labs with in-house wet labs get faster feedback loops than any partner arrangement, and Anthropic does partner externally too. We cannot resolve motives, but the practical point stands: verification needs physical experiments, and Anthropic says it built its own.

### "The safety contradiction"

The sharpest one-liner contrasted Anthropic's biosecurity restrictions on users with its own biology work. Anthropic's response is that its lab works only at BSL-1 and BSL-2, does not handle human-infecting pathogens, and all lab work is done by humans. For how Anthropic restricts model behavior in biology, see Fable 5 biology safeguards and the VirBench study.

### "How can a language model reason about biochemistry?"

A long side thread debated whether next-token prediction can "think" about DNA. The useful takeaway is narrower: reinforcement learning on verifiable tasks, tools, and long context changed what these systems do, and DNA is a text-like domain with plenty of literature to reason over. Whatever you believe about understanding, the empirical result is that agents found a pattern humans had not reported.

## What this means for people who build with AI

1. **Fan-out plus filter beats one brilliant prompt.** Most value came from generating thousands of candidates and killing most of them. The critique stage is the product.
2. **Force written reports.** Every candidate had to argue for a function with evidence. Structured reports make review scalable.
3. **Keep humans on the expensive verification step.** The lab was human-run; the agents were the search.
4. **Log everything.** The HN user who treats agent science like an IDE is right: without traceable commands and version control, you cannot reproduce or defend your result.
5. **Watch the claim language.** The strongest version of this work is "we found a candidate system." Every step beyond that needs data.

For related science-agent coverage, see Biohub's virtual biology work, OpenAI's Rosalind workbench, and GeneBench Pro. For a non-agent astronomy case study in the same week — real MeerKAT data, viral Polymarket alien odds — see Harvard exoplanet radio signals and AI hype.

## What is still unknown

- **What ART does.** Cutting, copying, pasting, defense, something else.
- **Whether it is programmable.** The array resembles a guide bank, but programmability is untested.
- **Whether anyone can reproduce it.** Independent labs will need the sequences and constructs.
- **Whether the framing will hold in peer review.** Reviewers may challenge the novelty claim, since the RT itself is known.

Anthropic says it wants collaborators and invites proposals for research questions in genomics and other fields.

## Bottom line

ART is a genuine and modestly framed result: a new **candidate** system, found by a massive agent search, supported by early expression data and a positive note from a leading CRISPR scientist. It is not yet a new gene-editing tool, and the strongest Hacker News criticisms (known RT, no peer review, autonomy framing) are fair. The lasting value is the workflow: 950 agents, a ruthless filter, and human hands on the pipette.

*This post reflects information available on September 24, 2026, from Anthropic's announcement and pre-print description and public discussion. Findings may change with peer review and further experiments.*

## Related reading

- 
Omnii-1: Radical Numerics' genome model and AI-designed phage claim
- 
How can a language model solve math problems and fight disease? — the generate-and-verify explainer
- 
Anthropic's physical wet lab: what it is for
- 
Claude Science built the first complete UV sky map
- 
Anthropic Life Sciences Verification Program and FlashPairformer
- 
Can AI cure cancer?
- 
Fable 5 biology safeguards update
- 
AlphaFold, organoids and autism protein interactions at UCSF
- 
Top 10 harness engineering concepts
- 
Anthropic IPO and Nvidia investment
- 
Official: Anthropic announcement
