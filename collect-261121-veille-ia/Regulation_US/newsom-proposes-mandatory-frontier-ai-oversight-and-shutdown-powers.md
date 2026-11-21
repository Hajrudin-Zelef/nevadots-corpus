---
id: collect-261121-veille-ia/regulation-us/newsom-proposes-mandatory-frontier-ai-oversight-and-shutdown-powers
title: "Newsom Proposes Mandatory Frontier AI Oversight and Shutdown Powers"
domain: regulation-us
role: reference
task: article
actors: []
dates: ["2026-11-18"]
keywords: ["would", "shutdown", "oversight", "frontier", "independent", "state", "could", "safety"]
source: https://www.unite.ai/newsom-proposes-mandatory-frontier-ai-oversight-and-shutdown-powers/
source_anchor: ""
source_lines: [1, 41]
section: "Regulation_US"
---
## Key PointsLicensed independent verifiers would give the state continuous technical scrutiny…

View more

- Licensed independent verifiers would give the state continuous technical scrutiny rather than one-time safety filings.
- Emergency shutdown orders would take effect immediately and stay in force during judicial review.
- Incident reporting would shift from training-scale thresholds to demonstrated capability for catastrophic risk.

California Governor Gavin Newsom released legislative recommendations on November 18, 2026, that would require continuous independent oversight of large frontier AI developers and let state officials order emergency shutdowns of models posing imminent catastrophic risks.

The California Government Operations Agency’s report, *AI Safety in 2026 and Beyond*, dated November 16, 2026, is intended to inform legislation when lawmakers return in December 2026; its recommendations are not yet enacted obligations. The plan would build on California’s requirement that large frontier developers publish safety frameworks by adding state-set baselines for safety, security and transparency throughout training, internal use and deployment.

## Continuous Independent Oversight

Every large frontier developer would be subject to ongoing oversight by licensed independent verification organizations, or IVOs. Those organizations would translate the state’s standards into technical tests and update them as AI evaluation methods change. They would also receive access to models, personnel, systems, training environments and records they judge necessary, then send unredacted assessments to the state.

The Government Operations Agency would license IVOs and could revoke licenses for insufficient expertise, independence or rigor. If an assessor could not attest that a developer met the standards, the state could order corrective action after engaging with the company. Developers could challenge such determinations in court before they take effect, apart from the expedited emergency process.

A surcharge on the verification fees developers pay would help finance publicly operated CalCompute capacity. Some of that computing capacity would be reserved for independent safety and security research and public oversight, linking an industry-funded surcharge to the infrastructure needed for external scrutiny.

## Risk Containment and Emergency Powers

Each large frontier developer would maintain a Risk Containment Protocol, tested and verified on an ongoing basis by an IVO and certified as effective by a senior company official. The required capability would span compute, agents, internal access and model deployment rather than depend on a single on-off switch. Protocols would identify responsible personnel, coordinate with cloud providers and power suppliers, and establish when a model can safely be restored.

Testing would address ways shutdowns can fail, including a model copying its weights to obstruct containment or an outside actor gaining access to shutdown controls. For open-weight developers, the containment and shutdown requirements would apply only to deployments under their control because they cannot shut down copies hosted by others.

State officials could order an immediate shutdown when they have credible reason to believe there is an imminent catastrophic risk or major loss-of-control event and the developer refuses to contain the danger. The order would require the narrowest workable intervention, take effect immediately and remain in force during judicial review, although the developer could seek a temporary restraining order. It would be lifted when the danger is no longer imminent.

## Incident Reporting and Automated AI Research

The reporting regime would cover attempted model-weight theft even without death or bodily injury, all internal or external loss-of-control incidents, and a new near-miss channel. The report proposes cutting the deadline for most critical incident reports from 15 days to 48 hours while retaining the 24-hour requirement for imminent catastrophic risks. Whistleblower protections would extend to former employees and contractors.

Models capable of actions that could create catastrophic risk would be treated as frontier models for incident reporting even when trained with fewer than 10^26 integer or floating-point operations. That capability-based trigger addresses the report’s concern that relying on training scale alone has left dangerous incidents outside state reporting requirements.

Companies automating AI research and development would report quarterly how their compute is divided among training, research experiments, inference for AI-research agents, safety and security work, external customers and unattributed activity, with IVOs verifying the categories. Additional disclosures would cover training-data volume, cost, subject domains and human-versus-AI provenance, as well as inference compute used to train models in reinforcement-learning environments, known rewarded misbehavior and the mitigations applied. The report says these measures would show whether AI is taking over more of its own development and whether safety investment is keeping pace.

Recursive self-improvement (RSI), whose strongest form involves an AI system independently developing its successor without human input, would be subject to standby cap authority rather than an immediate limit. Officials could impose a time-limited, periodically reviewed cap on automated AI research when it exceeds a developer’s committed thresholds or advances faster than its safeguards and evaluations, creating a material risk of catastrophic harm. The cap would be lifted after an IVO verified that adequate safeguards were in place.

The recommendations answer Newsom’s September 18, 2026 executive order on independent oversight and an AI kill switch, which requested expert proposals within two months and accelerated implementation of California’s independent-verification and AI-auditor laws. The Government Operations Agency developed the report with the California Office of Emergency Services and outside advisers Jason Goldman, Gillian Hadfield, Alondra Nelson and Rob Reich.

For developers, compliance would extend beyond publishing safety policies to continuous external scrutiny, faster incident reporting and demonstrable shutdown capabilities. The proposal leaves lawmakers with two connected decisions: how much intervention authority to enact and how to make continuous oversight operational while the licensed verification sector is still developing.
