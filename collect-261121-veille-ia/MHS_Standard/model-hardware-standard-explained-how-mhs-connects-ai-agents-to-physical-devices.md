---
id: collect-261121-veille-ia/mhs-standard/model-hardware-standard-explained-how-mhs-connects-ai-agents-to-physical-devices
title: "Model Hardware Standard explained: How MHS connects AI agents to physical devices"
domain: mhs-standard
role: reference
task: article
actors: []
dates: ["2026-08-27"]
keywords: ["agents", "standard", "model", "hardware", "physical", "devices", "equipment", "driver"]
source: https://medium.com/@DataNorthAI/model-hardware-standard-explained-how-mhs-connects-ai-agents-to-physical-devices-db04d6a13923
source_anchor: ""
source_lines: [1, 167]
section: "MHS_Standard"
---
# Model Hardware Standard explained: How MHS connects AI agents to physical devices

The Model Hardware Standard (MHS) is Anthropic’s shared specification for AI agents to discover, read from and safely operate physical equipment through a standardized driver. That equipment includes microscopes, liquid handlers, robotic arms and lasers. Anthropic launched its research preview on August 27, 2026, for an initial group of research labs and advanced manufacturers.

MHS supports devices with programmable interfaces and works with any AI model through standard protocols, including the Model Context Protocol (MCP).

Integrating lab and factory equipment normally takes specialists weeks or months of custom development. Anthropic says MHS can reduce that work to hours or minutes. This guide examines the design, its relationship to MCP and established standards, early pilot results and what remains ahead. The specification is still private, with no date announced for the open-source release.

## What is the Model Hardware Standard?

MHS combines a specification with a standardized driver between an AI agent and a physical device. It is neither a model nor a robot. It provides a common interface through which agents discover equipment, understand its capabilities and operate within owner-defined limits (Anthropic, August 27, 2026).

Three characteristics define it:

- It is model-agnostic.
- It supports any device with a programmable interface.
- Devices describe themselves and their safety limits, allowing agents to work with unfamiliar hardware.

The project began at HHMI Janelia Research Campus. Postdoctoral researcher Arco Bast created a shared-memory dictionary to connect his microscope’s lasers, focusers and cameras. He then worked with Alek Kemeny from Anthropic’s Beneficial Deployments team to connect AI models to that interface.

Companies developing or testing support include Amazon Web Services through Strands Robots, Automata, Danaher, Doosan Robotics, Hugging Face through LeRobot, MBF Bioscience, QIAGEN, Raspberry Pi, Tecan and Universal Robots.

## Why MHS was created

Hardware integration in labs and factories is slow, costly and highly customized. Giving an AI agent control adds another challenge. Each instrument has its own software, formats and driver, without a common route for sharing data with agents or letting them operate equipment safely.

Partner reports illustrate the problem. One Janelia microscope setup required seven vendor applications in three programming languages, started in a fixed sequence. At Carnegie Mellon, three instruments ran on separate computers: one accepted job files through a folder, another exposed an old COM scripting interface, and the third offered only a GUI. Important physical knowledge, such as a robot arm’s weight, often remains in manuals or an engineer’s memory.

Without a shared standard, each agent-device pairing requires a connector. Integration effort grows with agents multiplied by devices. A standard changes that to agents plus devices: one driver for each device and one protocol for each agent. MCP brought this approach to software tools in 2024. MHS extends it to physical equipment.

## How MHS works

A standardized driver wraps each device, exposing a small command set, describing the equipment consistently and enforcing safety limits. Agents communicate with MHS, which communicates with the hardware, rather than addressing vendor software directly.

### 1. Read and write primitives

Every driver provides the same basic commands, principally read, such as getting a temperature, and write, such as setting one. Agents no longer need a different API for every instrument.

### 2. Discovery

Devices announce themselves in a standard format. Agents and equipment can find one another across networks without custom translation software.

### 3. Physical context and safety limits

Users describe equipment in plain-language driver tags, including characteristics such as arm weight or a laser’s safe power range. An agent can interview the user to gather this information. The driver produces a reference file covering measurements, adjustable settings and enforced limits.

The driver enforces those boundaries independently of the model. Out-of-range commands are blocked regardless of the agent’s request. Carnegie Mellon tested six deliberately induced faults, including a missing plate and an active emergency stop. All six were blocked before movement occurred.

### 4. Three control paths

### 5. Live reasoning followed by deterministic code

An agent can coordinate instruments, monitor results and adjust parameters in real time. For long-running tasks or operations faster than its reasoning allows, it can write code that runs independently.

In Anthropic’s tests, Claude repeatedly adjusted a laser and observed the result through a camera. After learning the pattern, it wrote a script that aligned the laser with a single command.

## MHS and the Model Context Protocol

MHS and MCP work together. MCP connects agents to software and data, while MHS connects them to equipment. MCP is one of three routes to an MHS device.

Organizations already using MCP-based agents do not need to replace them. MHS adds a hardware layer beneath MCP. An agent reading an inventory system could also read the production-line sensor supplying its data.

## Early MHS examples and use cases

Six partners shared results at launch, mainly from life science labs. The clearest reported gains concern integration time and reliability.

QuEra’s work illustrates the approach. A recovery script developed over several months succeeded 58% of the time. Four Claude instances improved it overnight through hundreds of iterations. The resulting production script runs without an agent.

These are partner-reported pilots, using small samples and each team’s own baseline. Manufacturing results have not yet been published. Doosan Robotics is testing quality assurance across several robots, while QIAGEN is exploring instrument troubleshooting.

## Benefits of MHS

The main benefit is avoiding repeated integration work as equipment is added.

- **Faster setup:** pilots report hours or days instead of weeks or months.
- **A shared interface across vendors and languages:** analysis code can be written for each data type rather than each instrument.
- **Reusable drivers:** teams can share them; Carnegie Mellon plans to publish its drivers.
- **Continuous operation within limits:** agents monitor experiments and recover from routine errors while drivers enforce safety boundaries.
- **Expertise captured in code:** teams can read, test and run proven sequences without an agent.

Whether quicker integration produces faster discovery at scale remains unproven.

## Limitations, security and safety

MHS remains an early preview. Its largest constraint is the model itself: language models still struggle to reason about physical reality.

- **Physical reasoning:** at Genentech, Claude retried a pipetting operation that failed because of bubbles, creating more bubbles until researchers explained the cause. At QuEra, it could not diagnose hardware failures.
- **Interface requirements:** equipment without a programming interface is not yet supported. Anthropic is working with manufacturers on built-in drivers.
- **Supervision and context:** QuEra’s agent frequently waited for approval, sometimes overnight, and needed extensive context.
- **Cost and maturity:** continuous agents consume compute, and complex protocols still require substantial optimization.

### Security

Discoverable, controllable equipment also presents a target. Anthropic is developing a physical safety roadmap and intends to publish findings with the open-source release. Until then, treat MHS equipment as operational technology: separate network segments, least-privilege access and logging for every command.

### European regulation

The EU Machinery Regulation (EU) 2023/1230 applies from January 20, 2027, covering AI-based safety functions for the first time. As The Next Web notes, an MHS file restricting a robot’s speed and angles arguably performs a safety function. For high-risk categories, self-declaration of conformity is no longer sufficient. Bring compliance and machine safety specialists into any planned EU production-machinery deployment early.

## Getting started with MHS

Access requires an application through modelhardwarestandard.com. The initial audience is research labs and advanced manufacturers. Eligibility criteria, pricing and a general availability date have not been published.

### Is it worth exploring now?

### Planning a first pilot

These steps draw on partner reports; preview documentation may differ.

1. List your devices and the interfaces they expose.
2. Choose a small workflow with a measurable baseline, such as monitoring a long run.
3. Install or develop each device’s driver and document its characteristics and safety limits. Review the reference file before allowing movement.
4. Begin with read-only access. Introduce write access with human approval for high-risk actions.
5. Convert proven sequences into code files and compare their performance with the previous baseline.

## How MHS compares with existing hardware standards

MHS complements established standards. SiLA 2, OPC UA/LADS and ROS 2 generally expect a programmer or scheduler to control the device. MHS expects an AI agent to determine how to use it.

MHS adds physical context written for agents, safety limits that accompany the device and a direct route from live reasoning to deterministic code. It does not yet offer the maturity or openness of established alternatives.

These options are unlikely to require an either-or decision. Carnegie Mellon wrapped a COM interface, a job folder and even a GUI in MHS drivers. A SiLA 2 server or OPC UA endpoint is therefore likely another interface a driver could use. Existing investment in those standards should make the driver easier to build.

## What comes next for AI-controlled hardware?

Anthropic plans to release MHS as open source after the preview, together with safety findings and deployment guidance. It also intends to support more equipment, including boards and cameras, and work with manufacturers on built-in drivers. Genentech wants end-to-end autonomous workflows; QuEra wants to deploy its laser controller on live quantum processors.

Four developments will help determine whether MHS becomes an industry standard:

1. The open-source release date and license.
2. Support from other AI vendors’ agent frameworks, testing whether MHS grows beyond Anthropic’s ecosystem.
3. Drivers shipping in products from Tecan, Universal Robots and other vendors.
4. The treatment of MHS files under the EU Machinery Regulation from 2027.

## Frequently asked questions

### What is the Model Hardware Standard?

It is an Anthropic specification giving agents a consistent interface to physical equipment. Devices receive standardized drivers with read and write commands, descriptions including safety limits, and network discovery. Agents access them through MCP, a command line or code.

### How does MHS differ from MCP?

MCP links agents to software and data. MHS links them to hardware, adding physical context, device-level limits and code files for fast, repeatable operations. MCP is one route to MHS devices.

### Is MHS open source?

Not yet. Only accepted preview participants can access the specification. Anthropic plans an open-source release after the preview but has not announced a date.

### Does MHS support models other than Claude?

Its design allows any agent harness to connect through standard protocols such as MCP. However, every published pilot used Claude, so public evidence for other models is still missing.

### How can developers get access?

Apply through modelhardwarestandard.com. Accepted participants also receive a private pre-release of the AWS Strands Robots library, which supports MHS.

### Is MHS safe for physical equipment?

Drivers enforce owner-defined limits regardless of agent requests. Models still struggle with physical reasoning, so expert oversight and human approval for risky operations remain necessary.

## Conclusion

MHS gives agents a shared way to discover, understand and operate equipment. Early partners integrated devices in hours or days rather than weeks. A public specification, open-source release, other AI vendors’ support and results beyond labs are still missing, as are agents that reliably understand physical failures without assistance.

If disconnected equipment from different suppliers creates a real bottleneck, apply for the preview and test one small, measurable workflow. Otherwise, follow the open-source release.

*Originally published by* *Jorick van Weelie, DataNorth**, on* *20 October 2026**:* *Model Hardware Standard (MHS): What it is, How it works, and Why it matters**.*
