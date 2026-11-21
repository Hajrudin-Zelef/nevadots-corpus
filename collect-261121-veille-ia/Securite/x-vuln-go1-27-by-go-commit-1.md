---
id: collect-261121-veille-ia/securite/x-vuln-go1-27-by-go-commit-1
title: "x/vuln (go1.27) by go commit"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["reviewed-by", "mark", "commit", "golang", "google", "vuln", "time", "year"]
source: https://ci.chromium.org/p/golang/g/x-vuln-go1.27-by-go/console
source_anchor: ""
source_lines: [1, 1500]
section: "Securite"
---
# x/vuln (go1.27) by go commit

| Legend: | Passed | Failed | Running | Exception | 

              
                [release-branch.go1.27] time: use the real year length when extending past the last transition

              
                

              
                For a time after the last explicit transition, tzset falls back to the

              
                TZ footer rule and reports the end of the standard-time period as the

              
                start of the next year. It computed that as a fixed 365 days from the

              
                start of the year, so in a leap year the end landed a day early, on

              
                December 31. For a time in that last day, ZoneBounds then returned an

              
                end earlier than the time itself.

              
                

              
                Updates #81583

              
                Fixes #83118

              
                

              
                Change-Id: I3fb698c17789550f1f5b1bf7fddc5d1876d8ead3

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/834605

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Mark Freeman <mark@golang.org>

              
                Reviewed-by: Ian Lance Taylor <iant@golang.org>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Auto-Submit: Ian Lance Taylor <iant@golang.org>

              
                (cherry picked from commit 4f25c33e83df34f27e939f40109c27831e508b09)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/863886

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                Auto-Submit: Mark Freeman <mark@golang.org>

              
                

              
            

              
                [release-branch.go1.27] internal/poll: fix synchronous WSASendMsg byte counts on Windows

              
                

              
                CL 685436 dropped the byte-count output pointer from WSASendMsg.

              
                Restore it so execIO returns the number of bytes sent when a send

              
                completes synchronously without a completion-port notification.

              
                

              
                Fixes #82338

              
                Updates #82335

              
                

              
                Change-Id: I80bece5de469da46e9c56d430f10abeff14c2589

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/850627

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                (cherry picked from commit e9466ba2220484c40c1a3f655bbbc3ce2adc3478)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855442

              
                TryBot-Bypass: Mark Freeman <mark@golang.org>

              
                Reviewed-by: Mark Freeman <mark@golang.org>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                

              
            

              
                [release-branch.go1.27] runtime: crash if a Windows exception is dispatched below the goroutine stack

              
                

              
                Windows delivers exceptions on the faulting goroutine's own stack, and the

              
                stackSystem reserve below the stack guard exists to hold that OS-written

              
                frame. If the reserve is too small the kernel writes past stack.lo and

              
                silently corrupts the neighboring goroutine's stack, surfacing later as

              
                bogus tracebacks on an unrelated goroutine (see #70288).

              
                

              
                Check in sigtrampgo that the caller SP is still within a user

              
                goroutine's stack; if not, throw immediately instead of running on

              
                corrupted memory.

              
                

              
                For #82978.

              
                

              
                Change-Id: I2b1f1003ced3a61dda1a692614407eed90e25baf

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/824724

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                (cherry picked from commit d58290fefd4a6d0f68b291d598af908654221389)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/863173

              
                TryBot-Bypass: Mark Freeman <mark@golang.org>

              
                Reviewed-by: Mark Freeman <mark@golang.org>

              
                

              
            

              
                [release-branch.go1.27] runtime: fix g0 check in racecallbackthunk on amd64

              
                

              
                racecallbackthunk is meant to skip the g switch when it is already

              
                running on g0. Since CL 289196, which moved g into R14, it compares

              
                m (R13) instead of g (R14) against m->g0, so the check is always

              
                false. Every callback then takes the switch path and, on return, sets

              
                g to m->curg.

              
                

              
                When a race is detected on g0 while m->curg is nil, for example in a

              
                channel timer's sendTime run by the scheduler from findRunnable, the

              
                first symbolization callback leaves g set to nil. The next callback

              
                then dereferences the nil g and the process dies with SIGSEGV instead

              
                of printing the race report.

              
                

              
                Compare g with g0, as the other architectures do.

              
                

              
                Fixes #82784

              
                

              
                Change-Id: Ib4aea8652c77ef70318ca17dcdb403e66a6a6964

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/857721

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                (cherry picked from commit d3051490a57d80df0502bea0006c52c265b8e49e)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/864568

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                TryBot-Bypass: Mark Freeman <mark@golang.org>

              
                

              
            

              
                [release-branch.go1.27] net: preserve IPv6 zone in Resolver.LookupNetIP

              
                

              
                Fixes #82552.

              
                

              
                Change-Id: I3ec7f5217a590cce689e61367c1c4cf94e506f06

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/835285

              
                Reviewed-by: David Chase <drchase@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Auto-Submit: David Chase <drchase@google.com>

              
                (cherry picked from commit a87c0aebd57403f7b6b7064dc8784006d10d4e6b)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855181

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                Reviewed-by: Mark Freeman <mark@golang.org>

              
                TryBot-Bypass: Mark Freeman <mark@golang.org>

              
                

              
            

              
                [release-branch.go1.27] encoding/json: fix UnsupportedValueError.Value for cycles

              
                

              
                CL 824384 populated UnsupportedValueError.Value for NaN and ±Inf,

              
                but it is still invalid when a cycle is detected.

              
                Populate it with the value where the cycle was detected,

              
                as Go 1.26 did.

              
                

              
                Fixes #82108

              
                

              
                Change-Id: I7552f5c9c22444dee3d39f8af974e25c343eaaee

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/846965

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Auto-Submit: Dmitri Shuralyov <dmitshur@google.com>

              
                Auto-Submit: Damien Neil <dneil@google.com>

              
                Reviewed-by: Joseph Tsai <joetsai@digital-static.net>

              
                (cherry picked from commit ce3f2cc3d68a2d764bca81a3216197a8545127db)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855861

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                TryBot-Bypass: Dmitri Shuralyov <dmitshur@golang.org>

              
                

              
            

              
                [release-branch.go1.27] encoding/json: fix Indent with a non-space prefix or indent

              
                

              
                When the prefix or indent contains characters that v2 rejects,

              
                the v1 emulation formats with placeholder spaces of the same length

              
                and rewrites them with the original strings afterwards.

              
                

              
                The rewrite ran in a defer, so it ran after the trailing whitespace of

              
                src had already been appended and overwrote that whitespace as if it

              
                were indentation; with an empty indent it looped forever.

              
                Move the logic before the trailing whitespace is appended

              
                and avoid a defer.

              
                Also, add a test to both v1 and v1in2 to codify this behavior.

              
                

              
                This only modifies code that is compiled in under goexperiment.jsonv2.

              
                

              
                Fixes #82107

              
                

              
                Change-Id: I7312f2c1ea170142e24bc853cd16c83e243c6013

              
                GitHub-Last-Rev: ef1869aaee841e31b48aa4bdfa77336106cb7a6e

              
                GitHub-Pull-Request: golang/go#81557

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/833524

              
                Reviewed-by: Joseph Tsai <joetsai@digital-static.net>

              
                Reviewed-by: Carlos Amedee <carlos@golang.org>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Auto-Submit: David Chase <drchase@google.com>

              
                (cherry picked from commit 9a9a007bb7bff8e41ed95d3cccb2273a705c2718)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855841

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                TryBot-Bypass: Dmitri Shuralyov <dmitshur@golang.org>

              
                

              
            

              
                [release-branch.go1.27] encoding/json: document `embed` tag option

              
                

              
                The `embed` tag option is implicitly supported in v1

              
                by virtue of it being implemented under-the-hood using v2.

              
                As such, document support for it.

              
                

              
                Updates #82806

              
                Fixes #82809

              
                

              
                Change-Id: I2dbb97b9f3c0ee9b01a93a33993cde5a859359f1

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/824364

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                (cherry picked from commit 165bc6e7b86782c04ed7475199fe093b2f0f22fc)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/858021

              
                TryBot-Bypass: Mark Freeman <mark@golang.org>

              
                

              
            

              
                [release-branch.go1.27] html/template: document safe uses of JS interpolation

              
                

              
                Document an explicit list of safe uses of interpolation into JS,

              
                and mark everything else as out of scope for the security model.

              
                

              
                Also drop a mention of the long-removed jstmpllitinterp GODEBUG,

              
                which now appears as a confusing non-sequitur in the security model

              
                section.

              
                

              
                For #82854

              
                Fixes #82886

              
                

              
                Change-Id: I349de4d2eeec841e409b12fc68e863a86a6a6964

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/821740

              
                Reviewed-by: Neal Patel <neal@golang.org>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/860241

              
                TryBot-Bypass: Damien Neil <dneil@google.com>

              
                Reviewed-by: Mark Freeman <mark@golang.org>

              
                

              
            

              
                [release-branch.go1.27] cmd/internal/fuzztest: limit GOMAXPROCS for 386 subtests

              
                

              
                Running the fuzzer with GOMAXPROCS set too high causes the tests

              
                to run out of addesss space.

              
                

              
                For #65434

              
                Fixes #82703

              
                

              
                Change-Id: Id9e408a68667bad8a2cd65c5dd0e259e6a6a6964

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/853207

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@golang.org>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Michael Matloob <matloob@google.com>

              
                (cherry picked from commit 026b12efdfbc88b0a51dc3ddabb005b833819ad9)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/856161

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                

              
            

              
                [release-branch.go1.27] go1.27.4

              
                

              
                Change-Id: Ida742520c659821a11ea3ede01983feb265dec69

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861709

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                

              
            

              
                [release-branch.go1.27] net/http: prevent HTTP/1 server crash due to MaxBytesReader header race

              
                

              
                For HTTP/1 servers, when MaxBytesReader's limit is exceeded,

              
                "Connection: close" header is automatically set without synchronization,

              
                which might race with another header being set explicitly in the server

              
                handler. As a result, an HTTP/1 server that utilizes MaxBytesReader or

              
                MaxBytesHandler can crash due to runtime panic if its handler reads the

              
                request body concurrently with writing the response header.

              
                

              
                This affects use cases such as running httputil.ReverseProxy (which

              
                forwards a request body while copying headers from an early upstream

              
                response) with MaxBytesHandler.

              
                

              
                Thank you to Jakub Ciolek (https://ciolek.dev) for reporting this issue.

              
                

              
                Fixes CVE-2026-97054

              
                For golang/go#82563

              
                Fixes golang/go#82565

              
                

              
                Change-Id: I66db78624184d4ea795acc7abf2d96026a6a6964

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6480

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6820

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861495

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/go, cmd/compile: unquote //go:cgo_ldflag and bound slice removal

              
                

              
                Handle backslash-escaped quotes in pragmaFields and unquote cgo pragma

              
                strings with strconv.Unquote in both cmd/compile/internal/noder and

              
                cmd/go/internal/work/exec.go so flags quoted with strconv.Quote in

              
                _cgo_gotypes.go round-trip identically. Previously, the quoting mismatch

              
                between cmd/cgo, cmd/go, and cmd/compile allowed crafted #cgo LDFLAGS

              
                arguments containing escaped quotes to bypass checkLinkerFlags validation

              
                in cmd/go and cause unexpected code execution at link time.

              
                

              
                Also bound the outer loop in processCgoOutputs (i+len(LDFLAGS) <=

              
                len(flags)) to prevent an out-of-range slice panic when a prefix of

              
                runCgoProvider.LDFLAGS matches near the end of flags.

              
                

              
                Fixes CVE-2026-97043

              
                For golang/go#82567

              
                Fixes golang/go#82580

              
                

              
                Change-Id: If913a9257db2c82db4b5cdf1fc6aff2c943decb5

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6090

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6862

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861494

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/cgo, cmd/compile, cmd/link: validate cgo_import_dynamic library names

              
                

              
                Fix safeArg in cmd/compile/internal/noder to inspect every character of

              
                the library name (allowing '+' for C++ libraries like libstdc++.so.6)

              
                and reject ".." path traversal, and enforce safeArg in cgo-generated

              
                files on Windows. Also validate PE imported DLL names in cmd/cgo

              
                dynimport and cmd/link peimporteddlls so crafted DLL names cannot emit

              
                -l../../../../path/to/payload flags to the external linker.

              
                

              
                Fixes CVE-2026-97042

              
                For golang/go#82568

              
                Fixes golang/go#82582

              
                

              
                Change-Id: I25c1e8eeb3cd38864853a927ede3aaaaea8df67f

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6089

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6861

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861493

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/compile, cmd/link: validate cgo symbol names and prevent export overwrite

              
                

              
                Reject commas in //go:cgo_import_dynamic, //go:cgo_export_dynamic,

              
                //go:cgo_export_static, //go:cgo_import_static, and

              
                //go:cgo_dynamic_linker in both cmd/compile and cmd/link so they cannot

              
                split -Wl,--export-dynamic-symbol= or -Wl,--dynamic-linker= into

              
                arbitrary host linker flags, and pass -Wl,--dynamic-linker=%s rather

              
                than -Wl,--dynamic-linker,%s so interpreter paths cannot be treated as

              
                separate linker flags or @response files.

              
                

              
                Also prevent cgo_import_dynamic from overwriting SymExtname on symbols

              
                already marked for cgo export, and clear SDYNIMPORT when a cgo export

              
                overrides a prior cgo_import_dynamic without a library.

              
                

              
                Fixes CVE-2026-97041

              
                For golang/go#82569

              
                Fixes golang/go#82584

              
                

              
                Change-Id: I27d0ec60c5cbe0d7a282bc1615628c3525f069df

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6088

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6902

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861492

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/go, cmd/link: reject .syso files with Go object or unrecognized formats

              
                

              
                In ldobj, require .syso files to be recognized host object formats (ELF,

              
                Mach-O, PE, or XCOFF), and preserve the .syso suffix in packInternal and

              
                cmd/internal/archive.exactly16Bytes when truncating archive member names

              
                longer than 16 bytes. Previously, a .syso file starting with a "go

              
                object" header was parsed by ldpkg, allowing arbitrary $$ // cgo JSON

              
                directives (such as cgo_ldflag or cgo_dynamic_linker) to be injected

              
                without cgo or security.go validation, and unrecognized .syso formats

              
                set unknownObjFormat to force external linking in pure-Go builds.

              
                

              
                Fixes CVE-2026-97040

              
                For golang/go#82570

              
                Fixes golang/go#82586

              
                

              
                Change-Id: Ib800ea94e9ee4227a68942c9237f7175bb21fd5a

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6087

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6901

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861491

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/go: disallow pkg-config local .pc paths and inspect cgoCPPFLAGS

              
                

              
                Remove --with-path= and --personality= from validPkgConfigFlags while

              
                preserving --define-variable= and --prefix-variable= (whose outputs are

              
                validated by checkCompilerFlags and checkLinkerFlags), and disallow

              
                leading '=' or '$' in --define-variable= values so system .pc files

              
                cannot be rewritten to emit sysroot-prefixed -L= or -L$SYSROOT paths.

              
                

              
                In getPkgConfigFlags, inspect comma- and whitespace-separated module

              
                tokens in pkg-config package arguments and reject names ending in .pc

              
                (with or without version comparison suffixes) or arguments containing

              
                path separators, drive colons, or '..' so pkg-config cannot load

              
                repo-local .pc files directly from p.Dir. Also include cgoCPPFLAGS

              
                (which receives pkg-config --cflags) in

              
                flagsNotCompatibleWithInternalLinking.

              
                

              
                Fixes CVE-2026-97039

              
                For golang/go#82571

              
                Fixes golang/go#82588

              
                

              
                Change-Id: I12739135c713a731fb8a96694227fe6709cfcd81

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6086

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6842

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861490

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/go: validate .syso and linker input files against scripts and embedded options

              
                

              
                Verify that .syso files, direct linker inputs (.a, .o, .obj, .dll,

              
                .dylib, .lib, .so, .tbd), and libraries in -L directories (including

              
                symlinked subdirectories and sysroot-prefixed -L= and -L$SYSROOT paths)

              
                have valid binary object/archive headers rather than GNU ld implicit

              
                linker scripts (such as INPUT, GROUP, OUTPUT, or PLUGIN directives), and

              
                disallow -l:<filename> and :, =, or path separators (/ or \) in

              
                -l<namespec> so -L directories cannot load arbitrary filenames or

              
                unindexed subdirectories as scripts.

              
                

              
                Also reject GCC spec files in -L search trees (including multilib

              
                sibling directories before and after symlink resolution), recursively

              
                inspect ar archive members and universal Mach-O slices (requiring symbol

              
                table pseudo-members __.SYMDEF and __.SYMDEF SORTED at member index 0 and

              
                / at leading linker member indices with exact BSD #1/<len> name matching,

              
                allowing PE short import members, and rejecting truncated archives), reject COFF objects containing unallowlisted

              
                .drectve directives, reject Mach-O objects containing LC_LINKER_OPTION

              
                load commands, and reject ELF, PE/COFF, and Mach-O objects containing

              
                GCC or LLVM LTO IR sections (.gnu.lto_*, .gnu.debuglto_*, .llvm.lto*,

              
                .llvmbc, .llvmcmd, SHT_LLVM_LTO).

              
                

              
                Fixes CVE-2026-97038

              
                For golang/go#82572

              
                Fixes golang/go#82590

              
                

              
                Change-Id: Ia1fe15d70114aa44dcf22948b60b7b8169c92a02

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6085

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6900

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861489

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/go: validate split -Wl, next-arg flags and linker script targets

              
                

              
                In checkFlags, check strings.HasPrefix(arg, "-Wl,") before the generic

              
                load.SafeArg(list[i+1]) branch so split -Wl, flags in

              
                validLinkerFlagsWithNextArg require a -Wl,<SafeArg> second token (while

              
                continuing to allow bare <SafeArg> for "-Wl,-R" for backwards

              
                compatibility), and exclude bare -Wl,--warn-backrefs-exclude in

              
                invalidLinkerFlags so lld cannot consume the following linker token.

              
                

              
                In checkLinkerFlags, validate 1-token, 2-token, and -Wl,--push-state

              
                forms of -framework, -Wl,-framework, -Wl,-undefined, -Wl,--just-symbols,

              
                -Wl,-R (including -Wl,-R=<path>), -Wl,-subsystem, and -Wl,-sectcreate so

              
                they cannot pass linker script files to the external linker.

              
                

              
                Fixes CVE-2026-97037

              
                For golang/go#82573

              
                Fixes golang/go#82592

              
                

              
                Change-Id: I26f97c785ac420a999f77080b6c5e95268720a2e

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6084

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6882

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861488

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                

              
            

              
                [release-branch.go1.27] cmd/go: disallow separated -l flag followed by to_library

              
                

              
                We included "-l" in validLinkerFlagsWithNextArg. On macOS passing "-l

              
                to_library" would be recombined into "-lto_library" and interpreted with

              
                the next argument, which we include in invalidLinkerFlags because it

              
                could then cause arbitrary plugins to be loaded. Also exclude

              
                -lazy_library, -lazy_framework, -load_hidden, -library, and

              
                -library-path in invalidLinkerFlags, and check recombined "-l"

              
                arguments against invalidLinkerFlags and disallow patterns.

              
                

              
                Fixes CVE-2026-97036

              
                For golang/go#82574

              
                Fixes golang/go#82594

              
                

              
                Change-Id: I6ccca7bc34f0a41c73a2774b53c00954da95ff91

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6083

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6841

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861487
