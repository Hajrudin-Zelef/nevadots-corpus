---
id: collect-261121-veille-ia/apple-ios/apple-addresses-multiple-webkit-vulnerabilities-computer-weekly
title: "Apple addresses multiple WebKit vulnerabilities | Computer Weekly"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["apple", "webkit", "security", "vulnerabilities", "flaws", "multiple", "browser", "been"]
source: https://www.computerweekly.com/news/366649440/Apple-addresses-multiple-WebKit-vulnerabilities
source_anchor: ""
source_lines: [1, 35]
section: "Apple_iOS"
---
Getty Images/Caiaimage

Apple has addressed close to 20 vulnerabilities in the open source WebKit browser engine that underpins its Safari browser, which are present in its desktop and notebook, and mobile operating systems.

The updates, which take Safari to version 26.6.1 in MacOS Sonoma and MacOS Sequoia, MacOS Tahoe to version 26.6.2, and iOS and iPad OS to versions 18.7.10 and 26.6.1, respectively, were all released over the past couple of days.

In common with most other software suppliers, the updates mark a significant uptick in the volume of issues contained in Apple’s security fixes, and according to Cupertino, nine of them are attributed to a researcher using OpenAI Codex Security – a research preview that connects to GitHub to help teams identify coding flaws – a clear demonstration of how artificial intelligence (AI) is upending the world of vulnerability discovery.

Left alone, the issues may lead to multiple unpleasant outcomes, including browser and process termination, memory corruption, and crashes. In one instance, a flaw tracked as CVE-2026-64778 in WebKit History may cause a user lured to a maliciously crafted website to inadvertently leak sensitive data.

As is customary, Apple remained largely tight-lipped about whether or not any of the flaws have been exploited in the wild, but WebKit flaws are typically highly favoured by threat actors, as Adam Boynton, senior enterprise strategy manager at Jamf, explained.

“[WebKit is] one of the largest attack surfaces on the [Apple] platform. Memory corruption doesn’t mean remote code execution, but these have become browser exploit chains in the past,” he explained.

However, added Boynton, the volume of WebKit flaws in the latest update may not be the most noteworthy thing about it – the standout fix in his view is CVE-2026-65346, an integer overflow in ImageIO, a framework that enables applications to read and write image files.

“Exploiting it could allow an attacker to write memory where they shouldn’t and gain code execution,” he said. “Image parsing flaws have historically been the delivery mechanism for zero-click spyware targeting executives and other high-value individuals.”

Also worth prompt attention is CVE-2026-65329, a telephony issue affecting iPhones that could enable an attacker with network privileges to bypass IPSec authentication and snoop on network traffic.

Meanwhile, the US Cybersecurity and Infrastructure Security Agency (Cisa) has added another Apple flaw – CVE-2026-65400 – to its Known Exploited Vulnerabilities (Kev) catalogue of issues deemed of significant risk to the federal government.

CVE-2026-65400 was addressed by Apple earlier this month. It is another improper authentication vulnerability that could allow a threat actor with an established presence on the target network to authenticate to the target device’s Screen Sharing feature without valid credentials.

According to the Dutch National Cyber Security Centre – NCSC-NL – it has been used against multiple systems upon which port 5900 was exposed to the public internet to obtain root access and install a Monero crypto miner.

As CVE-2026-65400 enables root access, a threat actor could also use it as part of a wider attack to establish persistence, steal credentials and data, and deploy other malware, although at the time of writing, there appears to be no indication that it has been used in any ransomware attacks.

Under an internal directive, US government agencies are obligated to remediate CVE-2026-65400 by Friday 21 August – its inclusion on the regularly updated Kev list is an indication that private sector chief information security officers should also take steps to remediate it if they have not already.

### Read more about Apple security

- The Secure Enclave offers hardware-isolated security for Apple devices. Learn how it compares with TPM and HSM technologies, as well as the best uses for these three approaches.
- Examine Apple MDM capabilities, key features and use cases and how platforms support enterprise device management, security, scalability and governance.
- Apple’s first ever Background Security Update fixes a WebKit browser engine bug that could enable threat actors to see and steal important data from their victims.
