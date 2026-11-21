---
id: collect-261121-veille-ia/regulation-eu/tesla-s-roadmap-now-looks-like-xpeng-s-put-the-model-in-the-car
title: "Tesla's Roadmap Now Looks Like XPeng's: Put the Model in the Car"
domain: regulation-eu
role: reference
task: article
actors: []
dates: []
keywords: ["tesla", "roadmap", "model", "tops", "chip", "account", "memory", "published"]
source: https://evconduit.com/blog/tesla-s-roadmap-now-looks-like-xpeng-s-put-the-model-in-the-car
source_anchor: ""
source_lines: [1, 66]
section: "Regulation_EU"
---
Tesla does not publish much, so when it refreshes a roadmap slide the Chinese tech press reads every pixel of it. On 8 November the account **电动知士大雨** summarised what came out: the plan for the chips after AI5, and a training chip that was cancelled a year ago coming back in a new shape.

The details matter less than the direction of travel, and the direction is memory. Tesla's cars today carry **16 GB**. The next chip carries **96 GB**. The one after that carries **144 GB**. Whatever else is happening in that roadmap, the company is sizing its vehicle computer for models that do not fit in one today.

One reader, responding to this roadmap, put the strategic reading plainly: *"Looks like Tesla are following Xpeng and running more of the model on the car instead of edge compute."* That is worth taking seriously — and worth testing, because two of its clauses are right, one is a category slip, and the chronology is the opposite of what it implies.

## What Tesla actually published

The account compiled it into a single table from Tesla's investor material, its SEC 8-K and the trade press — worth saying, because the numbers below are a synthesis of what Tesla has shown, not a Tesla publication.

First, a naming change that explains why every Chinese write-up calls these chips "SI". Tesla has moved from "AI" to **SI — Super Intelligence** — for its accelerators, so the current car chip, formerly HW4 and then AI4, is now **SI4**. Musk has reportedly confirmed that SpaceXAI will be renamed SpaceXSI for the same reason. The roadmap as the account laid it out:

| Chip | Memory | Compute (claimed) | Timing / foundry | 
|---|---|---|---|
| **SI4** (HW4/AI4, in cars now) | 16 GB GDDR6 | not published; ~216–576 TOPS by estimate | Samsung 7nm-class, FCBGA | 
| **SI5** (HW5/AI5) | 96 GB (GDDR6/7 TBD) | 40× SI4; ~8,640–23,040 TOPS by estimate | TSMC 3nm + Samsung 2nm, MCM; 250–800 W; described as near production | 
| **SI6** (AI6) | **144 GB** (GDDR7 TBD) | 80× SI4; ~17,280–46,080 TOPS by estimate | 2nm-class, Samsung through 2033, MCM; **2029–2030** | 
| **SI7** | — | — | no published parameters; Musk says a more radical design needing a new plant | 

*The roadmap as the account compiled it from Tesla's investor material, the SEC 8-K and trade coverage. "SI6" is AI6 under Tesla's new naming; Dojo D3's HBM capacity is not published. Image: 电动知士大雨 (Weibo).*

Two things about that table are worth flagging before anyone quotes "46,000 TOPS". The multiples are against SI4, and **Tesla has never published SI4's TOPS**. The 17,280–46,080 range exists because the same analysts who produced it estimate SI4 at somewhere between 216 and 576 TOPS, derived from Musk's claim that HW4 is 3–8× HW3, which was rated at 72 TOPS. The memory and packaging numbers are harder facts; the compute number is arithmetic on an unpublished baseline.

There are also two errors on the slide deck itself, which the account's note caught: the SI4 process is labelled 5nm-class when the chip is Samsung's 7nm-class part, and the memory row says "8× more memory" where 16 GB to 144 GB is nine times. Both are the same fact written two ways, and both are the kind of slip that turns into a hundred wrong citations.

The other change is **Dojo D3**. The first Dojo was a wafer-scale system — 25 processors on one wafer, 11 GB of SRAM between them — and Musk cancelled that programme in August 2025. D3 comes back as something conventional: **chiplets with 2.5D/3D packaging and stacked HBM** — the same recipe the rest of the industry uses for training accelerators — with its HBM capacity unpublished. Tesla says it is optimised for "space, training capability and scalability", and specifically for **extraterrestrial deployment** — which, if it means anything, means radiation hardening.

## Where the reader is right

**The model is moving into the car.** A 144 GB vehicle computer is not a sensor-fusion chip. It is a host for a model, and the size of the model you can run locally is set by memory and bandwidth long before it is set by peak TOPS.

That is the argument XPeng has been making, and shipping. Its **Turing** chip is rated at 750 TOPS effective, with XPeng quoting support for models up to 30 billion parameters; the top triple-chip configuration in the G9L and GX is **2,250 TOPS**; the Robotaxi built on the GX runs **four Turing chips for 3,000 TOPS on the car**. XPeng's VLA 2.0 is an end-to-end model running on the vehicle, and it has also put a language model in the cockpit locally with its Banma partnership rather than calling a server.

So the direction the reader identifies is real: the industry's answer to "where does the model run" has been moving onto the car, and Tesla's roadmap now says the same thing in memory sizes.

## Where it needs care

**The chronology is inverted.** Tesla has designed its own inference silicon since HW3 in 2019 — seven years and two generations before XPeng's Turing shipped. If "following" means the decision to build a custom on-car accelerator instead of buying NVIDIA's, Tesla did it first and XPeng followed.

**"On the car instead of edge compute" is a category slip.** A car is the edge. Running inference on the vehicle *is* edge computing; the thing it replaces is off-board compute — a cloud or a roadside server. The real debate is car versus cloud, not car versus edge.

What the reader is right about, once those are corrected, is narrower and more interesting: **on shipped capability, XPeng is ahead right now.** Tesla's cars run 16 GB and an SI4 rated at perhaps 200–600 TOPS. XPeng ships 750 TOPS in an entry configuration and 2,250 in the top one, and claims parameter counts that Tesla has not had to claim because it has not yet shipped the memory to hold them. The roadmap closes that gap in 2029–30, which is a long time to be behind on paper in a market that is buying on paper.

## Other opinions worth putting next to that one

**"Discount Musk's claims by half."** A sceptical comment under the original post: 马斯克吹的先打个五折吧. It is crude and it has been good advice for a decade. The specific risk here is the timeline, not the silicon — SI6 is a 2029–2030 part, and the last big chip promise of this kind, wafer-scale Dojo, ended up cancelled.

**The comparison that flatters nobody.** By the time SI6 ships, NVIDIA's Rubin is already in customers' hands: ~50 NVFP4 PFLOPS for inference, ~35 for training, on a datacenter part. Tesla's 46 POPS (if the high estimate holds) is a car part and a different category, but it will arrive four years after Rubin and will be judged against whatever NVIDIA sells in 2029.

**The bandwidth view, which is ours.** We have made this point before about XPeng's own hardware: what runs on a car is limited by memory bandwidth, not TOPS. For reference, NVIDIA Orin is 205 GB/s, Thor 273 GB/s, NIO's Shenji 546 GB/s, and Tesla's HW4 around 448 GB/s from eight GDDR6 packages. SI5's projected **768 GB/s to 1.5 TB/s** — depending on whether it gets GDDR6 or GDDR7 — is the number that actually determines whether a large model runs well on the car. SI6's bandwidth is unpublished, which is why the 144 GB headline is only half a specification.

**The cloud counter-argument.** The case for keeping inference off the car is that compute then scales without reselling hardware, and a fleet can share one model. The case against is latency, dead zones, data-residency rules and the cost of a per-query server bill. Tesla has chosen the car, and so has XPeng — but note that both still train on datacenter hardware and both still farm data back from the fleet. The car is the inference site, not the whole system.

**The packaging and thermal cost.** 250–800 W is a lot of heat to remove from a passenger car, and MCM packaging is expensive. Tesla's move to chiplets for Dojo D3 and its reuse of the SI5 MCM for SI6 both look like cost containment, not ambition.

## The bottom line

Tesla's roadmap is a memory roadmap, and memory is what decides how much of the model lives in the car. On that axis the reader's instinct is sound and the ownership of the idea is contested: Tesla built the first custom inference chip for a car and lost the lead on shipped capability; XPeng took the same bet later, put it in production sooner, and is now selling cars whose on-car compute is larger than Tesla's current generation.

The interesting question for 2029 is not whether Tesla follows XPeng or XPeng follows Tesla. It is whether either of them needs 144 GB in a car at all — and what model they expect to be running there when the part finally ships.

*Source: 电动知士大雨 (Weibo), synthesising Tesla's refreshed AI accelerator roadmap; technical detail from EET-China's analysis of the same deck.*

## Enjoyed this? Get the next one direct.

New articles, or a daily / weekly digest — delivered to your inbox.
