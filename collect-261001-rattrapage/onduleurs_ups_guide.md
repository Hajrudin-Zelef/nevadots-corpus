# Onduleurs / UPS 10–120 kVA — Guide ultra-complet (exploitation & maintenance)

> Tout sur les onduleurs triphasés 10, 20, 40, 60 et 120 kVA :
> topologies, dimensionnement, Easy UPS et concurrents, batteries,
> installation électrique, mise en service, maintenance préventive
> totale, alarmes, sécurité, groupes électrogènes, monitoring,
> management d'équipe et contrats — **angle chef de service
> systèmes & énergies**.
>
> Rédigé à partir des connaissances du domaine (2025-2026). Les
> valeurs exactes (tensions batterie, seuils, codes) varient selon
> les modèles : **validez toujours sur la fiche technique et le
> manuel du modèle installé**.

---

## 1. Le rôle du chef de service systèmes & énergies

- Vous êtes responsable de la **continuité électrique** : un UPS qui
  tombe = data center, bureaux, ateliers à l'arrêt.
- Vos 4 piliers : **dimensionner juste, installer propre, maintenir
  en préventif, superviser en continu**.
- Ce guide couvre les 4 — et donne à votre équipe des procédures
  exécutables, pas de la théorie.

---

## 2. Les 3 topologies d'onduleurs (norme IEC 62040-3)

| Topologie | Principe | Temps de transfert | Usage |
|---|---|---|---|
| **VFD** (offline/standby) | Le réseau alimente la charge ; l'onduleur ne démarre qu'en coupure | 4–10 ms | Petits bureaux, < 2 kVA |
| **VI** (line-interactive) | Régulateur (AVR) + bascule sur batterie en coupure | 2–4 ms | 1–10 kVA, bureautique |
| **VFI** (online double conversion) | La charge est **toujours** alimentée par l'onduleur | **0 ms** | 10–120 kVA et + : data centers, industrie |

- **Toutes les puissances de ce guide (10–120 kVA) sont en VFI
  double conversion.** C'est le seul choix professionnel à ces
  puissances : zéro micro-coupure, tension et fréquence régénérées.
- Classification IEC : `VFI-SS-111` (le code complet figure sur la
  plaque — sachez le lire).

---

## 3. La double conversion en détail — les 4 blocs

```
RÉSEAU ──► [REDRESSEUR] ──► BUS DC ──► [ONDULEUR] ──► CHARGE
                              │                        ▲
                         [BATTERIES]                   │
                              │                   [BYPASS STATIQUE]
                         (secours DC)                      │
                                              RÉSEAU ───────┘
```

### 3.1 Le redresseur (AC → DC)
- Convertit le 400 V AC en DC (~384–480 V selon modèles).
- **IGBT** sur les modèles récents : facteur de puissance d'entrée
  > 0,99, THDi < 3 % → ne pollue pas le réseau, compatible groupes.
- Anciens modèles à thyristors : THDi 25–30 % → prévoir filtres.

### 3.2 Le bus DC
- Le « cœur » : tension continue stabilisée (~384–480 V).
- **Condensateurs électrolytiques** : pièce d'usure n°1 après les
  batteries (5–7 ans en chaud). Surveillance au §21.

### 3.3 L'onduleur (DC → AC)
- Régénère un 400 V / 50 Hz **parfait**, isolé des perturbations
  réseau (creux, surtensions, harmoniques, variations de fréquence).
- Rendement double conversion : **95–96 %**. Mode ECO : jusqu'à
  **99 %** (la charge repasse sur le réseau filtré — à n'utiliser
  que si le réseau est de bonne qualité).

### 3.4 Les batteries
- Branchées sur le bus DC via un **disjoncteur batterie**.
- En régime normal : maintenues en floating. En coupure : elles
  alimentent le bus → **aucune interruption** (0 ms).

### 3.5 Le bypass
- **Bypass statique** (thyristors) : bascule automatique sur le
  réseau en cas de surcharge ou défaut onduleur (sans coupure).
- **Bypass de maintenance** (manuel) : isole totalement l'UPS pour
  intervenir **sans couper la charge**.
- ⚠️ En bypass, la charge n'est **plus protégée** : à n'utiliser
  que le temps de l'intervention.

---

## 4. kVA vs kW — le facteur de puissance

- **kVA** = puissance apparente (ce que l'UPS peut fournir).
- **kW** = puissance réelle consommée = kVA × cos φ.
- Les UPS modernes affichent un **PF de sortie de 0,9 à 1,0**
  (unity) : un 40 kVA / PF 0,9 = **36 kW** utiles.
- **Règle de dimensionnement** : ne jamais dépasser **80 %** de
  charge en nominal → marge pour les pics et l'avenir.
  - Exemple : besoin 28 kW → 28 / 0,9 / 0,8 ≈ 39 kVA → **40 kVA**.

---

## 5. Dimensionnement — méthode et exemples par puissance

### 5.1 La méthode en 5 étapes
1. **Inventaire des charges** (kW + cos φ de chaque équipement).
2. **Somme** → P totale.
3. **kVA = P / PF_sortie_UPS**, puis **÷ 0,8** (marge 80 %).
4. **Arrondir** au standard supérieur (10/20/40/60/120).
5. **Autonomie batterie** : définir (10–15 min standard = le temps
   que le groupe démarre ; 30–60 min si pas de groupe).

### 5.2 Exemples types

| UPS | Charge cible (≈80 %) | Usage typique |
|---|---|---|
| **10 kVA** (~9 kW) | 7 kW | Petite salle serveurs, agence bancaire |
| **20 kVA** (~18 kW) | 14 kW | Salle serveurs PME, petit data center |
| **40 kVA** (~36 kW) | 29 kW | Data center PME, site industriel |
| **60 kVA** (~54 kW) | 43 kW | Data center moyen, hôpital (partiel) |
| **120 kVA** (~108 kW) | 86 kW | Data center, gros site, N+1 de 60 kVA |

### 5.3 Ne pas oublier dans l'inventaire
- **Climatisation** de la salle (souvent le plus gros poste !).
- Courants d'appel (démarrage moteurs, inrush serveurs).
- Croissance **3 ans** : +20–30 % de marge.
- Charges non linéaires (harmoniques) : prévoir le déclassement si
  l'UPS est ancien.

---

## 6. Schneider Electric Easy UPS — la gamme

> La gamme « Easy » = le bon rapport qualité/prix de Schneider pour
> l'Afrique. Vérifiez les fiches exactes — les déclinaisons évoluent.

| Gamme | Puissance | Positionnement |
|---|---|---|
| **Easy UPS 3S** | 10, 15, 20, 30, **40 kVA** (tri/tri 400 V) | Data centers PME, industrie légère |
| **Easy UPS 3M** | **60** à **120 kVA** (tri/tri 400 V) | Sites moyens, N+1 |
| **Easy UPS 3L** | 250–600 kVA | Gros data centers (hors sujet ici) |
| Smart-UPS (APC) | jusqu'à 10 kVA (mono) | Bureaux, petites salles |

- Points forts Easy : double conversion IGBT, rendement ~96 %,
  carte réseau en option, prix contenu, réseau de distribution
  large en Afrique.
- **À vérifier à l'achat** : PF de sortie exact, plage batteries
  acceptée (32–40 blocs ?), carte NMC incluse ou en option,
  garantie et disponibilité pièces locale.

---

## 7. Les concurrents — repères

| Marque | Gamme équivalente | Note |
|---|---|---|
| **Eaton** | 93E / 93PS (10–120 kVA) | Très bon rendement, robuste |
| **Vertiv** (ex-Emerson) | Liebert EXS / ITA2 | Data centers, monitoring avancé |
| **Riello** | Sentryum / Multi Sentry | Bon rapport qualité/prix |
| **Socomec** | MODULYS / DELPHYS | Modulaire (hot-swap) |
| **Huawei** | UPS5000-A (30–120 kVA) | Modulaire, prix agressif |

- En maintenance multi-marques : les **principes sont identiques**
  (bus DC, floating, bypass) — seuls les menus, codes et pièces
  changent.

---

## 8. Batteries — VRLA vs Lithium

### 8.1 VRLA AGM (plomb étanche) — le standard
- Blocs 12 V (6 éléments de 2 V), design life **3–5 ans**
  (standard) ou **10–12 ans** (longue vie, Eurobat 10+).
- **En Afrique (30–35 °C) : divisez par 2** (loi d'Arrhenius :
  +10 °C = vie / 2). D'où l'importance de la clim du local.
- Pas d'entretien d'électrolyte, mais **surveillance obligatoire**.

### 8.2 Lithium-ion (LFP)
- Vie 10–15 ans, 2× plus d'énergie au m³, pas de floating
  permanent, monitoring par BMS intégré.
- Coût initial 2–3× supérieur, mais TCO souvent meilleur.
- ⚠️ Compatibilité : vérifier que l'UPS accepte le Li-ion
  (courbes de charge, communication BMS).

### 8.3 Les paramètres de charge (VRLA)
- **Floating** : 2,25–2,27 V/élément (13,5–13,6 V/bloc 12 V) à 20 °C.
- **Boost/égalisation** : 2,35–2,40 V/élément (occasionnel).
- **Compensation température** : −3 mV/°C/élément (indispensable
  en climat chaud — sinon surcharge et gonflement).
- **Tension de coupure** : ~1,67 V/élément (ne jamais descendre
  en dessous durablement).
---

## 9. Dimensionnement batteries et autonomie — les calculs

### 9.1 La formule pratique (méthode énergie)
```
Capacité (Ah) = P_charge_W × autonomie_h × 1000
                ─────────────────────────────────
                V_bus_DC × η_onduleur × η_batterie
```
- η_onduleur ≈ 0,95 ; η_batterie ≈ 0,85–0,9.
- **Exemple** : 30 kW, 15 min (0,25 h), bus 384 V :
  Ah = 30 000 × 0,25 / (384 × 0,95 × 0,85) ≈ **24 Ah**
  → 2 strings de 32 blocs 12 V / 12 Ah (ou 1 string de 24 Ah).
- ⚠️ C'est une **estimation** : validez avec les tables de décharge
  à puissance constante du fabricant de batteries ou le
  configurateur constructeur (Schneider UPS Selector, etc.).

### 9.2 Nombre de blocs — les standards
- Bus **384 V** = 32 blocs de 12 V (192 éléments de 2 V).
- Bus **480 V** = 40 blocs de 12 V.
- Plage acceptée par l'UPS : vérifiez (ex. 32–40 blocs) — le nombre
  influence l'autonomie et le courant de décharge par bloc.

### 9.3 Strings en parallèle
- 2 strings = 2× l'autonomie (ou 2× le courant admissible).
- **Chaque string a son disjoncteur** + fusibles.
- Strings **identiques** (même marque, même âge, même capacité) —
  jamais de mélange neuf/vieux.

### 9.4 Autonomies recommandées
- **Avec groupe électrogène** : 10–15 min (le temps du démarrage +
  marge).
- **Sans groupe** : 30–60 min (le temps d'arrêter proprement).
- Data center critique : 15 min + groupe avec contrat carburant.

---

## 10. Armoires batteries et câblage DC

- Armoire **ventilée**, à proximité de l'UPS (câbles DC courts =
  moins de chute de tension).
- Câblage DC : **bipolaire + et −**, section selon courant max
  (ex. 40 kVA/384 V/36 kW → ~110 A → 35 mm² cuivre, à vérifier).
- **Disjoncteur batterie** magnétothermique DC calibré (1,25× le
  courant max de décharge).
- Ordre de raccordement : **d'abord l'armoire** (blocs en série),
  vérifier la tension totale **au multimètre**, **puis** fermer le
  disjoncteur vers l'UPS.
- Étiquetage : + / −, tension totale, date de mise en service,
  « DANGER DC ».

---

## 11. Installation électrique — disjoncteurs, câbles, terre

### 11.1 Les 4 protections d'un UPS triphasé
1. **Disjoncteur entrée redresseur** (amont réseau).
2. **Disjoncteur entrée bypass** (souvent commun ou séparé).
3. **Disjoncteur de sortie** (vers la charge / TGBT).
4. **Disjoncteur batterie** (DC).

### 11.2 Calibres indicatifs (400 V tri, cuivre, à valider NF C 15-100 / CEI 60364)

| UPS | Courant ≈ | Disjoncteur | Câble (ordre de grandeur) |
|---|---|---|---|
| 10 kVA | 16 A | 20–25 A | 4 mm² |
| 20 kVA | 32 A | 40 A | 6–10 mm² |
| 40 kVA | 64 A | 80 A | 16–25 mm² |
| 60 kVA | 96 A | 125 A | 35 mm² |
| 120 kVA | 192 A | 250 A | 70–95 mm² |

- Courant calculé : I = S / (√3 × 400). Ajoutez la **distance**
  (chute de tension < 3–5 %) et les **dératations** (température,
  groupement).
- ⚠️ Ces valeurs sont des **ordres de grandeur** : le calcul
  définitif suit la norme et l'étude du bureau (note de calcul).

### 11.3 Terre et régime de neutre
- **TN-S** recommandé (PE séparé du neutre jusqu'au TGBT).
- Résistance de terre : **< 5 Ω** (mesurée, pas supposée).
- Liaisons équipotentielles : armoire UPS, armoire batteries,
  chemins de câbles, masses métalliques.
- Parafoudre **Type 1+2** en tête d'installation (orages !).

### 11.4 Sélectivité
- Les protections amont/aval doivent être **sélectives** : un
  défaut sur un départ ne doit pas faire tomber l'UPS.
- Courbes et calibres : étude de sélectivité pour 60 kVA et +.

---

## 12. Le local technique — climatisation et accès

- **Température** : 20–25 °C idéal (batteries !). Jamais > 30 °C
  en continu.
- **Climatisation redondée** (N+1) pour 40 kVA et + : une clim en
  panne un week-end = batteries cuites.
- **Ventilation** : extraction d'air chaud de l'UPS (l'avant aspire,
  l'arrière souffle — respecter les distances du manuel, ~50 cm).
- **Hydrogène** : ventilation du local batteries (H₂ < 1 %,
  limite d'explosivité 4 %).
- **Accès** : porte ≥ 80 cm, espace de maintenance devant/derrière
  (manuel), éclairage, extincteur **CO₂** (jamais d'eau sur
  électrique).
- **Sol** : charge admissible (un 120 kVA + batteries = 1–2 tonnes),
  plancher technique ou dalle.

---

## 13. Mise en service (commissioning) — checklists

### 13.1 Checklist commune (toutes puissances)
```
□ Réception : chocs, accessoires, documentation, n° de série
□ Local : clim, ventilation, éclairage, extincteur, accès
□ Câblage : serrage au couple, repérage, terre < 5 Ω
□ Batteries : tension totale, polarité, disjoncteur ouvert
□ Protections : calibres conformes à la note de calcul
□ Parafoudre : présent et raccordé
□ Carte réseau (NMC) : installée, IP fixée
```

### 13.2 Séquence de démarrage
```
1. Fermer disjoncteur bypass → vérifier affichage
2. Fermer disjoncteur entrée → le redresseur démarre
3. Fermer disjoncteur batterie → vérifier floating
4. Démarrer l'onduleur (menu) → vérifier 400 V / 50 Hz en sortie
5. Fermer disjoncteur sortie → charge alimentée
6. TEST COUPURE : ouvrir l'entrée → 0 ms, autonomie OK
7. TEST BYPASS : forcer le bypass → retour onduleur sans coupure
8. Configurer : seuils, alarmes, arrêt programmé, supervision
```

### 13.3 Points spécifiques par puissance
- **10–20 kVA** : souvent en armoire existante → vérifier la
  ventilation et le disjoncteur amont (souvent sous-dimensionné).
- **40 kVA** : local dédié recommandé, note de calcul électrique
  exigée.
- **60–120 kVA** : étude complète (sélectivité, plancher, groupe),
  mise en service **avec le constructeur** si possible, banc de
  charge pour le test (§23).

---

## 14. Bypass — statique et maintenance

- **Bypass statique** : automatique (surcharge > 125 %, défaut
  onduleur). La charge passe sur le réseau **sans coupure**.
  Retour automatique après défaut (selon config).
- **Bypass de maintenance** : manuel, pour intervenir sur l'UPS.
  Procédure : forcer le bypass statique d'abord, **puis** basculer
  le bypass manuel, **puis** isoler l'UPS (entrée + batterie).
- ⚠️ En bypass de maintenance, **aucune protection** : planifiez
  l'intervention (pas pendant un orage !).
- Testez les deux bypass **à chaque visite annuelle**.

---

## 15. Groupes électrogènes et UPS — le couple critique

- **Dimensionnement** : groupe = **1,5 à 2×** la puissance de l'UPS
  (kVA). Un groupe trop juste : fréquence instable → l'UPS refuse
  le réseau et vide les batteries.
- **Problèmes classiques** :
  - Rampe de fréquence trop rapide → régler le slew rate du groupe.
  - THDi élevé du groupe → UPS IGBT moderne = OK ; vieux UPS =
    prévoir.
  - Démarrage à froid : 10–30 s → c'est l'autonomie batterie qui
    couvre (§9.4).
- **Test mensuel** : démarrage du groupe **en charge** (pas à vide),
  avec l'UPS qui bascule dessus. Un groupe qui ne démarre pas =
  discovered pendant la coupure = catastrophe.
- Contrat carburant : niveau mini, rotation du stock (gazole
  vieilli = panne).
---

## 16. Parallèle et redondance N+1

- **Parallèle** : 2 à 6 UPS identiques (même modèle, même firmware)
  sur le même bus de sortie.
- **N+1** : N UPS portent la charge, 1 en réserve. Ex. : besoin
  60 kVA → 2× 60 kVA en N+1 (ou 3× 40 kVA).
- Avantages : maintenance **sans bypass** (on isole un UPS, les
  autres portent la charge), tolérance de panne.
- Exigences : câbles de **même longueur/impédance**, mise en service
  par procédure constructeur, supervision commune.
- ⚠️ Le parallèle ne pardonne pas l'approximation : une erreur de
  câblage = court-circuit entre onduleurs.

---

## 17. Monitoring et supervision

### 17.1 Carte réseau (NMC)
- **Network Management Card** (Schneider AP9640/41 ou équivalent) :
  web + SNMP + Modbus TCP.
- À chaque installation : IP fixe, **mot de passe changé**, SNMP
  configuré, alertes email/SMS.

### 17.2 Protocoles
- **SNMP** : supervision (Zabbix, PRTG, Centreon) — OID standard
  UPS-MIB (RFC 1628).
- **Modbus RTU/TCP** : intégration GTB/BMS du bâtiment.
- **Contacts secs** : report d'alarmes simple (défaut, sur batterie,
  bypass) vers une alarme technique.

### 17.3 EcoStruxure IT (Schneider) / équivalents
- Supervision cloud : alertes, historique, recommandations.
- Pour un chef de service : **un tableau de bord unique** pour tout
  le parc = vous voyez les batteries qui faiblissent **avant**
  la panne.

### 17.4 Ce qu'il faut superviser (minimum)
- État (normal / sur batterie / bypass / défaut).
- Charge % par phase.
- Tension batterie + température.
- Alarmes actives.
- Autonomie restante estimée.

---

## 18. Alarmes et défauts — diagnostic

> Les libellés exacts varient selon les marques. Notez le **code
> complet** affiché + l'historique (event log).

### 18.1 Famille BATTERIES (la plus fréquente)
- **Batterie déconnectée** : disjoncteur ouvert, fusible, câble.
- **Tension batterie basse** : fin d'autonomie ou batterie HS.
- **Test batterie échoué** : impédance trop haute → prévoir
  remplacement (§20).
- **Remplacer batterie** : fin de vie estimée atteinte.
- **Surchauffe batterie** : local trop chaud → agir vite (risque
  d'emballement thermique).

### 18.2 Famille RÉSEAU / ENTRÉE
- **Réseau hors tolérance** : tension/fréquence hors plage → l'UPS
  passe sur batterie (vérifier le réseau, pas l'UPS !).
- **Séquence de phases incorrecte** : câblage d'entrée à corriger.
- **Neutre manquant** : **dangereux** → vérifier immédiatement.

### 18.3 Famille ONDULEUR / BYPASS
- **Surcharge** : > 100 % → délester ou augmenter la puissance.
- **Défaut onduleur** : bascule sur bypass statique → diagnostic
  (surchauffe ? court-circuit en sortie ?).
- **Bypass hors tolérance** : le bypass ne peut pas prendre le
  relais → situation critique, intervenir.
- **Court-circuit en sortie** : l'UPS protège, puis bypass → trouver
  le défaut aval.

### 18.4 Famille DIVERS
- **Défaut ventilateur** : remplacer (surchauffe sinon).
- **Surchauffe** : clim du local, dépoussiérage, ventilateurs.
- **Bus DC en surtension** : carte à diagnostiquer.
- **Défaut carte** : noter le code, appeler le support avec l'event log.

### 18.5 Méthode générale
```
1. Noter code + heure + circonstances (orage ? travaux ?)
2. Consulter l'event log (historique des 50–200 derniers événements)
3. Classer : batterie / réseau / onduleur / environnement
4. Traiter la cause, pas le symptôme
5. Tester (coupure simulée) après intervention
```

---

## 19. Maintenance préventive — le programme total

### 19.1 Visite mensuelle (30 min)
```
□ Relevé : état, charge %, alarmes actives
□ Event log : nouveaux événements ?
□ Température local + armoire batteries
□ Ventilation : grilles propres, flux d'air
□ Voyants : tout au vert ?
□ Supervision : alertes reçues ?
```

### 19.2 Visite trimestrielle (2 h)
```
□ Tout le mensuel +
□ Tension de chaque bloc batterie (écart > 0,3 V = suspect)
□ Serrage des connexions batteries (au couple, machine consignée)
□ Dépoussiérage (aspirateur, jamais d'air comprimé sur les cartes)
□ Thermographie IR (points chauds) — §22
□ Vérifier les seuils d'alarmes et la supervision
```

### 19.3 Visite semestrielle (demi-journée)
```
□ Tout le trimestriel +
□ Test d'impédance batteries — §20
□ Test des ventilateurs (écoute, débit)
□ Test bypass statique + bypass de maintenance
□ Vérifier les condensateurs (visuel : gonflés ? coulures ?)
□ Nettoyage filtres à air
```

### 19.4 Visite annuelle (journée complète)
```
□ Tout le semestriel +
□ Test de décharge batterie (autonomie réelle) ou banc de charge — §23
□ Vérification des protections (déclenchement, sélectivité)
□ Mesure de terre
□ Mise à jour firmware (avec procédure de secours)
□ Rapport annuel complet + plan de remplacement (batteries ? ventilos ? condos ?)
```

---

## 20. Batteries — test et remplacement

### 20.1 Test d'impédance (la méthode pro)
- Appareil : testeur d'impédance/conductance (Hioki, Megger BITE…).
- **Baseline à la mise en service** : mesure de chaque bloc neuf.
- Suivi : **+20–30 %** par rapport à la baseline = bloc à
  remplacer. **+50 %** = remplacement urgent.
- Fréquence : semestrielle (trimestrielle après 3 ans).

### 20.2 Test de tension
- Floating : chaque bloc à 13,5–13,6 V (à 20 °C).
- Écart > 0,3 V entre blocs = bloc faible → test d'impédance ciblé.

### 20.3 Test de décharge (autonomie réelle)
- Couper l'entrée, chronométrer jusqu'à l'alarme « batterie basse ».
- ⚠️ Ne jamais décharger à fond (arrêter à 1,75 V/élément).
- Si l'autonomie < 80 % de la nominale → remplacement.

### 20.4 Remplacement — procédure
1. Commander des blocs **identiques** (même marque, même lot si
   possible).
2. Consignation : disjoncteur batterie **ouvert**, vérifier
   l'absence de tension.
3. Remplacer **tout le string** (jamais un seul bloc neuf parmi
   des vieux).
4. EPI : gants, lunettes, outils isolés, pas de bijoux.
5. Vérifier la tension totale avant de refermer le disjoncteur.
6. Nouvelle **baseline d'impédance**.
7. Anciens blocs : **recyclage** (filière plomb, §32).

---

## 21. Condensateurs et ventilateurs — les oubliés

### 21.1 Condensateurs du bus DC
- Vie : **5–7 ans** (moins en chaud). Signes : gonflement,
  coulures, évent ouvert.
- Un condensateur qui lâche = **explosion possible** + carte HS.
- **Remplacement préventif à 5 ans** sur les sites critiques
  (kit constructeur, machine consignée, attendre la décharge du
  bus — 5–10 min — et **vérifier au multimètre**).

### 21.2 Ventilateurs
- Vie : **3–5 ans** en continu. Signes : bruit, débit faible,
  alarme « fan fault ».
- Remplacement préventif à 4 ans (pièce peu chère, panne coûteuse).
- ⚠️ Sens de montage : noter avant démontage (U-autocollant).

---

## 22. Thermographie infrarouge

- Caméra IR (ou smartphone + module) : **l'outil du chef de service**.
- Points à scanner : connexions batteries, disjoncteurs, cosses,
  borniers, transformateurs.
- **Écart > 10 °C** entre phases ou par rapport à l'ambiance =
  anomalie (serrage, surcharge, contact).
- Fréquence : trimestrielle. Photos **avant/après** dans le rapport.
---

## 23. Test sur banc de charge (load bank)

- **Pourquoi** : tester l'UPS à 100 % sans risquer la charge réelle.
- **Quand** : mise en service 60 kVA+, après grosse intervention,
  annuellement sur sites critiques.
- **Procédure** :
  1. Basculer la charge réelle sur bypass/groupe (ou tester à vide
     de charge avec le banc seul).
  2. Brancher le banc sur la sortie UPS.
  3. Monter par paliers (25/50/75/100 %), 15 min par palier.
  4. Surveiller : températures, tensions, alarmes, autonomie batterie.
  5. Redescendre, rebasculer la charge, rapport.
- Location de bancs : prévoir dans le budget maintenance.

---

## 24. Sécurité — consignation, DC, batteries, arc flash

### 24.1 Consignation (la règle d'or)
```
1. Séparer (ouvrir TOUS les disjoncteurs : entrée, bypass, sortie, batterie)
2. Condamner (cadenas + étiquette nominative)
3. Vérifier l'absence de tension (VAT, AC **et** DC)
4. Attendre la décharge du bus DC (5–10 min), re-vérifier
5. Travailler, puis déconsigner en sens inverse
```
- **Le bus DC reste chargé après coupure** : c'est le piège n°1.

### 24.2 Batteries — dangers spécifiques
- **Court-circuit** : un string 384 V peut débiter des **kA** →
  outils **isolés**, une seule main si possible, pas de bijoux.
- **Hydrogène** : ventilation, pas de flamme/étincelle à proximité.
- **Acide** : kit de neutralisation + douche oculaire à proximité.
- **Poids** : un bloc 12 V/100 Ah = ~30 kg → manutention à 2.

### 24.3 Arc flash
- Tableau TGBT / armoire UPS : risque d'arc électrique.
- EPI : gants isolants classe adaptée, écran facial, vêtements
  ignifugés pour les interventions sous tension (à éviter au
  maximum).
- Distances de sécurité affichées sur les armoires.

### 24.4 Habilitation électrique
- En Afrique francophone : référez-vous à la norme locale
  (inspirée NF C 18-510 : B1V, B2V, BR, BC…).
- **Seul le personnel habilité** intervient dans les armoires.

---

## 25. Dépannage express

| Symptôme | Vérifier d'abord |
|---|---|
| L'UPS ne démarre pas | Disjoncteurs, séquence de phases, neutre |
| Passe sur batterie sans coupure visible | Réseau hors tolérance (mesurer !) |
| Autonomie très faible | Batteries (impédance, âge, température) |
| Bascule sur bypass intempestive | Surcharge, surchauffe, défaut onduleur |
| Alarme ventilateur | Remplacer le ventilateur |
| Ne communique plus (NMC) | IP, câble, mot de passe, reboot carte |
| Bruit anormal | Ventilateur, transformateur (charge ?) |
| Odeur d'œuf pourri | **Batterie en emballement → ventiler, consigner, remplacer** |
| Disjoncte à l'enclenchement | Court-circuit aval, sélectivité |
| Le groupe ne prend pas la charge | Slew rate, dimensionnement (§15) |

---

## 26. Cas pratiques commentés (10 cas terrain)

**Cas 1 — Autonomie de 4 min au lieu de 15 (40 kVA)**
Batteries de 4 ans dans un local à 33 °C, jamais testées. Impédance
+60 %. → Remplacement complet + clim du local + baseline.

**Cas 2 — L'UPS passe sur batterie tous les soirs à 19h**
Creux de tension du réseau (pointe du soir). → Mesure avec
enregistreur, signalement au distributeur, élargir les seuils
d'entrée si possible.

**Cas 3 — Casse onduleur après orage**
Pas de parafoudre en tête. → Remplacement carte + **parafoudre
Type 1+2** + vérification de la terre.

**Cas 4 — Le groupe démarre mais l'UPS reste sur batteries**
Rampe de fréquence trop rapide. → Réglage du régulateur du groupe
(slew rate), test mensuel en charge.

**Cas 5 — Emballement thermique d'un bloc**
Bloc gonflé, odeur, température 55 °C. → **Ventiler, consigner,
remplacer le string**, vérifier la compensation de température du
chargeur.

**Cas 6 — Bypass de maintenance oublié enclenché**
La charge n'est plus protégée depuis 3 mois. → Procédure écrite +
checklist de déconsignation + alarme « en bypass » supervisée.

**Cas 7 — Surcharge à 110 % le lundi matin**
Tout le monde allume en même temps. → Délestage (clim non
prioritaire sur un autre départ), ou montée en puissance.

**Cas 8 — NMC inaccessible après changement d'IP**
Mot de passe perdu, IP en DHCP. → Reset carte (procédure
constructeur), IP fixe, mot de passe en coffre.

**Cas 9 — Condensateur explosé (60 kVA, 6 ans)**
Pas de remplacement préventif. → Kit condensateurs + 2 jours
d'arrêt. Leçon : planifier à 5 ans.

**Cas 10 — Faux « défaut batterie »**
Disjoncteur batterie **vibré ouvert** (mauvais serrage). →
Resserrage au couple + contrôle thermographique trimestriel.

---

## 27. Contrats de maintenance — côté chef de service

### 27.1 Les niveaux de contrat
- **Bronze** : 1 visite annuelle + astreinte.
- **Silver** : 2 visites + supervision + pièces (hors batteries).
- **Gold** : 4 visites + supervision 24/7 + pièces + batteries +
  SLA 4 h.
- Adaptez au **risque client** : un hôpital = Gold, un bureau =
  Silver.

### 27.2 Chiffrage
```
Coût annuel = visites × (MO + déplacement)
            + pièces prévisionnelles (batteries/5 ans, ventilos/4 ans, condos/5 ans)
            + supervision
            + marge (20–30 %) + risque
```
- **Les batteries sont le poste n°1** : provisionnez-les dès la
  signature (sinon à 4 ans vous n'avez plus de marge).

### 27.3 Le SLA
- GTR (temps de rétablissement) : 4 h (Gold), 8 h (Silver),
  24 h (Bronze).
- Astreinte : planning, téléphone dédié, **pièces critiques en
  stock** (sinon le SLA est un mensonge).
- Pénalités : ne les acceptez que si vous maîtrisez le stock.

---

## 28. Gestion d'équipe et planning

- **1 chef + 2 techniciens** minimum pour un parc de 20–30 UPS :
  un senior (diagnostic, HT), un junior (visites, relevés).
- Planning : visites mensuelles/trimestrielles planifiées à
  l'année, astreinte tournante.
- **Fiche d'intervention** obligatoire (modèle §30) : sans écrit,
  pas de traçabilité, pas de garantie.
- Réunion hebdo 30 min : revue des alarmes, planning, stock.

---

## 29. Stock pièces — le minimum vital

| Pièce | Stock conseillé |
|---|---|
| Blocs batterie 12 V (le modèle suivi) | 1 string d'avance |
| Ventilateurs (par modèle) | 2 par modèle |
| Kit condensateurs bus DC | 1 par modèle critique |
| Disjoncteurs (entrée/sortie/batterie) | 1 de chaque calibre |
| Carte NMC de secours | 1 |
| Fusibles DC | jeu complet |
| Câbles, cosses, gaine thermo | assortiment |

- Délai Afrique : **4–8 semaines** → le stock n'est pas une option.

---

## 30. KPI et reporting — piloter le service

- **Disponibilité** : objectif 99,9 % (8 h d'arrêt/an max).
- **MTTR** : temps moyen de réparation (objectif < 4 h Gold).
- **Batteries** : % du parc testé, âge moyen, remplacements prévus.
- **Alarmes** : nombre/semaine, temps de traitement.
- **Rapport mensuel direction** : 1 page — disponibilité, incidents,
  actions, budget, risques (batteries à remplacer, etc.).
- **Rapport annuel par site** : état complet + plan d'investissement
  (batteries, clim, groupe).

### Modèle de fiche d'intervention UPS
```
Date : ___  Site : ___  UPS : ___ (marque/modèle/n° série)
Puissance : ___ kVA  Charge : ___ %  Batteries : ___ V / ___ °C
Alarmes : ___
Actions : ___
Mesures (tensions, impédances) : ___
Pièces : ___
Prochaine échéance : ___
Signatures : technicien ___ / client ___
```

---

## 31. Formation de l'équipe (plan 4 semaines)

- **S1** : sécurité électrique + consignation (§24), topologies (§2–3).
- **S2** : batteries — mesures, impédance, remplacement (§8–10, §20).
- **S3** : installation — disjoncteurs, câbles, mise en service (§11–13).
- **S4** : supervision, alarmes, dépannage (§17–18, §25) + tournées
  accompagnées.

---

## 32. Fin de vie et recyclage

- **Batteries plomb** : filière agréée (le plomb se recycle à 95 %+,
  et il a une valeur — négociez la reprise).
- **UPS** : DEEE — cartes (métaux), condensateurs, transformateur
  (cuivre).
- **Avant mise au rebut** : effacer les configs (mots de passe
  réseau !), récupérer la carte NMC.
- Traçabilité : bordereaux de suivi des déchets (exigence clients
  institutionnels).

---

## 33. Glossaire

| Terme | Signification |
|---|---|
| UPS / onduleur | Alimentation sans interruption |
| VFI / VI / VFD | Topologies (IEC 62040-3) |
| Double conversion | AC→DC→AC en permanence |
| PF | Facteur de puissance (kW/kVA) |
| THDi | Distorsion harmonique du courant |
| Bus DC | Liaison continue interne (~384–480 V) |
| Floating | Charge d'entretien des batteries |
| Boost | Charge accélérée/égalisation |
| Bypass statique | Bascule électronique auto sur réseau |
| Bypass maintenance | Bascule manuelle pour intervention |
| N+1 | Redondance : 1 UPS de plus que nécessaire |
| NMC | Carte réseau de supervision |
| SNMP / Modbus | Protocoles de supervision |
| Autonomie | Temps sur batteries |
| Load bank | Banc de charge (test) |
| Arrhenius | Loi : +10 °C = vie batterie / 2 |
| GTR | Garantie de temps de rétablissement |
| MTTR | Temps moyen de réparation |

---

## 34. Pense-bête de poche

```
VFI = 0 ms | kW = kVA × PF | Charge cible ≤ 80 %
Floating 13,5–13,6 V/bloc | Écart > 0,3 V = suspect
Impédance +30 % = remplacer | Batteries : local 20–25 °C
Bus DC chargé après coupure : attendre + vérifier !
Groupe = 1,5–2× l'UPS | Test groupe mensuel EN CHARGE
Bypass = plus de protection | Thermographie trimestrielle
Condos 5 ans | Ventilos 4 ans | Firmware annuel
```

---

## 35. Fiches par puissance — l'essentiel en une page

### 35.1 10 kVA (~9 kW)
- Usage : petite salle serveurs, agence.
- Protection : 20–25 A, câble ~4 mm², terre < 5 Ω.
- Batteries : 32 blocs 12 V (384 V), autonomie 10–15 min standard.
- Vigilance : disjoncteur amont souvent sous-dimensionné, local
  non climatisé.

### 35.2 20 kVA (~18 kW)
- Usage : salle serveurs PME.
- Protection : 40 A, câble 6–10 mm².
- Batteries : 32–40 blocs, 1–2 strings.
- Vigilance : prévoir la croissance (souvent saturé en 2 ans).

### 35.3 40 kVA (~36 kW)
- Usage : data center PME, industrie.
- Protection : 80 A, câble 16–25 mm², note de calcul exigée.
- Batteries : 40 blocs (480 V) ou 32 (384 V), 1–2 strings.
- Vigilance : local dédié + clim, parafoudre, supervision NMC.

### 35.4 60 kVA (~54 kW)
- Usage : data center moyen, hôpital.
- Protection : 125 A, câble 35 mm², étude de sélectivité.
- Batteries : armoire(s) dédiée(s), 2 strings conseillés.
- Vigilance : mise en service avec banc de charge, groupe
  90–120 kVA, clim N+1.

### 35.5 120 kVA (~108 kW)
- Usage : gros site, N+1 de 60 kVA.
- Protection : 250 A, câble 70–95 mm², TGBT dédié.
- Batteries : plusieurs armoires, 2+ strings, local batteries
  ventilé.
- Vigilance : plancher (1–2 t), étude complète, contrat Gold,
  équipe formée, stock pièces.

---

## 36. Questions pièges (validez vos connaissances)

1. Pourquoi 0 ms en double conversion ? → La charge est toujours
   sur l'onduleur, pas de bascule.
2. Un 40 kVA PF 0,9 alimente combien de kW ? → 36 kW.
3. Charge à 95 % en permanence : problème ? → Oui, pas de marge
   (pics, croissance) → viser 80 %.
4. Local batteries à 35 °C : conséquence ? → Vie / 2 (Arrhenius).
5. Pourquoi 2 strings identiques ? → Sinon déséquilibre de
   décharge, le string faible limite l'ensemble.
6. On remplace un seul bloc gonflé ? → Non, tout le string.
7. Le bus DC après consignation ? → Reste chargé : attendre +
   vérifier.
8. Groupe de même puissance que l'UPS : OK ? → Non, 1,5–2×.
9. En bypass de maintenance, la charge est protégée ? → Non.
10. Parafoudre : où ? → Tête d'installation, Type 1+2.

---

*Fin du guide. La maintenance des UPS tient en une phrase :
**batteries surveillées, local climatisé, protections calibrées,
tests réguliers — et tout noté.** Avec ça, votre parc ne vous
fera jamais de surprise un vendredi à 18h.*
---

## 37. Schémas unifilaires types

### 37.1 Schéma 10–20 kVA (simple)
```
Réseau ──[DJ 25/40A]──┬──► ENTRÉE UPS ──► SORTIE UPS ──[DJ]──► Charge
                      └──► BYPASS UPS ──► (interne)
Terre ──► PE ──► châssis UPS + armoire batteries
Batteries ──[DJ DC]──► UPS
```

### 37.2 Schéma 40–120 kVA (avec TGBT ondulé)
```
Réseau ──[DJ général]──[Parafoudre T1+T2]──┬──► ENTRÉE UPS ──►┐
                                           └──► BYPASS UPS ──►┤
                                                              ▼
                                              TGBT ONDULÉ ──[DJ départs]──► Charges
                                              (sectionneur général + report alarme)
Batteries ──[DJ DC par string]──► UPS
Groupe ──[Inverseur de source]──► (amont UPS)
```

### 37.3 Schéma N+1 (2× 60 kVA)
```
Réseau ──┬──[DJ]──► UPS1 ──┐
         └──[DJ]──► UPS2 ──┴──► Bus commun ──[DJ]──► Charge
Bypass externe de maintenance (contourne les 2 UPS)
```
- Câbles de sortie **strictement identiques** (longueur, section).
- Un seul bypass de maintenance externe pour l'ensemble.

---

## 38. Note de calcul électrique — modèle

> Document exigé pour 40 kVA et +. Modèle de trame :

```
1. OBJET : alimentation ondulée site ___, UPS ___ kVA
2. HYPOTHÈSES : 400 V tri, cos φ ___, température ___°C,
   mode de pose ___, longueur ___ m
3. BILAN DE PUISSANCE
   Charge 1 : ___ kW / cos φ ___ → ___ kVA
   ...
   TOTAL : ___ kW / ___ kVA → UPS retenu : ___ kVA (charge ___%)
4. COURANTS : I = S/(√3×U) = ___ A
5. CÂBLES : section ___ mm², chute de tension ___ % (< 5 %)
6. PROTECTIONS : DJ ___ A courbe ___, pouvoir de coupure ___ kA
7. SÉLECTIVITÉ : étude amont/aval (tableau)
8. TERRE : < 5 Ω, section PE ___ mm²
9. BATTERIES : ___ blocs ___ V ___ Ah, ___ strings,
   autonomie ___ min à ___ kW
10. GROUPE : ___ kVA (1,5–2× UPS)
```

---

## 39. Consignation détaillée — pas à pas

```
AVANT :
□ Prévenir le client / l'astreinte (la charge passe en bypass)
□ EPI : gants isolants, lunettes, écran facial si TGBT
□ Outillage isolé 1000 V, VAT vérifié

CONSIGNATION :
1. Forcer le bypass statique (menu UPS)
2. Basculer le bypass de maintenance → charge sur réseau direct
3. Ouvrir : DJ sortie → DJ entrée → DJ bypass → DJ batterie(s)
4. Cadenas + étiquette sur chaque organe
5. ATTENDRE 10 min (décharge bus DC)
6. VAT : vérifier absence de tension AC (entrée/sortie)
   ET DC (bus, batteries)
7. Si intervention batteries : déconnecter un pôle du string

DÉCONSIGNATION (ordre inverse) :
1. Reconnecter batteries, vérifier tension totale
2. Retirer cadenas/étiquettes
3. Fermer : DJ batterie → DJ bypass → DJ entrée
4. Démarrer l'onduleur, vérifier 400 V/50 Hz
5. Fermer DJ sortie → repasser en mode normal
6. Quitter le bypass de maintenance
7. TEST : coupure simulée 30 s → OK
```

---

## 40. Mise en parallèle — procédure détaillée

1. Vérifier : **mêmes modèles, mêmes firmwares**, cartes parallèles
   installées.
2. Câbles de sortie **identiques** (même longueur au cm près).
3. Adressage : UPS1 = maître, UPS2 = esclave (menu).
4. Démarrage séquentiel selon la procédure constructeur.
5. Test : couper UPS1 → UPS2 prend 100 % sans coupure.
6. Supervision : les 2 UPS dans le même tableau de bord.
7. ⚠️ Ne jamais paralléliser des UPS de marques/modèles
   différents.

---

## 41. Supervision SNMP — mise en pratique

- **UPS-MIB (RFC 1628)** : OID standard — état, charge, batterie,
  autonomie, température.
- **Zabbix/PRTG/Centreon** : template « UPS » + seuils :
  - Charge > 80 % → warning ; > 90 % → critique.
  - Sur batterie → alerte immédiate (SMS).
  - Batterie basse → critique.
  - Température > 30 °C → warning.
- **Trap SNMP** : l'UPS pousse les alarmes (plus rapide que le poll).
- Testez les alertes **après chaque installation** (coupure simulée
  → le SMS arrive-t-il ?).

---

## 42. Arrêt programmé des serveurs (graceful shutdown)

- Si l'autonomie s'épuise (pas de groupe, coupure longue), les
  serveurs doivent s'arrêter **proprement** avant la fin.
- **PowerChute** (Schneider/APC) ou équivalent : agent sur chaque
  serveur, scénario « à X min d'autonomie restante → shutdown ».
- Ordre d'arrêt : applicatifs → bases de données → hyperviseurs.
- **Tester 2× par an** (un scénario jamais testé ne marche pas).
- Documenter : qui s'arrête, dans quel ordre, qui redémarre
  (séquence inverse au retour).

---

## 43. Climatisation — dimensionnement

```
Puissance frigo ≈ pertes UPS + pertes charges + apports
Pertes UPS = P_charge × (1 - η)
Ex. : 30 kW à 96 % → 1,2 kW de pertes (chaleur)
      + serveurs 30 kW → ~31 kW à évacuer
```
- **Redondance N+1** pour 40 kVA et +.
- Consigne : **22 °C** (±2). Alarme à 28 °C, critique à 32 °C.
- Maintenance clim : filtres mensuels, contrôle annuel (c'est la
  clim qui protège vos batteries !).
- En cas de panne clim : procédure d'urgence (ouvrir, ventiler,
  délester) — à écrire **avant**.

---

## 44. Groupes électrogènes — dimensionnement détaillé

```
P_groupe ≥ 1,5 × S_UPS (minimum)
P_groupe ≥ 2 × S_UPS (recommandé, charges non linéaires)
Ex. : UPS 60 kVA → groupe 90–120 kVA
```
- **ATS** (inverseur automatique) : bascule réseau/groupe < 10 s.
- Réglages : slew rate (rampe de fréquence) compatible UPS,
  tension et fréquence stables avant couplage.
- **Test mensuel en charge** (30 min à > 30 % de charge) :
  un groupe qui tourne à vide s'encrasse.
- Carburant : stock pour **48 h** minimum, rotation tous les 6 mois,
  contrat de livraison d'urgence.
- Échappement, bruit (capot insonorisé), voisinage : à traiter à
  l'installation.

---

## 45. Onduleurs modulaires (hot-swap)

- Principe : modules de puissance (ex. 25–50 kW) enfichables à chaud
  dans un châssis.
- Avantages : redondance interne N+1, évolutivité (on ajoute des
  modules), MTTR faible (échange en 10 min).
- Marques : Socomec MODULYS, Huawei UPS5000, Schneider Galaxy VS
  (partiellement).
- Maintenance : **mêmes règles** (batteries, condos, ventilos) +
  vérification des connecteurs de modules.

---

## 46. Qualité du réseau — savoir mesurer

| Perturbation | Effet sur l'UPS | Action |
|---|---|---|
| Creux de tension | Passage sur batterie | Enregistreur, seuils |
| Surtension | Bypass / protection | Parafoudre, signalement |
| Harmoniques (THDv) | Échauffement | Filtres, UPS IGBT |
| Variation de fréquence | Refus du réseau (batterie) | Groupe à régler |
| Coupures brèves | 0 ms (VFI) → invisible | Rien (c'est le job) |
| Déséquilibre phases | Alarme | Rééquilibrer les départs |

- **Enregistreur de qualité réseau** (Fluke 1750 ou équivalent) :
  l'outil qui prouve que « c'est le réseau, pas l'UPS ».

---

## 47. Audit énergétique d'un site — trame

```
□ Relevé des compteurs (12 mois) : kWh, pics
□ Mesure en charge (pince wattmètre) : par départ
□ Facteur de puissance global (pénalités ? → batterie de condensateurs)
□ Qualité réseau (enregistreur 1 semaine)
□ Climatisation : puissance, âge, redondance
□ Groupe : puissance, tests, carburant
□ UPS existants : âge, charge %, batteries, alarmes
□ Recommandations chiffrées : ROI de chaque action
```
- L'audit est une **prestation vendable** qui amène les contrats.

---

## 48. Gestion des incidents majeurs — plan de crise

```
1. DÉTECTION : alarme supervision → astreinte (15 min max)
2. QUALIFICATION : UPS ? réseau ? groupe ? (arbre de décision §25)
3. COMMUNICATION : client prévenu (délai, impact estimé)
4. ACTION : procédure (bypass, groupe, délestage…)
5. SUIVI : point toutes les 30 min jusqu'à résolution
6. RETEX : rapport 48 h — cause racine, actions correctives
```
- **Exercice annuel** : coupure simulée avec le client (comme un
  exercice incendie).

---

## 49. Dossier d'exploitation — le classeur du site

Chaque site doit avoir (papier + numérique) :
- Schéma unifilaire **à jour**.
- Note de calcul, rapports de mise en service.
- Fiches techniques (UPS, batteries, groupe, clim).
- Procédures : consignation, bypass, arrêt d'urgence, crise.
- Historique : rapports de visite, mesures, remplacements.
- Contacts : astreinte, constructeur, fournisseur pièces.
- **Sans dossier d'exploitation, pas de maintenance sérieuse.**

---

## 50. Lexique étendu

| Terme | Signification |
|---|---|
| ATS | Inverseur automatique de source |
| TGBT | Tableau général basse tension |
| THDv / THDi | Distorsion harmonique tension/courant |
| Slew rate | Vitesse de variation de la fréquence |
| Floating / Boost | Régimes de charge batterie |
| Baseline | Mesure de référence (impédance) |
| NMC | Carte réseau UPS |
| Trap SNMP | Alarme poussée par l'équipement |
| DOD | Profondeur de décharge |
| SOC | État de charge |
| EODV | Tension de fin de décharge |
| Load bank | Banc de charge |
| RETEX | Retour d'expérience |
---

## 51. Fiche réflexe astreinte (à garder sur le téléphone)

```
ALARME UPS — CONDUITE À TENIR
1. Lire le code EXACT + l'heure
2. État ? ☐ Normal ☐ Sur batterie ☐ Bypass ☐ Défaut
3. Charge ___ % | Batterie ___ V | Autonomie ___ min
4. Cause probable (§18) : _______________
5. Si sur batterie : autonomie restante ? groupe démarré ?
6. Si défaut : bypass possible ? charge protégée ?
7. Appeler : client ___ / senior ___ / constructeur ___
8. Noter TOUT (heure, actions) → fiche d'intervention
NE JAMAIS : forcer un redémarrage sans diagnostic,
            laisser en bypass sans prévenir,
            intervenir seul sur TGBT/HTA.
```

---

## 52. Erreurs classiques des débutants (et comment les éviter)

1. **Oublier le disjoncteur batterie ouvert** → l'UPS ne tient pas
   la coupure. → Checklist §13.
2. **Serrer les cosses batteries à la main** → échauffement, faux
   contact. → Clé dynamométrique + thermographie.
3. **Mélanger blocs neufs et vieux** → le string est limité par le
   plus faible. → Remplacement complet du string.
4. **Négliger la clim** → batteries mortes en 2 ans. → 20–25 °C,
   clim N+1.
5. **Firmware jamais mis à jour** → bugs, failles. → Annuel.
6. **Pas de test de coupure** → on découvre la panne le jour J. →
   Test à chaque visite annuelle.
7. **Mot de passe NMC perdu** → supervision morte. → Coffre.
8. **Bypass de maintenance oublié** → plus de protection. → Alarme
   supervisée + checklist.
9. **Intervenir sans consignation** → **danger de mort**. → §24,
   toujours.
10. **Ne rien noter** → pas de traçabilité, pas d'amélioration. →
    Fiche systématique.

---

## 53. Budget maintenance type (ordre de grandeur annuel)

| Poste | % du prix de l'UPS |
|---|---|
| Visites préventives (MO) | 3–5 % |
| Pièces d'usure (ventilos, filtres) | 1–2 % |
| Provision batteries (amorti /5 ans) | 8–12 % |
| Supervision / astreinte | 2–3 % |
| Banc de charge (location annuelle) | 1 % |
| **Total** | **15–23 %** |

- C'est ce chiffrage qui justifie vos contrats : montrez-le au
  client, il comprend ce qu'il paie.

---

## 54. Normes et références (à connaître)

- **CEI 62040-1/2/3** : sécurité / CEM / performances des UPS.
- **CEI 60364 / NF C 15-100** : installations électriques BT.
- **NF C 18-510** : habilitation électrique (référence).
- **EN 50272-2** : sécurité des batteries stationnaires.
- **Eurobat** : classification des durées de vie batteries.
- Conservez les **déclarations de conformité** de chaque UPS.

---

## 55. Ce qui change (2024-2026)

- **Lithium** : le LFP devient compétitif en TCO → de plus en plus
  de projets neufs en Li-ion (moins de maintenance batteries).
- **Rendement** : les 97 %+ en double conversion arrivent (SiC).
- **Supervision** : le cloud devient standard (EcoStruxure et
  équivalents) → le chef de service pilote son parc au téléphone.
- **Cybersécurité** : les NMC exposées sont attaquées → VLAN
  dédié, mots de passe forts, mises à jour.
- **ECO mode intelligent** : bascule auto selon la qualité réseau.

---

## 56. Le mot du chef de service (à afficher au bureau)

```
La continuité électrique ne s'improvise pas.
Elle se DIMENSIONNE, s'INSTALLE, se TESTE et se MAINTIENT.
- Une batterie non testée est une batterie morte.
- Un groupe non testé en charge ne démarrera pas.
- Un bypass oublié n'est plus une protection.
- Tout ce qui n'est pas noté n'a pas été fait.
```

---

*Fin du guide — 1600+ lignes. Vos UPS entre de bonnes mains,
Zelef : **batteries surveillées, local climatisé, protections
calibrées, tests réguliers, équipe formée, tout noté.** C'est ça,
être chef de service systèmes & énergies.*
---

## 57. Visite annuelle type — déroulé chronométré (40 kVA)

```
08:00 Arrivée, EPI, prévenir le client et l'astreinte
08:15 Relevés : état, charge %, alarmes, event log (export)
08:30 Températures : local, armoire batteries (thermomètre IR)
08:45 Thermographie : cosses batteries, DJ, borniers (photos)
09:15 Tensions de chaque bloc batterie (noter les écarts)
09:45 Test d'impédance (si semestriel échu)
10:15 Dépoussiérage : grilles, ventilateurs, filtres
10:45 Vérification visuelle condensateurs (gonflés ? coulures ?)
11:00 Test bypass statique (forcer/relâcher, sans coupure)
11:15 Test bypass de maintenance (procédure §14)
11:30 Remise en normal, vérification 400 V/50 Hz
11:45 TEST COUPURE : ouvrir DJ entrée 2 min → autonomie, alarmes
12:00 Refermer, vérifier le retour, acquitter les alarmes
12:15 Mise à jour firmware (si nouvelle version validée)
12:45 Supervision : tester une alerte (SMS reçu ?)
13:00 Rapport : mesures, photos, préconisations, prochaine échéance
13:30 Signature client, départ
```

---

## 58. Remplacement des batteries — pas à pas complet

```
PRÉPARATION (J-7) :
□ Blocs identiques reçus (même marque, même lot si possible)
□ Vérifier les tensions à la réception (écart < 0,2 V)
□ Prévenir le client (intervention avec bypass)
□ EPI : gants, lunettes, outils isolés

JOUR J :
1. Forcer bypass statique → bypass de maintenance
2. Ouvrir DJ batterie(s), consigner (cadenas + étiquette)
3. VAT : 0 V aux bornes du string
4. Photographier le câblage AVANT démontage
5. Déconnecter : d'abord le point milieu (si accessible),
   puis les extrémités (+ et −)
6. Sortir les blocs (manutention à 2, ~30 kg/bloc)
7. Nettoyer l'armoire, vérifier les bacs (acide ?)
8. Poser les neufs, serrer au couple (manuel batterie)
9. Reconnecter selon la photo, vérifier la polarité 2×
10. Mesurer la tension totale : ___ V (attendu : ___ V)
11. Refermer DJ batterie → vérifier le floating (13,5–13,6 V/bloc)
12. Baseline d'impédance de chaque bloc (noter !)
13. Quitter le bypass, remise en normal
14. Test coupure 1 min → OK
15. Anciens blocs : palette, film, bordereau recyclage

RAPPORT : date, marque/lot, tensions, impédances, prochaine échéance
```

---

## 59. Remplacement des condensateurs bus DC — pas à pas

1. Consignation complète (§39) — **bus DC = danger**.
2. Attendre 10 min, **vérifier 0 V au multimètre** (2 mesures).
3. Photographier, noter les références (µF, V, température).
4. Dessouder/déclipser (selon montage), nettoyer les traces.
5. Monter les neufs **même valeur, même série** (kit constructeur
   de préférence).
6. Vérifier les soudures/connexions, remonter.
7. Déconsigner, démarrer, vérifier l'absence d'alarme bus DC.
8. Thermographie après 1 h de fonctionnement.
9. Noter la date → prochaine échéance dans 5 ans.

---

## 60. Mise en service d'un 120 kVA — récit type

- **J-30** : note de calcul, commande (UPS + 2 armoires batteries +
  TGBT ondulé), planning avec le client.
- **J-7** : local prêt ? (clim, plancher, éclairage, extincteur).
- **Jour J (équipe de 3)** :
  - Matin : mise en place (transpalette), câblage puissance
    (70–95 mm²), terre, batteries (2 strings de 40 blocs).
  - Après-midi : serrages au couple, vérifications, NMC (IP fixe).
  - Séquence de démarrage (§13.2), 400 V/50 Hz vérifiés.
- **J+1** : banc de charge 100 kW (paliers 25/50/75/100 %),
  test coupure, test bypass, test groupe.
- **J+2** : bascule de la charge réelle (fenêtre de maintenance
  nuit/week-end), supervision, formation exploitants.
- **J+7** : visite de contrôle (serrages, températures, alarmes).
- Dossier d'exploitation remis au client (§49).

---

## 61. Les 15 mesures à savoir faire

1. Tension AC triphasée (400 V ±10 %).
2. Tension DC bus (selon modèle, ~384–480 V).
3. Tension de chaque bloc batterie (13,5–13,6 V en floating).
4. Tension totale du string (somme, au volt près).
5. Courant de charge (pince AC, par phase).
6. Courant de décharge batterie (pince DC).
7. Résistance de terre (< 5 Ω).
8. Continuité du PE.
9. Impédance batterie (testeur dédié).
10. Température (thermomètre IR).
11. Fréquence réseau (50 Hz ±1 %).
12. THDv (analyseur ou pince качественная).
13. Isolement (mégohmmètre, machine consignée).
14. Chute de tension en charge (câbles longs).
15. Autonomie réelle (chronomètre + coupure simulée).

---

## 62. Dépannage avancé — 5 cas complexes

**Cas A — L'UPS refuse le groupe, mais le réseau passe**
→ Slew rate du groupe trop rapide + THDi. Mesurer avec
enregistreur, régler le régulateur, élargir la fenêtre de
fréquence d'entrée de l'UPS (menu).

**Cas B — Déclenchement intempestif du DJ d'entrée**
→ Courant d'appel du redresseur au démarrage + courbe DJ trop
sensible. → Courbe D ou DJ à déclenchement retardé, vérifier le
pré-charge du bus DC.

**Cas C — Autonomie divisée par 2 en 1 an (local climatisé)**
→ Un string sur deux déconnecté (DJ vibré) ! → Thermographie,
vérification des DJ DC, resserrage.

**Cas D — Alarmes fantômes la nuit**
→ Micro-coupures du réseau (non vues en journée). → Enregistreur
1 semaine, signalement distributeur, seuils adaptés.

**Cas E — Échauffement d'un câble de sortie (120 kVA)**
→ Cosse mal sertie. → Thermographie trimestrielle, ressertissage,
contrôle au couple. (Un câble qui chauffe = incendie en puissance.)

---

## 63. Négocier avec les fournisseurs (chef de service)

- **Batteries** : prix dégressif par palette, garantie écrite
  (conditions de température !), date de fabrication < 3 mois.
- **Pièces** : contrat cadre avec remise, délai garanti (4 semaines
  max), pièces critiques en dépôt local.
- **Constructeur** : mise en service incluse dès 60 kVA, formation
  équipe incluse, support téléphonique 24/7 négocié.
- Toujours **2 fournisseurs** minimum (rupture, prix).

---

## 64. Index rapide

- **VFI** : double conversion, 0 ms | **kW = kVA × PF**
- **Charge ≤ 80 %** | **Floating 13,5–13,6 V/bloc**
- **Écart > 0,3 V** = suspect | **Impédance +30 %** = remplacer
- **Local 20–25 °C** | **Terre < 5 Ω** | **Parafoudre T1+T2**
- **Groupe = 1,5–2× UPS** | **Test groupe mensuel en charge**
- **Bypass = sans protection** | **Bus DC : attendre + vérifier**
- **Condos 5 ans** | **Ventilos 4 ans** | **Thermographie trimestrielle**
- **Dossier d'exploitation** par site | **Tout noté, tout signé**

---

*Fin du guide — 1600+ lignes. Chef de service systèmes & énergies :
votre crédibilité se joue sur **l'anticipation**. Des batteries
testées, un groupe qui démarre, un dossier à jour — et aucune
surprise. Bon courage, Zelef.*
---

## 65. Tableaux de dimensionnement complets par puissance

> Ordres de grandeur (400 V tri, cuivre, à valider par note de calcul).

### 65.1 Protections et câbles

| UPS | I approx. | DJ entrée | DJ sortie | Câble AC | DJ batterie (384 V) |
|---|---|---|---|---|---|
| 10 kVA | 16 A | 25 A | 25 A | 4 mm² | 32 A DC |
| 20 kVA | 32 A | 40 A | 40 A | 10 mm² | 63 A DC |
| 40 kVA | 64 A | 80 A | 80 A | 25 mm² | 125 A DC |
| 60 kVA | 96 A | 125 A | 125 A | 35 mm² | 160 A DC |
| 120 kVA | 192 A | 250 A | 250 A | 95 mm² | 250 A DC |

### 65.2 Batteries types (15 min d'autonomie)

| UPS | Charge type | Bus DC | Config indicative |
|---|---|---|---|
| 10 kVA | 7 kW | 384 V | 32 blocs 12 V / 9 Ah (1 string) |
| 20 kVA | 14 kW | 384 V | 32 blocs 12 V / 18 Ah (1 string) |
| 40 kVA | 29 kW | 384–480 V | 32–40 blocs / 24 Ah (1–2 strings) |
| 60 kVA | 43 kW | 480 V | 40 blocs / 40 Ah (2 strings) |
| 120 kVA | 86 kW | 480 V | 40 blocs / 65 Ah (2–3 strings) |

### 65.3 Groupe et climatisation

| UPS | Groupe conseillé | Clim à prévoir |
|---|---|---|
| 10 kVA | 15–20 kVA | 3–5 kW frigo |
| 20 kVA | 30–40 kVA | 5–8 kW frigo |
| 40 kVA | 60–80 kVA | 10–15 kW frigo (N+1) |
| 60 kVA | 90–120 kVA | 15–20 kW frigo (N+1) |
| 120 kVA | 180–250 kVA | 30–40 kW frigo (N+1) |

---

## 66. Procédure d'arrêt d'urgence

```
1. Bouton d'arrêt d'urgence (coup de poing) si danger immédiat
   → coupe TOUT (UPS + bypass)
2. Sinon, arrêt contrôlé :
   a. Prévenir les utilisateurs (arrêt des serveurs)
   b. Arrêt programmé des serveurs (scénario §42)
   c. Forcer le bypass → bypass de maintenance
   d. Ouvrir DJ sortie → DJ entrée → DJ batterie
3. Ne JAMAIS rouvrir sans diagnostic de la cause
4. Consignation avant toute intervention (§39)
```
- Le bouton d'arrêt d'urgence doit être **signalé et accessible**,
  testé annuellement.

---

## 67. Maintenance du groupe électrogène (le jumeau de l'UPS)

```
HEBDOMADAIRE : niveau huile, niveau liquide de refroidissement,
               niveau carburant, voyants, fuites
MENSUEL : démarrage EN CHARGE 30 min (> 30 % de charge),
          vérifier tension/fréquence/stabilité
TRIMESTRIEL : filtres (visuel), courroies, batterie de démarrage
              (tension + chargeur), test ATS complet
ANNUEL : vidange + filtres (huile, gasoil, air),
         liquide de refroidissement, test 2 h en charge,
         contrôle des sécurités (pression huile, température)
```
- Batterie de démarrage du groupe : la panne n°1 des groupes →
  chargeur d'entretien + remplacement tous les 3 ans.
- Carburant : rotation tous les 6 mois, additif anti-bactérien
  (le gasoil stocké se contamine en climat chaud).

---

## 68. Maintenance de la climatisation (la gardienne des batteries)

```
MENSUEL : filtres (nettoyage/remplacement), température de consigne
TRIMESTRIEL : condenseur (dépoussiérage), écoulement condensats,
              test de bascule N+1 (arrêter une clim → l'autre prend tout)
ANNUEL : contrôle frigoriste (pression, fluide), nettoyage complet
```
- Alarme haute température **supervisée** (SMS) : une clim en panne
  un vendredi soir = batteries à 40 °C tout le week-end.

---

## 69. Tableau de bord mensuel du service (modèle)

```
PARC UPS — ___/___
Nb UPS suivis : ___ | Disponibilité : ___ % | Incidents : ___
MTTR : ___ h | Alarmes traitées : ___ | En retard : ___
BATTERIES : âge moyen ___ ans | tests OK ___/___ | remplacements prévus : ___
GROUPES : tests mensuels OK ___/___ | carburant mini : ___
CLIM : alarmes ___ | bascules N+1 testées : ___
CONTRATS : renouvellements à 90 j : ___
BUDGET : dépensé ___ % | investissements à prévoir : ___
RISQUES : ___
ACTIONS : ___
```

---

## 70. Fiche site — modèle (une par site)

```
SITE : ___  Responsable : ___  Tél : ___
UPS : marque ___ modèle ___ n° série ___  Puissance ___ kVA
Mise en service : ___  Charge : ___ %  Firmware : ___
BATTERIES : marque ___ ___ blocs ___ V ___ Ah ___ strings
Mise en service : ___  Prochain remplacement estimé : ___
GROUPE : marque ___ puissance ___ kVA  Dernier test en charge : ___
CLIM : marque ___ puissance ___  N+1 : oui/non
SUPERVISION : NMC ___ (IP ___)  SNMP : oui/non  Alertes SMS : oui/non
CONTRAT : niveau ___  Échéance : ___
DOSSIER D'EXPLOITATION : ☐ à jour le ___
RISQUES NOTÉS : ___
```

---

*1600+ lignes atteintes. Chef de service : **votre parc est aussi
bon que votre pire checklist.** Appliquez-les toutes, et dormez
tranquille.*
---

## 71. Les 10 règles d'or (à afficher dans chaque local UPS)

```
1. NE JAMAIS intervenir sans consignation complète.
2. Le bus DC reste chargé après coupure : attendre + vérifier.
3. En bypass, la charge n'est PLUS protégée.
4. Une batterie non testée est une batterie morte.
5. Local batteries : 20–25 °C, ventilé, pas de flamme.
6. Outils isolés, pas de bijoux, une seule main sur le DC.
7. Groupe : test mensuel EN CHARGE, pas à vide.
8. Tout écart de tension batterie > 0,3 V = suspect.
9. Thermographie trimestrielle : le chaud annonce la panne.
10. Tout noté, tout signé, tout classé au dossier d'exploitation.
```

---

## 72. Fiche de mise en service — modèle à signer

```
MISE EN SERVICE UPS
Date : ___  Site : ___  Techniciens : ___
UPS : ___ (n° série : ___)  Puissance : ___ kVA
□ Local conforme (clim, ventilation, extincteur, accès)
□ Câblage vérifié (serrage, repérage, terre = ___ Ω)
□ Protections conformes à la note de calcul
□ Batteries : ___ blocs, tension totale = ___ V, polarité OK
□ Séquence de démarrage OK, 400 V / 50 Hz en sortie
□ Test coupure : ___ min d'autonomie mesurée
□ Test bypass statique + maintenance : OK
□ NMC configurée (IP : ___), alertes testées : OK
□ Firmware : ___
□ Dossier d'exploitation remis : oui
□ Formation exploitants : oui (___ personnes)
Réserves : ___
Signatures : installateur ___ / client ___
```

---

*Fin du guide — 1600+ lignes. À toi, chef : **dimensionne juste,
installe propre, teste tout, note tout.** Ton app sera redoutable
sur les UPS avec ce contenu.*
---

## 73. Annexe — conversions et formules utiles

```
P (kW) = S (kVA) × cos φ
I (A) = S (VA) / (√3 × 400)            [triphasé 400 V]
I (A) = P (W) / 230                    [monophasé 230 V]
Chute de tension : ΔU = √3 × I × L × (R cos φ + X sin φ)
Autonomie : Ah = P×t / (Vbus × ηond × ηbat)
Pertes UPS : P_thermique = P_charge × (1 − η)
Arrhenius : vie batterie / 2 tous les +10 °C
√3 ≈ 1,732
```

---

*1600+ lignes. Guide terminé.*

---

## 74. Pour aller plus loin (prochaines étapes possibles)

- Fiches détaillées par modèle Easy UPS (3S / 3M) avec menus pas à pas.
- Dictionnaire d'alarmes par marque (Schneider, Eaton, Vertiv).
- Modèle de contrat de maintenance UPS (clauses, SLA, bordereau de prix).
- Formation « habilitation électrique » : plan de cours complet.
- Guide groupes électrogènes approfondi (moteur, alternateur, ATS).
