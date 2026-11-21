---
id: collect-261121-veille-ia/apple-ios/safari-26-6-1-patches-multiple-webkit-security-flaws-on-macos-sonoma-and-sequoia-9to5mac
title: "Safari 26.6.1 patches multiple WebKit security flaws on macOS Sonoma and Sequoia - 9to5Mac"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: ["2026-08-18"]
keywords: ["webkit", "macos", "security", "bugzilla", "safari", "sonoma", "sequoia", "content"]
source: https://9to5mac.com/2026/08/18/safari-26-6-1-patches-multiple-webkit-security-flaws-on-macos-sonoma-and-sequoia/
source_anchor: ""
source_lines: [1, 214]
section: "Apple_iOS"
---
Following yesterday’s wave of point updates with security fixes, Apple has now detailed the security content of Safari 26.6.1. Here are the details.

## Codex is listed multiple times in the CVE credits

Apple today updated its “Apple security releases” page with details about the bug fixes included in Safari 26.6.1, which is now available for macOS Sonoma and macOS Sequoia.

According to the document, the update includes 22 CVEs, all related to WebKit, including two vulnerabilities in which “processing maliciously crafted web content [could] lead to memory corruption.”

Interestingly, and in line with recent security updates, OpenAI Codex Security is listed nine times, further highlighting the growing role of AI tools in finding security vulnerabilities in Apple’s software.

Here’s Apple’s full security content for Safari 26.6.1:

## Safari 26.6.1

Released August 18, 2026

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected Safari crash

Description: An out-of-bounds access issue was addressed with improved bounds checking.

WebKit Bugzilla: 317632

CVE-2026-64784: Janggoon Lee of Out of Bounds, OpenAI Codex Security – Amy Burnett

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected Safari crash

Description: The issue was addressed with improved memory handling.

WebKit Bugzilla: 313452

CVE-2026-43795: wwwlk

WebKit Bugzilla: 318348

CVE-2026-65338: OpenAI Codex Security – Amy Burnett

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to memory corruption

Description: The issue was addressed with improved memory handling.

WebKit Bugzilla: 318405

CVE-2026-65341: Henock Habte

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected Safari crash

Description: A memory corruption vulnerability was addressed with improved locking.

WebKit Bugzilla: 321480

CVE-2026-64782: Seonwook Kim, Shubham Chaskar, lattice, Josef Korbel

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected Safari crash

Description: The issue was addressed with improved input validation.

WebKit Bugzilla: 321484

CVE-2026-64781: Thomas Guillem

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected Safari crash

Description: This issue was addressed through improved state management.

WebKit Bugzilla: 321517

CVE-2026-65351: Niels Hofmans

WebKit Bugzilla: 316996

CVE-2026-65340: Claudio Bozzato and Francesco Benvenuto of Cisco Talos, Josef Korbel (Citadelo)

WebKit Bugzilla: 317142

CVE-2026-65337: OpenAI Codex Security – Amy Burnett

WebKit Bugzilla: 317349

CVE-2026-65336: Josef Korbel

WebKit Bugzilla: 316723

CVE-2026-65335: OpenAI Codex Security – Amy Burnett

WebKit Bugzilla: 317603

CVE-2026-65333: OpenAI Codex Security – Amy Burnett

WebKit Bugzilla: 317450

CVE-2026-65332: OpenAI Codex Security – Amy Burnett

WebKit Bugzilla: 317611

CVE-2026-65331: OpenAI Codex Security – Amy Burnett

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected process crash

Description: A use-after-free issue was addressed with improved memory management.

WebKit Bugzilla: 316347

CVE-2026-64715: Hossein Lotfi (@hosselot) of TrendAI Zero Day Initiative

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected Safari crash

Description: The issue was addressed with improved checks.

WebKit Bugzilla: 316918

CVE-2026-64780: OpenAI Codex Security – Amy Burnett

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected Safari crash

Description: A memory corruption issue was addressed with improved state management.

WebKit Bugzilla: 316791

CVE-2026-65334: OpenAI Codex Security – Amy Burnett

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to memory corruption

Description: A memory corruption issue was addressed with improved memory handling.

WebKit Bugzilla: 317317

CVE-2026-43794: Dung Do (@_piers2) of Calif.io

### WebKit

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected process termination

Description: A use-after-free issue was addressed with improved memory management.

WebKit Bugzilla: 313703

CVE-2026-64787: 杉山 壮太, Shubham Chaskar

### WebKit History

Available for: macOS Sonoma and macOS Sequoia

Impact: Visiting a maliciously crafted website may leak sensitive data

Description: The issue was addressed with improved checks.

WebKit Bugzilla: 315528

CVE-2026-64778: Mohit Negi

### WebKit Storage

Available for: macOS Sonoma and macOS Sequoia

Impact: Processing maliciously crafted web content may lead to an unexpected Safari crash

Description: A memory corruption vulnerability was addressed with improved locking.

WebKit Bugzilla: 321485

CVE-2026-64779: Shubham Chaskar, Tommy DeVoss from Braze Security Team (@thedawgyg)

## Additional recognition

### WebKit

We would like to acknowledge Henock Habte for their assistance.


To learn more about Apple’s security releases, follow this link.

## Comments
