---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-end-of-life-lts-and-stable-kernel-eol-dates-itechtics
title: "Linux Kernel End of Life: LTS and Stable Kernel EOL Dates - Itechtics"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["kernel", "linux", "ended", "confirmed", "unsupported", "left", "official", "year"]
source: https://www.itechtics.com/eol/linux-kernel/
source_anchor: ""
source_lines: [1, 226]
section: "Linux_Kernel"
---
1 mo left

Linux Kernel 7.3 gets security updates until 18 Jan 2027 (1 mo left). That is less than a year away. This date is our estimate: Non-LTS stable kernels usually get their last update about three months after release, shortly after the next mainline release.

Linus Torvalds releases a new mainline kernel about every nine to ten weeks. Each one becomes a stable branch that gets fixes for about three months, until shortly after the next mainline release. Once a year, usually with the last release of the year, one kernel is picked as a longterm kernel and gets fixes for several years.

The projected end dates for longterm kernels are not fixed. The maintainers extend them when companies commit to help with testing, as happened with 6.1 and 6.6.

Most users get their kernel from a distribution. Distributions such as RHEL, Ubuntu and Debian patch their own kernels for the life of the distribution, so a distribution kernel can stay supported after its kernel.org branch has ended. Civil Infrastructure Platform (CIP) also maintains some kernels for ten years.

## Linux Kernel support dates

| Version | Released | Maintained until | Time left | Status | Latest | 
|---|---|---|---|---|---|
| Linux Kernel 7.3 |  | Estimated[1] | 1 mo left | Ending within a year | 7.3.4 | 
| Linux Kernel 7.2 |  | Confirmed[2] | Ended | Unsupported | 7.2.15 | 
| Linux Kernel 7.1 |  | Confirmed[2] | Ended | Unsupported | 7.1.13 | 
| Linux Kernel 7.0 |  | Confirmed[2] | Ended | Unsupported | 7.0.14 | 
| Linux Kernel 6.19 |  | Confirmed[3] | Ended | Unsupported | 6.19.14 | 
| Linux Kernel 6.18 (LTS) LTS |  | Official[4] | 2 yr 1 mo left | Supported | 6.18.63 | 
| Linux Kernel 6.17 |  | Confirmed[3] | Ended | Unsupported | 6.17.13 | 
| Linux Kernel 6.16 |  | Confirmed[3] | Ended | Unsupported | 6.16.12 | 
| Linux Kernel 6.15 |  | Confirmed[3] | Ended | Unsupported | 6.15.11 | 
| Linux Kernel 6.14 |  | Confirmed[3] | Ended | Unsupported | 6.14.11 | 
| Linux Kernel 6.13 |  | Confirmed[3] | Ended | Unsupported | 6.13.12 | 
| Linux Kernel 6.12 (LTS) LTS |  | Official[4] | 2 yr 1 mo left | Supported | 6.12.117 | 
| Linux Kernel 6.11 |  | Confirmed[3] | Ended | Unsupported | 6.11.11 | 
| Linux Kernel 6.10 |  | Confirmed[3] | Ended | Unsupported | 6.10.14 | 
| Linux Kernel 6.9 |  | Confirmed[3] | Ended | Unsupported | 6.9.12 | 
| Linux Kernel 6.8 |  | Confirmed[3] | Ended | Unsupported | 6.8.12 | 
| Linux Kernel 6.7 |  | Confirmed[3] | Ended | Unsupported | 6.7.12 | 
| Linux Kernel 6.6 (LTS) LTS |  | Official[4] | 1 yr 1 mo left | Supported | 6.6.162 | 
| Linux Kernel 6.5 |  | Confirmed[3] | Ended | Unsupported | 6.5.13 | 
| Linux Kernel 6.4 |  | Confirmed[3] | Ended | Unsupported | 6.4.16 | 
| Linux Kernel 6.3 |  | Confirmed[3] | Ended | Unsupported | 6.3.13 | 
| Linux Kernel 6.2 |  | Confirmed[3] | Ended | Unsupported | 6.2.16 | 
| Linux Kernel 6.1 (LTS) LTS |  | Official[4] | 1 yr 1 mo left | Supported | 6.1.192 | 
| Linux Kernel 6.0 |  | Confirmed[3] | Ended | Unsupported | 6.0.19 | 
| Linux Kernel 5.19 |  | Confirmed[5] | Ended | Unsupported | 5.19.17 | 
| Linux Kernel 5.18 |  | Confirmed[5] | Ended | Unsupported | 5.18.19 | 
| Linux Kernel 5.17 |  | Confirmed[5] | Ended | Unsupported | 5.17.15 | 
| Linux Kernel 5.16 |  | Confirmed[5] | Ended | Unsupported | 5.16.20 | 
| Linux Kernel 5.15 (LTS) LTS |  | Official[4] | 1 mo left | Ending within a year | 5.15.225 | 
| Linux Kernel 5.10 (LTS) LTS |  | Official[4] | 1 mo left | Ending within a year | 5.10.274 | 
| Linux Kernel 5.4 (LTS) LTS |  | Confirmed[5] | Ended | Unsupported | 5.4.302 | 
| Linux Kernel 4.19 (LTS) LTS |  | Confirmed[6] | Ended | Unsupported | 4.19.325 | 
| Linux Kernel 4.14 (LTS) LTS |  | Confirmed[6] | Ended | Unsupported | 4.14.336 | 
| Linux Kernel 4.9 (LTS) LTS |  | Confirmed[6] | Ended | Unsupported | 4.9.337 | 

Official published by the vendor · Confirmed observed (for example, patches stopped) · Estimated our forecast, with the rule stated · How we date things

## Timeline

## What happens when support ends

### Will it still run?

The kernel keeps running. Nothing is switched off.

### Will it get security fixes?

kernel.org stops publishing stable updates for the branch, so new vulnerabilities in it are not fixed upstream.

### What stops working?

New hardware drivers and fixes go only into newer branches. Out-of-tree modules and tools may drop support for old kernels.

### What are my options?

- Use your distribution's supported kernel package.
- Move to a current longterm kernel such as 6.12 or 6.18.
- For embedded devices, consider a CIP or vendor-supported kernel.

## What to do

Run `uname -r` to see your kernel. Update it through your distribution's package manager rather than building from kernel.org, unless you maintain your own kernel.

## Linux Kernel versions: end of support, one by one

### Linux Kernel 7.3 end of life Ending within a year

Linux Kernel 7.3 gets security updates until 18 Jan 2027 (1 mo left). That is less than a year away. This date is our estimate: Non-LTS stable kernels usually get their last update about three months after release, shortly after the next mainline release.

### Linux Kernel 7.2 end of life Unsupported

Linux Kernel 7.2 stopped getting security updates on 1 Nov 2026 and is no longer supported.

### Linux Kernel 7.1 end of life Unsupported

Linux Kernel 7.1 stopped getting security updates on 2 Sep 2026 and is no longer supported.

### Linux Kernel 7.0 end of life Unsupported

Linux Kernel 7.0 stopped getting security updates on 27 Jun 2026 and is no longer supported.

### Linux Kernel 6.19 end of life Unsupported

Linux Kernel 6.19 stopped getting security updates on 22 Apr 2026 and is no longer supported.

### Linux Kernel 6.18 (LTS) end of life Supported

Linux Kernel 6.18 (LTS) gets security updates until 31 Dec 2028 (2 yr 1 mo left). kernel.org lists a projected EOL of Dec 2028 and says projected dates are not set in stone.

### Linux Kernel 6.17 end of life Unsupported

Linux Kernel 6.17 stopped getting security updates on 18 Dec 2025 and is no longer supported.

### Linux Kernel 6.16 end of life Unsupported

Linux Kernel 6.16 stopped getting security updates on 12 Oct 2025 and is no longer supported.

### Linux Kernel 6.15 end of life Unsupported

Linux Kernel 6.15 stopped getting security updates on 20 Aug 2025 and is no longer supported.

### Linux Kernel 6.14 end of life Unsupported

Linux Kernel 6.14 stopped getting security updates on 10 Jun 2025 and is no longer supported.

### Linux Kernel 6.13 end of life Unsupported

Linux Kernel 6.13 stopped getting security updates on 20 Apr 2025 and is no longer supported.

### Linux Kernel 6.12 (LTS) end of life Supported

Linux Kernel 6.12 (LTS) gets security updates until 31 Dec 2028 (2 yr 1 mo left). kernel.org lists a projected EOL of Dec 2028 and says projected dates are not set in stone.

### Linux Kernel 6.11 end of life Unsupported

Linux Kernel 6.11 stopped getting security updates on 5 Dec 2024 and is no longer supported.

### Linux Kernel 6.10 end of life Unsupported

Linux Kernel 6.10 stopped getting security updates on 10 Oct 2024 and is no longer supported.

### Linux Kernel 6.9 end of life Unsupported

Linux Kernel 6.9 stopped getting security updates on 27 Jul 2024 and is no longer supported.

### Linux Kernel 6.8 end of life Unsupported

Linux Kernel 6.8 stopped getting security updates on 30 May 2024 and is no longer supported.

### Linux Kernel 6.7 end of life Unsupported

Linux Kernel 6.7 stopped getting security updates on 3 Apr 2024 and is no longer supported.

### Linux Kernel 6.6 (LTS) end of life Supported

Linux Kernel 6.6 (LTS) gets security updates until 31 Dec 2027 (1 yr 1 mo left).

### Linux Kernel 6.5 end of life Unsupported

Linux Kernel 6.5 stopped getting security updates on 28 Nov 2023 and is no longer supported.

### Linux Kernel 6.4 end of life Unsupported

Linux Kernel 6.4 stopped getting security updates on 13 Sep 2023 and is no longer supported.

## For businesses and auditors

Paste this into a risk register, an audit response or an insurance questionnaire. It updates with the version picked above.

`As of 21 Nov 2026: Linux Kernel 7.3 gets security updates until 18 Jan 2027 (1 mo left). That is less than a year away. This date is our estimate: Non-LTS stable kernels usually get their last update about three months after release, shortly after the next mainline release. Source: https://www.itechtics.com/eol/linux-kernel/, verified 11 Nov 2026.`
## Get Linux Kernel end-of-life alerts

We email you 90, 30 and 7 days before a Linux Kernel version loses support, and when a vendor moves a date. One email at most per day, unsubscribe in one click.

Prefer your calendar? Add Linux Kernel end dates to your calendar (subscribe to the link in Outlook, Google or Apple Calendar).

## The Linux Kernel Organization (kernel.org) support policy

kernel.org maintains each stable branch until shortly after the next mainline release. One kernel per year becomes a longterm branch with projected end dates that the maintainers may extend.

## What changed

- Linux Kernel 4.14 (LTS) · Security updates until: 31 Jan 2024 → 10 Jan 2024
- Linux Kernel 5.4 (LTS) · Security updates until: 31 Dec 2025 → 3 Dec 2025
- Linux Kernel 7.2 reached end of support.
- Linux Kernel 7.3 was released.
- Linux Kernel 7.1 reached end of support.
- Linux Kernel 7.2 was released.
- Linux Kernel 7.0 reached end of support.
- Linux Kernel 7.1 was released.
- Linux Kernel 6.19 reached end of support.
- Linux Kernel 7.0 was released.

## Frequently asked questions

## Which Linux Kernel versions are still supported?

As of 21 Nov 2026, these Linux Kernel versions get security updates: Linux Kernel 7.3, Linux Kernel 6.18 (LTS), Linux Kernel 6.12 (LTS), Linux Kernel 6.6 (LTS), Linux Kernel 6.1 (LTS), Linux Kernel 5.15 (LTS), Linux Kernel 5.10 (LTS).

## When does Linux Kernel 7.3 support end?

Linux Kernel 7.3 gets security updates until 18 Jan 2027 (1 mo left). That is less than a year away. This date is our estimate: Non-LTS stable kernels usually get their last update about three months after release, shortly after the next mainline release.

## What happens when Linux Kernel reaches end of life?

The kernel keeps running. Nothing is switched off. kernel.org stops publishing stable updates for the branch, so new vulnerabilities in it are not fixed upstream.

## Which Linux kernels are LTS?

6.18, 6.12, 6.6, 6.1, 5.15 and 5.10.

## When does Linux 6.1 reach end of life?

kernel.org projects December 2027.

## When does Linux 5.15 reach end of life?

kernel.org projects December 2026.

## How long is a non-LTS kernel supported?

About three months, until shortly after the next mainline release.

## What is the latest Linux kernel?

The stable kernel is 7.3 (7.3.4 on 10 November 2026). 7.4 is in release candidates.

## Sources and method

Dates come from the vendor's own lifecycle pages wherever they exist and are checked against them on a schedule. A date is **Official** when the vendor publishes it, **Confirmed** when we have seen the updates stop, and **Estimated** when the vendor publishes nothing and we forecast it from their past behaviour. The rule is shown next to every estimate. Status and time left are worked out from today's date.

Spotted a wrong date? Tell us and include the source.
