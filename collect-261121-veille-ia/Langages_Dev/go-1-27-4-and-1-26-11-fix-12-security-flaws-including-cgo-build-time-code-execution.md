---
id: collect-261121-veille-ia/langages-dev/go-1-27-4-and-1-26-11-fix-12-security-flaws-including-cgo-build-time-code-execution
title: "Go 1.27.4 and 1.26.11 Fix 12 Security Flaws Including Cgo Build-Time Code Execution"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["security", "code", "exploited", "linker", "http", "build", "could", "flags"]
source: https://securityonline.info/go-security-fixes-1-27-4/
source_anchor: ""
source_lines: [1, 67]
section: "Langages_Dev"
---
## TL;DR

The Go team released Go 1.27.4 and 1.26.11 on November 10, 2026, with 12 security fixes. Eleven of these Go security fixes target the toolchain. They stop malicious packages that use cgo from running code while a developer builds them. The twelfth fixes a crash in HTTP/1 servers.

A malicious Go package could abuse cgo directives, linker flags or crafted object files to run code at build time. Go 1.27.4 and 1.26.11 close these gaps and fix a net/http race. Upgrade your Go toolchain and rebuild.

- **Total:** 12 CVEs
- **Severity:** 1 Critical · 10 High · 1 Medium
- **Actively exploited:** None confirmed
- **Highest severity:** 9.8 (Critical · CVSSv3) — CVE-2026-97040
- **Action:** Apply the latest security updates now

Notable CVEs

| CVE | CVSS (CVSSv3) | Type | Fixed in | Status | 
|---|---|---|---|---|
| CVE-2026-97040 | 9.8 | CWE-94 | 1.26.11, 1.27.4 | Not exploited | 
| CVE-2026-97033 | 8.8 | CWE-77 | 1.26.11, 1.27.4 | Not exploited | 
| CVE-2026-97034 | 8.8 | CWE-77 | 1.26.11, 1.27.4 | Not exploited | 
| CVE-2026-97035 | 8.8 | CWE-88 | 1.26.11, 1.27.4 | Not exploited | 
| CVE-2026-97036 | 8.8 | CWE-88 | 1.26.11, 1.27.4 | Not exploited | 
| CVE-2026-97037 | 8.8 | CWE-94 | 1.26.11, 1.27.4 | Not exploited | 
| CVE-2026-97038 | 8.8 | CWE-94 | 1.26.11, 1.27.4 | Not exploited | 
| CVE-2026-97041 | 8.8 | CWE-88 | 1.26.11, 1.27.4 | Not exploited | 

## Why It Matters

Developers expect “go build” to compile code, not run it. However, these bugs let a hostile dependency turn a build into code execution on a laptop or CI runner. That makes them a supply chain risk. A single poisoned module could reach build secrets and signing keys.

The Go Security Team found most of the issues during an internal audit. Outside researchers Jakub Ciolek, Ali Sherif and RyotaK of GMO Flatt Security reported the rest. No exploitation in the wild has been reported, and the CVE records carry no CVSS scores yet.

## How the Attack Works

### Unsafe cgo compiler and linker flags

Go lets packages pass flags to C compilers through “#cgo” directives, filtered by an allowlist. Several flags slipped through. For example, “–sysroot” and “-target” could point the compiler at attacker-controlled tools (CVE-2026-97033). Others let GCC load untrusted spec files (CVE-2026-97034) or write files to arbitrary folders (CVE-2026-97035).

### Linker script and plugin tricks

Meanwhile, split “-Wl,” flags could feed a regular file to the linker as a script (CVE-2026-97037). On macOS, the Apple linker rejoins “-l” and “to_library” into a flag that “loads an arbitrary LTO plugin dynamic library at link time” (CVE-2026-97036).

### Crafted object files and directives

Malicious “.syso” files and linker inputs could also carry hidden linker options (CVE-2026-97038, CVE-2026-97040). In addition, the team fixed injection through pkg-config directives, “//go:cgo_*” pragmas, Windows DLL names and escaped quotes (CVE-2026-97039, CVE-2026-97041, CVE-2026-97042 and CVE-2026-97043).

### HTTP/1 server crash

Finally, CVE-2026-97054 affects net/http. When MaxBytesReader hits its limit, it sets a “Connection: close” header without locking. As a result, a server “can crash due to runtime panic” if its handler writes headers while reading the body.

## Affected Versions

- Go versions before 1.26.11
- Go 1.27.0 through 1.27.3

### Who should act first

Teams that build third-party code in CI face the highest risk. Projects that rely on cgo, or on dependencies that ship native code, should also move quickly. Meanwhile, any HTTP/1 service that uses MaxBytesReader or MaxBytesHandler needs the net/http fix. The Go security fixes apply to Linux, macOS and Windows builds, since several bugs target each platform’s linker.

To check your setup, run “go version” on every build host. Also review the “toolchain” line in each go.mod file, because it can pin an older release.

## Patch and Mitigation

Install Go 1.27.4 or 1.26.11 from the official Go downloads page, then rebuild your binaries. Update CI images and build containers too. Until then, review new dependencies that use cgo or ship “.syso” files, and consider building with CGO_ENABLED=0 where possible. Applying these Go security fixes quickly protects both developer machines and pipelines.

#### Support Our Threat Intelligence

Find our vulnerability reports and weekly recaps helpful? Support our work today and unlock a **100% ad-free** reading experience!
