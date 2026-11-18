# Guide ultra-complet — Huawei eKit S310
## Switch manageable PME : installation, configuration, exploitation et dépannage

> **Public visé :** techniciens et responsables systèmes & réseaux en PME / multi-sites.
> **Ton :** terrain, concret, direct. Chaque section donne le pourquoi, le comment (CLI VRP + web),
> et le piège à éviter.
> **Périmètre :** gamme Huawei eKit S310 (eKitEngine S310) — L2+ manageable, gestion cloud eKit,
> web, CLI, SNMP. Les valeurs constructeur citées viennent de la fiche officielle de la série
> (datasheet eKitEngine S310). Quand une valeur dépend du modèle exact ou de la version logicielle,
> c'est écrit noir sur blanc : **« à vérifier sur la fiche du modèle exact »**.
>
> **Règle d'or de ce guide :** les mots de passe et clés figurant dans les exemples sont
> **fictifs** (ex. `Exemple_MotDePasse_2026!`). Ne jamais les utiliser en production.

**Sources constructeur consultées (recherche web, septembre 2026) :**
- Fiche produit officielle : `https://ekit.huawei.com/ekit/front/ssr/en/product/1239139538459211136` (S310-48P4S)
- Datasheet série eKitEngine S310 (PDF) : `https://i.pcdeacitec.com/datasheets/syscom/S310-24U4X/f2112277465062598bb47bdc547534d7.pdf`
- Datasheet miroir : `https://resource.ewe.rs/media/documents/2026/08/2026-08-07573726.pdf`
- Fiche S310-24T4S (datasheet PDF) : `https://shop.aodatacloud.es/medias/datasheet_s310-24t4s.pdf`

---

## 1. À qui s'adresse ce guide (et comment l'utiliser)

1. **Technicien de terrain** : tu déballes, tu montes en rack, tu câbles, tu fais la première
   configuration. Va directement aux sections 14 à 29, puis au pense-bête (section 119).
2. **Administrateur réseau PME** : VLAN, STP, PoE, QoS, sécurité. Cœur du guide : sections 30 à 95.
3. **Responsable / chef de service** : dimensionnement PoE (sections 66 à 70), maintenance
   préventive (sections 113 à 116), durcissement (sections 117 à 118), pièces de rechange.
4. **Conventions de lecture** :
   - Les blocs ` ``` ` sont des commandes CLI VRP Huawei à taper en mode système (`system-view`).
   - `[web]` signale l'équivalent dans l'interface web quand il existe.
   - ⚠️ = piège classique de terrain. ✅ = bonne pratique. 🔧 = commande de vérification.

---

## 2. La gamme eKit S310 en un coup d'œil

La série eKitEngine S310 est la gamme de switches manageables d'entrée/milieu de gamme Huawei
pour les PME, pilotable en cloud via l'app **Huawei eKit**, en web local, en CLI (VRP) ou en SNMP.
Tous les modèles sont **L2+** (commutation L2 + routage statique de base), 1U, châssis métal,
alimentation AC intégrée.

| Modèle | Ports cuivre | PoE | Uplinks | Capacité | Débit | PoE budget |
|---|---|---|---|---|---|---|
| S310-24T4S | 24× GE RJ45 | Non | 4× GE SFP | 56 Gbit/s | 42 Mpps | — |
| S310-24P4S | 24× GE RJ45 | PoE+ | 4× GE SFP | 56 Gbit/s | 42 Mpps | **400 W** |
| S310-24T4X | 24× GE RJ45 | Non | 4× 10GE SFP+ | 128 Gbit/s | 96 Mpps | — |
| S310-24P4X | 24× GE RJ45 | PoE+ | 4× 10GE SFP+ | 128 Gbit/s | 96 Mpps | **400 W** |
| S310-24PN4X | 24× 2,5GE RJ45 | PoE+ | 4× 10GE SFP+ | 200 Gbit/s | 144 Mpps | **400 W** |
| S310-24ST4X | 24× GE SFP (8 combo) | Non | 4× 10GE SFP+ | 128 Gbit/s | 96 Mpps | — |
| S310-48T4X | 48× GE RJ45 | Non | 4× 10GE SFP+ | 176 Gbit/s | 131 Mpps | — |
| S310-48P4S | 48× GE RJ45 | PoE+ | 4× GE SFP | 104 Gbit/s | 77 Mpps | **380 W** |
| S310-48P4X | 48× GE RJ45 | PoE+ | 4× 10GE SFP+ | à vérifier sur la fiche du modèle exact | à vérifier | à vérifier |

> ⚠️ Les références **48P4X / 48T4X** existent dans la gamme mais leurs valeurs détaillées
> (bruit, poids, conso exacte) sont **à vérifier sur la fiche du modèle exact** avant tout
> dimensionnement électrique ou thermique.

**Logique de nommage (à retenir pour commander sans se tromper) :**

| Lettre | Signification |
|---|---|
| 24 / 48 | Nombre de ports d'accès |
| T | Ports cuivre RJ45 **sans** PoE (ex. 24T4S) |
| P | Ports cuivre RJ45 **avec PoE+** (ex. 24P4S) |
| 4S | 4 uplinks **GE SFP** (1 Gbit/s) |
| 4X | 4 uplinks **10GE SFP+** (10 Gbit/s) |
| N | Ports d'accès **2,5GE** (ex. 24PN4X : 24 ports 2,5G PoE+) |
| ST | Ports d'accès **SFP fibre** (ex. 24ST4X) |

---

## 3. S310-24T4S — fiche technique détaillée (valeurs constructeur)

Le modèle de référence « sans PoE », idéal comme switch d'accès bureautique ou d'agrégation
cuivre quand l'alimentation des équipements est déjà gérée (ou inutile).

| Caractéristique | Valeur constructeur |
|---|---|
| Référence | 98012202 |
| Ports fixes | 24× 10/100/1000BASE-T + 4× GE SFP |
| Capacité de commutation | 56 Gbit/s |
| Débit de transfert | 42 Mpps |
| Table MAC | 16 000 entrées (valeur catalogue distributeur, à vérifier sur la fiche du modèle exact) |
| VLAN max | 4 094 |
| Dimensions (H×L×P) | 43,6 × 442 × 220 mm — 1U |
| Poids (avec emballage) | 3,44 kg |
| Alimentation | AC intégrée, 100–240 V AC 50/60 Hz (plage max 90–290 V AC, 45–65 Hz) |
| Consommation max | **34,04 W** |
| Bruit (puissance acoustique) | 47 dB(A) à température normale / 51 dB(A) à haute température |
| Pression acoustique | 35 dB(A) à température normale |
| Température de fonctionnement | –5 °C à +50 °C (0–1 800 m ; au-delà, –1 °C par tranche de 220 m jusqu'à 5 000 m) |
| Humidité | 5 % à 95 % HR, sans condensation |
| Refroidissement | Air, vitesse des ventilateurs intelligente |
| Protection surtension | ±6 kV (mode différentiel et mode commun) |
| Montage | Rack 19", bureau, mural |
| Management | Cloud eKit (NETCONF/YANG), web, CLI, SNMPv1/v2c/v3, SSH v2.0, Telnet |
| Stacking | iStack (jusqu'à 4 unités de la même série) |

✅ **Bon réflexe d'achat :** si tu n'as besoin d'aucun PoE aujourd'hui mais que des AP ou
caméras sont prévus dans 2 ans, prends directement le **24P4S**. Le surcoût est inférieur au
coût d'un remplacement plus un injecteur PoE par équipement.

---

## 4. S310-24P4S — fiche technique (le modèle PoE+ 24 ports)

Le cheval de bataille des PME : 24 ports PoE+ pour AP Wi-Fi, caméras, téléphones IP,
plus 4 SFP pour la fibre vers le cœur.

| Caractéristique | Valeur constructeur |
|---|---|
| Ports fixes | 24× 10/100/1000BASE-T **PoE+** + 4× GE SFP |
| Capacité / débit | 56 Gbit/s / 42 Mpps |
| Budget PoE total | **400 W** |
| Standard PoE | IEEE 802.3af/at (PoE/PoE+, 30 W max par port) |
| Consommation max | **491,66 W** à pleine charge PoE / 47,1 W sans PoE |
| Poids (avec emballage) | 3,79 kg |
| Bruit | 49,3 dB(A) normal / **63 dB(A)** à haute température |
| Pression acoustique | 37,3 dB(A) à température normale |
| Autres | Identique au 24T4S (dimensions, température, ±6 kV, iStack, management) |
| Fonctions PoE avancées | **Fast PoE** (alimentation des PD en quelques secondes après mise sous tension) et **Perpetual PoE** (le PoE reste alimenté pendant un redémarrage / une mise à jour logicielle) |

⚠️ **63 dB(A) à haute température**, c'est le niveau sonore d'une conversation forte :
ne mets pas un 24P4S chargé en PoE dans un bureau occupé sans réfléchir. Prévois un local
technique ou un placard ventilé (voir section 13).

---

## 5. Les versions 10G : S310-24T4X et S310-24P4X

Quand les uplinks 1G deviennent le goulot (NAS, serveurs, inter-sites, Wi-Fi 6 dense),
les modèles **X** apportent 4× 10GE SFP+.

| Caractéristique | S310-24T4X | S310-24P4X |
|---|---|---|
| Ports | 24× GE (sans PoE) | 24× GE **PoE+** |
| Uplinks | 4× 10GE SFP+ | 4× 10GE SFP+ |
| Capacité / débit | 128 Gbit/s / 96 Mpps | 128 Gbit/s / 96 Mpps |
| Budget PoE | — | **400 W** |
| Conso max | 35,04 W | à vérifier sur la fiche du modèle exact |
| Bruit | 47 dB(A) / 51 dB(A) | à vérifier sur la fiche du modèle exact |

🔧 **Règle de dimensionnement uplink :** additionne les débits crête réalistes des ports
d'accès, applique un ratio de sursouscription de 4:1 à 8:1 en bureautique. 24 ports GE
avec 20 postes actifs à ~50 Mbit/s crête = ~1 Gbit/s → un uplink 1G suffit ; avec un NAS
ou 3 AP Wi-Fi 6, passe en 10G (modèle X) ou en Eth-Trunk (section 55).

---

## 6. Les 48 ports : S310-48P4S (et 48T4X / 48P4X)

| Caractéristique | S310-48P4S (valeurs constructeur) |
|---|---|
| Référence | 98012384 |
| Ports | 48× 10/100/1000BASE-T **PoE+** + 4× GE SFP |
| Capacité / débit | 104 Gbit/s / 77 Mpps |
| Budget PoE | **380 W** |
| Conso | 34,04 W (statique) / 48,64 W (typique) / 63,7 W (max sans PoE) / **462,8 W** (max pleine charge PoE) |
| Ventilateurs | 2, intégrés, vitesse intelligente — flux d'air **de gauche vers la droite** |
| Dimensions | 43,6 × 442 × 220 mm (227 mm avec les parties saillantes) — 1U |
| Poids | 3,24 kg (4,29 kg avec emballage) |
| Montage | Rack, bureau, **mural** |
| Température / humidité | –5 °C à +50 °C / 5–95 % HR sans condensation |
| Alimentation | AC intégrée 90–290 V AC (45–65 Hz) ; entrée DC haute tension 190–290 V DC supportée |
| Protection surtension | ±6 kV |

⚠️ **380 W pour 48 ports** : c'est moins que 24P4S en proportion (7,9 W/port en moyenne).
Tu ne peux pas mettre 48 équipements PoE+ à 15 W. La planification PoE (sections 66–70)
est **obligatoire** sur ce modèle. Priorités PoE à configurer dès le jour 1.

---

## 7. Modèles spécifiques : S310-24PN4X et S310-24ST4X

- **S310-24PN4X** : 24× **2,5GE** RJ45 PoE+ (2,5G/1G/100M/10M) + 4× 10GE SFP+. Capacité
  **200 Gbit/s**, 144 Mpps, PoE **400 W**. Le choix naturel pour des **AP Wi-Fi 6/6E**
  dont le débit réel dépasse 1 Gbit/s. Température de fonctionnement limitée à
  **–5 °C à +45 °C** (et non +50 °C) — à vérifier sur la fiche du modèle exact si ton
  local est chaud.
- **S310-24ST4X** : 24× **GE SFP** (dont 8 ports **combo** 10/100/1000BASE-T ou SFP) +
  4× 10GE SFP+. 128 Gbit/s, 96 Mpps. Pour les répartiteurs tout-fibre ou les environnements
  perturbés (ateliers, liaisons inter-bâtiments).
- **S310-24U4X** (vu dans certains datasheets) : variante PoE++ — **à vérifier sur la
  fiche du modèle exact**, ne pas confondre avec le 24P4X.

✅ **Combo, rappel :** sur un port combo, le cuivre **ou** la fibre est actif, jamais les
deux en même temps. La fibre est généralement prioritaire quand un SFP est inséré
(comportement à vérifier sur la version logicielle exacte).

---

## 8. Face avant : ports, LED, console — savoir lire le switch

De gauche à droite sur un S310-24Tx/24Px typique :

1. **24 ports RJ45 GE** (1 à 24), avec 2 LED par port (Link/Act).
2. **4 cages SFP/SFP+** (25 à 28), LED dédiées.
3. **Port console RJ45** : pour le câble console (première config, secours).
4. **Bouton MODE** : change le mode d'affichage des LED (à vérifier sur le modèle exact).
5. **Bouton RST** : reset (appui court = reboot, appui long ~5–10 s = retour usine —
   ⚠️ à vérifier sur le modèle exact, un appui long efface la configuration).
6. **LED système** : PWR (alimentation), SYS (état système).
7. Sur les modèles PoE : LED **PoE** par port ou globale.

Face arrière : **vis de terre**, **passage pour la sangle du cordon secteur**,
**prise AC** (alimentation intégrée, pas de module amovible sur cette gamme).

🔧 **Réflexe terrain :** avant de toucher à quoi que ce soit, photographie la face avant
(LED allumées) et note les ports occupés. En cas de panne, tu compares.

---

## 9. Les LED : signification complète

| LED | État | Signification |
|---|---|---|
| PWR | Vert fixe | Alimentation OK |
| PWR | Éteinte | Pas d'alimentation — vérifier cordon, prise, disjoncteur |
| PWR | Rouge / orange | Défaut d'alimentation — à vérifier sur le modèle exact |
| SYS | Vert fixe | Système démarré et fonctionnel |
| SYS | Vert clignotant | Démarrage en cours |
| SYS | Rouge fixe/clignotant | Défaut système — consulter les logs, puis support |
| Port Link/Act | Vert fixe | Lien établi (link up) |
| Port Link/Act | Vert clignotant | Activité (émission/réception) |
| Port Link/Act | Éteinte | Pas de lien — câble, carte réseau distante, port désactivé |
| Port Link/Act | Orange (si dispo) | Lien à 10/100 Mbit/s ou défaut — à vérifier sur le modèle exact |
| PoE (par port) | Vert fixe | PD alimenté en PoE |
| PoE (par port) | Clignotant / orange | Négociation ou défaut PoE (surcharge, court-circuit, PD incompatible) |
| SFP | Vert fixe | Module inséré et lien OK |
| SFP | Éteinte | Pas de module ou pas de lien |

> Les couleurs exactes et les modes du bouton MODE sont **à vérifier sur le guide
> matériel du modèle exact** : Huawei a des variantes selon les séries. En cas de doute,
> la CLI (`display interface brief`, `display poe`) dit toujours la vérité.

---

## 10. Première mise sous tension : ce qui est normal (et ce qui ne l'est pas)

1. Branche le cordon secteur **avec la sangle de maintien** (elle évite les
   débranchements accidentels — un classique en baie).
2. Le ventilateur tourne à plein régime quelques secondes, puis ralentit
   (vitesse intelligente). ✅ Normal.
3. La LED SYS clignote pendant le boot (~1 à 2 minutes sur cette gamme, ordre de
   grandeur à vérifier), puis passe au vert fixe. ✅ Normal.
4. Sur un modèle PoE, les PD sont alimentés en **quelques secondes** (Fast PoE),
   sans attendre la fin du boot. ✅ Normal — ne t'inquiète pas si une caméra
   s'allume « avant » le switch.
5. ⚠️ **Anormal :** SYS rouge, ventilateur à fond en permanence après 5 minutes,
   odeur de chaud, LED PWR éteinte → coupe l'alimentation et vérifie
   l'environnement (section 13) avant de re-tester.

---

## 11. Alimentation électrique : caractéristiques et dimensionnement

- **Entrée :** 100–240 V AC, 50/60 Hz (plage de fonctionnement 90–290 V AC, 45–65 Hz).
  Le switch accepte donc les réseaux 110 V comme 230 V sans sélecteur.
- **Protection :** ±6 kV en mode différentiel et commun (parafoudre intégré de base —
  ça ne remplace pas un parafoudre T1+T2 en tête d'installation, voir section 114).
- **Dimensionnement du circuit :**

| Modèle | Conso max à prévoir | Disjoncteur conseillé | Commentaire |
|---|---|---|---|
| 24T4S / 24T4X | ~35 W | 10 A courbe C | Négligeable, mais dédié quand même |
| 24P4S / 24P4X | ~492 W (pleine charge PoE) | 10 A courbe C | Prévoir la charge PoE réelle, pas le max théorique |
| 48P4S | ~463 W (pleine charge PoE) | 10 A courbe C | Idem |

- ✅ Chaque switch sur un **départ protégé identifié** au tableau (« Baie — SW1 »).
- ✅ En environnement critique : **onduleur** en amont (voir le guide onduleurs de
  Zelef : un 24P4S pleine charge ≈ 500 W → prévoir ~1 kVA d'UPS pour 30+ min
  d'autonomie selon la batterie).
- ⚠️ Ne jamais brancher le switch sur une prise commandée par interrupteur mural.

---

## 12. Ventilation, bruit, température : où poser le switch

| Modèle | Bruit normal | Bruit haute temp. | Conséquence pratique |
|---|---|---|---|
| 24T4S / 24T4X | 47 dB(A) | 51 dB(A) | Acceptable en local technique, limite en open space |
| 24P4S | 49,3 dB(A) | **63 dB(A)** | Local technique recommandé si PoE chargé |
| 48P4S | à vérifier | à vérifier | 2 ventilateurs — prévoir dégagement gauche/droite (flux d'air latéral) |

**Règles d'installation thermique :**

1. Dégagement minimum : 10 cm à l'avant (câbles), 10 cm sur les côtés pour les
   modèles à flux latéral (48P4S : entrée d'air à gauche, sortie à droite).
2. Ne jamais empiler directement deux switches sans espace ni les coincer entre
   des classeurs : l'air doit circuler.
3. Température ambiante cible : **< 35 °C** en continu. Au-delà de 40 °C, les
   ventilateurs tournent à fond (bruit) et la durée de vie des condensateurs chute.
4. En baie : 1U de vide ou panneau de brassage ventilé entre deux switches PoE
   chargés si la baie est dense.
5. ⚠️ Poussière (ateliers, BTP, menuiserie) : le S310 est **IP20**, sans filtre.
   Prévois un nettoyage à l'air comprimé (bombe, pas de compresseur d'atelier
   humide) lors de la maintenance (section 114), ou un coffret étanche ventilé.

---

## 13. Déballage : contenu du carton et checklist de réception

**Contenu typique (à vérifier sur le bordereau du modèle exact) :**

- [ ] Le switch S310 (référence conforme au bon de commande : T/P, S/X, 24/48)
- [ ] Kit d'oreilles rack 19" + visserie
- [ ] Cordon secteur (avec sangle de maintien sur certains modèles)
- [ ] Câble console (RJ45 vers série/USB selon version — à vérifier)
- [ ] Pieds adhésifs pour pose bureau
- [ ] Guide de démarrage rapide / carte de garantie

**Checklist de réception (5 minutes qui évitent des semaines de galère) :**

- [ ] Référence sur l'étiquette = référence commandée (un 24T4S au lieu d'un 24P4S,
      ça arrive, et ça se voit **après** le câblage PoE).
- [ ] Numéro de série noté et archivé (photo de l'étiquette) — indispensable pour
      le support et la garantie.
- [ ] Version logicielle affichée au boot notée (pour planifier la mise à jour,
      section 102).
- [ ] Aucun choc sur le châssis, ventilateurs qui tournent librement (test à
      l'oreille au premier démarrage).
- [ ] Test de base : 2 PC en DHCP ou IP fixe sur 2 ports → ping OK avant montage.

🔧 **Astuce stock :** garde un carton d'origine pour chaque switch en spare.
Un retour SAV sans emballage d'origine, c'est un risque de casse refusée.

---

## 14. Montage en rack 19" : procédure pas à pas

1. Fixe les **oreilles rack** de chaque côté avec les vis fournies (ne force pas :
   un filetage foiré = oreille qui ne tient plus).
2. Présente le switch dans la baie à la hauteur prévue (prévois le brassage :
   panneau de brassage **juste au-dessus** ou **juste en dessous**).
3. Visse les 4 vis cage (2 par côté) sans serrer à fond, ajuste l'alignement,
   puis serre en croix.
4. ✅ Le switch 3,2–3,8 kg ne nécessite pas de rails, mais en baie profonde
   (> 80 cm) des **étagères 1U** ou rails légers évitent le porte-à-faux.
5. Passe la **terre** (section 15) **avant** le cordon secteur.
6. Câble les ports (section 16), puis alimente (section 10).

**Plan de baie type PME (exemple) :**

| U | Équipement |
|---|---|
| 42 | Panneau de brassage 24 ports (arrivées postes) |
| 41 | S310-24P4S (SW-ACC-01) |
| 40 | 1U vide (ventilation) ou second S310 |
| 39 | Onduleur / PDU |

---

## 15. Mise à la terre : pourquoi et comment (ne la saute pas)

La vis de terre en face arrière n'est pas décorative : avec la protection ±6 kV et
des liaisons cuivre longues, une terre correcte évite les reboots fantômes et les
ports qui « tombent » pendant les orages.

1. Utilise un fil **vert/jaune ≥ 2,5 mm²** (6 mm² si la baie est loin de la barrette).
2. Visse sur la borne de terre du switch, l'autre extrémité sur la **barrette de
   terre de la baie**, elle-même reliée à la terre du bâtiment.
3. Serre correctement : une cosse qui bouge = une terre qui ne sert à rien.
4. 🔧 Vérifie à l'ohmmètre si tu as un doute (continuité borne ↔ barrette).
5. ⚠️ Ne jamais utiliser le neutre ou une tuyauterie comme terre.

---

## 16. Câblage : choisir et poser les bons câbles

**Cuivre (ports RJ45 GE) :**

| Usage | Câble conseillé |
|---|---|
| Postes bureautiques, téléphones IP | Cat 6 (ou Cat 6A si neuf — pérenne 10G) |
| AP Wi-Fi 6 (surtout 24PN4X en 2,5G) | **Cat 6A minimum** pour le 2,5G/5G propre |
| Caméras PoE extérieures | Cat 6 extérieur (gaine UV) ou fibre + convertisseur |
| Jarretières en baie | 0,5–1 m, Cat 6, de couleur par usage (voir code couleur) |

**Code couleur suggéré (à afficher dans la baie) :**

| Couleur | Usage |
|---|---|
| Bleu | Postes utilisateurs |
| Jaune | AP Wi-Fi |
| Rouge | Uplinks / inter-switch |
| Vert | Téléphonie IP |
| Gris/noir | Serveurs / imprimantes |
| Orange | Caméras |

**Fibre (uplinks SFP/SFP+) :**

- SFP GE : modules **1000BASE-SX** (multimode, ≤ 550 m) ou **1000BASE-LX** (monomode, ≤ 10 km).
- SFP+ 10G : **10GBASE-SR** (multimode OM3/OM4) ou **10GBASE-LR** (monomode).
- ⚠️ N'achète que des modules **compatibles Huawei / codés Huawei** ou des compatibles
  tiers de qualité pro : un SFP non reconnu = uplink mort (voir dépannage, section 124).
- Nettoie les connecteurs (stylo de nettoyage fibre) avant insertion. Un connecteur
  sale = 3 dB de perte = lien instable.

**Règles de brassage :**

1. Longueur max d'un lien cuivre : **100 m** (90 m de câble fixe + 10 m de jarretières).
2. Sépare les chemins cuivre et 230 V (30 cm minimum en parcours parallèle).
3. Étiquette **les deux extrémités** de chaque câble (ex. `BRAS-12 ↔ SW1-P07`).
4. Laisse du mou (boucle de service) derrière la baie, jamais de câble tendu.
5. ✅ Photographie le brassage final : c'est ta doc « plan de câblage » v1.

---

## 17. Les 4 modes de management : choisir le bon

| Mode | Quand l'utiliser | Prérequis |
|---|---|---|
| **Cloud eKit** (app + portail) | Multi-sites, PME sans admin réseau à temps plein, supervision centralisée | Accès Internet sur le switch, compte Huawei eKit |
| **Web local** (HTTP/HTTPS) | Site unique, configuration ponctuelle, technicien sur place | IP de management joignable |
| **CLI** (console/SSH) | Scripts, dépannage avancé, tout ce que le web ne fait pas | Câble console ou SSH activé |
| **SNMP** | Supervision (Zabbix, PRTG, Centreon...), pas de configuration | SNMP activé + superviseur |

✅ **En pratique PME :** onboarding en **cloud eKit** pour le déploiement (rapide, guidé),
puis **CLI/SSH** pour le réglage fin et le dépannage. Les trois modes coexistent :
le cloud ne verrouille pas le CLI.

> Le cloud eKit utilise **NETCONF/YANG** en sous-jacent (valeur datasheet). Les deux
> modes « cloud » et « on-premise » sont commutables selon le besoin.

---

## 18. Branchement console : paramètres et matériel

Le port console RJ45 sert à la première configuration et au **secours** (quand le réseau
est en vrac, la console répond toujours).

**Paramètres série (standard Huawei) :**

| Paramètre | Valeur |
|---|---|
| Débit | 9 600 bit/s |
| Bits de données | 8 |
| Parité | Aucune |
| Bits d'arrêt | 1 |
| Contrôle de flux | Aucun |

**Matériel :**

- Câble console RJ45 ↔ USB (la plupart des PC récents n'ont plus de port série DB9).
  Installe le pilote du chipset (Prolific PL2303 / FTDI / CH340 selon le câble).
- Logiciels : PuTTY, Tera Term, ou `screen /dev/ttyUSB0 9600` sous Linux.

🔧 **Test rapide :** allume le switch avec la console branchée : tu dois voir les
messages de boot défiler. Si rien ne s'affiche : mauvais port COM, mauvais débit,
ou câble console HS (en avoir 2 en sacoche, c'est du vécu).

---

## 19. Première connexion console : pas à pas

1. Branche la console, ouvre le terminal à 9 600 8N1.
2. Allume le switch, attends la fin du boot.
3. Appuie sur **Entrée** : le switch affiche une invite de login (ou directement
   l'invite `<>` selon la version).
4. **Mot de passe initial :** sur les versions récentes, le switch impose de
   **créer un mot de passe admin au premier démarrage** (politique de sécurité
   Huawei). L'identifiant et la procédure exacte sont **à vérifier sur le guide
   de démarrage rapide du modèle exact** (étiquette / documentation fournie).
5. ⚠️ Choisis immédiatement un mot de passe **fort et unique par site**
   (12+ caractères, documenté dans le coffre de l'entreprise — voir section 117).
   Un switch avec mot de passe par défaut sur le réseau = porte ouverte.

**Les 3 niveaux d'invite VRP (à connaître par cœur) :**

```
<HUAWEI>            ← vue utilisateur : commandes d'affichage (display ...)
[HUAWEI]            ← vue système (après "system-view") : configuration
[HUAWEI-GigabitEthernet0/0/1]  ← vue d'interface : config d'un port
```

🔧 Commandes de survie immédiates :

```
display version              # version logicielle et matérielle
display current-configuration # config active
display interface brief      # état de tous les ports
save                         # SAUVEGARDER la config (sinon perdue au reboot !)
```

---

## 20. Configuration de l'IP de management (VLANIF)

Sans IP de management, pas de web, pas de SSH, pas de SNMP. La méthode Huawei :
une interface virtuelle **VLANIF** rattachée au VLAN de management.

**Exemple : VLAN 99 = management, réseau 192.168.99.0/24, switch en .10 :**

```
system-view
vlan 99
 description VLAN_MANAGEMENT
quit
interface Vlanif99
 ip address 192.168.99.10 255.255.255.0
quit
ip route-static 0.0.0.0 0.0.0.0 192.168.99.1   # passerelle par défaut
quit
save
```

Puis, sur un port d'accès au VLAN 99 (ex. port 24 réservé à l'admin) :

```
system-view
interface GigabitEthernet0/0/24
 port link-type access
 port default vlan 99
 description PORT_ADMIN
quit
save
```

[web] Équivalent : *Configuration → VLAN → créer VLAN 99*, puis *Interface VLANIF*,
puis *Routage → route statique*.

✅ **Bonnes pratiques :**
- VLAN de management **dédié** (pas le VLAN 1, pas le VLAN utilisateurs).
- Documente l'IP de chaque switch dans un **tableau d'adressage** (section 119).
- La passerelle par défaut est indispensable pour le cloud eKit et le syslog distant.

⚠️ **Le VLAN 1** est le VLAN natif par défaut sur tous les ports : ne jamais y mettre
le management en production (tout équipement branché y aurait accès).

---

## 21. Accès web (HTTP/HTTPS) : activation et premier login

```
system-view
http server enable            # active le serveur web
http secure-server enable     # active HTTPS (recommandé)
quit
save
```

Puis dans un navigateur : `https://192.168.99.10` (accepte le certificat
auto-signé — normal sur un équipement neuf).

**Premier login :** identifiant/mot de passe = ceux définis à la première connexion
(section 19). La procédure exacte et les identifiants initiaux sont
**à vérifier sur la documentation du modèle exact**.

[web] Une fois connecté, l'assistant de configuration initiale guide pour :
hostname, IP de management, mot de passe admin, date/heure.

⚠️ **HTTPS partout :** désactive HTTP pur dès que HTTPS fonctionne
(`undo http server enable`). Un mot de passe admin en clair sur le réseau,
même en interne, c'est non (voir durcissement, section 117).

---

## 22. L'app Huawei eKit : onboarding cloud pas à pas

Le mode cloud permet de **déployer sans toucher au CLI** : le switch récupère sa
configuration depuis le cloud après simple branchement (plug-and-play).

**Prérequis :**
- Compte sur la plateforme **Huawei eKit** (créé par le revendeur ou l'admin).
- Le switch a accès à Internet (passerelle par défaut configurée, section 20,
  + DNS si besoin).
- Numéro de série du switch (photo de l'étiquette, section 13).

**Procédure type :**

1. Dans le portail eKit (ou l'app mobile), crée le **site** (ex. « Agence Nord »).
2. Ajoute le switch au site via son **numéro de série** (ou scan du QR code
   sur l'emballage/l'étiquette si disponible).
3. Branche le switch au réseau avec accès Internet : il s'enregistre
   automatiquement (« deployment through the registration query center »).
4. Applique un **template de configuration** (VLAN, SSID si AP eKit associés)
   depuis le portail.
5. Vérifie dans le portail : switch « en ligne », version logicielle, alertes.

✅ **Intérêt majeur multi-sites :** un technicien branche, l'admin configure à
distance. Zéro déplacement pour un remplacement standard.

⚠️ **Dépendance Internet :** sans accès Internet, le switch ne peut ni
s'enregistrer ni recevoir de template. Toujours prévoir le **plan B local**
(console + config de base sur clé USB / documentée) pour les sites sans
connectivité fiable.

---

## 23. Cloud vs local : bascule et coexistence

- Les switches S310 supportent **les deux modes** et la bascule de l'un à l'autre
  selon le besoin (valeur datasheet).
- En mode cloud, la configuration poussée par le portail **prime** ; une
  modification locale en CLI peut être écrasée à la prochaine synchronisation.
- 🔧 **Règle d'équipe :** on choisit **un** mode de référence par site et on s'y
  tient. Le mélange « un coup cloud, un coup CLI » est la première source de
  configurations fantômes.
- Pour un dépannage urgent en CLI sur un switch cloud-managed : fais-le, mais
  **reporte la modification dans le template cloud** juste après, sinon elle
  disparaîtra.

---

## 24. Date, heure et NTP : le détail qui change tout

Des logs sans heure fiable sont inutilisables pour le dépannage (et pour la
corrélation avec la vidéosurveillance ou les tickets).

```
system-view
clock timezone Paris add 1        # fuseau (heure d'hiver ; +2 en été si pas de DST auto)
ntp-service unicast-server 192.168.99.5   # serveur NTP interne (ou  pool public si autorisé)
quit
save
```

🔧 Vérifications :

```
display clock
display ntp-service status
```

⚠️ Si ton pare-feu bloque le NTP (UDP 123) vers Internet, utilise **ton propre
serveau NTP interne** (souvent le contrôleur de domaine ou le routeur). Un switch
à l'heure, c'est aussi des certificats HTTPS qui fonctionnent et des logs
exploitables.

---

## 25. Nommer le switch : hostname et descriptions

```
system-view
sysname SW-ACC-01-AGENCE-NORD
quit
save
```

**Convention de nommage suggérée :** `SW-<rôle>-<num>-<site>`
(`SW-ACC-01-NORD`, `SW-CORE-01-SIEGE`). Et surtout, **décris chaque port** :

```
interface GigabitEthernet0/0/7
 description AP_ETAGE1_COULOIR
```

✅ Un `display interface brief` avec des descriptions, c'est 30 minutes de
dépannage économisées à chaque incident. Rends la description obligatoire dans
tes procédures d'ajout d'équipement.

---

## 26. Checklist de première configuration (à cocher sur site)

- [ ] Console OK, mot de passe admin fort défini et **documenté au coffre**
- [ ] Hostname selon la convention (`SW-...`)
- [ ] VLAN management créé, VLANIF avec IP, passerelle par défaut
- [ ] HTTPS activé, HTTP désactivé (ou planifié)
- [ ] SSH activé (section 117), Telnet désactivé
- [ ] NTP configuré, `display clock` correct
- [ ] Enregistrement eKit cloud (si mode cloud choisi)
- [ ] Ports décrits au fur et à mesure du câblage
- [ ] `save` effectué (vérifié par `display saved-configuration` vs `display current-configuration`)
- [ ] Config sauvegardée **hors du switch** (fichier texte, section 99)
- [ ] Étiquette sur le switch : hostname + IP de management (au marqueur sur
      scotch d'électricien, ou étiqueteuse — le post-it ne tient pas 3 ans)

🔧 **Le test final :** débranche/rebranche électriquement le switch. Tout doit
revenir (IP, config, PoE). Si quelque chose manque, c'est qu'un `save` a été oublié.

---

## 27. Comprendre les vues et la sauvegarde (VRP en 2 minutes)

| Commande | Effet |
|---|---|
| `system-view` | Passe en mode configuration |
| `quit` | Remonte d'un niveau (ou quitte le mode config) |
| `return` | Remonte directement en vue utilisateur `<...>` |
| `display current-configuration` | Config **active** (en RAM) |
| `display saved-configuration` | Config **sauvegardée** (en flash) |
| `save` | Copie la config active vers la flash |
| `display this` | Config de la vue courante (pratique dans une interface) |
| `?` | Aide contextuelle — ton meilleur ami |
| `Tab` | Complétion |

⚠️ **Piège n°1 des débutants Huawei :** tout ce que tu configures est **perdu au
reboot** tant que tu n'as pas fait `save`. Prends le réflexe : **chaque session de
config se termine par `save`**.

⚠️ **Piège n°2 :** `display current-configuration` peut être très longue.
Filtre avec `| include` : `display current-configuration | include vlan`.

---

## 28. Les ports du S310 dans la CLI : numérotation

Les interfaces se nomment `GigabitEthernet0/0/X` (abrégé `GE0/0/X`) pour les ports
cuivre, et `XGigabitEthernet0/0/X` pour les SFP+ 10G.

- `0/0/1` à `0/0/24` : ports RJ45 (sur un 24 ports).
- `0/0/25` à `0/0/28` : cages SFP/SFP+ (uplinks).
- Sur un 48 ports : `0/0/1` à `0/0/48`, SFP en `0/0/49` à `0/0/52`
  (**à vérifier sur le modèle exact** via `display interface brief`).

**Configurer plusieurs ports d'un coup (gain de temps énorme) :**

```
system-view
port-group pg-utilisateurs
 group-member GigabitEthernet0/0/1 to GigabitEthernet0/0/20
 port link-type access
 port default vlan 10
quit
save
```

🔧 `display interface brief` : la commande à taper en premier devant **tout**
problème de connectivité. Elle montre l'état physique (up/down) et protocole de
chaque port en un écran.

---

## 29. Mise à jour immédiate ? La question à se poser au déballage

Avant de configurer 50 VLAN, regarde la version logicielle :

```
display version
```

- Si le switch sort d'usine avec une version ancienne et que le site a Internet,
  le **Smart Upgrade** via la plateforme HOUP (Huawei Online Upgrade Platform)
  propose le chemin de mise à jour standardisé, en un clic (valeur datasheet).
- ✅ **Fais la mise à jour AVANT la configuration** sur un switch neuf : tu
  configures une fois, sur la version cible, sans risque de changement de
  comportement entre deux versions.
- ⚠️ Sur un switch **en production**, jamais de mise à jour sans sauvegarde
  (section 99) et sans plan de rollback (section 103).

---

## 30. VLAN : rappels 802.1Q (le minimum vital)

- Un VLAN = un **domaine de broadcast** séparé. Sans routage inter-VLAN, deux
  VLAN ne se parlent pas. C'est la base de la segmentation.
- Une trame **taggée** porte un en-tête 802.1Q avec l'ID du VLAN (1–4094).
  Une trame **non taggée** appartient au VLAN natif/PVID du port.
- **PVID** (Port VLAN ID) : le VLAN attribué aux trames non taggées qui entrent
  par le port.
- Sur le S310 : **4 094 VLAN** supportés, table MAC **16 000 entrées**
  (valeurs catalogue — à vérifier sur la fiche du modèle exact).

**Plan de VLAN type PME (exemple, à adapter) :**

| VLAN | Nom | Usage | Réseau exemple |
|---|---|---|---|
| 1 | default | Ne pas utiliser en production | — |
| 10 | USERS | Postes bureautiques | 192.168.10.0/24 |
| 20 | VOICE | Téléphonie IP | 192.168.20.0/24 |
| 30 | GUEST | Wi-Fi invités | 192.168.30.0/24 |
| 40 | CAMERA | Vidéosurveillance | 192.168.40.0/24 |
| 50 | SERVERS | Serveurs / NAS | 192.168.50.0/24 |
| 99 | MGMT | Management | 192.168.99.0/24 |

✅ Documente ce plan **par écrit** et affiche-le dans la baie. Un VLAN créé
« de tête » et oublié, c'est une panne dans 6 mois.

---

## 31. Créer des VLAN : la méthode propre

```
system-view
vlan batch 10 20 30 40 50 99
vlan 10
 description VLAN_USERS_BUREAUTIQUE
quit
vlan 20
 description VLAN_VOICE_TELEPHONIE
quit
vlan 99
 description VLAN_MANAGEMENT
quit
save
```

🔧 Vérification :

```
display vlan
display vlan 10
```

[web] *Fonctions L2 → VLAN → créer*, avec description. Le `vlan batch` n'a pas
toujours d'équivalent web : la CLI reste plus rapide pour les séries.

---

## 32. Ports access : la configuration de tous les jours

Un port **access** appartient à **un seul** VLAN, en non taggé. C'est le cas standard :
PC, imprimante, caméra, AP en mode simple.

```
system-view
interface GigabitEthernet0/0/5
 port link-type access
 port default vlan 10
 description PC_COMPTA_03
 undo shutdown
quit
save
```

**En série avec un port-group (20 postes d'un coup) :**

```
system-view
port-group pg-users
 group-member GigabitEthernet0/0/1 to GigabitEthernet0/0/20
 port link-type access
 port default vlan 10
quit
save
```

🔧 Vérification :

```
display port vlan GigabitEthernet0/0/5
```

⚠️ `undo shutdown` : sur Huawei, les ports sont **up par défaut**, mais après
certaines manips (ou un `shutdown` de test oublié), un port peut rester
administrativement down. Devant un port « mort », `display interface` dit si
c'est physique ou administratif.

---

## 33. Ports trunk : faire passer plusieurs VLAN

Un port **trunk** transporte **plusieurs VLAN taggés** (+ éventuellement un VLAN
natif non taggé). Usage : uplinks vers le cœur, vers un autre switch, vers un
pare-feu/routeur, vers un AP multi-SSID.

```
system-view
interface GigabitEthernet0/0/28        # cage SFP, uplink vers le cœur
 port link-type trunk
 port trunk allow-pass vlan 10 20 30 40 50 99
 port trunk pvid vlan 99              # VLAN natif (non taggé) = management
 description UPLINK_VERS_SW_CORE
 undo shutdown
quit
save
```

🔧 Vérifications :

```
display port vlan GigabitEthernet0/0/28
display trunkmembership
```

⚠️ **Erreur classique :** oublier un VLAN dans `allow-pass` → le VLAN « ne passe
pas » alors que tout semble configuré (voir dépannage section 121). **Des deux
côtés du lien**, les listes doivent correspondre.

⚠️ Le **PVID du trunk** (VLAN natif) doit être **identique des deux côtés**,
sinon : fuite de broadcast d'un VLAN vers l'autre et comportements bizarres.
En pratique : mets le VLAN de management en natif des deux côtés, ou `vlan 1`
si tu assumes (déconseillé, section 20).

---

## 34. Ports hybrid : la spécificité Huawei (à maîtriser absolument)

C'est **LA** différence avec Cisco/HP : le port **hybrid** peut envoyer certains
VLAN **taggés** et d'autres **non taggés**, au choix, port par port. C'est le mode
le plus souple — et celui qu'il faut comprendre pour la **voice VLAN** et les
cas mixtes (téléphone + PC).

```
system-view
interface GigabitEthernet0/0/8
 port link-type hybrid
 port hybrid pvid vlan 10                    # VLAN non taggé par défaut = users
 port hybrid tagged vlan 20                  # VLAN 20 (voix) sort taggé
 port hybrid untagged vlan 10                # VLAN 10 sort non taggé
 description TEL_IP_+_PC_BUREAU_08
 undo shutdown
quit
save
```

**Logique de fonctionnement :**

| Trame entrante | Traitement |
|---|---|
| Non taggée | Marquée avec le **PVID** du port |
| Taggée VLAN 20 | Acceptée (VLAN autorisé en tagged) |
| Taggée VLAN 30 | **Refusée** (non autorisée sur ce port) |

| Trame sortante (VLAN 10) | Non taggée (untagged) |
|---|---|
| Trame sortante (VLAN 20) | Taggée (tagged) |

✅ **Quand utiliser hybrid plutôt que trunk ?**
- Téléphone IP + PC branché derrière le téléphone (section 38).
- Équipement qui attend un VLAN en non taggé **et** un autre en taggé.
- Interconnexion avec du matériel hétérogène aux attentes exotiques.

⚠️ **Ne mélange pas les philosophies sans raison :** si tout ton parc est en
access/trunk classique, n'introduis pas du hybrid « pour voir ». Chaque
exception de config est une heure de dépannage en plus dans 2 ans.

---

## 35. Tableau récapitulatif : access / trunk / hybrid

|  | Access | Trunk | Hybrid |
|---|---|---|---|
| Nombre de VLAN | 1 | Plusieurs | Plusieurs |
| Trames entrantes non taggées | → PVID (le VLAN du port) | → PVID du trunk | → PVID du port |
| Trames sortantes | Toujours non taggées | Taggées (sauf PVID) | Au choix par VLAN (tagged/untagged) |
| Usage typique | PC, imprimante, caméra | Uplink, AP multi-SSID | Téléphone+PC, cas mixtes |
| Commande clé | `port default vlan X` | `port trunk allow-pass vlan ...` | `port hybrid tagged/untagged vlan ...` |

---

## 36. Interface VLANIF et routage inter-VLAN (le « L2+ » du S310)

Le S310 est **L2+** : il sait router entre VLAN avec des **routes statiques**.
Pour que le VLAN 10 parle au VLAN 50, il faut une interface IP dans chaque VLAN
(sur le switch qui fait office de passerelle, souvent le switch cœur).

```
system-view
interface Vlanif10
 ip address 192.168.10.1 255.255.255.0
quit
interface Vlanif50
 ip address 192.168.50.1 255.255.255.0
quit
# Les postes utilisent .1 comme passerelle → le switch route entre VLAN
quit
save
```

Et vers Internet / le routeur principal :

```
system-view
ip route-static 0.0.0.0 0.0.0.0 192.168.50.254   # route par défaut vers le pare-feu
quit
save
```

🔧 Vérifications :

```
display ip interface brief
display ip routing-table
ping 192.168.50.10 -a 192.168.10.1   # test avec source précisée
```

⚠️ **Qui route ?** Un seul équipement doit être la passerelle d'un VLAN donné.
Deux passerelles (le S310 **et** le pare-feu) sur le même VLAN = routage
asymétrique et pannes intermittentes. Décide et documente.

⚠️ Le routage inter-VLAN **sans ACL** ouvre tout entre les VLAN : le VLAN
invités peut joindre les serveurs. Ajoute des **ACL** (section 86) dès que le
routage inter-VLAN est actif.

---

## 37. Exemple complet commenté : PME 3 VLAN (bureautique, invités, serveurs)

**Contexte :** un S310-24P4S, 20 PC (VLAN 10), 2 AP Wi-Fi invités (VLAN 30),
1 NAS (VLAN 50), uplink SFP vers le pare-feu en trunk.

```
system-view
sysname SW-ACC-01
#
vlan batch 10 30 50 99
vlan 10
 description VLAN_USERS
quit
vlan 30
 description VLAN_GUEST
quit
vlan 50
 description VLAN_SERVERS
quit
vlan 99
 description VLAN_MGMT
quit
#
# --- Ports utilisateurs 1-20 : access VLAN 10 ---
port-group pg-users
 group-member GigabitEthernet0/0/1 to GigabitEthernet0/0/20
 port link-type access
 port default vlan 10
 stp edged-port enable        # ports terminaux : pas d'attente STP (section 48)
quit
#
# --- AP invités 21-22 : trunk, SSID invités taggé 30, management 99 natif ---
interface GigabitEthernet0/0/21
 port link-type trunk
 port trunk allow-pass vlan 30 99
 port trunk pvid vlan 99
 description AP_GUEST_ENTREE
 stp edged-port enable
quit
interface GigabitEthernet0/0/22
 port link-type trunk
 port trunk allow-pass vlan 30 99
 port trunk pvid vlan 99
 description AP_GUEST_ATELIER
 stp edged-port enable
quit
#
# --- NAS port 23 : access VLAN 50 ---
interface GigabitEthernet0/0/23
 port link-type access
 port default vlan 50
 description NAS_SYNOLOGY
 undo shutdown
quit
#
# --- Uplink SFP 28 vers pare-feu : trunk tous les VLAN ---
interface GigabitEthernet0/0/28
 port link-type trunk
 port trunk allow-pass vlan 10 30 50 99
 port trunk pvid vlan 99
 description UPLINK_FW_PFSENSE
 undo shutdown
quit
#
# --- Management ---
interface Vlanif99
 ip address 192.168.99.10 255.255.255.0
quit
ip route-static 0.0.0.0 0.0.0.0 192.168.99.1
quit
save
```

[web] Tout est faisable en web (*VLAN → Ports*), mais la CLI est 5× plus rapide
dès qu'il y a plus de 5 ports.

---

## 38. Voice VLAN : le téléphone IP sans se prendre la tête

**Principe :** le téléphone IP s'alimente en PoE, taggue sa voix dans le VLAN 20,
et laisse passer le PC branché derrière lui en non taggé dans le VLAN 10.
Le switch reconnaît le téléphone via son **OUI MAC** (préfixe constructeur).

```
system-view
vlan 20
 description VLAN_VOICE
quit
#
voice-vlan 20 enable
voice-vlan mac-address 001B-2A00-0000 mask ffff-ff00-0000 description YEALINK
#  ^ OUI du constructeur de téléphones (exemple fictif : remplace par le vrai OUI)
#
interface GigabitEthernet0/0/8
 port link-type hybrid
 port hybrid pvid vlan 10
 port hybrid tagged vlan 20
 port hybrid untagged vlan 10
 voice-vlan 20 enable
 description TEL_YEALINK_BUREAU08
 undo shutdown
quit
save
```

**Ce qui se passe :** le téléphone envoie ses trames taggées VLAN 20 → le switch
les accepte et les fait suivre ; le PC derrière envoie en non taggé → PVID 10.
**Zéro configuration sur le téléphone** (il doit juste être en mode VLAN auto /
LLDP-MED, réglage par défaut sur la plupart des Yealink/Grandstream — à vérifier
sur le modèle exact du téléphone).

🔧 Vérifications :

```
display voice-vlan status
display mac-address vlan 20
```

⚠️ **OUI inconnu :** si les téléphones n'apparaissent pas dans le VLAN 20,
vérifie l'OUI (`display mac-address` puis compare les 3 premiers octets) et
ajoute-le. C'est la cause n°1 des « la voix ne passe pas ».

---

## 39. Exemple : téléphone + PC en daisy-chain, avec QoS voix

On combine voice VLAN (section 38) et priorité voix (section 73) :

```
system-view
interface GigabitEthernet0/0/8
 port link-type hybrid
 port hybrid pvid vlan 10
 port hybrid tagged vlan 20
 port hybrid untagged vlan 10
 voice-vlan 20 enable
 trust dscp                       # fait confiance au marquage du téléphone
 description TEL+PC_BUREAU08
 undo shutdown
quit
save
```

✅ Le téléphone marque déjà ses paquets (DSCP EF en général) : avec `trust dscp`,
le switch les priorise sans autre config. Vérifie que le téléphone est bien
configuré pour marquer (souvent défaut usine, à vérifier sur le modèle exact).

---

## 40. MUX VLAN : isoler sans multiplier les VLAN

Le S310 supporte la fonction **MUX VLAN** (valeur datasheet) : un VLAN
**principal** + des VLAN **subordonnés** (groupes ou isolés). Les ports isolés
ne se parlent pas entre eux mais parlent au principal (ex. : imprimante
partagée, box Internet, serveur).

```
system-view
vlan 60
 mux-vlan
 subordinate separate 61
 subordinate group 62
quit
interface GigabitEthernet0/0/15
 port link-type access
 port default vlan 61          # port isolé : ne voit que le VLAN principal
quit
interface GigabitEthernet0/0/16
 port mux-vlan enable
quit
save
```

> La syntaxe exacte et la disponibilité dépendent de la **version logicielle** :
> **à vérifier sur le modèle exact** (`display version`, puis `?` dans la vue VLAN).
> Alternative universelle : **port isolation** (section 82), plus simple.

---

## 41. Interopérabilité : trunk vers un switch non-Huawei

Rien de magique : 802.1Q est un standard. Points de vigilance :

1. **VLAN natif identique** des deux côtés (PVID Huawei = native VLAN Cisco).
2. **Liste des VLAN autorisés identique** (`allow-pass` ↔ `switchport trunk allowed vlan`).
3. **STP compatible** : RSTP des deux côtés, ou MSTP avec même nom de région,
   même révision, même mapping VLAN→instance (section 52).
4. Côté Cisco, le trunk négocie parfois en DTP : force `switchport mode trunk`
   + `switchport nonegotiate` côté Cisco pour éviter les surprises.

🔧 Test : `ping` entre deux PC du même VLAN de part et d'autre du trunk,
puis `display mac-address vlan X` des deux côtés pour voir les MAC apprises.

---

## 42. Vérifications VLAN : la routine

```
display vlan                          # tous les VLAN + ports membres
display vlan 10                       # détail d'un VLAN
display port vlan                     # PVID et mode de tous les ports
display port vlan GigabitEthernet0/0/8
display mac-address vlan 10           # quelles MAC sont vues dans le VLAN
display mac-address interface GigabitEthernet0/0/8
```

✅ **Réflexe :** après chaque modification VLAN, `display port vlan` sur les
ports touchés. 80 % des pannes VLAN se voient là en 10 secondes.

---

## 43. STP/RSTP : pourquoi on ne peut pas s'en passer

Deux câbles entre deux switches (ou un switch bouclé sur lui-même) = **tempête
de broadcast** = réseau mort en quelques secondes. Le Spanning Tree bloque
les ports redondants et ne les ouvre qu'en cas de panne du lien principal.

**Les 3 protocoles sur le S310 (valeur datasheet) :**

| Protocole | Standard | Usage |
|---|---|---|
| STP | 802.1D | Historique, lent (30–50 s de convergence) — à éviter |
| **RSTP** | 802.1w | **Le choix par défaut** : convergence en quelques secondes |
| MSTP | 802.1s | Multiples instances par VLAN, pour les topologies complexes |

Le S310 supporte aussi **VBST** (VLAN-based Spanning Tree, spécifique Huawei,
interopérable avec PVST/PVST+/RPVST côté Cisco) et **ERPS** (protection en
anneau ITU-T G.8032).

⚠️ **Règle absolue :** STP/RSTP **toujours activé** sur les ports qui relient
des switches entre eux. Le seul endroit où on l'accélère (edge port), c'est
vers les équipements terminaux (section 48).

---

## 44. Activation de RSTP : la base

```
system-view
stp mode rstp
stp enable
quit
save
```

🔧 Vérifications :

```
display stp brief
```

Tu dois voir : le mode (RSTP), l'ID du root bridge élu, et pour chaque port
son rôle (Root/Designated/Alternate) et son état (Forwarding/Discarding).

⚠️ **Active STP AVANT de brancher le deuxième câble** d'une redondance.
L'ordre « je câble puis je configure » crée la boucle qu'on voulait éviter.
Procédure : configure les deux switches, active RSTP des deux côtés, **puis**
branche le lien redondant.

---

## 45. Choisir le root bridge : la priorité

Par défaut, le switch avec la plus petite priorité (puis la plus petite MAC)
devient root. **Ne laisse pas le hasard décider** : le root doit être ton
switch le plus central et le plus fiable (le cœur, pas un switch d'accès
au fond d'un atelier).

```
system-view
stp root primary        # sur le switch cœur : devient root (priorité 0)
quit
save
```

Sur le switch de secours :

```
system-view
stp root secondary      # priorité 4096 : prend le relais si le primary tombe
quit
save
```

🔧 Vérification : `display stp` → « CIST Root » doit être l'adresse MAC du
switch cœur, sur **tous** les switches.

⚠️ Un root élu au hasard (souvent le plus vieux switch avec la plus petite
MAC) = chemins sous-optimaux + reconvergences bizarres. **Toujours** fixer
primary/secondary explicitement.

---

## 46. Edge port (l'équivalent du portfast) : pour les terminaux

Un port vers un PC, une imprimante ou un AP **ne doit pas attendre** la
convergence STP (30 s en STP classique, même si RSTP est plus rapide) :
le DHCP timeout et l'utilisateur appelle.

```
system-view
interface GigabitEthernet0/0/5
 stp edged-port enable
quit
save
```

En série :

```
system-view
port-group pg-users
 group-member GigabitEthernet0/0/1 to GigabitEthernet0/0/20
 stp edged-port enable
quit
save
```

⚠️ **JAMAIS d'edge port vers un autre switch.** Si un BPDU arrive sur un edge
port, le port perd son statut edge et repasse en mode normal (comportement de
protection standard) — mais pendant ce temps, une boucle a pu faire des dégâts.
Combine avec **BPDU guard** (section 47) sur les ports utilisateurs.

---

## 47. BPDU guard : le coupe-circuit anti-boucle utilisateur

**Scénario :** un utilisateur branche un petit switch « de bureau » (5 ports à
20 €) sous son bureau, avec deux câbles vers la prise murale → boucle → réseau
à genoux. **BPDU guard** désactive automatiquement le port dès qu'il reçoit un
BPDU (donc dès qu'un switch est branché côté utilisateur).

```
system-view
interface GigabitEthernet0/0/5
 stp bpdu-protection
quit
save
```

En série sur tous les ports utilisateurs :

```
system-view
port-group pg-users
 group-member GigabitEthernet0/0/1 to GigabitEthernet0/0/20
 stp edged-port enable
 stp bpdu-protection
quit
save
```

🔧 Quand un port est coupé par BPDU guard : `display interface` montre le port
down, les logs (`display logbuffer`) indiquent la raison. Pour le réarmer :
`shutdown` puis `undo shutdown` sur le port — **après avoir retiré le switch
sauvage**, sinon ça recommence.

✅ **Combo gagnant sur chaque port utilisateur :** `stp edged-port enable` +
`stp bpdu-protection`. C'est la config « set and forget » qui évite 90 % des
boucles accidentelles.

---

## 48. Root guard et BPDU filter : les compléments

**Root guard** : sur un port descendant vers l'accès, empêche qu'un switch
d'accès mal configuré (ou malveillant) ne devienne root.

```
system-view
interface GigabitEthernet0/0/28     # uplink vers un switch d'accès
 stp root-protection
quit
save
```

**BPDU filter** : bloque totalement l'émission/réception de BPDU sur un port.
⚠️ À utiliser avec une extrême prudence (jamais vers un switch), typiquement
vers un équipement qui « n'aime pas » les BPDU.

> Disponibilité exacte de `stp root-protection` / `stp bpdu-filter` : **à vérifier
> sur la version logicielle du modèle exact** (`stp ?` en vue système).

**Résumé des protections STP :**

| Protection | Où | Effet |
|---|---|---|
| edged-port | Ports terminaux | Pas d'attente de convergence |
| bpdu-protection | Ports terminaux | Coupe le port si un BPDU arrive |
| root-protection | Ports vers l'accès | Bloque un root illégitime |
| bpdu-filter | Cas très spécifiques | Ignore totalement les BPDU |

---

## 49. Exemple STP complet : deux switches d'accès redondants

**Topologie :** SW-ACC-01 et SW-ACC-02 reliés au cœur par un lien chacun,
**plus** un lien direct entre eux (secours). RSTP doit bloquer le lien
inter-accès en temps normal.

**Sur le cœur (déjà root primary, section 45) :** rien de plus.

**Sur SW-ACC-01 :**

```
system-view
stp mode rstp
stp enable
stp root secondary      # si le cœur tombe, c'est lui le root de secours... 
                       # (en pratique : secondary sur UN seul des deux accès)
#
interface GigabitEthernet0/0/27
 description LIEN_VERS_ACC02_SECOURS
 port link-type trunk
 port trunk allow-pass vlan 10 20 30 50 99
 undo shutdown
quit
save
```

**Sur SW-ACC-02 :** identique, sans `stp root secondary`.

🔧 Vérification sur les trois switches : `display stp brief`.
Le lien inter-accès doit apparaître en **Alternate / Discarding** sur l'un des
deux. Débranche le lien principal d'un accès → le lien de secours passe en
Forwarding en quelques secondes. **Teste-le en vrai** avant la mise en
production : une redondance jamais testée n'existe pas.

---

## 50. Vérifications STP : la routine

```
display stp brief        # rôles et états par port (le plus utile)
display stp              # détail : root, priorités, timers
display stp interface GigabitEthernet0/0/27
display logbuffer | include STP   # événements STP récents
```

**Lecture d'un `display stp brief` sain :**

- Un seul root pour tout le domaine (même « CIST Root » partout).
- Les ports vers les terminaux : `DESI / FORWARDING`.
- Les liens redondants : un en `FORWARDING`, l'autre en `ALTE / DISCARDING`.
- ⚠️ Deux ports en FORWARDING sur une boucle physique = STP ne fait pas son
  travail → vérifie que `stp enable` est actif **des deux côtés**.

---

## 51. ERPS : la protection en anneau (mention)

Pour les topologies en **anneau** (sites industriels, campus), le S310 supporte
**ERPS** (ITU-T G.8032, valeur datasheet) : convergence < 50 ms, plus rapide
et plus propre que STP en anneau. Configuration plus complexe (R-APS, RPL) :
à réserver aux topologies en anneau volontaires, pas au câblage « en boucle
par accident ». Détail de la config : **à vérifier sur le guide de la version
logicielle exacte**.

---

## 52. MSTP : quand RSTP ne suffit plus

RSTP = **une seule** topologie pour tous les VLAN. Si tu veux que le VLAN 10
passe par le lien A et le VLAN 20 par le lien B (répartition de charge),
il faut **MSTP** avec des instances.

```
system-view
stp mode mstp
stp enable
stp region-configuration
 region-name PME_NORD
 revision-level 1
 instance 1 vlan 10 50
 instance 2 vlan 20 30
 active region-configuration
quit
stp instance 1 root primary
stp instance 2 root secondary
quit
save
```

⚠️ **Les 3 paramètres de région doivent être IDENTIQUES sur tous les switches :**
nom, révision, mapping VLAN→instance. Un seul qui diffère = les switches ne se
reconnaissent pas comme même région = comportements erratiques. `display stp
region-configuration` pour comparer.

✅ En PME simple : **reste en RSTP**. MSTP ne se justifie qu'avec un vrai besoin
de répartition de charge par VLAN.

---

## 53. Agrégation de liens (Eth-Trunk) : le principe

Un **Eth-Trunk** regroupe plusieurs liens physiques en **un seul lien logique** :
- **Plus de débit** (2× 1G = 2 Gbit/s agrégés).
- **Redondance** : si un brin tombe, le trafic bascule sur l'autre sans
  reconvergence STP (le trunk est vu comme un seul port).
- La répartition se fait par **hachage** (MAC/IP source/destination) : **une
  seule conversation** ne dépassera jamais la vitesse d'un brin. C'est normal,
  pas une panne.

**Deux modes :**

| Mode | Négociation | Usage |
|---|---|---|
| Manuel (`mode manual`) | Aucune | Équipements sans LACP, ou besoin déterministe |
| **LACP** (`mode lacp`) | Oui (802.3ad) | **Recommandé** : détection de panne, ajout/retrait dynamique |

---

## 54. Eth-Trunk manuel : configuration

**Exemple :** 2 liens (ports 25-26) vers le NAS en mode manuel.

```
system-view
interface Eth-Trunk1
 description TRUNK_VERS_NAS
 mode manual load-balance        # mode manuel (selon version : "mode manual")
quit
interface GigabitEthernet0/0/25
 eth-trunk 1
quit
interface GigabitEthernet0/0/26
 eth-trunk 1
quit
# Le trunk hérite ensuite de la config VLAN :
interface Eth-Trunk1
 port link-type trunk
 port trunk allow-pass vlan 10 50
quit
save
```

> La syntaxe exacte (`mode manual` vs `mode manual load-balance`) est **à vérifier
> sur la version logicielle du modèle exact** (`interface Eth-Trunk1` puis `mode ?`).

---

## 55. LACP : la configuration recommandée

**Exemple :** 2× 1G (ports 27-28) vers le switch cœur, en LACP actif.

**Sur SW-ACC-01 :**

```
system-view
interface Eth-Trunk10
 description LACP_VERS_CORE
 mode lacp
 port link-type trunk
 port trunk allow-pass vlan 10 20 30 50 99
quit
interface GigabitEthernet0/0/27
 eth-trunk 10
 lacp priority 100
quit
interface GigabitEthernet0/0/28
 eth-trunk 10
quit
save
```

**Sur le cœur :** config miroir (même numéro de trunk conseillé, pas obligatoire).

🔧 Vérifications :

```
display eth-trunk 10
```

État attendu : les deux membres en **Selected** (actifs). Un membre en
**Unselected** = problème de négociation (vérifie le mode LACP des deux côtés,
la vitesse/duplex, le câble).

⚠️ **LACP des deux côtés** : un côté en LACP et l'autre en manuel = le trunk ne
monte pas (ou monte à moitié). Toujours vérifier le mode en face.

---

## 56. Eth-Trunk : bonnes pratiques et limites

1. **Même vitesse, même duplex** sur tous les membres (négociation auto des
   deux côtés, ou forcé identique des deux côtés — jamais de mélange).
2. **Même type de port** : que du cuivre ou que de la fibre, pas de mix.
3. Maximum de membres : **à vérifier sur la fiche du modèle exact**
   (souvent 8 sur cette gamme).
4. Le trunk se comporte comme **un seul port** pour STP et les VLAN : configure
   le VLAN sur l'interface Eth-Trunk, **pas** sur les membres.
5. Pour un trunk **inter-sites en fibre** : utilise des paires de fibres
   identiques (même longueur d'onde, même type).
6. ⚠️ Ne mets jamais les deux brins d'un trunk sur le **même chemin physique**
   (même goulotte écrasée par un transpalette = les deux brins coupés).

---

## 57. PoE : rappels 802.3af/at et classes de puissance

Le switch **détecte** l'équipement (signature résistive), **classifie** son besoin,
puis alimente. Pas de détection = pas de courant : brancher un PC non-PoE sur un
port PoE **ne risque rien**.

| Classe | Standard | Puissance max fournie (PSE) | Puissance utile (PD) | Exemples |
|---|---|---|---|---|
| Classe 1 | 802.3af | 4 W | 3,84 W | Téléphone IP basique |
| Classe 2 | 802.3af | 7 W | 6,49 W | Téléphone IP écran couleur |
| Classe 3 | 802.3af | 15,4 W | 12,95 W | AP Wi-Fi 6 d'intérieur, caméra fixe |
| Classe 4 | 802.3at (PoE+) | 30 W | 25,5 W | AP extérieur, caméra PTZ, visio |

Le S310-P est **PoE+** (802.3at) : **30 W max par port**, dans la limite du
**budget total** (400 W sur 24 ports, 380 W sur 48 ports).

⚠️ La puissance « fournie » inclut les pertes dans le câble : un équipement de
12,95 W consomme ~15,4 W côté switch. **Toujours calculer côté switch.**

---

## 58. Budget PoE par modèle (valeurs constructeur)

| Modèle | Budget total | Max par port | Conso max pleine charge |
|---|---|---|---|
| S310-24P4S | **400 W** | 30 W | 491,66 W |
| S310-24P4X | **400 W** | 30 W | à vérifier sur la fiche du modèle exact |
| S310-24PN4X | **400 W** | 30 W | à vérifier sur la fiche du modèle exact |
| S310-48P4S | **380 W** | 30 W | 462,8 W |

🔧 Commandes :

```
display poe power-state          # budget total, consommé, disponible
display poe interface all        # état PoE port par port
```

---

## 59. Fast PoE et Perpetual PoE : ce que ça change au quotidien

Deux fonctions constructeur (valeur datasheet) qui évitent des incidents :

- **Fast PoE** : après une coupure de courant, le switch réalimente les
  équipements PoE en **quelques secondes**, sans attendre la fin du boot
  (1 à 3 minutes sur un switch classique). Tes caméras et AP repartent
  presque immédiatement.
- **Perpetual PoE** : pendant un **redémarrage logiciel** (ex. mise à jour
  firmware), l'alimentation PoE **n'est pas coupée**. Les AP restent associés,
  les caméras continuent d'enregistrer.

✅ **Conséquence pratique :** une mise à jour firmware un soir de semaine devient
envisageable sans couper la vidéosurveillance. (Ça ne dispense pas de la faire
en heure creuse — section 102.)

---

## 60. Configurer le PoE par port

```
system-view
interface GigabitEthernet0/0/7
 poe enable                      # active le PoE (souvent actif par défaut)
 poe priority critical           # priorité : critical / high / low
 poe power-off time-range ...    # extinction planifiée (si supporté)
 description AP_ETAGE1
quit
save
```

**Forcer une réalimentation (le « débranche/rebranche » à distance) :**

```
system-view
interface GigabitEthernet0/0/7
 poe power-off
 poe power-on
quit
```

🔧 C'est la commande magique quand une caméra ou un AP est « planté » :
couper/remettre le PoE **à distance**, sans monter à l'échelle. À documenter
dans tes procédures de niveau 1.

**Limiter la puissance max d'un port (sécurité + budget) :**

```
system-view
interface GigabitEthernet0/0/7
 poe power 15000                 # limite à 15 W (en milliwatts selon version)
quit
save
```

> L'unité et la syntaxe exacte (`poe power`, `poe max-power`) sont **à vérifier
> sur la version logicielle du modèle exact**.

---

## 61. Priorités PoE : éviter que l'AP directeur tombe avant la caméra du parking

Quand le budget est dépassé, le switch coupe les ports **par priorité croissante**
(d'abord `low`, en dernier `critical`). **Configure les priorités dès le jour 1**,
pas le jour où ça coupe.

**Politique type :**

| Priorité | Équipements |
|---|---|
| `critical` | AP du bureau de direction ? Non — **téléphones de secours, AP du hall**... En vrai : ce qui est vital pour la sécurité |
| `high` | AP Wi-Fi intérieurs, téléphones IP |
| `low` | Caméras extérieures non critiques, AP invités |

```
system-view
interface GigabitEthernet0/0/7
 poe priority high
quit
interface GigabitEthernet0/0/12
 poe priority low
quit
save
```

⚠️ **Réfléchis avant de mettre `critical` partout :** si tout est critique,
rien ne l'est, et le switch coupera au hasard (en pratique : par numéro de port).

---

## 62. Planification PoE : la méthode (à faire AVANT d'acheter)

1. **Liste** chaque équipement PoE prévu : type, quantité, puissance max
   **côté switch** (fiche constructeur de l'équipement, pas « à peu près »).
2. **Additionne** : total = Σ puissances.
3. **Ajoute 20 % de marge** (pics, vieillissement, un équipement ajouté « vite fait »).
4. **Compare** au budget du modèle : total × 1,2 ≤ budget → OK.
5. **Vérifie par port** : aucun équipement > 30 W (sinon : injecteur 60/90 W
   externe ou autre gamme).
6. **Attribue les priorités** (section 61).

**Template de tableau de planification :**

| Port | Équipement | Puissance (W) | Priorité | Commentaire |
|---|---|---|---|---|
| GE0/0/1 | AP361 hall | 8,8 | high | |
| GE0/0/2 | Caméra PTZ parking | 25 | low | Classe 4 |
| ... | ... | ... | ... | |
| **Total** | | **... W** | | Marge : budget − total |

---

## 63. Calcul : combien d'AP Huawei eKit alimentables ?

**Données constructeur vérifiées (recherche web, septembre 2026) :**
- **AP361** (Wi-Fi 6 intérieur) : **8,8 W** max, PoE 802.3af.
- **AP761** (Wi-Fi 6 extérieur) : **17,7 W** max, PoE 802.3at recommandé
  (en 802.3af, fonctions limitées).

**Sur S310-24P4S (budget 400 W, 24 ports) :**

| Équipement | Calcul | Résultat |
|---|---|---|
| AP361 (8,8 W) | 400 ÷ 8,8 = 45,4 | **Limité par les ports : 24 AP** (211 W consommés, marge énorme) |
| AP761 (17,7 W) | 400 ÷ 17,7 = 22,6 | **22 AP max** (389 W) — le 23e ne montera pas |
| Mixte : 12× AP361 + 6× AP761 | 12×8,8 + 6×17,7 = 211,8 W | ✅ Largement OK |

**Sur S310-48P4S (budget 380 W, 48 ports) :**

| Équipement | Calcul | Résultat |
|---|---|---|
| AP361 (8,8 W) | 380 ÷ 8,8 = 43,1 | **43 AP max** (378 W) — pas 48 ! |
| AP761 (17,7 W) | 380 ÷ 17,7 = 21,4 | **21 AP max** (372 W) |
| Mixte : 24× AP361 + 10 caméras 15 W | 24×8,8 + 10×15 = 361 W | ✅ OK (19 W de marge — juste, prévoir priorités) |

⚠️ **Le 48P4S ne peut pas alimenter 48 AP761** : c'est le piège classique du
« 48 ports PoE ». Le nombre de ports n'est **pas** le nombre d'équipements
alimentables. Fais toujours le calcul.

🔧 En exploitation : `display poe power-state` régulièrement, et alerte si le
disponible passe sous 20 % du budget.

---

## 64. Dépannage PoE : arbre de décision

**Symptôme : l'équipement ne s'allume pas.**

1. `display poe interface GigabitEthernet0/0/X` → le port fournit-il ?
   - **Non / « off »** : vérifie `poe enable` sur le port, puis le **budget
     restant** (`display poe power-state`). Budget épuisé → débranche ou
     repriorise (section 61).
   - **Oui mais l'équipement reste éteint** : passe à 2.
2. **Câble** : teste avec une jarretière courte connue bonne. Les paires
   utilisées par le PoE (1-2/3-6 en mode A, 4-5/7-8 en mode B) peuvent être
   coupées alors que les données passent (cas tordu mais réel).
3. **Distance** : > 100 m = PoE non garanti. Mesure la longueur (VCT, section 96).
4. **Classe** : `display poe interface` montre la classe détectée. Classe 0
   inattendue = l'équipement ne négocie pas → teste avec un injecteur pour
   isoler (équipement HS vs switch).
5. **Port** : essaie le même équipement sur un autre port PoE. Si ça marche
   ailleurs → port suspect (surtension antérieure ?).
6. **Budget par port** : une limite `poe power` trop basse (section 60) empêche
   la montée en puissance (caméra PTZ qui démarre puis s'éteint = typique).

**Symptôme : l'équipement s'éteint par intermittence.**

- Budget **tout juste** suffisant : un pic (démarrage IR de la caméra la nuit)
  fait dépasser → le switch coupe le port le moins prioritaire. Augmente la
  marge ou change les priorités.
- `display logbuffer | include POE` : les coupures sont loggées avec la cause
  (overload, short-circuit...).

✅ **Bon réflexe :** devant tout équipement PoE « bizarre », commence par
`poe power-off` / `poe power-on` à distance. Ça règle 50 % des cas (équipement
planté) en 30 secondes.

---

## 65. QoS : rappels (le minimum pour ne pas tout casser)

- La QoS ne **crée** pas de bande passante : elle **choisit qui passe en premier**
  quand il y a congestion.
- Deux marquages : **802.1p** (priorité 0–7 dans le tag VLAN, niveau 2) et
  **DSCP** (6 bits dans l'en-tête IP, niveau 3).
- Le switch classe le trafic, le met dans des **files d'attente**, et les vide
  selon un **ordonnancement** (strict priority, WRR...).
- **Règle d'or :** marque **au plus près de la source** (le téléphone marque sa
  voix), et fais **confiance** (`trust`) à ce marquage sur le switch. Marquer
  partout « au cas où » = marquage incohérent = QoS inutile.

**Valeurs à connaître :**

| Trafic | DSCP | 802.1p | Nom |
|---|---|---|---|
| Voix (RTP) | EF (46) | 5 | Priorité max |
| Signalisation voix (SIP) | CS3 (24) | 3 | Haute |
| Visio interactive | AF41 (34) | 4 | Haute |
| Données critiques | AF31 (26) | 3 | Moyenne-haute |
| Best effort (défaut) | 0 | 0 | — |

---

## 66. Trust DSCP vs trust 802.1p : configuration

**Faire confiance au DSCP (recommandé quand les équipements marquent bien) :**

```
system-view
interface GigabitEthernet0/0/8
 trust dscp
quit
save
```

**Faire confiance au 802.1p (utile en L2 pur, sans IP) :**

```
system-view
interface GigabitEthernet0/0/8
 trust 8021p
quit
save
```

**Forcer une priorité (quand l'équipement ne marque pas, ex. vieux téléphone) :**

```
system-view
interface GigabitEthernet0/0/9
 port priority 5        # tout ce qui entre par ce port = priorité 5
quit
save
```

> La disponibilité exacte de `trust dscp` / `trust 8021p` en vue interface est
> **à vérifier sur la version logicielle du modèle exact**. Sur certaines
> versions eKit, la QoS se configure via des **traffic classifiers/behaviors/
> policies** (modèle MQC, voir section 67).

---

## 67. Files, ordonnancement et MQC (le modèle Huawei)

Sur VRP, la QoS avancée suit le modèle **MQC** (Modular QoS Command-line) en
3 étapes : **classifier** (qui ?) → **behavior** (quoi faire ?) → **policy**
(appliquée où ?).

```
system-view
# 1. Classifier : le trafic voix (DSCP EF)
traffic classifier VOICE operator or
 if-match dscp ef
quit
# 2. Behavior : priorité stricte
traffic behavior VOICE_PRIO
 queue af bandwidth pct 30        # exemple : file avec 30 % de bande garantie
quit
# 3. Policy : on lie les deux et on l'applique sur l'uplink
traffic policy QOS_LAN
 classifier VOICE behavior VOICE_PRIO
quit
interface GigabitEthernet0/0/28
 traffic-policy QOS_LAN outbound
quit
save
```

> Les paramètres exacts de files (`queue af`, `pq`, `wrr`, `shaping`) et leur
> disponibilité sont **à vérifier sur la version logicielle du modèle exact**.
> En PME, le **trust DSCP + priorité stricte pour la voix** (section 68) couvre
> 95 % des besoins sans MQC complet.

---

## 68. Exemple : prioriser la voix sur tout le switch

Objectif : la voix (VLAN 20, DSCP EF) passe toujours en premier sur l'uplink,
même quand un utilisateur sature le lien avec un gros transfert.

```
system-view
# Sur chaque port téléphone : confiance au marquage du poste
port-group pg-voice
 group-member GigabitEthernet0/0/1 to GigabitEthernet0/0/20
 trust dscp
quit
# Sur l'uplink : la file voix est servie en priorité stricte
# (via MQC selon version, ou trust dscp + ordonnancement par défaut)
interface GigabitEthernet0/0/28
 trust dscp
quit
save
```

🔧 Vérifications :

```
display qos interface GigabitEthernet0/0/28
display traffic-policy applied-record
```

**Test de validation :** lance un gros transfert (iperf) entre deux PC pendant
un appel : l'appel ne doit pas se dégrader. Si ça craque, la QoS n'est pas
appliquée au bon endroit (souvent : oubliée sur l'uplink).

---

## 69. Exemple : prioriser la visio (Teams/Zoom)

La visio est plus gourmande que la voix et moins bien marquée (souvent en
best effort par défaut). Deux options :

1. **Marquer côté poste** (stratégie Windows/GPO, à vérifier) puis `trust dscp`.
2. **Classifier par ports/applications** sur le switch (ex. plages UDP de Teams)
   — complexe et fragile (les plages changent).

✅ **Recommandation terrain :** pour la visio en PME, la vraie solution est
rarement la QoS fine : c'est **de la bande passante** (uplink 10G, section 5)
et un **VLAN visio séparé** si besoin. La QoS ne sauve pas un lien saturé à 100 %.

---

## 70. Vérifications QoS : la routine

```
display qos interface GigabitEthernet0/0/28     # files et compteurs
display traffic classifier user-defined
display traffic behavior user-defined
```

**Compteurs à surveiller :** les paquets **droppés** par file. Des drops en file
voix = la priorité ne suffit pas = il faut plus de bande passante, pas plus de QoS.

---

## 71. Panorama sécurité du S310 (tableau)

Fonctions annoncées au datasheet — disponibilité fine **à vérifier sur la version
logicielle du modèle exact** :

| Fonction | Rôle | Section |
|---|---|---|
| Port security + sticky MAC | Limite les MAC par port, mémorise | 72 |
| Storm control | Plafonne broadcast/multicast/unicast inconnu | 73 |
| Port isolation | Isole des ports entre eux (L2) | 74 |
| DHCP snooping | Filtre les serveurs DHCP pirates | 75 |
| Binding IP+MAC+port+VLAN | Table de correspondance anti-spoofing | 76 |
| DAI (Dynamic ARP Inspection) | Vérifie les ARP (via binding) | 77 |
| IPSG (IP Source Guard) | Filtre le trafic selon le binding | 77 |
| ACL | Filtrage L2/L3/L4 | 78 |
| 802.1X / MAC auth / Portal | Authentification d'accès | 79 |
| AAA / RADIUS / HWTACACS | Centralisation des comptes | 79 |
| Anti-attaques DoS/ARP/ICMP | Protection CPU et usurpation | 80 |
| Blackhole MAC | Bloque une MAC partout | 80 |
| Apprentissage ARP strict | Anti-épuisement de la table ARP | 80 |

---

## 72. Port security et sticky MAC : un port = N équipements max

**Objectif :** empêcher qu'on branche un hub/switch sauvage (ou qu'on usurpe une
MAC) sur une prise murale. Limite le nombre de MAC apprises par port.

```
system-view
interface GigabitEthernet0/0/5
 port-security enable
 port-security max-mac-num 2          # 2 MAC max (PC + téléphone en daisy-chain)
 port-security mac-address sticky     # mémorise les MAC vues (sticky)
 port-security protect-action shutdown # action si dépassement : shutdown
quit
save
```

**Actions possibles en cas de violation** (à vérifier sur la version exacte) :
`protect` (bloque les nouvelles MAC, log), `restrict` (bloque + alarme),
`shutdown` (coupe le port).

⚠️ Avec `sticky`, les MAC sont mémorisées : si tu **changes le PC** d'une prise,
pense à effacer (`undo port-security mac-address sticky` puis réactiver) sinon
le nouveau PC est bloqué. Documente la procédure pour le support niveau 1.

🔧 Vérifications :

```
display port-security
display mac-address sticky
```

---

## 73. Storm control : plafonner les tempêtes

Même avec STP, une boucle sur un équipement non-STP ou un équipement défaillant
peut inonder le réseau. Le storm control **plafonne** les broadcast, multicast
et unicast inconnu par port.

```
system-view
interface GigabitEthernet0/0/5
 storm-control broadcast min-rate 1000 max-rate 2000    # en pps (exemple)
 storm-control multicast min-rate 1000 max-rate 2000
 storm-control unicast min-rate 1000 max-rate 2000
 storm-control action shutdown                           # ou block
quit
save
```

En série sur tous les ports d'accès :

```
system-view
port-group pg-users
 group-member GigabitEthernet0/0/1 to GigabitEthernet0/0/20
 storm-control broadcast min-rate 1000 max-rate 2000
 storm-control action block
quit
save
```

> Les unités (pps vs kbit/s) et la syntaxe exacte sont **à vérifier sur la version
> logicielle du modèle exact**. Valeurs de départ raisonnables en bureautique :
> broadcast ≤ 1–2 % de la bande passante du port.

✅ **À activer sur TOUS les ports d'accès**, en complément de BPDU guard
(section 47). Les deux ensemble = quasi-immunité aux boucles « de bureau ».

---

## 74. Port isolation : isoler sans VLAN

**Objectif :** empêcher des ports de communiquer **entre eux** en L2, tout en
les laissant joindre l'uplink (ex. : chambres d'hôtel, box clients, bornes
publiques). Plus simple qu'un VLAN par client.

```
system-view
interface GigabitEthernet0/0/5
 port-isolate enable
quit
interface GigabitEthernet0/0/6
 port-isolate enable
quit
# L'uplink (port 28) N'est PAS isolé : les ports isolés le joignent normalement
save
```

Par défaut, tous les ports isolés rejoignent le même **groupe d'isolation** :
ils ne se voient plus entre eux, mais voient tous les ports non isolés.

🔧 Vérification : `display port-isolate group all`.

⚠️ L'isolation est **L2 uniquement** : si un routeur fait du inter-VLAN, deux
ports isolés peuvent se reparler via le routeur (aller-retour L3). Pour une
vraie étanchéité, combine avec des ACL (section 78).

---

## 75. DHCP snooping : traquer les serveurs DHCP pirates

**Le problème :** n'importe quelle box/routeur branché au réseau peut se mettre
à distribuer des adresses DHCP → pannes aléatoires « j'ai Internet puis plus ».

**Principe :** on déclare **trusted** uniquement les ports vers le vrai serveur
DHCP (ou le relais). Les réponses DHCP (OFFER/ACK) arrivant par un port
**untrusted** sont jetées.

```
system-view
dhcp enable
dhcp snooping enable
#
vlan 10
 dhcp snooping enable          # active le snooping dans le VLAN users
quit
#
interface GigabitEthernet0/0/28
 dhcp snooping trusted         # uplink vers le serveur DHCP / le cœur
 description UPLINK_VERS_DHCP
quit
# Les ports 1-20 restent untrusted par défaut : c'est ce qu'on veut
save
```

**Table de binding :** le switch mémorise (IP, MAC, port, VLAN, bail) de chaque
client légitime. C'est cette table qui alimente DAI et IPSG (sections 76–77).

🔧 Vérifications :

```
display dhcp snooping user-bind all
display dhcp snooping statistics
```

⚠️ **Cause n°1 de « plus de DHCP » après activation :** le port vers le vrai
serveur DHCP oublié en `trusted`. Et en cascade : **chaque** switch traversé
doit avoir son uplink en trusted. Teste l'obtention d'un bail **après**
activation, sur chaque VLAN.

⚠️ Si le serveur DHCP est **sur le switch lui-même** (DHCP server local),
adapte : le snooping et le serveur local cohabitent, mais vérifie le
comportement sur ta version (à vérifier sur le modèle exact).

---

## 76. Binding IP + MAC + port + VLAN : la table de vérité

Le datasheet annonce la fonction de **binding de l'adresse IP, de l'adresse MAC,
de l'ID de port et de l'ID de VLAN** (valeur datasheet). Deux sources :

1. **Dynamique** : via DHCP snooping (section 75) — le plus simple.
2. **Statique** : pour les équipements en IP fixe (imprimantes, serveurs).

```
system-view
user-bind static ip-address 192.168.10.50 mac-address 0011-2233-4455 interface GigabitEthernet0/0/23 vlan 10
#  ^ exemple fictif : adapte IP, MAC, port
quit
save
```

🔧 `display dhcp snooping user-bind all` montre les deux types.

✅ **Inventaire vivant :** cette table, c'est ton inventaire IP/MAC/prise à jour
automatique. Exporte-la régulièrement (`display dhcp snooping user-bind all`
dans un fichier) : en cas d'incident, tu sais **qui était où**.

---

## 77. DAI et IPSG : anti-ARP-spoofing et anti-usurpation IP

**DAI (Dynamic ARP Inspection)** : vérifie chaque paquet ARP reçu contre la
table de binding. Un ARP qui ment (IP/MAC incohérents) est jeté. Fini les
« attaques » ARP qui coupent le réseau ou interceptent le trafic.

**IPSG (IP Source Guard)** : filtre **tout le trafic IP** d'un port selon la
table de binding. Un poste qui change d'IP manuellement pour « passer outre »
est bloqué.

```
system-view
# Sur un port d'accès utilisateur (binding dynamique via DHCP snooping)
interface GigabitEthernet0/0/5
 ip source check user-bind enable       # IPSG
quit
save
```

> La disponibilité exacte de DAI (`arp anti-attack` / `dai enable` selon version)
> et d'IPSG sur le S310 est **à vérifier sur la version logicielle du modèle
> exact** — le datasheet annonce le binding et la défense anti-ARP, qui sont les
> briques de base. Teste en maquette avant production.

⚠️ **Ordre de déploiement :** 1) DHCP snooping, 2) vérifie que les bindings se
remplissent, 3) **ensuite seulement** IPSG/DAI. Activer IPSG sans bindings =
couper tout le monde, y compris toi.

⚠️ **Équipements en IP fixe** : sans binding statique (section 76), IPSG les
bloque. Fais la liste des IP fixes **avant** d'activer.

---

## 78. ACL : le filtrage L2/L3/L4

Les ACL du S310 filtrent sur MAC, IP, ports TCP/UDP, VLAN (valeur datasheet :
« TCP/UDP port number, protocol type, or VLAN »).

**Exemple 1 : le VLAN invités (30) ne doit joindre que la passerelle Internet,
pas les autres VLAN.**

```
system-view
acl number 3000
 description ISOLEMENT_GUEST
 rule 10 permit ip source 192.168.30.0 0.0.0.255 destination 192.168.30.1 0
 rule 20 deny ip source 192.168.30.0 0.0.0.255 destination 192.168.0.0 0.0.255.255
 rule 30 permit ip
quit
# Application sur l'interface VLANIF du VLAN invités (là où le routage se fait)
interface Vlanif30
 traffic-filter inbound acl 3000
quit
save
```

**Exemple 2 : protéger le management (complément section 117).**

```
system-view
acl number 2000
 description MGMT_SEUL_ADMIN
 rule 10 permit source 192.168.99.0 0.0.0.255
 rule 20 deny
quit
# Appliquée aux accès distants (http/ssh/snmp) selon la version
quit
save
```

**Règles de survie ACL :**

1. Les règles se lisent **dans l'ordre** : la première qui matche gagne.
2. **Fin implicite = deny** : tout ce qui n'est pas permis est bloqué.
   Termine toujours par un `permit` explicite de contrôle ou assume le deny.
3. Teste **toujours** depuis un poste du VLAN concerné après application.
4. ⚠️ Une ACL sur le VLANIF de management peut **te couper ton propre accès** :
   applique-la en étant en console, ou prévois une fenêtre de rollback.

🔧 Vérifications :

```
display acl 3000
display traffic-filter applied-record
```

---

## 79. 802.1X, MAC auth et portail : l'authentification d'accès

Le S310 annonce **IEEE 802.1X**, authentification MAC, portail, et AAA via
**RADIUS / HWTACACS** (valeur datasheet). Principe : un port ne s'ouvre qu'après
authentification de l'équipement ou de l'utilisateur.

**Exemple 802.1X minimal (avec serveur RADIUS existant) :**

```
system-view
dot1x enable
#
radius-server template RADIUS_CORP
 radius-server shared-key cipher Exemple_Cle_Radius_2026!
 radius-server authentication 192.168.50.10 1812
quit
aaa
 authentication-scheme AUTH_R
  authentication-mode radius
 quit
 domain corp.lan
  authentication-scheme AUTH_R
  radius-server RADIUS_CORP
 quit
quit
#
interface GigabitEthernet0/0/5
 dot1x enable
 dot1x port-method portbased
quit
save
```

> La syntaxe exacte varie selon la version (template RADIUS, `dot1x` vs
> `authentication`) : **à vérifier sur la version logicielle du modèle exact**
> et à tester en maquette avec un vrai serveur (NPS, FreeRADIUS...).

**Quand déployer quoi :**

| Besoin | Solution |
|---|---|
| Postes du domaine, users connus | 802.1X (EAP) |
| Imprimantes, téléphones sans 802.1X | **MAC auth** (MAB) |
| Invités / BYOD ponctuel | Portail captif |
| Rien de tout ça en place | Port security (section 72) en attendant |

⚠️ **Ne déploie pas 802.1X un vendredi à 17h.** Prévois une phase pilote
(quelques ports), un bypass d'urgence documenté, et un moyen de revenir en
arrière port par port.

---

## 80. Protections CPU et anti-attaques : ce qui tourne en fond

Le datasheet annonce : défense contre **DoS** (SYN flood, Land, Smurf, ICMP
flood), attaques **ARP/ICMP**, **apprentissage ARP strict** (anti-épuisement
de la table), CPU defense.

```
system-view
# Exemples de principes (syntaxe à vérifier sur la version exacte)
arp anti-attack entry-check fixed-mac enable
# Limitation du nombre de MAC apprises par port (complément port-security)
quit
save
```

✅ La plupart de ces protections sont **actives par défaut** ou à activer en
une commande. Vérifie leur état (`display cpu-defend statistics`,
`display arp anti-attack statistics` — noms à vérifier sur la version exacte)
lors de ton audit initial.

**Blackhole MAC (bloquer un équipement malveillant partout, immédiatement) :**

```
system-view
mac-address blackhole 00aa-bbcc-ddee vlan 10
quit
save
```

🔧 Pour lever : `undo mac-address blackhole 00aa-bbcc-ddee vlan 10`.
C'est le « bouton rouge » quand tu as identifié une MAC source d'attaque :
rapide, radical, réversible.

---

## 81. LLDP : voir qui est branché où (sans se déplacer)

**LLDP** (Link Layer Discovery Protocol) fait s'annoncer les équipements entre
eux. Le S310 supporte **LLDP et LLDP-MED** (valeur datasheet) — LLDP-MED sert
aussi à la voice VLAN auto avec les téléphones.

```
system-view
lldp enable
quit
save
```

🔧 **La commande la plus rentable du guide :**

```
display lldp neighbor brief
```

Tu vois, port par port : le **nom**, le **modèle** et le **port distant** de
l'équipement en face (switch, AP, téléphone...). Fini le « c'est quoi ce câble
qui part vers le plafond ? ».

✅ **Active LLDP partout, tout le temps.** Coût : nul. Gain : cartographie
automatique du réseau. Tes outils de supervision (et l'app eKit) s'en servent
pour dessiner la topologie.

---

## 82. SNMP : v2c pour commencer, v3 en cible

Le S310 parle **SNMPv1/v2c/v3** (valeur datasheet). v2c = simple mais communauté
en clair ; **v3 = chiffré et authentifié**, l'objectif.

**SNMPv2c (mise en route rapide) :**

```
system-view
snmp-agent
snmp-agent community read cipher Exemple_Communaute_Lecture_2026! mib-view iso-view
snmp-agent sys-info version v2c
quit
save
```

**SNMPv3 (recommandé en production) :**

```
system-view
snmp-agent
snmp-agent sys-info version v3
snmp-agent group v3 GROUP_ADMIN privacy
snmp-agent usm-user v3 USER_SUP privacy
snmp-agent usm-user v3 USER_SUP authentication-mode sha Exemple_Auth_2026!
snmp-agent usm-user v3 USER_SUP privacy-mode aes128 Exemple_Priv_2026!
snmp-agent usm-user v3 USER_SUP group GROUP_ADMIN
quit
save
```

> Les mots de passe ci-dessus sont **fictifs** : génère les tiens (20+ caractères
> aléatoires) et stocke-les au coffre. La syntaxe exacte USM est **à vérifier sur
> la version logicielle du modèle exact**.

**Ce qu'on supervise en priorité :** état des ports, erreurs/compteurs, CPU,
mémoire, température, état PoE, traps de changement d'état.

🔧 Test depuis le superviseur : `snmpwalk -v3 ... sysDescr`. Si ça ne répond pas :
ACL (section 78), communauté, pare-feu, ou `snmp-agent` oublié.

---

## 83. Syslog : centraliser les logs (sinon ils ne servent à rien)

Les logs en local (`display logbuffer`) sont écrasés par rotation. Envoie-les
vers un **serveur syslog** (souvent le superviseur lui-même).

```
system-view
info-center enable
info-center loghost 192.168.50.20 facility local7
info-center source default channel 2 log level informational state on
quit
save
```

🔧 Vérifications :

```
display info-center
display logbuffer
```

**Niveaux utiles :** `debugging` (trop verbeux en prod), `informational`
(le bon compromis), `warning` (alertes), `critical`.

✅ **À faire dès le jour 1**, avec le NTP (section 24) : des logs horodatés et
centralisés, c'est la moitié du dépannage. Sans ça, tu enquêtes à l'aveugle.

---

## 84. Supervision via eKit cloud : ce que ça apporte

- **Vue topologique** auto (via LLDP), état des équipements, alertes.
- **Inspection intelligente** : l'app propose des diagnostics guidés.
- **Montées de version** pilotées (Smart Upgrade, section 102).
- Alertes (port down, PoE en surcharge, température...) remontées en push.

⚠️ Le cloud **complète** mais ne **remplace** pas SNMP/syslog locaux : si
Internet tombe, tu perds la supervision cloud mais pas la locale. En site
critique : les deux.

---

## 85. Port mirroring : capturer le trafic pour Wireshark

Pour analyser un problème applicatif (paquets perdus, DHCP qui ne répond pas),
on **miroite** un port vers un port d'analyse où tourne Wireshark.

```
system-view
observe-port 1 interface GigabitEthernet0/0/24   # port d'analyse (PC + Wireshark)
quit
interface GigabitEthernet0/0/5                   # port à observer
 port-mirroring to observe-port 1 inbound
quit
save
```

> Syntaxe (`observe-port`, `port-mirroring`) **à vérifier sur la version
> logicielle du modèle exact**.

⚠️ **Règles :**
- Le port d'analyse ne sert **qu'à ça** pendant la capture (pas de production).
- Ne miroite pas un uplink 10G vers un port 1G : tu perds des paquets.
- **Retire le mirroring après** (`undo`) : ça consomme des ressources et peut
  fausser les stats.

---

## 86. VCT : le testeur de câble intégré (virtual cable test)

Un port qui monte/descend, un débit anormal : avant de changer le câble dans le
faux plafond, fais un **VCT** — le switch mesure la longueur et détecte les
défauts par paire.

```
test cable GigabitEthernet0/0/5
```

Résultat type : longueur par paire, état (OK / ouvert / court-circuit /
désadapté), distance du défaut.

⚠️ Le test **coupe brièvement** le lien : à faire en heure creuse ou sur un port
déjà en panne. Disponibilité **à vérifier sur la version du modèle exact**.

✅ **Usage pro :** VCT systématique quand tu brasses une nouvelle prise avant de
déclarer « la prise est morte ». 50 % des « prises mortes » sont des jarretières
ou des ports du switch mal configurés.

---

## 87. Statistiques de ports et RMON : les compteurs qui parlent

```
display interface GigabitEthernet0/0/5
```

Regarde : **input/output errors, CRC, collisions, giants, runts**. Quelques
erreurs au boot = normal. Des compteurs qui **augmentent en continu** = problème
(câble, duplex, carte réseau distante).

**Remise à zéro pour mesurer :**

```
reset counters interface GigabitEthernet0/0/5
# ... attends 10 minutes de trafic réel ...
display interface GigabitEthernet0/0/5
```

Le S310 supporte aussi **RMON** et la **collecte de statistiques de trafic**
(valeur datasheet) pour l'historique via SNMP.

🔧 **Checklist « port malade » :**
- CRC/errors qui montent → câble ou jarretière (VCT, section 86).
- ` Giants` → MTU / équipement qui envoie des trames surdimensionnées.
- Débit négocié à 100M au lieu de 1G → câble (paires manquantes) ou
  négociation forcée d'un côté.

---

## 88. Sauvegarde de la configuration : 3 méthodes

**Méthode 1 — Fichier local (le minimum) :**

```
save                          # config active → flash (vrpcfg.zip)
display saved-configuration   # vérifie que c'est bien sauvegardé
```

**Méthode 2 — Export vers un serveur (l'automatique) :**

```
# Via TFTP/FTP/SFTP selon la version (à vérifier sur le modèle exact)
tftp 192.168.50.20 put vrpcfg.zip backup/SW-ACC-01_2026-09-27.zip
```

**Méthode 3 — Copier-coller (le rustique qui sauve) :**

```
display current-configuration
```
→ copie tout dans un fichier texte daté. En cas de remplacement urgent, tu
recolles la config sur le switch neuf en 10 minutes.

✅ **Politique :** sauvegarde **automatique hebdo** (script + TFTP/SFTP) +
sauvegarde **manuelle avant chaque changement**. Rétention : 3 mois minimum.
Teste la **restauration** une fois (section 89) — une sauvegarde jamais testée
n'est pas une sauvegarde.

---

## 89. Restauration de configuration : procédure

1. Transfère le fichier de sauvegarde sur le switch (TFTP/FTP, clé USB si
   supporté — à vérifier sur le modèle exact).
2. Deux approches :
   - **Remplacement du fichier de démarrage** puis reboot (le plus propre).
   - **Rejeu en CLI** : colle la config (méthode 3) — attention aux dépendances
     d'ordre (créer les VLAN avant de les affecter).
3. `save`, reboot, vérifie : `display current-configuration` vs fichier.
4. ✅ **Teste toujours la restauration sur un switch de maquette** avant d'en
   avoir besoin en urgence à 2h du matin.

⚠️ Restaurer une config d'un **autre modèle** (ex. 24P4S → 48P4S) : les numéros
de ports ne correspondent pas. Relis et adapte à la main, ne colle pas aveuglément.

---

## 90. Architecture des fichiers : vrpcfg, système, patches

```
dir flash:/
display startup
```

- **Fichier de configuration** : `vrpcfg.zip` (config de démarrage).
- **Fichier système** : l'image logicielle (`.cc` / nom selon version).
- `display startup` montre **quel** fichier système et **quelle** config seront
  utilisés au prochain boot — **vérifie toujours après une mise à jour**.

⚠️ Deux images système peuvent coexister en flash : c'est ce qui permet le
**rollback** (section 91). Ne supprime jamais l'ancienne image avant d'avoir
validé la nouvelle en production pendant au moins une semaine.

---

## 91. Mise à jour firmware : procédure complète (Smart Upgrade + manuel)

**Option A — Smart Upgrade via HOUP (recommandé si Internet OK) :**
La plateforme HOUP (Huawei Online Upgrade Platform) calcule le **chemin de mise
à jour standardisé**, pré-charge la nouvelle version, et propose une mise à jour
en un clic (valeur datasheet). Le Perpetual PoE maintient l'alimentation des
équipements pendant l'opération (section 59).

**Option B — Manuelle (classique, sans Internet) :**

1. **Sauvegarde** la config (section 88) — non négociable.
2. Télécharge l'image depuis le support Huawei (via ton revendeur / compte).
   **Vérifie la compatibilité modèle + version** (notes de release).
3. Transfère l'image sur le switch (FTP/SFTP/TFTP).
4. Déclare la nouvelle image comme image de démarrage :
   ```
   startup system-software <nom-image.cc>
   display startup          # VÉRIFIE : la bonne image est marquée "next startup"
   ```
   > La syntaxe exacte (`startup system-software`) est **à vérifier sur la
   > version logicielle du modèle exact**.
5. `save`, puis **reboot** en heure creuse : `reboot`.
6. Après reboot : `display version` → la nouvelle version est active.
7. **Batterie de tests** : ports up, VLAN, PoE, DHCP, un ping de chaque VLAN,
   accès web/SSH. Ne déclare pas « c'est bon » avant.
8. Garde l'ancienne image en flash pendant 1 à 2 semaines.

⚠️ **Fenêtre de maintenance :** même avec Perpetual PoE, le plan de commutation
est interrompu pendant le reboot. Jamais de mise à jour en pleine journée sans
validation écrite du responsable.

---

## 92. Rollback : revenir en arrière quand ça tourne mal

**Scénario :** la nouvelle version a un bug (fonction X qui ne marche plus).

1. En console/SSH : redéclare l'**ancienne image** comme image de démarrage
   (`startup system-software <ancienne-image>`).
2. `display startup` pour vérifier.
3. `reboot`.
4. Vérifie la version et refais la batterie de tests (section 91, étape 7).
5. Restaure la config sauvegardée **avant** la mise à jour si la nouvelle
   version l'a modifiée/migrée (ça arrive : compare les fichiers).

✅ **C'est pour ça qu'on garde l'ancienne image** (section 90). Un rollback sans
image de secours = réinstallation complète = heures d'arrêt.

---

## 93. Reset usine : procédure et précautions

**Quand :** switch à reconditionner, config irrécupérable, changement de site.

**Méthode CLI (à vérifier sur la version exacte) :**

```
reset saved-configuration
reboot
# Au reboot : [Y/N] → Y pour effacer
```

**Méthode bouton RST :** appui long (~5–10 s) jusqu'à changement de la LED SYS
(durée exacte **à vérifier sur le guide du modèle exact**).

⚠️ **Irréversible.** Avant : sauvegarde la config (section 88) — même si tu
penses ne plus en avoir besoin. Après : refais toute la checklist de première
configuration (section 26), **y compris le mot de passe admin**.

---

## 94. iStack : le principe (stacking intelligent)

**iStack** permet de regrouper **jusqu'à 4 switches de la même série** en **un
seul switch logique** (valeur datasheet) :
- **Une seule IP** de management, **une seule config** pour tout le stack.
- Les liens inter-switch servent aussi à l'**agrégation inter-équipements**
  (un Eth-Trunk avec un membre sur chaque switch = redondance matérielle).
- Si un membre tombe, les autres continuent (redondance).

**Prérequis impératifs :**
- Même **modèle** (même série S310), même **version logicielle** sur tous.
- Câblage des ports de stack **avant** la mise sous tension du stack.
- Certains modèles ont **2 ports de stack dédiés** (pas de config nécessaire,
  les uplinks restent libres) — présence **à vérifier sur la fiche du modèle exact**.

---

## 95. iStack : configuration pas à pas

**Topologie :** 2× S310-24P4S en anneau (chaque switch relié aux deux voisins
par les ports de stack — en anneau pour 3–4 membres, en chaîne simple pour 2).

**Sur le futur master (SW-1) :**

```
system-view
stack slot 0 priority 200          # priorité la plus haute = master
quit
save
# Câble les ports de stack vers SW-2 (ports dédiés ou ports business configurés)
```

**Sur SW-2 :**

```
system-view
stack slot 1 priority 100
quit
save
```

Puis **éteins les deux**, câble les ports de stack, **allume le master d'abord**,
puis le second. Le stack se forme automatiquement ; le second adopte la config
du master.

> La syntaxe exacte (`stack slot`, `stack priority`, ports de stack à déclarer
> via `stack-port`) est **à vérifier sur la version logicielle du modèle exact** :
> elle varie selon que les ports de stack sont dédiés ou non.

🔧 Vérifications :

```
display stack
display stack topology
```

Tu dois voir les 2 membres, leurs rôles (master/standby/slave), et les liens
de stack UP.

---

## 96. iStack : vérifications, pannes et bonnes pratiques

**Pannes classiques :**

| Symptôme | Cause probable | Action |
|---|---|---|
| Le 2e switch ne rejoint pas | Versions logicielles différentes | Aligne les versions **avant** |
| Stack instable, bascules | Câble de stack défectueux / port business mal déclaré | Teste les câbles, vérifie `display stack topology` |
| Split-brain (2 masters) | Lien de stack coupé | **Prévois un lien de secours** (anneau, pas chaîne) ou la détection de conflit (MAD/dual-active detect si dispo — à vérifier) |
| Config du membre effacée | Normal : le slave adopte la config du master | Sauvegarde la config du slave **avant** de le stacker |

**Bonnes pratiques :**

1. **Topologie en anneau** dès 3 membres (pas de point unique de défaillance).
2. Même version logicielle **partout**, vérifiée avant chaque ajout.
3. Étiquette les câbles de stack (ils ne doivent **jamais** être débranchés
   « pour tester autre chose »).
4. En cas d'ajout d'un membre : éteins-le, câble, allume — jamais de branchement
   « à chaud » sur un stack en production sans procédure.

---

## 97. Cascading : l'alternative quand on ne stacke pas

Si tu ne veux/peux pas stacker (modèles différents, versions différentes,
simplicité), relie les switches en **cascade** : uplink du switch 2 vers un
port du switch 1, en **trunk** avec RSTP.

```
# Sur SW-1, port vers SW-2 :
interface GigabitEthernet0/0/28
 port link-type trunk
 port trunk allow-pass vlan 10 20 30 50 99
 description CASCADE_VERS_SW2
quit
# RSTP actif des deux côtés (sections 44-45) : la redondance éventuelle est gérée
save
```

**Stack vs cascade :**

|  | iStack | Cascade |
|---|---|---|
| Management | 1 IP, 1 config | 1 IP par switch |
| Redondance inter-switch | Oui (trunk inter-membres) | Oui si double lien + RSTP |
| Complexité | Plus élevée (versions, câblage dédié) | Faible |
| Quand | Baie unique, besoin de redondance matérielle | Sites simples, modèles mixtes |

✅ **En PME standard : cascade + RSTP suffit largement.** Le stack se justifie
quand tu veux de l'agrégation inter-châssis ou une gestion unifiée poussée.

---

## 98. Dépannage : la méthode (à appliquer avant chaque cas ci-dessous)

1. **Définis le symptôme précisément :** qui, quoi, depuis quand, quoi qui a changé.
   « Ça ne marche plus » n'est pas un symptôme. « Le PC du bureau 8 n'a plus
   d'IP depuis ce matin 8h, après le brassage d'hier soir » en est un.
2. **Console ou SSH d'abord :** `display interface brief`, `display logbuffer`,
   `display clock` (l'heure est-elle bonne ?).
3. **Isole la couche :** physique (LED, câble) → lien (négociation) → VLAN →
   IP/DHCP → routage → applicatif. Ne saute pas d'étape.
4. **Un changement à la fois**, et note ce que tu fais. Deux changements
   simultanés = tu ne sauras jamais lequel a réparé (ou cassé).
5. **Vérifie ce qui a changé** : config récente (`display logbuffer`,
   historique), travaux, coupure électrique, nouvel équipement branché.
6. Si tu sèches après 30 minutes : **escalade** avec les infos collectées
   (logs, `display`...), pas avec « ça marche pas ».

🔧 **Le kit de survie du dépanneur :** câble console + adaptateur USB, 2
jarretières testées bonnes, un PC portable avec PuTTY/Wireshark, la doc
d'adressage du site, et ce guide.

---

## 99. Cas n°1 — Boucle réseau : tout le réseau est à genoux

**Symptômes :** plus rien ne répond (ou par intermittence), toutes les LED des
ports clignotent frénétiquement en même temps, le switch est injoignable ou
très lent.

**Diagnostic :**
1. En console : `display interface` → compteurs qui explosent, CPU à 100 %
   (`display cpu-usage`).
2. `display logbuffer` → tempête de changements d'état STP si STP est actif.
3. Cherche le coupable : **quoi de neuf ?** Un petit switch de bureau branché
   avec 2 câbles ? Un câble qui boucle deux prises murales ? Des travaux ?

**Solution :**
1. **Débranche les liens suspects un par un** jusqu'à ce que le réseau
   reparte (commence par les ajouts récents).
2. Une fois stable : active **RSTP** (section 44) + **BPDU guard** (47) +
   **storm control** (73) + **edge ports** (46) sur tous les ports utilisateurs.
3. Retrouve la boucle physique et supprime-la (ou transforme-la en vraie
   redondance avec RSTP, section 49).
4. Documente l'incident : cause, port, équipement, heure.

✅ **Prévention :** cette panne ne doit arriver **qu'une fois**. Après, les
protections ci-dessus sont obligatoires sur tous les sites.

---

## 100. Cas n°2 — Le VLAN ne passe pas sur le trunk

**Symptômes :** les PC du VLAN 20 ne se voient pas d'un switch à l'autre (ou ne
joignent pas la passerelle), alors que le VLAN 10 fonctionne sur le même lien.

**Diagnostic :**
1. Des deux côtés : `display port vlan <interface-trunk>` → le VLAN 20 est-il
   dans `allow-pass` **des deux côtés** ?
2. `display vlan 20` → le VLAN existe-t-il **des deux côtés** ?
3. PVID identique des deux côtés ?

**Solution :** ajoute le VLAN manquant :
```
interface GigabitEthernet0/0/28
 port trunk allow-pass vlan 10 20 30 50 99
```
**des deux côtés**, puis `save`.

⚠️ C'est **la** panne VLAN la plus fréquente. Le réflexe `display port vlan`
des deux côtés la résout en 2 minutes.

---

## 101. Cas n°3 — Le PoE ne monte pas

**Symptômes :** AP/caméra/téléphone éteint, LED PoE éteinte ou orange.

**Diagnostic (dans l'ordre) :**
1. `display poe interface <port>` : PoE activé ? Budget restant ?
   (`display poe power-state`).
2. `display poe interface` : quelle **classe** détectée ? Classe 0 inattendue =
   pas de négociation.
3. Teste l'équipement sur un **autre port** PoE.
4. Teste avec une **jarretière courte** (câble trop long ou paires coupées).
5. `display logbuffer | include POE` : cause (overload, short, etc.).

**Solution :** selon le diagnostic — réactiver `poe enable`, libérer du budget
(reprioriser, section 61), changer le câble, ou remplacer l'équipement.
Si la caméra **démarre puis s'éteint** : limite `poe power` trop basse ou budget
tout juste (pics IR la nuit) → augmente la marge.

---

## 102. Cas n°4 — Port en err-disable / coupé par une protection

**Symptômes :** un port (ou plusieurs) passe down sans raison physique évidente ;
les logs parlent de BPDU, de storm, de port-security.

**Diagnostic :** `display logbuffer` → la **cause exacte** (BPDU protection,
storm-control action shutdown, port-security violation...).

**Solution :**
1. **Retire la cause** : switch sauvage débranché, équipement qui spamme
   déconnecté, MAC légitime ajoutée au sticky...
2. Réarme le port :
   ```
   interface GigabitEthernet0/0/5
    shutdown
    undo shutdown
   ```
3. Si ça recommence immédiatement : la cause est toujours là. Ne force pas en
   désactivant la protection « pour voir » — sauf en dernier recours documenté.

✅ **Ne désactive jamais une protection pour masquer une panne.** Elle a coupé
pour une raison ; trouve-la.

---

## 103. Cas n°5 — DHCP snooping bloque tout le monde

**Symptômes :** après activation du DHCP snooping, **personne** n'obtient d'IP
(ou un seul VLAN est touché).

**Diagnostic :**
1. `display dhcp snooping user-bind all` → vide ou incomplet ?
2. Le port vers le **serveur DHCP** est-il en `dhcp snooping trusted` ?
   **Sur chaque switch traversé** (cascade !).
3. Le snooping est-il activé **dans le bon VLAN** ?

**Solution :**
```
interface GigabitEthernet0/0/28   # vers le serveur DHCP
 dhcp snooping trusted
```
+ vérifie la chaîne complète. Teste l'obtention d'un bail **par VLAN** après
chaque correction.

⚠️ **En cascade :** le DHCP traverse SW-2 puis SW-1 : les **deux** uplinks
doivent être trusted. C'est l'oubli classique.

---

## 104. Cas n°6 — Débit de l'uplink saturé

**Symptômes :** lenteurs généralisées, surtout à certaines heures ; tout est
« up » mais tout rame.

**Diagnostic :**
1. `display interface <uplink>` → utilisation proche de 100 % en continu ?
2. `display port statistics` / compteurs : quel port génère le trafic ?
   (Miroir + Wireshark, section 85, pour identifier : sauvegarde cloud ? Caméra
   en haute résolution ? Boucle partielle ?)
3. Heure de pointe corrélée à une activité (sauvegardes à 14h ?).

**Solution :**
- Court terme : déplace les sauvegardes en heure creuse, baisse la résolution
  des caméras si inutilement haute.
- Structurel : passe l'uplink en **10G** (modèle X, section 5) ou en
  **Eth-Trunk** (section 55).
- Vérifie qu'il n'y a pas de **boucle partielle** (storm control, section 73).

---

## 105. Cas n°7 — Plus d'accès web au switch

**Symptômes :** `https://<ip>` ne répond plus (timeout), alors que le ping passe
(ou pas).

**Diagnostic :**
1. Le ping passe ? Non → problème IP/routage/VLAN (revois sections 20, 36).
   Oui → le serveur web est en cause.
2. En console/SSH : `display http server` (ou équivalent) → HTTP/HTTPS activés ?
3. Une **ACL** (section 78) bloque-t-elle ton poste ?
4. Certificat expiré ou navigateur qui bloque ? Teste un autre navigateur / en
   navigation privée.

**Solution :** réactive (`http secure-server enable`), corrige l'ACL, ou
repasse par la console pour diagnostiquer. **Toujours garder l'accès console
possible** : c'est la porte de secours quand le réseau est en vrac.

---

## 106. Cas n°8 — Mot de passe admin oublié

**Symptômes :** impossible de se connecter (console/web/SSH), personne n'a le
mot de passe.

**Diagnostic :** c'est bien un oubli, pas un piratage ? (Vérifie avec le
responsable avant toute manip.)

**Solution :**
- Via la console, procédure de **récupération** (mode BootROM / reset du mot de
  passe) : la procédure exacte est **à vérifier sur le guide du modèle exact**
  (elle varie selon les versions et peut nécessiter un redémarrage).
- En dernier recours : **reset usine** (section 93) + restauration de la
  sauvegarde (section 89) — d'où l'importance des sauvegardes !
- Après : mot de passe au **coffre** (section 117), procédure écrite.

✅ **Prévention :** 2 comptes admin nominatifs (pas de compte partagé), mots de
passe au coffre d'entreprise, test de récupération une fois par an.

---

## 107. Cas n°9 — Le stack ne se forme pas

**Symptômes :** le 2e switch reste indépendant, ou le stack se forme puis casse.

**Diagnostic :** `display stack` / `display stack topology` sur les deux.
1. **Versions logicielles identiques ?** Non → c'est (presque) toujours ça.
2. Câbles de stack bien branchés sur les **bons ports** (dédiés ou déclarés) ?
3. Priorités cohérentes (un seul master) ?
4. Le 2e switch a-t-il été **éteint puis rallumé après** câblage ?

**Solution :** aligne les versions **avant** (section 91), vérifie le câblage,
respecte l'ordre (master d'abord). Voir le tableau des pannes section 96.

---

## 108. Cas n°10 — Module SFP non reconnu / lien fibre down

**Symptômes :** pas de lien sur l'uplink fibre, LED SFP éteinte, ou lien qui
tombe par intermittence.

**Diagnostic :**
1. `display interface` : le module est-il **détecté** (transceiver info) ?
   Non → compatibilité (module non codé Huawei / tiers bas de gamme).
2. `display transceiver` : niveaux **Tx/Rx** (dBm). Rx trop faible (< -20 dBm
   typique) → fibre sale, cassée, ou trop longue pour le module.
3. Les **deux extrémités** ont-elles le même type (SX↔SX, LX↔LX) ? Un SX face à
   un LX ne marchera jamais.
4. Fibres **croisées** (Tx→Rx) ? Avec des jarretières duplex, vérifie le sens.

**Solution :** nettoie les connecteurs (stylo fibre), remplace la jarretière,
change le module pour un compatible, vérifie le budget optique. **N'achète que
des modules compatibles vérifiés** (section 16).

---

## 109. Cas n°11 — Débit anormal sur un port cuivre (100M au lieu de 1G)

**Symptômes :** un poste rame, `display interface` montre 100Mbit/s négociés.

**Diagnostic :**
1. **Câble** : 4 paires nécessaires pour le gigabit. Un câble abîmé (paires
   4-5/7-8 coupées) tombe en 100M. → VCT (section 86).
2. Négociation **forcée** d'un côté (100M full forcé côté PC, auto côté switch)
   → duplex mismatch possible. **Remets l'auto-négociation des deux côtés.**
3. Jarretière ou prise murale défectueuse → teste en direct au switch.

**Solution :** remplace le câble/jarretière, repasse en auto. **Ne force jamais
la vitesse/duplex** sauf équipement industriel qui l'exige (et alors des deux
côtés, documenté).

---

## 110. Cas n°12 — Téléphone IP : pas de voix / pas de VLAN voix

**Symptômes :** le téléphone s'allume (PoE OK) mais n'obtient pas d'IP voix, ou
reste dans le VLAN data.

**Diagnostic :**
1. `display voice-vlan status` : voice VLAN actif sur le port ?
2. `display mac-address vlan 20` : la MAC du téléphone y apparaît-elle ?
   Non → **OUI MAC** du constructeur non déclaré (section 38) : récupère les
   3 premiers octets de la MAC (`display mac-address interface ...`) et
   ajoute l'OUI.
3. Le téléphone est-il en mode VLAN **auto/LLDP-MED** ? (Menu du téléphone —
   à vérifier sur le modèle exact.)
4. Port en **hybrid** avec `tagged vlan 20` ? (Pas en access !)

**Solution :** ajoute l'OUI, vérifie le mode hybrid, teste avec un téléphone
dont l'OUI est déjà déclaré pour isoler (téléphone vs switch).

---

## 111. Cas n°13 — 802.1X rejette tout le monde

**Symptômes :** après activation, aucun poste ne passe (ou certains seulement).

**Diagnostic :**
1. Le **serveur RADIUS** est-il joignable ? (`ping`, `display radius-server`.)
2. **Clé partagée** identique des deux côtés ? (Erreur n°1.)
3. Les postes ont-ils un **supplicant** configuré (Windows : service « Config
   automatique de réseau câblé ») ?
4. Logs RADIUS côté serveur : que dit le rejet ? (mauvais login, certificat...)

**Solution :** corrige la clé, vérifie le réseau vers le RADIUS, déploie en
**mode pilote** (quelques ports) avant généralisation. **Prévois le bypass
d'urgence** : procédure écrite pour désactiver 802.1X port par port en cas de
panne du RADIUS.

⚠️ Un RADIUS en panne + 802.1X strict = **tout le site coupé**. Soit un RADIUS
redondé, soit un mode « fail-open » documenté et assumé (à vérifier sur la
version exacte).

---

## 112. Cas n°14 — Mise à jour firmware qui échoue

**Symptômes :** le transfert échoue, ou le switch ne boote pas sur la nouvelle
image, ou des fonctions ne marchent plus après.

**Diagnostic :**
1. Image **compatible** avec le modèle exact ? (Notes de release lues ?)
2. Transfert **complet** ? (Taille du fichier en flash = taille attendue.)
3. `display startup` : la **bonne** image est-elle marquée pour le prochain boot ?
4. Assez de **place en flash** ? (`dir flash:/`.)

**Solution :** re-transfère (en binaire, pas en ASCII !), libère de la place,
re-déclare l'image, reboot. Si dysfonctionnement après boot : **rollback**
(section 92). **Jamais de mise à jour sans sauvegarde + image de secours.**

---

## 113. Cas n°15 — Ventilateur bruyant / surchauffe

**Symptômes :** bruit anormal (sifflement, claquement), LED d'alarme
température, ou switch qui reboot tout seul par forte chaleur.

**Diagnostic :**
1. `display temperature` / `display fan` (noms à vérifier sur la version exacte) :
   température, vitesse, état des ventilateurs.
2. Environnement : local à combien ? Dégagements respectés (section 12) ?
   Poussière ?
3. Un ventilateur **bloqué** (poussière, câble qui touche) ?

**Solution :** dépoussière (bombe d'air sec, switch **éteint**), dégage les
aérations, baisse la température du local. Si un ventilateur est HS : les
ventilateurs du S310 sont **intégrés non remplaçables** (valeur datasheet :
« built-in ») → c'est un **retour SAV**. D'où l'importance du **switch de
secours** (section 116).

⚠️ Un switch qui reboot par chaleur **abîme** le matériel à chaque cycle.
Ne laisse pas traîner : déplace-le ou ventile le local.

---

## 114. Cas n°16 — Intermittences inexpliquées (le pire)

**Symptômes :** coupures brèves aléatoires, sur un ou plusieurs ports, sans
cause évidente. Le cauchemar du dépanneur.

**Méthode (ordonnée) :**
1. **Heure exacte** des coupures (logs + témoignages) : corrélée à quelque
   chose ? (Clim qui démarre, machine industrielle, onduleur qui commute...)
2. `display interface` : **compteurs d'erreurs** sur les ports touchés.
   Erreurs qui montent = couche 1 (câble, interférences).
3. **Alimentation** : micro-coupures ? Le switch est-il sur onduleur ?
   (`display logbuffer` : reboots inexpliqués ?)
4. **Boucle partielle** : un équipement qui renvoie du trafic par intermittence
   (borne Wi-Fi en mode répéteur sauvage, double branchement).
5. **Surchauffe** cyclique (clim coupée la nuit/week-end ?).
6. Isole : déplace l'équipement sur un autre port/switch pour voir si le
   problème **suit l'équipement** ou **reste au port**.

✅ **Documente tout**, même les fausses pistes. Les intermittences se résolvent
par élimination, et l'historique écrit vaut de l'or quand le problème revient
3 mois plus tard.

---

## 115. Cas n°17 — L'AP Wi-Fi ne diffuse plus après un redémarrage du switch

**Symptômes :** après reboot/mise à jour, un ou plusieurs AP restent éteints ou
ne redémarrent pas.

**Diagnostic :**
1. Le PoE est-il revenu ? (`display poe interface`.) Avec le **Perpetual PoE**,
   il n'aurait pas dû couper (section 59) — sauf coupure **électrique** réelle.
2. **Budget** : au redémarrage, tous les AP appellent en même temps (pic) →
   si le budget est tout juste, certains ne montent pas. `display poe
   power-state` juste après boot.
3. L'AP a-t-il perdu sa config ? (Problème côté AP/contrôleur, pas switch.)

**Solution :** `poe power-off` / `poe power-on` ciblé sur les AP récalcitrants,
augmente la marge du budget (section 62), vérifie les priorités (section 61).
Si l'AP a perdu sa config : re-provisionne via l'app eKit.

---

## 116. Cas n°18 — Tempête de broadcast venant d'un équipement défaillant

**Symptômes :** comme une boucle (section 99) mais **sans** boucle physique :
un équipement (caméra HS, carte réseau défaillante, boucle logicielle dans un
AP) inonde le réseau.

**Diagnostic :**
1. `display interface` : **un** port avec un compteur d'output/input anormalement
   élevé par rapport aux autres = le suspect.
2. Miroir (section 85) + Wireshark : quel type de trafic ? (ARP en boucle ?
   Broadcast UDP ?)
3. `display mac-address` : une MAC qui « flappe » entre plusieurs ports ?

**Solution :** `shutdown` le port suspect → le réseau repart → isole et remplace
l'équipement. Puis active le **storm control** (section 73) pour que la prochaine
fois, le port se coupe tout seul.

✅ **Le storm control transforme une panne réseau totale en panne d'un seul
port.** C'est exactement pour ça qu'on l'active en préventif.

---

## 117. Maintenance préventive : le plan annuel

Un switch ne demande presque rien — jusqu'au jour où il demande tout. Le plan
ci-dessous coûte quelques heures par an et évite la plupart des pannes.

| Fréquence | Actions |
|---|---|
| **Mensuel** (10 min, à distance) | `display interface brief` (ports down anormaux), `display poe power-state` (budget), `display logbuffer` (erreurs récentes), `display cpu-usage` / mémoire, vérif alertes eKit |
| **Trimestriel** (30 min) | Sauvegarde config hors site (section 88), vérif NTP/heure, contrôle température locale, revue des ports inutilisés (à désactiver), mise à jour de la doc d'adressage |
| **Semestriel** (1 h) | Dépoussiérage (bombe d'air sec, switch éteint si très poussiéreux), vérif serrage terre et cordons, test d'un port de secours, revue des ACL et comptes (départs ?), test de restauration d'une sauvegarde sur maquette |
| **Annuel** (demi-journée) | Audit complet : firmware à jour ? (section 91), mots de passe renouvelés, inventaire physique vs doc, test du switch de secours (il boote ? sa config est à jour ?), exercice de panne (débranche un uplink : la redondance marche-t-elle vraiment ?), revue du plan avec l'équipe |

✅ **Règle :** chaque visite donne lieu à un **compte-rendu daté** d'une page
(état, actions, anomalies, prochaine échéance). Sans écrit, la maintenance
n'existe pas.

---

## 118. Checklist de visite sur site (à imprimer)

- [ ] État visuel : LED PWR/SYS vertes, pas de LED rouge, bruit de ventilateur normal
- [ ] Température du local : ___ °C (cible < 35 °C)
- [ ] Dépoussiérage fait (date : ___)
- [ ] Terre vérifiée (serrage visuel)
- [ ] `display interface brief` : aucun port down inexpliqué
- [ ] `display poe power-state` : budget dispo ___ W (alerte si < 20 %)
- [ ] `display logbuffer` : erreurs depuis la dernière visite ? ___
- [ ] Heure correcte : `display clock` → ___
- [ ] Sauvegarde config exportée (fichier : ___)
- [ ] Étiquetage ports à jour (nouveaux équipements décrits ?)
- [ ] Switch de secours : présent, config à jour (date : ___)
- [ ] Prochaine visite prévue le : ___

---

## 119. Pièces de rechange : le stock minimum

| Pièce | Quantité conseillée | Pourquoi |
|---|---|---|
| Switch S310 identique (ou modèle supérieur) | 1 pour ~10 switches en prod | Un ventilateur intégré HS = SAV = jours sans réseau sinon |
| Modules SFP/SFP+ compatibles | 2 par type utilisé | L'uplink fibre est un point unique de défaillance |
| Jarretières fibre + cuivre | assortiment | 80 % des pannes couche 1 |
| Cordon secteur + sangle | 1–2 | Ça disparaît mystérieusement |
| Câble console + adaptateur USB | 2 kits | La porte de secours |
| Ventilateur externe / clim d'appoint | selon local | Canicule : un switch qui chauffe, c'est un switch qui meurt |

✅ **Le switch de secours doit avoir :** la **même version logicielle** que la
prod (ou être mis à jour avant stockage) et une **copie récente de la config**
du switch qu'il remplace. Un spare avec un firmware de 3 ans d'écart = 2 heures
de mise à jour le jour J. Vérifie-le **une fois par an** (il boote ?).

---

## 120. Durcissement (1/4) : comptes et mots de passe

1. **Compte admin nominatif par personne**, pas de compte « admin » partagé :
   ```
   system-view
   aaa
    local-user TECH_MARIE password cipher Exemple_MotDePasse_Fictif_2026!
    local-user TECH_MARIE privilege level 3
    local-user TECH_MARIE service-type ssh https
   quit
   save
   ```
   > Identifiants **fictifs** : génère des mots de passe uniques de 16+ caractères
   > au coffre (Bitwarden, Keepass...).
2. **Niveau de privilège** : 0 (visite) à 3 (admin) — donne le minimum nécessaire.
   Le stagiaire n'a pas besoin du niveau 3.
3. **Désactive les comptes inutilisés** immédiatement au départ d'un technicien.
4. **Renouvelle** les mots de passe admin au moins une fois par an (et après
   chaque départ).
5. ⚠️ Ne stocke jamais un mot de passe **en clair** dans un fichier partagé, un
   post-it ou un ticket. Coffre d'entreprise, point.

---

## 121. Durcissement (2/4) : SSH only, bye-bye Telnet et HTTP

```
system-view
# --- SSH actif, Telnet interdit ---
stelnet server enable
undo telnet server enable
# --- HTTPS actif, HTTP pur coupé ---
http secure-server enable
undo http server enable
# --- Timeout de session : pas de console laissée ouverte ---
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound ssh
 idle-timeout 10 0
quit
user-interface console 0
 idle-timeout 10 0
quit
save
```

> La syntaxe exacte (`stelnet server enable`, `protocol inbound ssh`) est
> **à vérifier sur la version logicielle du modèle exact**.

✅ **Après :** teste que SSH fonctionne **avant** de fermer ta session console.
Te couper ton propre accès distant, c'est le bizutage classique.

---

## 122. Durcissement (3/4) : restreindre l'accès au management

**Principe :** l'administration (SSH/HTTPS/SNMP) n'est joignable que depuis le
VLAN/poste d'administration, jamais depuis les VLAN utilisateurs ou invités.

```
system-view
acl number 2000
 description ACCES_MGMT_ADMIN_ONLY
 rule 10 permit source 192.168.99.0 0.0.0.255     # VLAN management
 rule 20 deny
quit
# Application aux services (selon version : http / ssh / snmp)
http acl 2000
ssh server acl 2000
snmp-agent acl 2000
quit
save
```

> L'application d'ACL aux services (`http acl`, `ssh server acl`) est **à vérifier
> sur la version logicielle du modèle exact**. À défaut : ACL sur le VLANIF de
> management (section 78) — mais appliquée **en étant en console**.

**Et SNMP :** passe en **v3** (section 82), change les communautés v2c par défaut
si v2c il y a, et ne diffuse les traps que vers le superviseur.

---

## 123. Durcissement (4/4) : services, bannières, divers

- [ ] **Bannière légale** : `header login information "Acces reserve..."` —
  dissuasif et utile juridiquement.
- [ ] **Désactive les services inutilisés** : ce qui n'est pas utilisé est une
  surface d'attaque (FTP/TFTP server, Telnet déjà coupé...). `display` l'état
  des services et coupe.
- [ ] **LLDP** : utile en interne ; si le switch est dans une zone publique
  accessible, évalue (il annonce le modèle et le hostname).
- [ ] **CDP** (si présent) : à désactiver en environnement non-Cisco.
- [ ] **Mots de passe chiffrés** : utilise toujours `cipher`, jamais `simple`.
- [ ] **Logs d'accès** : `display logbuffer` doit tracer les connexions —
  vérifie après durcissement.
- [ ] **Sauvegarde post-durcissement** (section 88) : un durcissement non
  sauvegardé est un durcissement qui disparaîtra au prochain reboot.

🔧 **Audit express (15 min) :** `display current-configuration | include` sur
`telnet`, `http server` (non secure), `community`, `password simple` → tout ce
qui ressort est à corriger.

---

## 124. Pense-bête de poche : les 30 commandes qui sauvent

```
# --- État général ---
display version | display device | display clock | display cpu-usage | display memory-usage
display interface brief              # l'état de tous les ports en un écran
display interface GigabitEthernet0/0/5
display logbuffer                    # les logs récents
display current-configuration | display saved-configuration
save                                 # SAUVEGARDER !

# --- VLAN ---
display vlan | display vlan 10
display port vlan | display port vlan GigabitEthernet0/0/5
display mac-address vlan 10 | display mac-address interface GigabitEthernet0/0/5

# --- STP / Trunk ---
display stp brief
display eth-trunk 10

# --- PoE ---
display poe power-state | display poe interface all

# --- IP / routage ---
display ip interface brief | display ip routing-table
ping 192.168.10.1 | tracert 8.8.8.8

# --- Sécurité ---
display dhcp snooping user-bind all
display port-security

# --- Supervision ---
display lldp neighbor brief
display acl 3000

# --- Maintenance fichiers ---
dir flash:/ | display startup

# --- Actions ---
reboot                               # en heure creuse, après save !
reset counters interface GigabitEthernet0/0/5
test cable GigabitEthernet0/0/5      # VCT : coupe brièvement le lien
```

✅ **Imprime cette section**, plastifie-la, scotche-la dans la baie. Le jour où
tu en auras besoin, tu n'auras pas le temps de chercher le PDF.

---

## 125. Glossaire

| Terme | Signification |
|---|---|
| 802.1Q | Standard de taggage des VLAN |
| 802.1X | Authentification d'accès au réseau (port-based) |
| AAA | Authentication, Authorization, Accounting |
| ACL | Access Control List — règles de filtrage |
| BPDU | Trames d'échange du Spanning Tree |
| DAI | Dynamic ARP Inspection — vérifie les ARP contre le binding |
| DSCP | Marquage de priorité dans l'en-tête IP (QoS L3) |
| DHCP snooping | Filtrage des serveurs DHCP non autorisés + table de binding |
| ERPS | Protection en anneau (ITU-T G.8032) |
| Eth-Trunk | Agrégation de liens Huawei (équivalent EtherChannel) |
| HOUP | Huawei Online Upgrade Platform (mises à jour intelligentes) |
| IPSG | IP Source Guard — filtre selon le binding IP/MAC/port |
| iStack | Empilement logique de switches Huawei en un seul |
| LACP | Protocole de négociation d'agrégation (802.3ad) |
| LLDP / LLDP-MED | Découverte des voisins / variante pour la téléphonie |
| MQC | Modular QoS : classifier → behavior → policy |
| MSTP / RSTP / STP | Spanning Tree : multi-instance / rapide / historique |
| OUI | 3 premiers octets d'une MAC = identifiant constructeur |
| PD | Powered Device — équipement alimenté en PoE |
| PSE | Power Sourcing Equipment — le switch qui fournit le PoE |
| PVID | VLAN attribué aux trames non taggées entrant par un port |
| RMON | Surveillance distante (statistiques SNMP) |
| SFP / SFP+ | Modules fibre 1G / 10G |
| SNMP | Protocole de supervision (v1/v2c/v3) |
| Storm control | Plafonnement broadcast/multicast/unicast inconnu |
| VBST | VLAN-based Spanning Tree (spécificité Huawei) |
| VLANIF | Interface virtuelle L3 rattachée à un VLAN |
| VCT | Virtual Cable Test — testeur de câble intégré |
| VRP | Versatile Routing Platform — OS des équipements Huawei |

---

## 126. Quiz : 10 questions pour valider (réponses en section 127)

**Q1.** Quelle est la différence entre un S310-24T4S et un S310-24P4S ?
**Q2.** Quel est le budget PoE total d'un S310-24P4S ? Combien d'AP761 (17,7 W)
peut-il alimenter au maximum ?
**Q3.** Pourquoi ne faut-il jamais laisser le mot de passe d'usine, et où
stocke-t-on les mots de passe admin ?
**Q4.** Un port hybrid avec `pvid vlan 10`, `tagged vlan 20`, `untagged vlan 10` :
que devient une trame non taggée qui entre ? Une trame taggée VLAN 20 qui sort ?
**Q5.** Sur un trunk, quel oubli classique explique que « le VLAN 20 ne passe pas »
alors que le VLAN 10 passe ?
**Q6.** Cite les 3 protections à mettre sur chaque port utilisateur contre les
boucles accidentelles.
**Q7.** Après avoir activé DHCP snooping, plus personne n'a d'IP. Quelle est la
cause la plus probable ?
**Q8.** À quoi sert la table de binding du DHCP snooping, et quelles fonctions
s'appuient dessus ?
**Q9.** Pourquoi faut-il faire `save` après chaque session de configuration ?
**Q10.** Ton uplink 1G est saturé à 100 % en continu. Cite 2 solutions
structurelles (pas la QoS).

---

## 127. Réponses du quiz

**R1.** Le **T** = cuivre sans PoE ; le **P** = cuivre **avec PoE+** (802.3at,
budget 400 W). Le reste (24 ports GE + 4 SFP) est identique.
**R2.** **400 W**. 400 ÷ 17,7 = 22,6 → **22 AP761 maximum** (389,4 W). Le 23e ne
démarrera pas.
**R3.** Parce qu'un mot de passe connu/publié = accès admin à quiconque est sur
le réseau. Mots de passe **uniques, forts, au coffre d'entreprise**, jamais en
clair ni sur post-it.
**R4.** La trame non taggée entrante est marquée **VLAN 10** (PVID). La trame
VLAN 20 sortante sort **taggée** (et une trame VLAN 10 sortante sortirait non
taggée).
**R5.** Le VLAN 20 a été **oublié dans `port trunk allow-pass vlan ...`**
d'un côté (ou des deux). Vérifier avec `display port vlan` des deux côtés.
**R6.** **Edge port** (`stp edged-port enable`) + **BPDU guard**
(`stp bpdu-protection`) + **storm control**. (RSTP actif globalement, évidemment.)
**R7.** Le port vers le **serveur DHCP** (ou le relais) n'est pas en
`dhcp snooping trusted` — sur **chaque** switch traversé en cascade.
**R8.** Elle associe (IP, MAC, port, VLAN, bail) de chaque client légitime.
**DAI** (vérification des ARP) et **IPSG** (filtrage du trafic source) s'appuient
dessus. C'est aussi un inventaire vivant du réseau.
**R9.** Parce que la configuration active vit en **RAM** : sans `save`, elle est
**perdue au reboot**. `display current-configuration` ≠ `display
saved-configuration` tant qu'on n'a pas sauvegardé.
**R10.** Passer l'uplink en **10G** (modèle X / SFP+) ou monter un **Eth-Trunk
LACP** (2× 1G). Et traiter la cause (sauvegardes en heures creuses, caméras...).

**Score :** 9–10 : tu peux former les autres. 7–8 : solide, relis tes points
faibles. < 7 : reprends les sections correspondantes avant de toucher à la prod.

---

## 128. Pour aller plus loin : ressources officielles

- **Fiche produit officielle (eKit) :**
  `https://ekit.huawei.com/ekit/front/ssr/en/product/1239139538459211136`
- **Datasheet série eKitEngine S310 (PDF, spécifications complètes) :**
  `https://i.pcdeacitec.com/datasheets/syscom/S310-24U4X/f2112277465062598bb47bdc547534d7.pdf`
- **Datasheet miroir :**
  `https://resource.ewe.rs/media/documents/2026/08/2026-08-07573726.pdf`
- **Datasheet S310-24T4S :**
  `https://shop.aodatacloud.es/medias/datasheet_s310-24t4s.pdf`
- **Support entreprise Huawei** (firmwares, guides de configuration par version) :
  via ton revendeur / compte support — les images logicielles ne se téléchargent
  pas sans compte.
- **Communauté / forum Huawei** : utile pour les cas tordus déjà rencontrés
  par d'autres (toujours recouper avec la doc officielle).

> ⚠️ **Avertissement versions :** ce guide décrit des commandes **syntaxiquement
> plausibles pour VRP Huawei** et des comportements standard de la gamme eKit
> S310. La syntaxe exacte de certaines fonctions (MQC/QoS avancée, DAI/IPSG,
> MUX VLAN, 802.1X, ports de stack, `startup system-software`, VCT) peut varier
> selon la **version logicielle** : devant toute commande marquée « à vérifier
> sur la version du modèle exact », tape `?` en contexte et consulte le guide
> de configuration de ta version avant la production. En cas de doute, la
> maquette (un switch de test) tranche toujours.

---

*Fin du guide — Huawei eKit S310. Version 1.0 — septembre 2026.*
*Prochaines étapes suggérées : adapter les exemples à ton plan d'adressage réel,
imprimer la checklist (118) et le pense-bête (124), et planifier la première
visite de maintenance (117).*
