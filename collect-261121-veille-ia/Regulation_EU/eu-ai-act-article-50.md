---
id: collect-261121-veille-ia/regulation-eu/eu-ai-act-article-50
title: "EU AI Act Article 50"
domain: regulation-eu
role: reference
task: article
actors: []
dates: ["2024-08-01"]
keywords: ["systems", "obligations", "risk", "transparency", "article", "august", "requirements", "providers"]
source: https://www.legal500.com/intelligence/european-union/technology/eu-ai-act-article-50
source_anchor: ""
source_lines: [1, 143]
section: "Regulation_EU"
---
## 1. Introduction: Overview of the EU AI Act

EU AI Act represents the first comprehensive regulatory framework governing the development, deployment, and use of artificial intelligence systems. Adopted by the European Parliament and Council, the AI Act entered into force on August 1, 2024, establishing a phased implementation timeline that extends through August 2027, General-purpose AI model providers must comply with transparency and documentation requirements; AI Office and AI Board operational as of August 2025, August 26 Article 50 transparency obligations become legally binding (with an extension, AI Omnibus political agreement of 7 May 2026 grants a grace period until 2 December **for** **systems already on the market**). All transparency, disclosure, watermarking, and labelling obligations enforceable, Commission Guidelines were finalized July 20, 2026, and together with the Code of Practice, govern the disclosure requirements. Full application including high-risk system obligations will be enforced 12 months from now.

The Digital Omnibus on AI, which reached political agreement on May 7, 2026, and entered into force on July 27, 2026, introduces targeted amendments to simplify implementation and provides transitional relief for certain legacy systems. Organizations should note that deep fake content created before August 2, 2026, does not carry a retroactive labelling obligation.

**Penalties:** Non-compliance may result in administrative fines of up to EUR 15 million or 3% of total worldwide annual turnover (whichever is higher) for infringements of Article 50 transparency obligations. Higher penalties apply for prohibited AI practices (up to EUR 35 million or 7% of global turnover).

## 2. Risk-Based Classification Framework

The AI Act adopts a **risk-based approach** to regulation, categorizing AI systems into four tiers based on the degree of risk they pose to fundamental rights, health, and safety:

- **Prohibited AI Practices.** AI systems and practices deemed to pose an unacceptable risk are entirely banned. These include social scoring systems, real-time remote biometric identification in public spaces (with limited exceptions), and emotion recognition based on biometric data in workplaces and educational institutions.
- **High-Risk AI Systems.** Systems used in critical areas such as employment, education, law enforcement, migration, and access to essential services are subject to stringent compliance requirements including conformity assessments, technical documentation, risk management systems, and human oversight.
- **Limited Risk, Transparency Risk.** AI systems that interact with natural persons or generate synthetic content are subject to transparency and disclosure obligations under Article 50, including labelling and watermarking requirements.
- **Minimal Risk.** AI systems posing minimal or negligible risk (such as AI-enabled spam filters or AI in video games) may be used without additional regulatory requirements, though voluntary codes of conduct are encouraged.

*The AI Act applies extraterritorially to providers placing AI systems on the EU market and to deployers located within the EU, **regardless** of where the provider is established. This broad jurisdictional reach ensures that organizations worldwide must assess their compliance obligations where their AI systems affect persons within the European Union.*

## 3. Provider vs. Deployer

The AI Act draws a critical distinction between providers and deployers, assigning differentiated obligations to each. Understanding this distinction is essential for determining an organization’s compliance responsibilities

A “***provider***” is any natural or legal person that develops an AI system or has an AI system developed and places it on the market or puts it into service under its own name or trademark, whether for payment or free of charge.

Providers bear primary responsibility for embedding machine-readable marking into AI-generated content at the model level. Key obligations include implementing marking before placing the system on market, ensuring outputs are detectable as artificially generated or manipulated, providing free detection tools (APIs or user interfaces) for users and third parties, implementing detection capabilities for both marked and unmarked synthetic content, and adopting a multi-layered marking approach (no single technique is sufficient).

A “***deployer***” is any natural or legal person using an AI system under its authority, except where the AI system is used in the course of a personal non-professional activity.

Deployers are responsible for ensuring that AI-generated content is appropriately labelled at the point of contact with end users. Key obligations include labelling deepfakes clearly and distinguishably at the moment of first exposure, disclosing AI-generated text on matters of public interest, maintaining internal documentation of labelling practices, and ensuring that provider-embedded disclosures are surfaced rather than suppressed by custom front ends. Crucially, deployers must not remove or alter AI watermarks embedded by providers.

## 4. Article 50 Transparency Obligations (Effective August 2, 2026)

Article 50 of the AI Act establishes the cornerstone transparency obligations for AI systems posing limited risk. These obligations became legally binding on August 2, 2026, and represent the single most significant new compliance requirement for organizations deploying or providing AI systems. The European Commission adopted (approved) the final Guidelines on Transparency Obligations on July 20, 2026.

## 4. 1 Article 50(1): Human-AI Interaction Disclosure

Providers of AI systems designed to interact directly with natural persons must ensure that the system is designed and developed in such a manner that the persons concerned are informed they are interacting with an AI system. This obligation applies to chatbots, virtual assistants, AI agents, and any system that communicates with humans in a manner that could be mistaken for human interaction.

**Key requirement:** AI agents must disclose both their artificial nature and the identity of the natural or legal person on whose behalf they are acting. This dual disclosure requirement ensures that individuals understand not only that they are communicating with a machine, but also who is responsible for that machine’s conduct. Notably, open-source AI systems are **not** exempted from Article 50 obligations.

## 4.2. Article 50(2): Machine-Readable Marking of AI-Generated Content

Providers of AI systems that generate synthetic audio, image, video, or text content must mark outputs in a machine-readable format, enabling detection that the content has been artificially generated or manipulated. This obligation applies to **new** systems placed on the market from August 2, 2026 onward, systems placed in the market have grace period until December 2026.

**Critical implementation requirements:** providers must implement marking before placing the system on the market. Marking must be implemented at the **model level**, not merely as post-processing (See section 5 below) and Providers **must supply free detection tools (APIs or user interfaces) for users and third parties**, the Detection capabilities must cover both marked and unmarked synthetic content.

## 4. 3 Article 50(3): Emotion Recognition and Biometric Categorization

Deployers of emotion recognition systems or biometric categorization systems must inform the natural persons exposed to such systems of their operation. Affected individuals must be notified in advance, in a clear and distinguishable manner. This obligation interacts with the AI Act’s prohibition on emotion recognition in workplaces and educational institutions (discussed below).

## 4.4 Article 50(4): Deepfake Labelling and AI-Generated Text

Deployers of AI systems that generate or manipulate image, audio, or video content constituting a deep fake must disclose that the content has been artificially generated or manipulated. This obligation applies irrespective of intent to deceive even content generated for entertainment, artistic, or commercial purposes must be disclosed where it constitutes a deep fake.

Additionally, deployers of AI systems that generate or manipulate text published for the purpose of informing the public on matters of public interest must disclose that the content has been artificially generated or manipulated. An editorial exemption is available where the publication process involves genuine human editorial review and where a natural or legal person holds editorial responsibility.

## 5. AI Watermarking and Content Marking

*AI watermarking embeds machine-readable signals into AI-generated content to enable automated detection. Watermarks are imperceptible to humans but fully detectable by machines. The EU AI Act mandates transparency for AI-generated content. The Three-Layer Approach:*

The AI Act requires a multi-layered approach to content marking. No single technique is deemed sufficient to meet provider obligations under Article 50(2). The following three-layer framework represents current best practice and is endorsed by the **EU Code of Practice on Transparency**.

## Layer 1: Digitally Signed Metadata (e.g., C2PA Content Credentials)

The first layer involves attaching digitally signed metadata to AI-generated content using open standards, principally the Coalition for Content Provenance and Authenticity (C2PA) Content Credentials standard. This metadata records provenance information including the identity of the generating system, the date and time of generation, and any subsequent modifications, all signed cryptographically to verify authenticity.

**Advantages**: Easy to verify using standardized detection tools, supports broad interoperability across platforms and services, enables downstream systems to confirm origin and integrity of content and approved by the EU Commission.

## Layer 2: Imperceptible Watermarks Embedded in Content (e.g., Google’s SynthID)

The second layer embeds invisible watermarks directly within the content itself. Google’s SynthID technology represents the leading implementation of this approach. Imperceptible watermarks are embedded within images, video, and audio in a manner that is invisible to human perception but detectable by specialized tools.

These watermarks are designed to survive common transformations including resizing, compression, format conversion, clipping, and re-encoding. The AI Act and Code of Practice require that watermarks be “*embedded within the content in a manner that is difficult for it to be separated from the content*“.

## Layer 3: Logging and Fingerprinting for Centralized Records

The third layer involves centralized logging systems that record fingerprint data and watermark presence across platforms. This enables auditing and full traceability of AI-generated content throughout its lifecycle, supporting enforcement actions and investigations where necessary.

## 6. Marking Requirements by Content Type Images

AI-generated images must carry both C2PA Content Credentials and an imperceptible pixel-level watermark (such as SynthID). These marks must survive common transformations including screenshots and re-uploads. Where an AI-generated image constitutes a deep fake depicting a real person, a perceptible label visible to the viewer is additionally required.

## Voice and Audio

AI-generated audio content must embed inaudible patterns within audio waveforms, designed to survive compression, format conversion, clipping, and re-encoding. For long-form audio content, disclosure must be repeated at intervals to account for listeners who join midway. Deep fake voice content must be labelled even in the absence of any intent to deceive.

## Video

AI-generated video requires watermarking in video frames synchronized with audio marking. Persistent visual indicators must be maintained throughout playback, with repeated or persistent labels to ensure that viewers who tune in midway are informed. For multimodal content, marks must be synchronized across visual and audio tracks.

Deepfake video: labelling required at moment of first exposure for each viewer.

## Text

Article 50(4) applies specifically to AI-generated text published on matters of public interest. Machine-readable marking is achieved via token-level watermarking techniques (such as Google’s SynthID Text), which embed invisible statistical patterns during generation that are imperceptible to human readers but recognizable by detection tools.

Text watermarking methods include generative watermarking (modifying token selection probabilities during generation), edit-based watermarking (synonym substitution or Unicode character variations), and data-driven watermarking (embedding trigger phrases). SynthID Text performs consistently across different languages.

**Editorial exemption:** Where AI-generated text undergoes genuine human editorial review and a natural or legal person holds editorial responsibility for the publication, the labelling obligation under Article 50(4) may not apply. However, deployers relying on this exemption must retain logs demonstrating the extent and nature of human editorial involvement.

## AI Generated Avatars

Digital human likenesses generated or manipulated by AI constitute a rapidly growing use case across corporate communications, marketing, entertainment, and education. Under the AI Act, such content falls squarely within the definition of “deep fake” set out in Article 3(60).

Avatars include audio, image, video and text (if provides chat assistance features), highly regulated. AI Generated Avatars include audio, image, video and text (if provides chat assistance features), highly regulated

Deepfake rules apply even without intent to deceive: content that looks or sounds like a real person must be labelled. Use cases: AI presenters, virtual customer service agents Article 50(1) chatbot disclosure and Article 50(4) deepfake labelling.

## 7. EU Code of Practice on Transparency of AI-Generated Content

The EU Code of Practice on Transparency of AI-Generated Content was published in June 2026, providing the most streamlined compliance framework for Articles 50(2) and 50(4) of the AI Act. Together with the Commission’s Guidelines on Transparency Obligations (adopted July 20, 2026), the Code offers organizations a recognized compliance pathway.

By the end of July 2026, approximately 190 companies and organizations had signed the Code of Practice. Adherence to the Code is voluntary; however, it is critical to understand that the underlying Article 50 obligations are mandatory legal requirements regardless of whether an organization has signed the Code.

The European Commission and the AI Board have confirmed that the Code of Practice constitutes an adequate voluntary tool to demonstrate compliance with transparency obligations. Adherence minimizes regulatory scrutiny and establishes a recognized compliance pathway for enforcement purposes.

## EU Labelling Icons

Not all AI-generated or manipulated content needs to be labelled. The disclosure requirement under the AI Act covers only AI-generated or manipulated content. The icons support compliance with Article 50(4) of the AI Act, which requires deployers of AI systems to disclose:

- **Deep fakes –**  AI-generated or manipulated image, audio, or video content that resembles existing persons, objects, places, entities or events and would falsely appear to a person to be authentic or truthful.
- **AI-generated or manipulated text**  published to inform the public on matters of public interest, which did not undergo human review or editorial control and where editorial responsibility was not assumed by any legal or natural person.

## 8. Prohibited and High-Risk AI Systems (Transparency under Article 50(3))

The AI Act prohibits certain AI practices deemed to pose an unacceptable risk to fundamental rights. Of particular relevance to our clients’ operations is the prohibition on emotion recognition based on biometric data in the workplace and educational institutions.

**Prohibited:** emotion recognition based on biometric data in workplace or education.

***What qualifies as prohibited?***(i) facial-expression analysis from video (ii) inference from physiological signals (heart rate, skin conductance); (iii) any biometric-based emotion inference on employees or students.

***How to reduce risk?***(i)use sentiment analysis based on text, not biometric data (ii) ensure inference is not on employees or students.

## 9. Emotion Recognition, Sentiment

## 10. Conclusions

**Footnote**

1 AI Omnibus provides grace period for marking obligations for systems placed on market before August 2, 2026 until December 2, 2026

*The content of this article is intended to provide a general guide to the subject matter. Specialist advice should be sought about your specific circumstances.*
