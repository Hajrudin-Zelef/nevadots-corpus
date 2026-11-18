De nombreuses entreprises recherchent des alternatives aux serveurs montés en rack et les exigences physiques nécessaires à leur maintenance. Il est logique d'étudier les capacités des serveurs tour, car ils combinent la puissance et les performances des serveurs montés en rack avec les considérations de déploiement des petites entreprises. le serveur tour HPE ProLiant ML350 Gen11 est spécialement conçu pour répondre à ces besoins des PME ou des périphéries.
HPE a généreusement fourni une unité de révision afin que nous puissions la mettre à l'épreuve. Avant de passer aux résultats des tests, commençons par le contexte et les spécifications.
Spécifications du serveur HPE ProLiant ML350 Gen11
Une caractéristique unique du ML350 Gen11 est qu’il n’existe pas deux tours identiques. HPE a supprimé l'option de paramètre « par défaut » sur laquelle les clients pouvaient cliquer et commander. Au lieu de cela, HPE demande à tous ceux qui ont l'intention d'acheter ces bêtes de se présenter à la table en gardant à l'esprit leurs besoins et leurs spécifications afin de personnaliser une machine qui correspond précisément à la charge de travail qu'elle devra gérer.
La gamme potentielle de spécifications du ML350 Gen11 comprend :
| Processeur | Processeurs Intel® Xeon® Scalable de 4e/5e génération prenant en charge jusqu'à 64 cœurs | 
| Mémoire |  | 
| Contrôleurs de stockage | Contrôleur RAID trimode :  | 
| Baies de disques | Baies avant :  | 
| Alimentations |  | 
| Ventilateurs | Standard – 3 ventilateurs inclus | 
| Dimensions | 46.2 (H) x 71.2 (P) x 17.4 (L) cm 18.2 (H) x 28 (P) x 6.85 (L) po | 
| Facteur de forme | Tour 4U avec capacité de conversion en rack | 
| Gestion intégrée |  | 
| Utilitaires du serveur |  | 
| Sécurité |  | 
| Port réseau de gestion à distance HPE iLO | 1 Go dédié, arrière | 
| Options réseau | Aucun port natif n'est inclus. Choix d'OCP ou de carte stand-up | 
| Options GPU | Jusqu'à 8SW ou 4DW | 
| Ports |  Interne:  | 
| PCIe |  | 
| Système d'exploitation et hyperviseurs |  | 
Construction et conception HPE ML350 Gen 11
À première vue, la refonte unique du dernier serveur tour Gen11 de HPE ressemble à quelque chose que vous pourriez rencontrer dans l'horizon de Chicago, dominant le brouillard matinal qui s'étend sur le lac Michigan glacial.
Comme la plupart des serveurs tour, celui-ci se démarque dans la salle des serveurs. À 18 pouces de hauteur et plus de 2 pieds de profondeur, il sera nécessaire de prendre en compte l'emplacement pour la répartition de la chaleur et le contrôle du bruit.
Le système de ventilateurs à double profondeur est une fonctionnalité remarquable pour HPE en matière de distribution de chaleur. Il s’agit d’un système de ventilateur de refroidissement impressionnant que notre équipe a immédiatement remarqué et qui a été impressionnée lorsque nous avons ouvert le panneau latéral du serveur.
HPE livre le ML350 avec trois ventilateurs dans les configurations de base où il s'agit d'un remplacement non à chaud et jusqu'à huit ventilateurs remplaçables à chaud dans la configuration à double processeur de notre serveur.
Le HPE ProLiant ML350 Gen11 offre plusieurs configurations de fond de panier pour le stockage, notamment la prise en charge des SSD SFF, LFF et Gen5 E3.S. Notre machine de test était dotée d'une capacité de fond de panier pour HPE MR408i-o Gen11 x8 Lanes.
L’avant de la tour avec le capot avant s’est ouvert. Notre modèle dispose de 4 des 8 baies équipées de disques SATA.
Tout aussi personnalisable que le reste du système, l'arrière de la tour fournit 10 emplacements pour cartes d'extension PCIe, telles que les HBA, les GPU et les cartes réseau, avec des options sur la barre latérale pour configurer des interfaces réseau et des sorties vidéo spécifiques.
Gestion HPE ProLiant ML350 Gen11 – iLO 6
Le ML350 Gen 11 intègre iLO 6, la dernière version de HPE de sa technologie Lights-Out. iLO 6 apporte une série d'avantages (tels que la configuration de serveur à distance, la surveillance de l'état et le contrôle de l'alimentation et de la température) qui améliorent la gérabilité, la sécurité et l'efficacité du DL320 et simplifient les environnements informatiques complexes. Bien qu'iLO ne soit pas nouveau et soit couramment présent sur la plupart des serveurs HPE, nous le notons ici car il constitue une valeur ajoutée considérable pour ceux qui ne sont peut-être pas habitués à une gestion de serveur aussi approfondie.
Système d'information
Grâce à ILO, vous pouvez consulter des informations générales telles que les configurations du processeur, de la mémoire, du réseau et du stockage, ainsi que l'état de chacun.
Ensuite, dans l'onglet Processeur de l'OIT, se trouve l'onglet Mémoire. Vous pouvez voir l'état et la configuration de votre DRAM, jusqu'au niveau de l'emplacement, ainsi que la fréquence à laquelle chaque dimm fonctionne.
À la fin de la section Informations système se trouve l'onglet Stockage. Ici, vous pouvez voir l'état de vos contrôleurs de stockage et des lecteurs qui y sont connectés. Ces informations peuvent aider à repérer les disques défectueux dans votre configuration en cas de panne.
Puissance et thermique
Dans l'onglet Alimentation et thermique, nous pouvons voir la lecture de la puissance et l'état de l'alimentation sur la configuration du serveur. Les informations plus détaillées sur la puissance se trouvent sous l'onglet du compteur de puissance, mais cela nécessite une licence OIT payante non incluse avec ce système pour que nous puissions l'essayer.
L'OIT contient un onglet d'informations sur la température sous alimentation et thermique. Ces informations vous montrent, en fonction de chaque capteur de température sur un graphique 3D, où se trouvent vos points chauds dans le système. Il est important de surveiller les données de température pour garantir la stabilité et la durée de vie de votre matériel.
Micrologiciel et logiciel du système d'exploitation
Un énorme avantage de l'OIT est de pouvoir installer à distance des systèmes d'exploitation et des micrologiciels sur l'appareil sans avoir à mettre la main dessus. Ces intégrations à la gestion à distance permettent un gain de temps considérable puisque vous n'avez pas à vous soucier de l'entrée vidéo et périphérique directement sur l'appareil.
Performances HPE ProLiant ML350 Gen 11
Vérifier la configuration
- 2 processeurs Intel Xeon 8480+
- Mémoire 256GB DDR5
- Windows Server 2022
Test de vitesse Blackmagic RAW
Nous avons commencé à exécuter le test de vitesse RAW de Blackmagic, qui teste la lecture vidéo. Il s’agit plutôt d’un test hybride incluant les performances du CPU et du GPU pour le décodage RAW réel. Bien qu'il n'y ait pas d'autres machines HPE pour montrer une comparaison directe ici, pour vous donner une idée de la situation de celle-ci, les autres serveurs ne produisent généralement qu'à 50-60 ips.
| Test de vitesse Blackmagic RAW (Plus c'est haut, mieux c'est) | HPE ML350 Gen11 (Double Intel Xeon(r) Platinum 8480+, 112 cœurs, 2 GHz) | 
| CPU 8K | 136 | 
| CUDA 8K | N/D | 
Cinebench R23
Cinebench R23 de Maxon est une référence de rendu de processeur qui utilise tous les cœurs et threads de processeur. Nous l'avons exécuté pour des tests multicœurs et monocœurs. Des scores plus élevés sont meilleurs.
| Cinebench R23 | HPE ML350 Gen11 (Double Intel Xeon(r) Platinum 8480+, 112 cœurs, 2 GHz) | 
| Processeur (multicœur) (points) | 79164 | 
| Processeur (monocœur) (points) | 1461 | 
| Rapport PM | 54.20x | 
Cinebench 2024
Cinebench 2024 de Maxon est une référence de rendu CPU et GPU qui utilise tous les cœurs et threads du processeur. Nous l'avons exécuté pour des tests multicœurs et monocœurs. Comme cette configuration n'a pas de GPU, nous n'avons pas ces chiffres. Des scores plus élevés sont meilleurs.
| Cinebench 2024 | HPE ML350 Gen11 (Double Intel Xeon(r) Platinum 8480+, 112 cœurs, 2 GHz) | 
| Processeur (multicœur) (points) | 4,699 | 
| Processeur (monocœur) (points) | 76 | 
| Rapport PM | 61.44x | 
Évaluation du processeur Geekbench
Geekbench 6 est un outil d'évaluation multiplateforme mesurant les performances globales d'un système. Toutefois, il serait intéressant d'analyser les performances monocœur et multicœur, ainsi que les résultats du benchmark OpenCL. Un score élevé indique de meilleures performances. Précisons que nous n'avons examiné que les résultats du processeur, ce serveur ne disposant pas de carte graphique.
Vous pouvez trouver des comparaisons avec n'importe quel système dans le navigateur Geekbench.
| Geekbench 6 | HPE ML350 Gen11 (Double Intel Xeon(r) Platinum 8480+, 112 cœurs, 2 GHz) | 
| CPU Benchmark - Monocœur | 1,939 | 
| Référence CPU - Multi-Core | 15,218 | 
| Référence GPU – OpenCL | N/D | 
croque-y
y-cruncher est un programme multi-thread et évolutif qui peut calculer Pi et d'autres constantes mathématiques jusqu'à des billions de chiffres. Depuis son lancement en 2009, il est devenu une application populaire d'analyse comparative et de test de résistance pour les overclockeurs et les passionnés de matériel.
| croque-y (Temps de calcul total) | HPE ML350 Gen11 (Double Intel Xeon(r) Platinum 8480+, 112 cœurs, 2 GHz) | 
| 1 milliard de chiffres (secondes) | 5.136 | 
| 2.5 milliard de chiffres (secondes) | 29.889 | 
| 10 milliard de chiffres (secondes) | 65.194 | 
| 25 milliards de chiffres (secondes) | 186.841 | 
| 50 milliards de chiffres (secondes) | 413.722 | 
Compression à 7 zips
L'utilitaire populaire 7-Zip dispose d'un test de mémoire intégré qui démontre très bien les performances du processeur. Dans ce test, nous l'exécutons avec une taille de dictionnaire de 128 Mo lorsque cela est possible.
|  | HPE ML350 Gen11 (Double Intel Xeon(r) Platinum 8480+, 112 cœurs, 2 GHz) | 
|---|---|
| Compression |  | 
| Utilisation actuelle du processeur | 5,482 % | 
| Courant nominal/utilisation | 4.628 GIPS | 
| Courant | 253.724 GIPS | 
| Utilisation résultante du processeur | 5,475 % | 
| Évaluation/utilisation résultante | 4.628 GIPS | 
| Note résultante | 253.382 GIPS | 
| Décompression |  | 
| Utilisation actuelle du processeur | 6,219 % | 
| Courant nominal/utilisation | 3.745 GIPS | 
| Courant | 231.916 GIPS | 
| Utilisation résultante du processeur | 6,129 % | 
| Évaluation/utilisation résultante | 3.871 GIPS | 
| Note résultante | 237.259 GIPS | 
| Note totale |  | 
| Utilisation totale du processeur | 5,802 % | 
| Note totale/utilisation | 4.249 GIPS | 
| Note totale | 245.320 GIPS | 
Inférence UL Procyon AI
La suite de tests d' inférence IA Procyon de UL évalue les performances de différents moteurs d'inférence IA utilisant des réseaux neuronaux de pointe. Ces tests ont été exécutés uniquement sur le processeur. Chaque valeur représente un temps d'inférence moyen (plus la valeur est basse, meilleures sont les performances), et la dernière ligne indique un score global (plus la valeur est élevée, meilleures sont les performances).
|  | HPE ML350 Gen11 (Double Intel Xeon(r) Platinum 8480+, 112 cœurs, 2 GHz) | 
|---|---|
| Mobile Net V3 | 2.34 | 
| ResNet 50 | 5.76 | 
| Création V4 | 21.70 | 
| Deep Lab V3 | 23.00 | 
| YOLO V3 | 30.81 | 
| RÉEL-ESRGAN | 1535.27 | 
| Note globale | 191 | 
Conclusion
Dans l'ensemble, le HPE ProLiant ML350 Gen11 arrive sur le marché avec une multitude d'options de personnalisation qui séduiront toute entreprise cherchant à mettre à niveau les capacités de son serveur sans sacrifier l'espace physique qui serait généralement requis pour un système monté en rack. Lorsque nous avons commencé l'examen, ce système prenait en charge Intel Xeon Scalable de 4e génération, mais au cours du processus, ils ont également récupéré les puces de 5e génération, apportant encore plus de puissance à cette plate-forme.
De plus, ce serveur peut accueillir plus de deux douzaines de SSD NVMe hot-plug pour une capacité allant jusqu'à 368 To, 32 emplacements DIMM capables d'exécuter jusqu'à 8 To de RAM DDR5, une tonne d'extension PCIe et un système de ventilateur impressionnant à conserver. tout est cool et fonctionne efficacement. Les serveurs tour sont souvent négligés, car ils sont conçus « uniquement pour les PME ». Le HPE ProLiant ML350 Gen11 est un autre excellent rappel que les tours peuvent être bien plus encore ; ils font partie des systèmes les plus diversifiés et personnalisables disponibles pour répondre à l’ensemble de cas d’utilisation de plus en plus diversifiés d’aujourd’hui.