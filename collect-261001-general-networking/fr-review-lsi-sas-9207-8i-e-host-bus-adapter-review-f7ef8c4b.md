Bien que les adaptateurs de bus hôte LSI SAS 9207-8i/e existent depuis quelques années maintenant, ils sont toujours la norme de facto pour les solutions SAS2, qui a été immédiatement adaptée lors de la sortie de PCIe 3.0. Depuis la création des HBA et des contrôleurs compatibles SAS 6 Gb/s, LSI a été son plus grand promoteur, ayant fabriqué des appareils incroyablement réussis depuis le début de la gamme 9200. Avec ces adaptateurs HBA, le LSI a joué un rôle déterminant dans l'intégration du 6 Gb/s dans le courant dominant.
Cela dit, les LSI SAS 9207-8i et 9207-8e ne font pas exception à ce fait en raison de leur équilibre entre prix abordable, performances et évolutivité interne. Les HBA LSI sont conçus pour les applications de stockage de serveur de milieu de gamme, y compris le stockage hiérarchisé et la sauvegarde et la restauration avec la capacité du 9207-8i de connecter jusqu'à 256 périphériques SAS et SATA grâce à ses 8 ports SAS 6 Gb/s internes, tandis que le 9207- 8e prend en charge jusqu'à 1024 SAS ou SATA avec ses 8 ports externes. Le 8i et le 8e sont tous deux équipés du contrôleur d'E/S SAS LSISAS2308 6 Gb/s ainsi que d'un processeur PowerPC double cœur 800 MHz.
En ce qui concerne l'encombrement, les HBA LSI peuvent s'intégrer dans des serveurs et des stations de travail montés en rack 1U/2U avec un facteur de forme compact et prennent en charge toutes les applications critiques qui utilisent la bande passante de la connectivité PCIe 3.0.
Spécifications techniques
- Modèle : LSI SAS 9207-8i (LSI00302)
- Contenu : HBA, deux câbles Mini-SAS vers SAS x4 (CBL-SAS8087OCF-06M) et CD de documentation
- Supports : Pleine hauteur et profil bas
- Type de bus hôte : x8 voies PCI Express 3.0
- Dimensions physiques : Profil bas (2.6" x 6.6")
- Ports internes : 8
- Support de câble : Cuivre passif
- Taux de transfert de données : 6 Gb/s conforme à la norme SAS 2.1
- Périphériques pris en charge : 256 périphériques SAS/SATA non RAID
- Prise en charge du système d'exploitation : Microsoft Windows, Linux (SuSE, Red Hat), Solaris, VMware, FreeBSD
- Prise en charge de Fusion MPT : Fusion MPT 2.0
- Contrôleur d'E/S : LSI SAS2308
- Connecteurs internes : 2 Mini-SAS SFF8087
- MTBF : > 2,000,000 XNUMX XNUMX heures
- Température de fonctionnement: 0 ° C à 55 ° C
- Humidité en fonctionnement:% 5 à 90% sans condensation
- Tension de fonctionnement : +12 V +/-8 % ; 3.3V +/-8%
- Alimentation PCI : 9.8 W typique, débit d'air 200 LFM
- Garantie : 3 ans ; avec option de remplacement avancée
Conception et construction
Comme nous l'avons mentionné ci-dessus, les HBA LSI SAS 9207-8i/e sont des facteurs de forme à profil bas qui s'intègrent dans n'importe quel rack de serveur 1U et 2U, offrant 8 voies de SAS 6 Gb/s ainsi que 8 voies de PCI Express 3.0 avec 8 Gb/s. . La ligne 9207 est sensiblement différente des nouveaux modèles LSI (tels que la série 93XX), y compris une nuance de vert différente ainsi qu'une forme différente.
Au cœur des 9207-8i et 9207-8e se trouve le contrôleur SAS LSISAS2308 6Gb/s, qui est recouvert d'un grand dissipateur thermique. Le contrôleur LSI est intégré aux processeurs PowerPC, ce qui permet un déchargement maximal du processeur hôte et donne à LSI la possibilité de publier un seul pilote de système d'exploitation binaire pour faire fonctionner n'importe quel contrôleur ou adaptateur Fusion MPT ; une caractéristique très attrayante pour les entreprises.
Les deux connecteurs mini-SAS internes SFF-9207 (SFF8) du 4-8087i pour son interface PCIe 8087 sont situés à l'extrémité de la carte, ce qui fournit des taux de transfert de données SAS et SATA de 3.0, 1.5 et 3 Gb/s par voie avec la capacité d'atteindre plus de 6 650 IOP. Le 9207-8i peut prendre en charge jusqu'à 256 périphériques non RAID.
Le 9207-8e est équipé de deux (x4) connecteurs mini-SAS externes (SFF8088) situés à l'avant gauche de la carte, avec la possibilité d'offrir également des taux de transfert de données SAS et SATA de 1.5, 3 et 6 Gb/s par voie à 650 9207 IOP. Le 8-1024e peut prendre en charge jusqu'à XNUMX XNUMX périphériques non RAID.
Analyse de la charge de travail des applications
Pour évaluer ses performances lors d'une charge de travail applicative, nous examinerons le test du Toshiba HK3R2, où le contrôleur LSI SAS 9207-8i a été utilisé. Comme vous le verrez ci-dessous, ses performances sont impressionnantes.
Notre environnement de base de données MarkLogic NoSQL nécessite des groupes de quatre disques SSD d'une capacité utile d'au moins 200 Go, car la base de données NoSQL nécessite environ 650 Go d'espace pour ses quatre nœuds de base de données. Notre protocole utilise un hôte SCST et présente chaque SSD dans JBOD, avec un alloué par nœud de base de données. Le test se répète sur 24 intervalles, nécessitant entre 30 et 36 heures au total. MarkLogic enregistre la latence moyenne totale ainsi que la latence d'intervalle pour chaque SSD.
Dans nos tests de latence moyenne globale utilisant notre référence de base de données MarkLogic NoSQL, le HK3R2 a affiché une latence moyenne globale de 2.122 ms lors de l'utilisation du 9207-8i.
Le test de performance suivant consiste à évaluer les performances d'une base de données Percona MySQL OLTP à l'aide de SysBench . Dans cette configuration, nous avons utilisé un groupe de serveurs Lenovo ThinkServer RD630 comme clients de base de données, l'environnement étant stocké sur un seul disque. Pour cette analyse, nous nous concentrerons uniquement sur ses performances en termes de transactions moyennes par seconde (TPS).
Tirant parti du HBA 9207-8i, le SSD Toshiba HK3R2 a poursuivi ses meilleures performances de sa catégorie, atteignant près de 1,700 XNUMX TPS. Ces résultats en font le grand gagnant parmi ses comparables.
Le protocole de test OLTP Microsoft SQL Server de StorageReview utilise la version préliminaire actuelle du benchmark TPC-C (Transaction Processing Performance Council's Benchmark C), un benchmark de traitement transactionnel en ligne qui simule les activités rencontrées dans des environnements applicatifs complexes. Le benchmark TPC-C est plus représentatif que les benchmarks de performance synthétiques des performances réelles, permettant d'évaluer avec précision les points forts et les goulots d'étranglement de l'infrastructure de stockage dans les environnements de bases de données. Notre protocole SQL Server utilise une base de données SQL Server de 685 Go (échelle 3 000). Nous analyserons ses performances en termes de latence avec une charge de 30 000 unités virtuelles (VU).
Ici, le Toshiba HK3R2 affichait un impressionnant 12.0 ms (à égalité avec le Samsung 845DC Pro) pour la meilleure latence moyenne globale.
Analyse synthétique de la charge de travail d'entreprise
Pour mesurer les performances synthétiques d'entreprise du contrôleur HBA LSI SAS 9207-8i/e, nous prendrons comme référence le châssis d'extension de stockage JBOD iXsystems Titan 316J sur lequel il a été utilisé avec les disques suivants :
- Toshiba MK01GRRB (147 Go, 15,000 6.0 tr/min, XNUMX Gb/s SAS)
- Toshiba MBF2600RC (600 Go, 10,000 6.0 tr/min, XNUMX Gb/s SAS)
- Hitachi Ultrastar 7K4000 (4 To, 7,200 6.0 tr/min, XNUMX Gb/s SATA)
La plateforme de tests d'entreprise était basée sur un serveur Lenovo ThinkServer RD630 . Le ThinkServer RD630 est configuré avec :
- 2 x Intel Xeon E5-2620 (2.0 GHz, cache de 15 Mo)
- Windows Server 2008 R2 SP1 64 bits, Windows Server 2012 64 bits et CentOS 6.3 64 bits
- Jeu de puces Intel C602
- Mémoire – 16 Go (2 x 8 Go) 1333 Mhz DDR3 enregistrés RDIMM
- HBA LSI 9207 SAS/SATA 6.0Gb/s
Notre processus de référence de stockage d'entreprise commence par une analyse des performances du disque au cours d'une phase de préconditionnement approfondie. Chacune des baies de disques durs comparables est configurée en RAID10, autorisée à se synchroniser entièrement, puis testée sous une charge importante de 16 threads avec une file d'attente exceptionnelle de 16 par thread jusqu'à notre charge légère de 2 threads avec une file d'attente exceptionnelle de 2 par fil de discussion.
Nous examinerons deux des six analyses de charge de travail synthétiques d'entreprise qui ont été testées :
- Séquentiel
  - 8K
    - 100 % de lecture ou 100 % d'écriture
    - 100% 8K
  - 128K
    - 100 % de lecture ou 100 % d'écriture
    - 100% 128K
- 8K
Dans les performances de lecture et d'écriture séquentielles 8K, la vitesse de broche et la densité surfacique sont utilisées. Ici, le Hitachi Ultrastar 7,200K7 RAID4000 à 10 8 tr/min offrait la vitesse de lecture 911K la plus rapide, mesurant 82 Mo/s (15 Mo/s en écriture), tandis que le Toshiba 10K RAID811 mesurait 186 Mo/s (10 Mo/s en écriture) et le Toshiba 10K RAID612 mesurait 178 Mo. /s (XNUMX Mo/s en écriture).
Notre prochain test séquentiel a mesuré les vitesses de transfert de gros blocs. Dans ce profil de charge de travail utilisant le HBA LSI SAS 9207-8i/e, la baie SAS 15K mesurait 1,535 839 Mo/s en lecture et 7.2 Mo/s en écriture, tandis que la baie SATA 1,361 K enregistrait 912 10 Mo/s en lecture et 1,142 Mo/s en écriture, et la matrice SAS 540K avec XNUMX XNUMX Mo/s en lecture et XNUMX Mo/s en écriture.
Conclusion
Les HBA LSI SAS 9207 apportent une solution RAID SAS2 impressionnante pour les entreprises disposant de serveurs de stockage haut de gamme, en particulier pour ceux qui recherchent un équilibre entre prix abordable, performances, évolutivité ainsi que la fiabilité donnée de LSI. Conçu pour les applications de stockage de serveur de milieu de gamme, y compris le stockage hiérarchisé, la sauvegarde et la restauration, le 9207-8i connecte jusqu'à 256 périphériques SAS et SATA avec ses 8 ports SAS internes 6 Gb/s, tandis que le 9207-8e prend en charge jusqu'à 1024 périphériques SAS ou SATA avec ses 8 ports externes ; cependant, il doit s'agir de périphériques non RAID
Dans l'ensemble, la série 9207 est à la fois extrêmement stable et axée sur les performances. Le contrôleur d'E/S SAS LSISAS2308 6 Gb/s équipé, ainsi que son processeur PowerPC double cœur 800 MHz, permettent à ces HBA de se vanter de ces performances impressionnantes. Cela était certainement évident dans pratiquement toutes les applications pour lesquelles nous avons exploité les HBA LSI, comme une latence très faible et un débit très élevé avec l'extension de stockage JBOD iXsystems Titan 316J et le SSD Toshiba HK3R2, comme démontré ci-dessus. Par exemple, le lecteur Toshiba a affiché le meilleur benchmark de base de données MarkLogic NoSQL que nous ayons vu à ce jour, avec plus de 2.122 ms sur 32 threads tout en atteignant presque 1,700 32 TPS sur XNUMX threads dans notre benchmark MySQL.
Globalement, ces cartes LSI HBA sont difficiles à battre en matière de technologie SAS2, bien que les utilisateurs de SAS3 devraient se tourner vers la série LSI 9300.
Conclusion
LSI SAS 9207-8i/e est une série impressionnante de HBA offrant un équilibre entre accessibilité, performances et évolutivité tout en offrant aux entreprises une solution idéale pour les serveurs de stockage haut de gamme.