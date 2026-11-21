---
id: collect-261121-veille-ia/copilot-microsoft/go-1-27-0-1-microsoft-build-now-available
title: "Go 1.27.0-1 Microsoft build now available"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["systemcrypto", "release", "build", "microsoft", "fips", "goexperiment", "available", "notes"]
source: https://devblogs.microsoft.com/go/go-1-27-0-1-microsoft-build-now-available/
source_anchor: ""
source_lines: [1, 59]
section: "Copilot_Microsoft"
---
A new release of the Microsoft build of Go is now available for download. For more information about this release and the changes included, see the table below:

| Microsoft Release | Upstream Tag | 
|---|---|
| v1.27.0-1 | go1.27.0 release notes | 

As of this release, Go 1.25 is no longer supported, per the Go release policy.

## Changes in the Microsoft build of Go 1.27

The 1.27.0-1 release is a major version update.

As always, please let us know if you encounter a problem or have a question by filing an issue. If you have access to Microsoft internal sites, you can alternatively use one of the channels listed in our internal support documentation such as the Golang Friends group in Teams.

The following is a summary of the Microsoft build of Go 1.27 release notes, emphasizing important changes. To see the canonical release notes doc, visit the full go1.27 release notes Markdown file.

## Systemcrypto

See the Go 1.27 changelog in the FIPS documentation for more information.

### Systemcrypto is no longer a GOEXPERIMENT setting

`systemcrypto` is still enabled by default on supported platforms, as it has been since Go 1.25 (Linux and Windows) and Go 1.26 (macOS).
What’s new in 1.27 is that it is now selected automatically by the toolchain instead of being configured through `GOEXPERIMENT`:

- Setting the environment variable `GOEXPERIMENT` to`systemcrypto` or`nosystemcrypto` is now rejected with an error.
To disable`systemcrypto` , set the environment variable`MS_GO_NOSYSTEMCRYPTO` to`1` instead.
- `systemcrypto` no longer appears in`go env GOEXPERIMENT` ,`goexperiment.Flags` , or other GOEXPERIMENT-derived output.

The per-platform GOEXPERIMENTs `opensslcrypto`, `cngcrypto`, and `darwincrypto` have also been removed and now cause a build error.

The `goexperiment.systemcrypto` build tag is still emitted when `systemcrypto` is enabled.
That tag, and the build tags for the removed per-platform experiments, remain supported for source compatibility, with no change in behavior.

### New platform and backend support

`systemcrypto` is now supported on FreeBSD (`amd64` and `arm64`), using the same OpenSSL backend as on Linux.

On Linux, `systemcrypto` now supports `CGO_ENABLED=0` on architectures that have a cgo-less OpenSSL implementation.
This replaces the Go 1.26 `GOEXPERIMENT=ms_nocgo_opensslcrypto` experiment, which has been removed.

### New algorithms

The `systemcrypto` backends now implement the following ML-DSA parameter sets:

- ML-DSA-44
- ML-DSA-65
- ML-DSA-87

### FIPS 140

`GODEBUG=fips140=only` has been added.
It acts like `fips140=on`, but also panics if a non-FIPS-approved algorithm is used.

`GODEBUG=fips140=off` now explicitly disables FIPS mode and skips the platform-specific FIPS detection (such as the Linux kernel FIPS flag at `/proc/sys/crypto/fips_enabled`).

The `GOFIPS` and `GOLANG_FIPS` environment variable checks now match their intended behavior: only the value `1` enables FIPS mode, and any other value (including `0` and the empty string) is treated as if the variable were unset.

## 0 comments
