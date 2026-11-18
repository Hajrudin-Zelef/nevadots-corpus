Les serveurs Dell PowerEdge série R7x0 sont depuis longtemps un pilier des centres de données, réputés pour leur qualité de fabrication exceptionnelle, leur conception soignée, leurs performances, leur densité et leur fiabilité, le tout dans un format 2U polyvalent. Ces serveurs ont constamment évolué pour répondre aux exigences changeantes. Aujourd'hui, avec l'introduction du Dell PowerEdge R770, la série franchit une étape décisive.
Le R770 inaugure la nouvelle famille de processeurs Intel Xeon 6, avec les processeurs à cœurs Xeon 6500 et 6700 (architectures P et E). Il marque la première adoption complète par Dell de la norme OCP Data Center Modular Hardware System (DC MHS) dans sa gamme de serveurs grand public. Ensemble, ces deux évolutions promettent une avancée significative en termes de performances et de conception.
Répondre aux exigences des centres de données modernes
Le lancement du R770 intervient alors que les centres de données sont confrontés à une pression croissante. Les charges de travail sont de plus en plus diversifiées et exigeantes. La croissance incessante des données renforce le besoin d'analyses et de bases de données robustes. De l'entraînement de modèles complexes au déploiement d'inférences en temps réel, l'intelligence artificielle n'est plus une application de niche, mais un moteur métier essentiel nécessitant une puissance de calcul importante et une accélération spécialisée.
Parallèlement, l'efficacité énergétique et l'optimisation du coût total de possession font l'objet d'une attention particulière. De plus, l'industrie se tourne de plus en plus vers les normes ouvertes pour favoriser l'innovation, améliorer l'interopérabilité et potentiellement réduire la dépendance vis-à-vis d'un fournisseur. Le R770, avec ses nouvelles options de processeur et l'adoption de l'OCP DC MHS, est conçu pour relever ces défis.
Processeurs Intel Xeon 6 P-Core
Le processeur R770 utilise les processeurs Intel Xeon série 6, notamment les séries 6700 et 6500, intégrant les cœurs Performance et Efficiency basés sur la plateforme Socket E2 (LGA4710-2). Dans cette analyse, nous nous concentrons spécifiquement sur les références de la série P.
Intel construit ces processeurs selon une conception en tuiles, combinant des tuiles d'E/S avec une ou deux tuiles de calcul. Cela permet une évolutivité au sein de la série, avec des configurations allant jusqu'à 86 cœurs P (XCC) avec deux tuiles de calcul, et jusqu'à 48 cœurs P (HCC) ou 16 cœurs P (LCC) avec une seule tuile de calcul.
Comparés aux processeurs Sapphire et Emerald Rapids de génération précédente, ces processeurs se distinguent par la disponibilité universelle d'accélérateurs intégrés sur tous les processeurs Xeon 6. Parmi ceux-ci figurent la technologie Intel QuickAssist pour le chiffrement et la compression, l'accélérateur de streaming de données Intel pour le transfert de données, l'accélérateur d'analyse en mémoire Intel pour l'accélération des bases de données et de l'analyse, et l'équilibreur de charge dynamique Intel pour l'efficacité du traitement réseau.
La mémoire et la bande passante d'E/S bénéficient également d'améliorations substantielles. Les processeurs Xeon 6700/6500 P-core prennent en charge la mémoire DDR8 à 5 canaux. Ils ouvrent également la voie aux modules MRDIMM (Multiplexed Rank DIMM), qui offrent des vitesses allant jusqu'à 8,800 5.0 MT/s. Côté E/S, ces processeurs prennent en charge les normes PCIe 2.0 et CXL 88. En configuration double socket, la plateforme peut offrir jusqu'à 176 voies PCIe par socket (soit XNUMX voies au total).
Malgré la différenciation entre les processeurs P-core et E-core, la famille Xeon 6 conserve une cohérence dans les jeux d'instructions, le BIOS, les pilotes, la prise en charge des systèmes d'exploitation et des applications, ainsi que les fonctionnalités RAS, simplifiant ainsi l'intégration et la gestion entre différents types de déploiement. Les processeurs P-core sont destinés aux charges de travail où les performances par cœur, l'accélération de l'IA, une bande passante mémoire élevée et des E/S importantes sont primordiales ; pensez aux bases de données exigeantes, aux simulations HPC, à l'analyse avancée et à un large éventail d'applications d'IA.
Spécifications Dell PowerEdge R770
| Spécifications | Dell PowerEdge R770 | 
| Processeur | Deux processeurs Intel Xeon 6 avec jusqu'à 144 cœurs E ou 86 cœurs P par processeur | 
| Mémoire | 32 emplacements DIMM DDR5, prend en charge RDIMM 8 To max, vitesses jusqu'à 6400 5 MT/s, prend en charge uniquement les DIMM DDRXNUMX ECC enregistrés | 
| Contrôleurs de stockage | Démarrage interne : Sous-système de stockage optimisé pour le démarrage (BOSS-N1 DC-MHS) : HWRAID 1, 2 x SSD M.2 NVMe ou carte intercalaire M.2 (DC-MHS) : 2 x SSD M.2 NVMe ou USB, Contrôleurs internes : PERC H965i avant, PERC H975i avant, PERC H365i avant | 
| Baies avant et arrière |  | 
| Blocs d'alimentation remplaçables à chaud |  | 
| Options de refroidissement | Refroidissement par air et refroidissement liquide direct (DLC est une solution de rack et nécessite des collecteurs de rack et une unité de distribution de refroidissement (CDU) pour fonctionner) | 
| Ventilateurs | Ventilateurs Silver hautes performances (HPR SLVR)/Ventilateurs Gold hautes performances (HPR GOLD), jusqu'à 6 ventilateurs remplaçables à chaud | 
| Dimensions et poids | Hauteur – 86.8 mm (3.42 pouces), largeur – 482 mm (18.97 pouces), poids – 28.53 kg (62.89 livres), profondeur (pour la configuration E/S arrière) – 802.40 mm (31.59 pouces) avec cadre, 801.51 mm (31.56 pouces) sans cadre, profondeur (pour la configuration E/S avant) – 814.52 mm (32.07 pouces) sans cadre | 
| Facteur de forme | Serveur rack 2U | 
| Gestion intégrée | iDRAC, iDRAC Direct, API RESTful iDRAC avec Redfish, CLI RACADM, module de service iDRAC (iSM), point de terminaison NativeEdge, orchestrateur NativeEdge | 
| Biseau | Lunette de sécurité en option | 
| Sécurité | Micrologiciel signé cryptographiquement, chiffrement des données au repos (SED avec gestion des clés locale ou externe), démarrage sécurisé, vérification des composants sécurisés (contrôle de l'intégrité du matériel), racine de confiance en silicium, verrouillage du système, verrouillage du système (nécessite iDRAC10 Enterprise ou Datacenter), détection d'intrusion dans le châssis, TPM 2.0 FIPS, certifié CC-TCG | 
| Options réseau |  | 
| Options GPU | Jusqu'à 6 x 75 W FHHL ou jusqu'à 2 x 350 W DWFL | 
| Ports | Ports avant : 1 port USB 2.0 Type C, 1 port USB 2.0 Type A (en option), 1 port Mini-DisplayPort (en option), 1 port série DB9 (avec configuration E/S avant), 1 port Ethernet dédié à la gestion iDRAC ; Ports arrière : 1 port Ethernet dédié à la gestion iDRAC, 1 port VGA, 2 ports USB 3.1 Type A ; Ports internes : 1 port USB 3.1 Type A | 
| PCIe |  | 
| Systèmes d'exploitation et hyperviseurs | Serveur Ubuntu LTS canonique, serveur Microsoft Windows avec Hyper-V, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, VMware avec vSphere | 
Dell PowerEdge R770 : la modularité avec OCP DC MHS
Le Dell PowerEdge R770 présente des avancées notables et une flexibilité dans sa conception physique et son architecture de composants, en adoptant la norme Data Center Modular Hardware System (OCP DC MHS) du projet Open Compute.
S'inscrivant dans la lignée de la série R7x0, le R770 offre de nombreuses options de configuration pour répondre à divers besoins de déploiement. Une première significative pour cette gamme est le choix entre une configuration d'E/S arrière traditionnelle et une configuration d'E/S avant accessible en couloir froid, offrant une plus grande flexibilité pour s'adapter aux différentes configurations de centres de données et aux exigences de maintenance. Les options de stockage sont tout aussi polyvalentes, allant des nœuds de calcul avec un stockage local minimal, voire nul, aux configurations haute densité prenant en charge jusqu'à 40 disques E3.S pour les charges de travail centrées sur le stockage.
Pour répondre aux besoins croissants en calcul accéléré, notamment pour l'IA et le HPC, le R770 offre de solides capacités d'extension. Selon la configuration du châssis et de la carte d'extension, le serveur peut accueillir jusqu'à six cartes PCIe Gen 5 x16 pleine hauteur et pleine longueur (FHFL). De plus, il prend en charge l'installation de deux GPU double largeur, ce qui en fait une plateforme performante pour un large éventail de tâches. La flexibilité réseau est assurée par des emplacements mezzanine OCP 3.0, prenant en charge des cartes x8 ou x16 selon la configuration.
Dell a également apporté plusieurs améliorations de conception visant à améliorer la facilité d'entretien et la fiabilité. L'évolution de la carte Boot Optimized Storage Solution (BOSS) en est un parfait exemple. Auparavant relié par câbles et intégré à la carte PCIe, le contrôleur BOSS du R770 est désormais une carte standardisée OCP qui s'interface directement avec la carte mère, éliminant ainsi la complexité du câblage. Ce nouveau contrôleur BOSS intègre également des disques NVMe M.2 plus rapides et des dissipateurs thermiques pour garantir des températures de fonctionnement et des performances optimales pour les périphériques de démarrage. Autre amélioration subtile, mais pratique pour les techniciens : le remplacement des cavaliers traditionnels par des commutateurs DIP plus conviviaux pour des fonctions telles que l'effacement de la NVRAM.
Le changement architectural le plus profond est l'adoption complète de la norme OCP DC MHS. Dell avait déjà intégré des éléments OCP dans les générations précédentes, notamment en adoptant des emplacements pour cartes réseau OCP 3.0. Le R770 va encore plus loin. Les composants clés sont désormais conformes aux spécifications OCP, notamment les modules processeurs hôtes (HPM), communément appelés carte mère, qui incluent des composants tels que les emplacements pour cartes graphiques, désormais des connecteurs M-XIO. Le connecteur M-XIO offre une interface standardisée pour les cartes riser, améliorant ainsi la flexibilité et l'évolutivité. L'iDRAC est également implémenté comme OCP DC-SCM (Server Control Module).
De plus, le R770 intègre le nouveau connecteur d'alimentation PICPWR pour les connexions de périphériques tels que les GPU et les fonds de panier. Ce connecteur constitue une avancée significative, simplifiant l'alimentation et intégrant la surveillance de l'alimentation en ligne.
		
Cette intégration poussée standardise les interfaces et les formats des différents sous-systèmes. Si Dell insiste sur l'importance de privilégier des composants validés pour garantir la compatibilité et le support, la standardisation sous-jacente rend de nombreux composants intrinsèquement plus faciles à entretenir et potentiellement interchangeables entre les systèmes compatibles à l'avenir.
Gestion et iDRAC
Le serveur Dell PowerEdge R770 s'appuie sur l'iDRAC 9, déjà riche en fonctionnalités et très apprécié, avec l'iDRAC 10 de nouvelle génération. Ce dernier améliore l'administration système grâce à une intégration transparente avec le module de contrôle sécurisé du centre de données (DC-SCM). Cette intégration permet de simplifier les mises à jour du firmware et la gestion de la configuration, garantissant ainsi des opérations cohérentes et évolutives dans l'ensemble des centres de données. L'iDRAC 10 prend également en charge des fonctionnalités avancées d'automatisation et de surveillance, permettant aux administrateurs informatiques de gérer efficacement les déploiements à grande échelle sans compromettre les performances ni la fiabilité.
La sécurité est un pilier des fonctionnalités de gestion du R770, Dell ayant mis en œuvre des mécanismes robustes de vérification avant et après le démarrage. S'appuyant sur la technologie Root of Trust au niveau du silicium, iDRAC 10 garantit la vérification cryptographique de l'ensemble du firmware, y compris le BIOS et iDRAC, avant son exécution. Cette mesure de sécurité matérielle immuable protège contre les altérations par logiciels malveillants et les attaques ciblant la chaîne d'approvisionnement, offrant ainsi une base sécurisée pour le fonctionnement du système. De plus, le R770 intègre des protocoles de démarrage résistants à l'informatique quantique afin d'atténuer les nouvelles menaces cryptographiques, renforçant ainsi son rôle dans la protection des infrastructures critiques.
L'engagement de Dell en matière de sécurité de la chaîne d'approvisionnement se reflète dans la conception du R770, qui s'appuie sur un processus complet d'authentification par chaîne de confiance. Chaque composant matériel est soumis à une vérification rigoureuse grâce à des signatures cryptographiques intégrées lors de la fabrication. Ce processus garantit que seuls les micrologiciels et composants autorisés sont utilisés, réduisant ainsi les risques liés aux modifications non autorisées ou aux contrefaçons.
Éléments constitutifs des usines d'IA
Le R770 est disponible avec de nombreuses configurations de GPU et de châssis, ce qui en fait une plateforme polyvalente pour un large éventail de charges de travail d'IA. Sa flexibilité, combinée à ses capacités de stockage et de mise en réseau robustes, en fait un choix attractif pour les entreprises déployant des solutions d'IA dans des AI Factory. Les AI Factory désignent l'infrastructure et les outils nécessaires à la création, à l'entraînement et au déploiement de modèles d'IA à grande échelle. Ces usines sont essentielles au développement de systèmes avancés tels que les véhicules autonomes et la robotique, car elles fournissent la puissance de calcul et les pipelines de données nécessaires au traitement efficace d'énormes volumes de données.
Le développement de véhicules autonomes et de systèmes robotiques nécessite des données d'entraînement exhaustives, reflétant des scénarios réels. Cosmos NIM de NVIDIA constitue une avancée majeure dans ce domaine, offrant aux développeurs une boîte à outils puissante pour accélérer la création et le déploiement de systèmes d'IA physiques tels que les modèles fondamentaux mondiaux.
Comprendre les modèles de fondations mondiales
Les modèles de fondation du monde (WFM) sont des réseaux neuronaux sophistiqués qui simulent des environnements réels et prédisent des résultats précis à partir de diverses données d'entrée. Contrairement aux modèles d'IA traditionnels axés sur des tâches spécifiques, les WFM comprennent la dynamique du monde physique, notamment les lois de la physique et les propriétés spatiales . Ils peuvent générer des vidéos à partir d'instructions textuelles, d'images ou d'autres données d'entrée, tout en représentant fidèlement le mouvement, les forces et les relations spatiales.
NVIDIA Cosmos NIM : un tremplin vers les modèles fondamentaux mondiaux
Les modules NIM Cosmos de NVIDIA constituent une étape cruciale vers la réalisation de modèles fondamentaux du monde. Ils permettent aux organisations et aux laboratoires d'IA de générer des données d'entraînement synthétiques, en adaptant efficacement la quantité de données nécessaires à l'entraînement de ces modèles d'IA. Nous avons déployé le modèle Cosmos Predict , un modèle généraliste qui génère des états du monde à partir d'instructions textuelles ou vidéo et synthétise un mouvement continu en prédisant les images clés.
Voici quelques résultats intéressants que nous avons obtenus avec Cosmos à partir d'une seule image de notre laboratoire. Bien que non parfaits, les résultats obtenus à partir d'une seule image sont très impressionnants.
La capacité du R770 à prendre en charge des GPU hautes performances, tels que le NVIDIA H100, ainsi que ses capacités de stockage et de mise en réseau robustes en font un choix attrayant pour les organisations cherchant à déployer des solutions d'IA.
En exploitant les capacités du R770, les entreprises peuvent former et déployer efficacement des modèles d'IA comme Cosmos NIM, accélérant ainsi le développement de véhicules autonomes et de systèmes robotiques. Ses performances et son évolutivité en font une plateforme idéale pour gérer les grandes quantités de données nécessaires à la formation des modèles d'IA, et sa polyvalence lui permet de prendre en charge un large éventail de charges de travail d'IA.
Stockage direct du GPU
Le stockage direct GPU est une technologie qui permet le transfert direct de données entre les périphériques de stockage et les GPU, en contournant le processeur et la mémoire système. Dans un transfert de données traditionnel, les données sont lues depuis le stockage vers la mémoire du processeur, puis copiées vers celle du GPU. Ce processus implique de multiples copies de données, ce qui entraîne une latence accrue et une baisse des performances. Le processeur agit comme un goulot d'étranglement, devant gérer le transfert de données entre le stockage et le GPU. GDS élimine ce goulot d'étranglement en permettant directement aux périphériques de stockage de transférer les données vers et depuis la mémoire du GPU.
Nous avons effectué une analyse de la charge de travail GDSIO sur un système de stockage composé de 16 disques, en augmentant progressivement le nombre de disques utilisés pour comprendre les performances de stockage et sa capacité à saturer un GPU PCIe Gen 5.
Le graphique de lecture GDSIO illustre l'impact de l'augmentation du nombre de SSD KIOXIA CD8P sur le débit de lecture global et moyen du r770. Initialement, lorsque le nombre de disques passe de un à quatre, le débit de lecture global augmente rapidement, atteignant environ 50.2 Gio/s. Cela suggère que le système peut saturer le PCIe Gen 5 x16 avec seulement trois ou quatre disques pour le chargement des données. Au-delà de cinq disques, le débit global stagne, indiquant que l'ajout de disques n'améliore pas significativement. Parallèlement, le débit de lecture moyen par disque reste stable jusqu'à quatre disques, puis diminue à mesure que d'autres disques sont ajoutés. Cette baisse des performances par disque s'explique par le fait que davantage de disques partagent la bande passante disponible du bus PCIe, ce qui réduit les lectures individuelles.
En revanche, les performances d'écriture de ces disques sont bien inférieures à leurs performances de lecture. Il a fallu que les 16 disques atteignent une bande passante d'écriture de 46.7 Gio/s, la vitesse d'écriture moyenne restant quasiment constante. Compte tenu des capacités d'écriture inférieures de la gamme KIOXIA CD8, les versions haute capacité ou les autres SSD PCIe Gen5 s'en sortiront mieux.
Analyse comparative du Dell PowerEdge R770
Concernant les benchmarks, le R770 est le système phare de Dell et, à ce titre, sera déployé dans des environnements très variés. Nous avons donc réalisé une série complète de benchmarks pour cette plateforme afin d'évaluer ses performances dans différents environnements. Le Lenovo ThinkSystem SR630 V4 a été comparé lors de certains tests afin de mettre en évidence la différence entre les processeurs haut de gamme à cœurs multiples et à cœurs multiples.
Configuration du système
- CPU: 2x Intel Xeon 6787P (86 cœurs chacun)
- RAM: Mémoire DDR32 double rang 64x Micron 5 Go 6400 MT/s Mémoire totale : 2 To
- Alimentations: 2x Delta 1500W
- GPU: 1x NVIDIA H100 pour le benchmark TGI, 1x NVIDIA L4 pour les tests restants
- FIL: Carte réseau DELL BRCM 4P 25G SFP 57504S OCP
- Carte BOSS : Disques BOSS-N1 DC-MHS 0 et 1 SK hynix 480 Go Dell NVMe ISE PE9010 RI M.2 480 Go
- Disques: 0-5 dans le fond de panier 1 : Samsung 6.4 To, Dell NVMe PM1745 MU E3.S 6.4 To
Performances de la charge de travail de l'IA
Benchmark d'inférence de génération de texte
Text Generation Inference (TGI) est un serveur d'inférence LLM hautes performances développé par Hugging Face. Conçu pour optimiser le déploiement et l'utilisation des LLM, il constitue un choix idéal pour les environnements de production. TGI prend en charge divers LLM open source et offre des fonctionnalités telles que le parallélisme tensoriel, le streaming de jetons et le traitement par lots continu, qui améliorent ses performances et son efficacité.
La fonction d'analyse comparative de TGI permet d'évaluer ses performances sous différentes configurations et charges de travail. Elle offre une représentation plus précise des performances réelles, car elle prend en compte la complexité de la gestion des LLM en environnement de production.
La génération de texte à l'aide de LLM comprend deux étapes principales : le préremplissage et le décodage. Le préremplissage est l'étape initiale, où le LLM traite l'invite de saisie pour générer les représentations intermédiaires nécessaires. Cette étape est gourmande en ressources de calcul, car elle implique le traitement de l'intégralité de l'invite de saisie en un seul passage dans le modèle.
Lors de l'étape de préremplissage, l'invite de saisie est tokenisée et convertie dans un format exploitable par le LLM. Ce dernier calcule ensuite le cache KV, qui stocke les informations relatives aux jetons d'entrée. Ce cache KV est une structure de données essentielle qui facilite la génération de jetons de sortie.
En revanche, l'étape de décodage est un processus autorégressif où le LLM génère les jetons de sortie un par un, en s'appuyant sur les représentations intermédiaires générées lors de l'étape de préremplissage. L'étape de décodage s'appuie fortement sur le cache KV généré lors de l'étape de préremplissage, qui fournit le contexte nécessaire à la génération de jetons de sortie cohérents et contextuellement pertinents.
Étape de pré-remplissage
À mesure que la taille du lot augmente de 1 à 32, la latence des trois modèles augmente ; la latence de DeepSeek-R1-Distill-Qwen-32 B augmente de 29.97 ms pour une taille de lot de 1 à 76.95 ms pour une taille de lot de 32. De même, la latence de GEMMA-3-27B-IT et Qwen/QwQ-32B augmente de 51.84 ms et 29.90 ms à 79.58 ms et 76.30 ms, respectivement.
En revanche, le débit de jetons s'améliore significativement avec l'augmentation de la taille du lot. Pour un lot de 1, les débits des trois modèles varient de 192.95 à 334.46 jetons par seconde. Pour un lot de 32, ils atteignent respectivement 4158.67 4021.40, 4194.13 1 et 32 3 jetons par seconde pour DeepSeek-R27-Distill-Qwen-32B, GEMMA-XNUMX-XNUMXB-IT et Qwen/QwQ-XNUMXB.
| Performances de l'étape de pré-remplissage LLM : latence (ms) et débit de jetons (jetons/s) |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
| Taille du lot | DeepSeek-R1-Distillation-Qwen-32B |  | GEMMA-3-27B-IT |  | Qwen/QwQ-32B |  | 
|---|---|---|---|---|---|---|
|  | Latence (ms) | Taux de jeton | Latence (ms) | Taux de jeton | Latence (ms) | Taux de jeton | 
| 1 | 29.97 | 333.64 | 51.84 | 192.95 | 29.90 | 334.46 | 
| 2 | 30.21 | 662.09 | 52.55 | 380.61 | 29.95 | 667.80 | 
| 4 | 32.40 | 1234.72 | 52.62 | 760.12 | 32.12 | 1245.47 | 
| 8 | 36.98 | 2163.46 | 52.66 | 1519.19 | 36.69 | 2180.66 | 
| 16 | 51.63 | 3125.50 | 60.96 | 2624.64 | 51.29 | 3147.61 | 
| 32 | 76.95 | 4158.67 | 79.58 | 4021.40 | 76.30 | 4194.13 | 
Étape de décodage
Contrairement à l'étape de pré-remplissage, la latence pendant l'étape de décodage reste relativement stable quelle que soit la taille du lot. Par exemple, la latence de DeepSeek-R1-Distill-Qwen-32 B varie de 27.14 ms à 29.52 ms lorsque la taille du lot augmente de 2 à 32.
Le débit de jetons lors de la phase de décodage s'améliore avec la taille du lot, mais pas de manière aussi spectaculaire que lors de la phase de préremplissage. Pour un lot de 1, le débit est d'environ 36-37 jetons par seconde pour DeepSeek-R1-Distill-Qwen-32B et Qwen/QwQ-32B, et de 33.96 jetons par seconde pour GEMMA-3-27B-IT. Pour un lot de 32, les débits augmentent respectivement à 1083.83 873.39, 1084.89 et XNUMX XNUMX jetons par seconde.
| Performances de décodage LLM (jeton) : latence (ms) et débit de jetons (jetons/s) |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
| Taille du lot | DeepSeek-R1-Distillation-Qwen-32B |  | GEMMA-3-27B-IT |  | Qwen/QwQ-32B |  | 
|---|---|---|---|---|---|---|
|  | Latence (ms) | Taux de jeton | Latence (ms) | Taux de jeton | Latence (ms) | Taux de jeton | 
| 1 | 27.24 | 36.71 | 29.45 | 33.96 | 27.24 | 36.71 | 
| 2 | 27.14 | 73.70 | 30.80 | 64.93 | 27.14 | 73.69 | 
| 4 | 27.50 | 145.46 | 31.33 | 127.65 | 27.47 | 145.62 | 
| 8 | 27.91 | 286.61 | 32.54 | 245.83 | 27.90 | 286.78 | 
| 16 | 28.31 | 565.07 | 34.71 | 460.92 | 28.44 | 562.56 | 
| 32 | 29.52 | 1083.83 | 36.64 | 873.39 | 29.50 | 1084.89 | 
Ceci est normal, car l'étape de préremplissage calcule les états cachés initiaux et les caches clé-valeur pour l'intégralité de l'invite de saisie, ce qui peut saturer le GPU, car de grandes opérations par lots peuvent être exécutées simultanément. Après le traitement de l'invite, le modèle génère de nouveaux jetons, généralement un par un. À chaque étape, le modèle utilise le jeton précédent et les états cachés mis en cache pour produire le jeton suivant. Comme cette étape procède jeton par jeton, la taille du lot est souvent réduite, ce qui entraîne une sous-utilisation fréquente du GPU.
Benchmark de vision par ordinateur Procyon AI
À l'aide de tâches de vision artificielle concrètes, le benchmark Procyon AI Computer Vision évalue les performances d'inférence IA sur les processeurs, les GPU et les accélérateurs IA. Il prend en charge plusieurs moteurs d'inférence tels que TensorRT, OpenVINO, SNPE, Windows ML et Core ML, offrant ainsi des informations sur l'efficacité, la compatibilité et l'optimisation.
Les résultats du benchmark Procyon AI Computer Vision démontrent également d'excellentes performances d'inférence IA. Le système a obtenu de faibles temps d'inférence, avec MobileNet V3 à 20.64 ms et ResNet 50 à 22.42 ms. Inception V4 et DeepLab ont fonctionné respectivement à 65.23 ms et 41.37 ms, gérant efficacement des charges de travail de vision plus complexes. YOLO V3, un modèle clé de détection d'objets, a traité en 37.80 ms, ce qui le rend particulièrement adapté aux applications d'IA en temps réel. REAL-ESRGAN, un modèle de super-résolution à forte intensité de calcul, a enregistré 1,159.22 81 ms, ce qui nous a valu un score global de XNUMX en AI Computer Vision.
| Vision par ordinateur IA (une durée plus courte est meilleure) (un score plus élevé est meilleur) | Dell PowerEdge R770 (2 processeurs Intel Xeon 6787P \| 2 To de RAM) | 
|---|---|
| Temps d'inférence moyen MobileNet V3 | 20.64 ms | 
| Temps d'inférence moyen ResNet 50 | 22.42 ms | 
| Temps d'inférence moyen Inception V4 | 65.23 ms | 
| Temps d'inférence moyen de DeepLab | 41.37 ms | 
| Temps d'inférence moyen YOLO V3 | 37.80 ms | 
| Temps d'inférence moyen REAL-ESRGAN | 1,159.22 ms | 
| Score global de vision par ordinateur IA | 81 | 
Hammer DB TPROC-C
Nous avons également évalué les performances de quatre bases de données open source populaires (MariaDB 11.4.4, MySQL 8.4.4, MySQL 5.7.44 et PostgreSQL 17.2) à l'aide du benchmark HammerDB TPROC-C pour simuler les charges de travail OLTP sur 500 entrepôts.
MariaDB s'est imposée comme la solution la plus performante, notamment dans les configurations à double socket, où elle a évolué efficacement et atteint le débit de transactions le plus élevé. MySQL 8.4.4 a enregistré des améliorations notables par rapport à l'ancienne version 5.7.44, mettant en évidence les améliorations des versions récentes. PostgreSQL 17.2 a fourni des performances constantes, mais a affiché un léger retard par rapport à MariaDB et MySQL 8.4.4. MariaDB a fourni 3.15 millions de TPM sur un seul socket et 5.8 millions de TPM sur deux sockets, surpassant les autres dans les deux scénarios.
Tableau de comparaison des performances (Transactions par minute, TPM)
| Moteur de base de données | TPM à socket unique | TPM à double socket | 
|---|---|---|
| MariaDB 11.4.4 | 3,150,000 | 5,800,000 | 
| MySQL 8.4.4 | 2,850,000 | 5,150,000 | 
| PostgreSQL 17.2 | 2,700,000 | 4,900,000 | 
| MySQL 5.7.44 | 2,300,000 | 4,250,000 | 
Malgré la puissance matérielle du R770, avec ses 86 cœurs par processeur (un mélange de cœurs à haute et basse priorité), aucune base de données n'a enregistré de gains de performances significatifs lorsqu'elle était répartie sur les deux sockets. Cela reflète la préférence générale des bases de données open source pour l'exécution sur un seul socket, en raison d'une meilleure localisation du cœur et d'une latence mémoire réduite.
Compte tenu de ces résultats, le R770 est plus adapté à l'exécution de plusieurs instances de bases de données dans un environnement virtualisé qu'à la mise à l'échelle d'une seule instance. L'architecture du système est idéale pour prendre en charge une charge de travail de bases de données mixtes à haute densité, exploitant à la fois les cœurs de performance et d'efficacité pour assurer un débit constant sur plusieurs instances.
7-Zip
L'outil de référence de mémoire intégré de l'utilitaire populaire 7-Zip mesure les performances du processeur et de la mémoire d'un système pendant les tâches de compression et de décompression, indiquant dans quelle mesure le système peut gérer les opérations gourmandes en données.
Lors du benchmark 7-Zip, le système Dell a obtenu une note supérieure (266.425 GIPS) à celle de Lenovo (224.313 GIPS) pour les tâches de compression, le système Dell affichant une utilisation du processeur légèrement inférieure. Cependant, Lenovo a surpassé Dell en décompression, avec une note supérieure (288.457 GIPS contre 256.154 GIPS) et une utilisation du processeur légèrement supérieure. Dell a obtenu une note globale légèrement supérieure (261.290 GIPS), démontrant une meilleure efficacité globale pour les tâches de compression et de décompression.
| Compression 7-Zip et Décompression | Dell PowerEdge R770 (2 processeurs Intel Xeon 6787P \| 2 To de RAM) | Lenovo ThinkSystem SR630 V4 (2 x Intel Xeon 6780E \| 512 Go de RAM) | 
|---|---|---|
| Compression – Utilisation actuelle du processeur | 5267 % | 5064 % | 
| Compression – Courant nominal/utilisation | 5.061 GIPS | 4.341 GIPS | 
| Compression – Courant nominal | 266.591 GIPS | 219.840 GIPS | 
| Compression – Utilisation du processeur résultante | 5270 % | 5156 % | 
| Compression – Évaluation/utilisation résultante | 5.056 GIPS | 4.350 GIPS | 
| Compression – Évaluation résultante | 266.425 GIPS | 224.313 GIPS | 
| Décompression – Utilisation actuelle du processeur | 5623 % | 6184 % | 
| Décompression – Évaluation/utilisation actuelle | 4.586 GIPS | 4.688 GIPS | 
| Décompression – Courant nominal | 257.909 GIPS | 289.879 GIPS | 
| Décompression – Utilisation du processeur résultante | 5627 % | 6205 % | 
| Décompression – Évaluation/utilisation résultante | 4.553 GIPS | 4.649 GIPS | 
| Décompression – Évaluation résultante | 256.154 GIPS | 288.457 GIPS | 
| Total – Utilisation totale du processeur | 5448 % | 5681 % | 
| Total – Note totale/Utilisation | 4.804 GIPS | 4.500 GIPS | 
| Total – Note totale | 261.290 GIPS | 256.385 GIPS | 
croque-y
y-cruncher est une application de benchmarking et de test de résistance populaire lancée en 2009. Ce test est multithread et évolutif, calculant Pi et d'autres constantes jusqu'à des milliers de milliards de chiffres. Plus vite c'est mieux dans ce test. Ce logiciel a été fantastique pour tester les plates-formes à nombre de cœurs élevé et montrer les avantages de calcul entre les plates-formes à un ou deux sockets.
Les résultats du benchmark Y-Cruncher montrent un écart de performances significatif entre le Dell PowerEdge R770, équipé de processeurs P-core, et le Lenovo ThinkSystem SR630 V4 équipé de processeurs E-core, notamment à mesure que la taille du jeu de données augmente. Il s'agit moins de déterminer quel système est le meilleur que de comparer les types de processeurs sous cette charge de travail.
Pour les calculs de moindre envergure, le système Dell était déjà en tête, calculant 1 milliard de chiffres de Pi en 2.753 secondes, tandis que le système Lenovo prenait plus du double, soit 5.997 secondes. À mesure que la charge de travail augmentait, l'écart se creusait. À 10 milliards de chiffres, le système Dell a terminé en 34.873 secondes, soit moins de la moitié du temps de 81.046 secondes du système Lenovo. Au-delà des 50 milliards de chiffres, Dell a conservé son avance, réalisant la tâche en 221.255 secondes, contre 476.826 secondes pour Lenovo, soit une vitesse de 53 % pour Dell.
À 100 milliards de chiffres, Lenovo n'a pas pu terminer le test en raison de sa configuration actuelle de 512 Go de RAM. Avec 2 To de RAM, Dell a géré la charge de travail efficacement, terminant en 491.737 secondes.
| Y-cruncher (une durée plus courte est meilleure) | Dell PowerEdge R770 (2 processeurs Intel Xeon 6787P \| 2 To de RAM) | Lenovo ThinkSystem SR630 V4 (2 x Intel Xeon 6780E \| 512 Go de RAM) | 
|---|---|---|
| 1 milliard | 2.753 secondes | 5.997 secondes | 
| 2.5 milliard | 7.365 secondes | 17.573 secondes | 
| 5 milliard | 16.223 secondes | 37.793 secondes | 
| 10 milliard | 34.873 secondes | 81.046 secondes | 
| 25 milliard | 99.324 secondes | 220.025 secondes | 
| 50 milliard | 221.255 secondes | 476.826 secondes | 
| 100 milliard | 491.737 secondes |  | 
Mixeur OptiX
Une application de modélisation 3D open source. Ce benchmark a été réalisé avec l'utilitaire Blender Benchmark. Le score est exprimé en échantillons par minute, le plus élevé étant le meilleur.
Les résultats du benchmark Blender montrent un net avantage en termes de performances pour le Dell PowerEdge R770 par rapport au Lenovo ThinkSystem SR630 V4, notamment en termes de rendu CPU. Dans le test « CPU Monster », Dell a atteint 1,706.002 19 échantillons par minute, soit une avance de 1,432.09 % sur les 1,169.370 914.75 échantillons par minute de Lenovo. Le test « CPU Junkshop » a encore accentué cet écart, le Dell atteignant 28 791.475 échantillons par minute, surpassant de 656.68 % les 20 échantillons par minute de Lenovo. De même, Dell a enregistré XNUMX échantillons par minute dans le test « CPU Classroom », tandis que Lenovo était à la traîne avec XNUMX échantillons par minute, soit une différence de XNUMX %.
L'absence de GPU dans le système Lenovo signifiait également qu'il ne pouvait pas participer au rendu basé sur le GPU, où le NVIDIA L4 de Dell affichait un score de 1,895.71 950.42 échantillons/min pour Monster, 968.43 échantillons/min et un score Classroom de XNUMX échantillons/min.
| Analyse comparative du CPU Blender | Dell PowerEdge R770 (2 processeurs Intel Xeon 6787P \| 2 To de RAM) | Lenovo ThinkSystem SR630 V4 (2 x Intel Xeon 6780E \| 512 Go de RAM) | 
|---|---|---|
| Monstre CPU (Blender 4.3) | 1,706.002 XNUMX échantillons/min | 1432.09 XNUMX échantillons/min | 
| Magasin de récupération de processeurs (Blender 4.3) | 1,169.370 XNUMX échantillons/min | 914.75 XNUMX échantillons/min | 
| Classe CPU (Blender 4.3) | 791.475 XNUMX échantillons/min | 656.68 XNUMX échantillons/min | 
| Monstre GPU (Blender 4.3) | 1,895.712 XNUMX échantillons/min | (pas de GPU) | 
| Magasin de GPU (Blender 4.3) | 950.424 XNUMX échantillons/min | (pas de GPU) | 
| Classe GPU (Blender 4.3) | 968.432 XNUMX échantillons/min | (pas de GPU) | 
Cinebench R23
L'outil de référence Cinebench R23 évalue les performances du processeur d'un système en restituant une scène 3D complexe à l'aide du moteur Cinema 4D. Il mesure les performances monocœur et multicœur, offrant une vue complète des capacités du processeur dans la gestion des tâches de rendu 3D.
Dans Cinebench R23, les résultats du benchmark mettent en évidence des différences notables de performances CPU entre le Dell PowerEdge R770 et le Lenovo ThinkSystem SR630 V4, notamment en termes de nombre de cœurs par processeur. Le Lenovo ThinkSystem SR630 V4, équipé de deux processeurs Intel Xeon 2E (6780 cœurs par processeur), a surpassé le Dell au test CPU multicœur avec un score de 144 99,266 points, contre 74,710 288 points pour le Dell. Cet écart reflète l'avantage de Lenovo dans les charges de travail multithread, grâce à son nombre de cœurs plus élevé (2 cœurs au total) par rapport aux deux processeurs Intel Xeon 6787P du Dell (86 cœurs par processeur), ce qui limite ses performances multicœurs.
Lors du test CPU Single-Core, Dell a obtenu de meilleurs résultats avec un score de 1,272 894 points, surpassant les XNUMX points de Lenovo, soulignant l'efficacité supérieure du processeur monothread de Dell malgré son nombre de cœurs inférieur.
| Cinebench R23 | Dell PowerEdge R770 (2 processeurs Intel Xeon 6787P \| 2 To de RAM) | Lenovo ThinkSystem SR630 V4 (2 x Intel Xeon 6780E \| 512 Go de RAM) | 
|---|---|---|
| Processeur multicœur | 74,710 pts | 99,266 pts | 
| Processeur monocœur | 1,272 pts | 894 pts | 
| Rapport PM | 58.74 x | 111.00 x | 
Cinebench 2024
Cinebench 2024 étend les capacités de référence de R23 en ajoutant une évaluation des performances du GPU. Il continue de tester les performances du processeur mais inclut également des tests qui mesurent la capacité du GPU à gérer les tâches de rendu.
Dans ce benchmark mis à jour, le Dell PowerEdge R770 a obtenu 12,996 630 points pour les performances du GPU, soulignant sa capacité à gérer les tâches de rendu accélérées par le GPU. Le Lenovo ThinkSystem SR4 VXNUMX n'a pas de GPU dédié et n'a donc pas enregistré de score GPU.
Lors du test CPU multicœur, le Lenovo a obtenu 2,884 2,831 points, légèrement devant les 71 53 points de Dell, ce qui témoigne d'un léger avantage en termes de performances multicœurs. En CPU monocœur, le Dell a surpassé le Lenovo, obtenant XNUMX points, contre XNUMX points pour Lenovo, ce qui témoigne des performances supérieures du Dell en monocœur malgré un nombre de cœurs réduit.
| Cinebench R24 | Dell PowerEdge R770 (2 processeurs Intel Xeon 6787P \| 2 To de RAM) | Lenovo ThinkSystem SR630 V4 (2 x Intel Xeon 6780E \| 512 Go de RAM) | 
|---|---|---|
| Score GPU | 12,996 pts |  | 
| Processeur multicœur | 2,831 pts | 2,884 pts | 
| Processeur monocœur | 71 pts | 53 pts | 
| Rapport PM | 39.77 x | 54.43 x | 
Geekbench 6
Geekbench 6 est un outil d'évaluation des performances multiplateforme qui mesure les performances globales d'un système. Le navigateur Geekbench permet de comparer n'importe quel système à cet outil.
Les résultats du benchmark Geekbench 6 montrent des différences de performances évidentes entre le Dell PowerEdge R770 et le Lenovo ThinkSystem SR630 V4. Lors du test CPU monocœur, le Dell a surpassé le Lenovo avec un score de 1,797 1,173, tandis que ce dernier a obtenu 53 XNUMX, soit une amélioration de XNUMX % des performances monocœur pour le Dell.
Lors du test CPU multicœur, Dell a de nouveau dominé avec 15,880 13,868 points, tandis que Lenovo a obtenu 14 6787 points, ce qui confère à Dell un avantage de XNUMX % en performances multicœurs. Cela suggère que les processeurs Intel Xeon XNUMXP de Dell offrent une puissance de calcul globale supérieure, notamment pour les tâches nécessitant plusieurs cœurs.
Le test GPU OpenCL a encore plus mis en évidence l'avantage de Dell, avec un score de 148,730 4 grâce au GPU NVIDIA LXNUMX.
| Geekbench 6 (Plus c'est mieux) | Dell PowerEdge R770 (2 processeurs Intel Xeon 6787P \| 2 To de RAM) | Lenovo ThinkSystem SR630 V4 (2 x Intel Xeon 6780E \| 512 Go de RAM) | 
|---|---|---|
| Processeur monocœur | 1,797 | 1,173 | 
| Processeur multicœur | 15,880 | 13,868 | 
| Score OpenCL du GPU | 148,730 | (pas de GPU) | 
Test de vitesse Blackmagic RAW
Le Blackmagic RAW Speed Test est un outil d'analyse comparative des performances conçu pour mesurer les capacités d'un système à gérer la lecture et l'édition vidéo à l'aide du codec Blackmagic RAW. Il évalue la capacité d'un système à décoder et à lire des fichiers vidéo haute résolution, en fournissant des fréquences d'images pour le traitement basé sur le CPU et le GPU.
Lors du test basé sur le processeur, le Dell PowerEdge R770 a atteint 141 ips, surpassant le Lenovo ThinkSystem SR630 V4, qui a obtenu 120 ips. Cela indique que le système Dell gère le traitement vidéo basé sur le processeur plus efficacement que le Lenovo. Lors du test basé sur le processeur graphique, le Dell PowerEdge R770 a obtenu 157 ips, bénéficiant de la présence d'un processeur graphique NVIDIA.
| Test de vitesse Blackmagic RAW (plus c'est élevé, mieux c'est) | Dell PowerEdge R770 (2 processeurs Intel Xeon 6787P \| 2 To de RAM) | Lenovo ThinkSystem SR630 V4 (2 x Intel Xeon 6780E \| 512 Go de RAM) | 
|---|---|---|
| FPS CPU | FPS 141 | FPS 120 | 
| FPS CUDA | FPS 157 | 0 FPS (pas de GPU) | 
Test de vitesse du disque Blackmagic
Le test de vitesse du disque Blackmagic évalue les vitesses de lecture et d'écriture d'un disque, en évaluant ses performances, en particulier pour les tâches de montage vidéo. Il aide les utilisateurs à s'assurer que leur stockage est suffisamment rapide pour le contenu haute résolution, comme la vidéo 4K ou 8K.
Lors du test de vitesse Blackmagic, la carte Dell PowerEdge R770 Boss avec SK hynix 480 Go Dell NVMe en miroir a atteint une vitesse de lecture de 3,010.3 976.3 Mo/s et une vitesse d'écriture de XNUMX Mo/s.
Conclusion
Le Dell PowerEdge R770 nous enthousiasme particulièrement, grâce à son adoption de la norme Data Center Modular Hardware System de l'Open Compute Project et à son matériel de pointe. L'intégration d'OCP DC MHS offre de nombreux avantages, notamment une modularité accrue, une facilité de maintenance améliorée et une réduction potentielle des coûts grâce à une standardisation accrue. Cette philosophie de conception se reflète dans tous les aspects du système, de la mise en œuvre de l'iDRAC comme OCP DC-SCM jusqu'aux ports.
Le R770 offre également des capacités de stockage impressionnantes, prenant en charge jusqu'à 40 disques E3.S dans un seul châssis 2U, ce qui en fait la solution idéale pour les charges de travail gourmandes en stockage. De plus, la flexibilité du serveur est renforcée par la prise en charge de diverses configurations, notamment une configuration accessible en couloir froid des E/S avant, offrant une plus grande flexibilité pour s'adapter aux différentes configurations de centres de données et aux exigences de maintenance.
Compatible avec une large gamme de GPU et les processeurs Intel Xeon 6 Performance, le R770 est une plateforme serveur puissante et polyvalente, parfaitement adaptée aux exigences des centres de données modernes. Son matériel de pointe, sa conception modulaire et ses fonctionnalités de sécurité robustes font du R770 une option attractive pour les entreprises souhaitant déployer des applications d'IA, de calcul haute performance (HPC) et des charges de travail d'entreprise traditionnelles.