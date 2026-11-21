---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-rubin-explained-specs-price-vs-blackwell-2026
title: "Nvidia Rubin Explained: Specs, Price, vs Blackwell (2026)"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-04"]
keywords: ["rubin", "nvidia", "vera", "blackwell", "rack", "specs", "nvl72", "explained"]
source: https://thebriefscript.com/nvidia-rubin/
source_anchor: ""
source_lines: [1, 174]
section: "NVIDIA_Hardware"
---
# Nvidia Rubin Explained (2026): Specs, Pricing, Availability and Rubin vs Blackwell

Nvidia Rubin explained for buyers: Rubin GPU and Vera Rubin NVL72 specs (288 GB HBM4, 50 PF NVFP4), Rubin vs Blackwell B200/B300/GB300, release timeline, cloud availability (CoreWeave, Oracle, Google), analyst rack prices, Rubin Ultra roadmap and whether to wait or buy Blackwell now.

*Last updated: Nov 4, 2026*

**Short answer:** Nvidia Rubin is the GPU generation after Blackwell. Each Rubin GPU has 288 GB of HBM4 and 50 petaflops of NVFP4 inference. It ships mainly in the Vera Rubin NVL72 rack (72 GPUs, 36 Vera CPUs). It is in full production and has been rentable on CoreWeave since Sep 30 and Oracle since Oct 27, 2026.

This guide is for buyers. It covers the specs that matter, how Rubin compares with Blackwell (B200, B300 and GB300), when and where you can get it, what it costs, and whether you should wait for it or buy Blackwell now. Facts were checked against Nvidia’s own pages and press releases on November 4, 2026. Anything Nvidia has not confirmed is marked with a dagger (†).

For the wider chip market, see our AI chip news 2026 tracker.

## What is Nvidia Rubin?

Rubin is the name of Nvidia’s 2026 data center GPU architecture, named after astronomer Vera Rubin. Nvidia launched the platform at CES on January 5, 2026, as “six new chips, one AI supercomputer”: the Rubin GPU, the Vera CPU, the NVLink 6 switch, the ConnectX-9 SuperNIC, the BlueField-4 DPU and the Spectrum-6 Ethernet switch (Nvidia).

You will see three names that cause confusion:

- **Rubin GPU:** the chip itself. Two GPU dies in one package.
- **Vera Rubin:** the platform that pairs Rubin GPUs with Nvidia’s own Vera CPU.
- **Vera Rubin NVL72 vs NVL144:** the same rack. Nvidia first called it NVL144 because it counted GPU dies (two per package). It renamed it NVL72 to count packages, matching GB200 and GB300 (Nvidia). If you search “vera rubin nvl144”, this is the product you mean.

Most buyers will meet Rubin as a full rack or a cloud instance. Nvidia also sells an 8-GPU board, HGX Rubin NVL8, for x86 servers.

## Nvidia Rubin specs

Nvidia quietly revised several Rubin numbers in late October 2026. HBM4 bandwidth per GPU fell from 22 TB/s to 19.2 TB/s and NVLink 6 from 3.6 TB/s to 3 TB/s. Nvidia said the earlier figures were “preliminary architectural targets” and the new ones reflect “what the system sustains at qualified, volume production” (Wccftech). The table uses the revised production specs from Nvidia’s Vera Rubin NVL72 page.

|  | Rubin GPU | Vera Rubin Superchip | Vera Rubin NVL72 rack | HGX Rubin NVL8 | 
|---|---|---|---|---|
| What it is | 1 GPU | 2 GPUs + 1 Vera CPU | 72 GPUs + 36 CPUs | 8 GPUs, x86 or Vera host | 
| NVFP4 inference (sparse) | 50 PF | 100 PF | 3,600 PF | 400 PF | 
| NVFP4 training (dense) | 35 PF | 70 PF | 2,520 PF | 270 PF | 
| FP8 training (dense) | 17.5 PF | 35 PF | 1,260 PF | 130 PF | 
| HBM4 memory | 288 GB | 576 GB | 20.7 TB | 2 TB | 
| HBM4 bandwidth | 19.2 TB/s | 38.5 TB/s | 1,400 TB/s | 130 TB/s | 
| NVLink 6 bandwidth | 3 TB/s | 6 TB/s | 216 TB/s | 24 TB/s (switch) | 
| CPU | n/a | 88 Olympus Arm cores | 3,168 cores, up to 54 TB LPDDR5X | x86 or Vera | 
| Power | Not published† | Not published | ~220 kW per rack† | Not published | 

Power is the gap buyers should notice. Nvidia publishes no per-GPU or per-rack wattage. Bernstein models a Vera Rubin NVL72 rack at 220 kW† (Investing.com), and analyst Ming-Chi Kuo puts the Rubin GPU at up to 2,300 W† (Investing.com). The rack is 100% liquid cooled and designed for 45°C inlet water (Nvidia).

One more catch: as of November 4, Nvidia’s HGX page still lists the old 22 TB/s and 3.6 TB/s per-GPU figures. Quote the NVL72 page, not the HGX page.

### What about Rubin CPX?

Rubin CPX was announced on September 9, 2025 as a cheaper “context” GPU for long prompts: 30 PF of NVFP4, 128 GB of GDDR7, and a Vera Rubin NVL144 CPX rack rated at 8 exaflops, due at the end of 2026 (Nvidia). It then vanished from Nvidia’s GTC 2026 roadmap in March, with Groq 3 LPX racks shown instead (Tom’s Hardware). Kuo reported in August that CPX was revived with 168 GB of HBM4 and production in Q1 2027†. Nvidia has not confirmed that. Do not plan a 2026 purchase around CPX.

## Nvidia Rubin vs Blackwell

On paper, Rubin is a big jump in compute and memory speed, but not in memory size. At rack level, Vera Rubin NVL72 delivers 2.3x the dense FP4 compute and 3.5x the dense FP8 compute of GB300 NVL72, with 2.4x the HBM bandwidth. GPU memory barely moves: 20.7 TB vs 20 TB.

|  | HGX B200 | HGX B300 (Blackwell Ultra) | HGX Rubin NVL8 | 
|---|---|---|---|
| GPUs | 8 | 8 | 8 | 
| FP4 dense | 72 PF | 108 PF | 270 PF | 
| FP8 dense | 36 PF | 36 PF | 130 PF | 
| GPU memory | 1.4 TB HBM3E | 2.1 TB HBM3E | 2 TB HBM4 | 
| Memory bandwidth | Not listed | Not listed | 130 TB/s | 
| NVLink per GPU | 1.8 TB/s (NVLink 5) | 1.8 TB/s (NVLink 5) | 3 TB/s (NVLink 6) | 
| Networking | 0.8 TB/s | 1.6 TB/s | 1.8 TB/s | 
| Status (Nov 2026) | Shipping | Shipping | Planned on OCI; partners | 

*Source: Nvidia HGX and Vera Rubin NVL72 pages. Blackwell FP8 is Nvidia’s sparse figure halved.*

Two things stand out for buyers. First, an 8-GPU Rubin board has slightly less memory than an 8-GPU B300 board (2 TB vs 2.1 TB) after Nvidia’s revision cut HGX Rubin NVL8 from 2.3 TB (Wccftech). If your models are memory-bound rather than compute-bound, B300 is not obsolete. Second, Rubin’s FP8 gain (about 3.6x per board) is bigger than its FP4 gain (2.5x vs B300). Teams still training in FP8 benefit most.

Nvidia’s own headline claims are larger: one-tenth the cost per million tokens and one-fourth the GPUs to train mixture-of-experts models versus Blackwell, and up to 10x more tokens per megawatt than GB200 NVL72 (Nvidia). Those are vendor benchmarks on chosen models. The first customer number is more modest: Cognition measured up to 4.8x the total token throughput of GB200 NVL72 on its coding agent (CoreWeave). Note that both comparisons are against GB200, not the newer GB300.

## Rubin release date and timeline

| Date | What happened | 
|---|---|
| Mar 2025 | GTC 2025: Vera Rubin NVL144 shown for 2H 2026 (later renamed NVL72) | 
| Sep 9, 2025 | Rubin CPX announced for end of 2026 | 
| Jan 5, 2026 | CES: Rubin platform launched, “in full production” | 
| Mar 16, 2026 | GTC 2026: Vera Rubin POD, Rubin Ultra NVL576 and Kyber roadmap | 
| May 31, 2026 | GTC Taipei (Computex): full-production ramp; shipments “this fall” | 
| Jun 1, 2026 | CoreWeave brings up the first cloud Vera Rubin NVL72 | 
| Aug 26, 2026 | Racks running at CoreWeave, Google Cloud, Azure, OCI and Nebius | 
| Sep 30, 2026 | CoreWeave: first customer (Cognition) in production | 
| Oct 27, 2026 | Oracle: Vera Rubin NVL72 generally available on OCI | 
| Nov 2026 | Google Cloud A5X general availability (announced as “next month” on Oct 21) | 
| Nov 17, 2026 | Nvidia Q3 FY27 earnings, the next Rubin volume update | 

*Sources: Nvidia CES, Nvidia GTC Taipei, Nvidia Q2 FY27, Oracle/Nvidia, Google Cloud, Nvidia IR.*

So the answer to “Rubin release date” depends on how you buy. Hyperscalers and AI labs have racks now. Enterprises renting cloud capacity can get it today on CoreWeave and OCI, with Google Cloud next. Buying your own racks through Dell, HPE, Lenovo or Supermicro means joining an allocation queue.

## Availability and pricing

**Where you can rent Rubin today:**

- **CoreWeave:** available since Sep 30, 2026, with Cognition as the first production customer.
- **Oracle Cloud (OCI):** Vera Rubin NVL72 and Vera CPUs generally available since Oct 27, 2026, with HGX Rubin NVL8 servers planned. Our AI infrastructure news hub tracks the OCI rollout.
- **Google Cloud:** A5X instances with first customers live; general availability due in November 2026, scaling to 80,000 Rubin GPUs per site.
- **Microsoft Azure:** racks running per Nvidia, but no public instance or GA date yet.
- **AWS:** received its first Vera Rubin hardware in August and plans 2 million more Nvidia GPUs, including Rubin, in 2027-2028 (Nvidia). No Rubin instance or price yet.

**What it costs:** Nvidia publishes no list price for Rubin. No cloud provider posts a public on-demand Rubin rate either; one GPU price index tracking 800+ offers found zero Rubin listings in early November (SAVRN). Expect quote-only contracts for now.

Analyst estimates for buying a rack:

- **Morgan Stanley:** about $7.8M per Vera Rubin NVL72 for hyperscalers, versus about $4.0M for GB300 NVL72. Memory alone is about $2M, up 435%. That implies about $55,000 per Rubin GPU† (Tom’s Hardware).
- **Bernstein:** about $9.1M per rack and about $47B per gigawatt of data center, all in† (Investing.com).

Enterprises buying through OEMs should expect to pay more.

The chart shows the uncomfortable math. The rack costs roughly twice as much, so raw compute per dollar improves by 0% to 20% depending on whose estimate you use. The case for Rubin rests on Nvidia’s token-efficiency claims: more tokens per watt and per dollar on large reasoning models. That holds for frontier-scale inference, less so for mid-size enterprise workloads.

For comparison, Blackwell is easy to rent today. Typical on-demand prices run about $8.55 per GPU-hour for B200, $11.46 for B300 and $13.18 for GB300 (SAVRN). If renting versus buying is your real question, read our guide to renting GPUs instead of buying them.

### Supply: why Rubin will stay tight

Two bottlenecks set how fast Rubin ships. First, HBM4. SK hynix says memory demand will exceed supply through 2030, and Samsung’s 2027 HBM is reportedly sold out (SK hynix). Second, TSMC’s CoWoS packaging. TSMC is reportedly weighing a CoWoS price increase of up to 20%†, with tool lead times stretching to eight months (TrendForce). Both push prices up and delivery dates out.

## Rubin Ultra and Feynman: what comes next

Nvidia’s official roadmap from GTC 2026 has **Rubin Ultra** next, in three sizes: NVL72, a new Kyber NVL144 rack with 144 GPUs, and a flagship NVL576 that links eight 72-GPU racks into one NVLink domain (Nvidia). The original target was 2H 2027 with four dies and 1 TB of HBM4E per package.

That plan is under pressure. Reports say Rubin Ultra moved to a two-die design and that Kyber slipped to 2028†; Nvidia says its “roadmap is intact” (Inside Deep Tech). TrendForce says Nvidia is evaluating smaller HBM configurations because DRAM stays tight through 2027† (TrendForce).

**Feynman** follows in 2028, with stacked dies, custom HBM and a new Rosa CPU (HPCwire).

## Should you wait for Rubin or buy Blackwell now?

**Buy or rent Blackwell now if:**

- You need capacity this quarter. B200 and B300 are rentable by the hour; Rubin is quote-only.
- Your data center cannot host fully liquid-cooled racks in the ~200 kW class. Rubin NVL72 needs 100% liquid cooling and an estimated ~220 kW per rack†.
- Your workloads are memory-bound. An 8-GPU B300 board has as much memory as an 8-GPU Rubin board.
- You run small and mid-size models, or you are serving a private LLM for internal users. Blackwell’s cost per token is already good enough, and prices will fall as Rubin ramps.

**Wait for (or reserve) Rubin if:**

- You train or serve frontier-scale mixture-of-experts or long-context reasoning models, where Nvidia’s 4x-10x efficiency claims apply.
- You are signing a multi-year commitment from 2027 onward. Buying a 2025 architecture on a five-year contract is a depreciation risk.
- You already have liquid-cooled halls with power for 200 kW-plus racks.
- You can live with quote-only pricing and allocation queues.

**A practical middle path:** rent Blackwell for 6-12 months with an exit or upgrade clause, and reserve Rubin capacity for 2027 on CoreWeave, OCI or Google Cloud. Skip Rubin Ultra in planning until Nvidia restates its specs and date.

## Rubin vs AMD MI455X, Google TPU and Trainium

|  | Nvidia Rubin GPU | AMD Instinct MI455X | Google TPU7x Ironwood | AWS Trainium3 | 
|---|---|---|---|---|
| Memory | 288 GB HBM4 | 432 GB HBM4 | 192 GiB HBM | 144 GB HBM3e | 
| Memory bandwidth | 19.2 TB/s | 23.3 TB/s | 7.4 TB/s | 4.9 TB/s | 
| FP4 / low precision | 50 PF NVFP4 (sparse) | 40 PF MXFP4 | No native FP4 | MXFP4 supported | 
| FP8 | 17.5 PF (dense) | 20 PF | 4.6 PF | 2.5 PF | 
| Rack / pod | 72 GPUs | 72 GPUs (Helios) | 9,216 chips per pod | 144 chips per UltraServer | 
| How you get it | Many clouds, OEMs | OEMs, select clouds | Google Cloud only | AWS only | 

*Sources: Nvidia, AMD, Google Cloud, AWS. Peak vendor specs; sparse and dense conventions differ.*

AMD’s MI455X beats Rubin on memory: 50% more HBM4 and faster bandwidth, launched July 23, 2026, with Anthropic, Meta, Microsoft, OpenAI and OCI running first Helios racks. Rubin wins on software maturity (CUDA) and on the number of clouds that offer it. Google’s Ironwood TPU and AWS Trainium3 are cheaper per chip-hour inside their own clouds but lock you into one provider. Our TPU vs GPU guide goes deeper on that trade-off.

## FAQ

### When is Nvidia Rubin available?

Now, through cloud providers. CoreWeave made Vera Rubin NVL72 available on Sep 30, 2026, and Oracle made it generally available on Oct 27, 2026. Google Cloud’s A5X general availability is due in November 2026. AWS and Azure have hardware but no public instances yet.

### How much does an Nvidia Rubin GPU cost?

Nvidia does not publish prices. Analysts estimate a Vera Rubin NVL72 rack at $7.8 million (Morgan Stanley) to $9.1 million (Bernstein), or roughly $55,000 per GPU inside a hyperscaler rack†. No cloud lists a public hourly Rubin rate yet.

### Is Vera Rubin NVL144 the same as NVL72?

Yes. Nvidia first counted GPU dies (144) and now counts GPU packages (72). The Vera Rubin NVL72 rack has 72 Rubin GPUs and 36 Vera CPUs. “NVL144 CPX” is different: it is the rack design announced with Rubin CPX, whose 2026 launch did not happen.

### How much faster is Rubin than Blackwell?

At rack level, about 2.3x the dense FP4 compute, 3.5x the dense FP8 compute and 2.4x the memory bandwidth of GB300 NVL72, per Nvidia’s specs. Memory capacity is nearly the same. Nvidia claims up to 10x lower cost per token than Blackwell; the first customer benchmark showed 4.8x the throughput of GB200.
