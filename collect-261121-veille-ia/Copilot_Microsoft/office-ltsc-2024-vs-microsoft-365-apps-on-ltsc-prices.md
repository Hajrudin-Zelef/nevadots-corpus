---
id: collect-261121-veille-ia/copilot-microsoft/office-ltsc-2024-vs-microsoft-365-apps-on-ltsc-prices
title: "Office LTSC 2024 vs Microsoft 365 Apps on LTSC: Prices"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["ltsc", "office", "device", "microsoft", "windows", "enterprise", "apps", "user"]
source: https://o365hq.com/blog/office-ltsc-2024-vs-microsoft-365-apps-on-windows-ltsc-devices-what-runs-where-and-what-it-costs
source_anchor: ""
source_lines: [1, 113]
section: "Copilot_Microsoft"
---
The device answer first: an office PC on Windows 11 Pro or Enterprise runs Microsoft 365 Apps under the user's subscription. A device on Windows 10 Enterprise LTSC 2021, Windows 11 Enterprise LTSC 2024 or an IoT Enterprise LTSC release runs Office LTSC 2024 or a single LTSC application, because Microsoft's Office and Windows configuration matrix lists Microsoft 365 Apps as not supported on Windows LTSC at the time of writing. On the October 2026 price list, Office LTSC Standard 2024 is $495 per device, Professional Plus $675, and Word, Excel, Outlook and Access LTSC 2024 $170 each, perpetual, Microsoft list price. Office LTSC 2021 reached end of support on 13 October 2026 with no extended phase and no ESU; Office LTSC 2024 runs to 9 October 2029 at the time of writing. Here is the table, then the prices, the rules and a plan in progress.

## Which Office goes on which device, in one table

Two questions sort the rows: which Windows servicing channel the device is on, and whether a licensed user signs in to it. Every price is Microsoft list price, October 2026 price list, Commercial segment, one-time and perpetual.

| Device | Windows edition | Office to install | License | Why | 
|---|---|---|---|---|
| Office PC with a signed-in user | Windows 11 Pro or Enterprise, General Availability Channel | Microsoft 365 Apps | Per user, through Business Premium, Business Standard, Apps for business, Apps for enterprise, E3 or E5 | The supported configuration; the apps update on the user's channel | 
| Kiosk, laboratory instrument PC, medical device | Windows 11 IoT Enterprise LTSC 2024, Windows 10 IoT Enterprise LTSC 2021 or Enterprise LTSC | Office LTSC Standard 2024, or one application such as Excel LTSC 2024 | Per device, perpetual: $495 for Standard, $170 per single application | Microsoft 365 Apps is not supported on Windows LTSC; the build is frozen, so the suite is too | 
| Shared device with no user sign-in (reception, warehouse, ward) | Windows 11 Pro or Enterprise if general purpose; LTSC if fixed-function | Microsoft 365 Apps with shared computer activation when users sign in with a qualifying plan; otherwise Office LTSC 2024 | Per user with shared computer activation, or per device | A user subscription needs a user to sign in; a device nobody signs in to needs a device license | 
| Device the vendor froze on an old Windows build | Whatever the vendor certified, usually an LTSC release | Office LTSC 2024 where the vendor permits an Office change; otherwise a written risk acceptance | Per device, perpetual | Office LTSC 2021 has been unsupported since 13 October 2026; the matrix sets which Office is supported on each LTSC build | 

Microsoft's license charges are billed to you by Microsoft and are never a fee of ours. The companion article on Windows Enterprise LTSC, N LTSC and IoT Enterprise LTSC settles the Windows row; this one settles the Office row.

## What Office LTSC 2024 is, what it leaves out, and what it costs

Office LTSC 2024 is Microsoft's current perpetual, on-premises Office release. You buy it once, it is licensed per device, and it receives security and quality updates but no new features after release. Microsoft's overview describes five years of mainstream support with no extended support afterward, which puts the end at 9 October 2029 at the time of writing; Project 2024 and Visio LTSC 2024 follow the same policy.

What is in the box, per Microsoft's product descriptions on the October 2026 price list: Office LTSC Standard 2024 contains Word, Excel, PowerPoint and Outlook plus the OneNote desktop app; Office LTSC Professional Plus 2024 adds Access. Teams is not preinstalled and is a separate download. Publisher is not included, because Publisher leaves support in October 2026.

| Product | List price, October 2026 | Contains | 
|---|---|---|
| Office LTSC Standard 2024 | $495 per device | Word, Excel, PowerPoint, Outlook, OneNote desktop app | 
| Office LTSC Professional Plus 2024 | $675 per device | Standard plus Access | 
| Word, Excel, PowerPoint, Outlook or Access LTSC 2024 | $170 per device, each | One application | 
| Visio LTSC Standard 2024 | $310 per device | Diagramming, business and network stencils | 
| Visio LTSC Professional 2024 | $580 per device | Visio Standard plus the professional shapes and templates | 
| Office LTSC Standard for Mac 2024 | $495 per device | Word, Excel, PowerPoint, OneNote, Outlook for Mac | 

A kiosk that only opens a workbook takes Excel LTSC 2024 at $170; two single applications are $340; from the third, Standard at $495 is the cheaper and fuller buy.

## Why Microsoft 365 Apps and Windows LTSC do not mix

Microsoft 365 Apps is the subscription version of the desktop apps: licensed per user, activated by that user signing in, updated on Current Channel, Monthly Enterprise Channel or Semi-Annual Enterprise Channel. Each channel assumes a Windows release that also moves, which is what the General Availability Channel is.

The Long-Term Servicing Channel is the opposite design: one Windows build, kept for five or ten years, with security fixes only. Microsoft's Office and Windows configuration matrix, at the time of writing, lists Microsoft 365 Apps as not supported on Windows 10 Enterprise LTSC 2021 or Windows 11 Enterprise LTSC 2024. The supported Office on those builds is Office LTSC 2024, through January 2027 on LTSC 2021, when that Windows release ends, and through October 2029 on LTSC 2024. The installer may run; what you do not have is a configuration Microsoft will take a support case on.

The license adds a second reason. A kiosk or an instrument PC usually has no user signing in, and a user subscription needs a user. Shared computer activation covers the opposite case, many licensed users on one general-purpose PC. A device nobody signs in to needs a device license, and the device-licensed Office is Office LTSC 2024.

## Office LTSC 2021 ended on 13 October 2026: what that means and the move to 2024

Office LTSC 2021, Office 2021, Visio LTSC 2021 and Project LTSC 2021 stopped receiving security updates, bug fixes and technical support on 13 October 2026. There is no extended support phase and no Extended Security Updates program, so unlike Windows 10 there is no paid bridge to buy. Our Microsoft deadlines page carries the entry with Microsoft's announcement.

What it means on a device: every Word, Excel and Outlook vulnerability published from October 2026 onward stays open on that machine. For a device that opens outside files or receives mail, that is an attack surface with no fix coming; for an air-gapped lab PC it is a risk-acceptance decision with a named owner, and an acquirer's or insurer's "supported software on every device" row usually makes it for you.

The move to Office LTSC 2024 is the same shape as the product it replaces: a device-based perpetual license, installed with the Office Deployment Tool from a configuration file, activated against your KMS host or with a Multiple Activation Key, and updated from the Office content delivery network or a share you control. One configuration file removes 2021 and installs 2024 in a single pass. What does not carry over is the license: an Office LTSC 2021 license without active Software Assurance does not become a 2024 license, and each device on the list needs a 2024 purchase.

## The licensing rules in plain words

Office LTSC 2024 is licensed per device, perpetually: one license per device, any user of that device may use it, and it is sold through Volume Licensing (Open Value, MPSA, Enterprise Agreement, Select Plus) and as CSP perpetual software at the same October 2026 Commercial list prices.

Software Assurance is optional and available only through the volume-licensing programs. With it, a 2021 license moves to 2024 at no additional charge from Microsoft under new-version rights; without it, every LTSC release is a new purchase, which is where most 2021 estates stand today.

A Microsoft 365 Apps subscription does not downgrade to Office LTSC. The subscription is a per-user right to the subscription apps; Office LTSC is a per-device perpetual product, and the two are separate purchases. The result is the mix most organizations end up with: a user plan for every person with an office PC, and a short list of device licenses for the LTSC machines, each with a written reason.

The Mac has its own edition, Office LTSC Standard for Mac 2024, $495 per device, perpetual, through volume licensing. What a license reviewer asks for: the volume-licensing or CSP perpetual record, the device list it covers, the activation method, and confirmation that no user subscription is being counted as the license for a device nobody signs in to.

## A worked example: Business Premium on the office PCs, Office LTSC Standard 2024 on the LTSC devices

A medical practice of about 200 users and about 250 Windows devices is being acquired by a hospital group, with a possession date about four weeks out. The practice runs Microsoft 365 Business Premium with hybrid identity through Entra Connect, so every office PC runs Microsoft 365 Apps under the user's license on Windows 11 Pro in the General Availability Channel. Those machines need nothing from this article.

A handful of devices do. Three instrument PCs in the laboratory run a vendor application certified on Windows 10 Enterprise LTSC 2021 and open the vendor's exports in Excel and print a Word report; a digital-signage player is being rebuilt on Windows 10 IoT Enterprise LTSC 2021 to replace a Windows Server 2016 virtual machine. All four carried Office LTSC 2021, unsupported since 13 October 2026, and the acquirer's checklist requires supported software on every device.

The plan runs through the table. The signage player needs no Office, so it gets none. Two instrument PCs use Word and Excel and are headed for Office LTSC Standard 2024 at $495 per device; the third only opens a workbook and takes Excel LTSC 2024 at $170. Microsoft 365 Apps is excluded because the matrix does not support it on LTSC 2021 and no user signs in. The licenses are ordered as CSP perpetual software through the practice's existing provider, one Office Deployment Tool configuration removes 2021 and installs 2024, activation goes to the existing KMS host, and the three devices are tagged device-licensed in the inventory the acquirer's team reads. Because Windows 10 LTSC 2021 itself ends on 12 January 2027, the same record carries the Windows decision for each device, gated on the vendor's certification.

## What it costs on our side

Microsoft's charges, the Office LTSC licenses and the subscriptions, are billed to you by Microsoft and are never a fee of ours. Our work is quoted in writing before it starts.

- Office LTSC licenses: the quote button on each software page prices the product for your device count: Office LTSC Standard 2024 for a device that needs the suite, Office LTSC Professional Plus 2024 where Access is required, and Excel LTSC 2024 or Word LTSC 2024 for a device that needs one application.
- The estate that is moving: Windows 11 Migration and Windows 10 ESU Transition, $25 per device plus a $1,950 tenant fee, four weeks: the per-device assessment that separates the General Availability Channel PCs from the LTSC devices, the Intune-driven upgrades, the edition set in the image, and the Office configuration for each device class.

## Frequently asked questions

### Can we install Microsoft 365 Apps on a Windows 11 LTSC device?

Not in a supported configuration. Microsoft's Office and Windows configuration matrix lists Microsoft 365 Apps as not supported on Windows 10 Enterprise LTSC 2021 or Windows 11 Enterprise LTSC 2024 at the time of writing. The supported Office on those builds is Office LTSC 2024, licensed per device.

### What does Office LTSC Standard 2024 cost and what is in it?

$495 per device, perpetual, Microsoft list price, October 2026 price list. It contains Word, Excel, PowerPoint and Outlook plus the OneNote desktop app. Professional Plus, at $675, adds Access. Teams is a separate download and Publisher is not included.

### Office LTSC 2021 support ended in October 2026; what now?

Office LTSC 2021, Office 2021, Visio LTSC 2021 and Project LTSC 2021 stopped receiving security updates on 13 October 2026, with no extended phase and no ESU. Office PCs move to Microsoft 365 Apps under a user plan; Windows LTSC devices move to Office LTSC 2024 with a new per-device license.

### Can we buy just Excel or Word LTSC for a kiosk or a lab PC?

Yes. Word, Excel, PowerPoint, Outlook and Access LTSC 2024 are sold as single applications at $170 per device each on the October 2026 price list, perpetual. Two applications cost $340; from the third, Office LTSC Standard 2024 at $495 is the cheaper and fuller answer.

### How long is Office LTSC 2024 supported, and is there an extended phase?

Microsoft gives Office LTSC 2024, Project 2024 and Visio LTSC 2024 five years of mainstream support with no extended support afterward, which ends on 9 October 2029 at the time of writing.

## Sources

- Microsoft Learn, "Overview of Office LTSC 2024" (office/ltsc/2024/overview), support policy, Teams, Publisher and OneNote, retrieved October 2026
- Microsoft Learn, "Microsoft Office and Windows configuration support" (lifecycle/office-windows-configuration-matrix), Microsoft 365 Apps and Office LTSC 2024 support on Windows LTSC editions, retrieved October 2026
- Microsoft Learn, "Office LTSC 2021 reaching end of support on October 13, 2026" (lifecycle/announcements/office-ltsc-2021-end-of-support), retrieved October 2026
- Microsoft Learn, "Overview of the Office Deployment Tool" (microsoft-365-apps/deploy/overview-office-deployment-tool) and "Overview of shared computer activation for Microsoft 365 Apps" (microsoft-365-apps/deploy/overview-shared-computer-activation), retrieved October 2026
- Microsoft Learn, "Overview of update channels for Microsoft 365 Apps" (microsoft-365-apps/updates/overview-update-channels), retrieved October 2026
- Microsoft Learn, "Windows Enterprise LTSC overview" (windows/whats-new/ltsc/overview) and "Windows 10 Enterprise LTSC 2021" (lifecycle/products/windows-10-enterprise-ltsc-2021), retrieved October 2026
- IT Partner Microsoft deadlines page; Microsoft NCE Software price list (US, Commercial), October 2026: Office LTSC Standard 2024, Office LTSC Professional Plus 2024, Word LTSC 2024, Excel LTSC 2024, PowerPoint LTSC 2024, Outlook LTSC 2024, Access LTSC 2024, Visio LTSC Standard 2024, Visio LTSC Professional 2024, Project Standard 2024, Project Professional 2024 and Office LTSC Standard for Mac 2024

## Key takeaways

- Office PCs on Windows 11 Pro or Enterprise run Microsoft 365 Apps under a user plan; Windows LTSC devices run Office LTSC 2024, because Microsoft's configuration matrix lists Microsoft 365 Apps as not supported on Windows LTSC at the time of writing.
- October 2026 list prices: Office LTSC Standard 2024 $495 per device, Professional Plus $675, Word, Excel, PowerPoint, Outlook and Access LTSC 2024 $170 each, perpetual; from the third single application, Standard is the cheaper buy.
- Office LTSC 2021 ended on 13 October 2026 with no extended phase and no ESU; Office LTSC 2024 runs to 9 October 2029 at the time of writing, and a 2021 license without Software Assurance does not become a 2024 license.
- Office LTSC 2024 has no Teams preinstalled and no Publisher; it is device-based, sold through Volume Licensing or CSP perpetual, and a Microsoft 365 Apps subscription does not downgrade to it.

**Know which row each device is on?** Open Office LTSC Standard 2024 and use the quote button for the devices on Windows LTSC, or the single-application pages where one app is enough. For the rest of the fleet, Windows 11 Migration and Windows 10 ESU Transition, $25 per device plus a $1,950 tenant fee, four weeks, separates the General Availability Channel PCs from the LTSC devices and sets the Office configuration for each. Microsoft's license charges are billed to you; our work is quoted in writing before it starts.
