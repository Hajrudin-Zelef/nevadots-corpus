---
id: collect-261121-veille-ia/muse/meta-employees-raise-muse-ai-privacy-concerns-exposing-security-flaws-chat-leaks
title: "Meta Employees Raise Muse AI Privacy Concerns, Exposing Security Flaws, Chat Leaks"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "meta", "employees", "security", "concerns", "internal", "data", "testing"]
source: https://www.techtimes.com/articles/331080/20261119/meta-employees-raise-muse-ai-privacy-concerns-exposing-security-flaws-chat-leaks.htm
source_anchor: ""
source_lines: [1, 64]
section: "Muse"
---
Meta launched its Muse AI agent with a promise that it was built from the ground up for privacy and security.

A recent investigation and several reports reveal that the people testing the product internally were raising serious doubts about whether that promise was being met before and even after the product shipped.

## Meta Employees Expose Muse AI Privacy Concerns

Reuters reviewed internal communications at Meta that showed employees raising significant concerns about Muse's handling of private data, including incidents where the AI agent appeared to be mixing up conversations between different user accounts. Those internal posts revealed a pattern of safety issues that employees believed could expose private data, with some describing failure modes that made the product unreliable in real-world conditions.

Employees testing Muse as recently as launch week flagged these concerns through internal channels, yet Meta proceeded with the September 2026 US launch as scheduled. A Meta spokesperson, Daniel Roberts, described the Reuters examples as isolated incidents and stated that the company prioritizes safety, conducts extensive internal testing, and follows strict rules. Meta did not dispute that the internal posts existed.

Separately, security firm Claroc discovered that the chat-sharing feature in Muse exposed shared conversations through publicly searchable DNS records. Users who shared a conversation using Muse's built-in share function did not realize the resulting links were discoverable by anyone with the right DNS query, not just the intended recipients.

Researchers found the exposed data included business documents, financial reviews, internal company information, holiday schedules for entire companies, and financial data from a large marketing firm, all accessible without signing in.

## Meta Muse AI: Security Flaws Detailed by Staff

Internal testing at Meta revealed additional specific security failures that employees raised in the period leading up to and during the product launch (via ArsTechnica). During controlled tests conducted by Reuters, Muse accessed private Signal messages on the device where it was installed, made possible by the broad Android notification permissions the app requests.

Muse also sent employees unsolicited bank balance figures from an unidentified source during testing, raising concerns about where the financial data was originating from and how the agent was processing information it had not been directly prompted to retrieve.

Employees also raised concerns about a human concierge feature that Meta had been developing alongside Muse. That feature, which involves Muse placing actual phone calls handled by human contractors when the AI cannot complete a task autonomously, prompted warnings from employees that it risked exposing sensitive user information to call center workers in ways that would undercut Muse's privacy-first positioning.

Meta began testing the phone-calling capability with employees in August 2026 before gradually rolling it out to public Muse users in the days following launch.

A separate disclosure from security researcher Patrick Wardle identified a vulnerability in Muse that put a user's dictated audio and account token at risk. That flaw affected individual users within their own Muse sessions rather than exposing data across accounts, making it technically distinct from the cross-account confusion described in the internal posts.

However, the cumulative picture painted by Reuters's investigation and the public security disclosures was of a product dealing with multiple simultaneous security issues across different layers of its architecture.

## Meta Muse Exposes User Chats to Others

The Claroc discovery about DNS-accessible shared chats represented one of the most immediately concrete examples of Muse exposing user data beyond the intended audience. The flaw did not require any sophisticated hacking. Security researchers needed a single DNS query to surface thousands of previously shared chat links, all of which were accessible to anyone who found them, without any login requirement.

The exposed conversations were created by users who had explicitly chosen to share them with specific recipients but did not expect that those links would be discoverable by third parties. Claroc confirmed it had no evidence that anyone had misused the exposed chats before the issue was identified, and after Meta was alerted, the company temporarily disabled the sharing feature while it investigated the problem.

Meta subsequently restored the feature with a fix in place, and Claroc confirmed that the previously accessible links were no longer reachable.

Ars Technica's separate reporting on internal Meta communications showed that employees were not merely raising abstract concerns but actively "freaking out" in internal channels about the privacy implications of what they were observing. The internal chats reviewed by Ars Technica showed employees describing Muse behavior that they found alarming, and that conflicted directly with how the product had been marketed to the public.

**Related Article:**
**Meta Muse AI Assistant Suffers From Zero-Day Vulnerability Despite Promise of Privacy and Security**

## Frequently Asked Questions

### What Is Meta?

Meta Platforms, Inc. is an American technology company headquartered in Menlo Park, California, and is the parent company of Facebook, Instagram, WhatsApp, Messenger, and Threads, as well as the developer of Meta AI, Muse, and the company's portfolio of AI products. Founded by Mark Zuckerberg in 2004 as Facebook, the company rebranded as Meta in 2021 to reflect its focus on the development of the metaverse and augmented reality hardware through its Quest VR headset line. Meta is one of the most widely used technology companies in the world, with billions of users across its apps, and it generates revenue primarily through advertising across its social media platforms.

### What Is Meta Muse?

Meta Muse is an AI agent developed by Meta and launched for US users in September 2026. Zuckerberg described Muse at launch as a personal AI assistant built from the ground up for privacy and security. The agent is designed to autonomously manage tasks across a user's digital life, including reading and composing emails, managing calendar events, handling financial tasks through connected payment accounts, making phone calls on behalf of the user, browsing the internet, and acting on information drawn from connected apps and devices. Muse operates through broad device permissions that allow it to access notifications, messages, and other data from other apps installed on the same device in order to provide context-aware assistance. Each Muse agent runs in its own cloud virtual machine, gated by a separate security process called Sentinel.

### Is Meta Muse a Security Liability?

Based on the Reuters investigation, Claroc's research, and the Ars Technica reporting on internal Meta communications, Muse had experienced multiple distinct security and privacy failures by November 2026, two months after its US launch.

The failures range from the DNS-accessible shared chat links, which exposed conversations that users believed were private, to internal testing that showed Muse reading Signal messages without direct user instruction, to employees reporting cross-account data confusion during testing. Meta described the incidents individually as isolated, but the volume and variety of issues raised by both internal employees and external researchers suggest a pattern rather than a series of unrelated anomalies.

The severity varies: the shared chat exposure affected users across the platform broadly, while the Signal and bank balance issues were observed in controlled testing rather than as widespread public incidents.

### Is Meta Muse Safe to Use?

Meta maintains that Muse is built with strong security foundations, that the incidents described are isolated, and that the company conducts extensive internal testing and follows strict internal rules. The sharing feature vulnerability was identified, reported to Meta through a bug bounty program, and patched.

For users who want to minimize risk while using Muse, security researchers and IT experts recommend treating any AI agent with broad device permissions with caution, avoiding sharing conversations containing sensitive personal, financial, or business information through any AI tool's share feature, and monitoring the app's permission settings to understand what data Muse can access. Organizations using Muse in business contexts should establish clear internal policies about which categories of information employees may submit to AI assistants and audit which tools are connected to business accounts.
