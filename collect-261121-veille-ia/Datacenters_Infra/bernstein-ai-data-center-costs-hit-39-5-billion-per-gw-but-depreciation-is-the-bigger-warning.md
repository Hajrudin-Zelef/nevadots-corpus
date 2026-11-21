---
id: collect-261121-veille-ia/datacenters-infra/bernstein-ai-data-center-costs-hit-39-5-billion-per-gw-but-depreciation-is-the-bigger-warning
title: "Bernstein AI Data Center Costs Hit $39.5 Billion per GW, but Depreciation Is the Bigger Warning"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: []
keywords: ["billion", "bernstein", "data", "costs", "gigawatt", "memory", "center", "estimate"]
source: https://www.remio.ai/post/bernstein-ai-data-center-costs-hit-39-5-billion-per-gw-but-depreciation-is-the-b
source_anchor: ""
source_lines: [1, 177]
section: "Datacenters_Infra"
---
Bernstein AI data center costs now reach an estimated $39.5 billion per gigawatt for Nvidia’s Vera Rubin architecture. Yet the headline investment is not the most consequential part of the analysis. The lasting burden comes from depreciating billions of dollars in servers, networking equipment, memory, and power systems before the hardware loses its economic value.

The estimate places different accelerator architectures within a range of $34.6 billion to $39.5 billion per gigawatt. OpenAI’s reported Jalapeno custom ASIC design sits near the lower end, while Nvidia’s Vera Rubin NVL72 platform reaches the top. That relatively narrow spread challenges the idea that choosing a cheaper accelerator can transform the economics of an entire facility.

Bernstein’s earlier work put one gigawatt of AI capacity near $35 billion, below Nvidia’s own estimate of $50 billion to $60 billion. Its latest calculations refine the component assumptions, but the strategic conflict remains the same. Technology companies are pursuing more compute, while accounting costs begin accumulating as soon as the equipment enters service.

## Bernstein AI Data Center Costs Reach $39.5 Billion per Gigawatt

**Bernstein’s latest estimate shows that accelerator choice changes the cost mix, but does not make gigawatt-scale AI infrastructure inexpensive.**

A report summarized on October 10 placed total investment for one gigawatt of capacity between $34.6 billion and $39.5 billion. The calculation covers more than an empty building connected to the grid. It includes accelerators, servers, memory, storage, networking, cooling, backup power, and the electrical systems needed to support dense computing racks.

The upper estimate applies to Nvidia’s planned Vera Rubin NVL72 architecture. Bernstein reportedly reduced its estimated cost for one Rubin rack from $9.1 million to $7.52 million. That is a decline of about 17 percent from the firm’s earlier model.

The revision reflects lower assumptions for high-bandwidth memory and NAND storage capacity, according to the published infrastructure estimate. High-bandwidth memory, usually shortened to HBM, places fast memory close to an accelerator so models can move data without waiting on slower storage.

The reduction matters because memory has become a substantial part of each AI server. However, lower memory assumptions do not remove the surrounding costs. More racks still require switches, network interface cards, power conversion hardware, cooling equipment, transformers, and generators.

OpenAI’s reported Jalapeno architecture appears at the lower end of Bernstein’s range. Jalapeno is described as a custom application-specific integrated circuit, or ASIC, designed around a narrower workload than a general-purpose accelerator. Custom chips can reduce some supplier margins and optimize performance for selected tasks.

Even so, the total difference between the lowest and highest configurations is less than $5 billion per gigawatt. That is below 15 percent of the upper estimate. The result suggests that chip selection reshapes where money goes more than it changes the scale of the commitment.

This point was already visible in Bernstein’s previous analysis of Nvidia’s GB200 systems. That work put total construction near $35 billion per gigawatt, with graphics processors accounting for about 39 percent of capital spending. Networking represented roughly 13 percent, while mechanical and electrical systems approached one-third.

Accelerators therefore remain the largest individual category, but they are not the entire data center. A lower accelerator price does not eliminate the equipment that delivers power, removes heat, and connects thousands of processors.

Bernstein previously estimated that a GB200 NVL72 rack cost approximately $5.9 million. About $3.4 million went toward computing hardware, while $2.5 million covered associated physical infrastructure. Those figures, reported in an earlier cost breakdown, demonstrate why comparisons based only on chip prices can mislead investors.

The unit also needs careful interpretation. One gigawatt describes an enormous continuous power load, not a conventional server room. At full utilization, it represents enough electrical demand to make the project a regional infrastructure concern.

The Bernstein data center estimate consequently provides a common denominator for competing systems. Nvidia, custom ASIC developers, cloud operators, and infrastructure suppliers can all describe superior efficiency. However, investors still need to ask how much productive computing and revenue each complete gigawatt delivers.

That question creates the central tension. The industry has spent years treating access to accelerators and electricity as the binding constraints. Bernstein’s analysis suggests the next constraint is whether operators can earn enough before the installed equipment becomes financially and technically outdated.

## Depreciation, Not Electricity, Creates the Heaviest Recurring Burden

**Electricity attracts public attention, but depreciation determines how quickly AI infrastructure must generate durable revenue.**

Depreciation is the accounting process that allocates an asset’s cost across its expected useful life. It does not require a new cash payment every quarter. However, it reduces reported earnings and represents the consumption of equipment purchased with real capital.

Bernstein’s earlier Vera Rubin model illustrates the scale. At an electricity rate of $0.15 per kilowatt-hour, a one-gigawatt facility would incur approximately $1.3 billion in annual power expense. The same model produced around $7.9 billion in annual depreciation under a six-year hardware life.

Those figures came from a June analysis that then estimated total Vera Rubin infrastructure at $47 billion per gigawatt. Bernstein has since reduced its rack estimate, so the exact depreciation charge would change. The relationship still explains the firm’s warning: ownership costs can outweigh the electric bill even for a facility consuming a gigawatt.

The underlying Rubin cost model assumed 3,557 racks at 220 kilowatts each. It allocated about $32 billion to rack-related equipment and another $15 billion to physical infrastructure. Its six-year schedule converted that initial commitment into a continuing earnings expense.

An accounting life of six years does not guarantee that every accelerator remains competitive for six years. A server can continue functioning after newer systems deliver better performance per watt or lower inference costs. That gap between physical life and economic usefulness is the real risk.

If next-generation hardware sharply lowers the cost of producing an AI response, owners of older systems face pressure to reduce rental prices. Their depreciation schedules remain in place unless the companies extend useful lives or record impairment charges. Neither choice creates more demand for the older equipment.

Extending the useful life reduces annual depreciation and supports near-term earnings. It also assumes that the equipment will remain productive longer. A company that makes this adjustment too aggressively can postpone recognition of deteriorating economics.

An impairment takes the opposite route. It acknowledges that an asset will not recover its recorded value through future cash flows. The charge is noncash when recorded, but it reveals that earlier capital spending will earn less than expected.

This mechanism separates AI depreciation costs from a normal utility bill. Electricity rises with usage, so a lightly used cluster consumes less power. Depreciation continues even when servers wait for customers, software, or network capacity.

Utilization therefore becomes critical. A fully occupied cluster can spread depreciation across more training runs, generated tokens, or cloud contracts. An underused facility carries much of the same ownership burden while producing less revenue.

The timing problem is equally important. Companies usually commit money to land, electrical equipment, chips, and construction before the completed cluster begins serving customers. Delays can leave capital tied up without corresponding revenue, while some installed assets already begin aging.

AllianceBernstein has described a related danger as an information “air pocket.” Revenue evidence can remain incomplete during a period shorter than the depreciation cycle. Investors must then value long-lived spending without knowing whether adoption will broaden quickly enough.

That concern does not establish that the spending is wasteful. Demand for AI training and inference can grow rapidly, and newer systems can process far more work. It does mean that revenue growth, utilization, and hardware efficiency must arrive together.

Depreciation also changes how readers should interpret cloud margins. A provider can report rising AI demand and still face margin pressure as new assets enter service. Cash flow, operating income, and adjusted earnings can tell different stories because each treats capital investment differently.

The burden can move between companies without disappearing. A model developer that rents computing avoids owning every server, but its cloud provider then prices depreciation and financing into the contract. A cloud operator can use special-purpose vehicles or leases, yet another investor still owns the assets.

The central issue is therefore not whether someone can finance the next cluster. It is whether the complete chain can generate sufficient economic output before another hardware generation resets the competitive standard.

## Nvidia and Custom AI Chips Face the Same Capital Reality

**The main contest is not Nvidia versus one custom chip; it is the promise of expanding compute against the reality of depreciating infrastructure.**

Nvidia’s position makes it the most visible beneficiary of the spending wave. Its accelerators, networking products, software, and rack-scale systems capture several layers of a deployment. Higher system performance can also justify more expensive configurations when customers value time to train or serve a model.

Custom ASICs offer a different proposition. Google, Amazon, Microsoft, Meta, and AI laboratories have pursued chips designed for their own workloads. These systems can remove some merchant-chip margins and match hardware more closely to internal software.

Bernstein’s estimates imply that neither route escapes the wider bill. A custom accelerator can cost less, but the operator may deploy more racks with the available budget. Each rack still needs memory, networking, power distribution, cooling, and a physical site.

The result resembles Jevons paradox, where efficiency lowers the cost of using a resource and stimulates additional consumption. Better performance per dollar can reduce the cost of one workload while encouraging developers to run larger models, use longer contexts, or serve more requests.

That dynamic favors infrastructure suppliers, but it complicates the claim that efficiency will automatically reduce capital needs. The industry can convert efficiency gains into more computation instead of lower total spending.

Nvidia’s own estimates show how uncertain these calculations remain. The company previously suggested a range of $50 billion to $60 billion per gigawatt, while Bernstein’s older analysis placed comparable capacity near $35 billion. The gap reflects different hardware generations, definitions, and assumptions about included infrastructure.

Broadcom and AMD have described an addressable accelerator opportunity of roughly $15 billion to $20 billion per gigawatt. That figure does not represent the cost of an entire completed data center. It describes the semiconductor opportunity within the broader project.

These measurements are often repeated as if they answer the same question. They do not. One estimate can cover accelerators, another can cover a complete rack, and another can include power generation or financing.

The latest Bernstein range is useful because it compares complete architectures under one methodology. It is not a universal construction quote. Land prices, grid access, energy systems, utilization targets, financing, and local labor can all alter the final result.

Architecture comparisons must also account for useful output. A more expensive cluster can be economical if it trains a model sooner or serves significantly more requests. A cheaper cluster can disappoint if software support, reliability, or customer demand limits utilization.

Vera Rubin is expected to improve compute performance substantially over Nvidia’s Blackwell generation. Bernstein’s earlier model estimated 2,520 FP8 petaflops per Rubin rack, compared with 720 petaflops for Blackwell. FP8 is an eight-bit numerical format that accelerates AI calculations while accepting less precision than larger formats.

Raw throughput does not translate directly into billable work. Model architecture, memory capacity, communication overhead, software optimization, and uptime affect how much of the advertised performance reaches customers.

The custom-chip route introduces another tradeoff. Operators gain control over hardware design and potentially reduce supplier margins. They also assume development risk, software integration work, and the challenge of maintaining a competitive roadmap.

Nvidia spreads those costs across many customers and supports its hardware with a mature software environment. That can reduce deployment risk even when the purchase price is higher. A hyperscaler with enough internal demand can justify the opposite choice.

The Bernstein data center estimate therefore does not identify a simple winner. It shows that neither architecture changes the order of magnitude. The financial outcome depends more heavily on utilization, revenue, and the speed of hardware replacement.

This is why the primary opponent is best understood as compute expansion versus capital recovery. Nvidia and custom ASICs represent different paths through the same pressure. Both must produce enough valuable work before depreciation consumes the expected return.

## What the $39.5 Billion Estimate Does Not Prove

**Bernstein’s calculation exposes the cost structure, but it cannot establish future utilization, useful life, or investment returns.**

The estimate reaches the public through summaries of proprietary research rather than a fully accessible engineering bill of materials. Readers can inspect the headline assumptions, but they cannot independently audit every component or supplier price.

That limitation matters because the model has changed. Bernstein’s June analysis put Vera Rubin infrastructure near $47 billion per gigawatt and one rack at $9.1 million. The October summary reduced the rack estimate to $7.52 million and total investment to $39.5 billion.

A 17 percent rack revision within several months demonstrates how sensitive the model is to memory, storage, and configuration assumptions. It does not invalidate the new estimate. It does argue against treating $39.5 billion as a fixed law of AI infrastructure.

The lower number also does not prove that returns improved. If market rental rates decline alongside hardware costs, operators may pass the savings to customers. Competition can convert a cost reduction into cheaper inference rather than higher margins.

Useful-life estimates create another uncertainty. Six years provides a convenient depreciation schedule, but AI accelerators compete on a much faster product cycle. Some older systems will remain valuable for inference, fine-tuning, or less demanding workloads. Others can become uneconomic earlier because of energy use or software requirements.

Companies can extend older hardware by shifting it down the workload ladder. A cluster that no longer trains the largest frontier model might still serve smaller models or batch jobs. The residual demand and pricing for those tasks remain uncertain.

Power costs also vary widely. Bernstein’s earlier $1.3 billion annual estimate used $0.15 per kilowatt-hour. A facility with cheaper contracted power would spend less, while constrained regions can face higher electricity and grid-upgrade costs.

A gigawatt of connected capacity is not always a gigawatt of continuous IT load. Maintenance, redundancy, cooling overhead, power usage effectiveness, and ramp schedules affect consumption. Comparisons require consistent definitions of utility power, critical load, and accelerator load.

Financing further complicates the picture. Two projects with identical equipment can produce different returns when one relies on corporate cash and another uses expensive debt. Interest is separate from depreciation, but both must ultimately be supported by revenue.

The funding structure is becoming more important as spending moves beyond the strongest corporate balance sheets. AllianceBernstein’s broader AI capex analysis warned that increasing debt and vendor financing can reduce funding quality.

The same analysis estimated approximately $400 billion of data center construction spending during 2025. It also placed projected hyperscaler investment through 2027 above $1 trillion. Those totals show why small changes in utilization or asset life can affect sector-wide earnings.

Demand remains the decisive missing variable. Signed contracts provide more certainty than forecasts, but even contracts require evaluation. Duration, termination rights, customer concentration, prepayments, and credit support determine whether promised revenue matches the asset’s risk.

Some operators can protect themselves through long-term leases that pass electricity and other operating costs to tenants. That structure improves visibility for the property owner. It does not ensure that the tenant earns enough from the computing capacity.

The analysis also cannot determine how much AI adoption will expand. Model developers are creating coding systems, research tools, media generators, advertising products, and business agents. The revenue per user and cost per task remain unsettled.

For enterprise buyers, lower inference costs can accelerate adoption. More organizations can move AI systems from experiments into regular workflows. Teams will still need to measure whether those systems save labor, improve output, or create new revenue.

Knowledge workers face a similar discipline. Access to more compute does not automatically produce better decisions. Organizations need trusted information, usable processes, and ways to retain results. A searchable AI knowledge base can help preserve context, but it does not change infrastructure economics.

The skeptical conclusion is narrow but important. Bernstein has provided a plausible cost framework, not proof of an AI bubble or proof that custom chips will outperform Nvidia. The estimate identifies the hurdle that future revenue must clear.

## Three Signals Will Show Whether the Investment Can Pay

**The next phase will be judged by depreciation, utilization, and revenue quality rather than announcements of additional gigawatts.**

The first signal is hyperscaler depreciation growth. Amazon, Microsoft, Alphabet, Meta, and Oracle disclose depreciation through financial statements and earnings commentary. Investors should compare those charges with cloud revenue and operating income.

Rising depreciation is expected during a construction wave. The warning appears when depreciation consistently grows faster than the revenue supported by the new assets. That pattern would strengthen the case that capital recovery has become the primary constraint.

Useful-life changes deserve equal attention. Extending server lives can reduce annual expense and lift reported earnings without changing current cash flow. Companies should explain why older accelerators remain productive and how they are being reassigned.

The second signal is realized utilization for new Rubin and custom-ASIC clusters. Order announcements show demand for equipment, but occupancy and workload volumes reveal whether completed capacity earns money.

Cloud providers can offer evidence through backlog conversion, AI service revenue, deployment schedules, and margin trends. Model companies can provide stronger proof through paying usage rather than broad measures of registered users or generated tokens.

If Rubin systems enter service with high utilization and lower cost per unit of useful output, the upper end of Bernstein’s range becomes easier to defend. If capacity ramps slowly, depreciation starts working against the investment thesis.

Custom chips face the same test. A lower facility cost helps only when software and demand keep the hardware busy. Strong utilization would support the claim that vertical integration creates durable savings. Weak utilization would show that lower component costs cannot compensate for unused capacity.

The third signal is the relationship between contracted revenue and financing obligations. The industry increasingly uses leases, debt, special-purpose vehicles, customer prepayments, and supplier arrangements to fund infrastructure.

These structures can accelerate construction and distribute risk. They can also make the economics harder to follow. Investors should watch contract duration, customer concentration, interest costs, termination provisions, and the timing difference between payments and asset delivery.

AllianceBernstein argues that the sector needs evidence of broad revenue growth before the depreciation cycle outruns visibility. Its concern is not simply the total valuation of AI companies. It is the period when spending is certain but the revenue trajectory remains unclear.

A stronger outcome would include expanding cloud margins despite higher depreciation, rising use of completed clusters, and longer contracted revenue coverage. Those developments would weaken the view that the buildout has outrun demand.

A weaker outcome would combine deployment delays, useful-life extensions, falling rental prices, and slower backlog conversion. That combination would strengthen Bernstein’s warning even if companies continue announcing new projects.

The broader lesson is that power availability is only the entrance requirement. Once a company secures land, electricity, cooling, and accelerators, it acquires a second problem. It must turn that capacity into recurring economic output quickly enough to recover the investment.

Bernstein AI data center costs make that deadline visible. The difference between $34.6 billion and $39.5 billion matters, but it is not the decisive variable. Revenue earned before the hardware loses economic value will determine whether the next gigawatt becomes productive infrastructure or an expensive accounting burden.

Readers should now look past the size of each announced campus. Watch depreciation in quarterly results, utilization when new systems enter service, and the durability of the contracts supporting construction. Those three signals will reveal whether the AI infrastructure boom is building an enduring computing platform or borrowing growth from hardware that ages faster than its business model.
