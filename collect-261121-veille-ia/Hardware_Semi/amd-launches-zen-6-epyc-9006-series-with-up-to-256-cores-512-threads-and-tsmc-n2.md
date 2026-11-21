---
id: collect-261121-veille-ia/hardware-semi/amd-launches-zen-6-epyc-9006-series-with-up-to-256-cores-512-threads-and-tsmc-n2
title: "AMD launches Zen 6 Epyc 9006 series with up to 256 cores, 512 threads, and TSMC N2"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["epyc", "threads", "cores", "clock", "also", "first", "lineup", "series"]
source: https://www.techspot.com/news/114513-amd-launches-zen-6-epyc-9006-series-up.html
source_anchor: ""
source_lines: [1, 60]
section: "Hardware_Semi"
---
**Highly anticipated:** AMD has fully pulled back the curtain on its first Zen 6 CPU lineup. Debuting with Epyc server chips, the new architecture offers up to 256 cores and 512 threads at the top end, and AMD has promised substantial upgrades in AI, GPU hosting, and per-watt performance. 

AMD's first Zen 6 Epyc processors, labeled the 9006 series and codenamed Venice, are now shipping to server clients, with SP7 systems arriving first and SP8 following in the first half of 2027. The company recently published the full technical specs for every SP7 and SP8 CPU in the lineup, mainly promoting their usefulness for AI workloads.

As expected, AMD's new flagship Epyc 9996 raises the maximum core count to 256, with 512 threads, clocked at up to 4.1GHz. With 1GB of L3 cache, it sits at a 2.55GHz base clock and defaults to 600W, though it can be configured down to 400W.

AMD's rack-density comparisons also reference a 400W, 256-core 9956, but that chip isn't on the published list. According to AMD's internal benchmarks, the 9996 at least doubles the Intel Xeon 6980P's performance in some tasks and more than triples it in others.

Early independent testing backs up the scale of the gains: Tom's Hardware measured a dual 9996 server at up to 93% faster than a dual Epyc 9965 in SPEC CPU 2026's intrate test. AMD lists the top-end chip at $14,904, but the lineup also scales all the way down to the eight-core, 16-thread Epyc 9016, at just $700.

Shifting to TSMC's N2 process node grants Epyc Venice up to a 2.6x improvement in performance per watt. AMD also claims 1.87x as many AI agents per CPU dollar compared to recent Intel Xeon processors.

With a 5.3GHz maximum boost clock on its high-frequency host-node parts, Venice also supposedly delivers up to 1.8x the LLM inference output of the previous Epyc generation. PCIe 6.0 doubles CPU-to-GPU I/O bandwidth, with up to 128 lanes in dual-socket SP7 systems and on SP8.

| Name | # of CPU Cores | # of Threads | Max. Boost Clock | Base Clock | L3 Cache | Default CPU Power | 
|---|---|---|---|---|---|---|
| AMD Epyc 9996 | 256 | 512 | Up to 4.1 GHz | 2.55 GHz | 1024 MB | 600W | 
| AMD Epyc 9956 | 256 | 512 | Up to 4.1 GHz | 1.7 GHz | 1024 MB | 400W | 
| AMD Epyc 9966 | 192 | 384 | Up to 4.1 GHz | 3.05 GHz | 768 MB | 600W | 
| AMD Epyc 9846 | 168 | 336 | Up to 4.1 GHz | 2.9 GHz | 768 MB | 500W | 
| AMD Epyc 9756 | 128 | 256 | Up to 4.1 GHz | 3.2 GHz | 512 MB | 500W | 
| AMD Epyc 9G76 | 96 | 192 | Up to 5.3 GHz | 3.65 GHz | 384 MB | 500W | 
| AMD Epyc 9686F | 96 | 192 | Up to 5.3 GHz | 4.05 GHz | 384 MB | 600W | 
| AMD Epyc 9656 | 96 | 192 | Up to 4.1 GHz | 2.85 GHz | 512 MB | 400W | 
| AMD Epyc 9586F | 64 | 128 | Up to 5.3 GHz | 4.05 GHz | 384 MB | 400W | 
| AMD Epyc 9556 | 64 | 128 | Up to 5.1 GHz | 3.6 GHz | 192 MB | 400W | 

Per-socket memory bandwidth reaches up to 1.6 TB/s. AMD also maintained its claim that Zen 6 provides up to 3.3 times as many agents per CPU-watt as Nvidia Vera.

Since AMD recently declared itself a data center-first company, it remains unclear exactly when Zen 6's architectural advantages will filter down to consumer-grade products. The successor to the Ryzen 9000 CPU lineup, presumably labeled Ryzen 10000, is now widely expected to arrive in 2027, with a CES announcement and first-half availability the most likely window.

| Name | # of CPU Cores | # of Threads | Max. Boost Clock | Base Clock | L3 Cache | Default CPU Power | 
|---|---|---|---|---|---|---|
| AMD Epyc 9746 | 128 | 256 | Up to 4.1 GHz | 2.9 GHz | 512 MB | 400W | 
| AMD Epyc 9736P | 128 | 256 | Up to 4.1 GHz | 2.7 GHz | 256 MB | 360W | 
| AMD Epyc 9736 | 128 | 256 | Up to 4.1 GHz | 2.7 GHz | 256 MB | 360W | 
| AMD Epyc 9726N | 128 | 256 | Up to 4.1 GHz | 2.15 GHz | 512 MB | 280W | 
| AMD Epyc 9676F | 96 | 192 | Up to 5.3 GHz | 3.1 GHz | 384 MB | 400W | 
| AMD Epyc 9646P | 96 | 192 | Up to 4.1 GHz | 2.8 GHz | 256 MB | 300W | 
| AMD Epyc 9646 | 96 | 192 | Up to 4.1 GHz | 2.8 GHz | 256 MB | 300W | 
| AMD Epyc 9576F | 64 | 128 | Up to 5.3 GHz | 3.55 GHz | 384 MB | 400W | 
| AMD Epyc 9536P | 64 | 128 | Up to 4.1 GHz | 3.25 GHz | 256 MB | 300W | 
| AMD Epyc 9536 | 64 | 128 | Up to 4.1 GHz | 3.25 GHz | 256 MB | 300W | 
| AMD Epyc 9526 | 64 | 128 | Up to 4.1 GHz | 2.75 GHz | 256 MB | 220W | 
| AMD Epyc 9476F | 48 | 96 | Up to 5.3 GHz | 3.65 GHz | 192 MB | 330W | 
| AMD Epyc 9456P | 48 | 96 | Up to 4.1 GHz | 3.2 GHz | 256 MB | 265W | 
| AMD Epyc 9456 | 48 | 96 | Up to 4.1 GHz | 3.2 GHz | 256 MB | 265W | 
| AMD Epyc 9376F | 32 | 64 | Up to 5.3 GHz | 3.8 GHz | 192 MB | 285W | 
| AMD Epyc 9356P | 32 | 64 | Up to 4.8 GHz | 3.6 GHz | 192 MB | 250W | 
| AMD Epyc 9356 | 32 | 64 | Up to 4.8 GHz | 3.6 GHz | 192 MB | 250W | 
| AMD Epyc 9336 | 32 | 64 | Up to 4.1 GHz | 3.15 GHz | 128 MB | 195W | 
| AMD Epyc 9276F | 24 | 48 | Up to 5.3 GHz | 3.8 GHz | 96 MB | 230W | 
| AMD Epyc 9256 | 24 | 48 | Up to 4.8 GHz | 2.85 GHz | 96 MB | 190W | 
| AMD Epyc 9176F | 16 | 32 | Up to 5.3 GHz | 3.9 GHz | 192 MB | 200W | 
| AMD Epyc 9116 | 16 | 32 | Up to 4.8 GHz | 2.85 GHz | 48 MB | 160W | 
| AMD Epyc 9016 | 8 | 16 | Up to 4.8 GHz | 3.05 GHz | 48 MB | 130W | 

Those processors are expected to support existing AM5 socket motherboards and raise the maximum core count to 24. The lineup's flagship may also reach a landmark 7GHz boost clock with an astounding 288MB of L3 cache.

Low-power Zen 6 CPU cores could also scale down to a new tier for portable devices further out, including a rumored PlayStation handheld, while new PlayStation and Xbox consoles are expected to debut with Zen 6 processors in 2027 or 2028.
