---
id: collect-261121-veille-ia/windows-os/windows-11-26h2-app-updates-new-photos-windows-security-paint-and-camera
title: "Windows 11 26H2 App Updates: New Photos, Windows Security, Paint and Camera"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "photos", "microsoft", "security", "camera", "updates", "version", "apps"]
source: https://dev.to/rohan_techofficial_81a0f/windows-11-26h2-app-updates-new-photos-windows-security-paint-and-camera-1b6i
source_anchor: ""
source_lines: [1, 54]
section: "Windows_OS"
---
Four of Windows 11's built-in apps changed in a big way this month, and none of them needed a Windows reinstall to do it. Photos has been rebuilt from the ground up, Windows Security finally looks like it belongs on Windows 11, Paint picked up a fix you really shouldn't skip, and the Camera gained tricks that let one webcam serve several apps at once. So what does each one actually do now — and how do you get the updates on your own PC?

## What's arriving with the November 2026 update

Windows 11, version 26H2 — the 2026 Update — has been out since September 29, 2026, and it arrived as a small enablement package for PCs already running versions 24H2 or 25H2. The app updates landing now ride on top of it. The November 2026 update (KB5127936) went out as an optional preview on October 28 and reaches everyone on Patch Tuesday, November 10, as build 26300.9818 for 26H2.

One thing to know before you go hunting for these: Microsoft rolls features out in waves, so your PC might get one of them a few days after your neighbour's. That's normal, not a bug. The four apps below don't all arrive the same way, either. Some come through the Microsoft Store, and some need the monthly Windows update. Microsoft's own what's-new page for version 26H2 lists the full picture, and if you want the broader rundown of the release, our Windows 11 26H2 what's new guide covers the rest of it.

## Photos gets a full rebuild

This is the biggest change of the four. Microsoft started rolling out a redesigned Photos app to Windows Insiders on October 15, 2026, and plans to reach all Windows 11 users later in 2026 — though it hasn't given an exact date yet. The rebuild touches all three parts of the app at once.

The new Gallery pulls photos and videos from your PC, OneDrive, and iCloud into one place, with albums, OneDrive Moments, and natural-language search for finding people and moments. The Editor has been rebuilt as a native WinUI 3 experience, with its tools grouped into Crop, Enhance, Repair, and Stylize. It keeps HDR intact while you edit, saves HEIC and WebP files in their original formats, and its Repair tool uses an improved AI model to remove objects you circle or brush over. Relight, which lets you place up to three movable lights in a photo, is expanding to all Windows 11 devices. The redesigned Viewer is cleaner and photo-first, with SVG support and an info panel showing camera details, a location map, a histogram, and visual search.

Microsoft also published speed numbers from its own controlled tests against Photos version 2026.11060: the Editor launches 25% faster, and the Viewer starts 15% faster on average — up to 30% faster for HEIC images. Real results will vary by device, but the direction is clear. One quirk worth knowing: on Insider PCs, the app you know as Photos is being renamed Photo Viewer, while the new Gallery takes the Photos name and its spot on the Start menu. Neowin's breakdown of the announcement has the full feature list if you want to dig deeper.

## Windows Security finally looks like Windows 11

Has the Windows Security app ever felt like it came from an older Windows? Microsoft agrees. Its own release notes put it plainly: the current version "still relies on navigation, layouts, and visual elements that feel noticeably older than Microsoft's modern Windows 11 apps." The refresh fixes exactly that.

Pages across the app now use consistent layouts and navigation that respond to different window sizes. Controls, status indicators, typography, spacing, and colours have all been refreshed so the app feels like the rest of Windows 11, and status information is shown more clearly, with better progress, error, and refresh states. On managed devices, device security details now load progressively, so the information you need appears sooner instead of waiting for everything at once.

The refreshed app is rolling out to Windows Insiders with version 1000.29680.0.1000, and Microsoft hasn't announced a date for everyone else yet. You can check your own version in the app under Settings > About. The November update adds more than looks, too: you can now manage Administrator Protection from the Account protection section, and Memory Integrity is being enabled on more eligible devices. Windows Security is still the same protection underneath — this is a long-overdue coat of paint on the interface, not a new engine. If you want to see how it stacks up against third-party options, our best free antivirus for Windows guide puts it through its paces.

## Paint gets small fixes and one big security fix

Paint's November update looks quiet on the surface, and that's fine — until you read the security part. Version 11.2609.401.0 rolled out to all Windows Insider channels on November 6, 2026, with faster image loading, clear error messages for damaged or unsupported files instead of the app closing on you, correct layer selection after deleting a layer, brushes that work right at the canvas edges, and selection overlays that show at the right size and position on high-resolution displays. Microsoft's official Paint release notes list every one of them.

But the same build also fixes CVE-2026-83844, a remote code execution flaw in Paint rated 8.1 ("Important"). And here's the catch that matters: the fix comes through the Microsoft Store only. Windows Update won't deliver it, so a PC that looks fully patched can still run a vulnerable Paint. If you haven't opened the Store in a while, do it now and check that Paint is on 11.2609.401.0 or later. June's update added adjustable eraser transparency too, so if you edit images in Paint, both updates are worth having.

## Camera: one webcam, many apps at once

The camera changes in the November update are practical rather than flashy. Multi-App Camera lets several applications use the same camera stream at the same time, so a video call and a recording app no longer fight over the webcam. Basic Camera mode gives you a stripped-down camera that helps with troubleshooting or stability when the full pipeline misbehaves. Camera settings also gain pan and tilt controls for supported cameras, and the Privacy & security page now groups Camera, Microphone, and Location controls together with usage information in one place.

There's more on the way, though it's still in testing. Windows 11 Insider Beta build 26220.9836, released October 30, 2026, adds per-app camera, microphone, and location controls for desktop apps — each program gets its own switch instead of one shared toggle for all of them. Microsoft hasn't given a date for that one, and Insider features can still change or be dropped, so treat it as a preview of what's coming rather than a promise.

## How to get all four updates

The apps and the operating system update through different channels, so you need two quick checks:

1. 
**Open the Microsoft Store, go to Library, and click "Get updates."** This is how Photos, Paint, Camera, and Windows Security arrive. If an update is waiting, install it and restart the app.
2. 
**Open Settings > Windows Update and click "Check for updates."** This is how you get the November 2026 update (KB5127936) and, if you're still on 24H2 or 25H2, the 26H2 enablement package itself. Install what's offered and restart when asked.
3. 
**Check the versions.** In Paint, open Settings to see the version number — you want 11.2609.401.0 or later. In Windows Security, look under Settings > About for version 1000.29680.0.1000 to confirm you have the refreshed interface.
4. 
**If an update isn't there yet, wait a day or two and check again.** Microsoft staggers these rollouts, and the November update only reaches everyone on Patch Tuesday, November 10. Insiders get the app updates first; everyone else follows later in 2026.

Four apps, two update channels, and one security fix that only the Store can deliver — that's the November 2026 update in a nutshell. Get the Store updates first, then the Windows update, and check the version numbers if you're not sure what landed. And if you like tuning Windows once it's updated, Microsoft PowerToys for PC is the next thing worth installing.

*Originally published on Rohan Tech Official.*

## Top comments (0)
