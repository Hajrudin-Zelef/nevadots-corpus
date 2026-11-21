---
id: collect-261121-veille-ia/datacenters-infra/ukraine-strikes-yandex-s-third-data-center-in-four-days
title: "Ukraine Strikes Yandex's Third Data Center in Four Days"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: ["2026-11-15"]
keywords: ["data", "center", "october", "sasovo", "yandex", "vladimir", "yandexs", "kaluga"]
source: https://tech-insider.org/ukraine-strikes-yandex-third-data-center-4-days-2026/
source_anchor: ""
source_lines: [1, 57]
section: "Datacenters_Infra"
---
Ukrainian drones halted operations at Yandex’s data center in the Vladimir region on October 11, the third of the company’s facilities hit in four days after Sasovo on October 8 and Kaluga on October 9, according to United24’s account of the sequence and the Boston Globe. Yandex Cloud said the Vladimir site’s infrastructure was damaged and “Operations at the data center have been completely halted. There were no injuries,” per the Boston Globe, and users in dozens of Russian cities and in Kazakhstan, Belarus and Armenia could not order taxis or reach banking services, according to the independent outlet Astra. On November 10, Yandex Cloud said it had restored 97.16% of the affected customer data and that the remaining 2.84% is permanently lost, as reported by the Moscow Times and Data Center Dynamics.

For a customer, what has changed since the first reports of the Vladimir outage is the data. The lost 2.84% fell on customers without backups, and United24 reports that Yandex Cloud will enable automatic backups by default whenever customers create new virtual machines, with users able to disable the feature manually through the management console.

## Sasovo on October 8, Kaluga on October 9, Vladimir on October 11: three of Yandex’s five large sites

United24 dates the first attack to the night of October 8, when Ukrainian drones targeted the Sasovo data center in Russia’s Ryazan region. The Boston Globe describes the same run: a drone attack took the Ryazan site offline on October 8, Ukrainian drones hit the major Kaluga data center a day later, and the Vladimir site was shut down on October 11.

| Date | Site and region | Reported impact | Capacity or kit | Source | 
|---|---|---|---|---|
| October 8 (overnight) | Sasovo, Ryazan region | Fire, part of the infrastructure affected, no casualties; data center stopped operating entirely (per Yandex); first of the three strikes | Houses two of Yandex’s three supercomputers, Chervonenkis and Lyapunov; constructed in 2012 per Yandex’s ESG report, checked November 15, 2026 (The Insider dates the opening to 2014); designed to house tens of thousands of servers (The Insider) | United24, Boston Globe, Data Center Dynamics | 
| October 9 | Kaluga region | Part of the infrastructure damaged, several modules completely knocked out (a partial failure) | Yandex’s largest; designed capacity 63 MW, 130,000 square metres | Data Center Dynamics, Militarnyi, Boston Globe | 
| October 11 | Vladimir region | Infrastructure damaged, operations completely halted, no injuries; fire overnight, per Yandex’s press service | Designed capacity about 56 MW; houses the Galushkin supercomputer, 20.64 petaflops peak | Boston Globe, Militarnyi, United24 | 

Data Center Dynamics lists Yandex’s five large data centers as Vladimir, Sasovo, Ivanteevka, Mytishchi and Kaluga, so the strikes reached three of the five. The same report describes the Sasovo strike as “causing a fire and bringing operations down at the facility.” Yandex said, according to that report, that the fire affected part of the Sasovo infrastructure, that there were no casualties, and that the data center stopped operating entirely and some services might be unavailable. Yandex’s own ESG report, checked on November 15, 2026, lists the Sasovo data center as “Constructed in 2012.” The Insider gives 2014 as the year Sasovo opened, a date that differs from the ESG report, and adds that it is designed to house tens of thousands of servers. For Kaluga, the Data Center Dynamics report says a drone attack damaged part of the infrastructure, “completely knocking out several modules”, and Yandex said on October 9 that part of the Kaluga infrastructure was affected and several modules were out of service. That makes Kaluga a partial failure, where Sasovo and Vladimir were halted. The Vladimir halt, the absence of injuries and the overnight fire come from Yandex itself (the Cloud statement and its press service, as relayed by the Boston Globe and Militarnyi). The 56 MW and 63 MW capacities, and the description of Kaluga as Yandex’s largest site, are Militarnyi’s figures and are designed capacity, not output or load.

## Taxis and banking in dozens of cities and three other countries: what one halted site reached

Astra, cited by the Boston Globe, put the Vladimir data center outage across dozens of Russian cities and into Kazakhstan, Belarus and Armenia, where users could not order taxis or access banking. The Moscow Times describes the wider run of strikes as disrupting ride-hailing apps, car-sharing, food delivery and online maps, as well as email, banking and streaming platforms. Militarnyi reports that the Vladimir site supports Yandex Cloud, the platform on which companies rent server capacity, and that its infrastructure may also support Yandex’s own services, including search.

Yandex’s role explains why one site matters this much. The Boston Globe describes it as often called Russia’s Google, running the country’s most popular search engine and heavily involved in its AI development. We think a single halted site that produced outages in four countries is evidence of a dependency worth mapping, for a team in Kazakhstan, Belarus or Armenia as much as in Russia, whichever layer of Yandex’s stack failed.

## All three of Yandex’s supercomputers sat at struck sites, and Yandex says those at two sites survived

United24 places two of Yandex’s three supercomputers, Chervonenkis and Lyapunov, at Sasovo, and the third, Galushkin, at Vladimir, where its maximum computing capacity is 20.64 petaflops. The mallory.ai account describes the Sasovo pair as two Nvidia A100-based supercomputers used for AI development and says Yandex had not disclosed whether they were damaged. By the Moscow Times’s account on November 10, Yandex had earlier said its specialized supercomputers at two of the targeted facilities survived the attacks unharmed and were transferred to secure alternative sites. Sasovo and Vladimir are the two sites that hold supercomputers, and SOFX, relaying Meduza, says Yandex relocated all three machines and said they were not damaged.

Surviving hardware and a working site are separate things. RFE/RL’s Current Time service reported that the strikes on the three data centers will reportedly take months to repair. That estimate is RFE/RL’s report, not a Yandex commitment.

## 97.16% of affected customer data restored, 2.84% lost: the split follows backups

According to Data Center Dynamics, the remaining 2.84% “cannot be recovered due to critical damage to equipment and the absence of backups on the platform by customers.” The Moscow Times adds that Yandex said cloud customers who had enabled automated backup features experienced zero data loss. Those two statements together say the loss landed on customers who had no backups, so we think the 2.84% measures configuration as much as destruction. It is also a share of the affected data, not of everything Yandex Cloud stores, so the percentage alone does not translate into gigabytes or customer numbers.

United24 reports the restoration figures and two changes Yandex Cloud announced alongside them, and each leaves a gap.

| Change | Who it covers | What it leaves open | 
|---|---|---|
| 97.16% of affected data restored, announced November 10 | Customers whose data was affected | The figure is Yandex’s own, expressed as a share of affected data; volumes and customer counts have to be checked against your own records | 
| Automatic backups on by default | New virtual machines; users can switch the feature off in the management console | United24 quotes the default as applying “whenever customers create new virtual machines,” and says users can disable it manually through the management console, so confirm the setting on each machine you run | 
| New availability zone ru-central1-f, which “operates from a separate location” | Customers who choose to place workloads there | Distance from, and power and network independence of, the three struck regions are what to ask Yandex Cloud for | 

The new zone’s value depends on where it sits. Sasovo in the Ryazan region, Kaluga and Vladimir are three different regions, and all three were hit within four days, so a zone only helps if it does not share failure modes with them. United24 quotes Yandex Cloud saying the zone “operates from a separate location,” and mallory.ai describes the zone as being at an undisclosed location with a limited range of services at first. Distance and shared power or network are the points to put to Yandex Cloud directly.

## A 10% reserve for the state, underground data centers and a look at Kazakhstan: what has changed since the strikes

The one confirmed change is a decree. SOFX reports that on November 2 President Putin signed a decree requiring data center operators to reserve 10% of their available space and computing capacity for potential government use, which authorities can order made available within 24 hours. It applies to operators, so customers of a Russian data center could be affected. We infer that a call on the reserve would take capacity commercial customers would otherwise use; how that would work in practice is a question for your operator.

The other two responses are weaker evidence. SOFX reports that state-owned Rostelecom is discussing plans with the mining company Norilsk Nickel to build underground data centers, which is talks, not a project. United24 reports that Russian businesses are showing particularly strong interest in data center facilities in Kazakhstan, and that two other industry sources confirmed some companies had already begun moving operations to foreign facilities. That is reporting from industry sources, not a measured shift, and it matters for Kazakh teams because it would put more Russian-origin workloads next to their own.

## What Zelenskyy said about data centers

SOFX quotes President Zelenskyy: “We always respond in kind. As for data centers, you all know, they have been and continue to attack our data centers. We’ve responded.” SOFX presents this as retaliation for Russian attacks on Ukrainian data centers. In that quote he speaks of data centers in general. The official president.gov.ua page, checked on November 14, 2026, is explicit: it says the Head of State “emphasized that after Ukrainian strikes on Yandex’s data centers, the number of bots spreading hatred and provocations decreased on many social media platforms.” That is the president’s office’s own account of his remark, and it attributes the strikes on Yandex’s data centers to Ukraine.

## Three checks for a team that runs on one cloud operator

1. Confirm automated backups on every virtual machine you run, since customers with backups lost no data; United24 describes Yandex’s new default as applying whenever customers create new virtual machines.
2. If you plan to use ru-central1-f, ask Yandex Cloud in writing for its distance from, and its power and network independence of, the Sasovo, Kaluga and Vladimir sites.
3. Map which services your own users reach through Yandex, including banking and taxi dependencies in Kazakhstan, Belarus and Armenia, and decide what you would do if the state’s 10% reserve were called on within 24 hours.
