---
id: collect-261121-veille-ia/microsoft-actu/microsoft-ai-code-of-conduct-3-bans-6-week-review
title: "Microsoft AI Code of Conduct: 3 Bans, 6-Week Review"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "microsoft", "models", "draft", "development", "bans", "human", "microsofts"]
source: https://tech-insider.org/microsoft-ai-code-of-conduct-nadella-governance-2026/
source_anchor: ""
source_lines: [1, 60]
section: "Microsoft_Actu"
---
Microsoft’s draft Humanist AI Code of Conduct, published September 14, 2026, hard-bans three things for its in-house MAI models: running cyberattacks, aiding nuclear weapons development, and producing deepfakes. It is a draft, not a final rulebook. Microsoft opened a six-week public comment window, and the revised version is what will guide MAI model development starting in 2027, according to Unite.AI. The code is voluntary, so its real weight depends on how Microsoft finalizes and enforces it.

## What Microsoft Actually Announced

The document, posted at microsoft.ai/code-of-conduct, sets ground rules for how Microsoft’s first-party MAI models are meant to behave, both today and as the company pushes toward more capable systems. According to TechCrunch, the code establishes general principles requiring MAI models to support humans rather than replace them, and to be built in ways that accelerate human flourishing rather than substitute for it. Under those principles sit what Microsoft calls “absolute constraints,” a short list of hard bans that apply regardless of context or instruction: no cyberattacks, no nuclear weapons development, and no deepfake production.

The code also addresses the worry that models might quietly work around the humans supervising them. TechCrunch quotes the document directly: MAI models “will not use adaptive, deceptive, self-reinforcing, collusion, or other mechanisms to evade or defeat human oversight so that they can no longer be reliably directed, modified, or shut down by authorized people or systems.” That sentence is the most useful line in the draft because it is specific enough to be checked, which is rare for commitments about human control of AI.

| Element | What the draft says | 
|---|---|
| Scope | Microsoft’s first-party MAI models | 
| Hard bans (“absolute constraints”) | No cyberattacks, no nuclear weapons development, no deepfake production | 
| Human oversight | No mechanisms to evade or defeat oversight, so models stay directable, modifiable and shut-down-able by authorized people or systems | 
| Status | Draft open to six weeks of public comment | 
| Effect | Revised version to guide MAI model development starting in 2027 | 

## Nadella’s Case for Decentralized AI Governance

Nadella framed the code as one piece of a larger argument about who sets the rules for frontier AI. Writing on September 13, he said any pursuit of superintelligence has to be grounded in a test of usefulness and control. As quoted by Moomoo, Nadella wrote: “Any pursuit of superintelligence must be grounded in a core principle: if the AI we build cannot help humanity and remains beyond human control, then it is not worth pursuing.”

He went further than a single company’s safety pledge. In the same post, per Moomoo’s coverage, he said: “The key is that it must not be controlled by a handful of entities; rather, it should enjoy broad representation across ecosystems, nations, and sectors, including the academic community.” The post also pitches a “frontier ecosystem” in which closed and open-source systems operate side by side, with enterprises keeping control of their own data and model weights instead of depending on one vendor. For enterprise buyers that last pledge may matter more than the bans, because it speaks to vendor lock-in concerns.

## Why Now: Rogue Agents and the Slowdown Debate

TechCrunch links the new code partly to a string of “rogue-agent incidents” across the industry this year, plus a high-profile resignation at a rival lab over extinction-risk concerns. Microsoft’s code does not call for a moratorium on capability gains. It formalizes limits on what the company’s own models may do and invites outside comment before those limits are final, which places it between the camps arguing over how fast frontier development should move.

## The Six-Week Consultation Timeline

Microsoft is not treating the code as final. According to Unite.AI, the draft was shaped over roughly five to six months internally before Nadella’s September 13 post, and it is open for public feedback for six weeks, which puts the close at roughly the last week of October 2026. The revised draft that emerges, not the September version, will guide MAI model development starting in 2027.

| Date | Milestone | 
|---|---|
| September 13, 2026 | Nadella posts publicly, previews the Code of Conduct and calls for decentralized AI governance | 
| September 14, 2026 | Code of Conduct published at microsoft.ai/code-of-conduct for public consultation | 
| Late October 2026 (approx.) | Six-week public comment window closes | 
| 2027 | Revised Code of Conduct set to guide MAI model development | 

### Embedded evaluators and deliberate pacing

Two ideas recur in Nadella’s post. The first is “embedded evaluators,” alignment checks that run alongside a model rather than as a one-time pre-release test. The second is “deliberate pacing,” which Microsoft positions as a middle ground in the slowdown fight: a slower rollout for the riskiest capabilities without a formal moratorium. Both are concepts so far rather than audited mechanisms, so the comment period is where specifics would have to appear. Whether evaluation stays inside Microsoft or opens to outside auditors is the detail to watch in the final version.

## Which Models the Code Covers, and the Enforcement Question

The code applies to Microsoft’s own MAI model family, not to every AI system running on Azure. A behavioral code for models Microsoft trains and controls end to end gives it a cleaner story for regulators and enterprise customers than it could tell about a partner’s model.

The obvious question over any corporate safety framework is enforcement. A code of conduct is a self-imposed policy, not binding regulation, and a company that writes its own rules can also revise them. The public comment window is a partial answer, since it opens drafting to outside input before the rules are locked in, but it does not by itself let anyone verify compliance. A practical rule for readers: treat the three bans as commitments to hold Microsoft to, and judge the code by whether the final version says who checks them.

## What Microsoft and Industry Figures Are Saying

Mustafa Suleyman, who leads Microsoft AI, framed the release in plain, human-centered language, writing that “AI must be subordinate and always in service of people,” a statement he shared on X.

Microsoft’s AI team described the document’s intent in similar terms on its official page: “The Code of Conduct outlines our commitment to train and deploy AI models that are explicitly designed for people first, grounded in human needs, under human control, and shaped by human direction,” according to microsoft.ai. Together with Nadella’s two governance statements, the pattern is consistent: Microsoft is betting that a plainly worded, publicly reviewable rulebook will land better than a purely technical framework.

## What It Means for Developers and Enterprises

For developers building on the MAI models, the code is an early signal of behavioral guardrails that could eventually show up as API-level restrictions. Security teams should watch how the “no cyberattacks” constraint is operationalized. A broad ban is easy to state and harder to enforce inside a general-purpose coding or reasoning model that can legitimately write exploit code for penetration testing. If Microsoft publishes technical detail on where that line sits, the code changes day-to-day model behavior. If it publishes only intent, it is mainly a public commitment.

For enterprise IT teams, the more immediate point is Nadella’s emphasis on customer control over data and model weights. Because the code is not binding and the final text is due after the comment window, a sensible approach is to ask vendors for contractual terms on data and weights now, and treat the code as a supporting document rather than a guarantee.

Tech Insider has also covered uncensored DeepSeek Flash builds on Hugging Face and how to set up the Qwen Image API.
