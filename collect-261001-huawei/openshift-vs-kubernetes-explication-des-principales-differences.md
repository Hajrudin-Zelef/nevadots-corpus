Cours

Au cours des dernières années, j'ai eu l'occasion de travailler à la fois avec Kubernetes et OpenShift, et chacun a joué un rôle distinct dans mon parcours.

Tout en préparantma certification CKAD, j'ai surtoutly utilisé bare Kubernetes pour des projets personnels, ce qui m'a aidé à comprendre comment les choses fonctionnent sous le capot. En revanche, la majeure partie de mon expérience professionnelle en entreprise s'est faite avec OpenShift. Les équipes s'appuient sur cette solution pour exécuter des applications conteneurisées en production, grâce à sa sécurité intégrée, ses outils de développement et son support d'entreprise.

Dans cet article, je souhaite vous faire part de ce que j'ai appris en utilisant les deux. Quelles sont leurs similitudes, leurs différences et laquelle pourrait être la mieux adaptée à vos besoins. Que vous commenciez à utiliser des conteneurs ou que vous cherchiez à les faire évoluer dans un environnement de production, ce comparatif devrait vous aider à trouver la bonne direction.

## Aperçu de Kubernetes

Kubernetes est devenu la norme pour l'orchestration moderne des conteneurs (). Initialement développée par Google et désormais maintenue par la Cloud Native Computing Foundation (CNCF), c'est l'une des plateformes open-source les plus largement adoptées dans le monde cloud-native.

Avant de nous pencher sur la comparaison avec OpenShift, nous allons d'abord expliquer ce qu'est Kubernetes, ce qu'il fait bien et où il est généralement utilisé.

### Qu'est-ce que Kubernetes ?

Kubernetes (K8S) est une plateforme open-source conçue pour automatiser le déploiement, la mise à l'échelle et la gestion des applications conteneurisées.

Il est né de l'expérience de Google en matière de gestion de conteneurs en production et est basé sur leur système interne appelé Borg. Depuis qu'il est devenu un logiciel libre, il a développé un vaste écosystème et obtenu un soutien important de la part de la communauté.

À la base, Kubernetes vous aide à gérer des grappes de machines exécutant des conteneurs. Il fournit des API et un modèle déclaratif pour gérer l'ensemble du cycle de vie des charges de travail conteneurisées, de la programmation et de la mise à l'échelle à la mise en réseau et à la découverte de services.

Kubernetes est devenu la norme du secteur pour la gestion des applications basées sur les microservices dans les environnements cloud.

Vous pouvez en savoir plus sur Kubernetes dans lecours Introduction à Kubernetes ou en consultant le site Qu'est-ce que Kubernetes ? Une introduction avec des exemples.

Composants de base de Kubernetes. Image par Kubernetes.io

### Caractéristiques principales

Kubernetes offre un large éventail de fonctionnalités qui le rendent robuste et flexible :

- Orchestration de conteneurs: Automatise le déploiement, la mise à l'échelle et la gestion des conteneurs.
- Configuration déclarative: Définissez l'état souhaité de vos applications à l'aide de fichiers YAML ou JSON.
- Autocicatrisation: Redémarre automatiquement les conteneurs défaillants, remplace les nœuds morts et réorganise les charges de travail pour assurer un fonctionnement continu.
- Découverte des services et équilibrage de la charge: Achemine le trafic vers les bons pods à l'aide du DNS intégré et de l'équilibrage de charge.
- Mise à l'échelle horizontale: Augmentez ou réduisez la taille des applications en fonction de l'utilisation de l'unité centrale ou de paramètres personnalisés.
- Mises à jour et retours en arrière: Déployez de nouvelles versions de votre application avec un minimum de temps d'arrêt et revenez en arrière si nécessaire.
- Gestion des secrets et de la configuration: Gérez en toute sécurité les données sensibles, telles que les clés API et les variables d'environnement.

Ces fonctionnalités fournissent les éléments de base nécessaires à l'exécution d'applications hautement disponibles, évolutives et résilientes dans divers environnements, depuis les clusters de développement locaux jusqu'à l'infrastructure de production.

Si vous souhaitez acquérir une expérience pratique de Kubernetes, je vous recommandele tutoriel Kubernetes Tutorial : Guide du débutant pour le déploiement d'applications.

### Cas d'utilisation

Kubernetes convient parfaitement à de nombreuses applications modernes. Parmi les cas d'utilisation les plus courants, citons

- Architectures de microservices: Idéal pour gérer et mettre à l'échelle de manière indépendante des services faiblement couplés.
- Applications cloud-natives: Conçu pour tirer pleinement parti de l'élasticité du cloud et de l'infrastructure distribuée.
- Configurations multi-cloud ou hybrides-cloud: Exécutez vos applications sur plusieurs fournisseurs de cloud ou centres de données sur site avec le même ensemble d'outils.
- DevOps et pipelines CI/CD: Kubernetes permet l'automatisation et la flexibilité dans la construction, le test et le déploiement d'applications en continu.
- Plateformes d'apprentissage automatique et de science des données: Avec les outils tels que Kubeflow et MLflow, Kubernetesprend également en charge la formation de modèles distribués, les flux de travail reproductibles et le déploiement de modèles.

Vous voulez en savoir plus ? Consultezk Kubernetes Architecture Explained pour une analyse plus détaillée.

## Aperçu d'OpenShift

Si Kubernetes vous donne le moteur brut pour l'orchestration de conteneurs, OpenShift est plus comme un véhicule entièrement équipé construit autour de ce moteur.

OpenShift est la distribution Kubernetes de Red Hat, et bien qu'elle s'appuie directement sur Kubernetes, elle ajoute toute une couche de fonctionnalités visant à simplifier, sécuriser et rationaliser l'expérience des développeurs et des opérations.

OpenShift donne moins l'impression de tout configurer à partir de zéro, contrairement à l'utilisation de Kubernetes nu.

### Qu'est-ce qu'OpenShift ?

OpenShift est essentiellement Kubernetes avec des outils supplémentaires, des couches de sécurité et une meilleure expérience utilisateur. Il est maintenu et soutenu commercialement par Red Hat. Il est conçu pour les équipes qui souhaitent utiliser Kubernetes en production sans avoir à tout assembler elles-mêmes.

Bien que vous disposiez toujours de tous les composants de base de Kubernetes (tels que le serveur API, le planificateur et les kubelets), OpenShift ajoute des fonctionnalités supplémentaires, notamment un registre de conteneurs intégré, une console d'administration basée sur le web, des outils de développement et des valeurs par défaut de sécurité plus strictes.

Il est disponible dans plusieurs modèles de déploiement, allant des services cloud entièrement gérés aux plateformes auto-hébergées dans votre centre de données.

### Principaux composants et caractéristiques

Voici quelques-uns des outils et fonctionnalités les plus remarquables qui font d'OpenShift bien plus qu'un simple Kubernetes :

- CI/CD intégré: OpenShift inclut un support natif pour les pipelines Tekton et les flux de travail GitOps.
- Console Web: Une interface web propre et conviviale pour les développeurs et les administrateurs de clusters. Il rend la gestion des déploiements, la surveillance des ressources et l'affichage des journaux beaucoup plus accessibles.
- Améliorations de la sécurité: Des éléments tels que les contraintes de contexte de sécurité (SCC), l'analyse d'images et le RBAC intégré sont plus stricts et plus standardisés que Kubernetes vanille.
- Red Hat Enterprise Linux CoreOS (RHCOS): Un système d'exploitation spécialement conçu pour fonctionner sur les nœuds OpenShift, optimisé pour les charges de travail des conteneurs.
- OperatorHub: Une place de marché curatée pour installer et gérer les opérateurs Kubernetes, qui automatisent les tâches du cycle de vie des applications.

Ces ajouts rendent OpenShift particulièrement intéressant pour les grandes équipes ou les organisations qui ont besoin de plus qu'une plateforme Kubernetes de base.

### Saveurs OpenShift

Selon la façon dont vous souhaitez déployer et gérer OpenShift, il existe quelques saveurs différentes :

- OpenShift Container Platform: La version autogérée que vous pouvez exécuter sur votre infrastructure, qui est populaire dans les configurations sur site et hybrides.
- OpenShift Online: Un service OpenShift entièrement hébergé et géré par Red Hat, idéal pour démarrer sans rien mettre en place soi-même.
- OpenShift Dedicated: Une offre OpenShift gérée, hébergée sur des fournisseurs de cloud public comme AWS et Google Cloud, mais gérée par Red Hat en votre nom.

Chacun d'entre eux répond à des besoins différents, qu'il s'agisse de startups souhaitant être rapidement opérationnelles ou d'entreprises ayant des exigences strictes en matière de sécurité et de conformité.

## Comparaison des architectures

Si OpenShift et Kubernetes partagent une base commune, leurs approches architecturales divergent dès que l'on dépasse les bases.

Kubernetes offre un modèle modulaire, à construire soi-même, tandis qu'OpenShift fournit une pile plus intégrée, fondée sur des opinions. Il est essentiel de comprendre en quoi les deux plateformes diffèrent sous le capot pour savoir laquelle conviendra le mieux à votre équipe ou à votre organisation.

### Plate-forme de base

À la base, les deux plateformes exécutent Kubernetes. OpenShift est construit directement au-dessus de Kubernetes et s'en tient à ses API et composants en amont. Un cluster OpenShift standard comprend tous les éléments Kubernetes essentiels, comme le serveur API, etcd, le planificateur et les kubelets. Cela signifie qu'il est entièrement compatible avec les charges de travail et les outils Kubernetes.

Ce qui distingue OpenShift, c'est l'écosystème que Red Hat construit autour de cette base. Il introduit des fonctionnalités telles que Red Hat Enterprise Linux CoreOS (RHOCS) en tant que système d'exploitation du nœud, un registre de conteneurs intégré et des services de plateforme supplémentaires étroitement couplés à Kubernetes.

Ces intégrations réduisent la quantité de travail que les équipes doivent effectuer pour créer et sécuriser un environnement de production.

Aperçu de l'architecture de la plateforme de conteneurs OpenShift. Image fournie par la documentation de Red Hat.

### Installation et configuration

L'une des différences architecturales les plus importantes est la méthode d'installation de chaque plateforme.

- Kubernetes offre une grande souplesse de déploiement. Vous pouvez mettre en place un cluster local à l'aide d'outils comme minikube, de services gérés dans le cloud comme GKE ou EKS, ou l'installer manuellement à l'aide d'outils comme kubeadm. Bien que cela vous donne un contrôle total, cela signifie également que vous êtes responsable de tout, y compris de la mise en place du réseau, de la configuration du stockage et de l'installation d'outils tels que la surveillance et l'enregistrement.
- OpenShift, en revanche, offre un processus d'installation plus guidé et automatisé. OpenShift propose plusieurs méthodes d'installation pour s'adapter aux différents environnements d'infrastructure et aux préférences des utilisateurs.
- La première est l'infrastructure fournie par l'installateur, qui automatise la création de grappes sur les plates-formes prises en charge.
- L'autre est l'infrastructure fournie par l'utilisateur, qui donne plus de contrôle aux équipes d'infrastructure.
- Vous pouvez en savoir plus sur l'installation d'OpenShift dans la documentation utilisateur officielle.

### Outils de déploiement et de gestion

Kubernetes et OpenShift fournissent tous deux des outils en ligne de commande pour interagir avec le cluster, mais ils diffèrent en termes de portée et de facilité d'utilisation :

- Kubernetes s'appuie sur `kubectl` , un outil CLI puissant qui interagit directement avec l'API Kubernetes. Il est flexible et largement utilisé, mais suppose un certain niveau de familiarité avec YAML et l'architecture sous-jacente.
- OpenShift inclut `oc` , un CLI qui étend`kubectl` et possède des capacités supplémentaires. Il prend en charge les mêmes commandes de base et ajoute des fonctionnalités telles que l'accès par projet, la gestion des flux d'images et des flux de connexion simplifiés.

OpenShift comprend également une console web complète qui facilite la gestion des applications, le suivi des charges de travail et le contrôle des accès, sans qu'il soit nécessaire d'écrire YAML à la main.

Kubernetes propose également un tableau de bord, mais sa configuration et sa sécurisation nécessitent plus de travail.

Les deux plates-formes prennent en charge Helm pour manager les paquets d'applications. OpenShift s'intègre également étroitement avec les opérateurs, disponibles via l'OperatorHub intégré.

## Sécurité et gouvernance

La sécurité est souvent un facteur de différenciation clé entre Kubernetes et OpenShift, en particulier pour les équipes opérant dans des environnements réglementés ou les entreprises ayant des exigences de conformité strictes.

Comprendre comment les deux plateformes gèrent la sécurité, le contrôle d'accès et la conformité peut vous aider à décider laquelle est la mieux adaptée au profil de risque et aux besoins de gouvernance de votre organisation.

### Politiques de sécurité par défaut

Alors que Kubernetes fournit un cadre de sécurité flexible, OpenShift s'appuie sur lui avec une approche plus stricte et plus sécurisée par défaut.

- Kubernetes offre beaucoup de flexibilité en matière de sécurité, mais il vous faut surtout tout construire à partir de zéro. Kubernetes n'applique pas de règles de sécurité strictes au niveau de l'exécution. Il fournit des fonctionnalités telles que PodSecurity Admission (PSA) et Security Contexts, qui doivent être explicitement configurées et gérées.

- OpenShift est plus proactif en matière de sécurité. Il est livré avec les contraintes de contexte de sécurité (SCC) activées par défaut. Ils définissent ce qu'un pod peut et ne peut pas faire, par exemple s'il peut fonctionner en tant que root ou utiliser le réseau de l'hôte. En appliquant ces contraintes, OpenShift permet de réduire le risque de mauvaise configuration et d'escalade des privilèges, ce qui est particulièrement important dans les clusters multi-tenants.

En pratique, les charges de travail qui fonctionnent bien sur Kubernetes pourraient échouer sur OpenShift jusqu'à ce qu'elles répondent à ses normes de sécurité plus strictes. Cela peut sembler un obstacle à première vue, mais cela oblige les équipes à adopter les meilleures pratiques dès le début du cycle de développement.

### Contrôle d'accès basé sur les rôles (RBAC)

Kubernetes et OpenShift prennent tous deux en charge le RBAC, ce qui vous permet de contrôler qui a accès à quoi au sein du cluster.

- Kubernetes fournit un système RBAC hautement configurable, vous permettant de définir des rôles et de les attribuer à des utilisateurs ou à des comptes de service. Cependant, c'est à l'administrateur de la grappe de définir les rôles correctement, et il n'existe pas de structure imposée sur la manière dont l'accès doit être défini.
- OpenShift s'appuie sur le modèle RBAC de Kubernetes, mais inclut des rôles prédéfinis et un modèle d'accès basé sur les projets qui facilitent la gestion cohérente des accès. Chaque projet OpenShift (à peu près équivalent à un espace de noms Kubernetes) est accompagné de son propre ensemble de permissions, ce qui aide les équipes à isoler les charges de travail et à gérer l'accès de manière plus intuitive.

OpenShift est donc mieux adapté aux équipes qui souhaitent mettre en place un contrôle d'accès sans partir de zéro.

### Conformité et surveillance intégrées

- Dans Kubernetes, la surveillance et la conformité sont généralement des modules complémentaires. Vous pouvez installer des outils tels que Prometheus, Grafana, Falco ou des systèmes de journalisation d'audit. Cependant, ces derniers nécessitent une configuration et une maintenance manuelles. Cela vous donne de la flexibilité mais augmente les frais généraux.

- OpenShift comprend une pile de surveillance intégrée (Prometheus, Alertmanager et Grafana), une journalisation centralisée et des outils tels que le Compliance Operator. Ils sont intégrés à la plateforme et pris en charge dès le départ, ce qui permet aux équipes de savoir ce qui est en cours d'exécution et si cela correspond aux normes de sécurité.

Pour les équipes travaillant dans des secteurs réglementés, tels que la finance, la santé et l'administration, cela permet de gagner beaucoup de temps et d'efforts et de réduire le risque de passer à côté de quelque chose de crucial.

## Développeur et expérience utilisateur

Une part importante du choix entre Kubernetes et OpenShift se résume à la sensation qu'ils procurent à l'utilisation.

Alors que Kubernetes fournit des blocs de construction puissants, OpenShift ajoute une couche raffinée qui simplifie les flux de travail et améliore la productivité.

Cette section examine le processus de développement, de déploiement et de gestion des applications sur chaque plateforme.

### Interface et facilité d'utilisation

- Kubernetes est principalement piloté par la ligne de commande, avec `kubectl` comme interface principale. Il propose également un tableau de bord en ligne, qui est relativement basique et souvent désactivé par défaut pour des raisons de sécurité. La plupart des utilisateurs interagissent avec Kubernetes par le biais de fichiers de configuration et de commandes CLI. Cela vous donne beaucoup de contrôle, mais s'accompagne également d'une courbe d'apprentissage abrupte, en particulier pour les nouveaux arrivants.
- OpenShift, en revanche, comprend une console web complète conçue à la fois pour les développeurs et les administrateurs de clusters. Vous pouvez créer et gérer des projets, déployer des applications à partir de dépôts Git ou d'images de conteneurs, afficher des journaux et des mesures, et même déclencher des constructions sans toucher à la ligne de commande. L'interface utilisateur est bien intégrée et conviviale, ce qui abaisse la barrière d'entrée pour les équipes qui ne disposent pas d'une expertise approfondie de Kubernetes.

J'aime travailler avec l'interface utilisateur d'OpenShift, car elle vous permet de gérer vos applications facilement et rapidement. En particulier, le débogage est devenu beaucoup plus rapide grâce à l'utilisation de l'interface utilisateur au lieu de s'appuyer entièrement sur les commandes de l'interface de programmation.

Je vous recommande de lire What's New in the OpenShift 4.4 Web Console Developer Experience pour ensavoir plus sur les fonctionnalités et la conception de la console web d'OpenShift.

OpenShift prend également en charge son propre outil CLI (`oc`), qui étend `kubectl` et fournit des commandes supplémentaires adaptées aux fonctionnalités d'OpenShift. Les utilisateurs plus expérimentés disposent ainsi du même niveau de contrôle que celui qu'ils attendent de Kubernetes.

### Intégration des outils DevOps

- Kubernetes offre une intégration avec des outils DevOpstels que Jenkins, Argo CD, Tekton et Flux. Mais leur mise en place implique généralement un travail supplémentaire. Vous devez les installer et les configurer manuellement, gérer les identifiants et construire vos propres pipelines CI/CD.
- OpenShift simplifie cela en incluant OpenShift Pipelines (basé sur Tekton) et OpenShift GitOps (basé sur Argo CD) comme composants natifs. Ceux-ci sont étroitement intégrés à la plateforme, de sorte que vous pouvez construire, tester et déployer directement à partir de l'interface utilisateur ou du CLI sans dépendre de plugins externes. OpenShift prend également en charge les configurations de construction, les flux d'images et les déploiements automatiques déclenchés par Git ou les changements d'image. Ces fonctionnalités facilitent la livraison en continu.

### Ecosystème et extensibilité

- Kubernetes dispose d'un écosystème massif. Vous trouverez des milliers d'outils open-source, des graphiques Helm et des modules complémentaires pris en charge par la communauté pour tout ce qui concerne la surveillance et le traçage, les maillages de services et les charges de travail d'intelligence artificielle. Il est flexible et extensible, mais cette flexibilité signifie que vous devez prendre plus de décisions et maintenir plus de composants vous-même.
- OpenShift prend également en charge Helm, les CRD et les extensions natives de Kubernetes, mais gère son écosystème par l'intermédiaire d' OperatorHub. OperatorHubfournit un catalogue centralisé, soutenu par Red Hat, qui facilite la recherche et le déploiement de composants prêts pour la production.

En bref, Kubernetes vous donne les éléments de base pour assembler votre plateforme. OpenShift vous en donne plus dès le départ, avec des outils déjà intégrés et pris en charge, ce qui permet de gagner du temps et de réduire la complexité.

## Soutien et écosystème

Le succès à long terme d'une plateforme dépend souvent non seulement de ses fonctionnalités, mais aussi de son écosystème et du type de soutien qu'elle offre. Ceci est particulièrement important lorsque vous travaillez dans une grande entreprise, car vous avez besoin d'une assistance complète pour votre environnement de production.

Kubernetes et OpenShift disposent tous deux de communautés florissantes et du soutien des fournisseurs, mais ils adoptent des approches très différentes en matière de gouvernance, de responsabilité des fournisseurs et de préparation des entreprises.

### Soutien de la communauté et des fournisseurs

- Kubernetes est régi par la Cloud Native Computing Foundation (CNCF) et maintenu par une grande communauté mondiale de contributeurs. Il bénéficie d'une large participation des principaux fournisseurs de cloud, des entreprises technologiques et des développeurs indépendants. Ce modèle décentralisé permet à Kubernetes d'évoluer rapidement et de rester neutre vis-à-vis des fournisseurs, mais il signifie également qu'il n'y a pas de point unique de responsabilité commerciale. L'assistance dépend de votre fournisseur, de votre prestataire de services gérés ou de votre équipe DevOps interne.
- OpenShift, en revanche, est développé et pris en charge par Red Hat. Bien qu'il soit basé sur Kubernetes, il est accompagné d'un support commercial, y compris des accords de niveau de service (SLA), des intégrations certifiées et des conseils d'experts. Red Hat devient le partenaire de choix si votre équipe a besoin d'un support prévisible, de mises à jour régulières et d'une fiabilité de niveau entreprise.

Cette différence peut s'avérer cruciale pour les équipes travaillant dans des environnements de production ou réglementés, où la responsabilité du fournisseur et le soutien à long terme ne sont pas négociables.

### Soutien aux fournisseurs de cloud

Les deux plateformes sont bien prises en charge par les principaux fournisseurs de cloud, mais elles répondent à des cas d'utilisation différents.

Kubernetes est la base de tous les services Kubernetes primaires gérés :

- GKE (Google Kubernetes Engine)
- EKS (Elastic Kubernetes Service sur AWS)
- AKS (Azure Kubernetes Service)

Ces services permettent de s'affranchir d'une grande partie de la charge opérationnelle et sont parfaits pour les équipes qui souhaitent rester proches de Kubernetes en amont.

OpenShift est également disponible en tant que service géré :

- Red Hat OpenShift Service sur AWS (ROSA)
- Microsoft Azure Red Hat OpenShift (ARO)
- OpenShift sur IBM Cloud

Red Hat propose également OpenShift Dedicated, un environnement OpenShift entièrement géré, hébergé sur le cloud public mais exploité par Red Hat.

### Préparation de l'entreprise

- Kubernetes peut être utilisé efficacement dans les environnements d'entreprise, mais il nécessite souvent l'intégration d'outils tiers pour la surveillance, la sécurité, la conformité et l'automatisation. Les équipes doivent construire, configurer et maintenir ces intégrations, une tâche avec laquelle les grandes organisations dotées d'équipes DevOps expérimentées peuvent être plus à l'aise.
- OpenShift a été conçu pour les entreprises. Il comprend une surveillance et une journalisation intégrées, des politiques de sécurité renforcées, une authentification centralisée (via LDAP, OAuth et SSO) et une suite d'outils validés pour une utilisation en production. Vous bénéficiez également d'opérateurs certifiés, de versions de support à long terme et d'un chemin de mise à niveau clair, le tout soutenu par les accords de niveau de service (SLA) de Red Hat.

OpenShift convient mieux aux entreprises qui ont besoin d'une plateforme stable et prise en charge dès le départ.

## Coût et licence

Lorsque vous évaluez Kubernetes et OpenShift, le coût n'est pas seulement une question de licences logicielles. Il s'agit d'une prise en charge totale, y compris l'infrastructure, l'outillage, l'assistance et l'expertise interne.

Les deux plateformes ont des modèles très différents, et leur compréhension peut vous aider à planifier de manière réaliste l'adoption à court et à long terme.

### Considérations sur les coûts de Kubernetes

L'un des principaux avantages de Kubernetes est qu'il est open-source et gratuit. Vous pouvez le télécharger, l'exécuter n'importe où et l'utiliser sans payer de licence. Cela le rend intéressant pour les startups, les projets de loisir ou les équipes qui veulent un contrôle total sans être dépendantes d'un fournisseur.

Mais "gratuit" ne signifie pas qu'il n'y a pas de coûts. L'exécution de Kubernetes en production implique généralement :

- Coûts d'infrastructure: Que ce soit sur site ou dans le cloud, vous devrez gérer les ressources de calcul, de stockage et de mise en réseau.
- Frais généraux opérationnels: La mise en place de la surveillance, de la journalisation, du RBAC, des pipelines CI/CD et de la sécurité nécessite du temps et de l'expertise.
- Coûts de soutien: En cas de panne, il n'y a pas de support officiel, sauf si vous utilisez un service Kubernetes géré ou si vous payez un fournisseur tiers.

Ainsi, bien que Kubernetes n'ait pas de frais de licence, les coûts humains et d'outillage peuvent être importants, en particulier lorsque votre cluster évolue.

### Modèle de tarification d'OpenShift

OpenShift adopte une approche plus traditionnelle basée sur l'abonnement. Vous payez pour une licence Red Hat, qui vous donne accès à :

- La plateforme de conteneurs OpenShift ou des services gérés dans le cloud (comme ROSA ou ARO).
- Outils intégrés de CI/CD, d'observabilité et de sécurité.
- Accès au support client et à l'écosystème certifié de Red Hat.
- Mises à jour régulières, correctifs et versions de support à long terme (LTS).

Les prix dépendent de facteurs tels que le nombre de nœuds, les cœurs de CPU et le modèle de déploiement (autogéré ou géré). Néanmoins, il est conçu pour les entreprises qui ont besoin d'une solution packagée, prête à la production et bénéficiant d'un support officiel.

Le compromis est simple : vous payez plus cher au départ avec OpenShift, mais vous bénéficiez d'une expérience intégrée et de moins d'efforts opérationnels. Avec Kubernetes, vous économisez sur les licences, mais vous investissez plus de temps et de ressources dans la construction et la maintenance de la pile vous-même.

## Quand choisir ?

Maintenant que nous avons décomposé les caractéristiques, les architectures et les compromis, la grande question à laquelle il faut répondre est la suivante : quelle plateforme devriez-vous utiliser et quand ?

La réponse dépend de vos objectifs, de l'expérience de votre équipe et de vos besoins opérationnels.

### Scénarios les mieux adaptés à Kubernetes

Kubernetes brille dans les configurations où la flexibilité, la personnalisation et l'ouverture des outils sont des priorités absolues.

C'est une solution idéale lorsque :

- Vous avez une équipe DevOps expérimentée et souhaitez avoir un contrôle total sur votre stack.
- Vous créez des applications cloud-natives avec un état d'esprit Do It Yourself (bricolage).
- Vous pouvez gérer votre soutien
- Vous voulez réduire les coûts
- Vous voulez éviter le verrouillage des fournisseurs
- Vous travaillez sur des projets secondaires, des outils internes ou des produits en phase de démarrage.

En bref, si votre équipe apprécie l'autonomie, a le temps de maintenir la plateforme et préfère choisir et configurer chaque pièce du puzzle, Kubernetes vous donne ce pouvoir.

### Scénarios les mieux adaptés à OpenShift

OpenShift est spécialement conçu pour les organisations qui ont besoin d'une plateforme sécurisée, prise en charge et de qualité professionnelle.

Elle est la plus logique lorsque :

- Vous travaillez dans un secteur réglementé (par exemple, finance, soins de santé, gouvernement) avec des exigences strictes en matière de conformité.
- Votre équipe souhaite une solution prête à l'emploi avec CI/CD, surveillance et RBAC intégrés.
- Vous avez besoin d'une assistance officielle et d'une stabilité à long terme de la part d'un fournisseur comme Red Hat.
- Vos développeurs bénéficieraient d'une interface utilisateur conviviale et de flux de travail en libre-service.
- Vous cherchez à réduire les coûts opérationnels et à accélérer les processus de livraison.

### Facteurs clés de décision

Voici quelques considérations finales pour vous aider à faire votre choix :

- Budget: Kubernetes n'a pas de frais de licence, mais ses coûts opérationnels internes sont plus élevés. OpenShift nécessite un abonnement mais offre davantage de services intégrés.
- Expérience en équipe: Kubernetes nécessite plus d'expérience pratique en matière de DevOps. OpenShift permet d'abstraire une grande partie de cette complexité.
- Délai de production: OpenShift permet souvent aux équipes d'être prêtes pour la production plus rapidement.
- Sécurité et conformité: Les valeurs par défaut d'OpenShift sont plus strictes et mieux adaptées aux environnements d'entreprise ou réglementés.

## Conclusion

Kubernetes et OpenShift ne sont pas des concurrents, mais font partie du même écosystème, chacun répondant à des besoins et des profils d'utilisateurs différents. Kubernetes vous offre une flexibilité et un contrôle inégalés. OpenShift s'appuie sur cette base et offre une expérience plus intégrée, sécurisée et adaptée aux entreprises.

Dans le cadre de mon travail, j'ai pu constater à quel point Kubernetes nu peut être un excellent environnement d'apprentissage et un outil puissant pour les flux de travail personnalisés et cloud-natifs, en particulier lorsque vous souhaitez rester proche de la communauté open-source. Cependant, dans les entreprises où les équipes ont besoin de conformité, de support et de flux de travail cohérents entre plusieurs équipes, OpenShift s'est avéré être le choix le plus pratique.

Le choix de la bonne plateforme ne dépend pas uniquement des caractéristiques techniques. Il s'agit de l'expérience de votre équipe, des objectifs de votre entreprise et du temps que vous êtes prêt à consacrer à la construction par rapport à l'adoption.

Les deux plates-formes sont là pour durer, et quel que soit votre choix, vous construirez sur des bases solides et pérennes !

- Si vous souhaitez en savoir plus sur la certification CKAD et comment l'obtenir, je vous recommande de lire l'article Kubernetes Certification Guide : Examens, conseils et ressources d'étude.
- Si vous souhaitez vous préparer à un entretien d'embauche Kubernetes, je vous recommande de lire les 44 meilleures questions et réponses d'entretien Kubernetes en 2025.

## Maîtriser Docker et Kubernetes

## FAQ

### Quelle est la principale différence entre OpenShift et Kubernetes ?

**Kubernetes est une plateforme d'orchestration de conteneurs open-source, tandis qu'OpenShift est une distribution de Kubernetes qui inclut des fonctionnalités d'entreprise supplémentaires et la prise en charge de Red Hat.**

### OpenShift est-il construit au-dessus de Kubernetes ?

**Oui, OpenShift est construit sur Kubernetes et l'étend avec des outils de développement, des améliorations de sécurité et des pipelines intégrés d'intégration continue/déploiement continu (CI/CD).**

### Dois-je connaître Kubernetes avant d'utiliser OpenShift ?

**Des connaissances de base sur Kubernetes sont utiles, mais OpenShift fait abstraction d'une partie de sa complexité grâce à une interface conviviale et à l'automatisation.**

### Puis-je migrer de Kubernetes vers OpenShift ?

**Oui. OpenShift étant basé sur Kubernetes, la migration est possible moyennant quelques ajustements pour s'adapter aux politiques et outils d'OpenShift.**

### En quoi la sécurité diffère-t-elle entre OpenShift et Kubernetes ?

**OpenShift applique une sécurité plus stricte par défaut, avec des contraintes de contexte de sécurité (SCC), une surveillance intégrée et des outils de conformité.**

Je suis un ingénieur cloud avec de solides bases en génie électrique, en apprentissage automatique et en programmation. J'ai commencé ma carrière dans le domaine de la vision par ordinateur, en me concentrant sur la classification des images, avant de passer aux MLOps et aux DataOps. Je suis spécialisé dans la construction de plateformes MLOps, le soutien aux data scientists et la fourniture de solutions basées sur Kubernetes pour rationaliser les flux de travail d'apprentissage automatique.