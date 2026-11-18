En juin 2020, Hewlett Packard Enterprise annonçait le lancement de HPE GreenLake Cloud Services afin de répondre aux besoins des entreprises hybrides et multicloud. Cette année, l'offre s'est encore enrichie avec HPE GreenLake for Private Cloud Business Edition. Offrant une flexibilité et des options encore plus grandes, HPE GreenLake for Private Cloud Business Edition permet aux entreprises de bénéficier d'une expérience hautement personnalisée et efficace pour choisir l'infrastructure et la capacité les mieux adaptées à leurs besoins.
Cette expérience et cette offre uniques rendent HPE GreenLake plus attrayante en tant qu'option d'infrastructure cloud hybride que les offres cloud plus omniprésentes, telles qu'Amazon Web Services et Azure. Il est spécialement conçu pour les entreprises recherchant l'agilité des modèles de consommation cloud combinée au contrôle de l'infrastructure sur site. Il offre un déploiement simplifié avec des configurations prédéfinies, permettant une configuration plus rapide et une complexité réduite. Grâce à son optimisation pour des charges de travail spécifiques, les entreprises peuvent garantir l'efficacité des performances sans les frais généraux liés à la gestion d'une vaste gamme de services souvent présents dans les grands cloud publics. De plus, pour les entreprises soucieuses de la souveraineté des données, de la conformité réglementaire ou de mesures de sécurité spécifiques, l'édition Business offre l'avantage de la localisation des données et un contrôle accru sur leur environnement.
En bref, HPE GreenLake for Private Cloud Business Edition est une solution de cloud privé autogérée avec une interface unifiée pour simplifier la gestion des VM vers l'infrastructure. Il vous permet de créer votre cloud en libre-service à la demande là où vous en avez besoin, avec un choix de facturation mensuelle prévisible ou de paiement initial.
Infrastructure
Pour démarrer avec HPE GreenLake Private Cloud Business Edition, les utilisateurs doivent d'abord provisionner le matériel pour créer leur cloud privé. Pour les sites sur site, HPE utilise principalement une infrastructure cloud native composée de l'infrastructure hyperconvergée désagrégée HPE Alletra (dHCI) pour les charges de travail critiques pour l'entreprise ou de HPE SimpliVity (véritable HCI) pour les sites de périphérie distribués. Il fournit aux entreprises un package de cloud privé complet, fidèle au concept de flexibilité de HPE GreenLake pour les organisations ayant des charges de travail variables.
HPE s'est associé à Amazon Web Services pour compléter son offre sur site afin d'utiliser ses instances EC2 en tant que segment de cloud public. La combinaison de ces deux solutions permet à HPE GreenLake de présenter un cloud hybride unifié aux entreprises à la recherche de la solution adaptée à leurs besoins.
Une fois que vous avez provisionné le matériel et préparé la configuration cloud via Amazon Web Services, vous pouvez commencer à provisionner des machines virtuelles, des politiques, des sauvegardes, des contrôles de santé et toute autre myriade de services proposés par HPE GreenLake.
SimpliVité HPE
Pour un rappel rapide sur HPE SimpliVity et une mise à jour de ses capacités, il s'agit d'une véritable appliance d'infrastructure hyperconvergée (HCI) qui combine toutes les fonctionnalités et services principaux nécessaires aux fonctionnalités du serveur (comme l'informatique, la mise en réseau et le stockage) dans une seule grande boîte. Il améliore considérablement l’efficacité et la sécurité des données ainsi que la résilience intégrée.
Auparavant, il était accessible et géré via un portail centralisé. Avec l'introduction de HPE GreenLake Private Cloud Business Enterprise, HPE SimpliVity peut être géré au sein de la plateforme HPE GreenLake.
Dans HPE GreenLake, l'accessibilité d'un système HPE SimpliVity par rapport à un système HPE Alletra est affichée dans une vue commune. Il se trouve dans le portail Systèmes et n'est délimité que par le type de système.
Politiques de provisionnement des machines virtuelles
Les politiques de provisionnement des machines virtuelles sont essentielles à la réussite d’une stratégie globale de déploiement hybride. Pour accéder au portail VM Provisioning Policy, sélectionnez le bouton de menu des barres horizontales en haut à gauche pour ouvrir la liste des options d'accès aux portails. À partir de là, choisissez Politiques de provisionnement de VM, ce qui ouvrira la page principale de toutes les politiques actuellement disponibles dans votre environnement.
Pour créer un nouveau profil de provisionnement de VM, cliquez sur l'icône « + » en haut à gauche et une fenêtre s'ouvrira pour vous permettre de commencer à remplir les informations requises. Vous devrez nommer le profil et fournir une description de base. Sélectionnez le bouton radio approprié pour Déduplication, Chiffrement des données ou All-Flash si ce sont les options dont vous avez besoin. En passant votre curseur sur l'icône d'information à droite de chaque option, vous obtenez des informations supplémentaires sur l'utilisation de ces options. Enfin, vous pouvez laisser les paramètres de performances QoS par défaut, soit la limite d'un million d'IOPS, ou les ajuster en fonction de l'environnement.
Surveillance des machines virtuelles
Une fois que vous avez créé une stratégie de provisionnement, revenez à la page Machines virtuelles pour créer les instances souhaitées.
Dans le menu supérieur, vous pouvez afficher vos VM de cloud privé (physiques) ou les VM de cloud public (AWS).
La page principale vous donne un bref aperçu des machines virtuelles de votre environnement et d'autres données fondamentales les concernant, y compris les détails de leur état et de la protection des données. Vous pouvez les trier selon l'une des caractéristiques répertoriées et même avoir la possibilité d'afficher leurs statistiques de performances et d'utilisation actuelles à partir des boutons sur le côté droit.
La vue Public Cloud n'est pas différente, si ce n'est qu'elle n'a pas autant d'informations à afficher en raison du manque de matériel physique nécessaire à la surveillance.
Création d'une machine virtuelle – Cloud privé
Pour créer une nouvelle machine virtuelle, cliquez sur l'icône (+) dans le coin supérieur gauche pour placer la fenêtre de création au premier plan.
La première section contient les informations générales : noms de machines virtuelles, nombre de machines virtuelles et si vous souhaitez qu'elles soient allumées après leur création.
Dans la section suivante, sélectionnez le cluster Hyperviseur où ces VM seront créées. Vous pouvez choisir des clusters HPE SimpliVity ou HPE Alletra dHCI.
Dans la section 3, choisissez la banque de données cible.
Ensuite, sélectionnez le modèle de système d'exploitation souhaité.
Enfin, la politique de provisionnement des machines virtuelles spécifie la politique de protection des données de classe entreprise et exploite HPE GreenLake pour la sauvegarde et la restauration.
Une fois toutes les différentes options et configurations sélectionnées, cliquez sur Créer pour soumettre la VM pour création à votre pile d'infrastructure.
Vous pouvez également vérifier l'état de mise à disposition de la VM en bas, ce qui fera également apparaître une fenêtre pour voir les tâches en cours et en attente ainsi que l'état et une brève sortie de journal de base du processus.
Création d'une machine virtuelle – Cloud public
Créer une VM dans le Cloud Public est aussi simple que d'en créer une dans le Cloud Privé. Cliquez sur l'icône (+) pour afficher la fenêtre des options de création. Sur la première page se trouvent les options initiales de la VM :
- Nom de la machine virtuelle
- Fournisseur de services (pour cet examen, nous avons testé AWS ; Azure est également disponible)
- Surnom du compte – Le compte AWS auquel celui-ci sera attaché
- Région
- Nom de la paire de clés – AWS associera ce nom à la clé publique générée
Après avoir saisi les informations de base, l'écran suivant présentera plus de 2200 XNUMX images à sélectionner pour votre instance de machine virtuelle.
Le dernier écran consiste à choisir le type d'instance. Les options répertoriées sont la taille, l'architecture et si elle est disponible dans l'offre gratuite AWS.
Comme pour le cloud privé, pendant le provisionnement des machines virtuelles, vous pouvez afficher l'état et tous les messages de journal essentiels générés au cours du processus.
Mise à jour des systèmes
La fonctionnalité de mise à jour des systèmes permet aux administrateurs de mettre à jour sans interruption les versions de HPE Alletra Storage Array, ESXi et du micrologiciel du serveur pour un cluster entier. Ces mises à jour sont exécutées dans la section Systèmes de la console de gestion.
Dans cette section de la console de gestion, cliquez sur le bouton Logiciel dans le coin droit de la liste Systèmes pour afficher les catalogues de logiciels actuellement installés dans la pile et le catalogue disponible à mettre à jour dans la pile.
Un catalogue contient des versions spécifiques de Array OS, ESXi, HPE Storage Connection Manager et Service Pack pour ProLiant. Les versions individuelles du catalogue sont visibles en survolant le numéro de version du catalogue affiché dans la ligne de la pile que vous examinez.
Pour commencer le processus de mise à niveau, sélectionnez le cluster que vous souhaitez mettre à niveau et cliquez sur Exécuter la vérification préalable, ou explorez ce système et sélectionnez Exécuter la vérification préalable dans le menu d'actions. Ensuite, sélectionnez la version du catalogue de destination vers laquelle vous souhaitez mettre à niveau la pile de cluster.
Une fois la mise à niveau commencée, elle sera soumise à un ensemble complet de vérifications préalables pour garantir qu'elle est préparée et qu'elle n'entraînera pas de dommages ni de perte de données sur le système.
Une fois terminé, vous recevrez une notification et la vue principale du logiciel mettra également à jour son apparence pour refléter les modifications.
Bilans de santé
Maintenir et confirmer la santé du matériel physique est une responsabilité impérative de chaque administrateur système. L'application Health Checks fournit une automatisation permettant de gagner du temps, garantissant le bon fonctionnement des applications et de l'infrastructure et le respect des meilleures pratiques par l'environnement cloud. Pour valider cela, HPE GreenLake charge le système d'exécuter des vérifications par rapport à environ 75 règles ou directives de configuration pour garantir que l'infrastructure fonctionne dans des conditions optimales.
À titre d'exemple concret, en cas de problème d'accès à une banque de données de machine virtuelle, les contrôles d'intégrité guideront l'administrateur vers l'hôte concerné parmi tous les clusters présentant ce problème. Dans ce cas précis, au sein du portail HPE GreenLake, la méthode la plus efficace pour exécuter les contrôles d'intégrité nécessaires sur les piles d'infrastructure Alletra dHCI consiste à accéder à la page du portail du système concerné via le menu de gauche et à sélectionner « Systèmes » . Une fois sur cette page, choisissez la pile dHCI spécifique sur laquelle vous souhaitez effectuer un contrôle d'intégrité.
Intégrations avec l'automatisation de la console Cloud Data Services
Comprendre comment HPE GreenLake for Private Cloud Business Edition s'intègre dans l'architecture HPE Data Services est essentiel. Comme le montre le diagramme ci-dessous, la console Data Services Cloud (DSCC) comporte plusieurs couches. En commençant par le bas se trouve la couche d’infrastructure de données cloud native. Ceci est représenté par les clusters sur site exécutant des machines virtuelles telles que les clusters HPE Alletra dHCI ou les clusters HPE SimpliVity, y compris les clusters cloud exécutés dans l'un des hyperscalers pris en charge.
La couche intermédiaire est constituée des services d'infrastructure Cloud, auxquels s'intègre Private Cloud Business Edition. Il comprend également des services tels que le service de configuration pour automatiser les déploiements de clusters sur les sites afin de gagner du temps et Data Ops Manager qui peut être utilisé pour configurer des services de blocage tels que la réplication entre clusters ou l'analyse des performances pour détecter les problèmes de performances de stockage.
La couche supérieure est constituée des services de données Cloud, avec des exemples tels que HPE GreenLake pour la sauvegarde et la restauration.
Sauvegarde et récupération
Chaque ingénieur système sait que la sauvegarde et la restauration font partie des domaines d'administration les plus critiques. HPE le comprend également et consacre un portail entier à cet objectif.
Le tableau de bord principal fournit un aperçu du résumé de l'inventaire, du nombre de tâches et de tâches de protection, des avertissements et des problèmes, ainsi que de la consommation des données issues de la sauvegarde dans le cloud et sur site.
Dans le menu de gauche, vous disposez d'un large éventail d'options pour gérer les sauvegardes et les politiques de tous les systèmes gérés dans HPE GreenLake, notamment :
- Politiques de protection
- Amazon Web Services
  - Magasin de blocs élastiques
  - EC2
  - Grappes EKS
  - Service de base de données relationnelle
- Serveurs Microsoft SQL
  - Bases de données
  - Cas
  - Groupes de protection
  - Hôtes d'applications
- VMware
  - Machines virtuelles
  - Banques de données
  - Groupes de protection
  - Serveurs vCenter
- Volumes de baies HPE
- Rapports
- Configuration sur site
  - Orchestrateurs de données
  - Passerelles du magasin de protection
  - Magasins de protection
  - MagasinUne fois
Réflexions finales
HPE GreenLake pour Private Cloud Business Edition, s'appuyant sur HPE GreenLake Cloud Services, offre aux entreprises une solution de cloud hybride sur mesure. Contrairement aux fournisseurs traditionnels, à savoir Amazon Web Services et Microsoft Azure, HPE GreenLake combine l'agilité de la consommation du cloud avec le contrôle de l'infrastructure sur site, mettant l'accent sur la localisation des données pour les entreprises soucieuses de conformité et de sécurité. Il s'appuie sur une infrastructure de données cloud native composée de HPE Alletra dHCI pour les charges de travail critiques pour l'entreprise ou de HPE SimpliVity pour les sites périphériques distribués. Il s'intègre parfaitement aux instances EC2 et Microsoft Azure d'Amazon. Les utilisateurs bénéficient d'une configuration, d'un déploiement, d'un provisionnement de machines virtuelles rationalisés, d'outils de sauvegarde/récupération complets et de contrôles efficaces de l'état du système, garantissant une expérience cloud robuste et efficace.
* HPE GreenLake a déployé l'accès au cloud public aux machines virtuelles Azure en décembre. Cela signifie que vous pouvez désormais créer et gérer des machines virtuelles dans l'écosystème Microsoft de la même manière que vous les gérez actuellement avec AWS EC2.
HPE GreenLake pour l'édition professionnelle du cloud privé
Démo minute 8
Démo minute 3