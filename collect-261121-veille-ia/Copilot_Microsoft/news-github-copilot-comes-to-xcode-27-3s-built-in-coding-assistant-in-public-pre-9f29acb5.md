---
id: collect-261121-veille-ia/copilot-microsoft/news-github-copilot-comes-to-xcode-27-3s-built-in-coding-assistant-in-public-pre-9f29acb5
title: "News github copilot comes to xcode 27 3s built in coding assistant in public pre"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-04"]
keywords: ["xcode", "copilot", "github", "assistant", "apple", "coding", "extension", "beta"]
source: ""
source_anchor: ""
source_lines: [1, 34]
section: "Copilot_Microsoft"
---
What actually shipped
Apple released the first Xcode 27.3 beta and added GitHub Copilot to the IDE's built-in coding assistant. GitHub's changelog dated November 4, 2026 calls the integration a public preview that starts with Xcode 27.3 beta. It says all Copilot plans are supported.
Apple's release notes list the change under Coding Intelligence. They give it the identifier 185837709. MacRumors says the same beta has two other headline features: a way to test Siri AI integration in apps, and extra APIs for upcoming MacBook Pro models with touchscreens.
Extension or built-in: they are separate
GitHub says the older GitHub Copilot for Xcode extension stays available as a separate option. It describes the new integration as "another way to use Copilot in Xcode". Nothing in the announcement says the extension is deprecated.
The two options differ in how they plug in:
- The extension: GitHub's repository describes it as an Xcode extension that gives inline suggestions as you type, plus a chat assistant. It needs the Accessibility and Xcode Source Editor Extension permissions. The repository also lists a Background permission.
- The built-in assistant integration: You set it up in Xcode's Intelligence settings. GitHub's changelog doesn't mention extension permissions for it.
The extension's documented prerequisites are Xcode 8.0 or later and macOS Monterey 12.0 or later. Those don't apply to the new route. The preview needs Xcode 27.3 beta and macOS Tahoe 26.6 or later.
Setup steps
These steps come from GitHub's announcement:
- Download Xcode 27.3 beta from Apple Developer Downloads.
- Make sure the Mac runs macOS Tahoe 26.6 or later.
- Open Xcode, then go to Xcode > Settings > Intelligence.
- Set up GitHub Copilot in the coding assistant from that pane.
GitHub also points to Apple's "Setting up coding intelligence" page and the beta release notes. Those sources didn't give Copilot-specific sign-in screens or labels. I won't guess at them. If your Copilot plan is managed by an organization, expect your admin's policies to apply, which is general Copilot behavior rather than something the announcement states.
Where this fits
This isn't Apple's first third-party assistant. Developers have been able to integrate AI coding agents directly into Xcode since Xcode 26.3. Xcode 27 added Google Gemini support, a redesigned interface, interactive planning and multi-turn conversations. Copilot now sits alongside those options. The practical shift is that Copilot users don't have to depend on the extension and its permissions.
Dataconomy reports that Xcode 27.3 is expected to ship in December with macOS 27.3 and related OS updates. That is a press expectation, not a date from GitHub or Apple's changelog entry. Treat the final release timing as unconfirmed.
What we don't know
The announcement leaves several gaps:
- Models and features: It doesn't say which Copilot models, agent actions or chat features the integration exposes. It doesn't say whether it matches Copilot in VS Code or JetBrains.
- Privacy: There are no Copilot-specific data-handling or retention terms. Apple's general guidance, as relayed in the research notes, says an agent may access project files. It points users to "About Intelligence in Xcode & Privacy" in the Intelligence settings. Read that before pointing the assistant at proprietary code.
- Preview limits: There is no stated end date or list of known issues.
Advice for admins and teams
- Don't put the beta on production build machines. Keep Xcode 27.3 beta on a separate Mac or a separate install while you evaluate it.
- Managed Macs: The research notes cite an Apple MDM setting, CodingAssistantAllowExternalIntegrations , which turns off the coding assistant when set to false. The notes don't say whether it targets Copilot alone or all external integrations, so test it before relying on it.
- Send feedback. GitHub asks for it in the GitHub Copilot for Xcode repository or through Apple Feedback Assistant.
Bottom line
This is a modest but real change for Apple-platform developers who use Copilot. The built-in route trades the extension's permission setup for a beta toolchain and a recent macOS. Test it on a non-critical project first. Then check how it handles your code, your plan's policies and your workflow before you change anything.
References
- GitHub Copilot is now available in Xcode’s coding assistant GitHub Changelog · 2026-11-04T19:10:40+00:00
- Installing the GitHub Copilot extension in your environment - GitHub Docs docs.github.com
- Apple Adds GitHub Copilot To Xcode 27.3 Beta - Dataconomy dataconomy.com
