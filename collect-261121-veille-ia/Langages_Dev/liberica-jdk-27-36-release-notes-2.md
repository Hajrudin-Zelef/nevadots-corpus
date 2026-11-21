---
id: collect-261121-veille-ia/langages-dev/liberica-jdk-27-36-release-notes-2
title: "Liberica JDK 27+36: Release Notes"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["java", "test", "refactor", "tests", "httpclient", "junit", "testng", "remove"]
source: https://docs.bell-sw.com/liberica-jdk/27b36/general/release-notes/
source_anchor: ""
source_lines: [1501, 2698]
section: "Langages_Dev"
---
| JDK-8378388 | Add missing @Override annotations in "javax.print.attribute.standard" package part 1 | 
| JDK-8378389 | Remove AppContext from the Swing RepaintManager | 
| JDK-8378397 | Disable usage of system level jshell history in test/hotspot/jtreg/runtime/os/TestWXHealing.java | 
| JDK-8378398 | Modernize test/jdk/java/net/URLClassLoader/HttpTest.java | 
| JDK-8378413 | C2: Missed Ideal optimization opportunity in PhaseIterGVN for URShiftI still exists | 
| JDK-8378417 | Printing All pages results in NPE for 1.1 PrintJob | 
| JDK-8378431 | Move input validation checks to Java for java.lang.StringUTF16 intrinsics | 
| JDK-8378439 | Remove unused methods in HSpaceCounters | 
| JDK-8378440 | ASAN build fails on Windows due to missing dlfcn.h file | 
| JDK-8378442 | RBTreeCHeap does not deallocate removed nodes when using remove_at_cursor | 
| JDK-8378443 | ACVP testing should include LMS signature verification | 
| JDK-8378462 | Build fails with --enable-linktime-gc set when using some devkits/toolchains | 
| JDK-8378464 | PixelInterleavedSampleModel constructors and methods do not specify behavior when arguments are null or out of bounds | 
| JDK-8378470 | Add future Intel family number | 
| JDK-8378506 | [macOS] Window content not updated when rendering to multiple windows | 
| JDK-8378524 | Compile-time constant generated through annotation processor is not usable in repeated annotation but fails compilation | 
| JDK-8378535 | Parallel: Replace SpaceCounters with HSpaceCounters | 
| JDK-8378549 | Incorrect assert in JvmtiThreadState::update_for_pop_top_frame | 
| JDK-8378559 | Add setting of captured states like errno | 
| JDK-8378561 | Mark gc/shenandoah/compiler/TestLinkToNativeRBP.java as /native | 
| JDK-8378563 | ConnectionRefusedMessage::testFinishConnect fails when jdk.includeInExceptions contains hostInfo | 
| JDK-8378565 | Refactor test/jdk/java/net/httpclient/http3/*.java TestNG tests to JUnit | 
| JDK-8378569 | Refactor test/jdk/java/net/httpclient/http3/H3ErrorHandlingTest.java to use JUnit | 
| JDK-8378578 | Add final to XbmColormap and XbmHints in XbmImageDecoder | 
| JDK-8378583 | (process) ProcessBuilder tests that span deeper trees must pass launchMechanism to their children | 
| JDK-8378584 | (process) Investigate and fix Alpine errors in Basic.java ProcessBuilder test, and re-enable tests | 
| JDK-8378585 | Mark fields in MediaTracker final | 
| JDK-8378595 | Refactor miscellaneous tests under test/jdk/java/net/httpclient from TestNG to JUnit | 
| JDK-8378598 | Refactor tests under test/jdk/java/net/httpclient/websocket from TestNG to JUnit | 
| JDK-8378599 | Refactor tests under test/jdk/java/net/httpclient/whitebox from TestNG to JUnit | 
| JDK-8378600 | Refactor tests under test/jdk/java/net/httpclient/http2 from TestNG to JUnit | 
| JDK-8378603 | libfontmanager FFM path needs to use FFM to locate both malloc and free | 
| JDK-8378605 | Remove AppContext from the Swing TimerQueue implementation | 
| JDK-8378607 | GlyphLayout cache can prevent Fonts from being GC’d | 
| JDK-8378615 | FFM Bound up call stub keeps JNI Global Ref to bound parameter | 
| JDK-8378623 | Use unique font names in FormatCharAdvanceTest | 
| JDK-8378625 | C2: Refactor MemNode::detect_access_independence to a separate method | 
| JDK-8378631 | Update Zlib Data Compression Library to Version 1.3.2 | 
| JDK-8378641 | Test serviceability/jvmti/RedefineClasses/RedefineVerifyError.javaâ€Ž missing UnlockDiagnosticVMOptions after 8376295 | 
| JDK-8378642 | Add src/utils/LogCompilation/logc.jar to .gitignore | 
| JDK-8378657 | OutputAnalyzer should have a quiet mode | 
| JDK-8378675 | PPC64: increase instruction cache line size | 
| JDK-8378677 | Inline clear into ContiguousSpace::initialize | 
| JDK-8378684 | Fix -Wdeprecated-declarations warnings from gtest by clang23 | 
| JDK-8378687 | Improve delegation of HttpURLConnection | 
| JDK-8378698 | Optimize Base64.Encoder#encodeToString | 
| JDK-8378699 | runtime/ClassUnload/KeepAliveSoftReference.java Test failed: true != false | 
| JDK-8378702 | jdk.test.lib.Platform.isMusl() may return false negative on Alpine Linux | 
| JDK-8378713 | C2: performance regression due to missing constant folding for Math.pow() | 
| JDK-8378715 | Use early field initialization for java.lang.invoke generated code | 
| JDK-8378719 | CompiledDirectCall::set_to_interpreted() fails with guarantee(chk == -1 \|\| chk == 0) failed: Field too big for insn | 
| JDK-8378723 | Locale variant delimiter is unclear | 
| JDK-8378727 | [macOS] Missing dispatch_release for semaphores in CDesktopPeer | 
| JDK-8378731 | Move AOT-inited classes to initialized state in early VM bootstrap | 
| JDK-8378737 | AArch64: Fix SVE match rule issues for VectorMask.andNot() | 
| JDK-8378740 | Suppressed warnings reported when implicit compilation is combined with annotation processing | 
| JDK-8378742 | C2: constant folding for ModFloatingNode should be done in Value method | 
| JDK-8378744 | Obsolete NewSizeThreadIncrease flag | 
| JDK-8378746 | ZGC: jdk/jfr/event/gc/detailed/TestZRelocationSetGroupEvent.java intermittent OOME | 
| JDK-8378749 | Early GC crash with unresolved AOT FMG subgraph classes | 
| JDK-8378752 | Enable some subword vector casts IR matching tests for RISC-V | 
| JDK-8378758 | [VectorAPI] Add Jtreg scalar operation wrappers for math operators and APIs | 
| JDK-8378764 | fileStream::fileSize() fails for >2GB files on Windows | 
| JDK-8378779 | NBody demo test times out with C1 stress testing | 
| JDK-8378786 | PeerConnectionId::cloneBuffer should use absolute bulk get | 
| JDK-8378792 | ObjectMethods.bootstrap missing getter validation | 
| JDK-8378793 | Add ResolvedFieldEntry is_valid assert | 
| JDK-8378796 | java.lang.runtime bootstrap methods missing lookup validation | 
| JDK-8378798 | Test tools/jpackage/junit/share/jdk.jpackage/jdk/jpackage/internal/util/CommandOutputControlTest.java failed: missing IOException | 
| JDK-8378804 | Shenandoah: Convert remaining components to Atomic<T> | 
| JDK-8378806 | Genshen: Add scope of collection to end of cycle message in JMX notification | 
| JDK-8378808 | Refactor java/nio/Buffer TestNG tests to use JUnit | 
| JDK-8378810 | Enable missing FFM test via jtreg requires for RISC-V | 
| JDK-8378823 | AIX build fails after zlib updated by JDK-8378631 | 
| JDK-8378833 | Improve offset arithmetic in ArraysSupport::mismatch | 
| JDK-8378836 | Enable linktime-gc by default on Linux ppc64le | 
| JDK-8378838 | Fix issues with "dead" code elimination and serviceability agent in libjvm.so on Linux | 
| JDK-8378845 | Add NoSafepointVerifier to CriticalSection classes | 
| JDK-8378854 | Disable SA tests when ZGC is being used | 
| JDK-8378865 | After fix for JDK-8378385 two tests are failing on windows | 
| JDK-8378867 | jpackage references non-existing "message.app-image-requires-identifier" l10n key | 
| JDK-8378870 | Remove sun.awt.AWTAccessor from imports in ImageIcon | 
| JDK-8378871 | CPU feature flags are not properly set in vm_version_windows_aarch64.cpp | 
| JDK-8378872 | Mark waitList in FetcherInfo final | 
| JDK-8378873 | jpackage: remove macOS-specific code from jdk.jpackage.internal.ModuleInfo | 
| JDK-8378874 | jpackage: remove redundant messages | 
| JDK-8378876 | jpackage: facilitate testing with mocks | 
| JDK-8378877 | jpackage: improve rebranding of exe files on Windows | 
| JDK-8378878 | Refactor java/nio/channels/AsynchronousSocketChannel test to use JUnit | 
| JDK-8378879 | Refactor java/nio/channels/Channels TestNG tests to use JUnit | 
| JDK-8378880 | GenShen: Fix typo in recent conversion to atomic field | 
| JDK-8378883 | Enable more vector reductions IR matching tests for RISC-V | 
| JDK-8378884 | Migrate jdk/jdk/nio/zipfs tests to use JUnit | 
| JDK-8378885 | Add missing @Override annotations in "javax.print.attribute.standard" package part 2 | 
| JDK-8378888 | jdk/incubator/vector/Float16OperationsBenchmark.java uses wrong package name | 
| JDK-8378891 | Problemlist TestStaticCallStub on Windows AArch64 | 
| JDK-8378893 | X25519 should utilize a larger limb size | 
| JDK-8378894 | AOT training run crashes with jcmd VM.native_memory | 
| JDK-8378897 | assertion failure due to missing depends_only_on_test_impl definition in SqrtHFNode | 
| JDK-8378898 | Remove AppContext from sun/awt/windows/WWindowPeer.java | 
| JDK-8378899 | Remove AppContext from java.awt.Toolkit implementation | 
| JDK-8378901 | Test "java/foreign/TestSegmentOffset.java timed out | 
| JDK-8378902 | Test compiler/vectorapi/TestVectorLibraryUnaryOpAndBinaryOp.java failed | 
| JDK-8378905 | RISC-V: fastdebug build fails after JDK-8377554 | 
| JDK-8378906 | NPE from javac thrown when encountering abstract synthetic bridge method | 
| JDK-8378917 | InputEvent checks for SystemClipboard access are unused | 
| JDK-8378918 | Remove AppContext from some DnD classes | 
| JDK-8378919 | Remove AppContext from the java.awt.Desktop class | 
| JDK-8378920 | Remove AppContext from SequencedEvent | 
| JDK-8378923 | Remove AppContext from datatransfer classes | 
| JDK-8378927 | H3MultipleConnectionsToSameHost.java#with-continuations intermittent fails | 
| JDK-8378935 | C2 crash in PhaseOutput::fill_buffer: wrong size of mach node | 
| JDK-8378946 | threadDump.schema.json syntax error, missing comma after owner | 
| JDK-8378948 | Remove unused local variable in RunnerGSInserterThread | 
| JDK-8378950 | Repeated warnings when annotation processing is happening | 
| JDK-8378963 | Test test/jdk/java/lang/management/RuntimeMXBean/InputArgument.java#id4 failed | 
| JDK-8378966 | C2: PhaseIdealLoop::pin_nodes_dependent_on must not be called with nullptr | 
| JDK-8378968 | C2 VectorAVX AVX2: wrong result, bad optimization in VectorLongToMaskNode::Ideal | 
| JDK-8378971 | Test jdk/jfr/event/runtime/TestVMInfoEvent.java fails after JDK-8378110 (RuntimeMXBean.getInputArguments()) | 
| JDK-8378985 | serviceability/sa/TestJhsdbJstackMixedWithXComp.java failed if sender frame is return barrier of Continuation | 
| JDK-8378992 | Case folding cache should not look up code point U+0000 | 
| JDK-8378999 | BeanContextSupport.add(Object) synchronizes on its argument | 
| JDK-8379013 | Remove some unused code in generateOopMap.cpp | 
| JDK-8379015 | Convert TraceNewOopMapGeneration to unified logging | 
| JDK-8379018 | Remove AppContext from two input method related classes | 
| JDK-8379019 | Remove AppContext from WindowEvent | 
| JDK-8379020 | GenShen: Promote all objects when whitebox full GC is requested | 
| JDK-8379021 | Shenandoah: Speedup ShenandoahSimpleBitMapTest | 
| JDK-8379024 | Convert utilities/vmError to use Atomic<T> | 
| JDK-8379025 | Convert utilities/debug to use Atomic<T> | 
| JDK-8379026 | Convert utilities/events to use Atomic<T> | 
| JDK-8379027 | Convert utilities/exceptions to use Atomic<T> | 
| JDK-8379035 | (tz) Update Timezone Data to 2026a | 
| JDK-8379039 | Build failure on vector API source generation after JDK-8378312 | 
| JDK-8379040 | Remove inclusion of allocation.hpp from atomicAccess.hpp | 
| JDK-8379041 | Crash in ResolvedFieldEntry::assert_is_valid(): invalid put bytecode 0 | 
| JDK-8379044 | [macos] javadoc for setOpenURIHandler refers to wrong type | 
| JDK-8379083 | Executable source program should ignore module declaration | 
| JDK-8379114 | HttpServer path prefix matching incorrectly matches paths that are not slash-prefixed | 
| JDK-8379119 | G1: Move NoteStartOfMarkHRClosure out of global namespace | 
| JDK-8379121 | G1: Remove redundant const_cast in g1BlockOffsetTable | 
| JDK-8379122 | Test java/lang/ProcessBuilder/Basic.java fails with 'Exception: java.lang.Error: PATH search algorithm" for multi-call binaries | 
| JDK-8379125 | C2: Control of rewired data in Range Check Elimination is not updated when further splitting a main loop leading to a segfault | 
| JDK-8379129 | C2 crash in LoadNode::can_split_through_phi_base during Escape Analysis (JDK 25.0.1+8) | 
| JDK-8379137 | Remove AppContext from javax.swing.JOptionPane | 
| JDK-8379153 | Refactor java/nio/channels/File{Channel,Lock} TestNG tests to use JUnit | 
| JDK-8379154 | Refactor Selector TestNG tests to use JUnit | 
| JDK-8379155 | Refactor Files TestNG tests to use JUnit | 
| JDK-8379156 | Specify behavior of Types.asElement() on array types | 
| JDK-8379158 | Update FreeType to 2.14.2 | 
| JDK-8379162 | AggregateRequestBodyTest.java intermittent fails "Connection closed by client peer: APPLICATION_ERROR" | 
| JDK-8379166 | Upstream redundant diffs fixed in Valhalla - Part 1 | 
| JDK-8379174 | G1: Too high G1RemSetArrayOfCardsEntriesBase causes invalid ergonomic G1RemSetArrayOfCardsEntries | 
| JDK-8379180 | C2: assertptr→bottom_type() == Type::TOP) \|\| ((base == Compile::current()→top( == (ptr→bottom_type()→make_ptr()→isa_oopptr() == nullptr))) failed: base input only needed for heap addresses | 
| JDK-8379181 | Convert ObjectMonitorTable to use Atomic<T> | 
| JDK-8379182 | [test] java/lang/ProcessBuilder/PipelineLeaksFD.java fails | 
| JDK-8379191 | SunX509KeyManagerImpl alias chooser methods returns null for EC_EC | 
| JDK-8379192 | Use an initializer list in the ObjectWaiter constructor | 
| JDK-8379193 | Incorrect build ID path in SATestUtils.java | 
| JDK-8379195 | Refactor Arrays TestNG tests to use JUnit | 
| JDK-8379196 | delta apply fix for JDK-8214934 | 
| JDK-8379200 | G1: Remove G1HeapRegion completion facility | 
| JDK-8379201 | Wrong type annotation offset on casts on expressions (redo) | 
| JDK-8379202 | Support linktime-gc on Linux with clang | 
| JDK-8379203 | [BACKOUT] Remove some unused code in generateOopMap.cpp | 
| JDK-8379224 | Always use zero as invalid page size | 
| JDK-8379229 | Remove AppContext from javax.swing.JComponent | 
| JDK-8379230 | JFR: Do not store leak context edge idx in markWord | 
| JDK-8379231 | Assembler::mov64 always emits 10-byte movabs even for small immediates | 
| JDK-8379233 | [REDO] Remove some unused code in generateOopMap.cpp | 
| JDK-8379256 | Update GIFlib to 6.1.1 | 
| JDK-8379260 | C2: Separate volatile barrier and full barrier | 
| JDK-8379273 | Convert miscellaneous utilities to use Atomic<T> | 
| JDK-8379284 | Avoid the need to keep obsolete preview feature constants until bootstrap JDK is upgraded | 
| JDK-8379297 | Serial: Replace CSpaceCounters with HSpaceCounters | 
| JDK-8379326 | Use JUnit in lib-test/jdk tests, here MethodTemplateTest | 
| JDK-8379331 | Remove modular transition support settings from TEST.ROOT files | 
| JDK-8379334 | jpackage: fix bug in DottedVersion.greedy() function | 
| JDK-8379337 | Deprecate Modifier.toString | 
| JDK-8379341 | jpackage: consolidate modular app tests | 
| JDK-8379344 | Compact the Unicode/CLDR version tables | 
| JDK-8379345 | jpackage: Fix issues in tests to improve their flexibility | 
| JDK-8379346 | Cleanup casts and implicit conversions to boolean | 
| JDK-8379347 | VoiceOver Doesn’t Correctly Identify JToggleButtons as "toggle buttons" | 
| JDK-8379348 | jpackage will use wrong arch suffix for RPM bundle when running on Debian Linux | 
| JDK-8379362 | C2: Buffer overrun in VectorSupport::lanetype2name | 
| JDK-8379363 | FontInfo improvements | 
| JDK-8379364 | Shenandoah: Remove duplicate semicolons in GenShen code | 
| JDK-8379367 | GenShen: Replace atomic promotion failure counters with thread local variables | 
| JDK-8379371 | Proposed cleanups for JDK-8373595 A new ObjectMonitorTable implementation | 
| JDK-8379372 | Enable some vector algorithms IR matching tests for RISC-V | 
| JDK-8379394 | Confine array dimension setting to the ArrayKlass constructor | 
| JDK-8379395 | [VectorAlgorithms] new dot-product implementation using fma | 
| JDK-8379396 | "assert(offset + partition_size ⇐ size()) failed: partition failed" when combining NonProfiledCodeHeapSize and large value for CICompilerCount | 
| JDK-8379404 | G1: Hide ConcurrentMarkThread reference from outside ConcurrentMark | 
| JDK-8379411 | Use TestFramework scenarios in 837841 | 
| JDK-8379412 | JfrJavaSupport::new_string should return early if pending exception | 
| JDK-8379415 | Contended classes can leave unused alignment padding | 
| JDK-8379416 | AIX build fails if system (not GNU) date tool is in PATH | 
| JDK-8379425 | Windows and macOS should not allow unsupported headless-only build | 
| JDK-8379426 | [macos] jpackage: runtime bundle version suffix is out of sync with the version property in the Info.plist file | 
| JDK-8379431 | [macos] jpackage issues unexpected warning when bundling an unsigned runtime package from the signed predefined runtime bundle | 
| JDK-8379432 | jpackage: Make default equals() in jdk.jpackage.test.CannedFormattedString class work as expected | 
| JDK-8379433 | Throwing proper exception for invalid encapsulation length in Hybrid | 
| JDK-8379435 | More proposed cleanups for JDK-8373595 A new ObjectMonitorTable implementation | 
| JDK-8379437 | Refactor ProcessBuilder and ProcessHandle tests to JUnit | 
| JDK-8379441 | Refactor jaxp/javax catalog tests to use JUnit | 
| JDK-8379442 | GenShen: Fix two errors introduced by 29039 | 
| JDK-8379443 | Correct build tags in some math tests | 
| JDK-8379448 | [PPC64] Build without C2 broken after 8373595 | 
| JDK-8379454 | SA still calculates end address with incorrect alignment | 
| JDK-8379456 | Test jdk/jfr/event/runtime/TestVMInfoEvent.java now fails with java.lang.NoClassDefFoundError: java/lang/management/ManagementFactory | 
| JDK-8379457 | Test EATests.java#id0 ERROR: monitor list errors: error_cnt=1 | 
| JDK-8379460 | C2: Notify AddI/AddL to URShiftI/URShiftL users | 
| JDK-8379464 | Enable missing stack walking test via jtreg requires for RISC-V | 
| JDK-8379477 | Tests in test/jdk/com/sun/net/httpserver/ may need to use othervm | 
| JDK-8379481 | Recent JNI methods are missing DTrace probes | 
| JDK-8379487 | Dangling static ref to fallback error in libinstrument’s JavaExceptions.c | 
| JDK-8379499 | [AIX] headless-only build of libjawt.so fails | 
| JDK-8379502 | Remove unused PhaseOutput::need_register_stack_bang() | 
| JDK-8379506 | Parallel: Move Parallel specific flags to parallel_globals.hpp | 
| JDK-8379511 | G1: G1CollectorState should derive concurrent cycle state from G1ConcurrentMark | 
| JDK-8379515 | draft-ietf-lamps-kyber-certificates is now RFC 9935 | 
| JDK-8379516 | Adjust JVM debug helper exports | 
| JDK-8379520 | Make use of new Taglet.toString method in JDK build taglets | 
| JDK-8379528 | Add @required in open/test/jdk/:jdk_sctp test, as these are applicable only on linux platforms | 
| JDK-8379531 | Shenandoah: Allow safepoint preemption during allocation of very large arrays | 
| JDK-8379535 | test/jdk/java/io/File/ListRoots.java fails with RuntimeException: Does not match FileSystem::getRootDirectories | 
| JDK-8379544 | C2: URShiftI Ideal optimization opportunity for - 3rd level notification required | 
| JDK-8379548 | java/util/logging/ParentLoggersTest.java failed with missing myParentLogger value | 
| JDK-8379549 | sun/security/ssl/SSLSocketImpl/SSLSocketSSLEngineCloseInbound.java failed with SocketException: Connection reset by peer | 
| JDK-8379550 | Bad bytecode offset after JDK-8371155 | 
| JDK-8379551 | sun/text/IntHashtable/Bug4705389.java timed out | 
| JDK-8379552 | Correct build tags in StrictMath/PowTests.java | 
| JDK-8379555 | Test compiler/igvn/ExpressionFuzzer.java crashed with -Xcomp: Not monotonic | 
| JDK-8379557 | Further optimize URL.toExternalForm | 
| JDK-8379558 | Test java/util/ResourceBundle/modules/basic/BasicTest.java times out on windows-x64 in Xcomp mode | 
| JDK-8379564 | Hotspot fails to build with gcc 14 after JDK-8378804 | 
| JDK-8379567 | URL documentation about encoding/decoding is hard to read | 
| JDK-8379583 | (fs) Files.copy use of posix_fadvise is problematic on Linux | 
| JDK-8379620 | G1: Typing error in jvmFlagConstraintsG1.cpp | 
| JDK-8379621 | Compiler produces unverifiable class-file when using primitive type with instanceof | 
| JDK-8379622 | Problemlist java/nio/channels/SocketChannel/OpenLeak.java and CloseDuringConnect.java on macos 26 | 
| JDK-8379625 | PPC: cleanup C2 OptoAssembly | 
| JDK-8379626 | Refactor jaxp/functional/javax/xml tests to use JUnit | 
| JDK-8379628 | Refactor langtools/shellsupport/doc tests to use JUnit | 
| JDK-8379630 | Add JMH benchmark to measure the overhead of using captured call state | 
| JDK-8379636 | constantPool.hpp includes bsmAttribute.inline.hpp | 
| JDK-8379665 | Obsolete AlwaysActAsServerClassMachine and NeverActAsServerClassMachine | 
| JDK-8379667 | C2: Deep recursion with cmovP_regNode::bottom_type | 
| JDK-8379671 | C2: Fix usage of PhaseGVN::transform in some intrinsics | 
| JDK-8379672 | jdk/internal/misc/VM/RuntimeArguments.java test still fails in Virtual threads mode after JDK-8373718 | 
| JDK-8379675 | test/jdk/sun/security/pkcs11/rsa/GenKeyStore.java uses removed methods | 
| JDK-8379677 | G1: Micros-System.gc benchmarks regress after moving to PartialArrayStates | 
| JDK-8379679 | java/foreign/TestConcurrentClose.java awaitTermination times out on Windows debug builds | 
| JDK-8379687 | Reduce C heap usage when VerifyMethodHandles flags is on | 
| JDK-8379688 | GenShen: Skip promotions when marking finds enough immediate garbage | 
| JDK-8379699 | H3ConnectionPoolTest::testH2H3WithTwoAltSVC fails intermittently | 
| JDK-8379704 | [s390x] Build without C2 broken after 8373595 | 
| JDK-8379706 | Cleanup and clarify BarrierSetAssembler::try_resolve_weak_handle_in_c2 | 
| JDK-8379707 | UserAuthWithAuthenticator::h3Test fails intermittentently | 
| JDK-8379710 | Enable vector if-conversion IR matching tests for RISC-V | 
| JDK-8379722 | C2: assert that result is used for CallLeafPureNode in final graph reshaping | 
| JDK-8379744 | Add a way to update the key of a red-black tree node | 
| JDK-8379778 | SunGraphics2D improvements | 
| JDK-8379781 | G1: Full GC does not print partial array task stats | 
| JDK-8379782 | Implement Object Monitor Table enabled by default | 
| JDK-8379790 | MemNode::can_see_stored_value incorrectly walks past barriers | 
| JDK-8379792 | UBSAN runtime error: signed integer overflow during compress expand value transform | 
| JDK-8379794 | C2: UBSAN runtime error: shift exponent 64 is too large for 64-bit type 'long unsigned int' | 
| JDK-8379795 | Test SuspendResume3.java can timeout due to deadlock | 
| JDK-8379798 | Refactor remaining tests in javax/xml/jaxp/functional to JUnit | 
| JDK-8379799 | Upstream redundant diffs fixed in Valhalla - langtool tests 1 | 
| JDK-8379802 | [AIX] unify DL_info struct and put it into a single header | 
| JDK-8379804 | Refactor jdk/com/sun tests to use JUnit | 
| JDK-8379811 | Disable AOTCodeCache if Assembly intrinsics use differs from Production use | 
| JDK-8379818 | Refactor java/nio/file/Files/StreamLinesTest.java to use JUnit | 
| JDK-8379819 | Creating AOT configuration crashes in MethodTrainingData::prepare | 
| JDK-8379824 | Refactor java/io tests to use JUnit | 
| JDK-8379828 | Remove ResourceBundle module tests with legacy ISO code property | 
| JDK-8379830 | The warnings for value-based classes need updating to include use of the Reference API | 
| JDK-8379846 | G1: IHOP Allocation rate calculation too sensitive to outliers causing endless concurrent cycles | 
| JDK-8379867 | C2 VectorAPI: Rename TypeVectMask → TypePVectMask for clarity | 
| JDK-8379873 | Remove undefined debugging declarations in os_windows.cpp | 
| JDK-8379917 | JFR: Method timing event can have incorrect minimum value | 
| JDK-8379937 | NullActiveWindowOnFocusLost.java fails intermittently. | 
| JDK-8379938 | [macos] jpackage SigningPackageTest test doesn’t create .pkg and .dmg bundles | 
| JDK-8379952 | Revert JDK-8365711: Restore protected visibility of menuBarHeight and hotTrackingOn | 
| JDK-8379953 | [macos] VoiceOver Reads "Header" instead of "Heading" | 
| JDK-8379967 | (process) Improve ProcessBuilder error reporting | 
| JDK-8379968 | Test crashed: assert(cl._thread_status != JavaThreadStatus::NEW) failed: unstarted Thread | 
| JDK-8379973 | Provide means to customize localized patterns in DateTimeFormatter/Builder | 
| JDK-8379981 | Virtual thread: crash in stackChunkOopDesc::print_on when thawing frame | 
| JDK-8380011 | Path-to-gcroots search should not trigger stack overflows | 
| JDK-8380025 | C2: Missed Value() optimization opportunity in CCP with LoadUB | 
| JDK-8380028 | Fix Page8 of manual test java/awt/print/PrinterJob/PrintTextTest.java on macOS | 
| JDK-8380030 | [AIX] unify dladdr between hotspot and libawt | 
| JDK-8380033 | Test java/util/jar/Manifest/IncludeInExceptionsTest.java could be more agnostic to VM default | 
| JDK-8380037 | JFR: Don’t format identifiers as numbers | 
| JDK-8380041 | PPC: remove POWER6 remnants | 
| JDK-8380060 | C2: Wrong execution with COH and arraycopy | 
| JDK-8380074 | Reflection::areNestMates should be used by Class::isNestmateOf | 
| JDK-8380078 | Update GIFlib to 6.1.2 | 
| JDK-8380079 | Add separate flag for platforms on which copy and clear operations are faster with AVX3Threshold set to 0 | 
| JDK-8380080 | PopFrame logic assumes trailing member name arg is recoverable as local 0 | 
| JDK-8380081 | jpackage: Incorrect descriptions of win-specific options | 
| JDK-8380083 | Enable some vector mask cast IR matching tests for RISC-V | 
| JDK-8380085 | jpackage (Linux) launcher crashes with SIGSEGV when input contains large number of JAR files | 
| JDK-8380091 | Tests in runtime/cds are failing with log parsing errors - missing output - but the output seems present | 
| JDK-8380103 | Perfdata shared memory file flock failures | 
| JDK-8380105 | Test vmTestbase/nsk/jdi/ThreadReference/ownedMonitorsAndFrames/ownedMonitorsAndFrames003/ownedMonitorsAndFrames003.java failed | 
| JDK-8380109 | Implement JEP 533: Structured Concurrency (Seventh Preview) | 
| JDK-8380125 | Formatting issue for few swing classes | 
| JDK-8380129 | Remove AccessFlags::print_on in favor of context-specific printing | 
| JDK-8380147 | Don’t require a CompilationUnit for end position APIs | 
| JDK-8380150 | JavaCallArguments defined too late | 
| JDK-8380158 | C2: compiler/c2/TestGVNCrash.java asserts with adr and adr_type must agree | 
| JDK-8380163 | Fix implicit conversion in macroAssembler_aarch64.hpp | 
| JDK-8380164 | Fix implicit conversion in nativeInst_aarch64.hpp | 
| JDK-8380168 | Obsolete unused flag UseNewLongLShift | 
| JDK-8380198 | Convert java/util/prefs/PrefsSpiTest.java to JUnit | 
| JDK-8380204 | java/io/File/EmptyPath.java fails due to unexpected listRoots value | 
| JDK-8380218 | Refactor test/jdk/java/nio/charset TestNG tests to JUnit | 
| JDK-8380219 | Interpreter debugging would be easier if InterpreterCodelets could print their assembly code | 
| JDK-8380221 | Change jdk/nio/Basic.java to use JUnit | 
| JDK-8380222 | Refactor test/jdk/java/lang/Character TestNG tests to JUnit | 
| JDK-8380223 | Refactor test/jdk/sun/util/resources TestNG tests to JUnit | 
| JDK-8380228 | JNI primitive type mismatch in Java_com_sun_java_swing_plaf_gtk_GTKLookAndFeel_applyThemeIfNeeded of swing_GTKEngine.c:403 (ID: 52423) | 
| JDK-8380229 | 2 Impossible or redundant condition defect groups in 2 files | 
| JDK-8380236 | macOS build is broken by JDK-8359706 | 
| JDK-8380281 | Remove the client emulation mode | 
| JDK-8380288 | [BACKOUT] Incorrect Interpretation of POSIX TZ Environment Variable on AIX | 
| JDK-8380291 | AOT cache should store only unregistered classes with file: code source | 
| JDK-8380292 | Confusing "reverted *" messages during training | 
| JDK-8380298 | Rename _pagesizes and _pre_allocated_pagesizes in hugepages.cpp | 
| JDK-8380303 | Obsolete unused flag UseXMMForArrayCopy | 
| JDK-8380307 | Refactor miscellaneous JAXP tests in javax/xml/jaxp/unittest to JUnit | 
| JDK-8380316 | Test runtime/os/AvailableProcessors.java fails Invalid argument | 
| JDK-8380390 | Shenandoah: Missing store barrier when resetting bitmaps | 
| JDK-8380407 | GenShen: Problem with com/sun/jdi/ThreadMemoryLeakTest | 
| JDK-8380408 | GenShen: Fix regression with vmTestbase/gc/memory/Churn/Churn2/TestDescription.java | 
| JDK-8380409 | JVM crashes when -XX:AOTMode=create uses app.aotconf generated with JVMTI agent | 
| JDK-8380416 | Enable CmpUNodeValueTests.java IR tests for RISC-V | 
| JDK-8380422 | Fix Formatting issues missed in JDK-8380125 | 
| JDK-8380427 | Revert JDK-8380292Confusing "reverted *" messages during training | 
| JDK-8380428 | ProblemList containers/docker/TestJcmdWithSideCar.java on linux-all | 
| JDK-8380431 | Shenandoah: Concurrent modification of stack-chunk objects during evacuation | 
| JDK-8380441 | G1: Move G1GCPauseType to G1CollectorState | 
| JDK-8380446 | C2: mask_and_replace_shift_amount should work for effectively-constant shift counts | 
| JDK-8380447 | Fix linter warnings in compiler testlibrary | 
| JDK-8380450 | (zipfs) ZipFileSystem::getPath and ZipPath::resolve throw unspecified IllegalArgumentException for unmappable names | 
| JDK-8380452 | ZipOutputStream::putNextEntry(ZipEntry) throws unspecified IllegalArgumentException for unmappable entry name | 
| JDK-8380457 | Cleanup ShortRSAKeyWithinTLS test and remove hardcoded debug | 
| JDK-8380459 | Shenandoah: Do not reset bytes-allocated-since-gc-start when degenerating | 
| JDK-8380464 | Inconsistency in emitting warning messages in vm_version_x86.cpp | 
| JDK-8380467 | JFR: RepositoryFiles.updatePaths() searches for chunkfiles incorrectly | 
| JDK-8380472 | Clean up test/jdk/jdk/nio/zipfs/PathOps.java shared file system usage | 
| JDK-8380474 | Crash SEGV in ThreadIdTable::lazy_initialize after JDK-8323792 | 
| JDK-8380475 | C2: Improve AndNode::Identity with KnownBits | 
| JDK-8380513 | [VectorAlgorithms] mismatch benchmark and test | 
| JDK-8380516 | HotSpot has two different ways of making Windows strtok_s available as strtok_r | 
| JDK-8380523 | Refactor TLAB slow allocation naming | 
| JDK-8380526 | G1: Remove "last young" use for the Prepare Mixed GC | 
| JDK-8380532 | Parallel: Rename allocate to cas_allocate in PSYoungGen | 
| JDK-8380534 | Use loopback address in java/net/DatagramPacket/ReuseBuf.java test | 
| JDK-8380541 | G1: Add g1CollectorState.inline.hpp file | 
| JDK-8380542 | ZipOutputStream.setComment and ZipEntry.setComment spec updates for rejecting unmappable characters and allowing null or empty comments | 
| JDK-8380561 | Refactor java/nio/channels/DatagramChannel TestNG tests to use JUnit | 
| JDK-8380562 | GenShen: GC notification event may see invalid values | 
| JDK-8380564 | Parallel: Rename allocate to cas_allocate_with_expansion in PSOldGen | 
| JDK-8380565 | PPC64: deoptimization stub should save vector registers | 
| JDK-8380578 | Remove miscellaneous AppContext uses, mostly in XAWT | 
| JDK-8380579 | C2: Intermittent fastdebug assert in Parse::sharpen_type_after_if: missing type check info | 
| JDK-8380580 | Enable TestCombineAddPWithConstantOffsets.java IR tests for RISC-V | 
| JDK-8380590 | Parallel: Improve tenuring-threshold heuristics | 
| JDK-8380634 | [macos] Remove macOS version restrictions on start with zero and limit to three components | 
| JDK-8380636 | Add static asserts for mirrored library constants | 
| JDK-8380641 | Thread dump parsing and test improvements | 
| JDK-8380649 | Parallel: Inline initialize_region_data and create_vspace of ParallelCompactData | 
| JDK-8380651 | [ubsan] gc/logging/TestGCId.java triggers runtime error: division by zero in shenandoahAdaptiveHeuristics | 
| JDK-8380656 | G1: Refactor G1IHOPControl | 
| JDK-8380663 | Update jcmd man page to include AOT.end_recording diagnostic command | 
| JDK-8380664 | Remove stub entries used in x86-32 | 
| JDK-8380665 | (dc) java/nio/channels/DatagramChannel/SendReceiveMaxSize.java could also test the loopback interface | 
| JDK-8380671 | Refactor BasicFloat16ArithTests.java | 
| JDK-8380672 | Improve certification checking | 
| JDK-8380686 | GenerateOopMap logging crashes in rewriter | 
| JDK-8380703 | Assertion failure in TestCodeEntryAlignment.java | 
| JDK-8380710 | nmethod::finalize_relocations() should be called before ICache::invalidate_range() is called | 
| JDK-8380714 | sun.font.HBShaper : use getUpcallStub to create all up call stubs | 
| JDK-8380753 | C2: RegMask::find_first_set() incorrectly returns partially contained sets for scalable vector registers | 
| JDK-8380755 | jdk/jfr/event/gc/heapsummary/TestHeapSummaryEventG1.java fails gcId should be increasing | 
| JDK-8380764 | Vector classes should have @ValueBased | 
| JDK-8380771 | Add missing CodeBlob Vptr implementations | 
| JDK-8380772 | Rename _total_allocations to _total_allocated_size in ThreadLocalAllocStats | 
| JDK-8380773 | Extend ContainerMemoryUsageEvent to include host memory usage | 
| JDK-8380776 | No longer necessary to disable calloc-transposed-args warnings when building harfbuzz | 
| JDK-8380789 | RISC-V: TestOpaqueConstantBoolNodes.java fails when running without RVV | 
| JDK-8380790 | Each invocation of getAccessibleText() adds DocumentListener | 
| JDK-8380794 | AttributedString performance | 
| JDK-8380795 | Consider omitting type annotations from method arguments in diagnostics | 
| JDK-8380804 | Remove remaining AppContext usage from the Swing implementation | 
| JDK-8380824 | java/net/DatagramSocket/SendReceiveMaxSize.java could also test the loopback interface | 
| JDK-8380825 | AOT tests should run in two step training mode by default | 
| JDK-8380828 | AOTCodeReader::fix_relocations() should be called before ICache::invalidate_range() is called | 
| JDK-8380846 | GenShen: Remove the experimental option to disable adaptive tenuring | 
| JDK-8380848 | jdk/nio/zipfs/ZipFSTester.java fails after JDK-8378884 | 
| JDK-8380849 | VoiceOver Fails to Activate Custom Button | 
| JDK-8380872 | Remove lingering comments about the removed stack locking | 
| JDK-8380896 | Reduce runtime for MonitorVmStartTerminate.java on hosts with a lot of VMs | 
| JDK-8380903 | [BACKOUT] Mitigate Neoverse-N1 erratum 1542419 negative impact on GCs and JIT performance | 
| JDK-8380904 | Test java/lang/StringBuilder/HugeCapacity.java asserts during code emission on Windows | 
| JDK-8380912 | "Trailing white space will be removed" incorrectly triggered with \u2028 | 
| JDK-8380913 | Sealed graph taglet omits public subtypes of invisible subtypes | 
| JDK-8380915 | Cleanup some Java declarations of the Vector API | 
| JDK-8380931 | Refactor remaining java/nio/channels TestNG tests to use JUnit | 
| JDK-8380936 | Cleanup temp dir usage in ZipFSTester | 
| JDK-8380941 | Remove AppContext from java.awt.Window implementation | 
| JDK-8380942 | Fix whitespace and conditional logic in interpreter code | 
| JDK-8380945 | [IR Framework] Test VM is finished before TestFrameworkSocket is ready to accept connection | 
| JDK-8380947 | Add pull request template | 
| JDK-8380955 | Value-based classes in FFM should have @ValueBased | 
| JDK-8380956 | HexFormat shoud have @ValueBased | 
| JDK-8380958 | GenShen: Regulator thread may observe inconsistent old generation state | 
| JDK-8380959 | Update Libpng to 1.6.56 | 
| JDK-8380960 | "Foreign function access" discussion links to wrong downcallHandle overload | 
| JDK-8380968 | classfile package doc mentions nonexistent UtfEntry | 
| JDK-8380971 | New -Werror syntax does not exclude categories | 
| JDK-8380987 | Fix implicit narrowing conversion in klass.hpp | 
| JDK-8380988 | C2: Unexpected node in SuperWord truncation: UModI/UDivI | 
| JDK-8380989 | HttpRequest.Builder#headers() throws an exception if an empty array is passed | 
| JDK-8380990 | Update testng and junit tests to use diagnoseConfigurationIssue() method and post processing | 
| JDK-8380999 | Update IPSupport by adding diagnoseConfigurationIssue() method | 
| JDK-8381002 | Thread dump should convert longs to json numbers if possible | 
| JDK-8381003 | [REDO] Mitigate Neoverse-N1 erratum 1542419 negative impact on GCs and JIT performance | 
| JDK-8381006 | G1: Wrong IHOP old gen allocation rate calculation in presence of periodic gcs | 
| JDK-8381007 | MultiPixelPackedSampleModel can throw ArithmeticException | 
| JDK-8381008 | [aix] Test java/net/DatagramSocket/SendReceiveMaxSize.java#preferIPv6Loopback fails after JDK-8380824 | 
| JDK-8381012 | Add warning for == for MemorySegment.NULL | 
| JDK-8381015 | CharsetEncoder.canEncode(CharSequence) is slow for UTF-8, UTF-16, UTF-32 | 
| JDK-8381019 | Remove AppContext usage from AccessBridge | 
| JDK-8381020 | Remove AppContext from java.awt.Dialog | 
| JDK-8381037 | Remove AppContext from miscellaneous awt shared classes | 
| JDK-8381039 | Enhance AWT ImagingLib | 
| JDK-8381049 | Enhance Jar handling | 
| JDK-8381059 | Add class name to deoptimization events and -Xlog:deoptimization=debug output | 
| JDK-8381117 | AOT training run fails if memory for ConstantPool is reused | 
| JDK-8381145 | Missing ResourceMark in Mutex::print_on() | 
| JDK-8381154 | Shenandoah: Region states could be read in wrong order for comparisons | 
| JDK-8381170 | JFR: Recursion in jdk.MethodTrace caused by Integer::reverseBytes | 
| JDK-8381176 | Utilities.expectedNegoAppProtocol chooses client ALPN protocol over server’s | 
| JDK-8381184 | Code clean up: missing line break and print out duplication | 
| JDK-8381205 | GHA: Upgrade Node.js 20 to 24 | 
| JDK-8381208 | Init cause with the caught runtime exception | 
| JDK-8381219 | ZGC Relocation Set Selection Silently Loses Page Statistics | 
| JDK-8381233 | Linker does not need to be value based class | 
| JDK-8381235 | TestCompileTaskTimeout intermittently fails due to unexpected exit code 1 | 
| JDK-8381250 | Enable VectorLogicalOpIdentityTest.java IR tests for RISC-V | 
| JDK-8381257 | [s390x] deoptimization stub should save vector registers | 
| JDK-8381270 | TestGCPhaseConcurrent.java#Z intermittent fails OOME | 
| JDK-8381315 | compiler/vectorapi/TestVectorReallocation.java fails with -XX:UseAVX=1 after JDK-8380565 | 
| JDK-8381316 | HttpClient / Http3: poor exception messages on SSL handshake errors | 
| JDK-8381320 | Problemlist compiler/vectorapi/TestVectorReallocation.java | 
| JDK-8381359 | Refactor java/net/DatagramSocket TestNG tests to use JUnit | 
| JDK-8381362 | C2: assert(_late_inlines.length() == 0 \|\| IncrementalInlineMH \|\| IncrementalInlineVirtual) failed: not empty | 
| JDK-8381364 | Update the java tool specification for CompileCommand compile levels | 
| JDK-8381367 | [lworld] tools/jpackage/share/AppVersionTest.java timed out while reporting errors | 
| JDK-8381379 | Support std/dstOffset attributes in CLDR’s metazone definitions | 
| JDK-8381380 | Test host wildcard matching as part of a TLS validation of the server’s identity | 
| JDK-8381382 | Shenandoah: assert(capacity > 0) failed: free regions must have allocation capacity | 
| JDK-8381384 | jpackage: add test coverage to WiX discovery | 
| JDK-8381385 | nsk/jdi/ClassLoaderReference/visibleClasses/visibleclasses002/ fails with --enable-preview and Java Usage Tracker enabled | 
| JDK-8381397 | G1: Inline initiate_conc_mark/record_conc_mark_init_end | 
| JDK-8381403 | @Deprecated annotation has no effect warnings on record components | 
| JDK-8381409 | Fix whitespace and variable name in classfile code | 
| JDK-8381412 | [PPC64] unproblemlist JDK-8320897 | 
| JDK-8381413 | G1: Verification accesses G1CollectorState checking whether in concurrent start gc after it has been reset for this pause | 
| JDK-8381418 | Clarifying the identity Parameter in the Java Stream.reduce() Method Documentation | 
| JDK-8381434 | Convert hard coded debug to Boolean.getBoolean in security libraries | 
| JDK-8381436 | Remove Obsolete Translation Resources | 
| JDK-8381449 | Build fails when Windows is not installed on C: | 
| JDK-8381451 | Prefetch support on Windows/x86 and Windows/AArch64 | 
| JDK-8381452 | TestMultiplyReductionByte.java: Update the IR test on AArch64 for vector length above 128 bits | 
| JDK-8381464 | Typo in Linker::captureCallState JavaDoc | 
| JDK-8381466 | G1: Fix outdated name in G1Policy::decide_on_concurrent_start_pause | 
| JDK-8381475 | Test tools/sincechecker/modules/jdk.compiler/JdkCompilerCheckSince.java fails for DocSourcePositions.java | 
| JDK-8381487 | Replace threadDump.schema.json with document to describe format | 
| JDK-8381519 | Enhance Der Value Handling | 
| JDK-8381549 | GenShen: Global collections skip coalesce and fill when the collection set is empty | 
| JDK-8381550 | Shenandoah: Fix confusing end of degenerated cycle log message | 
| JDK-8381553 | Clean up AOTCodeCache configuration checks | 
| JDK-8381554 | RISC-V: Small refactoring for cmp_klass_compressed macro-assembler routine | 
| JDK-8381555 | test/hotspot/jtreg/gc/arguments/TestG1CompressedOops.java fails on 32 bit architectures | 
| JDK-8381561 | G1: Cleanup patch in preparation for eager reclamation of flat arrays | 
| JDK-8381566 | G1: Concurrent refinement pre-sweep time logged as incorrect negative value | 
| JDK-8381568 | Refactor java/net/spi/InetAddressResolverProvider test from testNg to use JUnit | 
| JDK-8381579 | C2: "fatal error: LROTATE: double" when using VectorAPI | 
| JDK-8381596 | Adjust checks which use supports_ht() on x86 for hybrid CPUs | 
| JDK-8381606 | Javadoc typo in ResourceBundle example | 
| JDK-8381609 | com/sun/jdi/EATests.java should not synchronize on an Integer instance | 
| JDK-8381611 | RISC-V: Factor out function cmpptr from MacroAssembler | 
| JDK-8381616 | Refactor various java/net/**Socket** / TestNG tests to use JUnit | 
| JDK-8381617 | vm_version_windows_aarch64.cpp: PF_ARM_SVE_INSTRUCTIONS_AVAILABLE undeclared identifier | 
| JDK-8381623 | Additional immutability in sun.font: ExtendedTextSourceLabel, GlyphLayout | 
| JDK-8381636 | Add built-in AOT support to GrowableArray | 
| JDK-8381639 | Remove bug id 8381475 from test/jdk/tools/sincechecker/modules/jdk.compiler/JdkCompilerCheckSince.java | 
| JDK-8381642 | javac provides the wrong line number for an error in a lambda expression | 
| JDK-8381643 | Shenandoah: Remove duplicate region filtering logic | 
| JDK-8381644 | Locale matching APIs should describe `null` element behavior | 
| JDK-8381650 | Vector rotate operations on AArch64 with NEON | 
| JDK-8381651 | Remove non-failing ModalFocusTransfer test from Problemlist | 
| JDK-8381654 | Tokenizer warnings dropped when annotation processing is present | 
| JDK-8381655 | Port JDK-8363978 fix from leyden repo to mainline | 
| JDK-8381656 | Take into account trampoline stub size and its relocations on Aarch64 | 
| JDK-8381658 | Update nsk/jvmti/scenarios/sampling to better test virtual threads | 
| JDK-8381659 | Determine actual jump instruction when AOT and JITed code are used | 
| JDK-8381662 | New test compiler/exceptions/TestDebugDuringExceptionCatching.java failing with C1-only | 
| JDK-8381670 | Revert the changes to GZIPInputStream related to InputStream.available() usage | 
| JDK-8381677 | Remove dependence of UseSHA on UseSHAIntrinsics | 
| JDK-8381681 | Clarify locale usage in Javadoc for the Locale display methods | 
| JDK-8381683 | Pass temp registers and use them in CardTableBarrierSetAssembler::store_check() on Aarch64 | 
| JDK-8381703 | G1: Racy re-read of G1CMRootRegion::num_remaining_regions causes assertion failures | 
| JDK-8381710 | Avoid using lambdas in ModuleBootstrap in AOT training mode | 
| JDK-8381722 | Incremental build fails with FileAlreadyExistsException: PreviewFeature.java | 
| JDK-8381723 | Crash in fastdebug VM: assert(strlen(name1) + strlen(name2) < sizeof(stub_id)) failed | 
| JDK-8381724 | RISC-V: Pass temp registers and use them in CardTableBarrierSetAssembler::store_check() | 
| JDK-8381739 | Comma-separated declaration with type-use annotations causes compiler crash | 
| JDK-8381745 | Ensure Modal/FileDialog tests explicitly reference Asserts class | 
| JDK-8381746 | Add --with-print-assembly-options configure flag to set default PrintAssemblyOptions | 
| JDK-8381765 | AArch32: Capstone hsdis does not build | 
| JDK-8381767 | Refactor various java/net/**[URL/Http]** / TestNG tests to use JUnit | 
| JDK-8381768 | C2: GC barrier stubs miscalculate skipped instructions size | 
| JDK-8381770 | Revert some shared changes done by JDK-8381003 | 
| JDK-8381771 | Add a check for DNS label not to end with a hyphen | 
| JDK-8381772 | Remove mention of "mutable strings" in java.lang.String Javadocs | 
| JDK-8381787 | Add testcase for Vector Lane Reversal issue | 
| JDK-8381793 | Clean up unused ProblemList-coh.txt | 
| JDK-8381795 | AArch64: Optimized build is broken after JDK-8381003 | 
| JDK-8381796 | Enhance Certificate parsing | 
| JDK-8381807 | AArch64: compiler/cpuflags/CPUFeaturesClearTest.java fails on SVE after JDK-8364584 | 
| JDK-8381811 | [S390] Add support for JDK-8200555 | 
| JDK-8381812 | The synthetic $deserializeLambda$ is notably bigger | 
| JDK-8381819 | Test compiler/cpuflags/TestAESIntrinsicsOnUnsupportedConfig.java fails after JDK-8364584 | 
| JDK-8381829 | Obsolete AggressiveHeap | 
| JDK-8381834 | Improve TLAB sizing heuristics | 
| JDK-8381838 | java/net/DatagramSocket/SendReceiveMaxSize.java fails to compile after JDK-8381008 | 
| JDK-8381840 | Lots of /tmp/8173970-<number> folders on test machines | 
| JDK-8381842 | Refactor remaining TestNG tests under java/net/ to use JUnit | 
| JDK-8381843 | FileDialog:setFile() specification needs to be enhanced w.r.to file selection | 
| JDK-8381851 | handleMessage use uninitialized struct | 
| JDK-8381862 | Refactor remaining TestNG tests under jdk/net/ExtendedSocketOption to use JUnit | 
| JDK-8381865 | Fix debug build failure caused by not clearing CPU_APX_F feature after JDK-8364584 | 
| JDK-8381871 | GenShen: ShenandoahGCHeuristics flag not reset after ignoring non-adaptive value | 
| JDK-8381872 | C2: assert before block local scheduling failed with UseCompactObjectHeaders | 
| JDK-8381879 | JDK-8372617 has broken the Zero build | 
| JDK-8381880 | Test compiler/c1/TestTooManyVirtualRegistersMain.java uses wrong class in CompileCommand | 
| JDK-8381898 | SwitchBootstraps::typeSwitch() not accepting Float, Double types | 
| JDK-8381899 | Assertions and crashes in aot/cds tests on Linux Alpine | 
| JDK-8381900 | Test vmTestbase/nsk/jvmti/scenarios/allocation/AP03/ap03t001 does not handle frequent GC gracefully | 
| JDK-8381924 | Fix include guard in lambdaProxyClassDictionary.hpp | 
| JDK-8381926 | Fix implicit conversion in macroAssembler_riscv.hpp | 
| JDK-8381932 | Publish stubgen entries when -XX:-AOTStubCaching configured | 
| JDK-8381933 | Possible memory leak in src/hotspot/share/gc/shenandoah/shenandoahHeapRegionCounters.cpp | 
| JDK-8381934 | Wrong type passed to FREE_C_HEAP_ARRAY deallocating G1CardSetMemoryManager | 
| JDK-8381935 | Improve numChunks range in the PPC64 CallAranger | 
| JDK-8381937 | Make exceptions in Java_sun_security_mscapi_CKeyPairGenerator generateCKeyPair more specific | 
| JDK-8381939 | G1: Fix marking state verification at the end of a concurrent start pause | 
| JDK-8381965 | NullPointerException in TypeAnnotationPositions | 
| JDK-8381968 | Extend cds/appcds/aotCode/AOTCodeFlags.java to run test with different flags in assembly and prod run | 
| JDK-8381969 | os::physical_memory is still broken in 32-bit systems when running on 64-bit OSes | 
| JDK-8381975 | CPU feature verification in AOTCodeCache should check for exact match | 
| JDK-8381984 | Test jdk/nio/zipfs/ZipFSTester.java fails with mismatched timestamp | 
| JDK-8381985 | Shenandoah: allocation rate sample could be wrong in some rare cases | 
| JDK-8381988 | Fix inconsistency in clearing cpu feature bits | 
| JDK-8381993 | Simplify MetaspaceClosure handling of Array and GrowableArray | 
| JDK-8381997 | Move markWord.hpp cleanups from the valhalla lworld branch | 
| JDK-8381998 | Move UseCompressedClassPointers to obsolete flags section | 
| JDK-8382018 | test/jdk/java/nio/file/spi/SetDefaultProvider.java leaves a directory in /tmp | 
| JDK-8382020 | Time Zone Abbreviation Not Localized for Non-English Locales | 
| JDK-8382022 | jpackage: enhance CompositeProxy | 
| JDK-8382025 | [lworld] tools/jpackage/share/AsyncTest.java fails with An error of type -10810 has occurred | 
| JDK-8382030 | GHA: Enable Windows ARM64 tests | 
| JDK-8382031 | Update IANA Language Subtag Registry to Version 2026-05-05 | 
| JDK-8382034 | JFR: jdk-agents view is missing information | 
| JDK-8382035 | [ubsan] Under UBSAN builds, disable tests that rely on simulated JVM crashes | 
| JDK-8382039 | Add support do_arch_array_entry() template | 
| JDK-8382043 | Add missing @Override annotations in "javax.print.attribute" package | 
| JDK-8382045 | Increase AOT C strings table size in not product VM | 
| JDK-8382047 | Update Libpng to 1.6.57 | 
| JDK-8382048 | JFR: RecordingFile::write doesn’t truncate when overwriting | 
| JDK-8382050 | compiler/intrinsics/klass/CastNullCheckDroppingsTest.java fails with -XX:+StressUnstableIfTraps | 
| JDK-8382054 | TestLoopPeelingDisabled.java fails with -DVerifyIR=false | 
| JDK-8382055 | G1: Remove unused unused args in mark_evac_failure_object | 
| JDK-8382057 | C2: "assertptr→bottom_type() == Type::TOP) \|\| ((base == Compile::current()→top( == (ptr→bottom_type()→make_ptr()→isa_oopptr() == nullptr))) failed: base input only needed for heap addresses" with -XX:+CountCompiledCalls | 
| JDK-8382063 | Jtreg test javax/swing/SwingWorker/TestDoneBeforeDoInBackground.java fails | 
| JDK-8382081 | AOT tests fail in add_stub_entries without JVMCI | 
| JDK-8382084 | Use dynamic chunk sizes in PartialArraySplitter | 
| JDK-8382087 | aarch64: remove unused function declarations in macroAssembler_aarch64.hpp | 
| JDK-8382088 | ObjectMonitor::wait() lacks suspension handling for TS_RUN path | 
| JDK-8382089 | G1: Refactor concurrent refinement state machine | 
| JDK-8382090 | Remove .rej and .orig from .gitignore | 
| JDK-8382095 | GenShen: Do not clamp young reserve unnecessarily | 
| JDK-8382103 | Shenandoah: Optimize ShenandoahLock size for release build | 
| JDK-8382118 | Refactor open/test/jdk/sun/net/www/protocol/file/DirPermissionDenied.java test | 
| JDK-8382130 | Remove test/failure_handler/Makefile | 
| JDK-8382134 | Replace assert with fatal check in AOTCodeCache::load_strings() to make sure we have valid value in product VM | 
| JDK-8382146 | jdk/jfr/event/compiler/TestCodeCacheFull.java fails when C2 is disabled | 
| JDK-8382150 | Shenandoah: Clean up nmethod entry barrier support | 
| JDK-8382166 | [AOT Cache] CRC was not checked on Code Region | 
| JDK-8382167 | Warning message should be displayed only when UseAESCTRIntrinsics is explicitly enabled by the user | 
| JDK-8382170 | Assert in AnyObj::operator delete due to race condition in in_aot_cache() | 
| JDK-8382174 | Clarify the meaning of address cast in ADD() macro | 
| JDK-8382196 | Clean up C++ iterator style in compactHashtable.hpp and trainingData.hpp | 
| JDK-8382199 | Remove unused parameter "dst" from CardTableBarrierSetAssembler::store_check() | 
| JDK-8382201 | Transparent Window is Opaque w/o Volatile Buffering | 
| JDK-8382202 | New file VectorMinMaxTransforms.java has a copyright format error | 
| JDK-8382211 | Shenandoah: Clean up and wire up final verification | 
| JDK-8382222 | sun/security/ssl/SSLSocketImpl/SSLSocketBruteForceClose.java fails sporadically | 
| JDK-8382242 | JFR: Metadata reconstruction invalidates ConstantMap for java.lang.String | 
| JDK-8382249 | G1: Racy concurrent update of young gen size and revising young gen length | 
| JDK-8382257 | Shenandoah: Clean up stack watermark barrier support | 
| JDK-8382264 | Refactor java.logging TestNG tests to use JUnit | 
| JDK-8382267 | VectorMathLibrary uses locale-sensitive String.format to construct native symbol names | 
| JDK-8382273 | Update nsk/jvmti/unit tests to use ThreadWrapper | 
| JDK-8382275 | Update nsk/stress/jni to use ThreadWrapper | 
| JDK-8382277 | Shenandoah: Improve mutator runtime entry points | 
| JDK-8382295 | Shenandoah: wrong denominator in full gc summary | 
| JDK-8382306 | Remove JvmtiTagMap::check_hashmap | 
| JDK-8382311 | 30% Performance regression in Philosophers benchmark | 
| JDK-8382312 | Cleanup instanceKlass dead code | 
| JDK-8382315 | RISC-V: TestMultiplyReductionByte.java fails with guarantee(is_uimm5(imm)) failed: uimm is invalid | 
| JDK-8382316 | Unused xtmp in long_to_maskLE8_avx | 
| JDK-8382327 | Disable some SA tests for mixed jstack on musl C | 
| JDK-8382332 | Large regression (~50%) in SPECjbb2015 when using JFR after JEP 500 | 
| JDK-8382359 | Align 32-bit data in AOT Code Cache to sizeof(uint) | 
| JDK-8382368 | Warning that @Deprecated "has no effect" is suppressed by @Deprecated | 
| JDK-8382375 | Remove obsolete options from CLDRConverter | 
| JDK-8382389 | Test java/lang/LazyConstant/LazyConstantSafePublicationTest.java failed | 
| JDK-8382390 | Shenandoah: heap usage log improvement | 
| JDK-8382391 | Cleanup unnecessary storestore() in stringStream::as_string | 
| JDK-8382393 | Enable VectorStoreMaskIdentityTest.java IR tests for RISC-V | 
| JDK-8382395 | Disable stringop-overflow in shenandoahGenerationalHeap.cpp | 
| JDK-8382396 | RepaintManager throws NPE when painting detached JPanel without volatile buffering | 
| JDK-8382397 | Make lint category property declaration more straightforward | 
| JDK-8382401 | Remove return type parameters from FREE_ and REALLOC_ macros | 
| JDK-8382403 | Enhance Kerberos DNS KDC discovery logging to expose sanitized DNS failure | 
| JDK-8382418 | G1: Remove some stale comments about time accounting | 
| JDK-8382419 | Add missed @key randomness after JDK-8370489 | 
| JDK-8382420 | [macos] remove java/awt/Modal/ToBack/ tests from problemlist | 
| JDK-8382422 | G1: G1GCPhaseTimes::record_merge_heap_roots() uses += instead of direct assignment | 
| JDK-8382423 | G1: Remove unused G1GCPhaseTimes::_external_accounted_time_ms | 
| JDK-8382430 | Extend output format of -XX:+PrintCompilation2 diagnostic flag | 
| JDK-8382433 | GenShen: Remembered set scan encountered invalid object | 
| JDK-8382436 | Build failures after JDK-8326205 | 
| JDK-8382441 | Problemlist TestStressAllocationGCEventsWithShenandoah and TestStressBigAllocationGCEventsWithShenandoah | 
| JDK-8382471 | Improve Resource Resolving | 
| JDK-8382482 | Optimize equals scenario in x86 scalar floating point min/max reduction loops | 
| JDK-8382485 | ArrayIndexOutOfBoundsException was hiding in ClhsdbPrintAll.jtr | 
| JDK-8382487 | Remove unnecessary SuppressWarnings annotations in java.desktop | 
| JDK-8382499 | G1: Let non young cset choice time also use Ticks API for measurements | 
| JDK-8382501 | Rename occurrences of com.sun.hotspot.perfdata in HotSpot source code | 
| JDK-8382522 | Disable stringop-overflow in safepointMechanism.cpp | 
| JDK-8382529 | Tests com/sun/jdi/RedefineCrossEvent.java and com/sun/jdi/ClassesByName2Test.java fail when running frequent GC | 
| JDK-8382532 | C2: VectorAPI: disable VectorMaskCmp-not optimization for predicated compares | 
| JDK-8382534 | Some opto methods should return bool instead of int | 
| JDK-8382536 | C2: sharpen_type_after_if: assert(val→find_edge(con) > 0) failed: mismatch | 
| JDK-8382537 | Invalid JDWP command set number or command number can result in crash | 
| JDK-8382546 | Missing AOT external address initialization for string_indexof stubs on x86 | 
| JDK-8382548 | serviceability/sa/TestJhsdbJstackMixedWithXComp.java#xcomp-disable-tiered-compilation failed with Test failed! | 
| JDK-8382582 | Remove the experimental JVMCI feature | 
| JDK-8382588 | AArch64: Sync aarch64_vector.ad with aarch64_vector_ad.m4 after JDK-8382039 | 
| JDK-8382606 | Enable VectorMinMaxTransforms.java IR tests for RISC-V | 
| JDK-8382608 | Shenandoah: STS yield deadlocks in OOM-evac scope | 
| JDK-8382609 | G1: Remove outdated comments in do_full_collection | 
| JDK-8382614 | Consolidate implementation of current_stack_base_and_size on BSD | 
| JDK-8382616 | Create GCArguments::set_heap_size interface | 
| JDK-8382617 | Test runtime/cds/appcds/aotCache/AOTCacheConsistency.java fails with "can’t find HelloWorld in test directory or libraries" | 
| JDK-8382620 | G1: Remove unused G1RefineRegionClosure::_num_collections_at_start | 
| JDK-8382625 | G1: Constify G1Policy::need_to_start_conc_mark() | 
| JDK-8382628 | VectorExpressionFuzzer.java fails due to C1 compilation bailout | 
| JDK-8382634 | Remove redundant TLABRefillWasteFraction zero check | 
| JDK-8382635 | G1: TAMSes overwritten by G1ConcurrentMark::fully_initialize() cause verification errors | 
| JDK-8382636 | Shenandoah: Use self-forwarding to handle OOM during evacuation | 
| JDK-8382637 | GenShen: ubsan error, divide by zero during TestThreadFailure | 
| JDK-8382681 | GenShen: ProblemList TestThreadFailure | 
| JDK-8382684 | jpackage: assorted fixes of the test lib | 
| JDK-8382690 | aarch64: Clean up redundant restore_locals() in TemplateTable::invokeinterface | 
| JDK-8382691 | RISC-V: Clean up redundant restore_locals() in TemplateTable::invokeinterface | 
| JDK-8382700 | C2: Delay inlining instead of giving up when hit NodeCountInliningCutoff | 
| JDK-8382711 | Shenandoah: Support AOT code dumping | 
| JDK-8382740 | JFR: Disable jdk.OldObjectSample event for generational ZGC | 
| JDK-8382777 | PrintMethodData not showing BranchData | 
| JDK-8382815 | C2: Reference intrinsics are broken by shadow "referent" field | 
| JDK-8382843 | Update ProblemList comments for preview features | 
| JDK-8382871 | Completion failure during diagnostic formatting | 
| JDK-8382877 | Heap pollution warning points to first parameter instead of last | 
| JDK-8382878 | RISC-V: Missing InlineSkippedInstructionsCounter in ZGC barriers stubs | 
| JDK-8382880 | C2: Remove unused Final_Reshape_Counts::get_*_count() methods | 
| JDK-8382881 | Swap min/max values and avoid equals min/max values in MinMaxVector | 
| JDK-8382888 | [asan] ASAN crashes with high heap base addresses during startup | 
| JDK-8382890 | JcmdOnTrainingProcess.java fails Native memory tracking is not enabled | 
| JDK-8382920 | Shenandoah: Do fewer implicit narrowing conversions | 
| JDK-8382928 | Add documentation to java.net.URI.normalize about empty segments | 
| JDK-8382930 | Do fewer implicit narrowing conversions in methodData.hpp | 
| JDK-8382931 | Do fewer implicit narrowing conversions in deoptimization.hpp | 
| JDK-8382933 | Improve Shenandoah clone barrier tests | 
| JDK-8382934 | LateInlineQueueDrainTest.java fails without diagnostic option | 
| JDK-8382936 | C2: Load folding must not drop Type information | 
| JDK-8382940 | Clean up the AlwaysAtomicAccesses flag | 
| JDK-8383161 | [PPC64] MachCallDynamicJavaNode::ret_addr_offset() needs adaptation for COH | 
| JDK-8383162 | jpackage: decouple WinMsiPackager and RTF converter | 
| JDK-8383163 | Build without CDS broken after 8382170 | 
| JDK-8383168 | jpackage: enhance capabilities of the test lib for testing jpackage console output | 
| JDK-8383172 | Create AbstractLRUReferencePolicy base class | 
| JDK-8383173 | Adjust CopyAVX3Threshold for AMD AVX512 targets | 
| JDK-8383174 | Improve exceptions in Java_sun_net_www_protocol_http_ntlm_NTLMAuthSequence_getNextToken | 
| JDK-8383175 | (tz) Update Timezone Data to 2026b | 
| JDK-8383176 | Shenandoah: Skip marked objects in final verification steps | 
| JDK-8383183 | Shenandoah: Mangle trashed regions up to top instead of end | 
| JDK-8383186 | Large SoftRefLRUPolicyMSPerMB can lead to overflow | 
| JDK-8383196 | Shenandoah: Interpreter barriers clobber interpreter registers | 
| JDK-8383243 | Make objcopy configurable to compress debug info | 
| JDK-8383262 | Implement optimized `aarch64_get_thread_helper()` for Windows/AArch64 | 
| JDK-8383354 | Update LCMS to 2.19.1 | 
| JDK-8383366 | Missing call store_archive_data() on one of paths in generate_kyber12To16_avx512() | 
| JDK-8383367 | jdk/jfr/event/oldobject/TestDFSWithSmallStack.java failed with -XX:+UseZGC -XX:+UseCompactObjectHeaders | 
| JDK-8383396 | Parallel: Avoid reloading klass when pushing promoted object contents | 
| JDK-8383414 | Exhaustiveness rejects a type pattern that covers all permitted subtypes | 
| JDK-8383420 | SIGSEGV in PhaseChaitin::gather_lrg_masks | 
| JDK-8383421 | ZGC: Problematic interactions between `JVMTI_EVENT_SAMPLED_OBJECT_ALLOC` and`clone` | 
| JDK-8383459 | [BACKOUT] [macos] VoiceOver Reads "Header" instead of "Heading" | 
| JDK-8383462 | G1: scan_root_regions should guarantee that the number of workers is nonzero | 
| JDK-8383473 | Follow on from 2026b time change to include temporary hack BC time change | 
| JDK-8383485 | [S390X] Add patching support for LIR_Assembler::leal | 
| JDK-8383486 | G1: Revise documentation about root scanning | 
| JDK-8383503 | Test runtime/cds/MetaspaceAllocGaps.java failed | 
| JDK-8383515 | Add guarantees to disallow running WorkerTasks with zero workers | 
| JDK-8383525 | DateTimeFormatterBuilder.getLocalizedDateTimePattern() concurrency issue | 
| JDK-8383526 | Update warning messages to be consistent in the naming the instruction when it is not supported | 
| JDK-8383541 | Safefetch should return the error value when accessing pages protected with PAGE_GUARD on windows-aarch64 | 
| JDK-8383542 | Remove hard-coded enum value from CPU_FEATURE_FLAGS | 
| JDK-8383545 | Gcc warning in bsmAttribute.hpp: comparison is always true | 
| JDK-8383546 | HeapReserver::Instance::reserve_heap() breaks 32-bit build | 
| JDK-8383550 | AsyncExceptionHandshakeClosure::do_thread() should not be passed nullptr | 
| JDK-8383564 | Avoid reloading klass when transforming stack chunks | 
| JDK-8383570 | Test DisabledAlgorithms.java intermittently timed out | 
| JDK-8383601 | RISC-V: ShenandoahBarrierSetAssembler::load_reference_barrier calls "weak" on "phantom" path | 
| JDK-8383605 | Incomplete input validation on the bands parameters in j.a.i.Raster | 
| JDK-8383608 | Make BinaryEncodable non-exhaustive | 
| JDK-8383611 | assert(count == os::processor_count() + 1) failed: invalid enumeration! | 
| JDK-8383623 | (fs) Update Windows file system provider to use GetFileInformationByName where possible | 
| JDK-8383629 | JNI exception pending in -[TabButtonAccessibility accessibilityValue] of TabButtonAccessibility.m | 
| JDK-8383630 | Fix iteration in tests doing class redefinition | 
| JDK-8383631 | Test vmTestbase/nsk/jdb/kill/kill003/kill003.java failed - possible NullPointerException | 
| JDK-8383634 | AOTCodeTest fails intermittently with Parallel | 
| JDK-8383635 | Improve performance of ML-KEM’s implKyberNttMultJava() method | 
| JDK-8383637 | JConsole should not render HTML in components where it could be confusing | 
| JDK-8383639 | JConsole unnecessary classloading for MBean types | 
| JDK-8383642 | Additional Shenandoah clone barrier tests | 
| JDK-8383643 | Serial: Avoid reloading klass when checking string dedup candidates | 
| JDK-8383650 | Shenandoah: Missing card table updates for pinned regions | 
| JDK-8383709 | Linux perf map should record individual stubs | 
| JDK-8383738 | javac crashes with StackOverflowError when a class extends a type parameter | 
| JDK-8383743 | Use RAII pattern to find matching compiler directive | 
| JDK-8383767 | C2: assert(curr_ctrl→in(0)→Opcode() == Op_If) failed: unexpected node MemBarStoreStore | 
| JDK-8383769 | compiler/c2/TestDependsOnTestSqrtHFAssertion.java timed out | 
| JDK-8383770 | Remove flags from AOTCodeCache::Config already covered by cpu features | 
| JDK-8383772 | runtime/reflect/ReflectOutOfMemoryError.java can fail intermittently on Shenandoah with small heaps | 
| JDK-8383794 | G1: Rename refinement sweep duration helper to reflect SwapGlobalCT start | 
| JDK-8383807 | Parallel: Improve Young GC stack trimming | 
| JDK-8383810 | Shenandoah: Simplify native CAS barriers | 
| JDK-8383813 | pkcs12 keystore fails to load because of incorrect DER length-encoding | 
| JDK-8383821 | IllegalStateException when command executed by jpackage "Executor" timeouts | 
| JDK-8383824 | TestCopyFiles.java fails after JDK-8347112 | 
| JDK-8383825 | Incorrect handling of Hawaii_Aleutian metazone | 
| JDK-8383830 | Vector API: internal AbstractVector helper methods exposed in public Javadoc | 
| JDK-8383832 | AOTMode=required fails with "incompatible CompressedOops::base()" | 
| JDK-8383834 | x86_64: Client build fails after JDK-8379706 | 
| JDK-8383847 | Shenandoah: gc/TestJNIWeak/TestJNIWeak.java times out intermittently | 
| JDK-8383848 | [PPC64] Remove outdated comments for old versions of PPC | 
| JDK-8383850 | Move TestLargeUTF8Length.java to resourcehogs | 
| JDK-8383851 | TypeInstPtr::get_const_boxed_value is unused since JDK-8149813 | 
| JDK-8383856 | Implement JEP 523: Make G1 the Default Garbage Collector in All Environments | 
| JDK-8383861 | Shenandoah: Use int[] as filling class for compressed oop array allocation | 
| JDK-8383864 | G1: Separate worker-local refinement data from shared concurrent refinement stats | 
| JDK-8383867 | File.getCanonicalPath drops backslash from UNC path with directory junctions | 
| JDK-8383868 | Separate resourcehogs tests from hotspot_misc | 
| JDK-8383873 | SNIWildcardMatching test logs an error instead of failing | 
| JDK-8383881 | Cleanup cpu feature enums and test functions for features that should always be available on x86-64 platforms | 
| JDK-8383892 | Shenandoah: Decouple allocation rate sampling from GC cycle | 
| JDK-8383909 | Unused ThreadDumper.BoundedByteArrayOutputStream.write(int b) is wrong | 
| JDK-8383910 | klass_or_null_acquire asserts on null when using compact object headers | 
| JDK-8383912 | Deduplicate klass fetching and decoding in oopDesc | 
| JDK-8383914 | [PPC] cleanup dtrace leftovers | 
| JDK-8383954 | Fix tearing-prone nmethod bitfields | 
| JDK-8383990 | Remove exception code filters from Windows AArch64 safefetch handler | 
| JDK-8384042 | [BACKOUT] Incorrect handling of Hawaii_Aleutian metazone | 
| JDK-8384043 | [REDO] Incorrect handling of Hawaii_Aleutian metazone | 
| JDK-8384072 | Problem list runtime/Thread/TestAlwaysPreTouchStacks.java | 
| JDK-8384080 | Shenandoah: Simplify C1 CAS barriers | 
| JDK-8384085 | Make os::Bsd::dlopen_helper private, and drop unused arg | 
| JDK-8384087 | Test tools/jpackage/junit/share/jdk.jpackage/jdk/jpackage/internal/cli/MainTest.java fails | 
| JDK-8384088 | osThread _thread_id should be an int on all BSD’s | 
| JDK-8384089 | Correctly catch invalid uses of StubCodeMark | 
| JDK-8384097 | G1: G1ConcurrentMarkThread::total_mark_cpu_time_s adds together nanoseconds and seconds | 
| JDK-8384098 | Fix typo in "elasped_time" in Universe::log_cpu_time() | 
| JDK-8384100 | UNSIGNED5::Reader::try_skip() is missing a terminating condition | 
| JDK-8384104 | Obsolete Xmaxjitcodesize | 
| JDK-8384124 | TestCompilerCounts.java fails CICompilerCount is too large | 
| JDK-8384131 | Lowering of array enhanced for loop misses a synthetic cast | 
| JDK-8384132 | javac: Cannot read field "sym" because "this.lvar[1]" is null | 
| JDK-8384143 | ZGC: Add minor performance improvment to oop iteration during flip promotion | 
| JDK-8384145 | Use the default collector in JDK builds | 
| JDK-8384158 | GHA: Downgrade Windows GHA runners to windows-2022 temporarily | 
| JDK-8384161 | [PPC64] Consolidate code related to calls in nmethods that use trampoline stubs | 
| JDK-8384163 | (so) SocketChannel.connect and finishConnect() exception messages could be improved | 
| JDK-8384164 | JFR: Omit command-line arguments for jdk.SystemProcess | 
| JDK-8384182 | Incorrect assertion eq4a_state in vectorization AlignmentSolver::solve | 
| JDK-8384206 | Remove AppContext from java.awt.Container | 
| JDK-8384208 | Remove AppContext from java.awt.Taskbar | 
| JDK-8384209 | Remove AppContext from AWT Shutdown implementation | 
| JDK-8384215 | Remove AppContext from unix/classes/sun/awt/X11/XWindow.java | 
| JDK-8384221 | GenShen: WhiteBox full GC promotion is not reliable | 
| JDK-8384223 | RISC-V: entry_barrier_offset should consider UseZtso | 
| JDK-8384228 | Cleanup compiler/arguments/TestCompilerCounts.java | 
| JDK-8384229 | Previously accepted code is rejected after JDK-8378102 | 
| JDK-8384235 | javac crashes with AssertionError: Cannot add metadata to a void type when annotating void as a generic bound | 
| JDK-8384250 | [macos] Test tools/jpackage/share/AsyncTest.java failed | 
| JDK-8384251 | Test java/lang/instrument/GetObjectSizeIntrinsicsTest.java crashed: fatal error: Not compilable at tier 1: CodeBuffer overflow | 
| JDK-8384274 | Shenandoah: GC checks in C1 LRB clobber fastpath result | 
| JDK-8384277 | Internal error happened when building fastdebug with LTO | 
| JDK-8384278 | Potential deadlock after JDK-8373839 | 
| JDK-8384281 | [BACKOUT] C2: Deep recursion with cmovP_regNode::bottom_type | 
| JDK-8384284 | [REDO] C2: Deep recursion with cmovP_regNode::bottom_type | 
| JDK-8384353 | SHA3 AVX2 and AVX512 intrinsics and improvements | 
| JDK-8384371 | CngCipher and DupAlias tests fail on Windows | 
| JDK-8384381 | Remove AppContext from java.awt.EventQueue implementation | 
| JDK-8384416 | Crash in RunTimeClassInfo::klass() during AOT cache assembly | 
| JDK-8384428 | C2: compiler/vectorapi/TestVectorLongToMaskNodeIdealization.java fails with AVX2 | 
| JDK-8384441 | Problemlist compiler/vectorapi/TestVectorLongToMaskNodeIdealization.java | 
| JDK-8384442 | Shenandoah: Remove vestigial CAS barrier code | 
| JDK-8384466 | Test io/Console/RestoreEchoTest.java fails with expect: spawn id exp3 not open | 
| JDK-8384486 | NTLM tests fail on Windows 11 and Windows Server 2025 | 
| JDK-8384495 | Update Libpng to 1.6.58 | 
| JDK-8384505 | Account for all jtreg-based frames in stack walk test | 
| JDK-8384507 | Incorrect vector reassociation for signed saturating addition | 
| JDK-8384514 | Shenandoah: gc/shenandoah/mxbeans/TestCycleEndMessage.java intermittent fails | 
| JDK-8384520 | AArch64: Prevent out-of-CodeBuffer read in MacroAssembler::membar | 
| JDK-8384553 | ProblemList runtime/jni/critical/SuspendInCritical.java for virtual threads | 
| JDK-8384602 | Remove AppContext from java.awt.Component | 
| JDK-8384604 | Shenandoah: Completed GC count doesn’t add up in the log | 
| JDK-8384609 | Remove old CHA implementation | 
| JDK-8384631 | SwitchBoostraps.typeSwitch method Javadoc still mentions target of a reference type | 
| JDK-8384636 | Parallel: Remove redundant PSAdjustTask claim counter initialization loop | 
| JDK-8384648 | LazyConstantTest::interruptStatusIsPreservedForComputingThread fails | 
| JDK-8384653 | Potential null pointer dereference in java.base/share/classes/jdk/internal/classfile/impl/StackMapGenerator.java | 
| JDK-8384708 | Enhance HTTP Connections | 
| JDK-8384801 | Shenandoah: ShouldNotReachHere() in shenandoahSupport.cpp with AllocatePrefetchStyle=3 | 
| JDK-8384804 | JMX remote bootstrap tests fail on Windows | 
| JDK-8384808 | Remove AppContext-based Window Activation time map from SunToolkit | 
| JDK-8384815 | SelectOneKeyOutOfMany and PreferredKey fail after expired test certificate | 
| JDK-8384817 | Deprecate AlwaysCompileLoopMethods VM flag | 
| JDK-8384835 | Parallel: Removing unused local variable in ParallelCompactData::clear_range | 
| JDK-8384843 | Duplicated type annotations in classfile | 
| JDK-8384862 | CompilationLog::log_failure() does not produce report message | 
| JDK-8384864 | Problemlist java/nio/channels/SocketChannel/OpenLeak.java and CloseDuringConnect.java on macos 26.4 and 26.5 | 
| JDK-8384869 | C2: IR Verification Test failures on x86_64 after JDK-8383881 | 
| JDK-8384902 | Update GIFlib to 6.1.3 | 
| JDK-8384940 | Open source accessibility tests | 
| JDK-8384946 | os::Bsd::gettid misses the return value | 
| JDK-8384948 | Disable DelayAfterInliningCutoff until memory consumption issues are fixed | 
| JDK-8384958 | Regression of jvmti/init/GetAllStackTraces/getallstktr001.java caused by JDK-8382273 | 
| JDK-8384960 | Convert MallocSiteTable to use Atomic<T> | 
| JDK-8384964 | Add an entry count to the MallocSiteTable | 
| JDK-8384997 | "Unrecognized system!" failures on MSys2/Windows | 
| JDK-8384999 | Rename CodeBuffer::last_insn() to something more descriptive | 
| JDK-8385011 | GHA: Enable Linux AArch64 tests | 
| JDK-8385013 | Startup regression ~30% / 7-10ms with noop | 
| JDK-8385018 | Consistently escape CLDR resources | 
| JDK-8385019 | Build error without CDS | 
| JDK-8385022 | ZGC: Crash with sparse NUMA node configuration | 
| JDK-8385054 | Spec: Clarify ofLazy List/Set/Map equals/hashCode/toString Javadoc | 
| JDK-8385076 | Follow-on fix for Improve certification checking | 
| JDK-8385092 | KeyChainStore entry creation time is always now | 
| JDK-8385096 | Test runtime/cds/MetaspaceAllocGaps.java fails on Windows | 
| JDK-8385110 | JFR: Race when accessing Recording::getSettings() | 
| JDK-8385114 | Prevent creation of invalid TableSwitchInstruction | 
| JDK-8385116 | nmethod::Flags is not guaranteed to fit uint8_t | 
| JDK-8385137 | C2: Uncast arguments to MemNode::all_control_dominate in some cases | 
| JDK-8385165 | [ubsan] TestDockerMemoryMetricsSubgroup.java jtreg test fails with ubsan-enabled binaries | 
| JDK-8385179 | Shenandoah: Simplify and optimize string deduplication | 
| JDK-8385180 | Shenandoah: Load klass once in marking task | 
| JDK-8385309 | Test jdk/jfr/event/gc/collection/TestGCEventMixedWithParallelOld.java failed | 
| JDK-8385312 | Intermittent assert while running JCK test api/java_util/concurrent/Assorted/SynchronousQueue20.html | 
| JDK-8385319 | AOT: Missed BAD_ADDRESS_ID spots in JDK-8357175 | 
| JDK-8385333 | Shenandoah: Final mark spins up workers without work to do | 
| JDK-8385334 | Shenandoah: Improve gc+stats logging | 
| JDK-8385345 | AWT and ImageIO shell tests fail when run using MSys/MinGW | 
| JDK-8385347 | Remove AppContext from SunToolkit and also remove AppContext | 
| JDK-8385355 | NullPointerException in jdk.tools.jlink.internal.ImageResourcesTree after JDK-8377070 | 
| JDK-8385369 | G1: Concurrent Cleanup For Next Mark accesses uncommitted bitmaps after region uncommit | 
| JDK-8385375 | Shenandoah: Sampled allocation rate is too slow to react to lower allocation rates | 
| JDK-8385390 | Update FreeType to 2.14.3 | 
| JDK-8385408 | C2: no reachable node should have no use | 
| JDK-8385409 | Javap documentation says by default protected and public members are printed but package private members are also printed by default (`-package` behavior, not`-protected` ) | 
| JDK-8385425 | AArch64: Comment re set_and_get_current_sve_vector_length() is misleading | 
| JDK-8385430 | Shenandoah: Compact heuristic runs continuously when allocation is idle | 
| JDK-8385437 | Provide test property vm.cds.nocoh.archive.available and use it in CompressedCPUSpecificClassSpaceReservation | 
| JDK-8385490 | Update HarfBuzz to 14.2.0 | 
| JDK-8385525 | Suggest Metal toolchain download in configure error on Xcode 26+ | 
| JDK-8385528 | Fix linker alignment warning for libawt on macOS with Xcode 26 | 
| JDK-8385552 | Improve devkit/sysroot creation | 
| JDK-8385557 | Shenandoah: Runtime calls from C2 stubs need to be relocatable | 
| JDK-8385572 | [PPC64] frame alignment assertion after JDK-6960970 | 
| JDK-8385574 | JFR: Redaction should check file | 
| JDK-8385583 | RISC-V: Debugger very slow during stepping | 
| JDK-8385584 | CAInterop.java#buypassclass3ca fails with Intermediate Root CA not found in the chain | 
| JDK-8385606 | Shenandoah: Remove PLAB card-table alignment and per-object register_object during old-gen allocation | 
| JDK-8385610 | Shenandoah: TestSoftMaxHeapSizeAvailableCalc should have more tolerance for other GC triggers | 
| JDK-8385622 | [s390x] Debugger very slow during stepping | 
| JDK-8385627 | Typo in JMXServiceURL.java: misssing | 
| JDK-8385628 | Typo in jdk.attach comments | 
| JDK-8385632 | ZGC: Incorrect object undo in relocation race for relocation workers | 
| JDK-8385633 | PPC64: Shenandoah weak CAS fails after late barrier expansion | 
| JDK-8385648 | PPC64: Improve receiver type profiling reliability | 
| JDK-8385651 | HotCodeSampler crashes with JFR enabled | 
| JDK-8385655 | Timeout in java/lang/Thread/virtual/KlassInit.java | 
| JDK-8385656 | Misplaced comment on enum value in jdk.internal.util.Architecture | 
| JDK-8385658 | Debug code in frame::patch_pc() doesn’t initialize on_heap info | 
| JDK-8385661 | jvmti/vthread/ThreadStateTest/ThreadStateTest.java triggers assert(f.pc() == _chunk→pc()) failed | 
| JDK-8385664 | Stale comment about JDK-6996415 | 
| JDK-8385670 | Incorrect warning: Unexpected gaps for Klass alignment | 
| JDK-8385702 | Improve polymorphic handling in JDK-8385648 | 
| JDK-8385723 | Intermittent failure of serviceability/sa/ClhsdbInspect.java | 
| JDK-8385736 | Optimize ListFormat custom pattern parsing | 
| JDK-8385738 | Javadoc does not produce reproducible output due to the snippet ids | 
| JDK-8385747 | Test gc/g1/TestGCLogMessages.java failed: 'Update Derived Pointers' found in stdout | 
| JDK-8385806 | Assert failed when running Skynet.java with continuation trace logging enabled | 
| JDK-8385828 | The javac benchmarks fail after JDK-8385347 | 
| JDK-8385830 | ForkJoinTask#get may swallow caller thread’s interrupt flag | 
| JDK-8385831 | Prevent APX instruction being generated on non-APX machines | 
| JDK-8385833 | C2 Vector API: assert(false) failed: infinite loop in PhaseIterGVN::transform_old | 
| JDK-8385835 | Shenandoah: Polish C2 pre/post barrier signatures | 
| JDK-8385836 | Update comment in SourceVersion for language evolution history for changes in 27 | 
| JDK-8385855 | C2: IfNode::filtered_int_type should only allow CmpI | 
| JDK-8385885 | Clarify comment in LazyConstantImpl | 
| JDK-8385891 | Introduce a test for GZIPInputStream whose underlying stream is a blocking InputStream | 
| JDK-8385906 | DirPermissionDenied.java uses chmod instead of Java APIs for changing permissions | 
| JDK-8385915 | RISC-V: Remove unused cmpxchg* methods | 
| JDK-8385924 | GZIPInputStream.read() behaves differently on some Java versions | 
| JDK-8385927 | JDK 27 RDP1 L10n resource files update | 
| JDK-8385957 | JFR: Sensitive command-line arguments still in environment variable values | 
| JDK-8385978 | Test javax/net/ssl/SSLSession/TestEnabledProtocols.java failed: java.security.cert.CertificateException: Unable to initialize, java.io.IOException: Too short | 
| JDK-8386085 | Livelock in AbstractQueuedSyncronizer.cleanQueue() when multiple threads do tryAcquire() with a short timeout and no permits available | 
| JDK-8386124 | Test serviceability/sa/TestG1HeapRegion.java failed: Address of G1HeapRegion does not match | 
| JDK-8386155 | C2 Vector API: missing truncation in VectorNode::push_through_replicate | 
| JDK-8386205 | Enhance TLS server | 
| JDK-8386287 | GenShen: assert(starts_object(card_index)) failed: Can’t get last start because no object starts here | 
| JDK-8386298 | Improve font loading | 
| JDK-8386328 | TemporalField.adjustInto Javadoc incorrectly uses 1-argument method signatures | 
| JDK-8386332 | G1: Cleanup pause incorrectly updates old gen MemoryPoolMXBean.getCollectionUsage() | 
| JDK-8386466 | DESedeKeySpec.isParityAdjusted spec permits 8-byte key but RI throws InvalidKeyException | 
| JDK-8386473 | DESKeySpec and DESedeKeySpec may throw InvalidKeyException instead of ArrayIndexOutOfBoundsException for Integer.MIN_VALUE offset | 
| JDK-8386475 | C2 x64: -XX:-UseBMI2Instructions is broken for AVX-512 | 
| JDK-8386482 | C2 CountedLoopConverter::filtered_type_from_dominators: assert(_base == Int) failed: Not an Int | 
| JDK-8386503 | C2: assert(adr_type == nullptr \|\| adr_type→isa_aryptr() != nullptr) failed: unexpected type-unsafe store | 
| JDK-8386551 | Windows build broken because of MSys2/Make update | 
| JDK-8386595 | Two java/net/MulticastSocket tests fail on machines with no ipv6 address other than loopback | 
| JDK-8386600 | Fix comparison checks in DHasKEM | 
| JDK-8386609 | WinL10nTest.java does not respect locale for WixType | 
| JDK-8386656 | C2 AVX512: -XX:-UseCountTrailingZerosInstruction causes assert(UseCountTrailingZerosInstruction) failed: tzcnt instruction not supported | 
| JDK-8386671 | Raster factory methods fail to throw specified exceptions for invalid bandOffsets and bankIndices | 
| JDK-8386681 | Remove RawKeySpec | 
| JDK-8386700 | Class-File API: StackMapGenerator.setLocalsFromArg leaves stale locals, generating invalid stack maps | 
| JDK-8386707 | [BACKOUT] ZGC: Incorrect object undo in relocation race for relocation workers | 
| JDK-8386795 | Swing specification needs caveats on L&F rendering behaviors | 
| JDK-8386798 | Shenandoah: Missing load barrier when making assertions about mark bitmap | 
| JDK-8386865 | Fix links in JDK 27 JavaDoc API documentation | 
| JDK-8386911 | Crypto benchmark regressions after JDK-8384353 | 
| JDK-8387002 | Test ManualTests/JPackage/JPKG001/JPKG001_004: CommonLicenseTest fails on Windows because the license agreement text is not displayed | 
| JDK-8387012 | C2: PhaseVector::expand_vunbox_node should not inject the payload type into the load | 
| JDK-8387015 | C2: crash with "named projection 2 not found" from ArrayCopyNode::finish_transform() for clone | 
| JDK-8387017 | java/lang/instrument/GetObjectSizeIntrinsicsTest.java fails with Error evaluating expression: invalid boolean value: `null' for expression `vm.opt.VerifyOops' | 
| JDK-8387041 | Add a URL link to BCP 47 in the Locale class | 
| JDK-8387042 | Shenandoah: Build time regression with LBE | 
| JDK-8387073 | C2: StoreNode::Ideal wrongly optimizes away scalar store before masked vector store | 
| JDK-8387206 | G1: Code root verification crashes because of stale table scanner | 
| JDK-8387215 | On-demand attribution of a record constructor body causes javac to emit an invalid diagnostic | 
| JDK-8387259 | Clarify extlang in Locale composition description | 
| JDK-8387306 | Replace InputStream#read(byte[]) with InputStream#readNBytes(int) in RtfConverter.isRtfFile() | 
| JDK-8387328 | C2: A Phi must not have a narrower Type than its inputs | 
| JDK-8387395 | [REDO] C2: SIGSEGV in compiled code due to missing ctrl | 
| JDK-8387411 | C2: assertin_vt→isa_pvectmask() == nullptr) == (vt→isa_pvectmask() == nullptr failed: Both BVectMask, or both NVectMask, or both PVectMask | 
| JDK-8387722 | The race during initialization or lazy constants in and Map/Set | 
| JDK-8387757 | Man pages still say CompactObjectHeaders is not default | 
| JDK-8388183 | Wrong example in documentation for String.toLowerCase() | 
| JDK-8388449 | GenShen: Degenerated cycle could be skipped | 
| JDK-8388523 | C2: Test compiler/igvn/TestMaskedStoreIdealization.java failed | 
| JDK-8388539 | [TESTBUG] compiler/igvn/TestMaskedStoreIdealization.java fails on Release Builds | 
| JDK-8388809 | Shenandoah: Control thread may observe incorrect cancellation reason | 
| JDK-8389473 | Remove EA from the JDK 27 version string with RC/GAC promotion | 
| JDK-8390590 | [BACKOUT] C2: Fix the memory around some intrinsics nodes | 
| JDK-8390591 | Add regression test for JDK-8390590 | 

### JFX issues

This is the list of JFX issues fixed in this release.

| Issue ID | Summary | 
|---|---|
| JDK-8154847 | Rendering is incorrect or not visible with StageStyle.UNIFIED on some graphics cards | 
| JDK-8181411 | Performance problem with TreeTableView selectAll() | 
| JDK-8195614 | FilteredList throws ArrayIndexOutOfBoundsException if created with 1 element | 
| JDK-8195750 | FilteredList throws ArrayIndexOutOfBoundsException on ListChangeEvent with multiple change | 
| JDK-8202066 | ListView with large number of rows is extremly slow when changing current multiple selection to a subset | 
| JDK-8208758 | ListChangeBuilder doesn’t shift indices after idempotent change pairs | 
| JDK-8237018 | Unstable test RenderLock1Test always fails on Windows | 
| JDK-8237868 | java.lang.IndexOutOfBoundsException in FilteredList | 
| JDK-8253769 | MouseEventFirer: must not fire event with incorrect coordinates | 
| JDK-8256142 | TreeView: initial focused item incorrect | 
| JDK-8263959 | Unexpected disable behaviour on macOS MenuBar | 
| JDK-8268657 | Looked-up color fails for -fx-background-color in JavaFX CSS file | 
| JDK-8274928 | JavaFX virtual keyboard missing ^ character | 
| JDK-8293356 | Testbug: broken usage of MouseEventFirer/VirtualFlowTestUtils | 
| JDK-8301283 | Util methods for computing text/width height giving up some performance | 
| JDK-8303060 | ChoiceBox: adding ~1000 items takes a long time | 
| JDK-8311505 | Deselection of TableView rows is slow | 
| JDK-8318860 | [test bug] Add tests to verify the caret positions | 
| JDK-8335541 | [macOS] Help system menu won’t hide or it is shown twice | 
| JDK-8338145 | ComboBox popup is in wrong location on first showing | 
| JDK-8339270 | GHA: build on Linux / aarch64 | 
| JDK-8340564 | [macOS] Intermittent build failure in native font with parallel compilation | 
| JDK-8341852 | Fix potential threading issue in WebView’s RTImage | 
| JDK-8342733 | Remove references to compiler flags /D_STATIC_CPPLIB and /D_DISABLE_DEPRECATE_STATIC_CPPLIB in win.gradle | 
| JDK-8343133 | Create implementation of NSAccessibilityNavigableStaticText protocol | 
| JDK-8344109 | FXMLLoader: Check and remove unused callerClass field and argument | 
| JDK-8351094 | macOS: MenuBar with custom menu items disappears | 
| JDK-8352298 | FileChooser: inconsistent behavior with setInitialName() | 
| JDK-8356042 | RichTextArea: tab stops attributes | 
| JDK-8357625 | Build should generate platform specific shaders | 
| JDK-8358823 | Improve documentation of custom header bars | 
| JDK-8359020 | TabObservableList.reorder changes content of filtered list | 
| JDK-8361719 | Enhance Handling of URIs | 
| JDK-8362535 | Update libxslt support | 
| JDK-8364149 | Conditional stylesheet imports | 
| JDK-8364680 | HelloWebView demo crashes when msn webpage is scrolled | 
| JDK-8365938 | FileChooser: initialDirectory must be a valid folder | 
| JDK-8366198 | RichTextArea: embedded image, text background, wavy underline attributes | 
| JDK-8368572 | Update WebKit to 623.1 | 
| JDK-8368600 | Missing "/DELAYLOAD:shlwapi.dll" in win.gradle | 
| JDK-8368704 | Better glyph handling | 
| JDK-8369018 | [TestBug] Monkey Tester Application Update 7 | 
| JDK-8369348 | Failed assertion with scissor rect when Metal API Validation is enabled | 
| JDK-8370902 | RichTextArea: migrate to new text APIs | 
| JDK-8372398 | [macOS] Shown ContextMenu does not close when macOS system menu is clicked | 
| JDK-8373091 | Make Metal the default JavaFX rendering pipeline for macOS | 
| JDK-8373527 | Improve Graphics playback | 
| JDK-8373688 | Wrong render scale is used if Window is on another screen when Scene is sized | 
| JDK-8373936 | RichEditorDemoApp enhancements | 
| JDK-8374630 | Replace GtkFileChooserDialog with GtkFileChooserNative to allow for better Flatpak integration | 
| JDK-8374804 | ConditionalFeature media queries | 
| JDK-8374809 | [RichTextArea] accessibility | 
| JDK-8374822 | Platform media query | 
| JDK-8375016 | Several catch blocks for NullPointerExceptions exist in the codebase | 
| JDK-8375067 | Change JavaFX release version to 27 | 
| JDK-8375070 | NPE in Scene.ClickGenerator::preProcess when mouse button is none | 
| JDK-8375084 | Update libxslt to 1.1.45 | 
| JDK-8375225 | WebIObserverTest fails with WebKit 623.1 | 
| JDK-8375227 | Silent OOBE in NGGroup::renderContent | 
| JDK-8375363 | StyleHelper.resetToInitialValues() interrupts animated properties | 
| JDK-8375444 | MenuButton - textTruncated Property is always false | 
| JDK-8375466 | Metal rendering pipeline crashes on virtualized OS | 
| JDK-8375561 | Class NGGroup is in need of some cleanup | 
| JDK-8376138 | RichTextArea: getModelStyleAttrs might return wrong attributes | 
| JDK-8376282 | [linux, macos] JavaFX fails to build WebKit in DebugNative | 
| JDK-8376492 | NullPointer in ContextMenu sub-menu when graphic and style classes are changed while the menu is open | 
| JDK-8376601 | Bump minimum JDK version for JavaFX to JDK 25 | 
| JDK-8376735 | Update Eclipse projects to use JDK25 | 
| JDK-8376738 | Warnings shown when running jfx.incubator.richtext unit tests | 
| JDK-8376836 | Fix mistakes in FX API docs | 
| JDK-8377099 | Additional WebKit 623.1 fixes from WebKitGTK 2.50.4 | 
| JDK-8377153 | JavaFX FlowPane layout causing improper wrapping of TextFlow nodes with max-width styling | 
| JDK-8377185 | [iOS] Native code compiler warnings and error building libprism_es2 | 
| JDK-8377189 | Intermittent failure in TooltipTest.testCssStylesheetChangeTooltip | 
| JDK-8377316 | [iOS] App crashes due to GlassThreadDataKey not initialized | 
| JDK-8377350 | [iOS] Add support for UIWindowScene | 
| JDK-8377393 | RichTextArea: failed to export null color attribute value | 
| JDK-8377427 | Reduce substring allocations in Color.web(String, double) | 
| JDK-8377706 | [iOS] UI API called on a background thread: UIView layer | 
| JDK-8377904 | Replace Double.parseDouble() with CssNumberParser | 
| JDK-8377930 | Additional WebKit 623.1 fixes from WebKitGTK 2.50.5 | 
| JDK-8378034 | Add licenses for gcc 14.2.0 | 
| JDK-8378226 | Animated GIFs do not animate after WebKit 620.1 update | 
| JDK-8378277 | Improve Editor selection | 
| JDK-8378359 | Remove unused local variable in ViewContainer::HandleViewMouseEvent | 
| JDK-8378366 | Accessibility issues in CSS Reference Guide and Introduction to FXML | 
| JDK-8378383 | Create release notes for JavaFX 26 | 
| JDK-8378507 | JavaFX CSS Reference skips heading levels: <h2> followed by <h4> | 
| JDK-8378510 | Provide media support for libavcodec version 62 | 
| JDK-8378895 | Reduce object allocations in Renderer.getPeerInstance() | 
| JDK-8378970 | Don’t use exceptions for flow control in CssParser.colorValueOfString() | 
| JDK-8379150 | [macos] [a11y] Create implementation of accessibility protocol for hyperlink | 
| JDK-8379206 | 4 Null pointer dereference defect groups in 4 glib files | 
| JDK-8379207 | Improve audio conversion | 
| JDK-8379209 | Uninitialised variable in pathApplierFunctionFast of coretext.c | 
| JDK-8379210 | Null pointer dereference in gst_value_compare_fraction of gstvalue.c | 
| JDK-8379211 | Uninitialised memory in Java_com_sun_javafx_font_freetype_OSFreetype_FT_1Outline_1Decompose | 
| JDK-8379213 | 3 Null pointer dereference defect groups in dlmalloc.c | 
| JDK-8379257 | Update JPEG Image Decoding Software to 10 | 
| JDK-8379336 | Update libxml2 to 2.15.2 | 
| JDK-8379561 | Remove UNICODE related code from GLib | 
| JDK-8379662 | TreeTableView: initial focused item incorrect | 
| JDK-8379823 | Doc files should not have a + in the filename | 
| JDK-8380216 | [macos] Missing dispatch_release in the GlassAplication class | 
| JDK-8380308 | TreeView: selection of many rows is slow | 
| JDK-8380392 | In Ensemble sample for WebView one unable to open https address | 
| JDK-8380557 | Additional WebKit 623.1 fixes from WebKitGTK 2.50.6 | 
| JDK-8380574 | [TestBug] MouseEventFirer should not create a temporary Stage | 
| JDK-8380926 | HeavyweightDialog does not check for a valid stage | 
| JDK-8380933 | TableView sorting very slow when many cells selected | 
| JDK-8380935 | TextFieldSkin caret width calculation is broken | 
| JDK-8381447 | Remove G_DISABLE_CHECKS compiler flag on Windows to align GLib/GStreamer compilation with macOS/Linux | 
| JDK-8381508 | Default package used in GifImageTestApp.java | 
| JDK-8381517 | GlassViewDelegate::convertNSStringToJString can return uninitialized value | 
| JDK-8381578 | Skip failing SystemMenuBarHelpMenuTest | 
| JDK-8381982 | Add pull request template | 
| JDK-8382046 | GHA: Update to wrapper-validation@v6, checkout@v6, cache@v5 | 
| JDK-8382049 | [TestBug] Multiple StubToolkit checks in JavaFX production code | 
| JDK-8382165 | Emojis used in Stage.setTitle are not rendered and break application switching on Linux platform | 
| JDK-8382175 | RichTextAreaDemo is not built by "gradle apps" | 
| JDK-8382256 | Unused method in TreeUtil can be cleaned up | 
| JDK-8382437 | Update eclipse config for jdk26 | 
| JDK-8382883 | JavaFX on Linux crashed after some time when ES2Context.makeCurrent() was called | 
| JDK-8382886 | Running a JavaFX build may not work due to duplicate entries of swt-debug.jar in the gradle cache | 
| JDK-8383092 | Enhance WebView Resource Loading | 
| JDK-8383129 | Better Handling of MP4 Files | 
| JDK-8383143 | Enhance Playlist Loading | 
| JDK-8383783 | MediaQueryParser should reject query lists without comma separator | 
| JDK-8384006 | ComboBox text does not update on String converter update | 
| JDK-8384522 | Update libxml2 to 2.15.3 | 
| JDK-8384806 | ComboBox converter does not properly update contained null values | 
| JDK-8384809 | Update GStreamer to 1.28.3 | 
| JDK-8384814 | [macOS] Trackpad scroll gestures suddenly stop | 
| JDK-8385459 | Animations should respect reducedMotion preference | 
| JDK-8385533 | Cell "focused" PseudoClass handling is unnecessary | 
| JDK-8385666 | Fix for JDK-8384806 broke combo box cell graphics | 
| JDK-8385737 | TextLayoutUtilsContractTest fails when run with JDK 26 or later | 
| JDK-8385956 | Skip failing DialogRepeatedShowHideTest on Wayland | 
| JDK-8385959 | Prompt text should be visible in empty focused TextInputControls | 
| JDK-8386590 | GridPane - content-biased child spanning multiple columns/rows is sized ignoring hgap/vgap | 
| JDK-8386617 | JavaFX controls in the title bar | 
| JDK-8386698 | PlatformUtil has unused fields and methods | 
| JDK-8387020 | StyleManagerTest#testSetApplicationUserAgentStylesheetFromDataURI does not correctly reset the Application user agent stylesheet | 
| JDK-8387027 | GHA: Update GitHub Actions | 
| JDK-8387119 | Tests for ScrollEvent redirection | 
| JDK-8387337 | GlassClipboard: Replace deprecated hash_map and hash_set | 
| JDK-8387626 | java.lang.NullPointerException: Cannot invoke "com.sun.glass.ui.Window.isMinimized()" because "this.platformWindow" is null | 
| JDK-8387655 | java.lang.NullPointerException: Cannot invoke "java.lang.Boolean.booleanValue()" because "newValue" is null | 
| JDK-8387773 | Missing eclipse project files in tests/manual/systemmenu | 
| JDK-8388140 | [BACKOUT] Looked-up color fails for -fx-background-color in JavaFX CSS file | 
| JDK-8388508 | [macOS] MenuItem graphics in the system menu bar is corrupted or disappears, or JVM crashes in CoreGraphics, under memory pressure | 
| JDK-8388526 | [Windows] Crash in Platform.Preferences refresh on OS theme change when using FXCanvas | 
| JDK-8389093 | Typos in the JavaFX CSS Reference Guide | 
| JDK-8389371 | cssref: explain font properties | 
| JDK-8390279 | Update copyright header for files modified in 2026 | 

## 6. Updates to Third Party Libraries

This is the list of changes in the third party libraries.

| Library | Full name | New Version | Module | JBS number | 
|---|---|---|---|---|
| CLDR | CLDR | 48.2 | core-libs | JDK-8371842 | 
| FreeType | FreeType | 2.14.1 | java.desktop | JDK-8373290 | 
| FreeType | FreeType | 2.14.2 | java.desktop | JDK-8379158 | 
| FreeType | FreeType | 2.14.3 | java.desktop | JDK-8385390 | 
| GIFlib | GIFlib | 6.1.1 | java.desktop | JDK-8379256 | 
| GIFlib | GIFlib | 6.1.2 | java.desktop | JDK-8380078 | 
| GIFlib | GIFlib | 6.1.3 | java.desktop | JDK-8384902 | 
| GStreamer | GStreamer | v1.28.3 | javafx | JDK-8384809 | 
| HarfBuzz | HarfBuzz | 12.3.2 | java.desktop | JDK-8375057 | 
| JPEG | JPEG | 10 | javafx | JDK-8379257 | 
| LCMS | LittleCMS | 2.18 | java.desktop | JDK-8375065 | 
| LCMS | LittleCMS | 2.19.1 | java.desktop | JDK-8383354 | 
| Libpng | Libpng | 1.6.54 | java.desktop | JDK-8375063 | 
| Libpng | Libpng | 1.6.55 | java.desktop | JDK-8377526 | 
| Libpng | Libpng | 1.6.56 | java.desktop | JDK-8380959 | 
| Libpng | Libpng | 1.6.57 | java.desktop | JDK-8382047 | 
| Libpng | Libpng | 1.6.58 | java.desktop | JDK-8384495 | 
| libxml2 | libxml2 | v2.15.3 | javafx | JDK-8384522 | 
| libxslt | libxslt | 1.1.45 | javafx | JDK-8375084 | 
| SLEEF | SLEEF | 3.9.0 | core-libs | JDK-8376602 | 
| WebKit | WeKit | v623.1 | javafx.web | JDK-8368572 | 
| Zlib | Zlib Data Compression Library | 1.3.2 | core-libs | JDK-8378631 | 

## 7. Upgrading to the New Version

To keep your Liberica JDK up-to-date and secure, always upgrade to the newest available version once it is released. To upgrade, install the new version over the previous one. For the installation instructions, see Liberica JDK Installation Guide.
