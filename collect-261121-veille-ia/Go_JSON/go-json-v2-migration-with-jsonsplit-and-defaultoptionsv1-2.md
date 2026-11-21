---
id: collect-261121-veille-ia/go-json/go-json-v2-migration-with-jsonsplit-and-defaultoptionsv1-2
title: "Go JSON v2 Migration with jsonsplit and DefaultOptionsV1"
domain: go-json
role: reference
task: article
actors: []
dates: []
keywords: ["json", "jsonsplit", "steps", "encoding", "migration", "test", "defaultoptionsv1", "production"]
source: https://blogs.abhipanseriya.dev/guides/migrate-go-encoding-json-to-v2
source_anchor: ""
source_lines: [1, 342]
section: "Go_JSON"
---
To migrate Go encoding/json to v2 on a live service, move every call onto encoding/json/v2 with DefaultOptionsV1, let jsonsplit compare v1 and v2 on a sample of production traffic, then keep only the v1 options that traffic proved you need. You end with code on the v2 API and the same bytes on the wire. This runbook is for a Go service that still imports `encoding/json` and has consumers you cannot break: other services, mobile clients, rows in a cache.

Go 1.27 (August 2026) made `encoding/json/v2` generally available, and the release notes say the old package is now backed by the v2 implementation, so your v1 calls already run on the new engine. What changes when you switch is defaults: nil slices become `[]`, map keys stop being sorted, `time.Duration` stops marshaling at all. The steps below follow the official migration guide and add the parts it leaves out, each one run before it went in here.

Tested on macOS 27.0.1 (Apple M3 Pro) with Go 1.27.3, jsonsplit v0.0.0-20260814182812-9b1aeb428384 and go-json-experiment/json v1.27.2, November 2026. Steps 1-8 ran locally against a small module; the production observation in steps 5 and 7 was simulated by forcing call modes and reading the expvar counters.

## Prerequisites

- Go 1.27 or later. Check with `go version` ; you should see`go1.27` or newer. The stable v2 package ships in Go 1.27; Go 1.25 and 1.26 only had it behind the jsonv2 experiment.
- A test suite that exercises your JSON types, and permission to add golden files to it. Step 2 adds them if you have none.
- Access to production logs or metrics for the service, and a deploy you can repeat in minutes. Steps 5 and 7 are config changes you will ship more than once.
- A list of who reads your JSON. A difference jsonsplit reports matters only if some consumer depends on it.

Read the 16 behavior differences in the encoding/json "Migrating to v2" section once. You will not need all 16: jsonsplit names the ones your traffic actually hits in Step 5.

Budget about two hours for the code changes in steps 1-4, 6 and 8. Steps 5 and 7 add calendar time on top: days of production traffic, depending on how often your rarer endpoints and batch jobs run.

## Step 1. Raise the go directive in go.mod to 1.27

Go 1.27's `go test` runs the `stdversion` vet check by default, which rejects standard library symbols newer than your module's `go` directive. Raise it before touching any code:

```
go version
go mod edit -go=1.27
grep '^go ' go.mod
```
You should see:

```
go version go1.27.3 darwin/arm64
go 1.27
```
If you skip this, `go build` still succeeds, which is what makes it confusing, but `go test` fails before running a single test with lines like `json.Marshal requires go1.27 or later (module is go1.24)`. If you ran the jsonv2 experiment before Go 1.27, also remove any `format` tag options now: Go 1.27 dropped them, and one team's v1 `json.Marshal` calls started failing with ``json: unsupported value: Go struct field ParentAccountIds has unsupported `format` tag option`` (golang/go#80962). The `inline` tag option was renamed to `embed` in the same release.

## Step 2. Pin today's JSON output with golden tests

The migration's promise is byte-identical output until you choose otherwise, so record the current bytes while the code is still on v1. Add a golden test for each type you marshal, with fixtures that include the awkward values: nil slices and maps, zero numbers under `omitempty`, a map with several keys, strings containing `<` or `&`, byte arrays and durations.

```
package orders
import (
	"bytes"
	"flag"
	"os"
	"path/filepath"
	"testing"
	"time"
)
var update = flag.Bool("update", false, "rewrite golden files")
var fixtures = map[string]Order{
	"empty": {ID: "ord_1"},
	"full": {
		ID: "ord_2", Items: []string{"a"}, Meta: map[string]string{"z": "1", "a": "2"},
		Retries: 3, Note: "R&D <q3>", Checksum: [4]byte{1, 2, 3, 4}, Timeout: 30 * time.Second,
	},
}
func TestEncodeGolden(t *testing.T) {
	for name, o := range fixtures {
		got, err := Encode(o)
		if err != nil {
			t.Fatalf("%s: %v", name, err)
		}
		path := filepath.Join("testdata", name+".golden")
		if *update {
			os.WriteFile(path, got, 0o644)
			continue
		}
		want, _ := os.ReadFile(path)
		if !bytes.Equal(got, want) {
			t.Errorf("%s:\n got  %s\n want %s", name, got, want)
		}
	}
}
```
`Order` and `Encode(Order) ([]byte, error)` stand in for your own type and marshal function. Run the update from the package that owns the test, since other test binaries do not define the `-update` flag:

```
go test . -update
go test ./...
```
Expect the second run to pass. On v1, a fixture with those awkward values looks like this:

`{"id":"ord_2","items":["a"],"meta":{"a":"2","z":"1"},"retries":3,"note":"R\u0026D \u003cq3\u003e","checksum":[1,2,3,4],"timeout":30000000000}`
If the second run already fails before you have changed anything, the output is not stable on v1: look for a custom MarshalJSON that ranges over a map or embeds a timestamp, and pin that first. If you write JSON to an io.Writer anywhere, give that path its own golden file too; in Step 3 it is the one that changed on us. Commit the golden files before going further, so every later step diffs against v1.

## Step 3. Route every call through one wrapper on DefaultOptionsV1

One package that owns all JSON calls turns the jsonsplit swap in Step 4 and the cleanup in Step 8 into one-file changes. Find the callsites first:

`grep -rn --include='*.go' '"encoding/json"' .`
Then create `internal/jsonx` on the v2 API. `DefaultOptionsV1` sets all 21 options that reproduce v1 semantics, and the encoding/json docs state that v1's `Marshal` is semantically identical to `jsonv2.Marshal(v, jsonv1.DefaultOptionsV1())`.

```
package jsonx
import (
	jsonv1 "encoding/json"
	"encoding/json/jsontext"
	"encoding/json/v2"
	"io"
)
func Marshal(v any) ([]byte, error) {
	return json.Marshal(v, jsonv1.DefaultOptionsV1())
}
func Unmarshal(b []byte, v any) error {
	return json.Unmarshal(b, v, jsonv1.DefaultOptionsV1())
}
func Encode(w io.Writer, v any) error {
	return json.MarshalEncode(jsontext.NewEncoder(w), v, jsonv1.DefaultOptionsV1())
}
func Decode(d *jsontext.Decoder, v any) error {
	return json.UnmarshalDecode(d, v, jsonv1.DefaultOptionsV1())
}
```
Point the callsites at `jsonx`. Most of them map one to one; the streaming ones do not:

| v1 call | v2 replacement inside jsonx | What we saw | 
|---|---|---|
| `json.Marshal(v)` | `json.Marshal(v, jsonv1.DefaultOptionsV1())` | Golden test passed unchanged | 
| `json.Unmarshal(b, &v)` | `json.Unmarshal(b, &v, jsonv1.DefaultOptionsV1())` | Tests passed unchanged | 
| `json.MarshalIndent(v, "", "  ")` | `json.Marshal(v, jsonv1.DefaultOptionsV1(), jsontext.WithIndent("  "))` | Byte-identical output | 
| `json.NewEncoder(w).Encode(v)` | `json.MarshalEncode(jsontext.NewEncoder(w), v, jsonv1.DefaultOptionsV1())` | Golden test passed; `MarshalWrite` had lost the trailing newline | 
| `json.NewDecoder(r).Decode(&v)` | Create `jsontext.NewDecoder(r)` once, then call`jsonx.Decode(d, &v)` per value | Read both values; `UnmarshalRead` had rejected the second | 

`go vet ./... && go test ./...`
You should see:

`ok  	example.com/orders	0.339s`
If the build fails with `undefined: json.NewEncoder`, that is expected: v2 has no `NewEncoder`, `NewDecoder` or `MarshalIndent`. The obvious substitute, `json.MarshalWrite`, is documented as not terminating output with a newline, and our golden test failed on exactly that, even with DefaultOptionsV1:

`got "{\"id\":\"ord_1\",\"items\":null,\"meta\":null,\"note\":\"\",\"checksum\":[0,0,0,0],\"timeout\":0}" want "{\"id\":\"ord_1\",\"items\":null,\"meta\":null,\"note\":\"\",\"checksum\":[0,0,0,0],\"timeout\":0}\n"`
`jsontext.Encoder` terminates each top-level value with a newline, so use it through MarshalEncode. On the read side, `json.UnmarshalRead` reads the reader to EOF and expects a single value. Fed `{"N":1} {"N":2}` it returned `invalid character '{' after top-level value`, where v1's `Decoder.Decode` returned the first object. Keep a `jsontext.Decoder` through `jsonx.Decode` for request bodies on a long-lived connection or NDJSON. Options control marshaling behavior, not which API you call, so DefaultOptionsV1 cannot fix either of these for you.

## Step 4. Add jsonsplit and update its v2 dependency

jsonsplit wraps Marshal and Unmarshal so a single call can run v1, v2 or both. Add it together with the latest go-json-experiment/json:

```
go get github.com/go-json-experiment/jsonsplit@latest github.com/go-json-experiment/json@latest
go list -f '{{.GoFiles}}' github.com/go-json-experiment/json
```
You should see:

```
go: added github.com/go-json-experiment/json v1.27.2
go: added github.com/go-json-experiment/jsonsplit v0.0.0-20260814182812-9b1aeb428384
[alias.go options_format.go options_format_global.go]
```
The second module is the part the official guide leaves out. jsonsplit's own source still imports `github.com/go-json-experiment/json` for its v2 side, with a `TODO: Use "encoding/json/v2"` comment, and its go.mod pins a July 2025 pseudo-version of that prototype. The module's README says that on Go 1.27 the package now aliases the standard library implementation, which is what the `alias.go` file list confirms. Without the bump, two things went wrong for us: jsonsplit compared against the prototype instead of the real `encoding/json/v2` (its Duration error still read `specify an explicit format`, advice that points at the removed `format` tag), and the compiler rejected standard library options:

`cannot use json.FormatNilSliceAsNull(true) (value of interface type "encoding/json/v2".Options) as "github.com/go-json-experiment/json".Options value in argument to jsonsplit.Marshal`
If you see that error, run the `go get` line above again and check that `go list -m github.com/go-json-experiment/json` prints v1.27.2 or later.

## Step 5. Shadow-compare v1 and v2 on sampled production traffic

Swap the wrapper's Marshal and Unmarshal to jsonsplit with `CallBothButReturnV1` on 10% of calls. This is shadow testing: callers keep getting v1 bytes while the sampled calls also run v2 and report any difference. `Helper()` makes reports name the real caller instead of the wrapper.

```
package jsonx
import (
	jsonv1 "encoding/json"
	"encoding/json/jsontext"
	"encoding/json/v2"
	"io"
	"log/slog"
	"github.com/go-json-experiment/jsonsplit"
)
func init() {
	jsonsplit.GlobalCodec.AutoDetectOptions = true
	jsonsplit.GlobalCodec.ReportDifference = func(d jsonsplit.Difference) {
		slog.Warn("detected jsonv1-to-jsonv2 difference", "diff", d)
	}
	jsonsplit.GlobalCodec.SetMarshalCallRatio(jsonsplit.OnlyCallV1, jsonsplit.CallBothButReturnV1, 0.1)
	jsonsplit.GlobalCodec.SetUnmarshalCallRatio(jsonsplit.OnlyCallV1, jsonsplit.CallBothButReturnV1, 0.1)
	jsonsplit.Publish()
}
func Marshal(v any) ([]byte, error) {
	jsonsplit.GlobalCodec.Helper()
	return jsonsplit.Marshal(v)
}
func Unmarshal(b []byte, v any) error {
	jsonsplit.GlobalCodec.Helper()
	return jsonsplit.Unmarshal(b, v)
}
func Encode(w io.Writer, v any) error {
	return json.MarshalEncode(jsontext.NewEncoder(w), v, jsonv1.DefaultOptionsV1())
}
func Decode(d *jsontext.Decoder, v any) error {
	return json.UnmarshalDecode(d, v, jsonv1.DefaultOptionsV1())
}
```
Set `ReportDifference` in `init`: the docs require it before any Marshal or Unmarshal call. Encode and Decode stay on DefaultOptionsV1 because jsonsplit has no writer or stream wrappers, so the golden tests from Step 2 remain your only check on those paths. Deploy, then read the counters `Publish()` exposes under the `jsonsplit` expvar. If your service serves `http.DefaultServeMux`, they are at `/debug/vars`:

`curl -s localhost:8080/debug/vars | jq '.jsonsplit.marshal_option_histogram'`
Replace 8080 with your port. Our test harness had no HTTP server, so we read the same expvar with expvar.Get after 1,000 Marshal calls at a 0.1 ratio: 106 were compared and every one differed, because the fixture hit five v1 behaviors at once. The relevant counters:

`{"num_marshal_total":1000,"num_marshal_call_both":106,"num_marshal_diffs":106,"num_marshal_return_v1":1000,"marshal_option_histogram":{"jsonv1.FormatByteArrayAsArray":106,"jsonv1.FormatDurationAsNano":106,"jsonv1.OmitEmptyWithLegacySemantics":106,"jsonv2.FormatNilMapAsNull":106,"jsonv2.FormatNilSliceAsNull":106}}`
`num_marshal_return_v1` equal to the total is your proof that nothing changed for clients. The log line carries the detail: Marshal reports include the v1 and v2 output, Unmarshal reports include the input JSON. An uppercase-keyed request body produced:

`2026/11/07 18:27:46 WARN detected jsonv1-to-jsonv2 difference diff="{\"Caller\":\"/tmp/hands-on/migrate-go-encoding-json-to-v2/run2/orders.go:24\",\"Func\":\"Unmarshal\",\"GoType\":\"*example.com/orders.Order\",\"JSONValue\":{\"ID\":\"ord_3\",\"NOTE\":\"hi\"},\"Options\":[\"jsonv2.MatchCaseInsensitiveNames\"]}"`
The migration guide warns that comparing doubles the cost of each compared marshal, and `AutoDetectOptions` adds more marshals to find the responsible option. Start at 0.1 and raise the ratio only once you know what the compared calls cost your p99.

If `/debug/vars` has no `jsonsplit` key, `Publish()` never ran: check that the binary imports `jsonx`. If the key exists but every counter stays at zero, your callsites still go straight to encoding/json; rerun the grep from Step 3.

## Step 6. Resolve each reported difference with a tag or a kept option

Every name in the Options list is a v1 behavior your traffic depends on. Where a tag fixes it, prefer the tag: `omitzero` is supported identically by v1 and v2, so the type serializes the same whichever package calls it. Keep an option when the behavior is part of your wire contract. Names map to packages by prefix: `jsonv2.` is `encoding/json/v2`, `jsonv1.` is `encoding/json`, `jsontext.` is `encoding/json/jsontext`.

| Reported option | What v2 does instead | Fix we used | 
|---|---|---|
| `jsonv1.OmitEmptyWithLegacySemantics` | Emits `"retries":0` for a zero int | Change the tag to `omitzero` , which v1 and v2 both support | 
| `jsonv2.FormatNilSliceAsNull` ,`jsonv2.FormatNilMapAsNull` | `[]` and`{}` instead of`null` | Keep both if any client checks for null | 
| `jsonv1.FormatDurationAsNano` | Errors: `no default representation` | Keep the option (the `format` tag is gone in 1.27) | 
| `jsonv1.FormatByteArrayAsArray` | Base64 string, `"AAAAAA=="` | Keep it if the bytes are stored or sent as arrays | 
| `jsontext.EscapeForHTML` | Writes `<` and`&` unescaped | Keep it if any output lands in HTML | 
| `jsonv2.MatchCaseInsensitiveNames` | `"NOTE"` no longer fills`Note` | Keep it, plus `jsonv1.MatchCaseSensitiveDelimiter(true)` | 
| `jsonv2.Deterministic` | Map keys in random order | Keep it if anything hashes, caches or diffs the output | 

v2's case-insensitive matching also ignores underscores and dashes, so `MatchCaseInsensitiveNames` alone would start matching `foo_bar` to `fooBar`; `MatchCaseSensitiveDelimiter(true)` restores v1's exact behavior. And `jsonv2.Deterministic` showed up in only two of our three identical runs, because a random map order sometimes happens to match the sorted one. Union the option lists over the whole observation window rather than trusting one log line.

Put the kept options in one variable and pass them to jsonsplit, which applies them on top of its v1 or v2 defaults:

```
var keepV1 = json.JoinOptions(
	json.Deterministic(true),
	json.FormatNilMapAsNull(true),
	json.FormatNilSliceAsNull(true),
	json.MatchCaseInsensitiveNames(true),
	jsonv1.MatchCaseSensitiveDelimiter(true),
	jsonv1.FormatByteArrayAsArray(true),
	jsonv1.FormatDurationAsNano(true),
	jsontext.EscapeForHTML(true),
)
func Marshal(v any) ([]byte, error) {
	jsonsplit.GlobalCodec.Helper()
	return jsonsplit.Marshal(v, keepV1)
}
func Unmarshal(b []byte, v any) error {
	jsonsplit.GlobalCodec.Helper()
	return jsonsplit.Unmarshal(b, v, keepV1)
}
```
Because Step 4 updated the mirror module, these are plain `encoding/json/v2` options and the same variable carries over to Step 8 unchanged. Redeploy and watch `num_marshal_diffs` and `num_unmarshal_diffs` stop growing. Ten shadow runs after this change reported `0` differences on our module, and the golden tests still passed on v1, which is the check that a tag change like `omitzero` did not move a byte.

If a difference names `jsonv1.ReportErrorsWithLegacySemantics`, look at the struct before keeping that option. The migration guide's example is a `string` tag on a type it cannot apply to: v1 ignores it, v2 errors, and the right fix is deleting the tag.

## Step 7. Start returning v2 results

Once the counters stay flat for a full traffic cycle, long enough to include your batch jobs and rare endpoints, flip the ratios so a share of calls return v2 output while the rest keep comparing. Start at 10% and step up:

```
jsonsplit.GlobalCodec.SetMarshalCallRatio(jsonsplit.CallBothButReturnV1, jsonsplit.OnlyCallV2, 0.1)
jsonsplit.GlobalCodec.SetUnmarshalCallRatio(jsonsplit.CallBothButReturnV1, jsonsplit.OnlyCallV2, 0.1)
```
At ratio 1.0 every call is v2. In our run 1,000 marshals reported `"num_marshal_only_call_v2": 1000, "num_marshal_return_v2": 1000, "num_marshal_diffs": 0`, and `go test -count=20 ./...` passed against the v1 golden files. If errors or client complaints rise at any ratio, set the ratio back to 0. `SetMarshalCallRatio` is documented as safe to call concurrently with Marshal, so if you read it from a config flag you can revert without a deploy.

## Step 8. Replace jsonsplit with encoding/json/v2

With v2 serving all traffic, drop the comparison layer. The wrapper ends up with the kept options and nothing else:

```
package jsonx
import (
	jsonv1 "encoding/json"
	"encoding/json/jsontext"
	"encoding/json/v2"
	"io"
)
var keepV1 = json.JoinOptions(
	json.Deterministic(true),
	json.FormatNilMapAsNull(true),
	json.FormatNilSliceAsNull(true),
	json.MatchCaseInsensitiveNames(true),
	jsonv1.MatchCaseSensitiveDelimiter(true),
	jsonv1.FormatByteArrayAsArray(true),
	jsonv1.FormatDurationAsNano(true),
	jsontext.EscapeForHTML(true),
)
func Marshal(v any) ([]byte, error) {
	return json.Marshal(v, keepV1)
}
func Unmarshal(b []byte, v any) error {
	return json.Unmarshal(b, v, keepV1)
}
func Encode(w io.Writer, v any) error {
	return json.MarshalEncode(jsontext.NewEncoder(w), v, keepV1)
}
func Decode(d *jsontext.Decoder, v any) error {
	return json.UnmarshalDecode(d, v, keepV1)
}
```
```
go mod tidy
go vet ./... && go test -count=50 ./...
grep -rn --include='*.go' 'go-json-experiment' . || echo "no jsonsplit imports"
```
You should see:

```
ok  	example.com/orders	0.358s
no jsonsplit imports
```
Run the tests with `-count` well above 1. When we left `json.Deterministic(true)` out of this list, `-count=20` failed on the two-key map fixture, because v2's random map order only sometimes disagrees with the sorted golden file:

```
got  {"id":"ord_2","items":["a"],"meta":{"z":"1","a":"2"},"retries":3,"note":"R\u0026D \u003cq3\u003e","checksum":[1,2,3,4],"timeout":30000000000}
want {"id":"ord_2","items":["a"],"meta":{"a":"2","z":"1"},"retries":3,"note":"R\u0026D \u003cq3\u003e","checksum":[1,2,3,4],"timeout":30000000000}
```
This step also moves `Encode` and `Decode` from DefaultOptionsV1 to `keepV1`, and jsonsplit never compared those paths. If a writer or stream golden test fails here, the missing option is one jsonsplit could not have reported; add it to keepV1 rather than updating the golden file.

## Verify

- `go vet ./... && go test -count=50 ./...` passes with the golden files recorded in Step 2 still unmodified.`git diff <step-2-commit> -- testdata/` should print nothing.
- `grep -rn --include='*.go' '"encoding/json"' .` lists only`internal/jsonx` , where the import exists for the`jsonv1` option constructors.
- `go.mod` declares`go 1.27` and, after`go mod tidy` , no longer requires either go-json-experiment module.
- Production error rates for JSON decode failures are flat compared to the week before Step 7. v2 rejects invalid UTF-8 and duplicate object names unless you kept `jsontext.AllowInvalidUTF8` or`jsontext.AllowDuplicateNames` , so a rise there is a client sending bad JSON that v1 used to accept.

## Rollback

Up to Step 7, rollback is a ratio change. Set both ratios to 0 and every call returns v1 output again:

```
jsonsplit.GlobalCodec.SetMarshalCallRatio(jsonsplit.OnlyCallV1, jsonsplit.CallBothButReturnV1, 0)
jsonsplit.GlobalCodec.SetUnmarshalCallRatio(jsonsplit.OnlyCallV1, jsonsplit.CallBothButReturnV1, 0)
```
After Step 8, revert the commit, which restores the Step 7 jsonsplit wrapper. That wrapper ends at ratio 1.0, all v2, so set both ratios back to 0 with the two lines above before you deploy, or the revert ships v2 output again:

```
git revert <step-8-commit>
go test ./...
```
We ran exactly that sequence: after the revert and the ratio change, 1,000 marshals reported `"num_marshal_only_call_v1": 1000` and `"num_marshal_return_v2": 0`.

Tags you changed in Step 6 can stay, since `omitzero` is supported identically by v1.

Do not plan on `GOEXPERIMENT=nojsonv2`. The release notes describe it as restoring the original v1 implementation for code that calls encoding/json, and as an opt-out expected to be removed in a future release. Code that imports the v2 package does not build with it:

`imports encoding/json/v2: build constraints exclude all Go files in /opt/homebrew/Cellar/go/1.27.3/libexec/src/encoding/json/v2`
The point of no return is not in the code. It is any JSON that v2 writes somewhere durable without the matching kept option: a cache, a queue, a database column. When we fed v2-shaped data back to v1, `{"checksum":"AQIDBA=="}` failed with `json: cannot unmarshal string into Go struct field R.checksum of type [4]uint8`. Rolling the code back does not rewrite those rows. Before you drop any kept option later, check whether its output is persisted and whether an older binary will ever read it.

## What's next

Each option in `keepV1` names a v1 behavior you chose to keep. Revisit them one at a time with the same golden tests, starting with the ones no external consumer sees, such as `Deterministic` on maps that never leave the service. If unmarshal speed rather than strictness brought you here, benchmark your own payloads on v2 before reaching for a third-party JSON library. The same pin-the-output-then-swap approach works outside Go: we used it to migrate to TypeScript 7 beta without breaking CI, and it is the check worth having whenever an engine changes under a stable API, as with Prisma 7 deleting its Rust engine or Bun's Rust rewrite.
