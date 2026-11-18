Dans le torrent incessant d’innovation du monde de l’IA d’aujourd’hui, il est essentiel de mesurer et de comprendre les capacités des diverses plates-formes matérielles. Toutes les IA ne nécessitent pas d'énormes fermes de GPU de formation, il existe un segment important de l'IA d'inférence, qui nécessite souvent moins de puissance GPU, en particulier à la périphérie. Dans cette revue, nous examinons plusieurs GPU NVIDIA L4, sur trois serveurs Dell différents et diverses charges de travail, y compris MLperf, pour voir comment le L4 se compare.
GPU NVIDIA L4
À la base, le L4 offre une impressionnante capacité de 30.3 téraFLOP en performances FP32, idéale pour les tâches informatiques de haute précision. Ses prouesses s'étendent aux calculs de précision mixte avec les cœurs Tensor TF32, FP16 et BFLOAT16, cruciaux pour l'efficacité de l'apprentissage en profondeur, la fiche technique L4 cite des performances comprises entre 60 et 121 téraFLOP.
Dans les tâches de faible précision, le L4 brille avec 242.5 téraFLOP dans les cœurs Tensor FP8 et INT8, améliorant ainsi l'inférence du réseau neuronal. Sa mémoire GDDR24 de 6 Go, complétée par une bande passante de 300 Go/s, le rend capable de gérer de grands ensembles de données et des modèles complexes. L'efficacité énergétique du L4 est ce qui est le plus remarquable ici, avec un TDP de 72 W le rendant adapté à divers environnements informatiques. Ce mélange de hautes performances, d'efficacité de la mémoire et de faible consommation d'énergie fait du NVIDIA L4 un choix incontournable pour les défis informatiques de pointe.
| Spécifications NVIDIA L4 |  | 
|---|---|
| FP 32 | 30.3 téraFLOP | 
| Noyau tenseur TF32 | 60 téraFLOP | 
| Noyau tenseur FP16 | 121 téraFLOP | 
| Noyau tenseur BFLOAT16 | 121 téraFLOP | 
| Noyau tenseur FP8 | 242.5 téraFLOP | 
| Noyau tenseur INT8 | TOP 242.5 | 
| Mémoire GPU | 24GB GDDR6 | 
| Bande passante mémoire GPU | 300GB / s | 
| Puissance thermique maximale (TDP) | 72W | 
| Facteur de forme | PCIe profil bas à 1 emplacement | 
| Interconnect | PCIe Gen4x16 | 
| Tableau des spécifications | L4 | 
Bien sûr, avec le prix du L4 proche de 2500 2 $, l'A4 coûtant environ la moitié du prix et le T1000 vieilli (mais toujours assez performant) disponible pour moins de XNUMX XNUMX $, la question évidente est de savoir quelle est la différence entre ces trois GPU d'inférence.
| Spécifications NVIDIA L4, A2 et T4 | Nvidia L4 | Nvidia A2 | NVIDIA T4 | 
|---|---|---|---|
| FP 32 | 30.3 téraFLOP | 4.5 téraFLOP | 8.1 téraFLOP | 
| Noyau tenseur TF32 | 60 téraFLOP | 9 téraFLOP | N/D | 
| Noyau tenseur FP16 | 121 téraFLOP | 18 téraFLOP | N/D | 
| Noyau tenseur BFLOAT16 | 121 téraFLOP | 18 téraFLOP | N/D | 
| Noyau tenseur FP8 | 242.5 téraFLOP | N/D | N/D | 
| Noyau tenseur INT8 | TOP 242.5 | 36 TOPS | 130 TOPS | 
| Mémoire GPU | 24GB GDDR6 | 16GB GDDR6 | 16GB GDDR6 | 
| Bande passante mémoire GPU | 300GB / s | 200GB / s | 320+ Go/s | 
| Puissance thermique maximale (TDP) | 72W | 40-60W | 70W | 
| Facteur de forme | PCIe profil bas à 1 emplacement |  |  | 
| Interconnect | PCIe Gen4x16 | PCIe Gen4x8 | PCIe Gen3x16 | 
| Tableau des spécifications | L4 | A2 | T4 | 
Une chose à comprendre en regardant ces trois cartes est qu'elles ne sont pas exactement des remplacements générationnels individuels, ce qui explique pourquoi le T4 reste encore, de nombreuses années plus tard, un choix populaire pour certains cas d'utilisation. L'A2 est venu remplacer le T4 en tant qu'option à faible consommation et plus compatible (mécanique x8 vs x16). Techniquement, le L4 remplace alors le T4, l'A2 étant à cheval sur un intermédiaire qui pourrait ou non être actualisé à un moment donné dans le futur.
Performances de l'inférence MLPerf 3.1
MLPerf est un consortium de leaders de l'IA issus du monde universitaire, de la recherche et de l'industrie, créé pour fournir des références matérielles et logicielles d'IA justes et pertinentes. Ces benchmarks sont conçus pour mesurer les performances du matériel, des logiciels et des services d'apprentissage automatique sur diverses tâches et scénarios.
Nos tests se concentrent sur deux benchmarks MLPerf spécifiques : Resnet50 et BERT.
- Resnet50 : Il s'agit d'un réseau neuronal convolutif utilisé principalement pour la classification d'images. C'est un bon indicateur de la capacité d'un système à gérer les tâches d'apprentissage en profondeur liées au traitement d'images.
- BERT (Bidirectionnel Encoder Representations from Transformers) : cette référence se concentre sur les tâches de traitement du langage naturel, offrant un aperçu de la façon dont un système fonctionne dans la compréhension et le traitement du langage humain.
Ces deux tests sont cruciaux pour évaluer les capacités du matériel d’IA dans des scénarios réels impliquant le traitement d’images et de langage.
L'évaluation de NVIDIA L4 avec ces benchmarks est essentielle pour aider à comprendre les capacités du GPU L4 dans des tâches d'IA spécifiques. Il offre également un aperçu de la façon dont différentes configurations (configurations simples, doubles et quadruples) influencent les performances. Ces informations sont vitales pour les professionnels et les organisations qui cherchent à optimiser leur infrastructure d’IA.
Les modèles fonctionnent sous deux modes clés : serveur et hors ligne.
- Mode hors ligne : ce mode mesure les performances d'un système lorsque toutes les données sont disponibles pour un traitement simultané. Cela s'apparente au traitement par lots, dans lequel le système traite un grand ensemble de données en un seul lot. Le mode hors ligne est crucial pour les scénarios dans lesquels la latence n’est pas une préoccupation majeure, mais le débit et l’efficacité le sont.
- Mode serveur : en revanche, le mode serveur évalue les performances du système dans un scénario imitant un environnement de serveur réel, dans lequel les requêtes arrivent une par une. Ce mode est sensible à la latence et mesure la rapidité avec laquelle le système peut répondre à chaque demande. C'est essentiel pour les applications en temps réel, telles que les serveurs Web ou les applications interactives, pour lesquelles une réponse immédiate est nécessaire.
1 x NVIDIA L4 – Dell PowerEdge XR7620
Dans le cadre de notre récent test du Dell PowerEdge XR7620 , équipé d'un seul NVIDIA L4, nous l'avons poussé dans ses retranchements pour exécuter plusieurs tâches, dont MLPerf.
La configuration de notre système de test comprenait les composants suivants :
- 2 Xeon Gold 6426Y – 16 cœurs 2.5 GHz
- 1 x Nvidia L4
- 8 x 16GB DDR5
- BOSS RAID480 de 1 Go
- Ubuntu Server 22.04
- Pilote NVIDIA 535
| Dell PowerEdge XR7620 1x NVIDIA L4 | Score | 
|---|---|
| Resnet50 – Serveur | 12,204.40 | 
| Resnet50 – Hors ligne | 13,010.20 | 
| BERT K99 – Serveur | 898.945 | 
| BERT K99 – Hors ligne | 973.435 | 
Les performances dans les scénarios de serveur et hors ligne pour Resnet50 et BERT K99 sont presque identiques, ce qui indique que le L4 maintient des performances cohérentes sur différents modèles de serveur.
1, 2 et 4 NVIDIA L4 – Dell PowerEdge T560
La configuration de notre unité d'examen comprenait les composants suivants :
- 2 x Intel Xeon Gold 6448Y (32 cœurs/64 threads chacun, TDP 225 watts, 2.1-4.1 GHz)
- 8 disques SSD Solidigm P1.6 de 5520 To avec carte RAID PERC 12
- 1 à 4x GPU NVIDIA L4
- 8 modules RDIMM de 64 Go
- Ubuntu Server 22.04
- Pilote NVIDIA 535
| Dell PowerEdge T560 1x NVIDIA L4 | Score | 
|---|---|
| Resnet50 – Serveur | 12,204.40 | 
| Resnet50 – Hors ligne | 12,872.10 | 
| Bert K99 – Serveur | 898.945 | 
| Bert K99 – Hors ligne | 945.146 | 
Lors de nos tests avec deux L4 dans le Dell T560, nous avons observé cette mise à l’échelle quasi linéaire des performances pour les benchmarks Resnet50 et BERT K99. Cette mise à l'échelle témoigne de l'efficacité des GPU L4 et de leur capacité à fonctionner en tandem sans pertes significatives dues à la surcharge ou à l'inefficacité.
| Dell PowerEdge T560 2x NVIDIA L4 | Score | 
|---|---|
| Resnet50 – Serveur | 24,407.50 | 
| Resnet50 – Hors ligne | 25,463.20 | 
| BERT K99 – Serveur | 1,801.28 | 
| BERT K99 – Hors ligne | 1,904.10 | 
La mise à l'échelle linéaire cohérente à laquelle nous avons assisté avec deux GPU NVIDIA L4 s'étend de manière impressionnante aux configurations comportant quatre unités L4. Cette mise à l'échelle est particulièrement remarquable car le maintien des gains de performances linéaires devient de plus en plus difficile avec chaque GPU ajouté en raison de la complexité du traitement parallèle et de la gestion des ressources.
| Dell PowerEdge T560 4x NVIDIA L4 | Score | 
|---|---|
| Resnet50 – Serveur | 48,818.30 | 
| Resnet50 – Hors ligne | 51,381.70 | 
| BERT K99 – Serveur | 3,604.96 | 
| BERT K99 – Hors ligne | 3,821.46 | 
Ces résultats sont fournis à titre indicatif uniquement et ne constituent pas des résultats officiels ou compétitifs de MLPerf. Pour consulter la liste complète des résultats officiels, veuillez vous rendre sur la page des résultats MLPerf.
En plus de valider l'évolutivité linéaire des GPU NVIDIA L4, nos tests en laboratoire mettent en lumière les implications pratiques du déploiement de ces unités dans différents scénarios opérationnels. Par exemple, la cohérence des performances entre les modes serveur et hors ligne dans toutes les configurations avec les GPU L4 révèle leur fiabilité et leur polyvalence.
Cet aspect est particulièrement pertinent pour les entreprises et les institutions de recherche où les contextes opérationnels varient considérablement. De plus, nos observations sur l’impact minimal des goulots d’étranglement d’interconnexion et l’efficacité de la synchronisation GPU dans les configurations multi-GPU fournissent des informations précieuses pour ceux qui cherchent à faire évoluer leur infrastructure d’IA. Ces informations vont au-delà de simples chiffres de référence, offrant une compréhension plus approfondie de la manière dont un tel matériel peut être utilisé de manière optimale dans des scénarios du monde réel, guidant de meilleures décisions architecturales et stratégies d'investissement dans l'infrastructure d'IA et HPC.
NVIDIA L4 – Performances des applications
Nous avons comparé les performances du nouveau NVIDIA L4 à celles des NVIDIA A2 et NVIDIA T4 qui l'ont précédé. Pour présenter cette amélioration des performances par rapport aux modèles précédents, nous avons déployé les trois modèles sur un serveur de notre laboratoire, avec Windows Server 2022 et les derniers pilotes NVIDIA, en exploitant l'ensemble de notre suite de tests GPU.
Ces cartes ont été testées sur un Dell Poweredge R760 avec la configuration suivante :
- 2 x Intel Xeon Gold 6430 (32 cœurs, 2.1 GHz)
- Windows Server 2022
- Pilote NVIDIA 538.15
- ECC désactivé sur toutes les cartes pour un échantillonnage 1x
Alors que nous lançons les tests de performances entre ce groupe de trois GPU d'entreprise, il est important de noter les différences de performances uniques entre les modèles A2 et T4 précédents. Lorsque l'A2 est sorti, il offrait des améliorations notables telles qu'une consommation d'énergie inférieure et un fonctionnement sur un emplacement PCIe Gen4 x8 plus petit, au lieu du plus grand emplacement PCIe Gen3 x16 requis par l'ancien T4. Dès le départ, cela lui a permis de s'intégrer dans davantage de systèmes, en particulier avec le plus petit encombrement nécessaire.
Mélangeur OptiX 4.0
Blender OptiX est une application de modélisation 3D open source. Ce test peut être exécuté à la fois pour le CPU et le GPU, mais nous n'avons effectué que le GPU comme la plupart des autres tests ici. Ce benchmark a été exécuté à l'aide de l'utilitaire CLI Blender Benchmark. Le score est exprimé en échantillons par minute, le plus élevé étant le meilleur.
| Mixeur 4.0 (Plus haut, c'est mieux) | Nvidia L4 | Nvidia A2 | Nvidia T4 | 
|---|---|---|---|
| GPU Blender CLI – Monstre | 2,207.765 | 458.692 | 850.076 | 
| GPU Blender CLI – Junkshop | 1,127.829 | 292.553 | 517.243 | 
| GPU Blender CLI – Salle de classe | 1,111.753 | 262.387 | 478.786 | 
Test de vitesse Blackmagic RAW
Nous testons les CPU et les GPU avec le RAW Speed Test de Blackmagic qui teste les vitesses de lecture vidéo. Il s’agit plutôt d’un test hybride incluant les performances du CPU et du GPU pour le décodage RAW réel. Ceux-ci sont affichés sous forme de résultats séparés, mais nous nous concentrons ici uniquement sur les GPU, donc les résultats du CPU sont omis.
| Test de vitesse Blackmagic RAW (Plus haut, c'est mieux) | Nvidia L4 | Nvidia A2 | NVIDIA T4 | 
|---|---|---|---|
| CUDA 8K | FPS 95 | FPS 38 | FPS 53 | 
GPU Cinebench 2024
Cinebench 2024 de Maxon est une référence de rendu CPU et GPU qui utilise tous les cœurs et threads du processeur. Encore une fois, puisque nous nous concentrons sur les résultats du GPU, nous n’avons pas exécuté les parties CPU du test. Des scores plus élevés sont meilleurs.
| Cinebench 2024 (Plus haut, c'est mieux) | Nvidia L4 | Nvidia A2 | NVIDIA T4 | 
|---|---|---|---|
| GPU | 15,263 | 4,006 | 5,644 | 
GPU PI
GPUPI 3.3.3 est une version de l'utilitaire d'analyse comparative léger conçu pour calculer π (pi) en milliards de décimales à l'aide de l'accélération matérielle via les GPU et les CPU. Il exploite la puissance de calcul d'OpenCL et de CUDA, qui comprend des unités de traitement centrales et graphiques. Nous avons exécuté CUDA uniquement sur les 3 GPU et les chiffres ici sont le temps de calcul sans temps de réduction ajouté. Plus bas, c'est mieux.
| Temps de calcul du GPU PI en secondes (Plus bas, c'est mieux) | Nvidia L4 | Nvidia A2 | NVIDIA T4 | 
|---|---|---|---|
| GPUPI v3.3 – 1B | 3.732s | 19.799s | 7.504s | 
| GPUPI v3.3 – 32B | 244.380s | 1,210.801s | 486.231s | 
Alors que les résultats précédents ne portaient que sur une seule itération de chaque carte, nous avons également eu l'occasion d'examiner un déploiement de 5 cartes NVIDIA L4 à l'intérieur du Dell PowerEdge T560.
| Temps de calcul du GPU PI en secondes (Plus bas, c'est mieux) | Dell PowerEdge T560 (2x Xeon Gold 6448Y) avec 5x NVIDIA L4 | 
|---|---|
| GPUPI v3.3 – 1B | 0 s 850 ms | 
| GPUPI v3.3 – 32B | 50 s 361 ms | 
Banc Octane
OctaneBench est un utilitaire d'analyse comparative pour OctaneRender, un autre moteur de rendu 3D avec prise en charge RTX similaire à V-Ray.
| Octane (plus haut est mieux) |  |  |  |  | 
| Scène | Noyau | Nvidia L4 | Nvidia A2 | NVIDIA T4 | 
| Intérieur | Canaux d'information | 15.59 | 4.49 | 6.39 | 
|  | Eclairage direct | 50.85 | 14.32 | 21.76 | 
|  | Suivi de chemin | 64.02 | 18.46 | 25.76 | 
| L'idée | Canaux d'information | 9.30 | 2.77 | 3.93 | 
|  | Eclairage direct | 39.34 | 11.53 | 16.79 | 
|  | Suivi de chemin | 48.24 | 14.21 | 20.32 | 
| VTT | Canaux d'information | 24.38 | 6.83 | 9.50 | 
|  | Eclairage direct | 54.86 | 16.05 | 21.98 | 
|  | Suivi de chemin | 68.98 | 20.06 | 27.50 | 
| Skybox | Canaux d'information | 12.89 | 3.88 | 5.42 | 
|  | Eclairage direct | 48.80 | 14.59 | 21.36 | 
|  | Suivi de chemin | 54.56 | 16.51 | 23.85 | 
| Score total |  | 491.83 | 143.71 | 204.56 | 
Carte graphique Geekbench 6
Geekbench 6 est un outil d'évaluation multiplateforme qui mesure les performances globales d'un système. Il propose des tests pour le processeur et la carte graphique. Plus le score est élevé, meilleures sont les performances. Nous nous sommes concentrés ici sur les résultats de la carte graphique.
Vous pouvez trouver des comparaisons avec n'importe quel système dans le navigateur Geekbench.
| Geekbench 6.1.0 (Plus c'est mieux) | Nvidia L4 | Nvidia A2 | NVIDIA T4 | 
|---|---|---|---|
| GeekbenchGPU OpenCL | 156,224 | 35,835 | 83,046 | 
marque de luxe
LuxMark est un outil d'analyse comparative multiplateforme OpenCL conçu par ceux qui maintiennent le moteur de rendu 3D open source LuxRender. Cet outil examine les performances du GPU dans la modélisation 3D, l'éclairage et le travail vidéo. Pour cette revue, nous avons utilisé la version la plus récente, v4alpha0. Dans LuxMark, plus le score est élevé, mieux c'est.
| Luxmark v4.0alpha0 GPU OpenCL (Plus haut, c'est mieux) | Nvidia L4 | Nvidia A2 | NVIDIA T4 | 
|---|---|---|---|
| Banc d'entrée | 14,328 | 3,759 | 5,893 | 
| Banc de nourriture | 5,330 | 1,258 | 2,033 | 
GROMACS CUDA
Nous nous approvisionnons également en GROMACS, un logiciel de dynamique moléculaire, spécifiquement pour CUDA. Cette compilation sur mesure devait exploiter les capacités de traitement parallèle des 5 GPU NVIDIA L4, essentielles pour accélérer les simulations informatiques.
Le processus impliquait l'utilisation de nvcc, le compilateur CUDA de NVIDIA, ainsi que de nombreuses itérations des indicateurs d'optimisation appropriés pour garantir que les binaires étaient correctement adaptés à l'architecture du serveur. L'inclusion du support CUDA dans la compilation GROMACS permet au logiciel de s'interfacer directement avec le matériel GPU, ce qui peut considérablement améliorer les temps de calcul pour les simulations complexes.
Le test : interaction protéique personnalisée dans Gromacs
En tirant parti d'un fichier d'entrée fourni par la communauté à partir de notre divers Discord, qui contenait des paramètres et des structures adaptés à une étude d'interaction protéique spécifique, nous avons lancé une simulation de dynamique moléculaire. Les résultats ont été remarquables : le système a atteint un taux de simulation de 170.268 nanosecondes par jour.
| GPU | Système | ns/jour | temps de base (s) | 
|---|---|---|---|
| Nvidia A4000 | Boîte blanche AMD Ryzen 5950x | 84.415 | 163,763 | 
| RTX NVIDIA 4070 | Boîte blanche AMD Ryzen 7950x3d | 131.85 | 209,692.3 | 
| 5x Nvidia L4 | Dell T560 avec 2x Intel Xeon Gold 6448Y | 170.268 | 608,912.7 | 
Plus que l'IA
Avec le battage médiatique de l'IA qui fait fureur, il est facile de se laisser prendre aux performances des modèles sur NVIDIA L4, mais il a également quelques autres atouts dans son sac, ouvrant un champ de possibilités pour les applications vidéo. Il peut héberger jusqu'à 1,040 1 flux vidéo AV720 simultanés à 30pXNUMX. Cela peut transformer la façon dont le contenu peut être diffusé en direct pour les utilisateurs périphériques, améliorer la narration créative et présenter des utilisations intéressantes pour des expériences AR/VR immersives.
Le NVIDIA L4 excelle également dans l’optimisation des performances graphiques, comme en témoignent ses capacités de rendu en temps réel et de lancer de rayons. Dans un bureau périphérique, le L4 est capable de fournir une accélération de calcul graphique robuste et puissante en VDI aux utilisateurs finaux qui en ont le plus besoin lorsqu'un rendu graphique de haute qualité en temps réel est essentiel.
Réflexions de clôture
Le GPU NVIDIA L4 fournit une plate-forme solide pour l'IA de pointe et le calcul haute performance, offrant une efficacité et une polyvalence inégalées sur plusieurs applications. Sa capacité à gérer des pipelines intensifs d’IA, d’accélération ou de vidéo et à optimiser les performances graphiques en fait un choix idéal pour l’inférence de périphérie ou l’accélération des bureaux virtuels. La combinaison du L4 entre une puissance de calcul élevée, des capacités de mémoire avancées et une efficacité énergétique le positionne comme un acteur clé dans l'accélération des charges de travail à la périphérie, en particulier dans les secteurs de l'IA et des graphiques à forte intensité.
Il ne fait aucun doute que l’IA est l’œil de l’ouragan informatique ces jours-ci, et la demande pour les GPU monstrueux H100/H200 continue d’exploser. Mais il y a également un effort majeur pour mettre en place un ensemble de kits informatiques plus robustes vers la périphérie, où les données sont créées et analysées. Dans ces cas-là, un GPU plus approprié est nécessaire. Ici, le NVIDIA L4 excelle et devrait être l'option par défaut pour l'inférence de bord, soit en tant qu'unité unique, soit à l'échelle globale, comme nous l'avons testé dans le T560.