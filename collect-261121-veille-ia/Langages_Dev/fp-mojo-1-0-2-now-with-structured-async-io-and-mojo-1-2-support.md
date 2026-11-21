---
id: collect-261121-veille-ia/langages-dev/fp-mojo-1-0-2-now-with-structured-async-io-and-mojo-1-2-support
title: "`fp_mojo` 1.0.2 — Now with Structured Async IO and Mojo 1.2 Support! 🚀"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["mojo", "structured", "fibers", "thread", "asynchronous", "async", "through", "stack"]
source: https://forum.modular.com/t/fp-mojo-1-0-2-now-with-structured-async-io-and-mojo-1-2-support/3621
source_anchor: ""
source_lines: [1, 150]
section: "Langages_Dev"
---
I took @maxchisto’s and @Kausch’s advice to heart and incorporated a lot of design decisions from Cats and Cats Effect into `fp_mojo`, especially around syntax and our new **structured asynchronous IO runtime**.

With the release of **`fp_mojo` 1.0.2**, the library is fully upgraded and qualified for **Mojo 1.2.0** on both **Linux x86-64** and **macOS arm64**.


In addition to composable functional abstractions—fluent pipelines, zero-allocation lazy streaming, inductive algebraic data types, and compile-time exhaustive pattern matching—the library now includes a structured asynchronous concurrency engine. Programs coordinate non-blocking I/O through lightweight cancelable fibers, a multi-threaded work-stealing scheduler, and deterministic virtual-time simulation, preserving Mojo’s explicit memory ownership and exact compiler error types.


## Architectural Highlights

### 1. Structured Asynchronous Concurrency (`fp.effects.async_runtime`)

Rather than relying on unmanaged OS threads or unchecked callback graphs, `IO[T, E]` describes a program without running it: building one runs nothing, and each value runs once.

- **Bounded Native Stack** : Evaluation occurs through an iterative, trampolined run loop on a dedicated frame stack list whose growth amortizes to ~126 bytes per frame of a runtime-built chain. Deep monadic chains, nested`IO.defer` expressions, and`tail_rec_m` loops execute in constant native stack depth, preventing call-stack overflow under a 1 MiB stack limit.
- **Lightweight Cancelable Fibers** : Fibers are managed concurrent tasks; a start and join takes ~6.8 µs. Started via`.start()` , concurrent branches coordinate through`.both()` (parallel combination),`.race()` /`.race_pair()` (first-result selection), and`Supervisor[E].scoped` (lexical supervisor scopes). Fibers yield cooperatively and, by default, run in parallel on worker threads.
- **Automatic Cancellation & Draining Invariants** : Child fibers remain structurally owned by their enclosing scope. When a parent task completes, errors, or cancels, uncompleted child fibers are automatically notified via atomic cancellation tokens and cooperatively drained through their masked cleanup finalizers before the parent finishes.

### 2. Multi-Threaded Work-Stealing Topologies

- **Work-Stealing Pool (`Profile.work_stealing`)** : The default profile on multi-processor architectures manages worker threads via local ring deques (`_LocalQueue` ) with LIFO optimization, a striped external queue (`_ExternalQueue` ), per-worker timer heaps, and descriptor readiness polling.
- **Cooperative Single-Thread Profile (`Profile.current_thread`)** : Drives asynchronous tasks deterministically on the calling host thread without thread switching or lock contention.
- **Isolated Blocking Offload (`BlockingPool`)** : Synchronous system calls and CPU-intensive operations are offloaded via`IO.blocking(...)` or cancelable`IO.interruptible(...)` to dedicated, cached thread pools, preventing worker thread starvation.

### 3. Payload Thread Safety & Formal Memory Ordering Audit

Mojo does not yet implement native compiler-verified `Send` and `Sync` traits. `fp_mojo` provides compile-time safety boundaries and audited atomic orderings:

- **Static Field-Tree Inspection** :`_Erased.__init__[T]` inspects the recursive field layout of any erased async payload, rejecting structs that reach raw`Pointer` or unsynchronized reference fields unless explicitly marked as conforming to`ThreadTransferable` .
- **Memory Ordering Invariants** : All synchronization primitives (`_OneShot` slot claim/publish,`_ClosableStack` , task scheduling headers, and wake registry notifications) document their atomic acquire-release fences, lost-wake handshakes, and sequential consistency barriers.

### 4. Deterministic Virtual-Time Simulation (`TestControl`)

Asynchronous scheduling, timeout races, and cancellation protocols can be verified deterministically without wall-clock sleeps or non-deterministic test latency:

```
from fp.effects import EffectFailure, fold_outcome
from fp.effects.async_runtime import IO, Duration, TestControl
def success(value: Int) -> Int: return value
def failure(var errors: EffectFailure[Int]) -> Int: return -1
def canceled() -> Int: return 0
def main() raises:
    # A 60-second asynchronous sleep with a 5-second timeout fallback:
    var slow = IO[NoneType, Int].sleep(Duration.seconds(60)).map(lambda (unit: NoneType) -> Int: 42)
    var program = slow^.timeout_to(Duration.seconds(5), IO[Int, Int].pure(7))
    # TestControl advances virtual time instantly without wall-clock execution:
    var control = TestControl(program^)
    control.tick()       # Advances virtual clock to the earliest deadline (5s)
    control.tick_all()   # Drains virtual-time timers and completes execution
    print("result:", fold_outcome(control^.results(), success, failure, canceled))  # result: 7
```

### 5. Resumable Coordination Primitives

State transitions reuse canonical synchronous logic behind non-blocking polling guards:

- `Deferred[T]` : Single-assignment cell for asynchronous result publishing; every waiting reader receives a copy.
- `Ref[T]` : Synchronized cell whose updates are atomic with respect to other fibers.
- `Queue[T]` : Bounded FIFO queue supporting backpressure and move-only elements.
- `Semaphore` : Non-negative permit counter that suspends fibers until a permit is available (no waiter-fairness guarantee).

### 6. Independent Error Accumulation & MTL Capabilities

- **Error Accumulation (`par_map_n`, `par_traverse`)** : Validates independent computations via`ValidationFamily` , preserving*all* aggregated domain errors rather than short-circuiting on the first failure.
- **`NonEmptyList` and `Ior`** : Typed data structures providing guaranteed non-empty error collections and inclusive-or outcomes (valid values accompanied by accumulated warnings).
- **Cats MTL Capability Traits** : Abstract effect capabilities (`Ask` ,`Local` ,`Stateful` ,`Tell` ,`Listen` ,`Censor` ) implemented via compile-time capability dictionaries over monad transformers (`ReaderT` ,`StateT` ,`WriterT` ,`ResultT` ).


## Code Examples

### Independent Error Validation with `par_map_n`

```
from fp.data import Result, Ok, Err
from fp.functions import pipeline
comptime Checked = Result[Int, List[String]]
def positive(value: Int) -> Checked:
    if value <= 0:
        return Err(List[String]([String(value) + " must be positive"]))
    return Ok(value)
def total(a: Int, b: Int, c: Int) -> Int:
    return a + b + c
def main():
    # Both negative inputs fail independently; par_map_n preserves both failures:
    var invalid = pipeline(positive(-2)).par_map_n((positive(3), positive(-4)), total).get()
    try:
        _ = invalid^.raise_on_err()
    except errors:
        for error in errors:
            print("validation error:", error)
    # validation error: -2 must be positive
    # validation error: -4 must be positive
```

### Monadic Async Sequencing and Outcome Settlement

```
from fp.effects import EffectFailure, fold_outcome
from fp.effects.async_runtime import IO, run
from fp.functions import pipeline
def next_value(value: Int) -> IO[Int, Int]:
    return IO[Int, Int].pure(value + 2)
def initial() -> Int:
    return 40
def success(value: Int) -> Int: return value
def failure(var errors: EffectFailure[Int]) -> Int: return -100
def canceled() -> Int: return -200
def main():
    var program = pipeline(IO[Int, Int].delay(initial)).flat_map(next_value).get()
    print("result:", fold_outcome(run(program^), success, failure, canceled))  # 42
```


## Package Architecture

| Package | Responsibility | Primary APIs | 
| `fp.effects.async_runtime` | Structured asynchronous runtime and concurrency primitives | `IO` (`.both` ,`.race` ,`.sleep` ,`.timeout_to` ,`.background` ),`Fiber` ,`Supervisor` ,`Resource` ,`Driver` ,`TestControl` ,`Runtime` ,`Deferred` ,`Queue` ,`Semaphore` | 
| `fp.effects.async_runtime.protocols` | Extension protocols for custom async leaves and scoped borrowing | `Machine` ,`ScopedMachine` ,`CleanupMachine` ,`Context` ,`Step` ,`Wait` ,`Signal` | 
| `fp.effects` | Synchronous IO, resource scopes, and monad transformers | `Reader` ,`State` ,`Writer` ,`ReaderT` ,`StateT` ,`WriterT` ,`OptionalT` ,`ResultT` ,`IO` ,`Resource` ,`Outcome` ,`delay` ,`run_io` ,`guarantee` | 
| `fp.algebra` | Category-theoretic typeclasses and instances | `Functor` ,`Filterable` ,`Applicative` ,`Monad` ,`Traversable` ,`Semigroup` ,`Monoid` ,`par_traverse` ,`tail_rec_m` | 
| `fp.data` | Typed algebraic results and error channels | `Result` ,`Ok` ,`Err` ,`ControlFlow` ,`Ior` ,`NonEmptyList` ,`raise_on_err` ,`collect_results` | 
| `fp.adt` | In-place and recursive algebraic data types | `Data` ,`Cases` ,`Node` (recursive values with shared subtrees),`Choice` (stack-allocated variant) | 
| `fp.matching` | Exhaustive pattern matching and term rewriting | `fp.match` ,`fp.rewrite` ,`fp.when` ,`Next` ,`Guarded` | 
| `fp.iteration` | Zero-allocation lazy adapters and terminal folds | `map` ,`filter` ,`filter_map` ,`flat_map` ,`flatten` ,`scan_left` ,`fold_left` ,`fold_until` ,`collect_list` | 
| `fp.functions` | Eager pipelines, function composition, and partial application | `pipeline` (`.par_map_n` ),`pipe` ,`flow` ,`compose` ,`flip` ,`partial` ,`identity` | 
| `fp.callables` | Fixed-arity callable protocols and receiver dispatch | `Unary` ,`Binary` ,`Thunk` ,`BorrowCallable` ,`as_unary` ,`as_binary` ,`call_once` ,`call_repeated` | 

 

## Installation & Getting Started

Install via Pixi with the Modular community channel (Coming soon https://github.com/modular/modular-community/pull/443 ):

```
pixi add fp_mojo "mojo==1.2.0"
```

Or clone the repository directly:

```
git clone https://github.com/Spellbound-Mojo/fp_mojo.git
cd fp_mojo
pixi install --locked
pixi run mojo run -I src docs/examples/quickstart.mojo
pixi run mojo run -I src docs/examples/async_sequencing.mojo
```

### Documentation & Learning

The complete documentation suite, including our 14-part tutorial series, guides, and API reference, is published at:
