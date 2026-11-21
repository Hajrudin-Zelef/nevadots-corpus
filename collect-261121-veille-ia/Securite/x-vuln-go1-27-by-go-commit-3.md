---
id: collect-261121-veille-ia/securite/x-vuln-go1-27-by-go-commit-3
title: "x/vuln (go1.27) by go commit"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["window", "initial", "stream", "reviewed-by", "google", "work", "flow", "control"]
source: https://ci.chromium.org/p/golang/g/x-vuln-go1.27-by-go/console
source_anchor: ""
source_lines: [3001, 3891]
section: "Securite"
---
              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6020

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847032

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                

              
            

              
                [release-branch.go1.27] net/http/internal/http2: avoid excessive work on initial window changes

              
                

              
                Defend against a malicious peer sending many adjustments to the initial

              
                flow control window.

              
                

              
                The flow control window for a stream is:

              
                  initial_window + sum(window_updates) - sum(bytes_sent)

              
                

              
                The initial window starts at 64k, but may be adjusted at any time

              
                by a SETTINGS frame.

              
                

              
                A decrease in the initial window might cause a stream's current

              
                window to become negative, which is fine.  (This happens when a

              
                stream has already consumed some portion of its existing window.)

              
                

              
                An increase in the initial window MUST NOT cause the window of

              
                any stream to exceed the maximum of (2^31-1). RFC 9113 requires

              
                us to check for this:

              
                

              
                	An endpoint MUST treat a change to SETTINGS_INITIAL_WINDOW_SIZE

              
                	that causes any flow-control window to exceed the maximum size as

              
                	a connection error (Section 5.4.1) of type FLOW_CONTROL_ERROR.

              
                

              
                We enforced this by iterating over every active stream to adjust its

              
                window and verify that the new window is not too large. While iterating

              
                over streams is O(N), a malicious peer could send many small SETTINGS frames

              
                and cause a large amount of CPU consumption when the number of open

              
                streams is high.

              
                

              
                The new implementation enforces the window less strictly, but performs

              
                O(1) work in response to a change in the initial window.

              
                Individual streams now maintain a delta from the initial window:

              
                

              
                  delta = sum(window_updates) - sum(bytes_sent)

              
                

              
                The current stream window is computed on-demand by adding the

              
                current initial window to the delta.

              
                

              
                Overflow is detected lazily, when trying to write data for a stream

              
                or when processing a WINDOW_UPDATE. This is technically speaking

              
                a violation of RFC 9113's requirements, but it's a harmless one

              
                and ensures we never do excessive work in reaction to a change

              
                in the initial window.

              
                

              
                Alternatives considered:

              
                

              
                - Maintain a heap of streams, sorted by current window size.

              
                  Too complicated. Too much overhead. The juice is not worth the squeeze.

              
                

              
                - Maintain a high-water mark for the stream delta, and only do an O(N)

              
                  iteration over all streams when the high-water mark indicates that

              
                  overflow might have happened. Avoids expensive processing in most

              
                  cases, but is vulnerable to a peer sending a rapid sequence of:

              
                

              
                    create stream

              
                    update stream window (updating the high-water mark)

              
                    reset stream

              
                    increase initial window to cause iteration (no violation)

              
                    decrease initial window

              
                

              
                  This scenario is more work than just a sequence of changes to the

              
                  initial window, and there are possibly ways to defend against it,

              
                  but lazy overflow detection is simpler and extremely robust.

              
                

              
                Thanks to Jakub Ciolek (https://ciolek.dev) for reporting this issue.

              
                

              
                Fixes CVE-2026-78669

              
                Fixes #81904

              
                For #81742

              
                

              
                Change-Id: I5667b01f81427141a0d873e2ec47052a6a6a6964

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/5921

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847031

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                

              
            

              
                [release-branch.go1.27] net/http: limit the size of range headers parsed in ServeContent

              
                

              
                Set a limit on the number of ranges in a Range header,

              
                avoiding excessive allocations/CPU when parsing a header

              
                containing a large number of small ranges.

              
                

              
                The limit is controlled by GODEBUG=httpservecontentmaxranges.

              
                The default is 200 (same as Apache).

              
                GODEBUG=httpservecontentmaxranges=0 disables the limit.

              
                

              
                Thanks to Jakub Ciolek (https://ciolek.dev) for reporting this issue.

              
                

              
                Fixes CVE-2026-78667

              
                For golang/go#81858

              
                Fixes golang/go#81920

              
                

              
                Change-Id: Id7df8059e2a406e3a401bb5cb767b4d16a6a6964

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/5560

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6220

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847030

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                

              
            

              
                [release-branch.go1.27] os: correct Root junction handling on Windows (avoid mkdir escape)

              
                

              
                Avoid an escape from the root on Windows when the target of Root.Mkdir

              
                or Root.MkdirAll is a dangling junction. This would incorrectly create

              
                a new directory at the target of the junction, even when this is a

              
                location outside of the root.

              
                

              
                Also fix a more minor issue where various operations on a junction

              
                would fail when the target name ended in a trailing slash. For example,

              
                Root.Lstat("target/") would return ENOTDIR when "target" is a junction,

              
                instead of following the junction or returning a path-escapes error.

              
                

              
                Thanks to Daniele Ballarini for reporting this issue.

              
                

              
                Fixes CVE-2026-56857

              
                For golang/go#81739

              
                Fixes golang/go#81898

              
                

              
                Change-Id: If92a6b425ff33af6fb05c94b0d3d98e26a6a6964

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/5580

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6200

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847029

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                

              
            

              
                [release-branch.go1.27] net/http: avoid server reuse of connection after CONNECT

              
                

              
                When a server HTTP handler responds to an HTTP/1 CONNECT request

              
                with a 2xx response and returns without taking ownership of (hijacking)

              
                the connection, the server incorrectly continues to read requests

              
                from the connection.

              
                

              
                Since a 2xx response to a CONNECT converts the connection into

              
                a tunnel, this can result in parser misalignment between the net/http

              
                server and intermediate proxies, and a potential request smuggling

              
                vector.

              
                

              
                There are a number of problems in net/http's handling of CONNECT

              
                requests. This CL is a minimal change to avoid exploitable misbehavior

              
                in the server.

              
                

              
                The HTTP/1 server now always closes a connection after handling a

              
                CONNECT request, if the handler did not hijack the connection.

              
                

              
                Thanks to Jakub Ciolek (https://ciolek.dev) for reporting this issue.

              
                

              
                Fixes CVE-2026-94439

              
                For golang/go#81744

              
                Fixes golang/go#81908

              
                

              
                Change-Id: I260f23ca8c8fbe51abb2bdfef5afb2d96a6a6964

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/5641

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6142

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847028

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                

              
            

              
                [release-branch.go1.27] net/http, net/http/httputil: avoid post-CONNECT pool corruption

              
                

              
                When the HTTP/1 transport sends a CONNECT request, it sends the

              
                Request.Body along with the headers.

              
                

              
                CONNECT requests do not contain data.

              
                

              
                If the server accepts the CONNECT, the Request.Body becomes the

              
                outbound tunneled data. If the server rejects the CONNECT, however,

              
                the Request.Body becomes the start of a new request on the connection.

              
                The transport improperly returns the connection to the pool. If the

              
                data on the connection is a valid request, the next request sent

              
                on the connection will read the response to that prior request.

              
                

              
                There are a number of problems in net/http's handling of CONNECT

              
                requests. This CL is a minimal change to avoid exploitable misbehavior

              
                in the transport.

              
                

              
                  - Transport closes a connection after making a CONNECT request,

              
                    regardless of the response it gets.

              
                  - ReverseProxy rejects CONNECT requests with 405 Method Not Allowed.

              
                

              
                Thanks to Xclow3n (Rajat Raghav) for reporting this issue.

              
                

              
                Fixes CVE-2026-56866

              
                For golang/go#81740

              
                Fixes golang/go#81900

              
                

              
                Change-Id: Ie101b9345a9c4b3525476c0627fa0b736a6a6964

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/5640

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6180

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847027

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                

              
            

              
                [release-branch.go1.27] net/textproto: enforce MIME memory limit on first line

              
                

              
                readMIMEHeader subtracted 400 bytes from its maxMemory parameter,

              
                for overhead accounting, but did not check to see if this caused

              
                maxMemory to become negative. When called with a small maxMemory

              
                limit, this disabled the memory limit (<0: unlimited) for the

              
                first line read.

              
                

              
                Drop the 400 byte adjustment entirely, as serving no useful

              
                purpose: If the caller wants to deduct some amount per MIMEHeader,

              
                they may do so.

              
                

              
                Thanks to Jakub Ciolek (https://ciolek.dev) for reporting this issue.

              
                

              
                Fixes CVE-2026-94440

              
                For golang/go#81741

              
                Fixes golang/go#81902

              
                

              
                Change-Id: I0f42d331b2355346b07d1b9987a488996a6a6964

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/5680

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6141

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847026

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                

              
            

              
                [release-branch.go1.27] crypto/tls: validate ECH outer extension references

              
                

              
                Using a single cursor, reject malformed encodings

              
                and esnure that each outer extension is copied at

              
                most once.

              
                

              
                Fixes CVE-2026-97031

              
                For golang/go#81855

              
                Fixes golang/go#81916

              
                

              
                Change-Id: I5b09f2b6275d42f7701fae4e583eb3a2caeaa44d

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/5320

              
                Reviewed-by: Roland Shoemaker <bracewell@google.com>

              
                Reviewed-by: Nicholas Husin <husin@google.com>

              
                Reviewed-on: https://go-internal-review.googlesource.com/c/go/+/6140

              
                Reviewed-by: Damien Neil <dneil@google.com>

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/847025

              
                Reviewed-by: Dmitri Shuralyov <dmitshur@google.com>

              
                Auto-Submit: Gopher Robot <gobot@golang.org>

              
                TryBot-Bypass: Gopher Robot <gobot@golang.org>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>

              
                

              
            

              
                [release-branch.go1.27] cmd/go: always verify toolchain downloads with sumdb

              
                

              
                Instead of allowing a matching entry

              
                that specifies golang.org/toolchain,

              
                always go to the network to obtain

              
                the canonical checksum.

              
                

              
                Fixes CVE-2026-94447

              
                For golang/go#81834

              
                Fixes golang/go#81914

              
                

              
                Change-Id: I5bffb80f7f18e3c1ac7b5e7ef027a4acf1f2a7e1

              
                Reviewed-on: https://go-review.googlesource.com/c/go/+/843587

              
                LUCI-TryBot-Result: golang-scoped@luci-project-accounts.iam.gserviceaccount.com <golang-scoped@luci-project-accounts.iam.gserviceaccount.com>

              
                Reviewed-by: Neal Patel <nealpatel@google.com>

              
                Reviewed-by: Michael Pratt <mpratt@google.com>
