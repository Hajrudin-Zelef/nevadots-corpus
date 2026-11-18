StorageReview a testé plusieurs serveurs durcis, en se concentrant sur leurs performances, leur capacité et leur facilité d'utilisation. Nous avons récemment testé la solution RAID Cheetah équipée de SSD Solidigm. Malheureusement, pour des raisons évidentes, nous n'avons pas pu tester la résistance de ces serveurs. Cependant, Dell a placé la barre encore plus haut en fournissant un serveur Edge PowerEdge XR7620 et un conteneur de transport robuste, capables de résister à des conditions extrêmes.
La vie est dure au bord
L'infrastructure robuste en périphérie n'est pas un modèle nouveau. Les serveurs de périphérie existent depuis un certain temps déjà ; ils collectent des données et les renvoient au siège social pour en exploiter tout le potentiel. L'intelligence artificielle a mis l'accent sur un traitement plus rapide des données pour une utilisation quasi immédiate, ce qui implique un traitement à la source. L'IA en périphérie a eu un impact considérable, apportant un traitement plus innovant, plus rapide et plus sécurisé à de nombreuses applications concrètes. Un assistant personnel toujours à portée de main, prenant des décisions en temps réel, vous faisant gagner du temps, de l'argent et parfois même des vies !
Configuration Dell PowerEdge XR7620
Dell a présenté ces serveurs PowerEdge spécialement conçus l'année dernière. Le modèle que nous examinons est le Dell PowerEdge XR7620, un serveur robuste à double socket, optimisé pour la périphérie, à courte profondeur, spécialement conçu et compact, offrant des solutions axées sur l'accélération pour la périphérie. La différence entre le XR7620 et les serveurs Edge classiques est que ce serveur répond à la maturation rapide de l'IA/ML avec la prise en charge des charges de travail les plus exigeantes, notamment l'automatisation industrielle, l'analyse vidéo, l'analyse des points de vente, l'inférence IA et l'agrégation de périphériques Edge.
Notre XR7620 est configuré avec 2x Intel Xeon Gold 6426Y, 128 Go DDR5 4800, 480 Go Dell BOSS RAID1 et 4x SSD Solidigm. Nos tests auront lieu dans le laboratoire de recherche environnementale de Jordan au Canada. Nous avons donc acheté un disque de 60 To pour renvoyer les données au laboratoire Storagereview de Cincinnati.
Spécifications du Dell PowerEdge XR7620
| Fonctionnalité | Spécifications techniques | 
|---|---|
| Processeur | Deux processeurs Intel® Xeon® Scalable de 4e génération avec jusqu'à 32 cœurs par processeur | 
| Mémoire | 16 emplacements DIMM DDR5, prend en charge RDIMM 1 To maximum, vitesses jusqu'à 4800 5 MT/s. Prend en charge uniquement les DIMM DDRXNUMX ECC enregistrés | 
| Contrôleurs de stockage |  | 
| Baies de disques | Baies avant : jusqu'à 4 disques SSD SAS/SATA/NVMe de 2.5 pouces, 61.44 To maximum, jusqu'à 8 disques directs NVMe E3.S, 51.2 To maximum | 
| Alimentations |  | 
| Options de refroidissement | refroidissement par air | 
| Ventilateurs | Six ventilateurs de refroidissement échangeables à froid | 
| Dimensions |  | 
| Poids | Maximum 21.16 kg (46.64 livres) | 
| Facteur de forme | Serveur rack 2U | 
| Gestion intégrée | iDRAC9, iDRAC Direct, iDRAC RESTfulAPI avec Redfish, module de service iDRAC | 
| Biseau | Cadre de sécurité en option avec filtre à poussière (capteur de poussière disponible uniquement pour les systèmes de configuration à accès frontal) | 
| Logiciel OpenManage |  | 
| Mobilité | OuvrirGérer Mobile | 
| Intégrations OpenManage |  | 
| Sécurité |  | 
| Options de processeur graphique | Jusqu'à 5 x 75 W (simple largeur, pleine hauteur/demi-longueur, profil bas) GPU ou jusqu'à 2 x 300 W (double largeur, pleine hauteur/pleine longueur) | 
| Carte réseau intégrée | 2 x 1 GbE LOM | 
| Options réseau | 1 x carte OCP 3.0 (en option) | 
| Ports |  | 
| PCIe | Configuration à 2 processeurs : jusqu'à 5 emplacements PCIe (4 x16 Gen4/5, 1 x16 LP Gen4) | 
| Système d'exploitation et hyperviseurs |  | 
Dell PowerEdge XR7620 – Fonctionnalités Edge critiques
Sécurité
Comme tous les autres serveurs PowerEdge, le XR7620 est conçu dans un souci de sécurité à chaque étape du cycle de vie du serveur. Le processus de sécurité commence par une chaîne d'approvisionnement sécurisée avant la construction du serveur. Il s'étend aux serveurs livrés, avec Secure Lifecycle Management et Silicon Root of Trust, puis sécurise ce qui est créé/stocké par le serveur dans Data Protection.
Cette approche de sécurité zéro confiance suppose des autorisations d'accès privilégiées nécessitant une validation à chaque point d'accès et de mise en œuvre avec des fonctionnalités telles que la gestion des accès aux identités (IAM) et l'authentification multifacteur (MFA). Ceci est essentiel pour les déploiements en périphérie où les serveurs sont généralement installés dans des environnements peu sécurisés. Cette politique de sécurité stricte offre la capacité de détecter et de réagir en cas de falsification ou d'intrusion. La plateforme Root of Trust basée sur silicium de Dell crée un environnement sécurisé qui garantit que le micrologiciel provient d'une source fiable. Toute modification détectée dans les versions ou les configurations du micrologiciel peut forcer PowerEdge à verrouiller la configuration et à lancer une restauration vers le dernier bon environnement connu.
Direction
Tous les serveurs Dell PowerEdge suivent une approche de gestion de système standard à trois niveaux qui inclut le contrôleur d'accès à distance Dell intégré (iDRAC), OpenManage Enterprise (OME) et CloudIQ. Cette approche fournit une solution de gestion de système unifiée, simple, automatisée et sécurisée. Il vous permet de gérer un seul serveur à l'aide de la console iDRAC Baseboard Management Controller (BMC), de gérer des milliers de serveurs simultanément avec OME et d'utiliser des informations intelligentes sur l'infrastructure et des analyses prédictives pour maximiser la productivité du serveur avec CloudIQ.
Refroidissement
Nous avons abordé l'importance du refroidissement sur les performances et l'efficacité globales du serveur, en particulier en ce qui concerne l'IA. Ce n’est peut-être pas sexy, mais offrir des performances thermiques optimales est essentiel lors de la conception de serveurs Edge résilients et robustes. Les serveurs PowerEdge XR ont été conçus avec un flux d'air équilibré et efficace en matière de refroidissement et, si nécessaire, une gestion thermique complète qui optimise le flux d'air, régule la vitesse des ventilateurs et réduit la consommation d'énergie.
Ce niveau de détail permet aux serveurs PowerEdge XR de fonctionner entre -5 °C et 55 °C. Dell travaille sur des solutions susceptibles d'étendre encore davantage cette plage opérationnelle.
Tous les serveurs PowerEdge XR sont conçus avec plusieurs doubles ventilateurs contrarotatifs. Cela équivaut à placer deux ventilateurs dans le même boîtier. Il prend en charge la redondance des ventilateurs N+1, permettant au serveur de continuer à fonctionner avec une seule panne de ventilateur à une température maximale de 40 °C pendant au moins quatre heures.
Évolutivité
La capacité du serveur à héberger jusqu'à huit disques NVMe et la prise en charge de capacités d'accélération denses le positionnent bien pour les avancées futures dans les applications de données de pointe et d'inférence d'IA. Sa conception robuste et son système de refroidissement complet signifient également qu'il continuera à fonctionner efficacement à mesure que les charges de travail et les conditions environnementales évoluent.
Charges de travail ciblées
Les charges de travail ciblées incluent les charges de travail de fabrication numérique pour l'agrégation de machines, le VDI, l'inférence IA, la traduction OT/IT, l'automatisation industrielle, ROBO et les applications militaires où une conception robuste est requise. Le XR7620 profite au marché de détail et est conçu pour des applications telles que les opérations d'entrepôt, l'agrégation de points de vente, la gestion des stocks, la robotique et l'inférence IA.
Des serveurs comme ceux-ci sont déployés par l'armée, les sociétés d'exploration et d'extraction d'énergie, les télécommunications et d'autres ayant besoin de nœuds de périphérie dans les environnements les plus difficiles. La gamme de serveurs PowerEdge XR est un serveur 2U à 2 sockets de courte profondeur avec un calcul au niveau du centre de données qui offre des performances élevées, une capacité élevée et une latence réduite.
Concevoir et construire
Le Dell PowerEdge XR7620 se distingue par sa conception robuste et robuste. Il est conçu pour résister aux environnements difficiles et est souvent utilisé dans des environnements où la fiabilité est cruciale. Typique de tous les serveurs PowerEdge, le châssis XR7620 est bien construit, avec des matériaux de haute qualité pour relever divers défis.
En termes de conception, il s’agit d’un serveur compact et efficace, tirant le meilleur parti de l’espace disponible tout en conservant un excellent flux d’air pour le refroidissement.
Une caractéristique clé du XR7620 est la lunette intelligente. Il joue un rôle essentiel dans l'amélioration des fonctionnalités et de la sécurité du serveur Dell PowerEdge XR7620. Le cadre intelligent n'est pas seulement un ajout esthétique mais un composant essentiel qui contribue à l'efficacité et à la fiabilité globales du serveur.
L'une de ses principales fonctions est de fournir une interface visuelle pour surveiller et gérer l'état et les performances du serveur. Avec des indicateurs LED intégrés et un affichage clair et convivial, il offre des informations binaires en temps réel sur l'état général du système, la température, la consommation d'énergie, etc. Cette fonctionnalité est inestimable pour les professionnels de l'informatique, car elle leur permet d'identifier rapidement les serveurs et de résoudre les problèmes, garantissant ainsi un fonctionnement ininterrompu du serveur.
De plus, la lunette intelligente XR7620 constitue un atout de sécurité. Il permet de protéger les données sensibles et le serveur en fournissant une protection physique contre la falsification et les accès non autorisés. Les mécanismes de verrouillage et la détection des intrusions ajoutent une couche de défense supplémentaire à l'infrastructure du serveur.
Une autre caractéristique remarquable de la lunette intelligente XR7620 est sa capacité de filtrage. Ce mécanisme de filtrage va au-delà de l’esthétique ; il contribue activement aux performances et à la fiabilité du serveur. Le cadre comprend des filtres à air stratégiquement placés qui aident à maintenir un environnement propre et sans poussière au sein du châssis du serveur. Ces filtres aident à empêcher la poussière et autres grosses particules de pénétrer dans le serveur, ce qui est particulièrement crucial dans les environnements difficiles où le serveur peut être exposé à des particules en suspension dans l'air.
En gardant les composants internes exempts de poussière et de contaminants, le mécanisme de filtrage prolonge la durée de vie des composants critiques du serveur tels que les ventilateurs, les dissipateurs thermiques et les circuits. Il permet également de maintenir un flux d'air optimal pour le refroidissement, garantissant ainsi que le serveur fonctionne efficacement et avec un risque minimal de surchauffe.
Le mécanisme de filtrage du cadre intelligent XR7620 est un élément clé qui améliore la fiabilité du serveur en gardant les composants internes propres et bien ventilés. Cette fonctionnalité joue un rôle important pour garantir la longévité et le bon fonctionnement du serveur dans divers environnements opérationnels.
Le PowerEdge XR7620 est conçu pour fonctionner de manière fiable dans des conditions difficiles, allant des températures et humidités extrêmes aux environnements poussiéreux et aux chocs, et est certifié NEBS niveau 3, GR-3108 classe 1 et MIL-STD-810G.
Le XR7620 améliore les précédents serveurs PowerEdge XR2 et XE2420, avec des fonctionnalités de base similaires et les composants les plus récents, notamment :
- Une mise à niveau du processeur vers le processeur Intel Xeon Scalable de 4e génération récemment annoncé, avec jusqu'à 32 cœurs.
- 2x la bande passante mémoire avec la mise à niveau de DDR4 vers DDR5
- Capacités d'E/S plus performantes avec la mise à niveau de PCIe Gen 4 vers PCIe Gen 5, avec 5 emplacements PCIe.
- Capacités de stockage améliorées avec jusqu'à 8 disques NVMe, prise en charge BOSS et RAID NVMe basé sur le matériel.
- Des capacités d'accélération denses à la limite où le XR7620 excelle, avec la prise en charge de jusqu'à 2 accélérateurs double largeur (DW) jusqu'à 300 W chacun ou de quatre accélérateurs simple largeur (SW) jusqu'à 150 W chacun. Lunette filtrée intelligente pour travailler dans des environnements poussiéreux
Performances du Dell PowerEdge XR7620
Notre système d'évaluation est configuré avec les composants clés suivants pour les tests :
- 2 Xeon Gold 6426Y – 16 cœurs 2.5 GHz
- Nvidia L4
- 8 x 16GB DDR5
- BOSS RAID480 de 1 Go
- Dell PERC H755 PERC11
- Blocs d'alimentation 1400 XNUMX W
- Carte réseau Broadcom 2 x 10 Go
- Carte réseau Intel 4 x 10 Go
Geekbench 6 est une référence multiplateforme qui mesure les performances d'un système et fournit un score à des fins de comparaison. Il est conçu pour fonctionner sur plusieurs plates-formes et fournit une mesure de performances cohérente sur de nombreux appareils, des smartphones et tablettes aux ordinateurs de bureau et serveurs.
| Geekbench6 Multi | 16,507 | 
| Geekbench6 Unique | 1,864 | 
Cinebench est un outil d'analyse comparative largement utilisé qui mesure les performances des processeurs et des GPU utilisant Maxon Cinema 4D pour le rendu. Il fournit un score qui peut être utilisé pour comparer les performances de différents systèmes et composants. Nous avons inclus les résultats R23 et 2024.
| Cinebench R23 Multi | 46,544 | 
| Cinebench R23 Single | 1,292 | 
| Cinebench 2024 Multi | 2,383 | 
| Cinebench 2024 Single | 73 | 
y-cruncher 0.8.3.9522 est un programme multithread et évolutif qui peut calculer Pi et d'autres constantes mathématiques jusqu'à des milliards de chiffres. Depuis son lancement en 2009, elle est devenue une application d'analyse comparative et de test de résistance populaire auprès des overclockeurs et des passionnés de matériel.
| y-cruncher 1b (1 milliard de chiffres) | 9.805s | 
| y-cruncher 2.5b (2.5 milliard de chiffres) | 27.344s | 
| y-cruncher 5b (5 milliard de chiffres) | 60.564s | 
| y-cruncher 10b (10 milliard de chiffres) | 133.100s | 
| y-cruncher 25b (25 milliard de chiffres) | 383.261s | 
Le benchmark Blender mesure les performances de rendu 3D d'un CPU ou d'un GPU en rendant une scène 3D dans le logiciel Blender. Il fournit un score qui peut être utilisé pour comparer les performances de différents systèmes et composants.
|  | Processeur CLI Blender 3.6 uniquement | Processeur CLI Blender 4.0 uniquement | 
| Monster | 333.737716 | 312.455545 | 
| Brocanteur | 216.598456 | 213.333376 | 
| Salle de classe | 161.74015 | 161.812143 | 
| Total | 657.218016 | 687.601064 | 
Accélération GPU
Le XR7620 prend en charge jusqu'à 2 cartes accélératrices de 300 W pour les GPU afin de gérer les charges de travail exigeantes, offrant une classification d'images 45 % plus rapide que le serveur Dell XR 12 avec un seul accélérateur GPU de 300 W. La combinaison d'une faible latence et d'une puissance de traitement élevée permet une analyse des données plus rapide et plus efficace, permettant aux organisations de prendre des décisions en temps réel pour une monétisation accrue.
Pour tester les performances de l'inférence, nous avons exécuté l'inférence MLPerf 3.1, à la fois hors ligne et sur le serveur. BERT (Bidirectionnel Encoder Representations from Transformers) est un modèle basé sur un transformateur principalement utilisé pour les tâches de traitement du langage naturel telles que la réponse aux questions, la compréhension du langage et la classification des phrases. ResNet50 est un modèle de réseau neuronal convolutif (CNN) largement utilisé pour les tâches de classification d'images. Il s’agit d’une variante du modèle ResNet à 50 couches, connue pour son architecture profonde mais ses performances efficaces.
Afin de tester MLPerf 3.1, nous avons équipé notre XR7620 d'un seul GPU NVIDIA L4. La L4 est une carte très performante, conçue spécifiquement pour les charges de travail d’IA et d’apprentissage profond. Avec un énorme 24 Go de VRAM sur une carte mi-hauteur mi-longueur de 70 watts économe en énergie, le L4 est un accélérateur parfait pour ce serveur Edge robuste. Le L4, qui fait partie de la dernière génération de GPU pour centres de données Ada Lovelace de NVIDIA, est conçu pour offrir des performances exceptionnelles dans les tâches d'apprentissage automatique, ce qui en fait un choix idéal pour nos objectifs d'analyse comparative.
| Resnet50 – Hors ligne : | 13,010.2 | 
| Resnet50 – Serveur : | 12,204.4 | 
| Bert K99 – Hors ligne : | 973.465 | 
| Bert K99 – Serveur : | 898.945 | 
- Mode hors ligne : ce mode mesure les performances d'un système lorsque toutes les données sont disponibles pour un traitement simultané. Cela s’apparente au traitement par lots, dans lequel le système traite un grand ensemble de données en un seul lot. Ce mode est crucial pour les scénarios dans lesquels la latence n’est pas une préoccupation majeure, mais le débit et l’efficacité le sont.
- Mode serveur : en revanche, le mode serveur évalue les performances du système dans un scénario qui imite un environnement de serveur réel, dans lequel les requêtes arrivent une par une. Ce mode est sensible à la latence et mesure la rapidité avec laquelle le système peut répondre à chaque demande. C’est crucial pour les applications en temps réel où une réponse immédiate est nécessaire, comme dans les serveurs Web ou les applications interactives.
Notre XR7620, équipé de deux processeurs Xeon Scalable Gold de 4e génération, a parfaitement géré le cache L4, avec des résultats conformes à ceux validés par NVIDIA lors de son test d'inférence MLPerf 3.1 . Nos validations MLPerf 3.1 étant encore en cours, ce test servira de référence pour nos futurs tests.
Conclusion
Le Dell PowerEdge XR7620 est un serveur bien conçu et robuste connu pour sa durabilité et sa fiabilité. Sa conception réfléchie et sa qualité de fabrication en font un choix fiable pour les entreprises et les organisations qui ont besoin d'un calcul haute performance dans des environnements difficiles. Sa conception robuste et résiliente et ses avancées technologiques de pointe le positionnent comme un acteur redoutable dans des environnements difficiles. De plus, les options polyvalentes de connectivité et d’extension du serveur, combinées aux services d’assistance et de garantie fiables de Dell, garantissent qu’il répond non seulement, mais qu’il les dépasse, aux exigences des scénarios informatiques de pointe modernes.
Même si le prix est un facteur à prendre en compte, le Dell PowerEdge XR7620 offre un rapport qualité-prix indéniable, en particulier pour les secteurs exigeant une fiabilité et des performances sans faille dans des conditions extrêmes. Dans l’ensemble, le PowerEdge XR7620 est une solution complète conçue pour relever les complexités et les défis de l’informatique de pointe moderne, s’avérant être un investissement judicieux pour les entreprises tournées vers l’avenir.
Ensuite, nous poussons ce système à l’extrême pour un projet de recherche majeur, d’autres à venir !