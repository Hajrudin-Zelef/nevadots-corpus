---
id: collect-261121-veille-ia/hardware-semi/portable-speed-still-needs-four-real-lanes
title: "Portable speed still needs four real lanes."
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["portable", "simd", "package", "still", "speed", "real", "four", "lanes"]
source: https://www.pimpmyide.com/vector-lane-dyno.html
source_anchor: ""
source_lines: [1, 28]
section: "Hardware_Semi"
---
## One source file can address several vector widths.

Go 1.27 adds an experimental, platform-independent `simd` package. It currently targets AVX, AVX2, and AVX512 on amd64, NEON on arm64, and WebAssembly SIMD. Unsupported platforms use emulation, so the same source still runs.<sup>[1]</sup>

The package keeps fixed vector sizes out of its types. A value such as `simd.Float32s` reports its length at runtime. The portable package supports operations shared across targets and fills some missing operations with other instructions.<sup>[1]</sup>

The abstraction makes the source portable. Only a matched run can make the speed claim portable.

## The package is an experiment, not a magic import.

Developers must build with `GOEXPERIMENT=simd`. The first release also has deliberate gaps. The Go team calls out reduction as one example. Go 1.27 lacks a portable sum across every vector lane, while a later release is expected to add `ReduceSum`.<sup>[1]</sup>

The release notes place `simd` beside the lower-level `simd/archsimd` package. The portable package covers the shared operation set. The architecture package exposes target-specific power when the shared set cannot express the job.<sup>[2]</sup>

## Measure the work users run.

Go's profile-guided optimization guide gives the right warning for this experiment. A useful profile must represent production behavior. The guide says microbenchmarks are usually poor PGO inputs because they exercise little of the whole program.<sup>[3]</sup>

A microbenchmark still helps compare one scalar loop with one SIMD loop. It cannot predict request latency, memory pressure, or throughput for the whole service. Keep both views. Measure the kernel, then run the real workload on every target you plan to support.

## Use four lanes before changing the default.

1. **Representative load:** Save the input shape, size distribution, hot path, and production profile that justify the work.
2. **Scalar baseline:** Keep the readable implementation and compare output before comparing time.
3. **Target matrix:** Run on the actual amd64, arm64, and WebAssembly targets you ship. Record CPU features and toolchain revision.
4. **Fallback drill:** Force the non-vector or emulated route. Check correctness, acceptable performance, and a clean rollback.

The dyno below writes an experiment template. It does not run Go, detect CPU instructions, compare outputs, or measure speed.
