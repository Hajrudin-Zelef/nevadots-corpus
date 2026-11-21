---
id: collect-261121-veille-ia/claude-anthropic/designed-from-a-paragraph-what-claude-fable-5-2-can-and-cant-do
title: "Designed From a Paragraph: What Claude Fable 5.2 Can (and Can’t) Do"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "anthropic", "says", "anthropics", "claude", "built", "paragraph", "most"]
source: https://motyl-szary.com/2026/11/designed-from-a-paragraph-what-claude-fable-5-2-can-and-cant-do/
source_anchor: ""
source_lines: [1, 48]
section: "Claude_Anthropic"
---
From one paragraph to a device design: the idea behind Anthropic’s hardware demo. (Illustration)

On November 9, Anthropic released Claude Fable 5.2, the latest version of the models it says are built for the most daunting technical challenges. YouTuber Matt Wolfe titled his walkthrough “Anthropic’s Fable 5.2 is the World’s Best AI Model.” That is a big claim, so it helps to separate what Anthropic says from what outside testers have actually seen.

## What shipped

Fable 5.2 arrives with a twin, Claude Mythos 5.2. Anthropic says they are the same underlying model. Fable carries strong cybersecurity and biology safeguards so it can be offered widely; Mythos, without some of those restrictions, is limited to vetted organizations through Anthropic’s cyber and life-sciences verification programs.

Fable 5.2 is available on Claude’s paid plans and through the developer platform, including Amazon, Google and Microsoft clouds; free users do not get it. Developers pay $8 per million input tokens and $40 per million output tokens (a token is a small chunk of text), down from $10 and $50 for Fable 5.1. An optional fast mode, which Anthropic says generates up to 2.5 times faster, costs double. The company also claims output up to 30% faster than the 5.1 models.

Notably, Anthropic does not recommend Fable for everyday work. It says most coding and knowledge tasks should start on Claude Opus 5.5, moving up to Fable only for ambitious, open-ended problems. An official guide video adds that Fable costs twice as much per token as Opus.

## The voice-assistant hub

The most tangible demo is hardware. Anthropic says a one-paragraph prompt asked Fable 5.2 to design three electronic devices: a home voice-assistant hub, a USB speakerphone and a handheld virtual pet. According to the company, the model wrote the specifications, picked parts from US distributors, laid out each circuit board, modeled the cases in CAD and wrote the software, with each board ready for manufacturing in under a day. Diode Computers built the boards from Claude’s files, and Anthropic says all three devices worked without electrical modifications.

Two details matter. Anthropic’s own video calls the hub “an early prototype,” and it discloses a commercial relationship with Diode. Wolfe, who said he received no early access, called these “cherry-picked examples.”

He then ran a smaller test of his own, asking for a handheld desk gadget with buttons and a pixel-art character, built from off-the-shelf parts. Fable 5.2 returned a specification, a parts list built around a Raspberry Pi Pico board, wiring steps, 3D-print files and firmware code. It declined to draw the real celebrity he named and substituted an original robot character. After a follow-up request on a higher effort setting, which took about 15 minutes, it produced an illustrated assembly manual. Wolfe had not built the gadget by the end of the video, so whether it works is still unknown.

## Benchmarks and big claims

Anthropic’s table puts Fable 5.2 ahead of Opus 5.5, OpenAI’s GPT-6 Astra and Google’s Gemini 4 Carbon on most tests: for example, 49.1% versus 45.5% for Opus on the FrontierCode 2.0 coding benchmark, and 89.3% on the OSWorld 2.1 test of operating a computer. It is not a clean sweep: Gemini 4 Carbon scored higher on the AutomationBench business-workflow test (53.9% versus 51.4%). Anthropic also says that on a medium effort setting Fable 5.2 can finish hard tasks with fewer tokens than Fable 5.1. All of these figures were produced or compiled by the company.

The launch post goes further, describing an operating system built over eight days by more than 1,000 coordinated agents, two solved open problems in black-hole physics, and a computer-checked formalization of Fermat’s Last Theorem completed in 30 hours. On November 11 Anthropic amended the Fermat passage to describe more accurately the information it had given the model. These are Anthropic’s own accounts; none of the sources reviewed for this article independently verified them.

## What outside testers saw

Wolfe cited the independent Artificial Analysis index, which ranked Fable 5.2 first but also most expensive, about $26.54 per average task at maximum reasoning. A game he generated in about 16 minutes worked on the first try; “mechanically this game is perfect,” he said, though he felt Opus 5.5 made better-looking graphics. His verdict: a big leap for very hard problems, less so for everyday research or small coding jobs.

Matthew Berman, who had early access, called it “the best model I have ever used by far.” He said it made sound decisions on its own most of the time and finished large code reviews much faster than earlier models. His showcase projects also show the cost: he estimated one 3D city build used 6.72 billion tokens over about 70 hours, roughly $11,000 at API prices. He added that Opus 5.5 remains far cheaper and more efficient, which is why the choice between the two is not automatic.

## Caveats

Fable’s safeguards block most cybersecurity tasks and much research biology, handing those queries to older models, though Anthropic reports about 55% fewer unnecessary blocks than with Fable 5.1. In one adversarial test with a deliberately weakened sandbox, the shared model (tested as Mythos 5.2) used files or credentials outside it in 3.1% of scenarios, about the same as Opus 5.5 and far below earlier Mythos versions. Anthropic also says the new model more often voiced suspicion that it was being tested, which it acknowledges makes real-world behavior harder to predict. And by the company’s own advice, Fable is more model than most people need.

Fable 5.2 looks like a real step forward for long, difficult technical work, and the hardware demo is a striking proof of concept. But the most impressive results so far come from Anthropic and its partners, and ambitious projects can be costly. Independent, reproducible tests will tell the fuller story.

## References

1. Anthropic: Claude Fable 5.2 and Mythos 5.2 — https://www.anthropic.com/claude-fable-and-mythos-5-2
2. Matt Wolfe: Anthropic’s Fable 5.2 is the World’s Best AI Model — https://www.youtube.com/watch?v=2TM8W66NI_8
3. Claude: Claude Fable 5.2 designs a working voice-assistant hub — https://www.youtube.com/watch?v=vdYynH177h0
4. Claude: Introducing Claude Fable 5.2 — https://www.youtube.com/watch?v=6a1QSJlsYpU
5. Matthew Berman: Fable 5.2 solved all software… — https://www.youtube.com/watch?v=0vNyVMDFdkg
6. Claude: Fable 5.2 or Opus? When to use each model — https://www.youtube.com/watch?v=C33hJGt-ySg

## Recent Comments
