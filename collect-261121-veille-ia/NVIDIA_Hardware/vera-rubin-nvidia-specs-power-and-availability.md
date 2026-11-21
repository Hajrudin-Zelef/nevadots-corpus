---
id: collect-261121-veille-ia/nvidia-hardware/vera-rubin-nvidia-specs-power-and-availability
title: "Vera Rubin: NVIDIA Specs, Power and Availability"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-08-26"]
keywords: ["nvidia", "rubin", "vera", "rack", "nvl72", "nvfp4", "blackwell", "platform"]
source: https://usd.ai/blog/vera-rubin
source_anchor: ""
source_lines: [1, 124]
section: "NVIDIA_Hardware"
---
# Vera Rubin Explained: Specs, Power and Availability

## Summary

- NVIDIA Vera Rubin, the platform after Blackwell, pairs the Rubin GPU with the Vera CPU; its liquid-cooled NVL72 rack holds 72 GPUs and 36 CPUs.
- NVIDIA rates the rack at 227 kW (MaxP), 187 kW (MaxQ) or 156 kW (MaxLPS), fully liquid-cooled at a 45°C inlet (October 2026).
- NVIDIA began production shipments in August 2026. CoreWeave, Oracle OCI and Google Cloud have announced availability, and NVIDIA expects supply to stay constrained.

## Introduction

Searches for "Vera Rubin" now split between the American astronomer and NVIDIA's next computing platform. For a GPU cloud operator, the NVIDIA platform raises practical questions. What is the system, what does it ask of a data center, and when can it earn revenue under an offtake contract?

Answers to those questions are scattered across launch blogs, datasheets, earnings calls and cloud announcements, each with its own date and baseline. The figures below come from NVIDIA's own dated documents and the companies' own announcements. Every comparison names its baseline, and systems that are shipping, available or planned are kept apart.

The sections below cover the platform and its specs, the power and cooling a rack needs, and who has Vera Rubin under constrained supply. The last section covers the astronomer the platform is named for.

## What Is NVIDIA Vera Rubin?

NVIDIA Vera Rubin is NVIDIA's computing platform after Blackwell, pairing the Rubin GPU with the Vera CPU. Its flagship system, the Vera Rubin NVL72, holds 72 Rubin GPUs and 36 Vera CPUs across 18 compute trays, with 9 NVLink switch trays. NVIDIA says the rack weighs roughly 4,000 lbs (March 2026).

At the GPU level, NVIDIA's technical overview lists up to 288 GB of HBM4 and up to 22 TB/s of memory bandwidth per GPU (January 2026). NVLink 6, the GPU-to-GPU interconnect, runs at 3.6 TB/s of bidirectional bandwidth per GPU. NVIDIA rates each GPU at 50 PFLOPS of NVFP4 inference and 35 PFLOPS of NVFP4 training. NVFP4 is NVIDIA's 4-bit floating-point format for AI math.

At the rack level, the Vera Rubin NVL72 datasheet lists 20.7 TB of HBM4 per rack (October 2026). It rates the rack at 3,600 PFLOPS of NVFP4 inference and 2,520 PFLOPS of NVFP4 training, equal to the per-GPU compute figures times 72. Its rack bandwidth figures are lower than the January per-GPU peaks times 72: 1,400 TB/s of HBM4 and 216 TB/s of NVLink. Those peaks would give 1,584 TB/s and about 259 TB/s, so plan on the datasheet's figures.

NVIDIA's own comparison puts Rubin at up to five times Blackwell's NVFP4 inference compute per GPU. NVIDIA's January 2026 table labels the baseline only as "Blackwell". The table below keeps NVIDIA's label.

| Per GPU, NVIDIA figures (January 2026) | Blackwell | Rubin | 
|---|---|---|
| NVFP4 inference (PFLOPS)¹ | 10 | 50 | 
| NVFP4 training (PFLOPS)² | 10 | 35 | 
| HBM bandwidth (TB/s) | 8 | 22 | 
| NVLink bandwidth, bidirectional (TB/s) | 1.8 | 3.6 | 

¹ NVIDIA's "Transformer Engine compute" figure. ² Dense compute.

NVIDIA's cost claims rest on modeled workloads. NVIDIA says Vera Rubin NVL72 delivers "up to 10x lower cost per million tokens" on long-context reasoning inference. The baseline is Blackwell NVL72 (GB200 NVL72 in NVIDIA's October 2026 datasheet), on a modeled Kimi-K2-Thinking workload with 32K input and 8K output tokens. For a modeled mixture-of-experts (MoE) training run, NVIDIA says Vera Rubin needs about one-quarter of the GPUs of Grace Blackwell NVL72.

Both are NVIDIA's modeled claims for those scenarios, published in January 2026 and restated in the October 2026 datasheet. Neither is a measured result on GB300 systems. An operator pricing capacity should test them against the workloads its customers will actually run.

The Vera Rubin hardware reference collects the full spec sheet in one place. The GB300 NVL72 reference page covers the current-generation rack for side-by-side planning. For the rack design both generations use, read how NVL72 racks work.

## What a Vera Rubin Rack Needs from Your Site

NVIDIA's datasheet rates the Vera Rubin NVL72 at 227 kW in MaxP, 187 kW in MaxQ and 156 kW in MaxLPS mode (October 2026). Each figure is NVIDIA's rating for an operating mode, and actual draw depends on how the rack runs. The site's power plan has to be built around the mode you will run.

The gap between modes is 71 kW per rack. Across a row of racks, that difference decides how much utility capacity, switchgear and cooling the site has to provide.

Cooling is liquid only. The datasheet specifies "100% liquid cooling engineered for 45°C warm-water inlet temperatures." Warm-water direct liquid cooling means the facility water loop and coolant distribution units (CDUs) must be in place before the racks arrive. CDUs move heat from the rack's coolant into the building's loop.

NVIDIA calls the system a "third-generation NVIDIA MGX rack" and says it is "designed to drop seamlessly into existing data center footprints" (March 2026). The footprint claim covers floor space. The operator still has to confirm loop, CDU and power capacity for each rack.

Inside the rack, power shelves convert AC to 54 V DC, according to NVIDIA's DGX Vera Rubin NVL72 user guide (November 2026). NVIDIA's March 2026 technical blog describes liquid-cooled busbars supporting up to 5,000 A. Together these describe how power moves from the shelves to the compute trays.

System builders publish the capacity of the equipment around the rack. Supermicro specifies 4 x 110 kW power shelves per rack and an in-row CDU of up to 5 MW serving several racks. These figures describe installed capacity, and the rack's load still follows its operating mode.

An equipment plan for a financing review documents the same items as a site plan. Each one below ties back to the figures in this section.

- **Operating mode and kW per rack.** Confirm your operating mode with the system builder and plan power for its rating: 156 kW, 187 kW or 227 kW.
- **Liquid-cooling loop and CDU capacity.** Show that the facility loop can supply a 45°C inlet and that CDU capacity covers every rack it serves.
- **Rack weight and floor loading.** At roughly 4,000 lbs per rack, confirm the floor rating in the hall and along the delivery path.
- **Delivery schedule.** Match the system builder's delivery dates to the dates power and cooling will be ready at the site.
- **Colocation agreement.** Show that the colocation agreement covers the power, cooling and space the racks need for the full offtake contract term.

## Who Has Vera Rubin Now, and What Is Still Planned

NVIDIA began production shipments of Vera Rubin in August 2026. NVIDIA CFO Colette Kress said on NVIDIA's Q2 FY27 earnings call on Aug 26, 2026: "We commenced production shipments of Vera Rubin earlier this month." NVIDIA forecast Vera Rubin at about 20% of data center revenue in Q3 FY27. That is a forecast, and NVIDIA reports Q3 FY27 results on Nov 17, 2026.

On the same call, NVIDIA said purchase orders came "from every major hyperscaler, AI cloud, and system OEM." Orders turn into revenue as systems ship. NVIDIA CEO Jensen Huang's figure of "about $40 billion per gigawatt" measures NVIDIA's revenue opportunity per gigawatt of capacity. NVIDIA has published no Vera Rubin system price.

Named availability covers three clouds as of Nov 11, 2026. The table separates systems that have shipped, are available to customers, are powered on in labs, are being deployed or are still planned.

| Status | Company | What was announced | Date | 
|---|---|---|---|
| Shipping | Dell | First Vera Rubin NVL72 systems shipped to CoreWeave | Jun 1, 2026 | 
| Available | CoreWeave | Announced availability, with Cognition "the first customer anywhere running production workloads on the system" | Sep 30, 2026 | 
| Available | Oracle OCI | General availability | Oct 27, 2026 | 
| Available | Google Cloud A5X | General availability | Nov 6, 2026 update | 
| Powered on in labs | Microsoft | "The first hyperscale cloud to power on" Vera Rubin NVL72, in its labs | Mar 16, 2026 | 
| Planned | Nebius | Vera Rubin NVL72 "for customers starting H2 2026" | Jan 5, 2026 | 
| Planned | Lambda | "Production availability is planned for the second half of 2026" | Jan 9, 2026 | 
| Planned | Crusoe | Deployments "targeted for late 2026 and throughout 2027" | Mar 16, 2026 | 
| Deploying | AWS | Per NVIDIA's August 2026 call, deploying 2 million GPUs plus Vera CPUs, some integrated with Rubin; no Vera Rubin instances announced | As of Nov 11, 2026 | 

CoreWeave's release also carries a customer performance figure. Cognition reported "up to a 4.8 times increase" in total token throughput for its SWE-2 inference workloads, measured against GB200 NVL72 (September 2026). That result covers one customer's workload on one baseline.

NVIDIA's inference add-on is already in production, and its larger rack designs carry no dates. NVIDIA announced on Aug 24, 2026 that Groq 3 LPX "is now in full production." Each LPX rack holds 256 LPU processors with 128 GB of on-chip SRAM, according to NVIDIA's March 2026 announcement.

NVIDIA's March 2026 technical blog describes two next steps: Rubin Ultra NVL576, built from eight 72-GPU racks, and Kyber, with 144 GPUs per rack. NVIDIA has published no dates for either, and no date for Feynman. NVIDIA said in September 2025 that Rubin CPX "is expected to be available at the end of 2026," and its current status is unconfirmed.

On NVIDIA's Q1 FY27 earnings call (May 20, 2026), NVIDIA CEO Jensen Huang said, "we'll be supply constrained throughout the entire life of Vera Rubin." Micron said in March 2026 that it had begun volume shipment of HBM4 "designed for NVIDIA Vera Rubin." SK hynix said in its third-quarter results that it plans to bring M15X to full capacity by year-end to raise HBM4 output (October 2026). No public system price or reliable per-GPU-hour list price exists as of November 2026.

In our view, Blackwell systems keep earning under their existing contracts while Vera Rubin ramps. The useful question for an operator is what each generation earns under contract over the loan term.

## Why Is It Called Vera Rubin?

NVIDIA named the platform for the American astronomer Vera Rubin. NVIDIA's CES 2026 keynote recap describes her as the "pioneering American astronomer Vera Rubin, after whom NVIDIA named its next-generation computing platform" (January 2026). NVIDIA gives no further reason for the choice.

According to Carnegie Science, Rubin measured galaxy rotation curves with Kent Ford in the 1970s that supported the existence of dark matter. She had joined Carnegie's Department of Terrestrial Magnetism in 1965 as its first woman staff scientist. Carnegie's obituary reports she died in Princeton, N.J., on Dec 25, 2016, aged 88.

Her name was on an observatory before it was on a GPU. In January 2020, the Large Synoptic Survey Telescope was renamed the NSF Vera C. Rubin Observatory. The observatory and NVIDIA's platform now share her name, which is why searches for "Vera Rubin" return both.

## Conclusion

In NVIDIA's figures, Vera Rubin raises per-rack compute and memory to 3,600 PFLOPS of NVFP4 inference and 20.7 TB of HBM4 per NVL72 (October 2026). The rack needs 156–227 kW depending on its operating mode, plus warm-water liquid cooling at a 45°C inlet. As of November 2026, it is shipping to a short list of clouds under constrained supply.

USD.AI's published underwriting standard lists Vera Rubin (VR200) as approved collateral (October 2026). GPU financing through USD.AI covers new deployments. Escrow releases the loan capital once the release conditions are met: the servers are installed, burned in and independently verified. Compute IQ scores each deal on the operator, the customer's credit and the offtake contract.

If you are planning a Vera Rubin deployment, bring your equipment plan: system, quantity, supplier, site, power and cooling, and the offtake contract. Talk to USD.AI to review it.

## FAQ

These answers cover the platform, its shipping dates and its rack power. Each uses NVIDIA's dated figures as of November 2026.

### What is NVIDIA Vera Rubin?

NVIDIA Vera Rubin is NVIDIA's computing platform after Blackwell, pairing the Rubin GPU with the Vera CPU. Its flagship system, the Vera Rubin NVL72, puts 72 Rubin GPUs and 36 Vera CPUs in one liquid-cooled rack.

### When does Vera Rubin ship?

NVIDIA began production shipments of Vera Rubin in August 2026, according to its Aug 26, 2026 earnings call. CoreWeave, Oracle OCI and Google Cloud announced availability between September and November 2026.

### How much power does a Vera Rubin NVL72 rack use?

NVIDIA's October 2026 datasheet rates the Vera Rubin NVL72 at 227 kW in MaxP, 187 kW in MaxQ and 156 kW in MaxLPS mode. Each figure is a rating for an operating mode, so a site should plan power for the mode it will run.
