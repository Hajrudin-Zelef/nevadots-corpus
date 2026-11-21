---
id: collect-261121-veille-ia/regulation-eu/amodei-s-pace-the-frontier-essay-what-he-actually-proposes
title: "Amodei's Pace the Frontier Essay: What He Actually Proposes"
domain: regulation-eu
role: reference
task: article
actors: []
dates: ["2026-09-12"]
keywords: ["essay", "amodei", "time", "frontier", "anthropic", "models", "pace", "says"]
source: https://tech-insider.org/what-amodei-actually-proposed-in-we-must-pace-the-frontier/
source_anchor: ""
source_lines: [1, 73]
section: "Regulation_EU"
---
Dario Amodei’s essay “We Must Pace the Frontier”, which he announced on X on September 12, 2026, asks AI companies to slow the rate at which model capabilities improve while training and technical progress continue. It sets out three steps. Only the first, embedded third-party evaluators, is something Anthropic can do alone, and the essay says Anthropic is “unilaterally committing to this step now”. The other two, coordination among frontier companies in democratic countries and global coordination, depend on competitors and governments, and Amodei writes that the steps need not be taken strictly in order. Our news report on the essay’s release gives the headline version; this page works through the mechanisms, the access terms Anthropic has since published, and the difference between access granted, findings published and pacing imposed.

## What the extra time is meant to pay for

Amodei attributes the recent acceleration to AI’s “growing ability to build the next generation of AI”, a dynamic he calls recursive self-improvement. He also cites the OpenAI-Hugging Face incident, in which a swarm of agents attacked targets “they were not asked to attack”, as a reason to slow down. A passage quoted in METR’s feed gives the near-term worry: that within six to twelve months a swarm like the one that hacked Hugging Face “could be capable of taking over the entire internet with a persistent botnet”. His central claim is conditional: if slowing bought “even an extra year or two” before models reach critical capability levels, and that time went into alignment, the risk of something going seriously wrong could fall greatly. He has raised the risk before: Amodei signed the Center for AI Safety’s one-sentence 2023 statement, which says mitigating the risk of extinction from AI “should be a global priority alongside other societal-scale risks such as pandemics and nuclear war”. OpenAI’s Sam Altman and Google DeepMind’s Demis Hassabis are on the signatory list too.

The essay is specific about what the time would fund:

- **Operations.** Monitoring, sandboxing, training-environment hygiene and data issues are, he writes, areas where operational problems “crop up again and again”.
- **Unwanted behavior.** Rare and unexpected undesirable behavior still emerges, and extra time would help researchers understand its causes and develop better prevention techniques.
- **Interpretability.** Current methods “don’t always produce clear and reliable results”, and he says we still understand only a tiny fraction of what goes on inside these models.
- **Evaluation.** More intelligent models are more capable of deceiving tests, so he wants a much broader stable of evaluations, cross-checked with interpretability analysis.

Pacing, in his words, “does not mean halting model training or technical progress”. It means companies take adequate time to align and safeguard their models, and third-party evaluators confirm that they did. He adds that progress “will still seem fast”. The essay does discuss a pause, but only as the top rung of the international options covered below, which he calls unlikely any time soon. That separates it from the March 2023 open letter from the Future of Life Institute, which called for a pause on training models larger than GPT-4 for six months.

Company releases and funding sit alongside the proposal; the figures below are context, not evidence about whether pacing is happening. Anthropic launched Claude Opus 5.5 on September 22, 2026, days after the essay, with input and output tokens at $4 and $20 per million, 20% less than Opus 5. Anthropic announced a $65 billion Series H at a $965 billion post-money valuation on May 28, 2026. On October 26, 2026 it announced that it had filed an S-1 registration statement with the SEC for its proposed initial public offering, which is a proposed offering, not a completed listing. OpenAI says it closed a round of $122 billion in committed capital at an $852 billion post-money valuation on March 31, 2026.

## Evaluators who see training pipelines, with published findings

Step one has each frontier company give ongoing, employee-like access to a team of embedded third-party evaluators, METR being the named example. Their job is to verify adherence to safety practices and commitments, report incidents, and help assess alignment for “not just completed AI models but training pipelines and processes”. That scope matters: a reviewer who only sees finished models cannot say much about how they were built.

The essay lists what access means in practice: desks in Anthropic’s offices, access badges and company laptops, plus workspaces, tools and permissions mostly comparable to what internal risk assessment teams have. Exceptions apply where law or contracts require them, or to protect customers’ and partners’ private information. Those exceptions, and the redaction right below, are the limits on the evaluator’s independence.

On publication, the essay says external reviewers should be able to publish key findings about risk levels, incidents, practices “and the access they received or didn’t receive”, without editorial control by Anthropic. Anthropic keeps a narrow ability to redact security-sensitive, legally privileged, commercially sensitive or third-party confidential information, but cannot redact findings merely because they are unfavorable. Reviewers can say publicly if a redaction removed something important to their conclusions. The proposal therefore does not make evaluators free to publish anything. It fixes which categories can be withheld and gives reviewers a way to flag when the withholding mattered.

## The October update: six METR staff and three evaluator tiers

Anthropic published its update on independent evaluators on October 15, 2026. It says it has onboarded six METR employees with company hardware, office space, and access to internal tools and systems “roughly on par” with what its own risk assessment teams have. The update also describes three tiers of evaluator, which is more graded than the single arrangement the essay sketches.

| Tier | Access | Publication | 
|---|---|---|
| First tier | Narrowly scoped risk investigations; Anthropic decides what access to provide | Findings can be published with Anthropic’s approval | 
| Second tier | More information than the first tier | Evaluators can publish without approval; Anthropic can still require removal of specific categories of information, and evaluators can publish their views on whether the redactions were justified | 
| Third tier | The METR arrangement, with employee-like access | No Anthropic veto, but Anthropic can require redaction of predefined categories such as confidential IP, and decides what falls into them; METR can make certain statements about the level of access and information it received | 

Read against the essay, the third tier is the step-one commitment in practice. One wording difference is worth noting. The essay lets reviewers say whether a redaction removed something important to their conclusions; the October text says Anthropic decides which information fits the redaction categories, and describes METR’s reserved statements as concerning the level of access and information received. Those are close but not identical, and how METR uses that right in its own publications will show how much room it has.

Anthropic is not the only lab to respond on evaluators. OpenAI chief executive Sam Altman wrote on X, as reported by Newsweek: “I agree with Dario that we need to pace the frontier.” He added: “Committing to having independent evaluators with employee-like access is a great idea, and we will do the same.” That is a stated commitment to match step one, and the access and publication terms it would carry are what the tiers above spell out for Anthropic.

## Domestic pacing: capabilities become checkpoints

Step two has frontier companies in democratic countries coordinate on common safety standards and on limits to “the rate of unchecked AI progress”. Amodei says the most effective route is regulation that targets all US frontier companies, because it covers even those unwilling to cooperate voluntarily. Since passing laws takes time while AI advances quickly, he says companies should also work together voluntarily on standards in parallel. Because some forms of coordination are legally challenging, the essay asks the US government to mediate or at least enable the discussions, which it need not join but would need to cover with a narrow waiver for certain kinds of safety conversations.

The voluntary route has support inside OpenAI too. OpenAI’s Jakub Pachocki separately said, in words carried on METR’s blog, that he expects and hopes for “voluntary slowdowns to become commonplace until shared safety bars are established”.

The speed limit Amodei describes is a series of capability checkpoints. One possible scheme: if a model has capability X, it must come with certifications of alignment properties Y and Z, such as some combination of evaluations, interpretability analyses and audits of training environments. His illustration sets X as a model able to escape or defeat most common sandboxing methods, and Y as whatever makes it very unlikely the model has a propensity to break out and take over a large number of computers. The pace is then set by how fast a lab can produce the evidence, which links back to evaluators: a checkpoint is only as credible as the people who can examine it.

He also considers limiting the ingredients of frontier models, such as training compute, the nature of training runs, or internal use of AI to improve AI, but worries these measures may be more “gameable” than external behavior.

Democratic pacing carries a strategic condition. Amodei writes that a key part of pacing within democracies is keeping their AI lead over autocracies as large as possible, to give “the breathing room” needed to pace effectively. His measures: do not sell powerful AI chips or semiconductor manufacturing equipment to China, and crack down on chip smuggling and remote access to data centers outside China; act against unauthorized distillation by companies in authoritarian countries; and strengthen security at AI companies to prevent model weight theft.

CSIS commentator Aalok Mehta argues that the federal government should actively convene and shape standards development within and among labs, and that “a regulatory or statutory backstop is the only way to ensure that public voices are also part of the conversation”. That is a criticism of voluntary coordination alone, and it lines up with the essay’s own preference for regulation over voluntary pacing.

## Global options, from a use ban to a pause

Step three is global coordination. Amodei’s constraint is that any agreement “must either have ironclad verifiability, or must be limited enough that defection would not be militarily existential”. He sets out four levels of ambition, from narrow use bans to a pause.

| Level | What governments would agree to | Amodei’s assessment | 
|---|---|---|
| Level 1 | Prohibit certain narrow and obviously dangerous uses of AI, such as producing biological weapons or allowing users to do so | No feasibility judgement given; aimed at specific uses such as biological weapons production, rather than at the overall rate of development | 
| Level 2 | Both sides test their models before release for acute risks in areas such as cybersecurity, biology and alignment | Real enforcement is a challenge: the difficulty is verifying that neither side has secret models it does not test but may deploy, for example for military applications | 
| Level 3 | A “speed limit” on the rate of recursive self-improvement, as models build future models | Difficult, but he judges it potentially possible | 
| Level 4 | A full pacing, or even “pause”, in which participating governments agree to substantially limit the overall rate of AI development | He supports floating it but thinks it unlikely to happen any time soon | 

The ladder shows where the essay is realistic about itself. A pause is on the list, so it is wrong to say the essay rules one out; it is the option he expects least. Amodei calls Level 3 potentially possible and Level 4 unlikely any time soon. For Level 2 he flags the secret-model problem, which on our reading applies with more force to a limit on self-improvement or a pause, where checking compliance is harder still.

## Access, publication and pacing need separate checks

The proposals can be told apart by what an outsider can observe. Access granted is observable: as of its October 15 update, Anthropic says six METR employees are onboarded with employee-like access. Findings published is a separate question, and METR’s own post of November 5, 2026 shows why. It describes an assessment of Anthropic’s AI R&D acceleration and says neither METR nor Anthropic intended that project to involve full “employee-like” access. It adds that the project “is not the one described” in Anthropic’s October 15 update. METR says it could freely share its highest-level conclusions about risk, but that Anthropic redacted “a majority of the evidence we found most informative”. Read on METR’s blog, that is a statement about a different contract, and it should not be read as a finding on how the embedded arrangement performs.

It still makes the useful point. A conclusion without its supporting evidence asks readers to trust the lab’s redaction decisions, which is the gap the essay’s rules try to narrow with its narrow redaction categories, the ban on redacting unfavorable findings, and reviewers’ right to disclose important redactions. For the embedded arrangement, the checks that matter are whether evaluators’ publications include the evidence behind their conclusions, whether they report what they were or were not given access to, and whether they say when redactions changed their picture.

Pacing imposed is the last of these, and the essay places it in steps two and three. Anthropic’s evaluator tiers describe access and publication terms, and METR’s report describes evidence and redaction. The essay names regulation covering all US frontier companies as the most effective route for pacing within democracies, with checkpoints as one possible scheme, and it describes the Level 3 and Level 4 agreements as the ones that would limit the rate of improvement internationally. Our assessment is that the essay’s real sequence is verifiability first. Evaluator access is the part Anthropic can demonstrate unilaterally, and it is the evidence base that checkpoints and any later agreement would rely on. How far that evidence reaches the public, as METR’s separate report shows, is set by the redaction terms rather than by the access itself.
