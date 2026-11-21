---
id: collect-261121-veille-ia/securite/black-october-why-nine-ai-developer-platforms-lost-customers-secrets-in-one-month
title: "Black October: Why Nine AI Developer Platforms Lost Customers' Secrets in One Month"
domain: securite
role: reference
task: article
actors: []
dates: ["2026-10-30"]
keywords: ["october", "nine", "report", "disclosed", "month", "environment", "variables", "access"]
source: https://dev.to/rbonweb/black-october-why-nine-ai-developer-platforms-lost-customers-secrets-in-one-month-4nj3
source_anchor: ""
source_lines: [1, 95]
section: "Securite"
---
Black October is the name Wiz Research gave its October 30, 2026 report after counting at least nine AI developer platforms that disclosed security incidents in a single month, each one hunting the same thing: environment variables holding AI provider keys. No shared vulnerability connects all nine. The shared target is what makes this worth examining as a pattern, not nine unrelated stories.

Jump to what one stolen key actually bought an attacker if you want the financial scale first.

According to Bromure's timeline, built from Wiz's report, the nine disclosures ran as follows. Fireworks AI disclosed on October 6, a credential-access incident with signs of what Wiz calls an "agent swarm" attack pattern. Andromeda followed on October 9, described as part of an "industry-wide attack." Zeabur disclosed on October 14, credential access, ahead of its later shutdown announcement.

Lightning AI disclosed credential access on October 15. OpenCode's disclosure spanned October 16-17, tracing to a "legacy system" that exposed stored provider keys. Railway disclosed privilege escalation in its build infrastructure on October 20. Warp, on October 23, pointed to a third-party vendor and reported agent traces taken. CommandCode, on October 28, attributed its incident to model distillation. Northflank closed out the month on October 30, telling customers to assume environment variables were fully decrypted.

Towards AWS's analysis of the Wiz report is explicit that the nine incidents share "different root causes where any are known, credential access, build infrastructure privilege escalation, a vendor dependency, a legacy system, but the same objective: stored environment variables, especially the ones that grant AI access." Most of the nine, including Northflank, still have not published a confirmed root cause at all.

The financial scale is the part that turns this from a compliance footnote into an active market. FourWeekMBA reports that Wiz traced a single stolen enterprise Amazon Bedrock key to approximately 1.7 million API calls in thirteen hours, about 36 calls a second sustained for half a day before anyone intervened. Across two documented Bedrock cases, Wiz counted roughly 2.5 million combined calls and more than $100,000 in usage at list price, per teiss's reporting.

This pattern has a name: LLMjacking, using stolen AI credentials to reach models on someone else's paid account, either to run personal workloads for free or, increasingly, to resell that access. Bromure's report states Wiz observed stolen keys from these breaches tested within thirty minutes of exposure, then resold into pools whose outbound traffic resembles ordinary coding-agent usage, which is precisely what makes the abuse hard to distinguish from a legitimate customer's own spike in AI usage.

Railway's own incident disclosure, as quoted across multiple outlets, described "potential privilege escalation in our build infrastructure" that led to "an unauthorized query of encrypted environment variables," a subset of which the attacker successfully decrypted. Railway estimated the exposure at "at most 1.85% of all workspaces," roughly 100,000 workspaces, and asked Anthropic, OpenAI, OpenRouter, GitHub, and Stripe directly to revoke affected keys on October 21.

Northflank's own public timeline, the most recent and most granular of the nine, shows a Wiz threat alert at 11:43 UTC on October 30 followed by network-policy patches within twenty minutes, an escalating public status from "rotate now" to "assume all environment variables have been decrypted" within roughly two hours, and user-confirmed AI key misuse by 15:12 UTC the same day. Audit logging, previously a paid feature, was enabled for every customer including the free tier the following morning, a response that arrived only after the breach rather than before it.

Wiz researchers Benjamin Read and Rami McCarthy frame the report around two converging trends: the broader rise of agentic attacks and the specific economics of LLMjacking, where stolen access is "easily monetizable" through resale. The nine platforms didn't share a vulnerability, a vendor, or an attacker, as far as Wiz could determine. They shared a business model: centrally storing third-party API credentials on behalf of customers so those customers don't have to manage secrets themselves.

That business model is not a mistake. It's the entire value proposition of a managed developer platform. But Black October is a month-long demonstration of what happens when that convenience becomes a single point of failure across an entire category of companies at once, rather than a risk specific to any one vendor's engineering quality.

**Our verdict:** if your organization has ever deployed on Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode, or Northflank, the safe assumption is that every environment variable those platforms could read is compromised, whether or not you received a direct notice, because several of the nine have not confirmed the scope of what was actually taken.

- 
**Revoke and reissue at the source, not just inside the platform.** Rotating a secret stored on a breached platform does nothing if the original credential at the issuer, AWS, Anthropic, OpenAI, OpenRouter, GitHub, or Stripe, was already copied before rotation.
- 
**Check provider usage and billing logs for the period before you knew about any breach, not just after.** Northflank's own timeline shows confirmed misuse within roughly 90 minutes of the breach becoming public; the exposure window for most of the nine started well before public disclosure.
- 
**Treat "how many platforms hold a copy of this key" as a real metric** , not an afterthought. Every platform storing a credential is a separate chance for it to leak, and October showed that risk compounding across nine unrelated companies in thirty days.

**What is Black October for AI developer platforms?**

Black October is the name Wiz Research gave its October 30, 2026 report documenting at least nine AI developer platforms, Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode, and Northflank, that disclosed security incidents during October 2026. Wiz found no shared vulnerability connecting all nine, but all involved attackers hunting stored environment variables, especially those granting access to AI models.

**How much money did attackers make from the Black October breaches?**

Wiz traced two stolen enterprise Amazon Bedrock keys from these breaches to a combined total of roughly 2.5 million API calls, including one key that generated approximately 1.7 million calls in thirteen hours. Across the two documented Bedrock cases, usage totaled more than $100,000 at list price, driven by a practice called LLMjacking, in which stolen AI credentials are used directly or resold to others.

**Is Zeabur part of the Black October report, or a separate incident?**

Zeabur's October 14 breach is one of the nine incidents Wiz counted in its Black October report. It is a separate, earlier event from Zeabur's second breach in mid-October that led to the company's shutdown announcement; the October 14 disclosure referenced in the Black October report is the first of Zeabur's two 2026 incidents.

We pulled Wiz's findings as reported directly by Bromure, Towards AWS, FourWeekMBA, and teiss, cross-referencing the nine-platform list and disclosure dates across all four independent sources to confirm agreement. Most of the nine platforms, as several of these sources note explicitly, have not published a confirmed root cause, and we've preserved that gap rather than attributing a specific cause where none has been disclosed.

I'm Rohit, founder of __**Krova Cloud**__: we store scoped API keys with idempotency rather than broad standing credentials, and every Cube is isolated enough that compromising one doesn't hand an attacker a path to every other customer's secrets. That reduces specific categories of blast radius. It does not make storing a third-party AI key on any platform, including ours, risk-free, and Black October is a useful reminder that the number of places holding a copy of your key matters as much as how well any one of them is built. More debugging stories at __**debugly.dev**__.

```
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "BlogPosting",
      "headline": "Black October: Why Nine AI Developer Platforms Lost Customers' Secrets in One Month",
      "datePublished": "2026-11-04",
      "dateModified": "2026-11-04",
      "author": {
        "@type": "Person",
        "name": "Rohit"
      },
      "about": ["Black October AI developer platforms", "LLMjacking", "environment variable secrets exposure", "AI API key theft"]
    },
    {
      "@type": "FAQPage",
      "mainEntity": [
        {
          "@type": "Question",
          "name": "What is Black October for AI developer platforms?",
          "acceptedAnswer": {
            "@type": "Answer",
            "text": "Black October is the name Wiz Research gave its October 30, 2026 report documenting at least nine AI developer platforms, Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode, and Northflank, that disclosed security incidents during October 2026. Wiz found no shared vulnerability connecting all nine, but all involved attackers hunting stored environment variables, especially those granting access to AI models."
          }
        },
        {
          "@type": "Question",
          "name": "How much money did attackers make from the Black October breaches?",
          "acceptedAnswer": {
            "@type": "Answer",
            "text": "Wiz traced two stolen enterprise Amazon Bedrock keys from these breaches to a combined total of roughly 2.5 million API calls, including one key that generated approximately 1.7 million calls in thirteen hours. Across the two documented Bedrock cases, usage totaled more than $100,000 at list price, driven by a practice called LLMjacking, in which stolen AI credentials are used directly or resold to others."
          }
        },
        {
          "@type": "Question",
          "name": "Is Zeabur part of the Black October report, or a separate incident?",
          "acceptedAnswer": {
            "@type": "Answer",
            "text": "Zeabur's October 14 breach is one of the nine incidents Wiz counted in its Black October report. It is a separate, earlier event from Zeabur's second breach in mid-October that led to the company's shutdown announcement; the October 14 disclosure referenced in the Black October report is the first of Zeabur's two 2026 incidents."
          }
        }
      ]
    }
  ]
}
```
## Top comments (0)
