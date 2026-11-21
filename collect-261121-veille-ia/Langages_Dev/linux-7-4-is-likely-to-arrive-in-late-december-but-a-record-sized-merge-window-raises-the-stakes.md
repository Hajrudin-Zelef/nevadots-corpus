---
id: collect-261121-veille-ia/langages-dev/linux-7-4-is-likely-to-arrive-in-late-december-but-a-record-sized-merge-window-raises-the-stakes
title: "Linux 7.4 Is Likely to Arrive in Late December, but a Record-Sized Merge Window Raises the Stakes"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-01"]
keywords: ["window", "december", "merge", "release", "linux", "likely", "late", "torvalds"]
source: https://pbxscience.com/linux-7-4-is-likely-to-arrive-in-late-december-but-a-record-sized-merge-window-raises-the-stakes/
source_anchor: ""
source_lines: [1, 50]
section: "Langages_Dev"
---
## Linux 7.4 Is Likely to Arrive in Late December, but a Record-Sized Merge Window Raises the Stakes

**Linux 7.4’s final release will most likely land around December 20, 2026. If testing turns up more problems than usual, it could slip to the holiday-season fringe or early January.**

## Why late December is the likely date

Kernel releases follow a predictable rhythm. A two-week merge window brings in new features, and then the first release candidate (rc1) closes it. Weekly candidates from rc2 to rc7 are then mostly for bug fixes and stabilization. Linus Torvalds tagged Linux 7.4-rc1 on Sunday, November 1, 2026, and one outlet projects the final release for December 20, 2026, with the caveat that extra candidates could change that.

The calendar works out like this:

- rc1: November 1
- rc2 to rc7: November 8, 15, 22, 29 and December 6 and 13
- Final release: December 20 if seven candidates are enough, or December 27 if an rc8 is needed

Three things support the December estimate:

1. **The schedule is on track.** Phoronix noted that rc1 arrived on time even though Torvalds was on holiday, avoiding the possible one-week delay he had mentioned in the 7.3 announcement. That means no schedule has been lost.
2. **The cadence is steady.** Linux 7.3 arrived on October 18 and 7.4-rc1 followed on November 1. Releases are coming roughly every nine to ten weeks.
3. **Phoronix’s read is optimistic.** It says 7.4 still stands good chances of releasing before the end of December.

The “early January” scenario comes from the holidays and the unusually large volume of changes. Torvalds has stretched cycles with an extra release candidate before, and that is a common outcome when late fixes keep arriving. I’m basing that on how past cycles have gone, not on a statement from him about 7.4.

## The challenge: a record-breaking merge window

The central challenge is size. Reports differ slightly on the exact count, between about 19,700 and 19,800 commits depending on whether merge commits are included. Either way, it is the largest merge window in the kernel’s roughly 35-year history, about 21% above 7.3. That compares with roughly 15,000 commits in a typical window.

Because so much code arrived at once, there is more to test and more places for regressions to hide. One report put the risk bluntly: a big merge window means a lot shipped fast before anyone had a chance to shake out the problems. Release candidates are also, by definition, likely to contain bugs, regressions or incomplete features.

The new code includes support for Apple M4, AMD Ryzen Zen 6 and Google Tensor G5. Support for new hardware like this tends to need extra tuning once real users start testing it.

## Why the window was so big

**AI-assisted development.** Torvalds said the main driver of the size was the “by now obvious” rise in AI-triggered work, and he treated it as a good sign, not a problem. One analysis found that commits declaring AI use rose about 2.4 times in the 7.4 merge window. By the same tally, 16.2% of non-merge commits in the window declared AI involvement, up from 8.2% in 7.3.

Torvalds welcomed the timing. He praised that AI-found fixes now land during the merge window and not in the later stabilization phase. If that holds, the rc period should be calmer than the volume suggests, though the real test comes over the next six weeks.

**The possible LTS factor.** Torvalds also speculated that some of the rush may come from people expecting 7.4 to be the next Long-Term Support release. Developers tend to push their work into a release they expect to be supported for years. That raises the stability bar, because LTS kernels end up underneath enterprise systems, Android devices and embedded hardware. Note that there has been no official announcement designating 7.4 as an LTS. One report says Greg Kroah-Hartman is set to take over its LTS management in early 2027.

## Security and trust pressures

Alongside the volume problem, the kernel has been dealing with fuzzing-driven bug discovery. Phoronix reported that a driver core pull request for 7.4 included a TAINT_FORCED_BIND change meant to deal with a major fuzzing attack. I couldn’t retrieve the details, so I won’t characterize it further. Heavy automated bug-hunting, human or AI-driven, is part of why stabilization work matters so much for this release.

## What to watch

- **rc2 and rc3 announcements.** Torvalds’ tone on regressions is the best early signal of whether things are calm or troubled.
- **Whether the rc period shrinks.** For comparison, 7.3’s rc phase added about 3,500 commits. If 7.4’s rc phase is similar or smaller despite the huge merge window, the AI-fixes-early theory looks right.
- **An rc8 announcement.** That would point to a final release in late December or early January.
- **Any LTS announcement.** A formal designation would confirm how much weight this release carries.

**Bottom line:** the timetable points to a December 20 release, and the main risk to that date is the sheer volume of changes. If the rc cycle goes smoothly, Linux users could have 7.4 before the holidays, and if it doesn’t, expect an extra week or two.
