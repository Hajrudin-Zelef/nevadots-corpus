---
id: collect-261121-veille-ia/claude-anthropic/claude-found-a-crispr-like-system-in-virus-dna-what-anthropic-s-lab-actually-did
title: "Claude Found a CRISPR-Like System in Virus DNA: What Anthropic's Lab Actually Did"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-23"]
keywords: ["anthropic", "system", "array", "repeat", "claude", "found", "reverse", "crispr"]
source: http://dev.to/jamilxt/claude-found-a-crispr-like-system-in-virus-dna-what-anthropics-lab-actually-did-fko
source_anchor: ""
source_lines: [1, 61]
section: "Claude_Anthropic"
---
On September 23, Anthropic published something unusual for an AI company: a biology pre-print. The claim is that Claude, running as a swarm of agents, found a previously unknown enzyme system hiding in the DNA of bacteriophages, the viruses that infect bacteria.

The system is now called ART, short for array-associated reverse transcriptases. Its function is still unknown. Anthropic shared the finding early anyway, and the way they found it is the more interesting story.

The setup started with one prompt: search a massive DNA sequence database for interesting new examples of reverse transcriptases, enzymes that copy RNA into DNA.

Anthropic says their involvement was limited to that initial prompt and the lab work afterward. In between, roughly 950 Claude agents spent 21 hours and burned about 210 million tokens. The pipeline looked like this:

- Collected more than 200,000 reverse transcriptases from the database
- Narrowed that to 3,500 candidate systems
- Cut that list to the 20 most compelling candidates
- Wrote a human-readable report for each one, proposing a function and listing the evidence

Anthropic says this kind of analysis can take a human scientist weeks to months. The agents did it in under a day, and most candidates were eliminated along the way, which is exactly how real science works. A survey that eliminates 19 of 20 candidates is a good survey.

One agent noticed something odd. Next to the gene for a strange-looking reverse transcriptase sat a long array of evenly spaced DNA repeats. The agent's own output, quoted in Anthropic's post: "[The DNA next to the RT] is spectacular: I can see by eye a tandem repeat array … that's a CRISPR-like … repeat array?!"

That pattern of a working scientist recognizing something, then immediately doubting it, is worth sitting with. The agent counted the repeats, measured their spacing, compared the layout to known systems, searched the literature for prior reports, and only then filed a report for human review.

The system has three parts sitting together in phage DNA: the reverse transcriptase gene, a partner gene of unknown function, and the repeat array itself.

The repeat layout resembles a CRISPR array. In CRISPR, that array works like a memory bank of RNA sequences that lets the system recognize and cut matching viral DNA. It is what makes CRISPR programmable, and programmability is what turned a bacterial immune quirk into gene-editing medicine.

Anthropic's first lab experiments show the ART array is also expressed as a set of distinct short RNAs. That suggests something analogous may be happening. But nobody knows yet. The comparison to CRISPR is about structure, not proven function.

There is a detail here that matters for honesty in AI announcements. The reverse transcriptase itself had been identified in earlier studies. What Claude appears to be first at is noticing the system's defining features: the repeat array and the accessory protein next to it. This is not "AI invents biology from nothing." It is "AI spotted a pattern in public data that the field had walked past." Those are very different claims, and Anthropic's own paper draws the line correctly.

Feng Zhang, one of the pioneers of CRISPR genome editing, reviewed the pre-print and called the RNA-repeat arrays "genuinely intriguing," adding that he hopes the work encourages more scientists to explore how AI can support research.

Some of the biggest tools in biology started exactly this way: someone noticed something odd.

Restriction enzymes came from bacterial immune systems that destroy invading viral DNA. Researchers realized they could use those enzymes to cut DNA at chosen spots, and the biotechnology industry followed. Taq polymerase, the enzyme that makes PCR possible, was found in a bacterium living in a Yellowstone hot spring. CRISPR itself was first noticed as an unusual repeat sequence in bacterial DNA, long before anyone understood what it did.

The systems with CRISPR-like repeat arrays share a rare set of characteristics. Only a handful of known systems have them all together, and every one of them turned out to be programmable: cutting, copying, and pasting DNA on demand. ART shows structural signs of joining that club. If it does, the discovery of its function becomes a very valuable question. If it does not, the search method is still the story.

Strip out the biology and this is an agent-architecture case study.

The workflow was not one model answering one question. It was hundreds of parallel sessions coordinated by a harness, each producing candidate reports, with a filtering process where most candidates died at the review stage. Anthropic says they now study the rejected hypotheses themselves, trying to learn what separates proposals worth testing from noise, and feed that back into the instructions they give Claude. They describe it as teaching the model their scientific taste.

They also ran this on Claude Science and Claude Code, the same tools available to any scientist. No secret internal model.

That has implications for how you might use agents on your own domain. The pattern generalizes: give agents a large corpus, a cheap first-pass filter, a written report as the output format, and a human review gate before anything expensive happens. The reports become auditable artifacts. You can disagree with a rejection and re-run it. Nobody has to trust a black box score.

Anthropic's reason for publishing early is stated plainly in the post: to demonstrate what Claude can do and to show the community what they are working on. The company is reportedly preparing for a public offering, so some skepticism about timing is fair. But a pre-print with a named method, raw numbers, and an open question is a checkable claim, not a marketing video. Feng Zhang's decision to comment publicly is a reasonable signal that the work is being taken seriously by people qualified to judge it.

The open question is function. Anthropic says experiments are underway to determine how ART works. If the short RNAs turn out to guide the system toward targets the way CRISPR's RNAs do, that would be the beginning of a new programmable biology toolkit, found by an AI, in data humans already had.

If ART turns out to be something mundane, the 950-agent search pipeline still worked end to end: prompt, search, filter, report, lab test. That pipeline is now documented, and other labs can copy it.

Either way, the honest summary is this. No one knows what ART does yet. But the method that found it produced a testable candidate in 21 hours from 200,000 possibilities, with humans touching only the start and the end. That workflow is the part worth stealing.

- Anthropic blog post: Claude discovers a novel enzyme system with CRISPR-like repeats (anthropic.com/news/claude-discovers-novel-enzyme-system)
- Anthropic announcement thread, September 23, 2026 (x.com/AnthropicAI/status/2102824959827742916)
- The Verge: Anthropic's biolab made a discovery it's comparing to Crispr
- Anthropic pre-print (PDF linked from the blog post)

## Top comments (2)

Excelente articulo Jamilxt, gracias por divulgar.

Thank you!
