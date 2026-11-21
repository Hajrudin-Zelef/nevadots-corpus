---
id: collect-261121-veille-ia/docs-techniques/mhs-positioning-above-the-model-hardware-standard-physical-ai-safet-urml-mars-urml-d021afd
title: "mhs: positioning above the Model Hardware Standard; physical-AI safet… · URML-MARS/URML@d021afd"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2026-08-27"]
keywords: ["urml", "standard", "hardware", "device", "layer", "model", "anthropic", "align"]
source: https://github.com/urml-mars/urml/commit/d021afd48e494ef2abfacd17440cad79bc48a07e
source_anchor: ""
source_lines: [1, 80]
section: "Docs_Techniques"
---
|  | 
|  | `1` | `+`<p align="center"> | 
|  | `2` | `+`  <a href="https://urml.dev"><img src="https://urml.dev/favicon.svg" alt="URML" width="72" height="72"></a> | 
|  | `3` | `+`</p> | 
|  | `4` | `+` | 
|  | `5` | `+`<p align="center"> | 
|  | `6` | `+`  A small, opinionated, human-readable language for describing robot intent. | 
|  | `7` | `+`</p> | 
|  | `8` | `+` | 
|  | `9` | `+`<p align="center"> | 
|  | `10` | `+`  <a href="https://urml.dev"><b>urml.dev</b></a> | 
|  | `11` | `+`</p> | 
|  | `12` | `+` | 
|  | `13` | `+`--- | 
|  | `14` | `+` | 
|  | `15` | `+`# URML and the Model Hardware Standard | 
|  | `16` | `+` | 
|  | `17` | `+`Anthropic opened a research preview of the [Model Hardware Standard](https://www.anthropic.com/news/model-hardware-standard-research-preview) (MHS) on 2026-08-27. This page states where URML sits relative to it, what maps onto what, and what waits until the standard is open. Everything here is drawn from Anthropic's own announcement; URML has not seen the specification and claims no compatibility with it. | 
|  | `18` | `+` | 
|  | `19` | `+`## What MHS is, in Anthropic's words | 
|  | `20` | `+` | 
|  | `21` | `+`A standardized driver: "software that translates between a computer's operating system and a hardware device", built on "a simple set of primitives, commands like read (for example, get temperature) or write (for example, set temperature), that any hardware device can understand". Devices are discoverable "in a standard format, so that devices and agents can find each other and communicate across networks". Each device carries a reference file "with information about a device's general characteristics, such as what it can measure, what can be adjusted, and what safety limits will be enforced". It is model-agnostic and reachable through MCP, a command line, or code. Anthropic will open-source it after building "safety evaluations and best practices for AI systems that operate physical equipment". Preview partners include Universal Robots, Doosan Robotics, AWS, Automata, Danaher, Tecan, QIAGEN, Genentech, HHMI Janelia, the University of Washington, Carnegie Mellon, and QuEra. | 
|  | `22` | `+` | 
|  | `23` | `+`## Where URML sits | 
|  | `24` | `+` | 
|  | `25` | `+```` | 
|  | `26` | `+`   natural language / an agent's goal | 
|  | `27` | `+`                 \| | 
|  | `28` | `+`   Layer 4  NL grammar + LLM bridge        URML | 
|  | `29` | `+`   Layer 3  behavior composition           URML | 
|  | `30` | `+`   Layer 2  intent primitives              URML     <- one program, validated whole | 
|  | `31` | `+`   Layer 1  capability manifest + envelope URML     <- derives from device descriptions | 
|  | `32` | `+`   ------------------------------------------------------------------------ | 
|  | `33` | `+`   Layer 0  substrate: ROS 2, PX4, OPC UA, vendor SDKs, and now MHS drivers | 
|  | `34` | `+`             (device description, discovery, read/write, per-call limits) | 
|  | `35` | `+```` | 
|  | `36` | `+` | 
|  | `37` | `+`URML's Manifesto has said since day one that Layer 1 "extends existing standards (URDF, SDF); we do not reinvent robot description". MHS is another description and driver standard at Layer 0, and URML treats it the same way it treats URDF: as a source the capability manifest derives from, and as a substrate the validated program dispatches to. URML already ships adapters for two of the MHS preview partners (Universal Robots and Doosan, in `reference/cobot-runtime/`) and an OPC UA runtime whose intent-to-node mapping is the closest existing analogue of intent-to-read/write. | 
|  | `38` | `+` | 
|  | `39` | `+`The one-line version: **MHS tells the agent what a device is and what it will refuse per call. URML checks the whole program against the manifest and the deployment envelope before the first call.** | 
|  | `40` | `+` | 
|  | `41` | `+`## What each side has that the other does not | 
|  | `42` | `+` | 
|  | `43` | `+`MHS, as announced, has: a driver contract, network discovery, a device-authored reference file, per-call limit enforcement at the device, and a distribution channel through the vendors themselves. | 
|  | `44` | `+` | 
|  | `45` | `+`URML has, shipped and tested: | 
|  | `46` | `+` | 
|  | `47` | `+`- **Whole-program static validation** before dispatch: five passes over an entire program (argument typing, capability, safety envelope, bindings and cross-primitive types, compliance policy). A per-call limit catches the fifth action; a program check refuses the plan. | 
|  | `48` | `+`- **The self-declaration / deployment-envelope split.** A device's own limits (what MHS's reference file carries) and a deployment's limits (what a site owner imposes) are separate artifacts; the validator conjoins them, strictest wins. A lab may cap an arm below what its vendor allows. | 
|  | `49` | `+`- **Runtime enforcement** as a second, independent check ([RFC-0667](../rfcs/0667-envelope-enforcement.md) shield) and a **rehearsal gate** that rolls the validated program out in simulation before real execution ([RFC-0668](../rfcs/0668-rehearsal-gate.md)). | 
|  | `50` | `+`- **A conformance suite** and a normative runtime contract ([RFC-0014](../rfcs/0014-substrate-conformance.md)), so "this runtime is URML-compatible" is a test result, not a claim. | 
|  | `51` | `+`- **Evidence traceability** on every capability claim ([RFC-0631](../rfcs/0631-evidence-traceability.md)): declared, derived, or verified. | 
|  | `52` | `+`- **Provider neutrality and offline execution.** MHS claims model-agnosticism too, so this is a shared value, not a differentiator. The differentiator is the validator. | 
|  | `53` | `+` | 
|  | `54` | `+`## Mapping the announced reference-file concepts onto the manifest | 
|  | `55` | `+` | 
|  | `56` | `+`Only the three concepts Anthropic's post names are mapped; the columns will be filled with real field names when the specification is public. | 
|  | `57` | `+` | 
|  | `58` | `+`\| MHS reference file (as announced) \| URML capability manifest \| URML deployment envelope \| | 
|  | `59` | `+`\|---\|---\|---\| | 
|  | `60` | `+`\| "what it can measure" \| `perception.sensors[]` (`measurement_type`, `range_min/max`, `units`, RFC-0039 capability fields), `perception.cameras[]` (RFC-0682) \| not applicable \| | 
|  | `61` | `+`\| "what can be adjusted" \| `mobility` (`max_velocity`, `max_payload`, ...), `manipulation.grippers[]` (`force_min_n/force_max_n`, `accepted_classes`), `manipulation.arms[]`, `outputs.named_endpoints`, `programs` (RFC-0015) \| not applicable \| | 
|  | `62` | `+`\| "what safety limits will be enforced" \| the same mechanical ceilings, tagged with `evidence` (RFC-0631) \| `max_velocity`, `max_grip_force_n`, `max_payload`, geofences, people-occupancy zones, monitorable temporal properties (RFC-0382/0667) \| | 
|  | `63` | `+`\| read/write primitives \| not a manifest concern \| not applicable; a URML *adapter* lowers Layer-2 primitives onto them (see below) \| | 
|  | `64` | `+` | 
|  | `65` | `+`The split in the third row is the point: MHS puts the device's own limits in the device file, which is exactly right for a device; URML adds the deployment's limits on top and validates against the stricter of the two. | 
|  | `66` | `+` | 
|  | `67` | `+`## The evaluation harness | 
|  | `68` | `+` | 
|  | `69` | `+`Anthropic's stated gate before open-sourcing MHS is "safety evaluations and best practices for AI systems operating physical equipment". URML's contribution to that gate is [`examples/physical-ai-safety-eval/`](../../examples/physical-ai-safety-eval/): a hermetic harness that takes a corpus of agent intents against a lab-cell manifest shaped like the assay in Anthropic's post (a liquid handler, a robotic arm, a plate reader), validates each one, and reports the refusal matrix with machine-readable reasons, the envelope-monitor verdict over the rehearsed trace of every accepted program, and the evidence class of every capability the refusals relied on. It measures whether an agent's intent is admissible on declared hardware under a declared envelope. It does not measure physics, and it says so. | 
|  | `70` | `+` | 
|  | `71` | `+`## What waits for the open specification | 
|  | `72` | `+` | 
|  | `73` | `+`- A reference-file importer (`mhs-to-manifest`), the same shape as the shipped [`examples/urdf-to-manifest/`](../../examples/urdf-to-manifest/). | 
|  | `74` | `+`- A real `MhsAdapter` runtime (RFC-0014 conformance) lowering URML primitives onto MHS read/write. The harness ships a labeled scaffold with an injectable transport; its wire format is a placeholder. | 
|  | `75` | `+`- Conformance fixtures for MHS-driven devices, starting with the two partners URML already has adapters for. | 
|  | `76` | `+`- Any manifest schema change. None is proposed against an unpublished schema. | 
|  | `77` | `+` | 
|  | `78` | `+`Tracked in [RFC-0683](../rfcs/0683-model-hardware-standard-outreach.md). | 
## 0 commit comments
