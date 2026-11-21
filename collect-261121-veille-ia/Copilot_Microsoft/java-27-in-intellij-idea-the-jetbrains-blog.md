---
id: collect-261121-veille-ia/copilot-microsoft/java-27-in-intellij-idea-the-jetbrains-blog
title: "Java 27 in IntelliJ IDEA - The JetBrains Blog"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-09-15"]
keywords: ["intellij", "idea", "java", "features", "preview", "project", "using", "download"]
source: https://blog.jetbrains.com/?p=739827/
source_anchor: ""
source_lines: [1, 139]
section: "Copilot_Microsoft"
---
# Java 27 in IntelliJ IDEA

In line with the six-month release cadence, Java 27 was released on September 15, 2026. In this blog post, we will look at what’s included in this release, how IntelliJ IDEA continues to support the latest Java versions, and even how IntelliJ IDEA is benefiting from some of the changes to the JDK.

Java 27 delivers nine JEPs, four of which are final. In addition, four preview features continue to mature and one JEP continues as an incubator feature.

Even if you’re not using IntelliJ IDEA, you can still get its intelligence elsewhere: *IntelliJ IDEA Goes LSP: Java and Kotlin Intelligence Comes to VS Code, Cursor, and Agentic Flows*. This provides the same level of support for the latest Java versions and features, including early access versions and preview features.

For this blog post, let’s start by setting up IntelliJ IDEA to use the latest Java version.

# Using Java 27 in IntelliJ IDEA (setup)

First, you will need to download JDK 27. There are several ways to do so; you can download it from inside IntelliJ IDEA, using tools like SDKMAN! or asdf, or install it yourself.

## Downloading the JDK

To download JDK 27 from IntelliJ IDEA, open the *Project Structure,* go to the tab *Project Settings | Project*, open the drop-down menu in the *SDK* field, and select *Download JDK*. In the *Download JDK* popup that opens, set *Version* to *27*, and in the *Vendor* field, select the vendor you want to use.

If you have already installed the JDK, you can point IntelliJ IDEA to your installation. Instead, select *Add JDK from Disk* from the drop-down menu.

The JDK version will be automatically propagated from your project settings to the terminal, which means that both you and your agent can use the right JDK for your project on the terminal in IntelliJ IDEA. A demo of this feature was shown in the What’s New in IntelliJ IDEA 2026.2 video.

If you use SDKMAN! or asdf to manage your JDKs, IntelliJ IDEA can read the `.sdkmanrc` or `.tool-versions` file in your project and can configure the JDK automatically if you already have it installed.

If you’re using SDKMAN! and the JDK version specified in the `.sdkmanrc` file is not yet installed, an inlay hint will appear in the file, allowing you to download it directly from the IDE by running the relevant SDKMAN! command. This even works if you don’t have SDKMAN! installed, which means this functionality is also available for Windows users.

For more information on how to set up the JDK, see the documentation.

## Setting the language level

IntelliJ IDEA supports Java 27 from day one. In the *Project Structure*, set the *Language level* to *27 – No new language features*.

If you’re using preview features, set it to *27 (Preview) – Primitive types in patterns, instanceof, and switch (5th preview).* Note that IntelliJ IDEA will highlight the usage of preview features in the editor, so that you are aware of exactly where you’re using preview features.

# New stable features in Java 27

Java 27 introduced 4 new stable features. Let’s take a quick look.

The Garbage-First (G1) garbage collector has been the default garbage collector since Java 9, with one exception: Machines with only one CPU or less than 1,792 MB of RAM fell back to Serial GC. This JEP makes G1 the default garbage collector for the HotSpot JVM in all environments, not just server environments, replacing the Serial collector.

Improvements were made to the G1 garbage collector (including JEP 522: G1 GC: Improve Throughput by Reducing Synchronization in Java 26) so that its performance is now sufficient to replace Serial. For applications in constrained environments that perform better with Serial, it can be explicitly enabled with the option `-XX:+UseSerialGC`.

This JEP enhances the security of Java applications that require secure network communication by implementing hybrid key exchange algorithms for TLS 1.3. This helps guard against “harvest now, decrypt later” attacks, where an adversary records encrypted traffic today with the intention of decrypting it once quantum computers are powerful enough.

This JEP makes compact object headers the default object header layout in the HotSpot JVM.

By reducing object headers from 96 bits to 64 bits on 64-bit architectures, compact object headers reduce heap size, improve deployment density, and increase data locality.

Compact object headers were introduced as an experimental feature in Java 24 and became opt-in in Java 25. Since compact object headers are now the default, you no longer need to enable them using a command line option, like with JDK 25.

Applications with large numbers of objects will benefit from this change, as using compact object headers reduces the JVM memory footprint. JetBrains IDEs are using this feature and have observed consistent memory savings with it enabled. Learn more.

This JEP enhances the JDK Flight Recorder (JFR) by redacting command-line arguments and the initial values of environment variables and system properties in recordings before it leaves the process, so sensitive information does not leak.

# Preview features in Java 27

Preview features give developers a chance to try new features and provide feedback before they become final. Preview features might go through multiple rounds, either with or without changes, before they are finalized and become a standard part of the language.

IntelliJ IDEA strives to support preview features, so you can experiment with them before they become standard. Because these features are always subject to change, IntelliJ IDEA only supports preview features for the current JDK. To enable preview features, set *Language l**evel* to *27 (Preview) – Primitive types in patterns, instanceof, and switch (5th preview).*

Java 27 contains four preview features and one incubator feature. The features themselves are the same as in Java 26, but some of them have been revised in Java 27.

Lazy constants are objects that hold immutable data. As the name suggests, they are evaluated lazily. This offers advantages over final fields, which are evaluated eagerly. With lazy constants, fields are evaluated only when needed, which can improve startup time.

For a slightly more detailed explanation, see the section on *Lazy Constants in Java 26 in IntelliJ IDEA*.

Lazy constants were first introduced in Java 25 (as stable values) and underwent significant changes in Java 26. The API has been revised further in Java 27.

As described in *Java 26 in IntelliJ IDEA*, this feature enhances pattern matching to support all primitive types in pattern contexts, including `instanceof` and `switch`. The feature removes some of the existing limitations of pattern matching, making it possible to match, test, and safely convert primitive values in these constructs. Removing the need for unsafe manual casts (which might cause subtle bugs) and range checks leads to improved code safety and readability. This feature is still in preview in Java 27, with no changes from the Java 26 version.

### A quick example

Let’s look at a quick example of using primitive types in switch expressions with guard patterns.

```
int itemDiscountPercentage(int items) {
   return switch (items) {
       case 2 -> 5;
       case 3, 4 -> 10;
       case int i when i >= 5 -> 20;
       default -> 0;
   };
}
```
We can add Javadoc to this method to explain how it works. As of Java 23 (JEP 467), Javadoc can be written in Markdown. If your project uses JDK 23 or later, IntelliJ IDEA will insert Markdown documentation comments by default when you use the context action *Add Javadoc*.

Sidenote: If you prefer to use traditional comments instead of Markdown, open *Settings*, go to *Editor | Code Style | Java | Code Generation | Comment Code*, and deselect the *Prefer documentation line comments* option.

IntelliJ IDEA can convert existing Javadoc to markdown using the context action *Convert to Markdown documentation comment*.

You can find a more detailed explanation of this feature in this section of *Java 23 and IntelliJ IDEA*.

As described in *Java 26 in IntelliJ IDEA*, structured concurrency gives us better “idioms” for multithreaded code, which means the code more closely matches the user’s mental model. 

Structured concurrency treats groups of related tasks running in different threads as single units of work. This improves readability and observability, making concurrent code easier to understand and reason about. It also helps eliminate thread leaks and cancellation delays.

For more details on structured concurrency and how it works together with other features from project Loom, please see my previous blog post, *Project Loom in IntelliJ IDEA: Virtual Threads, Scoped Values, and Structured Concurrency*.

As mentioned in that blog post, there is a live template for quickly scaffolding a `StructuredTaskScope`. You can pull up a list of available live templates in IntelliJ IDEA using the shortcut (*⌘J* on macOS / *Ctrl+J* on Windows and Linux). Alternatively, you can use command completion: Type .. – and a list of all commands relevant to your current context will appear. You can even type part of the name of the command (for example “live” for Show live templates), and the relevant command will show up as part of regular completion. 

The list of live templates is searchable, both by name and description. If we search for “structured”, we will find the live template `sts`.

For more information on command completion, see my previous blog post, *Universal Entry Point: A Single Entry Point for Context-Aware Coding Assistance*.

As described in *Java 26 in IntelliJ IDEA*, this JEP provides a preview API for encoding and decoding cryptographic objects (keys, certificates, and certificate revocation lists) to and from the Privacy-Enhanced Mail (PEM) format.

The API has undergone further changes in Java 27 compared to the previous version.

# Incubator features in Java 27

Incubator modules are experimental APIs. They are explicitly non-final APIs and tools, made available for developers to try out. Unlike preview features, incubator features may change significantly, or even be removed in future releases. They are not intended for production use.

To use an incubator feature, you’ll need to explicitly add the module while you run the code, by adding the `--add-modules` command-line option for that specific module.

If you are running the code from IntelliJ IDEA, you can add command line options to the run configurations for your application. Select the run configuration for your program and click the three dots for *More Actions*. Select *Edit* to open the *Run/Debug Configurations*. Click the link *Modify Options*, and in the *Add Run Options* popup, select *Add VM Options*. A new field will appear with the hint VM Options. Add the command line option to that field.

As described in *Java 26 in IntelliJ IDEA*, this JEP introduces an API that allows developers to express vector computations that reliably compile to optimal vector instructions on supported CPUs. Vector operations enable more work to be performed in a single CPU cycle, which can result in significant performance gains.

As this is an incubator module, you’ll have to provide a command line option to add the module explicitly, as described above. To use the Vector API, add the following command line option: `--add-modules jdk.incubator.vector`.

There are no changes to the Vector API in Java 27, compared to Java 26. This feature will continue to incubate until the required features of Project Valhalla become available as preview features.

# Other changes: Debugger improvements

Other than JEPs, JDK 27 contains bug fixes, like JDK-6960970. The debugger was very slow during stepping. This has been fixed in JDK 27, which means that stepping through the debugger is faster now. Our engineers have backported this fix to JetBrains Runtime (JBR) 25, so they can use it when stepping during debugging the code for IntelliJ IDEA itself. This shows that it is possible to backport this change to Java 25 and that the change works correctly. If you want to take advantage of this change, you can use JBR 25 or ask your vendor to backport this change.

# Looking ahead

Features from Project Valhalla have started making their way into Java. Both value objects (JEP 401) and strict field initialization in the JVM (JEP 539) have been targeted to JDK 28 as preview features. This means that if you want to try out these features, you no longer have to download a special Valhalla distribution. You can download the JDK 28 EA (Early Access) version. This version is available for download from inside IntelliJ IDEA.

If you have already downloaded JDK 28 EA, you can point IntelliJ IDEA to your installation as well.

# Conclusion

While not an LTS release, Java 27 brings security and performance improvements and continues to evolve several important preview features. IntelliJ IDEA supports Java 27 from day one and will continue to support the latest Java features. Please let us know if you have any feedback.

*Prev post*Debug Past an HTTP 403 Without Breaking Spring SecurityIntelliJ IDEA 2026.2.3 Is Out!

*Next post*
