La famille EPYC 4005 d'AMD intègre Zen 5 à l'AM5 avec un objectif opérationnel clair. Maintenir la cohérence de la plateforme pour les constructeurs et les hébergeurs de systèmes, puis optimiser les éléments essentiels à l'utilisation quotidienne. Le résultat est une approche de l'AM5 axée sur le serveur, qui privilégie des déploiements prévisibles, un firmware stable et une matrice de système d'exploitation conçue pour l'évolutivité.
Dans la gamme AMD, l'EPYC 4005 se situe entre Ryzen sur AM5 et les plateformes EPYC haut de gamme sur SP5 et SP6. Ryzen peut ancrer des rôles de serveur léger, mais il n'offre pas la profondeur de validation, le cycle de vie ni l'ensemble de fonctionnalités attendus par les opérateurs de centres de données. Les gros sockets offrent davantage de cœurs, de canaux mémoire et de voies, mais un coût bien plus élevé. L'EPYC 4005 comble cet écart. Vous conservez le côté économique et la simplicité de l'AM5 tout en bénéficiant d'un cadre d'entreprise compatible avec les licences et les enveloppes de puissance courantes.
La gamme de produits reflète cette intention. La couverture s'étend dans les deux sens, permettant aux acheteurs de standardiser avec un budget énergétique modeste ou d'opter pour un niveau de cache supérieur important pour les services sensibles à la latence, le tout sans changer de châssis ni de forfait d'E/S.
AMD EPYC 4004 vs 4005 : les améliorations pratiques
L'EPYC 4005 est une évolution générationnelle simple par rapport à la 4004, qui conserve l'héritage AM5 tout en améliorant la plateforme. Avec la 4005, vous bénéficiez de cœurs Zen 5, d'une augmentation des performances de la RAM jusqu'à la DDR5-5600 et de la même capacité PCIe Gen28 jusqu'à 5 voies. AMD ajoute également l'AVX-512 avec un chemin de données complet de 512 bits, ce qui offre à cette catégorie de serveurs monosocket une marge de manœuvre accrue pour les tâches vectorielles intensives sans changer de carte mère ni de châssis.
Au sommet de la pile, la comparaison est simple. L'EPYC 4565P du 4005 est un processeur 16 cœurs avec 64 Mo de mémoire L3, un TDP de 170 W et une fréquence d'horloge allant jusqu'à 5.7 GHz. Son homologue du 4004, l'EPYC 4564P, est également un processeur 16 cœurs avec 64 Mo de mémoire L3 et un TDP de 170 W, avec la même augmentation maximale de 5.7 GHz. Cela signifie que les gains générationnels proviennent des mises à jour du cœur et de la plateforme plutôt que d'un saut significatif des spécifications.
Si vous souhaitez travailler avec du cache, les deux familles proposent une option V-Cache 3D. Cependant, le 4005 place le processeur EPYC 4585PX au sommet avec 170 W et 128 Mo de mémoire L3, tandis que le 4004 plafonne avec l'EPYC 4584PX à 120 W et 128 Mo de mémoire. Les fréquences de base du 4585PX sont également légèrement supérieures.
L'ajout pratique le plus significatif du 4005 est une référence 16 cœurs et 65 W. L'EPYC 4545P permet aux serveurs hébergés bare metal et PME de standardiser 16 cœurs pour les licences Windows tout en respectant des enveloppes de puissance et de refroidissement plus strictes. Cette option n'existait pas dans le 4004, où 16 cœurs signifiaient 170 W sur la puce standard ou un composant V-Cache de 120 W avec des fréquences de base plus basses.
Comme indiqué précédemment, la prise en charge de la mémoire a été légèrement améliorée. Le 4004 a validé deux canaux de DDR5 jusqu'à 5200 4005 MT/s. Le 5600 porte cette vitesse à 192 128 MT/s et augmente la capacité maximale prise en charge au niveau de la plateforme de XNUMX Go à XNUMX Go, ce qui facilite les charges virtualisées mixtes et les petites instances de bases de données.
Comparaison des processeurs EPYC 4004 et 4005
| Série AMD EPYC 4004 Modèles | Arch. | Noyaux / Threads | Cache L3 (Mo) | TDP (W) | Horloge de base (GHz) | Boost horloge (GHz) | Prix (1KU, USD) | 
|---|---|---|---|---|---|---|---|
| EPYC 4124P | Zen 4 | 4/8 | 16 | 65W | 3.8 | 5.1 | $149 | 
| EPYC 4244P | Zen 4 | 6/12 | 32 | 65W | 3.8 | 5.1 | $229 | 
| EPYC 4344P | Zen 4 | 8/16 | 32 | 65W | 3.8 | 5.3 | $329 | 
| EPYC 4364P | Zen 4 | 8/16 | 32 | 105W | 4.5 | 5.4 | $399 | 
| EPYC 4464P | Zen 4 | 12/24 | 64 | 65W | 3.7 | 5.4 | $429 | 
| EPYC4484PX | Zen 4 | 12/24 | 128 | 120W | 4.4 | 5.6 | $599 | 
| EPYC 4564P | Zen 4 | 16/32 | 64 | 170W | 4.5 | 5.7 | $699 | 
| EPYC4584PX | Zen 4 | 16/32 | 128 | 120W | 4.2 | 5.7 | $699 | 
| Modèles de la série AMD EPYC 4005 |  |  |  |  |  |  |  | 
| EPYC 4245P | Zen 5 | 6/12 | 32 | 65W | 3.9 | 5.4 | $239 | 
| EPYC 4345P | Zen 5 | 8/16 | 32 | 65W | 3.8 | 5.5 | $329 | 
| EPYC 4465P | Zen 5 | 12/24 | 64 | 65W | 3.4 | 5.4 | $399 | 
| EPYC 4545P | Zen 5 | 16/32 | 64 | 65W | 3.0 | 5.4 | $549 | 
| EPYC 4565P | Zen 5 | 16/32 | 64 | 170W | 4.3 | 5.7 | $589 | 
| EPYC4585PX | Zen 5 | 16/32 | 128 | 170W | 4.3 | 5.7 | $699 | 
Plateforme de test : MSI S1102-02 avec refroidissement liquide
Pour ce test, nous avons utilisé la carte mère MSI S1102-02, un barebone 1U AM5 parfaitement adapté au marché cible des processeurs EPYC 4005. Elle prend en charge les processeurs AM5 jusqu'à 170 W et notre modèle est livré avec un système de refroidissement liquide tout-en-un en option, une caractéristique rare dans cette catégorie et idéale pour les processeurs 4005 haut de gamme. Ce système devrait notamment permettre de réduire la vitesse et le niveau sonore des ventilateurs. Côté refroidissement, le boîtier intègre sept ventilateurs système de 40 mm, une alimentation Platinum 1+1 de 600 W et une gestion complète via un contrôleur BMC AST2600 compatible IPMI et Redfish.
La mise en réseau s'effectue via deux ports RJ10 45 GbE sur le processeur Intel X710. Le stockage est simple avec quatre baies SATA remplaçables à chaud en façade, deux baies SATA internes 2.5 pouces et deux emplacements NVMe M.2 2280/22110. L'extension comprend un emplacement Gen5 x16 pour cartes FHHL et un chemin Gen4 x4 partagé pouvant être acheminé vers un emplacement PCIe secondaire ou vers l'un des sockets M.2. La mémoire est composée de quatre emplacements DDR5 UDIMM avec prise en charge ECC, validés jusqu'à 5600 1 MT/s à 48 DPC et 192 Go par DIMM pour une capacité maximale de 26 Go. La profondeur physique est de XNUMX pouces, ce qui offre une grande flexibilité de déploiement dans les racks courts.
Spécifications complètes du MSI S1102-02
| Spécifications | DÉTAILS | 
| Facteur de forme | 1U | 
| Dimensions | 438.5 mm (17.26″) L x 43.5 mm (1.71″) H x 660 mm (26″) P | 
| Processeur | Processeur AMD Ryzen™ série 7000/9000 et EPYC™ série 4004/4005, jusqu'à TDP 170 W | 
| Douille | (1) Prise AMD AM5 | 
| Chipset | AMD B650 | 
| Mémoire | (4) emplacements DIMM DDR5, 2DPC, UDIMM ECC/non-ECC – Max. Fréquence 5600MT/s(1DPC) et 3600MT/s(2DPC) – Max. Capacité par DIMM : 48 Go | 
| Baies de disques | (4) Les baies de disque 3.5"/2.5" remplaçables à chaud prennent en charge SATA 3.0 | 
| Stockage interne | (2) ports M.2 2280/22110 PCIe4.0 x4 (2) Les baies de disque internes 2.5″ prennent en charge SATA 2.0 | 
| Slots d'extension | (1) L'emplacement PCIe 4.0 x16 du processeur prend en charge la carte PCIe FHHL | 
| Networking | (2) ports Ethernet 10GBase-T (Intel® X710AT2) *JLAN1,2 prend en charge NCSI | 
| RAID | N/D | 
| Front I / O | (4) Baies de lecteur 3.5" remplaçables à chaud (2) Ports USB3.2 Gen1 Type-A (1) Bouton d'alimentation du système (1) Bouton UID (1) LED UID (5) LED d'état : Alimentation/Défaut/HDD/(2)LAN | 
| I / O arrière | (2) ports Ethernet 10GBase-T (1) Port de gestion de serveur dédié 1000Base-T (4) Ports USB3.2 Gen1 Type-A (1) port VGA D-Sub (1) Port COM DB9 (1) Bouton LED UID | 
| TPM | (1) En-tête TPM avec interface SPI | 
| Sécurité | TPM 2.0 | 
| Gestion de serveur | (1) Port de gestion de serveur dédié 1000Base-T (Realtek RTL8211FD-CG) ASPEED AST2600 avec micrologiciel basé sur AMI MegaRAC prenant en charge IPMI 2.0 et API DMTF Redfish | 
| Environnement | Température de fonctionnement du système : 0°C ~ 35°C Température hors fonctionnement : -20°C ~ 70°C Humidité relative hors fonctionnement : 5 % ~ 85 % (sans condensation) | 
| Alimentation | (1+1) Bloc d'alimentation redondant 450 W 80+ Platinum | 
| Refroidissement | (1) Module de refroidissement liquide en boucle fermée pour processeur max. 170 W (7) 4028 ventilateurs système | 
| Certifications | CE, FCC (Classe A) | 
| Contenu de l'emballage | (1) Barebone 1U S1102-02 (1) Guide rapide (1) Kit de glissières (1) Module de refroidissement liquide en boucle fermée (assemblé à l'intérieur du châssis) (4) Câbles d'alimentation | 
| Liste des pieces | (1) Guide rapide : G52-S3362X2-Q13 (1) Kit de glissières : E21-S336020-C27 (1) Module de refroidissement liquide en boucle fermée : E34-F000091-AQ0 (4) Câbles d'alimentation : (2)K33-3001002-I45 pour l'Amérique du Nord, (2)K33-3001005-I45 pour l'UE | 
| Pièces en option | Module TPM2.0 : TPM20-IR Cordon d'alimentation C13 250 V/10 A (CN) : K33-3001029-I45 Cordon d'alimentation C13 125 V/13 A (JP) : K33-3001346-I45 | 
Performances de l'AMD EPYC 4005
Dans ce test, nous comparerons le processeur AMD EPYC 4564P à 16 cœurs avec 64 Mo de cache (génération 4004) au processeur AMD EPYC 4585PX à 16 cœurs avec 128 Mo de cache (génération 4005). Les tests ont été effectués sur le serveur MSI S1102-2, comme indiqué précédemment. Voici les composants du serveur de test MSI S1102-02 :
- AMD EPYC 4585PX ou AMD EPYC 4564P
- GPU NVIDIA L4
- 4 disques durs Seagate Exos M 30 To
- SSD de démarrage Solidigm P44 Pro M.2
- 4 x Samsung 16 Go DDR5-4800 ECC UDIMM
Y-Cruncher
y-cruncher est un programme multithread et évolutif capable de calculer Pi et d'autres constantes mathématiques jusqu'à des milliers de milliards de chiffres. Depuis son lancement en 2009, il est devenu une application de benchmarking et de test de résistance populaire auprès des overclockeurs et des passionnés de matériel informatique.
Lors des tests standard (1B à 10B), l'EPYC 4585PX est systématiquement plus rapide que l'EPYC 4564P, réduisant le temps de calcul de 4.5 à 10.2 %. Par exemple, lors du test à 1 milliard de chiffres, il affiche 18.802 s contre 20.933 s pour le 4564P.
Le test BBP montre des gains bien plus importants, jusqu'à 40 %, ce qui souligne la sensibilité de ces charges de travail à la latence mémoire et à la capacité L3. Le cas d'un milliard de BBP le montre clairement : 1 s (0.387 4585 PX) contre 0.630 s (4564 38.6 P), soit une réduction de XNUMX %. Des avantages similaires sont observés pour des tailles de BBP plus élevées.
Ce modèle s'aligne sur le cache L4585 plus grand du 3PX (128 Mo contre 64 Mo sur le 4564P) et sur les améliorations architecturales
| y-cruncher Temps de calcul total (Plus bas, c'est mieux) | AMD EPYC 4585PX 16 cœurs | AMD EPYC 4564P 16 cœurs | 
| 1 milliard | 18.802 secondes | 20.933 secondes | 
| 2.5 milliard | 54.085 secondes | 58.108 secondes | 
| 5 milliard | 121.711 secondes | 128.428 secondes | 
| 10 milliard | 269.096 secondes | 281.677 secondes | 
| 1 milliard de BBP | 0.387 secondes | 0.630 secondes | 
| 10 milliard de BBP | 4.450 secondes | 7.367 secondes | 
| 100 milliard de BBP | 50.311 secondes | 84.051 secondes | 
Mixeur 4.0
Blender 4.0 est une application de modélisation 3D open source. Ce benchmark a été réalisé à l'aide de l'utilitaire Blender Benchmark CLI. Le score est mesuré en échantillons par minute, les valeurs les plus élevées étant les meilleures.
Bien que les deux processeurs aient obtenu de bons résultats pour leur catégorie, le cache accru de l'EPYC 4585PX lui permet d'être encore plus rapide. Par exemple, lors du test Monster, le 4585PX a atteint 351.90 échantillons par minute, tandis que le 4564P a obtenu 306.57 échantillons par minute. Dans Junkshop, les scores étaient de 226.59 pour le 4585PX et de 195.26 pour le 4564P. De même, toujours dans Classroom, avec respectivement 171.87 et 150.27.
| Échantillons CPU par minute de Blender 4.0 (plus c'est élevé, mieux c'est) | AMD EPYC 4585PX 16 cœurs | AMD EPYC 4564P 16 cœurs | 
| Monster | 351.90 | 306.57 | 
| Brocanteur | 226.59 | 195.26 | 
| Salle de classe | 171.87 | 150.27 | 
Points de repère Phoronix
Nous avons utilisé la suite de tests Phoronix pour automatiser les installations, exécuter les charges de travail et collecter les résultats sur cinq plateformes clés : STREAM, 7-Zip, la compilation du noyau Linux, le serveur HTTP Apache et OpenSSL. Ci-dessous, l'EPYC 4585PX est comparé directement à l'EPYC 4564P de génération précédente (tous deux à 16 cœurs).
Bande passante mémoire STREAM : le 4585PX accuse un léger retard, avec 38,472 40,106.9 Mo/s contre 4.1 1,634.9 Mo/s (−4564 %, −XNUMX XNUMX Mo/s). STREAM est très sensible aux horloges/topologies mémoire et aux choix du compilateur ; cela indique une faible marge de bande passante pour le XNUMXP dans notre configuration.
7-Zip (compression + décompression) : le 4585PX délivre 162,951 176,484 MIPS contre 7.7 13,533 MIPS (−7 %, −4564 XNUMX MIPS). XNUMX-Zip s'appuie sur le débit entier et le comportement du cache/mémoire ; ici, le XNUMXP est légèrement en avance.
Compilation du noyau Linux (allmodconfig) : 4585PX s'exécute en 621.967 s contre 747.610 s (-125.643 s, soit 16.8 % de plus). Ce temps réduit reflète un meilleur débit de construction parallèle grâce aux améliorations architecturales.
Requêtes Apache/sec : 4585PX sert 181,764.45 132,744.77 R/s contre 36.9 49,019.68 R/s (+XNUMX %, +XNUMX XNUMX R/s), indiquant un avantage considérable dans le débit HTTP.
Vérification OpenSSL : le 4585PX atteint 400,939,420,057 224,487,686,510 78.6 176.45 vérifications/s contre 4585 3 XNUMX XNUMX vérifications/s (+XNUMX %, +XNUMX milliards de vérifications/s). Les charges de travail cryptographiques bénéficient des gains architecturaux du XNUMXPX et de son LXNUMX plus étendu.
Globalement : le 4585PX affiche des avances substantielles dans les tests de construction, Web et crypto (gains de 17 à 79 %), tandis que le 4564P se démarque sur la bande passante mémoire brute et 7-Zip dans notre configuration.
| Points de repère Phoronix | AMD EPYC 4585PX 16 cœurs | AMD EPYC 4564P 16 cœurs | 
| Discussions | 38,472.0 Mo / s | 40,106.9 Mo / s | 
| 7-ZIP | 162,951 XNUMX MIP/s | 176,484 XNUMX MIP/s | 
| Compilation du noyau (allmod) | 621.967 XNUMX secondes | 747.610 XNUMX secondes | 
| Apache (requêtes par seconde) | 181,764.45 XNUMX R/s | 132,744.77 XNUMX R/s | 
| OpenSSL | 400,939,420,057 XNUMX XNUMX XNUMX Vérifications | 224,487,686,510 XNUMX XNUMX XNUMX Vérifications | 
Performances FIO du disque dur
Le serveur MSI S1102-02 prend en charge quatre disques durs 3.5 pouces en façade, offrant ainsi plusieurs options de stockage. Bien que le stockage le plus rapide soit assuré par les emplacements PCIe 4.0 M.2 intégrés, il est possible d'installer des disques durs jusqu'à 30 To en façade. En utilisant des disques durs SATA Seagate Exos M de 30 To , nous avons mesuré les performances séquentielles et aléatoires avec FIO. Nous avons mesuré une bande passante séquentielle maximale de 1.3 Go/s en lecture et 1.1 Go/s en écriture, avec des IOPS aléatoires 4K de 1 720 IOPS en lecture et 1 990 IOPS en écriture. Ces résultats ont été obtenus en configuration JBOD ; le RAID influencera donc les performances finales.
| Charge de travail | Bande passante la plus élevée | IOPS les plus élevées | Latence la plus faible | 
|---|---|---|---|
| Écriture aléatoire (4K) | 7.8 Mo / s | 1,990 | 2.106 ms | 
| Lecture aléatoire (4K) | 6.7 Mo / s | 1,720 | 6.861 ms | 
| Écriture séquentielle (128 Ko) | 1,102.5 Mo / s | 8,820 | 0.486 ms | 
| Lecture séquentielle (128 Ko) | 1,322.3 Mo / s | 10,578 | 0.459 ms | 
Conclusion
L'EPYC 4005 remplit sa mission. Il simplifie l'AM5 pour les constructeurs de systèmes et les hébergeurs, tout en offrant une marge de manœuvre précieuse grâce à Zen 5, à la DDR5-5600 validée, à l'AVX-512 et à une couverture SKU plus large. Cette famille se positionne parfaitement entre Ryzen sur AM5 et les plateformes SP5 et SP6 plus imposantes, offrant aux fournisseurs de services une solution d'entreprise sans le coût des gros sockets. La carte SKU couvre les modèles de déploiement courants. L'EPYC 4545P propose un processeur 16 cœurs à 65 W pour un hébergement dense et des licences Windows Server propres. L'EPYC 4565P reste la solution standard à 170 W, tandis que l'EPYC 4585PX ajoute le V-Cache 3D pour les services sensibles à la latence qui s'appuient sur un cache de dernier niveau plus important. Une validation mémoire jusqu'à 192 Go sur quatre UDIMM et des E/S stables avec jusqu'à 28 voies PCIe Gen5 complètent la plateforme.
Nos données montrent où cela compte. Le 16PX à 4585 cœurs remporte des victoires nettes dans les tâches sensibles au calcul et au cache. y-cruncher exécute des tâches 4.5 à 10.2 % plus rapidement sur 1 à 10 milliards de données, tandis que les exécutions BBP réduisent le temps jusqu'à 38.6 %. Blender en bénéficie également, avec des gains importants sur Monster, Junkshop et Classroom. Dans la suite Phoronix, le 4585PX réduit le temps de compilation du noyau Linux de 16.8 %, traite 36.9 % de requêtes Apache en plus par seconde et enregistre une augmentation de 78.6 % de la vérification OpenSSL. STREAM et 7-Zip se rapprochent légèrement du précédent 4564P dans notre configuration.
L'EPYC 4005 est une mise à jour modérée mais significative d'AMD. Elle préserve l'aspect économique et la simplicité qui ont facilité l'adoption de la version 4004, tout en améliorant les performances et la flexibilité pratiques dans un large éventail de cas d'utilisation hébergés et en périphérie. Si vous développez des serveurs AM5 monosocket à grande échelle, cette solution par défaut est idéale pour les nouveaux déploiements et constitue une mise à niveau judicieuse lorsque le cache ou l'efficacité sont des enjeux, notamment lorsqu'elle est associée au serveur MSI S1012-02 doté de la boucle liquide interne.