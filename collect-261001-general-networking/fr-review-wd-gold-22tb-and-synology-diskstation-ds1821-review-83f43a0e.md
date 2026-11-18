En août dernier, nous avons testé le disque dur WD Gold 22 To CMR , notamment grâce à sa technologie ArmorCache compatible OptiNAND. Nous avions précédemment testé les disques Gold sur notre Superserveur de stockage Supermicro 36 baies (SSG-540P-E1CTR36H) avec TrueNAS installé (RAID 2Z). Cependant, nous souhaitions évaluer les performances (et le rapport qualité-prix) des disques Gold 22 To dans un environnement de PME. Pour ce test, nous examinerons donc leurs performances au sein d'un Synology DiskStation DS1821+.
WD Gold 22 To Synology DS1821+
Alimenté par un processeur AMD Ryzen, le Synology DS1821+ est un NAS à 8 baies facile à utiliser, destiné davantage aux PME qui nécessitent un peu plus de performances et de capacité. Il dispose de deux emplacements M.2 pour le cache SSD NVMe ainsi que d'une connectivité 10GbE (bien que cela doive être ajouté via l'un de ses emplacements d'extension PCIe). Dans l'ensemble, le Synology DiskStation DS1821+ est très performant pour un NAS SMB.
La combinaison des disques WD Gold 22 To avec le DS1821+ crée un énorme référentiel de stockage potentiel pour les PME via un faible encombrement physique. Bien que cela puisse être un cas d'utilisation plus spécialisé (car la plupart des entreprises n'auront probablement pas cette quantité de stockage dans un NAS à 8 baies), les petits bureaux professionnels comme les architectes ou les sociétés de création de contenu peuvent commencer avec 4 ou 5 des disques WD 22 To. , les autres baies pouvant être étendues si nécessaire. Et oui, nous savons que les WD Red Pro seraient préférés ici, mais nous ne les avons pas dans le laboratoire.
La combinaison du Synology SMB NAS et des disques Gold de 22 To constituerait également une solution de sauvegarde interne idéale. Par exemple, une organisation peut utiliser la plupart des baies de lecteur comme pool de stockage principal, tout en laissant quelques-unes disponibles pour les sauvegardes en ligne.
Cependant, quel que soit le cas d'utilisation, les énormes disques WD Gold 22 To donneront aux organisations une grande flexibilité sur la façon dont elles gèrent leurs données avec des tonnes d'opportunités d'évolutivité.
Configuration et configuration
Comme d'habitude, nous avons utilisé l' application Gestionnaire de stockage DSM pour gérer le disque dur WD Gold DS1821+ déjà équipé. Elle simplifie considérablement la configuration RAID, les pools et les volumes, ainsi que la consultation de l'état du disque.
Cela dit, lors de la configuration des disques durs Gold, nous avons en fait rencontré une limitation du système de fichiers. Il existe une limite supérieure à la taille du volume, nous avons donc d'abord dû créer un volume d'une taille maximale de 103.5 To (comme vous pouvez le voir dans l'image ci-dessous).
Nous avons ensuite dû faire un deuxième volume pour utiliser le reste de l'espace de la piscine.
Spécifications du disque dur WD Gold 22 To
| Référence du modèle | WD221 CRISE | 
| Facteur de forme | 3.5 pouce | 
| Interface | SATA 6 Gb / s | 
| 512n / 512e secteurs utilisateur par lecteur4 | 512e | 
| Capacité formatée | 22TB | 
| Technologie OptiNAND | Oui | 
| conforme RoHS | Oui | 
| Performances |  | 
| Taux de transfert de données (max soutenu) | 291MB / s | 
| RPM | 7200 | 
| Cache | 512MB | 
| Gestion de l'énergie |  | 
| Besoins de puissance moyens (W) |  | 
| Efficacité | 7.1W | 
| Idle | 5.7W | 
| Indice d'efficacité énergétique (W/To, inactif) | 0.26 | 
| Fiabilité |  | 
| MTBF (heures, prévisionnel) | 2,500,000 | 
| Taux d'échec annualisé2 (AFR, %) | 0.35 | 
| Garantie limitée | 5 ans | 
| Environnemental |  | 
| Température de fonctionnement | 5 ° C à 60 ° C | 
| Température hors fonctionnement | -40 ° C à 70 ° C | 
| Choc (lecture/écriture)  En fonctionnement (demi-sinusoïdale, 2ms) | 40G / 40G | 
| Hors fonctionnement (demi-onde sinusoïdale, 2 ms) | 200G | 
| Acoustique (moyenne) |  | 
| Mode inactif | 20 dBA | 
| Mode de recherche | 32 dBA | 
| Dimensions physiques |  | 
| Hauteur (max) | 26.1mm | 
| Longueur (maximale) | 147.0mm | 
| Largeur (± 01 po) | 101.6mm | 
| Poids | 1.47 lb (67 kg)  ± 10% | 
Spécifications du Synology DiskStation DS1821+
| Processeur |  | 
| Mémoire |  | 
| Mémoire système | SODIMM DDR4 ECC de 4 Go | 
| Module de mémoire préinstallé | 4 GB (4 GB x 1) | 
| Nombre total d'emplacements de mémoire | 2 | 
| Capacité de mémoire maximale | 32 GB (16 GB x 2) | 
| Stockage |  | 
| Baies de disques | 8 | 
| Nombre maximal de baies de disques avec unité d'extension | 18 (DX517x2) | 
| Emplacements de lecteur M.2 | 2 (NVMe) | 
| Type de lecteur compatible |  | 
| Taille maximale d'un volume unique | 108 TB | 
| Disque remplaçable à chaud | Oui | 
| Ports externes |  | 
| Port LAN RJ-45 1GbE | 4 (avec prise en charge de l'agrégation de liens / basculement) | 
| Port USB 3.2 Gen 1 | 4 | 
| Port eSATA | 2 | 
| PCIe |  | 
| Extension PCIe | 1 emplacement Gen3 x8 (liaison x4) | 
| Système de fichiers |  | 
| Lecteurs internes |  | 
| Disques externes |  | 
| lustrée |  | 
| Taille (Hauteur x Largeur x Profondeur) | 166 mm x 343 mm x 243 mm | 
| Poids | 6 kg | 
| Autres |  | 
| Système Fan | 120 mm x 120 mm x 2 pièces | 
| Mode de vitesse du ventilateur |  | 
| Ventilateur du système de remplacement facile | Oui | 
| Indicateurs LED avant réglables en luminosité | Oui | 
| Récupération de puissance | Oui | 
| Niveau de bruit | 22.2 dB (A) | 
| Marche/Arrêt programmé | Oui | 
| Réveil sur LAN/WAN | Oui | 
| Bloc d'alimentation / Adaptateur | 250W | 
| Tension d'alimentation d'entrée CA | 100 V à 240 V CA | 
| Fréquence de puissance | 50/60 Hz, monophasé | 
| Consommation d'énergie | 59.8 W (accès)  26.18 W (hibernation du disque dur) | 
| Unité thermique britannique | 204.05 BTU/h (accès)  89.33 BTU/h (hibernation du disque dur) | 
| Environnement | Conforme RoHS | 
| Contenu de l'emballage |  | 
| Garantie | 3 ans | 
Performances du disque dur WD Gold 22 To
Analyse synthétique de la charge de travail d'entreprise
Comme indiqué précédemment, nous avons testé le disque dur WD Gold 22 To au sein du serveur Supermicro ; cependant, pour ces tests de performance, nous en avons installé huit dans notre Synology DiskStation DS1821+ configuré en RAID 6. Nous effectuerons des tests avec les protocoles iSCSI et SMB.
Notre processus de référence de disque dur d'entreprise préconditionne chaque ensemble de disques dans un état stable avec la même charge de travail avec laquelle l'appareil sera testé sous une lourde charge de 16 threads, avec une file d'attente exceptionnelle de 16 par thread. L'appareil est ensuite testé à des intervalles définis dans plusieurs profils de profondeur de thread/file d'attente pour montrer les performances en cas d'utilisation légère et intensive. Étant donné que les disques durs atteignent très rapidement leur niveau de performance nominal, nous ne représentons graphiquement que les principales sections de chaque test.
Tests de préconditionnement et d'état stable primaire :
- Débit (agrégat IOPS lecture + écriture)
- Latence moyenne (latence de lecture + écriture moyennée ensemble)
- Latence maximale (latence maximale de lecture ou d'écriture)
- Écart-type de latence (écart-type de lecture + écriture moyenné ensemble)
Notre analyse de charge de travail synthétique d'entreprise comprend quatre profils basés sur des tâches réelles. Ces profils ont été développés pour faciliter la comparaison avec nos références passées, ainsi qu'avec des valeurs largement publiées telles que la vitesse de lecture et d'écriture maximale de 4K et 8K 70/30, qui est couramment utilisée pour les disques d'entreprise.
4K
- 100 % de lecture ou 100 % d'écriture
- 100% 4K
8K70/30
- 70 % de lecture, 30 % d'écriture
- 100% 8K
128K (séquentiel)
- 100 % de lecture ou 100 % d'écriture
- 100% 128K
Notre premier test de débit mesure les performances aléatoires 4K à l'intérieur du Synology DS1821+. Ici, le WD Gold 22 To a affiché 328 IOPS en lecture et 1,477 5,204 IOPS en écriture (SMB) et 1,513 XNUMX IOPS en lecture et XNUMX XNUMX IOPS en écriture (iSCSI).
En latence moyenne, le WD Gold de 22 To a pu atteindre respectivement 778.4 ms et 174.4 ms en lecture et écriture en SMB, tandis que l'iSCSI affichait 49.2 ms en lecture et 169.1 ms en écriture.
Passant à la latence maximale, le WD Gold atteint 2,453.2 3,779.2 ms en lecture et 1,461.1 846.9 ms en écriture en SMB, tandis qu'il atteint XNUMX ms en lecture et XNUMX XNUMX ms en écriture en iSCSI.
Pour l'écart type, le WD Gold a enregistré 611.48 ms de lecture et 333.9 51.12 ms d'écriture en SMB, et 165.22 ms de lecture et XNUMX ms d'écriture en iSCSI.
Notre prochain benchmark soumet les disques à une activité de lecture et d'écriture de 100 % à un débit séquentiel de 8K. À l'intérieur du Synology DS1821 +, le disque WD Gold de 22 To a affiché 43,709 30,951 IOPS en lecture et 140,877 56,623 IOPS en écriture en SMB, tout en atteignant XNUMX XNUMX IOPS en lecture et XNUMX XNUMX IOPS en écriture en iSCSI.
Notre prochain test passe d'un scénario de lecture/écriture séquentielle 8K pure à 100 % à une charge de travail mixte 8K 70/30, qui démontrera comment les performances évoluent dans un environnement allant de 2T/2Q à 16T/16Q.
Lors de notre premier test, qui mesure le débit, le WD Gold 22 To avait une plage de 354 IOPS à 412 IOPS en SMB et de 526 IOPS à 1,700 XNUMX IOPS en iSCSI.
Avec une latence moyenne de 8K 70/30 en utilisant un Synology DS1821+, le 22 To Gold 22 To a affiché 11.26 ms à 619.27 ms en SMB, tandis que l'iSCSI a enregistré une plage de 7.57 ms à 150.39 ms.
Passant aux chiffres de latence maximale, le WD Gold de 22 To avait une plage de 710.87 ms à 4,883.6 1,309.31 ms tandis que l'iSCSI affichait 1,902.51 XNUMX ms à XNUMX XNUMX ms.
Pour nos résultats de latence d'écart type, le 22 To Gold a atteint 14.04 ms à 408.46 ms (SMB) et 16.83 ms à 245.49 ms (iSCSI).
Notre dernier test est le benchmark 128K, un test séquentiel à gros blocs montrant la vitesse de transfert séquentielle la plus élevée. Ici, le WD Gold 22 To affiche 2.3 Go/s en lecture et 1.33 Go/s en écriture en SMB, tandis que l'iSCSI affiche 1.88 Go/s en lecture et 615 Mo/s en écriture. Il convient de noter que le nombre de 2.3 Go/s est effectivement plafonné par l'interface du Synology.
Conclusion
Le disque WD Gold 22 To est conçu pour réduire le coût total de possession des entreprises, car il permet une plus grande capacité sans avoir à augmenter l'empreinte physique ou la même capacité à une empreinte physique inférieure. De plus, les Golds sont l'un des disques durs 3.5 pouces les plus performants que nous ayons vus dans notre laboratoire.
Bien que cela en fasse certainement un choix parfait pour les centres de données au niveau de l'entreprise, ils constituent également un excellent choix pour les PME qui ont besoin de stocker d'énormes quantités de données et pour ceux qui souhaitent pérenniser leur référentiel (en termes de capacité). Il était donc logique pour nous de voir ce qu'il est capable de produire en utilisant certains des principaux facteurs de forme NAS de bureau (ou SMB).
En tant que tel, nous avons rempli le Synology DS1821+ (un NAS SMB à 8 baies très performant) avec huit disques WD Gold 22 To et l'avons soumis à nos tests habituels. Dans l'ensemble, nous avons constaté d'excellents résultats de débit dans l'ensemble de nos analyses comparatives, en particulier lorsqu'elles sont configurées avec le protocole iSCSI.
Cela inclut 328 IOPS en lecture / 1,477 5,204 IOPS en écriture (SMB) et 1,513 4 IOPS en lecture / 43,709 30,951 IOPS en écriture (iSCSI) en 140,877K aléatoire, et 56,623 8 IOPS en lecture / 354 412 IOPS en écriture (SMB) et 526 1,700 IOPS en lecture / 2.3 1.33 IOPS en écriture (iSCSI) pendant la charge de travail séquentielle 1.88K. De plus, le WD Gold a pu enregistrer une plage de 615 IOPS à XNUMX IOPS en SMB et de XNUMX IOPS à XNUMX XNUMX IOPS en iSCSI. Pour notre test séquentiel à grands blocs, il a affiché XNUMX Go/s en lecture (plafonnant l'interface) et XNUMX Go/s en écriture en SMB, tandis que iSCSI a affiché XNUMX Go/s en lecture et XNUMX Mo/s en écriture.
Le WD Gold 22 To n'a pas déçu au niveau des PME, affichant des chiffres solides tout au long. Les bureaux professionnels comme les architectes et les créateurs de contenu trouveront que ces disques durs massifs apporteront beaucoup de flexibilité, de performances et de marge de croissance lorsqu'ils seront combinés avec une solution NAS comme le Synology DS1821+.