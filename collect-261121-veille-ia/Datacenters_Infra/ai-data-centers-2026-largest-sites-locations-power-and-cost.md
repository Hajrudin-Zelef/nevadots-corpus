---
id: collect-261121-veille-ia/datacenters-infra/ai-data-centers-2026-largest-sites-locations-power-and-cost
title: "AI Data Centers (2026): Largest Sites, Locations, Power and Cost"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: []
keywords: ["data", "power", "center", "about", "centers", "chips", "built", "cost"]
source: https://ai-magazine.com/ai-data-centers/
source_anchor: ""
source_lines: [1, 229]
section: "Datacenters_Infra"
---
# AI Data Centers: What They Are, Where They Are Being Built, and What They Cost

An AI data center is a data center built to train and run AI models. It holds thousands to hundreds of thousands of specialised chips, mostly Nvidia GPUs and Google TPUs, packed far more densely than in an ordinary data center, cooled with liquid instead of air and wired together so they can work as one machine. The largest now draw about a gigawatt each, as much electricity as a mid-sized city.

They are also the biggest building project in the US economy right now. Our analysis of US Census data shows that private construction spending on data centers reached an annual rate of $96.5 billion in September 2026 (preliminary), up 90% in a year and now more than twice what is spent on general office buildings. Our second analysis, of Epoch AIâs open dataset of 96 large AI data centers, shows their combined IT power growing from 2.0 gigawatts at the end of 2024 to 15.1 gigawatts today, with 38 gigawatts projected by the end of 2028.

This guide explains what makes these sites different, lists the largest ones, shows where they are being built and why, and covers what they cost, how much power they use and the fights they have started in 2026.

## AI data centers at a glance

| Question | Short answer | 
|---|---|
| What is an AI data center? | A data center built around AI chips (GPUs, TPUs and similar), with racks that can draw 100 kW or more, liquid cooling and very fast networking between chips | 
| How is it different from a normal data center? | The average data center rack draws about 8 kW. A current Nvidia AI rack draws about 120-140 kW, roughly 15 times more | 
| What is the largest AI data center? | By power, Amazonâs New Carlisle campus in Indiana, built for Anthropic (about 1.04 GW of IT power). By computing power, xAIâs Colossus 2 in Memphis, followed closely by OpenAIâs Stargate site in Abilene, Texas (Epoch AI estimates) | 
| Where are they being built? | Mostly in the US (84% of tracked capacity today), led by Texas. Counting sites planned by 2028, Wisconsin, Indiana, Ohio and New Mexico follow. Abroad: Malaysia and China today and, in plans, the UAE, Saudi Arabia, India and the EU | 
| How much does one cost? | Roughly $35-60 billion per gigawatt including chips, of which about two thirds is chips and servers | 
| How much power do they use? | US data centers used 4.7% of US electricity in 2024; Berkeley Lab projects 9.5-15.3% by 2030. A 1 GW campus running at full power all year uses about as much as 850,000 US homes | 
| Who owns the most AI computing power? | Google, with about a quarter of the worldâs AI compute at the end of 2025. Google, Microsoft, Amazon, Meta and Oracle together held 71% (Epoch AI; its provisional figures for early 2026 are higher) | 

## What is an AI data center?

All data centers do the same basic job: they house computers, feed them power, keep them cool and connect them to networks. What makes an AI data center different is the kind of computer inside and how hard it runs.

**The chips.** AI models are trained and run on accelerators: Nvidia GPUs such as the B200 and B300, Googleâs TPUs, Amazonâs Trainium and AMDâs Instinct chips. The newest Nvidia chips each draw over a kilowatt. A conventional data center is filled mostly with general-purpose servers running websites, databases and business software.

**The density.** Nvidiaâs current systems put 72 GPUs into a single rack. A GB200 NVL72 rack draws about 120 kW and a GB300 NVL72 about 135 kW, according to figures published by server makers. Nvidiaâs next generation, Vera Rubin, is reported at roughly 190-230 kW per rack. Compare that with an ordinary data center: the Uptime Instituteâs 2026 survey of more than 800 operators found the most common rack density was 11 kW and the average, excluding the densest facilities, about 7.8 kW. The International Energy Agency (IEA) estimates that AI server power density rose 11 times between 2020 and 2025 and will rise another 4 times by 2027 (IEA, Key questions on energy and AI).

**The cooling.** Above about 40-50 kW per rack, fans cannot move enough air. AI racks are cooled by liquid pumped through cold plates sitting directly on the chips. Nvidia says its Vera Rubin compute trays are fully liquid-cooled and can use water as warm as 45Â°C.

**The network.** Training a large model means splitting the work across tens or hundreds of thousands of chips that must exchange data constantly. AI data centers use two layers of networking: a very fast link between chips inside a rack (Nvidiaâs NVLink, Googleâs inter-chip links) and a second fabric between racks, using InfiniBand or specialised Ethernet. Microsoft now links two of its sites, in Wisconsin and Atlanta, over a dedicated network so they can work on a single training job.

**Training versus inference.** Training a frontier large language model needs one huge campus with as much cheap power as possible in one place. Inference, answering usersâ requests, can be spread across smaller sites closer to users. Most of the gigawatt campuses in this guide were built first for training, though all of them also serve inference.

**Efficiency.** The industry measures overhead with power usage effectiveness (PUE): total power divided by the power that reaches the computers. Uptimeâs 2026 survey puts the industry average at 1.52, meaning a third of the power goes on cooling and losses. Google reports 1.09 for its fleet in 2025 and Microsoft 1.17 (company figures).

|  | Conventional data center | AI data center | 
|---|---|---|
| Main chips | General-purpose CPUs | GPUs, TPUs and other AI accelerators | 
| Power per rack | About 8-11 kW on average | About 120-140 kW today, around 200 kW next | 
| Cooling | Mostly air | Liquid cooling to the chip | 
| Network | Standard Ethernet | Dedicated high-bandwidth fabrics linking every chip | 
| Typical large site | Tens of megawatts | Hundreds of megawatts to over 1 gigawatt | 

## How big AI data centers have become

There is no official register of AI data centers. The best public source is Epoch AIâs AI data centers dataset, which tracks 96 of the largest sites using satellite images, permits, company statements and filings. Epoch estimates that these sites hold about 44% of all the AI computing power in the world, with a wide margin of uncertainty. We downloaded the data on November 12, 2026 (licence CC BY 4.0) and added it up.

- **Capacity has grown 7.5 times in under two years.** The tracked sites had 2.0 GW of IT power (the power used by chips and servers, before cooling) at the end of 2024, 6.8 GW at the end of 2025 and 15.1 GW in November 2026.
- **There is far more to come.** Based on construction already under way, Epochâs timelines put the same sites at 25.0 GW by the end of 2027 and 38.2 GW by the end of 2028. These are projections, and some will slip.
- **The sites hold about 15.8 million H100-equivalents** , Epochâs unit for comparing different chips against Nvidiaâs 2022 flagship.

### The largest AI data centers

Ranked by IT power in operation now (Epoch AI estimates, November 2026):

| Site | Owner (main users) | Location | IT power now | Compute, H100-equivalents | 
|---|---|---|---|---|
| New Carlisle (Project Rainier) | Amazon (Anthropic) | Indiana, US | 1,037 MW | 818,000 | 
| Colossus 2 | xAI, now part of SpaceX (xAI, Anthropic, Cursor) | Memphis, Tennessee, US | 946 MW | 1,112,000 | 
| Stargate Abilene | Oracle (OpenAI) | Texas, US | 824 MW | 1,019,000 | 
| Fairwater Atlanta | Microsoft | Georgia, US | 636 MW | 769,000 | 
| Nusajaya | DayOne | Johor, Malaysia | 473 MW | 584,000 | 
| Prometheus | Meta | Ohio, US | 471 MW | 536,000 | 
| Kempas | DayOne | Johor, Malaysia | 422 MW | 481,000 | 
| Fairwater Wisconsin | Microsoft | Wisconsin, US | 369 MW | 446,000 | 
| Pryor (North) |  | Oklahoma, US | 368 MW | 637,000 | 
| Colossus 1 | xAI, now part of SpaceX (Anthropic) | Memphis, Tennessee, US | 340 MW | 276,000 | 

Two caveats. Epoch estimates power from visible cooling equipment and other evidence, so these figures can differ from what companies announce. And the labels change fast: Epoch lists Colossus 2âs owner as âSpaceXAIâ after xAI was merged into SpaceX, and Anthropic as one of its users.

### The largest AI data centers being built

By planned IT power on Epochâs timelines (projections):

- **Microsoft Fairwater, Wisconsin:** about 2.3 GW by 2028. Microsoft said its first facility there was fully operational in June 2026 and a second is under construction.
- **Amazon New Carlisle, Indiana:** about 1.9 GW by 2028.
- **OpenAI Stargate, New Mexico (DoÃ±a Ana County):** about 1.75 GW by the end of 2028.
- **Meta Hyperion, Louisiana:** about 1.6 GW by 2028. Meta has said the site will reach 2 GW by 2030 and could scale toward 5 GW.
- **Colossus 2, Tennessee:** about 1.5 GW by early 2027.
- **OpenAI Stargate, Shackelford County, Texas:** about 1.4 GW by late 2028.

## Where AI data centers are being built

The US dominates. Of the 96 sites Epoch tracks, 77 are in the US, with 84% of the capacity operating today and 86% of what is planned by 2028. Malaysia (8% of current capacity, almost all in Johor across the border from Singapore) and China (6%) are next. The dataset under-counts China, where Epoch has less visibility.

Within the US, our breakdown by state shows a clear shift:

- **Texas leads by a wide margin** , with 1.7 GW of IT power running today at nine sites and 6.5 GW planned by 2028 across 14 tracked sites, about a fifth of the US total.
- **New Mexico, Louisiana and Michigan** have little or nothing running now, but each has a giant campus under construction. Wisconsinâs 0.4 GW today is set to grow about ninefold by 2028.
- **Virginia falls from fifth to tenth.** Northern Virginia is still the largest single data center market in the world, holding about 11% of global hyperscale capacity according to Synergy Research. But among the large AI campuses in Epochâs data, it accounts for 8% of US capacity today and only 4% of what is planned for 2028.

### Why AI data centers go where they do

The first question for any gigawatt site in 2026 is where the power can come from, and how fast. Other factors follow:

- **Power.** Utilities in many regions cannot connect a new gigawatt load for years. Several of the largest sites, including Abilene, Shackelford and DoÃ±a Ana under the Stargate project, Colossus in Memphis and Metaâs Hyperion, rely partly on gas turbines built on or next to the site.
- **Land.** A gigawatt campus can need well over a thousand acres. Amazonâs New Carlisle site covers about 1,200.
- **Tax breaks.** Many states exempt data center equipment from sales tax, which matters when most of the cost is chips.
- **Fibre and water.** Both are needed, though closed-loop liquid cooling uses far less water than older evaporative systems.
- **Speed of permits.** Local approval can make or break a timeline, and opposition is rising (see below).

The scale of demand shows up in utility queues. In Virginia, Dominion Energy reports about 55 GW of data center capacity in various stages of contracting. Georgia Power reported a 76 GW pipeline of economic development projects, 73 GW of them large loads, in a filing earlier this year. Texas grid operator ERCOT has received large-load connection requests reported in the hundreds of gigawatts, far more than will be built.

### Outside the US

- **Malaysia:** Johor has three of the 15 largest AI sites in Epochâs data.
- **China:** official figures put the countryâs âintelligent computingâ capacity at 2,185 exaflops at the end of June 2026, up 177% in a year. Most is in eight national computing hubs.
- **UAE:** Stargate UAE, built by G42 with OpenAI, Oracle and Nvidia, plans 1 GW, with the first 200 MW targeted for 2026.
- **Saudi Arabia:** the state-backed company Humain is reported to target about 3 GW by 2030, up from an earlier goal of 1.9 GW.
- **India:** Google broke ground on a $15 billion AI hub of about 1 GW in Visakhapatnam in April 2026 (company announcement).
- **European Union:** the EUâs call for up to seven âAI gigafactoriesâ, backed by about â¬20 billion, closed on November 12, 2026, with first sites expected around mid-2028 (EuroHPC).

## OpenAI data centers

OpenAI owns little of its data center capacity directly. It rents capacity from partners, mainly Oracle, Microsoft and CoreWeave, under its Stargate programme and earlier deals. Its chief financial officer said in January 2026 that OpenAIâs compute grew from about 0.2 GW in 2023 to about 1.9 GW at the end of 2025 (company claim, as reported).

The US Stargate sites, as tracked by Epoch AI (Stargate: where the US sites stand):

| Site | Builder and operator | Planned power (total, incl. cooling) | Status | 
|---|---|---|---|
| Abilene, Texas | Crusoe, Oracle | About 1.2 GW | Operating; Oracle reported 75% delivered in September 2026 | 
| Shackelford County, Texas | Vantage, Oracle | About 2.0 GW | Under construction, due late 2028 | 
| DoÃ±a Ana County, New Mexico | STACK, Oracle | About 2.2 GW | Under construction, due late 2028 | 
| Milam County, Texas | SB Energy, SoftBank | About 1.2 GW | Under construction, due late 2028 | 
| Port Washington, Wisconsin | Vantage, Oracle | About 1.3 GW | Under construction, due late 2028 | 
| Saline Township, Michigan | Related Digital, Oracle | About 1.4 GW | Under construction, due late 2028 | 
| Lordstown, Ohio | SoftBank | Under 0.3 GW | Small; the town has since banned new data centers | 

OpenAI said in April 2026 that it had passed its 10 GW Stargate target, years ahead of its 2029 deadline. That figure counts contracted capacity, not capacity that is running. In March 2026 Bloomberg reported that OpenAI and Oracle had dropped a planned expansion next to the Abilene site, which Oracle disputed; the developer, Crusoe, is still building it, reportedly for another tenant. Outside the US, OpenAI is involved in Stargate UAE and a Stargate site in Norway.

## Google AI data centers

Google is the largest owner of AI computing power, and most of it runs on its own TPU chips rather than Nvidia GPUs. Epoch estimates Google held about 5.0 million H100-equivalents at the end of 2025, 24.8% of the worldâs AI compute (Epoch AI). Epochâs provisional figure for March 2026 is 31%.

Googleâs AI capacity is spread over many large sites rather than one giant campus. Epochâs data includes Pryor, Oklahoma (368 MW of IT power), New Albany and Columbus in Ohio (333 MW and 303 MW), Bristow in Virginia, Council Bluffs in Iowa, three sites in Nebraska, Mesa in Arizona and The Dalles in Oregon. Google has announced $40 billion for three new campuses in Texas through 2027 and the $15 billion hub in India. Its electricity use rose 37% in 2025, the largest annual jump it has reported (company figure).

## Who owns the most AI computing power?

| Company | Share of global AI compute, end of 2025 | 
|---|---|
|  | 24.8% | 
| Microsoft | 16.8% | 
| Amazon | 12.1% | 
| Meta | 11.3% | 
| Oracle | 5.6% | 
| Top five combined | 70.7% (62.5% in early 2024) | 

Source: Epoch AI estimates, in H100-equivalents. Its provisional figures for March 2026 put the top five at 75.8%. Ownership and use differ: Anthropic and OpenAI, two of the largest users, own little of the hardware they train on.

## How AI data centers are built

The US Census Bureau has tracked private construction spending on data centers since 2014. We took its monthly series (seasonally adjusted annual rate, Census C30) and compared it with other building types:

- **Spending grew 59 times** , from an annual rate of $1.6 billion in January 2014 to $96.5 billion in September 2026 (preliminary). It was $50.7 billion a year earlier.
- **Data centers have outspent general office buildings every month since September 2025** , after briefly overtaking them in April 2025. In September 2026 the US was spending $96.5 billion a year on data centers and $46.0 billion on general office buildings.
- **They are now 12.2% of all private non-residential construction** , up from 6.6% a year earlier and 0.5% in 2014. Without data centers, private non-residential construction fell 3.9% over the year.

These figures cover all data centers, not only AI sites, and only the buildings and their systems, not the chips and servers inside.

**How long building takes.** Epoch AI finds that reaching 1 GW takes one to four years from the start of construction. xAI aimed to build Colossus 2 in about 12 months, the fastest Epoch tracks. Stargate Abilene started construction in mid-2024 and its first building came online in September 2025.

**What slows it down.**

- **Grid connections** , which can take three to seven years in congested regions.
- **Gas turbines.** GE Vernova reported a backlog and reservations of 116 GW of gas turbines in mid-2026, and the IEA says turbine orders rose 70% in 2025.
- **Electrical equipment, memory chips and skilled labour.** Metaâs Louisiana site had about 3,700 workers early this year, as reported by the press.

## How much does an AI data center cost?

Estimates cluster around $35-60 billion per gigawatt of capacity, including chips:

- **Nvidia** has said a gigawatt of AI capacity costs $50-60 billion, of which its own products are a large share (company claim, 2025).
- **Morgan Stanley** puts an all-in US build with Nvidiaâs next-generation chips at about $49 billion per gigawatt (analyst estimate, reported).
- **Our calculation from Epochâs data:** the 96 tracked sites have an estimated capital cost of $573 billion for 15.1 GW of IT power, about $38 billion per gigawatt of IT power. Epochâs model puts about 69% of that in chips and servers and 31% in buildings, power and cooling.
- **The building alone** , without chips, is usually estimated at $11-18 billion per gigawatt.

So chips are most of the bill and they wear out fastest, typically depreciated over five or six years against decades for the building. That mismatch is one reason the financing of these projects worries some central banks, including the Bank of England. We cover the spending and the debt behind it in is AI a bubble?, including our analysis of how much of the big cloud companiesâ cash now goes on capital spending.

Running costs are large too. A 1 GW site running at full power all year uses 8.76 terawatt-hours of electricity. At $0.10 per kilowatt-hour that is about $0.9 billion a year in power alone (our arithmetic).

## How much power do AI data centers use?

Our guide to AI water and energy use covers this in depth, from a single prompt to company totals. The key numbers:

- **Worldwide:** data centers used about 485 TWh of electricity in 2025, up 17%, and the IEA projects about 950 TWh by 2030, close to 3% of global electricity. Consumption by AI-focused data centers rose 50% in 2025 (IEA).
- **United States:** data centers used 192 TWh in 2024, 4.7% of US electricity, according to Lawrence Berkeley National Laboratoryâs 2025 update, published in June 2026. Its reference case is 649 TWh by 2030, about 11.8%, within a range of 9.5% to 15.3% (LBNL).
- **One campus:** a 1 GW site running at full power all year uses about as much electricity as 850,000 average US homes (our arithmetic, using the US Energy Information Administrationâs 2024 average of about 10,400 kWh a year per household).
- **The grid in the Mid-Atlantic:** PJM, the grid operator covering 13 states and Washington, D.C., forecast about 32 GW of peak demand growth from 2024 to 2030, almost all of it from data centers; its 2026 forecast trimmed the near-term outlook slightly after screening utilitiesâ data center requests.

## Problems and controversies in 2026

- **Texas paused approvals.** On August 3, 2026, Governor Greg Abbott halted approval of new large data center grid connections pending audits, covering about 300 large projects, and in September extended the pause to state environmental permits (KERA News). Abbott lost the November 3 election to Democrat Gina Hinojosa (AP call; results are preliminary until certified). He remains governor until January 2027, so the stateâs policy after that is uncertain.
- **Local opposition is spreading.** Data Center Watch counted at least 75 projects worth about $130 billion blocked or delayed in the first quarter of 2026, roughly as much as in all of 2025, and at least 45 more, worth about $68 billion, in the second quarter (Data Center Watch). Local moratoria are in place in many towns and counties, including several in Georgia, and Lordstown, Ohio, has banned new data centers.
- **Electricity prices.** In PJMâs capacity auction for 2027/28, prices cleared at the maximum allowed, and its market monitor attributes billions of dollars of the cost to data center demand. Households in several states have seen bills rise.
- **Pollution and noise.** xAIâs gas turbines in Memphis and Southaven, Mississippi, face lawsuits from local and civil rights groups over air permits, as reported.
- **Cancellations.** Microsoft pulled out as the planned tenant of Nscaleâs 50 MW AI campus at Loughton in the UK after the local grid operator said full power would not arrive until the early-to-mid 2030s, as reported in October. In November 2026 Bloomberg and others reported that Microsoft had stepped back from several leases with smaller GPU cloud providers in the US, which Microsoft has not confirmed.

## AI data center stocks

Many readers search for âAI data center stocksâ. We do not give investment advice. The listed companies most exposed to the build-out fall into a few groups: chipmakers (Nvidia, Broadcom, AMD), the cloud companies that build and rent the capacity (Microsoft, Alphabet, Amazon, Meta, Oracle), specialised GPU cloud providers (CoreWeave, Nebius), suppliers of power and cooling equipment (GE Vernova, Vertiv, Schneider Electric) and data center property companies (Equinix, Digital Realty). Several of these fell sharply in the AI sell-off of November 2026, which we cover in is AI a bubble?.

## FAQ

### What is an AI data center?

It is a data center designed for AI chips, mainly GPUs and TPUs. Compared with a conventional data center, its racks draw roughly 15 times more power, it is cooled with liquid rather than air, and its chips are linked by very fast networks so they can train or run large AI models together.

### How many AI data centers are there?

There is no official count. Epoch AI tracks 96 large AI data centers that together hold about 44% of the worldâs AI computing power. Synergy Research counted about 1,360 hyperscale data centers of all kinds worldwide at the end of 2025, 580 of them in the US, many of which now run some AI work.

### What is the largest AI data center in the world?

By power in use, Amazonâs New Carlisle campus in Indiana, built for Anthropic, at about 1.04 GW of IT power. By computing power, xAIâs Colossus 2 in Memphis, with about 1.1 million H100-equivalents, just ahead of OpenAIâs Stargate Abilene in Texas. Larger campuses are being built, including Microsoftâs Fairwater in Wisconsin and Metaâs Hyperion in Louisiana.

### Where are AI data centers located?

Mostly in the United States. Texas has the most AI data center capacity, followed by Ohio, Tennessee, Indiana and Virginia for sites running today. Counting everything planned by 2028, Wisconsin, Indiana, Ohio and New Mexico come next after Texas. Outside the US, the largest clusters are in Johor in Malaysia and in China, with big projects planned in the UAE, Saudi Arabia, India and the EU.

### How much electricity does an AI data center use?

The largest use around 1 GW, which at full power over a year equals the electricity of about 850,000 US homes. All US data centers together used 4.7% of the countryâs electricity in 2024.

### How much does it cost to build an AI data center?

Roughly $35-60 billion per gigawatt, including the chips. The building, power and cooling alone are usually estimated at $11-18 billion per gigawatt.

Related: is AI a bubble?, how AI search is changing website traffic, what generative AI is, and small language models, which can run without a data center at all.
