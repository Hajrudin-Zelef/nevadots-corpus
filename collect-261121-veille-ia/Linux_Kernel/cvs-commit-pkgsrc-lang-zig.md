---
id: collect-261121-veille-ia/linux-kernel/cvs-commit-pkgsrc-lang-zig
title: "CVS commit: pkgsrc/lang/zig"
domain: linux-kernel
role: reference
task: article
actors: []
dates: ["2026-11-04"]
keywords: ["libc", "include", "any-darwin-any", "pkgsrc", "lang", "makefile", "plist", "distinfo"]
source: https://mail-index.netbsd.org/pkgsrc-changes/2026/11/04/msg358809.html
source_anchor: ""
source_lines: [1, 106]
section: "Linux_Kernel"
---
pkgsrc-Changes archive

[Date Prev][Date Next][Thread Prev][Thread Next][Date Index][Thread Index][Old Index]
# CVS commit: pkgsrc/lang/zig



```
Module Name:    pkgsrc
Committed By:   adam
Date:           Wed Nov  4 07:07:08 UTC 2026
Modified Files:
        pkgsrc/lang/zig: Makefile PLIST distinfo
Log Message:
zig: updated to 0.17.1
0.17.1
Bug fixes
To generate a diff of this commit:
cvs rdiff -u -r1.55 -r1.56 pkgsrc/lang/zig/Makefile
cvs rdiff -u -r1.18 -r1.19 pkgsrc/lang/zig/PLIST
cvs rdiff -u -r1.23 -r1.24 pkgsrc/lang/zig/distinfo
Please note that diffs are not public domain; they are subject to the
copyright notices on the relevant files.
```
```
Modified files:
Index: pkgsrc/lang/zig/Makefile
diff -u pkgsrc/lang/zig/Makefile:1.55 pkgsrc/lang/zig/Makefile:1.56
--- pkgsrc/lang/zig/Makefile:1.55       Sun Oct 11 16:02:21 2026
+++ pkgsrc/lang/zig/Makefile    Wed Nov  4 07:07:08 2026
@@ -1,7 +1,6 @@
-# $NetBSD: Makefile,v 1.55 2026/10/11 16:02:21 wiz Exp $
+# $NetBSD: Makefile,v 1.56 2026/11/04 07:07:08 adam Exp $
 
-DISTNAME=      zig-0.17.0
-PKGREVISION=   1
+DISTNAME=      zig-0.17.1
 CATEGORIES=    lang
 MASTER_SITES=  https://ziglang.org/download/${PKGVERSION_NOREV}/
 EXTRACT_SUFX=  .tar.xz
Index: pkgsrc/lang/zig/PLIST
diff -u pkgsrc/lang/zig/PLIST:1.18 pkgsrc/lang/zig/PLIST:1.19
--- pkgsrc/lang/zig/PLIST:1.18  Sun Oct 11 06:41:44 2026
+++ pkgsrc/lang/zig/PLIST       Wed Nov  4 07:07:08 2026
@@ -1,4 +1,4 @@
-@comment $NetBSD: PLIST,v 1.18 2026/10/11 06:41:44 adam Exp $
+@comment $NetBSD: PLIST,v 1.19 2026/11/04 07:07:08 adam Exp $
 bin/zig
 lib/zig/build-web/fuzz.zig
 lib/zig/build-web/index.html
@@ -258,6 +258,7 @@ lib/zig/compiler_rt/fmin.zig
 lib/zig/compiler_rt/fmod.zig
 lib/zig/compiler_rt/fmodq_test.zig
 lib/zig/compiler_rt/fmodx_test.zig
+lib/zig/compiler_rt/fmv.zig
 lib/zig/compiler_rt/hexagon.zig
 lib/zig/compiler_rt/int.zig
 lib/zig/compiler_rt/int_from_float.zig
@@ -1487,10 +1488,24 @@ lib/zig/libc/include/any-darwin-any/math
 lib/zig/libc/include/any-darwin-any/memory.h
 lib/zig/libc/include/any-darwin-any/monetary.h
 lib/zig/libc/include/any-darwin-any/ndbm.h
+lib/zig/libc/include/any-darwin-any/net/bpf.h
+lib/zig/libc/include/any-darwin-any/net/dlil.h
+lib/zig/libc/include/any-darwin-any/net/ethernet.h
 lib/zig/libc/include/any-darwin-any/net/if.h
+lib/zig/libc/include/any-darwin-any/net/if_arp.h
 lib/zig/libc/include/any-darwin-any/net/if_dl.h
+lib/zig/libc/include/any-darwin-any/net/if_llc.h
+lib/zig/libc/include/any-darwin-any/net/if_media.h
+lib/zig/libc/include/any-darwin-any/net/if_mib.h
+lib/zig/libc/include/any-darwin-any/net/if_types.h
+lib/zig/libc/include/any-darwin-any/net/if_utun.h
 lib/zig/libc/include/any-darwin-any/net/if_var.h
+lib/zig/libc/include/any-darwin-any/net/if_var_status.h
+lib/zig/libc/include/any-darwin-any/net/ipsec_offload.h
+lib/zig/libc/include/any-darwin-any/net/kext_net.h
+lib/zig/libc/include/any-darwin-any/net/ndrv.h
 lib/zig/libc/include/any-darwin-any/net/net_kev.h
+lib/zig/libc/include/any-darwin-any/net/pfkeyv2.h
 lib/zig/libc/include/any-darwin-any/net/route.h
 lib/zig/libc/include/any-darwin-any/netdb.h
 lib/zig/libc/include/any-darwin-any/netinet/in.h
Index: pkgsrc/lang/zig/distinfo
diff -u pkgsrc/lang/zig/distinfo:1.23 pkgsrc/lang/zig/distinfo:1.24
--- pkgsrc/lang/zig/distinfo:1.23       Sun Oct 11 06:41:44 2026
+++ pkgsrc/lang/zig/distinfo    Wed Nov  4 07:07:08 2026
@@ -1,7 +1,7 @@
-$NetBSD: distinfo,v 1.23 2026/10/11 06:41:44 adam Exp $
+$NetBSD: distinfo,v 1.24 2026/11/04 07:07:08 adam Exp $
 
-BLAKE2s (zig-0.17.0.tar.xz) = 143c2ab8b7ad8456dc2a7f53c39a7b4f907baa59e27dd7c48625ec13921cc3f3
-SHA512 (zig-0.17.0.tar.xz) = 49abf3a1ab2eb58e066fa87cd197c47bb36d729bb55d72fd35ff0a65b0f610daa0feec51c064a8c9016ac44c501744b1d3df9935424e4703e71fbf262645b2dd
-Size (zig-0.17.0.tar.xz) = 23049960 bytes
+BLAKE2s (zig-0.17.1.tar.xz) = 586422130f21f65e5f56876b88865879ac8cfd0bf562aa8eaacb35aa0f30f5a2
+SHA512 (zig-0.17.1.tar.xz) = f57a59c86ace6bf14698ba3c8c314cfb8716c0ce5ee6817f4c8f89fea90665a45df3f12fa6cc1fb04d49a4c503f76fe6bedf44b15a51f9110ecd667d9e7183fc
+Size (zig-0.17.1.tar.xz) = 23183076 bytes
 SHA1 (patch-CMakeLists.txt) = bb3561135250f31f0023700bb13625eaafabae88
 SHA1 (patch-build.zig) = 98d6c64e74024312e8e27191103c6c90ad10d840
```


**Home |
Main Index |
Thread Index |
Old Index**
