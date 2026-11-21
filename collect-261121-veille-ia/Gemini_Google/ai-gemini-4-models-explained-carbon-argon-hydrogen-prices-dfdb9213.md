---
id: collect-261121-veille-ia/gemini-google/ai-gemini-4-models-explained-carbon-argon-hydrogen-prices-dfdb9213
title: "Ai gemini 4 models explained carbon argon hydrogen prices"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "models", "carbon", "tokens", "november", "argon", "hydrogen", "google"]
source: ""
source_anchor: ""
source_lines: [1, 54]
section: "Gemini_Google"
---
Google now lists prices for three Gemini 4 models on its own pages. Its Gemini API pricing page puts Gemini 4 Carbon and Gemini 4 Argon at $2 per million input tokens and $10 per million output tokens through 30 November and $4 and $20 starting 1 December, and Google Cloud’s SKU catalogue lists Gemini 4 Hydrogen, which Google launched on 12 November at those same rates, $1.50 and $7.50.
On a workload of 1 million input and 200,000 output tokens, Carbon or Argon costs $4.00 at today’s rates and $8.00 at the 1 December rates, level with Anthropic’s Claude Opus 5.5 at $8.00, by our arithmetic. Hydrogen’s listed rates come to $3.00, and Carbon’s 1 December input price is 40 times the $0.10 of Google’s new Gemini Decision 1 model.
Last updated: 12 November 2026, 18:30 UTC. Prices and availability as listed on Google’s pages on that date; this guide is revised when Google changes a price or adds a Gemini 4 model.
Business Pill · Tokens Explained
An evergreen Business Engineer explainer on tokens, the unit AI reads, writes and bills in. It teaches the general idea only and does not discuss Gemini.
The key insight: As we read it, Google prices its Gemini 4 line by date rather than by model: Carbon and Argon share $2 and $10 per million tokens through 30 November and $4 and $20 after that, so 1 million input and 200,000 output tokens go from $4.00 to $8.00 on either model, by our arithmetic.
The Four Models and What Google Says Each Is For
Gemini 4 Carbon. Google’s models page calls it “Our strongest reasoning model, built for complex software engineering, multimodal knowledge work, scientific reasoning, and cybersecurity defense”. Google announced Carbon on 28 October and said it would first go to “a subset of cyber defenders”; it made Carbon available on 10 November.
Gemini 4 Argon. The models page calls Argon “Our efficient Gemini 4 frontier reasoning model, delivering faster responses and lower token spend per turn compared to Gemini 4 Carbon”. Google launched Argon on 13 October, and its 10 November Carbon post says “Argon remains the ideal choice for the majority of software engineering and knowledge work, while Carbon is built for tasks that test the limits of a model’s intelligence”.
Gemini 4 Hydrogen (launched 12 November). Hydrogen appears as billing rows in Google Cloud’s public SKU catalogue: text input at $1.50 per million tokens, text output at $7.50, cached input at $0.075 and flex input at $0.75. Google’s Gemini API models and pricing pages and its Vertex AI pricing page had no Hydrogen entry when we checked at 10:31 UTC on 12 November. Update, 12 November, 16:15 UTC: a developer posted on X at 15:54 UTC a screenshot of what it presents as AI Studio code naming “models/hydrogen” and a gemini-4-hydrogen documentation address; at 16:10 UTC that address returned Google’s not-found page and the models page, pricing page and release notes still had no Hydrogen entry. Update, 12 November, 18:30 UTC: Google launched Hydrogen on 12 November. Its Gemini API pricing page lists it at $1.50 input and $7.50 output per million tokens, matching the catalogue rows, with a free tier; the model id is gemini-4-hydrogen and the context window 1,048,576 tokens. Our launch piece: Gemini 4 Hydrogen Launches at 2x Gemini 3.8 Flash’s Price. The Argon launch post ended: “We’re looking forward to bringing you the rest of the Gemini 4 family.”
Gemini Decision 1. Decision is not in the Gemini 4 list on the models page, which places it under specialised task models. Google’s 11 November post says the model scores candidate options “in a single forward pass, returning probabilities and confidence scores in 150-250 ms end-to-end latency at $0.10 per 1M input tokens with zero output tokens billed”.
Gemini 3.8 Flash. The tier below Gemini 4 on the pricing page is Gemini 3.8 Flash, “Our most intelligent Flash model”, at $0.75 input and $3.75 output through 31 December and $1.50 and $7.50 starting 1 January 2027, the same rates as Hydrogen’s catalogue rows.
Context Windows and Where Each Model Runs
Google’s model pages give Carbon and Argon a context window of 1,048,576 tokens shared by input and output, with a maximum output of 65,536 tokens by default and up to 1,048,576 with long decode. Gemini 3.8 Flash takes 1,048,576 input tokens and returns up to 65,536. Gemini Decision 1 takes up to 262,144 input tokens, returns zero output tokens and answers up to 1,000 questions per request. The Hydrogen catalogue rows we read list prices, not a context window.
Google says Carbon is available through the Gemini API in Google AI Studio and Gemini for business “for all paying developers and enterprises”, and is rolling out to Google AI Ultra subscribers in Google Antigravity, the Gemini app and AI Mode in Search. The pricing page says Carbon “is not available on the Free tier”.
Argon’s launch post put it in the Gemini API paid tier and, for consumers, in the Gemini app and AI Mode in Search for anyone who is “a Google AI Ultra or Pro subscriber”. Decision runs through the Gemini API in Google AI Studio; the pricing page lists its free tier as “Free of charge” and says the Batch API “isn’t supported”.
On Google Cloud’s Vertex AI pricing page, Carbon and Argon carry the same global rates, and non-global rates of $2.20 and $11 through 30 November and $4.40 and $22 from 1 December, 10% above global, by our arithmetic. The page says the introductory pricing is “provided through 50% credits back on net spend on select models within a given period”.
What Each Model Costs on One Workload
We priced one fixed job: 1 million input tokens and 200,000 output tokens, for example 100 requests of 10,000 input and 2,000 output tokens each. The cost is the input price plus 0.2 times the output price, at standard paid rates for short prompts, with no caching, batch or regional adjustments.
By our arithmetic, Carbon or Argon costs $4.00 through 30 November and $8.00 from 1 December. Hydrogen’s catalogue rates come to $3.00. Gemini 3.8 Flash costs $1.50 through 31 December and $3.00 from 1 January 2027.
Anthropic’s pricing page lists Claude Opus 5.5 at $4 input and $20 output, which comes to $8.00 on the same workload; OpenAI’s pricing page lists GPT-6.1 Sol at $2.00 and $10.00, which comes to $4.00, by our arithmetic. Carbon and Argon cost the same as Sol today and the same as Opus 5.5 from 1 December, when they cost twice Sol. Hydrogen’s listed rates are 75% of Sol and 37.5% of Opus 5.5.
Cached input changes the totals. The pricing page lists Carbon and Argon context caching at $0.10 through 30 November and $0.20 from 1 December, 5% of the input price; Hydrogen’s cached row is $0.075, also 5% of its input price; Gemini 3.8 Flash caching is $0.075, 10% of its input price, by our arithmetic.
The Price Ladder, From Decision to Carbon
Ranked by paid input price per million tokens, Google’s ladder runs from Gemini Decision 1 at $0.10 to Gemini 3.8 Flash at $0.75, Hydrogen’s listed $1.50, Carbon and Argon at $2.00 and Carbon and Argon at $4.00 from 1 December. As multiples of Decision’s price, that is 7.5 times, 15 times, 20 times and 40 times, by our arithmetic.
On the full workload, Carbon at its 1 December rates costs about 2.7 times Hydrogen’s listed rates and 5.3 times Gemini 3.8 Flash at its current rate, by our arithmetic. Decision is priced for a different job: it answers yes/no, choice and rubric questions and bills no output, so it has no output price to compare.
More on this. Per task, the order changes. On the Artificial Analysis leaderboard, Hydrogen (high) scores 51 at $1.87 per index task against $7.77 for Gemini 3.8 Flash (high), about 76% less by our arithmetic: Gemini 4 Hydrogen Costs 76% Less per Task Than 3.8 Flash. On Arena’s Agent Arena, Hydrogen ranks fifth, six places above Argon, but its median task costs $1.62 against Argon’s $0.71: Gemini 4 Hydrogen Is 5th on Agent Arena but Costs 2.3x Argon.
Our Coverage, Model by Model
Hydrogen: the catalogue listing at $1.50 and $7.50, its $3 workload against 13 rival models and the earlier report that Google was testing it.
Carbon: the 28 October announcement and benchmark claims, its $4 and $20 rows in Google’s cloud catalogue, its 10 November launch at $2 and $10 and Perplexity’s cost-per-task test.
Argon and the wider catalogue: Argon’s global launch on 13 October and our count of Gemini model launches with prices. Decision: the Gemini Decision 1 launch and what its saving over Flash-Lite depends on.
The Structural Read
As we read Google’s pages, Carbon and Argon differ in token use rather than in rates: the per-token prices are identical, and Google’s 10 November post says Carbon works by using more tokens than Argon to explore thoroughly.
As we read the catalogue, Hydrogen’s listed $1.50 and $7.50 are the same rates that Google’s pricing page sets for Gemini 3.8 Flash starting 1 January 2027.
Google Gemini API pricing page, on Gemini 4 Carbon
“Every token the model processes is counted and billed, including thinking tokens and internal tool system instructions and function declarations when using grounding.”
Three Implications
FOR DEVELOPERS ON CARBON OR ARGON Google’s pricing page lists both at $2.00 input and $10.00 output through 30 November and $4.00 and $20.00 starting 1 December.
FOR TEAMS COMPARING LABS On our workload Carbon and Argon cost $4.00, the same as GPT-6.1 Sol, and $8.00 from 1 December, the same as Claude Opus 5.5, by our arithmetic.
FOR ROUTING AND CLASSIFICATION JOBS Google prices Gemini Decision 1 at $0.10 per million input tokens with zero output tokens billed.
The Business Engineer Lens
This story maps onto the Business Engineer framework The CFO’s Guide to the Token Economy.
The guide says: “The new job of finance is to manage the ratio between the two — work produced per token burned.”
As we read it, Google’s price ladder is the cost side of that ratio: the same workload costs from $1.50 on Gemini 3.8 Flash to $8.00 on Carbon at its 1 December rates, by our arithmetic. The guide does not discuss Gemini 4.
What Is Not Established
Google launched Hydrogen on 12 November at the catalogue rates; its benchmark figures are Google’s own and were not independently checked for this guide.
Per-token prices are not per-task prices. Google’s pricing page says output prices include thinking tokens, and its 10 November post says Carbon works “using more tokens than Argon to explore thoroughly, reason deeply, and run extended verification loops”, so the two can cost different amounts for the same job at the same rates. We did not test any of the models.
The Bottom Line
Gemini 4 Carbon and Argon share one price on Google’s pricing page: $2 and $10 per million tokens through 30 November, then $4 and $20. On 1 million input and 200,000 output tokens that is $4.00, then $8.00, level with Claude Opus 5.5, against $3.00 for Hydrogen’s listed but unannounced rates, by our arithmetic.
95,000+ executives read Business Engineer for the AI strategy frameworks cited by ChatGPT, Claude, and Perplexity.
A note on sourcing. Model roles, context windows, availability and prices come from Google’s own pages, read on 12 November 2026: the Gemini API models and pricing pages, the model pages for each model, the Vertex AI generative AI pricing page, and Google’s launch posts for Argon (13 October), Carbon (28 October and 10 November) and the Decisions API (11 November). Hydrogen’s rates are the text rows of Google Cloud’s public SKU catalogue as we read them at 10:32 UTC; Google launched the model on 12 November at the same rates, per its blog and Gemini API pricing page.
Claude Opus 5.5 and GPT-6.1 Sol prices are from Anthropic’s and OpenAI’s pricing pages, read the same day. Workload totals, ratios and multiples are our arithmetic. We did not test the models, and Google was not contacted for this piece.
Read the full analysis
We publish 12+ AI analyses every day — sourced, structured, no hype. Unlock this one and every story we break. Plus The New Roles of the Agentic Era (full book) free.
Instant access. No spam. Join 95,000+ operators.
