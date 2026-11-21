---
id: collect-261121-veille-ia/go-json/go-patches-build-time-code-execution-and-http-crash
title: "Go patches build-time code execution and HTTP crash"
domain: go-json
role: reference
task: article
actors: []
dates: []
keywords: ["code", "execution", "http", "crash", "patches", "build-time", "releases", "security"]
source: https://www.freenode.org/article/go-patches-build-time-code-execution-and-http-crash
source_anchor: ""
source_lines: [1, 11]
section: "Go_JSON"
---
# Go patches build-time code execution and HTTP crash

Releases 1.27.4 and 1.26.11 include 12 security fixes, among them cgo flags that could run attacker code during a build.

The Go team has released 1.27.4 and 1.26.11, minor point releases that include 12 security fixes.

CVE-2026-97033 lets a malicious package that uses cgo run arbitrary code while it is being built, by passing compiler and linker flags in cgo directives that redirect tools, startup files, or linker input under the attacker's control. The Go Security Team found the issue in an internal audit. Those flags are now rejected or restricted so a package cannot steer the build that way.

CVE-2026-97054 can crash an HTTP/1 server that uses a body-size limit. When a read exceeds that limit, the server sets a connection-close header without coordinating with other header writes, so a handler that reads the body while writing the response can panic. Jakub Ciolek reported the race.

The same releases also stop arbitrary command execution at link time through GCC spec files placed in cgo linker search directories, among the remaining fixes.
