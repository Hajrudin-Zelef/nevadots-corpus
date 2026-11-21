---
id: collect-261121-veille-ia/langages-dev/go-1-27-already-runs-your-encoding-json-on-v2-up-to-10x-faster-unmarshaling
title: "Go 1.27 already runs your encoding/json on v2: up to 10x faster unmarshaling"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["json", "encoding", "already", "error", "unmarshaling", "faster", "unmarshal", "runs"]
source: https://blog.jorgeturbica.dev/en/go-1-27-already-runs-your-encoding-json-on-v2-up-to-10x-faster-unmarshaling
source_anchor: ""
source_lines: [1, 86]
section: "Langages_Dev"
---
Since Go 1.27, every `json.Unmarshal` in your code already runs on the `encoding/json/v2` implementation, even if you never touched an import. The new package and its syntactic foundation, `encoding/json/jsontext`, are now stable after six years of work, and on 4 November 2026 the Go team published the full rationale and a migration guide. The upside is unmarshaling up to 10x faster; the price is stricter defaults that can change what your service sends over the wire.

## What already changed without you doing anything

Go 1.27 shipped on 19 August 2026, and the current release is 1.27.3. Its release notes contain one line that reads quickly and matters a lot: the classic `encoding/json` is now backed by the v2 implementation. `Marshal` and `Unmarshal` behave as before, but **the exact text of error messages may differ**. If you have tests that compare error strings, or alerts that match a specific message in your logs, that is the first place to look when you upgrade.

If something breaks, there is an escape hatch: building with `GOEXPERIMENT=nojsonv2` restores the old implementation. The notes warn that this opt-out will be removed in a future release, so it buys you time, not a permanent exemption.

```
# Build with the old v1 implementation while you investigate
GOEXPERIMENT=nojsonv2 go build ./...
```
## Why a v2 was needed

`encoding/json` had accumulated problems that could not be fixed without breaking the Go 1 compatibility promise. Some are about security: it accepts invalid UTF-8 and silently replaces it, accepts objects with duplicate keys (an attack vector already exploited in CVE-2017-12635), and matches field names case-insensitively. Others are about performance: `MarshalJSON` forces an allocation of a `[]byte` that then gets revalidated, `UnmarshalJSON` means parsing the value twice, and if a method recurses into `Unmarshal` the cost turns quadratic.

The clearest case the Go team cites comes from Kubernetes: recursive parsing of OpenAPI specs inside `UnmarshalJSON` was dragging down a service, and switching to `UnmarshalJSONFrom` improved it by orders of magnitude.

The fix splits the work into two layers. `jsontext` deals only with syntax (tokens and values, truly streaming and without reflection), while `json/v2` gives that data meaning. `json.RawMessage` is now an alias of `jsontext.Value`.

## v1 vs v2: the behavior changes

| Case | `encoding/json` (v1) | `encoding/json/v2` | 
|---|---|---|
| Invalid UTF-8 | Silently replaced | Error | 
| Duplicate keys | Accepted | Error | 
| `nil` slice or map | `null` | `[]` or`{}` | 
| Field names when unmarshaling | Case-insensitive | Case-sensitive | 
| `omitempty` | Based on the Go value | Omits if the JSON would be `null` ,`""` ,`[]` or`{}` | 
| Malformed tags | Ignored | Runtime error | 

On top of that, `time.Duration`: v2 returns an error when marshaling it, because it has no default representation, and lets you choose a format through options.

The `nil` change is the one most likely to bite. A struct with an uninitialized `Nicknames []string` goes from `"Nicknames":null` to `"Nicknames":[]`. For a new service that is an improvement, but if a mobile client or a consumer written in another language treats `null` and an empty list differently, you have just changed your API contract.

Performance-wise, `Marshal` is roughly at parity with v1, while `Unmarshal` is much faster, with gains of up to 10x in the team's benchmarks. To get the full benefit, types with custom serialization should also implement `MarshalJSONTo` and `UnmarshalJSONFrom`, which work directly on the streaming `Encoder` and `Decoder`.

## How to migrate to encoding/json/v2

The API is almost identical: `b, err := json.Marshal(v)` still compiles after switching the import to `encoding/json/v2`. The hard part isn't compiling, it's output changing without anyone noticing. The official guide offers three paths depending on how much risk you can take:

- **All at once** , for simple applications: switch the imports, get the tests passing and set options where something fails.
- **Option by option** : move every call to v2 with`jsonv1.DefaultOptionsV1()` , which behaves exactly like v1, then turn on each v2 behavior separately.
- **With `jsonsplit` in production** : a wrapper that marshals with both v1 and v2, returns the v1 result and reports every difference, including which option caused it.

The second path looks like this. Later options override earlier ones:

```
import (
	jsonv1 "encoding/json"
	"encoding/json/v2"
)
// Behaves like v1, except that nil slices become []
b, err := json.Marshal(v, jsonv1.DefaultOptionsV1(), json.FormatNilSliceAsNull(false))
```
And `jsonsplit` is configured once at startup:

```
func init() {
	jsonsplit.GlobalCodec.SetMarshalCallMode(jsonsplit.CallBothButReturnV1)
	jsonsplit.GlobalCodec.AutoDetectOptions = true
	jsonsplit.GlobalCodec.ReportDifference = func(d jsonsplit.Difference) {
		slog.Warn("json v1/v2 difference", "diff", d)
	}
}
```
For a production service, this is the route I would take: ship the differences to your logs or metrics, let real traffic run for a few days, and only switch the mode to `OnlyCallV2` or `CallBothButReturnV2` once they stop showing up. The cost is real: marshaling twice doubles your JSON work, and option auto-detection adds more. `SetMarshalCallRatio` lets you sample just a fraction of calls.

If you already tried v2, review that code: its API changed during the experiment. The `format` tag option, the `unknown` option and `DiscardUnknownMembers` are gone; `inline` was renamed to `embed`; and the numeric accessors on `jsontext.Token` now also return errors. This affects anyone who used the Go 1.25 experiment or the `github.com/go-json-experiment/json` module.

## Pre-production checklist

- Upgrade to Go 1.27.3 and run your tests without changing imports: this catches comparisons against error messages that changed.
- Search logs, alerts and tests for literal `encoding/json` error strings.
- Before moving a service to v2, review contracts with external consumers: `null` vs`[]` , fields with different casing, and`time.Duration` .
- Run `jsonsplit` with sampling in production and don't switch modes until differences stop arriving.
- On types with heavy `MarshalJSON` or`UnmarshalJSON` methods, implement`MarshalJSONTo` and`UnmarshalJSONFrom` so streaming actually pays off.

There's no rush: v1 will keep working indefinitely and nobody is forcing you to migrate. But rejecting duplicate keys and invalid UTF-8 closes real security gaps, and for services that unmarshal a lot of JSON, the performance difference shows up on the CPU bill.

## Sources

- A new Go API for JSON, Go Blog, 4 November 2026.
- Go 1.27 Release Notes, go.dev.
- encoding/json/v2 Migration Guide, go.dev.
- Release History, go.dev.
