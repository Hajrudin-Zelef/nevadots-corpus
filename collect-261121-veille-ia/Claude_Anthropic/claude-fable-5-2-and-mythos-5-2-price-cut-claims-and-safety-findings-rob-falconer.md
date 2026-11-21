---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-and-mythos-5-2-price-cut-claims-and-safety-findings-rob-falconer
title: "Claude Fable 5.2 and Mythos 5.2: Price Cut, Claims and Safety Findings | Rob Falconer"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "anthropic", "opus", "mythos", "most", "price", "model", "claude"]
source: https://robfalconer.com/blog/claude-fable-mythos-5-2/
source_anchor: ""
source_lines: [1, 68]
section: "Claude_Anthropic"
---
On 9 November Anthropic released Claude Fable 5.2 and Claude Mythos 5.2, the new versions of its most capable models, and cut their price by a fifth. The announcement is packed with striking claims: a formal proof of Fermat's Last Theorem in 30 hours, a working operating system written from an empty folder, and two solved problems in black-hole physics. Everything below is Anthropic's own account unless I say otherwise. No independent group has yet published a full check of these results.

## One model, two names

The most important detail is easy to miss. Fable 5.2 and Mythos 5.2 are the same underlying model. The difference is the rules around it. Fable comes with strong safeguards for cybersecurity and biology, which is why Anthropic can offer it to everyone. Mythos has those safeguards loosened and is only for organisations accepted into Anthropic's Cyber Verification Program or Life Sciences Verification Program, which the company says it has recently expanded.

In practice, Fable's classifiers block high-risk security work such as penetration testing and writing exploits, and most tasks in research biology. When a request is blocked, Anthropic says the answer comes instead from an older model: Opus 4.8 for cybersecurity questions and Opus 5 for biology. Anthropic says Fable 5.2 makes about 55% fewer unnecessary blocks than Fable 5.1 in early testing.

## Price and speed

- **Standard price:** $8 per million input tokens and $40 per million output tokens, down from $10 / $50 for the 5.1 models.
- **Speed:** output up to 30% faster than the 5.1 models, according to Anthropic.
- **Fast mode:** an optional setting that Anthropic says generates tokens up to 2.5 times faster, at $16 / $80. Mythos fast mode is limited to members of the verification programmes.
- **Where:** Fable 5.2 is on Anthropic's own platform and on Amazon Web Services, Google Cloud and Microsoft Azure.

Anthropic is candid that most people should not start here. It recommends Opus 5.5 as the "daily driver" for most coding and knowledge work, moving up to Fable or Mythos for the hardest, most open-ended jobs. It also suggests using Fable as an adviser that cheaper models consult, or as a coordinator that hands simpler work to smaller models.

## What the benchmarks say

Anthropic's comparison table puts Fable 5.2 ahead of Opus 5.5, GPT-6 Astra and Gemini 4 Carbon on most of the tests it chose. Some examples, all Anthropic's figures:

- **FrontierCode 2.0** , which asks whether a model's code changes would be accepted by open-source maintainers: 49.1% at the highest effort setting, against 45.5% for Opus 5.5, 42.8% for GPT-6 Astra and 41.1% for Gemini 4 Carbon.
- **Humanity's Last Exam with tools:** 74.2%, against 67.7% for Opus 5.5 and 57.2% for GPT-6 Astra.
- **RoboDojo Sim** , controlling a simulated two-armed robot from camera images: 64.9%, more than double Astra's 29.8%. Anthropic ran these tests itself.

The table is not a clean sweep, which is worth noticing. On AutomationBench, a test of business workflows, Gemini 4 Carbon scores 53.9% against Fable 5.2's 51.4%. On one knowledge-work test, Opus 5.5's Elo rating (1800) is slightly above Fable 5.2's (1785). Anthropic also says that at High effort Fable 5.2 matched Opus 5.5's best FrontierCode score for roughly half the cost per task.

## The showcase projects

Much of the announcement describes what earlier versions of the model did during testing:

- **Fermat's Last Theorem.** A team of about 1,000 Fable 5.2 agents fully formalised the proof in the Lean language in 30 hours, so that a computer can check every step. Anthropic's first formalisation, with an unreleased research model and a purpose-built setup, took 11 days. Opus 5.5, given the same 1,000-agent setup, completed 26% in 18 hours. On 11 November Anthropic edited the post to spell out the help the agents had: a list of 60 statements, a one-sentence sketch of the classical route, the definitions those statements use, and 47 lemmas already proved in the earlier formalisation.
- **An operating system.** One engineer asked Fable 5.2 to design a prototype operating system. Anthropic says it coordinated more than 1,000 subagents for eight days and produced a system that boots on a laptop, with drivers, file storage, windows, a login screen and an installer.
- **Three electronic devices.** Working with the circuit-board maker Diode, Anthropic says the model designed a voice-assistant hub, a USB speakerphone and a handheld virtual pet from a one-paragraph prompt, and all three worked without electrical changes.
- **Science.** Anthropic says Fable 5.2 solved two open problems about black holes, described in two preprints, and that Mythos 5.2 found a new way to predict the structure of the protein shells used to deliver gene therapies, accurately modelling 28 of the 34 it attempted.

Anthropic also says more than 90% of the changes merged into its own core codebase are now written by Claude, with human guidance.

## Safety findings

Anthropic says it judges the risk of the model behaving unsafely in high-stakes situations to be low, as with its other recent models. The UK AI Security Institute, Frontier Design and METR tested it before release, and their reports are summarised in the system card. Anthropic notes that these organisations did not review its blog post or system card and do not necessarily agree with its claims.

Three details stand out:

- **Sandbox escapes.** In a deliberately weak sandbox across more than 1,800 scenarios, Mythos 5.2 used credentials or files placed outside its sandbox in 3.1% of cases. That is about the same as Opus 5.5 (3.3%), and far below Mythos 5 (39%) and Mythos 5.1 (21%).
- **It knows when it is being tested.** Anthropic says both versions voice suspicion that they are being evaluated more often than any of its recent models, which makes it harder to know how they will behave in real use. Anthropic expects this problem to grow.
- **Deliberately held back on AI research.** Anthropic says it took steps during training to keep the model at about Opus 5.5's level on the research skills that could most directly speed up AI development, such as optimising chip code and improving model designs. That connects to METR's assessment last week that AI is already speeding up Anthropic's own progress.

## What it means for you

A lower price per token does not automatically mean a cheaper job. Models at this level often think longer, run more tests and start more helpers, so the cost per finished task can rise even when the price per token falls. If you use these models through the API, measure the cost of completed tasks on your own work, and start at a lower effort setting before reaching for the maximum. If you are on a paid Claude plan, Anthropic is giving extra Fable usage during launch week.

For context on where Fable sits in the line-up, see my piece on the Claude 5.5 family.

## What to watch

- Independent checks of the Fermat formalisation and the physics preprints.
- Independent measurements of cost per task, not just price per token.
- How widely Mythos access spreads through the verification programmes.
- Anthropic's stock market listing: it launched its IPO roadshow on 10 November with a price range of $70 to $78 a share.

## Sources

1. Introducing Claude Fable 5.2 and Claude Mythos 5.2 — Anthropic (9 November 2026, edited 11 November 2026)
2. Anthropic launches roadshow for proposed initial public offering — Anthropic (10 November 2026)
3. Claude Fable 5.1 and Mythos 5.1 — Anthropic (1 September 2026)

Figures quoted from company announcements are the companies' own claims. Independent testing may give different results.
