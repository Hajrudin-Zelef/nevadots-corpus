La gamme PowerEdge de 17e génération de Dell est déjà bien implantée, et avec les R4715 et R5715, elle cible désormais plus spécifiquement les PME et les entreprises de taille intermédiaire. Ces deux serveurs monoprocesseurs reposent sur la même architecture AMD EPYC de 5e génération que l'ensemble de la gamme PowerEdge de 17e génération. Ils sont optimisés pour les organisations où le nombre de cœurs adapté, la gestion des licences et la simplicité d'utilisation priment sur le débit maximal. Le R4715 est un modèle 1U plus compact, conçu pour la virtualisation, les bases de données à grande échelle et les déploiements en périphérie de réseau. Le R5715 adopte un format 2U, offrant davantage de baies de disques et d'extension PCIe, et convient aux configurations où la capacité de stockage et les performances d'E/S sont essentielles.
Le serveur R4715 est conçu pour les organisations exécutant des charges de travail de virtualisation, de bases de données à grande échelle et de calcul en périphérie, pour lesquelles l'efficacité des licences et la simplicité d'utilisation sont essentielles. Notre modèle de test était équipé d'un processeur AMD EPYC 9335, le processeur haut de gamme à 32 cœurs disponible sur cette plateforme, associé à 384 Go de mémoire DDR5 et une configuration de démarrage RAID 1 BOSS. Outre ses 32 cœurs, le 9335 offre 128 Mo de cache L3 et un TDP de 210 W. Les utilisateurs n'ayant pas besoin d'autant de cœurs peuvent opter pour les modèles EPYC 9255 (24 cœurs), EPYC 9135 (16 cœurs) ou EPYC 9015 (8 cœurs).
Quelques points importants à préciser d'emblée concernant la plateforme : le R4715 ne prend pas en charge les GPU ni les DPU. Il ne s'agit pas d'un oubli. Cette plateforme est conçue spécifiquement pour les charges de travail axées sur le processeur, et Dell a fait des compromis délibérés afin de limiter le coût des composants et l'encombrement. Pour les charges de travail nécessitant des accélérateurs, les modèles R6715 et R7715 sont plus adaptés.
Le R4715 offre un châssis compact refroidi par air, avec jusqu'à trois emplacements PCIe Gen5, 24 emplacements DDR5 RDIMM et des options de stockage flexibles de 3.5 et 2.5 pouces, y compris U.2 NVMe. De plus, il intègre iDRAC10 avec OpenManage Enterprise et une sécurité matérielle via une racine de confiance au niveau du silicium. Les options réseau incluent 25 GbE via OCP 3.0, 100 GbE et 400 GbE via PCIe AIC. Broadcom, Intel et NVIDIA complètent l'écosystème de cartes réseau ; à noter qu'aucune connectivité Fibre Channel n'est officiellement prise en charge. Il fonctionne avec des alimentations de 800 W ou 4715 1100 W, disponibles en versions Platinum ou Titanium, et prend en charge la redondance tolérante aux pannes. Pour un serveur d'entrée de gamme, les fonctionnalités de gestion et de sécurité pour entreprises sont globalement satisfaisantes.
Spécifications Dell PowerEdge R4715
Le tableau ci-dessous met en évidence les spécifications physiques et matérielles de la plateforme Dell PowerEdge R4715.
| Spécifications | Dell PowerEdge R4715 | 
|---|---|
| Processeur |  | 
| Processeur | Un processeur AMD EPYC série 9005 de 5e génération, jusqu'à 32 cœurs | 
| Facteur de forme | Serveur rack 1U | 
| Mémoire |  | 
| Emplacements DIMM | 24 emplacements DIMM DDR5 | 
| Mémoire maximale | 1.5 To (jusqu'à 64 Go par DIMM) | 
| Vitesse de Mémoire | Jusqu'à 5200 MT / s | 
| Type de mémoire | Modules RDIMM ECC DDR5 enregistrés uniquement | 
| Stockage |  | 
| Contrôleurs internes (RAID) | PERC H365i, H965i | 
| Soufflet interne | BOSS-N1 DC-MHS | 
| HBA externes | N/D | 
| Baies d'entraînement avant | 4x 3.5 pouces SAS 8 ports SAS/SATA 2.5 pouces 8 ports U.2 NVMe G4 | 
| Tuning Moteur |  | 
| Alimentations | Platine 800 W, 1100 W Titane 800 W, 1100 W FTR pris en charge | 
| Refroidissement et ventilateurs |  | 
| Options de refroidissement | refroidissement par air | 
| Ventilateurs | Jusqu'à quatre ensembles (module à double ventilateur) de ventilateurs remplaçables à chaud | 
| Dimensions |  | 
| Hauteur | 42.8 mm (1.68 pouces) | 
| Largeur | 482.0 mm (18.97 pouces) | 
| Profondeur (avec lunette) | 816.921 mm (32.16 pouces) | 
| Profondeur (sans lunette) | 815.141 mm (32.09 pouces) | 
| Biseau | Lunette métallique en option | 
| Réseautage et expansion |  | 
| Options de réseau OCP | 2 cartes réseau OCP 3.0 (en option), 1 GbE, 10 GbE, 25 GbE Emplacement 2 : 1×16 Gen5 OCP 3.0 Emplacement 5 : 1×16 Gen5 OCP 3.0 | 
| Carte réseau intégrée | Port Ethernet BMC dédié 1 Gb | 
| Carte réseau PCIe AIC | 100 GbE et 400 GbE ; NDR VPI (400 GbE) | 
| Emplacements PCIe | Jusqu'à 3 emplacements PCIe Gen5 (connecteurs x16) Emplacement 1 : 1×16 Gen5 pleine hauteur ou profil bas Emplacement 2 : 1×16 Gen5 Low Profile ou 1×16 OCP3.0 Emplacement 4 : 1×16 Gen5 pleine hauteur ou profil bas | 
| Options GPU | N/D | 
| Ports |  | 
| Ports avant | 1 port USB 2.0 Type-A (KVM LCP en option) 1 port USB 2.0 Type-C (HÔTE/BMC Direct) 1 port MiniDisplayPort (KVM LCP en option) | 
| Ports arrière | 2x USB 3.1 type A 1x VGA Port Ethernet BMC dédié 1 Gb | 
| Ports internes | 1x USB 3.1 type A | 
| Direction |  | 
| Gestion intégrée | iDRAC10, iDRAC Direct, API RESTful iDRAC avec Redfish, interface de ligne de commande RACADM, module sans fil Quick Sync 2 | 
| Logiciel OpenManage | OpenManage Enterprise (OME), OME Power Manager, OME Services, OME Update Manager, OME APEX AIOps Observability, OME Integration for VMware vCenter, OME Integration for Microsoft System Center, OpenManage Integration for Windows Admin Center | 
| Outils | IPMI | 
| intégrations | Intégrations OpenManage : Collections Red Hat Ansible, fournisseurs Terraform | 
| La Gestion du changement | Gestionnaire de référentiel Dell, Mise à jour du système Dell, Catalogues d'entreprise, Utilitaire de mise à jour du serveur (SUU) | 
| Sécurité |  | 
| Caractéristiques de sécurité | Firmware signé cryptographiquement, chiffrement des données au repos (SED avec gestion de clés locale ou externe), démarrage sécurisé, vérification sécurisée des composants (contrôle d'intégrité matérielle), effacement sécurisé, racine de confiance sur le silicium, verrouillage du système (nécessite iDRAC10 Enterprise ou Datacenter), TPM 2.0 certifié FIPS/CC-TCG, détection d'intrusion dans le châssis, virtualisation chiffrée sécurisée AMD (SEV), chiffrement sécurisé de la mémoire AMD (SME) | 
| Systèmes d'exploitation et hyperviseurs |  | 
| Systèmes d'exploitation/hyperviseurs pris en charge | Serveur Ubuntu LTS de Canonical, Microsoft Windows Server avec Hyper-V, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, VMware ESXi | 
Dell Conception et installation du PowerEdge R4715
Le R4715 est un serveur rack 1U mesurant 1.68 mm de hauteur, 18.97 mm de largeur et 32.09 mm de profondeur sans la façade métallique optionnelle (32.16 mm avec la façade). La face avant comprend un bouton d'alimentation, un bouton d'identification du système, un port USB 2.0 Type-A (utilisé avec le module KVM LCP optionnel), un port USB 2.0 Type-C pour l'accès direct à iDRAC et un port MiniDisplayPort optionnel pour la même configuration KVM. L'ouverture des baies de disques se fait sans outil sur l'ensemble du châssis.
Configuration du stockage
Le R4715 est disponible avec trois configurations de baies avant : 4 baies SAS 3.5 pouces, 8 baies SAS/SATA 2.5 pouces ou 8 baies NVMe Gen4 U.2. Les deux dernières configurations partagent le même format 8 baies 2.5 pouces, le choix du fond de panier déterminant la compatibilité avec les protocoles de disques. Le RAID interne est géré par le contrôleur PERC H365i ou le contrôleur PERC H965i, plus performant. Le démarrage du système d'exploitation utilise un module BOSS-N1 DC-MHS dédié, qui isole complètement le volume de démarrage du pool de stockage de données et évite d'avoir à allouer de l'espace au système d'exploitation au sein d'une baie active.
L'unité testée était livrée avec un seul SSD SATA de 480 Go et deux disques NVMe U.2 de 1.92 To.
Processeur et mémoire
La carte mère R4715 prend en charge un processeur AMD EPYC série 9005 de 5e génération, jusqu'à 32 cœurs. La mémoire est gérée par 24 emplacements DIMM DDR5 compatibles uniquement avec les modules RDIMM (ni UDIMM ni LRDIMM). La capacité maximale est de 1.5 To avec des modules DIMM de 64 Go par emplacement, pour des vitesses allant jusqu'à 5 200 MT/s.
Refroidissement
Le R4715 est exclusivement refroidi par air. Le processeur est équipé d'un dissipateur thermique à cinq caloducs et d'un imposant empilement d'ailettes qui s'étend dans l'espace habituellement vide à côté du socket, augmentant ainsi la surface d'échange thermique. La ventilation est assurée par huit modules de ventilateurs haute performance remplaçables à chaud, disposés en groupes, qui brassent l'air de l'avant vers l'arrière du châssis. Aucun système de refroidissement liquide n'est disponible sur cette plateforme.
Tuning Moteur
Le R4715 prend en charge les alimentations redondantes remplaçables à chaud, disponibles en deux puissances : 800 W et 1 100 W, chacune proposée avec les certifications 80 PLUS Platinum ou Titanium. La certification FTR (Flex Titanium Rating) est également prise en charge sur l’ensemble de la gamme d’alimentations.
Extension et mise en réseau PCIe
La carte mère R4715 offre jusqu'à trois emplacements PCIe Gen5 avec connecteurs x16. L'emplacement 1 prend en charge les cartes pleine hauteur ou profil bas, l'emplacement 2 les cartes profil bas ou OCP 3.0, et l'emplacement 4 les cartes pleine hauteur ou profil bas. Deux emplacements pour cartes réseau OCP 3.0 (emplacements 2 et 5, Gen5 x16) couvrent les options d'adaptateurs 1 GbE, 10 GbE et 25 GbE. Pour les besoins en bande passante plus élevés, les cartes réseau PCIe AIC prennent en charge jusqu'à 100 GbE et 400 GbE, avec NDR VPI (400 GbE). La gestion hors bande s'effectue via un port Ethernet BMC 1 Gb dédié, isolant ainsi le trafic de gestion du plan de données. Cette plateforme ne prend pas en charge les GPU.
Panneau arrière
La connectique arrière comprend deux ports USB 3.1 Type-A, un port VGA et un port Ethernet 1 Gb dédié (BMC) pour la gestion iDRAC. Un port USB 3.1 Type-A supplémentaire est disponible en interne.
Gestion iDRAC10
La gestion à distance du R4715 utilise iDRAC10, la même plateforme que Dell propose en standard sur toute sa gamme PowerEdge de 17e génération, y compris les PowerEdge R770 et R7725 présentés précédemment. L'interface étant identique sur l'ensemble de la gamme, les administrateurs connaissant déjà iDRAC sur d'autres serveurs PowerEdge s'y retrouveront immédiatement.
Le tableau de bord iDRAC10 offre une vue d'ensemble complète de l'état de santé de tous les sous-systèmes clés : état du système, processeur, mémoire, refroidissement, stockage, tensions, alimentations, batteries et détection d'intrusion. L'unité de test affiche tous les sous-systèmes comme étant opérationnels au moment du test. Les informations système et les détails de la version du firmware sont affichés directement sur le tableau de bord, ainsi que le statut de la licence, qui est confirmé comme étant de type Entreprise sur l'unité de test. Le panneau « Résumé des tâches » suit les tâches en attente, en cours et terminées. L'unité de test affiche les tâches terminées d'un cycle de provisionnement initial, y compris un petit nombre d'erreurs et un échec, ce qui est typique d'un nouveau déploiement.
En explorant la section « Environnements système », vous accédez aux détails du refroidissement, notamment l'état de chaque ventilateur, les vitesses PWM, les paramètres du profil thermique et les relevés de température d'entrée, le tout en temps réel. Ceci est particulièrement utile pour vérifier le flux d'air dans les configurations de racks denses ou pour diagnostiquer les problèmes thermiques sans accès physique au serveur.
La visibilité de la consommation électrique suit le même principe. La section « Informations sur l'alimentation » détaille l'état du bloc d'alimentation, la consommation de courant et le taux d'utilisation, ainsi qu'un graphique historique glissant. Les administrateurs peuvent ainsi visualiser rapidement la consommation moyenne et maximale au fil du temps, ce qui est utile pour la planification de la capacité et la détection des pics de consommation liés à la charge de travail, sans avoir besoin d'un outil de surveillance supplémentaire.
Ensemble, ces points de vue font d'iDRAC10 une solution de gestion hors bande performante qui couvre l'intégralité du cycle de vie opérationnel du R4715, du déploiement initial à la surveillance quotidienne, le tout accessible à distance via un navigateur ou l'API RESTful Redfish.
Performances Dell PowerEdge R4715
Pour évaluer les performances du Dell PowerEdge R4715, nous l'avons comparé à son équivalent 2U, le Dell PowerEdge R5715 . Les deux plateformes partagent la même configuration mémoire et la même architecture PowerEdge, ce qui rend leur comparaison pertinente. La principale différence entre les deux unités testées réside dans le processeur : le R4715 est équipé d'un processeur AMD EPYC 9335 à 32 cœurs, tandis que le R5715 intègre un processeur AMD EPYC 9015 à 8 cœurs.
Il est important de noter que les deux serveurs prennent en charge la même gamme de processeurs EPYC série 9005 et peuvent être configurés avec l'une ou l'autre puce selon les besoins de la charge de travail. La différence du nombre de cœurs entre ces deux unités se reflétera dans les résultats, mais ces derniers indiquent les performances réelles de chaque plateforme, telles qu'elles sont livrées, et non une comparaison des performances maximales entre les deux.
Pour évaluer la capacité des processeurs sur l'ensemble des systèmes, nous avons utilisé un ensemble ciblé de tests de calcul. y-cruncher a mesuré le débit arithmétique brut et les performances en virgule flottante multithread. Blender a fourni une charge de travail de rendu réaliste, évolutive en fonction du nombre de cœurs disponibles et de la bande passante mémoire. La suite de tests Phoronix a complété cet ensemble de tests en ajoutant une plus grande variété de charges de travail gourmandes en ressources processeur, offrant ainsi une vision plus complète des performances de calcul soutenues sur les deux plateformes.
Spécifications du système de test
- Plate-forme: Dell PowerEdge R4715
- CPU: AMD EPYC 9335 monocœur
- Mémoire: 384GB DDR5
- Stockage: Boss RAID1
croque-y
y-cruncher est un programme multithread et évolutif capable de calculer Pi et d'autres constantes mathématiques jusqu'à des milliers de milliards de chiffres. Depuis son lancement en 2009, il est devenu une application de benchmarking et de test de résistance populaire auprès des overclockeurs et des passionnés de matériel informatique.
Le R4715 a systématiquement surpassé le R5715 quelle que soit la taille de la charge de travail, réalisant les calculs environ 2.8 à 2.9 fois plus rapidement sur toute la plage de 1 à 50 milliards de chiffres. À 1 milliard de chiffres, le R4715 a terminé le calcul en 5.305 secondes, contre 14.537 secondes pour le R5715, et cet écart est resté constant malgré l'augmentation de la charge de travail. À 50 milliards de chiffres, le R4715 a atteint 445.440 secondes, tandis que le R5715 a nécessité 1 273,734 secondes. Ce résultat reflète directement la différence du nombre de cœurs : l'EPYC 9335 possède 32 cœurs contre 8 cœurs pour l'EPYC 9015 du R5715.
| y-cruncher (une durée plus courte est préférable) | Dell PowerEdge R4715 (AMD EPYC 9335 32 cœurs \| 384 Gio de RAM) | Dell PowerEdge R5715 (AMD EPYC 9015 8 cœurs \| 384 Gio de RAM) | 
|---|---|---|
| 25 millions | 0.11 secondes | 0.25 secondes | 
| 50 millions | 0.23 secondes | 0.51 secondes | 
| 100 millions | 0.46 secondes | 1.08 secondes | 
| 250 millions | 1.22 secondes | 3.00 secondes | 
| 500 millions | 2.49 secondes | 6.60 secondes | 
| 1 milliard | 5.30 secondes | 14.53 secondes | 
| 2.5 milliard | 14.58 secondes | 41.32 secondes | 
| 5 milliard | 32.38 secondes | 92.99 secondes | 
| 10 milliard | 71.54 secondes | 202.87 secondes | 
| 25 milliard | 203.40 secondes | 576.87 secondes | 
| 50 milliard | 445.44 secondes | 1,273.73 secondes | 
Mixeur 4.5
Blender 4.5 est un logiciel de modélisation 3D open source. Ce test de performance a été réalisé à l'aide de l'utilitaire en ligne de commande Blender Benchmark. Le score est basé sur le nombre d'échantillons par minute ; plus la valeur est élevée, meilleures sont les performances.
Le R4715 a affiché un débit de rendu environ 3.8 à 4 fois supérieur à celui du R5715 sur les trois scènes, un écart légèrement plus important que sur le test Y-Ccruncher. Ceci illustre la forte augmentation des performances du moteur de rendu CPU de Blender en fonction du nombre de cœurs lors de la parallélisation des calculs de lancer de rayons. Sur la scène « Monster », le R4715 a atteint 523.29 échantillons par minute, contre 135.21 pour le R5715. Dans « Junkshop », le score était de 355.43 contre 88.61, et dans « Classroom », de 264.70 contre 68.48.
| Test de performance du processeur avec Blender 4.5 (un nombre d'échantillons par minute plus élevé est préférable) | Dell PowerEdge R4715 (AMD EPYC 9335 32 cœurs \| 384 Gio de RAM) | Dell PowerEdge R5715 (AMD EPYC 9015 8 cœurs \| 384 Gio de RAM) | 
|---|---|---|
| Monster | 1 076,122 échantillons/min | 1 076,122 échantillons/min | 
| Brocanteur | 1 076,122 échantillons/min | 1 076,122 échantillons/min | 
| Salle de classe | 1 076,122 échantillons/min | 1 076,122 échantillons/min | 
Points de repère Phoronix
La suite de tests Phoronix est une plateforme d'évaluation des performances automatisée et open source qui prend en charge plus de 450 profils de test et plus de 100 suites de tests via OpenBenchmarking.org. Elle gère l'ensemble du processus, de l'installation des dépendances à l'exécution des tests et à la collecte des résultats, ce qui la rend idéale pour les comparaisons de performances, la validation matérielle et l'intégration continue. Nous comparerons les performances des processeurs R4715 et R5715 avec les tests Stream, 7-Zip, la compilation du noyau Linux, Apache et OpenSSL.
Le débit du serveur web Apache figurait parmi les résultats les plus proches de la suite, le R4715 atteignant 177 839,86 requêtes par seconde contre 123 710,75 pour le R5715. Apache peut maintenir un débit raisonnable avec moins de cœurs lorsque la bande passante mémoire est suffisante, ce qui explique un écart plus faible ici qu'avec des charges de travail plus fortement parallélisées.
Le débit de transfert OpenSSL a affiché un écart plus important, le R4715 atteignant 533 318 299 283 octets/s contre 148 168 050 733 octets/s pour le R5715. Le débit cryptographique augmente fortement avec le nombre de threads, et cet écart le reflète directement.
Le test de compilation du noyau Linux a révélé l'un des écarts les plus marqués de la suite. Le R4715 a terminé en 379.53 secondes contre 1 244,86 secondes pour le R5715 ; la compilation du noyau étant l'une des mesures les plus directes du nombre de threads qu'un système peut exécuter simultanément.
La compression 7-Zip a atteint 260 124 MIPS sur le R4715 contre 98 555 MIPS sur le R5715, suivant de manière cohérente avec le reste de la suite.
Le débit de mémoire de flux a été mesuré à 370 228,9 Mo/s sur le R4715, contre 230 123,6 Mo/s sur le R5715.
| Points de repère Phoronix | Dell PowerEdge R4715 (AMD EPYC 9335 32 cœurs \| 384 Gio de RAM) | Dell PowerEdge R5715 (AMD EPYC 9015 8 cœurs \| 384 Gio de RAM) | 
|---|---|---|
| Requêtes Apache par seconde | 177,839.86 | 123,710.75 | 
| Débit de transfert OpenSSL (octets/s) | 533,318,299,283 | 148,168,050,733 | 
| Temps de compilation du noyau (secondes) (plus le temps est court, mieux c'est) | 379.531 | 1,244.86 | 
| 7-ZIP MIPS | 260,124 | 98,555 | 
| Débit du flux (Mo/s) | 370,228.9 | 230,123.6 | 
Conclusion
Le Dell PowerEdge R4715 est une plateforme 1U performante qui justifie pleinement une architecture mono-processeur pour les charges de travail courantes des PME. Les entreprises utilisant la virtualisation, des bases de données à grande échelle et des déploiements en périphérie de réseau, pour lesquelles l'efficacité des licences et la simplicité d'utilisation sont primordiales, trouveront le R4715 parfaitement adapté. Son format 1U, ses trois emplacements PCIe Gen5, ses 24 emplacements DDR5 RDIMM et ses options de stockage flexibles de 2.5 et 3.5 pouces confèrent à cette plateforme une grande polyvalence sans les coûts supplémentaires d'un châssis bi-processeur.
Les résultats de performance démontrent les capacités de la plateforme. Testé avec le processeur EPYC 9335, le R4715 a systématiquement surpassé le R5715 sur tous les benchmarks, l'avantage du nombre de cœurs étant particulièrement visible dans les charges de travail fortement parallélisées telles que la compilation du noyau, OpenSSL et Blender. Les utilisateurs n'ayant pas besoin de 32 cœurs peuvent opter pour l'EPYC 9255 (24 cœurs), l'EPYC 9135 (16 cœurs) ou l'EPYC 9015 (8 cœurs), en choisissant un processeur adapté à leurs besoins et à leur budget.
Il est important de préciser ce que le R4715 n'est pas. Il ne prend pas en charge les GPU ni les DPU, un choix délibéré pour limiter les coûts et l'encombrement. Pour les charges de travail nécessitant des accélérateurs, les modèles R6715 et R7715, basés sur AMD, sont les plus adaptés. Une connectivité réseau plus rapide est disponible via PCIe AIC, avec des options 100 GbE et 400 GbE.
Le R4715 excelle constamment dans les domaines essentiels pour ses cas d'utilisation cibles. Il offre une densité de calcul élevée dans un châssis 1U, une efficacité énergétique optimale grâce à ses alimentations Platinum et Titanium de 800 W et 1 100 W, des configurations de stockage NVMe et SAS/SATA flexibles, ainsi qu'une expérience de gestion iDRAC10 Enterprise éprouvée, parfaitement intégrée à la gamme PowerEdge de 17e génération. Pour les PME et les entreprises de taille intermédiaire recherchant une plateforme de calcul mono-socket adaptée à leurs besoins, sans surcoût en stockage ni en capacité d'extension, le R4715 est un excellent choix et un complément naturel au R5715 dans la gamme actuelle de serveurs Dell basés sur AMD.