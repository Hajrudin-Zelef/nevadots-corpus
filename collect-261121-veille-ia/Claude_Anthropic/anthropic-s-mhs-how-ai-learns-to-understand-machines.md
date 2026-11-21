---
id: collect-261121-veille-ia/claude-anthropic/anthropic-s-mhs-how-ai-learns-to-understand-machines
title: "Anthropic's MHS: How AI Learns to Understand Machines"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-08-27"]
keywords: ["anthropic", "does", "model", "standard", "without", "agent", "understand", "device"]
source: https://igromi.com/en/blog/anthropic-mhs-ai-understand-machines
source_anchor: ""
source_lines: [1, 73]
section: "Claude_Anthropic"
---
**The Model Hardware Standard (MHS) is Anthropic's bet that an AI agent can find a machine, understand what it measures, what can be adjusted and what its limits are, and give it commands without a custom program for every device.** It was announced on August 27, 2026 as a research preview. In practice, it takes the idea the Model Context Protocol (MCP) brought to software into the physical world.

This note explains what the standard includes according to Anthropic's official announcement, what it does not cover yet and what it means for a plant in Chile, where almost everything goes through a PLC.

## What problem does MHS solve? #

Today every device speaks its own language. A microscope, a robotic arm and a plate reader each come with their vendor's program, and connecting them requires hand-written translators. Anthropic says that at HHMI Janelia, where the standard started, a single microscopy rig used seven programs from different vendors.

For an AI the problem is the same, multiplied. A language model does not know what register 40012 is or whether an arm can lift five kilos. Without that description, the agent guesses. MHS proposes that the description exists, in a common format, next to each device.

## How does MHS work? #

According to the announcement, the standard has four parts:

- **One driver per device.** It translates between the computer and the machine with two basic operations: read and write. "Get temperature", "set temperature". It is written once and reused.
- **A device description.** The person who knows the machine describes it in natural language, directly or by answering an agent's questions. MHS turns that into a reference file: what it measures, what can be adjusted and what safety limits apply. Anthropic's example is the weight of a robotic arm, something software cannot see.
- **Discovery.** Devices announce themselves in a standard format, so the agent finds them on the network without custom translators.
- **Three control paths.** MCP, a command line and code files. Code files chain commands so a multistep sequence runs without the model reasoning at each step.

That last point matters most for industry: the agent builds the procedure, but what runs afterwards can be a normal script, repeatable and reviewable, with no language model in the loop.

## Who is testing it? #

Anthropic names AWS, Doosan Robotics, QIAGEN, Tecan, Universal Robots, Hugging Face and Raspberry Pi as partners or early adopters, and Genentech, Carnegie Mellon, the University of Washington and QuEra as test cases. The figures it publishes come from those projects and are reported by Anthropic itself: integrations that go from weeks to hours, and an agent-built controller that recovers a laser lock 99.3 % of the time without human intervention. There is no independent measurement yet.

The standard is model-agnostic: it does not require Claude.

## What does it not cover yet? #

- **Hardware without a programming interface.** The announcement says so explicitly. A scale that only shows the number on its screen is left out.
- **Industrial protocols.** The announcement does not mention PLCs, Modbus, OPC UA or EtherNet/IP. The examples come from labs and robotics.
- **Physical reasoning.** Anthropic acknowledges that the model learns about the world through text and images and needs expert oversight. In the Genentech case, researchers had to explain that foam in a liquid was a physical problem, not a software bug.
- **Availability.** It is a preview with a waitlist at modelhardwarestandard.com. Anthropic plans to open-source it, with no date.

## What does it mean for a plant in Chile? #

In a food or manufacturing plant, machines do not connect like a lab microscope. They talk through a PLC, often fifteen or twenty years old, and what each memory address means is rarely documented. That is where the real work is, and MHS names it well: **before giving an order, the AI has to understand the machine**.

That description, what it measures, what can be adjusted, what the limits are, is what a plant usually does not have written down. While MHS drivers for PLCs arrive, three things can move forward:

1. **Read before writing.** Connect the AI read-only to the PLC and the cameras, and measure: how much was produced, when it stopped, how much energy it used. That is what local AI on PLC and SCADA data can do without touching control.
2. **Write the description of each machine.** Normal ranges, limits, which variable leads. It is the same content MHS asks for and it stays useful even if the standard changes.
3. **Leave safety where it is.** Interlocks and safety logic stay in the PLC. If an agent one day proposes an adjustment, a person approves it and it is recorded.

Order matters because a mistake in the physical world is not undone with a click. The standard itself pushes in that direction: procedures that end up as reviewable scripts, not as model decisions at every step.

## Where does iGromi stand? #

The iGromi engine reads the PLC and the camera on the same node, inside the plant, and today works read-only: it counts production and measures stops, energy and temperature. We do not send orders to machines. We follow MHS closely because it solves, in a common format, the step that takes the most time in every project: understanding what each machine is saying. To see how AI is applied today without touching control, start with the secure industrial artificial intelligence guide.

## Frequently asked questions #

### What is Anthropic's MHS?

A specification announced on August 27, 2026 so AI agents can discover physical equipment, understand what it measures and allows, and send it commands through a common interface. It is in a research preview.

### Does MHS only work with Claude?

No. It is model-agnostic and can be used through MCP, a command line or code.

### Does MHS work with PLCs, Modbus or OPC UA?

The announcement does not mention them and says it does not yet cover hardware without a programming interface. A plant with PLCs has to wait for specific drivers. Meanwhile, see OPC UA vs Modbus.

### Can I use MHS in my plant today?

Only within the preview, with a waitlist at modelhardwarestandard.com. Open source has been announced, with no date.

### Is it safe for an AI to give orders to a machine?

It depends on where the limits sit. Anthropic acknowledges that the model's physical reasoning is limited and that expert oversight is needed. In a plant, safety stays in the PLC and the interlocks.

*Sources: Anthropic's announcement of Aug 27, 2026, CNBC, The Decoder and IBM Think. Checked on Nov 1, 2026.*
