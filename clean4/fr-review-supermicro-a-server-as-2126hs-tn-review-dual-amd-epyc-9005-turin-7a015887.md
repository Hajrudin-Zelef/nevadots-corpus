Nous avons récemment reçu le serveur Supermicro AS-2126HS-TN, un serveur A+ biprocesseur basé sur l'architecture Turin, pour test. Cette plateforme 2U est conçue pour prendre en charge des charges de travail extrêmement gourmandes en ressources et offre une extension PCIe flexible. Basé sur deux processeurs AMD EPYC séries 9005 (Turin) et 9004 (Genoa), ce système cible un large éventail de cas d'utilisation en entreprise et en centre de données, notamment la virtualisation, le stockage défini par logiciel, l'inférence IA et l'apprentissage automatique, le cloud computing, la consolidation de serveurs d'entreprise et le calcul haute performance (HPC).
La carte AS-2126HS-TN prend en charge plusieurs configurations d'emplacements PCIe, permettant ainsi de privilégier la densité d'accélérateurs ou la flexibilité des E/S. Selon la configuration, la plateforme peut accueillir jusqu'à quatre emplacements PCIe 5.0 x16, jusqu'à huit emplacements PCIe 5.0 x8, ou des configurations mixtes pour intégrer des GPU, des contrôleurs réseau haut débit et des contrôleurs de stockage. Cette approche modulaire permet au système de s'adapter aux charges de travail exigeantes en calcul, en E/S ou axées sur les accélérateurs, sans nécessiter de modifications du châssis.
Pour évaluer la plateforme, nous avons soumis l'AS-2126HS-TN à notre environnement de tests d'entreprise, en nous concentrant sur des charges de travail gourmandes en ressources CPU reflétant des scénarios de déploiement réels. Nos tests mettent l'accent sur les performances multithread soutenues, le comportement de la mémoire et l'efficacité globale de la plateforme, offrant ainsi un aperçu des performances de l'architecture Turin de Supermicro dans des conditions exigeantes d'entreprise et de calcul haute performance (HPC).
Spécifications du Supermicro AS-2126HS-TN
Le tableau ci-dessous présente les spécifications matérielles du Supermicro AS-2126HS-TN, offrant un aperçu de sa conception de plateforme, de ses capacités de calcul, de ses options d'extension et de ses caractéristiques d'alimentation et de refroidissement.
| Spécifications | Serveur Supermicro A+ AS -2126HS-TN | 
|---|---|
| Présentation du système |  | 
| Produit REF | Serveur A+ AS -2126HS-TN | 
| Carte mère | Super H14DSH | 
| Facteur de forme | Montage en rack 2U | 
| Modèle de châssis | CSE-HS201-R000NFP | 
| Processeurs |  | 
| Support de CPU | Processeur double ; processeurs AMD EPYC™ série 9005/9004 | 
| Nombre maximal de cœurs | Jusqu'à 384 °C / 768 °C | 
| TDP maximal du processeur | Prend en charge les processeurs jusqu'à 500 W de TDP* | 
| Note sur le refroidissement du processeur | Les processeurs refroidis par air avec un TDP supérieur à 400 W ne sont pris en charge que dans des conditions spécifiques. | 
| GPU et accélération |  | 
| Nombre maximal de GPU | Jusqu'à 3 GPU double largeur | 
| GPU pris en charge (PCIe) | NVIDIA : H100 NVL, RTX 6000 génération Ada, L4 AMD : Instinct™ MI210 | 
| Interconnexion GPU-GPU | PCIe | 
| Mémoire |  | 
| Emplacements DIMM | 24 emplacements DIMM | 
| Mémoire maximale (1DPC, EPYC 9005) | Jusqu'à 6 To de mémoire RDIMM ECC DDR5 à 4 800 MT/s | 
| Mémoire maximale (1DPC, EPYC 9004) | Jusqu'à 6 To de mémoire RDIMM ECC DDR5 à 4 800 MT/s | 
| Tension de mémoire | 1.1V | 
| Dispositifs embarqués et réseau |  | 
| Chipset | Système sur puce | 
| Connectivité réseau | Via AIOM (options AOC disponibles) | 
| BMC / IPMI | IPMI 2.0 avec prise en charge de Virtual Media over LAN et KVM-over-LAN | 
| Entrée / Sortie |  | 
| LAN (BMC) | 1 port LAN BMC dédié RJ45 1GbE | 
| USB | 2 ports USB 3.0 (arrière) | 
| Vidéo | 1×VGA | 
| TPM | 1 TPM embarqué / port 80 | 
| BIOS |  | 
| Type de BIOS | AMI Flash EEPROM 64 Mo SPI | 
| Fonctionnalités du BIOS | Plug and Play (PnP) ; UEFI 2.8 ; prise en charge des claviers USB ; ACPI 6.5 ; SMBIOS 3.7 ou version ultérieure | 
| Direction |  | 
| Logiciels / Outils | SuperCloud Composer® ; Supermicro Server Manager (SSM) ; Super Diagnostics Offline (SDO) ; KVM avec LAN dédié ; IPMI 2.0 ; Service d’agent léger Supermicro (TAS) ; Assistant d'automatisation SuperServer (SAA) ; IPMIView | 
| Configurations d'alimentation | Gestion de l'alimentation ACPI/APM ; contrôle du mode de mise sous tension pour la récupération après une coupure de courant alternatif ; mécanisme de contournement du bouton d'alimentation | 
| Sécurité |  | 
| Sécurité matérielle | TPM 2.0 ; Racine de confiance en silicium (RoT) – Conforme à la norme NIST 800-193 | 
| Caractéristiques de sécurité | Micrologiciel signé cryptographiquement ; Démarrage sécurisé ; Mises à jour sécurisées du micrologiciel ; Récupération automatique du micrologiciel ; Sécurité de la chaîne d'approvisionnement (attestation à distance) ; protections BMC en temps réel ; verrouillage du système | 
| Surveillance de la santé PC |  | 
| Surveillance du ventilateur | Surveillance du tachymètre ; état du régulateur de vitesse ; connecteurs de ventilateur PWM | 
| Surveillance de la température | Surveillance de l'environnement du processeur et du châssis ; contrôle thermique des connecteurs de ventilateurs | 
| Tension / Capteurs | Température du système ; Température de la mémoire ; Température du processeur ; Veille 3.3 V ; Veille +5 V ; +5 V ; +3.3 V ; +12 V ; Protection contre la surchauffe du processeur | 
| Panneau avant |  | 
| Voyants | Activité du disque dur ; activité du réseau local ; état de l’alimentation ; informations système | 
| Boutons | Marche/Arrêt ; UID | 
| Extension et interconnexion |  | 
| Configuration des emplacements PCIe | Option A*: 4 emplacements PCIe 5.0 x16 FHFL double largeur ; 1 emplacement PCIe 5.0 x16 AIOM (compatible OCP 3.0) Option B*: 8 emplacements PCIe 5.0 x8 (dans x16) FHFL ; 1 module PCIe 5.0 x16 AIOM (compatible OCP 3.0) *Nécessite des pièces supplémentaires ; voir la liste des pièces optionnelles. Reportez-vous aux schémas du système pour plus de détails. | 
| Assistance CXL | Jusqu'à 4 appareils CXL 2.0 x16 | 
| Stockage |  | 
| Baies de disques (par défaut) | 8 baies au total ; 8 baies avant remplaçables à chaud pour disques NVMe*/SATA* de 2.5 pouces | 
| Baies de disques (Option A) | 24 baies au total ; 24 baies avant remplaçables à chaud pour disques NVMe*/SATA* de 2.5 pouces | 
| M.2 | 2 × M.2 PCIe 3.0 x4 NVMe (clé M 2280/22110) | 
| Remarque sur le stockage | La prise en charge NVMe/SATA peut nécessiter un contrôleur de stockage et/ou des câbles supplémentaires. | 
| Refroidissement |  | 
| Ventilateurs | Jusqu'à 6 ventilateurs contrarotatifs de 60×60×56 mm | 
| Suaire d'air | 2 enveloppes d'air | 
| Tuning Moteur |  | 
| Options d'alimentation | 2× 1200W / 1300W / 1600W / 2000W / 2600W (varie selon la configuration) | 
| Alimentation incluse/mentionnée | 2 × 2000 W redondants (1+1) Niveau Titane (96 %) | 
| Dimensions du bloc d'alimentation (L×H×P) | 73.5 × 40 x 265 mm | 
| Entrée (variable selon l'alimentation) | 1000 W : 100–127 Vca / 50–60 Hz 1800 W : 200–220 Vca / 50–60 Hz 2700 W : 200–240 Vca 1980 W : 220–230 Vca / 50–60 Hz 2000 W : 220–240 Vca / 50–60 Hz (homologué UL uniquement) 2000 W : 230–240 Vca / 50–60 Hz 2000 W : 230–240 Vcc / 50–60 Hz (CQC uniquement) | 
| Rail +12V (varie selon l'entrée) | Max 83 A (100–127 Vca) / Max 150 A (200–220 Vca) / Max 225 A (200–240 Vca) Max 165 A (220–230 Vca) / Max 166 A (230–240 Vca) | 
| SB 12V | Max. 3.5 A / Min. 0 A | 
| Type de sortie | Fond de panier (doigt d'or) | 
| Dimensions et poids |  | 
| Hauteur | 3.5 mm (88.9 po) | 
| Largeur | 17.2 mm (437 po) | 
| Profondeur | 31.74 mm (806.2 po) | 
| Emballage (H×L×P) | 9.96 "× 26.46" × 43.31 " | 
| Poids | Poids brut : 34 kg (75 lb) ; Poids net : 20.5 kg (45 lb) | 
| Couleur disponible | un Prix d'argent | 
Conception et montage du Supermicro AS-2126HS-TN
Le serveur Supermicro A+ AS-2126HS-TN est une plateforme 2U double socket compacte, conçue pour les configurations à très grand nombre de cœurs, processeurs à TDP élevé et accélérateurs multiples. Son architecture interne optimise la circulation de l'air de l'avant vers l'arrière, assurant un équilibre entre efficacité thermique, évolutivité de la consommation et facilité de maintenance pour les sous-systèmes de calcul, de mémoire, de stockage et d'extension.
Partie avant : Rangement et prise d'air
La face avant du châssis est conçue pour un stockage haute densité et une circulation d'air optimale. Selon la configuration, le système prend en charge jusqu'à 8 ou 24 baies pour disques NVMe, offrant une connectivité PCIe directe pour les charges de travail de stockage hautes performances. Ces baies sont positionnées en amont du flux d'air principal, garantissant ainsi une distribution homogène de l'air frais entrant avant qu'il n'atteigne les zones de calcul et d'extension.
Cette configuration permet au stockage de fonctionner indépendamment des températures du processeur et de l'accélérateur, maintenant ainsi des performances constantes sous une charge d'E/S soutenue tout en minimisant les perturbations du flux d'air à l'intérieur du châssis.
Architecture de refroidissement du châssis central
Juste derrière le compartiment de stockage avant se trouve un ensemble de six ventilateurs internes haute performance remplaçables à chaud. Ces ventilateurs créent un canal de refroidissement direct, générant une pression statique suffisante pour alimenter deux processeurs AMD EPYC d'une enveloppe thermique allant jusqu'à 500 W et plusieurs cartes graphiques PCIe double largeur.
La vitesse des ventilateurs est contrôlée dynamiquement via des connecteurs PWM et surveillée par des tachymètres intégrés, tandis que la gestion thermique s'effectue au niveau système. Cette conception permet à la plateforme d'adapter son refroidissement en temps réel en fonction des températures du processeur, de la mémoire et du châssis, garantissant ainsi un fonctionnement stable pour une large gamme de charges de travail.
Cœur de calcul : processeurs et mémoire
Au cœur du système se trouve la carte mère Super H14DSH, compatible avec deux processeurs AMD EPYC séries 9005 ou 9004. Prenant en charge jusqu'à 384 cœurs et 768 threads, l'AS-2126HS-TN est parfaitement adapté aux charges de travail hautement parallèles telles que la virtualisation, le calcul haute performance (HPC) et l'inférence en intelligence artificielle.
La plateforme prend en charge les processeurs dont le TDP atteint 500 W, avec un refroidissement par air possible au-delà de 400 W sous certaines configurations thermiques. Chaque processeur est associé à un ensemble complet de canaux mémoire alimentant 24 emplacements DIMM DDR5, prenant en charge jusqu'à 6 To de mémoire ECC DDR5 RDIMM en configuration 1DPC. La vitesse de la mémoire évolue avec la génération du processeur, atteignant jusqu'à 6 400 MT/s avec les processeurs EPYC 9005 et 4 800 MT/s avec les processeurs EPYC 9004.
Cette disposition symétrique de la mémoire minimise la latence et assure un flux d'air équilibré sur toutes les barrettes DIMM, même dans des configurations entièrement occupées.
Stockage de démarrage et connectivité embarquée
Pour le système d'exploitation et le stockage de gestion, le système comprend deux emplacements M.2 PCIe 3.0 NVMe intégrés, isolés du fond de panier NVMe principal. Cette séparation permet aux administrateurs de dédier les baies avant au stockage d'applications ou de données tout en préservant l'indépendance thermique et logique du support de démarrage.
La gestion du réseau s'effectue via un emplacement PCIe 5.0 x16 AIOM (OCP 3.0), permettant des configurations réseau flexibles sans monopoliser les emplacements d'extension PCIe standard. Cette approche préserve l'espace PCIe disponible pour les accélérateurs ou les contrôleurs de stockage supplémentaires.
Extension et prise en charge GPU
La partie arrière du châssis est dédiée à l'extension PCIe et à la prise en charge des accélérateurs. La carte mère AS-2126HS-TN prend en charge jusqu'à trois GPU PCIe double largeur, ce qui la rend idéale pour les environnements de calcul intensifs. Parmi les accélérateurs compatibles figurent les GPU NVIDIA PCIe tels que les H100 NVL, RTX 6000 Ada Generation et L4, ainsi que les accélérateurs AMD Instinct MI210.
Tous les emplacements d'extension sont PCIe Gen 5 et agencés de manière à garantir une ventilation optimale et une stabilité mécanique parfaite pour les cartes pleine hauteur et pleine longueur. La communication entre les GPU s'effectue via PCIe, ce qui assure la flexibilité et la compatibilité de la plateforme avec tous les fournisseurs, tout en prenant en charge un large éventail de configurations d'accélérateurs.
E/S arrière et gestion
Le panneau d'E/S arrière centralise l'accès à la gestion locale et hors bande. Il comprend un port LAN IPMI 1 GbE dédié, deux ports USB 3.0 et une sortie VGA pour un accès direct à la console. La gestion s'effectue via IPMI 2.0, avec prise en charge KVM sur LAN et des supports virtuels intégrée à la suite logicielle de gestion Supermicro, qui inclut SuperCloud Composer, Supermicro Server Manager et IPMIView.
Distribution de l'énergie et flexibilité électrique
L'alimentation est assurée par des blocs d'alimentation redondants à haut rendement (titane) disponibles en configurations de 1 200 W, 1 300 W, 1 600 W, 2 000 W et 2 600 W. Cette large gamme d'alimentations permet à la plateforme d'évoluer des configurations à forte densité de processeurs aux systèmes entièrement équipés de GPU, sans surdimensionnement ni sous-dimensionnement de la capacité d'alimentation.
Ce système prend en charge les tensions d'entrée de 120 V CA, 240 V CA et 48-60 V CC, ce qui le rend adapté aux baies d'entreprise traditionnelles, aux centres de données haute densité et aux environnements de télécommunications. La gestion de l'alimentation ACPI/APM intégrée permet une reprise contrôlée après une coupure de courant, un comportement configurable à la mise sous tension et une fonction de priorité sur le bouton d'alimentation. Grâce à sa conception à alimentation redondante, la plateforme offre à la fois une efficacité énergétique optimale et une grande fiabilité opérationnelle sous charge soutenue.
Sécurité, surveillance et fiabilité
Le contrôleur AS-2126HS-TN intègre un cadre de sécurité et de surveillance complet, comprenant TPM 2.0, une racine de confiance matérielle, le démarrage sécurisé, un firmware signé cryptographiquement et la restauration automatique du firmware. Les protections BMC en cours d'exécution et les fonctions de verrouillage du système contribuent à garantir l'intégrité de la plateforme tout au long de son cycle de vie.
La surveillance de l'état du système couvre le processeur, la mémoire, les lignes d'alimentation, la vitesse des ventilateurs et la température du châssis, avec une télémétrie en temps réel intégrée aux outils de gestion de Supermicro. Ceci permet une surveillance proactive et une réponse rapide aux anomalies thermiques ou d'alimentation dans les environnements de production.
Gestion Supermicro (BMC)
Le routeur AS-2126HS-TN est livré avec le BMC intégré de Supermicro, une plateforme que nous avons déjà présentée pour plusieurs systèmes A+, et qui demeure un élément essentiel de la gestion quotidienne. Le tableau de bord offre une vue consolidée de l'état et de la configuration du système, permettant une visibilité immédiate sur l'état du firmware, l'inventaire matériel et les données de télémétrie des capteurs. Depuis la page d'accueil, les administrateurs peuvent rapidement vérifier les versions du BMC, du BIOS, du CPLD et de Redfish, confirmer la configuration réseau et valider l'identité de l'hôte avant de procéder à des opérations de dépannage ou de maintenance plus approfondies.
Dans la section « Informations sur les composants », le BMC affiche avec précision les processeurs AMD EPYC installés, notamment les deux processeurs 192 cœurs présents dans le système. Chaque socket est équipé d'un AMD EPYC 9965, doté de 192 cœurs actifs et de 384 threads par processeur, avec un TDP configuré de 500 W. Cette vue permet de vérifier rapidement la configuration des processeurs et les autres caractéristiques des composants directement depuis le BMC, garantissant ainsi leur fonctionnement optimal.
Dans l'onglet Refroidissement des informations sur les composants, le BMC permet de contrôler quatre modes de ventilation : Vitesse standard, Vitesse maximale, Vitesse optimale et Vitesse pour activités E/S intensives. Ces préréglages permettent d'ajuster rapidement le flux d'air en fonction de la charge de travail et des besoins thermiques, sans modifier le BIOS. Sous la sélection du mode, l'interface affiche l'état de chaque ventilateur et sa vitesse de rotation en temps réel, offrant ainsi une vue claire de l'état du refroidissement du système.
Pour l'accès à distance, le BMC de l'AS-2126HS-TN intègre une console KVM accessible via une interface HTML5 ou un plug-in Java. Les paramètres de la console permettent de sélectionner le mode souris de base pour s'adapter aux différents systèmes d'exploitation, et une option de réinitialisation IKVM est disponible en cas de blocage de la session. Ceci offre un accès complet à la console hors bande pour l'installation, le dépannage et la restauration, sans nécessiter de périphériques locaux.
Tests de performance du Supermicro AS-2126HS-TN
Pour les tests, la carte mère Supermicro AS-2126HS-TN n'a pas été configurée avec une architecture de stockage dense ou une alimentation et un câblage optimisés pour les GPU. Par conséquent, notre évaluation s'est concentrée exclusivement sur les performances du processeur, en utilisant les benchmarks Blender, y-cruncher et Phoronix pour caractériser le débit de calcul brut sur la plateforme AMD EPYC biprocesseur.
Le serveur AS-2126HS-TN était équipé de deux processeurs AMD EPYC 9965 à 192 cœurs, offrant un total de 384 cœurs et 768 threads. À titre de comparaison, les résultats sont obtenus avec ceux d'un serveur Dell PowerEdge R7725 configuré avec les mêmes deux processeurs AMD EPYC 9965 et 1.5 To de mémoire système. Les deux systèmes ont été testés avec les mêmes suites de tests et paramètres afin de garantir la cohérence des résultats.
Supermicro Configuration du serveur A+ AS -2126HS-TN
- CPU: 2x AMD EPYC 9965 (192 cœurs)
- RAM: 1.5 To 24 x 64 Go DDR5 6000 MHz
- SSD SSD NVMe Micron 7.68 To pour centres de données
Mixeur 4.5
Blender est une application de modélisation 3D open source. Ce benchmark a été réalisé avec l'utilitaire Blender Benchmark. Le score est mesuré en échantillons par minute, les valeurs les plus élevées indiquant de meilleures performances.
Dans le benchmark CPU SMT de Blender, le serveur Supermicro A+ AS-2126HS-TN, équipé de deux processeurs AMD EPYC 9965, affiche d'excellents résultats, illustrant les gains de performance d'une architecture à 192 cœurs pour les charges de travail de rendu multithreadées. Avec l'utilitaire Blender Benchmark, le système a enregistré 3 070,84 échantillons par minute pour Monster, 2 063,61 pour Junkshop et 1 527,39 pour Classroom.
À titre de comparaison, le Dell PowerEdge R7725, équipé des mêmes deux processeurs EPYC 9965, affiche un débit légèrement supérieur dans ce scénario avec SMT activé, atteignant 3 193,11 échantillons par minute dans Monster, 2 174,63 dans Junkshop et 1 608,79 dans Classroom. L’écart relativement faible entre les deux plateformes suggère un comportement globalement similaire sous des charges de rendu intensives en SMT, les différences étant probablement dues à l’optimisation au niveau de la plateforme plutôt qu’à la puissance de calcul brute.
| SMT du processeur Blender (Échantillons par minute ; plus c'est élevé, mieux c'est) | Serveur Supermicro A+ AS -2126HS-TN (AMD EPYC 9965 192C) | Dell PowerEdge R7725 (double processeur AMD EPYC 9965 192C) | 
|---|---|---|
| Monster | 3,070.84 | 3,193.11 | 
| Brocanteur | 2,063.61 | 2,174.63 | 
| Salle de classe | 1,527.39 | 1,608.79 | 
Avec la technologie SMT désactivée, le système Supermicro affiche une augmentation notable du débit brut dans les trois scènes. Les performances atteignent 4 018,10 échantillons par minute dans Monster, 2 707,10 dans Junkshop et 1 990,51 dans Classroom, démontrant ainsi comment le moteur de rendu CPU de Blender peut tirer parti d'une réduction des conflits entre les threads sur les systèmes à très grand nombre de cœurs.
Dans cette configuration, le Dell PowerEdge R7725 affiche toujours un débit absolu supérieur, avec respectivement 4 304,21, 2 870,34 et 2 079,79 échantillons par minute dans les environnements Monster, Junkshop et Classroom. Bien que le système Dell conserve un avantage en termes de performances maximales, le Supermicro AS-2126HS-TN présente des gains substantiels avec le SMT désactivé, ce qui souligne l'influence significative de la stratégie de multithreading sur les performances des plateformes de cette envergure.
| Processeur Blender sans SMT (Échantillons par minute ; plus c'est élevé, mieux c'est) | Serveur Supermicro A+ AS -2126HS-TN (AMD EPYC 9965192C) | Dell PowerEdge R7725 (double processeur AMD EPYC 9965 192C) | 
|---|---|---|
| Monster | 4,018.10 | 4,304.21 | 
| Brocanteur | 2,707.10 | 2,870.34 | 
| Salle de classe | 1,990.51 | 2,079.79 | 
croque-y
y-cruncher est un programme multithread et évolutif capable de calculer Pi et d'autres constantes mathématiques jusqu'à des milliers de milliards de chiffres. Depuis son lancement en 2009, il est devenu une application de benchmarking et de test de résistance populaire auprès des overclockeurs et des passionnés de matériel informatique.
Dans le benchmark y-cruncher, le Supermicro AS-2126HS-TN démontre une mise à l'échelle constante et prévisible à mesure que la taille des problèmes augmente. Le système effectue le calcul d'un milliard de décimales en 8.092 secondes, puis passe à 15.245 secondes pour 2.5 milliards de décimales et à 24.961 secondes pour 5 milliards de décimales.
À mesure que la charge de travail augmente, les temps de calcul évoluent de manière linéaire, atteignant 44.350 secondes pour 10 milliards de chiffres, 114.107 secondes pour 25 milliards et 246.688 secondes pour 50 milliards. Lors du calcul le plus exigeant, portant sur 100 milliards de chiffres, le système effectue l'opération en 572.800 secondes, démontrant ainsi sa capacité à gérer un nombre élevé de threads sur de longues périodes d'exécution.
Le Dell PowerEdge R7725 exécute les mêmes charges de travail légèrement plus rapidement sur toutes les tailles de test, terminant l'exécution de 100 milliards de chiffres en 481.207 secondes.
| Y-Cruncher (temps de calcul total) | Serveur Supermicro A+ AS -2126HS-TN (AMD EPYC 9965 192C) | Dell PowerEdge R7725 (double processeur AMD EPYC 9965 192C) | 
|---|---|---|
| 1 milliard | 8.092 s | 7.879 secondes | 
| 2.5 milliard | 15.245 s | 13.811 secondes | 
| 5 milliard | 24.961 s | 22.107 secondes | 
| 10 milliard | 44.350 | 40.111 secondes | 
| 25 milliard | 114.107 | 98.445 secondes | 
| 50 milliard | 246.688 | 211.567 secondes | 
| 100 milliard | 572.800 | 481.207 secondes | 
Points de repère Phoronix
Phoronix Test Suite est une plateforme d'analyse comparative automatisée et open source qui prend en charge plus de 450 profils de test et plus de 100 suites de tests via OpenBenchmarking.org. Elle gère l'ensemble du processus, de l'installation des dépendances à l'exécution des tests et à la collecte des résultats, ce qui la rend idéale pour les comparaisons de performances, la validation matérielle et l'intégration continue.
Bande passante de la mémoire de flux
Lors du test de bande passante mémoire Stream, le Supermicro AS-2126HS-TN atteint un débit soutenu de 807 766 Mo/s. Le Dell PowerEdge R7725, quant à lui, culmine à 883 312 Mo/s, ce qui indique une bande passante mémoire maximale légèrement supérieure. Toutefois, les deux systèmes se situent dans la plage de performances attendue pour les plateformes biprocesseurs EPYC 9005.
Compression à 7 zips
Pour la compression 7-Zip, le système Supermicro atteint 1 262 832 MIPS, démontrant ainsi d'excellentes performances en calcul entier et une mise à l'échelle multithread efficace. Le système Dell affiche 1 326 967 MIPS, ce qui représente un léger avantage pour cette charge de travail, tout en restant dans la même catégorie de performances globales.
Compilation du noyau
Lors de la compilation du noyau (allmod), le Supermicro AS-2126HS-TN exécute la tâche en 117.97 secondes, devançant ainsi le Dell PowerEdge R7725 qui réalise la même opération en 139.36 secondes. Ce résultat souligne l'efficacité de la plateforme Supermicro dans les scénarios de compilation parallèle courants dans les environnements de développement et d'intégration continue.
Serveur Web Apache
Le test de performance Apache montre que le système Supermicro traite 90 623,69 requêtes par seconde, tandis que le système Dell atteint 96 782,75 requêtes par seconde. Les deux systèmes affichent un débit élevé pour les serveurs web, la configuration Dell présentant toutefois un pic de requêtes légèrement supérieur.
Vérification OpenSSL
Lors des tests OpenSSL, le Supermicro AS-2126HS-TN atteint 3.55 To/s, ce qui témoigne d'un débit cryptographique important, adapté aux charges de travail exigeantes en matière de chiffrement. Le Dell PowerEdge R7725 va encore plus loin avec 4.41 To/s, indiquant des performances cryptographiques de pointe supérieures. Les deux plateformes offrent des performances largement supérieures aux besoins habituels des entreprises.
| Points de repère Phoronix | Serveur Supermicro A+ AS -2126HS-TN (AMD EPYC 9965 192C) | Dell PowerEdge R7725 (double processeur AMD EPYC 9965 192C) | 
|---|---|---|
| Discussions | 807,766.0 Mo / s | 883,312.0 Mo / s | 
| 7-ZIP | 1,262,832 MIPS | 1,326,967 XNUMX MIP/s | 
| Compilation du noyau (allmod) | 117.97 secondes | 139.356 secondes | 
| Apache (requêtes par seconde) | 90,623.69 XNUMX R/s | 96,782.75 XNUMX R/s | 
| OpenSSL | 3,545,769,484,910 XNUMX XNUMX XNUMX Vérifications | 4,409,642,672,307 XNUMX XNUMX XNUMX XNUMX Vérifications | 
Conclusion
Le serveur Supermicro A+ AS-2126HS-TN offre une plateforme Turin biprocesseur équilibrée, privilégiant la densité de calcul, l'évolutivité énergétique et l'extension PCIe flexible dans un format 2U. Compatible avec deux processeurs AMD EPYC série 9005, les configurations à TDP élevé et les configurations d'E/S flexibles, il est idéal pour les environnements d'entreprise, de cloud et de calcul haute performance (HPC) où la performance parallèle soutenue et la configurabilité sont essentielles.
Dans notre suite de tests d'entreprise, le AS-2126HS-TN a offert des performances constantes et prévisibles pour les charges de travail gourmandes en ressources CPU. Bien qu'il ait légèrement esquivé certains indicateurs de débit absolu, la plateforme Supermicro est restée compétitive, démontrant une excellente évolutivité et une grande efficacité pour le rendu, le calcul et les charges de travail mixtes. Ces résultats confirment que le AS-2126HS-TN constitue une base performante et flexible pour les déploiements à grand nombre de cœurs, notamment lorsque la polyvalence de la plateforme et la marge de puissance sont aussi importantes que les performances maximales des benchmarks.