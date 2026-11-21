---
id: collect-261121-veille-ia/actu-business/november-2026-patch-tuesday-outlook-and-chrome-zero-days-first
title: "November 2026 Patch Tuesday: Outlook and Chrome zero-days first"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-11-11"]
keywords: ["november", "microsoft", "windows", "chrome", "outlook", "exploited", "rated", "code"]
source: https://ecorpit.com/november-2026-patch-tuesday-outlook-chrome-zero-days/
source_anchor: ""
source_lines: [1, 168]
section: "Actu_Business"
---
**Summary.** Patch two things first: Outlook, through Microsoft 365 Apps or Office LTSC, and Google Chrome. Microsoft's 10 November 2026 release fixes CVE-2026-124194, an Outlook spoofing flaw rated CVSS 6.5 and Moderate that Microsoft marks as exploited: opening a crafted message file makes Outlook contact an attacker's server without its usual external-content warning, which leaks the user's NTLMv2 hash. Google's 9 November update, Chrome 157.0.8103.50/.51, fixes CVE-2026-125350, a use-after-free in XML with an exploit in the wild. CISA added both to its Known Exploited Vulnerabilities (KEV) catalogue on 10 November with a due date of 24 November, 13 days after 11 November. Servers come next. CVE-2026-116928 (CVSS 9.8) lets an emailed attachment run code through the Windows Search component that Exchange uses for indexing, with no click needed, and Exchange Server Subscription Edition needs its own update, KB5133453, for nine Exchange CVEs. Microsoft Edge on desktop had no listed fix for the Chrome bug on 11 November. The update guide lists 1,095 Microsoft-issued CVEs this month, and only one is marked exploited. Our order: exploited first, zero-click server bugs second, everything else in normal rings, with build numbers as proof.

**About this article.** For IT managers and managed service providers running Windows, Microsoft 365 and Chrome fleets. On 11 November 2026 we checked every CVE, score, KB number, fixed build and KEV date against the Microsoft Security Response Center (MSRC) Security Update Guide API, Microsoft Learn and Support pages, Chrome Releases, NIST NVD and CISA's KEV feed. This is defensive guidance for general information, not legal advice; vendors revise advisories, so re-check before you act, and no vendor paid for inclusion. Researched with AI assistance and reviewed by our editorial team.

This article covers endpoints, Microsoft 365 and Windows servers. For remote-access appliances, see our companion piece on exploited VPN flaws in Ivanti, Cisco and SonicWall.

## What shipped on 9 and 10 November 2026

Google published Chrome 157 to the Stable channel on Monday 9 November. Microsoft published its monthly security release, labelled 2026-Nov in the Security Update Guide, on Tuesday 10 November. The table lists the entries that set the patch order. Scores are the vendor's CVSS 3.1 base score unless noted, and the KEV dates come from CISA's catalogue feed.

| CVE | Product | Type | CVSS | Exploited | KEV due | Fixed in | 
|---|---|---|---|---|---|---|
| CVE-2026-124194 | Outlook (Microsoft 365 Apps, Office LTSC 2024 and 2021) | Spoofing, NTLMv2 hash leak | 6.5 | Yes | 24 Nov 2026 | November Office builds | 
| CVE-2026-125350 | Google Chrome | Use-after-free in XML | 8.8 (CISA) | Yes | 24 Nov 2026 | 157.0.8103.50 and later | 
| CVE-2026-116928 | Windows Search component, reached through Exchange | Remote code execution, no user action | 9.8 | No | Not in KEV | November Windows cumulative updates | 
| CVE-2026-118207 | Exchange Server Subscription Edition | Remote code execution, authenticated | 8.8 | No, rated more likely | Not in KEV | KB5133453, build 15.02.2562.057 | 
| CVE-2026-119958 | Windows LDAP | Remote code execution | 9.8 | No | Not in KEV | November Windows cumulative updates | 
| CVE-2026-118146 | Active Directory Domain Services | Remote code execution, authenticated | 8.8 | No, rated more likely | Not in KEV | November Windows cumulative updates | 
| CVE-2026-119952 | Windows Schannel | Remote code execution | 8.1 | No, rated more likely | Not in KEV | November Windows cumulative updates | 
| CVE-2026-118148 | Active Directory Domain Services | Elevation of privilege | 7.5 | No, rated more likely | Not in KEV | November Windows cumulative updates | 

Rated more likely refers to Microsoft's exploitability assessment, published with each CVE. It is a forecast, not a report of attacks. Microsoft marks exactly one November CVE as exploited, the Outlook bug, and none as publicly disclosed before release.

## The Outlook zero-day: CVE-2026-124194

Microsoft describes CVE-2026-124194 as a user-interface flaw (CWE-356) in which Outlook fails to warn the user before an unsafe action. Its FAQ sets out the attack. An unauthenticated attacker sends a crafted message file and persuades the user to open it. Outlook then connects to an attacker-controlled network location without showing the external-content warning, and that connection discloses the user's NTLMv2 hash, which an attacker can use to authenticate as the user.

Three details shape your response:

- **The Preview Pane is not an attack vector.** Microsoft says so in the FAQ. The user has to open the file.

- **The score undersells the risk.** CVSS 6.5 and a Moderate rating reflect the user-interaction requirement. The temporal vector marks exploit code maturity as high, and Microsoft confirms use in attacks. A leaked hash for a finance or IT administrator is worth far more than the number suggests.

- **Only Office channels fix it.** The fix ships in Microsoft 365 Apps and Office LTSC builds, not in the Windows cumulative update. A PC that is current on Windows but lagging on Office is still exposed.

MSRC lists three affected products: Microsoft 365 Apps for Enterprise (32-bit and 64-bit), Office LTSC 2024 (fixed in build 16.0.17932.21140) and Office LTSC 2021 (fixed in build 16.0.14334.21048). For Microsoft 365 Apps, Microsoft points to its security release notes, which list the 10 November builds per channel.

Office 2019 is not on the affected-product list. Its support ended on 14 October 2025, although the same release notes still list a November build for volume-licensed Office 2019 (version 1808, build 10417.20236). Do not read its absence from the list as proof that it is safe; plan to move those users.

The same Office update also fixes three Critical Outlook remote code execution bugs, CVE-2026-110043, CVE-2026-110068 and CVE-2026-111309, each scored 8.8. For all three, Microsoft says the Preview Pane is an attack vector. None is marked exploited, but they are one more reason to get the Office update out fast.

CISA added CVE-2026-124194 to the KEV catalogue on 10 November with a due date of 24 November. That date binds US federal civilian agencies under Binding Operational Directive 26-04, issued on 10 June 2026, which replaced BOD 22-01 and sets KEV timelines by asset exposure, exploit automation and technical impact. For everyone else it is a useful outside benchmark: if a US agency has 14 days, a private company running the same software has no good reason to take longer.

## The Chrome zero-day: CVE-2026-125350, and the Edge gap

Chrome 157.0.8103.50/.51 for Windows and Mac, and 157.0.8103.50 for Linux, carries nine security fixes. CVE-2026-125350 is a use-after-free in Chrome's XML handling. Google says it knows an exploit exists in the wild. The bug was reported anonymously on 6 November, and the fix shipped three days later. A second XML use-after-free, CVE-2026-125346, is fixed in the same build. According to the NVD record, a crafted HTML page can use CVE-2026-125350 to run arbitrary code inside Chrome's sandbox, so one visit to a malicious or compromised site can start an attack chain.

Other channels matter for mixed fleets:

- **Extended Stable** moved to 156.0.8078.96 for Windows and Mac on 9 November, with 11 security fixes that include CVE-2026-125350.

- **Chrome for Android** 157.0.8103.50 shipped the same day. Google says Android releases carry the same security fixes as the matching desktop release unless noted.

- **ChromeOS** Stable was last updated on 6 November to browser version 156.0.8078.93, and its list of fixes does not include CVE-2026-125350. The Chrome Releases feed showed no newer ChromeOS Stable post on 11 November, so do not mark Chromebooks done yet.

Microsoft Edge is built on Chromium, and CISA's KEV entry warns that the flaw may affect other Chromium browsers, naming Edge and Opera. When we checked on 11 November, the MSRC API returned no entry for CVE-2026-125350. Microsoft's Edge security release notes, last updated 9 November, say Microsoft knows of recent in-the-wild exploits and is working on a security fix, without naming the CVE. The newest desktop Stable listed there is 157.0.4341.42 from 5 November, a day before Google received the bug report. If Edge is your managed desktop browser, watch that page and push the update the day it lands.

## Server bugs that jump the queue

Exploited bugs come first, but three groups of server bugs deserve the next slot because of how they can be reached.

**CVE-2026-116928 on Exchange servers.** This is an out-of-bounds write in the Windows Search component, CVSS 9.8, Critical. Microsoft's FAQ describes the path: an attacker emails a crafted attachment, Exchange processes it automatically for indexing, and code runs with the permissions of the processing service. The recipient does not need to open anything. Microsoft rates exploitation less likely and lists the bug as not publicly disclosed, but CISA's enrichment on the NVD record marks it as automatable. The fix ships in the November Windows cumulative update for every Windows version on the affected list, and the entry gives no workaround, so patch.

**Exchange Server Subscription Edition.** KB5133453 fixes nine Exchange CVEs. The most serious, CVE-2026-118207, is a Critical remote code execution flaw (CVSS 8.8) that an attacker with low-level access can trigger without user interaction, and Microsoft rates it exploitation more likely. The others include three more 8.8 bugs (one remote code execution, two privilege escalations), a privilege escalation to SYSTEM and an unauthenticated denial of service. The update moves Exchange SE RTM to build 15.02.2562.057 and needs a restart. An Exchange server needs both the Windows update and the Exchange update this month. Exchange SE is the only Exchange version on this month's affected-product list; Exchange Server 2019 left extended support on 14 October 2025.

**Domain controllers and TLS-facing servers.** CVE-2026-119958 is a Critical Windows LDAP remote code execution flaw rated 9.8, reachable by an unauthenticated attacker through crafted LDAP calls. Three more Critical bugs carry the exploitation-more-likely rating: CVE-2026-118146, an authenticated Active Directory Domain Services remote code execution flaw (8.8); CVE-2026-118148, an AD DS privilege escalation to SYSTEM (7.5); and CVE-2026-119952, a Windows Schannel remote code execution flaw (8.1). Domain controllers and servers that terminate TLS on Windows belong in the ring straight after Exchange.

The rest of the Critical list is long but familiar: Excel, Word and PowerPoint remote code execution bugs fixed by the Office update; Windows Message Queuing, IIS, CredSSP, Graphic Fonts and Windows Deployment Services remote code execution; a Windows NAT remote code execution bug also rated more likely; and Hyper-V and Remote Desktop Services privilege escalations. None is marked exploited.

## How big is the November 2026 release?

Headline counts vary because sources count differently. We queried the MSRC Security Update Guide API on 11 November.

| Filter on the 2026-Nov release | Entries returned | 
|---|---|
| All entries | 1,383 | 
| Issued by Microsoft as numbering authority | 1,095 | 
| Rated Critical | 64 | 
| Marked exploited | 1 (CVE-2026-124194) | 
| Marked publicly disclosed | 0 | 

The 288-entry gap between 1,383 and 1,095 is CVEs issued by other numbering authorities that the guide tracks alongside Microsoft's own, led by 165 Chrome CVEs and 41 from the Linux kernel. Tenable's Patch Tuesday analysis counts 1,093 CVEs, 63 of them Critical. For planning, the shape matters more than the count: one exploited Microsoft bug, one exploited Chrome bug, and a handful of server bugs reachable without a click.

A monthly figure above 1,000 also changes how you work. Nobody reads a thousand advisories. Sort by three filters, in order: exploited or in KEV; reachable without authentication or user action on an internet-facing or mail-handling system; Critical on systems that hold identity (domain controllers, Exchange, identity servers). Everything else rides the normal monthly ring.

## The prioritised patch order for November 2026

This is the order we would follow for a typical Microsoft 365 and Windows estate. The time targets are our suggestions, measured from 10 November, set to finish everything exploited before the 24 November KEV date.

| Order | What to deploy | Why | Suggested target | 
|---|---|---|---|
| 1 | Chrome 157.0.8103.50/.51 (Extended Stable 156.0.8078.96), with a forced relaunch | Exploited in the wild; KEV due 24 Nov | Within 48 hours | 
| 2 | Microsoft 365 Apps and Office LTSC November builds | Outlook zero-day exploited; KEV due 24 Nov; also fixes Preview Pane Outlook RCEs | Within 3 to 5 days | 
| 3 | Exchange servers: Windows cumulative update plus Exchange SE KB5133453 | Zero-click CVE-2026-116928 (9.8) and an Exchange RCE rated more likely | Within 7 days | 
| 4 | Domain controllers and internet-facing Windows servers | LDAP RCE (9.8), plus AD DS and Schannel bugs rated more likely | Within 7 to 10 days | 
| 5 | Workstation broad ring: Windows cumulative update | Graphic Fonts, CredSSP and other Critical RCEs; none exploited | Within 14 days | 
| 6 | Remaining servers, Hyper-V hosts, MSMQ and IIS roles, lab machines | Critical but role-dependent | Within 30 days | 
| Watch | Microsoft Edge desktop, ChromeOS | No fix listed for CVE-2026-125350 on 11 Nov | Same day as release | 

Steps 1 and 2 are client-side and usually low-risk to deploy quickly, because browsers and Click-to-Run Office update in place. Steps 3 and 4 need maintenance windows and a rollback plan, which is why they sit behind the exploited client bugs even though their scores are higher.

Two edge cases. For Windows 11 23H2 Enterprise and Education, this is the last monthly update: servicing ended on 10 November 2026, as our Windows 11 24H2 and 25H2 servicing guide explains, so those devices need a feature update after this patch. Windows 10 22H2 receives the November update only through Extended Security Updates; MSRC lists it under the ESU product family. Our Windows 10 ESU guide covers enrolment and the 12 October 2027 end of consumer ESU.

## Fixed builds to check, product by product

The builds below come from the MSRC affected-product data, the Windows and Exchange KB pages and Microsoft's Office release notes. A device at or above the build is patched for the November release.

| Product | Update | Fixed build | 
|---|---|---|
| Windows 11 24H2 / 25H2 / 26H2 | KB5132061 (hotpatch KB5132043) | 26100.9974 / 26200.9974 / 26300.9974 (hotpatch .9911) | 
| Windows 11 26H1 | KB5132059 | 28000.3501 | 
| Windows 11 23H2 (final update) | KB5131087 | 22631.7711 | 
| Windows 10 22H2 (ESU) and 21H2 | KB5131096 | 19045.7846 and 19044.7846 | 
| Windows Server 2025 | KB5131080 (hotpatch KB5131077) | 26100.33822 (hotpatch .33730) | 
| Windows Server 2022 | KB5131090 | 20348.5870 | 
| Windows Server 2019 and Windows 10 1809 | KB5131085 | 17763.9487 | 
| Windows Server 2016 and Windows 10 1607 | KB5131291 | 14393.9688 | 
| Exchange Server SE RTM | KB5133453 | 15.02.2562.057 | 
| Microsoft 365 Apps Current Channel; Office 2024 and 2021 retail | Version 2610 | 20527.20196 | 
| Microsoft 365 Apps, Monthly Enterprise Channel | Versions 2610 / 2609 / 2608 | 20527.20194 / 20430.20264 / 20326.20320 | 
| Office LTSC 2024 / LTSC 2021 | Volume licensed | 17932.21140 / 14334.21048 | 
| Google Chrome desktop | Stable / Extended Stable | 157.0.8103.50 or .51 / 156.0.8078.96 | 

Microsoft's release notes also list Semi-Annual Enterprise Channel devices as receiving Monthly Enterprise Channel builds (20527.20192, 20430.20262 and 20326.20318). If your Office fleet sits on a channel that lags by months, check that it is on one of these builds rather than assuming the channel is current.

Windows Server 2016 reaches the end of extended support on 12 January 2027, so the December 2026 and January 2027 updates should be its last free ones; our Windows Server 2016 migration playbook covers the options.

## How to verify deployment, not just approval

Most missed patches are not missed decisions. They are updates that were approved, downloaded and never activated: a browser that was not relaunched, an Office install that never closed, a server whose restart was deferred. Verify in three layers.

**Layer 1: your patch tool's report.** Intune, Configuration Manager, WSUS or your RMM will show approval and install status. Treat it as a starting list, not proof. Filter on the devices that matter most (Exchange servers, domain controllers and admin workstations) and look at those marked pending restart.

**Layer 2: the device itself.** Read the build from the machine. These PowerShell lines return what is running or installed, not what is queued; the Exchange command is the one Microsoft documents for reading the build.

Run them through your management tool across the fleet and compare against the fixed-build table. For Chrome, the version on disk changes when the update downloads, but the vulnerable code keeps running until the browser restarts. Chrome's RelaunchNotification policy can make a relaunch required rather than recommended, and RelaunchNotificationPeriod sets how long users have; this month, a short period is justified.

**Layer 3: an outside check.** Run an authenticated vulnerability scan after each ring and confirm the KEV-listed CVEs drop to zero on scanned assets. Keep a short exceptions register: device, reason, compensating control, owner and date. On 24 November you should be able to show, for each exploited CVE, the number of devices patched, the number excepted and why. That is the evidence an auditor, a customer security questionnaire or your own board will ask for.

## If you cannot patch everything by 24 November

Some devices will lag: kiosks, shared PCs, laptops that rarely connect, or servers waiting for a change window. Reduce exposure while you chase them.

- **Outlook hash leak.** Microsoft recommends blocking TCP port 445 outbound to the internet at the corporate firewall; if you have not done so, do it now so a leaked authentication attempt has nowhere to go. Members of the Protected Users group cannot authenticate with NTLM to domain controllers, so add high-value accounts where that does not break their workflow. These steps limit the value of a leaked hash; they do not fix the Outlook flaw.

- **Chrome.** There is no workaround for a browser memory-safety bug. Enforce the update and the relaunch, and keep web filtering on while stragglers catch up.

- **Exchange.** Patch these servers before anything else on the server side. If a restart has to wait, schedule it for the next available window rather than the next monthly cycle.

- **Compromise checks.** For the Outlook bug, look in firewall and proxy logs for SMB or WebDAV connections from user workstations to external addresses since early November. For Chrome, look for browser crashes followed by unusual child processes on endpoints with detection and response tooling. Neither check is conclusive; both are cheap.

If you find signs of compromise, follow your incident-response plan and your reporting duties. In India, CERT-In's directions of 28 April 2022 (No. 20(3)/2022-CERT-In) require service providers, intermediaries, data centres, body corporates and government organisations to report listed incident types within six hours of noticing them. Other jurisdictions set their own timelines.

## What to watch before 31 December 2026

- **Edge desktop fix.** Expect an Edge Stable release and an MSRC entry for CVE-2026-125350; deploy it the same day.

- **ChromeOS Stable.** Look for the next Stable post on Chrome Releases with a browser version that includes the fix.

- **24 November.** The KEV due date for both exploited CVEs; have your patched and excepted counts ready.

- **8 December.** The next Patch Tuesday. Check whether Microsoft revises the exploitation status of CVE-2026-116928 or any Exchange CVE before then, and whether CISA adds another November CVE to KEV.

## How eCorpIT can help

eCorpIT is a Gurugram-based technology organisation, founded in 2021, assessed at CMMI Level 5 and MSME certified, with senior-led engineering teams working across AWS, Microsoft and Google platforms. Our cybersecurity services team can rank a release like this against your asset inventory, run the priority rings and build device-level evidence before the KEV dates. Talk to us at /contact-us/.

Last updated: 11 November 2026.
