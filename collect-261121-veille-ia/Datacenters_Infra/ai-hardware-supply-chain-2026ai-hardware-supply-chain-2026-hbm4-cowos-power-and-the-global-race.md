---
id: collect-261121-veille-ia/datacenters-infra/ai-hardware-supply-chain-2026ai-hardware-supply-chain-2026-hbm4-cowos-power-and-the-global-race
title: "AI Hardware Supply Chain 2026AI Hardware Supply Chain 2026: HBM4, CoWoS, Power and the Global Race…"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: ["2026-10-31"]
keywords: ["billion", "hardware", "supply", "about", "power", "chain", "race", "memory"]
source: https://medium.com/@rameezramzan/ai-hardware-supply-chain-2026ai-hardware-supply-chain-2026-hbm4-cowos-power-and-the-global-race-0470a92a56e4
source_anchor: ""
source_lines: [1, 665]
section: "Datacenters_Infra"
---
# AI Hardware Supply Chain 2026AI Hardware Supply Chain 2026: HBM4, CoWoS, Power and the Global Race to Build AI Infrastructure

*From NVIDIA’s Rubin racks and Taiwan’s packaging lines to Korea’s sold-out memory fabs, China’s Ascend push and the power grid, the real contest is over who can secure, build, power and finance complete AI systems.*

## Key takeaways

The AI hardware race has shifted from a contest over chips to a contest over memory, packaging, power and financing, and as of 31 October 2026 memory and power are the tightest links.

• **Memory is the binding constraint.** SK hynix said on 28 October that its DRAM and HBM output is effectively sold out through 2029 once volumes under negotiation are counted. Samsung says its 2027 HBM supply is already sold out.

• **Memory makers are posting record profits.** SK hynix reported Q3 operating profit of KRW 83.3 trillion on a margin of about 80%. Samsung’s preliminary Q3 operating profit was KRW 107.4 trillion.

• **Packaging is easing, not solved.** TSMC reported Q3 revenue of $46.7 billion and said CoWoS demand still exceeds supply.

• **Rubin has arrived, but Blackwell still leads 2026.** TrendForce expects GB300 racks to drive NVIDIA’s 2026 shipments and Vera Rubin VR200 racks to lead in 2027.

• **Custom silicon is the fastest-growing slice.** ASIC-based servers are forecast at about 27.8% of 2026 AI server shipments, and every one of those chips still needs TSMC capacity and HBM.

• **Power now gates deployment.** Racks draw 100 kW or more, liquid cooling is standard on the newest platforms, and grid connections can take years.

• **The bill is landing on balance sheets.** Alphabet posted negative free cash flow for a second straight quarter, Microsoft priced $25 billion of bonds on 29 October, and Amazon has raised its 2026 capex outlook to about $220 billion, citing memory costs.

• **Buyers should expect higher prices.** TrendForce forecasts Q4 2026 contract prices up 10–15% for conventional DRAM and 15–20% for NAND, quarter on quarter.

## The AI hardware market

The latest earnings season shows record supplier profits, rising hyperscaler spending and, for the first time, visible strain on cash flow.

**Memory and chips**

- **SK hynix Q3 2026:** Revenue KRW 104.6 trillion; operating profit KRW 83.3 trillion (about 80% margin); DRAM and HBM effectively sold out through 2029.*(Company results, 28 Oct, via Korea Times and TrendForce)*
- **Samsung Q3 2026 (preliminary):** Operating profit KRW 107.4 trillion on revenue of KRW 195 trillion; HBM bit shipments reportedly up about 50% on the quarter.*(Samsung guidance, 8 Oct, via TrendForce and Reuters)*
- **TSMC Q3 2026:** Revenue $46.7 billion (+16.2% on the quarter); gross margin 66.4%; Q4 guidance $52.3–54.3 billion; 2026 capex $60–64 billion.*(TSMC earnings call, 15 Oct)*
- **NVIDIA:** Fiscal Q2 FY2027 revenue $96.2 billion; fiscal Q3 guidance $108 billion, plus or minus 2%; results expected around 18 Nov (date unconfirmed).*(NVIDIA, 26 Aug; MarketBeat)*
- **Memory contract prices, Q4 2026 (forecast):** Conventional DRAM +10–15% on the quarter; DRAM including HBM +15–20%; NAND +15–20%.*(TrendForce, 30 Sep)*

**Hyperscalers**

- **Microsoft, fiscal Q1 FY2027 (Jul-Sep 2026):** Azure +48% to $33.2 billion; capex $45.2 billion including finance leases; FY2027 capex about $170 billion.*(Microsoft, 28 Oct)*
- **Alphabet Q3 2026:** Capex about $51.4 billion; free cash flow about -$9.1 billion; shares fell roughly 11–12%.*(24/7 Wall St., 29 Oct)*
- **Amazon Q3 2026:** AWS +39% to $46 billion; 2026 capex about $220 billion.*(Amazon, 29 Oct, via Constellation Research)*
- **Meta 2026:** Capex guidance $135–145 billion.*(Meta, late Oct, via 24/7 Wall St.)*

**AI server mix**

- **ASIC share of AI servers, 2026 (forecast):** About 27.8% of shipments, the highest since 2023.*(TrendForce)*

## Why the AI hardware race is a system race, not a chip race

No single company or country controls enough layers of the stack to build AI infrastructure alone, so the winners are the ecosystems that can secure, manufacture, package, connect, power and finance a complete system.

A modern AI data centre is an industrial system. A GPU cannot deliver its full performance without enough memory bandwidth. Thousands of GPUs cannot work together without high-speed networking. And racks cannot be switched on without power and cooling that many sites do not yet have.

The table below shows where each layer is concentrated and what is limiting it today.

- Accelerator Design

- Who leads: NVIDIA, AMD, Google (with Broadcom), Amazon (with Marvell), Huawei.
- Binding constraint: HBM4 and CoWoS allocation.

- Foundry and Packaging

- Who leads: TSMC (Taiwan) and SMIC (China).
- Binding constraint: CoWoS capacity, 2nm production ramp, and testing capacity.

- Memory

- Who leads: SK hynix and Samsung (South Korea), Micron (US), CXMT (China).
- Binding constraint: HBM4 and server DRAM capacity committed under long-term agreements.

- Storage

- Who leads: NAND and enterprise SSD manufacturers.
- Binding constraint: NAND allocation and high-capacity QLC SSD availability.

- Networking and Optics

- Who leads: NVIDIA, Broadcom, and optical transceiver manufacturers.
- Binding constraint: 800G and faster optical transceivers, including laser availability.

- Servers and Racks

- Who leads: Foxconn, Quanta, Wistron, Inventec, and Pegatron.
- Binding constraint: Rack integration capacity and liquid-cooling infrastructure.

- Power and Cooling

- Who leads: Utilities, power infrastructure providers, and cooling equipment manufacturers.
- Binding constraint: Grid connections, power delivery, and heat removal capacity.

- Capital

- Who leads: Hyperscalers, sovereign wealth funds, and chipmakers’ own balance sheets.
- Binding constraint: Free cash flow, financing availability, and appetite for debt.

The United States leads in chip design, hyperscale spending and software. Taiwan leads in advanced manufacturing and packaging. South Korea leads in memory. China is building a parallel stack under export controls. Japan, the Netherlands, Malaysia, Singapore, India and the Gulf states each hold a piece that the rest cannot easily replace.

## Accelerators: NVIDIA Rubin, AMD Helios and the rise of custom silicon

NVIDIA still sets the pace, but the accelerator market is diversifying faster than at any point since the AI boom began, while the inputs stay concentrated.

## NVIDIA: from Blackwell to Vera Rubin

NVIDIA reported revenue of $96.2 billion for fiscal Q2 FY2027 (May-July 2026) and guided to $108 billion for the next quarter. TrendForce puts data centre at close to 93% of its revenue.

Rubin is NVIDIA’s Blackwell successor. Each GPU carries up to 288 GB of HBM4, and it pairs with the 88-core Vera CPU. Partner systems began shipping in the second half of 2026. TrendForce’s April outlook said Rubin’s ramp slipped, leaving Blackwell above 70% of NVIDIA’s high-end GPU shipments in 2026, with GB300 as the lead product. The same research expects VR200 racks to account for the largest share in 2027, at roughly double the system price of GB300, and puts combined NVL72 rack output value above $710 billion in 2027.

The customer base is also widening. TrendForce estimates the five big North American cloud providers will fall from about 70% of NVIDIA rack procurement in 2025 to about 60% in 2026, as xAI, CoreWeave and other neoclouds, and sovereign buyers take more.

## AMD: the second source

AMD’s Instinct MI450-series accelerators (the MI455X is the flagship) carry up to 432 GB of HBM4 and anchor its Helios rack. OpenAI has committed to 6 GW of AMD GPU capacity, with the first gigawatt expected in the second half of 2026.

The constraint is memory. After meeting Samsung executives in Seoul on 7 October, CEO Lisa Su described memory supply as very tight. AMD and Samsung then signed a three-year memorandum covering HBM4, DDR5, advanced packaging and possible foundry work, according to press reports. AMD says it now plans supply three to five years ahead.

## Custom silicon: a growing share, the same bottlenecks

TrendForce forecasts that ASIC-based servers will reach about 27.8% of AI server shipments in 2026, with ASIC shipments growing about 44.6% against about 16.1% for GPU servers. It expects the ASIC share to approach 40% by 2030.

• **Google** is the only large cloud provider building more ASIC servers than GPU servers. TrendForce expects TPU volume to grow about 80% in 2026.

• **Amazon** expects Trainium to exceed 40% of its own AI server shipments in 2026, and Amazon’s custom silicon business passed a $20 billion annual run rate in Q1.

• **Broadcom and Marvell** co-design roughly 95% of custom AI ASICs. Broadcom is working with Google, OpenAI (a 10 GW programme) and Anthropic, whose Google TPU capacity via Broadcom is expected to rise from about 1 GW in 2026 to more than 3 GW in 2027.

• **Inference is the driver.** Deloitte projects that about two-thirds of AI compute in 2026 is inference, where custom chips can win on cost per token.

The point for buyers is that custom chips do not escape the supply chain. TSMC manufactures them, they use the same HBM suppliers, and they compete for the same CoWoS capacity. The industry is moving to many accelerator designs on one concentrated manufacturing base.

## HBM4 and DRAM: the shortage that now sets the pace

Memory has overtaken packaging as the hardest AI hardware input to secure, and the three suppliers are telling the market the squeeze will last for years.

## What the suppliers are saying

• **SK hynix** (28 October): DRAM and HBM supply is effectively sold out through 2029 once long-term agreements and demand under negotiation are counted. It has signed long-term agreements with 14 customers, expects HBM4 to overtake HBM3E in Q4, and press reports say it sees shortages lasting through 2030.

• **Samsung** (October): 2027 HBM supply is already sold out, and the company warned that the AI memory shortage will worsen through 2027. Reuters reports that Samsung and Micron both expect supply to stay tight through 2028.

• **Micron** (July): HBM3E and HBM4 capacity was fully pre-sold through the end of 2026.

## Why HBM4 tightens everything else

HBM stacks DRAM dies vertically and bonds them next to the accelerator. It consumes more wafer capacity per bit than standard DRAM, needs specialised stacking and testing, and has lower early yields. HBM4 entered mass production in 2026, with Samsung and SK hynix shipping for NVIDIA’s Vera Rubin and Micron announcing volume production in March. Industry estimates put SK hynix at roughly 60–70% of Rubin’s HBM4 volume and Samsung at 25–30%, with Micron supplying the rest.

Because memory makers shift wafers towards HBM and server DRAM, other products lose out. TrendForce expects PC DRAM to receive a shrinking share of capacity in 2027, while server DRAM stays in short supply.

## Prices

TrendForce price trackers show conventional DRAM contract prices rising roughly 93–98% in Q1 2026 alone. Its latest outlook, published 30 September, expects further increases at a slower pace in Q4: 10–15% for conventional DRAM, 15–20% for DRAM including HBM, and 15–20% for NAND.

## Memory inflation is now a capex line item

The memory squeeze has moved from component price lists to hyperscaler guidance. Meta raised its 2026 capex range citing higher component pricing. Amazon lifted its 2026 outlook to about $220 billion, citing memory costs. Microsoft earlier said roughly $25 billion of its 2026 plan reflected higher component prices (reported July 2026). A rising share of AI capex is therefore buying the same hardware at a higher price rather than more hardware.

## The spillover

The shortage is not confined to data centres. It is lifting prices for servers, PCs, phones and consoles that draw on the same DRAM lines, and Western Digital’s 2026 hard drive supply was reportedly booked for enterprise customers before February. For a business buying ordinary infrastructure, the AI boom is a cost event even when the business runs no AI workloads.

## NAND, enterprise SSDs and HDDs: storage enters its own price cycle

AI inference is turning storage from a background purchase into a constrained one, and NAND is following DRAM up, though TrendForce expects the two markets to diverge in 2027.

## Why AI raises storage demand

Training data, checkpoints, vector databases, inference caches and agent logs all need fast persistent storage. As workloads move from training to production inference and retrieval-augmented generation, demand shifts towards high-capacity, read-heavy enterprise SSDs. QLC NAND, which stores four bits per cell, suits that profile because it lowers cost per bit, though it trades away write endurance. TrendForce’s September outlook reported enterprise SSD bit demand growing by more than 80% year on year in 2026, with QLC adoption accelerating in vector database workloads.

Enterprise and consumer SSDs are different products. Enterprise drives need power-loss protection, consistent latency, strong error correction, firmware reliability and validation on specific server platforms. Matching capacity and interface is not enough.

## Pricing and supply

• **Q4 2026:** TrendForce forecasts NAND contract prices up 15–20% on the quarter.

• **Spending:** IDC reported worldwide external OEM enterprise storage spending of $10.3 billion in Q2 2026, up 33.7% year on year, with much of the rise due to price rather than shipped capacity. IDC’s outlook is for elevated storage pricing to last at least through the second half of 2027.

• **2027 divergence:** TrendForce’s July report on memory supply and demand points to structural DRAM tightness in 2027 alongside possible NAND oversupply. Buyers should plan DRAM and NAND on separate price curves.

• **Hard drives:** Western Digital’s 2026 supply was reportedly committed to enterprise customers early in the year, which pushes some bulk storage demand towards high-capacity SSDs.

## What it means for buyers

A company that deferred a storage refresh to prioritise AI spending may find the replacement costs far more than it budgeted. Validate drive models and firmware for each server platform, ask for confirmed stock rather than quoted availability, and treat QLC as a deliberate choice for read-intensive tiers rather than a default.

## TSMC, CoWoS and Taiwan: the manufacturing centre of gravity

Taiwan remains the single most important node in the AI supply chain, and TSMC’s October results show demand still outrunning advanced packaging capacity.

## TSMC’s Q3 2026 results

TSMC reported Q3 revenue of $46.7 billion, up 16.2% on the quarter and above its guidance, with a gross margin of 66.4%. Net income rose about 69% year on year, and the 2nm node reached roughly 8% of wafer revenue. Q4 guidance is $52.3–54.3 billion. Full-year capex is guided to $60–64 billion, up from $52–56 billion earlier in the year.

Management said demand for CoWoS, its advanced packaging platform, still exceeds supply, that larger-reticle packages have entered production, and that it is working with tool suppliers because equipment is tight. It did not give a capacity figure.

## CoWoS: the less visible bottleneck

CoWoS integrates logic dies and HBM stacks in one package. Adding front-end wafer capacity does not fix a packaging shortage, because a foundry can have wafers and still be unable to ship finished accelerators. Industry trackers estimate TSMC’s CoWoS capacity at roughly 120,000–130,000 wafers per month by the end of 2026, more than ten times the late-2023 level. One tracker estimates NVIDIA holds about 60% of CoWoS allocation, with Broadcom’s share trimmed. Treat these as estimates, since TSMC does not publish them.

TSMC has also outlined a 9.5-reticle CoWoS solution for volume production in 2027, able to integrate 12 or more HBM stacks, and its System-on-Wafer-X architecture for the same year.

## The ecosystem behind it

Taiwan’s advantage is density. Foundry, packaging, substrates, testers and server assemblers sit within a short distance of each other. Foxconn, Quanta, Wistron, Inventec and Pegatron build most of the world’s AI servers. Wistron reported that AI infrastructure made up 79% of its Q1 2026 revenue, against 46% in 2024, according to Focus Taiwan. NVIDIA’s May 2026 account of its Taiwan partners cited more than 500 ecosystem partners across wafers, packaging, testing and system integration.

TSMC continues to expand in the United States, including Arizona, but a US fab does not replicate Taiwan’s supplier network.

## The geopolitical exposure

A disruption involving Taiwan would hit accelerators, packaging, server assembly and, beyond tech, telecoms, autos, defence and finance. Diversification reduces risk over years, not months. Buyers should map which of their suppliers’ products depend on Taiwanese packaging or assembly and plan for the delay that a regional disruption would cause.

## South Korea: the memory powerhouse in its biggest cycle

South Korea’s two memory makers are posting the largest profits in their history, and they are using that position to lock customers into multi-year contracts.

**SK hynix** reported Q3 revenue above KRW 100 trillion for the first time (KRW 104.6 trillion) and operating profit of KRW 83.3 trillion, an operating margin of about 80%. It plans to bring its M15X fab to full capacity by year end to raise HBM4 output. The company also announced record shareholder returns.

**Samsung Electronics** pre-announced Q3 operating profit of KRW 107.4 trillion, up about 783% from a year earlier, on revenue of KRW 195 trillion, the first time a technology company has posted more than KRW 100 trillion in a quarter. Its chip division earned KRW 89.2 trillion. Analysts attribute much of the gain to higher conventional memory prices and server mix, not HBM4 alone, and Samsung has not disclosed HBM profit separately. Samsung’s 4nm foundry process produces the HBM4 base die, which links its memory and foundry businesses. Reports say it plans to commit about 70% of memory capacity to long-term agreements.

**Micron**, headquartered in the United States, is the third HBM supplier, which means the leading US accelerator designers depend heavily on Korean memory.

## Korea’s AI infrastructure paradox

Korea’s AI data centre push does not make its hardware supply chain Korean. A Digital Today report in August 2026 highlighted continued reliance on Taiwanese server assembly and cooling suppliers in Korea’s AI factory projects. Korea leads in memory, but it still depends on American chip design, Taiwanese integration and international equipment suppliers.

## What to watch

The risk for Korean suppliers is the other side of record margins: customers who feel squeezed will push for second sources, including China’s CXMT, and higher prices invite capacity expansion. Neither has changed the 2027 outlook yet, because 2027 volumes are largely committed.

## China: building a parallel AI hardware stack

China is no longer just trying to replace one NVIDIA GPU. It is building a separate stack of chips, memory, packaging, interconnect and software, and in 2026 that stack found its first large-scale production customers.

## Huawei and the Ascend line

Huawei expects AI processor revenue of roughly $12 billion in 2026, up from $7.5 billion last year, according to Tom’s Hardware. The Ascend 950PR is now the main procurement target for China’s largest technology companies, and the training-focused 950DT is scheduled for Q4 2026. Huawei’s Atlas 950 SuperPoD, which links 8,192 Ascend chips, is also due in Q4, with the 960 and 970 generations following in 2027 and 2028.

A turning point was DeepSeek V4. Reports say a model of about 1.6 trillion parameters ran in production on Ascend 950 hardware from April 2026, with Huawei engineers working directly with DeepSeek. That moved the question from whether the chips work to whether they can be made in volume. Tom’s Hardware reported that NVIDIA’s data centre share in China has fallen to effectively zero, and TrendForce expects China’s high-end AI chip market to grow more than 60% in 2026 with domestic suppliers taking about half.

## The manufacturing ceiling

• **Foundry:** SMIC makes the Ascend 950PR on a 7nm-class process without EUV lithography. Beijing is reported to be targeting a fivefold increase in 7nm and 5nm-class output within two years. JPMorgan estimates SMIC’s wafer-to-packaged-chip cycle at about eight months, against roughly three for comparable TSMC nodes.

• **Packaging:** Domestic HBM base-die and 2.5D packaging lines at JCET and others are running at full capacity, and new capacity is not expected to matter until 2027.

• **Memory:** CXMT was reported by Reuters on 28 October to be preparing limited HBM3E production at yields well below established suppliers, while Huawei has developed its own HBM for the Ascend 950.

• **Gaps:** A June 2026 assessment by CEIAS found significant remaining gaps in advanced logic and memory manufacturing despite progress in chip design.

## Two software worlds

Two software ecosystems are hardening: NVIDIA’s CUDA, and Huawei’s CANN for Ascend. Each one raises switching costs for the developers invested in it. A CSIS analysis in March 2026 argued that allied restrictions have strengthened China’s incentive to localise equipment and chips, which cuts both ways: restrictions slow access to the best technology and speed up the domestic alternative.

What to watch is volume, yield and reliability at scale. Announcements of production programmes are not the same as commercial competitiveness, and China’s architectures may increasingly be optimised for different trade-offs, such as using more, less advanced chips linked at large scale.

## Export controls and tariffs: policy is now a supply variable

A product that can be sourced through one channel today can face a licence requirement or a tariff tomorrow, so compliance has become part of supply-chain risk.

- 31 May 2026 — Expanded Export Licensing Requirements

- Action: The U.S. Department of Commerce’s Bureau of Industry and Security (BIS) issued guidance requiring licences for advanced AI chips shipped to companies whose ultimate parent is based in China, regardless of the subsidiary’s location.
- Why it matters to buyers:
- Overseas subsidiaries can no longer automatically bypass restrictions.
- Ultimate-parent ownership checks are now critical.
- Buyers must assess corporate ownership and supply-chain compliance before placing orders.

- 14–15 January 2026 — Changes to Advanced Chip Export Policy

- Action: Export policy for NVIDIA H200 and AMD MI325X-class chips destined for China shifted from a presumption of denial to case-by-case review.
- A Section 232 proclamation introduced a 25% tariff on certain advanced chips, including the H200 and MI325X, with broad exemptions for U.S. use.
- Broader tariffs and an offset programme for U.S. investment were also flagged.
- Why it matters to buyers:
- Licensing and tariff exposure can vary by product and destination.
- Buyers must monitor policy changes before committing to procurement.
- A second phase of tariffs could expand the scope of affected products.

- 2022 Onward — Expansion of Semiconductor Export Controls

- Action: U.S. export controls expanded beyond finished chips to include:
- Semiconductor manufacturing equipment.
- Chip design software.
- Advanced memory technologies.
- Specific Chinese companies and entities.
- Why it matters to buyers:
- A single AI chip may involve design, manufacturing, packaging, and distribution across four or more countries.
- Each stage can introduce separate regulatory and compliance risks.
- Procurement teams must evaluate the entire supply chain rather than just the chip’s country of origin.

Commerce was due to report by 1 July 2026 on the market for data centre chips to inform tariff changes. We could not confirm the outcome, so check the current position before pricing a cross-border deal.

## The unintended effect

Controls slow China’s access to specific technologies and also accelerate domestic substitution, as the Huawei and SMIC section shows. The Netherlands expanded its own equipment controls in early 2025, so the restrictions are now multilateral.

## What buyers should do

• Screen the end user and the ultimate parent, not just the shipping address.

• Record where each critical component was designed, made and packaged.

• Build tariff and licence contingencies into contracts, including who bears the cost of a mid-order change.

• Expect separate sourcing strategies for different geographic markets.

## The rest of the world: Japan, the Netherlands, Southeast Asia, India, Europe and the Gulf

Several countries outside the big four hold chokepoints or are becoming the largest buyers, and the Gulf is the fastest-moving of them.

• **Japan** supplies wafers, photoresists, chemicals and precision equipment that never appear in the finished chip but are essential to making it, and it is investing again in domestic advanced manufacturing.

• **The Netherlands** is home to ASML, whose EUV lithography systems are required for leading-edge production, and it has expanded its own equipment export controls.

• **Malaysia and Singapore** are central to assembly, testing and packaging, logistics and regional investment. Moving advanced production elsewhere needs skilled labour, suppliers and customer commitments, not just factory space.

• **India** is building assembly, testing and electronics manufacturing through incentives and partnerships. A globally competitive advanced chip ecosystem will take years.

• **Europe** aims to protect strategic positions in equipment, industrial and automotive chips rather than compete in AI accelerators.

## The Gulf: from energy exporter to AI buyer

Capital, energy and national strategy are making the UAE and Saudi Arabia major deployment markets, though they will rely on imported accelerators, memory and servers.

• **UAE:** Stargate UAE, a project involving OpenAI and G42 in Abu Dhabi, targets 200 MW in its first phase by the end of 2026 and up to 1 GW over time.

• **Saudi Arabia:** Humain, backed by the Public Investment Fund, has a target of 1.9 GW of AI data centre capacity by 2030. Reports in 2026 describe plans for up to 600,000 NVIDIA chips over three years, alongside a parallel 500 MW of AMD and Cisco-based capacity.

• **Demand signal:** NVIDIA management said sovereign customer revenue would exceed $20 billion in 2025, more than double 2024, according to a Goldman Sachs note.

The Gulf is a buyer, investor and operator. For distributors, integrators and suppliers, that makes it one of the clearest growth markets, but also one where export licences, end-user rules and power availability decide timelines.

## Power, cooling and the grid: the constraint beyond the chip

For many AI projects in 2026 the limiting factor is not silicon but megawatts, and hardware that arrives without power and cooling sits idle.

## The scale of the demand

• The International Energy Agency projects data centre electricity use rising from 415 TWh in 2024 to 945 TWh by 2030.

• TrendForce projects that the top North American cloud providers’ combined server power consumption will rise by about 18 GW in 2026 as GB300, AMD Helios and custom ASIC racks deploy at scale.

• An industry report cited in June 2026 estimated that AI-optimised servers would account for about 31% of data centre power use in 2026 and exceed conventional servers by 2027.

## Density breaks air cooling

Legacy racks drew roughly 5–10 kW. Current AI racks draw 50–100 kW or more, and TrendForce notes that the newest NVIDIA, AMD and cloud-ASIC rack platforms all use liquid cooling, which also shrinks server height so a rack can hold more accelerators. Liquid cooling moves heat more efficiently, but it does not reduce the watts going into the chips. Prefabricated power and cooling modules from suppliers such as Schneider Electric, Eaton and Vertiv are one response.

## The grid is slower than the order book

One industry analysis estimates that grid interconnection can take five to ten years in some markets, against about two years to build the facility itself. Developers are responding with on-site generation, energy storage and grid-independent power, an approach Turner & Townsend has recommended for AI facilities. Projects without secured power are the ones most exposed to delay.

## What it means for procurement

1. **Check power and cooling readiness before ordering racks.** A server that cannot be energised is a stranded asset.

2. **Compare platforms on performance per watt, not just price per accelerator,** because power is now a major part of lifetime cost.

3. **Treat power and cooling equipment as long-lead items,** alongside memory and optics.

4. **Ask suppliers whether liquid cooling, manifolds and power distribution units are included in the quote** and when they ship.

A useful working rule: in 2026 a delivery date for a rack is only as reliable as the delivery date for its power.

## Networking and optics: moving data is the next bottleneck

A GPU that waits for data is wasted capital, so networking has become one of the fastest-growing parts of the AI bill of materials.

AI clusters need two kinds of networking. Scale-up links connect accelerators and memory inside a tightly coupled system, such as NVIDIA’s NVLink. Scale-out links connect servers across a cluster using InfiniBand or increasingly capable Ethernet. The contest between them is less about protocol than about reliability, interoperability and cost at scale.

As bandwidth needs rise and clusters spread across more racks, optical connections replace copper over more distances. TrendForce projected that the AI-focused optical transceiver market would grow from $16.5 billion in 2025 to $26 billion in 2026, growth of more than 57%. Goldman Sachs Research has identified pluggable optics and co-packaged optics as major emerging AI infrastructure opportunities.

Optics can become the next supply bottleneck, because high-speed transceivers depend on specialised lasers, photonic devices, packaging and testing that cannot be scaled overnight. For buyers, the practical rule is to specify networking when the server is specified. Ordering racks first and switches or transceivers later is how a cluster ends up delivered but unusable.

## Rack-scale systems, capex and who pays for it

The five largest US spenders are now funding AI infrastructure with debt and equity as well as cash flow, and investors are starting to ask for proof of payback.

## From servers to racks

AI is bought as complete racks. TrendForce expects global AI server shipments to grow more than 28% in 2026, and top-five North American cloud provider inference computing power to rise about 122% against about 56% for training. Racks combine accelerator trays, CPUs, memory, networking, power distribution and liquid cooling, so a failure in one part affects the whole. Taiwan’s ODMs are adapting to that: Wistron said AI infrastructure was 79% of its Q1 2026 revenue.

The Q3 2026 spending scorecard

- Microsoft

- Latest quarter:
- Capital expenditure: $45.2 billion, including leases.
- Azure revenue growth: 48%.
- Free cash flow: $18.6 billion.
- Outlook:
- Fiscal Q2 capex expected to exceed $50 billion.
- Approximately $170 billion in FY2027 capital expenditure.
- Priced $25 billion in bonds on 29 October.
- Market reaction: Mixed; shares moved within a few percentage points.

- Alphabet (Google)

- Latest quarter:
- Capital expenditure: Approximately $51.4 billion.
- Free cash flow: Approximately -$9.1 billion, marking the second consecutive negative quarter.
- Outlook:
- 2026 capex guidance: $195–205 billion (July guidance).
- Management signalled a significant increase in spending during 2027.
- Market reaction: Shares fell approximately 11–12%.

- Amazon

- Latest quarter:
- AWS revenue increased 39% to $46 billion.
- AWS operating income: $17.8 billion.
- Outlook:
- 2026 capital expenditure: Approximately $220 billion.
- Management projects AWS growth above 40% in 2027 and 2028.
- Market reaction: Overall financial results fell short of market expectations.

- Meta

- Latest quarter:
- Capital expenditure: Approximately $34.9 billion.
- Free cash flow: Approximately -$2.1 billion.
- Outlook:
- 2026 capital expenditure guidance: $135–145 billion.
- Market reaction: Mixed investor response.

In total, Microsoft, Alphabet, Amazon, Meta and Oracle spent about $556.9 billion on capital projects in the first nine months of 2026, according to 24/7 Wall St. Moody’s estimates sector capex of about $785 billion in 2026 and close to $1 trillion in 2027, and says hyperscaler debt issuance is running near $175 billion this year, against a five-year average of about $30 billion, as cited by BeInCrypto.

## Chipmakers are financing their own demand

TrendForce highlights a new pattern. NVIDIA has provided a residual value guarantee, with obligations capped at $105 billion, for the first 4.25 GW of an Ohio campus that OpenAI will lease, and it is the exclusive compute supplier. Google is guaranteeing data centre lease payments for Anthropic, up to $44 billion in total, in return for roughly 20% equity stakes in related projects, and Broadcom has committed to buy back chips if they cannot be sold. When suppliers underwrite customers, demand is secured, but risk is concentrated.

## Why this matters to buyers

The optimistic reading is that Azure and AWS growth shows demand is real. The cautious reading is that Microsoft’s capex, close to half its revenue, and Alphabet’s negative free cash flow leave little margin for error. Either way, the largest customers are securing supply for years ahead, and everyone else buys what is left.

## The allocation divide and the secondary market

Hyperscalers lock in supply years ahead, which leaves smaller buyers with longer lead times, and that is creating a real market in used and refurbished equipment.

## Who gets supply first

SK hynix has long-term agreements with 14 customers, and Samsung reportedly plans to commit about 70% of its memory capacity to long-term contracts. A European distributor’s 2026 sourcing guide describes lead times for HBM4 and advanced DRAM stretching from 8–10 weeks in 2024 to more than 20–30 weeks in 2026, with allocation-only ordering. That figure is one supplier’s estimate rather than an industry statistic, but the direction matches what manufacturers say publicly.

A hyperscaler buying thousands of AI servers and an enterprise buying 20 servers may approach the same manufacturer. Manufacturers rationally favour the strategic customer when capacity is short, so smaller buyers face different pricing, lead times and availability.

## Why used hardware has a role

Not every workload needs the newest accelerator. Previous-generation GPUs remain useful for inference, fine-tuning smaller models, research, internal enterprise AI and GPU cloud services. One GPU-cloud analysis notes that A100 and L40S capacity is broadly available, and that older Hopper parts should return to the general market through 2027 as hyperscalers move to Blackwell. In July 2026, SiliconANGLE reported the launch of a dedicated secondary marketplace for used NVIDIA H100 and A100 GPUs, Compute Exchange.

The lifecycle runs as follows: new accelerators enter hyperscale data centres, previous-generation systems are redeployed or retired, qualified systems move to secondary channels, and GPU clouds, smaller AI firms and enterprises buy them for less demanding work.

## The risks

A used GPU is not automatically a bargain. Buyers should verify:

• Operating hours, thermal history and previous workload intensity

• Firmware, BIOS and memory health

• Cooling system condition and power consumption

• Warranty, replacement parts and manufacturer support status

• Software compatibility with the target environment

Buy complete validated servers rather than loose accelerators, since a working GPU can still be incompatible with a given motherboard, power design or cooling setup. Refurbished servers, storage arrays, switches and enterprise SSDs suit development, disaster recovery, backup and branch-office use, with extra attention to firmware support and security updates.

## Emerging markets

For businesses in Pakistan, the UAE, Saudi Arabia and other regional markets, a tested, warranted previous-generation system can deliver a better return than scarce new hardware. The condition is a reputable supplier with transparent testing and after-sales support.

## Outlook 2027–2029: three scenarios and what to watch

Supply pressure looks set to persist through 2027 and probably 2028, but the shape of 2029 depends on whether AI revenue catches up with AI spending. The scenarios below are analytical expectations, not forecasts of record.

## What suppliers and analysts expect

• **Memory:** Samsung and Micron expect supply to stay tight through 2028. SK hynix says DRAM and HBM are effectively sold out through 2029 and, per press reports, expects shortages to run through 2030. IDC’s July assessment expected constraints across several AI infrastructure components through 2027, with memory pressure possibly into 2028.

• **DRAM and NAND diverge:** TrendForce expects DRAM to stay structurally tight in 2027 while NAND could ease in the second half.

• **Rubin:** TrendForce expects VR200 racks to lead NVIDIA shipments in 2027, with NVL72 rack shipments up more than 50%.

• **Custom silicon:** ASIC share is forecast to keep rising, with a path towards about 40% by 2030.

## Scenarios

- Bullish AI Expansion

- What it looks like (2027–2029):
- Hyperscalers continue aggressive infrastructure spending.
- Memory, advanced packaging, and power infrastructure remain in short supply.
- Hardware prices stay elevated due to sustained demand and limited capacity.
- Signals to watch:
- Hyperscaler 2027 capex guidance exceeds 2026 levels.
- New long-term memory supply agreements emerge.
- NVIDIA Rubin platform shipments increase.

- Balanced Normalisation

- What it looks like (2027–2029):
- New manufacturing and infrastructure capacity improves hardware availability.
- AI demand remains strong but supply constraints gradually ease.
- Hardware prices become more predictable.
- Signals to watch:
- NAND supply conditions ease in the second half of 2027.
- CoWoS advanced packaging capacity expands.
- HBM production yields improve.

- Demand Correction

- What it looks like (2027–2029):
- AI infrastructure returns fall short of expectations.
- Hyperscalers delay or scale back infrastructure projects.
- Inventory levels increase, putting downward pressure on hardware prices.
- Signals to watch:
- Hyperscalers continue reporting negative free cash flow.
- Cloud revenue growth weakens.
- AI infrastructure projects are cancelled or delayed.
- Credit conditions tighten, making infrastructure financing more expensive.

Do not ignore the third scenario. Alphabet’s negative free cash flow and the jump in hyperscaler borrowing show that investors are now asking for returns on AI spending. Semiconductors have always run in investment cycles.

## Calendar: the next 90 days

1. **Around 18 November:** NVIDIA’s fiscal Q3 results (date unconfirmed), including guidance on Rubin ramp and supply.

2. **Through Q4:** Contract-price settlements for DRAM and NAND against TrendForce’s forecasts.

3. **Q4 2026:** Huawei’s Ascend 950DT and Atlas 950 SuperPoD launches.

4. **Q4 results and 2027 guidance:** TSMC (January), and hyperscaler 2027 capex plans (January-February).

5. **Any time:** Phase two of the Section 232 chip tariffs and further export-control guidance.

Plan on multiple scenarios rather than assuming demand rises indefinitely.

## How enterprise procurement must change

Procurement built around annual price comparisons no longer fits a market where memory prices reset quarterly and supply is allocated, so buyers need a component-aware, scenario-based approach.

1. **Budget for price movement, not last year’s price.** Q4 2026 contract-price forecasts are up 10–15% for conventional DRAM and 15–20% for NAND on the quarter. A budget built on a quote from two quarters ago is already wrong.

2. **Shorten quote cycles and fix the terms.** Agree quote validity, price-adjustment conditions, confirmed inventory, delivery commitments, substitution rules, payment schedules and cancellation terms. A quote that outlives an approval process is a quote that will be repriced.

3. **Demand component-level visibility.** A server in stock is not a server that can ship if the memory configuration or SSD model is missing. Ask for the bill of materials and confirmed stock for critical parts.

4. **Treat memory, optics, power and cooling as separate risk items.** Keep a risk register for specific DRAM configurations, enterprise SSD models, switches, transceivers, power distribution and cooling.

5. **Qualify multiple suppliers, then test their dependencies.** Three vendors do not give resilience if all three depend on the same constrained component source.

6. **Share forecasts early and consider framework agreements.** Communicating likely demand does not mean buying everything now. For larger projects, reserved allocations and scheduled purchasing can secure position.

7. **Match hardware to the workload.** Start from what the workload needs: training, inference, real-time data, latency sensitivity, or storage-heavy retrieval. Previous-generation or refurbished systems can serve non-critical workloads, as long as they are tested against real application requirements.

8. **Compare total cost of ownership.** Include power, cooling, software licences, warranty, spares, lifecycle and replacement. A server that costs 10% less but draws substantially more power can cost more over its life.

9. **Add geopolitical and compliance checks to supplier evaluation.** Screen the end user and ultimate parent, understand where critical parts are made and packaged, and decide who bears tariff or licence changes.

10. **Bring procurement in at the start.** Involve IT, infrastructure, finance, security, legal and vendors before a platform is chosen, not after.

The objective is not to buy inferior equipment. It is to avoid paying for performance the workload does not need, and to avoid a deployment that stalls for lack of one small component.

## What this means for hardware distributors and suppliers

In a market where supply, not price, decides who wins the order, distributors that compete only on price lose ground to those that can prove availability and compatibility.

• **Supply-chain expertise becomes the product.** Reliable sourcing, accurate delivery information and compatible configurations are worth more than a slightly lower quote.

• **Customers need architecture guidance.** A buyer asking for an AI server often has not planned for power, cooling, memory, networking and storage. Helping them size the system before they commit is a value-added service.

• **After-sales support differentiates.** Warranty management, spares, configuration validation, testing and lifecycle support matter more as systems get more complex.

• **Refurbished and secondary-market channels are a real category.** Success depends on transparent grading, reliable testing, warranty coverage and clear disclosure of equipment history.

• **Regional demand is growing.** Gulf and emerging-market buyers want access to advanced infrastructure on constrained budgets, and trusted intermediaries are scarce.

## Frequently asked questions

## Is there still an AI chip shortage in 2026?

Yes, but the tightest constraints have moved. Packaging is easing slowly, while HBM4, server DRAM and power are now the hardest inputs to secure. TSMC says CoWoS demand still exceeds supply, and SK hynix says DRAM and HBM are effectively sold out through 2029.

## When will the memory shortage end?

Suppliers disagree on timing, but none expects relief in 2027. Samsung and Micron expect tightness through 2028. SK hynix says supply is sold out through 2029 and, per press reports, sees shortages lasting through 2030. TrendForce expects NAND to ease before DRAM.

## Who makes HBM?

SK hynix, Samsung and Micron. One 2026 estimate put SK hynix at about half of HBM revenue and Samsung at about a third in Q2, with Micron third. Estimates vary by source.

## What is CoWoS and why does it matter?

CoWoS (Chip on Wafer on Substrate) is TSMC’s advanced packaging platform that joins logic chips and HBM stacks into one accelerator. A chip cannot ship without it, so packaging capacity limits accelerator output even when wafers are available.

## Why are DRAM and SSD prices rising for ordinary servers and PCs?

Memory makers are moving wafer capacity to HBM and server products for AI, which leaves less for everything else. TrendForce forecasts Q4 2026 contract prices up 10–15% for conventional DRAM and 15–20% for NAND.

## Is China building its own AI chips?

Yes. Huawei’s Ascend 950 line is now the main procurement target for China’s large technology companies, and Huawei expects about $12 billion of AI chip revenue in 2026. The limits are SMIC’s non-EUV process, long cycle times, packaging capacity and HBM.

## How do export controls and tariffs affect buyers?

Licence rules now reach companies whose ultimate parent is in China, wherever they are located, and a 25% Section 232 tariff applies to certain advanced chips, with broader tariffs possible. Buyers should screen end users and ultimate parents and agree who bears the cost of rule changes.

## Are used H100 and A100 GPUs worth buying?

Often, for inference, fine-tuning smaller models and development, provided the hardware is tested, warranted and sold as a validated system. Check operating history, thermals, firmware, cooling and power compatibility.

## Will AI hardware prices fall in 2027?

Not for DRAM, which TrendForce expects to remain structurally tight. NAND may ease in the second half of 2027. A demand correction is possible, but the major memory suppliers are not guiding to one.

## Conclusion: the next AI winner may not have the fastest chip

The first phase of the AI hardware race was about building the most powerful accelerator. The current phase is about who can secure memory, packaging, power and capital, and deliver complete systems at scale.

NVIDIA is shipping Rubin, AMD is pursuing a second-source position, hyperscalers are expanding custom silicon, and China is building a parallel stack. None of this removes the need for a functioning international supply chain. The binding constraint may be HBM4, a CoWoS slot, an enterprise SSD, an optical transceiver, a liquid-cooling manifold or a grid connection, and it can differ by project.

For enterprise buyers, hardware procurement is no longer routine purchasing. It requires an understanding of the full supply chain, multiple sourcing options, scenario-based budgets and infrastructure plans built around workload economics. For distributors, the opportunity lies in combining availability, technical knowledge, reliable sourcing, compatibility and after-sales support.

The companies and countries that control the scarcest layers of this ecosystem will shape the next generation of technology. For ongoing analysis of AI infrastructure, enterprise hardware and procurement, follow Direct Macro at DirectMacro.com and on LinkedIn.

## Sources and methodology

Research cutoff: 31 October 2026. Reported results are company-reported figures as cited by the publications below. Forecasts, including TrendForce price and shipment outlooks, are projections and not final results. Estimates for CoWoS capacity and HBM market share come from trackers and analysts, not from the companies. Figures marked as reported should be checked against the primary filing or release before being relied on commercially.

**Memory and storage**

• SK hynix Q3 2026 results (TrendForce) and Korea Times

• Samsung Q3 2026 preliminary results (TrendForce) and Samsung profit analysis (Xenospectrum)

• TrendForce Q4 2026 memory price outlook (IT Home), TrendForce DRAM bulletin and DRAM and NAND quarterly tracker (NextMSC)

• AMD, Samsung and HBM4 supply (Coinpaper) and HBM4 supply shares (Tech Insider)

**Accelerators, servers and custom silicon**

• Custom ASIC market (Tom’s Hardware), ASIC share data (INDmoney) and Broadcom TPU capacity (Tom’s Hardware)

**Foundry and packaging**

• TSMC Q3 2026 call summary (Benzinga via Webull), TSMC Q3 analysis (Odaily) and Focus Taiwan

**Hyperscaler spending and financing**

• Big Tech capex and funding (24/7 Wall St.) and Meta guidance (24/7 Wall St.)

• Meta Q3 summary (Bloomberg via AI Weekly) and Moody’s estimates (BeInCrypto)

**China and policy**

• Huawei AI chip revenue (Tom’s Hardware), Ascend roadmap (RCR Wireless) and DeepSeek V4 on Ascend (DEV)

• Section 232 chip tariffs (GHY) and Section 232 analysis (Mondaq)

**Power, sovereign AI and sourcing**

• Data centre power wall (COR Advisors), IEA projections (AI CERTs) and power and supply chain strain (The Register)

• Stargate UAE (The Decoder), Humain build-out (RCR Wireless) and Humain GPU plans (AI in Arabia)

• Rubin component lead times (GlobX) and GPU availability (Spheron)

**Carried over from the previous draft and not re-checked in this revision:** TrendForce’s September enterprise SSD demand projection and optical transceiver forecast, IDC’s Q2 2026 enterprise storage figures and 2027 outlook, Goldman Sachs Research on optics, Focus Taiwan on Wistron, TSMC’s 2025 North America Technology Symposium on CoWoS roadmap, NVIDIA’s May 2026 Taiwan partner account, Reuters on CXMT HBM3E, CSIS and CEIAS assessments, Digital Today on Korea’s AI factories, SiliconANGLE on Compute Exchange, and Deloitte inference projections. Please verify these against the original publications before publishing.
