---
id: collect-261121-veille-ia/hardware-semi/256-cores-1-024-threads-amd-zen-6-first-benchmarks-land-integer-throughput-up-to-93-higher
title: "256 cores, 1,024 threads: AMD Zen 6 first benchmarks land, integer throughput up to 93% higher"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["cores", "first", "threads", "epyc", "benchmarks", "land", "integer", "higher"]
source: https://www.ainomam.com/post/256-核1024-线程amd-zen-6-首测解禁整数吞吐最高提升-93-d5e5f7
source_anchor: ""
source_lines: [1, 110]
section: "Hardware_Semi"
---
# 256 cores, 1,024 threads: AMD Zen 6 first benchmarks land

# 256 Cores, 1,024 Threads: AMD Zen 6 First Benchmarks Land, Integer Throughput Up to 93% Higher

At the Advancing AI event in July, when AMD put the sixth-generation EPYC "Venice" on display, Zen 6 was still just an architecture on a slide. Today is different. On November 10 US time, the EPYC 9006 SP7 platform officially goes on sale, with complete systems shipping at the same time, and on the same day, independent testing from two foreign outlets was released together. **The measured results of the first mass-produced CPU on the Zen 6 architecture are laid out in front of everyone for the first time.**

After going through both sets of tests this morning, my biggest takeaway is this: the step this generation takes is considerably bigger than I expected. The flagship EPYC 9996 has 256 cores and 512 threads per socket; two sockets fully populated gives 512 cores and 1024 threads. Today's protagonist is the data-center EPYC, and what we're looking at is what this entirely new architecture can do the first time it lands on silicon.

## The First to Deliver Is the Server

Zen 6 makes its debut as a mass-produced chip on the EPYC product line. The first systems come from Dell, HPE, Lenovo, and Supermicro, and the cloud is not left out either: Microsoft Azure's HDv2 virtual machines are already open for preview applications, making it the first platform to bring sixth-generation EPYC to the cloud; Oracle's OCI Compute E7 is also on the way.

According to AMD's plan, SP7 is only the first of four families to arrive. Next year there will also be SP8 for enterprise and storage, Venice-X with 3D V-Cache, and Verano using LPDDR5X. One architecture spanning four product lines—that pace alone says a lot about the weight of this generation.

## The Specs Laid Out: What's Advanced Is Visible at a Glance

| EPYC 9996 | Specifications | 
|---|---|
| Cores / Threads | 256 cores, 512 threads; dual-socket 512 cores, 1024 threads | 
| Frequency | 2.55GHz base / 4.1GHz boost | 
| Cache / Power | 1024MB L3 / 600W default, configurable down to 400W | 
| Memory | 16-channel DDR5-8000 or MRDIMM-12800, up to 1.6TB/s, up to 8TB per socket | 
| I/O | 96 PCIe 6.0 lanes per socket, CXL 3.1 | 
| Process / Price | TSMC N2P, 2nm / $14,904; 31 SKUs across the lineup, starting at $700 | 

First, the foundation. Zen 6 uses TSMC N2P; AMD's comparison is: 18% higher performance than N3E at the same power, 36% lower power at the same performance, and another 15% density improvement. The previous generation, Zen 5, was still on N4P—this jumps two process generations at once.

Next, packaging. Previously one EPYC used one IOD; Venice splits it into two, each with 8 memory channels, together giving 16 channels, up to 1.6TB/s per socket, and up to 8TB of memory. I/O also moves to PCIe 6.0, with bandwidth per lane doubled over the previous generation, plus CXL 3.1. For feeding data to AI servers, bandwidth and lane count matter more than anything.

The changes inside the cores are just as dense. A Zen 6c CCD goes from 16 cores to 32 cores, shared L3 from 32MB to 128MB, averaging 2MB to 4MB per core; a standard Zen 6 CCD goes from 8 cores to 12 cores with 48MB L3, reaching up to 5.3GHz on Venice. A single processor can carry up to 8 CCDs, hence the 256-core high-density model and the 96-core high-frequency model.

Drilling down into the pipeline: the Op Cache expands to 6.75K Ops, supplying up to 18 operations and 8-wide dispatch per cycle; the out-of-order window grows from 448 entries to 640, integer physical registers from 240 to 352, and each core keeps 1MB of private L2. The vector path continues at 512-bit, this time with native AVX-512 FP16 half-precision support, and the INT8 VNNI pipeline goes from two paths to four. There's also a design I quite appreciate: Zen 6 and Zen 6c are the same microarchitecture, differing only in trade-offs of density, frequency, power, and area—the high-density and high-frequency routes share the same architectural work.

Stacking all this up, AMD's fixed-frequency, same-core comparison gives an average IPC improvement of 27%, 20% on enterprise workloads, and 31% on AI workloads.

## The Report Card: One Set Official, One Set Independently Measured

The generational comparison in AMD's official briefing is striking: versus the previous generation Turin, 2x integer compute, 2.4x floating point, 2.7x memory bandwidth, 1.4x HPC, 1.3x matrix compute; versus Intel Xeon 6980P, an average of 2.2x on the selected HPC workloads, up to 3.2x on NAMD.

What's more exciting today is the two third-party yardsticks.

Tom's Hardware ran SPEC CPU 2026 integer throughput on dual 9996 and got **up to 93% higher** than the previous-generation dual 9965's 768 threads. In the same test suite, Blender is 30–40% faster and Stockfish nearly 50% faster. The high-frequency model 9686F delivers even more consistent head-to-head results: versus NVIDIA Vera, 17% higher integer throughput, 40% floating point, 21% integer speed, 32% faster PyBench, 92% faster SciMark, more than 2x on CloverLeaf and nearly 2x on FFTE; versus the previous generation's 128-core 9755, it has fewer cores yet still leads integer throughput by 19%.

Over at Phoronix, hundreds of Linux tests were averaged into a geometric mean: dual 9996 is **1.58x** the 9965, with only 33% more cores. The 9686F is 1.48x the previous generation's high-frequency flagship 9575F, 1.39x the 128-core 9755, and 1.56x the Xeon 6980P; code compilation is 1.63x faster, HPC 1.58x faster, and rendering workloads like Blender, LuxCore, and V-Ray combined are 2.2x faster. Among the HPC numbers, a few especially intuitive ones stand out: on the OpenRadioss Ford Taurus model, the 9996 took only 54% of the previous generation's time and one-third of the Xeon's time; NAMD and GROMACS can saturate 1024 threads, and Embree set a new world record outright.

I like looking at the two kinds of numbers together. The most multi-core-hungry throughput scenarios reach 93%, and after averaging hundreds of workloads there's still 1.58x—showing that this generation moves IPC, cache, memory, and I/O forward together, with core count stacking only one part of it. Even more notable is efficiency: even though the 9996 is a 600W part, Phoronix's testing still shows a leading system-level performance-per-watt; the 9686F, with higher frequency and a larger TDP, is also better in performance per watt in almost every project. There's a small easter egg too: the 9686F's initially published maximum boost was 5.0GHz, and the mass-production silicon hit 5.3GHz outright.

## Where This Architecture Is Advanced

AMD's positioning for Zen 6 this time is written plainly: flexible computing for the agent era. Agents don't work in a straight line—querying databases, running code, calling tools, spinning up sandboxes—and over one round the CPU side has to handle a large amount of concurrency and a large number of serial critical steps. So the product line is split into three roles: acting as host for GPUs, needing speed, corresponding to the 96-core 5.3GHz 9686F; running concurrent sandboxes, needing density, corresponding to the 256-core 9996; and the enterprise's existing databases, virtual machines, and business systems, left to next year's SP8.

The hardware trade-offs match this division of labor. Native FP16 and the 512-bit vector path let the CPU itself handle part of inference and matrix compute, the 16-channel 1.6TB/s bandwidth is responsible for delivering large contexts and datasets in time, and PCIe 6.0 doubles the width of the GPU path. With the high-frequency and high-density lines sharing the same core, AMD can configure SKUs by role, spanning from 8 cores at $700 all the way to 256 cores.

Engineering maturity has kept up too. Phoronix ran hundreds of tests on Ubuntu 26.10 and Linux 7.3; Ubuntu 26.04 LTS and the latest RHEL 10 work just as well, and GCC and LLVM/Clang Zen 6 support entered the mainline long ago. On launch day there are complete systems, cloud instances, and compiler and kernel support—more than paper specs, this makes me feel the architecture has truly landed.

There are three more cards to play. SP8 cuts memory channels to 8 but offers 128 PCIe 6.0 lanes per socket and 192 dual-socket, clearly aimed at storage- and I/O-intensive enterprise machines; Venice-X stacks 3D V-Cache to 1152MB L3 with up to 96 cores at 5.3GHz, targeting EDA and engineering simulation; Verano uses 24-channel LPDDR5X and replaceable SOCAMM2 modules, focusing on the energy efficiency of AI host nodes. All arrive successively in 2027.

## Final Thoughts

Zen 6 has only handed in its first paper, with three more families queued up behind it. I'm quite curious to see how much further the HPC column can rise once Venice-X stacks L3 to 1152MB.

Which follow-up results would you rather see? I'm betting on Venice-X—tell me who you're betting on in the comments.

### Sources

- Tom's Hardware, "AMD Zen 6 EPYC 'Venice' deep dive and benchmarking" (November 10, independent testing; SPEC data are unverified estimates)
- Phoronix, "AMD EPYC 9996 Benchmarks" and "AMD EPYC 9686F Linux Benchmarks" (November 10, independent Linux testing)
- AMD official blog "AMD EPYC 9006 SP7-based Platforms Are Now Shipping" and the official newsroom (November 10; specifications, availability, and cloud partnerships)
- AMD In Depth event (November 4), public materials on the Zen 6 microarchitecture and product families
- TOPCPU, Basic Tutorials (Zen 6 architecture summary, SKU price list, platform comparison)
- ServeTheHome, Tencent News / ZAKER (platform specifications and product family roadmap)

Note on methodology: "Official" data in this article are AMD's own testing and briefing figures; "measured" refers to independent testing by Tom's Hardware and Phoronix; SPEC CPU 2026 results are estimates and have not yet been submitted for SPEC certification. This article discusses only EPYC server processors and the Zen 6 architecture.


### Publishing Notes (not part of the body)

**Image list (generated, all bright and highly saturated, placed directly in the folder root)**

| File | Content | Suggested insertion point | 
|---|---|---|
| `封面.png` | 1536×1024 atmospheric version (default): an enlarged PCB circuit board macro as background, with traces, pads, vias, and scattered SMD components clearly visible; in the center, a three-dimensional metal frame folded from the geometric outline of the AMD logo, with a multi-chiplet EPYC package embedded in the square recess; orange-gold backlight bursts from the board surface at the upper left. Chinese text overlaid locally | WeChat article header image | 
| `封面-亮色版.png` | Cream white / bright orange / bright cyan-blue bright background, mirrored metal shell bursting open with fragments flying, close-up of a multi-chiplet package on a deep blue substrate; matches the established preference for bright, highly saturated visuals | Bright alternative | 
| `封面-备选.png` | Orange-black contrasting version, strong light-dark contrast, the most impactful | Dark alternative | 
| `figure-1.png` | 1280×720@2x spec card: 2nm / 256 cores / 1024 threads / 1.6TB/s, orange-yellow | Before the "The Specs Laid Out" section | 
| `figure-2.png` | Two large number cards for the measured report card: +93% and 1.58×, green palette | Before the "The Report Card" section | 
| `figure-3.png` | Zen 6 architecture diagram: dual IOD / 8×CCD / PCIe 6.0, blue-purple | Before the "Where This Architecture Is Advanced" section | 
| `figure-4.png` | Product family timeline: SP7 → SP8 → Venice-X / Verano, pink-orange-blue | After the "The First to Deliver Is the Server" section in Chapter 01 (actual layout position, as Figure 1) | 

**Source files**: `配图源文件/figure-1.html ~ figure-4.html` and `render.py`. After editing text, run `render.py` with system Python313 to overwrite and re-render; no need to rewrite the images. All four figures carry a "鹏叔大玩家" corner mark at the bottom right.

**Cover source files**: `配图源文件/封面底图-A.png`, `封面底图-B.png`, and `封面底图-C.png` are text-free generated base images; version C was produced from the reference image `kvpXFJbfLcJSNxLaK2gvRA.jpg` via image-to-image, locking in the PCB background and the AMD logo geometric frame aesthetic. `封面叠字.py` handles local overlay of the Chinese title: no text is written into the generated image, so Chinese text is 100% controllable with zero typos; to change the title, just edit the three constants `TITLE / SUB / TAG` in the script and rerun, and all three versions are regenerated at once. `封面-旧版-扁平插画.png` is a backup of the initial version.

**Cover notes**: The default `封面.png` is the atmospheric version; to restore the PCB circuit board texture, its base color is darker than the bright version, but the orange-gold highlights are highly saturated and the dark areas are clean; if you want to strictly adhere to bright and highly saturated, you can rename `封面-亮色版.png` to `封面.png`. All three versions carry the platform's "AI生成" label at the bottom right, unobscured; if you need to remove it, crop off a thin strip at the bottom.

**WeChat layout (generated, theme: olive-journal)**

- `公众号版_排版_橄榄手记(olive-journal)_预览.html` :**main deliverable** ; open in a browser → click "📋 一键复制到公众号" at the top → Ctrl/⌘+V in the WeChat backend.
- `公众号版_排版_橄榄手记(olive-journal).html` : pure section body fragment, no buttons, a fallback for manual drag-select copying.
- Assembled per the `gzh-design-skill` component library (data review / report recipe): header image card → numbered sections 01–05 → spec comparison table → 4 image cards → key viewpoint card → roadmap capsules → dark summary columns → sources card → byline and triple CTA; body paragraphs carry orange keyword underlines.
- Validated with `validate_gzh_html.py` :**0 ERROR / 0 WARNING** , 206`<span leaf="">` wrappers; body fragment styles fully inlined, with no`<style>` , class, id, grid, float, or other platform-prohibited items.
- The 4 figures are compressed to 1280-wide JPEGs embedded as data URIs, and the preview is self-contained (HTML 541KB). **If images are not automatically uploaded after pasting into WeChat, manually upload `figure-1.png ~ figure-4.png` to the asset library and insert them at the caption positions** ; the cover`封面.png` is uploaded separately in the backend and does not go into the body.
- Source files `排版源文件/build_layout.py` (rerun to rebuild after text changes) and`qa_layout.py` (browser acceptance and segmented screenshots). After changes, rerun in order: build →`validate_gzh_html.py` →`wrap_preview.py` .
- Browser acceptance: 4/4 images loaded successfully, no horizontal overflow, copy button visible and clicking shows "✅ 已复制", 0 JS errors, 5 section numbers consecutive, page 900×5933.

**Summary (WeChat description, about 100 characters)**: AMD's sixth-generation EPYC officially goes on sale today, and independent testing of the first mass-produced CPU on the Zen 6 architecture is unlocked at the same time. 256 cores, 1024 threads dual-socket, 2nm, 1.6TB/s, integer throughput up to 93% higher, geometric mean 1.58x across hundreds of workloads. This article explains the architecture changes and both report cards in one go.

**Publishing tip**: Chinese media are mostly listing specs today; this article is positioned as "Zen 6 first benchmarks + positive technical analysis," and is recommended for tonight's publishing window.

Written by our editorial team; tools listed here are tested or verified against public sources. Links point to official sites or GitHub repos for reference only — no paid placements.
