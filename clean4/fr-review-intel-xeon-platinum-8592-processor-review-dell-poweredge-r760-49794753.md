Nous avons déjà testé le Dell PowerEdge R760 et l'avons trouvé, comme toujours chez Dell, extrêmement performant. Suite au récent lancement des processeurs Intel Xeon Scalable de 5e génération , nous avons pu nous procurer un ensemble de processeurs haut de gamme Intel Xeon Platinum 8592+. Nous nous sommes dit : quoi de mieux que de moderniser l'un de nos PowerEdge R760 pour combiner le meilleur des deux mondes et voir si la dernière innovation d'Intel mérite de faire des envieux dans le monde de l'entreprise ?
Spécifications Intel Xeon Platinum 8592+
Nous avons commencé avec la configuration fournie par Dell dans notre R760, qui était composée de deux processeurs Intel 6430 et de 1 To de RAM DDR5 4800, avant d'échanger les processeurs 8592+. Bien que le Xeon 6340 ne soit pas le haut de gamme de la famille Sapphire Rapids, nous allons quand même le comparer au Xeon 8592+. Nous alignons ces deux-là dans cette revue non pas pour les présenter comme des concurrents directs, mais pour montrer le potentiel des plates-formes existantes avec la sortie des nouveaux processeurs Emerald Rapids.
Nous avons apporté les chiffres du Sapphire Rapids Platinum 8480+ ainsi que ceux d'un HP ML350 à titre de comparaison avec le Xeon 8592+ pour obtenir des chiffres plus précis. Le 8480+ du ML350 Ge11 est à une place parmi les meilleurs processeurs de la gamme Sapphire Rapids. Il convient de noter que nous examinons ici uniquement les processeurs, sans comparer les serveurs eux-mêmes les uns aux autres.
|  | Processeur Intel Xeon Platinum 8592+ (R760) | Processeur Intel Xeon Gold 6430 (R760) | Processeur Intel Xeon Platinum 8480+ (ML350 G11) | 
|---|---|---|---|
| Génération | 5e, Rapides d'Émeraude | 4ème, Rapides Saphir | 4ème, Rapides Saphir | 
| Coloré | 64 | 32 | 56 | 
| Threads | 128 | 64 | 112 | 
| Fréquence de base | 1.9 GHz | 2.10 GHz | 2.0 GHz | 
| Turbo Fréquence | 3.9 GHz | 3.4 GHz | 3.8 GHz | 
| Cache | 320 MB | 60 MB | 105 MB | 
| Vitesse Intel UPI | 20 GT / s | 16 GT / s | 16 GT / s | 
| Liens UPI maximum | 4 | 3 | 4 | 
| TDP | 350 W | 270 W | 350 W | 
| Date de lancement | Q4 '23 | Q1 '23 | Q1 '23 | 
| Taille maximale de la mémoire | 4TB |  |  | 
| Types de mémoire | DDR5 5600 1 MT/s (XNUMX DPC) | Jusqu'à DDR5 4400 MT/s (1DPC et 2DPC) | Jusqu'à DDR5 4800 1 MT/s XNUMXDPC Jusqu'à DDR5 4400 2 MT/s XNUMXDPC | 
| Nombre maximum de canaux mémoire | 8 |  |  | 
| Révision PCIe | 5 |  |  | 
| Nombre maximum de voies PCIe | 80 |  |  | 
| Cœurs hautement prioritaires | 20 @ 2.1 GHz | 12 @ 2.2 GHz | 16 @ 2.1 GHz | 
| Cœurs à faible priorité | 44 @ 1.7 GHz | 20 @ 1.8 GHz | 40 @ 1.7 GHz | 
| Taille EPC maximale par défaut pour Intel SGX | 512 GB | 128 GB | 512 GB | 
| Page Produit | Lien | Lien | Lien | 
Performances du processeur Intel Xeon Platinum 8592+
Les spécifications système testées de haut niveau pour chaque processeur sont les suivantes. Chaque système possédait deux des processeurs répertoriés.
Xeon Platine 8592+
- Dell Poweredge R760
- 1 To DDR5 4800 XNUMX MHz
Xeon Gold 6430
- Dell Poweredge R760
- 1 To DDR5 4800 XNUMX MHz
Xeon Platine 8480+
- HP ML350 génération 11
- 256GB DDR5 4800MHz
Mixeur OptiX
Le premier est Blender OptiX, une application de modélisation 3D open source. Ce benchmark a été exécuté à l'aide de l'utilitaire CLI Blender Benchmark. Le score est exprimé en échantillons par minute, le plus élevé étant le meilleur.
Les Xeon 8592+ connaissent un bon départ avec plus du double des performances des Xeon 6430, reflétant les données indiquées sur les fiches techniques. Les Xeon 8480+ se rapprochent cependant de 81 points du Xeon 8592+ en classe.
| Processeur Blender 4.0 | 2 Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|
| Monster | 1115.057 | 540.039 | 943.300 | 
| Brocanteur | 780.408 | 361.066 | 627.662 | 
| Salle de classe | 556.550 | 278.228 | 475.144 | 
Cinebench R23
Cinebench R23 de Maxon est une référence de rendu de processeur qui utilise tous les cœurs et threads de processeur. Nous l'avons exécuté pour des tests multicœurs et monocœurs. Des scores plus élevés sont meilleurs. Dans ce test, nous constatons environ le double des résultats sur les Emerald Rapids Xeon 8592+ par rapport aux Sapphire Rapids 6430. Nous avons constaté de meilleures performances monocœur du Xeon 8480+ que du Xeon 8592+, mais cela n’est pas vraiment surprenant car le Xeon 8480+ avait une vitesse d’horloge de base plus élevée. Le Xeon 8592+ a pris une longueur d'avance dans le test multicœur.
| Cinebench R23 | 2 Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|
| Processeur multicœur | 110,498 | 69,663 | 79,164 | 
| Processeur monocœur | 1,144 | 1,022 | 1,461 | 
| Rapport PM | 96.63x | 68.17x | 54.20x | 
Cinebench 2024
Voici les résultats CPU pour la version 2024 de Cinebench.
Nous avons constaté un modèle de performances similaire entre les trois processeurs et Cinebench R23.
| Cinebench 2024 | 2 Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|
| Processeur multicœur | 6,001 | 3,746 | 4,699 | 
| Processeur monocœur | 68 | 59 | 76 | 
| Rapport PM | 88.48x | 63.22x | 61.44x | 
Geekbench 6
Geekbench 6 est un outil d'évaluation multiplateforme mesurant les performances globales d'un système. Plus le score est élevé, meilleures sont les performances. Geekbench propose un test de performances du GPU, mais sans GPU, seuls les résultats du CPU sont disponibles.
*Nous avons eu des problèmes avec les Emerald Rapids Xeon 8592+ lors de ce test et celui-ci n'a pas abouti. Nous y reviendrons donc lorsque nous pourrons obtenir des chiffres de performances. En attendant, nous n'avons de numéros que pour les Xeon 6430 et Xeon 8480+*.
| Geekbench 6 | 2 Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|
| Benchmark CPU – Single Core | N/D | 1,488 | 1,939 | 
| Benchmark du processeur – Multicœur | N/D | 16,054 | 15,218 | 
Y-Cruncher
Y-cruncher est une application populaire d'analyse comparative et de tests de résistance lancée en 2009. Ce test est multithread et évolutif, calculant Pi et d'autres constantes jusqu'à des milliards de chiffres. Plus vite, c'est mieux dans ce test.
Nous constatons que les résultats suivent le même schéma que les autres tests, le Xeon 6430 étant à l'arrière et le Xeon 8480+ juste derrière les chiffres du Xeon 8592+.
| Y-Cruncher (plus bas est mieux) | 2 Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|
| 1 milliard | 4.239 secondes | 6.060 secondes | 5.136 secondes | 
| 2.5 milliard | 11.466 secondes | 16.896 secondes | 13.768 secondes | 
| 5 milliard | 25.325 secondes | 36.843 secondes | 29.889 secondes | 
| 10 milliard | 54.921 secondes | 80.574 secondes | 65.194 secondes | 
| 25 milliard | 156.923 secondes | 229.017 secondes | 186.841 secondes | 
Compression à 7 zips
L'utilitaire populaire 7-Zip dispose d'un test de mémoire intégré qui démontre très bien les performances du processeur. Dans ce test, nous l'exécutons avec une taille de dictionnaire de 128 Mo lorsque cela est possible. Comme prévu, nous constatons toujours de meilleurs résultats sur les processeurs Xeon 8592+.
|  | 2 Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|
| Compression |  |  |  | 
| Utilisation actuelle du processeur | 5,609 % | 5,732 % | 5,482 % | 
| Courant nominal/utilisation | 4.912 GIPS | 3.912 GIPS | 4.628 GIPS | 
| Courant | 275,503 GIPS | 224.209 GIPS | 253.724 GIPS | 
| Utilisation résultante du processeur | 5,605 % | 5,669 % | 5,475 % | 
| Évaluation/utilisation résultante | 4.883 GIPS | 3.923 GIPS | 4.628 GIPS | 
| Note résultante | 273.716 GIPS | 222.407 GIPS | 253.382 GIPS | 
| Décompression |  |  |  | 
| Utilisation actuelle du processeur | 6,243 % | 5,852 % | 6,219 % | 
| Courant nominal/utilisation | 3.635 GIPS | 3.423 GIPS | 3.745 GIPS | 
| Courant | 226.917 GIPS | 200.350 GIPS | 231.916 GIPS | 
| Utilisation résultante du processeur | 6,232 % | 5,894 % | 6,129 % | 
| Évaluation/utilisation résultante | 3.654 GIPS | 3.385 GIPS | 3.871 GIPS | 
| Note résultante | 227.744 GIPS | 199.363 GIPS | 237.259 GIPS | 
| Note totale |  |  |  | 
| Utilisation totale du processeur | 5,919 % | 5,781 % | 5,802 % | 
| Note totale/utilisation | 4.269 GIPS | 3.654 GIPS | 4.249 GIPS | 
| Note totale | 250.730 GIPS | 210.363 GIPS | 245.320 GIPS | 
Inférence UL Procyon AI
La suite de tests d' inférence IA Procyon d'UL évalue les performances de différents moteurs d'inférence IA utilisant des réseaux neuronaux de pointe. Ces tests ont été effectués uniquement sur le processeur. Chaque valeur représente un temps d'inférence moyen (plus le temps est court, meilleures sont les performances), et la dernière ligne indique un score global (plus le score est élevé, meilleures sont les performances). Les différences entre ces processeurs sont moins marquées dans ce test : le Xeon 8480+ se distingue même dans certains tests, mais au final, le Xeon 8592+ obtient tout de même un score global supérieur.
|  | 2 Xeon Platinum 8592+(ER) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Gold 6430(SR) (R760 – 1 To DDR5 4800 XNUMX MHz) | 2x Xeon Platinum 8480+(SR) (ML350 G11 – 256 Go DDR5 4400 XNUMX MHz) | 
|---|---|---|---|
| Mobile Net V3 | 3.17 | 3.71 | 2.34 | 
| ResNet 50 | 5.23 | 4.37 | 5.76 | 
| Création V4 | 18.66 | 22.59 | 21.70 | 
| Deep Lab V3 | 23.99 | 28.25 | 23.00 | 
| YOLO V3 | 35.47 | 40.28 | 30.81 | 
| RÉEL-ESRGAN | 1021.49 | 1277.08 | 1535.27 | 
| Note globale | 196 | 161 | 191 | 
Processeur Z
Bien qu’il ne s’agisse pas exactement d’une référence, nous avons pensé qu’il valait la peine de mentionner les différences de vitesse de mémoire, le seul changement étant le processeur installé.
Voici d’abord une photo de l’onglet mémoire des Sapphire Rapids Xeon 6430 qui étaient initialement installés dans notre R760.
Voici ensuite une photo de l'onglet mémoire de l'Emerald Rapids Xeon 8592+.
Il existe une nette différence dans la synchronisation de la mémoire sur le Xeon 8592+, ce qui la rend un peu inférieure à celle de notre Xeon 6430, mais probablement pas suffisamment pour avoir un impact sur les performances dans la plupart des applications du monde réel.
Test de mémoire
En analysant les informations mémoire fournies par CPU-Z, nous pouvons exécuter des microbenchmarks depuis Clamchowder . Cet outil est conçu pour évaluer les paramètres liés au processeur et à la mémoire, notamment la taille des fichiers ROB et de registres, la latence de cohérence des verrous et du cache, ainsi que les performances du cache et de la mémoire. Nous avons choisi ce test pour analyser les gains de performance entre les différentes générations de processeurs et pour différentes tailles d'accès.
Pour ce test, nous avons utilisé 64 threads et exécuté un profil complet de 64 Ko à 3,145,728 5 XNUMX Ko. En nous concentrant sur les performances une fois que nous quittons le cache du processeur et accédons à la mémoire système, nous pouvons constater une augmentation significative du débit avec le Xeon de XNUMXe génération dans tous les domaines lors du test d'écriture.
En ce qui concerne la bande passante de lecture, l’histoire n’est pas aussi dramatique, mais remarquable. La différence de vitesse d'horloge entre le processeur de 4e génération et le processeur de 5e génération que nous avons testés devient un peu plus claire ici dans ce test. Malgré le léger inconvénient de la 5e génération, nous pouvons toujours constater que la 5e génération Xeon Scalable peut offrir une baisse de l'augmentation des performances.
Conclusion
Comme prévu, le processeur Intel Xeon Platinum 8592+ a affiché des performances nettement supérieures aux Xeon 6430 dans la plupart des tests, ce qui est logique puisqu'il ne s'agit pas de processeurs concurrents directs. En intégrant le Xeon 8480+, l'écart de performances se réduit considérablement, ce dernier étant un prédécesseur direct du Xeon 8592+. Un avantage majeur des processeurs Xeon de 5e génération réside dans leur compatibilité avec le même socket que la 4e génération, permettant ainsi la mise à niveau de plateformes plus anciennes sur site, comme nous l'avons fait pour certains de nos serveurs.
Nous avons constaté des améliorations significatives des performances sur toutes nos charges de travail entre les deux processeurs Intel Xeon de 4e génération et le Xeon 5+ de 8592e génération. Les plus grandes améliorations ont été visibles dans nos charges de travail informatiques, telles que y-cruncher. Au cours de nos courses à 25 milliards de chiffres, nous avons mesuré des temps sur le Gold 6430 et le Platinum 8480+ de 229 et 187 secondes respectivement, tombant à seulement 157 secondes sur le Platinum 8592+. Les performances ont également été considérablement améliorées dans Cinebench R23 et 2024, passant du Platinum 4+ de 8480e génération au 5+ de 8592e génération. Dans la R23, nous avons vu les performances passer de 79 110 à 2024 4,699 km, et en 6,001, les vitesses sont passées de XNUMX XNUMX à XNUMX XNUMX.
L'Intel Xeon Platinum 8592+ marque un grand pas en avant pour l'entreprise et même s'il ne peut pas aller de cœur à cœur avec les puces robustes d'AMD, le 8592+ possède des atouts uniques qui en font un choix convaincant. Intel a considérablement optimisé ces processeurs pour les charges de travail IA et HPC, offrant des performances robustes à tous les niveaux.