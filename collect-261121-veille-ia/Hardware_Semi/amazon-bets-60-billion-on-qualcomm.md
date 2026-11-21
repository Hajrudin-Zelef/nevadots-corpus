---
id: collect-261121-veille-ia/hardware-semi/amazon-bets-60-billion-on-qualcomm
title: "Amazon bets $60 billion on Qualcomm"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["qualcomm", "amazon", "billion", "silicon", "inference", "warrant", "filing", "custom"]
source: https://www.jonpeddie.com/news/amazon-bets-60-billion-on-qualcomm/
source_anchor: ""
source_lines: [1, 61]
section: "Hardware_Semi"
---
*Qualcomm just landed Amazon as a data center customer, and the regulatory filing tells the real story. The press release describes custom silicon for AI inference plus optical links to 1.6 Tb/s. The SEC filing adds a warrant letting Amazon buy 25 million Qualcomm shares at $161.26, vesting against purchases that ladder up to $60 billion. Here is what Qualcomm actually sells, what Amazon actually promised, and what silicon teams and IT buyers should watch between now and 2028.*


Qualcomm and Amazon signed a multi-generation agreement on September 8, 2026, covering customized silicon for AWS AI infrastructure, with the first phase aimed at inference. The two companies will also develop high-performance optical connectivity extending to 1.6 Tb/s and beyond, using the SerDes and optical DSP technology from Qualcomm’s Alphawave acquisition. Qualcomm plans to deepen its own use of AWS infrastructure, including Amazon Bedrock, for electronic design automation workloads specifically, targeting shorter chip design cycles.

<sup>Table 1. Warrant terms behind the Qualcomm and Amazon agreement.</sup>

The regulatory filing describes a commercial relationship of a different magnitude. Amazon received a warrant to purchase up to 25 million Qualcomm common shares at $161.26 each, roughly $4 billion at issuance. The warrants vest in stages tied to commercial agreements, binding purchase orders, and Amazon’s actual purchases of Qualcomm server chips, technology, systems, and manufacturing services. The full vesting schedule tops out at a maximum threshold of $60 billion of payments from Amazon over the warrant’s life, which runs to September 2036. Qualcomm already vested warrants covering 3.75 million shares at issuance against initial purchase commitments.

Read the $60 billion figure precisely. It sets the ceiling of the vesting ladder. It marks the maximum purchase-based threshold for full warrant vesting, giving both sides real incentive to grow the relationship over time regardless. It carries no purchase commitment, no contract value, and no guaranteed backlog. The structure pays Amazon in Qualcomm equity for buying Qualcomm silicon, which aligns both parties toward volume growth and gives Qualcomm a major reference customer at hyperscale that builds credibility and production scale. Marvell wrote the same shape of deal with Google in August, granting a warrant for about 59 million shares worth up to $12.2 billion.

**The scope runs past an accelerator**

Much of the initial coverage framed this as a custom AI chip deal. The filing describes server chip products, technology, complete systems, manufacturing services, and the optical connectivity that ties AI infrastructure together. That maps onto the Dragonfly portfolio Qualcomm laid out in June 2026, which spans custom silicon, CPUs, AI inference accelerators, and connectivity products, built on strength in energy-efficient processing, digital signal processing, high-speed SerDes, and system-level semiconductor integration.

<sup>Table 2. What Qualcomm brings to AWS racks by 2028.</sup>

Meta signed for the C1000 in June for its next-generation server fleet. Microsoft also backs the data center push. Amazon becomes the third hyperscaler on the list, and the one paying with equity upside.

Silicon teams should focus on the inference framing. Training large foundation models captures most industry attention. The cumulative arithmetic of inference—the repeated execution of trained models against billions of user and application requests—is on track to become the larger long-term computational workload. Inference rewards narrow architectures. A design tuned specifically for particular models, a fixed numeric format, a specific memory hierarchy, and a defined thermal envelope can beat a general-purpose part on tokens per dollar. GPUs deliver broad programmability and a mature, deep software environment. Custom accelerators deliver efficiency inside a smaller problem definition. At Amazon’s fleet size, even modest gains of a few percentage points of performance per watt translate into substantial reductions in infrastructure and operating costs.

**Connectivity carries equal weight**

The optical half of the agreement demonstrates why Qualcomm paid $2.4 billion for Alphawave Semi and closed the deal in December 2025. Alphawave brought high-speed SerDes, optical DSPs, chiplets, and custom silicon expertise, technology Qualcomm explicitly positioned as accelerating its data center expansion by complementing existing Oryon CPU and Hexagon NPU technology with the high-speed connectivity needed to build genuinely larger systems. Former Alphawave CEO Tony Pialis now runs Qualcomm’s data center chip business.

<sup>Figure 1. How Qualcomm compute and optics attach to AWS inference infrastructure.</sup>

AI system performance increasingly depends on moving data between accelerators, memory, servers, racks, and entire buildings, not just on raw processor speed. Accelerators wait on memory, on neighboring dies, on the top-of-rack switch, and on links between buildings. The AI300 scales up over UALink and ESUN, the Ethernet for scale-up networking effort, and scales out over copper inside the rack and optics between racks. A vendor that supplies the accelerator, the CPU, the SerDes, and the optical DSP can co-design across those boundaries. Combining existing compute and AI technology with Alphawave’s connectivity portfolio positions Qualcomm as a provider of multiple technologies required to build and connect complete AI infrastructure, separating it from suppliers offering compute alone or networking alone.

**Amazon keeps its options open**

The agreement does not indicate Amazon is replacing Nvidia GPUs or abandoning its own internally developed processors. AWS already runs one of the industry’s broadest computing architecture collections, spanning Nvidia and AMD accelerators, its own Trainium and Inferentia AI processors, Graviton CPUs, and Nitro infrastructure silicon. Amazon’s custom chip business passed a $25 billion annualized run rate at the end of the June quarter. Adding Qualcomm reinforces Amazon’s ability to match the best architecture to each specific workload, maintaining both architectural and supplier flexibility and negotiating leverage. Qualcomm and Amazon have disclosed nothing about how the joint products relate to the Trainium and Inferentia roadmaps.

Qualcomm previously entered the server processor market through its Arm-based Centriq family, then withdrew before reaching commercial scale. Today’s market runs considerably different: Cloud providers now rank among the world’s largest semiconductor customers, increasingly willing to deploy custom silicon to control cost, power consumption, supply, and product differentiation, and generative AI has created demand for new compute and networking architectures at genuinely unprecedented scale. Qualcomm also enters this market with a far broader asset base than it had during the Centriq era, including Oryon CPU cores, mature AI acceleration technology, an expanding AI software platform, and the SerDes, optical DSP, chiplet, and custom silicon capabilities Alphawave added directly.

<sup>Figure 2. Two technical pillars, one warrant structure: Qualcomm’s real bet on Amazon.</sup>

“As AI demand accelerates, data center infrastructure will require advances in both computing and connectivity to deliver greater performance with more efficiency,” said Cristiano Amon, Qualcomm’s president and CEO.

Prasad Kalyanaraman, AWS vice president, frames the deal from Amazon’s side around continuity: “By working together on customized silicon and advanced connectivity, we’re delivering more performant, efficient, and cost-effective infrastructure for our customers.”

**What this means for buyers and developers**

ISVs get a practical checklist. Qualcomm’s inference stack will need to absorb the model formats and serving frameworks that AWS customers already run, and the porting cost lands on the software layer. They need to ask about compiler maturity, quantization support, kernel coverage for attention variants, and the migration path from CUDA-shaped code.

CIOs and IT decision-makers get a supply question with a 2028 horizon. Neither the C1000 nor the AI300 ships before then, so procurement teams have two years to model the effect on their AWS instance pricing and reserved capacity plans. A third viable inference architecture inside AWS pushes down on per-token cost. It also fragments the tooling surface that platform teams have to support.

*What do we think?*

The warrant structure is the real story here, not the press release. A $60 billion maximum threshold, even as an aspirational ceiling, signals Qualcomm’s genuine confidence in landing sustained Amazon business. The optical connectivity angle deserves equal attention: Qualcomm’s Alphawave acquisition now reads as a deliberate bet that AI infrastructure competition runs as much through data movement as raw compute, and Amazon validating that thesis with real purchase-linked warrants carries weight.

Qualcomm bought credibility with equity, and the price looks reasonable. A warrant costs nothing until Amazon buys, so dilution tracks revenue. The technical case rests on claims Qualcomm has to prove on production workloads: 2× performance per watt on the C1000 and 4× to 8× memory bandwidth per watt on the AI300. Owning the SerDes and optical DSP gives Qualcomm co-design reach that a merchant accelerator vendor lacks. The risk sits in the calendar. Nothing ships until 2028, and Nvidia, AMD, and Amazon’s own Trainium team all get two more generations first. Watch whether Trainium and Inferentia roadmaps shift once Qualcomm silicon actually ships.

*Inflection point.* Qualcomm and Amazon structuring a deal around purchase-linked equity warrants instead of a simple supply contract marks a real inflection point in how AI infrastructure partnerships find financing. Hyperscalers now write equity into supply agreements to seed alternatives to a single GPU vendor, aligning incentives in a way traditional vendor contracts never did. Combined with Marvell’s similar Google warrant deal, a pattern emerges: Cloud providers extract equity upside from every major silicon partnership they sign, and chipmakers accept that exposure to prove they can win hyperscale customers at genuine production scale.

That marks an inference inflection point: The workload has grown predictable enough, and large enough, to justify architectures purpose-built for it and financing structures purpose-built to fund them. Compute alone no longer wins the socket. The winning bids pair accelerators with the SerDes and optics that move data between them. Expect more warrant-backed silicon deals, and expect data movement to set the ceiling on AI performance.

If you’re building an AI processor or following the industry, you should check out our quarterly update service and our enormous AI Processor management information system tracker, which has all the details on the 153 companies, including the $33 billion investment in start-ups, and the technical details of the 330 products serving five segments with 10 types of processors.


<sub>AND IF YOU LIKED WHAT YOU’VE READ, TELL YOUR FRIENDS.</sub>
