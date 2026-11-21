---
id: collect-261121-veille-ia/hardware-semi/intel-forecasts-14a-within-5-of-tsmc-s-a14-despite-18a-clock-speed-lead
title: "Intel forecasts 14A within 5% of TSMC's A14 despite 18A clock-speed lead"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["intel", "tsmc", "process", "clock", "power", "lead", "high", "performance"]
source: https://news.lavx.hu/article/intel-forecasts-14a-within-5-of-tsmc-s-a14-despite-18a-clock-speed-lead
source_anchor: ""
source_lines: [1, 67]
section: "Hardware_Semi"
---
Intel expects its 14A process to land within 5% of TSMC's A14, a cautious forecast that falls below the advantage suggested by both companies' published performance targets and current CPU clock data.

Intel expects its 14A process to deliver performance within 5% of TSMC's A14, according to Naga Chandrasekaran, Intel's chief technology and operations officer and general manager of Intel Foundry. The forecast places the two 1.4-nanometer-class nodes near parity, though the wording leaves open which company will lead.

That estimate appears cautious beside Intel's published targets for 14A and TSMC's projections for A14. Current processor clock speeds also give Intel's 18A process a strong starting position against TSMC's N2 node.

*Image credit: Intel*

## Intel and TSMC publish different performance targets

Intel expects 14A to deliver 15% to 20% more performance than 18A at the same power. At the same frequency and transistor count, Intel projects a 25% to 35% power reduction. The company's foundry process roadmap places 14A after 18A and adds backside power delivery to the process technology mix.

TSMC projects A14 to deliver 10% to 15% more performance than N2 at the same power. The company expects a 25% to 30% power reduction at the same clock speed and transistor count. TSMC describes its process technologies on its official technology page.

Those figures compare each company's next node with its own previous node. They do not form a direct 14A-versus-A14 test. The comparison requires a common reference point, such as a processor clock produced with each process.

## Current clocks favor Intel's 18A starting point

Intel's Core Ultra X9 388H, built with 18A, can reach 5.10 GHz with an 80-watt maximum turbo power rating. AMD's EPYC 9586F, which represents one of the fastest available N2-based designs, reaches 5.0 GHz for a single core at a 500-watt default CPU power rating.

Apple's A20 Pro reaches 4.93 GHz, while the M6 reaches 4.78 GHz. Those chips offer additional N2 reference points, though their designs target different power envelopes and workloads.

Clock speed alone cannot establish a process node's full performance. Architecture, voltage, cache design, standard-cell libraries, thermal limits and physical layout all affect the result. A CPU with a higher clock may still deliver less work per cycle than a rival chip.

The clock data still offers a useful market reference. Available N2 processors have not shown a clear frequency lead over 18A. Intel's 5.10 GHz result sits above the 5.0 GHz EPYC figure and the clock speeds reported for Apple's chips.

## What the published figures imply

Using Intel's 5.10 GHz 18A result and AMD's 5.0 GHz N2 result, the company projections produce the following estimates:

| Scenario | Intel 14A gain | TSMC A14 gain | 14A estimate | A14 estimate | Implied 14A lead | 
|---|---|---|---|---|---|
| Intel low, TSMC high | 15% | 15% | 5.865 GHz | 5.75 GHz | 2.0% | 
| Both at low end | 15% | 10% | 5.865 GHz | 5.50 GHz | 6.6% | 
| Both at high end | 20% | 15% | 6.12 GHz | 5.75 GHz | 6.4% | 
| Intel high, TSMC low | 20% | 10% | 6.12 GHz | 5.50 GHz | 11.3% | 

The calculation gives Intel 14A a projected advantage of 2% to 11.3% over A14. The smallest gap appears when Intel reaches the low end of its range and TSMC reaches the high end of its range.

Apple's 4.78 GHz M6 provides a second N2 reference. Applying the same process gains produces a wider gap:

| Scenario | Intel 14A gain | TSMC A14 gain | 14A estimate | A14 estimate | Implied 14A lead | 
|---|---|---|---|---|---|
| Intel low, TSMC high | 15% | 15% | 5.865 GHz | 5.506 GHz | 6.5% | 
| Both at low end | 15% | 10% | 5.865 GHz | 5.267 GHz | 11.4% | 
| Both at high end | 20% | 15% | 6.12 GHz | 5.506 GHz | 11.2% | 
| Intel high, TSMC low | 20% | 10% | 6.12 GHz | 5.267 GHz | 16.2% | 

This second comparison places 14A 6.5% to 16.2% ahead of A14 under the stated assumptions. The figures illustrate the relationship between the companies' claims and existing products. They do not predict the clock speeds of future processors.

## Why Intel may be using a narrower forecast

Intel's within-5% estimate may include factors that a clock-based calculation misses. A process node can offer strong transistor-level performance while a product team chooses a lower clock to control heat, improve battery life or meet a specific package limit.

Design teams also select different libraries for mobile chips, server CPUs and client processors. Those choices affect density, voltage and frequency. A node that performs well in a high-performance CPU may produce a different result in a compact mobile system.

Manufacturing maturity could also affect the comparison. Intel and TSMC will need to ramp new equipment, improve yields and qualify customers before either node reaches broad production. Defect rates, wafer output and packaging capacity can change the commercial value of a process even when transistor performance looks strong.

Intel's own forecast may also reflect a cautious planning position. The company has spent years rebuilding its process pipeline and expanding its foundry business. A narrow gap gives Intel room to meet its public target across several product categories, even if one early chip favors TSMC or Intel by a wider margin.

The commercial test will arrive with shipping products. Engineers will need to compare processors built with 14A and A14 at matched power, frequency, transistor count and workload. Until those systems reach customers, the published figures support a possible Intel lead, while Chandrasekaran's statement sets a much tighter expectation.

Source:tomshardware.com

## Comments

Please log in or register to join the discussion
