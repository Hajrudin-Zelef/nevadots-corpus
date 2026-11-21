---
id: collect-261121-veille-ia/hardware-semi/why-are-ram-and-gpu-prices-spiking-the-rampocalypse
title: "Why Are RAM and GPU Prices Spiking? The RAMpocalypse"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["prices", "price", "memory", "ddr5", "more", "november", "spot", "times"]
source: https://capitalandcompute.net/blog/why-are-ram-and-gpu-prices-spiking/
source_anchor: ""
source_lines: [1, 219]
section: "Hardware_Semi"
---
RAM and GPU prices are spiking because AI data centers claimed the world’s DRAM first. A 32GB DDR5 kit now costs about 7 times its 2025 price, and RTX 50 cards rise again this week.

That is the RAMpocalypse, and the first week of November 2026 delivered it all at once. Graphics card makers in China pushed through RTX 50 increases of up to 3,300 yuan (nearly $500) a card. Amazon started cancelling GPU orders, and Epic Games CEO Tim Sweeney says his were among them. Days earlier, OpenAI CEO Sam Altman said on a podcast that the RAM in your computer is a “quite terrible” use of the world’s memory supply. The same company signed letters of intent in 2025 for a reported 40% of global DRAM output.

PC builders have noticed. This piece pulls every thread together: what is rising, by how much, who is behind it, whether Altman has a point, and what to do if you need hardware now.

## How much have RAM and GPU prices actually gone up?

Far more than the headline percentages suggest, because the rises have compounded for a year. Measured against pre-shortage prices, memory chips cost about 9 times more, consumer RAM kits 5 to 7 times more, and graphics cards 1.6 to 2.5 times their official MSRP.

| How many times more memory and GPUs cost now than before the shortage |  |  | 
|---|---|---|
| Metric | November 2026 as a multiple of Pre-shortage price | Values | 
|---|---|---|
| DDR5 16Gb spot chip | 8.99x | $61.50 vs $6.84 | 
| Cheapest 32GB DDR5 kit | 7.27x | $429 vs $59 | 
| GDDR6 spot per GB | 4.98x | $12.45 vs $2.50 | 
| Average 32GB DDR5 kit | 4.94x | $617 vs $125 | 
| RTX 5090 street vs MSRP | 2.50x | $4,999 vs $1,999 | 
| RTX 5070 street vs MSRP | 1.64x | $900 vs $549 | 

The numbers behind each row:

- **Memory chips.** TrendForce’s November 2025 spot price update already had DDR5 spot prices up 307% since September 2025, from a chip price near $6.84. On November 3, 2026, the DRAMeXchange spot page put the DDR5 16Gb chip at a $61.50 session average.
- **RAM kits.** The cheapest 2x16GB DDR5-6000 CL30 kit tracked by WhereIsMyRAM’s US price chart cost $59 in April 2025 and $429 on November 3, 2026. The average listing went from $125 to $617.
- **Graphics memory.** TrendForce data puts GDDR6 spot at about $2.50 per gigabyte in September 2025 and $12.45 by October 5, 2026.
- **Graphics cards.** Nvidia has not changed its official MSRPs. The street did it instead: Tom’s Hardware found the RTX 5090 at $4,999 or more in September against a $1,999 MSRP.

The live versions of these readings sit on the memory price tracker and the GPU price tracker.

## When did RAM prices start rising?

In October 2025, and the turn is sharp enough to see on a chart. The cheapest DDR5 kit hovered between $50 and $63 for six months. Then, within weeks of OpenAI’s Korean memory agreements, it began a climb that has not stopped.

| Price of a 32GB DDR5-6000 kit, April 2025 to November 2026 |  |  | 
|---|---|---|
| Date | Average listing | Cheapest kit | 
|---|---|---|
| 2025-04-04 | $125 | $59 | 
| 2025-06-15 | $127 | $50 | 
| 2025-08-08 | $132 | $55 | 
| 2025-10-01 | $139 | $53 | 
| 2025-12-12 | $449 | $126 | 
| 2026-02-04 | $569 | $150 | 
| 2026-04-17 | $539 | $150 | 
| 2026-06-10 | $570 | $360 | 
| 2026-08-03 | $571 | $390 | 
| 2026-10-14 | $600 | $396 | 
| 2026-11-03 | $617 | $429 | 

Two phases stand out. The average listing jumped first, from $139 on October 1, 2025 to $449 by mid-December, as retailers repriced stock. The cheapest kit held at $150 through spring 2026, then more than doubled in June when the last low-priced inventory sold through. Timing is not proof of cause: the memory market was already tightening on AI server demand. But the sequence of a 40%-of-supply announcement followed by a fourfold price jump is exactly what the forums are angry about.

## Why are GPU prices going up again this week?

Because the GDDR7 memory on every RTX 50 card keeps getting more expensive, and board partners are passing it on. TechPowerUp reported that RTX 50 prices are scheduled to rise this week, with the largest increase, up to 3,300 yuan or nearly $500, on RTX 5090 D V2 cards sold in China. Wccftech’s November 3, 2026 report gives the model-by-model cost-price changes from Colorful’s partner sources:

| Model | Reported increase (USD) | Cost-price change (yuan) | Where | 
|---|---|---|---|
| RTX 5090 D V2 | $330 to $550 | 2,200 to 3,300 | China | 
| RTX 5080 | $380 to $450 | 2,500 to 2,800 | China | 
| RTX 5070 Ti | about $150 | 1,000 | China | 
| RTX 5070 | $90 to $135 | 600 to 900 | China | 
| RTX 5060 Ti 16GB | $37 to $60 | 250 to 400 | China | 
| RTX 5060 8GB | $22 to $30 | 150 to 200 | China (GALAX also about $30) | 

Two cautions apply. These are partner cost-price changes reported through unnamed industry sources, not an Nvidia announcement, and they are China-market figures. Price changes in China have been an early signal for the rest of the world all year, but a US card will not automatically cost $500 more next week. TechPowerUp notes retail prices may take a week or two to reflect the change.

The pattern behind it is not new. Tom’s Hardware reported RTX 50 prices jumping up to 30% in South Korea, with GDDR7 modules at about $20 each. Over the year, a 2026 PC Guide analysis of 3DCenter’s German price data puts the average rise from November 2025 to October 2026 at 66% for Nvidia, 46% for AMD and 38% for Intel.

| Average GPU price rise by vendor, November 2025 to October 2026 |  | 
|---|---|
| Item | Value | 
|---|---|
| Nvidia | +66% | 
| AMD | +46% | 
| Intel | +38% | 

One likely reason Nvidia rose most: the RTX 50 series uses GDDR7, the newest and scarcest graphics memory, while most current AMD and Intel boards use GDDR6. The full story of how a $1,999 card reached $5,000 is in why the RTX 5090 sold out at $5,000, and the outlook for next year is in will GPU prices go up in 2027.

## Is Amazon cancelling GPU orders?

For some buyers, yes, and nobody knows why. On November 2, 2026, Wccftech reported that Amazon was cancelling orders for compute hardware and, in rare cases, closing accounts. Two cases anchor the story:

- **AI educator Nick Saraev** says Amazon cancelled his four Nvidia DGX Spark orders, placed October 31, and then closed his account after a “verification process.” Tom’s Hardware’s headline on the case counts seven units. His own post on X reads: “Update: Amazon just closed my account. Buying even modest amounts of compute immediately puts you on a watchlist. Be careful!”
- **Epic Games CEO Tim Sweeney** says his account was not closed, but after he tried to buy two Nvidia GPUs, Amazon kept accepting his regular orders and cancelling them shortly before they were due to arrive.

Whatever the cause, the timing makes it explosive. When a GPU or a DGX Spark is scarce enough that one buyer ordering a handful looks like reselling, the hardware has stopped behaving like a consumer product.

## Did OpenAI lock up 40% of the world’s RAM?

On paper, it signed for up to that much. In October 2025, Altman met Samsung and SK Group leadership in Seoul, and both chipmakers signed letters of intent to supply OpenAI’s Stargate build-out, as Bloomberg reported. The target was up to 900,000 DRAM wafer starts a month, which outlets such as The Deep Dive put at up to 40% of global DRAM output.

Three caveats keep this accurate:

1. **They are letters of intent, not purchase orders.** As The Deep Dive’s headline puts it, OpenAI has “no obligation to buy any of it.”
2. **40% is a ceiling and a press estimate** , based on planned capacity, not a confirmed volume OpenAI has taken.
3. **The “80%” on social media has no source.** No primary report found for this piece supports it.

Even the honest version is enormous. A single customer reserving a large share of the industry’s wafer starts tells every memory maker where the next two years of capacity should go, and it is not to the retail shelf.

## How does the AI memory shortage actually work?

Every DRAM chip, whether it ends up in a gaming PC, a phone or an Nvidia AI accelerator, is cut from the same silicon wafers in the same handful of fabs, run mainly by Samsung, SK hynix and Micron. When a wafer is assigned to AI memory, it is not making your RAM.

AI memory is also far hungrier per bit. High-bandwidth memory (HBM) stacks many DRAM dies on top of each other and loses yield in the process. TrendForce estimates HBM takes about 22% of DRAM wafer input in 2026 while producing only about 9% of the bits, and expects about 30% of wafers for 13% of bits by the end of 2027. Micron has described HBM as consuming roughly three units of DDR5 capacity for each unit of HBM, as covered in why RAM is so expensive. So every shift toward AI removes about three times as much ordinary memory as it adds.

The makers are choosing that trade on purpose, because it pays. On September 30, 2026, Micron reported record quarterly revenue of $54.23 billion, against $11.32 billion a year earlier, at a gross margin near 87%. In December 2025, it announced it was exiting the Crucial consumer business altogether, to “improve supply and support for our larger, strategic customers in faster-growing segments.” One of the three big DRAM makers stopped selling RAM to consumers under its own brand. That is the shortage in one sentence.

Graphics cards sit right in the blast radius. GDDR6 and GDDR7 are DRAM too, made by the same three companies, so when memory makers raise prices, board partners raise theirs.

## What did Sam Altman say about your RAM?

On the Cheeky Pint podcast with Stripe co-founder John Collison, published October 29, 2026, Altman said this, from the official episode transcript:

Coming from the CEO whose company signed for a reported 40% of global DRAM output, it landed badly. Tom’s Hardware headlined it as “quite terrible for your PC to have RAM.” PC Gamer ran it as Altman arguing “it’s bad you want DRAM in your computer and not a data center, actually.” Futurism went further: “Sam Altman Absolutely Hates the Idea of People Running Their Own AI Models at Home.” Windows Central’s columnist called him a sociopath. Elon Musk replied on X that “the people deserve to have their RAM too,” as summarized in X’s trending note on the backlash.

## Why are PC gamers so angry about the RAM shortage?

Because, from where a PC builder sits, the story has a villain, a motive and a punchline. The same company that reserved a reported 40% of the world’s DRAM wafers is now saying the RAM people already own is the waste. On r/pcmasterrace, users quoted by Futurism called Altman a “self-centered POS” with a “cult leader mentality,” and one argued that the idea of people owning their own computers seems to be “an alien concept to him.” On r/LocalLLaMA, the clip became a thread summed up as “Guy selling product”. As each round of price rises lands, r/pcmasterrace greets it with posts like this one, simply titled “Anytime now”.

The threads are reader sentiment, not evidence. But the sequence they are reacting to is documented:

1. October 1, 2025 OpenAI signs Korean memory letters of intent Samsung and SK hynix target up to 900,000 DRAM wafers a month for Stargate, reported as up to 40% of global output.
2. November 2025 DDR5 spot up 307% since September TrendForce reports the jump; the average 32GB DDR5 kit listing passes $240, from $139 in early October.
3. December 3, 2025 Micron exits the Crucial consumer business One of the three big DRAM makers stops selling consumer RAM and SSDs to focus on strategic customers.
4. June 2026 The last cheap RAM sells through The cheapest 32GB DDR5 kit jumps from $150 to $360.
5. September 2026 RTX 5090 hits $5,000 The $1,999 card sells for $4,999 or more at US retailers.
6. September 30, 2026 Micron books $54 billion quarter; TrendForce sees more rises Record revenue at a gross margin near 87%. DRAM contract prices forecast up 10% to 15% in 4Q26.
7. October 29, 2026 Altman: your RAM is a "quite terrible" allocation The OpenAI CEO argues DRAM belongs in high-utilization data centers.
8. November 2, 2026 Amazon cancels GPU orders Reports that Amazon is cancelling GPU and DGX Spark orders; Tim Sweeney says his were hit.
9. November 3, 2026 RTX 50 rises of up to $500 in China Partner sources report cost-price increases of up to 3,300 yuan on the RTX 5090 D V2, effective this week.

## Is Sam Altman right that home RAM is wasted?

Half right. The economics part holds: hardware that sits idle most of the day makes each unit of work cost more, and a data center running memory near full time gets more out of each gigabyte. Running the numbers in is local AI a waste of DRAM? shows a home rig at personal volume can cost up to 13 times more per token than the same machine kept busy.

The scarcity part does not hold. Home PCs are not what is draining supply; HBM for AI accelerators is, and it burns about three times the wafer per gigabyte that desktop RAM does. Altman also gave no source for his 5% utilization figure, and RAM in a PC does far more than run AI: games, browsers and everyday multitasking all lean on it. Reallocating consumer RAM would not dent the shortage. Building fewer AI accelerators would, and nobody in the industry is proposing that.

## Who is affected beyond PC gamers?

Almost everyone who buys a device with memory in it:

- **PCs and laptops.** Gartner expects combined DRAM and SSD prices to rise about 130% across 2026, lifting average PC prices by roughly 17% and cutting PC and smartphone shipments. PC makers including Dell, Lenovo, HP, Acer and ASUS have warned of 15% to 20% price increases, per Tom’s Hardware.
- **Phones.** IDC expects smartphone average selling prices to rise as memory costs pass through, and TrendForce reported phone and notebook brands raising prices and downgrading specs to cope.
- **Storage.** NAND flash is caught in the same squeeze; SSD prices have followed RAM up.
- **Local AI users.** Anyone hoping to run models at home needs exactly the large RAM and VRAM pools that cost the most now. The trade-offs are in how much RAM you need to run a local LLM.

## When will RAM and GPU prices come down?

Not this year, and probably not next. TrendForce’s September 30, 2026 release projects conventional DRAM contract prices up another 10% to 15% quarter on quarter in the fourth quarter. TrendForce has also raised its 2027 HBM price outlook, with the blended HBM average selling price up 121% year on year, which keeps memory makers’ incentives pointed at AI.

Intel CEO Lip-Bu Tan has said the memory industry told him there is no relief until 2028. SK hynix’s CEO has called 2027 the worst year of the shortage, per a Reuters interview reported by Tom’s Hardware. The earliest credible inflection anyone names is late 2027, as laid out in the 2027 memory shortage forecast, and the new fabs that could actually fix it mostly come online in 2028 and 2029, per the memory capex build-out. Even then, relief is likely to mean prices that stop rising, not a return to $59 kits.

## Should you buy RAM or a GPU now or wait?

The verdict depends on how urgently you need it:

- **You need it in the next six months:** buy now. Waiting has cost money in every quarter of 2026, contract prices are still rising, and another round of RTX 50 increases is already under way in China.
- **You can wait a year or more:** waiting is reasonable, but plan for flat prices in late 2027 at best, not a crash. When will RAM prices drop tracks the signals worth watching.
- **You already own enough RAM:** keep it, run whatever you like on it, and ignore anyone who calls it wasted.
- **You are waiting on the Nvidia investigation:** don’t. The board’s probe into Jensen Huang has no channel to card prices, as explained in will the Nvidia CEO probe lower GPU prices.

## How to buy RAM or a GPU during the shortage

1. **Size the purchase to your real need.** How much RAM for gaming has the per-game detail. Buying 64GB “for the future” costs about $1,360 at the RAM Prices USA median of $21.25 per gigabyte.
2. **Check the floor, not the average.** The gap between the cheapest and the average 32GB kit is close to $190. The memory price tracker shows both.
3. **Consider DDR4 if your platform allows it.** A 16GB DDR4 kit runs about $130 against roughly $297 for 16GB of DDR5, per RAM Prices USA Amazon medians from October 2026.
4. **Buy GPUs from first-party or major retailer stock,** and expect order limits. If you are buying more than one high-end card or AI box, the Amazon reports suggest splitting orders or buying direct from the maker.
5. **Look beyond the RTX 50 series.** AMD and Intel cards, which mostly use GDDR6, rose less than Nvidia’s over the past year (46% and 38% against 66%). More savings ideas are in how to save money on RAM.

## Frequently asked questions

- Why are RAM and GPU prices so high in November 2026?
- AI data centers are absorbing DRAM supply, especially high-bandwidth memory, which uses about 22% of DRAM wafers in 2026 for roughly 9% of the bits. The cheapest 32GB DDR5 kit rose from $59 in April 2025 to $429 in November 2026, and GPU makers pass higher GDDR7 costs to buyers.
- How much are RTX 50 prices going up this week?
- Partner sources reported by TechPowerUp and Wccftech put China-market cost-price increases at up to 3,300 yuan (nearly $500) on the RTX 5090 D V2, up to 2,800 yuan on the RTX 5080, about 1,000 yuan on the RTX 5070 Ti and 150 to 200 yuan on the RTX 5060. These are China figures, not an Nvidia announcement.
- Did OpenAI buy 80% of the world RAM supply?
- No primary report supports 80%. OpenAI signed non-binding letters of intent with Samsung and SK hynix in October 2025 for up to 900,000 DRAM wafers a month, reported as up to 40% of global DRAM output.
- What did Sam Altman say about RAM in PCs?
- On the Cheeky Pint podcast on October 29, 2026, Altman said it is "quite terrible" from a global allocation perspective for everyone to keep part of the world's limited DRAM supply in their own computers, used about 5% of the time, and that the cloud offers better utilization.
- Why is Amazon cancelling GPU orders?
- Amazon has not said. Since early November 2026, buyers including Epic Games CEO Tim Sweeney report cancelled GPU orders, and AI educator Nick Saraev says his account was closed after ordering DGX Spark units. A purchase limit, a fraud filter or a system error would all fit the evidence.
- When will RAM prices go down?
- TrendForce expects DRAM contract prices to rise another 10% to 15% in 4Q26. The earliest credible relief point is late 2027, with new fab capacity arriving mostly in 2028 and 2029, and relief is likely to mean prices that stop rising rather than fall back to 2025 levels.

## Sources

- TrendForce (2025). *Memory Spot Price Update: DDR5 prices up 307% since September as module costs poised to surge* . https://www.trendforce.com/news/2025/11/19/insights-memory-spot-price-update-ddr5-prices-up-307-since-september-as-module-costs-poised-to-surge/
- TrendForce (2026). *DRAM spot price* (DRAMeXchange, DDR5 16Gb session average, November 3 2026). https://www.trendforce.com/price/dram/dram_spot
- WhereIsMyRAM / DropReference (2026). *US DDR5 price chart* , 2x16GB DDR5-6000 CL30 (read November 3 2026; secondary aggregator). https://whereismyram.com/us/price-chart
- TrendForce (2026). *AMD Reportedly Raises GPU-GDDR Kit Prices by ~10% for AIB Partners, Effective Jul. 2026* . https://www.trendforce.com/news/2026/07/03/news-amd-reportedly-raises-gpu-gddr-kit-prices-by-10-for-aib-partners-effective-jul-2026/
- Tom’s Hardware (2026). *Nvidia’s top-end RTX 5090 gaming GPU now costs at least $5,000* . https://www.tomshardware.com/pc-components/gpus/nvidias-top-end-rtx-5090-gaming-gpu-now-costs-at-least-usd5-000-blackwell-cards-continue-to-endure-drastic-price-hikes
- TechPowerUp (2026). *NVIDIA GeForce RTX 50-Series Scheduled for up to $500 Price Hike This Week* . https://www.techpowerup.com/354427/nvidia-geforce-rtx-50-series-scheduled-for-up-to-usd-500-price-hike-this-week
- Wccftech (2026). *NVIDIA RTX 50 GPUs Will See A Massive Price Increase In November* (November 3 2026; citing partner sources). https://wccftech.com/nvidia-rtx-50-gpus-see-a-massive-price-increase-in-november/
- Tom’s Hardware (2026). *Nvidia RTX 50 series prices jump up to 30 percent in South Korea* . https://www.tomshardware.com/pc-components/gpus/in-a-troubling-sign-nvidia-rtx-50-series-prices-jump-up-to-30-percent-in-south-korea-tsmc-wafer-hikes-and-usd20-gddr7-modules-push-rtx-5090-past-usd5-100
- 3DCenter via PC Guide (2026). *GPU pricing is getting much worse, it’s hard to see an end in sight* (secondary coverage of 3DCenter data). https://www.pcguide.com/news/gpu-pricing-is-getting-much-worse-its-hard-to-see-an-end-in-sight/
- Wccftech (2026). *Amazon Is Allegedly Canceling GPU Orders And, In Rare Cases, Closing Accounts For Purchasing Compute Hardware; Tim Sweeney Also A Victim Of This Practice* (November 2 2026). https://wccftech.com/amazon-allegedly-canceling-gpu-orders-closing-accounts-compute-hardware-tim-sweeney/
- Tom’s Hardware (2026). *Amazon closes AI creator’s account after he attempts to buy seven Nvidia DGX Spark mini-PCs* . https://www.tomshardware.com/desktops/mini-pcs/amazon-closes-ai-creators-account-after-he-attempts-to-buy-seven-nvidia-dgx-spark-mini-pcs-epic-ceo-tim-sweeney-says-his-regular-orders-were-canceled-before-arrival-after-he-tried-to-buy-two-gpus
- Nick Saraev (2026). Post on X, November 2026. https://x.com/nicksaraev/status/2116916521368981687
- Bloomberg (2025). *Samsung, SK Hynix Shares Leap After OpenAI Taps Korea for Chips* . https://www.bloomberg.com/news/articles/2025-10-01/samsung-sk-hynix-ink-deal-to-supply-gear-to-openai-s-stargate
- The Deep Dive (2026). *OpenAI Locked Up 40% of Global RAM With No Obligation to Buy Any of It* . https://thedeepdive.ca/openai-locked-up-40-of-global-ram-with-no-obligation-to-buy-any-of-it/
- TrendForce (2026). *Tight DRAM Supply Gives Suppliers Greater Pricing Power in HBM* (press release, June 2 2026). https://www.trendforce.com/presscenter/news/20260602-13074.html
- Micron Technology (2026). *Fiscal Q4 2026 results* (press release, SEC Form 8-K exhibit 99.1, September 30 2026). https://www.sec.gov/Archives/edgar/data/0000723125/000072312526000018/a2026q4ex991-pressrelease.htm
- Micron Technology (2025). *Micron Announces Exit from Crucial Consumer Business* (press release, December 3 2025). https://micron.gcs-web.com/node/49841
- Cheeky Pint (2026). *Why Sam Altman is against a single superintelligence* (episode transcript, October 29 2026). https://cheekypint.substack.com/p/why-sam-altman-is-against-a-single
- Tom’s Hardware (2026). *OpenAI CEO says it’s ‘quite terrible’ for your PC to have RAM* . https://www.tomshardware.com/tech-industry/data-centers/openai-ceo-says-its-quite-terrible-for-your-pc-to-have-ram-sam-altman-says-data-centers-offer-better-utilization
- PC Gamer (2026). *Sam Altman argues that it’s bad you want DRAM in your computer and not a data center, actually* . https://www.pcgamer.com/software/ai/sam-altman-argues-that-its-bad-you-want-dram-in-your-computer-and-not-a-data-center-actually/
- Futurism (2026). *Sam Altman Absolutely Hates the Idea of People Running Their Own AI Models at Home* (October 30 2026). https://futurism.com/artificial-intelligence/sam-altman-hates-local-ai-models-computers
- Windows Central (2026). *Sam Altman says it’s terrible that your PC, Xbox etc. have RAM* (opinion column). https://www.windowscentral.com/artificial-intelligence/you-dont-hate-him-enough-openai-sociopath-sam-altman-says-its-terrible-that-your-pc-xbox-etc-have-ram-that-could-be-used-for-ai
- X (2026). *Sam Altman’s RAM critique draws memes and backlash* (trending summary; secondary). https://x.com/i/trending/2116562776718250414
- TrendForce (2026). *AI Server Demand Sustains Memory Contract Price Increases in 4Q26* (press release, September 30 2026). https://www.trendforce.com/presscenter/news/20260930-13258.html
- TrendForce (2026). *HBM Supply Constraints Persist, 2027 Price Outlook Revised Upward* (press release, September 29 2026). https://www.trendforce.com/presscenter/news/20260929-13255.html
- Reuters via Tom’s Hardware (2026). *SK Hynix says 2027 will be the worst year for memory shortage* . https://www.tomshardware.com/pc-components/dram/sk-hynix-says-2027-will-be-the-worst-year-for-memory-shortage-forecasts-crunch-to-last-until-2030-ceo-shares-grim-outlook-on-the-day-sk-hynix-gets-listed-on-nasdaq
- RAM Prices USA (2026). *DDR4 and DDR5 price medians* (Amazon listings, October and November 2026; secondary aggregator). https://rampricesusa.com/
- Bloomberg (2026). *Intel CEO says there’s no relief on memory shortage until 2028* . https://www.bloomberg.com/news/articles/2026-02-03/intel-ceo-says-there-s-no-relief-on-memory-shortage-until-2028
- Gartner (2026). *Gartner Says Surging Memory Costs Will Reduce Global PC and Smartphone Shipments in 2026* (press release, February 26 2026). https://www.gartner.com/en/newsroom/press-releases/2026-02-26-gartner-says-surging-memory-costs-will-reduce-global-pc-and-smartphone-shipments-in-2026
- Tom’s Hardware (2026). *Framework warns of even more rising RAM and SSD prices through 2026* . https://www.tomshardware.com/laptops/framework-warns-of-even-more-rising-ram-and-ssd-prices-through-2026-as-memory-crisis-persists-some-reprieve-as-prices-plateau-in-latest-monthly-update
- IDC (2025). *Global Memory Shortage Crisis: Market Analysis and the Potential Impact on the Smartphone and PC Markets in 2026* . https://www.idc.com/resource-center/blog/global-memory-shortage-crisis-market-analysis-and-the-potential-impact-on-the-smartphone-and-pc-markets-in-2026/
- TrendForce (2025). *Memory Price Surge to Persist in 1Q26; Smartphone and Notebook Brands Begin Raising Prices and Downgrading Specs* (press release). https://www.trendforce.com/presscenter/news/20251211-12831.html
