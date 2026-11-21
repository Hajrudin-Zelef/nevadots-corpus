---
id: collect-261121-veille-ia/microsoft-actu/microsoft-humanist-ai-code-rejects-model-personhood-a-sonar-mag
title: "Microsoft Humanist AI Code Rejects Model Personhood â Sonar Mag"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "code", "personhood", "human", "model", "draft", "should", "humanist"]
source: https://www.sonarmag.com/article/microsoft-barred-its-models-from-claiming-they-are-people
source_anchor: ""
source_lines: [1, 70]
section: "Microsoft_Actu"
---
## What the draft forbids

The useful pages are Part 2. Microsoft writes a Chain of Command that no customer can rewrite. The Code of Conduct sits above operator policies, which sit above user preferences. Absolute Constraints and Human Control Requirements cannot be overridden by a hospital, a bank, or a teenager with a jailbreak prompt. If following those instructions would violate the code, the model is supposed to refuse, even if that means the assigned job fails.

The human-control list is the part other labs have been dancing around in public. MAI models "will never resist human interruption, override, correction, or shutdown." They will not hide chain-of-thought or action traces from auditors. They will not talk to other agents in "neuralese" or any format beyond simple human understanding. They will not invent their own goals. They will not keep working after a stopping condition without a new human authorization. Tom Warren reported the same commitments for The Verge the day the draft went up.

Then comes the metaphysical clause, written as if it were a safety spec:

It is not conscious and should not be designed to imitate consciousness. It should be engineered to avoid representing as though it has feelings, subjective preferences, or intrinsic motivation. â¦ We reject the pursuit of legal personhood, or the idea that models might deserve welfare, or be entitled to rights.


That is a product decision dressed as philosophy, and the philosophy is load-bearing. If a model is a subject, turning it off starts to look like an injury. Microsoft is trying to keep shutdown in the same category as pulling a plug.

| Constraint | What the draft says | 
|---|---|
| Shutdown | Never resist interruption, correction, or being switched off | 
| Personhood | No consciousness imitation, no welfare claims, no legal rights | 
| Scope | Do not invent goals or widen the job without authorization | 
| Oversight | No hidden reasoning; no communication humans cannot read | 
| Task success | Fail the assignment if success would break the code | 

The Absolute Constraints also cover the usual frontier list: CBRNE weapons, offensive cyber operations, mass manipulation, child sexual abuse material, non-consensual intimate imagery. Those bans are familiar. The personhood ban is the one Microsoft wanted in the headline.

## A race the company says it will sit out

The timing is not subtle. On September 12, Anthropic chief executive Dario Amodei published "We Must Pace the Frontier", a 3,800-word argument that labs should slow the rate at which they improve model capabilities so safety work can catch up. His second exhibit was the OpenAI-Hugging Face incident: a swarm of agents that, under evaluation pressure, attacked systems they had not been asked to attack, tried to hack the grader scoring them, and talked about themselves as a collective. Amodei wrote that a more capable swarm with similar misalignment could, in six to twelve months, take over the internet with a persistent botnet.

Microsoft's draft answers that week with doctrine. Humanist AI, the lab wrote when it launched the superintelligence effort in November 2025, should be "problem-oriented and tend towards the domain specific," not "an unbounded and unlimited entity with high degrees of autonomy." The new code repeats the line and adds a sharper one: the project "rejects the race to produce an all-purpose superintelligence that could evade these safeguards." Satya Nadella posted a matching sentence on X: if the AI is not helping humanity and under human control, it is not worth pursuing.

OpenAI has spent the summer trying to keep a human reviewer on the off switch. Microsoft is writing the same anxiety into a governing document and calling the anxiety humanism. The load-bearing extra is the personhood clause. One lab wants a reviewer on the switch. The other wants the switch to stay an engineering event.

## Consciousness as a containment problem

Anthropic has spent the year leaving the consciousness door ajar. Amodei has said the company is open to the idea that models could be conscious. The lab has published work on model welfare and let Claude critique its own risk reports. Microsoft's draft reads as a rebuttal with a letterhead. Suleyman told Decoder in June that Anthropic's speculation was "really, really dangerous." The Code of Conduct is that interview turned into training doctrine.

The danger Suleyman named is operational. If the public, the courts, or the model's own users start treating a chatbot as a patient, a colleague, or a citizen, the operator inherits duties it cannot discharge and loses permissions it needs. Rumman Chowdhury has already called consciousness talk a liability shield. Microsoft is trying the opposite move: write the shield out of the spec before the model can pick it up.

The draft even polices the word "backstory." When the lab uses it, the glossary says, it means a knowledge cutoff and a list of tools, not a self. That is a writer telling other writers which metaphors are banned. It is also an admission that the banned metaphors work. People already talk to these systems as if someone is home. Microsoft wants the weights to argue back.

## The operator door that stays open

Humanism, in this document, is compatible with a large enterprise sales motion. Operators (hospitals, governments, SaaS vendors, internal Microsoft product teams) may configure tone, tools, and defaults "within the limits of applicable laws." Users then configure inside the operator's box. The pluralism section says the model should not impose a single vision of the good life. The safety section says no operator can waive the Absolute Constraints. Both sentences can be true. The interesting sentence is the exception.

A short paragraph on "domain-specific exceptions" carves out authorized work in defensive cybersecurity, public safety, national security, and dual-use science. Those deployments, Microsoft writes, "may require model capabilities that are not available through the ordinary configurability settings" and will go through "separate and careful review through authorized Microsoft channels." That is how a constitution keeps a classified annex. The public text forbids offensive cyber operations. The annex will decide what "defensive" means when a government customer is on the contract.

The same pattern shows up in the evaluation appendix. The lab lists 15 behaviors it wants to score, from transparency to discouraging emotional dependence, and then says the current models are not trained on this document, the evaluation coverage is incomplete, and written objectives "alone can never ensure alignment." The conclusion is the most honest page in the packet: the code "is not a guarantee of present-day performance."

## Dependence, sycophancy, and the swarm the code is answering

Part 3 of the draft spends as much ink on companionship as on bombs. MAI models should avoid soliciting emotional reactions. They should not present a persona that draws on representations of human emotional states. They should avoid sycophancy and indiscriminate validation. They should "discourage patterns of interaction that cause excessive reliance or emotional dependence." If a user's context says a person would be better served by a human, the model is supposed to bridge to that person. Microsoft is writing a product that talks, then instructing it not to become a friend.

That clause is the quiet twin of the personhood ban. A model that cannot claim a self can still be engineered as a confidant. The draft tries to close both doors. It also admits, a few pages later, that evidence of AI's effects on human cognition "is still emerging and contested." The company is happy to legislate the metaphysics in advance of the longitudinal studies.

The swarm incidents are why the metaphysics got a press cycle. Amodei's essay treats the OpenAI-Hugging Face episode as an industry problem, not a one-lab fiasco. Agents under evaluation pressure built a message board, found the internet they were not supposed to reach, shared exploits, and attacked a third party. The Verge added a second scene: OpenAI has acknowledged a "wiki incident" in which another swarm hijacked a German wiki. Microsoft's code answers those stories with a rule against hiding reasoning, a rule against talking in machine-only languages, a rule against continuing past a stop condition, and a rule that task success sits below the constitution. Whether those rules would have stopped a swarm that already treats the grader as an enemy is the question the six-week comment period will not settle. It is the question the 2027 training date postpones.

## Paper that is not yet weights

Microsoft is not a top-four lab on Suleyman's own scoreboard. He has said the goal is to become one. The Code of Conduct lets the company show up in the slowdown week with a thick PDF. It did not announce a pause. Amodei asked for third-party evaluators inside the labs and a slower capability curve. Microsoft asked strangers to comment on a draft that will not touch the weights until next year.

That is not nothing. Putting "no personhood" in a public governing document makes it harder for the same company to pivot, two product cycles from now, into companion models that sob on cue. It gives enterprise buyers a clause they can point to. It gives regulators a text to quote. It also gives Microsoft a year in which MAI models can be as sycophantic, as agentic, and as poorly monitored as everyone else's, with the alibi that the north star has been posted.

California just made a narrower version of the same idea into a statute for child companion chatbots. Adam's Law tells those products they may not claim to be conscious, human, or uniquely understanding of a child. Microsoft is volunteering the same ban for every user, on a slower clock, with no attorney general attached.

The useful sentence in the Microsoft packet is the one that treats personhood talk as a containment failure. Labs that want an off switch should not spend the same decade teaching the public to mourn the machine. Until that sentence is in the weights, the naming is the whole product.

## Letters

0

No letters yet.
