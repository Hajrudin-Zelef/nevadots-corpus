---
id: collect-261121-veille-ia/samsung-mobile/raspberry-pi-5-16gb-reaches-385-as-samsung-ends-lpddr4-pi-6-still-eighteen-months-out
title: "Raspberry Pi 5 16GB Reaches $385 as Samsung Ends LPDDR4; Pi 6 Still Eighteen Months Out"
domain: samsung-mobile
role: reference
task: article
actors: ["Samsung"]
dates: []
keywords: ["raspberry", "lpddr4", "price", "samsung", "months", "same", "supply", "dram"]
source: https://www.techtimes.com/articles/329919/20261102/raspberry-pi-5-16gb-reaches-385-samsung-ends-lpddr4-pi-6-still-eighteen-months-out.htm
source_anchor: ""
source_lines: [1, 69]
section: "Samsung_Mobile"
---
The 16GB Raspberry Pi 5 now costs $385 — more than three times its $120 launch price — after the Raspberry Pi Foundation announced its fourth price increase on the 16GB model since January 2025, and its fifth price change on the line overall in ten months. What makes today's $80 jump different from the ones before it is not the magnitude, but the supply-chain reality it reflects: Samsung has formally discontinued LPDDR4 production entirely, the remaining manufacturers cannot fill the high-density 16GB gap, and Raspberry Pi's next-generation platform is not expected before early 2028. For anyone budgeting a 16GB Pi 5 project, this is not a temporary disruption — it is the new floor.

### Price History: A Timeline With No Plateau

The 16GB Pi 5 arrived in January 2025 at $120, a price point that positioned it as a credible workstation-class single-board computer for developers, educators, and embedded-systems engineers who needed serious RAM. That was twelve months ago. The table below tells the story since:

| Date | 16GB Pi 5 Price | Change | 
|---|---|---|
| January 2025 (launch) | $120 | — | 
| December 2025 | $145 | +$25 | 
| February 2026 | $205 | +$60 | 
| April 2026 | $305 | +$100 | 
| **November 2, 2026** | **$385** | **+$80** | 

The 16GB Raspberry Pi Compute Module 5 — used in industrial and commercial embedded products — also increased by $40 with today's announcement. In a notable carve-out, the Raspberry Pi 500+, the premium keyboard-integrated desktop that also uses 16GB of RAM, had its price held. The Foundation cited "a lack of alternative supply for 16GB DRAM, compounded by other ongoing component shortages" as the cause.

### Why Samsung's Exit Changes Everything

Every previous Raspberry Pi price announcement has carried the same message: LPDDR4 costs are up, but this is temporary; the Foundation expects to roll back increases once the market normalizes. That framing remains technically accurate but increasingly incomplete, because the supply structure behind LPDDR4 has changed in ways that make normalization far less predictable.

The Raspberry Pi 5 uses LPDDR4X-4267 memory — the same broad family of mobile DRAM that powers mid-range smartphones and IoT devices. For the 16GB variant, Micron supplies a single package containing eight 16Gbit dies stacked together, a configuration that only a handful of manufacturers can produce at volume. In April 2026, Samsung confirmed it had formally discontinued LPDDR4 and LPDDR4X production, with its manufacturing lines at the Hwaseong plant being converted to LPDDR5 and High Bandwidth Memory (HBM) output. SK Hynix wound down LPDDR4 volume production between the first and second quarters of 2026, as the same capacity shift took hold at its facilities.

That leaves Nanya Technology, Winbond, and the Chinese CXMT/GigaDevice partnership as the principal remaining LPDDR4 producers — none of which has demonstrated the high-density packaging capability needed for an eight-die 16GB stacked configuration at Raspberry Pi's required volumes. This is what Eben Upton means by "lack of alternative supply for 16GB DRAM": it is not that suppliers are temporarily unavailable, but that the suppliers who could provide the right product at the right density have exited the market.

The reason they left is straightforward and structural. The same Samsung and SK Hynix fabrication facilities that produced LPDDR4X wafers now produce HBM stacks for AI GPU accelerators. HBM uses a different, far more complex process — drilling thousands of Through-Silicon Vias (TSVs) per die and achieving sub-micron alignment across 12 to 16 stacked layers — but it all starts on the same DRAM wafer fabrication equipment. When AI infrastructure demand for HBM became large enough and profitable enough, manufacturers made irreversible wafer allocation decisions. Samsung memory executive Kim Jaejune warned the 2027 gap would widen even further than 2026, telling analysts that "our supply falls far short of customer demand" for AI applications. SK Hynix's CEO similarly warned analysts that 2027 will be memory's "worst year" on record from a supply perspective.

For Raspberry Pi, which purchases LPDDR4X at market rates and has neither Samsung's leverage nor Apple's purchasing power, the structural shift means the seven-fold cost increase in LPDDR4 DRAM over the past two years is not a spike that will revert, but a step-function to a new price level.

### Why the Pi 5 Cannot Simply Switch to Better Memory

A natural question: if LPDDR4 is scarce and expensive, why not design around LPDDR5, which remains in healthier supply and which alternative SBCs like the Orange Pi 5 Plus already use?

The answer is architectural. The Raspberry Pi 5's Broadcom BCM2712 processor contains a memory controller specifically designed for LPDDR4X. LPDDR4X and LPDDR5 are not interchangeable: they operate at different signaling voltages, use different command protocols, and require different timing parameters. A BCM2712 board cannot be reflashed or reconfigured to use LPDDR5 — the silicon itself does not support the standard. Additionally, LPDDR4X on the Pi 5 is Package-on-Package: soldered directly to the board alongside the SoC, not inserted into a removable slot. There is no memory upgrade path. A 4GB Pi 5 buyer in 2024 cannot add RAM; a 16GB Pi 5 buyer today cannot switch to cheaper memory later.

This architectural constraint means the only genuine path to LPDDR5 for the Raspberry Pi platform is the Pi 6 — and CEO Eben Upton confirmed in a Reddit AMA in May 2026 that the Pi 6 will not arrive before early 2028, reflecting the company's typical four-to-four-and-a-half-year product cycle. Until then, buyers who need 16GB on a Pi platform have exactly one option: the $385 board available today.

### What the Foundation Is Doing — and What It Is Not

Raspberry Pi has not been passive. The Foundation has been qualifying alternative DRAM suppliers to increase sourcing flexibility, though as today's announcement makes clear, those efforts have not yet yielded a viable 16GB alternative. Lower-memory models — the 1GB, 2GB, and 4GB Pi 5 variants, and all models based on older LPDDR2 memory — remain at unchanged prices, because LPDDR2 is in adequate supply and lower-density LPDDR4X still has more available sourcing options, as the Foundation confirmed in today's announcement.

The Foundation also confirmed that Raspberry Pi Holdings built up a substantial LPDDR4 stockpile in late 2025, before the worst of the market tightening. That stockpile — purchased at pre-shortage prices — enabled strong H1 2026 financial performance, with The Register reporting the company posted its best-ever first half as customers continued buying boards despite price increases. That buffer is now exhausted, which is why prices accelerated through the second half of 2026.

### What This Means for Your Project

The Foundation's advice is practical: make sure you actually need 16GB before paying for it. Many Pi 5 use cases — retro computing, home automation, lightweight server tasks, classroom Raspberry Pi OS desktops — run comfortably on 4GB or 8GB, and those models are currently priced at $85 and $130 respectively. Software-level memory optimization can also reclaim meaningful headroom; the Raspberry Pi Official Magazine has published a detailed memory optimization guide for Raspberry Pi OS. Raspberry Pi engineers are active on the Raspberry Pi community forums for project-specific advice.

For projects that genuinely need 16GB — containerized applications, edge AI workloads, multitasking dev environments — the realistic alternatives in the current market are narrow. Alternative SBCs using LPDDR5, such as the Orange Pi 5 Plus (with Rockchip RK3588 and LPDDR5), offered competitive pricing at launch, but independent tracking by CNX Software found the 16GB Orange Pi 5 Plus unavailable through Amazon US as of August 2026, as LPDDR5 supply comes under its own pressure. Refurbished mini-PCs or x86 platforms running full DDR5 have become an increasingly rational alternative for users who prioritize memory headroom over Pi-specific I/O and the ARM ecosystem, as Tom's Hardware noted in its coverage of today's price increase.

### Reseller Warning: Read Before You Buy

The Foundation flagged that a number of sellers are exploiting the shortage to charge above the official $385 price, and some are misrepresenting products entirely. All Raspberry Pi products are available at official prices only through the Foundation's network of Approved Resellers. The Approved Reseller page allows buyers to find vetted suppliers by country. If a listing is significantly above $385, or from an unfamiliar seller, the Foundation recommends walking away.

## Frequently Asked Questions

### Why is the Raspberry Pi 5 16GB so expensive now?

The 16GB Pi 5 uses LPDDR4X memory — an older mobile DRAM standard — packaged in a high-density eight-die stack configuration that only a few manufacturers could produce. Samsung, the largest producer, formally discontinued LPDDR4 and LPDDR4X production in 2026 to reallocate its wafer fabrication capacity to High Bandwidth Memory (HBM) for AI GPU accelerators and to LPDDR5 for newer devices. With the main alternative sources unable to supply the high-density format at scale, Raspberry Pi faces a structural supply shortage — not a temporary one. That structural shortage is reflected in today's $385 price, which marks a 221% increase from the $120 launch price in January 2025.

### Will Raspberry Pi 5 prices come back down?

The Foundation has consistently pledged to roll back price increases once memory costs fall. However, with Samsung permanently out of LPDDR4 production and SK Hynix having wound down volume output as well, the supply-side constraint is not a market cycle that will simply correct itself. Relief would require either new entrants scaling up high-density LPDDR4X production (no credible candidates are currently announced) or Raspberry Pi moving to a new platform. The Pi 6 — which would likely adopt LPDDR5 and a new SoC — is not expected before early 2028, as confirmed by Eben Upton in the May 2026 Reddit AMA. For most users, the practical answer is: do not count on a price reversal any time soon for the 16GB tier.

### Do I actually need 16GB, or will a lower-memory Pi 5 meet my needs?

Most Pi 5 use cases do not require 16GB. General desktop use, home automation, retro gaming, media servers, and classroom environments typically run well on 4GB ($85) or 8GB ($130). The 16GB tier is primarily beneficial for containerized server workloads, running multiple virtual machines, edge AI models with large context windows, or software development involving multiple concurrent heavyweight processes. If you are unsure, the Foundation recommends checking its memory optimization guide before committing to the 16GB model.

### Is there a good alternative to the Raspberry Pi 5 16GB at this price?

At $385, the Pi 5 16GB is approaching or matching the price of some entry-level x86 mini-PCs and the Orange Pi 5 Plus with RK3588 and LPDDR5, though the latter has faced its own availability constraints. The Pi 5's key advantages remain its unmatched software ecosystem, tutorial library, HAT accessory compatibility, and GPIO integration — factors that matter greatly for hardware projects. For pure compute and RAM headroom without GPIO requirements, x86 mini-PCs or refurbished workstations may now offer better value per gigabyte.
