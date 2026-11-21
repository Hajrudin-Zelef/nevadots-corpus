---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2
title: "Claude Fable 5.2"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["claude", "fable", "anthropic", "opus", "tool", "thinking", "every", "model"]
source: https://ai.xmc.tw/en/models/claude-fable-5-2
source_anchor: ""
source_lines: [1, 44]
section: "Claude_Anthropic"
---
### Overview

Claude Fable 5.2 is Anthropic's current Fable model, released on November 9, 2026. Anthropic positions it for demanding reasoning and long-horizon agentic work, with typical uses such as agent sessions that run for hours, multistep deep research, and analysis carried through to a finished document, spreadsheet or deck. It takes text and images as input and returns text, with a 1M-token context window, up to 128K output tokens and a reliable knowledge cutoff of June 2026.

### Strengths

- Long, multistep agentic and research work. Anthropic's model guide places it above Claude Opus 5.5 for the hardest tasks.
- Adaptive thinking is always on. Thinking depth is set with the effort parameter (`low` to`max` , default`high` ).
- Supports the computer use toolset, compaction on demand (beta) and mid-conversation tool changes (beta).
- Its Artificial Analysis Intelligence Index on this site is 65, against 59 for Claude Opus 5.5.

### Use cases

Anthropic suggests starting most workloads on Claude Opus 5.5 and moving to Fable 5.2 when evals on Opus at higher effort still fall short. Typical fits are multi-hour autonomous agents, research tasks with many tool calls, and long finished deliverables.

### Limitations

- Thinking can't be turned off. Forced tool use (`tool_choice` of`any` or`tool` ) returns a 400 error, and`temperature` ,`top_p` and`top_k` should be omitted.
- Requires 30-day data retention. It isn't available under zero data retention unless Anthropic expressly authorizes it.
- Editing earlier turns invalidates its thinking blocks on every account, and only Fable 5.2 and Mythos 5.2 can read those blocks.
- Latency is listed as moderate. At `high` effort and above it thinks before almost every reply, so simple requests can be slow or costly.
- It runs safety classifiers, and a declined request returns `stop_reason: "refusal"` .

### Versions & pricing notes

- $8 per million input tokens and $40 per million output tokens. Cache reads cost $0.20 (2.5% of input) and 5-minute cache writes $10. The Batch API is 50% off, and fast mode costs $16 / $80.
- Cheaper than Fable 5.1 and Fable 5 ($10 / $50), and twice the price of Opus 5.5 ($4 / $20).
- Claude Mythos 5.2 has the same specifications and pricing but is limited to organizations verified through Anthropic's programs.
- Available on the Claude API, Amazon Bedrock, Google Cloud and Microsoft Foundry. Retirement is not sooner than November 9, 2027.

### Hands-on: Blender low-poly blacksmith

Tested by the site owner: a single run, not a benchmark. Test date: 2026-11-11 (the first five models were tested 2026-11-06 to 11-08). The same Chinese prompt asked each model to build a low-poly medieval blacksmith shop in the local Blender install using Python (bpy), with no downloaded models; render a 1920×1080 image; save a .blend; and export a .glb.


Claude Fable 5.2 finished every step in Blender 5.2.3 LTS. Its code is one entry script plus nine modules (about 3,150 lines). By file times it took about 3 hours, including about 2 hours between 10:20 and 12:26 with no file changes.

- **Requirements: all 7 met.** A ground floor of 231 stones, a half-timbered gable, a roof of 500-odd slate shingles, a stone chimney with flashing, a plank door with iron strap hinges and shuttered windows; a forge, bellows, an anvil holding a hammer, tongs and a glowing blade, a quench barrel, a grindstone and a workbench, plus an outdoor weapon rack with three swords, an axe, a war hammer and a spear; 33 of 36 materials are procedural node materials, with no image textures; 6 lights and 4 cameras; four 1920×1080 renders; a .blend and a .glb.
- **Look:** the cleanest and most consistent scene of the eight. Every prop is easy to read, and the forge and the weapon rack each get a close-up.
- **Self-correction:** it ran six preview passes and added temporary cameras for close-ups of the anvil, chimney, door and shed. Changes visible in the images: a tighter main shot, the gap between chimney and roof closed, tools added to the anvil, and the white flames turned orange. Every kept log is free of Python errors; if anything failed before the first render, it was not logged. At the end it reopened the .blend to check it, and imported the .glb into an empty scene and rendered it; the exported model looks almost the same as the final render. It left no written report, but the numbers in its JSON build report match both checks.
- **Remaining problems:** the island floats on an empty grey-blue gradient with no horizon, so it has less atmosphere than DeepSeek's sunset. The smoke is a chain of white icospheres and the flames are small cones. The window panes are flat glowing panels, the axe blade looks washed out, and the weapon rack is small in the main shot. At 65,165 triangles it is about half of DeepSeek's count.
- **Verdict:** the best result of the eight, ready to show. Its lead over DeepSeek comes from finish and tidiness; the two scenes are similar in scope.

The full comparison of eight models, with scores, is in Hands-on: a Blender low-poly blacksmith.
