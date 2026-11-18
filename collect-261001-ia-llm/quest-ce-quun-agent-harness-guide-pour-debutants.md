Cours

L’idée n’est pas nouvelle. Les développeurs créent depuis des années des wrappers, des échafaudages et des environnements d’exécution autour des modèles. L’expression s’est imposée après que Mitchell Hashimoto, cofondateur de HashiCorp, a parlé de « harness engineering » dans un billet de blog de février 2026 sur son workflow IA. Son point était simple : lorsqu’un agent commet une erreur, modifiez l’environnement pour que l’erreur ne puisse plus se reproduire. OpenAI a repris le terme la même semaine pour ses travaux sur Codex, et LangChain a suivi avec le même cadrage.

Dans cet article, j’explique ce qu’est un agent harness, pourquoi les agents d’IA en ont besoin, en quoi il diffère des frameworks et des runtimes, et quels outils les développeurs utilisent pour bâtir des systèmes de type harness.

## Qu’est-ce qu’un agent harness ?

Une définition vient de LangChain : « Si vous n’êtes pas le modèle, vous êtes le harness. » En pratique, un agent harness est le logiciel qui entoure un modèle de langage : outils, mémoire, état, exécution, garde-fous et observabilité.

Agent = Modèle + Harness

Le modèle raisonne. Le harness lui fournit un environnement pour agir, se souvenir, vérifier les résultats et suivre des règles.

*Modèle au sein de son agent harness opérationnel. Image par l’auteur.*

La formule est utile, mais reste un modèle mental, pas une norme industrielle. Certains éditeurs utilisent encore « harness », « framework » et « scaffold » pour désigner à peu près la même chose.

## Pourquoi les agents d’IA ont besoin d’un harness

Un modèle de langage brut atteint vite ses limites quand vous lui demandez de travailler sur de nombreuses étapes. Il ne maintient pas un état durable tout seul, n’exécute pas de lui-même des outils, ne gère pas un contexte qui s’allonge, et ne se remet pas d’un échec d’appel d’outil sans aide.

Imaginez un agent chargé de corriger un test défaillant dans un projet Python. Sans harness, le modèle peut proposer une correction en apparence, mais il ne peut ni lire le vrai fichier de test, ni lancer pytest, ni voir l’erreur réelle, ni éditer la fonction fautive, ni confirmer que la correction passe. Avec un harness, toute cette boucle devient quelques minutes de travail que l’agent réalise seul, chaque étape étant consignée pour inspection humaine.

La recommandation d’Anthropic reste valable : commencez par l’approche la plus simple possible et n’ajoutez des pièces mobiles que lorsque la tâche l’exige.

## De quoi se compose un agent harness

Les composants varient, mais la plupart partagent quelques briques communes. Voyez-les comme une check-list, pas comme un cahier des charges strict. Un petit agent n’aura besoin que d’une partie de ces éléments, tandis qu’un agent en production en nécessitera davantage.

### Prompts système et règles de comportement

Le harness contrôle généralement les instructions de base du modèle. Cela inclut le prompt système, mais aussi les règles projet, les standards de code, les contraintes de rôle et les politiques de sécurité. Dans Deep Agents de LangChain, par exemple, un fichier `AGENTS.md` peut poser le cadre avant le démarrage d’une tâche.

Certains harnesses en 2026 utilisent aussi une divulgation progressive des instructions. Plutôt que de charger au démarrage la description complète de chaque outil, le harness n’ajoute qu’un résumé de ce qui est disponible. La notice détaillée d’un outil n’est chargée que lorsque le modèle en a besoin.

### Outils : comment les agents interagissent avec le monde

Les outils permettent à l’agent d’aller au-delà de la simple génération de texte. Exemples courants : recherche web, lecture/écriture de fichiers, requêtes de base de données, appels d’API, actions navigateur, exécution de code et commandes terminal. Le harness contrôle quels outils sont disponibles, quand le modèle est autorisé à les appeler, et comment les résultats sont formatés et réinjectés dans le contexte de l’agent.

Model Context Protocol (MCP) est devenu en 2026 l’interface standard pour cela. De nombreux harnesses, dont Anthropic Agent SDK, LangChain Deep Agents et OpenAI Agents SDK, utilisent MCP pour connecter des serveurs d’outils externes sans écrire d’intégration spécifique pour chacun.

### Mémoire et état

Les agents doivent savoir ce qui s’est passé plus tôt dans une tâche. Un harness peut conserver l’état de court terme dans la conversation active et l’état de long terme dans des fichiers, journaux, résumés ou préférences enregistrées. Certains compressent aussi de longues histoires en résumés pour éviter de surcharger le contexte.

### Environnement d’exécution : où l’agent s’exécute et agit

Beaucoup d’agents utiles ont besoin d’un véritable lieu de travail : un système de fichiers, un conteneur, un terminal isolé, une instance de navigateur ou un runtime cloud. Sans environnement d’exécution géré par le harness, les appels d’outils n’ont nulle part où atterrir.

Nombre de harnesses utilisent désormais des conteneurs bac à sable isolés : des environnements éphémères limités à une session, nettoyés en fin de tâche, afin d’éviter que les écritures de fichiers, installations de paquets et appels réseau d’une tâche ne débordent sur une autre.

### Orchestration et planification

Certaines tâches ne se prêtent pas à une suite d’étapes linéaires. Le harness peut fournir un outil de planification qui décompose un objectif en sous-tâches et en suit l’avancement. Il peut aussi lancer des sous-agents dédiés à une partie du travail et ne renvoyer au principal qu’un résumé.

LangChain Deep Agents, par exemple, suit les étapes du plan dans un fichier du système de fichiers, en les passant de « en attente » à « terminé » au fil de l’exécution.

### Garde-fous et autorisations

Le harness est l’endroit où poser les règles : approbation humaine, blocage d’appels d’outils, permissions basées sur les rôles et contrôles de sortie. OpenAI Agents SDK, LangChain Deep Agents et Microsoft Agent Framework prennent en charge ce type de contrôle. Le schéma le plus sûr consiste à vérifier séparément les entrées, les sorties et les permissions d’outils.

### Observabilité et traçage

Quand une tâche de cinquante étapes échoue à la trente-septième, un trace révèle ce qui s’est passé. Le traçage enregistre appels modèle, appels d’outils, passages de relais, erreurs, latence et coût sur l’ensemble du run. L’OpenAI Agents SDK active le traçage par défaut. LangSmith ajoute des tableaux de bord de débogage et d’évaluation. OpenTelemetry est devenu la norme d’export de traces au format neutre, pour éviter l’enfermement dans un outil d’observabilité.

## Agent harness vs framework vs runtime : quelle différence ?

La question revient souvent, et la réponse est plus subtile que ne le laissent entendre certains articles. La taxonomie est utile, mais elle n’est pas figée.

*Trois couches, abstraction croissante de bas en haut. Image par l’auteur.*

Commençons par le framework, que beaucoup de développeurs ont déjà utilisé.

### Qu’est-ce qu’un framework d’agent ?

Un framework d’agent offre aux développeurs des briques pour créer des agents. Il couvre les appels modèle, la définition d’outils, les schémas de mémoire et la boucle d’agent. Exemples : les premières versions de LangChain, CrewAI et Google ADK. Un framework vous indique comment structurer un agent, mais pas toujours comment l’exécuter de façon fiable en production.

### Qu’est-ce qu’un runtime d’agent ?

Un runtime d’agent est la couche qui aide un agent à s’exécuter de manière fiable dans le temps. Il gère l’exécution durable, la persistance de l’état, les relances, l’humain dans la boucle et le streaming. LangGraph, Temporal et Inngest en sont des exemples. Harrison Chase propose cette analogie : si Node.js est le runtime et Express le framework, un harness ressemble à Next.js.

### Qu’est-ce qui distingue un harness ?

Un harness opère à plus haut niveau qu’un framework. Là où un framework fournit des composants, un harness arrive généralement avec davantage de choix déjà posés : outils, planification, accès au système de fichiers et gestion du contexte.

## Cas d’usage d’un agent harness : code, recherche, data et entreprise

On retrouve les mêmes briques sur des métiers très différents, mais c’est la combinaison qui change. Un agent de développement et un agent de workflow en entreprise ont tous deux besoin d’un harness, mais n’en sollicitent pas les mêmes parties. Ces catégories ne sont pas des standards formels, ce sont des manières pratiques de voir comment une même idée s’adapte au travail à réaliser.

### Harness pour agents de développement

Les agents de développement sont un bon exemple actuel, car le harness y est très visible. Pour produire un travail utile, un agent a besoin d’accès fichiers, de contexte git, d’exécution terminal, de lancement de tests, d’installation de dépendances et de règles projet. Claude Code et Codex illustrent ce schéma : ils reposent fortement sur du code de harness, pas sur un simple appel d’API à un modèle.

La différence entre un bon et un moyen harness de développement se voit dans les détails : comment il se remet d’un test échoué, s’il peut annuler une mauvaise modification, ou la propreté avec laquelle il expose l’historique git au modèle. C’est là que se concentre l’essentiel de l’effort d’ingénierie.

### Harness pour agents de recherche

Les agents de recherche ont besoin d’un autre arsenal : recherche web, suivi des sources, prise de notes, gestion des citations et synthèse. Le harness gère la façon de stocker les résultats, d’attribuer les sources, et de découper de longs documents pour éviter de saturer la fenêtre de contexte d’un seul coup.

### Harness pour agents d’analyse de données

Les agents data ont besoin d’accès à des jeux de données, à des bases SQL, à des environnements Python et au contexte de schéma pour connaître tables et colonnes disponibles avant d’écrire des requêtes. Le harness applique aussi des frontières de permission, essentielles quand l’agent peut toucher des données de production.

### Harness pour workflows d’entreprise

Les déploiements en entreprise ajoutent une couche d’exigences : authentification, journaux d’audit, circuits d’approbation, contrôle d’accès par rôle et connexions aux systèmes internes. AWS AgentCore en est un exemple managé, avec identité, réseau VPC et observabilité intégrés. Microsoft Agent Framework couvre des besoins similaires pour les équipes sur Azure ou dans des environnements .NET.

## Outils pour construire des systèmes d’agent harness en 2026

Une poignée de produits reviennent le plus souvent à mi-2026. Ils se situent à différents niveaux du spectre framework–runtime–harness, et les frontières évoluent encore.

### LangChain Deep Agents

LangChain Deep Agents est le harness open source de LangChain, construit sur LangGraph comme runtime. Il inclut un outil de planification, un système de fichiers virtuel, le lancement de sous-agents, la compression automatique du contexte et un middleware pour l’approbation humaine et la détection de données personnelles. Agnostique au modèle, il prend en charge les endpoints compatibles OpenAI et se connecte à des bacs à sable comme Modal, Runloop et Daytona pour l’exécution de code.

### Anthropic Agent SDK

L’Anthropic Agent SDK (nom du package : `claude-agent-sdk`) a été extrait de Claude Code et publié en option autonome. Il inclut une boucle d’agent intégrée, des outils pour l’exécution bash, la lecture/écriture de fichiers, la recherche web, l’intégration MCP et la compaction du contexte. Il fonctionne uniquement avec les modèles Claude, via l’API d’Anthropic, Amazon Bedrock, Vertex AI et Azure.

### OpenAI Agents SDK

Comme mentionné plus haut, OpenAI Agents SDK a franchi le cap du framework vers le harness à mesure que ses fonctionnalités ont grandi. La version d’avril 2026 a ajouté l’exécution en sandbox native, la compaction de mémoire et des outils système de fichiers. Disponible en Python et TypeScript, le SDK gère l’usage d’outils, les handoffs entre agents et les garde-fous.

### Google Agent Development Kit

Google ADK prend en charge l’orchestration multi-agents avec des classes intégrées pour des structures séquentielles, parallèles et en boucle. Il inclut des outils d’évaluation, fonctionne avec Vertex AI pour les déploiements managés et supporte MCP pour la connexion aux outils. Disponible en Python, Java, TypeScript et Go, il est optimisé pour les modèles Gemini mais se veut agnostique au modèle.

### Microsoft Agent Framework

Microsoft Agent Framework est la voie de migration actuelle de Microsoft pour les projets AutoGen. Il prend en charge Python et .NET, fonctionne avec Azure AI et inclut la prise en charge MCP pour la connectivité des outils.

### CrewAI

CrewAI adopte une approche basée sur les rôles pour les systèmes multi-agents. Vous définissez des agents avec des rôles spécifiques, assignez des tâches, configurez des équipes et déclarez mémoire et garde-fous. Il convient aux problèmes qui se mappent naturellement à une équipe de spécialistes.

### Temporal et Inngest

Ce ne sont pas des agent harness à eux seuls. Ce sont des plateformes d’exécution durable qui gèrent ce qui se passe lorsqu’une tâche d’agent doit tourner des heures ou des jours sans perdre l’état. En cas d’échec, le moteur rejoue depuis le dernier point de contrôle réussi plutôt que de tout recommencer.

## Défis et arbitrages avec un agent harness

Ajouter un harness élargit les capacités du système, mais chaque outil, permission et agent supplémentaire ouvre une nouvelle voie de défaillance. À mesure que les tâches s’allongent, les garde-fous, le traçage et l’état durable cessent d’être optionnels et deviennent ce qui rend un long run récupérable.

Il existe aussi un risque de couplage qui surprend les équipes. LangChain a rapporté une hausse de 10 à 20 points sur un sous-ensemble de tau2-bench après l’ajout de profils de harness spécifiques au modèle. Artificial Analysis va dans le même sens dans son Coding Agent Index : les résultats des agents de développement dépendent du modèle et du harness ensemble, avec des variations marquées de coût, de jetons et de temps par tâche selon les combinaisons. Le modèle n’a pas changé. Les prompts, outils et middleware autour, si. Et ce profilage relève du travail de harness.

## Avez-vous vraiment besoin d’un agent harness ?

Voici une façon directe d’évaluer si vous en avez besoin.

Vous avez probablement besoin d’un harness si votre système remplit une ou plusieurs de ces conditions :

- Il doit utiliser des outils externes
- Il doit se souvenir des avancées entre les sessions
- Il doit exécuter du code dans un environnement réel
- Il coordonne plus d’un agent
- Il doit se remettre de défaillances partielles sans perdre le travail
- Il requiert une approbation humaine

Vous n’avez probablement pas besoin d’un harness si la tâche est un workflow prévisible où chaque étape est prédéfinie.

Un test utile : si la tâche peut être prise en charge par un seul appel au modèle, ou par un petit script déterministe avec quelques conditions, un harness est sans doute excessif. Dès que la tâche exige que l’agent prenne des décisions, utilise des outils et réagisse aux résultats dans le temps, le harness commence à faire un vrai travail.

Un travers courant que j’observe : des équipes adoptent un harness trop tôt, bâtissant traçage et sandbox pour ce qui n’est en réalité qu’une génération de texte one-shot. L’erreur inverse est plus douloureuse : brancher directement le modèle puis découvrir, au deuxième test raté, au troisième appel d’outil ou au cinquième redémarrage, qu’il n’existe aucune infrastructure de repli.

## Dernières réflexions

Comme indiqué plus haut, les éditeurs n’emploient pas tous les mêmes mots, et la frontière entre framework, runtime et agent harness continue d’évoluer.

Pour une génération one-shot, le wrapper est superflu. Pour des agents qui doivent agir, mémoriser et se rétablir sur de longues sessions, l’agent harness devient une pièce maîtresse du système. Le choix du bon harness est de plus en plus une décision distincte de celle du modèle. Je suis curieux de voir quelle part de cette couche sera absorbée par la prochaine génération de modèles, car certaines annonces d’OpenAI et d’Anthropic laissent penser que la frontière va continuer de bouger. L’idée de base reste valable : un agent, c’est un modèle plus un agent harness.

Pour aller plus loin sur la construction de systèmes à agents, notre cours Building Scalable Agentic Systems couvre les schémas d’usage d’outils, d’orchestration et de workflows d’agents longue durée.

Je suis ingénieur de données et créateur de communautés. Je travaille sur les pipelines de données, le cloud et les outils d'IA, tout en rédigeant des tutoriels pratiques et percutants pour DataCamp et les développeurs émergents.

## FAQ sur les agents harness

### Quelle est la différence entre un agent harness et un prompt système ?

**Un prompt système est une instruction que l’agent lit au départ. L’agent harness est la couche plus large qui gère les outils, l’état, les permissions et la gestion des échecs. Le cadrage le plus simple : le prompt système dit au modèle quoi faire, l’agent harness contrôle ce qu’il peut faire. Vous pouvez avoir un prompt système soigné sans agent harness : vous restez dans un appel d’API sans état. L’agent harness est ce qui transforme un prompt en système.**

### Puis-je construire mon propre agent harness from scratch ?

**En principe, oui. Dans sa forme la plus simple, un harness est une boucle : appeler le modèle, analyser la réponse, exécuter les appels d’outils éventuels, renvoyer les résultats, recommencer. Cette boucle s’écrit en quelques dizaines de lignes de Python en une après-midi. La difficulté arrive après la boucle : débordement de contexte, appels d’outils échoués, perte d’état au redémarrage, application des permissions et traçage. En pratique, ce travail post-boucle prend toujours plus de temps que prévu, ce qui explique pourquoi les harness open source grossissent plutôt qu’ils ne s’allègent.**

### Le modèle sait-il qu’il est à l’intérieur d’un harness ?

**Pas explicitement. Certains harnesses informent le modèle, via le prompt système, des outils disponibles, mais le modèle n’a pas la notion du harness comme système autour de lui. Il ne voit que le contexte fourni, génère une réponse et produit parfois un appel d’outil. Conséquence : quand quelque chose casse, le modèle ne peut souvent pas expliquer pourquoi, car il ignore l’existence du harness. Le débogage d’un agent consiste donc surtout à déboguer le harness, pas le modèle.**

### Comment le choix du modèle influence-t-il le harness à utiliser ?

**Plus qu’on ne le pense. Les modèles de pointe pour le code sont parfois post-entraînés avec leur propre agent harness dans la boucle, si bien que remplacer ce harness peut dégrader les performances. Heuristique pratique : si votre équipe s’engage sur une famille de modèles, la short-list d’agent harness s’impose souvent d’elle-même. Le cas le plus dur est le changement de modèle plus tard : cela implique généralement de réécrire la logique du harness, pas seulement de modifier une valeur de configuration.**

### Est-ce différent de ce qu’on appelait autrefois « LLM scaffolding » ?

**Pas vraiment. C’est la même idée sous un nom plus récent. « LLM scaffolding », « agent wrapper » et « environnement d’exécution » vont tous dans la même direction. La nuance en 2026 : « scaffolding » évoque une structure temporaire à démonter une fois le modèle assez bon, tandis que « agent harness » suggère quelque chose que le modèle conserve autour de lui. Cela change la façon de budgéter : le scaffolding est retiré, les agent harness deviennent partie intégrante du système.**