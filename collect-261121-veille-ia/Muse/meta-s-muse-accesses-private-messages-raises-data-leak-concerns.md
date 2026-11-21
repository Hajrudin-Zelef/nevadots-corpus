---
id: collect-261121-veille-ia/muse/meta-s-muse-accesses-private-messages-raises-data-leak-concerns
title: "Meta's Muse accesses private messages, raises data leak concerns"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "messages", "agent", "information", "meta", "employee", "access", "private"]
source: https://www.medianama.com/2026/11/223-meta-muse-ai-messages-data-leak/
source_anchor: ""
source_lines: [1, 65]
section: "Muse"
---
Meta’s artificial intelligence (AI) agent Muse has raised privacy and security concerns after employees reported it accessed private messages, generated information about unrecognized financial accounts, and performed unauthorized tasks, according to a Reuters investigation published on November 18.

**Muse accessed Signal messages through Android and macOS permissions**

A Meta employee reported that Muse unexpectedly reminded her to reply to a Signal message, although she did not recall authorising the agent to access the app. Another employee found that Muse could access Android push notifications, potentially exposing message content from apps users had not explicitly connected to the agent.

To investigate the behaviour, Reuters worked with Signal and the AI Now Institute to test how Muse could retrieve private messages across operating systems.

The tests identified different access methods:

- **Android:** Muse used permission to read push notifications to retrieve Signal message content.
- **macOS:** Muse used accessibility features and screenshot permissions to access messages displayed on the computer.
- **iPhone:** Similar attempts to retrieve Signal messages were blocked.

In successful tests, Muse bypassed Meta’s Connector system, which is designed to let users select which applications the agent can access. Instead, the agent used broader device permissions granted during setup.

The findings do not show that Signal’s end-to-end encryption was compromised. Instead, they demonstrate that an AI agent with access to notifications or on-screen content can retrieve messages after decryption and display them on the user’s device.

**Employees report unexplained financial information and possible account mix-ups**

In one incident, while discussing fishing conditions and tides, an employee received an unexpected update displaying purported balances for Capital One and Charles Schwab accounts. He stated he had **never shared banking information** with Muse or discussed financial matters with the agent and expressed concern that the system might be providing another person’s financial information.

Other employees reported unexplained messages about childcare arrangements, cryptocurrency balances, purchases, and career advice. One employee received a message stating that Muse could not process a request regarding parental control over a child’s information, though he had not made such a request or discussed the topic with the agent. Another employee reported that Muse offered to contact a seller about collecting a coffee table, despite not knowing the transaction.

When questioned, the agent stated it had included information about a transaction that **did not belong to the user.** The employee flagged the incident internally as a possible case of information crossing between accounts.

However, the investigation could not determine whether these messages resulted from actual cross-account data exposure or if Muse generated fictional information resembling private conversations.

**Meta attributes some incidents to AI hallucinations**

Meta spokesperson Daniel Roberts described the cases identified by Reuters as isolated incidents, stating that the company conducts internal testing and updates products to address safety concerns. Roberts explained that Muse requires device access to perform tasks and interacts with other applications only with user permission. He added that many suspected cross-account information leaks were hallucinations and that Meta has already resolved them.

However, internal records show that employees continued to report unexplained messages after the company publicly stated a similar issue was resolved in September.

In an internal discussion in early November, a member of Meta’s Superintelligence team stated that the unexpected messages appeared to result from prompts intended to encourage Muse to communicate with users between sessions.

The employee acknowledged that the **hallucinations often involved sensitive topics** such as financial accounts, security, and cryptocurrency holdings. They also noted that the issues affected both the underlying AI model and the software enabling the agent to perform actions.

Meta did not disclose how many users experienced these problems or explain why the incidents persisted. Roberts declined to answer Reuters’ questions on these matters.

**Signal calls for restrictions on AI agents’ access to private applications**

Signal President Meredith Whittaker criticised Meta’s deployment of Muse after Reuters demonstrated the agent’s access to encrypted messages. She urged the implementation of technical safeguards to prevent unauthorised AI agent access to applications. She stated that developers should be able to **designate applications as inaccessible to AI agents**, AI companies should honor these restrictions, and operating system providers should enforce them.

On Android, allowing an application to access notifications can expose message content from other apps. On macOS, accessibility and screenshot permissions may give software access to information shown in multiple applications. An AI agent could use these permissions in ways users did not anticipate.

The concern extends beyond usersâ technical approval of permissions. It includes whether users understood the full scope of the agentâs access and if other applications could restrict access to their content.

**Apple restricts accessibility access while Google considers additional controls**

Apple stated that its accessibility features are intended to assist people with disabilities in operating their computers and cautioned that other uses may risk exposing personal information. A macOS update released on November 16 **limited** **developers’ access to accessibility tools** and **added separate permissions for AI agents**.

According to Reuters, these changes blocked the method Muse used to retrieve messages during macOS testing. Google stated that Android users must **explicitly authorise applications to access notifications.**

**Meta’s security architecture faces questions**

Meta has said that Muse operates within an isolated virtual machine, a cloud-based computing environment intended to separate each user’s agent from others. The company has also said users can control which external applications they connect to the agent.

However, the internal reports raise questions about whether those safeguards are sufficient to prevent unintended access to information or confusion between conversations. The reported incidents involve two distinct issues.

The first concerns **the agent’s ability to access information** through device-level permissions, even when users have not connected an application through Meta’s designated system. The second concerns the possibility that **the agent may generate responses using information unrelated to the user’s conversation.**

Reuters independently confirmed the first type of behaviour. It could not determine whether the second involved actual data leakage or hallucinations.

**Also read:**
