---
id: collect-261121-veille-ia/datacenters-infra/bernstein-ai-data-center-investment-reaches-up-to-39-5-billion-per-gw-depreciation-is-the-biggest-bu
title: "Bernstein: AI Data Center Investment Reaches Up to $39.5 Billion per GW — Depreciation Is the Biggest Burden — BigGo Finance"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: []
keywords: ["billion", "data", "center", "cost", "rubin", "bernstein", "approximately", "million"]
source: https://finance.biggo.com/news/95215e61-23e7-4057-baea-71291e662af1
source_anchor: ""
source_lines: [1, 71]
section: "Datacenters_Infra"
---
# Bernstein: AI Data Center Investment Reaches Up to $39.5 Billion per GW — Depreciation Is the Biggest Burden

How much does it actually cost to build an AI data center with 1GW of power capacity? Wall Street research firm Bernstein's latest breakdown provides an answer: depending on the accelerator architecture, capital expenditure ranges from $34.6 billion to $39.5 billion (approximately NT$1.1 trillion to NT$1.3 trillion). Nvidia's Vera Rubin architecture is the most expensive, while OpenAI's in-house ASIC architecture Jalapeno is relatively cheaper — but the gap between the two is actually smaller than most people imagine.

In a research report published on October 5, Bernstein conducted a systematic cost comparison of major AI accelerator architectures from Nvidia, AMD, Google, and OpenAI, drawing on industry expert interviews, supply chain surveys, corporate disclosures, and third-party data. The report also significantly revised down the cost estimate for a single Nvidia Rubin NVL72 rack, from the previous $9.1 million to $7.52 million (approximately NT$240 million), a reduction of about 17%, primarily driven by adjustments to HBM pricing and NAND storage capacity assumptions.

### Wide Gap in Individual System Prices, but Overall Build Costs Converge

Looking at hardware alone, the price differences across architectures are substantial. Bernstein estimates that Nvidia's current-generation GB200 NVL72 rack costs approximately $4.13 million, while the next-generation Vera Rubin NVL72 comes in at about $7.52 million. AMD's MI455X Helios rack is approximately $7.33 million, quite close to Rubin. Google's TPU v7 Ironwood single-system cost is only about $2.69 million, while OpenAI's Jalapeno system, which uses a dual-rack design, costs approximately $5.03 million.

However, the sticker price of an individual system does not directly reflect the overall cost of building a data center. After factoring in power consumption and hardware configuration differences, Bernstein's model shows that building a 1GW data center requires approximately $39.5 billion with Nvidia's Rubin architecture, about $35.7 billion with AMD Helios, roughly $39 billion with Google TPU v7, around $34.6 billion with OpenAI Jalapeno, and approximately $36.5 billion with Nvidia GB200.

One noteworthy observation: Google's TPU single-system cost is less than 40% of Rubin's, yet the total investment per GW of data center capacity differs by only about $500 million (approximately NT$16 billion). The key difference lies in power density. Bernstein estimates that a Rubin NVL72 rack consumes about 220 kilowatts, while a TPU v7 system draws roughly 80 kilowatts. After accounting for supporting infrastructure power requirements, a 1GW data center needs approximately 3,557 Rubin systems but about 9,783 TPU systems. Ultimately, the compute hardware investment per GW for the two architectures comes to roughly $26.8 billion and $26.3 billion respectively — a fairly narrow gap.

This means that while in-house ASICs can reduce chip and individual server prices, whether they can meaningfully lower the overall cost of building a data center still depends on power density, network architecture, and the quantity of hardware required.

| Architecture | Single-System Cost (US$10,000) | 1GW Data Center Total Investment (US$100M) | Cost per Accelerator-Hour (US$) | 
|---|---|---|---|
| Nvidia Vera Rubin NVL72 | 752 | 395 | 3.18 | 
| AMD MI455X Helios | 733 | 357 | 3.23 | 
| Google TPU v7 Ironwood | 269 | 390 | 1.29 | 
| OpenAI Jalapeno | 503 | 346 | 1.27 | 
| Nvidia GB200 NVL72 | 413 | 365 | 1.77 | 

Note: Cost per accelerator-hour is estimated using a model assuming a five-year accelerator lifespan, electricity at $0.15 per kilowatt-hour, and 40% utilization. These figures do not represent the GPU rental prices that cloud service providers charge customers.

### Behind the Rubin Cost Revision: A Major Correction in HBM Price Assumptions

Bernstein's downward revision of Rubin NVL72 cost expectations stems from five factors: lowering NAND storage capacity assumptions, reducing HBM4 price expectations, revising network equipment costs, adjusting cooling and power system costs, and updating DRAM and NAND pricing.

Among these, the change in HBM pricing is the most significant. The firm lowered its HBM4 price assumption from $48 per GB to $30.6 per GB (approximately NT$980), a reduction of about 36%. Analysts believe supply chain feedback has been weaker than previously expected, while confidence in the judgment that HBM margins will eventually converge with traditional DRAM has diminished. However, Bernstein still expects Nvidia to pass through some memory costs to end customers and charge a premium of about 10% on HBM. Based on 20.7TB of HBM4 capacity per rack, Rubin's HBM cost comes to approximately $697,000.

The GPU itself remains the largest cost component. Bernstein estimates the Rubin GPU at approximately $55,000 per unit, with 72 units totaling $3.96 million, accounting for about 53% of the total rack cost.

Additionally, the firm significantly reduced its estimate of internal NAND storage capacity for Rubin. The previous model assumed internal NAND capacity could reach approximately 2.2PB, with about 1.2PB of external ICMS storage included. The latest model lowers internal NAND capacity to 576TB while excluding external ICMS and future CMX storage from rack bill-of-materials costs. Traditional memory prices have actually risen, with DRAM price assumptions raised from $14.85 per GB to $16.45 per GB, and TLC NAND from $0.37 to $0.38.

### Structural Differences Between AMD and the In-House Camp

AMD Helios's single-rack cost is close to Rubin's, but the internal composition is markedly different. Bernstein estimates that AMD's MI455X GPU, excluding HBM, costs approximately $35,000 per unit — lower than Rubin's $55,000 — with 72 units totaling about $2.52 million, or $1.44 million less than Rubin. Analysts believe part of the difference may stem from AMD's lower GPU margins.

But AMD spends more on other hardware. Helios configures approximately 31.1TB of HBM per rack, higher than Rubin's 20.7TB, corresponding to HBM costs of about $950,000. CPU-side DRAM capacity is estimated at 74TB, costing approximately $1.21 million — also notably higher than Rubin. Network differences are equally pronounced: AMD Helios network equipment costs about $1.47 million per rack, higher than Rubin's $1.17 million. Bernstein notes that AMD uses an Ethernet-based UALink interconnect solution, with some scale-up switching equipment provided by Celestica, and related switch costs may be lower than Nvidia's proprietary solution. However, AMD configures up to three network interface controllers per GPU, increasing demand for scale-out switches, network interfaces, and optical transceivers, offsetting some of the cost advantage.

OpenAI's Jalapeno takes a different design approach to networking and memory configuration. It is a dual-rack system optimized for AI inference: the main rack contains 16 Katsu trays with a total of 32 Turin-class CPUs, while the other rack houses 16 Vindaloo ASIC trays with a combined 128 ASICs, along with Chana switch trays. Bernstein estimates each ASIC, including HBM, costs approximately $22,500, with 128 units totaling $2.88 million; the entire system costs about $5.03 million.

The more important distinction lies in network design. OpenAI is attempting to expand the scale-up interconnect range, storing more data in local HBM and DRAM to reduce reliance on traditional scale-out networking and NAND storage. Jalapeno's scale-up network uses Broadcom Tomahawk 6 switching silicon and incorporates optical circuit switching (OCS) technology, with overall network equipment costs of approximately $810,000.

Google's TPU v7 Ironwood takes yet another path. Its single-system cost is about $2.69 million, including 64 ASICs, 16 Axion CPUs, and supporting storage and network equipment. The TPU ASIC, excluding HBM, is priced at approximately $22,000 — lower than the estimated price of nearly $30,000 for Nvidia's GB200 GPU. The research team attributes this to margin differences between Broadcom and Nvidia, with their model assuming margins of approximately 55% and 70% respectively. Google's TPU uses HBM3E, with 192GB per ASIC and total capacity of about 12.3TB, costing roughly $275,000. On the networking side, Google also uses OCS but deploys it more for scale-up interconnect, with OCS costs of approximately $154,000 per system.

### Depreciation Is the Heaviest Burden on AI Data Centers

After comparing hardware investments, Bernstein went further to calculate the operating economics of AI data centers. The report notes that even at a relatively high electricity price of $0.15 per kilowatt-hour, a 1GW data center's annual electricity expenditure is approximately $1.3 billion (about NT$41 billion). By comparison, a data center using the Rubin architecture with total investment of approximately $39.5 billion, if all capital expenditure is simply depreciated over six years on a straight-line basis, would incur annual depreciation of about $6.6 billion (approximately NT$210 billion). This means the depreciation burden from capital expenditure far exceeds electricity costs.

Bernstein argues that because servers, storage, and network hardware typically have shorter useful lives than land, buildings, and other infrastructure, the importance of IT hardware in analyzing the long-term economics of AI compute is even greater than its share of initial investment suggests. Depreciation policies also vary across companies: Google depreciates servers and network equipment over six years, Amazon over five years, Meta over four to five years, Microsoft over two to six years for related computing equipment, and CoreWeave over six years for technology equipment.

Under a model assuming a five-year accelerator lifespan, electricity at $0.15 per kilowatt-hour, and 40% utilization, the cost per accelerator-hour is approximately $3.18 for Rubin, $3.23 for AMD Helios, $1.29 for Google TPU v7, $1.27 for OpenAI Jalapeno, and $1.77 for Nvidia GB200. However, these figures are not the GPU rental prices that cloud service providers charge customers, nor can they be directly equated to the cost of completing the same AI tasks.

### Global AI Compute Supply Continues to Expand, Infrastructure Construction Accelerates Overseas

Despite the high construction costs, Bernstein still expects global AI compute equipment shipments to continue growing. Based on estimates of accelerator shipments and their power consumption, the data center power capacity corresponding to global AI equipment shipments is projected to increase from 27GW in 2025 to 35.5GW in 2026, and further to 43.3GW in 2027. Nvidia Rubin will gradually become a significant growth driver, with related equipment shipments in 2026 corresponding to approximately 8.3GW of power, rising to 12.1GW in 2027. Nvidia B200's corresponding shipment scale is expected to decline from 9.9GW in 2025 to 5.4GW in 2026, and further to 0.2GW in 2027.

Notably, the power capacity corresponding to global AI equipment shipments is significantly higher than Bernstein's forecast for new data center capacity in the United States. The firm expects the U.S. to add approximately 18GW of new data center capacity in 2026 and about 26GW in 2027. The research team believes this discrepancy partly reflects the time lag between equipment shipments and actual deployment, and may also indicate that AI infrastructure construction is increasingly shifting overseas. U.S. data center projects face challenges including power supply constraints, operating conditions, and policy restrictions, which may delay some new capacity from coming online.

Bernstein notes that OpenAI's collaboration with Firmus on AI data center capacity across Australia, Singapore, Indonesia, and Malaysia exemplifies the importance of overseas infrastructure development. Analysts expect the United States' share of future global data center capacity may fall below historical levels.

Increased infrastructure investment will also affect the growth dynamics within the hardware supply chain. Bernstein projects that from 2025 to 2027, Innolight and Eoptolink could achieve revenue compound annual growth rates of 92% and 78% respectively, while Delta Electronics and Vertiv are expected to grow 38% and 32% respectively over the same period. However, not all hardware suppliers will see simultaneous improvements in profitability — some server manufacturers, despite faster revenue growth, may face margin pressure.

Overall, Bernstein believes that AI end-application adoption has shown signs of acceleration in 2026, driving rapid growth at AI labs. As Nvidia Rubin enters a new product cycle and custom ASICs gain broader adoption, available compute capacity will further increase, creating more conditions for AI application proliferation. However, the economics of AI infrastructure depend not only on chip prices but also on memory, networking, power supply, and depreciation costs. For technology companies continuing to pour massive capital expenditure into AI, improving hardware utilization efficiency and controlling overall construction costs will be critical factors influencing long-term investment returns.

Once added, BigGo Finance appears first in Google Search Top Stories, so you get the broadest, most up-to-the-minute, and most comprehensive global financial news first.

Add preferred source
