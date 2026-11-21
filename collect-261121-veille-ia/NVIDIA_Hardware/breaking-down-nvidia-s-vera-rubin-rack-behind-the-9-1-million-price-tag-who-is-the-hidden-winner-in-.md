---
id: collect-261121-veille-ia/nvidia-hardware/breaking-down-nvidia-s-vera-rubin-rack-behind-the-9-1-million-price-tag-who-is-the-hidden-winner-in-
title: "Breaking Down NVIDIA's Vera Rubin Rack: Behind the $9.1 Million Price Tag, Who Is the Hidden Winner in AI Computing Power?"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["million", "rack", "approximately", "rubin", "power", "vera", "data", "costs"]
source: https://q.futunn.com/feed/116719391540004?futusource=news_topic_page&lang=en-us
source_anchor: ""
source_lines: [1, 93]
section: "NVIDIA_Hardware"
---
Super Micro Computer delivers blockbuster Q1 guidance! Is the AI trade rally reigniting?

14.6M Views · 3089 Posts 

The arms race in AI infrastructure is entering an even more capital-intensive phase.

In the past, market discussions about AI data centers have typically focused on two issues: first, whether there are enough NVIDIA GPUs; and second, whether there is sufficient power supply.

However, a recent research report from Bernstein provides a more concrete answer: $NVIDIA (NVDA.US)$ Next-generationVera Rubin NVL72 AI rack costs approximately $9.09 million each,significantly higher than the previously circulated market estimate of around $8 million. Building a 1GW-scale Vera Rubin AI data center could entail total capital expenditures of up toapproximately $47.3 billion.。

This means AI data centers are no longer just about 'buying GPUs,' but rather represent a massive capital expenditure project encompassing GPUs, HBM, DRAM, NAND, networking, power, liquid cooling, and civil construction.

Breaking down rack costs: Why is it as high as $9.1 million?

Bernstein analysts pointed out that the widely reported figure of 'approximately $8 million per rack' in the media was based on outdated memory pricing and significantly underestimated actual costs. The key discrepancy lies in high-bandwidth memory (HBM): current HBM4 prices are around $16.6 per GB, but by 2027—when Vera Rubin systems are expected to ship at scale—prices could rise to approximately $53 per GB. NVIDIA is also likely to pass these costs on to end customers through dynamic pricing mechanisms.

Bernstein used a bottom-up approach to break down the Vera Rubin NVL72 rack line by line, ultimately arriving at a cost estimate of approximately $9.1 million.

GPUs remain the largest single cost component. The report shows that each Rubin GPU is priced at approximately $55,000, and with 72 GPUs per rack, the GPUs alone account for $3.96 million—nearly half of the total rack cost. Additionally, the 36 Vera CPUs per rack contribute approximately $180,000 in total.

Memory and storage costs have risen significantly, representing the primary source of divergence between this estimate and market expectations.Bernstein estimates this cost at approximately $3.2 million, substantially higher than the roughly $2 million derived from historical pricing. Of this, HBM4 contributes about $1.09 million, CPU DRAM (LPDDR5X) about $800,000, and direct-attached storage about $1.28 million. The report specifically cautions that memory and storage prices are highly volatile—NAND prices have surged 11.3x cumulatively from their April 2023 low to May 2026, equivalent to an annualized increase of 115%. Investors must continuously monitor price movements to maintain forecast accuracy.

Networking, cooling, and power collectively contribute approximately $2 million. Networking costs amount to about $1.27 million, including roughly $250,000 for NVLink switches, $240,000 for cabling, $380,000 for backplanes and other scale-out components, and $200,000 for Spectrum-X switches; cooling accounts for approximately $160,000, and power about $150,000.

How much would it cost to build a 1GW AI data center?

Now comes the most staggering figure.

Each Vera Rubin NVL72 rack consumes approximately 220 kW. Bernstein assumes rack power consumption accounts for roughly 78% of the data center’s total power draw, implying that a 1GW data center could accommodate approximately 3,557 Vera Rubin racks.

At approximately $9.09 million per rack, rack costs alone would total: 3,557 racks × $9.09 million ≈ $32.3 billion.

If we further include mechanical, electrical, land, building, and other infrastructure costs for the data center,Bernstein estimates the total cost at approximately $47.3 billion per gigawatt (GW),This continues to rise from the Blackwell cycle’s roughly $40.5 billion/GW, indicating that the capital intensity of AI data centers is still increasing.

This can be simply understood as:Going forward, anyone aiming to deploy top-tier AI computing capacity at the 1 GW scale will need to budget nearly $50 billion in capital expenditures on their balance sheet.

On the surface, Vera Rubin appears more expensive. However, from a compute-per-dollar perspective, the outlook isn’t pessimistic. Bernstein notes that the Vera Rubin NVL72 rack delivers approximately 2,520 PFLOPS of FP8 compute power, significantly higher than the Blackwell NVL72’s 720 PFLOPS. In other words, although the per-rack price has risen from around $4.3 million to about $9.1 million, the increase in compute performance is even greater.

This means that while AI data centers are becoming increasingly expensive, the amount of compute power purchased per dollar is still improving. This is precisely why cloud providers and AI labs remain willing to keep investing: in an environment of compute scarcity, next-generation GPUs offer higher returns per unit of compute, and as long as downstream AI demand continues to grow, there remains a strong rationale for expanding capital expenditures.

The real investment theme: AI Capex evolves from a 'GPU story' into a 'full-stack supply chain story'

If the first phase of AI development was defined by 'whoever controls GPUs wins,' then entering the Vera Rubin era—with its staggering capital expenditure of $47.3 billion/GW—marks the dawn of a new age:The investment logic for AI infrastructure has expanded beyond individual chips to encompass an end-to-end hardware supply chain—a 'system-level mega-engineering' endeavor.

When the cost of a single Vera Rubin rack soars to approximately $9.1 million, and the GPU itself accounts for 'only' about half of that, where exactly does the remaining several million dollars go? Examining the full landscape of the AI server supply chain reveals clearly that funding from this arms race is now flowing comprehensively into the following five core ecosystems:

--

--

--

1. Core AI compute engines and underlying IP: No longer just NVIDIA's solo show

Although $NVIDIA (NVDA.US)$ remains the undisputed star of this boom, the overall expansion in compute demand has also created opportunities for other giants.Advanced Micro DevicesandIntel is accelerating its catch-up efforts, aiming to carve out a share in inference workloads or specific application scenarios. Meanwhile, surging demand for underlying architectures and customized chips is fueling growth in ASIC design services and the IP segment, $Broadcom (AVGO.US)$ 、 $Marvell Technology (MRVL.US)$as well as$Arm Holdings (ARM.US)$ becoming an indispensable foundation for diversified AI computing capabilities.

2. Memory and data transmission 'highways': The critical battle to overcome bandwidth bottlenecks

According to Bernstein’s report, the most surprising cost surge comes from memory and networking. Massive data throughput requires compute power and data transmission to advance in lockstep.

Memory: Anticipated sharp price increases for HBM4 and widespread adoption of LPDDR5X mean that companies holding pricing power— $SK Hynix (000660.KR)$ and those rapidly advancing— $Samsung Electronics (005930.KR)$ , and $Micron Technology (MU.US)$ A golden window marked by simultaneous increases in both volume and price has arrived.

Optical Communications:With per-rack network costs reaching as high as $1.27 million, the optical communications supply chain is experiencing an unprecedented boom, and niche segments within the sector are flourishing like never before:

Network Switches and Equipment: Supporting the interconnection of clusters at the scale of 100,000 GPUs $Arista Networks (ANET.US)$ And, $Cisco (CSCO.US)$ is central to building the backbone network of AI data centers.

3. The 'arms manufacturers' behind computing power: wafer foundry, packaging & testing, and semiconductor equipment

No matter how expensive a chip design is, it requires cutting-edge manufacturing processes to be realized.

Wafer Foundry and Packaging & Testing: The undisputed leader $Taiwan Semiconductor (TSM.US)$ controls the lifeline of advanced process nodes and CoWoS packaging, while advanced packaging partners $Amkor Technology (AMKR.US)$ And, $ASE Technology (ASX.US)$ are also capturing massive spillover capacity demand.

Semiconductor Equipment Suppliers:Supporting this capacity expansion are upstream equipment giants—the lithography machine leader $ASML Holding (ASML.US)$ 、 $Applied Materials (AMAT.US)$ 、 $Lam Research (LRCX.US)$ , equipment manufacturers for testing $KLA Corp (KLAC.US)$ , as well as Japanese suppliers playing a critical role in specific processes (such as wafer thinning and dicing) $Shibaura Mechatronics (6590.JP)$ And, $Disco (6146.JP)$ 。

Substrates and PCBs: High-end servers impose extremely high demands on PCB layer count and materials, driving a revaluation of $TTM Technologies (TTMI.US)$ 、 $VGT (02476.HK)$as well as$Ibiden (4062.JP)$ 's valuation.

4. The foundation pushing physical limits: Power management and system integration

The Vera Rubin system has a single rack power consumption as high as 220 kW, presenting extreme challenges for power delivery and full-system assembly.

Power supply and management: Massive currents require highly efficient conversion and distribution, $STMicroelectronics (STM.US)$ 、 $INFINEON TECHNOLOG (IFNNY.US)$as well as$Monolithic Power Systems (MPWR.US)$ provides key power management ICs (PMICs) and power components.

Server Hardware and System Integration: Ultimately, assembling millions of dollars’ worth of components into a stable, 220kW 'liquid-cooled beast' places immense demands on hardware vendors’ system integration capabilities. $Super Micro Computer (SMCI.US)$ 、 $Dell Technologies (DELL.US)$ And, $Hewlett Packard Enterprise (HPE.US)$ Becoming the most direct 'shovel seller' in this infrastructure boom.

Summary: An Epic, Full-Chain Infrastructure Windfall

Overall, the estimated cost of $47.3 billion per gigawatt (GW) during the Vera Rubin cycle reflects a significant rise in the barriers to building AI data centers. This also signals a shift in infrastructure investment logic—from early-stage purchases of individual GPUs to a coordinated, full-supply-chain strategy encompassing wafer fabrication, high-end memory, optical communications, and power and system integration.

Supported by the rationale of 'improving price-performance per unit of compute,' cloud service providers are expected to maintain a certain level of continuity in their capital expenditures on AI infrastructure. Going forward, market focus will no longer be limited solely to key chip suppliers but will extend down the entire hardware ecosystem.

Comments (9)

 to post a comment
