---
id: collect-261121-veille-ia/securite/go-ships-12-security-fixes-including-a-server-crash-and-build-time-code-execution
title: "Go ships 12 security fixes, including a server crash and build-time code execution"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["security", "server", "code", "fixes", "ships", "including", "crash", "build-time"]
source: https://www.freenode.org/article/go-ships-12-security-fixes-including-a-server-crash-and-build-time-code-executio
source_anchor: ""
source_lines: [1, 11]
section: "Securite"
---
# Go ships 12 security fixes, including a server crash and build-time code execution

Go 1.27.4 and 1.26.11 close an HTTP/1 panic in MaxBytesReader and stop malicious cgo packages from running code during the build.

The Go team has released 1.27.4 and 1.26.11, minor point releases that include 12 security fixes under the project's security policy.

The most visible runtime bug is in the standard HTTP server. On HTTP/1, exceeding the limit set by MaxBytesReader automatically adds a Connection: close header without synchronizing that write. If a handler reads the request body at the same time it writes response headers, the server can panic. Any HTTP/1 server that uses MaxBytesReader or MaxBytesHandler is affected. Jakub Ciolek reported the flaw, tracked as CVE-2026-97054.

A second issue is worse for anyone who builds untrusted packages. Several compiler and linker flags still allowed in cgo directives could run arbitrary code at build time. --sysroot can relocate compiler pieces and startup objects into an attacker-controlled directory. Clang's -target can fall back to helper tools in the current working directory. -isysroot and -Wl,-syslibroot can hand GNU ld a regular file that it treats as a linker script. -x could name compiler input languages cgo does not support. The Go Security Team found this during an internal audit. It is CVE-2026-97033. Those flags are now blocked, checked so they do not point at regular files, or limited to the languages cgo actually supports.

The same releases also cover a related cgo link-time path in which GCC spec files in linker search directories could run arbitrary commands while building a malicious package.
