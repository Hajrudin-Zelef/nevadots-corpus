# GUIDE ULTRA-COMPLET — PLATEFORME HUAWEI eKit (PME / DISTRIBUTION)

> **Public :** Zelef, chef de service systèmes & énergies — technicien de terrain, responsable d'équipe.
> **Ton :** concret, direct, sans jargon inutile. Ce guide est fait pour être utilisé sur site, pas pour être beau sur une étagère.
> **Périmètre vérifié par recherche web (septembre 2026) :** la plateforme Huawei eKit, lancée en 2023 pour le marché de la distribution mondiale, couvre les PME via les gammes **eKitEngine** (réseau : passerelles AR, points d'accès Wi-Fi, switchs, pare-feux USG), **eKitOptix** (MiniFTTO fibre), **eKitStor** (stockage) et **IdeaHub** (collaboration). Ce guide se concentre sur le **datacom** : AR, AP, switchs, USG6000F-S, app eKit et cloud SNC.
> **Règle d'honnêteté de ce guide :** tout ce qui vient des fiches techniques constructeur est sourcé. Quand une info dépend de la version logicielle ou n'a pas pu être vérifiée, c'est écrit noir sur blanc : **« à vérifier sur la documentation officielle »**. Aucune procédure n'est inventée.

---

## SOMMAIRE

- **Sections 1 à 15** — L'écosystème eKit : positionnement, gammes, modèles réels
- **Sections 16 à 30** — L'app eKit : installation, compte, onboarding, organisation par sites
- **Sections 31 à 45** — Le cloud eKit (SME Network Center) : comptes, rôles, multi-sites, limites honnêtes
- **Sections 46 à 60** — Scénarios de déploiement types complets (bureau 20 postes, hôtel, boutique, entrepôt)
- **Sections 61 à 75** — Interopérabilité, migration, bonnes pratiques (adressage, VLAN, SSID, PoE, ordre de mise en service)
- **Sections 76 à 90** — Supervision, limites vs vrai NMS, quand passer au Datacom entreprise
- **Sections 91 à 100** — Sauvegarde, mises à jour, dépannage (15+ cas terrain)
- **Sections 101 à 115** — Maintenance, sécurité, pense-bête de poche, glossaire, quiz, pour aller plus loin

---

## 1. eKit, c'est quoi exactement ?

Huawei eKit est la marque **distribution** de Huawei pour les PME, lancée en 2023. Traduction terrain : ce n'est pas la gamme « opérateur » ni la gamme « grande entreprise » (celle-là, c'est Datacom avec iMaster NCE, les switchs S600 et compagnie). eKit, c'est la gamme que ton **distributeur local** te vend, avec :

- des produits **pré-packagés par scénario** (bureau, hôtel, école, commerce) ;
- une gestion pensée pour des gens qui ne sont **pas des experts réseau** : l'app mobile eKit et le cloud SME Network Center (SNC) ;
- une promesse constructeur : **management app/cloud gratuit, sans licence** (c'est ce qu'annoncent Huawei et ses distributeurs — voir section 33 pour les nuances).

En chiffres (annoncés par Huawei au MWC 2025) : plus de **70 pays**, plus de **400 partenaires Gold distribution**, croissance de l'activité de l'ordre de 30 %. L'Afrique (Afrique du Sud en tête) fait partie des marchés les plus dynamiques. Moralité : tu trouveras du stock et du support, ce n'est pas une gamme fantôme.

## 2. Positionnement : pour qui, pour quoi — et surtout PAS pour quoi

**C'est fait pour :**
- PME de 5 à ~200 utilisateurs ;
- sites uniques ou multi-sites simples (boutiques, agences, hôtels, écoles, cliniques) ;
- équipes IT réduites ou prestataires qui déploient vite ;
- budgets serrés mais besoin de Wi-Fi 6/7 propre et de PoE.

**Ce n'est PAS fait pour :**
- les campus de plusieurs milliers d'utilisateurs avec roaming fin et 802.1X complexe ;
- les datacenters (pas de 25/40/100G sérieux dans la gamme eKit — le S620 monte en 25G/100G mais reste positionné PME) ;
- les environnements qui exigent du routage dynamique avancé (OSPF/BGP multi-zones), du MPLS, du VXLAN ;
- les sites où la **redondance matérielle** (double alim, stacking, VRRP/HSRP) est contractuelle.

Retiens la phrase qui te sauvera en réunion : *« eKit, c'est du solide pour du simple. Dès que le besoin devient complexe, on bascule sur du Datacom entreprise. »* (Critères de bascule détaillés en sections 83-90.)

## 3. Les quatre piliers de l'écosystème eKit

| Pilier | Nom commercial | Contenu | Ce guide couvre |
|---|---|---|---|
| Datacom | **eKitEngine** | Passerelles AR, AP Wi-Fi, switchs, pare-feux USG | Oui, en détail |
| Fibre bureau | **eKitOptix** (MiniFTTO) | F700D (AP Wi-Fi 7 optique 3-en-1), FG736 | Oui, survol + cas hôtel |
| Stockage | **eKitStor** | SSD Xtreme 200E, Shield 210 portable | Non (hors périmètre réseau) |
| Collaboration | **IdeaHub** | S3, B3 (écrans interactifs 4K) | Non (mention uniquement) |

Le datacom eKitEngine est le cœur de ton métier de déploiement. Les trois autres piliers existent, tu dois savoir qu'ils existent, mais ton réseau s'arrête au datacom.

## 4. Les passerelles eKitEngine AR : AR180, AR180 Plus, AR180 Pro, AR280, AR281

La passerelle AR eKit, c'est la « box pro » : **routage + switch + Wi-Fi + gestion Internet** dans un seul boîtier compact. D'après les fiches techniques constructeur (datasheets AR180/AR280, 2025) :

| Modèle | Ports | Wi-Fi | Capacité annoncée | PoE | Refroidissement |
|---|---|---|---|---|---|
| **AR180** | 4× GE LAN + 1× 2.5GE WAN | Wi-Fi 7 double bande (2,4/5 GHz, 2×2) | ~100 terminaux, 8 tunnels IPsec | Non | Passif (sans ventilateur) |
| **AR180 Plus** | 4× GE LAN + 1× 2.5GE WAN | Wi-Fi 7 double bande | ~100 terminaux, 8 tunnels IPsec | Non | Passif |
| **AR180 Pro** | 5× GE (configurables) + 1× 2.5GE | Wi-Fi 7 double bande | ~150 terminaux, 16 tunnels IPsec, **8 AP max, 32 équipements gérés**, 256 VLAN | Non | Passif (boîtier métal) |
| **AR280** | 4× GE (1 LAN dédié + 3 LAN/WAN) + 1× 2.5GE (WAN/LAN) | — (pas de Wi-Fi intégré d'après la fiche) | ~150 terminaux, 16 tunnels IPsec, **16 AP max, 32 équipements gérés** | Non (conso max 49 W dont 41 W PoE — à vérifier : la fiche mentionne une capacité PoE, probablement sur un modèle dérivé) | Passif |
| **AR281** | à vérifier sur la documentation officielle | — | Annoncé en 2026 comme « 1 appareil = 5 fonctions » : routage, **NVR**, **WAC**, accès ligne privée, gestion du comportement en ligne | à vérifier | à vérifier |

**Points terrain à retenir :**
- Débit VPN IPsec annoncé : **200 Mbit/s** (AR180/AR280). C'est une valeur labo. Si ton client fait du VPN site-à-site intensif, dimensionne large.
- Débit de contrôle de bande passante (egress) : **2 Gbit/s** annoncé.
- Les AR180 ont **4 antennes Wi-Fi externes intelligentes** (non démontables, livrées montées), gain max 3 dBi en 2,4 GHz / 4 dBi en 5 GHz, puissance d'émission max 23 dBm (réglementation locale applicable).
- **Pas de port console** sur AR180/AR280 d'après la fiche technique. Traduction : pas de plan B série si tu perds l'accès IP. Le reset physique existe (bouton reset), mais il coupe le service — voir section dépannage.
- Alimentation : adaptateur secteur externe, 100-240 V. Pas de redondance d'alimentation. Pour un site critique, prévois un onduleur (lien direct avec ton guide onduleurs).
- Température de fonctionnement AR180 : fiche à vérifier selon modèle (la série accepte typiquement 0 à 40/45 °C — **à vérifier sur la documentation officielle** du modèle exact que tu déploies).

## 5. Les points d'accès eKitEngine : la gamme réelle

Voici les modèles **réellement existants** d'après les fiches constructeur et les catalogues distributeurs (pas de modèles inventés) :

### 5.1. Wi-Fi 6 d'entrée de gamme (le gros des déploiements PME)

| Modèle | Usage | Wi-Fi | Débit max agrégé | Ports | PoE-in | Particularités |
|---|---|---|---|---|---|---|
| **AP361** | Intérieur (plafond/mur) | Wi-Fi 6, 2,4 GHz 2×2 + 5 GHz 2×2 | **1,775 Gbit/s** (575 + 1200 Mbit/s) | 1× GE | 802.3af (~9 W) | Antenne intelligente intégrée, modes Fit/Fat/Cloud, USB pour IoT (ZigBee/RFID selon fiche) |
| **AP266** | Intérieur | Wi-Fi 6 | à vérifier sur la documentation officielle | à vérifier | à vérifier | Gestion cloud, PSK + portail captif intégrés **sans WAC ni serveur d'authentification** (gros avantage PME) |
| **AP761** | **Extérieur** | Wi-Fi 6 (AX1800 : 574 + 1201 Mbit/s) | 1,775 Gbit/s | 1× GE + **1× SFP** | 802.3at recommandé (af = fonctions limitées), conso max 17,7 W | BLE 5.2 (gestion, localisation, IoT), MU-MIMO, Beamforming, TWT |

### 5.2. Wi-Fi 7 (la vague 2025-2026)

| Modèle | Usage | Wi-Fi | Particularités |
|---|---|---|---|
| **AP572** | Intérieur haute densité (salles de classe, salles de réunion) | Wi-Fi 7 | Positionné « haute densité » par Huawei |
| **AP673E** | Intérieur haute densité | Wi-Fi 7 | Salles de classe / grandes salles de conférence |
| **AP673H (iGuard)** | Intérieur (bureaux, hôtels, santé, éducation) | Wi-Fi 7 **tri-bande** 2,4 (2×2) + 5 (2×2) + **6 GHz (4×4)**, 8 flux spatiaux, **13,66 Gbit/s** | **Détection de caméras espion** par IA (voir section 29) — la détection spycam ne fonctionne qu'en **gestion locale** (pas via cloud, pour des raisons de confidentialité) |
| **AP772** | Extérieur | Wi-Fi 7 | Antenne **omnidirectionnelle 360°** |
| **AP772E** | Extérieur longue distance | Wi-Fi 7 | Antenne **directionnelle** |

**Règle de choix rapide :**
- Bureau/boutique standard → **AP361** (le best-seller, prix contenu, largement suffisant).
- Extérieur (terrasse, parking, piscine d'hôtel) → **AP761** (Wi-Fi 6) ou **AP772/AP772E** (Wi-Fi 7).
- Salle de réunion dense, école → **AP572/AP673E**.
- Hôtel haut de gamme / salle de direction (argument confidentialité) → **AP673H iGuard**.
- Fibre jusqu'au bureau/chambre → **eKitOptix F700D** (MiniFTTO, section 14).

## 6. Les switchs eKitEngine : S110, S220, S310, S620

### 6.1. Tableau de la gamme réelle

| Série | Positionnement | Modèles connus | PoE |
|---|---|---|---|
| **S110** | Entrée de gamme, petits sites | modèles à vérifier sur la documentation officielle | Budget PoE inférieur au S310 (le distributeur cdr.pl le cite comme référence basse) |
| **S220** | PME, L2 managé | **S220-8P4S** : 8× GE + 4× SFP 1G, **125 W PoE+**, 24 Gbit/s, 18 Mpps | 802.3af/at, Fast PoE, Perpetual PoE |
| **S310** | Cœur de gamme PME, L2+ | **S310-48P4S** : 48× GE PoE+ + 4× SFP 1G, **380 W**, 104 Gbit/s / 77 Mpps ; **S310-48P4X** : 48× GE PoE+ + **4× SFP+ 10G**, 380 W, 176 Gbit/s / 131 Mpps ; **S310-24J4X / 48J4X** : 24 ou 48× **2.5GE SFP** (tout-optique) + 4× SFP+ 10G | 802.3af/at sur tous les ports (48P), 380 W |
| **S620** | Haut de gamme eKit | **S620-24T16X8Y2CZ** : 24× GE + 16× 10GE + 8× 25GE + 2× 100GE | à vérifier |

**Détails terrain sur le S310 (le switch que tu déploieras le plus) :**
- Les 4 uplinks SFP/SFP+ acceptent modules optiques GE/10GE, jarretières cuivre DAC et câbles de stacking dédiés (**stacking zéro-configuration** supporté d'après la fiche).
- Mise à jour « intelligente » via **HOUP** (Huawei Online Upgrade Repository) : le switch récupère lui-même son chemin de mise à jour — mise à jour en un clic, avec pré-chargement pour réduire la coupure.
- Gestion : **app eKit (cloud) OU locale** (CLI, web, SNMP, SSH) — les deux modes sont **commutables** selon la fiche. C'est important : tu n'es pas enfermé dans le cloud.
- Protection surtension : ±7 kV en mode commun sur les ports de service, ±6 kV sur l'alimentation (S310-48P4X). Bien pour les sites à réseau électrique capricieux, mais ça ne remplace pas un parafoudre en tête d'installation.
- MTBF annoncé : ~48 ans, dispo > 99,999 %. Valeurs constructeur, à prendre comme ordre de grandeur.

### 6.2. Lecture d'une référence : S310-48P4X

- **S310** = série ; **48** = 48 ports descendants ; **P** = PoE ; **4X** = 4 uplinks SFP+ 10G (un **S** final = SFP 1G ; **J** = ports SFP 2.5GE tout-optique ; **T** = ports cuivre).
- Réflexe : devant une référence inconnue, décode-la au lieu de la googler en panique.

## 7. Le pare-feu eKitEngine USG6000F-S : S125 / S150 / S200

Huawei positionne l'USG6000F-S comme le **pare-feu IA pour PME** : « un boîtier = routeur + switch + antivirus » (formule distributeur). D'après la fiche technique V600R025 :

| Modèle | Débit pare-feu IPv4 | Débit NGFW (FW+SA+IPS) | Débit protection complète (+AV) | IPsec | Utilisateurs recommandés |
|---|---|---|---|---|---|
| **S125** | 8/8/3,6 Gbit/s | 1,7 Gbit/s | 1,4 Gbit/s (HTTP 100K) / 1 Gbit/s (mix réel) | 3,7 Gbit/s, 2000 tunnels | ~600 |
| **S150** | 12/12/4 Gbit/s | 2,1 Gbit/s | 1,8 / 1,2 Gbit/s | 3,5 Gbit/s, 4000 tunnels | ~1000 |
| **S200** | 16/16/5 Gbit/s | 3 Gbit/s | 2,5 / 1,8 Gbit/s | 5,6 Gbit/s, 4000 tunnels | ~2500 |

**À retenir pour le dimensionnement :** le débit qui compte, c'est le **débit « enterprise mix » avec toutes les protections activées** (1 à 1,8 Gbit/s selon modèle), pas le débit pare-feu brut. Si le client a une fibre 2 Gbit/s et active tout, le S125 sera le goulot. Fonctions : IPS, antivirus, filtrage URL (base de 500 M d'URL), anti-spam, SSL VPN (100 utilisateurs par défaut, jusqu'à 1000-2000 en licence), IPsec, portail captif local, SSO RADIUS/AD. Mises à jour de signatures via le centre de sécurité Huawei.

## 8. eKitOptix (MiniFTTO) : la fibre jusqu'à la chambre / au bureau

Le **MiniFTTO** (Fiber To The Office), c'est l'argument « hôtel » d'eKit : remplacer le faisceau de câbles (TV + téléphone + Wi-Fi) par **une seule fibre** jusqu'à chaque chambre/bureau, terminée par un boîtier discret qui fait Wi-Fi + ports Ethernet (+ parfois POTS/TV selon modèle).
- **eKitOptix F700D** : AP Wi-Fi 7 optique « 3-en-1 » (fibre + Wi-Fi + switch local).
- **eKitOptix FG736** : nouvelle génération d'AP Wi-Fi 7 optique.
- Cas d'usage roi : **hôtellerie** (une fibre par chambre, esthétique, pas de goulotte), bureaux neufs, cliniques.
- Contrainte : ça suppose une infrastructure optique passive (OLT en tête, splitters) — ce n'est pas « juste un AP », c'est une architecture. Ne le propose que si le câblage est à créer ou à refaire, ou si le client veut du tout-fibre. Détail d'architecture en section 52.

## 9. Wi-Fi Shield, iGuard et les arguments sécurité « différenciants »

Huawei pousse trois arguments marketing que tu dois savoir **expliquer honnêtement** au client :
1. **Wi-Fi Shield** : présenté comme une protection « zéro-écoute » intégrée au Wi-Fi 7 eKit. Concrètement : chiffrement et mécanismes anti-espionnage renforcés. Ne le vends pas comme « inviolable » — c'est une couche de plus, pas une baguette magique.
2. **iGuard / AP673H** : détection de **caméras espion** par analyse des ondes (CSI — Channel State Information) + IA, y compris des caméras **non connectées au Wi-Fi** (qui stockent en local ou transmettent en 4G/5G). La fiche constructeur précise : la détection spycam fonctionne en **gestion locale uniquement** (confidentialité des données). Il existe aussi un détecteur portable **DF10** avec appli mobile iGuard (mode « meeting guard », ~1,5 h d'autonomie annoncée). Argument fort pour hôtellerie haut de gamme et salles de direction.
3. **App/cloud « 100 % gratuits, zéro licence »** : c'est l'argument économique central (voir nuances section 33).

## 10. Où acheter, où se documenter (sources officielles)

- Portail officiel : **ekit.huawei.com** (cité dans toutes les fiches techniques).
- Documentation technique : **support.huawei.com** (fiches, guides d'installation, notes de version) et **e.huawei.com** (présentations solutions).
- **À vérifier sur la documentation officielle** systématiquement avant un déploiement : la fiche exacte du modèle (les gammes évoluent vite — AR281, AP772, F700D sont des nouveautés 2025-2026), les notes de version du firmware que tu vas installer, et la disponibilité des fonctions cloud dans ton pays.
- Ton **distributeur Gold eKit** : c'est ton premier niveau de support (stock, RMA, firmwares). Garde son contact dans ton téléphone — section 101.

## 11. Vocabulaire eKit : Fit, Fat, Cloud — ne plus confondre

| Mode | Signification | Quand l'utiliser |
|---|---|---|
| **Fit** | L'AP est piloté par un contrôleur (WAC) | Avec un AR eKit qui fait office de WAC (l'AR280/AR281 intègre la fonction WAC), ou un WAC externe |
| **Fat** | L'AP est autonome, configuré unitairement | Petit site, dépannage, ou quand tu veux garder la main en local |
| **Cloud** | L'AP est piloté par le cloud eKit (app/SNC) | Le mode standard des déploiements eKit multi-sites |

Un AP eKit peut **changer de mode** selon la fiche (AP361 : Fit, Fat, cloud). En pratique eKit : tu onboardes en **Cloud** via l'app, et tu bascules en local si besoin. L'erreur classique : laisser des AP en Fat « parce que ça marche » puis perdre la supervision centralisée. Choisis ton mode **avant** le déploiement et tiens-le.

## 12. Ce que l'app eKit n'est PAS

Pour cadrer les attentes (les tiennes et celles du client) :
- Ce n'est pas un **NMS** complet (pas de NetFlow/sFlow, pas de corrélation d'événements avancée, pas d'API publique documentée à ma connaissance — **à vérifier sur la documentation officielle**).
- Ce n'est pas un outil de **planification radio** (pas de heatmap prédictive comme Ekahau) : le placement des AP reste ton métier, au mètre et au plan.
- Ce n'est pas un **contrôleur local redondé** : si Internet tombe, la gestion cloud tombe (le réseau local, lui, continue de fonctionner — le data plane ne dépend pas du cloud, voir section 38).

## 13. Les 9 secteurs verticaux eKit (pour parler au client)

Huawei décline eKit en solutions par secteur : **bureaux, hôtellerie, éducation, santé, commerce/retail, restauration**, et d'autres. Intérêt pratique : quand tu réponds à un besoin, cherche la « solution eKit » du secteur — tu y trouveras une topologie de référence et une liste de matériel type déjà pensées par le constructeur. Ça t'évite de réinventer la roue et ça rassure le client (« c'est la solution hôtel officielle, pas un bricolage »).

## 14. eKitStor et IdeaHub : ce que tu dois en savoir (en 2 minutes)

- **eKitStor Xtreme 200E** (SSD) et **Shield 210** (stockage portable) : pour les postes et la sauvegarde locale. L'AR281 intègre une fonction **NVR** avec compression « SuperCoding 3.0 » annoncée à -85 % de stockage vidéo — intéressant si le client couple réseau + vidéosurveillance.
- **IdeaHub S3 / B3** : écrans collaboratifs 4K pour salles de réunion (visio, tableau blanc, projection sans fil). Ils se connectent à ton réseau eKit comme des clients Wi-Fi/filaires gourmands — prévois-le dans le dimensionnement (une visio 4K, c'est du débit réservé).
- Tu n'as pas besoin d'être expert de ces gammes, mais tu dois savoir qu'elles existent quand le client dit « et pour nos salles de réunion ? ».

## 15. Check-list « avant de proposer eKit » (à valider avec le client)

- [ ] Nombre d'utilisateurs et de terminaux (avec marge +30 % à 3 ans) ?
- [ ] Surface, nombre de pièces/étages, nature des murs (béton, placo, verre) ?
- [ ] Débit Internet disponible et type de raccordement (fibre, 4G/5G, ADSL) ?
- [ ] Besoin de PoE : combien d'AP, caméras, téléphones IP ?
- [ ] Invités / portail captif ? (oui → prévoir la fonction portail de l'AP ou de l'USG)
- [ ] Vidéosurveillance sur le même réseau ? (oui → VLAN dédié + NVR, attention au dimensionnement)
- [ ] Multi-sites ? (oui → eKit cloud prend tout son sens)
- [ ] Contraintes de sécurité : invités isolés, 802.1X, VPN site-à-site ?
- [ ] Le client a-t-il déjà du matériel à réutiliser ? (voir section 61)
- [ ] Qui administre au quotidien ? (si « personne », l'app eKit + un contrat de maintenance, c'est toi)

---

## 16. L'app eKit : c'est quoi, où la trouver

L'**app HUAWEI eKit** (Android/iOS) est l'outil central du déploiement : c'est par elle que tu **onboardes** les équipements (tu les déclares au cloud), que tu configures les sites, et que tu fais la maintenance courante. D'après les fiches techniques (AP361, AP266, switchs S310/S620) :
- déploiement **par Wi-Fi** : le téléphone se connecte au Wi-Fi de gestion de l'équipement, l'app configure le projet réseau, les équipements sont automatiquement onboardés et gérables à distance ;
- déploiement **par scan de code-barres** : tu scannes le numéro de série (SN) sur le châssis, l'info est synchronisée vers le système eKit, l'équipement est onboardé — idéal pour ajouter des équipements à un projet existant ou quand le modèle ne supporte pas le déploiement par Wi-Fi.
- Après le déploiement, « davantage d'opérations de maintenance du projet peuvent être effectuées sur l'app » (fiche AP361).

**Installation :** cherche « HUAWEI eKit » sur le Play Store / App Store. Vérifie que c'est bien l'app officielle Huawei (éditeur Huawei). **À vérifier sur la documentation officielle** : la disponibilité de l'app et du cloud eKit dans ton pays — certaines fonctions cloud peuvent varier selon la région.

## 17. Création du compte : la base à ne pas rater

1. Crée **un compte par structure d'intervention**, pas un compte par technicien avec son adresse perso. Si ton technicien part, tu perds l'accès aux sites clients.
2. Recommandation terrain : une adresse e-mail **générique de ton service** (type `reseau@tonentreprise.xx` — exemple fictif) comme compte « propriétaire », puis tu invites les techniciens avec leurs comptes nominatifs en rôle restreint (rôles : section 34).
3. Active la **double authentification** si l'app la propose (**à vérifier sur la documentation officielle** — si elle n'existe pas encore, mot de passe long unique + gestionnaire de mots de passe).
4. Note le compte propriétaire dans ton **registre des accès** (section 106) : client, site, compte, rôle, date. C'est ton assurance-vie le jour où le client t'appelle dans 2 ans.

## 18. Organisation par sites : le concept central

L'app organise tout par **sites** (un site = un bâtiment, une boutique, un hôtel). Chaque site contient ses équipements, ses SSID, ses VLAN, ses alertes. Règles d'organisation :
- **1 site physique = 1 site eKit.** Ne mets jamais deux bâtiments dans le même site « pour simplifier » : tu perdrais la supervision par bâtiment et les configs divergeraient.
- Nomme les sites de façon **explicite et durable** : `CLIENT – Ville – Bâtiment` (ex. fictif : `HOTEL PALM – Douala – Bâtiment A`). Pas de « Site 1 », « Test », « Nouveau site (2) ».
- Un compte peut gérer **plusieurs sites** (multi-sites, section 35) : c'est tout l'intérêt pour un prestataire qui suit 10 boutiques d'une enseigne.
- Documente pour chaque site : adresse, contact sur place, plan de câblage, plages d'adresses (section 66).

## 19. Onboarding par Wi-Fi : la méthode rapide (AP)

D'après la fiche technique (déploiement « Wi-Fi-based ») :
1. Mets l'AP sous tension (PoE ou adaptateur), attends qu'il diffuse son **Wi-Fi de gestion** (SSID de setup d'usine).
2. Sur le téléphone, ouvre l'app eKit, crée ou sélectionne le **site**, lance l'ajout d'équipement en mode Wi-Fi.
3. Connecte le téléphone au Wi-Fi de gestion de l'AP quand l'app le demande.
4. L'app pousse la configuration du projet (SSID, mot de passe, paramètres réseau) et **onboarde automatiquement** l'équipement au cloud.
5. Vérifie dans l'app que l'AP apparaît **en ligne** sur le site avant de passer au suivant.

**Conseil terrain :** fais l'onboarding **au bureau avant d'aller sur site** quand c'est possible (pré-staging) : tu scannes/configures tout au calme, sur site tu ne fais que brancher. Tu divises le temps d'intervention par deux.

## 20. Onboarding par scan de code-barres / QR : la méthode robuste

D'après la fiche technique (déploiement « barcode scanning-based ») :
1. Dans l'app, sur le site concerné, choisis l'ajout par **scan du SN** (le numéro de série sur l'étiquette du châssis ou de l'emballage).
2. Scanne chaque équipement (AP, switch, passerelle). L'app synchronise les SN vers le système eKit.
3. Quand les équipements sont branchés et ont accès à Internet, ils **remontent automatiquement** au cloud et rejoignent le site.
4. Cas d'usage typiques : ajout de capacité à un projet existant ; équipement dont le firmware ne supporte pas le déploiement par Wi-Fi (dans ce cas, **mets à jour d'abord**, ou passe par le scan).

**Réflexe :** photographie les étiquettes SN **avant** de fixer les AP au plafond. Une fois l'AP à 3 mètres de haut, tu ne veux pas ressortir l'échelle pour lire un SN.

## 21. Onboarding d'un switch ou d'une passerelle AR

Même logique que les AP, via l'app :
1. **Câblage d'abord** : la passerelle AR en tête (port WAN vers le modem/ONT de l'opérateur), le switch derrière (uplink vers l'AR), les AP sur les ports PoE du switch.
2. Allume dans l'ordre : **modem opérateur → AR → switch → AP** (l'ordre de mise en service détaillé en section 75).
3. Scanne ou déclare chaque équipement dans le site via l'app.
4. La passerelle AR récupère son accès Internet (DHCP ou PPPoE selon l'opérateur — paramètres à saisir dans l'app), puis les équipements derrière elle remontent au cloud **à travers elle**.
5. **Point de vigilance :** si l'AR n'a pas Internet, **rien ne remonte**. En cas d'échec global d'onboarding, commence toujours par vérifier l'accès Internet de la passerelle (section dépannage 91).

## 22. Premier paramétrage d'un site : l'ordre qui évite les allers-retours

1. Crée le site (nom explicite, adresse, fuseau horaire).
2. Déclare la **passerelle** (accès Internet : DHCP/PPPoE/IP statique + DNS).
3. Crée les **VLAN** (section 67) : gestion, utilisateurs, invités, IoT/caméras, voix si besoin.
4. Crée les **SSID** (section 68) : un SSID « staff » (WPA2/WPA3), un SSID « invités » (portail captif ou PSK simple, isolé), éventuellement un SSID IoT.
5. Déclare les **switchs** (activation PoE, VLAN natifs sur les ports AP).
6. Déclare les **AP** (association aux SSID, canaux/puissance — laisse l'auto par défaut au début, optimise après).
7. Teste : un client sur chaque SSID, un ping vers Internet, un test de débit.
8. **Sauvegarde** : exporte la configuration (section 89) et note-la dans le dossier du site.

## 23. Ce que tu configures dans l'app au quotidien

D'après les fiches techniques, l'app permet après déploiement : configuration, monitoring, inspection des équipements sur le cloud. Concrètement, attends-toi à pouvoir gérer :
- état en ligne/hors ligne des équipements par site ;
- SSID (création, PSK, diffusion ou non du SSID) ;
- paramètres radio de base (bande, canal, puissance — niveau de détail **à vérifier sur la documentation officielle** selon version de l'app) ;
- VLAN et adressage de base ;
- PoE (activation/désactivation par port — **à vérifier sur la documentation officielle**) ;
- redémarrage à distance d'un équipement ;
- mise à jour firmware (section 90).
Ce que tu ne feras **pas** dans l'app : du tuning radio fin, des ACL complexes, du routage avancé — pour ça, bascule l'équipement en gestion locale (CLI/web) si le modèle le permet (les switchs S310/S620 le permettent d'après leurs fiches).

## 24. L'app pour le technicien sur site : les bons réflexes

- **Mode hors-ligne partiel :** prépare tout ce qui est préparables avant de partir (site créé, équipements pré-déclarés par scan des cartons). Sur site, tu n'as besoin que d'Internet sur ton téléphone pour finaliser.
- **Batterie externe** dans la sacoche : l'app + le flash + les photos de chantier vident un téléphone en une demi-journée.
- **Photos systématiques** : baie, étiquettes, cheminement des câbles, position des AP. Tu les ranges dans le dossier du site — dans 6 mois, tu remercieras ton toi du passé.
- **Ne rends jamais un site « à l'aveugle »** : avant de partir, fais le tour avec le téléphone — tous les équipements verts dans l'app, un test client par SSID, et une capture d'écran de l'état du site.

## 25. Wi-Fi de gestion et sécurité pendant le déploiement

Pendant l'onboarding par Wi-Fi, l'équipement diffuse un SSID de configuration. Règles :
- Ne laisse **jamais** un équipement en mode setup sur un site en production : termine l'onboarding ou éteins-le.
- Change les mots de passe par défaut **dès** la première configuration (section 102).
- Si tu pré-stages au bureau, isole les équipements de ton réseau de production (un petit switch ou un VLAN dédié) pour éviter qu'un DHCP sauvage ne perturbe ton LAN.

## 26. Gestion des firmwares via l'app : le minimum vital

- L'app propose les **mises à jour** des équipements du site (via le mécanisme HOUP côté switch, et l'équivalent côté AP/AR — **à vérifier sur la documentation officielle** pour le détail par modèle).
- Règle d'or : **ne mets jamais à jour le jour de la mise en service**. Stabilise d'abord, mets à jour ensuite, en heures creuses, site par site.
- Avant toute mise à jour : **sauvegarde la config** (section 89), préviens le client, et ne mets à jour qu'**un site pilote** d'abord en multi-sites.
- Note la version installée dans le dossier du site (ex. fictif : `AR180 – V100R001C00SPC100 – 2026-09-27`). Si ça casse quelque chose, tu sais d'où tu reviens.

## 27. L'app eKit et les notifications : les régler pour ne pas devenir fou

- Active les alertes **équipement hors ligne** et **nouvel équipement détecté** pour tes sites : ce sont les deux alertes qui valent de l'or en maintenance.
- Désactive (ou regroupe) les alertes informatives en masse si l'app les propose — un technicien qui reçoit 50 notifications par jour finit par toutes les ignorer, y compris les importantes.
- En multi-sites, vérifie que les alertes sont bien **rattachées au site** : « AP hors ligne » ne sert à rien si tu ne sais pas dans quelle boutique.

## 28. Partager un site avec le client (ou pas)

- Question à trancher **contractuellement** : le client a-t-il accès à l'app sur son site ? Avantages : il redémarre un AP tout seul à 22 h. Risques : il change le PSK, casse un VLAN, puis t'appelle en disant « ça ne marche plus depuis votre passage ».
- Pratique recommandée : donne au client un accès **lecture seule** s'il existe (**à vérifier sur la documentation officielle** — sinon, pas d'accès du tout et un numéro d'astreinte), et garde l'administration chez toi dans le cadre du contrat de maintenance.
- Si le client exige l'admin : fais-le par écrit, avec une clause « toute modification hors contrat = intervention facturée ».

## 29. iGuard et la détection de caméras espion : mode d'emploi honnête

- L'**AP673H (iGuard)** détecte les caméras espion (y compris non connectées au Wi-Fi) par analyse des ondes radio + IA, 24h/24 d'après Huawei.
- **Limite officielle** (fiche constructeur) : la fonction spycam ne marche qu'en **gestion locale** — pas de remontée cloud, pour protéger la confidentialité. Donc : tu installes l'AP673H, tu le gères en local pour cette fonction, et tu le supervises via l'app pour le reste.
- Le détecteur portable **DF10** + appli **iGuard** : pour des contrôles ponctuels (chambre d'hôtel avant l'arrivée d'un VIP, salle de conseil avant une réunion sensible). Autonomie annoncée ~1,5 h, taux de fausses alertes annoncé 5 % avec le modèle IA multidimensionnel.
- Ne promets jamais « zéro caméra garantie » : c'est un **outil de détection**, pas une certification. Positionne-le comme une couche de sécurité supplémentaire, avec une procédure (qui scanne, quand, que faire en cas d'alerte).

## 30. L'app eKit : limites à connaître avant de t'engager

- **Dépendance Internet** : sans Internet sur le site, pas de gestion cloud (le réseau local continue de tourner — section 38).
- **Dépendance au téléphone** : pas de vrai portail web complet d'administration d'après les infos disponibles (**à vérifier sur la documentation officielle** — le SNC apporte une console web, section 31).
- **Fonctions avancées limitées** : pas de scripting, pas d'automatisation type Ansible, pas d'API publique documentée à ma connaissance.
- **Verrouillage écosystème** : l'app ne gère que du Huawei eKit. Ton parc existant non-Huawei reste à gérer à part (interopérabilité : section 61).
- **Données hébergées chez Huawei** : à signaler au client dans les secteurs sensibles (données de gestion du réseau sur un cloud constructeur — clause contractuelle à prévoir).

---

## 31. Le cloud eKit : SME Network Center (SNC), c'est quoi ?

Le **SME Network Center (SNC)** est la plateforme cloud de gestion des réseaux eKit, complémentaire de l'app mobile : l'app, c'est le tournevis (déploiement et gestes quotidiens) ; le SNC, c'est l'atelier (vue d'ensemble, planification, maintenance multi-sites). D'après Huawei (MWC 2025) : le SNC couvre la **planification, le déploiement, l'inspection et l'O&M en ligne** du réseau, avec une fonction de **dépannage simplifié 2.0** et d'**expansion simplifiée** du réseau.
Concrètement, attends-toi à une console qui montre : l'état du réseau, l'état des équipements et l'état des connexions des clients (STA) **pour tous les sites d'un tenant**, de façon centralisée (c'est ce que décrit la fiche AP361 pour la plateforme cloud).

## 32. Comptes et tenants : comment c'est organisé

- Un **tenant** (locataire cloud) = généralement ton entreprise (prestataire) ou le client final — à trancher selon le contrat (section 28).
- Le tenant contient les **sites**, chaque site contient les **équipements**.
- Un équipement n'appartient qu'à **un seul site** à la fois. Pour le déplacer : le retirer du site A puis l'onboarder sur le site B (ne le fais jamais « à chaud » sans prévenir : le déplacement réinitialise la configuration réseau de l'équipement — **à vérifier sur la documentation officielle** pour le comportement exact).
- **Règle d'or multi-clients :** un tenant par client final, ou un tenant prestataire avec des sites bien séparés ? La pratique la plus sûre : **un tenant par client** quand le client est autonome, **un tenant prestataire multi-sites** quand tu assures l'infogérance. Ne mélange jamais deux clients dans les mêmes vues sans cloisonnement.

## 33. Le modèle économique : « gratuit », vraiment ?

Huawei et ses distributeurs annoncent une gestion **app + cloud gratuite, sans frais de licence**. Ce que ça veut dire en pratique, d'après les sources publiques :
- pas de licence annuelle par AP comme chez certains concurrents ;
- l'app et le SNC sont inclus dans l'achat du matériel.
**Nuances honnêtes :**
- « Gratuit » ne veut pas dire « sans coût » : il te faut de la connectivité Internet sur chaque site, un smartphone, et ton temps.
- Les fonctions avancées (pare-feu USG : IPS/AV/SSL VPN au-delà des seuils par défaut, certaines fonctions IA) peuvent relever de **licences ou d'abonnements de signatures** — **à vérifier sur la documentation officielle** et auprès de ton distributeur avant de chiffrer.
- Vérifie aussi la **durée d'engagement** : le gratuit d'aujourd'hui peut devenir payant demain. Ne construis pas ton business model sur une promesse marketing sans filet contractuel.

## 34. Rôles et permissions : qui peut faire quoi

Principe à appliquer même si le détail des rôles varie selon la version (**à vérifier sur la documentation officielle**) :
| Rôle (logique) | Peut voir | Peut modifier | À donner à |
|---|---|---|---|
| Propriétaire / Admin | Tout, tous les sites | Tout | Toi (compte générique du service) |
| Technicien | Sites assignés | Configurer, redémarrer, mettre à jour | Ton équipe terrain |
| Lecture seule | Sites assignés | Rien (diagnostic uniquement) | Client final, stagiaire |
| Installateur temporaire | 1 site, durée limitée | Onboarding uniquement | Sous-traitant ponctuel |

- **Revue trimestrielle des accès** : qui a encore accès à quoi ? Un ex-technicien avec l'admin sur 15 sites clients, c'est une bombe à retardement.
- Journalise les changements importants (qui a modifié quel SSID, quand) — si la plateforme le propose (**à vérifier**), active-le.

## 35. Multi-sites : le vrai terrain de jeu du cloud eKit

C'est là que le cloud paie : une enseigne avec 12 boutiques, un groupe avec 5 agences.
- **Tableau de bord global** : tous les sites, avec un code couleur (vert/orange/rouge) par site. Le matin, un coup d'œil suffit pour savoir où intervenir.
- **Modèles de configuration** : définis une fois la configuration type « boutique standard » (VLAN, SSID, règles), applique-la aux nouveaux sites. C'est le « déploiement simplifié » mis en avant par Huawei.
- **Mises à jour par vagues** : site pilote → 3 sites → tous les sites. Jamais tout d'un coup un vendredi soir.
- **Alertes centralisées** avec le nom du site dans chaque alerte (section 27).
- Limite honnête : le cloud ne remplace pas une **visite sur site** quand le problème est physique (câble arraché, AP décroché, coupure électrique). La supervision dit *où* ça fait mal, pas *pourquoi* au niveau physique.

## 36. Ce qu'on peut faire à distance (et qui marche bien)

- Voir l'état de chaque équipement (en ligne/hors ligne, charge CPU/mémoire — niveau de détail **à vérifier**) ;
- voir les **clients connectés** par AP et par SSID (utile : « le Wi-Fi est lent » → 45 clients sur un seul AP361 = problème de densité, pas de panne) ;
- redémarrer un équipement à distance ;
- modifier un SSID / un PSK (ex. : changer le mot de passe invités d'un hôtel chaque semaine) ;
- pousser une mise à jour firmware ;
- activer/désactiver le PoE d'un port (caméra qui ne répond plus → cycle PoE à distance avant de se déplacer) ;
- diagnostiquer un lien (inspection en ligne, tests de connectivité — « troubleshooting 2.0 »).

## 37. Ce qu'on NE peut PAS faire à distance (limites honnêtes)

- **Remplacer un câble défectueux** ou rebrancher un AP : le cloud ne fait pas de magie physique.
- **Configurer finement** ce que l'interface cloud n'expose pas (ACL avancées, QoS fine, routage dynamique) — bascule en local (CLI/web) si le modèle le permet.
- **Récupérer un équipement totalement planté** (boot en échec) : il faut le reset physique sur site.
- **Contourner une coupure Internet du site** : sans uplink, le cloud ne voit plus rien (le réseau local continue — section 38).
- **Gérer du non-Huawei** : ton vieux switch manageable d'une autre marque reste hors du cloud eKit.
- **Faire de l'analyse forensique poussée** : pas de capture de paquets distante ni d'export NetFlow à ma connaissance (**à vérifier sur la documentation officielle**).

## 38. Panne Internet sur site : que devient le réseau eKit ?

Question que tout client pose : « si Internet tombe, mon réseau local tombe ? » Réponse à donner, honnête :
- **Le data plane est local** : le switch continue de commuter, les AP continuent de servir les SSID, le DHCP local (sur l'AR) continue de distribuer des adresses. Le bureau continue de travailler en local (fichiers, imprimantes).
- **Ce qui tombe** : l'accès Internet (évidemment), la **gestion cloud** (tu ne vois plus le site), les remontées d'alertes, et les fonctions qui dépendent du cloud (portail captif hébergé cloud s'il y en a un — **à vérifier**).
- **Au retour d'Internet** : les équipements se reconnectent seuls au cloud (prévois un délai de quelques minutes, ne redémarre pas tout en panique).
- Conséquence pratique : pour un site isolé (entrepôt au fin fond d'une zone), prévois un **accès de secours** : 4G sur la passerelle si le modèle le permet, ou au minimum un accès local documenté (IP de gestion, identifiants dans le coffre — section 102).

## 39. Planification radio avec le SNC : ce qu'il faut en attendre (ou pas)

Le SNC aide à la **planification et au déploiement** d'après Huawei, mais ne t'attends pas à un outil de survey prédictif complet :
- Utilise le SNC pour **placer tes AP sur plan** et suivre le déploiement (quoi, où, quel état).
- Le **vrai dimensionnement radio** reste manuel : 1 AP pour ~100-150 m² en bureaux cloisonnés placo, 1 AP par 2-3 chambres d'hôtel en cloison légère, 1 AP pour 20-30 utilisateurs actifs en usage bureautique. Ce sont des ordres de grandeur terrain — valide par un **survey post-installation** (tour avec un smartphone et une app de mesure Wi-Fi).
- Laisse l'**allocation automatique des canaux** active au début (les AP eKit gèrent le RRM de base), puis fige les canaux si tu constates des instabilités (**à vérifier** : niveau de contrôle RRM exposé dans l'app/SNC selon version).

## 40. Le portail captif invités : la fonction qui fait vendre

Pour hôtels, restaurants, boutiques : le Wi-Fi invités avec portail captif (page d'accueil avec CGU / code d'accès).
- Les AP eKit supportent l'**authentification PSK et portail** sans WAC ni serveur externe (fiche AP266 : fonctions d'authentification intégrées au cloud).
- L'USG6000F-S propose aussi un **portail local intégré** avec base locale ou RADIUS/AD/LDAP.
- Bonnes pratiques : SSID invités **isolé** (pas d'accès au LAN), débit **limité** par client (ex. fictif : 10 Mbit/s), durée de session limitée, page aux couleurs du client, mentions légales.
- **Piège classique** : le portail captif qui ne s'affiche pas sur certains smartphones → prévois toujours un **PSK invités de secours** communiqué à l'accueil.

## 41. Sauvegarde de la configuration via le cloud

- Le cloud conserve la configuration des sites (c'est le principe du pilotage centralisé). Mais **ne compte pas uniquement dessus** : exporte régulièrement une copie locale (section 89).
- Avant chaque changement majeur : capture d'écran ou export de la config actuelle + note datée (« avant migration VLAN invités »).
- En cas de remplacement d'un équipement défectueux (RMA) : le nouvel équipement onboardé sur le même site **récupère la configuration du site** via le cloud — c'est l'un des gros avantages opérationnels. Vérifie quand même après remplacement (SSID, VLAN, PoE).

## 42. Mises à jour via le cloud : méthode sans stress

1. **Lis la note de version** (release notes) avant tout : quoi de neuf, quoi de corrigé, incompatibilités connues.
2. **Sauvegarde** (section 89).
3. **Site pilote** : 1 site non critique, en heures creuses, avec quelqu'un sur place ou joignable.
4. **48 h d'observation** sur le pilote avant de généraliser.
5. **Par vagues** : jamais plus d'un tiers du parc en une fois.
6. **Fenêtre de maintenance annoncée** au client (même si « ça prend 5 minutes » — quand ça plante, ça prend 2 heures).
7. Note les versions dans le dossier de chaque site.

## 43. Inspection en ligne et « troubleshooting 2.0 »

La fonction d'**inspection en ligne** du SNC (mise en avant par Huawei) sert à :
- vérifier la santé d'un site avant une visite (tu arrives avec un diagnostic, pas avec des questions) ;
- détecter les anomalies récurrentes (AP qui décroche toutes les nuits à 2 h → suspecte le PoE ou l'alimentation, pas le Wi-Fi) ;
- valider après intervention (« tout est vert, je peux partir »).
Construis-toi une **routine d'inspection hebdomadaire** (15 min) : dashboard global → sites orange/rouge → détail → action ou ticket. C'est cette routine qui transforme le cloud en vraie valeur de maintenance.

## 44. Expansion simplifiée : ajouter un site ou étendre un site

- **Nouveau site** : crée le site dans le cloud, applique le modèle de configuration type (section 35), pré-stage les équipements au bureau (section 19), expédie ou va brancher. Un technicien junior peut faire le branchement si tout est pré-configuré.
- **Extension d'un site** : scan du SN du nouvel AP → branchage sur un port PoE → l'AP récupère la config du site automatiquement. C'est le cas d'usage « barcode scanning » de la fiche technique.
- **Règle** : toute extension = mise à jour du **plan** (adressage, ports utilisés, position des AP) le jour même. Le plan obsolète est la première cause de galère 6 mois plus tard.

## 45. Cloud eKit : tableau récapitulatif honnête

| Fonction | Via app/SNC | Remarques |
|---|---|---|
| Onboarding (Wi-Fi / scan SN) | Oui | Le cœur du système |
| Config SSID, VLAN de base, PoE | Oui | Détail selon version — à vérifier |
| Supervision multi-sites, alertes | Oui | Le gros point fort |
| Redémarrage / cycle PoE à distance | Oui | Évite 50 % des déplacements |
| Mises à jour firmware | Oui (HOUP côté switch) | Par vagues, jamais en prod directe |
| Inspection / dépannage simplifié | Oui (« 2.0 ») | Aide au diagnostic, pas de magie |
| Portail captif | Oui (AP ou USG) | Prévoir un PSK de secours |
| Config avancée (ACL fines, QoS, routage dyn.) | Non / limité | Gestion locale (CLI/web) si supportée |
| Équipements non-Huawei | Non | Outil séparé |
| Fonctionnement sans Internet | Gestion : non ; réseau local : oui | À expliquer au client |
| API publique / automatisation | À vérifier sur la documentation officielle | Ne pas promettre |
| Coût de la gestion cloud | Annoncé gratuit / sans licence | Vérifier licences USG avancées auprès du distributeur |

---

## 46. Scénario A — Bureau 20 postes : cadrage

**Besoin type :** PME de 20 personnes, open space + 2 bureaux fermés + 1 salle de réunion, 1 seul étage (~300 m²), fibre opérateur 1 Gbit/s, 25 PC filaires, 40 terminaux Wi-Fi (PC portables, smartphones), 1 imprimante réseau, 2 caméras IP (option), pas de téléphonie IP (ou 5 postes IP).
**Choix d'architecture :** 1 passerelle AR + 1 switch PoE + 3 AP. Simple, évolutif, tout supervisé par l'app.

## 47. Scénario A — Topologie ASCII

```
                    [Internet Fibre Operateur]
                               |
                          (ONT/Box operateur en mode bridge)
                               |  (WAN - PPPoE ou DHCP)
                    +---------------------+
                    |  AR180 Pro (passerelle)|
                    |  192.168.10.1 (LAN)   |
                    |  DHCP + DNS relay     |
                    +----------+----------+
                               | (LAN trunk vers switch)
                    +----------+----------+
                    | S310-24 ports PoE+   |
                    | (ex: S310-24P4S)     |
                    +--+---+---+---+--+---+
                       |   |   |   |  |
        +--------------+   |   |  |  +--------------+
        |                  |   |  |                 |
   [AP361 #1]         [AP361 #2] |           [AP361 #3]
   Open space         Salle reunion|           Bureaux fermes
   (plafond)          (plafond)    |
                              [PC filaires: ports 5-20, VLAN 10]
                              [Imprimante: port 21, VLAN 10]
                              [Cameras: ports 22-23, VLAN 40, PoE]

VLAN:
  VLAN 1   (defaut, eviter pour l'usage) 
  VLAN 10  STAFF      192.168.10.0/24  (PC + Wi-Fi staff)
  VLAN 20  GUESTS     192.168.20.0/24  (Wi-Fi invites, isole, portail captif)
  VLAN 40  CAMERAS    192.168.40.0/24  (si cameras)
```

## 48. Scénario A — Liste matériel

| Qté | Équipement | Rôle | Notes |
|---|---|---|---|
| 1 | AR180 Pro | Passerelle (routage, DHCP, Wi-Fi de secours) | Gère jusqu'à 8 AP / 32 équipements : de la marge |
| 1 | S310-24P4S (ou 24 ports PoE+ équivalent eKit) | Switch d'accès PoE | Vérifier la référence exacte dispo chez ton distributeur |
| 3 | AP361 | Wi-Fi intérieur | 1 / ~100-150 m² en open space placo |
| 1 | Onduleur 1000-1500 VA | Protection électrique | AR + switch + ONT dessus (cf. ton guide onduleurs) |
| — | Câblage Cat6, goulottes, baie 6U | Infra passive | 25 prises RJ45 murales |
| 2 | Caméras IP (option) | Vidéosurveillance | VLAN 40 dédié |

**Budget PoE (vérification) :** 3× AP361 (~9 W) = 27 W + 2 caméras (~10 W) = ~47 W. Un switch 24 ports PoE+ (~190-370 W selon modèle) est large. **Toujours** calculer : ne suppose jamais.

## 49. Scénario A — Configs clés (via app eKit / cloud)

1. **Accès Internet AR** : PPPoE (identifiants opérateur — fictifs ici : `client@fai.xx` / mot de passe à saisir, jamais noté en clair dans ce guide) ou DHCP selon l'opérateur. DNS : ceux de l'opérateur ou publics.
2. **VLAN** : créer 10 (staff), 20 (invités), 40 (caméras). Le lien AR↔switch en **trunk** (tous les VLAN taggés).
3. **Ports switch** : ports 1-3 (AP) en trunk avec VLAN natif 10 ; ports 5-20 en access VLAN 10 ; port 21 (imprimante) access VLAN 10 ; ports 22-23 access VLAN 40 + PoE activé.
4. **SSID** : `BUREAU-Staff` (WPA2/WPA3, VLAN 10, SSID non diffusé en option) ; `BUREAU-Invites` (portail captif, VLAN 20, isolation client, limite 10 Mbit/s/client — valeurs fictives d'exemple).
5. **DHCP** : un scope par VLAN sur l'AR (10 : 192.168.10.50-200 ; 20 : 192.168.20.50-200, bail court 4 h ; 40 : 192.168.40.10-50).
6. **Tests** : client filaire (ping passerelle + Internet), client Wi-Fi staff, client invité (portail OK ? isolation OK ? pas d'accès au VLAN 10 ?), débit speedtest à 3 endroits.

## 50. Scénario B — Hôtel 40 chambres : cadrage

**Besoin type :** hôtel 3 étoiles, 4 étages × 10 chambres + réception + restaurant + piscine extérieure. 60-80 clients simultanés le soir, portail captif avec code chambre, 8 caméras, 5 téléphones IP (réception), TV IP (option).
**Deux architectures possibles :**
- **Classique cuivre** : 1 AR280 (ou USG6000F-S125 si besoin pare-feu) + 2× S310-48P4S + ~20 AP361 (1 pour 2 chambres) + 2 AP761 (piscine/extérieur).
- **MiniFTTO** : OLT + 1 F700D par chambre (fibre). Plus cher à l'achat, imbattable en esthétique et en évolutivité — à proposer en neuf/rénovation lourde uniquement.

## 51. Scénario B — Topologie ASCII (version cuivre)

```
                    [Internet Fibre 1-2 Gbit/s]
                               |
                    +---------------------+
                    | AR280 ou USG6000F-S |
                    | (pare-feu + routage) |
                    +----------+----------+
                               | (trunk 10G si possible)
              +----------------+----------------+
              |                                 |
   [S310-48P4S #1 - Etages 1-2]      [S310-48P4S #2 - Etages 3-4 + RDC]
      |      |      |      |              |        |         |
   [AP361] x10 (1 pour 2 chambres,   [AP361] x10   [AP761] x2
    couloir, plafond)                 + RDC:      (piscine, terrasse,
    VLAN 30 (chambres)                 reception    exterieur IPxx)
                                       restaurant
                                       VLAN 30/50
   [Cameras: VLAN 40]                [Telephones IP: VLAN 60]
   [TV IP: VLAN 70 si present]

VLAN:
  VLAN 10  MGMT/STAFF    10.10.10.0/24   (personnel, recep.)
  VLAN 30  CHAMBRES      10.10.30.0/24   (clients, portail captif, isole)
  VLAN 40  CAMERAS       10.10.40.0/24   (VMS/NVR)
  VLAN 50  RESTAURANT    10.10.50.0/24   (caisse, Wi-Fi clients resto)
  VLAN 60  VOIX          10.10.60.0/24   (telephones IP, QoS prioritaire)
  VLAN 70  TV-IP         10.10.70.0/24   (si TV sur IP, multicast)
```

**Règles radio hôtel :** puissance d'émission **modérée** (mieux vaut plus d'AP à faible puissance que peu d'AP à fond — sinon les clients s'accrochent à l'AP du couloir d'en face), canaux 1/6/11 en 2,4 GHz répartis par étage, 5 GHz en priorité (band steering si dispo).

## 52. Scénario B — Variante MiniFTTO (eKitOptix)

```
   [Internet] -> [OLT eKitOptix] -> [Splitter optique 1:32]
                                              |
                    +------------+------------+------------+
                    |            |                         |
              [F700D Ch.101] [F700D Ch.102] ... [F700D Ch.140]
              (1 fibre / chambre : Wi-Fi 7 + 2-4 ports GE + TV/tel.)

Avantages : 1 seul cable (fibre) par chambre, aucun switch d'etage,
            Wi-Fi 7 homogene, esthetique (boitier mural discret).
Contraintes : OLT + splitters + soudures fibre (competence fibre requise),
              cout initial superieur, ecosysteme ferme.
```

**Quand proposer quoi :** rénovation légère / existant cuivre → version cuivre (AP361). Construction neuve ou rénovation lourde avec faux plafonds ouverts → chiffrer les deux et laisser le client choisir avec un comparatif honnête (CAPEX vs OPEX vs esthétique).

## 53. Scénario B — Liste matériel (version cuivre)

| Qté | Équipement | Rôle |
|---|---|---|
| 1 | AR280 ou USG6000F-S125 | Tête de réseau (si pare-feu exigé par le client → USG) |
| 2 | S310-48P4S (380 W PoE+) | Distribution étages |
| 20 | AP361 | Chambres (1/2 chambres) + couloirs |
| 2 | AP761 | Extérieur (piscine, terrasse) — prévoir parafoudre et terre |
| 8 | Caméras IP | VLAN 40, vers NVR |
| 5 | Téléphones IP | VLAN 60 |
| 1 | Onduleur 2000-3000 VA | Tête + switchs (autonomie 15-30 min mini) |
| — | Câblage Cat6, 60+ prises | Chambres + paliers |

**Budget PoE :** 20× 9 W = 180 W + 2× 17,7 W = 35,4 W + 8× 10 W = 80 W + 5× 5 W = 25 W → **~320 W**. Deux S310-48P4S (380 W chacun) : OK avec marge si bien réparti (ne mets pas tout sur le même switch).

## 54. Scénario B — Configs clés

1. **Portail captif** sur VLAN 30 : code chambre ou e-mail + CGU, session 24 h, débit 15 Mbit/s/client (exemple fictif), isolation inter-clients.
2. **QoS voix** : VLAN 60 prioritaire (les téléphones de la réception ne doivent jamais hacher).
3. **Isolement** : VLAN 30 et 50 sans accès inter-VLAN (un client ne voit ni les autres clients ni le LAN staff).
4. **Plages DHCP** : /24 par VLAN avec baux adaptés (chambres : 12 h ; restaurant : 4 h).
5. **Nommage AP** : `HOTEL-Etage1-AP01` … — quand l'alerte « AP hors ligne » arrive, tu sais où envoyer le technicien sans réfléchir.
6. **Tests** : chambre la plus éloignée (débit), piscine (AP761), bascule (débranche un switch → vérifie que l'autre moitié tient), portail captif sur iPhone + Android (les deux comportements diffèrent).

## 55. Scénario C — Commerce / boutique : cadrage

**Besoin type :** boutique 150 m² + réserve, 8 employés, 2 caisses (TPE), Wi-Fi clients gratuit, 4 caméras, 1 enseigne avec 3 boutiques (multi-sites).
**Architecture :** par boutique : 1 AR180 (suffit : ~100 terminaux) + 1 S220-8P4S (8 ports PoE+ 125 W : 2 AP + 4 caméras + marge) + 2 AP361 (surface de vente + réserve). Le tout supervisé en **multi-sites** dans le cloud.

## 56. Scénario C — Topologie ASCII (par boutique)

```
   [Internet Box operateur]
              |
   [AR180] (192.168.1.1, DHCP, Wi-Fi integre en secours)
              |
   [S220-8P4S] (125 W PoE+)
     |    |    |    |
  [AP361] [AP361] [Cameras x4] [Caisses x2 (VLAN 10)]
  Surface  Reserve  (VLAN 40)   TPE (VLAN 10, prioritaire)
  (VLAN 10/20)

VLAN:
  VLAN 10  CAISSES+STAFF  192.168.1.0/24
  VLAN 20  CLIENTS         192.168.2.0/24 (portail captif, isole, 5 Mbit/s)
  VLAN 40  CAMERAS         192.168.4.0/24

Cloud : 1 tenant prestataire, 3 sites (Boutique Centre, Boutique Nord, Boutique Sud),
        modele de config "boutique standard" applique aux 3.
```

**Spécificités commerce :**
- Les **TPE (terminaux de paiement)** : jamais sur le Wi-Fi invités, VLAN dédié ou a minima VLAN staff, et surtout **ne jamais couper leur connectivité** pendant une mise à jour en journée. Fenêtre de maintenance : avant ouverture.
- **Portail captif** = outil marketing (récupération d'e-mails opt-in, page promo). Cadre-le juridiquement (mentions, consentement).
- Multi-sites : le changement du PSK invités se fait **une fois pour les 3 boutiques** depuis le cloud.

## 57. Scénario C — Liste matériel et configs clés

| Qté / boutique | Équipement |
|---|---|
| 1 | AR180 |
| 1 | S220-8P4S (125 W PoE+) |
| 2 | AP361 |
| 4 | Caméras IP |
| 1 | Onduleur 1000 VA |

**Budget PoE :** 2× 9 + 4× 10 = 58 W < 125 W : OK.
**Configs clés :** modèle « boutique standard » dans le cloud (VLAN, SSID `BOUTIQUE-Clients` + `BOUTIQUE-Staff`, portail captif identique, plages DHCP identiques — attention : **mêmes plages IP sur des sites différents, c'est OK** car chaque site est un LAN indépendant ; ça simplifie même la maintenance). Alertes « équipement hors ligne » par boutique avec le nom du responsable local.

## 58. Scénario D — Petit entrepôt : cadrage

**Besoin type :** entrepôt 800 m², 10 employés, terminaux mobiles (douchettes Wi-Fi), 6 caméras, portail d'accès, bureau mezzanine (5 PC), chariots élévateurs (pas de Wi-Fi temps réel critique, mais connectivité stable).
**Défis spécifiques :** hauteur sous plafond (6-8 m), rayonnages métalliques (réflexions, zones d'ombre), poussière, température.
**Architecture :** 1 AR280 + 1 S310-24P4S + 4-6 AP (AP361 en intérieur, ou AP761 si zones semi-ouvertes / quais). AP fixés **sous plafond mais pas trop haut** : en entrepôt, on descend les AP à 4-5 m sur des mats ou chemins de câbles plutôt que de les coller à 8 m (à 8 m, le signal s'étale et les douchettes en bas captent mal).

## 59. Scénario D — Topologie ASCII

```
   [Internet Fibre]
          |
   [AR280] (bureau mezzanine, baie)
          |
   [S310-24P4S] (baie mezzanine, 380 W PoE+)
     |        |         |          |
  [AP x2]  [AP x2]   [Cameras]  [PC bureau]
  Allee A  Allee B   (VLAN 40)  (VLAN 10)
  (mats 4-5 m, VLAN 10/20)

VLAN:
  VLAN 10  BUREAU+DOUCHETTES  192.168.10.0/24 (les douchettes = critiques, SSID dedie)
  VLAN 20  INVITES            192.168.20.0/24 (transporteurs au bureau, isole)
  VLAN 40  CAMERAS            192.168.40.0/24

Regle radio entrepot : 1 AP / ~400-500 m² en rayonnages hauts (ordre de grandeur),
canaux 5 GHz uniquement pour les douchettes si elles le supportent (moins d'interferences),
puissance moderee, survey OBLIGATOIRE avec un chariot (test en mouvement).
```

## 60. Scénario D — Liste matériel et points de vigilance

| Qté | Équipement |
|---|---|
| 1 | AR280 |
| 1 | S310-24P4S |
| 4-6 | AP361 (intérieur) — valider le nombre par survey |
| 6 | Caméras IP (quais + allées) |
| 1 | Onduleur 1500-2000 VA |

**Vigilances terrain :**
- **Douchettes** : SSID dédié, 5 GHz, pas de portail captif (une douchette ne sait pas cliquer sur « J'accepte »), PSK robuste, roaming testé **en marchant** avec le chariot.
- **Poussière** : baie fermée avec filtre, AP IP adaptés si zone très poussiéreuse (sinon AP361 standard + nettoyage annuel).
- **Câbles** : chemin de câbles métallique, pas de câble volant au-dessus des allées (chariots + câbles = catastrophe).
- **Coupure électrique** : l'entrepôt a souvent des coupures — onduleur dimensionné (cf. guide onduleurs) + redémarrage automatique des équipements (ils reviennent seuls, vérifier quand même via le cloud).

---

## 61. Interopérabilité : eKit avec du non-eKit, est-ce que ça marche ?

Oui, **aux standards ouverts**, non aux protocoles propriétaires. Tableau de vérité :

| Fonction | eKit ↔ non-Huawei | Détails |
|---|---|---|
| Commutation L2 / VLAN 802.1Q | Oui | Un trunk 802.1Q entre un S310 et un switch d'une autre marque fonctionne (tagging standard). Vérifie le VLAN natif des deux côtés — cause n°1 d'incompatibilité. |
| Routage statique / DHCP / DNS | Oui | Standards IP universels. |
| Wi-Fi (clients) | Oui | 802.11ax/be : n'importe quel client Wi-Fi 6/7 se connecte. |
| Roaming entre AP eKit et AP tiers | **Non / dégradé** | Le roaming rapide (802.11r/k/v) inter-constructeurs est aléatoire. En pratique : les clients se reconnectent, avec une micro-coupure. Ne mélange pas les marques sur une même zone de couverture. |
| Gestion unifiée (app eKit) | **Non** | L'app ne voit que du eKit. Le reste = son outil d'origine. |
| PoE 802.3af/at | Oui | Standard : un AP eKit sur un switch PoE tiers fonctionne, et inversement. |
| Agrégation de liens (LACP) | Oui si 802.3ad des deux côtés | À vérifier par modèle côté eKit (documentation officielle). |
| Stacking | **Non** | Le stacking est propriétaire : on ne stacke pas un S310 avec un switch d'une autre marque. |

**Règle d'or :** homogène par **fonction** (tous les AP d'un site = même gamme ; tous les switchs d'accès = même gamme), hétérogène autorisé aux **frontières** (un vieux switch en cascade derrière un S310, ça passe).

## 62. Migration depuis un parc existant : la méthode en 5 phases

**Phase 1 — Inventaire (1/2 journée) :** liste chaque équipement existant (marque, modèle, âge, rôle, PoE ou non), le plan d'adressage actuel, les SSID et PSK en service, les points de douleur (« le Wi-Fi coupe dans la salle de réunion »).
**Phase 2 — Coexistence (jour J) :** installe le cœur eKit (AR + switch) **en parallèle** de l'existant. Nouvelle plage IP eKit, SSID eKit en plus des anciens (ex. fictif : `BUREAU-NEW` à côté de `BUREAU`).
**Phase 3 — Bascule pilote :** migre 2-3 utilisateurs volontaires + 1 imprimante. 48 h d'observation.
**Phase 4 — Bascule générale :** en heures creuses, bascule les clients (change le PSK de l'ancien SSID pour forcer la migration, ou éteins les vieux AP un par un). Garde l'ancien matériel sous la main 1 semaine.
**Phase 5 — Décommissionnement :** une fois le client satisfait (2 semaines), retire l'ancien matériel, mets à jour le plan, archive l'ancien adressage.
**Jamais** de big-bang un vendredi à 17 h. La migration en douceur, c'est 90 % de la réussite.

## 63. Réutiliser l'existant : quoi garder, quoi jeter

- **Câblage Cat5e/Cat6 en bon état** : à garder (teste-le au testeur, surtout les prises qui « marchaient avant »).
- **Vieux switch 100 Mbit/s** : à jeter (goulot d'étranglement garanti avec du Wi-Fi 6).
- **Vieux switch Gbit non-PoE en bon état** : réutilisable en cascade pour des PC filaires non critiques (pas pour alimenter des AP).
- **Vieux AP Wi-Fi 4/5** : à éteindre une fois les eKit en place (un vieux AP qui diffuse encore crée des interférences et attire les clients lents qui plombent la cellule radio).
- **Onduleur existant** : à garder si les batteries ont moins de 3 ans et la puissance suffit (sinon : remplacement — cf. guide onduleurs).

## 64. Adressage IP : le plan type PME eKit

Utilise du **privé RFC 1918** avec une logique par site. Modèle recommandé (exemple fictif, à adapter) :

| Site | Réseau de base | VLAN staff | VLAN invités | VLAN caméras | VLAN voix |
|---|---|---|---|---|---|
| Site 1 (siège) | 192.168.10.0/23 | .10.0/24 (VLAN 10) | .11.0/24 (VLAN 20) | .12.0/24 (VLAN 40) | .13.0/24 (VLAN 60) |
| Site 2 (boutique) | 192.168.20.0/24 | .20.0/24 (VLAN 10) | .21.0/24 (VLAN 20) | — | — |

**Règles :**
- Le **3e octet = le site**, le VLAN affine. En dépannage à 2 h du matin, tu sais où tu es rien qu'en voyant l'IP.
- **Équipements d'infrastructure en IP fixes basses** : passerelle en .1, switchs en .2, .3…, AP en .10-.49 (ou DHCP avec réservation — les deux se défendent ; le fixe documenté est plus robuste si le DHCP tombe).
- **Jamais** de 192.168.1.0/24 ou 192.168.0.0/24 sur un site pro : ce sont les plages par défaut des box grand public → conflits garantis dès qu'on branche une box ou un VPN.
- Documente tout dans une **fiche site** (section 66).

## 65. Fiche site type (à remplir pour chaque déploiement)

```
FICHE SITE - [Nom client - Ville - Batiment]          Date : __________
Contact client : __________ Tel : __________
Compte eKit / tenant : __________ Role : __________
Acces Internet : operateur __________, type __________, debit __________
Passerelle : modele __________, SN __________, IP LAN __________, FW __________
Switchs : modele __________, SN __________, IP __________, FW __________
AP : modele __________, SN __________, emplacement __________, IP __________
Adressage : (tableau section 64)
VLAN : (tableau)
SSID : nom __________, PSK (dans le coffre, pas ici), VLAN __________
Onduleur : modele __________, puissance __________, autonomie estimee __________
Plan de cablage : [photo/schema joint]
Remarques : __________________________________________
```

## 66. VLAN : le découpage standard PME

| VLAN ID | Nom | Usage | Inter-VLAN |
|---|---|---|---|
| 1 | Défaut | À éviter pour le trafic (risque de fuite) | — |
| 10 | STAFF | Utilisateurs, PC, imprimantes | Vers Internet : oui ; vers autres VLAN : selon besoin |
| 20 | GUESTS | Invités / clients | Internet uniquement, **isolation totale** du LAN |
| 30 | CHAMBRES (hôtel) | Clients hôtel | Comme GUESTS |
| 40 | CAMERAS | Vidéosurveillance | Vers NVR uniquement, pas d'Internet (sauf accès distant via VPN) |
| 50 | CAISSES/TPV | Paiement, caisses | Internet + serveur de caisse uniquement |
| 60 | VOIX | Téléphonie IP | Prioritaire (QoS), vers IPBX |
| 70 | TV-IP | Télévision sur IP | Multicast local |
| 99 | MGMT | Gestion des équipements | Accès restreint aux admins |

**Erreurs à éviter :** mettre les caméras sur le VLAN staff « parce que c'est plus simple » (une caméra compromise = porte d'entrée sur le LAN) ; oublier le VLAN natif cohérent sur les trunks ; utiliser le VLAN 1 pour de la prod.

## 67. SSID : la doctrine (moins c'est mieux)

- **3 SSID maximum** par site : staff, invités, + 1 technique (douchettes, IoT) si indispensable. Chaque SSID supplémentaire = overhead radio (beacons) qui dégrade tout le monde.
- **Nommage** : explicite, sans caractères spéciaux ni espaces si possible (ex. fictifs : `HOTELPALM-Staff`, `HOTELPALM-Invites`). Évite les noms génériques (`WiFi`, `Linksys`) qui créent des confusions.
- **Bandes** : diffuse en 2,4 + 5 GHz par défaut ; si les clients sont récents, envisage un SSID **5 GHz uniquement** pour le staff (moins d'interférences) — à tester.
- **Sécurité** : WPA2-AES minimum, **WPA3** si tous les clients le supportent (les AP eKit récents le supportent d'après les fiches — **à vérifier** par modèle). PSK long (20+ caractères) généré, pas un mot du dictionnaire.
- **SSID invités** : portail captif ou PSK simple tourné régulièrement, **isolation client** activée, débit limité.
- **Ne diffuse pas le SSID staff** si tu veux réduire le bruit (sécurité par l'obscurité = faible, mais ça évite les tentatives des voisins). Le SSID invités, lui, doit être visible.

## 68. PoE : dimensionner sans se tromper (la méthode)

**Étape 1 — Lister les consommateurs :**

| Équipement | Standard PoE | Conso typique (terrain) |
|---|---|---|
| AP361 | 802.3af | ~9 W |
| AP761 | 802.3at (recommandé) | ~17,7 W max |
| AP Wi-Fi 7 (AP572/673) | 802.3at/bt selon modèle | 20-30 W (à vérifier par fiche) |
| Caméra IP fixe | 802.3af | 5-10 W |
| Caméra PTZ / IR puissant | 802.3at | 20-30 W |
| Téléphone IP | 802.3af | 3-7 W |

**Étape 2 — Additionner et comparer au budget du switch** (ex. : S310-48P4S = 380 W ; S220-8P4S = 125 W).
**Étape 3 — Marge de 20-30 %** : un budget PoE utilisé à 100 % ne laisse aucune place à l'ajout d'une caméra dans 6 mois.
**Étape 4 — Vérifier le budget PAR PORT** : 30 W max en 802.3at. Une caméra PTZ à 25 W sur un port af (15,4 W) = caméra qui redémarre en boucle.
**Étape 5 — Câble** : PoE = courant dans le câble → **Cat5e minimum, Cat6 recommandé**, 90 m max par brin, pas de « rallonge bricolée ».

## 69. PoE : les fonctions qui sauvent des déplacements

D'après les fiches (S220 : Fast PoE, Perpetual PoE) :
- **Perpetual PoE** : le switch continue d'alimenter les ports PoE pendant son redémarrage (mise à jour sans couper les caméras/AP). Vérifie que c'est activé.
- **Fast PoE** : réalimentation rapide des ports après une coupure électrique (les AP reviennent vite).
- **Cycle PoE à distance** : via l'app/cloud, coupe/rallume le PoE d'un port pour redémarrer un AP ou une caméra plantée — **le geste n°1 du dépannage à distance** (section 36).
- **Planification PoE** (si supportée — à vérifier) : couper le PoE des AP la nuit dans un bureau vide = économies d'énergie.

## 70. Ordre de mise en service : la séquence qui marche à tous les coups

```
1. INFRA PASSIVE : baie, câblage, prises, terre, étiquetage. (Tester chaque brin.)
2. ÉLECTRIQUE : onduleur en place, prises ondulées identifiées.
3. TÊTE : modem/ONT opérateur -> AR (WAN). Valider INTERNET sur l'AR d'abord.
4. DISTRIBUTION : switch(s) derrière l'AR. Valider : l'AP de test obtient une IP.
5. ACCÈS : AP un par un, vérifiés dans l'app (en ligne, bon site).
6. CONFIG : VLAN, SSID, DHCP, portail captif, règles.
7. TESTS : chaque SSID, chaque VLAN, isolation, débit, roaming à pied.
8. SAUVEGARDE : export config + fiche site + photos.
9. TRANSMISSION : former le contact client (2-3 gestes : redémarrer, lire une alerte).
10. SUIVI : inspection cloud à J+2 et J+7.
```

**Pourquoi cet ordre :** chaque étape valide la précédente. Si tu configures les SSID avant d'avoir Internet, tu ne sauras jamais si « ça ne marche pas » vient du Wi-Fi ou de l'uplink.

## 71. Étiquetage et documentation physique : le professionnalisme visible

- Chaque câble : étiquette aux **deux extrémités** (`B12-P07` = baie 1, prise 2, port 7 du switch — exemple fictif de convention).
- Chaque AP : étiquette discrète avec son nom logique (`Etage1-AP02`).
- Chaque port du switch : correspond au plan (ports 1-4 = AP, 5-20 = PC…).
- Dans la baie : un **schéma plastifié** du brassage + la fiche site (section 65).
- Le client voit la différence entre « des câbles » et « une installation ». C'est aussi ce qui te fait rappeler.

## 72. Mise à la terre et parafoudre : ne pas négliger

- Les switchs eKit ont une protection interne (±6-7 kV d'après les fiches S310), mais en zone orageuse ou réseau électrique instable : **parafoudre en tête** + **terre < 5 ohms** (cf. ton guide onduleurs).
- Les **AP761 extérieurs** : mise à la terre du mât, parafoudre PoE sur la ligne si exposition (toit, zone dégagée).
- Un AP grillé par la foudre, c'est un RMA refusé (« surtension » = exclusion de garantie typique). Protège, et documente la protection.

## 73. Plan de nommage des équipements (convention proposée)

```
[SITE]-[ETAGE]-[TYPE][NUM]
Exemples fictifs :
  HOTELPALM-RDC-AR01      (passerelle)
  HOTELPALM-E1-SW01       (switch etage 1)
  HOTELPALM-E1-AP01..AP10 (AP chambres etage 1)
  HOTELPALM-EXT-AP01      (AP761 piscine)
```
Avantages : tri alphabétique = tri géographique dans l'app ; une alerte « HOTELPALM-E1-AP07 hors ligne » est immédiatement localisable.

## 74. Check-list de mise en service (à cocher sur site)

- [ ] Internet OK sur l'AR (ping 8.8.8.8 — exemple fictif d'IP publique de test — + DNS résout)
- [ ] Tous les équipements **verts** dans l'app, sur le **bon site**
- [ ] Chaque SSID testé avec un vrai client (connexion + Internet + débit)
- [ ] Isolation invités vérifiée (pas d'accès au VLAN staff, pas de ping inter-clients)
- [ ] Portail captif testé sur Android ET iPhone
- [ [ ] Imprimante et équipements filaires OK
- [ ] Caméras : image OK sur le NVR, PoE stable
- [ ] Téléphones IP : appel interne + externe
- [ ] Firmwares notés, config sauvegardée, fiche site remplie
- [ ] Photos baie + AP + étiquettes
- [ ] Client formé aux 3 gestes de base, numéro d'astreinte communiqué
- [ ] Inspection cloud planifiée à J+2

## 75. Les 10 commandements du déploiement eKit (pense-bête)

1. **Internet d'abord** : rien ne se configure avant que l'AR ait son uplink.
2. **Un site = un site** : nomme, cloisonne, documente.
3. **Pré-stage au bureau** : scanne et prépare avant d'aller sur site.
4. **PoE calculé, pas supposé** : additionne, marge 30 %.
5. **3 SSID max** : staff, invités, technique si besoin.
6. **VLAN invités isolé** : toujours, sans exception.
7. **Photos des SN avant** de fixer les AP au plafond.
8. **Teste en marchant** : le roaming ne se valide pas assis.
9. **Sauvegarde le jour J** : config + fiche + photos.
10. **Reviens à J+2/J+7** (via le cloud) : un site qu'on ne revoit jamais finit par tomber en panne un dimanche.

---

## 76. Supervision via eKit : ce que tu vois vraiment

D'après les fiches techniques, la plateforme cloud supervise **l'état du réseau, l'état des équipements et l'état des connexions clients (STA)** sur tous les sites du tenant. En pratique, organise ta supervision autour de 4 vues :
1. **Vue santé globale** : tous les sites, code couleur. Ton rituel du matin (5 min).
2. **Vue site** : équipements en ligne/hors ligne, charge, alertes du site.
3. **Vue équipement** : détail d'un AP/switch/AR (état, clients associés, version firmware, uptime — niveau de détail **à vérifier** selon version).
4. **Vue clients** : qui est connecté où (utile pour « le Wi-Fi est lent dans la salle de réunion » → 30 clients sur 1 AP).

## 77. Les alertes qui comptent (et celles qu'on peut ignorer)

| Alerte | Priorité | Action |
|---|---|---|
| Équipement hors ligne | **Haute** | Vérifier cloud (tout le site ? → uplink/électrique ; un seul AP ? → PoE/câble), puis cycle PoE, puis déplacement |
| Nouvel équipement détecté | Moyenne | Normal après ajout ; suspect sinon (intrusion ?) |
| Taux d'utilisation CPU/mémoire élevé | Moyenne | Surveiller, planifier remplacement si récurrent |
| Client DHCP épuisé (scope plein) | Moyenne | Élargir le scope ou réduire le bail |
| Interférences / changement de canal fréquent | Basse | Survey radio si récurrent |
| Firmware disponible | Basse (info) | Planifier selon section 42 |

**Règle :** une alerte sans action définie = du bruit. Pour chaque type d'alerte, écris qui fait quoi (ta procédure d'astreinte, section 101).

## 78. Supervision eKit vs vrai NMS : le comparatif honnête

| Fonction | eKit (app/SNC) | Vrai NMS (Zabbix, PRTG, NCE…) |
|---|---|---|
| État en ligne/hors ligne | Oui | Oui |
| Alertes de base | Oui | Oui, bien plus fines |
| Historique de performance (débit, erreurs) | Limité (**à vérifier**) | Oui, graphes long terme |
| NetFlow / sFlow / analyse de trafic | Non (à ma connaissance) | Oui |
| Supervision d'équipements tiers | Non | Oui (SNMP générique) |
| Corrélation d'événements | Non | Oui (règles) |
| API / webhooks / intégration ticketing | À vérifier sur la documentation officielle | Oui |
| Cartographie réseau auto | Basique (à vérifier) | Oui |
| Coût | Inclus (annoncé gratuit) | Licence / infra à prévoir |

**Conclusion :** eKit suffit pour **surveiller** 5-50 sites PME. Dès que tu veux **analyser** (pourquoi c'est lent tous les jours à 14 h ?), il te faut un NMS complémentaire — même simple (un Zabbix qui ping les équipements eKit en SNMP si les modèles l'exposent — **à vérifier** par modèle).

## 79. Compléter eKit avec un NMS léger : la bonne combinaison

- Garde eKit pour : onboarding, config, alertes « équipement hors ligne », gestion multi-sites.
- Ajoute un NMS (Zabbix/PRTG/LibreNMS — tu as des guides complets dans ton workspace) pour : ping/SNMP des passerelles et switchs, graphes de débit des uplinks, disponibilité sur 30 jours (utile pour prouver un SLA ou diagnostiquer un problème récurrent).
- Les switchs eKit supportent **SNMPv1/v2c/v3** d'après les fiches (S220, S310, S620) : c'est ta porte d'entrée NMS. Active SNMPv3 (authentifié/chiffré), jamais v1/v2c en production si tu peux l'éviter.
- Documente les **community strings / credentials SNMP** dans ton coffre (section 102), pas dans la fiche site.

## 80. Les KPI à suivre par site (tableau de bord mensuel client)

| KPI | Cible (ordre de grandeur) | Source |
|---|---|---|
| Disponibilité équipements | > 99,5 % (PME) | Cloud eKit / NMS |
| Temps moyen de détection panne | < 15 min | Alertes cloud |
| Temps moyen de résolution | < 4 h (ouvré) | Tes tickets |
| Clients Wi-Fi max simultanés / AP | < 30-40 (bureautique) | Vue clients cloud |
| Taux de réussite portail captif | > 95 % | Tests / retours |
| Firmwares à jour | 100 % des sites (N-1 max) | Cloud eKit |

Un rapport mensuel d'une page avec ces KPI, c'est ce qui transforme « le gars du Wi-Fi » en « notre prestataire infra ». Et ça justifie le contrat de maintenance.

## 81. Quand le client grandit : les signaux d'alerte

Surveille ces signaux — ils annoncent qu'eKit atteint ses limites :
- plus de **30-40 AP** sur un site unique avec besoin de roaming fin ;
- besoin de **802.1X / NAC** (authentification par certificats, contrôle d'accès dynamique) ;
- besoin de **routage dynamique** (OSPF/BGP), de **VPN maillés** complexes, de **SD-WAN** avancé ;
- **plusieurs Gbit/s** de trafic inter-VLAN à router en permanence ;
- exigences de **redondance** (double passerelle, stacking, double alimentation) contractuelles ;
- équipe IT interne qui veut du **CLI/API/automatisation** (Ansible, Terraform) ;
- secteurs réglementés exigeant des **journaux centralisés** et de l'audit fin.
Dès 2-3 signaux : ouvre le dossier « migration Datacom » (sections 83-90), ne bricole pas.

## 82. Ce que la gamme Datacom entreprise apporte (pour situer)

Sans entrer dans le détail (tu as tes propres ressources Datacom), la gamme entreprise Huawei apporte :
- **iMaster NCE** : le vrai contrôleur campusc/datacenter (politiques, automatisation, assurance réseau) ;
- switchs **S600 et supérieurs**, **CloudEngine** : 25/40/100G, stacking, redondance ;
- **HiSecEngine USG6700F** : pare-feux haut de gamme (100+ Gbit/s) ;
- **AirEngine** (AP entreprise) + **WAC** dédiés : roaming fin, 802.1X, IoT ;
- routage dynamique complet (OSPF, BGP, MPLS), VXLAN/EVPN.
Le prix et la complexité vont avec : c'est un autre métier (ou un partenaire à trouver).

## 83. Critères de décision : rester eKit ou passer Datacom — la grille

| Critère | Reste eKit | Passe Datacom |
|---|---|---|
| Utilisateurs / site | < ~200 | > 300-500 |
| AP / site | < 30 | > 50 ou roaming critique |
| Sites | Multi-sites simples | Topologie hub-and-spoke complexe, SD-WAN |
| Authentification | PSK / portail captif | 802.1X, certificats, NAC |
| Routage | Statique, 1-2 uplinks | OSPF/BGP, multi-opérateurs avec bascule fine |
| Redondance | Onduleur + 4G de secours suffisent | Double équipement exigé contractuellement |
| Admin | App/cloud, pas d'expert réseau | Équipe réseau avec compétences CLI/API |
| Budget | Serré, OPEX minimal | Budget d'infrastructure assumé |

**Phrase à dire au client :** « Aujourd'hui eKit couvre votre besoin avec de la marge. Si vous dépassez [seuil chiffré], on basculera telle brique en Datacom — c'est prévu, pas une surprise. »

## 84. Migration eKit → Datacom : anticiper sans tout jeter

Bonne nouvelle : une migration n'est pas un big-bang.
- Les **AP eKit en mode Fit** peuvent théoriquement rejoindre un WAC (à valider par modèle — **à vérifier sur la documentation officielle** : tous les modèles ne basculent pas).
- Le **câblage** (Cat6, baie, chemins) est réutilisable à 100 %.
- Le **plan d'adressage et les VLAN** se transposent tels quels.
- La **passerelle AR eKit** devient un routeur d'agence ou est remplacée par un AR entreprise / USG supérieur.
- Stratégie : migrer **par couche** (d'abord le cœur : passerelle + switchs d'agrégation, puis les AP par zone), en gardant le cloud eKit pour les sites restants en attendant leur tour.

## 85. Le piège du « on mettra du Datacom plus tard » sans plan

Si tu sais dès le départ que le client dépassera eKit dans 18 mois :
- câble en **Cat6A** (pas Cat5e), prévois des **fourreaux** en réserve ;
- prends des switchs avec des **uplinks 10G** (S310-48P4X plutôt que 48P4S) même si inutilisés aujourd'hui ;
- documente un **plan d'adressage extensible** (/23 plutôt que /24) ;
- écris la trajectoire dans la proposition commerciale (« phase 1 eKit, phase 2 Datacom si > X utilisateurs »).
Le surcoût initial est faible ; le coût d'un recâblage dans 18 mois est énorme.

## 86. Coexistence eKit + Datacom : c'est possible

Un groupe peut très bien avoir : le siège en Datacom (NCE + S600) et 15 agences en eKit supervisées via le cloud. C'est même une architecture **recommandée** économiquement : du lourd où c'est complexe, du simple où c'est simple. Points d'attention : interconnexion via VPN IPsec ou lignes privées, adressage cohérent global, et **deux outils de supervision** (NCE + cloud eKit) — à intégrer dans ton NMS central via SNMP quand c'est possible.

## 87. L'argument commercial honnête face aux concurrents

Face à un concurrent qui propose du « tout-cloud avec licence annuelle » :
- eKit : gestion annoncée **sans licence**, matériel souvent moins cher, déploiement rapide par des non-experts.
- Limites à assumer : écosystème fermé, fonctions avancées limitées, dépendance au cloud Huawei.
Face à du Datacom (Huawei ou autre) :
- eKit : 2-3× moins cher à l'achat, déploiement en jours pas en semaines, maintenance sans expert réseau.
- Ne joue jamais eKit contre du Datacom sur un besoin Datacom : tu perdras le client à la première panne grave. Qualifie le besoin d'abord (section 15).

## 88. Garantie et RMA : ce qu'il faut savoir

- Les distributeurs annoncent typiquement **3 ans de garantie** sur les switchs eKit (ex. : S310 chez les distributeurs cités) — **à vérifier** pour chaque modèle auprès de ton distributeur (la durée peut varier : AP, AR, USG).
- **Procédure RMA** : via ton distributeur Gold (pas directement Huawei en général). Garde : facture d'achat, SN, description de panne, photos.
- **Stock tampon** : pour un client multi-sites, négocie 1 AP et 1 switch de secours chez toi (ou chez le client). Un AP en RMA qui met 2 semaines, c'est 2 semaines de zone blanche.
- **Exclusions typiques** : surtension/foudre (d'où la section 72), mauvaise installation, firmware non officiel. Documente tes installations (photos, parafoudre) pour ne jamais être en tort.

## 89. Sauvegarde des configurations : la méthode complète

1. **Cloud** : la config des sites est conservée dans le cloud eKit (principe du pilotage centralisé) — c'est ta sauvegarde n°1, mais pas la seule.
2. **Export local** : exporte la configuration de chaque site après chaque changement majeur (fichier daté : `CLIENT-Site_2026-09-27.cfg` — nom fictif d'exemple). Stockage : dossier du site + copie hors site (ton NAS / cloud pro).
3. **Fiche site** (section 65) : la version papier/lisible par un humain de l'essentiel.
4. **Avant chaque changement** : sauvegarde + note (« avant : … / après : … »). En cas de problème, tu reviens en arrière en minutes au lieu de reconstruire de mémoire.
5. **Test de restauration** : une fois par an, restaure une config sur un équipement de labo (ou simule la procédure). Une sauvegarde jamais testée n'est pas une sauvegarde.
6. **Rétention** : garde les N-3 dernières sauvegardes par site.

## 90. Mises à jour firmware : le détail par famille

| Famille | Mécanisme | Via cloud | Points d'attention |
|---|---|---|---|
| Switchs (S220/S310/S620) | **HOUP** (dépôt de mise à jour en ligne Huawei) : le switch trouve son chemin de MAJ, 1 clic, pré-chargement | Oui (app/SNC) | Coupure brève malgré le pré-chargement ; Perpetual PoE = les AP restent alimentés |
| AP | Mise à jour via cloud/app | Oui | Les clients sont déconnectés pendant la MAJ → heures creuses |
| AR (passerelles) | Via cloud/app | Oui | Coupe Internet du site pendant la MAJ → prévenir, heures creuses |
| USG6000F-S | Via interface locale/cloud + signatures (centre de sécurité Huawei) | À vérifier | MAJ signatures = sans coupure en général ; MAJ firmware = coupure → planifier |

**Règle commune :** lire la release note, sauvegarder, site pilote, vagues, fenêtre annoncée (section 42). Et **jamais de mise à jour à distance sans personne joignable sur site** : si ça tourne mal, il faut une main pour le reset physique.

---

## 91. Dépannage : la méthode avant les cas (à afficher dans l'atelier)

```
1. QUOI ?  -> Symptome exact, perimetre (1 client ? 1 AP ? tout le site ?)
2. QUAND ? -> Depuis quand ? Qu'est-ce qui a change juste avant ?
3. OU ?    -> Localiser : cloud eKit (quel equipement est rouge ?)
4. COUCHE PAR COUCHE (bas -> haut) :
   Physique (cable, PoE, LED) -> Liaison (lien up ?) -> IP (DHCP ?)
   -> Wi-Fi (SSID visible ?) -> Service (Internet ? portail ?)
5. UNE HYPOTHESE, UN TEST : ne change jamais 3 choses a la fois.
6. NOTER : symptome, cause, solution, dans le dossier du site.
```

**Le réflexe n°1 :** ouvrir l'app eKit et regarder **quel est l'état du site**. 50 % des diagnostics se font là, en 2 minutes.

## 92. Cas n°1 — Un équipement ne s'onboard pas (reste invisible dans l'app)

**Symptômes :** l'AP/switch/AR n'apparaît pas dans le site après scan ou déploiement Wi-Fi.
**Diagnostic :**
1. L'équipement est-il **sous tension** ? (LED allumée ?)
2. A-t-il **accès à Internet** ? (pour un AP derrière un switch : le switch a-t-il son uplink ? pour l'AR : le WAN est-il monté ?)
3. Le **SN scanné** est-il le bon ? (erreur de lecture, étiquette d'un autre carton — ça arrive plus souvent qu'on ne croit)
4. L'équipement est-il déjà onboardé sur **un autre site/compte** ? (matériel d'occasion ou de démo)
5. Firmware trop ancien pour le déploiement par Wi-Fi → passer par le **scan de code-barres** (cas prévu par la fiche technique).
**Solution :** corriger la cause (câble, uplink, re-scan du bon SN, suppression de l'ancien site), puis relancer l'onboarding. Si l'équipement vient d'un autre compte : le **retirer proprement** de l'ancien site d'abord (ou reset usine + ré-onboard — comportement exact **à vérifier sur la documentation officielle**).

## 93. Cas n°2 — Le cloud ne voit plus tout un site (tous les équipements hors ligne)

**Symptômes :** dans l'app, tout le site passe au rouge d'un coup. Les utilisateurs sur place disent « Internet ne marche plus » (ou parfois « ça marche encore en local »).
**Diagnostic :**
1. **Coupure électrique ?** Appeler le contact sur place : « est-ce que la baie est allumée ? »
2. **Lien opérateur tombé ?** (box/ONT éteint, travaux dans la rue)
3. **Passerelle AR plantée ?** (plus de ping, LED anormale)
4. Si le réseau **local fonctionne encore** (imprimante OK, fichiers OK) → c'est uniquement l'uplink Internet ou le lien cloud qui est coupé, pas le LAN.
**Solution :** selon la cause — faire réenclencher le disjoncteur, redémarrer la box opérateur **puis** l'AR (dans l'ordre : modem → AR → switch → AP), appeler l'opérateur si le lien reste down. **Ne jamais** reset les équipements eKit dans ce cas : le problème est en amont, pas dans leur config. Au retour : vérifier que tout repasse au vert dans le cloud (délai de quelques minutes normal).

## 94. Cas n°3 — AP adopté mais aucun SSID diffusé

**Symptômes :** l'AP est vert dans l'app, mais aucun SSID n'apparaît sur les téléphones.
**Diagnostic :**
1. Les SSID sont-ils **associés à cet AP** (ou au groupe de cet AP) dans la config du site ? (Erreur classique : SSID créés mais non appliqués aux nouveaux AP.)
2. L'AP a-t-il **reçu sa configuration** ? (forcer une re-synchronisation depuis le cloud — libellé exact **à vérifier** dans l'app.)
3. Les **radios** sont-elles activées ? (un AP en « monitor » ou avec radios désactivées ne diffuse rien.)
4. Conflit de **pays/réglementation** ? (canaux 6 GHz interdits localement sur un AP673H par ex. — vérifier la conformité locale.)
**Solution :** associer les SSID à l'AP, re-pousser la config, vérifier l'état des radios. Tester avec un téléphone à 2 mètres de l'AP (si ça marche à 2 m mais pas à 20 m, c'est un problème de couverture, pas de diffusion).

## 95. Cas n°4 — Clients connectés au Wi-Fi mais « pas d'Internet »

**Symptômes :** le Wi-Fi s'associe, mais pas de navigation. Le plus fréquent de tous les tickets.
**Diagnostic (dans l'ordre) :**
1. Le client a-t-il une **IP** ? (169.254.x.x = pas de DHCP → problème DHCP ou VLAN)
2. Le client ping-t-il la **passerelle** ? (non → problème L2/VLAN/SSID ; oui → problème routage/DNS/uplink)
3. Le **DNS** résout-il ? (`nslookup` vers un nom connu — si l'IP passe mais pas le DNS, c'est le DNS)
4. Le **portail captif** s'affiche-t-il ? (sur SSID invités : si le portail ne se déclenche pas, tester l'URL de détection captive du téléphone)
5. Un seul client ou tous ? (un seul → oublier le réseau sur le client et reconnecter ; tous → infra)
**Solution :** selon l'étage du problème — relancer le DHCP (scope plein ? bail trop long ?), corriger le VLAN du SSID, corriger les DNS sur l'AR, vérifier l'uplink Internet.

## 96. Cas n°5 — Wi-Fi « lent » (le ticket le plus flou)

**Symptômes :** « c'est lent » — sans plus de précision. Ne jamais partir en chasse sans mesurer.
**Diagnostic :**
1. **Mesurer** : speedtest à côté de l'AP, puis à l'endroit de la plainte. Noter les chiffres.
2. **Combien de clients** sur l'AP concerné ? (vue clients du cloud — 40 clients sur un AP361 = saturation, pas panne)
3. **Le lien filaire** de l'AP est-il en 1 Gbit/s ? (un AP Wi-Fi 6 bridé en 100 Mbit/s à cause d'un mauvais câble = débit plafond à ~90 Mbit/s)
4. **Interférences** : l'AP change-t-il de canal sans arrêt ? Un four à micro-ondes / un vieux AP voisin en 2,4 GHz ?
5. **L'uplink Internet** est-il saturé ? (tester en filaire direct sur l'AR pour isoler Wi-Fi vs Internet)
**Solution :** ajouter un AP si densité, remplacer le câble si lien 100 Mbit/s, figer les canaux si instabilité, limiter le débit invités si c'est eux qui saturent, augmenter le débit opérateur si l'uplink est le goulot.

## 97. Cas n°6 — Le portail captif ne s'affiche pas

**Symptômes :** connexion au SSID invités OK, mais pas de page d'accueil ; Internet « ne marche pas ».
**Diagnostic :**
1. Tester sur **2 téléphones différents** (iPhone + Android — les mécanismes de détection captive diffèrent).
2. Le client a-t-il une IP du VLAN invités ? (non → DHCP/VLAN)
3. Le DNS du VLAN invités fonctionne-t-il ? (la détection captive repose sur DNS + HTTP)
4. Un **VPN** actif sur le téléphone ? (un VPN bloque souvent la détection du portail — le couper pour le test)
5. Date/heure du téléphone correctes ? (un certificat HTTPS avec une date fausse = page bloquée)
**Solution :** corriger DHCP/DNS du VLAN invités, communiquer le **PSK invités de secours** (section 40) en attendant, et afficher en réception une petite affichette « Wi-Fi : si la page ne s'ouvre pas, ouvrez votre navigateur sur [adresse] » (adresse fictive à définir par site).

## 98. Cas n°7 — Un AP redémarre en boucle

**Symptômes :** l'AP apparaît/disparaît du cloud, les clients se déconnectent par vagues.
**Diagnostic :**
1. **PoE insuffisant ?** (cas n°1 : AP761 sur port 802.3af au lieu de at → fonctions limitées / redémarrages ; vérifier la classe PoE négociée)
2. **Budget PoE du switch dépassé ?** (ajout récent d'une caméra ? le switch coupe les ports les moins prioritaires)
3. **Câble défectueux** (un brin qui fait des micro-coupures = renégociations PoE en boucle → tester au testeur, remplacer le câble)
4. **Surchauffe** (AP dans un faux plafond à 50 °C ?)
5. **Firmware corrompu** (après une MAJ interrompue → re-flasher)
**Solution :** passer l'AP sur un port PoE+ / at, alléger le budget PoE, changer le câble, ventiler, re-flasher. **Ne jamais** laisser un AP « qui redémarre de temps en temps » : c'est toujours un symptôme qui empire.

## 99. Cas n°8 — Après une mise à jour, plus rien ne marche

**Symptômes :** MAJ firmware poussée hier soir, ce matin le site est en vrac.
**Diagnostic :**
1. Quels équipements ont été mis à jour ? Lesquels sont en panne ?
2. La **release note** mentionnait-elle un changement de comportement ? (ex. : nouveau mode de gestion par défaut)
3. La configuration a-t-elle **survécu** à la MAJ ? (comparer avec la sauvegarde d'avant-MAJ, section 89)
**Solution :** si un seul équipement : le **revenir en arrière** (downgrade vers la version précédente — procédure **à vérifier sur la documentation officielle** par modèle) ou restaurer sa config. Si tout le site : restaurer les configs une par une depuis la sauvegarde. **Leçon :** la prochaine fois, site pilote + 48 h d'observation (section 42). Et on ne fait pas de MAJ le vendredi.

## 100. Cas n°9 à n°16 — En rafale (symptômes → piste → solution)

**N°9 — Un switch ne distribue plus de PoE sur aucun port.** Piste : alimentation du switch (LED PWR ?), budget PoE désactivé après MAJ, surchauffe. Solution : vérifier l'alim, réactiver le PoE dans la config, laisser refroidir, RMA si le module PoE est mort.
**N°10 — Débit filaire plafonné à ~90 Mbit/s sur un PC.** Piste : lien négocié à 100 Mbit/s (câble ou prise défectueuse). Solution : tester le brin, refaire la prise, vérifier la négociation sur le port du switch.
**N°11 — Les téléphones IP se coupent / voix hachée.** Piste : pas de QoS, VLAN voix mélangé au data, ou switch saturé. Solution : VLAN 60 dédié, priorité QoS voix, vérifier que les téléphones sont bien sur le bon VLAN (LLDP-MED si supporté — à vérifier).
**N°12 — Une caméra ne remonte plus au NVR.** Piste : PoE (cycle PoE à distance d'abord !), IP changée (DHCP sans réservation), VLAN erroné. Solution : cycle PoE via le cloud, réservation DHCP ou IP fixe, vérifier le VLAN du port.
**N°13 — Conflit d'IP : « un autre appareil utilise cette adresse ».** Piste : IP fixe en doublon avec le scope DHCP, ou un équipement invité avec IP en dur. Solution : exclure les IP fixes du scope DHCP (toujours !), passer en réservation DHCP.
**N°14 — Le VPN IPsec ne monte pas (AR/USG).** Piste : paramètres IKE phase 1/2 incohérents entre les deux sites, NAT devant l'AR, PSK erroné, port UDP 500/4500 filtré. Solution : comparer les configs des deux côtés ligne par ligne, vérifier le NAT-T, tester sans NAT si possible.
**N°15 — Impossible d'accéder à l'interface locale d'un équipement.** Piste : gestion locale désactivée en mode cloud, mauvaise IP / mauvais VLAN depuis le poste, HTTP/HTTPS désactivé. Solution : se mettre sur le bon VLAN avec une IP du bon sous-réseau, vérifier que la gestion locale est autorisée (ou repasser temporairement en gestion locale selon le modèle).
**N°16 — L'app eKit ne se connecte plus à mon compte.** Piste : mot de passe expiré/modifié, compte verrouillé, problème réseau du téléphone, maintenance cloud Huawei. Solution : vérifier sur un 2e appareil, réinitialiser le mot de passe via l'e-mail du compte, vérifier le statut du cloud (distributeur / support Huawei), et en attendant : gestion locale des équipements si elle est activée.

---

## 101. Maintenance : le plan annuel type (par site)

| Fréquence | Action | Durée indicative |
|---|---|---|
| **Hebdomadaire** | Rituel cloud 15 min : dashboard global → sites orange/rouge → action ou ticket (section 43) | 15 min |
| **Mensuelle** | Vérifier les alertes récurrentes, les scopes DHCP (taux de remplissage), l'état des firmwares (N-1 max ?) | 30 min/site |
| **Trimestrielle** | Revue des accès (section 34), test d'un cycle PoE sur un AP témoin, dépoussiérage baie, vérification onduleur (voyants, test batterie) | 1 h/site |
| **Semestrielle** | Survey Wi-Fi rapide (tour avec smartphone : débits aux points clés), vérification étiquetage, mise à jour fiche site | 2 h/site |
| **Annuelle** | Mise à jour firmware planifiée (section 42), test de restauration de sauvegarde (section 89), révision du plan d'adressage, renégociation contrat de maintenance | 1/2 journée |

**Le contrat de maintenance, c'est ça que tu vends** : pas « on viendra si ça tombe en panne », mais ce plan, écrit, chiffré, avec les KPI de la section 80.

## 102. Sécurité des accès : le coffre et les règles

- **Coffre de mots de passe** (Bitwarden, KeePass — ton choix) : un coffre **par client**, partagé uniquement avec les techniciens habilités. Jamais de PSK ou d'admin dans un e-mail, un SMS ou un post-it sur la baie.
- **Mots de passe** : uniques par site et par équipement critique, 20+ caractères générés. Les exemples de ce guide (`BUREAU-Staff`, `192.168.10.1`) sont **fictifs** — ne les utilise jamais tels quels.
- **Comptes** : 1 compte propriétaire générique par tenant (section 17), comptes nominatifs pour les techniciens, retrait **immédiat** au départ d'un technicien ou à la fin d'une prestation.
- **Gestion locale** : change les identifiants par défaut **dès** la première configuration, désactive les protocoles non utilisés (Telnet si présent — préférer SSH/HTTPS).
- **SNMP** : v3 avec authentification si le modèle le supporte (les switchs eKit annoncent SNMPv1/v2c/v3 — utilise v3).
- **Journal** : note chaque accès admin sensible (qui, quand, quoi) dans le dossier du site.

## 103. Partage d'accès avec le client : le cadre contractuel

À écrire noir sur blanc dans le contrat :
- qui administre quoi (toi : tout ; client : lecture seule ou rien — section 28) ;
- ce qui se passe si le client modifie la config (intervention facturée, exclusion de garantie sur la partie modifiée) ;
- la **réversibilité** : en fin de contrat, transfert du tenant/compte au client ou à son nouveau prestataire (procédure + délai + format des exports). Un client « otage » de son prestataire, c'est illégal dans l'esprit et toxique commercialement — prévois la sortie dès l'entrée.
- les **données** : où sont hébergées les données de gestion (cloud Huawei), qui y a accès, durée de conservation.

## 104. Sécurité réseau : les 10 règles non négociables

1. VLAN invités **isolé**, toujours.
2. PSK robustes, uniques par site, **tournés** au moins 1×/an (et à chaque départ d'un employé sensible).
3. WPA3 quand les clients le permettent, WPA2-AES minimum — **jamais** de WPA/TKIP ni de réseau ouvert sans portail.
4. SSID staff **non diffusé** (obscurité ≠ sécurité, mais ça réduit le bruit).
5. Caméras et IoT sur VLAN dédiés, **sans accès Internet** sauf besoin (NVR accessible via VPN).
6. Gestion des équipements sur VLAN ou via cloud uniquement — pas d'admin depuis le Wi-Fi invités.
7. Mises à jour firmware suivies (une faille non patchée = la porte ouverte).
8. USG6000F-S (ou équivalent) dès que le client a des données sensibles : IPS/AV/filtrage URL activés, signatures à jour.
9. Sauvegardes chiffrées, stockées hors site.
10. **Personne ne branche** un équipement personnel sur le LAN staff (ou alors VLAN invités). Le PC portable du cousin « juste pour imprimer », c'est non.

## 105. Pièces de rechange : le stock minimal

| Pour | Stock conseillé | Pourquoi |
|---|---|---|
| Parc < 5 sites | 1 AP361 + 1 petit switch PoE (S220) | Couvre 80 % des pannes (l'AP est l'élément le plus exposé) |
| Parc 5-20 sites | + 1 AR180 + 1 S310 + modules SFP de secours + 2 alimentations | Autonomie RMA de 2 semaines |
| Client hôtel / critique | 2 AP + 1 switch **sur site client** (dans la réserve, étiquetés) | Intervention en minutes, pas en jours |
| Toujours | Câbles RJ45, jarretières fibre, étiquettes, colliers, 1 testeur | Le consommable qui manque un dimanche soir |

**Règle :** un équipement de secours **pré-configuré** (onboardé sur le site, config synchronisée) se remplace en 10 minutes. Un équipement « à configurer » se remplace en 2 heures.

## 106. Registre des accès et traçabilité (modèle)

```
REGISTRE DES ACCES - [Client]
Date       | Systeme      | Compte/Role      | Action                 | Par
2026-09-27 | eKit cloud   | admin (proprio)  | Creation site + onboard| Zelef
2026-09-28 | AR180 Pro    | admin local      | Changement mdp defaut  | Zelef
...        | ...          | ...              | ...                    | ...
```
Tenu à jour = preuve de professionnalisme en cas d'audit ou de litige. 2 minutes par intervention, valeur inestimable.

## 107. Pense-bête de poche — déploiement (à imprimer)

```
eKit DEPLOIEMENT EXPRESS
1. Internet sur l'AR d'abord (ping + DNS)
2. Ordre : modem -> AR -> switch -> AP
3. Scan SN AVANT de fixer les AP au plafond
4. 3 SSID max | VLAN invites isole | PSK 20+ car.
5. PoE : additionner + 30% de marge
6. Tester en marchant (roaming), pas assis
7. Photos : baie, SN, AP, etiquettes
8. Sauvegarde + fiche site le jour J
9. Client forme (3 gestes), astreinte communiquee
10. Revoir le cloud a J+2 et J+7
```

## 108. Pense-bête de poche — dépannage (à imprimer)

```
eKit DEPANNAGE EXPRESS
1. Ouvrir l'app : QUEL equipement est rouge ?
2. Tout le site rouge -> electrique / operateur / AR (pas les AP !)
3. 1 AP rouge -> PoE / cable (cycle PoE a distance d'abord)
4. Wi-Fi associe mais pas d'Internet -> IP ? passerelle ? DNS ? portail ?
5. "C'est lent" -> MESURER (chiffres, pas d'impression)
6. Apres MAJ qui casse -> sauvegarde d'avant-MAJ + downgrade
7. Une hypothese, un test. Noter. Ne jamais changer 3 choses a la fois.
8. Si on ne comprend pas en 30 min : escalader (distributeur / support)
```

## 109. Pense-bête de poche — les chiffres à retenir

```
AP361 : Wi-Fi 6, 1,775 Gbit/s, PoE af ~9 W, 1x GE
AP761 : Wi-Fi 6 EXT, PoE at conseille, 17,7 W max, 1x GE + 1x SFP
S310-48P4S : 48x GE PoE+, 4x SFP 1G, 380 W, 104 Gbit/s
S310-48P4X : 48x GE PoE+, 4x SFP+ 10G, 380 W, 176 Gbit/s
S220-8P4S : 8x GE + 4x SFP, 125 W PoE+
AR180/Pro : Wi-Fi 7, 4-5x GE + 1x 2.5GE WAN, 100-150 terminaux, 8-16 AP geres
AR280 : 4x GE + 1x 2.5GE, 16 AP, 32 equip. geres, 16 tunnels IPsec
USG6000F-S125/150/200 : ~600/1000/2500 utilisateurs reco.
Regles : 1 AP / 100-150 m2 (bureau placo) | 30-40 clients/AP max
         PoE : marge 30% | Cable : Cat6, 90 m max
         192.168.1.0/24 et 192.168.0.0/24 : INTERDITS en pro
```

## 110. Glossaire eKit (les 30 termes à connaître)

- **eKit** : marque distribution Huawei pour les PME (lancée 2023).
- **eKitEngine** : gamme datacom eKit (AR, AP, switchs, USG).
- **eKitOptix (MiniFTTO)** : fibre jusqu'au bureau/chambre (F700D, FG736).
- **eKitStor / IdeaHub** : stockage / écrans collaboratifs (hors réseau).
- **SNC (SME Network Center)** : cloud de gestion des réseaux eKit.
- **Onboarding** : déclaration/rattachement d'un équipement au cloud.
- **Site** : unité d'organisation dans l'app (1 bâtiment = 1 site).
- **Tenant** : compte client/fournisseur dans le cloud, contient les sites.
- **Fit / Fat / Cloud** : modes de fonctionnement d'un AP (contrôleur / autonome / cloud).
- **WAC** : contrôleur Wi-Fi (fonction intégrée aux AR280/AR281).
- **SSID** : nom du réseau Wi-Fi diffusé.
- **PSK** : clé pré-partagée (mot de passe Wi-Fi).
- **WPA2 / WPA3** : protocoles de chiffrement Wi-Fi.
- **Portail captif** : page d'authentification avant accès Internet (invités).
- **VLAN** : réseau local virtuel (cloisonnement logique, 802.1Q).
- **Trunk / Access** : port multi-VLAN taggé / port mono-VLAN.
- **PoE (af/at/bt)** : alimentation par le câble réseau (15,4 / 30 / 60-90 W).
- **Perpetual / Fast PoE** : PoE maintenu pendant reboot / réalimentation rapide.
- **HOUP** : dépôt en ligne des mises à jour Huawei (switchs).
- **MU-MIMO / OFDMA / Beamforming** : technos Wi-Fi 6 d'efficacité radio.
- **MLO / 320 MHz / 4096-QAM** : technos Wi-Fi 7 (débit, latence).
- **Band steering** : orientation des clients vers le 5 GHz.
- **Roaming (802.11r/k/v)** : passage fluide d'un AP à l'autre.
- **STA** : station = client Wi-Fi.
- **IPS / AV / URL filtering** : prévention d'intrusion / antivirus / filtrage web (USG).
- **IPsec / SSL VPN** : tunnels chiffrés site-à-site / accès distant.
- **NVR** : enregistreur vidéo réseau (fonction intégrée à l'AR281).
- **iGuard (AP673H)** : détection de caméras espion par IA (gestion locale uniquement).
- **Wi-Fi Shield** : fonctions anti-écoute mises en avant par Huawei sur le Wi-Fi 7.
- **RMA** : retour matériel en garantie via le distributeur.
- **Gold Partner** : distributeur agréé eKit (ton support niveau 1).

## 111. Quiz — 10 questions pour valider (réponses en section 112)

1. Un AP761 consomme jusqu'à 17,7 W. Sur quel standard PoE faut-il l'alimenter, et pourquoi pas en 802.3af simple ?
2. Ton S310-48P4S a un budget PoE de 380 W. Tu y branches 20 AP361 (~9 W), 2 AP761 (~17,7 W) et 8 caméras (~10 W). Le budget tient-il avec 30 % de marge ?
3. Dans l'app, tout un site passe au rouge d'un coup mais le client dit que « les fichiers en local marchent encore ». Où cherches-tu en premier ?
4. Cite les 3 modes de fonctionnement d'un AP eKit et dis dans quel cas tu choisis chacun.
5. Pourquoi ne faut-il jamais utiliser 192.168.1.0/24 sur un site professionnel ?
6. Un client veut 5 SSID « pour chaque service ». Que lui réponds-tu, et combien en proposes-tu ?
7. Après une mise à jour firmware, un site est en vrac. Quelles sont tes 3 premières actions, dans l'ordre ?
8. À partir de quels signaux dois-tu proposer de basculer du eKit vers du Datacom entreprise ? (cite-en 3)
9. L'AP673H détecte les caméras espion. Pourquoi cette fonction ne remonte-t-elle pas dans le cloud ?
10. Un hôtel veut du Wi-Fi dans 40 chambres en construction neuve. Quelles 2 architectures peux-tu proposer, et quel est l'arbitrage ?

## 112. Quiz — réponses

1. **802.3at (PoE+) recommandé** : la fiche technique précise qu'en 802.3af certaines fonctions peuvent être limitées. Avec 17,7 W max, on est au-delà des 15,4 W du af.
2. Calcul : 20×9 = 180 W ; 2×17,7 = 35,4 W ; 8×10 = 80 W → **295,4 W**. Avec 30 % de marge : 295,4 × 1,3 = **384 W > 380 W** : ça ne tient pas avec la marge. Il faut répartir sur 2 switchs ou réduire la charge.
3. **En amont des équipements** : alimentation du site, lien opérateur (box/ONT), passerelle AR. Le LAN local fonctionne → le problème est l'uplink Internet (ou le lien vers le cloud), pas les AP/switchs.
4. **Fit** (piloté par WAC — ex. AR280/AR281 en tête), **Fat** (autonome — petit site ou dépannage), **Cloud** (piloté par app/SNC — le standard eKit multi-sites).
5. Parce que c'est la **plage par défaut des box grand public** : dès qu'on branche une box, un routeur 4G ou un VPN, les conflits d'adressage sont garantis.
6. Chaque SSID consomme de l'airtime (beacons) et dégrade tout le monde : on propose **3 SSID max** (staff, invités, + 1 technique si vraiment nécessaire) et on fait le tri des usages réels avec le client.
7. (1) Identifier quels équipements ont été mis à jour et lesquels sont en panne ; (2) comparer la config actuelle à la **sauvegarde d'avant-MAJ** ; (3) restaurer/downgrader vers la version précédente stable. Et noter la leçon : site pilote + 48 h la prochaine fois.
8. Exemples : > 30-50 AP avec roaming fin ; besoin de 802.1X/NAC ; routage dynamique (OSPF/BGP) ; redondance matérielle contractuelle ; débits inter-VLAN multi-Gbit/s permanents ; équipe réseau voulant CLI/API/automatisation.
9. Pour des **raisons de confidentialité** (fiche constructeur) : la détection spycam fonctionne en **gestion locale uniquement**, les données ne partent pas dans le cloud.
10. **(a) Cuivre classique** : AR280/USG + switchs S310 PoE + AP361 (1 pour 2 chambres) — moins cher, s'appuie sur un câblage cuivre standard. **(b) MiniFTTO eKitOptix** : OLT + 1 F700D par chambre en fibre — plus cher à l'achat, esthétique et évolutivité supérieures, idéal en neuf. Arbitrage : CAPEX vs OPEX, esthétique, compétence fibre disponible.

## 113. Pour aller plus loin

- **Documentation officielle** : ekit.huawei.com (portail), support.huawei.com (fiches techniques, guides d'installation, release notes — à consulter **avant chaque déploiement** pour le modèle exact).
- **Vidéos constructeur** : le distributeur eKit local publie souvent des tutoriels d'onboarding via l'app — demande à ton Gold Partner ses liens.
- **Formations** : renseigne-toi auprès de ton distributeur sur les formations/certifications eKit (argument commercial + montée en compétence de ton équipe).
- **Communauté** : forums Huawei Enterprise (e.huawei.com) — retours d'autres intégrateurs sur les firmwares et les cas tordus.
- **Dans ton workspace** : croise ce guide avec `onduleurs_ups_guide.md` (dimensionnement électrique des baies), `debian_ubuntu_guide.md` / `proxmox_guide.md` (si tu héberges un NMS), et les guides `*_guide.md` réseau pour la supervision complémentaire.
- **Prochaine étape terrain** : monte une **maquette** (1 AR + 1 switch + 2 AP) dans ton atelier et déroule les sections 19-24 : c'est en onboardant pour de vrai que l'app n'aura plus de secret.

## 114. Sources et avertissement de fraîcheur

Informations constructeur vérifiées par recherche web (septembre 2026) :
- Fiches techniques **eKitEngine AR180 / AR280** (hardware description, datasheets ES — via e-file.huawei.com et distributeurs).
- Fiches techniques **AP361, AP761, AP266, AP673H (iGuard)** (datasheets constructeur et distributeurs).
- Fiches techniques **S310-48P4S / 48P4X / 24J4X / 48J4X, S220-8P4S, S620** (distributeurs, datasheet S310).
- Fiche technique **USG6000F-S** (S125/S150/S200, V600R025).
- Communiqués Huawei **MWC 2025** (20+ produits eKit, SNC, troubleshooting 2.0) et **Intelligent Office Solution 2.0** (2026 : AR281, DF10).
- Portail **ekit.huawei.com** (cité dans les fiches).
**Avertissement :** la gamme eKit évolue vite (nouveautés 2025-2026 : AR281, AP772/AP772E, F700D, USG6000F-S). Les références, les fonctions de l'app/SNC et les conditions de licence peuvent changer : **revérifie la fiche exacte du modèle avant chaque chiffrage**, et traite tout « à vérifier sur la documentation officielle » de ce guide comme une action, pas comme une formule.

## 115. Note de l'auteur : comment utiliser ce guide sur le terrain

1. **Avant-vente** : sections 1-15 (périmètre) + 83 (grille de décision) + un scénario (46-60) = ta proposition technique.
2. **Jour J** : fiche site (65) + pense-bêtes (107-109) imprimés + check-list (74).
3. **Exploitation** : rituel hebdo (43), plan annuel (101), KPI mensuels (80).
4. **Panne** : méthode (91) + cas (92-100) + pense-bête dépannage (108).
5. **Fin de contrat** : transfert documenté (103) + registre (106) à jour.
Ce guide vit : annote-le à chaque déploiement (un cas tordu = un cas n°17 à ajouter). Dans un an, il vaudra de l'or.

---

## 116. Fiche terrain — AP361 (l'ouvrier de base)

- **Positionnement :** AP intérieur Wi-Fi 6 d'entrée de gamme, le plus déployé en PME.
- **Radio :** 2,4 GHz 2×2 (575 Mbit/s) + 5 GHz 2×2 (1,2 Gbit/s), 1,775 Gbit/s agrégés, MU-MIMO, OFDMA, 1024-QAM.
- **Antenne :** intelligente intégrée (ajustement auto de la couverture) — pas d'antenne externe.
- **Ports :** 1× GE (données + PoE-in 802.3af). Pas de 2e port sur certaines déclinaisons (la fiche de base cite 1× GE ; une variante cite uplink + downlink — **à vérifier sur la documentation officielle** du modèle exact).
- **Alimentation :** PoE 802.3af, ~8,8-9,4 W. Un injecteur PoE fait l'affaire pour un AP isolé.
- **Modes :** Fit, Fat, Cloud. **Montage :** plafond ou mural (kit inclus selon distributeur).
- **Bonus :** port USB pour extension IoT (ZigBee/RFID selon fiche), Bluetooth pour O&M.
- **Cas d'usage :** bureaux, chambres d'hôtel, boutiques, écoles, cliniques.
- **Limite :** 1 Gbit/s d'uplink — ne pas le mettre là où il faudrait du 2,5G (forte densité Wi-Fi 7).
- **Prix indicatif :** le moins cher de la gamme AP eKit — **à vérifier auprès du distributeur** (les prix varient fortement par pays).

## 117. Fiche terrain — AP761 (l'extérieur)

- **Positionnement :** AP extérieur Wi-Fi 6 (AX1800 : 574 + 1201 Mbit/s).
- **Ports :** 1× GE PoE-in + **1× SFP** (fibre — parfait pour les longues distances : parking, portail, dépendance).
- **Alimentation :** 802.3at **recommandé** (en af, fonctions limitées), 17,7 W max.
- **Particularités :** BLE 5.2 (gestion, localisation, IoT), MU-MIMO, Beamforming, TWT.
- **Installation :** mât ou mur, **mise à la terre + parafoudre** si exposé (section 72), presse-étoupes bien serrés (l'eau ne pardonne pas).
- **Cas d'usage :** terrasses, piscines, parkings, cours d'école, quais semi-ouverts.
- **Piège :** l'oublier dans le plan de nommage (« l'AP dehors » n'est pas un nom). Le nommer `SITE-EXT-AP01` dès l'onboarding.

## 118. Fiche terrain — AP266 (le simple et efficace)

- **Positionnement :** AP intérieur Wi-Fi 6, gestion cloud, **authentification intégrée** (PSK + portail) sans WAC ni serveur externe.
- **Déploiement :** par Wi-Fi (connexion au Wi-Fi de gestion) ou par scan du SN — les deux modes documentés par la fiche.
- **Cas d'usage :** quand le client veut un portail captif simple sans ajouter d'USG : le couple AP266 + cloud suffit.
- **À vérifier sur la documentation officielle :** débit exact, nombre de ports, puissance PoE — la fiche distributeur reste sommaire.

## 119. Fiche terrain — AP572 (la haute densité Wi-Fi 7)

- **Positionnement :** AP intérieur **Wi-Fi 7 haute densité**, pensé pour salles de classe et grandes salles de réunion (annonce MWC 2025).
- **Cas d'usage :** écoles, amphis, open spaces denses, salles de conférence.
- **À vérifier sur la documentation officielle :** specs radio exactes (flux, débit), PoE (probablement at/bt), prix et disponibilité — modèle récent.
- **Conseil :** ne le déploie pas « parce que c'est du Wi-Fi 7 » dans un bureau de 5 personnes — l'AP361 suffit et coûte moins cher. Le Wi-Fi 7 paie en **densité**, pas en prestige.

## 120. Fiche terrain — AP673E (Wi-Fi 7 haute densité, 2e option)

- **Positionnement :** Wi-Fi 7 intérieur haute densité, cité aux côtés de l'AP572 pour les mêmes usages (classes, conférences).
- **À vérifier sur la documentation officielle :** différences exactes avec l'AP572 (positionnement, prix, specs) — les deux modèles coexistent dans les annonces 2025.
- **Réflexe commercial :** demande au distributeur lequel est **stocké localement** — un modèle légèrement moins bon mais dispo en 48 h bat un modèle parfait en 6 semaines.

## 121. Fiche terrain — AP673H iGuard (le confidentiel)

- **Positionnement :** AP intérieur **Wi-Fi 7 tri-bande** (2,4 GHz 2×2 + 5 GHz 2×2 + **6 GHz 4×4**), 8 flux spatiaux, **13,66 Gbit/s** théoriques.
- **Différenciant :** **détection de caméras espion** par IA (analyse des ondes, y compris caméras non connectées au Wi-Fi) — voir section 29.
- **Contrainte officielle :** détection spycam en **gestion locale uniquement** (pas de cloud pour cette fonction).
- **Cas d'usage :** hôtels haut de gamme, salles de direction, cabinets sensibles, résidences VIP.
- **À vérifier :** disponibilité de la bande 6 GHz dans ton pays (réglementation), prix (positionné premium), et la procédure exacte de scan/rapport.

## 122. Fiche terrain — AP772 / AP772E (l'extérieur Wi-Fi 7)

- **AP772 :** extérieur Wi-Fi 7, antenne **omnidirectionnelle 360°** — couverture large (place, cour).
- **AP772E :** extérieur Wi-Fi 7, antenne **directionnelle** — couverture longue distance ciblée (allée, parking en longueur, liaison entre bâtiments proches).
- **À vérifier sur la documentation officielle :** specs détaillées, PoE, indice de protection, prix — annoncés en 2025.
- **Règle :** directionnel = on vise une zone précise ; omnidirectionnel = on couvre autour. Ne mets pas un omni là où il faut un directionnel « parce qu'il est moins cher » : tu arroseras les voisins et pas la cible.

## 123. Fiche terrain — AR180 / AR180 Plus / AR180 Pro

- **Concept :** passerelle tout-en-un (routage + switch + Wi-Fi + gestion Internet), 8 fonctions intégrées d'après Huawei.
- **AR180 / Plus :** 4× GE LAN + 1× 2,5GE WAN, Wi-Fi 7 double bande, ~100 terminaux, 8 tunnels IPsec, 200 Mbit/s VPN, 2 Gbit/s de contrôle de bande passante, **sans ventilateur**, pas de PoE, pas de port console.
- **AR180 Pro :** 5 ports commutables + 1× 2,5GE, ~150 terminaux, 16 tunnels IPsec, **8 AP max / 32 équipements gérés**, 256 VLAN, boîtier métal.
- **Antennes :** 4 externes intelligentes, non démontables, 23 dBm max (réglementation locale).
- **Cas d'usage :** AR180 = TPE/boutique/bureau < 15 personnes ; AR180 Pro = PME jusqu'à ~50 personnes avec quelques AP.
- **Limite dure :** 8 AP max sur le Pro — au-delà, passer à l'AR280.

## 124. Fiche terrain — AR280 (le chef d'orchestre PME)

- **Ports :** 4× GE (1 LAN dédié + 3 LAN/WAN configurables) + 1× 2,5GE (WAN/LAN configurable). **Pas de Wi-Fi intégré** d'après la fiche — le Wi-Fi passe par les AP.
- **Capacité :** ~150 terminaux, **16 AP max, 32 équipements gérés**, 16 tunnels IPsec, 200 Mbit/s VPN.
- **Gestion :** planification, déploiement, inspection et O&M via la plateforme cloud Huawei SME.
- **Cas d'usage :** la tête de réseau standard des déploiements 20-100 utilisateurs (bureau, hôtel, école).
- **Point d'attention :** la fiche mentionne une capacité PoE (41 W) — modèle ou variante à clarifier (**à vérifier sur la documentation officielle** : ne compte pas dessus pour alimenter des AP sans confirmation).

## 125. Fiche terrain — AR281 (le tout-en-un 2026)

- **Annonce 2026** (Intelligent Office Solution 2.0) : « un appareil = 5 fonctions » — routage, **NVR** (vidéosurveillance), **WAC** (contrôleur Wi-Fi), accès ligne privée, gestion du comportement en ligne.
- **Stockage vidéo :** compression « SuperCoding 3.0 » annoncée à -85 % (1 disque = 6× la capacité) — à valider en conditions réelles.
- **Cas d'usage visé :** petite structure voulant réseau + Wi-Fi + vidéosurveillance dans un seul boîtier.
- **À vérifier sur la documentation officielle :** TOUT (specs, prix, dispo) — produit très récent au moment de la rédaction. Ne le chiffre pas sans fiche en main.

## 126. Fiche terrain — S220-8P4S (le petit switch qui dépanne)

- **Ports :** 8× GE + 4× SFP 1G, **125 W PoE+** (802.3af/at), 24 Gbit/s, 18 Mpps.
- **Fonctions :** Fast PoE, Perpetual PoE, SNMP v1/v2c/v3, CLI, web, SSH, DHCP snooping, isolation de ports, sticky MAC, 802.1X.
- **Environnement :** -5 à +50 °C, ventilation intelligente (silencieux ajusté).
- **Cas d'usage :** boutique, agence, petit bureau (8 ports PoE suffisent pour 2-3 AP + 3-4 caméras), switch de secours dans ton stock.
- **Limite :** 8 ports — dès 6-7 équipements PoE, passe au S310 24 ports pour la marge.

## 127. Fiche terrain — S310-48P4S (le cheval de labour)

- **Ports :** 48× GE PoE+ + 4× SFP 1G, **380 W PoE+**, 104 Gbit/s, 77 Mpps.
- **Gestion :** app eKit (cloud) **ou** locale (CLI/web/SNMP/SSH) — commutable.
- **Particularités :** HOUP (MAJ intelligente en 1 clic), stacking zéro-config, protection ±7 kV / ±6 kV, MTBF ~48 ans.
- **Cas d'usage :** LE switch des hôtels, écoles, bureaux 30-100 utilisateurs.
- **Règle PoE :** 380 W = 24 ports à 15,4 W (af) ou 12 ports à 30 W (at) en pleine charge — **au-delà, le switch gère par priorité** : configure les priorités PoE (AP critiques d'abord).

## 128. Fiche terrain — S310-48P4X (le 10G)

- **Ports :** 48× GE PoE+ + **4× SFP+ 10G**, 380 W PoE+, 176 Gbit/s, 131 Mpps.
- **Différence avec le 48P4S :** les uplinks passent à 10 Gbit/s — pour agréger plusieurs switchs ou un serveur/NAS gourmand.
- **Cas d'usage :** tête de distribution d'un hôtel/école avec 2-3 switchs d'étage, ou site avec NAS de sauvegarde.
- **Surcoût vs 48P4S :** à chiffrer — si aucun besoin 10G à 3 ans, le 48P4S suffit. Si le client « grandit vite », le 48P4X est l'assurance (section 85).

## 129. Fiche terrain — S310 tout-optique (24J4X / 48J4X)

- **Ports :** 24 ou 48× **SFP 2,5GE** (tout-optique) + 4× SFP+ 10G. Capacités : 200 Gbit/s (24) / 320 Gbit/s (48).
- **Cas d'usage :** colonnes montantes en fibre (hôtel, campus), environnements perturbés électromagnétiquement, longues distances inter-bâtiments.
- **Contrainte :** 100 % fibre = 100 % modules SFP à acheter + compétence fibre (soudures, réflectométrie). Ne le propose que si tu maîtrises ou sous-traites la fibre.
- **Astuce :** en hôtel, une dorsale fibre + des S310-48P4S cuivre par étage = le meilleur des deux mondes.

## 130. Fiche terrain — USG6000F-S (le pare-feu IA)

- **Modèles :** S125 (~600 utilisateurs, 1,7 Gbit/s NGFW) / S150 (~1000, 2,1 Gbit/s) / S200 (~2500, 3 Gbit/s). Débits « enterprise mix » : 1 / 1,2 / 1,8 Gbit/s avec tout activé.
- **Fonctions :** pare-feu, IPS, antivirus, filtrage URL (500 M d'URL), IPsec (2000-4000 tunnels), SSL VPN (100 inclus, jusqu'à 1000-2000), portail captif local, SSO RADIUS/AD/LDAP, SD-WAN sécurisé.
- **Dimensionnement :** on dimensionne sur le **débit tout activé**, pas le débit brut (section 7).
- **Cas d'usage :** PME avec données sensibles, hôtel (portail + sécurité), multi-sites avec VPN.
- **À vérifier :** licences des fonctions avancées et abonnement signatures auprès du distributeur avant chiffrage.

---

## 131. Cas pratique — Restaurant 80 couverts (commenté)

**Contexte :** restaurant avec terrasse, 12 employés, caisse + 2 TPE, Wi-Fi clients gratuit, 4 caméras, pas d'IT.
**Matériel :** 1× AR180, 1× S220-8P4S, 2× AP361 (salle + terrasse couverte), 1× AP761 si terrasse très exposée (à la place du 2e AP361).
**VLAN :** 10 (caisse/TPE/staff), 20 (clients, portail captif), 40 (caméras).
**Commentaire :** le point critique, ce sont les **TPE** : jamais de MAJ en service, VLAN 10 prioritaire, et un **câble 4G de secours** sur la caisse si le restaurateur l'accepte (un samedi soir sans TPE = catastrophe). Le portail captif clients = page avec la carte des desserts + avis Google (marketing). Puissance radio modérée : la terrasse ne doit pas arroser toute la rue (sinon 30 passants squattent le Wi-Fi).

## 132. Cas pratique — École primaire 12 classes (commenté)

**Contexte :** 12 classes, bureau direction, 300 élèves (pics : 1 classe = 30 tablettes), fibre 1 Gbit/s.
**Matériel :** 1× AR280, 2× S310-48P4S (un par aile), 12-14× AP361 (1 par classe + couloirs), onduleur 3000 VA.
**VLAN :** 10 (admin/profs), 20 (élèves, filtré), 40 (caméras), 70 (TBI/vidéoprojecteurs si IP).
**Commentaire :** la densité est le sujet : 30 tablettes × 12 classes qui se connectent à 8 h = **storm DHCP et pics radio**. Baux DHCP courts, band steering vers 5 GHz, et surtout **filtrage web** (USG6000F-S125 ou filtrage DNS) — une école sans filtrage, c'est un incident avec les parents assuré. Prévoir 1 AP par classe, pas 1 pour 2 : les murs de classes sont souvent en dur.

## 133. Cas pratique — Clinique 20 lits (commenté)

**Contexte :** clinique privée, 20 lits, 30 soignants, dossiers patients informatisés, Wi-Fi patients, 10 caméras, exigence de confidentialité.
**Matériel :** 1× USG6000F-S125 (sécurité : données de santé), 1× AR280 ou l'USG en tête, 2× S310-48P4S, 10× AP361, onduleur 3000 VA + groupe si possible.
**VLAN :** 10 (soignants/dossiers — chiffré, audité), 20 (patients/invités, isolé), 40 (caméras), 60 (téléphonie/DECT).
**Commentaire :** ici la **sécurité n'est pas une option** : données de santé = USG obligatoire, journaux conservés, accès tracés (registre section 106), WPA3 si les terminaux suivent. Le Wi-Fi soignants (chariots de soins, tablettes) = SSID dédié prioritaire, jamais mélangé au Wi-Fi patients. Prévoir une **procédure écrite** remise à la direction (qui accède à quoi).

## 134. Cas pratique — Agence bancaire / assurance (commenté)

**Contexte :** agence 10 personnes, VPN vers le siège, GAB (distributeur de billets) à connecter, exigence de sécurité maximale.
**Matériel :** 1× USG6000F-S125 (VPN IPsec vers siège), 1× S220-8P4S ou S310-24P, 2× AP361.
**VLAN :** 10 (postes agence), 20 (invités, isolé), 50 (GAB — **totalement isolé**, accès siège uniquement via VPN), 99 (gestion).
**Commentaire :** le GAB ne touche **jamais** le LAN de l'agence : VLAN dédié + règles pare-feu strictes + VPN vers la banque. C'est typiquement un besoin où le **cahier des charges de la banque** prime sur tes habitudes : lis-le avant de chiffrer. eKit convient pour l'agence ; le GAB suit les règles du groupe bancaire.

## 135. Cas pratique — Espace de coworking 40 postes (commenté)

**Contexte :** 40 postes en open space + 3 bureaux fermés + salle de réunion, clients qui changent chaque mois, imprimante partagée, domiciliation.
**Matériel :** 1× AR280 (ou USG si besoin), 1× S310-48P4S, 4× AP361 (densité !), onduleur 2000 VA.
**VLAN :** 10 (staff coworking), 20 (coworkers — portail captif avec code mensuel), 21 (bureaux fermés — 1 VLAN par bureau si exigé), 40 (caméras/contrôle d'accès).
**Commentaire :** le **turnover** est le sujet : PSK coworkers **changé chaque mois** (procédure section 152), portail captif avec CGU (responsabilité du gérant en cas d'abus — mentions légales !), isolation inter-clients **impérative** (un coworker ne doit pas voir le PC du voisin). La salle de réunion = AP dédié proche + priorité visio.

## 136. Cas pratique — Salle de sport / fitness (commenté)

**Contexte :** 500 m², 200 adhérents/jour, musique en streaming, écrans, contrôle d'accès par badge, 6 caméras, Wi-Fi adhérents.
**Matériel :** 1× AR180 Pro, 1× S310-24P4S, 3× AP361 (salle muscu, cardio, accueil), onduleur 1500 VA.
**VLAN :** 10 (staff/musique/caisse), 20 (adhérents, portail captif), 40 (caméras + contrôle d'accès).
**Commentaire :** la **musique** passe souvent par le réseau (streaming) : SSID ou VLAN prioritaire pour ne pas hacher quand 50 adhérents se connectent à 18 h. Humidité/chlore si piscine : AP standard **hors** zone humide, ou modèle adapté. Les vestiaires : pas d'AP dedans (confidentialité), couverture depuis le couloir.

## 137. Cas pratique — Lieu de culte / grande salle (commenté)

**Contexte :** salle de 500 places, streaming des cérémonies (caméras + diffusion), Wi-Fi public modéré, 4 caméras.
**Matériel :** 1× AR280, 1× S310-24P4S, 3-4× AP361/AP572 (densité le jour d'affluence), onduleur 2000 VA.
**VLAN :** 10 (régie/streaming — **prioritaire absolu**), 20 (public, limité), 40 (caméras).
**Commentaire :** le **streaming** est critique et gourmand (montant) : VLAN dédié, QoS, et surtout **ne jamais faire de MAJ le jour d'une cérémonie**. Le Wi-Fi public : débit bridé (5 Mbit/s), sinon 500 fidèles en direct tuent le streaming. Tester en conditions réelles (répétition) avant le jour J.

## 138. Cas pratique — Pharmacie (commenté)

**Contexte :** pharmacie 100 m², 6 employés, robot de stockage (parfois en réseau), TPE, ordonnances électroniques, 4 caméras, Wi-Fi clients discret.
**Matériel :** 1× AR180, 1× S220-8P4S, 2× AP361, onduleur 1000 VA.
**VLAN :** 10 (officine : postes, TPE, robot), 20 (clients, discret voire non diffusé en continu), 40 (caméras).
**Commentaire :** le **robot** et les **TPE** = critiques : IP fixes, VLAN 10, jamais de MAJ en journée. Les données de santé (ordonnances) imposent la même rigueur qu'une clinique (section 133) à petite échelle. Le Wi-Fi clients : utile mais secondaire — ne jamais le laisser impacter l'officine.

## 139. Cas pratique — Garage / concession automobile (commenté)

**Contexte :** showroom + atelier (poussière, métal), 15 employés, diagnostic véhicules en Wi-Fi (valises), 8 caméras, Wi-Fi clients showroom.
**Matériel :** 1× AR180 Pro, 1× S310-24P4S, 3× AP361 (showroom, atelier, bureau), onduleur 1500 VA.
**VLAN :** 10 (bureaux/valises diag), 20 (clients showroom), 40 (caméras).
**Commentaire :** l'**atelier** est un enfer radio (métal, ponts élévateurs) : survey obligatoire, AP en hauteur dégagée, et les **valises de diagnostic** = SSID dédié si elles sont capricieuses en roaming. Poussière : baie fermée, nettoyage annuel des AP. Les caméras extérieures (parc véhicules) : AP761 + caméras sur mâts avec parafoudre.

## 140. Cas pratique — ONG / bureau de terrain (commenté)

**Contexte :** bureau de projet 15 personnes, connexion Internet instable (4G principale, VSAT en secours), coupures électriques fréquentes, budget serré.
**Matériel :** 1× AR180 (4G en USB si supporté — **à vérifier** — sinon routeur 4G devant l'AR), 1× S220-8P4S, 2× AP361, **onduleur 2000 VA + batteries étendues** (cf. guide onduleurs).
**VLAN :** 10 (staff), 20 (invités/partenaires).
**Commentaire :** ici l'**énergie** est le sujet n°1, pas le Wi-Fi : dimensionne l'onduleur pour 2-4 h (les coupures sont la norme), active le **redémarrage auto** partout, et mets le cloud eKit en mode « supervision quand ça passe » (les alertes « site hors ligne » seront fréquentes : règle les seuils pour ne pas spammer). Pré-stage tout au siège avant envoi sur le terrain — sur place, on ne fait que brancher.

## 141. Cas pratique — Cybercafé / salle de gaming (commenté)

**Contexte :** 30 PC gaming filaires, fibre 2 Gbit/s, tournois le week-end, 20 spectateurs en Wi-Fi, 4 caméras.
**Matériel :** 1× AR280 ou USG6000F-S150 (2 Gbit/s !), 1× S310-48P4X (**uplinks 10G**), 2× AP361 (spectateurs), onduleur 3000 VA.
**VLAN :** 10 (PC gaming — **latence minimale**), 20 (spectateurs Wi-Fi, bridé), 40 (caméras).
**Commentaire :** le gaming = **latence**, pas débit : pas de QoS qui ajoute du traitement inutile, pas de Wi-Fi pour les joueurs (filaire obligatoire), firmware stable (jamais de MAJ avant un tournoi). Le S310-48P4X se justifie ici (2 Gbit/s d'uplink). Mesure le ping, pas le débit, pour valider.

## 142. Cas pratique — Boulangerie / chaîne de 5 boutiques (commenté)

**Contexte :** 5 boutiques, chacune : 6 employés, 2 caisses, 3 caméras, Wi-Fi clients, pas d'IT sur place.
**Matériel (par boutique) :** 1× AR180, 1× S220-8P4S, 1-2× AP361. **Cloud :** 1 tenant, 5 sites, modèle « boutique standard ».
**VLAN :** 10 (caisses/staff), 20 (clients), 40 (caméras).
**Commentaire :** le multi-sites eKit **brille** ici : 1 modèle de config, 5 sites identiques, alertes par boutique avec le nom du responsable. Le technicien n'a besoin d'aller sur place que pour le physique. Procédure écrite remise à chaque responsable : « si Internet tombe : 1) vérifier la box, 2) appeler l'astreinte, 3) ne toucher à rien d'autre ». Et les caisses : jamais de MAJ entre 6 h et 20 h.

## 143. Cas pratique — Cabinet comptable / d'avocats (commenté)

**Contexte :** 12 personnes, données clients ultra-sensibles, secret professionnel, VPN vers 2 clients, 3 caméras.
**Matériel :** 1× USG6000F-S125 (IPS/AV/filtrage + VPN), 1× S220-8P4S ou S310-24P, 2× AP361, onduleur 1500 VA.
**VLAN :** 10 (collaborateurs), 20 (invités, isolé, **jamais** de PSK partagé avec le staff), 40 (caméras), 99 (gestion).
**Commentaire :** secret professionnel = **USG obligatoire**, WPA3, PSK tourné au départ de chaque collaborateur, registre des accès (106) tenu à jour, et **clause de confidentialité** avec toi le prestataire. Le Wi-Fi invités : PSK différent du staff, changé régulièrement. C'est aussi un client qui paiera un contrat de maintenance premium — propose-le.

## 144. Cas pratique — Station-service / boutique autoroute (commenté)

**Contexte :** boutique + 6 pompes (automates), TPE, 8 caméras, Wi-Fi clients, site isolé, 24h/24.
**Matériel :** 1× AR280 ou USG, 1× S310-24P4S, 2× AP361 (boutique) + 1× AP761 (piste, zone ATEX à respecter !), onduleur 2000 VA.
**VLAN :** 10 (boutique/TPE), 20 (clients), 40 (caméras), 50 (automates pompes — **isolé, critique**).
**Commentaire :** les **automates de pompes** = système critique, VLAN dédié, jamais touché sans l'accord de l'exploitant pétrolier (souvent un prestataire dédié — **coordination obligatoire**). Zone ATEX : aucun AP actif dans les zones classifiées sans certification — l'AP761 couvre **depuis** l'extérieur de la zone. Site 24/24 : fenêtres de maintenance à 3 h du matin, jamais de MAJ sans astreinte sur place.

## 145. Cas pratique — Résidence hôtelière / appart-hôtel (commenté)

**Contexte :** 30 appartements, séjours longs, box TV par appartement, 60+ clients simultanés le soir, laverie commune, parking.
**Matériel :** 1× AR280 ou USG6000F-S125, 2× S310-48P4S, 15× AP361 (1 pour 2 apparts) + 1× AP761 (parking/laverie), onduleur 3000 VA.
**VLAN :** 10 (staff/réception), 30 (résidents — portail captif, baux longs 7 jours), 40 (caméras), 70 (TV si IP).
**Commentaire :** séjours longs = **baux DHCP longs** (pas de coupure à minuit quand le bail expire en plein film), **isolation inter-clients** (les résidents restent des semaines : ils *essaieront* de voir le voisin), et un **débit équitable** par client (sinon 3 gamers monopolisent la fibre). La laverie et le parking : souvent oubliés dans le chiffrage — prévois-les dès le départ.

---

## 146. Tableau de référence — canaux Wi-Fi (plan de fréquences)

| Bande | Canaux utilisables (Europe/CEPT, ordre de grandeur) | Bonnes pratiques |
|---|---|---|
| 2,4 GHz | 1 à 13 (20 MHz) — canaux non chevauchants : **1, 6, 11** | N'utiliser que 1/6/11 en 20 MHz. Réserver le 2,4 GHz aux objets connectés et au secours. |
| 5 GHz | 36-64, 100-140 (20/40/80 MHz selon pays) | 40 MHz en bureau dense, 80 MHz si peu d'AP voisins. Éviter les canaux DFS (52-140) si des coupures radar sont constatées. |
| 6 GHz (Wi-Fi 7) | Selon réglementation locale | **Vérifier la disponibilité légale dans ton pays** avant de déployer l'AP673H en 6 GHz. |

**Règle :** en multi-AP, alterne les canaux (1/6/11 au rez, 6/11/1 à l'étage) et baisse la puissance plutôt que de monter les canaux en puissance. La réglementation locale (puissance max, canaux autorisés) prime toujours — **à vérifier sur la documentation officielle** et les textes locaux.

## 147. Tableau de référence — câblage et distances

| Support | Distance max | Usage eKit typique |
|---|---|---|
| Cuivre Cat6 (PoE) | **90 m** (lien permanent) + 10 m de jarretières | AP, caméras, téléphones |
| Cuivre Cat6A | 90 m (10G jusqu'à 100 m) | Uplinks 10G (S310-48P4X), serveurs |
| Fibre OM3/OM4 (multimode) | 300-400 m (10G) | Dorsales inter-bâtiments, colonnes |
| Fibre OS2 (monomode) | 10 km et + | Inter-sites, MiniFTTO (selon OLT) |
| SFP+ DAC (cuivre twinax) | 1-3 m (parfois 5-7 m) | Liaison switch-switch dans la même baie |

**Règles :** jamais de câble cuivre > 90 m pour du PoE (chute de tension + erreurs) ; en extérieur : **câble extérieur (UV) ou gaine**, jamais du câble intérieur au soleil ; teste chaque brin au testeur (continuité + cartographie des paires), pas « à l'œil ».

## 148. Tableau de référence — budgets types par scénario (ordres de grandeur)

| Scénario | Matériel eKit type | Échelle de budget matériel* |
|---|---|---|
| Boutique (C) | AR180 + S220-8P4S + 2 AP361 | € (le plus accessible) |
| Bureau 20 postes (A) | AR180 Pro + S310-24P + 3 AP361 | €€ |
| Restaurant (131) | AR180 + S220 + 2-3 AP | € |
| Entrepôt (D) | AR280 + S310-24P + 4-6 AP | €€€ |
| Hôtel 40 ch. (B) | AR280/USG + 2 S310-48P + 20 AP361 + 2 AP761 | €€€€ |
| École 12 classes (132) | AR280 + 2 S310-48P + 12-14 AP | €€€€ |
| Clinique (133) | USG-S125 + 2 S310-48P + 10 AP | €€€€ |

*Échelle relative uniquement — **les prix réels sont à vérifier auprès de ton distributeur** (ils varient par pays, stock et remise). Ajoute toujours : câblage (~30-40 % du matériel en rénovation), onduleur, main-d'œuvre, contrat de maintenance annuel (~15-20 % du matériel/an, ordre de grandeur).

## 149. Comparatif honnête — eKit vs alternatives PME

| Critère | Huawei eKit | Concurrent cloud avec licence/an | Solution « box grand public » |
|---|---|---|---|
| Coût gestion cloud | Annoncé **gratuit/sans licence** | Licence annuelle par équipement | Gratuit mais pas de gestion |
| Déploiement | App mobile, scan QR, rapide | App/cloud aussi, parfois plus guidé | Manuel, un par un |
| Multi-sites | Oui (SNC) | Oui | Non |
| Fonctions avancées | Limitées (pas de NAC, routage simple) | Variables, souvent + complètes | Quasi inexistantes |
| Wi-Fi 7 | Oui (AP572/673/772) | Oui chez la plupart | Rare |
| Verrouillage | Écosystème Huawei | Écosystème du concurrent | Aucun (mais aucun support) |
| Support local | Distributeur Gold | Variable | Aucun |

**Lecture :** eKit gagne sur le **rapport fonctions/prix** en PME simple. Il perd dès que le besoin devient « entreprise » (section 83). Ne dénigre jamais un concurrent devant un client : compare des **faits** (prix, fonctions, support), pas des slogans.

## 150. Procédure — reset usine et ré-onboarding propre

1. **Sauvegarde** la config du site (section 89) — même si l'équipement est en vrac, la config du *site* dans le cloud reste la référence.
2. Trouve le **bouton reset** (trou ou bouton selon modèle — voir le guide d'installation du modèle).
3. **Équipement sous tension**, appuie 5-10 s (durée exacte **à vérifier sur la documentation officielle** du modèle) jusqu'au changement de LED.
4. **Attention :** le reset interrompt le service (l'AP/la passerelle redémarre d'usine). Jamais en journée sur un site en production.
5. Après redémarrage : l'équipement repasse en mode setup → **ré-onboard** via l'app (sections 19-20) sur le bon site → il récupère la configuration du site.
6. Vérifie : SSID diffusés, VLAN, PoE, état vert dans le cloud.
7. Note l'opération dans le registre (106) : un reset n'est jamais « pour rien », il y avait une cause — écris-la.

## 151. Procédure — remplacement RMA sans coupure longue

1. Dès la panne : **équipement de secours** (ton stock, section 105) onboardé sur le site — il prend la config automatiquement. Coupure : 10-15 min.
2. Déclare la panne au **distributeur** : SN, facture, symptômes, photos, ce que tu as déjà testé (un RMA bien documenté va 3× plus vite).
3. À réception du remplacement : onboard sur le site, **vérifie la config**, remets l'équipement de secours en stock.
4. Mets à jour la **fiche site** (nouveau SN) et le registre.
5. Analyse : pourquoi est-il tombé en panne ? (surtension → revoir la protection ; surchauffe → revoir la ventilation). Un RMA sans analyse = un 2e RMA dans 6 mois.

## 152. Procédure — changer le PSK invités sur N sites (multi-sites)

1. Génère le **nouveau PSK** (20+ caractères, gestionnaire de mots de passe) — jamais un mot simple, jamais l'ancien + « 1 ».
2. Dans le cloud, modifie le SSID invités du **modèle de configuration** (pas site par site).
3. Applique au **site pilote** d'abord, teste avec un téléphone (connexion + portail).
4. Déploie **site par site en heures creuses** (pas les 12 boutiques à 12 h un samedi).
5. Communique le nouveau PSK aux responsables (canal sûr, pas d'affichage public en clair si portail à code).
6. Mets à jour le **coffre** (102) et note la date de rotation (prochaine : +12 mois max, ou au départ d'un employé sensible).

## 153. Procédure — ajouter un VLAN sur un site existant (sans tout casser)

1. **Planifie** : ID VLAN libre, sous-réseau (jamais en conflit), usage, quels ports/AP concernés.
2. **Sauvegarde** la config actuelle (89).
3. Crée le VLAN + le scope DHCP dans le cloud, **sans l'appliquer** aux ports d'abord.
4. Applique sur **1 port test** (ou 1 AP test) : branche un PC/téléphone, vérifie IP + connectivité + isolation.
5. Déploie port par port (ou groupe d'AP par groupe), en vérifiant à chaque étape.
6. Mets à jour : fiche site (65), plan de brassage, documentation.
7. **Heures creuses** obligatoires si le VLAN touche des ports en production (une erreur de VLAN = des utilisateurs coupés).

## 154. Procédure — portail captif : mise en place pas à pas (logique)

1. Choisir le **mode** : portail de l'AP (simple, sans serveur) ou portail de l'USG (plus d'options : base locale, RADIUS, AD).
2. Créer le **VLAN invités** (20/30) avec DHCP, DNS fonctionnel, **isolation** activée.
3. Créer le **SSID invités** rattaché au VLAN, activer le portail captif.
4. Personnaliser la **page** : logo client, CGU, mentions légales, langue(s).
5. Définir les **quotas** : débit/client (ex. fictif : 10 Mbit/s), durée de session (ex. : 4 h commerce, 24 h hôtel), volume si besoin.
6. Tester sur **Android + iPhone** (les deux), avec et sans VPN, avec date/heure correctes et incorrectes (cas n°7).
7. Préparer le **PSK de secours** et l'affichette d'aide (section 97).
8. Former l'accueil : « voici ce que vous dites au client qui n'y arrive pas » (2 phrases, pas un manuel).

## 155. Procédure — audit de sécurité annuel d'un site eKit

- [ ] Firmwares : tous à N ou N-1 ? (sinon : planifier MAJ)
- [ ] Mots de passe : PSK tournés dans l'année ? Admin par défaut changés partout ?
- [ ] Accès cloud : revue des comptes (qui a quoi ? ex-employés retirés ?)
- [ ] VLAN : invités toujours isolés ? Aucun équipement « oublié » sur le mauvais VLAN ?
- [ ] SSID : toujours 3 max ? Aucun SSID « TEST » ou « TEMP » qui traîne ?
- [ ] SNMP : v3 uniquement ? Strings par défaut changées ?
- [ ] Ports inutilisés du switch : **désactivés** (un port libre = une prise d'entrée physique)
- [ ] Sauvegardes : existent, récentes, testées ?
- [ ] Onduleur : test batterie OK ? Autonomie conforme ?
- [ ] Photos/baie : étiquetage toujours lisible ?
Chaque point non conforme = un ticket avec échéance. L'audit tient en 2 h et évite 90 % des incidents « bêtes ».

## 156. Erreurs classiques n°17 à 25 (la suite du dépannage)

**N°17 — Le « ça marchait avant » sans sauvegarde.** Cause : changement non tracé, pas de sauvegarde. Solution : toujours sauvegarder avant (89) ; pour retrouver : interroger (« qui a touché à quoi ? »), comparer avec la fiche site, et reconstruire proprement.
**N°18 — Le DHCP qui s'épuise.** Symptômes : les nouveaux clients n'ont plus d'IP le matin. Cause : bail trop long + beaucoup de passages (hôtel, boutique). Solution : baux courts sur VLAN invités (4-12 h), élargir le scope.
**N°19 — L'AP « trop puissant ».** Symptômes : clients accrochés à l'AP du bout du couloir, débits faibles. Cause : puissance au max partout. Solution : **baisser** la puissance, ajouter des AP si besoin — la densité se règle en ajoutant des cellules, pas en criant plus fort.
**N°20 — Le câble « qui marchait » en 100 Mbit/s.** Symptômes : AP bridé à ~90 Mbit/s. Cause : paire abîmée, prise mal sertie. Solution : testeur, refaire la prise — un câble à 100 Mbit/s sur un AP Wi-Fi 6, c'est 80 % du potentiel jeté.
**N°21 — L'oubli du VLAN natif.** Symptômes : l'AP ne remonte pas / pas de DHCP. Cause : trunk avec natif incohérent entre switch et AP. Solution : aligner le natif des deux côtés (ou tout tagger explicitement).
**N°22 — La MAJ du vendredi soir.** Symptômes : week-end de panne. Cause : optimisme. Solution : MAJ en début de semaine, heures creuses, site pilote (42). Toujours.
**N°23 — Le SSID « TEST » oublié.** Symptômes : faille béante (souvent sans mot de passe). Cause : SSID de chantier jamais supprimé. Solution : l'audit annuel (155) les chasse ; nommer les SSID temporaires `ZZZ-TEMP-` pour les repérer.
**N°24 — Le switch plein à 100 % de PoE.** Symptômes : après ajout d'une caméra, un AP redémarre. Cause : budget dépassé (68). Solution : calculer avant d'ajouter, configurer les priorités PoE, 2e switch si besoin.
**N°25 — Le client qui « n'a rien touché ».** Symptômes : config modifiée, personne ne sait par qui. Cause : accès partagés / non tracés. Solution : comptes nominatifs (34), registre (106), et la phrase magique : « ce n'est pas grave, on va remettre d'aplomb — mais racontez-moi tout, même les détails bêtes ».

## 157. Plan de continuité (PRA) version PME — le minimum

Un PRA n'est pas réservé aux grands groupes. Pour chaque site critique :
1. **Inventaire vital** : passerelle, switch(s), AP critiques, onduleur — avec SN et contacts (distributeur, opérateur, électricien).
2. **Scénarios** : coupure électrique (→ onduleur + groupe ?), coupure opérateur (→ 4G de secours ?), panne passerelle (→ équipement de secours, 151), incendie/dégât des eaux de la baie (→ sauvegarde hors site, 89).
3. **RTO/RPO** : en PME, vise simple — RTO 4 h ouvrées (équipement de secours + config cloud), RPO 24 h (sauvegarde quotidienne de la config si changements fréquents).
4. **Test annuel** : coupe (simulée) — est-ce que l'onduleur tient ? Est-ce que l'équipe sait quoi faire ? Un PRA non testé = du papier.
5. **Document** : 2 pages max, dans la baie (plastifié) + chez toi. Le jour J, personne ne lit un pavé.

## 158. Former ton équipe à eKit : programme en 5 modules

1. **Fondamentaux** (1/2 j) : écosystème eKit, modèles, Fit/Fat/Cloud, VLAN/SSID/PoE de base.
2. **L'app** (1/2 j) : compte, sites, onboarding Wi-Fi + scan, config d'un site type — **en maquette** (113), pas en théorie.
3. **Déploiement** (1 j) : câblage, ordre de mise en service (70), étiquetage, check-list (74), sur un vrai site avec un senior.
4. **Dépannage** (1/2 j) : la méthode (91), les cas 92-100, le pense-bête (108), gestion de l'astreinte.
5. **Sécurité & admin** (1/2 j) : coffre, accès, sauvegardes, audit (155), relation client.
**Validation :** chaque technicien déploie seul un site « boutique » en maquette avant d'aller seul chez un client. Et ce guide = le manuel de référence de l'équipe.

## 159. Chiffrer une affaire eKit : la méthode de devis

1. **Visite technique** (jamais de devis sans voir le site) : mesures, murs, existant, besoins (check-list 15).
2. **Matériel** : liste (scénarios 46-60) + 10 % de marge (câbles, fixations, imprévus) — prix distributeur à jour.
3. **Main-d'œuvre** : câblage (au point ou au forfait), installation, configuration, tests, formation client — **ne brade jamais** la configuration et les tests, c'est là que se joue la qualité.
4. **Onduleur + électrique** : souvent oubliés — chiffre-les (lien avec ton guide onduleurs).
5. **Récurrent** : contrat de maintenance annuel (plan 101 + KPI 80) — c'est ta marge durable, propose-le systématiquement.
6. **Options** : USG (sécurité), AP supplémentaires (densité), MiniFTTO (si neuf), extension de garantie.
7. **Présentation** : 1 page de synthèse (besoin → solution → prix), puis le détail. Le client achète la **confiance**, pas la liste de courses.

## 160. Modèle de PV de recette (réception de chantier)

```
PROCES-VERBAL DE RECETTE - [Client - Site]              Date : __________
Installation realisee conformement au devis n° __________
[ ] Tous les equipements sont en ligne dans le cloud eKit (capture jointe)
[ ] SSID testes : __________ (debits releves : __________)
[ ] Isolation invites verifiee (pas d'acces LAN)
[ ] Portail captif teste (Android + iPhone)
[ ] Equipements filaires testes (imprimante, caisses, cameras)
[ ] Firmwares notes : __________
[ ] Sauvegarde remise : oui / Fiche site remise : oui / Photos : oui
[ ] Formation client effectuee (nom du forme : __________)
[ ] Reserves eventuelles : __________________________________________
Signature client : __________            Signature prestataire : __________
```
Le PV signé = la fin officielle du chantier et le début de la maintenance (et de la facturation du contrat). Sans PV, « il reste un petit truc » pendant 6 mois.

## 161. Diagnostic visuel : lire les LED (mémo générique)

Les codes exacts varient par modèle (**à vérifier sur la documentation officielle** du modèle), mais la logique est commune :
- **PWR/SYS vert fixe** : alimenté, démarré.
- **SYS clignotant** : démarrage en cours ou activité — attendre la fin du boot (1-3 min) avant de conclure à une panne.
- **LED éteinte** : pas d'alimentation → vérifier prise, PoE, câble.
- **LED port allumée** : lien établi ; **clignotante** : trafic ; **éteinte** : pas de lien → câble, port distant, PoE.
- **Rouge / orange** : alarme ou défaut → noter le motif exact (clignotement rapide/lent) et chercher dans le guide du modèle.
**Règle :** photographie les LED avant de toucher — c'est une preuve de l'état initial, utile pour le RMA et le rapport.

## 162. Câbles et connectique : le mémo de la sacoche

- **Toujours avoir** : 5 jarretières RJ45 Cat6 (0,5/1/2/3/5 m), 2 jarretières fibre (selon connectique du site), 1 injecteur PoE af/at, 1 testeur de câble, étiquettes + marqueur, colliers, tournevis, pince à sertir + 10 connecteurs RJ45.
- **Ne jamais faire** : rallonger un câble PoE avec un raccord « domino », sertir à l'arrache sans tester, laisser un câble en tension sur son connecteur, mélanger fibre monomode et multimode (ou leurs modules SFP).
- **Tester** chaque brin après sertissage : continuité + paires + longueur. Un câble non testé = un câble en panne.

## 163. eKit et la réglementation : ce qu'il faut respecter

- **Puissance d'émission** : limitée par la réglementation locale (la fiche AR180 cite 23 dBm max, variable selon pays). Ne dépasse jamais les limites — amende + brouillage des voisins.
- **Bande 6 GHz** : pas autorisée partout — vérifier avant de déployer l'AP673H en 6 GHz (sinon : 2,4/5 GHz uniquement).
- **Données personnelles** : portail captif = collecte de données (e-mail, logs) → mentions légales, consentement, durée de conservation (selon droit local).
- **Vidéosurveillance** : déclaration/autorisation selon le pays + information des personnes filmées (panneaux).
- **Sécurité électrique** : la baie et l'onduleur suivent les normes locales (un disjoncteur dédié pour la baie, c'est la base).
En cas de doute : le distributeur local et un juriste valent mieux qu'une amende.

## 164. Les 10 questions que les clients te poseront (et tes réponses)

1. **« C'est chinois, c'est fiable ? »** → « C'est du matériel pro avec 3 ans de garantie via notre distributeur local, déployé dans 70+ pays. La fiabilité se joue surtout à l'installation et à la maintenance — c'est notre métier. »
2. **« Et si Internet tombe ? »** → Section 38 : le réseau local continue, seule la gestion cloud est aveugle.
3. **« Le cloud, c'est gratuit vraiment ? »** → Section 33 : gestion annoncée sans licence ; je te détaille ce qui est inclus et les options.
4. **« On pourra changer de prestataire ? »** → Section 103 : oui, réversibilité prévue au contrat dès le départ.
5. **« Ça capte jusqu'où ? »** → « On dimensionne par pièce/zone avec un AP par X m², et on valide par des mesures — pas des promesses. »
6. **« Pourquoi pas juste des répéteurs ? »** → « Un répéteur divise le débit par 2 à chaque saut et crée des coupures. Des AP câblés, c'est le seul vrai Wi-Fi pro. »
7. **« C'est sécurisé ? »** → Section 104 : VLAN, WPA2/3, isolation invités, pare-feu en option — et je te montre l'audit annuel.
8. **« Combien de temps pour installer ? »** → « Une boutique : 1-2 jours. Un hôtel 40 chambres : 1-2 semaines avec le câblage. Je te donne un planning écrit. »
9. **« Et dans 3 ans quand on grandit ? »** → Section 83-85 : l'architecture est prévue pour grandir, avec une trajectoire écrite.
10. **« Pourquoi vous et pas un autre ? »** → « Pour la méthode : visite technique, plan écrit, PV de recette, maintenance avec KPI. Le matériel, tout le monde le vend — le service, c'est nous. »

## 165. Index et fin

**Index rapide :** adressage (64) · app eKit (16-30) · audit (155) · budgets (148) · canaux Wi-Fi (146) · chiffrer (159) · cloud/SNC (31-45) · comparatif concurrents (149) · Datacom : bascule (83-90) · dépannage méthode (91) · dépannage cas 1-16 (92-100) · dépannage cas 17-25 (156) · équipe : formation (158) · fiches modèles (116-130) · garantie/RMA (88, 151) · glossaire (110) · hôtel (50-54) · interopérabilité (61) · iGuard (29, 121) · LED (161) · maintenance plan (101) · migration existant (62-63) · migration Datacom (84) · MiniFTTO (8, 52) · multi-sites (35) · onduleur/énergie (48, 72) · PoE (68-69) · portail captif (40, 154) · PRA (157) · PV de recette (160) · quiz (111-112) · réglementaire (163) · reset (150) · sauvegarde (89) · scénarios A-D (46-60) · cas pratiques 131-145 · sécurité (102-104) · sites : organisation (18) · SNC (31) · supervision (76-80) · USG (7, 130) · VLAN (66) · SSID (67).
**Ce guide est un document vivant** : à chaque déploiement, ajoute ton cas n°26, ta fiche modèle, ton retour distributeur. Dans un an, ce sera l'actif le plus précieux de ton service.
**Fin du guide — bon déploiement.**

---

## 166. DHCP : le régler comme un pro (et éviter 50 % des tickets)

- **Un scope par VLAN**, avec la passerelle du VLAN comme routeur par défaut et des DNS valides. L'erreur n°1 : un scope sans DNS ou avec un DNS injoignable → « Internet ne marche pas » alors que l'IP est bonne.
- **Baux** : staff/bureaux = 24 h ; invités/commerce = 4-8 h ; hôtel chambres = 12-24 h ; résidence longue = 7 jours (section 145). Un bail trop long sur un VLAN à fort turnover = scope épuisé (cas n°18).
- **Exclusions** : toutes les IP fixes d'infrastructure **hors** du scope (passerelle .1, switchs .2-.9, AP .10-.49, imprimantes, NVR). Le conflit d'IP (cas n°13) vient toujours d'ici.
- **Réservations** : pour les équipements qui doivent garder leur IP (imprimante, NVR, TPE) sans IP fixe en dur — le meilleur des deux mondes.
- **Option 43 / vendor-specific** : si les AP ont besoin d'options DHCP particulières pour trouver leur contrôleur/cloud — **à vérifier sur la documentation officielle** par modèle (en mode cloud pur, l'onboarding se fait via l'app, pas via DHCP).
- **Surveillance** : alerte « scope > 80 % » si le NMS le permet — sinon, contrôle mensuel (section 101).

## 167. DNS : le maillon invisible

- Si les clients ont une IP mais « rien ne s'affiche » : c'est **presque toujours le DNS** (cas n°5, diagnostic n°3).
- Sur l'AR : DNS de l'opérateur en primaire + un DNS public en secondaire (ex. fictifs : 1.1.1.1, 8.8.8.8 — à adapter selon politique locale).
- **Ne jamais** laisser un DNS interne injoignable comme DNS unique d'un VLAN invités.
- Portail captif : le DNS du VLAN invités doit résoudre **vite** — la détection captive en dépend (cas n°7).
- Test de validation : `nslookup` (ou équivalent) d'un nom externe depuis un client de chaque VLAN, le jour J et à chaque audit.

## 168. NAT/PAT sur la passerelle AR : l'essentiel

- En PME, la passerelle fait du **NAT/PAT** (masquerading) : tout le LAN sort avec l'IP publique du WAN. C'est le mode par défaut — vérifie juste qu'il est actif.
- **Redirection de ports** (port forwarding) : à éviter autant que possible (chaque port ouvert = une surface d'attaque). Si indispensable (caméras accessibles à distance, serveur local) : règle stricte, IP source limitée si possible, et **documentée** dans la fiche site.
- Alternative propre : **VPN** (IPsec/SSL sur AR/USG) pour l'accès distant — jamais d'admin exposée sur Internet.
- **UPnP** : désactivé par défaut en pro (un malware qui ouvre ses propres ports via UPnP, c'est le scénario classique).

## 169. Routage inter-VLAN : qui parle à qui

- Par défaut sur l'AR : les VLAN se voient (routage inter-VLAN actif). **Ce n'est pas ce que tu veux** pour les invités.
- Matrice type à appliquer :
  - GUESTS → Internet : oui ; → autres VLAN : **non**.
  - STAFF → Internet : oui ; → CAMERAS : non (sauf NVR) ; → VOIX : oui si besoin.
  - CAMERAS → NVR : oui ; → Internet : non (sauf accès distant via VPN).
  - MGMT → tout (admin) ; ← tout : **non** (personne n'administre depuis les invités).
- Implémentation : règles pare-feu/ACL sur l'AR ou l'USG — le niveau d'exposition dans l'app varie (**à vérifier sur la documentation officielle** ; sinon via l'interface locale).
- **Teste la matrice** le jour J avec des pings croisés — une règle oubliée = un VLAN invités qui voit les caméras.

## 170. QoS pratique : prioriser sans se noyer

- En PME, 3 classes suffisent : **voix/temps réel** (priorité max), **bureautique** (normal), **invités** (dégradé, avec plafond).
- Marque la voix en **DSCP EF** si les téléphones le font (la plupart oui) et fais confiance au marquage bout-en-bout sur le LAN.
- Le levier le plus efficace en PME n'est pas la QoS fine : c'est le **plafond de débit** sur le VLAN invités (10-20 Mbit/s partagés ou par client — valeurs fictives d'exemple à ajuster).
- Streaming/régie (sections 137) : VLAN dédié + priorité — et surtout pas de MAJ pendant l'événement.
- Ne promets jamais « la QoS réglera votre débit » si l'uplink est saturé : la QoS **arbitre** la pénurie, elle ne crée pas du débit.

## 171. 802.1X / NAC : ce que eKit peut et ne peut pas

- Les switchs eKit annoncent l'**authentification 802.1X et MAC** (fiches S220/S310) : de quoi faire du contrôle d'accès filaire de base avec un serveur RADIUS.
- En pratique PME : le 802.1X complet (certificats, EAP-TLS) est **rarement déployé** sur eKit — c'est un marqueur typique de bascule vers le Datacom (section 81).
- Ce que tu peux faire en eKit : portail captif (invités), PSK par SSID, isolation, MAC filtering basique (faible sécurité, utile en complément).
- Si un client exige du 802.1X **Wi-Fi d'entreprise** : qualifie avec la grille (83) — c'est probablement un besoin Datacom (AirEngine + NCE).

## 172. STP/RSTP : éviter les boucles qui tuent un LAN

- Une **boucle** (deux câbles entre les mêmes switchs, ou un câble bouclé sur lui-même) = tempête de broadcast = site mort en secondes.
- Les AR eKit annoncent **RSTP** (fiches AR180) : laisse-le **activé** partout, ne le désactive jamais « pour aller plus vite ».
- En pratique : les boucles viennent d'un câble branché « pour tester » entre deux prises murales, ou d'un petit switch de bureau rebouclé. L'étiquetage (71) et les ports inutilisés désactivés (155) préviennent 90 % des cas.
- Symptôme typique : tout le site rame d'un coup, LED des switchs qui clignotent frénétiquement à l'unisson → cherche la boucle physique **avant** de toucher à la config.

## 173. Multicast / IGMP snooping : pour la TV sur IP

- TV IP d'hôtel (VLAN 70) = **multicast** : sans IGMP snooping, chaque flux TV est diffusé à tous les ports (le switch devient un hub).
- Vérifie que l'**IGMP snooping** est actif sur les switchs du VLAN TV (**à vérifier sur la documentation officielle** par modèle).
- Un VLAN TV **séparé** du VLAN chambres : un client ne doit ni voir ni perturber les flux TV.
- Dimensionnement : ~8-15 Mbit/s par flux HD (ordre de grandeur) × nombre de chaînes simultanées — à intégrer dans le budget de la dorsale.

## 174. NTP : l'heure juste, partout

- Des équipements **pas à l'heure** = logs inexploitables, certificats qui échouent, portail captif qui coince (cas n°7).
- Configure un **serveur NTP** sur l'AR (celui de l'opérateur ou un public — ex. fictif : `pool.ntp.org`) et propage-le en DHCP (option 42) aux clients qui l'acceptent.
- Vérifie l'heure et le **fuseau horaire** de chaque site dans le cloud (un site à cheval sur deux fuseaux = des alertes horodatées bizarres).
- À chaque dépannage : commence par vérifier l'heure des équipements — 5 % des « bugs mystérieux » viennent de là.

## 175. Syslog et journaux : la mémoire du réseau

- Les switchs eKit supportent **Syslog** (écosystème cité dans les fiches) : envoie les logs vers un serveur central (même un simple syslog sur ton NMS).
- En PME : au minimum, conserve les logs de la **passerelle/USG** (connexions, VPN, alertes sécurité) — durée selon la réglementation locale.
- Les logs sont ta **boîte noire** : quand le client dit « c'est tombé mardi », tu dois pouvoir dire « mardi 14:32, perte du lien WAN pendant 12 minutes ».
- Sans serveur syslog : exporte les logs avant chaque MAJ et à chaque incident majeur — un log effacé par un reboot est une preuve perdue.

## 176. Gestion des incidents : le processus (même à 3 personnes)

1. **Détection** : alerte cloud, appel client, rituel hebdo.
2. **Qualification** (5 min) : périmètre (qui est impacté ?), criticité (tout le site ? la caisse ?), workaround possible ?
3. **Ticket** : même simple (date, client, symptôme, criticité) — sans ticket, pas de suivi, pas de KPI.
4. **Diagnostic** : méthode section 91, une hypothèse à la fois.
5. **Résolution + vérification** : le client confirme que c'est bon (pas toi seul).
6. **Clôture** : cause racine écrite, fiche site mise à jour si besoin, cas ajouté au guide si nouveau.
7. **Retour d'expérience** (mensuel, 30 min en équipe) : qu'est-ce qui a cassé ce mois-ci ? Qu'est-ce qu'on change dans nos procédures ?
Un processus écrit, même d'une page, bat l'héroïsme désorganisé à tous les coups.

## 177. Astreinte : l'organiser sans y laisser sa santé

- **Plages** : définis les heures couvertes (ex. fictif : 7 h-22 h, 7 j/7 pour l'hôtellerie ; heures ouvrées pour les bureaux) — écrites au contrat.
- **Rotation** : jamais la même personne 2 semaines d'affilée. Un technicien épuisé fait des bêtises à 3 h du matin.
- **Escalade** : niveau 1 (technicien : cycle PoE, redémarrage, lecture cloud) → niveau 2 (toi : config, RMA, distributeur) → niveau 3 (distributeur/support Huawei).
- **Kit d'astreinte** : téléphone chargé, accès cloud, coffre, contacts (opérateurs, électricien, distributeur), équipement de secours pour les sites critiques.
- **Facturation** : interventions hors contrat = facturées (tarif écrit à l'avance). La gratuité systématique tue ton business.

## 178. SLA : les écrire pour de vrai

| Niveau | Exemple d'engagement (fictif, à adapter) | Pour qui |
|---|---|---|
| Bronze | Intervention sous 48 h ouvrées, sans astreinte | Bureau non critique |
| Silver | Sous 8 h ouvrées + astreinte téléphonique | Commerce, PME standard |
| Gold | Sous 4 h, 7 j/7 + équipement de secours sur site | Hôtel, clinique, industrie |

- Un SLA se **mesure** (KPI section 80) et se **rapporte** (rapport mensuel 1 page).
- Ne promets jamais un SLA que ton stock (105) et ton équipe ne peuvent pas tenir.
- Pénalités : à manier avec prudence — propose plutôt des **avoirs** en cas de non-respect, c'est plus sain commercialement.

## 179. Reporting client : la page mensuelle qui fidélise

```
RAPPORT MENSUEL - [Client - Site] - [Mois]
Disponibilite : 99,7 % (1 coupure WAN operateur, 22 min, hors notre perimetre)
Incidents : 2 (AP chambres debranche par le menage -> rebranche ; PSK invites tourne)
Interventions : 1 sur site, 3 a distance
Firmwares : a jour (versions : ...)
Alertes traitees : 5 (detail en annexe)
Recommandations : ajouter 1 AP aile ouest (densite en hausse), prevoir remplacement
                  onduleur (batteries 3 ans)
Prochain audit : [date]
```
Un client qui lit ça chaque mois **ne te quitte pas** — et il accepte tes recommandations parce qu'elles sont écrites et chiffrées.

## 180. Gérer un parc multi-clients : l'organisation du prestataire

- **1 tenant par client** (ou tenant prestataire bien cloisonné — section 32) : jamais de mélange.
- **Nommage global** : `[CLIENT]-[SITE]-[EQUIPEMENT]` partout (cloud, fiches, coffre, tickets) — quand tu gères 200 sites, la rigueur de nommage est ta mémoire externe.
- **Modèles de config** par vertical (boutique, hôtel, bureau) : 80 % commun, 20 % spécifique — c'est ça, l'industrialisation.
- **Calendrier partagé** : MAJ planifiées, audits, renouvellements de contrats — un oubli de MAJ sur 30 sites, c'est 30 sites vulnérables.
- **Facturation** : aligne la facturation maintenance sur les audits (le rapport mensuel justifie la facture).

## 181. Documentation d'exploitation : le dossier client type

```
DOSSIER CLIENT - [Client]
  01_Contrat/ (contrat, SLA, PV de recette)
  02_Sites/ (par site : fiche site, plan, photos, sauvegardes)
  03_Acces/ (references au coffre, registre des acces)
  04_Incidents/ (tickets, rapports mensuels)
  05_Maintenance/ (audits, MAJ, RMA)
```
5 dossiers, toujours les mêmes, pour tous les clients. Un nouveau technicien doit pouvoir prendre un dossier et intervenir **sans t'appeler** — c'est le test.

## 182. Passation entre techniciens (et le jour où tu pars en vacances)

- Aucune intervention ne doit dépendre d'**une seule personne** : tout est dans le dossier client (181) + le coffre (102) + ce guide.
- Passation écrite : sites sensibles du moment, MAJ en cours, incidents ouverts, « ne pas toucher à ».
- Teste une fois par an : « si je suis injoignable 1 semaine, qui fait quoi ? » — si la réponse est « personne », corrige.
- C'est aussi ta **valorisation** : une entreprise dont le savoir est documenté vaut plus qu'une entreprise « dans la tête du chef ».

## 183. Fin de contrat : le transfert propre

1. **Préavis** : comme écrit au contrat (jamais de coupure brutale).
2. **Transfert des accès** : tenant cloud au client ou à son nouveau prestataire, avec inventaire écrit (comptes, rôles, ce qui est transféré).
3. **Exports** : configurations, plans, fiches sites, historique des incidents — le client a payé pour, c'est à lui.
4. **Retrait de tes accès** : le jour du transfert, pas 6 mois après (ni 6 mois avant).
5. **Clôture** : PV de transfert signé, factures soldées, retour des équipements de secours.
Un départ propre = un client qui te **recommande** même en partant. Un départ sale = une réputation durablement abîmée.

## 184. Relation distributeur : ton allié n°1

- Ton **Gold Partner eKit** : stock, prix, RMA, firmwares, formations, escalade vers Huawei. C'est ton support niveau 1 — cultive la relation.
- À négocier : remises par volume, **stock de secours** dédié, délais RMA écrits, accès aux formations, un interlocuteur nommé (pas un ticket anonyme).
- Remonte les bugs firmware **avec des preuves** (logs, captures, procédure de reproduction) : un bug bien documenté est corrigé, un « ça marche pas » est ignoré.
- Demande les **roadmaps produits** : savoir 6 mois à l'avance qu'un modèle est remplacé t'évite de chiffrer du obsolète.

## 185. Extension de garantie et fin de vie : anticiper

- Note la **date d'achat + durée de garantie** de chaque équipement dans la fiche site — un RMA refusé pour « garantie expirée depuis 2 mois » alors que tu aurais pu l'étendre, c'est rageant.
- **Fin de vie / fin de support** d'un modèle : le distributeur l'annonce — prévois le renouvellement 12 mois avant (budget client).
- Règle de renouvellement : un switch/AR se garde **5-7 ans**, un AP **4-6 ans** (le Wi-Fi évolue vite : un AP Wi-Fi 6 en 2030 sera un frein). Planifie le refresh dans la roadmap client au lieu de le subir.

## 186. Annexe — modèle de fiche d'intervention

```
FICHE D'INTERVENTION n° __________
Date/heure : __________  Client/site : __________
Technicien : __________  Type : [ ] install [ ] depannage [ ] maintenance [ ] audit
Motif : __________________________________________
Constats : __________________________________________
Actions realisees : __________________________________________
Materiel remplace (SN ancien/nouveau) : __________
Config modifiee : [ ] oui (sauvegarde : __________) [ ] non
Tests effectues : __________________________________________
Temps passe : __________  Deplacement : __________
Reste a faire : __________________________________________
Signature client : __________  Signature technicien : __________
```
Une fiche par intervention, classée dans le dossier client (181). C'est ta preuve, ta mémoire et ta base de facturation.

## 187. Annexe — check-list de réception du câblage (avant de brancher l'actif)

- [ ] Chaque brin testé au testeur (continuité, paires, longueur < 90 m)
- [ ] Étiquettes aux deux extrémités, lisibles, conformes au plan
- [ ] Prises murales fixées, caches posés, pas de câble en tension
- [ ] Chemins de câbles fermés, pas de câble au sol dans les zones de passage
- [ ] Terre de la baie vérifiée (< 5 ohms visé)
- [ ] Distances PoE respectées (brins AP/caméras < 90 m)
- [ ] Photos du cheminement avant fermeture des faux plafonds (sinon c'est perdu)
- [ ] Plan de brassage à jour et affiché dans la baie
**Ne branche jamais l'actif sur un câblage non réceptionné** : 80 % des « pannes eKit » sont des pannes de câblage.

## 188. Annexe — lexique anglais-français (pour lire les docs constructeur)

| Anglais (docs) | Français | Note |
|---|---|---|
| Onboarding | Rattachement au cloud | Le geste n°1 |
| Uplink | Lien montant (vers le cœur/Internet) | — |
| Downlink | Lien descendant (vers les AP/clients) | — |
| Throughput | Débit utile | ≠ débit radio théorique |
| Forwarding rate (pps/Mpps) | Capacité de commutation en paquets/s | Le vrai muscle d'un switch |
| Switching capacity | Capacité de commutation (Gbit/s) | — |
| Egress / Ingress | Sortant / entrant | — |
| Power budget | Budget PoE total du switch | À ne jamais dépasser |
| Perpetual / Fast PoE | PoE maintenu au reboot / réalim. rapide | — |
| Heat dissipation | Dissipation thermique | Pour le dimensionnement clim |
| MTBF / MTTR | Temps moyen entre pannes / de réparation | Ordres de grandeur constructeur |
| Stacking | Empilement logique de switchs | Propriétaire : même marque |
| Zero-touch provisioning | Mise en service sans intervention | L'idéal eKit via scan SN |
| Captive portal | Portail captif | — |
| Site survey | Étude de couverture radio | À pied, avec mesures |
| RMA | Retour garantie | Via le distributeur |

## 189. Annexe — lire une fiche technique sans se faire piéger

- **« Jusqu'à »** = valeur labo, divise par 2 en conditions réelles pour ton dimensionnement mental.
- **Débit Wi-Fi agrégé** (ex. 1,775 Gbit/s) = somme théorique des bandes, jamais atteinte par un seul client.
- **Débit pare-feu brut** vs **débit tout activé** : seul le 2e compte (section 7).
- **« Recommandé pour X utilisateurs »** : c'est pour de la bureautique légère — divise par 2 si visio intensive.
- **Température de fonctionnement** : vérifie qu'elle colle au local (un switch à -5/+50 °C dans un local à 55 °C l'été = panne).
- **Garantie** : la fiche dit rarement la durée — demande au distributeur, par écrit.
- **« Cloud-managed »** : vérifie *ce qui* est gérable dans le cloud (tout ? juste le monitoring ?) — le diable est dans le détail, et dans la version du firmware.

## 190. Annexe — les 7 péchés capitaux du déploiement eKit

1. **L'orgueil** : « pas besoin de survey, je connais » → zones blanches.
2. **L'avarice** : 1 AP pour 300 m² « pour économiser » → tout le monde rame, on en rajoute 2 après (plus cher).
3. **La paresse** : pas de sauvegarde, pas de fiche site → 6 mois plus tard, on redécouvre tout.
4. **La gourmandise** : 5 SSID « au cas où » → airtime bouffé par les beacons.
5. **La colère** : reset usine en pleine journée « pour voir » → site coupé, client furieux.
6. **L'envie** : du Wi-Fi 7 partout « comme le concurrent » → budget explosé pour un besoin Wi-Fi 6.
7. **La luxure** : (on la remplace par) **l'approximation** : PoE « à peu près », adressage « on verra » → la dette technique se paie toujours, avec intérêts.

## 191. Ce qu'il faut surveiller en 2026-2027 (veille)

- **Disponibilité réelle** de l'AR281, des AP772/AP772E, du F700D dans ton pays (annoncés ≠ stockés).
- **Bande 6 GHz** : évolution réglementaire locale (impact direct sur l'AP673H).
- **Fonctions SNC** : le cloud s'enrichit vite (troubleshooting 2.0 aujourd'hui, quoi demain ?) — suis les release notes.
- **Conditions de licence** : le « gratuit sans licence » d'aujourd'hui — vérifier qu'il dure, surtout sur les fonctions IA/USG.
- **Wi-Fi 8 (802.11bn)** : en préparation dans l'industrie — ne fige pas un parc pour 10 ans sur du Wi-Fi 6 sans trajectoire.
- **Tes concurrents locaux** : que proposent-ils ? À quel prix ? (Le meilleur argumentaire se construit en connaissant l'adversaire.)
- **Ce guide** : relis-le une fois par an et mets à jour les sections marquées « à vérifier » — c'est ta dette documentaire.

## 192. Bibliographie commentée (d'où vient ce guide)

- **Fiches techniques constructeur** (e-file.huawei.com, distributeurs) : AR180/AR280, AP361/AP761/AP266/AP673H, S220/S310/S620, USG6000F-S — la source de vérité pour les specs.
- **Communiqués Huawei** (MWC 2025, Intelligent Office 2.0, e.huawei.com) : positionnement, nouveautés, chiffres de la gamme.
- **ekit.huawei.com** : portail officiel — à consulter avant chaque chiffrage.
- **support.huawei.com** : guides d'installation, release notes — la lecture obligatoire avant chaque MAJ.
- **Tes guides workspace** : `onduleurs_ups_guide.md` (énergie), guides supervision/réseau (NMS complémentaire), `proxmox_guide.md` (si NMS auto-hébergé).
- **La règle d'or** : une info de ce guide contredite par une fiche technique récente ? **La fiche a raison.** Signale-le et mets le guide à jour.

## 193. Historique des versions du guide

| Version | Date | Contenu |
|---|---|---|
| 1.0 | 2026-09-27 | Création : 195 sections, recherche web de cadrage (gamme vérifiée : AR180/Pro/280/281, AP361/761/266/572/673E/673H/772, S220/S310/S620, USG6000F-S, eKitOptix, app eKit, SNC). Sections marquées « à vérifier sur la documentation officielle » là où la source manquait. |

(Tiens ce tableau à jour à chaque mise à jour significative — un guide sans historique est un guide dont on ne sait plus ce qui est frais.)

## 194. Anti-sèche ultime — une page pour tout retenir

```
eKit = PME simple, app+cloud gratuits annonces, 70+ pays, depuis 2023.
AR : 180 (petit) / 180 Pro (8 AP) / 280 (16 AP, tete PME) / 281 (2026, 5-en-1).
AP : 361 (base Wi-Fi 6) / 761 (ext.) / 572-673E (Wi-Fi 7 dense) /
     673H iGuard (spycam, local only) / 772-772E (ext. Wi-Fi 7).
Switch : S220-8P4S (125 W) / S310-48P4S/4X (380 W, SFP/SFP+) / tout-optique J.
USG6000F-S : S125/150/200 = ~600/1000/2500 users (debit tout active !).
Methodes : Internet d'abord | modem->AR->switch->AP | scan SN avant plafond |
           3 SSID max | VLAN invites isole | PoE +30% | tester en marchant |
           sauvegarde jour J | PV signe | revoir cloud J+2/J+7.
Depannage : app d'abord | site rouge = amont | AP rouge = PoE/cable |
            cycle PoE a distance | une hypothese, un test.
Limites : pas de 802.1X pousse, pas de routage dyn., pas de redondance :
          -> Datacom (NCE/S600) quand ca devient serie (grille §83).
Or : documenter (fiche site, registre, photos), sauvegarder, auditer 1x/an,
     contrat de maintenance avec KPI, distributeur = allie.
```

## 195. Dernier mot

Un réseau eKit bien déployé, c'est 90 % de **méthode** et 10 % de matériel. Les AP et les switchs font ce qu'on leur demande — c'est la préparation (survey, adressage, PoE calculé), la rigueur (sauvegarde, fiche site, étiquetage) et le suivi (rituel cloud, audits, KPI) qui font la différence entre « le Wi-Fi marche » et « on ne pense jamais au Wi-Fi ». Ce guide t'a donné la méthode ; le terrain fera le reste. Bon déploiement, chef.

---

## 196. Procédure — migrer un SSID sans couper les utilisateurs

1. Crée le **nouveau SSID** à côté de l'ancien (ex. fictif : `BUREAU-New` à côté de `BUREAU`), même VLAN, nouveau PSK robuste.
2. Migre les utilisateurs **par vagues** (un service par jour) : ils se connectent au nouveau, tu vérifies.
3. Quand tout le monde est migré : **désactive** l'ancien SSID (ne le supprime pas tout de suite — 1 semaine de battement).
4. Surveille les alertes « client qui cherche l'ancien SSID » (il reste toujours un oublié).
5. Supprime l'ancien SSID, mets à jour la fiche site et le coffre.
**Jamais** de changement de PSK « à chaud » sur le SSID de production un lundi à 9 h : c'est 20 personnes qui t'appellent en même temps.

## 197. Procédure — remplacer une passerelle AR (panne ou upgrade)

1. **Sauvegarde** complète : config cloud du site + export local + fiche site (la config WAN/PPPoE est critique — identifiants opérateur dans le coffre).
2. Prépare la **nouvelle AR** : onboard sur le site **avant** de toucher à l'ancienne si possible (pré-staging).
3. Fenêtre de maintenance annoncée (coupure Internet du site pendant l'opération).
4. Permute : WAN (opérateur) → nouvelle AR → LAN vers switch. Vérifie : Internet, DHCP, DNS, VPN s'il y en a.
5. La nouvelle AR récupère la config du site via le cloud → **vérifie quand même** : SSID, VLAN, règles, portail.
6. Tests complets (check-list 74, version courte), puis RMA de l'ancienne (151).
7. Mets à jour fiche site (nouveau SN, nouveau firmware) et registre.

## 198. Procédure — ajouter un 2e accès Internet (secours)

- **Objectif** : si la fibre tombe, le site bascule sur un 2e lien (4G/5G ou 2e opérateur) — au minimum pour les fonctions critiques (caisses, TPE).
- **Prérequis** : une passerelle avec 2 ports WAN configurables (AR280 : ports LAN/WAN commutables ; vérifier le basculement automatique sur le modèle exact — **à vérifier sur la documentation officielle**).
- **Logique** : WAN1 = fibre (prioritaire), WAN2 = 4G (secours) ; bascule sur détection de panne (ping de supervision) ; retour automatique au rétablissement (avec hystérésis pour éviter le yoyo).
- **Pièges** : la 4G a un débit et un quota limités → en secours, **brider** les invités et le streaming ; tester la bascule **pour de vrai** (débrancher la fibre) 1×/trimestre ; noter la consommation data.
- **Alternative simple** : un routeur 4G indépendant branché en secours manuel (moins élégant, mais robuste et pas cher).

## 199. Procédure — durcir un site existant (sans tout reconstruire)

1. **Audit** (155) : état des lieux écrit, priorités.
2. Mots de passe : changer tous les défauts, PSK robustes uniques, rotation des invités.
3. VLAN : isoler les invités (souvent le point le plus critique et le plus rapide à corriger).
4. Ports switch inutilisés : **désactiver**. SSID temporaires : supprimer.
5. Firmwares : monter à N/N-1 (procédure 42).
6. SNMP : passer en v3, changer les strings. Telnet : désactiver si présent.
7. Sauvegarde + documentation : fiche site à jour, coffre à jour.
8. USG : si le client a des données sensibles et n'a qu'une AR, proposer l'ajout (argumentaire section 7).
Un durcissement se fait **en journée** (pas de coupure si bien mené), VLAN par VLAN, avec rollback possible à chaque étape.

## 200. Procédure — déménager un site (transfert de locaux)

1. **Avant** : sauvegarde complète, photos de la baie, inventaire SN, préavis opérateur (résiliation/transfert du lien).
2. **Pendant** : démonter en notant (quel AP allait où — les étiquettes servent ici), protéger les AP (cartons d'origine si gardés).
3. **Nouveau local** : refaire le **survey** (les murs ne sont pas les mêmes !), adapter le plan (nombre/position des AP), câblage neuf réceptionné (187).
4. **Remise en service** : ordre section 70, comme un nouveau site — parce que c'en est un.
5. **Après** : nouveau nom de site si besoin dans le cloud (ou nouveau site + transfert des équipements), fiche site neuve, PV de recette (160).
Un déménagement = un nouveau déploiement déguisé. Ne le chiffre jamais « comme une simple dépose/repose ».

## 201. Négocier avec le distributeur : les 8 leviers

1. **Volume annuel** : consolide tes achats chez un seul Gold Partner — la remise suit.
2. **Stock tampon** : négocie un dépôt-vente ou un stock dédié pour tes clients critiques.
3. **RMA** : délais écrits, procédure express pour les sites Gold (tes clients).
4. **Formation** : places gratuites aux sessions eKit (argument pour ton équipe, section 158).
5. **Démo** : kit de démo (AR + switch + 2 AP) pour tes avant-ventes — un client qui voit l'app en action signe plus vite.
6. **Co-marketing** : cas clients communs (le distributeur adore, et ça te fait de la pub).
7. **Roadmap** : être prévenu des fins de vie et nouveautés (184).
8. **Paiement** : délais alignés sur tes propres encaissements clients — le cash-flow tue plus de prestataires que la technique.

## 202. TCO 3 ans : le vrai coût d'un site (méthode)

```
TCO 3 ans = Matériel + Câblage/pose + Onduleur/élec + Main-d'œuvre config
          + 3 × Maintenance annuelle + 3 × (énergie + 4G secours éventuel)
          + 1 × Renouvellement partiel provisionné (ex. : batteries onduleur an 3)
Exemple de structure (ordres de grandeur, prix à vérifier) :
  Boutique type C : matériel 1×, pose 0,5×, maintenance 0,2×/an
  → TCO 3 ans ≈ 2,1× le prix du matériel seul.
  Hôtel 40 ch. : câblage souvent = 40-60 % du matériel en rénovation.
```
**Leçon** : le matériel n'est que la moitié du coût. Un devis qui oublie la maintenance et l'énergie est un devis qui fait perdre de l'argent — au client d'abord, à toi ensuite (retours SAV non facturés).

## 203. Les 5 erreurs de chiffrage qui coûtent cher

1. **Oublier le câblage** : en rénovation, c'est 30-60 % du budget — toujours une visite technique (159).
2. **Sous-dimensionner le PoE** : le 2e switch acheté en urgence coûte plus cher que le bon switch du départ.
3. **Oublier l'onduleur** : « on verra plus tard » = première coupure = premier incident non couvert.
4. **Ne pas chiffrer la maintenance** : 3 ans sans contrat = 3 ans d'interventions gratuites qui tuent ta marge.
5. **Chiffrer au prix catalogue sans remise** : ou l'inverse — figer un prix client avant d'avoir le prix distributeur. Toujours : prix fournisseur écrit → marge → prix client.

## 204. Appels d'offres : les 5 pièges

1. **Le cahier des charges copié d'un concurrent** : s'il exige du Cisco/Meraki nommément, tu ne gagneras pas avec eKit — sauf à faire valoir l'équivalence fonctionnelle (rarement gagné, ne perds pas 3 jours dessus).
2. **Les critères flous** : « Wi-Fi performant » ne veut rien dire — demande des critères mesurables (débit, couverture, utilisateurs) ou propose les tiens.
3. **Le moins-disant** : si le seul critère est le prix, quelqu'un proposera toujours moins cher — avec moins de service. Ne brade pas ta maintenance pour gagner : tu paieras pendant 3 ans.
4. **Les délais irréalistes** : « installation en 1 semaine » pour un hôtel 40 chambres sans câblage existant = impossible proprement. Écris ton planning réel ; un délai tenu vaut mieux qu'une promesse intenable.
5. **L'absence de visite technique** : un AO sans visite = un piège (murs en béton armé découverts le jour J). Si la visite est impossible, mets des **hypothèses écrites** et des prix conditionnels.

## 205. Planning type d'un déploiement (modèle)

```
Semaine -2 : visite technique, devis signé, commande matériel
Semaine -1 : réception matériel, pré-staging (onboarding au bureau), plan final
Jour J-1   : câblage réceptionné (187), baie prête, onduleur en place
Jour J     : AM : tête (modem->AR->switch), PM : AP + config + tests (70, 74)
Jour J+1   : tests utilisateurs, formation client, PV de recette (160)
J+2 / J+7  : inspection cloud, ajustements radio, clôture
```
Adapte les durées (×3 pour un hôtel), mais garde la **structure** : jamais de config avant le câblage réceptionné, jamais de PV sans tests utilisateurs.

## 206. Double WAN et VPN site-à-site : l'architecture qui tient

```
        [Site A - Siège]                    [Site B - Agence]
   [Fibre Op1]   [4G secours]          [Fibre] (mono-lien suffit souvent)
        \           /                        |
     [USG6000F-S125]  <-- IPsec VPN -->  [AR280]
           |                                  |
     [LAN A + VLAN]                     [LAN B + VLAN]
```
- Le VPN IPsec entre USG/AR eKit : paramètres IKE à aligner (phase 1/2, PSK, sous-réseaux) — **une ligne différente = tunnel qui ne monte pas** (cas n°14).
- Chaque site garde son **adressage propre** (jamais le même sous-réseau des deux côtés !).
- En secours 4G : le VPN doit **remonter** sur le lien de secours — à tester explicitement (le failover WAN sans failover VPN = agence isolée).
- Alternative : VPN **hub-and-spoke** vers le siège pour 5+ agences (plus simple à administrer que le maillé complet).

## 207. Supervision SNMP pratique : par où commencer

- Les switchs eKit exposent **SNMPv1/v2c/v3** (fiches S220/S310/S620) : active **v3** (auth + priv) avec un utilisateur dédié au monitoring.
- Dans ton NMS (Zabbix/PRTG/LibreNMS) : ajoute chaque switch/AR en SNMP, supervise au minimum : **disponibilité (ping), état des interfaces, erreurs/compteurs, charge CPU/mémoire** (selon les MIB exposées — **à vérifier sur la documentation officielle** par modèle, ne promets pas une métrique sans l'avoir vue).
- **Ne scrape pas** toutes les 30 secondes un parc de 50 sites : 5 min d'intervalle suffit en PME (le temps réel, c'est le cloud eKit qui le fait).
- Complémentarité : eKit = alertes temps réel + config ; NMS = historique + tendances + preuve SLA (section 78-79).

## 208. eKit et la GTB/domotique : cohabiter intelligemment

- Climatisation, contrôle d'accès, alarmes : de plus en plus **IP** — donc sur **ton** réseau.
- Règle : **un VLAN par famille** (GTB, contrôle d'accès, alarme), avec des règles inter-VLAN strictes (la clim n'a pas besoin de voir les PC).
- Les équipements GTB sont souvent **fragiles** réseau (vieilles piles IP, pas de DHCP fiable) : IP fixes, documentées, et **ne jamais** les mettre sur un VLAN avec portail captif.
- Coordination : le chauffagiste/alarmeur n'est pas un administrateur réseau — **tu** gères le VLAN, **il** gère son application. Écris-le.
- PoE : beaucoup d'équipements GTB sont PoE — intègre-les dans ton budget (68).

## 209. Wi-Fi invités multi-sites : centraliser sans se tromper

- **Modèle unique** de SSID invités (nom, portail, quotas) appliqué à tous les sites : l'expérience client est identique partout, et tu ne gères qu'un modèle.
- **Attention** : les plages IP peuvent être identiques entre sites (LAN indépendants — section 57), mais les **noms de sites** dans les alertes doivent être explicites (sinon « AP hors ligne » ne veut rien dire).
- Portail : si tu veux une page **par enseigne** avec le logo de chaque boutique, vérifie la personnalisation par site (**à vérifier sur la documentation officielle**).
- **Données** : les logs de connexion des invités sont des données personnelles — mentions légales + durée de conservation par site/pays (163).

## 210. Tester avant de promettre : la maquette d'atelier

- **Investissement** : 1 AR180, 1 S220-8P4S, 2 AP361 (ton kit de démo, section 201) — amorti à la première avant-vente gagnée.
- **Usages** : pré-staging (19), formation équipe (158), reproduction de bugs avant d'aller sur site, test des MAJ (42), démo client.
- **Rituel** : chaque nouvelle version firmware passe 1 semaine sur la maquette avant le site pilote.
- Une maquette qui tourne en permanence dans ton atelier, c'est aussi une **vitrine** : le client qui la voit comprend immédiatement ce qu'il achète.

## 211. Recruter et faire grandir un technicien réseau PME

- **Profil** : curiosité + rigueur > diplômes. Un technicien qui documente tout et teste méthodiquement vaut mieux qu'un « expert » qui improvise.
- **Parcours** : maquette (210) → binôme sur site (3-4 déploiements) → site simple seul (boutique) → multi-sites → astreinte.
- **Ce guide** = son manuel : fais-le lire (au moins les sections 16-30, 70-75, 91-100) avant le premier site seul.
- **Erreur à laisser faire** : un technicien qui n'a jamais planté un site ne sait pas dépanner. Laisse-le se tromper **sur la maquette**, pas chez le client — et débriefe chaque incident (176).

## 212. Sous-traitants : les cadrer pour garder la qualité

- **Ce que tu sous-traites** : câblage, fibre, électricité — **jamais** la configuration sans supervision (c'est ton savoir-faire et ta responsabilité).
- **Contrat écrit** : périmètre, délais, réception (187), garantie de la pose, pénalités de retard.
- **Réception systématique** : tu testes le câblage **avant** de payer le solde — sans exception.
- **Ne sous-traite jamais** à quelqu'un moins rigoureux que toi sur la documentation : un câbleur qui n'étiquette pas te coûte 3× son prix en dépannage futur.

## 213. Qualité : les 5 indicateurs de ton service

1. **Taux de sites « verts »** (cloud) : objectif > 98 % en heures ouvrées.
2. **Délai moyen de résolution** par criticité (vs SLA, section 178).
3. **Retours sur site** dans les 30 jours après installation : objectif < 5 % (un retour = un défaut de recette).
4. **Documentation** : 100 % des sites avec fiche à jour (audit trimestriel).
5. **Satisfaction client** : un appel ou un mini-questionnaire 1 mois après chaque déploiement — 5 minutes qui valent de l'or commercial.
Mesure-les, affiche-les en interne, améliore le plus mauvais chaque trimestre.

## 214. Litiges : les éviter, les gérer

- **Prévention** : devis détaillé signé, PV de recette (160), contrat de maintenance écrit, tout par écrit (les promesses orales n'existent pas).
- **En cas de litige** : dossier complet (181) + fiches d'intervention (186) + PV signé = ta défense. Sans écrits, c'est parole contre parole.
- **Négociation** : propose toujours une solution technique avant de parler argent (un AP ajouté gracieusement coûte moins cher qu'un procès et sauve la relation).
- **Ne jamais** : couper un service pour faire pression (illégal et suicidaire commercialement), ni insulter qui que ce soit par écrit.
- **Assurance** : RC pro à jour, avec une couverture adaptée à ton activité (un dégât des eaux causé par ta baie, ça arrive).

## 215. Cap sur l'autonomie : ta feuille de route 12 mois

- **Mois 1-2** : maquette montée (210), 1er site pilote eKit, ce guide lu par toute l'équipe.
- **Mois 3-4** : 3-5 sites en production, rituel cloud hebdo en place (43), modèles de config par vertical.
- **Mois 6** : 10+ sites, premier rapport mensuel client (179), stock de secours constitué (105), 1er audit annuel planifié.
- **Mois 9** : NMS complémentaire en place (79), SLA proposés (178), formation équipe terminée (158).
- **Mois 12** : revue annuelle — KPI (213), mise à jour de ce guide (sections « à vérifier » levées), roadmap produits avec le distributeur (184), objectif de croissance fixé.
Dans 12 mois, tu ne « poses plus du Wi-Fi » : tu exploites un parc. C'est là que la marge et la réputation se construisent.

---

## 216. Topologie type — site industriel léger / atelier de production

```
   [Internet Fibre]                [4G secours - routeur dedie]
          |                                   |
   +------+----------------+------------------+------+
   |              [USG6000F-S125]                    |
   |         (pare-feu + VPN siege)                 |
   +------+----------------+------------------+------+
          | (trunk, fibre si distance)
   +------+------+                                  (bureaux)
   | [S310-24P4X] |                         +-------+--------+
   |  (atelier)  |                         | [S220-8P4S]    |
   +--+--+--+---+                         |  (bureaux)   |
      |  |  |                             +--+--+---+----+
   [AP761] [Cameras] [Automates*]            |  |   |
   (quais,  (VLAN 40)  (VLAN 50,            [AP361] [PC]
    exterieur)          isoles)             bureaux

VLAN 10 BUREAUX / 20 INVITES / 40 CAMERAS / 50 AUTOMATES / 60 VOIX
* Automates : coordonner avec l'integrateur industriel (jamais touche sans lui).
Regles : poussiere = baie filtree ; vibrations = fixation renforcee ;
         coupures elec frequentes = onduleur + redemarrage auto ; ATEX = voir §144.
```

## 217. Tableau — ce que chaque usage consomme (pour dimensionner)

| Usage | Débit indicatif par flux (ordre de grandeur) | Sensible à |
|---|---|---|
| Navigation web / mail | 1-5 Mbit/s | Latence modérée |
| Visio HD (1 participant) | 2-4 Mbit/s | Latence + gigue (jitter) |
| Visio 4K / salle immersive | 15-25 Mbit/s | Latence + gigue |
| Streaming vidéo HD | 5-8 Mbit/s | Débit descendant |
| Streaming 4K | 20-30 Mbit/s | Débit descendant |
| VoIP (1 appel) | 0,1 Mbit/s | Latence + gigue (prioritaire !) |
| Sauvegarde réseau (1 poste) | Variable (pics) | Ne pas saturer l'uplink en journée |
| Caméra IP 1080p (1 flux) | 4-8 Mbit/s | Débit montant vers NVR |
| Caméra IP 4K (1 flux) | 12-20 Mbit/s | Débit montant vers NVR |
| Mise à jour OS (1 PC) | Pics à 50-100 Mbit/s | Planifier hors heures de pointe |

**Usage** : multiplie par le nombre de flux simultanés pour dimensionner l'uplink et les dorsales. Une salle de réunion 10 personnes en visio = ~30 Mbit/s réservés.

## 218. Glossaire complémentaire (20 termes)

- **Airtime** : temps d'antenne — la ressource radio partagée. Chaque SSID/client en consomme.
- **Beacon** : trame de signalisation émise par chaque SSID (~10/s) — d'où la règle des 3 SSID max.
- **DFS** : canaux 5 GHz partagés avec les radars — l'AP doit les libérer si radar détecté (micro-coupures possibles).
- **Tx Power** : puissance d'émission — baisser > augmenter en dense.
- **RSSl / SNR** : niveau de signal / rapport signal-bruit — le SNR fait la qualité, pas le nombre de barres.
- **Co-channel interference** : deux AP sur le même canal qui se gênent — d'où l'alternance 1/6/11.
- **Hidden node** : deux clients qui ne s'entendent pas mais brouillent l'AP — typique en entrepôt.
- **Jitter** : variation de la latence — tue la voix et la visio.
- **MOS** : note de qualité vocale (1-5) — vise > 4 en téléphonie IP.
- **LLDP** : protocole de découverte de voisinage — utile pour voir « qui est branché où ».
- **LACP (802.3ad)** : agrégation de liens — à vérifier par modèle côté eKit.
- **IGMP snooping** : optimisation multicast (TV IP) — section 173.
- **DHCP snooping** : protection contre les serveurs DHCP pirates — annoncé sur S220.
- **Sticky MAC** : mémorisation des MAC par port — sécurité filaire de base.
- **Port isolation** : isolation L2 entre ports — pour les ports invités filaires.
- **Rogue AP** : AP non autorisé (celui du voisin ou celui « branché pour dépanner ») — à détecter et neutraliser.
- **Heatmap** : carte de couverture radio — issue d'un survey, pas d'une promesse.
- **Backhaul** : lien de collecte (filaire idéalement) — un AP en mesh sans fil divise le débit.
- **Failover** : bascule automatique (WAN, VPN) — à tester pour de vrai, pas sur papier.
- **Baseline** : mesures de référence d'un site sain — speedtests, ping, charge — pour comparer quand « c'est lent ».

## 219. La valise du technicien eKit (check-list matériel)

- [ ] Smartphone chargé + batterie externe + câble
- [ ] PC portable + câble console (si le modèle a un port console) + adaptateur USB-série
- [ ] Testeur de câble + pince à sertir + connecteurs + étiquettes + marqueur
- [ ] Jarretières RJ45 (assortiment) + 2 jarretières fibre + injecteur PoE af/at
- [ ] Tournevis, pince coupante, colliers, scotch d'électricien, lampe frontale
- [ ] AP361 + S220 de secours (selon criticité du site)
- [ ] Multimètre (vérifier une prise avant de brancher la baie)
- [ ] Fiche site vierge + PV de recette vierge + ce guide (sur le téléphone)
- [ ] EPI : gants, lunettes (fibre), chaussures de sécurité (chantier/entrepôt)
- [ ] De l'eau et de la patience (les deux s'épuisent vite sur site)

## 220. Compte-rendu de visite technique avant-vente (modèle)

```
CR VISITE TECHNIQUE - [Client prospect] - Date : __________
Participants : __________
Locaux : surface ____ m2, etages ____, murs (placo/beton/verre) ____
Existant : cablage (etat) ____, equipements ____, operateur/debit ____
Besoins exprimes : ____ utilisateurs, Wi-Fi ____, cameras ____, tel IP ____,
                   invites ____, videosurveillance ____, multi-sites ____
Contraintes : budget ____, delai ____, securite ____, electrique ____
Photos : [jointes]   Plan : [croquis joint]
Hypotheses de chiffrage : __________________________________________
Prochaine etape : devis sous ____ jours / 2e visite ____
```
Un CR écrit après chaque visite = un devis juste + une preuve en cas de « vous n'aviez pas vu que… ».

## 221. Erreurs classiques n°26 à 30 (fin de la série)

**N°26 — Le firmware « dernier cri » installé sans lire la note.** Symptôme : régression surprise. Solution : la note de version se lit **avant**, pas après (42). Le « latest » n'est pas toujours le « greatest » en production.
**N°27 — Le test « ça marche sur mon téléphone ».** Symptôme : le client se plaint alors que « ça marchait ». Cause : testé sur 1 seul terminal, près de l'AP. Solution : tester sur 2-3 terminaux différents, aux endroits où les utilisateurs travaillent vraiment (74).
**N°28 — L'AP oublié dans le cloud après déménagement.** Symptôme : alertes fantômes, inventaire faux. Cause : équipement déplacé physiquement mais pas dans l'app. Solution : tout mouvement physique = mouvement dans le cloud le jour même (32).
**N°29 — Le mot de passe du coffre perdu.** Symptôme : personne ne peut administrer. Cause : 1 seul détenteur. Solution : 2 détenteurs + procédure de récupération écrite et scellée (102). Teste la récupération 1×/an.
**N°30 — Le « petit site » sans documentation.** Symptôme : 2 ans plus tard, personne ne sait ce qui est installé. Cause : « c'était petit, pas besoin de fiche ». Solution : **il n'y a pas de petit site** — fiche site (65) même pour 1 AP. Le temps « gagné » se paie ×10 au premier dépannage.

## 222. Mini-lexique fibre (pour parler MiniFTTO sans rougir)

- **OLT** : tête optique (côté baie) — elle pilote les terminaux.
- **ONT/ONU** : terminal optique (côté chambre/bureau) — ici intégré au F700D.
- **Splitter** : diviseur optique passif (1:16, 1:32…) — pas d'alimentation, pas de panne électronique.
- **dB / budget optique** : chaque splitter et soudure « mange » du signal — le budget total doit rester dans les limites de l'OLT.
- **Soudure (fusion)** : épissure des fibres — nécessite soudeuse + technicien formé (sous-traite si besoin, 212).
- **Réflectométrie (OTDR)** : mesure qui localise les défauts sur la fibre — à exiger à la réception d'un câblage fibre.
- **Pigtail / jarretière** : brin de raccordement — ne jamais le pincer ni le courber à angle vif (rayon de courbure !).
En MiniFTTO, la fibre est simple **quand elle est bien posée** — et un cauchemar quand elle est pincée dans une porte. Protège les cheminements.

## 223. Les logiques de l'app à connaître par cœur (sans les nommer)

L'app évolue, mais les logiques restent. Retiens les **principes**, pas les libellés exacts (qui changent à chaque version) :
1. Tout part du **site** : pas de site = pas d'équipement gérable.
2. L'**onboarding** précède la **configuration** : un équipement non onboardé ne reçoit rien.
3. La config se pense en **modèle** (VLAN, SSID) puis s'**applique** aux équipements — pas l'inverse.
4. Les **alertes** sont rattachées aux équipements **et** aux sites : lis toujours les deux niveaux.
5. Une **mise à jour** est une opération à part, jamais mélangée à un changement de config.
6. Ce que tu ne trouves pas dans l'app se fait (si possible) en **local** — l'app n'est pas le seul chemin.
Quand l'app change d'interface après une MAJ : respire, les 6 principes n'ont pas bougé — cherche où ils se cachent dans la nouvelle interface.

## 224. Tableau — qui fait quoi dans ton équipe (RACI simplifié)

| Tâche | Chef de service (toi) | Technicien senior | Technicien junior | Client |
|---|---|---|---|---|
| Avant-vente / devis | **R**esponsable | Consulté | — | Informé |
| Déploiement site simple | Approuve | **R** | Exécute | Informé |
| Déploiement site complexe | **R** | Exécute | Assiste | Informé |
| Astreinte niveau 1 | Escalade | **R** (rotation) | Assiste | Appelle |
| MAJ firmware | Valide | **R** | — | Informé |
| Audit annuel | **R** | Exécute | Assiste | Reçoit rapport |
| Accès cloud (création/retrait) | **R** | Demande | — | — |
| Facturation / litige | **R** | — | — | — |

**R** = responsable final. Une seule personne « R » par tâche — sinon personne ne l'est vraiment.

## 225. Naviguer dans ce guide : la table de décision rapide

```
JE DOIS...                                    -> VA VOIR...
Choisir du materiel pour un devis             -> §4-8, fiches §116-130, budgets §148
Convaincre un client                          -> §13, §87, FAQ §164, CR visite §220
Preparer un deploiement                       -> §15 (check-list), §64-69, §70 (ordre)
Onboarder des equipements                     -> §19-21, pense-bete §107
Configurer VLAN/SSID/PoE                      -> §64-69, procedures §152-154
Tester et recetter                            -> §74, PV §160, cablage §187
Depanner                                      -> §91 (methode), §92-100, §156, §221, §108
Superviser au quotidien                        -> §43 (rituel), §76-80
Securiser                                     -> §102-104, audit §155, durcir §199
Faire evoluer / migrer                        -> §61-63 (interop), §83-86 (Datacom)
Vendre de la maintenance                      -> §101, §178-180, §202
Former un technicien                          -> §158, maquette §210, valise §219
Gerder le cap sur 12 mois                     -> §215
```
**Fin du guide — 225 sections. Bon déploiement, chef.**

---

## 226. Ton premier client eKit : la check-list de zéro à facturé

- [ ] Maquette montée et maîtrisée (210) — tu as onboardé 3 équipements les yeux fermés
- [ ] Distributeur Gold identifié, tarifs et délais RMA connus par écrit (184, 201)
- [ ] Visite technique faite + CR écrit (220)
- [ ] Devis signé avec hypothèses écrites (159, 203)
- [ ] Matériel reçu, pré-staging au bureau (19)
- [ ] Câblage réceptionné (187), baie et onduleur prêts
- [ ] Déploiement selon l'ordre (70) + check-list (74)
- [ ] PV de recette signé (160), fiche site remise (65)
- [ ] Contrat de maintenance proposé (101, 178) — même s'il dit non, propose
- [ ] Inspection cloud à J+2/J+7 faite, cas notés, guide annoté
Le premier client est le plus dur et le plus précieux : c'est lui qui fera tes 5 suivants par recommandation.

## 227. Les 5 phrases à bannir devant un client

1. « C'est bizarre, ça marchait chez moi. » → « Je reproduis le problème et je reviens vers vous avec un diagnostic. »
2. « C'est la faute de l'opérateur / du matériel. » → « J'ai identifié la cause, voici ce qu'on fait. » (même si c'est l'opérateur, c'est toi le responsable du dossier)
3. « Il faut tout changer. » → « Voici ce qu'on garde, voici ce qu'on remplace, et pourquoi. »
4. « Je ne sais pas. » (sec) → « Je ne sais pas encore, je vérifie et je vous réponds aujourd'hui. » (puis le faire vraiment)
5. « C'est compliqué à expliquer. » → explique simplement (si tu ne peux pas l'expliquer simplement, c'est que tu ne l'as pas compris — retourne à la maquette)
La confiance se gagne en disant la vérité vite, pas en ayant toujours raison.

## 228. Modèle de message post-installation (J+7)

```
Bonjour [Nom],
Une semaine après la mise en service de votre réseau, petit point :
- Supervision : tout est au vert depuis le [date], aucune alerte critique.
- Ce que nous avons ajusté : [ex. : puissance radio de l'AP accueil].
- Rappel : votre interlocuteur reste [nom] au [téléphone] (astreinte : [plages]).
- En pièce jointe : la fiche de votre installation + le PV signé.
- Prochaine étape : [audit trimestriel / proposition de contrat de maintenance].
Bonne journée,
[Ton nom] – [Ton service]
```
Un message comme ça, 7 jours après, vaut 10 campagnes de pub : le client se sent suivi, pas abandonné.

## 229. Signaler un bug ou demander une fonction : les bons canaux

1. **Distributeur Gold** : premier niveau — avec logs, captures, SN, versions, procédure de reproduction (un bug sans reproduction est un bug qui ne sera pas corrigé).
2. **Support Huawei** (via le distributeur ou support.huawei.com) : pour les bugs confirmés et les questions de compatibilité.
3. **Forums e.huawei.com** : pour les retours d'expérience et les cas tordus déjà rencontrés par d'autres intégrateurs.
4. **En interne** : note le bug dans le dossier du site + une entrée « contournement » dans ce guide — en attendant le correctif, c'est ton contournement qui fait tourner le site.
Ne laisse jamais un bug « en l'air » : soit il est signalé avec preuves, soit il est contourné et documenté.

## 230. Crédits, avertissement final et mot de la fin (bis)

- **Sources** : fiches techniques constructeur (AR180/AR280, AP361/AP761/AP266/AP673H, S220/S310/S620, USG6000F-S), communiqués Huawei (MWC 2025, Intelligent Office 2.0), portail ekit.huawei.com — vérifiées par recherche web en septembre 2026.
- **Avertissement** : la gamme eKit évolue vite ; toute section marquée « à vérifier sur la documentation officielle » est une action à faire avant chiffrage/déploiement, pas une clause de style.
- **Exemples** : adresses IP, noms de SSID, mots de passe et prix de ce guide sont **fictifs ou indicatifs** — ne les déploie jamais tels quels.
- **Responsabilité** : ce guide est un aide-mémoire de terrain, pas un substitut aux documentations constructeur ni aux normes locales. En cas de contradiction, la doc officielle et la réglementation gagnent toujours.
- Voilà, chef : 230 sections, de la visite technique à la fin de contrat, en passant par la panne de 3 h du matin. Le reste, c'est le terrain qui l'écrira — avec tes annotations en marge.
