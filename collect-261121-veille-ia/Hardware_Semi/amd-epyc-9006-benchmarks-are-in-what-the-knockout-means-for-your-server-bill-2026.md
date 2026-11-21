---
id: collect-261121-veille-ia/hardware-semi/amd-epyc-9006-benchmarks-are-in-what-the-knockout-means-for-your-server-bill-2026
title: "AMD EPYC 9006 Benchmarks Are In: What the 'Knockout' Means for Your Server Bill (2026)"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["epyc", "core", "benchmarks", "server", "bill", "venice", "phoronix", "knockout"]
source: https://www.beyondimy.com/post/amd-epyc-9006-server-results
source_anchor: ""
source_lines: [1, 89]
section: "Hardware_Semi"
---
# AMD EPYC 9006 Benchmarks Are In: What the 'Knockout' Means for Your Server Bill (2026)

AMD EPYC 9006 'Venice' benchmarks land. What the 256-core Zen 6 results, real pricing, and the DRAM crunch mean for your server bill (2026).

By Bongyeon Kim · Researched from primary sources, verified by hand, and produced with AI assistance on a production stack this site actually operates (Vercel · Neon · Cloudflare R2).

AMD EPYC 9006 "Venice" reviews went live on November 10, 2026, and the headline is loud. ServeTheHome titled its launch review "AMD EPYC 9006 Results Are Here and AMD Delivers a Knockout Part Deux" — a callback to the first-generation EPYC's upset of Intel — and the Phoronix verdict was blunter: "a grand slam alright -- out of this world." That framing is accurate for hyperscalers running 256-core flagships. It is mostly noise for the person running a 2U box for a dev team. This article walks through what actually shipped, what the numbers say, and where the money in a server bill actually goes now.

## TL;DR

- **What shipped:** 6th-gen EPYC 9006 "Venice" — Zen 6 / Zen 6c on TSMC 2nm, up to 256 cores / 512 threads per socket, 16 channels of DDR5-8000 or MRDIMM-12800, PCIe 6.0, on the new SP7 socket. SP8 parts follow in H1 2027 (per Phoronix's launch coverage).
- **Independent results back the hype:** Phoronix measured the 2P flagship (EPYC 9996) at**1.58x the geometric mean of its own predecessor (EPYC 9965 2P)** and roughly**2x Intel's Xeon 6980P in HPC composites** , while still leading performance-per-watt despite a 600W rating.
- **ServeTheHome's own tests:** fastest single-core agentic score (849 vs Vera's 727 and Xeon 6980P's 448), best SMT uplift (34.3% on one core), and the best multi-agent split — 16 agents of 16 cores each got**6.73x the work** of one 256-core agent.
- **Pricing is public:** 31 SKUs from**$700 (8 cores, EPYC 9016)** to**$14,904 (256-core EPYC 9996)** at 1KU. The flagship is ~**$58 per core** ; the 64-core SP7 entry is ~**$125 per core** . Per-core economics favor big cores-per-chip — which is a hyperscaler's game, not a small team's.
- **The real budget problem is not the CPU.** STH reports DRAM prices "through the roof," to the point that "the cost of even a high-end EPYC CPU is now a drop in the bucket compared to the cost of the RAM needed to fill it out."
- **For small teams, the takeaway is boring:** wait for SP8 mid-range SKUs, budget RAM before CPU, and don't size infra by flagship benchmarks.

## What the 9006 series actually is

EPYC 9006 is the successor to EPYC 9005 "Turin" (Zen 5, max 192 cores), following AMD's two-year cadence. Per Phoronix's launch review, AMD claims roughly 20% higher IPC for Zen 6, and the platform jumps from Turin's 12 DDR5 channels to 16 channels of DDR5-8000 or second-gen MRDIMM at 12,800 MT/s, plus PCIe 6.0 and CXL 3.1. Two notes that matter for planning:

1. **New socket.** Venice is on Socket SP7 (high-end, 16-channel) with SP8 coming in H1 2027. Phoronix confirms SP5 systems do not accept Venice — there is no drop-in upgrade path from a Turin board.
2. **Full line, not just flagships.** StorageReview published the complete SKU list on September 26, 2026: 31 parts across SP7 and SP8, from an 8-core $700 EPYC 9016 up to the 256-core EPYC 9996. SP7 systems started shipping November 10; the SP8-heavy part of the range (and the LPDDR5X "Verano" parts) arrive later.

AMD also talks up Venice for "agentic AI" workloads — the orchestration, tool-call, and parsing layers around LLM inference. That's the workload AMD is using to sell 256-core boxes to cloud buyers, and it's the reason this launch got front-page treatment.

## The benchmark numbers, with sources

The launch-day coverage splits into three tiers of evidence, and it's worth keeping them straight.

**Independent, measured (Phoronix, Nov 10):** the EPYC 9996 2P tested across hundreds of Linux benchmarks landed at **1.58x the geometric mean of the EPYC 9965 2P** — against a 1.33x core/thread count advantage, so the architecture and memory subsystem did real work beyond just adding cores. In the HPC composite, the 9996 2P was "around 2x the performance of Intel's Xeon 6980P current flagship." Even at 600W per socket, Phoronix found the 9996 2P "still delivering leading performance-per-Watt."

**Independent, measured (STH, Nov 10):** STH ran its AgentSTH V7 agentic-workload suite on a single-socket Dell PowerEdge R9815. Results, per STH's review:

```
AgentSTH V7 single-core score (STH, Nov 10, 2026)
  AMD EPYC 9996   : 849
  NVIDIA Vera     : 727
  AMD EPYC 9965   : 657
  NVIDIA Grace    : 572
  Intel Xeon 6980P: 448
```
STH also measured SMT uplift at **34.3%** on one 9996 core (Vera 37.2%, Xeon 6980P 31.7%), and memory latency of about **143 ns** at the largest working sets — slightly behind its predecessor's 127 ns and Vera's 134 ns, but far ahead of Xeon 6980P's 196 ns and Grace's 247 ns. The honest caveat from STH: their 2P MRDIMM test system shipped late, so power figures are "pretty good directional numbers" pending a follow-up.

**Vendor estimates (AMD whitepaper, treat as marketing):** AMD's own paper claims the 9996 2P delivers 2.37x the Xeon 6980P and 2.24x NVIDIA Vera on SPECrate 2026 integer — but AMD's footnote explicitly says these are **estimates, not publishable SPEC results**, and the Vera comparison mixes vendor-controlled test environments. Read those slides as positioning, not proof.

## Spec and price table (from the published SKU list)

Figures below are from StorageReview's September 26, 2026 SKU-list report, cross-checked against AMD product pages. Prices are 1KU (per-1,000-unit) OEM pricing — what you actually pay through a vendor is higher.

| Model | Platform | Cores/Threads | Base/Boost | L3 | Default Power | 1KU Price | ~$ per core | 
|---|---|---|---|---|---|---|---|
| EPYC 9996 | SP7 | 256/512 | 2.55/4.1 GHz | 1024 MB | 600W (400–600) | $14,904 | $58 | 
| EPYC 9966 | SP7 | 192/384 | 2.9/4.0 GHz | 768 MB | 600W (400–600) | $14,079 | $73 | 
| EPYC 9756 | SP7 | 128/256 | 3.15/4.0 GHz | 512 MB | 500W (320–500) | $12,498 | $98 | 
| EPYC 9586F | SP7 | 64/128 | 3.75/5.0 GHz | 384 MB | 500W (400–600) | $9,701 | $152 | 
| EPYC 9556 | SP7 | 64/128 | 2.75/4.3 GHz | 384 MB | 300W (220–300) | $8,008 | $125 | 
| EPYC 9746 | SP8 | 128/256 | 2.9/4.0 GHz | 512 MB | 400W (200–400) | $11,679 | $91 | 
| EPYC 9016 | SP8 | 8/16 | 3.05/4.8 GHz | 48 MB | 130W (130–220) | $700 | $88 | 

For reference, AMD's July comparison priced Intel's 128-core Xeon 6980P at $13,955 (per the same StorageReview report). The 9996 at $14,904 costs $91 more than its own Turin predecessor — but delivers a third more cores, so per-core cost actually fell.

## The per-core math nobody at a small company should play

Here's the trap in that table. Per-core price falls dramatically at the top — $58/core at 256 cores versus $125/core at 64 cores — and the launch narrative leans on exactly that ratio. But that math only pays off if you can saturate 256 cores. A dev team running Postgres, a CI runner, a few containers, and a staging environment will not. What they need is a handful of fast cores, sane memory, and low idle power — which is roughly the opposite of what a benchmark-winning flagship optimizes for.

The genuinely useful numbers for a small buyer are the boring ones: the EPYC 9016 (8 cores, 130W, $700) and the mid-range SP8 SKUs coming in 2027. Those will show up in cheap single-socket boxes, and the previous two generations (9004, 9005) are already flooding the used market at steep discounts — a Genoa/Bergamo box with 32–64 cores costs a fraction of anything in the 9006 list and will outlast the amortization period of most small infra. Phoronix's own review closes on this point from the other side: even at the flagship tier, MRDIMM-12800 pricing wasn't visible at launch, and with even DDR5 RDIMM-6400 prices elevated, filling a 16-channel Venice board is "a very sizable investment."

## The DRAM line item is the real story

This is the part that should change your budget spreadsheet. STH's launch report notes the AI boom has pushed DRAM demand through the roof, to the point that the CPU is the cheap part of the build. AMD's answer is MEXT, a software memory-expansion layer that pages cold data to SSDs; AMD projects many workloads could hit "90% of their current performance with half as much DRAM" (per STH's coverage of the AMD launch deck). That's a hyperscaler-grade argument, but the underlying arithmetic applies at any scale: in 2026, your server bill is increasingly a RAM bill.

Practical consequences for a small team:

- **Size RAM before cores.** A 64-core machine starved of memory is slower than a 32-core machine with comfortable headroom — and costs less to run.
- **Second-hand Turin and Milan are the value play.** The 9006 launch doesn't obsolete them; it just discounts them. STH has repeatedly shown prior-gen EPYC holding strong perf-per-dollar.
- **Don't build on a socket you can't refill.** SP7/SP8 boards and ECC RDIMM/MRDIMM supply are early-cycle; used SP5 ecosystems are mature and cheap.

## What hyperscalers buy vs what you need

The gap between the headlines and your needs is a difference in objective function. Hyperscalers buy **throughput per rack** — that's why Meta is AMD's named lead customer validating SP7 platforms, and why per-core pricing collapses at 256 cores. You buy **latency per user and dollars per month**. A 256-core 600W socket idling at 3% utilization for a staging box is the wrong purchase at any discount.

So when you read "EPYC 9996 delivers 1.58x its predecessor" (Phoronix, measured), the correct small-team translation is: *the platform beneath the flagship is healthy, Zen 6 is real, and in 12–18 months the mid-range SP8 SKUs and second-hand Turin parts will inherit these gains at your price point.* The benchmark winners of November 2026 become your cheap options of late 2027.

## Wrap-up

- **The "knockout" is real but aimed at the wrong buyer.** Measured results (Phoronix: 1.58x gen-over-gen, ~2x Xeon 6980P in HPC; STH: fastest single-core agentic score at 849) confirm AMD leads the x86 server race — that race is run with 256-core, $14,904 sockets that small teams will never touch.
- **Price-per-core improves only at scales you don't operate.** $58/core at the flagship versus $88–$125/core in the affordable SKUs means the value story requires saturation; for 2–20 person infra, used 9004/9005 EPYC remains the best dollars-per-usable-core.
- **Budget the memory first.** With DRAM prices inflated by AI demand — and AMD's own MEXT pitch premised on halving DRAM — the server bill in 2026 is decided by RAM lines and power, not the CPU SKU on the invoice.
