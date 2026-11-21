---
id: collect-261121-veille-ia/linux-kernel/arch-linux
title: "Arch Linux"
domain: linux-kernel
role: reference
task: article
actors: []
dates: ["2026-11-08"]
keywords: ["projects", "would", "because", "changes", "needs", "updates", "only", "there"]
source: https://bbs.archlinux.org/viewtopic.php?pid=2313851
source_anchor: ""
source_lines: [1, 34]
section: "Linux_Kernel"
---
Hi!

AFAIK when a new language or building tool is released like LLVM a rebuild is triggered for projects depending on it. Usually on the TODO list. Recently Zig got bumped from 0.17 to 0.17.1. Shouldn't all packages using Zig be rebuilt then?

*Last edited by alosarjos (2026-11-08 16:45:08)*

Offline

Just because a compiler changes doesn't mean everythign compiled with it needs to be updated - llvm and gcc have wider impact because they resp. their libraries are internally used by some other projects (but not everything built w/ gcc needs to be recompiled when gcc updates, most things don't)

Only zsl and cargo-zigbuild hard-depend on zig, both seem to only invoke the executable so if there're no incompatible CLI switch changes they won't require updates at all and in any event the upstream projects would have to update to acommodate to that first.

*zls

I Have Linux Perl Can i Download Gnome???

Just because a compiler changes doesn't mean everythign compiled with it needs to be updated - llvm and gcc have wider impact because they resp. their libraries are internally used by some other projects (but not everything built w/ gcc needs to be recompiled when gcc updates, most things don't)

Only zsl and cargo-zigbuild hard-depend on zig, both seem to only invoke the executable so if there're no incompatible CLI switch changes they won't require updates at all and in any event the upstream projects would have to update to acommodate to that first.


Shouldn't projects like Ghostty, that depends on Zig 0.17, be recompiled with 0.17.1 for potential bugfixes?

I know nothing of Zig, do they use their own version number scheme? Under most version number standards, an increment in a minor version number shouldn't introduce ABI incompatibilities (and therefore would not require rebuilds).

"UNIX is simple and coherent" - Dennis Ritchie; "GNU's Not Unix" - Richard Stallman

The idea would be that zig produced buggy binaries and a rebuild is required to fix those - in which case nothing should™ use zig tbw but there's also be a heads up from zig.

Generally OC new compiler versions might instroduce better optimizations etc. and things might benefit from a rebuild but that as well would certainly not justify or reason any automatism.

Is there a special condition you're worried about?

Nothing in particular, this is more a question on how I thought thinks work where I could be just wrong. Thanks for the answers
