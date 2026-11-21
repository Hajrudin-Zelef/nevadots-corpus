---
id: collect-261121-veille-ia/securite/x-vuln-go1-27-by-go-commit-2
title: "x/vuln (go1.27) by go commit"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["spec", "golang", "flag", "reviewed-by", "google", "linker", "such", "argument"]
source: https://ci.chromium.org/p/golang/g/x-vuln-go1.27-by-go/console
source_anchor: ""
source_lines: [1501, 3000]
section: "Securite"
---
              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                

              
            

              
                [release-branch.go1.27] cmd/go: disallow -gen-* and -gnat* in flag allowlists

              
                

              
                Exclude Clang's -gen-* (such as the 2-argument -gen-cdb-fragment-path

              
                <dir> flag) and GCC's -gnat* (such as the 2-argument -gnatO <file> flag)

              
                from matching the 1-argument -g* wildcard in validCompilerFlags and

              
                validLinkerFlags. Allowing these flags permitted writing sidecar output

              
                files and desynchronized flag arity between checkFlags and the compiler

              
                driver, allowing subsequent arguments (such as a linker script after a

              
                2-argument flag like -arch or -L) to be passed as positional inputs to

              
                the linker and cause unexpected code execution at build time.

              
                

              
                Fixes CVE-2026-97035

              
                For golang/go#82575

              
                Fixes golang/go#82596

              
                

              
                Change-Id: I334aee8d66f96db3abe1dfb722cb69b5cb4585ae

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6082

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6881

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861486

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                

              
            

              
                [release-branch.go1.27] cmd/go: reject GCC spec files in linker search directories

              
                

              
                Keep -fsanitize=, -fopenmp, and -static-lib* in the compiler and linker

              
                flag allowlists, and instead prevent GCC from loading untrusted specs or

              
                *.spec files (such as libgomp.spec or libsanitizer.spec) from -L,

              
                -isysroot, and -Wl,-syslibroot directories (including subdirectories,

              
                symlinked subdirectories, sysroot-prefixed -L= and -L$SYSROOT paths, and

              
                GCC multilib sibling directories ../lib, ../lib32, ../lib64, and

              
                ../libx32 both before and after resolving symlinks).

              
                

              
                When linker flags that cause GCC's *link_command spec to evaluate

              
                %:include(libgomp.spec) (-fopenmp or -fopenacc) or

              
                %:include(libsanitizer.spec) (-fsanitize=* combined with -static or

              
                -static-lib*) are present, also check the current working directory and

              
                package directory for spec files, while allowing standalone -fsanitize=*,

              
                -fno-openmp, -fopenmp-simd, and -static. Also tighten -fsanitize= in

              
                validCompilerFlags to reject @ and - prefixes.

              
                

              
                Fixes CVE-2026-97034

              
                For golang/go#82576

              
                Fixes golang/go#82598

              
                

              
                Change-Id: I1878e1b9eae7c2dee4a6dbc96ff11e17c3dac527

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6081

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6880

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861485

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/go: disallow --sysroot and -target, validate -isysroot/-syslibroot, and restrict -x

              
                

              
                Remove --sysroot and -target from the compiler and linker flag

              
                allowlists, as GCC and Clang use --sysroot to relocate subprograms, spec

              
                files, and CRT startup objects, and Clang's -target flag falls back to

              
                executing target helper tools (such as orbis-ld, prospero-lld, xcc, or

              
                llvm-spirv) from the current working directory when not found in PATH.

              
                

              
                Keep -isysroot and -Wl,-syslibroot for SDK selection, but verify in

              
                checkLinkerFlags that their target is not a regular file on disk (which

              
                GNU ld.bfd treats as a positional linker script).

              
                

              
                Also restrict -x to the C/C++/Objective-C/assembler languages supported

              
                by cgo.

              
                

              
                Fixes CVE-2026-97033

              
                For golang/go#82577

              
                Fixes golang/go#82600

              
                

              
                Change-Id: I6e85fe46f26bb6366e03e17f8761e7c884694874

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6080

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6860

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/861484

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                

              
            

              
                [release-branch.go1.27] runtime: match deferring function when looking for linked defers in nextFrame

              
                

              
                In _panic.nextFrame, searching for the frame containing linked defers

              
                (limit = gp._defer.sp) only compared u.frame.sp == limit. On

              
                architectures where leaf calls do not adjust the stack pointer or in

              
                the presence of frameless leaf wrappers, a callee wrapper shares the

              
                exact same SP as its caller. When panicking inside such a wrapper,

              
                nextFrame prematurely broke on the wrapper frame. Because the frameless

              
                wrapper has no saved link register on the stack, subsequent traceback

              
                initialization from p.pc failed with "fatal error: traceback did not

              
                unwind completely".

              
                

              
                Require u.frame.fn == findfunc(d.pc) in addition to u.frame.sp == limit

              
                before stopping at a frame with linked defers. This ensures nextFrame

              
                skips frameless wrappers and unwinds to the actual function that

              
                registered the defer.

              
                

              
                Updates #81959

              
                Fixes #82657

              
                

              
                Change-Id: Ic9c63b8133041968044062a5983f8519a94dafb1

              
                GitHub-Last-Rev: e855c3ae381903090a9ccbc77201c9bdd5440b32

              
                GitHub-Pull-Request: golang/go#81989

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/844565

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Carlos Amedee <carlos@golang.org>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                Auto-Submit: Carlos Amedee <carlos@golang.org>

              
                (cherry picked from commit 02e6c32a4b58ae78639adc34692e86e2ea14247c)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/854949

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                TryBot-Bypass: David Chase <drchase@google.com>

              
                TryBot-Bypass: Junyang Shao <shaojunyang@google.com>

              
                

              
            

              
                [release-branch.go1.27] crypto/tls: omit ML-DSA from signature_algorithms_cert

              
                

              
                Some non-compliant middleboxes reject ML-DSA in

              
                signature_algorithms_cert due to rules targeting an old OpenSSL DoS

              
                vulnerability, causing connection failures.

              
                

              
                Restore the Go 1.26 contents of signature_algorithms_cert while

              
                retaining ML-DSA in signature_algorithms.

              
                

              
                Fixes #82536

              
                Updates #81199

              
                

              
                Change-Id: Ib0563c4925a913d7422ee1ad84ed25473ee1a4a7

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/854185

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                Reviewed-by: Neal Patel <neal@golang.org>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                Reviewed-by: Filippo Valsorda <filippo@golang.org>

              
                TryBot-Bypass: Junyang Shao <shaojunyang@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/compile: record method indices before encoding a named type

              
                

              
                CL 826105 added explicit method indices to unified export data (V5)

              
                so that importers can restore a named type's source method order.

              
                But the encoder only recorded a type's method indices after encoding

              
                its underlying type, and encoding the underlying type can encode one

              
                of the type's own generic methods first: if a field's type has a

              
                method whose body calls the generic method, encoding that body

              
                encodes the generic method object, which then looks up its index

              
                before it was recorded and gets 0. Importers then sort it among the

              
                type's first methods, so objectpath again attributes analysis facts

              
                to the wrong methods (crashing staticcheck as in #81188).

              
                

              
                Record the indices before encoding anything else.

              
                

              
                [Note: I manually edited the cherrypick to avoid a dependency

              
                on the too-new testimporter package. --adonovan.]

              
                

              
                Updates #81188

              
                Updates #82105

              
                Fixes #82106

              
                

              
                Change-Id: Icc8cfdd5620bb6d50b1c0b296f325c7d99ee9cc5

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847627

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Alan Donovan <adonovan@google.com>

              
                Auto-Submit: Alan Donovan <adonovan@google.com>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855582

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                Auto-Submit: Junyang Shao <shaojunyang@google.com>

              
                TryBot-Bypass: Junyang Shao <shaojunyang@google.com>

              
                

              
            

              
                [release-branch.go1.27] crypto/internal/fips140/sha3: record legacy Keccak as non-approved

              
                

              
                The legacy Keccak hashes share the Digest type with SHA-3, and

              
                Digest.Sum recorded them as approved. HMAC and RSA-PSS/OAEP accept any

              
                *sha3.Digest and rely on the hash recording its own indicator, so they

              
                were recorded as approved over Keccak, too.

              
                

              
                NewLegacyKeccak256 and NewLegacyKeccak512 are not used outside of

              
                tests, so no public API was affected.

              
                

              
                For #82625

              
                Fixes #82627

              
                

              
                Change-Id: I34c2cf54ba12073a690b93012075c06e6a6a6964

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855105

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                TryBot-Bypass: Junyang Shao <shaojunyang@google.com>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                

              
            

              
                [release-branch.go1.27] crypto/internal/fips140/tls12: record SHA-384 as approved in MasterSecret

              
                

              
                The SHA-384 output size is 48 bytes, not 46, so the extended master

              
                secret derivation with SHA-384 was recorded as non-approved.

              
                

              
                Updates #79228

              
                For #82625

              
                For #82627

              
                

              
                Change-Id: I781be024b999fdc832a28d8964241b1d6a6a6964

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855104

              
                TryBot-Bypass: Junyang Shao <shaojunyang@google.com>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                

              
            

              
                [release-branch.go1.27] crypto/hkdf: remove unnecessary HMAC key length allowance for HKDF-Expand

              
                

              
                For #82625

              
                For #82627

              
                

              
                Change-Id: I11c0ff2a8be4f88e59bf139c4bb42bec6a6a6964

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855103

              
                Reviewed-by: David Chase <drchase@google.com>

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                TryBot-Bypass: Junyang Shao <shaojunyang@google.com>

              
                

              
            

              
                [release-branch.go1.27] crypto/rsa: apply FIPS 140-3 approved checks to MGF hash in OAEP

              
                

              
                For #82625

              
                For #82627

              
                

              
                Change-Id: I3dcbd54a4f2a360a4300f7e8cc31d9196a6a6964

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/831726

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Daniel McCarney <daniel@binaryparadox.net>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                (cherry picked from commit e9a2cefe3670ea20947e71fe535f7e5accc71f9b)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855102

              
                TryBot-Bypass: Junyang Shao <shaojunyang@google.com>

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                

              
            

              
                [release-branch.go1.27] crypto/internal/fips140: run CASTs when generating keys

              
                

              
                Arguably, the PCTs are sufficient on these paths, but be conservative.

              
                

              
                It's free anyway, assuming the key will be used shortly after.

              
                

              
                For #82625

              
                For #82627

              
                

              
                Change-Id: If19efa7d9d1beb0e49b6f6803a492ed06a6a6964

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/855101

              
                TryBot-Bypass: Junyang Shao <shaojunyang@google.com>

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                

              
            

              
                [release-branch.go1.27] net: fix zero value address handling in Dialer.Dial{TCP,UDP,IP}

              
                

              
                Fixes #82023

              
                

              
                Change-Id: I4f1698fa924f71d791c842cfbad9915ad7c5932b

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/835805

              
                Auto-Submit: Damien Neil <dneil@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                (cherry picked from commit 0b6b8381b6a8190c585cd454c747a501a5bfec7b)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/852025

              
                Reviewed-by: David Chase <drchase@google.com>

              
                TryBot-Bypass: David Chase <drchase@google.com>

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/compile: teach deadstore about CondSelect

              
                

              
                DeadStore never used to have to think about CondSelect since

              
                before CL 685676 and CL 715044 it were impossible for CondSelect

              
                values to exists when deadstore ran.

              
                

              
                Now it might so tell deadstore what to do about them.

              
                

              
                Updates #81863

              
                Fixes #81866

              
                

              
                Change-Id: Id1c37da97137bd7091f169a9169ee04e7eb07f87

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/841425

              
                Auto-Submit: Jorropo <jorropo.pgm@gmail.com>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                Reviewed-by: PJ Malloy <pjmalloy@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Daniel Morsing <daniel.morsing@gmail.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/845968

              
                TryBot-Bypass: David Chase <drchase@google.com>

              
                Reviewed-by: Junyang Shao <shaojunyang@google.com>

              
                

              
            

              
                [release-branch.go1.27] Revert "runtime: fix timespec definition on 32bits systems"

              
                

              
                This reverts commit ba402cd756a02cd80bcd76a2f7afc22ae2041c6c.

              
                

              
                I was looking at the wrong linux timespec struct (internal vs UAPI).

              
                

              
                Updates #82147

              
                Fixes #82150

              
                

              
                Change-Id: I773f7413d5b7a16fae707502409ea0e784c4e69d

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/848207

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/internal/obj/x86: clear forward-jump list on PCALIGN

              
                

              
                CL 696915 moved the PCALIGN case in span6 below forward-jump

              
                processing but left it ahead of the p.Rel = nil reset. When a

              
                short branch needs widening, span6 reassembles and pushes the

              
                same jump onto the stale list again, making q.Forwd == q and

              
                looping forever.

              
                

              
                Clear p.Rel for every Prog right after its forward jumps are

              
                patched.

              
                

              
                For #81792

              
                Fixes #81927

              
                

              
                Change-Id: Ief88bc5350a95a71487a1dc51dc2309c6252336e

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/839925

              
                Reviewed-by: guangyuan zhou <zhouguangyuan@golangcn.org>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Cherry Mui <cherryyz@google.com>

              
                (cherry picked from commit 27903a87177570c03b827b4985a8522da1e89bd3)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/842345

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Reviewed-by: Liz Fong-Jones <lizf@honeycomb.io>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@golang.org>

              
                

              
            

              
                [release-branch.go1.27] cmd/dist: fix GOFIPS140 toolchain compilation

              
                

              
                Go 1.26 started rejecting purego+GOFIPS140 builds, so it won't build a

              
                toolchain1 if GOFIPS140 is set (which is intended to apply only to the

              
                final tools and as the default for builds produced with that toolchain).

              
                

              
                Updates #81335

              
                Fixes #82334

              
                

              
                Change-Id: I27cf27848b7612d00ba3ade4c259acb46a6a6964

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/827164

              
                Reviewed-by: Daniel McCarney <daniel@binaryparadox.net>

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Roland Shoemaker <roland@golang.org>

              
                Reviewed-by: Mark Freeman <mark@golang.org>

              
                Auto-Submit: Filippo Valsorda <filippo@golang.org>

              
                Reviewed-by: Daniel Morsing <daniel.morsing@gmail.com>

              
                (cherry picked from commit 2e7619c3ceb133d5f395a844f0305370ba14c56c)

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/850645

              
                Reviewed-by: Carlos Amedee <carlos@golang.org>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                

              
            

              
                [release-branch.go1.27] go1.27.3

              
                

              
                Change-Id: Icf51c3e5809b43e3137f6e269b1196ec9f74626e

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/851146

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Reviewed-by: Carlos Amedee <carlos@golang.org>

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                

              
            

              
                [release-branch.go1.27] crypto/x509: distrust a handful of intermediates

              
                

              
                Filter a set of intermediates from chains returned by calls to Verify.

              
                These chains are identified via SHA-256 SPKI hashes.

              
                

              
                Fixes CVE-2026-111943

              
                For golang/go#82345

              
                Fixes golang/go#82349

              
                

              
                Change-Id: I43172dd50560eb7d1a80bc2554a67234d3029501

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6380

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-by: David Chase <drchase@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6400

              
                Reviewed-by: Roland Shoemaker <bracewell@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/851165

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Carlos Amedee <carlos@golang.org>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                

              
            

              
                [release-branch.go1.27] go1.27.2

              
                

              
                Change-Id: Ic0b65cb1b99dfa414ec1c8b11360114db21b7530

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/846986

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                

              
            

              
                [release-branch.go1.27] net/http: apply header limits to Trailer headers too

              
                

              
                When "Trailer" headers are sent by a client, the HTTP server internally

              
                uses the header values to populate the Request.Trailer map passed to the

              
                server handler. Because Request.Trailer is a map, each entry incurs

              
                memory overhead. For HTTP/2 servers, a malicious client can exploit this

              
                by sending a "Trailer" header that declares a large number of fields,

              
                causing the server to allocate a disproportionate amount of memory while

              
                bypassing Server.MaxHeaderValueCount and Server.MaxHeaderBytes limits.

              
                This exploit is not applicable for HTTP/1 servers, which do not support

              
                multiplexing a large number of requests over one TCP connection, and

              
                whose Server.MaxHeaderBytes is calculated differently.

              
                

              
                Fix this by applying MaxHeaderValueCount to the trailer fields declared

              
                in the "Trailer" header in both HTTP/1 and HTTP/2, and MaxHeaderBytes in

              
                HTTP/2.

              
                

              
                Thank you to RyotaK (https://ryotak.net) of GMO Flatt Security Inc. for

              
                reporting this issue.

              
                

              
                Fixes CVE-2026-78659

              
                For #81857

              
                Fixes #81918

              
                

              
                Change-Id: I01f52e9873be80ac72d88f9e4d96f6096a6a6964

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/5980

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847034

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Auto-Submit: Dmitri Shuralyov <dmitshur@google.com>

              
                

              
            

              
                [release-branch.go1.27] net/http/internal/http2: prevent crash due to HPACK encoder race

              
                

              
                Our HTTP/2 server could end up crashing due to inadvertently

              
                modifying its HPACK encoder concurrently. This happens because the

              
                server modifies the HPACK encoder from two goroutines without

              
                synchronization: one uses the encoder to encode a HEADERS frame as part

              
                of a response sent to a client and the other modifies the encoder's

              
                table size when handling a SETTINGS frame containing

              
                SETTINGS_HEADER_TABLE_SIZE that a client sends.

              
                

              
                As a result, a malicious client can repeatedly send a request while

              
                changing the header table size to crash the server.

              
                

              
                Fix this issue by not applying SETTINGS_HEADER_TABLE_SIZE immediately.

              
                Instead, buffer any SETTINGS_HEADER_TABLE_SIZE received, and only apply

              
                the new value prior to the next time the server writes a frame.

              
                

              
                Thank you to RyotaK (https://ryotak.net) of GMO Flatt Security Inc. for

              
                reporting this issue.

              
                

              
                Fixes CVE-2026-97032

              
                For #81867

              
                Fixes #81922

              
                

              
                Change-Id: Ife977865a8bb190ae2cf6f462979ae406a6a6964

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/5940

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847033

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Auto-Submit: Dmitri Shuralyov <dmitshur@google.com>

              
                

              
            

              
                [release-branch.go1.27] net/http/internal/http2: avoid 2x refund on stream close with buffered data

              
                

              
                Fix the following bug:

              
                

              
                  - client opens a stream and sends data

              
                  - client resets the stream, and server refunds flow control

              
                  - handler reads the sent data, and server refunds flow control again

              
                

              
                This double-refund allows a malicious client to exceed the server's

              
                flow control limits, potentially causing the server to buffer much

              
                more data than those limits should allow.

              
                

              
                Thanks to Ali Sherif (https://www.linkedin.com/in/ali-sherif-13812b276/)

              
                for reporting this issue.

              
                

              
                Fixes CVE-2026-78663

              
                Fixes #81906

              
                For #81743

              
                

              
                Change-Id: I86159053f7ce509d1ee93de55697f7ef6a6a6964
