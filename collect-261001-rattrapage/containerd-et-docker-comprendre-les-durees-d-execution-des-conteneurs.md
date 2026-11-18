Cours

Dans l'écosystème des conteneurs, une source fréquente de confusion survient lorsque les développeurs comparent Docker (une plateforme complète) à containerd, qui est en réalité un composant d'exécution spécialisé. Cette comparaison revient à comparer une automobile complète à son moteur : les deux sont essentiels, mais ils remplissent des fonctions différentes à des niveaux d'abstraction différents.

Dans ce guide, je vais clarifier la relation entre ces deux technologies, en explorant leurs architectures, l'intégration de Kubernetes et les scénarios spécifiques dans lesquels chaque outil excelle. Que vous construisiez des conteneurs localement ou que vous gériez des clusters Kubernetes de production, comprendre quand utiliser Docker plutôt que containerd peut avoir un impact significatif sur vos décisions en matière d'infrastructure.

À la fin de cet article, vous aurez une compréhension claire de la manière dont Docker et containerd se complètent, et vous saurez comment choisir l'outil adapté à vos besoins spécifiques, qu'il s'agisse de développement local rapide ou de déploiements de production à grande échelle.

Si vous débutez dans le domaine de la conteneurisation, je vous recommande vivement de suivre notre cours sur les concepts de conteneurisation et de virtualisation.

## Qu'est-ce que Docker ?

Docker a révolutionné le paysage du développement logiciel en rendant la conteneurisation accessible et pratique pour les développeurs au quotidien. En tant que plateforme complète, Docker fournit tous les éléments nécessaires pour créer, livrer et exécuter des applications conteneurisées.

Pour les débutants, ce guide pratique sur les conteneursconstitue une excellente introduction à Docker et aux conteneurs.

### Une plateforme de conteneurs complète

Docker fonctionne comme une plateforme de conteneurs tout-en-un , offrant une pile d'outils intégrée qui couvre la majeure partie du cycle de vie des conteneurs. À la base, Docker regroupe les applications et toutes leurs dépendances dans des images portables et autonomes qui peuvent fonctionner de manière cohérente dans n'importe quel environnement, de l'ordinateur portable d'un développeur aux serveurs de production.

Cette approche globale a fait de Docker la norme industrielle en matière de développement local et d'expérience développeur. Parmi les principaux avantages, on peut citer :

- **Écosystème étendu :** Docker Hub héberge des millions d'images pré-construites, allant des bases de données aux serveurs web, ce qui élimine les procédures d'installation complexes.
- **Portabilité universelle :** Un conteneur créé sur macOS fonctionne de manière identique sur Linux ou Windows, à condition que Docker soit installé.
- **Priorité aux développeurs :** Résume les différences d'infrastructure afin que les développeurs puissent se concentrer sur leurs applications plutôt que sur les complexités du déploiement.
- **Intégration rapide :** Les nouveaux membres de l'équipe peuvent mettre en place des environnements de développement en quelques minutes plutôt qu'en plusieurs heures.

### Éléments clés et processus de travail

L'architecture de Docker se compose de plusieurs composants interconnectés qui fonctionnent ensemble de manière transparente.

L'interface CLI Docker fournit l'interface utilisateur à partir de laquelle les développeurs exécutent des commandes. Lorsque vous exécutez une commande telle que ` `docker run``, l'interface CLI communique avec le démon Docker (``dockerd``), qui sert de service central pour gérer les conteneurs, les images, les réseaux et les volumes.

Pour les utilisateurs Windows et macOS, Docker Desktop offre une couche supplémentaire de commodité grâce à une interface graphique, rendant la gestion des conteneurs accessible même à ceux qui sont moins à l'aise avec les outils en ligne de commande. Cet environnement intégré comprend tout ce qui est nécessaire au développement local, du support Kubernetes à la gestion des volumes.

Le flux de travail type suit un schéma clair :

1. Les développeurs créent des images à l'aide de fichiers Dockerfiles.
2. Ils les expédient vers des registres tels que Docker Hub.
3. Enfin, les images sont exécutées en tant que conteneurs sur n'importe quel hôte compatible avec Docker.

Ce que de nombreux développeurs ignorent, c'est que Docker n'exécute pas directement les conteneurs. `dockerd` e la délégation de l'exécution effective du conteneur à containerd, qui s'exécute en arrière-plan en tant que processus distinct.

Cette conception modulaire, dans laquelle Docker utilise containerd pour les opérations de bas niveau, permet à chaque composant de se concentrer sur ce qu'il fait le mieux : Docker offre une interface et un écosystème conviviaux pour les développeurs, tandis que containerd gère les détails techniques de l'exécution des conteneurs.

Que vous soyez novice dans l'utilisation de Docker ou que vous souhaitiez passer à l'étape suivante, nous vous invitons à consulter nos 10 idées de projets Docker adaptés à tous les niveaux.

## Qu'est-ce que Containerd ?

Après avoir examiné l'approche globale de la plateforme Docker, nous allons maintenant nous intéresser à containerd, le runtime spécialisé qui permet l'exécution des conteneurs à la fois dans Docker et dans l'écosystème plus large.

### Un environnement d'exécution de conteneurs conforme aux normes de l'industrie

Containerd est un moteur d'exécution de conteneurs léger, conforme aux normes de l'industrie, qui a été certifié par la Cloud Native Computing Foundation (CNCF), ce qui témoigne de sa maturité, de sa fiabilité et de son adoption généralisée. Initialement intégré à Docker, containerd a été extrait en 2017 et transféré à la CNCF afin de permettre une adoption plus large au sein de l'écosystème.

Contrairement à l'ensemble complet de fonctionnalités de Docker, la portée de containerd est délibérément limitée : il gère les opérations essentielles du cycle de vie des conteneurs telles que le démarrage, l'arrêt, la mise en pause et la suppression, et prend en charge le transfert et le stockage des images. Cette approche ciblée rend containerd exceptionnellement stable et efficace, des qualités essentielles pour les environnements de production.

Considérez containerd comme une « plomberie » : une infrastructure conçue pour être intégrée dans des systèmes plus vastes plutôt que d'être utilisée directement par les utilisateurs. Les principales plateformes telles que Kubernetes, AWS Fargate et Google Kubernetes Engine s'appuient toutes sur containerd pour exécuter des conteneurs, même si les utilisateurs interagissent avec ces plateformes via leurs propres interfaces.

### Architecture et design

Pour appréhender pourquoi containerd est si largement adopté dans les systèmes de production, il est nécessaire d'examiner ses principes architecturaux.

L'architecture de Containerd illustre parfaitement les principes de conception modulaire. Conçu selon les normes de l'Open Container Initiative (OCI), containerd garantit la compatibilité dans l'ensemble de l'écosystème des conteneurs. Cela signifie que toute image conforme à OCI fonctionnera avec containerd, quel que soit l'outil utilisé pour la créer.

L'architecture se compose de plusieurs couches principales :

- **Couche API gRPC :** Fournit un modèle client-serveur dans lequel plusieurs clients (Docker, Kubernetes, outils personnalisés) communiquent avec une seule instance containerd.
- **Démon Containerd :** Gère l'état des conteneurs en cours d'exécution, administre le stockage des images via des snapshotters et coordonne la mise en réseau via des plugins.
- **runc runtime :** Un environnement d'exécution léger et conforme à l'OCI qui s'interface directement avec les fonctionnalités du noyau Linux, créant des espaces de noms, configurant des cgroups et lançant des processus de conteneurs.
- **Plugins modulaires :** Des snapshotters personnalisés pour le stockage spécialisé, des environnements d'exécution alternatifs (gVisor, Kata Containers) et des plugins réseau peuvent être intégrés sans modification de containerd.

Lorsque containerd doit démarrer un conteneur, il lance runc, qui effectue le travail réel consistant à créer des espaces de noms isolés, à configurer des cgroups pour les limites de ressources et à lancer le processus du conteneur.

Cette séparation des préoccupations rend containerd hautement extensible. Les organisations peuvent personnaliser presque tous les aspects sans modifier le code conteneur principal.

## Containerd et Docker : principales différences

Maintenant que nous avons une bonne compréhension du fonctionnement de chaque outil, examinons leurs différences dans la pratique, de l'architecture aux modèles d'intégration.

### Exécution de conteneurs par rapport à la plateforme

La différence fondamentale réside dans la portée et l'objectif.

Docker utilise une architecture démon centralisée dans laquelle `dockerd` coordonne de nombreux aspects différents, tels que :

- Construction d'image
- Exécution de conteneurs
- Réseautage
- Gestion du volume

Cette conception tout-en-un simplifie l'expérience des développeurs, mais introduit une surcharge due à la multiplicité des couches d'abstraction.

Containerd, en revanche, se concentre exclusivement sur l'exécution des conteneurs. Il n'inclut pas de fonctionnalités de création d'images, d'orchestration ou d'interface graphique. Pour la mise en réseau, Docker intègre une interface réseau de conteneur ( `Libnetwork` ) dans son démon, tandis que containerd s'appuie sur des plugins externes d'interface réseau de conteneur (CNI) qui peuvent être remplacés en fonction des besoins.

Cette différence architecturale a des implications en termes de performances. Dans les environnements à forte rotation où les conteneurs démarrent et s'arrêtent fréquemment, tels que les clusters Kubernetes à dimensionnement automatique, la conception rationalisée de containerd peut se traduire par des temps de démarrage plus rapides et une consommation de ressources réduite. La réduction des frais généraux signifie que containerd utilise moins de mémoire et de CPU, ce qui devient significatif à grande échelle.

### Intégration Kubernetes

En ce qui concerne Kubernetes, la relation entre les environnements d'exécution de conteneurs et les plateformes d'orchestration a considérablement évolué, notamment en ce qui concerne la manière dont Kubernetes se connecte à containerd.

La relation entre Kubernetes et les environnements d'exécution de conteneurs a connu un changement majeur avec la version 1.24 de Kubernetes, publiée en 2022. Cette version a supprimé « Dockershim », une couche de compatibilité qui permettait à Kubernetes d'utiliser Docker comme environnement d'exécution de conteneurs.

Dockershim a toujours été conçu comme une solution temporaire. Lorsque Kubernetes a introduit l'interface CRI (Container Runtime Interface) afin de normaliser la communication avec les environnements d'exécution, Docker n'a pas pu l'implémenter directement, car Docker était antérieur à la conception de CRI. Dockershim assure la traduction entre Kubernetes et Docker, ajoutant ainsi une couche de traduction qui pourrait s'avérer superflue.

Les versions modernes de Kubernetes communiquent directement avec containerd via CRI, éliminant ainsi complètement la couche de traduction Docker. Cette simplification apporte des avantages concrets :

- **Réduction de la latence :** La communication directe élimine les frais de traduction dans les opérations de conteneurs.
- **Stabilité améliorée :** Moins de pièces mobiles signifie moins de points de défaillance potentiels.
- **Meilleures performances :** La pile d'exécution rationalisée est particulièrement avantageuse pour les déploiements à grande échelle.
- **Débogage simplifié :** L'intégration directe du CRI simplifie le dépannage.

Pour les utilisateurs de Kubernetes, ce changement est largement transparent. Les images Docker restent entièrement compatibles car elles respectent les normes OCI. Concrètement, cela signifie que les clusters Kubernetes de production fonctionnent désormais plus efficacement en utilisant directement containerd, tandis que les développeurs peuvent continuer à utiliser Docker localement pour la construction et les tests.

Si vous n'êtes pas certain des avantages et des inconvénients liés à l'utilisation de Kubernetes, veuillez consulter cette comparaison entre Docker Compose et Kubernetes.

### Construction et gestion de l'image

Bien que l'intégration à l'exécution soit essentielle pour l'orchestration, le flux de travail des développeurs dépend fortement de la manière dont chaque outil gère la création et le stockage des images. La création d'images représente un écart de capacité significatif entre Docker et containerd.

Docker fournit un système de compilation intégré via Dockerfiles et BuildKit, permettant aux développeurs de créer des compilations complexes en plusieurs étapes avec mise en cache, parallélisation et fonctionnalités avancées telles que les secrets de compilation et le transfert d'agent SSH.

Conformément à sa conception, Containerd n'inclut aucun workflow natif de création d'images. Pour créer des images avec containerd, les développeurs doivent utiliser des outils externes. Les options incluent l'exécution de BuildKit en tant que démon distinct et l'utilisation de `buildctl` pour les compilations en ligne de commande, ou l'adoption de `nerdctl`, une interface CLI compatible avec Docker qui intègre BuildKit.

Les mécanismes de stockage diffèrent également dans leur approche. La gestion des volumes de Docker offre une abstraction qui semble naturelle pour les développeurs, avec des volumes nommés qui conservent les données indépendamment du cycle de vie des conteneurs. Containerd utilise un système de capture d'écran de niveau inférieur, dans lequel différents pilotes de capture d'écran peuvent être connectés pour gérer différemment les systèmes de fichiers en couches en fonction des exigences de stockage sous-jacentes.

Cette différence reflète les publics visés par ces outils :

- Docker optimise la productivité des développeurs grâce à des fonctionnalités intégrées pratiques.
- Containerd fournit des primitives flexibles que les développeurs de plateformes peuvent assembler en fonction de leurs besoins spécifiques.

### CLI, expérience développeur et nerdctl

Au-delà de l'architecture et des fonctionnalités, l'expérience quotidienne des développeurs est directement influencée par l'interface de ligne de commande fournie par chaque outil. L'expérience de la ligne de commande met clairement en évidence les différentes philosophies de conception.

L'interface CLI de Docker est réputée pour sa convivialité. Les commandes telles que `docker run`, `docker build` et `docker logs` sont intuitives, bien documentées et conçues pour les utilisateurs. L'interface CLI comprend des paramètres par défaut utiles, des messages d'erreur clairs et des options étendues qui couvrent la plupart des cas d'utilisation.

Containerd est fourni avec `ctr`, une interface CLI minimale destinée exclusivement au débogage et au test des fonctionnalités de bas niveau de containerd. L'outil `ctr` est intentionnellement peu convivial pour les développeurs. Il manque des fonctionnalités courantes telles que les raccourcis de mappage de ports, les politiques de redémarrage automatique et l'intégration avec les assistants d'identification. Il est conçu pour les développeurs de conteneurs, et non pour les développeurs d'applications.

#### Combler le fossé avec nerdctl

Cette lacune en matière d'utilisabilité a conduit à la création d'`nerdctl`, une interface CLI compatible avec Docker pour containerd. L'utilisation d'`nerdctl` est similaire à celle de Docker : même syntaxe de commande, mêmes indicateurs, même flux de travail, mais avec containerd comme environnement d'exécution sous-jacent. Cela fait d'`nerdctl`, un excellent outil de transition pour les équipes qui passent de Docker à containerd dans leurs environnements de développement.

Dans la pratique, les développeurs interagissent rarement directement avec containerd. Lorsque cela s'avère nécessaire, `nerdctl` fournit l'interface familière à laquelle ils s'attendent, tandis que les opérateurs et administrateurs de plateformes utilisent les API de containerd de manière programmatique via des systèmes d'orchestration tels que Kubernetes.

Afin d'illustrer les différences dans la pratique, voici une comparaison des opérations courantes sur les conteneurs entre les trois outils CLI :

| **Tâche** | **Docker** | **nerdctl** | **ctr** | 
| Exécuter le conteneur | `docker run -d -p 8080:80 nginx` | `nerdctl run -d -p 8080:80 nginx` | `ctr run --net-host -d docker.io/library/nginx:latest nginx_id` | 
| Liste des conteneurs | `docker ps` | `nerdctl ps` | `ctr tasks list` | 
| Construire une image | `docker build -t myapp .` | `nerdctl build -t myapp .` | *Non pris en charge* | 
| Consulter les journaux | `docker logs`  | `nerdctl logs`  | *Non pris en charge* | 
| Veuillez inspecter le conteneur. | `docker inspect`  | `nerdctl inspect`  | `ctr containers info`  | 
| Veuillez cliquer sur l'image | `docker pull nginx` | `nerdctl pull nginx` | `ctr images pull docker.io/library/nginx:latest` | 
| Composer le soutien | `docker compose up` | `nerdctl compose up` | *Non pris en charge* | 

Remarque : ctr ne dispose pas de mappage de ports (`-p`) et nécessite une mise en réseau hôte (`--net-host`) pour exposer les services. Il ne télécharge pas automatiquement les images.

### Aperçu des principales différences

Avant d'aborder les recommandations pour des cas d'utilisation spécifiques, récapitulons les différences que nous avons abordées jusqu'à présent. Le tableau suivant résume les principales différences fonctionnelles entre Docker et containerd, en mettant en évidence leurs capacités distinctes et leurs publics cibles :

| **Caractéristique** | **Docker** | **Conteneur** | 
| Construction d'image | Intégré (Dockerfiles, BuildKit) | Nécessite des outils externes (buildctl, nerdctl) | 
| Orchestration | Docker Swarm / Kubernetes | Aucun (utilisé par Kubernetes) | 
| Gestion du stockage | Gestion du volume | Système de capture d'écran | 
| Interface graphique | Docker Desktop | Aucun | 
| Cycle de vie des conteneurs | Gestion complète (via containerd) | Objectif principal (compatible CRI) | 
| Utilisateurs principaux | Développeurs d'applications | Opérateurs de clusters, développeurs de plateformes | 

## Pourquoi choisir Docker ?

Maintenant que nous avons abordé les différences techniques, examinons des scénarios pratiques dans lesquels chaque outil excelle. Malgré l'essor des conteneurs dans les environnements de production, Docker demeure le choix privilégié pour des scénarios spécifiques où l'expérience des développeurs et la disponibilité d'outils complets sont primordiales.

### Pour le développement local et le prototypage

Docker est particulièrement efficace lorsque vous avez besoin d'une solution « tout-en-un » pour écrire et tester du code. Grâce à la chaîne d'outils intégrée, les développeurs peuvent passer de zéro à l'exécution de conteneurs en quelques minutes, sans avoir à assembler plusieurs composants ni à configurer un réseau complexe.

L'écosystème Docker offre d'importants avantages en termes de productivité :

- **Docker Hub :** Des millions d'images prêtes à l'emploi pour les bases de données, les files d'attente de messages et les serveurs Web.
- **Docker Compose:** Définissez des applications multi-conteneurs dans un seul fichier YAML et lancez des environnements de développement complets à l'aide d'une seule commande.
- **Interface graphique Docker Desktop :** Gestion visuelle des conteneurs, exploration des volumes, ajustements des limites de ressources et prise en charge intégrée de Kubernetes.
- **Réduction des obstacles à l'entrée :** Des outils graphiques et des commandes intuitives rendent les conteneurs accessibles aux développeurs novices dans cette technologie.

Pour les équipes utilisant Docker Desktop, l'interface graphique offre des fonctionnalités supplémentaires qui accélèrent considérablement l'intégration et les flux de travail quotidiens.

### Pour les pipelines de construction complexes

Au-delà du développement, les capacités de compilation de Docker en font un choix naturel pour les workflows sophistiqués d'intégration et de déploiement continus.

Le BuildKit intégré à Docker offre des fonctionnalités avancées indispensables aux pipelines CI/CD modernes. Les constructions en plusieurs étapes réduisent la taille des images tout en conservant la lisibilité des fichiers Dockerfile. Les mécanismes de mise en cache de BuildKit réutilisent intelligemment les couches d'une compilation à l'autre, ce qui réduit considérablement les temps de compilation dans les environnements d'intégration continue.

La plupart des plateformes d'automatisation, telles que GitHub Actions, GitLab CI, Jenkins et d'autres, disposent d'intégrations Docker éprouvées et matures. Ces intégrations gèrent l'authentification, la mise en cache et la publication d'images sans aucune difficulté.

Bien que d'autres outils puissent offrir des résultats similaires, l'omniprésence de Docker signifie que des solutions et une assistance pour le dépannage sont facilement accessibles, ce qui constitue un autre avantage considérable.

## Pourquoi choisir Containerd ?

Containerd se distingue dans les scénarios de production où le minimalisme, les performances et la stabilité priment sur la commodité des outils intégrés.

### Pour les clusters Kubernetes de production

L'utilisation de containerd comme environnement d'exécution pour les nœuds Kubernetes apporte des avantages significatifs :

- **Réduction des frais généraux :** La suppression du démon Docker permet de réduire la consommation de ressources par nœud et de réaliser des économies significatives à grande échelle.
- **Stabilité améliorée :** Moins de composants mobiles signifie moins de points de défaillance potentiels dans votre infrastructure.
- **Surface d'attaque réduite :** Moins de code à auditer et moins de vulnérabilités potentielles en matière de sécurité
- **Débogage simplifié :** L'intégration directe de CRI élimine la couche de traduction dockershim, ce qui simplifie le dépannage.
- **Meilleures performances :** La pile d'exécution optimisée améliore les temps de démarrage et la réactivité des conteneurs.

Le statut de graduation CNCF de Containerd témoigne de sa maturité et de sa fiabilité. Les principaux fournisseurs de services cloud, notamment AWS, Google Cloud et Azure, ont adopté containerd comme norme pour leurs offres Kubernetes gérées, démontrant ainsi leur confiance dans sa capacité à être utilisé en production pour les infrastructures critiques.

### Pour les environnements spécialisés et minimalistes

Bien que Kubernetes soit le cas d'utilisation le plus courant en production, la nature légère de containerd ouvre la voie à des scénarios de déploiement où Docker serait peu pratique.

L'informatique en périphérie et les appareils IoT fonctionnent souvent dans des conditions de ressources très limitées. La conception légère de Containerd le rend viable dans ces environnements où la pile complète de Docker serait prohibitive. Chaque mégaoctet de mémoire et chaque cycle CPU sont importants lors de l'exécution sur du matériel embarqué.

Les scénarios de sécurité avancés bénéficient de l'architecture d'exécution modulaire de containerd. Les organisations peuvent intégrer des environnements d'exécution sandboxés pour les charges de travail nécessitant des limites de sécurité supplémentaires. Voici quelques exemples :

- **gVisor : assure** une isolation efficace du noyau
- **Kata Containers :** exécute des conteneurs dans des machines virtuelles légères.

Ces intégrations s'intègrent à containerd sans nécessiter de modifications importantes.

## Transition de Docker vers Containerd

Il est important de comprendre quand utiliser chaque outil, mais il est tout aussi crucial de savoir comment passer de l'un à l'autre. La transition de Docker vers containerd dans les environnements existants nécessite une planification minutieuse, mais le processus est bien documenté et simple.

### Migration des nœuds Kubernetes

Les étapes opérationnelles pour migrer les nœuds Kubernetes de Docker vers containerd suivent un modèle standard :

1. 
**Veuillez isoler le nœud :** Empêcher la planification de nouveaux pods (`kubectl cordon` )
2. 
**Vider les pods existants :** Transférez les charges de travail vers d'autres nœuds (`kubectl drain` )
3. 
**Mettre à jour la configuration Kubelet :** Veuillez pointer vers le socket CRI de containerd à l'adresse suivante :`/run/containerd/containerd.sock`
4. 
**Vérifier les plugins CNI :** Veuillez vous assurer que les plugins réseau nécessaires sont installés sur containerd.
5. 
**Veuillez redémarrer Kubelet :** Veuillez vous enregistrer avec le nouveau runtime et rejoindre à nouveau le cluster.

Tester la migration sur des nœuds hors production permet d'identifier les problèmes spécifiques à l'environnement avant de déployer les modifications à l'échelle du cluster. Afin d'éviter les erreurs courantes, veuillez respecter les bonnes pratiques suivantes :

- **Modifications du chemin d'accès au journal :** Docker et containerd utilisent des emplacements de journaux par défaut différents. Veuillez mettre à jour votre infrastructure de journalisation en conséquence.
- **Veuillez installer les plugins CNI manquants :** Containerd nécessite les binaires du plugin CNI pour la mise en réseau ; ceux-ci ne sont pas toujours installés par défaut.
- **Veuillez prêter attention aux différences de tirage d'image :** Les paramètres d'authentification et de registre peuvent nécessiter des ajustements.
- **Veuillez prendre garde aux incompatibilités entre les pilotes de stockage :** Veuillez vous assurer que vos volumes persistants sont compatibles avec le snapshotter de containerd.

### Comprendre les différences entre les interfaces CLI

Une fois votre infrastructure migrée, les développeurs devront adapter leurs processus quotidiens afin de travailler avec le nouveau runtime.

Pour les développeurs habitués aux commandes Docker, nerdctl offre une expérience similaire. Les commandes telles que ` `nerdctl run``, ` `nerdctl build`` et ` `nerdctl compose up` ` fonctionnent exactement comme leurs équivalents Docker, ce qui rend la transition transparente.

Pour le débogage, il est essentiel de comprendre comment mapper les workflows de débogage Docker vers containerd. Si vous utilisez `docker inspect` pour examiner un conteneur, `ctr containers info` fournit des informations similaires, mais dans un format différent. De même, `ctr tasks list` affiche les conteneurs en cours d'exécution.

La plupart des développeurs constatent qu' `nerdctl` élimine la nécessité d'apprendre la syntaxe d' `ctr` pour les tâches quotidiennes. L' `ctr`, de bas niveau, reste utile pour le dépannage de problèmes spécifiques à l'exécution ou lorsque l'on travaille directement avec les API de containerd.

## Conclusion

La relation entre Docker et containerd illustre parfaitement la réussite de la conception modulaire dans l'infrastructure logicielle. Docker demeure l'outil optimal pour les développeurs qui écrivent du code, car il offre une expérience intégrée, un écosystème complet et des interfaces conviviales qui rendent la création d'applications conteneurisées productive et agréable.

Containerd, quant à lui, se distingue comme le runtime idéal pour les machines exécutant du code, offrant la stabilité, les performances et le minimalisme requis pour les plateformes d'orchestration de production. Le fait que Docker Engine utilise containerd en arrière-plan démontre que ces deux outils se complètent plutôt qu'ils ne se font concurrence.

Je recommande une approche pragmatique pour la plupart des organisations : continuer à utiliser Docker sur les ordinateurs portables des développeurs, où ses outils accélèrent les workflows de développement, mais envisager de migrer les clusters Kubernetes de production vers containerd directement pour bénéficier des avantages opérationnels que sont la réduction des frais généraux et la simplification des piles d'exécution.

Que vous optiez pour la plateforme complète de Docker ou pour le runtime spécialisé de containerd, ces deux solutions restent des composants essentiels de l'écosystème moderne des conteneurs, chacune étant optimisée pour différentes étapes du cycle de vie des applications.

Pour continuer à vous former, nous vous invitons à vous inscrire à notre cursus de compétences « Conteneurisation et virtualisation avec Docker et Kubernetes ».

## FAQ sur Containerd et Docker

### Est-il possible d'utiliser des images Docker avec containerd ?

**Oui, tout à fait. Containerd prend en charge toutes les images de conteneur conformes à la norme OCI, y compris celles créées avec Docker. Étant donné que Docker crée des images conformes à la norme OCI, celles-ci fonctionnent parfaitement avec containerd et tous les autres environnements d'exécution compatibles OCI. Vous pouvez utiliser `docker build` localement et exécuter ces images avec containerd en production sans aucun problème de compatibilité.**

### Docker utilise-t-il containerd en arrière-plan ?

**Oui, Docker Engine utilise containerd comme moteur d'exécution de conteneurs principal. À partir de la version 1.11, Docker a intégré containerd pour gérer les opérations liées au cycle de vie des conteneurs, telles que la création, l'exécution et la gestion. Lorsque vous exécutez `docker run`, le démon Docker (`dockerd`) délègue l'exécution effective du conteneur à containerd, qui utilise ensuite runc pour interagir avec le noyau Linux.**

### Pourquoi Kubernetes a-t-il supprimé la prise en charge de Docker ?

**En 2022, Kubernetes a supprimé dockershim (la couche de compatibilité Docker) afin d'éliminer une couche de traduction superflue. Docker est antérieur à l'interface d'exécution de conteneurs (CRI), donc Kubernetes avait besoin de dockershim pour assurer la conversion entre ses API et Docker. En communiquant directement avec containerd via CRI, Kubernetes atteint de meilleures performances, une plus grande stabilité et une pile d'exécution plus simple. Les images Docker continuent de fonctionner parfaitement dans Kubernetes.**

### Devrais-je passer de Docker à containerd pour le développement local ?

**Non, Docker demeure le choix le plus approprié pour le développement local. Docker fournit une chaîne d'outils intégrée avec Docker Compose, l'interface graphique de Docker Desktop et une prise en charge étendue de l'écosystème qui accélère les workflows de développement. Veuillez utiliser containerd pour les clusters Kubernetes de production, où sa faible surcharge et son intégration directe à CRI offrent des avantages évidents, mais conservez Docker sur les ordinateurs portables des développeurs pour bénéficier de son expérience utilisateur supérieure.**

### Qu'est-ce que nerdctl et en ai-je besoin ?

**Nerdctl est une interface CLI compatible avec Docker pour containerd qui offre la même expérience utilisateur que Docker (prend en charge la plupart des commandes et indicateurs courants) mais utilise containerd comme environnement d'exécution. Vous en avez besoin si vous souhaitez interagir directement avec containerd à l'aide des commandes Docker habituelles. Il est particulièrement utile pour les environnements de développement qui utilisent containerd ou lors de la transition des équipes de Docker vers des workflows basés sur containerd.**

En tant que fondateur de Martin Data Solutions et Data Scientist freelance, ingénieur ML et AI, j'apporte un portefeuille diversifié en régression, classification, NLP, LLM, RAG, réseaux neuronaux, méthodes d'ensemble et vision par ordinateur.

- A développé avec succès plusieurs projets de ML de bout en bout, y compris le nettoyage des données, l'analyse, la modélisation et le déploiement sur AWS et GCP, en fournissant des solutions impactantes et évolutives.
- Création d'applications web interactives et évolutives à l'aide de Streamlit et Gradio pour divers cas d'utilisation dans l'industrie.
- Enseigne et encadre des étudiants en science des données et en analyse, en favorisant leur développement professionnel par le biais d'approches d'apprentissage personnalisées.
- Conception du contenu des cours pour les applications de génération augmentée par récupération (RAG) adaptées aux exigences de l'entreprise.
- Rédaction de blogs techniques à fort impact sur l'IA et le ML, couvrant des sujets tels que les MLOps, les bases de données vectorielles et les LLM, avec un engagement significatif.

Dans chaque projet que je prends en charge, je m'assure d'appliquer des pratiques actualisées en matière d'ingénierie logicielle et de DevOps, comme le CI/CD, le linting de code, le formatage, la surveillance des modèles, le suivi des expériences et la gestion robuste des erreurs. Je m'engage à fournir des solutions complètes, en transformant les connaissances sur les données en stratégies pratiques qui aident les entreprises à se développer et à tirer le meilleur parti de la science des données, de l'apprentissage automatique et de l'IA.