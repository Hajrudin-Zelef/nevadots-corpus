---
id: collect-261121-veille-ia/claude-anthropic/anthropic-s-895-4bn-compute-bill-king-of-wall-street
title: "Anthropic's $895.4bn Compute Bill | King of Wall Street"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-10"]
keywords: ["anthropic", "broadcom", "prospectus", "commitments", "compute", "amazon", "leases", "google"]
source: https://mastermeigu.com/en/analysis/anthropic-s1a-compute-commitments-2026/
source_anchor: ""
source_lines: [1, 230]
section: "Claude_Anthropic"
---
*Master M | November 10, 2026, Taipei time*

## The conclusion first

A chart has been making the rounds among investors: "Google's TPUs are 44% of Anthropic's $644bn of deals." It comes from Haircut, and a long Chinese-language commentary built on it describes a "nested web of debt" and ends with a warning that, when the bill for this chip-backed contract game finally comes due, the people at the very end of the market will quietly pay it.

I checked it line by line against Anthropic's S-1/A (Amendment No. 1 to its IPO prospectus, filed November 2), the Broadcom master equipment lease (Exhibit 10.11), and Broadcom's own investor presentation. My view comes down to five points:

1. **The numbers are mostly right, but $644bn is not a line item in the prospectus.** Total commitments of $895.4bn, $69.8bn due within 12 months, Google $121.3bn, Amazon $110bn and SpaceX up to $120.5bn all match the filing. The $644bn is Haircut's own sum of selected deals, and it adds three different kinds of numbers: minimum commitments, maximum caps and lease principal.
2. **The crucial 72.5% clause has been read in the wrong direction.** It is not Broadcom's protection; it is Broadcom's obligation to the lenders. If Anthropic defaults and the recovered racks are worth less than 72.5% of the remaining rent, Broadcom covers the shortfall, up to about $43bn, declining to zero over five years.
3. **The "invisible balance sheet" claim does not hold.** Every commitment is disclosed in the prospectus; the Broadcom leases are accounted for as finance leases; the two special purpose entities are Anthropic's own subsidiaries, with obligations guaranteed by the parent. What sits off the balance sheet are commitments that have not started yet, not hidden debt.
4. **The $42bn PIK convertible facility is overstated.** It is a standby facility that can be used "if needed." Nothing was drawn as of September 30, no notes are expected before the IPO, and any notes would ultimately convert into stock. It looks more like future dilution than a debt snowball.
5. **The real risk: $447.5bn falls due before the end of 2030, and the residual-value risk sits mainly with Anthropic.** It can pay in the short term, with $131.9bn in cash and short-term investments. Whether it can pay in the long term depends on whether revenue keeps growing at a high rate. This is a bet that revenue must catch up with compute, not yet evidence that a bubble is bursting.

Let's take it piece by piece.

## 1. Where the $644bn actually comes from

The prospectus gives total commitments of $895.4bn, "substantially all" related to compute and infrastructure. That total includes the $320.0bn in the contractual obligations table, $451.2bn of leases not yet commenced, about $100.2bn of commitments signed after September 30, and short-term commitments.

Haircut's $644bn takes seven deals from the prospectus's "most significant arrangements" section and groups them by chip:

- **Google TPUs ($282.5bn):** the Google cloud commitment of $121.3bn plus all Broadcom equipment leases of $161.2bn. The prospectus says the Broadcom leases provide "TPU-based compute capacity," so the grouping is reasonable.
- **NVIDIA GPUs ($251.3bn):** SpaceX up to $120.5bn, Microsoft $44.9bn, Nscale up to $44.6bn and Lambda $41.3bn.
- **Amazon Trainium ($110bn):** the Amazon cloud commitment.

$121.3bn plus $161.2bn is $282.5bn; $120.5bn, $44.9bn, $44.6bn and $41.3bn add up to $251.3bn; add Amazon's $110bn and you get $643.8bn, or about $644bn (1,213 + 1,612 = 2,825; 1,205 + 449 + 446 + 413 = 2,513; plus Amazon 1,100). The 44%, 39% and 17% shares are also correct.

Two caveats. First, Haircut notes that counting Microsoft as NVIDIA capacity is its own classification. I went back to check, and the S-1/A does say that its partnership with NVIDIA, "together with our arrangements with Microsoft, SpaceXAI, Nscale, Lambda, and other cloud providers," provides NVIDIA GPU capacity, so the grouping is supported by the filing. Second, the seven deals are very different in nature: Google, Amazon and Microsoft are minimum commitments ("at least"); SpaceX and Nscale are caps ("up to"); Broadcom is lease principal excluding interest. Haircut's caption admits it "adds unlike things," but the Chinese commentary turned it into a single item called "compute deals," and that is where the distortion begins.

## 2. The eight deals, one by one

Every figure below comes from the S-1/A. For each deal I note three things: who gets paid, who carries the hardware risk, and when the contract can be terminated.

### Broadcom equipment leases: about $161.2bn

- **What it is:** five-year equipment leases providing TPU compute; the 2026 lease alone is about $125.2bn, excluding interest.
- **How it is paid:** quarterly rent once the equipment is "ready for service"; rent can start earlier if the data center is delayed.
- **Who carries the hardware risk:** Anthropic. At the end of the term, Anthropic pays a redacted price to take title to the equipment (Exhibit 10.11, Section 17.01). On default, the equipment can be sold, and Anthropic remains liable for any deficiency (Section 14.02).
- **Can it be terminated:** neither party can cancel during the lease term, except the lessor upon default.

### Google Cloud: at least $121.3bn

- **What it is:** April 2026 to July 2033, regardless of actual usage; any shortfall must be paid to Google as a true-up.
- **What changed:** the original S-1 said $114.5bn; the S-1/A raised it to $121.3bn.
- **Who carries the hardware risk:** Google, which owns the chips and the facilities.
- **Can it be terminated:** only upon Google's uncured material breach.
- **Also:** additional TPUs are to be procured from 2028 to 2031, with estimated payments of $125bn to $150bn,**not included in the $895.4bn.**

### SpaceX: up to $120.5bn

- **What it is:** NVIDIA GPU capacity over terms extending into 2029, not a ten-year contract. About $3.1bn a month once all capacity is delivered, which is expected in December 2026.
- **Who carries the hardware risk:** SpaceX.
- **Can it be terminated:** after initial non-cancelable periods ending in January 2027 and March 2027, either party can cancel on 90 days' notice.
- **Clarification:** as Haircut's chart also notes, Apollo's $40bn Vera Rubin lease is SpaceX's own deal and unrelated to this compute contract; the prospectus does not mention Apollo.

### Amazon AWS: at least $110bn

- **What it is:** May 2026 to April 2036, regardless of actual usage, with a true-up for any shortfall.
- **Who carries the hardware risk:** Amazon.
- **Can it be terminated:** only upon Amazon's uncured material breach.

### Microsoft Azure: at least $44.9bn

- **What it is:** November 2026 to**June 2033** , regardless of actual usage. The original S-1 said May 2033, and Haircut's chart still shows the old date.
- **Who carries the hardware risk:** Microsoft.
- **Can it be terminated:** upon Microsoft's uncured material breach, or tranche by tranche for undelivered tranches.

### Nscale: up to about $44.6bn

- **What it is:** NVIDIA GPU capacity into 2033;**fees accrue only once capacity is delivered and accepted by Anthropic.**
- **Who carries the hardware risk:** Nscale and its lenders. Nscale must use best efforts to obtain qualifying financing for the GPUs and data centers within a set period.
- **Can it be terminated:** if Nscale misses delivery or availability requirements, or upon material breach.

### Lambda: about $41.3bn

- **What it is:** NVIDIA GPU capacity into 2034, also billed only on delivery and acceptance.
- **Who carries the hardware risk:** Lambda and its lenders.
- **Can it be terminated:** the August agreement, about $34.9bn, can be terminated by either party if Lambda does not secure the facility lease in time; some agreements can also be cancelled tranche by tranche for missed milestones.

### AMD Helios: expected to exceed $20bn

- **What it is:** purchasing or accessing AMD Helios rack-scale systems from 2027, with no fixed dollar commitment; outside both the $895.4bn and the $644bn.
- **The other side:** AMD has committed to buy up to $5bn of Anthropic's Class N common stock once delivery milestones are met. A supplier becoming a shareholder is another example of circular financing.

## 3. Four mistakes in the viral commentary

### Mistake 1: the 72.5% clause runs the other way

The commentary says Broadcom's remaining asset protection "only kicks in if Anthropic defaults and the racks resell for less than 72.5% of what is owed."

First, the source: 72.5% is **not in the prospectus**. It comes from Broadcom's investor presentation. According to that presentation, a newly formed special purpose vehicle (SPV) raised $63bn: the first-lien portion was arranged by Bank of America, Citi and Morgan Stanley, and Blackstone is the anchor provider of second-lien financing. The SPV uses the money to buy Broadcom XPU accelerators and networking equipment, then leases them to Anthropic.

What Broadcom provides is "residual value support." If Anthropic defaults and the recovered AI racks are worth less than 72.5% of the remaining lease amounts, Broadcom pays the difference to the SPV. Peak exposure is expected to be about $43bn, amortizing to zero on a straight-line basis over five years of lease payments.

So this clause is **Broadcom's promise to the lenders**, not Broadcom's protection. The ones protected are the banks and Blackstone. If five-year-old chips really turn out to be worthless, the first one to pay will be Broadcom, not the banks.

### Mistake 2: the balance sheet is not invisible

The commentary says companies use SPVs and lease agreements to keep capital spending off their financial statements.

The prospectus says the opposite. The Broadcom leases are accounted for as finance leases. The two special purpose entities that signed them are Anthropic's own subsidiaries; their obligations are guaranteed by Anthropic, PBC and secured by the leased equipment. The $451.2bn of leases not yet commenced is fully disclosed, including amounts, start dates and a weighted-average term of about ten years. The company also states that, apart from hosting and lease commitments in the normal course of business, funding commitments to equity method investments, and guaranty and cost reimbursement arrangements with data center developers (all disclosed in the notes), it has no off-balance sheet financing arrangements.

To be precise: finance lease liabilities on the balance sheet at September 30 were only $1.674bn, because most of the equipment has not been delivered yet. As it arrives, the liabilities will move onto the balance sheet one by one. That is "not started yet," not "invisible."

The commentary does get one thing right, though. Leasing instead of buying keeps reported capital expenditures low. Anthropic's capex for the first nine months was only $0.9bn, but that does not mean it is not spending; the money shows up as rent and cloud fees. With companies like this, you cannot look at capex alone.

It also helps to separate two kinds of SPVs. One kind is the two Anthropic subsidiaries on the lessee side. The other is the bank-funded SPV to which Broadcom assigned part of its lease rights in October, which sits on the lessor side. The commentary mixes the two.

### Mistake 3: the $42bn PIK convertible is not a debt snowball

The commentary says Broadcom is lending its customer $42bn to pay the rent, at 9% paid in kind, so "the debt will balloon over time."

The prospectus terms are:

- The notes can only be used to pay quarterly rent under the 2026 lease, and only "if needed."
- The draw period has not started; it begins when the equipment becomes serviceable. Nothing was outstanding at September 30, and no notes are expected before the IPO.
- Issuing notes requires meeting conditions, including revenue growth thresholds (set well below the company's actual growth), maintaining at least $5bn of liquidity at each quarter-end, and continued performance under the leases.
- Interest is 9%, paid in kind by adding it to principal; if the revenue conditions are not met, the rate rises to 13% and the conversion price drops from 90% to 85% of the 30-day average price.
- Holders can convert the notes into Class N common stock.

So it works more like an "equity backstop for rent." If it is ever used, the cost is dilution for shareholders, not unlimited debt growth. The signal to watch for is this: **if Anthropic ever starts drawing on this facility, it means its cash flow is not keeping up with the rent.**

### Mistake 4: two cushions were left out

The commentary describes every commitment as having "no way out," but the prospectus discloses that about $213.8bn of commitments are subject to exercisable termination rights or contingent conditions before they start, such as the counterparty's ability to secure financing, facilities or regulatory approvals. In addition, contracts such as Nscale and Lambda only start billing on delivery and acceptance; if the supplier cannot deliver, Anthropic does not pay.

Of the $69.8bn due within 12 months, $4.3bn also falls into this conditional or terminable category.

## 4. Three things the commentary gets right

Having pointed out the mistakes, fairness requires giving credit too. The commentary makes three points worth remembering.

First, **buying compute has become a capital-structure exercise.** We used to think a tech company buying cloud capacity was like paying a utility bill. Now a single contract can run past $100bn, with bank syndicates, private credit funds, special purpose entities, convertible notes and residual value guarantees behind it. To analyze an AI company, technology and revenue are no longer enough; you also have to read its balance sheet and its contract terms.

Second, **SpaceX's 90-day exit clause really does show how both sides respect the technology roadmap.** Each generation of NVIDIA chips is much stronger than the last, and nobody wants to be locked into one architecture. That is also why I see NVIDIA demand coming in through neoclouds as the least certain piece.

Third, **risk really does travel down the contract chain.** The commentary says neoclouds such as Nscale and Lambda, and their lenders, carry risk, and that is correct. They have to borrow to buy GPUs and lease data centers first, then wait for Anthropic's acceptance before they get paid. If Anthropic scales back or delays acceptance, they are the first to feel the squeeze.

As for the commentary's three reasons why the industry evolved such complex structures, here is my scorecard: "transferring hardware depreciation risk" is half right, because the residual value of the Broadcom leases still sits mainly with Anthropic, and only in the Google, Amazon, Microsoft and neocloud deals does the hardware risk truly sit with the supplier; "forced ecosystem lock-in" is broadly right, since Google and Amazon use pay-regardless-of-usage terms to secure a long-term customer; and "making the balance sheet invisible" does not hold, for the reasons above.

## 5. Can Anthropic actually pay?

Start with the revenue side. All figures are from the S-1/A:

- **FY2025 revenue:** $4.59bn.
- **Revenue for the first nine months of 2026:** $33.24bn.
- **Q3 2026 revenue:** $16.96bn (about $17.0bn), up 1,127% year over year, or about $68bn annualized.
- **Nine-month operating loss:** $1.26bn. Q2 operating income was $974m, but Q3 swung back to an operating loss of $567m.
- **Nine-month compute and infrastructure cost:** $18.81bn, about 57% of revenue.
- **Nine-month operating cash flow:** +$6.57bn (versus â$2.02bn a year earlier); after $0.93bn (9.3) of capex, free cash flow was about $5.6bn (estimate).
- **Money on hand at September 30:** $95.6bn of cash plus $36.3bn of short-term investments, $131.9bn in total, plus $2.6bn of restricted cash, mainly collateral for the leases. Cumulative capital raised: $141.1bn.

Then the spending side:

- The $69.8bn due within 12 months is roughly equal to Q3 annualized revenue and roughly half of cash plus short-term investments. **There is no short-term problem.**
- $447.5bn falls due before the end of 2030, and substantially all of the rest falls between 2031 and 2045.

Here is a simple stress test (all estimates). From Q4 2026 through the end of 2030 is a little over four years, so $447.5bn works out to about $105bn a year. If Anthropic wants compute costs to stay around 57% of revenue, the level of the first nine months of this year, it would need average annual revenue of close to $185bn over that period, about 2.7 times Q3 annualized revenue.

Is that a high bar? Given its growth over the past year, not really; but if growth falls quickly to 20% or 30% a year, it will become a struggle. In other words, **what decides whether this bill is healthy is not how much cash it holds today, but whether revenue can double or triple again over the next three years.** That is why I call this a bet that revenue must catch up with compute.

As for the $104.0bn net loss for the first nine months, most of it comes from $94.3bn of fair-value changes on convertible notes and similar instruments, plus the fair-value recognition of partner financing arrangements, all non-cash items. The higher the company's valuation, the worse this line looks; it will disappear once the instruments convert to common stock at the IPO. It should not be read as the core business burning cash.

There is one signal I will watch closely, though: **quarter-on-quarter revenue growth is slowing.** Q1 2026 revenue grew about 132% from the previous quarter, Q2 about 144%, and Q3 slowed to about 47% (estimates). Year-over-year growth is still a stunning 1,127%, but the commitments were signed on the assumption of continued rapid growth. Dropping from triple digits to double digits is normal; the question is whether growth can hold at a high enough level.

## 6. Where the real risks are

I agree with the commentary's central question: **what will a five-year-old TPU be worth?**

Under the Broadcom leases, most of that risk sits with Anthropic: it pays to take title at the end of the term, and on default it must cover any shortfall from selling the equipment. Broadcom's $43bn of residual value support protects the lenders, not Anthropic. In other words, if chips lose value faster than expected, Anthropic absorbs the first losses, and Broadcom only steps in for the portion beyond 27.5%.

The second risk is concentration. Google, Amazon, Microsoft, Broadcom and AMD are Anthropic's suppliers and also its shareholders or potential creditors. I discussed this circular financing in my October 30 article, "Anthropic's IPO Filing, Dissected": money flows from shareholders into Anthropic, then back to those shareholders as compute fees. When Anthropic does well, their backlogs look great; if Anthropic stalls, those backlogs shrink together.

The third risk is cross-default. The prospectus states that the leases include customary events of default, including cross-defaults, upon which obligations may be accelerated. That is exactly the "chain reaction" the commentary describes, and on this point it is right; the trigger, however, is revenue actually failing to keep up.

## 7. Why my numbers differ from the October article

When I wrote "Anthropic's IPO Filing, Dissected" on October 30, I used the original S-1. The total commitments field was blank at the time, so I added up the major contracts myself and estimated about $676bn to $701bn. The S-1/A now gives an official total of $895.4bn, because it also counts leases not yet commenced, commitments signed after September 30 and short-term commitments.

Other differences: Google rose from $114.5bn to $121.3bn; Microsoft's end date moved from May 2033 to June 2033; and the financials were updated from the first half to the first nine months. The October article said the money would "become lease liabilities, or sit in off-balance-sheet purchase commitments," meaning "not yet recorded but disclosed," which does not conflict with this article's point that nothing is invisible.

## 8. What it means for the related stocks

Viewed through my ten stock-selection questions, here is the direction only; this is not a recommendation to buy or sell.

- **Google (GOOGL), Amazon (AMZN):** they hold long-term contracts that must be paid regardless of usage, with true-ups for shortfalls, so their backlog visibility is the highest, and the hardware stays in their own hands. The risk is concentration in a single customer that is partly funded by their own investments.
- **Broadcom (AVGO):** on top of selling chips, it now carries up to about $43bn of residual value support exposure. Broadcom says this is not a direct cash outlay and has a low probability of being triggered, but investors should track it as a contingent liability and check that it declines on schedule as rent is paid.
- **NVIDIA (NVDA):** demand coming through SpaceX, Nscale and Lambda is the least certain. SpaceX can cancel on 90 days' notice from 2027, and Nscale and Lambda need financing or facilities first. That does not make the demand fake, but this part of the order book is easier to adjust. At the November 17 earnings report, watch how management talks about these neocloud customers.
- **AMD:** the Helios agreement has no fixed dollar amount and only starts in 2027, so it behaves more like an option. AMD has also agreed to buy Anthropic stock, so it is important to separate real demand from mutual support.

## 9. Will history repeat? Railways, fiber and this time

The commentary compares today with the 19th-century railway mania and the fiber build-out around 2000. I think the analogy is fair, but the differences matter.

What is the same: the infrastructure did change the world in the end, but the early capital structures were often restructured because they borrowed too heavily against the future. Too much fiber was laid, many companies went bankrupt, and the internet became cheaper as a result.

What is different comes down to two things. First, as I wrote in "Dot-com Bubble vs. AI Wave," almost no internet companies were profitable back then, unlike today, when the big companies leading AI infrastructure are already making money. The main funders this time are Google, Amazon and Microsoft, companies with enormous cash flow, not startups living on fundraising. Second, Anthropic's own revenue and operating cash flow are real and have already turned positive.

But I will not say "this time is different." Fiber in the ground can be used for twenty years; the economic life of GPUs and TPUs is much shorter. The shorter the asset's life, the higher the bar for revenue growth. That is what deserves the most caution in this cycle.

## 10. Using the ten questions to write down when I would admit I'm wrong

I like to turn views into conditions that can be tested. For Anthropic and its supply chain, these are the things I will track:

1. **Is revenue still accelerating?** If quarterly revenue growth falls below 20% for two quarters in a row while commitments keep rising, I would lower my view of the whole supply chain.
2. **Are orders leading revenue?** Watch whether Google Cloud and AWS backlogs keep growing, and whether Anthropic's new commitments start to slow.
3. **Is free cash flow keeping up?** Can operating cash flow cover rent and cloud fees as they rise each year?
4. **Has the PIK convertible been drawn?** If it is, cash flow is getting tight. This is the warning sign I care about most.
5. **How much does the IPO raise?** The more it raises, the thicker the short-term cushion, but it also means the commitments rely more and more on capital markets.
6. **Is Broadcom's residual value support declining on schedule?** If it declines, rent is being paid and risk is falling.

If these signals move in the right direction, today's commitments are a land grab for capacity. If they move the other way, this web of contracts will face the reckoning the commentary describes.

## Closing

The most valuable thing about the viral post is its reminder that AI compute has moved from simple purchasing to credit and lease engineering, and I fully agree with that. But it gets the direction of the risk backwards: the protected party is not Broadcom but the banks; the residual value is borne not by the suppliers but by Anthropic; and the $42bn is not a debt snowball but future equity dilution.

My conclusion: this is not evidence that a bubble is bursting, but a bet that revenue must catch up with compute. The stakes are huge, but for now Anthropic's hand is not bad. Over the next year, watch revenue growth and whether that convertible facility is drawn, and you will know which way this bet is going.

**Disclaimer:** This article is personal research and opinion only and does not constitute investment advice or an offer to buy or sell any security. Figures have been checked against primary sources wherever possible but may contain errors or become outdated; items marked "estimate" are the author's own calculations. Markets carry risk; please make your own judgment and consult a professional adviser where appropriate.

**Secondary sources (used only as a starting point for discussion):** Haircut's chart "Google's TPUs are 44% of Anthropic's $644bn of deals," and the Chinese-language commentary circulating online.

### Primary sources

- Anthropic S-1/A (2026-11-02): https://www.sec.gov/Archives/edgar/data/2130357/000162828026068676/anthropicpbc-sx1ax1.htm
- Anthropic original S-1 (2026-10-26): https://www.sec.gov/Archives/edgar/data/2130357/000162828026067168/anthropicpbc-sx1.htm
- Master Equipment Lease Agreement between Broadcom and AI Infrastructure OpCo II (S-1 Exhibit 10.11): https://www.sec.gov/Archives/edgar/data/2130357/000162828026067168/exhibit1011-sx1.htm
- Broadcom investor presentation ($63bn SPV and residual value support): https://investors.broadcom.com/static-files/f4bda79d-cc66-4748-919b-40d08c658f2d
