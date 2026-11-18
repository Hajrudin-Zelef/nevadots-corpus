# Step 4 — Track C: CUDA + ROCm + PyTorch (GPU compute stack)
## Research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 4 (Infra inference/training), Track C
**Coverage window:** February 1, 2026 → September 22, 2026
**Topics:** NVIDIA CUDA Toolkit, AMD ROCm, PyTorch 2.x, plus Triton, FlashAttention, HIP/CUDA translation, enterprise support
**Status:** Research snapshot. Version numbers and dates below were current as of the sources cited; re-verify against official pages before use.

### Provenance legend
- **[official]** — vendor's own blog, docs, changelog, release notes, GitHub release.
- **[vendor-reported]** — figure claimed by the vendor (benchmarks, perf claims) without independent audit.
- **[independent]** — reputable third-party press or independent measurement.
- **[secondary]** — lower-tier press, blogs, community docs, AI-generated summaries; useful but treat with caution.
- **[unverified]** — single-source or conflicting claims; treat as uncertain.

---

## 1. NVIDIA CUDA Toolkit

### 1.1 Latest release: CUDA 13.4 / 13.4 Update 1 (GA ~September 10, 2026)

NVIDIA released **CUDA Toolkit 13.4.1** (13.4 Update 1) with the CUDA 13.4 platform reaching general availability **[secondary — https://videocardz.com/newz/cuda-13-4-released-ready-for-rtx-spark, https://wccftech.com/nvidia-cuda-13-4-support-windows-on-arm-ahead-of-rtx-spark-launch/]**.

Key highlights of CUDA 13.4:
- **Windows-on-Arm support** — first native Windows Arm64 CUDA development package, timed ahead of RTX Spark PCs (Windows 11 on Arm systems with Grace CPU cores + Blackwell RTX graphics, shipping October 2026). NVCC, CUDA runtime, cuBLAS, cuFFT, and Nsight tools now support Windows Arm64; both on-device compilation and cross-compilation from Windows x86-64 are supported **[secondary — https://videocardz.com/newz/nvidia-releases-first-cuda-toolkit-preview-for-windows-on-arm-and-rtx-spark, https://videocardz.com/newz/cuda-13-4-released-ready-for-rtx-spark]**.
- **Preview support for NVIDIA Rubin (Vera Rubin) GPUs** — the toolkit adds the **SM_107** architecture target, identified as **compute capability 10.7**, allowing developers to compile and begin porting CUDA software before full Rubin support goes GA. NVIDIA states the preview is not intended for production deployment or performance benchmarking **[secondary — https://videocardz.com/newz/cuda-13-4-released-ready-for-rtx-spark, https://wccftech.com/nvidia-cuda-13-4-support-windows-on-arm-ahead-of-rtx-spark-launch/]**.
- **Multi-Process Service (MPS) V3** — modernized control layer with a scriptable CLI, named server instances, TOML configuration, and cgroup-integrated GPU memory limits for GPU partitioning in containerized environments **[secondary — https://wccftech.com/nvidia-cuda-13-4-support-windows-on-arm-ahead-of-rtx-spark-launch/]**.
- **CUDA Compute Fabric Transport** for NVLink-based communication **[secondary — https://videocardz.com/newz/cuda-13-4-released-ready-for-rtx-spark]**.
- Updates across the CUDA compiler, **PTX ISA 9.4**, CUDA C++, CUDA Tile programming, runtime APIs, and libraries **[secondary — https://wccftech.com/nvidia-cuda-13-4-support-windows-on-arm-ahead-of-rtx-spark-launch/]**; official 13.4 Update 1 release notes confirm PTX ISA 9.4 support and CUDA Toolkit Discovery via `CUDA_HOME`/`CUDA_PATH`/`CUDA_ROOT` environment variables (in cuDNN 9.24.0) **[official — https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html]**.

Developer preview timeline: CUDA 13.4 Developer Preview was released **July 19, 2026** with the first official Windows Arm64 package **[secondary — https://videocardz.com/newz/nvidia-releases-first-cuda-toolkit-preview-for-windows-on-arm-and-rtx-spark]**; GA followed ~September 10, 2026 **[secondary — https://videocardz.com/newz/cuda-13-4-released-ready-for-rtx-spark]**.

### 1.2 Component versions in CUDA 13.4 Update 1 (official)

From the official release notes, major component versions **[official — https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html]**:

| Component | Version |
|---|---|
| CUDA compiler (nvcc), runtime (cudart), NVRTC, CUPTI, Nsight toolset core | 13.4.92 |
| cuBLAS | 13.8.0.4 |
| cuFFT | 12.4.0.43 |
| cuSPARSE | 12.8.6.72 |
| cuSOLVER | 12.3.4.7 |
| cuRAND | 10.4.4.72 |
| NPP | 13.2.0.58 |
| nvJPEG | 13.2.3.58 |
| cuFile | 1.19.1.55 |
| Thrust / CUB / libcu++ (CCCL) | 3.4.3 |
| Nsight Compute | 2026.3.1.2 |
| Nsight Systems | 2026.3.2.476 |
| Nsight Visual Studio Edition | 2026.3.0.26187 |

cuBLAS highlights in the 13.4 line: **emulated FP64 matrix multiplications** with up to **25% ZGEMM peak-performance improvement on Rubin GPUs**; when per-handle workspace is insufficient, fixed-point FP64 emulation now allocates temporary workspace from a cuBLAS-managed per-device CUDA memory pool retained across stream synchronizations **[official — https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html]**.

Known issues (official): cuBLASLt Grouped GEMM with per-batch tensor-wide scales can cause invalid memory access for groups with `m > 0, n > 0, k = 0` (introduced in CUDA 13.1); cuBLASLt Grouped GEMM can produce incorrect results on **Blackwell and Rubin** GPUs (introduced in CUDA 13.4, cuBLAS 13.7.0) **[official — https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html]**.

Since **March 9, 2026**, cuBLAS patch releases are published **independently of CUDA Toolkit releases** for critical bug fixes **[official — https://docs.nvidia.com/cuda/archive/13.0.3/pdf/CUDA_Toolkit_Release_Notes.pdf]**.

### 1.3 cuDNN 2026 track

- **cuDNN Backend 9.24.0** (June–July 2026): CUDA Toolkit 13.3 support; SDPA backward `d=256` on Blackwell (2-CTA MMA path, bfloat16/float16); Ubuntu 26.04 support; CUDA Toolkit discovery via environment variables; expanded causal conv1d via SubquadraticOps API with NVRTC runtime compilation **[secondary — https://github.com/nvidia/cudnn-frontend/issues/442]**.
- **cuDNN Frontend v1.29.0** (released ~September 13, 2026) is the recommended frontend for **cuDNN 9.26** and later; it adds HSTU (Hierarchical Sequential Transduction Unit) attention as a complete CuTe DSL kernel family for Blackwell — packed variable-length forward/backward, FP16/BF16, head dims 64/128/256, full/causal/local/arbitrary masks, paged-KV forward — plus optimized qlen=1 causal/local attention on SM100, SM103, and **SM107** (Rubin) **[secondary — https://github.com/NVIDIA/cudnn-frontend/releases/tag/v1.29.0]**.
- **cuDNN Frontend v1.26.0** (July 2026): unified-engine FP8 and MXFP8 forward SDPA (requires cuDNN 9.25.0+); SDPA backward `d=256` native path on cuDNN 9.23+; block-sparse attention CuTe DSL kernels for Hopper and Blackwell; DeepSeek Sparse Attention (DSA) kernels with q-causal offsets and SM100F support; grouped-GEMM quant wrapper for SM100 **[secondary — https://github.com/NVIDIA/cudnn-frontend/releases/tag/v1.26.0]**.
- Supported architectures this period: Blackwell (B200, B300; compute capability 10.0/10.3/12.0), Hopper (H200/H20; 9.0), Ada Lovelace (L40S, RTX PRO 6000; 8.9), Ampere (A100; 8.0) **[secondary — https://github.com/nvidia/cudnn-frontend/issues/442]**.

### 1.4 Driver requirements (official)

Driver-branch mapping **[official — https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html]**:

| CUDA Toolkit | Driver branch |
|---|---|
| CUDA 13.4 | **R615** |
| CUDA 13.3 | R610 |
| CUDA 13.2 | R595 |
| CUDA 13.1 | R590 (Windows minimum 581.15) |
| CUDA 13.0 | R580 |

- Minor-version compatibility: existing CUDA 13.x applications run on drivers **≥ 580**; CUDA 13.4 new features/newly enabled platforms require an **R615+ driver** (Windows driver for RTX Spark devices: **616.41 or later**). The driver is **no longer bundled** with the toolkit (Windows since CUDA 13.1, Linux since CUDA 13.4) **[official — https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html]**.
- Community corroboration: llama.cpp CUDA 13 builds require the documented 581.15 minimum driver for CUDA 13.1 **[secondary — https://github.com/atomicbot-ai/atomic-chat/blob/HEAD/docs/decisions/2026-05-26-correct-cuda-13-1-driver-gate-to-nvidia-documented-581-15-and.md]**.

### 1.5 Deprecations and 2026 housekeeping (official)

- **Python 3.10 deprecated** across CUDA Python 13.4 packages **[official — https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html]**.
- Legacy **Nsight Eclipse Edition** plugins no longer delivered in toolkit packages beginning with CUDA 13.3 **[official — https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html]**.
- **CUDA 14.0** will move to **Armv8.2-A** as the minimum supported architecture for ARM64-SBSA **[official — https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html]**.

⚠️ **Verification gap:** exact release dates for CUDA 13.0, 13.1, 13.2, and 13.3 were not confirmed from official notes in this research. Bundling evidence: PyTorch 2.12 wheels (July 2026) shipped CUDA 13.0.2, and PyTorch 2.13 wheels (July 20, 2026) shipped CUDA 13.3.0, so CUDA 13.0 GA'd by ~June 2026 and 13.3 by mid-July 2026 **[secondary — https://github.com/aws/deep-learning-containers/blob/HEAD/docs/pytorch/changelog/index.md]**.

---

## 2. AMD ROCm

### 2.1 Latest release: ROCm 10.0.0 (~August 26–28, 2026)

AMD released **ROCm 10.0.0 (ROCm Core SDK 10.0.0)** — the first major version jump after the 7.x series, announced around August 26–28, 2026, marking 10 years since ROCm 1.0 (April 2016) **[secondary — https://wccftech.com/amd-rocm-10-big-ai-updates-performance-gains/, https://www.thelec.net/news/articleView.html?idxno=13453, https://github.com/datawhalechina/hello-rocm/blob/HEAD/docs/en/00-environment/rocm-10-0-0-release-notes.md]**.

Headline changes:
- Built on **[TheRock](https://github.com/ROCm/TheRock)** as the production build and release foundation (the 7.x line completed the move from a monolithic bundle to a modular Core SDK) **[secondary — https://github.com/datawhalechina/hello-rocm/blob/HEAD/docs/en/00-environment/rocm-10-0-0-release-notes.md]**.
- **ROCm.AI** — an AI-assistant-native developer workflow folding install, coding, and performance work into three entry points:
  - **AMD Skills** — official AMD optimization knowledge packaged as skills for AI coding agents (Claude Code, Cursor, Codex).
  - **Hyperloom** (rendered "ROCm HyperRoom" in press) — open-source auto-optimization engine: profile, find bottlenecks, rewrite kernels, tune parameters, validate; uses an AI agent for GPUs to optimize inference workloads **[secondary — https://www.thelec.net/news/articleView.html?idxno=13453]**.
  - **ROCm CLI & Console** — one command surface for install/verify/deploy/manage with real-time monitoring through a conversational interface **[secondary — https://www.thelec.net/news/articleView.html?idxno=13453]**.
- Support spans Instinct accelerators, Radeon graphics, and Ryzen integrated graphics on **both Windows and Linux** **[secondary — https://wccftech.com/amd-rocm-10-big-ai-updates-performance-gains/]**.
- Performance claims (AMD, **vendor-reported**): on the same hardware, **3.3× inference performance** and **2.4× training performance** vs ROCm 7. Testing by AMD Performance Labs as of July 7, 2026, on an 8× MI355X platform comparing ROCm 7.0 against a preview ROCm.AI build (ROCm 7.2.2 with optimized kernels/parallelism/scheduling), running GLM-5, Kimi-K2.5, and DeepSeek-R1-0528 **[secondary — https://wccftech.com/amd-rocm-10-big-ai-updates-performance-gains/]**.
- Packaging: ships **vLLM and SGLang containers**, Python wheels, and modular software packages; the pip wheel index moved from `repo.amd.com/rocm/whl-multi-arch/` to `https://stable.repo.amd.com/rocm/whl-next/`; apt/dnf repos moved to stable.repo.amd.com **[secondary — https://www.thelec.net/news/articleView.html?idxno=13453, https://github.com/datawhalechina/hello-rocm/blob/HEAD/docs/en/00-environment/index.md]**.

### 2.2 ROCm 7.x track (2025–mid-2026)

| Version | Date | Key changes | Provenance |
|---|---|---|---|
| ROCm 7.0 | Late 2025 | MI350X/MI355X GA support (CDNA 4), HIP compiler major update, MX data types (FP4/FP6/FP8) on MI350X, up to 3.8× perf vs ROCm 6.0 (vendor-reported), vLLM-d/DeepEP distributed inference, GPU-direct communications | [independent — https://www.techpowerup.com/341074/amd-launches-rocm-7-0-up-to-3-8x-performance-uplift-over-rocm-6-0] |
| ROCm 7.1 | Early 2026 | Deeper MI350X/MI355X optimization, improved FlashAttention, HIP Python improvements | [secondary — https://www.spheron.network/blog/cuda-news-today-nvidia-toolkit-amd-rocm-ai-frameworks-2026/] |
| ROCm 7.2 | ~March 2026 | RDNA4 support, Ubuntu 24.04.x support, RCCL improvements; Triton v3.3 shipped with first-class AMD backend | [secondary — https://www.spheron.network/blog/cuda-news-today-nvidia-toolkit-amd-rocm-ai-frameworks-2026/, https://github.com/damustermann/rocm-wsl-ai/blob/HEAD/CHANGELOG.md] |
| ROCm 7.2.3 | May 2026 | vLLM profiling stability, embedding-inference optimization | [secondary — https://www.spheron.network/blog/cuda-news-today-nvidia-toolkit-amd-rocm-ai-frameworks-2026/] |
| ROCm 7.14.0 (?) | 2026 | Reportedly completed the TheRock production transition | **[unverified — conflicting with the 7.2.x line; see §5]** |

GPU support status: MI300X **stable** since ROCm 7.0; MI350X/MI355X **stable** since ROCm 7.0; **MI400 in developer preview**; minimum kernel 6.8 for the 7.x line **[secondary — https://www.spheron.network/blog/cuda-news-today-nvidia-toolkit-amd-rocm-ai-frameworks-2026/]**.

### 2.3 PyTorch/JAX on ROCm

- Official AMD PyTorch wheels are published from repo.radeon.com/repo.amd.com; e.g., **PyTorch 2.9.1** official AMD wheels (March 2026 community docs) **[secondary — https://github.com/damustermann/rocm-wsl-ai/blob/HEAD/CHANGELOG.md]**.
- PyTorch 2.12's official release notes list ROCm platform gains: **expandable memory segments**, **rocSHMEM symmetric-memory collectives**, and **FlexAttention pipelining** on ROCm **[official via secondary — https://github.com/pytorch/pytorch/releases]**.
- PyTorch 2.13 release material lists **expanded ROCm platform support** **[secondary — https://www.youtube.com/watch?v=vbdduGRTQC0]**.
- AMD's ROCm PyTorch support matrix (rocm/pytorch RELEASE.md) tracks stable CUDA/ROCm combinations per PyTorch release **[secondary — https://github.com/rocm/pytorch/blob/HEAD/RELEASE.md]**.
- ROCm Windows note: the full toolchain (rocminfo, amd-smi, multi-GPU, containerized deployment) is best on Ubuntu 24.04; Windows covers quick inference and experimentation **[secondary — https://github.com/datawhalechina/hello-rocm/blob/HEAD/docs/en/00-environment/index.md]**.

### 2.4 HIP / CUDA translation tooling

- AMD's HIPify (hipify-perl, hipify-clang) remains the standard CUDA→HIP source-translation path; no major 2026 HIPify-specific announcements were captured in this research window — flag as a gap.
- Triton is increasingly the portability vehicle: vLLM's Triton attention backend runs identical kernel source on CUDA/H100 and ROCm/MI300-class hardware, though **tuning does not transfer** (different optimal block sizes and scheduling knobs per backend) **[secondary — https://www.spheron.network/blog/triton-on-amd-rocm-vs-nvidia-cuda-same-kernel-different-perf/]**.

---

## 3. PyTorch 2.x (2026)

### 3.1 Release timeline

| Version | Release date | Notes |
|---|---|---|
| 2.11.0 | ~April 30, 2026 | Enterprise-deployment features, FlashAttention integration for Blackwell/Hopper (per AI-generated summary — [unverified]), XPU Graph support |
| 2.12.0 | ~June 2026 (2.12.1 in DLC Jul 2, 2026) | torch.accelerator.Graph API, batched linalg.eigh up to 100× faster on CUDA, torch.export.save MX quantization, fused Adagrad, torch.cond in CUDA Graphs, ROCm memory/collective gains |
| 2.13.0 | July 20, 2026 | CuTeDSL "Native DSL" Inductor backend, FlexAttention on Apple Silicon (~12×), nn.LinearCrossEntropyLoss (4× peak-memory cut), torchcomms, FSDP2 comm-overlap, Python 3.15 (+ free-threaded 3.15t) wheels, 3,328 commits / 526 contributors |
| 2.14.0 | September 2, 2026 | clamp/min/max boundary-subgradient change (1→0 at boundaries — silent numeric behavior change), bfloat16 complex promotion now raises, cholesky/qr removed; verify behavior by running before/after upgrade |

Sources: PyTorch 2.13/2.12 release Q&A and GitHub release notes **[secondary — https://www.youtube.com/watch?v=vbdduGRTQC0, https://github.com/pytorch/pytorch/releases, http://dev.to/tsho/pytorch-214-clamps-boundary-gradient-silently-changes-bfloat16-complex-promotion-now-raises-26g1]**; AWS DLC changelog for dates/CUDA bundling **[secondary — https://github.com/aws/deep-learning-containers/blob/HEAD/docs/pytorch/changelog/index.md]**.

### 3.2 Compilation (torch.compile / Inductor)

- **PyTorch 2.13** introduces the **CuTeDSL "Native DSL" backend** for key GPU operations in Inductor **[secondary — https://www.youtube.com/watch?v=vbdduGRTQC0]**.
- **FlexAttention on Apple Silicon** with up to ~12× speedup over SDPA on sparse patterns **[secondary — https://www.youtube.com/watch?v=vbdduGRTQC0]**.
- **Deterministic backward computation on CUDA** (2.13) **[secondary — https://www.youtube.com/watch?v=vbdduGRTQC0]**.
- **nn.LinearCrossEntropyLoss** reduces peak GPU memory by up to **4×** during large-vocabulary LM training **[secondary — https://www.youtube.com/watch?v=vbdduGRTQC0]**.
- **Device-agnostic `torch.accelerator.Graph` API** (2.12) unifies graph capture and replay across CUDA, XPU, and out-of-tree backends **[official via secondary — https://github.com/pytorch/pytorch/releases]**.
- `torch.cond` control flow can now be captured and replayed inside CUDA Graphs (2.12) **[official via secondary — https://github.com/pytorch/pytorch/releases]**.
- Build requirements tightened in 2.12: **minimum CUDA 12.6** to build from source, **C++20** minimum enforced **[official via secondary — https://github.com/meta-pytorch/torch-release-notes/blob/HEAD/2.12.0/final.md]**.
- Triton pinned to **3.7.1** in 2.12.x to fix a nondeterministic FLASH_ATTN output regression and an illegal-memory-access in the Triton `convolution2d_bwd_weight` kernel on B100/B200 (sm100) GPUs **[secondary — https://github.com/pytorch/pytorch/releases]**.
- PyTorch **2.6** (Jan 2026) deprecated the official **Anaconda channel**; **2.12.1** dropped CPython **3.13t** from the binary build matrix; **2.13** adds wheels for **Python 3.15** including free-threaded 3.15t builds on Linux **[secondary — https://ngontinh24.com/article/pytorch-2-6-release-blog-pytorch, https://github.com/pytorch/pytorch/releases, https://www.youtube.com/watch?v=vbdduGRTQC0]**.

### 3.3 Distributed training

- **torchcomms** for large-cluster training (2.13) **[secondary — https://www.youtube.com/watch?v=vbdduGRTQC0]**.
- **FSDP2 communication-overlap improvements** (2.13) **[secondary — https://www.youtube.com/watch?v=vbdduGRTQC0]**.
- NCCL bumps: NCCL **2.26.2** in PyTorch 2.11/2.12 images; NCCL **2.30.7** + EFA 1.49.0 in PyTorch 2.13 DLC images for improved multi-node collective performance **[secondary — https://github.com/aws/deep-learning-containers/blob/HEAD/docs/pytorch/changelog/index.md]**.
- ROCm side: rocSHMEM symmetric-memory collectives (2.12); RCCL in the ROCm stack **[secondary]**.

### 3.4 Quantization and torchao

- `torch.export.save` supports **microscaling (MX) quantization formats** (2.12), enabling full export of aggressively compressed models — relevant to FP4/FP6/FP8 on Blackwell and MI350X **[official via secondary — https://github.com/pytorch/pytorch/releases]**.
- ⚠️ **Gap:** no 2026-specific torchao releases were captured in this research window; torchao remains the quantization toolkit used by vLLM/SGLang serving stacks, but current version numbers are unverified — re-check the torchao GitHub releases before citing.

### 3.5 CUDA architecture support in PyTorch 2.12–2.14 wheels (official support matrix)

From AMD's rocm/pytorch RELEASE.md matrix (current as of September 2026) **[secondary — https://github.com/rocm/pytorch/blob/HEAD/RELEASE.md]**:

| CUDA build | Linux x86 + Windows arch coverage |
|---|---|
| 12.6.3 | Maxwell (5.0), Pascal (6.0), Volta (7.0), Turing (7.5), Ampere (8.0/8.6), Hopper (9.0) |
| 13.0.2 | Turing (7.5), Ampere (8.0/8.6), Hopper (9.0), Blackwell (10.0, 12.0+PTX; Linux-only +PTX) |
| 13.2.1 | Turing (7.5), Ampere (8.0/8.6), Hopper (9.0), Blackwell (10.0, 12.0+PTX; Linux-only +PTX) |

Linux aarch64 adds Blackwell 11.0 on the 13.0.2/13.2.1 lines. Note: Blackwell compute capabilities (sm_100/sm_120) were first introduced in **CUDA 12.8**; nvcc 12.6 cannot emit them **[secondary — https://github.com/mesh-llm/mesh-llm/blob/HEAD/docs/cuda-release-lanes.md]** — relevant for local-inference builds (llama.cpp etc.) targeting RTX 50-series cards.

---

## 4. Triton compiler (2026)

- Triton ships as **v3.3 in ROCm 7.0** with a **first-class AMD backend**: CDNA-specific GPU-IR layouts (`amd_mfma` matrix core, `amd_wmma`, blocked/shared/sliced), tunables `matrix_instr_nonkdim` (16→mfma_16x16, usually winning on MI300X; 32→mfma_32x32), `waves_per_eu`, `kpack`; native FNUZ FP8 (`fp8e4b8`/`fp8e5b16`) on CDNA3; XF32 on gfx942; dedicated `ROCm/triton` fork plus `occ.sh` occupancy tooling; FlashAttention fallback path on AMD via `FLASH_ATTENTION_TRITON_AMD_ENABLE=TRUE`; MIT license **[secondary — https://github.com/amd-agi/geak/blob/HEAD/perf_knowledge/landscape/authoring_dsls.md]**.
- PyTorch 2.12.x pins Triton **3.7.1** to fix sm100 (B100/B200) correctness regressions **[secondary — https://github.com/pytorch/pytorch/releases]**.
- Portability caveat (Spheron, September 2026): identical Triton attention kernels run on both CUDA/H100 and ROCm/MI300X, but **tuning does not transfer** — AMD's fastest config uses smaller BLOCK_M (16), AMD tunes `waves_per_eu` where NVIDIA tunes warp specialization; **HIP graphs cut MI300-family latency ~2×** on short sequences vs ~6% from CUDA graphs on H100; hand-tuned attention kernels still beat Triton's AMD backend by ~1.53× (0.36ms vs 0.55ms) on MI300X in TileLang comparisons **[secondary — https://www.spheron.network/blog/triton-on-amd-rocm-vs-nvidia-cuda-same-kernel-different-perf/]**.
- AMD's **GEAK** Triton agents use the AMD layout/tunable vocabulary as agent search dimensions **[secondary — https://github.com/amd-agi/geak/blob/HEAD/perf_knowledge/landscape/authoring_dsls.md]**.
- Intel also maintains an XPU backend for Triton (out-of-tree) **[secondary — https://github.com/intel/intel-xpu-backend-for-triton/blob/HEAD/RELEASE.md]**.

## 5. FlashAttention

- **flash-attn 2.8.3** is the pinned version in PyTorch 2.11/2.12/2.13 AWS DLC images (fused attention) **[secondary — https://github.com/aws/deep-learning-containers/blob/HEAD/docs/pytorch/changelog/index.md]**.
- A "FlashAttention-4 integration for Blackwell/Hopper" claim appears in an AI-generated PyTorch 2.11 research summary **[unverified — https://github.com/edsonesf/atu-csd-pokedex/blob/HEAD/academic/researches/PyTorch/gemini-pytorch.md]** — the official flash-attn package numbering (2.8.3) and Dao-AILab's FlashAttention-3 (2024) were the last independently verified milestones. **Treat "FlashAttention-4" as unverified until checked against dao-ailab/flash-attention releases.**
- cuDNN-side attention kernels (SDPA fprop now open-sourced via cuDNN Frontend, DSA, block-sparse attention) increasingly overlap FlashAttention's niche on Blackwell — see §1.3 **[secondary — https://github.com/NVIDIA/cudnn-frontend/releases/tag/v1.26.0]**.

## 6. AMD vs NVIDIA for local/enterprise inference (2026 snapshot)

- **NVIDIA local:** RTX 50-series (Blackwell, sm_120) requires CUDA 12.8+ toolkits and drivers; **RTX Spark** (Grace CPU + Blackwell RTX, 6,144 or 5,120 CUDA cores, up to 128 GB unified LPDDR5X) arrives **October 2026**, with CUDA 13.4 GA providing the Windows-on-Arm toolkit ahead of launch **[secondary — https://videocardz.com/newz/cuda-13-4-released-ready-for-rtx-spark]**.
- **AMD local:** ROCm 10.0.0 supports Radeon graphics and Ryzen iGPUs on Windows; ROCm 7.2 added RDNA4 support; Windows ROCm is positioned for quick inference/experimentation, with Ubuntu 24.04 recommended for the full toolchain **[secondary — https://wccftech.com/amd-rocm-10-big-ai-updates-performance-gains/, https://www.spheron.network/blog/cuda-news-today-nvidia-toolkit-amd-rocm-ai-frameworks-2026/]**.
- **Serving stacks:** ROCm ships **vLLM and SGLang containers** and Python wheels as first-class artifacts (both ROCm 10 and the 7.x line); AMD touts distributed inference via vLLM-d and DeepEP compatibility **[secondary — https://www.thelec.net/news/articleView.html?idxno=13453, https://www.techpowerup.com/341074/amd-launches-rocm-7-0-up-to-3-8x-performance-uplift-over-rocm-6-0]**.
- **Enterprise:** NVIDIA's **AI Enterprise** platform (per earlier step-3 research, ~$4,500/GPU/yr list) bundles the CUDA/NIM stack with enterprise support — treat pricing as [secondary/unverified] pending re-check. AMD offers ROCm enterprise support via its partner/OEM ecosystem; no 2026-specific pricing or program changes were captured in this window — **flag as gap**.

## 7. Open verification items

1. Exact GA dates for CUDA 13.0, 13.1, 13.2, 13.3 (only 13.4 verified: dev preview Jul 19, 2026; GA ~Sep 10, 2026).
2. ROCm 7.x sequence conflict: Spheron tracker lists 7.2.3 (May 2026) as latest 7.x; community docs reference **7.14.0** completing the TheRock transition. One or both may be mislabeled (7.2.x patches vs an internal numbering) — needs the official ROCm release-notes page.
3. ROCm 10.0.0 exact date: Aug 26 vs Aug 28, 2026 across sources.
4. cuDNN 9.25.0/9.26.0 detailed release notes (frontend v1.29.0 "recommended for 9.26" implies it exists, but backend notes not fetched).
5. "FlashAttention-4" — unverified; check dao-ailab/flash-attention.
6. PyTorch 2.11.0 official release date and full highlights (currently from AI-generated summary).
7. torchao current version and 2026 changes — not captured.
8. HIPify 2026 improvements — not captured.
9. NVIDIA AI Enterprise 2026 pricing and AMD ROCm enterprise support program details.
10. vLLM / SGLang version-specific ROCm-CUDA parity measurements (independent, not vendor claims).
11. torchcomms details (topology support, backends) beyond the 2.13 release headline.
12. CUDA 14.0 release date — only the Armv8.2-A minimum note is confirmed.

## 8. Collection metadata

- **Collected:** September 22, 2026.
- **Primary sources used:** NVIDIA CUDA Toolkit 13.4/13.4-Update-1 release notes (docs.nvidia.com) [official]; cuDNN Frontend GitHub releases [secondary/official repo]; PyTorch GitHub releases + meta-pytorch release-notes worksheets [secondary/official repo]; rocm/pytorch RELEASE.md support matrix [secondary/official repo]; AMD community ROCm docs (datawhalechina/hello-rocm, damustermann/rocm-wsl-ai) [secondary].
- **Press used:** VideoCardz, Wccftech, TechPowerUp, TheElec, Spheron blog, TensorOpera/dev.to community notes [secondary/independent where noted].
- **Notes:** Dates for press-covered releases reflect article publication dates; official docs may list slightly different GA dates. Performance figures from AMD/NVIDIA are vendor-reported. "Unverified" items are explicitly tagged in §7.
