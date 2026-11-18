La famille de processeurs de serveur Intel Xeon 6 a été lancée il y a quelques semaines, remplaçant l'ancienne marque « Scalable ». Il existe deux gammes de puces : Sierra Forest, qui propose des cœurs E, et Granite Rapids, qui propose des cœurs P. Dans cette revue, nous avons échantillonné quelques premiers ensembles de processeurs Sierra Forest, y compris le 6780E haut de gamme, qui commercialise 144 cœurs.
Les processeurs Xeon 6 sont conçus pour prendre en charge pratiquement toutes les charges de travail, depuis les cas d'utilisation évidents de l'IA jusqu'aux déploiements moins exigeants en périphérie. Pour aider les clients à comprendre les deux couloirs de nage, Intel indique que Sierra Forest est « optimisé pour les performances par watt dans les charges de travail de calcul haute densité et évolutives ». Le processeur P-core Granite Rapids est « optimisé pour les performances par cœur dans les charges de travail gourmandes en calcul ». Cependant, tous les processeurs Xeon 6 utilisent une plate-forme et une pile de micrologiciels communes.
Pour donner un peu plus de contexte sur les performances des processeurs, Intel positionne Sierra Forest comme un excellent remplacement pour les systèmes ayant un cycle de rafraîchissement de 5 ans. Dans ce cas, les chiffres de performance par watt devraient être nettement plus favorables.
Le déploiement du Xeon 6 se déroulera par étapes, avec le lancement de la famille 6700E aujourd'hui. Mais il y a bien plus à venir. Les processeurs Intel Xeon 6900P devraient sortir cette année au troisième trimestre. Nous nous attendons à voir un déluge de puces, notamment les 3E, 6900P, 6700P, Xeon 6500 SoC et 6P, au cours du premier trimestre 6300.
Innovations architecturales
Le Xeon 6 présente deux conceptions de microarchitecture, une plate-forme de cœurs optimisés en termes de performances (cœurs P) et une plate-forme de cœurs optimisés en efficacité (cœurs E). Cette approche hybride permet aux centres de données de concevoir un rack dans lequel les charges de travail à forte intensité de calcul, telles que l'IA et le HPC, bénéficient de performances maximales tandis que les tâches orientées débit, comme les microservices et la mise en réseau, atteignent un nouveau niveau d'efficacité énergétique.
Augmentation du nombre de cœurs
La principale caractéristique des processeurs Xeon 6 Sierra Forest est l'augmentation significative du nombre de cœurs. En intégrant davantage de cœurs par processeur, Intel permet aux centres de données de gérer simultanément un plus large éventail de charges de travail. Cette augmentation de la densité du cœur est particulièrement bénéfique pour les applications qui nécessitent des niveaux élevés de traitement parallèle, garantissant que les ressources sont utilisées de manière optimale pour maximiser le débit et minimiser la latence.
Bande passante mémoire améliorée
Depuis quelques générations, on a l’impression que nous sommes coincés avec des vitesses de DRAM de serveur plus lentes alors que le marché grand public a basculé jusqu’à bien au-dessus de 7000 5 MT/s. L'intégration de la mémoire DDR2.0 avec Ultra Path Interconnect (UPI) 6 dans les processeurs Xeon XNUMX Sierra Forest améliore considérablement la bande passante mémoire. Cela permet un accès plus rapide aux données et réduit les goulots d'étranglement, garantissant ainsi le bon fonctionnement des applications hautes performances. L'amélioration de la bande passante mémoire est essentielle pour les applications qui gèrent de grands ensembles de données et nécessitent une récupération et un traitement rapides des données, comme la formation à l'IA et l'analyse du Big Data.
Capacités d'E/S avancées d'Intel Xeon 6
La prise en charge de PCIe 5.0 et Compute Express Link (CXL) 2.0 offre aux processeurs Xeon 6 Sierra Forest des capacités d'E/S avancées. PCIe 5.0 offre le double de la bande passante de son prédécesseur, permettant une communication plus rapide entre le processeur et les périphériques. CXL 2.0 améliore encore la connectivité en fournissant une interconnexion à large bande passante et à faible latence pour les processeurs, les accélérateurs, la mémoire et le stockage. Cela garantit que les centres de données peuvent répondre aux exigences des applications modernes à haut débit et s'intégrer de manière transparente aux technologies futures.
Architecture modulaire multi-matrices
Les processeurs Xeon 6 Sierra Forest utilisent une architecture multi-puces modulaire activée par la technologie Embedded Multi-die Interconnect Bridge (EMIB). Cette conception permet de combiner plusieurs puces dans un seul boîtier, offrant une bande passante élevée et une faible latence tout en maintenant une consommation d'énergie efficace. L'approche modulaire offre flexibilité et évolutivité, permettant aux centres de données de personnaliser leur infrastructure pour répondre aux exigences spécifiques de la charge de travail. Cette architecture prend également en charge une meilleure gestion thermique et une meilleure efficacité énergétique, qui sont essentielles au maintien des performances et de la fiabilité dans les environnements haute densité.
Architecture de matrice Intel Xeon 6
L'architecture des puces des processeurs Xeon 6 Sierra Forest constitue un aperçu intéressant de l'approche innovante d'Intel visant à maximiser les performances et l'efficacité. Contrairement à un grand interposeur en silicium traditionnel, EMIB implémente une petite puce de pont avec plusieurs couches de routage. En tirant parti de la technologie EMIB, Intel peut interconnecter plusieurs puces dans un seul boîtier, réduisant ainsi la taille globale du boîtier et améliorant l'intégrité du signal. Cette configuration permet des taux de transfert de données plus rapides entre les matrices, ce qui est essentiel pour maintenir des performances élevées dans une large gamme d'applications.
Importance pour les centres de données
Performances et efficacité
Par rapport aux générations précédentes, les processeurs Xeon 6 offrent des performances par watt jusqu'à 2.7 fois supérieures, ce qui les rend idéaux pour l'inférence d'IA, le transcodage multimédia et les tâches de calcul générales. Cet équilibre garantit que les centres de données peuvent gérer davantage de charges de travail avec une consommation d'énergie inférieure, ce qui se traduit directement par une réduction des coûts opérationnels et une durabilité améliorée.
Scalabilité et flexibilité
L'architecture modulaire permet aux centres de données de personnaliser leur infrastructure en fonction des demandes de charge de travail spécifiques. La possibilité de déployer une combinaison de cœurs P et de cœurs E sur plusieurs plates-formes offre une approche personnalisée pour gérer divers besoins informatiques, améliorant à la fois les performances et l'efficacité. Cela présente un concept intéressant dans lequel un châssis peut être sélectionné puis déployé, certains disposant de processeurs E-core disponibles pour exécuter efficacement les services de base et gérer les opérations à des moments moins exigeants, tandis que d'autres sont équipés de processeurs P-core pour gérer la demande de pointe et charges de travail exigeantes.
Sécurité et fiabilité améliorées
Construits avec des fonctionnalités de sécurité améliorées au niveau matériel, les processeurs Xeon 6 offrent une protection robuste pour l'intégrité des données et la fiabilité du système. Ceci est essentiel pour maintenir la confiance et la conformité dans les environnements sensibles aux données.
Pérennité grâce aux technologies avancées
Prenant en charge les dernières technologies telles que DDR5, PCIe 5.0 et CXL 2.0, les processeurs Xeon 6 garantissent que les centres de données sont prêts pour les avancées futures et peuvent s'intégrer de manière transparente aux solutions matérielles et logicielles émergentes.
Des solutions modernes pour des problèmes modernes
Les processeurs Intel Xeon 6 Sierra Forest représentent une avancée majeure dans l'architecture des centres de données. En combinant hautes performances, efficacité énergétique et évolutivité, ils répondent aux besoins multiformes des centres de données modernes, ouvrant la voie à une efficacité opérationnelle améliorée, à des coûts réduits et à la capacité de répondre aux demandes croissantes de l'IA et d'autres applications à forte intensité de calcul.
Carte des références Intel Xeon 6 E-core
Toutes les références Sierra Forest comportent des E-Cores avec 88 voies PCIe Gen5/CXL.
| SKU | Coloré | GHz de base | TurboGHz | Turbo-GHz maximum | Mo de cache L3 | Watts TDP | Max Scala. | Vitesse mémoire DDR5 1DPC | 
|---|---|---|---|---|---|---|---|---|
| 6780E | 144 | 2.2 | 3.0 | 3.0 | 108 | 330 | 2S | 6400 | 
| 6766E | 144 | 1.9 | 2.7 | 2.7 | 108 | 250 | 2S | 6400 | 
| 6756E | 128 | 1.8 | 2.6 | 2.6 | 96 | 225 | 2S | 6400 | 
| 6746E | 112 | 2.0 | 2.7 | 2.7 | 96 | 250 | 2S | 5600 | 
| 6740E | 96 | 2.4 | 3.2 | 3.2 | 96 | 250 | 2S | 6400 | 
| 6731E | 96 | 2.2 | 3.1 | 3.1 | 96 | 250 | 1S | 5600 | 
| 6710E | 64 | 2.4 | 3.2 | 3.2 | 96 | 205 | 2S | 5600 | 
Tests de performances Intel Xeon 6
Notre laboratoire a échantillonné deux ensembles de processeurs pour cette revue, le 6780E et le 6766E. Intel a fourni une plate-forme serveur QCT pour les tests. Nous souhaitons noter quelques mises en garde clés dans nos données. La plate-forme serveur elle-même était instable dans bon nombre de nos environnements de test. Par exemple, Windows Server 2022 ne fonctionnerait pas correctement, nous avons donc utilisé 2025.
Les processeurs sont des échantillons précoces et non des processeurs neufs dans la boîte. En tant que tel, nous n’avons pas pu exécuter toute notre batterie de tests et n’avons pas eu le temps avant la levée de l’embargo de résoudre correctement certains des problèmes, de performances et de stabilité que nous constatons. En tant que telles, les données suivantes doivent être considérées comme instructives et non définitives. Nous attendrons la livraison des plates-formes par les constructeurs OEM d'Intel avant de porter un jugement plus concluant sur les capacités des processeurs Sierra Forest.
Intel nous a expédié un système Quanta QuantaGrid D55Q-2U comme plate-forme de test pour présenter les nouveaux processeurs. Ce serveur propose un fond de panier à connexion directe NVMe de 24 baies prenant en charge les SSD U.2 Gen4/Gen5.
Notre configuration de test comprenait 16 x 16 Go de RAM DDR5-6400 et des modules Micron MTC10F1084S1RC64BDY.
Xeon 6780E et Xeon 6766E
- Quanta QuantaGrid D55Q-2U
- 256GB DDR5 6400MHz
Xeon Platine 8592+
- Dell Poweredge R760
- 1 To DDR5 4800 XNUMX MHz
Xeon Gold 6430
- Dell Poweredge R760
- 1 To DDR5 4800 XNUMX MHz
Xeon Platine 8480+
- HP ML350 génération 11
- 256GB DDR5 4800MHz
Avec les tout premiers gremlins de la plate-forme de test en jeu, les nouveaux processeurs Xeon 6780E et 6766E ont été testés dans un environnement Windows Server 2025, tandis que les anciens processeurs ont été testés dans Windows Server 2022.
Mélangeur OptiX 4.0
Dans Blender OptiX, nous avons 3 tests différents : Monster, Junkshop et Classroom. Chez Monster, nous avons vu les 6780E et 6766E dépasser les 8592+ de 21 % et 14 %, avec une différence de 18 % entre les deux. Sur Junkshop, le 6780E était en avance de 8592 % sur le 10.5+, le 6766E étant derrière le 8592+ de seulement 0.35 %. Il y avait une différence de 10.8 % entre le 6780E et le 6766E sur Junkshop. Pour la partie salle de classe, les 6780E et 6766E ont devancé les 8592+ de 20 % et 22 % avec une différence de 10 % entre les 6780E et 6766E.
| Processeur Blender 4.0 | 2x Xeon 6780E (256 Go DDR5) | 2x Xeon 6766E (256 Go DDR5) | 2x Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 
|---|---|---|---|---|---|
| Monster | 1410.463 | 1297.715 | 1115.057 | 943.300 | 540.039 | 
| Brocanteur | 862.418 | 777.716 | 780.408 | 627.662 | 361.066 | 
| Salle de classe | 696.543 | 628.960 | 556.550 | 475.144 | 278.228 | 
Cinebench R23
Pour Cinebench R23, les performances multicœurs ont montré le 6780E et le 6766E derrière le 8592+ de 38 % et 42 %, respectivement. Pour les performances monocœur, le 6780E était 24 % derrière le 8592+ et le 6766E était 31 % derrière. La différence entre le 6780E et le 6766E est tombée à environ 5 % pour le score multicœur et à 18 % pour le monocœur.
| Cinebench R23 | 2x Xeon 6780E (256 Go DDR5) | 2x Xeon 6766E (256 Go DDR5) | 2x Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 
|---|---|---|---|---|---|
| Processeur multicœur | 67,984 | 64,326 | 110,498 | 79,164 | 69,663 | 
| Processeur monocœur | 873 | 793 | 1,144 | 1,461 | 1,022 | 
| Rapport PM | 77.91x | 81.10x | 96.63x | 54.20x | 68.17x | 
Cinebench 2024
Pour Cinebench 2024, le score multicœur a connu une baisse d'environ 55 % du 8592+ au 6780E et une baisse de 61 % du 8592+ au 6766E. Cela présentait également une différence de 13 % entre le 6780E et le 6766E sur Multi-Core. Pour les scores monocœur, les 6780E et 6766E n'avaient qu'une différence de 5 %, les 6780E et 6766E étant respectivement 37 % et 34 % derrière le 8592+.
| Cinebench R23 | 2x Xeon 6780E (256 Go DDR5) | 2x Xeon 6766E (256 Go DDR5) | 2x Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|---|---|
| Processeur multicœur | 2,687 | 2,347 | 6,001 | 3,746 | 4,699 | 
| Processeur monocœur | 43 | 45 | 68 | 59 | 76 | 
| Rapport PM | 62.85x | 52.65x | 88.48x | 63.22x | 61.44x | 
Y-Cruncher
Y-cruncher est une application populaire d'analyse comparative et de tests de résistance lancée en 2009. Ce test est multithread et évolutif, calculant Pi et d'autres constantes jusqu'à des milliards de chiffres. Plus vite, c'est mieux dans ce test.
Les nouveaux 6780E et 6766E fonctionnent un peu plus lentement que l'Emerald Rapids 8592+, mais ce ne sont pas exactement des concurrents directs. Le 6780E fonctionnait environ 13 % plus vite que le Xeon Gold 6430 pour le test 1 milliard, mais environ 39 % plus lentement que le Xeon Platinum 8592+. Le 6766E fonctionnait 19 % plus lentement que le Gold 6439 et 42 % plus lentement que le Platinum 8592+.
| Y-Cruncher (plus bas est mieux) | Xeon 6780E (256 Go DDR5) | Xeon 6766E (256 Go DDR5) | 2x Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|---|---|
| 1 milliard | 6.927 secondes | 7.254 secondes | 4.239 secondes | 6.060 secondes | 5.136 secondes | 
| 2.5 milliard | 17.898 secondes | 19.507 secondes | 11.466 secondes | 16.896 secondes | 13.768 secondes | 
| 5 milliard | 38.454 secondes | 41.116 secondes | 25.325 secondes | 36.843 secondes | 29.889 secondes | 
| 10 milliard | 81.146 secondes | 87.403 secondes | 54.921 secondes | 80.574 secondes | 65.194 secondes | 
| 25 milliard | 217.530 secondes | 238.813 secondes | 156.923 secondes | 229.017 secondes | 186.841 secondes | 
| 50 milliard | 565.913 secondes | 502.245 secondes | N/D | N/D | N/D | 
7-Zip
L'utilitaire populaire 7-Zip dispose d'un test de mémoire intégré qui démontre très bien les performances du processeur. Dans ce test, nous l'exécutons avec une taille de dictionnaire de 128 Mo lorsque cela est possible. Dans ce test, le 6780E devance à peine le Platinum 8592+ dans la note totale. Pour la décompression, le 6766E a également devancé le 8592+.
| Compression à 7 zips | Xeon 6780E (256 Go DDR5) | Xeon 6766E (256 Go DDR5) | 2x Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|---|---|
| Compression |  |  |  |  |  | 
| Utilisation actuelle du processeur | 5,891 % | 4,768 % | 5,609 % | 5,732 % | 5,482 % | 
| Courant nominal/utilisation | 4.985 GIPS | 4.614 GIPS | 4.912 GIPS | 3.912 GIPS | 4.628 GIPS | 
| Courant | 293.689 GIPS | 220.001 GIPS | 275,503 GIPS | 224.209 GIPS | 253.724 GIPS | 
| Utilisation résultante du processeur | 5,603 % | 5,103 % | 5,605 % | 5,669 % | 5,475 % | 
| Évaluation/utilisation résultante | 4.954 GIPS | 4.638 GIPS | 4.883 GIPS | 3.923 GIPS | 4.628 GIPS | 
| Note résultante | 277.670 GIPS | 236.910 GIPS | 273.716 GIPS | 222.407 GIPS | 253.382 GIPS | 
| Décompression |  |  |  |  |  | 
| Utilisation actuelle du processeur | 5,962 GIPS | 5,798 % | 6,243 % | 5,852 % | 6,219 % | 
| Courant nominal/utilisation | 4.550 GIPS | 4.152 GIPS | 3.635 GIPS | 3.423 GIPS | 3.745 GIPS | 
| Courant | 271.266 GIPS | 240.693 GIPS | 226.917 GIPS | 200.350 GIPS | 231.916 GIPS | 
| Utilisation résultante du processeur | 5,832 % | 6,029 % | 6,232 % | 5,894 % | 6,129 % | 
| Évaluation/utilisation résultante | 4.540 GIPS | 4.161 GIPS | 3.654 GIPS | 3.385 GIPS | 3.871 GIPS | 
| Note résultante | 264.764 GIPS | 250.853 GIPS | 227.744 GIPS | 199.363 GIPS | 237.259 GIPS | 
| Note totale |  |  |  |  |  | 
| Utilisation totale du processeur | 5,717 % | 5,566 % | 5,919 % | 5,781 % | 5,802 % | 
| Note totale/utilisation | 4.747 GIPS | 4.399 GIPS | 4.269 GIPS | 3.654 GIPS | 4.249 GIPS | 
| Note totale | 271.217 GIPS | 243.882 GIPS | 250.730 GIPS | 210.363 GIPS | 245.320 GIPS | 
Conclusion
La nouvelle gamme E-core Intel Xeon 6, qui fait partie de la famille Sierra Forest, représente un changement de rythme en matière de cadence de révision. Dans les versions précédentes d'Intel, nous avons généralement vu les processeurs haut de gamme en premier ; cette fois-ci, Intel est en tête avec les SKU efficaces de milieu de gamme.
Cela dit, les processeurs Sierra Forest résistent bien, même par rapport aux modèles évolutifs de 5e génération précédents. Mais d'après Intel, les clients cherchant à acheter du nouveau matériel vont les comparer à des systèmes vieux d'environ 5 ans qui sont en train d'être mis hors service, exécutant quelque chose comme les processeurs Intel Xeon 8280. Avec ces systèmes comme composants, les processeurs E-Core Xeon 6 offrent un argument massif en matière de densité et d’économie d’énergie.
Dans cette revue, nous avons affaire à des processeurs très anciens et à un serveur assez performants pour une visite de Sierra Forest, mais pas tout à fait là où nous le souhaitons pour une revue complète. Entre les bugs du système d'exploitation Windows (non présents dans nos tests Ubuntu) et l'attente des révisions du BIOS, il est clair qu'il y a de la place pour grandir et maximiser les performances de ces CPU. Avec ce petit avant-goût, nous sommes optimistes quant à ce que Sierra Forest peut offrir aujourd'hui et à ce que les processeurs Granite Rapids peuvent offrir plus tard cette année pour les charges de travail performantes. De plus, ne dormons pas sur l'éventuel 288 E-Core 6900 Series au cours des prochains trimestres. Intel va offrir à ses clients plus d'options que jamais en matière de réglage de l'infrastructure pour leurs applications.
Nous attendons bientôt le matériel final d'expédition comprenant les processeurs Sierra Forest en laboratoire. Nous fournirons une analyse approfondie de la consommation d’énergie, des performances de stockage et bien plus encore dans les prochaines revues.