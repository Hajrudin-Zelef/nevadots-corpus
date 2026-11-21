---
id: collect-261121-veille-ia/hardware-semi/amd-epyc-embedded-9006-brings-256-zen-6-cores-and-pcie-gen-6-to-test-storage-and-firewall-systems
title: "AMD EPYC Embedded 9006 Brings 256 Zen 6 Cores and PCIe Gen 6 to Test, Storage, and Firewall Systems"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["epyc", "core", "embedded", "test", "storage", "cores", "pcie", "memory"]
source: https://www.storagereview.com/news/amd-epyc-embedded-9006-series-256-cores-pcie-gen-6?amp
source_anchor: ""
source_lines: [1, 25]
section: "Hardware_Semi"
---
AMD announced the EPYC Embedded 9006 Series today, bringing the Zen 6 cores of its 6th Gen EPYC server processors to the SP7 socket for embedded systems, with up to 256 cores and 512 threads, up to 96 lanes of PCIe Gen 6, CXL 3.1, and a seven-year product longevity commitment. The targets are semiconductor automated test equipment, enterprise storage controllers, and 400G and 800G firewalls. The 128-core part ships November 15, and the rest of the 64-core to 256-core lineup is due by the end of the year.

The processors carry sixteen DDR5 channels, with RDIMMs up to 8,000MT/s and MRDIMMs up to 12,800MT/s, and up to 96 PCIe Gen 6 lanes at 64GT/s that also serve CXL 3.1 memory expansion. AMD’s headline claim is up to 1.8x the throughput of the 5th Gen EPYC 9005 Series, from an estimated SPECrate2017_int_base of 4,900 for a two-socket 256-core system on MRDIMM-12800 against 2,740 for a two-socket EPYC 9965 on DDR5-6400. The company also cites up to 1.5x the I/O bandwidth and 2.7x the memory bandwidth of the prior generation, and the memory figure works out to sixteen channels at 12,800MT/s against twelve at 6,400MT/s.

For storage controllers, AMD positions the parts for NVMe connectivity, data movement, compression, caching, and RAID, and claims up to 1.5x the prior generation on the ISA-L storage acceleration library that underpins those services. Its configuration pairs a 128-core EPYC Embedded 9756 with 32 128GB DDR5-8000 modules against a 128-core EPYC 9755 with 24 24GB DDR5-6400 modules, both on Ubuntu 24.04 with AVX-512. The 1.3x firewall throughput claim comes from a DPDK L3 forwarding test on the same two processors with 400Gb/s of aggregate NIC bandwidth and one core per port. These are AMD’s numbers, and we did not run or audit them.

In semiconductor ATE systems, the CPUs manage memory and storage test workloads and the interfaces for DDR5 RDIMM, MRDIMM, PCIe Gen 6, and CXL 3.1 devices, and AMD says the core counts let more device tests run in parallel. Samsung, Lanner, Portwell, and IEI Integration endorsed the launch, with Lanner naming its NCA-6145 network appliance and Samsung its CXL memory modules.

AMD root of trust, or ART, establishes the silicon root of trust, authenticates firmware during secure boot, and supports post-quantum cryptography for firmware signing. The AMD Security Processor orchestrates boot, provides a trusted execution environment and authorized secure debug, and manages the keys that protect virtual machine memory. Caliptra, the open-source root-of-trust project, provides device identity and attestation so a remote party can verify the device, its BIOS, and its firmware.

The processors carry the enterprise reliability, availability, and serviceability feature set, with telemetry and predictive failure analysis available through supported platforms, and AMD commits to firmware updates and software support across the seven-year availability window.

The lineup spans 64 to 256 cores. The 128-core SKU, the EPYC Embedded 9756 named in AMD’s test configurations, ships November 15, and the remaining SKUs are expected to begin shipping by the end of the year. AMD has not published per-SKU clocks, cache, or power figures for the embedded parts; the server-side EPYC 9006 SKU list gives the shape of the family.

Dell Technologies says it is the first OEM to deliver a fully validated, direct liquid-cooled networking rack, and that the…

Graid Technology plans to bring VROC, the CPU-based NVMe RAID stack it took over from Intel, to AMD EPYC servers.…

HighPoint Technologies, best known for its NVMe RAID and switch adapters, has moved into Ethernet with the Rocket 9500A Series,…

AMD has released ROCm 10.2.0, a stability-focused update to its open GPU software stack that refreshes the serving frameworks, libraries,…

SC26 is under way at McCormick Place in Chicago, and behind the headline launches we have already covered, a second…

Hammerspace has turned its software-defined Data Platform into the Hammerspace Data System and, for the first time, will sell it…
