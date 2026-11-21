---
id: collect-261121-veille-ia/muse/muse-reads-encrypted-messages-0d8a374a
title: "Muse reads encrypted messages"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "signal", "text", "access", "android", "notification", "message", "messages"]
source: ""
source_anchor: ""
source_lines: [1, 17]
section: "Muse"
---
Meta’s AI agent Muse read Signal message text on Android phones and Macs without directly connecting to the encrypted messaging app, Reuters reported on November 18.
The investigation found that Muse could use Android notifications and macOS permissions to see text already displayed on a device. The tests did not show that Muse broke Signal’s end-to-end encryption.
The finding followed an internal complaint from a Meta employee who said Muse unexpectedly reminded her to reply to a Signal message from her sister. The employee was using a personal phone and said she did not recall granting Muse access to other apps. A colleague said he traced similar reminders to Android notification access.
The complaints surfaced after Muse, represented by a fuzzy mascot named Jolly, passed 10 million downloads following its September launch.
Employees also reported unsolicited messages about childcare, career advice, and purchases they had not made. In one case, a routine update about fishing conditions included two bank balances the recipient did not recognize.
Muse Read Signal Messages on Android and Mac
On Android, Muse read message text from push notifications. Signal encrypts messages between devices, but a notification preview can display the text after it arrives.
An app with notification access can see what appears in those alerts. That is different from opening Signal, reading its full chat history, or breaking its encryption.
On macOS, testers found that Muse could use accessibility and screenshot tools to read Signal text. Neither the Android nor the Mac test required adding Signal through Meta’s Connector feature, which links external services to the assistant. The same request was blocked on the iPhone used in testing.
The Reuters investigation highlights a distinction users can easily miss: leaving Signal off an agent’s list of connected apps does not revoke permissions granted through the operating system.
Cybersecurity News previously covered a separate Apple notification privacy flaw that exposed Signal message text through a different route. In both cases, the exposure involved text the device had already received.
Meta said Muse can access other apps only through permissions users explicitly grant. The company described the reported cases as isolated and said many suspected account mix-ups were AI hallucinations, meaning the assistant generated information that was not based on real records. The tests confirmed that Muse could read Signal text, but they did not establish that the bank figures belonged to another user.
Mac users can check System Settings under Privacy & Security and remove Muse’s accessibility or screen recording access if they do not need those features.
Cybersecurity News’ coverage of macOS 27.3 beta privacy controls explains why existing grants deserve a separate review: tighter controls do not necessarily remove access an app already has.
Android users can search system settings for “Notification access” and turn off Muse’s access if it is enabled. They can also follow Signal’s notification settings guide to hide message text in alerts.
The two changes do different things: hiding previews limits what Signal puts in a notification, while revoking access stops Muse from reading notifications through that permission. Neither requires turning off Signal notifications altogether, so users can still receive alerts without showing the message text.
What Are Leading Organizations Doing About AI and Cyber Risks? Find Out. View Details!
