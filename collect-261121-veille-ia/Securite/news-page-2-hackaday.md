---
id: collect-261121-veille-ia/securite/news-page-2-hackaday
title: "News – Page 2 – Hackaday"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["decision", "linux", "using", "llm-based", "news", "page", "hackaday", "kernel"]
source: https://hackaday.com/category/news/page/2/
source_anchor: ""
source_lines: [1, 7]
section: "Securite"
---
Since we last covered the way that the Linux Kernel project has been dealing with the influx of code created using LLM-based coding assistants, all the way back in April, things have changed a lot, with most recently the Linux maintainers deciding to fully drop the requirement to mark LLM-assisted commits with the `Assisted-by` tag.

While not a fully uncontroversial decision – as also reported by Phoronix – with key maintainers already having made this decision it seems certain to proceed. This is not the only change made fairly recently, with a ‘Sashiko’ bot introduced that is supposed to handle code reviews.

In a July LKML post [Linus Torvalds] defends this decision, with a follow-up post in August by [Jakub Kicinski] it being stated that budget has been secured for using Meta’s LLMs, using said LLM-based resources for reviewing patches, as well as creating and editing commit messages. As noted by [Jakub] they’re running multiple frontier models on each patch to try and weed out confabulations.

Whether this will prove to be the right decision remains to be seen. Whereas most open source projects have firmly banished or heavily curtailed LLM-based contributions on slop, copyright as well as licensing grounds, the Linux Kernel project may be an interesting experiment in what the outcome of the exact opposite approach will be.
