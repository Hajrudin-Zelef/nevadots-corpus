---
id: collect-261121-veille-ia/hardware-semi/intel-14a-pdk-0-9-reaches-foundry-customers-and-it-s-built-on-measured-silicon-hardware-busters
title: "Intel 14A PDK 0.9 Reaches Foundry Customers, and It's Built on Measured Silicon - Hardware Busters"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["intel", "design", "foundry", "first", "silicon", "customers", "hardware", "measured"]
source: https://hwbusters.com/news/intel-14a-pdk-0-9-reaches-foundry-customers-and-its-built-on-measured-silicon/
source_anchor: ""
source_lines: [1, 24]
section: "Hardware_Semi"
---
**Intel Foundry has released version 0.9 of its Intel 14A PDK, the first build of the design kit grounded in measured silicon rather than models. It is the release many chip teams wait for before committing a real project to a node, and it puts 14A on a clock: PDK 1.0 in the first quarter of 2027.**


Intel Foundry has made the 0.9 release of its Intel 14A PDK (process design kit) generally available, according to a post on Intel’s newsroom dated November 2. Intel calls it the first silicon-validated version of the kit, and it moves 14A a step from roadmap slide toward something an outside customer can actually start designing on.

## Why a version number matters

A PDK is the bridge between a fab and the people who design chips for it. It bundles the transistor models, design rules and libraries that EDA tools need to place, route and verify a design on one specific process. Early releases lean heavily on simulation. By 0.9 the models are meant to reflect what the fab is really printing, which is why plenty of companies treat this release as the gate for starting a serious design.

Ben Sell, who runs front-end technology development at Intel Foundry, put it in exactly those terms. “From a customer perspective, the PDK 0.9 gives customers the confidence to design products on Intel 14A,” he said, adding that the node is reaching the performance and yield maturity expected at this stage. What Intel did not do is attach numbers to that claim. There is no defect density, no yield figure and no fresh performance target in the announcement.

## What is inside 14A

The node rests on three pieces of technology. RibbonFET 2 is Intel’s second-generation gate-all-around transistor, following the first one on 18A. PowerDirect is a new take on backside power delivery that feeds current straight to the transistor’s source and drain, skipping the intermediate connections that PowerVia uses on 18A. Turbo Cells are high-performance standard cells meant to speed up the timing-critical paths in CPU and GPU designs.

Intel says 14A folds in what it learned running 18A, backed by more investment in inline defect detection, modeling and simulation. Tom’s Hardware adds that Intel now brings suppliers in earlier and holds regular yield summits with them. On the tools side, Cadence, Keysight, Siemens and Synopsys have all announced certifications for the process, and Cadence runs a design-technology co-optimization program with Intel on 14A.

## The road from here

“Our next major milestone is the Intel 14A PDK 1.0, expected in the first quarter of 2027,” said Ali Farhang, who heads Intel Foundry’s design technology platform group. Per Tom’s Hardware, Intel’s own products are slated for risk production on 14A in the second half of 2027, with high-volume manufacturing following in 2028.

That schedule is the part to watch. 14A is the node Intel has openly tied to winning outside customers, and so far none has been announced, no external tape-out disclosed and no manufacturing deal signed. A 0.9 kit does not change that on its own, but it does remove one reason a prospective customer could give for waiting. It also arrives as TSMC lines up A14 for a similar 2028 window, so the two nodes will be judged side by side rather than on their roadmaps.

For PC buyers, the payoff is still a long way off. The client processors landing over the next year are built on earlier nodes. Intel 14A silicon in a desktop is a 2028-and-later conversation, and PDK 1.0 in a few months is the next checkpoint that tells us whether that date holds.
