---
id: collect-261121-veille-ia/actu-business/chrome-release-monitor-src-content-releases-en-chrome-152-md-at-76203d98592b06d5427f8b2f5f77e7fcce08
title: "chrome-release-monitor/src/content/releases/en/chrome-152.md at 76203d98592b06d5427f8b2f5f77e7fcce08eec7 · Shonee/chrome-release-monitor"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-09-03"]
keywords: ["chrome", "chrome-release-monitor", "performance", "title", "stable", "audience", "windows", "macos"]
source: https://github.com/shonee/chrome-release-monitor/blob/HEAD/src/content/releases/en/chrome-152.md
source_anchor: ""
source_lines: [1, 54]
section: "Actu_Business"
---
| title | Chrome 152 Release and Usage Guide | 
|---|---|
| locale | en | 
| milestone | 152 | 
| version | 152.0.7977.75/76 | 
| channel | Stable | 
| publishedAt | 2026-09-03 12:00:00 +0800 | 
| updatedAt | 2026-09-03 13:18:00 +0800 | 
| stableReleasedAt | 2026-08-25 00:00:00 -0700 | 
| versionReleasedAt | 2026-09-01 00:00:00 -0700 | 
| status | published | 
| summary | Chrome 152 expands CSSPseudoElement support, introduces the CPU Performance API, and adds Connection Allowlists for network restrictions. | 
| platforms |  | 
| tags |  | 
| audience |  | 
| highlights |  | 
| securityFixes | 26 | 
| sources |  | 
| images |  | 

| Windows | macOS | Linux | 

| Chrome | Stable | Security | 

| user | developer | enterprise | 

|  |  |  | 

| title | description | audience | 
|---|---|---|
| More pseudo-elements available to JavaScript | CSSPseudoElement expands to backdrop, scroll-marker, and view-transition pseudo-elements. | developer | 

| title | description | audience | 
|---|---|---|
| CPU Performance API | Applications can adapt graphics, computation, and background work to a device performance class. | developer | 

| title | description | audience | 
|---|---|---|
| Connection Allowlists | Response headers can restrict the external endpoints available to documents and workers. | enterprise | 

Chrome 152 entered the desktop Stable channel on August 25, 2026. The recorded `152.0.7977.75/76` update was published on September 1 and lists 26 security fixes.


The initial Stable build was Windows `152.0.7977.53/54` and macOS/Linux `152.0.7977.54`. This article tracks Windows and macOS `152.0.7977.75/76`, with Linux on `152.0.7977.75`.

CSSPseudoElement support expands to `::backdrop`, `::scroll-marker`, and view-transition pseudo-elements. Developers can associate animation and interaction logic more precisely, while feature detection remains necessary for older browsers.

The CPU Performance API exposes a device performance class so applications can reduce graphics quality, divide expensive computation, or adjust background work. Do not use the class for fingerprinting or identity, and distinguish it from live Compute Pressure signals.

The `Connection-Allowlist` response header defines URL patterns that documents and workers may contact. Inventory APIs, telemetry, CDNs, and third-party services before rollout so required endpoints are not accidentally blocked.

Users should apply the September 1 patch and restart Chrome. Enterprise teams should test connection policies, proxies, monitoring scripts, and applications that adapt to hardware capability.

Open the Chrome menu, choose **Help → About Google Chrome**, wait for installation, restart, and confirm Windows/macOS `152.0.7977.75/76` or Linux `152.0.7977.75`.
