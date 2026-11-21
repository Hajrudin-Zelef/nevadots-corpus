---
id: collect-261121-veille-ia/langages-dev/jdk-27-g1-garbage-collector-changes
title: "JDK 27 G1 Garbage Collector Changes"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["collector", "java", "serial", "services", "changes", "available", "small", "garbage"]
source: https://dzone.com/articles/jdk-g1-garbage-collector
source_anchor: ""
source_lines: [1, 183]
section: "Langages_Dev"
---
# Java 27 Makes G1 the Default Everywhere: How to Test Your Small Containers Before You Upgrade

If a container has 1 CPU or under 1,792 MB, Java 27 silently switches it from Serial GC to G1. Test those services before you upgrade.

When JDK 27 was released on 15 September 2026, most of the attention went to compact object headers and post-quantum TLS. JEP 523 received less coverage, partly because its description sounds like housekeeping: G1 becomes the default garbage collector in all environments.

For most applications, that changes nothing. For a specific class of container, it changes the collector silently, with no warning in the logs and no failure at startup.

This article covers what the fallback did, which workloads lose it, how to identify affected services, and how to run a comparison that produces usable numbers rather than noise.

## What the Serial GC Fallback Did

G1 has been the nominal default since Java 9. What is less widely known is that HotSpot inspected the machine at startup and overrode that choice in constrained environments.

The JVM selected Serial GC when either condition was true:

The reasoning was sound at the time. Serial GC is single-threaded with a very small native memory footprint. On a single-core machine, a concurrent collector spends time competing with application threads for the only available core, and the bookkeeping structures G1 maintains were expensive relative to a small heap.

G1 has improved substantially since then. Its native memory footprint has been reduced across several releases, and JDK 26 delivered JEP 522, which removed synchronization overhead from G1's write barriers by introducing a second card table.

JEP 523 concludes that work. The fallback is removed, and G1 is now selected in every environment regardless of CPU count or available memory.

Serial GC remains fully supported and can still be selected explicitly:


`java -XX:+UseSerialGC -jar application.jar`
Applications that already specify a collector, whether G1, ZGC, Parallel, or Shenandoah, are unaffected by this change.

## Which Workloads Are Affected

The change only reaches services that never had an explicit collector flag and that run in a constrained environment. In practice, this describes a predictable set of deployments:

- **Sidecar containers** . Log shippers, metrics agents, and service mesh components typically receive a fraction of a CPU and a few hundred megabytes of memory. They are rarely tuned.
- **Batch and queue workers** . Short-lived jobs scheduled onto whatever capacity is available.
- **Low-traffic internal services** . Admin APIs, configuration services, and internal tooling that are deliberately given small pods.
- **CI runners and development containers** . Environments where JVM flags are seldom set because the workload is not production.
- **Pods with fractional CPU limits** . This case is easy to miss. A container configured with`cpu: 500m` may report a single available processor to the JVM, which placed it in Serial GC territory on earlier releases.

These are the workloads least likely to appear in a performance test plan, which is precisely why the change deserves deliberate verification.

## Step 1: Identify Your Current Collector

Before changing anything, determine which of your services are actually running Serial GC today. The `GarbageCollectorMXBean` interface exposes this directly:


```
import java.lang.management.GarbageCollectorMXBean;
import java.lang.management.ManagementFactory;
public final class RuntimeFacts {
    public static void main(String[] args) {
        Runtime rt = Runtime.getRuntime();
        System.out.printf("java version   : %s%n", Runtime.version());
        System.out.printf("available cpus : %d%n", rt.availableProcessors());
        System.out.printf("max heap MB    : %,d%n",
                rt.maxMemory() / (1024 * 1024));
        for (GarbageCollectorMXBean gc :
                ManagementFactory.getGarbageCollectorMXBeans()) {
            System.out.printf("collector      : %s%n", gc.getName());
        }
    }
}
```
The collector names identify which implementation is active:

For a Spring Boot application, registering this as a startup listener puts the information into your existing log aggregation without any additional infrastructure:


```
import org.springframework.boot.context.event.ApplicationReadyEvent;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;
import java.lang.management.GarbageCollectorMXBean;
import java.lang.management.ManagementFactory;
import java.util.stream.Collectors;
@Component
public class GcFactsLogger {
    @EventListener(ApplicationReadyEvent.class)
    public void logCollector() {
        Runtime rt = Runtime.getRuntime();
        String collectors = ManagementFactory.getGarbageCollectorMXBeans()
                .stream()
                .map(GarbageCollectorMXBean::getName)
                .collect(Collectors.joining(", "));
        System.out.printf(
                "JVM %s | cpus=%d | heap=%,dMB | gc=%s%n",
                Runtime.version(),
                rt.availableProcessors(),
                rt.maxMemory() / (1024 * 1024),
                collectors);
    }
}
```
Deploy this across your estate, then filter your logs for entries reporting `Copy` as a collector name. That result set defines the scope of your testing.

## Step 2: Understand the Native Memory Difference

Discussions about garbage collector selection usually center on pause times. For this particular change, native memory consumption is the more consequential metric.

G1 maintains bookkeeping structures that Serial GC does not require, including remembered sets, card tables, and per-region metadata. This memory is allocated outside the Java heap, which means it counts against a container memory limit without appearing in heap metrics.

Measurements published after the JDK 27 release place G1's native memory overhead at approximately 43 MB, compared with roughly 1.7 MB for Serial GC. Ankur Mundra measured both collectors on Spring Boot 4.1 within real cgroup limits and reported these figures.

The absolute difference is modest. The proportional impact depends entirely on container size:

On a tightly sized container, this overhead is consumed before the application allocates its first object. Where memory limits were set with minimal headroom, the change alone can produce OOM kills after an upgrade that altered nothing else.

Native Memory Tracking provides the per-category breakdown if you want to confirm the figure in your own environment:


```
java -XX:NativeMemoryTracking=summary \
     -XX:+UnlockDiagnosticVMOptions \
     -jar application.jar
```
Then, against the running process:


`jcmd <pid> VM.native_memory summary`
The `GC` section of the output reports committed native memory attributable to the collector.

## Step 3: Run a Controlled Comparison

A meaningful comparison requires the smallest container size you actually deploy, representative load, and a run long enough to reach steady state. A thirty-second run measures startup behavior rather than collection behavior.

Capture GC logs for both configurations:


```
# Pre-Java 27 behaviour
java -XX:+UseSerialGC \
     -Xlog:gc*:file=serial.log:time,uptime,level,tags \
     -jar application.jar
# Java 27 default
java -Xlog:gc*:file=g1.log:time,uptime,level,tags \
     -jar application.jar
```
Three measurements matter, and the third is the one most often omitted:

- **Total pause time** . Sum the pause durations across the run rather than comparing individual collections. Serial GC performs fewer, longer pauses; G1 performs more, shorter ones. Only the aggregate is comparable.
- **Tail latency** . G1 runs concurrent phases. On a single-core container, those phases contend with request handling for the only available processor. This effect is invisible in a mean and clearly visible at p99.
- **Resident set size under load** . This is where the native memory difference becomes operationally relevant. Sample RSS throughout the run rather than reading it once at startup:


```
while true; do
  ps -o rss= -p "$PID" | awk '{printf "%s %d MB\n", strftime("%H:%M:%S"), $1/1024}'
  sleep 5
done
```
If RSS approaches your container limit under G1 where it did not under Serial, you have found the problem before it found you.

## Step 4: Pin the Collector When G1 Loses

If measurement shows that G1 performs worse on a constrained workload, the appropriate response is not to defer the JDK upgrade. It is to state the collector explicitly:


`java -XX:+UseSerialGC -jar application.jar`
This is a supported configuration. Serial GC is neither deprecated nor scheduled for removal, and JEP 523 explicitly preserves it.

In a Kubernetes deployment, express the choice where it will be reviewed:


```
spec:
  containers:
    - name: batch-worker
      env:
        - name: JAVA_TOOL_OPTIONS
          value: "-XX:+UseSerialGC"
```
There is an argument for setting the flag explicitly even where G1 performs well. An explicit collector choice is self-documenting. A future engineer reading the manifest can see that a decision was made and tested, rather than inferring what the JVM selected at runtime.

## Conclusion

JEP 523 is a reasonable change. G1 has genuinely closed the gap that justified the fallback, and removing a hidden conditional from the JVM's startup logic simplifies the mental model for everyone.

The operational consideration is narrower: a set of small, rarely tested containers will change collectors on upgrade without any signal that it happened. Identifying those containers takes one log statement. Testing them takes one load run. The cost of skipping both is a latency regression or an OOM kill that appears unrelated to any change your team made.

Java 27 is not a long-term support release, and many teams will remain on Java 25. That does not remove the relevance of this work. The same default will arrive with the next LTS, and evaluating it on a short-term release carries considerably less risk than discovering it mid-migration.

## **Further Reading**

Opinions expressed by DZone contributors are their own.
