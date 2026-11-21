---
id: collect-261121-veille-ia/linux-kernel/suse-su-2026-5226-1-go1-27-distro-suse-linux-enterprise-high-performance-computing-15-sp4-espos-suse
title: "SUSE-SU-2026:5226-1: go1.27&distro=SUSE Linux Enterprise High Performance Computing 15 SP4-ESPOS SUSE Linux Enterprise | CveMate"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "suse", "enterprise", "distro", "high", "performance", "computing", "execution"]
source: https://cvemate.com/vulnerability/SUSE-SU-2026%3A5226-1
source_anchor: ""
source_lines: [1, 58]
section: "Linux_Kernel"
---
## Vulnerability summary

SUSE-SU-2026:5226-1 is a vulnerability in go1.27&distro=SUSE Linux Enterprise High Performance Computing 15 SP4-ESPOS SUSE Linux Enterprise with no CVSS score yet.

No CVSS vector published; no exploitation reported by the 5 checked sources

Source freshness unavailable

How is this decided?
## Advisory lineage

Upstream 12 · Downstream 0 · Related 12
## Timeline

1. 12-11-2026PublishedVulnerability first disclosed.
2. 13-11-2026Last modifiedVulnerability information updated.

## Description

Security update for go1.27 This update for go1.27 fixes the following issue: Security issues fixed: - CVE-2026-97033: `cmd/go`: unexpected code execution via `--sysroot`, `-target`, `-isysroot`, `-Wl,-syslibroot`, and `-x cgo` flags (bsc#1289978). - CVE-2026-97034: `cmd/go`: arbitrary command execution via GCC spec files in `cgo` linker search directories (bsc#1289979). - CVE-2026-97035: `cmd/go`: unexpected code execution and arbitrary file write via Clang `-gen-*` and GCC `-gnat*` flags in `cgo` (bsc#1289980). - CVE-2026-97036: `cmd/go`: arbitrary code execution during build on darwin via `-l to_library` in `cgo` (bsc#1289981). - CVE-2026-97037: `cmd/go`: linker script injection via split `-Wl,` flags and Darwin or symbol linker flags in `cgo` (bsc#1289982). - CVE-2026-97038: `cmd/go`: arbitrary code execution via crafted `.syso` files and `cgo` linker input files (bsc#1289983). - CVE-2026-97039: `cmd/go`: flag sanitization bypass via `#cgo pkg-config:` directives (bsc#1289984). - CVE-2026-97040: `cmd/go`, `cmd/link`: `cgo` directive injection via `.syso` files disguised as Go object files (bsc#1289985). - CVE-2026-97041: `cmd/compile`, `cmd/link`: host linker flag injection via `//go:cgo_*` directives (bsc#1289986). - CVE-2026-97042: `cmd/cgo`, `cmd/compile`, `cmd/link`: linker flag injection via `cgo_import_dynamic` and PE DLL names (bsc#1289987). - CVE-2026-97043: `cmd/go`, `cmd/compile`: unexpected code execution via escaped quotes in `//go:cgo_ldflag` (bsc#1289988). - CVE-2026-97054: `net/http`: HTTP/1 server crash due to `MaxBytesReader` header race (bsc#1289989). Changes for go1.27: Update to version go1.27.4 (bsc#1272545): - go#81866 `cmd/compile`: `branchelim` optimization can eliminate result of store - go#81927 `cmd/asm`: amd64 assembler loops forever on a forward jump to a labelled `PCALIGN` when a branch needs widening - go#82023 `net`: `Dialer.Dial{TCP,UDP,IP}` incorrectly treats invalid addresses as `::` - go#82106 `cmd/compile`: `staticcheck` still crashes with generic methods on Go 1.27.2 - go#82150 `runtime`: linux/mips `time64` nanosecond layout causes busy waits and empty CPU profiles - go#82334 `cmd/dist`: `GOFIPS140` bootstrap from Go 1.26 fails - go#82536 `crypto/tls`: omit ML-DSA from `signature_algorithms_cert` in Go 1.27CherryPickApproved - go#82627 `crypto/fips140`: minor compliance improvements - go#82657 `runtime`: nil-pointer panic in a promoted-method wrapper becomes 'traceback did not unwind completely' under `-race`

## CVSS metrics

Risk metrics not yet published.

## EPSS trend

No EPSS score has been published for this record yet.

Not in the CISA KEV catalog.

Not in the ENISA CNW catalog.

Not in the CIRCL catalog.

## Public exploits

0 found
No public exploits indexed.

## Weaknesses

CWE · root cause
No weakness data has been published for this record.

## Affected systems

Known conditions: not stated in public data.

## Suse11 products

- go1.27&distro=SUSE Linux Enterprise High Performance Computing 15 SP4-ESPOS< 1.27.4-150000.1.18.1
- go1.27&distro=SUSE Linux Enterprise High Performance Computing 15 SP4-LTSS< 1.27.4-150000.1.18.1
- go1.27&distro=SUSE Linux Enterprise High Performance Computing 15 SP5-ESPOS< 1.27.4-150000.1.18.1
- go1.27&distro=SUSE Linux Enterprise High Performance Computing 15 SP5-LTSS< 1.27.4-150000.1.18.1
- go1.27&distro=SUSE Linux Enterprise Module for Development Tools 15 SP7< 1.27.4-150000.1.18.1

1 vendor · 11 products
