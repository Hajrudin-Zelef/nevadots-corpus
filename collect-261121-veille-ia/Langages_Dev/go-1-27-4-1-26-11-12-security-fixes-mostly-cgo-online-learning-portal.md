---
id: collect-261121-veille-ia/langages-dev/go-1-27-4-1-26-11-12-security-fixes-mostly-cgo-online-learning-portal
title: "Go 1.27.4 & 1.26.11: 12 Security Fixes, Mostly cgo - Online Learning Portal"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-10"]
keywords: ["http", "code", "build", "toolchain", "security", "fixes", "flags", "arbitrary"]
source: https://onlinelearningportal.website/go-1-27-4-security-release-cgo/
source_anchor: ""
source_lines: [1, 142]
section: "Langages_Dev"
---
On 10 November 2026 the Go team released **Go 1.27.4 and Go 1.26.11** with **12 security fixes**. One fixes a crash in the `net/http` HTTP/1 server. The other eleven close ways a **malicious package that uses cgo could run code on your machine at build or link time**, just because you ran `go build`. Most were found by the Go Security Team during an internal audit. If you build Go code from third-party modules, on laptops or CI runners, you should update. This guide explains the issues in plain terms, shows how to upgrade quickly and how to check what your projects use, and covers the cgo patterns that the patched toolchain now rejects.

## The release at a glance

From the official announcement (forwarded from golang-announce):

- **net/http (CVE-2026-97054):** an HTTP/1 server that uses`MaxBytesReader` or`MaxBytesHandler` can crash with a runtime panic if the handler reads the request body while writing the response header at the same time. When the body limit is exceeded, the`Connection: close` header was set without synchronisation and could race with the handler.
- **cmd/go and the toolchain (11 CVEs, CVE-2026-97033 to CVE-2026-97043):** different ways to sneak dangerous flags or files past the go command’s checks on`#cgo` directives,`.syso` objects,`pkg-config` and`//go:cgo_*` pragmas, leading to code execution or arbitrary file writes during the build.

Both supported release lines are fixed: **1.27.4** and **1.26.11**.

## Why “build-time code execution” matters

Go has a good safety story for builds: unlike many ecosystems, compiling a module isn’t supposed to run arbitrary code from it. The go command enforces this partly by allowing only a safe list of compiler and linker flags in `#cgo CFLAGS` and `#cgo LDFLAGS`. These vulnerabilities were gaps in that allow-list and in how linker inputs were checked. In practice, a booby-trapped dependency could:

- Point the C compiler at an attacker-controlled sysroot (`--sysroot` ,`-target` ,`-isysroot` ,`-Wl,-syslibroot` ).
- Make GCC load a malicious `.spec` file found in a linker search directory.
- Use Clang `-gen-*` or GCC`-gnat*` flags to write files into arbitrary directories.
- On macOS, load an arbitrary LTO plugin by splitting `-l to_library` into two arguments.
- Smuggle a linker script in through split `-Wl,` flags, a disguised`.syso` file or a library in a`-L` directory.
- Inject flags through `#cgo pkg-config:` options or local`.pc` files.
- Bypass checks with crafted quoting in `//go:cgo_ldflag` , commas in`//go:cgo_*` directives, or crafted PE DLL names on Windows.

Your CI runner usually holds secrets: deploy keys, cloud credentials, signing keys. Build-time code execution in CI is serious, even if the final binary is never run.

## Who is affected

- **Anyone building packages that use cgo** from sources they don’t fully control. That includes indirect dependencies.
- Builds with `CGO_ENABLED=1` (the default on most native builds when a C compiler is available).
- **HTTP/1 servers** using`http.MaxBytesReader` or`http.MaxBytesHandler` , for the crash fix.

If you build with `CGO_ENABLED=0`, the cgo issues don’t apply to those builds, but the `net/http` fix still does. Upgrade anyway.

## How to upgrade

### Check your version

```
go version
# go version go1.27.3 linux/amd64  <- needs updating
```
### Update the toolchain

Download installers from go.dev/dl, use your package manager, or let the go command fetch the patched toolchain for a module by raising the `toolchain` line:

```
go get [email protected]
```
That updates `go.mod`:

```
module example.com/myapp
go 1.27
toolchain go1.27.4
```
With the default `GOTOOLCHAIN=auto`, anyone building the module with an older local Go downloads and uses 1.27.4 automatically.

Teams still on the 1.26 line should use `go1.26.11`.

### Update Docker images and CI

Pin patch versions explicitly so you know you got the fix:

```
FROM golang:1.27.4 AS build
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -trimpath -o /out/app ./cmd/app
```
For GitHub Actions with `actions/setup-go`:

```
- uses: actions/setup-go@v6
  with:
    go-version: '1.27.4'
```
Avoid floating tags like `1.27` in security-sensitive pipelines unless you’re sure your cache pulls the newest patch.

## Finding cgo in your dependency tree

Want to know which dependencies use cgo? List packages that have cgo files:

```
go list -deps -f '{{if .CgoFiles}}{{.ImportPath}}{{end}}' ./...
```
And check for prebuilt `.syso` objects, which several of the fixes target:

```
go list -deps -f '{{if .SysoFiles}}{{.ImportPath}}: {{.SysoFiles}}{{end}}' ./...
```
Review anything unexpected. A small utility library that suddenly ships C code or binary objects is worth a closer look.

### Run govulncheck

govulncheck reports known vulnerabilities that affect your code, including standard library issues tied to your Go version:

```
go install golang.org/x/vuln/cmd/govulncheck@latest
govulncheck ./...
```
## What the patched toolchain now rejects

Once you upgrade, some builds that used to work may fail with “invalid flag” style errors. That’s intentional. According to the announcement, the go command now:

- Disallows `--sysroot` and`-target` in`#cgo` flags, and verifies`-isysroot` and`-Wl,-syslibroot` don’t point to regular files.
- Restricts `-x` to C, C++, Objective-C and assembler.
- Rejects `-L` ,`-isysroot` and`-Wl,-syslibroot` directories that contain GCC spec files, and disallows`@` and`-` prefixes in`-fsanitize=` values.
- Disallows `-gen-*` and`-gnat*` flags.
- On Darwin, disallows `-l` followed by`to_library` and rejects`-lazy_library` ,`-lazy_framework` ,`-load_hidden` ,`-library` and`-library-path` .
- Requires a `-Wl,` prefix on the second token of split`-Wl,` flags (except`-Wl,-R` ).
- Verifies that `.syso` files and direct linker inputs are valid objects without embedded linker options or LTO IR sections, and disallows`-l:` and path separators in`-l` flags.
- Disallows `--with-path=` ,`--personality=` and local`.pc` paths in`#cgo pkg-config:` directives.
- Rejects commas in `//go:cgo_*` symbol and dynamic-linker directives.

If a legitimate dependency breaks, check whether it has a release that drops the unusual flag. Legitimate packages rarely need these flags in `#cgo` lines. Usually the right fix is to set them through environment variables like `CGO_LDFLAGS` in your own build, which you control.

## Fixing the net/http race in your own code

The crash fix lives in the standard library, so upgrading is enough. Still, the bug is a reminder: don’t read the request body in one goroutine while writing headers in another. Read or limit the body first, then write the response:

```
func handler(w http.ResponseWriter, r *http.Request) {
    r.Body = http.MaxBytesReader(w, r.Body, 1<<20) // 1 MiB
    data, err := io.ReadAll(r.Body)
    if err != nil {
        http.Error(w, "request too large", http.StatusRequestEntityTooLarge)
        return
    }
    w.Header().Set("Content-Type", "application/json")
    w.Write(process(data))
}
```
## Key takeaways

- Go 1.27.4 and 1.26.11 fix 12 security issues: 11 cgo and toolchain build-time code execution bugs and one `net/http` server crash.
- Building an untrusted module with cgo could run code on the build machine. Patch developer laptops and CI runners.
- Upgrade with `go get [email protected]` (or`go1.26.11` ), and pin patch versions in Docker and CI.
- Use `go list` to find cgo and`.syso` dependencies, and run`govulncheck` regularly.

Sources: Go 1.27.4 and Go 1.26.11 security announcement (oss-security), Go release history and Go security policy.
