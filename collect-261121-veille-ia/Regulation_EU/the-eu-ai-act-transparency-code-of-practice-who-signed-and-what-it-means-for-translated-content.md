---
id: collect-261121-veille-ia/regulation-eu/the-eu-ai-act-transparency-code-of-practice-who-signed-and-what-it-means-for-translated-content
title: "The EU AI Act transparency Code of Practice: who signed, and what it means for translated content"
domain: regulation-eu
role: reference
task: article
actors: []
dates: []
keywords: ["code", "commission", "content", "article", "section", "practice", "ai-generated", "providers"]
source: https://www.locize.com/blog/ai-act-transparency-code-of-practice
source_anchor: ""
source_lines: [1, 89]
section: "Regulation_EU"
---
The **Code of Practice on transparency of AI-generated content** is a voluntary framework published by the European Commission that providers and deployers of generative AI systems can sign to demonstrate compliance with the marking and labelling obligations of Article 50 of the EU AI Act. As of **September 9, 2026**, the Commission's signatory table names 83 organisations under Section 1 (providers) and 152 under Section 2 (deployers), and the Commission states that the list is updated on an ongoing basis.

This post explains what the Code is and is not, who has signed it as of today, what a signature commits a company to, and the question that matters for anyone running a localized website or app: what the Code changes, and does not change, for machine- and AI-translated content. The underlying question, whether machine-translated text is "AI-generated" under the Act, is answered in the Article 50 explainer; this post starts where that one ends.

**This is not legal advice.** This article summarizes public documents of the European Commission for orientation, with the date each fact was checked. The Code and the guidelines are non-binding; authoritative interpretation of the AI Act rests with the CJEU. For decisions about your own obligations, consult a qualified lawyer.


- **What it is:** a voluntary code with two sections, Section 1 for providers (machine-readable marking and detection) and Section 2 for deployers (labelling deepfakes and AI-generated text on matters of public interest), assessed as adequate by the Commission and the AI Board.
- **Who signed, as of September 9, 2026:** 83 Section 1 and 152 Section 2 signatories in the Commission's table (its summary line says 82), about 190 in total; the initial list was published on July 31, 2026, and the list remains open.
- **Voluntary, in the Commission's own words:** "Signing the code of practice is voluntary. Not signing it does not constitute non-compliance with the AI Act." Signatories may rely on the Code to demonstrate compliance; non-signatories demonstrate it by other means.
- **For translated content nothing moves:** faithful AI translations are exempt from machine-readable marking under the final guidelines of July 20, 2026; visible disclosure applies only to public-interest text and falls away with documented human review under editorial responsibility. The Code is a way to demonstrate compliance, not a change to those rules.
- **Dates around it:** Article 50 applies since August 2, 2026; generative systems already on the market before that date have until December 2, 2026 for machine-readable marking; signatory task forces were announced for September 2026.

Article 50 of the AI Act puts two content obligations on two different actors: providers of generative AI systems must mark their output in a machine-readable way (Article 50(2)), and deployers must visibly disclose deepfakes and AI-generated text published to inform the public on matters of public interest (Article 50(4)). Article 50(7) asks the Commission to encourage codes of practice that make those obligations workable, and the Code of Practice on transparency of AI-generated content is the result.

It has two sections that mirror the two obligations. **Section 1** is for providers: rules for marking and detecting AI-generated and manipulated content. It can also be signed by providers of marking and detection technology and by AI model providers whose models help others implement the measures. **Section 2** is for deployers: rules for labelling deepfakes and certain AI-generated or manipulated text published to inform the public on matters of public interest. The Commission also published a set of icons that deployers may use for labels.

What the Code is not: it is not the law, and it is not the guidelines. The obligations are in the Act; how the Commission reads them is in the final guidelines on transparency obligations of July 20, 2026. The Code is a way to demonstrate that you meet the obligations. The Commission and the AI Board have confirmed, in the Commission's words, that "the code is an adequate voluntary tool to demonstrate compliance with the AI Act transparency obligations".

The Commission published the initial signatory list on July 31, 2026, two days before the obligations applied, and describes about 190 organisations "across various sectors, including IT, telecom, education, and retail", about half of them "small and recent companies". Counting the table on that page on September 9, 2026 gives 83 names under Section 1 and 152 under Section 2; the page's own summary line says 82 for Section 1, so treat the exact figure as approximate and the list as moving. The Commission's own examples for Section 1 are Aleph Alpha, Anthropic, Black Forest Labs, Cohere, Google, Meta, Microsoft, Mistral, OpenAI and Synthesia; for Section 2, Bulgari, Fastweb, Getty Images, Iberdrola, Lenovo and Lufthansa.

Two observations matter for localization teams. First, the model and platform providers whose engines translation workflows commonly run on (Google, Microsoft, OpenAI, Mistral and Anthropic among them) are Section 1 signatories, which is where the machine-readable marking duty sits. Second, the translation industry is barely represented by name: one translation-technology company (EasyTranslate) appears under Section 1 as of that date, and no translation or localization vendor is among the Commission's own examples. Given that faithful translations sit outside the marking duty, that is consistent, and it implies nothing about anyone's compliance, because signing is voluntary.

The Commission also announced that signatories would be invited to collaborate in two task forces "to be launched in September 2026" to share practices and advance implementation. As of the date of this post the Commission's page still describes them in the future tense.

According to the Commission's signing FAQ, the mechanics are simple:

- **Who can sign:** providers of generative AI systems subject to Article 50(2) (Section 1); deployers subject to Article 50(4), particularly those using generative AI professionally in ways that affect public discourse (Section 2); plus AI model providers and providers of marking and detection technology (Section 1).
- **What a signature is worth:** signatories "may rely on the code to demonstrate compliance with their obligations under Article 50", with, in the Commission's words, "clarity and increased legal certainty across the EU". Providers and deployers that comply through other means "will have to demonstrate that those measures are adequate", assessed individually by market surveillance authorities.
- **What it is not:** an obligation. "Signing the code of practice is voluntary. Not signing it does not constitute non-compliance with the AI Act." Signatories can withdraw by letter at executive level.
- **What Section 2 signatories commit to:** disclosure and labelling of deepfakes and of certain AI-generated or manipulated text published with the purpose of informing the public on matters of public interest, following the Code's measures.

In other words, the Code trades a documented, EU-wide recognised way of meeting the obligation for a public commitment to it. For a company whose exposure under Article 50 is small, that trade is often not worth the signature; for one that publishes AI-generated public-interest content across several member states, it may well be.

This is the part localization teams actually need, and it is shorter than the rest, because the Code does not change the rules that apply to translation; it only offers a way to demonstrate compliance with them.

**Machine-readable marking is your provider's duty, and faithful translation is exempt from it.** Article 50(2) sits with the provider of the AI system. The final guidelines of July 20, 2026 list "AI-generated translations of text" among the examples of standard editing that benefit from the Article 50(2) exception, next to grammar correction and minor stylistic polishing, while summaries and rewrites that change meaning stay marking-required. Your MT or AI provider signing Section 1 tells you how they demonstrate the marking of the output that does need marking; it does not create a marking duty for your translated UI strings.

**Visible disclosure is your duty, and it is narrow.** Article 50(4) covers AI-generated or manipulated text "published with the purpose of informing the public on matters of public interest". Product copy, UI strings, entertainment and ordinary advertising fall outside; public health and safety information, reporting on political or economic developments, corporate reports, public-service information can fall inside. An AI-translated help article on product safety is the kind of content to look at.

**The exemption that touches translation workflows is human review, not the Code.** Article 50(4) does not apply where the content "has undergone a process of human review or editorial control" and a natural or legal person holds editorial responsibility for the publication. The final guidelines' own example of content that qualifies is, verbatim, an "AI-supported translation of a human-written article whereby the translation has undergone human review". Review has to be substantive (fact-checking is named as a minimum ingredient), it has to be the last touch before publication, and the responsible person has to be findable. A signature on the Code is an alternative way for a deployer to demonstrate its Article 50(4) compliance; it does not replace the review, and it does not lower the bar for it.

**So, as of September 9, 2026, the practical position is unchanged from August 2:** for the small share of translated content in Article 50(4)'s scope, either label it or review it under a named editorial responsibility and keep the record. Whether to sign Section 2 on top is a voluntary, company-level decision that has little to do with translation and a lot to do with what else the company publishes with generative AI.

- **Ignore it, correctly or not.** Most localized products publish nothing that informs the public on matters of public interest, and for them the honest answer to "what do we need to do about the Code" is "nothing, but write down why". The risk is the team that has not checked its help center, its status page or its investor pages against the scope.
- **Sign Section 2.** Deployers with substantial AI-generated public-interest output, typically media, public bodies and large corporates, take the documented route. As of September 9, 2026 that is 152 organisations.
- **Document the review instead.** Route AI-translated public-interest content through substantive human review with recorded decisions and a named responsible person, and rely on the Article 50(4) exemption. This is the path that overlaps with what a translation workflow does anyway, which is why it is the one most localization teams end up on.
- **Ask the vendors.** Ask MT and AI providers how they handle Article 50(2) marking, and whether they signed Section 1; the Commission's list answers the second question in seconds. Ask agencies and translation platforms how review decisions are recorded and exported.

- **It is not binding and not a safe harbour in the legal sense.** The Commission's assessment says the Code is adequate for demonstrating compliance; the last word on Article 50 belongs to the CJEU.
- **It does not make anything reviewed.** A Section 2 signature commits a deployer to labelling; it does not turn unreviewed machine output into reviewed content. The human-review exemption is met by the review, not by the signature.
- **It does not cover most translated content, because Article 50(4) does not.** UI strings, product descriptions and marketing copy are outside the scope whether or not anyone signs anything.
- **It is still forming.** The task forces announced for September 2026 had not been described as launched on the Commission's pages as of this post; the list changes; the guidelines have a stated review path. Date every claim you make about it, including the ones in this post.

Locize does not sign anything on your behalf and does not make you compliant; no tool does. What it gives a deployer on the review path is the evidence the Article 50(4) exemption asks for:

- The review workflow holds AI and machine translations as proposals or marks them as needing review until a person with the right permission confirms, edits or declines them, per segment.
- Quality estimation scores every AI translation and routes low-confidence ones into review, so substantive attention goes where the model is least sure.
- The history records who decided what and when, and the provenance export described in the Article 50 workflow guide packages it as CSV and JSON per version, language or namespace, with a review status per key that an auditor can read without the product. For what that export shows on a real run, from an agent's strings to the reviewer's decisions, see a coding agent localizes a feature, step by step.

Not sure whether any of your translated content is in scope at all? The Article 50 readiness check walks through the questions. If it is, create a project, enable review proposals for the languages that matter, and the record starts with the next translation.

## Frequently asked questions

### What is the Code of Practice on transparency of AI-generated content?

A voluntary framework published by the European Commission that providers and deployers of generative AI systems can sign to demonstrate compliance with the transparency obligations of Article 50 of the EU AI Act. It has two sections: Section 1 for providers (machine-readable marking and detection of AI-generated content) and Section 2 for deployers (labelling of deepfakes and of AI-generated text published to inform the public on matters of public interest). The Commission and the AI Board have assessed it as an adequate tool for demonstrating compliance; signatories can rely on its measures, while non-signatories demonstrate compliance by other means.

### Who has signed the Code of Practice on transparency of AI-generated content?

As of September 9, 2026, the Commission's signatory table lists 83 organisations under Section 1 (the page's own summary line says 82) and 152 under Section 2, about 190 in total. Section 1 examples the Commission itself names include Aleph Alpha, Anthropic, Black Forest Labs, Cohere, Google, Meta, Microsoft, Mistral, OpenAI and Synthesia; Section 2 examples include Bulgari, Fastweb, Getty Images, Iberdrola, Lenovo and Lufthansa. The Commission states the list is updated on an ongoing basis, so check the live page before quoting a number.

### Is signing the Code of Practice mandatory?

No. The Commission's own FAQ states that signing is voluntary and that not signing does not constitute non-compliance with the AI Act. The difference is procedural: signatories may rely on the Code's measures to demonstrate compliance with Article 50, while providers and deployers that comply through other means have to show that those means are adequate, assessed individually by the market surveillance authorities. Signatories can also withdraw.

### Does the Code of Practice apply to machine-translated or AI-translated content?

Indirectly. The Code is a way to demonstrate compliance with Article 50; what Article 50 requires for translations is set by the Act and the Commission's final guidelines of July 20, 2026. Under those guidelines a faithful AI translation counts as standard editing and is exempt from the machine-readable marking duty of Article 50(2). The visible disclosure duty of Article 50(4) covers AI-generated text published to inform the public on matters of public interest, and falls away where the text has undergone human review or editorial control with a person holding editorial responsibility. Signing the Code does not change either rule. This is not legal advice.

### What should a company that publishes AI-translated content do now?

Decide which of its translated content falls under the public-interest scope of Article 50(4), which for most UI strings and product copy is none of it, and for public health, safety, corporate reporting or public-service content can be all of it. For that content, either apply a visible disclosure or run a documented human review with a named editorial responsibility, which the final guidelines illustrate with exactly the case of a human-reviewed AI translation. Keep the record per translation. Whether to sign Section 2 of the Code on top is a separate, voluntary decision. Consult a lawyer for your own obligations.

## Tired of managing translations by hand?

Locize is the translation management backend by the i18next team: CDN delivery, AI translation, in-context editing, no redeploys.

Start your free 14-day trial
