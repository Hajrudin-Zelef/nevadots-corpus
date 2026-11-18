# Énergie solaire — Guide ultra-complet (théorie, matériel, déploiement)

> De la physique du photon au dimensionnement d'une centrale : panneaux,
> onduleurs, batteries, installation, sécurité, maintenance, économie.
> Exemples chiffrés calibrés pour l'**Afrique de l'Ouest**
> (ensoleillement ~4,5–5,5 kWh/m²/jour, réseau instable, saison des pluies).
>
> Rédigé à partir des connaissances générales du domaine (2025-2026) :
> vérifiez les prix et réglementations locales, ils évoluent vite.

---

## 1. Bases physiques — comprendre avant d'acheter

### 1.1 L'effet photovoltaïque
- Un photon frappe une cellule en silicium → il arrache un électron →
  le champ électrique interne de la jonction P-N dirige les électrons →
  **courant continu (DC)**.
- Une cellule produit ~0,5–0,6 V. 60–72 cellules en série = panneau
  « 12/24 V » (en réalité 30–45 V à vide, Voc).
- La puissance d'un panneau se mesure en **watts-crête (Wc)** : puissance
  sous conditions standard STC (1000 W/m², 25 °C cellule, AM1.5).

### 1.2 L'irradiation : la vraie unité qui compte
- **kWh/m²/jour** (ou « heures de soleil équivalent plein ») : l'énergie
  reçue par jour. C'est LE chiffre du dimensionnement.
- Ordres de grandeur :
  - Abidjan / Afrique de l'Ouest : **4,5 à 5,5** (saison sèche haute,
    saison des pluies basse).
  - Sahel : 5,5 à 6,5. Europe du Nord : 2,5 à 3,5.
- Un panneau de 550 Wc à Abidjan produit environ :
  `550 Wc × 5 h × 0,75 (rendement système) ≈ 2 kWh/jour`.
- La **température** fait perdre du rendement : −0,35 %/°C au-dessus de
  25 °C (cellule). Sous 65 °C de cellule (courant en Afrique), perte
  ~14 %. D'où l'importance de la ventilation sous les panneaux.

### 1.3 Orientation et inclinaison
- Hémisphère nord : plein **sud**. Hémisphère nord proche de
  l'équateur (Côte d'Ivoire, ~5°N) : inclinaison faible, **5–15°**.
- Règle simple : inclinaison ≈ latitude (Abidjan : ~10°).
- 10–15° minimum quand même : en dessous, la poussière et l'eau
  stagnent (l'harmattan et la poussière rouge encrassent vite).
- **L'ombrage est l'ennemi n°1** : 10 % d'ombre sur un panneau peut
  faire perdre 50 % de sa production (les cellules sont en série).
  Étudiez l'ombre à 9h, 12h et 16h, en saison sèche ET des pluies
  (le soleil est plus bas en décembre).

---

## 2. Les panneaux — choisir le bon

### 2.1 Technologies

| Technologie | Rendement | Durée de vie | Notes |
|---|---|---|---|
| Mono PERC | 20–22 % | 25–30 ans | Standard actuel, bon rapport qualité/prix |
| Mono TOPCon (N-type) | 22–24 % | 25–30 ans | Meilleur en chaleur, devient le standard 2024+ |
| HJT | 23–25 % | 25–30 ans | Excellent coefficient température, plus cher |
| Bifaciaux | +5–15 % de gain | 25–30 ans | Face arrière capte le réfléchi (au sol, surélevés) |
| Poly (P-type) | 16–18 % | 20–25 ans | En fin de vie commerciale, à éviter en neuf |
| Couches minces (CdTe) | 17–19 % | 25 ans | Bon en chaleur/ombrage partiel, grandes surfaces |

- **En 2025-2026 : achetez du mono TOPCon 550–580 Wc** (ou HJT si le
  budget suit). C'est le meilleur compromis chaleur/rendement/prix.
- Bifacial = rentable **au sol** (surélevé, sol clair) ; inutile sur
  toit collé (la face arrière ne voit rien).

### 2.2 Lire une fiche technique
- **Pmax** : puissance crête (Wc). **Voc** : tension à vide.
  **Isc** : courant de court-circuit. **Vmpp/Impp** : point de
  puissance max.
- **Coefficient de température** : le plus bas (en valeur absolue) est
  le mieux pour l'Afrique (−0,30 %/°C bat −0,40 %/°C).
- **Garanties** : 2 niveaux — produit (12–15 ans, défauts) et
  **puissance** (25–30 ans, ≥ 87 % à 25 ans). Exigez les deux par écrit.
- **Certifications** : IEC 61215, IEC 61730, CE. Méfiez-vous des panneaux
  sans marquage (contrefaçons fréquentes sur les marchés).

### 2.3 Tester un panneau avant achat (lot)
- Mesurez **Voc** et **Isc** au soleil avec un multimètre : comparez à
  la fiche (±5 % acceptable).
- Inspectez : microfissures, délamination, jaunissement, boîtier de
  jonction bien scellé, diodes bypass présentes.
- Flash-test en usine : demandez le rapport (puissance réelle ≥ Pmax
  annoncée, tolérance positive 0/+5 W de préférence).

---

## 3. Architecture d'une installation

```
 [Panneaux] --DC--> [Régulateur MPPT] --DC--> [Batteries]
     |                                              |
     +------------DC-------------------> [Onduleur] --AC 230V--> [Consommateurs]
                                                |
                                         [Réseau / Groupe] (hybride)
```

### 3.1 Les 4 architectures

| Architecture | Principe | Idéal pour |
|---|---|---|
| **Off-grid (autonome)** | 100 % solaire + batteries | Site isolé, village, relais télécom |
| **Hybride** | Solaire + batteries + réseau/groupe en secours | Maison/bureau avec délestages |
| **Raccordé réseau (grid-tie)** | Solaire injecté, pas de batteries | Réduire la facture (réseau fiable) |
| **Pompage solaire** | Panneaux → variateur → pompe, sans batteries | Forage, irrigation |

- **En Afrique de l'Ouest, l'hybride est roi** : le réseau existe mais
  coupe ; le solaire couvre le jour, les batteries les coupures, le
  réseau/groupe en dernier recours.
- Le grid-tie pur sans batteries est inutile là où le réseau coupe
  (l'onduleur grid-tie **s'arrête** quand le réseau tombe — norme
  anti-îlotage).

### 3.2 Onduleurs

| Type | Usage | Notes |
|---|---|---|
| **String** | 1–2 trackers MPPT, installation simple | Le standard résidentiel (3–10 kW) |
| **Micro-onduleurs** | 1 par panneau (AC dès le toit) | Cher, mais top contre l'ombrage partiel |
| **Hybride** | Solaire + batteries + réseau/groupe | **Le choix Afrique** (Deye, Victron, Growatt…) |
| **Central** | > 100 kW | Centrales au sol |

- **Dimensionnement** : puissance onduleur ≈ 80–100 % de la puissance
  crête panneaux (on surdimensionne légèrement les panneaux, jamais
  l'inverse — un onduleur trop gros tourne à faible rendement).
- **Surge** : un onduleur 5 kW doit encaisser le démarrage d'un
  congélateur/clim (2–3× le nominal quelques secondes). Vérifiez la
  puissance de surcharge (ex. : 10 kW pendant 5 s).
- **Forme d'onde** : **sinusoïdale pure** obligatoire (les « modifiés »
  tuent les moteurs et l'électronique).
- **Rendement** : ≥ 95 % (européen), comptez les pertes dans le
  dimensionnement.

### 3.3 Régulateurs de charge

- **MPPT** (Maximum Power Point Tracking) : +20–30 % de production vs
  PWM, **obligatoire** au-delà de quelques centaines de Wc.
- **Dimensionnement** : courant max = P_panneaux / V_batterie.
  Ex. : 3000 Wc en 48 V → 62,5 A → régulateur **80 A**.
- Tension PV max du régulateur > Voc totale de la string **par temps
  froid** (le Voc augmente quand il fait froid : +0,3 %/°C sous 25 °C).
- Plusieurs strings d'orientations différentes = plusieurs MPPT
  (ou micro-onduleurs).

---

## 4. Batteries — le poste le plus cher et le plus critique

### 4.1 Comparatif des technologies

|  | Plomb (AGM/Gel) | Lithium LiFePO4 |
|---|---|---|
| Profondeur de décharge | 50 % max | 80–90 % |
| Cycles | 500–1 200 | 4 000–6 000+ |
| Durée de vie | 3–5 ans (chaleur) | 10–15 ans |
| Rendement charge/décharge | 80–85 % | 95 %+ |
| Entretien | Nul (AGM) mais chaleur = mort | BMS intégré, zéro entretien |
| Prix (par kWh utile) | Moins cher à l'achat, **plus cher sur 10 ans** | Plus cher à l'achat, rentable |
| Sécurité | Dégage de l'hydrogène (ventiler) | Très stable (LiFePO4, pas NMC) |

- **Verdict 2025-2026 : LiFePO4 partout** sauf tout petit budget
  immédiat. Le plomb en Afrique (35–40 °C) meurt en 2–3 ans :
  chaque +10 °C au-dessus de 25 °C **divise par 2** sa durée de vie.
- **BMS** (Battery Management System) : obligatoire en lithium —
  il protège contre surcharge, décharge profonde, court-circuit,
  déséquilibre des cellules. Ne jamais acheter de lithium sans BMS.

### 4.2 Dimensionner le parc batteries

**Formule** :
```
Énergie à stocker (Wh) = Consommation nocturne (Wh) × jours d'autonomie
Capacité utile (Wh) = Énergie / profondeur de décharge max
Capacité nominale (Wh) = Capacité utile / rendement
```

- **Jours d'autonomie** : 1–2 jours en hybride (le réseau/groupe
  prend le relais), 2–3 jours en off-grid (saison des pluies).
- **Tension du parc** : 12 V (< 1 kW), 24 V (1–3 kW), **48 V (> 3 kW,
  le standard)**. Plus la tension est haute, plus les courants sont
  faibles (câbles moins gros, moins de pertes).
- **Exemple** : besoin nocturne 5 kWh, 2 jours d'autonomie, LiFePO4
  (DoD 80 %) :
  `5 × 2 / 0,8 = 12,5 kWh utiles` → parc **48 V 260 Ah** (≈ 12,5 kWh).

### 4.3 Règles d'or
- Jamais de mélange : même marque, même modèle, même âge, même lot
  si possible.
- Local **ventilé**, à l'ombre, 15–30 °C idéal. Jamais en plein soleil.
- Câbles de même longueur entre batteries en parallèle (équilibrage
  des courants).
- Coupe-batterie + fusible **au plus près** des bornes (le
  court-circuit d'un parc lithium = soudure instantanée).

---

## 5. Dimensionnement complet — méthode pas à pas

### Étape 1 : bilan de consommation
Listez chaque appareil : puissance (W) × heures/jour = Wh/jour.

| Appareil | Puissance | h/j | Wh/j |
|---|---|---|---|
| 10 LED | 90 W | 6 | 540 |
| Frigo | 150 W | 8 (cyclé) | 1 200 |
| Congélateur | 200 W | 8 (cyclé) | 1 600 |
| Ventilateurs ×3 | 180 W | 8 | 1 440 |
| TV + box | 120 W | 5 | 600 |
| Chargeurs/PC | 200 W | 6 | 1 200 |
| **Total** | | | **≈ 6 580 Wh/j** |

### Étape 2 : puissance crête panneaux
```
P(Wc) = Conso quotidienne (Wh) / (ensoleillement × rendement système)
P = 6 580 / (5 × 0,75) ≈ 1 755 Wc → 4 panneaux de 550 Wc = 2 200 Wc
```
(rendement système 0,70–0,80 : onduleur, câbles, température, poussière).

### Étape 3 : batteries (hybride, 1,5 jour d'autonomie, nuit = 60 % conso)
```
Nuit ≈ 4 kWh × 1,5 jour / 0,8 (DoD) ≈ 7,5 kWh → 48 V 160 Ah LiFePO4
```

### Étape 4 : onduleur et régulateur
- Pic simultané estimé : ~2,5 kW → onduleur hybride **5 kW** (marge).
- Régulateur : 2 200 Wc / 48 V ≈ 46 A → **MPPT 60 A**.

### Étape 5 : synthèse exemple « villa »
- 4 × 550 Wc TOPCon, onduleur hybride 5 kW 48 V, 1 × batterie
  LiFePO4 48 V 160 Ah (7,7 kWh), MPPT 60 A intégré ou externe.
- Production ≈ 8 kWh/jour en saison sèche, ~5 en saison des pluies.

---

## 6. Trois exemples chiffrés

### 6.1 Studio / petit appartement (secours délestages)
- **Besoin** : éclairage, ventilateur, TV, chargeurs, frigo ≈ 2,5 kWh/j.
- **Kit** : 2 × 550 Wc, onduleur hybride 3 kW 24 V, batterie
  LiFePO4 24 V 100 Ah (2,4 kWh).
- **Ordre de prix** : 1 500–2 500 € / 1–1,7 M FCFA (matériel).

### 6.2 Villa familiale (exemple §5)
- **Besoin** : ~6,5 kWh/j.
- **Kit** : 4 × 550 Wc, hybride 5 kW 48 V, LiFePO4 48 V 160 Ah.
- **Ordre de prix** : 4 000–6 000 € / 2,6–4 M FCFA.

### 6.3 Bureau / site télécom 10 kW
- **Besoin** : ~40 kWh/j (clim, serveurs, onduleurs).
- **Kit** : 24 × 550 Wc (13,2 kWc), 2 × hybrides 8 kW en parallèle,
  4 × LiFePO4 48 V 200 Ah (38 kWh), structure au sol.
- **Ordre de prix** : 18 000–28 000 € / 12–18 M FCFA.
- Envisager un **groupe électrogène** en secours + un contrat de
  maintenance (site critique).

> Prix indicatifs 2025-2026, matériel seul, hors main-d'œuvre et
> transport. Les prix ont fortement baissé depuis 2022 (panneaux
> −40 %, lithium −50 %).

---

## 7. Installation — pas à pas

### 7.1 Étude préalable
1. **Relevé d'ombres** (9h/12h/16h, deux saisons).
2. **Toiture** : orientation, pente, état (un toit à refaire dans
   5 ans = refaire l'installation ; renforcez si tôle fine).
3. **Structure** : point d'ancrage, cheminement des câbles DC,
   local technique (batteries/onduleur) ventilé et sécurisé.
4. **Compteur et protections AC** existants.

### 7.2 Structure de montage
- **Toiture tôle** : rails aluminium + crochets/vis avec
  **étanchéité** (joints EPDM) à chaque perçage. Surélévation
  10–15 cm (ventilation).
- **Toiture terrasse / au sol** : châssis acier galvanisé ou alu,
  **lesté ou ancré** (prise au vent : un panneau = une voile ;
  dimensionnez contre les tornades de la saison des pluies).
- **Inclinaison** : 10–15° en Afrique de l'Ouest, plein sud
  (ou est-ouest pour lisser la production sur la journée).
- **Mise à la terre** : toutes les structures métalliques reliées
  à la terre (câble 16 mm² vert/jaune).

### 7.3 Câblage DC (panneaux → régulateur/onduleur)
- Câble **solaire** (double isolation, résistant UV) : 4 mm² jusqu'à
  ~20 A, 6 mm² au-delà. **Jamais de câble domestique en DC.**
- **Chute de tension < 3 %** en DC (calculez : longueur × 2
  aller-retour × courant).
- Connecteurs **MC4** : sertis proprement, jamais de dominos à l'air
  libre. Un mauvais MC4 = point chaud = **incendie**.
- **Coffret DC** : sectionneur DC, **parafoudre DC type 2**,
  fusibles par string si ≥ 3 strings en parallèle.
- Séparez physiquement DC et AC dans les cheminements.

### 7.4 Câblage AC et protections
- Disjoncteur AC dédié en sortie d'onduleur, **interrupteur
  différentiel 30 mA** sur les circuits desservis.
- **Inverseur de source** (manuel ou automatique) entre
  réseau / solaire / groupe — **jamais de couplage sauvage**.
- Parafoudre **AC type 2** au tableau.
- Terre : une seule prise de terre, toutes les masses reliées
  (panneaux, onduleur, coffrets).

### 7.5 Mise en service
1. Vérifier **toutes** les polarités au multimètre avant branchement.
2. Brancher **batteries d'abord**, puis panneaux (régulateur), puis AC.
3. Configurer : type de batterie (LiFePO4 : tensions bulk/float selon
   fabricant, typiquement 56,8 V / 54,4 V en 48 V), seuils de
   bascule réseau, limites de charge.
4. Tester : coupure réseau simulée → bascule < 20 ms (hybride),
   production à midi ≈ P_théorique × 0,7–0,8.
5. **Documenter** : schéma, réglages, mots de passe → remis au client.

---

## 8. Sécurité — à ne jamais négliger

- **Le DC ne pardonne pas** : une string de 10 panneaux = 400 V DC,
  **l'arc DC ne s'éteint pas tout seul** (pas de passage par zéro
  comme en AC). Sectionneurs DC **adaptés** (pas de disjoncteur AC
  en DC !).
- **Arc-fault** : les onduleurs modernes détectent les arcs
  (AFCI) — activez la fonction.
- **Incendie** : 90 % des sinistres = mauvais connecteurs MC4 ou
  câbles sous-dimensionnés. Serrage au couple, thermographie annuelle
  sur les grosses installations.
- **Batteries** : local ventilé, extincteur **poudre ABC ou CO2**
  (jamais d'eau sur un feu électrique/lithium), pas de stockage de
  carburant à côté.
- **Travail en hauteur** : harnais sur toit, jamais seul.
- **Foudre** : paratonnerre si site exposé + parafoudres DC et AC ;
  en zone orageuse (toute l'Afrique de l'Ouest en saison des pluies),
  c'est indispensable, pas optionnel.

---

## 9. Maintenance et monitoring

### 9.1 Routine
| Fréquence | Action |
|---|---|
| Mensuel | Nettoyage panneaux (eau claire, tôt le matin ; l'harmattan encrasse vite) |
| Mensuel | Vérifier production vs attendu (appli onduleur) |
| Trimestriel | Serrage visuel connecteurs, état câbles (rongeurs !) |
| Semestriel | Vérifier fixations (vents violents), corrosion |
| Annuel | Thermographie des coffrets, test parafoudres, resserrage au couple |
| Annuel | Test capacité batteries (décharge contrôlée) |

- **Nettoyage** : un panneau poussiéreux perd 5–15 %. Eau claire,
  éponge douce, **jamais** de jet haute pression ni de produits abrasifs.
- **Monitoring** : onduleurs connectés (WiFi/4G) → applis (Victron
  VRM, Deye Cloud, etc.). Une chute de production = alerte = on agit
  avant que le client s'en aperçoive.

### 9.2 Dépannage express

| Symptôme | Causes probables | Action |
|---|---|---|
| Production nulle, onduleur éteint | Sectionneur DC ouvert, fusible HS | Vérifier coffret DC, polarités |
| Production faible durable | Poussière, ombre nouvelle (arbre poussé), panneau HS | Nettoyer, relever ombres, mesurer Voc/Isc par string |
| Une string à zéro | Diode bypass HS, MC4 fondu, panneau fissuré | Thermographie / mesure string par string |
| Onduleur en défaut « isolement » | Défaut d'isolement DC (câble abîmé, humidité) | Tester l'isolement au mégohmmètre, sécher/réparer |
| Batteries ne chargent plus | BMS en protection, régulateur mal configuré | Lire le BMS (Bluetooth), vérifier seuils |
| Bascule réseau intempestive | Seuil de tension batterie trop haut, surcharge | Relever les logs, ajuster les seuils |

---

## 10. Économie et rentabilité

### 10.1 Calculer le retour sur investissement
```
Coût total installé (€)
÷ Économies annuelles (€/an)  =  temps de retour (ans)
```
- **Économies** : kWh solaires autoconsommés × prix du kWh réseau
  (+ coût du gazole évité si groupe).
- En Afrique de l'Ouest : kWh réseau ~0,15–0,25 €, gazole groupe
  ~0,35–0,50 €/kWh → **retour typique 3–6 ans** en hybride, 2–4 ans
  si ça remplace un groupe.
- **LCOE** (coût actualisé du kWh solaire sur 25 ans) : aujourd'hui
  ~0,05–0,10 €/kWh en Afrique — imbattable face au réseau et au groupe.

### 10.2 Modèles économiques
- **Achat direct** : le client paie, ROI en 3–6 ans, 20 ans de
  quasi-gratuité ensuite.
- **Leasing / PAYG** (pay-as-you-go) : très développé en Afrique
  (M-KOPA, etc.) — le client paie au mois via mobile money,
  l'installateur garde la propriété jusqu'au solde.
- **Autoconsommation + vente du surplus** : là où le rachat existe
  (compteur bidirectionnel) ; sinon, dimensionnez pour
  l'autoconsommation (le surplus perdu = argent perdu).
- **Centrale villageoise / mini-grid** : modèle opérateur (kiosques,
  abonnements) — le plus impactant en zone rurale.

### 10.3 Financement
- Banques locales : de plus en plus de lignes « énergie verte ».
- Bailleurs (Banque mondiale, AFD, UE) : programmes d'électrification
  rurale — les installateurs certifiés y accèdent via appels d'offres.
- **Attention aux arnaques** : acompte > 50 % avant livraison,
  matériel sans garantie écrite, « panneaux 1000 Wc » à prix cassé
  (physiquement impossible en taille standard) → fuyez.

---

## 11. Réglementation et normes (repères)

- **Électrique** : NFC 15-100 (ou équivalent local) pour la partie AC ;
  **UTE C 15-712** (France) = la référence pour le photovoltaïque
  (dimensionnement, protections, mise à la terre).
- **Raccordement réseau** : déclaration auprès du distributeur
  (CIE en Côte d'Ivoire, etc.), conformité anti-îlotage (VDE 0126 /
  EN 50549 pour les onduleurs).
- **Urbanisme** : déclaration de travaux souvent requise au-delà
  d'une certaine puissance/surface.
- **Environnement** : recyclage des panneaux (filières DEEE) et des
  batteries (filière agréée — **jamais** de plomb à la décharge).
- Les règles varient par pays : vérifiez auprès du ministère de
  l'énergie / de l'agence d'électrification rurale locale.

---

## 12. Pompage solaire (cas particulier très demandé)

- **Principe** : panneaux → **variateur de pompe solaire**
  (onduleur dédié avec MPPT) → pompe immergée AC ou DC. **Pas de
  batteries** : on pompe quand il y a du soleil, on stocke **l'eau**
  (château d'eau), pas l'électricité.
- **Dimensionnement** : débit (m³/h) × HMT (hauteur manométrique
  totale) → puissance pompe → puissance crête = P_pompe × 1,3.
- **Exemple** : besoin 10 m³/jour, HMT 40 m → pompe ~1,5 kW →
  ~2 kWc de panneaux + variateur 2,2 kW.
- **Protection** : marche à sec (sonde de niveau), parafoudre,
  filtration en entrée de pompe.
- C'est le système solaire **le plus rentable** (pas de batteries =
  pas de remplacements).

---

## 13. Erreurs classiques des débutants

1. **Sous-dimensionner les batteries** (« on ajoutera plus tard » —
   on ne mélange jamais des batteries d'âges différents).
2. **Surdimensionner les panneaux sans augmenter les batteries**
   (l'énergie excédentaire est perdue, les batteries restent le
   goulot).
3. **Négliger l'ombrage** (un simple câble qui passe devant…).
4. **Câbles trop fins / trop longs** (chutes de tension = pertes
   silencieuses de 5–10 %).
5. **Pas de parafoudres** en zone orageuse.
6. **Onduleur « modified sine »** pour économiser 100 € → moteurs
   grillés.
7. **Batteries au soleil** dans un coin du local technique.
8. **Aucun monitoring** → panne découverte 3 mois plus tard.
9. **Acheter au prix, pas à la fiche technique** (panneaux
   contrefaits).
10. **Oublier la maintenance dans le devis** (le client doit savoir
    nettoyer ses panneaux).

---

## 14. Glossaire express

| Terme | Signification |
|---|---|
| Wc (watt-crête) | Puissance max d'un panneau en conditions STC |
| STC | 1000 W/m², 25 °C, AM1.5 — conditions de test |
| MPPT | Régulateur qui cherche le point de puissance max |
| Voc / Isc | Tension à vide / courant de court-circuit |
| DoD | Profondeur de décharge (50 % plomb, 80 %+ LiFePO4) |
| BMS | Électronique de protection des batteries lithium |
| AFCI | Détection d'arcs électriques (sécurité DC) |
| LCOE | Coût du kWh sur la durée de vie |
| Autoconsommation | Consommer ce qu'on produit |
| Îlotage | Fonctionnement isolé du réseau (interdit en grid-tie) |
| HMT | Hauteur manométrique totale (pompage) |
| PAYG | Paiement à l'usage via mobile money |

---

---

## 15. Onduleurs hybrides — réglages et modes (le cœur du système)

Les marques dominantes en Afrique : **Deye/Sunsynk, Victron, Growatt,
Must, Voltronic (Axpert)**. Principes communs :

### 15.1 Les 3 modes de fonctionnement
- **Priorité solaire** (Solar first) : le solaire alimente les charges
  et charge les batteries ; le réseau n'intervient qu'en secours.
  → Le mode standard en zone de délestages.
- **Priorité réseau** (Utility first) : le réseau alimente, le solaire
  en appoint. → Utile si le kWh réseau est subventionné et fiable.
- **SBU** (Solar → Battery → Utility) : ordre de priorité explicite.
  C'est le réglage le plus fin : solaire d'abord, batteries ensuite,
  réseau en dernier recours.

### 15.2 Réglages critiques (à configurer à la mise en service)
| Paramètre | Valeur type (48 V LiFePO4) | Pourquoi |
|---|---|---|
| Tension bulk/absorption | 56,8 V | Charge complète sans stress |
| Tension float | 54,4 V | Maintien |
| Coupure basse (back to grid) | 48–49 V (~20–30 % restants) | Protège les batteries |
| Retour solaire (back to battery) | 52–53 V | Évite les oscillations |
| Courant de charge max | Selon parc (0,5C max) | 100 Ah → 50 A max |
| Puissance de sortie max | Bridée si besoin | Évite les surcharges |

- **0,5C** : ne chargez jamais une LiFePO4 à plus de la moitié de sa
  capacité en ampères (100 Ah → 50 A). Au-delà = vieillissement accéléré.
- Activez la **compensation en température** si le capteur est fourni
  (surtout en plomb).

### 15.3 Parallélisme
- La plupart des hybrides 5 kW se mettent en **parallèle** (jusqu'à 6
  unités) pour monter à 30 kW. Câbles de communication entre onduleurs
  (CAN/RS485) **obligatoires**, même longueur de câbles batteries.
- En triphasé : 3 onduleurs (un par phase) — voir §16.

---

## 16. Triphasé et couplage groupe électrogène

### 16.1 Installation triphasée
- 3 onduleurs hybrides monophasés (un par phase) **ou** 1 onduleur
  triphasé (Deye 12 kW tri, etc.).
- **Équilibrage des phases** : répartissez les charges (jamais tout
  sur une phase). Un déséquilibre > 30 % = disjonctions et mauvais
  rendement.
- Moteurs triphasés (pompes, clim centrales) : **démarrage progressif**
  (variateur) obligatoire — le courant de démarrage direct (6–8× In)
  fait disjoncter l'onduleur.

### 16.2 Couplage avec groupe électrogène
- Le groupe se branche sur l'entrée **GEN/AC-in** de l'onduleur hybride
  (jamais en direct sur les charges avec le solaire sans inverseur).
- **Démarrage automatique (ATS)** : l'onduleur démarre le groupe quand
  batteries < seuil ET pas de soleil (contact sec + démarreur auto).
- Dimensionnez le groupe à **1,5× la puissance de l'onduleur**
  (un groupe sous-chargé s'encrasse, surchargé disjoncte).
- Le groupe charge les batteries via l'onduleur (chargeur intégré) :
  réglez le courant de charge groupe à ~80 % de sa capacité.

---

## 17. Câblage — abaques et calculs

### 17.1 Chute de tension : la formule
```
ΔU (V) = 2 × L (m) × I (A) × ρ / S (mm²)    avec ρ cuivre = 0,0172
% chute = ΔU / U × 100    →  viser < 3 % en DC, < 5 % en AC
```

### 17.2 Abaque DC (aller-retour 20 m, 48 V)
| Courant | Section | Chute |
|---|---|---|
| 20 A | 6 mm² | 2,3 % |
| 40 A | 16 mm² | 1,7 % |
| 60 A | 25 mm² | 1,6 % |
| 80 A | 35 mm² | 1,6 % |

- **Exemple** : parc 48 V, 60 A, 15 m aller → `2×15×60×0,0172/25 =
  1,24 V` → 2,6 % : OK en 25 mm², limite en 16 mm² (4 %).
- En **400 V DC** (string), les chutes sont 8× moindres à puissance
  égale : d'où l'intérêt des strings haute tension.

### 17.3 Protections : calibres
- **Fusible DC par string** : 1,56 × Isc (ex. Isc 14 A → fusible 20 A
  DC 1000 V, type gPV).
- **Sectionneur DC** : calibre ≥ 1,25 × Isc total, **pouvoir de
  coupure DC** réel (un sectionneur AC fond en DC).
- **Disjoncteur AC sortie onduleur** : In = P / 230 V × 1,25.
- **Différentiel 30 mA** type A (ou B si onduleur sans séparation
  galvanique — lisez la doc).

---

## 18. Structures — tenir face au vent

- **Prise au vent** : un panneau de 2,3 m² à 120 km/h subit ~150 kg
  de poussée. Une rangée de 10 panneaux = 1,5 tonne à ancrer.
- **Toiture tôle** : vis tirefond dans les **pannes** (pas dans la
  tôle seule !), joint EPDM, rail alu. Espacement des crochets
  selon doc fabricant (tous les 1–1,2 m).
- **Terrasse / au sol** : triangles acier galvanisé, **lest béton**
  (plots) ou pieux battus. Lest indicatif : 80–120 kg/panneau en
  zone ventée (à faire valider par un BE pour les grandes centrales).
- **Corrosion** : bord de mer (Abidjan !) → aluminium anodisé +
  visserie **inox A4**, jamais d'acier brut. L'air salin tue
  l'acier galvanisé en 3 ans.
- **Dilatation** : laissez du jeu dans les rails (l'alu se dilate
  de ~2 mm/m entre nuit et plein soleil).

---

## 19. Mini-grids villageoises (électrification rurale)

Le modèle qui change la donne en zone non raccordée :

### 19.1 Architecture type (village 200 foyers)
- **Production** : 30–60 kWc au sol + onduleurs triphasés 30–50 kW.
- **Stockage** : 100–200 kWh LiFePO4 (2 jours d'autonomie).
- **Distribution** : réseau BT 400/230 V sur poteaux (quelques km),
  compteurs **prépayés** (STS) par foyer.
- **Tarification** : abonnement + kWh (mobile money), typiquement
  0,30–0,50 €/kWh — cher au kWh mais sans investissement pour le foyer.

### 19.2 Points critiques
- **Foisonnement** : jamais 200 foyers à pleine puissance en même
  temps → dimensionnez sur la pointe **probable** (compteurs
  communicants pour l'apprendre).
- **Vol et vandalisme** : clôture, gardiennage, panneaux boulonnés
  (vis antivol), GPS sur les onduleurs de valeur.
- **Maintenance locale** : formez 2 techniciens villageois (nettoyage,
  lecture des défauts, fusibles). Sans eux, la centrale meurt en 2 ans.
- **Modèle économique** : l'opérateur doit couvrir O&M + renouvellement
  batteries (provisionnez **dès le jour 1** : ~15 % du CA).

---

## 20. Froid solaire et climatisation

- **Chambres froides solaires** (poisson, vaccins, produits agricoles) :
  le froid se stocke **thermiquement** (plaques eutectiques, glace) —
  bien moins cher que des batteries électriques.
- **Climatisation solaire directe** : clims à compresseur DC alimentées
  en direct par les panneaux (sans batteries, elles tournent quand il y
  a du soleil — exactement quand on a chaud). Marques : Gree, Midea
  (gammes « solar hybrid »).
- **Réfrigérateurs DC** (12/24 V, Steca, Sundanzer) : 3–4× moins
  gourmands qu'un frigo AC via onduleur. Pour le médical et l'off-grid,
  c'est le standard.

---

## 21. Eau chaude solaire (thermique — pas photovoltaïque)

- **Chauffe-eau solaire** (capteurs plans + ballon) : 60–80 % d'économie
  sur l'eau chaude, retour en 3–5 ans. Technologie mûre et simple.
- **Ne pas** chauffer l'eau avec des panneaux PV + résistance : le
  rendement est 3–4× inférieur au thermique (le PV fait de
  l'électricité, pas de la chaleur efficacement).
- En Afrique de l'Ouest : un simple **thermosiphon** (ballon au-dessus
  des capteurs, circulation naturelle) suffit — pas de pompe, pas de
  panne.

---

## 22. Devenir installateur — le métier

### 22.1 Outillage indispensable
- Multimètre TRMS + pince ampèremétrique **DC** (AC/DC !).
- Testeur d'isolement (mégohmmètre 1000 V).
- Sertisseuse MC4 + dénudeur solaire.
- Perceuse, visseuse à chocs, scie, niveau laser.
- Harnais + longe (toiture), EPI (gants isolés 1000 V, lunettes).
- Caméra thermique (ou louez-la pour les audits).

### 22.2 Devis type (ce qu'il doit contenir)
1. Bilan de consommation (tableau signé par le client).
2. Descriptif matériel **avec références exactes** (marque, modèle,
   puissance, garanties).
3. Schéma de principe + plan d'implantation.
4. Planning, garantie d'installation (2 ans mini), contrat de
   maintenance proposé.
5. Prix **détaillé** (matériel / main-d'œuvre / transport), pas un
   forfait opaque.
6. Production estimée (kWh/an) et temps de retour calculé.

### 22.3 Certifications et crédibilité
- Formations fabricants (Victron, Deye…) : gratuites ou peu chères,
  elles ouvrent les garanties pro.
- Qualifications type **QualiPV** (France) ou équivalents locaux ;
  en Afrique de l'Ouest, les appels d'offres des agences
  d'électrification rurale exigent des références.
- **Assurance décennale / RC pro** : indispensable dès que vous touchez
  à la toiture d'un client.

---

## 23. Études de cas détaillées

### 23.1 Clinique rurale (froid vaccins + éclairage + petit labo)
- **Besoin** : 8 kWh/j, criticité maximale (chaîne du froid).
- **Kit** : 8 × 550 Wc (4,4 kWc), hybride 5 kW, 2 × LiFePO4 48 V
  200 Ah (19 kWh, 2+ jours d'autonomie), **2 réfrigérateurs DC**
  médicaux en redondance, monitoring 4G avec alertes SMS.
- **Particularité** : double chaîne de froid (si un frigo lâche,
  l'autre prend le relais), groupe en secours auto.

### 23.2 École (salles + bureautique, usage diurne)
- **Besoin** : 12 kWh/j, **80 % le jour** → peu de batteries.
- **Kit** : 10 × 550 Wc (5,5 kWc), onduleur hybride 8 kW,
  1 × LiFePO4 48 V 200 Ah (9,6 kWh, juste le soir).
- **Astuce** : le profil diurne divise par 2 le coût batteries —
  toujours analyser la **courbe de charge**, pas seulement le total.

### 23.3 Maquis / restaurant (froid + sono le soir)
- **Besoin** : 15 kWh/j, pointe le soir (sono, éclairage, congélateurs).
- **Kit** : 12 × 550 Wc (6,6 kWc), hybride 8 kW, 2 × LiFePO4 48 V
  200 Ah (19 kWh).
- **Astuce** : les congélateurs sont des « batteries thermiques » —
  on les surgèle le jour (solaire abondant), ils tiennent la nuit.

---

## 24. Monitoring avancé et supervision multi-sites

- **Victron VRM**, **Deye Cloud**, **Growatt Shine** : production,
  SoC batteries, alertes — gratuit avec l'onduleur connecté.
- **Installateur multi-sites** : créez un compte pro, ajoutez les
  installations clients → maintenance **prédictive** (vous voyez la
  panne avant l'appel du client = argument commercial massif).
- **Alertes utiles** : production < 70 % de l'attendu, SoC < 20 %,
  température batterie > 40 °C, onduleur en défaut.
- **4G** : un routeur 4G à 30 € + carte SIM suffit pour connecter
  un site isolé (quelques Mo/mois).

---

## 25. Recyclage et fin de vie

- **Panneaux** : 95 % recyclables (verre, alu, silicium). Filières
  DEEE en Europe ; en Afrique, filières émergentes — **ne jamais**
  les abandonner (cadmium sur certaines couches minces).
- **Batteries plomb** : filière plomb très active (les ferrailleurs
  les rachètent — attention aux fonderies sauvages, très polluantes).
- **Batteries lithium** : filière en structuration ; stockez-les en
  lieu sûr en attendant (jamais à la décharge, risque d'incendie).
- Prévoyez une **provision démantèlement** dans les gros projets
  (c'est parfois une obligation légale).

---

## 26. FAQ express

**Le solaire marche-t-il quand il pleut ?**
Oui, à 10–25 % de la puissance (lumière diffuse). D'où les jours
d'autonomie des batteries.

**Faut-il laver les panneaux ?**
Oui en saison sèche/harmattan (poussière = −5 à −15 %). Eau claire,
tôt le matin.

**Combien de temps vit une installation ?**
Panneaux 25–30 ans, onduleur 10–15 ans, batteries LiFePO4 10–15 ans,
structure 25 ans. Prévoyez le remplacement onduleur + batteries une
fois dans la vie de l'installation.

**Peut-on climatiser avec le solaire ?**
Oui, mais c'est le poste le plus gourmand : comptez 1,5 kWc par
clim de 12 000 BTU en usage diurne. Isolation d'abord, clim ensuite.

**Solaire + parafoudre : obligatoire ?**
En zone orageuse (toute l'Afrique de l'Ouest de mai à octobre) : oui,
DC + AC. Un seul impact proche sans protection peut tuer l'onduleur.

**Que faire en cas d'ouragan/tempête ?**
Inclinaison faible (10°) = moins de prise au vent. Au-delà de
150 km/h prévus, aucune structure standard ne garantit rien :
assurance multirisque.

---

## 27. Pompage solaire — dimensionnement détaillé

### 27.1 Calcul de la HMT (hauteur manométrique totale)
```
HMT = hauteur géométrique (niveau nappe → réservoir)
    + pertes de charge (tuyaux, coudes, filtre)
    + pression résiduelle (0,5–1 bar au robinet = 5–10 m)
```
- **Pertes de charge** : comptez ~10–15 % de la hauteur géométrique en
  première approximation (affinez avec l'abaque du tuyau : diamètre,
  longueur, débit).
- **Exemple** : forage à 30 m, château à 6 m, 50 m de tuyau 50 mm →
  HMT ≈ 36 + 5 (pertes) + 5 (pression) ≈ **46 m**.

### 27.2 De l'eau à l'électricité
```
P hydraulique (W) = Q (m³/s) × HMT (m) × 9,81 × 1000
P électrique = P hydraulique / rendement pompe (0,4–0,6)
```
- **Exemple** : 2 m³/h (0,00056 m³/s), HMT 46 m →
  P_hydr = 0,00056 × 46 × 9810 ≈ 252 W →
  P_élec ≈ 252 / 0,5 ≈ **504 W** → pompe 0,75 kW.
- **Champ PV** : P_pompe × 1,3 à 1,5 (le variateur a besoin de marge
  le matin/soir) → **1–1,2 kWc** + variateur solaire 1,5 kW.

### 27.3 Règles du pompage
- **Stockez l'eau, pas l'électricité** : château d'eau = batterie
  gratuite et inusable. Dimensionnez le réservoir à 2–3 jours de
  consommation.
- **Sonde de niveau** bas (marche à sec = mort de la pompe) et haut
  (arrêt quand le château est plein).
- **Démarrage progressif** : le variateur rampe en fréquence —
  indispensable (le pic de démarrage direct grillerait le champ PV).
- **Filtration** : crépine + filtre à sable en entrée, surtout en
  forage sableux.

---

## 28. Auditer une installation existante (checklist)

Utile avant rachat, reprise de maintenance ou diagnostic :

- [ ] **Visuel** : panneaux (fissures, délamination, jaunissement),
  structure (corrosion, desserrage), câbles (rongeurs, UV).
- [ ] **Électrique** : Voc/Isc par string vs fiche technique (±10 %),
  isolement DC au mégohmmètre (> 1 MΩ), serrage des bornes.
- [ ] **Thermographie** : points chauds (MC4, diodes, cellules mortes).
- [ ] **Batteries** : âge, technologie, tensions par élément/bloc
  (écart > 0,2 V = déséquilibre), historique BMS (cycles, défauts).
- [ ] **Production** : comparer 12 mois de monitoring à la théorie
  (P_c × ensoleillement × 0,75). Écart > 20 % = problème.
- [ ] **Documents** : schémas, réglages, garanties, factures —
  une installation sans doc = décote de 20 %.
- [ ] **Sécurité** : parafoudres présents et < 5 ans, terre mesurée
  (< 100 Ω, idéalement < 10 Ω), extincteur présent.

---

## 29. Unités et conversions (pense-bête)

| Grandeur | Unité | Repère |
|---|---|---|
| Puissance instantanée | W / kW | Ce que tire un appareil **maintenant** |
| Énergie | Wh / kWh | Puissance × temps (facture, dimensionnement) |
| Puissance crête | Wc / kWc | Puissance max du panneau (STC) |
| Capacité batterie | Ah | Ah × V = Wh (ex. 200 Ah × 48 V = 9,6 kWh) |
| Irradiation | kWh/m²/jour | 5 à Abidjan ≈ 5 h de « plein soleil » |
| Courant | A | P = U × I (60 A en 48 V = 2 880 W) |

- **Ne confondez jamais W et Wh** : un panneau de 550 Wc ne produit
  pas 550 Wh par heure toute la journée — il produit 550 W **au zénith**,
  soit ~2 kWh sur la journée.
- **1 kWc à Abidjan ≈ 1 400–1 600 kWh/an** (retenez 1 500 pour vos
  devis rapides).

---

*Fin du guide — 800+ lignes. Retenez l'essentiel : dimensionnez sur
la consommation réelle, achetez TOPCon + LiFePO4 + hybride sinusoïdal
pur, protégez (parafoudres, fusibles DC, terre), nettoyez, monitorez.
Le solaire est simple ; c'est la rigueur d'exécution qui fait la
différence entre 5 ans et 25 ans de service.*
