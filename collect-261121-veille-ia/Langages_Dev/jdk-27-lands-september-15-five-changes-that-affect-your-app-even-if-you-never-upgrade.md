---
id: collect-261121-veille-ia/langages-dev/jdk-27-lands-september-15-five-changes-that-affect-your-app-even-if-you-never-upgrade
title: "JDK 27 Lands September 15. Five Changes That Affect Your App Even If You Never Upgrade"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["heap", "changes", "upgrade", "object", "default", "never", "small", "september"]
source: https://dev.to/jamilxt/jdk-27-lands-september-15-five-changes-that-affect-your-app-even-if-you-never-upgrade-10h
source_anchor: ""
source_lines: [1, 66]
section: "Langages_Dev"
---
Next Tuesday, September 15, JDK 27 hits general availability. If you run Spring Boot in production, your first instinct is probably to file this under "not my problem, we are on an LTS."

Here is the problem with that instinct. Three of the nine features in JDK 27 change default behavior of the JVM itself. The garbage collector your app gets if you never specify one. The object header layout every single object in your heap uses. The key exchange your TLS handshakes negotiate. Defaults are how the platform changes underneath you, whether you upgrade or not. Your clients, your build agents, your cloud base images will move before your servers do.

I have spent six years shipping Spring Boot services, and every JDK upgrade I have ever done followed the same pattern: the code migrated fine, and something in the "invisible defaults" layer broke or improved things nobody expected. So instead of another feature list, here is the checklist I would run this week, before the release even lands, based on the official JEPs.

Everything below comes from the official JDK 27 project page and the individual JEPs. JDK 27 is not an LTS release. It is a six-month feature drop. The point of this checklist is not to rush an upgrade. It is to make sure that when your team does move, likely to whatever LTS follows, none of these defaults surprise you.

**What changes:** JEP 523 makes the Garbage-First collector the default in all environments. Until now, running Java on a machine the JVM classified as non-server-class, like a small container or a CI runner, silently gave you SerialGC. Tiny heap, stop-the-world everything. G1 already worked, you just had to ask for it.

**Who this actually hits:** anyone running JVM processes on small instances. Kubernetes sidecars, Lambda-style functions, staging pods with 1 GB limits, GitHub Actions runners. Those environments often hit the non-server-class path, and a lot of teams never noticed because the workload was small anyway.

**Check this week:** run this in every environment your containers land in, not just your laptop.


```
java -Xlog:gc+heap=trace -version 2>&1 | grep "Using the"
```
Or check the `garbage collector` line in any startup log. If a config file somewhere pins `-XX:+UseSerialGC` because someone cargo-culted it for a small container in 2019, decide now whether G1 with a small heap is better for that workload. It usually is, but test it, because G1 has its own floor: region size and remembered set overhead are real costs at 256 MB heaps.

**What changes:** JEP 534 switches the HotSpot object header from 96 bits to 64 bits on 64-bit architectures, by default. Every object in your heap shrinks by 4 bytes. The JEP cites SPECjbb2015 runs using 22 percent less heap space and 8 percent less CPU, plus a 15 percent reduction in GC counts in another setting.

This one already shipped as opt-in in JDK 25, and it has been battle-tested. Amazon runs hundreds of production services on it, mostly backported to JDK 17 and 21. SAP switched their downstream OpenJDK fork to it by default. So the risk here is low. But "the entire heap gets 4 bytes denser per object" is a change with wide blast radius, and there are known interaction edges with tools that poke at object layouts.

**Who this actually hits:** services tuned to the old memory math. If you right-sized your heap for a specific live-set size, your live set just got smaller. Off-heap tools, agents, and anything doing sun.misc.Unsafe tricks on headers should be verified against your real workload.

**Check this week:** the escape hatch is one flag, `-XX:-UseCompactObjectHeaders`, so the safest path is: upgrade, run your load tests with the default, and only flip the flag off if something regresses. Also note the old layout is on a deprecation path, so "opt out forever" is not a strategy.

**What changes:** JEP 527 adds post-quantum hybrid key exchange to TLS 1.3. The JDK now combines ML-KEM, the NIST standardized quantum-resistant algorithm, with classical ECDHE. The default scheme list puts X25519MLKEM768 first, and no code change is needed. That is the point: the platform defends against the "harvest now, decrypt later" attack, where an adversary records your encrypted traffic today and decrypts it once quantum computers mature, without you doing anything.

**Who this actually hits:** mostly nobody, but verify. Your app's outbound TLS calls to modern endpoints negotiate the hybrid scheme transparently. The failure mode is the other side: an old TLS terminator, a legacy appliance, a middleware box that chokes on the larger handshake messages hybrid key exchange produces. ML-KEM-768 public keys and ciphertexts add roughly a kilobyte to the handshake.

**Check this week:** after any JDK bump, smoke-test every TLS peer that is not a public website. Internal service meshes, database drivers over TLS, that one nginx box nobody has reconfigured since 2021. If a handshake fails, the fix is documented: set the `jdk.tls.namedGroups` system property or call `SSLParameters.setNamedGroups()` to control the scheme list.

**What changes:** JEP 536 makes JDK Flight Recorder redact command-line arguments, environment variables, and system properties in recordings, before the data leaves the process. Until now, a `.jfr` file captured the exact value of `javax.net.ssl.keyStorePassword`, your `ACCESS_TOKEN` env var, or a `--dbpassword` argument in plain text. The JEP's own motivation example shows all three leaking from one recording.

I have lived this one. A few years ago our standard "attach the JFR file to the vendor ticket" workflow was one careless consultant away from shipping a database password to a third party. Nobody audited what was inside those recordings. Redaction here is on by default, with a sensible built-in filter list: patterns like `*password*`, `*token*`, `*secret*`, `*api*key*`, `*passphrase*`. Even the filter list shows care, the command-line filter deliberately drops `*auth*` because it would also match innocent flags like `--author`.

**Who this actually hits:** teams with compliance requirements, and anyone whose incident response runbook says "grab a JFR recording and share it." This feature upgrades your default security posture with zero effort.

**Check this week:** if you have custom secret naming (say, env vars named `KREDAILY_KEY` or other names the filters miss), configure the new `-XX:FlightRecorderOptions` sub-options like `redact-key` and `redact-argument` with your own patterns. And if your runbook says "scrub the JFR before sharing," you can shorten that step, but do not delete it, redaction only covers process-level startup data, not secrets that leak into other event payloads.

**What changes:** JEP 533 is the seventh preview of StructuredTaskScope. Per the JEP, this round refines how exceptions propagate out of a scope, and the InfoQ analysis of the change notes the API's shape has held stable since the fifth preview. The scope of changes keeps shrinking. That convergence is the signal: this API is close to final.

**Why it matters for your roadmap:** if you write concurrent fan-out code, parallel calls to downstream services, scatter-gather patterns, timeouts that cancel the whole unit of work, structured concurrency will eventually be the idiomatic way to write it on the JVM. seventh preview means the design risk of adopting it is now low, but you still need `--enable-preview` to use it, which means no production use yet.

**Check this week:** pick one executor-based fan-out in your codebase, the ugliest one, and prototype it with StructuredTaskScope in a JDK 27 early-access build. You are not migrating. You are building institutional familiarity so that when it finalizes, your team migrates in a sprint instead of a quarter. The same goes for Lazy Constants (third preview) and primitive patterns (fifth preview). Preview fatigue is real, but the features that survive seven previews tend to survive to final.

- 
**Garbage collector:** verify which collector each environment actually gets today with`java -Xlog:gc+heap=trace -version` , and test G1 on your smallest containers.
- 
**Object headers:** plan load tests with compact headers on by default, keep`-XX:-UseCompactObjectHeaders` as the rollback flag.
- 
**TLS:** smoke-test every non-public TLS peer after a JDK bump, know about`jdk.tls.namedGroups` before you need it.
- 
**JFR:** review your env var and property naming against the redaction filter list, add custom`redact-key` patterns if needed.
- 
**Previews:** prototype one StructuredTaskScope migration target now, with`--enable-preview` , off the production path.

And one roadmap note. JDK 28, due March 2027, is where Project Valhalla finally lands as a preview: JEP 401, value objects, is already integrated into the JDK 28 main line, and a Simple JSON API (JEP 540) is coming as an incubator. If JDK 27 is the release of better defaults, JDK 28 is shaping up to be the release of new memory-model capabilities. Worth watching, not worth rushing.

I write about Java, Spring Boot, and AI infrastructure every week. Subscribe, it is free, and you will get the next piece on whether value classes change how we model domain objects.

Do you pin your garbage collector and JVM flags explicitly in production, or do you ride the platform defaults? And have you been bitten by a silent default change before? I want to hear the story in the comments.

## Top comments (0)
