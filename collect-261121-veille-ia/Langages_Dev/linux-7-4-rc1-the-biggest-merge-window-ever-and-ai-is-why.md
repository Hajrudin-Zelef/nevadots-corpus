---
id: collect-261121-veille-ia/langages-dev/linux-7-4-rc1-the-biggest-merge-window-ever-and-ai-is-why
title: "Linux 7.4-rc1: The Biggest Merge Window Ever, and AI Is Why"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "merge", "window", "kernel", "work", "bugs", "ever", "release"]
source: https://www.bikrambhujel.com.np/2026/11/linux-74-rc1-biggest-merge-window-ever.html
source_anchor: ""
source_lines: [1, 99]
section: "Langages_Dev"
---
Linus Torvalds released Linux 7.4-rc1 on 1 November 2026, and the number that caught everyone's eye wasn't a feature. It was the size. Close to 20,000 commits landed in a two-week merge window, the most in the kernel's history. For most of the last decade a merge window brought in 13,000 to 14,000.

A big part of the reason is AI. Tools that find bugs and suggest fixes are now feeding the kernel faster than ever, and Torvalds says he's glad that work is arriving now rather than piling up later. I run Linux on servers, NAS boxes and VMs at home and at work, so here's what's in 7.4, what the AI flood means, and when you'll actually get this kernel.

## Why this merge window was so big

Every kernel release starts with a two-week merge window, when maintainers send in the new work they've queued up. After that come weekly release candidates that should only fix bugs.

In his announcement, Torvalds gave two reasons for the size. First, developers want their work in 7.4 because many expect it to be the next long-term support kernel. Second, a lot of AI-triggered work, meaning bugs found by AI tools and the fixes that follow, came in during the merge window instead of trickling in later. He said he's "much happier with the flood of AI triggered work" arriving this way, because it's a healthier pattern than AI fixes piling up during the release candidates. He also said we'll see how the rest of the cycle goes.

The coverage from LinuxCompatible and Phoronix puts the numbers side by side:

| Kernel | Merge window size | 
|---|---|
| Typical release, last decade | 13,000 to 14,000 commits | 
| Linux 7.3 (October 2026) | Nearly 16,000 commits | 
| **Linux 7.4-rc1** | **Nearly 20,000 commits** , the largest ever | 

## The AI part, and why maintainers are tired

This is the bigger story behind the release. AI-generated code was estimated at around 17% of kernel patches in September. Some maintainers have said they feel overwhelmed by the volume of AI-generated reports and fixes, and there has been talk of keeping AI patches out of the staging tree.

The picture is mixed. Good AI-found bugs are real bugs, and fixing them makes Linux safer for everyone. But each report still needs a human to check it, and a lot of suggested patches get rewritten before they're accepted. A few days ago I wrote about OpenAI's AI finding 948 Linux kernel bugs. This merge window is what that looks like from the maintainers' side: more work, landing faster.

## What's new in Linux 7.4

### Hardware

- **Apple M4** : initial support, though it's not ready for everyday use yet.
- **AMD Ryzen Zen 6** : early enablement ahead of the chips.
- **Google Tensor G5** and Pixel 10 mainline support.
- More **Qualcomm Snapdragon X2** laptops.

### Graphics

- **AMDGPU** : HDMI 2.1 FRL, FreeSync, ALLM and VRR, plus a performance option aimed at AI workloads on Radeon integrated graphics.
- **Intel Xe** : memory page offlining and VRAM temperature monitoring.

### Performance and filesystems

- **FAT32** is reported to be up to about 265 times faster in some cases, which matters for USB sticks and SD cards.
- The early-boot memory allocator reportedly drops from around 268 seconds to about 50 milliseconds on affected systems.
- **Btrfs** zstd decompression improvements,**ext4** buffered iomap support, and SMB compression you can toggle with`chattr` .
- Scheduler updates for load balancing, NUMA and energy-aware scheduling.

### Networking and security

- **TCP ROCCET** , a CUBIC-based congestion control designed for cellular networks, and hardware pacing offload.
- Composable **Landlock** rules for sandboxing.

TCP ROCCET is the one I'm curious about. A lot of people in Nepal are on mobile data or patchy links. Whether it helps in practice depends on distributions picking it up and on real-world testing, so I'll wait for numbers before getting excited.

## When will you actually get it?

The final Linux 7.4 is expected around **20 December 2026**, or 27 December if an eighth release candidate is needed. After that, rolling distributions such as Arch and Fedora usually ship it within weeks. Ubuntu and Debian stable releases will take longer.

The long-term support question isn't fully settled. Torvalds said developers are aiming for 7.4 as the next LTS kernel, and some coverage reports Greg Kroah-Hartman will maintain it from early 2027. Earlier reports had pointed at 7.3. I'd wait for the official listing on kernel.org before planning around either.

Meanwhile, Linux 7.3 came out on 18 October 2026, and Ubuntu 26.10 ships with it. For stable systems, the 7.3.x series is where to be. Check what you're running with:

```
uname -r
cat /etc/os-release | grep PRETTY_NAME
```
## Should you try 7.4-rc1?

On a test VM, sure, if you want to help find bugs or need early hardware support. On a server, a NAS or anything people depend on, no. Release candidates are for testing, and this one has more new code than any before it. I'd expect the release candidates to be busier than usual.

If you want a safe place to try it, a spare VM is perfect. My notes on running Linux on Windows without WSL and installing Proxmox VE 9.2 cover two ways to set one up.

## A few things to keep in mind

- **Your NAS and hypervisor pick their own kernel.** TrueNAS and Proxmox ship tested kernels with their updates. Don't swap them out yourself.
- **More AI-found bugs means more updates.** Expect more frequent kernel security updates from your distro, and apply them. Restarts after kernel updates matter.
- **Hardware support takes time.** "Initial support" for Apple M4 or Zen 6 is the start of the work, not a finished experience.

If you'd like help keeping a fleet of Linux servers patched and stable, without the drama, that's one of the things I do through my IT and infrastructure services.

## Frequently asked questions

## When was Linux 7.4-rc1 released?

Linus Torvalds released Linux 7.4-rc1 on 1 November 2026, closing a merge window of nearly 20,000 commits, the largest in the kernel's history.

## When will the final Linux 7.4 be released?

The final Linux 7.4 is expected around 20 December 2026, or 27 December 2026 if an eighth release candidate is needed.

## Why was the Linux 7.4 merge window so big?

Torvalds pointed to two reasons: a flood of AI-triggered bug fixes arriving during the merge window, and developers aiming to get their work into 7.4 because it is expected to become the next long-term support kernel.

## Is Linux 7.4 an LTS kernel?

It is expected to be, but it is not official yet. Torvalds said developers are aiming for 7.4 as the next LTS, while earlier reports pointed to 7.3. Check the releases page on kernel.org for the official LTS list.

## Should I install Linux 7.4-rc1 on my server?

No. Release candidates are for testing. Use a spare VM if you want to try it, and keep servers on the 7.3.x stable series or your distribution's supported kernel.
