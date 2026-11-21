---
id: collect-261121-veille-ia/windows-os/office-2021-end-of-support-options-what-to-do-before-2026
title: "Office 2021 End of Support Options: What to Do Before 2026"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["office", "support", "microsoft", "october", "security", "cost", "best", "main"]
source: https://windows.gadgethacks.com/how-to/office-2021-end-of-support-options-what-to-do-before-2026/
source_anchor: ""
source_lines: [1, 70]
section: "Windows_OS"
---
Office 2021 support ends on October 13, and after that date, Microsoft stops shipping security updates and technical support for the suite. There's no extension and no paid Extended Security Updates program at any price to push that date back. Office 2021 is expected to keep launching past that date, but it won't get another security fix, bug fix, or ESU patch of any kind. Weighing the real Office 2021 end of support options now beats scrambling later, after a vulnerability goes unpatched.

This Office 2021 end-of-life deadline applies to the listed retail Office 2021 editions, including Home & Student, Home & Business, and Professional. Office LTSC 2021 also reaches end of support on October 13, 2026. Whichever one sits on a machine right now, the same cutoff applies no matter which box or license key got purchased.

Picking a path comes down to two questions: how much security exposure is tolerable, and whether files, macros, and workflows survive a move to something else. The table below lays out five realistic paths, then two shorter sections cover a diagnostic worth running first and a separate Mac/iPhone issue that's easy to mistake for part of this deadline.

- **Stay on Office 2021**
  - **Cost:** No new spending
  - **Best for:** Offline-only work and rarely opening outside files
  - **Main limitation:** No further fixes after October 13, 2026 (WindowsForum)
- **Buy Office 2024 / LTSC 2024**
  - **Cost:** One-time purchase
  - **Best for:** Desktop apps without a subscription
  - **Main limitation:** Supported only through October 9, 2029
- **Subscribe to Microsoft 365**
  - **Cost:** Recurring subscription fee
  - **Best for:** Apps that need to stay current across devices
  - **Main limitation:** Confirm tier, pricing, and lapse terms directly with Microsoft
- **Switch to LibreOffice**
  - **Cost:** Free to install
  - **Best for:** Standard documents without heavy VBA or Access use
  - **Main limitation:** Must stay on a currently supported branch
- **Try web-based Office apps**
  - **Cost:** Free to test
  - **Best for:** Light editing with no offline requirement
  - **Main limitation:** Feature limits aren't established in the sourcing here

Since July 13, 2026, some Office and Microsoft 365 apps on macOS and iOS have been dropping into what Microsoft calls reduced functionality mode: files open and print, but editing, saving, or creating new files stops working. It hits Microsoft 365 subscribers and owners of non-subscription versions, including Office 2021 and Office 2019 on Mac, the same way.

This is separate from the October 13 lifecycle deadline. Microsoft says older operating systems can trigger this reduced-functionality state, and explains that the certificate involved validates the Office license rather than protecting file or device security. Windows and Android devices aren't affected at all.

For most people, the fix is to update macOS to version 12 or later with Office 16.83, iOS or iPadOS 17 or later with Office 2.93. If editing is still blocked afterward, quit the app and sign back in.

Hardware that can't reach those versions is the real catch. A Mac below macOS 12, or an iPhone or iPad below iOS 17, has no path to the required update, and Office 2019 for Mac, already past its own end of support since 2023, can't be patched back into shape because it can't reach the needed version either. On a device that genuinely can't update, a browser-based editor may offer an alternative, but check that it actually supports the file type and device before leaning on it; the same caution applies to any third-party mobile app, since compatibility isn't something this guide's sourcing establishes.

1. **Confirm the exact edition installed.** On Windows, check File > Account; on Mac, check About [App Name]. Retail Office 2021 and the volume-licensed Office LTSC 2021 hit the same October 13 wall, but they're sold and licensed differently, so don't assume one applies without checking.
2. **List what actually carries the daily workload.** Basic document and spreadsheet work opens up more of the paths below than heavy reliance on Outlook, Access, or VBA macros does. Flag those dependencies now; they're what the workflow test below is for, not something to assume transfers cleanly.
3. **Check for a dormant Microsoft 365 subscription.** Open File > Account and look for an active sign-in tied to Microsoft 365 Personal, Family, or a work/school account. If one turns up, it may already cover what's needed without a new purchase; confirm current tiers, pricing, and included apps on Microsoft's official plans page before ruling anything out.

Whatever edition and dependencies surface, settle them with one test: open the most demanding files in daily use, a macro-enabled workbook, a tracked-changes document, a presentation with embedded media, inside whichever alternative is under consideration, and watch for anything that breaks. Call this the workflow test. It gets referenced through the five paths below instead of being re-explained each time.

This suits genuinely offline-dominant work, rarely opening documents, attachments, or downloads from outside personal devices. It costs nothing and needs no migration.

What it gives up is real. Microsoft will no longer provide fixes for vulnerabilities discovered after October 13, 2026, and there's no paid ESU option to change that. Because Office 2021 won't receive further Microsoft fixes, treat files from email, downloads, and shared storage as higher-risk input, and avoid opening them in the unsupported app wherever another option exists. Set a personal deadline to move off this path rather than let it become permanent by default.

Office 2024 and Office LTSC 2024 are both currently supported perpetual-license routes, retiring on the same date, October 9, 2029; Office 2024 also adds ODF 1.4 support, useful for anyone trading files with LibreOffice users.

Neither is an indefinite entitlement. A future major version means another full-price purchase down the line, and Office 2024 only receives security updates during its current support window. LTSC 2024 follows that same 2029 endpoint as a volume-licensed product aimed at commercial customers rather than the standard retail buy. Whether a trial exists before purchase isn't established in the sourcing here, so check Microsoft's product page directly rather than assume one does. Decide whether the LTSC edition or standard retail Office 2024 fits, confirm current pricing with Microsoft, and buy from there.

A subscription suits anyone who'd rather keep the apps current than lock into one fixed version. Plan tiers, pricing, and which desktop apps each one bundles aren't detailed in the research behind this guide, so check Microsoft's official Microsoft 365 plans page for what's included, what cross-device access actually covers, and what happens to installed apps if a subscription lapses.

The dormant-subscription check from the diagnostic step above settles part of this decision already. If nothing active turns up, weigh the recurring cost against buying Office 2024 outright or switching to something free.

LibreOffice is a credible zero-license-cost option for workflows that pass the test, provided it stays on a currently supported branch and gets patched like any other exposed application (WindowsForum). It defaults to OpenDocument formats, ODT, ODS, and ODP, which Word, Excel, and PowerPoint can also open and save, though fidelity for macros, pivot tables, and complex layouts is exactly what the workflow test above exists to catch.

The Document Foundation's "yours for a lifetime" framing describes free access to future releases, not indefinite support for any single version. LibreOffice 25.8 reached end of life on June 12, 2026, with its final release explicitly marked as the last maintenance update and users redirected to the 26.2 branch for continued security patches. Organizations wanting an SLA-backed version can get one through ecosystem partners offering multi-year security backports, though that's a business consideration most individual users don't need to weigh. Run the workflow test before uninstalling anything.

For light needs, basic browser-based editing tied to a Microsoft account is worth a look too. This guide's sourcing doesn't document specific feature limits, offline behavior, or file-compatibility edge cases for these web apps, so treat it as a path to verify directly on Microsoft's site rather than a fully vetted desktop replacement.

Open one representative file there before counting on it for anything beyond basic editing. Anyone leaning on complex formatting, macros, or offline access day to day will likely hit the edges of what a browser tab can do fairly quickly.

Handle the Mac or iPhone certificate problem first if it applies. It's unrelated to which Office path gets chosen and won't resolve itself by switching suites.

Past that, the decision splits by what the files in daily use actually need. Anyone who wants familiar offline desktop apps without a subscription should evaluate retail Office 2024 pricing now. Anyone whose work is ordinary documents and spreadsheets should run the workflow test in LibreOffice. Anyone who only needs light editing should try a web app on one real file. Anyone dependent on macros, Access, Outlook, or complex formatting shouldn't uninstall Office 2021 until those specific workflows pass somewhere else, regardless of which option looks best on paper.

Run that test before October 13, then commit to whichever option passes with the fewest compromises.

## Comments

Be the first, drop a comment!
