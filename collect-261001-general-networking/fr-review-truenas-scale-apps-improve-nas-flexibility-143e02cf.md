Avec le lancement de TrueNAS SCALE 22.12.2 plus tôt cette année, l'équipe iX a clairement l'intention de parler un peu plus à l'entreprise. Une partie de la messagerie mise à l'échelle provient de nouveaux systèmes matériels et de la prise en charge de la gestion des NVDIMM et des améliorations des fonctionnalités HA et de réplication. Mais une autre poussée majeure est venue autour de la prise en charge des applications, faisant des applications TrueNAS SCALE une affaire beaucoup plus importante.
iX a expédié un nouveau Mini R, configuré avec TruNAS SCALE, et nous a demandé de fouiller un peu pour voir ce que nous pensions du catalogue d'applications amélioré et des intégrations.
TrueNAS SCALE Bluefin, version 22.12.2, introduit un nouvel ensemble d'applications dans le catalogue TrueNAS. Ces applications sont classées en trois trains en fonction de leurs niveaux de support et de maintenance. Le train « Communauté » se compose d'applications fournies par iXsystems ou la communauté, iX examinant et modérant les modifications, mais ne fournissant pas de support direct.
Le train "officiel" contient des applications régulièrement testées et entretenues par les ingénieurs de TrueNAS, avec des problèmes importants résolus rapidement. Les applications du train "Communauté" peuvent passer au train "Officiel" si elles s'avèrent de haute qualité et largement utilisées.
Le train "Enterprise" est exclusif aux appliances TrueNAS Enterprise et comprend des applications avec des fonctionnalités de niveau entreprise, soigneusement testées, maintenues et documentées pour les cas d'utilisation critiques. Les éditeurs de logiciels peuvent contacter iXsystems pour discuter de l'inclusion de leurs applications dans ce train.
En ce qui concerne le matériel sous-jacent, le TrueNAS Mini R est un NAS rackable plus grand au sein de la famille Mini ; vous trouverez plus d’informations sur le site Web de TrueNAS.
Bien que physiquement plus grand que les autres Minis, le TrueNAS Mini R utilise cette empreinte pour le stockage. Le serveur 2U à faible profondeur offre 12 baies de stockage SATA 3.5″ pouvant être utilisées avec un mélange de disques SSD et de disques durs. Le système commence à 1848 $ et est assez configurable en fonction des besoins des clients.
Spécifications du TrueNAS Mini R
| Fonctionnalité | TrueNAS Mini R | 
|---|---|
| Châssis | Boîtier à 12 baies - Conception silencieuse pour le bureau : 45 dB en veille, 52 dB en crête | 
| Baies de disques | 12 baies de lecteur SATA 3.5" remplaçables à chaud (adaptateurs 3.5" à 2.5" en option disponibles) | 
| Capacité brute maximale | Jusqu'à 216 TB | 
| Processeur | Processeur Intel octa-core C3758 | 
| Mémoire | 32 Go DDR4 avec ECC (extensible à 64 Go) | 
| RAID | OpenZFS : bande (RAID0), miroir multidisque (RAID10), parité RAIDZ1 (RAID5), RAIDZ2 (RAID6) et RAIDZ3 (triple parité) | 
| Gestion de disque | Disques remplaçables à chaud, Bad Block Scan + HDD SMART, prise en charge du montage ISO, chiffrement de disque accéléré par le matériel | 
| Réseau | Standard : 2 ports LAN Ethernet RJ45 1/10GBaseT Port IPMI RJ45 dédié (gestion du matériel à distance) 2 x carte d'extension SFP+ 10G (en option) | 
| Ports USB | 1 port USB 3.0 (arrière) 2 ports USB 2.0 (arrière) | 
| Cache de lecture/écriture | (Facultatif) Améliorez les performances en ajoutant un cache de lecture hautes performances dédié (L2ARC) ou en ajoutant un cache d'écriture hautes performances dédié (ZIL/SLOG) | 
| Extension PCIe | 1 x PCI Express 3.0 x 4 | 
| Alimentation | 100 V à 240 V CA, 50/60 HZ, monophasé | 
| Consommation électrique (maximale) | Sans disque : 63 W, avec disques et carte d'extension 10 G : 167 W | 
| Gestion de l'énergie | Mise sous/hors tension à distance (IPMI), réponse au signal de l'onduleur et alertes | 
| Interface de contrôle utilisateur | Navigateur Web et gestion du matériel à distance (IPMI) | 
| Dimensions (L x P x H) | 17.2″ x 21″ x 3.5″ / 437 × 533 × 89 mm | 
| Poids (pas de disques) | 18.7 livres / 8.5 kg | 
| Garantie limitée | Garantie d'un an incluse - Garantie de 1 ans en option à l'achat. La garantie logicielle nécessite un enregistrement sur portail.ixsystems.com. | 
| Accessoires | Guide de configuration de base ; 2 clés de lunette ; sac de vis HDD ; 2 câbles en cuivre cat7 de 6 m ; Facette; 4 pieds en caoutchouc adhésifs ; Kit de rails courts : profondeur de montage en rack de 19″ à 26.6″ ; Kit de rails longs en option disponible : profondeur de montage en rack de 26.5″ à 36.4″ | 
ÉCHELLE TrueNAS avec LINUX
TrueNAS SCALE sur le Mini R offre suffisamment de puissance pour exécuter un partage ZFS entièrement déployé avec la possibilité d'exécuter plusieurs conteneurs. Bien que le processeur ne soit pas conçu pour être une centrale électrique (processeur Intel Atom C3758 à 2.20 GHz), il s'agit d'une puce à huit cœurs à huit threads avec un cache L16 de 2 Mo.
Il s'agit d'une puce 14 nm basée sur l'architecture Intel Denverton, lancée au troisième trimestre 2017, qui affiche une faible consommation (TDP de 25 W) et prend en charge la mémoire vive DDR4 ECC (qu'elle exploite pleinement). Notre modèle de test est équipé de 64 Go de RAM. Pour répondre aux besoins de traitement et d'accélération, le TrueNAS requiert une quantité importante de RAM, pouvant atteindre 5 Go par To pour la déduplication. La configuration minimale est de 8 Go de RAM pour un maximum de huit disques, et il faut ajouter 1 Go par disque supplémentaire. Pour une configuration de base, il est conseillé d'opter pour 16 Go ou 32 Go de RAM.
Notre expérience a montré que le processeur était plus que suffisant pour exécuter TrueNAS et suivre plusieurs conteneurs et même certaines machines virtuelles.
TrueNAS SCALE offre toujours toutes les fonctionnalités ZFS et de partage de TrueNAS CORE tout en ajoutant une nouvelle prise en charge des applications conteneurisées via Docker et le catalogue d'applications organisé.
Nous avons même obtenu une installation complète de Windows Server 2022 pour installer et fonctionner dans l'espace de virtualisation. Certes, c'était un peu plus rugueux qu'un hyperviseur plus mature.
Nous avons constaté que les pilotes de Windows Server 2022 devaient être chargés à partir d'un ISO monté séparément, mais il s'intégrait bien aux ressources embarquées et était immédiatement en ligne.
Applications TrueNAS SCALE
Les applications TrueNAS SCALE sont très similaires à celles prises en charge par TrueNAS CORE. La sélection de l'application dans la liste et son déploiement sont à peu près identiques à l'expérience de déploiement et de gestion de TrueNAS CORE.
Nous avons essayé de déployer "Home Assistant" et "Grafana", et c'est aussi simple que de choisir, cliquer et déployer.
Une fois lancé, il affiche le numéro de port de l'application en cours d'exécution et vous pouvez vous connecter via un navigateur sur le réseau. Nous devions simplement ouvrir un navigateur, entrer l'adresse IP du NAS et le numéro de port de l'application, et chaque nouvelle application nous accueillait avec un écran de configuration et de configuration.
Ce fut une expérience presque identique à CORE et très similaire à de nombreuses autres offres NAS grand public.
Cependant, ce qui est nouveau, c'est la possibilité d'ajouter des conteneurs non répertoriés dans la liste des applications. Le déploiement de conteneurs Docker était simple si vous avez une certaine expérience avec Docker.
SCALE vous permet de déployer un conteneur Docker en sélectionnant « Lancer une image Docker » dans la section Applications et en le configurant directement via l'interface graphique. Cette méthode est cependant moins intuitive que l'utilisation du dépôt d'applications et nécessite de parcourir le dépôt Docker Hub ( https://hub.docker.com/search?q= ) pour trouver une image. Elle est plutôt destinée aux utilisateurs intermédiaires ayant une certaine expérience des conteneurs (Docker ou Kubernetes).
Entrer le nom du référentiel Docker et le configurer à partir du menu déroulant de l'interface graphique était agréable, mais si c'est votre première fois avec Docker, l'interface graphique ne vous tient pas la main et vous devrez consulter la base de connaissances.
La suppression des conteneurs inutiles est simple ; sélectionnez le conteneur à supprimer et cliquez sur supprimer.
Performances du iX Mini R
Alors que nous étions principalement intéressés par la vérification du catalogue et de l'intégration des applications TrueNAS SCALE, nous voulions pousser un peu le Mini R du côté des performances pour voir comment il se gère. Pour cet examen, notre unité était équipée de 4 disques SSD iX SATA de 1.9 To et de 8 disques durs WD Red Plus de 10 To. Notre unité comprenait également l'empreinte RAM améliorée de 64 Go.
Nous avons abordé les performances du flash à l'intérieur de notre unité d'examen iX Mini R. Nous avons configuré deux pools de stockage RAIDZ2, chacun avec la compression activée mais sans la déduplication activée. Nous avons examiné à la fois les performances externes sur la connexion 10GbE et les performances internes lorsque le stockage a été présenté à une machine virtuelle Server 2022 exécutée sur le système.
Notre processus de référence de stockage partagé et de disque dur d'entreprise préconditionne chaque disque dans un état stable avec la même charge de travail avec laquelle l'appareil sera testé sous une lourde charge de 16 threads avec une file d'attente exceptionnelle de 16 par thread, puis testé à intervalles définis dans plusieurs threads. / profils de profondeur de file d'attente pour afficher les performances en cas d'utilisation légère et intensive. Étant donné que les solutions NAS atteignent très rapidement leur niveau de performance nominal, nous ne représentons graphiquement que les principales sections de chaque test.
Tests de préconditionnement et d'état stable primaire :
- Débit (agrégat IOPS lecture + écriture)
- Latence moyenne (latence de lecture + écriture moyennée ensemble)
- Latence maximale (latence maximale de lecture ou d'écriture)
- Écart-type de latence (écart-type de lecture + écriture moyenné ensemble)
Notre analyse de charge de travail synthétique d'entreprise comprend quatre profils basés sur des tâches réelles. Ces profils ont été développés pour faciliter la comparaison avec nos références passées ainsi qu'avec des valeurs largement publiées telles que la vitesse de lecture et d'écriture maximale de 4k et 8k 70/30, qui est couramment utilisée pour les disques d'entreprise.
- 4K
  - 100 % de lecture ou 100 % d'écriture
- 8K (séquentiel)
  - 100 % de lecture ou 100 % d'écriture
- 128K (séquentiel)
  - 100 % de lecture ou 100 % d'écriture
En commençant par des performances de lecture aléatoire 4K sur 10 GbE sur nos quatre SSD en RAIDZ2, nous avons mesuré 3,545 1,017 IOPS en lecture et XNUMX XNUMX IOPS en écriture.
Ensuite, nous sommes passés à notre test de lecture et d'écriture séquentielle de 8 16, où nous avons mesuré un peu plus de 14.9 XNUMX IOPS en lecture et XNUMX XNUMX IOPS en écriture.
Enfin, lors de notre test de transfert séquentiel de 128k, nous avons mesuré 972 Mo/s en lecture et 738 Mo/s en écriture sur le fil.
Pour mesurer les performances du stockage interne à la box, sans la surcharge d'Ethernet, nous avons utilisé CrystalDiskMark à l'intérieur de la VM fonctionnant sur l'hyperviseur intégré. Ici, nous avons mesuré 811 Mo/s en lecture et 425 Mo/s en écriture avec une charge de travail de transfert séquentiel de 1 M.
Conclusion
Il y a beaucoup à explorer dans le catalogue d'applications TrueNAS SCALE, et pour la plupart, il est facile de travailler avec, en particulier pour ceux qui connaissent TrueNAS SCALE. Il y a d'autres endroits, cependant, où il est facile de se perdre dans les mauvaises herbes. La communauté voudra certainement continuer à créer de la documentation et des guides pour les utilisateurs qui souhaitent explorer en dehors des trains d'applications d'entreprise et officiels.
C'est également formidable de voir du matériel optimisé comme l'iX Mini R. Avec le passage à Linux et la puissance de ZFS, et la capacité d'héberger des conteneurs et des machines virtuelles, il s'agit d'un appareil capable pour les PME avec la polyvalence d'expansion à l'avenir. La croissance combinée continue de la plate-forme logicielle, avec davantage de prise en charge du matériel et des services de données d'entreprise et la variation croissante des plates-formes matérielles, sont toutes de bons pas en avant pour TrueNAS en général et SCALE en particulier, car iX continue de pousser plus loin la pile informatique de l'entreprise.