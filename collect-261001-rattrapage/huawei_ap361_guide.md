# Huawei eKit AP361 — Guide ultra-complet

**Point d'accès Wi-Fi 6 d'intérieur — Gamme eKit PME**
**Rédigé pour Zelef, chef de service Systèmes & Énergies — ton terrain, direct, sans blabla.**
**Version du guide : 2026-09-27**

> ⚠️ **Avertissement de méthode** : les caractéristiques marquées ✅ sont tirées de la
> fiche produit officielle Huawei eKit (datasheet AP361) et de fiches distributeurs
> recoupées en septembre 2026. Les points marqués 🔎 **« à vérifier sur la fiche du
> modèle exact »** n'ont pas pu être confirmés par une source officielle accessible :
> vérifiez-les sur l'étiquette du produit, le *Hardware Installation and Maintenance
> Guide* WLAN Huawei, ou ekit.huawei.com avant de les appliquer en production.
> Les commandes CLI sont syntaxiquement plausibles pour VRP Huawei ; la syntaxe exacte
> dépend de la version logicielle — l'app eKit / le cloud eKit restent le chemin
> officiel de configuration. Tous les mots de passe et clés du guide sont **fictifs**.

---

## Sommaire

- **A. Fiche produit** — sections 1 à 10
- **B. Montage mécanique** — sections 11 à 17
- **C. Alimentation PoE** — sections 18 à 25
- **D. Première mise en route** — sections 26 à 32
- **E. Gestion : eKit app, cloud, Fat/Fit/Cloud** — sections 33 à 44
- **F. Configuration radio** — sections 45 à 58
- **G. SSID et sécurité d'accès** — sections 59 à 72
- **H. VLAN** — sections 73 à 80
- **I. Itinérance (roaming)** — sections 81 à 88
- **J. Sécurité Wi-Fi (WIDS, rogue, isolation)** — sections 89 à 96
- **K. QoS / WMM** — sections 97 à 103
- **L. Supervision** — sections 104 à 111
- **M. Sauvegarde, restauration, firmware** — sections 112 à 119
- **N. Dépannage terrain (16 cas)** — sections 120 à 137
- **O. Maintenance préventive** — sections 138 à 143
- **P. Durcissement** — sections 144 à 151
- **Q. Pense-bête de poche** — sections 152 à 156
- **R. Glossaire** — section 157
- **S. Quiz (10 questions + réponses)** — sections 158 à 159
- **T. Pour aller plus loin** — section 160

---

# A. FICHE PRODUIT

## 1. L'AP361, c'est quoi exactement

Le **Huawei eKitEngine AP361** est un point d'accès Wi-Fi 6 (802.11ax) d'intérieur,
positionné sur le segment **PME / SOHO** via la gamme **Huawei eKit** (anciennement
« eKitEngine »). C'est l'entrée de gamme « plafond » de la famille eKit : petit,
léger, alimenté en PoE, pensé pour être déployé en nombre sans contrôleur physique
dédié.

Cas d'usage typiques cités par le constructeur ✅ :

| Environnement | Pourquoi l'AP361 colle |
|---|---|
| Bureaux PME / SOHO | Densité moyenne, 1 AP pour 15-25 utilisateurs |
| Petits/moyens hôpitaux | Couverture chambres/couloirs, coût maîtrisé |
| Commerces, retail | SSID invité + SSID caisse/terminaux |
| Hôtellerie économique | 1 AP pour 4-8 chambres selon cloisonnement |
| Écoles primaires/secondaires | Salles de classe, faible densité simultanée |

Ce n'est **pas** un AP de très haute densité (pas de 4x4, pas de 6 GHz) : pour des
amphithéâtres, halls de gare ou stades, viser la gamme AirEngine (ex. AP5760) ou
l'équivalent eKit haut de gamme.

## 2. Caractéristiques hardware vérifiées (constructeur)

Tableau de synthèse — valeurs recoupées entre la datasheet officielle et plusieurs
fiches distributeurs (septembre 2026) :

| Caractéristique | Valeur ✅ | Source |
|---|---|---|
| Norme Wi-Fi | 802.11ax (Wi-Fi 6), rétrocompatible a/b/g/n/ac | Datasheet |
| Bande 2,4 GHz | 2x2 MIMO, jusqu'à **575 Mbit/s** | Datasheet |
| Bande 5 GHz | 2x2 MIMO, jusqu'à **1 200 Mbit/s** | Datasheet |
| Débit agrégé max | **1,775 Gbit/s** | Datasheet |
| Antennes | Intégrées « smart antenna » (commutation intelligente) | Datasheet |
| Gain d'antenne | **5 dBi** (valeur distributeur, à confirmer) | 🔎 fiche distributeur |
| Ports Ethernet | **1 x GE RJ45** (10/100/1000BASE-T) | Datasheet |
| Alimentation | **PoE 802.3af** (rétrocompatible 802.3at) | Datasheet + fiches |
| Consommation max | **8,8 W** (datasheet) ; certains revendeurs annoncent 9,4 W | Datasheet |
| Dimensions | **Ø 180 mm x 35 mm** (hauteur) | Fiches distributeurs |
| Poids | **≈ 0,45 kg** | Fiches distributeurs |
| Montage | Plafond et mur | Datasheet |
| Température de fonctionnement | **-10 °C à +50 °C** (fiche Digitec/Galaxus) ; un revendeur annonce 0 à +40 °C | 🔎 à vérifier selon révision |
| Humidité | 5 % à 95 % HR, sans condensation | Fiche revendeur |
| Altitude | jusqu'à 5 000 m | Fiche revendeur |
| Modes de fonctionnement | **Fit, Fat, Cloud** | Datasheet |
| Sécurité Wi-Fi | WPA/WPA2/WPA3 (PSK et EAP selon fiches) | Fiches distributeurs |
| Gestion | App mobile **HUAWEI eKit**, cloud eKit, Web | Datasheet + fiches |
| Référence fabricant | **50086871** | Fiches distributeurs |

Technologies radio annoncées ✅ : MU-MIMO (montant et descendant, 2,4 + 5 GHz),
OFDMA, 1024-QAM (+25 % d'efficacité vs 256-QAM du Wi-Fi 5), beamforming,
BSS Coloring (réduction des interférences inter-BSS).

## 3. Ce qui est incertain — à vérifier sur la fiche du modèle exact

Soyons honnête, voici ce que la datasheet publique **ne détaille pas** clairement :

- 🔎 **Port console** : la datasheet ne mentionne qu'**1 port GE**. La présence d'un
  port console (RJ45 ou micro-USB) n'est pas confirmée pour l'AP361. Si vous
  prévoyez une procédure de secours par console, vérifiez physiquement l'appareil.
- 🔎 **Bouton reset** : très probable (trou « Reset » comme sur les autres AP
  Huawei), mais non documenté dans l'extrait de datasheet consulté.
- 🔎 **Comportement exact des LED** : les codes couleur/clignotement (boot, upgrade,
  défaut) sont décrits dans le *Hardware Installation and Maintenance Guide* WLAN
  (chapitre AP361) — pas dans la datasheet commerciale.
- 🔎 **Température de fonctionnement** : -10/+50 °C (Digitec, Galaxus) vs 0/+40 °C
  (revendeur Maroc). Écart probable entre révision hardware ou marge commerciale :
  retenez **0 à +40 °C** comme plage sûre en intérieur climatisé, et vérifiez
  l'étiquette.
- 🔎 **Version logicielle** : l'AP361 tourne sous le logiciel système eKit
  (dérivé VRP). Le numéro exact (ex. V200Rxxx) évolue — vérifiez dans l'app eKit
  ou sur ekit.huawei.com au moment du déploiement.
- 🔎 **PoE OUT / passthrough** : non mentionné pour l'AP361 (contrairement à
  certains AP muraux « wall plate »). Considérez qu'il n'y en a pas.
- 🔎 **USB / IoT / BLE** : non mentionnés → considérez comme absents.

> 💡 **Réflexe terrain** : avant d'acheter en volume, demandez au fournisseur la
> *datasheet PDF officielle* du lot (elle porte la révision hardware) et le
> *Hardware Installation and Maintenance Guide* correspondant. C'est 10 minutes qui
> évitent les surprises de chantier.

## 4. Contenu du carton (typique, à vérifier à réception)

D'après les pratiques Huawei eKit (🔎 à vérifier à l'ouverture, ça varie selon les lots) :

- [ ] L'AP361 lui-même (film de protection sur le dôme)
- [ ] Kit de fixation plafond (platine + vis/chevilles)
- [ ] Kit de fixation murale (souvent la même platine)
- [ ] Guide de démarrage rapide (QR code vers l'app eKit)
- [ ] Étiquette avec **numéro de série (SN)** et adresse **MAC** — photographiez-la,
      c'est votre identifiant d'onboarding cloud

**Non fourni** (à commander à part) : injecteur PoE, câble Ethernet, switch PoE.
L'AP361 n'a **pas** d'adaptateur secteur fourni : il est conçu pour être alimenté
exclusivement en PoE (802.3af).

## 5. Ports, LED, boutons — lecture terrain

### 5.1. Le port GE (unique)

- **1 x RJ45 10/100/1000BASE-T**, en dessous / à l'arrière du boîtier selon le sens
  de montage.
- C'est à la fois le **port de données** (uplink vers le switch) et le **port
  d'alimentation PoE-in**.
- Vitesse négociée affichée dans l'app eKit : visez **1000 Mbit/s full duplex**.
  Si vous voyez 100 Mbit/s → câble ou port du switch en cause (voir dépannage §124).

### 5.2. LED d'état

L'AP361 dispose d'une LED d'état (souvent sur la tranche ou près du port).
Signification **typique** des AP Huawei eKit (🔎 codes exacts à vérifier dans le
guide hardware du modèle) :

| État LED (typique) | Signification probable |
|---|---|
| Éteinte | Pas d'alimentation PoE → vérifier injecteur/switch/câble |
| Verte fixe | Fonctionnement normal, associé au cloud / en service |
| Verte clignotante lente | Démarrage en cours, ou en cours d'association |
| Rouge / orange | Défaut : échec boot, pas d'IP, pas de cloud, firmware corrompu |
| Clignotement rapide | Mise à jour firmware en cours → **ne pas couper l'alimentation** |

> ⚠️ Ne vous fiez pas à ma table par cœur : photographiez la page « LED » du guide
> hardware officiel et gardez-la dans le dossier du site. En dépannage nocturne,
> c'est elle qui fait foi.

### 5.3. Bouton Reset

- Trou « Reset » accessible avec un trombone (🔎 à vérifier physiquement).
- Procédure standard Huawei : AP sous tension → appuyer **> 5 secondes** →
  relâcher → l'AP redémarre en configuration d'usine.
- **Effet** : efface SSID, comptes, adresse cloud. Ne touche pas (en général) au
  firmware actif. À réserver aux reprises en main (voir §26).

## 6. Dimensions, poids, discrétion

- **Ø 180 mm, 35 mm d'épaisseur, ~450 g** ✅ (fiches distributeurs).
- Format « galet » blanc, discret au plafond. En zone patrimoniale ou architecte
  exigeant, prévoir une peinture adaptée (🔎 vérifier que la peinture n'est pas
  métallisée — elle atténue le signal — et qu'elle ne fait pas sauter la garantie).
- 450 g : une fixation sur dalle de faux plafond standard tient sans renfort
  particulier, mais **toujours** visser la platine dans l'ossature ou utiliser des
  chevilles adaptées au support (voir §11-14).

## 7. Environnement d'exploitation

| Paramètre | Valeur de référence | Commentaire terrain |
|---|---|---|
| Température | -10 à +50 °C (fiche) / 0 à +40 °C (revendeur) | En intérieur climatisé : aucun souci. Éviter les combles non ventilés, les cuisines pro, les locaux techniques surchauffés |
| Humidité | 5-95 % HR sans condensation | Pas de montage en zone humide (piscine, buanderie) sans caisson adapté |
| Poussière | Usage intérieur propre | En atelier poussiéreux, prévoir nettoyage trimestriel des ouïes |
| CEM | Éviter à moins de 30 cm des câbles de puissance | Les variateurs, ballasts et moteurs sont les pires voisins |
| Altitude | jusqu'à 5 000 m | Sans objet en métropole/Afrique de l'Ouest |

> 🔥 **Point chaud** : un AP collé au plafond d'un local non climatisé sous
> toiture tôle peut dépasser 50 °C l'après-midi. Mesurez avant de valider
> l'emplacement définitif (thermomètre IR à 20 €, ça paie).

## 8. Antennes intelligentes : ce que ça change (et ce que ça ne change pas)

L'AP361 embarque des antennes internes dites « smart » : l'AP commute
électroniquement entre plusieurs diagrammes de rayonnement pour suivre les
clients. Concrètement :

- ✅ Ça **améliore la stabilité** quand les utilisateurs bougent (couloirs, open space).
- ✅ Ça **ne remplace pas** une étude de couverture : 2x2 MIMO à 5 dBi reste un
  petit AP. Comptez un AP pour 80-150 m² en open space, beaucoup moins derrière
  des murs béton.
- ✅ Le gain de 5 dBi est **omnidirectionnel** : le signal part dans toutes les
  directions, y compris vers l'étage du dessus/dessous. En immeuble, baissez la
  puissance plutôt que de « arroser » les voisins (voir §49).

## 9. Gamme eKit : où se situe l'AP361

La gamme **Huawei eKit** vise les PME avec une promesse : *zéro contrôleur à
acheter, gestion cloud depuis un smartphone*. L'AP361 est le modèle d'intérieur
« standard ». Au-dessus, on trouve des modèles 4x4 ou extérieurs ; en dessous,
des AP muraux (wall plate) pour l'hôtellerie.

**Conséquence pratique** : tout l'écosystème (app mobile, portail cloud,
licences) est pensé « simple ». Si vous venez du monde AirEngine + AC (contrôleur
WLAN), vous allez trouver ça dépouillé — c'est normal, c'est le positionnement.
Pour un parc PME de 5 à 50 AP, c'est un avantage. Au-delà, évaluez la gamme
entreprise classique.

## 10. Documents officiels à garder sous la main

Checklist documentaire du chef de service (à constituer **avant** le chantier) :

- [ ] Datasheet officielle AP361 (PDF, ekit.huawei.com)
- [ ] *WLAN Hardware Installation and Maintenance Guide* — chapitre AP361
      (montage, LED, reset, specs environnementales détaillées)
- [ ] *Configuration Guide* eKit correspondant à votre version logicielle
- [ ] Release notes du firmware que vous déployez
- [ ] Plan de masse du site avec emplacements AP numérotés (AP-01, AP-02…)
- [ ] Tableau d'adressage IP / VLAN du site

> 💡 Stockez tout ça dans le dossier du site sur votre partage d'équipe. Dans
> 2 ans, quand un AP tombera un dimanche, vous bénirez cette discipline.

---
---

# B. MONTAGE MÉCANIQUE

## 11. Le kit de fixation : inventaire avant de monter à l'échelle

Avant de grimper, ouvrez **un** carton et vérifiez le kit (🔎 contenu exact selon lot) :

- Platine de fixation (avec ergots baïonnette ou clips)
- Vis + chevilles (souvent prévues pour béton/placo — **à adapter au support réel**)
- Gabarit de perçage (parfois imprimé sur le carton)
- Éventuellement : collier ou adaptateur pour rail de faux plafond

**Règle d'or** : ne commencez jamais un chantier de 20 AP sans avoir validé le
montage sur **1 AP témoin**. Tous les faux plafonds ne se ressemblent pas.

## 12. Montage au plafond — dalle de faux plafond (cas le plus courant)

Procédure type (adaptez au kit réel) :

1. **Repérez l'ossature** : la platine doit être fixée sur le profilé métallique
   (T-bar), pas seulement sur la dalle minérale qui s'effrite.
2. **Faites passer le câble** : RJ45 caté6a ou cat6, avec **30 cm de mou** en
   réserve au-dessus de la dalle (pour décrocher l'AP sans arracher le câble).
3. **Vissez la platine** sur l'ossature (2 vis minimum).
4. **Clipsez l'AP** sur la platine (quart de tour ou clips selon modèle) jusqu'au
   « clic ».
5. **Branchez le RJ45** (clic du connecteur), vérifiez que la LED s'allume.
6. **Étiquetez** : AP-01, AP-02… au marqueur sur la platine + dans l'app eKit.

> ⚠️ **Sécurité** : un AP de 450 g qui tombe de 3 m sur un collaborateur, c'est un
> accident du travail. Serrage vérifié + test de traction à la main (tirez
> franchement : ça ne doit pas bouger).

## 13. Montage au plafond dur (béton, hourdis)

1. Percez au gabarit (Ø selon chevilles, typiquement 6 mm).
2. Chevilles adaptées : **béton** → chevilles nylon/frappe ; **hourdis creux** →
   chevilles à expansion adaptées (pas de simple cheville nylon dans du creux !).
3. Vissez la platine, clipsez l'AP, câblez avec un chemin de câble ou une goulotte
   jusqu'au point de raccordement.
4. Si le câble court en apparent : goulotte blanche 20x12, propre et pro.

## 14. Montage mural

- L'AP361 se monte aussi au mur (✅ datasheet). Orientez le dôme **vers la zone à
  couvrir** : le rayonnement est optimisé pour un montage plafond (vers le bas) ;
  au mur, la couverture devient asymétrique.
- Hauteur conseillée : **2,5 à 4 m**. En dessous, vandalisme et masquage par le
  mobilier ; au-dessus, le signal s'étale et la puissance utile baisse.
- Évitez le montage mural **derrière** une armoire métallique, un tableau
  électrique ou un pilier béton : c'est le meilleur moyen de créer une zone d'ombre.

## 15. Positionnement radio : les règles qui paient

| Règle | Pourquoi |
|---|---|
| Un AP par zone de vie, pas par « surface théorique » | Les murs béton tuent le 5 GHz ; comptez les cloisons, pas les m² |
| Évitez les extrémités de couloir | Mieux vaut centrer sur la zone d'usage |
| Distance entre AP : 10-20 m en open space | Pour un roaming propre et un réemploi de canaux |
| Jamais dans un placard / local technique fermé | Cage de Faraday + surchauffe |
| Jamais à côté (< 1 m) d'un four micro-ondes | Le 2,4 GHz se fait hacher (voir §121) |
| Hauteur homogène sur un même plateau | Sinon le roaming devient imprévisible |
| Évitez l'aplomb des détecteurs incendie/sprinklers | Contrainte réglementaire + maintenance |

**Test terrain** : après montage, faites le tour avec votre téléphone (analyseur
Wi-Fi type « WiFi Analyzer ») : visez **-60 dBm ou mieux** en 5 GHz dans les zones
de travail, **-67 dBm minimum** pour la voix. En dessous de -70 dBm : ajoutez ou
déplacez un AP.

## 16. Précautions CEM et thermiques

- **Distance aux câbles de puissance** : 30 cm minimum en parcours parallèle
  (norme usuelle de séparation courants forts/faibles). Croisement à 90° si
  inévitable.
- **Ne pas poser l'AP sur** : un ballast, un variateur de lumière, un moteur, un
  onduleur. Les harmoniques et le champ magnétique perturbent l'électronique.
- **Ventilation** : ne pas enfermer l'AP dans un caisson étanche sans
  déclassement thermique. Si caisson obligatoire (extérieur abrité, atelier),
  mesurez la température interne en charge.
- **Foudre** : en intérieur c'est le switch et le câblage qui prennent ; prévoyez
  parafoudre sur l'arrivée électrique de la baie et, en zone orageuse, des
  protections PoE/Ethernet sur les liens exposés.

## 17. Checklist de fin de montage (à cocher par AP)

- [ ] Platine fixée sur support solide (test de traction OK)
- [ ] AP clipse, ne bouge pas
- [ ] RJ45 branché (clic), 30 cm de mou au-dessus de la dalle
- [ ] LED allumée (couleur notée : _______)
- [ ] Étiquette AP-XX posée sur la platine
- [ ] Photo de l'emplacement (pour le DOE — dossier des ouvrages exécutés)
- [ ] Emplacement reporté sur le plan de masse
- [ ] Câble testé au testeur (continuité + plan de câblage)

---
---

# C. ALIMENTATION POE

## 18. Ce que dit le constructeur : 802.3af, 8,8 W

✅ Datasheet : alimentation **PoE 802.3af**, consommation **8,8 W** (certains
revendeurs annoncent 9,4 W en pointe — retenez **10 W** pour vos calculs, marge
incluse).

Rappel des standards PoE :

| Standard | Nom courant | Puissance max par port (PSE) | Puissance utile (PD) | Tension |
|---|---|---|---|---|
| 802.3af | PoE | 15,4 W | 12,95 W | 44-57 V |
| 802.3at | PoE+ | 30 W | 25,5 W | 50-57 V |
| 802.3bt type 3 | PoE++ / 4PPoE | 60 W | 51 W | 50-57 V |
| 802.3bt type 4 | PoE++ | 90/100 W | 71,3 W | 52-57 V |

L'AP361 étant **802.3af**, un switch **PoE (15,4 W/port) suffit** : pas besoin de
PoE+ pour l'alimenter. MAIS (voir §21) prévoyez large si le switch doit aussi
alimenter des caméras ou des AP plus gourmands demain.

## 19. Calcul du budget PoE — la méthode

**Formule** : `Budget à prévoir = Σ (consommation max de chaque PD) × 1,2`
(20 % de marge pour les pertes câble et les pics).

**Exemple : 12 x AP361 sur un switch 24 ports PoE**

| Poste | Calcul | Résultat |
|---|---|---|
| 1 AP361 | 8,8 W → arrondi | 10 W |
| 12 AP361 | 12 × 10 W | 120 W |
| Marge 20 % | 120 × 1,2 | **144 W** |
| Budget switch à choisir | ≥ 144 W | **≥ 150 W** (palier commercial courant : 185 W, 250 W, 370 W) |

> 💡 **Piège classique** : le « budget PoE total » du switch n'est pas le nombre
> de ports × 15,4 W. Un switch « 24 ports PoE » avec 185 W de budget ne peut pas
> alimenter 24 équipements à 15,4 W (ça ferait 370 W). Lisez **toujours** la ligne
> « PoE power budget » de la fiche du switch.

**Tableau de dimensionnement rapide (AP361 seuls, marge 20 % incluse)** :

| Nb d'AP361 | Budget à prévoir | Palier switch conseillé |
|---|---|---|
| 4 | 48 W | 60-130 W |
| 8 | 96 W | 130-185 W |
| 12 | 144 W | 185 W |
| 16 | 192 W | 250 W |
| 24 | 288 W | 370 W |

## 20. Distances : jusqu'où peut aller le câble ?

- **Limite Ethernet : 100 m** (90 m de câble fixe + 10 m de jarretières). Au-delà :
  pas de PoE fiable, pas de gigabit garanti.
- **Chute de tension** : en 802.3af à 10 W sur 100 m de Cat5e, la perte est de
  l'ordre de 1 à 2 W — acceptable. En câble bas de gamme (CCA — aluminium cuivré),
  la résistance est plus forte : **exigez du 100 % cuivre**.
- **Règle terrain** : si la distance dépasse 80 m, tirez du **Cat6A** (meilleure
  section, moins de pertes) et mesurez la tension PoE à l'arrivée si vous avez un
  testeur PoE (recommandé : ~200 €, amorti au premier dépannage).

| Distance | Câble conseillé | Remarque |
|---|---|---|
| < 50 m | Cat6 100 % cuivre | Cas standard |
| 50-80 m | Cat6 100 % cuivre | OK sans souci |
| 80-100 m | Cat6A 100 % cuivre | Marge de sécurité |
| > 100 m | **Interdit en cuivre** | Ajouter un switch intermédiaire ou passer en fibre + convertisseur PoE local |

## 21. Switch PoE vs injecteur : comment choisir

| Critère | Switch PoE manageable | Injecteur PoE individuel |
|---|---|---|
| Nb d'AP | ≥ 4 : le switch gagne | 1-3 AP isolés |
| Supervision | Oui (SNMP, état par port, reset PoE à distance) | Non (sauf modèles manageables, rares) |
| Redémarrage à distance | Oui (cycle PoE du port) | Non (il faut aller sur place) |
| Coût | Mutualisé, dégressif | ~20-40 €/pièce, mais N alimentations = N points de panne |
| Secours électrique | Centralisé sur l'onduleur de la baie | Dispersé, souvent oublié |
| Recommandation | ✅ **Choix par défaut** | Dépannage, site distant, ajout ponctuel |

**Choix du switch** (exemples de gammes, pas de pub : vérifiez les fiches) :
un switch manageable **24 ports GE avec budget PoE ≥ 185 W** couvre 12 AP361 +
caméras. Vérifiez : budget PoE, ventilateurs (bruit en open space !), garantie,
et compatibilité 802.3af (tous les switchs PoE+ / bt sont rétrocompatibles af).

**Choix de l'injecteur** : **802.3af minimum, 15,4 W**, 44-57 V. Un injecteur
802.3at (30 W) convient aussi (rétrocompatible). Évitez les injecteurs « passifs »
24 V non normalisés (type vieux Ubiquiti passif) : **incompatibles et dangereux**
pour l'AP361.

## 22. Câblage : les 5 règles non négociables

1. **100 % cuivre, pas de CCA** (Copper Clad Aluminum). Le CCA a ~60 % de
   résistance en plus → échauffement + chute de tension PoE. Exigez la mention
   sur le touret.
2. **Cat6 minimum** pour du neuf (Cat6A si > 80 m ou environnement perturbé).
3. **Pas de rallonge RJ45 bricolée** : un seul tenant de la baie à l'AP.
   Les raccords intermédiaires sont la cause n°1 des « PoE qui ne monte pas ».
4. **Rayon de courbure** : ≥ 4x le diamètre du câble. Un câble plié à angle droit
   derrière la dalle = paire fragilisée = 100 Mbit/s au lieu du gigabit.
5. **Testez chaque lien** au testeur (wiremap + longueur). Un lien non testé est un
   lien en panne qui s'ignore.

## 23. Sécurisation électrique de la baie PoE

Vous êtes chef de service Systèmes & Énergies : c'est votre terrain.

- **Onduleur** : le switch PoE **doit** être sur onduleur. 12 AP361 = ~120 W :
  c'est rien pour un onduleur de baie, mais si le switch n'est pas ondulé, tout
  le Wi-Fi tombe à la première microcoupure (et les AP mettent 2-3 min à revenir).
- **Dimensionnement onduleur** : ajoutez la charge PoE réelle (pas le budget max)
  + switch lui-même (~30-50 W) + marge. Voir votre guide onduleurs.
- **Parafoudre** : type 2 en tête de l'alimentation de la baie ; en zone
  tropicale/orages fréquents, envisagez des parafoudres Ethernet sur les liens
  les plus exposés.
- **Disjoncteur dédié** : un départ dédié pour la baie informatique, étiqueté,
  avec consignation possible. Évitez le départ « prises du couloir ».

## 24. Mise en garde : le PoE qui « ne monte pas »

Symptômes : LED éteinte, ou LED allumée puis l'AP reboote en boucle.

| Cause | Diagnostic | Solution |
|---|---|---|
| Budget PoE du switch dépassé | Voyant PoE du switch en défaut, log « power denied » | Délester ou changer de switch |
| Câble trop long / CCA | Testeur : longueur > 90 m, ou résistance anormale | Refaire le lien en cuivre |
| Paire abîmée | Négociation à 100 Mbit/s ou rien | Refaire les connecteurs, retester |
| Injecteur sous-dimensionné | Injecteur 24 V passif ou < 15 W | Injecteur 802.3af 15,4 W |
| Port du switch en défaut | Tester l'AP sur un autre port | Reset du port / RMA switch |
| AP défectueux | Même symptôme sur 2 alimentations différentes | RMA |

> 🔁 **Astuce** : un reboot en boucle à froid le matin puis stable l'après-midi =
> souvent un câble limite (résistance qui varie avec la température) ou un budget
> PoE tout juste. Ne laissez pas traîner.

## 25. Checklist PoE de mise en service

- [ ] Budget PoE calculé et documenté (tableau §19 rempli pour le site)
- [ ] Switch : budget ≥ besoin + 20 %, firmware à jour
- [ ] Chaque lien testé (wiremap + longueur < 90 m câble fixe)
- [ ] Négociation **1000 Mbit/s full** vérifiée sur chaque port (dans l'app eKit ou le switch)
- [ ] Switch sur onduleur, parafoudre en tête
- [ ] Étiquetage : chaque câble repéré aux deux extrémités (AP-01 ↔ port switch 5)
- [ ] Test de coupure : débrancher/rebrancher un port PoE → l'AP doit revenir seul en < 5 min

---
---

# D. PREMIÈRE MISE EN ROUTE

## 26. Reset d'usine : quand et comment

**Quand** : AP d'occasion, AP déjà configuré pour un autre site, onboarding cloud
qui échoue mystérieusement, reprise en main après départ d'un prestataire.

**Comment** (procédure standard Huawei, 🔎 libellé exact du bouton à vérifier) :

1. AP sous tension (PoE branché, LED allumée).
2. Trombone dans le trou **Reset**, appuyer **> 5 secondes** (jusqu'à changement
   d'état de la LED).
3. Relâcher. L'AP redémarre (2 à 4 minutes).
4. L'AP est en configuration d'usine : prêt pour l'onboarding.

**Ce que le reset efface** : SSID, comptes admin, adresse du cloud, certificats.
**Ce qu'il n'efface pas** (en général) : le firmware. Si l'AP a été « briquée »
par un upgrade interrompu, le reset ne suffit pas → voir §118.

## 27. Onboarding par l'app eKit (chemin officiel recommandé)

C'est **le** chemin prévu par Huawei pour l'AP361. Procédure type :

1. Installez **HUAWEI eKit** (App Store / Play Store), créez un compte Huawei.
2. Créez votre **site** (nom du client / bâtiment) dans l'app.
3. **Scannez le QR code / code-barres** de l'AP (sur le carton ou sous l'appareil)
   → l'AP est ajouté au site.
4. Branchez l'AP en PoE sur le réseau (il doit obtenir une IP par DHCP et accéder
   à Internet pour joindre le cloud eKit).
5. Dans l'app : l'AP apparaît « en ligne » → assistant de configuration :
   SSID, mot de passe, bande, etc.
6. Répétez pour chaque AP, puis vérifiez la topologie dans l'app.

**Prérequis réseau pour l'onboarding cloud** (à préparer **avant** le jour J) :

- [ ] DHCP actif sur le VLAN d'accueil des AP (ou IP statique prévue)
- [ ] Accès Internet sortant (l'AP doit joindre le cloud eKit : **ne pas**
      bloquer les flux HTTPS sortants vers les domaines Huawei)
- [ ] DNS fonctionnel
- [ ] Heure correcte (NTP) — un décalage > quelques minutes peut faire échouer
      l'établissement TLS vers le cloud

## 28. Interface web locale (mode autonome/Fat)

Si l'AP est en mode autonome (Fat) ou pour un accès local de secours :

1. Branchez un PC sur le même réseau que l'AP (ou directement sur son port via
   un switch/injecteur).
2. Trouvez son IP : via le DHCP du site (cherchez l'entrée correspondant à la
   MAC de l'étiquette) ou via un scan réseau.
3. `https://<IP-AP>` — identifiants par défaut : 🔎 **à vérifier sur le guide de
   démarrage du lot** (les valeurs par défaut changent selon les versions ; les
   firmwares récents **forcent** la création d'un mot de passe à la première
   connexion — ne cherchez pas un « admin/admin » universel).
4. Première action : **changer le mot de passe** (voir §144).

> ⚠️ Si la page web ne répond pas : vérifiez que vous êtes bien sur le même
> sous-réseau, que le firewall du PC ne bloque pas, et essayez en HTTP si le
> firmware est ancien (puis basculez en HTTPS — voir §146).

## 29. Accès SSH/CLI : état des lieux honnête

- Sur les AP Huawei récents gérés par eKit, l'accès SSH/Telnet **peut être
  désactivé par défaut** ou réservé au mode Fat. 🔎 Vérifiez dans la documentation
  de votre version logicielle.
- Si disponible : activez SSH (jamais Telnet en production), restreignez-le au
  VLAN de management (voir §145), et utilisez des clés ou des mots de passe
  robustes (voir §144).
- Les exemples CLI des sections suivantes sont donnés pour le mode autonome /
  pour comprendre la logique VRP ; en mode cloud eKit, **l'app fait foi** et
  certaines commandes peuvent ne pas exister.

## 30. Mise à niveau logicielle initiale (upgrade « jour 1 »)

**Règle** : ne laissez jamais un AP en production sur le firmware d'usine.

1. Dans l'app eKit / le cloud : vérifiez la version actuelle de chaque AP.
2. Consultez les **release notes** de la version cible (correctifs de sécurité,
   compatibilité WPA3, stabilité).
3. Planifiez l'upgrade **hors heures ouvrées** (un upgrade = 5 à 15 min/AP avec
   reboot).
4. Upgrader **un AP pilote** d'abord, validez 24 h (SSID, roaming, débit), puis
   généralisez par vagues (jamais 100 % du parc d'un coup).
5. Vérifiez que tous les AP sont sur la **même version** à la fin (un parc
   hétérogène = diagnostics infernaux).

> ⛔ **Interdit** : couper le PoE pendant l'upgrade. C'est le meilleur moyen de
> corrompre la flash (voir §118).

## 31. Vérifications post-boot (les 5 minutes qui évitent les retours)

Pour chaque AP, après mise en service :

- [ ] LED verte fixe (état normal)
- [ ] Visible « en ligne » dans l'app eKit / le cloud
- [ ] IP correcte (bon VLAN, bon sous-réseau)
- [ ] Port négocié à **1000 Mbit/s**
- [ ] SSID diffusés visibles depuis un téléphone (les 2 bandes si activées)
- [ ] Association test : un client se connecte, obtient une IP, ping la passerelle
- [ ] Débit test : speedtest local ou iperf vers un serveur du LAN (notez la valeur)
- [ ] Version firmware = version cible du site

## 32. Erreurs classiques de la mise en route

| Erreur | Conséquence | Prévention |
|---|---|---|
| Onboarding sans DHCP | L'AP ne joint pas le cloud, reste « hors ligne » | Préparer le DHCP **avant** le chantier |
| Firewall qui bloque le cloud | AP « en ligne » puis « hors ligne » en boucle | Ouvrir HTTPS sortant vers Huawei eKit dès la maquette |
| Tous les AP upgradés d'un coup | Panne généralisée si la version a un bug | Vague pilote + généralisation |
| Pas d'étiquetage SN/MAC | Impossible de retrouver quel AP est où | Photo de l'étiquette + plan numéroté |
| Mot de passe par défaut conservé | Porte ouverte (voir §144) | Changement systématique jour 1 |

---
---

# E. GESTION : APP EKIT, CLOUD, FAT / FIT / CLOUD

## 33. Les trois modes de fonctionnement : Fit, Fat, Cloud

✅ La datasheet annonce trois modes. Voici ce que ça veut dire concrètement :

| Mode | Pilotage | Idéal pour | Limites |
|---|---|---|---|
| **Cloud** | Portail/app **eKit** (cloud Huawei) | PME, multi-sites, 1 à ~50 AP | Dépend d'Internet pour la gestion (le Wi-Fi local continue si le cloud tombe — à vérifier selon version) |
| **Fat** (autonome) | Web/CLI local sur chaque AP | 1-3 AP, site isolé sans Internet, maquette | Gestion AP par AP, fastidieux au-delà de 5 |
| **Fit** | Contrôleur WLAN (AC) Huawei | Parcs entreprise classiques | Nécessite un AC (coût, complexité) — hors positionnement eKit |

> 💡 **En pratique eKit** : 95 % des déploiements AP361 se font en mode **Cloud**.
> Le mode Fat sert de secours / maquette. Le mode Fit ne se justifie que si vous
> avez déjà un AC Huawei dans l'entreprise.

## 34. Mode Cloud eKit : quand l'utiliser (et quand ne pas)

**Utilisez le mode Cloud quand** :

- Le site a un accès Internet fiable (même modeste : la gestion consomme peu).
- Vous gérez plusieurs sites (le cloud donne une vue unifiée).
- L'équipe sur place n'est pas spécialiste réseau (l'app guide pas à pas).
- Vous voulez superviser à distance (alertes, état des AP, statistiques).

**Évitez-le (ou prévoyez un plan B) quand** :

- Site sans Internet ou avec Internet instable (carrière, zone rurale) → mode Fat.
- Contrainte réglementaire interdisant la gestion cloud hors UE/zone (données de
  gestion hébergées chez Huawei — vérifiez votre politique de conformité).
- Besoin de fonctionnalités avancées type WIDS poussé, NAC 802.1X complexe :
  vérifiez que le cloud eKit les expose pour l'AP361 (🔎 selon version).

## 35. Mode Fat/autonome : quand l'utiliser

- **Maquette** avant déploiement : 1 AP sur un bureau, on teste SSID/VLAN/roaming.
- **Site isolé** : pas d'Internet, pas de cloud → configuration locale web/CLI.
- **Secours** : si le cloud est injoignable durablement, basculer temporairement
  en Fat pour garder la main localement (procédure à tester **avant** d'en avoir
  besoin).

**Inconvénient majeur** : chaque AP se configure séparément. À 10 AP, c'est déjà
une corvée et une source d'incohérences. Documentez chaque AP (fichier de config
sauvegardé, voir §112).

## 36. Mode Fit : quand l'utiliser

- Vous avez **déjà** un contrôleur WLAN Huawei (AC) qui gère un parc AirEngine.
- L'AP361 peut alors rejoindre le parc existant comme AP léger (léger = la
  config vient de l'AC).
- 🔎 Vérifiez la **compatibilité de version** AC ↔ AP361 avant tout achat :
  un AP trop récent pour un AC trop vieux ne s'enregistrera pas.

## 37. App eKit : onboarding pas à pas (procédure détaillée)

Prérequis : compte Huawei, app eKit à jour, Bluetooth/GPS activés si l'app les
demande pour la découverte locale.

1. **Créer l'organisation / le site** : `eKit > Sites > +` → nom explicite
   (`ClientX-Siege-Etage2`), adresse, fuseau horaire.
2. **Ajouter les équipements** : scannez le QR code de chaque AP361
   (ou saisissez le SN manuellement). Vérifiez que le SN scanné correspond à
   l'étiquette physique — une erreur de saisie = un AP « fantôme ».
3. **Brancher** : PoE + lien vers le switch, DHCP actif (voir §27).
4. **Attendre l'état « En ligne »** (2 à 5 min après boot).
5. **Assistant SSID** : créez le SSID principal (WPA2/WPA3, clé robuste fictive
   type `Bureau-2026-Exemple!Changez-Moi`), choisissez les bandes.
6. **Appliquer au site** : la config est poussée vers tous les AP du site.
7. **Vérifier** : topologie dans l'app, chaque AP vert, test d'association (§31).

**Conseil d'équipe** : faites l'onboarding **à deux** — un qui scanne/étiquette
en haut de l'échelle, un qui valide dans l'app en bas. Ça divise les erreurs par deux.

## 38. Portail cloud eKit : structurer vos sites

Le portail web eKit (ekit.huawei.com) donne la vue d'ensemble :

- **Organisation** → **Sites** → **Équipements** : nommez de façon hiérarchique
  (`SIEGE/ETAGE2/AP-03-ZoneOpenSpace`).
- **Modèles de configuration** : créez un template « SSID Bureau standard » et
  appliquez-le aux nouveaux sites plutôt que de tout refaire à la main.
- **Comptes** : un compte admin par technicien (pas de compte partagé
  « admin » — voir §144), avec journal des actions si disponible.

## 39. Rôles et comptes : qui peut faire quoi

| Rôle (typique) | Droits | Pour qui |
|---|---|---|
| Super admin / Owner | Tout, y compris suppression du site | Vous (chef de service) |
| Admin réseau | Config SSID/radio, upgrades | Techniciens réseau |
| Lecture seule | Supervision, pas de modif | Astreinte, direction |
| Installateur | Onboarding (ajout d'AP) sans config | Prestataire de pose |

> ⚠️ **Retirez les accès des prestataires** à la fin du chantier. Un compte
> installateur oublié, c'est une porte dérobée.

## 40. Topologie dans eKit : lire la carte

L'app/le cloud affichent la topologie (AP ↔ switch ↔ Internet). Apprenez à la lire :

- AP **vert** : en ligne, OK.
- AP **gris** : hors ligne → vérifier PoE/réseau (voir §124).
- AP **orange/rouge** : alarme → cliquer pour le détail (forte utilisation CPU,
  interférences, clients en échec d'association).
- Lien **dégradé** : négocié à 100 Mbit/s → câble à refaire.

Faites une **capture d'écran de la topologie saine** le jour de la mise en
service : c'est votre référence « avant » pour comparer en cas de panne.

## 41. Mise en service multi-sites : méthode

1. **Site pilote** (1 site, 2-3 AP) : validez le template de config complet
   (SSID, VLAN, sécurité, radio).
2. **Documentez le template** : exportez/sauvegardez la config du site pilote.
3. **Dupliquez** vers les autres sites via le cloud (même SSID, mêmes VLAN si
   l'adressage est homogène).
4. **Adaptez localement** : canaux radio (l'environnement diffère), puissance,
   nom du site.
5. **Vague d'upgrade firmware** synchronisée sur tous les sites (même version
   partout — voir §30).

## 42. Sauvegarde de la configuration cloud

- Le cloud eKit conserve la configuration du site : c'est déjà une forme de
  sauvegarde. Mais **ne comptez pas que sur lui**.
- Exportez périodiquement la configuration (si la fonction existe dans votre
  version — 🔎) ou documentez-la dans un fichier versionné (voir §112).
- Notez les **versions firmware** par site dans un tableau de suivi.

## 43. Quand le cloud est injoignable : plan B

Scénario : Internet coupé, vous devez modifier un SSID **maintenant**.

1. Accès local : l'AP en mode Cloud garde-t-il une interface web locale ?
   🔎 **À tester sur votre version** : sur certains firmwares, l'accès local
   reste possible ; sur d'autres, tout passe par le cloud.
2. Si pas d'accès local : bascule temporaire en mode Fat (reset + config locale),
   puis ré-onboarding cloud quand Internet revient.
3. **Testez cette procédure une fois par an** en exercice (comme un exercice
   incendie). Le jour où vous en aurez besoin, il sera trop tard pour découvrir
   qu'elle ne marche pas.

## 44. Checklist de gestion

- [ ] Organisation/sites nommés de façon hiérarchique et homogène
- [ ] Un compte par technicien, pas de compte partagé
- [ ] Accès prestataires révoqués en fin de chantier
- [ ] Template de configuration documenté et versionné
- [ ] Capture « topologie saine » archivée (référence)
- [ ] Procédure « cloud injoignable » testée une fois

---
---

# F. CONFIGURATION RADIO

## 45. Les deux bandes : à quoi sert chacune

| Bande | Points forts | Points faibles | Usage conseillé |
|---|---|---|---|
| **2,4 GHz** | Porte loin, traverse (un peu) les murs | 3 canaux non chevauchants seulement, encombrée (Bluetooth, micro-ondes, voisins) | IoT, anciens terminaux, couverture de secours |
| **5 GHz** | Beaucoup de canaux, débits élevés, moins d'interférences | Portée plus courte, traverse mal le béton | Usage principal : bureautique, voix, vidéo |

**Règle d'or** : poussez les clients capables vers le **5 GHz** (band steering,
§52). Le 2,4 GHz est une ressource rare : ne la gaspillez pas.

## 46. Plan de canaux 2,4 GHz : 1, 6, 11 — et rien d'autre

En 2,4 GHz, seuls **3 canaux ne se chevauchent pas** en largeur 20 MHz : **1, 6, 11**
(en Europe ; le 13 existe mais chevauche partiellement — à éviter en planifié).

```
Canaux 2,4 GHz (20 MHz) :

 1        6        11      <- utilisez UNIQUEMENT ceux-ci
 |####|   |####|   |####|
 1  2  3  4  5  6  7  8  9  10 11 12 13
         ^^^^^^^^^^^^^^^^
         chevauchement = interférences
```

- **Largeur 20 MHz uniquement** en 2,4 GHz. Le 40 MHz en 2,4 GHz, c'est occuper
  2/3 de la bande tout seul : égoïste et contre-productif dès qu'il y a des voisins.
- En planifié : alternez 1/6/11 entre AP adjacents (motif en nid d'abeille).
- **Ne laissez pas « Auto » sans contrôle** : vérifiez après 48 h les canaux
  réellement choisis (voir §50).

## 47. Plan de canaux 5 GHz : le terrain de jeu

En Europe (ETSI), canaux 20 MHz utilisables : **36, 40, 44, 48** (UNII-1, sans DFS),
**52-64** (UNII-2, DFS), **100-140** (UNII-2e, DFS), **149-165** restreints selon pays.

| Groupe | Canaux 20 MHz | DFS (radar) | Usage |
|---|---|---|---|
| UNII-1 | 36, 40, 44, 48 | Non | ✅ Premier choix : pas de DFS, mise en service immédiate |
| UNII-2 | 52, 56, 60, 64 | Oui | OK si pas de radar à proximité (aéroports, météo, militaires) |
| UNII-2e | 100-140 | Oui | OK, plus de canaux pour les grands déploiements |
| UNII-3 | 149+ | Selon pays | 🔎 Vérifiez la réglementation locale |

**DFS** : si l'AP détecte un radar, il doit **changer de canal** (coupure de
quelques dizaines de secondes à 10 min). Près d'un aéroport ou d'une station
météo : **restez sur 36-48** pour éviter les bascules intempestives.

**Plan 80 MHz** (canaux « larges ») : 42 (36-48), 58 (52-64), 106, 122, 138, 155.
**Plan 40 MHz** : paires 36-40, 44-48, 52-56, etc.

## 48. Largeur de canal : 20, 40 ou 80 MHz — que choisir

| Largeur | Débit max théorique (2x2, 5 GHz) | Quand l'utiliser |
|---|---|---|
| 20 MHz | ~287 Mbit/s (ax) | 2,4 GHz (toujours) ; 5 GHz en zone très dense / beaucoup d'AP |
| 40 MHz | ~574 Mbit/s (ax) | 5 GHz : bon compromis densité/débit |
| 80 MHz | ~1201 Mbit/s (ax) | 5 GHz : peu d'AP voisins, besoin de débit brut |

**Recommandation terrain** :

- **2,4 GHz : 20 MHz, toujours.**
- **5 GHz : 40 MHz par défaut** en PME (bon équilibre). Passez en **80 MHz**
  uniquement si : peu d'AP (≤ 4), pas de voisins Wi-Fi agressifs, et besoin réel
  de > 400 Mbit/s par client (rare en bureautique).
- Le 80 MHz « mange » 4 canaux : en immeuble de bureaux, vous allez vous battre
  avec les voisins. Le 40 MHz est le choix adulte.

## 49. Puissance d'émission : moins, c'est souvent mieux

Réflexe débutant : « mettre la puissance à fond pour mieux couvrir ». **Erreur.**
Trop de puissance = :

- L'AP arrose l'étage du dessus et les voisins (interférences).
- Les clients « entendent » l'AP de loin mais l'AP n'entend pas leurs réponses
  faibles → **liaison asymétrique**, paquets perdus.
- Les clients restent « collés » à un AP lointain au lieu de roamer (sticky client).

| Contexte | Puissance 5 GHz conseillée | Puissance 2,4 GHz conseillée |
|---|---|---|
| Open space dense (AP tous les 15 m) | 11-14 dBm | 11-14 dBm |
| Bureaux cloisonnés | 14-17 dBm | 14-17 dBm |
| Entrepôt / grand volume | 17-20 dBm | 17-20 dBm |
| Maximum réglementaire (ETSI) | 23 dBm (200 mW) en 5 GHz UNII-1… selon bande | 20 dBm (100 mW) en 2,4 GHz |

> 💡 **Méthode** : commencez à **14 dBm** sur les deux bandes, mesurez (-60 dBm
> cible en zone de travail), ajustez par pas de 3 dB. Notez chaque changement.

🔎 Les plages exactes réglables sur l'AP361 (pas de 1 dB ? max ?) dépendent du
firmware : vérifiez dans l'app eKit.

## 50. DCA (sélection dynamique de canal) : l'auto qui se surveille

Le DCA choisit automatiquement le meilleur canal selon les interférences mesurées.
C'est utile, mais :

- **Vérifiez le résultat** : après 48 h, contrôlez les canaux attribués. Un DCA
  mal réglé met deux AP voisins sur le même canal.
- **Figez si c'est bon** : une fois le plan validé, passez en manuel pour éviter
  les changements intempestifs (un changement de canal = microcoupure pour les
  clients).
- **Planifiez les scans** : si le DCA est actif, limitez les scans aux heures
  creuses (un scan = l'AP quitte son canal quelques ms).

## 51. TPC (contrôle de puissance) : l'auto, bis

Même logique que le DCA : l'AP ajuste sa puissance selon la couverture mesurée.
En PME, **préférez le réglage manuel** (§49) : vous avez 5-15 AP, pas 500.
Le TPC auto a du sens dans les très grands parcs homogènes.

## 52. Band steering : pousser vers le 5 GHz

Le band steering incite les clients dual-band à s'associer en **5 GHz** plutôt
qu'en 2,4 GHz (en retardant/ignorant leurs sondes en 2,4 GHz).

- ✅ Activez-le sur le SSID bureautique : ça désengorge le 2,4 GHz.
- ⚠️ Certains objets connectés **anciens** (imprimantes, IoT) ne supportent que
  le 2,4 GHz et supportent mal le steering agressif → prévoyez un **SSID IoT
  dédié en 2,4 GHz uniquement** (voir §60).
- Si un client « ne voit pas » le SSID : premier suspect = band steering trop
  agressif + client 2,4 GHz only (voir §122).

## 53. Airtime fairness : contre les clients boulets

Un vieux client en 802.11b à 1 Mbit/s peut **monopoliser le temps d'antenne**
et plomber tout le monde. L'airtime fairness donne à chaque client un temps
d'émission équitable plutôt qu'un nombre de paquets équitable.

- ✅ Activez-le sur les SSID à forte mixité de clients (invités, écoles).
- C'est transparent pour les bons clients, salvateur pour le débit global.

## 54. Recouvrement de cellules : le dosage du roaming

Pour un roaming fluide, les cellules doivent se **recouvrir de 15 à 20 %** :

- Trop peu de recouvrement → trous de couverture, coupures.
- Trop de recouvrement → interférences co-canal, sticky clients.

**Méthode terrain** : marchez entre deux AP avec l'analyseur Wi-Fi. Le client
doit voir les deux AP à **-67 dBm ou mieux** dans la zone de transition. Si ce
n'est pas le cas : rapprochez les AP ou montez légèrement la puissance.

## 55. Configurer la radio via l'app eKit (pas à pas)

1. `eKit > Site > Paramètres Wi-Fi > Radio`.
2. Par AP (ou par groupe) : bande 2,4 GHz → canal **1/6/11**, largeur **20 MHz**,
   puissance **14 dBm** (point de départ).
3. Bande 5 GHz → canal **36/40/44/48** (ou plan DFS validé), largeur **40 MHz**,
   puissance **14 dBm**.
4. Activez **band steering** sur le SSID bureautique.
5. Appliquez, attendez la propagation (1-2 min), vérifiez dans l'app que les
   valeurs sont bien prises en compte.
6. **Mesurez** : tour du site à l'analyseur, notez les niveaux par zone.

## 56. Exemples CLI (mode autonome — syntaxe indicative VRP)

> ⚠️ Syntaxe **plausible** pour un AP Huawei en mode Fat ; vérifiez sur votre
> version. En mode cloud, utilisez l'app.

```
<AP361> system-view
[AP361] wlan
[AP361-wlan-view] ap-id 0
[AP361-wlan-ap-0] radio 0                    # radio 0 = 2,4 GHz (convention)
[AP361-wlan-radio-0/0] channel 20mhz 6
[AP361-wlan-radio-0/0] eirp 14              # puissance en dBm (indicatif)
[AP361-wlan-radio-0/0] radio enable
[AP361-wlan-radio-0/0] quit
[AP361-wlan-ap-0] radio 1                    # radio 1 = 5 GHz (convention)
[AP361-wlan-radio-0/1] channel 40mhz 44
[AP361-wlan-radio-0/1] eirp 14
[AP361-wlan-radio-0/1] radio enable
[AP361-wlan-radio-0/1] quit
[AP361-wlan-ap-0] quit
[AP361-wlan-view] quit
[AP361] save
```

Vérification :

```
<AP361> display wlan radio all
<AP361> display wlan ap all
```

## 57. Tableau récapitulatif : profil radio « PME standard »

| Paramètre | 2,4 GHz | 5 GHz |
|---|---|---|
| Canal | 1 / 6 / 11 (alternés) | 36 / 40 / 44 / 48 (ou 40 MHz : 36, 44…) |
| Largeur | 20 MHz | 40 MHz (80 si justifié) |
| Puissance initiale | 14 dBm | 14 dBm |
| Band steering | — (bande d'accueil IoT) | Actif vers 5 GHz |
| Airtime fairness | Actif | Actif |
| 802.11b (débits 1-11 Mbit/s) | **Désactivés** si possible | N/A |

> 🧹 **Désactiver les bas débits 802.11b** : ça force les vieux clients à parler
> plus vite et libère de l'airtime. Attention : certains très vieux équipements
> (ou badges) en ont besoin → testez avant en condition réelle.

## 58. Checklist radio

- [ ] Plan de canaux documenté (tableau AP × canal 2,4 × canal 5)
- [ ] Largeurs : 20 MHz en 2,4 GHz, 40 MHz en 5 GHz (sauf justification)
- [ ] Puissances réglées manuellement, notées
- [ ] Band steering actif sur SSID bureautique
- [ ] Mesure de couverture faite (-60 dBm cible, -67 dBm mini voix)
- [ ] Zones de recouvrement vérifiées entre AP adjacents
- [ ] Canaux DFS évités près des zones radar (ou assumés et documentés)

---
---

# G. SSID ET SÉCURITÉ D'ACCÈS

## 59. Créer un SSID via l'app eKit

1. `eKit > Site > Wi-Fi > + SSID`.
2. Nom (SSID) : explicite, sans accent ni espace si possible
   (`Bureau-Etage2`, pas `Bureau Étage 2 café ☕`).
3. Bande : les deux (sauf SSID IoT → 2,4 GHz only, voir §60).
4. Sécurité : voir §60-61.
5. VLAN : voir §73.
6. Options : diffusion du SSID (oui sauf cas particulier, §63), isolation client
   (§93), limite de débit (§101).
7. Appliquer → vérifier la propagation sur tous les AP (1-2 min).

**Convention de nommage conseillée** (multi-sites) :

| SSID | Usage | Exemple |
|---|---|---|
| `STE-Bureau` | Collaborateurs | Préfixe société, pas de site dedans (le même SSID partout = roaming inter-sites naturel) |
| `STE-Invite` | Visiteurs | Portail captif |
| `STE-IoT` | Objets connectés | 2,4 GHz only, WPA2-PSK, VLAN isolé |
| `STE-Voix` | Téléphones Wi-Fi (optionnel) | QoS prioritaire, 5 GHz de préférence |

## 60. WPA2-PSK vs WPA3-SAE : que choisir en 2026

| Critère | WPA2-PSK (AES) | WPA3-SAE | Mode transition WPA2/WPA3 |
|---|---|---|---|
| Sécurité | Correcte si clé robuste (attaque par dictionnaire possible) | **Supérieure** (SAE résiste aux attaques hors ligne) | Les deux proposés |
| Compatibilité | Universelle | Problèmes avec vieux clients (Windows 7, vieilles imprimantes, IoT bas de gamme) | Bonne (négociation) |
| Recommandation | SSID IoT / legacy | SSID bureautique **si le parc client est récent** | ✅ **Choix par défaut** en PME |

**Recommandation terrain** :

- **SSID bureautique** : **WPA3-SAE**, ou **transition WPA2/WPA3** si le parc est
  hétérogène. Testez avec les PC les plus vieux du parc avant de généraliser.
- **SSID invité** : WPA2-PSK suffit (rotation de clé régulière) + portail captif.
- **SSID IoT** : WPA2-PSK (beaucoup d'objets ne connaissent pas WPA3).

> 🔑 **Clé robuste** : 20+ caractères, générée aléatoirement, stockée dans le
> coffre de l'équipe. Exemple **fictif** : `Trombone-Cactus-4821-Bureau!Vert`.
> Jamais de clé = nom de société + année.

## 61. SAE (WPA3-Personal) : points d'attention

- Le handshake SAE remplace le 4-way handshake PSK : plus de capture de handshake
  exploitable hors ligne.
- **PMF (Protected Management Frames)** : requis/recommandé avec WPA3 (voir §94).
  Certains vieux clients refusent de s'associer si PMF = required → utilisez
  « optional » en transition.
- Si un client **ne s'associe plus** après passage en WPA3 pur : repassez en
  transition et identifiez le client fautif (voir §123).

## 62. 802.1X / EAP : l'authentification entreprise

Pour un vrai contrôle d'accès (un identifiant par utilisateur, révocable), il faut
passer au **WPA2/WPA3-Enterprise (802.1X)** :

```
Client ---(EAP)---> AP361 ---(RADIUS)---> Serveur RADIUS (ex: NPS Windows, FreeRADIUS)
```

| Méthode EAP | Certificat serveur | Certificat client | Usage |
|---|---|---|---|
| **PEAP-MSCHAPv2** | Oui (requis) | Non | Le plus courant en PME (login/mot de passe AD) |
| **EAP-TLS** | Oui | **Oui** | Le plus sûr (PKI), plus lourd à déployer |
| EAP-TTLS | Oui | Non | Alternative à PEAP |

**Prérequis** :

- Serveur RADIUS joignable depuis les AP (ports UDP 1812/1813).
- Certificat serveur **valide** (sinon les clients affichent des alertes et les
  utilisateurs cliquent « accepter » → faille homme-du-milieu).
- 🔎 Vérifiez que votre version eKit expose la configuration RADIUS pour
  l'AP361 (sinon : SSID PSK + portail, ou gamme supérieure).

**En PME sans AD** : un **FreeRADIUS** sur un petit Linux fait le job pour
20-50 utilisateurs. Mais ça ajoute une brique à maintenir : pesez le pour et le
contre face à un WPA3-SAE avec clé robuste + rotation trimestrielle.

## 63. Portail captif (captive portal) : pour les invités

Le portail captif redirige le visiteur vers une page d'authentification
(CGU, code, identifiant/mot de passe temporaire).

**Bonnes pratiques** :

- SSID **dédié** (`STE-Invite`), VLAN **isolé** (voir §76), débit **limité**
  (ex. 10/10 Mbit/s par client).
- **Isolation client** activée : les invités ne se voient pas entre eux.
- Page de CGU courte, en français, avec durée de validité du ticket.
- **Ne jamais** mettre le portail captif comme seule sécurité du SSID
  bureautique : le trafic avant authentification n'est pas chiffré.

🔎 Les capacités de portail (interne à l'AP vs externe, personnalisation) dépendent
de la version eKit : vérifiez dans l'app.

## 64. SSID invité : le kit complet

- [ ] SSID dédié, WPA2-PSK (clé simple, affichée à l'accueil, **changée
      mensuellement**)
- [ ] Portail captif avec CGU
- [ ] VLAN invité dédié, **sans accès** au LAN (ACL ou firewall, voir §76)
- [ ] Isolation client activée
- [ ] Limite de débit par client (ex. 10 Mbit/s)
- [ ] Bande : les deux (ou 5 GHz only si zone dense)

## 65. SSID caché (non diffusé) : pour ou contre ?

**Contre** (position recommandée) :

- Ça n'apporte **aucune sécurité réelle** (le SSID se découvre en 30 secondes
  avec un sniffer).
- Ça complique la vie des utilisateurs (saisie manuelle) et **casse** le band
  steering et certains mécanismes de roaming.
- Les clients configurés en « SSID caché » **sondent en permanence** à la
  recherche du réseau → pollution radio + batterie.

**Pour** (seul cas acceptable) : SSID technique temporaire, ou contrainte
contractuelle explicite. Sinon : **diffusez vos SSID**.

## 66. Combien de SSID ? La règle des 3-4

Chaque SSID consomme de l'**airtime** (beacons ~10 fois/seconde par SSID et par
bande). Au-delà de 4-5 SSID par AP, la surcharge devient mesurable.

| Nb de SSID | Verdict |
|---|---|
| 1-2 | Idéal |
| 3-4 | ✅ Standard PME (bureau, invité, IoT, +1) |
| 5-6 | Limite haute, à justifier |
| 7+ | ⛔ Non : fusionnez ou utilisez le VLAN par d'autres moyens |

## 67. Exemples CLI — SSID et sécurité (indicatif VRP)

```
<AP361> system-view
[AP361] wlan
[AP361-wlan-view] ssid-profile name PROF-BUREAU
[AP361-wlan-ssid-prof-PROF-BUREAU] ssid STE-Bureau
[AP361-wlan-ssid-prof-PROF-BUREAU] quit
[AP361-wlan-view] security-profile name SEC-BUREAU
[AP361-wlan-ssid-prof-SEC-BUREAU] security wpa3-sae
[AP361-wlan-ssid-prof-SEC-BUREAU] wpa3 sae password cipher Exemple-Fictif-2026!Changez-Moi
[AP361-wlan-ssid-prof-SEC-BUREAU] quit
[AP361-wlan-view] vap-profile name VAP-BUREAU
[AP361-wlan-vap-prof-VAP-BUREAU] ssid-profile PROF-BUREAU
[AP361-wlan-vap-prof-VAP-BUREAU] security-profile SEC-BUREAU
[AP361-wlan-vap-prof-VAP-BUREAU] service-vlan vlan-id 20
[AP361-wlan-vap-prof-VAP-BUREAU] quit
```

> ⚠️ Noms de commandes **indicatifs** : `ssid-profile`, `security-profile`,
> `vap-profile`, `service-vlan` existent dans l'univers WLAN Huawei (AC/AP),
> mais les mots-clés exacts (`wpa3-sae`, etc.) varient selon la version.
> En mode cloud : faites-le dans l'app.

## 68. Rotation des clés PSK : procédure

1. Générez la nouvelle clé (20+ caractères aléatoires, stockée au coffre).
2. Appliquez-la **hors heures ouvrées** (chaque client devra se reconnecter).
3. Communiquez : affichez la nouvelle clé à l'accueil (invités), envoyez-la aux
   utilisateurs (bureau) via le canal interne habituel.
4. Vérifiez le lendemain : clients encore en échec = clé mal recopiée
   (le classique).
5. Fréquence : **trimestrielle** pour le SSID bureau, **mensuelle** pour invités.

## 69. Filtrage MAC : utile ou pas ?

- **Pas une sécurité** : une adresse MAC se spoofe en 2 minutes.
- **Utile en complément** : pour du matériel fixe connu (imprimantes, caméras)
  sur un SSID IoT, ça ajoute une barrière anti-erreur (pas anti-attaquant).
- **Coûteux à maintenir** : chaque changement de téléphone = une entrée à
  ajouter. En pratique : à réserver aux parcs stables.

## 70. PMF (Protected Management Frames / 802.11w)

- Protège les trames de management (désauthentification, désassociation) contre
  la falsification → anti « déauth attack ».
- **Requis** par WPA3, **optionnel** en WPA2.
- En WPA2 : passez en « capable/optional » si vos clients le supportent
  (la plupart des clients récents oui). Si un vieux client coince : repassez en
  « disabled » **pour ce SSID** et notez-le.

## 71. Checklist SSID/sécurité

- [ ] ≤ 4 SSID par AP
- [ ] SSID diffusés (pas de SSID caché sauf justification)
- [ ] Bureau : WPA3-SAE ou transition, clé 20+ caractères au coffre
- [ ] Invité : SSID + VLAN dédiés, portail, isolation, débit limité
- [ ] IoT : SSID 2,4 GHz only, WPA2-PSK, VLAN isolé
- [ ] PMF : required (WPA3) / optional (WPA2)
- [ ] Rotation des clés planifiée (trimestrielle/mensuelle)

## 72. Erreurs classiques SSID

| Erreur | Symptôme | Correction |
|---|---|---|
| WPA3 pur + vieux clients | Certains PC/imprimantes ne s'associent plus | Mode transition |
| Clé avec caractères exotiques | Saisie impossible sur IoT (pas de clavier) | Clé alphanumérique simple pour IoT |
| Trop de SSID | Débit global en berne | Fusionner, max 4 |
| Même clé partout depuis 3 ans | — (risque silencieux) | Rotation planifiée |
| SSID bureau sans VLAN | Invités sur le LAN si clé fuitée | VLAN dédiés (§73) |

---
---

# H. VLAN : SSID → VLAN, TRUNK VERS LE SWITCH

## 73. Le principe : un SSID = un VLAN (en général)

Le VLAN sépare les populations **au niveau filaire**, dès la sortie de l'AP :

```
Client Wi-Fi --(SSID STE-Bureau)--> AP361 --(tag VLAN 20)--> Switch --(routage/firewall)--> LAN/Internet
Client Wi-Fi --(SSID STE-Invite)--> AP361 --(tag VLAN 30)--> Switch --(routage/firewall)--> Internet seul
```

**Plan de VLAN type PME** :

| VLAN | Nom | Usage | Accès |
|---|---|---|---|
| 1 (défaut) | — | Éviter pour la prod | — |
| 10 | MGMT | Management (AP, switchs) | Admins uniquement |
| 20 | BUREAU | Collaborateurs Wi-Fi (+ filaire) | LAN + Internet |
| 30 | INVITE | Visiteurs Wi-Fi | Internet uniquement |
| 40 | IOT | Objets connectés | Internet restreint, pas de LAN |
| 50 | VOIX | Téléphonie (optionnel) | Serveur téléphonique |

> ⚠️ **Ne laissez pas les AP et les utilisateurs sur le même VLAN** : un invité
> qui devine l'IP de l'AP ne doit pas pouvoir ouvrir sa page d'admin.

## 74. Côté AP : associer SSID → VLAN

Dans l'app eKit : `SSID > Paramètres avancés > VLAN` → saisir l'ID (ex. 20).
En CLI (indicatif) : `service-vlan vlan-id 20` dans le vap-profile (voir §67).

**VLAN natif / non tagué** : le trafic de management de l'AP lui-même transite
(en général) **non tagué** sur le port du switch → le VLAN natif du port doit
correspondre au VLAN de management (voir §75).

## 75. Côté switch : le trunk vers l'AP

Le port du switch qui alimente l'AP doit être en **trunk** : VLAN management en
natif (non tagué) + VLANs utilisateurs tagués.

**Exemple côté switch Huawei (S5735, VRP — syntaxe réelle courante)** :

```
<SW> system-view
[SW] vlan batch 10 20 30 40
[SW] interface GigabitEthernet 0/0/5        # port vers AP-01
[SW-GigabitEthernet0/0/5] port link-type trunk
[SW-GigabotEthernet0/0/5] port trunk pvid vlan 10      # VLAN natif = management
[SW-GigabitEthernet0/0/5] port trunk allow-pass vlan 10 20 30 40
[SW-GigabitEthernet0/0/5] port-isolate enable          # optionnel : isole les ports AP entre eux
[SW-GigabitEthernet0/0/5] quit
[SW] save
```

Vérification :

```
<SW> display vlan
<SW> display port trunk
<SW> display interface GigabitEthernet 0/0/5
```

> 💡 **Test décisif** : client connecté au SSID Bureau → `ipconfig` doit montrer
> une IP du sous-réseau du VLAN 20, pas du VLAN 10. Sinon le tagging est faux
> (voir §126).

## 76. Isoler le VLAN invité : ACL / firewall

Un VLAN séparé ne suffit pas : il faut **bloquer le routage** vers le LAN.
Selon votre routeur/firewall :

**Exemple d'ACL sur switch L3 Huawei (logique indicative)** :

```
[SW] acl number 3001
[SW-acl-adv-3001] rule deny ip source 192.168.30.0 0.0.0.255 destination 192.168.0.0 0.0.255.255
[SW-acl-adv-3001] rule permit ip source 192.168.30.0 0.0.0.255 destination any
```

Traduction : le VLAN 30 (invités, 192.168.30.0/24) peut aller vers Internet
mais **pas** vers les réseaux internes (192.168.0.0/16).

**Mieux** : faites-le sur le **firewall** (USG/OPNsense/pfSense) avec une vraie
règle inter-VLAN + log. L'ACL switch dépanne, le firewall administre.

## 77. DHCP par VLAN

Chaque VLAN utilisateur a besoin de son **serveur DHCP** (sauf IP statiques, rare
en Wi-Fi) :

| VLAN | Sous-réseau exemple | DHCP | Passerelle |
|---|---|---|---|
| 10 MGMT | 192.168.10.0/24 | Statique ou réservations | 192.168.10.1 |
| 20 BUREAU | 192.168.20.0/24 | Serveur Windows / routeur | 192.168.20.1 |
| 30 INVITE | 192.168.30.0/24 | Routeur/firewall (bail court : 2 h) | 192.168.30.1 |
| 40 IOT | 192.168.40.0/24 | Routeur (baux longs, réservations) | 192.168.40.1 |

**Bail court sur INVITE** (1-2 h) : les adresses se recyclent vite avec le
turnover des visiteurs. **Réservations DHCP** sur MGMT : chaque AP a toujours la
même IP (indispensable pour la supervision).

## 78. Exemple complet : 3 SSID / 3 VLAN sur un site

```
Site : SIEGE-ETAGE2 — 6 x AP361

VLAN 10 MGMT   : 192.168.10.0/24  (AP en .11 à .16, switch .2, passerelle .1)
VLAN 20 BUREAU : 192.168.20.0/24  (SSID STE-Bureau, WPA3-SAE transition)
VLAN 30 INVITE : 192.168.30.0/24  (SSID STE-Invite, portail, isolé)
VLAN 40 IOT    : 192.168.40.0/24  (SSID STE-IoT, 2,4 GHz only)

Ports switch 1-6 : trunk, PVID 10, allow 10/20/30/40
DHCP : Windows Server pour VLAN 20, firewall pour 30/40
DNS : interne pour 20, public (ou filtré) pour 30
```

## 79. Erreurs VLAN classiques

| Erreur | Symptôme | Diagnostic |
|---|---|---|
| PVID ≠ VLAN management | L'AP ne joint plus le cloud après passage en trunk | Remettre le port en access le temps de corriger, vérifier le PVID |
| VLAN oublié dans allow-pass | Clients associés mais pas d'IP / pas de ping passerelle | `display port trunk`, ajouter le VLAN |
| SSID → mauvais VLAN | Un invité obtient une IP du LAN | Vérifier le mapping SSID/VLAN dans l'app |
| DHCP manquant sur le VLAN | Associé mais « pas d'accès Internet » / APIPA 169.254.x.x | Tester en filaire sur le même VLAN |
| VLAN 1 utilisé partout | Aucun symptôme… jusqu'au jour où | Migrer vers le plan ci-dessus |

## 80. Checklist VLAN

- [ ] Plan de VLAN documenté (ID, nom, sous-réseau, usage)
- [ ] Ports AP en trunk, PVID = VLAN management
- [ ] `allow-pass` = uniquement les VLANs nécessaires (pas de `all` par paresse)
- [ ] DHCP actif sur chaque VLAN utilisateur
- [ ] Réservations DHCP pour les AP (IP fixes)
- [ ] VLAN invité bloqué vers le LAN (règle firewall testée)
- [ ] Test par SSID : bonne IP, bon accès, bonnes restrictions

---
---

# I. ITINÉRANCE (ROAMING)

## 81. Le problème : le client décide, pas l'AP

En Wi-Fi, **c'est le client qui décide** de changer d'AP (contrairement au
cellulaire). L'AP peut l'encourager (802.11k/v) et accélérer la ré-authentification
(802.11r), mais un client têtu restera accroché à un AP lointain. D'où l'importance
de comprendre les trois protocoles.

## 82. 802.11k : le client voit la carte

Le 802.11k permet à l'AP de fournir au client la **liste des AP voisins** avec
leurs canaux. Le client n'a plus à scanner tous les canaux (scan = coupure) :
il sait où aller.

- ✅ Activez-le : roaming plus rapide, moins de coupures.
- Invisible pour l'utilisateur, bénéfique surtout aux smartphones modernes.

## 83. 802.11v : l'AP suggère (BSS Transition)

Le 802.11v permet à l'AP de **suggérer** au client de migrer vers un meilleur AP
(trame BSS Transition Management).

- ✅ Activez-le en complément du k.
- L'AP peut ainsi **délester** un AP surchargé vers un voisin moins chargé
  (load balancing).
- Les clients ne sont pas obligés d'obéir, mais la plupart des clients récents
  suivent la suggestion.

## 84. 802.11r (Fast BSS Transition) : la ré-authentification express

Le 802.11r accélère la poignée de main lors du changement d'AP (utile surtout en
WPA2/WPA3-Enterprise où la ré-authentification 802.1X est lente).

- ✅ **Indispensable pour la voix sur Wi-Fi** (téléphones, softphones) : sans r,
  la coupure peut dépasser 200-500 ms → microcoupure audible.
- En PSK simple, le gain est moindre mais réel.
- ⚠️ Quelques vieux clients supportent mal le 802.11r → s'ils décrochent en
  boucle, testez avec r désactivé **sur un SSID de test** avant de toucher à la prod.

**Trio gagnant** : k + v + r activés sur le SSID bureautique/voix. C'est le
réglage « ne pas réfléchir » qui marche dans 90 % des cas.

## 85. PMK caching / OKC : les cousins du r

- **PMK caching** : le client réutilise sa clé maîtresse en revenant sur un AP
  déjà visité (pas de ré-authentification complète).
- **OKC (Opportunistic Key Caching)** : variante inter-AP.
- En pratique : laissez les valeurs par défaut sauf problème avéré. Le 802.11r
  couvre le besoin principal.

## 86. Sticky client : le fléau des open spaces

**Symptôme** : un utilisateur au fond de l'open space reste accroché à l'AP
d'entrée avec 1 barre, alors qu'un AP est à 5 mètres. Débit minable, plaintes.

**Causes** :

1. Puissance trop forte (le client « entend » toujours l'ancien AP — voir §49).
2. Seuils de roaming côté AP trop permissifs.
3. Client mal élevé (pilote Wi-Fi ancien, notamment sur vieux PC Windows).

**Remèdes** :

- Baisser la puissance (§49) — remède n°1.
- Activer k/v pour encourager le départ.
- **Déconnexion des clients faibles** : certains firmwares permettent de
  déconnecter un client sous un seuil RSSI (ex. -75 dBm) pour le forcer à
  se réassocier au meilleur AP. À utiliser avec doigté (trop agressif = flap).
- Côté client : mettre à jour les pilotes Wi-Fi (surtout Intel/Realtek sur Windows).

## 87. Cas voix : téléphonie sur Wi-Fi

Exigences pour de la voix correcte :

- [ ] Couverture **-67 dBm minimum** partout où l'on téléphone (pas -70, pas -75)
- [ ] 802.11r **activé**
- [ ] 5 GHz privilégié (band steering)
- [ ] QoS voix prioritaire (voir §99)
- [ ] Recouvrement de cellules vérifié en marchant (voir §54)
- [ ] Test d'appel en marchant d'un bout à l'autre du site : **zéro coupure**

> 📞 Si la direction veut « le Wi-Fi pour les téléphones » : faites signer ce
> tableau d'exigences **avant**. La voix ne pardonne pas une couverture « à peu près ».

## 88. Checklist roaming

- [ ] 802.11k activé (SSID bureau/voix)
- [ ] 802.11v activé
- [ ] 802.11r activé (indispensable si voix)
- [ ] Puissances équilibrées entre AP adjacents (pas un AP à fond au milieu de petits)
- [ ] Test de marche : association → déplacement → vérifier le changement d'AP
      (dans l'app eKit : client visible sur le nouvel AP)
- [ ] Pilotes Wi-Fi des PC du parc à jour (campagne annuelle)

---
---

# J. SÉCURITÉ WI-FI (WIDS, ROGUE AP, ISOLATION)

## 89. WIDS/wIPS sur l'AP361 : périmètre honnête

WIDS (détection d'intrusion sans fil) / wIPS (prévention) : l'AP surveille les
trames pour détecter les comportements suspects.

Sur un AP d'entrée de gamme comme l'AP361 en gestion eKit, attendez-vous à
l'essentiel (🔎 détail exact selon version) :

| Fonction | Attente réaliste |
|---|---|
| Détection rogue AP | ✅ Probable (voir §90) |
| Détection d'attaques deauth | ✅/🔎 Selon version |
| Contre-mesures actives (déauth du rogue) | 🔎 Souvent réservé aux gammes supérieures / contrôleur |
| Classification auto autorisé/voisin/rogue | 🔎 Selon version |

> 💡 En PME, le wIPS « de base » suffit à **voir** les problèmes. La
> contre-mesure active (qui est juridiquement délicate — vous émettez vers un
> équipement tiers) reste un sujet d'expert : contentez-vous de détecter et
> d'alerter, sauf politique écrite.

## 90. Détection des rogue AP : la méthode

Un **rogue AP** = un point d'accès non autorisé sur votre réseau (ex. : un
collaborateur qui branche sa box perso « pour avoir du Wi-Fi dans son bureau »,
ou un attaquant).

**Procédure** :

1. Activez la détection rogue dans l'app eKit (si disponible).
2. L'AP361 écoute et remonte les SSID/BSSID vus qui ne font pas partie du site.
3. **Qualifiez** chaque détection :
   - SSID de l'entreprise émis par un BSSID inconnu → **suspect** (evil twin possible).
   - Box du voisin d'en face → **voisin**, à ignorer (mais notez le canal : interférences).
   - Répéteur inconnu branché sur votre LAN → **rogue avéré** : débranchez physiquement.
4. Pour confirmer qu'un rogue est **sur votre LAN** (wired rogue) : cherchez sa
   MAC sur votre switch (`display mac-address`) → le port donne l'emplacement physique.

**Réflexe** : à chaque détection « SSID entreprise / BSSID inconnu », traitez
comme un incident (voir §145) jusqu'à preuve du contraire.

## 91. Evil twin : le jumeau maléfique

Un attaquant diffuse **votre SSID** depuis son propre AP pour capturer des
handshakes ou servir un faux portail.

**Défense** :

- WPA3-SAE / 802.1X : l'attaquant ne peut pas prouver son identité → les clients
  bien configurés refusent (d'où l'importance des certificats valides en EAP).
- En PSK : sensibilisez les utilisateurs (« si votre téléphone redemande la clé
  du Wi-Fi bureau sans raison, appelez l'IT »).
- Détection rogue (§90) + ronde occasionnelle à l'analyseur Wi-Fi.

## 92. Attaques deauth : comprendre et parer

Une trame de **désauthentification** forgée peut déconnecter un client (outil
d'attaque classique, 2 minutes avec un ESP32).

- **Parade** : PMF / 802.11w (§70). Avec PMF required, les trames deauth forgées
  sont rejetées.
- **Détection** : pics de déconnexions simultanées sur un AP = suspect.
- En WPA2 sans PMF : vous êtes vulnérable. C'est un argument de plus pour
  migrer vers WPA3 ou activer PMF.

## 93. Isolation client (client isolation) : quand l'activer

L'isolation empêche les clients d'un même SSID de communiquer **entre eux**
(ils ne voient que la passerelle).

| SSID | Isolation | Pourquoi |
|---|---|---|
| Invité | ✅ **Toujours** | Les visiteurs n'ont rien à se dire |
| IoT | ✅ Recommandé | Une caméra piratée ne doit pas attaquer la suivante |
| Bureau | ⚠️ Selon besoin | Casse le partage local (imprimantes, Chromecast, partage Windows) |

**Piège** : activer l'isolation sur le SSID bureau un vendredi soir = lundi
matin, « l'imprimante réseau ne marche plus » et « je ne vois plus le PC de
Paul ». Testez les usages **avant**.

## 94. PMF : rappel opérationnel

Déjà vu au §70. En résumé d'exploitation :

- WPA3 → PMF required (imposé).
- WPA2 → PMF optional si le parc le supporte, sinon disabled (noté au dossier).
- Un client qui **s'associe puis est immédiatement éjecté** après un changement
  PMF = incompatibilité PMF → revenez en arrière pour ce SSID.

## 95. Sécurité du management : l'AP lui-même est une cible

- L'AP a une IP, un OS, des services : c'est un équipement réseau à part entière.
- VLAN management dédié (§73), pas d'accès depuis les VLANs utilisateurs.
- Comptes nominatifs, mots de passe robustes (§144).
- HTTPS uniquement (§146), SSH restreint (§145).

## 96. Checklist sécurité Wi-Fi

- [ ] Détection rogue activée et qualifiée (pas de « rogue » non traité > 48 h)
- [ ] PMF : required (WPA3) / optional ou disabled documenté (WPA2)
- [ ] Isolation client : active sur Invité et IoT, choix documenté sur Bureau
- [ ] Pas de WEP, pas de WPA-TKIP (obsolètes, cassés)
- [ ] Clés PSK robustes + rotation planifiée
- [ ] Ronde Wi-Fi trimestrielle à l'analyseur (nouveaux SSID suspects ?)

---
---

# K. QOS / WMM

## 97. WMM : les 4 files d'attente

Le WMM (Wi-Fi Multimedia) classe le trafic en 4 catégories d'accès, par priorité
décroissante :

| File (AC) | Usage | Priorité |
|---|---|---|
| AC_VO (Voice) | Voix (téléphonie) | La plus haute |
| AC_VI (Video) | Vidéo (visio) | Haute |
| AC_BE (Best Effort) | Bureautique courante | Normale |
| AC_BK (Background) | Mises à jour, backup | La plus basse |

**C'est actif par défaut** sur la plupart des équipements Wi-Fi : ne le désactivez
jamais sans raison valable.

## 98. Marquer le trafic : DSCP et 802.1p

L'AP mappe les marques du monde filaire vers les files WMM :

| DSCP | 802.1p (CoS) | File WMM | Usage |
|---|---|---|---|
| EF (46) | 5 | AC_VO | Voix |
| AF41 (34), CS4 | 4 | AC_VI | Vidéo |
| 0 (BE) | 0 | AC_BE | Défaut |

**À faire côté filaire** : votre switch/téléphonie doit **marquer** (ou faire
confiance aux marques) en entrée. Si rien n'est marqué, tout tombe en Best
Effort et la QoS Wi-Fi ne sert à rien. Vérifiez la chaîne complète :
téléphone → switch → AP → air.

## 99. Prioriser la voix et la visio : recette

1. **Marquage** : les téléphones marquent EF (souvent par défaut) ; vérifiez.
2. **Confiance** : sur le port du switch vers l'AP, `trust dscp` (ou cos).
3. **SSID voix dédié** (optionnel mais propre) : QoS maximale, 5 GHz, 802.11r.
4. **Limitez le reste** : plafond de débit sur le SSID invité pour qu'un
   streaming 4K d'invité ne mange pas l'airtime de la visio du directeur.
5. **Testez en charge** : appel + speedtest simultanés → la voix doit rester claire.

Exemple indicatif côté switch Huawei (trust DSCP sur port AP) :

```
[SW-GigabitEthernet0/0/5] trust dscp
```

## 100. Limitation de débit (rate limiting) par SSID / par client

Utile pour :

- **Invités** : 10 Mbit/s down / 5 Mbit/s up par client (suffit pour mail/web,
  décourage le streaming massif).
- **IoT** : 2-5 Mbit/s (une caméra n'a pas besoin de plus en général — sauf
  4K, à dimensionner).
- **Bureau** : généralement pas de limite, ou limite haute anti-abus
  (ex. 100 Mbit/s).

🔎 Vérifiez les granularités offertes par votre version eKit (par SSID, par
client, montants/descendants séparés).

## 101. Admission control : le videur de la boîte

L'admission control refuse de nouveaux clients voix/vidéo quand la cellule est
saturée, plutôt que de dégrader tout le monde. C'est un réglage avancé :
à n'activer que si vous avez un vrai besoin voix dense (centre d'appels sur Wi-Fi)
et après mesure.

## 102. Erreurs QoS classiques

| Erreur | Symptôme |
|---|---|
| Rien n'est marqué côté filaire | La QoS Wi-Fi ne change rien (tout en BE) |
| Trust boundary mal placé | Les marques des clients sont effacées au switch |
| Pas de limite sur l'invité | Un invité en streaming 4K sature la cellule |
| WMM désactivé « pour tester » | Tout le monde en BE, voix hachée — et on oublie de le réactiver |

## 103. Checklist QoS

- [ ] WMM actif (par défaut — vérifié, pas supposé)
- [ ] Chaîne de marquage vérifiée de bout en bout (téléphone → AP)
- [ ] `trust dscp` (ou équivalent) sur les ports AP du switch
- [ ] SSID invité : débit plafonné
- [ ] Test en charge voix + données : OK

---
---

# L. SUPERVISION

## 104. Superviser via eKit : le quotidien

Le cloud/l'app eKit est votre écran de contrôle principal :

**Quotidien (2 minutes)** :

- Tous les AP verts ? (Si un est gris : voir §124.)
- Alarmes non acquittées ? (Les traiter ou les qualifier.)
- Un AP avec un nombre de clients anormalement haut/bas par rapport à d'habitude ?

**Hebdomadaire (15 minutes)** :

- Courbe de clients par AP : un AP saturé régulièrement = redécoupage de couverture.
- Taux d'échec d'association : en hausse = interférences ou problème de clé/serveur.
- Versions firmware : homogènes ?
- Espace disque / état du cloud : RAS.

## 105. Alertes : lesquelles configurer

| Alerte | Seuil indicatif | Action |
|---|---|---|
| AP hors ligne | Immédiat | Intervention (PoE/réseau) |
| Taux d'échec d'asso > 10 % | 15 min glissantes | Diagnostic interférences/auth |
| Utilisation canal > 70 % | 1 h | Rééquilibrage canaux/puissance |
| Rogue AP détecté | Immédiat | Qualification (§90) |
| Firmware non homogène | Hebdo | Planifier upgrade |

🔎 Les seuils configurables dépendent de la version eKit : adaptez ce tableau à
ce que l'outil permet réellement.

## 106. SNMP : la supervision « à l'ancienne » qui dépanne

Si l'AP361 expose SNMP (🔎 à vérifier selon version — souvent SNMP v2c/v3 en
lecture seule sur les AP Huawei) :

1. Activez SNMP **v3** (auth + priv) — jamais v1/v2c en production sauf
   contrainte (communautés en clair).
2. Restreignez l'accès SNMP au VLAN management et à l'IP du superviseur.
3. Intégrez à votre supervision existante (**Zabbix** — voir votre guide Zabbix) :
   - `sysUpTime`, état des radios, nombre de clients associés.
   - État du port Ethernet (up/down, vitesse négociée).
4. MIB Huawei : `HUAWEI-WLAN-*` (🔎 vérifiez les MIB supportées par le firmware).

Exemple indicatif (syntaxe VRP classique si CLI dispo) :

```
[AP361] snmp-agent sys-info version v3
[AP361] snmp-agent group v3 MONITEUR authentication
[AP361] snmp-agent usm-user v3 UserZabbix group MONITEUR auth sha Fictif-Auth-2026! priv aes Fictif-Priv-2026!
[AP361] snmp-agent target-host trap address udp-domain 192.168.10.50 params securityname UserZabbix v3 authentication
```

> ⚠️ Mots de passe **fictifs** ci-dessus. En vrai : 20+ caractères au coffre.

## 107. Syslog : centraliser les journaux

- Envoyez les logs vers votre serveur syslog central (celui de votre infra,
  ou un Graylog/Loki — voir vos guides).
- Niveau : `informational` en temps normal, `debugging` ponctuellement sur
  demande (jamais en continu : ça noie le serveur).
- **À logger impérativement** : associations/échecs, changements de config,
  upgrades, reboots, détections rogue.

Exemple indicatif :

```
[AP361] info-center loghost 192.168.10.60
[AP361] info-center source default channel loghost log level informational
```

## 108. Les KPI à suivre (tableau de bord)

| KPI | Cible | Où le voir |
|---|---|---|
| Disponibilité AP | > 99,5 % mensuel | eKit / Zabbix (ping + SNMP) |
| Clients max simultanés / AP | < 40-50 (AP361) | eKit |
| Taux d'échec d'association | < 5 % | eKit |
| Utilisation canal 5 GHz | < 60 % en heures pleines | eKit |
| Débit médian client (5 GHz) | > 50 Mbit/s en bureautique | Tests iperf périodiques |
| Temps de roaming (voix) | < 100 ms | Test d'appel en marchant |

> 📊 Un KPI sans cible, c'est de la déco. Fixez les cibles **avec** la direction
> (ou au moins informez-la) : le jour d'une plainte, vous sortez le graphe.

## 109. Zabbix : intégration type

Vous avez déjà un guide Zabbix : voici le template minimal « AP361 » :

- **Ping** : disponibilité (trigger : 3 échecs → alerte).
- **SNMP** : `sysUpTime` (reboot détecté si reset), nombre de clients par radio,
  état du port GE.
- **Syslog** : corrélation (reboot → chercher l'upgrade ou la coupure PoE).
- **Carte** : un « host » par AP, groupés par site, avec la topologie.

## 110. Rapport mensuel Wi-Fi (modèle)

Fournissez à la direction (ou gardez pour vous) un rapport d'une page :

1. Disponibilité du parc (%)
2. Top 3 des AP les plus chargés (+ action prévue si saturation)
3. Incidents du mois (date, durée, cause, résolution)
4. Firmwares : version déployée, homogénéité
5. Actions préventives du mois suivant

> 💼 Ce rapport, c'est votre assurance : le jour où « le Wi-Fi ne marche jamais »,
> vous répondez avec des chiffres, pas avec des impressions.

## 111. Checklist supervision

- [ ] Tous les AP dans eKit + dans Zabbix (double rattachement)
- [ ] Alertes configurées (tableau §105 adapté à l'outil)
- [ ] Syslog centralisé, niveau informational
- [ ] SNMP v3 restreint au VLAN management (si supporté)
- [ ] Rapport mensuel produit et archivé

---
---

# M. SAUVEGARDE, RESTAURATION, FIRMWARE

## 112. Sauvegarder la configuration : méthode

**En mode cloud** : la config vit dans le cloud eKit. Complétez par :

- Une **documentation versionnée** du site : SSID, clés (référence au coffre,
  jamais en clair), VLAN, plan radio, versions firmware.
- Des **captures d'écran** des pages de config critiques (ou export si dispo — 🔎).
- Le **tableau d'adressage** (AP, IP, MAC, SN, emplacement).

**En mode Fat** : sauvegarde classique :

```
<AP361> save                       # sauvegarde en flash (vrpcfg.cfg)
<AP361> backup configuration to 192.168.10.60  ap361-etage2.cfg
```

(🔎 Commande exacte selon version : `save`, puis transfert TFTP/FTP/SFTP vers
le serveur de sauvegarde.)

**Règle** : toute modification de config = sauvegarde + note au journal
(qui, quand, pourquoi). Ça prend 2 minutes et ça sauve des carrières.

## 113. Restaurer : procédure

1. Identifiez la **bonne** sauvegarde (date, version firmware compatible).
2. En Fat : transférez le fichier puis `startup saved-configuration <fichier>`
   + reboot (🔎 syntaxe selon version).
3. En cloud : réappliquez le template / restaurez depuis l'historique du site
   si la fonction existe.
4. **Vérifiez** : SSID diffusés, VLAN, test client complet (§31).
5. Si ça ne revient pas : reset d'usine (§26) + reconfiguration depuis la doc
   (d'où l'importance de la doc versionnée, pas seulement du fichier binaire).

## 114. Mise à jour firmware : procédure complète

**Avant** :

- [ ] Lire les **release notes** (correctifs sécu ? régressions connues ? prérequis ?)
- [ ] Sauvegarder la config (§112)
- [ ] Choisir la fenêtre : **hors heures ouvrées**, avec marge (prévoir 2x le temps estimé)
- [ ] Prévenir les utilisateurs (« maintenance Wi-Fi de 20 h à 22 h »)
- [ ] Vérifier l'espace flash et la stabilité PoE (pas d'upgrade pendant un orage)

**Pendant** :

1. Upgrader **1 AP pilote**, attendre le reboot complet.
2. Valider 24 h : associations, roaming, débit, logs (pas d'erreur nouvelle).
3. Généraliser **par vagues** (ex. un étage par soir), jamais tout le parc d'un coup.
4. Surveiller chaque vague (un AP qui ne revient pas en 15 min = intervention).

**Après** :

- [ ] Tous les AP sur la même version (vérifié un par un)
- [ ] Tests clients OK sur chaque zone
- [ ] Note au journal : version, date, AP concernés, incidents

## 115. Rollback : revenir en arrière

Si la nouvelle version pose problème (bug, incompatibilité client) :

1. **Ne paniquez pas** : la plupart des équipements Huawei conservent l'ancienne
   image en secours (🔎 vérifiez : `display version`, partition backup).
2. Procédure type (indicatif) : redémarrer sur l'image précédente via la
   commande de boot ou via l'app eKit (fonction « revenir à la version
   précédente » si elle existe — 🔎).
3. Si pas d'image de secours : réinstallez l'ancienne version (fichier conservé
   sur votre serveur — **gardez toujours les N-1**).
4. **Règle d'or** : ne supprimez l'ancienne image qu'après **2 semaines** de
   production stable sur la nouvelle.

## 116. Gérer les versions sur un parc multi-sites

| Principe | Détail |
|---|---|
| Une version de référence | La même partout (ex. `VxxxRxxxCxxSPCxxx`) |
| Fenêtre de validation | 2 semaines sur site pilote avant généralisation |
| Gel des upgrades | Pas d'upgrade en période critique (inventaires, examens, clôtures) |
| Registre | Tableau : site / AP / version / date d'upgrade / validé par |

## 117. Ce qu'il ne faut jamais faire

- ⛔ Upgrader pendant les heures ouvrées « parce que c'est rapide ».
- ⛔ Couper le PoE pendant l'écriture flash.
- ⛔ Upgrader sans avoir lu les release notes.
- ⛔ Laisser cohabiter 3 versions différentes « parce que ça marche ».
- ⛔ Supprimer l'image N-1 le jour même.

## 118. AP « briquée » après upgrade interrompu : sauvetage

Symptômes : LED rouge/éteinte anormale, pas d'IP, reset sans effet.

1. Laissez-la **10 minutes** (certains bootloaders tentent une récupération auto).
2. Tentez le reset long (§26).
3. Si l'AP expose un mode de secours (BootROM via console — 🔎 selon modèle,
   port console non confirmé sur AP361) : réinjection du firmware via TFTP.
4. Sinon : **RMA** (retour garantie). D'où l'importance d'avoir **1 AP de spare**
   en stock (voir §140).

## 119. Checklist maintenance logicielle

- [ ] Registre des versions tenu à jour
- [ ] Config sauvegardée avant chaque changement
- [ ] Images N et N-1 conservées sur le serveur
- [ ] 1 AP de spare en stock, sur la version de référence
- [ ] Fenêtres de maintenance planifiées au trimestre

---
---

# N. DÉPANNAGE TERRAIN — 16 CAS

## 120. Méthode : le modèle « Symptôme → Hypothèses → Test → Solution »

Avant les cas particuliers, la méthode qui marche à tous les coups :

1. **Qualifier** : qui est impacté (1 client ? 1 AP ? tout le site ?), depuis quand,
   quoi qui a changé (travaux, nouvel équipement, upgrade ?).
2. **Segmenter** : le problème est-il **radio** (le client ne voit pas / signal
   faible), **association** (voit mais ne se connecte pas), **réseau** (connecté
   mais pas d'IP / pas d'Internet) ou **alimentation** (l'AP est mort) ?
3. **Tester du simple vers le compliqué** : câble → PoE → IP → VLAN → SSID →
   radio → client.
4. **Une variable à la fois** : changez un paramètre, testez, notez. Sinon vous
   ne saurez jamais ce qui a réparé (ni ce qui a cassé).
5. **Documentez** : fiche d'incident (date, symptômes, cause, solution, durée).
   Dans 6 mois, vous vous remercierez.

**Kit de dépannage Wi-Fi** (dans le sac, toujours) :

- Téléphone avec analyseur Wi-Fi + app eKit
- PC portable + câble console/RJ45 de secours
- Testeur de câble + testeur PoE
- Tournevis, trombone (reset), étiqueteuse
- AP361 de spare pré-configuré

---

## 121. Cas n°1 — Interférences : débit en dents de scie

**Symptômes** : débit qui s'effondre par moments, ping irrégulier, surtout à
certaines heures. Le signal est bon (-55 dBm) mais ça rame.

**Diagnostic** :

1. Analyseur Wi-Fi : regardez l'**utilisation du canal** (channel utilization).
   > 60-70 % en permanence = canal saturé.
2. Listez les SSID voisins sur le même canal (co-canal) ou chevauchant (adjacent).
3. Cherchez les **non-Wi-Fi** : four micro-ondes (2,4 GHz), caméras analogiques,
   Bluetooth massif, variateurs.
4. Corrélez avec l'heure : micro-ondes = heures des repas ; voisin = heures de bureau.

**Solutions** :

- Changez de canal (le moins occupé mesuré, pas deviné).
- En 2,4 GHz : passez en 20 MHz si ce n'est pas fait, canaux 1/6/11 stricts.
- Éloignez/éteignez la source (le micro-ondes du bureau à 1 m de l'AP, c'est du vécu).
- Baissez la puissance pour réduire la zone de contention.
- En dernier recours : ajoutez un AP pour diviser la charge (plus de cellules
  petites = moins de clients par cellule).

## 122. Cas n°2 — Débit faible alors que le signal est bon

**Symptômes** : -55 dBm, mais 15 Mbit/s au speedtest sur un AP361 capable de bien plus.

**Diagnostic** (dans l'ordre) :

1. **Négociation du port** : l'AP est-il en 100 Mbit/s au lieu de 1000 ?
   (eKit ou switch : `display interface`). Si oui → câble à refaire (§124).
2. **Bande** : le client est-il en 2,4 GHz alors que le 5 GHz est dispo ?
   (band steering, §52).
3. **Largeur de canal** : 20 MHz là où 40 était prévu ?
4. **Vieux client** : un client 802.11n ou avec pilote pourri plombe l'airtime
   (airtime fairness, §53).
5. **Charge de la cellule** : 30 clients actifs sur un AP361 = normal que ça
   rame. Mesurez à 3 h du matin pour comparer.
6. **Lien montant** : testez en filaire au switch. Si le filaire rame aussi,
   le problème n'est pas le Wi-Fi (lien Internet, firewall saturé…).

**Solutions** : selon la cause trouvée. Le cas le plus fréquent en PME : **port
négocié à 100 Mbit/s** à cause d'un câble limite. Refaire le câble = débit x10.

## 123. Cas n°3 — Des clients ne s'associent pas (ou plus)

**Symptômes** : le SSID est visible, mais la connexion échoue (« impossible de se
connecter », boucle d'authentification).

**Diagnostic** :

1. **Un seul client ou tous ?** Un seul → problème client (pilote, clé mal
   tapée, certificat). Tous → problème AP/réseau.
2. **Quoi qui a changé ?** Changement de clé ? Passage WPA3 ? PMF activé ?
   Upgrade firmware ?
3. **Type de client** : les vieux clients + WPA3 pur = échec (§61). Les IoT +
   band steering agressif = échec (§52).
4. **Logs** : échecs d'authentification dans eKit / syslog (mauvaise clé vs
   timeout vs rejet EAP).
5. **Serveur RADIUS** (si 802.1X) : est-il joignable ? Certificat expiré ?
   (Le classique : certificat RADIUS expiré un dimanche.)

**Solutions** :

- Mauvaise clé : re-saisir (le copier-coller évite les erreurs).
- Incompatibilité WPA3 : mode transition.
- Pilote Wi-Fi obsolète : mise à jour (surtout Intel AX200/201 et Realtek).
- « Oublier le réseau » côté client + se reconnecter (résout 50 % des cas
  bizarres post-changement de clé).
- Certificat RADIUS expiré : renouveler (et mettre une alerte d'expiration !).

## 124. Cas n°4 — Le PoE ne monte pas (LED éteinte)

**Symptômes** : AP muet, LED éteinte.

**Diagnostic** (5 minutes chrono) :

1. L'AP sur un **autre port** du switch (connu bon) → s'allume ? Oui = port ou
   câble d'origine en cause.
2. Un **autre équipement PoE** sur le port suspect → s'allume ? Non = port/câble.
3. **Testeur PoE** en bout de câble : tension présente ? (44-57 V attendus).
4. **Budget PoE** du switch : dépassé ? (`display poe` / voyant).
5. L'AP sur un **injecteur connu bon** → s'allume ? Non = AP HS (RMA).

**Solutions** : voir tableau §24. Le trio gagnant des causes : câble trop long/
abîmé, budget PoE dépassé, injecteur sous-dimensionné.

## 125. Cas n°5 — L'AP reboote en boucle

**Symptômes** : LED qui s'allume, l'AP apparaît brièvement puis disparaît, en boucle.

**Causes possibles** :

1. **Alimentation limite** : le PoE « tient » au boot puis s'écroule en charge
   (pics de consommation au démarrage radio). → Tester avec injecteur 802.3at.
2. **Firmware corrompu** : suite à upgrade interrompu → §118.
3. **Boucle réseau** : l'AP branché sur deux ports (ou via un petit switch
   bouclé) → storm → watchdog → reboot. Vérifiez le câblage.
4. **Surchauffe** : AP dans un caisson ou sous toiture à 55 °C → protection
   thermique. Mesurez.

## 126. Cas n°6 — Le DHCP ne passe pas (APIPA 169.254.x.x)

**Symptômes** : client associé au Wi-Fi, mais IP en 169.254.x.x → « connecté,
pas d'accès Internet ».

**Diagnostic** :

1. Le problème touche-t-il **tous les SSID/VLAN** ou un seul ? Un seul → VLAN/
   tagging en cause. Tous → DHCP ou trunk.
2. **Test filaire** : un PC en filaire sur le même VLAN obtient-il une IP ?
   Non → le problème n'est pas le Wi-Fi (serveur DHCP down, VLAN mal routé).
3. **Trunk** : le VLAN du SSID est-il dans `allow-pass` du port AP ? (§75).
4. **Port du switch** : PVID correct ? Le serveur DHCP est-il joignable depuis
   ce VLAN (relai DHCP / ip helper si le serveur est sur un autre VLAN) ?
5. **Plage épuisée** : le scope DHCP est-il plein ? (baux trop longs sur un
   VLAN invité à fort turnover).

**Solutions** : corriger le trunk, ajouter le relai DHCP, purger/raccourcir les
baux, étendre le scope. **Piège** : le relai DHCP oublié quand le serveur DHCP
est sur un autre VLAN que les clients (classique en multi-VLAN).

## 127. Cas n°7 — Le roaming casse les appels (voix hachée en marchant)

**Symptômes** : appel OK à l'arrêt, hachures/coupures en se déplaçant.

**Diagnostic** :

1. Couverture : mesurez en marchant — des zones < -67 dBm ? → ajouter/rapprocher
   des AP (§54).
2. 802.11r : activé ? (§84). Sans r, chaque changement d'AP = ré-authentification
   lente.
3. Sticky client : le téléphone reste-t-il accroché trop longtemps à l'ancien AP ?
   (puissance trop forte, §86).
4. QoS : la voix est-elle marquée et priorisée ? (§99). Un appel qui partage
   l'airtime avec un download massif sans QoS = hachures.
5. Le téléphone : certains smartphones ont un roaming agressif/conservateur
   réglable dans les options avancées (seuil de roaming).

**Solutions** : r + k + v activés, couverture -67 dBm partout, QoS voix,
puissances équilibrées. **Test de validation** : appel de 5 minutes en marchant
sur tout le site, zéro coupure.

## 128. Cas n°8 — « Le Wi-Fi est lent depuis les travaux »

**Symptômes** : dégradation datée précisément d'un événement (travaux, nouveau
voisin, nouvel équipement).

**Causes typiques** :

- Nouvelle **cloison** (placo avec isolant métallisé, vitre blindée) → zone d'ombre.
- Nouvel équipement **perturbateur** (variateur LED, onduleur, moteur).
- Câble **sectionné puis réparé** à la va-vite (domino, scotch) → 100 Mbit/s.
- AP **déplacé** par les peintres et jamais remis (« il gênait »).
- Nouveau **voisin Wi-Fi** agressif sur votre canal.

**Méthode** : « quoi qui a changé ? » → visite sur place → mesure avant/après.
Dans 80 % des cas, la cause est **physique** (câble, emplacement, cloison), pas
logicielle.

## 129. Cas n°9 — Un SSID a disparu

**Symptômes** : un des SSID n'est plus diffusé (les autres OK).

**Diagnostic** :

1. eKit : le SSID est-il toujours **activé** ? (Un collègue l'a-t-il désactivé
   « temporairement » il y a 3 mois ?)
2. Est-il restreint à une **plage horaire** ? (SSID invité coupé la nuit, c'est
   normal.)
3. Est-il lié à une **radio désactivée** ? (SSID 5 GHz only + radio 5 GHz en
   panne = SSID invisible.)
4. L'AP diffuse-t-il les autres SSID ? Non → problème AP global, pas SSID.

## 130. Cas n°10 — L'AP est « en ligne » mais personne ne s'y connecte

**Symptômes** : AP vert dans eKit, zéro client dessus, les voisins sont chargés.

**Causes** :

1. **SSID non appliqué** à cet AP (oubli dans le template / groupe).
2. **Radio désactivée** ou en panne (vérifier l'état radio).
3. **Canal DFS en attente** : l'AP a détecté un radar et attend (jusqu'à 10 min)
   — ou change de canal en boucle près d'un aéroport.
4. **Puissance trop faible** : l'AP chuchote, personne ne l'entend.
5. **VLAN** : les clients s'associent puis repartent faute de DHCP (voir §126) —
   l'AP paraît « vide ».

## 131. Cas n°11 — Débit asymétrique (down OK, up minable)

**Symptômes** : 200 Mbit/s en download, 5 Mbit/s en upload.

**Causes** :

1. **Liaison asymétrique** : l'AP arrose fort (puissance haute) mais le client
   (smartphone, faible puissance d'émission) ne « remonte » pas bien. →
   **Baisser la puissance de l'AP** (contre-intuitif mais efficace).
2. **Interférences** près du client (pas près de l'AP).
3. **Limite configurée** : rate-limiting asymétrique oublié (§100).
4. **Client** : économie d'énergie agressive qui bride l'émission.

## 132. Cas n°12 — Imprimante / IoT qui ne se connecte plus

**Symptômes** : l'objet connecté marchait, puis plus rien après un changement
(ou sans raison apparente).

**Checklist IoT** :

- [ ] L'objet est-il **2,4 GHz only** ? (La plupart.) Le SSID IoT est-il bien en
      2,4 GHz ? Le band steering ne le bloque-t-il pas ? (§52)
- [ ] Clé : caractères spéciaux non supportés par l'objet ? (Certains n'acceptent
      que l'alphanumérique.)
- [ ] WPA3 : l'objet ne connaît que WPA2 ? → SSID IoT en **WPA2-PSK**.
- [ ] DHCP : l'objet a-t-il une IP ? (Beaucoup d'IoT n'ont pas d'écran : sniffez
      le DHCP.)
- [ ] L'objet s'est-il mis à jour tout seul avec un firmware buggué ? (Ça arrive.)
- [ ] Redémarrage électrique de l'objet (le « have you tried turning it off and
      on again » s'applique cruellement bien aux IoT).

## 133. Cas n°13 — Lenteurs à heures fixes (12h-14h, 18h…)

**Symptômes** : ça rame tous les jours aux mêmes heures.

**Causes** :

1. **Micro-ondes** (12h-14h en 2,4 GHz) — le grand classique.
2. **Pic d'usage** : tout le monde en visio à 14h → saturation de la cellule
   (normal, à dimensionner).
3. **Sauvegardes** : un backup qui part à 18h et sature le lien ou l'airtime.
4. **Voisin** : le restaurant d'à côté allume son Wi-Fi le midi.
5. **Planification** : scans DCA ou upgrades planifiés aux mauvaises heures.

**Méthode** : corrélez avec l'agenda du bâtiment, pas seulement avec les logs.

## 134. Cas n°14 — Après une coupure de courant, le Wi-Fi ne revient pas

**Symptômes** : le courant est revenu, mais pas le Wi-Fi (ou partiellement).

**Diagnostic** :

1. Le **switch PoE** est-il revenu ? (S'il n'est pas sur onduleur et que son
   disjoncteur a sauté, les AP restent morts.)
2. Les AP **bootent-ils** ? (2-4 min par AP, ne paniquez pas à T+30 s.)
3. Le **DHCP/DNS/Internet** sont-ils revenus ? (L'AP sans cloud reste-t-il
   opérationnel ? — voir §43.)
4. Un AP ne revient pas : PoE ? (§124) Firmware corrompu par la coupure
   pendant un upgrade ? (§118).
5. **Ordre de démarrage** : le routeur/firewall doit être up **avant** les AP
   (sinon les AP bootent sans DHCP/DNS et restent dans un état bancal → prévoir
   un redémarrage PoE des AP après le retour du cœur réseau).

> 🔌 **Procédure de réalimentation** : cœur réseau (routeur, DHCP, DNS) → attendre
> 5 min → switch PoE → attendre 5 min → vérifier les AP. Automatisez ça avec des
> prises séquencées si les coupures sont fréquentes.

## 135. Cas n°15 — Le cloud eKit voit l'AP « hors ligne » mais le Wi-Fi marche

**Symptômes** : les utilisateurs sont contents, mais eKit dit l'AP hors ligne.

**Causes** :

1. L'AP a perdu l'accès **Internet** (le cloud) mais pas le LAN → le Wi-Fi local
   continue (selon version), la gestion est aveugle.
2. **DNS** en panne : l'AP ne résout plus les domaines Huawei.
3. **Horloge** : décalage NTP → TLS vers le cloud refusé.
4. **Firewall** : une règle a été ajoutée et bloque les flux sortants de l'AP.

**Danger** : sans supervision cloud, vous ne verrez pas la prochaine vraie panne.
Traitez comme un incident de supervision (priorité haute, pas critique).

## 136. Cas n°16 — Boucle / tempête broadcast via un AP

**Symptômes** : tout le réseau rame, switch saturé, même le filaire.

**Causes** :

1. Un **petit switch** branché sous l'AP (pour « avoir 2 prises ») avec un câble
   en boucle.
2. L'AP branché sur **deux ports** du switch (l'utilisateur a vu 2 prises…).
3. Un **rogue AP** en mode répéteur qui reboucle.

**Action immédiate** : débranchez le port suspect (vous retrouverez le calme en
quelques secondes), puis cherchez la boucle physiquement. **Prévention** :
`port-isolate` sur les ports AP (§75), **STP** actif sur le switch, et interdiction
affichée de brancher des petits switchs sauvages.

## 137. Arbre de décision rapide (à afficher dans la baie)

```
AP LED éteinte ?
├── Oui → PoE ? (testeur) → Non → câble/switch/budget (§124)
│                          → Oui → AP HS ? tester sur injecteur → RMA
└── Non (LED allumée)
    ├── SSID visible ?
    │   ├── Non → SSID activé ? radio OK ? canal DFS ? (§129, §130)
    │   └── Oui → association OK ?
    │       ├── Non → 1 client ou tous ? clé ? WPA3 ? pilote ? (§123)
    │       └── Oui → IP obtenue ?
    │           ├── Non (169.254.x.x) → DHCP/VLAN/trunk (§126)
    │           └── Oui → Internet OK ?
    │               ├── Non → routage/firewall/DNS
    │               └── Oui mais lent → port 100M ? interférences ?
    │                   charge ? (§121, §122)
    └── Wi-Fi OK mais eKit « hors ligne » → Internet/DNS/NTP/firewall (§135)
```

---
---

# O. MAINTENANCE PRÉVENTIVE

## 138. Plan de maintenance annuel (modèle)

| Période | Actions | Charge |
|---|---|---|
| **Mensuel** | Revue eKit (2 min/jour en fait), acquittement alarmes, vérification sauvegardes | 30 min |
| **Trimestriel** | Ronde radio à l'analyseur, dépoussiérage, contrôle fixations, revue des logs, rotation clé invités | ½ journée |
| **Semestriel** | Audit sécurité (rogue, PMF, comptes), test de la procédure « cloud injoignable », revue du plan radio | 1 journée |
| **Annuel** | Upgrade firmware de référence, audit complet, exercice de réalimentation (§134), révision documentaire, inventaire | 2-3 jours |

> 📅 Mettez ces créneaux **au planning d'équipe** en début d'année. La maintenance
> qui n'est pas planifiée n'existe pas.

## 139. Checklist trimestrielle (ronde terrain)

- [ ] **Visuel** : chaque AP est-il toujours à sa place, bien clipsé, LED normale ?
- [ ] **Poussière** : soufflette sur les ouïes (AP éteint si démontage, sinon
      soufflage léger sans démonter).
- [ ] **Fixations** : test de traction sur 10 % des AP (tous si < 10).
- [ ] **Câbles** : jarretières en baie non écrasées, étiquettes lisibles.
- [ ] **Radio** : tour à l'analyseur — nouveaux SSID voisins ? canaux toujours
      pertinents ? niveaux conformes à la mesure initiale ?
- [ ] **Logs** : revue des 3 derniers mois (reboots inexpliqués ? échecs d'asso
      en hausse ?).
- [ ] **Clé invités** : rotation effectuée ?
- [ ] **Spare** : l'AP de secours est-il toujours là, sur la bonne version ?

## 140. Le stock de spare : votre assurance panne

- **1 AP361 de spare** minimum par site de plus de 10 AP (ou 1 pour 20 AP sur
  des petits sites groupés).
- Le spare est **pré-configuré** (même version firmware, onboarding déjà fait
  ou procédure écrite) : en cas de panne, échange en 15 minutes.
- L'AP remplacé part en **RMA** ; le spare est remplacé dès le retour.
- Stockez-le dans son carton, au sec, avec son kit de fixation.

## 141. Nettoyage et inspection : les détails qui comptent

- **Soufflette** (air sec) sur les ouïes d'aération : la poussière isole
  thermiquement. En atelier/boulangerie : trimestriel obligatoire.
- **Ne jamais** : eau, solvant, nettoyeur haute pression (oui, on l'a déjà vu).
- **Dôme** : chiffon microfibre légèrement humide si traces.
- **Vérifiez les nids** : dans les entrepôts, les oiseaux et les toiles
  d'araignées adorent les AP chauds perchés.
- **Après travaux** : ronde systématique (peinture sur le dôme = atténuation ;
  AP déplacé par les peintres = repositionner + re-mesurer).

## 142. Revue documentaire annuelle

- [ ] Plan de masse à jour (déménagements, nouveaux AP) ?
- [ ] Tableau d'adressage à jour ?
- [ ] Plan radio toujours valable (nouveaux voisins, nouvelles cloisons) ?
- [ ] Mots de passe du coffre à jour ? Comptes des partants révoqués ?
- [ ] Release notes du firmware lues (failles corrigées depuis votre version ?)
- [ ] Contrats de garantie / RMA : dates de fin connues ?

## 143. Indicateurs de vieillissement : quand remplacer

Un AP361 n'est pas éternel. Signes de fin de vie :

- Reboots spontanés de plus en plus fréquents (hors cause réseau/PoE).
- Dégradation du débit radio sans cause externe (composants RF qui dérivent).
- Firmware **plus supporté** (plus de correctifs de sécurité) → remplacement
  planifié, pas en urgence.
- Besoin fonctionnel dépassé (densité qui explose, besoin 6 GHz) → montée en gamme.

**Durée de vie typique** : 5 à 7 ans en intérieur propre. Prévoyez le
renouvellement au **budget N+5** dès l'achat initial.

---
---

# P. DURCISSEMENT

## 144. Comptes et mots de passe : la base

- [ ] **Un compte par personne** (traçabilité). Pas de « admin » partagé.
- [ ] Mot de passe initial **changé jour 1** (ne jamais laisser le défaut).
- [ ] Robustesse : 20+ caractères, générés aléatoirement, au **coffre d'équipe**.
      Exemple **fictif** : `Grue-Torche-9917-Management!Bleu`.
- [ ] **Révocation immédiate** au départ d'un technicien/prestataire.
- [ ] Revue des comptes **semestrielle** : qui a accès ? pourquoi encore ?
- [ ] Verrouillage après N tentatives (si la fonction existe — 🔎 selon version)
      pour contrer le brute-force.

## 145. Management VLAN + restriction d'accès

- AP administrés sur le **VLAN 10 MGMT**, injoignable depuis les VLANs
  utilisateurs (règle firewall).
- Si l'AP le permet (🔎 selon version) : **ACL d'administration** n'autorisant
  que les IP des postes admin (ex. `192.168.10.100-110`).
- SSH : **jamais de Telnet** en production. SSH restreint au VLAN management.
- SNMP : v3 uniquement, communauté/credentials au coffre, restreint au superviseur.

## 146. HTTPS, certificats, chiffrement

- Interface web en **HTTPS uniquement** (désactiver HTTP si possible — 🔎).
- Certificat : le certificat auto-signé d'usine déclenche des alertes navigateur.
  Envisagez un certificat d'une **CA interne** (voir votre guide PKI) pour les
  équipements gérés en Fat.
- Vérifiez que les protocoles obsolètes sont désactivés (SSLv3, TLS 1.0/1.1 —
  🔎 selon ce que le firmware expose).

## 147. Désactiver les services inutiles

Principe : **tout ce qui n'est pas utilisé est désactivé**.

- [ ] Telnet : **désactivé** (SSH à la place, ou rien si cloud-only).
- [ ] HTTP : désactivé si HTTPS suffit.
- [ ] SNMP v1/v2c : désactivés si v3 en place (ou SNMP totalement si non utilisé).
- [ ] Découverte (LLDP/CDP) : utile en exploitation — à garder **sauf** exigence
      de discrétion, mais ne jamais diffuser d'infos sensibles dedans.
- [ ] Portail captif local : désactivé si non utilisé.

## 148. 802.1X sur le port filaire (si supporté)

Si le switch et l'AP le permettent : authentifier l'AP lui-même en 802.1X sur le
port du switch (le port ne s'ouvre que pour un AP connu). C'est le niveau
« paranoïaque sain » : quelqu'un qui débranche un AP pour brancher son PC se
retrouve avec un port mort.

🔎 Vérifiez le support côté AP361 et côté switch avant de promettre ça à la direction.

## 149. Journalisation des accès admin

- Toute connexion admin doit laisser une trace (syslog, §107).
- Revue **mensuelle** : connexions inhabituelles (nuit, week-end, IP inconnue) ?
- En cas d'incident : ces logs sont vos preuves. Sans logs, pas d'enquête.

## 150. Checklist de durcissement (audit semestriel)

- [ ] Comptes nominatifs, pas de défaut, pas de partagé
- [ ] Mots de passe robustes, au coffre, rotation annuelle minimum
- [ ] VLAN management isolé, ACL admin en place
- [ ] Telnet/HTTP/SNMP v1-v2c désactivés (ou justification écrite)
- [ ] HTTPS forcé, TLS récents
- [ ] Firmware à jour (failles connues corrigées)
- [ ] Logs d'accès admin centralisés et revus
- [ ] Comptes des prestataires/partants révoqués

## 151. En cas d'incident de sécurité : conduite à tenir

1. **Isoler** : déconnectez l'AP suspect du réseau (port du switch en shutdown)
   — ne l'éteignez pas tout de suite si vous voulez les logs volatils.
2. **Préserver** : exportez les logs (syslog, eKit) avant toute manipulation.
3. **Qualifier** : rogue AP ? Evil twin ? Compromission d'un compte admin ?
4. **Éradiquer** : reset + reconfiguration saine, rotation de **toutes** les clés,
   révocation des comptes suspects.
5. **Retour d'expérience** : fiche d'incident + mise à jour de ce guide.

---
---

# Q. PENSE-BÊTE DE POCHE

## 152. Les 10 commandes / actions qui sauvent

| # | Action | Où / comment |
|---|---|---|
| 1 | Voir l'état des AP | App eKit > Site > Équipements |
| 2 | Redémarrer un AP à distance | eKit > AP > Redémarrer (ou cycle PoE du port switch) |
| 3 | Voir la vitesse du port | eKit (état AP) ou `display interface` sur le switch |
| 4 | Changer un canal | eKit > Radio > canal manuel |
| 5 | Voir les clients d'un AP | eKit > AP > Clients |
| 6 | Sauvegarder la config (Fat) | `save` (+ copie vers serveur) |
| 7 | Voir les logs | Syslog central / eKit |
| 8 | Tester un câble | Testeur wiremap + longueur |
| 9 | Tester le PoE | Testeur PoE (tension 44-57 V) |
| 10 | Reset d'usine | Trombone > 5 s, attendre 2-4 min |

## 153. Valeurs à connaître par cœur

- AP361 : **2x2 ax, 575 + 1200 Mbit/s, 1 GE, 802.3af, 8,8 W**
- 2,4 GHz : canaux **1/6/11**, **20 MHz** toujours
- 5 GHz : **36-48** sans DFS, **40 MHz** par défaut
- Puissance de départ : **14 dBm**, cible **-60 dBm** en zone de travail
- Voix : **-67 dBm minimum**, **802.11r** activé
- PoE : **100 m max**, cuivre 100 %, budget = Σ + 20 %
- SSID : **max 4**, diffusés, clés 20+ caractères
- Un AP361 ≈ **40-50 clients** confortables

## 154. Numéros et contacts du site (à compléter)

| Rôle | Nom | Téléphone |
|---|---|---|
| Responsable IT | __________ | __________ |
| Astreinte réseau | __________ | __________ |
| Fournisseur / RMA Huawei | __________ | __________ |
| Électricien (baie) | __________ | __________ |
| Gardien / accès bâtiment | __________ | __________ |

## 155. Check-list « jour J » d'une intervention (à imprimer)

- [ ] Sac : testeur câble + testeur PoE + trombone + AP spare + étiqueteuse
- [ ] Téléphone chargé (app eKit + analyseur Wi-Fi)
- [ ] Accès : badge, clé du local, autorisation
- [ ] Consignation si intervention électrique en baie
- [ ] Photo « avant », photo « après »
- [ ] Test client complet après chaque modif (§31)
- [ ] Note au journal d'exploitation

## 156. Phrases à dire (et à ne pas dire)

- ✅ « Le signal est bon mais le débit est mauvais : je regarde le câble et les
      interférences. » (Ne confondez jamais signal et débit.)
- ✅ « Je change une variable à la fois et je note. »
- ⛔ « J'ai mis la puissance à fond, ça devrait mieux passer. » (Non.)
- ⛔ « C'est le Wi-Fi qui rame. » (Lequel ? Quel AP ? Quel client ? Quelle heure ?
      Mesurez d'abord.)

---
---

# R. GLOSSAIRE

## 157. Glossaire (les termes qui reviennent tout le temps)

| Terme | Définition courte |
|---|---|
| **802.11ax** | Norme Wi-Fi 6 : OFDMA, MU-MIMO montant/descendant, 1024-QAM, BSS Coloring |
| **802.11k** | Le client reçoit la liste des AP voisins → roaming plus rapide |
| **802.11v** | L'AP peut suggérer au client de changer d'AP (BSS Transition) |
| **802.11r** | Ré-authentification rapide lors du roaming (Fast BSS Transition) |
| **802.11w (PMF)** | Protection des trames de management contre la falsification |
| **AC (contrôleur)** | Équipement central pilotant des AP en mode Fit (pas le positionnement eKit) |
| **Airtime fairness** | Partage équitable du temps d'antenne entre clients |
| **AP** | Access Point — point d'accès |
| **Band steering** | Mécanisme poussant les clients vers le 5 GHz |
| **Beacon** | Trame émise ~10x/s par SSID annonçant le réseau |
| **BSS Coloring** | Marquage des trames (Wi-Fi 6) pour ignorer les transmissions voisines |
| **Captive portal** | Portail captif : page d'authentification avant accès Internet |
| **DCA** | Dynamic Channel Assignment — choix automatique du canal |
| **DFS** | Dynamic Frequency Selection — changement de canal obligatoire si radar détecté |
| **DSCP** | Marquage de QoS dans l'en-tête IP |
| **EAP** | Extensible Authentication Protocol — cadre d'authentification 802.1X |
| **EIRP** | Puissance isotrope rayonnée équivalente (puissance + gain d'antenne) |
| **Evil twin** | Faux AP diffusant votre SSID pour piéger les clients |
| **Fat / Fit** | Mode autonome / mode piloté par contrôleur |
| **GE** | Gigabit Ethernet (10/100/1000) |
| **MIMO** | Multiple-Input Multiple-Output — plusieurs flux spatiaux simultanés |
| **MU-MIMO** | MIMO multi-utilisateurs : l'AP parle à plusieurs clients à la fois |
| **OFDMA** | Découpage du canal en sous-porteuses allouées à plusieurs clients (Wi-Fi 6) |
| **OKC** | Opportunistic Key Caching — réutilisation de clé au roaming |
| **PVID** | Port VLAN ID — VLAN natif (non tagué) d'un port trunk |
| **QAM** | Modulation d'amplitude en quadrature (1024-QAM = plus de bits/symbole) |
| **RADIUS** | Serveur d'authentification (802.1X, WPA-Enterprise) |
| **RMA** | Return Merchandise Authorization — retour garantie |
| **Roaming** | Passage d'un client d'un AP à un autre en se déplaçant |
| **Rogue AP** | Point d'accès non autorisé |
| **RSSI** | Received Signal Strength Indicator — niveau de signal (dBm, négatif) |
| **SAE** | Simultaneous Authentication of Equals — handshake WPA3-Personal |
| **SSID** | Nom du réseau Wi-Fi diffusé |
| **Sticky client** | Client qui reste accroché à un AP lointain |
| **TPC** | Transmit Power Control — ajustement automatique de puissance |
| **Trunk** | Port transportant plusieurs VLAN tagués |
| **VAP** | Virtual AP — instance de SSID sur une radio |
| **VLAN** | Réseau local virtuel — séparation logique au niveau 2 |
| **WIDS / wIPS** | Détection / prévention d'intrusion sans fil |
| **WMM** | Wi-Fi Multimedia — QoS sans fil (4 files) |
| **WPA2 / WPA3** | Protocoles de sécurité Wi-Fi (PSK = clé partagée, EAP = entreprise) |

---
---

# S. QUIZ — 10 QUESTIONS

## 158. Questions (réfléchissez avant de regarder les réponses)

**Q1.** L'AP361 est alimenté en 802.3af et consomme 8,8 W max. Vous déployez
12 AP361 sur un switch PoE. Quel budget PoE minimum prévoyez-vous (avec 20 % de
marge), et quel palier commercial choisissez-vous ?

**Q2.** En 2,4 GHz, quels canaux utilisez-vous en planifié, et quelle largeur de
canal ? Pourquoi pas autre chose ?

**Q3.** Un client est associé au SSID Bureau mais obtient une adresse en
169.254.x.x. Citez 3 causes possibles, dans l'ordre de vérification.

**Q4.** Pourquoi ne faut-il pas mettre la puissance d'émission « à fond » ?
Citez au moins 3 effets négatifs.

**Q5.** Un utilisateur se plaint que ses appels Wi-Fi coupent quand il marche
dans les couloirs. Quelles sont les 3 vérifications prioritaires ?

**Q6.** Vous activez l'isolation client sur le SSID Bureau un vendredi soir.
Que risque-t-il de se passer lundi matin ? Sur quels SSID l'isolation est-elle
recommandée ?

**Q7.** Quelle est la différence entre 802.11k, 802.11v et 802.11r ? Lequel est
indispensable pour la voix sur Wi-Fi ?

**Q8.** Un AP est « en ligne » dans eKit mais aucun client ne s'y connecte alors
que l'AP voisin est saturé. Citez 4 causes possibles.

**Q9.** Pendant un upgrade firmware, que ne faut-il jamais faire ? Citez la
procédure de déploiement recommandée sur un parc (ordre des étapes).

**Q10.** Votre switch a un budget PoE de 185 W. Pouvez-vous y brancher 12 AP361
+ 4 caméras consommant 12 W chacune ? Détaillez le calcul.

## 159. Réponses

**R1.** 12 × 10 W (arrondi avec marge unitaire) = 120 W ; × 1,2 = **144 W
minimum**. Palier commercial : **185 W** (le palier 130-150 W est trop juste).
Et on vérifie que le budget PoE est bien la valeur « power budget », pas
24 ports × 15,4 W.

**R2.** Canaux **1, 6, 11** uniquement (les seuls non chevauchants en 20 MHz en
Europe), largeur **20 MHz toujours**. Le 40 MHz en 2,4 GHz occupe deux tiers de
la bande et génère des interférences avec les voisins ; tout autre canal
chevauche 1/6/11.

**R3.** (1) Le VLAN du SSID n'est pas autorisé sur le trunk du port AP
(`allow-pass`) ; (2) pas de serveur DHCP sur ce VLAN / relai DHCP oublié ;
(3) scope DHCP plein (baux trop longs). Ordre : d'abord tester en filaire sur le
même VLAN pour isoler Wi-Fi vs réseau.

**R4.** (1) L'AP arrose les voisins et l'étage → interférences ; (2) liaison
asymétrique : le client entend l'AP mais l'AP n'entend pas le client →
pertes ; (3) sticky clients : les terminaux restent accrochés de loin au lieu
de roamer. Bonus : gaspillage énergétique.

**R5.** (1) Couverture : mesurer en marchant, visez **-67 dBm minimum** partout ;
(2) **802.11r activé** (sans lui, ré-authentification lente à chaque changement
d'AP) ; (3) QoS voix (marquage EF + WMM) pour que l'appel ne soit pas noyé.
Vérifier aussi le sticky client (puissances équilibrées).

**R6.** Lundi : « l'imprimante réseau ne marche plus », « je ne vois plus les
PC du bureau » — l'isolation bloque les communications **entre clients** du
même SSID (partage Windows, impression locale, Chromecast). Elle est
recommandée sur **Invité** (toujours) et **IoT**, pas sur Bureau sans analyse
des usages.

**R7.** **k** : le client reçoit la liste des AP voisins (scan plus rapide) ;
**v** : l'AP suggère au client de migrer (BSS Transition) ; **r** : la
ré-authentification est accélérée lors du changement d'AP. Pour la voix,
c'est le **r (802.11r)** qui est indispensable (coupure < 100 ms).

**R8.** (1) SSID non appliqué à cet AP (oubli de template) ; (2) radio
désactivée ou en panne ; (3) canal DFS en attente de détection radar ;
(4) puissance trop faible ; (5) clients qui s'associent puis repartent faute de
DHCP sur le VLAN. (4 réponses suffisent.)

**R9.** **Ne jamais couper l'alimentation pendant l'écriture flash.**
Procédure : lire les release notes → sauvegarder la config → fenêtre hors
heures ouvrées → **1 AP pilote**, validation 24 h → généralisation **par
vagues** (jamais tout d'un coup) → vérifier l'homogénéité des versions →
conserver l'image N-1 deux semaines.

**R10.** 12 AP × 10 W = 120 W ; 4 caméras × 12 W = 48 W ; total = 168 W ;
× 1,2 = **201,6 W > 185 W** → **non**, ça ne passe pas avec la marge.
Solutions : switch à budget supérieur (250-370 W), ou répartir sur deux
switchs, ou accepter de fonctionner sans marge (déconseillé).

---
---

# T. POUR ALLER PLUS LOIN

## 160. Pour aller plus loin

**Documentation officielle (à récupérer)** :

- Datasheet officielle AP361 — ekit.huawei.com (vérifiez la révision hardware
  de votre lot)
- *WLAN Hardware Installation and Maintenance Guide* — chapitre AP361 (LED,
  reset, specs détaillées, couples de serrage)
- *Configuration Guide* eKit de votre version logicielle + release notes
- Guide de démarrage rapide fourni dans le carton (QR code)

**Dans votre bibliothèque (déjà livrés)** :

- `onduleurs_ups_guide.md` — dimensionner l'onduleur de la baie PoE (§23)
- `proxmox_guide.md` — héberger le serveur syslog / RADIUS / Zabbix en VM
- `zabbix_guide.md` — template de supervision §109
- `debian_ubuntu_guide.md` — serveur RADIUS (FreeRADIUS), syslog, scripts
- `ssh_guide.md` — durcir l'accès SSH §145
- `loki_guide.md` / `prometheus_guide.md` / `grafana_guide.md` — supervision avancée

**Sujets d'approfondissement** :

1. **Étude de couverture (site survey)** : avec un logiciel de survey
   (Ekahau, NetSpot…), passez du « au pif » au dimensionné — indispensable
   au-delà de 20 AP ou en environnement contraint (hôpital, entrepôt).
2. **802.1X + PKI interne** : EAP-TLS avec votre CA (voir guide Debian/Ubuntu)
   pour un contrôle d'accès sans mot de passe partagé.
3. **NAC** : coupler le RADIUS à une politique d'accès dynamique (VLAN
   assigné par utilisateur).
4. **Wi-Fi 6E / Wi-Fi 7** : quand la densité ou les usages (AR/VR, vidéo 4K
   massive) l'exigeront — l'AP361 est Wi-Fi 6 2,4/5 GHz, pas 6 GHz.
5. **Analyse spectrale** : un analyseur de spectre (même USB à ~300 €) pour
   traquer les interférences non-Wi-Fi que l'analyseur logiciel ne voit pas.

**Dernier mot terrain** : un réseau Wi-Fi, c'est 20 % d'achat et 80 % de
méthode — plan radio écrit, étiquetage, mesures, documentation, maintenance.
L'AP361 fait correctement son travail si **vous** faites correctement le vôtre.
Bon chantier. 🔧

---

*Fin du guide — Huawei eKit AP361 — rédigé le 2026-09-27 pour Zelef.*
*Vérifiez les points marqués 🔎 sur la fiche du modèle exact avant mise en production.*
