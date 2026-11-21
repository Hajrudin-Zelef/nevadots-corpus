---
id: collect-261121-veille-ia/zig-lang/zig-0-17-1-ignores-make-jobs
title: "zig 0.17.1 ignores MAKE_JOBS"
domain: zig-lang
role: reference
task: article
actors: []
dates: ["2026-11-04"]
keywords: ["index", "ignores", "thread", "make", "jobs", "date", "cores", "zig"]
source: https://mail-index.netbsd.org/tech-pkg/2026/11/04/msg032661.html
source_anchor: ""
source_lines: [1, 23]
section: "Zig_Lang"
---
tech-pkg archive

[Date Prev][Date Next][Thread Prev][Thread Next][Date Index][Thread Index][Old Index]
# zig 0.17.1 ignores MAKE_JOBS



Hi,
I noticed that the latest zig version ignores the 10 cores that I've set for
pkgsrc but rather uses all my 12 cores. I'm pretty sure that this shouldn't
be like that. Also the package takes quite some while for the installation
process (and also all the 12 cores). This seems a bit weird because I thought
that installing something most times is just copying files over to another
directory, I might be wrong about this though.
Best regards,
Ivan (arcathrax)



**Home |
Main Index |
Thread Index |
Old Index**
