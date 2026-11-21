---
id: collect-261121-veille-ia/hardware-semi/m5-max-macbook-pro-redesign-tested-lighter-cooler-and-way-longer-battery
title: "M5 Max MacBook Pro Redesign Tested: Lighter, Cooler, and Way Longer Battery"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["macbook", "original", "machines", "redesigned", "same", "about", "lighter", "battery"]
source: https://www.mindstudio.ai/blog/m5-max-macbook-pro-redesign-review
source_anchor: ""
source_lines: [1, 91]
section: "Hardware_Semi"
---
## What actually changed in the redesigned M5 Max MacBook Pro?

Apple kept the same M5 Max chip but reworked the body around it. The new model weighs roughly 1,837 grams against 2,147 grams on the original, a difference heavy enough to notice on a shoulder strap. The chassis is shallower front to back, though width is unchanged. Apple also split the internal vent into two sections instead of one continuous vent, removed some rear screws, recessed the rubber feet instead of letting them protrude, and added a dynamic-island style notch at the top of the screen. The display resolution moved slightly, from 1728x1117 to 1736x1133, and the menu bar grew from about 7mm to 9mm to make room for the notch. The screen is also brighter and, based on hands-on testing, crisper even in dark mode. The biggest new feature is a touchscreen, which can be disabled in software if you don’t want it.

## TL;DR

- The new M5 Max MacBook Pro is **about 310 grams lighter** than the original model despite having the same width and chip.
- **Raw performance is unchanged** : Xcode clean builds, C++ sort tests, big .NET builds, and Python interpreter tests all landed within margin of error between the two machines.
- Under a sustained 10-minute full-core load, the redesigned machine’s **fans spin faster (around 6,200 to 6,700 RPM versus 5,300 to 5,700 RPM)** but the chassis actually runs a bit cooler, especially on the lid.
- In a full-day simulated developer workload, the new MacBook Pro lasted **10 hours 47 minutes versus 6 hours 54 minutes** on the original, roughly 56% more runtime at the same 100% brightness.
- **Dark mode saves far more battery on the new screen** : switching VS Code and the OS to dark mode cut power draw by about 27% on the new model versus about 15% on the original.
- Oddly, on a **pure black test screen the original model actually drew less power** (1.7 watts versus 3.3 watts), suggesting the new panel’s backlight zones or touch layer carry some fixed overhead.
- The new **touchscreen isn’t just a gimmick for app developers** : it lets you test touch interactions directly in the iOS Simulator with your finger instead of a mouse or a physical device.

Live class · Wed, Dec 2

### $40,000+/mo

in SaaS spend the Remy team cut by building its own apps with AI.

We'll show you how.

Bring an idea.

Build it live.

Leave certified.

## Is the new MacBook Pro actually faster than the original?

No, and that’s expected since both machines use the same M5 Max chip. A battery of workload tests, including a clean Xcode build using 76 popular Cocoa Pods and their dependencies, a .NET build generating 100,000 namespaces and classes, a single-core C++ sort, a multi-core sort, an interpreted Python benchmark, and a memory bandwidth check, all came back essentially tied. The Xcode build finished in about 79 to 81 seconds on both machines. The .NET build landed at 44.9 and 45.4 seconds. Memory bandwidth measured 382 GB/s on the original and 377 GB/s on the redesigned unit, a gap small enough to be noise.

The test unit had 48GB of memory compared to 128GB in the original review unit, and it still kept pace across every compute-bound task tried. For typical app development work, including compiling, running, and building projects, the amount of memory tested here didn’t become a bottleneck. That calculus changes for local AI workloads where memory capacity and bandwidth matter more directly, but for compiling code and running IDEs, the thinner body and redesigned vents didn’t cost any speed.

## Does the smaller, lighter body cause the new MacBook Pro to throttle?

Not in sustained testing. Pinning every CPU core for 10 minutes on both machines showed performance cores hitting 100% utilization and efficiency cores running at 80 to 86% on both laptops, with clock speeds on the performance cores holding around 4.2GHz on each. Total system power draw landed in the same 100 to 110 watt range for both.

The redesigned machine’s fans ran noticeably harder, reaching roughly 6,200 to 6,700 RPM compared to 5,300 to 5,700 RPM on the original. That’s the tradeoff for the thinner chassis and the new split-vent design: more fan noise to move the same heat out of a smaller space. Surface temperatures told a more interesting story. The original model developed hot spots on the lid reaching around 42°C, while the redesigned model showed no comparable hot spots and measured a couple degrees cooler in the same area. The new vent layout and thinner body didn’t translate into worse thermals or reduced sustained performance, just louder fans getting the job done.

## Why does the redesigned MacBook Pro get so much better battery life?

This is the most unexpected result in the comparison, and it comes down to the new screen technology. Running a simulated full workday (writing code, building projects, copying files, checking off tasks, reading documentation, and watching video) with both laptops at 100% brightness and roughly identical 100 watt-hour batteries, the original model died at 6 hours 54 minutes. The redesigned model kept running until 10 hours 47 minutes, nearly four hours longer and about 56% more total runtime.

An overnight power-draw comparison backed this up: the original averaged 13.9 watts, the redesigned unit averaged 9.2 watts, roughly a third less power draw for comparable work.

**›**Build a loan-approval app for 2,000 bankers.

*already included:*✓ SOC 2✓ role-based access✓ audit log✓ self-hosted

The display appears to be the main driver. The new panel behaves like an OLED where individual pixels emit their own light, meaning darker content draws meaningfully less power. Switching VS Code and the OS to dark mode cut the original model’s draw by about 15% (17.2 watts to 14.7 watts), but cut the redesigned model’s draw by about 27% (13 watts to 9.5 watts), nearly double the savings. Just switching the code editor’s theme to dark shaved 5.5 watts off the new machine’s draw.

Interestingly, testing a pure black fullscreen image showed the original model drawing less power (1.7 watts) than the redesigned model (3.3 watts), the opposite of what OLED behavior would predict. That suggests the new screen is a mini-LED panel with backlight zones rather than true per-pixel OLED, and the touch layer or panel electronics add a small fixed power cost even when the screen shows nothing. On a pure white screen, the new model still drew noticeably less power than the original (10.5 watts versus 14 watts), which is where most of the real-world battery gain is coming from.

## Is the MacBook Pro touchscreen actually useful for developers?

For most Mac users, probably not a daily habit. But for mobile app developers, it solves a real workflow problem. Testing touch interactions in iOS or iPadOS apps traditionally means clicking around the Simulator with a mouse or trackpad, which doesn’t replicate how a finger actually interacts with a strum gesture, a swipe, or a multi-touch control, or deploying the build to a physical device just to test touch behavior.

With a touchscreen Mac, the Simulator becomes directly touchable. One demonstrated example was Guitar Pocketbook, a NativeScript app for iPhone and iPad built around strumming gestures. Testing that interaction with a finger directly on the laptop screen is simply more natural than dragging a cursor across it. The touchscreen can be turned off entirely if you don’t want it, but for anyone building touch-first mobile interfaces, it removes a step from the test cycle.

## Is the M5 Max MacBook Pro redesign worth upgrading for?

If you already own an M5 Max MacBook Pro, the case for upgrading rests almost entirely on weight and battery life, not speed. Performance is identical chip for chip. The redesign trims nearly 300 grams, keeps thermals under control despite a smaller chassis, and nearly doubles real-world battery runtime in a full-day workload, with dark mode users seeing the largest gains. The touchscreen is a genuine workflow upgrade for mobile developers specifically, less so for everyone else. For anyone not already on the original M5 Max model, the longer battery life alone makes the redesigned version the easy pick.

## Frequently Asked Questions

### Does the redesigned MacBook Pro use a different chip?

No. Both the original and redesigned versions tested here use the same M5 Max chip with identical CPU and GPU core counts, which is why compute benchmarks came back essentially tied.

### How much lighter is the new MacBook Pro?

The redesigned unit weighed about 1,837 grams compared to 2,147 grams for the original, a reduction of roughly 310 grams, even though the width of the chassis stayed the same.

### Does dark mode make a real difference in battery life?

Yes, more so on the redesigned model. Switching to dark mode in the OS and code editor cut power draw by about 27% on the new screen versus about 15% on the original screen, suggesting the new panel rewards dark themes more directly.

**Adjuster**✕ Export PHI

**#4471**

**12,480**users online

### Can you turn off the MacBook Pro’s touchscreen?

Yes, the touchscreen can be disabled in software for users who don’t want it, though it’s positioned as genuinely useful for testing touch-based mobile app interactions directly in the Simulator.

### Does the new vent design cause more fan noise?

Yes. Under a sustained full-core load, the redesigned model’s fans ran at roughly 6,200 to 6,700 RPM compared to 5,300 to 5,700 RPM on the original, though this didn’t translate into slower build times or reduced performance.
