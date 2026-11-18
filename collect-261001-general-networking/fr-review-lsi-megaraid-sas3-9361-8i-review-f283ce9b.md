Le contrôleur RAID SAS3 LSI MegaRAID 9361-8i est une carte de stockage 12 Gb/s dotée d'une interface hôte PCIe 3.0 , de 1 Go de mémoire cache DDR3, d'un processeur double cœur PowerPC 476 cadencé à 1.2 GHz avec un ROC de 12 Gb/s et de 8 ports SATA+SAS 12 Gb/s. Avec un débit de transfert de données maximal deux fois supérieur à celui des solutions SAS 6 Gb/s, le 9361-8i offre une bande passante suffisante pour saturer pleinement le bus PCI Express Gen3. Outre ses débits de transfert de données exceptionnels, le 9361-8i assure une protection et une sécurité des données de niveau entreprise et prend en charge la protection du cache flash CacheVault. Les premières solutions SAS 12 Gb/s du marché de LSI, telles que le 9361-8i, sont conçues pour répondre aux exigences de performance et de sécurité des systèmes de stockage de données d'entreprise de nouvelle génération.
LSI et Adaptec se font une concurrence féroce dans l'espace SAS3 émergent, chacun poussant la barre plus haut à chaque version. Avec le 9361-8i, LSI propose un contrôleur SAS RAID qui a un taux de transfert de données élevé de 12 Gb/s, ainsi qu'une rétrocompatibilité pour 6 Gb/s et 3 Gb/s (permet jusqu'à 128 3 Gb/s, 6 Gb/s , ou périphériques SATA et SAS 12 Gb/s), prend en charge les niveaux RAID 0,1,5,6,10, 50, 60, XNUMX, XNUMX, XNUMX et XNUMX, et la protection des données d'entreprise via CacheVault qui permet de placer un disque en état de protection s'il existe un panne de disque, pour déterminer si le disque est réellement tombé en panne et peut être restauré.
Le 9361-81 fait partie de la plus grande famille de cartes RAID SAS LSI 12 Gb/s. Le 9361 est également disponible dans une configuration interne à quatre ports, 9361-4i. LSI propose également les cartes 9341-4i et 9341-8i qui sont des cartes internes à 4 et 8 ports qui prennent en charge moins de disques et n'ont pas de cache intégré. Le LSI MegaRAID 9361-8i SAS RAID est livré maintenant, est livré avec une garantie de 3 ans et a un prix public de 570.00 $.
Spécifications RAID SAS LSI MegaRAID 9361-8i :
- Numéro de pièce de commande : LSI00416
- Niveaux RAID : 0,1,5,6,10, 50, 60, XNUMX, XNUMX, XNUMX et XNUMX
- Facteur de forme:
  - Low Profile
  - 2.535"H x 6.6"L (64mm x 167mm)
- Ports : 8 internes
- Connecteurs : 2 connecteurs internes mini-SAS SFF8643 (montage horizontal)
- Interface de bus : conforme à la norme PCI Express 8 x3.0 voies
- Processeur : LSISAS3108 dual core RAID on Chip (ROC)
- Mémoire cache : 1 Go de mémoire SDRAM DDRIII à 1866 XNUMX MHz
- Protection du cache : module Flash CacheVault en option (LSICVM02)
- Température de fonctionnement : Température ambiante maximale : Carte contrôleur : 55 °C, avec l'accessoire CacheVault en option (LSICVM02) : 55 °C
- Tension de fonctionnement : +3.3 V, +12 V
- Certifications réglementaires : EN55022, EN55024, EN60950, EN 61000-3-2, EN 61000-3-3 ; FCC Classe A, Classe B ; UL1950 ; UL ; CSA C22.2 ; VCCI ; RRL pour MIC ; BSMI; C-tick
- Prise en charge du système d'exploitation : Microsoft Windows Server 2012/8 et 7/2008/Vista/2003/XP, Linux, SolarisTM (x86), Netware, FreeBSD et VMware
- Garantie: ans 3
Conception et construction
Au cœur du LSI MegaRAID 9361-8i se trouve le contrôleur RoC double cœur LSISAS3108, couplé à 1 Go de SDRAM DDRIII à 1866 MHz, qui offre protection des données et performances. Lorsqu'une panne de disque physique se produit, il passe en "état de protection". Une fois que cela se produit, le contrôleur MegaRAID commence immédiatement les diagnostics du disque afin de découvrir si le disque est réellement tombé en panne et/ou s'il peut être restauré ; c'est une fonctionnalité très pratique pour les utilisateurs qui cherchent à gagner du temps et de l'argent.
Le 9361-8i peut connecter jusqu'à 128 disques SATA ou SAS avec ses huit ports SATA et SAS 12 Gb/s internes et peut s'intégrer dans des serveurs montés en rack avec un facteur de forme HHHL à profil bas et des connecteurs SAS latéraux.
Les deux connecteurs internes mini-SAS HD-8643i SFF-4 connectent le contrôleur par câble aux disques SAS ou aux disques SATA. La série 9361 ne prend pas en charge les connecteurs externes, donc pour les solutions JBOD, vous devrez pour l'instant revenir aux cartes SAS2 telles que la LSI 9286-8e.
Spécifications du serveur de test SuperStorage Server 2027R-AR24NV :
- 2 x Intel Xeon E5-2687 v2 (3.4 GHz, 25 Mo de cache, 8 cœurs)
- Jeu de puces Intel C602
- Mémoire - 256 Go (16 x 16 Go) 1333 Mhz Micron DDR3 enregistrés RDIMM
- Norme Windows Server 2012 – 100 Go Micron RealSSD P400e Démarrage SSD
- Supermicro HBA SAS3 (Contrôleur LSI SAS 3008)
- LSI MegaRAID 9361-8i
  - 8 x 400GB HGST Ultrastar SSD800MM
Analyse synthétique de la charge de travail d'entreprise
Notre processus de référence d'entreprise préconditionne chaque baie de stockage dans un état stable avec la même charge de travail avec laquelle l'appareil sera testé sous une charge lourde de 16 threads avec une file d'attente exceptionnelle de 16 par thread, puis testé à des intervalles définis dans plusieurs threads/profondeur de file d'attente profils pour afficher les performances en cas d'utilisation légère et intensive.
Tests de préconditionnement et d'état stable primaire :
Débit (agrégat IOPS lecture + écriture)
Latence moyenne (latence de lecture + écriture moyennée ensemble)
Latence maximale (latence maximale de lecture ou d'écriture)
Écart-type de latence (écart-type de lecture + écriture moyenné ensemble)
Notre analyse synthétique de la charge de travail d'entreprise comprend des profils basés sur des tâches réelles. Ces profils ont été développés pour faciliter la comparaison avec nos références passées ainsi qu'avec des valeurs largement publiées telles que 8k 70/30 qui est couramment utilisée pour les produits d'entreprise.
- 4k
  - 100 % de lecture ou 100 % d'écriture
  - 100% 4K
- 8k 70/30
  - 70 % de lecture, 30 % d'écriture
  - 100% 8K
- 8k (séquentiel)
  - 100 % de lecture ou 100 % d'écriture
  - 100% 8K
- 128k (séquentiel)
  - 100 % de lecture ou 100 % d'écriture
  - 100% 128K
Pour mesurer les performances maximales du LSI MegaRAID 9361-8i, nous avons utilisé huit SSD HGST Ultrastar SSD800MM dans notre serveur Supermicro SuperStorage 2027R-AR24NV . Ces SSD ont ensuite été configurés en différents types de RAID ou de RAID imbriqués afin de montrer comment les performances évoluaient selon les modes pris en charge.
Dans ce test de débit 4k, LSI MegaRAID en configuration RAID0 est en tête du classement en termes de vitesse de lecture et d'écriture, avec un impressionnant 556,427 573,489 IOP en lecture et 60 556,870 IOP en écriture. Rien d'autre ne s'est même rapproché à distance dans la colonne Vitesse d'écriture; cependant, le MegaRAID RAID XNUMX a battu de justesse son débit de lecture avec XNUMX XNUMX IOPS.
Comme ce fut le cas lors de nos tests 8K, les configurations MegaRAID RAID 5 et 6 de LSI avaient une latence moyenne nettement plus élevée, en particulier dans la colonne Write, qui atteignait respectivement 38.43 ms et 38.89 ms. Toutes les autres configurations ont généralement bien fonctionné.
Lors de la comparaison de la latence maximale, tout le monde a bien performé à l'exception de l'énorme blip dans le graphique qui est le 9300-8i IR en configuration RAID 10.
En regardant plus loin dans la latence globale, le LSI 9300-8i IR en RAID10 montre toujours une faiblesse en ce qui concerne la gestion des données de parité par rapport à l'autre raid H/W du 9361-8i.
Dans notre test de charge de travail mixte 8k, 70% lecture, 30% écriture, mesurant le débit, la configuration LSI MegaRAID RAID0 offrait sans surprise les meilleures performances globales à pratiquement toutes les profondeurs avec un pic supérieur à 450,000 10 IOPS, soit presque le double du débit de la deuxième place RAID 9300 configuration. La différence surprenante dans cette revue était la quantité d'amélioration des performances offerte par la carte RAID par rapport à la LSI 8-0i IR en RAID10, qui atteignait environ la moitié du débit. La même différence peut être observée en comparant RAID9361 sur le 8-9300i par rapport au 8-XNUMXi IR.
Le LSI-Supermicro 9300-8i en configuration RAID 0 offrait la latence moyenne globale la plus faible du 16T/16Q. La configuration MegaRAID RAID 5 et 6 de LSI avait le score de latence le plus élevé de 16T/16Q, atteignant presque 30 ms.
La plupart des configurations RAID ont obtenu d'assez bons résultats dans les tests de latence moyenne, en particulier MegaRAID RAID 0, qui n'avait presque pas de pics de latence. Le MegaRAID en configuration RAID 6 faisait cependant exception avec une latence moyenne maximale atteignant près de 10ms. Un élément qui a suivi le graphique était la latence maximale du LSI 9300-8i IR en RAID10, qui flottait au-dessus de 1,000 XNUMX ms.
Parmi les configurations RAID, le MegaRAID RAID50 et le LSI 9300-8i IR RAID0 offraient l'écart type le plus étroit au niveau 16T/16Q. Encore une fois, la configuration MegaRAID RAID 5 et 6 de LSI a un peu augmenté au cours de ces tests et avait un écart type significativement plus élevé à la fin. Le 9300-8i IR en RAID10 a de nouveau tracé ce graphique dans la plage d'écart type de 11 à 37 ms.
Dans notre prochain benchmark, nous nous en tenons toujours à la taille de transfert de 8k, mais en passant à des opérations séquentielles de lecture à 100% et d'écriture à 100%. Sous cette charge de travail, le MegaRAID en configuration RAID 0 a continué de briller, atteignant respectivement 482,722 438,929 IOPS et XNUMX XNUMX IOPS en lecture et en écriture. Nous voyons également l'avantage de passer à une solution RAID matérielle sur le HBA avec des capacités RAID intégrées, où pour une configuration RAID donnée, la solution H/W avait le double des performances de lecture.
Dans notre benchmark synthétique final, nous avons utilisé une taille de transfert beaucoup plus grande de 128k avec des opérations de lecture à 100% et d'écriture à 100%. Sous cette charge de travail, la plupart des configurations RAID pour MegaRAID et Supermicro étaient au coude à coude pour le débit de lecture ; cependant, le MegaRAID RAID 0 est une fois de plus arrivé en tête dans la colonne d'écriture avec une marge significative.
Conclusion
Le LSI MegaRAID SAS 9361-8i apporte une solution RAID SAS3 complète sur le marché pour les serveurs qui veulent tirer le meilleur parti de leur stockage haut de gamme, mais qui ont besoin de la fiabilité, de la compatibilité et de la protection des données qui sont l'héritage de LSI. Avec les extensions, le 9361-8i peut prendre en charge jusqu'à 128 disques SATA ou SAS. Bien que la carte soit conçue pour les disques SAS3, elle est bien sûr rétrocompatible. LSI prend en charge les niveaux RAID 0,1,5,6,10, 50, 60, 9361, 8, XNUMX et XNUMX avec le XNUMX-XNUMXi, et la protection des données d'entreprise via CacheVault (facultatif) qui ajoute une autre couche de protection si un disque semble en panne.
En ce qui concerne les performances, le LSI 9361-8i n'est pas en reste lorsqu'il s'agit de gérer les derniers et les meilleurs SSD SAS3. En gérant huit SSD Hitachi SSD800MM, nous avons testé tous les modes RAID pris en charge sur la carte et les avons comparés à l'ensemble de fonctionnalités RAID intégré sur un LSI/Supermicro 9300-8i. En RAID0, nous avons mesuré plus de 556 573 IOPS en lecture, 4 450 IOPS en écriture dans notre test aléatoire 8 70 et un peu moins de 30 0 IOPS dans notre test de charge de travail mixte 10 XNUMX XNUMX/XNUMX. Cela se traduit par environ le double des performances par rapport au modèle HBA qui prend en charge à la fois RAIDXNUMX et RAIDXNUMX. Pour être juste, le HBA n'est évidemment pas destiné aux configurations les plus performantes, mais l'échelle comparative est pertinente.
En passant à des charges de travail séquentielles, nous avons constaté que le débit a atteint son maximum lors de notre test 8 482 à 438 0 IOPS en lecture et 9361 8 IOPS en écriture en RAID4.9. Dans les transferts de gros blocs, le 50-0i a fait pencher la balance en offrant un peu moins de 4.3 Go/s en lecture dans son mode RAID9361 et RAID8 offrant la bande passante d'écriture la plus élevée à 2 Go/s en continu. Dans l'ensemble, lorsqu'il s'agit de prendre en charge la dernière génération de SSD, le LSI MegaRAID 3-1i a beaucoup à offrir. Il s'appuie sur les modèles SASXNUMX incroyablement populaires qui l'ont précédé, tout en ajoutant la prise en charge des SSD SASXNUMX qui peuvent atteindre des vitesses individuelles allant jusqu'à XNUMX Go/s R/W.
Avantages
- Excellentes performances à coupler avec les SSD SAS3
- Offre la protection du cache flash CacheVault
- Large prise en charge des pilotes
Inconvénients
- Limité à 8 ports internes sur la carte
Conclusion
L'adaptateur de stockage LSI MegaRAID SAS 9361-8i permet aux SSD SAS3 de dernière génération de se concentrer sur la saturation du bus PCIe Gen3 sans avoir à se soucier de la carte RAID qui les retient.
LSI MegaRAID 9361 sur Amazon