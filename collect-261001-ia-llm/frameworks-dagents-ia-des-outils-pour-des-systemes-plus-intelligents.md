Cours

Imaginez un collaborateur qui ne dort jamais, ne se plaint pas et s’améliore en continu. De la science-fiction ? C’est précisément ce que les agents IA apportent. Ces cerveaux numériques planifient des tâches, récupèrent des informations, dialoguent avec les utilisateurs, se connectent à des API — et peuvent même coopérer entre eux pour résoudre des problèmes d’envergure.

Bien sûr, vous pourriez en construire un de A à Z, mais la plupart des équipes s’appuient sur des frameworks d’agents IA : des boîtes à outils prééquipées qui gèrent la mémoire, l’orchestration — bref, l’essentiel. Dans cet article, nous expliquons ce que sont ces frameworks, comment ils fonctionnent et comment choisir celui qui convient à votre cas d’usage.

## Introduction aux agents d'intelligence artificielle

## Qu’est-ce qu’un framework d’agent IA ?

Au fond, les agents IA sont des programmes capables de percevoir, planifier et agir. Conçus pour analyser un objectif, le décomposer en étapes et passer à l’action — seuls ou à plusieurs.

Qu’il s’agisse de répondre à une question, lancer une recherche ou collaborer avec un autre agent, ils sont utilisés dans de nombreuses industries et opèrent avec un niveau d’autonomie surprenant.

Il existe plusieurs façons de construire un agent IA à partir de zéro. On peut les développer en Python, avec React ou d’autres stacks. Mais repartir de la feuille blanche, c’est une toute autre histoire : il faut gérer la mémoire, planifier les tâches, brancher les outils, coordonner, traiter les erreurs… et bien plus encore. Autant de pièces à assembler sans faux pas.

#### Frameworks d’agents IA = cerveau + boîte à outils

Ces frameworks facilitent grandement la création et la mise à l’échelle des agents IA.

*Composants d’un framework d’agent IA. Source : Napkin IA*

Ces frameworks apportent :

- Architecture : une structure d’interaction entre agents.
- Mémoire : rappel à court et à long terme.
- Modèles : les grands modèles de langage (LLMs) sont le cœur des agents, leur donnant compréhension, raisonnement et capacité d’action.
- Boîtes à outils : APIs, moteurs de recherche, interpréteurs — tout le nécessaire pour agir.
- Couche d’orchestration : coordination des tâches et de la collaboration (notamment en multi-agents).
- Intégrations : connexion à LangChain, OpenAI, Azure, Slack, etc.

En bref : ils vous font gagner du temps, vous épargnent des migraines et réduisent la complexité.

## Comment fonctionnent les agents IA en pratique

Pour comprendre un agent IA, partons de son cycle de décision le plus simple.

Tout commence par la planification des tâches. Face à une demande — « Résumez cet article » ou « Trouvez un vol à moins de 1 000 $ » — l’agent la décompose en étapes. Il se crée une to-do list pour décider quoi faire, dans quel ordre, et s’il peut tout gérer seul ou doit solliciter d’autres agents.

*Flux de travail d’un agent IA. Source : Napkin IA*

Vient ensuite le function calling, où l’agent choisit les outils ou APIs à mobiliser. Qu’il s’agisse de naviguer sur le web, consulter la météo ou interroger une base, il appelle la bonne fonction — un peu comme vous alternez entre applications pour boucler votre liste de tâches.

Puis vient la phase d’exécution, où l’agent réalise l’action : exécuter du code, récupérer des données, envoyer un e-mail ou rédiger une réponse. Il interagit avec ses outils et systèmes pour mener à bien la mission.

Enfin, la boucle de feedback. Qu’il ait réussi, partiellement réussi ou échoué, l’agent évalue le résultat, met à jour sa mémoire, ajuste ses prochaines étapes ou demande des précisions à l’utilisateur. Ce retour d’expérience l’affûtte au fil du temps.

Selon la complexité, l’agent parcourt ce cycle une fois ou itère jusqu’au bon résultat.

### Systèmes mono-agent et multi-agents

Les agents IA se classent selon leurs rôles, capacités et environnements. Une distinction clé tient au nombre d’agents impliqués.

Les systèmes mono-agent opèrent de manière indépendante pour un objectif précis. Ils s’appuient sur des outils et ressources externes pour agir efficacement dans divers environnements. Idéals pour des buts bien définis qui ne nécessitent pas de coordination. En général, un seul modèle fondationnel suffit.

Les systèmes multi-agents mobilisent plusieurs agents qui collaborent ou entrent en compétition pour des objectifs partagés ou individuels. Ils capitalisent sur des compétences et rôles variés, parfaits pour les problèmes complexes. Ils peuvent aussi simuler des comportements proches de l’humain (communication, interactions). Chaque agent peut reposer sur un modèle fondationnel différent, adapté à sa fonction.

### Fonctionnalités communes des frameworks d’agents IA

La plupart des frameworks partagent un socle de fonctionnalités : le minimum vital pour que les agents livrent des résultats.

*Fonctionnalités communes des frameworks d’agents IA. Source : auteur*

D’abord, la mémoire persistante, qui permet de conserver le contexte. Plutôt que de tout réinitialiser, l’agent capitalise sur l’historique, ce qui le rend plus pertinent, réactif et presque « humain ».

Ensuite, la génération augmentée par recherche (RAG). En clair, l’agent va chercher en temps réel la bonne information dans des sources externes : documents, bases, web. Il ne se limite plus à son entraînement et s’appuie sur un savoir à jour et spécifique au domaine.

Puis l’usage d’outils, qui fait toute la différence. Un agent n’est pas seulement là pour répondre : il agit. Appeler une API, faire des calculs, scraper un site, déclencher un job backend : c’est ce qui transforme un simple chatbot en assistant opérationnel.

Enfin, la collaboration entre agents. Les agents se partagent des tâches, se transmettent des mises à jour ou résolvent chacun une partie d’un problème global. Comme dans une entreprise : l’un fait la recherche, l’autre rédige le rapport. À plusieurs, ils traitent des missions qu’un seul agent ne pourrait pas mener.

## Frameworks d’agents IA populaires

L’offre évolue vite, et choisir le bon framework peut ressembler à un choix de garniture sur une pizza : il y en a pour tous les goûts, mais trop d’options, ça déroute.

Simplifions : voici les frameworks qui font le plus parler d’eux, comment ils fonctionnent, leurs atouts et leurs terrains de jeu idéaux.

*Principales options de frameworks d’agents IA : source : Napkin IA*

### 1. CrewAI

CrewAI, c’est comme monter votre équipe de super-héros : chaque agent a un rôle (« Researcher », « Writer », « Analyst ») et travaille en équipe.

L’approche orientée rôle assigne les tâches à chaque agent, et le système se charge de la coordination.

Ses atouts :

- Prise en main simple : il masque la complexité d’ingénierie pour vous laisser vous concentrer sur les résultats.
- Open source, compatible avec des LLMs majeurs (OpenAI, Anthropic) et RAG intégré.

Idéal pour les entreprises qui déploient des bots de service client ou automatisent des flux de recherche intensive.

Limite : ça peut sembler contraint si vous avez besoin d’agents très flexibles dans leurs rôles, adaptables à la volée.

### 2. LangGraph

Si vous voulez que votre agent stratégise et improvise ses étapes, LangGraph est un excellent choix. Basé sur la théorie des graphes, il permet boucles, branches et détours. Parfait pour des workflows non linéaires.

Utile par exemple dans l’hospitalité, où un assistant de voyage évalue plusieurs options ou vérifie le système avant de répondre. Dans l’assurance, un agent qui compare des offres personnalisées doit jongler avec de nombreuses variables : LangGraph s’y prête bien.

Intégré à LangChain, il donne accès à tout l’écosystème d’outils. Inconvénient : une courbe d’apprentissage marquée. Mais une fois maîtrisé, vous débloquez une logique très sophistiquée.

Explorez nos cours Designing Agentic Systems with LangChain et le nouveau Multi-Agent Systems with LangGraph pour apprendre à construire des systèmes multi-agents avec des patterns agentiques.

## Systèmes multi-agents avec LangGraph

### 3. AutoGen

AutoGen est la réponse de Microsoft à l’orchestration d’agents, et il ne vient pas les mains vides. Son architecture en couches comprend des fonctions Core, AgentChat pour la messagerie inter-agents, et des Extensions avancées.

Point fort : AutoGen Studio — un environnement low-code pour concevoir visuellement des agents. Particulièrement utile si vous travaillez avec Azure ou Teams.

Il gère aussi la messagerie asynchrone entre agents, pratique pour des workflows dynamiques. Seul bémol : il peut paraître trop « ingénieré » pour des projets simples.

### 4. Agno (par Phidata)

Agno suit la philosophie « less is more » : un framework minimaliste, syntaxe Python épurée, déploiement cloud intégré, idéal pour prototyper vite.

Envie d’un bot d’analyse de sentiment sur X/Twitter via AWS Lambda ? C’est un bon point de départ. Il s’intègre facilement aux LLMs et à des APIs comme DuckDuckGo ou Yahoo Finance : parfait pour des dashboards, agents ou outils internes en prototype.

Limite : communauté plus restreinte, donc moins de plug-ins et de guides en cas de blocage.

### 5. Atomic Agents

Atomic Agents s’adresse à ceux qui veulent un contrôle total, sans pilotage automatique. L’architecture, claire et modulaire, demande plus de mise en place au départ, mais offre ensuite une flexibilité maximale.

Pas de couche d’orchestration « magique » en coulisses : ce que vous construisez est ce que vous obtenez. Idéal pour des équipes focalisées sur la maintenabilité et la performance long terme (outils d’entreprise, R&D).

En revanche, pour un prototype à livrer vite, ce n’est pas le plus adapté.

### 6. OpenAI Agents SDK

OpenAI Agents SDK est l’évolution prêt-à-la-production de Swarm. Framework léger pour créer des workflows multi-agents, il est agnostique côté fournisseur et prend en charge les APIs OpenAI Responses et Chat Completions ainsi que 100+ autres LLMs.

Son idée clé : les « handoffs », où un agent termine une tâche et la transmet au suivant, comme un relais. Idéal pour des applis ou démos qui exigent confidentialité et rapidité, sans lourde infrastructure.

### 7. LlamaIndex

Lancé comme GPT Index, LlamaIndex aidait d’abord les LLMs à exploiter des données structurées — et s’est désormais aventuré dans le monde des agents IA.

Excellent pour extraire des insights à partir de documents, tableaux et bases, il gère désormais les transitions entre requêtes et des workflows simples. Autres cas d’usage : applications multimodales et agents autonomes capables de rechercher et d’agir.

Moins fort en orchestration que CrewAI ou LangGraph, il excelle dès que l’enjeu est l’interaction poussée avec la donnée plutôt que des comportements d’agents complexes.

### 8. Semantic Kernel

Encore un Microsoft : Semantic Kernel est le framework d’entreprise pour agents, compatible Python, C# et Java, conçu pour intégrer des LLMs dans des systèmes comme des ERP et CRM. Il automatise des processus en combinant prompts et APIs existantes pour exécuter des actions.

Il gère mémoire, planification et « skills » (outils) et passe à l’échelle dans le cloud. Si vous avez besoin d’une intégration stricte à l’existant, c’est probablement le meilleur choix. En solo ou petite équipe, il peut toutefois paraître un peu lourd pour démarrer vite.

### 9. Google ADK (Agent Development Kit)

Pour finir : vous pouvez créer des agents IA avec Google ADK. Si vous ne connaissez pas encore, Google ADK est un framework open source pour construire des agents modulaires et intelligents, avec un fort accent sur l’orchestration et l’usage d’outils. Il prend en charge les LLMs (Gemini ou autres), les agents personnalisés et des agents de workflow.

ADK gère le paradigme « agent-comme-outil » (où des agents peuvent en appeler d’autres) et propose des workflows avec évaluation. C’est un excellent choix pour les développeurs qui veulent bâtir des assistants sérieux, multi-étapes.

## Créer des agents d'intelligence artificielle avec Google ADK

## Comment choisir le bon framework

Avant de vous lancer, clarifiez l’objectif final : que doivent concrètement faire vos agents ? Le bon framework doit s’aligner sur votre stack, votre budget et vos besoins à court comme à long terme.

*Critères pour sélectionner le bon framework. Source : Napkin IA*

### Facilité d’usage

Pour démarrer, Agno ou CrewAI sont d’excellents points d’entrée. Documentation claire, frictions minimales. La syntaxe épurée d’Agno permet de prototyper très vite, tandis que l’organisation par rôles de CrewAI est intuitive, même sans être développeur. Les deux prennent en charge la complexité en coulisses pour vous laisser viser le résultat.

### Complexité du workflow

Besoin de boucles, conditions ou branches ? LangGraph et LangChain sont de bonnes options. LangGraph gère des flux à états complexes : parfait pour des chatbots qui retentent une étape ou escaladent un incident.

Et si vous coordonnez une équipe d’agents aux rôles différents, le modèle de délégation de CrewAI fonctionne très bien.

### Personnalisation

Si vous voulez tout contrôler dans le détail, Atomic Agents est fait pour vous. Aucune abstraction cachée : flexibilité brute pour les équipes de dév. Idéal quand le plug-and-play ne suffit pas (ex. : coordination de drones en situation de crise). LangGraph offre un juste milieu : vous ajoutez votre logique tout en profitant d’outils intégrés pour le courant.

### Besoins d’intégration

Si votre projet dépend d’APIs ou d’outils d’entreprise, et si votre organisation est déjà sur l’écosystème Microsoft, Semantic Kernel s’impose.

CrewAI est aussi solide : connecteurs pratiques pour des APIs comme X. Si votre agent doit agréger de multiples sources, ces frameworks gèrent l’intégration pour vous.

### Environnement de déploiement

Pour des applis locales dans des secteurs sensibles (santé, banque) où la confidentialité est critique, l’Agent SDK d’OpenAI permet de rester on-device/on-prem sans cloud. Si vous visez le cloud, Agno propose le support intégré d’AWS, GCP et des workflows serverless : quasi « déploiement en un clic » en production.

### Performance/évolutivité

Si vous gérez un trafic important ou de gros volumes de données, LangGraph brille par sa capacité à piloter des milliers de workflows simultanés — idéal pour la détection de fraude. AutoGen de Microsoft est conçu pour l’échelle entreprise, avec support des systèmes distribués : parfait pour de grands réseaux et chaînes d’approvisionnement.

### Sécurité et confidentialité

Dans le cloud, Semantic Kernel apporte une sécurité de niveau entreprise. Avec CrewAI, la sécurité dépend de votre déploiement : chiffrement et contrôles d’accès restent à votre charge.

Voici un récapitulatif rapide des critères et des meilleurs choix probables.

| **#** | **Facteur** | **Meilleurs choix** | 
| 1 | Débutant-friendly | CrewAI, Agno | 
| 2 | Workflows complexes | LangGraph, AutoGen | 
| 3 | Personnalisation poussée | Atomic Agents, LangGraph | 
| 4 | Intégration API/outils | Semantic Kernel, CrewAI | 
| 5 | Déploiement cloud | Agno, Semantic Kernel | 
| 6 | Local/axé confidentialité | OpenAI’s Agent SDK | 
| 7 | Applications à l’échelle | AutoGen, LangGraph | 

## Tableau comparatif : en un coup d’œil

*Tableau comparatif des frameworks d’agents IA. Source : auteur*

## Conclusion : lequel choisir ?

Il n’y a pas de vainqueur universel — et c’est tout l’enjeu. Les frameworks d’agents IA sont comme des outils électroportatifs : le bon dépend du chantier.

Démarrez petit, apprenez les spécificités de chaque framework, et n’hésitez pas à combiner les briques (ex. : LangChain + LangGraph). L’objectif n’est pas la perfection : c’est de résoudre des problèmes.

Cela dit, voici notre avis : pour aller vite sans complexité, CrewAI ou Agno sont de bons paris. Si votre équipe a besoin de plus de contrôle ou de puissance, regardez LangGraph pour les systèmes multi-agents ou AutoGen. Et pour les grandes entreprises aux exigences strictes : Semantic Kernel.

Gardez à l’esprit que l’intelligence en temps réel des agents reste en pleine évolution, et que les coûts peuvent grimper vite à l’échelle. Les bugs — surtout en multi-agents — sont bien réels et parfois coriaces. Sans oublier les enjeux de sécurité, éthique et d’explicabilité qui exigent une vigilance constante. Abonnez-vous à notre newsletter IA sélectionnée, The Median pour rester au fait de l’actualité IA.

Professionnel chevronné de la science des données, de l'intelligence artificielle, de l'analyse et de la stratégie des données.

## FAQs

### Qu’est-ce qu’un agent IA ?

**Un agent IA est un programme capable de comprendre un objectif, planifier les étapes et agir — comme un assistant numérique qui travaille en autonomie.**

### Qu’est-ce qu’un framework d’agent IA ?

**C’est une boîte à outils pour créer des systèmes IA capables d’agir de façon autonome — par exemple un bot de service client ou un trader. Les frameworks offrent des briques prêtes à l’emploi pour éviter de tout recoder.**

### Puis-je créer des agents IA sans framework ?

**Oui, mais cela demande plus de temps et d’efforts. Les frameworks vous épargnent de repartir de zéro.**

### Quelle différence entre systèmes mono-agent et multi-agents ?

**Un système mono-agent repose sur un seul agent qui fait tout. Un système multi-agents regroupe plusieurs agents qui se partagent les tâches et collaborent.**

### Les agents IA peuvent-ils utiliser des APIs et des outils externes ?

**Oui, la plupart des frameworks permettent d’appeler des APIs, exécuter des fonctions, rechercher sur le web, etc.**