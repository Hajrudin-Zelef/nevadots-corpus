---
id: collect-261121-veille-ia/nvidia-hardware/amd-epyc-venice-often-beats-nvidia-vera-in-tests-a-origotech
title: "AMD EPYC Venice often beats Nvidia Vera in tests Â· OrigoTech"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-07"]
keywords: ["epyc", "vera", "hardware", "venice", "tomas", "amdas", "often", "ahead"]
source: https://origotech.dev/articles/amd-zen-6-epyc-venice-built-on-tsmc-n2p-often-beats-nvidia-vera-in-toms-hardware-tests-but-vera-leads-on-some-workloads-and-amd-s-own-agents-per-watt-claims-remain-company-figures/
source_anchor: ""
source_lines: [1, 64]
section: "NVIDIA_Hardware"
---
Analysis

# AMD Zen 6 EPYC Venice, built on TSMC N2P, often beats Nvidia Vera in Tomâs Hardware tests, but Vera leads on some workloads and AMDâs own agents-per-watt claims remain company figures

Tomâs Hardware found AMDâs EPYC 9686F often ahead of Nvidiaâs Vera, though not everywhere. AMDâs own efficiency claims are company figures.

AMD launched its 6th Gen EPYC 9006 âVeniceâ server processors on November 10, and Tomâs Hardware published a deep dive with benchmarks. âOften beats Nvidiaâs Veraâ is a fair summary of the headline, but the lead is not universal and the tested chip is a specific one. The most striking efficiency numbers are AMDâs own.

**The essentials**

- **The chips** : Venice for the SP7 platform offers up to 256 Zen 6 cores per socket, 16 DDR5 channels and PCIe 6.0, per MLQ.ai. Tomâs Hardware says the compute dies use TSMCâs N2P process.
- **Against Vera** : Tomâs Hardware reports the frequency-optimized EPYC 9686F 21% ahead of Vera in the integer suite, both in two-socket setups, and ahead by 11% to 19% in STREAM. MLQ.aiâs summary gives 17% on integer and 40% on floating point for the same chip.
- **Where Vera wins** : MLQ.ai says Vera led in memory bandwidth, C-Ray and Octave. That conflicts with the STREAM result above.
- **Generation gain** : a dual 256-core EPYC 9996 scored 93% above a dual EPYC 9965 in SPEC CPU 2026 integer throughput, per Tomâs Hardware. That compares AMD with its own previous generation.

## Independent tests versus company claims

The Tomâs Hardware figures come from its own testing. MLQ.ai describes AMDâs claim of 3.3 times more concurrent agents per watt than Vera as a company figure, not an independent test.

## Why the exact chip matters

The comparison that favors AMD uses a 96-core, high-frequency part. Results for the 256-core flagship against Vera depend on workload, and the relays disagree on some numbers.

## What is not established

- Why the STREAM and memory-bandwidth results differ between relays: not established.
- Independent confirmation of AMDâs agents-per-watt claim: none found.
- Pricing and availability of the tested configurations: not covered in the pages read.

## Summary table

| Summary table |  |  | 
|---|---|---|
| Point | What is stated as of November 7, 2026 | Source | 
|---|---|---|
| Process | Zen 6 compute dies use TSMC N2P | Mezha | 
| Specs | Up to 256 cores, 16 DDR5 channels and PCIe 6.0 per socket | MLQ.ai | 
| EPYC 9686F vs Vera | 21% ahead in integer, both 2P; ahead by 11% to 19% in STREAM | Tomâs Hardware | 
| Generation gain | Dual EPYC 9996 up to 93% above dual EPYC 9965 in SPEC CPU 2026 | Techmeme | 
| AMD efficiency claim | 3.3 times more concurrent agents per watt, a company figure | MLQ.ai | 

## FAQ

### Does AMDâs Venice always beat Nvidiaâs Vera?

No. Tomâs Hardware found AMD often ahead, but MLQ.aiâs summary says Vera led in memory bandwidth, C-Ray and Octave.

### Which chip was compared with Vera?

The 96-core, frequency-optimized EPYC 9686F, in two-socket configurations.

### Is the 3.3 times agents-per-watt figure independent?

No. It is an AMD claim, described as such by MLQ.ai.

## Sources

- Tomâs Hardware: AMD Zen 6 EPYC Venice deep dive and benchmarking (verified 2026-11-08)
- Techmeme: A deep dive into AMD's Zen 6 EPYC Venice (Jake Roach/Tom's Hardware) (verified 2026-11-08)
- MLQ.ai: AMD launches Zen 6 EPYC Venice server CPUs pitched at agentic AI (verified 2026-11-08)
- Mezha: AMDâs Zen 6 EPYC Venice: architecture and benchmark results (verified 2026-11-08)
- Aroged: AMD announced the start of shipments of the first Zen 6 chips (verified 2026-11-08)

Spotted an error in this article? Report it Â· Our corrections policy
