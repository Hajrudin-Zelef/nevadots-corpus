---
id: collect-261121-veille-ia/gpt-openai/announcing-gpt-6-sol-and-gpt-6-luna-in-the-api-codex-and-chatgpt
title: "Announcing GPT-6 Sol and GPT-6 Luna in the API, Codex and ChatGPT"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "luna", "high", "pass", "gpt-5", "gpt-6-sol", "astra", "more"]
source: https://community.openai.com/t/announcing-gpt-6-sol-and-gpt-6-luna-in-the-api-codex-and-chatgpt/1399925
source_anchor: ""
source_lines: [1, 183]
section: "GPT_OpenAI"
---
Please welcome GPT-6 Sol and GPT-6 Luna to the GPT-6 universe.

GPT-6 Sol and Luna build on the advances behind GPT-6 Astra, bringing much of its strengths into faster and more affordable models to support work at scale.

We’ve also made caching and inference more efficient, and we’re passing the savings directly to you: 50% lower API prices for Sol and Luna compared with GPT‑5.6 promotional pricing.

Higher usage limits and lower cost give you more flexibility and room to iterate.

GPT‑6 Sol and Luna build on Astra’s advances in alignment, showing improvements over their GPT-5.6 counterparts.

GPT-6 Sol and Luna roll out today in ChatGPT Work and Codex for Plus, Pro, Business, Enterprise, and Edu users.

Both are also available in the API. Free and Go users can try GPT-6 Luna in the desktop app.

Updated Model Guidance for the GPT-6 model family

             
            
           
          
            
            
              If you like solely AutomationBench and what it measures, I made the gpt-6 models alone easier to read .. which required a few image edit turns to maintain any kind of fidelity in the values, and gpt-5.6-sol was left in, unlabeled.


Interpret it as gpt-6-sol max running its reasoning up to max actually making no headway in the problems and outputting worse for the increased cost.

For the actual vertical score, the models name increases seem to continue well at medium, continuing right where the xhigh or max of the prior size left off.

## 
GPT-6-Luna writes the remaining lines of my song chorus

You’re my cancer cat, through the fight,

**Your purr is a lantern, still burning bright;**

**You swat at the shadows, pounce on the worry, then zoom like a comet through one more long night,**

**And paw by brave paw, we keep clawing toward light.**

 
## 
GPT-6-Sol names the models that should come below Astra

(and this forum screws up the markdown paste)

**Astra → Stella → Stellula**

The names form a Latin-inspired progression: **Astra** evokes the stars, **Stella** means “star,” and **Stellula** means “little star.”


 
             
            
           
          
            
            
              Quick Codexometer Bench run, ranking with bias towards lower cost (actually, prioritising for speed doesn’t change the ranking!)

| RANK | MODEL | EFFORT | TASK | RESULT | TIME | TOKENS | API EQ | 
| #1 | GPT-6-Luna | HIGH | DEPENDENCY SCHEDULER | PASS | 27.4s | 15.4K | ~$0.0023 | 
| #1 | GPT-6-Luna | HIGH | VERSION RESOLVER | PASS | 16.5s | 14.7K | ~$0.0019 | 
| #1 | GPT-6-Luna | HIGH | EVENT PROCESSOR | PASS | 14.5s | 14.6K | ~$0.0018 | 
| #2 | GPT-6-Sol | HIGH | DEPENDENCY SCHEDULER | PASS | 41.7s | 16.6K | ~$0.0575 | 
| #2 | GPT-6-Sol | HIGH | VERSION RESOLVER | PASS | 15.8s | 14.8K | ~$0.0184 | 
| #2 | GPT-6-Sol | HIGH | EVENT PROCESSOR | PASS | 17.5s | 15.1K | ~$0.0204 | 
| #3 | GPT-5.6-Sol | HIGH | DEPENDENCY SCHEDULER | PASS | 1m08s | 17.1K | ~$0.1307 | 
| #3 | GPT-5.6-Sol | HIGH | VERSION RESOLVER | PASS | 11.7s | 14K | ~$0.0294 | 
| #3 | GPT-5.6-Sol | HIGH | EVENT PROCESSOR | PASS | 15.7s | 14.5K | ~$0.0363 | 
| #4 | GPT-5.6-Luna | HIGH | DEPENDENCY SCHEDULER | FAIL | 1m16s | 17.8K | ~$0.0097 | 
| #4 | GPT-5.6-Luna | HIGH | VERSION RESOLVER | PASS | 22.2s | 13.4K | ~$0.0043 | 
| #4 | GPT-5.6-Luna | HIGH | EVENT PROCESSOR | PASS | 34.9s | 14.6K | ~$0.0037 | 

 Looks like a win guys!  (but mainly because of that 50% reduction, though it appears GPT-6-Sol can be faster too)

(NB I reran the failed result for 5.6 Luna and it … failed again, so I’ve let that stand - looks like 6 Luna is a “better” model too)

             
            
           
          
            
            
              Lifesaver this evening. tyvm.

             
            
           
          
            
            
              i’m running on fumes right now.. reset did not come :-\ maybe just slow to reach me?

             
            
           
          
            
            
              @Joao_Martins

It’s a “banked reset” - you have the choice to apply it if you wish before it expires.

It’s not an across the board reset applied without user discretion.

             
            
           
          
            
            
              did not notice that. last time i checked was at 0 the available resets.. maybe changed when i had to update the app this morning. Thank you ^^

             
            
           
          
            
            
              Oh hope it comes through very soon.  Try logging out and back in?

             
            
           
          
            
            
              I do not have much to add beyond what @merefield already said.

I can keep an eye out for more reports from users who are not receiving the reset and escalate if needed.

Fingers crossed for you!

             
            
           
          
            
            
              The GPT-6 Sol price came as a shock—a good one, that is! 

             
            
           
          
            
            
              Good pricing, but when used in orchestration an Astra model is overkill and thinks too much for the role of orchestrator. Wasted 80% getting side tracked. Sol High is decent – but really needs more context window as the deepseek-flash Max as a orchestrator just works.

Sol High Orc → Luna worker → Astra re-worker → Astra Max reviewer seems to work well.

             
            
           
          
            
            
              Guys, the new models GPT-6 Luna and GPT-6 Sol and they’re both supposed to be smarter AND CHEAPER than GPT-5.6 Luna and GPT-5.6 Sol. NOT BRINGING THIS TO CHAT IS LITERALLY MORE EXPENSIVE FOR OPENAI AND WORSE FOR THE CONSUMER. THIS IS A LOSE-LOSE.

             
            
           
          
            
            
              astra takes a flame thrower to your credits, even luna admits that, and the new image gen is a 100% downgrade. repeats the same image over and over, clean new thread same thing. will acknowledge what your trying to do, and then does the complete opposite of what you and chat were describing, , i swear 5.4 was way better. im convinced these are not new models just small tweaks with a higher price…i hear good things about grok though. wasted my money here. nothing in these last updates make me want to stay

flagged my comment cause it hurt someones feelings? you cant be serious. “only possitive feedback allowed” lmao yup good bye gpt

             
            
           
          
            
            
              been playing around with this. reverse orchestration is kinda kicking ass.

SO now Luna Max Orc → Ask Sol 6.1 Max to review and create fix plan → Same Luna Orc implements fix → New Sol 6.1 Max reviews work → Same Luna fixes… when ready to merge commit to candidate Astra Max review tip (with fix plan if it finds an issue) and Luna fixes baed off plan then another astra review. then merge.

Much cheaper too.  I use this for post 80% complete work where Im debugging specific issues.

For a new build out I use the a team of teams swarm approach.
