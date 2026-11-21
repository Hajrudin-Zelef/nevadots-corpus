---
id: collect-261121-veille-ia/mhs-standard/model-hardware-standard-mhs-complete-guide
title: "Model Hardware Standard (MHS): Complete Guide"
domain: mhs-standard
role: reference
task: article
actors: []
dates: []
keywords: ["device", "standard", "agent", "model", "hardware", "driver", "devices", "every"]
source: https://datanorth.ai/blog/model-hardware-standard-mhs-what-it-is-how-it-works-and-why-it-matters
source_anchor: ""
source_lines: [1, 198]
section: "MHS_Standard"
---
**The Model Hardware Standard (MHS) is a shared specification from Anthropic that lets AI agents discover, read from and safely operate physical devices, such as microscopes, liquid handlers, robotic arms and lasers, through one standardized driver.** Anthropic opened MHS as a research preview on August 27, 2026, for a first group of research labs and advanced manufacturers. It works with any device that has a programmable interface and with any AI model, through standard protocols such as the Model Context Protocol (MCP).

Connecting lab and factory equipment usually takes specialists weeks or months of custom work. Anthropic says MHS cuts that to hours or minutes. This guide explains how MHS works, how it compares to MCP and existing standards, what the pilots showed, and what is still planned: the specification is not yet public and the open-source release has no date.

## What is the Model Hardware Standard (MHS)?

MHS is a specification plus a standardized driver that sits between an AI agent and a physical device. It is not a model and not a robot, but the common layer that lets any agent find a device, understand what it can do and operate it within limits the owner sets (Anthropic, August 27, 2026).

Three properties set it apart:

- MHS is model-agnostic,
- MHS works with any device that has a programmable interface,
- Each device describes itself, including its safety limits, so an agent can use hardware it has never seen before.

MHS began at HHMI Janelia Research Campus. Postdoc Arco Bast built a shared-memory dictionary so the lasers, focusers and cameras on his microscope could talk to each other, and Alek Kemeny of Anthropic’s Beneficial Deployments team worked with him to connect AI models to it.

Vendors building or testing MHS support include Amazon Web Services (Strands Robots), Automata, Danaher, Doosan Robotics, Hugging Face (LeRobot), MBF Bioscience, QIAGEN, Raspberry Pi, Tecan and Universal Robots.

## Why was the Model Hardware Standard created?

Connecting lab and factory hardware is slow, expensive and bespoke, and it gets harder once an AI agent needs to operate the equipment. Every device ships with its own software, data format and driver, there is no common way to share data with an agent, and no common way to let an agent operate devices safely.

The partner reports show how deep this goes. One Janelia microscope rig needed seven vendor programs in three programming languages, launched in a fixed order. At Carnegie Mellon, three instruments sat on three computers: one read job files from a folder, one had only an old COM scripting interface, and one had nothing but a GUI. Knowledge about safe operation, such as the weight of a robot arm, usually lives in a paper manual or in one engineer’s head.

Without a standard, every pairing of agent and device needs its own connector, so integration work grows with agents times devices. A shared standard turns that into agents plus devices: one driver per device, one protocol per agent. MCP did this for software tools in 2024; MHS applies the same idea to physical equipment.

## How does the Model Hardware Standard work?

MHS wraps every device in a standardized driver that speaks a small set of commands, describes the device in a standard format, and enforces its safety limits. The agent never talks to vendor software directly: it talks to MHS, and MHS talks to the hardware.

The agent works through MCP, the command line or code files. Every command passes through the device’s MHS driver, which enforces its safety limits, and device state streams back up to the agent.

### 1. Read and write primitives

The MHS driver gives every device the same small command set, mainly **read** (get the temperature) and **write** (set the temperature). The agent does not have to learn a new API per instrument.

### 2. Discovery

Each device announces itself in a standard format, so agents and devices find each other across a network without a custom translator in between.

### 3. Physical context and safety limits

Users describe a device in plain language in the driver’s tags, such as a robot arm’s weight or a laser’s safe power range, or let an agent interview them. The driver turns this into a reference file: what the device can measure, what can be adjusted and which limits are enforced.

The driver enforces those limits, not the model: a command outside them is blocked, whatever the agent asks. Carnegie Mellon tested this with six deliberately induced faults, including a missing plate and an active emergency stop, and the system blocked all six before anything moved.

### 4. Three control paths

| Control path | Typical use | 
|---|---|
| MCP | Agents and chat clients that already use MCP | 
| Command line | Coding agents and engineers testing a device step by step | 
| Code files (APIs) | Long-running, repeatable or time-critical sequences | 

### 5. Live reasoning, then deterministic code

The agent can supervise live: sequence steps across instruments, watch results and adjust parameters. When a task runs too long or needs to be faster than the model can reason, the agent writes a code file that runs on its own. In Anthropic’s tests, Claude adjusted a laser, checked the result through a camera until it understood the pattern, then wrote a script that aligns the laser in one command.

## Model Hardware Standard vs. Model Context Protocol (MCP)

MHS and MCP are complements. MCP connects AI agents to software and data; MHS connects them to physical devices, and MCP is one of the three ways to reach an MHS device.

|  | MCP | MHS | 
|---|---|---|
| Connects agents to | Software tools and data sources | Physical devices with a programmable interface | 
| Introduced | November 2024 | August 2026 (research preview) | 
| Status | Open standard, widely adopted | Gated preview, open source planned | 
| Context for the model | Tool names and descriptions | Physical characteristics and safe ranges in plain language | 
| Safety enforced in | Client and server permissions | The device driver | 
| Control speed | Every call passes through the model | Live calls, plus code files that run without the model | 

If your organization already runs MCP-based agents, MHS replaces nothing. It adds the layer below MCP: the agent that reads your inventory system could also read the sensor on the production line that feeds it.

## Real-world examples and use cases of MHS

Six partners published early results with the launch, mostly in life science labs. The strongest numbers are about integration time and reliability.

| Partner | What they did with MHS | Reported result | 
|---|---|---|
| Carnegie Mellon University | Dose-response experiments across a liquid handler, plate reader, robot arm and cameras | About 3x faster; integrated in about 8 hours instead of weeks; the agent rejected a poor curve fit and reran it without human input | 
| Genentech | Protein assay across three instruments; Claude tuned liquid flow rates | Found flow rates that automation experts confirmed; recovered from routine pipetting errors | 
| HHMI Janelia | Unified a microscope rig that needed seven vendor programs | Adding a camera took minutes instead of days; one click to start an experiment instead of seven steps | 
| QuEra Computing | Automatic laser recovery inside a quantum computer | Success rose from 58% to 99.3% in 700 blind trials; recovery in seconds instead of 5 to 10 minutes | 
| Tetsuwan Scientific | qPCR workflow with camera-based bubble detection | Pipetting precision model about 12% more accurate than the manufacturer’s specification | 
| University of Washington | Remote monitoring, agent-supervised qPCR, robot arm plate handoffs | Six instruments connected in under a week, drivers included | 

QuEra’s case shows the method best. A recovery script that a team had built over several months worked 58% of the time. Four Claude instances improved it overnight in hundreds of cycles, and the result is a plain script that runs in production without any agent.

These are pilots reported by the partners themselves, with small samples and each team’s own baseline. Manufacturing has no published results yet: Doosan Robotics is testing quality assurance across multiple robots and QIAGEN is testing instrument troubleshooting.

## Benefits of the Model Hardware Standard

The core benefit is that the cost of connecting hardware stops growing with every new device.

- **Faster integration:** hours or days instead of weeks or months in the pilots.
- **One interface across vendors and languages** , so analysis code is written once per data type instead of once per device.
- **Reusable drivers** that can be shared; Carnegie Mellon plans to publish its drivers.
- **Round-the-clock operation within limits:** agents monitor runs and recover from routine errors while the driver enforces safety limits.
- **Expertise captured in code** that a team can read, test and run without an agent.

Still unproven: whether faster setup turns into faster discovery at scale.

## Limitations, security, and safety considerations

MHS is an early preview, and its biggest limitation is the model behind it: language models still reason poorly about physical reality.

- **Weak physical reasoning.** At Genentech, Claude kept retrying a pipetting step that failed because of bubbles, which only created more bubbles, until researchers explained the physical cause. At QuEra, Claude could not troubleshoot hardware failures.
- **A programming interface is required.** Devices without one are not supported yet; Anthropic is working with manufacturers on built-in drivers.
- **Supervision and context.** QuEra’s agent often paused for human approval, sometimes overnight, and needed extensive context to perform well.
- **Cost and maturity.** Running an agent continuously costs compute, and complex protocols still need significant optimization.

### Security

Devices that an agent can discover and control are also targets. Anthropic is developing a physical safety roadmap and will publish its findings at the open-source release. Until then, treat MHS devices as operational technology: separate network segments, least-privilege access and a log of every command.

### Regulation in Europe

The EU Machinery Regulation (EU) 2023/1230 applies from January 20, 2027, and for the first time covers AI-based safety functions. As The Next Web points out, an MHS file that limits a robot arm’s speed and angles arguably performs a safety function, and for high-risk categories self-declaration of conformity is no longer enough. Involve compliance and machine safety experts early if you plan to use MHS on production machinery in the EU.

## How to get started with Model Hardware Standard

Access is by application only: apply for the research preview on modelhardwarestandard.com. The first group is research labs and advanced manufacturers; eligibility criteria, pricing and a general availability date have not been published.

### Is MHS worth exploring now?

| Your situation | Recommendation | 
|---|---|
| Several machines from different vendors with a lot of manual coordination | Strong candidate: apply and pick one contained workflow | 
| You already run MCP-based agents | Low barrier: start with a read-only monitoring pilot | 
| A mature, validated automation stack | Little to gain today: follow the open-source release | 
| No programmable interface on your equipment | Not yet: ask your vendor about MHS plans | 
| Regulated production, such as GMP or safety-critical machinery in the EU | Keep it in R&D until safety guidance and regulation are clear | 

### A practical first pilot

Based on the partner reports; the preview documentation may differ.

1. **Map your devices** and the interface each one exposes.
2. **Pick one small workflow** with a clear baseline, such as monitoring a long run.
3. **Install or write a driver per device** and describe its characteristics and safety limits; review the reference file before anything moves.
4. **Connect the agent read-only first** , then add write access with human approval for high-risk actions.
5. **Turn proven sequences into code files** and measure them against your old baseline.

## Model Hardware Standard vs. Existing Hardware Standards

MHS does not replace existing standards. SiLA 2, OPC UA/LADS and ROS 2 assume a programmer or scheduler on the other end; MHS assumes an AI agent that has to work out how to use a device on its own.

| Standard | Origin | Built on | Main user | Status | 
|---|---|---|---|---|
| SiLA 2 | SiLA consortium, announced 2016 | gRPC, mDNS discovery | Lab automation software | Open standard, in use | 
| OPC UA with LADS | OPC Foundation, SPECTARIS and VDMA; LADS since December 2023 | OPC UA information models | Lab, MES and industrial systems | Open standard, in use | 
| ROS 2 | Open robotics community | DDS middleware | Robotics developers | Open source, widely used | 
| MHS | Anthropic and HHMI Janelia, August 2026 | Standardized read and write driver | AI agents via MCP, CLI or code | Gated research preview | 

What MHS adds is physical context written for an agent, safety limits that travel with the device, and a built-in path from live reasoning to deterministic code. What it lacks is the maturity and openness of the others.

You probably do not have to choose. Carnegie Mellon wrapped an old COM interface, a job folder and even a GUI in MHS drivers, so a SiLA 2 server or OPC UA endpoint is likely just another interface an MHS driver can sit on. Investments in those standards make the MHS driver easier to write.

## The future of MHS and AI-controlled hardware

Anthropic has announced an open-source release after the preview, with safety findings and deployment guidance. It also plans to cover more device types, such as boards and cameras, and to build drivers into devices together with manufacturers. Partners have next steps of their own: Genentech wants end-to-end autonomous workflows and QuEra wants its laser controller on live quantum processors.

Whether MHS becomes an industry standard depends on what happens next. Watch for:

1. The open-source date and license.
2. Support from agent frameworks of other AI vendors, the real test of whether this is a standard or an Anthropic ecosystem.
3. MHS drivers shipping in products from Tecan, Universal Robots and other vendors.
4. How MHS files are treated under the EU Machinery Regulation from 2027.

## Frequently Asked Questions About MHS

## **What is the Model Hardware Standard (MHS)?**

An Anthropic specification that gives AI agents one consistent way to work with physical equipment. Each device gets a standardized driver with read and write commands, a description that includes its safety limits, and network discovery. Agents operate it through MCP, a command line or code.

## **How is MHS different from MCP?**

MCP connects AI agents to software and data. MHS connects them to physical devices and adds physical context, device-level safety limits and code files for fast, repeatable control. MCP is one of the ways to reach an MHS device.

## **Is Model Hardware Standard open source?**

Not yet. The specification is only available to accepted preview participants. Anthropic plans to open-source it after the preview but has not given a date.

## **Can MHS work with AI models other than Claude?**

Yes, by design: any agent harness can connect through standard protocols such as MCP. All published pilots used Claude, so there is no public evidence yet for other models.

## **How can developers access MHS?**

Apply through the form on modelhardwarestandard.com. Accepted participants also get a private pre-release of the AWS Strands Robots library, which supports MHS.

## **Is Model Hardware Standard safe for physical devices?**

The driver enforces the safety limits the owner sets, whatever the agent requests. But agents still reason poorly about physical problems, so expert oversight and human approval for risky actions remain necessary.

## Conclusion

MHS gives AI agents a standard way to find, understand and operate physical equipment, and early partners connected devices in hours or days instead of weeks. Still missing are a public specification, the open-source release, support from other AI vendors, results outside the lab, and agents that understand physical failures without help. If disconnected equipment from different vendors is a real bottleneck for you, apply for the preview and run one small, measurable pilot. If not, follow the open-source release.
