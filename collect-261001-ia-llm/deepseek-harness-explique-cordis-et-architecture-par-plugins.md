Cursus

DeepSeek Harness est conçu pour exécuter une tâche, pas seulement répondre à une question. Il s’agit d’un environnement d’exécution d’agent open source qui connecte un modèle à votre dépôt, votre terminal, vos outils et l’historique de session. Demandez-lui de corriger un bug : il peut inspecter des fichiers, modifier du code, lancer des tests et réagir quand une commande échoue. Un simple appel modèle ne peut pas faire tout cela seul.

La partie la plus originale se trouve sous ce flux de travail. DeepSeek Harness expose l’adaptateur de modèle, les outils, les sessions, le bac à sable et même la boucle de l’agent sous forme de plugins coordonnés par Cordis. Le modèle n’est qu’un des composants de l’agent, pas le produit en soi.

Ce n’est pas un logiciel abouti. Harness est encore en préversion développeur ; ses API peuvent changer entre deux versions et son propre avis de sécurité indique qu’aucun audit n’a été réalisé. J’aborderai ces limites en même temps que l’architecture et les différences avec Claude Code, Codex et OpenCode.

## En bref

- **Ce que c’est :** DeepSeek Harness est un environnement d’exécution d’agent open source, pas un modèle. Il fournit au modèle des outils, des sessions, un bac à sable et une boucle d’agent.
- **Conception centrale :** Cordis expose l’adaptateur de modèle, les outils, le magasin de sessions, le bac à sable et la boucle de l’agent comme des plugins interchangeables.
- **Sessions :** Un journal d’événements en ajout-only permet la reprise, le fork, la recherche, la relecture et la vue Trajectory.
- **Modes :** Standard, PTC, Minimal et Creator modifient les outils accessibles à l’agent et la manière d’y accéder.
- **Principale différence :** DeepSeek Harness permet aux développeurs de remplacer des composants bas niveau que Claude Code, Codex et OpenCode laissent fixes.
- **Principale limite :** cela reste une préversion développeur sans audit de sécurité, et ses API peuvent évoluer entre versions.

## Introduction aux agents d'intelligence artificielle

## Qu’est-ce que DeepSeek Harness ?

DeepSeek Harness, abrégé en `dsh`, est un agent harness open source de DeepSeek AI sous licence MIT. Il s’intercale entre un modèle de langage et le monde extérieur, en fournissant des outils, des sessions, un bac à sable et la boucle qui fait avancer la tâche.

Chez DeepSeek, l’équation est **« Agent = Modèle + Harness ».** Le modèle gère le raisonnement et la génération. Le harness correspond à tout ce qui permet à ce raisonnement d’agir sur un système de fichiers réel et de poursuivre sans que vous ayez à réexpliquer la tâche à chaque étape.

Il s’appuie sur Cordis, un framework de plugins antérieur à DeepSeek Harness. Cordis permet de remplacer ces briques de manière indépendante via la configuration. Je reviendrai plus loin sur le coût de ce choix.

Avec ce cadre en tête, voici deux idées reçues fréquentes.

### DeepSeek Harness n’est pas un modèle d’IA

Comme indiqué plus haut, le modèle et l’environnement d’exécution sont deux couches distinctes. Cette séparation permet de changer de fournisseur sans toucher aux outils ni au paramétrage des sessions. Le même runtime peut utiliser DeepSeek, Anthropic, OpenAI ou un endpoint compatible OpenAI.

### DeepSeek Harness va au-delà d’un assistant de code

Le mode Standard peut donner l’impression d’un assistant de développement, mais ce n’est qu’une configuration. Comme nous le verrons, les modes Minimal et Creator changent les outils accessibles à l’agent. Construire une nouvelle configuration demande toujours du travail d’ingénierie ; les développeurs ont accès aux pièces.

## Comment Cordis organise les plugins de DeepSeek Harness

Comme mentionné, Cordis est le framework de plugins sous DeepSeek Harness. Il permet à chaque composant de demander un service sans lier le code à un fournisseur unique.

Cordis vient de l’écosystème du chatbot Koishi et a été construit par un développeur connu sous le nom de Shigma ; DeepSeek le distribue et l’étend. Les auteurs décrivent la conception dans leur article A Programming Paradigm for Spatiotemporal Composability.

Ces bases mènent au slogan principal du projet et à deux notions de Cordis. Les noms paraissent académiques, mais le fonctionnement reste simple.

### « Tout est un plugin »

La documentation d’architecture de DeepSeek explique que vous étendez `dsh` en montant un plugin à côté des autres. Les adaptateurs de modèle, les outils, les sessions, les bacs à sable, le stockage, l’ordonnancement, la boucle d’agent et l’interface sont tous des plugins.

Pris au pied de la lettre, le slogan va trop loin. Cordis reste en dessous des plugins. Il les charge et les décharge, vérifie leurs dépendances et pilote les événements qu’ils utilisent pour communiquer. Cordis est indispensable, ce n’est pas une pièce optionnelle de plus.

### La composabilité spatiale gère les dépendances entre plugins

Un plugin déclare les services dont il a besoin sans exiger une séquence de démarrage écrite à la main. Il s’active quand ces services existent et se désactive si un service requis disparaît. Ses dépendances dictent quand il peut s’exécuter.

DeepSeek appelle cela composabilité spatiale. Les dépendances indiquent à Cordis où se place un composant, évitant aux développeurs d’ordonner le démarrage à la main.

### La composabilité temporelle annule les effets des plugins

Cordis suit aussi les enregistrements tels que les écouteurs d’événements, sections de prompt et schémas d’outils. Retirer un plugin supprime ces effets au lieu de laisser des écouteurs orphelins. Cela n’annule pas une action externe comme une commande shell ; la réversibilité ne s’applique qu’aux effets suivis par Cordis.

## Architecture de DeepSeek Harness : comment s’assemble le runtime

Une instance en cours d’exécution est un arbre de plugins construit à partir de paramètres chargés dans un ordre défini. Ces paramètres déterminent les briques actives.

Cordis connecte tous les plugins remplaçables du runtime. Image de l’auteur.

### Les services Cordis permettent aux plugins de se découvrir

Cordis fournit un annuaire partagé de services. Les plugins utilisent des clés stables comme `ctx.tools`, `ctx.llm` et `ctx.sessions` au lieu d’importer le code d’un fournisseur. Un outil qui appelle `ctx.llm` n’a pas besoin de savoir quel adaptateur de modèle se trouve derrière.

### Presets d’agent et profils d’exécution pilotent des couches différentes

Si tout est remplaçable, il faut tout de même décider quoi monter pour une exécution donnée, et DeepSeek Harness répond à deux niveaux faciles à confondre.

Version courte : un profil contrôle la façon dont le programme démarre, tandis qu’un preset contrôle ce que l’agent peut faire. Si vous n’utilisez que l’application web, vous pouvez ignorer les deux sous-sections suivantes.

#### Profils d’exécution

Un profil d’exécution (`web`, `headless`, `sdk`, `sdk-minimal` et `acp` sont fournis comme modèles) décide comment l’application se lance et quels bundles de plugins Cordis sont empilés au démarrage. La plupart des lecteurs ne toucheront à ce niveau qu’en lançant `dsh web` ou une commande similaire.

#### Presets d’agent

Un preset d’agent (Standard, PTC, Minimal ou Creator) décide de ce qu’une session active peut utiliser. Un fichier de patch peut changer le preset sans modifier le code source d’Harness.

### La boucle d’agent coordonne tours, étapes et appels d’outils

DeepSeek distingue une étape d’un tour. Une étape est une requête modèle plus ses appels d’outils. Un tour regroupe zéro ou plusieurs étapes : il s’ouvre avant que sa première entrée ne soit prise en charge et se ferme quand plus rien n’est dû. La plupart des tours exécutent plusieurs étapes avant que l’agent ne réponde, mais une entrée rejetée ferme un tour sans étape consommée.

Un tour peut contenir plusieurs étapes. Image de l’auteur.

### Les sessions utilisent un journal d’événements en ajout-only

C’est, à mon sens, l’élément le plus important. Une session est un journal append-only d’événements typés, pas un tableau de messages de chat. Harness reconstruit l’historique du modèle à partir de ce journal, et la documentation des sessions exige que tout ce qui est envoyé au modèle soit récupérable depuis celui-ci.

La reprise, le fork, la recherche, la relecture et la vue Trajectory reposent tous sur ce flux d’événements.

Reconstituer l’historique n’est pas une réexécution déterministe. La sortie du modèle et l’état externe peuvent différer, mais le journal fournit tout de même une trace consultable de ce qui s’est passé.

L’historique de session est un journal en ajout-only. Image de l’auteur.

### Comment DeepSeek Harness contrôle les outils et les bacs à sable

Un modèle peut demander un outil par son nom, mais il ne peut pas l’exécuter directement. Deux contrôles distincts s’interposent entre la demande et une modification du système de fichiers.

#### Le pipeline d’exécution des outils

L’appel passe par une vérification de politique, l’exécution, puis le traitement du résultat. Le modèle choisit l’outil ; le runtime décide si et comment il s’exécute.

Le runtime décide comment les outils s’exécutent. Image de l’auteur.

#### Bac à sable versus approbations

- **Approbation** demande si l’utilisateur doit confirmer une action.
- **Bac à sable** limite où et comment l’action s’exécute.

DeepSeek les sépare, même si des presets d’autorisations regroupent les deux contrôles, à l’image d’un runtime de conteneur qui sépare permissions de processus et limites d’exécution.

À signaler dès maintenant, car j’y reviendrai dans les limites : dire à un modèle dans un prompt système de « ne lire que des fichiers » est une consigne qu’il peut choisir de suivre, pas une barrière imposée comme le ferait une restriction de bac à sable au niveau OS.

## Modes de DeepSeek Harness : Standard, PTC, Minimal et Creator

DeepSeek Harness propose quatre modes. Aucun n’est « meilleur » que les autres. Ce sont quatre réponses à « quelle part du runtime doit être exposée à cette session », et le bon choix dépend de la tâche. Comme l’a montré l’architecture, chaque mode modifie l’ensemble d’outils disponible pour l’agent.

Quatre modes, une base runtime commune. Image de l’auteur.

### Mode Standard

Le socle polyvalent :

- Édition de fichiers
- Accès shell
- Recherche de fichiers et sur le web
- Skills
- Planification
- Objectifs
- Sous-agents
- Workflows

Pour un travail habituel sur dépôt, c’est mon point de départ.

### Mode PTC

Le mode PTC conserve presque tout l’outillage de Standard mais change la façon dont le modèle y accède. (Depuis la version 0.1.2, le mode Web PTC n’expose plus par défaut l’outil générique `workflow`.) 

Au lieu d’appeler des outils un par un sur plusieurs étapes, le modèle écrit un programme contre un SDK généré. Ce programme peut invoquer plusieurs outils via `run_code`. Chaque appel passe toujours par les mêmes contrôles de politique : PTC change la façon d’énoncer le plan, pas ce que le modèle est autorisé à faire.

La page produit utilise encore l’étiquette « Code mode », mais une version officielle plus récente l’a renommé « mode PTC » tout en gardant lisibles les anciennes conversations. J’utiliserai « mode PTC » partout ; la FAQ revient sur la signification possible de ces initiales.

### Mode Minimal

Le mode Minimal réduit l’environnement à deux outils : un shell persistant et un éditeur de fichiers par substitution de chaînes. DeepSeek l’utilise pour les benchmarks de modèles, car les résultats dépendent en partie du harness, pas seulement des poids du modèle.

### Mode Creator

Le mode Creator permet aux développeurs d’inspecter le runtime et de tester des plugins Cordis en mémoire. Il sert à construire des presets ; je ne le qualifierais pas « d’auto-améliorant » dans un sens plus profond.

## Ce qui distingue DeepSeek Harness des autres frameworks d’agents

DeepSeek Harness se distingue de nombreux frameworks d’agents en rendant remplaçables les couches basses du runtime. J’aurais pu l’intégrer à l’architecture, mais la nuance est facile à manquer. Cordis gère ces changements via un système unique de plugins.

Vous pouvez modifier le fonctionnement même de l’agent, pas seulement les outils qu’il appelle. Le journal d’événements crée également une exécution inspectable par les développeurs, plutôt qu’un simple relevé de chat. Les modes Minimal et Creator permettent ensuite de tester le runtime depuis deux angles opposés.

## DeepSeek Harness vs Claude Code, Codex et OpenCode

Une checklist de fonctionnalités passerait à côté de l’essentiel. Chaque concurrent prend en charge des extensions ; la vraie question est : quelles parties les développeurs peuvent-ils changer ? La nuance semble minime, elle ne l’est pas. Notre comparatif dédié Harness vs Claude Code utilise le même modèle des deux côtés et couvre configuration, journaux et coût.

### DeepSeek Harness vs Claude Code

Claude Code prend en charge des instructions de projet, des skills, des hooks, le MCP, des sous-agents et un Agent SDK, tout en gardant sa boucle interne fixe. DeepSeek Harness permet de remplacer via configuration la boucle, l’adaptateur de modèle et la couche de stockage.

### DeepSeek Harness vs Codex

Codex nécessite une comparaison plus fine, car son CLI et son App Server sont également open source. Il fournit un agent harness que les développeurs étendent via des points d’entrée documentés. DeepSeek Harness est conçu pour modifier le runtime lui-même. Les niveaux de contrôle offerts diffèrent.

### DeepSeek Harness vs OpenCode

OpenCode est déjà open source, fonctionne avec plusieurs fournisseurs de modèles et utilise une architecture client-serveur. Vous pouvez configurer ses outils, permissions, sessions et fournisseurs. Ses plugins étendent un noyau serveur fixe, tandis que DeepSeek rend aussi la boucle et le magasin de sessions remplaçables.

## Quand utiliser DeepSeek Harness

Remplacer des parties du runtime n’a pas d’intérêt en soi. Ce contrôle supplémentaire ne compte que s’il résout un problème que vous avez déjà.

- **Quand le runtime fait partie du projet.** Si vous modifiez des adaptateurs de modèle, la boucle d’agent, le stockage ou le comportement des sessions — pas seulement si vous construisez au-dessus d’un agent —, c’est un meilleur choix.
- **Quand vous comparez des modèles en environnement contrôlé.** Utiliser le même runtime fixe davantage de paramètres lors des échanges de modèle, même si les modèles diffèrent encore par l’usage des outils et le style de raisonnement.
- **Quand le débogage d’une exécution complexe est crucial.** Le journal d’événements de session et la vue Trajectory facilitent la reconstitution de ce que le modèle a vu et quels outils ont tourné.
- **Quand vous testez l’interne des agents.** Le mode Creator et Cordis s’adressent aux développeurs qui étudient la composition des agents, plus qu’à ceux qui veulent seulement générer du code applicatif.

Cela peut être superflu pour de simples appels modèle ou pour des équipes qui veulent un agent de code prêt à l’emploi sans toucher à ses entrailles. Remplacer davantage de pièces ne vaut l’effort que si ce contrôle résout un vrai problème.

## Limites de DeepSeek Harness : statut de préversion et risques de sécurité

Toute l’architecture ci-dessus importe peu sans une vision claire de ses limites actuelles.

### C’est toujours une préversion développeur

Le dépôt DeepSeek indique clairement que des changements cassants surviendront. C’est déjà arrivé : le renommage de Code en PTC s’est accompagné de changements d’API de session et du retrait d’une option SQLite facultative. Figez vos versions. Sauter cette étape en espérant une stabilité de configuration n’est pas une stratégie.

### Plus de contrôle, plus de complexité

Rendre davantage de couches remplaçables implique davantage à apprendre : dépendances de plugins, paramètres, différences entre fournisseurs, compatibilités de versions. C’est le compromis habituel entre confort et contrôle.

### DeepSeek Harness est-il local ?

DeepSeek Harness stocke par défaut le contenu des sessions, les traces d’outils et les paramètres en local, conformément à sa déclaration de traitement des données. Vous pouvez désactiver ses rapports anonymes sur les paramètres et les listes de projets.

Mais un fournisseur de modèle externe, un outil web, un serveur MCP ou un plugin peuvent toujours envoyer des données hors de votre machine selon leur propre politique. « Local-first » ne couvre pas tous les services que vous connectez.

### Exécuter des agents comporte des risques de sécurité

Un runtime capable d’éditer des fichiers, d’exécuter des commandes et de charger des plugins tiers peut causer de vrais dégâts. L’avis de sécurité de DeepSeek précise qu’aucun audit n’a été mené. Bacs à sable, approbations et contrôles d’autorisations réduisent le risque sans garantir l’isolation.

Exécuter le logiciel sur votre propre machine n’élimine pas ce risque. Utilisez des permissions limitées et un environnement jetable pour les tâches non fiables, et soyez vigilant avec les contenus susceptibles de contenir des instructions cachées.

## Pourquoi le comportement d’un agent dépend de plus que du modèle

Le comportement d’un agent dépend du runtime autant que du modèle. On revient à « Agent = Modèle + Harness », et cette séparation vaut pour les agents LLM au-delà de DeepSeek.

Ce qu’un modèle peut produire dépend de ses poids. Ce que fait un agent dépend aussi du contexte transmis au modèle, des actions autorisées et du degré de contrainte de l’exécution. Rien de cela ne réside dans les poids.

DeepSeek Harness met en lumière cette couche environnante en la découpant en composants nommés et remplaçables. Le mode Minimal illustre pourquoi cela dépasse DeepSeek : un score de benchmark reflète en partie le harness utilisé pour le test, pas seulement le modèle. Le harness ne rend pas un modèle plus « intelligent ». Il change le cadre dans lequel il opère.

## Conclusion

La phrase d’ouverture est à retenir : le modèle raisonne, mais le runtime décide à quoi ce raisonnement peut accéder et ce qu’il peut faire. DeepSeek Harness rend ce runtime modifiable, de l’adaptateur de modèle et des outils jusqu’au magasin de sessions et à la boucle d’agent.

Ce contrôle a un coût. Remplacer davantage de briques signifie assumer davantage la configuration, les évolutions de versions et les frontières de sécurité. Une préversion développeur avec accès shell n’est pas un outil à « installer et oublier ».

Mon avis est simple : utilisez DeepSeek Harness quand le runtime fait partie du travail. Si vous avez seulement besoin de modifications dans un dépôt, un agent de code prêt à l’emploi vous en demandera moins.

Notre tutoriel DeepSeek Harness couvre la configuration. Le guide sur les alternatives à Claude Code compare d’autres agents de code, tandis que Introduction to AI Agents revient sur les bases supposées connues ici.

## FAQ sur DeepSeek Harness

### DeepSeek Harness est-il la même chose qu’un modèle DeepSeek ?

**Non, le modèle et le runtime sont distincts. Harness n’inclut pas les poids d’un modèle et n’exécute pas l’inférence lui-même ; il envoie des requêtes à DeepSeek, Anthropic, OpenAI ou à un modèle local.**

### L’utilisation de DeepSeek Harness est-elle gratuite ?

**Le logiciel lui-même est gratuit et sous licence MIT. Ce qui ne l’est pas, c’est le fournisseur de modèle auquel vous vous connectez, puisque l’inférence est facturée séparément par l’opérateur du modèle, ainsi que tout coût d’infrastructure lié aux bacs à sable ou services externes que vous ajoutez par-dessus.**

### Que signifie réellement l’acronyme PTC ?

**Les notes de version de DeepSeek utilisent « mode PTC » sans en donner une forme développée officielle, même si le comportement correspond à « programmatic tool calling ». J’en ferais une définition de travail, pas un sigle confirmé, tant que DeepSeek n’en précise pas un explicitement.**

### Puis-je confier à DeepSeek Harness un dépôt qui compte pour moi ?

**Certaines limites demeurent. Pour un dépôt important, travaillez sur une copie ou une branche séparée, gardez les identifiants de production hors de l’environnement et examinez chaque plugin avant de le charger.**

### « Tout est un plugin » signifie-t-il que je peux en faire n’importe quel type d’agent ?

**Pas sans un vrai travail d’ingénierie. Remplacer l’adaptateur de modèle ou la boucle d’agent nécessite un plugin qui respecte le contrat de service adéquat. Le système de plugins vous donne accès à plus de pièces ; il ne fait pas disparaître le travail.**