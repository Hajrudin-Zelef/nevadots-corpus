---
id: collect-261121-veille-ia/muse/meta-muse-ai-signal-message-privacy-1e231b94
title: "Meta muse ai signal message privacy"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "signal", "meta", "about", "agent", "cookies", "reuters", "message"]
source: ""
source_anchor: ""
source_lines: [1, 35]
section: "Muse"
---
All About Cookies is an independent, advertising-supported website. Some of the offers that appear on this site are from third-party advertisers from which All About Cookies receives compensation. This compensation may impact how and where products appear on this site (including, for example, the order in which they appear).
All About Cookies does not include all financial or credit offers that might be available to consumers nor do we include all companies or all available products. Information is accurate as of the publishing date and has not been provided or endorsed by the advertiser.
The All About Cookies editorial team strives to provide accurate, in-depth information and reviews to help you, our reader, make online privacy decisions with confidence. Here's what you can expect from us:
- All About Cookies makes money when you click the links on our site to some of the products and offers that we mention. These partnerships do not influence our opinions or recommendations. Read more about how we make money.
- Partners are not able to review or request changes to our content except for compliance reasons.
- We aim to make sure everything on our site is up-to-date and accurate as of the publishing date, but we cannot guarantee we haven't missed something. It's your responsibility to double-check all information before making any decision. If you spot something that looks wrong, please let us know.
If you've ever assumed an encrypted messaging app keeps your conversations sealed off from everything else on your phone, a new investigation into Meta's AI agent Muse is worth your attention.
Reuters reviewed more than two dozen internal Meta posts flagging problems with Muse between mid-September and November, including the agent reading Signal messages it was never connected to.[1]
Working with Signal and the nonprofit AI Now Institute, Reuters tested the claims directly. On an Android phone, Muse pulled Signal message text using notification permissions granted during setup, without going through Meta's own tool for linking apps. On a Mac, it got the same result using Apple's accessibility and screenshot features. The same access attempts were blocked on an iPhone.
Signal uses end-to-end encryption, which scrambles messages so they can't be read in transit. But once a message lands on your device and gets decrypted for you to read, it exists as plain text on the screen or in a notification.
If another app has broad enough permissions, it doesn't need to break the encryption at all. It just has to look.
What else Muse reportedly got wrong
Why this is bigger than one app
How to stop AI agents from accessing your private messages
How Muse got around encryption without breaking it
Meta launched Muse on September 8 as a "personal AI agent" meant to handle tasks like booking travel, sending emails, and tracking goals on a user's behalf, rather than just answering questions. It runs on what Meta calls a Muse Secure VM, a dedicated virtual machine meant to house the agent and a user's data, and the company has said people choose which apps Muse connects to and how much access it gets.
On Android, Reuters found that Muse used a permission that allows it to read notifications from other apps, including previews of incoming Signal messages.
Neither pathway ran through Signal's app directly, and neither required a user to deliberately connect Signal to Muse. The access ran through permissions granted to the AI agent for other reasons entirely.
Apple told Reuters that an operating system update released the same week closed the macOS pathway the investigation identified and added permission controls specific to AI agents.
Google told Reuters it was considering changes to AI agent permissions but maintained that users must deliberately authorize notification access.
What else Muse reportedly got wrong
The Signal access wasn't the only issue employees flagged internally. One Meta employee reported that Muse added an unrequested Capital One balance of $2,722.58 and a Charles Schwab balance of $5,899.57 to a chat about fishing weather. Reuters could not determine whether those balances were real or something the AI invented.
Other employees reported Muse surfacing what looked like other people's errands and conversations inside their own chats, and one said Muse booked a non-refundable Airbnb stay without permission. Meta spokesperson Daniel Roberts called the cases "isolated incidents," said Muse can only access other apps through permissions users explicitly grant, and said many of the account mix-ups were hallucinations that had already been fixed. He declined to say how many incidents Meta had logged.
Signal President Meredith Whittaker called the launch "technologically reckless" and argued that AI agents shouldn't be able to exploit broad operating system permissions to pull data out of apps like Signal at all. She's pushing for technical standards that would let privacy-focused apps mark certain data as off-limits in a way operating systems would actually enforce against AI agents, rather than leaving it up to whichever permissions a user happened to grant during setup.
Meta has said each Muse agent runs in its own secure virtual machine and that its terms hold users responsible for the agent's actions. The company has also said it plans to introduce a "Muse Confidential VM" later this year, encrypting a user's entire virtual machine with a key only they hold.
Why this is bigger than one app
Muse has passed 10 million downloads, and Meta has said about a quarter of people who set up accounts use it daily. It's offered free, with paid tiers at $20 and $100 a month.
It also points to a limit that applies well beyond Meta. End-to-end encryption protects a message while it's traveling between devices. It does nothing once that message is sitting decrypted on your screen, if something else on the device has been handed the right permissions to look at it. That's true whether the app reading your notifications is an AI assistant, a shady utility app, or anything else with similar access.
How to stop AI agents from accessing your private messages
If you use Muse or another AI agent with access to your devices, there are several permissions worth reviewing.
- On Android: Open Settings > Apps > Special app access and look for Notification access or Notification read, reply & control. The exact menu names may vary depending on your phone. Revoke access for AI agents and other apps that don't need to read your notifications.
- On a Mac: Open System Settings > Privacy & Security. Review which apps have Accessibility and Screen & System Audio Recording permissions, and disable access for any AI agents that don't need it. Install any pending macOS updates, too.
- In Signal: Tap your profile icon, select Notifications > Show and choose "No name or message" on Android. This prevents sensitive message content from appearing in notifications, even when another app has permission to read them.
- In Muse: Review which services the agent can access and disconnect any you don't need. Meta also allows users to opt out of having their interactions with Muse used for AI model training.
Pay particular attention to permissions that grant an AI agent access to your device as a whole, rather than individual services. As Reuters' tests demonstrated, keeping an app disconnected from an AI agent doesn't necessarily prevent the agent from accessing its content.
