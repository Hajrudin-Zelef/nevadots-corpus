Une caméra encore en bon état ne devrait pas devoir partir à la benne parce que l’entreprise change de console d’administration. L’ouverture de Meraki aux équipements Axis répond à ce problème : conserver une partie du parc vidéo tout en rapprochant son exploitation de celle du réseau.
Mais « visible dans Meraki » et « exploitable comme une caméra Meraki » ne signifient pas la même chose. L’intégration est réelle, mais son périmètre dépend de la licence, du modèle et des services Axis. Et un choix mérite d’être posé avant toute commande : garder son logiciel vidéo actuel, ou confier cette fonction à Meraki ?
Le billet Cisco du 16 septembre 2026 met cette convergence en avant. Ce n’est toutefois pas la date de première disponibilité : le guide d’intégration situe Essentials au 14 avril 2026 et Advantage au 23 juin. Voici ce qu’il faut examiner pour un projet, à partir des informations vérifiées le 18 septembre 2026.
Ce qu’il faut retenir
- Essentials ajoute une couche d’administration et de vidéo en direct. Le VMS local, c’est-à-dire le logiciel de gestion vidéo existant, peut rester en service.
- Advantage change le système d’exploitation vidéo du parc : Meraki devient le VMS exclusif. Le matériel peut être réemployé, mais pas nécessairement toute l’architecture qui l’entoure.
- 90 références sont répertoriées dans la matrice officielle, dont 87 pour les deux niveaux de licence. Les trois exceptions sont détaillées plus bas.
- Axis Cloud Connect reste indispensable à l’intégration. Une console commune ne signifie pas un fournisseur technique unique.
- Les prix, la disponibilité dans le pays concerné et la couverture fonctionnelle de chaque caméra doivent être validés pour le projet. La compatibilité d’un modèle n’est pas une garantie de parité avec toute la gamme MV.
Dashboard pour administrer, Vision pour exploiter les images
Le premier intérêt est organisationnel. L’équipe réseau et l’équipe sûreté peuvent travailler dans un environnement commun sans confondre leurs métiers. La première cherche pourquoi une caméra est hors ligne ; la seconde cherche ce qui s’est passé devant une porte.
Cisco positionne Meraki Dashboard comme l’interface de gestion des équipements et Meraki Vision comme celle de consultation et d’investigation vidéo. Le billet montre un mur vidéo mêlant des vues Axis et Cisco. L’accès à l’historique et aux exports relève du niveau Advantage, pas de la seule présence d’une caméra dans l’inventaire.
Ce qui est effectivement administrable
Le guide d’enrôlement Meraki détaille des actions allant au-delà d’un simple lien vers une autre console :
| Opération | Essentials | Advantage | 
|---|---|---|
| Vidéo en direct, état et connectivité | Oui | Oui | 
| Configuration et gestion du firmware | Oui | Oui | 
| Diagnostic : ping, redémarrage, traceroute, capture réseau | Oui | Oui | 
| Zoom, ouverture, mise au point ; orientation PTZ | Selon le matériel | Selon le matériel | 
| Historique, exports et enregistrement SD pilotés par Meraki | Non | Oui | 
| Stockage vidéo cloud de 30 jours | Non | Inclus | 
| Qualité/résolution vidéo, zones de mouvement et analyses | Non dans ce périmètre | Oui, selon les capacités prises en charge | 
| Conservation du VMS local en parallèle | Oui | Non : Meraki exclusif | 
La ligne sur l’enregistrement décrit ce que gère cette intégration, pas une interdiction pour le VMS existant d’enregistrer en mode Essentials.
Les analyses annoncées pour Advantage comprennent la recherche d’événements avec filtres personnes/véhicules, les alertes de mouvement et les cartes de chaleur. Pour l’historique, le guide demande une carte SD Axis ou l’activation du stockage cloud inclus. Vérifiez la capacité de stockage et les fonctions réellement disponibles sur chaque référence du pilote.
En pratique, il faut rédiger la recette à partir des gestes des utilisateurs : régler l’image d’une entrée, retrouver un événement, extraire une séquence, vérifier une panne. Un bouton présent dans une interface n’est utile que si l’opération aboutit sur les caméras du site, avec les bons droits.
Ne transposons pas non plus automatiquement les fonctions des caméras Cisco Meraki MV à un modèle Axis. Notre article sur le suivi inter-caméras dans Meraki Vision traite d’une fonction spécifique : la compatibilité Axis générale ne suffit pas à en garantir le bénéfice.
Essentials ou Advantage : conserver les caméras n’est pas conserver le VMS
Prenons un cas théorique : plusieurs bureaux disposent déjà de caméras Axis et d’un logiciel vidéo utilisé quotidiennement par les équipes. La DSI souhaite mieux superviser ces équipements depuis Meraki.
Avec Essentials, le projet porte d’abord sur l’exploitation technique. Il peut être pertinent de conserver les habitudes de consultation, les processus d’export et le stockage existants, tout en rapprochant le suivi des caméras de celui du réseau. Il faut néanmoins tester que les changements de configuration ou de firmware ne perturbent pas ces usages.
Avec Advantage, le projet devient une migration vidéo. L’exclusivité du VMS Meraki impose de vérifier ce qui est abandonné : intégrations métier, postes opérateurs, mécanismes d’alerte, procédures d’investigation et accès aux archives antérieures. Ne retirez pas un NVR simplement parce que le direct apparaît dans Vision : faites d’abord valider la continuité de service et le devenir des enregistrements existants.
Axis présente lui-même cette offre comme une approche hybride avec deux niveaux de licence. Le bon critère de choix n’est donc pas « quelle formule a le plus de fonctions ? », mais « quel fonctionnement voulons-nous conserver ou remplacer ? ».
Le coût doit être comparé sur la durée : abonnements, éventuelles cartes SD, capacité Internet, maintenance, formation et temps de migration. Réemployer les caméras peut éviter un remplacement matériel ; cela ne prouve pas à lui seul que le coût global baisse.
Axis Cloud Connect ne disparaît pas derrière Meraki
L’intégration relie les organisations Cisco et Axis par OAuth 2.0. Cisco décrit cette liaison cloud-to-cloud comme nécessaire à l’émission des commandes et à la fourniture des mises à jour. L’application embarquée sur la caméra s’appuie également sur l’écosystème ACAP d’Axis.
Autrement dit, Meraki fournit le point d’entrée commun, pas l’effacement de la plateforme Axis. Axis Cloud Connect demeure la plateforme de services permettant de connecter et de gérer les équipements avec des solutions partenaires.
Cette architecture appelle trois vérifications concrètes. Ce sont des points de qualification du projet, pas des pannes constatées sur cette solution :
- Identités et délégations. Qui possède les deux organisations ? Qui peut autoriser ou révoquer leur liaison ? Évitez que le départ d’un administrateur ou d’un prestataire rende les accès difficiles à reprendre.
- Continuité. Que reste-t-il accessible pendant une coupure Internet ? Que deviennent l’enregistrement, la consultation et les alertes ? Testez séparément le stockage local et le stockage cloud ; un écran d’inventaire ne répond pas à ces questions.
- Responsabilités. Qui prend en charge le matériel, le réseau, AXIS OS, l’intégration et le service vidéo ? Inscrivez le chemin d’escalade dans le dossier d’exploitation, avec un responsable de suivi côté client.
La FAQ prévoit une collaboration Cisco/Axis sur un même dossier de support, mais affiche des plages différentes : Cisco 24 h/24, 7 j/7 ; Axis 24 h/24, 5 j/7. Ce dispositif ne dispense pas de convenir de la prise en charge d’un incident vidéo critique hors heures ouvrées.
La sécurité ne se résume pas non plus à la présence du mot « cloud ». Comptes nominatifs, authentification multifacteur, droits d’export limités et retrait des accès inutiles restent des exigences de recette. Faites confirmer l’hébergement, les conditions de traitement des images et les engagements contractuels applicables. Les 30 jours inclus sont une caractéristique commerciale, pas une durée de conservation à appliquer automatiquement.
Comment se passe l’enrôlement ?
Le parcours documenté repose sur des comptes administrateurs Meraki et Axis, une licence, un modèle compatible et un accès Internet autorisé vers les services concernés. Le minimum général indiqué est AXIS OS 9.8, avec AXIS OS 11 ou ultérieur recommandé ; il faut confirmer la version requise pour le modèle et le niveau retenus.
Après activation de l’intégration et autorisation OAuth, trois méthodes sont proposées : découverte via des switches Cisco administrés dans Dashboard, numéro de série et clé propriétaire OAK, ou récupération d’appareils déjà dans Axis Cloud Connect. La méthode OAK/série nécessite aussi l’activation d’O3C ou l’action « one-click » sur l’équipement.
Un switch Cisco facilite donc la découverte, mais n’est pas une condition universelle de raccordement. Pour un parc hétérogène, commencez par distinguer les caméras automatiquement découvertes de celles à enrôler autrement.
Pour le pilote, préparez un inventaire avec la référence exacte, le firmware, le site, le mode d’alimentation, le stockage et les dépendances applicatives. Traitez les clés OAK et les identifiants locaux comme des secrets, pas comme des colonnes anodines d’un tableur partagé. Enfin, planifiez les mises à jour : une évolution d’AXIS OS peut nécessiter un redémarrage et interrompre momentanément la vidéo.
Quels produits Axis sont compatibles avec Meraki ?
La matrice Cisco Meraki, consultée le 18 septembre 2026, comprend les 90 références ci-dessous. La liste est regroupée pour faciliter la lecture, sans étendre la compatibilité à d’autres variantes d’une même famille. Toutes les références portent la marque AXIS.
87 références compatibles Essentials et Advantage
| Groupe de références | Modèles explicitement listés | 
|---|---|
| M42 | M4227-LVE, M4228-LVE | 
| M43 | M4317-PLR, M4317-PLVE, M4318-PLR, M4318-PLVE, M4327-P, M4328-P | 
| P1387 | P1387, P1387-B, P1387-BE, P1387-LE | 
| P1388 | P1388, P1388-B, P1388-BE, P1388-LE | 
| P14 | P1467-LE, P1468-LE, P1468-XLE | 
| P15 | P1518-E, P1518-LE | 
| P326 | P3267-LV, P3267-LVE, P3268-LV, P3268-LVE, P3268-SLVE | 
| P327 | P3275-LV, P3275-LVE, P3277-LV, P3277-LVE, P3278-LV, P3278-LVE | 
| P328 | P3285-LV, P3285-LVE, P3287-LV, P3287-LVE, P3288-LV, P3288-LVE | 
| P37 et P38 | P3735-PLE, P3737-PLE, P3738-PLE, P3747-PLVE, P3748-PLVE, P3827-PVE | 
| P47 | P4705-PLVE, P4707-PLVE, P4708-PLVE | 
| P91 | P9117-PV | 
| Q1656 | Q1656, Q1656-B, Q1656-BE, Q1656-BLE, Q1656-DLE, Q1656-LE | 
| Q1686 et Q17 | Q1686-DLE, Q1728, Q1728-LE | 
| Q18 | Q1800-LE, Q1800-LE-3, Q1805-LE, Q1806-LE, Q1808-LE, Q1809-LE | 
| Q19 | Q1961-TE, Q1961-XTE, Q1971-E, Q1972-E | 
| Q21 | Q2101-TE, Q2111-E, Q2112-E | 
| Q353 | Q3536-LVE, Q3538-LVE, Q3538-SLVE | 
| Q354 et Q355 | Q3546-LVE, Q3548-LVE, Q3556-LVE, Q3558-LVE | 
| Q36 et Q38 | Q3626-VE, Q3628-VE, Q3839-PVE | 
| Q48 et Q60 | Q4809-PVE, Q6020-E | 
| Q63 | Q6300-E, Q6355-LE, Q6358-LE | 
| Q93 et XFQ | Q9307-LV, XFQ1656 | 
Trois références compatibles Essentials uniquement
| Modèle | Essentials | Advantage | 
|---|---|---|
| P1245 Mk II | Oui | Non | 
| P1265 Mk II | Oui | Non | 
| P1275 Mk II | Oui | Non | 
Ces trois modèles utilisent le SoC CV25 ; les 87 autres références de la matrice reposent sur ARTPEC-8 ou ARTPEC-9, avec parfois deux processeurs. Ce constat n’est pas une règle de compatibilité à extrapoler : un modèle absent ne devient pas éligible parce qu’il utilise une puce similaire.
Même précaution pour les usages spécialisés. Une référence thermique, panoramique ou radar-vidéo présente dans la liste n’implique pas que toutes ses données et fonctions soient exposées dans Vision. Demandez une démonstration des fonctions nécessaires sur la référence exacte, puis consignez le résultat.
Licences : les points à verrouiller avant le devis
Chaque appareil Axis administré exige une licence Cisco spécifique, distincte des licences MV. Les modes co-term, Enterprise Agreement et Subscription sont prévus. Surtout, une organisation ne peut avoir qu’un seul niveau Axis à la fois ; un changement en cours de période nécessite de passer par l’équipe commerciale, sans bascule autonome garantie. Ces conditions figurent dans le guide d’intégration et sa FAQ.
Cela mérite une attention particulière si votre inventaire mélange les trois modèles limités à Essentials et des caméras destinées à Advantage. Ne construisez pas le budget sur l’hypothèse d’un choix libre caméra par caméra dans la même organisation.
Demandez une proposition qui précise le niveau, la durée, les quantités, le renouvellement et les conditions de sortie. Faites également confirmer la disponibilité régionale du service et les modalités de stockage. Nous ne donnons pas ici de tarif : les sources consultées ne permettent pas d’établir un prix public applicable à un projet français.
Plan d’action recommandé : prouver l’usage avant la généralisation
Chez BoucheCousue, nous recommandons de qualifier un petit échantillon représentatif avant de décider pour tout le parc. Voici une grille de recette, à adapter au besoin réel.
| Test proposé | Preuve attendue | 
|---|---|
| Caméra fixe, panoramique ou PTZ représentative du parc | Direct exploitable et réglages utiles accessibles | 
| Recherche puis export d’une séquence avec Advantage | Fichier réellement récupéré et relu, avec horodatage et droits vérifiés | 
| Coexistence du VMS avec Essentials | Usages habituels du logiciel existant toujours opérationnels | 
| Coupure Internet contrôlée puis rétablissement | Comportement mesuré de l’enregistrement, de la consultation et des alertes | 
| Maintenance et redémarrage | Interruption mesurée, reprise vérifiée et responsable identifié | 
| Départ d’un administrateur | Accès révoqué sans perte de maîtrise des organisations | 
| Sortie de la solution | Procédure documentée pour les équipements, les archives et les licences | 
N’attribuez pas un gain de temps au seul rapprochement des interfaces. Comparez le temps nécessaire pour résoudre une panne ou retrouver une séquence avant et après le pilote. C’est ce résultat qui permet de décider si la convergence simplifie vraiment le travail.
FAQ
Une caméra Axis compatible devient-elle une caméra MV ?
Non. Elle reste un équipement Axis intégré à l’environnement Meraki, avec son matériel, son firmware et ses capacités propres. L’intégration ne démontre pas une identité fonctionnelle avec une MV.
Peut-on conserver son enregistreur actuel ?
Essentials permet la coexistence avec le VMS local. Advantage impose Meraki comme VMS exclusif pour les appareils concernés. Il faut donc examiner les archives et les intégrations avant de retirer l’ancien système.
Faut-il remplacer les switches non-Cisco ?
Pas systématiquement. L’enrôlement par numéro de série et OAK couvre notamment les appareils raccordés à d’autres switches. Le réseau doit toutefois fournir l’alimentation et la connectivité nécessaires.
Un modèle compatible garantit-il toutes les fonctions vidéo ?
Non. La liste valide une compatibilité par niveau ; elle ne détaille pas chaque fonction spécialisée. La recette doit rester centrée sur les usages attendus et la référence exacte.
Notre lecture : une vraie intégration, avec un choix d’architecture
L’ouverture aux caméras Axis ne se limite pas à rassembler des raccourcis dans une console. Elle peut rapprocher la gestion du réseau, celle des équipements vidéo et le traitement des incidents.
La décision essentielle reste néanmoins très concrète : Essentials pour ajouter de la gestion en conservant le VMS ; Advantage pour confier l’exploitation vidéo à Meraki. Dans les deux cas, gardons les dépendances Axis visibles dans le projet et évaluons le service rendu, pas seulement la qualité de la démonstration.
Vous souhaitez savoir quelles caméras conserver et quel scénario serait pertinent pour vos sites ? Contactez BoucheCousue pour cadrer l’inventaire, les licences et un pilote avec des critères de validation précis.
Informations vérifiées le 18 septembre 2026. Analyse documentaire : aucun test matériel de cette intégration n’a été réalisé pour cet article.
Les deux visuels proviennent du billet Cisco cité et restent attribués à Cisco. Ils illustrent l’interface du constructeur, pas un déploiement BoucheCousue.