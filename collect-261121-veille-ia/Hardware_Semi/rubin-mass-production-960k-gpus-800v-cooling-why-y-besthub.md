---
id: collect-261121-veille-ia/hardware-semi/rubin-mass-production-960k-gpus-800v-cooling-why-y-besthub
title: "Rubin Mass Production: 960K GPUs, 800V Cooling — Why Y… | BestHub"
domain: hardware-semi
role: reference
task: article
actors: []
dates: ["2026-10-29"]
keywords: ["rubin", "bandwidth", "full", "rack", "gpus", "cooling", "single", "factory"]
source: https://www.besthub.dev/articles/rubin-mass-production-960k-gpus-800v-cooling-why-your-mac-isn-t-obsolete-d3921b785fff
source_anchor: ""
source_lines: [1, 85]
section: "Hardware_Semi"
---
On October 21 at GTC Berlin, NVIDIA and Google Cloud announced staggering numbers: Google Cloud A5X instances based on Vera Rubin NVL72 can scale to 80,000 Rubin GPUs per site, up to 960,000 across sites; Rubin cabinets use 800V DC power + 100% liquid cooling. What does this have to do with you running local LLMs on a Mac? This article uses a table to bring the massive data center and your desktop Mac together, clarifying what ordinary people should worry about and what they should do.

## First, understand: Rubin is not a "graphics card", it's an entire supply chain

Many hear "NVIDIA new card" and think it's just another GPU. Rubin is a full-stack platform from single chip to full cabinet to "AI factory". Three levels:

Rubin GPU (single chip) : 288GB HBM4, ~22 TB/s memory bandwidth, 50 PFLOPS NVFP4 inference (sparse) — ~5x over Blackwell.

NVL72 full rack : 36 Vera CPUs + 72 Rubin GPUs connected via 6th-gen NVLink into one "giant chip", 20.7 TB total HBM4, 1400 TB/s aggregate bandwidth, 3600 PFLOPS NVFP4 inference, 190–230 kW power.

100MW AI factory : ~40,000 Rubin GPUs, 2 ZFLOPS NVFP4 inference, 12 PB HBM4, 800 PB/s bandwidth — what Huang calls "AI factory".

"Full production" is real: Huang announced Vera Rubin NVL72 full production at CES 2026, NVIDIA May 31 announced "ramp into full production", July 21 announced volume production running in customer data centers. Supply chain analyst Ming-Chi Kuo estimates 250–300k Rubin GPUs shipped in H2 2026 (~3500 full systems), industry-wide volume delivery in Q1 2027.

## 960K, 800V liquid cooling — what does it mean?

960K is scale: Google Cloud A5X via Virgo Network, single site 80K, multi-site 960K — industrial AI capacity, not buying a few cards.

800V liquid cooling is two linked hard changes:

100% liquid cooling : Rubin is the world's first fully fanless AI compute platform, no cables/fans/hoses in cabinet, coolant inlet up to 45°C, can use dry coolers without chillers, saving millions of gallons water per MW per year.

800V DC power : traditional data centers use 48V rack distribution + 480V AC into building; Rubin goes straight to 800V DC, same copper carries 150% more power, saves ~200kg copper busbars per rack, TCO down up to 30%. Not just a PSU swap, entire facility redesign.

Key metric: vs Grace Blackwell, Rubin delivers ~10x tokens per MW, inference cost per token ~1/10. That's the real economics of "AI factory".

## Core comparison: Rubin full stack vs your Mac

Put the massive data center and your desktop device in one table, the gap is obvious.

Rubin full stack vs your Mac: every metric is orders of magnitude apart (data as of 2026-10-29, sources at end)

Bandwidth : M5 Ultra 1.2 TB/s, single Rubin GPU 22 TB/s (~18x), full rack 1400 TB/s (~1166x), 100MW factory 800 PB/s (~660,000x).

Memory : your 256GB unified memory vs single Rubin 288GB HBM4, rack 20.7 TB, factory 12 PB — different world.

Power : Mac ~120W, single Rubin ~2.3 kW, rack 200 kW, factory 100 MW — your Mac's yearly electricity wouldn't run a Rubin rack for minutes.

Bandwidth gap on log scale makes it even clearer — your Mac bar is nearly invisible:

Memory bandwidth log comparison: your Mac bar is nearly invisible against Rubin

## Bandwidth is the real gap: why "your Mac can't run large models" is a misconception

Here's a fair defense of local deployment. Many see the table and panic: "Is my Mac useless?" No. Two points:

First, your M5 Ultra (256GB unified, 1.2 TB/s) runs 70B quantized, 120B at 20–26 tok/s, even fits DeepSeek-V4-Flash 284B — "more than enough productivity for individuals/small teams". An RTX 5090 (32GB, 1.79 TB/s) also runs 70B quantized happily.

Second, "fits" and "runs fast" are different . Local bandwidth is fixed by physical packaging; model size limit set by memory, speed by bandwidth. Rubin single GPU 18x your bandwidth, rack 1000x+ — so local suits "small/medium model high-privacy inference, fine-tuning, learning", cloud Rubin suits "frontier training + massive concurrent inference". They're complementary, not substitutes.

⚠️ Warning: Rubin rack and 100MW factory are "presumed denied" for China (performance >20x licensed products), Chinese enterprises basically can't get compliant supply; individuals/small teams can't use or afford this scale anyway. Don't let "960K" scare you into anxiety about your Mac.

## What's it to you: three paths, pick yours

Don't fret over compute, first clarify "what should I do":

Path A | Students / general users : Your Mac or PC running 7B–70B quantized locally is plenty. Learn AI, code, side projects, personal knowledge base — focus on "using it", not 960K GPUs.

Path B | Developers / small teams : Local Mac Studio M5 Ultra (256GB, 1.2 TB/s, ¥46,999+) or RTX 5090 for medium models; if you need frontier models (hundreds of B, long context, high concurrency), rent cloud — but Rubin-level cloud not widely available until 2027, currently mostly Blackwell.

Path C | Enterprises / researchers : Watch Rubin cloud rental pricing, export compliance boundaries, 800V liquid cooling driving data center redesign trends. If you build AI infrastructure, this "power+cooling+network" reconstruction is the real opportunity.

Closing line: Rubin changes the "AI factory" cost curve, not the value of the Mac on your desk. Squeezing every drop out of local models is far more useful than fretting over 960K GPUs.

## Data sources and timeliness

NVIDIA official Vera Rubin NVL72 spec page (nvidia.com)

Google Cloud Blog "Google Cloud infrastructure at NVIDIA GTC Berlin 2026", NVIDIA Blog "NVIDIA and Google Cloud Collaborate to Advance Agentic and Physical AI"

ModulEdge "Nvidia Vera Rubin: Data Center Impact Explained", TechTimes, The Elec (7/21 volume production report), Guolian Minsheng Securities "2026 Vera Rubin Production Supply Chain Opportunity Analysis"

Ming-Chi Kuo supply chain shipment estimates (H2 2026: 250–300k GPUs)

Apple Silicon local deployment data from this account's prior testing (M5 Ultra 256GB / 1.2 TB/s / RTX 5090 32GB / 1.79 TB/s)

Data as of 2026-10-29 . Chip specs per NVIDIA / Apple official; shipment volumes, rental prices are analyst estimates and market references, verify official sites and cloud vendor real-time quotes before ordering.

Signed-in readers can open the original source through BestHub's protected redirect.

This article has been distilled and summarized from source material, then republished for learning and reference. If you believe it infringes your rights, please contactand we will review it promptly.

### 0 Comments

Thoughtful readers leave field notes, pushback, and hard-won operational detail here.
