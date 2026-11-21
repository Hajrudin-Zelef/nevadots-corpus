---
id: collect-261121-veille-ia/langages-dev/typescript-7-0-is-ga-the-10x-compiler-migration-playbook
title: "TypeScript 7.0 Is GA: The 10x Compiler Migration Playbook"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["typescript", "compiler", "defaults", "code", "migration", "tooling", "playbook", "install"]
source: https://www.digitalapplied.com/blog/typescript-7-0-ga-native-compiler-migration-playbook-2026
source_anchor: ""
source_lines: [1, 171]
section: "Langages_Dev"
---
TypeScript 7.0 reached general availability on July 8, 2026 — a complete native port of the compiler and language service, rewritten in Go, that Microsoft says delivers full-build speedups “typically between 8x and 12x.” The flagship number: type-checking the VS Code codebase fell from 125.7 seconds on TypeScript 6 to 10.6 seconds on TypeScript 7 at default settings — an 11.9x speedup.

The catch is that GA is not a uniform green light. TypeScript 7.0 ships without a stable programmatic API — Microsoft expects TypeScript 7.1 to deliver a new, different one — which means Vue, Angular’s template type-checking, Svelte, Astro, MDX, and API-dependent tooling like typescript-eslint cannot simply flip the switch. At the same time, 7.0 hardens TypeScript 6.0’s deprecations into outright errors and adopts strict defaults, so even the teams that *can* move today have a real migration to run, not a version bump.

This playbook covers what changed between the RC and GA, the benchmark numbers with their scopes kept honest, what early adopters report in CI, the full list of hardened defaults, a who-can-move-now matrix, and a step-by-step migration sequence for the Next.js and React shops that can adopt today.

1. 01GA landed July 8, 2026 — a Go-native compiler port.TypeScript 7 replaces the prior compiler, which was written in TypeScript and ran on Node.js, with native Go code plus shared-memory multithreading. Install is now the standard npm install -D typescript.
2. 02The honest speedup range is 7.7x to 11.9x at defaults.Microsoft’s five published codebases (VS Code, Sentry, Bluesky, Playwright, tldraw) range 7.7x to 11.9x on full builds at default settings. The 16.7x figure is a tuned --checkers 8 run; the VS Code team’s own 36s-to-5s number is a different command and scope entirely.
3. 03Strict mode and 6.0 deprecations are now hard defaults.strict is true, module defaults to esnext, types defaults to empty, and flags like target: es5, baseUrl, and moduleResolution: node are hard errors rather than warnings. Budget config-reconciliation time before upgrading.
4. 04No stable programmatic API until TypeScript 7.1.Vue, Angular templates, Svelte, Astro, MDX, and Volar-based tooling cannot run on 7.0 yet. The @typescript/typescript6 package bridges API-dependent tooling with a tsc6 binary and the 6.0 API re-exported.
5. 05Next.js, React, and plain Node services can move now.App-code type-checking has no API dependency. Early adopters relay large CI wins — Slack reports its type-check step fell from about 7.5 minutes to about 1.25 minutes — though all such figures are vendor-relayed rather than independently audited.

## 01 — RC to GAWe covered the RC — here is what *changed* at GA.

The 7.0 Release Candidate shipped on June 18, 2026, and we walked through the Go port’s architecture in our TypeScript 7.0 RC upgrade guide at the time. GA landed just under three weeks later, on July 8. If you read the RC coverage, here is the delta that matters:

- **Distribution is now mainstream.** The RC installed via`npm install -D typescript@rc` ; GA ships under the normal`latest` tag, so`npm install -D typescript` now gets you 7.0. Nightly builds move from the standalone`@typescript/native-preview` package — which had accumulated over 8.5 million weekly downloads before GA — to the standard`typescript` package under the`next` tag.
- **The editor path is now default-on.** The TypeScript Native Preview extension on the VS Code Marketplace becomes the default TypeScript experience once installed, with a command-palette toggle to fall back to 6.0. Visual Studio auto-enables TypeScript 7 based on the workspace with no user action required, per Microsoft.
- **The API story did not change.** Both the RC and GA posts state, in the same words, that 7.0 ships without a stable programmatic API and that 7.1 is expected to ship a new, different one. Anyone hoping GA would quietly unblock Vue or Svelte tooling is still waiting.

One naming footnote: some secondary press coverage refers to the rewrite by the codename “Project Corsa,” but that name does not appear anywhere in Microsoft’s own RC or GA announcements — so we will call it what Microsoft calls it: TypeScript 7, the native port.

"Today we are proud to announce the availability of TypeScript 7, a 10x faster native port of TypeScript!"— TypeScript Team, Microsoft DevBlogs, July 8, 2026

## 02 — BenchmarksThe benchmarks, with their *scopes* kept honest.

Most GA-day coverage collapses this release into a single “10x” claim. That number is real, but it is an average of genuinely different measurements, and conflating them is how upgrade expectations go wrong. There are at least four distinct TypeScript 6-to-7 numbers in circulation, each from a different command, scope, and configuration:

#### Full-build speedup, TypeScript 6 → 7 · Microsoft’s published codebases

Source: Microsoft DevBlogs, Announcing TypeScript 7.0 benchmark tables, July 8, 2026
The table below is the artifact we could not find anywhere else in GA-day coverage: Microsoft’s standardized five-codebase benchmark, the tuned variant, and the VS Code team’s separate dogfooding numbers — side by side, with each row labeled by its actual command and scope so you know which one to quote.

| TypeScript 6 versus TypeScript 7 benchmark matrix: Microsoft’s standardized full-build GA benchmark across five codebases at default settings, followed by tuned and different-scope measurements that are not directly comparable. Assembled by Digital Applied from Microsoft DevBlogs benchmark tables of July 8, 2026 and the VS Code team blog of June 26, 2026. |  |  |  |  | 
|---|---|---|---|---|
| Benchmark | TypeScript 6 | TypeScript 7 | Speedup | Memory delta | 
|---|---|---|---|---|
| Microsoft’s standardized GA benchmark — full builds, default settings (--checkers 4) |  |  |  |  | 
| VS Code | 125.7s | 10.6s | 11.9x | −18% (5.2GB → 4.2GB) | 
| Sentry | 139.8s | 15.7s | 8.9x | −6% | 
| Bluesky | 24.3s | 2.8s | 8.7x | −26% | 
| Playwright | 12.8s | 1.47s | 8.7x | −11% | 
| tldraw | 11.2s | 1.46s | 7.7x | −15% | 
| Tuned or different-scope measurements — not comparable with the rows above |  |  |  |  | 
| VS Code, tuned to --checkers 8 (GA post) | 125.7s | 7.51s | 16.7x | — | 
| VS Code main source, tsc --noEmit (VS Code team blog) | 36s | 5s | over 7x | — | 
| VS Code full npm run watch, ~50 tsconfig projects (VS Code team blog) | ~80s | ~20s | ~4x | — | 
| Error-on-open editor latency, VS Code repo (GA post) | ~17.5s | under 1.3s | over 13x | — | 

Three reading notes. First, the “8x to 12x” range Microsoft quotes is its own rounded characterization — the five published codebases actually span 7.7x to 11.9x at default settings, so cite the precise 11.9x only for the VS Code flagship run. Second, the 16.7x number requires `--checkers 8`, double the default worker count of four — impressive, but a tuned configuration, not what a fresh install does. Third, the VS Code team’s own blog measured a different thing entirely: `tsc --noEmit -p src/tsconfig.json` on the main source went from 36 seconds to 5, and the full `npm run watch` across roughly fifty tsconfig projects went from about 80 seconds to about 20. Different command, different scope, different harness — do not average it into the GA table.

Where does the speed come from? Native code instead of JavaScript on Node.js, shared-memory multithreading across checker workers, and new optimizations in the port. The editor story compounds it: opening a file with an error in the VS Code repo dropped from about 17.5 seconds to under 1.3 — over 13x faster to first diagnostic — and the VS Code team reports its editor-tooling load for the main project fell from close to a minute to about 10 seconds. Watch mode was also rebuilt on a Go port of the Parcel bundler’s cross-platform file-watcher, credited to Parcel’s Devon Govett, after Microsoft found pure polling too resource-hungry in large node_modules trees.

## 03 — CI ImpactWhat early adopters report in *CI*.

Microsoft’s GA post relays results from pre-release testers — a list that includes Bloomberg, Canva, Figma, Google, Lattice, Linear, Miro, Notion, Sentry, Slack, Vanta, Vercel, and VoidZero, plus internal Microsoft teams like Loop, Office, PowerBI, Teams, and Xbox. The numbers below are the concrete ones — with the caveat, worth stating plainly, that every figure in this section is vendor-relayed or vendor-stated, republished inside Microsoft’s own announcement rather than independently audited.

##### CI type-check, down from ~7.5 min

Slack, via its own Bluesky account as relayed in the GA post, says TypeScript 7 cut CI type-checking from about 7.5 minutes to about 1.25 minutes and eliminated 40% of its merge-queue time. In-editor local dev had previously been described as almost unusable.

##### CI wait saved per month

Microsoft’s own internal News Services team reports TypeScript 7 saved roughly 400 hours a month of CI wait time. An internal customer, so inherently vendor-stated — but a useful order-of-magnitude signal for large monorepos.

##### First error in editor, down from ~58s

Canva reports time-to-first-error in the editor dropped from about 58 seconds to about 4.8. This is editor latency, not build time — a different axis of the same rewrite. Vanta separately reports up to a 9x speedup on one of its biggest projects.

Run the arithmetic on Slack’s relayed figures and the CI story gets concrete: 7.5 minutes down to 1.25 saves 6.25 minutes per type-checking run — call it 625 minutes, over ten hours, per hundred CI runs. That translation is our own illustrative math, not a Microsoft or Slack claim, and neither company has published a dollar figure — but if you know your per-minute CI runner rate, the multiplication is yours to do. Teams already reworking pipelines for npm 12’s install-script defaults have a natural window to fold this upgrade into the same CI pass.

Reliability moved with speed, per Microsoft’s own telemetry: the new language server reduced failing language-server commands by more than 80% and server crashes by more than 60% versus TypeScript 6.0. PowerBI engineers described the TypeScript 7 editor experience last year as “life-saving” for their codebase, adopting it as their default before rename support had even shipped in VS Code.

*vendor-relayed*— republished in Microsoft’s announcement, some linked to the customer’s own posts, none independently audited. The language-server reliability stats are Microsoft’s internal telemetry comparison. Treat them as directional, then benchmark TypeScript 7 on your own repository before you commit the team.

## 04 — Breaking ChangesHard defaults, *hard* errors.

TypeScript 7.0 adopts TypeScript 6.0’s newer defaults and turns the constructs 6.0 deprecated into outright errors. If your tsconfig has been carried forward since the 5.x era — we covered that baseline in our TypeScript 5.9 developer guide — this section is your pre-upgrade checklist.

### New defaults you inherit on upgrade

- `strict` is now`true` by default — the headline change for loosely-configured codebases.
- `module` defaults to`esnext` ;`target` defaults to the last stable ECMAScript version before`esnext` .
- `noUncheckedSideEffectImports` is`true` ;`libReplacement` is`false` ;`stableTypeOrdering` is`true` and not configurable.
- `rootDir` now defaults to`./` (previously implicit), and`types` defaults to an empty array — where it previously meant “all @types packages.” Codebases that silently depended on ambient @types will feel this one first.

### Deprecations that are now hard errors

- `target: es5` ,`downlevelIteration` ,`baseUrl` ,`moduleResolution: node` ,`node10` , and`classic` , plus`module: amd` ,`umd` ,`systemjs` , and`none` — all error rather than no-op.
- `esModuleInterop` and`allowSyntheticDefaultImports` can no longer be set to`false` ; neither can`alwaysStrict` .
- The `module` keyword is banned inside namespace declarations, and import`asserts` must use`with` instead.
- CLI builds can no longer take bare file-path arguments when the working directory has a tsconfig.json — you need an explicit `--ignoreConfig` flag.

*one unit*instead of splitting UTF-16 surrogate pairs. Any type-level string-length utility that modeled UTF-16 code units will produce different results on 7.0. Obscure, but the kind of break that surfaces as a mystery test failure three layers away from the cause.

## 05 — Adoption MatrixWho can move *now*, who waits for 7.1.

Microsoft states the compatibility picture only in prose, scattered through the GA post’s embedded-languages section. Here it is as a decision table. The dividing line is simple: if your toolchain needs programmatic access to the compiler — template type-checking, embedded-language support, lint rules that consume the type-checker — you are waiting on the 7.1 API. If it just runs `tsc` and a language server, you can move.

| TypeScript 7 adoption matrix: which stacks can adopt the native compiler today and which are blocked on the TypeScript 7.1 programmatic API, with the reason and the recommended path forward for each. Synthesized by Digital Applied from the Microsoft DevBlogs GA announcement of July 8, 2026. |  |  |  | 
|---|---|---|---|
| Stack | TS 7 today? | Why | Path forward | 
|---|---|---|---|
| Clear to move today |  |  |  | 
| Next.js / React app code | Yes | Type-checking app code has no dependency on the TypeScript programmatic API | npm install -D typescript — then reconcile tsconfig with the new strict defaults | 
| Plain Node / TypeScript backend services | Yes | tsc and the language server are fully ported; emit is finished | Same install; tune --checkers / --builders in CI, --singleThreaded on constrained runners | 
| VS Code / Visual Studio editing | Yes | The TypeScript Native Preview extension becomes the default TS experience once installed; Visual Studio auto-enables per workspace | Fall back any time via the Disable TypeScript 7 Language Server command | 
| Blocked on the TypeScript 7.1 API |  |  |  | 
| Vue | Not yet | Volar-based template tooling needs the programmatic compiler API, which 7.0 does not ship | Stay on 6.0 for tooling; the unblock signal is the 7.1 API | 
| Angular | Split-stack | Template-specific type-checking depends on the API | Microsoft’s interim recommendation: TS 7 tsc for fast whole-project CLI checks, TS 6.0 for editor and template support | 
| Svelte / Astro / MDX | Not yet | Embedded-language tooling cannot run on 7.0 without the API | Stay on 6.0 until the 7.1 API lands | 
| typescript-eslint and other API-dependent tooling | Via shim | 7.0 ships without a stable programmatic API by design | @typescript/typescript6 re-exports the 6.0 API and a tsc6 binary while tsc itself runs on 7.0 | 

For Next.js shops specifically, TypeScript 7 slots into a build pipeline that has been getting faster on several fronts at once — Next.js 16.3’s Turbopack persistent cache attacks bundling time while TypeScript 7 attacks type-checking time, and the two are independent wins that compound in CI. The VS Code team, which adopted TypeScript 7 across its own development before GA, put its assessment on the record:

"TypeScript 7 has everything we needed to start using it fully. The TypeScript team had done truly amazing work making type checking trustworthy, finishing up emit, and improving the language tooling in the editor."— VS Code Team, code.visualstudio.com blog, June 26, 2026

## 06 — The PlaybookThe migration playbook, *step by step*.

For teams in the “clear to move” rows — Next.js, React, plain Node services — here is the sequence we would run. Budget a day for a mid-sized codebase with a reasonably modern tsconfig; more if you are un-deprecating a 5.x-era configuration at the same time.

- **1. Reconcile tsconfig before upgrading.** Grep your config for the hard-error list in Section 04 —`baseUrl` ,`target: es5` , legacy`moduleResolution` values — and remove or replace them while still on 6.x, where they are warnings rather than blockers.
- **2. Install.**`npm install -D typescript` now resolves to 7.0 under the`latest` tag. Run`tsc --noEmit` and triage: config errors first, then the new-strict-default errors if you were not already on`strict` .
- **3. Restore ambient types deliberately.** Because`types` now defaults to`[]` , add back only the @types packages you actually reference — this is a hidden hygiene win, not just a chore.
- **4. Bridge API-dependent tooling.** If typescript-eslint or custom scripts consume the compiler API, add`@typescript/typescript6` — it provides a`tsc6` binary and re-exports the 6.0 API so tooling keeps working while`tsc` itself runs on 7.0.
- **5. Roll out the editor experience.** Install the TypeScript Native Preview extension; it becomes the default TS experience in VS Code, with the Disable TypeScript 7 Language Server command as the escape hatch. Visual Studio picks it up per workspace automatically.
- **6. Tune CI last.** The default worker count is four checkers. On beefier runners, experiment with`--checkers 8` — that is the configuration that took VS Code from 10.6 to 7.51 seconds — and use`--builders` for project-reference parallelism.`--singleThreaded` exists for debugging and resource-limited CI boxes.

Two verification habits pay for themselves here. Diff a production build’s emitted output before and after the upgrade — emit is finished in the port, but trust-then-verify is free. And capture a before/after timing on your own repo; your number, not VS Code’s, is the one that belongs in your upgrade PR description. If your team wants senior hands on a gnarlier migration — monorepos, mixed Angular estates, custom compiler plugins — our web development team runs exactly these upgrades.

##### Next.js, React, *Node*

No programmatic-API dependency in the type-checking path. Reconcile tsconfig against the new hard defaults, bridge lint tooling with @typescript/typescript6, and take the CI win this sprint.

##### Angular estates

Microsoft’s own interim recommendation: use TypeScript 7’s tsc for fast whole-project CLI error detection while keeping 6.0 for editor and template support, reverting the language server in VS Code where needed.

##### Vue, Svelte, Astro, MDX

Volar-based and embedded-language tooling cannot run on 7.0 without a stable programmatic API. The unblock signal to watch for is the new API Microsoft expects to ship in TypeScript 7.1.

## 07 — EcosystemThe ecosystem *floor* is rising.

A small, telling data point landed the same week as GA: on July 10, the Supabase changelog published a deprecation notice titled “Deprecation notice: @supabase/supabase-js will require TypeScript 5.0” — raising the client library’s minimum supported TypeScript version to 5.0. To be precise about what that is and is not: it is a forward-looking notice with no enforced version or cutoff date published yet, and it is a Supabase-specific move that happens to be concurrent with — not caused by — TypeScript 7’s GA. But the direction of travel is the point. Library authors are pruning old TypeScript floors at the same moment the compiler itself hard-errors on its own legacy flags; teams still pinned to old majors will feel squeezed from both ends.

The project’s own trajectory is worth tracking too. The staging repo for the Go port, microsoft/typescript-go, sits at roughly 25.9 thousand GitHub stars under an Apache-2.0 license, with a long-term plan to merge back into the main TypeScript repository. Microsoft says future major versions will keep arriving roughly every three to four months — the same cadence as before the rewrite — with 7.1 expected to carry the new programmatic API. No date for 7.1 has been announced, and Microsoft has said nothing about its feature set beyond the API itself, so treat any specific 7.1 claims you read elsewhere with suspicion.

Our forward read: the interesting second-order effect is not this quarter’s CI savings but what an order-of-magnitude-faster type-checker does to how often type-checking runs. At 10 seconds instead of two minutes, full-project checks become viable on every save, in pre-commit hooks, and inside AI coding agents’ inner loops — places where TypeScript 6 was simply too slow to sit. The teams that treat TypeScript 7 as a latency budget to reinvest, rather than a cost cut to bank, will likely get the most out of the native era.

##### Upgrade in the next sprint

Small tsconfig surface, no embedded-language tooling — the migration is hours, not days, and the editor-latency win lands immediately. Reconcile the strict defaults and go.

##### Upgrade with *measurement*

Biggest absolute CI savings live here — Slack-scale wins came from codebases where type-checking was minutes per run. Benchmark before and after, tune --checkers to the runner, and bridge API tooling via @typescript/typescript6.

##### Prepare, do not force it

Blocked on the 7.1 API through no fault of your own. Use the wait to clear the deprecated-flag list from your configs so the eventual jump is a version bump, not a project.

## 08 — ConclusionA once-a-decade *compiler* moment.

### TypeScript 7 is a real 8-12x — if you quote the right benchmark and run the right playbook.

TypeScript 7.0’s GA is the rare release where the headline number survives scrutiny — as long as you keep the scopes straight. At default settings, Microsoft’s five published codebases span *7.7x to 11.9x* on full builds; the tuned and editor-latency numbers go higher still, and they measure different things. Quote precisely and the release still impresses.

The migration is genuinely two-track. Next.js, React, and Node teams can install today and spend their effort reconciling the new hard defaults — strict mode on, ambient types off, a deprecated-flag list that now errors. Vue, Svelte, Astro, MDX, and Angular-template workflows wait on the 7.1 API, with the *@typescript/typescript6* shim and Microsoft’s split-stack Angular guidance as the bridges.

Either way, the useful move this week is the same: benchmark your own repository, not VS Code’s. The vendor-relayed CI stories — Slack’s minutes-to-seconds type-checks, four hundred hours a month at Microsoft News Services — are directional evidence that the win is large, not a promise about your pipeline. Measure, migrate on your track, and then decide what to do with a type-checker that is suddenly fast enough to run everywhere.
