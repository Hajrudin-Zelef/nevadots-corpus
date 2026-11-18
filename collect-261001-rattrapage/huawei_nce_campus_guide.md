# Guide technique ultra-complet — iMaster NCE-Campus (Huawei)

**Version :** 1.0 — Septembre 2026
**Public cible :** Zelef, chef de service systèmes & énergies — gestionnaire d'un parc Huawei (AP361, AP761, S310, AR720, USG6000, eKit), connaisseur d'eSight.
**Objectif :** comprendre iMaster NCE-Campus en profondeur : positionnement stratégique, architecture, licences, installation, onboarding, gestion filaire/WLAN, automatisation, supervision, sécurité, migration eSight → NCE, limites honnêtes.
**Convention du guide :** toute valeur tarifaire ou de dimensionnement non publique est marquée `à vérifier auprès du partenaire / de la documentation officielle`. Les informations issues de documentation Huawei accessible (brochures, Product Overview V300R022C10, Monitoring and O&M V300R020C10, MSP License Usage Guide V300R022C00/V300R024C00, datasheet NCE-CampusInsight V100R025C00) sont utilisées à titre de description fonctionnelle ; toujours recouper avec la fiche technique de la version cible avant un projet réel.

---

## Table des matières (sections numérotées)

1. Ce qu'est iMaster NCE-Campus — définition officielle
2. La famille iMaster NCE : NCE-Campus, NCE-Fabric, NCE-WAN, NCE-IP
3. Pourquoi Huawei pousse NCE côté campus — la logique produit
4. ADN : Autonomous Driving Network — ce que ça veut dire concrètement
5. Relation avec eSight — le positionnement officiel de Huawei
6. eSight reste-t-il valable ? — réponse honnête
7. Fin de vie eSight — ce qui est connu et ce qui ne l'est pas (à vérifier)
8. iMaster NCE-Campus vs Huawei eKit — la question du petit site
9. Qui a besoin de NCE-Campus — profils types
10. Qui n'en a PAS besoin — quand eSight ou eKit suffisent
11. Architecture globale — vue à 10 000 mètres
12. Les trois couches logiques : management, control, analysis
13. Le contrôleur SDN au cœur du système
14. Southbound interfaces : comment NCE parle aux équipements
15. NETCONF/YANG — le protocole de configuration structurée
16. SNMP — la télémétrie et le legacy
17. CAPWAP — la gestion des AP AirEngine
18. HTTP/2, HTTPS et autres canaux southbound
19. Base de données et stockage — que garde NCE ?
20. iMaster NCE-CampusInsight — l'analyseur (composant distinct)
21. Le composant d'authentification — déploiement distribué
22. Haute disponibilité — modes de déploiement
23. Dimensionnement : petit site (ordre de grandeur)
24. Dimensionnement : site moyen (ordre de grandeur)
25. Dimensionnement : grand campus / multi-sites (ordre de grandeur)
26. Modèle de licence — principe général (device-day)
27. Licence plateforme vs licence équipement
28. Licences par équipement géré — comment elles se consomment
29. Période de grâce et comportement en cas de dépassement
30. Mode MSP — multi-tenant et licence commune
31. iMaster NCE-CampusInsight — souscriptions associées
32. Licence de 60 devices incluse dans eSight — comparaison de logique
33. Coût — ordre de grandeur et pourquoi on ne donne pas de prix ici
34. Checklist avant achat de licences
35. Architecture matérielle requise — serveur / VM
36. Système d'exploitation supporté
37. Prérequis réseau (ports, flux, DNS, NTP)
38. Prérequis d'installation — checklist complète
39. Obtenir le package d'installation
40. Installation pas à pas — grandes étapes
41. Premier login — comptes par défaut et politique
42. Configuration initiale — assistant de démarrage
43. Création de l'organisation : tenant, régions, sites
44. Intégration DNS / NTP / certificats
45. Sauvegarde initiale — avant toute chose
46. Checklist post-installation
47. Échecs d'installation fréquents et remèdes
48. Mise à jour du contrôleur lui-même
49. Notion de site dans NCE-Campus
50. Les équipements gérés : S310, AP361, AP761, AR720, USG6000
51. ZTP — principe du zero-touch provisioning
52. ZTP via DHCP (option 148/option personnalisée) — détail
53. ZTP via e-mail d'activation — détail
54. ZTP via scan (QR / code) — détail
55. Onboarding manuel — quand le ZTP n'est pas possible
56. Ajouter un S310 — procédure détaillée
57. Ajouter un AP361 — procédure détaillée
58. Ajouter un AP761 — procédure détaillée
59. Ajouter un AR720 — procédure détaillée
60. Vérifier qu'un équipement est bien géré — checklist
61. États des équipements dans NCE (normal, alarme, hors ligne, non enregistré)
62. Templates de configuration — concepts
63. Variables dans les templates — paramétrage par site/équipement
64. VLAN — conception et déploiement via NCE
65. Politiques d'accès filaire — 802.1X : principes
66. 802.1X : rôles (supplicant, authentificateur, serveur)
67. 802.1X : méthodes EAP courantes
68. MAC-auth et portail captif — les alternatives
69. Autorisation dynamique : VLAN, ACL, QoS par utilisateur
70. QoS filaire — modèles et déploiement
71. Gestion WLAN centralisée — principes
72. Le modèle WAC + AP : comment NCE pilote le Wi-Fi
73. Profils radio — conception (canaux, puissance, bandes)
74. SSID — conception et bonnes pratiques
75. Planification radio : RRM, DCA, TPC
76. Roaming — 802.11k/v/r et pratiques Huawei
77. Sécurité WLAN : WPA2/WPA3, 802.1X Wi-Fi, PPSK
78. NCE vs eKit pour le WLAN — quand NCE apporte un plus
79. Invités et BYOD — portail captif via NCE
80. IoT sur le WLAN — segmentation
81. Automation — vue d'ensemble
82. ZTP détaillé — séquence complète d'un onboarding automatique
83. Templates et variables — guide pratique
84. Déploiement en masse — vagues et fenêtres de maintenance
85. Mise à jour firmware en masse — procédure
86. Politique de mise à jour : manuelle vs automatique
87. Rollback — revenir en arrière après un déploiement raté
88. API northbound RESTful — principes
89. Authentification API : tokens (POST /controller/v2/tokens)
90. Cas d'usage API : topologie, métriques, alarmes
91. Intégration avec des outils tiers (Zabbix, GLPI, scripts)
92. Assurance et supervision — vue d'ensemble
93. Monitoring temps réel — tableaux de bord
94. Cartographie réseau (digital map) — topologie
95. Détection d'anomalies — ce que fait l'IA/ML
96. Analyse de cause racine (root cause analysis)
97. Rapports — types et planification
98. Alertes — configuration mail/SMS
99. SLA management — NQA et mesures actives
100. Maintenance des équipements depuis NCE
101. Comptes et rôles — modèle RBAC
102. Rôles prédéfinis et rôles personnalisés
103. Authentification des administrateurs — locale, RADIUS, LDAP/AD
104. Journal d'audit — traçabilité des actions
105. Durcissement du contrôleur — checklist
106. Certificats — gestion et renouvellement
107. Sauvegardes NCE — stratégie
108. Restauration — procédure et tests
109. Sécurité des flux southbound
110. Stratégie de migration eSight → NCE — vue d'ensemble
111. Coexistence eSight + NCE — mode recommandé
112. Migration par site — méthode pas à pas
113. Ce qui est repris de eSight — inventaire
114. Ce qui N'est PAS repris — limites honnêtes
115. Checklist de migration complète
116. Risques de la migration et mitigations
117. Planning type de migration (exemple 3 sites)
118. Retour en arrière — plan B
119. Comparaison eSight vs NCE-Campus — tableau général
120. Comparaison détaillée par fonction
121. Coût comparé — TCO sur 5 ans (méthode)
122. Complexité comparée
123. Cas d'usage : quand choisir quoi — arbre de décision
124. Cas pratique 1 — L'onboarding ZTP qui échoue (DHCP)
125. Cas pratique 2 — Un AP361 qui ne remonte pas dans NCE
126. Cas pratique 3 — Un AP761 extérieur qui reste hors ligne
127. Cas pratique 4 — Un S310 qui boucle en enregistrement
128. Cas pratique 5 — Un AR720 qui ne télécharge pas sa config
129. Cas pratique 6 — Un template qui casse la production
130. Cas pratique 7 — Licence dépassée : que se passe-t-il ?
131. Cas pratique 8 — Mise à jour firmware massive qui échoue à mi-parcours
132. Cas pratique 9 — Roaming Wi-Fi dégradé après migration NCE
133. Cas pratique 10 — 802.1X qui rejette tous les utilisateurs un lundi matin
134. Cas pratique 11 — Le contrôleur NCE ne répond plus
135. Cas pratique 12 — Faux positifs d'alarmes après un orage
136. Cas pratique 13 — Un site distant perd le lien vers NCE
137. Cas pratique 14 — Conflit entre config manuelle CLI et NCE
138. Cas pratique 15 — Certificat expiré : tout s'arrête
139. Cas pratique 16 — Restauration après sinistre du serveur NCE
140. Cas pratique 17 — Montée en charge : NCE sature
141. Cas pratique 18 — Intégration RADIUS/LDAP qui casse l'authentification admin
142. Cas pratique 19 — eKit et NCE se disputent un AP
143. Cas pratique 20 — Audit de sécurité : durcir en urgence
144. Cas pratique 21 — Migration eSight → NCE d'un site pilote
145. Cas pratique 22 — Rapport mensuel pour la direction
146. Cas pratique 23 — Supervision croisée NCE + Zabbix
147. Cas pratique 24 — PPSK pour les invités d'un hôtel
148. Cas pratique 25 — Coupure électrique et reprise (lien onduleurs)
149. Limite 1 — Courbe d'apprentissage
150. Limite 2 — Dépendance au contrôleur (single point of management)
151. Limite 3 — Coût des licences
152. Limite 4 — Maturité sur petit site (eKit suffit)
153. Limite 5 — Écosystème et documentation
154. Limite 6 — Dépendance au support Huawei/partenaire
155. Synthèse honnête — faut-il y aller ?
156. Quiz — 10 questions
157. Quiz — réponses détaillées
158. Glossaire
159. Pour aller plus loin — ressources et prochaines étapes

---

# PARTIE 1 — POSITIONNEMENT

## 1. Ce qu'est iMaster NCE-Campus — définition officielle

iMaster NCE-Campus est le système de gestion et de contrôle de réseau campus de Huawei, qualifié par le constructeur de « système de gestion et de contrôle de réseau autonome (autonomous driving network) de nouvelle génération pour les réseaux campus ». Concrètement, c'est une plateforme logicielle qui **intègre trois fonctions** historiquement séparées :

- **Management (gestion)** : inventaire, configuration, supervision, alarmes, rapports — le rôle classique d'un NMS comme eSight.
- **Control (contrôle)** : pilotage SDN des équipements via des protocoles structurés (NETCONF/YANG notamment), déploiement automatisé de services, orchestration des politiques.
- **Analysis (analyse)** : collecte massive de données (télémétrie), big data et algorithmes de machine learning pour la visibilité d'expérience, la prédiction de pannes et l'analyse de cause racine.

La promesse marketing — à prendre avec le recul d'un exploitant — est la « full-lifecycle automation » : de la planification à la construction, de l'exploitation à l'optimisation et à la sécurité, tout le cycle de vie du réseau campus serait piloté depuis un point unique, avec une réduction annoncée des coûts OPEX/O&M (Huawei évoque une division par deux des coûts d'O&M dans ses argumentaires — chiffre marketing, à considérer comme un objectif, pas une garantie).

Techniquement, iMaster NCE-Campus se déploie soit **on-premises** (sur serveurs/VM du client), soit en **cloud** (modèle MSP / cloud public Huawei selon les offres commerciales), ce qui ouvre des modèles économiques variés : achat classique ou souscription.

Point important pour Zelef : NCE-Campus gère les familles d'équipements qu'il connaît déjà — **switches CloudEngine (dont S310), routeurs NetEngine AR (dont AR720), AP AirEngine (dont AP361 et AP761), firewalls USG (dont USG6000)** — via une gestion unifiée filaire + sans-fil + WAN de branche.

## 2. La famille iMaster NCE : NCE-Campus, NCE-Fabric, NCE-WAN, NCE-IP

« iMaster NCE » n'est pas un produit unique mais une **famille de contrôleurs** partageant une base technologique commune (SDN, big data, API ouvertes). Il faut les distinguer clairement :

| Membre de la famille | Périmètre | Rôle |
|---|---|---|
| **iMaster NCE-Campus** | Réseaux campus et agences/branches | Gestion unifiée LAN filaire, WLAN, WAN de branche (SD-WAN), authentification des utilisateurs |
| **iMaster NCE-Fabric** | Data centers | Automatisation du fabric DC (VXLAN/EVPN), gestion du cycle de vie des commutateurs de data center |
| **iMaster NCE-WAN** | Réseaux WAN / transport | Contrôleur SD-WAN et orchestration WAN (selon la documentation SD-WAN Huawei, NCE-Campus peut aussi gérer le SD-WAN et « sera le contrôleur SD-WAN mainstream à l'avenir » — formulation Huawei) |
| **iMaster NCE-IP** | Réseaux IP d'opérateurs / entreprises | Gestion des réseaux IP à grande échelle (selon déclinaisons commerciales) |

À cela s'ajoute **iMaster NCE-CampusInsight**, l'analyseur intelligent du campus : plateforme d'analyse big data/ML qui numérise l'expérience utilisateur (par utilisateur, par application, à chaque instant). C'est un **composant indépendant** (licence et souscription séparées), qui se connecte à NCE-Campus.

Pour un chef de service systèmes & énergies comme Zelef, seul **NCE-Campus** (éventuellement + CampusInsight) est pertinent : NCE-Fabric concerne les data centers, NCE-WAN les opérateurs.

## 3. Pourquoi Huawei pousse NCE côté campus — la logique produit

Huawei pousse iMaster NCE-Campus pour des raisons à la fois techniques et commerciales qu'il faut comprendre pour négocier sereinement :

1. **Fin du paradigme CLI + scripts** : Huawei constate — à juste titre — que les réseaux campus deviennent trop complexes (BYOD, IoT, Wi-Fi 6/7, cloud, vidéo) pour être gérés en CLI équipement par équipement. NCE est la réponse « intent-based » : on déclare l'intention (qui accède à quoi, avec quelle qualité), le contrôleur traduit en configurations.
2. **Convergence LAN/WLAN/WAN** : historiquement, le filaire (switches), le Wi-Fi (contrôleur WAC/AC) et les branches (routeurs) se géraient avec des outils différents. NCE-Campus unifie les trois — c'est son argument différenciant principal face à eSight, qui supervise mais n'orchestre pas.
3. **Modèle économique récurrent** : les licences NCE (device-day, souscriptions) génèrent des revenus récurrents, contrairement à la licence perpétuelle classique. C'est une logique d'abonnement assumée par tout le secteur (Cisco DNA, Aruba Central, Juniper Mist font pareil).
4. **IA comme différenciateur marketing** : CampusInsight, l'optimisation radio par ML, la prédiction de pannes — Huawei investit massivement dans l'« autonomous driving network » pour se différencier de Cisco/Aruba.
5. **Cloud et MSP** : NCE permet aux partenaires/intégrateurs de proposer du « network as a service » managé multi-tenant — nouveau canal de vente pour Huawei.

Conséquence pour l'acheteur : NCE n'est pas qu'un « eSight amélioré », c'est un **changement de modèle** (pilotage centralisé + abonnement). Il faut l'évaluer comme tel, pas comme une simple mise à jour.

## 4. ADN : Autonomous Driving Network — ce que ça veut dire concrètement

« Autonomous Driving Network » (ADN) est le concept marketing de Huawei pour les réseaux qui se gèrent (presque) seuls. Derrière le slogan, il y a des fonctions réelles, classées par niveau d'autonomie :

- **Niveau 1-2 (assistance)** : tableaux de bord unifiés, topologie automatique, alarmes corrélées. C'est ce que fait déjà un bon NMS.
- **Niveau 3 (automatisation conditionnelle)** : ZTP (zero-touch provisioning), templates de configuration, mise à jour firmware en masse, déploiement de politiques 802.1X en un clic. C'est le cœur utile de NCE-Campus pour une équipe d'exploitation.
- **Niveau 4 (prédiction)** : CampusInsight analyse la télémétrie pour prédire les pannes (ex : port en dégradation, interférences Wi-Fi croissantes), recommande des optimisations radio. Utile, mais à valider sur le terrain : les faux positifs existent.
- **Niveau 5 (autonomie complète)** : le réseau se répare seul. **N'existe pas en production** à ce jour — à considérer comme une vision, pas une fonctionnalité achetable.

En pratique pour Zelef : attendre de NCE les niveaux 1 à 3 (réels et matures), considérer le niveau 4 comme un bonus à évaluer en pilote, ignorer le niveau 5.

## 5. Relation avec eSight — le positionnement officiel de Huawei

eSight est la plateforme historique de gestion unifiée Huawei (Unified Network Management Platform) : supervision multi-vendeurs (via SNMP), gestion des configurations, alarmes, rapports, topologie. NCE-Campus n'est **pas** présenté par Huawei comme un simple successeur versionné d'eSight, mais comme une **nouvelle génération** fondée sur le SDN.

Le positionnement officiel, tel qu'il ressort de la documentation :

- eSight = **NMS traditionnel** : il supervise (SNMP), configure (CLI/SNMP), génère des rapports. Il ne fait pas d'orchestration SDN, pas de ZTP avancé, pas d'analyse IA poussée.
- NCE-Campus = **manager + contrôleur + analyseur** : il supervise ET pilote (NETCONF/YANG), automatise le déploiement (ZTP, templates), analyse (big data/ML via CampusInsight).
- Huawei vend NCE-Campus comme la plateforme cible pour les nouveaux projets campus, eSight restant la plateforme installée chez des milliers de clients.

Il n'y a pas, dans la documentation publique consultée, d'outil de migration automatique eSight → NCE : la migration est un **re-projet** (ré-onboarding des équipements, re-création des politiques), pas un upgrade. C'est un point structurant de la stratégie (voir Partie 11).

## 6. eSight reste-t-il valable ? — réponse honnête

**Oui.** Sans ambiguïté :

- eSight reste **commercialisé** (brochure eSight 23.1 datée de 2024, références S-Part actives : 88034GED plateforme, 88034GEE licence network/device, 88034GEF licence WLAN/AP, etc., avec souscriptions SnS annuelles).
- eSight couvre des périmètres que NCE-Campus ne couvre pas ou moins bien : supervision **multi-vendeurs** (NCE est quasi exclusivement Huawei), gestion des onduleurs et équipements d'énergie via SNMP générique (point crucial pour Zelef !), PON/OLT/ONT, data center, vidéosurveillance.
- Pour un parc mixte ou une supervision d'infrastructure énergie (UPS, climatisation, groupes électrogènes via SNMP), eSight reste souvent **plus pertinent** que NCE.
- Le coût d'eSight (licence plateforme + licences par device/AP) est sans commune mesure avec un projet NCE complet (serveurs, licences device-day, services d'intégration).

La bonne question n'est donc pas « eSight ou NCE » mais « **pour quel périmètre** » : NCE pour l'automatisation du campus Huawei neuf, eSight pour la supervision transverse/multi-vendeurs/énergie.

## 7. Fin de vie eSight — ce qui est connu et ce qui ne l'est pas (à vérifier)

Honnêteté requise sur ce point sensible :

- **Aucune date officielle de fin de vie (EOM/EOS) d'eSight n'a été trouvée** dans les sources publiques consultées pour ce guide (septembre 2026). Huawei met à disposition un outil « EOM, EOFS & EOS Date Query » sur e.huawei.com pour vérifier par produit — c'est la source faisant foi.
- Les rumeurs de « mort d'eSight » circulent depuis des années dans la communauté ; elles sont **non confirmées** par de la documentation officielle accessible.
- Ce qui est observable : l'effort R&D et marketing de Huawei va très majoritairement vers NCE (et vers eKit pour les PME). eSight évolue lentement (versions 23.x).
- **Recommandation** : avant tout investissement eSight significatif, interroger le partenaire Huawei sur la roadmap officielle et exiger un écrit. Marquer ce point **à vérifier** à chaque comité de pilotage annuel.

En résumé : eSight n'est pas mort, mais ce n'est plus le produit d'avenir de Huawei. On investit dessus pour **maintenir**, on investit sur NCE pour **transformer**.

## 8. iMaster NCE-Campus vs Huawei eKit — la question du petit site

Huawei eKit est la plateforme cloud légère pour PME (petits switches, AP eKit, gestion via app/cloud). La question se pose pour les petits sites de Zelef :

| Critère | eKit | iMaster NCE-Campus |
|---|---|---|
| Cible | PME, petits sites (< ~50 équipements, ordre de grandeur) | Campus moyens/grands, multi-sites |
| Modèle | Cloud eKit, simple, app mobile | Contrôleur on-prem/cloud, complet |
| Coût | Faible / inclus | Licences device-day + infra + intégration |
| Fonctionnalités | Gestion de base, SSID, VLAN simples | ZTP avancé, 802.1X, CampusInsight, API, multi-tenant |
| Compétences requises | Faibles | Équipe formée, projet d'intégration |

**Règle pratique** : un site avec quelques S310 et 2-3 AP361/AP761 en gestion simple → **eKit suffit** et NCE serait du surdimensionnement coûteux. NCE se justifie quand il faut du 802.1X, du multi-site centralisé, de l'automatisation ou de l'analyse avancée. Attention au piège : un AP géré par eKit doit être **désenrôlé** avant de passer sous NCE (voir cas pratique 19).

## 9. Qui a besoin de NCE-Campus — profils types

NCE-Campus est pertinent quand **au moins deux** de ces conditions sont réunies :

1. **Parc Huawei campus significatif** : > 30-50 équipements administrables (switches, AP, AR) — ordre de grandeur indicatif.
2. **Multi-sites** : plusieurs agences/branches à administrer depuis un point central, avec des intervenants locaux peu qualifiés réseau (le ZTP brille ici).
3. **Exigence d'authentification** : 802.1X filaire/Wi-Fi, invités, BYOD, segmentation IoT — la gestion centralisée des politiques devient vite indispensable.
4. **Équipe réduite** : peu d'administrateurs pour un parc qui grandit — l'automatisation compense.
5. **Renouvellement** : projet de refresh du campus = fenêtre idéale pour introduire NCE (greenfield partiel).
6. **Exigence de visibilité** : besoin de SLA, de rapports d'expérience utilisateur, d'analyse de cause racine rapide (contexte exigeant : industrie, santé, éducation).

Pour Zelef : si son entreprise multi-sites déploie du S310/AP361/AP761/AR720 de façon récurrente avec une petite équipe, le profil correspond.

## 10. Qui n'en a PAS besoin — quand eSight ou eKit suffisent

À l'inverse, NCE-Campus est **déconseillé** (ou prématuré) si :

- Le parc est **petit et stable** (< 20 équipements, peu de mouvements) : eKit ou la gestion locale + eSight suffisent.
- Le parc est **multi-vendeurs** : NCE ne gère pratiquement que du Huawei ; eSight ou un NMS tiers (Zabbix, etc.) restent nécessaires en transverse.
- Le besoin principal est la **supervision d'énergie** (onduleurs, etc.) : eSight/SNMP ou une GTE dédiée sont plus adaptés — NCE n'est pas un superviseur d'infrastructure technique.
- **Aucune ressource projet** : NCE exige un vrai projet (dimensionnement, licences, formation, intégration). Sans sponsor ni budget, c'est un échec programmé.
- L'équipe est **débordée par l'exploitation courante** : ajouter un contrôleur SDN sans dégager du temps de montée en compétence = usine à gaz.

Le courage de dire non à NCE sur un petit site est une qualité d'architecte, pas un aveu de faiblesse.

---

# PARTIE 2 — ARCHITECTURE

## 11. Architecture globale — vue à 10 000 mètres

L'architecture logique d'iMaster NCE-Campus s'organise en couches, du terrain vers l'utilisateur :

```
+--------------------------------------------------------------+
|  COUCHE APPLICATION (portails, API NBI, applis tierces)      |
|  Portail admin / Portail tenant / Self-service / VAS store   |
+--------------------------------------------------------------+
|  iMaster NCE-Campus                                          |
|  +------------+  +------------+  +------------------------+  |
|  |  Manager   |  | Controller |  | Analyzer (base)        |  |
|  | (gestion)  |  | (SDN)      |  | + CampusInsight (opt.) |  |
|  +------------+  +------------+  +------------------------+  |
+--------------------------------------------------------------+
|  Southbound : NETCONF/YANG | SNMP | HTTP/2 | HTTPS | TCP ...  |
+--------------------------------------------------------------+
|  COUCHE RÉSEAU : Switches | Routeurs AR | AP/WAC | USG | ...  |
+--------------------------------------------------------------+
```

Deux compléments distribués existent pour les architectures multi-sites :

- **Composant d'authentification** : déployable (jusqu'à 20 instances selon la documentation V300R022C10) sur des branches distantes pour assurer l'authentification locale même si le lien vers le contrôleur central est dégradé.
- **iMaster NCE-CampusInsight** : l'analyseur avancé, composant indépendant connecté à NCE-Campus (redirection one-click via proxy de service depuis NCE).

Cette architecture « manager + controller + analyzer » intégrés est la différence fondamentale avec eSight (manager seul).

## 12. Les trois couches logiques : management, control, analysis

**Management (gestion)** : c'est la partie « NMS » — inventaire des équipements, gestion des configurations et des firmwares, topologie, alarmes, rapports, gestion des licences, portails multi-tenant. Si vous venez d'eSight, c'est la partie la plus familière.

**Control (contrôle)** : la partie SDN — le contrôleur traduit des **intentions de service** (ex : « les utilisateurs du VLAN 20 ont 100 Mbps garantis ») en configurations poussées aux équipements via NETCONF/YANG. Il orchestre aussi le ZTP, les templates, les politiques d'accès et de QoS. C'est ce qui n'existe pas dans eSight.

**Analysis (analyse)** : la collecte et l'exploitation des données — télémétrie temps réel, statistiques d'expérience par utilisateur/application, détection d'anomalies, aide au diagnostic. La version de base est intégrée à NCE-Campus ; la version avancée (ML, prédiction) est apportée par **CampusInsight**.

L'intérêt de l'intégration : quand une alarme remonte (management), le contrôleur connaît la configuration déployée (control) et l'analyseur sait si c'est un vrai problème d'expérience (analysis) — la corrélation est native, pas bricolée entre trois outils.

## 13. Le contrôleur SDN au cœur du système

Le contrôleur SDN est le cerveau : il maintient une **vue centralisée** de l'état du réseau (topologie, configurations, politiques) et pilote les équipements en mode « intent-based ».

Concrètement, l'administrateur ne configure plus chaque switch en CLI : il définit dans NCE des **sites**, des **templates**, des **politiques** (VLAN, SSID, 802.1X, QoS). Le contrôleur :

1. Valide la cohérence (conflits, ressources).
2. Génère les configurations effectives par équipement (en substituant les variables des templates).
3. Les pousse via NETCONF (edit-config) ou les protocoles adaptés.
4. Vérifie la conformité en continu (configuration consistency verification : get-config vs attendu, avec notification de changement).

Point d'honnêteté : le SDN de campus Huawei reste un **SDN « piloté par templates »** plus qu'un SDN temps réel type data center — les décisions de forwarding restent locales aux équipements ; le contrôleur orchestre la configuration et les politiques, il ne calcule pas chaque flux. C'est un choix d'architecture sain pour un campus (résilience en cas de perte du contrôleur).

## 14. Southbound interfaces : comment NCE parle aux équipements

« Southbound » = les protocoles entre le contrôleur et les équipements gérés. NCE-Campus en utilise plusieurs, selon le type d'équipement et la fonction :

| Protocole | Usage principal | Équipements concernés |
|---|---|---|
| **NETCONF / YANG** | Configuration structurée (déploiement, vérification de conformité) | Switches, routeurs AR récents (modèles supportant YANG) |
| **SNMP** | Supervision, télémétrie classique, gestion des équipements legacy | Tous, y compris équipements traditionnels |
| **CAPWAP** | Tunnels de gestion/contrôle des AP vers le WAC | AP AirEngine (via WAC physique ou fonction WAC) |
| **HTTP/2** | Télémétrie et authentification (selon versions) | Équipements récents |
| **HTTPS** | Gestion web sécurisée, portails | Divers |
| **TCP** | Canaux dédiés (ex : synchro avec composants d'authentification distants) | Composants d'authentification |

La documentation V300R022C10 mentionne explicitement NETCONF/SNMP/HTTP/2/HTTPS/TCP comme canaux southbound. Le choix du protocole est automatique selon les capacités annoncées par l'équipement lors de l'onboarding.

## 15. NETCONF/YANG — le protocole de configuration structurée

NETCONF (RFC 6241/6242) est le protocole moderne de configuration réseau : contrairement à SNMP (orienté supervision) ou au CLI (texte libre), il manipule des **données structurées** décrites par des modèles **YANG** (RFC 7950).

Pourquoi c'est important dans NCE :

- **Idempotence et validation** : une opération `edit-config` est validée contre le modèle YANG avant application — moins d'erreurs de syntaxe qu'en CLI.
- **Transactions** : possibilité de valider/commit avec rollback (candidate datastore) — base du rollback automatisé.
- **Vérification de conformité** : NCE peut faire `get-config` et comparer avec la configuration attendue (configuration consistency verification), et recevoir des **notifications de changement** quand quelqu'un modifie un équipement en CLI hors NCE (détection du « shadow IT » local — voir cas pratique 14).
- **Interopérabilité** : YANG est un standard — les mêmes concepts servent pour l'automatisation par scripts/Ansible.

Limite honnête : tous les équipements ne supportent pas YANG de façon égale ; les gammes récentes (CloudEngine, NetEngine AR V5, AirEngine) sont les mieux couvertes. Les équipements anciens restent gérés en SNMP/CLI-like — vérifier la **matrice de compatibilité** de la version NCE cible pour chaque modèle (S310, AR720 : vérifier le support NETCONF/YANG exact selon la version logicielle).

## 16. SNMP — la télémétrie et le legacy

SNMP reste utilisé par NCE-Campus pour :

- La **supervision** des équipements traditionnels ou tiers (au sens large : tout ce qui n'est pas piloté en NETCONF).
- Les **tests SLA** : la documentation Monitoring and O&M (V300R020C10) indique que les tâches SLA s'appuient sur le protocole **NQA** des équipements, configurables depuis NCE pour les équipements gérés en SNMP.
- La **découverte** et la collecte d'indicateurs de performance de base (CPU, mémoire, interfaces).

Pour Zelef, SNMP reste le protocole de supervision de ses onduleurs et équipements d'énergie — mais **pas via NCE** : c'est le domaine d'eSight ou d'un superviseur dédié. Ne pas confondre les périmètres.

Bonnes pratiques SNMP sous NCE : utiliser **SNMPv3** (authPriv) partout, bannir les community strings `public`/`private`, filtrer les accès SNMP par ACL de management.

## 17. CAPWAP — la gestion des AP AirEngine

Les AP Wi-Fi Huawei (AirEngine : AP361, AP761, etc.) fonctionnent historiquement en mode **fit AP** pilotés par un contrôleur sans-fil (**WAC** — Wireless Access Controller, anciennement AC) via le protocole **CAPWAP** (RFC 5415) : le WAC centralise la gestion radio, les SSID, l'authentification, le roaming.

Dans l'architecture NCE-Campus :

- NCE-Campus **orchestre** la couche WLAN (profils radio, SSID, politiques) et la pousse vers les WAC/AP.
- Les AP peuvent être gérés en mode central (via WAC) ; NCE s'interface au-dessus pour l'automatisation et la supervision unifiée.
- Pour les petits déploiements, des AP peuvent fonctionner avec des fonctions WAC intégrées/virtuelles selon les modèles — vérifier par modèle.

Point de vigilance : la documentation V300R022C10 note des **restrictions** sur certaines fonctions avancées selon les générations d'équipements (ex : l'authentification sans-fil directe, l'identification de terminaux/application exigent des équipements V5 comme dispositifs d'accès). Toujours vérifier la matrice « fonction × modèle » avant de promettre une fonction à un métier.

## 18. HTTP/2, HTTPS et autres canaux southbound

Compléments aux protocoles principaux :

- **HTTP/2** : utilisé pour la télémétrie à haute fréquence (streaming telemetry) et certains échanges d'authentification sur les équipements récents. Plus efficace que le polling SNMP pour la donnée temps réel.
- **HTTPS** : portails captifs, redirection vers les interfaces web des équipements (la documentation note que la redirection SWEB vers le système web des switches/AR n'est pas supportée dans certaines versions — configuration via CLI/SSH dans ce cas).
- **TCP dédié** : les composants d'authentification distants synchronisent les informations d'authentification et d'identification des terminaux avec NCE-Campus via des canaux TCP.

En pratique d'exploitation : ces flux doivent être **autorisés explicitement** dans les pare-feu entre NCE et les sites (voir section 37 — prérequis réseau). Un oubli de flux = un onboarding qui échoue mystérieusement (voir cas pratiques).

---

# PARTIE 3 — BASE DE DONNÉES, ANALYSEUR, HA ET DIMENSIONNEMENT

## 19. Base de données et stockage — que garde NCE ?

NCE-Campus s'appuie sur des bases de données internes (le détail moteur exact dépend de la version et du mode de déploiement — ne pas spéculer : **à vérifier** dans le guide d'installation de la version cible). Fonctionnellement, il conserve :

- **Inventaire** : équipements, modules, versions logicielles, numéros de série/ESN.
- **Configurations** : configurations attendues (templates instanciés), historiques de déploiement, écarts détectés.
- **Données de performance** : indicateurs CPU/mémoire/interfaces, télémétrie — avec des durées de rétention paramétrables (la rétention longue = stockage conséquent : à dimensionner).
- **Alarmes et événements** : base d'alarmes courantes + historique.
- **Données d'authentification** : utilisateurs, sessions, logs d'accès (sensibles — voir sécurité).
- **Rapports** : rapports générés et planifiés.

Conséquences pratiques : prévoir du **stockage redondant** (RAID), une **politique de rétention** (ex : données fines 30 jours, agrégées 1 an — à adapter), et inclure les bases NCE dans la **sauvegarde** (section 107). La volumétrie croît avec le nombre d'équipements et la fréquence de télémétrie — c'est un poste du dimensionnement (sections 23-25).

## 20. iMaster NCE-CampusInsight — l'analyseur (composant distinct)

CampusInsight est la brique « analysis » avancée : plateforme d'analyse big data + ML qui **numérise l'expérience** — par utilisateur, par application, à chaque instant. Ce qu'elle apporte au-delà du NCE de base :

- **Visibilité d'expérience** : qualité perçue par utilisateur/application (pas seulement l'état des équipements).
- **Détection proactive** : identification de problèmes potentiels (utilisateurs, applications, réseau) avant qu'ils ne deviennent des tickets.
- **Root cause analysis** assistée et base de connaissances de pannes (localisation des pannes WLAN/LAN/WAN « en quelques minutes » selon Huawei — objectif à valider en conditions réelles).
- **Optimisation Wi-Fi** : ajustement intelligent des canaux, largeurs de bande et puissances à partir de l'historique de trafic télémétré (test Tolly cité par Huawei : +50 % de performance globale du réseau sans-fil après optimisation — résultat de laboratoire, à considérer comme un potentiel).

Points commerciaux à connaître (datasheet CampusInsight V100R025C00) :

- Souscription par **package logiciel** : package de base d'analyse réseau intelligent (**obligatoire**, tarifé selon le type et la quantité de NE — Network Elements), puis packages optionnels (analyse applicative, optimisation/auto-guérison, analyse de consommation énergétique, copilote O&M).
- C'est donc un **coût additionnel** au NCE-Campus lui-même — à chiffrer séparément.

## 21. Le composant d'authentification — déploiement distribué

Dans les architectures multi-sites, l'authentification des utilisateurs (802.1X, portail) ne doit pas dépendre du lien WAN vers le contrôleur central. NCE-Campus prévoit pour cela un **composant d'authentification** :

- Intégré à NCE-Campus **en tant que service** (pas un produit séparé à acheter dans la logique de base).
- Déployable sur les **branches distantes** : jusqu'à **20 composants** selon la documentation V300R022C10.
- **Synchronisation automatique** avec NCE-Campus central via des canaux TCP : les informations d'authentification des utilisateurs et d'identification des terminaux sont répliquées.
- En cas de coupure du lien vers le central, la branche continue d'authentifier **localement** — continuité de service.

Pour Zelef : sur des sites distants avec des liens WAN peu fiables, prévoir ce composant dans l'architecture. C'est un argument fort face à une authentification 100 % centralisée.

## 22. Haute disponibilité — modes de déploiement

Un contrôleur qui pilote tout le campus devient critique : sa disponibilité doit être traitée comme celle d'un cœur de réseau.

Principes (à valider selon la version — les modes exacts varient) :

- **Déploiement redondant** : NCE-Campus supporte des architectures à haute disponibilité (cluster / actif-passif selon les versions et les guides de déploiement). Exiger du partenaire le **guide de déploiement HA** de la version cible et le tester.
- **Comportement des équipements en cas de perte du contrôleur** : point fondamental — les équipements Huawei **continuent de commuter** avec leur dernière configuration (le plan de données est local). Ce qui s'arrête : les nouveaux déploiements, les changements de politiques, l'authentification centralisée (sauf composant local), la supervision. Le réseau ne « tombe » pas, il se « fige ». C'est rassurant mais à tester (voir cas pratique 11).
- **Bases de données** : la HA doit couvrir les données (réplication) autant que les services.
- **Sauvegarde/restauration** : même en HA, garder une stratégie de backup externe (section 107) — la HA protège de la panne, pas de l'erreur humaine ni du ransomware.

Checklist HA : deux nœuds sur **sites/racks différents**, alimentations et liens redondants, supervision du contrôleur lui-même (qui supervise le superviseur ? — prévoir Zabbix ou équivalent), exercice de bascule **au moins une fois par an**.

## 23. Dimensionnement : petit site (ordre de grandeur)

Ordres de grandeur indicatifs — **à faire valider par le partenaire** avec l'outil de dimensionnement Huawei de la version cible :

- **Périmètre** : 1 site, < 50 équipements (ex : 5-10 switches S310, 10-30 AP, 1-2 AR720, 1 USG6000).
- **Déploiement** : 1 serveur/VM unique (sans HA) ou petite grappe selon l'exigence.
- **Ressources** : VM avec vCPU/RAM/disque « confortables » (les contrôleurs SDN + bases sont gourmands en RAM ; ne pas sous-dimensionner — **valeurs exactes à vérifier** dans le guide d'installation).
- **Remarque honnête** : à cette taille, **le ROI de NCE est discutable** — eKit ou eSight + gestion locale sont souvent plus économiques (voir section 10 et 152). NCE se justifie ici seulement si le site est le pilote d'un déploiement multi-sites.

## 24. Dimensionnement : site moyen (ordre de grandeur)

- **Périmètre** : 1 campus ou 3-10 sites, 50 à 500 équipements administrables.
- **Déploiement** : NCE-Campus en **HA** (2 nœuds), bases répliquées, éventuellement composant d'authentification sur les branches critiques.
- **Ressources** : serveurs dédiés ou VMs sur cluster virtualisé avec réservations de ressources (CPU/RAM garantis — la télémétrie et l'analyse n'aiment pas la contention).
- **Réseau** : liens de management dédiés (VLAN/OOB), NTP commun, DNS.
- **C'est le sweet spot de NCE-Campus** : assez grand pour que l'automatisation paie, assez petit pour rester administrable par une petite équipe formée.

## 25. Dimensionnement : grand campus / multi-sites (ordre de grandeur)

- **Périmètre** : > 500 équipements, dizaines de sites, milliers d'utilisateurs.
- **Déploiement** : grappe NCE dimensionnée avec l'outil officiel, composants d'authentification distribués (jusqu'à 20), CampusInsight pour l'analyse à l'échelle, intégration ITSM (tickets) via API.
- **Gouvernance** : multi-tenant (entité centrale + entités locales), RBAC fin, processus de changement outillé (templates versionnés, fenêtres de maintenance).
- **À ce niveau, NCE n'est plus un outil mais un programme** : chef de projet, intégrateur, formation, runbook d'exploitation. Budgéter les services d'intégration au même ordre de grandeur que les licences (à chiffrer avec le partenaire).

---

# PARTIE 4 — ÉDITIONS ET LICENCES

## 26. Modèle de licence — principe général (device-day)

Le modèle de licence d'iMaster NCE-Campus documenté dans les guides d'usage (MSP Training Manual — License Usage Guide V300R022C00 / V300R024C00) repose sur la notion de **device-day** (équipement-jour) :

- Chaque équipement géré **consomme** des « resource items » de licence au fil du temps (un équipement géré pendant un jour = un device-day).
- Les licences sont **poolées** : un stock commun de device-days dans lequel puisent tous les équipements.
- Il existe une **licence commune** (ex : 30 000 device-days tous types d'équipements confondus dans le scénario MSP documenté), consommée **en priorité**, puis la licence commerciale une fois la commune épuisée.
- La licence commune **ne peut pas** être mise dans un package réallouable (détail du mode MSP).

C'est un modèle de **souscription à la consommation** : on paie pour une capacité de gestion sur une durée, pas pour un droit perpétuel par équipement. Avantage : flexibilité (le pool absorbe les variations de parc). Inconvénient : il faut **piloter** la consommation (voir section 29-30) — un parc qui grandit sans suivi = dépassement.

## 27. Licence plateforme vs licence équipement

Distinguer deux niveaux (logique observable dans la documentation et les offres) :

1. **Licence plateforme / software platform license** : le droit d'utiliser le logiciel NCE-Campus lui-même. Dans le scénario MSP documenté, une licence plateforme d'un an est fournie **gratuitement** (avec services logiciels de management uniquement, **sans garantie/warranty**) — les conditions exactes varient selon le mode commercial : **à vérifier** sur l'offre.
2. **Licences d'équipements gérés (device management licenses)** : la capacité à gérer N équipements, mesurée en device-days, **poolée**, avec **cotermination** possible (alignement des dates de fin — « resource collaboration »).

À cela s'ajoutent les **souscriptions CampusInsight** (section 31) : package de base obligatoire + packages optionnels, tarifés selon le type et la quantité de NE.

## 28. Licences par équipement géré — comment elles se consomment

Règles pratiques de consommation (d'après le License Usage Guide) :

- Tout équipement **enregistré et géré** par le contrôleur consomme des device-days, **quel que soit son type** dans le cas de la licence commune (les licences commerciales peuvent distinguer les types — à vérifier sur l'offre).
- Les équipements **non gérés / non collectés** ne consomment pas (un équipement découvert mais non pris en gestion ne doit pas être compté — vérifier le comportement exact de la version).
- Quand le stock restant passe en **négatif**, une **période de grâce de 30 jours** s'enclenche (section 29).
- **Pas de licence d'essai** fournie aux tenants dans le mode MSP documenté — prévoir la licence dès le pilote.

Conséquence pour le dimensionnement : compter **tous** les équipements qui seront gérés (switches, AP, AR, USG, WAC), ajouter une **marge de croissance** (10-20 %/an selon la dynamique du parc), et aligner la durée de souscription sur la stratégie budgétaire (1 an vs 3 ans — le 3 ans est souvent plus intéressant : à négocier).

## 29. Période de grâce et comportement en cas de dépassement

Quand le compteur de licences passe sous zéro :

- **Période de grâce de 30 jours** : le système continue de fonctionner — c'est le délai pour régulariser (acheter/recharger des licences).
- Au-delà : **à vérifier** le comportement exact de la version (dégradation de fonctions, blocage des nouveaux onboardings, etc.) — ne jamais tester en production : simuler en maquette.

Bonnes pratiques d'un chef de service :

- **Alerte à 80 % et 90 %** de consommation (si la fonction existe dans la version — sinon suivi via API/rapports).
- Revue **trimestrielle** du compteur en comité d'exploitation.
- Prévoir au budget annuel une **enveloppe de croissance** de licences.
- Documenter la procédure d'achat en urgence (qui commande ? quel délai partenaire ?).

Voir aussi le cas pratique 7 (licence dépassée).

## 30. Mode MSP — multi-tenant et licence commune

Le mode **MSP** (Managed Service Provider) permet à un prestataire de gérer plusieurs clients (tenants) depuis un NCE-Campus mutualisé :

- Le MSP achète la licence sur l'**ESDP** (plateforme de distribution logicielle Huawei), importe le fichier de licence dans NCE-Campus en tant qu'administrateur et le valide.
- L'administrateur crée des **packages combinés de resource items** et les **alloue** aux sous-MSP/tenants (mode « réallocation capable » : un sous-MSP peut à son tour allouer à des tenants de niveau inférieur).
- Politique de **suspension** configurable par niveau (un MSP peut suspendre un tenant en défaut).
- Licence commune de 30 000 device-days fournie, consommée en priorité, **non réallouable** en package.

Pour Zelef : le mode MSP ne le concerne que s'il envisage de **fournir** du réseau managé à des entités sœurs/filiales — sinon, c'est le mode entreprise classique (mono-tenant) qui s'applique. Mais comprendre le mode MSP aide à négocier avec un intégrateur qui l'utilise.

## 31. iMaster NCE-CampusInsight — souscriptions associées

CampusInsight se souscrit **en plus** de NCE-Campus (datasheet V100R025C00) :

| Module | Type | Principe de tarif |
|---|---|---|
| Package de base d'analyse réseau intelligente | **Obligatoire** | Selon le type et la quantité de NE |
| Package valeur ajoutée — analyse applicative | Optionnel | Selon le type et la quantité de NE |
| Package valeur ajoutée — optimisation et auto-guérison | Optionnel | Souscription logicielle |
| Package valeur ajoutée — analyse de consommation énergétique | Optionnel | Selon le type et la quantité de NE |
| Package valeur ajoutée — copilote O&M réseau | Optionnel | 1 licence pour 1 ensemble CampusInsight |

Note intéressante pour un chef de service énergies : le package **d'analyse de consommation énergétique** existe — à évaluer s'il apporte une vraie valeur (corrélation conso/réseau) ou s'il fait doublon avec la GTE.

## 32. Licence de 60 devices incluse dans eSight — comparaison de logique

Pour comparer honnêtement les modèles économiques :

- **eSight** : licence plateforme (ex : AppBase-Professional **incluant 60 devices**) + licences incrémentales **perpétuelles** par device (88034GEE), par AP (88034GEF), par ONU (88034GEL), + souscription annuelle SnS (support). Logique : investissement initial puis extensions à l'unité.
- **NCE-Campus** : plateforme (+ éventuellement offerte 1 an en mode MSP) + **souscription device-day** récurrente + CampusInsight en option. Logique : **OPEX récurrent**.

Conséquence : sur 5 ans, comparer le **TCO** (section 121), pas le prix facial de l'année 1. Un NCE « pas cher la première année » peut coûter plus cher qu'eSight sur 5 ans — ou l'inverse si l'automatisation fait économiser des ETP d'exploitation.

## 33. Coût — ordre de grandeur et pourquoi on ne donne pas de prix ici

**Aucun prix public fiable** n'existe pour les licences NCE-Campus : les tarifs dépendent du partenaire, du volume, de la durée de souscription, du pays, des remises projet. Tout chiffre trouvé sur un forum est à considérer comme **non vérifié**.

Ce que l'on peut dire honnêtement :

- Le **ticket d'entrée** (licences + serveurs/VM + intégration + formation) est **sans commune mesure** avec eSight ou eKit : prévoir un **budget projet**, pas un simple achat de licences.
- Les **services d'intégration** (dimensionnement, installation, templates, migration, formation) représentent souvent **autant que les licences** sur un premier déploiement — les oublier est l'erreur budgétaire n°1.
- Le **renouvellement annuel** des souscriptions doit être inscrit au budget récurrent (OPEX), avec une clause de révision.
- **À vérifier auprès du partenaire** : devis détaillé avec (a) licences plateforme, (b) device-days par an, (c) CampusInsight, (d) services d'intégration, (e) formation, (f) support.

## 34. Checklist avant achat de licences

- [ ] Inventaire exact des équipements à gérer (modèle, quantité, version logicielle) — S310, AP361, AP761, AR720, USG6000...
- [ ] Matrice de compatibilité NCE × modèles × versions validée avec le partenaire
- [ ] Croissance prévisionnelle du parc sur la durée de souscription (+ marge)
- [ ] Choix de la durée (1 an / 3 ans) et de la cotermination
- [ ] CampusInsight : oui/non, quels packages
- [ ] Mode MSP ou entreprise — trancher
- [ ] Devis comparé sur TCO 3-5 ans vs statu quo (eSight/eKit)
- [ ] Services d'intégration chiffrés séparément
- [ ] Formation de l'équipe chiffrée et planifiée
- [ ] Procédure de renouvellement et alerte de fin de souscription définies
- [ ] Clause contractuelle : que se passe-t-il en fin de souscription non renouvelée ? (accès lecture ? blocage ? — **à vérifier** et à écrire)

---

# PARTIE 5 — INSTALLATION

## 35. Architecture matérielle requise — serveur / VM

NCE-Campus s'installe sur des **serveurs x86** (physiques ou VMs). Les exigences exactes (vCPU, RAM, disque, IOPS) dépendent de la **version** et du **dimensionnement** (nombre d'équipements gérés, télémétrie, CampusInsight ou non) — **toujours** utiliser le guide d'installation et l'outil de dimensionnement officiels de la version cible.

Principes généraux d'un chef de service :

- **Ne jamais sous-dimensionner la RAM** : bases de données + analyse temps réel = mémoire. Le premier symptôme d'un NCE sous-dimensionné est une interface lente et des traitements d'alarmes en retard (voir cas pratique 17).
- **Stockage** : disques rapides (SSD/NVMe) en RAID redondant, volumétrie calculée avec la rétention (section 19).
- **Réseau** : plusieurs interfaces (management, service, éventuellement backup), redondance.
- **Virtualisation** : si VM, **réservations** de CPU/RAM (pas de sur-allocation sauvage), datastore performant, anti-affinité entre les nœuds HA.
- **HA** : deux machines sur des hôtes/racks/sites distincts (section 22).

## 36. Système d'exploitation supporté

NCE-Campus est livré comme une **solution packagée** (appliance logicielle / image) — le système d'exploitation sous-jacent est **imposé par Huawei** (généralement une distribution Linux durcie/validée par l'éditeur, installée avec le package). Concrètement :

- On ne choisit pas « Ubuntu vs RHEL » : on déploie **l'image/la procédure Huawei**.
- Vérifier les prérequis hyperviseur (versions VMware/Hyper-V/KVM supportées) dans le guide d'installation.
- Le durcissement OS est normalement intégré ; le durcissement **applicatif** reste à faire (section 105).

**À vérifier** pour la version cible : image ISO/OVA fournie, versions d'hyperviseur supportées, procédure d'installation (graphique/ligne de commande).

## 37. Prérequis réseau (ports, flux, DNS, NTP)

Avant d'installer, verrouiller ces prérequis — 80 % des échecs d'onboarding viennent d'ici :

- **Connectivité IP** : routage entre NCE et tous les sites/équipements gérés (ou via VPN/MPLS) ; VLAN de management dédié recommandé.
- **DNS** : résolution directe/inverse fonctionnelle pour NCE et les équipements (le ZTP par nom en dépend).
- **NTP** : **même source de temps** partout (NCE, équipements, serveurs d'authentification). Un décalage > quelques minutes casse les certificats, les tokens API et les logs corrélés.
- **Flux southbound** : autoriser NETCONF (TCP/830), SNMP (UDP/161-162), CAPWAP (UDP/5246-5247 si WAC), HTTPS (TCP/443), HTTP/2/télémétrie et TCP de synchro d'authentification entre NCE et les équipements/sites.
- **Flux northbound** : HTTPS vers les admins (portail web), port **18002** pour l'API NBI RESTful (documenté dans l'écosystème — POST /controller/v2/tokens), SMTP vers le relais mail (alertes), éventuellement SMS via passerelle.
- **Proxy Internet** : si NCE doit joindre les services Huawei (licences, mises à jour), prévoir le proxy avec authentification.
- **Pare-feu** : documenter **chaque flux** dans un tableau (source/destination/port/usage) et le faire valider par l'équipe sécurité — c'est un livrable du projet.

## 38. Prérequis d'installation — checklist complète

- [ ] Serveurs/VM conformes au dimensionnement validé (CPU/RAM/disque/réseau)
- [ ] OS/image Huawei disponible (ISO/OVA) + checksum vérifié
- [ ] Hyperviseur en version supportée
- [ ] Adressage IP, DNS, NTP, passerelle validés
- [ ] Flux réseau ouverts et testés (tableau des flux signé)
- [ ] Certificats (si PKI interne) prêts ou procédure d'auto-signé définie
- [ ] Compte de service / accès hyperviseur disponibles
- [ ] Fenêtre de maintenance planifiée et communiquée
- [ ] Sauvegarde de l'existant (si migration) réalisée
- [ ] Accès au support Huawei/partenaire vérifié (compte, contrat)
- [ ] Plan de rollback écrit (que faire si l'install échoue ?)
- [ ] Équipe formée aux concepts (au minimum : architecture, licence, ZTP)

## 39. Obtenir le package d'installation

- Via le **partenaire Huawei** ou le portail de distribution logicielle (ESDP) avec les droits associés au contrat.
- Vérifier : **référence exacte de version** (ex : V300R024C00 — ne pas mélanger les versions entre doc et package), **checksum** du fichier, **notes de version** (prérequis, incompatibilités connues, correctifs).
- Télécharger aussi : guide d'installation, guide de déploiement HA, matrice de compatibilité des équipements, License Usage Guide de la version.
- Stocker le package sur un support **fiable et sauvegardé** (pas sur le poste d'un technicien).

## 40. Installation pas à pas — grandes étapes

(Le détail écran par écran dépend de la version — suivre le guide d'installation officiel. Ci-dessous le déroulé logique.)

1. **Préparer la VM/le serveur** : créer la VM selon les specs, monter l'ISO/OVA, configurer le réseau (IP statique de management).
2. **Installer le système** : lancer l'installation, choisir le mode (standalone / HA — décider **avant**), définir les mots de passe initiaux (politique forte dès le départ).
3. **Premier démarrage** : vérifier les services (tous les composants manager/controller/analyzer démarrés), consulter les logs d'initialisation.
4. **Accéder au portail** : https://<IP-NCE> — accepter/vérifier le certificat.
5. **Assistant de configuration initiale** : fuseau horaire, NTP, DNS, nom d'hôte, paramètres réseau complémentaires.
6. **Importer la licence** : Maintenance > File Management (dépôt du fichier), puis activation (Device Maintenance > Activate Device License — logique documentée dans le guide Monitoring and O&M). Vérifier le compteur de device-days.
7. **Configurer les services tiers** : relais SMTP (alertes), serveur de fichiers (firmwares), centre de mise à jour / signature database (System > System Management > Third-Party Service).
8. **Créer l'organisation** : tenant, régions, sites (section 43).
9. **Sauvegarde initiale** : snapshot VM + backup applicatif (section 45/107).
10. **Test de bout en bout** : onboarder **un** équipement pilote (un S310 de maquette) et vérifier supervision + déploiement de template avant d'industrialiser.

## 41. Premier login — comptes par défaut et politique

- Utiliser les **comptes initiaux** documentés dans le guide d'installation (généralement un compte `admin` système).
- **Dès le premier login** : changer tous les mots de passe par défaut, appliquer la politique de complexité (longueur, rotation), créer les comptes nominatifs des administrateurs (jamais de compte partagé en production).
- Activer le **verrouillage après échecs** et la **déconnexion automatique** d'inactivité.
- Tracer : vérifier que les connexions alimentent le journal d'audit (section 104).
- Si annuaire d'entreprise : planifier le raccordement LDAP/AD (section 103) — mais garder **un compte local de secours** (break-glass) avec mot de passe sous enveloppe scellée.

## 42. Configuration initiale — assistant de démarrage

L'assistant post-installation couvre typiquement :

- **Système** : nom d'hôte, fuseau horaire, langue du portail.
- **Temps** : serveurs NTP (redondants), vérification de la dérive.
- **Réseau** : DNS, passerelle, éventuellement proxy.
- **Sécurité** : politique de mot de passe, certificats initiaux.
- **Organisation** : création du premier tenant/site (détaillée section 43).
- **Services** : SMTP, serveur de fichiers, mise à jour.

Conseil : faire cette configuration **en maquette d'abord**, documenter chaque écran (captures), puis rejouer en production — c'est le premier « template » de votre exploitation NCE.

## 43. Création de l'organisation : tenant, régions, sites

La hiérarchie logique type dans NCE-Campus :

```
Tenant (entreprise / organisation)
└── Région (géographique : ex. « Siège », « Ouest », « Est »)
    └── Site (bâtiment / agence : ex. « Siège — Bâtiment A »)
        └── Équipements (switches, AP, AR, USG, WAC)
```

- **Tenant** : entité de gestion (mono-tenant en entreprise classique ; multi-tenant en MSP).
- **Région** : regroupement géographique/administratif pour les rapports et les droits.
- **Site** : l'unité opérationnelle — c'est au niveau site que s'appliquent les templates, les politiques d'accès, les fenêtres de maintenance, les composants d'authentification.

Bonnes pratiques : nommer les sites de façon **stable et explicite** (code site + ville + bâtiment), ne jamais réutiliser un nom de site supprimé sans vérifier les restes en base, documenter la convention de nommage dans le dossier d'exploitation.

## 44. Intégration DNS / NTP / certificats

- **DNS** : NCE doit résoudre les noms des équipements/serveurs (et inversement pour les logs). Enregistrements A/PTR propres, TTL raisonnables. Tester avec `nslookup`/`dig` depuis NCE avant l'onboarding.
- **NTP** : hiérarchie claire (source stratum 1/2 → NCE → équipements). Vérifier l'offset (< 1 s idéalement). Un NTP en panne = des certificats qui semblent expirés, des tokens API rejetés, des logs incohérents entre NCE et équipements.
- **Certificats** : inventaire des certificats NCE (portail HTTPS, API, 802.1X/EAP, portail captif). Noter les **dates d'expiration** dans un calendrier avec alerte à J-60/J-30 (voir cas pratique 15). Privilégier la PKI d'entreprise pour l'EAP-TLS ; à défaut, planifier le renouvellement des auto-signés.

## 45. Sauvegarde initiale — avant toute chose

Avant d'onboarder le moindre équipement de production :

1. **Snapshot VM** (si virtualisé) après installation + config initiale validée.
2. **Backup applicatif NCE** (base + fichiers) via la fonction prévue — tester la restauration **en maquette** (un backup non testé n'est pas un backup).
3. Documenter : quoi, où, fréquence, rétention, responsable, procédure de restauration (section 107-108).

## 46. Checklist post-installation

- [ ] Tous les services NCE démarrés et stables (24 h d'observation)
- [ ] Licence importée, compteur device-days cohérent
- [ ] NTP/DNS vérifiés (offset, résolution)
- [ ] Organisation créée (tenant/région/site pilote)
- [ ] SMTP testé (mail d'alerte reçu)
- [ ] Serveur de fichiers joignable (dépôt firmware)
- [ ] Comptes admin nominatifs créés, mots de passe par défaut changés
- [ ] Audit activé et vérifié
- [ ] Sauvegarde initiale réalisée **et restauration testée**
- [ ] Un équipement pilote onboardé et supervisé
- [ ] Documentation d'installation archivée (versions, captures, flux réseau)

## 47. Échecs d'installation fréquents et remèdes

| Symptôme | Cause probable | Remède |
|---|---|---|
| L'ISO ne boote pas | Image corrompue / mauvais mode (UEFI/BIOS) | Vérifier le checksum, re-télécharger, tester l'autre mode |
| Services qui ne démarrent pas | RAM/disque insuffisants | Re-dimensionner la VM, vérifier les prérequis |
| Portail inaccessible | Flux HTTPS bloqué / certificat | Vérifier pare-feu, écoute du service, logs |
| Licence refusée | Fichier corrompu / mauvaise version / ESN | Re-télécharger depuis l'ESDP, vérifier la compatibilité de version |
| NTP qui dérive | Pas de connectivité UDP/123 | Ouvrir le flux, vérifier la hiérarchie |
| Lenteurs dès le départ | Sur-allocation hyperviseur | Réservations CPU/RAM, datastore dédié |

## 48. Mise à jour du contrôleur lui-même

NCE-Campus évolue par versions (V300R020 → R022 → R024...) et correctifs :

- **Politique** : rester à N-1 maximum (ni la toute dernière le jour de sa sortie, ni une version sans support). Lire les notes de version (breaking changes, prérequis).
- **Procédure** : sauvegarde complète avant, snapshot VM, fenêtre de maintenance, test en maquette d'abord, vérification post-upgrade (services, licences, onboarding d'un équipement test, API).
- **Rollback** : snapshot + backup = retour arrière possible (section 86/108).
- **Signature database** : mettre à jour la base de signatures via le centre de mise à jour configuré (System > System Management > Third-Party Service > Signature Database Server), périodiquement ou en temps réel selon la politique.

## 49. Notion de site dans NCE-Campus

Le **site** est l'unité de déploiement : il regroupe les équipements d'un lieu, porte les templates, les politiques (VLAN, SSID, 802.1X), les fenêtres de maintenance et le composant d'authentification éventuel. Bien découper les sites, c'est :

- Isoler les **blast radius** (un template erroné n'impacte qu'un site — voir cas pratique 6).
- Permettre des **fenêtres de maintenance** par site.
- Déléguer des droits par site (RBAC).
- Obtenir des **rapports** par site.

Règle : un site = un lieu physique avec une équipe d'astreinte identifiable. Ne pas faire un site « fourre-tout ».

## 50. Les équipements gérés : S310, AP361, AP761, AR720, USG6000

Rappel du parc de Zelef et du rôle de chaque famille sous NCE :

- **CloudEngine S310** : switches d'accès campus — VLAN, 802.1X filaire, PoE (pour les AP/téléphones), QoS. Gérés en NETCONF/YANG + SNMP.
- **AirEngine AP361** : AP Wi-Fi 6 d'intérieur — géré via WAC/CAPWAP, orchestré par NCE (SSID, profils radio).
- **AirEngine AP761** : AP Wi-Fi 6 d'extérieur (IP68) — mêmes principes, contraintes radio/extérieur spécifiques.
- **NetEngine AR720** : routeur de branche — WAN, VPN, SD-WAN de branche, DHCP/DNS local.
- **USG6000** : firewall — politiques de sécurité ; NCE-Campus gère le campus, le firewall peut être supervisé/piloté selon la matrice de compatibilité (vérifier les fonctions exactes supportées).

**Avant tout onboarding** : vérifier pour **chaque modèle** la version logicielle minimale requise par la version NCE cible (matrice de compatibilité). Un équipement en version trop ancienne = onboarding partiel ou refusé.

---

# PARTIE 6 — ONBOARDING (ZTP ET MANUEL)

## 51. ZTP — principe du zero-touch provisioning

Le **Zero-Touch Provisioning** : un équipement neuf (ou réinitialisé) sorti du carton, branché au réseau, **se configure seul** en allant chercher sa configuration sur NCE-Campus. Plus besoin d'envoyer un expert CLI sur site : un technicien local branche, NCE fait le reste.

Séquence logique :

1. L'équipement démarre en configuration d'usine et demande une adresse IP (DHCP).
2. Via DHCP (option dédiée), e-mail d'activation ou scan, il découvre **l'adresse du contrôleur NCE**.
3. Il s'enregistre auprès de NCE (authentification par ESN/numéro de série).
4. NCE l'affecte au **site** prévu, lui pousse le **template** correspondant, puis le **firmware** cible si nécessaire.
5. L'équipement redémarre/applique et passe à l'état « normal » (géré).

Bénéfice : déploiement d'un site distant en **heures** au lieu de jours, sans compétence CLI locale. Condition : le réseau d'amorçage (DHCP, connectivité vers NCE) doit être prêt **avant** l'arrivée des équipements.

## 52. ZTP via DHCP (option 148/option personnalisée) — détail

Méthode la plus courante en entreprise :

1. Le serveur **DHCP** du site (souvent l'AR720 ou un serveur central) attribue à l'équipement neuf une IP **et** une option DHCP contenant l'adresse (IP ou nom) du contrôleur NCE, voire l'identifiant du site.
2. L'équipement utilise cette information pour contacter NCE et s'enregistrer.
3. NCE vérifie l'ESN (l'équipement doit être **pré-déclaré** ou accepté selon la politique : liste blanche d'ESN recommandée en production).

Prérequis : serveur DHCP configuré avec la bonne option **avant** le branchement, connectivité IP équipement → NCE, ESN connus (relevés à la réception). Piège classique : une option DHCP mal formatée = l'équipement ne trouve jamais NCE (voir cas pratique 1).

## 53. ZTP via e-mail d'activation — détail

Variante : l'équipement reçoit ses informations d'amorçage via un **e-mail d'activation** (mécanisme prévu par Huawei pour certains scénarios cloud) :

- L'administrateur déclenche l'envoi depuis NCE vers l'adresse associée au site/technicien.
- Le technicien sur site suit la procédure d'activation (saisie/scan du code) pour lier l'équipement au site NCE.
- Utile quand le DHCP du site ne peut pas porter l'option (prestataire, réseau invité d'amorçage).

Prérequis : connectivité Internet/mail fonctionnelle, procédure écrite pour le technicien local (pas à pas, avec captures), fenêtre de support téléphonique.

## 54. ZTP via scan (QR / code) — détail

Variante terrain : le technicien **scanne** un QR code (ou saisit un code d'activation) affiché dans NCE pour le site concerné, via l'appli mobile/outil prévu :

- Lie l'équipement (ESN scanné ou saisi) au site en quelques secondes.
- Réduit les erreurs de saisie d'ESN (source fréquente d'échec).
- Idéal pour des déploiements en série (dizaines d'AP : on scanne à la chaîne).

Bonnes pratiques : préparer les étiquettes ESN à la réception (contrôle à la livraison), pré-créer les sites dans NCE, tester la procédure sur 2-3 équipements avant la vague.

## 55. Onboarding manuel — quand le ZTP n'est pas possible

Le ZTP échoue ou est inapplicable (site isolé, DHCP non maîtrisé, équipement d'occasion non réinitialisé) → onboarding manuel :

1. Réinitialiser l'équipement en configuration d'usine (si repris).
2. Lui donner une connectivité IP vers NCE (IP statique ou DHCP simple).
3. Dans NCE : ajouter l'équipement (adresse IP, ESN, identifiants), l'affecter au site.
4. Vérifier l'enregistrement, pousser le template, vérifier la conformité.
5. Documenter l'exception (pourquoi pas de ZTP ? action corrective ?).

L'onboarding manuel n'est pas un échec : c'est la **roue de secours**. Mais si > 20 % des équipements passent en manuel, c'est le processus ZTP qu'il faut réparer.

## 56. Ajouter un S310 — procédure détaillée

1. **Pré-déclaration** : dans NCE, site cible → ajouter l'équipement (modèle S310, ESN relevé à la réception, rôle : accès).
2. **Réseau d'amorçage** : port du switch amont en trunk/DHCP vers NCE, ou DHCP local avec option contrôleur.
3. **Branchement** : le S310 démarre, obtient son IP, contacte NCE, s'enregistre.
4. **Affectation** : vérifier qu'il apparaît dans le bon site, état « en enregistrement » puis « normal ».
5. **Template** : appliquer le template du site (VLAN d'accès, VLAN de management, 802.1X, SNMP, NTP, DNS).
6. **Firmware** : si la version n'est pas la cible, planifier la mise à jour (section 85).
7. **Vérification** : ping de management, `display` de conformité via NCE (get-config vs attendu), test d'un port d'accès (802.1X), PoE si AP raccordés.
8. **Documentation** : étiquetage physique (nom NCE = nom d'étiquette), mise à jour du plan de brassage.

## 57. Ajouter un AP361 — procédure détaillée

1. **Prérequis** : le WAC (ou la fonction de contrôle WLAN) doit être lui-même géré par NCE ; le S310 d'accès doit fournir PoE 802.3af (l'AP361 consomme ~8,8 W d'après sa fiche — vérifier l'alimentation : un port non-PoE = AP qui ne démarre pas).
2. **Pré-déclaration** : AP361, ESN, site, profil radio prévu.
3. **Branchement** : l'AP démarre, obtient une IP (DHCP), découvre le WAC (option DHCP ou DNS selon l'architecture), établit le tunnel CAPWAP.
4. **Remontée NCE** : l'AP apparaît sous le WAC dans NCE ; vérifier l'état « normal ».
5. **Profils** : appliquer le profil radio du site (canaux, puissance) et les SSID (section 72-74).
6. **Vérification** : l'AP diffuse les SSID, un client test s'associe, débit et roaming vérifiés, télémétrie visible dans NCE.
7. **Plan** : reporter l'AP sur le plan d'étage (position = qualité du troubleshooting futur).

## 58. Ajouter un AP761 — procédure détaillée

Même base que l'AP361, plus les spécificités **extérieur** :

1. **Physique** : fixation (mât/mur), étanchéité (presse-étoupes), parafoudre et **mise à la terre** (un AP extérieur mal relié à la terre = panne à la première saison des orages), câble extérieur blindé.
2. **Alimentation** : PoE (injecteur ou switch) — vérifier la distance (< 100 m) et la puissance.
3. **Pré-déclaration et ZTP** : identiques (ESN, site).
4. **Radio** : profil extérieur (puissance conforme à la réglementation locale, canaux autorisés en extérieur — **à vérifier** selon le pays).
5. **Vérification** : test d'association à distance réaliste, vérification de l'étanchéité après intervention, photo du montage archivée.
6. **Maintenance** : inspection visuelle annuelle (corrosion, serrage) — mettre au plan de maintenance préventive.

## 59. Ajouter un AR720 — procédure détaillée

1. **Pré-déclaration** : AR720, ESN, site (souvent **routeur de branche** = point d'entrée du site).
2. **Amorçage** : l'AR720 est fréquemment le **serveur DHCP** du site — paradoxe apparent : il faut une configuration minimale (WAN + DHCP + route vers NCE) avant le ZTP des autres équipements. Deux approches : (a) template AR poussé via ZTP avec option DHCP intégrée, (b) pré-configuration minimale manuelle puis bascule sous NCE.
3. **Enregistrement** : comme les autres (ESN, site).
4. **Template de branche** : WAN (PPPoE/DHCP/fibre selon l'accès), VPN vers le siège (si SD-WAN de branche), DHCP avec option NCE pour les équipements aval, DNS, NTP.
5. **Vérification** : connectivité WAN, tunnel VPN monté, DHCP distribué (tester avec un PC), équipements aval qui s'enregistrent via lui.
6. **Secours** : lien de backup (4G/5G si prévu) testé par bascule réelle.

## 60. Vérifier qu'un équipement est bien géré — checklist

- [ ] L'équipement apparaît dans le **bon site** avec le bon nom
- [ ] État **normal** (ni alarme, ni hors ligne, ni non enregistré)
- [ ] Version logicielle = version cible du site
- [ ] Template appliqué : **conformité vérifiée** (get-config vs attendu, aucun écart inexpliqué)
- [ ] Supervision : télémétrie reçue (CPU, interfaces), alarmes testées (débrancher/rebrancher un port et vérifier l'alarme)
- [ ] Management : ping/SSH depuis NCE (ou via NCE), SNMPv3 fonctionnel
- [ ] Fonctionnel : port d'accès/SSID/VPN testé avec un client réel
- [ ] Documentation : étiquette physique, plan, inventaire à jour

## 61. États des équipements dans NCE (normal, alarme, hors ligne, non enregistré)

La documentation de l'écosystème NCE (API NBI) illustre les états suivis par le contrôleur :

- **Normal (0)** : géré, joignable, sans alarme bloquante.
- **Alarme (1)** : géré mais avec alarme(s) en cours — l'équipement fonctionne mais un problème est signalé (température, port down, etc.).
- **Hors ligne (3)** : ne répond plus au contrôleur — à diagnostiquer (réseau ? alimentation ? panne ?).
- **Non enregistré (4)** : découvert ou déclaré mais pas encore pris en gestion (onboarding en cours ou en échec).

En exploitation : un tableau de bord « par état et par site » est le premier écran du matin. Tout équipement « hors ligne » > 15 min = ticket. Tout « non enregistré » qui stagne = onboarding à reprendre (cas pratiques 1-5).

---

# PARTIE 7 — GESTION DU RÉSEAU FILAIRE

## 62. Templates de configuration — concepts

Le **template** est le cœur de l'automatisation NCE : un modèle de configuration avec des **variables**, appliqué à un site, un groupe ou un équipement. Exemple : le template « Switch d'accès standard » contient les VLAN, le SNMP, le NTP, le 802.1X — avec des variables comme `{{VLAN_GESTION}}`, `{{NOM_SITE}}`.

Cycle de vie d'un template :

1. **Conception** : écrire le template à partir d'une configuration de référence validée (un switch qui marche, pas une théorie).
2. **Variables** : identifier ce qui change par site/équipement (VLAN, noms, adresses).
3. **Test** : appliquer en maquette, vérifier la conformité, tester les services.
4. **Versionnement** : chaque modification = nouvelle version (jamais de modification « à la main » en production).
5. **Déploiement** : par vagues, avec fenêtre de maintenance et rollback prêt (sections 84/87).

Règle d'or : **NCE est la source de vérité**. Toute modification faite en CLI hors NCE sera détectée comme écart (configuration consistency) — et c'est voulu.

## 63. Variables dans les templates — paramétrage par site/équipement

Les variables permettent un template unique pour des sites différents :

| Variable (exemple) | Portée | Exemple de valeur |
|---|---|---|
| `{{NOM_SITE}}` | Site | SIEGE-BAT-A |
| `{{VLAN_GESTION}}` | Site | 10 |
| `{{VLAN_USERS}}` | Site | 20 |
| `{{VLAN_VOIP}}` | Site | 30 |
| `{{VLAN_IOT}}` | Site | 40 |
| `{{NTP_SERVEUR}}` | Global | 192.168.100.10 |
| `{{NOM_SWITCH}}` | Équipement | SW-ACC-01 |
| `{{PORT_UPLINK}}` | Équipement | XGigabitEthernet0/0/1 |

Bonnes pratiques : nommer les variables en **majuscules explicites**, documenter chaque variable (description, exemple, obligatoire/facultatif), valider les valeurs (plages VLAN 1-4094, formats IP) avant déploiement, et **ne jamais** mettre de mot de passe en clair dans un template (utiliser le coffre/secret store de NCE si disponible, sinon procédure sécurisée).

## 64. VLAN — conception et déploiement via NCE

Conception type d'un site (exemple — à adapter) :

| VLAN | Nom | Usage |
|---|---|---|
| 10 | GESTION | Management des équipements |
| 20 | USERS | Postes utilisateurs (802.1X) |
| 30 | VOIP | Téléphonie (voice VLAN) |
| 40 | IOT | Objets connectés (segmenté, filtré) |
| 50 | INVITES | Wi-Fi invités (portail captif) |
| 60 | SERVEURS | Serveurs locaux du site |
| 99 | NATIVE/TRANSPORT | Interconnexions (à sécuriser) |

Déploiement via NCE : les VLAN sont définis dans le template du site (ou la politique), poussés en NETCONF sur les switches, vérifiés par conformité. Le **VLAN de management** doit exister sur tous les équipements et être routé vers NCE. Erreurs classiques : oublier le VLAN sur un trunk d'uplink (site isolé), VLAN voice mal configuré (téléphones sans réseau), VLAN 1 laissé en natif partout (mauvaise pratique de sécurité).

## 65. Politiques d'accès (802.1X) — principes

Le **contrôle d'accès réseau (NAC)** via 802.1X : avant de donner l'accès réseau à un équipement branché sur un port, le switch **authentifie** l'utilisateur/la machine. C'est le « badge d'entrée » du réseau filaire.

Pourquoi c'est structurant avec NCE : déployer 802.1X à la main sur 200 ports = des semaines et des erreurs ; via NCE : une politique définie une fois, poussée partout, avec des **autorisations dynamiques** (VLAN/ACL/QoS selon qui se connecte). C'est typiquement le projet qui **justifie** NCE.

Prérequis : serveur **RADIUS** (le composant d'authentification NCE ou un RADIUS externe), annuaire (AD/LDAP) ou base locale, supplicants configurés sur les postes (natif Windows/macOS), plan de **dérogations** (imprimantes, équipements sans 802.1X → MAB, voir section 68).

## 66. 802.1X : rôles (supplicant, authentificateur, serveur)

Trois rôles, à connaître par cœur pour dépanner :

1. **Supplicant** : le poste/l'équipement qui veut accéder au réseau (le PC avec son client 802.1X).
2. **Authentificateur** : le switch (S310) — il relaie les échanges EAP entre le supplicant et le serveur, et **applique** le résultat (autorise/bloque le port, assigne le VLAN).
3. **Serveur d'authentification** : le RADIUS (NCE ou externe) — il **décide** (vérifie les identifiants dans l'annuaire) et renvoie les attributs d'autorisation (VLAN, ACL, etc.).

Séquence : le port s'ouvre en « non autorisé » → EAPOL entre supplicant et switch → le switch relaie en RADIUS vers le serveur → décision → le switch applique (VLAN dynamique, ACL) et ouvre le port. En cas d'échec : VLAN de quarantaine ou blocage selon la politique.

## 67. 802.1X : méthodes EAP courantes

| Méthode | Authentification | Sécurité | Usage |
|---|---|---|---|
| **EAP-TLS** | Certificats (client + serveur) | Très forte | Postes gérés, le top sécurité (nécessite une PKI) |
| **PEAP-MSCHAPv2** | Identifiant/mot de passe (tunnel TLS) | Bonne | Le plus courant en entreprise (AD) |
| **EAP-TTLS** | Identifiant/mot de passe ou certificat | Bonne | Alternative, souple |
| **EAP-MD5** | Mot de passe (faible) | Faible | À éviter |

Recommandation : **PEAP-MSCHAPv2** adossé à l'AD pour démarrer (compromis simplicité/sécurité), trajectoire vers **EAP-TLS** pour les postes gérés (plus sûr, pas de mot de passe). Tester **chaque méthode** en maquette avec de vrais postes avant le déploiement (les supplicants ont leurs caprices : voir cas pratique 10).

## 68. MAC-auth et portail captif — les alternatives

Tout le monde ne parle pas 802.1X (imprimantes, caméras, vieux équipements, certains IoT) — prévoir les filets :

- **MAB (MAC Authentication Bypass)** : le switch authentifie l'équipement par son **adresse MAC** auprès du RADIUS. Pratique mais faible (MAC spoofable) → réserver aux équipements connus, avec **liste blanche** de MAC et VLAN dédié (ex : VLAN imprimantes, très filtré).
- **Portail captif** : l'utilisateur ouvre un navigateur, s'identifie sur une page (NCE peut porter le portail). Adapté aux **invités** et au BYOD léger.
- **Stratégie combinée type** : 802.1X en premier → si échec et MAC connue → MAB → sinon portail ou quarantaine. L'ordre et les délais se règlent dans la politique NCE.

Pour Zelef : les **copieurs** (son métier !) sont typiquement en MAB sur VLAN dédié — prévoir ce cas dès la conception 802.1X, pas après.

## 69. Autorisation dynamique : VLAN, ACL, QoS par utilisateur

La vraie puissance du 802.1X piloté par NCE : l'autorisation **change selon qui se connecte** :

- **VLAN dynamique** : le RADIUS renvoie `Tunnel-Private-Group-ID` = le VLAN de l'utilisateur (compta → VLAN 21, technique → VLAN 22). Un même port physique sert à tout le monde, en sécurité.
- **ACL dynamique** : règles de filtrage poussées à la session (ex : les invités ne voient qu'Internet).
- **QoS dynamique** : profil de débit/priorité selon le profil (ex : la visio des dirigeants priorisée).

Tout cela se définit dans les **politiques d'autorisation** de NCE (qui → quoi → sous quelles conditions : groupe AD, horaire, type d'équipement, site). Conseil : commencer avec 3-4 profils simples (employé, invité, IoT, quarantaine), complexifier ensuite — une matrice d'autorisation illisible est une matrice non maintenable.

## 70. QoS filaire — modèles et déploiement

La QoS garantit que le trafic important passe même quand le réseau est chargé (ex : la visio ne doit pas être hachée parce que quelqu'un télécharge).

Modèle type (DiffServ) :

| Classe | DSCP | Usage | Traitement |
|---|---|---|---|
| Voix | EF (46) | Téléphonie IP | Priorité stricte, faible latence |
| Visio | AF41 (34) | Visioconférence | Priorité élevée, bande garantie |
| Données critiques | AF31 (26) | Applis métier | Bande garantie |
| Données standard | AF21/BE (0) | Bureautique, web | Best effort |
| Invités/IoT | BE (0) + policing | Non critique | Débit plafonné |

Déploiement via NCE : classification (à l'entrée, selon 802.1X/ACL), marquage, files de priorité et **policing/shaping** sur les switches — le tout dans le template du site. Vérification : générer de la charge en maquette et mesurer (pas de QoS « à l'aveugle »). Erreur classique : marquer sans faire confiance au marquage en amont, ou policer trop bas et casser une appli métier (voir cas pratique 6).

---

# PARTIE 8 — GESTION WLAN CENTRALISÉE

## 71. Gestion WLAN centralisée — principes

Avec NCE-Campus, le Wi-Fi n'est plus configuré AP par AP (ou sur un WAC isolé) : il est **orchestré** comme le filaire — profils radio, SSID et politiques définis centralement, poussés aux WAC/AP, supervisés en unifié.

Bénéfices : cohérence (même SSID partout), déploiement rapide (un nouveau site = un profil), supervision croisée (un problème Wi-Fi visible avec son contexte filaire), et automatisation (ZTP des AP, mises à jour groupées).

Architecture : NCE-Campus → (orchestration) → **WAC** (contrôleur WLAN) → (CAPWAP) → **AP** (AP361/AP761). NCE ne remplace pas le WAC, il le **pilote** et l'intègre à la gestion unifiée.

## 72. Le modèle WAC + AP : comment NCE pilote le Wi-Fi

- Le **WAC** centralise : tunnels CAPWAP vers les AP, gestion des associations, politiques radio de base, authentification 802.11.
- **NCE-Campus** apporte au-dessus : les **profils** (radio, SSID, sécurité), le **déploiement** (ZTP des AP via le WAC), la **supervision unifiée** (le Wi-Fi dans la même topologie que le filaire), les **politiques d'accès** (802.1X Wi-Fi, PPSK, portail) et l'**analyse** (CampusInsight).
- L'authentification Wi-Fi 802.1X transite par le RADIUS (composant NCE ou externe) comme en filaire — **même annuaire, mêmes groupes**, cohérence totale.

Point de vigilance : les fonctions avancées (identification de terminaux, statistiques applicatives, certaines authentifications) exigent des équipements **V5** comme dispositifs d'accès selon la documentation — vérifier la matrice par modèle avant de s'engager.

## 73. Profils radio — conception (canaux, puissance, bandes)

Un **profil radio** définit comment les AP émettent. Conception type :

- **Bandes** : 2,4 GHz (portée, compatibilité — mais encombrée) et 5 GHz (débit, capacité) ; 6 GHz si Wi-Fi 6E/7 et modèles compatibles (vérifier la réglementation locale — **à vérifier**).
- **Largeur de canal** : 20 MHz en 2,4 GHz (toujours) ; 40/80 MHz en 5 GHz selon la densité (plus large = plus de débit mais moins de canaux non chevauchants).
- **Puissance** : **moins fort que le maximum** en intérieur dense (une puissance trop forte = interférences entre AP et clients qui s'accrochent au mauvais AP). Point de départ : 14-17 dBm en 5 GHz en bureau dense, à ajuster par mesure.
- **Canaux 2,4 GHz** : 1, 6, 11 uniquement (les seuls non chevauchants).
- **Canaux 5 GHz** : plan en quinconce, en évitant les canaux DFS si les coupures radar sont problématiques (ou en les assumant).

Le profil se définit une fois dans NCE et s'applique par site/type de zone (bureaux, entrepôt, extérieur). La **planification prédictive** (étude de couverture) reste recommandée pour les zones critiques — NCE optimise, il ne remplace pas une étude.

## 74. SSID — conception et bonnes pratiques

Règles d'un plan SSID propre :

- **Peu de SSID** : chaque SSID consomme de l'airtime (beacons). Au-delà de 3-4 SSID par AP, la performance se dégrade — regrouper par **profils d'accès** plutôt que par service.
- Exemple : `ENTREPRISE` (802.1X, employés), `INVITES` (portail captif, isolé, débit plafonné), `IOT` (PPSK ou 802.1X, VLAN dédié). Éventuellement un SSID `DIRIGEANTS`/spécifique si justifié.
- **Ne pas diffuser le SSID** sensible en clair partout (ou le restreindre par zone/AP).
- **Bande steering** : orienter les clients capables vers le 5 GHz.
- **Sécurité** : WPA3-SAE ou WPA2-Enterprise (jamais de WPA2-PSK partagé pour les employés — le PSK partagé est un secret qui fuit).
- **VLAN par SSID** (ou dynamique par utilisateur) : séparer les flux dès l'AP.

Dans NCE : les SSID se définissent dans les profils WLAN du site, avec la sécurité, le VLAN et les politiques associées — déploiement atomique sur tous les AP du site.

## 75. Planification radio : RRM, DCA, TPC

Le **RRM** (Radio Resource Management) ajuste automatiquement la radio :

- **DCA** (Dynamic Channel Assignment) : change les canaux des AP pour minimiser les interférences, en fonction des mesures.
- **TPC** (Transmit Power Control) : ajuste les puissances d'émission (baisse quand les AP se voient trop, monte pour couvrir un trou).

Avec NCE/CampusInsight, ces algorithmes s'enrichissent de l'historique (ML) : Huawei cite +50 % de performance Wi-Fi après optimisation dans un test Tolly — prendre comme un **potentiel**, pas une promesse contractuelle.

Bonnes pratiques : laisser le RRM actif mais **borné** (plages de puissance, canaux autorisés), planifier les changements la **nuit** (un changement de canal en pleine journée = micro-coupures), surveiller les AP « exclus » du RRM (souvent un symptôme : AP mal placé ou en panne partielle).

## 76. Roaming — 802.11k/v/r et pratiques Huawei

Le **roaming** : un client qui se déplace passe d'un AP à l'autre sans couper sa session (critique pour la voix/vidéo sur Wi-Fi).

- **802.11k** : l'AP informe le client des AP voisins (liste de voisins) → le client choisit mieux et plus vite.
- **802.11v** : gestion des transitions (l'AP peut « suggérer » au client de changer d'AP pour équilibrer la charge).
- **802.11r** (Fast BSS Transition) : ré-authentification rapide (évite le cycle 802.1X complet à chaque changement d'AP) — **indispensable** pour la voix sur Wi-Fi.
- Pratiques : activer k/v/r sur le SSID voix, dimensionner le **recouvrement de couverture** (-65 dBm en bordure de cellule pour la voix), tester en marchant avec un appel réel (le seul test qui compte).

Sous NCE : ces options s'activent dans le profil SSID ; la supervision permet de voir les **échecs de roaming** par client (diagnostic fin). Voir cas pratique 9 (roaming dégradé).

## 77. Sécurité WLAN : WPA2/WPA3, 802.1X Wi-Fi, PPSK

- **WPA2-Enterprise (802.1X)** : la base saine pour les employés (EAP-TLS ou PEAP, voir section 67).
- **WPA3-SAE** : remplace le PSK (meilleure résistance aux attaques hors ligne) ; **WPA3-Enterprise** (192-bit) pour les environnements exigeants. Vérifier la compatibilité des clients (parc ancien → mode transition WPA2/WPA3).
- **PPSK (Private Pre-Shared Key)** : une **clé par utilisateur/groupe** (au lieu d'un PSK partagé) avec VLAN/ACL par clé — idéal pour l'IoT, les prestataires, les invités « connus ». NCE gère le cycle de vie des PPSK (création, expiration, révocation).
- **Isolation** : invités isolés entre eux (client isolation) et d'Internet uniquement ; IoT sur VLAN dédié sans accès au LAN.

Règle : **jamais de PSK unique partagé** pour un usage employé — c'est le PPSK ou le 802.1X.

## 78. NCE vs eKit pour le WLAN — quand NCE apporte un plus

eKit gère très bien un Wi-Fi simple (SSID, PSK, quelques AP). NCE apporte un plus quand :

- **802.1X Wi-Fi** avec annuaire d'entreprise (eKit ne le fait pas au même niveau).
- **Multi-sites** avec SSID cohérents et administration centrale.
- **PPSK** à grande échelle (hôtel, résidence, campus étudiant).
- **Analyse** : expérience par utilisateur, optimisation radio ML, rapports.
- **Invités** avec portail personnalisé, vouchers, traçabilité (obligations légales de journalisation — **à vérifier** selon le pays).
- **Roaming voix** finement réglé et supervisé.

En dessous de ces besoins : eKit est plus simple et moins cher. Le Wi-Fi d'une petite agence (3 AP361, 1 SSID PSK) ne justifie pas NCE.

## 79. Invités et BYOD — portail captif via NCE

Le **portail captif** : l'invité s'associe au SSID `INVITES`, ouvre son navigateur, arrive sur une page d'authentification (voucher, SMS, e-mail, sponsor interne, ou simple acceptation des CGU).

Mise en œuvre avec NCE :

- SSID dédié, VLAN invités, ACL restrictives (Internet uniquement, pas de LAN), **débit plafonné** (QoS), **durée de session limitée**.
- Page de portail personnalisée (logo, CGU) — le portail peut être porté par NCE ou externalisé (intégrateurs type Purple : NCE relaie le RADIUS vers la plateforme tierce).
- **Journalisation** : conserver les logs de connexion (exigences légales variables — **à vérifier** localement).
- **BYOD employés** : onboarding avec PPSK ou 802.1X (selon la maturité), VLAN dédié, posture check si exigé.

Pièges : un portail qui ne s'affiche pas (problème DNS/HTTPS — prévoir le contournement), des vouchers qui fuient (durées courtes, un par invité).

## 80. IoT sur le WLAN — segmentation

L'IoT (capteurs, caméras, badges, équipements techniques) est le point faible de la sécurité : des objets peu patchés, parfois avec des stacks réseau fragiles.

Architecture type sous NCE :

- **SSID/VLAN IoT dédié** (ou PPSK par famille d'objets), **jamais** sur le VLAN utilisateurs.
- **Filtrage strict** : l'IoT ne parle qu'à ses serveurs (ACL), pas à Internet en direct (sauf besoin), pas au LAN.
- **Authentification** : PPSK par lot ou 802.1X si l'objet le supporte ; MAB en dernier recours avec liste blanche.
- **Supervision** : les objets qui se taisent (capteur muet) = alarme — définir des seuils d'inactivité.
- **Cycle de vie** : inventaire des objets (qui ? où ? qui est responsable ?), plan de remplacement.

Pour un chef de service énergies : les objets techniques (sondes, automates) passent idéalement en **filaire** sur VLAN IoT — le Wi-Fi reste pour le mobile.

---

# PARTIE 9 — AUTOMATISATION

## 81. Automation — vue d'ensemble

L'automatisation NCE, c'est l'exploitation **à l'échelle** sans multiplier les administrateurs :

1. **ZTP** : onboarding sans toucher (sections 51-59, 82).
2. **Templates + variables** : configuration industrialisée (62-63, 83).
3. **Déploiement par vagues** : changements planifiés, progressifs, réversibles (84).
4. **Firmware en masse** : mises à jour groupées avec politique (85-86).
5. **Rollback** : retour arrière automatisé (87).
6. **API** : intégration aux outils de l'entreprise (88-91).

Philosophie : **tout ce qui est fait deux fois doit être automatisé** ; tout ce qui est automatisé doit être **testé, versionné et réversible**. L'automatisation sans ces trois garde-fous, c'est juste une façon plus rapide de casser plus grand (voir cas pratique 6).

## 82. ZTP détaillé — séquence complète d'un onboarding automatique

Reprenons la séquence ZTP en détail opérationnel (exemple : 20 AP361 pour une nouvelle agence) :

**J-7 — Préparation (au bureau)**
- Sites créés dans NCE, templates validés en maquette, firmware cible déposé sur le serveur de fichiers.
- ESN relevés à la réception et **pré-déclarés** dans le site (liste blanche).
- DHCP du site configuré avec l'option contrôleur (ou procédure e-mail/scan prête).
- Fiche d'intervention rédigée pour le technicien local (branchement, ordre, contacts d'astreinte).

**Jour J — Exécution (sur site)**
1. Le technicien branche les AP (PoE vérifié).
2. Chaque AP : DHCP → découverte NCE → enregistrement (ESN) → affectation au site.
3. NCE pousse le template (SSID, profils radio) puis le firmware si écart.
4. Les AP redémarrent si nécessaire et passent « normal ».
5. Le technicien teste avec son téléphone (SSID visibles, association OK) et remonte la fiche.

**J+1 — Vérification (à distance)**
- Conformité des 20 AP (aucun écart), télémétrie OK, plan de couverture ajusté si besoin.
- Clôture du ticket de déploiement, archivage de la fiche.

Temps typique : quelques heures pour 20 AP avec un technicien non-spécialiste — contre 2-3 jours en configuration manuelle.

## 83. Templates et variables — guide pratique

Méthode pour industrialiser les templates :

1. **Partir du réel** : prendre la configuration d'un équipement qui fonctionne bien en production, la nettoyer (retirer le spécifique), en faire le template v1.
2. **Identifier les variables** : tout ce qui change par site/équipement devient une variable (section 63). Le reste est fixe (= standard d'entreprise).
3. **Tester en maquette** : appliquer le template sur un équipement de lab, vérifier chaque service (management, 802.1X, SSID, VPN...).
4. **Versionner** : v1.0, v1.1... avec changelog (qui, quoi, pourquoi). Stocker les templates hors NCE aussi (Git) — traçabilité et revue.
5. **Revue par un pair** : un deuxième œil avant chaque version majeure (comme du code).
6. **Déployer par vagues** : site pilote → 10 % → 50 % → 100 %, avec critères de passage (pas d'incident bloquant pendant X jours).
7. **Rollback prêt** : version précédente conservée et testée (section 87).

Anti-patterns : le template « copié de la doc sans test », la variable non documentée, le déploiement « big bang » un vendredi soir.

## 84. Déploiement en masse — vagues et fenêtres de maintenance

Un changement qui touche 100 équipements ne se fait pas en une fois :

- **Vague 0 — maquette** : validation fonctionnelle complète.
- **Vague 1 — pilote** : 1 site non critique (ou quelques équipements), fenêtre de maintenance dédiée, astreinte renforcée.
- **Vague 2 — 10-20 %** : sites variés (pour couvrir les cas particuliers).
- **Vague 3 — généralisation** : le reste, par groupes de sites, avec pause d'analyse entre groupes.
- **Critères de passage** : 0 incident bloquant, conformité 100 %, supervision nominale pendant 48-72 h.

Fenêtres de maintenance : planifiées, **communiquées** (utilisateurs, métiers, astreinte), avec heure de début **et de fin**, et procédure d'arrêt d'urgence (rollback). Un déploiement sans fenêtre annoncée = un incident de communication garanti.

## 85. Mise à jour firmware en masse — procédure

La documentation NCE (Monitoring and O&M V300R020C10) décrit la mécanique : l'administrateur définit une **politique de mise à jour** (manuelle ou automatique) ; à réception de la tâche, l'équipement **télécharge** le package depuis l'adresse indiquée (serveur de fichiers) et s'upgrade.

Procédure recommandée :

1. **Choisir la version cible** : version validée par Huawei pour vos modèles + correctifs de sécurité — jamais « la dernière sortie hier ».
2. **Déposer** le firmware sur le serveur de fichiers NCE (vérifier l'intégrité).
3. **Tester en maquette** : upgrade + downgrade sur chaque modèle, avec test de non-régression des services.
4. **Politique** : manuelle pour la production (l'automatique est réservée aux parcs très homogènes et aux fenêtres maîtrisées).
5. **Vagues** : comme section 84 (pilote → généralisation).
6. **Vérification** : version effective, conformité du template après upgrade (un firmware peut changer des défauts !), supervision nominale.
7. **Fenêtre** : les upgrades redémarrent les équipements — planifier hors production.

## 86. Politique de mise à jour : manuelle vs automatique

| | Manuelle | Automatique |
|---|---|---|
| Déclenchement | L'admin lance, par vague | Planifiée (ex : nuit, week-end) |
| Contrôle | Total | Délégué à la politique |
| Risque | Faible (si vagues respectées) | Moyen (un bug touche tout le monde la même nuit) |
| Charge admin | Élevée | Faible |
| Recommandé pour | Production, sites critiques | Maquette, sites homogènes non critiques |

Règle : **manuelle en production**, automatique éventuellement pour les AP d'un site non critique après plusieurs cycles manuels réussis. Et toujours : **ne jamais** mettre à jour le firmware de tout le parc la veille d'un événement critique (audit, visite officielle...).

## 87. Rollback — revenir en arrière après un déploiement raté

Le rollback, c'est la capacité à **revenir à l'état antérieur** quand un changement tourne mal. Trois niveaux :

1. **Rollback de template** : réappliquer la version précédente du template (conservée et versionnée — section 83). Le plus courant.
2. **Rollback de firmware** : downgrader vers la version précédente (testé en maquette au préalable — un downgrade non testé peut être pire que le bug).
3. **Rollback de contrôleur** : snapshot VM + backup avant upgrade de NCE (section 48).

Conditions d'un rollback qui marche : **détecter vite** (supervision + critères d'échec définis à l'avance : « si > 5 % d'équipements en écart ou 1 site critique impacté, on rollback »), **décider vite** (qui a l'autorité ? — écrit dans la procédure), **exécuter vite** (procédure testée, pas improvisée). Faire un **exercice de rollback** en maquette au moins une fois par an — le jour où on en a besoin, il est trop tard pour apprendre.

## 88. API northbound RESTful — principes

L'API **northbound** (NBI) expose NCE-Campus aux applications : portails, ITSM, supervision tierce, scripts. C'est une API **RESTful** documentée par Huawei (plus de 500 API selon le constructeur — l'écosystème développeur Huawei propose sandbox et outils graphiques).

Principes :

- Ressources (sites, équipements, alarmes, topologie...) manipulées en HTTP (GET/POST/PUT/DELETE), réponses JSON.
- **Ne jamais** exposer l'API sur Internet sans protection (VPN, IP allowlist, TLS).
- Versionner ses scripts (l'API évolue avec les versions NCE — figer la version cible).
- Journaliser les appels sensibles (qui a fait quoi via l'API — traçabilité).

## 89. Authentification API : tokens (POST /controller/v2/tokens)

D'après l'écosystème documenté (NBI V300R024C00) :

- Authentification par **token** : `POST /controller/v2/tokens` avec les identifiants d'un compte du **groupe d'utilisateurs NBI tiers** (third-party NBI User Group) → le contrôleur renvoie un token.
- Le token se transmet dans l'en-tête **`X-ACCESS-TOKEN`** des appels suivants.
- Expiration **glissante** (sliding-expiry) : le token est rafraîchi par l'usage ; prévoir une **reconnexion transparente** sur 401 (non autorisé).
- Les tokens sont **liés à l'IP** (documenté) : un script qui change d'IP source doit se ré-authentifier.

Bonnes pratiques : compte API **dédié** (pas le compte admin humain), droits minimaux (lecture seule si suffisant), secret stocké en coffre (jamais en dur dans un script), rotation périodique.

## 90. Cas d'usage API : topologie, métriques, alarmes

Exemples concrets (logique observée dans l'écosystème, endpoints à valider sur la version cible) :

- **Topologie** : récupérer les nœuds (AP/LSW/AR/FW) et les liens (avec ports d'extrémité) pour afficher la carte dans un outil tiers ou vérifier la cohérence avec l'inventaire.
- **Métriques** : CPU/mémoire/statut par équipement, utilisation des liens — pour alimenter Zabbix/Grafana ou des rapports custom.
- **Alarmes** : lire les alarmes courantes (scroll API, sévérités 1-4 : critique/haute/moyenne/basse) pour créer des **tickets automatiques** dans l'ITSM (GLPI...).
- **Inventaire** : synchroniser l'inventaire NCE avec la CMDB.

C'est par l'API que NCE s'intègre au SI existant au lieu de rester un silo — prévoir ce chantier dans le projet (voir cas pratique 23).

## 91. Intégration avec des outils tiers (Zabbix, GLPI, scripts)

Scénario type pour Zelef (qui connaît Zabbix) :

- **Zabbix** : supervision transverse (dont onduleurs !) — NCE pousse ses alarmes vers Zabbix (ou Zabbix interroge l'API NCE), corrélation réseau/énergie dans un seul écran (ex : « le switch est down **et** l'onduleur est sur batterie » — diagnostic immédiat).
- **GLPI** : création automatique de tickets depuis les alarmes NCE (via API), inventaire enrichi.
- **Scripts** : exports périodiques (inventaire, conformité), checks custom, rapports pour la direction.
- **Supervision du superviseur** : Zabbix surveille NCE lui-même (services, espace disque, certificats) — « qui supervise le superviseur » (section 22).

Architecture conseillée : NCE = source de vérité **réseau campus Huawei**, Zabbix = supervision **transverse**, GLPI = **tickets/inventaire**. Chacun son rôle, intégrés par API.

---

# PARTIE 10 — ASSURANCE ET SUPERVISION

## 92. Assurance et supervision — vue d'ensemble

L'« assurance » (assurance qualité de service) va au-delà de la supervision classique : il ne s'agit pas seulement de savoir si un équipement est up/down, mais si les **utilisateurs ont une bonne expérience**.

Les briques NCE :

- **Monitoring temps réel** : tableaux de bord, topologie, indicateurs (93-94).
- **Analyse** : détection d'anomalies, cause racine, expérience par utilisateur/application (95-96, CampusInsight section 20).
- **Pilotage** : rapports, alertes, SLA (97-99).
- **Maintenance** : actions correctives depuis NCE (100).

Pour une équipe réduite, l'assurance bien réglée = **moins de tickets**, des tickets **mieux qualifiés**, et des **preuves** (rapports) pour la direction.

## 93. Monitoring temps réel — tableaux de bord

Les tableaux de bord NCE affichent typiquement :

- **Santé globale** : équipements par état et par site (normal/alarme/hors ligne).
- **Utilisateurs** : connectés par SSID/site, échecs d'authentification.
- **Trafic** : top parleurs, applications (si identification activée), utilisation des liens.
- **Wi-Fi** : AP par état, clients par AP, interférences, roaming.
- **Alertes** : alarmes en cours par sévérité.

Bonnes pratiques : un **dashboard « tour de contrôle »** affiché en permanence (NOC/accueil technique), des dashboards **par site** pour les responsables locaux, des dashboards **métier** (ex : disponibilité du Wi-Fi invités pour l'hôtellerie). Revoir les dashboards **trimestriellement** (un dashboard que personne ne regarde est un dashboard à supprimer).

## 94. Cartographie réseau (digital map) — topologie

La **carte numérique** (digital map) : topologie **générée automatiquement** (découverte LLDP + remontées contrôleur), affichant équipements, liens, états en temps réel.

Usages : diagnostic visuel (« où est la coupure ? »), vérification d'architecture (le câblage réel correspond-il au plan ?), présentation à la direction, support aux techniciens distants (« cliquez sur l'AP rouge »).

Limites honnêtes : la topologie auto-découverte peut être **incomplète** (équipements non LLDP, liens non remontés) — la confronter au plan de câblage réel, et la **documenter** (un export mensuel archivé = preuve en cas de litige). Les liens LLDP vers des équipements tiers sont informatifs, pas contractuels.

## 95. Détection d'anomalies — ce que fait l'IA/ML

CampusInsight (et dans une moindre mesure NCE de base) applique du machine learning à la télémétrie :

- **Lignes de base** : le système apprend le comportement « normal » (trafic, latence, taux d'erreur par heure/jour) et signale les **écarts**.
- **Prédiction** : dégradation progressive détectée avant la panne (ex : erreurs CRC croissantes sur un port → SFP/câble à changer).
- **Optimisation radio** : ajustement des canaux/puissances (section 75).

Honnêteté : le ML a besoin de **données** (plusieurs semaines d'historique) et produit des **faux positifs** — ne jamais automatiser d'action corrective sur une simple anomalie ML sans validation humaine (au début du moins). Évaluer en pilote : compter les vrais positifs pendant 3 mois avant de s'y fier.

## 96. Analyse de cause racine (root cause analysis)

Quand un incident survient, NCE aide à remonter à la **cause** plutôt que de traiter les symptômes :

- **Corrélation** : une alarme « AP hors ligne » + une alarme « port PoE down sur le S310 » + une alarme « onduleur sur batterie » (via supervision transverse) = probablement une coupure électrique locale, pas un problème Wi-Fi.
- **Base de connaissances** : les pannes connues et leurs solutions (Huawei fournit une base ; l'enrichir avec **vos** cas — chaque incident résolu devient une fiche).
- **Chronologie** : timeline des événements avant l'incident (changement de config ? mise à jour ?).

Méthode : face à une alarme, toujours chercher **ce qui a changé** (config, firmware, environnement) avant de chercher ce qui est cassé. 80 % des incidents réseau suivent un changement.

## 97. Rapports — types et planification

Types de rapports utiles :

- **Disponibilité** : % uptime par site/équipement (pour les SLA internes).
- **Capacité** : utilisation des liens/ports, croissance (pour anticiper les investissements).
- **Sécurité** : échecs d'authentification, équipements non conformes, ports ouverts.
- **Wi-Fi** : expérience par SSID/site, roaming, interférences.
- **Conformité** : écarts de configuration détectés.
- **Direction** : synthèse mensuelle sur une page (état, incidents majeurs, projets).

Planification : rapports **automatiques** (hebdo/mensuel) envoyés par mail aux destinataires — un rapport manuel est un rapport qui ne sortira pas. Archiver les rapports (traçabilité, comparaisons annuelles).

## 98. Alertes — configuration mail/SMS

- **Canaux** : e-mail (via relais SMTP configuré section 40), SMS (via passerelle SMS — prévoir le contrat opérateur), éventuellement webhook vers l'ITSM.
- **Sévérités** : critique (panne de site, contrôleur), haute (équipement down, boucle), moyenne (dégradation, seuil), basse (informatif). Chaque sévérité = un **canal** et un **délai** (le SMS à 3 h du matin, c'est pour le critique uniquement).
- **Anti-bruit** : seuils, temporisations (pas d'alerte sur un flap de 30 s), fenêtres de maintenance (pas d'alerte pendant les travaux planifiés), **escalade** (si pas d'acquittement en X min → niveau supérieur).
- **Test** : provoquer une alarme test **mensuellement** (débrancher un port de maquette) — une alerte qui n'a jamais été testée est une alerte qui ne marche pas.

## 99. SLA management — NQA et mesures actives

La documentation NCE prévoit des **tâches SLA** pour les équipements gérés en SNMP, basées sur le protocole **NQA** (Network Quality Analysis) des équipements :

- **Principe** : l'équipement envoie des sondes actives (ping, jitter, HTTP...) vers des cibles et mesure : latence, gigue, perte, disponibilité.
- **Usages** : vérifier un lien WAN de branche (l'AR720 sonde le siège), valider un SLA opérateur (preuves chiffrées), détecter une dégradation **avant** les plaintes.
- **Configuration** : depuis NCE, planifier les sondes (cibles, fréquence, seuils), collecter les résultats, alerter sur dépassement.
- **Limites** : les sondes consomment (peu) de bande passante ; les résultats dépendent de la charge de l'équipement sondeur — interpréter avec recul.

## 100. Maintenance des équipements depuis NCE

NCE centralise les opérations de maintenance (documentation Monitoring and O&M) :

- **Gestion des fichiers** : dépôt central (firmwares, patches, licences) — Maintenance > File Management.
- **Upgrades/downgrades** : politiques et tâches (section 85).
- **Licences des équipements** : dépôt puis activation en masse (Maintenance > Device Maintenance > Activate Device License) — sans conflit avec l'activation directe sur l'équipement.
- **Signature database** : mise à jour des bases de signatures (sécurité) des équipements via le centre de mise à jour.
- **Redémarrage** : redémarrage planifié d'équipements (avec vérification post-redémarrage).
- **Diagnostic** : ping/traceroute depuis un équipement, collecte de logs, captures.

C'est le « couteau suisse » de l'exploitation quotidienne — former l'équipe à ces écrans en priorité.

---

# PARTIE 11 — SÉCURITÉ

## 101. Comptes et rôles — modèle RBAC

Le **RBAC** (Role-Based Access Control) : on n'attribue pas des droits aux personnes, on attribue des **rôles** aux personnes, et des **permissions** aux rôles.

Principes sous NCE :

- **Moindre privilège** : chacun n'a que les droits nécessaires à sa fonction (un technicien de site ne reconfigure pas le cœur).
- **Séparation** : lecture (supervision) vs écriture (configuration) vs administration (comptes, licences).
- **Périmètre** : les rôles se combinent avec les **sites/régions** (un admin local ne voit que ses sites).
- **Traçabilité** : chaque action est loguée avec le compte nominatif (pas de compte générique partagé).

## 102. Rôles prédéfinis et rôles personnalisés

Structure type (à adapter à l'organisation — les intitulés exacts dépendent de la version) :

| Rôle | Périmètre typique | Droits typiques |
|---|---|---|
| Super-admin | Tout | Tous (réservé, 1-2 personnes) |
| Admin réseau | Tous sites | Config, templates, maintenance |
| Opérateur NOC | Tous sites | Lecture, acquittement d'alarmes, diagnostic |
| Admin site | Son site | Config locale, onboarding, maintenance |
| Lecteur / Audit | Selon besoin | Lecture seule, rapports |
| Compte API | Selon besoin | API uniquement, droits minimaux |

Créer des **rôles personnalisés** quand les prédéfinis ne collent pas (ex : « technicien astreinte » = lecture + redémarrage AP, rien d'autre). Revoir les rôles **annuellement** (les gens changent de poste, les droits restent — c'est comme ça que naissent les incidents).

## 103. Authentification des administrateurs — locale, RADIUS, LDAP/AD

- **Locale** : comptes dans NCE — simple, mais à gérer (rotation des mots de passe, départs).
- **RADIUS** : centralise l'authentification des admins (même RADIUS que le 802.1X ou dédié).
- **LDAP/AD** : rattachement à l'annuaire d'entreprise — **recommandé** (un départ = une désactivation AD = plus d'accès NCE ; groupes AD → rôles NCE).
- **Hybride recommandé** : AD pour le quotidien + **compte local de secours** (break-glass) pour les situations où l'AD est injoignable (documenté, mot de passe sous enveloppe, usage audité).
- **MFA** : si NCE le supporte dans la version, l'activer pour les comptes privilégiés (**à vérifier**).

## 104. Journal d'audit — traçabilité des actions

Le journal d'audit enregistre **qui a fait quoi, quand, sur quoi** : connexions, modifications de configuration, déploiements de templates, upgrades, changements de comptes, acquittements d'alarmes.

Exigences :

- **Intégrité** : les logs d'audit ne doivent pas être modifiables par les administrés (droits séparés, voire export vers un SIEM).
- **Rétention** : conforme aux obligations (souvent 1 an minimum — **à vérifier** localement).
- **Revue** : revue périodique (au moins trimestrielle) des actions sensibles (qui a touché aux comptes ? aux templates ?).
- **Alertes** : alerter sur les actions critiques (création de compte admin, modification de template de production, désactivation de l'audit — cette dernière ne devrait jamais arriver).

## 105. Durcissement du contrôleur — checklist

- [ ] Mots de passe par défaut changés, politique forte appliquée
- [ ] Comptes inutiles désactivés/supprimés
- [ ] Services/ports non nécessaires fermés sur le serveur
- [ ] Accès au portail restreint (VPN, IP allowlist, pas d'exposition Internet directe)
- [ ] API NBI protégée (TLS, allowlist, compte dédié)
- [ ] Certificats valides (pas d'auto-signé en production si PKI disponible)
- [ ] NTP sécurisé (authentification NTP si possible)
- [ ] Logs d'audit exportés vers SIEM/syslog sécurisé
- [ ] Correctifs NCE appliqués (politique de patch)
- [ ] Sauvegardes chiffrées et testées
- [ ] Revue des rôles et des comptes (trimestrielle)
- [ ] Test d'intrusion ou audit de configuration annuel

## 106. Certificats — gestion et renouvellement

Inventaire (section 44) + processus :

1. **Registre** : liste de tous les certificats (portail, API, EAP, portail captif) avec émetteur, expiration, responsable.
2. **Alertes** : J-90, J-60, J-30, J-7 (plusieurs niveaux — un seul rappel se perd).
3. **Procédure** : renouvellement documenté pas à pas (génération CSR, validation, déploiement, vérification des services, rollback si échec).
4. **Test** : renouveler une fois en maquette avant la première expiration en production.
5. **EAP-TLS** : le renouvellement des certificats clients est le plus délicat (parc de postes) — automatiser via la PKI/GPO.

Un certificat expiré = portail inaccessible, EAP en échec, API rejetée (voir cas pratique 15). C'est l'incident le plus bête et le plus fréquent.

## 107. Sauvegardes NCE — stratégie

- **Quoi** : bases de données (inventaire, configs, alarmes), fichiers (firmwares, templates, licences, rapports), configuration système.
- **Fréquence** : quotidienne (incrémentale/différentielle selon l'outil) + hebdomadaire complète — à adapter au rythme des changements.
- **Où** : **hors du serveur NCE** (stockage dédié, idéalement hors site), chiffré.
- **Rétention** : ex : 30 jours glissants + 12 mensuelles (à valider avec les obligations).
- **Automatisation** : planifiée dans NCE (si la fonction existe) ou par script — **jamais manuelle** (l'humain oublie).
- **Supervision** : alerter sur **échec** de sauvegarde (une sauvegarde qui échoue en silence = pas de sauvegarde).

## 108. Restauration — procédure et tests

- **Procédure écrite** : pas à pas, avec les prérequis (version NCE identique, licence, réseau), les commandes, les vérifications.
- **Test annuel** (minimum) : restaurer en maquette, vérifier (inventaire, un onboarding test, une alarme, l'API). **Chronométrer** : le RTO (temps de restauration) doit être connu, pas deviné.
- **Scénarios** : restauration partielle (une base corrompue) vs totale (serveur perdu) — les deux doivent être documentés.
- **Après incident réel** : revue (qu'est-ce qui a manqué ?), mise à jour de la procédure.

Voir cas pratique 16 (restauration après sinistre).

## 109. Sécurité des flux southbound

- **Chiffrement** : NETCONF sur SSH/TLS, SNMPv3 (authPriv), HTTPS partout — bannir SNMP v1/v2c et HTTP en production.
- **Segmentation** : VLAN de management dédié, ACL sur les équipements (seul NCE — et les admins — joignent les interfaces de management).
- **Authentification mutuelle** : certificats/clés pour les canaux sensibles (télémétrie, authentification).
- **Surveillance** : détecter les tentatives d'accès au management depuis des zones non autorisées (alarme SIEM).
- **ZTP sécurisé** : liste blanche d'ESN (un équipement inconnu ne doit pas s'enregistrer tout seul en production).

---

# PARTIE 12 — MIGRATION ESIGHT → NCE

## 110. Stratégie de migration eSight → NCE — vue d'ensemble

Il n'existe pas de « bouton migrer » : passer d'eSight à NCE-Campus, c'est **reconstruire** la gestion du périmètre campus Huawei sous un nouveau paradigme. La stratégie recommandée :

1. **Ne pas tout migrer** : NCE ne remplace eSight que sur le **campus Huawei** (LAN/WLAN/branches). Le reste (multi-vendeurs, énergie, PON, DC) **reste** sur eSight.
2. **Coexistence** d'abord (section 111) : les deux plateformes vivent côte à côte, avec des périmètres clairs.
3. **Migration par site** (section 112) : site pilote, puis vagues.
4. **Capitaliser** : templates, politiques et procédures construits pendant la migration = le patrimoine d'exploitation NCE.

Erreur à éviter : « on migre tout d'un coup pour en finir » — c'est le meilleur moyen de tout casser en même temps.

## 111. Coexistence eSight + NCE — mode recommandé

Pendant (et après) la migration, les deux cohabitent :

| Périmètre | eSight | NCE-Campus |
|---|---|---|
| Campus Huawei (S310, AP, AR, USG) | Supervision transitoire (double alarme à gérer) | Gestion cible (pilotage) |
| Équipements non-Huawei | Supervision (durable) | — |
| Onduleurs / énergie | Supervision (durable) | — |
| PON / OLT | Supervision (durable) | — |
| Politiques 802.1X / templates | — (limité) | Cible |

Règles de coexistence : **définir qui fait quoi** (matrice RACI), éviter la double configuration (un équipement = un pilote), gérer la **double alarme** transitoire (filtrer ou accepter le bruit pendant la migration), et fixer une **date de fin** de coexistence par site (sinon elle devient permanente).

## 112. Migration par site — méthode pas à pas

Pour chaque site :

1. **Préparation** : inventaire du site (équipements, VLAN, SSID, politiques eSight), site créé dans NCE, templates du site prêts et testés en maquette.
2. **Fenêtre** : planifiée, communiquée, avec rollback (section 118).
3. **Désenrôlement eSight** : retirer le site d'eSight (ou le passer en lecture seule transitoire) — **un seul pilote à la fois**.
4. **Onboarding NCE** : enregistrer les équipements (ZTP si réinitialisation possible, sinon onboarding manuel avec reprise de la configuration via template).
5. **Reprise des services** : VLAN, SSID, 802.1X, QoS — vérifiés un par un avec des tests utilisateurs réels.
6. **Vérification** : conformité 100 %, supervision nominale 72 h, aucune plainte bloquante.
7. **Clôture** : documentation à jour, eSight nettoyé du site, retour d'expérience archivé.

Ne passer au site suivant qu'après **clôture complète** du précédent.

## 113. Ce qui est repris de eSight — inventaire

Ce qui se **transfère** (manuellement, pas automatiquement) :

- **L'inventaire** : la liste des équipements (export eSight → import/déclaration NCE) — avec les ESN à relever physiquement si manquants.
- **Les plans d'adressage** : VLAN, sous-réseaux, DHCP — la conception réseau ne change pas, seul l'outil de déploiement change.
- **Les politiques** : règles 802.1X, SSID, QoS — à **retranscrire** dans les concepts NCE (politiques, profils, templates), pas à copier-coller.
- **Les seuils d'alerte** : à réexprimer dans NCE (les métriques ne sont pas identiques).
- **Les rapports** : les besoins (pas les formats) — reconstruire les rapports équivalents.
- **L'historique** : archiver les rapports/alarmes eSight (traçabilité), mais ne pas chercher à l'importer dans NCE.

## 114. Ce qui N'EST PAS repris — limites honnêtes

- **L'historique fin** (courbes, alarmes anciennes) : pas d'import — archiver eSight en lecture seule le temps nécessaire.
- **Les scripts/customisations eSight** : à réécrire (API différentes).
- **Les équipements non-Huawei** : ils restent sur eSight (ou un autre NMS) — NCE ne les prendra pas.
- **Les habitudes** : l'équipe doit **réapprendre** (templates, politiques, ZTP) — budgéter la formation et accepter la phase « on est moins efficaces qu'avant » (2-3 mois typiques).
- **Les licences eSight** : non transférables vers NCE (modèles différents) — à intégrer au TCO.

## 115. Checklist de migration complète

- [ ] Périmètre de migration défini (quels sites, quels équipements — et ce qui reste sur eSight)
- [ ] Matrice de compatibilité NCE × modèles × versions validée
- [ ] NCE installé, licencié, sauvegardé (Partie 5)
- [ ] Templates du site standard conçus, testés en maquette, versionnés
- [ ] Politiques 802.1X/SSID/QoS retranscrites et validées
- [ ] Site pilote choisi (représentatif mais non critique)
- [ ] Fenêtre de migration planifiée et communiquée
- [ ] Procédure de rollback écrite et testée
- [ ] Équipe formée (au minimum : onboarding, templates, supervision, rollback)
- [ ] Astreinte renforcée pendant la migration
- [ ] Plan de communication utilisateurs (ce qui change pour eux : rien, idéalement)
- [ ] Critères de succès/échec écrits (quand on rollback ?)
- [ ] Documentation cible (dossier d'exploitation NCE) initiée
- [ ] Retour d'expérience prévu (post-mortem même en cas de succès)

## 116. Risques de la migration et mitigations

| Risque | Impact | Mitigation |
|---|---|---|
| Template erroné poussé en production | Panne de site | Maquette + vagues + rollback (cas pratique 6) |
| Équipement incompatible (version) | Onboarding partiel | Matrice de compatibilité + audit préalable |
| Perte de supervision transitoire | Angle mort | Coexistence eSight en lecture seule |
| 802.1X qui bloque les utilisateurs | Paralysie du site | Mode monitor/open d'abord, bascule progressive (cas pratique 10) |
| Équipe insuffisamment formée | Erreurs, lenteurs | Formation + pilote + astreinte experte |
| Dépassement budgétaire (licences) | Arrêt du projet | TCO préalable, marge, jalons go/no-go |
| Résistance au changement | Contournement (CLI sauvage) | Communication, formation, détection des écarts |

## 117. Planning type de migration (exemple 3 sites)

Exemple indicatif pour un parc modeste (à adapter) :

| Phase | Durée indicative | Contenu |
|---|---|---|
| Cadrage | 2-3 semaines | Périmètre, TCO, compatibilité, jalons go/no-go |
| Installation NCE | 2-4 semaines | Serveurs, licences, organisation, sauvegarde |
| Maquette + templates | 3-6 semaines | Templates, politiques, tests, formation |
| Site pilote | 2-3 semaines | Migration, vérification 72 h, REX |
| Vague 2 (1 site) | 2 semaines | Généralisation, ajustements |
| Vague 3 (1 site) | 2 semaines | Généralisation |
| Clôture | 1-2 semaines | Documentation, nettoyage eSight, bilan |

Total indicatif : **3 à 5 mois** pour 3 sites avec une petite équipe — sans compter les impondérables. Ne pas promettre moins sans maquette validée.

## 118. Retour en arrière — plan B

Chaque migration de site a un **plan B écrit** :

- **Seuil de déclenchement** : ex. « si le site n'est pas nominal à H+4, on rollback ».
- **Procédure** : ré-enrôler les équipements dans eSight (configurations sauvegardées avant migration !), rouvrir les accès, vérifier les services.
- **Prérequis** : **sauvegarde des configurations eSight/équipements avant** chaque migration (non négociable).
- **Communication** : message prêt (« retour à la situation antérieure, investigations en cours »).
- **Post-mortem** : analyser avant de retenter (jamais de « on recommence demain pareil »).

Un plan B non écrit n'existe pas.

---

# PARTIE 13 — COMPARAISON ESIGHT vs NCE-CAMPUS

## 119. Comparaison eSight vs NCE-Campus — tableau général

| Dimension | eSight | iMaster NCE-Campus |
|---|---|---|
| Nature | NMS traditionnel unifié | Manager + contrôleur SDN + analyseur |
| Protocoles | SNMP surtout, CLI | NETCONF/YANG, SNMP, CAPWAP, HTTP/2... |
| Périmètre vendeurs | **Multi-vendeurs** (atout) | Quasi **Huawei uniquement** |
| Périmètre fonctionnel | Supervision, config, rapports | + orchestration, ZTP, politiques, IA |
| Filaire/WLAN/WAN | Supervisés séparément | **Unifiés** et orchestrés |
| 802.1X | Gestion limitée | Politiques centralisées, autorisation dynamique |
| Automatisation | Faible (scripts externes) | **Forte** (ZTP, templates, API) |
| Analyse/IA | Basique | CampusInsight (ML, prédiction) — en option |
| Modèle de licence | Perpétuelle + SnS | **Souscription device-day** récurrente |
| Coût d'entrée | Modéré | **Élevé** (projet) |
| Complexité | Modérée | **Élevée** (projet + formation) |
| Idéal pour | Supervision transverse, multi-vendeurs, énergie | Campus Huawei neuf/renouvelé, multi-sites, automatisation |

## 120. Comparaison détaillée par fonction

| Fonction | eSight | NCE-Campus | Verdict |
|---|---|---|---|
| Inventaire | Oui (découverte SNMP) | Oui (enregistrement SDN) | Équivalent |
| Topologie | Oui | Oui (auto, temps réel) | NCE légèrement devant |
| Alarmes | Oui | Oui + corrélation | NCE devant |
| Configuration | CLI/SNMP, manuelle | Templates, ZTP, NETCONF | **NCE largement devant** |
| Firmware en masse | Limité | Politiques natives | NCE devant |
| 802.1X/NAC | Basique | Centralisé, dynamique | **NCE largement devant** |
| WLAN | Supervision | Orchestration (WAC+AP) | NCE devant |
| SD-WAN branches | Non | Oui (AR, orchestration) | NCE seul |
| Analyse/ML | Non | CampusInsight (option) | NCE seul |
| API | Limitée | RESTful riche (500+) | NCE devant |
| Multi-vendeurs | **Oui** | Non | **eSight seul** |
| Énergie/onduleurs (SNMP) | **Oui** | Non (pas son rôle) | **eSight seul** |
| PON/OLT | Oui | Non | eSight seul |
| Simplicité | Oui | Non (projet) | eSight devant |
| Coût | Modéré | Élevé (récurrent) | eSight devant |

## 121. Coût comparé — TCO sur 5 ans (méthode)

Ne comparez jamais les prix année 1. Méthode TCO 5 ans :

**Postes eSight (5 ans)** : licence plateforme + licences devices/AP + SnS × 5 + serveurs (amortis) + ETP d'exploitation (le poste dominant : config manuelle, déplacements) + formation ponctuelle.

**Postes NCE (5 ans)** : licences device-day × 5 + CampusInsight × 5 (si pris) + serveurs/VM + **intégration** (année 1, souvent = licences année 1) + formation (année 1 + recyclage) + ETP d'exploitation (réduits si l'automatisation tient ses promesses — à modéliser en fourchette).

**Règle de lecture** : NCE gagne le TCO quand (a) le parc est grand/volatile (beaucoup de changements), (b) les sites sont distants (ZTP évite des déplacements), (c) l'équipe est petite (l'automatisation compense). eSight gagne quand le parc est stable et mixte. **Chiffrer les deux scénarios** avec le partenaire avant de décider — c'est un livrable du cadrage, pas une intuition.

## 122. Complexité comparée

- **eSight** : installation simple (serveur Windows/Linux + package), concepts connus (SNMP, alarmes), courbe d'apprentissage de quelques jours pour un admin réseau.
- **NCE-Campus** : projet d'intégration (dimensionnement, HA, organisation, templates, politiques, ZTP, formation), concepts nouveaux (YANG, intent, device-day, RBAC multi-tenant), courbe d'apprentissage de **plusieurs semaines à mois**, besoin d'un **référent NCE** dans l'équipe.

Traduction managériale : eSight = un outil qu'on installe ; NCE = une **compétence** qu'on construit. Prévoir le turn-over (documenter !) — si le référent NCE part sans transmission, le contrôleur devient une boîte noire.

## 123. Cas d'usage : quand choisir quoi — arbre de décision

```
Le parc à gérer est-il majoritairement Huawei campus ?
├─ NON (multi-vendeurs, énergie, PON...) → eSight (ou NMS tiers) reste la base
└─ OUI
   ├─ < ~20 équipements, 1 site, besoins simples → eKit (+ eSight si supervision transverse)
   ├─ 20-50 équipements OU multi-sites OU 802.1X requis
   │   ├─ Budget projet + équipe formable → NCE-Campus (pilote d'abord)
   │   └─ Pas de budget projet → eSight + gestion locale (assumer les limites)
   └─ > 50 équipements / campus multi-sites → NCE-Campus (avec TCO et pilote)
```

Et dans tous les cas : **la supervision énergie/onduleurs reste hors NCE** (eSight/Zabbix/GTE) — ce n'est pas un choix, c'est une architecture.

---

# PARTIE 14 — CAS PRATIQUES (25)

## 124. Cas pratique 1 — L'onboarding ZTP qui échoue (DHCP)

**Symptôme** : les switches neufs restent en « non enregistré », ils ont une IP mais ne contactent jamais NCE.

**Diagnostic** : (1) Vérifier que le DHCP distribue bien l'option contrôleur (`ipconfig`/`dhcp-lease` côté équipement ou capture). (2) Tester la connectivité IP équipement → NCE (ping). (3) Vérifier que l'ESN est pré-déclaré / que la politique d'auto-enregistrement l'autorise. (4) Contrôler les flux pare-feu (le ping passe mais pas le port d'enregistrement ?).

**Causes fréquentes** : option DHCP mal formatée ou sur le mauvais scope, ESN non déclaré, route manquante vers NCE, pare-feu.

**Résolution** : corriger l'option DHCP, déclarer les ESN, ouvrir les flux — puis **réinitialiser** un équipement test et rejouer la séquence complète. **Leçon** : toujours valider le ZTP sur 1 équipement avant d'en brancher 50.

## 125. Cas pratique 2 — Un AP361 qui ne remonte pas dans NCE

**Symptôme** : l'AP est branché, mais n'apparaît ni dans NCE ni sur le WAC.

**Diagnostic** : (1) L'AP est-il **alimenté** ? (LED, test PoE du port du S310 — l'AP361 a besoin de PoE 802.3af ; un port non-PoE = AP mort). (2) A-t-il une IP ? (DHCP du site fonctionnel ?). (3) Voit-il le WAC ? (option DHCP/DNS du WAC, connectivité CAPWAP UDP/5246-5247). (4) Le WAC lui-même est-il géré par NCE ?

**Causes fréquentes** : PoE absent/désactivé, DHCP en panne, WAC non joignable, AP déjà enrôlé ailleurs (eKit ! — voir cas 19).

**Résolution** : traiter dans l'ordre **énergie → IP → WAC → NCE** (toujours du physique vers le logique). **Leçon** : 80 % des « problèmes NCE » sont des problèmes d'alimentation ou de DHCP.

## 126. Cas pratique 3 — Un AP761 extérieur qui reste hors ligne

**Symptôme** : l'AP761 fonctionnait, il est passé « hors ligne » après un orage.

**Diagnostic** : (1) Sécurité d'abord : ne pas intervenir sous orage. (2) Vérifier l'alimentation PoE (injecteur/switch — l'injecteur a-t-il pris la foudre ?). (3) État du câble extérieur et des connecteurs (infiltration ?). (4) Terre du mât et parafoudre (visuel). (5) Si l'AP répond au ping mais pas au contrôleur : vérifier les flux.

**Causes fréquentes** : surtension (injecteur grillé), infiltration d'eau, câble rongé/cisaillé, terre défectueuse.

**Résolution** : remplacer l'injecteur/contrôler le câble, refaire l'étanchéité, vérifier la terre au mégohmmètre. **Leçon** : un AP extérieur = un **actif exposé** — l'inspection annuelle (section 58) n'est pas optionnelle, et le stock de pièces (injecteur, câble) doit exister.

## 127. Cas pratique 4 — Un S310 qui boucle en enregistrement

**Symptôme** : le S310 apparaît « en enregistrement » puis disparaît, en boucle.

**Diagnostic** : (1) Version logicielle du S310 vs version minimale requise par NCE (matrice de compatibilité) — un firmware trop ancien = échec de négociation. (2) Date/heure du switch (NTP) — un certificat rejeté pour cause d'horloge fausse. (3) Doublon d'ESN (équipement déjà déclaré ailleurs). (4) Logs NCE côté onboarding.

**Résolution** : mettre à jour le firmware **manuellement** (USB/console) vers la version minimale, régler l'horloge, lever le doublon. **Leçon** : l'audit de versions **avant** la vague d'onboarding évite ce cas (checklist section 50).

## 128. Cas pratique 5 — Un AR720 qui ne télécharge pas sa config

**Symptôme** : l'AR720 s'enregistre mais le template ne s'applique pas (ou partiellement).

**Diagnostic** : (1) Le template est-il bien **affecté** au site/à l'équipement ? (2) Les **variables** sont-elles renseignées ? (un template avec une variable vide = échec silencieux ou partiel). (3) La taille du template vs les capacités (un AR d'entrée de gamme n'accepte pas tout). (4) Les écarts affichés par la vérification de conformité (get-config vs attendu).

**Résolution** : renseigner les variables manquantes, simplifier le template, appliquer par étapes. **Leçon** : un template se **teste variable par variable** en maquette — jamais directement sur l'AR de production d'un site distant.

## 129. Cas pratique 6 — Un template qui casse la prod

**Symptôme** : après application d'une nouvelle version de template, un site perd son accès (VLAN de management écrasé, uplink en erreur...).

**Réaction immédiate** : (1) Ne pas paniquer, ne pas multiplier les changements. (2) **Rollback** vers la version précédente du template (section 87) — c'est pour ça qu'elle existe. (3) Si le management est coupé : intervention locale (console) avec la procédure d'urgence.

**Analyse** : rejouer le template en maquette, identifier la ligne fautive (souvent : une variable non substituée, un VLAN oublié sur un trunk, une ACL trop restrictive). Corriger, **re-tester**, re-versionner.

**Leçons** : vagues de déploiement (section 84), jamais de big bang ; fenêtre de maintenance ; template relu par un pair ; rollback testé. Ce cas arrive à tout le monde — ce qui distingue les équipes, c'est le **temps de retour**, pas l'absence d'erreur.

## 130. Cas pratique 7 — Licence dépassée : que se passe-t-il ?

**Symptôme** : le compteur device-days passe en négatif ; alerte « license exceeded ».

**Ce qui se passe** : période de **grâce de 30 jours** (documentée) — le système continue de fonctionner. C'est le délai pour régulariser, pas pour ignorer.

**Conduite** : (1) Vérifier le compteur : croissance normale ou anomalie (équipements en double ? site oublié ?). (2) Commander le complément (procédure d'achat d'urgence — section 29). (3) Importer et valider la licence. (4) Post-mortem : pourquoi l'alerte à 80 % n'a-t-elle pas été vue ? (mettre en place le suivi trimestriel).

**Leçon** : la licence est un **poste d'exploitation**, pas un achat ponctuel — elle a sa ligne au budget et son responsable.

## 131. Cas pratique 8 — Mise à jour firmware massive qui échoue à mi-parcours

**Symptôme** : sur 60 AP, 40 sont à jour, 20 sont restés à l'ancienne version (ou en échec).

**Diagnostic** : (1) Les 20 en échec ont-ils téléchargé le package ? (serveur de fichiers joignable ? espace disque sur l'AP ?). (2) Ont-ils redémarré ? (3) La version est-elle compatible avec leur modèle exact ? (4) Les logs de la tâche d'upgrade dans NCE.

**Résolution** : relancer la tâche **uniquement** sur les en-échec (pas sur tout le monde), après correction de la cause (espace, connectivité, compatibilité). Vérifier la conformité post-upgrade.

**Leçons** : vagues (section 85), ne jamais upgrader 100 % d'un coup ; garder la version précédente validée pour downgrade ; planifier en fenêtre de maintenance (les AP redémarrent = coupure Wi-Fi).

## 132. Cas pratique 9 — Roaming Wi-Fi dégradé après migration NCE

**Symptôme** : après la migration sous NCE, les appels Wi-Fi coupent en se déplaçant (alors que ça marchait avant).

**Diagnostic** : (1) Le profil SSID migré a-t-il bien **802.11k/v/r** activés ? (souvent oubliés dans la retranscription — section 76). (2) Les puissances/canaux sont-ils les mêmes qu'avant ? (le RRM a peut-être tout rebattu). (3) Le recouvrement de couverture est-il suffisant ? (mesure terrain). (4) Les logs de roaming par client dans NCE.

**Résolution** : réactiver k/v/r, figer temporairement le RRM le temps de stabiliser, ajuster les puissances, **tester en marchant avec un appel réel**.

**Leçon** : la migration WLAN = **retranscription + re-validation radio**, pas un simple transfert de SSID. Le test d'acceptation, c'est l'utilisateur qui marche, pas le ping.

## 133. Cas pratique 10 — 802.1X qui rejette tous les utilisateurs un lundi matin

**Symptôme** : à 8 h, personne ne se connecte en filaire ; les ports restent bloqués.

**Diagnostic express** : (1) Le **RADIUS** répond-il ? (service NCE/authentification, connectivité, certificats). (2) L'**AD/LDAP** est-il joignable ? (un AD en panne = tout le monde rejeté). (3) Y a-t-il eu un **changement** ce week-end ? (certificat EAP expiré ? mise à jour ?). (4) Les logs RADIUS : la cause exacte du rejet (identifiants ? certificat client ?).

**Réaction** : si la cause n'est pas trouvée en < 30 min, basculer les ports critiques en **mode ouvert/monitor** temporaire (procédure d'urgence écrite à l'avance) pour rétablir le service, puis diagnostiquer à tête froide.

**Leçons** : le 802.1X a un **mode dégradé** prévu et testé ; les certificats EAP sont suivis (section 106) ; tout changement le week-end = vérification du 802.1X le lundi à 7 h 30 par l'astreinte.

## 134. Cas pratique 11 — Le contrôleur NCE ne répond plus

**Symptôme** : le portail est inaccessible, les API ne répondent pas.

**Rappel crucial** : le réseau **continue de fonctionner** avec la dernière configuration (le plan de données est local) — ce qui s'arrête : supervision, nouveaux déploiements, authentification centralisée (sauf composant local).

**Diagnostic** : (1) La VM tourne-t-elle ? (hyperviseur). (2) Espace disque plein ? (cause n°1 des plantages applicatifs — logs/bases). (3) Services NCE démarrés ? (4) Réseau (la VM a-t-elle perdu son IP ?).

**Résolution** : selon la cause (libérer l'espace, redémarrer les services, basculer sur le nœud HA, restaurer depuis le backup en dernier recours — section 108).

**Leçons** : supervision **du** contrôleur par Zabbix (qui supervise le superviseur), alerte disque à 75 %, exercice de bascule HA annuel, backup testé.

## 135. Cas pratique 12 — Faux positifs d'alarmes après un orage

**Symptôme** : après un orage, NCE affiche des dizaines d'alarmes (ports, AP, télémétrie).

**Diagnostic** : trier — (1) Alarmes **physiques réelles** (équipement vraiment down → intervention, voir cas 3). (2) Alarmes **transitoires** (micro-coupures, flaps — acquitter après vérification). (3) **Faux positifs ML** (l'anomalie détectée n'en est pas une — le modèle apprend).

**Conduite** : traiter le réel d'abord (site par site), acquitter le transitoire avec commentaire, **marquer** les faux positifs pour affiner les seuils (le ML a besoin de retours).

**Leçon** : après chaque événement météo majeur, prévoir une **ronde d'alarmes** (30 min) avant de crier au désastre — et affiner les seuils au fil des événements.

## 136. Cas pratique 13 — Un site distant perd le lien vers NCE

**Symptôme** : tout un site passe « hors ligne » dans NCE.

**Diagnostic** : (1) Le lien WAN est-il coupé ? (opérateur, AR720, 4G de secours). (2) Si le lien est OK : routage ? pare-feu ? (3) L'AR720 du site répond-il ?

**Comportement** : le site **continue de fonctionner** localement (dernière config) ; l'authentification continue si le **composant d'authentification local** est déployé (section 21) — sinon, les nouvelles authentifications 802.1X peuvent échouer (prévoir le mode dégradé).

**Résolution** : rétablir le lien (opérateur/secours), vérifier la resynchronisation (les équipements repassent « normal », les configs se resync).

**Leçon** : pour les sites critiques, **composant d'authentification local + lien de secours** — le centralisé pur est un pari sur le WAN.

## 137. Cas pratique 14 — Conflit entre config manuelle CLI et NCE

**Symptôme** : NCE signale des **écarts de conformité** sur un switch ; un technicien « a juste ajouté une ligne en CLI ».

**Diagnostic** : la vérification de conformité (get-config vs attendu) montre exactement les différences — c'est fait pour ça.

**Conduite** : (1) Comprendre **pourquoi** le technicien est passé en CLI (urgence ? template incomplet ? habitude ?). (2) Si la modif est légitime : l'intégrer **au template** (versionner, déployer proprement). (3) Si non : l'écraser par le template (NCE = source de vérité). (4) Dans les deux cas : en parler en REX, pas en sanction (la première fois).

**Leçon** : tout besoin récurrent de CLI = un **template incomplet** — le corriger plutôt que de verrouiller le CLI (qui reste nécessaire en dépannage).

## 138. Cas pratique 15 — Certificat expiré : tout s'arrête

**Symptôme** : portail NCE inaccessible (certificat HTTPS expiré) et/ou EAP en échec massif (certificat du serveur RADIUS expiré).

**Réaction** : (1) Confirmer (navigateur/logs : « certificate expired »). (2) Appliquer la **procédure de renouvellement d'urgence** (régénérer/déployer — section 106). (3) Vérifier tous les services après renouvellement.

**Leçon** : le registre des certificats avec alertes J-90/J-60/J-30 (section 106) aurait évité ça. C'est l'incident le plus **évitable** de cette liste — le mettre au plan de maintenance préventive avec un responsable nommé.

## 139. Cas pratique 16 — Restauration après sinistre du serveur NCE

**Symptôme** : le serveur NCE est perdu (panne matérielle, ransomware...).

**Procédure** : (1) Reconstruire la plateforme (nouvelle VM selon la doc d'installation, **même version** NCE). (2) Restaurer le **backup applicatif** (section 108). (3) Restaurer la licence (fichier archivé). (4) Vérifier : inventaire, un onboarding test, alarmes, API, supervision. (5) Les équipements se **reconnectent** au contrôleur restauré (ils gardent leur config locale en attendant).

**Leçon** : le **RTO** (temps de restauration) se connaît parce qu'on l'a **testé** (exercice annuel), pas parce qu'on l'a estimé. Archiver hors site : le backup sur le même rack que le serveur, c'est pas un backup.

## 140. Cas pratique 17 — Montée en charge : NCE sature

**Symptôme** : portail lent, alarmes en retard, traitements qui s'accumulent — après l'ajout de 200 équipements.

**Diagnostic** : (1) Ressources de la VM (CPU/RAM/disque/IOPS — la télémétrie et les bases sont gourmandes). (2) Rétention des données (trop longue ? granularité trop fine ?). (3) Le dimensionnement initial prévoyait-il cette charge ?

**Résolution** : augmenter les ressources (si VM), ajuster la **rétention** (données fines 30 j, agrégées au-delà), répartir la télémétrie, et si structurel : re-dimensionner avec l'outil officiel (voire ajouter un nœud selon l'architecture).

**Leçon** : le dimensionnement se fait pour **l'année 3**, pas pour le jour 1 (section 23-25) — avec des paliers de croissance planifiés.

## 141. Cas pratique 18 — Intégration RADIUS/LDAP qui casse l'authentification admin

**Symptôme** : après le raccordement de NCE à l'AD, plus personne ne se connecte au portail (même les comptes qui marchaient).

**Diagnostic** : (1) Le connecteur LDAP pointe-t-il sur le bon serveur/port ? (2) Le compte de liaison (bind) est-il valide (mot de passe expiré ?). (3) Les groupes AD sont-ils bien mappés aux rôles NCE ? (4) L'AD est-il joignable depuis NCE (pare-feu) ?

**Filet** : le **compte local de secours** (break-glass, section 103) permet de se connecter et de réparer — c'est exactement pour ça qu'il existe.

**Leçon** : toute intégration d'authentification se fait **en maintenant un accès local de secours**, et se teste avec un compte de test avant de basculer les admins.

## 142. Cas pratique 19 — eKit et NCE se disputent un AP

**Symptôme** : un AP enrôlé autrefois dans eKit refuse de s'enregistrer dans NCE (ou disparaît de NCE pour réapparaître dans eKit).

**Cause** : l'AP reste **lié** à la plateforme cloud eKit (il cherche à s'y enregistrer).

**Résolution** : **désenrôler proprement** l'AP d'eKit (retirer de la plateforme), le **réinitialiser** en configuration d'usine, puis l'onboarder dans NCE (ZTP ou manuel).

**Leçon** : un équipement = **une** plateforme de gestion. L'inventaire de migration doit tracer la plateforme d'origine de chaque équipement (eKit, eSight, local) pour prévoir le désenrôlement.

## 143. Cas pratique 20 — Audit de sécurité : durcir en urgence

**Symptôme** : un audit révèle : comptes partagés, SNMP v2c en `public`, portail exposé sans restriction, pas de revue des rôles depuis 2 ans.

**Plan d'action (30-60-90 jours)** :
- **J+30** : changer les mots de passe, supprimer les comptes partagés (comptes nominatifs), passer en SNMPv3, restreindre l'accès au portail (VPN/allowlist).
- **J+60** : revoir tous les rôles (moindre privilège), activer/renforcer l'audit, exporter les logs vers le SIEM, inventorier les certificats.
- **J+90** : test d'intrusion ciblé ou audit de configuration, exercice de restauration, documentation à jour.

**Leçon** : appliquer la checklist de durcissement (section 105) **dès l'installation** — le rattrapage coûte toujours plus cher que la prévention, et l'audit n'est pas une surprise si on s'auto-audite chaque année.

## 144. Cas pratique 21 — Migration eSight → NCE d'un site pilote

**Contexte** : premier site migré (12 switches S310, 18 AP361, 1 AR720).

**Déroulé** : (1) Inventaire et ESN relevés depuis eSight + terrain. (2) Templates du site créés à partir des configs eSight **retranscrites** (pas copiées). (3) Maquette : 1 switch + 2 AP validés. (4) Fenêtre un samedi : désenrôlement eSight, onboarding NCE (ZTP pour les AP, manuel assisté pour les switches — pas de réinitialisation massive sans filet). (5) Tests : 802.1X avec 3 profils, SSID, débit, roaming. (6) 72 h d'observation avant clôture.

**Incident rencontré** : 2 AP restés en « non enregistré » (ESN mal recopiés — erreur de saisie). Corrigé en 20 min grâce à la liste blanche vérifiée à la réception.

**Leçon** : le pilote sert à **découvrir** les problèmes — le planifier comme tel (temps, astreinte, droit à l'échec), et en faire un REX écrit avant la vague suivante.

## 145. Cas pratique 22 — Rapport mensuel pour la direction

**Besoin** : chaque mois, la direction veut une page sur l'état du réseau.

**Mise en œuvre** : (1) Définir les **4-5 indicateurs** qui parlent à la direction : disponibilité par site (%), incidents majeurs (nombre, durée), expérience Wi-Fi (taux de satisfaction ou plaintes), projets en cours, risques à venir. (2) Construire le rapport **automatique** dans NCE (planifié, envoyé par mail). (3) Le compléter d'un **commentaire humain** (3 lignes : ce qui s'est bien passé, ce qui a coincé, ce qu'on fait).

**Leçon** : un rapport automatique sans commentaire = un PDF que personne ne lit. Le commentaire du chef de service, c'est la valeur ajoutée — et c'est ce qui justifie les budgets.

## 146. Cas pratique 23 — Supervision croisée NCE + Zabbix

**Contexte** : Zelef supervise déjà ses onduleurs dans Zabbix ; il veut corréler avec NCE.

**Mise en œuvre** : (1) NCE expose ses alarmes via l'**API** (section 90) ; un script (ou connecteur) les injecte dans Zabbix (ou Zabbix interroge l'API périodiquement). (2) Dans Zabbix : corrélation « switch down + onduleur sur batterie sur le même site » = cause probable électrique. (3) Zabbix supervise **NCE lui-même** (services, disque, certificats — section 91).

**Leçon** : NCE ne remplace pas la supervision transverse — il l'**alimente**. L'architecture « NCE = réseau Huawei, Zabbix = transverse, GLPI = tickets » donne à chacun son rôle sans doublon.

## 147. Cas pratique 24 — PPSK pour les invités d'un hôtel

**Contexte** : un site de type hôtelier veut un Wi-Fi invité simple mais traçable, sans portail compliqué.

**Mise en œuvre** : (1) SSID `INVITES`, VLAN dédié, isolation clients, débit plafonné. (2) **PPSK** : une clé par chambre (ou par lot de chambres), générée dans NCE, imprimée sur la fiche d'accueil, **à durée limitée** (durée du séjour). (3) Révocation au départ (ou expiration automatique). (4) Journalisation des connexions (obligations locales — **à vérifier**).

**Leçon** : le PPSK est le bon compromis entre le PSK partagé (qui fuit) et le portail (lourd) — à condition d'en gérer le **cycle de vie** (création, expiration, révocation), ce que NCE fait nativement.

## 148. Cas pratique 25 — Coupure électrique et reprise (lien onduleurs)

**Contexte** : coupure secteur sur un site ; l'onduleur tient 30 minutes ; le groupe ne démarre pas.

**Ce qui se passe côté NCE** : les équipements s'éteignent par zone (alarmes « hors ligne » en cascade) ; à la reprise, ils **redémarrent** et se reconnectent au contrôleur (qui n'a rien perdu — il était sur onduleur+groupe au central, évidemment).

**Conduite** : (1) Côté énergie : procédure onduleur/groupe (le métier de Zelef — voir son guide onduleurs). (2) Côté NCE : **ne rien toucher** pendant la coupure (les alarmes sont le symptôme, pas le problème) ; à la reprise, vérifier la **resynchronisation** (tous les équipements « normal », conformité OK — un redémarrage brutal peut révéler un firmware corrompu). (3) REX croisé énergie/réseau.

**Leçon** : la **priorité d'extinction/allumage** se pense à l'avance (d'abord les charges non critiques, les équipements réseau en dernier à l'extinction et en premier à la reprise si possible) — et le NCE du site central doit être sur la chaîne ondulée la plus protégée (voir cas 11).

---

# PARTIE 15 — LIMITES HONNÊTES

## 149. Limite 1 — Courbe d'apprentissage

NCE-Campus n'est pas « eSight en mieux », c'est un **nouveau métier** : concepts SDN (YANG, intent), templates versionnés, politiques 802.1X, ZTP, API, licences device-day. Compter **plusieurs semaines** de formation par administrateur + **2-3 mois** de pratique accompagnée avant l'autonomie — et un **référent** qui monte en expertise sur 6-12 mois.

Conséquence managériale : ne pas lancer NCE en même temps qu'un autre gros projet ; sanctuariser du temps de formation (pas « entre deux tickets ») ; accepter une phase de productivité réduite. Une équipe qui n'a pas le temps d'apprendre fera du NCE un eSight cher et compliqué.

## 150. Limite 2 — Dépendance au contrôleur (single point of management)

Centraliser le pilotage, c'est centraliser le risque **de gestion** (pas de trafic — le plan de données reste local, rappel section 22). Si NCE est indisponible : plus de supervision, plus de déploiement, authentification centralisée fragilisée.

Mitigations réelles (pas des slogans) : **HA** déployée et testée, **composants d'authentification locaux** sur les sites critiques, **backups testés**, supervision du contrôleur, procédures dégradées écrites. Avec ça, la dépendance est **gérée** — sans ça, c'est une bombe à retardement. Le dire en comité de pilotage n'est pas du pessimisme, c'est du professionnalisme.

## 151. Limite 3 — Coût des licences

Le modèle device-day en souscription, c'est un **abonnement à vie** : on paie chaque année pour continuer à gérer son propre réseau. Sur 5 ans, le TCO peut dépasser largement un eSight perpétuel — surtout si le parc est stable (l'automatisation ne « rembourse » pas grand-chose sur un réseau qui ne change pas).

Points de négociation : durée (3 ans souvent mieux que 1 an), cotermination, marge de croissance incluse, **clause de sortie** (que se passe-t-il si on ne renouvelle pas ? — à écrire noir sur blanc), prix des extensions. Et surtout : **modéliser le TCO avant de signer** (section 121), pas après.

## 152. Limite 4 — Maturité sur petit site (eKit suffit)

Sur un petit site (quelques switches, une poignée d'AP, besoins simples), NCE-Campus est **surdimensionné** : coût, complexité et formation pour un bénéfice marginal — eKit fait le travail pour une fraction du prix et de l'effort.

Le vrai risque : déployer NCE « pour voir » sur un petit site, le sous-exploiter, et en conclure que « NCE ne sert à rien » — alors que c'est le **périmètre** qui était mauvais, pas l'outil. NCE se juge sur un périmètre à sa mesure (multi-sites, 802.1X, automatisation) — partout ailleurs, eKit/eSight sont des choix **respectables**, pas des aveux d'échec.

## 153. Limite 5 — Écosystème et documentation

Constats honnêtes :

- La documentation est **abondante mais inégale** : brochures marketing nombreuses, guides techniques parfois ardus, versions qui se succèdent vite (R020/R022/R024...) — toujours travailler sur la **doc de la version exacte** déployée.
- La **communauté** francophone/utilisateurs est plus petite que celles de Cisco/Aruba — moins de retours d'expérience, moins de scripts partagés.
- L'écosystème API existe (500+ API, sandbox) mais demande un investissement pour être exploité.
- Le support passe largement par le **partenaire/intégrateur** : sa qualité fait la moitié du projet — le choisir avec autant de soin que le produit.

## 154. Limite 6 — Dépendance au support Huawei/partenaire

NCE-Campus n'est pas un produit qu'on achète et qu'on oublie : versions fréquentes, matrices de compatibilité à suivre, licences à renouveler, évolutions de l'écosystème. Cela crée une **dépendance durable** au constructeur et à l'intégrateur.

La gérer : exiger la **documentation** de tout ce qui est fait par l'intégrateur (templates, procédures — pas de boîte noire), former un **référent interne** (jamais 100 % dépendant), maintenir un **contrat de support** avec des SLA écrits (délais de réponse/résolution), et garder une **porte de sortie** documentée (export des configurations, procédure de retour à une gestion locale si nécessaire — même si on ne s'en sert jamais, l'avoir change le rapport de force).

## 155. Synthèse honnête — faut-il y aller ?

**Oui, si** : parc Huawei campus significatif et/ou multi-sites, besoin de 802.1X/automatisation, équipe (petite mais) formable, budget projet avec TCO modélisé, sponsor direction. Dans ce cas, NCE-Campus est un vrai **saut capacitaire** : ZTP, templates, politiques centralisées, assurance — on change de division.

**Non (ou pas maintenant), si** : petit site stable (eKit), parc multi-vendeurs/énergie dominant (eSight/Zabbix), pas de budget projet, équipe saturée sans perspective de formation. Dans ce cas, NCE serait un **boulet** : cher, compliqué, sous-exploité — et c'est le produit qui serait blâmé pour une mauvaise décision de périmètre.

**La voie sage** : pilote sur un site représentatif (3-6 mois), REX honnête, décision go/no-go sur données réelles — pas sur des promesses commerciales. Et dans tous les cas : eSight reste **légitime** sur ses périmètres (supervision transverse, énergie) — la coexistence n'est pas un échec, c'est une architecture.

---

# PARTIE 16 — QUIZ

## 156. Quiz — 10 questions

**Q1.** Quelles sont les trois fonctions intégrées par iMaster NCE-Campus (qui le distinguent d'un NMS traditionnel comme eSight) ?

**Q2.** Citez trois protocoles southbound utilisés par NCE-Campus pour dialoguer avec les équipements, et le rôle de chacun.

**Q3.** Qu'est-ce qu'un « device-day » dans le modèle de licence NCE, et que se passe-t-il quand le compteur passe en négatif ?

**Q4.** Décrivez la séquence complète d'un onboarding ZTP via DHCP (5 étapes).

**Q5.** Un AP361 branché n'apparaît pas dans NCE. Citez les 4 niveaux de diagnostic dans l'ordre, du physique au logique.

**Q6.** Pourquoi faut-il désenrôler un AP d'eKit avant de l'onboarder dans NCE ? Que faire s'il « retourne » vers eKit ?

**Q7.** En 802.1X, quels sont les trois rôles (supplicant, authentificateur, serveur) et quel équipement Huawei joue typiquement l'authentificateur en filaire ?

**Q8.** Un template appliqué en production coupe l'accès d'un site. Quelle est la première action correcte, et quelles sont les trois mesures préventives qui auraient dû être en place ?

**Q9.** Le contrôleur NCE devient injoignable. Le réseau s'arrête-t-il ? Qu'est-ce qui s'arrête vraiment, et quelle brique permet aux branches de continuer à authentifier localement ?

**Q10.** Citez trois périmètres pour lesquels eSight reste plus pertinent que NCE-Campus, et justifiez en une phrase chacun.

## 157. Quiz — réponses détaillées

**R1.** **Management** (gestion : inventaire, supervision, alarmes — le rôle NMS), **Control** (contrôle SDN : orchestration des configurations/politiques via NETCONF/YANG, ZTP, templates), **Analysis** (analyse : télémétrie, big data/ML, détection d'anomalies, cause racine — avec CampusInsight en option avancée). C'est l'intégration des trois qui distingue NCE d'eSight (manager seul).

**R2.** **NETCONF/YANG** : configuration structurée (déploiement, vérification de conformité, notifications de changement). **SNMP** : supervision et télémétrie classique, gestion des équipements traditionnels, base des sondes SLA/NQA. **CAPWAP** : tunnels de contrôle/gestion entre le WAC et les AP AirEngine. (Acceptés aussi : HTTP/2 pour la télémétrie, HTTPS pour les portails/gestion web, TCP pour la synchro des composants d'authentification.)

**R3.** Un device-day = **un équipement géré pendant un jour** ; les licences sont poolées et consommées au fil du temps. Quand le compteur passe en négatif, une **période de grâce de 30 jours** s'enclenche (documentée) : le système continue de fonctionner, c'est le délai pour régulariser (racheter des licences) — pas pour ignorer le problème.

**R4.** (1) L'équipement neuf démarre et demande une IP au **DHCP**. (2) Le DHCP lui attribue une IP **et l'option** contenant l'adresse du contrôleur NCE. (3) L'équipement **contacte NCE et s'enregistre** (ESN vérifié — pré-déclaration/liste blanche). (4) NCE l'**affecte au site** et pousse le **template** (+ firmware si besoin). (5) L'équipement applique, redémarre si nécessaire, passe à l'état **normal** (vérification de conformité).

**R5.** Dans l'ordre : (1) **Énergie** — l'AP est-il alimenté ? (PoE 802.3af du S310 : LED, état PoE du port). (2) **IP** — a-t-il une adresse ? (DHCP fonctionnel ?). (3) **WAC** — voit-il le contrôleur WLAN ? (option DHCP/DNS, CAPWAP UDP/5246-5247). (4) **NCE** — le WAC est-il géré par NCE, l'AP remonte-t-il ? Toujours du physique vers le logique : 80 % des cas se règlent aux niveaux 1-2.

**R6.** Parce qu'un équipement ne peut être piloté que par **une seule** plateforme : tant qu'il est lié à eKit (cloud), il cherche à s'y enregistrer et refuse/ignore NCE. Il faut le **désenrôler proprement d'eKit**, le **réinitialiser** en configuration d'usine, puis l'onboarder dans NCE. S'il « retourne » vers eKit : vérifier que le désenrôlement est effectif côté cloud eKit (pas seulement local).

**R7.** **Supplicant** : le poste qui demande l'accès (client 802.1X). **Authentificateur** : le **switch** (ex : S310) — il relaie l'EAP et applique la décision (ouvre/bloque le port, VLAN dynamique). **Serveur d'authentification** : le RADIUS (composant NCE ou externe) — il décide en vérifiant les identifiants dans l'annuaire et renvoie les attributs d'autorisation.

**R8.** Première action : **rollback** vers la version précédente du template (ne pas improviser d'autres changements ; intervention console locale si le management est coupé). Trois mesures préventives : (1) **déploiement par vagues** (maquette → pilote → généralisation, jamais de big bang) ; (2) **fenêtre de maintenance** planifiée et communiquée ; (3) **template versionné, testé en maquette et relu par un pair**, avec procédure de rollback écrite et testée.

**R9.** **Non, le réseau ne s'arrête pas** : le plan de données est local aux équipements, qui continuent de commuter avec leur dernière configuration (le réseau se « fige », il ne tombe pas). Ce qui s'arrête : supervision, nouveaux déploiements/changements de politiques, authentification **centralisée**. La brique qui permet aux branches de continuer à authentifier localement est le **composant d'authentification** déployé sur les sites distants (jusqu'à 20, synchronisé avec le central via TCP).

**R10.** (1) **Supervision multi-vendeurs** : NCE ne gère pratiquement que du Huawei, eSight supervise les équipements tiers en SNMP. (2) **Énergie/onduleurs** : la supervision des UPS et équipements techniques via SNMP générique est le domaine d'eSight (ou d'un superviseur dédié) — NCE n'est pas un superviseur d'infrastructure technique. (3) **PON/OLT/ONT et data center** : périmètres couverts par eSight, hors scope de NCE-Campus. Bonus accepté : la **simplicité/coût** sur petit parc stable.

---

# PARTIE 17 — GLOSSAIRE

## 158. Glossaire

- **802.1X** : standard de contrôle d'accès réseau par authentification (EAP) avant ouverture d'un port / d'une association Wi-Fi.
- **AAA** : Authentication, Authorization, Accounting — les trois fonctions de la gestion des accès.
- **ADN (Autonomous Driving Network)** : concept Huawei de réseau à gestion autonome (niveaux 1 à 5, du pilotage assisté à l'autonomie complète — vision).
- **API NBI (Northbound Interface)** : interface RESTful exposée par NCE vers les applications (portail, ITSM, scripts) — port documenté 18002, authentification par token `X-ACCESS-TOKEN`.
- **Authentificateur** : en 802.1X, l'équipement (switch/WAC) qui relaie l'authentification et applique la décision.
- **BYOD (Bring Your Own Device)** : usage d'équipements personnels sur le réseau d'entreprise.
- **CampusInsight** : composant d'analyse big data/ML de NCE-Campus (expérience utilisateur, prédiction, optimisation) — souscription séparée.
- **CAPWAP** : protocole de contrôle et de provisionnement des AP sans-fil (tunnels WAC ↔ AP).
- **Composant d'authentification** : service NCE déployable sur les branches (jusqu'à 20) pour l'authentification locale en cas de perte du lien central.
- **DCA (Dynamic Channel Assignment)** : ajustement automatique des canaux radio des AP (RRM).
- **Device-day** : unité de licence NCE = un équipement géré pendant un jour ; licences poolées, grâce de 30 jours en cas de dépassement.
- **EAP** : Extensible Authentication Protocol — cadre des méthodes d'authentification 802.1X (TLS, PEAP, TTLS...).
- **ESDP** : plateforme Huawei de distribution des logiciels/licences (commande et téléchargement).
- **ESN** : Electronic Serial Number — identifiant unique d'un équipement, clé de l'enregistrement ZTP.
- **Intent-based networking** : pilotage par intention (« qui accède à quoi ») traduit automatiquement en configurations.
- **MAB (MAC Authentication Bypass)** : authentification par adresse MAC pour les équipements sans 802.1X (faible sécurité — liste blanche).
- **MSP** : Managed Service Provider — prestataire gérant plusieurs tenants depuis un NCE mutualisé (licences réallouables).
- **NAC (Network Access Control)** : contrôle d'accès au réseau (802.1X, MAB, portail).
- **NETCONF** : protocole de configuration réseau structurée (RFC 6241) ; **YANG** : langage de modélisation des données (RFC 7950).
- **NQA (Network Quality Analysis)** : sondes actives des équipements Huawei pour mesurer la qualité (base du SLA management).
- **OPEX / CAPEX** : dépenses d'exploitation / d'investissement.
- **PPSK (Private Pre-Shared Key)** : clé pré-partagée individuelle (par utilisateur/groupe) avec VLAN/ACL dédiés — alternative au PSK partagé.
- **Qos (Quality of Service)** : mécanismes de priorisation du trafic (DiffServ, DSCP, policing).
- **RBAC (Role-Based Access Control)** : gestion des droits par rôles.
- **RRM (Radio Resource Management)** : gestion automatique des ressources radio (DCA, TPC).
- **SBI (Southbound Interface)** : protocoles entre le contrôleur et les équipements (NETCONF, SNMP, CAPWAP...).
- **SLA** : Service Level Agreement — engagement de qualité de service, mesuré ici via NQA.
- **SNMPv3** : version sécurisée de SNMP (authentification + chiffrement — `authPriv`).
- **SSID** : Service Set Identifier — nom du réseau Wi-Fi diffusé par les AP.
- **TCO (Total Cost of Ownership)** : coût total de possession sur une durée (licences + matériel + services + exploitation).
- **Télémétrie** : collecte de données d'état/performance des équipements (polling SNMP ou streaming HTTP/2).
- **Template** : modèle de configuration versionné avec variables, appliqué par NCE aux équipements.
- **TPC (Transmit Power Control)** : ajustement automatique de la puissance d'émission des AP.
- **VLAN** : Virtual LAN — segmentation logique du réseau local.
- **WAC** : Wireless Access Controller — contrôleur des AP (tunnels CAPWAP).
- **ZTP (Zero-Touch Provisioning)** : provisionnement automatique d'un équipement neuf (DHCP/e-mail/scan), sans intervention experte locale.

---

# PARTIE 18 — POUR ALLER PLUS LOIN

## 159. Pour aller plus loin — ressources et prochaines étapes

**Ressources documentaires (à consulter sur la version exacte cible)** :
- Product Overview iMaster NCE-Campus (ex : V300R022C10) — architecture et fonctions par version.
- Monitoring and O&M Guide (ex : V300R020C10) — upgrade, licences devices, signature database, SLA.
- License Usage Guide (ex : V300R022C00/V300R024C00, MSP Training Manual) — modèle device-day, mode MSP.
- Datasheet iMaster NCE-CampusInsight (ex : V100R025C00) — souscriptions d'analyse.
- Guide d'installation et guide de déploiement HA de la version cible — **la référence** pour le dimensionnement.
- Matrice de compatibilité NCE × équipements — **à exiger du partenaire** avant tout achat.
- Outil EOM/EOFS/EOS Query (e.huawei.com) — vérifier le statut de support d'eSight et des équipements.
- Écosystème développeur Huawei — API NBI, sandbox, plus de 500 API documentées.

**Prochaines étapes proposées pour Zelef** :
1. **Cadrage** : périmètre (quels sites ?), inventaire précis, TCO 5 ans eSight vs NCE avec le partenaire.
2. **Maquette** : 1 switch S310 + 2 AP361 + NCE en lab — valider ZTP, template, 802.1X, supervision.
3. **Formation** : au moins le référent (et un suppléant) formés avant le pilote.
4. **Pilote** : 1 site représentatif non critique, 3 mois, REX écrit, go/no-go.
5. **Industrialisation** : vagues par site, documentation d'exploitation, intégration Zabbix/GLPI via API.
6. **Run** : revues trimestrielles (licences, rôles, rapports), exercice de restauration annuel, veille versions.

**En lien avec tes autres guides** : ce document complète ton guide eSight (supervision transverse), tes guides AP361/AP761/S310/AR720/USG6000 (les équipements pilotés), ton guide onduleurs (l'énergie qui fait tourner tout ça — NCE ne la supervise pas, Zabbix/eSight si), et ton guide eKit (le petit site). L'architecture cible saine : **NCE** pour le campus Huawei automatisé, **eSight/Zabbix** pour le transverse et l'énergie, **eKit** pour les petits sites simples — chacun à sa place, intégrés par API là où ça compte.

---

# PARTIE 19 — APPROFONDISSEMENTS OPÉRATIONNELS

## 160. VXLAN et virtualisation du campus — quand NCE fait du réseau overlay

Au-delà du VLAN classique, NCE-Campus peut déployer des **réseaux virtuels VXLAN** : la documentation Huawei décrit le provisionnement automatisé de VN (Virtual Networks) — « un réseau multifonctionnel, services VXLAN provisionnés en minutes ».

Principe : plutôt que d'étendre des VLAN sur tout le campus (avec les limites du spanning-tree et des domaines de broadcast), on crée des **overlays VXLAN** avec des **VNI** (identifiants de segment), routés/bridgés par les équipements (VTEP). Cas d'usage : segmentation forte entre populations (ex : réseau « production », réseau « invités », réseau « IoT ») avec des politiques de sécurité entre VN.

Recommandation pragmatique : ne pas mettre du VXLAN partout « parce que c'est moderne ». Le VLAN + VRF reste plus simple et suffisant pour la plupart des campus. Réserver VXLAN aux cas où la segmentation doit traverser plusieurs sites/équipements avec des politiques centralisées — et le **tester en maquette** (le dépannage d'un overlay est plus exigeant qu'un VLAN).

## 161. SD-WAN de branche avec l'AR720 — le rôle WAN de NCE

NCE-Campus ne gère pas que le LAN : il orchestre aussi le **WAN des branches** (les AR720 de Zelef sont typiquement des routeurs de branche).

Fonctions WAN pilotées :
- **Provisionnement des liens** : Internet, MPLS, 4G/5G — avec bascule automatique (le lien de secours prend le relais sans intervention).
- **VPN site à site** : tunnels chiffrés vers le siège / entre branches, déployés par politique (pas de configuration crypto manuelle par routeur).
- **Sélection de chemin par application** : le trafic critique (voix, applicatif métier) passe par le meilleur lien disponible.
- **ZTP du routeur de branche** : l'AR720 d'une nouvelle agence se configure seul (section 59) — c'est souvent **le** cas d'usage qui vend NCE à la direction (ouvrir une agence en jours, pas en semaines).

Point d'honnêteté : le SD-WAN Huawei est mature sur les scénarios standard (dual-link, VPN) ; les architectures exotiques (multi-opérateurs avec SLA différenciés fins) demandent une validation en maquette. Et la qualité du SD-WAN ne compensera jamais un lien opérateur pourri — mesurer les liens (NQA, section 99) avant d'accuser le contrôleur.

## 162. Atelier de conception NAC — méthode pas à pas

Déployer le 802.1X/NAC est un projet dans le projet. Méthode recommandée :

1. **Inventaire des populations** : qui se connecte ? (employés, prestataires, invités, IoT, équipements techniques — ne pas oublier les copieurs !).
2. **Matrice d'autorisation** : pour chaque population → méthode d'authentification (802.1X/MAB/portail), VLAN, ACL, QoS. Tenir sur **une page** — si ça ne tient pas, simplifier.
3. **Choix des méthodes EAP** : PEAP-MSCHAPv2 pour démarrer (AD), trajectoire EAP-TLS (section 67).
4. **Infrastructure** : RADIUS (composant NCE ou externe), AD/LDAP, PKI si EAP-TLS, supplicants (GPO pour les postes Windows).
5. **Mode monitor d'abord** : déployer en **observation** (on authentifie mais on ne bloque pas) pendant 2-4 semaines pour découvrir tous les cas particuliers (la vieille imprimante du comptable, le badgeuse...).
6. **Bascule progressive** : par VLAN/bâtiment, avec fenêtre et rollback.
7. **Filets** : MAB pour le non-802.1X (liste blanche), portail pour les invités, procédure d'urgence (mode ouvert temporaire — cas pratique 10).

Le mode monitor n'est pas une option : c'est lui qui évite le « lundi matin noir ».

## 163. Dimensionner le RADIUS — ne pas l'oublier

Le RADIUS est le cœur battant du 802.1X : s'il ne répond pas (ou lentement), tout le monde est bloqué ou ralenti.

- **Capacité** : estimer les pics (le matin à 8 h, tout le monde s'authentifie en même temps — prévoir la charge de pointe, pas la moyenne).
- **Redondance** : au moins 2 serveurs RADIUS (ou le composant NCE + un secondaire), sur des sites/VM différents.
- **Timeouts** : régler les délais côté switch (trop court = rejets intempestifs, trop long = ouverture de port interminable pour l'utilisateur).
- **Supervision** : sonder le RADIUS en permanence (une sonde d'authentification test toutes les 5 min — si elle échoue, alerte critique **avant** les utilisateurs).
- **Composant local** : sur les branches à WAN fragile, le composant d'authentification NCE local (section 21) évite la dépendance totale au central.

## 164. Portail captif invités — conception détaillée

Au-delà des principes (section 79), la conception d'un portail invité propre :

- **Parcours utilisateur** : association au SSID → redirection → page (logo, CGU, choix : voucher / SMS / sponsor) → authentification → accès. Chaque étape doit marcher sur **tous** les OS (iOS, Android, Windows ont des comportements de détection de portail différents — tester les trois).
- **Vouchers** : générés en masse (événements), à durée limitée, à usage unique si possible ; traçabilité (quel voucher → qui → quand).
- **SMS** : passerelle SMS avec contrat opérateur ; prévoir le cas « pas de réseau mobile » (voucher de secours).
- **Sponsor** : un employé valide l'invité (workflow mail) — adapté aux visiteurs réguliers.
- **Sécurité** : HTTPS sur le portail, isolation des invités, débit plafonné, durée de session, journalisation (obligations locales — **à vérifier**).
- **Test** : le test d'acceptation = une personne non-technique qui se connecte en moins de 2 minutes, sans aide.

## 165. Commandes de vérification côté équipement — mémo

NCE est la source de vérité, mais le dépannage passe parfois par le CLI des équipements. Mémo des vérifications utiles (syntaxe indicative Huawei VRP — adapter à la version) :

- État général : `display version`, `display device`, `display cpu-usage`, `display memory-usage`.
- Interfaces : `display interface brief`, `display interface <nom>` (erreurs CRC, collisions).
- VLAN : `display vlan`, `display port vlan`.
- 802.1X : `display dot1x` (sessions, statistiques d'authentification).
- WLAN (WAC) : `display ap all` (état des AP), `display radio all`, `display station all` (clients).
- PoE : `display poe power-state` / informations de puissance par port.
- LLDP : `display lldp neighbor brief` (confronter à la topologie NCE).
- Logs : `display logbuffer`, `display trapbuffer`.

Règle : toute modification faite en CLI pendant un dépannage est **retranscrite ensuite dans le template** (sinon l'écart de conformité la signalera — et c'est normal).

## 166. Exemple de template — squelette commenté

Squelette indicatif d'un template « switch d'accès » (pseudo-syntaxe — la syntaxe exacte dépend de l'éditeur de templates de la version NCE) :

```
# Template: SW-ACC-STD v1.2
# Variables: {{NOM_SITE}}, {{VLAN_GESTION}}, {{VLAN_USERS}}, {{VLAN_VOIP}}, {{NTP_SRVR}}

system-view
 sysname {{NOM_SITE}}-{{NOM_EQPT}}
vlan {{VLAN_GESTION}} / description GESTION
vlan {{VLAN_USERS}}  / description USERS-802.1X
vlan {{VLAN_VOIP}}   / description VOIP
# Management
interface Vlanif{{VLAN_GESTION}} / ip address {{IP_GESTION}} {{MASQUE}}
# NTP, DNS, SNMPv3 (variables)
ntp-service unicast-server {{NTP_SRVR}}
# 802.1X sur les ports d'accès (boucle sur {{PORTS_ACCES}})
dot1x enable (par port)
# Uplink en trunk avec les VLAN autorisés
```

Principes du squelette : **variables en haut** (documentées), sections **commentées**, pas de valeur en dur spécifique à un site, version dans l'en-tête. Ce squelette se teste en maquette avant industrialisation (section 83).

## 167. Seuils d'alerte recommandés — point de départ

Point de départ à ajuster selon l'historique (jamais de seuils « sortis du chapeau » en production sans période d'observation) :

| Métrique | Seuil d'attention | Seuil critique |
|---|---|---|
| CPU équipement | > 70 % pendant 15 min | > 90 % pendant 5 min |
| Mémoire équipement | > 80 % | > 90 % |
| Utilisation de lien | > 70 % (planifier l'upgrade) | > 90 % pendant 15 min |
| Erreurs CRC sur port | > 0,1 % des paquets | augmentation brutale |
| Température | > 50 °C (selon spec) | > 60 °C / seuil constructeur |
| AP hors ligne | 1 AP (info) | > 10 % des AP d'un site |
| Échecs 802.1X | pic inhabituel vs baseline | > 20 % des tentatives sur 15 min |
| Espace disque NCE | > 70 % | > 85 % |
| Certificats | J-90 / J-60 | J-30 / J-7 |

Après 3 mois d'historique : remplacer ces seuils génériques par des **baselines** (le ML de CampusInsight aide — section 95).

## 168. Scripts d'exploitation — exemples d'usage API

Exemples de scripts à développer avec l'API NBI (logique — endpoints à valider sur la version) :

1. **Export d'inventaire quotidien** : GET équipements → CSV/CMDB (réconciliation avec GLPI).
2. **Rapport de conformité hebdo** : équipements en écart → mail au référent NCE.
3. **Création de tickets** : alarmes critiques → tickets GLPI automatiques (avec l'ID d'alarme NCE pour traçabilité).
4. **Check de certificats** : liste des certificats + expirations → alerte à J-60.
5. **Nettoyage** : lister les équipements « non enregistré » depuis > 7 jours → relance ou suppression.

Bonnes pratiques de script : compte API dédié (section 89), gestion du token (re-login sur 401), journalisation, versionnement (Git), exécution planifiée (cron) avec alerte sur échec du script lui-même.

## 169. Plan de formation — qui apprend quoi

| Public | Contenu | Durée indicative | Format |
|---|---|---|---|
| Référent NCE (+ suppléant) | Architecture, templates, ZTP, 802.1X, API, dépannage avancé | 5-10 jours + 3 mois de pratique | Formation officielle + maquette |
| Administrateurs réseau | Onboarding, supervision, maintenance, rollback | 3-5 jours | Formation + compagnonnage |
| Opérateurs NOC | Dashboards, alarmes, diagnostic de niveau 1, escalade | 1-2 jours | Atelier pratique |
| Techniciens sites | Fiche ZTP, branchement, tests de base, escalade | 0,5 jour | Fiche + démo |
| Direction / métiers | Ce que NCE change (et ne change pas) pour eux | 1 heure | Présentation |

Budgéter la formation **dans** le projet (pas après), et prévoir le **recyclage** (nouveautés de version, 1 jour/an). Le suppléant du référent n'est pas un luxe : c'est la continuité de service.

## 170. RACI d'exploitation NCE — qui fait quoi

Exemple de matrice RACI (à adapter) :

| Activité | Référent NCE | Admin réseau | NOC | Technicien site | Direction |
|---|---|---|---|---|---|
| Créer/modifier un template | R/A | C | I | — | — |
| Déployer un template (vague) | A | R | I | I | I |
| Onboarding ZTP (site) | A | C | I | R | — |
| Acquitter une alarme | I | C | R/A | — | — |
| Upgrade firmware (vague) | A | R | I | I | I |
| Gérer les licences | R/A | I | — | — | C (budget) |
| Renouveler un certificat | A | R | I | — | — |
| Restaurer NCE (sinistre) | R/A | C | I | — | I |
| Rapport mensuel | R | C | I | — | A (destinataire) |

(R = réalise, A = approuve/responsable, C = consulté, I = informé.) Sans RACI, tout le monde attend que « quelqu'un » s'occupe des licences — et personne ne le fait.

## 171. KPI d'exploitation — piloter le réseau comme un service

Indicateurs à suivre mensuellement (tableau de bord du chef de service) :

- **Disponibilité** : % par site (objectif ex : 99,5 % — à définir contractuellement).
- **MTTR** : temps moyen de résolution des incidents (par sévérité).
- **Conformité** : % d'équipements sans écart de configuration (objectif : 100 %).
- **Onboarding** : délai moyen de mise en service d'un équipement (ZTP vs manuel).
- **Licences** : % de consommation du pool device-days.
- **Changements** : nombre de déploiements, taux d'échec, taux de rollback.
- **Sécurité** : échecs 802.1X, équipements non conformes, délai de patch des vulnérabilités.
- **Expérience** : plaintes Wi-Fi / 100 utilisateurs, temps moyen d'association.

Un KPI sans objectif ni action associée est une statistique décorative. Revoir les objectifs **annuellement**.

## 172. Fiche réflexe incident — une page pour l'astreinte

Modèle de fiche (à afficher / garder sous la main) :

```
INCIDENT RÉSEAU — FICHE RÉFLEXE
1. QUALIFIER : quel site ? quels services impactés ? depuis quand ? (NCE : dashboard)
2. ISOLER : le problème est-il local (1 équipement) ou global (site) ?
3. VÉRIFIER : y a-t-il eu un CHANGEMENT récent ? (template, firmware, énergie ?)
4. ÉNERGIE : l'onduleur du site est-il OK ? (corrélation Zabbix — cas 25)
5. DÉGRADER PROPREMENT : si 802.1X bloque tout → mode ouvert temporaire (procédure)
6. ESCALADER : au-delà de 30 min sans diagnostic → référent NCE + direction si critique
7. COMMUNIQUER : message aux utilisateurs (canal prévu), mise à jour toutes les 30 min
8. TRACER : tout noter (heures, actions) pour le REX
9. REX : sous 72 h, fiche incident + actions correctives (jamais de REX = incident qui reviendra)
NE JAMAIS : multiplier les changements à l'aveugle / travailler sans filet / cacher un incident.
```

## 173. Lexique des erreurs à ne pas commettre — top 10

1. Déployer NCE sans **TCO** ni pilote — signer sur une promesse commerciale.
2. Sous-dimensionner le serveur — économiser 2 000 € pour perdre des semaines.
3. Oublier les **flux réseau** (pare-feu) avant l'onboarding — 80 % des échecs ZTP.
4. Pousser un template **non testé** en production — le big bang du vendredi soir.
5. Faire du 802.1X sans **mode monitor** — le lundi matin noir.
6. Négliger les **certificats** — l'incident le plus bête (cas 15).
7. Ne pas **tester la restauration** — un backup non testé n'existe pas.
8. Laisser les techniciens configurer en **CLI sauvage** — la conformité s'effondre.
9. Oublier la **licence** au budget annuel — la grâce de 30 jours n'est pas une stratégie.
10. Ne pas **documenter** — quand le référent part, NCE devient une boîte noire.

---

# PARTIE 20 — GOUVERNANCE, RECETTE ET CADRAGE FOURNISSEUR

## 174. Interconnexion avec l'USG6000 — où s'arrête NCE, où commence le firewall

Dans le parc de Zelef, l'USG6000 protège le périmètre. Articulation avec NCE-Campus :

- **NCE-Campus** pilote le campus (switches, AP/WAC, AR, politiques d'accès utilisateurs, QoS).
- **L'USG6000** applique les politiques de sécurité inter-zones (filtrage, NAT, VPN IPsec/SSL, prévention d'intrusion selon la licence).
- Les deux se **complètent** : NCE authentifie et segmente (qui accède à quoi, VLAN/VN), l'USG filtre les flux entre zones et vers Internet.
- Vérifier dans la **matrice de compatibilité** ce que NCE peut faire sur l'USG (supervision ? déploiement de certaines politiques ? activation de licences via NCE — la fonction existe dans la documentation Monitoring and O&M).
- Règle d'architecture : **ne jamais** gérer les règles firewall uniquement depuis NCE si l'équipe sécurité a son processus sur l'USG — définir qui administre quoi (RACI, section 170), sinon les politiques se contredisent.

## 175. Plan d'adressage — méthode

Un plan d'adressage propre conditionne tout (ZTP, supervision, dépannage) :

1. **Blocs par usage** : management (/24 ou plus par région), utilisateurs (par site/bâtiment), voix, IoT, invités, serveurs, interconnexions, loopbacks.
2. **Hiérarchie** : un supernet par site (ex : 10.<site>.0.0/16) avec des /24 par usage — la lecture d'une IP révèle le site et l'usage.
3. **Réservations** : adresses de management en début de plage (passerelle, NCE, WAC, serveurs), DHCP pools documentés (début/fin, exclusions).
4. **DNS** : nommage systématique (sw-acc-01.site.domaine, ap-101.site.domaine) — un équipement sans nom DNS est un équipement qu'on ne retrouvera pas à 3 h du matin.
5. **Documentation** : le plan d'adressage est un **document versionné** (pas un tableau blanc), relu à chaque nouveau site.
6. **IPv6** : même si non déployé aujourd'hui, réserver la réflexion (les équipements récents le supportent — éviter de devoir tout renuméroter dans 5 ans).

## 176. AAA et mots de passe des équipements — politique

Même pilotés par NCE, les équipements gardent des accès locaux (console, SSH de secours) — à sécuriser :

- **Comptes locaux** : un compte admin local par équipement (mot de passe **unique par équipement**, stocké en coffre — jamais le même partout).
- **AAA centralisé** : authentifier les accès SSH/TACACS+/RADIUS sur l'annuaire quand possible (traçabilité nominative).
- **Rotation** : politique de changement (ex : annuelle, et à chaque départ d'un admin).
- **Console** : protéger l'accès physique (locaux fermés — un accès console = un contournement total).
- **NCE comme relais** : l'accès SSH via NCE (jump) centralise les logs — à privilégier sur l'accès direct.

## 177. Procédure de recette — valider avant de payer

Chaque jalon (installation, pilote, vague) se termine par une **recette** formelle :

- **Recette d'installation** : NCE installé, licencié, HA testée (bascule), sauvegarde/restauration testée, documentation livrée.
- **Recette du pilote** : onboarding ZTP fonctionnel, template appliqué et conforme à 100 %, 802.1X validé avec les 3 profils, SSID testés, supervision nominale, REX écrit.
- **Recette par vague** : critères de la section 84 + 72 h d'observation sans incident bloquant.
- **Recette globale** : tous les sites migrés, eSight nettoyé du périmètre, documentation d'exploitation complète, formation réalisée, transfert de compétences signé.

Chaque recette = **procès-verbal signé** avec réserves éventuelles et délais de levée. Ne pas payer le solde d'une prestation sans levée des réserves — c'est le seul levier contractuel qui compte.

## 178. Dossier d'exploitation — sommaire type

Le dossier d'exploitation NCE (le « classeur » de l'équipe) :

1. Architecture (schémas à jour : physique, logique, flux).
2. Plan d'adressage et nommage.
3. Inventaire (avec ESN, versions, dates de garantie).
4. Templates (versions, changelog, variables documentées).
5. Politiques (802.1X, SSID, QoS, invités).
6. Procédures (onboarding, upgrade, rollback, restauration, certificats).
7. RACI et astreinte.
8. Seuils d'alerte et dashboards.
9. Licences (contrats, compteurs, échéances).
10. Sauvegardes (politique, tests de restauration).
11. REX d'incidents.
12. Contacts (partenaire, support Huawei, opérateurs).

Un dossier d'exploitation à jour, c'est la différence entre une équipe et une personne irremplaçable.

## 179. Questions à poser au partenaire — mini-RFP

Avant de signer, exiger des réponses **écrites** :

1. Quelle **version exacte** de NCE-Campus proposez-vous, et quelle est sa date de fin de support ?
2. Fournir la **matrice de compatibilité** version NCE × nos modèles (S310, AP361, AP761, AR720, USG6000) × versions logicielles.
3. **Dimensionnement** : fournir le calcul (outil officiel) pour notre parc à 3 ans, avec hypothèses explicites.
4. **Licences** : détail device-days/an, CampusInsight (oui/non/prix), durée, cotermination, clause de non-renouvellement.
5. **Services** : détail des jours d'intégration (installation, templates, migration, formation) — que livrez-vous exactement ?
6. **Support** : SLA écrits (délais de réponse/résolution par sévérité), périmètre (NCE seul ? équipements ?), langue.
7. **Références** : 2-3 clients comparables (taille, secteur) joignables.
8. **Roadmap eSight** : position officielle sur la coexistence et le support eSight à 3-5 ans.
9. **Sortie** : comment récupère-t-on nos configurations si on arrête ? Quel est le comportement en fin de souscription ?
10. **Formation** : programme, durée, supports, certification éventuelle.

Un partenaire qui refuse l'écrit sur ces points n'est pas un partenaire.

## 180. Feuille de route 12 mois — exemple pour Zelef

Exemple de trajectoire réaliste (à adapter) :

- **Mois 1-2** : cadrage (périmètre, inventaire, TCO, mini-RFP, choix du partenaire), commande.
- **Mois 3** : installation NCE en lab + formation du référent (et suppléant).
- **Mois 4-5** : maquette (S310 + AP361 + AR720) — ZTP, templates, 802.1X en mode monitor, supervision, API vers Zabbix.
- **Mois 6-8** : site pilote (représentatif, non critique) — migration, 72 h d'observation, REX, go/no-go.
- **Mois 9-11** : vagues par site (1-2 sites/mois selon la taille), ajustements, documentation.
- **Mois 12** : clôture — recettes, nettoyage eSight du périmètre migré, bilan TCO réel vs prévisionnel, plan année 2 (CampusInsight ? nouveaux sites ?).

Et en parallèle, toute l'année : eSight/Zabbix continuent de superviser le transverse et l'énergie — la coexistence est assumée, pas subie.

---

# PARTIE 21 — ANNEXES

## 181. Annexe A — Mémo une page : les 20 réflexes NCE

1. NCE = source de vérité — pas de CLI sauvage en production.
2. Tout template est versionné, testé en maquette, relu par un pair.
3. Déploiement par vagues — jamais de big bang.
4. ZTP validé sur 1 équipement avant d'en brancher 50.
5. ESN relevés à la réception, liste blanche en production.
6. 802.1X toujours en mode monitor avant bascule.
7. Certificats suivis à J-90/J-60/J-30 — responsable nommé.
8. Licence suivie trimestriellement — alerte à 80 %.
9. Sauvegarde quotidienne + test de restauration annuel (chronométré).
10. Supervision du contrôleur par Zabbix (qui supervise le superviseur).
11. Compte local de secours documenté (break-glass).
12. Un équipement = une plateforme (désenrôler eKit avant NCE).
13. Fenêtre de maintenance planifiée et communiquée pour tout changement.
14. Rollback prêt avant chaque déploiement — seuil de déclenchement écrit.
15. Après incident : REX sous 72 h, fiche enrichie.
16. Dashboard « tour de contrôle » consulté chaque matin.
17. Revue des rôles et comptes chaque trimestre.
18. Rapport mensuel d'une page pour la direction (avec commentaire humain).
19. Coexistence eSight assumée : chacun son périmètre, matrice RACI.
20. En cas de doute : maquette d'abord, production ensuite.

## 182. Annexe B — Correspondance des écrans (anglais → français usuel)

Le portail NCE étant en anglais, ce lexique aide l'équipe :

| Écran / terme NCE | Traduction usuelle |
|---|---|
| Dashboard | Tableau de bord |
| Topology / Digital map | Topologie / carte réseau |
| Device Management | Gestion des équipements |
| Onboarding / Registration | Enrôlement / enregistrement |
| Template / Profile | Modèle / profil de configuration |
| Site / Region / Tenant | Site / région / tenant (organisation) |
| Policy (access, QoS) | Politique (d'accès, de QoS) |
| WLAN / SSID / Radio profile | Wi-Fi / nom du réseau / profil radio |
| Authentication component | Composant d'authentification (branches) |
| Device Maintenance | Maintenance des équipements |
| File Management | Gestion des fichiers (firmwares, licences) |
| Upgrade / Downgrade | Mise à jour / retour de version |
| Signature Database Upgrade | Mise à jour des bases de signatures |
| Activate Device License | Activation des licences équipements |
| Alarm / Event | Alarme / événement |
| SLA Management | Gestion des SLA (sondes NQA) |
| Report | Rapport |
| System Management | Administration système |
| Third-Party Service | Services tiers (SMTP, update center...) |
| Audit log | Journal d'audit |
| Grace period | Période de grâce (licences) |

## 183. Annexe C — Modèle de procès-verbal de recette

```
PROCÈS-VERBAL DE RECETTE — iMaster NCE-Campus
Projet : ...................... Site/Jalon : ......................
Date : ...................... Participants : ......................

1. PÉRIMÈTRE RECETTÉ
   [ ] Installation / [ ] Pilote / [ ] Vague n°__ / [ ] Globale

2. CRITÈRES (cocher + preuve)
   [ ] Onboarding ZTP fonctionnel (pr. : ........................)
   [ ] Conformité templates 100 % (pr. : ........................)
   [ ] 802.1X validé (profils : .................................)
   [ ] SSID testés (liste : .....................................)
   [ ] Supervision nominale 72 h (pr. : .........................)
   [ ] Sauvegarde/restauration testée (RTO mesuré : .............)
   [ ] Documentation livrée (pr. : ..............................)

3. RÉSERVES
   N° | Description | Responsable | Délai de levée
   ...| ........... | ........... | ..............

4. DÉCISION : [ ] Recette prononcée / [ ] Prononcée avec réserves / [ ] Ajournée
Signatures : ...................... ......................
```

## 184. Annexe D — Checklists imprimables (rappel)

**Installation (détaillée section 38)** : serveurs OK, image vérifiée (checksum), IP/DNS/NTP, flux pare-feu testés, certificats prêts, fenêtre planifiée, rollback écrit, équipe formée.

**Onboarding d'un site (détaillée sections 56-60)** : site créé, ESN pré-déclarés, DHCP/option prêts, templates validés, firmware cible déposé, fiche technicien prête, tests utilisateurs, conformité 100 %, documentation à jour.

**Migration d'un site (détaillée section 115)** : périmètre défini, compatibilité validée, NCE prêt, templates testés, pilote choisi, fenêtre communiquée, rollback écrit, équipe formée, critères go/no-go écrits, REX prévu.

**Quotidien NOC** : dashboard tour de contrôle, alarmes critiques acquittées ou escaladées, équipements hors ligne > 15 min = ticket, conformité vérifiée, sauvegarde de la nuit OK.

**Mensuel** : rapport direction, revue des licences, revue des certificats (échéances), REX des incidents, mise à jour du dossier d'exploitation.

**Annuel** : test de restauration (chronométré), exercice de bascule HA, revue des rôles, audit de configuration, plan de formation, révision du TCO.

## 185. Annexe E — Bibliographie commentée

- **iMaster NCE-Campus Product Overview (V300R022C10)** — la référence d'architecture : composants (manager/controller/analyzer), authentification distribuée (20 composants), southbound (NETCONF/SNMP/HTTP/2/HTTPS/TCP), restrictions par génération d'équipements. À lire en premier.
- **iMaster NCE-Campus Monitoring and O&M (V300R020C10)** — l'exploitation au quotidien : politiques d'upgrade, signature database, activation des licences devices, SLA/NQA. Le manuel de l'admin.
- **MSP Training Manual — License Usage Guide (V300R022C00 / V300R024C00)** — le modèle device-day, la grâce de 30 jours, le mode MSP et la licence commune de 30 000 device-days. Indispensable avant tout chiffrage.
- **iMaster NCE-CampusInsight Datasheet (V100R025C00)** — les souscriptions d'analyse (package de base obligatoire + options). Pour chiffrer l'« IA ».
- **iMaster NCE-Campus Product Brochure / Datasheet (R24C00)** — vision produit et arguments (OPEX, Tolly +50 % Wi-Fi) — à lire avec recul commercial.
- **Huawei SD-WAN Solution (manuel)** — le rôle WAN de NCE (NCE-Campus comme futur contrôleur SD-WAN mainstream selon Huawei).
- **eSight 23.1 Brochure + S-Part list** — pour comparer les modèles de licence (88034GED/88034GEE/88034GEF...) et garder eSight à sa juste place.
- **Outil EOM/EOFS/EOS Query (e.huawei.com)** — la seule source fiable pour le statut de support d'eSight et des équipements.
- **Guides compagnons de Zelef** : AP361/AP761/S310/AR720/USG6000/eKit (les équipements pilotés), onduleurs/UPS (l'énergie — hors périmètre NCE, à superviser en transverse), eSight (la coexistence).

---

*Fin du guide — v1.0, septembre 2026. Document de travail : recouper chaque valeur dimensionnante avec la documentation officielle de la version NCE cible et le partenaire avant engagement.*
