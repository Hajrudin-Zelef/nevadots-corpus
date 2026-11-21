---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-flash-cheap-tokens-expensive-minutes-memeburn
title: "GLM-5.3-Flash: Cheap Tokens, Expensive Minutes - Memeburn"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["glm-5", "flash", "tokens", "model", "million", "output", "cheaper", "intelligence"]
source: https://memeburn.com/glm-5-3-flash-cheap-tokens-expensive-minutes/
source_anchor: ""
source_lines: [1, 57]
section: "GLM_Zhipu"
---
TL;DR

- Z.ai released GLM-5.3-Flash on 26 August, a 320B-parameter (18B active) multimodal MoE with a 1M-token context, MIT-licensed weights, and a launch price of $0.15 per million input tokens and $0.50 per million output.
- Against the flagship GLM-5.3, that is roughly nine times cheaper on input and nearly nine times cheaper on output, and the model is genuinely smart, scoring 57 on Artificial Analysis's Intelligence Index versus GLM-5.3's 60.
- The catch is speed. Flash generates at about 49 tokens per second while GLM-5.3 runs near 88, so the budget "Flash" model is actually slower than the flagship it undercuts.
- It is also verbose, burning far more tokens to finish the same task, so you have a slow model producing more output, and that is where the cheap tokens quietly turn into expensive minutes.
- Use it where wall-clock time does not bill, like batch and async jobs; think twice where latency matters, like interactive coding agents.

Z.ai has a habit of resetting the price of intelligence, and GLM-5.3-Flash is its most aggressive move yet. Released on 26 August after a week running anonymously as the “Ox Alpha” stealth model on OpenRouter, it is the first model in the GLM-5 family built multimodal from the ground up: a 320-billion-parameter mixture-of-experts with 18 billion active, a one-million-token context window, and, in classic Z.ai fashion, open weights under the MIT license. The launch coverage has been near-unanimous in its enthusiasm, and most of it is deserved. But almost all of it has skipped the one number that matters most in production, and that omission is the whole reason for this piece.

## **The cheap tokens are real**

Let me give Flash its due first, because the value is not in doubt. At list price it charges $0.15 for a million input tokens and $0.50 for a million of output, with a 50% launch promo, running until 9 September, that cuts those rates to $0.075 and $0.25. Set against Z.ai’s own flagship GLM-5.3, which lists at $1.40 and $4.40, Flash is roughly nine times cheaper on input and about nine times cheaper on output. That is the baseline worth locking, because you will also see a “six times cheaper” figure floating around, and that one refers to the total cost of running Artificial Analysis’s Intelligence Index, roughly $138 on Flash against about $843 on the older GLM-5.2 (max), a different and earlier comparison entirely.

None of this cheapness comes at the expense of raw ability. Flash scores 57 on Artificial Analysis’s Intelligence Index, only three points behind GLM-5.3’s 60 and far above the median for its class, and Z.ai says it trails Claude Opus 4.8 by under half a point on the lab’s own coding test while outscoring the older GLM-5.2 across the board. On paper, then, this looks like most of the flagship’s brains at a ninth of the token cost. If price and intelligence were the only axes that mattered, the review would end here with a straightforward recommendation.


## **The expensive minutes**

They are not the only axes, and this is where the launch posts go quiet. Flash generates output at roughly 49 tokens per second, which Artificial Analysis rates as notably slow, sitting well below the 67-token-per-second median for models of its size. The flagship GLM-5.3, by contrast, pushes around 88 tokens per second on the same benchmark. Read that again, because it is the crux of the whole model: the cheaper, budget-branded “Flash” is close to half the speed of the premium model it is meant to undercut. A name that promises quickness delivers the opposite.

Speed alone would be manageable if Flash were terse. It is not. The same benchmark clocks it as very verbose, generating around 150 million tokens to complete the Intelligence Index against a median of 110 million, which means it does not just write slowly, it writes more. Stack those two traits together and the arithmetic turns ugly for anything time-sensitive: a model that produces extra tokens, and produces each of them slowly, takes disproportionately longer to finish a real task. Your token bill falls by a factor of nine. Your wall-clock time moves in the wrong direction. That trade is the entire story, and it is the one the price tag hides.

One fair caveat before anyone accuses me of cherry-picking. Throughput shifts with the provider, the workload and the measurement harness, and different trackers have clocked GLM-5.3 anywhere from the high eighties into the triple digits, so treat the exact ratio as directional rather than gospel. The direction, though, is not in dispute. Every serious measurement puts Flash at the slow end of its class.

## **So who is it actually for?**

Here is the part the more breathless takes miss in the other direction, because slow is not the same as useless. Flash’s time to first token is genuinely quick at about 1.52 seconds, better than the class median, so the model starts responding fast and only then crawls. That profile, quick to start and cheap to run but slow and wordy to finish, maps cleanly onto a specific kind of work. If you are running large batch jobs, overnight document processing, bulk code refactors or any asynchronous pipeline where nobody is watching a cursor blink, Flash is close to ideal, and its hybrid sparse-and-linear attention is explicitly tuned to keep million-token context cheap rather than to win a drag race. Z.ai built a marathon runner and called it a sprinter.

Where it hurts is the opposite case. If you are wiring Flash into an interactive coding agent, a chat product or anything a human waits on in real time, the low token price will not save you, because the person on the other end is paying in seconds, not cents. In those loops throughput is the product, and Flash is not built to compete on it.


## **The verdict**

GLM-5.3-Flash is a genuinely strong, genuinely cheap model with one clearly defined weakness, and the mistake would be reading its price tag as the whole story. Cheap tokens are real. So are expensive minutes. Match it to work where minutes do not bill and it is one of the best value models on the market. Point it at anything latency-sensitive and you will spend in time what you saved in tokens. The number to remember is not the price. It is the 49 tokens per second underneath it.

## **FAQs**

### **Is GLM-5.3-Flash actually fast?**

No. Despite the “Flash” name, it generates around 49 tokens per second, which independent testing rates as notably slow, and it is roughly half the speed of the flagship GLM-5.3.

### **How much cheaper is GLM-5.3-Flash than GLM-5.3?**

About nine times cheaper on input ($0.15 versus $1.40 per million tokens) and nearly nine times cheaper on output ($0.50 versus $4.40), before a 50% launch promo running to 9 September.

### **Is GLM-5.3-Flash open source?**

The weights are released under the MIT license on Hugging Face, which allows free commercial use. Whether that counts as fully “open source” depends on training-data disclosure, which is a separate question.

### **What is GLM-5.3-Flash best used for?**

Cost-sensitive, high-volume and asynchronous work such as batch processing, long-context document jobs and offline code tasks, where slow generation does not block a waiting user.

### **Why is GLM-5.3-Flash so verbose?**

It generates more tokens than most peers to finish the same task, which compounds its slow speed and partly offsets the low per-token price in real workloads.
