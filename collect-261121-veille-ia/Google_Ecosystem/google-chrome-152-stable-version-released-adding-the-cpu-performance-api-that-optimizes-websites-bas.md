---
id: collect-261121-veille-ia/google-ecosystem/google-chrome-152-stable-version-released-adding-the-cpu-performance-api-that-optimizes-websites-bas
title: "Google Chrome 152 stable version released, adding the 'CPU Performance API' that optimizes websites based on CPU performance."
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["performance", "chrome", "allows", "google", "stable", "user", "version", "released"]
source: https://gigazine.net/gsc_news/en/20260826-google-chrome-152/
source_anchor: ""
source_lines: [1, 79]
section: "Google_Ecosystem"
---
# Google Chrome 152 stable version released, adding the 'CPU Performance API' that optimizes websites based on CPU performance.


 Google has released version 152 of its web browser, Google Chrome, as a stable version. New features include the 'CPU Performance API,' which allows websites to determine the CPU performance of the user's device, and 'autocorrect,' which allows users to control whether or not automatic corrections are applied to user input.

 **Stable Channel Update for Desktop | Chrome Releases**

 

**Chrome 152 Beta | Blog | Chrome for Developers**

**https://developer.chrome.com/blog/chrome-152-beta?hl=ja#css_and_ui**

Google Chrome 152 moved to the stable channel for Windows, Mac, and Linux on August 25, 2026. The update will be rolled out gradually over the next few days to weeks.

**◆CSS and UI**

The CSS pseudo-element 'CSSPseudoElement' now supports an expanded range of elements, including '::backdrop', '::scroll-marker', and '::view-transition'. The new pseudo-elements are as follows:

**'::backdrop'**: Allows you to close a dialog box by clicking the background without interfering with clicks within the dialog box content. This eliminates the need for complex intersection logic.

**'::scroll-marker'**: Used to collect click statistics.

**'::view-transition'**: When

**GeometryUtils**is released, it will support geometry-aware view transitions and allow for interception of view transitions midway through.

In addition, HTML exposes the 'autocorrect' attribute. The HTML autocorrect attribute allows web authors to control whether automatic correction is applied to user input in editable elements such as 'input,' 'textarea,' and 'contenteditable hosts.'


 Additionally, CSS now includes 'media element pseudo-classes' that allow you to change the display depending on the playback status of video and audio. The supported pseudo-classes are: ':playing: playing', ':paused: paused', ':seeking: moving playback position', ':buffering: buffering', ':stalled: playback stopped', ':muted: muted', and ':volume-locked: volume locked'. These pseudo-classes are one of the focus areas of ' **Interop 2026** ', an initiative to improve web functionality compatibility across multiple browsers.

 In addition, it supports the 'relative alpha color CSS Color 5 alpha() function,' which allows you to set transparency relative to existing color values in CSS. Relative alpha color allows you to change only the alpha channel by referencing the original color.

 A new CSS property, 'window-drag,' has been added, allowing you to freely configure the title bar of a desktop web application as a draggable window title bar area. When applied, user pointer operations such as clicking and dragging in that area will not trigger normal page operations, but will instead move the top-level application window. While windows are usually moved by dragging the top of the window, this property allows you to freely specify which part of the screen to drag to move the window.

 **◆Addition of CPU Performance API**

 Chrome 152 introduces the 'CPU Performance API.' This API allows web applications to identify the CPU performance tier of a user's device. Web applications can use this information to improve the user experience. It can also be combined with the Compute Pressure API to respond to changes in CPU load, if needed. More detailed information is **available** on GitHub.

 **GitHub - WICG/cpu-performance: An API that exposes some information about how powerful the user device is. · GitHub**

 **https://github.com/WICG/cpu-performance**

 **◆ 'Connection permission list' to restrict connections to external servers.**

 In terms of security, a ' **Connection Allowlist' feature** has been added. The Connection Allowlist allows you to explicitly control external endpoints by restricting connections initiated from documents or workers via the Fetch API or other web platform APIs. Chrome evaluates the destination against this allowlist before establishing a connection; connections to verified endpoints are allowed, while connections that do not match are blocked.

 Furthermore, Chrome has announced that it is working on introducing notification attribution for Progressive Web Apps (PWAs) installed on macOS.

 **◆Frameless display mode and Window Shape API**

 In frameless display mode, the standard window frame and title bar are removed, allowing a standalone web app to occupy the entire browser window and supporting custom branding and menu hierarchies.

 Additionally, the Window Shape API allows you to set customized non-rectangular window shapes in isolated web apps on ChromeOS using `window.chromeos.isolatedWebApp.setShape`. This requires the window to be in frameless display mode and that you have window management permissions.

 **◆WebGPU: Controlling subgroup size**

 WebGPU, which enables high-speed computations using GPUs, now includes an optional GPU feature called 'subgroup-size-control.' This allows developers to specify the size of the groups processed at once in 'computation shaders' that perform calculations on the GPU. This optimizes performance and is particularly suitable for AI workloads.

 **◆ Remove 'Private Aggregation API'**

 Chrome 152 has also announced that it will end support for the 'Private Aggregation API' and other Privacy Sandbox APIs and will remove them. The Private Aggregation API is a Privacy Sandbox-related API that provides a mechanism for obtaining aggregated data used for advertising and other purposes while protecting user privacy. This API was only exposed through the Shared Storage API and Protected Audience API, and these APIs will also be deprecated and removed.

 **◆Security fixes**

 Chrome 152 also includes 327 security fixes. Ten of these vulnerabilities are classified as 'Critical.' You can find more details on **the security page** .

 **Chromium Security**

 **https://www.chromium.org/Home/chromium-security/**

 The next version, 'Google Chrome 153,' was released in **beta** on August 20, 2026. While Google had been releasing new versions of Chrome every four weeks since 2021, in March 2026 they announced a reduction in the release cycle from four weeks to two weeks, and Chrome 153 will be the first to adopt this new cycle. Therefore, the stable version of 'Google Chrome 153' is scheduled for release on September 8, 2026.

 **Google shortens Chrome's release cycle from 4 weeks to 2 weeks - GIGAZINE**



**Related Posts:**
