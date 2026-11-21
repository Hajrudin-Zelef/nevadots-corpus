---
id: collect-261121-veille-ia/nvidia-hardware/vera-rubin-release-date-what-nvidia-announced-vs-what-ships
title: "Vera Rubin release date: what NVIDIA announced vs what ships"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "rubin", "vera", "announced", "production", "nvl72", "shipping", "release"]
source: https://gpuwerk.com/blog/nvidia-vera-rubin-for-buyers/
source_anchor: ""
source_lines: [1, 130]
section: "NVIDIA_Hardware"
---
# NVIDIA Vera Rubin release date: what was announced, what ships in November 2026, and what to rent now

NVIDIA began production shipments of Vera Rubin in August 2026, its CFO said on the 26 August earnings call, and by 7 November 2026 Vera Rubin NVL72 racks were in production use at CoreWeave (first customer announced 30 September) and generally available on Oracle Cloud (27 October). No provider I checked publishes an on-demand hourly price. Rubin CPX, promised for the end of 2026, was left out of GTC 2026 in March and has not reappeared in NVIDIA's releases since, and Rubin Ultra is a second-half-2027 target that analysts say may slip. If you need GPUs in the next six months, you are choosing between H200, B200 and B300 rentals, RTX PRO cards or a DGX Spark, not Rubin.

| Date | Event | Status on 7 Nov 2026 | Source | 
|---|---|---|---|
| 18 Mar 2025 | GTC 2025 roadmap: Vera Rubin NVL144 in H2 2026, Rubin Ultra in H2 2027 | Announced | NVIDIA blog | 
| 9 Sep 2025 | Rubin CPX: 30 PFLOPS NVFP4, 128 GB GDDR7, "end of 2026" | Not shipping; absent since GTC 2026 | NVIDIA newsroom | 
| 5 Jan 2026 | CES: Rubin platform, six chips, "in full production", partner products H2 2026 | Announced | NVIDIA IR | 
| 16 Mar 2026 | GTC 2026: seven chips including Groq 3 LPU, five rack types | Announced | NVIDIA newsroom | 
| 31 May 2026 | GTC Taipei: ramp to full production, shipments "starting this fall" | Announced | NVIDIA IR | 
| Aug 2026 | Production shipments begin | Shipping | Q2 FY27 call | 
| 16 Sep 2026 | First MLPerf results, preview category | Benchmarked | Nebius | 
| 30 Sep 2026 | CoreWeave: Cognition first production customer | In customer use | CoreWeave | 
| 27 Oct 2026 | OCI: NVL72 and Vera CPU "now generally available"; HGX Rubin NVL8 planned | Generally available, no price | NVIDIA newsroom | 

"Announced" means a statement by NVIDIA about a future product; "shipping" and later rows mean hardware in customers' hands, as stated by NVIDIA or the cloud provider. All pages read 7 November 2026.

## What has NVIDIA announced for Vera Rubin?

A rack-scale system, the Vera Rubin NVL72, built from 72 Rubin GPUs and 36 Vera CPUs, plus an eight-GPU HGX board for ordinary servers. The CES release of 5 January 2026 named six chips (Vera CPU, Rubin GPU, NVLink 6 switch, ConnectX-9, BlueField-4, Spectrum-6) and claimed up to 10 times lower cost per token than Blackwell for inference. At GTC on 16 March 2026 NVIDIA added a seventh, the Groq 3 LPU, and described five rack types: GPU, CPU, Groq 3 LPX, storage and Ethernet.

The per-chip figures come from NVIDIA's Rubin platform technical post. One naming change trips people up: the rack called Vera Rubin NVL144 in March 2025 is the same product now sold as NVL72. NVIDIA moved from counting GPU dies to counting GPU packages, as Glenn Lockwood's GTC 2026 recap notes.

| Per GPU | H200 | B300 (Blackwell Ultra) | Rubin (NVL72) | Source | 
|---|---|---|---|---|
| NVFP4 inference | none | 15 PFLOPS dense, 20 sparse | 50 PFLOPS (sparse, "Transformer Engine compute") | NVIDIA technical posts, HGX page | 
| NVFP4 training | none | no separate figure | 35 PFLOPS dense | NVIDIA | 
| Memory | 141 GB HBM3e | 288 GB HBM3e | 288 GB HBM4 | NVIDIA | 
| Memory bandwidth | 4.8 TB/s | 8 TB/s | 22 TB/s | NVIDIA | 
| NVLink per GPU | 0.9 TB/s | 1.8 TB/s | 3.6 TB/s | NVIDIA | 
| CPU partner | x86 or Grace | x86 or Grace | Vera: 88 cores, up to 1.5 TB LPDDR5X | NVIDIA | 
| Status | Shipping since 2024 | Shipping | Shipping since Aug 2026, NVL72 only | NVIDIA, OCI | 

NVIDIA's own comparison is "up to 5x inference, 3.5x training, 2.8x memory bandwidth" against Blackwell. NVIDIA's Rubin post sets Rubin's 50 PFLOPS against 10 for Blackwell, but the HGX page labels the 50 as sparse, and Blackwell Ultra's sparse figure is 20, so the like-for-like FP4 step to a B300 is 2.5x. The HGX page lists the HGX Rubin NVL8 board at 2 TB of HBM4 and 130 TB/s for eight GPUs, which is about 250 GB and 16 TB/s per GPU by my division, below the NVL72 part. NVIDIA does not explain the difference.

## Is Vera Rubin shipping?

Yes, in racks, to large customers. "We commenced production shipments of Vera Rubin earlier this month," CFO Colette Kress said on the call of 26 August 2026 (transcript), and she expected Vera Rubin to be about 20 percent of data-center revenue in the October quarter. The results release named CoreWeave, Google Cloud, Microsoft Azure, Oracle Cloud Infrastructure and Nebius as partners with racks running.

Two cloud announcements turn that into customer use. CoreWeave said on 30 September 2026 that it stood up a Vera Rubin NVL72 cluster with Cognition in early September and called Cognition "the first customer anywhere running production workloads on the system", with 4.8 times the token throughput of a GB200 NVL72 on Cognition's coding-agent inference. On 27 October NVIDIA and Oracle said NVL72 and Vera CPU instances are "now generally available on OCI" and that OCI "plans to introduce" HGX Rubin NVL8 servers, with no date.

Supply is the limit. Kress said she expects supply "to remain a bottleneck, at least through the end of fiscal year '28" (January 2028), and Jensen Huang said NVIDIA has supply for about 70 percent of demand. If I were a small buyer I would assume Rubin capacity is spoken for by contract customers well into 2027.

## What happened to Rubin CPX?

It has not shipped, and NVIDIA has stopped talking about it without formally cancelling it. The September 2025 release promised a single-die GPU with 30 PFLOPS of NVFP4 and 128 GB of GDDR7 for long-context prefill, in a Vera Rubin NVL144 CPX rack with 100 TB of fast memory, "expected to be available at the end of 2026". At GTC 2026 there was "no mention of Rubin CPX at all", and Lockwood concludes it "appears dead by omission". NVIDIA instead launched the Groq 3 LPX rack (256 LPUs, 128 GB of on-chip SRAM per rack) for low-latency decode, "to be available in the second half of this year".

For a buyer the lesson is general. A product announced 15 months before its ship date can be replaced before it exists, so plan around hardware with a shipping date and a price.

## When is Rubin Ultra coming?

NVIDIA's only dated statement is the GTC 2025 one: systems built on Rubin Ultra are "due for the second half of 2027". At GTC 2026 NVIDIA showed the Kyber rack meant for Rubin Ultra; Lockwood counted 2 chassis and 36 sleds against 4 and 72 a year earlier, "half as dense as originally announced".

Rumour, labelled as such: on 6 July 2026 the analyst firm SemiAnalysis claimed the Kyber NVL144 rack would slip from 2027 to 2028 because of PCB midplane yields, as reported by Tom's Hardware and summarised with dates by The Planet Tools. NVIDIA's response, per the latter, was that its "road map is intact". Neither NVIDIA's August earnings release nor its October Oracle announcement gives a Rubin Ultra date.

## How much faster is Rubin, measured?

About twice a B300 per GPU on DeepSeek-R1, in the one public benchmark so far. MLPerf Inference v6.1 (16 September 2026) had Vera Rubin only in the preview category. Nebius submitted 36 Rubin GPUs on DeepSeek-R1 and reported 591,368 tokens/s in Server mode, 16,427 per GPU. Its own 8x B300 system did 63,909, or 7,989 per GPU, so the ratio is 2.06 by my division. NVIDIA's own claim, quoted in the MLCommons round summary, is up to 2.5 times the prior generation on DeepSeek-R1 and 3.7 times on Qwen3-VL.

That is far from the CES headline of 10 times lower cost per token, which compares full racks and software stacks at NVIDIA's chosen operating point. A 2x per-GPU gain sits below both the bandwidth step (22 against 8 TB/s, 2.75x) and the sparse FP4 step (2.5x). Software will move it: NVIDIA's MLPerf page credits software with up to 1.6 times more performance between v6.0 and v6.1 on existing hardware. Treat the 10x as NVIDIA's best case, not a planning number.

## Can you rent a Rubin GPU today?

Not on a public price list. On 7 November 2026, the newest systems on the CoreWeave and Nebius pricing pages were GB300 NVL72 (price on request at both) and HGX B300 ($9.50 per GPU-hour at Nebius, contact sales at CoreWeave), and neither lists Rubin. RunPod's newest is the B300 at $8.99 per GPU-hour. OCI announced general availability without a price.

The useful number is where Rubin breaks even. If Rubin delivers 2.06 times a B300's tokens per GPU on your workload, it is cheaper per token than a B300 at $8.99 below 2.06 x $8.99 = $18.52 per GPU-hour (my arithmetic). Any Rubin rate under about $18.50 beats today's B300 list price on DeepSeek-R1-style serving; above that, it does not. On a dense model that is less bandwidth-bound the ratio, and the break-even, will be lower.

Do not expect older GPUs to get cheap while you wait. Lambda's pricing page lists the H100 at $4.59 per GPU-hour in an 8x instance and $4.89 for one GPU, effective 16 November 2026; this site's H100 comparison recorded $3.99 and $4.29 there on 11 October, so the rate went up about 15 percent in five weeks. Kress described "extreme pricing conditions in memory" that she expects to rise "even higher into next year". Rubin's arrival has not pushed Hopper prices down.

## What should you rent or buy while Rubin ramps?

Size the decision by the next six months, not by the roadmap. I'd make these calls on 7 November 2026:

- **Serving a large model to many users now:** rent B200 or B300 by the hour. On MLPerf v6.1 numbers the two cost about the same per token at RunPod list prices, and the H200 about twice as much; the arithmetic is in the B300 vs B200 vs H200 comparison. Six months of an 8x B200 node at $7.99 per GPU-hour is $7.99 x 8 x 24 x 182 = $279,000, which is the budget to weigh against waiting.
- **Signing a multi-year commitment:** ask for Rubin or a contractual upgrade path, since that is where your contract overlaps Rubin's real availability. A three-year Blackwell term starting now runs into 2029.
- **One team, models up to about 90 GB, local hardware:** an RTX PRO 6000 Blackwell (96 GB) cost from €17,799 at German retailers on 7 November 2026. The RTX PRO 6000 comparison covers what it fits; with memory prices rising, waiting is unlikely to make it cheaper.
- **Development, evaluation, models up to 128 GB:** a DGX Spark. It is a Blackwell GPU with NVFP4, so an NVFP4 checkpoint you prepare on it is in the format Rubin's headline figures are quoted in. The Spark's own price went from $3,999 at launch to $6,950 for 128 GB in October 2026 (price history); renting one costs $0.79 an hour at GPUwerk.

For an 8-GPU server bought or leased outright, GPUwerk quotes 8x B200 and 8x H100 configurations against a signed order; it has no Rubin hardware, and none of the numbers on this page are its own measurements. Memory prices are the other half of the timing question, covered in the 2026 memory shortage article. The deskside Blackwell Ultra option, a DGX Station with 748 GB, is compared with a Spark in DGX Spark vs DGX Station.

## FAQ

## When is NVIDIA Vera Rubin released?

NVIDIA's CFO said on the 26 August 2026 earnings call that production shipments of Vera Rubin began that month. CoreWeave announced its first production customer on 30 September 2026, and NVIDIA and Oracle said on 27 October 2026 that Vera Rubin NVL72 is generally available on OCI. The roadmap first placed it in the second half of 2026 at GTC on 18 March 2025.

## Can I rent a Vera Rubin GPU by the hour?

Not at a published on-demand price as of 7 November 2026. CoreWeave's and Nebius's pricing pages list GB300 NVL72 and HGX B300 as their newest systems, with no Rubin entry. Oracle announced general availability of Vera Rubin NVL72 on OCI on 27 October 2026 without prices. Early capacity is going to large contracted customers such as Cognition at CoreWeave.

## Is Rubin CPX cancelled?

NVIDIA has not said so. It announced Rubin CPX on 9 September 2025 with 30 PFLOPS of NVFP4 and 128 GB of GDDR7, expected at the end of 2026. It was not mentioned at GTC on 16 March 2026, where NVIDIA introduced the Groq 3 LPX rack for low-latency decode instead. Glenn Lockwood's GTC 2026 recap called CPX dead by omission.

## How much faster is Rubin than Blackwell?

NVIDIA quotes 50 PFLOPS of NVFP4 inference per Rubin GPU against 10 for Blackwell, and 22 TB/s of HBM4 bandwidth against 8 TB/s. In MLPerf Inference v6.1 (16 September 2026, preview category), Nebius's 36-GPU Vera Rubin system did 16,427 DeepSeek-R1 tokens/s per GPU in Server mode, about twice its own HGX B300 result per GPU.

## When is Rubin Ultra coming out?

NVIDIA's GTC 2025 announcement placed Rubin Ultra systems in the second half of 2027. In July 2026 SemiAnalysis reported that the Kyber rack for Rubin Ultra would slip to 2028; NVIDIA told Bloomberg its road map is intact. As of 7 November 2026, NVIDIA has published no Rubin Ultra ship date beyond the 2027 target.

## Sources

Every figure above is attributed to its source in the sentence or table where it appears. Pages were read on 7 November 2026 unless a date says otherwise.

- GTC 2025: Announcements and Live Updates, NVIDIA Blog, 2025-03-25
- NVIDIA Unveils Rubin CPX: A New Class of GPU Designed for Massive-Context Inference, NVIDIA Newsroom, 2025-09-09
- NVIDIA Kicks Off the Next Generation of AI With Rubin: Six New Chips, One Incredible AI Supercomputer, NVIDIA Investor Relations, 2026-01-05
- Inside the NVIDIA Rubin Platform: Six New Chips, One AI Supercomputer, NVIDIA Technical Blog, 2026-01-05
- NVIDIA Vera Rubin Opens Agentic AI Frontier, NVIDIA Newsroom, 2026-03-16
- NVIDIA GTC 2026: Live Updates on What's Next in AI, NVIDIA Blog, 2026-03-16
- NVIDIA Vera Rubin Ramps Into Full Production to Power Agentic AI Factories Worldwide, NVIDIA Investor Relations, 2026-05-31
- NVIDIA Announces Financial Results for Second Quarter Fiscal 2027, NVIDIA Newsroom, 2026-08-26
- Nvidia (NVDA) Q2 2027 Earnings Call Transcript, The Motley Fool, 2026-08-31
- CoreWeave Delivers NVIDIA Vera Rubin NVL72 Performance at Production Scale, Starting With Cognition, CoreWeave, 2026-09-30
- NVIDIA and Oracle Expand NVIDIA Vera Rubin to Multi-Gigawatt Scale and Bring NVIDIA Vera CPU to Oracle Cloud Infrastructure, NVIDIA Newsroom, 2026-10-27
- MLPerf Inference v6.1 Results: NVIDIA Vera Rubin NVL72, Nebius, 2026-09-16
- Where the Industry Is Investing: A Look at MLPerf Inference v6.1, MLCommons, 2026-09-17
- GTC 2026 recap, Glenn K. Lockwood, 2026-03
- Nvidia's Kyber rack for Rubin Ultra reportedly delayed to 2028, Tom's Hardware
- NVIDIA Kyber NVL144 Delayed to 2028? Report vs Denial, The Planet Tools, 2026-07-27
- NVIDIA HGX Platform, NVIDIA
- NVIDIA: MLPerf AI Benchmarks, NVIDIA
- CoreWeave pricing, CoreWeave
- Nebius AI Cloud prices, Nebius
- RunPod pricing, RunPod
- Lambda pricing, Lambda
- NVIDIA RTX PRO 6000 Blackwell Workstation Edition price comparison, hardwaredealz.com, 2026-11-07
