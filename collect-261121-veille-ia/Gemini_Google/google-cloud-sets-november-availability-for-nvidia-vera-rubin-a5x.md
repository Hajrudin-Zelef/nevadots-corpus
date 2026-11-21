---
id: collect-261121-veille-ia/gemini-google/google-cloud-sets-november-availability-for-nvidia-vera-rubin-a5x
title: "Google Cloud sets November availability for NVIDIA Vera Rubin A5X"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "rubin", "cloud", "vera", "november", "nvidia", "nvidias", "infrastructure"]
source: https://magica.com/news/google-cloud-nvidia-a5x-vera-rubin-november-availability
source_anchor: ""
source_lines: [1, 107]
section: "Gemini_Google"
---
Google Cloud A5X Vera Rubin availability set for November | Magica

Google Cloud sets November availability for NVIDIA Vera Rubin A5X

Editorial Team

••📖6 min read

Google Cloud plans to open A5X generally in November after early access for Ineffable Intelligence. The launch adds a route to NVIDIA’s Vera Rubin platform, but its 960,000-GPU expansion is still a plan and published performance claims do not establish customer rental costs.

A5X is scheduled for general availability in November, following early access announced in July.

Google’s plan for as many as 960,000 Rubin GPUs spans multiple sites; it is not a count of launch capacity.

The first customer has engineering and financial ties to the suppliers, while performance evidence still leaves A5X customer costs open.

Google Cloud and chipmaker NVIDIA plan to make A5X, Google’s bare-metal cloud offering built on NVIDIA’s Vera Rubin NVL72 systems, generally available in November 2026. NVIDIA’s October 21 announcement also sets out plans for expansion to Europe, giving customers a launch month for infrastructure already being used in reinforcement-learning research and creative-agent tests.

The change is broader access to an existing platform. Google introduced A5X in April, and NVIDIA disclosed its first working instance for a customer in July. The November commitment matters to buyers planning deployments, but the announcement does not specify bookable capacity, rental prices or a timetable for Google’s proposed expansion to as many as 960,000 Rubin GPUs.

Google Cloud A5X hardware powered by NVIDIA Vera Rubin NVL72 and Google’s Virgo Network, pictured in NVIDIA’s July deployment update. Image courtesy of Google Cloud. Source: NVIDIA.

Google Cloud’s AI Hypercomputer diagram groups purpose-built hardware, open software and flexible consumption. It describes the broader infrastructure stack, rather than installed A5X capacity. Graphic: Google Cloud. Source: Google Cloud.

What November adds to the infrastructure plan

A5X combines NVIDIA’s Vera CPUs and Rubin GPUs in rack-scale systems, ConnectX-9 network adapters, Google’s Virgo Network and fourth-generation liquid cooling. Its intended uses include training, inference—the running of trained models—and reinforcement learning, in which systems learn through trial and error.

Ubisoft has made the team behind its Teammates experiment a dedicated subsidiary targeting AI characters with memory and outside game creators. Earlier playtests show both progress and limitations, while commercial plans remain undisclosed amid group cost cuts and refinancing.

Editorial Team

Google’s April 22 infrastructure announcement described Virgo support for up to 80,000 GPUs at one data center and 960,000 across multiple sites. NVIDIA now says Google plans to scale its Rubin infrastructure to that larger figure. Network design capacity and an expansion plan do not establish how many GPUs are installed or ready to rent.

The platform is reaching other clouds, too. NVIDIA’s deployment updates describe Rubin racks running at CoreWeave, Microsoft Azure, Oracle Cloud Infrastructure and Nebius, alongside Google. A report on the October rollout also relays NVIDIA’s statement that Vera Rubin is in full production. Neither fact makes every provider’s service generally available on the same schedule.

Google also has its own accelerator strategy. In April it introduced eighth-generation Tensor Processing Units: TPU 8t for training and TPU 8i for inference and reinforcement learning. In the same announcement, Google described existing NVIDIA-based infrastructure supporting Tinker, a platform for reinforcement learning and fine-tuning. A5X therefore adds a new hardware generation to workloads Google already serves; its appeal must rest on the combination of hardware, networking, software and commercial terms.

An early customer with ties to both suppliers

NVIDIA named Ineffable Intelligence as the first A5X customer. The London AI lab’s founder and CEO, David Silver, previously led reinforcement learning at Google’s DeepMind AI lab and helped develop the game-playing programs AlphaGo and AlphaZero. Ineffable aims to discover knowledge through experience without relying on human-generated examples, extending his work on reinforcement learning.

An April funding report put Ineffable’s seed financing at $1.1 billion and its valuation at $5.1 billion. It identified Sequoia Capital and Lightspeed Venture Partners as the round’s leaders and Google and NVIDIA among its investors. The cloud provider and chip supplier thus have a financial stake in the customer showcasing their infrastructure, as well as a commercial relationship with it.

The technical relationship was already explicit. In a May 13 joint announcement, Ineffable and NVIDIA said their engineers were developing reinforcement-learning infrastructure, starting on NVIDIA Grace Blackwell and planning early exploration of Vera Rubin. NVIDIA’s July 21 update then described an A5X instance running for the lab.

Their engineering account explains the demands: reinforcement-learning systems generate experience, evaluate it and update themselves repeatedly, putting pressure on memory, connections between processors and the infrastructure that runs models. Silver said in the October announcement that Vera Rubin’s compute and high-bandwidth CPU-to-GPU and GPU-to-GPU connections support that loop. This is a technically relevant endorsement from an early partner; it does not demonstrate that the lab’s proposed continuously learning systems have achieved their research goals.

CoreWeave’s company-reported DeepSeek R1 benchmark compares Vera Rubin NVL72 with GB200 NVL72 at matched per-user responsiveness, with inference optimizations enabled. It does not establish A5X reinforcement-learning performance or Google Cloud rental costs. Graphic: CoreWeave. Source: CoreWeave.

Performance gains need the right comparison

There is measured performance evidence for Vera Rubin beyond the A5X demonstrations. Rival cloud provider CoreWeave reported in a July 21 benchmark that Rubin NVL72 delivered ten times the tokens per second per megawatt of NVIDIA GB200 NVL72 on the DeepSeek R1 reasoning model, at matched per-user responsiveness. CoreWeave said both platforms used advanced inference optimizations through NVIDIA’s software.

That is a provider-reported inference result for a particular model and configuration. It does not establish the same gain for A5X reinforcement learning or a tenfold reduction in a Google Cloud bill. NVIDIA’s July A5X update separately advertised up to tenfold lower inference cost per token than the prior generation, but that claim is not a published A5X rental tariff.

WPP, the London-based marketing and communications company, supplies a different kind of evidence. NVIDIA says WPP demonstrated creative agents on A5X using its Nemotron models and OpenShell, software that controls agents’ access to tools and data. The agents can convert conversational requests into video generation, upscaling and restyling tasks, with human approval before execution and controls over sensitive brand assets.

According to NVIDIA’s October announcement, WPP workloads that typically needed multiple computing nodes ran on one A5X instance in initial tests. That suggests consolidation for those workloads. Without the previous hardware configuration, completion times, output quality and prices, it does not quantify customer savings.

The next decision depends on capacity, protection and cost

Customers assessing a November deployment still need regional availability, reservable capacity and comparable workload costs. The European expansion has no detailed schedule in NVIDIA’s announcement, and the 960,000-GPU plan has no delivery timetable there.

Security announcements also need to be matched to the product being purchased. NVIDIA says Google will offer rack-scale Confidential Computing, intended to protect models and data on third-party infrastructure, on bare-metal systems such as the Blackwell-based A4X Max. Google is separately making it generally available on G4 virtual machines with RTX PRO 6000 Blackwell Server Edition GPUs, for confidential inference and fine-tuning. Those are distinct offerings from the Rubin-based A5X launch.

Power is another limit on expansion. Google and NVIDIA say they are exploring NVIDIA DSX MaxLPS capabilities to improve performance per watt and fit more compute within available data-center power budgets. They have not presented that exploration as a measured efficiency gain for Google’s planned fleet.

November’s practical test is whether customers can obtain the capacity they need and reproduce useful performance at an acceptable total cost. The platform’s early deployments and benchmark results give them reasons to evaluate it; regional terms, workload measurements and a delivery schedule will determine how much of the expansion plan becomes usable infrastructure.

Neuralink has shown a trial participant controlling GTA VI through intended movements. Earlier gaming use and company-reported trial results put the demonstration in context, while its reliability across users remains unmeasured in the reports.

SoftBank founder Masayoshi Son reportedly explored financing a $1 trillion acquisition of memory-chip maker SK hynix, but the company says the deal is no longer under active consideration. The target brings surging earnings, existing shareholders and customer commitments that complicate any simple equation between ownership and access to AI-memory supply.

Nvidia reported $115.8 billion in quarterly revenue, but its shares fell 14% Friday amid a wider technology selloff, a leadership transition and a reported accounting investigation. Strong cash generation offers a counterweight to concerns about the durability of AI spending.

The Nasdaq 100 fell 5.6% on November 20 and closed 19.8% below its record after an intraday bear-market breach. Nvidia’s revenue surge and Anthropic’s huge IPO did not halt the rout; infrastructure bills and resilience elsewhere in the market complicate the picture.

OpenAI reports sharply lower reasoning-extraction rates in internal GPT-6.2 tests and backs sanctions against illicit foreign distillation. Its own research and rival Anthropic’s experience show why protecting hidden reasoning leaves capability transfer and customer costs unresolved.

Alberta proposes 600-metre setbacks for large data centres and expanded utility oversight, but exemptions and rules still to be written will determine how much protection communities receive.

President Donald Trump backed Commerce Secretary Howard Lutnick as lawmakers scrutinize alleged delays in restricting a suspected route for AI chips to China. A new congressional inquiry also questions Nvidia’s influence on an existing investigation.

Amazon’s month-old Echo Time is $50 off for Black Friday. Its touchscreen, home hub and optional sleep-aware alarm cost $90 more than the reported Echo Spot price, while Alexa+ access and evidence of better waking remain key buying questions.

Merck will pair its experimental data with Isomorphic Labs’ AI to seek small-molecule cancer drug candidates. The deal follows Isomorphic’s $5 billion financing, but its payments, target and development timetable remain undisclosed.

Thinking Machines Lab released a roughly 20GB open-weights model that reuses its middle layers for reasoning. It brings the Inkling family to local hardware, with competitive coding results but workload-dependent costs and preliminary evidence on oversight.

Apple’s first HomePod mini update since 2020 adds Mac audio and four-speaker surround, but Siri AI remains unavailable, core home-hub features carry over, and reviews disagree on playback reliability.

Treasury Secretary Scott Bessent says an unnamed Anthropic founder will announce a large gift to children's investment accounts within weeks, but the amount, recipients and connection to existing philanthropic pledges remain undisclosed.

Google says air cooling will limit water use at its Visakhapatnam AI hub, but environmental proceedings and residents’ accounts raise broader questions about land, electricity, public incentives and the evidence behind promised benefits.

Nvidia slipped behind Alphabet to fourth in U.S. market value during November 20 trading. The reversal follows a steep selloff despite strong reported revenue growth, with a permanent CEO still to be chosen.

Google will close the Nest app on January 25, 2027 and retire Assistant on home speakers and displays in August. Device transfers are automatic, but continued voice access requires accepting Gemini; basic assistance remains free.

Amazon says Cairn agents exploited a MONAI Label zero-day to take over a university server. Other attacks failed, and maintainers list no patched package release.

Federal Reserve Governor Lisa D. Cook sees early AI pressure on coding, translation and young workers, but says a solid overall labor market and persistent inflation still call for tighter monetary policy.

President Donald Trump threatened a tax on U.S. companies building data centers abroad without specifying how it would work. The proposal raises questions for an AI export program that has received 78 applications and for domestic construction constrained by power and community opposition.

Donald Trump said only older chips should be sold to China while discussing a broader deal with Beijing. His remarks leave unresolved which products would qualify as Washington tightens access to American AI hardware and overseas manufacturing.
