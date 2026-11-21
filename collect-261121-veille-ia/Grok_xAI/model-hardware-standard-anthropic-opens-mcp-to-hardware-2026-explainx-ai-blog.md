---
id: collect-261121-veille-ia/grok-xai/model-hardware-standard-anthropic-opens-mcp-to-hardware-2026-explainx-ai-blog
title: "Model Hardware Standard: Anthropic Opens MCP to Hardware (2026) | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["hardware", "anthropic", "standard", "agent", "device", "physical", "model", "agents"]
source: https://explainx.ai/blog/anthropic-model-hardware-standard-mhs-research-preview-august-2026
source_anchor: ""
source_lines: [1, 101]
section: "Grok_xAI"
---
Anthropic just extended agentic tool use past the browser and the terminal and into the physical world. On August 27, 2026, Anthropic previewed the Model Hardware Standard (MHS) — an open specification, also covered in explainx.ai's AI dictionary entry for Model Hardware Standard, that lets Claude, or any model-agnostic agent, safely discover and operate lab and manufacturing hardware: microscopes, liquid handlers, robotic arms, lasers, even the components of a quantum computer.

If you've followed how MCP standardized the way agents reach software tools, MHS will feel immediately familiar. It's the same "write the integration once, any agent harness can use it" philosophy — except the tool on the other end is a physical instrument that can foam a sample, break a laser alignment, or crush something if the agent gets it wrong.

## TL;DR

| Question | Answer | 
|---|---|
| What is it? | A driver-and-discoverability spec letting agents read from and write to physical lab/manufacturing devices | 
| Who announced it? | Anthropic, Aug 27, 2026, as a research preview — not yet open source | 
| Where did it start? | A collaboration between Anthropic's Alek Kemeny and HHMI Janelia postdoc Arco Bast | 
| How do agents connect? | Three mechanisms: MCP, CLI, and code files/APIs | 
| Is it Claude-only? | No — model-agnostic; any agent harness can access it via standard protocols like MCP | 
| What's the headline result? | QuEra's quantum laser stabilization went from 58% to 99.3% | 
| Biggest limitation? | Claude's physical/spatial reasoning still needs expert human oversight | 
| Can I use it today? | Only via research-preview access with participating labs and manufacturers | 

## The problem: hardware integration takes weeks, not minutes

Most lab and factory instruments don't talk to each other. Wiring a new microscope or liquid handler into a workflow typically means calling in a specialist to build a bespoke, one-off integration — a process that takes weeks to months per device, according to Anthropic. MHS is designed to cut that to hours or minutes, and to let agents orchestrate autonomous, round-the-clock experiments: reasoning through each step, adjusting parameters in real time, and in some cases recovering from hardware errors without a human in the loop.

MHS didn't start as a product initiative. It began with Alek Kemeny, from Anthropic's Beneficial Deployments team, and Arco Bast, a postdoctoral scientist at HHMI Janelia Research Campus. Bast was running brain-imaging experiments on a rig combining lasers, motorized focusers, and specialized cameras from different vendors with no common interface — so he built a shared memory dictionary that let the instruments talk to each other at memory speed. Kemeny and Bast then integrated AI models into that same interface, and MHS grew out of it.

## How MHS actually works

MHS is built around a **standardized driver** — software that translates between an operating system and a hardware device — using two primitives any device can implement:

- **Read** — e.g., get the current temperature
- **Write** — e.g., set the temperature

That's the entire vocabulary an agent needs to start operating a device. On top of read/write, MHS makes each device **discoverable in a standard format**, so devices and agents can find and address each other across a network without a bespoke "translator" program sitting in between — the same discoverability problem MCP solved for software APIs, applied one layer down.

The part that's genuinely new is how MHS handles **physical and safety context an agent can't infer from code alone**. The weight of a robot arm, the safe operating range of a laser, what a device can measure versus what's actually adjustable — that knowledge historically lived in paper manuals or a technician's tacit experience. MHS lets users write it in natural language, either self-authored or produced by chatting with an agent that interviews them about their setup. The output is a reference file covering everything the agent needs: what the device measures, what can be adjusted, and what safety limits are enforced.

Three control mechanisms work together on top of the driver layer:

1. **MCP** — for agent harnesses that already speak the protocol
2. **CLI** — direct command-line control
3. **Code files (APIs)** — for orchestrating multiple devices with a single line of code

Once connected, the agent doesn't babysit every micro-step. It supervises at a high level — sequencing steps across instruments, monitoring results, adjusting parameters in real time — and for long-running tasks, or operations faster than the agent can reason through live, it **chains driver commands into code files** so devices execute deterministically without the agent reasoning at every step.

Anthropic describes Claude interacting with hardware "in an exploratory manner, much as a scientist would" — adjusting a laser, observing the result through a camera, repeating the cycle to learn the sequence, then packaging the learned procedure into a deterministic script so the entire alignment process runs as a single command afterward. That exploration-then-compile pattern is worth remembering: it's the same explore/exploit shape covered in explainx.ai's guide to types of AI agents and in goal-mode agent design — reason step-by-step while uncertain, then collapse the learned procedure into a deterministic script once it's understood.

## Early results: real numbers from real labs

Three partners have already run MHS in production-adjacent settings:

| Partner | Result | 
|---|---|
| **Genentech** | Ran a drug-discovery experiment with real-time error handling — but had to guide Claude to recognize sample-foaming errors as physical failures, not software bugs | 
| **HHMI Janelia Research Campus** | Compressed an imaging experiment from weeks to a day | 
| **QuEra** | Improved laser stabilization on its quantum computers from **58% to 99.3%** | 

The Genentech example is the most instructive one for anyone building agentic pipelines, not just anyone running a lab. Foaming during a drug-discovery experiment is a physical failure mode — surface tension, gas exchange, mechanical agitation — and Claude's first instinct, per Anthropic's own account, was to treat it like a software bug rather than a hardware problem requiring physical correction. That's a concrete, named instance of the limitation explainx.ai's look at why robotics still isn't solved keeps circling back to: models trained on text and images don't have direct physical experience, so their failure diagnosis defaults to the domain they do have experience in.

## The vendor ecosystem building MHS support

Ten named hardware and software vendors are already integrating MHS, spanning robotics, lab automation, and consumer hardware:

| Vendor | What they're building | 
|---|---|
| **AWS** | MHS support through Strands Robots, its AI-agent-to-physical-device library; private pre-release for research-preview participants | 
| **Automata** | MHS in LINQ, its lab automation platform, for intelligent error handling in autonomous labs | 
| **Danaher** | Exploring MHS-supported capabilities for smart instruments and autonomous laboratories at biomedical R&D scale | 
| **Doosan Robotics** | Testing MHS with robotic arms, including automated QA and multi-robot task coordination | 
| **MBF Bioscience** | Building an MHS driver for ScanImage, laser-scanning microscope software used in hundreds of neuroscience labs | 
| **QIAGEN** | Proof of concept on QIAsymphony Connect — agents helping labs troubleshoot instrument issues and guide operator recovery | 
| **Tecan** | Adding MHS support for its Fluent liquid handling platforms | 
| **Universal Robots** | Early access, planning MHS support across its robotics platform | 
| **Hugging Face** | Adding MHS support in LeRobot, its robotics library | 
| **Raspberry Pi** | Enabling MHS integration across products after successful tests with its Camera MHS Driver | 

That's a wider spread than a typical Anthropic launch — it covers cloud infrastructure (AWS), lab automation platforms (Automata, Tecan, QIAGEN), instrument makers (Danaher, MBF Bioscience), collaborative robotics (Doosan, Universal Robots), open-source robotics tooling (Hugging Face), and even hobbyist hardware (Raspberry Pi). For a hardware-robotics angle on where physical AI is heading in parallel, see explainx.ai's coverage of Xynova's direct-drive dexterous hand and the broader World Humanoid Robot Games roundup — MHS is a software/protocol layer, not a new robot body, and it's designed to sit on top of whatever hardware these vendors already ship.

## Why a research preview, not open source, yet

MHS works with any device that has a programmable interface, and it's explicitly model-agnostic — any agent harness can reach it through standard protocols like MCP. Anthropic is sharing this early version with a first group of partners across science, robotics, electronics, and manufacturing specifically to build safety evaluations and best practices for AI operating physical equipment, ahead of an eventual open-source release. Research-preview access is available by applying via Anthropic's site.

The reasoning tracks with what Anthropic states as MHS's honest limitations:

- **Physical reasoning is still weaker than human expertise.** LLMs learn about the physical world from text and images, not direct physical experience — Claude still requires expert oversight, as the Genentech foaming example shows.
- **Programmable-interface hardware only.** MHS doesn't yet work with devices lacking a programmable interface; Anthropic is working with manufacturers to build drivers for more of them.
- **A safety roadmap is still in progress.** Anthropic is developing a "physical safety roadmap" to bolster safeguards, policy, and enforcement against misuse before open-sourcing MHS, and plans to release research-preview findings as safe-deployment guidance alongside that eventual release.

## What this means if you build with AI agents

Even if you'll never touch a liquid handler, MHS is worth studying as a pattern, not just a hardware story:

1. **Driver + discoverability is a reusable abstraction, not a lab-specific trick.** Any time you're standardizing how an agent reaches a category of tools — internal APIs, IoT devices, legacy systems — the read/write-primitive-plus-discoverable-format shape is a smaller, more portable idea than building a bespoke integration per device or endpoint.
2. **Letting an agent interview a human to build its own capability file is a pattern worth stealing for tool descriptions generally.** MHS's natural-language safety-context tags — self-authored or produced by an agent interviewing the operator — are effectively a structured way of writing system-prompt context that most teams still do by hand and inconsistently. The same interview pattern works for documenting internal tools, not just hardware.
3. **The Genentech foaming example is a clean, citable case for why human-in-the-loop still matters.** When you're designing an agentic pipeline with physical or irreversible consequences, don't assume the model will correctly attribute a failure mode outside its training distribution — build in a checkpoint where a human confirms the diagnosis before the agent acts on it.
4. **MCP is one piece of a bigger control stack, not the whole stack.** MHS treats MCP as one of three interchangeable control mechanisms (alongside CLI and code files), which is a useful reminder that MCP standardizes*discovery and invocation* , not every layer an agent needs — for anything with physical or safety stakes, you still need a driver abstraction underneath it.

**Update — September 20, 2026:** Anthropic's Bay Area wet lab is turning out to be MHS's proving ground — follow-up coverage confirms Anthropic is testing whether Claude can instruct robotic lab equipment to run real experiments with minimal human staffing.

## Related reading

**Official source:** Anthropic — "Previewing the Model Hardware Standard" (Aug 27, 2026)

*This post reflects Anthropic's August 27, 2026 research-preview announcement. MHS is not yet open source and access is limited to participating labs and manufacturers — check Anthropic's official page for current access status and specification details before building against it.*
