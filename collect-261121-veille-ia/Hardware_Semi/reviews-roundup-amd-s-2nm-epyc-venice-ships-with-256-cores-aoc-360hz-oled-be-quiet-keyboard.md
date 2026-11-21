---
id: collect-261121-veille-ia/hardware-semi/reviews-roundup-amd-s-2nm-epyc-venice-ships-with-256-cores-aoc-360hz-oled-be-quiet-keyboard
title: "Reviews Roundup: AMD's 2nm EPYC Venice Ships with 256 Cores, AOC 360Hz OLED, be quiet! Keyboard"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["epyc", "venice", "cores", "storagereview", "reviews", "core", "week", "servethehome"]
source: https://www.ntcompatible.com/story/reviews-roundup-amds-2nm-epyc-venice-ships-with-256-cores-aoc-360hz-oled-be-quiet-keyboard/
source_anchor: ""
source_lines: [1, 58]
section: "Hardware_Semi"
---
AMD's sixth-generation EPYC "Venice," built at 2nm with up to 256 cores per socket, shipped this week with a surprise 5.3GHz clock bump and a bet to push Intel's x86 data-center share below 50%. StorageReview and ServeTheHome both broke the story, with Venice topping the leaderboard on a two-socket build though real OLTP workloads couldn't use most of its 256 cores. The rest of the week's reviews were thinner: APH gave the holographic TRYX HOLO AIO a middling 6.2, and Club386 praised the AOC Agon Pro QD-OLED while flagging its £849 price and weak 15W USB-C. Flight-sim fans got a pricey but authentic 737 CDU, and be quiet!'s silent Dark Mount keyboard rounded things out with a strong 9.0.

### AMD EPYC "Venice" heads a busy week of server and PC hardware reviews

**AMD's sixth-generation EPYC, codenamed Venice, shipped this week at 2 nanometers with up to 256 cores per socket and a clock bump AMD never bothered to advertise. It's also publicly betting the new SP7 platform can push its x86 data-center share past 50% for the first time. StorageReview and ServeTheHome broke the story between November 10 and 16, and the other five reviews of the week covered everything from a holographic AIO cooler to a silent $239 keyboard. The EPYC lead is where the real weight sits.**

The interesting detail is the clocks. AMD announced the flagship 9686F at 5.0GHz. It actually ships at 5.3GHz. Almost the entire SP7 stack picked up base- and boost-clock increases, per ServeTheHome, and no part tops out below 4.1GHz. AMD says the final silicon simply came out better than its model predicted. In a market where vendors routinely sandbag launch clocks to leave room for later binning, this reads almost uncomfortably straight.

The top part is the 9996: 256 cores, 512 threads, 1,024MB of L3, and a 600W thermal envelope. Nobody expected the 9956 either. AMD slipped in a 256-core, 400W part you'd have to go looking for. The 9996's and the new 9956's base clocks were actually transposed in the preliminary spec sheet, ServeTheHome's readers caught it live in the comments, and the editors fixed it before the story went live. That kind of mid-publish correction is rare enough to notice.

#### ServeTheHome: AMD EPYC 9006 Results Are Here and AMD Delivers a Knockout Part Deux

The day is finally here for the launch of AMD's EPYC 9006 "Venice" platform and the Zen 6 CPU architecture. AMD has been hard at work over the past two years to build a better server CPU, and thanks to its performance, Venice is primed to deliver another knockout blow for AMD


StorageReview ran a pair of 9996s in a Dell PowerEdge R9825, then flipped to an AMD "Nigeria" reference board running a 9686F with RDIMM-12800 memory. Memory bandwidth is where Venice really flexes. STREAM Triad hit 1,201 GB/s on DDR5-8000, roughly double the Turin part it replaces. Switch to MRDIMMs and it climbs to 1,901 GB/s. That's a 58% jump driven by memory alone.


Keep in mind that this matters almost as much as core count for memory-bound workloads. A 96-core 9686F on MRDIMMs finished a large OpenFOAM mesh within 13% of a 512-core system running on slower memory. On single-thread OpenSSL AES, the 9686F posted a record 22.5 GB/s — nearly three times what the previous Vera generation managed.

#### StorageReview: AMD EPYC Venice SP7 Review: 256 Cores Per Socket and the Bandwidth to Feed Them

AMD EPYC Venice SP7 review: the 256-core EPYC 9996 and 5.0GHz 9686F tested against NVIDIA Vera, Turin, and Xeon 6 on bandwidth and speed.


Here's where you have to temper the hype. No open-source database engine could actually use more than about 75% of the 256 cores in StorageReview's HammerDB tests. MySQL jumped 14% over Vera, but lost ground on MariaDB and PostgreSQL. The densest 512-thread configs also flounder on JVM workloads that lockstep across every logical CPU. Core count is a great marketing number, and a poor one for real OLTP.

StorageReview's verdict still lands. The 9996 is "the fastest general-purpose processor we have tested" for a two-socket build today, with the 9686F the pick when a workload wants clocks over cores. The R9825 keeps the Best Overall Server slot on the leaderboard. If data-center share is the actual prize, AMD is aiming at a marker tantalizingly out of reach since Turin shipped.


#### On the desktop side, the week's best story was arguably the most underwhelming

APH Networks gave the TRYX HOLO AIO a 6.2 out of 10, and honestly that score feels fair. The hook is a holographic display block, a 640×480 60Hz IPS panel behind a 45° lens pulling a Pepper's Ghost trick. It's genuinely one of the nicest-looking AIOs around. Performance tells a different tale. On a Core Ultra 7 265K, it ran 4°C hotter than a 240mm Noctua, and only 2-3°C cooler than a dual-tower Noctua air cooler. Software is the real problem. KANALI hangs with keystroke buffering, an inconsistent UI, and a 1.1GB install. At about $180 it's a fine showpiece, but the cooler itself isn't in the conversation.

#### APH Networks: TRYX HOLO Review

I was casually eating breakfast one summer morning when a kid from church sent me a follow request on Instagram. I knew who he was, but I always give profiles a quick view before confirming. One thing that made me chuckle was his bio listing himself as an incoming "biological sciences (pre-med)" student. Medical school is a post-graduate professional degree here in North America, and the entry requirements are quite stringent, including a top undergraduate GPA, high MCAT scores, meaningful non-academic experiences, and passing a multiple mini-interviews stage. Therefore, identifying as a "pre-med" student in first year biological sciences is at best at aspiration, and it is not a whole lot different than someone enrolling in a first year business program and listing themselves as a "pre-billionaire". In fact, most students entering into biological sciences think they are going into medical school after they graduate, but maybe 2 or 3% of a first year class will end up making it. In other words, it is quite rare -- almost as rare as me reviewing AIO liquid coolers here at APH Networks. But can trends change? When I reviewed the Noctua NL-LC1-24 back in July, I pointed out I have personally reviewed only two prior to it, despite APH Networks as a whole has covered countless models from practically every other brand in our Cooling section. The AIO coolers I have tested before the Noctua included the Thermaltake WATER2.0 Pro in 2012 and Cooler Master MasterLiquid ML240 Illusion in 2021. Two in a year? Now that is a personal record. Today, I am going to take a look at the TRYX HOLO, a 360mm cooler that features a 640x480 60Hz IPS display on top of a 45° lens to create a slick Pepper’s Ghost holographic effect. Will it look as it cool as it sounds, and will it cool as much as we want? Read on to find out!


The AOC Agon Pro AGP346UCSD is the opposite; a genuinely excellent panel that stumbles on value. Club386 rated it 4 out of 5, and the panel earns it. It's a 34-inch QD-OLED at 3440×1440, curving at 1800R, running 360Hz with a 0.03ms response. Color hits 99% DCI-P3 with an average ΔE of 0.76, and the RGB Stripe subpixels keep text clean. Ports are generous too, with a full DP 2.1 connection that needs no DSC. The catch is the price at £849 or $899, plus a USB-C port that only delivers 15W. You can't charge a laptop off it while playing. A comparable MSI and a Philips both use the same panel, offer 98W, and cost less. It's a fantastic display. It's just not a fair deal.

#### Club386: AOC Agon Pro AGP346UCSD review: a great ultrawide QD-OLED gaming monitor with loads of features

With a 360Hz refresh rate, excellent colour reproduction, deep blacks, and an RGB Stripe subpixel layout, this is a superb QD-OLED screen for work and play.


Then there are the niche winners. Tom's Hardware's verdict on the Cockpit-Simulator 737X CDU (v2): it brings "an authentic, premium-build Boeing 737 Control Display Unit to the home cockpit," but its "high price makes it a niche product for serious flight sim enthusiasts." It's a 1:1 replica of the Boeing 737's CDU, built by the premium arm that spun out of Cockpit-Master last year. Authenticity and build are top-tier. The lone reason to hesitate is whether your simulator budget has room for a cockpit-sized splurge.

#### Tom's Hardware: Cockpit-Simulator 737X CDU (v2) Review: An Essential Component for Flight Sim Enthusiasts

An authentic replica of the Boeing 737 Control Display Unit.


be quiet! made a cleaner overall package with its Dark Mount keyboard, scoring a 9.0 out of 10 at Funky Kit. It's the German cooler maker's flagship mechanical board, built around silent linear orange switches that are noticeably quieter than typical while keeping the mechanical feel. Everything is modular: the numpad detaches, a Media Dock mounts left or right, and eight Display Keys can run macros or custom images. The brushed aluminum top and hot-swappable switches make it a strong build. The lone real knock is the $239 price and a footprint that's still large even without the numpad attached.

#### Funky Kit: be quiet! Dark Mount Mechanical Gaming Keyboard Review

Today, we’ll be taking a look at the be quiet! Dark Mount Mechanical Gaming Keyboard. be quiet! has built a strong reputation for producing high-quality PC components and peripherals, with a particular focus on performance, thoughtful engineering and understated design.
