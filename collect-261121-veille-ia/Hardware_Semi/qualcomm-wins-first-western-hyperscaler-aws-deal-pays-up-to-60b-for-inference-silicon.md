---
id: collect-261121-veille-ia/hardware-semi/qualcomm-wins-first-western-hyperscaler-aws-deal-pays-up-to-60b-for-inference-silicon
title: "Qualcomm Wins First Western Hyperscaler: AWS Deal Pays Up to $60B for Inference Silicon"
domain: hardware-semi
role: reference
task: article
actors: []
dates: ["2026-09-08"]
keywords: ["qualcomm", "inference", "first", "bandwidth", "silicon", "deal", "memory", "data"]
source: https://www.techtimes.com/articles/326993/20260908/qualcomm-wins-first-western-hyperscaler-aws-deal-pays-60b-inference-silicon.htm
source_anchor: ""
source_lines: [1, 87]
section: "Hardware_Semi"
---
Amazon Web Services on Tuesday became Qualcomm's first Western hyperscaler customer for custom AI inference silicon, signing a multi-generational co-development agreement in which Amazon received a warrant to acquire up to 25 million Qualcomm shares — the full block vesting only if Amazon spends up to $60 billion on Qualcomm chips, networking hardware, and manufacturing services through September 2036. The deal is disclosed in Qualcomm's SEC Form 8-K, filed September 8, 2026. The arrangement is not a forward-looking promise: Qualcomm CFO Akash Palkhiwala confirmed at the Goldman Sachs Communacopia & Technology Conference the same morning that revenue from the partnership will begin in Qualcomm's fiscal first quarter of 2027 — the December 2026 quarter — with chips already in production.

QCOM shares surged as much as 9.5% in Tuesday's session, touching an intraday high of $183.49 — their strongest single-day gain in years and the stock's first positive return for the year — while Amazon shares slipped roughly 1% as investors weighed the company folding a new silicon supplier into its AWS infrastructure stack.

The partnership has two product tracks. The first is customized AI inference silicon co-designed across multiple chip generations. The second is a joint effort to develop optical connectivity hardware scaling to 1.6 terabits per second (1.6T) for intra-cluster networking — the backbone that moves data between racks inside hyperscale data centers.

### What the Deal Actually Covers — and Why LPDDR Matters

Both tracks reflect a deliberate architectural choice that separates Qualcomm from every other inference competitor: it uses LPDDR memory rather than the HBM (High Bandwidth Memory) that powers Nvidia's B200 accelerators and AMD's Instinct MI350X cards. According to Qualcomm's official chip specifications, Nvidia's flagship B200 carries approximately 180 gigabytes (GB) of HBM3e per card; Qualcomm's AI200 supports up to 768GB of LPDDR per card — more than four times the capacity. HBM delivers dramatically higher peak memory bandwidth than LPDDR, which is why it dominates training workloads where massive parallel matrix operations occur simultaneously. But inference — specifically the "decode" phase, in which an AI model generates one token at a time in sequence — is not bandwidth-bound in the same way. It is memory-capacity-bound: the bottleneck is whether the entire model can stay resident on the card so the system avoids expensive data transfers while serving users.

Qualcomm's bet is that decode workloads reward capacity over peak bandwidth, and that LPDDR delivers sufficient bandwidth for those workloads at a dramatically lower cost per gigabyte than HBM. The AI250, Qualcomm's next-generation card expected in calendar 2027, takes this further with a "near-memory computing" architecture that stacks compute logic physically adjacent to memory, collapsing data travel distance. Qualcomm's official specifications indicate the AI250 delivers more than 10x higher effective memory bandwidth compared with conventional DRAM configurations at significantly lower power.

Palkhiwala was specific about this at the Goldman Sachs conference: "We have this innovative technology that we call High Bandwidth Compute, which is really kind of stacking compute and memory together to deliver extremely high bandwidth that our customers see as a perfect solution for certain kind of decode workloads within inference." He added that the technology will extend into prefill workloads and the broader inference stack over time.

### The Optical Layer: Why Qualcomm Bought Alphawave

The 1.6T optical connectivity component of the deal traces directly to Qualcomm's $2.4 billion acquisition of Alphawave Semi, announced in June 2025 and completed approximately December 2025. Alphawave is a specialist in high-speed serializer/deserializer (SerDes) IP — the technology that converts parallel chip-internal data into the serial format required for ultra-fast external communication.

Alphawave's portfolio includes PCIe Gen 6, CXL 3.0, and Ethernet IP rated for 400G, 800G, and 1.6T data rates, along with chiplet interconnect standards such as UCIe. In March 2026, Qualcomm and Lightmatter jointly demonstrated 1.6 Tbps per-fiber optical throughput using a 16-wavelength dense-wavelength-division multiplexing (DWDM) design — claimed to deliver eight times higher bandwidth density per fiber than competing near-packaged optics approaches.

What makes pairing inference compute with optical transport within a single partnership architecturally significant is the scale-out problem: as AI model sizes grow, the bandwidth demands on the network fabric that connects racks inside a data center become a primary bottleneck, separate from the compute silicon itself. A partner that controls both the inference chip and the data transport layer can optimize the full data path end-to-end rather than managing integration between products from separate vendors. Palkhiwala framed this at the Goldman Sachs event: "Even our custom silicon engagement includes Alphawave SerDes, and they have optical connectivity products that are also a part of the Amazon agreement."

### What This Deal Is — and What It Isn't

AWS operates two of its own captive AI accelerator programs: Trainium (for model training) and Inferentia (for inference). Research on Marvell's co-design role in AWS silicon confirms that those programs serve AWS's internal AI workloads and the infrastructure it sells as Trn and Inf instance types on its public cloud. Qualcomm's custom silicon sits in a different category: it would be incorporated into AWS's broader cloud infrastructure offering — the chips that run third-party and commercial inference workloads at scale — rather than replacing Trainium or Inferentia internally.

This distinction matters for understanding why AWS added Qualcomm rather than simply expanding Trainium. The inference market is disaggregating by workload sub-type. Prefill workloads — the initial processing of a user's input — and decode workloads — sequential token generation — have meaningfully different hardware requirements. A hyperscaler running inference at Amazon's scale has an economic incentive to route different workload types to different silicon architectures to minimize per-token cost. Qualcomm's LPDDR-based design, optimized for decode and memory capacity, represents a complementary tier rather than a competing alternative.

Palkhiwala also confirmed at the Goldman Sachs conference that a second, undisclosed global hyperscaler is engaged with a "similar technology scope" to the Amazon deal — suggesting the AWS agreement is structurally repeatable rather than a one-time win.

### The Warrant Structure: Pay-to-Play at Scale

The equity warrant structure disclosed in Qualcomm's SEC Form 8-K is notably precise about accountability. Amazon's affiliate received the right to purchase up to 25 million QCOM shares at an exercise price of $161.26 per share, with the warrant expiring September 3, 2036, and cashless exercise permitted. At $161.26, the maximum warrant represents approximately $4 billion in face value.

Shares vest in tranches tied directly to commercial activity: execution of binding agreements, placement of purchase orders, and actual purchases of Qualcomm server chip products, technology, systems, and manufacturing services. The 3.75 million shares that vested immediately (15% of the total) were tied to Amazon's initial purchase commitments. The remaining 85% vest only as Amazon spends — up to the $60 billion ceiling over the decade.

That structure functions as a mutual performance guarantee. Amazon's equity upside is maximized only if Qualcomm delivers at scale; Qualcomm's revenue is maximized only if Amazon's AI infrastructure build continues at pace. Neither company benefits from the other's failure. This "pay-to-play" approach, in which warrant vesting is milestone-linked rather than time-linked, is emerging as a template for major hyperscaler chip partnerships — a structural analog to the warrant Amazon issued against STMicroelectronics in a separate semiconductor supply arrangement earlier in 2026.

### The Strategic Context: Decoupling from Apple

Tuesday's deal arrives at a structurally important moment for Qualcomm. The company generates the majority of its revenue from smartphone chips and modem IP, with Apple historically accounting for a significant portion of that business. Apple's rollout of its in-house C1 modem — first deployed in the iPhone 16e — is the opening move in a planned phase-out that CEO Amon confirms will eliminate Qualcomm modems by 2027. Qualcomm's model share in Apple devices is expected to fall from approximately 70% in 2025 iPhones to 20% in 2026 iPhones to zero in 2027.

To compensate, Qualcomm set out an aggressive data center pivot at its June 24, 2026 Investor Day in New York, raising its $40 billion non-handset target by fiscal 2029 and setting a specific data center AI revenue goal of more than $15 billion by that year. The $5 billion data center revenue target for fiscal 2027 — which Palkhiwala described at Goldman Sachs as being held with "very high confidence" — is now grounded in confirmed purchase orders from the Amazon partnership.

Automotive revenue has also been a bright spot, reaching record levels for two consecutive quarters, with Qualcomm projecting it will become the largest chip supplier to the automotive industry next year. But the Amazon deal is what moves the data center narrative from projection to execution: for the first time, a major Western cloud provider has committed to Qualcomm silicon — not to look at it, as AWS did in preliminary discussions back in 2022, but to buy it at production scale with chips already being built.

### How Does Qualcomm Stack Up Against Nvidia?

Nvidia holds more than 90% of AI training in data centers, with Qualcomm holding no meaningful training presence and no plans to build one. Amon has been explicit: Qualcomm cannot create a CUDA ecosystem from scratch and will not try. The relevant competitive battlefield is inference.

In inference, industry analysts estimate Nvidia's share could decline from roughly 90% to somewhere between 20% and 30% by 2028 as hyperscalers route internal inference workloads to captive silicon — but most of that erosion accrues to the hyperscalers themselves (Trainium, TPU, Maia, MTIA), not yet to third-party inference providers like Qualcomm. The commercial inference market — chips that external customers and cloud tenants access — remains dominated by Nvidia's GPU cloud offerings.

Qualcomm's entry represents a new category: a third-party inference-specialist chip available through a major hyperscaler's infrastructure, benchmarked on per-token cost rather than peak FLOPS, and differentiated by memory capacity and power efficiency rather than raw compute throughput. Analyst firm Rosenblatt Securities maintained its Buy rating on Qualcomm with a $225 price target following the announcement.

Broadcom shares rose approximately 3% on Tuesday — a market reading widely interpreted as validation that the custom AI silicon category as a whole is gaining institutional credibility among hyperscalers, rather than a sign that existing Broadcom programs at Google and Meta are under pressure from Qualcomm's entry.

### What AMD and Marvell Are Watching

AMD has been Nvidia's most credible large-scale competitor in inference, with its Instinct MI350X carrying 288GB of HBM3e and deploying across multiple hyperscaler partnerships. Its inference revenue has grown meaningfully in 2026, and its ROCm software platform, while still behind CUDA in ecosystem depth, has matured. Marvell, which co-designs AWS Trainium as well as Microsoft's Maia accelerator, has guided AI revenue substantially above its original $2.5 billion fiscal 2026 target.

Qualcomm's differentiation relative to AMD is architectural: LPDDR vs HBM, memory capacity vs memory bandwidth, decode-efficiency vs general-purpose inference. Whether that tradeoff holds in production is the question the next several quarters will answer, as December shipments to AWS begin turning up in Qualcomm's revenue line.

### Is There a Software Problem?

Qualcomm's most frequently cited competitive liability in data center has been software. Nvidia's CUDA ecosystem — the parallel computing platform and programming model that runs across virtually every major AI research framework — creates enormous switching costs for customers and developer lock-in that has arguably done more to sustain Nvidia's market position than any hardware advantage.

Qualcomm's answer arrived in a separate June 2026 announcement: the acquisition of Modular Inc., a company co-founded by former Apple and Google compiler engineers whose software stack is designed to be cross-platform — porting models once and running them across Nvidia, AMD, Qualcomm, AWS, and Apple silicon without performance penalty. Palkhiwala described Modular's approach to the Goldman Sachs audience as "an Android-like approach, where we are making the stack available to everyone in the industry to deploy" — a bet that making the lower software layers open and portable will reduce the developer-level advantage that CUDA currently provides. Whether that bet proves correct depends entirely on whether hyperscale customers and AI developers adopt Modular's toolchain at scale, which remains to be seen.

Qualcomm will also expand its own use of AWS cloud infrastructure — including Amazon Bedrock — to accelerate its internal chip design workflows, shortening development cycles. That reciprocal dimension is unusual in supplier-customer relationships and signals the partnership is structured as a genuine engineering collaboration rather than a procurement arrangement.

## Frequently Asked Questions

### What does the $60 billion figure actually mean — is Qualcomm guaranteed that revenue?

No. The $60 billion is the ceiling on the warrant structure, not a committed revenue contract. Amazon's affiliate received the right to purchase 25 million Qualcomm shares at $161.26 apiece, but those shares vest only in tranches tied to actual purchases. If Amazon spends less than $60 billion with Qualcomm over the ten-year term, fewer shares vest and less revenue flows to Qualcomm. The 3.75 million shares that vested immediately reflected Amazon's initial commitments; the remaining 85% vest only as Amazon follows through with binding purchase orders and actual chip buys. What is confirmed is that revenue begins in the December 2026 quarter, with chips already in production.

### Why does Qualcomm use LPDDR memory instead of the HBM used in Nvidia and AMD AI chips?

HBM (High Bandwidth Memory) delivers very high peak bandwidth and is optimized for AI training, where massive numbers of operations happen simultaneously and raw data throughput determines speed. AI inference — particularly the "decode" phase where a model generates text one token at a time — is structured differently: the bottleneck is whether the full model can stay resident in memory without requiring expensive data transfers. LPDDR costs significantly less per gigabyte than HBM, allowing Qualcomm to offer 768GB per card versus approximately 180GB on Nvidia's B200. For serving large language models at scale — where fitting more models in memory reduces cost per user request — that capacity advantage may matter more than peak bandwidth. Qualcomm's AI250 takes this further with "near-memory computing," stacking compute logic directly adjacent to memory to boost effective bandwidth while preserving LPDDR's cost and capacity advantages.

### Does this deal mean AWS is moving away from its own Trainium and Inferentia chips?

No. AWS's Trainium and Inferentia programs — co-designed with Marvell — are internal accelerator programs that AWS built to reduce its own AI infrastructure costs and serve specific training and inference workloads captively. Those programs remain active and are separate from the Qualcomm deal. The Qualcomm silicon will serve AWS's broader cloud infrastructure — the chips available to external customers running inference workloads — and represents a complementary tier for decode-heavy serving rather than a replacement for AWS's in-house programs. Palkhiwala confirmed at Goldman Sachs that Qualcomm's "High Bandwidth Compute" technology is "uniquely positioned to decode workloads within inference," which is a different performance envelope from what Trainium or Inferentia optimizes for.

### When will QCOM shareholders actually see financial results from this deal?

According to Qualcomm CFO Akash Palkhiwala at the Goldman Sachs Communacopia conference on September 8, 2026, revenue from the Amazon deal will begin in Qualcomm's fiscal first quarter of 2027 — the December 2026 quarter. He described the $5 billion data center revenue target for all of fiscal 2027 as being held with "very high confidence" given that chips are already in production. The $15 billion data center revenue target for fiscal 2029 is a longer-term aspiration that assumes the Amazon program, the Meta CPU engagement, and additional hyperscaler relationships scale as projected. Investors should note that the full $60 billion ceiling represents a decade of potential purchases, not a near-term revenue commitment.
