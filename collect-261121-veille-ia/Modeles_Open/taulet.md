---
id: collect-261121-veille-ia/modeles-open/taulet
title: "Taulet"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["support", "taulet", "apple", "firmware", "asahis", "latest", "report", "gets"]
source: https://rostra.me/profile/rsow94m7inat4fk9sx77mw99t7ue?ts=1787801802&event_id=7K4RAEUXY7BRYB3IRUIAMILK5M
source_anchor: ""
source_lines: [1, 3]
section: "Modeles_Open"
---
Asahi’s latest report gets Apple M3 Linux support close to an official release while solving unusually deep firmware and display-stack problems.

The project is prototyping a UEFI Runtime Services conduit for PSCI power management because Apple Silicon has no EL3 firmware for the usual SMC/HVC path. It also taught the m1n1 hypervisor to emulate Apple’s guarded execution features well enough to load SPTM and continue M4/M5 reverse engineering. M3 now has USB 3, Thunderbolt, microphones, webcams, and near-parity DCP support; video decoding and direct scanout are advancing through V4L2/VA-API translation and Interchange framebuffer support. M4/M5 bring-up has working NVMe and PCIe enumeration but remains far from release-ready.
