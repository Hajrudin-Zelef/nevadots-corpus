---
id: collect-261121-veille-ia/copilot-microsoft/apple-ios-27-ipados-27-macos-golden-gate-what-s-new-addigy
title: "Apple iOS 27, iPadOS 27 & macOS Golden Gate: What's New | Addigy"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["siri", "apple", "iphone", "intelligence", "ipados", "rebuilt", "users", "hardware"]
source: https://addigy.com/blog/whats-new-in-os-27/
source_anchor: ""
source_lines: [1, 125]
section: "Copilot_Microsoft"
---
Apple ships OS 27 to Mac, iPhone, iPad, and Apple TV on **September 14, 2026**. This is a performance-and-Siri release: the headline is a fully rebuilt Siri AI, but the changes your users will notice first are speed improvements that reach all the way back to hardware from 2019.

Here’s what’s actually in it, and what each part means if you manage a fleet.

## **Before Anything Else: Two Availability Facts**

**Running OS 27 and getting Apple Intelligence are two different things.** iOS 27 installs on iPhone 11 and later — every model iOS 26 supported. Siri AI, Write with Siri, visual intelligence, and the generative features described below require an Apple Intelligence–capable device, which is a much shorter list. Before you plan any rollout communication, pull your fleet’s hardware inventory and split it into those two buckets. The gap between “my phone updated” and “I don’t have the new Siri” is going to generate help desk tickets.

**Siri AI is not launching in the EU on iPhone or iPad.** Apple has attributed this to the Digital Markets Act, and Craig Federighi has said no workable solution emerged with regulators. If you manage devices in the EU, plan on iOS 27 and iPadOS 27 arriving without the release’s marquee feature. Mac is not currently listed under this restriction.

Siri AI also launches **in beta**, in English first, with more languages to follow.

## What’s **New in iOS 27?**

#### **Performance and everyday polish**

The most broadly useful part of the release, because it applies to every supported device rather than just the newest ones.

- **Apps launch up to 30% faster,** including third-party apps, via preloading of key app data
- New photos appear in the library up to **70% faster;** AirDrop transfers are up to **80% faster**
- An optimized CPU scheduler improves performance-intensive workloads — and it’s coming to older models all the way back to **iPhone 11** , so iOS 27 supports every model iOS 26 did
- Smoother system animations, including swiping between Home Screen pages
- Smarter, more seamless transitions between Wi-Fi and cellular, so users stop manually toggling Wi-Fi off
- A new **send indicator in Messages** shows delivery status when users are on a slow connection
- A rebuilt search foundation powering Spotlight, Photos, and Mail, with a new Mail ranking system that surfaces better top hits

**What this means for IT Admins:** this is your case for updating older hardware rather than replacing it. If you’re carrying iPhone 11s and 12s, OS 27 is a performance upgrade on devices you already own — a useful line for budget conversations.

#### **Design Updates**

- Refined Liquid Glass that diffuses background content better for readability
- A **new settings slider** to tune Liquid Glass from ultra clear to fully tinted
- Redesigned app icons with additional layers of Liquid Glass in the artwork

#### **Siri AI and Apple Intelligence**

- **Siri AI** : a rebuilt, conversational Siri with personal context understanding, on-screen awareness, app actions, and world knowledge
  - Swipe down from the **Dynamic Island** to type to Siri or start a conversation
  - A dedicated **Siri app** for revisiting conversations, synced privately via iCloud
- Swipe down from the 
- **Visual intelligence in the Camera app** via a new Siri mode — point at food for nutritional insights, or point at a bill and split the tab with Apple Cash
- More expressive, customizable Siri voices, and significantly upgraded dictation accuracy on the most capable iPhones
- **Write with Siri** virtually anywhere you type, plus writing feedback and system-wide proofreading

**What this means for IT Admins:** Siri conversation history syncs through iCloud, and Siri AI reads on-screen content and personal data across Mail, Messages, Notes, Reminders, and Calendar. That’s a conversation to have with your security and compliance stakeholders *ASAP*. Decide now whether you’re enabling, deferring, or restricting these features by device group.

#### **Smarter apps**

- **Safari** : auto-organizes tabs into topics, monitors pages for changes with**Notify Me** , and builds custom extensions from a plain-language description with**Describe an Extension**
- **Passwords** : automatically updates weak or compromised passwords on eligible sites with one tap
- **Messages** : one-tap contextual suggestions (reminders, notes) and smart photo search within conversations
- **Calendar** : create and edit events using natural language
- **Phone** : call context surfaces relevant info (like a flight confirmation code) when you call a business, processed entirely on-device
- **Home** : consolidated smart notifications, AI-generated descriptions of camera clips, searchable footage across cameras, and 4K clip viewing on supported cameras
- **Shortcuts** : describe an automation in plain language and Shortcuts assembles it for you
- **Image Playground** : reimagined with new generative models, including photorealistic styles and touch-based editing
- **Photos** : an upgraded Cleanup tool, a new Extend tool for expanding images, and**spatial reframing** — adjust a photo’s perspective after the fact as if you’d moved the camera
- **iCloud Shared Albums** opened to Android and Windows contributors at full resolution
- Perimenopause and menopause support in Health cycle tracking

**What this means for IT Admins:** the Passwords one-tap credential rotation is worth a look. On managed devices where you’re already running a password manager, decide whether these two systems coexist or conflict.

#### **Other app updates**

- iCloud Shared Albums now let **Android and Windows** friends contribute, with full-resolution sharing
- Health adds **perimenopause and menopause** support to cycle tracking
- Maps gets a dramatically sharper, vision-model-enhanced **Flyover**

#### **Child safety**

- New setup assistant so parents choose exactly which apps kids start with
- **Ask to Browse** : kids request permission to visit new websites (on by default under 13)
- Communication Safety now also intervenes on gore and violent content
- New **time allowances** for entertainment, games, and social media, with expert-guided age-based recommendations and schedules
- A fully redesigned Screen Time experience for parents

**What this means for IT Admins:** if you support K–12 or any family-managed deployment, this is the section to read twice. Ask to Browse defaulting on for under-13 accounts will change behavior on managed student devices the day they update, and it works in Safari on Mac alongside iPhone and iPad.

## **New in iPadOS 27**

iPadOS 27 carries most of the iOS 27 improvements — Liquid Glass refinements, the rebuilt search, the app updates, and the child safety features — plus iPad-specific additions:

- Apps launch up to **30% faster**
- Browsing and transferring files to an external drive is up to **5x faster** — Apple says it’s now as fast as Finder on a Mac
- Siri AI’s full rich conversational experience comes to iPad
- **Visual intelligence is built into the screenshot experience** , letting you ask Siri about what’s on screen, search visually, and take action
- The Siri app syncs conversations so you can start on iPhone, continue on iPad, and finish on Mac

## **New in macOS 27 Golden Gate**

Golden Gate continues Apple’s California naming tradition. The interface changes are the visible part, but Siri’s integration into the system is the substantive one.

#### **Siri and intelligence**

- Siri is integrated into **Spotlight** — type a question, press return to start a conversation, then drag and resize it anywhere
- New system-wide context menus: control-click files, images, or text and ask Siri about the selection — for example, comparing multiple quote documents in a table
- Visual intelligence via a dedicated keyboard shortcut — select anything on screen and type directly to Siri
- The rebuilt search foundation for Spotlight, Photos, and Mail applies on Mac too

#### **Interface**

- A more uniform toolbar across the top of apps, for better legibility and structure
- Sidebars now extend to the edges of the window, with refractions as content scrolls beneath
- Sidebar icons regain their color, making apps easier to distinguish
- Every window gets the same tighter corner radius — including apps that haven’t been updated
- Smoother animations for Mission Control and moving between Spaces

#### **Child safety**

- Ask to Browse works in Safari on Mac alongside iPhone and iPad

**What this means for IT Admins:** Siri in Spotlight puts a conversational AI assistant one keystroke from every managed Mac, reachable by default. Whatever your organization’s stance on AI assistants and company data, that stance now needs a configuration behind it.

## **OS 27 Availability**

- **OS 27 ships September 14, 2026** for Mac, iPhone, iPad, and Apple TV
- **Siri AI launches in beta later this year** , in English first, with more languages to follow
- **Siri AI will not be available in the EU on iOS and iPadOS at launch**
- Apple Intelligence features are free and supported on the same hardware as today. Some server-powered features, like image generation, have daily limits; most iCloud+ plans increase that access.

## **Plan Your Rollout Before September 14**

Every major release is the same tradeoff: your users want the new features on day one, and you need time to validate. You don’t have to choose blindly — you just need the deferral window set before the update goes live.

Not managing your Apple fleet with Addigy yet? Start a free trial and go into OS 27 season with a plan instead of a scramble.
