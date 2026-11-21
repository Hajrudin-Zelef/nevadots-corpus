---
id: collect-261121-veille-ia/google-ecosystem/google-chrome-security-posture-report-card-q3-2026
title: "Google Chrome Security Posture Report Card Q3 2026"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-30"]
keywords: ["google", "chrome", "security", "posture", "report", "card", "releases", "period"]
source: https://www.secpod.com/learn/security-research/google-security-posture-report-card-q3-2026
source_anchor: ""
source_lines: [1, 149]
section: "Google_Ecosystem"
---
# Vendor Security Posture Report Card: Google - Q3 2026

Google’s Quarter 3 security posture is rated Adequate, reflecting rapid Chrome security updates, clear advisories, and one confirmed actively exploited V8 vulnerability.

## Vendor Summary

| Field | Assessment | 
|---|---|
| Vendor Name |  | 
| Product Lines Covered | Google Chrome Stable for desktop on Windows, macOS and Linux. ChromeOS, Android, iOS, Google Cloud and Google Workspace are outside the quantitative scope except where they provide relevant context. | 
| Reporting Period | Q3 2026 (July 1 through September 30, 2026) | 
| Overall Posture Rating | Adequate | 
| Prior Period Rating | Not available; this is the first report card in the series. | 
| Total CVEs Disclosed (Period) | Unavailable as a consolidated figure. Google does not publish a deduplicated CVE total for an arbitrary period. The 18 Stable-channel desktop releases reviewed list 1,297 security fixes; see Scope and Methodology for the limits of this count. | 
| Critical and High Severity Count | At least 395 across the 18 reviewed releases: 57 Critical and at least 338 High. High counts were confirmed for 16 of the 18 releases. | 
| CVEs With Confirmed In-the-Wild Exploitation | 2 verified, both in V8 and both exploited before a fix was available: CVE-2026-85046 (High, fixed September 3, 2026) and CVE-2026-87491 (Chromium rated Medium, fixed September 8, 2026). | 

## Rating Scale Reference

The rating scale applied consistently across every report card in this series, so that ratings remain comparable from one vendor and one period to the next:

| Rating | Criteria | 
|---|---|
| Strong | Disclosure volume and severity trend stable or improving, advisories timely and complete, no unpatched critical exposure at time of writing. | 
| Adequate | Posture broadly sound with isolated gaps, such as a delayed patch cycle or an incomplete advisory, that do not indicate a systemic pattern. | 
| Needs Improvement | Recurring gaps across the period: repeated delayed patches, incomplete disclosures, or a rising severity trend. | 
| Concerning | Evidence of a systemic issue: sustained critical exposure, exploitation preceding a patch, or a pattern of incomplete or delayed disclosure. | 

## Introduction

This report card assesses Google's handling of vulnerabilities in the desktop Stable channel of Chrome for Q3 2026, from July 1 through September 30. The overall posture is rated **Adequate**. The factor most responsible for the rating is the exploitation of two V8 vulnerabilities, CVE-2026-85046 and CVE-2026-87491, before their fixes shipped.

## Scope and Methodology

The assessment covers Chrome Stable for desktop on Windows, macOS and Linux from July 1 through September 30, 2026, because this product line has a consistent, public security-release record and represents a widely deployed enterprise attack surface. ChromeOS, Android, iOS, Google Cloud, Google Workspace and the Extended Stable channel are excluded. Evidence was drawn from Google's Chrome Releases posts, the Chromium issue references linked by those posts, and the United States government's Known Exploited Vulnerabilities reporting.

The quantitative review covers 18 Stable-channel desktop releases between July 8 and September 29 that list security fixes in Google's Chrome Releases posts. Where a severity count could not be confirmed for a release, it is marked NC (not confirmed) rather than estimated. The review is not guaranteed to be a complete inventory of every Q3 release, and other point releases may exist that were not reviewed. The Chrome 150 promotion on June 30, with 382 fixes and 15 Critical issues, fell one day before the quarter and is excluded. The review also does not claim to be a complete inventory of Google CVEs, because Google publishes updates across several products and channels. CVSS scores for the two exploited vulnerabilities exist in public vulnerability records but were not verified for this report.

## Vulnerability Disclosure Volume and Trend

The Q3 public record shows sustained security-fix activity. The 18 reviewed releases are listed below, with Chromium's four severity bands. Major-version promotions are labeled by Chrome version.

| Date (2026) | Stable build | Fixes | Critical | High | Medium | Low | 
|---|---|---|---|---|---|---|
| July 8 | 150.0.7871.114/.115 | 27 | 2 | 23 | 2 | 0 | 
| July 14 | 150.0.7871.124/.125 | 15 | 2 | 12 | 1 | 0 | 
| July 16 | 150.0.7871.128/.129 | 7 | 3 | 4 | 0 | 0 | 
| July 21 | 150.0.7871.181/.182 | 12 | 0 | 12 | 0 | 0 | 
| July 23 | 150.0.7871.186/.187 | 4 | 0 | 4 | 0 | 0 | 
| July 29 | 151.0.7922.71/.72 (Chrome 151) | 371 | 7 | 71 | 171 | 122 | 
| August 6 | 151.0.7922.108/.109 | 41 | 6 | 35 | 0 | 0 | 
| August 11 | 151.0.7922.137/.138 | 5 | 0 | 5 | 0 | 0 | 
| August 18 | 151.0.7922.169/.170 | 15 | 2 | 13 | 0 | 0 | 
| August 20 | 151.0.7922.173/.174 | 7 | 1 | 6 | 0 | 0 | 
| August 25 | 152.0.7977.64/.65 (Chrome 152) | 327 | 10 | 61 | 184 | 72 | 
| September 1 | 152.0.7977.75/.76 | 26 | 2 | 9 | 10 | 5 | 
| September 3 | 152.0.7977.82/.83 | 12 | 0 | 10 | 2 | 0 | 
| September 8 | 153.0.8010.36/.37 (Chrome 153) | 230 | 5 | 41 | NC | NC | 
| September 15 | 153.0.8010.47/.48 | 42 | 3 | NC | NC | NC | 
| September 17 | 153.0.8010.52/.53 | 16 | 2 | 7 | 6 | 1 | 
| September 22 | 154.0.8037.57/.58 (Chrome 154) | 108 | 11 | 25 | 47 | 25 | 
| September 29 | 154.0.8037.92/.93 | 32 | 1 | NC | NC | NC | 
| **Total** | **18 releases** | **1,297** | **57** | **At least 338** | **At least 423** | **At least 225** | 

NC: not confirmed. Dates are the dates shown on Google's Chrome Releases posts. Severity bands are Chromium's labels as published in Google's release posts. Band totals count only the releases where the band was confirmed: High in 16 of 18 releases, Medium and Low in 15 of 18. A band is shown as 0 only where that release's post lists no issue in the band.

A comparison to the prior reporting period is unavailable because this is the first report card in the series. Volume was steady across the quarter rather than rising: the reviewed releases carried 436 fixes in July, 395 in August and 466 in September. The table shows volume spikes on the four major-version promotion dates (July 29, August 25, September 8 and September 22). Together the promotions (Chrome 151, 152, 153 and 154) account for 1,036 of the 1,297 fixes, about 80 percent, and 33 of the 57 Critical issues. Google's posts do not explain these differences in volume; the link to promotions is an observation from the release record. These figures are release-level evidence rather than a complete Q3 CVE total.

## Severity Distribution Analysis

Google's release posts publish only Chromium's four severity labels (Critical, High, Medium and Low) and no CVSS scores, so the bands in this report are Chromium's labels. CVSS scores for the two exploited vulnerabilities exist in public vulnerability records but were not verified for this report. Chromium rated CVE-2026-85046 High and CVE-2026-87491 Medium, and both were exploited.

Across the 18 reviewed releases, Critical issues appeared in 14 and totaled 57, about 4 percent of the 1,297 fixes. High issues totaled at least 338 (confirmed in 16 releases), Medium issues at least 423 and Low issues at least 225 (both confirmed in 15 releases). Period totals for Medium and Low are unavailable because the Medium and Low split was not confirmed for three releases: September 8, September 15 and September 29.

The mix depends on release type. In the four major-version promotions, 805 of 1,036 fixes (about 78 percent) were Medium or Low. The Chrome 152 promotion carried 10 Critical issues: eight use-after-free defects, one improper input validation and one use of an uninitialized variable. Nine of the ten were credited to Google's own reporting. Attribution varies by release: all seven Critical issues in Chrome 151 were credited to Google, while two of the eleven in Chrome 154 were.

The recurring presence of use-after-free, type-confusion, out-of-bounds access and buffer-overflow defects confirms that memory-safety vulnerabilities remain a material part of the browser's risk profile. Severity labels alone are not sufficient for prioritization: both exploited vulnerabilities were rated High or Medium by Chromium, while 57 Critical issues were fixed without reported exploitation.

Whether the distribution is weighted toward higher severity relative to the prior period or to a typical distribution for this vendor is unavailable. This is the first report card in the series, and no baseline distribution has been established for Chrome. This Q3 analysis serves as that baseline.

## Patch and Remediation Timeliness

Google followed its rapid Stable-channel model throughout the reviewed Q3 releases, publishing security fixes on 18 dates. Each advisory announced fixed Windows, macOS and Linux versions and stated that deployment would roll out over the following days or weeks. The September 3 and September 8 releases delivered the fixes for CVE-2026-85046 and CVE-2026-87491 and disclosed the known exploitation status in the same posts.

A precise average or typical time-to-patch cannot be calculated consistently because internal discovery and disclosure dates are not uniformly equivalent. For CVE-2026-85046, the public release records an August 4 researcher report and a September 3 Stable-channel fix, a 30-day interval. For CVE-2026-87491, the report date is August 6 and the Stable fix followed on September 8, a 33-day interval. Google's Chrome for Developers announcement of March 2026 moved Chrome from a four-week to a two-week major release cycle starting with Chrome 153 on September 8, with weekly security updates continuing. The promotion dates in the reviewed releases follow that schedule: Chrome 151 (post dated July 29) and Chrome 152 (August 25) were about four weeks apart, and Chrome 152, Chrome 153 (September 8) and Chrome 154 (September 22) were each two weeks apart. The reviewed releases also include at least one Stable desktop security release in every week from the week of July 6 through the week of September 28. Adherence to the announced cadence is therefore consistent across the quarter. No case in which a fix lagged significantly behind disclosure was identified in the reviewed evidence, so that item is unavailable.

Vendor release speed is only the first half of remediation. Exposure continues until managed endpoints receive the fixed build and users restart Chrome, so deployment completion and browser relaunch determine how long an organization remains exposed after a fix ships.

## Exploitation and Real-World Impact

Two vulnerabilities from this period were confirmed exploited in-the-wild, and both are in the V8 JavaScript engine. CVE-2026-85046, a High-severity type confusion, was fixed on September 3 and added to the government exploitation catalog on September 4. CVE-2026-87491, an out-of-bounds write that Chromium rated Medium, was fixed on September 8 and added to the catalog on September 9. In each case the catalog entry followed Google's Stable-channel release by one day.

**Exploitation preceded the patch in both cases.** Google's release posts stated that an exploit existed in-the-wild when the fixed builds shipped, so attackers had working exploits before a fix was available. Google has not published when exploitation began or who was responsible, so the length of the pre-patch exposure is unavailable.

Two confirmed exploited vulnerabilities is a small number relative to 1,297 security fixes, but their operational importance is high because browser exploitation can be triggered through untrusted web content and because the Chromium codebase is used by browsers beyond Chrome. No exploitation was reported in the release posts for the Critical issues fixed in the Chrome 151, 152 and 154 promotions.

## Advisory and Communication Quality

Google's Q3 Chrome release communications were timely and technically useful. The reviewed posts consistently included fixed-version numbers by operating system, the number of security fixes, severity labels, CVE identifiers, vulnerability classes, researcher attribution and links to Chromium issues. Google also clearly stated when exploitation was known. Three further aspects of advisory completeness were assessed:

- **Versioned and dated updates:** Each post carries a publication date, but the posts reviewed show no revision history or dated change notes. For example, the July 29 Chrome 151 post lists 371 fixes and has no revision note.
- **Remediation guidance:** Guidance is limited to fixed build numbers and the statement that rollout takes days or weeks. The posts reviewed contain no workarounds, mitigations or relaunch instructions.
- **Severity scoring:** Posts carry Chromium severity labels but no CVSS scores, and CVSS scores for the exploited vulnerabilities are not verified here.

Two further limitations reduce completeness. First, Google may restrict issue details until most users have received a fix, which is a reasonable protection measure but temporarily limits independent analysis. Second, release posts do not provide one normalized, deduplicated CVE dataset for an arbitrary reporting period. This makes period-wide metrics more difficult to reproduce than individual release findings.

## Notable Incidents This Period

### CVE-2026-85046: Exploited V8 Type Confusion

Google released Chrome 152.0.7977.82 for Linux and 152.0.7977.82 or .83 for Windows and macOS on September 3. The update corrected CVE-2026-85046 and 11 other security issues. Google confirmed that an exploit existed in-the-wild, and the government catalog set a September 18 remediation due date for covered federal systems. The incident prevented a Strong rating, even though the vendor supplied a fix and clear exploitation notice.

### CVE-2026-87491: Exploited V8 Out-of-Bounds Write

Five days later, Google released Chrome 153.0.8010.36 for Linux and 153.0.8010.36 or .37 for Windows and macOS on September 8. The Chrome 153 promotion carried 230 fixes, including 5 Critical and 41 High, and corrected CVE-2026-87491. Google confirmed that an exploit existed in-the-wild. Chromium rated the flaw Medium, even though it was exploited. The government catalog added it on September 9 with a September 23 due date. Two exploited flaws in the same engine five days apart is a significant factor in this assessment.

## Historical Trend Comparison

Historical trend comparison is unavailable. This is the first report card in the series, so there is no prior-period rating, fix count or exploitation count against which to compare Q3 2026. The figures in this report, namely 1,297 fixes across 18 reviewed releases, 57 Critical issues and two confirmed exploited vulnerabilities, serve as the baseline for future editions.

## Risk Context for Organizations

An organization's exposure to Chrome vulnerabilities depends on more than the vendor's release speed. Google publishes a fixed build quickly, but exposure persists on each managed device until that device receives the update and the user restarts the browser. The Q3 record shows how short that window can matter: two exploited vulnerabilities were fixed five days apart, and the government catalog set remediation due dates of September 18 and September 23 for covered federal systems.

The shared Chromium codebase widens the context. The V8 engine is used by other Chromium-based browsers, which receive fixes on their own schedules, so exposure can extend beyond Chrome itself. Environments with unmanaged Chromium-based browsers, unrestricted extensions, long restart deferrals or limited visibility into endpoint patch status carry greater risk than the vendor's release cadence alone suggests. For organizations that rely on Chrome, the Adequate rating warrants closer monitoring of patch status and accelerated patch prioritization for releases that carry an exploitation notice, with no change to the decision to rely on the product.

## Overall Posture Rating and Rationale

**Overall Q3 rating: Adequate.** Three factors most influenced the rating. First, Google's release cadence was frequent and its advisories were clear and timely, as covered in the Patch and Remediation Timeliness and Advisory and Communication Quality sections. Second, two V8 vulnerabilities were exploited before their fixes shipped, as covered in the Exploitation and Real-World Impact and Notable Incidents This Period sections, which prevents a Strong rating. Third, exploitation preceding a patch is a Concerning indicator on the rating scale, but no sustained exposure and no incomplete or delayed disclosure was identified in the reviewed evidence, so the rating is Adequate rather than Concerning.

## Key Takeaways

- Google's Q3 2026 Chrome desktop security posture is rated Adequate.
- Two V8 vulnerabilities, CVE-2026-85046 and CVE-2026-87491, were exploited in-the-wild before their fixes shipped on September 3 and September 8, and each was added to the government exploitation catalog one day after its fix.
- Eighteen reviewed Chrome desktop releases corrected 1,297 security issues, including 57 Critical, and four major-version promotions account for about 80 percent of the fixes.
- Google's Chrome advisories give clear release-level detail but carry no CVSS scores, no visible revision history and no mitigation guidance.
- Enterprise exposure to Chrome vulnerabilities depends on update and browser-restart completion, not merely on the publication date of a fixed build.

## Conclusion

Google's Q3 2026 Chrome security posture is Adequate. Rapid Stable-channel updates and clear advisories were offset by two exploited V8 vulnerabilities and a consistent flow of Critical memory-safety fixes. This rating is a point-in-time assessment tied to ongoing vendor and compliance monitoring, which will track the frequency and timing of vulnerabilities exploited before a patch, the concentration of Critical fixes in major-version promotions, and whether Google adds CVSS scoring, revision history or mitigation guidance to its advisories. The next report card for Google is expected after the close of Q4 2026.

## Instantly Fix Risks with Saner Patch Management

Saner patch management is a continuous, automated, and integrated software that instantly fixes risks exploited in the wild. The software supports major operating systems like Windows, Linux, and macOS, as well as 550+ third-party applications.

It also allows you to set up a safe testing area to test patches before deploying them in a primary production environment. Saner patch management additionally supports a patch rollback feature in case of patch failure or a system malfunction.

Experience the fastest and most accurate patching software here.
