---
id: collect-261121-veille-ia/modeles-open/elorian-s-glimpse-is-a-visual-reasoning-model-that-crops-rewinds-and-measures-while-it-thinks
title: "Elorian's Glimpse Is a Visual Reasoning Model That Crops, Rewinds and Measures While It Thinks"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["glimpse", "elorian", "model", "visual", "astra", "gpt-6", "reasoning", "elorians"]
source: https://foundermag.co/launches/elorian-glimpse/
source_anchor: ""
source_lines: [1, 42]
section: "Modeles_Open"
---
## What launched

On October 16, 2026 Elorian, a Palo Alto AI startup, released its first model, **Glimpse**, as a research preview. It’s available now through an API and a chat interface on Elorian’s platform, with a free trial and no waitlist.

Elorian is led by CEO Andrew Dai, a former Google DeepMind researcher. It came out of stealth about six months ago with a **$55 million seed round at a $300 million valuation**, co-led by Striker Venture Partners, Menlo Ventures and Altimeter Capital, with Nvidia and Google chief scientist Jeff Dean also investing. Glimpse is what that money produced: Elorian says a small team trained it on a few hundred GPUs by post-training an open-weights base model.

## How Glimpse is different

Most multimodal models look at an image once, turn it into tokens and then reason in text. Elorian calls Glimpse a “visual thinking” model: while it reasons, it can go back into the image or video, crop into a region, pull specific frames, and run code to measure, count or compare what it sees. Elorian calls this a visual scratch space. Glimpse is also trained end-to-end as an agent that uses these tools and can spawn and coordinate subagents.

Elorian’s demos show the kinds of problems this helps with:

- **Tracking things through motion:** keeping track of which object is which after a camera pan, or working out whether a train is slowing down.
- **Counting by identity and state:** telling open pliers from closed ones in a cluttered tool tray, or counting individual puppies in a video.
- **Robotics:** labeling and segmenting teleoperation footage, and acting as the high-level planner for a robot policy (π0.5) on a simulated manipulation task.
- **Dense video captioning:** describing fine-grained movements and social dynamics, not just the main action.

## Benchmarks and price

Elorian compares Glimpse against OpenAI’s GPT-6 Astra and GPT-6.1 Sol, Anthropic’s Claude Opus 5.5 and Google’s Gemini 3.8 Flash, all run at maximum thinking effort in each vendor’s own agent harness. On its own evaluations, Glimpse leads on manipulation-event understanding (WGO Bench, 42.7 vs. 39.4 for Astra), short-video captioning (UGC-VideoCap, 82.7 vs. 80.3), event understanding (E.T. Bench) and open-vocabulary counting (FSC-147, 83.4 vs. 82.0), and ties Astra on embodied reasoning (ERQA, 83.75).

Elorian is also upfront about where Glimpse is behind. GPT-6 Astra beats it on BabyVision, dense video captioning (DREAM-1K), temporal reasoning (TOMATO), scene understanding (HSS) and product counting (CountQA), and GPT-6.1 Sol leads on long-video understanding with subtitles (Video-MME v2, 65.3 vs. 56.7). On scientific charts (CharXiv-R), Claude Opus 5.5 edges it out, 93.4 to 92.9. None of these results have been independently verified.

Price is the clearest win. Glimpse costs **$0.60 per million input tokens and $3 per million output tokens**. That compares with Elorian’s listed rates of $10/$50 for GPT-6 Astra, $2/$10 for GPT-6.1 Sol, $4/$20 for Claude Opus 5.5 and $0.75/$3.75 for Gemini 3.8 Flash. In Elorian’s robot-planning demo, one episode cost $0.035 with Glimpse versus $0.249 with Astra. The catch, which Elorian acknowledges, is that careful visual review uses more time and tokens, so per-task savings will be smaller than the per-token gap suggests.

## Why it matters

Visual reasoning is the part of frontier models that has improved least relative to text and code, and it’s the part robotics, lab automation and video-heavy businesses need most. Elorian’s bet is that a smaller, specialized model that actively inspects images can match or beat general-purpose frontier models on those tasks at a fraction of the price. The benchmark table makes that case partially: Glimpse is competitive with Astra on embodied and counting tasks while costing about 6% as much per input token, but it’s clearly behind on long and temporal video.

For most teams, that makes Glimpse a candidate for specific high-volume jobs rather than a general multimodal default: labeling robot or dashcam footage, counting and inventory from images, chart extraction, or short-form video captioning, where cost per item decides whether a pipeline is viable. Gemini 3.8 Flash is the closest price competitor, so it’s the model to test Glimpse against.

Two cautions. Glimpse is a research preview from a company that has never shipped a model before, so expect changes to behavior and pricing, and don’t build a production dependency on it without a fallback. And some of Elorian’s showcase examples are reviewed reference answers or recorded traces rather than live outputs, which Elorian notes on its own page. Run your own data through it before trusting the demos.

## Who should try it

- **Robotics and autonomy teams** that spend heavily on labeling or need a cheap high-level planner from camera input.
- **Media, e-commerce and research teams** captioning short video, counting objects or reading scientific charts at volume.
- **Developers comparing multimodal APIs on price:** benchmark Glimpse against Gemini 3.8 Flash on your own images and videos.

## Where to try it

Sign up for the API or chat at platform.elorian.ai; the full benchmark write-up is on Elorian’s blog.
