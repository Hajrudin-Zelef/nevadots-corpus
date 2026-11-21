---
id: collect-261121-veille-ia/langages-dev/go-1-27-4-security-release-cgo-flags-and-a-net-http-fix
title: "Go 1.27.4 security release: cgo flags and a net/http fix"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-11"]
keywords: ["flags", "linker", "http", "sysroot", "security", "clang", "files", "library"]
source: https://importstatic.com/go/go-1-27-4-cgo-security-flags
source_anchor: ""
source_lines: [1, 100]
section: "Langages_Dev"
---
Tested with Go 1.27.3, 1.27.4, 1.26.10 and 1.26.11 on darwin/arm64; Apple clang 21.0.0; govulncheck v1.9.0

A dependency that uses cgo could make the C compiler or linker run its code on the machine doing the build, by putting flags in its `#cgo` directives, object files or linker inputs. Eleven of the twelve vulnerabilities fixed in Go 1.27.4 and 1.26.11, released on November 10, are bugs of this kind in the way the go command, compiler and linker handle cgo. The fixes refuse more flags, which also breaks some builds that were never malicious. `--sysroot=` and `-target` in `#cgo CFLAGS`, both common in cross-compiling setups, now stop the build with "invalid flag".

The twelfth fix is for a crash in `net/http` HTTP/1 servers that use `MaxBytesReader`. That bug is in your deployed binaries, so they need a rebuild and a redeploy. The cgo fixes protect the build, so they need the new toolchain on every machine that builds code you didn't write. As of November 11, 2026, go.dev/dl lists 1.27.4 and 1.26.11 as the current stable releases.

## The twelve CVEs

The golang-announce post lists the twelve CVEs. They were pre-announced on November 5 as PRIVATE-track issues, which the Go security policy reserves for "violations of committed security properties" and fixes in the next scheduled minor release.

| CVE | Area | What changes | 
|---|---|---|
| CVE-2026-97054 | `net/http` | HTTP/1 servers using `MaxBytesReader` or`MaxBytesHandler` could panic when the limit was hit while the handler set response headers | 
| CVE-2026-97033 | `#cgo` compiler and linker flags | `--sysroot` and`-target` disallowed;`-isysroot` and`-Wl,-syslibroot` must not point at a regular file;`-x` limited to the languages cgo compiles | 
| CVE-2026-97034 | cgo linker search directories | `-L` ,`-isysroot` and`-Wl,-syslibroot` directories that contain GCC spec files are rejected | 
| CVE-2026-97035 | `#cgo` flags | Clang `-gen-*` and GCC`-gnat*` flags disallowed (the`-g*` wildcard used to match them) | 
| CVE-2026-97036 | `#cgo LDFLAGS` on macOS | `-l to_library` , which Apple's linker joins into`-lto_library` and uses to load a plugin, is rejected, along with`-lazy_library` ,`-lazy_framework` ,`-load_hidden` ,`-library` and`-library-path` | 
| CVE-2026-97037 | split `-Wl,` flags | the second token must also carry the `-Wl,` prefix | 
| CVE-2026-97038 | `.syso` files, linker inputs | inputs must be valid binaries or archives | 
| CVE-2026-97039 | `#cgo pkg-config:` | `--with-path=` ,`--personality=` , some`--define-variable=` values and local`.pc` paths disallowed | 
| CVE-2026-97040 | `.syso` files posing as Go objects | the linker rejects host object formats it doesn't recognise | 
| CVE-2026-97041 | `//go:cgo_*` directives | commas are rejected | 
| CVE-2026-97042 | `cgo_import_dynamic` , Windows DLL names | library names with path separators, `..` ,`:` , or a leading`-` or`@` are rejected | 
| CVE-2026-97043 | `//go:cgo_ldflag` | escaped quotes no longer slip disallowed flags past the check | 

The Go Security Team found eight of the cgo issues in an internal audit; the announcement credits outside reporters for the other three and for the `net/http` crash.

## Which #cgo lines stop building

The tracking issue for CVE-2026-97033, golang/go#82577, explains why `--sysroot` had to go: it makes GCC and Clang look for "compiler subprograms, spec files, and CRT startup objects" in a directory the package chooses. Clang's `-target` falls back to running helper tools from the current directory when they aren't on `PATH`.

To see which legitimate lines that catches, we generated seven small cgo packages and built each one with 1.27.3, 1.27.4, 1.26.10 and 1.26.11 on macOS with Apple clang 21.0.0. A Go driver ran `go build -a` with `GOTOOLCHAIN` pinned to each version. This is the `--sysroot` package; the others differ only in the `#cgo` line:

```
package sysroot
/*
#cgo CFLAGS: --sysroot=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk
#include <stdio.h>
static int answer(void) { return 42; }
*/
import "C"
// Answer calls into C.
func Answer() int { return int(C.answer()) }
```
1.27.3 and 1.26.10 gave the same results, as did 1.27.4 and 1.26.11:

| Flag | 1.27.3 and 1.26.10 | 1.27.4 and 1.26.11 | 
|---|---|---|
| `#cgo CFLAGS: --sysroot=<SDK>` | builds | invalid flag | 
| `#cgo CFLAGS: -target arm64-apple-macos13` | builds | invalid flag | 
| `#cgo CFLAGS: --target=arm64-apple-macos13` | invalid flag | invalid flag | 
| `#cgo CFLAGS: -isysroot <SDK directory>` | builds | builds | 
| `#cgo CFLAGS: -x c` | builds | builds | 
| `#cgo LDFLAGS: --sysroot=<SDK>` | invalid flag | invalid flag | 
| no directive, `CGO_CFLAGS=--sysroot=<SDK>` in the environment | builds | builds | 

On 1.27.4 the build stops with this error (1.26.11 prints the same):

`example.com/cgoflags/sysroot: invalid flag in #cgo CFLAGS: --sysroot=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk (see https://go.dev/s/invalidflag)`
For the two-argument form the message names only the flag: `invalid flag in #cgo CFLAGS: -target`. Pointing `-isysroot` at a directory, which is how macOS SDK paths are normally passed, still works; the new check only refuses a regular file. We didn't test the pkg-config, `.syso` or Windows changes, which need inputs this machine doesn't have.

## Fixing a build that now fails

The cgo documentation says "Flags obtained from environment variables are not subject to the security limitations", so the safer fix is to move the flag out of the source and into `CGO_CFLAGS`. The last row of the table shows that this builds on all four versions. The setting then lives in your build scripts, where a sysroot that depends on the build machine belongs anyway.

The other option is to widen the allowlist. In the 1.27.4 source, `--sysroot` and `-target` were removed from the list of valid flags, not added to a list of forbidden ones, so `CGO_CFLAGS_ALLOW` can still accept them. With `CGO_CFLAGS_ALLOW='--sysroot=.*|--target=.*'` and `CGO_LDFLAGS_ALLOW='--sysroot=.*'`, the `--sysroot=` packages and the single-argument `--target=` package built again on 1.27.4 and 1.26.11. The pattern has to match the whole argument, hence the `.*`. These variables apply to every cgo package in the build, so they reopen the hole CVE-2026-97033 closed for all of your dependencies. Prefer `CGO_CFLAGS` unless a package you maintain must carry the flag in its source.

## The MaxBytesReader crash in net/http

According to golang/go#82563, when a `MaxBytesReader` limit is exceeded, the HTTP/1 server sets `Connection: close` on the response "without synchronization". A handler that reads the request body in one goroutine while setting headers in another can then hit a runtime panic that takes the process down. The issue describes the crash only for that pattern on HTTP/1 servers. We did not try to reproduce it.

Don't rely on govulncheck to find affected binaries yet. We built a small server that wraps the body in `http.MaxBytesReader` with go1.27.3, and govulncheck v1.9.0, run on it at about 00:12 UTC on November 11, reported "No vulnerabilities found" for "the go1.27.3 standard library". `go version` tells you which toolchain built a binary:

```
$ go version ./server-bin
./server-bin: go1.27.3
```
## Updating the toolchain

The `go` line in go.mod doesn't pull in a point release. Per the toolchain documentation, the go command uses its own bundled toolchain "when that toolchain is at least as new as the `go` or `toolchain` lines", so a module that says `go 1.27.0` keeps building with whatever Go is installed on the CI image. Either update the installed Go, or pin the patched release in the module:

```
$ go get [email protected]
go: added toolchain go1.27.4
$ go version
go version go1.27.4 darwin/arm64
```
That adds `toolchain go1.27.4` under the `go` line, and any machine with an older Go and `GOTOOLCHAIN=auto` (the default) switches to 1.27.4, downloading it if needed, the next time it builds the module. Update first the CI jobs that build pull requests or try dependency upgrades with cgo enabled, since those run code you didn't write. The Go 1.27.3 release three weeks ago used the same mechanism for its `crypto/x509` fix.

## Sources checked for this article (9)

1. [security] Go 1.27.4 and Go 1.26.11 are released, golang-announce (2026-11-10) – the 12 fixes, CVE-2026-97033 to 97043 and 97054, issue numbers and credits
2. Go release history: go1.27.4 and go1.26.11 (2026-11-10) – security fixes to cgo, the go command, the compiler, the linker and net/http, plus bug fixes
3. golang/go#82577: cmd/go: unexpected code execution via --sysroot, -target, -isysroot, -Wl,-syslibroot, and -x cgo flags (CVE-2026-97033) – --sysroot and -target disallowed, -isysroot and -Wl,-syslibroot checked, -x restricted to cgo languages
4. golang/go#82563: net/http: HTTP/1 server crash due to MaxBytesReader header race (CVE-2026-97054) – who is affected by the runtime panic
5. golang/go#82574: cmd/go: arbitrary code execution during build on darwin via "-l to_library" in cgo (CVE-2026-97036) – Apple linker recombining -l to_library, newly rejected linker flags
6. golang/go#82571: cmd/go: flag sanitization bypass via "#cgo pkg-config:" directives (CVE-2026-97039) – pkg-config options and local .pc paths now rejected
7. Command cgo documentation, Go 1.27.4 source (src/cmd/cgo/doc.go) – CGO_CFLAGS_ALLOW must match a full argument; flags from environment variables are not subject to the checks
8. Go Toolchains, Go documentation – GOTOOLCHAIN=auto uses the bundled toolchain when it is at least as new as the go or toolchain line; go get toolchain@
9. Go Security Policy – PRIVATE track: fixed in the next scheduled minor releases, pre-announced three to seven days before
