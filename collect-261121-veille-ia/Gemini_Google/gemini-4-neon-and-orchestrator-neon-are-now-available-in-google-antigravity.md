---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-and-orchestrator-neon-are-now-available-in-google-antigravity
title: "Gemini 4 Neon and Orchestrator (Neon) are now available in Google Antigravity"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["neon", "antigravity", "thinking", "orchestrator", "gemini", "google", "plan", "included"]
source: https://discuss.ai.google.dev/t/gemini-4-neon-and-orchestrator-neon-are-now-available-in-google-antigravity/202301
source_anchor: ""
source_lines: [1, 76]
section: "Gemini_Google"
---
Hi everyone,

Gemini 4 Neon is rolling out today in the Antigravity app, Antigravity CLI, and Antigravity IDE Extensions. Neon sits between Gemini 4 Carbon (deepest long-horizon work) and Gemini 4 Hydrogen (fast everyday execution), and it becomes the default model for new conversations on plans that include it.

**Why try Neon**

- Built for daily coding and agent work: repo-level edits, debugging, refactoring, and multi-step instructions, with the lowest fabrication rate we have measured in Antigravity testing.
- #1 on the Artificial Analysis Coding Agent Index (58.6) and #3 overall on the Artificial Analysis Intelligence Index (61, at Extra High).
- At its default High thinking level, about 32% lower cost per task than Carbon, so your quota goes further.
- Four thinking levels: Low, Medium, High (default), and Extra. In the CLI: `--model gemini-4-neon-xhigh` or`--effort xhigh` .
- Context window of up to 512K tokens in Antigravity.

**Orchestrator (Neon)**

Orchestrator pairs two Gemini 4 models in one conversation. With Orchestrator (Neon), Neon plans and reviews, and Hydrogen does the implementation work: code edits, package updates, and test runs. If Hydrogen gets stuck, it hands control back to Neon to inspect the state, fix the plan, and continue. You get Neon’s judgment where it matters and Hydrogen’s speed everywhere else. Select **Orchestrator (Neon)** in the model selector under the prompt box.

**Who gets what**

| **Plan** | **Gemini 4 Neon** | **Orchestrator (Neon)** | 
| Free and Google AI Plus | Not included | Not included | 
| Google AI Pro (paid) | Included | Included | 
| Google AI Pro (trial) | Only on trials with eligible premium devices | Only on trials with eligible premium devices | 
| Google AI Ultra | Included | Included | 
| Enterprise | Included through Gemini for business | Not included | 

 If you are on a Google AI Pro trial and want to switch to a paid plan, you can submit a request. The full model and plan table is on the Choose a model page.

Read more in the launch post. If you hit a problem with Neon or Orchestrator, reply in a new topic with your Antigravity version, plan, and the model you selected so we can follow up.

Happy building, The Google Antigravity team

             
            
           
          
            
            
              @rody Hey Rody, a lot of us paid Ultra users have been stuck since 15 to 18 Nov and haven’t heard anything back yet, so posting here.

Neon doesn’t run for us either. On Neon (XHigh) the first message fails with:

INVALID_ARGUMENT (code 400): Thinking level is unsupported: THINKING_LEVEL_XHIGH

Same story on Carbon, Argon and Hydrogen:

- answers in seconds with almost no thinking
- long chats cut to about 100K, the older part just dropped, no summary
- the app log shows the plan as empty:
 SetUserTier called with userTier: “”, tierDisplayName: “”
CCPA routed this account to AICode
- updating to 2.33.0 didn’t fix it

For me it switched on 18 Nov at 04:43 UTC, right after my 5-hour limit reset. Every chat changed in the same minute. Before that the same chats ran to 511K with full thinking.

Everyone’s reports are in these two threads:

Our Ultra plans are just sitting there unused. Would really appreciate if you can get it checked.

             
            
           
          
            
            
              Hi @Devjay,

Thanks for flagging this here and linking both threads.

Several users in those threads have since confirmed normal thinking and `Extra`/Orchestrator behavior returning later on November 19, so please re-test in a fresh conversation on Antigravity App v2.33.0 (and try signing out and back in per Authenticate if you haven’t since then).

If you are still seeing the `THINKING_LEVEL_XHIGH` error, shallow responses, or early context cutoff on your Ultra account:

- **Interim fallback:** Switch to**Claude Sonnet 5.5 (thinking)** or**Claude Opus 5.5 (thinking)** (Models).
- **Long chats:** Use`/plan` (Plan) for reviewable plans and workspace rules like`AGENTS.md` or`.agents/rules/` (Rules) so key instructions stay intact across fresh conversations.

Please reply here and let us know whether this is still happening after re-testing so we can look into it further.
