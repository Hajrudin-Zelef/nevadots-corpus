---
id: collect-261121-veille-ia/mhs-standard/mhs-model-hardware-standard-ai-glossary
title: "MHS (Model Hardware Standard) - AI Glossary"
domain: mhs-standard
role: reference
task: article
actors: []
dates: []
keywords: ["model", "hardware", "standard", "anthropic", "physical", "equipment", "agent", "specification"]
source: https://www.thequery.in/glossary/mhs
source_anchor: ""
source_lines: [1, 70]
section: "MHS_Standard"
---
# MHS (Model Hardware Standard)

Agents & Workflows
Model Hardware Standard, an Anthropic research-preview specification for connecting AI agents to programmable physical equipment through a common interface.

MHS, short for Model Hardware Standard, is an Anthropic research-preview specification intended to give AI agents a common way to discover, understand, and operate programmable physical equipment. Anthropic introduced it on August 27, 2026 for laboratory instruments and advanced manufacturing systems such as microscopes, liquid handlers, robotic arms, cameras, plate readers, and quantum-computer laser controls.

The word "standard" needs a qualification. MHS is available only to approved research-preview partners. Anthropic has not published the specification, source repository, license, conformance tests, governance process, or enough implementation detail for an outside developer to build and verify an independent compatible driver. Anthropic says it plans to make MHS open source after the preview. Until that happens, MHS is more accurately described as a proposed standard supported by private implementations and partner demonstrations.

## How MHS is supposed to work

Laboratory and manufacturing devices commonly expose different APIs, software development kits, command languages, and vendor applications. Connecting three machines can require three separate integrations plus additional code to coordinate their states. MHS proposes a driver layer that translates those device-specific interfaces into a shared description an agent can use.

The driver describes what a device can measure, what settings can be changed, which procedures it can perform, and which limits must not be crossed. Anthropic describes simple primitives such as "read" for retrieving a temperature and "write" for changing a set point. Natural-language tags can add physical information that may not exist in an API, such as the weight of a robotic arm or the operating limits of an instrument.

Think of MHS as a universal control panel installed above a room full of machines with different switches. The control panel does not replace the wiring or motors inside each machine. It gives the operator one consistent place to see their state, issue allowed commands, and enforce the limits attached to each device.

An agent can reach the MHS layer through MCP, a command-line interface, or ordinary code. For operations that need to run faster than a language model can reason, the agent can assemble deterministic code from driver commands and let the equipment execute that program without asking the model to decide every step in real time.

## MHS vs MCP

MHS and the Model Context Protocol solve related problems at different layers. MCP defines how an AI application discovers and calls software tools, data sources, and services. MHS is intended to normalize the physical devices beneath those tools by describing their states, procedures, physical characteristics, and safety boundaries.

| Question | MCP | MHS | 
|---|---|---|
| Primary target | Software tools, services, and data | Programmable physical equipment | 
| Main abstraction | Tools, resources, and prompts | Device states, procedures, commands, and limits | 
| Typical use | Query a database or call an application | Read a sensor, move an arm, or change an instrument setting | 
| Public status | Open specification under vendor-neutral governance | Application-only research preview | 
| Relationship | Can carry an agent's requests | Can expose hardware capabilities through MCP | 

Calling MHS "MCP for hardware" is a useful introduction, but it is incomplete. MCP can tell an agent that a hardware-control tool exists and provide a way to call it. MHS is supposed to make the underlying equipment legible and consistent enough for that call to work across devices from different manufacturers.

## What the preview has demonstrated

Anthropic's launch material includes pilots from Genentech, HHMI Janelia Research Campus, QuEra Computing, the University of Washington, and other research and manufacturing partners. These are real implementations on physical equipment, not simulated examples, but the results remain reports from Anthropic and participating organizations rather than independent evaluations of a public standard.

At Genentech, MHS coordinated a liquid handler, robotic arm, and plate reader for a protein-assay workflow. Anthropic says the path from non-automated equipment to a completed dilution curve, including an autonomous rerun, took eight hours rather than the multiple weeks normally required for a vendor-built automation setup. The pilot also exposed a limit: Claude initially treated bubble-related liquid-handling failures like software errors and repeated the action that caused them. Researchers had to explain the physical cause before that knowledge could be turned into a reusable skill.

At Janelia, MHS unified a microscopy rig that had previously depended on seven vendor programs written in different languages. A researcher reported that adding a camera and feeding its output back into the laser-alignment system took minutes instead of becoming another multi-day integration project.

QuEra's pilot provides the clearest quantitative result. Claude used MHS to develop and test a laser-recovery controller for quantum-computing hardware. QuEra reports that the controller recovered the laser correctly in 695 of 700 timed trials across seven fault classes, or 99.3%, with most recoveries taking less than six seconds. A human specialist normally needed five to ten minutes. The operational distinction matters: Claude explored the system and wrote the controller, but the finished recovery system is a conventional, inspectable program. The model is not improvising every adjustment while the production laser runs.

## The safety model

MHS is designed to keep physical limits outside the model's discretion. Drivers can declare bounds, interlocks, and emergency-stop conditions that apply regardless of what an agent requests. Anthropic says one pilot artificially induced six unsafe or unavailable conditions, including a missing plate, rotated plate, disconnected camera, busy reader, unreachable device, and active emergency stop. The system blocked all six before any equipment moved.

That is the right architecture, but it is not yet independently auditable. Without the specification and reference implementation, outside reviewers cannot determine how permissions are represented, which component enforces a limit, how conflicting device states are handled, or whether a driver can accidentally expose a command that bypasses a safeguard.

MHS also does not replace the fast control loops already built into industrial and scientific equipment. A language model is too slow and unpredictable to stabilize a laser at microsecond timescales or control a motor servo directly. Its useful role is higher-level planning, fault diagnosis, experimental iteration, and generating deterministic control programs that run inside separately enforced safety boundaries.

## Applications

The immediate applications are in environments where several programmable machines must cooperate and where integration work consumes scarce specialist time. That includes automated drug-discovery experiments, microscopy, remote instrument monitoring, robotic sample handling, fault recovery, manufacturing quality assurance, and calibration of quantum-computing subsystems.

MHS could also make hardware more discoverable to an agent harness. Instead of hard-coding every machine into the harness, the agent could inspect a device manifest, learn which procedures are available, and assemble a workflow across several devices. Tool calling would then move from invoking software functions to coordinating actions with physical consequences.

The approach only works with equipment that already has a programmable interface or a vendor willing to build one. It cannot turn an entirely manual machine into an autonomous device by describing it in a manifest. Someone still has to write and validate the driver, connect the sensors and actuators, define the safe limits, and maintain the integration when the equipment changes.

## Why MHS matters

If MHS becomes open, implementable, and widely adopted, it could reduce the repeated integration work that keeps laboratory and industrial automation fragmented by vendor. A shared hardware layer would also let different models and agent systems operate the same equipment without rebuilding the entire stack for each provider.

That outcome depends on more than Anthropic publishing code. A credible standard needs stable semantics, versioning, conformance tests, security rules, independent implementations, a process for resolving incompatible changes, and governance that does not leave every important decision with one model company. MCP became an industry standard because outside companies could inspect it, implement it, and eventually participate in vendor-neutral governance. MHS has not reached that stage.

For now, MHS is a promising hardware-integration system with unusually concrete pilot results. The standard itself is still the part nobody outside the preview can see.

## References & Resources

Last updated: August 28, 2026
