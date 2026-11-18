Le PowerEdge R5715 est le deuxième modèle de la gamme PowerEdge de 17e génération de Dell, dédiée aux PME. Il se distingue de son homologue 1U par des priorités différentes. Alors que le R4715 privilégie la densité de calcul et l'efficacité du nombre de cœurs par unité de rack, le R5715 est conçu autour de la capacité de stockage et de l'extensibilité des E/S dans un format 2U mono-socket. Les lecteurs de notre test du R4715 retrouveront les fondamentaux de la plateforme : même famille de processeurs AMD EPYC de 5e génération, même architecture mémoire DDR5 à 24 emplacements et même système de gestion iDRAC 10. Seule la tâche confiée au R5715 diffère légèrement.
Notre serveur de test était équipé d'un processeur AMD EPYC 9015, le modèle 8 cœurs de la gamme Turin, associé à 384 Go de mémoire DDR5 et une configuration RAID 1 BOSS pour le démarrage. Nos tests ont porté sur le fond de panier de stockage 3.5 pouces à 12 baies du R5715, un domaine où le 9015 prend tout son sens. Les charges de travail telles que le partage de fichiers, la sauvegarde de données et la vidéosurveillance en point de vente n'ont pas besoin de 32 cœurs ; elles privilégient la densité de stockage, un débit soutenu et une gestion fiable. Le 9015 permet de limiter la consommation d'énergie et les coûts de licence, tout en offrant jusqu'à 288 To de capacité de stockage brute dans un seul nœud 2U.
La R5715 augmente le nombre de ports PCIe Gen5 à quatre, contre trois pour la R4715, et ajoute un port réseau OCP 3.0 supplémentaire, offrant ainsi une plus grande flexibilité pour répondre à la demande croissante d'E/S. Les deux plateformes prennent en charge les protocoles 100 GbE et 400 GbE via PCIe AIC, ce qui les rend parfaitement adaptées aux environnements exigeant une large bande passante. Cependant, aucune des deux ne prend officiellement en charge la connectivité Fibre Channel. De plus, aucune ne prend en charge les GPU ni les DPU. Elles fonctionnent avec les mêmes alimentations de 800 W et 1100 W, disponibles en versions Platinum et Titanium, avec une redondance tolérante aux pannes et un refroidissement par air.
Spécifications Dell PowerEdge R5715
Le tableau ci-dessous met en évidence les spécifications physiques et matérielles de la plateforme Dell PowerEdge R5715.
| Spécifications | Dell PowerEdge R5715 | 
|---|---|
| Processeur |  | 
| Processeur | Un processeur AMD EPYC série 9005 de 5e génération, jusqu'à 32 cœurs | 
| Facteur de forme | Serveur rack 2U | 
| Mémoire |  | 
| Emplacements DIMM | 24 emplacements DIMM DDR5 | 
| Mémoire maximale | 1.5 To (jusqu'à 64 Go par DIMM) | 
| Vitesse de Mémoire | Jusqu'à 5200 MT / s | 
| Type de mémoire | Modules RDIMM ECC DDR5 enregistrés uniquement | 
| Stockage |  | 
| Contrôleurs internes (RAID) | PERC H365i, H965i | 
| Soufflet interne | BOSS-N1 DC-MHS | 
| HBA externes | N/D | 
| Baies d'entraînement avant | 12 ports SAS/SATA 3.5 pouces 16 ports SAS/SATA 2.5 pouces | 
| Tuning Moteur |  | 
| Alimentations | Platine 800 W, 1100 W Titane 800 W, 1100 W FTR pris en charge | 
| Refroidissement et ventilateurs |  | 
| Options de refroidissement | refroidissement par air | 
| Ventilateurs | Jusqu'à six ventilateurs enfichables à chaud | 
| Dimensions |  | 
| Hauteur | 86.8 mm (3.41 pouces) | 
| Largeur | 482.0 mm (18.97 pouces) | 
| Profondeur (avec lunette) | 802.4 mm (31.59 pouces) | 
| Profondeur (sans lunette) | 801.51 mm (31.55 pouces) | 
| Biseau | Lunette métallique en option | 
| Réseautage et expansion |  | 
| Options de réseau OCP | 2 cartes réseau OCP 3.0 (en option), 1 GbE, 10 GbE, 25 GbE Emplacement 4 : 1×16 Gen5 OCP 3.0 Emplacement 10 : 1×16 Gen5 OCP 3.0 | 
| Carte réseau intégrée | Port Ethernet BMC dédié 1 Gb | 
| Carte réseau PCIe AIC | 100 GbE et 400 GbE ; NDR VPI (400 GbE) | 
| Emplacements PCIe | Jusqu'à 4 emplacements PCIe Gen5 (connecteurs x16) Emplacement 2 : 1×16 Gen5 Pleine hauteur Emplacement 3 : 1×16 Gen5 Pleine hauteur Emplacement 7 : 1×16 Gen5 Pleine hauteur Emplacement 9 : 1×16 Gen5 Pleine hauteur | 
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
Le Dell PowerEdge R5715 est un serveur rack 2U monoprocesseur basé sur la plateforme AMD EPYC série 9005 de 5e génération. Conçu comme une plateforme de stockage performante pour les entreprises exigeant une capacité élevée et une connectivité E/S fiable sans le surcoût d'une architecture biprocesseur, le R5715 cible les charges de travail telles que les bases de données, les partages de fichiers, les sauvegardes et la virtualisation, où un seul processeur EPYC puissant gère la charge plus efficacement que deux processeurs de génération précédente. Nous avons également utilisé ce châssis pour notre comparatif de consommation énergétique entre disques durs et mémoire flash Micron 6600 ION , en remplaçant huit disques durs de 30 To par un SSD de 245 To. Prenant en charge jusqu'à 288 To de stockage brut et disposant de quatre emplacements d'extension PCIe Gen5, le R5715 offre des performances bien supérieures à celles de sa catégorie de prix.
Panneau extérieur et avant
Le R5715 est livré avec un cadre métallique optionnel arborant le motif hexagonal emblématique de Dell. Ce cadre s'emboîte parfaitement sur le châssis et laisse apparaître les commandes du panneau avant sur le côté droit : un bouton d'alimentation, un port USB 2.0 Type-C pour un accès direct au BMC, un port iDRAC Direct et un bouton d'identification du système. Sans le cadre, le châssis mesure 3.41 cm de hauteur, 18.97 cm de largeur et 31.55 cm de profondeur, ce qui lui permet de s'intégrer dans les racks 2U standard. La qualité de fabrication est irréprochable, avec des loquets de baie de disques sans outil et des clips de fixation bleus utilisés systématiquement sur les composants internes pour un accès rapide.
Configuration du stockage
L'unité testée est équipée d'une baie avant SAS/SATA 12 x 3.5 pouces, dont quatre baies occupées par des disques durs SATA 6 Gb/s 7 200 tr/min de 20 To et huit baies libres pour de futures extensions. Une configuration alternative avec fond de panier SAS/SATA 16 x 2.5 pouces est également disponible, selon les besoins. La gestion RAID est assurée par le contrôleur interne PERC H365i ou le PERC H965i, plus performant. Le démarrage est géré séparément par un module BOSS-N1 DC-MHS dédié à l'arrière, isolant ainsi le système d'exploitation des données. Cette conception astucieuse évite l'erreur fréquente consistant à exécuter le système d'exploitation et le stockage des données sur la même baie.
Processeur et refroidissement
La R5715 est une plateforme mono-socket basée sur le processeur AMD EPYC série 9005, prenant en charge jusqu'à 32 cœurs. Son imposant dissipateur thermique est un modèle tour à ailettes intégrant des caloducs en cuivre, fixé au socket SP5 par six vis imperdables. Le refroidissement est entièrement par air ; jusqu'à six ventilateurs remplaçables à chaud assurent la circulation de l'air de l'avant vers l'arrière du châssis. Le refroidissement liquide n'est pas compatible avec cette plateforme.
Mémoire
La carte mère R5715 dispose de 24 emplacements DIMM DDR5 répartis en deux groupes de part et d'autre du socket du processeur. Cette plateforme est exclusivement compatible avec les modules RDIMM ; les modules UDIMM et LRDIMM ne sont pas pris en charge. La capacité maximale atteint 1.5 To avec 64 Go de RDIMM par emplacement, fonctionnant à une fréquence maximale de 5 200 MT/s. Le modèle testé est livré avec plusieurs emplacements occupés, tirant parti de l'architecture mémoire multicanal d'EPYC pour offrir une bande passante agrégée élevée sur l'ensemble du sous-système mémoire.
Extension et mise en réseau PCIe
Le R5715 offre jusqu'à quatre emplacements PCIe Gen5 x16 pleine hauteur (emplacements 2, 3, 7 et 9), répartis sur cinq emplacements de riser (Risers 1 à 5) visibles à l'intérieur du châssis. Deux emplacements supplémentaires pour cartes réseau OCP NIC 3.0 (emplacements 4 et 10, Gen5 x16) prennent en charge les adaptateurs réseau OCP 1 GbE, 10 GbE ou 25 GbE. Pour une connectivité haut débit, les cartes réseau PCIe AIC prennent en charge jusqu'à 100 GbE et 400 GbE, avec NDR VPI (400 GbE). Un port Ethernet BMC 1 Gb dédié est intégré au panneau arrière pour la gestion iDRAC hors bande. Le R5715 ne propose pas d'options GPU ; il s'agit d'une plateforme de stockage et de calcul, et non d'un châssis d'accélération.
Gestion iDRAC10
La gestion à distance du R5715 est assurée par iDRAC10, la même plateforme que Dell propose en standard sur l'ensemble de sa gamme PowerEdge de 17e génération, notamment les PowerEdge R770 et R7725 que nous avons testés précédemment. L'interface étant identique pour toute la gamme, les administrateurs familiarisés avec iDRAC sur d'autres plateformes PowerEdge s'y retrouveront immédiatement.
Le tableau de bord iDRAC10 offre un aperçu complet et instantané de l'état de santé de chaque sous-système principal : état du système, processeur, mémoire, refroidissement, stockage, tensions, alimentations, batteries et détection d'intrusion. L'unité de test indique que tous les sous-systèmes étaient opérationnels au moment du test. Les informations système et les détails de la version du firmware sont affichés directement sur le tableau de bord, ainsi que l'état de la licence, qui, sur l'unité de test, est confirmé comme étant de type Entreprise. Le panneau « Résumé des tâches » suit les tâches en attente, en cours et terminées. L'unité de test affiche les tâches terminées d'un cycle de provisionnement initial, dont quelques-unes avec des erreurs et une ayant échoué, ce qui est typique d'un nouveau déploiement.
En explorant la section « Environnements système », vous accédez aux détails du refroidissement, notamment l'état de chaque ventilateur, les vitesses PWM, les paramètres du profil thermique et les relevés de température d'entrée, le tout en temps réel. Cette fonctionnalité est particulièrement utile pour vérifier le flux d'air dans les configurations de racks denses ou pour diagnostiquer les problèmes thermiques sans avoir à accéder physiquement au serveur.
La visibilité de la consommation électrique suit le même principe. La section « Informations sur l'alimentation » détaille l'état du bloc d'alimentation, la consommation de courant et le taux d'utilisation, ainsi qu'un graphique historique glissant. Les administrateurs peuvent ainsi visualiser rapidement la consommation moyenne et maximale au fil du temps, ce qui est précieux pour la planification de la capacité et l'identification des pics de consommation liés à la charge de travail, sans avoir besoin d'un outil de surveillance supplémentaire.
Ensemble, ces points de vue font d'iDRAC10 une solution de gestion hors bande performante qui couvre l'intégralité du cycle de vie opérationnel du R5715, du déploiement initial à la surveillance quotidienne, le tout accessible à distance via un navigateur ou l'API RESTful Redfish.
Performances Dell PowerEdge R5715
Pour évaluer les performances du Dell PowerEdge R5715, nous l'avons comparé à son homologue 1U, le Dell PowerEdge R4715 . Les deux plateformes partagent la même configuration mémoire et la même architecture PowerEdge, ce qui en fait un point de comparaison évident. La principale différence entre les deux unités testées réside dans le processeur. Le R4715 était équipé d'un processeur AMD EPYC 9335 à 32 cœurs, tandis que le R5715 disposait d'un processeur AMD EPYC 9015 à 8 cœurs.
Il est important de noter que les deux plateformes prennent en charge la même gamme de processeurs EPYC série 9005 et peuvent être configurées avec l'une ou l'autre puce selon les besoins. La différence du nombre de cœurs entre ces deux unités se reflétera dans les résultats, mais ces derniers correspondent aux performances réelles de chaque plateforme, et non à une comparaison de performances maximales.
Afin de solliciter les processeurs des deux systèmes, nous avons utilisé un ensemble ciblé de tests de calcul. y-cruncher a permis d'évaluer le débit arithmétique brut et les performances en virgule flottante multithread. Blender a fourni une charge de travail de rendu réaliste, évolutive en fonction du nombre de cœurs disponibles et de la bande passante mémoire. La suite de tests Phoronix a complété cet ensemble avec une collection plus large de charges de travail gourmandes en ressources processeur, offrant ainsi une vision plus complète des performances de calcul soutenues sur les deux plateformes.
Spécifications du système de test
- Plate-forme: Dell PowerEdge R5715
- CPU: AMD EPYC 9015 unique
- Mémoire: 384GB DDR5
- Stockage: Boss RAID1
croque-y
y-cruncher est un programme multithread et évolutif capable de calculer Pi et d'autres constantes mathématiques jusqu'à des milliers de milliards de chiffres. Depuis son lancement en 2009, il est devenu une application de benchmarking et de test de résistance populaire auprès des overclockeurs et des passionnés de matériel informatique.
Le R5715 a affiché des performances prévisibles par rapport au R4715, quelle que soit la taille de la charge de travail. À 1 milliard de décimales, le R5715 a terminé en 14.537 secondes contre 5.305 secondes pour le R4715, et cet écart s'est maintenu. À 50 milliards de décimales, le R5715 a atteint 1 1,273.734 secondes tandis que le R4715 a terminé en 445.440 secondes, soit environ 2.8 à 2.9 fois plus rapide sur toute la plage de 4715 à 50 milliards de décimales. Malgré ses 8 cœurs seulement, l'EPYC 9015 est un processeur serveur dédié, doté d'une bande passante mémoire et d'un cache nettement supérieurs à ceux d'un processeur de bureau classique. Il reste ainsi bien plus performant que la plupart des processeurs grand public sur les mêmes charges de travail.
| Y-cruncher (une durée plus courte est meilleure) | Dell PowerEdge R4715 (AMD EPYC 9335 32 cœurs \| 384 Gio de RAM) | Dell PowerEdge R5715 (AMD EPYC 9015 8 cœurs \| 384 Gio de RAM) | 
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
Blender 4.5 est une application de modélisation 3D open source. Ce benchmark a été réalisé à l'aide de l'utilitaire Blender Benchmark CLI. Le score est mesuré en échantillons par minute, les valeurs les plus élevées indiquant de meilleures performances.
Les résultats de Blender suivent une tendance similaire à celle de Y-Cruncher : le nombre de cœurs supérieur du R4715 se traduit directement par un débit de rendu plus élevé. Sur la scène Monster, le R4715 a atteint 523.29 échantillons par minute contre 135.21 pour le R5715. La scène Junkshop a obtenu 355.43 contre 88.61, et la scène Classroom 264.70 contre 68.48 pour le R5715. Sur l'ensemble des trois scènes, le R4715 a affiché un débit de rendu environ 3.8 à 4 fois supérieur à celui du R5715, un écart légèrement plus important que sur Y-Cruncher. Ceci illustre l'importance du nombre de cœurs pour le rendu CPU de Blender lors de la parallélisation des calculs de lancer de rayons au sein d'une même scène.
| Test de performance du processeur avec Blender 4.5 (un nombre d'échantillons par minute plus élevé est préférable) | Dell PowerEdge R4715 (AMD EPYC 9335 32 cœurs \| 384 Gio de RAM) | Dell PowerEdge R5715 (AMD EPYC 9015 8 cœurs \| 384 Gio de RAM) | 
|---|---|---|
| Monster | 1 076,122 échantillons/min | 1 076,122 échantillons/min | 
| Brocanteur | 1 076,122 échantillons/min | 1 076,122 échantillons/min | 
| Salle de classe | 1 076,122 échantillons/min | 1 076,122 échantillons/min | 
Points de repère Phoronix
Phoronix Test Suite est une plateforme de benchmarking automatisée et open source prenant en charge plus de 450 profils de test et plus de 100 suites de tests via OpenBenchmarking.org . Elle gère l'ensemble du processus, de l'installation des dépendances à l'exécution des tests et à la collecte des résultats, ce qui la rend idéale pour les comparaisons de performances, la validation matérielle et l'intégration continue. Nous comparerons ici les performances des processeurs R5715 et R4715 avec les tests Stream, 7-Zip, la compilation du noyau Linux, Apache et OpenSSL.
En termes de débit pour les serveurs web Apache, le R4715 a atteint 177 839,86 requêtes par seconde, contre 123 710,75 pour le R5715, soit l'un des résultats les plus proches de toute la série. La capacité d'Apache à offrir des performances acceptables même avec un nombre de cœurs inférieur, pourvu que la bande passante mémoire soit suffisante, explique que l'écart soit ici plus faible que pour des charges de travail plus fortement parallélisées.
Le débit de transfert OpenSSL a affiché un écart plus important, le R4715 atteignant 533 318 299 283 octets par seconde contre 148 168 050 733 octets par seconde pour le R5715. Le débit cryptographique est l'une des charges de travail qui évoluent le plus rapidement avec le nombre de threads, et cet écart le reflète clairement.
Le test de compilation du noyau Linux a révélé l'un des écarts les plus marqués de la suite, le R4715 terminant en 379.53 secondes contre 1 244,86 secondes pour le R5715. La compilation du noyau est l'une des mesures les plus directes du nombre de threads qu'un système peut exécuter simultanément.
La compression 7-Zip a atteint 260 124 MIPS sur le R4715 contre 98 555 MIPS sur le R5715, ce qui est cohérent avec les résultats obtenus sur le reste de la suite.
Le débit de mémoire de flux était de 370 228,9 Mo/s sur le R4715, contre 230 123,6 Mo/s sur le R5715.
| Points de repère Phoronix | Dell PowerEdge R4715 (AMD EPYC 9335 32 cœurs \| 384 Gio de RAM) | Dell PowerEdge R5715 (AMD EPYC 9015 8 cœurs \| 384 Gio de RAM) | 
|---|---|---|
| Requêtes Apache par seconde | 177,839.86 | 123,710.75 | 
| Débit de transfert OpenSSL (octets/s) | 533,318,299,283 | 148,168,050,733 | 
| Temps de compilation du noyau (secondes) (plus le temps est court, mieux c'est) | 379.531 | 1,244.86 | 
| 7-ZIP MIPS | 260,124 | 98,555 | 
| Débit du flux (Mo/s) | 370,228.9 | 230,123.6 | 
Conclusion
Le Dell PowerEdge R5715 est une plateforme 2U dédiée au stockage, parfaitement conçue et qui justifie pleinement l'utilisation d'un châssis mono-processeur dans certains contextes de charge de travail. Les entreprises exploitant des services de fichiers, des cibles de sauvegarde, des systèmes de vidéosurveillance ou des bases de données, et qui privilégient la densité de disques et l'extensibilité des E/S à la puissance de calcul brute, trouveront dans le R5715 une solution idéale. Son fond de panier 3.5 pouces à 12 baies, prenant en charge jusqu'à 288 To de capacité brute, associé à quatre emplacements PCIe Gen5 et à la prise en charge de deux cartes réseau 3.0 OCP, offre à la plateforme une marge de progression significative sans nécessiter le passage à un châssis bi-processeur plus onéreux.
Les résultats de performance sont sans équivoque. Testé avec le processeur EPYC 9015, le R5715 affiche des performances nettement inférieures à celles du R4715 (32 cœurs) sur tous les benchmarks, comme prévu. Cependant, cette comparaison est quelque peu hors sujet. Le R5715 n'est pas conçu pour les calculs intensifs, et l'EPYC 9015 n'est pas le processeur que Dell prévoit pour la plupart de ses clients. Configurer le R5715 avec un processeur EPYC 9005 à plus grand nombre de cœurs permet de réduire considérablement cet écart, et l'architecture de la plateforme est parfaitement compatible.
Le R5715 excelle dans les domaines essentiels à ses cas d'utilisation cibles : densité de stockage, flexibilité d'extension, efficacité énergétique et gestion. iDRAC10 Enterprise offre une expérience de gestion hors bande éprouvée et cohérente, directement issue de la gamme PowerEdge de 17e génération, réduisant ainsi les coûts opérationnels pour les équipes ayant déjà investi dans la suite de gestion Dell.
Pour les acheteurs de PME et de marché intermédiaire qui cherchent à consolider leurs charges de travail de stockage sur une plateforme monoprocesseur adaptée sans surdimensionner leurs capacités de calcul, le R5715 est un excellent choix et un complément naturel au R4715 dans la gamme actuelle de Dell basée sur AMD.