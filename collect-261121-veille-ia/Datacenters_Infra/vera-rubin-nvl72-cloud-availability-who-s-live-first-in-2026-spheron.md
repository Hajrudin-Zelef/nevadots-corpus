---
id: collect-261121-veille-ia/datacenters-infra/vera-rubin-nvl72-cloud-availability-who-s-live-first-in-2026-spheron
title: "Vera Rubin NVL72 Cloud Availability: Who's Live First in 2026 | Spheron"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: ["2026-10-27"]
keywords: ["rubin", "vera", "nvl72", "cloud", "live", "availability", "customer", "first"]
source: https://www.spheron.network/blog/vera-rubin-nvl72-cloud-availability-who-s-live-first-in-2026/
source_anchor: ""
source_lines: [1, 127]
section: "Datacenters_Infra"
---
Vera Rubin NVL72 cloud availability finally has real dates behind it, not just roadmap language. As of this week, three providers have NVIDIA's new system doing actual work: Oracle flipped the general-availability switch on October 27, CoreWeave has had a named customer in production since September 30, and Google Cloud put its first live customer on Rubin-powered A5X instances on October 21. Everyone else, AWS, Azure, Lambda, Nebius, and Nscale, is still somewhere between "announced" and "H2 2026 target." Here's the scoreboard, dated and sourced, instead of another spec sheet.

## TL;DR: Vera Rubin NVL72 Cloud Availability Right Now

Three providers have Vera Rubin NVL72 running real workloads as of late October 2026: Oracle (general availability), CoreWeave (production for one named customer), and Google Cloud (one live customer, broader GA in November). Everyone else, including Spheron, is committed but not yet shipping.

| Provider | Status as of Oct 2026 | Date | Detail | 
|---|---|---|---|
| Oracle (OCI) | **Live, GA** | Oct 27, 2026 | Vera Rubin NVL72 and Vera CPUs generally available; OpenAI trains GPT-6 Astra on Oracle's Abilene supercluster | 
| CoreWeave | **Live, production** | Sep 30, 2026 | Cognition is the first customer anywhere running production workloads on the system | 
| Google Cloud | **Live, limited** | Oct 21, 2026 | First customer (Ineffable Intelligence) on A5X; full GA planned November 2026 | 
| AWS, Azure, Lambda, Nebius, Nscale | Committed, not yet shipped | 2026-2028 | AWS/NVIDIA committed 2M GPUs for 2027-2028; Lambda and Nebius target H2 2026; Azure and Nscale have no published date | 
| Spheron | Pre-order only | Not published | First cloud cohort marked "Coming H2 2026," broader availability targeted 2027 | 

Check current GPU pricing for what's actually bookable today, since none of the Rubin rows above carry a published hourly rate yet.

## NVIDIA Vera Rubin NVL72: Live, Validated, or Still a Preorder

Vera Rubin NVL72 packs 72 Rubin GPUs and 36 Vera CPUs per rack, connected by NVLink 6, and it's the successor architecture to Blackwell Ultra (B300). For the full spec breakdown, see the Vera Rubin NVL72 architecture guide; this post is about where you can rent one, not what's inside the rack.

The distinction that matters for a buyer searching "nvidia vera rubin nvl72" right now is the gap between three claims a provider can make:

| Stage | What it actually means | Who's there (Oct 2026) | 
|---|---|---|
| Announced | A provider has said it will offer Rubin, with no working hardware shown yet | AWS, Azure, Lambda, Nebius, Nscale, Spheron | 
| Validated | A rack has been powered on and run compute jobs, but no customer can book one yet | CoreWeave's own earlier bring-up, completed June 1, 2026 | 
| Live or GA | A paying customer is running a real workload, or general sign-up is open | Oracle, CoreWeave, Google Cloud | 

Independent benchmarks are starting to back up the vendor claims too: MLPerf Inference v6.1 published the first audited Vera Rubin results, and vLLM already has day-0 Vera Rubin NVL72 support, so the serving-stack side of "can I run my workload on this" is no longer the bottleneck. Access is.

## Rubin GPU, HGX Rubin NVL8, and Vera Rubin NVL72: Three Products, Three Timelines

"Rubin availability" searches lump together three different products shipping on three different schedules, and mixing them up is the fastest way to misread a press release. NVIDIA's own Rubin platform announcement describes the standalone Rubin GPU as rated at 50 petaflops of NVFP4 inference compute, with Rubin-based products from partners targeted for the second half of 2026. That's the chip on its own, sold into whatever server a partner builds around it.

Vera Rubin NVL72, the subject of this post, is the fully integrated 72-GPU, 36-CPU rack, and it's the product Oracle, CoreWeave, and Google Cloud have actually turned on for customers. Sitting between the two is a third product that gets less attention: Oracle's GA announcement also commits to introducing HGX Rubin NVL8, an eight-GPU, x86-based NVLink server, not a 72-GPU rack. It's sized for teams that want Rubin silicon without committing to a full rack's power and cooling footprint, and as of this post it's still "planned," with no GA date attached. If a headline says "Rubin is available" without naming which of these three it means, assume it means the rack, since that's the only one with a live customer anywhere.

## Oracle's October 27 GA: Vera Rubin NVL72 at Multi-Gigawatt Scale on OCI

Oracle and NVIDIA announced on October 27, 2026 that Vera Rubin NVL72 and Vera CPUs are generally available on Oracle Cloud Infrastructure, with plans to introduce HGX Rubin NVL8 systems on top. That's the first unambiguous "you can have this today" announcement in the Rubin generation; everyone else in this post is either running a pilot for one customer or pointing at a future quarter.

The GA news came paired with a separate commitment: NVIDIA and Oracle also said they'll expand OCI Superclusters built on Vera Rubin NVL72 to multi-gigawatt scale, which is capacity planning, not a shipped product. OCI already runs AI infrastructure at real scale: in Abilene, Texas, OpenAI has trained and served GPT-6 Astra on an OCI supercluster with more than 100,000 NVIDIA Blackwell GPUs, the install base Rubin GA is extending rather than starting from scratch.

Oracle's Mahesh Thiagarajan tied the release to what customers actually get from the partnership: "Oracle Cloud Infrastructure gives customers the performance, security and scale to advance frontier AI. Our collaboration with NVIDIA extends those capabilities across training, inference and agentic AI."

## MLPerf Inference v6.1: The First Audited Numbers Behind the Rollout

Everything in the section above comes from the vendors themselves. MLPerf Inference v6.1 is the first round to put third-party numbers behind the Vera Rubin NVL72 rollout, and the headline result is specific: in its first submission, Vera Rubin NVL72 delivered up to 3.7x higher throughput than GB300 NVL72, the Blackwell Ultra rack it replaces. That's a raw throughput comparison, measured per rack, which is a different claim from the power-normalized "tokens per megawatt" figures CoreWeave published separately below, even though both describe the same hardware generation.

The same MLPerf round answered a question that matters more to a buyer sizing a multi-rack deployment than the headline multiplier: does the gain hold as you add racks? A 288-GPU submission spanning four GB300 NVL72 racks held 99% scaling efficiency against a single-rack baseline, with throughput growing nearly linearly as racks were added. NVIDIA also credits part of the generational jump to software rather than silicon: v6.1's submission stack alone added up to 1.6x higher performance over v6.0 on identical hardware, with further gains still coming from post-submission optimization. Worth remembering before comparing any two vendors' throughput claims: the software stack underneath the benchmark moved the number by more than half, independent of any GPU.

## CoreWeave and Google Cloud: Production Workloads Already Running

CoreWeave got there first on the "real customer, real workload" bar, just not with a formal GA label. CoreWeave's Chen Goldberg credited the speed of that rollout to infrastructure CoreWeave had already built: "With customers like Cognition, that investment shows up in the ability to get production workloads running within days."

A 4.8x throughput jump from one customer isn't a benchmark you can generalize to your own workload, but it's a different kind of evidence than a vendor slide: a named company saying a specific number moved for a specific reason. Google Cloud moved on a similar timeline. Google Cloud announced its first live NVIDIA Vera Rubin-powered A5X instances. Google isn't calling this full GA yet; that's planned for November 2026, alongside expanded European availability. The efficiency claims attached to A5X are aggressive: up to 10x lower inference cost per token and up to 10x higher token throughput per megawatt against prior-generation instances, with plans to scale to as many as 960,000 Rubin GPUs across multiple sites. Those are Google's own projected figures for the platform at scale, not a measured result from Ineffable Intelligence's deployment, so treat the 10x numbers as a ceiling case, not what any workload sees on day one.

The pattern repeats at both: one flagship customer first, broader access later, weeks to months between "live" and "generally available."

## CoreWeave's Measured Silicon Numbers: 10x Tokens per Megawatt on DeepSeek-R1

Cognition's 4.8x and 3.8x figures above are customer-reported, on Cognition's own workloads. CoreWeave separately published what it calls the industry's first measured silicon performance numbers on the platform, showing 10 times the token throughput per megawatt over NVIDIA GB200 NVL72 on the DeepSeek R1 reasoning model at matched interactivity. NVIDIA's own write-up of the same deployment corroborates the direction: NVIDIA's benchmark of CoreWeave's rollout found a 10x improvement in tokens per second per megawatt on DeepSeek-R1 compared with Grace Blackwell NVL72.

Two details matter before you treat that 10x as comparable to the MLPerf 3.7x above. First, the baseline is GB200 NVL72, one generation further back than the GB300 NVL72 used in MLPerf, so the two numbers are measuring against different starting points. Second, tokens per megawatt is a power-normalized metric that matters most at the scale of a data center footprint, not necessarily at the single-rack scale most teams are pricing against when deciding whether to book one rack this quarter.

## Worked Example: Turning a 4.8x Throughput Claim Into a Capacity Decision

Here's how to actually use Cognition's number instead of just repeating it. Say a team running a SWE-2-style agentic inference workload currently needs ten GB300 NVL72 racks to hit its target token throughput. If Vera Rubin NVL72 reproduces Cognition's measured 4.8x increase in total token throughput on that same workload class, the same throughput could in theory come from a little over two Vera Rubin racks instead of ten, before accounting for the separate 3.8x boost in output token throughput Cognition reported on reinforcement learning workloads, which is a different number for a different job.

That math only holds if your workload looks like Cognition's. It's one company's result on one workload, not a guarantee, and CoreWeave hasn't published an industry-wide multiplier to replace it. But it's the right shape of calculation to run against your own token volume and rack count before deciding whether to wait for Rubin capacity or commit to GB300 now, a migration question this post comes back to further down.

## Google Cloud's Scale Plan: 80,000 GPUs in One Site Before It Goes Multi-Site

Google Cloud's Rubin deployment plan, published alongside the October 21, 2026 GTC Berlin announcement, describes two separate scale regimes rather than one number. A single site can grow to as many as 80,000 Rubin GPUs, and only past that ceiling does Google start talking about linking sites together for the multi-site total mentioned earlier in this post. Eighty thousand GPUs in a single location is larger than most providers' entire current fleet, which is the real signal in the announcement: Google isn't planning to grow A5X capacity by adding many small sites, it's planning to fill individual sites to a size most of the market has never operated at. General availability is still dated to the month after the Berlin announcement, so as of this post that single-site ceiling is a plan, not inventory a customer can book today.

## Azure, AWS, Lambda, Nebius, and Nscale: Where Each Stands

This is where the "H2 2026" language in most vendor announcements needs a reality check: H2 2026 ends on December 31, and several of these commitments still have no shipped instance behind them.

- **AWS** : committed in scale, not in a ship date. AWS and NVIDIA announced a commitment to deploy 2 million additional GPUs. That's a multi-year window, not a 2026 date.
- **Azure** : no published Vera Rubin NVL72 general-availability date as of this post. Nothing to book yet.
- **Lambda** : says Vera Rubin will serve as the core building block for Lambda Superclusters.
- **Nebius** : committed to offer Vera Rubin NVL72 in the US and Europe starting H2 2026. Same status as Lambda: real, not yet shipped.
- **Nscale** : has not published a Vera Rubin NVL72 general-availability date as of this post.

Budget for early 2027 if you're planning around any of these five, unless a harder date lands before year-end. NVIDIA's own roadmap dates have already slipped once this generation, and the gap between "validated" and "broadly available" tends to run longer than the press release implies.

## Why Capacity Is Landing a Few Customers at a Time: Inside the Supply Chain

The gap between a GA press release and a bookable instance for everyone comes down to how many hands build one rack. NVIDIA says Vera Rubin's rollout is backed by 300 global partners across more than 350 factory sites in 30 countries, which it calls the largest, most mature rack-scale supply chain assembled for a single generation. A 72-GPU, 36-CPU rack isn't a part one vendor ships and a provider bolts together; it's GPUs, CPUs, NVLink switches, cooling, and power delivery from dozens of specialized suppliers, all of which have to clear qualification before a provider can put a live instance behind a sign-up form.

That's why Oracle, CoreWeave, and Google Cloud reaching live status within about four weeks of each other in late September and October 2026 reads less like a coincidence and more like a supply chain that only recently got deep enough to support more than one customer at a time. It's not evidence that capacity is suddenly abundant everywhere, and it's the reason the other five providers in the list above are still stuck between "announced" and a real date.

## Cost Per Million Tokens: The Economics Behind the Access Race

The reason three providers raced to go live within weeks of each other shows up in the cost claims, not just the throughput ones. NVIDIA frames the generational jump less in FLOPS and more in what it costs to serve a token: compared with GB200 NVL72, Vera Rubin NVL72 is positioned to deliver 10x more tokens per megawatt and one-tenth the cost per million tokens. At the platform level, NVIDIA's own Rubin announcement claims up to a 10x reduction in inference token cost and a 4x reduction in the number of GPUs needed to train MoE models, compared with Blackwell.

Scale is what turns those percentages into a real number. DeepInfra, one of the deployment partners NVIDIA cites, processes nearly 5 trillion tokens a week, with roughly 30% of that volume coming from agentic systems rather than single-turn chat. A 10x cost-per-token improvement at that volume is the difference between a line item and a board-level budget conversation, which is the actual pressure behind Oracle, CoreWeave, and Google Cloud all shipping before year-end rather than waiting for a cleaner GA rollout. Unlike the MLPerf numbers earlier in this post, these cost figures are vendor comparisons, not independently audited, so treat them as the ceiling case rather than what any specific deployment will see.

## Which Cloud to Book Right Now: A Decision Guide by Workload

The four providers above aren't interchangeable once you need to actually sign a contract this quarter:

- **Need a published GA date, not a flagship customer story:** Oracle is the only provider with a formal general-availability date, October 27, 2026, rather than a single named customer running early access.
- **Already running inference-heavy agentic workloads like Cognition's SWE-2:** CoreWeave has the clearest published case study, though the 4.8x and 3.8x multipliers are one customer's numbers, not a guarantee for yours.
- **Need the largest eventual footprint and can wait past November:** Google Cloud's A5X, especially given the 80,000-GPU single-site plan above, once broader GA lands.
- **Planning past 2026 rather than booking this quarter:** none of the above is the right call yet, which is where the reservation route below comes in.

## Why Vera Rubin Is a Reservation Product on Spheron, Not an On-Demand SKU

Spheron's Vera Rubin reservation page is honest about where it sits on this scoreboard: pre-order only. That puts Spheron in the same bucket as AWS, Azure, Lambda, Nebius, and Nscale above: committed to the generation, not yet shipping it.

That's not a reason to skip the signup if you're planning ahead. Spheron's own pattern on prior GPU generations has specialist marketplace pricing coming in below the hyperscaler premium once a new architecture goes from reservation to live inventory, with no long-term contract required once capacity does go live, consistent with Spheron's existing per-minute billing on H100, H200, B200, and B300. If you need Rubin this quarter, Oracle, CoreWeave, or Google Cloud are the only places to get it. If you're planning for 2027, the pre-order pipeline is where pricing and availability outreach happens once Spheron's cohort opens, not a substitute for renting elsewhere today.

For teams deciding whether to wait on Rubin at all or deploy on what's already live, the GB300 to Vera Rubin comparison walks through what changes in software, drivers, and availability between the two generations, a more useful question than the rack spec sheet for a 2026 buying decision.

Rubin capacity is still landing at a handful of providers at a time; if you're planning a 2027 deployment rather than chasing a live rack this quarter, the Vera Rubin pre-order pipeline is where to get in line.


## Frequently *Asked Questions*

Partially. Oracle Cloud Infrastructure made Vera Rubin NVL72 and Vera CPUs generally available on October 27, 2026. CoreWeave has Vera Rubin NVL72 running production workloads for a named customer, Cognition, as of September 30, 2026. Google Cloud has one live customer on A5X instances since October 21, 2026, with broader general availability planned for November 2026. AWS, Azure, Lambda, Nebius, and Nscale have announced Rubin commitments but have not shipped a live instance yet.

Oracle (GA, October 27, 2026), CoreWeave (production workloads live since September 30, 2026), and Google Cloud (first customer live October 21, 2026, GA planned November 2026) are the three providers with Vera Rubin NVL72 running real workloads today. Lambda and Nebius have both committed to H2 2026 availability but have not confirmed a live instance as of this post. AWS and Azure have announced multi-year Rubin infrastructure commitments without a 2026 availability date.

AWS and NVIDIA announced on August 26, 2026 a commitment to deploy 2 million additional GPUs across Blackwell Ultra, Rubin, and Rubin Ultra through 2027-2028, which covers Rubin capacity but names no specific 2026 ship date. Azure has not published a Vera Rubin NVL72 general-availability date as of this post. Treat both as 2027-and-beyond for broad access rather than a 2026 bet.

Not yet. Spheron's Vera Rubin page is pre-order only: the first cloud cohort is marked 'Coming H2 2026,' with broader Spheron availability targeted for 2027 as supply scales, and no published hourly price. Readers who need Rubin capacity this quarter should go to Oracle, CoreWeave, or Google Cloud instead; readers planning ahead can join the Spheron pre-order pipeline now.
