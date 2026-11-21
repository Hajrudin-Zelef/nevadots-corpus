---
id: collect-261121-veille-ia/gpt-openai/how-to-install-chatgpt-desktop-app-on-linux
title: "How to Install ChatGPT Desktop App on Linux"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-08-11"]
keywords: ["chatgpt", "linux", "ubuntu", "install", "desktop", "fosslinux", "download", "package"]
source: https://www.fosslinux.com/160235/install-chatgpt-desktop-linux.htm
source_anchor: ""
source_lines: [1, 173]
section: "GPT_OpenAI"
---
*1.7K*

Yes, there is now an official ChatGPT desktop app for Linux. OpenAI released it in public preview on August 11, 2026, and it supports Ubuntu 24.04 and 26.04 LTS, Debian 13, and Fedora 43 and 44. Download the .deb or .rpm from chatgpt.com/download, install it with apt or dnf, and sign in with your ChatGPT account. Arch-based distros like CachyOS can use the community AUR package. This guide walks you through every install path and the features worth using.

## What Is the ChatGPT Desktop App for Linux and Why Should You Care

ChatGPT is OpenAI’s AI assistant and the most popular consumer AI tool on the planet. For years the desktop client existed only on macOS and Windows, which was a strange gap because so much real development and infrastructure work happens on Linux workstations. The Linux preview finally closes that gap.

The desktop app is not a thin browser wrapper. It bundles three things in one Electron application: regular ChatGPT chat, ChatGPT Work for documents and files, and Codex, OpenAI’s coding agent that can work against your local repositories. It also includes an in-app browser and supports Chrome extensions. If you are weighing OpenAI against Anthropic, my Claude Desktop setup guide covers the competitor’s Linux app.

Why does that matter for you? Because instead of juggling browser tabs, you get a dedicated window, a launcher icon, local file access, and a Codex environment that can read and edit projects on your machine. For me, that is the difference between asking an AI and letting an AI work alongside my actual tooling.

## System Requirements for the Linux Preview

The preview is deliberately conservative about what it supports. The official OpenAI documentation lists exactly these desktop distributions:

- Ubuntu 24.04 LTS and 26.04 LTS
- Debian 13
- Fedora 43 and 44

Each has packages for both x64 and ARM64 processors. To check your processor architecture, run:

fosslinux@ubuntu:~$ uname -m
x86_64

The output x86_64 means you want the x64 package. If you see aarch64 or arm64, grab the ARM64 build instead. Other Linux distributions may work, but OpenAI does not formally support them, which matters when a package update goes sideways.

One thing to know up front: native Wayland support is experimental. The app falls back to XWayland when available, and you can force native Wayland with:

fosslinux@ubuntu:~$ chatgpt --ozone-platform=wayland

I have had no issues with the default XWayland path on any of my test machines, but floating windows and keyboard shortcuts can be flaky under native Wayland while the support matures.

## How to Install ChatGPT on Ubuntu and Debian (Official .deb)

Ubuntu and Debian get the easiest path because the official package is a .deb. Head to chatgpt.com/download, pick the Linux build, and download the .deb for your architecture. The direct links also work: chatgpt_amd64.deb for x64 and chatgpt_arm64.deb for ARM64.

Once the download finishes, install it with apt. The leading ./ matters because it tells apt to treat the file as a local package:

fosslinux@ubuntu:~$ uname -m
x86_64
fosslinux@ubuntu:~$ cd ~/Downloads && wget -q https://persistent.oaistatic.com/codex-app-prod/linux/deb/latest/chatgpt_amd64.deb
fosslinux@ubuntu:~$ sudo apt install -y ./chatgpt_amd64.deb
...
Setting up chatgpt (26.810.52044)
...
fosslinux@ubuntu:~$ which chatgpt
/usr/bin/chatgpt
fosslinux@ubuntu:~$ chatgpt --version
26.810.52044

The installer also adds OpenAI’s signed package repository to your apt sources. That means future updates flow through the normal system update path, which I genuinely appreciate because I do not want to manually track Electron app releases.

## How to Install ChatGPT on Fedora (Official .rpm)

Fedora gets an official .rpm package for both x64 and ARM64. Download chatgpt.x86_64.rpm or chatgpt.aarch64.rpm from chatgpt.com/download, then install it with dnf:

fosslinux@fedora:~$ uname -m
x86_64
fosslinux@fedora:~$ sudo dnf install -y ./chatgpt.x86_64.rpm
[3/3] Installing chatgpt-0:26.810.52044
Complete!
fosslinux@fedora:~$ which chatgpt
/usr/bin/chatgpt
fosslinux@fedora:~$ chatgpt --version
26.810.52044
fosslinux@fedora:~$ dnf repolist | grep -i openai
openai-chatgpt        ChatGPT

Notice the last line: the installer registered an openai-chatgpt repository, exactly like the apt path on Ubuntu. Updates are handled through dnf from that point on.

## How to Install ChatGPT on CachyOS and Arch (AUR Package)

Arch-based distros are where I had to be honest with myself. There is no official pacman package from OpenAI, and the official docs do not list Arch or CachyOS as supported. But the desktop Linux community moved fast, and a maintainer repackaged the official binary into an AUR package called chatgpt-desktop.

On CachyOS, which ships paru as its AUR helper, the install is a single command:

fosslinux@cachyos:~$ uname -m
x86_64
fosslinux@cachyos:~$ paru -S chatgpt-desktop
...
==> Finished making: chatgpt-desktop 26.810.52044-1
...
fosslinux@cachyos:~$ which chatgpt
/usr/bin/chatgpt
fosslinux@cachyos:~$ chatgpt --version
26.810.52044

The AUR package builds from the same official .deb payload OpenAI publishes, so you get the identical binary and version. If you use plain Arch with yay instead of paru, the command is yay -S chatgpt-desktop.

Because this package is community-maintained, treat it as slightly behind the official release schedule and expect occasional rebuilds after upstream updates. That is the price of running on a rolling distro.

## First Launch and Configuration

After installation, launch ChatGPT from your applications menu or run chatgpt in a terminal. The first screen asks you to sign in with your ChatGPT account. You can authenticate with your email or an existing OpenAI login, and the app keeps you signed in across restarts.

Before you dive into prompts, take two minutes to set yourself up:

- Check Settings for your default project folder and the app theme.
- If you plan to use Codex, point it at a dedicated work directory so it does not roam your whole home folder.
- Enable auto-update if you want the app to pull new versions through the package manager automatically.

I keep auto-update on for daily use but off on anything resembling a production box, because I want to control when an agent client changes under me.

## Basic Features Every Beginner Should Try

Once you are signed in, the app looks familiar if you have used ChatGPT in a browser, but a few desktop features are worth knowing:

- ChatGPT chat: the classic assistant, now in its own window with conversation history.
- ChatGPT Work: a documents-and-files workspace for drafting, spreadsheets, and longer projects.
- Codex: OpenAI’s coding agent that can read your local repositories, edit files, and run commands with your permission.
- In-app browser: browse the web inside the app while ChatGPT works alongside you.
- Chrome extensions: the extension system works on the Linux build, so you can wire in the tools you already use.
- Voice: the Linux app supports Voice input and output, which surprised me given how much else was cut.

For a beginner, I would start with plain chat, then graduate to Codex once you are comfortable explaining tasks in natural language. If you want the terminal version, my guide to AI coding on Linux covers Codex CLI, Copilot, and Cursor in depth.

## Privacy and Data Considerations

The ChatGPT desktop app is a cloud service, full stop. Your prompts and files travel to OpenAI’s servers for processing, and the app holds an authenticated session that can touch your local files when you grant it access. Before you point Codex at a repository, think about what is in it.

Three habits I recommend: do not paste confidential code into chat, keep sensitive projects out of the folders you give Codex, and check OpenAI’s data usage policy for your account tier. Business and Work accounts have stricter retention controls than the free tier.

## ChatGPT Pricing at a Glance

The app itself is free to download and install. What you can do inside it depends on your plan:

- Free: standard ChatGPT with limited Codex usage.
- Plus: higher limits, faster models, and priority access.
- Pro: the top consumer tier with the most generous usage.
- Team and Enterprise: workspace controls, admin tools, and stricter data handling.

Pricing changes over time, so check chatgpt.com/pricing before committing a team to a paid tier.

## Troubleshooting Common Issues

Even in preview, the common failures are easy to fix.

**The app will not start after install.** On Wayland, launch it from a terminal with chatgpt –ozone-platform=wayland. If it still crashes, remove any stale config in ~/.config/chatgpt and relaunch.

**Dependencies missing on Ubuntu.** Run sudo apt –fix-broken install after a dpkg-based install, or reinstall the .deb with sudo apt install ./chatgpt_amd64.deb so apt resolves everything.

**ChatGPT is not on the applications menu.** Log out and back in so the desktop database refreshes, then search for ChatGPT in your launcher.

**Update not showing up.** Run sudo apt update on Ubuntu/Debian or sudo dnf upgrade –refresh chatgpt on Fedora. On CachyOS, rebuild the AUR package with paru -S chatgpt-desktop.

## What’s Missing on the Linux Preview

I want to be clear about what is cut, because the honest review matters more than the hype.

Native Computer Use, the feature that lets ChatGPT click around desktop apps like a human, is not available on Linux. That also removes Appshots, Record and Replay, and any voice requests that depend on desktop apps. OpenAI says a future release will add Linux support.

On top of that, the preview itself is early. You should not build a mission-critical workflow on a preview release, and neither should your team. Treat it as a fast-moving target and keep the browser tab as your reliable fallback.

## Frequently Asked Questions

**Is the ChatGPT desktop app free on Linux?** Yes, the app and the base ChatGPT experience are free. Higher usage limits and advanced tools require a paid plan.

**Does ChatGPT desktop work on ARM Linux?** Yes, official x64 and ARM64 packages ship for every supported distro.

**Is there an official Arch package?** No. Arch-based distros rely on the community AUR package chatgpt-desktop, which repackages the official binary.

**How do I update ChatGPT on Ubuntu?** Run sudo apt update then sudo apt install –only-upgrade chatgpt, because the installer adds OpenAI’s repository.

**Does the app work offline?** No. It is a client for OpenAI’s cloud services, so you need an internet connection and a signed-in account.

**Can I use ChatGPT on Linux without the app?** Absolutely, chatgpt.com works in any browser, and you can install it as a PWA from Chrome or Chromium. Codex CLI is the terminal option.

## Conclusion

OpenAI closing the Linux gap is one of those releases that feels overdue and welcome at the same time. The official app installs cleanly on Ubuntu, Debian, and Fedora with proper package-manager integration, and the community AUR package gets Arch-based distros like CachyOS most of the way there. I installed it on three distros in under an hour, and that is exactly the kind of frictionless setup a beginner deserves.

Start with the free tier, keep the browser tab as your fallback while the preview matures, and resist the urge to hand Codex your entire home directory. Then go build something.
