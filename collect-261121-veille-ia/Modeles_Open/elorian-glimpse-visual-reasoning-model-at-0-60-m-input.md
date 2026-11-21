---
id: collect-261121-veille-ia/modeles-open/elorian-glimpse-visual-reasoning-model-at-0-60-m-input
title: "Elorian Glimpse: Visual Reasoning Model at $0.60/M Input"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["elorian", "glimpse", "model", "visual", "reasoning", "gpt-6", "input", "astra"]
source: https://evermx.com/case/elorian-glimpse
source_anchor: ""
source_lines: [1, 112]
section: "Modeles_Open"
---
# Elorian Glimpse: Visual Reasoning Model at $0.60/M Input

Elorian's Glimpse reasons with a visual scratch space and costs $0.60/$3 per million tokens. Benchmarks are the company's own.

## Introduction

On October 16, 2026, Elorian published "Introducing Glimpse," the debut model from the AI lab founded by former Google DeepMind researcher Andrew Dai, who is the company's CEO. Glimpse is a research preview available through an API and a chat interface. Elorian describes it as a visual thinking model: a system that reasons about the physical world from what it sees, following objects through motion and changing viewpoints and connecting an observation to what happens next. According to Elorian, the team built it with a few hundred GPUs by post-training an open-weights base model. The benchmark claims in this article are Elorian's own evaluations, not independent measurements.

## Feature Overview

### A visual scratch space

Most multimodal models look at an image once, convert it to tokens, and then reason in text. TechCrunch's description of Glimpse is that it keeps returning to the source: mid-reasoning it can crop into a region, pull specific frames from a video, or run code to measure and compare what it sees. Elorian calls this a "visual scratch space," and its chat platform shows the scratch space as the model works, for example zooming into a region or rewatching a short interval at a denser frame rate. In one published trace, the model moves from an 8 fps inspection to roughly 14 fps over a 0.9-second interval to decide whether a shot is a camera pan or a hard cut.

TechCrunch notes the idea is not new: OpenAI added crop-and-zoom during reasoning to its o-series models in 2025. Elorian's pitch is that it built the whole model around the mechanism, at low cost.

### Agentic tool use and subagents

Elorian says Glimpse is trained end-to-end to use multimodal tools that write code, transform images, and watch videos, and that it can spawn and coordinate subagents that exchange text, visual artifacts, and internal representations.

### Benchmarks (Elorian's own evaluations)

Elorian reports that Glimpse exceeds GPT-6 Astra on four evaluations and matches it on a fifth. The blog's Table 1 compares Glimpse with GPT-6 Astra, GPT-6.1 Sol, Opus 5.5, and Gemini 3.8 Flash.

| Evaluation | Glimpse | GPT-6 Astra | GPT-6.1 Sol | Opus 5.5 | Gemini 3.8 Flash | 
|---|---|---|---|---|---|
| WGO Bench (manipulation events) | 42.70 | 39.42 | 38.91 | 12.34 | 33.99 | 
| ERQA (embodied spatial reasoning) | 83.75 | 83.75 | 82.45 | 82.20 | 81.20 | 
| CharXiv-R (scientific chart reasoning) | 92.90 | 92.10 | 92.60 | 93.40 | 91.80 | 
| UGC-VideoCap (short-form video captioning) | 82.66 | 80.32 | 80.41 | 76.35 | 78.33 | 
| FSC-147 (open-vocabulary counting) | 83.36 | 82.02 | 82.69 | 73.78 | 67.06 | 
| BabyVision (fine-grained visual reasoning) | 95.36 | 97.68 | 97.16 | not reported | 83.25 | 
| TOMATO (fine-grained temporal reasoning) | 93.60 | 96.02 | 89.02 | 72.44 | 83.02 | 
| CountQA (product image counting) | 82.66 | 86.42 | 86.42 | 82.20 | 81.09 | 

The same table also lists HSS, DREAM-1K, Video-MME v2 with subtitles, and E.T. Bench, where Glimpse trails GPT-6 Astra on HSS (46.36 vs 53.6), DREAM-1K (76.04 vs 79.05), and Video-MME v2 (56.68 vs 64.06). The picture is mixed: Glimpse leads on some robotics, captioning, and counting tests, while the larger models lead on several perception and temporal-reasoning tests. Opus 5.5 has the highest CharXiv-R score in the table (93.40). Elorian's footnote says results use each model's highest thinking effort and native agent harness where available, and that HSS includes paper-reported results.

### Pricing

Elorian lists Glimpse at $0.60 per million input tokens and $3.00 per million output tokens, which TechCrunch also reports. In the blog's table, GPT-6 Astra is $10.00 and $50.00, GPT-6.1 Sol $2.00 and $10.00, Opus 5.5 $4.00 and $20.00, and Gemini 3.8 Flash $0.75 and $3.75 at introductory rates. Elorian states that this makes Glimpse 94% cheaper than Astra, 85% cheaper than Opus 5.5, 70% cheaper than Sol, and 20% cheaper than Flash's introductory rates. The blog also acknowledges that "looking that closely takes time, and tokens," and names token efficiency as its next focus.

## Usability Analysis

The blog's worked examples target tasks where text-only reasoning struggles: judging whether a train is decelerating by tracking one window edge across frames, counting only the open pliers in a photograph, or keeping individual puppies numbered in a handheld video as the camera pans away and back. Several examples are labeled as source footage with reviewed reference answers rather than recorded model traces, so readers should treat those as illustrations of the task, not proof of model behavior.

On the application side, Elorian shows Glimpse acting as a planner for a robot policy, Physical Intelligence's pi0.5, on a LIBERO task. In that single episode both Glimpse and GPT-6 Astra completed the task; Elorian reports planning costs of $0.035 for Glimpse and $0.249 for Astra. Elorian also cites dense video captioning and labeling teleoperation data. TechCrunch reports the initial targets are companies with large video archives: robotics firms, scientific labs, media, and creative companies.

## Pros and Cons

On the plus side, the pricing is far below the frontier models in Elorian's own comparison, the scratch-space traces make the model's visual evidence inspectable, and the results are competitive on several video and counting tests. On the minus side, the benchmarks are self-reported, the model trails the larger systems on a number of tests, it consumes extra tokens when it looks closely, and it remains a research preview.

## Outlook

Elorian says it is working toward multimodal-native pretraining, models that both understand and generate, and direct action outputs for robots; the blog states that calibrated reconstruction and robot control remain work ahead. TechCrunch reports the company came out of stealth six months ago with a round co-led by Striker Venture Partners, Menlo Ventures, and Altimeter Capital, with participation from Nvidia and Google chief scientist Jeff Dean, and that Dai said the company aims to build "the world's best visual reasoning model" next year.

## Conclusion

Glimpse is a credible, inexpensive entry for teams that process lots of video or chart-heavy images, and its inspectable reasoning is a practical selling point. Until independent evaluations appear, the numbers should be read as vendor claims. It suits developers in robotics, scientific imaging, and media who can test it against their own data.

## Editor's Verdict

Elorian Glimpse: Visual Reasoning Model at $0.60/M Input earns a solid recommendation within the Other LLM space.

The strongest case for paying attention: priced at $0.60/$3.00 per million tokens, far below the larger models in Elorian's own comparison. That alone raises the bar for what readers should expect in this space. Reinforcing that, inspectable visual scratch space shows which regions and frames the model relied on — practical value rather than just headline appeal. The broader signal worth registering is straightforward: all benchmark comparisons are Elorian's own evaluations, so independent replication is the key open question. On the other side of the ledger, one constraint is real rather than a marketing footnote: benchmark results are the vendor's own and not independently verified. It should factor into any serious decision. Layered on top of that, trails GPT-6 Astra on BabyVision, TOMATO, CountQA, HSS, DREAM-1K, and Video-MME v2 — which narrows the set of teams for whom this is an obvious yes.

For multi-model deployment teams, cost-conscious operators, and developers willing to evaluate beyond the major labs, this is a serious evaluation candidate, not just a curiosity to bookmark. For everyone else, the safer posture is to monitor coverage and revisit once the use cases that matter to your team are demonstrated in the wild.

### Pros

- Priced at $0.60/$3.00 per million tokens, far below the larger models in Elorian's own comparison
- Inspectable visual scratch space shows which regions and frames the model relied on
- Competitive or leading on several video captioning, counting, and manipulation-event tests per Elorian
- Available now via API and chat interface

### Cons

- Benchmark results are the vendor's own and not independently verified
- Trails GPT-6 Astra on BabyVision, TOMATO, CountQA, HSS, DREAM-1K, and Video-MME v2
- Careful visual review consumes extra time and tokens, per Elorian
- Research preview; robot control and calibrated reconstruction are still future work

### References

### Comments0

### Key Insights

- All benchmark comparisons are Elorian's own evaluations, so independent replication is the key open question.
- Glimpse leads GPT-6 Astra on four of the blog's tests and ties on one, but trails it on several perception and temporal-reasoning tests.
- Opus 5.5 has the highest CharXiv-R score in Elorian's table, so the scientific chart claim is against Astra specifically.
- TechCrunch notes OpenAI added crop-and-zoom during reasoning to its o-series models in 2025, so the mechanism itself is established; Elorian's bet is building the whole model around it.
- Elorian acknowledges that close visual inspection costs time and tokens and lists token efficiency as its next focus.
- The robot-planning demo is a single LIBERO episode, so the $0.035 versus $0.249 cost gap is illustrative, not a general figure.
- Direct robot action output is described as future work, not a current capability.

## Related AI Reviews

### Grok Bot Background Check-Ins: 31 Unattended Calls Logged

NewCore says two Grok Bots made 31 unattended reads of work apps; an admin toggle arrived Nov 12, on by default. NewCore sells the gateway.

### Grok Build Apps Exposed: 35,578 Open Links Found by Claroc

Claroc tried 43,551 grok.me app addresses; 35,578 opened with no sign-in, including 24 built by SpaceXSI and X staff.

### Underdog Woof: A 31.7MB Speech Model That Runs in a Browser

Underdog's Woof text-to-speech model is a 31.7 MB browser download with 30.7M parameters; weights are licensed for non-commercial use.

### Meta Muse Spaces: Build Your Own Tools by Describing Them

Meta added spaces to Muse on Nov 6: custom trackers, planners and games built from a description. Sharing spaces with others is still being explored.
