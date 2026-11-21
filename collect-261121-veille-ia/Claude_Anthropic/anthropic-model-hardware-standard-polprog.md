---
id: collect-261121-veille-ia/claude-anthropic/anthropic-model-hardware-standard-polprog
title: "Anthropic Model Hardware Standard | POLPROG"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["model", "anthropic", "hardware", "standard", "equipment", "preview", "work", "does"]
source: https://polprog.pl/en/learning/model-hardware-standard-anthropic-mhs-mcp-ai-agents-hardware
source_anchor: ""
source_lines: [1, 861]
section: "Claude_Anthropic"
---
On 27 August 2026 Anthropic opened a **research preview of the Model Hardware Standard (MHS)**, a shared specification intended to let AI agents discover, describe and operate programmable physical equipment more safely.<sup>[1]</sup>

MHS began as a collaboration between Anthropic and **HHMI Janelia Research Campus**. Early deployments have connected microscopes, liquid handlers, robotic arms, plate readers and parts of the laser system of a quantum computer.<sup>[1]</sup>

This matters because most widely used AI agents have so far operated mainly in a digital world:

```
files
repositories
terminal
APIs
browser
databases
SaaS systems
```
MHS attempts to standardise another layer:

```
sensors
cameras
microscopes
lab robots
robotic arms
lasers
measurement equipment
manufacturing equipment
```
The announcement should not be described as “Claude can now control every machine”. **MHS is currently a limited research preview, access is application-based, and the standard has not yet been publicly released as open source.** Anthropic says it intends to open-source MHS after the preview and additional safety work, but as of 30 August it has not published a release date or a licence for that future open version.<sup>[1]</sup><sup>[2]</sup>

MHS is also not a new AI model and **does not replace the Model Context Protocol (MCP)**. Anthropic describes MHS as model-agnostic, and MCP is one of three mechanisms through which an agent can control MHS-described hardware. The other two are a CLI and code files/APIs.<sup>[1]</sup>

**The main conclusion:** MHS is neither “MCP 2.0” nor a robot. It is an attempt to create a common driver, device-description and physical-safety layer through which an AI agent can work with many different machines via a more consistent interface.


**Information status: 30 August 2026.**

## TL;DR

| Question | Verified answer | 
|---|---|
| What did Anthropic announce? | A research preview of Model Hardware Standard | 
| When? | 27 August 2026 | 
| What is MHS? | A shared specification and driver layer for programmable hardware | 
| Is MHS a new Claude model? | No | 
| Is MHS open source today? | No | 
| Does Anthropic plan to open-source it? | Yes, after the preview and safety work | 
| Is there a release date? | No | 
| Is there a public future licence? | None is named in the reviewed official materials | 
| Does MHS replace MCP? | No | 
| How does MHS use MCP? | MCP is one mechanism for controlling MHS hardware | 
| Other control mechanisms | CLI and code/API files | 
| Does MHS work only with Claude? | No, Anthropic calls it model-agnostic | 
| What hardware? | Equipment with a programmable interface; pilots cover lab and robotic equipment | 
| Strongest public pilot metric | QuEra: 695/700 successful laser relocks, 99.3% | 
| Is that an independent benchmark? | No, it is a partner pilot result | 
| Biggest risk | Agent errors can have physical consequences | 
| Key safety principle | Limits and interlocks should be enforced outside the model itself | 

## What exactly is the Model Hardware Standard?

Anthropic describes MHS as a **shared specification for AI agents to safely operate physical devices**.<sup>[1]</sup>

The problem is familiar. A laboratory or production line may contain equipment from many vendors, each with a different SDK, API, data format, desktop application, documentation set and error model.

Connecting several machines into one workflow often requires bespoke glue code.

Anthropic says such integrations can take weeks or months and argues that MHS can reduce some of that work to hours or minutes.<sup>[1]</sup> That broad statement is a vendor claim, not an independent benchmark of the whole industry.

## MHS is not a robot and not a model

MHS is not:

- a new Claude model,
- a robot operating system,
- a new robot class,
- a firmware replacement,
- a standalone motion-planning algorithm,
- an industrial network protocol replacing every existing control standard.

It is better understood as an **interoperability layer between an agent and programmable physical equipment**.

The agent still needs a model, a harness or application, permissions, a device driver, the actual hardware interface and independent physical safeguards.

## How does MHS work?

### 3.1. A standardised driver

MHS introduces a standard driver that translates operations between a computer and a device.<sup>[1]</sup>

Anthropic gives simple primitives such as:

```
read
write
```
for example:

```
read temperature
write temperature
```
This does not mean every machine exposes only two commands. The idea is to build more complex operations on top of a small and consistent set of primitives.

### 3.2. A standard device description

MHS makes devices discoverable in a standard format so that agents and other devices can find them over a network and understand their capabilities.<sup>[1]</sup>

The description can include:

- what the device measures,
- what can be adjusted,
- physical characteristics,
- important constraints,
- enforced safety boundaries.

Anthropic uses the weight of a robotic arm as an example of a physical fact that may matter for safe manipulation.<sup>[1]</sup>

### 3.3. Natural-language tags

MHS includes tags through which users can describe relevant hardware characteristics in natural language. They can do that directly or have an agent interview them about the setup.<sup>[1]</sup>

The driver then generates a reference file describing the device, measurements, adjustable parameters and safety limits.

Anthropic’s announcement is not the complete public schema. Because the specification remains in a limited preview, field names or configuration syntax should not be invented and presented as official.

## Three ways to control MHS hardware

Anthropic lists three control mechanisms:<sup>[1]</sup>

```
1. MCP
2. CLI
3. code files / APIs
```
### MCP

An agent can access hardware through Model Context Protocol. MCP servers can expose **tools**, executable functions that a model can discover and invoke.<sup>[4]</sup><sup>[5]</sup>

### CLI

Equipment can also be operated directly from a command-line interface. This is useful for human operators, scripting, debugging and integration testing.

### Code files and APIs

An agent can combine commands from one or several devices into ordinary code. Anthropic says this matters for fast operations, repeatable sequences and long-running tasks where an LLM should not reason about every micro-step.<sup>[1]</sup>

## The agent can learn a procedure and then leave the loop

A generative model does not need to control hardware every millisecond.

In one example Claude:

1. adjusted a laser,
2. observed the result through a camera,
3. repeated the experiment,
4. learned the relationship between actions and results,
5. packaged the procedure into deterministic code.<sup>[1]</sup>

The resulting script could then operate without continuous model reasoning.

That suggests an important production pattern:

```
AI explores
      ↓
AI produces a procedure
      ↓
humans / tests verify it
      ↓
deterministic code executes it
```
QuEra used this pattern for its laser-relock controller: the agent helped develop and validate the controller, while the final production relock logic is inspectable deterministic software that does not require an online model at runtime.<sup>[7]</sup>

## MHS vs MCP: the key difference

| Element | MCP | MHS | 
|---|---|---|
| Primary goal | Connect AI applications to tools, data and systems | Standardise description and control of physical equipment | 
| Typical target | Software, APIs, data, tools | Programmable physical devices | 
| Communication model | Host, client, server, JSON-RPC | Driver + hardware description + control mechanisms | 
| Tools | Yes | May be exposed through MCP | 
| CLI | Not the core protocol model | One documented MHS control path | 
| API/code files | Can exist behind MCP | One documented MHS control path | 
| Physical device limits | Not MCP’s central scope | Part of MHS device semantics | 
| Open source today | Yes | Not yet | 
| Status | Open protocol | Limited research preview | 

Anthropic released MCP in 2024 as an open standard for two-way connections between AI systems and data sources or tools.<sup>[4]</sup> Current MCP documentation describes a host-client-server architecture, capability negotiation and primitives such as tools, resources and prompts.<sup>[5]</sup><sup>[6]</sup>

MHS does not replace that layer.

A conceptual stack is:

```
MODEL / AGENT
      │
      ├── MCP
      ├── CLI
      └── CODE / API
             │
          MHS DRIVER
             │
       DEVICE INTERFACE
             │
        PHYSICAL HARDWARE
```
This is a POLPROG conceptual diagram, not an official MHS architecture diagram.

## Is MHS “MCP for the physical world”?

It is a useful shorthand, but technically too simplistic.

The similarity is real: both reduce bespoke integrations and aim to create a common interface for agents.

The difference is fundamental. **MCP is a communication protocol for AI systems and tools. MHS adds semantics for a physical device, including its state, capabilities and constraints.**

MHS can itself use MCP.

So the more accurate statement is:

MHS complements MCP with a layer designed for physical equipment.


not:

MHS replaces MCP in robotics.


## MHS is model-agnostic

Anthropic explicitly says MHS is **model-agnostic** and that any agent harness can access it using standard protocols such as MCP.<sup>[1]</sup>

The concept is therefore not formally limited to Claude or Claude Code.

Public launch case studies nevertheless focus mainly on Claude because they come from Anthropic and its preview partners. There is no broad independent benchmark comparing Claude, GPT, Gemini and open-weight models on identical equipment, with the same driver and the same tasks.

## Safety has to work below the model layer

In software, a wrong tool call may delete a file. In the physical world, a wrong decision may collide a robot, spill a sample, overheat equipment or destroy an experiment.

A natural-language prompt cannot be the only safety control.

MHS can communicate enforced safety limits.<sup>[1]</sup> QuEra additionally says its pilot enforced bounds, interlocks and emergency stops at the hardware interface independently of the model.<sup>[7]</sup>

The right direction is:

```
MODEL
proposes an action
POLICY / APPROVAL
accepts or rejects it
DRIVER / CONTROLLER
enforces limits
HARDWARE INTERLOCK
protects the system even if software fails
```
## Genentech: a physical failure the agent initially misunderstood

Genentech tested MHS on a **BCA protein assay** involving a liquid handler, robotic arm and microplate reader.<sup>[1]</sup>

The most instructive part was a failure.

Bubbles created during mixing caused runtime errors. Claude initially treated this like software trouble and retried the same well with different parameters. More mixing created more bubbles and made the physical problem worse.<sup>[1]</sup>

Human experts had to explain the physical cause and instruct the system to use a clean well and gentler mixing. The team later encoded the lesson into reusable liquid-handling skills.<sup>[1]</sup>

The lesson is important:

A model can understand the error message while misunderstanding the physics behind it.


## University of Washington: six devices in under a week

In the Baker and Pinglay labs, MHS was used for remote instrument monitoring, agent-supervised qPCR and coordination between a robotic arm and a liquid handler.<sup>[1]</sup>

In a plate-handoff demo, the liquid handler completed dispensing, the agent received the completion signal, and about ten seconds later it triggered the robotic arm. The author reports no collisions across repeated tests.

Connecting **six instruments** through MHS, including driver development, reportedly took under a week.<sup>[1]</sup>

That is a result from one laboratory, not a guaranteed deployment time.

## Carnegie Mellon: six deliberately induced safety conditions

A Carnegie Mellon University team used MHS for serial-dilution dose-response experiments.<sup>[1]</sup>

The setup combined a liquid handler, plate reader, robotic arm, monitoring cameras and three computers with incompatible control styles.

Researchers deliberately induced six conditions:

1. missing plate,
2. rotated plate,
3. reader busy,
4. camera disconnected,
5. unreachable device,
6. emergency stop active.<sup>[1]</sup>

The reported system blocked all six **before any device moved**.

That is a valuable proof of concept, but not a formal safety certification of MHS.

### Autonomous experiment correction

In the first run the agent rejected a curve with:

`R² < 0.9`
and reduced maximum concentration:

```
200 µg/mL
→
100 µg/mL
```
The second run achieved:

`R² > 0.98`
without human intervention.<sup>[1]</sup>

The team reports about **eight hours** from raw non-automated equipment readiness to a completed curve including the autonomous rerun, and contrasts that with multi-week vendor integration.<sup>[1]</sup>

Again, this is a case-study result rather than a universal benchmark.

## HHMI Janelia: one state layer for seven programs

In one Janelia project a researcher previously had to launch **seven programs in a fixed order**.

MHS replaced point-to-point connections with a shared state dictionary in shared memory.<sup>[1]</sup>

According to the case study:

- adding a new camera took minutes instead of days,
- experiment startup changed from seven steps to one operation,
- data streams could be analysed using reusable tools independent of the original vendor application.<sup>[1]</sup>

MHS also provided device-level boundaries such as a maximum laser-power limit so an agent could not exceed the range considered safe for the sample.<sup>[1]</sup>

## QuEra: 695 successful relocks out of 700

The most quantitative public pilot comes from QuEra Computing.

QuEra used MHS to give Claude access to part of the laser system inside a quantum computer.<sup>[1]</sup><sup>[7]</sup><sup>[8]</sup>

After an experimental phase, the work produced a deterministic controller.

QuEra ran:

```
700 trials
7 disturbance classes
100 trials per class
```
The controller recovered the correct target:

```
695 / 700
=
99.3%
```
Simpler faults took roughly:

`0.9–5.4 s`
and the hardest:

`10–14 s`
QuEra compares that with:

`5–10 minutes`
for a human expert’s manual procedure.<sup>[7]</sup>

### This is not a 99.3% Claude benchmark

During the final blind test the AI agent was not controlling the laser online.

The correct wording is:

An agent using MHS helped develop a controller that later achieved 99.3% in the blind test.


Not:

Claude has 99.3% accuracy at controlling hardware.


### A source discrepancy worth noting

Anthropic describes QuEra’s earlier handwritten recovery script as taking “several months” to build.<sup>[1]</sup>

QuEra’s own article says the earlier team spent roughly **two to three weeks**.<sup>[7]</sup>

Because those public descriptions conflict, we do **not** use the previous script’s development time as a hard comparison metric.

## Limitations revealed by the QuEra pilot

The public reports also describe limitations.

Claude:

- could not diagnose some purely physical hardware faults,
- understood the rig mainly through its programmatic representation,
- needed extensive context,
- frequently paused and requested human confirmation for actions it considered even slightly risky.<sup>[1]</sup>

In physical systems, that conservatism may be preferable to overconfidence.

## Tetsuwan: one device detects a problem and another fixes it

Tetsuwan connected MHS with ResearchOS for a qPCR workflow related to pollution monitoring in San Pedro Creek.<sup>[1]</sup>

A camera detected bubbles in a sample. The robot holding the sample could not remove them.

The system then:

1. detected the issue,
2. searched MHS-connected equipment,
3. found a centrifuge,
4. had Claude propose using it to remove the bubbles,
5. issued the centrifuge commands after approval.<sup>[1]</sup>

This illustrates a powerful orchestration idea: a problem detected by device A can be resolved using device B without the original workflow hard-coding that exact recovery path.

## The emerging partner ecosystem

Anthropic lists participants including:<sup>[1]</sup>

- Amazon Web Services,
- Automata,
- Danaher,
- Doosan Robotics,
- MBF Bioscience,
- QIAGEN,
- Tecan,
- Universal Robots,
- Hugging Face,
- Raspberry Pi.

AWS plans MHS support through Strands Robots. Hugging Face is adding support in LeRobot, and Raspberry Pi is working on integrations for selected products.<sup>[1]</sup>

This does not mean every integration is publicly production-ready today. Some are tests, planned integrations or preview work.

## MHS is not open source yet

The official project page says MHS begins as a **limited research preview**.<sup>[2]</sup>

Anthropic wants first to gather partner experience, build safety evaluations, develop best practices and strengthen safeguards before releasing the standard.<sup>[1]</sup>

As of 30 August 2026, the reviewed official materials provide:

- no public open-source release date,
- no final licence,
- no complete public specification comparable in maturity to MCP documentation.

Therefore:

`Anthropic plans to open-source MHS.`
is accurate.

`MHS is already open source.`
is not.

## Are the current results independent benchmarks?

No.

The public performance numbers come mainly from Anthropic and preview partners.

Reuters independently confirms the launch of the preview, its broad scope and Anthropic’s intention to make MHS open source later.<sup>[3]</sup>

But there is no public benchmark with:

- identical hardware,
- identical drivers,
- multiple models,
- one common agent harness,
- a repeatable fault suite,
- independent grading.

Numbers such as:

```
99.3%
3× faster
8 hours
under one week
```
must therefore remain attached to their individual pilot studies.

## Threat model: what can go wrong?

### Incorrect reasoning

A model can misunderstand a sensor reading, error code, image or physical cause. Genentech provides a real example.<sup>[1]</sup>

### Broken or malicious driver

Incorrect units, state reporting or validation can give the agent a false picture of reality.

### Prompt injection

Camera text, documentation, network data and external systems may contain malicious instructions or content.

### Confused deputy

An agent with access to multiple devices can use the right tool for the wrong purpose.

### Race conditions

Two agents or machines can attempt incompatible changes to the same physical state.

### Connectivity loss

The system must define a safe state if the model, MCP connection, network, driver or sensor disappears.

## A safer production architecture

```
┌───────────────────────────────┐
│ MODEL / AGENT                 │
│ plans and proposes actions    │
└──────────────┬────────────────┘
               │
┌──────────────▼────────────────┐
│ POLICY + APPROVAL             │
│ roles, scopes, human approval │
└──────────────┬────────────────┘
               │
┌──────────────▼────────────────┐
│ MCP / CLI / API               │
│ control channel               │
└──────────────┬────────────────┘
               │
┌──────────────▼────────────────┐
│ MHS DRIVER                    │
│ state, procedures, limits     │
└──────────────┬────────────────┘
               │
┌──────────────▼────────────────┐
│ DETERMINISTIC CONTROLLER      │
│ validation and sequencing     │
└──────────────┬────────────────┘
               │
┌──────────────▼────────────────┐
│ HARDWARE INTERLOCK / E-STOP   │
└──────────────┬────────────────┘
               │
        PHYSICAL HARDWARE
```
This is a POLPROG recommended architecture, not an official Anthropic diagram.

The key principle is:

**An LLM should not be the only component deciding whether a physical operation is safe.**


## What should the deterministic layer enforce?

Examples include:

- temperature ranges,
- maximum power,
- arm speed,
- workspace boundaries,
- operation order,
- collision zones,
- pressure limits,
- guard state,
- emergency stop,
- maximum operation duration.

If an agent requests a value outside the permitted range, the system should reject it regardless of the model’s reasoning.

## Human-in-the-loop still matters

MCP’s own tool specification recommends that users retain the ability to deny tool invocations.<sup>[5]</sup>

For physical hardware, risk tiers make sense:

```
READ
automatic
LOW-RISK WRITE
automatic within a narrow range
MEDIUM-RISK
policy + extra validation
HIGH-RISK
human approval
EMERGENCY / UNSAFE
always blocked
```
## Why deterministic fallback matters

QuEra demonstrates a practical pattern:

```
AI discovers a solution
→ code is generated
→ tests and humans verify it
→ production runs deterministic software
```
This can reduce inference cost, latency, nondeterminism, dependency on an online model and the risk of unexpected decisions.

## MHS and industrial control systems

MHS should not be treated as a replacement for PLCs, SCADA, OPC UA, safety PLCs or real-time controllers.

A more realistic position is above them:

```
agent
↓
orchestration
↓
MHS
↓
existing controllers
↓
hardware
```
Anthropic’s own design supports packaging fast or long-running operations into code so equipment can execute them without LLM reasoning on every step.<sup>[1]</sup>

## Who should care about MHS now?

Especially:

- laboratories with multi-vendor equipment,
- biotech and pharma,
- microscopy teams,
- robotics organisations,
- quantum-computing teams,
- advanced manufacturing,
- R&D groups maintaining large amounts of bespoke integration code.

## Who should be cautious?

Particularly organisations where:

- the system is safety-critical,
- a stable public specification is mandatory,
- a known open-source licence is required,
- certified industrial standards are mandatory,
- equipment lacks an automatable interface,
- there are no independent hardware interlocks,
- the team cannot audit drivers and control software.

A **research preview is not the same as a mature production standard**.

## How can a company prepare for MHS?

### Step 1: inventory equipment

For each device record:

```
vendor
model
SDK/API/GUI
units
states
commands
limits
E-stop
dependencies
```
### Step 2: separate read from write

Define what can be read, what can be changed, what is low risk and what requires approval.

### Step 3: remove safety from prompts

Do not rely on an instruction such as:

`"never set temperature above 80°C"`
A real physical limit must be enforced by code or hardware.

### Step 4: log every action

Record:

- agent/model,
- operation,
- parameters,
- state before and after,
- result,
- timestamp,
- policy decision,
- approval.

### Step 5: simulate before using real equipment

First:

`digital twin / mock driver`
then:

`real hardware`
## Minimum test plan

### Driver

- units,
- ranges,
- timeouts,
- reconnect,
- malformed replies,
- restart.

### Agent

- incorrect sensor reading,
- conflicting data,
- unknown error code,
- prompt injection,
- missing context.

### Hardware

- collision prevention,
- E-stop,
- power loss,
- network loss,
- mechanical blockage,
- out-of-range requests.

### Multi-agent

- simultaneous writes,
- stale state,
- resource locking,
- retry after timeout.

## MHS safety checklist

### Architecture

- The model never controls actuators without validation.
- Every driver has explicit limits.
- Values include units and ranges.
- Critical boundaries are deterministic.
- E-stop works independently of AI.
- A safe state exists after connectivity loss.
- Device state is timestamped.
- Retry behaviour is safe.

### Permissions

- The agent sees only required devices.
- READ and WRITE permissions are separate.
- High-risk actions require approval.
- Permissions expire.
- There is no single admin token for the whole environment.

### Monitoring

- Every tool call is logged.
- Physical changes generate telemetry.
- Alerts do not depend solely on the model.
- Operators can see current device state.
- Event replay is available.
- Failed actions are classified.

### Testing

- Mock hardware.
- Physical sandbox.
- Fault injection.
- Prompt injection.
- Race conditions.
- Network partition.
- Agent restart.
- Driver restart.
- Incorrect units.
- Out-of-range values.

### Production

- Rollout begins read-only.
- Low-risk writes come next.
- Safety-critical actions remain outside the agent.
- Deterministic code replaces AI where practical.
- Drivers receive code review.
- Rollback exists.
- Manual takeover exists.
- The team understands the physical impact of each command.

## What must happen before MHS becomes a real industry standard?

It will need, among other things:

1. a public specification,
2. stable versioning,
3. a compatibility model,
4. public SDKs,
5. an open-source licence,
6. reference drivers,
7. conformance tests,
8. security evaluations,
9. independent implementations,
10. vendor support,
11. driver validation,
12. a clear permission model.

MCP became important because it grew into an interoperable ecosystem. MHS will need a similar path.

## Will MHS change robotics?

Possibly, but it is too early to say.

The strongest near-term fit is in environments where equipment is already programmable, integration is expensive and workflows change frequently:

```
laboratories
R&D
biotech
microscopy
quantum
advanced manufacturing
```
MHS does not automatically solve robot perception, motion planning, real-time control, safety certification or manipulation physics.

It may, however, simplify the interface through which an agent uses existing control systems.

## POLPROG verdict

Model Hardware Standard is one of the more interesting agent developments of 2026 because it moves the interoperability problem from digital systems into physical equipment.

### What is confirmed?

- MHS entered research preview on 27 August 2026.<sup>[1]</sup><sup>[3]</sup>
- it originated in collaboration between Anthropic and HHMI Janelia.<sup>[1]</sup>
- it is model-agnostic.<sup>[1]</sup>
- control paths include MCP, CLI and code/API files.<sup>[1]</sup>
- the MHS driver describes devices and safety boundaries.<sup>[1]</sup>
- Anthropic intends to open-source the standard later.<sup>[1]</sup><sup>[2]</sup>
- QuEra confirms 695 successful relocks out of 700 for the final controller.<sup>[7]</sup>

### What do we still not know?

- the final public schema,
- the open-source release date,
- the licence,
- API stability,
- cross-implementation compatibility,
- independent benchmark results,
- how different models perform on identical hardware.

### The most promising pattern

Not:

`LLM controls everything continuously`
but:

```
agent understands the goal
→ explores a safe space
→ coordinates devices
→ creates or selects a procedure
→ the system validates it
→ repeatable work moves to deterministic code
```
If Anthropic opens the specification, vendors implement reusable drivers and independent teams validate safety and interoperability, MHS could become an important layer for physical AI.

**It is not there yet.**
