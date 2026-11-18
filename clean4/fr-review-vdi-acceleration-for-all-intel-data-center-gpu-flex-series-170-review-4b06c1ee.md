La communauté des entreprises Virtual Desktop Infrastructure (VDI) réclame des solutions plus robustes. Alors que les entreprises s’efforcent d’améliorer leur efficacité et leur expérience utilisateur, le rôle des accélérateurs matériels dédiés est devenu essentiel. La série Data Center GPU Flex d'Intel occupe une place importante dans cet espace, offrant des solutions sur mesure pour les jeux en nuage, les médias, le VDI et l'accélération graphique au sein des centres de données.
Que sont les GPU Intel Flex Series ?
L'offre d'Intel repose principalement sur deux produits phares : les GPU Flex 140 et Flex 170. La Flex Series 140, une carte PCIe Gen4 à profil bas, intègre deux GPU, chacun doté de huit cœurs Xe et de 6 Go de mémoire GDDR6. Cette configuration est idéale pour gérer jusqu'à 12 sessions VDI et répond aux besoins des utilisateurs ayant des exigences graphiques modérées.
Pour les applications plus gourmandes en graphiques, la Flex Series 170 évolue avec un seul nœud GPU doté de 32 cœurs Xe et de 16 Go de mémoire GDDR6, le tout sur une carte PCIe pleine taille, offrant une puissance de feu substantielle pour les tâches haute résolution.
|  | GPU pour centre de données Intel Flex 170 | GPU pour centre de données Intel Flex 140 | 
|---|---|---|
| Essentiels |  |  | 
| Microarchitecture | Xe HPG | Xe HPG | 
| Options embarquées disponibles | Non | Non | 
| Conditions d'utilisation | Serveur/Entreprise | Serveur/Entreprise | 
| Cas d'utilisation | Cloud Computing | Cloud Computing | 
| Spécifications du processeur graphique |  |  | 
| xe-couleurs | 32 | 16 | 
| Tranches de rendu | 8 | 4 | 
| Unités de lancer de rayons | 32 | 16 | 
| Moteurs d'extensions matricielles Intel® Xe (Intel® XMX) | 512 | 256 | 
| Unités d'exécution | 512 | 256 | 
| Horloge dynamique Graphics Max | 2050 MHz | 1950 MHz | 
| Extensions matricielles Intel® Xe (Intel® XMX) Horloge dynamique maximale | 1950 MHz | 1600 MHz | 
| TBP | 150 W | 75 W | 
| Caractéristiques de la mémoire |  |  | 
| Taille de la mémoire | 16 GB | 12 GB | 
| Type de mémoire | GDDR6 | GDDR6 | 
| Interface mémoire graphique | 256 Bits | 192 Bits | 
| Bande passante de la mémoire graphique | 576 GB / s | 336 GB / s | 
| Technologies prises en charge |  |  | 
| Lancer de rayons | Oui | Oui | 
| Prise en charge d'une API | Oui | Oui | 
| Prise en charge d'OpenVINO™ | Oui | Oui | 
| Prise en charge de DirectX* | DirectX 12 Ultime | DirectX 12 Ultime | 
| Assistance Vulkan* | 1.3 | 1.3 | 
| Prise en charge d'OpenGL* | Jusqu'à 4.6 | Jusqu'à 4.6 | 
| Prise en charge d'OpenCL* | 3 | 3 | 
| Moteurs de codecs multiformats | 2 | 4 | 
| Fonctionnalité |  |  | 
| Encodage/décodage matériel H.264 | Oui | Oui | 
| Encodage/décodage matériel H.265 (HEVC) | Oui | Oui | 
| Encodage/Décodage AV1 | Oui | Oui | 
| Flux binaire et décodage VP9 | Oui | Oui | 
Le fondement de ces unités de traitement graphique est le Xe-core, le Flex Series 170 comportant deux fois plus de cœurs et de tranches de rendu que le Flex 140. Cela trace une voie directe vers des prouesses de traçage de rayons doublées, renforçant la capacité du Flex 170 à gérer un rendu complexe. tâches et simulations facilement.
Sur le plan de la mémoire, nous voyons également le Flex Series 170 prendre de l'avance avec une allocation GDDR16 de 6 Go, surclassant les 140 Go du Flex 12. Cette mémoire supplémentaire, combinée à une interface plus large de 256 bits, offre une bande passante graphique culminant à 576 Go/s, contre 336 Go/s pour son homologue.
Le débat sur l'efficacité persiste concernant la puissance thermique de conception (TDP) des GPU. Le TDP de 170 watts du Flex Series 150 indique l'accent mis sur les performances, tandis que le TDP de 140 watts du Flex Series 75 souligne son penchant pour les applications économes en énergie et économes en énergie.
La prise en charge de technologies de pointe telles que Ray Tracing, oneAPI et OpenVINO est un fil conducteur commun aux deux modèles, garantissant une plate-forme de développement évolutive. Parallèlement, la compatibilité DirectX 12 Ultimate prend en charge des graphiques ultra-réalistes, un clin d'œil aux applications croisées potentielles dans des domaines tels que la visualisation professionnelle et les jeux en nuage.
La magie des GPU Intel Flex : SR-IOV
La magie du GPU Flex Series vient du SR-IOV. Si vous le connaissez, n'hésitez pas à passer à la section suivante ; Il n'y a rien de nouveau ici. Sinon, ou si vous avez besoin d’un rappel, attachez votre ceinture ; c'est des trucs sympas.
La virtualisation d'E/S à racine unique (SR-IOV) est une technologie qui améliore la gérabilité et l'efficacité des environnements virtualisés en permettant à un seul périphérique physique, tel qu'une carte d'interface réseau (NIC) ou une unité de traitement graphique (GPU), d'apparaître comme plusieurs appareils virtuels distincts. Ceci est particulièrement utile dans les centres de données pour améliorer les performances des machines virtuelles (VM) et maximiser l'utilisation des ressources matérielles sous-jacentes.
La technologie SR-IOV repose sur deux concepts fondamentaux : les fonctions physiques (PF) et les fonctions virtuelles (VF). Le PF est l'interface principale du périphérique physique et gère la fonctionnalité SR-IOV, y compris la création et la gestion des VF. Ces VF sont des versions allégées du PF, dotées des ressources nécessaires au déplacement des données mais avec des capacités de configuration réduites. Chaque VF peut être directement attribué à une VM, offrant ainsi un accès direct et hautes performances aux capacités du périphérique sans la surcharge typique des périphériques virtualisés.
Lorsque SR-IOV est utilisé avec des GPU, il permet à chaque machine virtuelle (VM) d'accéder directement à une partie des ressources du GPU. Cet accès direct est facilité grâce aux fonctions virtuelles (VF), qui sont des représentations légères du GPU qui peuvent être attribuées individuellement aux machines virtuelles.
Bien que les VF soient contrôlés par les machines virtuelles auxquelles ils sont attribués, le PF conserve le contrôle global, gérant les ressources et appliquant les politiques au niveau des appareils. Cette configuration est inestimable dans les scénarios où les performances et une faible latence sont critiques, comme dans les environnements VDI complexes, les tâches de calcul hautes performances et les services Web à grande échelle, améliorant considérablement l'efficacité opérationnelle des systèmes virtualisés.
Cette configuration permet aux machines virtuelles de contourner les méthodes traditionnelles de partage de ressources basées sur un hyperviseur, réduisant ainsi les frais généraux et améliorant les performances. Les tâches gourmandes en GPU, telles que le rendu 3D, le traitement vidéo ou les applications d'apprentissage automatique, peuvent entraîner une latence considérablement plus faible, une utilisation plus efficace des ressources GPU et une amélioration des performances globales dans les environnements virtualisés.
Les GPU Intel Flex offrent une accélération VDI « gratuite »
Le titre de la section n’est pas une blague. La série Intel Data Center GPU Flex arrive sur la scène des accélérateurs avec un avantage significatif dans les déploiements VDI : aucun coût de licence pour la configuration des configurations de GPU virtuel (vGPU). Tirant parti de la virtualisation GPU basée sur la virtualisation d'E/S à racine unique (SR-IOV) susmentionnée, cette série Intel Flex élimine les barrières financières traditionnelles associées au provisionnement de vGPU. L'absence de frais de licence réduit les coûts d'installation initiaux et diminue les dépenses opérationnelles continues, permettant ainsi des économies significatives à long terme.
Cette approche rentable de l'administration des vGPU améliore l'évolutivité des déploiements de serveurs VDI. Les organisations peuvent provisionner et ajuster dynamiquement les ressources vGPU dans les environnements virtuels sans se soucier des coûts de licence supplémentaires. Cette flexibilité est cruciale pour s'adapter à l'évolution des demandes de charge de travail et prendre en charge un nombre croissant d'utilisateurs, en particulier dans les environnements à haute densité typiques des segments des travailleurs du savoir.
D'un point de vue commercial, les GPU Intel Flex Series offrent des avantages financiers substantiels. En supprimant les complexités et les coûts liés aux licences, Intel permet aux organisations de rationaliser leur infrastructure VDI. Cette simplification accélère les temps de déploiement et réduit le besoin d'allocations budgétaires importantes pour les capacités GPU, faisant de la série Intel Flex une option intéressante pour les entreprises cherchant à optimiser leurs investissements VDI tout en maintenant des performances et une fiabilité élevées.
Pratique : Intel Flex Series 170 avec Supermicro SuperBlade
Dans notre laboratoire, nous avons testé la carte graphique Intel Flex Series 170 avec VMware sur notre système Supermicro SuperBlade X13 GPU Blade. L'installation des cartes graphiques dédiées Intel sur VMware ESXi a été simplifiée pour optimiser l'expérience utilisateur et les performances du système. Il a suffi de télécharger le pilote via SCP sur l'hôte à l'aide d'un fichier ZIP, d'activer l'accès SSH et de lancer l'installation. Après un redémarrage rapide de l'hôte, la Flex Series 170 est apparue dans la liste du matériel et les options SR-IOV (de 0 à 31) étaient disponibles pour la configuration.
Les tests ont été réalisés à l'aide du système Supermicro SuperBlade , indispensable pour valider les performances du GPU Flex Series 170 en tant qu'accélérateur VDI. Le système SuperBlade est conçu pour optimiser la densité de calcul et l'efficacité tout en minimisant la consommation d'énergie. Il constitue ainsi une plateforme idéale pour tester les cas d'utilisation à haute densité destinés aux travailleurs du savoir, tels que ceux proposés par les GPU Intel Flex Series.
| Partie | Configuration SuperMicro SuperBlade | 
|---|---|
| Processeur | 1xIntel Xeon 8562Y+ | 
| Mémoire | 256GB DDR5 | 
| par chaîne | 2x lecteur M.3840 Samsung 2G | 
| GPU | Intel Flex série 170 | 
Le système Supermicro SuperBlade X13 constitue une plateforme idéale pour optimiser la densité de déploiement. Grâce à la possibilité d'intégrer 10 nœuds dans un seul châssis 8U, il est envisageable d'héberger jusqu'à 320 VDI accélérés dans un châssis compact et facile à gérer. À l'inverse, en optant pour davantage de cœurs et de VRAM par carte Flex Series 170, on peut déployer 80 à 160 VDI pour les utilisateurs avertis. De plus, la possibilité d'intégrer jusqu'à 10 GPU dans ce système offre une grande flexibilité, et son réseau interne ultra-rapide permet de mettre en œuvre des solutions de basculement innovantes. Consultez notre test complet pour en savoir plus sur ce serveur SuperBlade, véritable couteau suisse du système.
Performances du GPU Intel Data Center Flex Series 170
Il est important de considérer ici que les tests ont été sélectionnés en fonction de ce qui produirait officiellement un résultat complet, une limitation des tests dans des environnements virtuels sans ajustements ni hacks particuliers. Nous avons sélectionné quelques tranches SR-IOV puis quelques tranches non standard pour voir à quoi cela ressemblerait.
Rendu 3D
Nous nous sommes tournés vers les benchmarks 3D Mark Wildlife pour montrer les capacités de jeu en nuage et de rendu 3D du GPU Flex Series.
3DMark Wild Life propose un outil d'analyse comparative multiplateforme compatible avec les systèmes Windows, Android et Apple iOS. Cet outil évalue et compare les performances graphiques de divers appareils, notamment les ordinateurs portables, les tablettes et les smartphones. Wild Life utilise l'API graphique Vulkan pour les appareils Windows et Android, alors qu'il utilise Metal pour les appareils iOS. Étant donné que ce test fonctionne sur des graphiques intégrés selon différents scores, il peut illustrer la puissance d'un score graphique pur de l'Intel Flex Series 170.
| Test/Tranche SR-IOV | 2GB | 4GB | 7GB | 14GB | 
|---|---|---|---|---|
| Marque 3D de la faune | 29,062 | 42,466 | 49,671 | 45,908 | 
| Marque 3D de la faune extrême | 9,023 | 14,948 | 17,661 | 16,959 | 
LuxMark
Vient ensuite LuxMark, un utilitaire d’analyse comparative des GPU OpenCL. Le Flex Series 170 s'est vraiment montré flexible lors de ce test, affichant des chiffres et une mise à l'échelle impressionnants.
| Test/Tranche SR-IOV | 2GB | 4GB | 7GB | 14GB | 
|---|---|---|---|---|
| Salle Luxmark | 2,961 | 4,382 | 11,002 | 11,202 | 
| Nourriture Luxmark | N/D | 1,316 | 4,502 | 4,525 | 
PCMark 10 Express
Vous vous tournez vers PCMark 10, équipé d'une vaste suite de tests qui reflètent avec précision le large éventail de tâches rencontrées sur le lieu de travail. Cet outil d'analyse comparative comprend diverses évaluations de performances, des options de tests personnalisés, un profil d'autonomie de la batterie et de nouveaux tests de stockage, ce qui en fait une solution globale pour évaluer les performances des PC de bureau modernes.
| Test/Tranche SR-IOV | 2GB | 4GB | 7GB | 14GB | 
|---|---|---|---|---|
| PCMark 10 Express dans son ensemble | 5,111 | 5,146 | 5,311 | 5,218 | 
| Essentials JAMAIS | 10,269 | 10,318 | 10,734 | 10,364 | 
| Score de démarrage des applications | 17,833 | 17,664 | 19,034 | 17,340 | 
| Score de vidéoconférence | 7,798 | 7,933 | 8,095 | 7,980 | 
| Score de navigation Web | 7,789 | 7,839 | 8,028 | 8,046 | 
| Productivité | 6,952 | 7,004 | 7,181 | 7,180 | 
| Score des feuilles de calcul | 6,924 | 6,953 | 7,186 | 7,184 | 
| Écriture de la partition | 6,981 | 7,057 | 7,178 | 7,177 | 
En regardant nos résultats ici, même s’ils ne sont pas aussi spectaculaires, nous pouvons constater une nette mise à l’échelle de diverses tâches qui tirent parti de l’accélération.
Pour faciliter la comparaison, j'ai compilé les résultats de tous les tests dans un seul tableau.
| Test/Tranche SR-IOV | 2GB | 4GB | 7GB | 14GB | 
|---|---|---|---|---|
| Marque 3D Vie Sauvage | 29,062 | 42,466 | 49,671 | 45,908 | 
| Marque 3D Wild Life Extreme | 9,023 | 14,948 | 17,661 | 16,959 | 
| Salle Luxmark | 2,961 | 4,382 | 11,002 | 11,202 | 
| Nourriture Luxmark | N/D | 1,316 | 4,502 | 4,525 | 
| PCMark 10 Express dans son ensemble | 5,111 | 5,146 | 5,311 | 5,218 | 
| Essentials JAMAIS | 10,269 | 10,318 | 10,734 | 10,364 | 
| Score de démarrage des applications | 17,833 | 17,664 | 19,034 | 17,340 | 
| Score de vidéoconférence | 7,798 | 7,933 | 8,095 | 7,980 | 
| Score de navigation Web | 7,789 | 7,839 | 8,028 | 8,046 | 
| Productivité | 6,952 | 7,004 | 7,181 | 7,180 | 
| Score des feuilles de calcul | 6,924 | 6,953 | 7,186 | 7,184 | 
| Écriture de la partition | 6,981 | 7,057 | 7,178 | 7,177 | 
Réflexions de clôture
En analysant toutes ces données et en tenant compte du facteur humain, j'étais constamment stupéfait, voire impressionné, par la simplicité et la puissance de ces cartes. Après avoir alloué 1/32e du GPU à la machine virtuelle et installé le pilote Windows d'Intel , des fonctionnalités comme le Bureau à distance intégré fonctionnaient mieux. L'utilisation de Google Earth dans Chrome, une fonctionnalité phare d'Intel, a démontré que même avec 512 Mo de VRAM et un seul cœur Xe, l'expérience VDI est étonnamment meilleure. Ayant utilisé d'autres produits VDI par le passé, une fois le pilote installé, l'expérience était tout simplement agréable.
La série Flex de GPU Intel Data Center, en particulier le modèle Flex Series 170, s'est avérée être une solution VDI fiable et évolutive qui offre des performances exceptionnelles tout en améliorant la densité et l'évolutivité des machines virtuelles. La combinaison d'une faible utilisation du processeur, de fréquences d'images constantes et de performances d'encodage élevées par watt en fait un choix idéal pour les centres de données cherchant à transformer leur infrastructure VDI en solutions hautement performantes, fiables et évolutives. Avec le GPU Intel Flex 170 en son cœur, les administrateurs VDI peuvent offrir en toute confiance aux utilisateurs finaux des expériences utilisateur graphiques exceptionnelles tout en maintenant des normes de qualité élevées sur différentes résolutions d'affichage.
En les combinant avec la densité pouvant être obtenue avec le système Supermicro SuperBlade, vous pouvez créer une appliance VDI super dense et très efficace. Grâce à la flexibilité des lames, le châssis pourrait accueillir des cartes Max plus puissantes à côté des GPU Flex Series, rapprochant ainsi tous les utilisateurs et les interactions de service. Nous en reparlerons bientôt.
Bien que la portée de ces tests n’inclue pas de comparaisons avec la plupart des ordinateurs portables professionnels, vous pouvez tracer ces lignes par vous-même. Lorsque l’on prend en compte la compression des données pour l’envoi de pixels par fil, la sécurité et la facilité de gestion fournies par un VDI, ainsi que le modèle de licence (qui, espérons-le, ne renversera pas la tendance), la série Flex se positionne étonnamment bien. Je peux résumer les cartes Flex Series en deux avantages qui rendent les entreprises et les utilisateurs heureux : de meilleures performances et des PC clients moins chers à déployer. Pour nous, cela devient une évidence lorsque nous cherchons à améliorer les configurations VDI vieillissantes ou ternes avec lesquelles les organisations ont du mal.
Page produit du processeur Intel Data Center GPU Flex Series 170