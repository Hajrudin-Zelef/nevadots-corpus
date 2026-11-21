---
id: collect-261121-veille-ia/hardware-semi/intel-14a-pdk-0-9-milestone-and-2027-roadmap
title: "Intel 14A PDK 0.9 Milestone and 2027 Roadmap"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["intel", "production", "development", "suppliers", "silicon", "power", "process", "equipment"]
source: https://diesignal.com/articles/intel-14a-hits-pdk-0-9-milestone-pdk-1-0-set-for-q1
source_anchor: ""
source_lines: [1, 57]
section: "Hardware_Semi"
---
## Spec summary

1. Intel released 14A PDK 0.9 last week, based on actual silicon measurements
2. PDK 1.0, validating HVM readiness, is due in Q1 2027
3. Intel targets 14A risk production in H2 2027 and mass production in 2028
4. 14A adds RibbonFET 2 GAA transistors, PowerDirect backside power, and Turbo Cells
5. Cadence, Keysight, Siemens, and Synopsys are collaborating or certified on 14A

Intel Foundry last week released version 0.9 of its Intel 14A process design kit, and the company plans to ship PDK 1.0 in the first quarter of 2027. That later release will validate readiness for high-volume manufacturing — but don't confuse it with a launch date. Intel itself expects to start mass production on 14A in 2028, following risk production of its own products in the second half of 2027.

A PDK bundles transistor models, design rules, and the other data chip designers need to build products for a given process using EDA tools. Its accuracy determines how well a design's simulated behavior matches manufactured silicon. Version 0.9 matters because it's based on actual silicon measurements rather than theoretical models, so customers can start product development with more confidence on performance and power targets.

Intel says the kit lets customers develop designs aimed at risk production around the time the process is certified. If the PDK reflects real silicon closely, designers can cut the number of silicon revisions between initial tapeout and mass production — revisions that cost months per spin.

## What does the new node actually change?

Intel 14A builds on the architecture introduced with 18A and adds three substantial pieces:

- **2nd Generation RibbonFET 2** gate-all-around (GAA) transistors
- **PowerDirect** backside power delivery, which feeds power straight to transistor source/drain regions without intermediate connections like the TSVs PowerVia uses
- **Turbo Cells** , high-performance standard cells for accelerating timing-critical paths in CPUs and GPUs

The announcement contains no updated figures for transistor performance, power consumption, or density. Intel outlined the technologies' advantages earlier and hasn't quantified them since.

## How is Intel speeding up maturation?

The company is leaning on twelve months of Intel 18A production experience. It has increased investment in inline defect detection, modeling, and simulation to catch manufacturing problems earlier in the development cycle, with the aim of improving yields, process variability, transistor performance, and production cycle times. Intel also says it has launched several strategic initiatives to accelerate development, without quantifying their impact.

"From a customer perspective, the PDK 0.9 gives customers the confidence to design products on Intel 14A, as the technology is reaching the industry-standard performance and yield maturity expected at this stage of development, supported by silicon-backed validation," said Ben Sell, corporate vice president and general manager of front-end technology development at Intel Foundry.

Intel has also reworked how it works with equipment and materials suppliers. Technology development teams and external suppliers now get involved earlier in the process development cycle, so defects and equipment shortcomings are fixed before they block yield improvements in mass production. Intel also runs regular yield summit meetings with suppliers.

"This has required evolving our collaboration model with equipment and material suppliers, and earlier engagement from our [technology development] leaders with suppliers to drive improvements in defects and equipment capabilities," Sell said.

There's a strategic bonus: suppliers may bake Intel's requirements into standard products and roadmaps. Competitors would then have to adapt their processes to Intel-optimized equipment or develop alternatives from scratch. Intel historically set the direction for semiconductor manufacturing — immersion DUV lithography, where TSMC led and Intel followed, is the notable exception — and early supplier engagement could restore some of that influence, though its importance shouldn't be overstated.

## Who's supporting the ecosystem?

Cadence and Intel have set up a design-technology co-optimization collaboration focused on next-generation AI-driven design tools and IP. Keysight, Siemens, and Synopsys have announced design-tool certifications for 14A, and Intel is working with other software developers and IP providers to validate design flows and reusable building blocks for prospective foundry customers.

Customer names remain absent — no commitments, tapeouts, or manufacturing agreements have been disclosed, which is normal for the foundry industry. But external customers are critical: leading-edge process development only pays off when the technology makes hundreds of millions of chips, far more than Intel ships today. Foundry success also depends on predictable schedules, not just transistor performance and yields.

"Our next major milestone is the Intel 14A PDK 1.0, expected in the first quarter of 2027," said Ali Farhang, corporate vice president and general manager of Intel Foundry's design technology platform organization. "I am looking forward to sustaining this level of quality across future releases and continuing to earn our customers' trust by delivering on our commitments with a customer-first mindset."

Looking ahead, Intel will keep working on 14A's performance, yields, variability, and cycle times, though it hasn't disclosed concrete targets. Derivative 14A-E and 14A-P nodes with additional enhancements and performance refinements are planned after the initial technology lands, with no schedules announced yet.

via Tom's Hardware (Source)

## More from Elena Vasquez

## Same lot · LOT-360A

- DSG-4396*5nm* Intel Lands Major Foundry Customer as 18A Output Beats Targets
- DSG-1694*14nm* Intel Links AI Chipmaking Push to Faster Next-Gen Process Rollout
- DSG-5346*3nm* Intel Launches Next-Gen PC Chip at CES in Las Vegas
- DSG-6144*10nm* Synopsys, TSMC extend design and IP support for AI silicon
- DSG-3801*28nm* Intel Sets 14A Parity Target Within 5% of TSMC's A14
