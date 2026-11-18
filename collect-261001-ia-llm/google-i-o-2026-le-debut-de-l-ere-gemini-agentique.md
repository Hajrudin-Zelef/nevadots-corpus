Cours

Le fil rouge de presque toutes les annonces du premier jour de la conférence Google I/O 2026 était le même : les agents. Pas des chatbots, ni de simples assistants, mais des agents persistants, exécutant des tâches en arrière-plan et intégrés dans l'ensemble de la pile produit de Google. Le CEO Sundar Pichai l'a nommé sans ambiguïté « l'ère Gemini agentique », et les annonces ont largement confirmé ce cadrage.

Google a également répondu aux abonnements Claude Max d'Anthropic et ChatGPT Pro d'OpenAI en introduisant une nouvelle offre Google AI Ultra à I/O, proposée au même prix de 100 $ par mois et donnant accès à certaines des fonctionnalités agentiques détaillées ci-dessous.

Dans cet article, je vous présente les annonces qui comptent le plus pour les praticiens de l'IA et les développeurs. Je me concentre sur les mises à jour disponibles dès maintenant ou attendues très bientôt.

## Gemini 3.5 Flash

Gemini 3.5 Flash est la sortie de modèle phare d'I/O 2026. Il surpasse Gemini 3.1 Pro sur des benchmarks agentiques et de codage, tout en étant, selon Google, 4 fois plus rapide en jetons de sortie par seconde que d'autres modèles de pointe. Nous ne pouvons pas encore le confirmer, mais la promesse est audacieuse.

Côté benchmarks, la progression est visible, notamment sur MCP Atlas, CharXiV Reasoning et Finance Agent v2, où Gemini 3.5 Flash prend la tête. Globalement, 3.5 Flash semble rivaliser avec Claude Opus 4.7 et GPT-5.5.

| **Benchmark** | **3.5 Flash** | **3 Flash** | **3.1 Pro** | **Claude Sonnet 4.6** | **Opus 4.7** | **GPT-5.5** | 
| Terminal-bench 2.1 | 76,2 % | 58,0 % | 70,3 % | -- | 66,1 % | **78,2 %** | 
| SWE-Bench Pro | 55,1 % | 49,6 % | 54,2 % | -- | 64,3 % | **58,6 %** | 
| MCP Atlas | **83,6 %** | 62,0 % | 78,2 % | 69,5 % | 79,1 % | 75,3 % | 
| OSWorld | 78,4 % | 65,1 % | 76,2 % | 72,5 % | 78,0 % | **78,7 %** | 
| Finance Agent v2 | **57,9 %** | 42,6 % | 43,0 % | 51,0 % | 51,5 % | 51,8 % | 
| CharXiv Reasoning | **84,2 %** | 80,3 % | 83,3 % | 72,4 % | 82,1 % | 84,1 % | 
| Humanity's Last Exam | 40,2 % | 33,7 % | 44,4 % | 33,2 % | **46,9 %** | 41,4 % | 
| ARC-AGI-2 | 72,1 % | 33,6 % | 77,1 % | 58,3 % | 75,8 % | **84,6 %** | 

Le volet coûts est à noter. Google affirme que des entreprises traitant environ 1 billion de jetons par jour pourraient économiser plus d'1 milliard de dollars par an en déplaçant 80 % des charges de travail d'autres modèles de pointe vers 3.5 Flash. C'est un message adressé directement aux clients entreprises d'OpenAI et d'Anthropic. Gemini 3.5 Flash est disponible dès aujourd'hui via l'API Gemini, Google AI Studio et l'application Gemini. Gemini 3.5 Pro est déjà utilisé en interne et attendu le mois prochain.

Pour en savoir plus, nous vous recommandons de lire notre article sur Gemini 3.5 Flash, où nous détaillons le nouveau modèle.

## Gemini Omni

Gemini Omni est le nouveau modèle de génération média nativement multimodal de Google, capable de prendre en entrée n'importe quelle combinaison de texte, images, audio et vidéo, et de produire une sortie vidéo. Le premier modèle de la famille, Gemini Omni Flash, est disponible dès aujourd'hui dans l'application Gemini, Google Flow et YouTube Shorts.

Point clé d'architecture : Omni fusionne ce qui était auparavant une pile séparée (Veo pour la vidéo, Imagen pour les images, systèmes audio distincts) en un modèle unique. Résultat : des éditions plus cohérentes et moins d'artefacts de pipeline lors de travaux multi-modaux. Google n'a pas publié de benchmarks chiffrés pour Omni lors du lancement, donc les évaluations indépendantes sont encore à venir. L'accès API pour les développeurs et les clients entreprises arrive dans les semaines suivant I/O.

Nous l'avons testé et détaillé dans notre article sur Gemini Omni. Les premiers résultats de génération vidéo sont inégaux (surtout face aux standards très élevés fixés par des outils comme Seedance 2.0), mais un Gemini Omni Pro plus puissant est attendu prochainement.

## Antigravity 2.0

Antigravity est la plateforme de développement centrée agents de Google, et la version 2.0 présentée à I/O marque une expansion majeure. Autrefois positionné comme un environnement de codage, c'est désormais une plateforme complète pour développer, déployer et gérer des cohortes d'agents IA autonomes. La pièce maîtresse est une nouvelle application de bureau autonome servant de hub central pour l'orchestration : vous pouvez exécuter plusieurs agents en parallèle sur des tâches distinctes, simultanément.

L'écosystème compte désormais quatre surfaces distinctes pour les développeurs :

- **Application de bureau Antigravity 2.0 :** orchestre plusieurs agents en parallèle et gère des tâches en arrière-plan planifiées. S'intègre avec Google AI Studio, Android et Firebase.
- **Antigravity CLI :** interface native terminal pour créer et exécuter des agents sans GUI. Google invite les utilisateurs de Gemini CLI à migrer.
- **Antigravity SDK :** accès programmable au même harnais d'agents qui propulse les produits Google, avec prise en charge de comportements d'agents personnalisés hébergés sur votre propre infrastructure.
- **Antigravity dans Gemini Enterprise Agent Platform :** connecte Antigravity directement aux projets Google Cloud pour les charges de travail entreprises.

Le cœur de l'agent gagne aussi plusieurs fonctions très utiles. La plus marquante : il peut désormais générer à la volée des **sous-agents modulaires**, chacun s'exécutant en parallèle avec isolement d'espace de travail, tout en héritant des outils et permissions du parent. Les opérations longues tournent en asynchrone et ne bloquent plus la boucle de l'agent.

Dans l'esprit des Claude Code Hooks, les **JSON Hooks** permettent d'attacher des scripts shell personnalisés à des étapes clés (avant/après appels d'outils, appels de modèle ou aux conditions d'arrêt) pour la journalisation, l'ajustement d'arguments ou l'injection d'instructions. Les **tâches planifiées** permettent de définir des invites basées sur cron pour des exécutions périodiques d'agents comme des synthèses quotidiennes de PR ou des vérifications de déploiement horaires, avec les résultats affichés dans la barre latérale pour une passation fluide avec l'humain dans la boucle.

Côté administration, Antigravity introduit les « projets » comme unité d'organisation pour borner les paramètres, ressources et permissions par groupe d'agents, évitant d'imposer des permissions globales trop larges. Le panneau latéral repensé permet de regrouper les conversations par projet, statut ou récence, avec une **prise en charge native des worktrees Git**. Cette organisation par projet rappelle la gestion multi-fenêtres de Cursor et la file de tâches de Codex, avec un scoping de permissions plus fin par projet.

**Saisie vocale** via les modèles audio Gemini et **nouvelles commandes slash** (`/goal` pour des exécutions autonomes, `/grill-me` pour clarifier avant tâche, `/schedule` pour des invites cron, `/browser` pour activer le navigateur) complètent l'expérience.

**A**ntigravity 2.0 est disponible dès aujourd'hui. L'offre Google AI Ultra (100 $/mois) inclut un quota d'usage 5 fois plus élevé dans Antigravity par rapport à l'offre Google AI Pro.

## Agents managés dans l'API Gemini

En parallèle d'Antigravity 2.0, Google a annoncé les Agents managés dans l'API Gemini, qui apportent des capacités agentiques directement dans la couche API pour les développeurs souhaitant bâtir des applications propulsées par des agents sans gérer eux-mêmes l'orchestration. C'est le pendant côté API de l'expérience de bureau Antigravity.

Concrètement, vous pouvez définir des comportements d'agent, des intégrations d'outils et des flux multi-étapes via l'API Gemini, et l'infrastructure Google prend en charge l'exécution. Potentiellement un vrai changement d'échelle pour les équipes qui construisent des applications de production nécessitant des tâches de longue haleine, sans déployer leur propre harnais d'agents. L'accès est disponible via Google AI Studio, et les clients entreprises y accèdent via la Gemini Enterprise Agent Platform.

Un bémol honnête : les premiers retours développeurs depuis I/O indiquent que la documentation des flux agentiques complexes et de la gestion d'erreurs reste clairsemée. Les limites de taux et la gestion des quotas sont aussi citées comme sources de friction. Ces points devraient s'atténuer avec le temps, mais il faut en avoir conscience avant de s'engager sur cette pile.

## Gemini Spark

Gemini Spark (ne pas confondre avec le dernier LLM de Meta, Muse Spark) est le nouvel agent personnel de Google, et c'est l'annonce agentique la plus orientée grand public. Il tourne 24 h/24, 7 j/7 sur des machines virtuelles dédiées dans Google Cloud, sans nécessiter que votre ordinateur reste ouvert. Spark est propulsé par Gemini 3.5 et le harnais Antigravity, ce qui lui permet de gérer des tâches de longue durée en arrière-plan.

Les fonctionnalités au lancement incluent :

- Intégration avec les **outils Google (Workspace, Gmail, Calendar)** dès le départ, avec support des outils tiers via MCP dans les semaines à venir.
- Interaction via l'**application Gemini** , puis bientôt par e-mail et chat.
- Fonctionnement directement dans **Chrome** comme couche de navigation agentique, attendu plus tard cet été.
- Suivi en direct de l'avancement des tâches via **Android Halo** , un nouvel espace d'interface sur Android, prévu plus tard cette année.

La comparaison avec l'écosystème d'agents d'OpenAI et les capacités d'usage d'outils d'Anthropic s'impose. Le différenciateur de Spark est l'exécution persistante 24 h/24 sur l'infrastructure Google Cloud, combinée à une intégration profonde à la suite de productivité de Google. Si votre travail vit déjà dans Google Workspace, c'est un vrai plus. Sinon, la proposition de valeur est moins évidente.

La confidentialité est ici une préoccupation légitime. Un agent qui surveille en continu votre boîte mail, votre agenda et vos documents pose de vraies questions de résidence des données et de conformité dans les secteurs réglementés. L'une des questions que je me posais, par exemple, était : « Que devient la mémoire de l'agent quand un collaborateur part ? » Google n'a pas encore apporté de réponses détaillées.

Spark est déployé cette semaine auprès de testeurs de confiance, avec une bêta pour les abonnés Google AI Ultra (100 $/mois) aux États-Unis la semaine suivante. Nous le couvrons en détail dans notre article sur Gemini Spark.

## Agents de recherche et mode IA

Le mode IA dans Search a été introduit lors du dernier I/O. Un an plus tard, il dépasse le milliard d'utilisateurs actifs mensuels. Google va plus loin avec deux nouvelles capacités agentiques.

La première : les **agents d'information dans Search** : des agents personnels en arrière-plan que vous configurez pour surveiller des sujets et faire remonter les informations pertinentes au bon moment. Ils seront déployés cet été, d'abord pour les abonnés Google AI Pro et Ultra.

La seconde : une **interface générative dans Search**, propulsée par Gemini 3.5 Flash et Antigravity. Search construira désormais des mises en page sur mesure, des visuels interactifs, et même des tableaux de bord persistants ou mini-apps pour des requêtes complexes et de longue durée. Les capacités d'interface générative arriveront gratuitement pour tous cet été. Les tableaux de bord persistants et apps personnalisées seront d'abord disponibles pour les abonnés Pro et Ultra aux États-Unis.

Cela inquiète légitimement les éditeurs et les professionnels du SEO (comme l'avaient déjà fait AI Overview et le mode IA). Quand des réponses générées par l'IA résolvent entièrement la requête dans Search, il n'y a plus de raison de cliquer vers la source. On l'a déjà constaté : les AI Overviews et le premier déploiement du mode IA ont entraîné des baisses de trafic marquées. Google n'a toujours pas proposé de mécanisme clair de partage de revenus ni de garantie de trafic pour les éditeurs dont le contenu alimente ces réponses.

## Google Flow

Google Flow, présenté à I/O 2025 comme un outil de création de films avec IA, franchit une étape majeure avec trois évolutions clés :

- **Agent de planification plus intelligent.** L'agent Flow mis à jour sait désormais planifier et raisonner sur des projets créatifs en plusieurs étapes. Vous lui fournissez vos entrées (par exemple un concept, des images références, un script brouillon) et il vous aide à passer du brainstorming à la création puis au montage, dans un même environnement. Le nouvel agent est disponible dès aujourd'hui pour tous.
- **Vidéo native avec Gemini Omni.** Flow gère désormais la génération et l'édition vidéo nativement via le modèle Omni. Vous pouvez décrire en langage naturel les modifications à apporter à un clip de votre pellicule et itérer de manière conversationnelle. La cohérence des personnages s'améliore aussi : identité et voix sont préservées entre les scènes, utile pour un court-métrage ou une campagne avec des personnages récurrents.
- **Vibe coding pour des outils sur mesure.** Au lieu de se limiter aux outils livrés avec Flow, vous pouvez désormais créer les vôtres directement sur la plateforme. Google a montré des exemples comme des effets vidéo personnalisés, des outils d'animation dessinée à la main ou des workflows de superposition de texte, sans quitter Flow.

Ensemble, ces mises à jour positionnent Flow au-delà d'un simple assistant créatif. Il devient une plateforme de construction de workflows créatifs, avec une application mobile désormais en bêta sur Android et bientôt sur iOS.

## Extension de SynthID

SynthID, le système de filigrane invisible de Google pour l'IA, a déjà marqué plus de 100 milliards d'images et vidéos et 60 000 années d'actifs audio cumulés depuis son lancement il y a trois ans. L'annonce la plus importante à I/O n'est pas l'ampleur, mais les partenaires : OpenAI, Kakao et Eleven Labs adoptent SynthID aux côtés de Nvidia, signé l'année dernière.

L'adoption intersectorielle fait toute la différence. Un standard de filigrane n'est utile que s'il est suffisamment répandu pour que « non filigrané » devienne un signal pertinent. Google étend aussi la vérification Content Credentials (standard C2PA) à Search et Chrome, afin d'indiquer si un contenu provient d'une IA ou d'un appareil photo et s'il a été édité avec des outils génératifs. La combinaison SynthID + C2PA ajoute deux couches indépendantes de provenance : une approche pertinente, sachant qu'il est facile de supprimer l'une ou l'autre isolément.

## Mentions honorables

Plusieurs autres annonces de I/O méritent un rapide coup d'œil :

- **Docs Live :** une nouvelle fonction vocale pour Google Docs qui permet de déverser vos idées à l'oral et de laisser Gemini les structurer en document. Déploiement cet été pour les abonnés, avec des fonctions vocales également prévues pour Gmail et Keep au même moment.
- **Google Pics :** un nouvel outil IA de création et d'édition d'images, bâti sur le modèle Nano Banana, qui traite chaque élément comme un objet individuel et non comme une image plate. Disponible dès maintenant pour des testeurs de confiance, déploiement cet été pour les abonnés Google AI Pro et Ultra.
- **Android Halo :** un nouvel espace d'interface sur Android pour visualiser en direct les mises à jour et l'avancement des tâches d'agents comme Gemini Spark. Arrive plus tard cette année.
- **Daily Brief :** un agent prêt à l'emploi dans l'application Gemini qui compile un résumé matinal personnalisé à partir de votre boîte mail, de votre agenda et de vos tâches, avec des prochaines étapes suggérées. Pas de tarification séparée annoncée ; attendu comme partie intégrante de l'expérience de l'app Gemini.
- **TPU 8t et 8i :** les TPU de 8e génération de Google adoptent une approche bi-puce : 8t optimisé pour le pré-entraînement à grande échelle (près de 3 fois la puissance de calcul brute de la génération précédente, extensible à plus d'1 million de TPU dans le monde) et 8i optimisé pour l'inférence. Les deux offrent jusqu'à 2 fois plus de performance par watt que la génération précédente.
- **Gemini for Science :** un ensemble d'outils IA reliant Antigravity à plus de 30 grandes bases de données en sciences de la vie. Science Skills est disponible dès aujourd'hui sur GitHub et directement dans Antigravity.

## Pensées finales

Google I/O 2026 parie sur les agents comme trajectoire principale de l'IA, avec Gemini 3.5 Flash et Antigravity 2.0 comme infrastructure sous-jacente à presque tout le reste. Ce que vous pouvez utiliser dès maintenant : Gemini 3.5 Flash (via l'API Gemini et AI Studio), le nouvel agent Flow, Gemini Omni Flash et l'application de bureau Antigravity 2.0. Gemini Spark, les agents Search et l'interface générative de Search arriveront au fil de l'été, principalement réservés à la nouvelle offre AI Ultra à 100 $/mois (au moins au lancement).

Pour moi, la mise à niveau d'Antigravity est la plus intéressante, car elle **opère à deux niveaux simultanément : en tant qu'application autonome pour développeurs, elle concurrence directement Codex et Claude Code ; en tant que plateforme, son ADK sous-jacent et son API Managed Agents challengent des cadres d'orchestration comme LangChain, AutoGen et l'Agents SDK d'OpenAI. L'intégration à Gemini et la couche de déploiement Google Cloud sont les différenciateurs (et le risque de verrouillage) sur les deux fronts.**

## Google I/O 2026 : FAQ

### Comment Gemini 3.5 Flash se compare-t-il à GPT-5.5 et Claude Opus 4.7 ?

Gemini 3.5 Flash est en tête sur plusieurs benchmarks agentiques comme MCP Atlas (83,6 %) et Finance Agent v2 (57,9 %), tandis que GPT-5.5 devance sur SWE-Bench Pro et ARC-AGI-2. Claude Opus 4.7 reste le meilleur sur Humanity's Last Exam (46,9 %). À retenir : il rivalise avec les modèles de pointe tout en tournant plus vite et beaucoup moins cher à grande échelle, comme son nom le laisse entendre. Une variante Pro plus puissante est attendue prochainement.

### En quoi Google Antigravity diffère-t-il de Claude Code ou Codex ?

Google Antigravity 2.0 est une plateforme de développement centrée agents qui vous permet d'orchestrer plusieurs agents IA en parallèle via une application de bureau, une CLI, un SDK et une API entreprise. Contrairement à Claude Code (agent de codage orienté terminal) ou Codex (système basé sur une file de tâches), Antigravity propose un scoping de permissions plus fin par projet, la création de sous-agents et une intégration directe à Google Cloud et Firebase. Son double rôle d'outil développeur et de SDK de plateforme le rapproche davantage d'un cadre d'orchestration que d'un simple assistant de codage.

### L'abonnement Google AI Ultra à 100 $/mois vaut-il le coup face à ChatGPT Pro ou Claude Max ?

Les trois offres sont facturées 100 $/mois, mais la valeur dépend de votre écosystème. Le différenciateur de Google AI Ultra est l'accès à Gemini Spark (un agent persistant 24 h/24), un quota d'usage Antigravity 5 fois supérieur et une intégration profonde à Google Workspace. Si votre flux de travail repose déjà sur Gmail, Docs et Calendar, Ultra a un avantage naturel. Si vous cherchez surtout de l'aide au codage ou de la flexibilité au niveau API, ChatGPT Pro ou Claude Max pourront mieux convenir.

### Qu'est-ce que Gemini Omni et comment gère-t-il la génération vidéo ?

Gemini Omni est le modèle nativement multimodal de Google acceptant n'importe quel mélange de texte, images, audio et vidéo en entrée, et produisant une sortie vidéo. Il unifie des systèmes auparavant séparés (Veo pour la vidéo, Imagen pour l'image) en un seul modèle, ce qui doit améliorer la cohérence des éditions entre modalités. La première version, Omni Flash, est disponible maintenant, avec un Omni Pro plus puissant attendu bientôt. Aucun benchmark indépendant n'a encore été publié, donc la qualité en situation réelle reste à évaluer.

**Rédacteur en chef Data Science chez DataCamp |** **Je suis passionné par la prévision et le développement à l'aide d'API.**