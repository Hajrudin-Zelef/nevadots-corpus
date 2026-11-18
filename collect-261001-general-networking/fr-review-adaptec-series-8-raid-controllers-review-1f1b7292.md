La gamme Adaptec Série 8 est composée d'adaptateurs RAID 12 Gb/s dotés d'une architecture PMC complète intégrant les contrôleurs de protocole d'E/S, les contrôleurs RoC, les expandeurs SAS et le logiciel PMC. Les cartes Adaptec Série 8 offrent des performances nettement supérieures à la génération précédente , avec des vitesses considérablement accrues. Elles sont également équipées de la technologie maxCache Plus, qui prend en charge la mise en cache et la hiérarchisation des données, permettant ainsi aux responsables informatiques, aux intégrateurs système et aux éditeurs de logiciels indépendants d'optimiser les performances et le rapport qualité-prix de leurs systèmes de stockage. maxCache Plus offre également aux utilisateurs la possibilité de configurer leurs périphériques de stockage dans des environnements serveur et d'utiliser les supports les plus rapides comme stockage et non plus seulement comme cache. Ainsi, le potentiel des disques 12 Gb/s les plus récents du marché est pleinement exploité.
Nous avions précédemment testé la gamme Adaptec Série 7 (71605Q) et étions satisfaits de ses performances en termes de débit. Cependant, c'est surtout sa grande flexibilité, avec ses 24 ports sans extension, qui nous avait impressionnés, établissant ainsi une nouvelle référence en matière de polyvalence sur le marché. À cette époque, Adaptec était (et est toujours) en concurrence féroce avec LSI, les deux entreprises rivalisant pour proposer soit un grand nombre de ports, soit des performances élevées. La Série 8, quant à elle, offre aux entreprises 16 ports et une bande passante impressionnante de 12 Gbit/s, démontrant ainsi la volonté d'Adaptec de repousser les limites du marché. Cette innovation constante contribue à faire d'Adaptec, l'entreprise l'espère, le leader incontesté du marché des adaptateurs RAID.
La gamme Series 8 d'Adaptec comprend cinq modèles différents :
- 81605ZQ: C'est la carte que nous utilisons dans notre Serveur de fichiers Supermicro, qui dispose de 16 ports internes avec protection de cache SuperCap (incluse) de sauvegarde Flash (intégrée).
- 8885 et 8885Q : 8 ports internes et 8 ports externes ; le 8885Q est équipé de la protection de cache AFM-700 en option.
- 8805: La seule carte de la gamme 8-Series avec 8 ports (internes) au total.
Tous les modèles prennent en charge les niveaux RAID 0, 1, 1E, 5, 6, 10, 50, 50 et 60.
Les 8885 et 8885Q coûtent actuellement respectivement 725.00 $ et 1100.00 $, tandis que les 81605ZQ et 8805 sont disponibles pour 1065.00 $ et 640.00 $. Toutes les versions sont livrées avec une garantie de trois ans. Notre examen se concentrera sur l'unité 81605ZQ.
Spécifications du contrôleur RAID Adaptec série 8
8160ZQ
- Numéro de pièce de commande : 2281600-R (simple)
- Niveaux RAID : 0,1,1E,5,6,10, 50, 60
- Facteur de forme:
  - MD2 – Profil bas
  - 2.535"H x 6.6"L (64mm x 167mm)
- Ports : 16 internes
- Connecteurs : 4 X SFF-8643
- Interface de bus : PCIe Gen8 à 3 voies
- Processeur : PMC PM8063
- Cache: 1024MB
- Protection du cache : Flash backup (intégré) Supercap (inclus)
8885Q
- Numéro de pièce de commande : 2277100-R (simple)
- Niveaux RAID : 0,1,1E,5,6,10, 50, 60
- Facteur de forme:
  - MD2 – Profil bas
  - 2.535"H x 6.6"L (64mm x 167mm)
- Ports : 8 internes/8 externes
- Connecteurs:
  - 2 x SFF-8643
  - 2 x SFF-8644
- Interface de bus : PCIe Gen8 à 3 voies
- Processeur : PMC PM8063
- Cache: 1024MB
- Protection du cache : AFM-700 (inclus)
8885
- Numéro de pièce de commande : 2277000-R (simple)
- Niveaux RAID : 0,1,1E,5,6,10, 50, 60
- Facteur de forme:
  - MD2 – Profil bas
  - 2.535"H x 6.6"L (64mm x 167mm)
- Ports : 8 internes/8 externes
- Connecteurs:
  - 2 x SFF-8643
  - 2 x SFF-8644
- Interface de bus : PCIe Gen8 à 3 voies
- Processeur : PMC PM8063
- Cache: 1024MB
- Protection du cache : AFM-700 (en option)
8805
- Numéro de pièce de commande : 2277500-R (simple)
- Niveaux RAID : 0,1,1E,5,6,10, 50, 60
- Facteur de forme:
  - MD2 – Profil bas
  - 2.535"H x 6.6"L (64mm x 167mm)
- Ports : 8 internes
- Connecteurs : 2 X SFF-8643
- Interface de bus : PCIe Gen8 à 3 voies
- Processeur : PMC PM8063
- Cache: 1024MB
- Protection du cache : AFM-700 (en option)
Spécifications générales:
- Dimensions physiques : 2.535" H x 6.6" L (64 mm x 167 mm)
- Température de fonctionnement : 0°C à 55°C* (avec débit d'air de 200 LFM, sans flash) ; 0°C à 50°C* (avec débit d'air de 200 LFM, avec flash)
- Courant de fonctionnement : 1.0 A à 3.3 V CC ; 1.1 A à 12.0 Vcc 8805, 8885, 8885Q ; 1.5 A à 3.3 Vcc ; 1.0 A à 12.0 Vcc 81605ZQ
- Certifications réglementaires CE, FCC, UL, C-tick, VCCI, KCC, CNS
- Conformité environnementale RoHS
- MTBF en test maintenant
- Garantie: ans 3
Construire et concevoir
Au cœur de chaque nouvelle carte RAID Adaptec de la série 8 se trouve le contrôleur RAID sur puce PM8063, qui s'interface avec le système hôte via une connexion PCIe Gen8 à 3 voies offrant jusqu'à 8 Go/s de bande passante brute combinée avec XOR en ligne pour réduire l'accès à la DDR3 pour optimiser les performances. Le PM8063 prend en charge 16 appareils à des vitesses natives de 12 Gb/s. Pour les besoins de cet examen, nous nous concentrons sur le nouveau 81605ZQ, qui prend en charge 16 périphériques internes, ainsi qu'un module flash intégré (sans avoir besoin d'une carte fille supplémentaire) pour la protection des données en cas de panne de courant. scénario.
La carte 81605ZQ se présente au format HHHL compact et intègre entièrement sa fonction de sauvegarde flash. Contrairement aux autres cartes de la série Adaptec, qui nécessitent des cartes filles additionnelles pour activer cette fonctionnalité, cette conception compacte libère de l'espace au profit de deux ports mini SAS HD supplémentaires à l'arrière. Bien que les autres cartes de la série Adaptec 8 prennent également en charge 16 périphériques, aucune ne propose 16 ports internes. Elles offrent en revanche une combinaison de 8 ports internes et 8 ports externes. Le seul périphérique externe requis pour la 81605ZQ est une batterie de sauvegarde, qui peut être placée quasiment n'importe où à l'intérieur du châssis du serveur. Dans notre cas, pour le serveur Supermicro SuperStorage 2027R-AR24NV, nous avons pu positionner les batteries directement derrière les baies de stockage avant, dans la zone de ventilation la plus optimale du châssis.
Enfin, sont les connecteurs internes SFF-8643 ; l'adaptateur se connecte via un emplacement PCIe Gen3 x8 ; cependant, il est également rétrocompatible avec les systèmes PCIe Gen2. L'adaptateur 81605ZQ n'est pas équipé de connecteurs externes, contrairement à deux autres modèles, les 8885 et 885Q, ce qui leur donne une certaine flexibilité.
Configuration des tests
La carte Adaptec série 8 81605ZQ est utilisée dans notre serveur de stockage Supermicro SuperStorage Server 2027R-AR24NV , fonctionnant sous Windows Server 2012 R2 et faisant office de serveur de fichiers pour le laboratoire de test StorageReview. Le SuperStorage Server AR24NV est une plateforme de stockage haut de gamme conçue spécifiquement pour exploiter les périphériques de stockage SAS3 les plus performants.
Les deux principaux sous-systèmes de l'AR24NV comprennent le châssis SC216A-R920LPB 2U 24 baies et la carte serveur biprocesseur X9DRH-iF-NV. Sa caractéristique la plus importante, cependant, est un tout nouveau fond de panier à connexion directe SAS24 3 Gb/s à 12 baies optimisé pour la dernière génération de SSD, où la série Adaptec 8 entre en jeu.
Nous avons installé trois cartes Adaptec 81605ZQ pour se connecter à quatre pools de disques SSD. Dans notre configuration haute performance pour les tests, nous avons inclus 16 SSD Hitachi SSD800MM 400 Go, 8 SSD Micron M500 960 Go, ainsi que 2 SSD Seagate SSD 600 480 Go. Nous avons réparti les groupes jusqu'à 8 SSD SSD800MM sur deux cartes Adaptec 81605ZQ, la dernière étant connectée aux SSD 8 Micron et 2 SSD Seagate. Dans notre configuration orientée vers des performances optimales, nous avions deux pools RAID10 SAS3 composés des SSD Hitachi, un pool RAID50 du Micron SSS et un dernier pool RAID1 des SSD Seagate pour le démarrage.
Pour mesurer les performances du 81605ZQ dans un environnement réel, nous avons créé des partages SMB3, iSCSI et NFS dans Windows Server 2012 R2 et y avons accédé via notre infrastructure 10GbE de bout en bout. Pour les tests HGST SSD800MM, nous avions deux partages ou LUN de 25 Go sur chaque pool de stockage RAID10 (quatre partages ou LUN au total) ou quatre partages ou LUN sur le seul pool de stockage RAID50 Micron M500. Pour ces quatre connexions, nous avons utilisé chacune un port 10GbE dédié pour leur donner autant de bande passante qu'elles en auraient besoin.
Analyse synthétique de la charge de travail d'entreprise
Notre processus de référence d'entreprise préconditionne chaque périphérique de stockage dans un état stable avec la même charge de travail avec laquelle le périphérique sera testé sous une charge lourde de 16 threads avec une file d'attente exceptionnelle de 16 par thread, puis testé à des intervalles définis dans plusieurs threads/profondeur de file d'attente profils pour afficher les performances en cas d'utilisation légère et intensive.
Tests de préconditionnement et d'état stable primaire :
- Débit (agrégat IOPS lecture + écriture)
- Latence moyenne (latence de lecture + écriture moyennée ensemble)
- Latence maximale (latence maximale de lecture ou d'écriture)
- Écart-type de latence (écart-type de lecture + écriture moyenné ensemble)
Notre analyse synthétique de la charge de travail d'entreprise comprend des profils basés sur des tâches réelles. Ces profils ont été développés pour faciliter la comparaison avec nos références passées ainsi qu'avec des valeurs largement publiées telles que 8k 70/30 qui est couramment utilisée pour les produits d'entreprise.
- 8k 70/30
  - 70 % de lecture, 30 % d'écriture
  - 100% 8K
Nous nous sommes concentrés sur notre charge de travail 8k 70/30, car dans notre laboratoire de test d'entreprise virtualisé avec plus de 100 machines virtuelles actives sur différents tests, les performances de la charge de travail aléatoire sont essentielles. Bien que notre environnement VMware exploite les chiffres de performance iSCSI, SMB3 et NFS ont également été inclus pour voir les performances de Windows Server 2012 R2 pour chaque protocole.
Lors du premier test mesurant le débit, les seize SSD Hitachi SSD800MM SAS3 dans deux pools RAID10 accessibles via SMB3 ont offert les meilleures performances globales à chaque profondeur, atteignant plus de 160,000 78 IOPS, battant facilement ses partages iSCSI ou NFS qui dépassaient respectivement 37 500 IOPS et 50 3 IOPS. Le pool Micron M55 RAID52 offrait des performances SMB11.4 dépassant XNUMX XNUMX IOPS, avec iSCSI et NFS à la traîne avec des chiffres supérieurs de XNUMX XNUMX IOPS et XNUMX XNUMX IOPS respectivement.
La configuration Hitachi SSD800MM 2xRAID10 sur SMB3 offrait la latence moyenne globale la plus faible, passant de 0.33 ms à 2T/2Q à 7.05 ms à 16T/16Q. Notre pool de stockage Micron M500 en RAID50 est passé de 0.66 ms à 2T/2Q à 19.24 ms à 16T/16Q.
La plupart des configurations de disque ont obtenu d'excellents résultats lors des tests Max Latency, en particulier le disque Micron, qui n'a montré pratiquement aucun pic. Cependant, le Hitachi SSD800MM 2xRAID10 via le conteneur iSCSI de Windows Server 2012 a atteint un pic assez élevé avec une latence maximale atteignant plus de 10,500 XNUMX ms, ce qui peut être inférieur à la limite supérieure que Windows peut prendre en charge via ce protocole.
Parmi les SSD, le pool de stockage Micron M500 RAID50 sur SMB3 a battu de peu le Hitachi SSD800MM SMB3 pour offrir l'écart type le plus étroit au niveau 16T/16Q. Le pool de stockage HGST sur iSCSI ainsi que le pool de stockage Micron sur NFS ont beaucoup augmenté au cours de ces tests et ont eu un écart type beaucoup plus élevé à la fin.
Conclusion
L'Adaptec Series 8 81605ZQ a beaucoup à offrir en tant que carte RAID de premier plan, avec 16 ports SAS 12 Gbit/s ainsi qu'une protection intégrée contre les pannes de courant sans avoir à ajouter de carte fille supplémentaire. En ce qui concerne les prix catalogue, le prix de la famille de cartes RAID de la série 8 n'est pas trop différent de celui des cartes de la série 7 qu'elles remplacent. Dans le cas du 16ZQ à 81605 ports (1,065 71605 $), il est en fait moins cher que le prix catalogue du 1,100Q (XNUMX XNUMX $), ce qui en fera une option intéressante pour ceux qui envisagent d'acheter de nouveaux équipements pouvant prendre en charge les dernières normes.
Adaptec montre à nouveau aux intégrateurs de systèmes, aux OEM et aux administrateurs informatiques autonomes que les cartes RAID de la série 8 ont beaucoup à offrir. Avec une densité de ports deux fois supérieure à celle des offres concurrentes, les fournisseurs de serveurs peuvent sérieusement envisager de maximiser le débit PCIe Gen3 avec des SSD SAS haut de gamme avec des versions moins complexes qui ignorent les extensions. Adaptec continue également à faire évoluer ses offres logicielles avec max Cache Plus, qui permet à la fois la mise en cache et la hiérarchisation.
Lors de nos tests dans un environnement Windows Server 2012 R2 hébergeant un stockage partagé dans notre environnement virtualisé, l'Adaptec Series 8 81605ZQ s'est plutôt bien comporté, poussant dans certains cas Windows à ses limites pour différents protocoles de stockage. En exploitant les performances de seize HGST SSD800MM dans deux pools RAID10 exploitant autant de cartes 81605ZQ, nous avons constaté des performances 8k 70/30 supérieures à 160k IOPS sur SMB3. Sur notre troisième 81605ZQ utilisé pour contrôler notre disque de démarrage RAID1 et un pool RAID50 de SSD Micron M500, nous avons vu les performances SMB3 dépasser 55 8 IOPS dans ce même test d'E/S aléatoires. Inutile de dire que le laboratoire est enthousiaste à l'idée de continuer à déployer les cartes de la série XNUMX dans une variété de projets à venir qui incluent un stockage haute performance.
Avantages
- Des performances incroyables
- Prix raisonnable pour le marché des entreprises
- Jusqu'à 16 ports SAS de 12 Go/s
Inconvénients
- Pas de modèle à 24 ports comme la série 7
Conclusion
La gamme Adaptec Series 8 convient parfaitement aux entreprises qui cherchent à améliorer considérablement les performances de leurs applications gourmandes en ressources ou de leurs environnements de serveurs denses à un niveau de pointe. Le 8160ZQ en particulier offre une excellente protection des données avec son module flash intégré, qui élimine le besoin d'une carte fille supplémentaire et les 16 ports offrent une énorme flexibilité dans le système.
Cartes RAID Adaptec série 8 sur Amazon
Discutez de cet avis