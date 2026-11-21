---
id: collect-261121-veille-ia/claude-anthropic/mai-code-2-et-flash-benchmarks-prix-vs-opus-5-5
title: "MAI Code 2 et Flash : benchmarks, prix, vs Opus 5.5"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-17"]
keywords: ["code", "flash", "microsoft", "copilot", "tokens", "prix", "sortie", "github"]
source: https://crewmark.ai/blog/mai-code-2
source_anchor: ""
source_lines: [1, 143]
section: "Claude_Anthropic"
---
## MAI Code 2 et MAI Code 2 Flash en un coup d’œil

|  | MAI Code 2 | MAI Code 2 Flash | 
|---|---|---|
| Sortie | 17 novembre 2026 (Ignite), disponibilité générale dans GitHub Copilot | 17 novembre 2026 (Ignite), disponibilité générale dans GitHub Copilot | 
| Fenêtre de contexte | 1,050,000 tokens (Microsoft parle de 1M) | 1,050,000 tokens (Microsoft parle de 1M) | 
| Entrée max. / sortie max. | 922,000 / 128,000 tokens | 922,000 / 128,000 tokens | 
| Entrées | Texte et images | Texte et images | 
| Effort de raisonnement | `low` ,`medium` ,`high` ,`xhigh` ,`max` | `low` ,`medium` ,`high` ,`xhigh` ,`max` | 
| Date limite des connaissances | Août 2026 | Août 2026 | 
| Forfaits Copilot | Pro, Pro+, Max, Business, Enterprise | Free et Student (uniquement via la sélection automatique du modèle), Pro, Pro+, Max, Business, Enterprise | 
| Prix Copilot, promotionnel (jusqu’au 17 mai 2027) | $0.50 en entrée / $2.50 en sortie par million de tokens | $0.05 en entrée / $0.25 en sortie par million de tokens | 
| Prix Copilot, à partir du 17 mai 2027 | $1.00 en entrée / $5.00 en sortie par million de tokens | $0.10 en entrée / $0.50 en sortie par million de tokens | 
| Multiplicateur de requêtes premium (anciens forfaits annuels uniquement) | 1 | 0.33 | 
| En dehors de Copilot | Non annoncé | Préversion à accès limité dans Microsoft Foundry | 

## Ce qu’est MAI Code 2, et en quoi il diffère de MAI-Code-1.1-Flash

MAI Code 2 et MAI Code 2 Flash forment la « famille MAI Code 2 ». Selon la fiche de modèle de Microsoft, les deux partagent la même famille d’architecture, la même recette d’entraînement et un mélange de données similaire. La principale différence est la taille. Ce sont deux transformeurs décodeur seul avec des couches Mixture-of-Experts éparses et un encodeur de vision intégré, construits sur MAI-Base-2.

- MAI Code 2 est pour le travail complexe : débogage approfondi, modifications sur plusieurs fichiers, refactorisation, raisonnement à l’échelle du dépôt, interface et mise en production du code. Microsoft dit qu’il est « à la frontière du rapport prix-performance pour les tâches de code courantes en entreprise ».
- MAI Code 2 Flash est réglé pour une faible latence et un faible coût de service : explications de code, modifications ciblées, corrections courantes et itérations rapides. La fiche dit qu’il est moins capable sur les tâches complexes de longue haleine et peut demander plus de guidage.
- Les prédécesseurs n’existaient qu’en version Flash. MAI-Code-1-Flash est sorti le 2 juin 2026 à Build et a été retiré le 10 septembre 2026. MAI-Code-1.1-Flash a suivi le 11 août 2026, avec 5B de paramètres actifs d’après la fiche de MAI Code 2.
- GitHub retirera MAI-Code-1.1-Flash de toutes les expériences Copilot le 18 décembre 2026. Le remplaçant suggéré par GitHub est « Latest MAI Code Flash model ». MAI-Code-1.1-Flash figure toujours comme préversion dans le catalogue Foundry.
- Nombre de paramètres : la fiche de modèle donne deux séries de chiffres différentes pour chaque modèle, nous les laissons donc de côté.
- GitHub décrit les modèles MAI comme « en amélioration continue » : le comportement peut changer à mesure que de nouveaux checkpoints sortent.

## Benchmarks de MAI Code 2 (annonces de Microsoft)

Ce sont les chiffres de Microsoft, pas des tests indépendants. Microsoft les a obtenus sur sa plateforme interne MSBench avec GitHub Copilot CLI v1.0.102, sous-agents désactivés, effort de raisonnement max, en moyenne sur 3 exécutions (taux de réussite en une tentative).

Les comparaisons phares font la moyenne de quatre benchmarks : DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas et FrontierCode v1.1. Les coûts de MAI sont comptés au prix promotionnel de 50 %. Les scores et coûts de FrontierCode viennent d’Artificial Analysis. Microsoft dit que les résultats varient selon la charge de travail, la configuration, l’effort de raisonnement et les hypothèses de prix.

| Affirmation | MAI Code 2 | MAI Code 2 Flash | 
|---|---|---|
| vs GPT-6.1 Sol (moyenne sur 4 benchmarks) | 98 % de ses performances pour 40 % de son coût | - | 
| vs Claude Opus 5.5 (moyenne sur 4 benchmarks) | 96 % de ses performances pour 5 % de son coût | - | 
| vs GPT-6 Luna et Claude Haiku 5.5 (3 benchmarks) | - | « Performances comparables » ; coût d’environ 65 % de GPT-6 Luna et d’environ 20 % de Haiku 5.5 | 
| Préférence en aveugle sur le front-end vs GPT-6 Sol (258 évaluateurs) | Préféré dans 64 % des cas | - | 
| DeepSWE 1.1 (fiche de modèle, tableau principal) | 73.2% | 59.9% | 
| TerminalBench 2.1 (fiche de modèle) | 84.6% | 78.3% | 
| SWE Atlas (fiche de modèle, tableau principal) | 43.1% | 44.8% | 
| SWE-bench (Verified, Pro ou Multilingual) | Non publié | Non publié | 

## Ce que les chiffres des benchmarks ne disent pas

- La fiche de modèle imprime d’autres chiffres pour DeepSWE 1.1 (60.2% et 27.4%) et SWE Atlas (80.9% et 65.2%) dans sa section sur la sécurité, sans expliquer la différence de configuration. Le tableau ci-dessus reprend le tableau principal des benchmarks.
- Dans le tableau principal, Flash obtient un score légèrement supérieur à MAI Code 2 sur SWE Atlas.
- Dans les sources que nous avons consultées, Microsoft n’a publié aucun score FrontierCode v1.1 par modèle, ni de chiffres par benchmark pour Flash face à ses concurrents.
- Aucun score SWE-bench, et aucun chiffre de vitesse : ni tokens par seconde ni latence pour l’un ou l’autre modèle.
- Chaque éditeur exécute ses benchmarks à sa façon. Ne mettez pas ces chiffres en regard des tableaux d’un autre éditeur comme s’ils avaient été obtenus de la même manière.
- Microsoft s’est comparé à GPT-6.1 Sol. Pour les modèles plus récents d’OpenAI, voir GPT-6.2 Astra et Sol.

## Prix de MAI Code 2 dans GitHub Copilot

Copilot facture ces modèles au token, converti en crédits GitHub AI (1 crédit = $0.01). Les prix sont par million de tokens, d’après la documentation de GitHub. Les prix promotionnels courent jusqu’au 17 mai 2027. Choisir le contexte de 1M ou un niveau de raisonnement plus élevé consomme plus de crédits.

| Modèle | Entrée | Entrée en cache | Sortie | 
|---|---|---|---|
| MAI Code 2, promotionnel | $0.50 | $0.025 | $2.50 | 
| MAI Code 2, à partir du 17 mai 2027 | $1.00 | $0.05 | $5.00 | 
| MAI Code 2 Flash, promotionnel | $0.05 | $0.005 | $0.25 | 
| MAI Code 2 Flash, à partir du 17 mai 2027 | $0.10 | $0.01 | $0.50 | 

## MAI Code 2 vs Claude Opus 5.5

L’affirmation phare de Microsoft est que MAI Code 2 atteint 96 % des performances de Claude Opus 5.5 pour 5 % de son coût. Lisez-la avec les réserves que Microsoft donne.

- C’est une moyenne sur quatre benchmarks choisis par Microsoft, exécutés sur le propre harnais de Microsoft avec Copilot CLI en raisonnement max.
- Le chiffre de 5 % de coût utilise le prix promotionnel de MAI Code 2, qui double le 17 mai 2027.
- Nous n’avons pas trouvé de détail par benchmark pour Opus 5.5 dans les sources de Microsoft : les 96 % ne peuvent donc pas être vérifiés ligne par ligne.
- Où ils tournent : MAI Code 2 n’est que dans GitHub Copilot, sans API annoncée. Opus 5.5 est le modèle d’Anthropic, utilisé dans Claude Code et sur la Claude API, où il est affiché à $4 par million de tokens en entrée et $20 par million en sortie.
- Anthropic recommande Opus 5.5 comme point de départ pour la plupart des travaux et son nouveau Claude Fable 5.2 pour les jobs les plus difficiles.
- Microsoft dit que MAI Code 2 n’est pas encore réglé pour le travail agentique de très longue haleine. Si vos tâches durent des heures, testez sur votre propre dépôt avant de changer.

## Comment utiliser MAI Code 2 dans GitHub Copilot

1. ### Mettez votre client à jourVisual Studio demande la version 17.14.6 ou plus récente. GitHub indique « TBD » comme version minimale pour VS Code, JetBrains, Xcode et Eclipse : mettez donc à jour l’extension ou le plugin Copilot vers la dernière version.
2. ### Choisissez le modèleOuvrez Copilot Chat ou le mode agent dans VS Code, ou Copilot CLI, et choisissez MAI Code 2 ou MAI Code 2 Flash dans le sélecteur de modèle. Les modèles sont aussi dans l’agent cloud Copilot, l’application GitHub Copilot, github.com, GitHub Mobile, Visual Studio, JetBrains, Xcode et Eclipse.
3. ### Activez le contexte de 1M ou réglez le raisonnement, si besoinLe contexte de 1M tokens n’est disponible que dans VS Code et Copilot CLI. Le raisonnement configurable fonctionne dans VS Code, Copilot CLI et l’agent cloud Copilot. Les deux consomment plus de crédits. GitHub n’indique pas la taille de contexte par défaut des modèles MAI.
4. ### Sur Business ou Enterprise, vérifiez la politique des modèlesSi « Default availability for released models » est activé, les modèles sont activés sauf si un administrateur les a désactivés. Pour les activer vous-même en tant que propriétaire d’organisation : photo de profil → Organizations → votre organisation → Settings → Code, planning, and automation → Copilot → Models, puis réglez MAI Code 2 et MAI Code 2 Flash sur Enabled. Une icône de bouclier signifie que le propriétaire de votre entreprise contrôle ce réglage.
5. ### Sur Free ou Student, utilisez AutoCes forfaits n’obtiennent MAI Code 2 Flash que via la sélection automatique du modèle. Vous ne pouvez pas le choisir par son nom, et MAI Code 2 n’est pas proposé.

## Peut-on utiliser MAI Code 2 en dehors de Copilot ?

- MAI Code 2 : pas encore. Microsoft ne cite que GitHub Copilot comme canal et n’a annoncé ni API ni offre Foundry.
- MAI Code 2 Flash : en préversion à accès limité dans Microsoft Foundry. Vous créez un déploiement et l’appelez via la Responses API compatible OpenAI avec une authentification Azure. Dans les sources que nous avons consultées, Microsoft n’a publié ni la façon de demander l’accès, ni un prix Foundry, ni un ID de modèle.
- Microsoft dit qu’il sortira « bientôt » sur Open Router, Vercel, Hermes, Open Code et Kilo. Il n’y a pas de date, et la phrase peut ne concerner que Flash.
- Les poids ne sont pas publiés. Les modèles ne tournent que sur Azure.
- Rien d’officiel ne dit qu’on peut utiliser l’un ou l’autre modèle dans Codex, Claude Code ou Cursor.

## Les limites que Microsoft indique

- Pas encore réglés pour la recherche scientifique fondamentale ni pour le travail agentique de très longue haleine.
- Non destinés au chat grand public, ni aux conseils médicaux, juridiques ou financiers.
- Dans les longues sessions, les modèles peuvent répéter des appels d’outils, dériver ou s’arrêter avant de vérifier leur travail. La qualité peut baisser près du contexte maximal.
- Optimisés pour la production de code en anglais. Les autres langues n’ont pas été évaluées en profondeur.
- Microsoft juge leur alignement similaire à GPT-5.6 Sol et inférieur à GPT-6.
- Selon le cadre de sécurité de Microsoft, les risques CBRN et de cyberattaque offensive sont classés High, avec des classificateurs, de la journalisation et une surveillance des abus comme mesures d’atténuation.

## Vous voulez un agent qui traite un backlog selon un planning ?

MAI Code 2 reste pour l’instant dans GitHub Copilot. Crewmark transforme des sessions d’agents de code en employés IA, chacun avec un rôle, un objectif, des horaires de travail et une pulsation, qui prennent des issues sur un tableau. Les employés Claude Code et Codex tournent en natif ; d’autres agents, comme OpenCode et DeepSeek Harness, se connectent via ACP (acpx), et le CLI de GitHub Copilot fait partie des agents que acpx peut piloter. Les utilisateurs de Copilot peuvent aussi confier du travail à l’agent Copilot depuis le chat : voir GitHub Copilot dans Slack.

1. ### Choisissez un rôlePar exemple l’ingénieur logiciel IA pour un backlog de bugs et de petites fonctionnalités.
2. ### Choisissez l’agentFaites tourner l’employé sur Claude Code, Codex ou un autre agent ; voir Claude Code vs Codex si vous hésitez.
3. ### Laissez-le travaillerIl se réveille au rythme de sa pulsation et prend l’issue suivante sur le tableau ; voir comment faire tourner Claude Code 24 h/24, 7 j/7.

## FAQ

## Qu’est-ce que MAI Code 2 ?

Un modèle de code de Microsoft AI, annoncé à Ignite le 17 novembre 2026 et entraîné de zéro par Microsoft. Il est conçu pour le travail complexe : débogage approfondi, modifications sur plusieurs fichiers et raisonnement à l’échelle du dépôt. Il est disponible dans GitHub Copilot sur Pro, Pro+, Max, Business et Enterprise.

## Qu’est-ce que MAI Code 2 Flash ?

Le modèle plus petit et moins cher de la même famille, réglé pour une faible latence : explications, modifications ciblées et corrections courantes. Dans Copilot, il coûte $0.05 en entrée et $0.25 en sortie par million de tokens jusqu’au 17 mai 2027. Il est aussi en préversion à accès limité dans Microsoft Foundry.

## Quels sont les benchmarks de MAI Code 2 ?

Les chiffres de Microsoft : 98 % des performances de GPT-6.1 Sol pour 40 % de son coût, et 96 % de celles de Claude Opus 5.5 pour 5 % de son coût, en moyenne sur DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas et FrontierCode v1.1 au prix promotionnel. Le tableau principal de la fiche de modèle donne 73.2% sur DeepSWE 1.1 et 84.6% sur TerminalBench 2.1. Microsoft n’a publié aucun score SWE-bench.

## MAI Code 2 est-il meilleur que Claude Opus 5.5 ?

Microsoft ne l’affirme pas. Il revendique 96 % des performances d’Opus 5.5 pour 5 % du coût, sur quatre benchmarks qu’il a choisis, au prix promotionnel. Opus 5.5 tourne dans Claude Code et sur l’API d’Anthropic ; MAI Code 2 ne tourne pour l’instant que dans GitHub Copilot.

## Que devient MAI-Code-1.1-Flash ?

GitHub le retirera de toutes les expériences Copilot le 18 décembre 2026 et suggère à la place le dernier modèle MAI Code Flash. Il est sorti le 11 août 2026 ; le MAI-Code-1-Flash précédent a été retiré le 10 septembre 2026.

## Puis-je utiliser MAI Code 2 via une API ou dans d’autres agents de code ?

Pas MAI Code 2 : aucune API n’a été annoncée. MAI Code 2 Flash est en préversion limitée dans Foundry via la Responses API. Microsoft dit qu’il arrivera « bientôt » sur Open Router, Vercel, Hermes, Open Code et Kilo, sans date.

## Quelle est la fenêtre de contexte de MAI Code 2 ?

1,050,000 tokens d’après la fiche de modèle, avec jusqu’à 922,000 tokens en entrée et 128,000 en sortie. Dans Copilot, la fenêtre de 1M n’est disponible que dans VS Code et Copilot CLI.

## Que disent les gens sur Reddit de MAI Code 2 ?

Cette page s’en tient à ce que Microsoft et GitHub ont publié, pas aux fils de forum. Microsoft n’a publié ni chiffres de vitesse ni score SWE-bench : tester le modèle sur votre propre dépôt dans Copilot reste la vérification la plus fiable.

Vérifié d’après l’annonce de Microsoft AI, la fiche de modèle de MAI Code 2, le journal des modifications de GitHub et la documentation de GitHub Copilot le 18 novembre 2026.
