---
id: collect-261121-veille-ia/gpt-openai/chatgpt-for-linux-official-app-pwa-and-cli-options
title: "ChatGPT for Linux: Official App, PWA and CLI Options"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-08-11"]
keywords: ["chatgpt", "linux", "official", "desktop", "browser", "install", "openai", "codex"]
source: https://www.layer3labs.io/guides/chatgpt-on-linux
source_anchor: ""
source_lines: [1, 130]
section: "GPT_OpenAI"
---
# ChatGPT on Linux

As of August 2026, OpenAI ships an official Linux desktop app, so running ChatGPT on Linux is finally first-class.

Yes, you can run ChatGPT on Linux, and there's finally an official way to do it. OpenAI released its Linux desktop app in public preview on August 11, 2026. Until then, Linux users had to rely on the web version or community-built clients.

This guide walks through all the practical ways to use ChatGPT on Linux: the official desktop app, the browser or PWA, unofficial third-party clients, and Codex CLI for the terminal.

Just keep in mind that the desktop app is still in public preview, so you may run into a few rough edges. We've also linked to more focused guides for Ubuntu, the terminal, and Codex if you want to dig deeper into the option that best fits your setup.

## Can You Run ChatGPT on Linux?

Yes. You can run ChatGPT on Linux four different ways, and as of August 2026 one of them is an official OpenAI desktop app.

The quickest option has always been the browser. Open chatgpt.com in any Linux browser and sign in. Nothing to install.

The new option is a native desktop app you install with a package file. It brings ChatGPT, the Codex coding agent, and a built-in browser to your desktop. The rest of this page walks through each method so you can pick one.

- Official desktop app (.deb / .rpm): new, public preview
- Web app or installable PWA in a browser: zero install
- Unofficial third-party desktop clients: vet before use
- Terminal via Codex CLI: best for developers

## The Official ChatGPT Linux App (New)

OpenAI launched an official ChatGPT desktop app for Linux on August 11, 2026, in public preview. This is the first time Linux got a native, OpenAI-built app.

You install it with a package file. Debian and Ubuntu-based systems use a .deb file. Fedora uses an .rpm file. There is no Snap or Flatpak at launch, and both x64 and Arm64 builds exist.

OpenAI tested the app on Ubuntu 24.04 LTS, Ubuntu 26.04 LTS, and Debian 13, with RPM builds for Fedora. Download it from OpenAI's official ChatGPT download page.

The app is more than a chat window. It bundles ChatGPT, ChatGPT Work, the Codex coding agent, and a built-in web browser. Codex can run inside your local repositories, and the app can read local files without uploading them elsewhere.

- Supported/tested: Ubuntu 24.04 LTS, Ubuntu 26.04 LTS, Debian 13, Fedora (RPM)
- Formats: .deb and .rpm; x64 and Arm64; no Snap or Flatpak yet
- Includes ChatGPT, ChatGPT Work, Codex, and a built-in browser
- Some features need a paid plan; the app is proprietary

## Is the Linux App Stable Enough to Use?

The Linux app is a public preview, so treat it as early software. OpenAI says it may contain bugs and warns against relying on it for mission-critical work.

Early reviews are mixed. Testers report rough edges, plus some performance and integration complaints on certain distros.

For everyday questions and coding help, the app is worth trying. For anything you cannot afford to have break, keep the web app open as a backup.

## ChatGPT in a Browser and as a PWA on Linux

The browser route works on every Linux distro with zero install. Go to chatgpt.com, sign in, and start typing.

You can also install ChatGPT as a Progressive Web App (PWA) for a windowed, app-like feel. In Chrome, Chromium, Brave, or Edge, open the address-bar menu and choose "Install app."

A PWA gives you a dedicated window and a launcher icon without a real package. Note that Firefox does not support native PWA install, so use a Chromium-based browser for this trick.

The web route is the most reliable path today. It works the same on Ubuntu, Debian, Fedora, and everything else.

- Zero install: any Linux browser at chatgpt.com
- PWA install: Chrome, Chromium, Brave, or Edge address-bar menu
- Firefox cannot install PWAs natively

## Unofficial Third-party ChatGPT Clients for Linux

Community developers have built unofficial ChatGPT desktop wrappers for Linux, usually with Electron or Tauri. These are not made by OpenAI, and quality varies a lot.

Now that an official app exists, most people should start there. But some users still prefer a lightweight community client or one with extra features.

If you go this route, vet the project first. Check the star count, the date of the last commit, and how many open issues sit unresolved. Some once-popular clients have been archived and no longer get updates.

- Prefer the official app; treat wrappers as a fallback
- Vet each project: stars, recent commits, open issues
- Assume no support and no security guarantees

## ChatGPT in the Terminal on Linux (Codex CLI)

For developers, Codex CLI is OpenAI's first-party way to use ChatGPT-style help in the terminal. It is a coding agent that works right in your shell.

Install it with npm using the scoped package: run npm install -g @openai/codex, which needs Node.js 18 or later. OpenAI also ships a standalone installer that removes the Node dependency, and Homebrew works on supported systems.

One common mistake trips people up: the unscoped codex npm package is an unrelated 2012 project. Always install the scoped @openai/codex instead. Then run codex --version to confirm and sign in with your account.

Other terminal options include calling the OpenAI API with curl or using community tools like shell-gpt. One clarification: the old "act as a Linux terminal" prompt only makes ChatGPT simulate a shell in chat. It does not run commands on your real machine.

- npm: npm install -g @openai/codex (Node.js 18+)
- Standalone installer removes the Node.js requirement
- Never install the unscoped 'codex' package by mistake
- Codex CLI also ships inside the official Linux desktop app

## What About ChatGPT Atlas on Linux?

ChatGPT Atlas is not available on Linux, and OpenAI has announced no Linux plans for it. Atlas is OpenAI's Chromium-based AI web browser, launched for macOS on October 21, 2025.

Windows, iOS, and Android are listed as coming soon, but Linux is not on the roadmap as of August 2026.

The practical substitute is the built-in browser inside the new official ChatGPT Linux app. It is the closest Atlas-style experience on Linux, and you can also use ChatGPT in any Chromium browser.

## Which Method Should You Use?

Pick your method based on how you work and how much you value stability. Most people want either the browser or the new desktop app.

Casual users get the smoothest experience from the web app or a PWA. Developers get the most value from Codex CLI or the desktop app's bundled Codex. Privacy-focused users who want offline models should look at local runners on the alternatives page.

- Want zero install and rock-solid reliability: use the web app or a PWA
- Want a native desktop app and can accept preview bugs: install the .deb or .rpm
- Live in the terminal and write code: use Codex CLI
- Need offline or private models: see our local LLM alternatives

## The Bottom Line on ChatGPT on Linux

Running ChatGPT on Linux is now official and simple. You have four solid paths: the new desktop app, the browser or PWA, vetted third-party clients, and Codex CLI in the terminal.

Start with the web app if you want reliability today. Try the official desktop app if you want native features and can accept preview rough edges. Reach for Codex CLI when your work lives in the shell.

From here, dig into the focused guides below for step-by-step setup on Ubuntu, the terminal, and Codex, or compare ChatGPT with local, offline models.

## Frequently Asked Questions

- Yes. OpenAI launched an official ChatGPT desktop app for Linux on August 11, 2026, in public preview. It installs as a .deb on Debian/Ubuntu systems or an .rpm on Fedora.
- OpenAI tested the app on Ubuntu 24.04 LTS, Ubuntu 26.04 LTS, and Debian 13, with RPM builds for Fedora. It ships for both x64 and Arm64, but there is no Snap or Flatpak at launch.
- Yes. Open chatgpt.com in any Linux browser and sign in. You can also install it as a PWA in Chrome, Chromium, Brave, or Edge for an app-like window.
- Installing the .deb adds an OpenAI apt repository to your system. After that, future updates arrive through your normal apt upgrade process, like any other package.
- Use Codex CLI, OpenAI's first-party terminal coding agent. Install it with npm install -g @openai/codex (Node.js 18+) or the standalone installer, then sign in with your account.
- Not yet. It is a public preview that may contain bugs, and OpenAI advises against relying on it for mission-critical work. Keep the web app as a backup for anything critical.
- No. ChatGPT Atlas is macOS-only, and OpenAI has announced no Linux plans as of August 2026. The built-in browser in the official Linux app is the closest substitute.
- They vary widely. They are community-built, not made by OpenAI, and some popular ones have been abandoned. Prefer the official app, and vet any third-party project's stars, commits, and open issues first.

### Rolling out ChatGPT on Linux across your team?

Layer3 Labs helps teams adopt ChatGPT, Codex, and local models inside real Linux and developer workflows. Book a free AI workflow audit and we will map the fastest, safest path for your environment.

Book a Consultation
