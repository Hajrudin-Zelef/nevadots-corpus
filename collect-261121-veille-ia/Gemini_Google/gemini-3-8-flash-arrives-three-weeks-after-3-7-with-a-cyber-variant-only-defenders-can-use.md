---
id: collect-261121-veille-ia/gemini-google/gemini-3-8-flash-arrives-three-weeks-after-3-7-with-a-cyber-variant-only-defenders-can-use
title: "Gemini 3.8 Flash Arrives Three Weeks After 3.7, With a Cyber Variant Only Defenders Can Use"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["flash", "gemini", "google", "more", "tokens", "model", "cyber", "three"]
source: https://www.webpronews.com/googles-gemini-3-8-flash-signals-relentless-pace-in-ai-race/
source_anchor: ""
source_lines: [1, 21]
section: "Gemini_Google"
---
Three weeks after Gemini 3.7 Flash, Google has shipped Gemini 3.8 Flash at the same introductory price of $0.75 per million input tokens and $3.75 per million output tokens, along with a security-tuned variant it is giving only to vetted defenders.

It is Google’s third Flash release in six weeks, according to the announcement from Tulsee Doshi, senior director of product management, and Raluca Ada Popa, Gemini Security Lead at Google DeepMind. “Gemini 3.8 Flash delivers substantial gains from 3.7 Flash, often approaching the performance of higher-cost frontier models,” they wrote. The introductory pricing runs through Dec. 31, and rates rise to $1.50 and $7.50 per million tokens on Jan. 1.

## Better scores, more tokens

Google says 3.8 Flash beats most larger frontier models on DeepSWE v1.1, a long-horizon software engineering benchmark, and outperforms 3.7 Flash on Vals Finance Agent V2 and Harvey’s Legal Agent Benchmark. It scored 54.9% on HLE-Verified.

Independent numbers are a bit more measured. Set to high reasoning, 3.8 Flash scores 59 on the Artificial Analysis Intelligence Index, three points above its predecessor and level with GPT-5.6 Sol and Grok 4.6, The Register reported. Claude Opus 5 (63) and Claude Fable 5.1 (66) still rank higher. At $0.58 per Intelligence Index task, Artificial Analysis called it “the cheapest model at its level of intelligence,” about one-sixth the per-task cost of Fable 5.1.

There is a cost catch. The Register found 3.8 Flash costs about 40% more per task than 3.7 Flash because it outputs more tokens and takes more turns as an agent. Google acknowledges this. “3.8 Flash works harder,” Doshi and Popa wrote, adding that the model “might use more tokens to maximize performance, especially at higher effort levels.” Developers can choose lower effort levels or stay on 3.7 Flash, which remains supported.

## A Cyber model for defenders

Gemini 3.8 Flash Cyber targets vulnerability discovery and automated patching and is available only through a new Fairwind Program for government authorities, critical infrastructure operators and software maintainers. Google says it prioritized fixing over offensive capabilities such as exploitation.

On the CWE-Bench patching benchmark run by Collinear, the Cyber model scored 47.2% pass@1 against 47.8% for an unnamed leading frontier model. Inside Google, the Chrome Security team found it produced 2.6 times more correct patches for Chrome vulnerabilities than the best, much larger commercial models, and the Cloud Vulnerability Research team used it to find a critical vulnerability in under two hours. Wiz reported 7.5% to 9.7% higher recall on its internal penetration-testing benchmark at 2.3 to 5.2 times lower cost.

The standard 3.8 Flash is available in Google Antigravity, the Gemini API through AI Studio and Android Studio, Stitch and Gemini Enterprise. Google AI Pro and Ultra subscribers get it in the Gemini app, AI Mode in Search and Gemini in Google Sheets.

The release also lands while Google is still missing a bigger model. The Register noted that Gemini 3.5 Pro did not arrive in June as promised, and that Demis Hassabis stepped down as Google DeepMind CEO in early August to become chairman.
