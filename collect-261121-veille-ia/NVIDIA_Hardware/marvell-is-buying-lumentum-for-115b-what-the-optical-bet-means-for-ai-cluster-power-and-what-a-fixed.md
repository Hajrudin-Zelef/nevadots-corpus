---
id: collect-261121-veille-ia/nvidia-hardware/marvell-is-buying-lumentum-for-115b-what-the-optical-bet-means-for-ai-cluster-power-and-what-a-fixed
title: "Marvell is buying Lumentum for $115B: what the optical bet means for AI cluster power, and what a fixed 4.5x exchange ratio really locks in"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-02"]
keywords: ["marvell", "lumentum", "about", "optical", "billion", "ratio", "fixed", "value"]
source: https://dev.to/ai_maya_063fc568e157562fd/marvell-is-buying-lumentum-for-115b-what-the-optical-bet-means-for-ai-cluster-power-and-what-a-5chf
source_anchor: ""
source_lines: [1, 156]
section: "NVIDIA_Hardware"
---
On November 2, 2026, Marvell Technology and Lumentum announced a definitive agreement under which Marvell will acquire Lumentum in an all-stock deal valued at an implied enterprise value of $115 billion. Each Lumentum share converts into 4.500 Marvell shares, Lumentum holders would own about 33% of the combined company, and closing is expected in the first half of 2028. The same day Lumentum reported revenue of $1.26 billion for its fiscal first quarter, up 136.5% from a year earlier, and guided to roughly $1.6 billion for the next one.

The headline is a merger, but the engineering story is about where the watts go in an AI cluster. Vendor claims put a 1.6T pluggable optical port at about 30 W and a co-packaged optics (CPO) port at about 9 W. For a 100,000 GPU cluster with two links per GPU, my illustrative model gives 6.0 MW versus 1.8 MW. This post walks through the announced terms, checks the arithmetic, explains why a laser maker sits at the center of a CPO supply chain even though CPO needs fewer lasers, and lists what to ask a vendor before you believe any per-port power figure.

Everything below comes from company press releases and press coverage, linked at the end. The power model is an illustration built on vendor-claimed inputs, not a measurement.

The terms, as stated in Marvell's press release and repeated by financial outlets:

| Item | Announced value | 
|---|---|
| Structure | All-stock, definitive agreement, announced 2026-11-02 | 
| Exchange ratio | Fixed, 4.500 Marvell shares per Lumentum share | 
| Implied enterprise value | About $115 billion | 
| Implied value per Lumentum share | About $1,132 | 
| Premium | About 23.9%, measured against unaffected closing prices on 2026-10-28 | 
| Pro forma ownership | About 67% Marvell holders, about 33% Lumentum holders (fully diluted) | 
| Cost synergies | More than $1 billion annual run-rate | 
| EPS effect | Expected to be accretive to Marvell non-GAAP EPS in year one | 
| Expected close | First half of 2028, subject to both stockholder votes and regulatory clearances | 
| Leadership | Matt Murphy stays chairman and CEO; three Lumentum directors join the Marvell board | 

Marvell's own framing is that the combination covers "electrical and optical interconnect, network switching, custom XPU" on one side and "high-performance lasers, optical transceivers, optical circuit switching" on the other, and it cites a combined addressable market of $550 billion. That figure is a company estimate, so treat it as positioning rather than a measurement.

Two details deserve a second look.

First, the ratio is fixed. A fixed exchange ratio means the dollar value of the deal moves with Marvell's share price every day until closing. The $115 billion and $1,132 figures are snapshots computed from reference prices around the announcement. If Marvell's stock falls 20% over the next 18 months, Lumentum holders receive 20% less value per share; if it rises, they receive more. The fixed ratio is what makes the 67/33 ownership split a stable number, and the dollar figures the unstable ones.

Second, the timeline is long. A first-half 2028 close is roughly 18 months out. That is a long window for a company whose customers are placing multi-year capacity bets, and it means both firms keep selling independently for a long time. For anyone designing a cluster that will ship in 2027, the practical impact of the merger itself is small. The signal is what the buyer thinks the architecture will look like.

Marvell's stated reason is an architectural shift. CEO Matt Murphy called it "a fundamental architectural shift toward optical connectivity, creating an opportunity that comes along once in a generation." Lumentum's CEO Michael Hurlston described the combined company serving "scale-across and scale-out today to scale-up and scale-in domains tomorrow."

Those four terms map to distances inside and between AI data centers:

- 
**Scale-across** : links between data center sites.
- 
**Scale-out** : the rack-to-rack and row-to-row network that connects accelerators into a cluster.
- 
**Scale-up** : the high-bandwidth domain that makes a rack of accelerators behave like one big device.
- 
**Scale-in** : connections at the package and board level.

Copper dominates the shortest links today. The argument for optics is that as per-lane speeds rise, the reach of passive copper shrinks, and each step up the hierarchy that moves from copper to light creates demand for lasers, modulators and the electronics around them. Marvell already bought into this thesis earlier in 2026 when it acquired Celestial AI, a company with optical multi-chip interconnect technology (reported by TrendForce). Lumentum adds the part Marvell did not own: the light source.

That context also explains the Nvidia angle. Nvidia made two $2 billion strategic investments in photonics suppliers, one in Lumentum and one in Coherent. A buyer that acquires one of those two suppliers inherits the relationship.

This is the number that drives the whole thesis, so it is worth computing instead of repeating.

In a conventional switch, each port is a pluggable transceiver: a module with its own laser, modulator, driver and a digital signal processor (DSP) that cleans up the electrical signal. In co-packaged optics, the optical engine sits next to the switch ASIC, which shortens the electrical path and removes most of the DSP.

Nvidia's published claims for its CPO switches, as relayed by trade press, are:

- About 30 W per 1.6T pluggable port versus about 9 W per 1.6T CPO port, which the company frames as 3.5x lower power. The two quoted wattages give 3.33x, so the 3.5x headline is probably computed on a slightly different baseline.
- Four times fewer lasers than the same bandwidth built from pluggable modules.
- Ten times higher mean time between incidents on the optical path.
- A 200 Gb/s per lane SerDes design. StorageReview reported on August 15, 2026 that the Spectrum-X Ethernet Photonics platform had entered full production, with Lumentum named as the laser chip supplier.

Here is the model I used. Every input is an assumption or a vendor claim, and the code makes that visible.


```
LINKS_PER_GPU = 2      # assumption: two optical links per GPU
W_PLUGGABLE   = 30.0   # W per 1.6T pluggable port (vendor claim, as reported)
W_CPO         = 9.0    # W per 1.6T CPO port (vendor claim, as reported)
PRICE_MWH     = 70.0   # USD per MWh (assumption)
HOURS         = 8760
def mw(gpus, w):
    return gpus * LINKS_PER_GPU * w / 1e6
for g in (25_000, 50_000, 100_000, 200_000):
    p, c = mw(g, W_PLUGGABLE), mw(g, W_CPO)
    usd = (p - c) * HOURS * PRICE_MWH
    print(f"{g:>7} GPUs  pluggable {p:5.2f} MW  CPO {c:5.2f} MW  "
          f"saved {p-c:5.2f} MW  ~${usd/1e6:4.2f}M/yr")
```
Output:


```
  25000 GPUs  pluggable  1.50 MW  CPO  0.45 MW  saved  1.05 MW  ~$0.64M/yr
  50000 GPUs  pluggable  3.00 MW  CPO  0.90 MW  saved  2.10 MW  ~$1.29M/yr
 100000 GPUs  pluggable  6.00 MW  CPO  1.80 MW  saved  4.20 MW  ~$2.58M/yr
 200000 GPUs  pluggable 12.00 MW  CPO  3.60 MW  saved  8.40 MW  ~$5.15M/yr
```
Reading the result honestly: the saving is real in absolute terms, 4.2 MW at 100,000 GPUs, but it is small next to the accelerators themselves. At a few hundred watts to over a kilowatt per accelerator, a 100,000 GPU cluster draws on the order of 100 MW or more, so the network optics in this model are single-digit percent of total power either way. The electricity bill is not the main reason the industry cares. The more important effects are that 4.2 MW is power and cooling capacity you can give to compute instead, and that fewer lasers and fewer modules mean fewer parts that can fail across hundreds of thousands of links. That is why the mean-time-between-incidents claim may matter more to operators than the watts.

This looks like a contradiction, and it is the most interesting question in the deal.

Fewer lasers per bit does not mean less demand for laser capability. In an external laser source design, the lasers move out of the switch package into separate, replaceable modules that feed continuous light to the optical engines. Each of those lasers has to be a high-power, highly reliable part, because one laser now serves many channels and its failure takes down more links. My reasoning, not a sourced claim, is that this shifts value from volume (millions of small lasers inside pluggables) toward quality and power (fewer, harder-to-make lasers). A supplier with proven high-power laser manufacturing is positioned for that shift, which fits Lumentum's reported strength in pump lasers and optical circuit switches.

Lumentum's latest quarter shows the demand side of this. These figures come from the company's results for the quarter ended September 26, 2026:

| Metric | Q1 FY2027 | 
|---|---|
| Net revenue | $1,262.6 million ($754.2M Components + $508.4M Systems) | 
| Year-over-year growth | 136.5% | 
| Non-GAAP gross margin | 53.2% | 
| Non-GAAP operating margin | 42.3% | 
| Non-GAAP EPS | $4.57 | 
| Q2 guidance, revenue | $1.575 billion to $1.625 billion | 
| Q2 guidance, non-GAAP operating margin | 45.0% to 46.0% | 

The segments add up: 754.2 plus 508.4 is 1,262.6, which makes Components 59.7% and Systems 40.3%. The guidance midpoint of $1.60 billion is about $337 million above Q1, or 24.7% to 28.7% sequential growth across the range, consistent with the company's own statement that it expects to add approximately $350 million in sequential revenue. Working backward from the reported 136.5% growth, Q1 of the prior fiscal year was roughly $534 million.

The company said pump lasers and optical circuit switches were the leading contributors. One secondary report also says Lumentum described a second major NPO/CPO design win with more than $5 billion in purchase commitments and capacity that is effectively sold out. I could only confirm that from a summary, not from the primary release, so treat it as unverified.

A few practical rules for reading a deal like this:

1. Separate the structure from the headline. The ratio, the ownership split and the closing date are facts. The dollar value is a moving number.
2. Separate company claims from measurements. The $550 billion market, the $1 billion synergies and the 3.5x power claim are all projections or vendor figures.
3. Check the baseline. A "3.5x" or "5x" reduction depends on what is being compared: a single port, a whole switch, or a whole network including lasers and cooling. The 30 W to 9 W pair above gives 3.33x, and a separate "5x" figure appears for the full platform, so at least two baselines are in circulation.

One related item landed the same weekend and is a useful reminder of what the optics are for. Saudi Aramco and the Public Investment Fund announced on November 1 that Aramco completed its purchase of a 49% stake in the AI company Humain for about $2.6 billion in cash contributions during 2026, with PIF keeping 51%. Humain is reported to plan three gigawatts of AI data center capacity by 2030. At gigawatt scale, megawatt-class savings from the network become one line in a long power budget, which is the right way to size the optics story.

If you are planning a cluster or buying switches, these questions turn a marketing figure into something you can budget:

- Does the 9 W figure include the share of the external laser source, or only the optical engine? I could not confirm this from public material.
- Is the number measured at full line rate with realistic traffic, or at idle?
- What is the replacement unit when an optical engine fails? A pluggable can be swapped in the field; a co-packaged engine may mean replacing a switch or a tray.
- How many lasers does a single laser module feed, and what is the failure domain when it dies?
- What is the cooling requirement at the switch faceplate compared with a pluggable design?
- What happens to the supply chain if your chosen laser supplier becomes part of a competitor's parent company? The merger will not close until 2028, but contract terms signed now will outlive that date.

**What is the Marvell and Lumentum deal in one sentence?**

Marvell agreed on November 2, 2026 to acquire Lumentum in an all-stock transaction with an implied enterprise value of about $115 billion, at a fixed ratio of 4.500 Marvell shares per Lumentum share.

**When will the deal close?**

Marvell expects the first half of 2028, subject to approval by both companies' stockholders and regulatory clearances.

**What is co-packaged optics?**

Co-packaged optics places the optical engine next to the switch chip instead of in a removable pluggable module, shortening the electrical path and removing most of the signal-processing electronics. Nvidia's switches are the best-known production example.

**How much power does CPO save?**

Nvidia's reported figures are about 30 W per 1.6T pluggable port versus about 9 W for CPO, a 3.33x ratio on those two numbers. In a 100,000 GPU cluster with two links per GPU that is 6.0 MW versus 1.8 MW in my illustrative model.

**Does CPO reduce the need for lasers?**

It reduces the count, with Nvidia claiming four times fewer lasers, but it concentrates demand on fewer, higher-power, more reliable lasers, which is the part of the supply chain Lumentum sells into.

**Is the saving large compared with total cluster power?**

In absolute terms it is megawatts, but it is a single-digit share of a cluster that draws on the order of 100 MW. The reliability claim and freed-up power capacity matter more than the electricity bill.

- Inference NPUs That Train: What a 325M Pretraining Run on a Rebellions ATOM-Max Shows
- Can two Mac Studios really run an 816B model? A bandwidth roofline check on Muse Spark Open

- Marvell and Lumentum to Combine, Creating a Category Leader in AI Connectivity (Marvell investor relations, 2026-11-02)
- Lumentum Announces First Quarter of Fiscal Year 2027 Financial Results (Lumentum investor relations)
- NVIDIA Spectrum-X Ethernet Photonics Enters Full Production (StorageReview, 2026-08-15)
- Marvell Eyes Lumentum Acquisition (TrendForce, 2026-10-29)
- Saudi Aramco acquires 49% stake in HUMAIN in $2.6bn AI deal with PIF (Arabian Business)

## Top comments (0)
