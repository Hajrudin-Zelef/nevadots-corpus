---
id: collect-261121-veille-ia/windows-os/office-2016-2019-and-2021-end-of-life-dates-and-what-to-do-now
title: "Office 2016, 2019 And 2021 End of Life: Dates And What to do Now"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-13"]
keywords: ["office", "support", "october", "ltsc", "dates", "every", "microsoft", "life"]
source: https://blog.invgate.com/microsoft-office-2016-2019-end-of-life
source_anchor: ""
source_lines: [1, 124]
section: "Windows_OS"
---
Office 2016 End of Life arrived on October 14, 2025, the same day Office 2019 lost support, and Office 2021 followed on October 13, 2026. IT teams search for these dates to confirm which Office versions still get security updates and to plan the move to a supported suite before an audit or an incident forces it.

This article lists the End-of-Support dates for every Office version from 2016 to 2024, explains what changes when Office runs without support, and compares the supported alternatives. It closes with a transition plan to find every unsupported install, remove it and roll out the replacement with InvGate Asset Management.

## When did Office 2016, 2019 and 2021 support end?

Office 2016 and Office 2019 reached End of Support on October 14, 2025, for every edition. Both followed the Fixed Lifecycle Policy, so they went through five years of mainstream support with new features and fixes, then an extended phase with security updates only that closed on that date. It is the same day Microsoft ended support for Windows 10, so many teams are handling the Windows 10 End of Life and the Office migration at once.

Office 2021 and Office Long-Term Servicing Channel (LTSC) 2021 reached End of Support on October 13, 2026. In every case, the apps keep opening and documents keep working, but Microsoft no longer ships security patches or bug fixes and offers no technical support for these versions. Office 2019 for Mac had already reached End of Support earlier, on October 10, 2023.

## Office end of life dates by version

The table below shows the lifecycle policy and the support dates of every Office version released since 2016. Office 2024 and Office LTSC 2024 are the only perpetual versions still supported.

| **Version** | **Lifecycle policy**  | **Mainstream support ended** | **End of Support** | 
| Office 2016 | Fixed | October 13, 2020 | October 14, 2025 | 
| Office 2019 | Fixed | October 10, 2023 | October 14, 2025 | 
| Office 2021 | Modern | Not applicable | October 13, 2026 | 
| Office LTSC 2021 | Fixed (no extended phase) | October 13, 2026 | October 13, 2026 | 
| Office 2024 | Modern | Not applicable | October 9, 2029 | 
| Office LTSC 2024 | Fixed (no extended phase) | October 9, 2029 | October 9, 2029 | 


Every date comes from Microsoft's lifecycle pages for Office 2016, Office 2019, Office 2021, Office LTSC 2021, Office 2024 and Office LTSC 2024. Those pages show each end date at 6:59 AM Pacific Time on the following day, while the table uses the dates Microsoft gives in its end-of-support announcements.

## Office 2021 and Office LTSC 2021 end of support

The End of Support covers every edition of Office 2021: Home and Student, Home and Business and Professional, including the Home and Student and Home and Business editions for Mac. It also covers Office LTSC 2021, the volume-licensed version that organizations buy for devices that stay on a fixed feature set, including its Mac edition.

Office 2021 had a shorter life than Office 2016 and 2019 because of how it was licensed. The retail editions follow the Modern Lifecycle Policy, and Office LTSC 2021 follows the Fixed Lifecycle Policy with five years of mainstream support and no extended phase. A perpetual license still runs after the date, but Microsoft's End of support for Office 2021 page confirms there are no further updates and no Extended Security Updates program, so the license now covers an unpatched product.

## What happens when Office is out of support

Unsupported Office stays exposed to every vulnerability discovered after its last patch, and the risk grows with each flaw that goes public. Like any deprecated software, it also becomes a finding in security and compliance audits.

Compatibility erodes too. According to Microsoft's page on Office versions and connectivity to Microsoft 365 services, connecting Office 2016 and Office 2019 to Microsoft 365 services has been unsupported since October 10, 2023, and Office LTSC 2021 was supported only until October 13, 2026. Unsupported versions may still connect, but Microsoft doesn't test its service changes against them, so features and reliability degrade over time.

## Supported alternatives to Office 2016, 2019 and 2021

Replacing an unsupported version is the moment to decide whether the organization stays on perpetual licenses or moves to a subscription. These are the three options most teams evaluate.

### Microsoft 365

Microsoft 365 Apps is the subscription version of Word, Excel, PowerPoint, Outlook and the rest of the Office apps, included in many Microsoft 365 business and enterprise plans. It receives security updates and new features continuously, as often as monthly, so it never reaches a fixed End-of-Support date while you run a supported build.

It also brings OneDrive storage and collaboration through Teams and SharePoint, and it stays supported for connecting to Microsoft 365 services. It fits organizations that want to stop planning Office migrations every few years and can manage a recurring per-user cost.

### Office LTSC 2024

Office LTSC 2024 is the current perpetual, volume-licensed version, sold per device and frozen at the features of its September 2024 release. Microsoft's guidance for upgrading older versions of Office positions it for specific scenarios, such as regulated devices that can't take feature updates, process control equipment and computers without internet access.

Its support ends on October 9, 2029, so choosing it means planning another migration in about three years. For retail licenses, Office 2024, in its Home and Home & Business editions, has the same End-of-Support date.

### Google Workspace

Google Workspace is Google's cloud productivity suite, with Docs, Sheets, Slides, Gmail, Drive and Meet running in the browser. Several people can edit the same file at once, changes save automatically, and Google updates the apps without any installation on the computer.

It opens and edits Word, Excel and PowerPoint files, which makes the switch easier for teams that exchange documents with partners who stay on Microsoft. It fits organizations ready to move to a browser-based way of working, with a training effort for users and for the processes built around Office.

## How to find and replace unsupported Office versions with InvGate Asset Management

InvGate Asset Management is IT Asset Management software that offers an enterprise experience without the complexity or the price of one, with an intuitive, no-code interface and a fast deployment. It builds the inventory from four sources (the Agent, network discovery, integrations and manual or CSV import), and the Agent records every application installed on each computer, so you can find every Office install and act on it from the same place.

The plan below takes you from the list of affected computers to the replacement rolled out, in five steps. It relies on the software inventory the Agent reports and on software deployment, which runs on Windows computers, so the Mac editions of Office 2021 need to be removed outside the platform.

### 1. Identify devices running Office 2016, 2019 or 2021

Start with the inventory. In the Software Explorer, under Software, filter by Microsoft Office and group the view by Market Version: each edition shows its release year and its install count, so you can see at a glance how many installs of Office 2016, 2019 and 2021 are left.

Market Version shows the release year of each install, even though every edition reports a 16.0 build number: 2021 covers both Office 2021 and Office LTSC 2021, and 2024 stays out of the filters because it is still supported. To get the computers behind those installs, go to Assets and open the filters of a new view:

- Add Computer > Software Installations > Software Name > contains > Microsoft Office.
- Add Computer > Software Installations > Market Version > is > 2016.
- Click Update to see every computer with Office 2016, and repeat with 2019 and 2021.

### 2. Create a Smart Tag for each version

A Smart Tag turns each filter into a list that keeps itself up to date. Computers join the tag when the Agent reports that version of Office and leave it once the uninstall is done, so the tag always shows the work that is left.

With the filter for Office 2016 applied, click Quick Smart Tag, next to Update, give the tag a name such as "Office 2016: unsupported", choose a color and click Create. Repeat with the 2019 and 2021 filters to end up with one tag per version.

### 3. Uninstall Office 2016, 2019 and 2021

Software deployment removes Office from every tagged computer without a custom script. You build a package that says what to remove and a plan that says where and when, both under Software > Deployment:

- **Package:** create a package with an action of type Uninstall software, choose the Office edition and its version (for example, Microsoft Office Home and Student, 2016) and keep the silent uninstall parameters it suggests. Add one action for each edition and version you found in step 1.
- **Plan:** create a single execution plan, include the computers by the Smart Tags from step 2 and add the package.
- **Schedule:** choose the days and hours when the plan can run, such as outside working hours.

If your process requires a second person to sign off, turn on approvals for plans and packages and prevent self-approval in Settings > Software Deployment > Security Controls. Each plan or package then needs the approval of a user other than its creator before it runs, and you can follow the progress of each computer from the plan itself.

### 4. Check license availability and plan the replacement

Before rolling out the replacement, confirm you have enough licenses for it. Under Contracts > Software Compliance, each software contract shows its assigned and available licenses, its assigned and total cost, and the licenses with low usage, as long as the contracts are loaded and up to date.

With a Microsoft 365 or Office LTSC 2024 contract loaded, this view shows how many licenses you can assign right away and how many you need to buy. Review the contracts of the versions you are retiring too, so you know which ones to close. Then deploy the new suite with a package of type Install software.

### 5. Keep unsupported Office from coming back

Once the cleanup is done, make sure an old installer doesn't bring Office 2016, 2019 or 2021 back. In Software > Authorization policies, classify those versions as prohibited, so any new install gets flagged as unauthorized as soon as the Agent reports it.

Authorization policies flag the install, and the Smart Tags from step 2 pick the computer up again. If you prefer to remove reinstalls automatically, build an automation with the Uninstall software action, one Office edition at a time.

## Conclusion

Office 2016 End of Life and the End of Support for Office 2019 and 2021 left every remaining install without security patches. The fastest way out starts with knowing where each version runs, removing it in a controlled way and making sure the replacement has the licenses it needs.

InvGate Asset Management brings that whole process into one platform, from the software inventory to the uninstall and the license check. Start a free trial to find your unsupported Office installs, or talk to Sales to plan the migration with your team.

## Frequently Asked Questions

These are the questions IT teams ask most often about the End of Support for Office 2016, 2019 and 2021. Each answer follows the dates on Microsoft's lifecycle pages.

### Can I still use Office 2016 after end of support?

Yes, Office 2016 keeps opening and editing documents after October 14, 2025. It no longer gets security patches or technical support, so every new vulnerability stays open and the risk grows over time.

### Is Office 2019 still supported?

No, Office 2019 reached End of Support on October 14, 2025, the same day as Office 2016. Its connection to Microsoft 365 services has been unsupported since October 10, 2023.

### Is Office 2021 still supported?

No, Office 2021 and Office LTSC 2021 reached End of Support on October 13, 2026. The supported perpetual versions are Office 2024 and Office LTSC 2024, both supported until October 9, 2029.
