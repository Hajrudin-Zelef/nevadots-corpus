---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-version-history-wikipedia
title: "Linux kernel version history - Wikipedia"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["version", "release", "kernel", "linux", "greg", "kroah-hartman", "history", "linus"]
source: https://en.wikipedia.org/wiki/Linux_kernel_version_history
source_anchor: ""
source_lines: [1, 866]
section: "Linux_Kernel"
---
This article documents the broad, **wide version history of the Linux kernel**, a free, open-source, and Unix-like kernel that is used on many computer systems and web servers worldwide.

Since the Linux kernel's creation by Linus Torvalds in 1991, it had rapidly gained popularity as many developers started contributing to the project.

The kernel became self-hosting as of version 0.11 in December 1991. The version 1.0.0 of the kernel was released on March 1994, which consisted of around 176,250 lines in the original unmodified source code.

The most recent stable release of the Linux kernel is 7.3, which was released on 18 October 2026<sup>[1]</sup>.

In contrast to semantic versioning, the major version carries no intrinsic meaning for the kernel, as Linux promises stable interfaces and behavior even when the major version changes. However, since version 3.x, minor versions have been capped around 20 to avoid the false perception that changes between large minor versions (e.g. from 5.30 to 5.31) would be less voluminous or impactful than changes between smaller minor versions (e.g. from 6.1 to 6.2).<sup>[2]</sup>

Each feature release – identified by the first two numbers of a release version – is designated one of the following levels of support:

- Stable/Mainline; supported until next stable version and/or 3 months after that.
- Long-term support (LTS); releases are maintained for a few years.<sup>[3]</sup>
- Super-long-term support (SLTS); releases maintained further for many more years by the Civil Infrastructure Platform (CIP).<sup>[4]</sup>

On 8 February 2026, Linus Torvalds announced that the next kernel release will be published as version 7.0.<sup>[5]</sup> Following the usual release cycle of about two months for every new version, kernel 7.0 was released on 12 April 2026.<sup>[6]</sup>

| Version | Original release date | Last release | Maintainer | EOL | Prominent features | Notes | 
|---|---|---|---|---|---|---|
| 7.4 | Future Release | 7.4-rc3 <sup>[7]</sup> | Linus Torvalds |  |  |  | 
| 7.3 | 18 October 2026 <sup>[1]</sup> | 7.3.5 | Greg Kroah-Hartman |  |  | This release may be an LTS release (usually the last release per calendar year gets designated as LTS, though 7.4 may still fall into 2026) | 
| 7.2 | 16 August 2026 <sup>[32]</sup> | 7.2.15 <sup>[33]</sup> | Greg Kroah-Hartman | 1 November 2026 |  |  | 
| 7.1 | 14 June 2026 <sup>[42]</sup> | 7.1.13 <sup>[43]</sup> | Greg Kroah-Hartman | 2 September 2026 <sup>[44]</sup> |  | This release removes all legacy i486 support, reducing the number of platforms the latest kernels can be used on.<sup>[55]</sup> | 
| 7.0 | 12 April 2026 <sup>[56]</sup> | 7.0.14 <sup>[57]</sup> | Greg Kroah-Hartman (formerly Linus Torvalds till the release cycle ended) | 27 June 2026 <sup>[58]</sup> |  | Following previous conventions, Linux 7.0 is not a major upgrade over the Linux 6.x series, but instead what would have been considered Linux 6.20. <sup>[75]</sup> | 
| **Legend:****Latest version** |  |  |  |  |  |  | 

| Version | Original release date | Last release | Maintainer | EOL | Prominent features | Notes | 
|---|---|---|---|---|---|---|
| 6.19 | 8 February 2026 <sup>[76]</sup> | 6.19.14 <sup>[77]</sup> | Greg Kroah-Hartman | 22 April 2026 |  |  | 
| 6.18 | 30 November 2025 <sup>[88]</sup> | 6.18.64 <sup>[89]</sup> |  | December 2028 <sup>[90]</sup><sup>[91]</sup> |  | 26th LTS release <sup>[105]</sup> | 
| 6.17 | 28 September 2025 <sup>[106]</sup> | 6.17.13 <sup>[107]</sup> |  | 18 December 2025 |  |  | 
| 6.16 | 27 July 2025 <sup>[118]</sup> | 6.16.12 <sup>[119]</sup> |  | 12 October 2025 |  |  | 
| 6.15 | 25 May 2025 <sup>[130]</sup> | 6.15.11 <sup>[131]</sup> |  | 20 August 2025 |  |  | 
| 6.14 | 24 March 2025 <sup>[133]</sup> | 6.14.11 <sup>[134]</sup> |  | 10 June 2025 <sup>[135]</sup> |  |  | 
| 6.13 | 20 January 2025 <sup>[139]</sup> | 6.13.12 <sup>[140]</sup> |  | 20 April 2025 <sup>[141]</sup> |  |  | 
| 6.12 | 17 November 2024 <sup>[142]</sup> | 6.12.118 <sup>[143]</sup> |  | December 2028 <sup>[90]</sup><sup>[91]</sup> |  | 25th LTS release <sup>[105]</sup>5th SLTS with 10 years of support through 2035.<sup>[146]</sup> Used in Debian 13 "Trixie"<sup>[147]</sup><sup>[148]</sup> , RHEL 10.0<sup>[149]</sup> and OpenSuse Leap 16.1 | 
| 6.11 | 15 September 2024 <sup>[150]</sup> | 6.11.11 <sup>[151]</sup> |  | 5 December 2024 <sup>[151]</sup> |  |  | 
| 6.10 | 14 July 2024 <sup>[154]</sup> | 6.10.14 <sup>[140]</sup> | Greg Kroah-Hartman & Sasha Levin | 10 October 2024 <sup>[155]</sup> |  | Named "Baby Opossum Posse". <sup>[157]</sup> The one last minute change was made in the credits of the ReiserFS README as requested by the original developer.<sup>[158]</sup> | 
| 6.9 | 12 May 2024 <sup>[159]</sup> | 6.9.12 <sup>[140]</sup> |  | 27 July 2024 <sup>[160]</sup> |  |  | 
| 6.8 | 10 March 2024 <sup>[163]</sup> | 6.8.12 <sup>[140]</sup> |  | 30 May 2024 <sup>[164]</sup> |  | Used in Ubuntu 24.04 LTS <sup>[172]</sup> | 
| 6.7 | 7 January 2024 <sup>[173]</sup> | 6.7.12 <sup>[140]</sup> |  | 3 April 2024 |  | According to Linus Torvalds, "one of the largest kernel releases we've ever had" <sup>[174]</sup> | 
| 6.6 | 29 October 2023 <sup>[177]</sup> | 6.6.162 <sup>[178]</sup> |  | December 2027 <sup>[90]</sup><sup>[91]</sup> |  | 24th LTS release The CFS scheduler was the de facto standard for 16+ years ReiserFS is now declared to be obsolete and flagged for removal in 2025. | 
| 6.5 | 27 August 2023 <sup>[181]</sup> | 6.5.13 <sup>[140]</sup> |  | 28 November 2023 <sup>[182]</sup> |  |  | 
| 6.4 | 25 June 2023 <sup>[184]</sup> | 6.4.16 <sup>[140]</sup> |  | 13 September 2023 <sup>[185]</sup> |  |  | 
| 6.3 | 23 April 2023 <sup>[189]</sup> | 6.3.13 <sup>[140]</sup> |  | 11 July 2023 <sup>[190]</sup> |  |  | 
| 6.2 | 19 February 2023 <sup>[191]</sup> | 6.2.16 <sup>[140]</sup> |  | 17 May 2023 <sup>[192]</sup> |  |  | 
| 6.1 | 11 December 2022 <sup>[193]</sup> | 6.1.192 <sup>[194]</sup> |  | December 2027 <sup>[3]</sup>August 2033<sup>[4]</sup> |  | 23rd LTS release Used in Debian 12 "Bookworm" <sup>[199]</sup>4th SLTS release (which CIP<sup>[200]</sup> is planning<sup>[4]</sup> to support until August 2033) 6.1.28 is named Curry Ramen<sup>[201]</sup> | 
| 6.0 | 2 October 2022 <sup>[202]</sup> | 6.0.19 <sup>[203]</sup> |  | January 2023 <sup>[203]</sup> |  | Named "Hurr durr I'ma [ *sic* ] ninja sloth"<sup>[206]</sup> | 
| **Legend:** |  |  |  |  |  |  | 

| Version | Original release date | Last release | Maintainer | EOL | Prominent features | Notes | 
|---|---|---|---|---|---|---|
| 5.19 | 31 July 2022 <sup>[207]</sup> | 5.19.17 <sup>[208]</sup> | Greg Kroah-Hartman & Sasha Levin <sup>[3]</sup> | October 2022 <sup>[208]</sup> |  |  | 
| 5.18 | 22 May 2022 <sup>[212]</sup> | 5.18.19 <sup>[213]</sup> |  | August 2022 <sup>[213]</sup> |  |  | 
| 5.17 | 20 March 2022 <sup>[221]</sup> | 5.17.15 <sup>[222]</sup> |  | June 2022 <sup>[222]</sup> |  | Used in Ubuntu 22.04 LTS on newer hardware <sup>[229]</sup>Named Superb Owl<sup>[230]</sup> | 
| 5.16 | 9 January 2022 <sup>[231]</sup> | 5.16.20 <sup>[232]</sup> |  | April 2022 <sup>[232]</sup> |  |  | 
| 5.15 | 31 October 2021 <sup>[237]</sup> | 5.15.225 <sup>[238]</sup> |  | December 2026 <sup>[3]</sup> |  | 22nd LTS release; used in Named Trick or Treat<sup>[244]</sup> | 
| 5.14 | 29 August 2021 <sup>[245]</sup> | 5.14.21 <sup>[246]</sup> | Greg Kroah-Hartman | November 2021 <sup>[246]</sup> |  | Used in RHEL 9.x and derivatives <sup>[247]</sup> (Red Hat ignores LTS-Kernel, own kernel-backports) and SLE 15 SP4/openSUSE Leap 15.4 | 
| 5.13 | 27 June 2021 <sup>[248]</sup> | 5.13.19 <sup>[249]</sup> | Greg Kroah-Hartman & Sasha Levin | September 2021 <sup>[249]</sup> |  | Named Opossums on Parade | 
| 5.12 | 25 April 2021 <sup>[252]</sup> | 5.12.19 <sup>[253]</sup> | Greg Kroah-Hartman | July 2021 <sup>[253]</sup> |  | Named Frozen Wasteland <sup>[254]</sup><sup>[255]</sup> | 
| 5.11 | 14 February 2021 <sup>[256]</sup> | 5.11.22 <sup>[257]</sup> |  | May 2021 <sup>[257]</sup> |  | Named "💕 Valentine's Day Edition 💕" <sup>[258]</sup> | 
| 5.10 | 13 December 2020 <sup>[259]</sup> | 5.10.274 <sup>[260]</sup> | Greg Kroah-Hartman & Sasha Levin <sup>[3]</sup> | December 2026 <sup>[3]</sup><sup>[261]</sup>January 2031<sup>[4]</sup> |  | 21st LTS release; used in Debian 11 "Bullseye" <sup>[263]</sup>3rd SLTS release (which CIP<sup>[264]</sup> is planning<sup>[4]</sup> to support until January 2031) Named "Dare mighty things"<sup>[265]</sup> | 
| 5.9 | 11 October 2020 <sup>[266]</sup> | 5.9.16 <sup>[267]</sup> | Greg Kroah-Hartman | December 2020 <sup>[267]</sup> |  |  | 
| 5.8 | 2 August 2020 <sup>[268]</sup> | 5.8.18 <sup>[269]</sup> |  | November 2020 <sup>[269]</sup> |  |  | 
| 5.7 | 31 May 2020 <sup>[270]</sup> | 5.7.19 <sup>[271]</sup> |  | August 2020 <sup>[271]</sup> |  |  | 
| 5.6 | 29 March 2020 <sup>[272]</sup> | 5.6.19 <sup>[273]</sup> |  | June 2020 <sup>[273]</sup> |  |  | 
| 5.5 | 26 January 2020 <sup>[275]</sup> | 5.5.19 <sup>[276]</sup> |  | April 2020 <sup>[276]</sup> |  |  | 
| 5.4 | 24 November 2019 <sup>[277]</sup> | 5.4.302 <sup>[140]</sup> | Greg Kroah-Hartman & Sasha Levin <sup>[3]</sup> | December 2025 <sup>[3]</sup> |  | 20th LTS release, used in Ubuntu 20.04 LTS 5.4-rc2 is named Nesting Opossum <sup>[278]</sup>5.4-rc5 is named Kleptomaniac Octopus<sup>[279]</sup> | 
| 5.3 | 15 September 2019 <sup>[280]</sup> | 5.3.18 <sup>[281]</sup> | Greg Kroah-Hartman | December 2019 <sup>[281]</sup> |  |  | 
| 5.2 | 7 July 2019 <sup>[282]</sup> | 5.2.21 <sup>[283]</sup> |  | October 2019 <sup>[283]</sup> |  | 5.2-rc2 is named Golden Lions <sup>[284]</sup><sup>[285]</sup>5.2 is named Bobtail Squid<sup>[286]</sup> | 
| 5.1 | 5 May 2019 <sup>[287]</sup> | 5.1.21 <sup>[288]</sup> |  | July 2019 <sup>[288]</sup> |  |  | 
| 5.0 | 3 March 2019 <sup>[290]</sup> | 5.0.21 <sup>[291]</sup> |  | June 2019 <sup>[291]</sup> |  |  | 
| **Legend:** |  |  |  |  |  |  | 

| Version | Original release date | Last release | Maintainer | EOL | Prominent features | Notes | 
|---|---|---|---|---|---|---|
| 4.20 | 23 December 2018 <sup>[292]</sup> | 4.20.17 <sup>[293]</sup> | Greg Kroah-Hartman | March 2019 <sup>[293]</sup> |  | Named Shy Crocodile <sup>[294]</sup> | 
| 4.19 | 22 October 2018 <sup>[295]</sup> | 4.19.325 <sup>[140]</sup>4.19.325-cip137<sup>[296]</sup> | Greg Kroah-Hartman & Sasha Levin <sup>[3]</sup>Ulrich Hecht & Pavel Machek<sup>[4]</sup> | December 2024 <sup>[3]</sup><sup>[297]</sup>January 2029<sup>[4]</sup> |  | 19th LTS release. Used in Debian 10 "Buster". <sup>[298]</sup> Second SLTS release (which CIP is planning<sup>[4]</sup> to support until January 2029).<sup>[299]</sup> Named "People's Front"<sup>[300]</sup> | 
| 4.18 | 12 August 2018 <sup>[301]</sup> | 4.18.20 <sup>[302]</sup> | Greg Kroah-Hartman | November 2018 <sup>[302]</sup> |  | RHEL 8.x (Red Hat ignores LTS-Kernel, own kernel-backports) | 
| 4.17 | 3 June 2018 <sup>[303]</sup> | 4.17.19 <sup>[304]</sup> |  | August 2018 <sup>[304]</sup> |  | Named Merciless Moray <sup>[305]</sup> | 
| 4.16 | 1 April 2018 <sup>[306]</sup> | 4.16.18 <sup>[307]</sup> |  | June 2018 <sup>[307]</sup> |  |  | 
| 4.15 | 28 January 2018 <sup>[308]</sup> | 4.15.18 <sup>[309]</sup> |  | April 2018 <sup>[309]</sup> |  | Used in Ubuntu 18.04 LTS | 
| 4.14 | 12 November 2017 <sup>[310]</sup> | 4.14.336 <sup>[311]</sup> | Greg Kroah-Hartman & Sasha Levin <sup>[3]</sup> | January 2024 <sup>[311]</sup> |  | 18th LTS release 4.14.1 is named Petit Gorille<sup>[313]</sup> | 
| 4.13 | 3 September 2017 <sup>[314]</sup> | 4.13.16 <sup>[315]</sup> | Greg Kroah-Hartman | November 2017 <sup>[315]</sup> |  |  | 
| 4.12 | 2 July 2017 <sup>[316]</sup> | 4.12.14 <sup>[317]</sup> |  | September 2017 <sup>[317]</sup> |  |  | 
| 4.11 | 30 April 2017 <sup>[319]</sup> | 4.11.12 <sup>[320]</sup> |  | July 2017 <sup>[320]</sup> |  |  | 
| 4.10 | 19 February 2017 <sup>[321]</sup> | 4.10.17 <sup>[322]</sup> |  | May 2017 <sup>[322]</sup> |  | 4.10-rc5 was named Anniversary Edition <sup>[323]</sup>4.10-rc6 was named Fearless Coyote<sup>[324]</sup> | 
| 4.9 | 11 December 2016 <sup>[325]</sup> | 4.9.337 <sup>[140]</sup> | Greg Kroah-Hartman & Sasha Levin <sup>[3]</sup> | January 2023 <sup>[3]</sup><sup>[326]</sup> |  | 17th LTS release. Used in Debian 9 "Stretch". <sup>[327]</sup> Named Roaring Lionus<sup>[328]</sup><sup>[329]</sup> | 
| 4.8 | 2 October 2016 <sup>[330]</sup> | 4.8.17 <sup>[331]</sup> | Greg Kroah-Hartman | January 2017 <sup>[331]</sup> |  |  | 
| 4.7 | 24 July 2016 <sup>[332]</sup> | 4.7.10 <sup>[333]</sup> |  | October 2016 <sup>[333]</sup> |  | Named Psychotic Stoned Sheep <sup>[337]</sup> | 
| 4.6 | 15 May 2016 <sup>[338]</sup> | 4.6.7 <sup>[339]</sup> |  | August 2016 <sup>[339]</sup> |  | Named Charred Weasel <sup>[340]</sup> | 
| 4.5 | 13 March 2016 <sup>[341]</sup> | 4.5.7 <sup>[342]</sup> |  | June 2016 <sup>[343]</sup> |  |  | 
| 4.4 | 10 January 2016 <sup>[344]</sup> | 4.4.302 <sup>[345]</sup>4.4.302-cip103<sup>[296]</sup> | Greg Kroah-Hartman & Sasha Levin <sup>[3]</sup>Ulrich Hecht & Pavel Machek<sup>[4]</sup> | February 2022 <sup>[345]</sup>January 2027<sup>[4]</sup> |  | 16th LTS release, used in Slackware 14.2. <sup>[346]</sup> Canonical provided extended support until April 2021.<sup>[347]</sup> As the first kernel selected for Super Long Term Support (SLTS), the Civil Infrastructure Platform will provide support until January 2027.<sup>[4]</sup> Used in Ubuntu 16.04 LTS | 
| 4.3 | 1 November 2015 <sup>[348]</sup> | 4.3.6 <sup>[349]</sup> | Greg Kroah-Hartman | February 2016 <sup>[350]</sup> |  | Named Blurry Fish Butt <sup>[351]</sup><sup>[352]</sup> | 
| 4.2 | 30 August 2015 <sup>[353]</sup> | 4.2.8 <sup>[354]</sup> |  | December 2015 <sup>[354]</sup> |  | Canonical provided extended support until July 2016. <sup>[355]</sup><sup>[356]</sup> | 
| 4.1 | 22 June 2015 <sup>[357]</sup> | 4.1.52 <sup>[358]</sup> | Sasha Levin <sup>[3]</sup><sup>[359]</sup> (formerly Greg Kroah-Hartman)<sup>[360]</sup> | May 2018 <sup>[358]</sup> |  | 15th LTS release. 4.1.1 was named Series 4800 <sup>[361]</sup> | 
| 4.0 | 12 April 2015 <sup>[362]</sup> | 4.0.9 <sup>[363]</sup> | Greg Kroah-Hartman | July 2015 <sup>[364]</sup> |  | Named "Hurr durr I'ma [ *sic* ] sheep"<sup>[366]</sup> (Internet poll) | 
| **Legend:** |  |  |  |  |  |  | 

The jump from 2.6.x to 3.x wasn't because of a breaking update, but rather the first release of a new versioning scheme introduced as a more convenient system.<sup>[367]</sup>

| Version | Original release date | Last release | Maintainer | EOL | Prominent features | Notes | 
|---|---|---|---|---|---|---|
| 3.19 | 8 February 2015 <sup>[368]</sup> | 3.19.8 <sup>[369]</sup> | Greg Kroah-Hartman | May 2015 <sup>[369]</sup> |  | Canonical provided extended support until July 2016. <sup>[355]</sup><sup>[370]</sup> | 
| 3.18 | 7 December 2014 <sup>[371]</sup> | 3.18.140 <sup>[372]</sup> | Greg Kroah-Hartman <sup>[373]</sup> (formerly Sasha Levin<sup>[374]</sup> ) (formerly Greg Kroah-Hartman) | May 2019 <sup>[372]</sup> |  | 14th LTS release, named Diseased Newt <sup>[377]</sup> | 
| 3.17 | 5 October 2014 <sup>[378]</sup> | 3.17.8 <sup>[379]</sup> | Greg Kroah-Hartman | January 2015 <sup>[379]</sup> |  |  | 
| 3.16 | 3 August 2014 <sup>[380]</sup> | 3.16.85 <sup>[381]</sup> | Ben Hutchings <sup>[3]</sup><sup>[382]</sup> (formerly Greg Kroah-Hartman) | Maintained until October 2014, then May 2016 to June 2020 <sup>[381]</sup><sup>[3]</sup><sup>[383]</sup> |  | 13th LTS release. Was used in Debian 8 "Jessie". <sup>[384]</sup> Canonical provided extended support until April 2016.<sup>[355]</sup><sup>[385]</sup>3.16.1 was named Museum of Fishiegoodies<sup>[386]</sup> | 
| 3.15 | 8 June 2014 <sup>[387]</sup> | 3.15.10 <sup>[388]</sup> | Greg Kroah-Hartman | August 2014 <sup>[388]</sup> |  |  | 
| 3.14 | 30 March 2014 <sup>[390]</sup> | 3.14.79 <sup>[391]</sup> | Greg Kroah-Hartman <sup>[3]</sup> | September 2016 <sup>[391]</sup> |  | 12th LTS release, named Shuffling Zombie Juror <sup>[393]</sup> | 
| 3.13 | 19 January 2014 <sup>[394]</sup> | 3.13.11 <sup>[395]</sup> | Greg Kroah-Hartman | April 2014 <sup>[395]</sup> |  | Canonical provided extended support until April 2016. <sup>[355]</sup><sup>[397]</sup> Named One Giant Leap for Frogkind<sup>[398]</sup> (NASA LADEE launch photo)<sup>[399]</sup> Used in Ubuntu 14.04 LTS | 
| 3.12 | 3 November 2013 <sup>[400]</sup> | 3.12.74 <sup>[401]</sup> | Jiří Slabý <sup>[3]</sup><sup>[402]</sup> (formerly Greg Kroah-Hartman) | May 2017 <sup>[402]</sup><sup>[401]</sup> |  | 11th LTS release, named Suicidal Squirrel <sup>[403]</sup> | 
| 3.11 | 2 September 2013 <sup>[404]</sup> | 3.11.10 <sup>[405]</sup> | Greg Kroah-Hartman | November 2013 <sup>[405]</sup> |  | Canonical provided extended support until August 2014. <sup>[355]</sup> Named Linux for Workgroups after the 20 years of Windows 3.11<sup>[407]</sup> | 
| 3.10 | 30 June 2013 <sup>[408]</sup> | 3.10.108 <sup>[409]</sup> | Willy Tarreau <sup>[3]</sup><sup>[410]</sup> (formerly Greg Kroah-Hartman) | November 2017 <sup>[409]</sup> |  | 10th LTS release, 3.10.6 was named TOSSUG Baby Fish <sup>[412]</sup><sup>[413]</sup><sup>[414]</sup> used in Slackware 14.1<sup>[415]</sup>RHEL 7.x | 
| 3.9 | 28 April 2013 <sup>[416]</sup> | 3.9.11 <sup>[417]</sup> | Greg Kroah-Hartman | July 2013 <sup>[417]</sup> |  | 3.9.6 was named Black Squirrel Wakeup Call <sup>[421]</sup> | 
| 3.8 | 18 February 2013 <sup>[422]</sup> | 3.8.13 <sup>[423]</sup> | Greg Kroah-Hartman | May 2013 <sup>[423]</sup> |  | Canonical provided extended support until August 2014. <sup>[355]</sup><sup>[426]</sup>Named Unicycling Gorilla<sup>[427]</sup><sup>[428]</sup> 3.8.5 was named Displaced Humerus Anterior <sup>[429]</sup> | 
| 3.7 | 10 December 2012 <sup>[430]</sup> | 3.7.10 <sup>[431]</sup> | Greg Kroah-Hartman | March 2013 <sup>[431]</sup><sup>[432]</sup> |  | Named Terrified Chipmunk <sup>[434]</sup><sup>[435]</sup> | 
| 3.6 | 30 September 2012 <sup>[436]</sup> | 3.6.11 <sup>[437]</sup> | Greg Kroah-Hartman | December 2012 <sup>[437]</sup> |  |  | 
| 3.5 | 21 July 2012 <sup>[439]</sup> | 3.5.7 <sup>[440]</sup> | Greg Kroah-Hartman | October 2012 <sup>[440]</sup> |  | Canonical provided extended support until April 2014. <sup>[355]</sup><sup>[441]</sup> | 
| 3.4 | 20 May 2012 <sup>[442]</sup><sup>[443]</sup> | 3.4.113 <sup>[444]</sup> | Li Zefan <sup>[3]</sup><sup>[445]</sup> (formerly Greg Kroah-Hartman) | October 2016 <sup>[446]</sup> |  | 9th LTS release | 
| 3.3 | 18 March 2012 <sup>[447]</sup> | 3.3.8 <sup>[448]</sup> | Greg Kroah-Hartman | June 2012 <sup>[448]</sup> |  |  | 
| 3.2 | 4 January 2012 <sup>[449]</sup> | 3.2.102 <sup>[450]</sup> | Ben Hutchings <sup>[3]</sup><sup>[451]</sup> | May 2018 <sup>[452]</sup> |  | 8th LTS release, used in Ubuntu 12.04 LTS <sup>[453]</sup> and optionally in 12.04 ESM,<sup>[454]</sup> Debian 7 "Wheezy" and Slackware 14.0.<sup>[3]</sup><sup>[451]</sup> Canonical promised to (at least) provide long-term support until April 2017;<sup>[355]</sup> Support has continued for months after.3.2 to 3.5 was named Saber-toothed Squirrel<sup>[455]</sup> | 
| 3.1 | 24 October 2011 <sup>[456]</sup> | 3.1.10 <sup>[457]</sup> | Greg Kroah-Hartman | January 2012 <sup>[457]</sup> |  | 3.1 provided the base for real-time tree. 3.1-rc2 was named Wet Seal 3.1 was named Divemaster Edition <sup>[458]</sup> (Linus' diving activities) | 
| 3.0 | 21 July 2011 <sup>[367]</sup> | 3.0.101 <sup>[459]</sup> | Greg Kroah-Hartman <sup>[460]</sup> | October 2013 <sup>[459]</sup><sup>[460]</sup> |  | 7th LTS release Named Sneaky Weasel <sup>[462]</sup><sup>[463]</sup> | 
| **Legend:** |  |  |  |  |  |  | 

Versions 2.6.16 and 2.6.27 of the Linux kernel were unofficially given long-term support (LTS),<sup>[464]</sup> before a 2011 working group in the Linux Foundation started a formal long-term support initiative.<sup>[465]</sup><sup>[466]</sup>

| Version | Original release date | Last release | Maintainer | EOL | Prominent features | Notes | 
|---|---|---|---|---|---|---|
| 2.6.39 | 18 May 2011 <sup>[467]</sup> | 2.6.39.4 <sup>[468]</sup> | Greg Kroah-Hartman | August 2011 <sup>[468]</sup> |  | Last stable release of the 2.6 kernel series | 
| 2.6.38 | 14 March 2011 <sup>[469]</sup> | 2.6.38.8 <sup>[470]</sup> |  | June 2011 <sup>[470]</sup> |  | Named Flesh-Eating Bats with Fangs <sup>[471]</sup> | 
| 2.6.37 | 4 January 2011 <sup>[472]</sup> | 2.6.37.6 <sup>[473]</sup> |  | March 2011 <sup>[473]</sup> |  |  | 
| 2.6.36 | 20 October 2010 <sup>[474]</sup> | 2.6.36.4 <sup>[475]</sup> |  | February 2011 <sup>[475]</sup> |  |  | 
| 2.6.35 | 1 August 2010 <sup>[477]</sup> | 2.6.35.14 <sup>[478]</sup> | Andi Kleen <sup>[479]</sup> | March 2012 <sup>[479]</sup> |  | 6th LTS release 2.6.35.7 was named Yokohama | 
| 2.6.34 | 16 May 2010 <sup>[480]</sup> | 2.6.34.15 <sup>[481]</sup> | Paul Gortmaker <sup>[482]</sup> | February 2014 <sup>[481]</sup><sup>[482]</sup> |  | 5th LTS release It was named Sheep on Meth <sup>[483]</sup><sup>[484]</sup> | 
| 2.6.33 | 24 February 2010 <sup>[485]</sup> | 2.6.33.20 <sup>[486]</sup> | Greg Kroah-Hartman <sup>[487]</sup> | November 2011 <sup>[486]</sup> |  | 4th LTS release. It was the base for real-time-tree, replaced by 3.0.x. <sup>[487]</sup> | 
| 2.6.32 | 2 December 2009 <sup>[489]</sup> | 2.6.32.71 <sup>[490]</sup> | Willy Tarreau <sup>[3]</sup><sup>[491]</sup> (formerly Greg Kroah-Hartman)<sup>[492]</sup><sup>[493]</sup> | March 2016 <sup>[3]</sup> |  | 3rd LTS release, used in Debian 6 *Squeeze* .<sup>[495]</sup> Canonical also provided support until April 2015.<sup>[355]</sup>RHEL 6.x | 
| 2.6.31 | 9 September 2009 <sup>[496]</sup> | 2.6.31.14 <sup>[497]</sup> | Greg Kroah-Hartman | July 2010 <sup>[497]</sup> |  |  | 
| 2.6.30 | 9 June 2009 <sup>[499]</sup> | 2.6.30.10 <sup>[500]</sup> |  | December 2009 <sup>[500]</sup> |  | 2.6.30-rc4–2.6.30-rc6 was named Vindictive Armadillo<sup>[502]</sup><sup>[503]</sup> Releases between 2.6 and 2.9 were named 2.Man-Eating Seals of Antiquity <sup>[504]</sup> | 
| 2.6.29 | 23 March 2009 <sup>[505]</sup> | 2.6.29.6 <sup>[506]</sup> |  | July 2009 <sup>[506]</sup> |  | Named Temporary Tasmanian Devil <sup>[508]</sup><sup>[509]</sup> | 
| 2.6.28 | 24 December 2008 <sup>[510]</sup> | 2.6.28.10 <sup>[511]</sup> |  | May 2009 <sup>[511]</sup> |  | 2.6.28-rc1–2.6.28-rc6 was named Killer Bat of Doom <sup>[513]</sup><sup>[514]</sup>2.6.28 was named Erotic Pickled Herring<sup>[515]</sup> | 
| 2.6.27 | 9 October 2008 <sup>[516]</sup> | 2.6.27.62 <sup>[517]</sup> | Willy Tarreau <sup>[518]</sup> (formerly Adrian Bunk,<sup>[519]</sup> and formerly Greg Kroah-Hartman) | March 2012 <sup>[519]</sup> |  | 2nd LTS release 2.6.27.3 was named Trembling Tortoise <sup>[520]</sup> | 
| 2.6.26 | 13 July 2008 <sup>[521]</sup> | 2.6.26.8 <sup>[522]</sup> | Greg Kroah-Hartman | November 2008 <sup>[522]</sup> |  | 2.6.26–2.6.27 was named Rotary Wombat <sup>[523]</sup> | 
| 2.6.25 | 16 April 2008 <sup>[524]</sup> | 2.6.25.20 <sup>[525]</sup> |  | November 2008 <sup>[525]</sup> |  | Named Funky Weasel is Jiggy wit it <sup>[527]</sup> | 
| 2.6.24 | 24 January 2008 <sup>[528]</sup> | 2.6.24.7 <sup>[529]</sup> |  | May 2008 <sup>[529]</sup> |  | 2.6.23-rc4–2.6.23-rc6 was named Pink Farting Weasel <sup>[531]</sup>2.6.23-rc7–2.6.23–2.6.24 was named Arr Matey! A Hairy Bilge Rat!<sup>[532]</sup> (TLAPD 2007) 2.6.24.1 was named Err Metey! A Heury Beelge-a Ret! <sup>[533]</sup> | 
| 2.6.23 | 9 October 2007 <sup>[534]</sup> | 2.6.23.17 <sup>[535]</sup> |  | February 2008 <sup>[535]</sup> |  |  | 
| 2.6.22 | 8 July 2007 <sup>[537]</sup> | 2.6.22.19 <sup>[538]</sup> |  | February 2008 <sup>[538]</sup> |  | 2.6.22-rc3–2.6.22-rc4 was named Jeff Thinks I Should Change This, But To What? 2.6.22-rc5–2.6.22 was named Holy Dancing Manatees, Batman!<sup>[540]</sup> | 
| 2.6.21 | 25 April 2007 <sup>[541]</sup> | 2.6.21.7 <sup>[542]</sup> |  | August 2007 <sup>[542]</sup> |  | Named Nocturnal Monster Puppy <sup>[544]</sup> | 
| 2.6.20 | 4 February 2007 <sup>[545]</sup> | 2.6.20.21 <sup>[546]</sup> |  | October 2007 <sup>[546]</sup> |  | Named Homicidal Dwarf Hamster <sup>[548]</sup><sup>[549]</sup> | 
| 2.6.19 | 29 November 2006 <sup>[550]</sup> | 2.6.19.7 <sup>[551]</sup> |  | March 2007 <sup>[551]</sup> |  | Named Avast! A bilge rat! (TLAPD 2006) <sup>[555]</sup> | 
| 2.6.18 | 20 September 2006 <sup>[556]</sup> | 2.6.18.8 <sup>[557]</sup> |  | February 2007 <sup>[557]</sup>2.6.18: RHEL 5.x |  |  | 
| 2.6.17 | 17 June 2006 <sup>[559]</sup> | 2.6.17.14 <sup>[560]</sup> |  | October 2006 <sup>[560]</sup> |  | 2.6.17-rc5 was named Lordi Rules <sup>[562]</sup> (Eurovision 2006 winners)<sup>[563]</sup>2.6.17-rc6–2.6.17 was named Crazed Snow-Weasel<sup>[564]</sup> | 
| 2.6.16 | 20 March 2006 <sup>[565]</sup> | 2.6.16.62 <sup>[566]</sup> | Adrian Bunk <sup>[567]</sup> (formerly Greg Kroah-Hartman)<sup>[492]</sup> | July 2008 <sup>[568]</sup><sup>[566]</sup> |  | 1st LTS release 2.6.16.28-rc2 was named Stable Penguin | 
| 2.6.15 | 2 January 2006 <sup>[570]</sup> | 2.6.15.7 <sup>[571]</sup> | Greg Kroah-Hartman | March 2006 <sup>[571]</sup> |  | Named Sliding Snow Leopard <sup>[573]</sup> | 
| 2.6.14 | 27 October 2005 <sup>[574]</sup> | 2.6.14.7 <sup>[575]</sup> |  | January 2006 <sup>[575]</sup> |  | Named Affluent Albatross <sup>[577]</sup> | 
| 2.6.13 | 28 August 2005 <sup>[578]</sup> | 2.6.13.5 <sup>[579]</sup> |  | December 2005 <sup>[579]</sup> |  | Named Woozy Numbat <sup>[581]</sup><sup>[582]</sup> The 2.6.12 release was the first one managed by Git.<sup>[583]</sup> | 
| 2.6.12 | 18 June 2005 <sup>[584]</sup> | 2.6.12.6 <sup>[585]</sup><sup>[586]</sup> |  | August 2005 <sup>[585]</sup> |  |  | 
| 2.6.11 | 2 March 2005 <sup>[587]</sup> | 2.6.11.12 <sup>[588]</sup> |  | June 2005 <sup>[588]</sup> |  |  | 
| 2.6.10 | 24 December 2004 <sup>[589]</sup> |  |  |  |  |  | 
| 2.6.9 | 18 October 2004 <sup>[591]</sup> |  |  |  |  |  | 
| 2.6.8 | 14 August 2004 <sup>[592]</sup> |  |  |  |  |  | 
| 2.6.7 | 16 June 2004 <sup>[593]</sup> |  |  |  |  |  | 
| 2.6.6 | 10 May 2004 <sup>[594]</sup> |  |  |  |  |  | 
| 2.6.5 | 4 April 2004 <sup>[596]</sup> |  |  |  |  |  | 
| 2.6.4 | 11 March 2004 <sup>[598]</sup> |  |  |  |  |  | 
| 2.6.3 | 18 February 2004 <sup>[600]</sup> |  |  |  |  |  | 
| 2.6.2 | 4 February 2004 <sup>[602]</sup> |  |  |  |  |  | 
| 2.6.1 | 9 January 2004 <sup>[604]</sup> |  |  |  |  |  | 
| 2.6 | 17 December 2003 <sup>[606]</sup> |  | Linus Torvalds | December 2004 <sup>[607]</sup> |  | 2.6.2–2.6.4 was named Feisty Dunnart <sup>[611]</sup>2.6.5–2.6.9 was named Zonked Quokka <sup>[612]</sup> 2.6.9: RHEL 4.xThe 2.5 kernels were development kernels<sup>[613]</sup> | 
| **Legend:** |  |  |  |  |  |  | 

| Version | Original release date | Last release | Maintainer | EOL | Prominent features | Notes | 
|---|---|---|---|---|---|---|
| 2.4 | 4 January 2001 <sup>[614]</sup> | 2.4.37.11 <sup>[615]</sup> | Willy Tarreau (formerly Marcelo Tosatti) | December 2011 <sup>[615]</sup> |  | The 2.3 kernels were development kernels<sup>[613]</sup> 2.4.9: RHEL 2.1 2.4.10: Featured a complete rewrite of the Virtual Memory Management (VMM) subsystem. <sup>[620]</sup> 2.4.21: RHEL 3.x | 
| 2.2 | 26 January 1999 <sup>[621]</sup> | 2.2.26 <sup>[622]</sup> | Marc-Christian Petersen (formerly Alan Cox) <sup>[623]</sup> | Made unofficially obsolete with the 2.2.27-rc2 <sup>[624]</sup><sup>[625]</sup> |  | The 2.1 kernels were development kernels <sup>[613]</sup> | 
| 2.0 | 9 June 1996 <sup>[630]</sup> | 2.0.40 <sup>[631]</sup> | David Weinehall | officially made obsolete with the kernel 2.2.0 release <sup>[632]</sup> |  | Larry Ewing created the Tux mascot in 1996 | 
| 1.3 | 12 June 1995 | 1.3.100 <sup>[634]</sup> | Linus Torvalds | EOL |  | Greased Weasel <sup>[640]</sup> | 
| 1.2 | 7 March 1995 | 1.2.13 |  |  |  | Linux '95 <sup>[641]</sup> | 
| 1.1 | 6 April 1994 | 1.1.95 |  |  |  |  | 
| 1.0 | 14 March 1994 | 1.0.9 |  |  |  |  | 
| 0.99 | 13 December 1992 | 0.99.15j <sup>[645]</sup> |  |  |  | The Linux 0.99 tar.bz2 archive grew from 426 kB to 1009 kB on the way to 1.0. | 
| 0.98 | 29 September 1992 | 0.98.6 <sup>[648]</sup> |  |  |  |  | 
| 0.97 | 1 August 1992 | 0.97.6 <sup>[651]</sup> |  |  |  |  | 
| 0.96 | 22 May 1992 | 0.96c.2 <sup>[654]</sup> |  |  |  |  | 
| 0.95 | 8 March 1992 | 0.95c+ <sup>[656]</sup> |  |  |  | Jump from 0.12 to 0.95 <sup>[659]</sup>First version released under the GPL.<sup>[658]</sup><sup>[660]</sup> Although the license change took effect as of the first of February 1992. | 
| 0.12 | 15 January 1992 |  |  |  |  |  | 
| 0.11 | 8 December 1991 |  |  |  |  | First kernel where other people start making real contributions <sup>[661]</sup> | 
| 0.10 | November 1991 |  |  |  |  | Jump from 0.03 to 0.10 First release where Minix isn't needed anymore<sup>[663]</sup> | 
| 0.03 | October 1991 <sup>[663]</sup> |  |  |  |  |  | 
| 0.02 | 5 October 1991 |  |  |  |  | First "usable" release; for wider distribution <sup>[664]</sup> | 
| 0.01 | 17 September 1991 |  |  |  |  |  | 
| **Legend:** |  |  |  |  |  |  | 

1. 1 2 Linus Torvalds (18 October 2026). "Linux 7.3". Retrieved 18 October 2026.
2. ↑ Edge, Jake (6 October 2021). "Rolling stable kernels". *LWN.net* . Retrieved 15 February 2026.
3. 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 "Active kernel releases". Kernel.org. 7 February 2023. Retrieved 22 September 2024.
4. 1 2 3 4 5 6 7 8 9 10 11 "CIP maintenance". Retrieved 7 September 2023.
5. ↑ "LKML: Linus Torvalds: Linux 7.0-rc1". *lkml.org* . Retrieved 11 April 2026.
6. ↑ "Linux 7.0 Ready For Release With Many Exciting Changes". *www.phoronix.com* . Retrieved 11 April 2026.
7. ↑ "Linux 7.4-rc3 - kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . Retrieved 15 November 2026.
8. ↑ "Btrfs updates for kernel 7.3". *lore.kernel.org* . Retrieved 29 August 2026.
9. ↑ "Ext4 changes for 7.3". *lore.kernel.org* . Retrieved 29 August 2026.
10. ↑ "Fix cluster scheduling in the presence of asymmetric capacity". *lore.kernel.org* . Retrieved 29 August 2026.
11. ↑ "KSM: performance optimizations for rmap_walk_ksm". *lore.kernel.org* . Retrieved 30 August 2026.
12. ↑ "mm/zsmalloc: reduce lock contention in zs_free()". *lore.kernel.org* . Retrieved 30 August 2026.
13. ↑ "smp/core for v7.3-rc1". *lore.kernel.org* . Retrieved 30 August 2026.
14. ↑ "Crypto library updates for 7.3". *lore.kernel.org* . Retrieved 30 August 2026.
15. ↑ "NFSD updates for v7.3". *lore.kernel.org* . Retrieved 30 August 2026.
16. ↑ "Linux 7.3 FUSE Delivering Nice Performance Improvements". *www.phoronix.com* . Retrieved 30 August 2026.
17. ↑ "fuse: add io-uring buffer pools and zero-copy". *lore.kernel.org* . Retrieved 30 August 2026.
18. ↑ "f2fs: introduce metadata cache". *lore.kernel.org* . Retrieved 31 August 2026.
19. ↑ "[PATCH v2 0/3] Fix RV1106 SAR-ADC support". *lore.kernel.org* . Retrieved 31 August 2026.
20. ↑ "[GIT PULL] Staging driver update for 7.3-rc2". *lore.kernel.org* . Retrieved 31 August 2026.
21. ↑ "[GIT PULL] Char/Misc/IIO driver driver updates for 7.3-rc1". *lore.kernel.org* . Retrieved 31 August 2026.
22. ↑ "[GIT PULL] 9p regression fix for 7.3-rc4". *lore.kernel.org* . Retrieved 17 September 2026.
23. ↑ "[GIT PULL] Please pull powerpc/linux.git powerpc-7.3-4 tag". *lore.kernel.org* . Retrieved 17 September 2026.
24. ↑ "[GIT PULL] i2c fixes for v7.3-rc4". *lore.kernel.org* . Retrieved 22 September 2026.
25. ↑ "[git pull] drm fixes for 7.3-rc6". *lore.kernel.org* . Retrieved 3 October 2026.
26. ↑ "[PATCH] drm/dp: keep MOT set between messages when no_zero_sized is set". *lore.kernel.org* . Retrieved 3 October 2026.
27. ↑ "[PATCH 0/3] nouveau: fix 3 null-ptr derefs". *lore.kernel.org* . Retrieved 3 October 2026.
28. ↑ "[PATCH 1/3] drm/nouveau: Fix NULL pointer dereferences in GETPARAM ioctl". *lore.kernel.org* . Retrieved 3 October 2026.
29. ↑ "[PATCH 2/3] drm/nouveau: Fix NULL pointer dereference in GET_ZCULL_INFO ioctl". *lore.kernel.org* . Retrieved 3 October 2026.
30. ↑ "[PATCH 3/3] drm/nouveau/gsp: Fix NULL dereference in nvkm_gsp_gcx_ready()". *lore.kernel.org* . Retrieved 3 October 2026.
31. ↑ "[RFC PATCH] drm/exec: track the first few locked objects inline". *lore.kernel.org* . Retrieved 3 October 2026.
32. ↑ Linus Torvalds (16 August 2026). "Linux 7.2". Retrieved 16 August 2026.
33. ↑ Kroah-Hartman, Greg. "Linux 7.2.15". *lore.kernel.org* . Retrieved 1 November 2026.
34. ↑ "Cache Aware Scheduling Merged For Linux 7.2 For Boosting Modern Intel & AMD CPUs". *www.phoronix.com* . Retrieved 22 August 2026.
35. ↑ "Apple M3 Booting On Linux After Three Years Plus Other SoC Updates In Linux 7.2". *www.phoronix.com* . Retrieved 22 August 2026.
36. ↑ "USB4STREAM Merged For Linux 7.2 To Quickly Send Data Between USB4 Connected Systems". *www.phoronix.com* . Retrieved 22 August 2026.
37. ↑ "AF_ALG Deprecation Approved For Linux 7.2, Useless & Insecure Crypto Driver Code Removed". *www.phoronix.com* . Retrieved 22 August 2026.
38. ↑ "Linux 7.2 Preparing Intel Key Protection Technology "KPT" For Next-Gen QAT". *www.phoronix.com* . Retrieved 22 August 2026.
39. ↑ "AMD Submits Its Long-Awaited HDMI 2.1 FRL Support For Linux 7.2 AMDGPU". *www.phoronix.com* . Retrieved 22 August 2026.
40. ↑ "Btrfs Now Enables Large Folios By Default, Lands Huge Folios With Linux 7.2". *www.phoronix.com* . Retrieved 22 August 2026.
41. ↑ "Linux Finally Eliminates The strncpy API After Six Years Of Work, 360+ Patches". *www.phoronix.com* . Retrieved 22 August 2026.
42. ↑ Linus Torvalds (14 June 2026). "Linux 7.1". Retrieved 14 June 2026.
43. ↑ Kroah-Hartman, Greg (2 September 2026). "Linux 7.1.13". *lkml* (Mailing list).
44. ↑ "Linux 7.1.13". *lore.kernel.org* . Retrieved 2 September 2026.
45. ↑ "Linux 7.1-rc1 Released With New NTFS Driver, FRED By Default & Much More". *www.phoronix.com* . Retrieved 31 May 2026.
46. ↑ "Linus Torvalds Merged The Code Beginning To Remove Intel 486 CPU Support In Linux 7.1". *www.phoronix.com* . Retrieved 31 May 2026.
47. ↑ "Linux 7.1 Lands The Apple SMC Power Driver For Reporting Battery Metrics On MacBooks". *www.phoronix.com* . Retrieved 31 May 2026.
48. ↑ "Linux 7.1 Is A Big Win For Intel Panther Lake With FRED Enabled By Default". *www.phoronix.com* . Retrieved 31 May 2026.
49. ↑ "Flexible Return and Event Delivery (FRED) Specification". *Intel* . Retrieved 31 May 2026.
50. ↑ "With Linux 7.1 The Mainline Kernel Now Supports Real-Time "RT" On ARM". *www.phoronix.com* . Retrieved 31 May 2026.
51. ↑ "Verisilicon DC8200 & Coreboot Framebuffer Drivers Sent To DRM-Next For Linux 7.1". *www.phoronix.com* . Retrieved 31 May 2026.
52. ↑ "Intel QAT Zstd, QAT Gen6 Improvements Merged For Linux 7.1". *www.phoronix.com* . Retrieved 31 May 2026.
53. ↑ "Intel® QuickAssist Technology (Intel® QAT) Improves Data Center..." *Intel* . Retrieved 31 May 2026.
54. ↑ "Linux 7.1 Will Bring Power Estimate Reporting For AMD Ryzen AI NPUs". *www.phoronix.com* . Retrieved 31 May 2026.
55. ↑ "Linus Torvalds Merged The Code Beginning To Remove Intel 486 CPU Support In Linux 7.1". *www.phoronix.com* . Retrieved 31 May 2026.
56. ↑ "Linux 7.0". 12 April 2026. Retrieved 12 April 2026.
57. ↑ "Linux 7.0.14". *lore.kernel.org* . Retrieved 30 August 2026.
58. ↑ "Linux 7.0.14 - Greg Kroah-Hartman". *lore.kernel.org* . Retrieved 27 June 2026.
59. ↑ Ph.D, Shlomi Boutnaru (24 April 2025). "The Linux Concept Journey — fsnotify (File System Notification)". *Medium* . Retrieved 31 May 2026.
60. ↑ "Linux_7.0 - Linux Kernel Newbies". *kernelnewbies.org* . Retrieved 31 May 2026.
61. ↑ Corbet, Jonathan (23 January 2026). "Filesystem medley: EROFS, NTFS, and XFS". *LWN.net* . Retrieved 31 May 2026.
62. ↑ "XFS Online Fsck Design — The Linux Kernel documentation". *www.kernel.org* . Retrieved 31 May 2026.
63. ↑ "Thread Safety Analysis — Clang 23.0.0git documentation". *clang.llvm.org* . Retrieved 31 May 2026.
64. ↑ Corbet, Jonathan (10 March 2025). "Capability analysis for the kernel". *LWN.net* . Retrieved 31 May 2026.
65. ↑ Corbet, Jonathan (19 January 2026). "Task-level io_uring restrictions". *LWN.net* . Retrieved 31 May 2026.
66. ↑ "OPEN_TREE_NAMESPACE To Provide A Security & Performance Win For Dealing With Containers". *www.phoronix.com* . Retrieved 31 May 2026.
67. ↑ Corbet, Jonathan (2 February 2026). "Modernizing swapping: introducing the swap table". *LWN.net* . Retrieved 31 May 2026.
68. ↑ Corbet, Jonathan (5 February 2026). "Modernizing swapping: the end of the swap map". *LWN.net* . Retrieved 31 May 2026.
69. ↑ Corbet, Jonathan (18 September 2025). "Extending the time-slice-extension discussion". *LWN.net* . Retrieved 31 May 2026.
70. ↑ Corbet, Jonathan (21 September 2023). "Revisiting the kernel's preemption models (part 1)". *LWN.net* . Retrieved 31 May 2026.
71. ↑ Corbet, Jonathan (2 October 2023). "Revisiting the kernel's preemption model, part 2". *LWN.net* . Retrieved 31 May 2026.
72. ↑ Corbet, Jonathan (18 February 2026). "More accurate congestion notification for TCP". *LWN.net* . Retrieved 31 May 2026.
73. ↑ btrfs. "Logical mapping tree feature (relocation rework) · Issue #54 · btrfs/btrfs-todo". *GitHub* . Retrieved 31 May 2026.
74. ↑ Corbet, Jonathan (16 March 2026). "A safer kmalloc() for 7.0". *LWN.net* . Retrieved 31 May 2026.
75. ↑ Jacobs, Skye (9 February 2026). "Torvalds kicks off Linux 7.0 after 6.19 lands with key tech gains". *TechSpot* . Retrieved 31 May 2026.
76. ↑ Torvald, Linus (8 February 2026). "Linux 6.19". *Linux Kernel Mailing List* (Mailing list). Retrieved 12 April 2026.
77. ↑ Kroah-Hartman, Greg (22 April 2026). "Linux 6.19.14". *Linux Kernel Mailing List* (Mailing list). Retrieved 27 April 2026.
78. ↑ Corbet, Jonathan (3 November 2025). "Namespace reference counting and listns()". *LWN.net* . Retrieved 31 May 2026.
79. ↑ Corbet, Jonathan (18 August 2025). "Kexec handover and the live update orchestrator". *LWN.net* . Retrieved 31 May 2026.
80. ↑ "Live Update Orchestrator "LUO" Merged For Linux 6.19". *www.phoronix.com* . Retrieved 31 May 2026.
81. ↑ "IDE and TDISP: An Overview of PCIe® Technology Security Features | PCI-SIG". *pcisig.com* . Retrieved 31 May 2026.
82. ↑ "Linux 6.19 Introduces PCIe Link Encryption & Device Authentication, AMD SEV-TIO Enabling". *www.phoronix.com* . Retrieved 31 May 2026.
83. ↑ "Btrfs In Linux 6.19 Adds Experimental Features, Continues Preparations For FSCRYPT". *www.phoronix.com* . Retrieved 31 May 2026.
84. ↑ "[PATCH v4 00/24] ext4: enable block size larger than page size". *lore.kernel.org* . Retrieved 31 May 2026.
85. ↑ "Linux Color Pipeline API — The Linux Kernel documentation". *docs.kernel.org* . Retrieved 31 May 2026.
86. ↑ "NVIDIA Improves Block Layer Peer-To-Peer DMA In Linux 6.19". *www.phoronix.com* . Retrieved 31 May 2026.
87. ↑ Corbet, Jonathan (11 July 2025). "SFrame-based stack unwinding for the kernel". *LWN.net* . Retrieved 31 May 2026.
88. ↑ Active kernel releases on kernel.org
89. ↑ "Linux 6.18.64". *git.kernel.org* . Retrieved 18 November 2026.
90. 1 2 3 Corbet, Jonathan (25 February 2026). "Support period lengthened for the 6.6, 6.12, and 6.18 kernels". *LWN.net* . Retrieved 25 February 2026.
91. 1 2 3 Kroah-Hartman, Greg (25 February 2026). "bump the longterm EOL dates to a be a bit longer than previously documented". *Kernel.org git repositories* . Retrieved 25 February 2026.
92. ↑ Corbet, Jonathan (1 April 2025). "Slab allocator: sheaves and any-context allocations". *LWN.net* . Retrieved 29 November 2025.
93. ↑ "dm-pcache — Persistent Cache — The Linux Kernel documentation". *docs.kernel.org* . Retrieved 29 November 2025.
94. ↑ "[PATCH v2 00/33] ns: support file handles". *lore.kernel.org* . Retrieved 29 November 2025.
95. 1 2 3 "Linux_6.18 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 29 November 2025.
96. ↑ "PSP Security Protocol — The Linux Kernel documentation". *docs.kernel.org* . Retrieved 29 November 2025.
97. ↑ google. "psp/doc/PSP_Arch_Spec.pdf at main · google/psp" (PDF). *GitHub* . Retrieved 29 November 2025.
98. ↑ Corbet, Jonathan (23 May 2024). "A new swap abstraction layer for the kernel". *LWN.net* . Retrieved 29 November 2025.
99. ↑ "Swap Table — The Linux Kernel documentation". *docs.kernel.org* . Retrieved 29 November 2025.
100. ↑ "[PATCH v4 00/15] mm, swap: introduce swap table as swap cache (phase I)". *lore.kernel.org* . Retrieved 29 November 2025.
101. ↑ Alden, Daroc (12 August 2025). "Possible paths for signing BPF programs". *LWN.net* . Retrieved 29 November 2025.
102. ↑ "MatthewWilcox/Memdescs - Linux Kernel Newbies". *KernelNewbies* . Retrieved 29 November 2025.
103. ↑ Moola, Vishal (25 June 2024). "Introducing Memdesc". *Oracle Linux Blog* . Retrieved 4 April 2026.
104. ↑ Torvalds, Linus (29 September 2025). "Remove bcachefs core code - kernel/git/torvalds/linux.git - Linux kernel source tree". Retrieved 6 February 2026.
105. 1 2 "The Linux Kernel Archives - Releases". *www.kernel.org* . Retrieved 5 December 2024.
106. ↑ Torvalds, Linus (28 September 2025). "Linux 6.17".
107. ↑ "Linux 6.17.13". *git.kernel.org* . 18 December 2025. Retrieved 18 November 2026.
108. ↑ "Attack Vector Controls — The Linux Kernel documentation". *docs.kernel.org* . Retrieved 29 November 2025.
109. ↑ Alden, Daroc (19 March 2025). "Better CPU vulnerability mitigation configuration". *LWN.net* . Retrieved 29 November 2025.
110. 1 2 3 "Linux_6.17 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 29 November 2025.
111. 1 2 Corbet, Jonathan (6 June 2025). "Slowing the flow of core-dump-related CVEs". *LWN.net* . Retrieved 29 November 2025.
112. ↑ Corbet, Jonathan (29 July 2025). "A proxy-execution baby step". *LWN.net* . Retrieved 29 November 2025.
113. ↑ Corbet, Jonathan (10 June 2025). "An end to uniprocessor configurations". *LWN.net* . Retrieved 29 November 2025.
114. ↑ "Runtime Verification — The Linux Kernel documentation". *docs.kernel.org* . Retrieved 29 November 2025.
115. ↑ "Real-time application monitors — The Linux Kernel documentation". *docs.kernel.org* . Retrieved 29 November 2025.
116. ↑ Alden, Daroc (30 July 2025). "Extending run-time verification for the kernel". *LWN.net* . Retrieved 29 November 2025.
117. ↑ Torvalds, Linus (28 August 2025). "Maintainers: mark bcachefs externally maintained - kernel/git/torvalds/linux.git - Linux kernel source tree". Retrieved 6 February 2026.
118. ↑ Torvalds, Linus (27 July 2025). "Linux 6.16".
119. ↑ "Linux 6.16.12". *git.kernel.org* . 12 October 2025. Retrieved 18 November 2026.
120. ↑ Edge, Jake (9 April 2025). "An update on torn-write protection". *LWN.net* . Retrieved 29 November 2025.
121. 1 2 "Linux_6.16 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 29 November 2025.
122. ↑ "Intel Trust Domain Extensions (TDX) — The Linux Kernel documentation". *docs.kernel.org* . Retrieved 29 November 2025.
123. ↑ "Documentation for Intel® Trust Domain Extensions". *Intel* . Retrieved 29 November 2025.
124. ↑ "Device Memory TCP — The Linux Kernel documentation". *docs.kernel.org* . Retrieved 29 November 2025.
125. ↑ Corbet, Jonathan (27 June 2024). "Direct-to-device networking". *LWN.net* . Retrieved 29 November 2025.
126. ↑ Corbet, Jonathan (15 April 2025). "Automatic tuning for weighted interleaving". *LWN.net* . Retrieved 29 November 2025.
127. ↑ "Introducing Intel® Advanced Performance Extensions (Intel® APX)". *Intel* . Retrieved 29 November 2025.
128. ↑ "[PATCH v12 00/21] futex: Add support task local hash maps, FUTEX2_NUMA and FUTEX2_MPOL". *lore.kernel.org* . Retrieved 29 November 2025.
129. ↑ "[PATCH v2 8/8] ext4: enable large folio for regular file". *lore.kernel.org* . Retrieved 29 November 2025.
130. ↑ Torvalds, Linus (25 May 2025). "Linux 6.15".
131. ↑ "Linux 6.15.11". *git.kernel.org* . 20 August 2025. Retrieved 18 November 2026.
132. ↑ "Btrfs Adding Fast/Realtime Zstd Compression & Other Performance Optimizations". *Phoronix* . Retrieved 31 July 2025.
133. ↑ Torvalds, Linus (24 March 2025). "Linux 6.14".
134. ↑ "Linux 6.14.11". *git.kernel.org* . 10 June 2025. Retrieved 18 November 2026.
135. ↑ "Linux 6.14.11". *lore.kernel.org* . Retrieved 10 June 2025.
136. ↑ "Linux-6.14-Char-Misc-NTSYNC".
137. ↑ "ntsync-for-proton-wine-now-in-linux-kernel-6-14-that-should-make-many-steamos-users-happy/". 28 January 2025.
138. ↑ Speed, Richard (24 January 2025). "Linux rolls out the welcome mat for Microsoft's Copilot key". *The Register* . Retrieved 5 April 2025.
139. ↑ "Linux 6.13". *lore.kernel.org* . Retrieved 21 January 2025.
140. 1 2 3 4 5 6 7 8 9 10 11 12 "Tags - kernel/git/stable/linux.git". *git.kernel.org* . Retrieved 18 November 2026.
141. ↑ "linux-kernel.vger.kernel.org archive mirror". *lore.kernel.org* . Retrieved 21 April 2025.
142. ↑ Torvalds, Linus (17 November 2024). "Linux 6.12".
143. ↑ "Linux 6.12.118". *git.kernel.org* . Retrieved 18 November 2026.
144. 1 2 "Linux 6.12 Features Are Super Exciting With Real-Time, Sched_ext, Intel Xe2 & Raspberry Pi 5". *Phoronix* . Retrieved 22 November 2024.
145. ↑ "Linux 6.12 Features Are Super Exciting With Real-Time, Sched_ext, Intel Xe2 & Raspberry Pi 5 (page 2)". *Phoronix* . Retrieved 22 November 2024.
146. ↑ rnkenchor (13 January 2025). "Kernel 6.12 will have 10 years support via CIP – Are all your maintenance problems solved?". *Civil Infrastructure Platform* . Retrieved 20 March 2025.
147. ↑ "Release Notes for Debian 13 (trixie)". *Debian* .
148. ↑ "Trixie kernel will be Linux 6.12 LTS". *Debian Wiki* .
149. ↑ "Release Notes for RHEL 10.0". *Redhat.com* .
150. ↑ Torvalds, Linus (15 September 2024). "Linux 6.11".
151. 1 2 "Linux-Kernel Archive: Linux 6.11.11". *lkml.iu.edu* . Retrieved 21 January 2025.
152. ↑ Corbet, Jonathan (18 July 2024). "The first half of the 6.11 merge window [LWN.net]". *LWN.net* . Retrieved 5 October 2024.
153. 1 2 Corbet, Jonathan (29 July 2024). "The rest of the 6.11 merge window [LWN.net]". *LWN.net* . Retrieved 5 October 2024.
154. ↑ Torvalds, Linus (14 July 2024). "Linux 6.10".
155. ↑ Greg Kroah-Hartman (10 October 2024). "Linux 6.10.14". Retrieved 11 October 2024.
156. 1 2 Sneddon, Joey (14 July 2024). "Linux Kernel 6.10 Released with LOTS of Epic Changes". *OMG! Ubuntu* . Retrieved 3 October 2024.
157. ↑ "Linux 6.10-rc1 - kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . Retrieved 16 June 2024.
158. ↑ "Linux 6.10 Honors One Last ReiserFS Request Made By Hans Reiser". *Phoronix* . Retrieved 18 January 2026.
159. ↑ Torvalds, Linus (12 May 2024). "Linux 6.9".
160. ↑ Greg Kroah-Hartman (29 July 2024). "Re: Linux 6.9.12". Retrieved 30 July 2024.
161. 1 2 3 4 5 6 "Linux 6.9 Features: DM VDO, AMD Preferred Core, Intel FRED & Larger Console Fonts". *Phoronix* . Retrieved 15 May 2024.
162. ↑ "Linux_6.9 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 19 July 2024.
163. ↑ Torvalds, Linus (10 March 2024). "Linux 6.8".
164. ↑ Kroah-Hartman, Greg (30 May 2024). "Linux 6.8.12". *lore.kernel.org* . Retrieved 8 June 2024.
165. ↑ Corbet, Jonathan (12 June 2023). "Deadline servers as a realtime throttling replacement". *LWN.net* . Retrieved 29 November 2025.
166. ↑ Corbet, Jonathan (6 July 2023). "Large folios for anonymous memory". *LWN.net* . Retrieved 29 November 2025.
167. ↑ Corbet, Jonathan (10 November 2023). "listmount() and statmount()". *LWN.net* . Retrieved 29 November 2025.
168. ↑ Squires, Julian (21 December 2023). "Data-type profiling for perf". *LWN.net* . Retrieved 29 November 2025.
169. ↑ Corbet, Jonathan (21 August 2023). "Defending mounted filesystems from the root user". *LWN.net* . Retrieved 29 November 2025.
170. ↑ Edge, Jake (4 January 2023). "Adding system calls for Linux security modules". *LWN.net* . Retrieved 29 November 2025.
171. ↑ "Linux_6.8 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 29 November 2025.
172. ↑ "Ubuntu 24.04 LTS (Noble Numbat) Release Notes". *Ubuntu Community Hub* . 16 June 2025. Retrieved 28 January 2026.
173. ↑ Torvalds, Linus (7 January 2024). "Linux 6.7".
174. 1 2 3 4 5 "Linux 6.7 Released With Bcachefs, Intel Meteor Lake In Good Shape & Nouveau GSP Support". *Phoronix* . Retrieved 12 January 2024.
175. 1 2 3 4 "Linux 6.7 Features Include Bcachefs, Stable Meteor Lake Graphics, NVIDIA GSP & More Next-Gen Hardware". *Phoronix* . Retrieved 12 January 2024.
176. ↑ "Btrfs Picks Up New Features For Linux 6.7". *Phoronix* . Retrieved 12 January 2024.
177. ↑ Torvalds, Linus (29 October 2023). "Linux 6.6".
178. ↑ "Linux 6.6.162". *git.kernel.org* . 6 November 2026. Retrieved 18 November 2026.
179. ↑ "Linux 6.6". *KernelNewbies* . 29 November 2023. Retrieved 4 April 2026.
180. ↑ Larabel, Michael (14 July 2023). "Linux 6.6 Will Make It Easy To Disable IO_uring System-Wide". *Phoronix* . Retrieved 4 April 2026.
181. ↑ Torvalds, Linus (27 August 2023). "Linux 6.5".
182. ↑ Kroah-Hartman, Greg (28 November 2023). "Linux 6.5.13". *lore.kernel.org* . Retrieved 5 December 2023.
183. 1 2 "Linux_6.5 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 5 October 2024.
184. ↑ Torvalds, Linus (25 June 2023). "Linux 6.4".
185. ↑ Kroah-Hartman, Greg (13 September 2023). "Linux 6.4.16". *lore.kernel.org* . Retrieved 13 September 2023.
186. ↑ "Intel Linear Address Masking "LAM" Merged Into Linux 6.4". *Phoronix* . Retrieved 24 January 2024.
187. 1 2 3 4 5 6 7 8 9 10 11 12 "Linux 6.4 Features: Many Intel & AMD Additions, Better Desktop/Laptop Hardware Drivers". *Phoronix* . Retrieved 24 January 2024.
188. ↑ "AMD Guided Autonomous Mode Submitted For Linux 6.4". *Phoronix* . Retrieved 24 January 2024.
189. ↑ Torvalds, Linus (23 April 2023). "Linux 6.3".
190. ↑ Kroah-Hartman, Greg (11 July 2023). "Linux 6.3.13". *lore.kernel.org* . Retrieved 11 July 2023.
191. ↑ Torvalds, Linus (19 February 2023). "Linux 6.2".
192. ↑ Kroah-Hartman, Greg (17 May 2023). "Linux 6.2.16". *lore.kernel.org* . Retrieved 17 May 2023.
193. ↑ Torvalds, Linus (11 December 2022). "Linux 6.1". *LKML* (Mailing list).
194. ↑ "Linux 6.1.192". *git.kernel.org* . 6 November 2026. Retrieved 18 November 2026.
195. ↑ Larabel, Michael (3 October 2022). "The Initial Rust Infrastructure Has Been Merged Into Linux 6.1". *Phoronix* . Retrieved 13 December 2022.
196. ↑ Yu Zhao (18 August 2022). "mm: multi-gen LRU: design doc". *Kernel.org git repositories* . Retrieved 3 June 2023.
197. ↑ Sneddon, Joey (12 December 2022). "Linux Kernel 6.1 Released, This is What's New". *OMG! Ubuntu* . Retrieved 13 January 2023.
198. ↑ "Linux kernel 6.1 is out now". *GamingOnLinux* . 12 December 2022. Retrieved 12 December 2022.
199. ↑ "Debian 12 -- Release Notes". *Debian* . Retrieved 20 June 2023.
200. ↑ "Civil Infrastructure Platform". *Linux Foundation Wiki* .
201. ↑ "kernel/git/stable/linux.git - Linux kernel stable tree". *Kernel.org git repositories* . Retrieved 10 September 2023.
202. ↑ "Linux 6.0". *lore.kernel.org* . Retrieved 8 October 2022.
203. 1 2 "Linux 6.0.19 - Greg Kroah-Hartman". *lore.kernel.org* . Retrieved 28 February 2023.
204. ↑ "Linux Kernel 6.0 Released, This is What's New". *OMG! Ubuntu* . 2 October 2022. Retrieved 3 October 2022.
205. ↑ "Linux 6.0 Supporting New Intel/AMD Hardware, Performance Improvements & Much More". *Phoronix* . Retrieved 3 June 2023.
206. ↑ "Makefile - kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . Retrieved 5 October 2022.
207. ↑ Torvalds, Linus (31 July 2022). "Linux 5.19". *LKML* (Mailing list). Retrieved 9 October 2022.
208. 1 2 Kroah-Hartman, Greg (24 October 2022). "Linux 5.19.17". *LKML* (Mailing list). Retrieved 27 October 2022.
209. 1 2 3 4 "Linux_5.19 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 10 August 2022.
210. 1 2 3 4 "Linux 5.19 Features: AMD SEV-SNP + Zen 4 Prep, Intel TDX + IFS, LoongArch, Big TCP, Apple M1 NVMe". *Phoronix* . Retrieved 10 August 2022.
211. ↑ "Linux 5.19 Finally Removes Obsolete x86 a.out support - Phoronix". *Phoronix* . Retrieved 25 August 2024.
212. ↑ Torvalds, Linus (22 May 2022). "Linux 5.18". *LKML* (Mailing list). Retrieved 24 May 2022.
213. 1 2 Kroah-Hartman, Greg (21 August 2022). "Linux 5.18.19". *LKML* (Mailing list). Retrieved 5 September 2022.
214. ↑ "Indirect Branch Tracking - 006 - ID:655258 | 12th Generation Intel® Core™ Processors". *edc.intel.com* . Retrieved 11 July 2022.
215. ↑ Corbet, Jonathan (31 March 2022). "Indirect branch tracking for Intel CPUs [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
216. ↑ "user_events: User-based Event Tracing — The Linux Kernel documentation". *www.kernel.org* . Retrieved 11 July 2022.
217. ↑ "Fprobe - Function entry/exit probe — The Linux Kernel documentation". *www.kernel.org* . Retrieved 11 July 2022.
218. ↑ Corbet, Jonathan (2 January 2022). "The fast kernel headers tree [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
219. ↑ Corbet, Jonathan (30 July 2021). "Strict memcpy() bounds checking for the kernel [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
220. ↑ Corbet, Jonathan (24 February 2022). "Moving the kernel to modern C [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
221. ↑ Torvalds, Linus (20 March 2022). "Linux 5.17". *LKML* (Mailing list). Retrieved 12 May 2022.
222. 1 2 Kroah-Hartman, Greg (14 June 2022). "Linux 5.17.15". *LKML* (Mailing list). Retrieved 22 June 2022.
223. ↑ "BPF CO-RE reference guide". *nakryiko.com* . Retrieved 11 July 2022.
224. ↑ Corbet, Jonathan (29 November 2021). "A different approach to BPF loops [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
225. ↑ "Random number generator enhancements for Linux 5.17 and 5.18". *zx2c4* . Retrieved 11 July 2022.
226. ↑ Daniel (16 January 2022). "And now Linux has a Real-Time Linux Analysis (RTLA) tool! – Daniel's page". Archived from the original on 18 January 2022. Retrieved 11 July 2022.
227. ↑ Corbet, Jonathan (3 September 2021). "Not-so-anonymous virtual memory areas [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
228. ↑ Corbet, Jonathan (9 December 2021). "Blocking straight-line speculation — eventually [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
229. ↑ Zemczak, Łukasz (21 April 2022), "Jammy Jellyfish Release Notes", *discourse.ubuntu.com* , archived from the original on 22 April 2022, retrieved 4 April 2026
230. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . Retrieved 23 March 2022.
231. ↑ Torvalds, Linus (9 January 2022). "Linux 5.16". *LKML* (Mailing list). Retrieved 22 March 2022.
232. 1 2 Kroah-Hartman, Greg (13 April 2022). "Linux 5.16.20". *LKML* (Mailing list). Retrieved 15 April 2022.
233. ↑ Corbet, Jonathan (16 August 2021). "Short subjects: Realtime, Futexes, and ntfs3 [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
234. ↑ Corbet, Jonathan (18 March 2021). "Clarifying memory management with page folios [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
235. ↑ "Advanced Matrix Extension (AMX) - x86 - WikiChip". *en.wikichip.org* . Archived from the original on 29 September 2022. Retrieved 11 July 2022.
236. ↑ Corbet, Jonathan (25 October 2021). "Replacing congestion_wait() [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
237. ↑ Torvalds, Linus (31 October 2021). "Linux 5.15". *LKML* (Mailing list). Retrieved 1 November 2021.
238. ↑ "Linux 5.15.225". *git.kernel.org* . 6 November 2026. Retrieved 18 November 2026.
239. ↑ "Re: [GIT PULL] ntfs3: bugfixes for 6.0 - Linus Torvalds". *lore.kernel.org* . Retrieved 17 March 2024.
240. ↑ Rybczyńska, Marta (28 May 2021). "Top-tier memory management [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
241. ↑ Corbet, Jonathan (26 July 2021). "Hastening process cleanup with process_mrelease() [LWN.net]". *LWN.net* . Retrieved 11 July 2022.
242. ↑ "Slackware 15 Release Announcement". *Slackware* . Retrieved 4 February 2022.
243. ↑ "Release Notes for Unbreakable Enterprise Kernel Release 7". *docs.oracle.com* . Retrieved 30 June 2022.
244. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . Retrieved 1 November 2021.
245. ↑ Torvalds, Linus (29 August 2021). "Linux 5.14". *LKML* (Mailing list). Retrieved 30 August 2021.
246. 1 2 Kroah-Hartman, Greg (21 November 2021). "Linux 5.14.21". *LKML* (Mailing list). Retrieved 21 November 2021.
247. ↑ "What's new in Red Hat Enterprise Linux 9 beta". *redhat.com* . Archived from the original on 20 April 2022. Retrieved 24 April 2022.
248. ↑ Torvalds, Linus (27 June 2021). "Linux 5.13". *LKML* (Mailing list). Retrieved 30 June 2021.
249. 1 2 Kroah-Hartman, Greg (18 September 2021). "Linux 5.13.19". *LKML* (Mailing list). Retrieved 19 September 2021.
250. ↑ "Linux 5.13 To Allow Zstd Compressed Modules, Zstd Update Pending With Faster Performance". *Phoronix* . Retrieved 13 March 2024.
251. ↑ ". *Phoronix* . Retrieved 3 October 2024.
252. ↑ Torvalds, Linus (25 April 2021). "Linux 5.12". *LKML* (Mailing list). Retrieved 26 April 2021.
253. 1 2 Kroah-Hartman, Greg (20 July 2021). "Linux 5.12.19". *LKML* (Mailing list). Retrieved 20 July 2021.
254. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 1 March 2021. Retrieved 6 March 2021.
255. ↑ Torvalds, Linus (1 March 2021). "Linux 5.12-rc1". *LKML* (Mailing list). Retrieved 6 March 2021.
256. ↑ Torvalds, Linus (14 February 2021). "Linux 5.11". *LKML* (Mailing list). Retrieved 15 February 2021.
257. 1 2 Kroah-Hartman, Greg (19 May 2021). "Linux 5.11.22". *LKML* (Mailing list). Retrieved 19 May 2021.
258. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 14 February 2021. Retrieved 26 February 2021.
259. ↑ Torvalds, Linus (13 December 2020). "Linux 5.10". *LKML* (Mailing list). Retrieved 15 December 2020.
260. ↑ "Linux 5.10.274". *git.kernel.org* . 6 November 2026. Retrieved 18 November 2026.
261. ↑ Larabel, Michael (26 October 2020). "Linux 5.10 Is The Next LTS Kernel". *Phoronix* . Retrieved 27 October 2020.
262. ↑ Corbet, Jonathan (15 October 2020). "The Arm64 memory tagging extension in Linux". *LWN.net* . Retrieved 11 June 2025.
263. ↑ "Bullseye kernel will be Linux 5.10 LTS". *Debian Wiki* .
264. ↑ "Civil Infrastructure Platform". *Linux Foundation Wiki* .
265. ↑ "kernel/git/stable/linux.git - Linux kernel stable tree". *Kernel.org git repositories* . 26 February 2021. Retrieved 17 December 2021.
266. ↑ Torvalds, Linus (11 October 2020). "Linux 5.9". *LKML* (Mailing list). Retrieved 20 October 2020.
267. 1 2 Kroah-Hartman, Greg (21 December 2020). "Linux 5.9.16". *LKML* (Mailing list). Retrieved 21 December 2020.
268. ↑ Torvalds, Linus (2 August 2020). "Linux 5.8". *LKML* (Mailing list). Retrieved 3 August 2020.
269. 1 2 Kroah-Hartman, Greg (1 November 2020). "Linux 5.8.18". *LKML* (Mailing list). Retrieved 2 November 2020.
270. ↑ Torvalds, Linus (31 May 2020). "Linux 5.7". *LKML* (Mailing list). Retrieved 18 June 2020.
271. 1 2 Kroah-Hartman, Greg (27 August 2020). "Linux 5.7.19". *LKML* (Mailing list). Retrieved 31 August 2020.
272. ↑ Torvalds, Linus (29 March 2020). "Linux 5.6". *LKML* (Mailing list). Retrieved 30 March 2020.
273. 1 2 Kroah-Hartman, Greg (17 June 2020). "Linux 5.6.19". *LKML* (Mailing list). Retrieved 18 June 2020.
274. 1 2 "The Best Features Of The Linux 5.6 Kernel From WireGuard To Y2038 Compatibility To USB4". *Phoronix* . Retrieved 3 October 2024.
275. ↑ Torvalds, Linus (26 January 2020). "Linux 5.5". *LKML* (Mailing list). Retrieved 10 February 2020.
276. 1 2 Kroah-Hartman, Greg (21 April 2020). "Linux 5.5.19". *LKML* (Mailing list). Retrieved 22 April 2020.
277. ↑ Torvalds, Linus (24 November 2019). "Linux 5.4". *LKML* (Mailing list). Retrieved 25 September 2019.
278. ↑ "Linux 5.4-rc2". *GitHub* . 6 October 2019. Retrieved 27 October 2019.
279. ↑ "Linux 5.4-rc5". *GitHub* . 27 October 2019. Retrieved 27 October 2019.
280. ↑ Torvalds, Linus (15 September 2019). "Linux 5.3". *LKML* (Mailing list). Retrieved 22 September 2019.
281. 1 2 Kroah-Hartman, Greg (18 December 2019). "Linux 5.3.18". *LKML* (Mailing list). Retrieved 19 December 2019.
282. ↑ Torvalds, Linus (7 July 2019). "Linux 5.2". *LKML* (Mailing list). Retrieved 8 July 2019.
283. 1 2 "Linux 5.2.21". *git.kernel.org* . 11 October 2019. Retrieved 18 November 2026.
284. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 26 May 2019. Retrieved 27 May 2019.
285. ↑ "Linux 5.2-rc2 Kernel Released As The "Golden Lions". *Phoronix* . 26 May 2019. Retrieved 27 October 2019.
286. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 7 July 2019. Retrieved 7 July 2019.
287. ↑ Torvalds, Linus (5 March 2019). "Linux 5.1". *LKML* (Mailing list). Retrieved 6 March 2019.
288. 1 2 Kroah-Hartman, Greg (28 July 2019). "Linux 5.1.21". *LKML* (Mailing list). Retrieved 5 August 2019.
289. ↑ Corbet, Jonathan (24 January 2020). "The rapid growth of io_uring [LWN.net]". *LWN.net* . Retrieved 18 October 2022.
290. ↑ Torvalds, Linus (3 March 2019). "Linux 5.0". *LKML* (Mailing list). Retrieved 4 March 2019.
291. 1 2 Kroah-Hartman, Greg (4 June 2019). "Linux 5.0.21". *LKML* (Mailing list). Retrieved 5 June 2019.
292. ↑ Torvalds, Linus (23 December 2018). "Linux 4.20 released." *LKML* (Mailing list). Retrieved 24 December 2018.
293. 1 2 Kroah-Hartman, Greg (19 March 2019). "Linux 4.20.17". *LKML* (Mailing list). Retrieved 19 March 2019.
294. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 25 November 2018. Retrieved 26 November 2018.
295. ↑ Kroah-Hartman, Greg (22 October 2018). "Linux 4.19". *LKML* (Mailing list). Retrieved 22 October 2018.
296. 1 2 "Tags of CIP Linux". *Kernel.org git repositories* . Retrieved 19 October 2025.
297. ↑ Nestor, Marius (5 September 2018). "It's Official: Linux Kernel 4.19 Will Be the Next LTS (Long-Term Support) Series". *Softpedia* . Retrieved 6 September 2018.
298. ↑ "Buster kernel will be Linux 4.19 LTS". *Debian Wiki* .
299. ↑ Meanor, Maemalynn (25 February 2019). "Civil Infrastructure Platform Announces New Super Long Term Support Kernel that Advances Automation, Machine Learning and Artificial Intelligence". The Linux Foundation. Archived from the original on 13 August 2020. Retrieved 13 November 2019.
300. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 22 October 2018. Retrieved 22 October 2018.
301. ↑ Torvalds, Linus (12 August 2018). "Linux 4.18". *LKML* (Mailing list). Retrieved 13 August 2018.
302. 1 2 Kroah-Hartman, Greg (21 November 2018). "Linux 4.18.20". *LKML* (Mailing list). Retrieved 21 November 2018.
303. ↑ Torvalds, Linus (3 June 2018). "Linux 4.17". *LKML* (Mailing list). Retrieved 6 June 2018.
304. 1 2 Kroah-Hartman, Greg (24 August 2018). "Linux 4.17.19". *LKML* (Mailing list). Retrieved 27 August 2018.
305. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 6 May 2018. Retrieved 7 May 2018.
306. ↑ Torvalds, Linus (1 April 2018). "Linux 4.16". *LKML* (Mailing list). Retrieved 3 April 2018.
307. 1 2 Kroah-Hartman, Greg (25 June 2018). "Linux 4.16.18". *LKML* (Mailing list). Retrieved 27 June 2018.
308. ↑ Torvalds, Linus (28 January 2018). "Linux 4.15". *LKML* (Mailing list). Retrieved 29 January 2018.
309. 1 2 Kroah-Hartman, Greg (19 April 2018). "Linux 4.15.18". *LKML* (Mailing list). Retrieved 24 April 2018.
310. ↑ Torvalds, Linus (12 November 2017). "Linux 4.14". *LKML* (Mailing list). Retrieved 22 November 2017.
311. 1 2 Kroah-Hartman, Greg (10 January 2024). "Linux 4.14.336". *LKML* (Mailing list). Retrieved 10 January 2024.
312. ↑ "Zstd Compression For Btrfs & Squashfs Set For Linux 4.14, Already Used Within Facebook". *Phoronix* . Retrieved 25 April 2024.
313. ↑ "kernel/git/stable/linux.git - Linux kernel stable tree". *Kernel.org git repositories* . 21 November 2017. Retrieved 17 December 2021.
314. ↑ Torvalds, Linus (3 September 2017). "Linux 4.13". *LKML* (Mailing list). Retrieved 4 September 2017.
315. 1 2 Kroah-Hartman, Greg (24 November 2017). "Linux 4.13.16". *LKML* (Mailing list). Retrieved 27 November 2017.
316. ↑ Torvalds, Linus (2 July 2017). "Linux 4.12". *LKML* (Mailing list). Retrieved 3 July 2017.
317. 1 2 Kroah-Hartman, Greg (20 September 2017). "Linux 4.12.14". *LKML* (Mailing list). Retrieved 20 September 2017.
318. 1 2 3 "Linux_4.12 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
319. ↑ Linus, Torvalds (30 April 2017). "Linux 4.11". *LKML* (Mailing list). Retrieved 2 May 2017.
320. 1 2 Kroah-Hartman, Greg (21 July 2017). "Linux 4.11.12". *LKML* (Mailing list). Retrieved 21 July 2017.
321. ↑ Linus, Torvalds (19 February 2017). "Linux 4.10". *LKML* (Mailing list). Retrieved 20 February 2017.
322. 1 2 Kroah-Hartman, Greg (20 May 2017). "Linux 4.10.17". *LKML* (Mailing list). Retrieved 22 May 2017.
323. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 22 January 2017. Retrieved 24 January 2017.
324. ↑ Corbet, Jonathan (1 February 2017). "Kernel release status". *LWN.net* . Retrieved 2 February 2017.
325. ↑ Linus, Torvalds (11 December 2016). "Linux 4.9". *LKML* (Mailing list). Retrieved 11 December 2016.
326. ↑ Kroah-Hartman, Greg (19 January 2017). "[PATCH] 4.9 is a longterm kernel". *LKML* (Mailing list). Retrieved 23 January 2017.
327. ↑ "Stretch kernel will be Linux 4.9 LTS". *Debian Wiki* .
328. ↑ Corbet, Jonathan (11 December 2016). "The 4.9 kernel has been released". *LWN.net* . Retrieved 12 December 2016.
329. ↑ "Linus Torvalds". *Google+* . Archived from the original on 21 December 2016. Retrieved 12 December 2016.
330. ↑ Linus, Torvalds (2 October 2016). "Linux 4.8". *LKML* (Mailing list). Retrieved 3 October 2016.
331. 1 2 Kroah-Hartman, Greg (9 January 2017). "Linux 4.8.17". *LKML* (Mailing list). Retrieved 11 January 2017.
332. ↑ Linus, Torvalds (24 July 2016). "Linux 4.7". *LKML* (Mailing list). Retrieved 25 July 2016.
333. 1 2 Kroah-Hartman, Greg (22 October 2016). "Linux 4.7.10". *LKML* (Mailing list). Retrieved 24 October 2016.
334. ↑ "CPUFreq's New Scaling Governor Is Coming For Linux 4.7". *Phoronix* . Retrieved 21 June 2024.
335. ↑ "EFI Bootloader Control Driver, Core EFI Capsule Ready For Linux 4.7". *Phoronix* . Retrieved 5 October 2024.
336. ↑ "Async Discard Support Comes For Linux 4.7". *Phoronix* . Retrieved 5 October 2024.
337. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . 29 May 2016. Retrieved 30 May 2015.
338. ↑ Torvalds, Linus (15 May 2016). "Linux 4.6 is out". *LKML* (Mailing list). Retrieved 17 May 2016.
339. 1 2 Kroah-Hartman, Greg (16 August 2016). "Linux 4.6.7". *LKML* (Mailing list). Retrieved 22 August 2016.
340. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . 1 May 2016. Retrieved 2 May 2015.
341. ↑ Torvalds, Linus (13 March 2016). "Linux 4.5". *LKML* (Mailing list). Retrieved 14 March 2016.
342. ↑ Kroah-Hartman, Greg (7 June 2016). "Linux 4.5.7". *LKML* (Mailing list). Retrieved 8 June 2016.
343. ↑ Nestor, Marius (9 June 2016). "Linux Kernel 4.5.7 Is The Last In The Series, Users Urged To Move To Linux 4.6". *Softpedia* . SoftNews NET SRL. Retrieved 28 June 2016.
344. ↑ Torvalds, Linus (10 January 2016). "Linux 4.4". *LKML* (Mailing list). Retrieved 10 June 2016.
345. 1 2 Kroah-Hartman, Greg (3 February 2022). "Linux 4.4.302". *LKML* (Mailing list). Retrieved 25 May 2022.
346. ↑ "Slackware 14.2 announcement". Retrieved 20 February 2021.
347. ↑ "Ubuntu kernel lifecycle and enablement stack". *Ubuntu.com* .
348. ↑ Torvalds, Linus (1 November 2015). "Linux 4.3 is out". *LKML* (Mailing list). Retrieved 2 November 2015.
349. ↑ Kroah-Hartman, Greg (19 February 2016). "Linux 4.3.6". *LKML* (Mailing list). Retrieved 21 February 2016.
350. ↑ Nestor, Marius (20 February 2016). "Linux Kernel 4.3.6 Is the Last in the Series, Users Urged to Move to Linux 4.4". *Softpedia* . SoftNews NET SRL. Retrieved 21 February 2016.
351. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 11 October 2015. Retrieved 6 December 2015.
352. ↑ "Linus Torvalds - Google+ - Pictures from the latest dive trip with Linus Torvalds and Miika Turkia". *plus.google.com* . 3 October 2015. Retrieved 11 March 2019.<sup>[*permanent dead link*]</sup>
353. ↑ Torvalds, Linus (30 August 2015). "Linux 4.2 is out". *LKML* (Mailing list). Retrieved 2 November 2015.
354. 1 2 Kroah-Hartman, Greg (15 December 2015). "Linux 4.2.8". *LKML* (Mailing list). Retrieved 15 December 2015.
355. 1 2 3 4 5 6 7 8 9 "Ubuntu Kernel Team Extended Support". *Ubuntu wiki* . Canonical. Archived from the original on 7 October 2013. Retrieved 30 August 2013.
356. ↑ Mostafa, Kamal (15 December 2015). "Linux 4.2.y.z extended stable support". *LKML* (Mailing list). Retrieved 15 December 2015.
357. ↑ Torvalds, Linus (22 June 2015). "Linux 4.1". *LKML* (Mailing list). Retrieved 22 June 2015.
358. 1 2 "Linux kernel 4.1.52 released". *LKML* (Mailing list). 29 May 2018. Retrieved 29 May 2018.
359. ↑ Kroah-Hartman, Greg (31 January 2016). "4.1 is now maintained by Sasha Levin". *LKML* (Mailing list). Retrieved 22 February 2016.
360. ↑ Kroah-Hartman, Greg (23 September 2015). "4.1 is a longterm kernel". *LKML* (Mailing list). Retrieved 27 September 2015.
361. ↑ "Greg KH: Re: Linux 4.1.1". LKML. 29 June 2015. Retrieved 6 December 2015.
362. ↑ Torvalds, Linus (12 April 2015). "Linux 4.0 released". *LKML* (Mailing list). Retrieved 12 April 2015.
363. ↑ Kroah-Hartman, Greg (21 July 2015). "Linux 4.0.9". *LKML* (Mailing list). Retrieved 22 July 2015.
364. ↑ Nestor, Marius (13 July 2015). "Linux Kernel 4.0 to Reach End of Life Soon, Users Urged to Move to Linux Kernel 4.1". *Softpedia* . SoftNews NET SRL. Retrieved 30 September 2015.
365. 1 2 "Linux_4.0 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
366. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . 23 February 2015. Retrieved 6 December 2015.
367. 1 2 Torvalds, Linus (21 July 2011). "Linux 3.0 release". Linux kernel mailing list. Archived from the original on 18 October 2019. Retrieved 16 May 2013.
368. ↑ Torvalds, Linus (8 February 2015). "Linux 3.19 - and merge window now open". *LKML* (Mailing list). Retrieved 10 February 2015.
369. 1 2 Kroah-Hartman, Greg (11 May 2015). "Linux 3.19.8". *LKML* (Mailing list). Retrieved 11 May 2015.
370. ↑ Mostafa, Kamal (13 May 2015). "Linux 3.19.y-ckt extended stable support". *LKML* (Mailing list). Retrieved 20 January 2016.
371. ↑ Torvalds, Linus (7 December 2014). "Linux 3.18". *LKML* (Mailing list). Retrieved 8 December 2014.
372. 1 2 Kroah-Hartman, Greg (16 May 2019). "Linux 3.18.140". *LKML* (Mailing list). Retrieved 20 May 2019.
373. ↑ Kroah-Hartman, Greg (8 February 2017). "Linux 3.18.48". *LKML* (Mailing list). Retrieved 18 April 2017.
374. ↑ Kroah-Hartman, Greg (11 March 2015). "3.18 is now maintained by Sasha Levin". *LKML* (Mailing list). Retrieved 11 March 2015.
375. 1 2 "Linux_3.18 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 19 July 2024.
376. ↑ "Linux kernel 3.18, Section 1.3. bpf() syscall for eBFP virtual machine programs". *KernelNewbies* . 7 December 2014. Retrieved 6 September 2019.
377. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . 2 November 2014. Retrieved 6 December 2015.
378. ↑ Torvalds, Linus (5 October 2014). "Linux 3.17". *LKML* (Mailing list). Retrieved 6 October 2014.
379. 1 2 Kroah-Hartman, Greg (8 January 2015). "Linux 3.17.8". *LKML* (Mailing list). Retrieved 9 January 2015.
380. ↑ Torvalds, Linus (3 August 2014). "Linux 3.16". *Linux kernel mailing list* (Mailing list). Retrieved 5 August 2014.
381. 1 2 Hutchings, Ben (11 June 2020). "Linux 3.16.85". *LKML* (Mailing list). Retrieved 18 June 2020.
382. ↑ Hutchings, Ben (2 May 2016). "Add 3.16 as a longterm release". *LKML* (Mailing list). Retrieved 9 May 2016.
383. ↑ Kroah-Hartman, Greg (30 October 2014). "Linux 3.16.7". *LKML* (Mailing list). Retrieved 30 October 2014.
384. ↑ Guerrero Lopez, Ana. "Jessie will ship Linux 3.16". *Debian Project* . Bits from Debian. Retrieved 27 May 2016.
385. ↑ Henriques, Luis (30 October 2014). "Linux 3.16.y.z extended stable support". *LKML* (Mailing list). Retrieved 3 November 2014.
386. ↑ "kernel/git/stable/linux.git - Linux kernel stable tree". *Kernel.org git repositories* . 14 August 2014. Retrieved 17 December 2021.
387. ↑ Torvalds, Linus (9 July 2014). "Linux 3.15". *LKML* (Mailing list). Retrieved 8 June 2014.
388. 1 2 Kroah-Hartman, Greg (14 August 2014). "Linux 3.15.10". *LKML* (Mailing list). Retrieved 14 August 2014.
389. ↑ "Linux kernel 3.15, Section 1.7. zram: LZ4 compression support, improved performance". *KernelNewbies* . 8 June 2014. Retrieved 15 June 2014.
390. ↑ Torvalds, Linus (30 March 2014). "Linux 3.14". *LKML* (Mailing list). Retrieved 30 March 2014.
391. 1 2 Kroah-Hartman, Greg (11 September 2016). "Linux 3.14.79". *LKML* (Mailing list). Retrieved 12 September 2016.
392. ↑ "Linux kernel 3.14, Section 1.2. zram: Memory compression mechanism considered stable". *KernelNewbies* . 30 March 2014. Retrieved 1 April 2014.
393. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . 3 February 2014. Retrieved 6 December 2015.
394. ↑ Torvalds, Linus (19 January 2014). "Linux 3.13". *LKML* (Mailing list). Retrieved 20 January 2014.
395. 1 2 Kroah-Hartman, Greg (23 April 2014). "Linux 3.13.11". *LKML* (Mailing list). Retrieved 23 April 2014.
396. ↑ "btrfs: update kconfig help text - kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . Retrieved 6 September 2026.
397. ↑ Mostafa, Kamal (24 April 2014). "[ANNOUNCE] Linux 3.13.y.z extended stable support". *LKML* (Mailing list). Retrieved 15 February 2015.
398. ↑ "Linux 3.12-rc1". kernel.org. Retrieved 16 September 2013.
399. ↑ "Frog Photobombs NASA Moon Probe Launch". space.com. 12 September 2013. Retrieved 19 September 2013.
400. ↑ Torvalds, Linus (3 November 2013). "Linux 3.12 released .. and no merge window yet .. and 4.0 plans?". *LKML* (Mailing list). Retrieved 3 November 2013.
401. 1 2 Slabý, Jiří (10 May 2017). "Linux 3.12.74". *LKML* (Mailing list). Retrieved 10 May 2017.
402. 1 2 Kroah-Hartman, Greg (26 February 2014). "3.12-stable kernel tree being taken over by Jiří Slabý". *LKML* (Mailing list). Retrieved 3 February 2015.
403. ↑ "Linux 3.12 Codenamed "Suicidal Squirrel". Phoronix. Retrieved 12 September 2013.
404. ↑ Torvalds, Linus (2 September 2013). "Linux 3.11". *LKML* (Mailing list). Retrieved 3 September 2013.
405. 1 2 Kroah-Hartman, Greg (29 November 2013). "Linux 3.11.10". *LKML* (Mailing list). Retrieved 6 December 2013.
406. ↑ "Linux kernel 3.11, Section 9. Zswap: A compressed swap cache". *KernelNewbies* . 2 September 2013. Retrieved 22 January 2014.
407. ↑ Vaughan, Steven J. (16 July 2013). "Linux 3.11: Linux for Workgroups". ZDNet. Retrieved 12 September 2013.
408. ↑ Torvalds, Linus (30 June 2013). "Linux 3.10". *LKML* (Mailing list). Retrieved 1 July 2013.
409. 1 2 Tarreau, Willy (5 November 2017). "Linux 3.10.108 (EOL)". *LKML* (Mailing list). Retrieved 5 November 2017.
410. ↑ Tarreau, Willy (13 April 2016). "[PATCH kernel.org] change 3.10 EOL and maintainer". *LKML* (Mailing list). Retrieved 14 June 2016.
411. ↑ "Linux kernel 3.10, Section 1.2. bcache, a block layer cache for SSD caching". *KernelNewbies* . 30 June 2013. Retrieved 4 October 2013.
412. ↑ "Baby Fish - Taipei Open Source Software User Group". tossug.org. 13 August 2013. Retrieved 12 September 2013.
413. ↑ "Greg KH: Re: Linux 3.10.9". LKML. Retrieved 12 September 2013.
414. ↑ "kernel/git/stable/linux-stable.git - Linux kernel stable tree". git.kernel.org. 12 August 2013. Retrieved 12 September 2013.
415. ↑ "Slackware 14.1 announcement". Retrieved 20 February 2021.
416. ↑ Torvalds, Linus (28 April 2013). "Linux 3.9 released". *LKML* (Mailing list). Retrieved 12 May 2013.
417. 1 2 Kroah-Hartman, Greg (21 July 2013). "Linux 3.9.11". *LKML* (Mailing list). Retrieved 22 July 2013.
418. ↑ "Linux kernel 3.9, Section 1.3. SSD cache devices". *KernelNewbies* . 28 April 2013. Retrieved 7 October 2013.
419. ↑ "Linux_3.9 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 22 June 2024.
420. ↑ "Linux_3.9_DriverArch - Linux Kernel Newbies". *KernelNewbies* . Retrieved 22 June 2024.
421. ↑ jake (13 June 2013). "Stable kernels 3.9.6, 3.4.49, and 3.0.82". LWN.net. Retrieved 12 September 2013.
422. ↑ Torvalds, Linus (18 February 2013). "Linux 3.8". *LKML* (Mailing list). Retrieved 12 May 2013.
423. 1 2 Kroah-Hartman, Greg (11 May 2013). "Linux 3.8.13". *LKML* (Mailing list). Retrieved 12 May 2013.
424. ↑ "F2FS File-System Merged Into Linux 3.8 Kernel". *Phoronix* . Retrieved 25 April 2024.
425. ↑ "Linux Kernel Drops Support For Old Intel 386 CPUs". *Phoronix* . Retrieved 25 April 2024.
426. ↑ Walker-Morgan, DJ (15 May 2013). "Canonical to maintain Linux 3.8 until August 2014". *The H Open* . Heinz Heise. Archived from the original on 15 May 2013. Retrieved 17 May 2013.
427. ↑ Leemhuis, Thorsten (19 February 2013). "What's new in Linux 3.8 - The H Open: News and Features". h-online.com. Archived from the original on 8 March 2013. Retrieved 12 September 2013.
428. ↑ "Linus Torvalds - Google+ - My meds must be kicking in.. I think I just saw somebody..." Google+. Archived from the original on 11 June 2013. Retrieved 12 September 2013.
429. ↑ "Linux 3.8.5". permalink.gmane.org. 28 March 2013. Archived from the original on 12 September 2015. Retrieved 12 September 2013.
430. ↑ Torvalds, Linus (10 December 2012). "Linux 3.7". *LKML* (Mailing list). Retrieved 14 May 2013.
431. 1 2 Kroah-Hartman, Greg (27 February 2013). "Linux 3.7.10". *LKML* (Mailing list). Retrieved 14 May 2013.
432. ↑ Stahie, Silviu (1 March 2013). "Linux Kernel 3.7.10 Officially Reaches End of Life, Kernel.org Website Updated". Softpedia. Retrieved 1 March 2013.
433. ↑ "ARM64/AArch64 Support Going Into Linux 3.7 Kernel". *Phoronix* . Retrieved 25 April 2024.
434. ↑ Thorsten Leemhuis. "Die Neuerungen von Linux 3.6 | heise open". heise.de. Retrieved 12 September 2013.
435. ↑ "Linus Torvalds - Google+ - Minky has gone from catching shrews to catching (and..." Google+. Archived from the original on 26 September 2014. Retrieved 12 September 2013.
436. ↑ Torvalds, Linus (30 September 2012). "Linux 3.6". *LKML* (Mailing list). Retrieved 14 May 2013.
437. 1 2 Kroah-Hartman, Greg (17 December 2012). "Linux 3.6.11". *LKML* (Mailing list). Retrieved 18 December 2012.
438. ↑ "Btrfs Filesystem In Linux 3.6 Kernel Has Big Changes". *Phoronix* . Retrieved 25 April 2024.
439. ↑ Torvalds, Linus (21 July 2012). "Linux 3.5 released". *LKML* (Mailing list). Retrieved 14 May 2013.
440. 1 2 Kroah-Hartman, Greg (12 October 2013). "Linux 3.5.7". *LKML* (Mailing list). Retrieved 14 May 2013.
441. ↑ Krzesinski, Herton Ronaldo (9 November 2012). "Linux 3.6.11". *LKML* (Mailing list). Retrieved 14 May 2013.
442. ↑ Torvalds, Linus (20 May 2012). "Linux 3.4 released". *LKML* (Mailing list). Retrieved 14 May 2013.
443. ↑ Larabel, Michael (21 May 2012). "Linux 3.4 Kernel Released With Many New Features". Phoronix. Retrieved 1 July 2013.
444. ↑ "Linux kernel 3.4.113 released". *Linux stable* (Mailing list). 26 October 2016. Retrieved 5 December 2016.
445. ↑ Kroah-Hartman, Greg (26 August 2014). "Li Zefan is now the 3.4 stable maintainer". *LKML* (Mailing list). Retrieved 14 October 2014.
446. ↑ "[Kernel 3.4.113][CAF][Monthly] Lambda Kernel ~ Infinito [All variants][N/M/L][01/29]". 13 December 2015. The 3.4.113 version of the Linux kernel has been finished in Oct 26, 2016 and it's currently marked as EOL (End of Life) version.
447. ↑ Torvalds, Linus (18 March 2012). "Linux 3.3 release". *LKML* (Mailing list). Retrieved 14 May 2013.
448. 1 2 Kroah-Hartman, Greg (4 June 2012). "Linux 3.3.8". *LKML* (Mailing list). Retrieved 14 May 2013.
449. ↑ Torvalds, Linus (4 January 2012). "Linux 3.2". *LKML* (Mailing list). Retrieved 14 May 2013.
450. ↑ Hutchings, Ben (1 June 2018). "Linux 3.2.102". *LKML* (Mailing list). Retrieved 6 June 2018.
451. 1 2 von Eitzen, Chris (23 April 2012). "Long-term maintenance for Linux 3.2". *The H Open* . Heinz Heise. Archived from the original on 27 July 2013. Retrieved 14 May 2013.
452. ↑ "Active kernel releases". *www.kernel.org* . Retrieved 6 April 2018.
453. ↑ "LTS Enablement Stacks". Retrieved 10 February 2014.
454. ↑ "SecurityTeam/ESM/12.04". *Ubuntu Wiki* . Archived from the original on 25 October 2017. Retrieved 24 October 2017.
455. ↑ "Greg KH: Re: Linux 3.4.53". LKML. 13 July 2013. Retrieved 12 September 2013.
456. ↑ Torvalds, Linus (24 October 2011). "Linux 3.1". *LKML* (Mailing list). Retrieved 16 May 2013.
457. 1 2 Kroah-Hartman, Greg (18 January 2012). "Linux 3.1.10". *LKML* (Mailing list). Retrieved 16 May 2013.
458. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". git.kernel.org. 22 August 2011. Retrieved 12 September 2013.
459. 1 2 Kroah-Hartman, Greg (22 October 2013). "Linux 3.0.101". *LKML* (Mailing list). Retrieved 6 November 2013.
460. 1 2 Leemhuis, Thorsten (12 January 2012). "Kernel Log: 15,000,000 lines, 3.0 promoted to long-term kernel". *The H Open* . Heinz Heise. Archived from the original on 23 March 2012. Retrieved 27 March 2012.
461. ↑ "Linux_3.0 - Linux Kernel Newbies, Section 1.1. Btrfs: Automatic defragmentation, scrubbing, performance improvements". *KernelNewbies* . Retrieved 16 March 2024.
462. ↑ "Linux 3.0 all about 'steady plodding progress'. *The Register* . 31 May 2011. Retrieved 6 December 2015.
463. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . 30 May 2011. Retrieved 6 December 2015.
464. ↑ Bunk, Adrian (11 October 2008). "Linux 2.6.27 will be a longtime supported kernel". *LKML* (Mailing list). Retrieved 12 January 2016.
465. ↑ Larabel, Michael (26 October 2011). "Linux Foundation Backs Long-Term Support Kernels". *Phoronix* . Phoronix Media. Retrieved 12 January 2016.
466. ↑ "What is LTSI?". *linuxfoundation.org* . The Linux Foundation. Archived from the original on 5 October 2014. Retrieved 12 January 2016.
467. ↑ Torvalds, Linus (18 May 2011). "Linux 2.6.39". *LKML* (Mailing list). Retrieved 17 May 2013.
468. 1 2 Kroah-Hartman, Greg (3 August 2011). "Linux 2.6.39.4". *LKML* (Mailing list). Retrieved 17 May 2013.
469. ↑ Torvalds, Linus (14 March 2011). "Linux 2.6.38". *LKML* (Mailing list). Retrieved 15 February 2015.
470. 1 2 Kroah-Hartman, Greg (2 June 2011). "Linux 2.6.38.8". *LKML* (Mailing list). Retrieved 15 February 2015.
471. ↑ Leemhuis, Thorsten (21 October 2010). "What's new in Linux 2.6.36 - The H Open Source: News and Features". Archived from the original on 24 October 2010. Retrieved 6 December 2015.
472. ↑ Torvalds, Linus (4 January 2011). "Linux 2.6.37". *LKML* (Mailing list). Retrieved 15 February 2015.
473. 1 2 Kroah-Hartman, Greg (27 March 2011). "Linux 2.6.37.6". *LKML* (Mailing list). Retrieved 15 February 2015.
474. ↑ Torvalds, Linus (20 October 2010). "Linux 2.6.36". *LKML* (Mailing list). Retrieved 15 February 2015.
475. 1 2 Kroah-Hartman, Greg (17 February 2011). "Linux 2.6.36.4". *LKML* (Mailing list). Retrieved 15 February 2015.
476. ↑ "Linux_2_6_36 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 3 October 2024.
477. ↑ Torvalds, Linus (1 August 2010). "Linux 2.6.35". *LKML* (Mailing list). Retrieved 17 May 2013.
478. ↑ Kleen, Andi (1 August 2011). "[ANNOUNCE] The longterm Linux 2.6.35.14 kernel is released". *LKML* (Mailing list). Retrieved 17 May 2013.
479. 1 2 Kleen, Andi (3 December 2010). "Plans for 2.6.35-longterm was Re: Linux stable kernel release procedure changes". *LKML* (Mailing list). Retrieved 17 May 2013.
480. ↑ Torvalds, Linus (16 May 2010). "Linux 2.6.34". *LKML* (Mailing list). Retrieved 17 May 2013.
481. 1 2 Gortmaker, Paul (11 February 2014). "Linux 2.6.34.15". *LKML* (Mailing list). Retrieved 12 February 2014.
482. 1 2 Gortmaker, Paul (3 December 2010). "Announcement: Plans for v2.6.34-longterm". *LKML* (Mailing list). Retrieved 17 May 2013.
483. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . 19 April 2010. Retrieved 6 December 2015.
484. ↑ "Linux-Kernel Archive: Sheep on Meth is out with wings (?) - compat-wireless for linux-2.6.35-rc1". *lkml.indiana.edu* . Retrieved 6 December 2015.
485. ↑ Torvalds, Linus (24 February 2010). "Linux 2.6.33 released". *LKML* (Mailing list). Retrieved 17 May 2013.
486. 1 2 Kroah-Hartman, Greg (7 November 2011). "Linux 2.6.33.20". *LKML* (Mailing list). Archived from the original on 26 January 2016. Retrieved 17 May 2013.
487. 1 2 Leemhuis, Thorsten (23 March 2011). "Kernel Log: Development of 2.6.39 under way, series 33 revived". *The H Open* . Heinz Heise. Archived from the original on 27 June 2013. Retrieved 17 May 2013.
488. ↑ "Linux_2_6_33 - Linux Kernel Newbies - 1.1. Nouveau, a driver for Nvidia graphic cards". *KernelNewbies* . Retrieved 22 March 2024.
489. ↑ Torvalds, Linus (2 December 2009). "Linux 2.6.32". *LKML* (Mailing list). Retrieved 17 May 2013.
490. ↑ Tarreau, Willy (12 March 2016). "Linux 2.6.32.71 (EOL)". *LKML* (Mailing list). Retrieved 23 June 2017.
491. ↑ Tarreau, Willy (5 March 2012). "Re: Linux 2.6.32.58". *LKML* (Mailing list). Retrieved 12 February 2014.
492. 1 2 Kroah-Hartman, Greg (8 March 2012). "The 2.6.32 Linux kernel". Retrieved 1 June 2013.
493. ↑ Walker-Morgan, Dj (5 March 2012). "Maintenance of Linux kernel 2.6.32 is slowing down". *The H Open* . Heinz Heise. Archived from the original on 8 December 2013. Retrieved 17 May 2013.
494. ↑ "Anatomy of Linux Kernel Shared Memory". *IBM Developer* . IBM. Archived from the original on 9 July 2016. Retrieved 21 August 2010.
495. ↑ "Package: linux-image-2.6-686 (2.6.32+29)". Debian.org. Retrieved 9 July 2014.
496. ↑ Torvalds, Linus (9 September 2009). "Linux 2.6.31". *LKML* (Mailing list). Retrieved 15 February 2015.
497. 1 2 Kroah-Hartman, Greg (5 July 2010). "Linux 2.6.31.14". *LKML* (Mailing list). Retrieved 15 February 2015.
498. ↑ "Linux_2_6_31 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 23 March 2024.
499. ↑ Torvalds, Linus (9 June 2009). "Linux 2.6.30". *LKML* (Mailing list). Retrieved 15 February 2015.
500. 1 2 "Linux 2.6.30.10". *git.kernel.org* . 3 December 2009. Retrieved 18 November 2026.
501. ↑ "Linux_2_6_30 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 3 October 2024.
502. ↑ "Kernel development". *LWN.net* . Retrieved 6 December 2015.
503. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . 16 May 2009. Retrieved 6 December 2015.
504. ↑ "Blubber-wrapped Linux kernel 2.6.30 hits the decks". *The Register* . 11 June 2009. Retrieved 6 December 2015.
505. ↑ Torvalds, Linus (23 March 2009). "Linux 2.6.29". *LKML* (Mailing list). Retrieved 15 February 2015.
506. 1 2 Kroah-Hartman, Greg (2 July 2009). "Linux 2.6.29.6". *LKML* (Mailing list). Retrieved 15 February 2015.
507. ↑ Wuelfing, Britta (12 January 2009). "Kernel 2.6.29: Corbet Says Btrfs Next Generation Filesystem". *Linux Magazine* . Retrieved 5 November 2011.
508. ↑ "Kernel Log: Tasmanian devil to be Linux's temporary mascot, new Radeon drivers - The H Open: News and Features". 19 March 2009. Archived from the original on 8 December 2013. Retrieved 6 December 2015.
509. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". *git.kernel.org* . Retrieved 6 December 2015.
510. ↑ Torvalds, Linus (24 December 2008). "Happy v2.6.28". *LKML* (Mailing list). Retrieved 15 February 2015.
511. 1 2 Kroah-Hartman, Greg (2 May 2009). "Linux 2.6.28.10". *LKML* (Mailing list). Retrieved 15 February 2015.
512. ↑ "ext4: Rename ext4dev to ext4 - kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . 10 October 2008. Retrieved 18 November 2026.
513. ↑ "Killer bat of doom". LWN.net. Retrieved 12 September 2013.
514. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". git.kernel.org. Retrieved 12 September 2013.
515. ↑ "LXR linux/Makefile". lxr.linux.no. Retrieved 12 September 2013.
516. ↑ Torvalds, Linus (9 October 2008). "Linux 2.6.27". *LKML* (Mailing list). Retrieved 21 February 2015.
517. ↑ Tarreau, Willy (17 March 2012). "Linux 2.6.27.62". *LKML* (Mailing list). Retrieved 12 February 2014.
518. ↑ Kroah-Hartman, Greg (12 December 2010). "Willy Tarreau is taking over the 2.6.27-longterm kernel release". *LKML* (Mailing list). Retrieved 12 February 2014.
519. 1 2 Bunk, Adrian (11 October 2008). "Linux 2.6.27 will be a longtime supported kernel". *LKML* (Mailing list). Retrieved 21 February 2015.
520. ↑ "Commit in mer-meego-kernel in Mer-meego". Gitorious. Archived from the original on 3 June 2015. Retrieved 12 September 2013.
521. ↑ Torvalds, Linus (13 July 2008). "Linux 2.6.26". *LKML* (Mailing list). Retrieved 21 February 2015.
522. 1 2 Kroah-Hartman, Greg (10 November 2008). "Linux 2.6.26.8". *LKML* (Mailing list). Retrieved 21 February 2015.
523. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". git.kernel.org. 12 June 2008. Retrieved 12 September 2013.
524. ↑ Torvalds, Linus (16 April 2008). "Linux 2.6.25". *LKML* (Mailing list). Retrieved 21 February 2015.
525. 1 2 Kroah-Hartman, Greg (10 November 2008). "Linux 2.6.25.20". *LKML* (Mailing list). Retrieved 21 February 2015.
526. ↑ Corbet, Jonathan (6 February 2008). "More stuff for 2.6.25 [LWN.net]". *LWN.net* . Retrieved 3 October 2024.
527. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". git.kernel.org. 15 February 2008. Retrieved 12 September 2013.
528. ↑ Torvalds, Linus (24 January 2008). "Linux 2.6.24". *LKML* (Mailing list). Retrieved 21 February 2015.
529. 1 2 Kroah-Hartman, Greg (6 May 2008). "Linux 2.6.24.7". *LKML* (Mailing list). Retrieved 21 February 2015.
530. ↑ Jonathan Corbet (29 October 2007). "Notes from a container". LWN.net. Retrieved 14 April 2015. The original 'containers' name was considered to be too generic – this code is an important part of a container solution, but it's far from the whole thing. So containers have now been renamed 'control groups' (or 'cgroups') and merged for 2.6.24.
531. ↑ corbet (29 August 2007). "Kernel release status". LWN.net. Retrieved 12 September 2013.
532. ↑ "git » linux-kernel » commit 3146b39". blitiri.com.ar. 2 October 2007. Retrieved 12 September 2013.
533. ↑ "Linux 2.6.24.6". permalink.gmane.org. 2 May 2008. Archived from the original on 3 June 2015. Retrieved 12 September 2013.
534. ↑ Torvalds, Linus (9 October 2007). "Linux 2.6.23". *LKML* (Mailing list). Retrieved 22 February 2015.
535. 1 2 Kroah-Hartman, Greg (25 February 2008). "Linux 2.6.23.17". *LKML* (Mailing list). Retrieved 22 February 2015.
536. ↑ "Linux_2_6_23 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 22 March 2024.
537. ↑ Torvalds, Linus (8 July 2007). "Linux 2.6.22 released". *LKML* (Mailing list). Retrieved 22 February 2015.
538. 1 2 Kroah-Hartman, Greg (25 February 2008). "Linux 2.6.22.19". *LKML* (Mailing list). Retrieved 22 February 2015.
539. 1 2 "Linux_2_6_22 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 27 May 2024.
540. ↑ "Makefile - kernel/git/torvalds/linux.git - Linux kernel source tree". *Kernel.org git repositories* . Retrieved 18 November 2026.
541. ↑ Torvalds, Linus (25 April 2007). "Linux 2.6.21". *LKML* (Mailing list). Retrieved 22 February 2015.
542. 1 2 Kroah-Hartman, Greg (4 August 2007). "Linux 2.6.21.7". *LKML* (Mailing list). Retrieved 22 February 2015.
543. 1 2 "Linux_2_6_21 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 27 May 2024.
544. ↑ "Linux 2.6.22-rc3". git.kernel.org. 25 May 2007. Retrieved 2 July 2018.
545. ↑ Torvalds, Linus (4 February 2007). "Super Kernel Sunday!". *LKML* (Mailing list). Retrieved 22 February 2015.
546. 1 2 Tarreau, Willy (17 October 2007). "Linux 2.6.20.21". *LKML* (Mailing list). Retrieved 22 February 2015.
547. ↑ "Linux_2_6_20 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
548. ↑ corbet (2 January 2007). "Linux 2.6.20-rc3". LWN.net. Retrieved 12 September 2013.
549. ↑ "Re: Linux 2.6.20.10". Linux.derkeiler.com. 27 April 2007. Archived from the original on 24 January 2016. Retrieved 12 September 2013.
550. ↑ Torvalds, Linus (29 November 2006). "Linux 2.6.19". *LKML* (Mailing list). Retrieved 29 September 2020.
551. 1 2 Kroah-Hartman, Greg (3 March 2007). "Linux 2.6.19.7". *LKML* (Mailing list). Retrieved 22 February 2015.
552. ↑ "Serial ATA (SATA) Linux hardware/driver status report". *linux-ata.org* . Archived from the original on 12 March 2007. Retrieved 26 January 2010.
553. ↑ "Linux_2_6_19 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
554. ↑ "Linux_2_6_19 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 18 March 2024.
555. ↑ "Shield Patch". *people.redhat.com* . Archived from the original on 11 September 2022. Retrieved 6 December 2015.
556. ↑ Torvalds, Linus (20 September 2006). "Arrr! Linux 2.6.18". *LKML* (Mailing list). Retrieved 22 February 2015.
557. 1 2 Kroah-Hartman, Greg (23 February 2007). "Linux 2.6.18.8". *LKML* (Mailing list). Retrieved 22 February 2015.
558. ↑ "Linux_2_6_18 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
559. ↑ Torvalds, Linus (17 June 2006). "Linux v2.6.17". *LKML* (Mailing list). Retrieved 22 February 2015.
560. 1 2 Kroah-Hartman, Greg (16 October 2006). "Linux 2.6.17.14". *LKML* (Mailing list). Retrieved 22 February 2015.
561. ↑ "Linux: Explaining splice() and tee()". kerneltrap.org. 21 April 2006. Archived from the original on 21 May 2013. Retrieved 27 April 2014.
562. ↑ "kernel/git/torvalds/linux.git - Linux kernel source tree". git.kernel.org. 25 May 2006. Retrieved 12 September 2013.
563. ↑ "Lordi Rules". blog.x-way.org. 25 May 2006. Retrieved 12 September 2013.
564. ↑ OSDir.com (19 June 2006). "Linux v2.6.17 "Crazed Snow-Weasel". ospedia.osdir.com. Archived from the original on 21 August 2016. Retrieved 12 September 2013.
565. ↑ Torvalds, Linus (20 March 2006). "Linux v2.6.16". *LKML* (Mailing list). Retrieved 22 February 2015.
566. 1 2 Bunk, Adrian (21 July 2008). "Linux 2.6.16.62". *LKML* (Mailing list). Retrieved 22 February 2015.
567. ↑ Kroah-Hartman, Greg (3 August 2006). "Adrian Bunk is now taking over the 2.6.16-stable branch". *LKML* (Mailing list). Retrieved 12 February 2014.
568. ↑ Kroah-Hartman, Greg (3 August 2006). "Adrian Bunk is now taking over the 2.6.16-stable branch". *LKML* (Mailing list). Retrieved 21 February 2015.
569. 1 2 "Linux_2_6_16 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
570. ↑ Torvalds, Linus (2 January 2006). "Linux 2.6.15". *LKML* (Mailing list). Retrieved 28 February 2015.
571. 1 2 Kroah-Hartman, Greg (28 March 2006). "Linux 2.6.15.7". *LKML* (Mailing list). Retrieved 28 February 2015.
572. ↑ "Linux_2_6_15 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
573. ↑ "Re: Linux 2.6.16.7". linux.derkeiler.com. 17 April 2006. Archived from the original on 24 January 2016. Retrieved 12 September 2013.
574. ↑ Torvalds, Linus (27 October 2005). "Linux 2.6.14". *LKML* (Mailing list). Retrieved 28 February 2015.
575. 1 2 Kroah-Hartman, Greg (31 January 2006). "Linux 2.6.14.7". *LKML* (Mailing list). Retrieved 28 February 2015.
576. ↑ "Linux_2_6_14 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
577. ↑ "Greg KH: Re: Linux 2.6.14.5". LKML. Retrieved 12 September 2013.
578. ↑ Torvalds, Linus (28 August 2005). "Linux 2.6.13". *LKML* (Mailing list). Retrieved 28 February 2015.
579. 1 2 Kroah-Hartman, Greg (15 December 2005). "Linux 2.6.13.5". *LKML* (Mailing list). Retrieved 28 February 2015.
580. ↑ Fox, Richard (26 August 2014). *Linux with Operating System Concepts* . CRC Press. p. 450. ISBN 978-1-4822-3589-0.
581. ↑ ". CIOL. 30 December 2004. Archived from the original on 3 October 2022. Retrieved 12 September 2013.
582. ↑ "Linux 2.6.10 Kernel Hits The Street". Informationweek. 29 December 2004. Retrieved 12 September 2013.<sup>[*dead link*]</sup>
583. ↑ "Linux 2.6.12". *LWN.net* . Retrieved 3 August 2025.
584. ↑ Torvalds, Linus (18 June 2005). "Linux 2.6.12". *LKML* (Mailing list). Archived from the original on 26 January 2016. Retrieved 28 February 2015.
585. 1 2 Wright, Chris (29 August 2005). "Linux 2.6.12.6". *LKML* (Mailing list). Retrieved 28 February 2015.
586. ↑ "Linux_2_6_12 - Linux Kernel Newbies". *KernelNewbies* .
587. ↑ Torvalds, Linus (2 March 2005). "Linux 2.6.11". *LKML* (Mailing list). Retrieved 28 February 2015.
588. 1 2 Wright, Chris (12 June 2005). "Linux 2.6.11.12". *LKML* (Mailing list). Retrieved 28 February 2015.
589. ↑ "Linux-Kernel Archive: Ho ho ho - Linux v2.6.10". *lkml.iu.edu* . Retrieved 24 May 2024.
590. ↑ "Linux_2_6_10 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
591. ↑ "Linux-Kernel Archive: Linux v2.6.9..." *lkml.iu.edu* . Retrieved 24 May 2024.
592. ↑ "Linux v2.6.8 [LWN.net]". *LWN.net* . Retrieved 24 May 2024.
593. ↑ "Linux 2.6.7 [LWN.net]". *LWN.net* . Retrieved 24 May 2024.
594. ↑ "Linux-Kernel Archive: Linux 2.6.6". *lkml.iu.edu* . Retrieved 24 May 2024.
595. 1 2 "Linux_2_6_6 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
596. ↑ "Linux-Kernel Archive: Linux v2.6.5". *lkml.iu.edu* . Retrieved 24 May 2024.
597. ↑ "Linux_2_6_5 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
598. ↑ "Linux 2.6.4 [LWN.net]". *LWN.net* . Retrieved 24 May 2024.
599. 1 2 3 "Linux_2_6_4 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
600. ↑ "Linux 2.6.3 [LWN.net]". *LWN.net* . Retrieved 24 May 2024.
601. ↑ "Linux_2_6_3 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 24 May 2024.
602. ↑ "Linux 2.6.2 aka "Feisty Dunnart" [LWN.net]". *LWN.net* . Retrieved 24 May 2024.
603. ↑ "Linux_2_6_2 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 23 May 2024.
604. ↑ "Linux-2.6.1 [LWN.net]". *LWN.net* . Retrieved 24 May 2024.
605. ↑ "Linux_2_6_1 - Linux Kernel Newbies". *KernelNewbies* . Retrieved 23 May 2024.
606. ↑ Torvalds, Linus (17 December 2003). "Linux 2.6.0". *LKML* (Mailing list). Retrieved 28 February 2015.
607. ↑ Torvalds, Linus (24 December 2004). "Ho ho ho - Linux v2.6.10". *LKML* (Mailing list). Retrieved 28 February 2015.
608. 1 2 "Introducing the 2.6 Kernel | Linux Journal". *www.linuxjournal.com* . Retrieved 22 March 2024.
609. ↑ "Release notes for v2.5.4". *The Linux Kernel Archives* . Linux Kernel Organization, Inc. Retrieved 3 July 2021.
610. ↑ "SELinux Security - Documentation". *docs.rockylinux.org* . Retrieved 3 October 2024.
611. ↑ "kpatchup 0.02 kernel patching script". LWN.net. Retrieved 12 September 2013.
612. ↑ "mikas blog » Blog Archive » Zonked Quokka and USB". michael-prokop.at. 14 September 2004. Retrieved 12 September 2013.
613. 1 2 3 "Active kernel releases". *www.kernel.org* . Retrieved 27 May 2024.
614. ↑ Torvalds, Linus (4 January 2001). "And oh, btw." *LKML* (Mailing list). Retrieved 8 March 2015.
615. 1 2 Tarreau, Willy (18 December 2010). "Linux 2.4.37.11". *LKML* (Mailing list). Retrieved 14 September 2011.
616. 1 2 3 "Man page of FILESYSTEMS". *sites.uclouvain.be* . Retrieved 16 March 2024.
617. ↑ Rob Radez (23 November 2001). "2.4.15-final". *Linux kernel mailing list* .
618. 1 2 Daniel Robbins (1 September 2001). "Common threads: Advanced filesystem implementor's guide, Part 3". IBM DeveloperWorks. Archived from the original on 3 February 2012. Retrieved 15 February 2013.
619. 1 2 Harald Welte, netfilter archeology: 18 years from 2.3 to 4.x, 5 December 2017
620. ↑ "Interview: Andrea Arcangeli". KernelTrap. 18 May 2004. Archived from the original on 5 December 2004. Retrieved 23 February 2024.
621. ↑ Torvalds, Linus (20 January 1999). "2.2.0-final". *LKML* (Mailing list). Retrieved 8 March 2015.
622. ↑ "LATEST-IS-2.2.26". Archived from the original on 13 July 2012.
623. ↑ McAllister, Neil (24 January 2013). "Kernel hacker Alan Cox quits Linux, Intel". *The Register* . Retrieved 23 September 2018.
624. ↑ Petersen, Marc-Christian (13 January 2005). "Linux 2.2.27-rc2". Archived from the original on 30 June 2007.
625. ↑ Petersen, Marc-Christian (25 February 2004). "Linux 2.2.26 aka "2.2 is not dead" released". *LKML* (Mailing list).
626. ↑ "Linux USB". *www.linux-usb.org* . Retrieved 27 May 2024.
627. 1 2 3 4 5 6 "The Wonderful World of Linux 2.2 LG #37". *www.linuxdoc.org* . Retrieved 27 May 2024.
628. 1 2 Jones, M. (18 September 2018) [first published on 2009-12-14]. "Inside the Linux 2.6 Completely Fair Scheduler". *developer.ibm.com* . Retrieved 7 February 2024.
629. 1 2 3 Babka, Vlastimil (12 September 2022). "The slab allocators of past, present, and future" (PDF).
630. ↑ Torvalds, Linus (9 June 1996). "Linux 2.0 really _is_ released." *LKML* (Mailing list). Retrieved 8 March 2015.
631. ↑ "LATEST-IS-2.0.40". Archived from the original on 13 July 2012.
632. ↑ Weinehall, David (8 February 2004). "[ANNOUNCE] Linux-kernel 2.0.40 aka 'The Moss-covered Tortoise'. Archived from the original on 16 November 2006.
633. ↑ "Linux Version 2.0 | Linux Journal". *www.linuxjournal.com* . Retrieved 27 May 2024.
634. ↑ Torvalds, Linus (10 May 1996). "Century Linux!". *LKML* (Mailing list). Retrieved 29 April 2016.
635. ↑ "Public Git Hosting - davej-history.git/blob - drivers/char/random.c". *repo.or.cz* . Retrieved 29 March 2024.
636. ↑ "Import 1.3.51 - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 23 May 2024.
637. ↑ "What is kerneld?". *www.linuxdoc.org* . Retrieved 23 May 2024.
638. ↑ "1. The Linux/x86 Boot Protocol — The Linux Kernel documentation". *www.kernel.org* . Retrieved 26 August 2025.
639. ↑ Rubini, Alessandro (June 1997). "Booting the Kernel". *www.linux.it* . Retrieved 26 August 2025.
640. ↑ Stephen Shankland (1 November 2000). "Test version of new Linux kernel available". CNET News. Archived from the original on 16 June 2011. Retrieved 22 November 2011.
641. ↑ "Linus Torvalds Releases Linux 1.2.0". Retrieved 5 April 2017.
642. ↑ "Original IP Firewall (2.0 Kernels)". *tldp.org* . Retrieved 29 March 2024.
643. 1 2 "Import 1.1.76 - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 23 May 2024.
644. 1 2 "Linux 0.99 patchlevel 14 - Diff - 75bb5836a8a8c0ee44ffd60a51f357b9568f1381^! - pub/scm/linux/kernel/git/nico/archive - Git at Google". *kernel.googlesource.com* . Retrieved 22 March 2024.
645. ↑ "Linux Kernel Version History: 0.99 series kernels". *www.oldlinux.org* . Archived from the original on 10 September 2022. Retrieved 11 October 2020.
646. 1 2 Jones, M. Tim (17 February 2009). "Anatomy of ext4". IBM Developer Works. Archived from the original on 20 February 2015. Retrieved 8 February 2012.
647. 1 2 3 4 "Finally: 0.98". *www.tech-insider.org* . Retrieved 29 March 2024.
648. ↑ "Linux Kernel Version History: 0.98 series kernels". *www.oldlinux.org* . Archived from the original on 10 September 2022. Retrieved 11 October 2020.
649. ↑ "[PATCH] Linux-0.98 (September 29, 1992) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 18 November 2026.
650. 1 2 "[PATCH] Linux-0.98.6 (December 2, 1992) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 20 May 2024.
651. ↑ "Linux Kernel Version History: 0.97 series kernels". *www.oldlinux.org* . Archived from the original on 10 September 2022. Retrieved 11 October 2020.
652. 1 2 "[PATCH] Linux-0.97.3 (September 5, 1992) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 19 May 2024.
653. 1 2 "[PATCH] Linux-0.97.5 (September 12, 1992) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 19 May 2024.
654. ↑ "Linux Kernel Version History: 0.96 series kernels". *www.oldlinux.org* . Archived from the original on 11 September 2022. Retrieved 11 October 2020.
655. ↑ "0.96c second patch - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 19 May 2024.
656. ↑ "Linux Kernel Version History: 0.95 series kernels". *www.oldlinux.org* . Archived from the original on 11 September 2022. Retrieved 11 October 2020.
657. ↑ "[PATCH] Linux-0.95c+ (April 9, 1992 ??) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 20 May 2024.
658. 1 2 "[PATCH] Linux-0.95 (March 8, 1992) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 20 May 2024.
659. 1 2 "Linux-0.12 (January 15, 1992) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 19 May 2024.
660. 1 2 3 4 5 6 "linux-0.12 is available - refs/tags/v0.12 - pub/scm/linux/kernel/git/nico/archive - Git at Google". *kernel.googlesource.com* . Retrieved 29 March 2024.
661. 1 2 3 "Linux-0.11 (December 8, 1991) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 20 May 2024.
662. 1 2 3 "Linux 0.10 (November 11, 1991 ???) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 20 May 2024.
663. 1 2 3 4 "Linux: The 0.02 and 0.03 Releases | KernelTrap". 11 March 2008. Archived from the original on 11 March 2008. Retrieved 13 May 2024.
664. ↑ "Happy 25th once again to Linux, 'the little OS that definitely could'. *PCWorld* . 5 October 2016. Retrieved 5 October 2019.
665. 1 2 3 4 5 "Linux-0.01 (September 17, 1991) - kernel/git/history/history.git - Linux kernel historic tree". *Kernel.org git repositories* . Retrieved 19 May 2024.
666. ↑ Mauerer, Wolfgang (2010). *Professional Linux Kernel Architecture* . John Wiley & Sons. p. 490. ISBN 9781118079911.

- Official Linux kernel website
- Active kernel releases, on the official Linux kernel website
- Linux versions changelog, in Linux Kernel Newbies
- Kernel coverage at LWN.net
- Linux Kernel Version History: Consolidated list at the Wayback Machine (archived 2023-04-06)
