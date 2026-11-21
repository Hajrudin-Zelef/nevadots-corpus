---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-office-and-windows-configuration-support-microsoft-lifecycle
title: "Microsoft Office and Windows configuration support - Microsoft Lifecycle"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "microsoft", "support", "office", "lifecycle", "server", "apps", "configuration"]
source: https://learn.microsoft.com/is-is/lifecycle/office-windows-configuration-matrix
source_anchor: ""
source_lines: [1, 62]
section: "Copilot_Microsoft"
---
Please go here to search for your product's lifecycle.

## Full configuration support end dates

Support for Microsoft 365 Apps is defined by the Modern Lifecycle Policy,  which requires customers to maintain an up-to-date configuration in order to stay in support. Support for non-subscription versions of Office is defined by the Fixed Lifecycle Policy, which offers a fixed term of support regardless of configuration. However, the nature and extent of support available for in-support, non-subscription versions of Office running on out-of-support versions of Windows will depend on the nature of the specific issue. Learn more at: Microsoft Lifecycle Policy FAQ.â

|  |  |  |  |  |  | 
|  | **Microsoft 365 Apps** No end date | **Office LTSC 2024** EOS Oct 2029 | **Office LTSC 2021** EOS Oct 2026 | **Office 2019** EOS Oct 2025 | **Office 2016** EOS Oct 2025 | 
| **Windows 11** In support <sup>1</sup> | No end date <sup>1</sup> | Oct 2029 | Oct 2026 | Oct 2025 | Oct 2025 <sup>2</sup> | 
| **Windows 10** EOS Oct 2025 | Oct 2025 <sup>6</sup> | Oct 2025 | Oct 2025 | Oct 2025 | Oct 2025 | 
| **Windows 8.1** EOS Jan 2023 | Jan 2023 <sup>3</sup> | n/a | n/a | n/a | Jan 2023 <sup>4</sup> | 
| **Windows Server 2025** EOS Oct 2034 | Oct 2029 <sup>5</sup> | Oct 2029 | Oct 2026 | Oct 2025 | n/a | 
| **Windows Server 2022** EOS Oct 2031 | Oct 2026 <sup>6</sup> | Oct 2029 | Oct 2026 | Oct 2025 | n/a | 
| **Windows Server 2019** EOS Jan 2029 | Oct 2025 <sup>6</sup> | n/a | Oct 2026 | Oct 2025 | n/a | 
| **Windows Server 2016** EOS Jan 2027 | Oct 2025 <sup>6</sup> | n/a | n/a | n/a | Oct 2025 | 
| **Windows 11 LTSC 2024** EOS Oct 2029 | n/a | Oct 2029 | Oct 2026 | Oct 2025 | n/a | 
| **Windows 10 LTSC 2021** EOS Jan 2027 | n/a | Jan 2027 | Oct 2026 | Oct 2025 | n/a | 
| **Windows 10 LTSC 2019** EOS Jan 2029 | n/a | Jan 2029 | Oct 2026 | Oct 2025 | n/a | 
| **Windows 10 LTSB 2016** EOS Oct 2026 | Jan 2020 | n/a | n/a | n/a | Oct 2025 | 
| **Windows 10 LTSB 2015** EOS Oct 2025 | Jan 2020 | n/a | n/a | n/a | Oct 2025 | 

<sup>1</sup> Details about Windows 11 lifecycle and servicing updates are available on the Lifecycle page.

<sup>2</sup> Historically, new versions of Windows have supported Office products still in mainstream support. Although Office 2016 is in extended support, weâre making an exception to maximize customer access to Windows 11.

<sup>3</sup> Security updates for Microsoft 365 Apps running on Windows 8.1 ended in January 2023. Learn more: Windows 8.1 end of support and Microsoft 365 Apps.

<sup>4</sup> Volume licensed versions of Office 2016 clients running on Windows 8.1 will continue to get monthly updates through the end of the Office clientâs lifecycle.

<sup>5</sup> Microsoft 365 Apps on Windows Server 2025 is supported during the Windows Server mainstream support period. Learn more: Windows Server end of support and Microsoft 365 Apps.

<sup>6</sup> In the interest of maintaining security while customers complete their migrations to a supported configuration, Microsoft will continue providing security updates for Microsoft 365 desktop apps running on Windows 10, Windows Server 2016, Windows Server 2019, and Windows Server 2022 until October 10, 2028. Information on the timeline for feature updates is available in the following articles:

## Connectivity to Microsoft 365 cloud services

Office apps connecting to the Microsoft 365 services (e.g., Exchange Online, SharePoint Online, OneDrive) must be in mainstream support to be considered a supported connection. Learn more: Office versions and connectivity to Microsoft 365 services.

|  |  | 
|  | **Microsoft 365 Services** No end date | 
| **Microsoft 365 Apps** No end date | No end date | 
| **Office LTSC 2024** EOS Oct 2029 End of mainstream support Oct 2029 | Oct 2029 | 
| **Office LTSC 2021** EOS Oct 2026 End of mainstream support Oct 2026 | Oct 2026 | 
| **Office 2019** EOS Oct 2025 End of mainstream support Oct 2023 | Oct 2023 | 
| **Office 2016** EOS Oct 2025 End of mainstream support Oct 2020 | Oct 2023 | 

### Example on using the above tables

I am running Office 2016 on Windows Server 2016. Reviewing the "Full Configuration Support" table, along the top I see that Office 2016 will reach end of support (EOS) in October 2025. From the leftmost column in that table, I find that Windows Server 2016 will reach end of support in January 2027. Where that row and column intersect, I find that my configuration will continue to be supported on Windows Server 2016 until October 2025, when Office 2016 goes out of support. From the "Service Connectivity" table, I canâ¯see that Office 2016 is no longer supported in connecting to Microsoft 365 services as of October 2023.

### Windows 365

As an online service managed by the Modern Lifecycle Policy, Windows 365 does not have an end of support date. Both Windows 11 and Windows 10 are supported on the Windows 365 service, and the product timelines shown here apply.

### Customer service and support limitations for incidents involving Microsoft 365 Apps or non-subscription Office running on an unsupported configuration

Customers with a valid Microsoft 365 subscription or a license for an in-support non-subscription version of Office will continue to be able to open support cases with Microsoft. However, the following limitations to customer service and support will apply to incidents involving Microsoft 365 Apps or in-support versions of Office running in an unsupported configuration:

1. If the issue occurs only with Microsoft 365 Apps or in-support versions of Office in the unsupported configuration and does not occur when running on a more up-to-date operating system, support will ask the customer to move to the more up-to-date operating system.
2. If the customer is unable to move to a more up-to-date operating system, support will provide troubleshooting assistance only; technical workarounds might be limited or unavailable.
3. Support incidents for Microsoft 365 Apps or in-support versions of Office running on an unsupported configuration do not include the option to log a bug or request other product updates.

For individuals, see Customer Service and Support for more information.
