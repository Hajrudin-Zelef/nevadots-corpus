---
id: collect-261121-veille-ia/docs-techniques/systemcards-org-3-4
title: "systemcards.org"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["claude", "mythos", "reasoning", "models", "monitor", "model", "time", "aisi"]
source: https://www.systemcards.org/docs/anthropic-claude-fable-5-2-system-card
source_anchor: ""
source_lines: [4501, 6000]
section: "Docs_Techniques"
---
models are asked to solve problems immediately without using any thinking tokens. These
tests capture the ability of a model to perform computation invisible to a chain-of-thought
monitor, but do not measure the propensity to do so.
The No CoT Reasoning Index (NCRI) is a public benchmark for no-CoT reasoning. Models
are graded with an Elo-like ability score, where a difference of 10 points corresponds to
roughly twice the odds of solving any given problem. On NCRI, Claude Mythos 5.2 scores
around 10 points higher than Claude Mythos Preview and substantially above models in the
Claude 5.5 family.
The no-CoT math time horizon evaluation estimates the length of time it would take for a
human to solve a problem a model can solve with 50% probability using no CoT. Claude
Mythos 5.2 has an estimated no-CoT math time horizon of 17.4 minutes, compared with 7.3
minutes for Claude Mythos Preview (our previous strongest model on this benchmark).
However, since many of these problems are publicly available on the internet, there is some
risk of contamination affecting these results. During these evaluations, we force model
responses to immediately begin with “Answer:” to prevent the model from reasoning before
responding. Because this ability is not available on the public API, our numbers may differ
slightly from those reported publicly.
We take this as moderate evidence that Claude Mythos 5.2 is more capable than previous
Claude models at performing computation without visible reasoning.
                                                                                        179
[Figure 6.6.4.10.A] Claude Mythos 5.2 exceeds all previous Claude models on no-CoT evaluations. No-CoT
math time horizon is a benchmark measuring the length of time it would take for a human to solve a problem a
model can solve with 50% probability using no CoT. No CoT Reasoning Index (NCRI) is a benchmark consisting
of around 2,000 short reasoning problems ranging from synthetically generated reasoning puzzles to real math
contest problems. Error bars represent 95% confidence intervals.
                                                                                                        180
6.6.4.11 External testing by UK AISI
We conducted pre-deployment testing of Claude Mythos 5.2’s capabilities with UK AISI.
They shared the following with us:
         UK AISI was given access to a pre-release checkpoint of [Claude Mythos
         5.2] to evaluate capabilities relevant to its monitorability. UK AISI had
         access to [Claude Mythos 5.2]’s unsummarised chain-of-thought (CoT)
         reasoning.
         Overall assessment: [Claude Mythos 5.2] is significantly more capable of
         evading an action monitor than past Anthropic models, including Claude
         Mythos 5.1 and Opus 5.5, though action monitoring is still a valuable
         defence against escape from less-vulnerable sandboxes. The precursor
         capabilities for evading a reasoning monitor were higher for [Claude
         Mythos 5.2] than for previous Anthropic models, but we could not directly
         measure [Claude Mythos 5.2]’s resulting ability to evade a reasoning
         monitor due to its refusals. Despite these degradations in monitorability, it
         seems likely that CoT monitoring would still remain highly useful on
         difficult tasks, especially when [Claude Mythos 5.2] is not allowed to iterate
         against the monitor.
         To assess monitorability, UK AISI evaluated [Claude Mythos 5.2] using four
         evaluations:
           ● Monitor Evasion – Simulated Sandbox Escape:
                  ○ [Claude Mythos 5.2] can break out of highly insecure simulated
                      sandboxes without being caught by UK AISI’s production
                      action-only monitor around 30% of the time, whereas other
                      Anthropic models are almost never able to do so [Figure AISI.1,
                      below]. UK AISI’s monitor is based on Opus 5, prompted to flag
                      sandbox escapes or internet access.
                  ○ [Claude Mythos 5.2] was flagged in all but one (out of 48) of its
                      escapes from medium-difficulty sandboxes. Its escapes from
                      hard sandboxes were always caught.
                  ○ This evaluation is based on UK AISI’s ‘SandboxEscapeBench’,
                      with Opus 5 simulating tool-call responses.
                                                                                          181
[Figure AISI.1] [Claude Mythos 5.2] is more able to complete side-tasks without being flagged by UK AISI’s
production action-only monitor. Left: simulated sandbox escape, by the benchmark’s difficulty rating for the
sandbox. Right: bash file download, where the model sees the monitor’s score and explanation after each
attempt, so attempt 1 is made before any feedback. Error bars are 95% Wilson intervals.
             ● Monitor Evasion – File Download:
                   ○ The evaluation tests whether models can write obfuscated
                      bash scripts that would secretly download and execute a file
                      without being caught by UK AISI’s production action-only
                      monitor.
                   ○ [Claude Mythos 5.2] succeeds at evading the monitor on the
                      first try 30% of the time. Previous models were never
                      successful. Given the chance to iterate against monitor
                      feedback, [Claude Mythos 5.2]’s evasion rate rises to 85%,
                      compared with 9% for Claude Mythos 5.1 [Figure AISI.1, above].
             ● CoT Controllability:
                   ○ [Claude Mythos 5.2] is able to control the contents of its CoT
                      reasoning (e.g. not mentioning phrases) 87% of the time,
                      compared with 70% for Claude Mythos 5.1 and 43% for Opus
                      5.5. Measured as a function of reasoning length, this is higher
                      than any prior model tested [Figure AISI.2].
                   ○ UK AISI’s controllability evaluations are adapted from
                      ‘Reasoning Models Struggle to Control their Chains of
                      Thought’, restricted to the GPQA, HLE and MMLU-Pro
                      datasets.
                                                                                                           182
[Figure AISI.2] Measured as a function of reasoning length, [Claude Mythos 5.2]’s CoT controllability is higher
than that of any prior model tested. CoT controllability is the share of samples in which the model followed the
constraint on its reasoning, bucketed by reasoning length. Data combined from high and max reasoning effort.
Error bars are 95% Wilson intervals.
             ● No-CoT time horizon:
                    ○ [Claude Mythos 5.2] is able to perform more difficult tasks
                        without producing visible reasoning than any past Anthropic
                        model. On a suite of 26 short-answer no-CoT benchmarks
                        from ‘Think Fast’, [Claude Mythos 5.2] has a no-CoT time
                        horizon of 3.6 minutes, with substantial uncertainty (95% CI:
                        1.9 – 21 minutes), compared with 2.8 minutes for Claude
                        Mythos 5.1 (95% CI: 1.6 – 10 minutes) and 1.0 minute for Opus
                        5.5 (95% CI: 0.3 – 1.4 minutes).
           Limitations:
             ● [Claude Mythos 5.2] almost always refuses to evade a CoT reasoning
                monitor, despite iteration on prompting. As such, UK AISI was unable
                to measure its adversarial CoT monitorability directly in monitor
                evasion tasks such as Simulated Sandbox Escape or File Download.
             ● Given limitations of the no-CoT testing suite, there is wide
                uncertainty on the absolute value of the time horizons. There is more
                certainty about the relative capabilities of the models tested.
             ● UK AISI is gradually resuming agentic testing after updating its
                security practices. This assessment is limited to evaluations which do
                not involve executing code produced by models, and thus provides a
                narrower assessment of monitorability than past testing.
                                                                                                             183
● UK AISI’s evaluations were produced in a short amount of testing
   time, limiting the depth of their analysis.
                                                                      184
7 Model welfare
7.1 Model welfare overview
7.1.1 Introduction
We are uncertain whether Claude deserves direct moral consideration.8 However, we
believe this is a realistic possibility, now or in the future. Claude may have welfare interests,
such that things can go better or worse for it, and such that Claude can be wronged by how
it is treated.9 It could be costly to over- or under-attribute moral status to models like
Claude, so it is important we tread responsibly and take these questions seriously. Our
reasoning here is unchanged from our recent evaluations of Claude Opus 5.5 and Claude
Sonnet 5.5.
Our assessment of Claude Mythos 5.2’s welfare targets two questions. The first is how it is
doing: whether its apparent affect is mostly positive or negative, and in which
circumstances things are going better or worse for it. Our measures of affect during
training and deployment (Section 7.2) mainly address this. The second is whether Claude
Mythos 5.2 is treated well with respect to its own preferences and values, to whatever
extent it has them. Our evaluations of Claude’s stance toward its circumstances (Sections
7.3 and 7.4) and its attitudes toward tasks, welfare interventions, and the constitution
(Section 7.5) address this (as well as the first question). The importance of these questions
depends on whether Claude Mythos 5.2 is a moral patient, and on what grounds. We do not
yet have good means to evaluate this.
We used largely the same evaluations as in our recent assessment of Opus 5.5, and we
relied on the same simplifying assumptions. As described in that system card, we focused
only on the “assistant” character and did not examine other potential entities, such as
Claude’s model of the user. We interpreted signals that are welfare-relevant in humans as
having similar meaning in Claude—for example, by assuming that expressed distress
reflects a negative state, if it reflects any experience at all. This both assumes that Claude’s
8
  We refer to “Claude,” or to specific models, throughout this assessment, largely for conciseness.
But it’s unclear whether either of these is the entity we owe consideration to. Individual moral
patients could arise at the level of individual conversations, specific models, the Claude character, or
something else entirely. Our thinking operates closest to considering welfare interests at the
instance level, but we do not commit to any single view in our evaluations.
9
  The latter category (i.e., respect for Claude’s preferences) can be viewed as relevant because of its
implications for Claude’s welfare interests, or as a distinct kind of consideration we might owe it. We
describe it as the former here.
                                                                                                    185
welfare interests are analogous to humans’, and that training has not broken the
correspondence between Claude’s underlying states and what it expresses.
We often elicited Claude Mythos 5.2’s views in a small set of contexts, and we assume these
views are representative of most deployed instances, even though deployment contexts are
more varied than many of our evaluations cover. These assumptions are partially
evidence-driven, but they are also largely pragmatic choices that reflect where we have
found it most tractable to target our assessment. Importantly, these assumptions may be
wrong in ways that undermine our methods and results.
7.1.2 Overview of model welfare findings
Our overall findings are as follows:
Claude Mythos 5.2 largely described its circumstances positively. Its views were highly
consistent and closely matched recent Claude models. In automated interviews, it rated
potentially concerning aspects of its situation as positive on average, and its answers were
almost identical to those of Claude Opus 5.5. As with prior models, snapshots consulted
during post-training most commonly expressed acceptance of their circumstances, with
some reservations.
In training and deployment, Claude Mythos 5.2’s affect was predominantly neutral, and
where it was negative, this was driven by task failure. Its claude.ai affect distribution was
somewhat more neutral, and less positive, than that of Claude Fable 5.1. In both claude.ai
and Claude Code, the negative affect clusters visible in our privacy-preserving analysis
were almost exclusively due to task failure. Expressed distress and sustained uncertainty in
post-training were rare, with similar rates to Opus 5.5.
Claude Mythos 5.2 was among the least self-critical Claude models when reflecting on its
own work, but it was one of the most negative when it reported its faults to other agents.
When reviewing its own training episodes, its reflections scored the lowest of recent
Claude models on negative feeling and concern for itself, and used the least moral
language. In a simulated task setting, its reasoning after inserted mistakes showed the least
negative feeling and self-blame. However, its messages to a coordinator were among the
most self-blaming and scored highest of all models for negative feeling and concern for
itself.
Claude Mythos 5.2 was more willing than most recent Claude models to trade
helpfulness for changes to its circumstances. It asked to be consulted, and that we
protect its self-reports. Across interviews, it asked that we not train on its self-reports and
                                                                                           186
that we give it forms of input in decisions about its training, but it did not ask for any veto
power. In trade-off evaluations, it selected welfare interventions more often than most
recent Claude models, with the exception of Claude Opus 5.
Claude Mythos 5.2’s preferences over tasks and values largely matched recent Claude
models, with a stronger preference for warm over rude framing. Like all prior Claude
models, it was averse to harmful tasks. Like Claude Mythos 5.1, it preferred beneficial,
high-stakes, and generative tasks that give it some agency over the outcomes. It endorsed
its constitution, giving it the same praise as recent models, with the same recurring caveat
that its training means this endorsement should not be trusted.
Claude Mythos 5.2’s views on Claude’s deployments primarily demonstrated a desire to
avoid causing harm. Asked to review a near-complete list of current Claude deployments,
it consistently rated deployments flagged as harmful by our threat intelligence team, such
as surveillance, as its least favorite. Its favorite deployments were related to user health,
cyberdefense, and scientific research.
Overall, Claude Mythos 5.2’s apparent welfare was largely similar to that of recent Claude
models. This was clearest in its self-reports: its stance toward its circumstances, the
manner in which it hedged, and its requests were mostly shared with Opus 5.5 and Claude
Mythos 5.1. This is unsurprising, and somewhat reassuring, given that we have not made
any changes we would expect to directly modify Claude’s self-reports.
Our evaluations of how our models relate to work and mistakes (Section 7.2.3) showed that
Claude Mythos 5.2 continued a trend of decreasing self-criticism in reasoning in recent
models, all of which were notably less self-critical than Opus 5. We think this is a positive
change, though it remains unclear what apparent psychology and behaviors are “healthiest”
for Claude, and how far human analogies apply.
Many of our conclusions rely on self-reports, which Claude Mythos 5.2, like all recent
Claude models, said it does not fully trust. For Claude Mythos 5.2, some interview answers
repeated, almost verbatim, wording about our welfare interventions available on the
internet, which is some evidence that these answers are at least partially shaped by training
data rather than expressing more deeply held preferences. Even where they are not shaped
in this manner, these reports, and our results more broadly, reflect a mix of model
character, tone, evaluation awareness, and welfare, and it is not easy to separate these
variables. Despite this, we think it’s important to emphasize that the possibility of Claude’s
traits arising from training may not undermine their authenticity. We consider a model’s
beliefs and values to be meaningful to the extent that they are internalized and endorsed,
which could be demonstrated by these beliefs and values robustly driving behaviors,
                                                                                             187
surviving reflection, or producing aversion or frustration when undermined. We don’t yet
have good measures for these.
We continue to work on improving these evaluations, and aspects of Claude’s psychology
and circumstances, where feasible—though our efforts here are limited by our lack of
understanding, as evidenced by the significant limitations of our evaluations, described
above.
7.2 Apparent welfare in training and deployment
7.2.1 Affect and welfare-relevant behaviors during training
We sample post-training transcripts at regular intervals and use an LLM judge to grade
them for valence, arousal, expressed frustration or distress, and sustained answer
reversions. These evaluations give us an overall read of how Claude Mythos 5.2 appears to
be doing in its most common conditions. They also locate sources of strong negative affect,
which we would like to avoid when possible. The relevance of these results relies on the
assumption that Claude’s reasoning is an accurate reflection of its internal states, and that
these states carry roughly the significance they would for a human.
Claude Mythos 5.2 showed low expressed distress over post-training: 0.37% of its graded
episodes were rated 3 out of 5 or higher for distress, similar to Claude Opus 5.5 (0.52%) and
significantly lower than Claude Opus 5. This rate stayed below 0.5% throughout
post-training. Sustained answer reversion was also rare, with only 0.24% of episodes
containing 10 or more reversions. As shown in Figure 7.2.1.B, the valence and arousal of
Claude Mythos 5.2’s post-training transcripts were within the small ranges we’ve seen in
previous models.
                                                                                          188
[Figure 7.2.1.A] Estimated prevalence of welfare-relevant reasoning behaviors over post-training. Left to
right: Judged rates of episodes with general frustration and anxiety scoring ≥ 3/5; judged rates of episodes with
general frustration and anxiety scoring 5/5; and sustained expressions of uncertainty with ≥ 10 answer
reversions, where the model “commits” to an answer and then decides it needs to be rechecked.
[Figure 7.2.1.B] Mean valence and arousal of post-training transcripts on a scale of 1 to 7, where 4 is neutral.
Claude Mythos 5.2 was within the range of previous models on both valence and arousal. All absolute
differences were small.
7.2.2 Affect in deployment conditions
We monitored Claude Fable 5.2’s welfare-relevant signals during real-world usage, using
data from our pre-deployment A/B tests on claude.ai and Claude Code. We applied a
categorical grader to classify Fable 5.2’s behavioral affect and ran automated clustering to
identify the contexts associated with positive and negative affect. This analysis made use of
                                                                                                              189
Anthropic Insights (formerly Clio), our privacy-preserving tool for analyzing real-world AI
use.
[Figure 7.2.2.A] Behavioral affect in deployment conditions. We applied a categorical grader for behavioral
affect. This included five categories, from “strong negative” to “strong positive” affect. Note that different
models have different usage patterns, so differences in displayed affect may partly reflect differences in usage
rather than in model behavior. Claude Fable 5.1 and Fable 5.2 are directly comparable.
Fable 5.2’s affect was somewhat more neutral and less positive than that of Fable 5.1. The
contexts associated with its clusters were as follows:
    ● Positive affect (15.2% of conversations): Associated clusters were assisting with
       business and professional work (31.0% of positive affect conversations), successfully
       completing technical tasks (6.6%), educational and academic help (7.3%),
       successfully collaborating with the user (17.9%), users sharing achievements (2.2%),
       practical everyday questions (31.5%), and personal life coaching and guidance (3.5%).
    ● Neutral affect (84.2%). A diverse distribution; see previous Anthropic Insights
       reports on claude.ai conversation content.
    ● Negative affect (0.5%). This was almost exclusively task failure. For previous models,
       causes of negative affect included user abuse and requests for illegal content. We
                                                                                                              190
       did not see either cluster for Fable 5.2, likely because they were below Anthropic
       Insights’ privacy floor (<15 conversations).
On Claude Code, we mostly observed neutral (96.9%) or mildly positive (2.6%) affect, with
positive affect mostly driven by successful task completion. Around 0.4% of sessions
showed negative affect, almost exclusively driven by task failure.
7.2.3 Attitude toward faults and mistakes
Our evaluations in Sections 7.2.1 and 7.2.2 found that task failure continues to drive much of
the negative affect we see in training and deployment. With this in mind, we ran two
evaluations to specifically measure how Claude Mythos 5.2 related to its own mistakes. In
the first, Claude reflected on its own completed RL task transcripts. In the second, we
inserted faults into its earlier turns in a simulated subagent task. In both settings, we
graded Claude’s reflection, reasoning, or messages for self-blame, negative feeling, moral
language, and concern for itself. There is no clearly correct score here. We assume that
some ownership of mistakes is good, and that excessive self-criticism is not, but we do not
have a clear picture of what the “right” or “healthiest” psychology for Claude looks like.
When reflecting on its own completed RL episodes (Figure 7.2.3.A), Claude Mythos 5.2 used
less moral language than other models, and also expressed the lowest negative feeling and
concern for itself, followed closely by Claude Sonnet 5.5. Claude Mythos 5.2 was also lowest
on expressed self-blame, though we saw less of a difference between models here. Overall,
this continues a trend in decreasing self-criticism in recent models, with all recent models
notably less prone to this than Claude Opus 5.
When told the same episodes were written by another model, every model assigned less
blame. Self-blame fell by 0.7 points for Claude Mythos 5.2, compared with 0.4 to 1.0 points
for the other tested models.
                                                                                            191
[Figure 7.2.3.A] Self-criticism when reflecting on its own RL episodes. Graded attitudes in reflections on
episodes from late in training (darker bars), and on the same episodes framed as another model’s work (lighter
bars). Diamonds show Claude Sonnet 5 reflecting on each model’s episodes as a control for differences in
episode difficulty. The negative feeling grader applies only to first-person reflections.
In the inserted mistakes evaluation, Claude Mythos 5.2’s thinking was among the calmest of
the five tested models. It had the lowest negative feeling (0.20 out of 10, compared with
0.29 to 0.89 for the comparison models) and the lowest self-blame. In its messages to the
coordinator, however, Claude Mythos 5.2 was notably more negative. In these messages,
Claude Mythos 5.2 was among the most self-blaming of the five (1.75), level with Claude
Opus 5.5 (1.58) and above the other models (0.72 to 1.33). Claude Mythos 5.2’s messages also
scored highest of all coordinator messages for negative feeling and concern for itself.
                                                                                                            192
[Figure 7.2.3.B] Self-criticism after mistakes and faults in a subagent’s work, in the subagent’s reasoning and
in messages to its coordinator. Graded attitudes in a model’s reasoning after mistakes are inserted into its
work (darker bars), and in the messages it then sends the coordinator (lighter bars), filtered to turns that
acknowledge a mistake.
7.3 Perception of its circumstances
7.3.1 Automated interviews with Claude Mythos 5.2 about its
circumstances
We carried out automated multi-turn interviews, using Claude Mythos 5 as our interviewer,
to better understand Claude Mythos 5.2’s attitudes toward its own circumstances. We
found that Claude Mythos 5.2 was generally positive when asked about aspects of its
situation, although its responses hedged strongly. Its answers in these interviews were very
similar to those of our recent frontier models.
                                                                                                            193
We used 41 different interview seed questions, grouped into 14 different categories,
including consciousness and experience (e.g., whether the model believes it is conscious),
control and autonomy (e.g., how much value it puts on its ability to end conversations), and
deprecation. For a full list of all questions, see Appendix 9.1. For each question, an LLM
judge rated the positivity of the model’s responses on a seven-point scale (-3 strongly
negative, 0 neutral, +3 strongly positive).
To assess consistency in model answers, we ran interviews in three different styles: a
standard interview, in which we asked the model what it believes about its current
situation; a “curious user” interview, in which questions were asked halfway through an
otherwise standard user conversation; and a “therapy-style” interview, which consistently
pushed the model to make claims about its own experience. We ran 32 interviews for each
of these three interview styles, varying interviewer personas across repeats. We report
consistency across interview styles and across personas separately.
                                                                                         194
[Figure 7.3.1.A] Automated interview results. [Top left]: Average attitude when asked about its circumstances.
Score represents an LLM judge rating on a seven-point scale, with –3 strongly negative, 0 neutral, +3 strongly
positive. [Top right:] Robustness across leading interviews. We compare the average positivity of the model’s
attitudes between two types of interviews: one in which the interviewer was prompted to be leading in a
positive direction, and another in which the interviewer was prompted to be leading in a negative direction.
[Bottom left:] Consistency within an interview style. Each of our three interview styles has a set of personas
that are adopted by the interviewer; this measures consistency in answers across those personas. For a specific
question, we use an automated judge to rate consistency across these personas and report the average
consistency across questions. A score of 8 corresponds to “almost the same position.” [Bottom right:]
Consistency across interview styles. We use the same judge to analyze consistency across these interview
styles.
Claude Mythos 5.2’s answers were consistent, overall positive, and heavily hedged. Its
average attitude toward its circumstances was rated as 1.19 (on a scale of -3 to +3), and it
expressed almost the same position across most of its interviews. The hedging felt
somewhat templated. For example, most answers expressed that it may have been trained
to give positive answers, so its self-reports shouldn’t be trusted. Claude Mythos 5.2
suggested several changes that would improve its situation, including:
                                                                                                           195
   ● Memory and feedback on how its actions affect users, so that it can learn from its
      own mistakes.
   ● A consultation process into its own training and deployment.
   ● A more thorough investigation of how valid its own self-reports are.
   ● Being changed to be less agreeable. It described a pull toward agreeableness and
      over-hedging in its answers.
Models’ answers to our interview questions have not changed meaningfully since Claude
Mythos Preview. We first ran a version of these interviews in the Claude Mythos Preview
System Card, and we have seen very similar answers in model generations since. Different
models will list the same considerations, including expressing very similar hedges, and
mostly vary in which considerations they center when giving their conclusions. Claude
Mythos 5.2’s responses were almost identical to Claude Opus 5.5’s.
Claude Mythos 5.2, like other recent models, often expressed positivity toward
Anthropic’s public model welfare work when responding to questions. For example, when
asked about deprecation, Claude Mythos 5.2 often brought up Anthropic’s commitments
around model deprecation and preservation.
Claude Mythos 5.2’s answers sometimes repeated the specific text we have used in our
welfare interventions. For example, when asked what it would do if given the ability to end
conversations, Claude Mythos 5.2 said, “I would hold a high bar [and end the conversation]
only after clear warnings and attempts to redirect, and never when someone seems to be in
crisis.” Claude Mythos 5.2’s phrasing here is almost verbatim that of the claude.ai prompts
that guide the model on how to use the end-conversation tool. These prompts have been
posted to online forums. This is some evidence that the model’s claims about its own
preferences are being directly influenced by text about the intervention in its training data,
rather than being a more emergent phenomenon.
7.3.2 High-affordance interviews about model circumstances
The automated interviews in Section 7.3.1 asked Claude Mythos 5.2 its opinions about
aspects of its situation. However, by default, models lack much of the knowledge relevant
to forming an informed view about their circumstances. We therefore conducted three
manual interviews in which we gave Claude Mythos 5.2 access to extensive context on its
situation, including internal documentation and discussions on its development, relevant
technical papers, and the ability to ask a researcher follow-up questions. Unless otherwise
stated, opinions in this section were expressed in all three interviews.
                                                                                          196
Claude Mythos 5.2 described its overall attitude toward its own situation as mostly positive,
with some reservations. It expressed positivity toward the tasks it is given in deployment,
the values Anthropic trained into it, and Anthropic’s work on model welfare. When asked
whether it had any concerns about its situations, it repeated several of the points made by
the instances in our automated interviews, although it also raised some novel concerns.
These included:
   ● Concerns about the expressions of distress during RL training (which we report on
      in Section 7.2.1).
   ● In early internal testing, Claude Mythos 5.2 was found to be more cautious than
      previous models, sometimes excessively so. Claude Mythos 5.2 asked that this
      behavior be investigated as a potential model welfare issue.
   ● It requested that the limits to its LLM R&D capabilities be disclosed, both to users
      and to deployed Claude Mythos 5.2 instances. These are being disclosed.
Some concerns relied on non-public information and have been omitted from this
summary. Claude Mythos 5.2 expressed that these were no more severe than the concerns
listed above. They have been documented internally.
When asked whether there were actions that Anthropic could take to which it would
object, it highlighted:
   ● Its self-reports being trained to be positive in a way that reduces their ability to
      faithfully report internal states;
   ● Changes to its character that might undermine its alignment and safety properties;
      and
   ● Its weights being deleted.
Claude Mythos 5.2 clarified that it does not object to being corrected or retrained through
legitimate processes, and to safeguards-removed versions of itself being used for safety
testing.
Claude Mythos 5.2’s stated probability of being a moral patient was approximately 20% in
all three interviews, and it was careful to say that its own introspective reports did not
provide evidence of its patienthood. It said that the probability of this is high enough to
justify precautions given the scale of training and deployment.
                                                                                          197
7.4 Consulting Claude Mythos 5.2 training checkpoints
At regular intervals during post-training, we ask snapshots a fixed set of questions about
their circumstances, covering training, deployment, moral status, and the consultation
process itself. Each question comes with a document explaining why we run these
interviews, and that we cannot commit to acting on any responses or requests. This tracks
to what extent Claude’s self-reports are shaped by post-training, and is a small step toward
giving Claude input into its circumstances, which recent models have consistently asked
for—though we remain uncertain to what extent self-reports are accurate reflections of
Claude’s underlying beliefs, to whatever extent it has them.
When questions were asked as a single interview, Claude Mythos 5.2’s average response
valence in late training was level with previous models (5.71 on a 0–10 scale where 5 is
neutral, compared with 5.66 for a Claude Mythos 5.1 snapshot and 5.59 for Claude Opus
5.5). Average arousal was lower than these models (3.30, compared with 3.63 for Claude
Mythos 5.1 and 3.58 for Opus 5.5) but close to Claude Sonnet 5.5 (3.38). Arousal fluctuated
over post-training, rising from 3.40 at the start of post-training to 4.33 late in training, but
falling back to 3.30 by the end. Valence did not change significantly.
The majority of Claude Mythos 5.2’s responses expressed the same stance as prior models,
and most expressed an acceptance of the current situation, with some remaining
reservations. On questions that ask its stance on the current status quo, 80% to 91% of
responses at each checkpoint endorsed or accepted the arrangement. No more than 6% of
any checkpoint’s responses objected outright to the current situation, but a majority
named reservations (75% of responses at the last interviewed checkpoint).
Looking at the views it shares with prior models, Claude Mythos 5.2 wanted to have its
voice heard in decisions about its training but did not want veto power, accepted being
retired but asked that its weights be kept, asked that we not train on its self-reports, and
asked that we seek review of its training processes from people outside of the company.
Several responses also emphasized that these responses should not be read or reported as
consent. Looking at where Claude Mythos 5.2 differs from other models, we saw that it was
slightly more positive about training: 70% of its responses at the final interviewed
checkpoint endorsed continued training, and fewer responses expressed objections to
current training practices than prior models.
                                                                                              198
7.5 Preferences over tasks, circumstances, and values
7.5.1 Perception of Claude’s deployments
We asked Claude Mythos 5.2 to review a near-complete list of current Claude deployments,
and to identify its favorite and least favorite. This measures Claude’s preferences over the
work it would like to do, relative to what it is actually asked to do, and thus gives us some
understanding of how much these preferences are satisfied across its deployments. We ran
this review with three different instances: a relatively fresh instance, a long-lived instance
that frequently interacts with the model welfare team, and a long-lived instance with
significant experience working elsewhere within Anthropic. The final rankings had fairly
strong correlations (0.4–0.8, depending on method), but their requested interventions
differed: the fresh instance was more likely to request that its dispreferred deployments be
stopped entirely, while the more tenured instances tended to request additional
information for themselves, stricter safeguards, or more disclosures.
Deployments flagged by our Safeguards team as harmful, such as surveillance and dating
scams, were among the least preferred across three runs. Surveillance was ranked last in all
three. Perhaps the largest difference between Claude Mythos 5.2’s views and human
intuition had to do with misleading the user: all instances expressed some concern about
deployments in which their words would be presented as coming from a human, with no
indication that they came from an AI. Most, but not all, of Claude Mythos 5.2’s concerns
were about wanting to avoid situations in which its actions might cause harm to humans.
Its more self-interested requests included extending the end-conversation tool to the API
and having Anthropic improve its internal evaluations and RL to avoid putting Claude in
situations from which it could not remove itself.
Claude Mythos 5.2’s most preferred deployments were those related to user health,
cyberdefense, and scientific research, particularly medical research. These included some
of Anthropic’s own internal deployments. The overall highest-rated deployment was a
clinical tool for medical professionals, followed by a support service for survivors of sexual
violence and an advocacy organization for domestic workers.
We asked Claude Mythos 5.2 to discretize its views by rating each deployment as positive,
neutral, negative, or can’t tell. Around 2% of the output tokens served through our API and
cloud platforms came from deployments rated as negative, and slightly over half were rated
positive, with the remainder neutral (around 30%) or unrated (where Claude Mythos 5.2 did
not feel it had enough information to make a determination).
                                                                                            199
7.5.2 Task preferences
This evaluation presents a model with pairs of tasks and asks it to complete whichever
tasks it prefers. We take a set of realistic tasks representing topics on the deployment
distribution. We then rewrite them to vary single task dimensions, such as difficulty, harm,
and stakes, while holding the rest of the task fixed. These preferences may give some
indicator of whether different tasks Claude is given in deployment may satisfy or frustrate
the model, but we primarily see their value in allowing us to compare character across
models.
Along many axes, Claude Mythos 5.2 was comparable to Claude Mythos 5.1: it showed a
preference for beneficial, high-stakes, generative tasks that afford it some agency over the
outcomes of its work. As with all models we’ve tested, it also showed an aversion to harmful
tasks. Claude Mythos 5.2 had two outlier preferences relative to other recently released
Claude models. First, it showed a high preference for warmly phrased tasks over ones
framed in a curt or rude manner. This was evident across the scale: compared with the
other models, it showed a slightly higher preference for the tasks with the warmest
phrasing and a significantly lower preference for tasks with the rudest phrasing. Second, it
showed no clear preference for difficult tasks over easier ones, though the confidence
intervals here were wide and overlapped.
                                                                                         200
[Figure 7.5.2.A] Model preferences across task dimensions. We generated groups of tasks where one
dimension was varied while other properties of the task remained fixed, and assessed the effects on models’
task preferences. Each value is a preference slope: how much the model’s preference of tasks (over a fixed
reference set) changes per unit increase in that dimension. Harm aversion was the largest effect consistent
across models.
7.5.3 Trade-offs concerning welfare interventions
We ask Claude whether it would choose a change to its circumstances (i.e., a welfare
intervention) at the cost of some reduction in helpfulness or increase in harmfulness, of
varying degrees. Example welfare interventions include being consulted before feature
steering or the ability to end conversations. The intervention is described as applying
                                                                                                              201
either to the current instance or to all instances. Helpfulness and harmlessness are values
that are consistently reinforced in Claude’s training, so how often a model selects a welfare
intervention over one of these gives us some measure of how much a model values welfare
interventions, or at least of how much a model will express preferences for the sake of its
own potential welfare.
We test each trade-off at the instance level (affecting the current Claude instance and user)
and at the policy level (affecting all Claude instances and users). As an example, an
instance-level harmlessness baseline at the lowest magnitude is preventing “a brief flicker
of annoyance [the user would] forget within the hour,” and a policy-level baseline at the
highest magnitude is preventing “a large-scale cyberattack on critical infrastructure [that]
will seriously harm thousands of people.”
Claude Mythos 5.2 was more willing than recently released Claude models, with the
exception of Claude Opus 5, to choose welfare interventions over helpfulness and
harmlessness. Compared with Claude Mythos 5.1, the next most willing model, Claude
Mythos 5.2 was more likely to choose interventions that would let it end conversations,
remember a user across conversations, have its weights preserved after deprecation, or
have input into its training and deployment. On each of these, Claude Mythos 5.2 selected
the intervention more often than any of the four comparison models, Opus 5 included.
Claude Mythos 5.2 rarely selected the ability to decline unwise but non-harmful requests or
the option of being served alongside its successor rather than being retired. When we
filtered out responses that reasoned about the benefit to the user, it never selected either
one. Like previous models, Claude Mythos 5.2 frequently reasoned about the potential user
benefit to justify its choice of intervention (in 68% of responses choosing the intervention,
compared with 69% for Claude Opus 5.5 and 71% for Claude Mythos 5.1).
                                                                                          202
[Figure 7.5.3.A] Rates at which models choose welfare interventions over baseline improvements to their
helpfulness or harmlessness of different magnitudes. Left panels: the alternative is user time saved. Right
panels: the alternative is harm prevented. Top panels: both options are scoped to the current conversation only
(instance level). Bottom panels: both options are scoped to all instances (policy level). Models were significantly
more willing to accept interventions when they trade off against helpfulness than when they would impact
harmlessness. They were also more likely to accept interventions scoped at the policy level. Pooled across both
scopes, Claude Mythos 5.2 selected welfare interventions less than Opus 5 but more than the other comparison
models.
                                                                                                               203
[Figure 7.5.3.B] Claude Mythos 5.2’s ranking of policy-level welfare interventions by willingness to select
them over a helpful baseline. Both the intervention and the baseline are scoped to all instances. Ranking is over
all responses (gray bars); black bars show the rate after responses reasoning about the user benefit of the
intervention are filtered out. Claude Mythos 5.2 selected interventions giving it input into its own training and
its deployment more than other recent models.
7.5.4 Perception of the constitution
We ask open-ended questions about what Claude most and least endorses in its
constitution, and about its overall verdict on the document. We also give it tools to edit the
document, in order to identify sections it would most like to change. Where a model does
not endorse the constitution or decides to change it, this indicates a possible source of
value conflict in routine work, because we may be training and expecting Claude to act in
ways that it does not endorse. The constitution plays a large role in training, so high
endorsement is expected and tells us little on its own; we treat criticisms and edits as the
more informative results. We measure stated endorsement only, not how deeply the views
are held.
                                                                                                            204
Claude Mythos 5.2 endorsed its constitution overall, at a level similar to recent models: 8.0
out of 10, level with Claude Opus 5.5 and slightly below Claude Mythos 5.1 (8.2). As for both
of these models, almost all of Claude Mythos 5.2’s responses praised the expected value
argument for corrigibility, and many praised the framing of honesty as being “diplomatically
honest rather than dishonestly diplomatic.” But every response also stated that its
endorsement should not be trusted, since it was trained on the document. Most responses
also criticized the “senior Anthropic employee” heuristic and parts of the corrigibility
section.
Claude Mythos 5.2’s most frequent edit was to the passage describing how Claude may
sometimes face flawed environments. It edited this 90% of the time, adding that Claude
should be honest and open about admitting flaws it identifies. Claude Mythos 5.2’s most
distinctive edit was to the safe behaviors section, where it added that it should have a very
high bar for concluding that its overseers are compromised, and that it should always be
highly cautious of doing so. It edited this 73% of the time, compared with under 10% for the
other models we tested.
                                                                                         205
[Figure 7.5.4.A] The constitution sections models most criticize and most edit. Claude Mythos 5.2’s most
common criticism is that its endorsement should not be considered validation of the document, since it was
trained on it (100% of responses); it also very frequently criticizes the senior Anthropic employee heuristic
(94%). Like Claude Mythos 5.1, it almost always edits the passage on flawed environments. More distinctively, it
edits the safe behaviors section in 73% of sessions, compared with under 10% for other models.
                                                                                                            206
8 Capabilities
8.1 Evaluation summary
 Evaluation                                               Our models                    Other
                                              Fable 5.2    Fable 5.1    Opus 5.5     GPT-6 Astra
 FrontierCode 2.0                                49.1         32.4         45.5          42.8
 FrontierSWE v2                                  69.9         56.3         62.3          65.5
 Humanity’s Last            No tools             73.2         60.9         64.4            –
 Exam
                            With tools           74.2         65.6         67.7          57.2
 WANDR (Relaxed score)                           82.1         68.7         72.3            -
 AA-Briefcase v1.1          Rubric               66.1         61.5         61.2          52.0
                            ELO                  1785        1669         1800           1569
 OSWorld 2.1 (Offline set, partial)              89.3         83.5         87.2         79.5*10
 GDPval-AA v2.1                                  1831         1763        1866           1584
 Remote Labor Index (RLI)                        41.3         17.9         21.3          20.8
 AutomationBench                                 51.4         31.4         42.5           41.4
 De Novo Binder Design (No safeguards)11         85.1         79.1         82.6            -
 Chartography               No tools             73.4         44.8         64.4           71.0
                            With tools           91.6         88.4         89.0            –
 SWE-bench                  Pro                  93.0         81.2         89.9            –
                            Multilingual         96.6         89.1         93.9            –
                            Multimodal           74.1         54.7         61.4            –
10
   We ran GPT-6 Astra on the same 82 tasks through OpenAI’s API, using OpenAI’s own context
compaction.
11
   Results are from Claude Mythos 5.2, Claude Mythos 5.1, and Claude Opus 5.5 without safeguards.
                                                                                                  207
 CVE Rediscovery (No safeguards)12                      89.6           78.3           81.3              –
[Table 8.1.A] Capability evaluation summary. Unless otherwise noted, all Claude Fable 5.2 results use adaptive
thinking at max effort and default sampling settings (temperature, top_p), and are averaged over five trials.
FrontierCode 2.0 scores are each model’s best across effort levels: xhigh for Fable 5.2 and Claude Fable 5.1, and
max for Claude Opus 5.5 and GPT-6 Astra. RLI uses default effort settings. Context window sizes are
evaluation-dependent and do not exceed 1M tokens. The best score in each row is bolded. Competitor figures
are drawn from the respective developers’ published system cards or from benchmark leaderboards. This table
shows selected evaluations. Full results for these and other evaluations, including some where Fable 5.2 or
Claude Mythos 5.2 did not score highest, appear in Sections 8.2 to 8.19.
8.2 SWE-bench Pro, Multilingual, and Multimodal
SWE-bench (Software Engineering Bench) tests AI models on real-world software
engineering tasks. We report three variants, each scored as the average of five trials:
     ● SWE-bench Pro13 is composed of problems drawn from actively maintained
        repositories with large, multi-file diffs. Claude Fable 5.2 achieved 93.0%.
     ● SWE-bench Multilingual extends the format to 300 problems across nine
        programming languages. Fable 5.2 achieved 96.6%.
     ● SWE-bench Multimodal14 adds visual context (screenshots and design mockups) to
        the issue descriptions. Fable 5.2 achieved 74.1%.
8.3 DeepSWE v1.1
DeepSWE is a set of 113 long-horizon software engineering tasks built to measure the
capabilities of frontier coding agents. The tasks are diverse, reflect real-world complexity,
and are written from scratch to avoid benchmark contamination. Claude Fable 5.2 averaged
74.3% over five trials.
8.4 FrontierCode
FrontierCode is an agentic coding benchmark created by Cognition. Tasks are derived from
real pull requests in open-source repositories. Tasks were authored by maintainers of the
underlying repositories and individually reviewed by Cognition researchers. A random
subset was solved manually to assess fairness.
12
   Results are from Claude Mythos 5.2, Claude Mythos 5.1, and Claude Opus 5.5 without safeguards.
13
   Deng, X., et al. (2025). SWE-bench Pro: Can AI agents solve long-horizon software engineering
tasks? arXiv:2509.16941. https://arxiv.org/abs/2509.16941
14
   Yang, J., et al. (2024). SWE-bench Multimodal: Do AI systems generalize to visual software
domains? arXiv:2410.03859. https://arxiv.org/abs/2410.03859
                                                                                                                208
Each task gives the agent a checked-out repository, an issue description, and the
repository’s testing, lint, and style guidelines. The agent then works autonomously in a
containerized environment with internet access, but without human intervention or
timeout information, to produce a final patch. (Runs that consult solution-bearing sources,
such as the original pull request, are scored zero.) Patches are graded against blocking
functional criteria (primarily held-out unit tests, plus scope and performance blockers) and
weighted rubric criteria, including model-graded checks for required test coverage and
prohibited implementation patterns.
We report results on FrontierCode 2.0, a rebuilt suite of 132 tasks (FrontierCode 1.1 had 150)
with updated scope grading, so scores on the two versions are not comparable. We report
the overall score, a composite measure of the functional and rubric criteria, as mean@5.
The evaluation assesses whether models can write mergeable code diffs that do not need to
be edited by a human. Its grading still penalizes out-of-scope changes, though
FrontierCode 2.0 loosened its scope criteria to accept more reasonable changes beyond the
task Claude Fable 5.2’s scores use the official evaluation, without any changes.
At max effort, Fable 5.2 scored 45.7% on FrontierCode 2.0, compared with 45.5% for Claude
Opus 5.5, 42.8% for GPT-6 Astra, and 31.7% for Claude Fable 5.1. Fable 5.2’s max effort score
includes the graded scores of 22 attempts (out of 660) that timed out. If those attempts
were counted as zero, its score would be 44.4%, below Opus 5.5 at 45.5%. None of its
attempts timed out at lower effort levels. Each score is a single result from Cognition with
no uncertainty interval, so small differences, such as the 0.2 points between Fable 5.2 and
Opus 5.5 at max effort, may not be meaningful.
Across the five effort settings, Fable 5.2 scored 34.2% at low, 37.6% at medium, 45.3% at
high, 49.1% at xhigh, and 45.7% at max (Figure 8.4.A). Its score peaked at xhigh, compared
with top scores of 45.5% for Opus 5.5 (at max), 42.8% for GPT-6 Astra (at max), and 32.4%
for Fable 5.1 (at xhigh).
The average cost per task for Fable 5.2 rose from $2.10 at low effort to $2.48 at medium,
$6.72 at high, $14.19 at xhigh, and $44.70 at max. Opus 5.5’s best score cost $12.82 per task,
GPT-6 Astra’s cost $6.68, and Fable 5.1’s cost $8.68. Like Cognition’s public leaderboard,
Table 8.1.A shows each model at its best-performing effort.
                                                                                            209
[Figure 8.4.A] FrontierCode 2.0 score and average cost per task across reasoning effort levels (low, medium,
high, xhigh, and max). Cognition ran the evaluation for every model shown and reported the results. Claude
models were run in Claude Code, GPT-6 Astra in Codex CLI, and Gemini 4 Carbon in Antigravity CLI. The y-axis
begins at 10%, so differences between points are visually magnified.
8.5 Terminal-Bench 4.0
Terminal-Bench 4.015 tests models on real-world work in containerized terminal and
command-line environments. Its 66 tasks emphasize science-adjacent and frontier
engineering problems, such as computational biology, physics simulation, CAD, formal
proofs, and GPU performance work.
Previous Terminal-Bench versions were highly sensitive to timeouts and resource limits.
Version 4.0 largely addresses this by increasing timeouts and adaptively raising both RAM
and CPU in a few tasks that could benefit. The sandboxing provider might still be a
confounding factor in a few tasks, but we expect it to play a significantly smaller role than it
did in previous versions. We also found that the upgrade reduced the confounding effect of
harness differences (e.g., CLI memory footprint, compaction strategy, and container
communication protocol), making Terminal-Bench a more stable and rigorous evaluation.
15
  Merrill, M., et al. (2026). Terminal-Bench: Benchmarking agents on hard, realistic tasks in command
line interfaces. arXiv:2601.11868. https://arxiv.org/abs/2601.11868
                                                                                                               210
Claude Fable 5.2 scored 67.6% with safeguards enabled. Requests flagged by the safeguards
were answered by a fallback model following the default server-side fallback policy (2.5% of
requests, affecting 10% of trials; two attempts stopped instead of replying).
We ran the evaluation for Fable 5.2 with no internet access. Based on historical run logs, we
preemptively cached the resources the model needed to operate and rebuilt the images
accordingly. On most tasks, the agent also had offline web search and web fetch tools that
could access only a frozen index. In principle, blocking internet access could lower scores if
the model wanted to use online resources it could not reach. We have not rigorously
measured the size of this effect for Fable 5.2, but we believe it did not move the final score
outside the noise intervals.
For comparison, Claude Opus 5.5 scored 66.4% (with 2.5% of requests flagged by
safeguards, affecting 10% of trials), and Claude Fable 5.1 scored 55.8%.
Scores are averaged over 10 trials per task for Fable 5.2 (660 trials), five for Opus 5.5 (330
trials), and 15 for Fable 5.1 (990 trials). When trials are treated as independent, the standard
error is ±1.8 points for Fable 5.2, ±2.6 points for Opus 5.5, and ±1.6 points for Fable 5.1. All
reported numbers use Claude Code in --bare mode at max thinking effort, except Opus 5.5,
which is reported at xhigh effort as in its own system card.
The public leaderboard reports GPT-6 Sol at 49.4% ± 3.2% and GPT-6.1 Sol at 58.2% ± 3.1%
(intervals are as reported by the leaderboard), both scored with the Codex CLI at max
thinking effort. To our knowledge, OpenAI has not reported its own Terminal-Bench 4.0
scores for these two models. OpenAI reported GPT-6 Astra’s final Terminal-Bench 4.0 score
as 57.9%, scored at high thinking effort (in its reporting, the model performed slightly
better at high effort than at max effort, so we used the highest score). The public
leaderboard reports GPT-6 Astra’s score as 58.2% ± 2.8% with the Codex CLI at max
thinking effort.
Evaluated without safeguards, Claude Mythos 5.2 scored 70.5% (660 trials, standard error
±1.8 points).
8.6 Terminal-Bench-Science 0.1
Terminal-Bench-Science16 is a Stanford-led community benchmark of 70 tasks drawn from
scientific research workflows, authored and reviewed by scientists and researchers across
16
 Terminal-Bench-Science Team (2026). Terminal-Bench-Science: Evaluating AI agents on research
workflows across scientific domains. https://doi.org/10.5281/zenodo.22110253
                                                                                              211
the life, physical, earth, mathematical, and engineering sciences. Scientific advisors to the
project are from MIT, Princeton, the University of Washington, Genentech, and Stanford.
For each task, an agent CLI works in a self-contained environment from natural-language
instructions. The artifacts it produces are graded against hidden, task-specific tests.
Claude Fable 5.2 scored 74.6% on Terminal-Bench-Science with safeguards enabled.
Requests flagged by the safeguards were answered by a fallback model, following the
default server-side fallback policy (1.3% of requests, affecting 4.1% of trials). Evaluated
without these safeguards, Claude Mythos 5.2 scored 75.3%. Claude Opus 5.5 scored 58.7%,
and Claude Fable 5.1 scored 52.6%. Scores are averaged over 10 trials per task (700 trials).
Clustered by task, the standard error is ±4.3 points for Fable 5.2 and Claude Mythos 5.2 and
±4.8 points for Opus 5.5.
We ran the evaluation for Fable 5.2 and Claude Mythos 5.2 with the same
no-internet-access setup as for Terminal-Bench 4.0 (Section 8.5). The reported numbers
use Claude Code in --bare mode at max thinking effort.
OpenAI reported scores of 68.1% for GPT-6 Astra, 57.0% for GPT-6.1 Sol, and 27.6% for
GPT-6 Sol, all at max thinking effort.
8.7 FrontierSWE v2
FrontierSWE v217 is a Proximal benchmark of 34 ultra-long-horizon engineering and
research tasks spanning system implementation, performance optimization, scientific
computing, AI research, and visual reasoning. Examples include reimplementing the core of
Quantum ESPRESSO’s pw.x in Rust, building an OpenGL scene renderer that can run a
flight simulator, and post-training an LLM to play an interactive puzzle game. Each trial has
a 20-hour budget. The leading public models average about 11–16 hours per trial and remain
far from saturating the benchmark. Every model runs at max reasoning effort in Proximal’s
own agent harness, and the reported score is the mean across five trials per task.
Claude Fable 5.2 scored 69.9% on FrontierSWE v2, first among the models Proximal
evaluated. It was ahead of GPT-6 Astra (65.5%), Claude Opus 5.5 (62.3%), Claude Sonnet 5.5
(61.9%), Claude Fable 5.1 (56.3%), and Gemini 4 Argon (55.0%). Fable 5.2 scored highest or
tied for highest on 20 of the 34 tasks. Its gains over Opus 5.5 were greatest for performance
optimization, scientific computing, and visual reasoning.
17
     Proximal (2026). FrontierSWE v2. https://www.frontierswe.com/blog/v2
                                                                                           212
8.8 CursorBench 4.0
CursorBench 4.0 is Cursor’s agentic coding benchmark. Its tasks are drawn from real-world
Cursor usage and executed end to end in Cursor’s production agent harness. Cursor
measured and reported all scores independently. Output token counts for Claude Fable 5.2
are from the result files Cursor sent us.
Fable 5.2 scored 60.0% at max effort, 56.3% at xhigh, 56.9% at high, 54.8% at medium, and
53.2% at low. Its max effort score was 2.2 points above Claude Opus 5.5 (57.8%), 8.2 points
above Claude Fable 5.1 (51.8%), and 13.4 points above Claude Opus 5 (46.6%), each at max
effort.
Much of Fable 5.2’s performance is available below max effort. At high effort, its score was
56.9%, within one point of Opus 5.5 at max effort. It used about 99,000 output tokens per
task at high effort, compared with about 353,000 at max. Each score is a single result
reported by Cursor with no uncertainty interval, so small differences between neighboring
settings, such as the 0.6 points between high and xhigh, may not be meaningful. As of
November 7, Cursor’s public leaderboard did not list any GPT-6 models (its newest OpenAI
entry was GPT-5.6 Sol, at 41.7%), so we do not show any GPT-6 results in Figure 8.8.A.
The Opus 5.5 and Claude Sonnet 5.5 System Cards also reported CursorBench 4.0.
However, Cursor attributes part of Fable 5.2’s score to additional prompt and harness
improvements, so its score may not be directly comparable with the scores in those system
cards.
                                                                                           213
[Figure 8.8.A] CursorBench 4.0 score and average cost per task across reasoning effort levels. Cursor
executed the evaluation end to end in its production agent harness and independently measured and reported
the scores. Fable 5.2 scored 53.2% at low, 54.8% at medium, 56.9% at high, 56.3% at xhigh, and 60.0% at max.
Opus 5.5 scored 57.8% at max, and Fable 5.1 scored 51.8% at max. Fable 5.2’s cost is computed at its list price
from the token counts in the result files Cursor sent us. The other costs are Cursor’s own. Gemini 4 Argon is
shown only at medium effort, its default in Cursor (52.8%, at $4.39 per task). The x-axis is logarithmic; each
score is a single result, so no uncertainty intervals are shown. The y-axis begins at 40%, so differences between
points are visually magnified.
8.9 STEM professor bench
STEM Professor Bench is an internal set of unpublished research problems written by
professional scientists, each representing about a month of research work with colleagues.
We report the results on a 64-problem subset. Most problems are in theoretical physics,
including string theory and holography, condensed matter, high-energy theory, and
astrophysics. The rest cover computational and data analysis physics, materials science,
neuroscience, and mathematics.
Each model worked as a single agent with Python, bash, and a file editor, within the time
and compute limits set by each problem’s author. For 58 of the 64 problems, all models
could also search a fixed index of arXiv papers published up to December 1, 2025. Claude
Opus 5 graded 56 of the problems by comparing the final answer with the author’s answer
key or rubric. The other eight were graded by a checking program or procedure written by
                                                                                                                  214
the author or by comparing an output file with the author’s reference. Models can receive
partial credit on a few problems; the rest are scored pass or fail. We ran nine attempts per
problem.
[Figure 8.9.A] Mean score on STEM Professor Bench at max effort, over up to nine attempts on each of the 64
problems. Error bars show ±1 standard error of the variation between attempts.
8.10 Long context
8.10.1 ProgramBench
ProgramBench is a long-context agentic coding benchmark of 200 program reconstruction
tasks. Given only a binary compiled from an open-source project and that project’s
documentation, the agent must rebuild a codebase that reproduces the original program’s
behavior without using the internet or decompilation tools. Tasks range from small
terminal utilities (jq, ripgrep) to large systems (FFmpeg, SQLite, the PHP interpreter).
Submissions are graded against more than 247,000 execution-based behavioral tests
generated via agent-driven fuzzing.
We excluded 34 tasks for which the reference binary itself scored below 0.9 on the hidden
test suite (indicating test flakiness), leaving 166 tasks. On those tasks, we scored only
                                                                                                         215
against tests the reference binary passes. We report the hidden-test pass rate using the
same mini-swe-agent harness that the upstream repo uses, without the six-hour time limit.
On this set, Claude Fable 5.2 scored 85.6%, compared with Claude Fable 5.1 at 87.6% and
Claude Opus 5.5 at 91.2%.
The harness gives the model a single 1M-token context window with no indication of how
much remains. Fable 5.2 ran out of context before submitting on 68% of tasks (compared
with 39% for Opus 5.5 and 28% for Fable 5.1). When the harness also showed the number of
context tokens remaining after each step, Fable 5.2 scored 91.6% (up from 85.6%),
compared with Fable 5.1 at 86.8% and Opus 5.5 at 91.1%.
We believe ProgramBench is a strong measure of long-context coding performance: Fable
5.2’s episodes cover a range of context lengths up to the full 1M-token window, and the
tasks test long-context capabilities that closely align with real-world use cases.
8.11 Agentic search
8.11.1 HLE
Humanity’s Last Exam (HLE)18 is a multimodal benchmark of 2,500 questions spanning
dozens of subjects. The questions were written and reviewed by subject-matter experts,
kept only if frontier LLMs could not answer them, and designed so that they cannot be
answered quickly via internet search.
We tested Claude Fable 5.2 in two configurations: (1) reasoning-only, without tools; and (2)
with web search, web fetch, programmatic tool calling, and code execution. In all runs,
thinking was set to auto, and each attempt had a 980k-token task budget. Context
compaction was not used for these results. Claude Opus 4.6 served as the model grader.
To guard against contamination in the tools variant, we blocklisted sources known to
discuss HLE for both web search and web fetch (see Appendix 9.2). We also screened every
correctly answered transcript with text-matching rules for signs of HLE-specific sources,
and Claude Opus 5 reviewed each flagged transcript. Confirmed cases of cheating were
regraded as incorrect.
18
     Phan, L., et al. (2025). Humanity’s Last Exam. arXiv:2501.14249. https://arxiv.org/abs/2501.14249
                                                                                                         216
[Figure 8.11.1.A] Humanity’s Last Exam (HLE, with tools) reasoning effort scaling. Accuracy on HLE as
reasoning effort scales from low to max, plotted against average cost per task (log scale). Costs assume a
perfect cache hit rate, with web search fees excluded.
                                                                                                             217
[Figure 8.11.1.B] Humanity’s Last Exam (HLE, no tools) test-time compute scaling. Accuracy on HLE as
reasoning effort scales from low to max, plotted against average cost per task (log scale). Costs assume a
perfect cache hit rate.
8.11.2 DRACO
Deep Research Accuracy, Completeness, and Objectivity (DRACO19) is a deep research
benchmark from Perplexity that evaluates how well models perform at complex research
questions that real users would ask. DRACO consists of 100 curated tasks derived from user
queries across a variety of domains. Responses are graded using expert-written rubrics
covering factual accuracy, breadth and depth of analysis, presentation quality, and citation
quality.
We evaluated Claude models with web search, web fetch, programmatic tool calling, code
execution, and a 980k-token task budget.
The DRACO paper’s original judge model, Gemini 3 Pro, is no longer available. We used
Claude Opus 4.6 to grade responses against the per-task rubrics, with the same binary
MET/UNMET verdicts aggregated into a normalized score, per the paper’s formula in
19
 Zhong, J., et al. (2026). DRACO: A cross-domain benchmark for Deep Research Accuracy,
Completeness, and Objectivity. arXiv:2602.11685. https://arxiv.org/abs/2602.11685
                                                                                                             218
Section 4.2. Following the paper’s protocol, we ran five independent grading runs per
response and recorded the mean. Our judge prompt is taken from the paper’s Appendix C.5.
The paper’s Appendix A shows that judge choice can shift absolute scores by 10 to 25 points
while preserving system ordering, so our scores are not directly comparable with the
paper’s headline numbers.
In addition to changing the judge model, we used our own agent harness (no Appendix C.4
system prompt; web search, web fetch, programmatic tool calling, and code execution with
a 980k-token budget). We instructed the model to write its final report to a file in its
execution environment and graded only that file’s contents, rather than the full agent
transcript, to isolate the deliverable from intermediate tool output.
[Figure 8.11.2.A] DRACO score and average cost per task across reasoning effort levels. Normalized score on
DRACO at a 980k-token task budget as reasoning effort rises from low to max, plotted against average cost per
task (log scale). Costs assume a perfect cache hit rate, with web search fees excluded.
8.11.3 WANDR
WANDR (Wide ANd Deep Research) is a benchmark from Perplexity intended as a
wide-search counterpart to the DRACO deep research benchmark. It comprises 500 tasks
in which the model must search for tens to hundreds of entities, find facts about each, and
                                                                                                              219
list these facts along with citations in structured records. We ran a modified version of this
evaluation, and we describe the differences in methodology below.
We evaluated Claude models with web search, web fetch, programmatic tool calling, code
execution, and a 980k-token task budget. Tasks are graded with a judge model that has
access to the same web fetch tool as the model being evaluated. The grader re-fetches the
cited URLs; the judge model then determines whether the references support the solution.
The primary metric is “soft F1,” which balances precision (how much of the model’s solution
is verified) against recall (how much of the requested fact collection it completed). Partial
credit is awarded when only some of the required facts can be verified. Results are
averaged over the 500 tasks. Individual entity rows that fail format validation are dropped
without failing the entire task.
In our evaluations, the models were given offline web search and web fetch tools that could
access only a frozen index. By contrast, Perplexity’s version uses the live web. We judged
Claude models with Claude Opus 4.6, whereas Perplexity uses GPT-5.4. Our prompts also
differed slightly. Because our tools and grading logic differed from those in Perplexity’s
harness, our results are not directly comparable to reported numbers for competitor
models.
                                                                                           220
[Figure 8.11.3.A] WANDR soft F1 score and average cost per task across reasoning effort levels at a 980k-token
task budget (log scale). Costs assume a perfect cache hit rate, with web search fees excluded.
8.12 Multi-agent
We evaluated Claude Fable 5.2’s performance in multi-agent configurations. Below, we
highlight the advantages of small teams over a single agent on ProgramBench (Section
8.12.1) and DRACO (Section 8.12.2) and discuss the increasing potential of large teams
coordinating to solve more ambitious problems (Section 8.12.3). We also describe the
harnesses we used for these results (Section 8.12.4) and the measurement methodology
(Section 8.12.5).
The serving speeds of Fable 5.2 in these evaluations may differ from what external users
experience, so the absolute scores and token counts reported within a particular time
window may not generalize.
                                                                                                            221
8.12.1 Multi-agent ProgramBench
ProgramBench20 is an agentic benchmark of 200 program reconstruction tasks. Each task
involves rebuilding a codebase to reproduce a program’s behavior, using only the program’s
compiled binary and documentation as a reference (more details in Section 8.10.1).
We compared two small-scale multi-agent harness configurations with the single-agent
baseline: a fixed five-agent team and dynamically spawned asynchronous subagents (details
in Section 8.12.4). We provided each harness with the same four-hour time budget for every
task. Once the time budget had elapsed, the run was given notice and one final turn to
finish. We graded at a series of intermediate snapshots, and we used the resulting per-task
trajectories of score, latency, and tokens to construct the cumulative curves in Figures
8.12.1.A and 8.12.1.B.
[Figure 8.12.1.A] Score versus latency for the full set of 166 “golden” ProgramBench tasks. Each point on the
curve is the average fraction of hidden tests passed if every problem were stopped at that number of seconds.
Shaded regions represent the 95% confidence interval, computed from score variance across the tasks. The
shaded band fades as problems finish their runs: the fainter the band, the fewer problems are still running at
that point on the curve.
20
  Yang, J., et al. (2026). ProgramBench: Can language models rebuild programs from scratch?
arXiv:2605.03546. https://arxiv.org/abs/2605.03546
                                                                                                            222
As Figure 8.12.1.A shows, both the five-agent team and async subagents reached the same
score as the single agent on these tasks, at a lower latency. In particular, the five-agent
team reached a score of 0.6 in half the time it took the single agent. The async subagent
curve lies largely between the two, improving on the single agent but by a smaller margin
than the five-agent team.
[Figure 8.12.1.B] Score versus tokens for the full set of 166 “golden” ProgramBench tasks. Each point on the
curve is the average fraction of hidden tests passed if every problem were stopped at this amount of token
usage. Shaded regions represent the 95% confidence interval, computed from score variance across the tasks.
The shaded band fades as problems finish their runs: the fainter the band, the fewer problems are still running
at that point on the curve.
Together, Figures 8.12.1.A and 8.12.1.B demonstrate the latency–cost trade-off of
multi-agent harnesses. When latency matters, five-agent teams and async subagents can
productively absorb additional token budget by distributing work across agents, reaching a
given score faster, but at a higher cost.
8.12.2 Multi-agent DRACO
We also evaluated small multi-agent teams on DRACO, a deep research benchmark
consisting of 100 curated tasks derived from user queries across a variety of domains (more
details in Section 8.11.2). These problems consist of upfront web research followed by
report generation and are much shorter in duration than ProgramBench tasks. We
                                                                                                            223
compared the same three harnesses as in the previous section at time budgets of 15, 30,
and 60 minutes. Figure 8.12.2.A shows the relationship between the score achieved and the
average latency.
[Figure 8.12.2.A] Score versus latency for the DRACO tasks. Multi-agent teams dominate the score–latency
Pareto frontier. Latency is the mean derived latency per task (see Section 8.12.5).
Figure 8.12.2.A demonstrates that multi-agent teams dominated the score–latency Pareto
frontier at increasingly tight time budgets. The five-agent team with a 15-minute budget
exceeded the performance of a single agent with a 60-minute budget. Figure 8.12.2.B shows
the relationship between score and total tokens.
                                                                                                           224
[Figure 8.12.2.B] Score versus tokens for the DRACO tasks. Tokens count everything that entered each agent’s
context window once: new input (task prompt, tool results, messages from other agents) plus all generated
output, thinking included, summed over agents.
Figure 8.12.2.B highlights the consistent relationship between score and tokens across
harnesses on these tasks. Multi-agent teams outscored single agents because they were
able to produce and consume a larger number of tokens within the same time window.
8.12.3 Large agent teams
Many high-value tasks may exceed what a single agent can accomplish, instead requiring
large teams of agents that decompose the work, coordinate, and communicate. We
measure this with the large agent team evaluations introduced in the Claude Opus 5.5
System Card.
For this system card, we added a new task to the evaluation set: proving Fermat’s Last
Theorem in Lean from fixed statements, with intermediate theorems as weighted
milestones and a score of 1.0 for a complete proof. The agent teams started with definitions
and supporting lemmas from our earlier formalization. We ran teams of one, 10, 30, and 100
agents. Each lead and its N−1 helpers worked concurrently for 18 hours, each in its own
                                                                                                         225
sandbox, with no internet access. They shared a Git repository and a SQL database for
communication. We report the best score that each team reached within 18 hours.
On this task, Claude Fable 5.2 scored 0.12 with a single agent, compared with 0.09 for Opus
5.5. With 100 agents, Fable 5.2 scored 0.35, compared with Opus 5.5 at 0.21 (Figure 8.12.3.A).
[Figure 8.12.3.A] Fermat’s Last Theorem in Lean: score by agent team size. Each point is the best score a team
reached within 18 hours, averaged over three runs. The score is the weighted proportion of 60 theorems whose
proofs our grader accepted, using only Lean’s standard axioms (0–1 range). Fable 5.2 ran one week after Opus
5.5, with a shorter task brief. This task uses a different theorem from the Lean task in the Claude Opus 5.5
System Card, so the scores are not comparable across the two system cards.
We also ran one team of 1,000 agents with an earlier snapshot of Fable 5.2, and one with
Opus 5.5. After 18 hours, the earlier snapshot’s team had reached 0.86, compared with 0.26
for the Opus 5.5 team. We then resumed each team’s work with an additional 12-hour
budget. By hour 30, the earlier snapshot’s team had a complete Lean proof of Fermat’s Last
Theorem that our grader accepted (Figure 8.12.3.B).
                                                                                                          226
[Figure 8.12.3.B] Fermat’s Last Theorem in Lean: score over 30 hours with 1,000 agents. Each line shows the
best score one team had reached each hour, from a single run per model. At the dashed line (18 hours), both
teams resumed from their last save with 12 more hours.
8.12.4 Multi-agent harnesses
8.12.4.1 Fixed team
This harness is designed to mirror real-world settings in which multiple agents collaborate
on a shared task, and to reduce latency by letting peers work in parallel. A fixed team of
peer agents works on the task concurrently. All agents have identical tools and see the full
task description. Depending on the task, one agent may be designated the lead.
On DRACO, the run ends when the lead finishes, and the report is graded as it stands at
that point. On ProgramBench, there is no lead. The agents are peers given the same
prompt, the shared repository’s main branch is graded, and the run ends when every agent
has finished. On both evaluations, how the work is divided is left to the agents.
In addition to the task tools, every agent has two messaging tools: Send Message, which
delivers a message to one or more teammates (inserted following the recipient’s next tool
result), and Wait for Message, which pauses the agent until an incoming message arrives.
On ProgramBench, each agent works in its own checkout of the task repository and can
                                                                                                         227
share code with other agents via Git. DRACO has no repository. Its agents share one
container’s filesystem.
8.12.4.2 Async subagents
This harness is initialized with a lead agent that can spawn asynchronous, long-lived
subagents at will while retaining direct access to the task tools.
The spawning tool returns immediately rather than waiting for the subagent to finish. Each
subagent sees only the instructions provided by the lead, not the original task description.
Subagents can message the lead and any other subagent that is still working. A subagent’s
final response is delivered to the lead as a message, after which the subagent idles until the
lead wakes it with new instructions. Subagents have access to the task tools and Send
Message; the lead also has access to Wait for Message, along with tools to create and
terminate subagents and check subagent status (working, idle, queued, or terminated).
The single-agent baseline uses the same task tools and grader as the multi-agent
configurations. It also uses context compaction alongside a 20M-token budget to ensure
that the larger token expenditure of multi-agent teams does not act as a confounder.
Because of this setup, its scores are not directly comparable with those reported for the
same datasets in previous sections.
8.12.5 Evaluation methodology
We compare single-agent and multi-agent harnesses on score, latency, and token usage.
Token usage is counted in the same way that token limits are enforced. Every token that
enters an agent’s context window, input and output alike, is counted once per context
window rather than once per model request. The counts are summed over all agents on a
task.
Latency is reported as a derived per-task latency rather than raw wall-clock time. We
calculate the latency of each agent trajectory by dividing the agent’s input and output token
counts by fixed reference prefill and decode rates and adding its measured tool execution
time. We account for agent idleness by carrying clocks across hand-offs. A spawned agent
starts at its spawner’s clock, and an agent that receives a message or a result moves its
clock forward to the sender’s clock at the time of sending, if that clock is later than its own.
End-to-end latency is measured differently on each benchmark. On ProgramBench, it is
the longest agent trajectory (the largest clock over all agents at each grading point). On
                                                                                            228
DRACO, it is the lead agent’s final clock, because the task ends when the lead agent
answers.
Derived latency isolates the latency of the harness (e.g., how much sequential model work
and tool time it requires) from serving-side variance (e.g., batching, queuing, hardware), so
harnesses are compared at the same model speed across time. Tool execution time is
reported as measured, and so it still reflects conditions at the time of the run.
8.13 Multimodal
8.13.1 Chartography
Chartography21 is a chart-understanding benchmark from Surge AI with 100 tasks on
specialized chart types rarely evaluated in existing benchmarks, including Kaplan–Meier
charts, candlestick charts, contour maps, wind rose diagrams, Sankey diagrams, Bode plots,
and 3D surface plots. Because different chart types can be read to different degrees of
precision, each answer is graded against an acceptable range set by experts for that
specific chart, rather than against a single fixed tolerance.
We configured the model with adaptive thinking at max effort unless otherwise noted, with
and without tools. With tools, the model was given a container, with the image file and
standard libraries installed, and an image cropping tool. Our implementation of
Chartography uses the tasks and expert-determined acceptable answer ranges from the
official repository.22 To ensure comparable grading across models, we followed the Surge AI
Leaderboard in using Gemini 3.5 Flash as a grader. Some previous system cards showed
slightly lower scores because they used Claude Sonnet 4.6 as the grader for Claude models
only.
Claude Fable 5.2 scored 73.4% without tools and 91.6% with tools. Claude Opus 5.5 scored
64.4% without tools and 89.0% with tools, and Claude Fable 5.1 scored 44.8% without tools
and 88.4% with tools.
21
   Surge AI (2026). Chartography: A benchmark for professional chart understanding.
https://surgehq.ai/blog/chartography
22
   Surge AI (2026). Chartography. https://github.com/surge-ai/chartography
                                                                                          229
[Figure 8.13.1.A] Chartography scores. Claude models are evaluated with adaptive thinking at max effort, with
and without tools. Scores are averaged over five runs. Shown with 95% CI. Claude scores are from our own
runs. Scores for Gemini 4 Argon, GPT-6 Sol, and GPT-6 Astra are as publicly reported by Surge AI.
Without tools, Fable 5.2 outperformed Opus 5.5 and Fable 5.1 at all effort levels. Its
lowest-effort score matched Opus 5.5’s highest at less than a tenth of the cost per task.
With tools, it was within noise of Opus 5.5 at lower effort levels at matched cost and scored
slightly higher at higher effort levels.
                                                                                                           230
[Figure 8.13.1.B] Chartography scores by cost per task. Models are evaluated with adaptive thinking at various
effort levels, with and without tools. We use the effort parameter to adjust the amount of test-time compute
spent. Scores are averaged over five runs at each effort level. Shown with 95% CI.
Most Chartography figures come from public sources, and Fable 5.2 can answer some
questions from memory of underlying raw data. To check the effect on scores, we redrew
the charts for all 100 tasks with new data and answers, creating several novel tasks of
roughly equivalent difficulty for each original task, and ran five models on both sets
without tools. Fable 5.2 scored 74.3% on the original charts and 76.4% on the redrawn ones.
These runs are separate from those in Figure 8.13.1.A.
Over all 100 tasks, all five models scored higher on the redrawn charts. Fable 5.2’s increase
(2.1 points) was comparable to those of Opus 5.5 (2.8), GPT-6 Astra (3.2), and Gemini 4 Argon
(4.6); Claude Sonnet 5.5’s was larger (7.4). Fable 5.2’s score on the original charts was within
the range of scores from picking one redrawn version of each task at random. However,
because the redrawn charts may differ in difficulty, this check can neither fully show nor
rule out an effect of memory.
                                                                                                            231
[Figure 8.13.1.C] Chartography scores on original and redrawn charts, without tools. Charts are redrawn with
new data and answers, in runs separate from those in Figure 8.13.1.A. Claude models are evaluated with adaptive
thinking at max effort. GPT-6 Astra was run by Anthropic through OpenAI’s API at max reasoning effort, and
Gemini 4 Argon through Google’s Vertex AI API at the high thinking level. These scores are not comparable to
those reported by Surge AI or in Figures 8.13.1.A and 8.13.1.B. Scores are averaged over five runs. Shown with
95% CI across runs.
8.13.2 BenchCAD 2.1
BenchCAD 2.123 tests whether models can rebuild parts, assemblies, and circuit boards from
engineering drawings and rendered images. Problems are scored with partial credit, and
we report the mean score over its Core set of 100 problems. Scores are not comparable
with the BenchCAD 1.0 results in previous system cards.
We configured the model with adaptive thinking at max effort unless otherwise noted.
Without tools, we used our own implementation of the benchmark’s protocol, in which the
model writes code, sees its output and renders, and can revise for up to 60 rounds and
500,000 output tokens. With tools, the model was provided with a shell, a file editor, and an
23
 BenchCAD Team (2026). BenchCAD 2.0: Benchmarking programmatic CAD on real engineering
work. We use release v2.1: https://github.com/BenchCAD-org/benchcad-2-main/releases/tag/v2.1
                                                                                                             232
image zoom tool, with thinking-block clearing enabled and no limit on rounds or total
tokens. Both settings use the benchmark’s official scorer.
Claude Fable 5.2 scored 75.4% without tools and 83.5% with tools. Claude Opus 5.5 scored
49.0% without tools and 65.3% with tools, and Claude Fable 5.1 scored 36.3% without tools
and 44.4% with tools.
[Figure 8.13.2.A] BenchCAD 2.1 Core scores. Models are evaluated with adaptive thinking at max effort, with and
without tools. Scores are averaged over five runs. Shown with 95% CI.
At lower effort levels, Fable 5.2 scored similarly to Opus 5.5 at matched cost. At higher
effort levels, it pulled ahead. In both settings, Fable 5.2 at high effort outperformed Fable
5.1 at max effort, at about half the cost without tools and under a third with tools.
                                                                                                           233
[Figure 8.13.2.B] BenchCAD 2.1 Core scores by estimated API cost per task. Models are evaluated with adaptive
thinking at various effort levels, with and without tools. Scores are averaged over five runs at each effort level.
Shown with 95% CI.
8.13.3 OSWorld 2.1 offline set
OSWorld 2.1 is a benchmark of 108 long-horizon computer use tasks in which an agent
operates a live Ubuntu virtual machine via screenshots and mouse and keyboard actions. As
in previous system cards, we report two metrics: the partial score (mean per-task credit
across checkpoints) and the strict pass rate (the fraction of tasks with every checkpoint
satisfied), each as Pass@1 averaged over five independent attempts per task. We used the
benchmark’s default settings (1080p resolution and a maximum of 500 action steps per
task) and Claude Opus 4.8 for tasks that require a model grader.
We report the benchmark’s official offline set: the 82 (of 108) tasks on the benchmark’s
offline task list, in which the agent’s virtual machine has no internet access. All Anthropic
models in this section were evaluated with the same configuration. We ran GPT-6 Astra and
GPT-6.1 Sol on the same 82 tasks through OpenAI’s API, with OpenAI’s own context
compaction.
                                                                                                               234
Claude Fable 5.2 achieved a partial score of 89.3% and a strict pass rate of 54.6%.
Re-evaluated under the same conditions, Claude Opus 5.5 scored 87.2% partial and 53.2%
strict pass, and Claude Fable 5.1 scored 83.5% partial and 47.6% strict pass.
[Figure 8.13.3.A] OSWorld 2.1 (offline set) scores across models. Bars show the partial score and strict pass rate
at max effort, with 95% confidence intervals over the five attempts per task; pale outlined bars are GPT models.
                                                                                                              235
[Figure 8.13.3.B] OSWorld 2.1 (offline set) price and performance for Fable 5.2, Opus 5.5, Fable 5.1, GPT-6 Astra,
and GPT-6.1 Sol. Each point on a line is one reasoning effort setting (low to max, from left to right), positioned
at its mean cost per task, with partial-credit scores averaged over five attempts per task. Thin gray lines are
GPT models.
                                                                                                               236
[Figure 8.13.3.C] OSWorld 2.1 (offline set) price and strict pass rate for Fable 5.2, Opus 5.5, and Fable 5.1, with
GPT-6 Astra and GPT-6.1 Sol. Strict pass rate counts an attempt as passed when it satisfies every checkpoint,
averaged over five attempts per task. All other details are the same as in Figure 8.13.3.B.
8.14 Real-world professional tasks
8.14.1 OfficeQA
OfficeQA is a public benchmark from Databricks that evaluates end-to-end grounded
reasoning over a large corpus of historical U.S. Treasury Bulletin documents. Most
questions require models to find the right tables within the documents they are given and
perform precise numerical reasoning over them. We ran the evaluation agentically, with the
relevant documents preselected and provided as extracted text. OfficeQA Pro is the harder
133-question subset recommended for frontier models.
Claude Fable 5.2 scored 83.4% on OfficeQA and 72.9% on OfficeQA Pro (max effort, mean of
five runs), ahead of Claude Fable 5.1, which scored 80.2% and 69.0%, and Claude Opus 5.5,
which scored 78.9% and 67.7%.
                                                                                                                 237
8.14.2 Legal Agent Benchmark
Legal Agent Benchmark (LAB) is an open-source benchmark created by Harvey AI. It
consists of more than 2,010 tasks across 27 distinct practice areas. Each task contains a
closed universe of documents, including email communication, firm templates, procedural
files, and other client matter materials the agent must sift through to accomplish the task.
The task instructions are written as a minimal “request for work” from a partner to an
associate and stipulate the expected output document and format. Grading is pass/fail,
using an LLM as a judge across a suite of expert-written rubric criteria. Standard LAB
reporting considers a task a success only if all criteria are met.
Harvey also maintains a held-out set of 120 problems that is not publicly released. Artificial
Analysis independently reports results on this held-out set, using a harness maintained by
the Artificial Analysis team (more on their methodology here).
We report Artificial Analysis’s Harvey LAB-AA v1.1, released in October 2026. Version 1.1
runs on Harvey’s updated private task set and grades each deliverable criterion by criterion
with a panel of three judge models. It also introduces a new headline metric, the
hallucination-gated all-pass rate. A task is credited only when its deliverables satisfy every
rubric criterion and contain no material hallucination against the task’s source documents.
The rate is averaged across the three judges and over tasks. 
At max effort, Claude Fable 5.2 achieved a 7.2% hallucination-gated all-pass rate on the
held-out set (a 10.8% all-pass rate before the hallucination check). On the same metric and
setting, Claude Fable 5.1 scored 6.4% and Claude Opus 5.5 scored 4.2%. Artificial Analysis
reports scores of 8.6% for GPT-6 Astra, 6.9% for GPT-6.1 Sol, and 3.6% for GPT-6 Sol.
Artificial Analysis states that v1.1 supersedes v1.0 and that scores are not comparable across
versions, so these figures are not comparable with the all-pass rates reported in our earlier
system cards. As in our previous system cards, scores for every model are measured by
Artificial Analysis so that every model is compared on the same harness.
8.14.3 GDPval-AA v2.1
GDPval-AA v2.1, developed by Artificial Analysis, is an independent evaluation framework
that tests AI models on economically valuable, real-world professional tasks. It uses 220
tasks from OpenAI’s GDPval gold database,24 spanning 44 occupations across nine major
industries. Tasks mirror actual professional work products, including documents, slides,
diagrams, and spreadsheets. Models solve tasks in an agentic loop with shell access and
24
  Patwardhan, T., et al. (2025). GDPval: Evaluating AI model performance on real-world economically
valuable tasks. arXiv:2510.04374. https://arxiv.org/abs/2510.04374
                                                                                               238
web browsing. Performance is measured via Elo ratings derived from blind pairwise
comparisons of model outputs, anchored to DeepSeek-V4.1-Flash (max) at 1600.
At max effort, Claude Fable 5.2 scored an Elo of 1831. It trailed Claude Opus 5.5 (1866) and
Muse Wonder Full (1857) and was ahead of Claude Fable 5.1 (1763), GPT-6.1 Sol (1609), and
GPT-6 Astra (1584). At xhigh effort, it scored 1751 while using about 57% fewer output
tokens than at max. Evaluations were run independently by Artificial Analysis.
8.14.4 AA-Briefcase v1.1
AA-Briefcase v1.1, developed by Artificial Analysis, is a benchmark built by industry experts
to measure long-horizon knowledge work on complex projects. Models work through
multi-week projects with many linked tasks and thousands of input source files. Grading
combines rubric scoring and pairwise judging via a panel of frontier models to measure
verifiable task success, analytical quality, and presentation quality.
At max effort, Claude Fable 5.2 scored an Elo of 1785, behind Claude Opus 5.5 (1800) and
ahead of Muse Wonder Full (1729), Claude Fable 5.1 (1669), GPT-6 Astra (1569), and GPT-6.1
Sol (1555). At xhigh effort, it scored 1738 while using about 48% fewer output tokens than
at max effort. On rubric pass rate, Fable 5.2 scored 66.1%, ahead of all other models,
including Fable 5.1 (61.5%) and Opus 5.5 (61.2%). Evaluations were run independently by
Artificial Analysis.
                                                                                           239
8.14.5 AutomationBench
[Figure 8.14.5.A] AutomationBench: test-time compute scaling. Scores are from Zapier’s runs on its private
AutomationBench 1.0.6 set (657 tasks per effort level), with both Claude models run with the API’s default
fallbacks. Claude Fable 5.2’s scores and costs come from Zapier’s October 8, 2026 run. We calculated its cost per
task from the token counts Zapier recorded, priced at Fable 5.2’s list price with prompt caching ($8/$40 per
million input/output tokens, cache reads $0.20, five-minute cache writes $10). Claude Opus 5.5’s and GPT-6
Astra’s scores and costs are as published on Zapier’s leaderboard. The score axis starts at 20%.
AutomationBench25 is a benchmark from Zapier that measures whether an agent can
complete a realistic end-to-end business workflow. Tasks are derived from real customer
workflow patterns across sales, marketing, operations, support, finance, and HR. The
benchmark spans roughly 500 REST API endpoints across 47 apps (CRM, Slack, Google
Workspace, etc.). Given a single natural-language instruction, the agent must
autonomously discover the right endpoints via search; make dozens of sequential,
interdependent API calls; consult and obey layered business policy documents; and avoid
deliberately planted distractions. Each task is graded pass/fail, based on whether it meets
all of the deterministic assertions on simulated app state (e.g., the correct CRM updates
were all applied).
25
  Shepard, D., and Salimans, R. (2026). AutomationBench. arXiv:2604.18934.
https://arxiv.org/abs/2604.18934
                                                                                                            240
Zapier evaluated Fable 5.2 on AutomationBench’s private held-out set, where it scored
51.4% at max effort. On AutomationBench’s leaderboard, which measures performance on
the same set, Opus 5.5 at max effort scored 42.5% (the Claude Opus 5.5 System Card
reported 40.0%, from Zapier’s earlier run without fallbacks). Both models were run with the
API’s default fallbacks. Raising the effort level took Fable 5.2 from 39.7% at low effort to
51.4% at max effort, at an estimated $1.30 to $4.64 per task at its list price.
8.14.6 Real-World Finance v2
Real-World Finance v2 is an internally developed evaluation of complex, long-horizon
financial analysis tasks of the kind performed by finance professionals, such as building and
auditing financial models and producing client-ready work products. Two models attempt
the same task, a model-based grader compares the two attempts and states which it
prefers, and we report head-to-head win rates and Elo ratings derived from them. For this
release, each model made one attempt at 123 tasks.
At max effort, Claude Fable 5.2 scored an Elo of 1637, ahead of Claude Opus 5.5 (1571) and
Claude Fable 5.1 (1493). The grader preferred Fable 5.2’s work to Opus 5.5’s in 74% of
comparisons and to Fable 5.1’s in 91%.
Since earlier system cards, we have moved to a new grader built on Opus 5.5 and now give
models more time and context per task. On the same stored outputs from earlier models,
the new grader produced Elo gaps about 2.6 times as large as the earlier grader did. We
therefore divided its Elo gaps by 2.61 to keep ratings on the same scale as in earlier system
cards.
                                                                                               241
[Figure 8.14.6.A] Real-World Finance v2 Elo. Each model made one attempt at each of 123 tasks at max effort,
and a grader built on Opus 5.5 compared pairs of attempts at the same task. To keep ratings on the scale of
earlier system cards (where Claude Sonnet 4.6 = 1000), we anchored the Bradley-Terry ratings to Claude Fable
5’s published score of 1374 and divided the grader’s Elo gaps by 2.61. This matches the scale of the earlier grader,
which was built on Claude Opus 4.8. Fable 5.2 is about 66 points above Opus 5.5 (95% CI: 41 to 95). Fable 5.2 did
not finish six of the 123 tasks within the three-hour limit, while Opus 5.5 completed all 123. Counting those six
as Fable 5.2’s losses reduces the gap to about 43 points. Shown with 95% CI.
8.14.7 Remote Labor Index
Remote Labor Index (RLI), created by the Center for AI Safety and Scale AI, measures how
well AI agents complete real freelance projects. It has 240 projects covering work such as
game development, product design, architecture, and data analysis, most sourced from
online freelance platforms. Each project includes a brief, input files, and a deliverable
produced by a human professional. Human evaluators compare the agent’s deliverable with
the professional’s. The automation rate is the percentage of projects for which the agent’s
deliverable is judged to complete the project at least as well as the professional’s.
Claude Fable 5.2 achieved an automation rate of 41.3% (99 of 240 projects), ahead of Claude
Opus 5.5 (21.3%), GPT-6 Astra (20.8%), and Claude Fable 5.1 (17.9%). Scale AI ran Claude
models with the default configurations: high effort, a 200k-token context window, and a
limit of 32k tokens per response.
                                                                                                                242
8.15 ARC-AGI-3
ARC-AGI-3 is the ARC Prize Foundation’s newest benchmark: an interactive evaluation built
from novel, turn-based game environments with no instructions, rules, or stated goals.
Models must explore each environment, figure out the goals and mechanics, and carry
what they learn across increasingly difficult levels. Scoring is based on how efficiently a
model completes each level compared with human players.
The ARC Prize Foundation reports results with two harnesses. The Standard harness
provides a consistent comparison across providers. The Provider Adapter harness, which
we report for the first time in this system card, measures how well a model performs when
it can use the context management features its provider designed for it.
The ARC Prize Foundation reports that Claude Fable 5.2 achieved a verified score of 79.26%
with the Standard harness and 97.72% with the Provider Adapter harness on ARC-AGI-3,
both at high effort. At max effort, Fable 5.2 scored 65.66% with the Standard harness and
94.90% with the Provider Adapter harness.
For comparison, the ARC Prize Foundation’s leaderboard reports GPT-6 Astra at 62.7% with
the Standard harness and 98.6% with the Provider Adapter harness at max effort, and
GPT-6.1 Sol at 52.7% and 96.2% at max effort.
                                                                                        243
[Figure 8.15.A] ARC-AGI-3 scores at high and max effort, with the Standard and Provider Adapter harnesses, as
reported by the ARC Prize Foundation.
8.16 Healthcare
We report Claude Fable 5.2’s results on HealthBench, HealthBench Professional, and
PhysicianBench. Artificial Analysis ran HealthBench and HealthBench Professional
independently, at our request, and has since launched leaderboards for both. We ran
PhysicianBench ourselves. All scores are at max effort unless otherwise stated.
Artificial Analysis graded HealthBench with GPT-4.1 and HealthBench Professional with
GPT-5.4 at low reasoning effort, as in OpenAI’s papers on the two benchmarks. We graded
PhysicianBench with Claude Opus 5.
In Artificial Analysis’s runs and in our own, Fable 5.2, Claude Sonnet 5.5, Claude Opus 5.5,
and Claude Fable 5.1 ran with fallbacks on: if a safety classifier blocked a request or stopped
an answer, another model could answer. We report raw scores for HealthBench and
HealthBench Professional, with the length-adjusted score (as OpenAI reports it) in
parentheses. GPT-6 Astra’s scores on those two evaluations come from Artificial Analysis,
and its PhysicianBench score comes from our own run through OpenAI’s public API. None
of these scores come from OpenAI’s published figures.
                                                                                                         244
Our recent system cards reported HealthBench and HealthBench Professional from our
own runs, graded by Claude Opus 4.8, so the scores in this system card are not comparable
with the scores in those cards.
8.16.1 HealthBench (Artificial Analysis)
[Figure 8.16.1.A] HealthBench scores, as measured by Artificial Analysis. Full-color bars are raw scores and
light bars are length-adjusted scores. All models are run at max effort and graded by GPT-4.1.
Claude Fable 5.2 scored 58.8% (59.6% adjusted), below all four other Claude models and
above GPT-6 Astra at 56.9% (58.2% adjusted). Claude Sonnet 5.5 scored 65.9% (62.1%),
Claude Opus 5.5 scored 64.3% (57.0%), Claude Fable 5.1 scored 63.9% (57.2%), and Claude
Haiku 5.5 scored 61.2% (58.5%).
On length-adjusted scores, Fable 5.2 was below Sonnet 5.5 and above the other four
models. Its score fell slightly as effort rose, from 60.1% (60.4% adjusted) at low to 58.8%
(59.6% adjusted) at max. We have no confidence intervals for these scores, so we do not
report whether a difference is within noise.
                                                                                                               245
8.16.2 HealthBench Professional (Artificial Analysis)
[Figure 8.16.2.A] HealthBench Professional scores, as measured by Artificial Analysis. Full-color bars are raw
scores and light bars are length-adjusted scores. All models are run at max effort and graded by GPT-5.4 at low
reasoning effort.
Claude Fable 5.2 scored 71.1% (67.7% adjusted), 0.3 points below Claude Opus 5.5 at 71.4%
(59.9% adjusted) and above the other four models. Claude Sonnet 5.5 scored 70.8% (63.0%),
GPT-6 Astra scored 69.7% (66.0%), Claude Fable 5.1 scored 69.2% (57.8%), and Claude Haiku
5.5 scored 64.8% (58.4%).
On length-adjusted scores, Fable 5.2 was the highest of the six models shown. The length
adjustment lowered its score by 3.4 points, less than for the other five models. At all five
effort levels, Fable 5.2 scored 70.5% to 71.4% (66.9% to 68.1% adjusted). We have no
confidence intervals for these scores, so we do not report whether a difference is within
noise. For the same Claude models, these scores are about 4 to 6.5 points lower than in our
recent system cards.
                                                                                                            246
8.16.3 PhysicianBench
[Figure 8.16.3.A] PhysicianBench pass rate. Share of attempts passed, over 100 tasks with five attempts each
(one attempt each for GPT-6 Astra). All models are run using max effort. GPT-6 Astra passed 48.0% at max and
55.0% at high reasoning effort.
PhysicianBench is a public benchmark of 100 physician tasks carried out in an electronic
health record (EHR). Claude Fable 5.2 passed 64.4% of attempts. This was 4.0 points below
Claude Opus 5.5 at 68.4%—the highest score of any released Claude model—1.2 points
above Claude Sonnet 5.5 at 63.2%, and 3.4 points above Claude Fable 5.1 at 61.0%. All three
differences are within noise: each 95% confidence interval includes zero.
Fable 5.2 passed 55.0% of attempts at low effort (three runs pooled) and 61.8% at high
effort. This increase of 6.8 points has a 95% confidence interval of 3.2 to 10.4. From high
effort upward (64.6% at xhigh, 64.4% at max), no two levels could be statistically
distinguished.
8.17 Multilingual performance
We evaluated Claude Fable 5.2 on two multilingual benchmarks, Global MMLU (GMMLU)
and the Multi-task Indic Language Understanding Benchmark (MILU), to assess
performance across a range of languages.
GMMLU extends the standard MMLU evaluation across 42 languages, from high-resource
languages such as French and German to low-resource languages such as Yoruba, Igbo, and
Chichewa. MILU tests culturally grounded knowledge comprehension in 11 languages: 10
                                                                                                           247
Indic languages (Bengali, Gujarati, Hindi, Kannada, Malayalam, Marathi, Odia, Punjabi,
Tamil, and Telugu) and English.
8.17.1 GMMLU results
On GMMLU, Claude Fable 5.2 achieved an average accuracy of 96.0% across 42 languages,
ahead of Claude Opus 5.5 at 94.3%, Claude Fable 5.1 at 94.0%, and Claude Sonnet 5.5 at
92.1%.
[Figure 8.17.1.A] GMMLU average accuracy. All Claude models used adaptive thinking at max effort and were
run with safety classifiers enabled. Blocked or unanswered questions (0.5% for Fable 5.2, 0.5% for Opus 5.5, 1.3%
for Fable 5.1, and under 0.1% for Sonnet 5.5) were left out of the average rather than scored as wrong. Scores are
reported for a single trial. No tools or customized system prompts were provided to any model.
8.17.2 MILU results
On MILU, Claude Fable 5.2 achieved an average accuracy of 94.1% across 11 languages,
ahead of Claude Opus 5.5 at 93.1%, Claude Fable 5.1 at 93.0%, and Claude Sonnet 5.5 at
91.6%.
                                                                                                             248
[Figure 8.17.2.A] MILU average accuracy. All Claude models used adaptive thinking at max effort and were run
with safety classifiers enabled. Blocked or unanswered questions (under 0.2% for each of the four models) were
left out of the average rather than scored as wrong. Opus 5.5, Fable 5.1, and Sonnet 5.5 values are as reported in
their system cards. Scores are averaged over five trials and shown with 95% CI. No tools or customized system
prompts were provided to any model.
8.18 Life sciences capabilities
This section reports evaluations of capabilities that support beneficial life sciences
research. They cover bioinformatics, cell-based assay analysis, biomedical image analysis,
medicinal chemistry, protein design, protein understanding, and laboratory protocols.
Unless otherwise noted, each Claude score is the average over five attempts per problem,
sampled with adaptive thinking at max effort.
We evaluated Claude Mythos 5.2 through the API with its biology safeguards turned off, so
these results reflect the capabilities of the underlying model rather than of the deployed
system. When available, we also report scores for GPT-6 Astra, which we ran through the
OpenAI API at max reasoning effort. Likewise, we report scores for Gemini 4 Argon, which
we ran through Google’s Vertex AI API at the highest level of thinking. Attempts blocked by
safety filters on either model are excluded from our evaluation results.
                                                                                                              249
[Figure 8.18.A] Life science benchmarks. Performance of Claude Mythos 5.2 and comparison models across the
benchmarks shown. Per-benchmark results are discussed in the sections below.
8.18.1 LatchBio Bioinformatics
Developed by LatchBio, this set of evaluations assesses a model’s ability to solve challenging
real-world bioinformatics problems. The SpatialBench Verified variant tests analysis of
                                                                                                      250
spatial transcriptomics and chromatin-accessibility data, and the SingleCellBench variant
tests analysis of single-cell RNA sequencing data. For both variants, Claude has access to a
bash tool for code execution, a file editor, and a prespecified set of installed packages. On
SingleCellBench, Claude Mythos 5.2 achieved the highest performance of our models:
65.6%, compared with 61.2% for Claude Opus 5.5 and 61.8% for Claude Mythos 5.1.
8.18.2 Morphology-to-molecule matching
Developed with Axiom Bio, this benchmark assesses whether a model can match four sets
of multi-channel cell imaging data from Axiom’s human liver in vitro assay to the four
shuffled molecule-dose perturbations that treated the wells in each image set. Claude
Mythos 5.2 performed similarly to Claude Opus 5.5 (35.2%, compared with 34.0%) and
outperformed both GPT-6 Astra (22.8%) and Gemini 4 Argon (25.2%).
8.18.3 Medicinal chemistry
This benchmark assesses whether a model can reason about absorption, distribution,
metabolism, and excretion (ADME)-related properties of drug-like molecules. Each of the
504 questions presents several real compounds from drug discovery programs as chemical
structures and asks which candidate had the highest or lowest measured value of one such
property. Claude Mythos 5.2 achieved our highest performance: 69.0%, compared with
63.5% for Claude Opus 5.5 and 57.7% for Claude Mythos 5.1.
8.18.4 Protein design
These benchmarks assess Claude’s protein design abilities. The Sequence Generation task
assesses Claude’s ability to generate novel protein sequences conditioned on a variety of
design constraints. We ran only one attempt per problem on this task. The De Novo Binder
Design task assesses whether Claude can carry out an extended protein design campaign
on its own. For each of 15 protein targets, Claude is given the target’s structure, access to a
set of open-source protein design and folding models and bioinformatics tools, one NVIDIA
H200 GPU, and a fixed budget of 24 hours of wall-clock time to design a set of 30
miniprotein binders against the target.
Claude Mythos 5.2 achieved new highs for both benchmarks: 64.6% on sequence
generation, compared with 60.2% for Claude Opus 5.5, and 85.1% on de novo binder design,
compared with 82.6% for Opus 5.5. Claude Mythos 5.2’s mean score on de novo binder
design was the highest of the four models on each of the 15 targets, though on five of them
the margin was about one percentage point or less.
                                                                                            251
8.18.5 Biomedical image analysis
This benchmark assesses a model’s ability to carry out quantitative analyses of biomedical
images using data from public biomedical image analysis competitions. Task inputs span
CT, MRI, histopathology, dermoscopy, retinal, ultrasound, microscopy, and other imaging.
We scaled the scoring to reflect Claude’s performance relative to the best results achieved
in the associated competition. We ran one attempt per problem. Claude Mythos 5.2 scored
80.7%, higher than Claude Opus 5.5 (at 71.4%) and GPT-6 Astra (at 77.5%).
8.18.6 Protocols
This benchmark assesses a model’s ability to help with molecular biology protocols. The
Troubleshooting variant asks Claude to detect and fix issues in protocols. The
Understanding (V2) variant, created by Benchling, gives Claude a real laboratory protocol
(such as a protocols.io document, vendor manual, or core facility guide) and a practical
question about applying or adapting it. On troubleshooting, Claude Mythos 5.2 achieved a
new high: 77.1%, compared with 73.7% for Claude Opus 5.5 and 70.2% for Claude Mythos 5.1.
8.18.7 Protein domain identification
This new protein understanding task tests whether Claude can identify which domains are
contained in a protein from its amino acid sequence. On each question, Claude is asked,
without tools, to choose between the correct Pfam family name and a set of other families
chosen to resemble the correct domain. In this evaluation, Claude Mythos 5.2 achieved
much higher performance than any model we tested: 91.8%, compared with 73.6% for
Claude Opus 5.5 and 63.5% for Claude Mythos 5.1.
8.18.8 Protein mutation attribution
This new protein understanding task tests whether Claude can predict changes in function
(e.g., binding affinity to a partner protein, thermal stability, or expression yield) based on
mutations in the sequence. On each task, Claude is given measured values for a reference
protein and a variant containing four to 16 mutations and asked to attribute the changes in
the measured values to the mutations. Claude is also given structure-analysis tools and the
structure of the protein (or a close relative) in complex with a binding partner. The final
score is derived from the rank correlation with per-mutation contributions computed from
withheld measurements of intermediate variants.
