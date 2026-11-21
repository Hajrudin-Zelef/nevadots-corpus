---
id: collect-261121-veille-ia/hardware-semi/tesla-renames-its-ai-chips-and-promises-80x-more-compute
title: "Tesla renames its AI chips and promises 80x more compute"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["tesla", "chip", "compute", "memory", "chips", "more", "training", "dojo"]
source: https://www.alextech.ai/en/news/tesla-renames-its-ai-chips-and-promises-80x-more-compute/
source_anchor: ""
source_lines: [1, 25]
section: "Hardware_Semi"
---
Tesla has published a renewed roadmap for its AI accelerators, and the most visible change is not technical: the chips are no longer called AI4, AI5 and AI6, but SI4, SI5 and SI6. SI stands for "super intelligence," the term the Trump administration wants used in place of "artificial intelligence" in federal documents, and that Elon Musk has already adopted for his companies.

On the technical side, the key figure concerns the sixth generation inference accelerator, the chip that runs model responses. Tesla estimates that SI6 will offer **80 times** the compute of the current fourth generation chip, HW4, and **nine times** the memory. In numbers, that means between 17,280 and 46,080 TOPS, or 17.28 to 46.08 POPS. These are estimates, and Tesla has not clarified which data formats are supported. The chip is due between 2029 and 2030.

*Image: kevinchen.co — https://kevinchen.co/blog/tesla-ai-day-2022/*

As a reference, Tesla's current in-car chip, HW4, is made by Samsung on a 7-nanometer-class process with GDDR6 memory. Tesla does not officially disclose its absolute compute, memory capacity or power consumption, which makes the jump hard to measure. Musk has said HW4 is between 3 and 8 times more powerful than HW3, which was rated at 72 TOPS, implying an estimate of 216 to 576 TOPS.

Nvidia is already shipping its Rubin chip, which it rates at 50 NVFP4 PFLOPS for inference and 35 for training. SI6 is still in development, several years away.

## Dojo 3 drops wafer-scale integration

The most interesting part of the roadmap is the return of Dojo, Tesla's in-house training accelerator. The third generation, Dojo D3, changes approach. D1 and D2 relied on distributed on-chip SRAM and processors interconnected on a single wafer. D3 is a chiplet design combining 2.5D and 3D packaging with stacked HBM memory.

*Image: MSN — https://www.msn.com/en-us/money/other/tesla-s-dojo-ai-the-future-of-machine-learning-power/vi-AA1uhBnE*

The shift follows a clear logic: AI models need huge amounts of memory, and SRAM does not scale that far. But whether Tesla has fully abandoned wafer-scale integration is not confirmed. Tesla has not disclosed D3's compute, memory capacity, manufacturing process or schedule. It says only that the chip will be optimised for "space, training and scaling," and that it may be adapted for extraterrestrial deployments, with likely radiation hardening for some versions.

The Dojo return was already anticipated by Musk, and Tesla places it in both terrestrial and space data centres. Whether the new architecture delivers remains to be seen, since many variables are still open.

## What it means for Tesla's AI

The roadmap shows Tesla wants to combine AI for cars and AI for data centres, with chips serving both driving and model training. The renaming is the most immediate signal of that ambition, but concrete numbers will only arrive with real products, and for now they remain estimates to treat with caution.

No comments yet. Be the first!
