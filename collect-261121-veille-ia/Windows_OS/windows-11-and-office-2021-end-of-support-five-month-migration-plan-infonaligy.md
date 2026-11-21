---
id: collect-261121-veille-ia/windows-os/windows-11-and-office-2021-end-of-support-five-month-migration-plan-infonaligy
title: "Windows 11 and Office 2021 End of Support: Five-Month Migration Plan | Infonaligy"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "support", "server", "microsoft", "office", "sharepoint", "migration", "ltsc"]
source: https://www.infonaligy.com/blog/windows-11-office-2021-end-of-support-migration-plan
source_anchor: ""
source_lines: [1, 136]
section: "Windows_OS"
---
Microsoft will stop releasing security patches for Windows 11 Home and Pro version 24H2 on October 13, 2026. Office LTSC 2021 hits the same wall on the same day. Enterprise and Education editions of Windows 11 23H2 follow a month later on November 10. If your business runs any of these, you have roughly five months to plan, budget, test, and deploy replacements before your systems start accumulating unpatched vulnerabilities.

This is not a theoretical risk. Once a product leaves Microsoftâs support lifecycle, every newly discovered vulnerability becomes permanent. There is no patch coming. Attackers know this, and they specifically target end-of-support software because the math favors them. The good news: five months is enough time to do this right if you start now.

## What Is Actually Ending

The mid-to-late 2026 deadlines cover more ground than most business owners realize. Here is the full list of Microsoft products losing support:

**July 14, 2026:**

- SharePoint Server 2016
- SharePoint Server 2019

**October 13, 2026:**

- Windows 11 Home and Pro, version 24H2
- Office LTSC 2021 (Word, Excel, Outlook, PowerPoint, Access)
- Advanced Threat Analytics 1.x

**November 10, 2026:**

- Windows 11 Enterprise and Education, version 23H2
- SQL Server 2016
- Microsoft App-V 5.x

âEnd of supportâ means Microsoft stops providing security updates, bug fixes, and technical support. The software keeps running, but every vulnerability discovered after each productâs end-of-support date stays open. For businesses subject to HIPAA, PCI DSS, CMMC, or cyber insurance requirements, running unsupported software creates an immediate compliance gap. Some cyber insurance policies exclude claims involving systems that have passed end of support.

## Where You Need to Go

Each retiring product has a clear migration target. The paths are well established, and Microsoft has documented the upgrade process for all of them.

**Windows 11 24H2** migrates to Windows 11 version 25H2 or later. This is a feature update, not a full OS reinstall. Most devices that run 24H2 today will accept 25H2 without hardware changes. However, devices older than four years may lack the hardware requirements for future feature updates, so this is a good time to flag machines that are nearing end of useful life.

**Office LTSC 2021** has two paths. The subscription route is Microsoft 365 Apps, which receives continuous updates and includes cloud features like real-time co-authoring and OneDrive integration. The perpetual license route is Office LTSC 2024, which gets five years of support but no feature updates after release.

For most businesses in the 50 to 500 employee range, Microsoft 365 Apps is the stronger choice because it includes security updates delivered automatically, reduces the need for future migration projects, and pairs well with a managed IT strategy. Office LTSC 2024 makes sense primarily for air-gapped or restricted environments where cloud connectivity is not an option.

**SharePoint Server 2016 and 2019** migrate to SharePoint Server Subscription Edition or to SharePoint Online as part of Microsoft 365. If you are still running on-premises SharePoint, this is a natural inflection point to evaluate whether moving to SharePoint Online eliminates the server management overhead entirely.

**SQL Server 2016** migrates to SQL Server 2022 or Azure SQL. SQL Server 2022 runs on-premises and is supported through 2032. Azure SQL is the cloud-managed option that eliminates patching and hardware maintenance. Your choice depends on whether your line-of-business applications support the newer engine and whether your data residency requirements allow cloud hosting.

## Why Waiting Until September Is Expensive

Compressed timelines are the single most common cause of migration failures. When businesses wait until the last month, several things happen at once that make the project harder and more expensive.

Hardware procurement lead times stretch during back-to-school and Q4 ordering cycles. If you discover in August that 40% of your fleet needs replacement, you are competing with every other business that waited, plus the education sector ordering for fall semester. Typical lead times for business-class laptops increase from two weeks to six weeks during this window.

Licensing changes require budget approval. Switching from perpetual Office licenses to Microsoft 365 subscriptions changes your cost from a one-time capital expense to a recurring operational expense. Your CFO needs time to model this and adjust the budget. That conversation is better held in June than in September.

Testing takes longer than expected. Every business has at least one application that behaves unexpectedly after an OS update. VPN clients, security agents, industry-specific software, Excel macros, printer drivers, and legacy browser-dependent apps all need validation. A pilot group of 10 to 15 users should run the new configuration for at least two weeks before broad deployment, and you need time to fix whatever breaks.

Emergency migrations also create security risk. When IT teams rush an OS rollout to meet a deadline, they cut corners on validation, skip pilot testing, and push updates to all devices simultaneously. A bad driver update or a broken line-of-business application across 200 machines simultaneously is worse than the risk you were trying to avoid.

## The Phased Approach: Month by Month

Five months is enough time to migrate methodically. This timeline assumes a business with 50 to 500 endpoints running a mix of Windows 11 24H2 and Office LTSC 2021.

### Month 1 (Late May through June): Inventory and Risk Ranking

Start with a complete picture of what you are running and which systems carry the most risk.

- **Pull a full software inventory** from your endpoint management platform (Intune, ConnectWise, or your MSPâs RMM tool). Identify every device running Windows 11 24H2, Office LTSC 2021, or any other retiring product.
- **Catalog your line-of-business applications.** Build a list of every application your teams depend on, including version numbers. Flag anything that has known compatibility issues with newer Windows or Office versions.
- **Rank systems by risk.** Devices that handle sensitive data (financial records, patient information, CUI) or that face the internet directly should migrate first. Internal-only systems with no sensitive data can wait for later phases.
- **Check hardware age.** Machines manufactured before 2022 may lack TPM 2.0, VBS support, or sufficient RAM for current workloads. Flag these for replacement rather than upgrade.

### Month 2 (July): Budget, Licensing, and Pilot Planning

This is the month where the project shifts from IT to finance and leadership.

- **Finalize licensing decisions.** Microsoft 365 Apps versus Office LTSC 2024. Per-user subscription costs versus one-time perpetual licenses. Factor in the ongoing patching and management cost difference, not just the sticker price.
- **Submit hardware refresh requests.** If your inventory identified devices that need replacement, get purchase orders in now. July orders typically ship before September crunch begins.
- **Define your pilot group.** Select 10 to 15 users across different departments and roles. Include at least one power user from each critical application (accounting, ERP, design software, practice management). These users will test the new configuration in real working conditions.
- **Order licenses** and begin any tenant-level preparation needed for Microsoft 365 migration.

### Month 3 (August): Pilot Deployment and Testing

Deploy the updated configuration to your pilot group and monitor closely for two weeks minimum.

- **Update pilot devices** to Windows 11 25H2 and migrate them from Office LTSC 2021 to Microsoft 365 Apps (or Office LTSC 2024).
- **Test critical workflows.** VPN connections, printing, security agents, line-of-business applications, Excel macros with external data connections, Outlook add-ins, and any browser-dependent internal tools.
- **Document issues and fixes.** Every problem discovered during the pilot is a problem you avoided discovering across your entire fleet. Track driver updates needed, application patches required, and configuration changes that resolved conflicts.
- **Validate security stack compatibility.** Confirm that your EDR agent, DNS filtering, backup agent, and any other security tools function correctly on the updated OS.

### Month 4 (September): Broad Deployment

Roll out to the rest of your fleet in waves, not all at once.

- **Deploy in groups of 20 to 30 devices** , starting with the highest-risk systems identified in Month 1. Wait 48 hours between waves to catch issues before they spread.
- **Apply fixes from the pilot** to each deployment wave. If the pilot required a specific driver update or application patch, build that into the deployment package so every device gets it automatically.
- **Monitor help desk tickets** for patterns. A spike in issues from a specific department or application means something needs attention before the next wave ships.
- **Handle exceptions.** Some devices will fail to update cleanly. Budget time for hands-on remediation of machines with unusual configurations, heavily customized software, or hardware limitations.

### Month 5 (Early October): Cleanup and Verification

Cross the finish line before October 13.

- **Run a final inventory scan** to confirm no devices are still running Windows 11 24H2 or Office LTSC 2021.
- **Address stragglers.** There will be devices that were missed, offline, or in a failed update state. Identify and remediate them individually.
- **Update compliance documentation.** If your business carries cyber insurance or operates under regulatory requirements, document the completed migration for audit purposes.
- **Decommission retiring servers.** If you migrated off SharePoint Server 2016/2019 or SQL Server 2016, remove those servers from your environment entirely. Leaving decommissioned servers running creates shadow IT risk.

## Budget Considerations for CFOs

Migration projects have costs beyond licensing. A realistic budget accounts for all of them.

**Licensing costs** vary based on your path. Microsoft 365 Apps costs between $12 and $22 per user per month at current list pricing, depending on the plan tier. Office LTSC 2024 is a one-time purchase per device. Windows feature updates are included with your existing Windows license at no additional cost.

**Hardware replacement** is the biggest variable. If 20% of a 200-device fleet needs replacement, that is 40 machines at $1,000 to $1,500 each for business-class laptops. Budget $40,000 to $60,000 for hardware alone in that scenario. Get quotes early to lock in pricing.

**Labor and project management** covers the IT hours needed to plan, test, deploy, and troubleshoot. If you manage IT internally, this competes with your teamâs existing workload. If you work with a managed IT provider, the migration planning and execution typically falls within your existing service agreement, though hardware procurement may be billed separately.

**Training time** is often overlooked. If employees move from Office 2021 to Microsoft 365 Apps, the core applications look similar but cloud features like co-authoring, OneDrive sync, and Teams integration work differently. Budget a 30-minute orientation session per department at minimum.

**Downtime costs** are the ones you control by not rushing. A phased migration with proper testing produces near-zero unplanned downtime. A last-minute all-at-once push does not.

## What Happens If You Do Nothing

Running unsupported software after October 13 creates compounding problems.

Every vulnerability discovered in Windows 11 or Office after that date stays open on your systems permanently. Microsoft publishes vulnerability details in its Security Update Guide monthly. Attackers monitor these releases and reverse-engineer patches to build exploits for the exact vulnerabilities that were fixed in supported versions. Your unsupported systems become a published target list.

Compliance frameworks notice. HIPAAâs Security Rule requires reasonable safeguards, and running software with known, unpatched vulnerabilities fails that test. PCI DSS Requirement 6.3.3 specifically requires patching critical vulnerabilities within 30 days. CMMC requires patching within defined timelines. You cannot meet these requirements on unsupported software.

Cyber insurance policies are tightening their exclusions around end-of-support software. Some carriers include questionnaire items about supported operating systems and office suites during underwriting. A claim filed while running Windows 11 24H2 after October 13 could face additional scrutiny.

### Need Help Planning Your Migration?

Our team can inventory your environment, build a phased migration plan, and handle the deployment so your team stays focused on their work.

Get a Free Assessment
The first step is knowing exactly what you are running. Every machine, every version, every application dependency. That inventory drives every decision that follows: what needs to move, what hardware needs replacing, and how much the project will cost.

If you manage IT internally, pull reports from your endpoint management tool this week. If you work with an MSP, ask them for a current software inventory broken down by OS version and Office version. That single report will tell you the scope of what is ahead and whether your current timeline and budget are realistic.

Five months is enough time to do this well. It is not enough time to wait three months and then do it in a rush.
