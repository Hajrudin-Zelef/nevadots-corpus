En novembre 2023, WD a lancé le disque dur Gold Enterprise de 24 To, offrant 2 To de plus que son précédent modèle de 22 To. Bien que cette mise à jour puisse paraître modeste, elle représente une amélioration significative de la capacité de stockage, notamment en termes de densité de stockage dans les configurations NAS et serveurs de grande envergure. Suite à notre premier test HL15 , où nous avons mis à l'épreuve quinze disques WD Gold de 24 To, nous sommes maintenant prêts à réévaluer ces disques au sein d'une configuration NAS standard à 8 disques.
La série WD Gold est conçue pour répondre aux divers besoins des entreprises, offrant une gamme complète de disques durs de classe entreprise de 1 To à 24 To. Ces disques sont conçus pour résister aux environnements de stockage exigeants, offrant un MTBF allant jusqu'à 2.5 millions d'heures, une technologie de protection contre les vibrations et une consommation d'énergie réduite grâce à la technologie HelioSeal pour les modèles de plus de 12 To.
Caractéristiques du WD Gold 24 To
WD a choisi de commercialiser ces disques haute capacité exclusivement avec une interface SATA, poursuivant ainsi la tradition de leur conception CMR remplie d'hélium à 7200 24 tr/min, un incontournable de leur gamme depuis plusieurs années. Il est intéressant de noter que le nouveau modèle Gold de 0.3 To réduit légèrement la consommation d'énergie, en utilisant 0.2 watt de moins en fonctionnement et 22 watt de moins en mode veille par rapport à son prédécesseur de XNUMX To.
Alors que la série Gold HDD couvre des capacités de 1 To à 24 To, il est à noter que la fonctionnalité OptiNAND, y compris l'innovant Armor Cache compatible OptiNAND, est exclusive aux modèles de 20 To et plus. Cette technologie met en synergie les avantages des modes activés et désactivés du cache en écriture, offrant aux utilisateurs un mélange de performances et de protection des données sans avoir à choisir entre les deux.
De plus, la série WD Gold offre flexibilité et évolutivité aux entreprises, permettant une configuration de stockage personnalisée capable de gérer des charges de travail d'applications intensives et de prendre en charge jusqu'à 550 To de données par an. Cela fait du disque dur WD Gold 24 To une option intéressante pour les entreprises cherchant à faire évoluer leur infrastructure de stockage de données tout en maintenant des niveaux élevés de fiabilité et de performances.
Gestionnaire de stockage et d'instantanés QNAP
Storage & Snapshots Manager est un outil polyvalent au sein de l'interface NAS QNAP qui permet aux utilisateurs de superviser et de gérer les différents aspects du stockage, y compris les configurations RAID, la santé des disques et la création d'instantanés pour la protection des données.
Le volet Stockage/Instantanés affiche un aperçu des volumes de stockage au sein du pool de stockage. Dans cet exemple, il met en évidence le volume système et plusieurs cibles iSCSI. Les barres indicatrices « Pourcentage utilisé » fournissent un aperçu rapide de l'utilisation de l'espace sur différents volumes.
La fenêtre Storage Pool 1 Management nous montre que tous les disques sont configurés dans une matrice RAID 6, ont une capacité totale de 130.91 To et sont en « bon » état. Cela indique que tout est prêt pour nos tests.
L'image ci-dessous montre la section Disques/VJBOD du gestionnaire de stockage et d'instantanés. Ici, les statuts « Bon » et « Prêt » confirment le déploiement de huit disques durs WD Gold haute capacité. Ce volet comprend également des informations instantanées telles que le numéro de modèle, la capacité du disque, la vitesse actuelle, le type de bus, etc., qui changent en fonction du disque sélectionné dans la liste. Ces informations sont cruciales pour maintenir des performances optimales et anticiper tout problème potentiel.
Spécifications du disque dur WD Gold 24 To
| Référence du modèle | WD241 CRISE | 
| Facteur de forme | 3.5 pouce | 
| Interface | SATA 6 Gb / s | 
| 512n / 512e secteurs utilisateur par lecteur4 | 512e | 
| Capacité formatée | 24TB | 
| Technologie OptiNAND | Oui | 
| conforme RoHS | Oui | 
| Performances |  | 
| Taux de transfert de données (max soutenu) | 298MB / s | 
| RPM | 7200 | 
| Cache | 512MB | 
| Gestion de l'énergie |  | 
| Besoins de puissance moyens (W) |  | 
| Efficacité | 6.8W | 
| Idle | 5.5W | 
| Indice d'efficacité énergétique (W/To, inactif) | 0.2 | 
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
Performances
Analyse synthétique de la charge de travail d'entreprise
Notre processus d'évaluation des disques durs d'entreprise conditionne chaque ensemble de disques à un état stable en utilisant la même charge de travail avec laquelle le périphérique sera testé. Cela implique une lourde charge de 16 threads et une file d'attente exceptionnelle de 16 par thread. L'appareil est ensuite testé à intervalles définis de plusieurs profils de profondeur de thread/file d'attente pour montrer ses performances dans des conditions d'utilisation légère et intensive. Étant donné que les disques durs atteignent rapidement leur niveau de performance nominal, seules les sections principales de chaque test sont représentées graphiquement.
Tests de préconditionnement et d'état stable primaire :
- Débit (agrégat IOPS lecture + écriture)
- Latence moyenne (latence de lecture + écriture moyennée ensemble)
- Latence maximale (latence maximale de lecture ou d'écriture)
- Écart-type de latence (écart-type de lecture + écriture moyenné ensemble)
Notre analyse synthétique de la charge de travail d'entreprise comprend quatre profils basés sur des tâches réelles. Ces profils ont été développés pour faciliter la comparaison avec nos références passées et les valeurs largement publiées, telles que la vitesse de lecture et d'écriture maximale de 4K et 8K 70/30, couramment utilisées pour les disques d'entreprise.
4K
- 100 pour cent de lecture ou 100 pour cent d'écriture
- 100 pour cent 4K
8K70/30
- 70 pour cent en lecture, 30 pour cent en écriture
- 100 pour cent 8K
8K (séquentiel)
- 100 pour cent de lecture ou 100 pour cent d'écriture
- 100 pour cent 8K
128K (séquentiel)
- 100 pour cent de lecture ou 100 pour cent d'écriture
- 100 pour cent 128K
4K 100 % lecture/écriture
Notre premier test mesure les performances aléatoires 4K. Dans ce test, le WD Gold a atteint 4,187 1,417 IOPS en lecture et 4,373 1,398 IOPS en écriture en SMB tout en affichant XNUMX XNUMX IOPS en lecture et XNUMX XNUMX IOPS en écriture iSCSI.
Pour une latence moyenne, le WD Gold a atteint 61.13 ms en lecture et 180.51 ms en écriture, et 58.52 ms en lecture/182.92 ms en écriture en SMB et iSCSI, respectivement.
À la latence maximale, les disques WD Gold 24 To ont enregistré 759.98 ms en lecture, 4,322.5 1,228.1 ms en écriture en SMB, 31,059 XNUMX ms en lecture et XNUMX XNUMX ms en écriture en iSCSI.
Concernant l'écart type, en SMB, le WD Gold a atteint 37.06 ms en lecture et 373.254 ms en écriture, tandis que l'iSCSI a enregistré 104.53 ms en lecture et 1,178 XNUMX ms en écriture.
8K 100 % lecture/écriture
Les deux disques ont montré des améliorations significatives des performances dans le test de lecture/écriture 8K par rapport au test de lecture/écriture 4K. Dans la configuration iSCSI, le WD Gold de 24 To affichait 157 95 IOPS en lecture, 215 228 IOPS en écriture pour SMB, XNUMX XNUMX IOPS en lecture et XNUMX XNUMX IOPS en écriture en iSCSI.
8K 70 pour cent de lecture 30 pour cent d'écriture
L'autre type de charge de travail que nous exécutons passe d'un scénario de lecture/écriture séquentielle pure à une charge de travail mixte, démontrant comment les performances évoluent de 2T/2Q à 16T/16Q. Ici, les performances en SMB ont montré un minimum de 589 IOPS (par coïncidence, c'était la même chose que le Seagate Exos de 24 To) et un maximum de 1,648 913 IOPS. Avec iSCSI, nous constatons un minimum de 1,923 IOPS et un maximum de XNUMX XNUMX IOPS, avec des performances généralement constantes.
Pour la latence moyenne, nous constatons des pics notables (dans SMB) allant jusqu'à 152,53 ms tout en descendant jusqu'à 6 ms à 2 Threads 4 Queue. Nous avons constaté des scores similaires avec iSCSI, avec une latence aussi faible que 6.78 ms et un maximum de 132.96 ms avec 16 Threads 2 Queue.
En ce qui concerne la latence maximale, WD Golds a montré une latence nettement plus élevée avec SMB par rapport à iSCSI (comme les disques Seagate). Plus précisément dans SMB, la latence a commencé à 5,395.85 5736.11 ms et a culminé à 16 16 ms dans la file d'attente 1167.74 threads 3,618.97, tandis que iSCSI a montré une latence plus faible et plus cohérente, commençant à XNUMX XNUMX ms et culminant à XNUMX XNUMX ms.
Dans la dernière partie du test 8k 70/30, nous avons observé que la configuration iSCSI fonctionnait mieux en écart type. Plus précisément, la configuration SMB a montré une latence aussi faible que 24.1 ms avec un maximum de 373.75 ms, tandis que l'iSCSI présentait une plage de 12.29 ms à 150.22 ms pendant le test.
128K 100 % lecture/écriture
Notre test final est le test 128K 100% lecture/écriture. Ce test implique de gros blocs et démontre la vitesse de transfert séquentielle la plus élevée que les disques peuvent atteindre. Pour les performances des PME, le WD Gold de 24 To a atteint 1.38 Go/s en lecture et 2.3 Go/s en écriture, atteignant 2.08 Go/s en lecture et 2.27 Go/s en écriture pour iSCSI.
Prix et garantie du WD Gold 24 To
Bien que les détails spécifiques des prix puissent varier en fonction du détaillant et des remises potentielles sur les achats en gros, le disque dur d'entreprise WD Gold 24 To coûte actuellement 630 $ dans la boutique Western Digital et est accompagné d'une garantie limitée de cinq ans.
Conclusion
La gamme WD Gold marque une étape importante dans l'expansion de la capacité de stockage avec son modèle 24 To. Proposé exclusivement dans l'interface SATA, ce disque haute capacité est conçu pour répondre aux besoins rapidement croissants en solutions de stockage plus étendues dans les environnements d'entreprise. Le modèle 24 To conserve les caractéristiques qui ont fait de la série Gold un choix fiable pour les entreprises, notamment une conception SATA CMR 7200 XNUMX tr/min remplie d'hélium. Dans le même temps, son efficacité énergétique représente une consommation électrique réduite par rapport aux modèles précédents.
En ce qui concerne les performances, le disque dur Gold de 24 To présente une image complexe dans notre suite de tests. Au cours de nos tests rigoureux avec huit disques dans notre configuration NAS QNAP standard, le modèle 24 To a indiqué une baisse des performances dans les tests de lecture 4K par rapport à son homologue 22 To, où nous avons noté une diminution marquée des vitesses de lecture et une latence plus élevée. Le Seagate Exos X24 l'a également surpassé.
Cela dit, l'intégration de technologies de pointe, des prix compétitifs et un solide ensemble de garanties en font un choix solide pour les entreprises qui envisagent d'étendre leurs capacités de stockage. La série WD Gold reste une option pertinente et fiable pour les centres de données modernes recherchant un équilibre entre performances, fiabilité et potentiel de croissance future.