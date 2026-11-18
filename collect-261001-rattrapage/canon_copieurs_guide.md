# Canon — Guide ultra-complet copieurs (maintenance au cœur)

> Tout sur les copieurs/multifonctions Canon imageRUNNER / imageRUNNER
> ADVANCE : fonctionnement, consommables, maintenance préventive,
> dictionnaire des codes erreur, bourrages, qualité d'image, mode
> service, procédures de remplacement, réseau, firmware, dépannage.
>
> **Orienté technicien de maintenance** : c'est le cœur du métier qui
> est détaillé ici. Rédigé à partir des connaissances du domaine
> (manuels de service Canon, 2025-2026). Les codes et procédures
> varient selon les séries : **vérifiez toujours le manuel de service
> exact de votre modèle** avant une intervention lourde.

---

## 1. Les gammes Canon — s'y retrouver

### 1.1 imageRUNNER ADVANCE (la gamme pro actuelle)
- **DX C3800 / C5800 series** (A3 couleur, 2023+) : remplace les C5500.
  Vitesses 30–60 ppm. La plus vendue en entreprise.
- **DX 6800 / 4800 series** (A3 N&B) : remplace les 6500/4500.
- **DX C2700 series** (A3 couleur compacte).
- **C3300 series** (A4 couleur, remplace C3500).
- Anciennes : **iR-ADV C5200/C5500**, **iR-ADV 4200/4500/6500**,
  **iR 2520/2530/2545** (très répandues en Afrique, robustes).

### 1.2 Autres gammes
- **imagePRESS** : production (C10000, V1000) — hors sujet ici sauf
  mention.
- **imageFORMULA** : scanners (pas des copieurs).
- **MAXIFY / PIXMA** : jet d'encre, pas concernées par ce guide.
- **imageRUNNER 1600/2600 series** (A4 N&B récents, remplace 1435).

### 1.3 Lire une référence
- **A3/A4** : format max. **C** = couleur, pas de C = N&B.
- **Chiffres** : vitesse approximative (C5860 ≈ 60 ppm).
- **DX** = génération actuelle (2021+), avec sécurité renforcée
  (McAfee embarqué, TPM).

---

## 2. Principe de fonctionnement — l'électrophotographie

Comprendre le cycle = diagnostiquer vite. 7 étapes :

```
1. CHARGE      → le tambour (OPC) est chargé uniformément (-)
2. EXPOSITION  → le laser « dessine » l'image (zones déchargées)
3. DÉVELOPPEMENT → le toner (-) est attiré sur les zones exposées
4. TRANSFERT   → le toner passe du tambour au papier (+)
5. SÉPARATION  → le papier se décolle du tambour
6. FUSION      → rouleaux chauffants (180-200 °C) fixent le toner
7. NETTOYAGE   → la raclette enlève le toner résiduel du tambour
```

- **Couleur** : 4 tambours (CMYK) + **courroie de transfert (ITB)**
  qui assemble les 4 couches avant transfert sur papier.
- Chaque étape = un composant = une famille de pannes :
  - Fond gris → charge ou développeur.
  - Image pâle → laser, développeur, transfert.
  - Lignes verticales → tambour ou raclette rayés.
  - Taches répétitives → mesurez l'intervalle (voir §7).
  - Papier qui s'enroule → fusion (température/pression).

---

## 3. Consommables et pièces d'usure

### 3.1 Durées de vie typiques (iR-ADV A3, à adapter)

| Pièce | Durée indicative | Signes de fin de vie |
|---|---|---|
| Toner | 15 000–40 000 pages (selon cartouche) | Message « toner bas », pâleur |
| Tambour (drum unit) | 100 000–250 000 pages | Lignes, fond gris, points répétitifs |
| Développeur (developer) | 200 000–500 000 pages | Densité instable, fond |
| Unité de fusion (fixing) | 200 000–500 000 pages | Bourrages sortie, toner qui s'efface |
| Courroie transfert (ITB) | 200 000–500 000 pages | Couleurs décalées, bandes |
| Rouleaux d'entraînement | 100 000–200 000 pages | Multi-prises, bourrages pickup |
| Raclette de nettoyage | avec le tambour | Traînées |

- Ces chiffres sont des **ordres de grandeur** : papier épais, humidité
  et poussière (contexte Afrique) divisent par 1,5 à 2.
- **Compteurs** : COPIER > COUNTER > PARTS (mode service) donne la
  vie restante estimée par pièce.

### 3.2 Toner : les règles d'or
- **Toner d'origine Canon uniquement** en maintenance pro : le
  compatible use le développeur et le tambour (granulométrie
  différente), et annule les arguments de garantie.
- Ne secouez jamais violemment une cartouche (poussière de toner =
  toxique à inhaler, portez un **masque**).
- Stockage : à l'horizontale, à l'ombre, < 35 °C. Le toner cuit
  s'agglutine.
- Après remplacement : la machine fait un cycle d'agitation
  (ne pas interrompre).

### 3.3 Le toner usagé (waste toner)
- Bac récupérateur à vider/remplacer quand plein (**E013** si débordé).
- Ne jamais réutiliser le toner usagé (chargé, contaminé).
- Le bac plein non vidé = toner qui remonte dans le développeur =
  fond gris généralisé.

---

## 4. Maintenance préventive — plannings et checklists

### 4.1 Fréquences recommandées

| Fréquence | Actions |
|---|---|
| **Chaque visite** | Compteurs, codes erreur historiques, test copie/scan, état consommables |
| **Tous les 50 000 pages** | Nettoyage optiques, vitre d'exposition, rouleaux, bac toner usagé |
| **Tous les 100 000 pages** | Rouleaux d'entraînement (si usure), filtres, ventilateurs |
| **Tous les 200 000 pages** | Tambour, développeur, ITB, unité de fusion (selon compteurs PARTS) |
| **Annuel** | Firmware, sauvegarde carnet d'adresses, test disques, dépoussiérage complet |

### 4.2 Checklist de visite (à cocher)

```
□ Relever compteurs (total, couleur, N&B) → COUNTER
□ Historique erreurs → mode service > DISPLAY > ERR / JAM
□ Test : 1 copie vitre + 1 copie ADF + 1 scan + 1 impression réseau
□ Qualité : mire de test (fond, lignes, densité, couleurs)
□ Consommables : % restants (toner, bac usagé)
□ Compteurs de pièces : PARTS (drum, fixing, ITB…)
□ Nettoyage : vitre expo, bande blanche ADF, optiques, corona
□ Rouleaux pickup : visuel (lisse = à changer)
□ Ventilateurs : bruit/poussière
□ Firmware : version vs dernière connue
□ Carnet d'adresses : sauvegarde (si modifs)
□ Client : formation express si bourrages récurrents (souvent = mauvais papier)
```

### 4.3 Le papier — 50 % des appels
- **80 g/m²** standard ; papier humide (saison des pluies !) =
  bourrages + ondulation. Stockez le papier **dans son emballage**,
  à l'horizonté, jamais au sol.
- Papier recyclé bas de gamme = poussière = encrassement optiques et
  rouleaux. Imposez une **marque de papier validée** au client.
- Ventilez la rame avant de charger (évite les multi-prises).
- Guides du bac **ajustés** au format (guides lâches = papier de travers
  = bourrages 0108/0208).

### 4.4 Environnement
- Température 15–30 °C, humidité 20–70 %. En Afrique : **clim ou
  local ventilé** — la chaleur cuit le toner et vieillit les
  électroniques.
- Poussière (harmattan) : dépoussiérage à l'air comprimé **sec**
  (jamais d'air humide de compresseur sans filtre), masque obligatoire.
- Onduleur sur l'alimentation : les micro-coupures corrompent le
  disque dur (**E602**) et tuent les cartes.

---

## 5. Dictionnaire des codes erreur (E-codes)

> Les codes sont au format `EXXX-YYYY` (code + détail). Le détail
> précise le sous-ensemble. Procédure standard : noter le code **complet**,
> éteindre/rallumer, si retour → mode service > COPIER > FUNCTION >
> CLEAR > ERR, si persistant → intervention.
>
> ⚠️ Les codes **E000/E001/E002/E003** (fusion) et **E602** (disque)
> sont les plus fréquents en maintenance. Ils sont détaillés en premier.

### 5.1 Bloc FUSION (les plus critiques)

**E000-0001 — Montée anormale en température (fixing)**
- Cause : thermistance sale/défectueuse, carte DC, surchauffe réelle.
- Action : laisser refroidir 15 min, CLEAR > ERR. Si retour :
  vérifier thermistances (résistance), connecteurs, puis remplacer
  l'unité de fusion. **Ne jamais shunter la sécurité thermique.**

**E001-0001 / 0002 — Température excessive**
- Idem E000, seuil dépassé. Vérifier le thermostat (coupure de
  sécurité) : s'il a sauté, la cause est réelle (thermistance ou
  triac de la carte d'alimentation).

**E002-0000 — Température trop basse**
- La fusion ne monte pas : lampe/résistance chauffante coupée,
  connecteur, carte d'alimentation. Tester la continuité de la
  lampe (hors tension !).

**E003-0000 — Basse température après veille**
- Variante de E002 au sortir de veille. Souvent lampe en fin de vie.

**E007-0000 — Erreur rotation film de fusion**
- Le film ne tourne pas : engrenages, moteur, capteur. Démonter,
  vérifier l'entraînement, graisser si préconisé (graisse spéciale
  fixing, jamais de graisse standard qui cuit).

**E009-0000 — Erreur pression de fusion**
- Capteur de pression came : vérifier le mécanisme, le capteur,
  les connecteurs.

### 5.2 Bloc TONER / DÉVELOPPEUR

**E020-0x81 — Erreur densité toner (noir)**
- Le capteur de densité (ou le patch de test) ne lit pas la bonne
  densité. Causes : toner vide, développeur usé, capteur sale,
  trémie bouchée.
- Action : vérifier toner, nettoyer le capteur de densité/patch,
  lancer un ajustement (FUNCTION > DENS), si échec → développeur.

**E021 à E028** — variantes couleur par couleur (C/M/J).
- Même logique, par station couleur.

**E025-0000 — Moteur d'alimentation toner**
- Trémie/bouteille bloquée, moteur HS, toner agglutiné (chaleur).
  Démonter la trémie, vérifier la rotation à la main.

**E013-0000 — Bac toner usagé plein**
- Vider/remplacer le bac, nettoyer le détecteur. Si le code persiste
  après vidage : capteur sale ou connecteur.

### 5.3 Bloc SCANNER / LECTEUR

**E110-0000 — Erreur moteur scanner**
- Le chariot ne bouge pas : courroie, moteur, capteur home.
  Vérifier le déplacement à la main (hors tension), la courroie.

**E202-0001 / 0002 — Erreur position home (CIS)**
- Le capteur de contact ne trouve pas sa position : butée, capteur,
  nappe. Nettoyer le rail, vérifier la nappe.

**E225-0000 — Intensité lampe CIS**
- Lampe/ligne CIS faible : nettoyer la vitre et la bande blanche,
  si persistant → unité CIS.

**E301-0001 — Erreur intensité lumineuse**
- Même famille : lampe d'exposition ou CIS selon modèle.

### 5.4 Bloc CONTRÔLEUR / COMMUNICATION

**E240-0000 — Erreur communication contrôleur**
- Carte principale ↔ sous-cartes : connecteurs, nappes, puis carte.
  Souvent un connecteur mal remis après intervention.

**E351-0000 — Erreur communication contrôleur principal**
- Redémarrer ; si persistant, carte principale (vérifier les
  alimentations d'abord).

**E602-0001 / 0002 / 0003 — Erreur disque dur**
- **Le classique après micro-coupure.** 0001 = boot impossible,
  0002 = erreur lecture/écriture, 0003 = secteurs défectueux.
- Action : mode service > FUNCTION > SYSTEM > CHK-HDD (vérification),
  si échec → remplacer le HDD/SSD, réinstaller le firmware (SST),
  restaurer la sauvegarde.
- **Prévention** : onduleur obligatoire, arrêt propre
  (pas de coupure sauvage).

**E604-0000 — Mémoire insuffisante**
- Travail trop lourd (fichier énorme) : augmenter la RAM si possible,
  simplifier le document, redémarrer.

**E610-xxxx — Erreur chiffrement disque**
- Clé TPM/pile : vérifier la pile de sauvegarde, réinitialiser le
  chiffrement (perte des données !).

**E710-0000 / E711-0000 — Erreur IPC (communication interne)**
- Initialisation ou communication entre cartes : connecteurs, puis
  cartes (DC controller, main controller).

**E732-0001 — Erreur communication lecteur**
- Nappe lecteur ↔ contrôleur : vérifier/rebrancher.

**E733-0001 — Erreur communication imprimante**
- Côté moteur d'impression : idem, connecteurs puis cartes.

**E744 / E747 / E748 / E749 — Cartes option**
- Langue, carte UFR, carte contrôleur, carte PDL : réinstaller le
  firmware du module, vérifier la carte.

### 5.5 Bloc ADF (chargeur)

**E400-0001 / E401-xxxx — Erreur communication ADF**
- Nappe ADF : vérifier le connecteur (souvent pincé à la charnière).

**E402-0000 — Moteur courroie ADF**
**E404-0000 — Moteur de sortie ADF**
**E405-0000 — Moteur de séparation ADF**
- Tester chaque moteur en mode service (FUNCTION > PART-CHK),
  vérifier les courroies et rouleaux ADF (usure = bourrages 00xx).

**E412-0000 — Ventilateur ADF**
- Souffler/nettoyer, remplacer si bloqué.

### 5.6 Bloc FINISHER (trieur/agrafage)

**E500-0001 / E501 — Erreur communication finisher**
- Câble finisher ↔ copieur : rebrancher (machine éteinte).

**E514-8000 — Moteur de sortie du finisher**
**E530-xxxx — Alignement arrière**
**E540-xxxx — Déplacement agrafeuse**
**E542-xxxx — Moteur agrafeuse**
- Vérifier les butées mécaniques (souvent une agrafe coincée),
  les capteurs (poussière de papier), puis les moteurs en PART-CHK.
- **Agrafes** : utiliser les agrafes Canon d'origine (les compatibles
  se coincent et faussent les capteurs).

**E590-xxxx — Perforatrice**
- Bac de confettis plein (le classique oublié !), capteurs sales.

### 5.7 Bloc VENTILATEURS / DIVERS

**E805-0000 — Ventilateur d'évacuation chaleur**
**E824-0000 — Ventilateur de charge primaire**
- Nettoyer (poussière bloquante), tester, remplacer. Un ventilateur
  mort = surchauffe = E000 en cascade.

**E808-0000 — Entraînement fixing**
- Moteur/engrenages de la fusion : vérifier la charge mécanique
  (unité de fusion grippée ?).

**E719-0000 — Monnayeur / lecteur de cartes**
- Périphérique de paiement : vérifier la connexion, désactiver si
  non utilisé (le code peut bloquer la machine).

### 5.8 Méthode générale face à un E-code

```
1. Noter le code COMPLET (EXXX-YYYY) + circonstances
2. Éteindre, attendre 2 min, rallumer → si disparu, surveiller
3. Si retour : mode service > COPIER > FUNCTION > CLEAR > ERR
4. Si persistant : consulter le manuel de service du modèle
5. Tester le composant (PART-CHK), vérifier connecteurs/nappes
6. Remplacer la pièce (toujours machine ÉTEINTE et débranchée)
7. CLEAR > ERR, test complet, noter l'intervention
```

---

## 6. Bourrages papier — jam codes

> Format type : `0AXX` ou `01XX`. Les 2 premiers chiffres = **zone**,
> les 2 derniers = **type**. Exemple : `0108` = zone pickup, bourrage
> au retrait.

### 6.1 Zones (2 premiers chiffres)

| Code zone | Localisation |
|---|---|
| `01xx` | Pickup (cassette → rouleaux d'entraînement) |
| `02xx` | Alimentation / registration (avant transfert) |
| `03xx` | Sortie / fusion (fixing → bac de sortie) |
| `0Axx` | Duplex (recto-verso) |
| `0Dxx` | Finisher / sortie option |
| `00xx` | ADF (chargeur de documents) |

### 6.2 Types (2 derniers chiffres, les plus courants)

| Code | Signification | Action |
|---|---|---|
| `xx01` | Retard (delay jam) : le papier n'arrive pas au capteur à temps | Rouleaux usés, papier humide, guides mal réglés |
| `xx02` | Stationnaire : le papier reste trop longtemps sur un capteur | Capteur sale/défectueux, papier coincé |
| `xx08` | Échec de prise (pickup retry) | Rouleaux pickup lisses → remplacer/nettoyer |
| `xx0B`–`xx0E` | Multi-prise, longueur anormale | Ventiler le papier, séparation usée |

### 6.3 Dépannage par zone

**Zone 01 (pickup)** — le plus fréquent :
- Rouleaux pickup lisses → nettoyer à l'alcool isopropylique ou
  remplacer (kit rollers). Test : frottez le doigt, si ça glisse
  sans accrocher, c'est mort.
- Séparateur (separation pad/roller) usé → multi-prises.
- Papier humide ou guides lâches.

**Zone 02 (registration)** :
- Rouleaux de registration sales → nettoyer.
- Capteurs poussiéreux → souffler.

**Zone 03 (fusion/sortie)** :
- Unité de fusion : doigts de séparation (picker fingers) usés,
  film abîmé → le papier s'enroule.
- Température : vérifier les thermistances (E002 en arrière-plan ?).

**Zone 0A (duplex)** :
- Chemin duplex obstrué (morceau de papier), rouleaux inverseurs.

**Zone 00 (ADF)** :
- Rouleaux ADF usés, guides du chargeur mal réglés, document
  froissé/agrafé (interdire les agrafes !).

### 6.4 Extraction d'un bourrage — les règles
1. **Suivre le chemin indiqué à l'écran** (les trappes sont numérotées).
2. Tirer **dans le sens du papier**, doucement, jamais à contre-sens
   (ça casse les capteurs).
3. Vérifier qu'il ne reste **aucun morceau** (un bout de 2 cm suffit
   à re-bourrer).
4. Refermer **toutes** les trappes (un capteur de porte ouvert =
   bourrage fantôme).
5. Si bourrages récurrents au même endroit → noter le jam code et
   traiter la cause (rouleaux, capteur, fusion).

---

## 7. Qualité d'image — diagnostic par symptôme

### 7.1 Mesurer l'intervalle des défauts répétitifs
Un défaut qui se répète à intervalle régulier = un rouleau/tambour
abîmé. Mesurez la distance entre deux défauts :
- ~94 mm → tambour OPC (circonférence typique).
- ~75–80 mm → rouleau de charge.
- ~60 mm → rouleau de développement.
- Intervalle = π × diamètre du composant (vérifiez au manuel).

### 7.2 Tableau des symptômes

| Symptôme | Causes probables | Actions |
|---|---|---|
| **Fond gris/voile** | Développeur usé, toner incompatible, charge primaire faible, bac usagé plein | Ajustement densité, remplacer développeur, toner origine |
| **Image pâle** | Toner bas, laser sale, transfert faible, développeur | Toner, nettoyer optiques laser, vérifier ITB |
| **Lignes verticales blanches** | Corona sale, LED/laser obstrué, tambour rayé | Nettoyer corona, inspecter tambour |
| **Lignes verticales noires** | Raclette abîmée, tambour rayé, rouleau de charge | Remplacer tambour/unité |
| **Points/taches répétitifs** | Tambour piqué, rouleau de fusion abîmé | Mesurer l'intervalle (§7.1) |
| **Bandes horizontales** | Charge, développeur, alimentation laser | Nettoyer, tester par couleur |
| **Couleurs décalées** | ITB, capteurs de registration, vibrations | Calibration auto (AJUST), nettoyer capteurs |
| **Papier ondulé** | Papier humide, fusion trop chaude | Papier sec, vérifier thermistances |
| **Toner qui s'efface au doigt** | Fusion froide (E002 latent), vitesse/papier épais mal réglés | Température fusion, type papier déclaré |
| **Copie ADF floue/décalée** | Vitre d'exposition (bande étroite) sale | Nettoyer la **bande blanche** et la vitre fine ADF |
| **Fond noir total** | Laser/exposition HS, charge primaire | Vérifier laser, E-codes |

### 7.3 Calibrations (à faire après chaque intervention image)
- **Ajustement auto des couleurs** : mode service ou menu utilisateur
  (Réglages > Ajustement > Calibration).
- **Densité** : FUNCTION > DENS (par couleur).
- Toujours calibrer après remplacement tambour/développeur/ITB.

---

## 8. Mode service — le cockpit du technicien

### 8.1 Accès
```
1. Appuyer sur * (ou Paramètres/Enregistrement)
2. Appuyer simultanément sur 2 et 8
3. Rappuyer sur * (ou Paramètres)
→ Menu SERVICE MODE
```
- Niveaux : **COPIER** (moteur), **FEEDER** (ADF), **SORTER** (finisher),
  **FAX**, **BOARD**, etc.
- **Ne touchez jamais** aux réglages d'usine (ADJUST) sans noter
  l'ancienne valeur. Photo avant chaque modif.

### 8.2 Les menus essentiels

| Menu | Usage |
|---|---|
| `COPIER > DISPLAY > ERR` | Historique des erreurs |
| `COPIER > DISPLAY > JAM` | Historique des bourrages |
| `COPIER > DISPLAY > ALARM` | Alarmes |
| `COPIER > FUNCTION > CLEAR > ERR` | Effacer un code erreur |
| `COPIER > FUNCTION > CLEAR > JAM-HIST` | Effacer historique bourrages |
| `COPIER > FUNCTION > PART-CHK` | Tester moteurs/capteurs un par un |
| `COPIER > FUNCTION > DENS` | Ajustement densité |
| `COPIER > COUNTER > TOTAL` | Compteurs |
| `COPIER > COUNTER > PARTS` | Vie des pièces d'usure |
| `COPIER > ADJUST` | Réglages fins (noter avant !) |
| `COPIER > OPTION` | Options machine (à manipuler avec précaution) |

### 8.3 PART-CHK — tester sans démonter
- Active chaque moteur/embrayage/capteur individuellement :
  idéal pour isoler un E-code (ex. : E404 → tester le moteur de
  sortie ADF, l'entendre tourner ou pas).
- **Attention** : certains tests font bouger des mécanismes —
  mains hors de la machine.

---

## 9. Procédures de remplacement (grandes classiques)

> Machine **éteinte et débranchée**, EPI (masque, gants), toner =
> ne pas inhaler, ne pas jeter au feu.

### 9.1 Unité de fusion
1. Ouvrir la trappe latérale/arrière (selon modèle), débrancher les
   connecteurs (thermistances, lampe).
2. Dévisser (2–4 vis), sortir l'unité en la tenant par les poignées.
3. Monter la neuve, rebrancher, revisser.
4. CLEAR > ERR, **remettre le compteur à zéro** (COUNTER > PARTS >
   FIXING > clear), test 20 copies, vérifier la température
   (DISPLAY > ANALOG > température fixing).

### 9.2 Tambour / unité tambour
1. Sortir la cartouche de toner, puis l'unité tambour (ne jamais
   toucher la surface verte/bleue, ne pas l'exposer à la lumière >
   5 min — la couvrir d'un chiffon).
2. Monter la neuve, remettre le toner.
3. Reset compteur PARTS > DRUM, calibration couleurs, test mire.

### 9.3 Développeur
1. Sortir l'unité de développement, vider l'ancien développeur
   (sac étanche, ne pas respirer).
2. Verser le neuf **lentement et uniformément**, remonter.
3. **Initialisation obligatoire** : FUNCTION > INSTALL > STIR ou
   TONER-S (selon modèle) — agite et calibre le mélange.
   Sans init, densité fausse garantie.
4. Reset PARTS > DEVELOPER, calibration.

### 9.4 Courroie de transfert (ITB)
1. Sortir l'unité ITB (souvent par l'avant après retrait des
   tambours), débrancher le moteur.
2. Ne pas toucher la courroie avec les doigts (graisse = défauts).
3. Remonter, reset PARTS > ITB, **registration couleurs**
   (FUNCTION > ADJUST > REGIST).

### 9.5 Rouleaux d'entraînement (kit rollers)
1. Cassettes dehors, localiser pickup/feed/separation rollers.
2. La plupart se déclipsent sans outils (languette).
3. Remplacer par kit complet (pickup + feed + separation),
   reset si compteur associé, test multi-pages.

---

## 10. Réseau, scan, impression

### 10.1 Installation réseau
- IP fixe recommandée (DHCP = l'imprimante « disparaît » au
  renouvellement). Réglages > Réseau > TCP/IP.
- Pilotes : **UFR II** (standard Canon), **PCL6** (universel),
  **PostScript** (option sur certains modèles, pour la PAO).
- Toujours prendre le pilote sur **canon.com** (pas le CD fourni,
  obsolète).

### 10.2 Scan vers dossier (SMB) — la panne n°1 en entreprise
- Prérequis : dossier partagé Windows, utilisateur dédié
  (ex. `scan` / mot de passe **qui n'expire pas**), droits en écriture.
- Sur le copieur : carnet d'adresses > nouveau > dossier SMB >
  `\\SERVEUR\scans`, identifiants.
- **Pannes classiques** :
  - Windows a mis à jour → SMBv1 désactivé : le vieux copieur ne
    parle que SMBv1 → **activer SMBv2/3 sur le copieur** (firmware
    récent) ou forcer SMBv2 côté Windows.
  - Mot de passe expiré → erreur d'authentification.
  - Antivirus/pare-feu bloque le partage.
- Alternative robuste : **Scan to FTP** ou **Scan to Email**.

### 10.3 Scan to Email
- SMTP du FAI ou Office 365 / Google (avec **mot de passe d'application**,
  pas le mot de passe normal).
- Ports : 587 (STARTTLS) ou 465 (SSL). Tester depuis le copieur
  (bouton Test).
- Limite de taille : régler la résolution (300 dpi suffit pour du
  bureau, pas 600).

### 10.4 Impression mobile / cloud
- **Canon PRINT** (appli), AirPrint, Mopria : activer dans les réglages
  réseau. Pratique, mais à sécuriser (impression suiveuse avec badge
  = **uniFLOW**, la solution Canon entreprise).

---

## 11. Firmware — SST (Service Support Tool)

- L'outil Canon **SST** (PC Windows + câble USB/réseau) flashe le
  firmware : indispensable après remplacement HDD/carte.
- **Règles** :
  1. Toujours sauvegarder (carnet d'adresses, réglages) avant.
  2. Ne jamais couper pendant le flash (onduleur !).
  3. Prendre le firmware **exact** du modèle et de la région.
  4. Après flash : vérifier la version, restaurer les réglages, tester.
- Firmwares récents = correctifs SMB, sécurité (failles), compatibilité
  Windows. Un copieur de 2018 non flashé **ne scannera plus** vers un
  Windows 11 à jour.

---

## 12. Compteurs et gestion

- **Compteurs** : total, par format, couleur/N&B, scan, fax.
  Relevé mensuel pour la facturation au coût-page.
- **Department ID** : codes par service/utilisateur (quotas,
  imputation). À activer pour les clients qui veulent contrôler.
- **Rapports** : la machine peut envoyer le compteur par email
  (utile pour la facturation sans déplacement).

---

## 13. Sécurité des données

- Les copieurs **stockent tout** : copies, scans, impressions sur le
  disque dur. Un copieur réformé = une fuite de données.
- **Avant revente/recyclage** : mode service > FUNCTION > SYSTEM >
  **HDD erase** (effacement complet, plusieurs passes) ou remplacement
  physique du disque.
- **Chiffrement disque** : à activer (avec sauvegarde de la clé !).
- **McAfee embarqué** (séries DX) : vérifie l'intégrité au boot.
- Politique : effacer les carnets d'adresses (emails, dossiers SMB
  avec mots de passe !) avant tout départ de machine.

---

## 14. Dépannage express par symptôme

| Symptôme | Vérifier d'abord |
|---|---|
| Ne s'allume pas | Prise, interrupteur, fusible interne, carte alim |
| Écran noir mais ventilo tourne | Carte contrôleur, barrette RAM, HDD |
| Bourrage à chaque copie | Rouleaux pickup, papier humide, guides |
| Copies pâles | Toner, densité, laser sale |
| Copies trop foncées / fond gris | Développeur, charge, toner compatible |
| Lignes verticales | Tambour, raclette, corona |
| Ne scanne plus vers dossier | SMB (v1/v2), mot de passe, réseau |
| N'imprime plus réseau | IP (DHCP ?), pilote, file d'attente, pare-feu |
| Bruit anormal | Ventilateur, engrenages, unité de fusion |
| Surchauffe / odeur | Fusion, ventilateurs → **éteindre, intervenir** |
| Code E persistant | §5, CLEAR > ERR, puis pièce |
| Lent / se bloque | HDD (E602 latent), mémoire, firmware |

---

## 15. Boîte à outils du technicien Canon

- Tournevis cruciforme/plat (jeux), pince fine, pince coupante.
- **Aspirateur à toner** (filtre HEPA — jamais d'aspirateur normal,
  le toner traverse les filtres et encrasse le moteur).
- Alcool isopropylique + chiffons non pelucheux (optiques, rouleaux).
- Air comprimé sec + masque FFP2.
- Multimètre (tensions, continuité lampes).
- Kit rollers universel Canon, agrafes d'origine, toner d'origine.
- PC portable + **SST** + firmwares des modèles suivis (sur clé USB).
- Lampe frontale (l'intérieur d'un copieur est sombre).
- Carnet : noter **chaque** intervention (date, compteur, pièces,
  codes) — c'est votre mémoire et votre argument commercial.

---

## 16. Glossaire

| Terme | Signification |
|---|---|
| OPC | Tambour photoconducteur organique |
| ITB | Courroie de transfert intermédiaire (couleur) |
| Fixing / Fusion | Unité qui fixe le toner par chaleur+pression |
| Developer | Poudre magnétique qui transporte le toner |
| Corona | Fil/charge de la charge primaire |
| ADF / DADF | Chargeur automatique (recto / recto-verso) |
| Finisher | Module de finition (tri, agrafage, pliage) |
| SST | Service Support Tool (flash firmware Canon) |
| UFR II | Langage d'impression Canon |
| MEAP | Plateforme d'applications embarquées Canon |
| Department ID | Gestion par codes utilisateurs |
| Pickup roller | Rouleau de prise papier |

---

---

## 17. Dictionnaire E-codes étendu (E012 → E8xx)

> Complément du §5. Format condensé : code → composant → action de
> première intention. Les détails (YYYY) précisent le sous-ensemble :
> notez-les toujours.

| Code | Composant / sens | Première action |
|---|---|---|
| E012-0000 | Moteur tambour | PART-CHK moteur, engrenages, connecteurs |
| E014-0000 | Moteur fixing | Vérifier charge mécanique de la fusion |
| E019-0001 | Pression toner usagé | Bac plein / capteur (voir E013) |
| E026–E028 | Densité toner couleur (M/C/J) | Voir E020, par station |
| E032-0001 | Compteur NE (option) | Réinitialiser le compteur du contrôleur NE |
| E121-0000 | Erreur rotation (générique moteur) | Identifier le moteur via le détail YYYY |
| E196-0001 | Erreur ROM / firmware | Reflash SST du module concerné |
| E197-0000 | Erreur moteur (laser/scanner) | Selon YYYY : laser ou scanner |
| E260-0000 | Capteur de densité | Nettoyer le capteur, ajustement DENS |
| E261-0000 | Signal zero-cross | Alimentation AC, carte d'alimentation |
| E315-xxxx | Erreur données image | Redémarrer, RAM/HDD, firmware |
| E317-xxxx | Erreur communication (module) | Selon YYYY, connecteurs puis carte |
| E350-0000 | Erreur communication (générique) | Câbles internes, cartes |
| E354-xxxx | Erreur contrôleur (sous-code) | Manuel de service du modèle |
| E390-xxxx | Erreur DDI (interface) | Redémarrer, carte interface |
| E407-0000 | Moteur rouleau ADF | PART-CHK, courroie |
| E408-0000 | Moteur séparation ADF | Rouleaux, moteur |
| E409-0000 | Moteur lecture ADF | Capteur + moteur |
| E413-0000 | Moteur décalage ADF | Butée, capteur |
| E420-0000 | EEPROM ADF | Réécrire les données, remplacer carte ADF |
| E490-xxxx | Mauvais type DADF | Vérifier la compatibilité du chargeur |
| E503-xxxx | Communication finisher (détail) | Câble, carte finisher |
| E505-xxxx | Mémoire finisher | Carte finisher |
| E514-xxxx | Moteur sortie finisher (détail) | Butée, capteur, moteur |
| E518-xxxx | Moteur décalage (finisher) | Idem |
| E520-xxxx | Moteur agrafage (détail) | Agrafe coincée, capteur |
| E531-xxxx | Alignement (détail) | Poussière papier, capteur |
| E540-xxxx | Déplacement agrafeuse | Rail, capteur, moteur |
| E544-xxxx | Réserve agrafes | Recharger, détecteur |
| E551-xxxx | Ventilateur finisher | Nettoyer/remplacer |
| E575-xxxx | Empileur (stacker) | Butée, capteur |
| E584-xxxx | Volet (shutter) finisher | Mécanisme, capteur |
| E590-xxxx | Perforatrice | Bac confettis plein ! capteurs |
| E5F0–E5F5 | Module piqûre à cheval (saddle) | Agrafes, butées, capteurs |
| E601-xxxx | Erreur communication (disque/carte) | Connecteurs, HDD |
| E603-xxxx | Erreur clavier/panneau | Nappe panneau |
| E605-xxxx | Erreur mémoire (détail) | Barrette RAM, réinstaller |
| E606-xxxx | Erreur HDD (détail) | Voir E602 |
| E609-xxxx | Erreur carte (détail) | Selon YYYY |
| E611-xxxx | Erreur communication (option) | Carte option |
| E614-xxxx | Firmware incompatible | Reflash version cohérente |
| E616-xxxx | Erreur données (détail) | Restaurer réglages |
| E674-xxxx | Carte fax | Réinstaller, vérifier ligne |
| E677-xxxx | Carte imprimante (option) | Firmware, carte |
| E676-xxxx | Erreur mémoire fax | Carte fax |
| E700-xxxx | Erreur finisher (générique) | Câble, carte |
| E701-xxxx | Erreur communication (module) | Selon YYYY |
| E712-xxxx | Erreur option (détail) | Carte option concernée |
| E719-xxxx | Monnayeur/lecteur cartes | Voir §5.7 |
| E720-xxxx | Erreur carte (détail) | Selon YYYY |
| E730-xxxx | Erreur PDL | Réinstaller firmware PDL |
| E734-xxxx | Erreur communication (détail) | Connecteurs |
| E737-xxxx | Erreur mémoire (détail) | RAM |
| E740-xxxx | Erreur carte réseau | Carte NIC, firmware |
| E741-xxxx | Erreur PCI | Reseat carte, slot |
| E743-xxxx | Erreur DDI-S | Redémarrer |
| E745-xxxx | Erreur carte (détail) | Selon YYYY |
| E746-xxxx | Erreur contrôleur d'impression | Carte, firmware |
| E750-xxxx | Erreur communication (détail) | Connecteurs |
| E804-xxxx | Ventilateur alimentation | Nettoyer/remplacer |
| E806-xxxx | Ventilateur finisher | Idem |
| E808-xxxx | Entraînement fixing | Voir §5.7 |
| E810-xxxx | Moteur nettoyeur | Vérifier mécanisme |
| E812-xxxx | Ventilateur (détail) | Selon YYYY |
| E815-xxxx | Moteur (détail) | PART-CHK |
| E817-xxxx | Rotatif développement (couleur) | Engrenages du carrousel |
| E819-xxxx | Erreur moteur (détail) | Selon YYYY |
| E821-xxxx | Nettoyeur (détail) | Raclette, bac |
| E823-xxxx | Ventilateur (détail) | Selon YYYY |
| E825-xxxx | Ventilateur (détail) | Selon YYYY |
| E830-xxxx | Erreur refroidissement | Ventilateurs, filtres |
| E840-xxxx | Volet (détail) | Mécanisme |

> **Règle** : un code en `E8xx` = souvent ventilateur/moteur
> (poussière), un code en `E02x` = toner/développeur, un code en
> `E40x` = ADF, un code en `E5xx` = finisher, un code en `E6xx` =
> disque/mémoire/firmware. Cette logique par familles accélère le
> diagnostic.

---

## 18. Jam codes — compléments par famille

### 18.1 Lire un jam code complet
L'historique (DISPLAY > JAM) affiche : `date, heure, code, cassette,
format`. Exemple : `0108, C1, A4` = échec de prise, cassette 1, A4.

### 18.2 Les récurrents en maintenance Afrique

| Jam | Zone | Cause n°1 terrain |
|---|---|---|
| 0108 | Pickup C1 | Rouleaux lisses (poussière) |
| 0101 | Pickup | Papier humide (saison des pluies) |
| 0208 | Registration | Guides bac mal réglés |
| 0301 | Sortie fusion | Doigts de séparation usés |
| 0A01 | Duplex | Morceau resté d'un bourrage précédent |
| 0011 | ADF | Document agrafé / rouleaux ADF |

### 18.3 Capteurs : les tester
- DISPLAY > SENSOR : état en temps réel de chaque capteur (0/1).
- Passez une feuille devant le capteur suspect : l'état doit changer.
  Sinon : capteur sale (souffler), débranché, ou HS.
- Les capteurs optiques (photosenseurs) s'encrassent avec la poussière
  de papier : nettoyage à l'air sec à chaque visite des zones à
  bourrages récurrents.

---

## 19. Procédures de remplacement — compléments

### 19.1 Remplacement HDD + réinstallation firmware
1. Sauvegarder : carnet d'adresses, réglages (export).
2. Machine éteinte, remplacer le HDD/SSD (modèle compatible,
   même capacité ou supérieure).
3. Boot en mode download, flasher avec **SST** (firmware complet).
4. Restaurer les réglages, reconfigurer le réseau.
5. Test : 50 copies + scans + impressions réseau.
6. **Prévention** : onduleur + arrêt propre systématique.

### 19.2 Kit de maintenance complet (200–300k pages)
1. Tambour(s), développeur(s), unité de fusion, ITB (couleur),
   kit rollers, bac toner usagé, filtres.
2. Ordre : sortir les anciennes unités, aspirer l'intérieur
   (aspirateur à toner), monter les neuves.
3. **Reset de chaque compteur PARTS** (sinon la machine croit que
   c'est toujours l'ancienne pièce).
4. Initialisations : STIR/TONER-S (développeur), calibration couleurs,
   ajustement densité, registration.
5. Test : 100 copies (N&B + couleur), mire qualité, 50 recto-verso.
6. Noter : date, compteur, pièces (n° de série si garantie).

### 19.3 Courroie et rouleaux ADF
1. Ouvrir le capot ADF, repérer pickup/feed/separation rollers
   (souvent clipsés).
2. Remplacer le kit, nettoyer les capteurs du chemin papier.
3. Test : 20 originaux recto-verso en ADF.

### 19.4 Après chaque remplacement — la règle des 3 R
- **Reset** (compteurs PARTS), **Réglage** (calibrations),
  **Rapport** (noter l'intervention).

---

## 20. Calibrations et réglages fins (ADJUST)

> ADJUST = réglages d'usine. **Photo/noter avant chaque modification.**
> Ne touchez que ce que vous comprenez.

| Réglage | Effet | Quand l'utiliser |
|---|---|---|
| DENS (par couleur) | Densité d'image | Après développeur/tambour |
| REGIST | Superposition des couleurs | Après ITB, couleurs décalées |
| BLANK | Marges blanches | Bords sales |
| FEED-ADJ | Vitesse d'entraînement | Bourrages/décalages récurrents |
| FIX-TEMP | Température de fusion | Toner qui s'efface / papier ondulé |
| LASER | Puissance laser | Image pâle persistante |

- Toujours faire les **calibrations automatiques** (menu utilisateur)
  avant de toucher aux réglages manuels : 80 % des problèmes s'y
  règlent.
- Un déréglage ADJUST sans note = des heures pour retrouver la valeur
  d'origine. Discipline non négociable.

---

## 21. Consommables — achats, stocks, faux toners

### 21.1 Gérer le stock
- Stock minimum : 1 toner de chaque couleur par machine suivie +
  1 kit rollers + 1 bac usagé. Les pièces lourdes (fusion, tambour)
  se commandent au compteur PARTS (anticipez à 80 % de vie).
- **FIFO** : premier entré, premier sorti (le toner vieillit).
- Traçabilité : n° de lot, date d'achat (litiges garantie).

### 21.2 Repérer un faux toner Canon
- Prix < 60 % du tarif officiel = suspect.
- Hologramme/QR : vérifiable (Canon a un système de vérification).
- Poids : les faux sont souvent plus légers (moins de toner).
- Symptômes après montage : fond gris rapide, fuite de toner,
  usure accélérée du développeur.
- **En maintenance pro : n'installez jamais de compatible**
  (responsabilité + image).

---

## 22. Contrats de maintenance — le business

> C'est votre métier : cette section vaut de l'or.

### 22.1 Les 3 modèles de contrat

| Modèle | Principe | Marge | Risque |
|---|---|---|---|
| **Coût-page** | X FCFA/page N&B, Y FCFA/page couleur, tout inclus (toner, pièces, MO) | Élevée si bien calculé | Sous-évaluation du coût réel |
| **Forfait + consommables** | Forfait mensuel MO + pièces/toner facturés | Moyenne | Client freine les remplacements |
| **Tout inclus au forfait** | Mensualité fixe tout compris | Prévisible | Dérive si volume explose |

### 22.2 Calculer un coût-page rentable
```
Coût-page = (toner/page + pièces/page + MO/visites + marge) × 1,2
```
- **Toner/page** : prix cartouche ÷ rendement (à 5 % de couverture ;
  en Afrique, la couverture réelle est souvent 8–12 % → ajustez !).
- **Pièces/page** : (prix kit ÷ durée de vie) par pièce.
- **MO** : visites préventives/an × coût visite ÷ volume annuel.
- **La couverture** : demandez au client des exemples d'impressions
  types. Un client « bureautique » à 10 % de couverture use 2× plus
  de toner que le calcul à 5 %.

### 22.3 Le SLA (engagement de service)
- Définissez : délai d'intervention (ex. 4h ouvrées), délai de
  résolution, machine de prêt si > 48h, pénalités.
- **Ne promettez que ce que vous pouvez tenir** : un SLA non tenu
  coûte plus cher qu'un SLA modeste tenu.
- Exclusions : papier non conforme, toner non d'origine, vandalisme,
  coupures électriques sans onduleur.

### 22.4 Fidéliser
- Rapport de visite **écrit** à chaque passage (compteurs, actions,
  préconisations) → le client voit le travail.
- Alerte proactive : « votre tambour est à 85 %, prévoyons le
  remplacement le mois prochain » → pas de panne = client heureux.
- Formation utilisateurs (15 min) : divise les appels « bourrage »
  par 2.

---

## 23. Diagnostic avancé

### 23.1 Au multimètre
- **Lampe de fusion** : continuité (hors tension, connecteur débranché).
  Infini = lampe coupée.
- **Thermistance** : résistance variable avec la température
  (comparez à la doc ; valeur aberrante = HS).
- **Moteur** : résistance d'enroulement (infini = coupé, 0 = court-circuit).
- **Alimentations** : 24 V / 5 V / 3,3 V présents sur la carte DC ?
  (machine sous tension, **prudence**.)

### 23.2 Lire les compteurs d'usure comme un pro
- COUNTER > PARTS : ne regardez pas seulement le % — regardez la
  **vitesse d'usure** (photos à 3 mois d'intervalle). Une pièce qui
  s'use 2× plus vite que la théorie = cause externe (papier,
  environnement, toner).
- Corrélez : fond gris + développeur à 90 % + toner compatible =
  diagnostic en 2 minutes.

### 23.3 La méthode des 5 pourquoi (exemple)
> Bourrages 0108 récurrents.
> Pourquoi ? → Rouleaux lisses. Pourquoi ? → Poussière + papier bas
> de gamme. Pourquoi ? → Client achète le moins cher. Pourquoi ? →
> Personne ne lui a expliqué le lien. → **Action** : former le client
> + imposer un papier validé au contrat. Problème réglé durablement.

---

## 24. Cas pratiques commentés (10 cas terrain)

**Cas 1 — E000 un lundi matin après un week-end orageux**
Micro-coupures → surtension → thermistance affolée. Laisser refroidir,
CLEAR > ERR. Si OK : vendre un onduleur + parafoudre au client
(argument : « ça a failli coûter une carte »).

**Cas 2 — Fond gris 2 semaines après un toner « pas cher »**
Toner compatible. Solution : aspirer le circuit, remettre toner
origine, ajustement DENS. Expliquer au client le coût réel
(développeur à remplacer à 200k au lieu de 400k).

**Cas 3 — 0108 tous les lundis**
Papier chargé le vendredi, humidité du week-end (local non climatisé).
Solution : ne charger que le nécessaire, stocker la rame emballée,
ventiler avant chargement.

**Cas 4 — Couleurs décalées après déménagement**
Choc pendant le transport → ITB décalée. Registration auto
(FUNCTION > ADJUST > REGIST). Si échec : vérifier la courroie.

**Cas 5 — E602 après coupure CIE**
HDD corrompu. CHK-HDD, si échec → remplacement + SST. Vendre
l'onduleur. (Cas ultra-fréquent en Afrique de l'Ouest.)

**Cas 6 — Scan SMB mort après mise à jour Windows**
Passer le copieur en SMBv2/3 (firmware + réglage) ou créer un partage
avec SMBv2 forcé. Alternative : basculer en scan-to-email.

**Cas 7 — Bourrage 0301 + toner qui s'efface**
Fusion froide : thermistance encrassée ou lampe faible. Nettoyer,
tester, remplacer l'unité si compteur > 80 %.

**Cas 8 — Lignes noires verticales toujours au même endroit**
Tambour rayé (agrafe passée dans l'ADF ? corps étranger ?).
Remplacer le tambour, chercher la cause (sinon le neuf se raye aussi).

**Cas 9 — Le client dit « elle imprime pâle » mais les copies sont OK**
Problème **pilote/PC**, pas copieur : vérifier le pilote (UFR II à
jour), les paramètres d'impression (brouillon ?), tester depuis un
autre PC.

**Cas 10 — Agrafeuse qui se bloque chaque semaine**
Agrafes compatibles. Passer aux agrafes Canon d'origine + nettoyer
le mécanisme. Coût des compatibles < coût de vos déplacements.

---

## 25. Fiches modèles — les plus courants en Afrique

### 25.1 imageRUNNER 2520 / 2530 / 2545 (N&B A3)
- La bête de somme : simple, robuste, pièces disponibles.
- Points faibles : rouleaux pickup (poussière), HDD vieillissant.
- Maintenance : kit rollers à 100k, tambour ~150k.
- Idéal en contrat coût-page (prévisible).

### 25.2 imageRUNNER ADVANCE C5500 series (couleur A3)
- 4 tambours + ITB : maintenance plus fine (registration couleurs).
- Points faibles : développeurs (toner compatible = mort),
  unité de fusion (compteur à surveiller).
- Toujours calibrer après intervention.

### 25.3 imageRUNNER ADVANCE DX C5800 series (génération actuelle)
- Sécurité renforcée (TPM, McAfee) : ne pas désactiver.
- Firmware : maintenir à jour (SMB, failles).
- Même base mécanique que C5500 : vos réflexes restent valables.

---

## 26. Sécurité de l'intervention

- **Électrique** : débrancher avant tout démontage (pas seulement
  éteindre — l'interrupteur ne coupe pas tout). Condensateurs :
  attendre 5 min après débranchement.
- **Fusion** : brûlure à 200 °C — attendre le refroidissement,
  gants thermiques si urgent.
- **Toner** : masque FFP2, ne pas inhaler, ne pas utiliser d'air
  comprimé sur du toner en vrac (nuage explosif en théorie,
  encrassement garanti en pratique → aspirateur à toner).
- **Laser** : classe 1 en fonctionnement normal, mais **ne jamais**
  regarder le faisceau unité ouverte (classe 3B à l'intérieur).
- **Poids** : un copieur A3 = 100–200 kg. Transport à 2 minimum,
  diable, jamais par les trappes.

---

## 27. Former le client — diviser les appels par 2

Formation express de 15 minutes à chaque installation :
1. Charger le papier (ventiler, guides, pas trop remplir).
2. Sortir un bourrage (suivre l'écran, sens du papier, tout refermer).
3. Changer le toner (sans secouer comme un shaker).
4. Ce qu'il ne faut JAMAIS faire : agrafes dans l'ADF, papier humide,
   éteindre en arrachant la prise, toner compatible.
5. Qui appeler et quoi noter (code affiché à l'écran).

Un client formé = 50 % d'appels en moins = votre marge.

---

---

## 28. Le chemin papier en détail — chaque rouleau compte

> Comprendre le trajet exact du papier = localiser tout bourrage
> sans hésiter.

```
Cassette → [Pickup roller] → [Feed roller + Separation roller]
         → [Registration rollers] → [Transfert (tambour/ITB)]
         → [Séparation] → [Fixing (fusion)] → [Sortie]
                                                    ↓ (duplex)
                                              [Inverseur] → [Duplex]
                                                    ↓ (finisher)
                                              [Finisher]
```

### 28.1 Rôle de chaque rouleau
- **Pickup** : prend la feuille en haut de la pile (usure = 0108).
- **Feed** : avance la feuille (usure = retard 0101).
- **Separation** : retient les feuilles suivantes (usure = multi-prise).
- **Registration** : synchronise la feuille avec l'image (sale =
  décalage d'image, bourrage 0201).
- **Sortie/duplex** : éjecte ou réintroduit (usure = 0301/0A01).

### 28.2 Quand remplacer quoi (symptômes)
- Multi-prises → **separation** en priorité.
- Non-prise → **pickup** en priorité.
- Retards sans cause papier → **feed**.
- Toujours remplacer en **kit complet** (les 3) : un neuf + deux usés
  = déséquilibre de friction = nouveaux bourrages.

---

## 29. L'unité laser (LSU) — diagnostic

- Rôle : « dessine » l'image sur le tambour (étape 2 du §2).
- Symptômes : page blanche totale, image pâle uniforme, bandes
  blanches verticales (poussière sur la vitre du laser).
- **Nettoyage** : la vitre/lentille du laser s'encrasse (toner en
  suspension) → chiffon sec non pelucheux, jamais de liquide à
  l'intérieur.
- **Sécurité** : ne jamais alimenter le laser capot ouvert sans les
  protections (rayonnement classe 3B).
- Si le polygone (miroir rotatif) est bruyant → unité à remplacer
  (pas réparable en atelier).

---

## 30. Haute tension — charge, transfert, séparation

- La machine génère des **hautes tensions** (jusqu'à −6 kV pour la
  charge primaire) via la carte **HVT** (High Voltage Transformer).
- Symptômes :
  - Fond gris/noir → charge primaire trop faible.
  - Image pâle/effacée → transfert trop faible.
  - Papier qui colle au tambour → séparation trop faible.
- Diagnostic :
  1. Nettoyer les coronas/fils de charge (outil de nettoyage fourni,
     ou coton-tige + alcool).
  2. Vérifier les contacts (lames de contact pliées = classique après
     remplacement tambour).
  3. Mesurer les sorties HVT (multimètre HT ou selon manuel — **danger**,
     personnel qualifié uniquement).
- **Grille de charge (grid)** : encrassée = charge irrégulière =
  bandes. Nettoyage systématique à chaque remplacement tambour.

---

## 31. FAX — installation et pannes

- Ligne analogique RTC requise (les box « voix sur IP » posent
  problème : **désactiver l'ECM** si échecs, passer en 9 600 bps).
- Réglages : n° de fax, nom d'émetteur, mode de réception (auto/manuel),
  renvoi vers email/dossier (pratique : fax → PDF par mail).
- Pannes classiques :
  - **E674** → carte fax (réinstaller, vérifier).
  - Échec d'envoi → ligne (tester avec un téléphone), ECM, vitesse.
  - Réception noire → cartouche/toner (côté réception c'est une
    impression normale).
- En Afrique : le fax décline, mais les administrations et banques
  l'exigent encore → savoir le configurer reste utile.

---

## 32. Impression suiveuse et uniFLOW

- **Problème** : documents abandonnés sur l'imprimante (confidentialité,
  gaspillage).
- **Solution Canon : uniFLOW** — l'utilisateur s'authentifie (badge,
  code) et libère ses impressions sur n'importe quel copieur du parc.
- Avantages : confidentialité, quotas, rapports par utilisateur,
  suppression auto des travaux non libérés.
- En maintenance : savoir réenregistrer un copieur dans uniFLOW
  (adresse serveur, certificat) après remplacement de carte/HDD.

---

## 33. Dépannage réseau avancé

```
1. Le copieur a-t-il une IP ? → Panneau > Réseau > TCP/IP
2. Ping depuis un PC → OK ? sinon : câble, switch, VLAN
3. Page web embarquée (http://IP) accessible ? → si non : HTTP désactivé ou carte NIC
4. Port 9100 (RAW) / 515 (LPD) ouverts ? → pare-feu
5. File d'impression Windows : erreur ? → pilote, spouleur (redémarrer le spouleur)
6. Wireshark si besoin : le PC envoie-t-il au copieur ?
```

- **VLAN** : en entreprise, les copieurs sont souvent sur un VLAN
  dédié → le PC d'un autre VLAN ne les « voit » pas sans routage.
- **Wi-Fi** : les copieurs en Wi-Fi perdent la connexion (mode éco) →
  privilégiez le **câble**.
- **IPv6** : source de bizarreries → désactivez-le si non utilisé.

---

## 34. Gestion de parc (flotte de copieurs)

- **Outil Canon : e-Maintenance / imageWARE** — remontée automatique
  des compteurs, alertes toner, historiques. Proposez-le à vos clients
  multi-sites : vous êtes alerté **avant** la panne.
- **Standardisation** : un seul modèle par client si possible
  (stock de pièces unique, formation unique).
- **Rotation** : les machines à faible volume vieillissent mal
  (toner qui s'agglutine) → planifiez des impressions de test.
- **Fin de vie** : à 5–7 ans ou 1M de pages, le coût de maintenance
  dépasse le remplacement → proposez le renouvellement **avant**
  la panne fatale (argument chiffré avec l'historique).

---

## 35. Pièces détachées — références et compatibilités

- **Toujours commander par référence exacte** (ex. : `GPR-57` pour les
  toners — la référence change par série !).
- Compatibilités : un tambour de C5535 ne va pas sur un C5560 même si
  ça ressemble → vérifiez la **parts list** du manuel de service.
- **Pièces d'occasion** : acceptable pour cartes/cartes mères (testées),
  **jamais** pour tambour/développeur/fusion (usure inconnue).
- Fournisseurs : distributeur Canon agréé (garantie) vs grossistes
  (prix). En Afrique : anticipez **3–6 semaines** de délai → stock
  tampon obligatoire.

---

## 36. Questions pièges (pour valider vos connaissances)

1. Pourquoi ne faut-il pas secouer une cartouche de toner neuve ?
   → Poussière en suspension, risque d'inhalation, bourrage de la vis sans fin.
2. Un client a des bourrages 0108 uniquement le matin. Pourquoi ?
   → Humidité nocturne du papier (local non climatisé).
3. Après un remplacement de développeur, les copies sont trop foncées.
   → Initialisation STIR/TONER-S non faite.
4. E602 après un orage. Premier réflexe ?
   → CHK-HDD en mode service, puis remplacement + SST si échec.
5. Le scan SMB marchait hier, plus aujourd'hui, rien n'a changé sur le copieur.
   → Mot de passe Windows expiré ou mise à jour Windows (SMB).
6. Des points se répètent tous les 94 mm.
   → Tambour (circonférence).
7. Pourquoi reset les compteurs PARTS après remplacement ?
   → Sinon la machine applique les corrections d'usure de l'ancienne pièce.
8. Un copieur réformé part chez un ferrailleur. Qu'oubliez-vous ?
   → Effacement du disque dur (données clients).

---

---

## 37. Démontage — méthode et accès aux cartes

> 80 % des dégâts en maintenance viennent du démontage, pas de la panne.

### 37.1 Méthode générale
1. Photo **avant** (câbles, nappes, vis).
2. Vis : posez-les sur un plan **dans l'ordre** (scotch + schéma papier).
   Les vis Canon ont des longueurs différentes : une vis trop longue
   dans un trou de nappe = carte percée.
3. Nappes : soulevez le **verrou** du connecteur avant de tirer
   (jamais tirer sur la nappe elle-même).
4. Connecteurs : ne forcez jamais — s'il résiste, il y a un verrou.
5. Remontage : rebranchez **tout** avant de revisser les capots
   (test intermédiaire).

### 37.2 Accès carte principale / DC controller
1. Capot arrière (4–6 vis), repérer la cage des cartes.
2. Débrancher les nappes (photo !), dévisser la carte.
3. **ESD** : bracelet antistatique ou touchez une masse avant de
   manipuler (une décharge tue une carte à 500 €).
4. Au remontage : vérifier chaque nappe est bien enfoncée **jusqu'au
   clic** (la moitié des E240/E711 viennent d'une nappe mal remise).

---

## 38. Refroidissement et gestion thermique

- Un copieur A3 dégage **1–2 kW** de chaleur en production : le local
  doit être ventilé/climatisé (sinon E000 en cascade l'été).
- Circuit : ventilateurs d'admission → cartes → ventilateur
  d'évacuation + ventilateur de la fusion.
- Maintenance : dépoussiérage des grilles **à chaque visite**
  (poussière = isolation thermique), vérifier le sens de rotation
  des ventilateurs après remplacement.
- Symptômes de surchauffe : E000/E001 sans cause fusion, ralentissement,
  erreurs aléatoires (E315, E240 fantômes).

---

## 39. Les capteurs — types et tests

| Type | Rôle | Test |
|---|---|---|
| Photo-interrupteur | Détection papier | DISPLAY > SENSOR, passer une feuille |
| Thermistance | Température fusion | Résistance vs température (manuel) |
| Capteur de densité | Patch toner | Nettoyer, DENS |
| Capteur de niveau toner | Fin de toner | Secouer doucement la cartouche |
| Capteur de porte | Sécurité | Ouvrir/fermer, état SENSOR |
| Capteur d'humidité/température | Environnement | DISPLAY > ANALOG |

- **90 % des « capteurs HS » sont des capteurs sales** : nettoyez
  avant de commander.
- Les capteurs optiques vieillissent (LED qui faiblit) : si le
  nettoyage ne suffit plus après 5 ans, remplacez.

---

## 40. Bonus — scanners imageFORMULA

- Pas de toner ni de tambour : la maintenance = **rouleaux + vitres**.
- Kit de rouleaux (pickup/feed/separation) à changer selon le compteur
  (souvent 200k–600k passages).
- Nettoyage : vitres CIS + rouleaux à l'alcool isopropylique,
  **kit de nettoyage Canon** (lingettes).
- Bourrages : guides mal réglés, documents agrafés/froissés,
  rouleaux lisses.
- Pilote : **CaptureOnTouch** (Canon) — à maintenir à jour.

---

## 41. Modèles A4 récents (imageRUNNER 1600/2600 series)

- N&B A4, remplace les 1435 : simples, pour petits bureaux.
- Maintenance allégée : cartouche **tout-en-un** (toner + tambour),
  peu de pièces détachées.
- Points faibles : chargeur ADF (rouleaux), bac papier (guides).
- En contrat : coût-page simple à calculer (une seule cartouche).

---

## 42. Lexique service étendu

| Abréviation | Signification |
|---|---|
| DC-CON | Carte contrôleur DC (moteur) |
| MAIN-CON | Carte contrôleur principale |
| HVT | Transformateur haute tension |
| LSU | Unité laser (Laser Scanner Unit) |
| CIS | Capteur d'image par contact (scanner) |
| CCD | Capteur d'image (anciens scanners) |
| ITB | Courroie de transfert intermédiaire |
| ETB | Courroie de transfert électrostatique (N&B) |
| OPC | Tambour photoconducteur organique |
| DADF | Chargeur recto-verso automatique |
| RADF | Chargeur recto-verso (ancien terme) |
| SST | Service Support Tool |
| MEAP | Plateforme applicative Canon |
| UFR II | Langage d'impression Canon |
| PCL | Langage d'impression universel |
| PS | PostScript (option) |
| SMB | Partage de fichiers Windows |
| LDAP | Annuaire (carnet d'adresses d'entreprise) |
| TPM | Puce de sécurité (chiffrement) |
| E-RDS | Diagnostic à distance Canon |

---

---

## 43. Fiches d'intervention types (templates)

### 43.1 Rapport de visite préventive
```
Date : ___  Client : ___  Machine : ___ (n° série : ___)
Compteur total : ___  Couleur : ___  N&B : ___
Erreurs relevées : ___
Bourrages relevés : ___
Consommables : toner N ___% C ___% M ___% J ___% / bac usagé ___%
Pièces (PARTS) : tambour ___%  fusion ___%  ITB ___%  développeur ___%
Actions : nettoyage optiques / rouleaux / calibration / firmware ___
Préconisations : ___
Prochaine visite : ___
Signature technicien : ___  Signature client : ___
```

### 43.2 Rapport de dépannage
```
Date : ___  Appel client : ___
Symptôme décrit : ___
Code erreur / jam : ___
Diagnostic : ___
Pièces remplacées (références) : ___
Réglages modifiés (anciennes valeurs notées) : ___
Tests effectués : ___
Résultat : OK / à suivre : ___
```

---

## 44. Garanties et litiges

- Garantie constructeur : 1 an pièces (hors consommables), main-d'œuvre
  selon contrat. **Le toner compatible annule la garantie** sur le
  circuit image.
- Garantie de votre intervention : 3 mois minimum sur la pièce
  remplacée (standard pro).
- Litige « ça marchait avant votre passage » : votre **rapport écrit**
  avec compteurs et photos = votre protection. Toujours faire signer.
- Pièce DOA (morte à l'arrivée) : ne jamais la monter de force —
  photo, retour fournisseur immédiat.

---

## 45. Ce qui change (2024-2026)

- **Sécurité** : TPM + McAfee de série (DX), chiffrement disque par
  défaut → les procédures HDD changent (sauvegarde de clé obligatoire).
- **SMB** : SMBv1 mort partout → firmwares récents indispensables.
- **Cloud** : impression/scannage cloud natif (OneDrive, Google Drive,
  Dropbox) → moins de serveurs locaux, plus de config réseau/web.
- **Consommables** : cartouches tout-en-un sur de plus en plus de
  modèles (maintenance simplifiée, mais coût-page à recalculer).
- **E-Maintenance** : la remontée auto des compteurs devient le standard
  des contrats → investissez dans la plateforme, c'est votre vigie.

---

## 46. Ressources

- **Manuels de service** : via le distributeur Canon agréé (accès
  partenaire). Indispensables pour les vues éclatées et les valeurs
  de réglage exactes.
- **SST + firmwares** : portail partenaire Canon.
- **Communautés** : forums de techniciens (Copytechnet, groupes
  Facebook/WhatsApp de techniciens Afrique) — entraide sur les pannes
  tordues.
- **Formation** : académies Canon régionales, formations fabricants
  (Deye/Victron pour le solaire, Canon pour le print — même logique).

---

## 47. Index rapide des codes

- **E000–E009** : fusion → §5.1
- **E010–E019** : moteurs, toner usagé → §5.1/§5.2
- **E020–E028** : densité toner → §5.2
- **E110–E301** : scanner/lecteur → §5.3
- **E240–E749** : contrôleur/communication/cartes → §5.4
- **E400–E490** : ADF → §5.5
- **E500–E5F5** : finisher → §5.6
- **E602–E630** : disque/mémoire/firmware → §5.4
- **E674–E699** : fax/cartes → §5.4
- **E700–E799** : modules/options → §17
- **E800–E84x** : ventilateurs/moteurs → §5.7/§17
- **Jam 00xx/01xx/02xx/03xx/0Axx/0Dxx** : bourrages → §6/§18

---

---

## 48. Annexe — durées de vie par famille (repères terrain)

> Ordres de grandeur constatés en Afrique de l'Ouest (chaleur,
> poussière, papier variable). Divisez par ~1,5 les valeurs
> constructeur « conditions idéales ».

| Pièce | iR 2520 (N&B A3) | iR-ADV C5500 (coul. A3) | DX C5800 (coul. A3) |
|---|---|---|---|
| Toner (cartouche std) | ~15k pages | ~20–30k (N) / ~15–20k (C/M/J) | idem C5500 |
| Tambour | ~150k | ~150–200k par couleur | ~150–200k |
| Développeur | ~300k | ~300–400k par couleur | ~300–400k |
| Unité de fusion | ~300k | ~300–400k | ~400–500k |
| ITB | — (transfert direct) | ~300k | ~300–400k |
| Kit rollers | ~100k | ~120k | ~150k |
| Bac toner usagé | ~50k | ~50–80k | ~50–80k |

## 49. Les 10 fautes professionnelles à ne jamais commettre

1. Monter un toner compatible chez un client sous contrat.
2. Oublier le reset PARTS après remplacement (corrections d'usure fausses).
3. Toucher la surface du tambour à mains nues.
4. Couper la machine en arrachant la prise (HDD).
5. Modifier un réglage ADJUST sans noter l'ancienne valeur.
6. Remonter sans rebrancher une nappe (E240 garanti).
7. Jeter un disque dur de copieur sans effacement.
8. Promettre un SLA intenable pour signer.
9. Intervenir sur la HVT sans qualification (haute tension).
10. Partir sans faire signer le rapport d'intervention.

## 50. Dépannage par téléphone — le script

Avant de vous déplacer, 5 minutes au téléphone économisent 2 heures :
1. « Que dit **exactement** l'écran ? » (code ou message)
2. « Depuis quand ? Après quel événement ? » (orage, déménagement, nouveau papier)
3. « Le papier utilisé a-t-il changé ? » (50 % des cas)
4. « Les trappes sont-elles bien fermées ? » (bourrage fantôme)
5. « Éteignez, attendez 2 minutes, rallumez — que se passe-t-il ? »
→ Si le code persiste : préparez la pièce probable **avant** de partir
(consultez §5/§17 avec le code donné).

---

---

## 51. Magasins papier grande capacité (LCT) et options

- **LCT (Large Capacity Tray)** : 2 000–3 000 feuilles. Ses pannes
  propres : moteur de montée du plateau (le plateau ne monte plus →
  0108), capteur de niveau, rouleaux spécifiques.
- **Ponte de passage (pass-through)** : entre copieur et finisher —
  bourrages 0Dxx fréquents si mal aligné après déplacement.
- **Module d'insertion** : bourrages si papier différent non déclaré.
- Règle : après tout déplacement de la machine, **réalignez les
  modules** (LCT, finisher) — 1 cm de décalage = bourrages en série.

## 52. Mots de passe et verrouillages à connaître

| Accès | Défaut usine (à changer !) |
|---|---|
| Mode service | `* 2 8 *` (pas de mot de passe, accès physique) |
| Admin système (Settings/Registration) | ID : `7654321` / PIN : `7654321` (séries récentes) |
| Anciennes séries | ID : `1` / PIN : `1111` ou sans |

- **Changez les mots de passe admin** à l'installation (sinon
  n'importe qui modifie le réseau/SMTP).
- Notez-les dans votre dossier client (chiffré) — un admin perdu =
  reset usine = perte de tous les réglages.

---

---

## 53. Que mettre dans la valise avant de partir (par symptôme)

> Le technicien qui part sans la pièce probable perd une demi-journée.
> Préparez d'après l'appel (§50).

| Symptôme annoncé | Emporter |
|---|---|
| E000/E001/E002 | Unité de fusion (ou thermistances + lampe) |
| E020/E025 | Toner origine + développeur |
| E202/E225 | Kit de nettoyage optiques + nappe ADF |
| E400–E413 | Kit rollers ADF + courroies |
| E514/E540/E590 | Agrafes origine + kit de nettoyage capteurs |
| E602 | HDD/SSD compatible + clé USB SST + firmware |
| E805/E824 | Ventilateurs (2 modèles courants) |
| Bourrage 0108 | Kit rollers complet + rame de papier de test |
| Bourrage 0301 | Unité de fusion ou doigts de séparation |
| Fond gris | Toner origine + développeur + outil nettoyage corona |
| Lignes verticales | Tambour de la couleur concernée |
| Scan SMB mort | PC portable (test réseau, firmware) |
| N'imprime plus | Câble réseau, pilote sur clé USB |

---

---

## 54. Plan de formation — technicien débutant (4 semaines)

- **Semaine 1** : théorie (§2), démontage/remontage (§37), nettoyages
  (§4), changement toner et bourrages (§6) — sur machine d'atelier.
- **Semaine 2** : mode service (§8), E-codes courants (§5),
  remplacement rollers et tambour (§9), qualité d'image (§7).
- **Semaine 3** : réseau et scan (§10, §33), firmware SST (§11),
  fusion et ITB (§9), sécurité (§26).
- **Semaine 4** : tournées accompagnées, rapports d'intervention
  (§43), script téléphonique (§50), puis autonomie sur les cas
  simples avec un senior en backup téléphonique.

---

---

## 55. Pense-bête de poche (à garder sur soi)

```
Mode service : * 2 8 *
Clear erreur : COPIER > FUNCTION > CLEAR > ERR
Historique   : DISPLAY > ERR / JAM / ALARM
Compteurs    : COPIER > COUNTER > TOTAL / PARTS
Test moteurs : FUNCTION > PART-CHK
Capteurs     : DISPLAY > SENSOR
Effacer HDD  : FUNCTION > SYSTEM > HDD erase
Jam 01xx pickup / 02xx registration / 03xx fusion / 0Axx duplex
E0xx fusion / E02x toner / E40x ADF / E5xx finisher / E6xx disque
94 mm = tambour / 60 mm = développeur (intervalles de défauts)
Règle d'or : photo avant, reset PARTS après, rapport signé.
```

---

*Fin du guide — 1500+ lignes. La maintenance Canon tient en une phrase :
**préventif régulier, pièces d'origine, papier de qualité, firmware à jour,
et tout noter.** Le reste, c'est de l'expérience — et ce guide est fait
pour l'accélérer.*
