---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-bets-on-maia-300-its-in-house-chip-to-bring-down-the-cost-of-ai-inference-basebcn
title: "Microsoft bets on Maia 300, its in-house chip to bring down the cost of AI inference | basebcn"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "maia", "cost", "more", "inference", "chip", "in-house", "models"]
source: https://basebcn.com/en/news/microsoft-maia-300-ai-inference-chip
source_anchor: ""
source_lines: [1, 51]
section: "Copilot_Microsoft"
---
## From training to inference

For the past few years, the conversation about AI infrastructure has revolved around training large models and the GPUs that make it possible. On October 12, Microsoft shifted the focus: it unveiled Maia 300, the third generation of its in-house accelerator, and announced that the Maia family will become the primary inference system for its products and services, with multi-gigawatt deployments starting in 2027.

Inference is the work repeated every time someone asks Copilot a question, asks GitHub Copilot for code or a business application calls a model on Azure. It is the cost that grows with usage, which is why any efficiency gain at that stage feeds straight into the economics of AI services.

## What Microsoft announced

According to the company's official blog, signed by executive vice president Scott Guthrie, Maia 200 is already serving real production workloads, including Microsoft Copilot, and is being deployed across the United States, Europe and Australia. GitHub Copilot and Microsoft Foundry will follow before the year is out. Across those workloads, Maia 200 runs OpenAI models, Microsoft's own MAI models and open-source model families.

Maia 300 will enter production in 2027. Microsoft expects it to deliver 40% more tokens per megawatt than leading GPU-based systems, and a full rack of 96 accelerators to deliver up to seven times the throughput of Maia 200. These are the manufacturer's projections: as analyst Patrick Moorhead told CNBC, nobody has tested these chips outside Microsoft yet.

## The numbers and what they mean

The metric Microsoft puts front and centre is telling: tokens per megawatt. In a datacenter, available power is a physical limit and a large share of operating cost. “You can think of power translating pretty directly into operational cost,” Alistair Speirs, a general manager at Microsoft, told CNBC. If the same megawatt produces 40% more answers, the cost per answer falls, or available capacity rises without building more.

The second figure, up to seven times more throughput per rack than Maia 200, is about density: Microsoft doubles the number of accelerators per rack. More useful work per square metre and per megawatt means the same investment in buildings and grid connections serves more users. For buyers of AI services, that usually ends up as lower usage prices or more generous capacity limits.

## A design built around memory

Microsoft describes Maia 300 as an inference-first architecture, optimised for long-context, memory-intensive workloads dominated by the phase in which the model generates its answer token by token. Each accelerator pairs eight HBM4 memory stacks delivering 18 TB/s of peak bandwidth within a 950-watt power budget, and uses the chip's own SRAM as a software-managed scratchpad.

The chip uses a multi-chiplet design that separates compute, memory controllers and input/output, with the compute chiplets built on TSMC's N2P process. It also supports low-precision formats such as MXFP4, which cut the memory needed both for calculations and for storing context. The result, according to the company, is longer context windows and more throughput from the same power, exactly what agents that chain steps and consult long documents require.

## Cloud giants are making their own silicon

Maia is not an isolated case. Google has had its TPUs for years and Amazon its Inferentia and Trainium chips, and the leading model developers already mix silicon from several vendors. The difference, CNBC noted, is that Microsoft's accelerators cannot be rented by the hour to other companies: they are an internal component meant to lower the cost of its own services. Chief financial officer Amy Hood has credited in-house chips with margin improvement over time.

Nvidia remains central. Microsoft still trains models on Nvidia and AMD GPUs, and Evercore ISI analysts estimate that Maia will account for around 10% of the company's AI chip spending by early 2027. Even so, the trend points to more diverse infrastructure, in which each kind of work runs on the hardware that makes it cheapest.

## What changes for European companies

For a company in Barcelona or anywhere in Europe, the news does not mean buying hardware, but it does change the context in which decisions are made. Maia 200 is already being deployed in European datacenters and powers Teams and Copilot features that many organisations use every day. More efficient inference means more capacity available in European regions, which matters when data has to be processed inside the Union.

It also reinforces a trend worth keeping in mind when budgeting projects: the cost of each AI answer tends to fall, while the value lies in the processes it automates. Companies that rule out a use case today because of token costs should revisit that calculation regularly, because price curves move fast.

## Assess whether your organisation is ready

The practical lesson is to design for portability. Architectures that separate business logic from the specific model, that measure cost per completed task rather than per token, and that allow a change of model or region without rebuilding the application are the ones that benefit most from every efficiency leap in the market.

The quiz below helps you place your organisation: whether it already has use cases in production, how it measures their cost and how much it depends on a single provider. It is no substitute for an analysis, but it gives a first sense of direction.

### Where does your company stand with generative AI?

## What comes next

Microsoft has promised more details at Ignite 2026, its November conference in San Francisco. Until Maia 300 reaches production in 2027 and independent measurements exist, its figures should be read as the manufacturer's targets. Even so, the direction is clear: the major providers are competing to serve AI more cheaply and efficiently.

For companies, that opens a favourable window. Compute capacity will stop being the main brake on many projects, and the competitive edge will lie in choosing the right processes, preparing the data and building solutions that can benefit from each new generation of infrastructure.

“The AI race is no longer decided only by training models: it is decided by how much it costs to serve every answer.”
