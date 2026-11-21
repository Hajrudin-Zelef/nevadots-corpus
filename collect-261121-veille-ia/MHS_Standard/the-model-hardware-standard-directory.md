---
id: collect-261121-veille-ia/mhs-standard/the-model-hardware-standard-directory
title: "The Model Hardware Standard Directory"
domain: mhs-standard
role: reference
task: article
actors: []
dates: ["2026-08-28"]
keywords: ["model", "standard", "hardware", "directory", "agents", "devices", "driver", "robots"]
source: https://www.gofastmhs.com/what-is-mhs
source_anchor: ""
source_lines: [1, 19]
section: "MHS_Standard"
---
Updated 2026-08-28

# What is the Model Hardware Standard?

The Model Hardware Standard (MHS) is Anthropic’s research-preview specification, announced 27 August 2026 with HHMI Janelia, for AI agents to discover and safely operate programmable physical devices. A standard driver uses read/write primitives, network discovery, and natural-language safety tags. Agents reach devices over MCP, a CLI, or generated code files. Access is apply-only; open source is planned after safety work.

Primary source: Anthropic, “Previewing the Model Hardware Standard,” 27 August 2026, developed with HHMI Janelia Research Campus. Control surfaces named by Anthropic: Model Context Protocol (MCP), command-line interface, and code files. Early device classes include cameras, robot arms, microscopes, centrifuges, pipette robots, spectrometers, and incubators. Vendors named include Tecan, Universal Robots, Danaher, QIAGEN, MBF Bioscience, Automata, Doosan Robotics, AWS Strands Robots, Hugging Face LeRobot, and Raspberry Pi. Waters Corporation later joined the research program (13 October 2026 press release) for LC-MS instruments.

## How is MHS different from MCP?

MCP connects agents to software tools. MHS is a driver and discovery format for physical instruments. An agent can use MCP to talk to an MHS driver. They are complementary, not substitutes.

## Where do I find devices and drivers today?

This directory indexes public names while the spec is gated. Apply for the preview at modelhardwarestandard.com. If you know of a missing device, vendor, or lab, submit it here.

## Subscribe for updates

Spec drops, new drivers, catalog changes. No ads. We email when something actually shipped.
