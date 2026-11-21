---
id: collect-261121-veille-ia/gpt-openai/gpt-6-2-astra-s-3d-and-cad-skills-are-its-real-standout-feature
title: "GPT-6.2 Astra's 3D and CAD Skills Are Its Real Standout Feature"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "claude", "model", "gpt-6", "openais", "robotics", "version", "real"]
source: https://www.mindstudio.ai/blog/gpt-6-2-astra-3d-modeling-benchmarks
source_anchor: ""
source_lines: [1, 69]
section: "GPT_OpenAI"
---
## What is GPT-6.2 Astra and why did its launch split opinion?

GPT-6.2 Astra is OpenAI’s latest frontier model, released alongside a smaller sibling called 6.2 Soul. Early testers, including Dan Shipper, Ethan Mollick, and Matt Schumer, reported inconsistent behavior, weaker instruction following, and a rougher day-to-day feel compared to Anthropic’s Claude Opus 5.5 and Claude 5.2. At the same time, Astra posted a perfect 100% score on ARC-AGI-3 (with OpenAI’s own adapter) and took the top spot on the AI Robotics leaderboard version 0.1, ahead of Claude 5.2 and Gemini 4 Hydrogen. The split reaction comes down to this: Astra is genuinely weak in some everyday use cases, but it is a clear frontier leader in 3D modeling, CAD generation, and robotics simulation.

## TL;DR

- **Mixed critical reception** greeted GPT-6.2 Astra, with multiple AI reviewers calling it uneven, hard to steer, or a regression compared to OpenAI’s previous releases and to Claude 5.2.
- **ARC-AGI-3 is fully solved** by Astra when paired with OpenAI’s custom adapter, the first model to hit 100% on that benchmark, though it scores lower without the harness.
- **Astra leads AI Robotics version 0.1** , outperforming Claude 5.2, Gemini 4 Hydrogen, and OpenAI’s own prior models on that leaderboard.
- **3D CAD generation is the standout skill** , demonstrated through a fully modeled, mechanically simulated 3D-printable pendulum clock with matching assembly instructions.
- **Cost efficiency, not raw quality, is the real pitch** , since Astra sits near the top-left of the Pareto frontier, delivering strong scores at a fraction of Claude 5.2’s cost per task.
- **Self-improvement capability crossed a new threshold** , with Astra becoming the first externally released model OpenAI classifies as meeting its “high” threshold for AI self-improvement, prompting added safety measures including a 35-second kill switch.
- **Coding may still favor rivals** , with early testers suggesting Claude Opus 5.5 and Claude 5.2 remain stronger choices for software development work right now.

**›**Build a shift-handoff app for 900 nurses.

*already included:*✓ HIPAA✓ access control✓ audit log✓ self-hosted

## How did reviewers actually test Astra’s 3D and CAD abilities?

Two independent hands-on tests focused heavily on Astra’s spatial reasoning. In one, the model was asked to design a 3D-printable pendulum clock from scratch: sketch the parts, model every component in CAD, write a Python solver to verify the mechanical motion, and then build a Blender environment to visualize the assembled clock running. The result showed individually modeled gears fitting together correctly, a working animation of the mechanism, and generated assembly instructions, essentially a complete design pipeline from concept to printable file.

A second round of testing had Astra generate a simulated RC buggy and a car with modeled front and rear suspension, tire contact patches, and traction circles, all placed inside a 3D physics environment. The buggy was built from public reference photos and manuals, with every part designed individually rather than pulled from a stock asset library. Reviewers also tested smaller creative tasks, including a photorealistic 3D landing page for a mechanical watch and an AI-generated 3D game level, both built from single-sentence prompts.

## Why does the ARC-AGI-3 and AI Robotics result matter?

ARC-AGI-3 is the latest version of a benchmark series specifically designed to resist being “solved” by pattern matching, testing a model’s ability to reason about novel visual and spatial puzzles it hasn’t seen before. Astra reaching 100% with OpenAI’s adapter marks the first time any model has fully maxed out this version of the test. Without the adapter, the model still performs well but falls short of Claude 5.2’s unassisted score, meaning the harness is doing real work here.

The AI Robotics leaderboard (version 0.1) measures different territory: how well a model handles the kind of physical and spatial reasoning robotics applications need, things like part relationships, mechanical constraints, and simulated physical interaction. Astra topping that leaderboard, ahead of Claude 5.2 and Gemini 4 Hydrogen, lines up with what the CAD and robotics demos show in practice. The model isn’t just generating pretty 3D renders, it’s reasoning about how physical systems fit together and move.

## Is GPT-6.2 Astra worth using if you’re not doing CAD or robotics work?

For general-purpose use, the answer from early testers leans toward “not clearly better than alternatives.” Multiple reviewers found Astra harder to steer, with one noting it produced oddly organized bullet-point summaries that lacked the logical flow of competing models. Visual and coding tasks that didn’t involve 3D generation showed no meaningful improvement over Claude 5.2, and for coding specifically, both Claude Opus 5.5 and Claude 5.2 appear to remain stronger picks right now.

*mission-critical*enterprise applications.

**911 Dispatch Console**Harbor County Emergency Services

Where Astra does win decisively is cost. On the Pareto frontier, the chart plotting benchmark score against cost per task, Astra sits close to the top-left corner, the ideal zone combining high accuracy with low cost. On Terminal-Bench Science 0.1, it scored close to 78% at under $10 per task, a result no other model matched at that price point. On the broader Artificial Analysis Intelligence Index, often treated as a more neutral cross-benchmark measure, Astra trails Claude 5.2 slightly but at roughly an eighth of the cost. For businesses running high volumes of CAD, simulation, or science-adjacent tasks, that efficiency gap may matter more than small quality differences.

## What does the AI self-improvement classification mean?

Alongside the benchmark results, OpenAI classified GPT-6.2 Astra as the first externally released model to meet its “high” threshold for AI self-improvement, meaning the model is capable enough at automating AI research and related tasks that OpenAI considers it a meaningfully different risk category. On a modified recursive self-improvement index, Astra scored 29%, behind Gemini 4 Argon but ahead of OpenAI’s own prior model generation. The index is calibrated so that 50% represents roughly human-level performance on each task, meaning current frontier models remain below that line but are closing the gap.

On Epoch AI’s Erdős Problems benchmark, a set of unsolved or under-solved math problems associated with mathematician Paul Erdős, Astra produced more complete solutions than its predecessor and reportedly solved one problem with no previously known solution, using a budget of $300 per question.

In response to these capability jumps, OpenAI added new safeguards: a 35-second kill switch triggered by detected misalignment, centralized safety infrastructure monitoring reinforcement learning runs, and a policy where any safety system outage halts inference entirely until resolved. The model also remains classified at OpenAI’s highest threshold for biological and chemical risk, a classification carried over from prior releases rather than new to Astra.

## Frequently Asked Questions

### Did GPT-6.2 Astra actually score 100% on ARC-AGI-3?

Yes, but only when paired with OpenAI’s custom adapter built for the benchmark. Without that adapter, Astra still performs well but scores lower than Claude 5.2’s result on the same unassisted test.

### Is GPT-6.2 Astra better than Claude 5.2 or Claude Opus 5.5?

Not across the board. On the Artificial Analysis Intelligence Index, a broad measure of general capability, Astra trails Claude 5.2 slightly. For coding specifically, Claude Opus 5.5 and Claude 5.2 appear stronger. Astra’s clearest advantages are in 3D/CAD generation, robotics tasks, and cost efficiency per task.

### What makes Astra’s CAD and 3D modeling demos notable?

Astra was shown generating fully modeled, mechanically simulated designs, including a 3D-printable pendulum clock with individually modeled gears, a Python-based physics solver verifying correct motion, and complete assembly instructions, plus an RC buggy and suspension system built part by part rather than from stock assets.

### Why did so many reviewers criticize the release?

Testers including Dan Shipper, Ethan Mollick, and Matt Schumer described Astra as harder to steer, inconsistent in quality, and in some cases a regression compared to prior OpenAI models, particularly for everyday writing and general-purpose tasks outside its 3D and robotics strengths.

### What is the AI Robotics leaderboard Astra leads?

It’s a version 0.1 leaderboard evaluating models on robotics-relevant reasoning tasks. GPT-6.2 Astra currently tops it, ahead of Claude 5.2, Gemini 4 Hydrogen, and OpenAI’s own earlier models.
