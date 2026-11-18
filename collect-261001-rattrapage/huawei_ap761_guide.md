# Guide ultra-complet — Huawei eKit AP761

**Sous-titre :** point d'accès extérieur Wi-Fi 6 — installation, configuration, supervision, dépannage terrain, et tout ce qu'il faut savoir sur le Wi-Fi 7 pour préparer la suite.

**Public :** Zelef, chef de service systèmes & énergies — ton terrain, concret, direct.
**Date de rédaction :** 27 septembre 2026. **Langue :** français.

> ⚠️ **AVERTISSEMENT DE FONDATION — À LIRE EN PREMIER**
>
> La mission initiale décrivait l'AP761 comme un « point d'accès Wi-Fi 7 ». **Vérification faite auprès des fiches constructeur et des distributeurs (recherche web du 27/09/2026) : c'est FAUX.**
>
> - **Le Huawei eKit AP761 (réf. 02355VFB) est un point d'accès EXTÉRIEUR Wi-Fi 6 (802.11ax)**, bi-bande 2.4 GHz + 5 GHz, 2×2, 1.775 Gbps max. Pas de Wi-Fi 7, pas de MLO, pas de 320 MHz, pas de 6 GHz, pas de 4096-QAM sur ce modèle.
> - **Les modèles eKit Wi-Fi 7 d'extérieur sont l'AP771 (3.57 Gbps) et l'AP772E (6.45 Gbps)** ; en intérieur : AP371 (3.57 Gbps) et AP673 (tri-bande avec 6 GHz / 320 MHz).
>
> Ce guide est donc construit honnêtement en deux volets :
> 1. **L'AP761 tel qu'il est vraiment** (Wi-Fi 6 outdoor) : hardware, installation, configuration, dépannage — c'est le gros du document, et c'est du concret vérifié.
> 2. **Le Wi-Fi 7 expliqué simplement** (théorie solide + modèles eKit qui le supportent réellement) pour que tu saches exactement ce que l'AP761 ne fait PAS, et vers quoi migrer si tu veux du Wi-Fi 7 en extérieur.
>
> Chaque fois qu'une valeur vient du constructeur, c'est indiqué. Chaque fois qu'une valeur n'a pas pu être vérifiée, c'est marqué **« à vérifier sur la fiche du modèle exact »** — sans exception, sans invention.

## Sources vérifiées (recherche web du 27/09/2026)

| Source | Ce qu'elle confirme |
|---|---|
| Datasheet constructeur Huawei eKitEngine AP761 (hw-trade.ru, PDF officiel) | Wi-Fi 6 802.11ax ; 2×2 2.4 + 2×2 5 GHz ; 1.775 Gbps ; 1× GE RJ45 + 1× SFP GE combo ; PoE 802.3at/af ; 17.7 W max ; IP68 ; −40 °C à +65 °C ; antennes 10/11 dBi ; 1024 STA ; 16 SSID/radio |
| Fiche distributeur alltronstamm.ch (EAN 6901443451289, réf 02355VFB) | Dimensions 200×200×69 mm ; 1.91 kg ; montage mur/mât ; pas de Mesh ; PoE+ |
| Fiche distributeur 4-electronics.ch | AX1800 ; canaux 20/40/80 MHz ; WPA3-SAE/EAP, WAPI ; BLE 5.2 ; 802.11k/v/r |
| Fiche distributeur technerve.co.ke / microviewng.com | Combo SFP/GE (l'optique est prioritaire) ; 802.3af = fonctions restreintes ; antennes directionnelles 65°H |
| Manuel HUAWEI eKit Scenario-based Products (absolut-distribution.ch) | Comparatif AP761 vs AP361/AP362/AP661 : AP761 = outdoor 1.775 Gbps, 17.7 W ; AP361 = indoor 1.775 Gbps, 8.8 W |
| Fiche bimel.com.tr (AP Wi-Fi 7 eKit outdoor) | Wi-Fi 7 802.11be : 320 MHz **non supporté** sur ce produit outdoor ; 4096-QAM, multi-RU, MLO supportés |
| Fiche microtech.net.pk (AP772E) | Wi-Fi 7 outdoor : 6.45 Gbps, 1× 2.5GE + 1× 10GE SFP+, PoE af/at/bt ou DC, IP68, −40 à +70 °C, bi-bande 2.4/5 GHz |
| Comparatif shop.itegy.com.eg | AP761 = Wi-Fi 6, 1800 Mbps, 1024 users max, 500 m ; AP772 = Wi-Fi 7, 6.45 Gbps ; AP771 = Wi-Fi 7, 3.57 Gbps |

---

## Sommaire

1. Ce que ce guide couvre (et ne couvre pas)
2. Positionnement de l'AP761 dans la gamme eKit
3. Contenu de la boîte et prérequis chantier
4. Fiche technique constructeur — tableau de synthèse
5. Hardware : boîtier, dimensions, robustesse
6. Hardware : ports filaires (GE RJ45 + SFP, mode combo)
7. Hardware : alimentation PoE — 802.3at/af, 17.7 W
8. Hardware : LED d'état et bouton reset
9. Hardware : antennes intégrées directionnelles
10. Hardware : BLE 5.2 et USB IoT
11. Les 3 modes de fonctionnement : Fat, Fit, Cloud
12. Wi-Fi 6 (802.11ax) : ce que l'AP761 fait vraiment
13. Rétrocompatibilité Wi-Fi 5/4/anciens clients
14. Le Wi-Fi 7 (802.11be) expliqué simplement
15. MLO — Multi-Link Operation, en clair
16. Canaux 320 MHz : pourquoi l'outdoor eKit n'en a pas
17. 4096-QAM (4K-QAM) : +20 % de débit, à quel prix
18. Preamble puncturing et multi-RU
19. 6 GHz et réglementation : le point pour l'Europe
20. Ce que l'AP761 ne fait PAS (tableau Wi-Fi 6 vs Wi-Fi 7)
21. La gamme eKit Wi-Fi 7 réelle : AP771, AP772E, AP371, AP673
22. Installation physique : où poser un AP extérieur
23. Montage mural et sur mât — procédure pas à pas
24. Câblage : choix du câble, distance, étanchéité
25. PoE : budget, injecteurs, switches — calculs
26. Protection foudre et mise à la terre
27. Checklist pré-installation chantier
28. Première mise en route : les 3 chemins
29. Onboarding via l'app HUAWEI eKit (QR code)
30. Mode cloud : créer un site, adopter l'AP
31. Mode Fat : accès web local et CLI
32. Mode Fit : raccordement à un AC (WAC)
33. Changer de mode : fat ↔ fit ↔ cloud
34. Configuration initiale minimale (recette)
35. Sauvegarde de la configuration initiale
36. Configuration radio : concepts de base
37. Plan de canaux 2.4 GHz — méthode terrain
38. Plan de canaux 5 GHz — méthode terrain
39. Largeur de canal : 20/40/80 MHz, que choisir
40. Puissance d'émission : régler sans bourriner
41. DFS en 5 GHz : ce qu'il faut savoir
42. Band steering et préférence de bande
43. SSID : stratégie de nommage et nombre
44. Sécurité : panorama WPA/WPA2/WPA3 sur l'AP761
45. WPA2-PSK : configuration pas à pas
46. WPA3-SAE : configuration et limites
47. Mode transition WPA2/WPA3 : pour qui, pourquoi
48. 802.1X / WPA2-Enterprise avec RADIUS
49. Portail captif (Portal) pour invités
50. Filtrage MAC : utile ou placebo ?
51. 802.11w (PMF) : protection des trames de management
52. VLAN : segmentation par SSID
53. Isolation client (STA isolation) dans un VLAN
54. QoS / WMM : prioriser la voix et la vidéo
55. ACL et DHCP snooping / DAI / IPSG
56. Itinérance : 802.11k, 802.11v, 802.11r
57. Fast roaming en pratique sur l'AP761
58. Planification : combien d'AP pour quelle surface
59. Règles de pouce densité / débit par usage
60. Site survey simplifié sans outils pro — méthode terrain
61. Lecture d'un relevé : RSSI, SNR, bruit
62. Erreurs classiques de placement d'AP extérieur
63. WIDS/wIPS : détection d'intrusion sans fil
64. Rogue AP : détecter, classifier, contenir
65. Analyse de spectre (mode Fit uniquement)
66. Supervision via la plateforme cloud eKit
67. Indicateurs Wi-Fi à surveiller (tableau)
68. SNMP : v1/v2c/v3 en mode Fat
69. Syslog : centraliser les journaux
70. NTP : pourquoi c'est critique
71. Alertes et seuils : que configurer
72. Sauvegarde de configuration : méthodes
73. Restauration de configuration
74. Firmware : cycle de vie et bonnes pratiques
75. Mise à jour firmware — procédure pas à pas
76. Rollback : revenir en arrière proprement
77. Dépannage : méthode générale (les 5 couches)
78. Cas 1 : l'AP ne s'allume pas / pas de PoE
79. Cas 2 : l'AP démarre mais n'apparaît pas dans le cloud
80. Cas 3 : SSID visible mais pas d'IP (DHCP)
81. Cas 4 : débit très en-deçà des attentes
82. Cas 5 : clients 5 GHz qui restent accrochés au 2.4 GHz
83. Cas 6 : coupures lors des déplacements (roaming)
84. Cas 7 : interférences 2.4 GHz (micro-ondes, Bluetooth, caméras)
85. Cas 8 : radar DFS — l'AP change de canal tout seul
86. Cas 9 : PoE insuffisant — fonctions restreintes en 802.3af
87. Cas 10 : SFP branché mais pas de lien
88. Cas 11 : WPA3 activé, certains clients ne se connectent plus
89. Cas 12 : portail captif qui ne s'affiche pas
90. Cas 13 : un client consomme tout l'airtime (slow client)
91. Cas 14 : l'AP surchauffe / redémarre en plein soleil
92. Cas 15 : rogue AP détecté — que faire concrètement
93. Cas 16 : après mise à jour firmware, comportements bizarres
94. Cas 17 : itinérance entre AP761 et AP indoor
95. Cas 18 : je veux du Wi-Fi 7 — que faire avec mes AP761 ?
96. Comparatif AP761 vs AP361 — tableau décisionnel
97. Quand choisir l'AP361 plutôt que l'AP761
98. Quand choisir l'AP761 plutôt que l'AP361
99. Et le Wi-Fi 7 outdoor ? AP771 vs AP772E
100. Tableau décisionnel global : quel AP eKit pour quel besoin
101. Maintenance préventive : planning annuel
102. Contrôle visuel et nettoyage
103. Contrôle électrique et PoE
104. Contrôle radio (audit annuel)
105. Gestion du cycle de vie et fin de vie
106. Durcissement : checklist sécurité
107. Durcissement : management (SSH, SNMP, web)
108. Durcissement : Wi-Fi (chiffrement, PMF, WIDS)
109. Pense-bête de poche — page 1 : specs
110. Pense-bête de poche — page 2 : CLI express
111. Pense-bête de poche — page 3 : dépannage express
112. Glossaire (A–Z)
113. Quiz : 10 questions
114. Quiz : réponses commentées
115. Cas pratique 1 : cour d'école / cour d'entreprise
116. Cas pratique 2 : parking et contrôle d'accès
117. Cas pratique 3 : entrepôt / zone logistique
118. Cas pratique 4 : terrasse d'hôtel / camping
119. Cas pratique 5 : relais entre deux bâtiments (point à point)
120. Pour aller plus loin : docs, outils, formations

---

## 1. Ce que ce guide couvre (et ne couvre pas)

**Ce guide couvre :**
- Le hardware réel de l'AP761, vérifié sur datasheet constructeur (sept. 2026).
- L'installation physique d'un AP extérieur (montage, PoE, foudre) — ton métier.
- La configuration via app eKit / cloud / web / CLI VRP.
- La radio Wi-Fi 6 : canaux, puissance, sécurité, VLAN, QoS, roaming.
- La supervision, la maintenance, le durcissement.
- 18 cas de dépannage terrain commentés.
- Un volet Wi-Fi 7 honnête : théorie + vrais modèles eKit concernés.

**Ce guide ne couvre PAS :**
- Le Wi-Fi 7 *sur* l'AP761 — parce qu'il n'en fait pas. Le chapitre 18 explique la migration.
- Les détails d'AirEngine (gamme entreprise) : l'AP761 est eKit (PME), la logique est simplifiée.
- Les procédures internes à ton entreprise (consignation électrique, habilitations) : elles s'appliquent en plus, pas à la place.

**Convention d'écriture :**
- `[valeur constructeur]` = valeur tirée du datasheet Huawei eKitEngine AP761.
- `« à vérifier sur la fiche du modèle exact »` = valeur non vérifiée au 27/09/2026. Ne jamais la prendre pour argent comptant sur un appel d'offres.
- Les commandes CLI sont **syntaxiquement plausibles pour VRP Huawei** (famille des AP eKit/CloudEngine). Le nom exact des vues et commandes peut varier selon la version logicielle : toujours valider avec `?` en CLI avant d'appliquer en production.

## 2. Positionnement de l'AP761 dans la gamme eKit

Huawei eKit = la gamme PME de Huawei (ex-« eKitEngine »), sans licence, gérée par app mobile ou cloud gratuit. Dans cette gamme :

| Modèle | Usage | Wi-Fi | Débit max | Ports | Conso max |
|---|---|---|---|---|---|
| AP160 / AP162E | Chambre d'hôtel, mur | Wi-Fi 6 | 1.775 / 2.975 Gbps | GE | faible |
| AP263 | Bureau PME | Wi-Fi 6 | 2.975 Gbps | 2× GE | 12 W |
| **AP361** | **Bureau PME (indoor)** | **Wi-Fi 6** | **1.775 Gbps** | 1× GE | **8.8 W** [constructeur] |
| AP362 / AP362E | Bureau dense (indoor) | Wi-Fi 6 | 2.975 Gbps | 1× GE / 1× 2.5GE | 11.2 W |
| AP661 | Indoor haute densité | Wi-Fi 6 | 6.575 Gbps | 1× 2.5GE + 1× GE | 21.2 W |
| **AP761** | **Extérieur (outdoor)** | **Wi-Fi 6** | **1.775 Gbps** | **1× GE + 1× SFP** | **17.7 W** [constructeur] |
| AP371 | Indoor | **Wi-Fi 7** | 3.57 Gbps | 1× 2.5GE | à vérifier sur la fiche du modèle exact |
| AP771 | Extérieur | **Wi-Fi 7** | 3.57 Gbps | à vérifier sur la fiche du modèle exact | à vérifier sur la fiche du modèle exact |
| AP772E | Extérieur | **Wi-Fi 7** | 6.45 Gbps | 1× 2.5GE + 1× 10GE SFP+ | à vérifier sur la fiche du modèle exact |
| AP673 | Indoor tri-bande | **Wi-Fi 7** | à vérifier sur la fiche du modèle exact | à vérifier sur la fiche du modèle exact | à vérifier sur la fiche du modèle exact |

**Lecture terrain :** l'AP761 est *le* point d'accès extérieur Wi-Fi 6 de la gamme PME. Si ton besoin est « couvrir une cour, un parking, une terrasse en Wi-Fi 6 fiable », c'est lui. Si ton besoin est « Wi-Fi 7 en extérieur », ce n'est pas lui : voir chapitres 21 et 99.

## 3. Contenu de la boîte et prérequis chantier

**Dans la boîte (kit standard, à vérifier sur la fiche du modèle exact pour ta région) :**
- L'AP761 lui-même (boîtier métal blanc, 200 × 200 × 69 mm, 1.91 kg) [constructeur].
- Kit de montage mural/mât (étriers, visserie de base).
- Cache presse-étoupe pour le passage du câble Ethernet.
- Guide de démarrage rapide + carte avec QR code d'onboarding cloud.

**Ce qui n'est généralement PAS fourni :**
- L'injecteur PoE ou le switch PoE (à prévoir, chap. 25).
- Le câble Ethernet extérieur (à prévoir, chap. 24).
- Le mât ou le bras de déport (à prévoir selon le site).
- Le parafoudre Ethernet / la barrette de terre (à prévoir, chap. 26).

**Prérequis chantier avant de monter :**
- [ ] Point de fixation identifié (mur porteur ou mât), dégagé, accessible à la nacelle/échelle en sécurité.
- [ ] Chemin de câble défini jusqu'au local technique (distance, traversées).
- [ ] Source PoE identifiée : switch PoE 802.3at minimum recommandé (voir chap. 25).
- [ ] Plan d'adressage : VLAN, sous-réseau de management, passerelle, DNS, NTP.
- [ ] Compte HUAWEI eKit / accès à la plateforme cloud si mode cloud (chap. 30).
- [ ] Smartphone avec l'app HUAWEI eKit installée (Android/iOS) pour l'onboarding.
- [ ] Autorisations : copropriété, mairie (façade classée ?), consignation électrique si intervention sur TGBT.

## 4. Fiche technique constructeur — tableau de synthèse

Toutes les valeurs ci-dessous viennent du **datasheet officiel Huawei eKitEngine AP761** (vérifié le 27/09/2026). Les valeurs non vérifiées sont marquées explicitement.

| Rubrique | Valeur constructeur |
|---|---|
| Référence | 02355VFB |
| EAN | 6901443451289 |
| Standard Wi-Fi | IEEE 802.11a/b/g/n/ac/ac Wave 2/**ax (Wi-Fi 6)** |
| Bandes / MIMO | 2.4 GHz 2×2 + 5 GHz 2×2, simultanées |
| Débit max agrégé | **1.775 Gbps** (0.575 Gbps @ 2.4 GHz + 1.2 Gbps @ 5 GHz) |
| Largeurs de canal | 20 / 40 / 80 MHz |
| Modulation max | 1024-QAM (compatible 256/64/16-QAM, QPSK, BPSK) |
| Ports filaires | **1× GE RJ45 (PoE-In)** + **1× SFP GE optique** — combo : un seul actif à la fois, **l'optique est prioritaire** |
| Alimentation | PoE **IEEE 802.3at/af** — en 802.3af, **fonctions restreintes** (voir chap. 9 du dépannage, cas 9) |
| Consommation max | **17.7 W** |
| Dimensions (H×L×P) | **69 × 200 × 200 mm** |
| Poids | **1.91 kg** |
| Antennes | Intégrées **directionnelles** : 2.4 GHz **10 dBi** (65° H / 40° V), 5 GHz **11 dBi** (65° H / 20° V), BLE 5 dBi |
| Puissance TX max combinée | 2.4 GHz : **28 dBm** ; 5 GHz : **27 dBm** (sous réserve réglementation locale) |
| Pas de réglage puissance | 1 dBm |
| Clients max | **1024** (512 par radio) — le réel dépend de l'environnement |
| SSID max | **16 par radio** |
| Bluetooth | **BLE 5.2** (O&M via app CloudCampus/eKit) |
| LED | États : power-on, démarrage, fonctionnement, alarme, défaut |
| Température de fonctionnement | **−40 °C à +65 °C** (dératage : −1 °C par 300 m au-delà de 1800 m) |
| Température de stockage | −40 °C à +85 °C |
| Humidité | 0 % à 100 % HR |
| Indice de protection | **IP68** |
| Altitude | −60 m à +5000 m ; pression 53 à 106 kPa |
| Surtension ports Ethernet | **6 kV** |
| Modes | **Fit, Fat, cloud management** |
| Sécurité Wi-Fi | WEP, WPA/WPA2-PSK, WPA/WPA2-Enterprise, **WPA3-SAE**, WPA3-Enterprise, WAPI, 802.11w |
| Fonctions radio | MU-MIMO, OFDMA, BSS coloring, TWT, beamforming, MRC, STBC, CDD/CSD, LDPC, A-MPDU/A-MSDU, DFS, SST, U-APSD, ACC, CAC |
| Réseau | 802.1Q (VLAN par SSID), DHCP client, NAT, ACL, DHCP snooping, DAI, IPSG, mDNS, IPv6 SAVI |
| Management | Web (HTTP/HTTPS), Telnet/STelnet (SSHv2), SFTP, SNMP v1/v2/v3 (mode Fat), NTP, CAPWAP, PnP |
| Montage | Mural / sur mât (kit fourni) |

> 📌 **Points à vérifier sur la fiche du modèle exact** (informations vues chez des revendeurs mais non confirmées par le datasheet consulté) : présence du **port USB** (extension IoT ZigBee/RFID), portée annoncée de **500 m**, puissance TX « ajustée » à 23 dBm par radio chez certains revendeurs, absence de **Mesh**.

## 5. Hardware : boîtier, dimensions, robustesse

L'AP761 est un pavé métallique blanc de **200 × 200 mm** pour **69 mm** d'épaisseur, **1.91 kg** [constructeur]. Ce n'est pas un AP de bureau : c'est un boîtier métal IP68 pensé pour rester dehors des années.

**Ce que ça change sur le terrain :**
- **1.91 kg + prise au vent** : sur mât, prévoir un collier sérieux et un mât qui ne fléchit pas. Un AP qui oscille au vent = des variations de RSSI côté clients fixes.
- **Boîtier métal = dissipateur** : la chaleur s'évacue par le boîtier. Ne jamais l'enfermer dans un coffret étanche non ventilé « pour le protéger » — tu le cuisinerais (voir cas 14).
- **IP68** : immersion temporaire supportée sur le papier. En pratique : ce qui tue les AP extérieurs, ce n'est pas la pluie directe, c'est l'eau qui **stagne** et remonte par capillarité dans un presse-étoupe mal serré, ou la condensation interne sur des cycles chaud/froid. Règle d'or : **presse-étoupe vers le bas**, boucle d'égouttage sur le câble (goutte d'eau), jamais de gaine qui fait entonnoir vers l'AP.
- **Plage −40 °C à +65 °C** [constructeur] : tient le plein soleil méditerranéen comme l'hiver. Au-delà de 1800 m d'altitude, dératage de 1 °C par 300 m — à noter pour les sites de montagne.
- **6 kV de protection surtension sur les ports Ethernet** [constructeur] : c'est une protection interne, pas un parafoudre. Elle encaisse les transitoires, pas la foudre directe. Chapitre 26 pour la vraie protection.

**Checklist réception matériel :**
- [ ] Boîtier sans choc ni déformation (un coin enfoncé = joint IP68 compromis).
- [ ] Joints et presse-étoupe présents et souples.
- [ ] Kit de montage complet (étriers, vis, colliers).
- [ ] Étiquette lisible : modèle AP761, numéro de série, adresse MAC, QR code d'onboarding.
- [ ] Photo de l'étiquette avant montage — quand l'AP est à 8 m de haut, tu seras content de l'avoir.

## 6. Hardware : ports filaires (GE RJ45 + SFP, mode combo)

L'AP761 a **deux ports filaires** [constructeur] :

| Port | Type | Rôle |
|---|---|---|
| GE / PoE_IN | RJ45 10/100/1000 Mbps | Uplink cuivre + **alimentation PoE** |
| SFP | Cage SFP 1 Gbps optique | Uplink fibre optique |

**Le point critique : ce sont des ports COMBO.** Un seul des deux est actif à la fois. **Si les deux sont branchés, le port optique (SFP) est prioritaire** [constructeur]. Conséquences terrain :

1. Tu ne peux pas faire « cuivre = uplink, fibre = downlink » : c'est l'un OU l'autre.
2. Si tu branches la fibre pour un test sans débrancher le cuivre, l'AP bascule sur la fibre. Si le SFP est mal enfiché, tu perds l'AP au lieu de tester.
3. **L'alimentation arrive TOUJOURS par le RJ45** (PoE-In). La fibre ne transporte pas d'énergie : un AP uplinké en fibre a quand même besoin du cuivre pour le PoE (ou d'une alimentation locale — à vérifier sur la fiche du modèle exact si un adaptateur secteur est supporté).

**Scénarios de câblage :**

| Scénario | Câblage | Remarque |
|---|---|---|
| Classique PME | RJ45 seul vers switch PoE | Le plus courant. 100 m max en cuivre. |
| Distance > 100 m | Fibre via SFP + RJ45 vers injecteur PoE local | Le SFP prend l'uplink, le RJ45 ne sert qu'au PoE. |
| Liaison inter-bâtiments | Fibre via SFP + PoE local | Idéal avec parafoudre des deux côtés. |

**Négociation :** auto-négociation du débit et du duplex, MDI/MDI-X automatique [constructeur] — un câble droit suffit dans tous les cas.

> ⚠️ **Piège classique :** brancher le SFP « au cas où » pendant que l'uplink cuivre fonctionne. L'AP bascule sur l'optique, et si le module SFP n'est pas compatible ou la fibre pas encore brassée, l'AP disparaît du réseau. **Ne branche le SFP que quand la fibre est prête.**

## 7. Hardware : alimentation PoE — 802.3at/af, 17.7 W

**Valeurs constructeur :** PoE **IEEE 802.3at/af**, consommation max **17.7 W**. Point capital du datasheet : **en mode 802.3af, l'AP fonctionne avec des fonctions restreintes** [constructeur].

**Traduction terrain :**
- **802.3af** = 15.4 W au port du switch (12.95 W utiles à l'AP après pertes câble). C'est **en dessous** des 17.7 W max de l'AP. Résultat : l'AP démarre mais bride des fonctions (typiquement : puissance radio réduite, USB/BLE désactivés, un débit plafonné — le détail exact des restrictions est **à vérifier sur la fiche du modèle exact**).
- **802.3at (PoE+)** = 30 W au port (25.5 W utiles). C'est le **minimum recommandé** : l'AP tourne à plein régime.
- **802.3bt** : non requis par l'AP761 (pas de 2.5G, pas de radios Wi-Fi 7 gourmandes). Un switch 802.3bt alimente très bien l'AP761 en négociant en 802.3at — c'est rétrocompatible.

**Règle d'or du budget PoE :**
> Budget par port = 17.7 W (AP) + pertes câble (~2.5 W pour 100 m en PoE+) + marge 20 % ≈ **25 W réservés par AP761** sur le budget du switch.

**Exemple :** un switch 8 ports PoE+ avec budget total 130 W → 130 / 25 ≈ **5 AP761 max** en sécurité, pas 8. Le nombre de ports PoE ne fait pas le budget : c'est le **budget total en watts** qui compte. Toujours lire la ligne « PoE budget » de la fiche du switch.

**Injecteur PoE :** si le switch n'est pas PoE, un injecteur **802.3at 30 W** fait l'affaire. Vérifier qu'il est **gigabit** (certains injecteurs bas de gamme ne passent que du 100 Mbps et tu brides l'AP sans le savoir).

**Câble et PoE :** le PoE chauffe le câble. En extérieur au soleil + PoE, prends du **câble extérieur blindé (F/UTP ou S/FTP)**, âme **100 % cuivre** (pas de CCA — cuivre plaqué aluminium — qui chauffe et casse). Section 24 AWG minimum.

## 8. Hardware : LED d'état et bouton reset

**LED** [constructeur] : une LED unique (ou un ensemble selon la version) indique les états **power-on, démarrage, fonctionnement, alarme, défaut**. En pratique sur les AP Huawei eKit :

| État LED | Signification probable |
|---|---|
| Éteinte | Pas d'alimentation — vérifier PoE/injecteur |
| Rouge fixe | Démarrage / bootloader |
| Vert clignotant | Démarrage en cours, chargement du firmware |
| Vert fixe | Fonctionnement normal |
| Orange/rouge clignotant | Alarme ou défaut (perte cloud, erreur radio…) |

> Les codes exacts variant selon la version logicielle : **à vérifier sur la fiche du modèle exact** (guide de démarrage rapide fourni dans la boîte). En dépannage, la LED est ton premier indicateur à distance quand quelqu'un est sur place au téléphone.

**Bouton reset** (revendeurs) : appui long (≈ 10 s, à vérifier sur la fiche du modèle exact) = retour aux paramètres d'usine. **Attention :** sur un AP en mode cloud déjà adopté, un reset d'usine le fait perdre du site — il faudra le ré-adopter (chap. 29). Ne reset jamais sans avoir la configuration sauvegardée (chap. 72).

**Port de verrouillage** (revendeurs) : emplacement pour câble antivol type Kensington (câble vendu séparément). Utile sur les AP accessibles au public (terrasses, campings).

## 9. Hardware : antennes intégrées directionnelles

**Valeurs constructeur :** antennes intégrées **directionnelles** —
- 2.4 GHz : **10 dBi**, ouverture **65° horizontale / 40° verticale**
- 5 GHz : **11 dBi**, ouverture **65° horizontale / 20° verticale**
- BLE : 5 dBi

**Ce que « directionnel 65° » veut dire sur le terrain :**
- L'AP761 n'arrose pas à 360°. Il couvre un **secteur d'environ 65°** devant lui — comme un projecteur, pas comme une ampoule.
- C'est un choix assumé : en extérieur, on veut couvrir une **zone ciblée** (une cour, une terrasse, un parking) en portant loin, pas arroser les voisins et ramasser leurs interférences.
- **Conséquence n° 1 : l'orientation compte.** L'AP doit être pointé vers la zone à couvrir, avec un léger **tilt vers le bas** (downtilt) si la zone est en contrebas. Un AP directionnel monté à l'envers ou de travers = une zone morte là où tu voulais couvrir.
- **Conséquence n° 2 : l'arrière est sourd.** Ne compte pas sur l'AP761 pour couvrir derrière lui (le bâtiment sur lequel il est fixé, par exemple). Prévoir un AP indoor séparé pour l'intérieur.
- **Conséquence n° 3 : le gain élevé (10–11 dBi) concentre l'énergie.** À puissance radio égale, ça porte plus loin dans l'axe, mais la zone couverte est plus étroite. Pour une couverture large (place publique), il faut soit plusieurs AP761 en secteurs, soit un modèle omnidirectionnel — **à vérifier sur la fiche du modèle exact** s'il existe une variante.

**Hauteur de montage :** 4 à 8 m typique pour une cour/un parking. Plus haut = plus de portée mais zone morte juste en dessous (lobe vertical étroit, surtout en 5 GHz : 20°). Règle de pouce : **ne pas monter plus haut que nécessaire** — chaque mètre de hauteur en trop creuse la zone d'ombre au pied du mât.

## 10. Hardware : BLE 5.2 et USB IoT

**BLE 5.2** [constructeur] : le Bluetooth Low Energy intégré sert à l'**O&M via l'app mobile** (onboarding, diagnostic de proximité) sans avoir besoin d'être sur le réseau. En pratique :
- L'app HUAWEI eKit détecte l'AP en BLE pour l'adoption initiale (chap. 29).
- Ça dépanne quand l'AP n'a pas encore d'IP ou que le réseau est en vrac : tu te connectes en BLE à côté de l'AP et tu lis son état.

**USB** (source revendeur, **à vérifier sur la fiche du modèle exact**) : un port USB permettrait l'extension IoT (dongles ZigBee, RFID). Si ton usage est purement Wi-Fi, ignore-le. Si un jour tu veux de la localisation d'actifs ou des capteurs, c'est une piste — mais vérifie d'abord la liste des dongles supportés par la version logicielle.

**Point d'attention PoE :** en alimentation 802.3af (fonctions restreintes), l'USB et le BLE sont typiquement les premières fonctions coupées. Encore une raison de rester en 802.3at.

## 11. Les 3 modes de fonctionnement : Fat, Fit, Cloud

L'AP761 supporte trois modes [constructeur]. Le choix du mode conditionne TOUTE l'administration : choisis-le avant de configurer.

| | **Fat (autonome)** | **Fit (contrôleur)** | **Cloud (eKit)** |
|---|---|---|---|
| Cerveau | L'AP lui-même | Un AC (contrôleur, ex. WAC eKit) | La plateforme cloud Huawei eKit |
| Config | Web/CLI sur chaque AP | Centralisée sur l'AC via CAPWAP | Centralisée sur le cloud via app/web |
| Idéal pour | 1–3 AP, site isolé | Parc homogène, roaming avancé, analyse de spectre | Multi-sites PME, gestion à distance, zéro licence |
| Analyse de spectre | Non | **Oui** (seul mode qui la supporte) | Non |
| SNMP | v1/v2c/v3 | Via l'AC | Limité (voir chap. 68) |
| Prérequis | Rien | Un AC compatible sur le réseau | Compte cloud + Internet |

**Recommandation terrain :**
- **1 AP isolé (terrasse, parking) → Fat ou Cloud.** Le cloud apporte la supervision à distance gratuite : si tu gères plusieurs sites, c'est imbattable.
- **Plusieurs AP avec roaming soigné → Cloud** (suffit en PME) ou **Fit** si tu as déjà un AC.
- **Site sans Internet fiable → Fat.** Le cloud a besoin d'Internet pour la gestion (le Wi-Fi local continue de fonctionner si le cloud tombe, mais tu ne peux plus administrer à distance).

**Changement de mode (principe) :**
```
# En CLI (mode Fat), basculer l'AP vers le mode cloud :
<AP761> system-view
[AP761] ap-mode cloud
# L'AP redémarre et cherche la plateforme cloud (QR code / app).
# Retour en Fat :
[AP761] ap-mode fat
```
> La syntaxe exacte de bascule (`ap-mode`) et les prérequis (firmware) sont **à vérifier sur la fiche du modèle exact** et la version logicielle. Ne jamais basculer un AP en production sans fenêtre de maintenance : ça redémarre et ça réinitialise la configuration locale.

## 12. Wi-Fi 6 (802.11ax) : ce que l'AP761 fait vraiment

Le Wi-Fi 6 n'est pas « le Wi-Fi plus vite » : c'est « le Wi-Fi **plus efficace quand il y a du monde** ». L'AP761 l'implémente en 2×2 sur deux bandes [constructeur]. Les briques qui comptent sur le terrain :

**OFDMA — le découpage de canal :** en Wi-Fi 5, un client monopolisait tout le canal même pour envoyer trois octets. En Wi-Fi 6, le canal est découpé en sous-porteuses (Resource Units) et **plusieurs clients émettent/reçoivent en même temps**. Résultat : latence divisée et bien meilleure tenue en charge quand 50 smartphones se battent pour le canal. C'est LA raison d'être du Wi-Fi 6 dans une cour d'école ou une terrasse bondée.

**MU-MIMO montant et descendant :** l'AP761 parle à plusieurs clients **simultanément** grâce à ses 2 flux spatiaux (2×2), en émission ET en réception [constructeur]. Limite physique : 2 flux = 2 clients en MU-MIMO plein débit en même temps ; au-delà, c'est l'OFDMA qui prend le relais.

**1024-QAM :** chaque symbole transporte 10 bits au lieu de 8 (256-QAM en Wi-Fi 5) → **+25 % de débit théorique** à signal égal [constructeur]. Mais ça exige un signal très propre (SNR élevé). En extérieur avec de la distance, les clients retomberont vite sur 256-QAM ou moins : le 1024-QAM ne sert qu'à courte distance.

**BSS Coloring :** l'AP761 « colore » son réseau [constructeur]. Quand il entend une trame d'un réseau voisin de couleur différente, il peut émettre quand même au lieu d'attendre — **moins d'attente, plus de débit utile** en zone dense (lotissement, zone commerciale). C'est invisible à configurer, mais c'est ce qui fait la différence entre un Wi-Fi 6 et un Wi-Fi 5 dans un environnement chargé.

**TWT (Target Wake Time) :** l'AP négocie avec les objets connectés leurs horaires de réveil [constructeur]. Pour des capteurs sur batterie en extérieur (station météo, compteur), c'est de l'autonomie en plus. Pour des smartphones, gain marginal.

**Débits réels à attendre (ordres de grandeur terrain) :**
| Client | Condition | Débit utile réaliste |
|---|---|---|
| Wi-Fi 6, 2×2, 80 MHz, proche (< 15 m, vue directe) | 1024-QAM | 400–600 Mbps |
| Wi-Fi 6, 2×2, 80 MHz, 30–50 m | 256-QAM | 150–300 Mbps |
| Wi-Fi 5, 2×2, 80 MHz | 256-QAM | 150–350 Mbps |
| Wi-Fi 4 / vieux smartphone, 20 MHz | 64-QAM | 20–70 Mbps |

Le 1.775 Gbps constructeur est un **débit PHY agrégé théorique** (les deux radios à fond en même temps). En pratique, un client seul ne verra jamais ce chiffre : le débit utile, c'est 50–70 % du débit PHY dans le meilleur cas.

## 13. Rétrocompatibilité Wi-Fi 5/4 et vieux clients

L'AP761 parle **802.11a/b/g/n/ac/ac Wave 2/ax** [constructeur] : un client de 2012 se connecte sans problème. Mais la rétrocompatibilité a un coût, et il faut le connaître :

**Ce qui change pour les vieux clients :**
- Un client Wi-Fi 4 (802.11n) ne comprend ni l'OFDMA ni le MU-MIMO : il est servi en mode « classique », un par un. Il ne casse rien, il ne profite juste pas des optimisations.
- Les débits 802.11b (1, 2, 5.5, 11 Mbps) : si tu les laisses activés, les vieux clients peuvent s'accrocher de loin avec un débit minable et **monopoliser l'airtime**. En extérieur, c'est le piège n° 1 (voir cas 13).
- Le 2.4 GHz reste la bande « refuge » des objets connectés (caméras, portiers, capteurs) : la plupart sont en Wi-Fi 4, parfois même en 802.11g.

**Modes mixtes — réglages recommandés :**
| Paramètre | Réglage conseillé | Pourquoi |
|---|---|---|
| Débits de base 2.4 GHz | Désactiver 1, 2, 5.5, 11 Mbps (garder 6, 12, 24 Mbps min) | Force les clients à ne s'associer que s'ils ont un signal correct ; libère l'airtime |
| Débits de base 5 GHz | Garder 6, 12, 24 Mbps | Le 5 GHz n'a jamais eu de 802.11b, moins critique |
| 802.11ax activé | Oui, toujours | Rétrocompatible par nature |
| WPA3-only | **Non** si tu as des vieux clients | Ils ne connaissent pas SAE (voir chap. 47) |

**Règle terrain :** en extérieur, un client accroché à −85 dBm en 802.11b à 1 Mbps peut à lui seul diviser le débit utile de la cellule par deux. **Mieux vaut un client déconnecté (qui basculera sur la 4G) qu'un client zombie qui pourrit l'airtime de tout le monde.** C'est violent à dire, mais c'est la physique.

## 14. Le Wi-Fi 7 (802.11be) expliqué simplement

> 📌 Ce chapitre est de la **théorie** (standard IEEE 802.11be, alias EHT — Extremely High Throughput, certifié « Wi-Fi CERTIFIED 7 » par la Wi-Fi Alliance en janvier 2024). **L'AP761 ne supporte AUCUNE de ces fonctions.** C'est ici pour que tu saches ce qui existe, et le chapitre 21 dit quels modèles eKit les supportent vraiment.

Le Wi-Fi 6 a optimisé **l'efficacité** (plus de clients bien servis). Le Wi-Fi 7 attaque trois autres fronts : **le débit crête**, **la latence**, et **la fiabilité**. Les quatre briques :

1. **MLO (Multi-Link Operation)** — utiliser plusieurs bandes en même temps (chap. 15).
2. **Canaux 320 MHz** — des canaux deux fois plus larges qu'en Wi-Fi 6 (chap. 16).
3. **4096-QAM (4K-QAM)** — 12 bits par symbole au lieu de 10 (chap. 17).
4. **Preamble puncturing / multi-RU** — utiliser les morceaux de canal non brouillés (chap. 18).

**Ordre de grandeur :** le débit PHY max du standard monte à **46 Gbps** (16 flux, 320 MHz, 4096-QAM). En pratique sur un AP eKit Wi-Fi 7 réel (AP772E) : **6.45 Gbps agrégés**. Et comme toujours : le débit utile sera 50–70 % du PHY, partagé entre les clients.

**Ce que le Wi-Fi 7 change VRAIMENT pour un chef de service :**
- **Latence** : le MLO fait chuter la latence et la gigue — intéressant pour la voix sur Wi-Fi, la visio, et plus tard la réalité augmentée terrain.
- **Densité** : le multi-RU rend l'OFDMA encore plus efficace en environnement très chargé.
- **Backbone** : un AP Wi-Fi 7 a besoin d'un uplink **2.5G minimum** (l'AP772E a du 2.5GE + 10GE SFP+) et de **PoE++ (802.3bt)** : le budget électrique et câblage change d'échelle (chap. 25).

## 15. MLO — Multi-Link Operation, en clair

**Le concept :** jusqu'ici, un client Wi-Fi n'était connecté que sur **UNE** bande à la fois (2.4 OU 5 GHz). Avec le MLO, un client Wi-Fi 7 peut être connecté **simultanément sur deux (ou trois) bandes** — par exemple 5 GHz + 6 GHz — et l'AP répartit les trames sur les deux liens en temps réel.

**Les deux modes de MLO :**
| Mode | Principe | Usage |
|---|---|---|
| **STR** (Simultaneous Transmit and Receive) | Les deux liens émettent/reçoivent en même temps | Débit max : agrégation pure. Exige une bonne isolation entre radios (matériel haut de gamme). |
| **EMLSR** (Enhanced Multi-Link Single Radio) | Un seul lien actif à la fois, mais bascule instantanée vers le lien libre | Latence min : si le 5 GHz est occupé, ça part sur le 6 GHz sans attendre. Moins gourmand en matériel. |

**Analogie terrain :** avant, tu avais **une voie** entre l'AP et le client (avec des bouchons). Le MLO, c'est **deux voies parallèles** avec un aiguillage intelligent : si une voie est bouchée (interférence, radar DFS), le trafic passe par l'autre **sans couper la connexion**.

**Limites honnêtes :**
- Il faut un **client Wi-Fi 7** aussi : un smartphone Wi-Fi 6 ne fera jamais de MLO, même face à un AP Wi-Fi 7.
- Les premiers clients Wi-Fi 7 gèrent mieux l'EMLSR que le STR : les gains en débit agrégé réel sont variables.
- En extérieur dans l'UE, sans 6 GHz (chap. 19), le MLO se fait entre 2.4 et 5 GHz : utile pour la latence, modeste pour le débit.

> **Sur l'AP761 :** pas de MLO. Point. Si un commercial te parle de MLO sur un AP761, c'est une erreur ou une tromperie.

## 16. Canaux 320 MHz : pourquoi l'outdoor eKit n'en a pas

**La théorie :** le Wi-Fi 7 double la largeur max de canal : **320 MHz** (contre 160 MHz en Wi-Fi 6). Un canal deux fois plus large = deux fois plus de débit PHY à modulation égale.

**La réalité physique :**
- Les canaux 320 MHz n'existent en pratique que dans la **bande 6 GHz** (il faut 320 MHz de spectre contigu — introuvable en 2.4 GHz, très rare en 5 GHz).
- En 5 GHz, le max réaliste reste **160 MHz** (et encore : avec le DFS, trouver 160 MHz propres en extérieur relève du défi — chap. 41).

**Le fait vérifié qui compte :** la fiche d'un AP eKit **outdoor Wi-Fi 7** (bimel.com.tr, vérifiée le 27/09/2026) précise noir sur blanc : **« 320 MHz bandwidth (not supported by this product) »**. Les AP outdoor Wi-Fi 7 eKit (AP771/AP772E) sont **bi-bande 2.4/5 GHz** : pas de 6 GHz, donc pas de 320 MHz. Leurs gains Wi-Fi 7 viennent du **MLO**, du **4096-QAM** et du **multi-RU**, pas des canaux géants.

**Seul l'AP673 (indoor tri-bande)** annonce du 320 MHz sur la bande 6 GHz — **à vérifier sur la fiche du modèle exact**.

**Implication pour toi :** ne dimensionne jamais un projet outdoor sur du 320 MHz. En extérieur UE, le plan de canaux réaliste reste : **20 MHz en 2.4 GHz, 40 ou 80 MHz en 5 GHz** (chap. 37–39).

## 17. 4096-QAM (4K-QAM) : +20 % de débit, à quel prix

**La théorie :** le 4096-QAM encode **12 bits par symbole** contre 10 en 1024-QAM → **+20 % de débit théorique** à largeur de canal égale.

**Le prix à payer :** pour distinguer 4096 points de constellation, il faut un signal **extrêmement propre** (SNR très élevé, typiquement > 40 dB — valeur indicative, à vérifier sur la fiche du modèle exact). En pratique :
- Ça ne marche qu'à **courte distance**, en vue directe, sans interférence.
- En extérieur (distance, vent dans les arbres, humidité), les clients retomberont sur 1024-QAM ou 256-QAM 95 % du temps.
- Le 4K-QAM est donc un **bonus de courte portée**, pas un argument de couverture.

**Support eKit :** le 4096-QAM est annoncé sur les AP eKit Wi-Fi 7 (AP772E, AP673). **L'AP761 s'arrête au 1024-QAM** [constructeur].

**Règle terrain :** ne jamais promettre un débit basé sur le 4K-QAM à un client ou à ta direction. Dimensionne sur du 256-QAM (Wi-Fi 5/6 robuste), et considère tout ce qui est au-dessus comme du bonus.

## 18. Preamble puncturing et multi-RU

**Le problème :** en Wi-Fi 6, si une petite portion d'un canal 80 MHz est brouillée (un radar, un réseau voisin sur un sous-canal), **tout le canal 80 MHz est inutilisable** — l'AP retombe sur 40 ou 20 MHz. C'est du gâchis : 60 MHz propres jetés à cause de 20 MHz brouillés.

**La solution Wi-Fi 7 :**
- **Preamble puncturing** : l'AP « perfore » le canal — il utilise les morceaux propres et ignore le morceau brouillé. Un 80 MHz avec 20 MHz brouillés devient un **60 MHz utiles** au lieu de retomber à 40 MHz.
- **Multi-RU** : en Wi-Fi 6, un client ne recevait qu'**un seul** bloc de sous-porteuses (RU) par trame OFDMA. En Wi-Fi 7, un client peut recevoir **plusieurs RU** à la fois → l'ordonnanceur remplit le canal beaucoup plus efficacement, surtout avec peu de clients gourmands.

**Pourquoi c'est important en extérieur :** l'extérieur est un environnement **sale** en radio (radars météo/aviation en 5 GHz, réseaux voisins, Bluetooth, fours à micro-ondes des food-trucks…). Le puncturing, c'est la différence entre « le canal 80 MHz tient » et « on retombe à 40 MHz dès qu'un radar passe ». C'est une fonction **de robustesse**, pas de débit crête — et c'est pour ça qu'elle compte plus sur le terrain que le 4K-QAM.

> **Sur l'AP761 :** pas de puncturing, pas de multi-RU. En cas d'interférence sur un sous-canal, il retombe à la largeur inférieure (80 → 40 → 20 MHz). D'où l'importance du plan de canaux (chap. 37–39).

## 19. 6 GHz et réglementation : le point pour l'Europe

**La théorie :** le Wi-Fi 7 adore la bande **6 GHz** (5945–7125 MHz) : beaucoup de spectre propre, canaux 320 MHz possibles, pas de clients legacy qui pourrissent l'airtime (le 6 GHz n'accepte que du WPA3).

**La réglementation (situation au 27/09/2026 — à vérifier auprès du régulateur local avant tout projet) :**
| Zone | Statut 6 GHz |
|---|---|
| **Union européenne** | Bande **5945–6425 MHz** ouverte au Wi-Fi en mode **LPI** (Low Power Indoor — intérieur, faible puissance) et VLP par décision UE 2021/1067. La partie haute **6425–7125 MHz** : statut en discussion, usage Wi-Fi non généralisé. |
| **France (ARCEP)** | Suit le cadre UE : 6 GHz cantonné à l'**intérieur** en faible puissance. **Pas d'usage extérieur** en 6 GHz dans le cadre actuel. |
| **USA (FCC)** | 6 GHz ouvert en intérieur ET extérieur (avec AFC pour l'extérieur en standard power). |
| **Reste du monde** | Très variable : certains pays n'ont pas ouvert le 6 GHz du tout. |

**Conséquence directe pour l'AP761 et l'outdoor en Europe :**
1. L'AP761 n'a pas de radio 6 GHz de toute façon — le sujet est clos pour lui.
2. Même les AP eKit Wi-Fi 7 **outdoor** (AP771/AP772E) sont bi-bande 2.4/5 GHz : les constructeurs ne mettent pas de 6 GHz sur de l'outdoor destiné au marché UE, parce que la réglementation ne le permet pas (ou pas encore).
3. **Ne jamais activer/importer un AP 6 GHz configuré en région US pour l'utiliser en France** : non-conformité réglementaire + risque de brouiller des services prioritaires (liaisons fixes, radioastronomie selon les sous-bandes).

**À vérifier sur la fiche du modèle exact** : le « Country Code & Channel Compliance Table » de Huawei liste les canaux autorisés par pays — c'est LA référence avant d'activer une bande ou une largeur de canal.

## 20. Ce que l'AP761 ne fait PAS (tableau Wi-Fi 6 vs Wi-Fi 7)

Pour couper court aux confusions commerciales, le tableau à garder sous la main :

| Fonction | AP761 (Wi-Fi 6) [constructeur] | Wi-Fi 7 (théorie / modèles eKit Wi-Fi 7) |
|---|---|---|
| Standard | 802.11ax | 802.11be |
| MLO | ❌ Non | ✅ (AP771/AP772E/AP371/AP673) |
| Canaux 320 MHz | ❌ Non (max 80 MHz) | ⚠️ Seulement avec 6 GHz (AP673 indoor — à vérifier) ; **non sur l'outdoor eKit** |
| 4096-QAM | ❌ Non (max 1024-QAM) | ✅ (modèles Wi-Fi 7) |
| Bande 6 GHz | ❌ Non | ⚠️ AP673 indoor uniquement — à vérifier ; pas sur l'outdoor |
| Multi-RU / puncturing | ❌ Non | ✅ (modèles Wi-Fi 7) |
| 16 flux spatiaux | ❌ Non (2×2) | Standard : oui ; produits eKit : à vérifier par modèle |
| WPA3 | ✅ Supporté (SAE + Enterprise) | ✅ (obligatoire pour la certification Wi-Fi 7) |
| OFDMA / MU-MIMO | ✅ | ✅ (améliorés) |
| BSS Coloring / TWT | ✅ | ✅ |
| Port 2.5G / 10G | ❌ (1× GE + 1× SFP GE) | ✅ AP772E : 2.5GE + 10GE SFP+ |
| PoE++ 802.3bt | ❌ Non requis (at/af) | ✅ Requis/recommandé (AP772E) |

## 21. La gamme eKit Wi-Fi 7 réelle : AP771, AP772E, AP371, AP673

Si un jour tu dois répondre à « on veut du Wi-Fi 7 », voici les vrais modèles eKit (valeurs vérifiées le 27/09/2026 — les détails fins restent **à vérifier sur la fiche du modèle exact**) :

**Outdoor (les remplaçants « esprit AP761 » en Wi-Fi 7) :**
| | AP771 | AP772E |
|---|---|---|
| Wi-Fi | 7 (802.11be), bi-bande 2.4/5 GHz | 7 (802.11be), bi-bande 2.4/5 GHz |
| Débit max | 3.57 Gbps | **6.45 Gbps** (5.76 @ 5 GHz + 0.688 @ 2.4 GHz) |
| Ports | à vérifier | **1× 2.5GE RJ45 (PoE) + 1× 10GE SFP+** |
| PoE | à vérifier | **802.3af/at/bt** ou DC |
| Protection | à vérifier | IP68, −40 à +70 °C, 6 kA |
| Portée indicative | ~130 m | ~250 m (optimale) |
| Fonctions Wi-Fi 7 | MLO, 4096-QAM, multi-RU | MLO, 4096-QAM, OFDMA, MU-MIMO |

**Indoor :**
| | AP371 | AP673 |
|---|---|---|
| Wi-Fi | 7, bi-bande 2.4/5 GHz | 7, **tri-bande** (avec 6 GHz) |
| Débit max | 3.57 Gbps (BE3600) | à vérifier |
| Ports | 1× 2.5GE PoE | à vérifier |
| Particularité | Le Wi-Fi 7 d'entrée de gamme | **320 MHz sur 6 GHz**, MLO, 4096-QAM |

**Lecture stratégique :** Huawei a choisi de ne pas mettre de 6 GHz sur l'outdoor eKit (réglementation UE, coût). Le Wi-Fi 7 outdoor eKit = du Wi-Fi 7 « sans 320 MHz », dont les gains viennent du MLO et de l'efficacité. Si ton besoin outdoor est de la **couverture fiable**, l'AP761 Wi-Fi 6 reste pertinent et deux fois moins cher ; si c'est de la **densité/latence**, regarde l'AP772E — et refais tout ton budget PoE et câblage (chap. 25).

## 22. Installation physique : où poser un AP extérieur

Un AP761 mal placé ne sera jamais rattrapé par la configuration. Les règles de placement, dans l'ordre d'importance :

1. **Vue directe sur la zone à couvrir.** Le Wi-Fi ne traverse pas bien les murs, et en extérieur les obstacles sont les bâtiments, les arbres (l'eau des feuilles absorbe le 5 GHz), les camions stationnés. Un AP à 6 m avec vue directe couvre 3× mieux qu'un AP à 3 m derrière un auvent métallique.
2. **Au centre du besoin, pas au centre du bâtiment.** On fixe souvent l'AP « où c'est facile » (à côté du local technique). Non : on le met **où sont les clients**, quitte à tirer 80 m de câble.
3. **Hauteur : 4 à 8 m.** En dessous de 4 m : vandalisme, obstacles humains/véhicules. Au-dessus de 8 m : zone morte au pied du mât (lobe vertical 20° en 5 GHz — chap. 9), et les clients voient l'AP sous un mauvais angle.
4. **Loin des sources de bruit :** moteurs, variateurs, postes de soudure, fours à micro-ondes des cuisines extérieures, mâts d'antennes 4G/5G (saturation du récepteur), enseignes LED à alimentation à découpage bon marché.
5. **Penser à la maintenance :** pourra-t-on y accéder dans 3 ans sans louer une nacelle ? Un AP inaccessible = un AP jamais dépoussiéré, jamais vérifié.

**Schéma de réflexion (à dessiner sur plan avant chaque chantier) :**
```
        [Bâtiment]
            |
   (zone morte = arrière de l'AP)
            |
    ┌───────AP761───────┐  <- 65° de secteur utile
    │                   │
    │   COUR / PARKING  │  <- clients ici, vue directe
    │                   │
```
Flécher le secteur de 65° sur le plan. Tout client hors du cône = non couvert. Si la zone fait plus de 65°, il faut 2 AP en secteurs opposés ou un autre modèle.

## 23. Montage mural et sur mât — procédure pas à pas

**Montage mural (façade) :**
1. Choisir un mur **porteur** (pas un bardage seul). Sonder si doute.
2. Présenter l'étrier, marquer les perçages au niveau (un AP penché = un secteur penché).
3. Percer, cheviller (chevilles adaptées au support : béton, brique creuse, parpaing — pas les mêmes).
4. Fixer l'étrier, **serrage au couple raisonnable** : trop serré sur brique creuse = éclatement.
5. Présenter l'AP, orienter le secteur vers la zone (chap. 22), régler le downtilt (quelques degrés vers le bas si la zone est en contrebas).
6. Passer le câble par le **presse-étoupe orienté vers le bas**, serrer le presse-étoupe **à la main + un quart de tour** (trop serré = écrasement du câble et du joint ; pas assez = entrée d'eau).
7. Faire une **boucle d'égouttage** : le câble descend sous l'AP puis remonte — l'eau suit la gravité et goutte au point bas au lieu de suivre le câble jusqu'au presse-étoupe.

**Montage sur mât :**
1. Vérifier le mât : verticalité, rigidité (pas de fléchissement au vent), mise à la terre existante.
2. Utiliser les colliers du kit (ou colliers inox adaptés au diamètre du mât — **à vérifier sur la fiche du modèle exact** pour la plage de diamètres).
3. Serrer en croix, progressivement. Revérifier après 48 h (tassement).
4. Même règle : presse-étoupe vers le bas, boucle d'égouttage.

**Erreurs vues sur le terrain :**
- ❌ AP fixé sur une gouttière ou un bardage léger : ça vibre, ça se desserre, le secteur bouge.
- ❌ Câble qui arrive **par le haut** dans le presse-étoupe : entonnoir à eau garanti.
- ❌ Serrage du presse-étoupe à la pince : joint écrasé, IP68 foutu.
- ❌ AP collé sous un auvent métallique : le métal fait écran, le secteur est haché.

## 24. Câblage : choix du câble, distance, étanchéité

**Le câble :** c'est le composant le moins cher du chantier et la cause n° 1 des pannes « mystères » 2 ans après. Ne pas lésiner.

| Critère | Exigence |
|---|---|
| Type | Câble **extérieur** (gaine PE résistante UV), **blindé F/UTP ou S/FTP** |
| Âme | **100 % cuivre**, 24 AWG (ou 23 AWG). **Jamais de CCA** (cuivre plaqué alu) |
| Catégorie | Cat6 minimum (le GE de l'AP761 passe sur Cat5e, mais le Cat6 laisse de la marge PoE et diaphonie) |
| Longueur max | **100 m** (90 m de câble + 10 m de cordons), norme Ethernet |
| Cheminement | Gaine ICTA ou chemin de câble ; **pas de câble nu au soleil** long terme même « extérieur » |

**Distances > 100 m :** c'est le cas d'usage du **port SFP** de l'AP761 — tirer une fibre optique jusqu'à l'AP (jarretière + SFP GE au bout), et alimenter en PoE via un injecteur local ou un switch PoE proche (chap. 6). La fibre règle aussi les problèmes de **différence de potentiel** entre bâtiments (pas de boucle de masse).

**Étanchéité des traversées :**
- Traversée de façade : fourreau + mastic/silicone extérieur, pente vers l'extérieur.
- Arrivée au local technique : le câble ne doit pas faire entrer l'eau dans la baie (boucle d'égouttage avant l'entrée).
- Connecteurs RJ45 : **ne jamais laisser un RJ45 nu en extérieur** — toujours le brancher sous le capot/presse-étoupe de l'AP ou dans un boîtier étanche. Un RJ45 oxydé = erreurs CRC = débit qui s'effondre par paliers (cas 4).

**Test du câble avant de quitter le chantier :** testeur de continuité a minima, **testeur qualifiant** (longueur, diaphonie, PoE) idéalement. Un câble qui « marche » à 100 Mbps au lieu du Gbps à cause d'une paire abîmée = un AP bridé dont personne ne comprendra la cause pendant des mois. **Noter les résultats dans le dossier de site.**

## 25. PoE : budget, injecteurs, switches — calculs

**Rappel valeurs constructeur :** 802.3at/af, **17.7 W max**, fonctions restreintes en 802.3af.

**Dimensionner le budget — la méthode :**
```
Par AP761 :  17.7 W (AP)
           +  2.5 W (pertes câble 100 m en PoE+)
           + 20 % de marge
           ≈ 25 W à réserver par AP sur le budget du switch
```

**Tableau de dimensionnement :**
| Switch (budget PoE total) | AP761 alimentables (règle des 25 W) |
|---|---|
| 8 ports, 130 W | 5 |
| 8 ports, 240 W | 9 (limité par le nb de ports) |
| 24 ports, 370 W | 14 |
| 24 ports, 740 W | 24 (limité par le nb de ports) |

**Injecteur PoE (quand le switch n'est pas PoE) :**
- Prendre un injecteur **802.3at, 30 W, gigabit** (pas 100 Mbps).
- L'injecteur se place **côté local technique**, pas en extérieur (sauf modèle extérieur — cher et rare).
- Un injecteur par AP : ça fait du câble et des prises 230 V en plus — à 4 AP et plus, un switch PoE devient plus propre et souvent moins cher.

**Chaînage interdit :** pas de PoE « pass-through » en cascade sur l'AP761 (il n'a pas de port PoE-Out — à vérifier sur la fiche du modèle exact, mais le datasheet ne mentionne qu'un PoE-In). Chaque AP = un câble PoE home-run jusqu'au switch/injecteur.

**Onduleur et PoE — ton métier, Zelef :**
- Un switch PoE 24 ports plein peut tirer **400–800 W** : c'est une charge à intégrer dans le dimensionnement de l'onduleur du local technique (voir ton guide onduleurs : `~/workspace/user/files/onduleurs_ups_guide.md`).
- En cas de coupure, les AP sont souvent les premiers qu'on accepte de perdre — ou les derniers, si le Wi-Fi porte la téléphonie ou le contrôle d'accès. **Décider à l'avance** et le noter dans le plan de délestage.
- **Astuce :** programmer l'extinction des SSID non critiques sur ordre (via le cloud eKit ou un script) pour allonger l'autonomie sur batterie.

## 26. Protection foudre et mise à la terre

L'AP761 a une protection **6 kV sur les ports Ethernet** [constructeur] : c'est une protection contre les **surtensions induites**, pas contre la foudre directe. Un AP sur un mât en extérieur est une cible : la protection se conçoit en couches.

**Les 3 couches :**
1. **Paratonnerre / zone de protection :** si le site est en zone à risque (plateau exposé, pylône), l'AP doit être dans le volume de protection d'un paratonnerre (règle de la sphère fictive — voir un spécialiste, norme NF EN 62305).
2. **Parafoudre Ethernet (SPD) :** un parafoudre **RJ45 PoE-compatible** (il doit laisser passer le PoE !) monté **à chaque extrémité** du câble : un près de l'AP, un à l'entrée du bâtiment. Raccordé à la terre avec un conducteur **court et rectiligne** (16 mm² cuivre typique — selon norme locale).
3. **Mise à la terre de l'AP et du mât :** le boîtier métal et le mât sont reliés à la terre. Le blindage du câble est mis à la terre **d'un seul côté** (côté bâtiment) pour éviter les boucles de masse — sauf consigne contraire du fabricant du parafoudre.

**Ce qu'il ne faut PAS faire :**
- ❌ Compter sur les 6 kV internes comme seule protection sur un site exposé.
- ❌ Mettre un parafoudre Ethernet non-PoE : il bloque ou dégrade l'alimentation.
- ❌ Raccorder la terre du parafoudre sur une « terre » flottante ou sur la tuyauterie sans continuité.
- ❌ Oublier la **vérification annuelle** : un parafoudre qui a encaissé une surtension est peut-être mort sans le montrer (voyant d'état à contrôler, chap. 101).

**Après un orage :** si l'AP ne répond plus, tester dans l'ordre : injecteur/switch PoE → parafoudre (le shunter temporairement pour tester) → câble → AP. Les parafoudres sacrifient souvent leur vie pour sauver l'AP : c'est leur travail.

## 27. Checklist pré-installation chantier

À cocher **avant** de monter sur l'échelle. Un chantier préparé = une demi-journée ; un chantier improvisé = deux jours et trois allers-retours.

**Site :**
- [ ] Emplacement validé sur plan avec le secteur de 65° tracé (chap. 22).
- [ ] Support vérifié (porteur, vertical, rigide).
- [ ] Accès maintenance future réfléchi (échelle, nacelle, chemin).
- [ ] Autorisations obtenues (copropriété, mairie si façade visible/protégée).

**Électrique / réseau :**
- [ ] Chemin de câble défini et mesuré (< 100 m ou fibre prévue).
- [ ] Switch PoE 802.3at avec **budget suffisant** (chap. 25) ou injecteur 30 W gigabit.
- [ ] Parafoudres prévus si site exposé (chap. 26).
- [ ] Prise 230 V pour l'injecteur si besoin, sur circuit secouru si le Wi-Fi est critique.

**Logique :**
- [ ] Plan d'adressage : VLAN management, SSID, sous-réseaux.
- [ ] SSID et clés **générés** (pas « on verra sur place ») — clés fictives notées dans le dossier de site, jamais « admin/admin ».
- [ ] Compte cloud eKit prêt si mode cloud ; AC joignable si mode Fit.

**Matériel :**
- [ ] AP761 + kit de montage + presse-étoupes vérifiés (chap. 5).
- [ ] Câble extérieur blindé 100 % cuivre en quantité suffisante (+ 10 % de mou).
- [ ] Connecteurs RJ45 blindés Cat6 + capuchons.
- [ ] Outillage : perceuse, chevilles adaptées, niveau, testeur câble, smartphone avec app eKit, étiqueteuse.

**Le jour J :**
- [ ] Photo de l'étiquette AP (S/N, MAC, QR) avant montage.
- [ ] Test du câble **avant** raccordement final.
- [ ] Mise sous tension, vérification LED, adoption cloud/Fat (chap. 28–35).
- [ ] Test de couverture : tour de la zone avec un smartphone (relevés RSSI, chap. 60).
- [ ] Photos « après » + mise à jour du dossier de site.

## 28. Première mise en route : les 3 chemins

Selon le mode choisi (chap. 11), la première mise en route suit l'un de ces trois chemins. **Ne mélange pas les méthodes** : un AP à moitié configuré en Fat puis adopté en cloud, c'est une heure de perdue.

| Mode | Chemin | Chapitres |
|---|---|---|
| **Cloud** (recommandé PME multi-sites) | App eKit → scan QR → créer/adopter dans un site → config poussée par le cloud | 29, 30 |
| **Fat** (autonome) | Web local ou CLI → config manuelle complète | 31, 34 |
| **Fit** (contrôleur) | L'AP cherche l'AC (DHCP option 43 ou DNS) → l'AC pousse la config | 32 |

**Dans tous les cas, l'ordre est :**
1. Alimenter l'AP (PoE) — attendre le démarrage complet (LED verte fixe, ~2–3 min).
2. L'AP obtient une IP (DHCP du réseau — prévoir un serveur DHCP sur le VLAN de management).
3. Adopter/configurer selon le mode.
4. Vérifier : SSID diffusés, client test connecté, débit testé, supervision OK.
5. **Sauvegarder** (chap. 35).

**Piège n° 1 : le VLAN de management.** Si ton switch envoie le port en trunk avec un VLAN natif différent de celui où est le DHCP, l'AP n'aura jamais d'IP. Pour la mise en route, le plus simple : port en **access** sur le VLAN de management, DHCP actif. On passe en trunk après (chap. 52).

**Piège n° 2 : l'heure.** Un AP dont l'horloge est fausse (pas de NTP) génère des certificats/logs incohérents et peut refuser de se connecter au cloud (TLS). Configurer le NTP dès le début (chap. 70).

## 29. Onboarding via l'app HUAWEI eKit (QR code)

L'app **HUAWEI eKit** (Android/iOS, gratuite) est la télécommande de l'écosystème : onboarding, supervision, alertes. C'est la méthode la plus rapide pour un AP761 en mode cloud.

**Procédure :**
1. Installer l'app HUAWEI eKit, créer un compte (ou se connecter).
2. Créer un **site** : nom explicite (`Client_Dupont_Siege`, `Usine_Nord`) — un site = un bâtiment ou un ensemble géré ensemble.
3. **Ajouter un équipement → scanner le QR code** de l'étiquette de l'AP (d'où l'intérêt de la photo, chap. 5).
4. L'app détecte l'AP en **BLE** à proximité : elle lit son état (démarré ? IP obtenue ? version firmware ?).
5. L'AP est **adopté** dans le site : il télécharge sa configuration et son firmware si besoin, puis redémarre.
6. Vérifier dans l'app : AP « en ligne », vert.

**Si le QR ne passe pas :**
- Saisie manuelle du numéro de série (sur l'étiquette).
- Vérifier que le smartphone a le Bluetooth activé et les autorisations de localisation (le BLE en a besoin sur Android).
- Se placer à moins de quelques mètres de l'AP.

**Bonnes pratiques :**
- Nommer l'AP dès l'adoption : `AP761-Cour-Est`, pas `AP-3F2A`. Un nom qui dit **où** il est.
- Renseigner la **position sur le plan** du site dans l'app si la fonction existe : dans 2 ans, tu sauras quel AP est où sans monter sur le toit.
- **Un seul administrateur « propriétaire »** du site + des comptes techniciens : éviter que 5 personnes se battent sur la même config.

## 30. Mode cloud : créer un site, adopter l'AP

Une fois l'AP adopté (chap. 29), la configuration se fait dans la console cloud (web ou app). Logique eKit : on configure des **modèles** (SSID, radio) appliqués au site, pas AP par AP.

**Structure logique type :**
```
Site « Usine_Nord »
├── Réseau filaire (VLAN, DHCP)
├── Wi-Fi
│   ├── SSID « USINE-PROD » (WPA2-Enterprise, VLAN 20)
│   ├── SSID « USINE-INVITE » (Portal, VLAN 30)
│   └── Paramètres radio (canaux, puissance)
└── Équipements
    ├── AP761-Cour-Est (adopté, en ligne)
    └── AP761-Parking (adopté, en ligne)
```

**Paramètres à régler au niveau du site (avant de brancher les clients) :**
1. **Pays/région réglementaire** : conditionne les canaux et puissances autorisés. **À régler en premier** — un mauvais domaine réglementaire = canaux interdits ou puissance bridée (chap. 41).
2. **SSID** : nom, sécurité (chap. 43–49), VLAN (chap. 52).
3. **Radio** : canaux 2.4/5 GHz, largeurs, puissances (chap. 37–40).
4. **NTP, DNS** : pour les logs et le TLS (chap. 70).

**Le cloud en panne, et le Wi-Fi ?** Question classique : **le Wi-Fi local continue de fonctionner** si la connexion au cloud tombe (l'AP garde sa dernière configuration). Ce qu'on perd : l'administration à distance, les stats temps réel, les alertes. Au retour d'Internet, l'AP se resynchronise. **Mais** : ne pas planifier de mise en service un jour où la fibre du site n'est pas encore active — l'adoption initiale a besoin d'Internet.

## 31. Mode Fat : accès web local et CLI

En mode **Fat**, l'AP761 est autonome : on l'administre directement, en web ou en CLI. C'est le mode du site isolé sans Internet.

**Accès web :**
1. Brancher un PC sur le même réseau que l'AP (ou directement sur l'AP via un switch).
2. Trouver l'IP de l'AP : via le serveur DHCP (chercher le hostname/MAC) ou via l'app eKit en BLE.
3. Ouvrir `https://<ip-ap>` (accepter l'avertissement de certificat autosigné — normal).
4. S'identifier avec le compte admin (identifiants par défaut : **à vérifier sur la fiche du modèle exact** — les changer **immédiatement**, chap. 106).

**Accès CLI (SSH) :**
```
# Depuis un poste du même réseau :
ssh admin@192.168.1.10
# Mot de passe : <celui défini> (jamais le défaut en production)
<AP761> display version          # vérifier modèle + version firmware
<AP761> display ip interface brief
<AP761> system-view              # passer en mode configuration
[AP761] sysname AP761-Cour-Est   # nom explicite, tout de suite
```

**Commandes de lecture indispensables (ne modifient rien) :**
```
<AP761> display current-configuration   # la config complète
<AP761> display wlan ap all             # état des radios/AP
<AP761> display wlan sta                # clients connectés (selon version)
<AP761> display interface brief
<AP761> display logbuffer               # journaux récents
```

> ⚠️ Les noms de commandes `display wlan ...` varient selon la version logicielle : utilise **la touche `?`** et la complétion par tabulation. Exemple : `display wlan ?` liste les options disponibles sur TA version. C'est la méthode la plus fiable, bien plus que n'importe quel guide.

## 32. Mode Fit : raccordement à un AC (WAC)

En mode **Fit**, l'AP761 est une « antenne intelligente » pilotée par un **contrôleur (AC)** — dans l'écosystème eKit, typiquement un **WAC** (ex. AC650 ou passerelle AR700 avec fonction WAC — à vérifier sur la fiche du modèle exact pour la compatibilité).

**Principe :** l'AP démarre, obtient une IP par DHCP, **découvre l'AC** (option DHCP 43, DNS, ou adresse statique), établit un tunnel **CAPWAP**, télécharge sa configuration, et remonte ses clients à l'AC.

**Découverte de l'AC — les 3 méthodes :**
1. **DHCP option 43** (la plus propre) : le serveur DHCP annonce l'IP de l'AC aux AP.
2. **DNS** : l'AP résout un nom prédéfini (ex. `huawei-wac` — à vérifier sur la fiche du modèle exact).
3. **Statique en CLI** :
```
<AP761> system-view
[AP761] capwap source interface vlanif 10
[AP761] controller ip address 192.168.10.5
```

**Vérifications côté AP :**
```
<AP761> display capwap status       # état du tunnel vers l'AC
<AP761> display wlan ap all          # l'AP doit apparaître « normal » côté AC
```

**Quand choisir Fit plutôt que Cloud :**
- Tu as déjà un AC et un parc existant.
- Tu veux l'**analyse de spectre** (seul le mode Fit la supporte sur l'AP761 — chap. 65).
- Le site n'a pas d'Internet fiable mais a un AC local.
- Politique interne : pas de management cloud (données sensibles).

**Inconvénient :** l'AC est un point central — s'il tombe, les AP en Fit perdent leur cerveau (les clients déjà connectés continuent souvent en local selon la config, mais plus de nouvelles associations ni de roaming propre). Prévoir la redondance d'AC sur les sites critiques — à vérifier sur la fiche du modèle exact.

## 33. Changer de mode : Fat ↔ Fit ↔ Cloud

On ne change pas de mode « pour voir » : chaque bascule **redémarre l'AP et réinitialise sa configuration locale**. Procédure avec fenêtre de maintenance.

**Principe général (CLI, depuis le mode Fat) :**
```
<AP761> system-view
[AP761] ap-mode cloud     # vers le cloud eKit
# ou
[AP761] ap-mode fit       # vers contrôleur (CAPWAP)
# ou
[AP761] ap-mode fat       # retour autonome
[AP761] quit
<AP761> save              # sauvegarder avant redémarrage
<AP761> reboot
```

**Points d'attention :**
- **Sauvegarder la config** avant (`save` + export, chap. 72) : la bascule efface.
- Vérifier que le **firmware supporte le mode cible** : certains firmwares eKit sont « cloud-only » par défaut — à vérifier sur la fiche du modèle exact.
- Après bascule vers Fit : l'AP doit trouver l'AC (chap. 32) sinon il boucle en découverte — prévoir l'option DHCP 43 **avant**.
- Après bascule vers Cloud : adoption par QR/BLE (chap. 29).
- **Ne jamais basculer à distance un AP dont on n'a pas d'accès physique de secours** : si la bascule échoue, c'est l'échelle.

## 34. Configuration initiale minimale (recette Fat AP)

La recette « ça marche en 15 minutes » en mode Fat, en CLI. Adapter les noms, VLAN et clés (clés **fictives** ci-dessous — génère les tiennes).

```
<AP761> system-view
[AP761] sysname AP761-Cour-Est

# --- Management ---
[AP761] vlan batch 10 20 30
[AP761] interface vlanif 10
[AP761-Vlanif10] ip address 192.168.10.11 24
[AP761-Vlanif10] quit
[AP761] ip route-static 0.0.0.0 0.0.0.0 192.168.10.1

# --- Port uplink en trunk ---
[AP761] interface gigabitethernet 0/0/0
[AP761-GigabitEthernet0/0/0] port link-type trunk
[AP761-GigabitEthernet0/0/0] port trunk pvid vlan 10
[AP761-GigabitEthernet0/0/0] port trunk allow-pass vlan 10 20 30
[AP761-GigabitEthernet0/0/0] quit

# --- WLAN : profil SSID ---
[AP761] wlan
[AP761-wlan-view] ssid-profile name SP-CORP
[AP761-wlan-ssid-prof-SP-CORP] ssid ENTREPRISE
[AP761-wlan-ssid-prof-SP-CORP] quit

# --- WLAN : profil sécurité WPA2-PSK ---
[AP761-wlan-view] security-profile name SEC-CORP
[AP761-wlan-sec-prof-SEC-CORP] security wpa2 psk pass-phrase ChangeMoi_2026! aes
[AP761-wlan-sec-prof-SEC-CORP] quit

# --- WLAN : profil VAP (SSID + sécu + VLAN + forwarding) ---
[AP761-wlan-view] vap-profile name VAP-CORP
[AP761-wlan-vap-prof-VAP-CORP] ssid-profile SP-CORP
[AP761-wlan-vap-prof-VAP-CORP] security-profile SEC-CORP
[AP761-wlan-vap-prof-VAP-CORP] service-vlan vlan-id 20
[AP761-wlan-vap-prof-VAP-CORP] forward-mode direct-forward
[AP761-wlan-vap-prof-VAP-CORP] quit

# --- Domaine réglementaire (France = FR) ---
[AP761-wlan-view] regulatory-domain-profile name DOM-FR
[AP761-wlan-view] ap-group name default
[AP761-wlan-ap-group-default] regulatory-domain-profile DOM-FR

# --- Attacher le VAP aux deux radios ---
[AP761-wlan-ap-group-default] vap-profile VAP-CORP wlan 1 radio 0
[AP761-wlan-ap-group-default] vap-profile VAP-CORP wlan 1 radio 1
[AP761-wlan-ap-group-default] quit
[AP761-wlan-view] quit

# --- NTP + sauvegarde ---
[AP761] ntp-service unicast-server 192.168.10.1
[AP761] quit
<AP761> save
```

**Vérification :**
```
<AP761> display wlan vap all       # le VAP doit être « up »
<AP761> display current-configuration | include ssid
```
Puis test avec un smartphone : association, IP obtenue, navigation, débit (speedtest local de préférence — un iperf vers un serveur du LAN, pas vers Internet, pour isoler le Wi-Fi).

> 📌 **Avertissement version :** les noms de vues (`wlan-ssid-prof-…`, `vap-profile`, `forward-mode`) sont la syntaxe classique des AP Huawei VRP. Selon ta version logicielle, certains mots-clés peuvent différer : **valide chaque bloc avec `?` avant de l'appliquer**, et consulte le guide de configuration de ta version.

## 35. Sauvegarde de la configuration initiale

**Règle d'or :** une configuration non sauvegardée **n'existe pas**. Le jour où l'AP prend la foudre, tu dois pouvoir le remplacer en 30 minutes avec un fichier.

**En mode Fat (CLI) :**
```
<AP761> save                          # écrit dans la flash (vrpcfg.zip)
<AP761> backup configuration to 192.168.10.100   # vers serveur TFTP/SFTP (selon version)
# ou via SFTP depuis un poste :
sftp admin@192.168.10.11
sftp> get vrpcfg.zip ./sauvegardes/AP761-Cour-Est_2026-09-27.zip
```

**En mode Cloud :** la configuration vit dans le cloud — elle est versionnée par la plateforme. **Exporter quand même** une copie après chaque changement majeur (fonction d'export du portail — à vérifier sur la fiche du modèle exact), et noter la version firmware associée.

**Convention de nommage des sauvegardes :**
```
sauvegardes/
├── AP761-Cour-Est_2026-09-27_v1-initiale.zip
├── AP761-Cour-Est_2026-10-15_v2-wpa3.zip
└── AP761-Parking_2026-09-27_v1-initiale.zip
```
Un fichier = `NomAP_AAAA-MM-JJ_vN-motif`. Stockage : dossier partagé d'équipe + copie hors site. **Tester la restauration une fois** (chap. 73) : une sauvegarde jamais testée est une promesse, pas une sécurité.

## 36. Configuration radio : concepts de base

Avant de toucher aux réglages, les 5 paramètres qui gouvernent une radio Wi-Fi, et ce qu'ils font vraiment :

| Paramètre | Ce que c'est | Effet si mal réglé |
|---|---|---|
| **Canal** | La fréquence d'émission | 2 AP sur le même canal = partage de l'airtime, débit divisé |
| **Largeur de canal** | 20 / 40 / 80 MHz : la « largeur de l'autoroute » | Trop large = plus de débit mais plus d'interférences et moins de canaux dispo |
| **Puissance TX** | La « voix » de l'AP (en dBm) | Trop fort = l'AP crie loin mais n'entend pas les clients faibles (asymétrie) ; trop faible = trous de couverture |
| **Débits de base** | Les débits minimums autorisés | Trop bas = clients zombies qui monopolisent l'airtime (chap. 13) |
| **Domaine réglementaire** | Le pays (FR, etc.) | Mauvais pays = canaux interdits utilisés ou puissance non conforme |

**Principe cardinal de la radio Wi-Fi :** c'est un médium **partagé et à écoute avant émission** (CSMA/CA). Deux AP sur le même canal ne se « brouillent » pas comme deux radios FM : ils **se partagent** le temps de parole. Le but du plan de canaux n'est pas d'éviter tout chevauchement à tout prix, c'est de **minimiser le partage** là où le trafic est fort.

**Ordre de configuration recommandé :**
1. Domaine réglementaire (chap. 30/34) — en premier, toujours.
2. Canaux (chap. 37–38).
3. Largeurs (chap. 39).
4. Puissances (chap. 40).
5. Débits de base et band steering (chap. 13, 42).

## 37. Plan de canaux 2.4 GHz — méthode terrain

La bande 2.4 GHz (2.412–2.472 GHz en Europe) est **minuscule** : 13 canaux de 20 MHz qui se chevauchent, dont **seulement 3 ne se recouvrent pas : 1, 6, 11**. C'est la règle la plus importante du Wi-Fi, et elle n'a pas changé depuis 1999.

**Le plan en 20 MHz (le seul raisonnable en 2.4 GHz) :**
```
Canaux :  1   2   3   4   5   6   7   8   9   10  11  12  13
               └─────┘       └─────┘       └─────────┘
              chevauchement    1, 6, 11 = les 3 canaux propres
```

**Méthode terrain (sans analyseur de spectre) :**
1. Avec un smartphone + app d'analyse Wi-Fi (ex. « WiFi Analyzer », open source), faire un scan depuis 3–4 points de la zone.
2. Noter les réseaux voisins forts (RSSI > −80 dBm) et leurs canaux.
3. Choisir pour chaque AP le canal **1, 6 ou 11** le moins occupé par les voisins **forts**.
4. Deux AP761 à portée l'un de l'autre : **jamais le même canal** — alterner 1 / 6 / 11.

**Le 40 MHz en 2.4 GHz : à proscrire en extérieur partagé.** Le 40 MHz en 2.4 GHz occupe 2 des 3 canaux propres : tu doubles ton débit théorique mais tu divises par deux la capacité du voisinage — et en extérieur, le voisinage (lotissements, commerces) est partout. **Règle : 2.4 GHz = 20 MHz, toujours.** (Le datasheet confirme que l'AP761 supporte le 40 MHz en 2.4 GHz [constructeur] — le supporter n'est pas une raison de l'utiliser.)

**Canaux 12 et 13 :** autorisés en Europe, interdits aux USA. Si tes clients sont des objets connectés « US-first » (certaines caméras, gadgets), ils ne verront pas les canaux 12/13. **Reste sur 1/6/11** sauf raison précise.

## 38. Plan de canaux 5 GHz — méthode terrain

La bande 5 GHz européenne (5.18–5.825 GHz selon le datasheet [constructeur]) offre **beaucoup plus de canaux** — c'est là que se joue le débit.

**Canaux 20 MHz usuels en Europe (liste indicative — à vérifier sur la fiche du modèle exact, tableau « Country Code & Channel Compliance ») :**
```
U-NII-1 (sans DFS) : 36, 40, 44, 48
U-NII-2 (DFS)      : 52, 56, 60, 64
U-NII-2e (DFS)     : 100, 104, 108, 112, 116, 120, 124, 128, 132, 136, 140
U-NII-3            : 149, 153, 157, 161, 165 (selon pays)
```

**Canaux non-DFS (36–48) :** pas de détection radar, pas de coupure surprise. **À privilégier** pour les usages critiques (voix, contrôle d'accès). Inconvénient : seulement 4 canaux 20 MHz (ou 2× 40 MHz, ou 1× 80 MHz) — vite saturés si les voisins s'y entassent aussi.

**Canaux DFS (52–140) :** beaucoup de spectre, mais l'AP doit **écouter 60 s avant d'émettre** (CAC — Channel Availability Check) et **quitter le canal** s'il détecte un radar (chap. 41, cas 8). À réserver aux usages non critiques ou quand les canaux propres sont saturés.

**Méthode terrain :**
1. Scanner la zone (smartphone + app) : noter les réseaux 5 GHz forts et leurs canaux.
2. Attribuer à chaque AP un canal 80 MHz (ou 40 MHz) **non chevauchant** avec ses voisins proches.
3. **Plan 80 MHz avec 2 AP proches :** AP1 = canal 36 (bloc 36–48), AP2 = canal 52 ou 100 (bloc DFS). Jamais deux blocs 80 MHz qui se recouvrent.
4. **Plan 80 MHz avec 4+ AP :** il faudra réutiliser — espacer au maximum les réutilisations (AP1 et AP4 sur le même canal s'ils sont à l'opposé du site).

**Tableau : combien de canaux 80 MHz propres en Europe :**
| Bloc | Canaux 80 MHz possibles | DFS ? |
|---|---|---|
| 36–48 | 1 (canal 42) | Non |
| 52–64 | 1 (canal 58) | Oui |
| 100–140 | 2–3 (106, 122, 138) | Oui |
| 149–165 | 1 (155, selon pays) | Non (selon pays) |

Soit **4 à 5 canaux 80 MHz** en tout — et les voisins les utilisent aussi. D'où l'intérêt du 40 MHz quand la densité augmente (chap. 39).

## 39. Largeur de canal : 20/40/80 MHz, que choisir

**Le compromis en une phrase :** canal large = plus de débit par client, mais moins de canaux disponibles et plus de sensibilité aux interférences.

| Largeur | Débit relatif | Canaux dispo (5 GHz EU) | Quand l'utiliser |
|---|---|---|---|
| 20 MHz | ×1 (référence) | ~13–19 | 2.4 GHz toujours ; 5 GHz en zone très dense ou très bruitée |
| 40 MHz | ×2 | ~6–9 | 5 GHz : bon compromis densité/débit en extérieur partagé |
| 80 MHz | ×4 | ~4–5 | 5 GHz : débit max, peu d'AP, environnement propre |
| 160 MHz | ×8 | 1–2 (DFS) | **Non supporté par l'AP761** (max 80 MHz [constructeur]) — cité pour mémoire |

**Recommandations terrain pour l'AP761 :**
- **2.4 GHz : 20 MHz.** Toujours. (chap. 37)
- **5 GHz, site isolé (campagne, zone industrielle propre) : 80 MHz.** Tu exploites le max du Wi-Fi 6.
- **5 GHz, site partagé (ville, lotissement, zone commerciale) : 40 MHz.** Tu divises par deux ta sensibilité aux voisins et tu doubles tes choix de canaux.
- **5 GHz, zone ultra-dense ou usage voix critique : 20 MHz** sur canaux non-DFS (36–48). La fiabilité avant le débit.

**Le piège du « 80 MHz partout » :** un canal 80 MHz qui chevauche un réseau voisin fort = **pire** qu'un 40 MHz propre. Le débit réel d'un 80 MHz brouillé est inférieur à celui d'un 40 MHz propre. **Un canal propre et étroit bat toujours un canal large et sale.** C'est LA phrase à retenir de ce chapitre.

**Configuration (principe CLI — noms à valider avec `?` selon version) :**
```
[AP761-wlan-view] radio-profile name RP-5G
[AP761-wlan-radio-prof-RP-5G] channel-width 40MHz
# ou 20MHz / 80MHz selon le plan
```

## 40. Puissance d'émission : régler sans bourriner

**Valeur constructeur :** puissance combinée max **28 dBm (2.4 GHz) / 27 dBm (5 GHz)**, réglable par pas de **1 dBm**, sous réserve de la réglementation locale.

**Le réflexe à perdre : « mettre à fond pour porter loin ».** La puissance max crée trois problèmes :
1. **Asymétrie :** l'AP crie à 27 dBm, le smartphone répond à 15–20 dBm. Le client « entend » l'AP de loin mais l'AP n'entend pas le client → associations fantômes, débits minables, roaming qui ne se déclenche pas (le client croit avoir du signal).
2. **Pollution :** tu arroses les voisins qui t'arrosent en retour — tout le monde perd en airtime.
3. **Non-conformité :** la réglementation limite la PIRE (puissance + gain d'antenne). Avec 11 dBi d'antenne, la puissance conduite doit être réduite d'autant : le firmware le fait si le domaine réglementaire est correct (chap. 36) — encore une raison de le régler en premier.

**Méthode de réglage terrain :**
1. Partir de la puissance **réglementaire max** du domaine (le firmware la connaît).
2. Faire un tour de la zone avec un smartphone : noter le RSSI aux limites de la zone utile.
3. **Baisser** jusqu'à ce que le RSSI en limite de zone soit ≈ **−67 dBm** (seuil voix/données confortables) à **−70 dBm** (données OK).
4. Entre deux AP qui se recouvrent : baisser pour que le recouvrement à −67 dBm soit de **15–20 %** — assez pour le roaming, pas assez pour se marcher dessus.

**Ordres de grandeur (indicatifs) :**
| Contexte | Puissance 5 GHz typique | Pourquoi |
|---|---|---|
| Cour 30×30 m, 1 AP | 14–17 dBm | Couvrir sans arroser le quartier |
| Parking 100 m, 2 AP | 17–20 dBm | Porter le long du parking |
| Grand site isolé | 20–23 dBm | Pas de voisins, on peut pousser |

> En 2.4 GHz, mettre **3 à 6 dB de moins** qu'en 5 GHz : le 2.4 GHz porte naturellement plus loin, et on veut que les clients **préfèrent le 5 GHz** (band steering, chap. 42).

## 41. DFS en 5 GHz : ce qu'il faut savoir

**DFS (Dynamic Frequency Selection)** [constructeur] : sur les canaux 52–140, l'AP doit détecter les **radars** (météo, aviation, militaires) et libérer le canal s'il en détecte un. C'est une obligation réglementaire, pas une option.

**Le cycle DFS :**
1. **CAC (Channel Availability Check) :** avant d'émettre sur un canal DFS, l'AP écoute **60 secondes** (10 minutes sur les canaux 120–128 dans certains pays). Pendant ce temps : pas de Wi-Fi sur cette radio.
2. **Surveillance continue :** l'AP écoute en permanence les signatures radar.
3. **Détection → évacuation :** si un radar est détecté, l'AP doit quitter le canal en quelques secondes et ne pas y revenir pendant **30 minutes** (NOP — Non-Occupancy Period).

**Conséquences terrain :**
- Un AP qui **redémarre** sur un canal DFS = 60 s sans Wi-Fi 5 GHz. Sur un site critique, préférer les canaux **non-DFS (36–48)**.
- Des **coupures inexpliquées** qui reviennent par météo orageuse ou près d'un aéroport = probablement du radar (cas 8).
- L'AP761 supporte le DFS [constructeur] : c'est transparent, mais il faut savoir que ça existe pour diagnostiquer.

**Bonnes pratiques :**
- Usages critiques (voix sur Wi-Fi, contrôle d'accès, vidéosurveillance) : **canaux 36–48 uniquement**.
- Noter dans le dossier de site quels AP sont en DFS : en dépannage, c'est la première question à poser.
- Après un changement de canal DFS, vérifier que les clients se sont bien réassociés (certains vieux clients mettent du temps à suivre).

## 42. Band steering et préférence de bande

**Le problème :** les clients préfèrent naturellement le **2.4 GHz** (il porte plus loin, le signal paraît « meilleur »), alors que le **5 GHz** est plus rapide et moins encombré. Résultat : tout le monde s'entasse sur le 2.4 GHz et le 5 GHz reste vide.

**Le band steering** (fonctionnalité classique des AP Huawei, nom exact à vérifier selon version) : l'AP **retarde ou refuse** les premières tentatives d'association en 2.4 GHz pour encourager les clients bi-bande à essayer le 5 GHz.

**Réglages qui aident (sans band steering) :**
1. **Puissance 2.4 GHz < puissance 5 GHz** de 3–6 dB (chap. 40) : le client « voit » le 5 GHz aussi fort que le 2.4 GHz.
2. **SSID identique** sur les deux bandes : le client choisit librement (la plupart des clients modernes préfèrent alors le 5 GHz).
3. **Débits de base 2.4 GHz relevés** (chap. 13) : les clients loin en 2.4 GHz ne s'associent plus.

**Quand NE PAS faire de steering :**
- Objets connectés 2.4 GHz uniquement (caméras, portiers) : ils ne comprendront pas le refus et boucleront en échec d'association. **SSID dédié 2.4 GHz** pour l'IoT (chap. 43).
- Si ça crée plus de problèmes que ça n'en résout : le steering agressif casse l'association de certains clients exotiques. **Tester avec le parc réel** avant de généraliser.

**Vérification :** après activation, contrôler la répartition des clients (`display wlan sta` ou console cloud) : objectif **> 70 % des clients bi-bande sur le 5 GHz** en zone couverte par les deux bandes.

## 43. SSID : stratégie de nommage et nombre

**Limite constructeur : 16 SSID par radio** [constructeur]. C'est un maximum technique, pas un objectif : **chaque SSID actif consomme de l'airtime** (beacons à ~10/s par SSID, sondes, etc.). Au-delà de 3–4 SSID, la surcharge devient mesurable.

**Stratégie recommandée (3 SSID types) :**
| SSID | Usage | Sécurité | VLAN |
|---|---|---|---|
| `ENTREPRISE` | Salariés | WPA2-Enterprise (802.1X) ou WPA3-SAE | 20 (réseau interne) |
| `INVITE` | Visiteurs | Portail captif | 30 (Internet seul, isolé) |
| `IOT` | Caméras, portiers, capteurs | WPA2-PSK clé forte, 2.4 GHz uniquement | 40 (restreint) |

**Règles de nommage :**
- Noms **courts, sans espaces ni accents** : `ENTREPRISE`, pas `WiFi de l'Entreprise Dupont & Fils`.
- **Ne pas mettre d'infos sensibles** dans le SSID (`COMPTA-SERVEUR` = indication gratuite pour un attaquant).
- SSID invité **distinct** : jamais le même SSID pour salariés et visiteurs avec un simple VLAN différent « invisible » — source d'erreurs.
- Éviter les SSID « pièges » trop communs (`FreeWifi`, `Linksys`) : ils attirent les confusions et les evil twins (chap. 64).

**SSID masqué (non diffusé) :** le datasheet mentionne le masquage de SSID par AP [constructeur]. **Ça n'apporte aucune sécurité** (le SSID se capture en 30 secondes avec un sniffer) et ça casse l'expérience (saisie manuelle, certains clients IoT ne gèrent pas). À réserver à des cas très spécifiques (SSID IoT qu'on ne veut pas voir dans la liste des voisins).

## 44. Sécurité : panorama WPA/WPA2/WPA3 sur l'AP761

**Supporté par l'AP761** [constructeur/revendeurs] : WEP, WPA/WPA2-PSK, WPA/WPA2-Enterprise, **WPA3-SAE**, WPA3-Enterprise, WAPI, 802.11w.

| Mode | Usage | Verdict terrain |
|---|---|---|
| WEP / WPA-TKIP | Historique | ❌ **Interdit** : cassé depuis 2001/2008. Ne jamais activer, même « pour un vieux truc ». |
| WPA2-PSK (AES) | PME, IoT, invités simples | ✅ Le standard de fait. Clé forte ≥ 20 caractères. |
| WPA3-SAE | PME moderne | ✅ Mieux que WPA2-PSK (résiste au brute-force hors ligne). **Vérifier la compatibilité du parc** (chap. 46). |
| WPA2/WPA3 transition | Parc mixte | ✅ Le bon compromis pendant la migration (chap. 47). |
| WPA2-Enterprise (802.1X) | Salariés, EAP-TLS/PEAP | ✅ Le top pour un parc géré : un identifiant par utilisateur, révocable. |
| WPA3-Enterprise | Exigeant | ✅ Si le RADIUS et les clients suivent. |
| Portail captif | Invités | ⚠️ Authentification, pas chiffrement : à combiner avec de l'isolation (chap. 49). |

**Question posée par la mission : « WPA3 obligatoire en Wi-Fi 7 ? »**
Réponse honnête : **le WPA3 n'est pas « obligatoire » au sens réglementaire**, mais :
- La certification **Wi-Fi CERTIFIED 7** exige WPA3.
- La bande **6 GHz** n'accepte QUE WPA3 (pas de WPA2, pas de transition) — pas de sujet en outdoor UE (chap. 19).
- **Sur l'AP761 (Wi-Fi 6), WPA3 est optionnel** : tu peux rester en WPA2 sans problème. Le WPA3-SAE est un plus, pas une obligation.

## 45. WPA2-PSK : configuration pas à pas

Le mode le plus courant en PME. Recette en CLI (Fat AP) — clé **fictive** :

```
[AP761-wlan-view] security-profile name SEC-PSK
[AP761-wlan-sec-prof-SEC-PSK] security wpa2 psk pass-phrase VoiciUneCleForte_2026! aes
[AP761-wlan-sec-prof-SEC-PSK] quit
```

**Règles pour une clé PSK sérieuse :**
- **≥ 20 caractères**, mélange imposé (majuscules, minuscules, chiffres, symboles). `VoiciUneCleForte_2026!` = 22 caractères, correct.
- **Une clé par SSID**, jamais la même clé pour salariés et invités.
- **Rotation** : changer la clé invité régulièrement (chaque trimestre, ou à chaque départ sensible pour le SSID salariés si pas de 802.1X).
- **Ne jamais** : `12345678`, le nom de l'entreprise, `password`, une date de naissance.

**En mode cloud :** la clé se saisit dans la fiche du SSID, avec option « afficher/masquer ». La plateforme ne stocke que le hash — mais **la clé transite** : la changer depuis un poste sûr, pas depuis le Wi-Fi invité d'un café.

**Limite du PSK à connaître :** avec une clé partagée, **quiconque connaît la clé peut déchiffrer le trafic des autres** (si la clé fuite, tout le monde est exposé). Pour les salariés, le 802.1X (chap. 48) supprime ce problème : chaque utilisateur a ses propres clés de session.

## 46. WPA3-SAE : configuration et limites

**SAE (Simultaneous Authentication of Equals)** remplace la poignée de main WPA2-PSK par un échange résistant aux attaques **hors ligne** : même avec une clé faible, un attaquant qui capture la poignée de main ne peut pas la brute-forcer à la maison — il doit attaquer en ligne, essai par essai, et l'AP le détecte.

```
[AP761-wlan-view] security-profile name SEC-SAE
[AP761-wlan-sec-prof-SEC-SAE] security wpa3 sae pass-phrase VoiciUneCleForte_2026! aes
[AP761-wlan-sec-prof-SEC-SAE] quit
```

**Limites à connaître avant d'activer :**
1. **Compatibilité clients :** les clients antérieurs à ~2018–2020 (et beaucoup d'objets connectés) ne connaissent pas SAE. En mode **WPA3-only**, ils ne se connectent plus du tout (cas 11).
2. **Pas de mélange avec WEP/TKIP** : le WPA3 impose AES (GCMP pour l'Enterprise 192 bits).
3. **Le roaming 802.11r** fonctionne avec SAE, mais certains vieux clients 802.11r ont des bugs d'interop — tester.

**Recommandation :** sur un SSID **salariés avec parc moderne** (smartphones récents, PC récents), le WPA3-SAE est un vrai plus. Sur un SSID **IoT/invités** avec du matériel hétéroclite, rester en WPA2-PSK ou utiliser le mode **transition** (chap. 47).

## 47. Mode transition WPA2/WPA3 : pour qui, pourquoi

Le mode **transition** diffuse un SSID acceptant **à la fois** WPA2-PSK et WPA3-SAE : les clients modernes négocient SAE, les anciens restent en WPA2. C'est le mode de migration idéal.

```
[AP761-wlan-view] security-profile name SEC-TRANSITION
[AP761-wlan-sec-prof-SEC-TRANSITION] security wpa2 psk pass-phrase VoiciUneCleForte_2026! aes
[AP761-wlan-sec-prof-SEC-TRANSITION] security wpa3 sae pass-phrase VoiciUneCleForte_2026! aes
# (syntaxe exacte du mode transition : à valider avec ? selon la version)
```

**Avantages :** zéro exclusion de clients, montée en sécurité progressive.
**Inconvénient honnête :** la sécurité globale reste celle du maillon faible — un attaquant forcera la négociation vers WPA2 s'il le peut (downgrade). Le mode transition est une **étape**, pas un état final : objectif = WPA3-only quand le parc le permet.

**Plan de migration type (6 mois) :**
1. Mois 1–2 : inventaire du parc (quels clients supportent WPA3 ? — la console cloud ou `display wlan sta` donne les capacités).
2. Mois 3 : bascule du SSID en **transition**, communication aux utilisateurs.
3. Mois 4–5 : remplacement/mise à jour des clients récalcitrants.
4. Mois 6 : bascule en **WPA3-only**, vérification qu'il ne reste aucun client WPA2.

## 48. 802.1X / WPA2-Enterprise avec RADIUS

Pour le SSID **salariés**, le 802.1X est le niveau professionnel : chaque utilisateur s'authentifie avec **son** identifiant (login/mot de passe via PEAP, ou mieux : certificat via EAP-TLS), et reçoit des **clés de chiffrement uniques**. Un départ = on désactive le compte, pas besoin de changer la clé de tout le monde.

**Architecture :**
```
Client ---(EAP/802.1X)--- AP761 ---(RADIUS)--- Serveur RADIUS (NPS, FreeRADIUS...)
                                              └── Annuaire (AD/LDAP)
```

**Configuration principe côté AP (Fat) :**
```
[AP761] radius-server template RAD-SRV
[AP761-radius-RAD-SRV] radius-server authentication 192.168.10.50 1812
[AP761-radius-RAD-SRV] radius-server accounting 192.168.10.50 1813
[AP761-radius-RAD-SRV] radius-server shared-key cipher CleRadiusForte_2026!
[AP761-radius-RAD-SRV] quit
[AP761-wlan-view] security-profile name SEC-ENT
[AP761-wlan-sec-prof-SEC-ENT] security wpa2 dot1x aes
# (attacher le template RADIUS au profil AAA — syntaxe à valider selon version)
```

**Choix EAP :**
| Méthode | Sécurité | Contrainte |
|---|---|---|
| PEAP-MSCHAPv2 | Bonne (tunnel TLS + login/mdp) | Mots de passe à gérer ; attaquable si le client ne vérifie pas le certificat serveur |
| EAP-TLS | Excellente (certificats mutuels) | PKI à déployer (mais tu as un guide CA interne — voir ton guide Debian) |
| EAP-TTLS | Bonne | Moins supportée côté clients |

**Point critique : le certificat du serveur RADIUS.** Les clients doivent **vérifier** le certificat du serveur (sinon : attaque par faux RADIUS / evil twin + credential harvesting). Déployer le certificat via GPO/MDM, et **former les utilisateurs à ne jamais accepter un certificat inconnu**.

## 49. Portail captif (Portal) pour invités

Le portail captif : le visiteur se connecte à `INVITE` (souvent en ouvert ou PSK simple), ouvre son navigateur, et tombe sur une **page d'authentification** (CGU à accepter, code reçu à l'accueil, identifiants temporaires).

**Ce que le portail fait :** de l'**authentification et du traçage** (qui était connecté quand — obligation légale en France pour un accès public : conservation des données de connexion).
**Ce que le portail ne fait PAS :** du **chiffrement**. Entre le client et l'AP, en portail ouvert, le trafic est en clair. D'où les mesures complémentaires obligatoires :

| Mesure | Pourquoi |
|---|---|
| **Isolation client** (chap. 53) | Un invité ne doit pas voir le PC de l'autre invité |
| **VLAN dédié** avec ACL « Internet seul » (chap. 52) | Un invité ne doit pas toucher le réseau interne |
| **HTTPS forcé côté services** | Les utilisateurs doivent privilégier les sites en HTTPS |
| **Bande passante limitée** par client | Un invité ne doit pas saturer le lien (QoS, chap. 54) |

**Problèmes classiques (cas 12) :** la page du portail ne s'affiche pas parce que le client a un **DNS personnalisé**, un **VPN actif**, ou un navigateur qui bloque la détection (CNA). Prévoir une **page d'aide** affichée à l'accueil avec l'URL directe du portail et la procédure.

**Obligation légale (France) :** un accès Wi-Fi public doit permettre l'identification des utilisateurs (LCEN) — le portail avec enregistrement (email, SMS, ticket) répond à ce besoin. **Conserver les journaux** selon les délais légaux — à valider avec ton service juridique.

## 50. Filtrage MAC : utile ou placebo ?

Le filtrage MAC (whitelist des adresses MAC autorisées) est supporté par l'AP761 [constructeur]. Verdict honnête : **c'est un placebo de sécurité**, pas une protection.

**Pourquoi ça ne protège pas :**
- Une adresse MAC se **capture en clair** dans l'air (même en WPA2) et se **spoofe en 10 secondes** sur n'importe quel OS.
- La gestion d'une whitelist à la main est un enfer administratif (chaque nouveau téléphone = un ticket).

**Où ça reste utile :**
- **SSID IoT** : limiter les associations à la liste des caméras/capteurs connus — pas comme sécurité, mais comme **garde-fou** (éviter qu'un visiteur ne se connecte au SSID IoT par erreur) et comme **inventaire**.
- **Détection** : une alerte « MAC inconnue sur SSID IoT » est un signal d'investigation, pas une barrière.

**Ne jamais** présenter le filtrage MAC comme une mesure de sécurité dans un dossier ou un audit : un auditeur sérieux le recalera.

## 51. 802.11w (PMF) : protection des trames de management

**PMF (Protected Management Frames, 802.11w)** [constructeur] : chiffre/signe les trames de management (désauthentification, désassociation). Sans PMF, un attaquant peut **éjecter n'importe quel client** en forgeant une trame de désauthentification — c'est l'attaque la plus simple du Wi-Fi.

| Mode PMF | Effet |
|---|---|
| Désactivé | Vulnérable aux attaques de désauthentification |
| **Optionnel** (recommandé en transition) | Les clients compatibles en profitent, les autres se connectent quand même |
| Obligatoire | Sécurité max, mais **exclut les clients non-PMF** (beaucoup d'IoT) |

**Recommandation :**
- SSID salariés (parc moderne) : **PMF obligatoire** si le parc le supporte, sinon optionnel.
- SSID IoT : **désactivé ou optionnel** (les caméras/capteurs sont souvent non-PMF).
- **WPA3 impose PMF** : en WPA3-only ou transition, le PMF est de fait requis pour les clients SAE.

**En CLI (principe) :**
```
[AP761-wlan-sec-prof-SEC-CORP] pmf optional
# ou : pmf mandatory / pmf disable
```

## 52. VLAN : segmentation par SSID

**Supporté** : 802.1Q, **VLAN par SSID** [constructeur]. C'est la base de la segmentation : chaque SSID = un VLAN = un sous-réseau = une politique.

**Schéma type :**
```
SSID ENTREPRISE ──► VLAN 20 ──► 192.168.20.0/24 ──► accès LAN + Internet
SSID INVITE     ──► VLAN 30 ──► 192.168.30.0/24 ──► Internet seul (ACL)
SSID IOT        ──► VLAN 40 ──► 192.168.40.0/24 ──► serveurs IoT seuls (ACL)
Management      ──► VLAN 10 ──► 192.168.10.0/24 ──► AP, switch, supervision
```

**Configuration :** le VAP attache le VLAN (`service-vlan vlan-id 20` — chap. 34), et le port de l'AP côté switch est en **trunk** avec le VLAN 10 en natif (PVID) :

```
# Côté switch (exemple générique) :
# port de l'AP : trunk, PVID 10, allowed 10, 20, 30, 40
```

**Règles de firewall inter-VLAN (à mettre sur le routeur/pare-feu) :**
| Flux | Règle |
|---|---|
| INVITE → LAN | ❌ Refusé |
| INVITE → Internet | ✅ Autorisé |
| INVITE → INVITE (inter-clients) | ❌ Refusé (isolation, chap. 53) |
| IOT → Internet | ❌ Refusé (sauf NTP/DNS si besoin) |
| IOT → serveur vidéosurveillance | ✅ Autorisé (ports caméra uniquement) |
| ENTREPRISE → tout | ✅ (selon politique interne) |
| Tout → Management (VLAN 10) | ❌ Refusé sauf postes d'admin |

**Erreur classique :** mettre le management de l'AP dans le même VLAN que les invités « pour simplifier ». Non : le management est la porte d'entrée vers toute l'infrastructure — **VLAN dédié, accès restreint**.

## 53. Isolation client (STA isolation) dans un VLAN

**STA isolation** (isolation intra-VLAN) [constructeur] : les clients d'un même SSID/VLAN **ne peuvent pas communiquer entre eux** — tout passe par la passerelle. Supporté par l'AP761.

**Où l'activer :**
- ✅ **SSID INVITE : toujours.** Deux visiteurs ne doivent jamais se voir (attaques MITM, partage de fichiers malveillant).
- ✅ **SSID IOT : souvent.** Une caméra n'a aucune raison de parler à une autre caméra.
- ❌ **SSID ENTREPRISE : généralement non.** Le partage d'imprimante, le Chromecast de la salle de réunion, les applis pair-à-pair en ont besoin. À activer seulement si la politique l'exige (et prévoir les exceptions).

**Limite :** l'isolation se fait **au niveau de l'AP**. Si deux clients sont sur deux AP différents du même VLAN sans isolation amont (sur le switch), ils se voient quand même. Pour une isolation totale, combiner avec du **Private VLAN** ou des ACL sur le switch — à vérifier sur la fiche du modèle exact.

## 54. QoS / WMM : prioriser la voix et la vidéo

**WMM (Wi-Fi Multimedia)** [constructeur] : le Wi-Fi reprend les 4 classes de priorité 802.11e — voix, vidéo, best effort, background. L'AP761 fait le **mapping et l'ordonnancement par priorité** [constructeur].

| Classe (AC) | Usage | Comportement |
|---|---|---|
| AC_VO (voix) | Téléphonie sur Wi-Fi, interphonie | Accès prioritaire au canal, files séparées |
| AC_VI (vidéo) | Visioconférence, caméras | Prioritaire après la voix |
| AC_BE (best effort) | Navigation, mail | Le tout-venant |
| AC_BK (background) | Mises à jour, sauvegardes | Sacrifié en premier en cas de congestion |

**Ce que le WMM fait vraiment :** il donne **plus de chances d'accéder au canal** aux trames prioritaires (temps d'attente plus court, backoff réduit). Il ne **réserve** pas de bande passante : si 50 clients font de la visio en même temps, le WMM ne créera pas de capacité magique.

**Bonnes pratiques :**
1. **Marquer en amont** : le WMM se base sur les marques DSCP/802.1p des trames. Si ton réseau ne marque pas (téléphones qui marquent EF, switch qui fait confiance), le WMM ne sert à rien. **Vérifier la chaîne de marquage de bout en bout.**
2. **Limiter le débit des classes basses** sur le SSID invité (rate limiting par client — fonction classique, nom à vérifier selon version) : un invité qui fait ses mises à jour Windows ne doit pas écraser la visio du salarié.
3. **CAC (Call Admission Control)** [constructeur] : limiter le nombre d'appels voix simultanés par radio pour garantir la qualité — à activer si tu fais de la téléphonie sur Wi-Fi en extérieur (interphonie de parking, par exemple).

**Ordre de grandeur :** un appel voix G.711 = ~100 kbps avec les en-têtes ; une visio correcte = 1–2 Mbps. Le dimensionnement voix sur Wi-Fi extérieur reste un exercice délicat : prévoir des tests réels (chap. 60).

## 55. ACL et DHCP snooping / DAI / IPSG

L'AP761 embarque des fonctions de **sécurité du plan filaire** [constructeur] : DHCP snooping, DAI (Dynamic ARP Inspection), IPSG (IP Source Guard), ACL. Ce sont des fonctions de switch, présentes sur l'AP pour protéger son port.

| Fonction | Ce qu'elle fait | Quand l'activer |
|---|---|---|
| **DHCP snooping** | Ne laisse passer les réponses DHCP que des serveurs de confiance | Toujours : tue les serveurs DHCP pirates (box 4G d'un visiteur branchée au réseau) |
| **DAI** | Vérifie que les ARP correspondent aux baux DHCP | Avec le snooping : tue l'ARP spoofing |
| **IPSG** | Ne laisse passer que les IP attribuées par DHCP | Réseaux invités/IoT : tue le vol d'IP |
| **ACL** | Filtre L2/L3/L4 sur le port | Complément au firewall pour des règles locales |

**En pratique sur un AP :** ces fonctions protègent surtout le **trafic ponté localement** (forward-mode direct). Si tout ton trafic remonte au contrôleur ou au firewall, c'est là-bas que se joue l'essentiel — mais activer le snooping sur l'AP coûte peu et protège le segment local.

**Attention :** mal configuré (serveur DHCP non déclaré « trusted »), le DHCP snooping **casse tout le DHCP** du segment. **Toujours déclarer les ports/uplinks de confiance en premier, tester, puis activer.**

## 56. Itinérance : 802.11k, 802.11v, 802.11r

L'AP761 supporte **802.11k, 802.11v et 802.11r** [revendeurs — à vérifier sur la fiche du modèle exact pour le détail d'implémentation]. Les trois ont des rôles différents, et il faut les comprendre pour ne pas les confondre :

| Standard | Nom | Rôle | Analogie |
|---|---|---|---|
| **802.11k** | Radio Resource Management | L'AP donne au client la **liste des AP voisins** avec leurs canaux et leur charge | Le GPS qui te montre les stations-service autour |
| **802.11v** | BSS Transition Management | L'AP peut **suggérer** au client de migrer vers un meilleur AP (et le client peut demander conseil) | Le copilote qui dit « sors à la prochaine, c'est mieux » |
| **802.11r** | Fast BSS Transition | **Accélère la ré-authentification** lors du roaming (clés pré-négociées) | Le télépéage au lieu du ticket à chaque barrière |

**Point fondamental : c'est le CLIENT qui décide de roamer.** L'AP propose (k, v), accélère (r), mais un client têtu restera accroché à un AP lointain. D'où l'importance des réglages côté AP : seuils de déconnexion des clients faibles (débits de base, chap. 13), puissances équilibrées (chap. 40).

**802.11r en détail :** sans 802.11r, un roaming en WPA2-Enterprise = ré-authentification complète (EAP + 4-way handshake) = **200–500 ms** de coupure — audible en voix, visible en visio. Avec 802.11r : **< 50 ms** typique. En PSK, le gain est moindre mais réel.

**Prérequis 802.11r :** les AP doivent partager un **domaine de mobilité** (même groupe, même contrôleur/cloud). En mode Fat avec des AP indépendants, le 802.11r est limité — une raison de plus de préférer le **cloud** ou le **Fit** dès qu'il y a plusieurs AP et du roaming.

## 57. Fast roaming en pratique sur l'AP761

**Checklist roaming propre :**
- [ ] Même **SSID**, même **sécurité**, même **mot de passe** (ou même RADIUS) sur tous les AP.
- [ ] **802.11r activé** sur le profil de sécurité (tester avec le parc réel — certains vieux clients bugguent avec r, voir cas 6).
- [ ] **802.11k/v activés** : aide les clients modernes à choisir vite et bien.
- [ ] Recouvrement de couverture **15–20 %** à −67 dBm entre AP voisins (chap. 40).
- [ ] Canaux **différents** entre AP voisins (chap. 37–38) : un client qui roame change aussi de canal, c'est normal.
- [ ] **VLAN cohérents** : le même SSID doit donner le même VLAN sur tous les AP, sinon le client change de sous-réseau en roamant = sessions coupées.

**Tester le roaming (méthode terrain) :**
1. Connecter un smartphone au SSID, lancer un **ping continu** vers la passerelle (`ping -t`).
2. Marcher lentement d'un AP à l'autre en regardant l'écran.
3. Noter : à quel RSSI le client lâche l'AP1 ? Combien de pings perdus pendant la bascule ? (< 5 = bon, > 20 = problème).
4. Refaire avec un appel voix ou une visio : c'est le test qui compte vraiment.

**Seuils indicatifs :** un client bien réglé roame vers −70 dBm. S'il reste accroché à −80 dBm, c'est soit qu'il ne voit pas mieux (trou de couverture), soit qu'il est têtu (désactiver le Wi-Fi/réactiver pour forcer, ou ajuster les débits de base pour le « pousser » dehors).

## 58. Planification : combien d'AP pour quelle surface

Deux logiques de dimensionnement, à combiner :

**A. Dimensionnement par COUVERTURE (le signal) :**
Un AP761 couvre un secteur de 65° (chap. 9). En pratique :
| Environnement | Portée utile 5 GHz (indicative) | Portée utile 2.4 GHz (indicative) |
|---|---|---|
| Vue directe, peu d'obstacles | 80–150 m | 150–250 m |
| Cour avec arbres, mobilier | 40–80 m | 80–150 m |
| Zone avec bâtiments intermédiaires | 20–40 m | 40–80 m |

> Ce sont des ordres de grandeur terrain, pas des valeurs constructeur. Le seul chiffre constructeur approchant est la « portée 500 m » citée par un revendeur (**à vérifier sur la fiche du modèle exact** — c'est probablement une portée max en conditions idéales, pas une portée utile).

**B. Dimensionnement par CAPACITÉ (le débit) :**
L'AP761 accepte **1024 clients max** (512/radio) [constructeur], mais c'est un maximum d'association, pas de confort. La vraie limite, c'est l'**airtime** :
| Usage | Clients confortables par AP (ordre de grandeur) |
|---|---|
| Navigation légère, IoT | 100–200 |
| Usage bureautique / visio occasionnelle | 40–80 |
| Visio dense, voix sur Wi-Fi | 20–40 |

**Règle :** on dimensionne sur le **plus contraignant** des deux. Une cour de 200 personnes en visio = 4–5 AP pour la capacité, même si 2 AP suffiraient pour la couverture.

## 59. Règles de pouce densité / débit par usage

Le tableau à sortir en réunion quand on te demande « ça va tenir ? » :

| Usage | Débit par client (utile) | Clients/AP | Largeur 5 GHz | Notes |
|---|---|---|---|---|
| Capteurs IoT | < 1 Mbps | 150+ | 20 MHz | 2.4 GHz suffit souvent |
| Navigation visiteurs (terrasse) | 2–5 Mbps | 80–120 | 40 MHz | Pics le midi/week-end |
| Bureautique extérieure | 5–10 Mbps | 40–60 | 40 MHz | Visio incluse |
| Visioconférence dense | 2–5 Mbps/client mais latence critique | 20–30 | 40 MHz | WMM + CAC (chap. 54) |
| Vidéosurveillance (caméras Wi-Fi) | 4–8 Mbps/caméra montant | 10–15 caméras | 40/80 MHz | Le montant est le goulot — tester ! |
| Événementiel (fête, marché) | 1–3 Mbps | 150–200 | 40 MHz | Prévoir +50 % de marge |

**Calcul de capacité simplifié :**
```
Capacité utile d'un AP761 ≈ 300–500 Mbps agrégés (les deux radios, conditions correctes)
Clients supportables = Capacité utile / débit par client
Exemple : 400 Mbps / 5 Mbps = 80 clients en navigation
```
Toujours garder **30 % de marge** : le Wi-Fi est un médium partagé avec des pics.

**Le cas des caméras Wi-Fi (fréquent en extérieur) :** 10 caméras à 6 Mbps montant = 60 Mbps de trafic **montant** constant. Le Wi-Fi est moins bon en montant qu'en descendant (les clients ont des petites antennes et peu de puissance). **Tester avec le nombre réel de caméras** avant de valider — c'est le cas d'usage qui casse le plus de dimensionnements théoriques.

## 60. Site survey simplifié sans outils pro — méthode terrain

Pas d'analyseur de spectre à 5000 € ? La méthode smartphone suffit pour 90 % des sites PME.

**Matériel :** un smartphone Android + app d'analyse Wi-Fi gratuite (ex. WiFi Analyzer), un mètre, le plan du site imprimé.

**Protocole (à faire AP par AP, une fois installé) :**
1. **Scan passif :** depuis 4–6 points de la zone, noter pour chaque AP : SSID, canal, RSSI, largeur. Repérer les voisins forts.
2. **Mesure de couverture :** marcher en grille (tous les 10–15 m), noter le RSSI du SSID principal sur le plan. Tracer la courbe des **−67 dBm** : c'est ta zone « confort ».
3. **Test de débit :** avec iperf3 vers un serveur du LAN (pas un speedtest Internet — on teste le Wi-Fi, pas la fibre), mesurer TCP descendant/montant à 3 distances (proche, moyen, limite).
4. **Test de roaming :** ping continu en marchant entre AP (chap. 57).
5. **Photo et notes :** chaque point de mesure = une photo du lieu + le relevé. Dans 2 ans, tu comprendras ce que tu as fait.

**Modèle de fiche de mesure (à recopier dans le dossier de site) :**
```
Date : ___  AP : AP761-Cour-Est  Téléphone : ___
Point A (entrée cour) : RSSI -55 dBm, canal 36/80MHz, iperf down 320 Mbps / up 280 Mbps
Point B (milieu cour) : RSSI -64 dBm, iperf down 180 Mbps / up 150 Mbps
Point C (fond cour)   : RSSI -72 dBm, iperf down 60 Mbps / up 40 Mbps  -> limite, prévoir AP2 si usage dense
Voisins forts : Freebox-XYZ canal 1 (-75 dBm), Entreprise-Voisine canal 44 (-70 dBm)
```

**Quand passer au survey pro :** site > 20 AP, exigences voix/vidéo critiques, environnement RF hostile (aéroport, hôpital, industrie lourde). Là, un vrai survey prédictif + validation active se justifie.

## 61. Lecture d'un relevé : RSSI, SNR, bruit

Les trois chiffres qui disent tout :

| Indicateur | Bon | Limite | Mauvais |
|---|---|---|---|
| **RSSI** (signal reçu) | > −60 dBm | −60 à −70 dBm | < −70 dBm (données), < −67 dBm (voix) |
| **Bruit** (noise floor) | < −90 dBm | −85 à −90 dBm | > −85 dBm (environnement bruyant) |
| **SNR** (RSSI − bruit) | > 25 dB | 20–25 dB | < 20 dB |

**Traductions terrain :**
- RSSI −55 dBm, bruit −95 dBm → SNR 40 dB : **excellent**, 1024-QAM possible à courte distance.
- RSSI −70 dBm, bruit −95 dBm → SNR 25 dB : **correct**, 256-QAM, débits honorables.
- RSSI −70 dBm, bruit −85 dBm → SNR 15 dB : **mauvais** malgré un RSSI « acceptable » — le bruit tue. Chercher la source (chap. 62, cas 7).
- **RSSI qui fluctue de ±10 dB** au même endroit : trajets multiples (réflexions), végétation qui bouge, ou interférence intermittente — à investiguer.

**L'erreur classique :** ne regarder que le RSSI. Un client à −65 dBm dans un bruit à −80 dBm (SNR 15 dB) aura de pires débits qu'un client à −72 dBm dans un bruit à −95 dBm (SNR 23 dB). **Le SNR gouverne, pas le RSSI seul.**

## 62. Erreurs classiques de placement d'AP extérieur

La galerie des horreurs, vue sur le terrain. À vérifier sur chaque chantier :

1. **L'AP derrière un obstacle métallique** (auvent, bardage, armoire) : le métal fait cage de Faraday partielle — le secteur est haché.
2. **L'AP trop haut** (« pour voir loin ») : zone morte au pied du mât + asymétrie avec les clients (chap. 40).
3. **L'AP collé à un mur** : la moitié du secteur part dans le mur, l'autre moitié est déformée par réflexion.
4. **Deux AP dos à dos sur le même mât, même canal** : ils se partagent l'airtime au lieu de se compléter.
5. **L'AP à côté du transformateur / du variateur** : bruit électromagnétique large bande.
6. **L'AP sous un arbre** : en été avec les feuilles mouillées, le 5 GHz perd 10–20 dB. Et ça bouge avec le vent (fluctuations).
7. **Le câble qui pend** : prise au vent, usure par frottement, eau qui suit le câble.
8. **L'AP orienté vers le parking… des voisins** : on couvre la zone utile, pas la plus facile d'accès.
9. **Oublier l'hiver** : un AP posé en été derrière des arbres feuillus se retrouve à découvert en hiver (ou l'inverse : la végétation estivale n'existait pas au survey d'hiver).
10. **Pas de repérage photo** : dans 3 ans, personne ne sait quel AP est où — étiquetage + plan à jour (chap. 101).

## 63. WIDS/wIPS : détection d'intrusion sans fil

**WIDS** (détection) / **wIPS** (prévention) [constructeur] : l'AP761 surveille l'air et détecte les comportements suspects. C'est le « vigile » du Wi-Fi.

**Ce que le WIDS détecte typiquement :**
| Attaque / anomalie | Principe |
|---|---|
| **Rogue AP** | Un AP non autorisé branché sur ton réseau filaire (chap. 64) |
| **Evil twin** | Un AP pirate qui usurpe ton SSID pour intercepter les clients |
| **Désauthentification massive** | Flood de trames de désauthentification (attaque DoS) |
| **Flood de sondes / d'associations** | Tentative de saturation de l'AP |
| **Ad-hoc / Wi-Fi Direct suspects** | Réseaux pair-à-pair qui contournent l'infrastructure |
| **Clients en mode promiscuous** | Sniffing potentiel |

**wIPS vs WIDS :** le WIDS **alerte**, le wIPS **contre-attaque** (désauthentification du rogue, par exemple). La contre-attaque est à manier avec précaution : désauthentifier un AP voisin légitime parce qu'on l'a mal classifié = incident avec le voisin + possible non-conformité (brouillage actif). **Recommandation : WIDS en alerte d'abord, wIPS en contre-mesure seulement sur les rogues confirmés branchés sur ton filaire** (chap. 64).

**Prérequis :** le WIDS consomme du temps radio (l'AP doit écouter hors de son canal). Sur un AP chargé, activer le WIDS peut coûter quelques % de capacité — acceptable sur l'AP761 en usage PME, à surveiller sur les sites denses.

## 64. Rogue AP : détecter, classifier, contenir

**Définition :** un **rogue AP** = un point d'accès non autorisé. Deux cas très différents :

| Type | Exemple | Danger | Action |
|---|---|---|---|
| **Rogue « naïf »** | Un salarié branche sa box 4G/routeur perso « pour avoir du Wi-Fi dans son bureau » | Il crée une porte d'entrée non contrôlée vers ton LAN | Le trouver (il est sur ton filaire), le débrancher, sensibiliser |
| **Rogue « malveillant »** | Evil twin qui usurpe ton SSID sur le parking | Interception d'identifiants, MITM | Contenir (désauth), alerter, enquête |

**Méthode de classification (à faire avant toute contre-mesure) :**
1. Le WIDS signale un AP inconnu avec un SSID proche du tien ou branché sur ton réseau.
2. **Vérifier s'il est sur ton filaire :** chercher son adresse MAC dans les tables MAC de tes switches. Si oui = rogue naïf (ou pire) **interne** → intervention physique.
3. **S'il n'est pas sur ton filaire :** c'est un voisin (légitime) ou un attaquant externe. Observer : usurpe-t-il ton SSID ? Émet-il fort près de tes locaux ?
4. **Ne jamais contre-attaquer un AP voisin légitime** : le risque juridique et relationnel dépasse le bénéfice.

**Procédure terrain « rogue naïf » (le cas à 90 %) :**
1. Identifier le port du switch où apparaît sa MAC (table MAC du switch).
2. Remonter le brassage jusqu'à la prise murale / la zone.
3. Aller voir : 9 fois sur 10, c'est un boîtier personnel branché « pour dépanner ».
4. Débrancher, expliquer, noter l'incident.
5. **Traiter la cause :** pourquoi le salarié a branché ça ? (Zone non couverte ? Procédure trop lente pour avoir du Wi-Fi ?) — un rogue naïf est souvent un **signal de besoin non satisfait**.

## 65. Analyse de spectre (mode Fit uniquement)

**Fait constructeur important :** sur l'AP761, l'**analyse de spectre** (identification des sources d'interférences : Bluetooth, micro-ondes, téléphones sans fil, ZigBee, vidéo/audio sans fil, babyphones) n'est supportée qu'en **mode Fit** [datasheet]. En Fat ou Cloud, tu n'y as pas accès.

**Ce que ça apporte :** au lieu de voir « le canal est bruyant », tu vois « c'est un four à micro-ondes » ou « c'est du Bluetooth ». Ça change le diagnostic : on ne change pas de canal contre un micro-ondes (il pollue large), on **déplace l'AP** ou on **blinde la source**.

**Si tu es en Cloud/Fat et que tu soupçonnes une interférence non-Wi-Fi :**
1. Méthode pauvre : éteindre successivement les suspects (le micro-ondes du food-truck, la caméra sans fil du voisin) et observer le bruit.
2. Méthode smartphone : certaines apps montrent la « qualité » du canal au-delà des réseaux Wi-Fi visibles.
3. Méthode pro : louer/emprunter un analyseur de spectre portable pour une journée (rentable sur un gros site qui rame).
4. **Basculer temporairement un AP en Fit** vers un AC de test si tu en as un — lourd, mais c'est la seule analyse de spectre native.

**Sources d'interférences typiques en extérieur (mémo) :** fours à micro-ondes, caméras vidéo sans fil 2.4 GHz, Bluetooth (enceintes, kits), ZigBee (domotique), téléphones DECT (normalement 1.9 GHz en Europe — vérifier si modèle exotique), drones FPV en 5.8 GHz (!), liaisons vidéo de sécurité.

## 66. Supervision via la plateforme cloud eKit

En mode cloud, la supervision est **gratuite et intégrée** : c'est l'argument n° 1 du cloud pour un chef de service multi-sites.

**Ce que la console cloud montre (selon version — à vérifier sur la fiche du modèle exact) :**
- **État des AP :** en ligne/hors ligne, uptime, version firmware, utilisation CPU/mémoire.
- **Clients :** nombre par AP/SSID, débits, RSSI, historique.
- **Radio :** canal utilisé, largeur, puissance, taux d'utilisation (channel utilization).
- **Alertes :** AP hors ligne, rogue détecté, interférences, seuil de clients dépassé.
- **Cartographie :** position des AP sur plan (si renseignée).

**Organisation multi-sites :**
```
Compte principal (toi)
├── Site « Siège » (3 AP)
├── Site « Usine Nord » (8 AP)
└── Site « Dépôt Sud » (2 AP)
Techniciens : accès par site, pas au compte global.
```

**Les alertes à configurer en priorité :**
1. **AP hors ligne** (le plus critique — souvent = coupure PoE ou câble arraché).
2. **Rogue AP détecté** (chap. 64).
3. **Utilisation canal > 70 %** prolongée (signe de saturation ou d'interférence).
4. **Firmware obsolète** (rappel de mise à jour).

**Limite honnête :** la supervision cloud dépend d'Internet et des serveurs Huawei. Pour les sites critiques, **doubler avec du SNMP local** (chap. 68) : en cas de coupure Internet, le cloud est aveugle mais ton superviseur local voit toujours.

## 67. Indicateurs Wi-Fi à surveiller (tableau)

Le tableau de bord minimal d'un parc Wi-Fi extérieur. Relever **une fois par semaine** en routine, **en continu** sur les sites critiques.

| Indicateur | Où le voir | Seuil d'alerte | Ce que ça veut dire si dépassé |
|---|---|---|---|
| AP hors ligne | Cloud / SNMP | > 0 | PoE, câble, AP mort (cas 1) |
| Channel utilization 5 GHz | Cloud / CLI | > 70 % prolongé | Saturation ou interférence (cas 4, 7) |
| Channel utilization 2.4 GHz | Cloud / CLI | > 60 % prolongé | Le 2.4 GHz sature vite — migrer vers 5 GHz |
| Clients par AP | Cloud / CLI | > 80 % du dimensionnement | Ajouter un AP ou rééquilibrer (chap. 58) |
| RSSI moyen des clients | Cloud | < −70 dBm | Couverture limite ou clients trop loin |
| Taux de réessais (retries) | CLI / SNMP | > 15–20 % | Interférences ou clients faibles |
| CRC errors filaire | Switch / CLI | > 0 croissant | Câble/connecteur défectueux (chap. 24) |
| CPU / mémoire AP | Cloud / CLI | > 80 % prolongé | Surcharge (trop de clients/fonctions) |
| Uptime | Cloud / SNMP | Redémarrage inattendu | Surchauffe, PoE instable (cas 14, cas 1) |
| Rogues détectés | WIDS / Cloud | > 0 non classifié | À classifier (chap. 64) |

**Rituel hebdo (15 min) :** ouvrir la console, vérifier les AP hors ligne, les alertes, un coup d'œil aux utilizations canal. **Rituel mensuel (1 h) :** tendances (la cour se remplit-elle ? un canal se dégrade-t-il ?), revue des firmwares, mise à jour du dossier de site.

## 68. SNMP : v1/v2c/v3 en mode Fat

**Supporté en mode Fat : SNMP v1/v2c/v3** [constructeur]. C'est la porte d'entrée vers ton superviseur (Zabbix, PRTG, Centreon — tu as un guide Zabbix : `~/workspace/user/files/zabbix_guide.md`).

**Configuration (principe CLI) :**
```
[AP761] snmp-agent
[AP761] snmp-agent sys-info version v3
[AP761] snmp-agent group v3 GRP-ADMIN privacy
[AP761] snmp-agent usm-user v3 USER-SUP auth-mode sha CleAuthForte_2026! priv-mode aes128 ClePrivForte_2026!
[AP761] snmp-agent group v3 GRP-ADMIN acl 2000
# (ACL 2000 = n'autoriser que le superviseur — à créer)
```

**Règles :**
- **Toujours SNMPv3** (auth + priv) : le v1/v2c envoie la community **en clair** — sur un réseau extérieur, c'est une faute.
- **Restreindre par ACL** : seul le superviseur interroge l'AP.
- **Ce qu'on supervise :** uptime, état des interfaces, clients associés, utilisation CPU/mémoire, traps (AP down, rogue détecté).
- Les **OID spécifiques** (clients Wi-Fi, stats radio) dépendent de la MIB Huawei : charger la MIB du firmware **exact** dans le superviseur — à vérifier sur la fiche du modèle exact.

**Traps vs polling :** le polling (toutes les 1–5 min) pour les tendances, les **traps** pour l'immédiat (AP down). Configurer les deux : un AP qui ne répond plus au polling ET n'envoie plus de traps = vraiment mort, pas juste un trou SNMP.

## 69. Syslog : centraliser les journaux

Les logs de l'AP sont la **boîte noire** : en dépannage, c'est souvent là que se trouve l'explication (désauthentifications en boucle, changements de canal DFS, pertes CAPWAP).

**Configuration (principe CLI) :**
```
[AP761] info-center enable
[AP761] info-center loghost 192.168.10.100 facility local7
[AP761] info-center source default channel loghost log level informational
```

**Bonnes pratiques :**
- Envoyer vers un **serveur syslog central** (celui de ton SI — voir ton guide Loki/Syslog si applicable), pas en local seul : si l'AP meurt, ses logs meurent avec lui.
- Niveau **informational** en routine, **debugging** seulement en dépannage ciblé (ça inonde).
- **Horodatage NTP obligatoire** (chap. 70) : des logs sans heure fiable sont inutilisables pour corréler avec d'autres équipements.
- **Rétention :** alignée sur tes obligations (au minimum quelques mois ; accès public = obligations légales, chap. 49).

**Ce qu'on cherche dans les logs :**
- `Deauth` en boucle sur un client → problème d'authentification ou client têtu.
- `Channel change` fréquent → DFS ou interférence (cas 8).
- `CAPWAP` down/up → problème réseau vers l'AC (mode Fit).
- `PoE` / `power` → alim instable (cas 1, cas 9).

## 70. NTP : pourquoi c'est critique

Un AP sans heure juste, c'est :
- Des **logs incohérents** (impossible de corréler avec le switch ou le firewall).
- Des **échecs TLS** vers le cloud (certificat « pas encore valide » ou « expiré ») → l'AP ne s'adopte pas (cas 2).
- Des **problèmes 802.1X** (Kerberos et les certificats détestent le décalage horaire).
- Des stats de supervision fausses.

**Configuration :**
```
[AP761] ntp-service unicast-server 192.168.10.1
# Idéalement 2 serveurs pour la redondance :
[AP761] ntp-service unicast-server 192.168.10.2
<AP761> display ntp-service status   # vérifier « synchronized »
```

**Architecture :** tes AP synchronisent sur **ton** serveur NTP interne (qui lui-même se synchronise sur des sources externes), pas directement sur Internet — moins de flux sortants, plus de contrôle. Si le site est isolé sans NTP interne, autoriser au moins un serveur public via le firewall.

**Vérification :** après chaque (re)démarrage, contrôler que l'AP est synchronisé avant de diagnostiquer autre chose. **Un AP qui vient de redémarrer avec une horloge à 1970 et qui refuse le cloud = NTP d'abord, diagnostic après.**

## 71. Alertes et seuils : que configurer

Ni trop ni trop peu : trop d'alertes = on ne les lit plus ; pas assez = on découvre les pannes par les utilisateurs. La config minimale viable :

| Alerte | Seuil | Canal | Priorité |
|---|---|---|---|
| AP hors ligne | Immédiat | SMS/app + mail | 🔴 Critique |
| Rogue AP non classifié | Immédiat | Mail | 🟠 Haute |
| Utilisation canal 5 GHz > 75 % | Pendant 30 min | Mail | 🟡 Moyenne |
| Client avec RSSI < −80 dBm nombreux | Tendance hebdo | Rapport | 🟡 Moyenne |
| Firmware obsolète | Mensuel | Mail | 🟢 Basse |
| Certificat RADIUS < 30 jours d'expiration | J-30, J-7 | Mail + SMS | 🔴 Critique (si 802.1X) |
| Espace flash AP < 20 % | Mensuel | Mail | 🟢 Basse |

**Règles d'or des alertes :**
1. **Chaque alerte doit déclencher une action connue.** « AP hors ligne → appeler l'astreinte, vérifier PoE » pas « AP hors ligne → 🤷 ».
2. **Tester les alertes** : débrancher volontairement un AP un jour calme et vérifier que l'alerte arrive. Une alerte jamais testée est une illusion.
3. **Astreinte :** qui reçoit quoi, quand. Le SMS critique à 3 h du matin pour un AP de terrasse de camping en hiver = à discuter (criticité par site).
4. **Éviter les flaps :** seuils avec hystérésis (alerter à 75 %, « rétabli » à 60 %) pour ne pas spammer.

## 72. Sauvegarde de configuration : méthodes

Trois méthodes complémentaires, pas concurrentes :

**1. Sauvegarde locale (flash de l'AP) — le minimum :**
```
<AP761> save
<AP761> display saved-configuration   # vérifier que c'est bien écrit
```
Protège contre le redémarrage, pas contre la foudre ni le vol.

**2. Export vers un serveur — la vraie sauvegarde :**
```
# Via SFTP depuis un poste d'administration :
sftp admin@192.168.10.11
sftp> get vrpcfg.zip /sauvegardes/AP761-Cour-Est_2026-09-27.zip
```
Automatiser : un script hebdomadaire qui aspire les configs de tous les AP (clef SSH dédiée, compte en lecture seule si possible).

**3. Version cloud — le filet de sécurité :**
En mode cloud, la config est dans la plateforme. **Exporter une copie** après chaque changement majeur + noter la version firmware. Le cloud n'est pas une sauvegarde si tu n'as plus accès au compte (départ du prestataire, litige) : **l'export local reste obligatoire**.

**Ce qu'on sauvegarde, au-delà de la config :**
- [ ] Fichier de config (`vrpcfg.zip` ou équivalent).
- [ ] Version firmware exacte (`display version`).
- [ ] Plan d'adressage, VLAN, SSID, clés (dans un coffre, pas dans le même fichier que la config en clair).
- [ ] Photos du montage, plan de canaux, relevés de survey.
- [ ] Dossier de site à jour (qui, quoi, où, quand).

**Test de restauration annuel** (chap. 73) : le seul moyen de savoir que ça marche.

## 73. Restauration de configuration

**Scénario :** l'AP761-Cour-Est a pris la foudre. Le remplaçant est sur l'établi. Objectif : 30 minutes chrono.

**Procédure :**
1. Noter la **MAC et le S/N** du nouvel AP (photo de l'étiquette).
2. Monter le nouvel AP (même fixation, même orientation — les photos du dossier aident).
3. L'alimenter en PoE, attendre le démarrage.
4. **Mode Fat :** injecter la config sauvegardée :
```
# Via SFTP : déposer la sauvegarde, puis :
<AP761> restore configuration from vrpcfg-sauvegarde.zip  # (syntaxe à valider selon version)
# ou copier-coller les blocs de la config en mode system-view, section par section
<AP761> reboot
```
5. **Mode Cloud :** adopter le nouvel AP dans le site (QR code), lui donner le **même nom** (`AP761-Cour-Est`), la config descend toute seule.
6. **Mode Fit :** déclarer la MAC du nouvel AP sur l'AC (ou laisser l'auto-découverte si configurée).
7. Vérifier : SSID diffusés, client test, supervision verte.
8. Mettre à jour le dossier de site (nouveau S/N, date de remplacement).

**Pièges :**
- La sauvegarde contient parfois l'**ancienne adresse MAC** ou des bindings : vérifier après restauration.
- **Version firmware différente** entre la sauvegarde et le nouvel AP : restaurer d'abord le même firmware (chap. 75), puis la config.
- En cloud, ne pas oublier de **retirer l'ancien AP** du site (sinon : AP fantôme « hors ligne » qui pollue les alertes).

## 74. Firmware : cycle de vie et bonnes pratiques

Le firmware d'un AP extérieur n'est pas un gadget : il corrige des failles, ajoute des fonctions radio, et parfois **change des comportements** (d'où le rollback, chap. 76).

**Politique de mise à jour recommandée :**
| Fréquence | Quoi | Comment |
|---|---|---|
| **À chaque faille critique** | Patch de sécurité | Sous 30 jours, après test |
| **2× par an** | Version stable (patch releases) | Fenêtre planifiée, un site pilote d'abord |
| **Jamais en automatique aveugle** | — | Toujours valider sur un AP test ou un site non critique |

**Règles :**
1. **Lire la release note** avant : qu'est-ce qui change ? Qu'est-ce qui est corrigé ? Y a-t-il des prérequis (bootloader, version intermédiaire) ?
2. **Un site pilote d'abord** : mettre à jour UN AP (ou un petit site), attendre 1–2 semaines, puis généraliser.
3. **Fenêtre de maintenance** : jamais en pleine journée d'activité. L'AP redémarre (coupure 2–5 min par AP).
4. **Sauvegarder avant** (chap. 72) : config + version actuelle notée.
5. **Ne pas sauter plusieurs versions majeures** d'un coup : certaines migrations exigent des paliers — à vérifier sur la fiche du modèle exact et la release note.

**Où trouver les firmwares :** portail support Huawei / plateforme cloud eKit (qui propose les versions validées). **Ne jamais** flasher un firmware trouvé sur un forum : risque de version trafiquée ou de variante régionale incompatible.

## 75. Mise à jour firmware — procédure pas à pas

**En mode Cloud (le plus simple) :**
1. Console cloud → site → AP → « Mise à jour firmware ».
2. Choisir la version (la console propose les versions validées).
3. Planifier : immédiate (site pilote) ou programmée (nuit).
4. Lancer, **surveiller** : l'AP télécharge, flashe, redémarre, se ré-adopte.
5. Vérifier : version (`display version` ou console), SSID diffusés, clients qui reviennent, logs propres.

**En mode Fat (CLI) :**
```
# 1. Déposer le firmware sur l'AP (SFTP) :
sftp admin@192.168.10.11
sftp> put AP761-V200Rxxx.cc /

# 2. Désigner le firmware de démarrage :
<AP761> startup system-software AP761-V200Rxxx.cc

# 3. Vérifier :
<AP761> display startup
# -> le nouveau firmware doit être « startup system software »

# 4. Sauvegarder la config, puis redémarrer :
<AP761> save
<AP761> reboot
# -> confirmer, attendre 3-5 min, vérifier la version :
<AP761> display version
```

**Checklist post-mise à jour :**
- [ ] Version correcte affichée.
- [ ] Tous les SSID diffusés (vérifier les deux radios).
- [ ] Clients tests connectés sur chaque SSID.
- [ ] Supervision verte (SNMP/cloud).
- [ ] Logs sans erreurs nouvelles (10 min d'observation).
- [ ] **Noter dans le dossier de site** : ancienne → nouvelle version, date, opérateur.

## 76. Rollback : revenir en arrière proprement

**Quand faire un rollback :** régression avérée après mise à jour (clients qui ne s'associent plus, débit effondré, redémarrages en boucle — cas 16). Pas sur une simple impression : **mesurer d'abord** (comparer les indicateurs chap. 67 avant/après).

**Procédure (Fat AP) :**
```
# L'ancien firmware est normalement conservé en backup :
<AP761> display startup
# -> repérer l'ancien firmware (ex. AP761-V200Ryyy.cc)

<AP761> startup system-software AP761-V200Ryyy.cc
<AP761> save
<AP761> reboot
# -> vérifier le retour à l'ancienne version
```

**En mode Cloud :** la console permet généralement de re-flasher une version antérieure (fonction « rollback » ou réinstallation d'une version — à vérifier sur la fiche du modèle exact).

**Règles du rollback :**
1. **Ne pas paniquer :** un rollback précipité sans diagnostic fait perdre l'information. Noter les symptômes AVANT.
2. **La config a peut-être migré** : si le nouveau firmware a converti la config dans un nouveau format, l'ancien firmware peut la refuser partiellement → **restaurer la sauvegarde pré-mise à jour** (chap. 73) après le rollback.
3. **Signaler** : remonter la régression au support / au distributeur avec les logs — ça sert à tout le monde.
4. **Geler** les mises à jour du reste du parc tant que la cause n'est pas comprise.

**Prévention :** garder **toujours** l'avant-dernière version stable sous la main (fichier + procédure testée). Le jour où tu en as besoin, tu n'auras pas le temps de la chercher.

## 77. Dépannage : méthode générale (les 5 couches)

Avant les cas particuliers, LA méthode. 90 % des tickets Wi-Fi se résolvent en suivant cet ordre, sans sauter d'étape :

**Couche 1 — Physique :** L'AP est-il alimenté ? LED ? Le câble est-il bon ? Le PoE arrive-t-il ? Le switch voit-il le lien ?
**Couche 2 — Réseau filaire :** L'AP a-t-il une IP ? Ping OK ? Le VLAN est-il bon ? Le DHCP répond-il ? Le trunk laisse-t-il passer les VLAN ?
**Couche 3 — Radio :** Les SSID sont-ils diffusés ? Le canal est-il correct ? La puissance est-elle suffisante ? Y a-t-il de l'interférence ?
**Couche 4 — Association :** Le client s'associe-t-il ? L'authentification passe-t-elle (PSK ? 802.1X ? Portail ?) ? Obtient-il une IP ?
**Couche 5 — Service :** Le client navigue-t-il ? Le débit est-il correct ? Le DNS répond-il ? Internet est-il OK ?

**Les 4 questions à poser à chaque ticket :**
1. **Quoi** exactement ? (« pas de Wi-Fi » → quoi : pas de SSID ? pas d'IP ? lent ?)
2. **Qui** est impacté ? (un client ? tous ? une zone ? un SSID ?)
3. **Quand** ça a commencé ? (toujours ? depuis quand ? à heures fixes ?)
4. **Qu'est-ce qui a changé ?** (travaux ? nouvel AP voisin ? mise à jour ? orage ?)

**Règle d'or :** « qui est impacté » oriente tout. **Un seul client** = problème client (pilote, config). **Tous les clients d'un AP** = problème AP/réseau. **Tous les clients partout** = problème d'infrastructure (DHCP, RADIUS, Internet). Ne jamais reconfigurer l'infrastructure pour un seul client têtu.

## 78. Cas 1 : l'AP ne s'allume pas / pas de PoE

**Symptômes :** LED éteinte, l'AP n'apparaît nulle part, pas de lien sur le switch.

**Diagnostic ordonné :**
1. **Le switch/injecteur fournit-il du PoE ?** Vérifier le voyant PoE du port, la config du port (`display poe` côté switch Huawei), le budget restant (chap. 25). Un budget épuisé = le dernier AP ne démarre pas.
2. **Le câble est-il bon ?** Tester au testeur (continuité + longueur). Un câble trop long (> 100 m) ou en CCA fait chuter la tension : l'AP peut clignoter sans jamais démarrer (brownout).
3. **L'injecteur est-il gigabit et 802.3at ?** Un injecteur 100 Mbps ou 802.3af seul peut empêcher le démarrage complet (fonctions restreintes, voire boot en boucle).
4. **Le parafoudre est-il mort ?** (chap. 26) : le shunter temporairement pour tester. S'il a pris un coup, il coupe tout.
5. **Tester l'AP sur table** : l'amener au local, le brancher en direct sur un injecteur connu bon avec un cordon court. S'il démarre = le problème est le câble/l'infrastructure. S'il ne démarre pas = l'AP est probablement mort (SAV).

**Piège :** un AP qui **démarre puis s'éteint en boucle** = souvent une alimentation limite (tension qui s'effondre quand les radios s'allument et tirent du courant). Mesurer sous charge si possible, ou simplifier : cordon court + injecteur 30 W.

## 79. Cas 2 : l'AP démarre mais n'apparaît pas dans le cloud

**Symptômes :** LED verte, mais l'AP reste « hors ligne » dans la console cloud / l'app.

**Diagnostic ordonné :**
1. **L'AP a-t-il une IP ?** Le trouver via le DHCP ou en BLE avec l'app. Pas d'IP = problème VLAN/DHCP (couche 2, chap. 77).
2. **L'AP sort-il sur Internet ?** Depuis l'AP (si CLI accessible) : `ping 8.8.8.8`, puis `ping` du nom de la plateforme cloud. Pas de sortie = firewall/proxy qui bloque.
3. **Le firewall laisse-t-il passer ?** La plateforme cloud eKit utilise des ports/URL spécifiques (**à vérifier sur la fiche du modèle exact** — typiquement HTTPS 443 vers des domaines Huawei). Un firewall d'entreprise qui ne laisse sortir que le web via proxy = l'AP ne passe pas.
4. **L'heure est-elle juste ?** (chap. 70) : sans NTP, le TLS échoue. `display clock` puis comparer.
5. **Le DNS résout-il ?** `nslookup`/`ping` du domaine cloud depuis l'AP.
6. **L'AP est-il déjà adopté ailleurs ?** Un AP adopté sur un autre compte/site doit être **libéré** avant ré-adoption.

**Test décisif :** brancher l'AP sur un réseau « simple » (box internet + DHCP, sans firewall) : s'il s'adopte = le problème est ton réseau (firewall/VLAN/DNS). Sinon = problème AP/compte.

## 80. Cas 3 : SSID visible mais pas d'IP (DHCP)

**Symptômes :** le client s'associe au Wi-Fi mais reste en 169.254.x.x (APIPA) ou « connecté, pas d'Internet ».

**Diagnostic ordonné :**
1. **Le VLAN du SSID est-il autorisé sur le trunk ?** (`port trunk allow-pass vlan X` côté AP + côté switch — chap. 34/52). C'est la cause n° 1, et de loin.
2. **Le serveur DHCP répond-il sur ce VLAN ?** Tester en filaire sur le même VLAN : si le filaire n'a pas d'IP non plus, le problème n'est pas le Wi-Fi.
3. **Le scope DHCP est-il plein ?** Un scope /24 plein à cause de baux fantômes = plus d'IP pour personne. Vérifier les baux, réduire la durée des baux sur les VLAN invités (1–4 h).
4. **Le relay DHCP (ip-helper) est-il configuré ?** Si le serveur DHCP n'est pas sur le même VLAN, il faut un relais sur la passerelle du VLAN.
5. **Le DHCP snooping bloque-t-il ?** (chap. 55) : port uplink non déclaré « trusted » = réponses DHCP jetées.

**Test décisif :** mettre une IP **statique** sur le client (dans le bon sous-réseau) : si ça marche en statique = c'est bien le DHCP. Sinon = problème VLAN/routage.

## 81. Cas 4 : débit très en-deçà des attentes

**Symptômes :** speedtest à 20 Mbps alors qu'on attend 200+.

**Checklist par probabilité :**
1. **Sur quelle bande est le client ?** (2.4 vs 5 GHz) : un client en 2.4 GHz à 20 MHz ne dépassera jamais ~70 Mbps. Forcer le test en 5 GHz (chap. 42).
2. **Le lien filaire est-il en gigabit ?** `display interface` : un port tombé à 100 Mbps (câble abîmé, injecteur 100M) bride tout l'AP à ~95 Mbps. **Cause ultra-fréquente et sous-diagnostiquée.**
3. **La largeur de canal est-elle celle prévue ?** Un 80 MHz retombé à 20 MHz (interférence, DFS) divise le débit par 4. Vérifier la largeur réelle.
4. **L'airtime est-il saturé ?** Channel utilization > 70 % = le canal est plein (voisins, clients zombies). Changer de canal ou réduire la largeur.
5. **Le client est-il le goulot ?** Tester avec **2–3 clients différents** (un vieux smartphone ne fera jamais 300 Mbps). Tester aussi en **montant** : un mauvais montant = souvent le client.
6. **Le serveur de test est-il le goulot ?** Un speedtest vers Internet mesure la fibre + le Wi-Fi. **iperf3 vers un serveur du LAN** pour isoler le Wi-Fi.
7. **Y a-t-il un client zombie ?** (cas 13) : un client à 1 Mbps en 2.4 GHz plombe toute la cellule.

**Méthode de mesure propre :** iperf3, 3 runs, noter down/up à 3 distances (chap. 60). Un seul speedtest ne prouve rien.

## 82. Cas 5 : clients 5 GHz qui restent accrochés au 2.4 GHz

**Symptômes :** le 5 GHz est vide, le 2.4 GHz est saturé, les utilisateurs se plaignent de lenteur.

**Causes et remèdes :**
1. **Puissance 2.4 GHz trop forte** par rapport au 5 GHz : le client « entend » mieux le 2.4 GHz. **Baisser le 2.4 GHz de 3–6 dB** sous le 5 GHz (chap. 40).
2. **Pas de band steering** : l'activer (chap. 42) — avec prudence sur les parcs IoT.
3. **Le client est loin** : à −75 dBm en 5 GHz, un client préfère légitimement le 2.4 GHz à −65 dBm. C'est normal : la solution est un **meilleur placement d'AP**, pas du forcing.
4. **Le client ne supporte pas le 5 GHz** : objets connectés, vieux PC. **SSID IoT dédié en 2.4 GHz** (chap. 43) pour les isoler du problème.
5. **Le 5 GHz est en DFS avec CAC** : après un redémarrage, le 5 GHz met 60 s à apparaître — les clients s'accrochent au 2.4 GHz entre-temps et y restent (sticky client). Préférer les canaux non-DFS si c'est récurrent.

**Vérification :** ratio clients 5 GHz / total > 70 % en zone couverte (chap. 42). En dessous : appliquer les remèdes ci-dessus un par un.

## 83. Cas 6 : coupures lors des déplacements (roaming)

**Symptômes :** en marchant d'une zone à l'autre, la visio coupe, l'appel tombe, le ping perd 30+ paquets.

**Diagnostic :**
1. **Y a-t-il un trou de couverture ?** Faire le tour avec un relevé RSSI (chap. 60) : si le client passe par une zone < −75 dBm entre les deux AP, il n'y a pas de roaming qui tienne — il faut un AP ou un repositionnement.
2. **Le 802.11r est-il actif ?** Sans r, chaque roaming en 802.1X coûte 200–500 ms (chap. 56). L'activer et **tester avec le parc réel**.
3. **Les AP sont-ils dans le même domaine de mobilité ?** Même SSID/sécurité/VLAN/groupe (chap. 57). En Fat avec des AP indépendants, le roaming est best-effort.
4. **Le client est-il sticky ?** Certains clients (vieux Android, IoT) ne roament jamais proprement. Tester avec 2–3 modèles différents pour isoler.
5. **Y a-t-il un changement de VLAN ?** Même SSID mais VLAN différent selon l'AP = changement de sous-réseau = sessions coupées. Uniformiser.

**Test de référence** (chap. 57) : ping continu en marchant. Objectif : **< 5 pings perdus** par transition en PSK avec 802.11r, **< 10** en 802.1X.

## 84. Cas 7 : interférences 2.4 GHz (micro-ondes, Bluetooth, caméras)

**Symptômes :** débit en dents de scie, retries élevés, problèmes **à heures fixes** (le midi = le micro-ondes du food-truck ; le soir = les caméras du voisin).

**Diagnostic :**
1. **Corréler avec l'heure :** un problème qui apparaît tous les jours à 12h00 n'est pas un problème Wi-Fi, c'est un **micro-ondes**. Noter les horaires précis pendant une semaine.
2. **Scanner :** les réseaux Wi-Fi voisins ne sont que la partie visible. Les **non-Wi-Fi** (micro-ondes, Bluetooth, caméras analogiques 2.4 GHz, ZigBee) n'apparaissent pas dans les scans Wi-Fi mais pourrissent le canal pareil.
3. **Tester en coupant les suspects** un par un (chap. 65).
4. **Vérifier le SNR**, pas seulement le RSSI (chap. 61) : un bon RSSI avec un mauvais SNR = interférence.

**Remèdes par source :**
| Source | Remède |
|---|---|
| Four à micro-ondes | Éloigner l'AP / changer de canal ne sert à rien (pollution large) → **déplacer l'AP** ou blinder |
| Caméra sans fil du voisin | Passer en 5 GHz les clients critiques ; discuter avec le voisin (canal) |
| Bluetooth (enceintes) | Le Bluetooth saute de fréquence : impact modéré, mais réel à proximité immédiate |
| Son propre IoT 2.4 GHz | SSID IoT sur canal 1/6/11 dédié, séparé du SSID principal |

**Si rien ne marche :** basculer les usages critiques en **5 GHz** (plus de canaux, moins de pollution non-Wi-Fi) et réserver le 2.4 GHz à l'IoT.

## 85. Cas 8 : radar DFS — l'AP change de canal tout seul

**Symptômes :** coupures brèves et simultanées de tous les clients 5 GHz, l'AP est sur un canal différent après, les logs montrent `radar detected` / `channel change`.

**C'est un comportement NORMAL et OBLIGATOIRE** (chap. 41), pas une panne. Le diagnostic consiste à le confirmer puis à décider :

1. **Confirmer :** logs (`display logbuffer` — chercher `DFS`, `radar`, `channel switch`), corrélation avec la météo (orages) ou la proximité d'un aéroport/météo radar.
2. **Si c'est occasionnel** (quelques fois par an) : ne rien faire, c'est la vie en DFS.
3. **Si c'est fréquent** (hebdomadaire) : **migrer vers les canaux non-DFS 36–48** (chap. 38). On perd du spectre, on gagne en stabilité — pour un usage pro, la stabilité gagne toujours.
4. **Si c'est quotidien à heure fixe** : suspecter un faux positif (un équipement local qui ressemble à un radar) — investiguer la source (chap. 65).

**À noter dans le dossier de site** quels AP sont en DFS : dans 6 mois, quand ça recommencera, tu sauras où regarder en 30 secondes.

## 86. Cas 9 : PoE insuffisant — fonctions restreintes en 802.3af

**Symptômes :** l'AP démarre, le Wi-Fi « marche », mais : débit plafonné, USB/BLE inactifs, puissance radio qui ne monte pas, messages `power insufficient` dans les logs.

**Rappel constructeur :** en **802.3af**, l'AP761 fonctionne en **mode restreint** [datasheet]. Le 802.3af fournit 12.95 W utiles, l'AP en veut 17.7 W max.

**Diagnostic :**
1. Vérifier ce que le switch **négocie** : `display poe interface` (côté switch Huawei) — af ou at ?
2. Vérifier la **classe LLDP** négociée par l'AP.
3. Câble trop long ou de mauvaise qualité : la chute de tension peut faire retomber une négociation at en comportement af.

**Remèdes :**
1. **Passer le port en 802.3at** (PoE+) : la solution dans 95 % des cas (chap. 25).
2. Si le switch ne fait que du af : **injecteur 802.3at 30 W** ou changement de switch.
3. En attendant : identifier les fonctions bridées (logs) et vérifier que l'essentiel (Wi-Fi) tourne — mais **ne pas laisser en l'état**, c'est un mode dégradé.

**Prévention :** à l'installation, **tester la négociation PoE** et la noter dans le dossier de site. Un AP installé en af « parce que le port était libre » = une panne programmée.

## 87. Cas 10 : SFP branché mais pas de lien

**Symptômes :** le module SFP est enfiché, la fibre est brassée, mais pas de lien (LED éteinte côté AP ou côté switch).

**Diagnostic ordonné :**
1. **Rappel combo** (chap. 6) : dès que le SFP est branché, **il prend la priorité** sur le RJ45. Si le SFP ne monte pas, l'AP perd aussi son lien cuivre → prévoir une intervention physique, pas à distance.
2. **Le module SFP est-il compatible ?** Certains AP sont stricts sur les SFP (codage constructeur). Tester avec un module **Huawei d'origine** ou un compatible connu.
3. **TX/RX croisés ?** La fibre doit croiser : TX d'un côté → RX de l'autre. Vérifier le brassage (jarretières croisées ou non selon les modules).
4. **Les niveaux optiques sont-ils bons ?** `display transceiver` côté équipements qui le supportent : TX/RX en dBm. Un RX < −20 dBm = fibre sale, trop longue, ou jarretière HS.
5. **Nettoyer les connecteurs** : 80 % des problèmes optiques = une férule sale. Nettoyage avec les outils adaptés (jamais au souffle ni au chiffon).
6. **Propreté du port SFP** : cache anti-poussière en place quand inutilisé.

**Test décisif :** boucle locale (loopback optique) sur le module : si le lien monte en loopback, le module est bon et le problème est la fibre/le distant.

## 88. Cas 11 : WPA3 activé, certains clients ne se connectent plus

**Symptômes :** après bascule en WPA3-SAE (ou WPA3-only), une partie du parc ne s'associe plus : vieilles imprimantes, caméras, badges, vieux smartphones.

**C'est le comportement attendu** (chap. 46), pas un bug. Conduite à tenir :

1. **Ne pas paniquer, ne pas tout rebasculer** : identifier les clients impactés (liste via la console cloud : qui ne revient pas ?).
2. **Basculer en mode transition WPA2/WPA3** (chap. 47) : les modernes passent en SAE, les anciens restent en WPA2.
3. **Isoler les récalcitrants** : SSID dédié en WPA2-PSK pour l'IoT/les vieux équipements (chap. 43) — c'est aussi plus propre en segmentation.
4. **Plan de remplacement** : noter les équipements à remplacer à leur prochain renouvellement (imprimante de 2015 → prévoir).
5. **Vérifier le PMF** (chap. 51) : en WPA3, le PMF est requis — certains clients « WPA3-compatibles » sur le papier échouent à cause du PMF. Tester PMF optionnel vs obligatoire.

**Règle :** la sécurité se monte **au rythme du parc réel**, pas au rythme des annonces marketing. Un SSID en transition bien géré vaut mieux qu'un WPA3-only qui force les utilisateurs à contourner (partage de connexion 4G = pire).

## 89. Cas 12 : portail captif qui ne s'affiche pas

**Symptômes :** le client se connecte à `INVITE`, mais la page d'authentification ne s'ouvre pas — « connecté, pas d'Internet ».

**Causes par ordre de fréquence :**
1. **Le client a un VPN actif** : le VPN chiffre tout avant la détection du portail → la sonde CNA échoue. Demander de **couper le VPN** pour l'authentification, le réactiver après.
2. **DNS personnalisé** (8.8.8.8 en dur, DNS-over-HTTPS du navigateur) : la redirection du portail repose sur l'interception DNS. Forcer le DNS du réseau pendant l'authentification, ou donner l'**URL directe** du portail (à afficher à l'accueil).
3. **Le navigateur bloque la détection** : ouvrir manuellement un site HTTP (pas HTTPS) quelconque, ex. `http://neverssl.com` — ça déclenche la redirection.
4. **HTTPS intercepté** : si le portail intercepte du HTTPS, le navigateur crie au certificat invalide — normal, mais ça effraie. Utiliser la détection CNA du système plutôt que le navigateur.
5. **Le client est déjà « authentifié » mais expiré** : vider le cache / oublier le réseau / réassocier.

**Prévention (à afficher à l'accueil, sur un chevalet) :**
```
Wi-Fi INVITÉ — mode d'emploi
1. Connectez-vous au réseau « INVITE »
2. Coupez votre VPN le temps de la connexion
3. Ouvrez votre navigateur : la page d'accueil s'affiche
   Si rien ne s'affiche : allez sur http://neverssl.com
4. Acceptez les conditions, cliquez « Se connecter »
5. Vous pouvez réactiver votre VPN
```

## 90. Cas 13 : un client consomme tout l'airtime (slow client)

**Symptômes :** tout est lent pour tout le monde, alors que le signal est bon et le canal pas saturé par les voisins. Un seul client avec un mauvais débit **monopolise le temps de parole**.

**La physique :** le Wi-Fi partage le **temps**, pas le débit. Un client à 1 Mbps qui envoie 1 Mbit occupe le canal **100× plus longtemps** qu'un client à 100 Mbps qui envoie le même Mbit. Un seul client à 1 Mbps peut diviser la capacité de la cellule par 2–3.

**Diagnostic :**
1. Identifier le client lent : console cloud (débit par client) ou `display wlan sta` — chercher les clients avec un **rate très bas** et un **RSSI très faible** (−80 dBm et pire).
2. Vérifier : est-ce un client légitime loin, ou un client bloqué en 802.11b ?

**Remèdes :**
1. **Relever les débits de base** (chap. 13) : interdire 1/2/5.5/11 Mbps en 2.4 GHz. Le client zombie ne pourra plus s'associer à 1 Mbps — il devra avoir un meilleur signal ou partir.
2. **Airtime fairness** (si supporté par la version — à vérifier) : l'AP alloue du **temps** équitable plutôt que du débit égal — les clients rapides ne sont plus pénalisés par les lents.
3. **Limiter le débit par client** sur le SSID invité : un invité n'a pas besoin de 100 Mbps.
4. **Déplacer le problème** : si c'est une caméra lointaine en 2.4 GHz, la rapprocher d'un AP ou la passer en filaire.

**Règle :** sur un site pro, **aucun client ne devrait rester associé sous −75 dBm en 2.4 GHz**. En dessous, c'est de la pollution pour tout le monde.

## 91. Cas 14 : l'AP surchauffe / redémarre en plein soleil

**Symptômes :** redémarrages aux heures chaudes (13h–17h), AP brûlant au toucher, logs avec des alertes température, retour à la normale le soir.

**L'AP761 est spécifié −40 °C à +65 °C** [constructeur] — mais c'est la température **ambiante**, pas la température du boîtier au soleil. Un boîtier métal blanc au soleil d'août peut dépasser 80 °C en surface.

**Diagnostic :**
1. Corréler les redémarrages avec la **température/météo** (logs horodatés — d'où le NTP, chap. 70).
2. Vérifier les alertes température dans les logs et la console.
3. Contrôler l'**exposition** : plein sud sans ombre ? Sous un auvent qui fait four ?

**Remèdes :**
1. **Ne jamais enfermer** l'AP dans un coffret étanche non ventilé (chap. 5).
2. **Pare-soleil** : un simple auvent/champagneau au-dessus de l'AP (sans bloquer la ventilation ni le secteur radio) fait chuter la température de 10–15 °C.
3. **Orientation** : éviter le plein sud direct si possible ; un mur à l'est ou à l'ouest prend le soleil moins longtemps.
4. Vérifier que les **ouïes de ventilation** (s'il y en a — à vérifier sur la fiche du modèle exact) ne sont pas obstruées (nids, toiles, poussière).
5. Si le site dépasse durablement les specs : **ombrage obligatoire** ou déplacement — faire fonctionner un AP hors spec, c'est accepter des pannes et perdre la garantie.

**En hiver, l'inverse :** −40 °C supportés [constructeur], mais attention à la **condensation** lors des cycles gel/dégel — presse-étoupes vers le bas, pas de stagnation d'eau (chap. 5).

## 92. Cas 15 : rogue AP détecté — que faire concrètement

**Symptômes :** alerte WIDS « rogue AP détecté », un SSID suspect apparaît dans les scans.

**Procédure (ne pas improviser) :**
1. **Ne pas contre-attaquer immédiatement** (chap. 63–64) : classifier d'abord.
2. **Identifier :** noter BSSID (MAC), SSID, canal, RSSI vu par tes AP (triangulation approximative : l'AP qui le voit le plus fort est le plus proche).
3. **Est-il sur ton filaire ?** Chercher sa MAC dans les tables MAC des switches.
   - **Oui → interne :** remonter au port, aller voir physiquement (chap. 64). 90 % des cas = un équipement personnel.
   - **Non → externe :** usurpe-t-il ton SSID (evil twin) ou est-ce un voisin légitime ?
4. **Evil twin confirmé** (même SSID que toi, fort près de tes locaux) : activer la **contre-mesure ciblée** (désauthentification du rogue — fonction wIPS, à vérifier selon version), **alerter la direction**, conserver les logs (preuve), envisager un dépôt de plainte si malveillance avérée.
5. **Voisin légitime** (SSID différent, pas sur ton filaire) : **ne rien faire** d'agressif. Optimiser tes canaux pour coexister (chap. 37–38).

**Après incident :** noter dans le dossier de site (date, MAC, action). Si c'était un salarié : **sensibilisation** — et se demander pourquoi il a branché un AP pirate (chap. 64).

## 93. Cas 16 : après mise à jour firmware, comportements bizarres

**Symptômes :** depuis la mise à jour : clients qui ne s'associent plus, débit en berne, redémarrages, fonctions qui ont disparu des menus.

**Conduite à tenir :**
1. **Ne pas paniquer, mesurer** : comparer les indicateurs (chap. 67) avant/après. « Bizarre » n'est pas un diagnostic.
2. **Lire la release note** : le comportement a peut-être **changé volontairement** (nouveau défaut de canal, PMF activé par défaut, nom de commande modifié).
3. **Vérifier la config migrée** : `display current-configuration` — une migration peut avoir réinitialisé des paramètres (canaux repassés en auto, puissances par défaut).
4. **Si régression avérée → rollback** (chap. 76) : re-flasher l'ancienne version + restaurer la sauvegarde pré-mise à jour.
5. **Remonter au support** avec : versions avant/après, logs, description reproductible.

**Prévention (rappel chap. 74) :** site pilote d'abord, jamais de mise à jour massive un vendredi à 17h, sauvegarde avant, rollback testé. **Le firmware n'est pas un antivirus : on ne met à jour que pour une raison** (faille, bug qui te touche, fonction nécessaire).

## 94. Cas 17 : itinérance entre AP761 et AP indoor

**Symptômes :** en passant de la cour (AP761) au bâtiment (AP361 indoor), les clients coupent ou restent accrochés à l'AP extérieur.

**C'est le cas mixte classique :** deux modèles, deux environnements, un seul parcours utilisateur.

**Points de vigilance :**
1. **Même SSID, même sécurité, mêmes VLAN** des deux côtés — sinon le client change de réseau en passant la porte.
2. **Puissances équilibrées à la frontière** : l'AP761 en extérieur a tendance à « crier » plus fort que l'AP361 indoor. Résultat : le client reste accroché à l'AP761 depuis l'intérieur (à travers le mur, en 2.4 GHz) avec un débit minable. **Baisser la puissance 2.4 GHz de l'AP761** côté bâtiment, ou orienter son secteur **loin** de la façade.
3. **Zone de transition** : il faut un **recouvrement** à −67 dBm juste devant l'entrée — ni trou (coupure), ni recouvrement énorme (client sticky).
4. **Bande 2.4 GHz traîtresse** : elle traverse les murs, pas le 5 GHz. Un client qui entre dans le bâtiment reste en 2.4 GHz sur l'AP761 extérieur. **Débits de base relevés** (chap. 13) pour le forcer à lâcher.
5. **Tester le parcours réel** : sortir avec un appel en cours, entrer, noter où ça coupe (chap. 57).

**Astuce :** si la zone d'entrée est critique (accueil, contrôle d'accès), dédier un **petit AP indoor près de l'entrée** plutôt que de compter sur l'AP761 extérieur pour couvrir l'intérieur — ce n'est pas son travail (chap. 9 : l'arrière est sourd).

## 95. Cas 18 : je veux du Wi-Fi 7 — que faire avec mes AP761 ?

**Question légitime** quand la direction lit « Wi-Fi 7 » dans la presse. Réponse structurée en 3 temps :

**Temps 1 — Cadrer le besoin réel :**
- Quel problème le Wi-Fi 7 résoudrait-il **que le Wi-Fi 6 ne résout pas** ? (Débit par client > 500 Mbps ? Latence < 5 ms ? Densité > 100 clients/AP ?)
- Les **clients** sont-ils Wi-Fi 7 ? (En 2026 : une minorité. Un AP Wi-Fi 7 avec des clients Wi-Fi 6 = un AP Wi-Fi 6 cher.)
- Dans 80 % des cas PME, la réponse est : **le Wi-Fi 6 bien déployé suffit** — et l'argent est mieux investi dans le câblage, le PoE et la couverture.

**Temps 2 — Si le besoin est avéré :**
- **Ne pas jeter les AP761** : ils restent excellents pour la couverture générale. Ajouter des **AP Wi-Fi 7 (AP772E)** sur les **zones denses** (chap. 21, 99–100).
- **Refaire le budget** : uplink 2.5G/10G, PoE 802.3bt, câblage Cat6A — le Wi-Fi 7 ne vit pas sur une infra Wi-Fi 6 (chap. 25).
- **Cohabitation** : même SSID sur les deux générations, transition WPA2/WPA3 (chap. 47), canaux coordonnés.

**Temps 3 — Calendrier réaliste :**
- 2026–2027 : les clients Wi-Fi 7 deviennent majoritaires dans le neuf.
- Un AP761 bien installé aujourd'hui a encore **5–7 ans** de vie utile devant lui (chap. 105).
- **Ne pas acheter du Wi-Fi 7 « pour le futur »** en 2026 si le besoin n'existe pas : dans 3 ans, les produits seront meilleurs et moins chers.

## 96. Comparatif AP761 vs AP361 — tableau décisionnel

Les deux sont des **Wi-Fi 6 1.775 Gbps 2×2** [constructeur] — la différence n'est pas le Wi-Fi, c'est **l'enveloppe et l'usage**.

| Critère | **AP761** (outdoor) | **AP361** (indoor) |
|---|---|---|
| Usage prévu | Extérieur : cours, parkings, terrasses | Intérieur : bureaux, commerces, salles |
| Standard | Wi-Fi 6, 2×2, 1.775 Gbps | Wi-Fi 6, 2×2, 1.775 Gbps |
| Ports | 1× GE PoE-In + 1× SFP GE (combo) | 1× GE PoE |
| PoE | 802.3at/af, **17.7 W max** | 802.3at/af (à vérifier), **8.8 W max** [constructeur] |
| Antennes | **Directionnelles** 10/11 dBi, 65° | **Smart antennas** (omnidirectionnelles adaptatives) |
| Protection | **IP68**, −40 à +65 °C, 6 kV | Intérieur (pas d'IP68) |
| Montage | Mur / mât | Plafond / mur |
| Dimensions / poids | 200×200×69 mm, **1.91 kg** | 180×35 mm (disque), léger |
| Clients max | 1024 (512/radio) | 300 (recommandés, selon manuel eKit) |
| Prix indicatif | ~200–245 € HT (discomp.cz, 2026) | ~70–85 € HT (discomp.cz, 2026) |
| BLE | 5.2 | À vérifier sur la fiche du modèle exact |

**Lecture :** à Wi-Fi égal, l'AP761 coûte **~3× le prix** de l'AP361 : tu paies l'IP68, le métal, les antennes directionnelles, le SFP, la plage de température. **Ne jamais mettre un AP761 en intérieur** (surcoût inutile + secteur directionnel inadapté) **ni un AP361 en extérieur** (il mourra à la première pluie — et la garantie ne suivra pas).

## 97. Quand choisir l'AP361 plutôt que l'AP761

**Choisir l'AP361 quand :**
- ✅ La zone est **intérieure** (bureau, salle de réunion, commerce, hall).
- ✅ Le budget est serré : ~3× moins cher que l'AP761 pour le même Wi-Fi 6.
- ✅ La consommation compte : 8.8 W vs 17.7 W — sur un gros parc, ça se voit sur le budget PoE et l'onduleur.
- ✅ Il faut une couverture **à 360°** (open space) : ses smart antennas omnidirectionnelles sont faites pour ça.
- ✅ Le montage est au **plafond** (format disque discret).

**Ne pas choisir l'AP361 quand :**
- ❌ Extérieur ou local humide/poussiéreux (pas d'IP68).
- ❌ Il faut de la **distance directionnelle** (cour de 100 m : l'AP361 arrosera partout faiblement au lieu de porter loin dans l'axe).
- ❌ Il faut un **uplink fibre** (pas de SFP).
- ❌ Environnement industriel chaud/froid extrême.

## 98. Quand choisir l'AP761 plutôt que l'AP361

**Choisir l'AP761 quand :**
- ✅ **Extérieur**, sans hésitation : c'est son métier (IP68, −40 à +65 °C, 6 kV).
- ✅ Zone à couvrir en **secteur** (cour, parking, terrasse) : les antennes directionnelles 65° portent loin et proprement.
- ✅ Distance **> 100 m** jusqu'au local : le SFP fibre règle le problème.
- ✅ Environnement **hostile** : poussière (entrepôt ouvert, carrière), embruns (bord de mer — rincer le sel lors des visites, chap. 102), variations thermiques fortes.
- ✅ Besoin de **BLE** pour l'onboarding/maintenance de proximité (5.2) — à vérifier pour l'AP361.

**Ne pas choisir l'AP761 quand :**
- ❌ Intérieur : surcoût, secteur directionnel inadapté, esthétique « pavé industriel » au plafond d'un bureau.
- ❌ Le besoin est du **Wi-Fi 7** : voir chap. 99.
- ❌ Le budget PoE est déjà tendu : 17.7 W par AP, ça se planifie (chap. 25).

## 99. Et le Wi-Fi 7 outdoor ? AP771 vs AP772E

Quand le besoin Wi-Fi 7 outdoor est avéré (chap. 95), les deux candidats eKit (valeurs vérifiées le 27/09/2026 — détails **à vérifier sur la fiche du modèle exact**) :

| Critère | AP771 | AP772E |
|---|---|---|
| Wi-Fi | 7 (802.11be), bi-bande 2.4/5 GHz | 7 (802.11be), bi-bande 2.4/5 GHz |
| Débit max | 3.57 Gbps | **6.45 Gbps** |
| Ports | à vérifier | 1× **2.5GE** PoE + 1× **10GE SFP+** |
| PoE | à vérifier | **802.3af/at/bt** ou DC |
| Protection | à vérifier (outdoor) | IP68, −40 à +70 °C, 6 kA |
| Portée indicative | ~130 m | ~250 m (optimale) |
| Cas d'usage | Outdoor Wi-Fi 7 **économique**, zones modérées | Outdoor Wi-Fi 7 **haute capacité**, zones denses |

**Ce que le choix implique (au-delà du prix de l'AP) :**
- **AP772E :** switch **2.5G PoE++ (802.3bt)** ou injecteur bt, câblage **Cat6A** conseillé pour la 10G sur SFP+ (fibre en pratique), budget PoE **recalculé** (un AP Wi-Fi 7 consomme nettement plus que 17.7 W — valeur exacte à vérifier sur la fiche du modèle exact).
- **AP771 :** marche probable sur infra existante 1G/PoE+ — à vérifier, mais c'est son positionnement « Wi-Fi 7 accessible ».
- **Les deux :** pas de 6 GHz, pas de 320 MHz (chap. 16) — le Wi-Fi 7 outdoor eKit, c'est du MLO + 4K-QAM + efficacité, pas des canaux géants.

## 100. Tableau décisionnel global : quel AP eKit pour quel besoin

| Besoin | Choix | Pourquoi |
|---|---|---|
| Bureau PME, Wi-Fi 6, petit budget | **AP361** | Le moins cher, 8.8 W, largement suffisant |
| Bureau dense, Wi-Fi 6 | **AP362/AP362E** | 2.975 Gbps, plus de capacité |
| Open space très dense / auditorium | **AP661** | 6.575 Gbps, tri-radio |
| **Cour, parking, terrasse — Wi-Fi 6** | **AP761** | **IP68, directionnel, SFP : c'est lui** |
| Cour/parking — Wi-Fi 7, budget serré | **AP771** | Wi-Fi 7 outdoor accessible — à vérifier |
| Zone dense outdoor — Wi-Fi 7 | **AP772E** | 6.45 Gbps, 2.5G/10G, bt — à vérifier |
| Indoor — Wi-Fi 7 économique | **AP371** | 3.57 Gbps, 2.5GE |
| Indoor — Wi-Fi 7 max (avec 6 GHz) | **AP673** | Tri-bande, 320 MHz — à vérifier |
| Chambre d'hôtel | **AP160/AP162E** | Format mural, discret |

**Règle d'or :** on choisit d'abord **l'environnement** (indoor/outdoor), puis **la capacité** (débit/densité), puis **la génération** (Wi-Fi 6 vs 7). Choisir la génération d'abord, c'est acheter un AP772E à 600 € pour couvrir un parking vide.

## 101. Maintenance préventive : planning annuel

Un AP extérieur s'entretient comme tout équipement exposé. Le planning type pour un parc d'AP761 :

| Fréquence | Action | Chapitre |
|---|---|---|
| **Mensuelle** (15 min, à distance) | Revue console cloud : AP en ligne, alertes, utilization canal, clients | 66–67 |
| **Trimestrielle** (à distance) | Tendances : un site se remplit-il ? Firmware à jour ? Sauvegardes OK ? | 72, 74 |
| **Semestrielle** (sur site) | Contrôle visuel : fixation, câble, presse-étoupes, parafoudre (voyant), nettoyage | 102 |
| **Annuelle** (sur site) | Contrôle électrique : PoE, terre, serrages. Contrôle radio : mini-survey, canaux, puissances | 103–104 |
| **Annuelle** (bureau) | Revue du dossier de site, test de restauration d'une sauvegarde, revue des alertes | 73 |
| **Après chaque orage violent** | Contrôle parafoudres + état des AP exposés | 26 |
| **Après travaux à proximité** | Vérifier orientation, obstacles nouveaux, câbles | 62 |

**Le dossier de site** (le vrai livrable de la maintenance) :
```
dossier-site/
├── plan-implantation.pdf (AP positionnés, secteurs tracés)
├── photos-avant-apres/
├── releves-survey/ (fiches de mesure chap. 60)
├── configs/ (sauvegardes chap. 72)
├── firmwares/ (versions + release notes)
├── interventions.log (date, qui, quoi, pourquoi)
└── contacts/ (astreinte, SAV, distributeur)
```
Un site sans dossier à jour = un site qu'on redécouvre à chaque panne. **Le dossier, c'est la moitié de la maintenance.**

## 102. Contrôle visuel et nettoyage (semestriel)

**Checklist visite semestrielle (par AP, 10 min) :**
- [ ] **Fixation :** étrier/colliers serrés ? Pas de jeu ? (Revérifier le serrage après l'hiver — le gel/dégel desserre.)
- [ ] **Orientation :** l'AP pointe-t-il toujours vers la zone ? (Tempête, travaux, vandalisme.)
- [ ] **Câble :** pas de frottement, pas de gaine craquelée par les UV, boucle d'égouttage en place.
- [ ] **Presse-étoupes :** serrés, pas de trace d'eau, joints souples.
- [ ] **Parafoudre :** voyant d'état OK (chap. 26). Le remplacer si doute.
- [ ] **Environnement :** végétation qui a poussé devant le secteur ? Nouvel obstacle (chantier, enseigne, camion permanent) ?
- [ ] **Propreté :** dépoussiérer le boîtier (chiffon humide, **jamais de nettoyeur haute pression** — l'IP68 a des limites face au karcher).
- [ ] **Bord de mer :** rincer le sel au jet doux — la corrosion adore les fixations.
- [ ] **Étiquetage :** l'étiquette d'identification est-elle toujours lisible ? (Sinon : ré-étiqueter.)

**Noter dans le dossier :** date, technicien, observations, photos si anomalie. Une anomalie notée = une anomalie suivie.

## 103. Contrôle électrique et PoE (annuel)

Ton domaine, Zelef — le Wi-Fi n'est que la partie émergée.

**Checklist annuelle électrique (par AP ou par départ) :**
- [ ] **Tension PoE** au pied de l'AP si accessible (ou négociation affichée côté switch : af/at ?).
- [ ] **Budget PoE** du switch : toujours suffisant ? (On a ajouté des AP depuis ? — chap. 25.)
- [ ] **Serrage** des connexions dans les coffrets (dilatation thermique annuelle).
- [ ] **Continuité de terre** : mât, boîtier, parafoudres (ohmmètre — valeur selon norme locale).
- [ ] **Parafoudres :** état, date de mise en service, remplacement si > 5 ans ou après coup encaissé (chap. 26).
- [ ] **Onduleur du local** : autonomie réelle vs dimensionnement (les AP + le switch PoE sont une charge non négligeable — voir ton guide onduleurs).
- [ ] **Thermographie** (si caméra dispo) : un connecteur ou un injecteur qui chauffe anormalement = un problème en gestation.

**Tracer les mesures** : un tableau de suivi annuel (tension, terre, observations) permet de voir les dérives — c'est ça, la maintenance préventive, par opposition au « on répare quand ça casse ».

## 104. Contrôle radio annuel (audit)

Une fois par an, refaire un **mini-survey** (chap. 60, version allégée) : 30 minutes par site.

**Protocole allégé :**
1. Relever les canaux/largeurs/puissances **réels** (ont-ils dérivé ? un voisin a-t-il changé de canal ?).
2. Mesurer le RSSI en 3 points de référence (les mêmes chaque année — les noter dans le dossier).
3. Comparer aux relevés de l'année précédente : **une dérive de 10 dB = quelque chose a changé** (végétation, nouvel obstacle, AP voisin, AP qui fatigue).
4. Vérifier la répartition 2.4/5 GHz des clients (chap. 42).
5. Vérifier les rogues (chap. 64) : un an, c'est long — il a pu s'en installer.

**Ce qu'on cherche :** pas la perfection, mais les **dérives**. Un réseau Wi-Fi extérieur vit : la végétation pousse, les voisins changent de box, les usages évoluent. L'audit annuel, c'est le moment où on le constate **avant** que les utilisateurs s'en plaignent.

## 105. Gestion du cycle de vie et fin de vie

**Durée de vie réaliste d'un AP761 extérieur : 5–7 ans.** Après :
- Les condensateurs et les joints vieillissent (surtout en plein soleil).
- Le Wi-Fi des clients a évolué (en 2030, le Wi-Fi 7 sera banal).
- Le support firmware se tarit (fin de support constructeur — à vérifier sur la fiche du modèle exact).

**Signes de fin de vie :**
- Redémarrages inexpliqués de plus en plus fréquents.
- Débit qui se dégrade sans cause externe (composants RF qui dérivent).
- Plus de firmware depuis 2+ ans (failles non corrigées = risque sécurité).
- Pièces de rechange introuvables.

**Planification du renouvellement :**
1. **Inventaire** : âge de chaque AP, date d'achat, S/N (le dossier de site, chap. 101).
2. **Provisionner** le renouvellement à 5 ans : budgéter le remplacement par tiers (un tiers du parc par an sur 3 ans = pas de big bang).
3. **Ne pas attendre la panne** : un AP extérieur qui meurt un vendredi soir en plein événement = une urgence évitable.
4. **Recyclage** : DEEE — les AP contiennent des métaux et des composants à traiter en filière adaptée. Effacer la config avant mise au rebut (`reset` + vérification).

**Et le Wi-Fi 7 dans tout ça ?** Le renouvellement naturel (5–7 ans) est **le bon moment** pour migrer vers le Wi-Fi 7 : les produits seront matures, les clients aussi, et tu ne jettes pas du matériel amorti (chap. 95).

## 106. Durcissement : checklist sécurité

Le durcissement, c'est la différence entre « ça marche » et « ça marche et c'est sûr ». Checklist à appliquer à chaque AP761, **dès l'installation** :

**Comptes et accès :**
- [ ] Compte admin par défaut **changé** (ou désactivé si l'auth se fait via le cloud).
- [ ] Pas de compte « invité »/« test » qui traîne.
- [ ] Mots de passe ≥ 16 caractères, uniques par site (coffre de mots de passe d'équipe).
- [ ] Sessions web en **HTTPS uniquement** (HTTP désactivé ou redirigé).

**Réseau de management :**
- [ ] VLAN management dédié (chap. 52), pas de management depuis les VLAN invités/IoT.
- [ ] ACL : seuls les postes d'administration et le superviseur accèdent au management.
- [ ] Telnet **désactivé**, SSHv2 uniquement (chap. 107).

**Wi-Fi :**
- [ ] Pas de WEP/TKIP, pas de SSID ouvert sans portail (chap. 44).
- [ ] PMF au minimum optionnel sur les SSID sensibles (chap. 51).
- [ ] WIDS activé, alertes configurées (chap. 63).
- [ ] SSID invité : isolation client + VLAN + ACL Internet seul (chap. 49, 53).

**Suivi :**
- [ ] NTP configuré (chap. 70), syslog centralisé (chap. 69).
- [ ] Firmware à jour (chap. 74), alertes configurées (chap. 71).
- [ ] Sauvegarde faite et testée (chap. 72–73).

**Revue :** repasser cette checklist **une fois par an** et après chaque changement majeur. Un durcissement qui n'est pas revu se dégrade (nouveau SSID « temporaire » oublié, compte prestataire jamais supprimé…).

## 107. Durcissement : management (SSH, SNMP, web)

**SSH (STelnet) :**
```
[AP761] ssh server enable
[AP761] undo telnet server enable        # couper Telnet : mots de passe en clair, inacceptable
[AP761] ssh server timeout 60
[AP761] ssh authentication-retries 3
# Restreindre par ACL aux postes d'admin :
[AP761] acl number 2000
[AP761-acl-basic-2000] rule permit source 192.168.10.0 0.0.0.255
[AP761-acl-basic-2000] quit
[AP761] ssh server acl 2000
```
- Clés SSH plutôt que mots de passe si la version le supporte (à vérifier).
- Désactiver l'accès SSH depuis les VLAN utilisateurs : le management ne répond que sur le VLAN 10.

**SNMP :** v3 uniquement, auth+priv, ACL vers le superviseur (chap. 68). **Désactiver v1/v2c** explicitement — un community `public` qui traîne, c'est une lecture du réseau offerte.

**Web :** HTTPS uniquement. Si l'interface web n'est pas utilisée (tout en CLI/cloud), la **désactiver** : moins de surface d'attaque.

**Comptes :** principe du moindre privilège — un compte « lecture seule » pour la supervision, un compte « admin » pour les changements, **jamais de compte partagé** (« admin » connu de 10 personnes = 0 traçabilité). En cas de départ d'un technicien : révoquer **le jour même**.

## 108. Durcissement : Wi-Fi (chiffrement, PMF, WIDS)

**Chiffrement :**
- Bannir WEP, WPA-TKIP, SSID ouverts sans portail (chap. 44).
- Objectif : WPA3-SAE ou transition sur les SSID salariés (chap. 46–47), WPA2-PSK fort minimum partout ailleurs.
- Clés PSK ≥ 20 caractères, uniques par SSID, rotation trimestrielle pour l'invité.

**PMF (802.11w) :** optionnel minimum sur les SSID salariés, obligatoire si le parc le supporte (chap. 51). C'est la protection contre la désauthentification forgée — l'attaque la plus simple et la plus courante.

**WIDS :** activé avec alertes (chap. 63). Revoir **mensuellement** les détections : un WIDS dont personne ne lit les alertes ne sert à rien.

**SSID :**
- Pas d'information sensible dans les noms (chap. 43).
- SSID invité isolé (VLAN + ACL + isolation client).
- Désactiver les SSID inutilisés : chaque SSID actif est de l'airtime consommé et une surface d'attaque.

**Physique :** un AP761 accessible au public (terrasse, camping) = risque de **reset physique** (bouton) ou de vol. Câble antivol (chap. 8), fixation hors de portée (> 3 m), et **en cas de vol : révoquer** — changer les clés PSK des SSID (l'AP volé contient la config… sauf si elle était chiffrée — à vérifier sur la fiche du modèle exact) et retirer l'AP du cloud.

## 109. Pense-bête de poche — page 1 : specs

À imprimer, plastifier, garder dans la caisse à outils.

```
AP761 — L'ESSENTIEL
Wi-Fi 6 (ax), 2×2 2.4+5 GHz, 1.775 Gbps max
Ports : 1× GE RJ45 (PoE-In) + 1× SFP GE — COMBO, optique prioritaire
PoE : 802.3at/af — 17.7 W max — af = FONCTIONS RESTREINTES
Budget : réserver 25 W / AP sur le switch
Antennes : directionnelles 65° — 10 dBi (2.4) / 11 dBi (5)
IP68, -40 à +65 °C, 6 kV surtension
1024 clients max (512/radio), 16 SSID/radio
Dimensions : 200×200×69 mm, 1.91 kg
Réf : 02355VFB — EAN : 6901443451289
Modes : Fat / Fit / Cloud
Canaux : 2.4 GHz = 20 MHz, canaux 1/6/11
         5 GHz = 40/80 MHz, préférer 36-48 (non-DFS)
Puissance : régler pour -67 dBm en limite de zone
Sécurité : WPA2-PSK mini, WPA3-SAE si parc OK, jamais WEP
```

## 110. Pense-bête de poche — page 2 : CLI express

```
--- Lecture (ne modifient rien) ---
display version                    # modèle + firmware
display current-configuration      # config complète
display ip interface brief         # IP des interfaces
display interface brief            # état + débit négocié (100M ? 1G ?)
display wlan vap all               # SSID diffusés
display logbuffer                  # logs récents
display clock                      # heure (NTP ?)
display cpu-usage / display memory # charge

--- Configuration ---
system-view                        # mode config
sysname NOM                        # nommer l'AP
save                               # sauvegarder (TOUJOURS après modif)
reboot                             # redémarrer
?                                  # aide contextuelle (TON MEILLEUR AMI)
[TAB]                              # complétion

--- Diagnostic express ---
ping 192.168.10.1                  # la passerelle répond ?
display arp | include <ip>         # résolution ARP OK ?
display poe interface              # (côté switch) af ou at ?
```

## 111. Pense-bête de poche — page 3 : dépannage express

```
AP éteint          → PoE ? câble ? injecteur ? parafoudre ? budget switch ?
AP non vu (cloud)  → IP ? Internet ? firewall ? NTP ? DNS ?
SSID sans IP       → trunk VLAN ? DHCP ? scope plein ? snooping ?
Débit faible       → bande 2.4/5 ? lien 100M/1G ? largeur canal ?
                     airtime saturé ? client zombie ?
Coupures roaming   → trou couverture ? 802.11r ? même VLAN ?
Interférences      → corréler l'heure, changer de canal, passer en 5 GHz
DFS                → normal ! passer en 36-48 si critique
WPA3 casse clients → mode transition, SSID IoT séparé
Portail invisible  → couper VPN, DNS auto, http://neverssl.com
Surchauffe         → pare-soleil, pas de coffret étanche, vérifier orientation
Rogue              → classifier AVANT de contre-attaquer
```

**La question magique devant tout ticket :**
« UN client ou TOUS les clients ? »
→ Un = problème client. Tous (un AP) = problème AP/réseau.
  Tous (partout) = problème d'infra (DHCP/RADIUS/Internet).

## 112. Glossaire (A–Z)

**802.11a/b/g/n/ac/ax/be** : les générations Wi-Fi. a/b/g = historiques ; n = Wi-Fi 4 ; ac = Wi-Fi 5 ; ax = Wi-Fi 6 ; be = Wi-Fi 7. L'AP761 fait a/b/g/n/ac/ax [constructeur].

**802.11k** : le client reçoit la liste des AP voisins (aide au roaming). Chap. 56.

**802.11r** : roaming rapide (ré-authentification accélérée). Chap. 56.

**802.11v** : l'AP suggère au client de changer d'AP (BSS Transition). Chap. 56.

**802.11w (PMF)** : protection des trames de management contre la désauthentification forgée. Chap. 51.

**802.1Q** : le standard des VLAN taggés sur trunk. Chap. 52.

**802.1X** : authentification réseau par identifiant (EAP), avec RADIUS. Chap. 48.

**802.3af/at/bt** : PoE (15 W), PoE+ (30 W), PoE++ (60/90 W). L'AP761 : af/at, 17.7 W max [constructeur]. Chap. 7.

**ACL** : liste de contrôle d'accès — règles de filtrage. Chap. 55.

**Airtime** : le temps d'occupation du canal radio. Le Wi-Fi partage le temps, pas le débit. Chap. 90.

**Airtime fairness** : allocation équitable du temps de parole entre clients (au lieu d'un débit égal qui pénalise les rapides).

**AP** : point d'accès.

**Association** : le fait pour un client de se connecter à un AP (après authentification).

**Atténuation** : perte de signal (distance, murs, végétation, pluie).

**Balise (beacon)** : trame émise ~10×/s par l'AP pour annoncer le SSID. Chaque SSID actif = des beacons = de l'airtime consommé.

**Band steering** : encourager les clients bi-bande à utiliser le 5 GHz plutôt que le 2.4 GHz. Chap. 42.

**BLE** : Bluetooth Low Energy — ici en 5.2 sur l'AP761, pour l'onboarding et la maintenance de proximité [constructeur].

**BSS coloring** : « couleur » du réseau en Wi-Fi 6 : permet de distinguer son réseau de celui des voisins et de réduire l'attente. Chap. 12.

**CAC (Call Admission Control)** : limite le nombre d'appels voix simultanés pour garantir la qualité. Chap. 54.

**Canal** : la fréquence centrale d'émission. En 2.4 GHz : 1/6/11. En 5 GHz : 36–48 (non-DFS), 52–140 (DFS).

**CAPWAP** : protocole entre un AP en mode Fit et son contrôleur (AC). Chap. 32.

**Client / STA** : l'équipement qui se connecte (smartphone, PC, caméra).

**Combo (port)** : deux ports physiques (ici RJ45 + SFP) dont un seul est actif à la fois. Chap. 6.

**Désauthentification (attaque)** : forgeage de trames pour éjecter les clients — contré par le PMF/802.11w. Chap. 51.

**DFS** : détection radar obligatoire sur les canaux 5 GHz 52–140 ; l'AP doit quitter le canal si radar. Chap. 41.

**DHCP snooping** : ne laisse passer que les réponses DHCP des serveurs de confiance. Chap. 55.

**Downtilt** : inclinaison de l'antenne vers le bas pour couvrir une zone en contrebas.

**EAP** : protocole d'authentification (PEAP, EAP-TLS…) utilisé avec 802.1X. Chap. 48.

**Evil twin** : AP pirate usurpant un SSID légitime. Chap. 64.

**Fat / Fit / Cloud** : les trois modes de l'AP761 — autonome, piloté par contrôleur, piloté par le cloud [constructeur]. Chap. 11.

**Forward-mode** : direct (le trafic client sort en local sur l'AP) vs tunnel (remonte au contrôleur). Chap. 34.

**Goulot (bottleneck)** : le maillon qui limite le débit (souvent : le client, le câble à 100M, ou l'airtime).

**IP68** : étanche à la poussière (6) et à l'immersion temporaire (8) [constructeur]. Chap. 5.

**Isolation client (STA isolation)** : les clients d'un même SSID ne se voient pas. Chap. 53.

**LLDP** : protocole de découverte de voisinage (sert aussi à négocier la classe PoE).

**MLO** : Multi-Link Operation — Wi-Fi 7 : utiliser plusieurs bandes simultanément. **Non supporté par l'AP761.** Chap. 15.

**MRC** : Maximum Ratio Combining — combinaison des signaux reçus sur les antennes pour améliorer la réception [constructeur].

**MU-MIMO** : l'AP parle à plusieurs clients en même temps grâce à ses flux spatiaux [constructeur]. Chap. 12.

**Multi-RU** : Wi-Fi 7 : attribuer plusieurs blocs de sous-porteuses à un même client. **Non supporté par l'AP761.** Chap. 18.

**NAT** : traduction d'adresses (supportée par l'AP761 [constructeur], utile en mode routeur de poche).

**Noise floor (bruit)** : le niveau de bruit ambiant ; le SNR = signal − bruit. Chap. 61.

**OFDMA** : découpage du canal en sous-porteuses pour servir plusieurs clients en même temps — le cœur du Wi-Fi 6 [constructeur]. Chap. 12.

**PIRE** : puissance isotrope rayonnée équivalente = puissance conduite + gain d'antenne. C'est elle qui est limitée par la réglementation.

**PMF** : voir 802.11w.

**PoE** : Power over Ethernet — alimenter l'AP par le câble réseau. Chap. 7, 25.

**Portail captif** : page d'authentification pour les invités. Chap. 49.

**Preamble puncturing** : Wi-Fi 7 : utiliser les morceaux propres d'un canal partiellement brouillé. **Non supporté par l'AP761.** Chap. 18.

**PSK** : clé pré-partagée (WPA2-PSK, WPA3-SAE). Chap. 45–46.

**QAM** : modulation — 1024-QAM (Wi-Fi 6, max de l'AP761 [constructeur]), 4096-QAM (Wi-Fi 7). Chap. 17.

**RADIUS** : serveur d'authentification pour le 802.1X. Chap. 48.

**Radio** : ici : l'émetteur-récepteur Wi-Fi (l'AP761 en a 2 : 2.4 GHz et 5 GHz).

**Retries (réessais)** : trames réémises — un taux élevé signale interférences ou mauvais signal. Chap. 67.

**Roaming** : passage d'un client d'un AP à l'autre en se déplaçant. Chap. 56–57.

**Rogue AP** : point d'accès non autorisé. Chap. 64.

**RSSI** : puissance du signal reçu, en dBm (négatif — plus proche de 0 = mieux). Chap. 61.

**SAE** : authentification WPA3 remplaçant le PSK, résistante au brute-force hors ligne. Chap. 46.

**SFP** : module optique enfichable — sur l'AP761 : port SFP GE pour uplink fibre [constructeur]. Chap. 6.

**SNR** : rapport signal/bruit — **l'indicateur qui gouverne vraiment la qualité**. Chap. 61.

**SSID** : le nom du réseau Wi-Fi. 16 max par radio sur l'AP761 [constructeur]. Chap. 43.

**STA isolation** : voir Isolation client.

**Sticky client** : client qui reste accroché à un AP lointain au lieu de roamer.

**STBC / CDD / CSD** : techniques de diversité d'antennes pour fiabiliser la réception [constructeur].

**Surtension (protection)** : 6 kV sur les ports Ethernet de l'AP761 [constructeur] — contre les transitoires, pas la foudre directe. Chap. 26.

**Syslog** : centralisation des journaux. Chap. 69.

**TWT** : Target Wake Time — négociation des cycles de sommeil des objets connectés (autonomie) [constructeur]. Chap. 12.

**Uplink** : le lien filaire de l'AP vers le réseau (RJ45 ou fibre).

**VAP** : profil virtuel d'AP — l'association SSID + sécurité + VLAN sur une radio (terminologie Huawei). Chap. 34.

**VLAN** : réseau local virtuel — segmentation par SSID. Chap. 52.

**VRP** : le système d'exploitation des équipements Huawei (l'interface CLI de ce guide).

**WIDS/wIPS** : détection / prévention d'intrusion sans fil [constructeur]. Chap. 63.

**Wi-Fi 6 (802.11ax)** : la génération de l'AP761 — OFDMA, MU-MIMO, 1024-QAM, BSS coloring. Chap. 12.

**Wi-Fi 7 (802.11be)** : MLO, 320 MHz, 4096-QAM, multi-RU — **non supporté par l'AP761**. Chap. 14–21.

**WLAN** : réseau local sans fil.

**WMM** : Wi-Fi Multimedia — les 4 classes de priorité QoS (voix, vidéo, best effort, background) [constructeur]. Chap. 54.

**WPA2 / WPA3** : protocoles de sécurité Wi-Fi. Chap. 44–48.

**Zombie (client)** : client accroché avec un signal très faible et un débit minime, qui monopolise l'airtime de toute la cellule. Chap. 90.

## 113. Quiz : 10 questions

Réponds sans regarder les réponses (chap. 114). L'objectif n'est pas le 10/10, c'est de repérer tes angles morts.

**Q1.** L'AP761 est-il un point d'accès Wi-Fi 7 ? Si non, quelles sont ses caractéristiques radio réelles (standard, bandes, MIMO, débit max) ?

**Q2.** L'AP761 a deux ports filaires : un RJ45 GE et un SFP GE. Peut-on les utiliser simultanément (l'un en uplink, l'autre en downlink) ? Que se passe-t-il si les deux sont branchés ?

**Q3.** Ton switch ne fournit que du PoE 802.3af sur le port de l'AP761. L'AP démarre. Quel est le problème, et que dit le constructeur à ce sujet ?

**Q4.** Tu dois couvrir une cour rectangulaire de 60 × 40 m avec des AP761. Les antennes sont directionnelles (65° horizontal). Où places-tu les AP et comment les orientes-tu ? Pourquoi ne peux-tu pas compter sur « l'arrière » de l'AP ?

**Q5.** En 2.4 GHz, quels sont les seuls canaux à utiliser en Europe, et pourquoi ? Que penses-tu du 40 MHz en 2.4 GHz ?

**Q6.** Un client se plaint de coupures de son appel Wi-Fi tous les jours vers 14h, et tu constates que l'AP a changé de canal 5 GHz. Explique le mécanisme et propose deux solutions.

**Q7.** Quelle est la différence entre WPA2-PSK, WPA3-SAE et le mode transition ? Dans quel cas utilises-tu chacun ?

**Q8.** Un seul smartphone ancien reste connecté en 2.4 GHz à −85 dBm, et depuis, tout le monde se plaint que « le Wi-Fi est lent ». Explique le mécanisme physique et donne deux remèdes côté AP.

**Q9.** Le WIDS te signale un « rogue AP » avec un SSID proche du tien. Détaille ta procédure avant toute contre-mesure.

**Q10.** La direction te demande « quand passe-t-on au Wi-Fi 7 en extérieur ? ». Construis une réponse argumentée en 3 points (besoin, parc clients, calendrier), en citant les vrais modèles eKit concernés.

## 114. Quiz : réponses commentées

**R1.** **Non.** L'AP761 est un AP **extérieur Wi-Fi 6 (802.11ax)** [constructeur] : bi-bande **2.4 GHz 2×2 + 5 GHz 2×2**, **1.775 Gbps** max agrégés (0.575 + 1.2 Gbps), canaux 20/40/80 MHz, 1024-QAM max. Pas de MLO, pas de 320 MHz, pas de 6 GHz, pas de 4096-QAM. Les modèles eKit Wi-Fi 7 d'extérieur sont l'**AP771** (3.57 Gbps) et l'**AP772E** (6.45 Gbps). *Si tu as répondu « Wi-Fi 7 » : relis l'avertissement en tête de guide — c'est l'erreur que ce guide existe pour éviter.*

**R2.** **Non.** Ce sont des ports **combo** : un seul actif à la fois, et **le SFP optique est prioritaire** s'il est branché [constructeur]. Brancher le SFP « pour tester » pendant que le cuivre fonctionne fait basculer l'AP sur la fibre — et si elle n'est pas prête, l'AP disparaît. L'alimentation reste **toujours** sur le RJ45 (PoE-In) : la fibre ne transporte pas d'énergie.

**R3.** Le 802.3af fournit ~12.95 W utiles, l'AP en veut 17.7 W max : le constructeur précise que **l'AP fonctionne en mode restreint (fonctions limitées)** en 802.3af [datasheet]. Symptômes : débit plafonné, USB/BLE coupés, puissance radio bridée. **Solution : passer en 802.3at (PoE+)** ou injecteur 30 W, et réserver 25 W par AP sur le budget du switch.

**R4.** Les AP se placent **en bordure de la zone, pointés vers l'intérieur**, secteur de 65° couvrant la cour — typiquement 2 AP en vis-à-vis ou en angles opposés selon la forme. On ne compte pas sur l'arrière car les antennes sont **directionnelles** : l'arrière est sourd (10–11 dBi concentrés vers l'avant [constructeur]). Hauteur 4–8 m, léger downtilt.

**R5.** **Canaux 1, 6 et 11** : les seuls 3 canaux 20 MHz qui ne se recouvrent pas en Europe (2.412–2.472 GHz). Le 40 MHz en 2.4 GHz est **à proscrire** en environnement partagé : il occupe 2 des 3 canaux propres, double le débit théorique mais divise la capacité du voisinage — et en extérieur, le voisinage est partout.

**R6.** **Mécanisme : DFS.** L'AP est sur un canal 52–140 ; à 14h (météo ? radar ?), il détecte une signature radar et **doit légalement quitter le canal** (chap. 41) → coupure pour tous les clients 5 GHz le temps de la bascule. **Solutions :** (1) migrer les usages critiques vers les canaux **non-DFS 36–48** ; (2) vérifier les logs pour confirmer (`radar detected`), noter l'AP en DFS dans le dossier de site, et si c'est quotidien, investiguer une source locale (chap. 65).

**R7.** **WPA2-PSK** : clé partagée, le standard PME — simple, mais la clé fuitée expose tout le monde, et vulnérable au brute-force hors ligne si clé faible. **WPA3-SAE** : résiste au brute-force hors ligne (échange par essai en ligne), impose AES+PMF — mais **exclut les clients non compatibles**. **Transition** : accepte les deux en parallèle — le mode de **migration** : modernes en SAE, anciens en WPA2, avec un plan pour passer en WPA3-only quand le parc suit.

**R8.** **Mécanisme : le client zombie et l'airtime.** Le Wi-Fi partage le **temps**, pas le débit : un client à 1 Mbps occupe le canal ~100× plus longtemps qu'un client à 100 Mbps pour la même quantité de données → toute la cellule ralentit. **Remèdes :** (1) **relever les débits de base** (interdire 1/2/5.5/11 Mbps en 2.4 GHz) pour empêcher l'association à débit minable ; (2) activer l'**airtime fairness** si supporté, ou limiter le débit par client.

**R9.** **Classifier avant de contre-attaquer :** (1) noter BSSID/SSID/canal/RSSI ; (2) chercher sa MAC dans les tables MAC des switches — **sur le filaire = interne** (remonter au port, aller voir : 90 % = équipement personnel) ; (3) si externe : usurpe-t-il ton SSID (evil twin → contre-mesure ciblée + logs + alerte direction) ou est-ce un voisin légitime (**ne rien faire d'agressif**, optimiser tes canaux) ; (4) noter l'incident dans le dossier de site.

**R10.** Réponse type : **(1) Besoin :** le Wi-Fi 7 apporte MLO (latence), 4K-QAM et efficacité — utile si on a des besoins de **densité/latence** que le Wi-Fi 6 ne satisfait pas ; sinon, un Wi-Fi 6 bien déployé suffit. **(2) Parc :** sans clients Wi-Fi 7, un AP Wi-Fi 7 = un AP Wi-Fi 6 cher — inventorier d'abord. **(3) Calendrier :** les AP761 ont 5–7 ans de vie devant eux ; la migration naturelle se fera au renouvellement (2029–2032), avec des **AP772E** (6.45 Gbps, 2.5G/10G, PoE bt) sur les zones denses et conservation des AP761 en couverture — en refaisant les budgets PoE et câblage.

**Barème indicatif :** 8+/10 = tu peux déployer et dépanner en autonomie. 5–7 = relis les chapitres de tes erreurs. < 5 = reprends le guide depuis le chapitre 12.

## 115. Cas pratique 1 : cour d'école / cour d'entreprise

**Contexte :** cour de 50 × 35 m, 150 élèves/salariés à la pause, usage = smartphones (réseaux sociaux, visio), 2 bâtiments en bordure.

**Solution :**
- **2× AP761** en angles opposés de la cour, secteurs pointés vers le centre (recouvrement 15–20 % à −67 dBm au milieu).
- Hauteur 5–6 m sur façades, downtilt léger.
- **Radio :** 2.4 GHz en 20 MHz (canaux 1 et 6), 5 GHz en 40 MHz (canaux 36 et 44 — non-DFS, pas de surprise à la récré).
- **SSID :** `ECOLE` (WPA2-PSK ou 802.1X selon le cas) + `ECOLE-INVITE` (portail, isolé). Pas de SSID « profs cachés » en plus — 2 SSID suffisent.
- **Sécurité :** débits de base relevés (pas de zombies à −85 dBm au fond de la cour), isolation client sur l'invité.
- **Capacité :** 150 clients / 2 AP = 75/AP en pic — dans la fourchette « navigation » (chap. 59). Si visio massive : passer à 3 AP.
- **PoE :** 2× 25 W = 50 W réservés sur le switch.

**Point de vigilance :** les pics sont **synchronisés** (la cloche sonne → 150 connexions en 2 minutes). Le DHCP doit encaisser : baux courts (2 h), scope dimensionné (un /23 si besoin, pas un /24 plein à 80 % en temps normal).

## 116. Cas pratique 2 : parking et contrôle d'accès

**Contexte :** parking de 120 × 40 m, barrières avec lecteurs de plaques connectés en Wi-Fi, 2 bornes de recharge VE connectées, smartphones des usagers.

**Solution :**
- **2–3× AP761** le long du parking, secteurs orientés dans l'axe (la forme allongée se prête aux secteurs directionnels).
- **SSID IOT** dédié en 2.4 GHz (canal 11, 20 MHz) pour les lecteurs de plaques et bornes : WPA2-PSK fort, VLAN 40 restreint (chap. 52), filtrage MAC en garde-fou (chap. 50).
- **SSID usagers** en 5 GHz pour les smartphones.
- **Criticité :** le contrôle d'accès ne doit pas tomber si le cloud est injoignable → vérifier le comportement local (le Wi-Fi continue, chap. 30), et **ne pas mettre les lecteurs sur un canal DFS** (une évacuation radar = barrières aveugles pendant la bascule).
- **Alimentation :** les lecteurs et l'AP sur le même onduleur ? **Décider** : si le parking doit rester contrôlé pendant une coupure, l'AP qui porte les lecteurs est critique (chap. 25).

**Point de vigilance :** les **carrosseries métalliques** font des réflexions et des zones d'ombre mouvantes (les voitures bougent). Le survey doit se faire **parking plein**, pas un dimanche vide — sinon les mesures sont optimistes.

## 117. Cas pratique 3 : entrepôt / zone logistique

**Contexte :** zone de chargement extérieure 80 × 50 m, douchettes/scanners Wi-Fi des caristes, 1 AP existant qui « ne passe pas » au fond.

**Solution :**
- **2× AP761** : un à chaque extrémité, secteurs vers le centre. Les scanners ont besoin de **roaming propre** (chap. 57) : 802.11r, même SSID/sécurité/VLAN.
- **Priorité à la fiabilité** : 5 GHz en 40 MHz sur canaux 36–48 (non-DFS), 2.4 GHz en 20 MHz pour les vieux scanners.
- **Scanners anciens** : beaucoup sont en Wi-Fi 4, voire avec des pilotes exotiques → **tester le modèle exact** avant de généraliser, prévoir un SSID dédié en WPA2-PSK si le 802.1X les fait trébucher.
- **Environnement :** racks métalliques = réflexions ; poussière = contrôle visuel semestriel renforcé (l'IP68 protège, mais les connecteurs et fixations s'encrassent).
- **Chariots élévateurs** : prévoir la **hauteur** (un AP à 4 m au-dessus d'une allée de chariots = collision possible) et les **vibrations** (serrage contrôlé).

**Point de vigilance :** les scanners envoient peu de données mais **n'acceptent pas les coupures** (une coupure = une palette non scannée = un litige). Le test de référence, c'est le **ping continu en chariot** sur tout le parcours (chap. 57), pas un speedtest.

## 118. Cas pratique 4 : terrasse d'hôtel / camping

**Contexte :** terrasse 40 × 25 m + piscine, 80 clients en été, usage = streaming vidéo, visio, réseaux sociaux. Équipement accessible au public.

**Solution :**
- **1–2× AP761** selon la forme (1 suffit en couverture, 2 pour la capacité en haute saison).
- **SSID `HOTEL`** (WPA2-PSK, clé changée chaque saison) + **portail captif** pour les visiteurs de passage (chap. 49) avec les mentions légales.
- **Isolation client** obligatoire (chap. 53), **limitation de débit par client** (10–15 Mbps : largement assez pour du streaming, ça protège des accapareurs).
- **Sécurité physique** : AP hors de portée (> 3 m), câble antivol, fixation vérifiée (chap. 108) — un AP arraché = un AP à remplacer + une config à révoquer.
- **Saisonnalité** : en hiver, baisser la puissance ou éteindre le SSID la nuit (économies + moins de pollution pour les voisins).

**Point de vigilance :** l'**eau** (piscine, arrosage) + le **soleil** (surchauffe, chap. 91) + le **sel** si bord de mer (corrosion, chap. 102). La visite semestrielle est **non négociable** sur ce type de site.

## 119. Cas pratique 5 : relais entre deux bâtiments (point à point)

**Contexte :** deux bâtiments distants de 150 m, pas de fourreau possible (route entre les deux), besoin de relier le réseau du bâtiment B.

**Solution honnête d'abord :** l'AP761 **n'est pas une antenne point-à-point dédiée** — ses antennes sont des secteurs 65°, pas des faisceaux étroits. Pour un vrai bridge 150 m, une paire d'antennes directionnelles dédiées (ou de la fibre aérienne) est plus propre. **Mais** si c'est du dépannage temporaire ou un faible débit :

- **2× AP761** en vis-à-vis, chacun pointé vers l'autre (lobe principal dans l'axe).
- **SSID dédié** au bridge, WPA2-PSK fort (ou WPA3-SAE), **masqué** (chap. 43 — ici justifié : ce n'est pas un SSID public).
- **VLAN trunké** sur le lien pour faire transiter plusieurs réseaux.
- **Débit attendu :** correct en 5 GHz 40 MHz à 150 m en vue directe (100–200 Mbps utiles), mais **sans garantie** : la pluie, le brouillard et les oiseaux sur l'axe dégradent.

**Limites à annoncer clairement :**
- Pas de redondance : si un AP tombe, le lien tombe.
- La latence/gigue est celle du Wi-Fi partagé, pas d'une fibre : **ne pas y faire passer de la téléphonie critique** sans test.
- **Solution pérenne = fibre** (aérienne sur poteaux ou forage dirigé sous la route). Le bridge Wi-Fi est un pansement, pas une artère.

## 120. Pour aller plus loin : docs, outils, formations

**Documentation officielle (à consulter en premier pour chaque valeur « à vérifier ») :**
- Datasheet **Huawei eKitEngine AP761** (réf. 02355VFB) — la source des valeurs constructeur de ce guide.
- **Country Code & Channel Compliance Table** Huawei — les canaux autorisés par pays (indispensable avant tout déploiement hors France).
- Guide de configuration WLAN de **ta version logicielle exacte** (`display version` sur l'AP) — la syntaxe CLI varie.
- Release notes de chaque firmware avant mise à jour (chap. 74).

**Outils terrain :**
- App **HUAWEI eKit** (onboarding, supervision) + **CloudCampus** (BLE).
- App d'analyse Wi-Fi sur smartphone (scan, RSSI) — pour le survey simplifié (chap. 60).
- **iPerf3** + un serveur sur le LAN — la mesure de débit qui isole le Wi-Fi (chap. 81).
- Testeur de câble qualifiant — le meilleur investissement anti-pannes « mystères » (chap. 24).

**Tes autres guides (compléments directs) :**
- `~/workspace/user/files/onduleurs_ups_guide.md` — dimensionner l'onduleur avec la charge PoE (chap. 25).
- `~/workspace/user/files/zabbix_guide.md` — supervision SNMP des AP (chap. 68).
- `~/workspace/user/files/debian_ubuntu_guide.md` — serveur RADIUS (FreeRADIUS), syslog, NTP (chap. 48, 69, 70).
- `~/workspace/user/files/proxmox_guide.md` — héberger les VM de supervision/RADIUS.

**Formations et communautés :**
- **Huawei eKit** : webinaires et certifications en ligne (portail Huawei) — la gamme évolue vite (le Wi-Fi 7 est arrivé en 2024–2025).
- **CWNA** (Certified Wireless Network Administrator) — la référence indépendante pour la radio Wi-Fi : si tu veux passer du « je dépanne » au « je conçois », c'est LA formation.
- Communautés : forums Huawei Enterprise, groupes d'intégrateurs eKit — pour les retours de terrain sur les firmwares (chap. 74).

**Veille à mettre en place :**
- S'abonner aux **avis de sécurité** Huawei (failles Wi-Fi : KRACK en 2017, FragAttacks en 2021 — il y en aura d'autres).
- Suivre l'**ouverture du 6 GHz** dans l'UE (chap. 19) : le jour où l'outdoor y aura droit, le dimensionnement changera.
- Garder un œil sur les **prix du Wi-Fi 7** : quand l'AP772E baissera, le calcul du renouvellement (chap. 105) changera.

---

*Fin du guide — Huawei eKit AP761. Rédigé le 27/09/2026 à partir des fiches constructeur vérifiées ce jour-là. Les valeurs marquées « à vérifier sur la fiche du modèle exact » doivent être re-validées avant tout appel d'offres ou déploiement. Bon chantier.* 🔧📡
