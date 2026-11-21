---
id: collect-261121-veille-ia/actu-business/runtime-security-updates-rebuilding-for-the-node-js-and-go-fixes-of-11-november-2026
title: "Runtime security updates: rebuilding for the Node.js and Go fixes of 11 November 2026"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-11-12"]
keywords: ["node", "http", "server", "fixes", "runtime", "edge", "security", "november"]
source: https://viweb.net/blog/runtime-security-updates-nodejs-go-2026-11-12/
source_anchor: ""
source_lines: [1, 59]
section: "Actu_Business"
---
Two runtimes behind many business websites shipped security releases on 11 November 2026. Node.js published 22.23.5, 24.22.3 and 26.13.1. Go published 1.26.11 and 1.27.4. No exploitation was established in the sources we reviewed, which is not the same as either project affirming that none exists. Several fixes are rated High and reachable from the network, while most apply only under specific configurations. Prioritize promptly by which features your site actually exposes and what the worst case would mean for it, rather than by one fixed timeline for every case. The real work is confirming which runtime actually serves your customers, since the version on a developer laptop rarely matches production.

## Node.js: sixteen fixes, each with conditions

The upstream announcement lists sixteen issues, the highest rated High, clustered around the HTTP/2 server, HTTPS parsing, TLS handshakes and client certificates. Three representative items show how conditional they are.

CVE-2026-123864 affects Node’s own HTTPS server. If a handler flushes headers before the request body has finished and later ends the response with data, a client can make the server copy memory from other connections, possibly including TLS private keys, into a request body that an ordinary echo or storage route then reveals. Cleartext node:http is not affected.

CVE-2026-123865 lets an HTTP/2 client leave a session that never cleans up, leaking memory and file descriptors until restart. Upstream calls it reliable against default HTTP/2 servers.

CVE-2026-123872 lets a client authenticated for one hostname renegotiate TLS 1.2 under another hostname and bypass a stricter client-certificate policy there. It only matters where one Node server applies different client-certificate rules per hostname.

All three are listed for 22.x, 24.x and 26.x. The umbrella lists several HTTP/2 items for 24.x and 26.x only, while the 22.23.5 notes also carry those fixes. Take them without reading that as proof 22.x was exposed. Per-issue conditions are in our Node.js briefing.

## Go: one fix for running services, eleven for build pipelines

CVE-2026-97054, tracked as GO-2026-6859, is a crash in the HTTP/1 server. It needs a handler that limits request bodies with MaxBytesReader or MaxBytesHandler, a body over the limit, and concurrent body reading and response-header writing. The process then panics and exits. Affected: 1.26 before 1.26.11 and 1.27 before 1.27.4. Details are in our Go briefing.

The same release carries eleven build-time fixes in the Go toolchain, compiler and linker. They cover malicious packages and build inputs, and not all of them need cgo: some involve directives in plain Go source files or object files bundled in a pure-Go package. They protect build machines that compile untrusted code. They are not remote code execution in a running service, whatever a hurried summary may say.

Go compiles the standard library into every binary. Updating Go on the build server changes nothing until each service is rebuilt, redeployed and restarted.

## A worked hypothetical: TLS at the edge, plain HTTP behind it

Picture a small shop whose site runs as a Node container. A load balancer or reverse proxy terminates TLS and forwards plain HTTP/1.1 to the container. This layout is very common, so walk it through.

1. **The HTTPS issue.** CVE-2026-123864 requires Node’s HTTPS server to handle TLS. Here the edge does, and Node listens on cleartext node:http, so that issue is not reachable by this path. The edge’s own TLS software has a separate update stream that still needs checking.
2. **The HTTP/2 issues.** These depend on whether Node itself runs an http2 server. If the edge speaks HTTP/1.1 to an Express-style node:http backend, they are not reachable. If the backend uses node:http2, several trigger over cleartext and the edge does not shield it.
3. **Update the runtime anyway.** Outbound https.Agent reuse, proxy error messages, zlib edge cases and Permission Model issues depend on application code, and a later architecture change can wake a dormant path. A controlled deployment of the patched release, with the previous artifact kept for rollback, settles the runtime question. Reviewing which features your application actually uses then tells you which issues could ever have applied, and both steps are worth doing.
4. **Do not patch inside a vendor image.** Swapping the node binary in a vendor-built container creates something the vendor never tested and that vanishes on the next pull. If the image is your own build, rebuild it from an updated base image. If it is a vendor-supplied image, move to the vendor’s updated supported tag or follow the vendor’s documented build procedure, rather than rebuilding it yourself as a substitute.
5. **Verify from the running process.** A health endpoint that reports the runtime version, or the running image digest matching the rebuilt one, is evidence. A changed Dockerfile is an intention.

## Go handoff: rebuild, accept, keep the old binary

If a contractor maintains a Go service for you, the handoff can be short.

- Ask which toolchain built the binary currently serving traffic. Go embeds this in the binary, and the CI log of the rebuild is good supporting evidence.
- Keep the previous artifact and its deployment definition so rollback is a redeploy, not a rebuild.
- Acceptance runs in authorized staging or the normal regression suite: the service restarts cleanly, an upload larger than the configured limit sent through the ordinary business flow is refused with the expected response while the process stays up, and error rates stay flat. That is bounded evidence the deployment works, not proof the race can never fire.
- A service that never uses MaxBytesReader still gets rebuilt. It just is not urgent.

## Evidence of completion

- The running Node process reports 22.23.5, 24.22.3 or 26.13.1 from inside the deployed container or host.
- The rebuilt image digest or build ID is recorded for each service.
- The deployed Go binary carries toolchain 1.26.11 or 1.27.4, and the previous artifact is retained.
- The edge proxy’s TLS update status is noted separately.
- Memory and restart counts stay flat after deployment; see monitoring beyond uptime.
- A restore point was confirmed before the deploy; see our backup strategy guide.

## Questions for your hosting or platform provider

- Which Node major runs our site, and has your platform image moved to the patched release?
- Does TLS terminate at your edge or inside our Node process?
- Does our backend speak HTTP/2 to anything, including internal callers?
- For Go services you build for us, which toolchain produced the artifact now serving traffic?
- How is rollback performed, and is the pre-update artifact retained?

None of this proves a site was exposed, and finishing the list does not prove it was not. It does turn two upstream releases into routine maintenance with acceptance you can check.
