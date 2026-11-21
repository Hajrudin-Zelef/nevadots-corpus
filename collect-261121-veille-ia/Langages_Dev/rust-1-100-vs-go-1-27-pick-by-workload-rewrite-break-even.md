---
id: collect-261121-veille-ia/langages-dev/rust-1-100-vs-go-1-27-pick-by-workload-rewrite-break-even
title: "Rust 1.100 vs Go 1.27: Pick by Workload, Rewrite Break-Even"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-08-19"]
keywords: ["rust", "release", "collector", "every", "pick", "memory", "discords", "workload"]
source: https://tech-insider.org/rust-vs-go-2026/
source_anchor: ""
source_lines: [1, 171]
section: "Langages_Dev"
---
Pick Go for most services, and pick Rust where latency, memory or memory safety justify the extra up-front effort. This page deliberately gives no flat “Rust is Nx faster” or compile-time multiplier, because the third-party figures use different hardware, workloads and Go versions and the sources we can cite do not support one; measure your own workload. That is the call as of 12 November 2026, the day the Rust team announced Rust 1.100.0, with Go 1.27 as the current Go release. Below: what each release changed, Discord’s 2020 garbage-collector case set against Go’s newer collector, what an upgrade can break, where each language is proven in production, and a break-even calculation for a Go-to-Rust rewrite that takes your own measured speed-up as its input.

## Go 1.27 and Rust 1.100: versions, release rhythm and the support window

Go 1.27.0 was released on 19 August 2026, per the Go release history. Go 1.26 arrived in February 2026, six months after Go 1.25, according to the Go 1.26 release notes, which also say the release keeps the Go 1 compatibility promise. On the Rust side, the Rust release notes list 1.95.0 on 16 April 2026 and 1.100.0 on 12 November 2026, the version the Rust 1.100.0 announcement introduces. That is five releases in 30 weeks, one every six weeks, against one Go major release every six months.

| Attribute | Go | Rust | 
|---|---|---|
| Current stable | Go 1.27.0 | Rust 1.100.0 | 
| Released | 19 August 2026 | 12 November 2026 | 
| Release rhythm | One major release every six months | One release every six weeks | 
| Memory management | Garbage collected; Green Tea collector on by default since Go 1.26 | Ownership and borrowing rules checked at compile time | 
| Language additions this year | Generic methods in Go 1.27, encoding/json/v2 | If let guards in 1.95, never type in 1.100 | 

Support differs in kind. The Go release history says each major release “is supported until there are two newer major releases.” With 1.27 out, Go 1.27 and Go 1.26 are supported, and a service still on Go 1.25 is not.

The memory models differ at the root. Goroutines are lightweight tasks scheduled by Go’s runtime; the Go 1.4 release notes say the default starting size of a goroutine’s stack was reduced from 8192 bytes to 2048 bytes. Rust has no collector. Every value has one owner and is dropped when the owner goes out of scope, and the Rust book puts the borrowing rule this way: “At any given time, you can have either one mutable reference or any number of immutable references. References must always be valid.” The compiler enforces that, which is how Rust rules out data races and use-after-free errors at compile time.

What each language shipped this year shapes the rest of this page. Rust 1.95 stabilized if let guards on match arms and Rust 1.99 stabilized C-variadic function definitions. Rust 1.100 stabilized the never type “after 10 years and 5 failed stabilization attempts,” in the Rust team’s words. Go 1.27 added generic methods: “a method declaration may declare its own type parameters.”

## Does the garbage collector still justify leaving Go? Discord’s 2020 case against Green Tea

Discord’s engineering post of 4 February 2020 describes moving its Read States service from Go to Rust. The reason was latency: “every few minutes we saw large latency spikes that were bad for user experience,” which the team traced to “core Go features: its memory model and garbage collector.” It remains the primary source for the best-known Go-to-Rust migration, and it describes the Go runtime of 2020.

Go has changed since. Go 1.26 enabled Green Tea by default and expects a 10-40% reduction in garbage-collection overhead in real-world programs that use the collector heavily. That is a cost figure for the collector, not a pause-time figure, and Discord’s post predates it by six years, so neither document tells you whether Discord’s spikes would return on Go 1.27.

Go 1.27 adds size-specialized allocation, which makes some small allocations (under 80 bytes) up to 30% cheaper, but the release notes expect only about 1% overall in real allocation-heavy programs. The goroutine leak profile, an experiment in Go 1.26, is generally available in Go 1.27. Go 1.26 also cut the baseline runtime overhead of cgo calls by about 30%. On the Rust side, 1.100 stabilized the core of the allocator API, so custom allocators now work with Vec and Box.

The practical test is cheap: run the service on Go 1.27 and compare the collector’s share of CPU and your tail latency with the figures that made you consider leaving. If the spikes persist, Discord’s case applies to you. If they do not, the argument for Rust has to rest on throughput, memory or safety instead.

## The same concurrent fetch and error path in Go 1.27 and Rust 1.100

Both samples fetch a list of URLs concurrently. The Go version closes each response body and carries errors in the result; the Rust version turns a failed request or a failed task into an error value in place of a bare unwrap, so neither leaks a connection or panics on a failure. Go’s results arrive in completion order, Rust’s in input order.

```
type Result struct {
	URL    string
	Status int
	Err    error
}
func fetchAll(urls []string) []Result {
	ch := make(chan Result)
	for _, u := range urls {
		go func(u string) {
			ch <- fetch(u)
		}(u)
	}
	results := make([]Result, 0, len(urls))
	for range urls {
		results = append(results, <-ch)
	}
	return results
}
func fetch(u string) Result {
	resp, err := http.Get(u)
	if err != nil {
		return Result{URL: u, Err: err}
	}
	defer resp.Body.Close()
	return Result{URL: u, Status: resp.StatusCode}
}
```
```
async fn fetch(url: &str) -> Result<u16, String> {
    let resp = reqwest::get(url)
        .await
        .map_err(|e| format!("{url}: {e}"))?;
    Ok(resp.status().as_u16())
}
async fn fetch_all(urls: Vec<String>) -> Vec<Result<u16, String>> {
    let futures: Vec<_> = urls
        .into_iter()
        .map(|url| tokio::spawn(async move { fetch(&url).await }))
        .collect();
    let mut results = Vec::new();
    for future in futures {
        results.push(match future.await {
            Ok(r) => r,
            Err(e) => Err(format!("task failed: {e}")),
        });
    }
    results
}
```
Error handling shows the other difference. Go returns an error as the last value and the caller checks it. Rust encodes the failure in the return type and propagates it with the `?` operator.

```
func processFile(path string) error {
	data, err := os.ReadFile(path)
	if err != nil {
		return fmt.Errorf("reading file: %w", err)
	}
	result, err := parse(data)
	if err != nil {
		return fmt.Errorf("parsing: %w", err)
	}
	err = save(result)
	if err != nil {
		return fmt.Errorf("saving: %w", err)
	}
	return nil
}
```
```
enum AppError {
    ReadFile(std::io::Error),
    Parse(ParseError),
    Save(SaveError),
}
fn process_file(path: &str) -> Result<(), AppError> {
    let data = std::fs::read(path).map_err(AppError::ReadFile)?;
    let result = parse(&data).map_err(AppError::Parse)?;
    save(&result).map_err(AppError::Save)?;
    Ok(())
}
```
Here `parse`, `save`, `ParseError` and `SaveError` stand for your own code. Interfaces and traits are the other everyday pairing: a Go interface lists methods that any type can satisfy, while a Rust trait is implemented for a type explicitly and can also carry default methods and associated types.

```
type Reader interface {
	Read(p []byte) (n int, err error)
}
```
```
trait Read {
    fn read(&mut self, buf: &mut [u8]) -> Result<usize, std::io::Error>;
}
```
Several 2026 changes touch samples like these. Go 1.26 lets the built-in `new` take an expression as its operand, which helps with optional pointer fields in serialization, and it revamped `go fix` as the home of Go’s modernizers. Go 1.27 adds `encoding/json/v2`, whose stricter defaults reject invalid UTF-8 in JSON strings and duplicate names within an object, so moving to it changes behaviour. In Rust, if let guards on match arms arrived in 1.95.

## What an upgrade can break, and how each toolchain handles dependencies

The Rust 1.100 announcement flags one change to read before bumping a pinned toolchain: it changes the “never type fallback” from `()` to `!` in editions before 2024, meaning 2015, 2018 and 2021, and it makes `Infallible` an alias of the never type. Code on those older editions is where to look first. Go’s answer to upgrade risk is the Go 1 compatibility promise that the 1.26 notes restate, together with the two-release support window described above. The one place Go 1.27 asks for care is json/v2, whose stricter defaults reject invalid UTF-8 and duplicate object names; the release notes also say encoding/json is now backed by the v2 implementation, so check the notes for behavioural differences.

Cadence compounds the difference. A Rust team that tracks stable sees a release about every six weeks; a Go team sees two a year and can stay on a supported release by upgrading roughly once a year.

Rust 1.100 also stabilizes Cargo’s min-publish-age aware dependency resolution, which waits out a window before picking up newly published versions. The announcement is plain about its limits: “Cooldowns reduce the risk from some supply chain attacks but are not a complete defense on their own.” Cargo gained its own lint system too, with controls in the `[lints.cargo]` table, and it now reports unused dependencies through it. Per the Rust release notes, the same release promoted `wasm32-wasip3` to Tier 2.

## Where each ecosystem is proven: Firecracker, Uber’s Go libraries and Axum under Tokio

On the Rust side, the Firecracker project’s FAQ answers the language question directly: “Firecracker is written in Rust.” The Firecracker FAQ says it was built to enable services such as AWS Lambda. That is a concrete case of Rust used where memory safety and a small footprint matter.

On the Go side, Uber’s open-source Go projects include Zap, for structured logging, and FX, for dependency injection; Uber’s engineering site lists Zap as “Blazing fast, structured, leveled logging in Go.”

Axum belongs to the Tokio project: the Tokio blog announced axum 0.7.0 and axum 0.8.0,. The code above uses Tokio’s `tokio::spawn`.

## Choosing by workload, and what a Go-to-Rust rewrite has to beat

| Workload | Pick | Evidence | 
|---|---|---|
| Tail latency is the product and heap churn is high | Rust, after measuring on Go 1.27 | Discord moved Read States in 2020 over GC latency spikes; Go 1.26 expects a 10-40% cut in GC overhead in GC-heavy programs, which Discord’s post predates | 
| Fleet cost driven by throughput or memory | Rust, if the gain you measure beats the rewrite cost | Break-even calculation below | 
| CRUD APIs, orchestration and internal services | Go | The Go 1 compatibility promise and the two-release support window; Uber’s open-source Zap and FX are Go projects | 
| Heavy calls into C libraries | Measure both | Go 1.26 cut baseline cgo call overhead by about 30%; Rust 1.99 stabilized C-variadic function definitions | 
| WebAssembly targets | Check Rust first | wasm32-wasip3 promoted to Tier 2 in Rust 1.100 | 
| Replacing C or C++ where memory safety is the requirement | Rust | Compile-time ownership and borrowing rules; Firecracker is written in Rust | 

For a team choosing what to learn or staff around, the pattern in the table holds: Go keeps a long compatibility promise and a short support-and-upgrade routine, while Rust asks for more up front and pays back in resource-bound or safety-critical code. That is Tech Insider’s reading of the evidence, not a measured learning curve. If the real alternative is another language, our Python vs Rust comparison and Zig vs Rust comparison cover those matchups.

If you do move code from Go to Rust, the concepts map like this:

- Goroutines become Tokio tasks (`tokio::spawn` or a`JoinSet` ).
- Channels become `tokio::sync::mpsc` channels.
- Interfaces become traits, which can also carry default methods and associated types.
- Error returns become `Result` values propagated with`?` .
- Slices become `Vec<T>` and`&[T]` .
- `defer` cleanup becomes the`Drop` trait.

The cost of a rewrite is engineering time set against what the new language saves. Define the ratio r as Rust’s throughput divided by Go’s on your own workload, measured on Go 1.27, let B be the monthly compute bill and C the rewrite cost. If a service is throughput-bound and its fleet scales with load, a ratio r cuts the instances needed to 1 / r of today’s, so the monthly saving is B x (1-1 / r).

The general form is break-even months = C / (B x (1-1 / r)). Because 1 / r stays above zero, the saving can never exceed the whole bill, so payback is never shorter than C / B months however fast the new code runs. Put your own bill and rewrite estimate in, and use your own measured ratio, because a bill that is dominated by memory or a service whose problem is tail latency needs that measurement in place of a throughput gain.
