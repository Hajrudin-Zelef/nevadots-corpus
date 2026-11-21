---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-for-robotics-what-it-does-and-what-it-costs
title: "Gemini 4 Argon for robotics: what it does and what it costs"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-13"]
keywords: ["argon", "google", "robot", "does", "gemini", "model", "robotics", "video"]
source: https://www.eesel.ai/blog/gemini-4-argon-for-robotics?ref=corti.com
source_anchor: ""
source_lines: [1, 135]
section: "Gemini_Google"
---
Gemini 4 Argon is a general frontier model, not a robotics model, but on 13 October 2026 Google published a robotics post for it and the model went generally available in the Gemini API the same day. Google reports 87% on the ERQA physical-reasoning test, ahead of Claude Opus 5.5 at 82.3% and GPT-6 Astra at 81.0%, and shows Argon writing physics simulations, labelling robot video, and steering a Boston Dynamics Spot.

The catch is where Argon sits in the stack. It is a planner and code-writer you call over the API, and it does not support the Live API, so it is not the model in your real-time control loop. That job belongs to Gemini Robotics 2 and its ER 2 endpoints. My take: use Argon before the robot moves (simulation, data labelling, planning code) and ER 2 while it moves.

I build integrations and APIs at eesel, so I read model docs the way an integrator does: what does the endpoint accept, what does it bill, what can't it do. The same rehearse-before-you-touch-the-real-thing idea is why eesel's AI helpdesk teammate gets tested against your past tickets before it answers a live one.

## What does Google say Argon does for robots?

Google's robotics post, written by principal engineer Kanishka Rao, frames Argon as "a general-purpose physical reasoning model" and groups the demos into four jobs: robot control, Real2Sim (turning real footage into a physics simulation), physical understanding of video, and research and self-improvement. Nothing in it says Argon is a new robot brain, and it reads more like a frontier model being pointed at a new field. It says Argon is useful around the robot.

The headline number is ERQA, a test of physical and spatial questions in 3D scenes. Google's chart is tools-off with 400 questions, and Argon leads it.

Google's own ERQA chart for Gemini 4 Argon, redrawn with the same four scores.

Two cautions before you quote that chart. It is Google-run, so treat it as a vendor claim until someone repeats it. And it is not comparable to the ERQA bar on the Gemini Robotics ER 2 page, which shows ER 2 at 78.5% against a different comparison set, including Opus 5 at 67.2%. Different model lists and a different harness mean you can't subtract one from the other. If you pick between Argon and ER 2 on this number alone, you are guessing.

The video score is the other one that matters for robots. On LVBench, a long-video understanding test, Google's Argon table lists 91.7%, against 87.5% for GPT-6 Astra, 83.7% for Claude Opus 5.5 and 79.7% for Claude Fable 5.1.

## Which robot jobs does Argon actually do?

I went through every demo in the post and the DeepMind page. The pattern is consistent: Argon writes code, builds simulations and judges video (the same agentic vision idea Google has pushed since earlier Gemini models), and the physical robot is downstream of that work.

- **Cornhole on a humanoid.** In an internal test, Argon watched a cornhole video, mapped the space, worked out whole-body joint movements and wrote the simulation code. On Apptronik's Apollo humanoid the robot sank`7` consecutive shots. Google calls the pipeline "Video-to-Sim-to-Real" and says it turns weeks of simulation modelling into hours.
- **Spot and a foot-pedal bin.** Argon controls a Boston Dynamics Spot (12-DoF legs, 6-DoF arm). It builds a MuJoCo digital twin of the bin and practises there before stepping on the real pedal.
- **Simulation data for a VLA.** From one photo of a real tabletop, Argon built a calibrated MuJoCo twin and collected`4,500+` randomised episodes. Google says this lifted a real robot's success rate from`56%` to`75%` and cut completion time from`35s` to`24s` .
- **Video labelling.** Given 136 seconds of a bimanual ALOHA 2 robot folding an origami fox, Argon produced a four-level label hierarchy: 1 goal, 6 subtasks, 17 contact-level primitives, plus quality control that holds progress flat during fumbles.
- **Value function.** On a 60-second ring-sorting episode it split the trajectory into`28` segments with positive, neutral or negative progress.
- **Self-improvement on a 6-DoF arm.** A pick-and-place task on an SO-101 arm went from`58.9s` to`22.9s` to`17.4s` over three iterations of trial and self-reflection.
- **Paper reproductions.** One prompt per paper,`68` interactive MuJoCo WebAssembly sandboxes.

Two partner statements on the DeepMind page back the video side. Cartwheel's chief scientist, Andrew Carr, says that after plugging Gemini 4 into its 3D reconstruction engine, runtime fell nearly 100x and token use fell 90% on their benchmarks. Roboflow says Argon ranked first in its object detection evaluations, on manufacturing, logistics and aerial images. Both are partner statements published by Google, so read them as claims, not audits.

## Argon or Gemini Robotics ER 2: where does each one sit?

Google splits its robotics line into a motor model and a reasoning model, a very different design from a general model such as Gemini 3.8 Flash. Gemini Robotics 2 is the vision-language-action model that turns camera input into motor control, and ER 2 is the planner above it. The ER overview in the docs lists spatial reasoning, agentic vision, task orchestration, video progress and a streaming endpoint. Every code sample there uses `gemini-robotics-er-2-preview`. Argon does not appear in the robotics docs at all.

The practical split, from the docs rather than my opinion: the Argon model page lists Live API as not supported, while the `gemini-robotics-er-2-streaming-preview` endpoint exists for real-time streaming robot agents.

| Job | Reach for | Why | 
|---|---|---|
| Turn robot or human video into a physics sim | Argon | Code, 3D reasoning and long video in one model, per Google's demos | 
| Label or score long robot episodes | Argon (or ER 2 for short clips) | Agentic video mode cuts video tokens by up to 88% on long footage | 
| Write control or training code in a long session | Argon | `1M` unified context and up to`1M` output tokens with long decode | 
| Real-time agent that streams camera and audio | ER 2 Streaming | Built on the Live API; Argon doesn't support it | 
| Success detection and progress checks while running | ER 2 | Has video progress classification as a named capability | 
| Planning that hands off to a motor policy | ER 2 | Google describes it as the layer that hands motor execution to a VLA | 
| Model on the robot with no network | Neither | Google's on-device option is Gemini Robotics On-Device 2 | 

One wrinkle on ER 2: the docs say it builds on Gemini 3.5 Flash, and the API changelog now lists `gemini-3.5-flash` as deprecated and routed to Gemini 3.6 Flash. I found no statement about what that means for ER 2, so I wouldn't assume either way. ER 1.6, the older robotics endpoint, was shut down on 31 August.

## What can Argon's API do that matters for robotics?

The Gemini 4 Argon guide gives the spec sheet, and a few lines of it matter more to robotics builders than the benchmarks.

- **One `1,048,576`-token window** shared by input, thinking and output, with three thinking levels (`low` ,`medium` ,`high` ).
- **Long decode** to`1M` output tokens, run as background execution in hops of up to 64K, which suits long simulation or control programs.
- **Video in, text out.** Inputs are text, image, video, audio and PDF; output is text only.
- **Function calling, code execution, structured outputs** and computer use in preview, which is what AI agent API integrations rely on, and what an API-first agent platform or an MCP server needs to hook a model into your own tools.
- **Default video is 1 frame per second.** That is fine for labelling and bad for anything reactive. Agentic video mode is opt-in and Google says it is best for footage over 5 to 10 minutes; for clips under 5 minutes it recommends the static default.

Google's video token math is roughly `100` tokens per second at default resolution and `300` at high. By my arithmetic, one hour at high resolution is about 1.08M tokens, which does not fit in Argon's window in a single request. Plan on clips or agentic mode.

## What does Argon cost for robot work?

Argon is `$2` input and `$10` output per million tokens through 30 November 2026, then `$4` and `$20` from 1 December, per the Gemini API pricing page. Cached input is `$0.10`, rising to `$0.20`, and it is implicit caching only. Output includes thinking tokens, which matters because a long reasoning run on a simulation task will mostly be billed as output. For the wider lineup see the Gemini pricing guide. The full Argon breakdown is in the Argon pricing guide.

| Endpoint | Input / output per 1M, intro | Input / output per 1M, later | Free tier | 
|---|---|---|---|
| Argon, standard | `$2` /`$10` (to 30 Nov 2026) | `$4` /`$20` (from 1 Dec 2026) | No | 
| Argon, batch or flex | `$1` /`$5` | `$2` /`$10` | No | 
| Argon, priority | `$3.60` /`$18` | `$7.20` /`$36` | No | 
| ER 2 preview, standard | `$1` /`$5` (to 31 Dec 2026) | `$2` /`$10` (from 1 Jan 2027) | Yes | 
| ER 2 preview, batch | `$0.50` /`$2.50` | `$1` /`$5` | No | 
| ER 2 streaming preview | `$1` /`$5` | `$2` /`$10` | Yes | 

The date mismatch is the thing to plan around. Argon's price doubles a month before ER 2's does, so for most of December you pay double for Argon while ER 2 is still on its intro rate.

Input-only cost of an hour of video at default resolution, from Google's token rate and price pages.

Here is a worked example. Say you label 1,000 robot episodes of 136 seconds, the length of Google's origami clip. At 100 tokens a second that is `13,600` input tokens each, or `13.6M` in total. I assumed `4,000` output tokens per episode including thinking, so `4M` output. That output figure is my assumption, not a Google number.

| Route | Intro price | Later price | 
|---|---|---|
| Argon, standard | `$67.20` | `$134.40` | 
| Argon, batch | `$33.60` | `$67.20` | 
| ER 2, standard | `$33.60` | `$67.20` | 
| ER 2, batch | `$16.80` | `$33.60` | 

The cost is not the story here; it's small either way. The story is that Google publishes no ER 2 or Argon result for this labelling task on the same footage, so the cheaper route may or may not label as well. Run 50 clips through both and compare before you commit 1,000.

## What do robot builders say about it?

The Hacker News thread reached `201` points. The most useful comments split into two camps: one tinkerer who tried it on a real robot, and a debate about where the model should run.

"I am very new to robots and recently decided to go crazy and buy a humanoid from Unitree. I've spent a while building some tools in order to integrate LLMs with vision capabilities that can control the robot motion in a simple manner... and while I saw Opus and Astra crashing against tables, beds and surfaces (yeah, my LIDAR data isn't being used yet), Argon 4 takes a lot of precautions to avoid crashing against walls and other objects!"


That is one hobbyist anecdote on one robot with no lidar, so it is a data point, not a benchmark. It does line up with the spatial-reasoning story, and the Gemini vs Claude debate is mostly about this kind of real-use feel.

On the deployment question, one commenter argued that most applications just need a link to a data centre:

"I think for 95%+ of applications.. you just need a link to a data center anywhere. My assumption is the hardware will have one/several smaller models running on it for locomotion, balance, etc. Then the hardware 'phones home' for decision making and planning."


That matches how Google splits the work, and how domain-specific models generally pair with a bigger planner: a cloud planner like Argon or ER 2 on top, a lighter policy near the motors. Another commenter, jacquesm, pushed back that robotics sits at "the worst possible intersection point" of power budget, model size and cost, which is the strongest argument for On-Device 2 over a hosted model.

## What I could not verify

I did not run Argon on a robot, and I want to be plain about the limits of this post.

1. **Every robotics result is Google's.** The cornhole run is described as an internal test. The`56%` to`75%` simulation result has no public task list or code I could rerun.
2. **No latency figure.** I found none in the robotics post or the API docs, which makes a closed control loop on Argon an open question. Google's own control demos run step by step over several turns.
3. **No same-harness Argon versus ER 2 comparison.** The two ERQA charts use different model lists, as above.
4. **I did not call the API today.** The changelog and model page list`gemini-4-argon` as generally available from 13 October. Google's launch post says developers get it through AI Studio and the Gemini platform, while the robotics post says the AI Studio app is for Google AI Ultra subscribers, so check your own account.
5. **Safety.** Google's launch post describes safeguards against misuse such as cyber and bio, and I found no robot-specific safety guidance. Anything that moves near people needs its own testing.

## Where would I start?

If I were a robotics team this week, I would do it in this order.

1. **Pick the offline job first.** Real2Sim and video labelling are the two places where Argon's demos are most concrete and a wrong answer costs you an hour, not a broken arm.
2. **Rehearse in simulation.** Google's own Spot demo practises in MuJoCo before touching the real pedal. Copy that habit, whatever model you use.
3. **Keep ER 2 for the loop.** If you need streaming, success detection or multi-robot orchestration, start with the endpoints Google documents for it, and read the Gemini Robotics 2 breakdown.
4. **Run a 50-clip bake-off** on Argon and ER 2 using your own footage, then price the winner on batch.
5. **Check the alternatives.** If you are already on another frontier model, such as Kimi K3 or Opus 5.5, the Argon alternatives guide and the independent review cover where it loses, mostly on terminal-heavy coding. See also Opus 5.5 pricing and GPT-6 Astra pricing, Fable 5.1 pricing and the Opus 5.5 alternatives if you are weighing cost.

## Try eesel

Argon is infrastructure: a model you wire into your own robot stack. eesel is the employee. If the queue that worries you is your robotics company's own customer support, eesel's AI helpdesk teammate joins your existing helpdesk, learns from your docs and past tickets, and gets rehearsed on those tickets before it answers a live one, the same sim-before-real habit this post is about. You can start free, and it works with whichever model is best that month, so a better Gemini later is an upgrade rather than a rebuild. The AI employee guide covers how that ready-to-work idea extends to other jobs. If you would rather see it first, read the guide to AI teammates.

eesel's activity view: every action the AI teammate takes on a ticket is logged and filterable, so you can review before you widen its scope.

## Frequently Asked Questions

`gemini-4-argon` as generally available from 13 October 2026, paid tier only, with no free tier. The robotics post says the AI Studio app is for Google AI Ultra subscribers, so your access depends on how you sign in. The base Gemini 4 Argon guide covers the launch.
