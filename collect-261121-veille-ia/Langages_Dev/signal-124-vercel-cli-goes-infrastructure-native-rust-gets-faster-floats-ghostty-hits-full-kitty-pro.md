---
id: collect-261121-veille-ia/langages-dev/signal-124-vercel-cli-goes-infrastructure-native-rust-gets-faster-floats-ghostty-hits-full-kitty-pro
title: "Signal #124: Vercel CLI Goes Infrastructure-Native, Rust Gets Faster Floats, Ghostty Hits Full Kitty Protocol"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["vercel", "algebraic", "rust", "ghostty", "kitty", "these", "output", "without"]
source: https://dev.to/devsignal/signal-124-vercel-cli-goes-infrastructure-native-rust-gets-faster-floats-ghostty-hits-full-fho
source_anchor: ""
source_lines: [1, 41]
section: "Langages_Dev"
---
This week's batch skews toward terminal-first workflows. Vercel's CLI now handles DNS, domains, and project membership alongside its new trace search, Rust 1.98 stabilizes opt-in algebraic float operations, and Ghostty 1.4 completes the Kitty graphics spec. Individually these are incremental; together they point at the same direction: more of the operational surface area is reachable from a shell, scriptable, and inspectable by agents.

The CLI now supports DNS record updates, domain renewals, and project member management, all with JSON output suitable for piping into scripts or agent tooling. Until now these operations mostly lived in the dashboard or required hitting the REST API directly.

Why it matters now: infrastructure changes that used to be click-driven can live in version-controlled scripts, and an agent with CLI access can manage production domains without a bespoke API wrapper. The tradeoff is obvious: a scriptable DNS mutation is also a scriptable footgun, so gate these commands behind review in any pipeline that touches production.

**Verdict: Ship.** Install with `npm i -g vercel@latest`. Replace dashboard-only DNS and domain workflows, and add the commands to deployment scripts where the change is reviewed and auditable.

Rust 1.98 stabilizes `algebraic_add`, `algebraic_sub`, `algebraic_mul`, `algebraic_div`, and `algebraic_rem`. These permit the compiler to reorder floating-point operations under the assumption that associativity and distributivity hold, which enables loop vectorization and partial-sum parallelization without hand-written SIMD. The result is not undefined behavior, but it is non-deterministic relative to strict IEEE evaluation order, which matters if you depend on bit-exact reproducibility.

The second change is more immediately useful. Buffered integer formatting via `format_into` now matches `itoa` performance in the standard library, which removes a common external dependency for anyone formatting integers in hot paths. The `ManuallyDrop<Box<T>>` fix is a correctness guarantee; you only need to care if you were hitting that edge case.

**Verdict: Ship for `format_into`; evaluate for algebraic floats.** Replace `itoa` with `format_into` now, and drop the dependency. Adopt the algebraic float methods only in numerically heavy code after profiling, and only if your tests tolerate reordered results.

Ghostty 1.4 implements the full Kitty graphics specification, including animations and relative placements. VT parsing is reported at roughly 5.7x faster, and on macOS and 64-bit Linux, scrollback compression is automatic and reduces memory by an estimated 70 to 90 percent. The release also patches eleven security issues, including a tmux control mode crash and hyperlink injection.

For most developers the practical gains are retained scrollback without memory pressure and smoother handling of image-heavy or high-volume output, such as log tailing and diagnostic tools that emit frames. The security fixes are the more pressing reason to upgrade if you're on 1.3.0 or 1.3.1.

**Verdict: Ship.** It's a drop-in upgrade with no configuration changes. Upgrade immediately if you're on 1.3.x, especially if you use tmux or open links from terminal output.

`vercel traces search` queries request spans from the terminal using KQL filters, covering status, duration, span name, and custom attributes. An example like `span.duration > 2000` surfaces slow requests without opening the dashboard. JSON output makes it straightforward to feed results to an agent or a triage script.

The value here is compressing the debugging loop, especially for teams deploying often and wanting agents to investigate production issues with the same tools a human would use. The caveat is that span data is only as useful as your instrumentation; if your attributes are sparse, the filters won't rescue you.

**Verdict: Ship, if you're on Vercel.** Run `vercel upgrade` to get the command, then start with duration and status filters before building anything more elaborate on top.

Eve introduces named memory slots backed by pluggable providers, so agent state persists across conversations instead of resetting on restart. Supported providers include file, Supermemory, Upstash, Kybernesis, and custom implementations. Adding one is done with `eve add memory/file`.

The benefit is removing the pattern of re-injecting prior context on every turn, which cuts both latency and token spend in multi-turn workflows. The tradeoff is that persisted memory is a new state surface to reason about: it needs a clear schema, retention policy, and a way to inspect or purge what the agent has stored.

**Verdict: Evaluate.** It's ready on Vercel, since it uses Vercel Blob. Self-hosted setups will need a custom provider. Before adopting, decide what the agent should remember and what it must forget.

`minimax/minimax-m3-free` and `minimax/minimax-m2.7-free` are available through the AI Gateway until September 6. After the promotion ends, switching to the standard model IDs with GMI Cloud provider ordering preserves billing continuity. Without that change, requests may error when the free tier expires, depending on your routing configuration.

This is a low-cost way to evaluate MiniMax inference, but the operational lesson is the more important one: promotional model IDs are a dependency with an expiration date. Pin the fallback path now rather than discovering it during an incident.

**Verdict: Evaluate now, migrate before September 6.** Test in a non-production route, and update your `streamText` calls with standard model IDs and explicit provider ordering so behavior after the promotion is deliberate.

If this kind of triage is useful to your week, subscribe to Signal at https://repowrapped.com/signal. We'll keep sorting the tooling changes that actually affect shipping code from the ones that merely make good headlines.

## Top comments (0)
