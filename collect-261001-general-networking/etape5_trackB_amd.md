# Step 5 — Track B: AMD Hardware (Instinct GPUs + EPYC/Ryzen CPUs)

## Research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 5 (Servers & hardware)
**Coverage window:** February 1, 2026 → September 22, 2026 (with launch-context for context)
**Scope:** Instinct MI300X/MI325X, MI350X/MI355X, MI400 series (MI455X/MI430X), Helios rack platform, EPYC Turin 9005 + Venice 9006, Ryzen client AI (Gorgon Point / Strix Halo), AMD AI partnerships 2026
**Status:** Research snapshot. Cloud $/hr prices and street prices are dated snapshots (mostly Sep 2026) and move constantly; re-verify before use.

### Provenance legend
- **[official]** — AMD's own announcements, press releases, keynotes, published specs, or SEC filings.
- **[vendor-reported]** — figures claimed by AMD (benchmarks, uplift claims) without independent audit.
- **[independent]** — reputable third-party press (Reuters, The Register, ServeTheHome, Tom's Hardware, SemiEngineering, EE Times, DIGITIMES) or independent measurement.
- **[secondary]** — lower-tier press, blogs, aggregators, community trackers (incl. GitHub research repos); useful but unverified.
- **[unverified]** — single-source, conflicting, or rumor claims; treat as uncertain.

### ROCm note
ROCm software stack details (ROCm 10.0.0, TheRock Core SDK, ROCm.AI, PyTorch support matrix) were covered in **Step 4 Track C** (`etape4_trackC_cuda_rocm_pytorch.md`) and are **not repeated here** — only hardware-side implications are referenced below.

---

## 1. INSTINCT MI300X / MI325X — 2026 status: mature workhorse, pricing trends, deployments

### 1.1 Product context (orientation, outside window)
- **MI300X** launched December 2023 (CDNA 3, 192 GB HBM3, 5.3 TB/s). **MI325X** launched late 2024 (CDNA 3, 256 GB HBM3E, 6 TB/s). Both are OAM 750 W parts **[secondary — https://www.bentoml.com/blog/amd-data-center-gpus-mi250x-mi300x-mi350x-and-beyond; https://github.com/regevba/fittracker2/blob/HEAD/docs/research/2026-04-28-hadf-signature-expansion.md]**.
- MI300X was the first AMD datacenter GPU adopted at meaningful hyperscaler scale (Microsoft, Meta, Oracle) as an H100 alternative **[secondary — https://www.nasdaq.com/articles/oracle-recently-delivered-incredible-news-advanced-micro-devices-amd-stock-investors]**.

### 1.2 2026 pricing and availability trends
- AMD does **not publicly disclose Instinct GPU list prices**; enterprise pricing is structured through OEM/hyperscaler deals (Dell, HPE, Supermicro, Lenovo channel) **[secondary — https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/amd-deep-dive.md]**. ⚠️ Treat all public "street prices" as community-sourced estimates.
- **Cloud $/GPU-hr (September 2026 snapshots):**
  | Provider | GPU | Price | Date | Provenance |
  |---|---|---|---|---|
  | Oracle Cloud (OCI) | MI300X | ~$6.00/hr | Sep 2026 | [secondary — https://susiloharjo.web.id/nvidia-h200-vs-amd-mi300x-vs-google-tpu-v6e-the-2026-ai-chip-benchmark-battle/] |
  | Azure | MI300X | $6.00–$7.86/hr | Sep 2026 | [secondary — same] |
  | Vultr | MI300X on-demand | $3.99/hr | 2025 collection | [secondary — https://www.bentoml.com/blog/amd-data-center-gpus-mi250x-mi300x-mi350x-and-beyond] |
  | DigitalOcean | MI300X | $1.99/hr on-demand | 2025 collection | [secondary — same] |
  | Hot Aisle | MI300X VM | $2.99/hr new allocations (was $1.99) | **Jul 14, 2026** | [secondary — https://github.com/hotaisle/hotaisle-website/blob/HEAD/src/content/blog/why-we-raised-our-mi300x-price.md] |
  | Spheron | MI300X on-demand | $3.59/hr (H100 SXM5 at $2.65/hr on the same marketplace) | **Sep 22, 2026** | [independent-ish marketplace — https://www.spheron.network/blog/triton-on-amd-rocm-vs-nvidia-cuda-same-kernel-different-perf/] |
- **Trend signal:** Hot Aisle raised newly allocated MI300X VM pricing from **$1.99 → $2.99/GPU-hr** (existing allocations held at $1.99; bare metal $3.39) on **Jul 14, 2026**, explicitly citing **full capacity** and MI300X's value position vs NVIDIA alternatives **[secondary — Hot Aisle blog]**. On Spheron's marketplace (Sep 22, 2026), MI300X at $3.59/hr was **more expensive than H100 at $2.65/hr** — the old "AMD is the cheap alternative" assumption is breaking down on some marketplaces **[independent — Spheron blog]**.
- ⚠️ Oracle Cloud is the mainstay for first-party MI300X cloud instances; AWS p5-class MI300X instances were still not generally available as of early 2026 **[secondary — https://susiloharjo.web.id/nvidia-h200-vs-amd-mi300x-vs-google-tpu-v6e-the-2026-ai-chip-benchmark-battle/]**.
- One real-world engineering-cost note (early 2026): teams report **40–80 engineering hours** on average to migrate production workloads from CUDA to ROCm — a TCO offset to the hourly savings **[secondary — same comparison]**. ⚠️ Anecdotal, treat as directional.

### 1.3 Key 2026 deployments
- **Oracle OCI:** announced **Oct 14, 2025** that OCI would deploy **50,000 AMD AI GPUs starting calendar Q3 2026** (expansion planned into 2027), beginning with MI300X-powered shapes and extending to MI355X GA on OCI Compute / "zettascale OCI Supercluster" **[independent/secondary — https://www.mitrade.com/insights/news/live-news/article-3-1194409-20251015; https://www.btcc.com/en-CA/square/Global%20Cryptocurrency/1069079; https://en.tmtpost.com/post/7721864]**. This is one of AMD's largest disclosed cloud wins to date.
- **Oracle 30,000 MI355X order:** on Oracle's **Mar 10, 2025** investor call, chairman Larry Ellison said Oracle placed a multibillion-dollar order for **30,000 AMD MI355X GPUs** — before the chips shipped in meaningful volume (mid-year 2025 target) **[secondary — https://www.nasdaq.com/articles/oracle-recently-delivered-incredible-news-advanced-micro-devices-amd-stock-investors]**.
- **IBM + Zyphra:** announced **Oct 28, 2025** (context): multiyear agreement for IBM to host a large **MI300X cluster on IBM Cloud** for Zyphra (open-source AI research company, SF), with Pensando Pollara 400 AI NICs — presented as one of the largest AMD-hardware genAI training clusters on a commercial cloud, used to train multimodal foundation models for Zyphra's "Maia" superagent **[secondary — https://techsabado.com/2025/10/28/tech-news-amd-ibm-zyphra-forge-new-path-for-ai-infrastructure/]**.
- **Meta:** continues as an MI300/MI350-series customer; the 2026 relationship is dominated by the new 6 GW deal (see §7).
- **MI325X cloud pricing (2025 collection, background):** Vultr $4.615/hr on-demand; DigitalOcean $1.69/hr (12-month) **[secondary — https://www.bentoml.com/blog/amd-data-center-gpus-mi250x-mi300x-mi350x-and-beyond]**.

---

## 2. INSTINCT MI350X / MI355X (CDNA 4) — specs, release status, benchmarks vs B200, pricing, customers

### 2.1 Launch and architecture
- **Announced June 12, 2025** at AMD's "Advancing AI" event in San Jose — MI350X and MI355X launched alongside **[independent — https://www.theregister.com/special-features/2025/06/12/amd-shines-a-light-on-its-helios-rack-scale-compute-platform/1352958; https://www.techpowerup.com/337940/amd-instinct-mi355x-draws-up-to-1-400-watts-in-oam-form-factor]**.
- **CDNA 4 architecture**, **185 billion transistors**, TSMC **N3P** process for the 8 XCD compute chiplets; dual I/O dies on TSMC 6nm; 128 HBM3E channels, 256 MB Infinity Cache, 4th-gen Infinity Fabric links **[official via press — https://www.tweaktown.com/news/105766/amd-launches-instinct-mi350-series-ai-chips-185-billion-transistors-288gb-hbm3e-memory/index.html; https://github.com/amd-agi/primus-turbo/blob/HEAD/agent/skills/kernel-optimize/knowledge/hardware/gfx950/overview.md]**.
- **256 active compute units** (8 XCD × 32 CU), **16,384 stream processors** — fewer CUs than MI325X/MI300X (304 CU), offset by ~2× per-CU low-precision throughput **[vendor-reported/secondary — https://www.tweaktown.com/news/105766/amd-launches-instinct-mi350-series-ai-chips-185-billion-transistors-288gb-hbm3e-memory/index.html; https://github.com/amd-agi/primus-turbo/blob/HEAD/agent/skills/kernel-optimize/knowledge/hardware/gfx950/overview.md]**.
- Memory: **288 GB HBM3E per GPU**, **8 TB/s** sustained bandwidth (12-Hi stacks, 36 GB each) **[official via press — same sources]**. AMD positions 288 GB vs **~60% more than NVIDIA B200's ~180–192 GB** and above MI325X's 256 GB, fitting up to ~520B-parameter models on a single chip **[vendor-reported — https://www.crn.com/news/components-peripherals/2025/amd-instinct-mi350-gpus-use-memory-edge-to-best-nvidia-s-fastest-ai-chips]**.
- Precision support: **FP16, FP8 (OCP), FP6, FP4 + MXFP8/MXFP6/MXFP4 block-scaled formats** — FP6/FP4 are new in CDNA 4 **[official via press — https://github.com/amd-agi/primus-turbo/blob/HEAD/agent/skills/kernel-optimize/knowledge/hardware/gfx950/overview.md]**.
- Form factor / power: **MI350X** = OAM, **1,000 W**, air-coolable; **MI355X** = OAM, **1,400 W**, direct-liquid-cooling target — the same 1,400 W envelope as NVIDIA GB300 "Grace Blackwell Ultra" **[official via press — https://www.techpowerup.com/337940/amd-instinct-mi355x-draws-up-to-1-400-watts-in-oam-form-factor]**.
- Peak theoretical throughput (per GPU, dense; ×2 with 2:4 sparsity):

| dtype | MI350X | MI355X |
|---|---|---|
| FP4 / FP6 / MX | 18.4–18.45 PFLOPS | 20.1 PFLOPS |
| FP8 | 9.2 PFLOPS | 10.1 PFLOPS |
| FP16/BF16 | 4.6 PFLOPS | 5.0 PFLOPS |
| FP64 | 72 TFLOPS | 78.6–79 TFLOPS |

**[official via press — https://www.tweaktown.com/news/105766/amd-launches-instinct-mi350-series-ai-chips-185-billion-transistors-288gb-hbm3e-memory/index.html; https://www.tomshardware.com/pc-components/gpus/amd-announces-mi350x-and-mi355x-ai-gpus-claims-up-to-4x-generational-gain-up-to-35x-faster-inference-performance; https://github.com/amd-agi/primus-turbo/blob/HEAD/agent/skills/kernel-optimize/knowledge/hardware/gfx950/overview.md]**

- Scale-up: 7 external Infinity Fabric links at 38.4 Gbps 16-lane (153.6 GB/s per link), **~1,075 GB/s** peer aggregate per device; host attach PCIe Gen 5 x16 **[secondary — https://github.com/openfabric-systems/simllm/blob/HEAD/docs/papers/amd-gpu-fabric.md]**.
- AMD's "UBB8" open AI-infrastructure standard was introduced with MI350 for faster air/liquid node deployment **[official via press — https://www.tweaktown.com/news/105766/amd-launches-instinct-mi350-series-ai-chips-185-billion-transistors-288gb-hbm3e-memory/index.html]**.

### 2.2 Release/GA status in 2026
- MI350X/MI355X began shipping in meaningful volume **mid-2025** (H2 2025 GA); by 2026 the series is in full production and is AMD's flagship shipping AI accelerator **[independent/secondary — https://www.tomshardware.com/pc-components/gpus/amd-announces-mi350x-and-mi355x-ai-gpus-claims-up-to-4x-generational-gain-up-to-35x-faster-inference-performance; https://github.com/regevba/fittracker2/blob/HEAD/docs/research/2026-04-28-hadf-signature-expansion.md]**.
- **Enterprise PCIe variant "MI350P"** is priced through channel partners (Dell, HPE, Supermicro, Lenovo) — no public list price **[secondary — https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/amd-deep-dive.md]**.
- **Cloud pricing (Sep 2026 snapshot):** Oracle Cloud MI355X at **$8.60/GPU-hr** **[secondary — https://susiloharjo.web.id/nvidia-h200-vs-amd-mi300x-vs-google-tpu-v6e-the-2026-ai-chip-benchmark-battle/]**.

### 2.3 AMD's competitive claims vs NVIDIA B200/GB200 (all vendor-reported)
At launch AMD claimed for MI355X **[vendor-reported — https://www.crn.com/news/components-peripherals/2025/amd-instinct-mi350-gpus-use-memory-edge-to-best-nvidia-s-fastest-ai-chips]**:
- **FP6:** 2× GB200 performance (FP6 is the format AMD positions for next-gen training).
- **FP4:** on par with GB200, ~10% faster than B200.
- **FP8:** on par with GB200, ~10% faster than B200.
- **FP16:** on par with GB200, ~10% faster than B200.
- **FP64:** 2× GB200 and B200.
- Inference throughput: **~20% better on DeepSeek R1** and **~30% better on Llama 3.1 405B** than B200; AMD claims MI355X "delivers the highest inference throughput" for large models **[vendor-reported — same]**.
- Generational: up to **4× vs MI300X-class (CDNA 3)** performance; up to **35× faster inference** than prior-gen in AMD's framing **[vendor-reported — https://www.tomshardware.com/pc-components/gpus/amd-announces-mi350x-and-mi355x-ai-gpus-claims-up-to-4x-generational-gain-up-to-35x-faster-inference-performance]**.
- ⚠️ Independent third-party head-to-heads at scale remain thin; TechPowerUp's launch coverage cautioned that "the question remains whether real-world benchmarks will match these ambitious specifications" **[independent — https://www.techpowerup.com/337940/amd-instinct-mi355x-draws-up-to-1-400-watts-in-oam-form-factor]**. Treat the B200 comparisons as vendor claims.

### 2.4 Customers and traction 2026
- **Oracle:** 30,000-unit MI355X order (multibillion-dollar, Mar 10, 2025 call); MI355X GA coming to OCI Compute starting calendar Q3 2026 as part of the 50,000-GPU AMD deployment **[secondary — https://www.nasdaq.com/articles/oracle-recently-delivered-incredible-news-advanced-micro-devices-amd-stock-investors; https://www.mitrade.com/insights/news/live-news/article-3-1194409-20251015]**.
- **Meta:** MI300/MI350 series installed base; 2026 procurement shifts toward the MI450/Helios generation (see §7) **[secondary — https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/amd-deep-dive.md]**.
- **OpenAI:** partnership "began with the MI300X and continued with the MI350X series" en route to MI450 deployments **[secondary — https://en.tmtpost.com/post/7721864]**.
- Rack-scale traction: Pegatron preparing **128-GPU racks** (EIA-compliant 96-GPU variant offers ~27 TB memory); full AMD rack vision: 96–128 MI350-series GPUs, up to ~36 TB HBM3E, ~2.6 exaflops FP4 / ~1.3 exaflops FP8, paired with EPYC "Turin" and Pollara 400 NICs **[secondary — https://webpronews.com (via search result); https://www.tweaktown.com/news/105766/amd-launches-instinct-mi350-series-ai-chips-185-billion-transistors-288gb-hbm3e-memory/index.html]**. ⚠️ The 30,000-unit figure and rack claims come from secondary aggregators.

---

## 3. INSTINCT MI400 SERIES — roadmap status, announced specs, timeline

### 3.1 Naming and positioning
- The **MI400 series** is AMD's next-generation AI/HPC accelerator family (CDNA 5), announced at **CES 2026** (Jan 2026) with flagship **MI455X**, and detailed further at **Advancing AI 2026** (Jul 22–23, 2026) and **Hot Chips 2026** (Aug 2026) **[independent — https://www.theregister.com/special-features/2025/06/12/amd-shines-a-light-on-its-helios-rack-scale-compute-platform/1352958; https://markets.financialcontent.com/bentoncourier/article/tokenring-2026-1-8-amd-ignites-the-yotta-scale-era-unveiling-the-instinct-mi400-and-helios-ai-infrastructure-at-ces-2026; https://www.servethehome.com/amd-helios-mi400-system-architecture-at-hot-chips-2026/]**.
- ⚠️ Naming caveats: sources variously reference "MI450", "MI455X", and "MI430X". The flagship rack part is **MI455X**; **MI430X** is positioned for sovereign AI/HPC offerings (e.g., Cirrascale's sovereign-AI cloud) **[secondary — https://www.businesswire.com/news/home/20260714481734/en/Cirrascale-Advances-Open-AI-Infrastructure-with-AMD-Helios-Rackscale-Solution-and-AMD-Instinct-MI400-Series-GPUs; https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/amd-deep-dive.md]**. A "custom MI450 variant" is the named vehicle for the Meta deal — see §7.
- One source (low-tier aggregator) claimed MI400 is "slated for 2026" at ~10× MI300X performance — treat the 10× figure as **[unverified — https://webpronews.com via search result]**.

### 3.2 Announced specs (MI455X)
- **CDNA 5**, TSMC **2nm (N2)** process, **320 billion transistors** **[official via press — https://markets.financialcontent.com/bentoncourier/article/tokenring-2026-1-8-amd-ignites-the-yotta-scale-era-unveiling-the-instinct-mi400-and-helios-ai-infrastructure-at-ces-2026]**.
- Memory: **432 GB HBM4 per GPU**, **19.6 TB/s** peak bandwidth **[official via press — https://www.theregister.com/special-features/2025/06/12/amd-shines-a-light-on-its-helios-rack-scale-compute-platform/1352958; https://markets.financialcontent.com/bentoncourier/article/tokenring-2026-1-8-amd-ignites-the-yotta-scale-era-unveiling-the-instinct-mi400-and-helios-ai-infrastructure-at-ces-2026]**.
- Compute: **40 petaFLOPS FP4** per GPU (OCP MXFP4) **[independent — https://www.theregister.com/special-features/2025/06/12/amd-shines-a-light-on-its-helios-rack-scale-compute-platform/1352958]**.
- Interconnect: **UALink** open standard for scale-up — AMD claims **3.6 TBps per-accelerator** bandwidth, "just as fast as NVLink" **[independent — The Register]**; AMD opened its Infinity Fabric technology to partners in 2024, foundational to UALink **[independent — same]**.
- **Timeline:** volume shipments **H2 2026**; first 1 GW customer deployments (Meta, OpenAI) scheduled to begin H2 2026 **[secondary — https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/amd-deep-dive.md; https://markets.financialcontent.com/ms.intelvalue/article/marketminute-2026-2-25-amd-secures-historic-6-gigawatt-ai-infrastructure-deal-with-meta-platforms]**.
- Beyond MI400: **MI500 series** announced as the 2027 next generation at AMD's Analyst Day **[secondary — https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/amd-deep-dive.md]**.

### 3.3 Analyst framing
- Coverage frames MI400/Helios as AMD's answer to NVIDIA Vera Rubin NVL144, with the differentiation being **memory capacity leadership (432 GB HBM4 vs competitors)** and an **open-standards stack** (UALink, UEC Ethernet, OCP) vs NVIDIA's closed NVLink ecosystem **[independent — https://www.theregister.com/special-features/2025/06/12/amd-shines-a-light-on-its-helios-rack-scale-compute-platform/1352958; https://www.servethehome.com/amd-helios-mi400-system-architecture-at-hot-chips-2026/]**.

---

## 4. HELIOS RACK PLATFORM — status

### 4.1 Overview
- **Helios** is AMD's rack-scale AI infrastructure blueprint/platform, co-developed with Meta through the **Open Compute Project (OCP)**, unveiled at CES 2026 and detailed at Advancing AI 2025/2026 and Hot Chips 2026 **[independent — https://www.theregister.com/special-features/2025/06/12/amd-shines-a-light-on-its-helios-rack-scale-compute-platform/1352958; https://www.servethehome.com/amd-helios-mi400-system-architecture-at-hot-chips-2026/]**.
- Target workloads: frontier-model training and large-scale inference serving; AMD markets it as "exaflop-class AI performance" in a single rack **[official via press — https://www.businesswire.com/news/home/20260714481734/en/Cirrascale-Advances-Open-AI-Infrastructure-with-AMD-Helios-Rackscale-Solution-and-AMD-Instinct-MI400-Series-GPUs]**.

### 4.2 Configuration (Hot Chips 2026 details)
- **72× MI455X accelerators** per rack, paired with 6th Gen EPYC "Venice" CPUs **[independent — https://www.theregister.com/special-features/2025/06/12/amd-shines-a-light-on-its-helios-rack-scale-compute-platform/1352958]**.
- Rack: **44OU ORW-HPR chassis**, 18 compute trays + 6 switch trays (72-GPU configuration); blind-mate quick-disconnect liquid cooling, redundant PSUs, 50V DC LC busbar; **OCP ORW-aligned 4-GPU compute trays** **[independent — https://www.servethehome.com/amd-helios-mi400-system-architecture-at-hot-chips-2026/]**.
- Per compute tray: **4× MI455X + 1× Venice SP7 CPU host**; each GPU node gets **1.8 TB/s/direction UALoE** (UALink-over-Ethernet) scale-up bandwidth across 12× 3×2 links, **coherent Infinity Fabric 128 GB/s/direction** to the CPU, and up to **3× Pensando Vulcano 800 AI NICs** per EAM via UALink **[independent — ServeTheHome]**.
- Scale-up fabric: two **512-port 200G UALoE switch ASICs** per switch tray, 10.8 TB/s/direction, 72 active links per switch, multi-plane all-to-all GPU connectivity; ~7 kW liquid-cooled tray managed by a Ryzen-based Run BMC **[independent — ServeTheHome]**.
- Aggregate performance: up to **2.9 exaFLOPS peak FP4 (OCP MXFP4)**, **31 TB HBM4 memory** per rack, ~**43 TB/s aggregate scale-out bandwidth** (BusinessWire/Cirrascale figures) / **260 TBps of UALink bandwidth** (The Register) **[vendor-reported via partners — https://www.businesswire.com/news/home/20260714481734/en/Cirrascale-Advances-Open-AI-Infrastructure-with-AMD-Helios-Rackscale-Solution-and-AMD-Instinct-MI400-Series-GPUs; https://www.theregister.com/special-features/2025/06/12/amd-shines-a-light-on-its-helios-rack-scale-compute-platform/1352958]**. ⚠️ Figures vary by source; treat as marketing maxima.
- Networking: **Pensando Vulcano superNICs** — **300 GBps scale-out bandwidth per accelerator** over (presumed) 12× 200 Gbps Ultra Ethernet links; scale-out across racks on **UEC-aligned Ethernet** (no proprietary fabric lock-in) **[independent/secondary — https://www.theregister.com/special-features/2025/06/12/amd-shines-a-light-on-its-helios-rack-scale-compute-platform/1352958; https://www.businesswire.com/news/home/20260714481734/en/Cirrascale-Advances-Open-AI-Infrastructure-with-AMD-Helios-Rackscale-Solution-and-AMD-Instinct-MI400-Series-GPUs]**.
- Software: shared-memory fabric managed by **AMD Instinct Fabric Manager**, UALoE transport over Ethernet with ESUN; defense-in-depth security, virtualization, rack-scale software acceleration; full ROCm stack **[independent — ServeTheHome; https://www.businesswire.com/news/home/20260714481734/en/Cirrascale-Advances-Open-AI-Infrastructure-with-AMD-Helios-Rackscale-Solution-and-AMD-Instinct-MI400-Series-GPUs]**.
- **Status:** system architecture public (Hot Chips 2026); customer deployments (Meta, OpenAI first gigawatts) scheduled from H2 2026; **Cirrascale** announced **Jul 14, 2026** it will offer Helios racks + MI455X (and MI430X for sovereign AI/HPC) in its AI innovation cloud — the first neocloud commitment **[official — https://www.businesswire.com/news/home/20260714481734/en/Cirrascale-Advances-Open-AI-Infrastructure-with-AMD-Helios-Rackscale-Solution-and-AMD-Instinct-MI400-Series-GPUs]**.

---

## 5. EPYC TURIN 9005 (5th Gen) — 2026 adoption and news

### 5.1 Specs (launch context, Oct 2024)
- 5th Gen EPYC "Turin" (EPYC 9005), Zen 5 / Zen 5c, SP5 socket, up to **192 cores / 384 threads** (Zen 5c 9965) or 128 Zen 5 cores (9755), 12-channel DDR5-6400, 128 PCIe 5.0 lanes, up to 5 GHz boost, full 512-bit AVX-512 + VNNI + BF16 **[independent — https://WWW.TECHSPOT.COM/news/105100-amd-launches-epyc-9005-turin-processors-up-192.html; https://convergedigest.com/amd-unveils-5th-gen-epyc-cpu-claims-34-of-server-business/]**.
- 27 SKUs at launch; pricing examples: **EPYC 9965 $14,813**, 9755 $12,984, 9015 (8c) $527 **[independent — https://WWW.TECHSPOT.COM/news/105100-amd-launches-epyc-9005-turin-processors-up-192.html]**.

### 5.2 2026 adoption and pricing trends
- **Google** adopted 5th Gen EPYC (Turin) 9005 for new AI server infrastructure (reported via CPU industry press) **[secondary — https://cpu.itbrief.co/posts/google-adopts-amd-5th-gen-epyc-cpus-for-new-ai-servers/]**.
- **OVHcloud** refreshed its Scale dedicated-server range on EPYC 9005 (up to 192c/384 threads, 384/768 GB RAM configs) — announcement dated **Oct 28, 2025** (context) **[secondary — https://github.com/ovh/infrastructure-roadmap/issues/285]**.
- **Meta AI servers** pair Turin EPYC host CPUs with MI350 racks (per AMD rack materials) **[secondary — https://www.tweaktown.com/news/105766/amd-launches-instinct-mi350-series-ai-chips-185-billion-transistors-288gb-hbm3e-memory/index.html]**.
- The **EPYC 9575F** (frequency-optimized, up to 5 GHz) is AMD's SKU positioned for GPU-powered AI host nodes (28% faster AI processing per AMD's claims) **[vendor-reported — https://convergedigest.com/amd-unveils-5th-gen-epyc-cpu-claims-34-of-server-business/]**.
- ⚠️ No major new Turin SKUs were announced in the Feb–Sep 2026 window found in this research; the 2026 EPYC news is dominated by Venice (see §6). 2026 pricing-trend data for Turin SKUs was not located — flagged as a gap.

---

## 6. EPYC VENICE 9006 (6th Gen, Zen 6) — 2026 announcement

### 6.1 Timeline
- **CES 2026 (Jan 6, 2026):** Lisa Su held up a bare (no-IHS) Venice sample in her keynote, revealing TSMC **InFO_oS** advanced chiplet packaging — 2 I/O dies surrounded by 2 rows of 4 CCDs **[independent — https://abit.ee/en/hard/processors/amd-epyc-venice-processor-server-zen-6-ces-2026-chiplet-infoos-tsmc-256-cores-en; https://www.guru3d.com/story/amd-unveils-256core-epyc-venice-for-helios-mi455x-ai-racks-platform/]**.
- **Advancing AI 2026 (Jul 22–23, 2026), San Francisco:** AMD formally unveiled the 6th Gen EPYC "Venice" (EPYC 9006 series); announced as **entering volume production on TSMC 2nm** — the first HPC/server chip on 2nm **[independent — https://www.martincid.com/technology-sv/amd-epyc-venice-256-cores-2nm/; https://finance.biggo.com/news/1fa76c25-95b5-4fd8-a771-5dd63c39e1a2]**. AMD CTO Mark Papermaster confirmed the July 2026 launch window, starting with server/datacenter EPYC **[independent — https://www.club386.com/amd-confirms-zen-6-launch-date/]**.
- EPYC roadmap through 2030 also published: **Venice (Zen 6) 2026 → Florence (Zen 7) 2028 → Ravenna (Zen 8) 2030** **[secondary — https://www.kad8.com/hardware/amd-epyc-roadmap-venice-florence-and-ravenna-through-2030/]**.

### 6.2 Specs and variants
- **Up to 256 cores / 512 threads** per socket: 8 CCDs × 32 Zen 6c cores (density-optimized), or up to **96 standard Zen 6 cores** (192 threads) for single-thread leadership **[official via press — https://finance.biggo.com/news/1fa76c25-95b5-4fd8-a771-5dd63c39e1a2; https://www.martincid.com/technology-sv/amd-epyc-venice-256-cores-2nm/]**. ⚠️ Whether the 32-core CCDs are full Zen 6 or dense Zen 6c was initially ambiguous in CES coverage; the Advancing AI 2026 reveal clarified the Zen 6 / Zen 6c split.
- **3D V-Cache up to 1,152 MB L3** on stacked variants **[official via press — https://finance.biggo.com/news/1fa76c25-95b5-4fd8-a771-5dd63c39e1a2]**.
- New **SP7 socket**; **16 channels DDR5-8000** (~1.6 TB/s aggregate, ~3× Turin's ~614 GB/s); **MRDIMM** support reaching DDR5-12800-equivalent bandwidth **[official via press — https://www.martincid.com/technology-sv/amd-epyc-venice-256-cores-2nm/; https://finance.biggo.com/news/1fa76c25-95b5-4fd8-a771-5dd63c39e1a2]**.
- **PCIe Gen 6 (64 Gbps)**, 5th-gen Infinity Fabric **[official via press — https://www.martincid.com/technology-sv/amd-epyc-venice-256-cores-2nm/]**.
- TSMC 2nm GAA nanosheet: 10–15% higher perf at same power or 25–30% lower power at same perf, +15% density (TSMC figures) **[independent — https://www.martincid.com/technology-sv/amd-epyc-venice-256-cores-2nm/]**.
- Product lines (4): **EPYC 9006 SP7** (flagship 256c "agentic AI" part, 5 GHz boost, 128 PCIe Gen6 lanes); **EPYC 9006X SP7** (up to 96c, 5.15 GHz, 3× L3 per core, HPC/simulation); **EPYC 9006 SP8** (8–128c, edge/power-constrained); **EPYC 9006 LP "Verano"** (LPDDR5X, 24 channels, SOCAMM2 form factor, 112 Gbps CPU-to-GPU bandwidth for rack-scale AI chassis) **[official via press — https://www.martincid.com/technology-sv/amd-epyc-venice-256-cores-2nm/; https://www.tweaktown.com/news/112793/amd-announces-6th-gen-amd-epyc-venice-cpus-up-to-256-cores-and-512-threads-and-built-for-ai/index.html]**.
- AMD vendor claims: **+70% CPU throughput vs Turin Zen 5**; **2.2× AI inference throughput vs NVIDIA's "Vera" platform** at the 256-core tier; Venice **>3× faster than NVIDIA Vera** (per TweakTown headline) **[vendor-reported — https://www.martincid.com/technology-sv/amd-epyc-venice-256-cores-2nm/; https://www.tweaktown.com/news/112793/amd-announces-6th-gen-amd-epyc-venice-cpus-up-to-256-cores-and-512-threads-and-built-for-ai/index.html]**.
- Manufacturing: initial production at **TSMC Taiwan**, later also **TSMC Arizona** **[independent — https://www.club386.com/amd-confirms-zen-6-launch-date/]**.

---

## 7. RYZEN CLIENT AI — 2026 releases

### 7.1 Gorgon Point / Ryzen AI 400 series (announced CES 2026, Jan 6, 2026)
- AMD announced the **Ryzen AI 400 series mobile processors** (codename **Gorgon Point**) — a refresh of Strix Point (Ryzen AI 300): same **Zen 5 / Zen 5c CPU + RDNA 3.5 GPU + XDNA 2 NPU** formula, 4nm-class, with higher clocks and an upgraded NPU **[independent — https://www.techpowerup.com/338398/amd-ryzen-ai-9-hx-475-470-gorgon-point-apus-surface-in-shipping-manifests; https://abit.ee/en/hard/processors/amd-ryzen-ai-400-gorgon-point-processor-apu-zen-5-copilot-strix-halo-ces-2026-npu-en; https://wccftech.com/amd-confirms-zen-6-medusa-cpus-2027-next-gen-gaming-gpu-improved-ai-raytracing/]**.
- Flagship **Ryzen AI 9 HX 475**: 12c/24t (4× Zen 5 + 8× Zen 5c), up to **5.2 GHz** (vs 5.1 on Strix Point), 16-CU Radeon 890M, **L3 up to 36 MB** (vs 34), **NPU up to 60 TOPS** (vs 50 on Strix Point; 55+ TOPS in other configs), LPDDR5X-8533 support (vs 8000), 28 W default TDP **[official via press / secondary — https://abit.ee/en/hard/processors/amd-ryzen-ai-400-gorgon-point-processor-apu-zen-5-copilot-strix-halo-ces-2026-npu-en; https://www.tweaktown.com/news/104174/amds-next-gen-gorgon-point-apus-leaked-zen-5-rdna-3-refresh-expected-in-2026/index.html; https://www.techpowerup.com/338398/amd-ryzen-ai-9-hx-475-470-gorgon-point-apus-surface-in-shipping-manifests]**.
- Also surfaced in shipping manifests: **Ryzen AI 9 HX 470** (5.2 GHz), plus new **Ryzen AI 7 450** and **Ryzen 5/3** entry models **[secondary — https://www.techpowerup.com/338398/amd-ryzen-ai-9-hx-475-470-gorgon-point-apus-surface-in-shipping-manifests]**.
- **Desktop + enterprise expansion (MWC 2026, Mar 2):** **Ryzen AI PRO 400 series** (enterprise-hardened), and **Ryzen AI 7 450G / 5 440G desktop APUs** — positioned by one source as the first **Copilot+-certified desktop processors** (Gorgon Point silicon, XDNA 2 NPU ≥40 TOPS) **[secondary — https://markets.financialcontent.com/pentictonherald/article/marketminute-2026-3-6-amd-solidifies-ai-everywhere-strategy-with-massive-ryzen-ai-400-expansion-into-desktop-and-enterprise-markets; https://abit.ee/en/hard/processors/amd-ryzen-ai-400-gorgon-point-processor-apu-zen-5-copilot-strix-halo-ces-2026-npu-en]**. ⚠️ HP, Lenovo, Dell OEM design wins reported; the "first Copilot+ desktop" claim is thinly sourced — treat cautiously.
- AMD's claimed positioning: flagship Ryzen AI 9 HX 470 shows **1.3× multitasking, 1.7× content creation, +10% gaming** vs Intel Core Ultra 9 288V at comparable TDP (28 W vs 30 W); up to 24h laptop battery life **[vendor-reported — https://abit.ee/en/hard/processors/amd-ryzen-ai-400-gorgon-point-processor-apu-zen-5-copilot-strix-halo-ces-2026-npu-en]**.
- **Roadmap context:** Gorgon Point is a Zen 5 refresh; true next-gen **Medusa Point (Zen 6, N3-class)** arrives **2027** with >10× AI performance claims, plus **Medusa Baby** for mainstream (H2 2027); Strix Halo remains in-market through at least end-2027 **[secondary — https://www.tomshardware.com/pc-components/cpus/amd-mobile-cpu-roadmap-leak-claims-zen-6-arrives-in-2027; https://wccftech.com/amd-confirms-zen-6-medusa-cpus-2027-next-gen-gaming-gpu-improved-ai-raytracing/]**.

### 7.2 Strix Halo (Ryzen AI Max 300) — 2026 adoption and new SKUs
- Strix Halo (Zen 5, up to 16c, 40-CU RDNA 3.5 Radeon 8060S, XDNA 2 50 TOPS, up to 128 GB unified LPDDR5X with up to 96 GB assignable as VRAM) launched Jan 2025; by 2026 it became the **de facto local-LLM workstation chip** **[secondary — https://www.techradar.com/pro/ryzen-ai-max-395-cpu-could-be-amds-sleeper-hit-against-nvidias-ai-dominance-as-nearly-30-strix-halo-mini-ai-workstation-models-hit-the-market-including-some-rather-funky-ones]**.
- **Adoption (2026):** nearly **30 mini AI workstation models** launched by 2026 — Beelink GTR9 Pro, Seaviv AideaStation R1, HP Z2 Mini G1a, GMKtec EVO-X2, Minisforum MS-S1 MAX, Abee AI Station 395 Max; laptops: Asus ROG Flow Z13, Chinese mobile-workstation designs (Sixunited, Linglong, Tianba). Price positioning **$1,800–$2,800** vs $21,000+ for multi-GPU AI servers **[secondary — TechRadar]**. ⚠️ Big-brand availability remains limited; "real-life tests remain scarce" per TechRadar.
- **New SKUs at CES 2026:** **Ryzen AI Max+ 392** and **Ryzen AI Max+ 388** — detuned CPU core counts from the Max+ 395 but **full 40-CU GPU** retained; the 392 surfaced in Geekbench (~2,917 single-core, ~12–15% behind the 395 in multi-core) **[independent — https://hothardware.com/news/amd-ces-2026?ref=thetechstreetnow.com; https://www.notebookcheck.net/New-AMD-Strix-Halo-Ryzen-AI-Max-392-stars-in-early-benchmark-after-CES-2026-debut.1204390.0.html]**.
- **Local-AI positioning:** AMD claims HP Z2 Mini G1a advantages of **1.5×/1.7× tokens-per-second-per-dollar** in LM Studio on GPT-OSS 20B/120B vs NVIDIA DGX Spark **[vendor-reported — https://hothardware.com/news/amd-ces-2026?ref=thetechstreetnow.com]**; community reports ~47–53 tok/s on 120B models vs DGX Spark's 56 tok/s at lower price **[secondary — https://github.com/getnyrex/strix-halo-guide/blob/HEAD/RESEARCH.md]**.
- **Framework Desktop** clusters: 4-node Strix Halo clusters running **1T-parameter Kimi K2.5** via distributed llama.cpp reported by community researchers **[secondary — https://github.com/getnyrex/strix-halo-guide/blob/HEAD/RESEARCH.md]**.
- **Ryzen AI Embedded (Jan 8, 2026):** new embedded portfolio — **P100** (Zen 5, up to 12c, RDNA 3.5, XDNA 2, 50 TOPS, 15–54 W, −40°C to +105°C) for in-vehicle/industrial; **X100** (Strix Halo silicon, up to 16c) for "physical AI"/autonomous systems **[official via press — https://videocardz.com/newz/amd-introduces-ryzen-ai-embedded-p100-strix-point-krackan-and-x100-strix-halo-series]**.
- Rumored roadmap: "Gorgon Halo" (Ryzen AI Max 400, same arch, higher clocks) Q4 2026; "Medusa Halo" with LPDDR6 (~80% more memory bandwidth) **[unverified — https://github.com/getnyrex/strix-halo-guide/blob/HEAD/RESEARCH.md]**.

---

## 8. AMD AI PARTNERSHIPS AND STRATEGIC DEALS — 2026

### 8.1 Meta — 6 GW multi-generation deal (announced ~Feb 25, 2026)
- AMD and Meta signed a multi-year agreement for up to **6 gigawatts** of AI infrastructure across multiple Instinct GPU generations **[independent — https://hostingjournalist.com/news/amd-lands-meta-as-major-ai-chip-customer]**.
- First phase: a **custom MI450-architecture GPU** variant (optimized for Meta's software stack and low-latency inference) + 6th Gen EPYC "Venice" CPUs + Helios rack architecture, on ROCm. **Shipments for the first 1 GW begin H2 2026**; remaining 5 GW roll out in tranches **through 2031** **[independent/secondary — https://hostingjournalist.com/news/amd-lands-meta-as-major-ai-chip-customer; https://markets.financialcontent.com/ms.intelvalue/article/marketminute-2026-2-25-amd-secures-historic-6-gigawatt-ai-infrastructure-deal-with-meta-platforms]**.
- Financial structure: AMD issued Meta **performance-based warrants for up to 160 million AMD shares** (~10% of the company) vesting in stages per deployed gigawatt; final tranche vests only if AMD's share price hits **$600** **[secondary — https://markets.financialcontent.com/ms.intelvalue/article/marketminute-2026-2-25-amd-secures-historic-6-gigawatt-ai-infrastructure-deal-with-meta-platforms]**.
- Deal value: Wolfe Research analysts estimated **$15–20B per gigawatt**, implying **$90–120B** in potential AMD revenue over the full 6 GW (Meta was already an AMD customer, so not all incremental) **[secondary — https://www.neowin.net/news/amd-lands-massive-100-billion-gpu-deal-with-meta-for-ai-data-centers/]**.
- Co-development: Helios rack architecture **co-designed by AMD and Meta through OCP**; Forrest Norrod (AMD DCGM EVP/GM): "Helios reflects the spirit of open collaboration that defines the OCP community" **[secondary — https://techsabado.com/2025/10/28/tech-news-amd-ibm-zyphra-forge-new-path-for-ai-infrastructure/; https://hostingjournalist.com/news/amd-lands-meta-as-major-ai-chip-customer]**.
- ⚠️ The same secondary sources note Meta signed a hardware deal with NVIDIA a week prior — the AMD deal is part of a multi-vendor diversification strategy **[secondary — https://www.neowin.net/news/amd-lands-massive-100-billion-gpu-deal-with-meta-for-ai-data-centers/]**.

### 8.2 OpenAI — 6 GW deal (announced Oct 2025; executing in 2026)
- AMD and OpenAI reached a **6 GW multi-generation agreement** (announced Oct 2025; OpenAI statement **Oct 6**): deployments start with the **Instinct MI450 series + rack-scale (Helios) solutions**, extending to future generations; **first 1 GW begins H2 2026**, tied to the Stargate buildout **[secondary — https://en.tmtpost.com/post/7721864]**.
- Same warrant structure as Meta: up to **160M AMD shares at $0.01**, vesting on deployment milestones (first tranche at 1 GW), share-price targets, and OpenAI technical/commercial milestones **[secondary — same]**.
- Collaboration history: "began with the MI300X and continued with the MI350X series" **[secondary — same]**.

### 8.3 Oracle (Oct 2025 announcement; deployment from Q3 2026)
- 50,000 AMD GPUs on OCI starting calendar Q3 2026, expansion 2027+: MI300X first, then MI355X GA; earlier 30,000-unit MI355X order (Mar 2025) **[secondary — https://www.mitrade.com/insights/news/live-news/article-3-1194409-20251015; https://www.nasdaq.com/articles/oracle-recently-delivered-incredible-news-advanced-micro-devices-amd-stock-investors]**.

### 8.4 Other 2026 ecosystem items
- **Red Hat:** Red Hat AI 3 certified on Instinct; joint vLLM development; AMD GPU Operator on OpenShift; RHEL AI bare-metal on MI300X **[secondary — https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/amd-deep-dive.md]**.
- **Cirrascale:** first neocloud to commit to Helios + MI455X/MI430X (Jul 14, 2026) **[official — https://www.businesswire.com/news/home/20260714481734/en/Cirrascale-Advances-Open-AI-Infrastructure-with-AMD-Helios-Rackscale-Solution-and-AMD-Instinct-MI400-Series-GPUs]**.
- **AMD AI DevDay 2026** (San Francisco) and **Advancing AI 2026** (Jul 22–23, San Francisco) as the year's developer/customer showcases **[secondary — https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/amd-deep-dive.md]**.
- **IBM/Zyphra** (Oct 2025, context): MI300X training cluster on IBM Cloud + Pensando Pollara 400 NICs **[secondary — https://techsabado.com/2025/10/28/tech-news-amd-ibm-zyphra-forge-new-path-for-ai-infrastructure/]**.

---

## 9. SPEC SUMMARY TABLE

| Product | Architecture / Process | Memory | Peak compute (dense) | Power | Status Sep 2026 |
|---|---|---|---|---|---|
| MI300X | CDNA 3 | 192 GB HBM3, 5.3 TB/s | ~1.3 PFLOPS FP16 | 750 W | Mature; ~$6/hr cloud; full-capacity pricing signals |
| MI325X | CDNA 3 | 256 GB HBM3E, 6 TB/s | 2.61 PFLOPS FP16 | 1,000 W | Shipping |
| MI350X | CDNA 4 / TSMC N3P | 288 GB HBM3E, 8 TB/s | 18.4 PFLOPS FP4 | 1,000 W | GA H2 2025; $8.60/hr OCI |
| MI355X | CDNA 4 / TSMC N3P | 288 GB HBM3E, 8 TB/s | 20.1 PFLOPS FP4 | 1,400 W | Flagship shipping; Oracle 30k order |
| MI455X | CDNA 5 / TSMC 2nm | 432 GB HBM4, 19.6 TB/s | 40 PFLOPS FP4 | TBD (liquid) | Volume H2 2026 |
| EPYC 9965 (Turin) | Zen 5c | DDR5-6400 12ch | 192c/384t | 500 W | Adopted (Google, OVHcloud) |
| EPYC Venice (9006) | Zen 6 / TSMC 2nm | DDR5-8000 16ch | 256c/512t | TBD | Volume production from Jul 2026 |
| Ryzen AI 9 HX 475 | Zen 5/5c + RDNA 3.5 + XDNA 2 (60 TOPS) | LPDDR5X-8533 | 12c/24t | 28 W | Announced CES 2026 (Gorgon Point) |
| Ryzen AI Max+ 395 | Zen 5 + RDNA 3.5 40CU + XDNA 2 (50 TOPS) | 128 GB unified | 16c/32t | 45–120 W | Shipping; ~30 mini-PC/laptop designs |

---

## 10. OPEN VERIFICATION ITEMS

1. **MI350 vs B200 head-to-heads** — AMD's 10–30% claims are vendor-reported; independent third-party benchmarks at scale remain thin as of Sep 22, 2026.
2. **MI300X/MI350 list prices** — AMD does not publish; all figures are cloud $/hr or community estimates. Older "~$15,000" claims have no verified 2026 source.
3. **MI400/MI455X final specs and exact volume timing** — H2 2026 volume is AMD's stated schedule; actual ramp and TDP figures unconfirmed.
4. **MI430X** — sovereign-AI/HPC variant; specs not published by AMD as of Sep 2026.
5. **"MI450" vs "MI455X" naming** — sources mix the two; MI455X is the flagship rack part; Meta's "custom MI450" variant details unconfirmed.
6. **MI500 (2027)** — announced at Analyst Day; no specs.
7. **Helios bandwidth figures** — 260 TBps (Register) vs 43 TB/s scale-out (Cirrascale/BusinessWire) vs per-GPU 1.8 TB/s/dir (STH): marketing maxima; apples-to-oranges.
8. **Oracle 30,000 MI355X order** — from a 2025 earnings call via secondary reporting; fulfillment status unconfirmed.
9. **Venice pricing** — no MSRP published as of Sep 2026.
10. **Turin 2026 pricing trends** — no 2026 SKU/pricing updates located in this research.
11. **"First Copilot+ desktop CPU"** — thinly sourced; AMD's Gorgon Point desktop APUs' certification details unconfirmed.
12. **Gorgon Halo / Medusa Halo** — rumor-grade (community research repo).
13. **Venice CCD core type** — resolved post-Advancing AI 2026 (Zen 6 / Zen 6c split), but per-SKU core maps incomplete.
14. **Wolfe Research $90–120B Meta-deal estimate** — analyst projection, not committed revenue.
15. **Spheron $3.59/hr MI300X vs $2.65/hr H100** — live marketplace snapshot, Sep 22, 2026; supply-driven, moves constantly.

---

## 11. COLLECTION METADATA

- **Research date:** September 22, 2026.
- **Search passes:** 8 web-search batches covering MI300/MI350 specs & benchmarks, MI400/Helios architecture, Oracle/Meta/OpenAI partnerships, EPYC Venice announcement, Ryzen Gorgon Point & Strix Halo, MI300X cloud pricing.
- **Source mix:** AMD announcements via press coverage (official-by-report), The Register, ServeTheHome, Tom's Hardware, TechPowerUp, CRN, TweakTown, Notebookcheck, HotHardware, BusinessWire (Cirrascale), plus secondary aggregators and community research repos (flagged as such).
- **Cross-references:** ROCm stack → `etape4_trackC_cuda_rocm_pytorch.md` (Step 4 Track C); NVIDIA comparison data → Step 3 Track D.
- **Nothing was sent externally; no live-browser visits; read-only web research + local file write.**
