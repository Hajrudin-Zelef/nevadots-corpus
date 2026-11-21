---
id: collect-261121-veille-ia/gemini-google/google-tpus-are-coming-to-oracle-cloud-what-the-2027-deal-means-for-ai-compute-asplatx-autonomous-cl
title: "Google TPUs Are Coming to Oracle Cloud: What the 2027 Deal Means for AI Compute | ASPlatX Autonomous Cloud"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-27"]
keywords: ["google", "oracle", "cloud", "tpus", "chips", "compute", "says", "deal"]
source: https://www.asplatx.com/blog/google-tpus-oracle-cloud-infrastructure-2027-ai-compute
source_anchor: ""
source_lines: [1, 102]
section: "Gemini_Google"
---
Oracle will offer Google's Tensor Processing Units on Oracle Cloud Infrastructure, making OCI the first major cloud outside Google to rent out the custom AI chips. The two companies announced the deal on October 27, 2026, at Oracle AI World in Las Vegas, with capacity expected to start arriving in calendar 2027 and ramp up over several years.

For years, TPUs have been something you could only use inside Google Cloud. Opening them to a rival cloud changes the AI accelerator market, which has been dominated by Nvidia GPUs. Oracle also used the event to line up chips from OpenAI, Cerebras, Positron and SambaNova next to its Nvidia fleet.

## Key facts at a glance

- **The deal:** Google TPUs will be available on OCI, the first hyperscaler outside Google to offer them; announced October 27, 2026.
- **Timing:** Capacity begins in calendar 2027 and scales over multiple years; no pricing yet.
- **The chips:** Google's eighth-generation TPU 8t for training and TPU 8i for inference, up to 12.6 petaflops of FP4 compute per chip.
- **Efficiency claims:** Google says up to 2x better performance per watt than the prior generation, and TPU 8t delivers up to 2.7x better performance per dollar.
- **Market reaction:** Oracle shares rose nearly 5% after the news, while Cerebras gained about 8%.

## Background: why TPUs stayed inside Google

Google designed its first TPU more than a decade ago to run its own search, translation and ad models. That first chip offered about 92 teraflops. Since then, TPUs have powered Google's Gemini models and have been sold to outside customers only as a Google Cloud service.

Nvidia, meanwhile, became the default supplier for nearly every other AI lab and cloud. Demand for its chips has outstripped supply, which pushed large AI buyers to look for second sources. Oracle has been one of the most aggressive builders of AI capacity, signing huge infrastructure contracts while its stock has fallen more than 30% this year on worries about the cost of that build-out.

Amin Vahdat, Google's senior vice president and chief technologist for AI and infrastructure, said the next wave of AI will need a diverse range of compute, and that putting TPUs on OCI gives more organizations access to them. Mahesh Thiagarajan, president of Oracle Cloud Infrastructure, said different AI workloads have different needs, so customers should be able to pick the right chip for the performance and economics of each one. Under the deal, Oracle supplies the power, cooling and data center networking to run the TPUs at scale.

## What Oracle customers will get

**The eighth-generation TPU line is the product on offer.** Google splits it into two chips. TPU 8t is aimed at training large models, and TPU 8i is tuned for inference, the work of running a trained model to answer requests.

Oracle says TPU 8t delivers nearly 3x the compute per pod of the previous generation and can cut frontier model development from months to weeks. Google says each chip delivers up to 12.6 petaflops of FP4 compute, which is more than 100 times the first TPU. FP4 is a low-precision number format that lets chips run AI math faster with less memory. Google says the TPU 8t offers up to 2.7x better performance per dollar than the previous generation, and the TPU 8i up to 80% better.

**Reliability is a big part of the pitch.** Google says its TPU systems achieve more than 97% "goodput", meaning the share of time a training cluster spends doing useful work instead of recovering from faults. It also says its newer network design cuts the number of hops between chips by more than 50%, which reduces delays on large jobs.

Software has long been the barrier to adopting TPUs, since most AI code is written for Nvidia's CUDA platform. Google says it has open-sourced TorchTPU so PyTorch developers can run their code on TPUs with fewer changes. That matters for Oracle customers who will not want to rewrite models to try a new chip.

## Oracle's wider "open silicon" line-up

**TPUs are one piece of a broader menu.** At the same event, Oracle said it will host several other accelerators on OCI alongside Nvidia hardware:

| Chip | Maker | Status on OCI | 
|---|---|---|
| TPU 8t / 8i |  | Calendar 2027, multi-year ramp | 
| Vera Rubin NVL72 | Nvidia | Generally available | 
| JalapeÃ±o | OpenAI, with Broadcom | Announced, no date | 
| Wafer-Scale Engine | Cerebras | 2027 | 
| Atlas | Positron | Generally available now | 
| SN50 | SambaNova | Announced; about 20 kW per rack | 

The OpenAI chip, code-named JalapeÃ±o, is being developed with Broadcom. Oracle did not give a date for when it will be available. The SambaNova system stands out for its low power draw of about 20 kilowatts per rack, far below the liquid-cooled racks used for top Nvidia systems.

Thiagarajan rejected the idea that this is a move away from Nvidia. He described Nvidia GPUs as still the most flexible option for customers and said the goal is to add choice, not replace one supplier with another.

## Why this matters for the chip race

**For Google, it is a new distribution channel.** Renting TPUs only through Google Cloud limited their reach. Putting them inside Oracle's data centers lets Google sell its chip to customers who are locked into Oracle for databases, applications or existing AI contracts.

**For Oracle, it is leverage.** Having several suppliers can help Oracle secure capacity and negotiate on price at a time when it has committed to massive spending. Investors appeared to like the idea, sending the stock up nearly 5% on the day.

**For Nvidia, it is another sign of growing competition.** Custom chips from Google, Amazon, Microsoft, Meta and now OpenAI are all aimed at the same AI budgets. Nvidia still holds a strong lead in software and supply, but the number of credible alternatives on a single major cloud is now unusually large.

## What it means for you

**If you are an Oracle Cloud customer,** nothing changes today. TPU capacity is not expected until 2027, and Oracle has not announced pricing, regions or instance shapes. Teams planning 2027 training or inference budgets should ask their Oracle account team about early access.

**If you build with PyTorch,** now is a reasonable time to test TorchTPU on Google Cloud. Code that already runs on TPUs should move to OCI more easily once capacity arrives.

**If you buy AI compute for a business,** the main benefit is choice. Multiple accelerators on one cloud could let you run training on one chip and inference on a cheaper one, without moving data between providers.

## The skeptics' view

Industry analysts welcomed the announcement but warned it is a long way from proof. Brendan Burke, an analyst at Futurum Group, said Oracle will need to show that these alternative chips are competitive on real workloads and that it is committed to supporting them for the long term. Customers have been burned before by niche hardware that lost vendor support.

Matt Kimball, an analyst at Moor Insights & Strategy, described Oracle's strategy as an "open silicon cloud", but the approach adds complexity. Each chip has its own software stack, tools and quirks, and spreading engineering effort across six accelerator families is harder than optimizing for one.

Timing is the other concern. With TPU capacity not arriving until 2027, and some chips still without dates, the near-term impact on what customers can actually rent is limited.

## What happens next

Oracle is expected to share more about its AI infrastructure plans and spending at its upcoming investor day. Customers will be watching for TPU pricing, which regions get capacity first, and how Oracle will bill for the different chips.

Oracle's data center expansion also faces local hurdles. An air-quality hearing for its Project Jupiter campus in New Mexico is set for December 7. On the Google side, the next signals will be whether other clouds follow Oracle in offering TPUs, and how quickly TorchTPU closes the software gap with CUDA.

## Conclusion

Google's decision to put TPUs inside Oracle's cloud breaks a decade-long pattern of keeping its chips in-house. It gives Oracle a deeper bench of AI hardware and gives Google a new way to sell silicon, but real availability is still more than a year away.

Takeaways:

1. **OCI is the first outside hyperscaler to offer Google TPUs,** with capacity starting in calendar 2027.
2. **The chips are TPU 8t and 8i,** with Google claiming up to 12.6 petaflops of FP4 per chip and up to 2.7x better performance per dollar for training.
3. **Oracle is building a multi-vendor menu** including Nvidia, OpenAI, Cerebras, Positron and SambaNova hardware.
4. **No pricing or regions yet,** so treat 2027 plans as provisional.
5. **Software is the swing factor:** TorchTPU's maturity will decide how easily PyTorch teams can switch.

## FAQ

**Can I rent Google TPUs on Oracle Cloud today?**

No. Oracle says capacity begins in calendar 2027 and will scale over several years.

**Is Oracle dropping Nvidia?**

No. Oracle says Nvidia GPUs, including Vera Rubin NVL72 systems, remain a core part of OCI and that the new chips add choice.

**Do I need to rewrite my models for TPUs?**

Not always. Google has open-sourced TorchTPU to help PyTorch code run on TPUs with fewer changes, though some tuning may still be needed.
