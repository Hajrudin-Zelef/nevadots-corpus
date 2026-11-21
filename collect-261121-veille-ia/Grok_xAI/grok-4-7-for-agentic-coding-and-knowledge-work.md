---
id: collect-261121-veille-ia/grok-xai/grok-4-7-for-agentic-coding-and-knowledge-work
title: "Grok 4.7 for Agentic Coding and Knowledge Work"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "work", "across", "knowledge", "coding", "engineering", "longer", "high"]
source: https://cursor.com/grok
source_anchor: ""
source_lines: [1, 43]
section: "Grok_xAI"
---
### Présentation de Grok 4.7

Notre modèle le plus performant pour le codage et le travail intellectuel de longue durée.

La fiche officielle du modèle Grok 4.7. Elle présente les benchmarks de fonctionnalités du modèle ainsi que l'évaluation de ses garde-fous.

Cursor s’associe à SpaceX pour accélérer l’entraînement de ses modèles.

Grok 4.7 stays with difficult tasks across many steps. It uses tools, checks its own work, adjusts its approach, and keeps moving toward a finished result.

Grok 4.7 works across large codebases and extended engineering projects. It researches unfamiliar systems, edits across files, runs tests, and verifies the result.

Beyond code, Grok 4.7 works across documents, spreadsheets, presentations, and other professional artifacts. Give it a multi-step project that requires research, analysis, and synthesis.

Grok 4.7 verifies results more carefully than Grok 4.6 and manages longer context, with a 256k standard window and 500k long context.

Grok 4.7 is built for long-running coding and knowledge work. It uses a larger base than Grok 4.6 and a longer training run on harder, multi-hour tasks. It handles projects that span research, analysis, implementation, and several rounds of refinement.

On CursorBench 4.1 it scores 48.6% at extra high effort. It is served at the same price and speed as Grok 4.6, and Cursor subscription plans for individuals and teams include significant usage of the model.

We trained Grok 4.7 jointly with SpaceXAI on a new, larger base model than Grok 4.6. The reinforcement learning run was longer and weighted toward problems that take many hours to complete.

The model is better at verifying its own work and managing longer context. It was also trained to understand the Grok Bot harness, which improves conversational tasks and general knowledge work.

|  | Grok 4.7 xHigh | Grok 4.6 High | GPT-5.6 Sol Max | Fable 5.1 Max | 
|---|---|---|---|---|
| CursorBench 4.0Software engineering | 46.3% | 40.4% | 41.7% | 51.8% | 
| DeepSWE v1.1Software engineering | 71.0% high effort | 65.2% | 72.7% | 70.0% | 
| EEBenchElectrical engineering | 64.0% | 53.0% | 39.4% | 56.4% | 
| AA Briefcase v1.1Multi-hour office work | 1,657 | 1,546 | 1,487 | 1,678 | 
| Terminal-Bench 4.0Multi-hour terminal work | 37.6% | 20.3% | 37.3% | 57.9% | 
| Harvey Legal Agent BenchmarkLegal work | 19.6% | 15.8% | 2.5% | 6.7% | 
| HealthBench ProfessionalClinical reasoning | 56.7% | 48.5% | 60.5% | 62.1% | 

Scores issus de l’annonce de Grok 4.7. Le résultat de Grok 4.7 sur DeepSWE a été obtenu avec un niveau d’effort élevé.

Sur GDPval, Grok 4.7 atteint une cote Elo de 1 695 avec un niveau d’effort très élevé, contre 1 605 pour Grok 4.6, 1 735 pour Fable 5.1 et 1 542 pour GPT-6 Astra.

(Above) Grok 4.7 results across agentic coding and knowledge work benchmarks.

Grok 4.7 is available today in Cursor across desktop, web, iOS, CLI, and our SDK.

(Above) Grok 4.7 scores 48.6% at extra high effort on CursorBench 4.1.
