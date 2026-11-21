---
id: collect-261121-veille-ia/hardware-semi/inikep-lzbench-discovergit-open-source-intelligence
title: "inikep/lzbench — DiscoverGit — Open source intelligence"
domain: hardware-semi
role: reference
task: article
actors: []
dates: ["2026-11-21"]
keywords: ["lzbench", "compressors", "compression", "linux", "open", "source", "zstd", "column"]
source: https://discovergit.app/repo/inikep/lzbench
source_anchor: ""
source_lines: [1, 286]
section: "Hardware_Semi"
---
lzbench is an in-memory benchmark of open-source compressors

Summary

lzbench is an in-memory benchmarking tool for open-source compressors, allowing users to compare the performance of various compression algorithms. It is designed for developers and researchers who need to evaluate and select the most efficient compression methods for their specific use cases.

At a glance

Difficulty

Easy

Maintenance

Very active

Documentation

Good

Production

Near production

Production readiness

Similar projects

similarity ≥ 0.67 · 5 found

Side-by-side comparison available for pairs with similarity ≥ 85% ("Compare" badge above)

Repository health

live · 21 Nov 2026

Stars

1.1k

Forks

225

Contributors

46

Latest release

v2.4

Indexed 30 Sep 2026 at 1.1k ⭐ → 1.1k today · +0.9% in 53 days

Overall trend: Slow growth ↑ +0.9% since first indexed

Daily star gains — 90 days

Technical details

Language

C

Forks

225

Open issues & PRs

15

Avg issue close

74.1d

PR merge rate

100%

Created

135mo ago

Repo size

31.8 MB

Quality score

7/10

Languages

Latest releases

every 130d (median)

Signal scores

Community profile

Contributors

46

top 83%

Watchers

34

Top contributors

## 
    README

    
  

  ## Introduction

lzbench is an in-memory benchmarking tool for open-source compressors. It integrates all compressors into a single executable. Initially, an input file is loaded into memory, after which each compressor is used to compress and decompress the file, ensuring the decompressed output matches the original. This method provides the advantage of compiling all compressors with the same compiler and optimizations. However, it requires access to the source code of each compressor, meaning e.g. Slug and lzturbo are not included.

| Status | 
|---|

The list of changes in lzbench is available in the CHANGELOG.

Contributor information can be found in CONTRIBUTING.md.

## Usage

```
usage: lzbench [options] input [input2] [input3]
For example:
  lzbench -ezstd filename = selects all levels of zstd
  lzbench -ebrotli,2,5/zstd filename = selects levels 2 & 5 of brotli and zstd
  lzbench -t3,5 fname = 3 sec compression and 5 sec decompression loops
  lzbench -t0,0 -i3,5 fname = 3 compression and 5 decompression iterations
  lzbench -o1c4 fname = output markdown format and sort by 4th column
  lzbench -ezlib -j -r dirname/ = test zlib on all files in directory, recursively
```
For complete list of options refer to manual in doc directory which contains more detailed documentation.

## Building

To compile, you need a C and C++ compiler that is GNUC-compatible, such as GCC, LLVM/Clang, or ICC. It is recommended to use GCC 7.1+ or Clang 6.0+.

For Linux/MacOS/MinGW (Windows):

`make -j$(nproc)`
The default linking for Linux is dynamic and static for Windows. This can be changed with `make BUILD_STATIC=0/1`.

For complete building instruction, with troubleshooting refer to BUILD.md.

## Supported compressors

The table below lists the supported compressors. The Last update column is the release date of the bundled version. Compressors are built on every CI platform (Linux x86-64/x86-32/ARM64/ARM32/PPC64LE, macOS arm64, Windows MinGW) unless the Notes column says otherwise. Where lzbench carries local build fixes to a codec's sources, the Notes column says so.

| Compressor | Last update | Notes | 
|---|---|---|
| aceapex 2.6.1 | 2026-11-03 |  | 
| aocl-compression 6.0.0 | 2026-11-11 | Disabled by default; enable with `make DONT_BUILD_AOCL=0` . Linux x86-64 only — AMD-optimized lz4/lz4hc/lzma/bzip2/snappy/zlib/zstd (compression prefixed`aocl-` ); requires GNU binutils (`nm` ,`objcopy` ); license: BSD 3-Clause / MIT / NCSA | 
| brieflz 1.3.0 | 2020-02-15 |  | 
| brotli 1.2.0 | 2025-10-27 |  | 
| bsc 3.4.0 | 2026-10-21 |  | 
| bzip2 1.0.8 | 2019-07-13 |  | 
| bzip3 1.5.4 | 2026-09-07 |  | 
| crush 1.0 | 2013-07-01 |  | 
| density 0.16.6 | 2025-08-27 | Linux x86-64 and macOS only — requires the Rust toolchain (skipped for 32-bit, cross-compiled and Windows builds) | 
| fastlz 0.5.0 | 2020-02-02 |  | 
| fast-lzma2 1.0.1 | 2019-05-06 |  | 
| glza 0.12 | 2026-03-23 |  | 
| go-brrr 1.3.0 | 2026-10-11 | Brotli in Go. Requires Go 1.22+. Skips 32-bit, cross and Windows builds. Uses `lz+entropy/gobrrr/ffi/ffi.go` for the C archive. Vendor copy omits tests, tools and`golang.org/x/tools` . | 
| gpucompact 1.1 | 2026-08-14 | CUDA only | 
| kanzi 2.6.0 | 2026-09-26 |  | 
| lbzip2 2.6.5 | 2026-08-18 | bzip2 format; benchmarked single-threaded | 
| libdeflate v1.26 | 2026-08-22 |  | 
| lizard v2.1 | 2025-01-26 |  | 
| lz4/lz4hc/lz4frame v1.11.0 | 2026-11-04 | lz4frame: frame format, 4 MB independent blocks, multithreaded compression with `-I#` | 
| lzav 7.2 | 2026-11-11 |  | 
| lzf 3.6 | 2014-03-13 |  | 
| lzfse/lzvn 1.0 | 2017-05-22 |  | 
| lzg 1.0.10 | 2023-08-31 |  | 
| lzham 1.0 | 2022-01-03 | Public domain since 2020-09-15. Build fixed in lzbench for MinGW and newer compilers ( `<cstdint>` includes,`GetSystemInfo` ,`DISABLE_THREADING` ). Disabled on macOS and 32-bit x86 — 64 MB dictionary overflows the 32-bit address space | 
| lzjb 2010 | 2010 |  | 
| lzlib 1.16 | 2026-03-11 |  | 
| lzma v26.03 | 2026-09-03 |  | 
| lzo 2.10 | 2017-03-01 |  | 
| lzsse 2019-04-18 (1847c3e827) | 2019-04-18 | 64-bit x86 only — requires SSE4.1 (Windows: MinGW-w64 only); lzsse8fast has a bug | 
| mbrotli 0.5.22 | 2026-11-13 | Requires the Rust toolchain 1.89+ (skipped for 32-bit, cross-compiled and Windows builds). Built through the `mbrotli-ffi` C ABI; manifests trimmed in lzbench (no dev-dependencies, rlib only) | 
| memlz 0.5 beta | 2026-09-24 | Patched in lzbench: SSE4.2 target attribute also applied for MinGW ( `memlz.h` ) | 
| misa77 0.7.0 | 2026-11-17 | Little-endian 64-bit only — needs a C++20 compiler (GCC 10+, Clang 12+); skipped automatically | 
| mzstd 0.2.14 | 2026-11-13 | Requires the Rust toolchain 1.89+ (skipped for 32-bit, cross-compiled and Windows builds). Built through the `mzstd-ffi` C ABI; manifests trimmed in lzbench (no dev-dependencies, rlib only) | 
| nvcomp 2.2.0 | 2022-02-07 | CUDA only — built with `make ENABLE_CUDA=1` ; not in the default CI matrix | 
| openzl 0.3.0 | 2026-09-29 | 64-bit only — upstream does not support 32-bit builds | 
| ppmd8 26.03 | 2026-09-03 |  | 
| pulsar 2.5.0 | 2026-09-02 | GPL-3; Rust 1.85+, skipped for 32-bit, cross-compiled and Windows builds. Patched in lzbench: `src/lib.rs` has a framed C ABI whose encoder does not re-decode candidates to verify them;`src/bin` not vendored | 
| quicklz 1.5.1 beta 7 | 2011-10-07 |  | 
| skim 0.1.0 | 2026-06-07 | Linux x86-64 and macOS only — requires the Zig compiler | 
| slz 1.4-dev5 | 2026-11-09 | Emits a gzip/deflate-compatible stream | 
| snappy 1.3.1 | 2026-09-18 |  | 
| tamp 2.3.0 | 2026-07-09 |  | 
| tornado 0.6a | 2014-03-08 | Disabled on RISC-V unless the build machine does misaligned access at full speed (asked via the hwprobe syscall); such a build is then not portable to a slower RISC-V machine | 
| ucl 1.03 | 2004-07-20 |  | 
| wlz4/wlz4hc 1.3.0, wzip 1.3.0 | 2026-10-26 |  | 
| xz 5.8.4 | 2026-09-09 | Built in lzbench with a hand-written `config.h` (upstream uses autotools) | 
| yalz77 2022-07-06 | 2022-07-06 |  | 
| zlib 1.4 | 2026-11-04 |  | 
| zlib-ng 2.3.3 | 2026-02-03 |  | 
| zlib-turbo 1.2.2 | 2026-11-10 | Built with `Z_PREFIX` ; levels 1-9 give zlib 1.4's output, also with`-I#` | 
| zling 2018-10-12 | 2018-10-12 | Build fixed in lzbench (missing `<functional>` include). Disabled on big-endian PowerPC; not recommended for production use (per author) | 
| zpaq 7.15 | 2016-08-17 | Slower on non-x86 — built with `-DNOJIT` (x86-only JIT, portable interpreter elsewhere) | 
| zstd 1.5.7 | 2025-02-19 |  | 
| zxc 1.0.0 | 2026-11-11 |  | 

**Warning**: The compressors listed below have security issues and/or are no longer maintained.

| Compressor | Last update | Notes | 
|---|---|---|
| csc 2016-10-13 | 2016-10-13 | Build fixed in lzbench (SSE intrinsics header, 7-Zip type guards). Disabled on macOS — segfaults with Apple LLVM 7.3.0 (clang-703.0.31) | 
| gipfeli 2016-08-16 | 2016-08-16 | Decompression file mismatch when compiled with GCC 14.2 using -O3 | 
| lzmat 1.01 v1.0 | 2008-07-08 | Build fixed in lzbench for 64-bit (pointer arithmetic truncated through `MP_U32` ). Decompression bugs; may segfault with GCC 4.9+ using -O3 | 
| lzrw 15-Jul-1991 | 1991-07-15 | May segfault with GCC 4.9+ using -O3 | 
| wflz 2015-09-16 | 2015-09-16 | May segfault with GCC 4.9+ using -O3 | 
| yappy 2014-03-22 | 2014-03-22 | Disabled on big-endian PowerPC; segfault with GCC 13.3.0 on 32-bit ARM (arm-linux-gnueabi) | 

## Benchmarks

The following results were obtained using `lzbench 2.4` (the `lzbench24_x86_64-linux-gnu-gcc15` release binary, built with `gcc 15.2.0`)
and executed with the options `-eALL -t8,8 -o1c4`.
The tests were run on a single thread of an AMD EPYC 9555P processor at 3.20 GHz, with the CPU governor set to `performance` and turbo
boost disabled for stability. The operating system was `Ubuntu 26.04`, and the benchmark made use of
`silesia.tar`, which contains tarred files from the
Silesia compression corpus.

The results sorted by ratio are available here. An interactive version of the single- and multi-threaded results can be sorted and searched, and charts compression ratio against speed with the Pareto frontier.

| Compressor name | Compression | Decompress. | Compr. size | Ratio | 
|---|---|---|---|---|
| memcpy | 24075 MB/s | 24002 MB/s | 211947520 | 100.00 | 
| aceapex 1.0.1 -1 | 55.9 MB/s | 675 MB/s | 68556760 | 32.35 | 
| aceapex 1.0.1 -2 | 39.2 MB/s | 679 MB/s | 68213303 | 32.18 | 
| brieflz 1.3.0 -1 | 192 MB/s | 293 MB/s | 81138803 | 38.28 | 
| brieflz 1.3.0 -3 | 124 MB/s | 301 MB/s | 75550736 | 35.65 | 
| brieflz 1.3.0 -6 | 22.8 MB/s | 328 MB/s | 67208420 | 31.71 | 
| brieflz 1.3.0 -8 | 3.02 MB/s | 355 MB/s | 64531718 | 30.45 | 
| brotli 1.2.0 -0 | 341 MB/s | 322 MB/s | 78433298 | 37.01 | 
| brotli 1.2.0 -2 | 139 MB/s | 378 MB/s | 68069489 | 32.12 | 
| brotli 1.2.0 -5 | 53.3 MB/s | 419 MB/s | 59555449 | 28.10 | 
| brotli 1.2.0 -8 | 12.6 MB/s | 443 MB/s | 57148304 | 26.96 | 
| brotli 1.2.0 -11 | 0.56 MB/s | 383 MB/s | 50407795 | 23.78 | 
| bsc 3.3.12 -m0 -e1 | 19.4 MB/s | 25.0 MB/s | 49295208 | 23.26 | 
| bsc 3.3.12 -m4 -e1 | 28.8 MB/s | 15.8 MB/s | 50689508 | 23.92 | 
| bsc 3.3.12 -m5 -e1 | 26.9 MB/s | 14.7 MB/s | 49609096 | 23.41 | 
| bzip2 1.0.8 -1 | 13.5 MB/s | 41.1 MB/s | 60484813 | 28.54 | 
| bzip2 1.0.8 -5 | 13.2 MB/s | 37.3 MB/s | 55724395 | 26.29 | 
| bzip2 1.0.8 -9 | 12.7 MB/s | 35.5 MB/s | 54572811 | 25.75 | 
| bzip3 1.5.4 -1 | 11.9 MB/s | 13.9 MB/s | 50325695 | 23.74 | 
| bzip3 1.5.4 -5 | 11.9 MB/s | 12.4 MB/s | 47236836 | 22.29 | 
| bzip3 1.5.4 -9 | 11.3 MB/s | 10.4 MB/s | 48753972 | 23.00 | 
| crush 1.0 -0 | 61.0 MB/s | 316 MB/s | 73064603 | 34.47 | 
| crush 1.0 -2 | 1.11 MB/s | 372 MB/s | 63746223 | 30.08 | 
| density 0.16.6 -1 | 1630 MB/s | 1617 MB/s | 133042150 | 62.77 | 
| density 0.16.6 -2 | 767 MB/s | 758 MB/s | 101613928 | 47.94 | 
| density 0.16.6 -3 | 442 MB/s | 578 MB/s | 92314010 | 43.56 | 
| fastlz 0.5.0 -1 | 273 MB/s | 587 MB/s | 104628084 | 49.37 | 
| fastlz 0.5.0 -2 | 294 MB/s | 561 MB/s | 100906072 | 47.61 | 
| fastlzma2 1.0.1 -1 | 21.8 MB/s | 65.0 MB/s | 59030950 | 27.85 | 
| fastlzma2 1.0.1 -3 | 12.1 MB/s | 69.9 MB/s | 54023833 | 25.49 | 
| fastlzma2 1.0.1 -5 | 8.08 MB/s | 75.7 MB/s | 51209567 | 24.16 | 
| fastlzma2 1.0.1 -8 | 4.52 MB/s | 78.5 MB/s | 49126736 | 23.18 | 
| fastlzma2 1.0.1 -10 | 3.38 MB/s | 79.1 MB/s | 48666061 | 22.96 | 
| kanzi 2.5.3 -1 | 196 MB/s | 1112 MB/s | 79343089 | 37.44 | 
| kanzi 2.5.3 -2 | 202 MB/s | 666 MB/s | 68630279 | 32.38 | 
| kanzi 2.5.3 -3 | 99.0 MB/s | 338 MB/s | 64427189 | 30.40 | 
| kanzi 2.5.3 -4 | 45.0 MB/s | 168 MB/s | 60365746 | 28.48 | 
| kanzi 2.5.3 -5 | 19.6 MB/s | 53.2 MB/s | 54016788 | 25.49 | 
| kanzi 2.5.3 -6 | 15.0 MB/s | 31.2 MB/s | 49517551 | 23.36 | 
| kanzi 2.5.3 -7 | 10.8 MB/s | 15.8 MB/s | 47308160 | 22.32 | 
| kanzi 2.5.3 -8 | 3.45 MB/s | 3.39 MB/s | 43250377 | 20.41 | 
| kanzi 2.5.3 -9 | 2.48 MB/s | 2.44 MB/s | 41809254 | 19.73 | 
| lbzip2 2.6.5 -1 | 22.0 MB/s | 62.7 MB/s | 60373170 | 28.48 | 
| lbzip2 2.6.5 -5 | 22.8 MB/s | 51.0 MB/s | 55682543 | 26.27 | 
| lbzip2 2.6.5 -9 |  |  |  |  |
