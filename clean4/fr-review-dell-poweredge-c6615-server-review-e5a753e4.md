La plate-forme Dell PowerEdge C-Series dispose d'un châssis 2U prenant en charge quatre serveurs dans la catégorie Dell Modular Infrastructure. En fonction de la charge de travail, le système C-series peut être configuré avec deux types de nœuds différents : un nœud AMD C6615 à socket unique ou un nœud Intel C6620 à double socket.
Notre examen se concentrera sur le châssis de la série C, qui comporte quatre nœuds AMD EPYC à socket unique connectés à un fond de panier de disque E8.S PCIe Gen3 à 5 baies.
Du point de vue du stockage, la plate-forme peut être configurée avec un fond de panier de disque SFF de 2.5 pouces, qui prend en charge jusqu'à 24 SSD NVMe ou un support Gen5 en exploitant un fond de panier E8.S à 3 baies. En interne, ces disques sont connectés directement à chaque nœud, avec une répartition égale sur les quatre serveurs. Par exemple, dans la configuration à 24 baies, chaque nœud voit six disques ; dans la configuration à 8 baies, chaque nœud voit deux disques.
Le châssis C6600 offre des alimentations et un refroidissement redondants partagés pour les quatre nœuds installés, bien qu'au-delà de cela, chaque nœud soit géré indépendamment. Ainsi, contrairement à un châssis lame géré avec un portail de gestion de châssis, il s'agit plutôt de quatre petits serveurs PowerEdge sous un même toit métallique. Chaque nœud C6615 dispose de connexions réseau dédiées, d'une interface iDRAC et d'emplacements PCIe pour l'extension.
Spécifications du nœud Dell PowerEdge C6615
| Spécifications C6615 |  | 
|---|---|
| Processeur | Un processeur AMD EPYC avec jusqu'à 64 cœurs | 
| Mémoire | 6 emplacements DIMM DDR5, prend en charge RDIMM de 576 Go (6 x 96 Go) maximum, vitesses allant jusqu'à 4800 XNUMX MT/s | 
| Contrôleurs de stockage | Contrôleurs internes (RAID) : PERC H755N, PERC H355 Démarrage interne : sous-système de stockage optimisé au démarrage (NVMe BOSS-N1) : HWRAID 1, 2 x SSD M.2 HBA SAS 12 Gbit/s internes (non RAID) : HBA355i RAID logiciel : S160 | 
| Disponibilité | Disques durs et blocs d'alimentation redondants enfichables à chaud | 
| Baies de disques | Baies avant : Jusqu'à 16 disques SAS/SATA (HDD/SSD) de 2.5 pouces, 61 To maximum Jusqu'à 16 disques SATA/NVMe de 2.5 pouces, 15.36 To maximum sur une configuration de fond de panier universel Jusqu'à 16 x 2.5 pouces sur fond de panier NVMe Jusqu'à 8 x E3.s sur le fond de panier de disque dur SSD NVMe | 
| Remplacement à chaud, alimentations redondantes | 3200 277 W 336 VCA ou XNUMX VCC 2800 200 W Titane 240-240 VCA ou XNUMX VCC Platine 2400 100 W 240-240 VCA ou XNUMX VCC 1800 200 W Titane 240-240 VCA ou XNUMX VCC | 
| Dimensions | Hauteur – 40.0 mm (1.57 pouces) Largeur – 174.4 mm (6.86 pouces) Profondeur – 549.7 mm (21.64 pouces), 561.3 mm (22.10 pouces) – SAS/SATA ou NVMe ou E3.S ou configuration universelle | 
| Poids | 3.7 kg (8.15 livres) | 
| Gestion intégrée | IDRAC9 IDRAC Direct API RESTful IDRAC avec Redfish Module de service IDRAC | 
| Logiciel OpenManage | Plug-in CloudIQ pour PowerEdge OpenManage Entreprise Intégration OpenManage Enterprise pour VMware Vcenter Intégration OpenManage pour Microsoft System Center Intégration d'OpenManage avec le centre d'administration Windows Plug-in OpenManage Power Manager Plug-in de service OpenManage Plug-in OpenManage Update Manager | 
| intégrations | BMC TrueSight Microsoft System Center Intégration d'OpenManage avec ServiceNow Intégration d'OpenManage avec le centre d'administration Windows Plug-in OpenManage Power Manager Plug-in de service OpenManage Plug-in OpenManage Update Manager | 
| Sécurité | Virtualisation cryptée sécurisée AMD (SEV) Chiffrement de mémoire sécurisé AMD (SME) Micrologiciel signé cryptographiquement Chiffrement des données au repos (SED avec gestion de clé locale ou externe) Vérification des composants Secure BootSecured (vérification de l'intégrité du matériel) Secure Erase Racine de confiance en silicone Verrouillage du système (nécessite IDRAC9 Enterprise ou Datacenter) TPM 2.0 FIPS, certifié CC-TCG, TPM 2.0 China NationZ | 
| Carte réseau intégrée | 1 x 1 Go | 
| Ports arrière | 1 x USB 3.0 1 port Ethernet IDRAC 1 port IDRAC Direct (Micro-AB USB) 1 x Mini DisplayPort | 
| Emplacements PCIE | Jusqu'à 2 emplacements PCIe x16 Gen5 Low-Profile 1 x OCP 3.0 x16 Gen5 | 
| Système d'exploitation et hyperviseurs | Serveur canonique Ubuntu LTS Serveur Microsoft Windows avec Hyper-V Red Hat Enterprise Linux Serveur d'entreprise SUSE Linux VMware ESXi/vSAN | 
Construire et concevoir
Le châssis Dell PowerEdge C6600 et les nœuds C6615 offrent une option informatique exceptionnellement dense pour les scénarios de déploiement qui doivent minimiser l'espace physique utilisé dans un environnement de montage en rack. Cela convient aux solutions hyperconvergées fonctionnant dans un environnement en cluster, nécessitant plusieurs nœuds ou des charges de travail lourdes qui ne nécessitent pas la consommation de 4U ou 8U via les conceptions de serveurs traditionnelles 1U ou 2U. Le châssis a une empreinte 2U avec une profondeur de 30 pouces. Le poids du châssis peut y monter en fonction de la configuration finale. Dell indique un poids maximum d'une configuration C16 à 6600 baies avec tous les disques installés à 93.69 livres.
L'avant du système est assez basique par rapport aux autres plates-formes PowerEdge, sans beaucoup de marque Dell. Ce type de serveur n'offre pas le cadre PowerEdge standard mais place les disques et les entrées de ventilateur au premier plan. L'avant de la version E3.S C6600 comporte huit SSD Gen5 NVMe au milieu, flanqués d'entrées de ventilateur de refroidissement.
Les oreilles latérales du châssis contiennent des boutons d'alimentation dédiés pour chaque nœud et des boutons d'information indiquant l'état ou les problèmes de ce nœud.
Chaque nœud C6615 dispose d'une disposition de ports condensée à l'arrière du châssis par rapport à un serveur 1U ou 2U traditionnel. Les ports incluent USB, iDRAC, un connecteur d'affichage et un port de service USB.
Pour la mise en réseau, un emplacement OCP est disponible pour différentes options d'interface (la nôtre dispose d'une carte réseau 25GbE à quatre ports), et deux emplacements PCIe sont également disponibles. Les emplacements OCP et double PCIe offrent une interface Gen5.
L'ouverture du châssis PowerEdge C6600 vous donne une visibilité sur la disposition de la manière dont le refroidissement, la distribution de l'alimentation et les chemins d'E/S des disques sont gérés. Le câblage PCIe/SAS du fond de panier du disque est acheminé directement vers chaque nœud via des raccords à connexion rapide qui transmettent également les données et l'alimentation.
En fonction de la configuration interne de chaque nœud, les connexions des lecteurs se connectent directement à la carte mère ou à une carte PERC pour les options RAID matérielles.
Hormis le refroidissement et l’alimentation, les nœuds ne partagent aucune autre ressource.
Performances du Dell PowerEdge C6615
Spécifications des nœuds testés
Nos quatre nœuds C6615 ont des configurations identiques. Nous les comparerons et montrerons les performances moyennes sur les nœuds.
- 1 processeur AMD EPYC 8534P 64 cœurs
- 6 x 96 Go DDR5 4800 576 Mo/s (XNUMX Go)
- Windows Server standard 2022
- SSD de démarrage Dell RAID1 BOSS
- 2 SSD PCIe Gen5 E3.S
Lors de nos tests de performances, les nœuds ont fonctionné en parallèle pour donner un score global prenant en compte les ressources d'alimentation et de refroidissement partagées.
Performances de stockage
Chacun des quatre nœuds Dell Power Edge C6615 comprend un SSD BOSS RAID1 pour le démarrage et deux baies E3.S pour les SSD d'entreprise Gen5. Bien que la carte BOSS ne soit pas en reste, elle offre un profil de performances très différent de celui des SSD E3.S.
Bien qu'une grande partie de cet examen se concentre sur les performances globales au niveau du système, nous avons légèrement abordé les deux types de stockage sur ce système avec des charges de travail aux quatre coins. Notre premier test s'est concentré sur le groupe SSD de démarrage BOSS RAID1.
| Dell BOSS RAID1 | Lire les performances | Performances d'écriture | 
|---|---|---|
| Séquentiel 1 Mo Q32/4T | 2,963MB / s | 1,067MB / s | 
| Aléatoire 4K Q32/8T | 600,786 0.426 IOPS (XNUMX ms) | 249,819 1.024 IOPS (XNUMX ms) | 
Ensuite, nous avons examiné un seul SSD Gen5 E3.S, qui comprenait le SSD KIOXIA CM7 de 7.68 To à lecture intensive dans notre système de test.
| KIOXIA 7.68 To CM7-R | Lire les performances | Performances d'écriture | 
|---|---|---|
| Séquentiel 1 Mo Q32/4T | 13,736MB / s | 7,089MB / s | 
| Aléatoire 4K Q32/8T | 931,671 0.266 IOPS (XNUMX ms) | 768,739 0.329 IOPS (XNUMX ms) | 
Cinebench R23
Cinebench R23 de Maxon est une référence de rendu de processeur qui utilise tous les cœurs et threads de processeur. Nous l'avons exécuté pour des tests multicœurs et monocœurs. Des scores plus élevés sont meilleurs. Voici les résultats pour toutes les puces EPYC.
Dans Cinebench R23, les quatre nœuds se situaient autour de 74,000 3 sur la partie multicœur, le nœud 75,000 se glissant dans 1 4. Les quatre nœuds sont restés beaucoup plus proches pour les scores monocœur, les nœuds 1,088 et 3 étant de 8 2. Le nœud 5 n’avait que XNUMX points de retard et le nœud XNUMX avait XNUMX points d’avance. Dans l’ensemble, tous les nœuds ne présentaient que des écarts de performances mineurs, typiques des différents processeurs, même s’ils appartiennent tous au même modèle.
| Cinebench R23 | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| Processeur multicœur | 74,877 | 74,961 | 75,011 | 74,745 | 74,898.5 | 
| Processeur monocœur | 1,088 | 1,093 | 1,084 | 1,088 | 1,088.25 | 
| Rapport PM | 64.84 | 68.60 | 69.17 | 68.70 | 67.83 | 
Cinebench 2024
Cinebench 2024 de Maxon est une référence de rendu CPU et GPU qui utilise tous les cœurs et threads du processeur. Nous l'avons exécuté pour des tests multicœurs et monocœurs. Étant donné que ces nœuds n'ont pas de GPU, nous n'avons que les numéros multicœurs et monocœurs.
Dans Cinebench 2024, tous les nœuds sont restés proches les uns des autres, avec une variance minime sur les parties multicœur et monocœur. Les performances moyennes étaient de 4,509 67.25 points pour le multicœur et de 66.98 points pour le monocœur, avec un ratio MP de XNUMX.
| Cinebench 2024 | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| Processeur multicœur | 4,544 | 4,577 | 4,436 | 4,481 | 4,509.5 | 
| Processeur monocœur | 68 | 68 | 65 | 68 | 67.25 | 
| Rapport PM | 66.79 | 67.23 | 68.21 | 65.69 | 66.98 | 
Processeur Geekbench 6
Geekbench 6 est un benchmark multiplateforme qui mesure les performances globales d'un système. Ce test comprend une partie dédiée au processeur et une autre au processeur graphique, mais comme ces nœuds ne possèdent pas de GPU, nous ne disposons que des résultats du processeur. Plus le score est élevé, meilleures sont les performances.
Dans Geekbench, nous avons vu des chiffres serrés jusqu'à ce que nous arrivions au nœud 3, qui s'est légèrement replié sur les monocœurs et les multicœurs. La moyenne entre tous les nœuds était de 1,687 19,319.5 en monocœur et de XNUMX XNUMX en multicœur.
| Processeur Geekbench 6 | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| Single-Core | 1,707 | 1,708 | 1,625 | 1,708 | 1,687 | 
| Multi-Core | 19,544 | 19,234 | 18,999 | 19,501 | 19,319.5 | 
Processeur Blender 4.0
La prochaine étape est Blender OptiX, une application de modélisation 3D open source. Ce benchmark a été exécuté à l'aide de l'utilitaire CLI Blender Benchmark. Le score est exprimé en échantillons par minute, le plus élevé étant le meilleur.
Les nœuds C6615 ont enregistré des chiffres assez cohérents. Les scores moyens étaient de 591.79 sur Monster, 415.88 sur Junkshop et 311.74 sur Classroom.
| Processeur Blender 4.0 | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| Monster | 595.23 | 593.51 | 584.35 | 594.07 | 591.79 | 
| Brocanteur | 415.26 | 415.11 | 418.05 | 415.08 | 415.88 | 
| Salle de classe | 308.57 | 312.91 | 312.69 | 312.78 | 311.74 | 
Processeur Blender 4.1
Blender OptiX 4.1 apporte de nouvelles fonctionnalités, telles que le débruitage accéléré par GPU, la rationalisation du processus de rendu et la réduction du temps nécessaire aux tâches de débruitage. Malgré ces progrès, les améliorations globales des performances dans les scores de référence par rapport à la version 4.0 sont minimes, indiquant seulement de légères améliorations en termes d'efficacité.
Encore une fois, nous constatons des chiffres cohérents dans tous les domaines, avec des moyennes de 587.22 sur Monster, 420.20 sur Junkshop et 306.60 sur Classroom.
| Processeur Blender 4.1 | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| Monster | 590.46 | 590.58 | 584.76 | 583.08 | 587.22 | 
| Brocanteur | 418.38 | 416.71 | 426.73 | 419.03 | 420.20 | 
| Salle de classe | 306.86 | 304.81 | 308.95 | 305.79 | 306.60 | 
Compression à 7 zips
L'utilitaire populaire 7-Zip dispose d'un test de mémoire intégré qui démontre les performances du processeur. Dans ce test, nous l'exécutons sur une taille de dictionnaire de 128 Mo lorsque cela est possible.
Des scores équitables ont été observés dans tous les nœuds. Dans les scores totaux, nous avons constaté une utilisation totale du processeur de 5,778.75 4.355 %, une note/utilisation totale de 252 GIPS et une note totale de XNUMX GIPS.
| Processeur Blender 4.1 | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| Compression |  |  |  |  |  | 
| Utilisation actuelle du processeur | 5,548 % | 5,549 % | 5,633 % | 5,585 % | 5,578.75 % | 
| Note actuelle/utilisation | 4.256 GIPS | 4.210 GIPS | 4.156 GIPS | 4.177 GIPS | 4.20 GIPS | 
| Courant | 236.158 GIPS | 233.626 GIPS | 234.092 GIPS | 233.285 GIPS | 234.290 GIPS | 
| Utilisation résultante du processeur | 5,536 % | 5,537 % | 5,601 % | 5,553 % | 5,556.75 % | 
| Évaluation/utilisation résultante | 4.193 GIPS | 4.202 GIPS | 4.172 GIPS | 4.168 GIPS | 4.184 GIPS | 
| Note résultante | 232.118 GIPS | 232.631 GIPS | 233.691 GIPS | 231.443 GIPS | 232.470 GIPS | 
| Décompression |  |  |  |  |  | 
| Utilisation actuelle du processeur | 5,973 % | 6,027 % | 5,992 % | 6,014 % | 6,001.5 % | 
| Note actuelle/utilisation | 4.543 GIPS | 4.501 GIPS | 4.565 GIPS | 4.509 GIPS | 4.530 GIPS | 
| Courant | 271.343 GIPS | 271.287 GIPS | 273.507 GIPS | 271.196 GIPS | 271.833 GIPS | 
| Utilisation résultante du processeur | 5,997 % | 6,015 % | 5,999 % | 5,990 % | 6,000.25 % | 
| Évaluation/utilisation résultante | 4.537 GIPS | 4.519 GIPS | 4.550 GIPS | 4.499 GIPS | 4.526 GIPS | 
| Note résultante | 272.066 GIPS | 271.775 GIPS | 272.946 GIPS | 269.509 GIPS | 271.574 GIPS | 
| Note totale |  |  |  |  |  | 
| Utilisation totale du processeur | 5,767 % | 5,776 % | 5,800 % | 5,772 % | 5,778.75 % | 
| Note totale/utilisation | 4.365 GIPS | 4.360 GIPS | 4.361 GIPS | 4.333 GIPS | 4.355 GIPS | 
| Note totale | 252.092 GIPS | 252.203 GIPS | 253.318 GIPS | 250.476 GIPS | 252.022 GIPS | 
Test de vitesse brute Blackmagic
Nous utilisons le Raw Speed Test de Blackmagic pour évaluer la façon dont les machines effectuent le décodage RAW réel. Ce test peut intégrer à la fois l’utilisation du CPU et du GPU, mais nous testerons uniquement l’utilisation du CPU.
Les quatre nœuds ont présenté des performances extrêmement proches, avec une moyenne de 119.75 FPS.
| Test de vitesse brute Blackmagic | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| CPU 8K | FPS 121 | FPS 121 | FPS 118 | FPS 119 | FPS 119.75 | 
Test de vitesse du disque Blackmagic
Vient ensuite le test de vitesse du disque Blackmagic. Ce test exécute un exemple de fichier de 5 Go pour les vitesses de lecture et d'écriture. Comme il est monothread, il n'affichera pas les vitesses les plus élevées du disque, mais il donne quand même une bonne perspective.
Les C6615 ont une carte BOSS à l'intérieur, utilisant deux disques M.2 en RAID1, de sorte que les performances sont légèrement dégradées pour la fiabilité. Pour les vitesses d'écriture, nous avons constaté une moyenne de 991.6 Mo/s et pour les vitesses de lecture, une moyenne de 2,801 XNUMX Mo/s.
| Test de vitesse du disque Blackmagic | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| Écrire | 999.8 Mo / s | 977.4 Mo / s | 991.4 Mo / s | 997.7 Mo / s | 991.6 Mo / s | 
| Lire | 2,807.4 Mo / s | 2,790.1 Mo / s | 2,828.0 Mo / s | 2,780.4 Mo / s | 2,801.5 Mo / s | 
Y-Cruncher
y-cruncher est un programme multi-thread et évolutif qui peut calculer Pi et d'autres constantes mathématiques jusqu'à des billions de chiffres. Depuis son lancement en 2009, il est devenu une application populaire d'analyse comparative et de test de résistance pour les overclockeurs et les passionnés de matériel.
Pour nos vitesses moyennes, nous avons vu 9.5 secondes pour 1 milliard, 24.20 secondes pour 2.5 milliards et 50.73 secondes pour 5 milliards. Sur les calculs des chiffres les plus significatifs, nous avons vu 105.73 secondes pour 10 milliards, 288.85 secondes pour 25 milliards et 633.5 secondes pour 50 milliards.
| Y Cruncher (temps de calcul total, en secondes) | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| 1 milliard | 9.587 | 9.459 | 9.350 | 9.633 | 9.507 | 
| 2.5 milliard | 24.490 | 24.225 | 23.334 | 24.740 | 24.197 | 
| 5 milliard | 51.427 | 50.990 | 49.303 | 51.214 | 50.734 | 
| 10 milliard | 107.084 | 107.646 | 103.772 | 107.443 | 105.736 | 
| 25 milliard | 291.918 | 290.944 | 280.632 | 291.902 | 288.849 | 
| 50 milliard | 641.709 | 640.289 | 619.100 | 640.917 | 635.504 | 
Benchmark de vision par ordinateur UL Procyon AI
UL Procyon AI Inference est conçu pour évaluer les performances d'une station de travail dans des applications professionnelles. Il est important de noter que ce test n'exploite pas les capacités de plusieurs processeurs. Plus précisément, cet outil évalue la capacité de la station de travail à gérer les tâches et les flux de travail basés sur l'IA, fournissant une analyse détaillée de son efficacité et de sa rapidité de traitement des algorithmes et applications d'IA complexes.
Pour ce test, nous utilisons Procyon V2.7.0. Dans ce test, des temps inférieurs sont meilleurs. Sur l’ensemble des nœuds, les moyennes étaient de 3.91 ms sur MobileNet V3, de 8.4.0 ms pour Resnet50 et de 29.47 ms. Sur le reste des scores, nous avons vu 30.96 ms sur DeepLab V3, 44.68 ms sur YOLO V3 et 2008.65 ms sur Real-ESRGAN. Pour le score global, les nœuds étaient en moyenne de 133.5.
| Vision par ordinateur UL Procyon (Temps d'inférence moyen) | Nœud 1 | Nœud 2 | Nœud 3 | Nœud 4 | Normale | 
|---|---|---|---|---|---|
| Mobile Net V3 | 3.87 ms | 3.94 ms | 3.84 ms | 4.00 ms | 3.91 ms | 
| ResNet50 | 8.47 ms | 8.45 ms | 8.23 ms | 8.46 ms | 8.40 ms | 
| Création V4 | 29.76 ms | 29.55 ms | 28.74 ms | 29.84 ms | 29.47 ms | 
| Deep Lab V3 | 30.39 ms | 30.21 ms | 33.18 ms | 30.07 ms | 30.96 ms | 
| YOLO V3 | 44.71 ms | 44.58 ms | 44.79 ms | 44.63 ms | 44.68 ms | 
| Réel-ESRGAN | 2003.18 ms | 1971.97 ms | 2018.26 ms | 2041.18 ms | 2008.65 ms | 
| Note globale | 134 | 134 | 133 | 133 | 133.5 | 
Conclusion
Les nœuds Dell PowerEdge C6615 offrent un seul processeur AMD EPYC avec jusqu'à 64 cœurs et six emplacements DDR5 prenant en charge des DIMM de 96 Go. Le châssis C6600 qui héberge ces nœuds propose quelques configurations de stockage. Notre système d’évaluation dispose du fond de panier SSD 8x E3.S Gen5. Dans la conception C6600, chaque nœud a accès à deux de ces SSD ; le châssis fournit simplement l'alimentation et un accès câblé direct aux disques. Pour la gestion, chaque C6615 propose iDRAC ; le châssis n'a pas de gestion dédiée.
Nous avons évalué de manière indépendante les capacités de chaque nœud C6615 lors de nos tests de performances et avons calculé la moyenne des scores des quatre nœuds pour identifier les anomalies de performances. Les données de performances mettent en évidence que les nœuds fonctionnent de manière cohérente, sans valeurs aberrantes ni performances inégales. Cette prévisibilité est essentielle pour les fournisseurs de services et les clients hyperscale qui peuvent bénéficier de systèmes denses comme celui-ci.
Nous avons trouvé le système bien conçu pour le cas d'utilisation prévu ; notre seul reproche concerne la prise en charge relativement limitée des SSD Gen5 : seulement deux disques par nœud. Dell suggérerait probablement que les clients à forte densité de calcul n'ont pas besoin d'autant de stockage local et que le refroidissement de davantage de disques Gen5 constitue un défi technique sérieux, et ils ont probablement raison, nous préférons simplement plus de disques que moins à presque chaque occasion. Une autre remarque qui mérite d'être mentionnée, nous examinons le C6615 ici, mais comme indiqué en haut de cette revue, Dell propose des types de nœuds supplémentaires pour cette plate-forme, le C6620 basé sur Intel est disponible dans une version refroidie par liquide, que certains peut trouver convaincant.
Les nœuds de calcul Dell PowerEdge C6615 offrent aux fournisseurs de services une étonnante combinaison de performances par rack U. Nous avons déjà vu de nombreuses configurations 2U4N, mais cette conception permet une plus grande largeur, et donc une plus grande flexibilité d'extension, dans chaque serveur que de nombreux systèmes concurrents. Associez l'excellente conception à des logiciels de gestion comme iDRAC et OpenManage Enterprise et nous sommes de grands fans du résultat final.
Page produit Dell PowerEdge C6615