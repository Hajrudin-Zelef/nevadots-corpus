# Test du Lenovo ThinkSystem SR650 V4 : Un serveur 2U polyvalent et performant

*Source : https://www.storagereview.com/fr/review/lenovo-thinksystem-sr650-v4-review-versatile-2u-workhorse*

Le Lenovo ThinkSystem SR650 V4 est un serveur rack 2U à deux sockets, flexible et puissant, conçu pour répondre aux besoins de secteurs tels que les services cloud, les télécommunications et le calcul haute performance (HPC). Que ce soit pour optimiser les charges de travail à forte croissance horizontale ou pour pérenniser votre centre de données grâce à une puissance de calcul haute densité, le SR650 V4 offre des améliorations significatives par rapport à son prédécesseur, le SR650 V3.

## Différences entre Lenovo SR650 V4 et SR650 V3

### Processeurs

Les performances du processeur constituent l'une des améliorations majeures du SR650 V4. Alors que le SR650 V3 s'appuyait sur des processeurs Intel Xeon Scalable de 4e génération, le SR650 V4 inaugure la plateforme Intel Xeon 6 (anciennement nommée « Granite Rapids »). Notre test porte sur l'architecture à cœurs de performance (P-core), conçue spécifiquement pour les charges de travail exigeantes en calcul. Le V4 prend en charge un ou deux processeurs, avec jusqu'à 86 cœurs P par socket (soit jusqu'à 172 threads), offrant des fréquences plus élevées (jusqu'à 4 GHz) et une enveloppe thermique (TDP) jusqu'à 350 W.

Cette transition permet aux entreprises de gérer des applications plus exigeantes par serveur, réduisant ainsi leur encombrement physique. L'architecture offre une base solide pour la virtualisation et les opérations de bases de données lourdes. De plus, l'architecture V4 prend en charge une bande passante PCIe massive, avec jusqu'à 88 lignes PCIe 5.0 par processeur, essentielle pour alimenter les adaptateurs réseau haut débit modernes et les baies de stockage NVMe sans goulots d'étranglement.

| Modèle de CPU | Noyaux / Threads | Base Freq | Max Turbo | L3 Cache | TDP |
|---|---|---|---|---|---|
| 6787P | 86/172 | 2.0 GHz | 3.8 GHz | 336 MB | 350 W |
| 6781P | 80/160 | 2.0 GHz | 3.8 GHz | 336 MB | 350 W |
| 6767P | 64/128 | 2.4 GHz | 3.9 GHz | 336 MB | 330 W |
| 6761P | 64/128 | 2.5 GHz | 3.9 GHz | 320 MB | 330 W |
| 6760P | 64/128 | 2.2 GHz | 3.8 GHz | 320 MB | 330 W |
| 6747P | 48/96 | 2.7 GHz | 3.9 GHz | 288 MB | 330 W |
| 6745P | 32/64 | 3.1 GHz | 4.3 GHz | 336 MB | 300 W |
| 6741P | 48/96 | 2.5 GHz | 3.8 GHz | 288 MB | 300 W |
| 6740P | 48/96 | 2.1 GHz | 3.8 GHz | 288 MB | 270 W |
| 6737P | 32/64 | 2.9 GHz | 4.0 GHz | 144 MB | 270 W |
| 6736P | 36/72 | 2.0 GHz | 4.1 GHz | 144 MB | 205 W |
| 6732P | 32/64 | 3.8 GHz | 4.3 GHz | 144 MB | 350 W |
| 6731P | 32/64 | 2.5 GHz | 4.1 GHz | 144 MB | 245 W |
| 6730P | 32/64 | 2.5 GHz | 3.8 GHz | 288 MB | 250 W |
| 6724P | 16/32 | 3.6 GHz | 4.3 GHz | 72 MB | 210 W |
| 6714P | 8/16 | 4.0 GHz | 4.3 GHz | 48 MB | 165 W |
| 6530P | 32/64 | 2.3 GHz | 4.1 GHz | 144 MB | 225 W |
| 6527P | 24/48 | 3.0 GHz | 4.2 GHz | 144 MB | 255 W |
| 6521P | 24/48 | 2.6 GHz | 4.1 GHz | 144 MB | 225 W |
| 6520P | 24/48 | 2.4 GHz | 4.0 GHz | 144 MB | 210 W |
| 6517P | 16/32 | 3.2 GHz | 4.2 GHz | 72 MB | 190 W |
| 6515P | 16/32 | 2.3 GHz | 3.8 GHz | 72 MB | 150 W |
| 6511P | 16/32 | 2.3 GHz | 4.2 GHz | 72 MB | 150 W |
| 6507P | 8/16 | 3.5 GHz | 4.3 GHz | 48 MB | 150 W |
| 6505P | 12/24 | 2.2 GHz | 4.1 GHz | 48 MB | 150 W |

### Mémoire

En matière de mémoire, le SR650 V4 améliore considérablement les performances du V3, optimisant ainsi le fonctionnement du sous-système. Il prend en charge des vitesses de mémoire DDR5 jusqu'à 6 400 MHz (1 DIMM par canal), un gain important par rapport à la limite de 4 800 MHz de la génération précédente. Le système dispose de 32 emplacements DIMM (16 par processeur) répartis sur huit canaux mémoire par processeur. Il est compatible avec les modules RDIMM standard, les modules RDIMM 3DS et les nouveaux modules MRDIMM (Multiplexed Rank DIMM), qui peuvent fonctionner à des vitesses allant jusqu'à 8 000 MHz pour les applications gourmandes en bande passante. Avec des modules RDIMM 3DS de 256 Go, le serveur prend en charge une capacité de mémoire système totale impressionnante de 8 To.

De plus, la version 4 introduit la prise en charge de l'extension de mémoire CXL 2.0 (Compute Express Link). Les administrateurs peuvent installer jusqu'à 12 modules de mémoire CXL dans les baies de disques E3.S (plus précisément au format E3.S 2T). Ceci permet au système d'étendre la capacité et la bande passante de la mémoire au-delà des limites traditionnelles du processeur, réduisant ainsi la latence de calcul pour les charges de travail de nouvelle génération et diminuant le coût total de possession (TCO) en optimisant l'utilisation de la mémoire inutilisée.

### Stockage

Les capacités de stockage du Lenovo SR650 V4 témoignent d'une évolution vers une densité NVMe haute performance et des formats flexibles. Tout en prenant en charge les disques 3.5 pouces et 2.5 pouces classiques, le V4 introduit une compatibilité massive avec le format NVMe E3.S. Le système peut accueillir jusqu'à 32 disques E3.S de 1 To ou 12 disques E3.S de 2 To, offrant une densité supérieure et une gestion thermique améliorée par rapport aux SSD U.2 traditionnels.

Point essentiel, la V4 prend en charge la connectivité NVMe directe sans sursouscription (1:1), garantissant ainsi que le stockage haute vitesse ne soit pas limité par le partage des lignes PCIe. L'intégration d'options de disque de démarrage M.2 remplaçables à chaud améliore encore la facilité de maintenance.

### Networking

Pour la connectivité réseau, la SR650 V4 dispose de deux emplacements OCP 3.0 dédiés, compatibles PCIe Gen 5 x16. Cette mise à niveau majeure permet une connectivité réseau redondante et haut débit (par exemple, deux ports 200 GbE ou un port 400 GbE) sans monopoliser les emplacements d'extension PCIe standard. Le passage à la norme PCIe 5.0 double la bande passante théorique (32 GT/s contre 16 GT/s), permettant ainsi à la V4 de gérer le trafic réseau le plus exigeant pour le cloud et l'IA.

### Puissance et refroidissement

Le SR650 V4 propose des options d'alimentation améliorées, de 800 W à 3 200 W, disponibles en versions Titane et Platine. Le système prend également en charge les options -48 V CC et HVAC/HVDC pour répondre aux exigences spécifiques des centres de données.

Pour gérer la chaleur dégagée par les processeurs et la mémoire à forte consommation, Lenovo propose les solutions de refroidissement liquide Neptune. Le module « Compute Complex Neptune Core Module » utilise un système de refroidissement liquide en circuit ouvert pour évacuer la chaleur des processeurs, de la mémoire et des régulateurs de tension. Il est capable de capter plus de 80 % de la chaleur des serveurs et de réduire considérablement les coûts de refroidissement des centres de données.

Globalement, le SR650 V4 représente un véritable bond en avant en termes de performances, de densité et d'efficacité thermique pour les environnements d'entreprise modernes.

## Spécifications du Lenovo Think System SR650 V4

| Spécifications | DÉTAILS |
|---|---|
| Facteur de forme | rack 2U |
| Processeur | Jusqu'à 2 processeurs Intel Xeon série 6700/6400 (cœurs P) ; jusqu'à 86 cœurs par processeur ; TDP jusqu'à 350 W |
| Mémoire | 32 emplacements DIMM (16 par processeur) ; prend en charge les modules TruDDR5 RDIMM jusqu'à 6 400 MHz et les modules MRDIMM jusqu'à 8 000 MHz. |
| Extension de mémoire | Prend en charge les modules de mémoire CXL 2.0 au format E3.S 2T (jusqu'à 12 modules DIMM). |
| Mémoire maximale | Jusqu'à 8 To de mémoire système (utilisant des modules RDIMM 3DS de 256 Go) |
| Baies de lecteur de disque | Avant : Jusqu'à 24 disques NVMe/SAS/SATA 2.5 pouces, 16 disques SAS/SATA 3.5 pouces ou 32 disques NVMe E3.S. Milieu : Jusqu'à 8 disques de 2.5 pouces à échange simple. Arrière : Jusqu'à 8 disques de 2.5 pouces ou 4 disques de 3.5 pouces remplaçables à chaud |
| Contrôleur de stockage | Jusqu'à 36 ports NVMe intégrés (connectivité 1:1) ; prise en charge RAID/HBA |
| Interfaces réseau | Deux emplacements OCP 3.0 SFF avec interface hôte PCIe 5.0 x16 |
| Emplacements d'extension PCI | Jusqu'à 10 emplacements PCIe 5.0 (à l'arrière) ; emplacements PCIe avant en option |
| Prise en charge du GPU | Jusqu'à 10 GPU simple largeur ou 2 GPU double largeur |
| Ports | Avant : Port de diagnostic externe, USB en option et Mini-DP. Arrière : 2 ports USB 3.0, 1 port VGA, 1 port RJ-45 de gestion, port série en option |
| Refroidissement | Jusqu'à 6 ventilateurs remplaçables à chaud (redondance N+1) ; modules de refroidissement liquide Neptune en option |
| Alimentation | Jusqu'à deux alimentations AC/DC redondantes remplaçables à chaud (800 W – 3200 W) |
| Systems Management | Contrôleur XClarity 3 (XCC3) ; XCC3 Premier en option |
| Caractéristiques de sécurité | TPM 2.0, racine de confiance PFR (NIST SP800-193), détection d'intrusion dans le châssis, lunette verrouillable |
| Systèmes d'exploitation | Serveur Microsoft Windows, RHEL, SLES, Serveur Ubuntu |
| Garantie | Garantie de base de 3 ans (configurable) |
| Dimensions | 87 mm (3.4 po) H x 440 mm (17.3 po) L x 800 mm (31.5 po) P |
| Poids | Poids maximal : 38.8 kg (85.5 lb) |

## Conception et fabrication du Lenovo ThinkSystem SR650 V4

Le Lenovo ThinkSystem SR650 V4 est une plateforme 2U standard qui optimise son encombrement, en conciliant densité des composants, ventilation et facilité de maintenance. Son châssis mesure 3.4 cm de hauteur, 17.3 cm de largeur et 31.5 cm de profondeur, pour un poids maximal de 85.5 kg. Ces dimensions le placent au même niveau que les autres serveurs d'entreprise 2U, tout en offrant une profondeur suffisante pour les processeurs hautes performances, les configurations mémoire haute densité et les fonds de panier de stockage flexibles.

Le design industriel ThinkSystem de Lenovo est immédiatement reconnaissable, avec son châssis en acier rigide et son étiquetage clair et fonctionnel. À l'intérieur, l'agencement est épuré et fonctionnel, privilégiant la circulation d'air directe, les zones de composants modulaires et l'accès sans outil pour la maintenance courante. La construction générale inspire confiance et témoigne d'une ingénierie de pointe, reflétant une plateforme conçue pour un fonctionnement continu en environnement de centre de données plutôt que pour l'esthétique.

### Panneau avant

Le panneau avant est hautement configurable. Notre système de test est équipé d'un fond de panier 8 baies 2.5 pouces, mais le châssis peut facilement être configuré avec 16 ou même 24 baies. Les clients peuvent également opter pour des baies 3.5 pouces pour une capacité brute supérieure ou les nouveaux fonds de panier E3.S pour une densité NVMe haute performance. La compatibilité AnyBay permet aux disques SAS, SATA et NVMe de coexister dans le même châssis, offrant une grande flexibilité pour les configurations de stockage mixtes.

Le panneau avant comprend les commandes essentielles : un bouton marche/arrêt, un bouton d'identification et des voyants d'état indiquant le fonctionnement du système et l'activité réseau. Une languette d'information amovible permet d'accéder rapidement à l'étiquette d'accès réseau XCC et au numéro de série. La connectivité locale optionnelle inclut un port Mini DisplayPort et des ports USB pour l'accès via un chariot d'intervention, indispensable pour le dépannage dans les centres de données hors ligne.

### Panneau arrière

L'arrière du châssis est conçu pour une densité d'E/S maximale. Il peut accueillir jusqu'à 10 emplacements PCIe Gen 5, selon la configuration des cartes d'extension. L'intégration de deux emplacements OCP 3.0 libère les cartes d'extension PCIe standard pour d'autres cartes d'extension, telles que des GPU ou des contrôleurs de stockage.

Outre l'extension, le panneau arrière abrite le port réseau de gestion BMC intégré, deux ports USB-A et une sortie VGA pour l'accès à la console locale. Deux alimentations remplaçables à chaud sont montées à l'arrière, préservant ainsi la facilité d'entretien sans impacter la circulation de l'air ni les possibilités d'extension.

### Interne

À l'intérieur, l'agencement est épuré et optimisé pour la circulation de l'air. 32 emplacements DIMM encadrent deux sockets CPU, disposés de manière à assurer un refroidissement optimal de l'avant vers l'arrière grâce à un ensemble de six ventilateurs haute performance remplaçables à chaud, situés à l'avant du châssis. Ces ventilateurs sont entièrement démontables et remplaçables sans outil, et le plateau de ventilateurs est amovible sans outil. Le retrait de ce plateau offre un accès direct et dégagé à la baie de stockage AnyBay, simplifiant ainsi la maintenance, les mises à niveau et le changement de fond de panier.

Les câbles de stockage sont soigneusement acheminés le long des côtés du châssis afin de ne pas obstruer la circulation de l'air vers les processeurs et la mémoire. Pour les configurations utilisant des modules de refroidissement liquide Neptune, l'agencement interne est légèrement modifié afin d'intégrer les circuits de refroidissement, mais l'accessibilité des composants reste optimale. Le module de disque de démarrage M.2 est monté sur le déflecteur d'air ou la cage de disques, ce qui permet un accès facile sans avoir à démonter d'autres composants.

### Contrôleur XClarity 3

Le SR650 V4 intègre le nouveau contrôleur XClarity 3 (XCC3). Ce moteur de gestion offre des performances nettement supérieures aux générations précédentes, avec des temps de démarrage plus rapides et une interface HTML5 plus réactive. Le gestionnaire de ressources de la plateforme fournit des données télémétriques détaillées sur l'alimentation et la température, permettant aux administrateurs d'optimiser l'efficacité du centre de données.

XCC3 prend en charge un large éventail de fonctions de gestion à distance, notamment le contrôle à distance (KVM), le montage de supports virtuels et les mises à jour du firmware. Son interface intuitive offre une vue d'ensemble de l'état du système, des événements en cours et de l'inventaire matériel. Pour l'automatisation, XCC3 est entièrement compatible avec les API REST de Redfish.

## Performances du Lenovo ThinkSystem SR650 V4

Cette section examine les résultats des tests de performances réalisés avec Blender, y-cruncher, vLLM et Phoronix. Les tests initiaux ont été effectués sur un Lenovo ThinkSystem SR650 V4 équipé de processeurs Intel Xeon 6740P, ce qui nous a permis d'évaluer l'architecture P-core axée sur les performances et le débit global de la plateforme sous des charges de travail gourmandes en ressources CPU. En cours de test, nous avons modifié la configuration du système pour activer l'accélération GPU. La transition s'est faite sans difficulté grâce au kit de mise à niveau GPU de Lenovo, qui comprend des ventilateurs hautes performances, des dissipateurs thermiques améliorés, un carénage de flux d'air repensé et des risers compatibles GPU conçus pour supporter des accélérateurs plus puissants. L'installation n'a nécessité aucune modification importante du châssis et s'est parfaitement intégrée à la configuration système existante.

Grâce à l'installation des composants de refroidissement et de ventilation améliorés, nous avons intégré un GPU NVIDIA L40S à la plateforme. Ceci nous a permis d'étendre nos tests aux charges de travail accélérées par GPU, telles que le rendu GPU sous Blender et les benchmarks axés sur l'inférence, tout en garantissant une température stable et un comportement système constant. La modularité de cette mise à niveau souligne la flexibilité du SR650 V4, lui permettant de passer facilement d'une configuration CPU axée sur l'efficacité à une plateforme CPU+GPU équilibrée, adaptée aux charges de travail de calcul mixtes.

### Configuration du Lenovo ThinkSystem SR650 V4 :

- **CPU:** 2 processeurs Intel Xeon 6740P
- **Mémoire:** 1 To de RAM
- **Stockage:** 4 SSD de 960 Go
- **GPU:** Nvidia L40S

## Blender 4.5

Blender est une application de modélisation 3D open source. Ce benchmark a été réalisé avec l'utilitaire Blender Benchmark. Le score est mesuré en échantillons par minute, les valeurs les plus élevées indiquant de meilleures performances.

Dans le test de performances CPU de Blender, le Lenovo ThinkSystem SR650 V4 offre d'excellentes performances de rendu sur toutes les scènes, avec un score de 1136.99 échantillons par minute pour Monster, 707.60 pour Junkshop et 562.53 pour Classroom. Ces résultats témoignent d'une forte capacité de traitement multithread grâce aux deux processeurs Intel Xeon 6740P, garantissant des performances constantes malgré la complexité croissante des scènes.

| Processeur de Blender (Échantillons par minute ; plus c'est élevé, mieux c'est) | Lenovo ThinkSystem SR650 V4 (2x Intel Xeon 6740P, 1 To de RAM) |
|---|---|
| Monster | 1136.99 |
| Junkshop | 707.60 |
| Classroom | 562.53 |

Dans le benchmark Blender CPU sans SMT, le SR650 V4 affiche une nette amélioration du débit de rendu, atteignant 4 686,40 échantillons par minute dans Monster, 2 223,96 dans Junkshop et 2 385,98 dans Classroom. Les résultats GPU soulignent la capacité du système à accélérer les charges de travail de rendu complexes, offrant un débit considérablement supérieur au rendu CPU seul.

| Processeur Blender (sans SMT) (Échantillons par minute ; plus c'est élevé, mieux c'est) | Lenovo ThinkSystem SR650 V4 (2x Intel Xeon 6740P, 1 To de RAM) |
|---|---|
| Monster | 4686.40 |
| Junkshop | 2223.96 |
| Classroom | 2385.98 |

## y-cruncher

y-cruncher est un programme multithread et évolutif qui calcule Pi et d'autres constantes mathématiques avec une précision de plusieurs billions de décimales. Depuis son lancement en 2009, il est devenu une application populaire de test de performance et de résistance pour les overclockeurs et les passionnés de matériel informatique.

Dans y-cruncher, le Lenovo affiche une montée en charge fluide face à l'augmentation de la taille des problèmes, soulignant ainsi les excellentes performances de calcul multithread et la bande passante mémoire de la plateforme. Avec deux processeurs Intel Xeon 6740P et 1 To de RAM, le système effectue le calcul de Pi à 1 milliard de décimales en un peu plus de 20 secondes et conserve une efficacité constante jusqu'à 25 milliards de décimales, en 362 secondes. Ces résultats démontrent une montée en charge prévisible sous une pression soutenue sur le processeur et la mémoire, faisant du SR650 V4 un choix idéal pour les charges de travail mathématiques importantes et les tests de résistance.

| Y-Cruncher (temps de calcul total) | Lenovo ThinkSystem SR650 V4 (2x Intel Xeon 6740P, 1 To de RAM) |
|---|---|
| 1 milliard | 20.715 s |
| 2.5 milliard | 44.412 s |
| 5 milliard | 81.937 s |
| 10 milliard | 152.743 s |
| 25 milliard | 362.566 s |

## Performances d'inférence LLM du service en ligne vLLM

vLLM est le moteur d'inférence et de diffusion à haut débit le plus populaire pour les LLM. Le benchmark de diffusion en ligne vLLM est un outil d'évaluation des performances qui mesure les capacités de diffusion réelles de ce moteur d'inférence sous des requêtes simultanées. Il simule les charges de travail de production en envoyant des requêtes à un serveur vLLM en cours d'exécution avec des paramètres configurables, tels que le débit de requêtes, la longueur des entrées/sorties et le nombre de clients simultanés. Le benchmark mesure des indicateurs clés, notamment le débit (jetons par seconde), le temps d'obtention du premier jeton et le temps par jeton de sortie (TPOT), permettant ainsi aux utilisateurs de comprendre les performances de vLLM sous différentes conditions de charge.

Nous avons testé les performances d'inférence sur une suite complète de modèles couvrant diverses architectures, échelles de paramètres et stratégies de quantification, et évalué le débit sous différents profils de concurrence.

### Performances du modèle dense

Les modèles denses suivent l'architecture LLM conventionnelle, dans laquelle tous les paramètres et activations sont utilisés lors de l'inférence, ce qui engendre un traitement plus intensif en ressources de calcul que leurs homologues clairsemés. Afin d'évaluer de manière exhaustive les performances des différents modèles, en fonction de leur échelle et des stratégies de quantification, nous avons comparé plusieurs configurations de modèles denses de la famille Llama 3.1 8B.

## Llama 3.1 8B Precision FP8

Le modèle Llama 3.1 8B, exécuté en précision FP8, présente un profil de mise à l'échelle différent de celui de la configuration en précision standard. Il privilégie l'efficacité à des niveaux de concurrence modérés, au détriment du débit maximal pour des tailles de lots plus importantes. En mode mono-utilisateur (BS=1), le modèle atteint 67.8 tok/s par utilisateur, pour un débit total de 135.6 tok/s et un TPOT d'environ 3.1 ms, établissant ainsi une performance de base inférieure en flux unique par rapport à la précision maximale.

À mesure que la taille des lots augmente, le débit total croît rapidement tandis que le débit par utilisateur diminue de manière contrôlée. À BS=2, le débit total atteint 271 tok/s, soit 66.8 tok/s par utilisateur. À BS=4, le débit total atteint 523 tok/s et à BS=8, le modèle fournit 1 017 tok/s, soit 63.6 tok/s par utilisateur. La latence reste stable à ces faibles niveaux de concurrence, ce qui rend FP8 particulièrement adapté aux scénarios d'inférence multi-utilisateurs légers.

La mise à l'échelle se poursuit jusqu'à BS=16, où le modèle maintient un débit total de 1 918 tok/s, soit 60.0 tok/s par utilisateur. À BS=32, le débit total se stabilise à environ 1 920 tok/s, le débit par utilisateur chutant à 30.0 tok/s. Ce plateau indique que la configuration FP8 atteint son point de saturation plus tôt que la précision standard, privilégiant l'efficacité et la prévisibilité au détriment du débit agrégé maximal.

Globalement, FP8 permet d'obtenir un débit total environ 14 fois supérieur entre BS=1 et son pic à BS=16-32. La latence reste constante jusqu'à BS=16, puis se stabilise parallèlement au débit à des niveaux de concurrence plus élevés, ce qui souligne que FP8 est un choix pratique pour les charges de travail d'inférence équilibrées et sensibles à la latence plutôt que pour une mise à l'échelle extrême par lots.

## Llama 3.1 8B Standard Précision

Avec une précision standard, le modèle Llama 3.1 8B présente un comportement prévisible en termes de mise à l'échelle à faible concurrence, avec d'excellentes performances par utilisateur pour les petits lots et un débit agrégé en constante augmentation avec la concurrence. En mode mono-utilisateur (BS=1), le modèle atteint environ 45.8 tok/s par utilisateur, soit un débit total de 91.6 tok/s, établissant ainsi une base solide pour les charges de travail sensibles à la latence.

À mesure que la concurrence augmente, le débit total croît efficacement tandis que le débit par utilisateur diminue progressivement. À BS=2, le débit total atteint 176 tok/s, soit 44.0 tok/s par utilisateur, et à BS=4, il atteint 344 tok/s, soit 43.0 tok/s par utilisateur. Ce comportement se maintient jusqu'à BS=8, où le modèle conserve un débit total de 672 tok/s, soit 42.0 tok/s par utilisateur, ce qui indique une bonne utilisation sans dégradation significative du débit par utilisateur.

La mise à l'échelle se poursuit jusqu'à BS=16, où le débit total atteint un pic d'environ 1 280 tok/s, soit 40.0 tok/s par utilisateur. À BS=32, le débit total reste stable autour de 1 280 tok/s, tandis que le débit par utilisateur chute à 20.0 tok/s, signalant la saturation du pipeline d'exécution. Ce plateau suggère qu'à précision standard, le modèle atteint son point de fonctionnement optimal autour de BS=16, offrant un équilibre entre débit et réactivité.

Globalement, la précision standard permet d'obtenir un débit total environ 14 fois supérieur entre BS=1 et son débit maximal, avec des performances stables par utilisateur pour des niveaux de concurrence modérés. Ce profil rend la précision standard particulièrement adaptée aux déploiements privilégiant une latence constante et une mise à l'échelle prévisible plutôt qu'une expansion par lots trop rapide.

### Performances du modèle parcimonieux

Les modèles épars, notamment les architectures Mixture of Experts (MoE), constituent une approche émergente pour la mise à l'échelle efficace des modèles de langage. Ces architectures conservent un nombre total de paramètres élevé tout en n'activant qu'un sous-ensemble de paramètres par jeton, ce qui peut potentiellement améliorer les performances par paramètre actif.

### Performances du Qwen3-Coder-30B-A3B FP8

Le modèle Qwen3-Coder-30B-A3B, exécuté en précision FP8, présente un profil de mise à l'échelle optimisé pour une concurrence modérée, offrant d'excellentes performances par utilisateur tout en atteignant la saturation plus rapidement que les modèles plus petits. En mode mono-utilisateur (BS=1), le modèle atteint environ 98 tok/s par utilisateur, pour un débit total de 196 tok/s, établissant ainsi une base de référence élevée pour un flux unique, témoignant de son optimisation pour les charges de travail de génération de code.

À mesure que la concurrence augmente, le débit total croît efficacement tandis que le débit par utilisateur diminue de manière contrôlée. Avec BS=2, le débit total atteint 317 tok/s, soit 79 tok/s par utilisateur, et avec BS=4, le modèle fournit un débit total de 477 tok/s, soit 59 tok/s par utilisateur. Ces résultats démontrent une utilisation efficace des ressources de calcul disponibles grâce à un traitement par lots faible à modéré, sans dégradation brutale de la réactivité par utilisateur.

Le modèle atteint son débit agrégé maximal à BS=8, se maintenant à environ 905 tok/s de débit total, soit 56 tok/s par utilisateur. La mise à l'échelle se poursuit marginalement jusqu'à BS=16, où le débit total augmente légèrement à 920 tok/s, tandis que le débit par utilisateur chute à 29 tok/s, indiquant la saturation du pipeline d'inférence. Au-delà de ce point, l'augmentation de la concurrence n'apporte que des gains minimes en termes de débit total, tout en impactant significativement les performances par utilisateur.

Globalement, la configuration Qwen3-Coder-30B-A3B FP8 offre un débit total environ 4.7 fois supérieur entre BS=1 et son débit maximal à BS=16. Les caractéristiques de latence et de débit indiquent que cette configuration est idéale pour les charges de travail de codage multi-utilisateurs modérées nécessitant des performances élevées par requête. Toutefois, le traitement par lots à grande échelle n'est pas son objectif principal.

### Performances des types de données à micro-échelle

La micro-échelle représente une approche de quantification avancée qui applique des facteurs d'échelle précis à de petits blocs de poids, plutôt qu'une quantification uniforme à de grands groupes de paramètres. Le format NVFP4 de NVIDIA implémente cette technique grâce à une représentation en virgule flottante par blocs, où chaque bloc de micro-échelle de 8 à 32 valeurs partage un exposant typique comme facteur d'échelle. Cette approche granulaire préserve la précision numérique tout en assurant une représentation sur 4 bits, maintenant ainsi la plage dynamique essentielle aux architectures de transformateurs. Ce format s'intègre à l'architecture Tensor Core de NVIDIA, permettant un calcul efficace en précision mixte avec décompression à la volée lors des opérations matricielles.

### Performances de GPT-OSS-20b

Le modèle gpt-oss-20b présente d'excellentes performances de mise à l'échelle pour des niveaux de concurrence croissants, avec un débit agrégé particulièrement élevé pour les lots de grande taille. En mode mono-utilisateur (BS=1), le modèle atteint environ 138 tok/s par utilisateur, soit un débit total de 276 tok/s, établissant ainsi une base solide pour l'inférence mono-flux.

À mesure que la concurrence augmente, le débit total croît fortement tandis que le débit par utilisateur diminue, comme prévu. Avec BS=2, le débit total atteint 420 tok/s, soit 105 tok/s par utilisateur, et avec BS=4, il atteint 690 tok/s au total, soit 86 tok/s par utilisateur. Cette progression constante témoigne d'une utilisation efficace des ressources de calcul disponibles grâce à un traitement par lots faible à modéré.

La mise à l'échelle se poursuit jusqu'à BS=8, où le modèle atteint un débit total d'environ 1 120 tok/s à raison de 70 tok/s par utilisateur, et jusqu'à BS=16, où le débit total passe à 1 900 tok/s, soit 60 tok/s par utilisateur. Ces résultats montrent que gpt-oss-20b conserve des performances relativement élevées par utilisateur même lorsque la concurrence augmente, ce qui le rend particulièrement adapté aux scénarios de service multi-utilisateurs.

Le modèle atteint son débit agrégé maximal à BS=32, avec un débit total d'environ 3 250 tok/s, le débit par utilisateur chutant à 50 tok/s. Cela représente une augmentation de 11.8 fois du débit total par rapport aux performances d'un utilisateur unique. Bien que le débit par utilisateur continue de diminuer pour des tailles de lots plus importantes, l'efficacité globale de la mise à l'échelle reste élevée, ce qui indique que le modèle tire parti d'une concurrence accrue sans atteindre de point de saturation prématuré.

Globalement, gpt-oss-20b présente un profil de scalabilité équilibré, alliant d'excellentes performances mono-utilisateur à une forte capacité de traitement par lots. Il constitue ainsi une option intéressante pour les déploiements nécessitant à la fois une inférence individuelle réactive et un débit agrégé élevé sous une charge multi-utilisateurs importante.

## Points de repère Phoronix

Phoronix Test Suite est une plateforme d'analyse comparative automatisée et open source qui prend en charge plus de 450 profils de test et plus de 100 suites de tests via OpenBenchmarking.org. Elle gère l'ensemble du processus, de l'installation des dépendances à l'exécution des tests et à la collecte des résultats, ce qui la rend idéale pour les comparaisons de performances, la validation matérielle et l'intégration continue.

**Bande passante de la mémoire de flux** : Le SR650 V4 a fourni une bande passante mémoire de 369.6 Go/s, démontrant un débit élevé pour les charges de travail gourmandes en données et confirmant des performances mémoire multicanaux saines.

**Compression à 7 zips** : Avec 584 499 MIPS, le système a affiché d'excellentes performances de compression et de décompression, reflétant une solide efficacité multicœur et une forte capacité de calcul entier.

**Compilation du noyau** : Le serveur a terminé la compilation du noyau allmod en 256.175 secondes, un résultat respectable qui souligne sa capacité à gérer efficacement la compilation parallèle et les flux de travail des développeurs.

**Serveur Web Apache** : Avec 61 654 R/s, le SR650 V4 a démontré de solides performances de serveur Web, maintenant un débit de requêtes élevé adapté à l'hébergement frontal ou aux piles de virtualisation légères.

**Vérification OpenSSL** : Atteignant 712.6 milliards d'octets par seconde, la plateforme a démontré des performances robustes en matière de vérification cryptographique, confirmant ainsi la capacité du processeur à effectuer des opérations sécurisées nécessitant de nombreux certificats.

| Points de repère Phoronix | Lenovo Think System SR650 V4 |
|---|---|
| Stream (bande passante mémoire) | 369.6 Go/s |
| 7-ZIP | 584 499 MIPS |
| Compilation du noyau (allmod) | 256.175 secondes |
| Apache (requêtes par seconde) | 61 654 R/s |
| OpenSSL | 712 633 776 990 octets/s |

## Conclusion

Le ThinkSystem SR650 V4 représente une mise à jour majeure, car elle ne se limite pas à un simple changement de processeur. Lenovo a profité de cette génération pour apporter des améliorations significatives dans les domaines qui limitent les déploiements en conditions réelles : bande passante mémoire, disponibilité des lignes PCIe, densité de stockage et capacité d'adaptation de la configuration aux besoins évolutifs. Le passage à l'Intel Xeon 6 offre davantage d'options de cœurs et une capacité d'E/S accrue par socket. De plus, les améliorations apportées à la plateforme, notamment la prise en charge de la DDR5 plus rapide avec MRDIMM et l'extension CXL optionnelle, permettent au SR650 V4 de rester compétitif face à des charges de travail toujours plus gourmandes en mémoire.

Côté stockage, malgré notre configuration de test limitée avec U.2, l'adoption par Lenovo de la norme E3.S offre une densité NVMe supérieure avec une connectivité 1:1 et des caractéristiques thermiques optimisées, en phase avec les infrastructures modernes, notamment pour les environnements de virtualisation qui nécessitent davantage de mémoire flash locale, et pour les pipelines liés à l'IA où l'alimentation des GPU est aussi importante que la puissance de calcul brute. Les deux emplacements OCP 3.0 Gen5 x16 constituent également une amélioration pratique : vous pouvez déployer une infrastructure réseau performante sans sacrifier les emplacements PCIe que vous préférez réserver aux accélérateurs, au stockage ou aux adaptateurs spécialisés.

Si vous utilisez un SR650 V3 et que vous vous sentez limité par la vitesse de la mémoire, la marge PCIe ou la densité NVMe, le SR650 V4 représente une avancée majeure. Il conserve la facilité de maintenance et la configurabilité qui ont fait le succès de la gamme SR650, tout en offrant une plateforme évolutive qui permet aux équipes informatiques d'éviter les situations figées. Que vous construisiez un cluster de virtualisation, modernisiez des services à grande échelle ou assembliez un nœud CPU/GPU équilibré, le SR650 V4 répond aux besoins essentiels et ouvre la voie aux évolutions futures.
