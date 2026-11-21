---
id: collect-261121-veille-ia/gpt-openai/openai-jalapeno-the-custom-inference-chip-explained
title: "OpenAI Jalapeno: The Custom Inference Chip Explained"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-06-24"]
keywords: ["openai", "jalapeno", "inference", "chip", "data", "venturebeat", "custom", "broadcom"]
source: https://www.layer3labs.io/guides/openai-jalapeno-explained
source_anchor: ""
source_lines: [1, 87]
section: "GPT_OpenAI"
---
# OpenAI Jalapeno Is a Custom Chip Built to Serve Models

OpenAI and Broadcom unveiled the processor on June 24, 2026, and OpenAI has said it will roll out across active data centers by the end of the year.

Jalapeno is a custom chip designed by OpenAI and Broadcom to run inference, the process of serving a model's answers to users. Unlike a general-purpose graphics processing unit (GPU), it is an application-specific integrated circuit (ASIC) built specifically for large language model (LLM) inference.

The companies announced their partnership in October 2025 and unveiled the finished processor on June 24, 2026 (VentureBeat). Broadcom provides the core silicon implementation and networking, including its Tomahawk networking silicon, while Celestica handles board, rack, and system integration.

For businesses buying AI, Jalapeno is more of a supply-side development than a product launch. Customers won't see it in an interface; they'll feel its impact through the cost of serving inference.

## What Jalapeno Is

Jalapeno is a purpose-built processor for LLM inference (VentureBeat). An ASIC does one job, and doing one job is what lets it beat a general-purpose chip on the power and cost of that job.

The design goal OpenAI describes is reducing unnecessary data movement while matching compute, memory, and networking resources to each other. Moving data between memory and compute burns most of the energy in inference, so cutting that movement is where an inference-specific design earns its return.

Greg Brockman said on CNBC that Jalapeno is a real performance improvement on performance per watt and performance per dollar. OpenAI has published its own account on the Jalapeno announcement page and a follow-up on first results. Check those for figures rather than any secondhand summary.

- Type: an ASIC, designed for one workload, rather than a general-purpose GPU.
- Job: inference only. Training stays on other hardware.
- Partners: Broadcom for core silicon and Tomahawk networking, Celestica for board, rack, and system integration.
- Design aim: less data movement between memory and compute, which is where inference energy goes.

## How Fast It Was Built, and When It Ships

Jalapeno went from early schematics to fabrication readiness in a nine-month window (VentureBeat). New processor programs are normally measured in years, and OpenAI says its own models took part in the design work.

OpenAI has said it will begin rolling the processors out across active data centers by the end of 2026 (VentureBeat). At the time of the unveiling it was running GPT-5.3-Codex-Spark on the chips at a production workload, in a test environment.

Treat the deployment date as a plan rather than a delivered fact. Silicon schedules slip, and a rollout that begins in December is a rollout that mostly lands the following year.

- October 2025: OpenAI and Broadcom announce the partnership.
- Nine months: early schematics to fabrication readiness, with OpenAI's models assisting the design.
- June 24, 2026: the finished processor is unveiled, running GPT-5.3-Codex-Spark at a production workload in a test environment.
- End of 2026: the stated start of rollout across active data centers.

## What It Means for OpenAI and NVIDIA

The headline framing of Jalapeno as a move against NVIDIA overstates what has changed. NVIDIA made a $30 billion direct investment into OpenAI in February 2026, as part of a $110 billion funding round. That agreement covers 10 gigawatts of computing systems, including 3 gigawatts of dedicated inference capacity (VentureBeat).

Reporting at the unveiling indicated NVIDIA remains central to OpenAI on model training and development, and that existing vendor agreements were unaltered (VentureBeat). A company that just took $30 billion from a supplier is not replacing that supplier this year.

The accurate read is narrower and still important. OpenAI is moving part of one workload, inference, onto silicon it controls, which gives it a hand on the cost of the thing it sells most of.

## What It Changes for a Business Buying AI

Nothing changes in how you use OpenAI's models. Jalapeno has no interface, no setting, and no signup, and a customer cannot choose to run on it.

The mechanism that reaches you is price. If serving a model costs OpenAI less per token, that shows up over time. It arrives as cheaper application programming interface (API) rates, larger plan allowances, or capability that used to sit in a higher tier.

In the implementations we run for clients, inference cost decides which workloads survive a pilot. A summarization job that costs a fraction of a cent per document gets deployed across a company. The same job at ten times the price stays a demo, so the cost curve is the part of this announcement worth tracking.

- No action required: there is nothing to enable, install, or migrate to.
- Watch pricing pages, not chip news: any benefit reaches customers as a published rate change.
- Do not rebuild architecture around it: a chip in one provider's data center is not a reason to change how your integration is written.
- Do keep vendor optionality: the reason to stay portable across providers is unchanged by anyone's silicon.

## Who Should Ignore This Entirely

If you buy AI as a product rather than as infrastructure, Jalapeno is not your news. Teams using ChatGPT, Codex, or a vendor's API through an integration will see no difference in behavior from a chip change.

The audiences it matters to are narrow. Anyone modeling long-term inference cost, anyone holding NVIDIA or Broadcom stock, and anyone whose inference margins depend on hardware they do not own.

For everyone else, check what you pay per million tokens today and set a reminder to check again in six months. That number is where any of this becomes real.

## What Would Change This Read

Published performance-per-watt figures with a stated comparison point would move this from a directional claim to a measurable one. Brockman's comments describe an improvement without a number attached to a named baseline.

A rate cut on OpenAI's published API pricing, timed to the rollout, would be the first hard evidence. It would show the cost advantage reaching customers rather than staying with the company.

A second-generation chip aimed at training would be the change that justifies the framing this got in the press. Until then Jalapeno is an inference chip, and the training market is where the NVIDIA question is actually settled.

## Frequently Asked Questions

- Jalapeno is a custom application-specific integrated circuit (ASIC) that OpenAI designed with Broadcom to run inference, the work of serving model answers to users. It is purpose-built for large language model inference rather than being a general-purpose GPU, and OpenAI unveiled it on June 24, 2026 (VentureBeat).
- OpenAI designed it in partnership with Broadcom, which supplies the core silicon implementation and networking technology including its Tomahawk networking silicon (VentureBeat). Celestica handles board, rack, and system integration. OpenAI has said its own models took part in the design work, which helped compress the schedule.
- No. Jalapeno targets inference, while NVIDIA hardware remains central to OpenAI for model training and development, and existing vendor agreements were reported as unaltered (VentureBeat). NVIDIA also made a $30 billion direct investment into OpenAI in February 2026 covering 10 gigawatts of computing systems.
- OpenAI has said it will begin rolling the processors out across active data centers by the end of 2026 (VentureBeat). At the unveiling it was running GPT-5.3-Codex-Spark at a production workload in a test environment. Silicon schedules slip regularly, so treat the date as a plan rather than a delivered result.
- Not directly, and not yet. A cheaper cost to serve inference reaches customers as published API rate changes or larger plan allowances, and only when the provider chooses to pass it on. Check OpenAI's own pricing page for the current rates rather than inferring a price from a hardware announcement.
- Inference is the workload OpenAI runs most of, so its cost sets the margin on everything the company sells. A chip designed for one workload can beat a general-purpose GPU on performance per watt and per dollar. That is the improvement Greg Brockman described on CNBC at the unveiling.

### Get your inference costs under control

At Layer3Labs, we build and operate AI systems inside other people's businesses, and cost per task is what decides whether one survives its pilot. Bring us the workload and we will show you where the spend actually goes.

Book a Consultation
