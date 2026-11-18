Bien que la virtualisation existe depuis les années 1960 (merci IBM), les machines virtuelles (VM) sont devenues courantes au début des années 2000. L'idée était de permettre une meilleure utilisation des ressources matérielles disponibles en exécutant plusieurs machines virtuelles sur une seule plate-forme matérielle physique.
VMware a commencé à développer des logiciels de virtualisation pour les architectures x86 dans les années 90 et a lancé son premier produit de virtualisation en 1999, VMware Workstation. La percée vers l'adoption généralisée de la technologie VM a commencé avec l'introduction en 2001 de VMware ESX Server. D'autres fournisseurs, comme Microsoft et Citrix, sont entrés sur le marché de la virtualisation pour rivaliser pour des parts de marché.
La technologie de virtualisation a continuellement évolué, offrant des innovations telles qu'une meilleure utilisation du matériel, une flexibilité accrue, un déploiement logiciel simplifié et des économies substantielles. Les machines virtuelles sont devenues une pierre angulaire dans divers paysages, des centres de données et systèmes informatiques d'entreprise aux plates-formes de cloud computing et même aux configurations personnelles. La plupart des applications d'entreprise critiques sur les plates-formes de virtualisation accèdent au stockage partagé via Fibre Channel.
Cependant, chaque fournisseur disposait d’un ensemble d’outils de gestion exclusifs et la plupart ne s’intégraient pas à d’autres plates-formes. Ainsi, gérer des centaines ou des milliers de machines virtuelles était un cauchemar avec de multiples outils, interfaces et périphériques de stockage pris en charge. Les équipes opérationnelles ont été confrontées à un défi de gestion lié à la complexité supplémentaire de la gestion des structures Fibre Channel (FC) et des tentatives de résolution des problèmes de VM.
Rendre la gestion plus gérable
En 2007, VMware a introduit ESXi en réponse à l'évolution du paysage et des demandes du marché de la virtualisation. ESXi a amélioré le serveur VMware ESX d'origine avec une efficacité accrue, une meilleure sécurité, une gestion rationalisée, une intégration et des exigences du marché.
VMware a toujours joué un rôle dans la gestion des machines virtuelles, en utilisant divers identifiants pour identifier de manière unique les machines virtuelles au sein de son écosystème. Initialement, VMware utilisait l'adresse MAC unique pour identifier les machines virtuelles, puis passait aux identifiants universels uniques (UUID) du BIOS qui stockaient les données de configuration et incluaient l'identifiant de la machine virtuelle. Avec ESXi 4.x, VMware a introduit le concept d'UUID d'instance pour suivre les instances de VM pendant les opérations de migration et sur vCenter Server.
Avec la sortie de VMware vSphere 5.x, VMware a adopté une approche standardisée pour implémenter des VM-UUID universellement uniques (format UUID-GUID). Ces VM-UUID (alias VM-ID) ont été utilisés pour identifier de manière unique les machines virtuelles dans l'ensemble de l'environnement vCenter Server.
Grâce aux informations contenues dans l'identifiant unique attribué aux machines virtuelles (VM-ID), la gestion d'un écosystème virtualisé et du réseau de stockage (SAN) associé devient plus facile pour les opérations. En tirant parti des détails de l'ID de VM, les administrateurs peuvent identifier et traiter des VM individuelles et effectuer des tâches critiques telles que le provisionnement du stockage, la gestion des cycles de vie des machines virtuelles et la surveillance des mesures de performances. De telles innovations en faveur de la simplicité de gestion ont permis à VMware de devenir le leader majoritaire du marché de la virtualisation de serveurs pour les centres de données d'entreprise.
ID de machine virtuelle Fibre Channel
Bien que VM-ID identifie de manière unique une VM dans un domaine vCenter, cet ID n'est généralement pas partagé ou disponible pour d'autres composants d'infrastructure avec lesquels la VM interagit, comme les périphériques de stockage et de réseau. Les nouvelles fonctionnalités de FC permettent de partager l'ID de machine virtuelle à travers la structure SAN. Dans les dernières versions de VMware vSphere, notamment les versions 7.x et 8.x, VM-ID est une technologie complète et robuste offrant de nombreux avantages lorsqu'elle est intégrée à une infrastructure Fibre Channel moderne.
En tirant parti des ID de machine virtuelle, les administrateurs peuvent mettre en œuvre un contrôle et une surveillance précis du trafic de stockage au sein de la structure Fibre Channel. Mais c'est bien plus que cela. Le VM-ID contient des informations que les administrateurs peuvent utiliser pour affiner les politiques de contrôle d'accès basées sur les VM-ID, garantissant ainsi des transferts de données sécurisés et efficaces entre les machines virtuelles et les périphériques de stockage. Les ID de machine virtuelle facilitent également le dépannage et l'analyse des performances en fournissant un moyen de suivre et d'isoler les problèmes liés à des machines virtuelles spécifiques.
Avec leur gamme QLogic de HBA FC, les fournisseurs de HBA comme Marvell peuvent exploiter les ID de machine virtuelle et permettre au réseau Fibre Channel de s'intégrer de manière transparente aux plates-formes et outils de gestion de virtualisation de Brocade et Cisco.
VM-ID est essentiel pour la gestion centralisée du SAN
Brocade a développé la solution de gestion SANnav pour simplifier et rationaliser la gestion et la surveillance du SAN, en utilisant la technologie VM-ID pour fournir des analyses avancées. Cependant, VM-ID fonctionne avec d'autres solutions de gestion SAN, telles que le Nexus Dashboard Fabric Controller (NDFC) de Cisco.
Comme pour pratiquement tous les aspects de la journée d'un administrateur informatique et cela se répète régulièrement, le volume considérable de données et les infrastructures de stockage complexes rendent la gestion des SAN plus intense et plus difficile. SANnav est un outil précieux pour les administrateurs de stockage, fournissant une plate-forme centralisée pour la gestion SAN. L'exploitation des informations fournies dans VM-ID permet aux administrateurs de contrôler, configurer et dépanner leurs déploiements SAN de manière plus efficace et en toute confiance.
En tant que solution logicielle complète de gestion SAN, SANnav offre une gamme de fonctionnalités pour faciliter une gestion efficace de l'infrastructure de stockage. L'un des principaux avantages de SANnav est sa capacité à fournir une vue consolidée de l'ensemble de l'environnement SAN. Les administrateurs peuvent acquérir une perspective globale sur le réseau interconnecté de commutateurs, de périphériques de stockage et d'hôtes, permettant ainsi une meilleure compréhension et un meilleur contrôle de la structure SAN.
Marvell QLogic : Rendre les HBA plus intelligents
Les adaptateurs de bus hôte intelligents jouent un rôle essentiel dans la gestion SAN et les ID de machine virtuelle. Marvell conçoit et développe des processeurs et des technologies de systèmes sur puce (SoC) qui couvrent de nombreux nouveaux services de centres de données. Ils proposent une gamme large et innovante de solutions de semi-conducteurs pour l'infrastructure de données couvrant le calcul, la mise en réseau, la sécurité et le stockage. Les contrôleurs Fibre Channel et les HBA de Marvell incluent une prise en charge complète de la virtualisation avec les ID de VM, permettant à des milliers de VM d'accéder au stockage partagé via la même puce Fibre Channel.
Marvell QLogic Fibre Channel excelle en termes de performances et de fonctionnalités pour les réseaux de stockage dans VMware. Il rationalise le déploiement de VM à l'aide de VM-ID et prend en charge plusieurs ports avec des configurations FC et FC-NVMe simultanées pour une flexibilité optimale.
Les ID de machine virtuelle fournissent les détails
Il est essentiel de comprendre comment l'intégration de VM-ID à une solution de gestion SAN donne aux administrateurs une vue plus granulaire de l'ensemble de l'infrastructure SAN. SANnav et VM-ID répondent à des objectifs différents au sein de la structure de gestion d'un réseau de stockage, se complétant mutuellement en fournissant une approche complète de la gestion du réseau de stockage dans un environnement virtualisé.
Un VM-ID différencie et identifie les machines virtuelles individuelles, permettant aux plates-formes de virtualisation d'allouer des ressources, de gérer le cycle de vie des machines virtuelles, de faciliter la mise en réseau et de s'intégrer aux outils de gestion. VM-ID permet aux administrateurs de suivre et de gérer les machines virtuelles, d'attribuer des configurations spécifiques et de surveiller les performances.
ID de machine virtuelle Marvell QLogic et SANnav
Il ne fait aucun doute que la virtualisation des serveurs a grandement profité à la plupart des organisations, mais elle a posé plusieurs défis à l'équipe d'infrastructure et aux propriétaires d'applications. Au départ, les propriétaires d'applications étaient sceptiques quant à la capacité d'une plate-forme virtualisée à répondre aux besoins de leurs applications, et il y avait une résistance persistante à l'abandon des serveurs autonomes au profit d'un environnement virtualisé.
Il est juste de dire que les développeurs d'applications n'ont pas eu d'impact sur la virtualisation des serveurs. Cependant, des plaintes persistaient quant au besoin de plus de visibilité sur les mesures réelles, principalement en ce qui concerne les E/S. Le manque de visibilité était dû au fait que l'hyperviseur (dans le cas du serveur VMware vSphere ESXi) faisait abstraction du disque physique vers des disques virtuels placés sur un magasin de données, avec toutes les E/S vers le magasin de données de toutes les machines virtuelles de l'hyperviseur. agrégat. Ainsi, même si les performances globales du sous-système d’E/S sur le serveur étaient visibles, le niveau granulaire de visibilité sur la machine virtuelle réelle et l’application était inconnu.
Pour gagner en visibilité sur ces flux de VM individuels, la structure FC fournit une balise d'identification d'application de machine virtuelle basée sur des normes, VM-ID, à chaque VM. Une fois l'ID d'application attribué à une VM, la VM et les HBA Marvell QLogic 32GFC et 64GFC sur l'hyperviseur utilisent l'ID de la VM pour baliser toutes les trames de cette VM.
Le VM-ID identifie l'instance de VM spécifique qui lance les E/S et toutes les E/S ultérieures destinées à la cible. La balise VM-ID ne peut être appliquée que si la matrice de stockage prend en charge VM-ID. Les informations contenues dans chaque ID de VM permettent à SANnav de les corréler avec des mesures de performances, permettant ainsi aux administrateurs de surveiller des VM individuelles, de suivre l'utilisation des ressources et d'identifier rapidement les goulots d'étranglement potentiels en matière de performances. Les informations fournies par VM-ID fournissent aux administrateurs les détails nécessaires pour identifier et dépanner une VM affectée et prendre des mesures pour résoudre rapidement le problème.
Travaillant de concert, SANnav utilise les informations VM-ID intégrées dans chaque paquet FC par le Marvell QLogic FC-HBA pour suivre efficacement les performances des machines virtuelles individuelles.
VMware ESXi dans l'entreprise
Les organisations utilisent VMware ESXi pour créer et gérer des machines virtuelles. ESXi est un hyperviseur nu qui permet aux organisations de consolider plusieurs machines virtuelles sur un seul serveur, offrant ainsi flexibilité, optimisation des ressources et gestion plus facile d'une infrastructure informatique.
Le déploiement d'ESXi dans l'entreprise présente de nombreux avantages, ainsi que certains inconvénients. Grâce à la possibilité d'exécuter plusieurs machines virtuelles sur un seul serveur physique, les organisations peuvent réduire les coûts matériels et optimiser l'utilisation du serveur. Cela permet d'économiser sur la consommation d'énergie, le refroidissement et l'espace requis. Cependant, il s’agit du facteur le plus important contribuant à la prolifération des machines virtuelles.
Avec ESXi, les administrateurs peuvent allouer des ressources informatiques, comme le processeur, la mémoire et le stockage, aux machines virtuelles en fonction des besoins. Cette flexibilité garantit une utilisation efficace des ressources et évite le surprovisionnement et la sous-utilisation des ressources du serveur. ESXi prend en charge des fonctionnalités telles que vSphere High Availability (HA) et vSphere Fault Tolerance (FT), offrant ainsi une disponibilité et une résilience accrues du serveur.
ESXi utilise VMware vCenter Server comme interface de gestion centralisée pour surveiller, provisionner et gérer les environnements virtualisés. Sans VM-ID, gérer une infrastructure virtualisée comme celle-ci serait difficile, voire impossible.
Propagation des machines virtuelles
ESXi peut sans aucun doute contribuer à la prolifération des machines virtuelles s'il n'est pas correctement géré. VMware répond à ces préoccupations en fournissant des outils et des bonnes pratiques aux administrateurs informatiques. Ces outils incluent la planification des ressources et des capacités, la gestion du cycle de vie et l'automatisation basée sur des politiques.
VMware ESXi et vCenter sont essentiels au déploiement de la virtualisation d'entreprise, permettant aux organisations de répondre aux exigences de consolidation des serveurs, d'optimisation des ressources, de haute disponibilité et de gestion. Cependant, VM-ID est fondamental pour identifier et différencier les machines virtuelles individuelles, permettant ainsi aux administrateurs de gérer et d'optimiser efficacement leur infrastructure virtualisée.
Méfiez-vous de l'effet de mixage d'E/S
L'effet de mélange d'E/S se produit dans les environnements virtualisés, y compris ESXi, lorsque les modèles d'entrée/sortie (E/S) de stockage deviennent aléatoires et moins prévisibles. Cela peut être dû au fonctionnement simultané de plusieurs machines virtuelles (VM) partageant le même hôte physique et accédant aux ressources de stockage.
Dans un environnement virtualisé, plusieurs machines virtuelles exécutées sur un seul hôte peuvent envoyer des requêtes d'E/S à l'infrastructure de stockage sous-jacente à des moments différents, avec différents niveaux d'intensité et de fréquence. Lorsque l'hyperviseur reçoit ces requêtes d'E/S, elles sont agrégées et sérialisées avant d'être envoyées au système de stockage. En conséquence, les modèles d'E/S générés par les machines virtuelles deviennent « mélangés » ou mélangés. Cela rend difficile et chronophage l'identification des voisins bruyants et des coupables qui provoquent des embouteillages ou des blocages de tête de ligne, ce qui entraîne souvent le non-respect des SLA.
L’atténuation des effets de ce phénomène peut être obtenue grâce à diverses techniques telles que :
- Implémentation de la hiérarchisation du stockage
- Utiliser les mécanismes de QoS
- Optimisation des E/S
- Technologie VM-ID dans les HBA FC
Quel rôle joue le VM-ID ?
Bien que le VM-ID n’affecte pas directement l’effet du mélangeur d’E/S, il atténue considérablement son impact. Le VM-ID peut être exploité en implémentant les éléments suivants :
- Associer des ID de machine virtuelle à des politiques de QoS de stockage spécifiques
- Mappage d'ID de machine virtuelle spécifiques à des ressources de stockage dédiées
- Utiliser les ID de VM pour l'équilibrage de charge
- Utiliser les ID de VM en conjonction avec des outils de surveillance
Ainsi, même si l'ID de VM n'a pas d'impact direct sur l'effet du mélangeur d'E/S, offrant une visibilité et un contrôle sur les machines virtuelles individuelles, les administrateurs peuvent personnaliser l'approvisionnement du stockage et hiérarchiser les ressources en fonction des ID de VM, ce qui entraîne des performances améliorées, une réduction des conflits et meilleure gestion globale de l’effet du mixeur d’E/S.
Marvell continue d'innover
Les cartes HBA Fibre Channel de Marvell offrent des performances et des fonctionnalités optimales pour le protocole Fibre Channel (FCP) et NVMe sur Fibre Channel ( FC-NVMe ). Conçues avec des chemins isolés pour chaque port, elles garantissent des performances à débit maximal par port et une fiabilité exceptionnelle. Ces adaptateurs offrent des millions d'IOPS, une latence de l'ordre de la microseconde et un débit maximal jusqu'à 64 Gbit/s par canal. Les technologies Marvell StorFusion™ et VM-ID simplifient le déploiement et l'intégration dans les réseaux SAN Fibre Channel.
Marvell StorFusion
La technologie Marvell StorFusion inclut des fonctionnalités avancées activées lorsqu'elles sont déployées avec les commutateurs Brocade et Cisco pris en charge. En combinant ces solutions, les administrateurs SAN peuvent profiter de fonctionnalités améliorées qui améliorent la disponibilité, accélèrent le déploiement et augmentent les performances du réseau.
À partir de la carte HBA QLE2690 et améliorée par les séries QLE2770 et QLE2870, les adaptateurs Marvell ont pris en charge plusieurs fonctionnalités de virtualisation basées sur des normes qui optimisent le déploiement, le dépannage et les performances des applications des serveurs virtuels.
La technologie Marvell VM-ID s'intègre facilement aux commutateurs Brocade et Cisco, permettant aux clients de surveiller et de gérer la qualité de service dans leurs réseaux de stockage Fibre Channel ; par exemple, l'équilibrage de charge des clusters de machines virtuelles avec le stockage pour garantir une utilisation efficace des ressources de stockage.
À partir de VMware ESXi 6.x, la prise en charge du balisage des demandes et des réponses d'E/S avec l'ID de VM de la machine virtuelle correspondante offre une visibilité complète au niveau de la VM.
De plus, la technologie Marvell StorFusion Universal SAN Congestion Mitigation ( USCM), basée sur la norme industrielle Fabric Performance Impact Notifications (FPIN), permet aux HBA et aux commutateurs du SAN d'identifier et d'atténuer les problèmes de congestion potentiels au sein du réseau. La prise en charge de la virtualisation N_Port ID (NPIV) permet à un seul port d'adaptateur FC de fournir plusieurs ports virtuels pour une évolutivité accrue du réseau. La technologie QoS basée sur le contrôle spécifique à la classe (CS_CTL) standard par port NPIV permet des contrôles et des garanties de bande passante multiniveaux par machine virtuelle. Par conséquent, les charges de travail critiques peuvent se voir attribuer une priorité plus élevée que le trafic de stockage moins sensible au facteur temps pour des performances optimales.
Balisage des trames d'E/S pour les machines virtuelles Fibre Channel
Sous les couvertures, la technologie FC VM-ID implique que le FC HBA balise les trames d'E/S avec des balises VM et que le commutateur FC lise ces balises et enregistre des statistiques par VM. Cela apporte plusieurs avantages, notamment une visibilité améliorée, une allocation des ressources et un dépannage. Le marquage des trames d'E/S avec des balises VM offre une visibilité améliorée du trafic généré par chaque VM individuelle. Cela permet une surveillance, une analyse et une gestion efficaces du trafic de stockage. Grâce au balisage des VM, les administrateurs obtiennent une vue claire de l'allocation des ressources, mettent en œuvre des politiques de QoS spécifiques à une VM individuelle, simplifient le dépannage et améliorent la sécurité et le contrôle d'accès.
La technologie Fibre Channel VM-ID nécessite la prise en charge de VM-ID dans le HBA FC, le commutateur FC et la baie de stockage. La majorité des HBA et commutateurs FC modernes prennent en charge VM-ID. Cependant, VM-ID n'est pris en charge que sur les baies de stockage de NetApp et PureStorage, ce qui pose un défi à l'adoption et au déploiement généralisés de cette technologie. L'innovation récente sur les commutateurs Brocade supprime la limitation VM-ID grâce à la technologie Tagless VM-ID ou VM-ID+.
Marvell VM-ID au Flash Memory Summit 2023
Lors du Flash Memory Summit (FMS), une vitrine internationale annuelle sur la mémoire et le stockage, nous avons visité le stand Marvell pour avoir un aperçu de ce sur quoi ils travaillaient. Parallèlement aux démonstrations de contrôleurs SSD, d'accélérateurs NVMe et de chipsets CXL, il y avait une démonstration en direct de l'ID VM Fibre Channel. La démonstration de VM-ID a attiré un grand nombre de clients et de partenaires. VM-ID pour Fibre Channel prend définitivement son essor.
VM-ID sans balise ou VM-ID+
Comme mentionné ci-dessus, VM-ID+ supprime la dépendance à la matrice de stockage pour prendre en charge le balisage VM-ID. VM-ID+ est configuré sur les ports de la structure SAN auxquels la matrice de stockage est connectée. Lorsque VM-ID+ est activé, la balise VM-ID des trames envoyées depuis l'hyperviseur vers la baie de stockage est supprimée par le commutateur Brocade Gen 7 au niveau du port de sortie connecté à la baie de stockage. Les trames que la baie de stockage envoie à l'hyperviseur ont la balise VM-ID ajoutée par la structure. Les commutateurs Fabric maintiennent le mappage et la collecte des données de télémétrie des VM.
Suivi des machines virtuelles avec VM-ID à partir de la ligne de commande
Les commandes du commutateur Brocade FC qui affichent les machines virtuelles actuelles et leurs statistiques fonctionnant au sein de la structure :
A. Exécutez la commande « appserver –show -all ». Le résultat est affiché ci-dessous ; les informations indiquent le nombre total de machines virtuelles (VM) en fonctionnement, identifiées par leur ID. Dans ce cas, trois VM sont présentes sur chaque hôte ESX. La dernière ligne affiche le nombre total de VM.
sw0-G720:FID128: > appserver –show-all
--------------------
Affichage des résultats pour Tissu
--------------------
N_ID de port : 010300
ID d'entité (ASCII) : 52 b3 0f fc 5a 05 47 a6-18 eb aa b4 b4 8f 9a 5f
ID d'entité (hexadécimal) : 0x35322062332030662066632035612030352034372061362d3138206562206161206234206234203866203961203566
ID de l'application : 0x00000010 (16)
Nom de l'entité :
Identifiant de l'hôte :
Données symboliques :
-------
N_ID de port : 010300
ID d'entité (ASCII) : 52 2c c3 8f c8 3f f5 75-a5 6c db bd 89 3a 95 13
ID d'entité (hexadécimal) : 0x35322032632063332038662063382033662066352037352d6135203663206462206264203839203361203935203133
ID de l'application : 0x00000012 (18)
Nom de l'entité :
Identifiant de l'hôte :
Données symboliques :
-------
N_ID de port : 010300
ID d'entité (ASCII) : 52 b1 ac 8d 2a aa 93 c4-5e 51 98 24 84 63 e0 c2
ID d'entité (hexadécimal) : 0x35322062312061632038642032612061612039332063342d3565203531203938203234203834203633206530206332
ID de l'application : 0x00000018 (24)
Nom de l'entité :
Identifiant de l'hôte :
Données symboliques :
-------
N_ID de port : 010800
ID d'entité (ASCII) : 52 bb 51 48 8a 5c 98 33-7a 74 c6 d5 27 05 58 49
ID d'entité (hexadécimal) : 0x35322062622035312034382038612035632039382033332d3761203734206336206435203237203035203538203439
ID de l'application : 0x00000010 (16)
Nom de l'entité :
Identifiant de l'hôte :
Données symboliques :
-------
N_ID de port : 010800
ID d'entité (ASCII) : 52 36 64 98 87 5d a5 c6-02 38 0a d7 85 42 3b 4b
ID d'entité (hexadécimal) : 0x35322033362036342039382038372035642061352063362d3032203338203061206437203835203432203362203462
ID de l'application : 0x00000012 (18)
Nom de l'entité :
Identifiant de l'hôte :
Données symboliques :
-------
N_ID de port : 010800
ID d'entité (ASCII) : 52 de 5b 4f a9 9f 98 12-65 4f e7 ca c5 78 c2 3c
ID d'entité (hexadécimal) : 0x35322064652035622034662061392039662039382031322d3635203466206537206361206335203738206332203363
ID de l'application : 0x00000018 (24)
Nom de l'entité :
Identifiant de l'hôte :
Données symboliques :
-------
Le serveur d'applications affiche six entrées.
B. Brocade Analytics Engine a rapporté des statistiques pour les machines virtuelles
Utilisez les commandes ci-dessous pour détailler les métriques d'E/S pour chaque VM
C. Vérification et configuration d'un port de commutateur Brocade à cibler pour le VMID sans étiquette (pour les baies de stockage autres que NetApp et PureStorage) :
sw0-G720:FID128:admin> portcfgappheader -h
Usage:
portCfgAppHeader <[slot/]port> – activer/– désactiver
D. L'exécution de la commande sur un port précédemment configuré affichera ce qui suit :
portcfgappheader 26 – activer
Même configuration pour le port 26
Le mode d'investigation de SANnav fournit un aperçu des performances opérationnelles de chaque machine virtuelle. Il collecte et stocke les statistiques de performances du SAN et les données de télémétrie, puis fournit des graphiques de séries chronologiques clairs et intuitifs qui tracent les principales mesures de trafic. Il comprend des détails sur les violations MAPS pour les ports, les liaisons et les lignes réseau, les tunnels et circuits d'extension et les flux pour aider les utilisateurs à comprendre et à étudier les comportements complexes des modèles de trafic. De plus, il peut collecter des métriques plus fréquemment et en temps quasi réel (à intervalles de 10 secondes) pour les ports sélectionnés.
Une fois connecté, la vue du tableau de bord SANnav affichera la structure/les commutateurs en cours de gestion.
1. Accédez à Inventaire—> Flux —> sélectionnez le filtre défini par l'utilisateur :
« All-VMs-flows » et les flux détaillés seront affichés comme illustré ci-dessous :
2. Cliquez sur l'icône (…) dans le coin supérieur droit pour afficher un menu déroulant, puis cliquez sur « Sélection en masse ».
3. Dans la zone de bannière au-dessus de toutes les machines virtuelles, cochez la case pour sélectionner toutes les machines virtuelles.
4. Une fois les machines virtuelles sélectionnées, cliquez sur le bouton Action en haut à droite, puis sélectionnez « Analyser ».
Dans la fenêtre Mode Investigation :
5. Cochez les cases correspondant à chacune des VM et cliquez sur l'option « Débit de données de lecture » dans le panneau de gauche.
6. Ensuite, cliquez sur la flèche vers le bas à côté de « 30 dernières minutes ».
7. Une autre fenêtre « Sélectionnez la plage de dates » s’affiche. Cliquez sur l’option prédéfinie « Dernière semaine » à gauche, puis sur « Appliquer ».
8. Cliquez ensuite sur la flèche vers le bas à côté de « Intervalle : 5 minutes » et sélectionnez l'option « 6 heures ».
9. Une vue d'informations détaillée sera affichée pour les six machines virtuelles et le trafic associé présent dans la structure. Déplacer le curseur et survoler un index temporel graphique spécifique affichera les performances de débit de données en lecture pour chacun.
10. Cliquez ensuite sur la flèche vers le bas à côté de « Intervalle : 5 minutes » et sélectionnez l'option « 6 heures ».
11. Une vue d'informations détaillée sera affichée pour les six machines virtuelles et le trafic associé présent dans la structure. Déplacer le curseur et survoler un index temporel graphique spécifique affichera les performances de débit de données en lecture pour chacun.
12. Cliquez sur l'option « Temps réel » en haut à droite pour afficher et actualiser les détails toutes les dix secondes.
Les profils de performances et d'E/S des machines virtuelles individuelles activés par VM-ID et affichés dans les écrans SANnav ci-dessus permettent aux administrateurs SAN et de stockage d'obtenir une visibilité sur les modèles de trafic pour chaque VM.
La technologie Marvell QLogic VM-ID et Brocade SANnav sont des leaders de l'innovation en matière de gestion moderne des données. Avec VMware ESXi, les capacités transparentes de déploiement et d'orchestration de VM-ID et les outils complets de gestion du stockage de SANnav, les entreprises peuvent naviguer en toute confiance et facilement dans les complexités des environnements virtualisés.
Ces solutions permettent aux organisations d'exploiter tout le potentiel de leur infrastructure de données, garantissant des performances, une efficacité et une adaptabilité optimales. À mesure que la technologie évolue, Marvell QLogic VM-ID et SANnav restent des partenaires fidèles dans le cheminement vers une gestion rationalisée des données et une excellence opérationnelle améliorée.
Ce rapport est parrainé par Marvell. Tous les points de vue et opinions exprimés dans ce rapport sont basés sur notre vision impartiale du ou des produits à l'étude.